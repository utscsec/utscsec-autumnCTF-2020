# DO YOUR FLY UP
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{cl13nt_c4nt_b3_trust3d}
**Category:** Challenges by Sam

**Description:** The start of do your fly up, you are given a .jpg file which doesn’t open.

**Hint(s):** 

1. Running the file command on the given file actually shows that it is a .zip file. So renaming the file and unzipping it we are given another zip file. We then run the unzip command to unzip these files, until we get to ponocihio.tar.gz where we use tar to unzip the file. 
2. Then again unzip the files until we get flag.txt
3. The screenshot below shows all of the commands. {IMAGE}
4. Which gives the flag: cysoc{z1pp1ty_z1p_5ip}
