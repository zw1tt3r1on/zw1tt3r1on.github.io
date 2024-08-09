---
layout: post
title:  "[HTB] Pilgrimage"
date:   2024-07-18
categories: CTF
---

As usual let us start with our nmap scan

---

└─# nmap 10.10.11.219 -vv                 
Starting Nmap 7.94 ( https://nmap.org ) at 2024-08-09 01:55 EDT
Initiating Ping Scan at 01:55
Scanning 10.10.11.219 [4 ports]
Completed Ping Scan at 01:55, 0.18s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 01:55
Completed Parallel DNS resolution of 1 host. at 01:55, 0.04s elapsed
Initiating SYN Stealth Scan at 01:55
Scanning 10.10.11.219 [1000 ports]
Discovered open port 22/tcp on 10.10.11.219
Discovered open port 80/tcp on 10.10.11.219
Completed SYN Stealth Scan at 01:55, 1.32s elapsed (1000 total ports)
Nmap scan report for 10.10.11.219
Host is up, received echo-reply ttl 63 (0.077s latency).
Scanned at 2024-08-09 01:55:03 EDT for 1s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 63
80/tcp open  http    syn-ack ttl 63

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.63 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.036KB)
                                                                      

---

Visiting the web application, we see that there is a file upload functionality which got my senses tingling since there might be a file upload RCE vulnerability. 

![alt text](/assets/uploads/htb-pilgrimage/image-1.png)


However upon trying to upload a php web or reverse shell, and trying to bypass the file upload functioanlity using different kinds of encoding or file type manipulation, I still did not get any shell because it is being converted to a .jpeg image somehow.  

With this, I go back to the drawing board and fuzzed the directories of the web application and we see that there is a .git directory

![alt text](/assets/uploads/htb-pilgrimage/image.png)

Dumping this git repository and investigating its contents, we see that there is a magick binary that is probably being used to process the images 

![](/assets/uploads/htb-pilgrimage/image-2.png)

Searching for vulnerabilities for ImageMagick 7.1.0-49 beta, we see that there is a known arbitrary file read issue that we can leverage to try and read files within the system.

https://www.exploit-db.com/exploits/51261
https://github.com/voidz0r/CVE-2022-44268
https://github.com/Sybil-Scan/imagemagick-lfi-poc

Using this exploit, we generate an image to read the /etc/passwd and upload it to the machine. 

![](/assets/uploads/htb-pilgrimage/image-3.png)

Now let us download the uploaded file and use the command "identify -verbose filename.jpeg" 

![alt text](/assets/uploads/htb-pilgrimage/image-4.png)

![alt text](/assets/uploads/htb-pilgrimage/image-5.png)

![alt text](/assets/uploads/htb-pilgrimage/image-6.png)

We see that the raw data type contains the contents of /etc/passwd when decoded from hex

![alt text](/assets/uploads/htb-pilgrimage/image-7.png)

Now, okay we have an arbitrary file read, so what? What can we do now? Investigating the dashboard.php file, it is connecting to a sqlite database and querying users. So this means that this sqlite database contents the username and passwords for users within the system. So maybe we can retrieve this sqlite database and look at its contents using our arbitrary file read

![alt text](/assets/uploads/htb-pilgrimage/image-8.png)

Upon dumping this sqlite database, we now obtain the credentials for emily with password "abigchonkyboi123". We can now use this for ssh and retrieve the user flag

![alt text](/assets/uploads/htb-pilgrimage/image-9.png)

Looking for privilege escalation pathways, we see that there is a not normal running script called malwarescan.sh

![alt text](/assets/uploads/htb-pilgrimage/image-10.png)

Looking at this script in more detail we see that it uses inotifywait to contantly look at the /var/www/pilgrimage.htb/shrunk/ directory for new files and when there is a new file, binwalk is used to extract any binary data and store it in the binout variable. 

![alt text](/assets/uploads/htb-pilgrimage/image-11.png)

Googling on what is binwalk, binwalk is basically a software to analyze, reverse engineering, and extracting firmware images. Searching whether binwalk has any vulnereabilities lead us to https://www.exploit-db.com/exploits/51249. So let us use this script to get an exploit png file

![alt text](/assets/uploads/htb-pilgrimage/image-12.png)

Now let us setup a listener on port 11111 and upload this exploit png file to /var/www/pilgrimage.htb/shrunk/ via anyway that we want. In this case, I just hosted a python http server and wget the file onto the machine, but you can also just upload it via the web application and we get root on our nc listener!

![alt text](/assets/uploads/htb-pilgrimage/image-13.png)

![alt text](/assets/uploads/htb-pilgrimage/image-14.png)
