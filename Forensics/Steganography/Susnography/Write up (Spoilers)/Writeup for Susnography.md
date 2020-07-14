# Susnography
**Autumn 2020 Semester Long CTF**

**Flag:** CSEC{57up1d_5u5p1c10u5_fl4nd3r5} 
**Category:** Steganography

**Description:** Don’t mind flanders he has nothing to hide… nothing at all, nothing at all

**Hint(s):** 

1. Download “suspicious.gif”
2. Upon viewing suspicious.gif in a browser/software it becomes apparent there is a URL at the end of the GIF. {IMAGE}
3. Visit https://github.com/vipyne/giffy
a.	Download the master branch as .zip and extract it
i.	Compile in Kali via console / Terminal
--	cd to directory of extracted .zip
--	Type: ‘make giffy’ as directed by `https://github.com/vipyne/giffy` (To compile) {IMAGE}

4. Follow the instructions for decoding on the github
a. In terminal
i. `./giffy.exe d suspicious.gif`
ii.	Or if the suspicious.gif is in the downloads folder still
--	`./giffy.exe ~/Downloads/suspicious.gif`{IMAGE}
iii.	Or
--	`./giffy.exe [directory of whereever else you put the GIF]`

b.	User is shown: `PFRP{57hc1q_5h5c1p10h5_sy4aq3e5}` Hint:The flag has ROTen
i.	Simple Rot 13 Cipher: Use a website to reverse the ROT13
--	http://decode.org/ or https://gchq.github.io/CyberChef/{IMAGE} {IMAGE} 

5.	Receive Flag “CSEC{57up1d_5u5p1c10u5_fl4nd3r5}”

Which gives the flag: cysoc{suh_dUd3_suH}