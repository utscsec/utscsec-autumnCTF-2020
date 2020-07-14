# STRINGERS
**Autumn 2020 Semester Long CTF**

**Flag:** cysoc{suh_dUd3_suH} 
**Category:** Challenges by Sam

**Description:** This was a very simple binary challenge were you had to find a flag in the binary file. 

**Hint(s):** 

1. Running the binary on the command line using “./stringers” simply prints out a string. But running the strings command on the binary reveals the flag.
{IMAGE}
2. The source code of the compiled file was: 
'#include <stdio.h>'

'void main(void) {'

'    char flag[64] = "cysoc{suh_dUd3_suH}";'

'    printf("Thanks for opening my file :)");'

'}'

5. Which gives the flag: cysoc{suh_dUd3_suH}