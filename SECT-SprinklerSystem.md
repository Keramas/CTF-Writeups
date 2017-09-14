<b>CTF:</b>
SEC-T CTF 2017

<b>Challenge:</b>
Sprinkler System

<b>Classification:</b>
Web

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SprinklerSystem/sprinkleschallenge.png">

The challenge starts by pointing to a URL, and when we check it out the following site comes up:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SprinklerSystem/sprikles.png">

Not too much here at all... But out of habit when nothing else is shown, check robots.txt! 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SprinklerSystem/sprinkles_robots.png">

Well this is interesting and worth checking out, so after putting "/cgi-bin/test-cgi" into the browser, it gives us a test script report, which should be exploitable.

<a href="http://insecure.org/sploits/test-cgi.html">This site</a> served as a good reference for how to exploit this.

Inputting "/cgi-bin/test-cgi?*" shows the scripts that are present for this site, and we can see something sprinkler-related.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SprinklerSystem/enablesprinker.png">

Let's see if we can execute this by appending it to our URL...

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SprinklerSystem/sprinke_flag.png">

Sprinkler systems activated and we get our flag!

