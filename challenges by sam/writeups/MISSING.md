# MISSING
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{g1t_1n1t}
**Category:** Challenges by Sam

**Description:** For this challenge the user is presented with a file folder with a web page that plays the popular 2048 game, although this has nothing to do with the challenge!

**Hint(s):** 

1. Viewing the hidden files in the root of the directory we will see that this folder is being tracked by git as the .git folder is present. {IMAGE}
2. Viewing the commit’s of the folder shows that the developer has only 2 commits, the intial commit and then adding the 2048 game. {IMAGE}
***BUT!***
3. Viewing all the commits using - `git log –reflog`
4. Shows more commits than the first command initially showed. Git reflog is a record of all commits that are or were referenced in your repo at any time. 
5. The interesting commit is the “hide flag” commit which contains the flag! {IMAGE} {IMAGE}
5. Hard resetting to that commit gives us the flag: cysoc{g1t_1n1t}