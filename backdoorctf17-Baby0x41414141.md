<b>CTF</b>:
backdoorctf 2017

<b>Challenge</b>:
baby-0x41414141

<b>Category</b>:
PWN

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/baby41414141.png">

NCing into the challenge, I threw a couple of different tests at it, and quickly discovered we have a format string exploit on our hands here. By passing in "%x" as an argument, you can see that it starts to leak memory addresses, so adding a bunch of them after a short egg string "AAAA", we find that our string is in fact placed on the stack. Furthermore, it's directly accessible in the 10th spot in, which you can verify by passing the program "AAAA.%10$x" (I just like to add the period in there to make it easier to read so the memory addresses get segmented better.).

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/stringexploitforsure.png">

Okay, so what to do now with this? Well, we need to explore the program a bit more in depth to find out if there are any functions we can call that will give us the flag. 

Using Hopper we can find something pretty interesting:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/itsaflagfunction.png">

We have a function that calls system and sends the command "cat flag.txt", so this is definitely what we want to use in our exploit. The address we want to execute is 0x804870b, but how to do that?

I'm not sure whether there was an easier way to accomplish this, but I used two short writes to change the address of the global offset table. (More on this soon...) 

Disassembling main in GDB, we see the final instruction is "exit@plt", and disassembling this address we find that there is indeed a jump to the global offset table at 0x804a034. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/got.png">

Let's try to simulate changing the address of the GOT to the flag() address in GDB.
We set a breakpoint in our program right at the call to printf and another breakpoint at the exit@plt call, and then run the program. Once it hits the first breakpoint we are going to set the GOT to our new address, and then confirm it by examining the address. 

<img src=https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/breakpoints.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/gotoverwritetest.png">

Now that it's changed, let's continue running our program. Once it hits the call to exit@plt it should be redirected to our flag() function. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/gotoverwritesuccess.png">

And yep, it did. You can see it is trying to execute /bin/dash.

Now we just have to do the actual overwriting which is possible with two short writes using the information we gathered from our format string exploit and some trial and error. If you aren't familiar with what a short write is, what we are essentially going to do is break up our 4 byte address into 2 bytes and 2 bytes, and change them individually by writing our new address values via a format string exploit. 

There is a great video about this process by <a href="https://www.youtube.com/watch?v=t1LH9D5cuK4">Live Overflow</a> and it's very similar to a challenge from Protostar. He will be able to explain things much better than me, for sure.

I whipped up the following exploit with Python:

<code>
import struct

flag = 0x804870b
exit = 0x804a034

payload = ""
payload += struct.pack("I", exit)
payload += struct.pack("I", exit+2)
payload += "%10$34492x "	
payload += "%10$n"
payload += "%33016x "
payload += "%11$n"

print payload
</code>

After exporting this into a file, we are ready to deliver the payload to our challenge.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/startexploit.png">

The program starts printing out a bunch of stuff, but ultimately it terminates giving us our flag!

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/baby0x41414141/flaggedexploit.png">
