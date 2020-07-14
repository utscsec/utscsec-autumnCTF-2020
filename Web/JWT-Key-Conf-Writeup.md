# Jason Gets Confused
**Autumn 2020 Semester Long CTF**
**Date made:** 3/30/2020

**Flag:** CSEC{J50n_@lw4yz_g3Tz_C0nFu53d_0n_t3h_w3B}
**Category:** Web

**Description:** Jason always gets locked out of his house because he gets his keys confused

**Hint(s):** JWT

1. Look at robots.txt to find a key.pem file
2. Find username and password located in the source code of the login page
3. Login as that user
4. See JWT and that it’s signed with RS256, probably using key.pem as the public key
5. Decode JWT and see user role field
6. Use a JWT key confusion vulnerability to tamper with and sign your own token with HS256 instead of RS256, with the public key being used as the HMAC secret.
7. Apply the token and get flag

