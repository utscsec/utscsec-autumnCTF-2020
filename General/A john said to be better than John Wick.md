# A John said to be better than John Wick
**Autumn 2020 Semester Long CTF**
**Date made:** 29/03/2020

**Flag:** CSEC{!!!!HEYBABE!!!!}
**Difficulty (Out of 10):** 3
**Category:** General/Misc

**Description:** I found this really cool file (link file) but I can’t seem to figure out the password. Can you help me? (Flag format is: CSEC{INSERT PASSWORD HERE})

**Hint(s):** When you see John you will need to play a famous queen song to get his attention.

1. Download zip file
2. Unzip file with the following command - `unzip coolfile.zip`
3. Figure out it is an ssh private key using `file`
4. Use ssh2john.py on the file - `python /usr/share/john/ssh2john.py coolfile > tocrack`
5. Use john to crack the password - `sudo john tocrack --wordlist=/usr/share/wordlists/rockyou.txt`
6. Figure out password is !!!!HEYBABE!!!!
7. Flag is CSEC{!!!!HEYBABE!!!!}
