# SHIFTY SAUCEY
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{b1ts_4nd_p1ec3s} 
**Category:** Challenges by Sam

**Description:** The second episode of the saucy trilogy. You need to put in the correct flag to the box and it would tell you if it was correct. Essentially for this challenge you again needed to view the source of the page. 
**Hint(s):** 

1. But right clicking was disabled so you needed to use the short cut in Firefox (Ctrl + U, Command + U) and see every time you hit the Check button it would run the btoa function on the inputted value and compare it against the value: `Y3lzb2N7YjF0c180bmRfcDFlYzNzfQ==` {IMAGE}
2. Googling what the btoa() function does in javascript reveals that it is a base64 encoded. So to get the flag we just need to base64 decode the given value. {IMAGE}
3. This gives the flag:  cysoc{b1ts_4nd_p1ec3s}

You can also double check this on the website.