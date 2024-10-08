---
layout: post
title:  "[RootCon] RootCon 18 CTF Pre-Qualifiers"
date:   2024-09-17
categories: CTF
---

We recently took a shot in this year's RootCon 18 CTF Pre-Qualifiers and had a lot of fun solving the challenges. In this post, I’d like to share a brief writeup of the challenges our team was able to solve including the scripts that I have wrote to solve the challenges.

Big shoutout to my friends [jmrcsnchz](https://jmrcsnchz.github.io/) and [damaidec](https://medium.com/@damaidec) for pushing through the CTF and solving these tricky challenges with me!


# Super Secure Static Website

Visiting the ip and port given to us, we see a login page. I initially tried random credentials but nothing worked and just showed 405 method not allowed. So we tried changing the request to GET but still it did not work. 

![alt text](/assets/uploads/rootcon18-ctf-prequals/image.png)

<img src='/assets/uploads/rootcon18-ctf-prequals/image-2.png' width="80%">

Looking also at source, we see that there is a js file that contains a hardcoded password but upon trying this password on the login page, it still did not work. 

<img src='/assets/uploads/rootcon18-ctf-prequals/image-1.png' width="90%">

So investigating more, we discovered that this web application is hosted on AWS, so we focused on that. We queried the version of the objects and then queried the contents of each of the versions and found the flag in one of them. 

---

aws s3api list-object-versions --bucket admin-panel.pwndemanila.ph

aws s3api get-object --bucket admin-panel.pwndemanila.ph --key script.js --version-id 3m838_45RnoGWi9ssn0xV90xdwzU1Zh8 test.js   

---

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-3.png)

# Run

This challenge was a bit confusing for us since it did not give any clear indications for exploitation. It just showed a big maze with the instruction as shown below

---

Reach the flag (F) while avoiding the monster (M).
Use WASD to move (W=up, A=left, S=down, D=right).

@ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ 
@ . . # . . . . . # . . . . . . . . # . . @
@ . . . . . . . . . . . . . . # . . . . . @
@ . . . . . . . . . . # . . # . . . . # . @
@ M . . . # . . . . . . . . # . . . # . . @
@ . . . . . . # . . . . . . . . . . . . . @
@ . . . . . . . . . . . . . . . . . # . . @
@ . . . . . . # # . . . . . . . . . . . . @
@ . . . # . . . . . . . . . . . . . . . . @
@ . # . . . . . . . . . . . . . . . . . . @
@ . . . . . . . . . . . . . . . . . . . . @
@ . . . . . . . . . . # . . . . . . . . . @
@ . . . . . . . . # . . . . . . . . # . . @
@ . . . . . . . . . # . . . . . . . . # # @
@ . . . . . # . . . . . . . # . . . P . . @
@ . . . . . . . . . . . . . . . . . . . . @
@ . . . . . . . . . . . . . # . . . . . . @
@ . . . # . . . . . . . . . . . . . . . . @
@ . # . # . # . . . . . . . . . . . . . . @
@ . . . . # . . . . . . . . . . . . . . . @
@ . . . F . . . . . . . . . . . . . . . . @
@ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ 

Distance to monster: 27
Your move (WASD): ...

---

I am not exactly sure if this is intended or on how exactly this challenge should have been solved, but my teammate just reset the game multiple times to get a favorable starting position and just played the game normally. After your player reaches the flag, the flag was also then shown.  

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-10.png)


# Zippy

After downloading the given file, I tried and extracting the contents of it and it showed another zip file. This indicated that I need to recursively unzip the contents of this file and MAYBE the last file contains the flag. So I developed a bash script to automate this for me as shown below

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-4.png)

So after running it for a while, my script showed that the last file is empty. I initially thought that this challenge might have been broken since there is no flag whatsoever.

<img src='/assets/uploads/rootcon18-ctf-prequals/image-5.png' width="95%">

However, we realized that the names of the folders might be hex encoded since they only contain the characters 0-9 and a-f. So collecting these and passing it through cyberchef and hex decoding it did NOT get us anywhere :(

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-6.png)

It was then further discovered that the output should be first reversed before being hex decoded and we get the flag from there

<img src='/assets/uploads/rootcon18-ctf-prequals/image-7.png' width="90%">

# I Can See The End

This one was a bit tricky since the only thing that was provided to us was a png file so we immediately assumed that this was related to steganography.

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-13.png)

Looking at this png and checking different variations of it did not provide any progress.

<img src='/assets/uploads/rootcon18-ctf-prequals/image-8.png' width="90%">

Later on, we have discovered that there is this tool called zsteg that detects stegano-hidden data specifically in PNG & BMP, which exactly fits our file! Running "zsteg -a -l 0 see.png" command gave us some output that kinda resembles the flag 

<img src='/assets/uploads/rootcon18-ctf-prequals/image-14.png' width="90%">

Trying to decipher/guess this, I have came up with 
- lianl ghaib
- lisn all ghai
- lisanal ghaib
- lisan lghab
- and many more... 

Searching about this thing, I found this https://dune.fandom.com/wiki/Lisan_al_Gaib

So at this point, it was just about finding the right combination of characters and we get the flag: RC18{l1s4n_@L_Ga1b!}  

# Rock Paper Scissors

This was another fun one because this involved winning a rock, paper, and scissors match for 1000 games consecutively and obviously we need to automate to do this again and again.

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-9.png)

Observing how the mechanism of this works, it shows YOU WON if you obviously won against the opponent, and it showed YOU LOSE THIS ROUND when you tie or lose. We can leverage this by just bruteforcing rock, paper, and scissors and see if you win or lose and recording that. So I again made a script for this as shown below

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-11.png)

So running this for a while and getting all the winning moves, we get the flag

![alt text](/assets/uploads/rootcon18-ctf-prequals/image-12.png)


# Beelzebub

This was a very fun challenge since it was a binary file! So let us try and run this and see what we get

Initially it is looking for a shared library called "libcrypto.so.1.1", so we fetch that and install it on our machine

![alt text](/assets/uploads/rootcon18-ctf-prequals/1image.png)

After that, it looks for the right permissions, but we do not know what permissions is it looking for, so we try and reverse engineer this binary using ghidra

<img src='/assets/uploads/rootcon18-ctf-prequals/1image-1.png' width="90%">

We see here that we need to first be running as the user beelzebub with password ILOVEROOTCON

![alt text](/assets/uploads/rootcon18-ctf-prequals/1image-3.png)
<img src='/assets/uploads/rootcon18-ctf-prequals/1image-5.png' width="80%">

Next we see that we need to have the following 
- uid 0x1b39 (6969 in decimal) or else it would say "wrong user ID"
- gid of 0x1a4 (420 in decimal) or else it would say "wrong group ID"
- euid of 0 (on the if statement)

![alt text](/assets/uploads/rootcon18-ctf-prequals/1image-2.png)
![alt text](/assets/uploads/rootcon18-ctf-prequals/1image-6.png)

Next on the list is that the binary should be in the /tmp/i/l/o/v/e/r/o/o/t/c/o/n directory 

![alt text](/assets/uploads/rootcon18-ctf-prequals/1image-4.png)

Now assuming that all the setup was done correctly, running this binary should show us the flag

![alt text](/assets/uploads/rootcon18-ctf-prequals/1image-7.png)