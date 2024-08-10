---
layout: post
title:  "[HTB] Poison"
date:   2024-04-21
categories: CTF
---

As usual let us start with our nmap scan

---

└─# nmap -vv 10.10.10.84     
Starting Nmap 7.94 ( https://nmap.org ) at 2024-08-08 02:43 EDT
Initiating Ping Scan at 02:43
Scanning 10.10.10.84 [4 ports]
Completed Ping Scan at 02:43, 0.21s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 02:43
Completed Parallel DNS resolution of 1 host. at 02:43, 0.04s elapsed
Initiating SYN Stealth Scan at 02:43
Scanning 10.10.10.84 [1000 ports]
Discovered open port 80/tcp on 10.10.10.84
Discovered open port 22/tcp on 10.10.10.84
Completed SYN Stealth Scan at 02:43, 14.20s elapsed (1000 total ports)
Nmap scan report for 10.10.10.84
Host is up, received timestamp-reply ttl 63 (0.13s latency).
Scanned at 2024-08-08 02:43:45 EDT for 14s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 63
80/tcp open  http    syn-ack ttl 63

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 14.54 seconds
           Raw packets sent: 1250 (54.976KB) | Rcvd: 1142 (46.300KB)

---

We see that there are only 2 open ports, so lets look at HTTP. We see that this a site to test php files.

![](/assets/uploads/htb-poison/image.png)

Investigating the listfiles.php file, we see that there is a file called pwdbackup.txt

![](/assets/uploads/htb-poison/image-1.png)

Looking at this pwdbackup.txt file, this is a suspicious file and we can decode this via base64 and we get "Charix!2#4%6&8(0"

![](/assets/uploads/htb-poison/image-2.png)

Using these credentials to login via ssh for the user "charix", we get our user flag

![](/assets/uploads/htb-poison/image-3.png)

Looking at the current directory, there is a zip file called secret.zip. Getting this to our host machine and cracking this via hashcat doesnt produce any good results. Trying to use the same password "Charix!2#4%6&8(0" worked! So now we just need to know what is this secret file for.

![alt text](/assets/uploads/htb-poison/image-5.png)

Looking at the running processes, we see that there is a vnc process and maybe we can connect to this 

![alt text](/assets/uploads/htb-poison/image-4.png)

![alt text](/assets/uploads/htb-poison/image-6.png)

So we can port forward port 5901 to our machine so that we can connect to vnc

![alt text](/assets/uploads/htb-poison/image-7.png)

Connecting to vnc using the secret file that we got from the zip file and we get root

![](/assets/uploads/htb-poison/image-8.png)

![](/assets/uploads/htb-poison/image-9.png)
