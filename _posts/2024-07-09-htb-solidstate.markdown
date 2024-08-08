---
layout: post
title:  "[HTB] SolidState"
date:   2024-07-09
categories: CTF
---

Let us start with our nmap scan

---

└─# nmap 10.10.10.51 -p- -A
Starting Nmap 7.94 ( https://nmap.org ) at 2024-08-07 03:16 EDT
Nmap scan report for 10.10.10.51
Host is up (0.071s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4p1 Debian 10+deb9u1 (protocol 2.0)
| ssh-hostkey: 
|   2048 77:00:84:f5:78:b9:c7:d3:54:cf:71:2e:0d:52:6d:8b (RSA)
|   256 78:b8:3a:f6:60:19:06:91:f5:53:92:1d:3f:48:ed:53 (ECDSA)
|_  256 e4:45:e9:ed:07:4d:73:69:43:5a:12:70:9d:c4:af:76 (ED25519)
25/tcp   open  smtp    JAMES smtpd 2.3.2
|_smtp-commands: solidstate Hello nmap.scanme.org (10.10.14.12 [10.10.14.12])
80/tcp   open  http    Apache httpd 2.4.25 ((Debian))
|_http-title: Home - Solid State Security
|_http-server-header: Apache/2.4.25 (Debian)
110/tcp  open  pop3    JAMES pop3d 2.3.2
4555/tcp open  rsip?
| fingerprint-strings: 
|   GenericLines: 
|     JAMES Remote Administration Tool 2.3.2
|     Please enter your login and password
|     Login id:
|     Password:
|     Login failed for 
|_    Login id:
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port4555-TCP:V=7.94%I=7%D=8/7%Time=66B31F49%P=x86_64-pc-linux-gnu%r(Gen
SF:ericLines,7C,"JAMES\x20Remote\x20Administration\x20Tool\x202\.3\.2\nPle
SF:ase\x20enter\x20your\x20login\x20and\x20password\nLogin\x20id:\nPasswor
SF:d:\nLogin\x20failed\x20for\x20\nLogin\x20id:\n");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.13 (96%), Linux 3.2 - 4.9 (96%), Linux 4.8 (96%), Linux 4.9 (95%), Linux 3.16 (95%), Linux 3.12 (95%), Linux 3.18 (95%), Linux 3.8 - 3.11 (95%), ASUS RT-N56U WAP (Linux 3.4) (95%), Linux 4.4 (95%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: solidstate; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   60.07 ms 10.10.14.1
2   60.14 ms 10.10.10.51

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 241.70 seconds

---

Searching for vulnerabilities on JAMES remote administration tool 2.3.2, we find that there is an authenticated RCE issue as detailed on https://www.exploit-db.com/exploits/35513

Running exploit will give us a shell the next time someone logs in. So now let us find for a way to be able to login. We can change the password of mindy and login to her account. Upon logging in to mindy's account, we see that there are credentials sent in clear text

![](</assets/uploads/htb-solidstate/2024-08-07 19_14_49.png>)

So now if we use these credentials to login using ssh, we get a shell now.

![](</assets/uploads/htb-solidstate/2024-08-07 19_15_02.png>)

Looking around the file system, we see that there is a file in the /opt folder call tmp.py and this tmp.py is world writable and is being ran on a routinely basis. So what we do is we edit the contents of the tmp.py file to give us a reverse shell as root.

![](</assets/uploads/htb-solidstate/2024-08-07 19_30_22.png>)

![](</assets/uploads/htb-solidstate/2024-08-07 19_30_14.png>)
