# UTS CSEC CTF FLAG WRITEUP
## Title: Secret Pipes
__Date made:__ 01/04/2020

__Used in what CSEC CTF:__ Autumn 2020 Semester Long CTF

__Made by:__ @Conletz

__Flag:__ CSEC{53cr37_5qu1rr3l_7h1n65}

__Difficulty:__ Easy

__Category:__ General/Linux

__Description:__
My friends messed with my computer when I went to the bathroom and now I can’t find anything!!! All my files are hidden… can you find my files for me?
(Flag format is: CSEC{FLAGGYFLAGFLAGFLAGGED})

__Hint(s):__
- Your list may be hiding something from your search
- How can you narrow down the search? GRAB what you are looking for.

***

__Writeup:__
1. File is ‘hidden’ so run `ls -a`
2. Read the .secret file
3. Run a find command to find all the other hidden files --
4. There’s a lot of these files that will take a long time to go through one by one
5. Pipe the find command into grep and search for the flag pattern
6. `Find [basedirectory of the challenge] -type f -iname ".*" | xargs grep "CSEC"`
7. Open the file isitthis34/ormaybehere39/whatabouthere14/.squirrel9345 to find the flag

***

__Comments:__
- There are some hints encoded in base64, hex, rot13, base32 etc in the files
- Lots of false flags CTF{} FTC{} UTS{}
- I totally didn’t delete this entire challenge by accident and definitely didn’t have to trudge through all raw memory to get everything back
- There are 64000 secret files in this challenge, don't try to brute force it ;)
