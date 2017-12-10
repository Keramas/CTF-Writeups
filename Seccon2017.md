<b>CTF:</b>
SECCON 2017

<b>Challenge:</b>
Run Me!
<b>Category:</b>
Programming

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/runme.png">

If you immediately recognize this script as something that is familiar to you, you are right!
It's a script for getting the nth number of the Fibonacci sequence (with some other stuff for our flag). One problem though--the number we need for the flag is SUPER big and it would take waaaay too long for your computer to finish running this. 

While it's possible to optimize with memoize perhaps, luckily there are precomputed numbers already available on the net. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/largefib.png">

So here is the number we need. Looking at the script though we only need the first 32 digits, so I sloppily copied a bunch of numbers, and then threw it into a script to get the flag.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/runmemodded.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/runmeflag.png">

====================================

<b>Challenge:</b>
Putchar Music
<b>Category:</b>
Programming

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/putcharmusic_challenge.png">

Using the challenge title as a hint, I was mind-blown to discover Algorithmic Music, which takes single-line C programs to generate cool tunes! 

The first step is to compile the code, and then use that file along with Sox to generate a .wav file that we can listen to. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/putcharmusic.png">

Opening the wav file, it plays the iconic Star Wars theme song, which allows us to construct our flag: 
SECCON{STAR_WARS}

====================================

<b>Challenge:</b>
JPEG file
<b>Category:</b>
binary

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/jpegfile_challenge.png">

This challenge gives us a broken JPEG that does not display a proper image, and it's our job to fix it to reveal the flag. 

After playing detective in hexedit, I couldn't find anything wrong with the header, but using jpeginfo it guided me to the spot in the hex that needed to be edited.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/jpgbroken.png">

Conducting some tests by changing the 0xFC value I found in hexedit, which is right where all the actual image data begins, I confirmed that it was indeed the location it was indicating, and turning this into 0x00 allowed the image to display properly.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/fixedjpg.png">

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/seccon2017/brokenjpg_flag.png">
