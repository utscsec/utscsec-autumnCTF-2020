# UTS CSEC CTF FLAG WRITEUP
## __Title:__ Pass your kee
__Date made:__ 09/04/2020
__Used in what CSEC CTF:__ Autumn 2020 Semester Long CTF

__Made by:__ @Conletz

__Flag:__ CSEC{n07_50_53cr37_n0w}

__Difficulty (Out of 10):__ 4
__Category:__ General/Misc

__Description:__
Can you crack the kee?

__Hint(s):__
- When you see John you will need to play a famous queen song to get his attention.

***

__Writeup:__
1. Download file
2. Run file hackerman.dat command
3. Rename the file to hackerman.kdbx
4. `Keepass2john hackerman.kdbx > hashed.kdbx`
5. `John --wordlist=[path to rockyou] hashed.kdbx` - this step may take a while
6. You should get the password yahoosecurity
7. If you’ve forgotten the database password run `john --show hashed.kdbx`
8. Open the kdbx file in keepass on a windows vm using the password found using john
9. Flag is in the flag password entry.
