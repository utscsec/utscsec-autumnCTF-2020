# UTS CSEC CTF FLAG WRITEUP
## Title: No Mercy
__Date made:__ 12/04/2020

__Used in what CSEC CTF:__ Autumn 2020 Semester Long CTF

__Made by:__ @Crem

__Flag:__ CSEC{_sanic_gotta_go_fast_sanic_gotta_go_fast_}

__Difficulty:__ Difficult

__Category:__ General/Linux

__Description:__
This server is being temperamental and kicks me from the server every 30 seconds. I cannot even do anything in 30 seconds, let alone find a flag amongst gibberish. You look smart, can you help me?

Lucky for you I will pass on this hint someone gave me for this challenge:
https://www.youtube.com/watch?v=oHg5SJYRHA0

Nah jokes, I am just preparing you for this box.
This is the actual hint:
https://media.giphy.com/media/yXVO50FJIJMSQ/giphy.gif

`ssh ctf@128.199.239.130 -p 8010`

password is `ctf`

Also I just want to say I am sorry for this.

__Hint(s):__
- NA
- NA

***

__Writeup:__
1. Login into the SSH server
2. Be kicked and hate your life.
3. Look through all the files and realise they look like Base64. If you look at the Base64 for CSEC it is Q1NFQ. However if you recursively grep for this string it doesn't come up.
4. So realise that something else has been done to the base64 maybe reverse the string or rot13. Rot13 is the correct answer.
5. If you rot13 ‘Q1NFQ’, you get D1ASD. If you recursively grep for this string you will find it matches to something. If you rot13 and base64 it you get the flag.

Solutions:
1. `grep -r D1ASD` copy found string, rot13 and base64 it.
2. `for i in $(find . -type f); do cat $i |  tr 'A-Za-z' 'N-ZA-Mn-za-m' | base64 -d | grep CSEC; done`

***

__Comments:__
- This challenge is just plain annoying. There is a cronjob running and approximately every 30 seconds it will kick you from the SSH server, it will also delete all the files and remake them in random locations
- You will need to think for this challenge. It uses very common encodings like base64 and rot13 and once you know this you have the challenge.
- You want to bypass the SSH server shenanigans, just SCP it - Sam
  - This is an alternative solution
