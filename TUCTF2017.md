VM CHALLENGES

<b>Challenge:</b>
Gateway
<b>Category:</b>
VM
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gateway_challenge.png">

This CTF provided a VM which encompassed a number of different challenges, the first part of which is
to actually access it. When booting up the VM, we find that the drive it's accessing is 
encrypted and you need a passkey to decrypt it. Luckily there is an unencrypted drive that is
accessible. 

To access this, we need to add a live CD to the VM so that we can boot off that instead.
I chose to use Kali Linux for this. Once we boot into Kali, we can access the unencrypted 
drive which is labeled "Boot". 

Right away there is a file that stands out: "usefultool.exe"
Running this we discover that it is just a program that ROT13's whatever string you provide, so
this is a pretty good indicator that our flag will be a ROT13 that needs to be decoded.

Looking around the drive more, nothing of use was really found, so I took a closer look at the
.exe. Running strings on it I found something interesting.

However, this turned out to only be part of the flag. Looking closer at the strings output,
it seems that the exe has been packed with UPX. Using UPX to unpack it, we can finally get the full
output of all the strings, as well as our full ROT13 flag.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gateway_packed.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gateway_fullflagrot13.png">

Decoding it we get our flag as well as the password to unencrypt the drive.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gateway_flagdecoded.png">


<b>Challenge:</b>
Leap of Faith
<b>Category:</b>
VM

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/leapoffaith_challenge.png">

This challenge utilized the same exe from the previous challenge. I overthought this one a lot, but looking at all the strings in GDB by accessing the function 'randomPaddingFunction' that I was told to ignore, I finally realized that the first letter of each string gave the flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/leapoffaith_flag.png">


<b>Challenge:</b>
Worth a Thousand Words
<b>Category:</b>
VM

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/worthathousandword_challenge.png">

In the photos folder on the VM there are three different jpgs. Each one contains a part of the flag.
Using strings on 1.jpg we get the first part which is just TUCTF{.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/1000words_1jpg.png">

2.jpg has a hidden file inside of it and we can extract it with binwalk. Dumping the contents we get our second part of the flag: Devils

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/1000words_part2.png">

3.jpg is corrupted and needs to be fixed before we can open it. This took a bit of research, but once I found which portions of the IHDR were wrong, I was able to edit it in hexeditor and make it so we can see the image. Running pngcheck helped confirm when it was actually uncorrupted.

Broke:
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/100wordshexedit.png">

Edited:
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/1000words_hexedited3.png">

Opening the image, there is a barcode on the screen in the picture, and scanning this gives us the final part of the flag: InThePixels}

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/1000words_uncorrupt.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/1000words_barcode.png">

<b>Challenge:</b>
Euchlid Go Away
<b>Category:</b>
VM

This was one of my favorite challenges of the CTF. Remoting in with netcat, we are dropped into a text-based game. There is a lot of messaging about a rumored hidden room, so I guessed this is exactly what needed to be found. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/euclid_start.png">

There were a couple tricks to this: first, we need to be admin, which I guessed would give us some extra powers. Messaging an admin in the game reveals their username, so I took that and logged back in under that admin name. 

Next, I sent a message that I had bugged leveling and increased my level past the normal user cap of 255 and make myself 256. 
Upon doing so we are identified as admin and we have some newfound powers!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/euclid_escalate.png">

One of which is the ability to teleport and a map of all the rooms. 

Teleporting to the admin room first, there was a function to leave messages for the devs. I immediately thought to input "cat flag.txt". After this, I teleported to the dev room and it gave me the option to read the message I left for myself. This executed the action I left in the message, and I got the flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/euclid_flagget!.png">


=======================================


WEB CHALLENGES

<b>Challenge:</b>
High Source
<b>Category:</b>
Web

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/tuctf_highsource_challenge.png">

Accessing the web page and immediately looking at the source we get trolled. Nothing here... But what about in the login.js source?

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/highsource_sourcecode.png"> 

Oh hey, a password! Using this to login we then get our flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/highsource_loginJS.png">

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/highsource_flag.png">


<b>Challenge:</b>
Cookie Duty
<b>Category:</b>
Web

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookieduty_challenge.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookieduty_homepage.png">

The first page we land on gives a good hint as to what this will entail with cookies... We aren't admin, but can we become an admin by modifying the cookie? "not_admin" is set to a 1 flag, so if we modify this to a 0 we should be able to become admin.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookieduty_application.png">

Changing the cookie and making a GET request with Burp Suite, we trick it to thinking we are admin and get our flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookieduty_flag.png">


<b>Challenge:</b>
Git Gud
<b>Category:</b>
Web

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gitgud_challenge.png">

Based on the challenge description we know that the site is using Version Control Systems and the title also gives it away that we are looking for something git-related. After researching some stuff about this, it's possible to access a directory at /.git which gives all of the history info, objects, what have you. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gitgudgitdirectory.png">

To make this a bit easier, I just downloaded everything and used comannd line to traverse through the directories. In the master file you can see an update entry for adding a flag, and using the data on the left we can cat the info until we are able to see the past entry and get our flag! 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gitgud_gitupdated.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/gitgud-getflag.png">



<b>Challenge:</b>
Cookie Harrelson
<b>Category:</b>
Web

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookieharrelson_hompage.png">

More cookies! Inspecting the response/request headers upon loading, we can see that there is a cookie being passed.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookieharrelson_startcookie.png">

This coded in base64, and decoding it we get:
cat index.txt

Neat... so what if we just encode something like 'cat flag.txt' in base64 and swap it in? Well, this is what I did, but it wasn't as easy as that. Unfortunately when subbing in something different, the cookie gets modified and it becomes something like this:

cat index.txt #(new command here)

So we are getting filtered and need to bypass it somehow! After a lot of experimentation/trial and error, the trick here is to get a carriage return. Using Burp to decode the original cookie, modify it, and then re-encode it, we get our new cookie.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookies_flagencoded.png">

Now we can put this in the repeater and make our GET request, which gives us the flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/cookies_getflag.png">



<b>Challenge:</b>
iFrame and Shame
<b>Category:</b>
Web

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/iframe_challenge.png">

This challenge took a bit of trial and error to solve. I started by checking for input validation in the search box by adding an " and that seemed to break the format a bit, which kind of tipped me off there may be a possibility for code injection.

After trying different things for a while, the following command worked as a test:
"; echo $(ls) #

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/iframebroke!.png">

Based on this, we just change our command to give us our flag:
"; echo $(cat flag) #

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/iframe_flagget.png">


===================================================


REVERSE CHALLENGES


<b>Challenge:</b>
Funmail
<b>Category:</b>
Rev

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/funmail_challenge.png">

Running the binary, we are prompted for a username and password. We already know the username, so we need to find out the password. This is easy enough because it is hard coded and can be discovered by running strings on the program.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/tuctf_funmail.png">

Now we log in with our credentials and read the email, which gives us our flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/tuctf_funmail2.png">


<b>Challenge:</b>
Funmail 2.0
<b>Category:</b>
Rev

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/funmail2_challenge.png">

For this challenge, we get another hard coded password, but the program doesn't work properly and terminates. What to do?

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/funmail2.png">

Well, we can grab the address for a function called printFlag, and then use GDB to set a breakpoint in main and set the EIP to the address of printFlag. Continuing to run the program gives us our flag.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/funmail2_flag.png">


======================================


PWN CHALLENGE


<b>Challenge:</b>
Vuln Chat
<b>Category:</b>
Pwn 

At first I thought this appeared to be a pretty simple buffer overflow problem, but it was a bit more interesting than that!
Running the program we get prompted for a name and then Djinn enters and asks for proof before he spills the beans with his hot info. 

Exploring with objdump a bit, there is a hidden function 'printFlag', so this is going to be our target address to use to overwrite EIP at some point.

Attempting to input a bunch of A's only went so far and I wasn't able to overwrite EIP. Scanf was limiting my overwrite.

Setting some breakpoints at the printf calls, I took a look at the stack and noticed that the scanf buffer was actually on the stack, and it was possible to overwrite far enough to modify this. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/vulnchat_scanfbuffer.png">

I was able to change the scanf buffer to something bigger (I set it to 64 bytes), and then I was finally able to write far enough to overflow and get a segfault. 

Using some trial and error to get the proper offset to land the printFlag address, I found that an offset of 54 bytes was the ripe spot. 

I wrote up the following exploit in python to be deployed to the server.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/vulnchat_exploitcode.png">

Executing this we get our flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/TUCTF2017/vulnchat_flag.png">












