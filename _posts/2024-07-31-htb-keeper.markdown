---
layout: post
title:  "[HTB] Keeper"
date:   2024-07-31
categories: CTF
---

As usual let us start with our nmap scan

---

└─# nmap -vv 10.10.11.227
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-11 10:22 EDT
Initiating Ping Scan at 10:22
Scanning 10.10.11.227 [4 ports]
Completed Ping Scan at 10:22, 0.07s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 10:22
Scanning keeper.htb (10.10.11.227) [1000 ports]
Discovered open port 22/tcp on 10.10.11.227
Discovered open port 80/tcp on 10.10.11.227
Completed SYN Stealth Scan at 10:22, 0.77s elapsed (1000 total ports)
Nmap scan report for keeper.htb (10.10.11.227)
Host is up, received reset ttl 63 (0.049s latency).
Scanned at 2024-08-11 10:22:32 EDT for 1s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 63
80/tcp open  http    syn-ack ttl 63

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.96 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.048KB)
                                                                                                                     

---

Seeing that only port 22 and 80 are open, this means we would highly likely be exploring the web application. Upon visiting the web application, we see the page below, which means we need to add keeper.htb and tickets.keeper.htb to our /etc/hosts

![alt text](/assets/uploads/htb-keeper/image.png)

So now visiting tickets.keeper.htb, we see that we have a login page for Request Tracker. Googling on possible default credentials and we see that the default credentials are root:password

![alt text](/assets/uploads/htb-keeper/image-9.png)

![alt text](/assets/uploads/htb-keeper/image-1.png)

After logging in to the web application and we look around and we see something about a "keepass program"

![alt text](/assets/uploads/htb-keeper/image-5.png)

And looking around more, we see the credentials for lnorgaard 

![alt text](/assets/uploads/htb-keeper/image-2.png)

Trying these credentials on ssh gives us access to lnorgaard's account and the user flag

![alt text](/assets/uploads/htb-keeper/image-3.png)

Looking for privilege escalation pathways, we immediately see that we have a suspicious zip file on lnorgaard's Desktop. So let us download this onto our machine so that we can investigate this zip file further

Upon extracting the contents of the zip file, we see a .kdbx and a .dmp file. Using file to check the .kdbx file reveals to us that this is a keepass password database file. 

![alt text](/assets/uploads/htb-keeper/image-4.png)

Upon searching about this, there seems to be a way to be able to retrieve the masterpassword.
Exploit for windows: https://github.com/vdohney/keepass-password-dumper
Exploit for linux: https://github.com/dawnl3ss/CVE-2023-32784

For this, I would be using the linux version and we somewhat retrieve something but this doesnt make any sense

![alt text](/assets/uploads/htb-keeper/image-6.png)

So upon googling these words, it seems to be a food called "rødgrød med fløde" but our terminal cannot display it to us properly due to the special characters

![alt text](/assets/uploads/htb-keeper/image-7.png)

So let us now open this file using keepass and enter "rødgrød med fløde" as the password. Here we see that we have the credentials for the root user. However upon trying to use "F4><3K0nd!" to login to the root user via ssh, it doesnt work. 

![alt text](/assets/uploads/htb-keeper/image-8.png)

Looking further, we see some putty ssh key for the root user, maybe this would work... So let us just copy this and save as a ppk file and configure putty to connect to the machine using the ssh key

![alt text](/assets/uploads/htb-keeper/image-10.png)

![alt text](/assets/uploads/htb-keeper/image-11.png)

![alt text](/assets/uploads/htb-keeper/image-12.png)

And after logging in, we get the root flag

![alt text](/assets/uploads/htb-keeper/image-13.png)