# UTS CSEC CTF FLAG WRITEUP

## Title: Empty Nester

__Date made:__ 09/04/2020

__Used in what CSEC CTF:__ Autumn

__Made by:__ @Conletz

__Flag:__ CSEC{wh475_up_d0c}

__Difficulty:__ 3/10

__Category:__ General/Linux?

__Description:__
I can’t reach my file, can you get it for me?

__Hint(s):__ It's just like a present

***

__Writeup:__
1. Run a file command on tricky.txt
2. The file will be identified as one of 4 types
  - Zip
    1. Rename file to tricky.zip using ```move [file] [dest file name]```
    2. ```Unzip tricky.zip```
  -Bzip2
    1. Rename file to tricky.bz2
    2. ```Bzip2 -d tricky.bz2```
  - Gzip
    1. Rename file to tricky.gz
    2. ```Gunzip tricky.gz```
  - Tar
    1. Rename file to tricky.tar.gz
    2. ```Tar -xvf tricky.tar.gz```

__Order:__
1. Zip
2. Gzip
3. Tar -czf
4. Tar -czf
5. Tar -czf
6. Bzip2
7. Gzip
8. Tar -czf
9. Bzip2
10. Bzip2
11. Bzip2
12. Bzip2
