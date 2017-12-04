<b>CTF:</b>
Shellter Hacking Express #18

<b>Challenge:</b>
Authenticate Me

<b>Category:</b>
Rev/Pwn

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/authenticate.png">

This was a pretty neat little rev/pwn challenge. To start, we need to SSH in with the credentials provided.
Exploring the directory we are dropped into there is a flag.txt file, but unfortunately we are not root so we can't open it.
However, there is also a program in the file called 'auth'.

Running it, it asks for identification and then 2 keys. Experimenting a bit, it will only accept integer values as input for the keys, so we know that they need to be numerical or else it won't be accepted.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/authtest.png">

Time to disassemble this program and find out what's going on.
Using GDB, we can see there are multiple functions, and the one we will zero in on is 'auth'. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/authdisassabled.png">

Looking at the disassembly, after both scanf functions are called, there are two compare instructions which are used to authenticate the keys input. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/keys_shx18.png">

The first value we see being compared is 0x1e1d0, which is 123344 in decimal, and the second value, 0x539, is 1337 in decimal. If these values match with what is stored in memory from the scanf, /bin/cat flag.txt is ran, which will give us the flag. So these are clearly the keys we need to input to authenticate! Well, if only it were that easy... Unfortunately just entering them when running the program does not work, so it looks like we will have to find another way to get them in. 

Luckily, I found that we are able to perform a buffer overflow with the scanf used in the 'greeting' function which is used to grab our identity. I set breakpoints at the two scanfs in the 'auth' function for my initial test, and then input 100 A's to see what would happen.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/overflowconfirmed.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/anotherscanf.png">

At both scanfs in auth, the EAX value which is used to store our key input was overwritten with As. After some trial and error, the offset to reach the first scanf was after 72 bytes and this was confirmed by using some Bs and Cs! However, when looking at this, the keys were being pulled by scanf in a different order than how they were being compared in the assembly.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/overflowed1.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/overflowed2.png">

This is important so we can order our keys properly in the exploit code. Modifying the test python code and subbing in the key values for the Bs and C, and packing them so they are not just passed as decimal, we get the following:

python -c 'from struct import pack; print "A" * 72 + pack("<I", 1337) + pack("<I", 123344)

Testing this in GDB with the same scanf breakpoints, we can confirm that they are making it to EAX properly for each scanf.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/confirmation1.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/confirmation2.png">

Time to run it outside of GDB on auth!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/SHX18/authflag.png">

And there is our flag!






