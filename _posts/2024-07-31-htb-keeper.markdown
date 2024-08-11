---
layout: post
title:  "[HTB] Keeper"
date:   2024-07-31
categories: CTF
---

As usual let us start with our nmap scan

---


---

Seeing that only port 22 and 80 are open, this means we would highly likely be exploring the web application. Upon visiting the web application, we see the page below, which means we need to add keeper.htb and tickets.keeper.htb to our /etc/hosts

![alt text](/assets/uploads/htb-keeper/image.png)

So now visiting tickets.keeper.htb, we see that we have a login page for Request Tracker. Googling on possible default credentials and we see that the default credentials are root:password

![alt text](/assets/uploads/htb-keeper/image-1.png)

Now looking around the request tracker web application, we see something about a "keepass program"

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

Looking further, we see some putty ssh key for the root user, maybe this would work... So let us just copy this and save as a ppk file and use configure putty to connect to the machine using the ssh key

![](/assets/uploads/htb-keeper/image-9.png)

![alt text](/assets/uploads/htb-keeper/image-10.png)

![alt text](/assets/uploads/htb-keeper/image-11.png)

![alt text](/assets/uploads/htb-keeper/image-12.png)

And after logging in, we get the root flag

![alt text](/assets/uploads/htb-keeper/image-13.png)