# LET ME OUT
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{w3_w1ll_r0ck_u_:)}
**Category:** Challenges by Sam

**Description:** This challenge shows how to utlise brute forcing. We are given an encrypted zip file that needs a password to get the contents and no other information.

**Hint(s):** 

1. Using a command on kali called fcrackzip we are able to brute force the zip file. Using the rockyou.txt file we are able get the password.
2. The command to brute force it is - `fcrackzip -D -p /usr/share/wordlists/rockyou.txt -u jail.zip`. {IMAGE}
3. The switches in this attack mean 
- -D: dictionary attack
- -p: password list
- -u: try to unzip the file 
4. After this runs we are given the password of “ love” one of the last entries in rockyou.txt (sorry not sorry). 
5. Unzipping the file we get the flag: cysoc{w3_w1ll_r0ck_u_:)}
