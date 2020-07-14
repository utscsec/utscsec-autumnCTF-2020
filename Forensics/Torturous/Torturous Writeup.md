# Susnography
**Autumn 2020 Semester Long CTF**

**Flag:** CSEC{you_have_beaten_the_cyclops}
**Category:** Forensics
**Author:** Crem
**Description:** Odysseus went on a torturous journey in the book ‘The Odyessey’. You will probably have to go on a torturous journey to get this flag too.

**Hint(s):** 

1.	Download “Odyessey.txt”
2.	Look through text file and somewhere in the middle of the text file, there is a huge string. You have to identify that it is a base64 string by the equal sign at the end.
3.	If you decode it, it look like this: {IMAGE}
4.	It shows that it is actually a PDF document. If you go to this website that can convert Base64 to PDF. https://base64.guru/converter/decode/pdf
5.	When base64 it says “good work now what type of file is this?” If you research it is a PDF so you put the PDF magic bytes in. Rebase64 it. :) 
6.	Paste the base64 into the website and click convert.
{IMAGE}
7.	You can either choose to download it or view it on the website. However, the PDF looks blank and 17 pages. However, if you press Ctrl + A or highlight a bunch of stuff there is actually white text which is the flag.
8.	Get flag which is “CSEC{you_have_beaten_the_cyclops}” and receive your points.
