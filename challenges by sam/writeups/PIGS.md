# PHP WEAK TYPE
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{H1_c4n_1_g3t_a_burg3r} 
**Category:** Challenges by Sam

**Description:** This challenge tests how php responds to comparing equality of two datas.

**Hint(s):** 

1. The challenge starts with a simple password input box, when inspecting the source of the page we are given the following message.
`<!-- @dave we heard reports there is something wrong with this webpage can u check it out at index.php.bak -->`
`Navigating to index.php.bak we are able to view the php source of the webpage` 
{IMAGE}
2. First it checks whether the inputted password is different from QNKCDZO. If it is, then it hashes both the user inputted passwords and QNKCDZO if they match, the user is granted the flag.
3. The trick here is to see that the hashes are compared using a weak type ie the == operator. If we check what the hash of hard coded password is we find out it starts with 0e and when php compares two strings starting with 0e they are always a match. So googling a hash that starts with `0e` we find: `240610708`
4. Inputting this as the password we get the flag! {IMAGE}
5. Flag: cysoc{H1_c4n_1_g3t_a_burg3r} 