<b>CTF:</b>
n00b16ctf

<b>Challenge:</b>
bin-medium

<b>Classification:</b>
Reversing

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/binmedium/binmed.png">

Pretty clear-cut challenge. We just need to dive into this binary and get the flag. If we run the program, it prompts us for a special passphrase, which we don't have on hand. But perhaps we don't need it!

Looking at the output from objdump we can see a lot of stuff going on in the memory, which is putting together what will ultimately be the flag. If you want to take the long way, you can step through the assembler code and construct the flag yourself OR you can simply use GDB and debug to bypass the passphrase check and get the flag.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/binmedium/memory-binmed.png">

Set a breakpoint in the program right before the first printf() at 0x80485d4 and run the program.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/binmedium/breakpoint1.png">
<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/binmedium/break2.png">

Once the breakpoint is hit, we have freedom to change where our instruction pointer is set to, so let's point to it directly to the beginning of where our flag is constructed.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/binmedium/binmed_flag.png">

Redirecting the instruction pointer to bypass the necessity of a passphrase, it builds our flag for us and prints it out! 

Changing where the instruction pointer goes by modifying it with "set $eip=< memory address >" is quite useful when debugging.


