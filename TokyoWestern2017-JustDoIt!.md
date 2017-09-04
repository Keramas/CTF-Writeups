<b>CTF:</b>
Tokyo Westerns CTF 3rd 2017

<b>Challenge Name:</b>
Just Do It!

<b>Category:</b>
PWN

<b>Useful tools:</b>
GDB, strace, Hopper

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doit1.png">

Clicking on the challenge we have an address and port to connect to with netcat. They also give the binary to look at as well.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doitstart.png">

Loading up a terminal and connecting with netcat we are prompted for a password.
So, as a first step, let's run strings on the binary file.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doitstrings.png">

P@SSW0RD looks like it could be promising (or just a troll), but unfortunately this doesn't work.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doitpwfail.png">

Time for another test to see what the program is doing! Loading it up with strace we can see two things:
1. The program is reading a file called flag.txt (we will likely need to make use of this...)
2. When inputting a password an automatic linebreak ('\n') is added to the end. Weird...

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/stracedoit.png">

Diverting from this for a moment, since we are asked for user input might as well try to see if there is the possibility of an overflow. And, what do you know, we get a segmentation fault. Looks like this is just going to be a standard buffer overflow attack. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doitdumped.png">

Running this in GDB and disassembling it, we find that the buffer is quite small and we can overwrite the pointer with an address of our choosing with 20 bytes of nops (or garbage) and then our 4 byte hex address. 

Before we write up our exploit, I want to look at one interesting thing: P@SSW0RD was in fact the password, but it is a red herring and even though we get a success message, this is not what we need to do. Since there is a linebreak we need to remove it and that can be done by adding a NULL character after our password and passing it to the program.

<code>
python -c "print 'P@SSW0RD' + '\0'" > doitexploit
</code>
<br>

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doitpw.png">

Okay, back to the actual exploit. What is the address we want to use to jump to? Well, we know that flag.txt is being read by the program, so let's try and zero in on that a bit.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doit2.png">

Looking at the address right before fopen is called to read the file, we see an address getting pushed onto the stack.
Examining this we get the following:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doit3.png">

This can also be verified with Hopper:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doit33.png">

Seems like this is what we want, so let's create our line of code to pass to the program via netcat (remember: use little endian for these addresses!):

<code>
python -c "print '\x90' * 20 + '\x80\xa0\x04\x08'" > doitexploit
</code>
<br>

(Note: I just used garbage A's instead of nops for my actual execution)

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/JustDoIt/doit4.png">

Annnnnd boom, there is our flag!






