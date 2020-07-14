# SIMPSONS
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{https://www.youtube.com/watch?v=fvtQYsckLxk}
**Category:** Challenges by Sam

**Description:** To do the simpsons steg, there was two parts to the solution. Firstly finding the correct password key and then decoding it using steghide, or an online decoder.

**Hint(s):** 

1. So the first part you can use photoshop to change the contrast or something, but the easy way is to download stegsolve.jar and load up the file. Then you should see the phrase when you hit blue plane 5. {IMAGE}
2. It’s quite hard to see, but it is visible underneath the sign as the phrase says “my_biggest_yeah_boi_ever”.
3. Using this phrase we can then use steghide on kali with that as the password to extract the flag. `steghide extract -sf lisasteg.jpg` {IMAGE}
4. And then put in the password given above.s
5. This writes the flag to a file containing the flag which is: cysoc{https://www.youtube.com/watch?v=fvtQYsckLxk}