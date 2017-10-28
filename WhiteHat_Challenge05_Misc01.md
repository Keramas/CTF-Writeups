<b>CTF:</b>
WhiteHat Challenge 05

<b>Challenge Name:</b>
Misc01

<b>Category</b>
Misc

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/challengestart.png">

Heading over to the website, we get some directions.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/start.png">

Looking at the image closely, there is definitely some text at the bottom that is not too legible. Sure, we could zoom in, but I want the file on my machine so we can analyze. Unfortunately it wasn't possible to inspect from browser, so curl to the rescue!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/whitehatchallenge1.png">

We get a base64 clue too that the image is the key, so I download the image.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/base64stuff.png">

That is base64 at the bottom there, so decoding it we get the following:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/moreinfo.png">

Not the flag, but a clue! A different format? Perhaps jpg? 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/rabbitholejpg.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/deeper.png">

Hm, nope! I guess it is a .txt file, then!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/rabbithole.png">

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/whitehat_misc01/flag.png">

The flag! :D











