# SERVE MY SAUCE
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{c4n_u_pls_st0p_ste4l1nG_MY_s4u(e}
**Category:** Challenges by Sam

**Description:** The giant finale in the sauce trilogy we have again a saucy webpage. But this time reading the sauce code isn’t going to help us.

**Hint(s):** 

1. Navigating to different pages in the top navbar we notice that there is a page parameter in the URL: {IMAGE}
2. The home page also hints at a Local File Inclusion with 3 sentences all starting with local file and including.
3. Changing the page param to something else results in an empty webpage. {IMAGE}
4. We are also told from the challenge description that the flag is stored in `/flag.txt` on the server. Trying `page=/flag.txt` also results in nothing as this is an absolute path and will not work in the command. So trying putting a relative path in the command with `page=../../../../../../../flag.txt` reveals the flag. {IMAGE}
5. This gives the flag: cysoc{c4n_u_pls_st0p_ste4l1nG_MY_s4u(e}

As this is a command injection it is also possible to perform remote code execution. It is left to the reader to try and see if you can pop a shell on the box ;)

