CTF:
Can-CWIC CTF

Challenge:
Rev Me Easy

Classification:
Rev

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/Revmeeasy/revmeeasytask.png">

Downloading the source code and running objdump on it, we can see the following output:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/Revmeeasy/xorasmobj1.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/Revmeeasy/xorasmobj2.png">

Pretty simple to see what's going on here: Hex values are being moved into the edx register and then xor'd with eax, which we can see from the first instruction is 0x10. So let's collect all of these edx values, xor them with 0x10, and see if we get anything interesting to work with.

To do this, we can create a simple python script: 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/Revmeeasy/revmepythonsolution.png">

Running this we get the following:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/Revmeeasy/revmeFLAG.png">

And that's all she wrote, folks--our flag! Reversing this was indeed easy!



