# CONFIDENT SECURITY
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{cl13nt_c4nt_b3_trust3d} 
**Category:** Challenges by Sam

**Description:** We are presented with a simple login page where the user must put in a password to access the flag. 

**Hint(s):** 

1. The hash of the password needed is given to the user as the password is so secure (this means that pre computed hashes will not work). {IMAGE}
2. Looking at the source of the web page we can see a hidden field which contains the hash of the password which is calculated whenever the value changes.{IMAGE}
3. This leads to the notion that the hash is sent over the wire as it is hashed client side rather than on the server. So looking at the network requests we can confirm this. {IMAGE}
4. So resending the request with the given hash, instead of the calculated hash gives the required flag.{IMAGE}
5. Which results in response: {IMAGE}
6. Which gives us the flag: cysoc{cl13nt_c4nt_b3_trust3d} 
