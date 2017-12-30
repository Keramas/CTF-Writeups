<b>CTF</b>: 34C3 Junior CTF 2017

<b>Challenge</b>:
SPI

<b>Category</b>:
Misc

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/SPI_Challenge.png">

We are given a sound file to listen to, which is a recording of various values. 
Writing them all down gives us the following:

76 83 48 116 76 105 52 103 
76 83 48 116 76 83 52 103 
76 105 48 117 76 83 65 116 
76 83 48 116 76 105 65 116 
76 105 48 117 76 83 52 103 
76 83 48 116 76 83 48 103 
76 83 52 117 76 83 65 117 
76 83 52 117 73 67 48 117 
76 83 52 116 76 105 65 116 
76 83 48 117 73 67 48 116 
76 83 48 116 73 67 48 117 
76 83 52 116 76 105 65 116 
76 83 48 116 76 83 65 116 
76 105 52 116 73 67 52 116 
76 105 52 78 67 103 61 61 

These are decimal values, so converting to ascii we are given a base64 encoding:

LS0tLi4gLS0tLS4gLi0uLSAtLS0tLiAtLi0uLS4gLS0tLS0gLS4uLSAuLS4uIC0uLS4tLiAtLS0uIC0tLS0tIC0uLS4tLiAtLS0tLSAtLi4tIC4tLi4NCg==

Converting the base64 back to ascii gives us some morse code:

---.. ----. .-.- ----. -.-.-. ----- -..- .-.. -.-.-. ---. ----- -.-.-. ----- -..- .-..
 
I spent a bit trying to decipher this because it is not just a simple case of decoding it as is. I even reversed the whole code, but that didn't work either, but then I decided to swap all the dashes for dots and all the dots for dashes and got the flag!

...-- ....- -.-. ....- .-.-.- ..... .--. -.-- .-.-.- ...- ..... .-.-.- ..... .--. -.--
                                            

Flag:
	
34C4.5PY.V5.5PY


===================================

<b>Challenge</b>: 
Digital Billboard

<b>Category</b>:
PWN

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/billboard_challenge.png">

This program allows us to set text displayed on a billboard, but when exploring the menu options there is a devmode that is locked to the regular user. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/billboard.png">

Exploring the source code that they give us, this devmode allows us to execute the priviledged function "shell" which gives us access to /bin/bash--so this is our goal here.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/billboard_code.png">

Luckily there is an easy way to enable devmode. The set_text function is using the strcpy function, which we can exploit. 
The struct for billboard has an array for text with a buffer of 256 bytes, which is immediately followed by the flag for devmode. As it stands, devmode is set to 0, but if we write 257 bytes and make the final byte a 1, we can enable devmode.

Inputting our string we are able to access devmode and use the shell command which gives us an interactive bash shell.
Checking the files in the directory, we find the flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/billboard2.png">


===================================

<b>Challenge</b>:
Babybash

<b>Category</b>:
Misc

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/babybash_Challenge.png">

For this challenge we are dropped into a restricted bash shell, but we need to execute the "get_flag" program. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/babybashshell.png">

Luckily we are able to use capital letters and creativity to bypass this. 

They throw a bit of a twist into the mix when executing the program for the first time, and it requires an additional argument,so we modify our command a bit and get the flag upon properly execution.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/babybash_answer.png">

===================================

<b>Challenge</b>:
ARM1

<b>Category</b>:
Rev

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/arm1_challenge.png">

Simply running strings on the ARM binary will reveal the flag (the other ARM-related challenges were not this simple...)

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/34C3-CTF/arm1_flag.png">



