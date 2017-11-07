<b>CTF Name:</b>
Shellter SHX#17

<b>Category</b>
Forensics

<b>Challenge #1: Recover</b>

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/forensics_shellter.png">

Downloading the challenge file, we can just use strings on it and grep for 'password'.
This reveals most of the flag, but using grep again for the content inside the brackets uncovers the entire flag. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/shellterflag.png">


<b>Challenge #2: os.environ.get </b>

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/environget.png">

I actually tried to mount and explore this drive image, but it wasn't leading anywhere, so back to strings!
I ran strings on the whole drive and exported it into a .txt file, and then started to search for various keywords.
Since we know the issue is with os.environ.get, we can assume it has to do with a problem with something related to "PATH=".
Searching for this as a keyword, we can find a string of hex characters with a tiny hint that says "HEX2ASCII".

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/pathflagged.png">

So, converting from hex to ascii we get our flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/hextoasciiflag.png">

<b>Challenge #3: Suspicious Behavior</b>

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/suspiciousbehavior.png">

This challenge uses the same drive image as the prior challenge, and I haphazardly came upon this while searching for some keywords. Using "OS=" as a keyword, I found a suspicious string of what looked to me like hexidecimal characters, and sure enough, when converting them into ascii, we got our flag locating the malware!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/malwarecoded.png">

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/ShellterSHX-17/flagget.png">
