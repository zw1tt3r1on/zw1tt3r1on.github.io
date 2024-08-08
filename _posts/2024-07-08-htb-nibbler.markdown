---
layout: post
title:  "[HTB] Nibbler"
date:   2024-07-08
categories: CTF
---

Let us start with our nmap scan

---

└─# nmap 10.10.10.75 
Starting Nmap 7.94 ( https://nmap.org ) at 2024-08-07 02:52 EDT
Nmap scan report for 10.10.10.75
Host is up (0.058s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 1.92 seconds


---

We can see that we have port 22 and 80 only. Since ssh is often not vulnerable. We will be investigating the web application hosted on port 80.

Visiting the web application and looking at the view source, we see that there is a directory called /nibbleblog
![](/assets/uploads/htb-nibbles/image.png)

Upon visiting that, we see that this is a nibble blog web application and it has an admin panel on /nibbleblog/admin.php

![](/assets/uploads/htb-nibbles/image-1.png)

Testing default weak credentials for this, we discover that the username:password is "admin:nibbles"

Upon logging on the web application, we discover that this is Nibbleblog version 4.0.3

![](/assets/uploads/htb-nibbles/image-2.png)

Searching for vulnerabilities on this, we see that there is an RCE via unrestricted file upload on the "My image" plugin. 

With this, let us just upload a php reverse shell and visit the path /nibbleblog/content/private/plugins/my_image/image.php to get our initial access

![](/assets/uploads/htb-nibbles/image-3.png)

![](/assets/uploads/htb-nibbles/image-4.png)

Looking for privilege escalation possibilities, we see that we have sudo privileges on a file called monitor.sh and it should be on /home/nibbler/personal/stuff/monitor.sh

With this, we can just overwrite the contents of the monitor.sh file and run the monitor.sh file as root to get root access 

![](/assets/uploads/htb-nibbles/image-5.png)

![](/assets/uploads/htb-nibbles/image-6.png)

# > Pwned!

![](/assets/uploads/htb-nibbles/image-7.png)
