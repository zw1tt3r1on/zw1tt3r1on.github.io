---
layout: post
title:  "[HTB] Sunday"
date:   2024-06-18
categories: CTF
---

As usual let us start with our nmap scan

---

─# nmap 10.10.10.76 -p- -A
Starting Nmap 7.94 ( https://nmap.org ) at 2024-08-09 01:28 EDT
Nmap scan report for 10.10.10.76
Host is up (0.12s latency).

PORT      STATE SERVICE  VERSION
79/tcp    open  finger?
|_finger: No one logged on\x0D
| fingerprint-strings: 
|   GenericLines: 
|     No one logged on
|   GetRequest: 
|     Login Name TTY Idle When Where
|     HTTP/1.0 ???
|   HTTPOptions: 
|     Login Name TTY Idle When Where
|     HTTP/1.0 ???
|     OPTIONS ???
|   Help: 
|     Login Name TTY Idle When Where
|     HELP ???
|   RTSPRequest: 
|     Login Name TTY Idle When Where
|     OPTIONS ???
|     RTSP/1.0 ???
|   SSLSessionReq, TerminalServerCookie: 
|_    Login Name TTY Idle When Where
111/tcp   open  rpcbind  2-4 (RPC #100000)
515/tcp   open  printer?
| fingerprint-strings: 
|   TerminalServerCookie: 
|_    Unable to get printer : No destinations added.
22022/tcp open  ssh      OpenSSH 8.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 aa:00:94:32:18:60:a4:93:3b:87:a4:b6:f8:02:68:0e (RSA)
|_  256 da:2a:6c:fa:6b:b1:ea:16:1d:a6:54:a1:0b:2b:ee:48 (ED25519)
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port79-TCP:V=7.94%I=7%D=8/9%Time=66B5A8EA%P=x86_64-pc-linux-gnu%r(Gener
SF:icLines,12,"No\x20one\x20logged\x20on\r\n")%r(GetRequest,93,"Login\x20\
SF:x20\x20\x20\x20\x20\x20Name\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20TTY\x20\x20\x20\x20\x20\x20\x20\x20\x20Idle\x20\x20\x20
SF:\x20When\x20\x20\x20\x20Where\r\n/\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\?\?\?\r\nGET\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\?\?\?
SF:\r\nHTTP/1\.0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\?
SF:\?\?\r\n")%r(Help,5D,"Login\x20\x20\x20\x20\x20\x20\x20Name\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20TTY\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20Idle\x20\x20\x20\x20When\x20\x20\x20\x20Where\r\nHELP\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:?\?\?\r\n")%r(HTTPOptions,93,"Login\x20\x20\x20\x20\x20\x20\x20Name\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20TTY\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20Idle\x20\x20\x20\x20When\x20\x20\x20\x20Where\r
SF:\n/\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\?\?\?\r\nHTTP/1\.0\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\?\?\?\r\nOPTIONS\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\?\?\?\r\n")%r(RTSPRequest,93,"Login\x20\x20\
SF:x20\x20\x20\x20\x20Name\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20TTY\x20\x20\x20\x20\x20\x20\x20\x20\x20Idle\x20\x20\x20\x20
SF:When\x20\x20\x20\x20Where\r\n/\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\?\?\?\r\nOPTIONS\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\?\?\?\r\nRTSP/1\.0\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\?\?\?\r\n")%r(SSL
SF:SessionReq,5D,"Login\x20\x20\x20\x20\x20\x20\x20Name\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20TTY\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20Idle\x20\x20\x20\x20When\x20\x20\x20\x20Where\r\n\x16\x03\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\?\?\?\r\n")%r(TerminalServerCookie,5D,"Login\x20\x20\x20\x20\x20\x
SF:20\x20Name\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20T
SF:TY\x20\x20\x20\x20\x20\x20\x20\x20\x20Idle\x20\x20\x20\x20When\x20\x20\
SF:x20\x20Where\r\n\x03\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\?\?\?\r\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port515-TCP:V=7.94%I=7%D=8/9%Time=66B5A8EA%P=x86_64-pc-linux-gnu%r(GetR
SF:equest,1,"\x01")%r(RTSPRequest,1,"\x01")%r(SSLSessionReq,1,"\x01")%r(Te
SF:rminalServerCookie,2E,"Unable\x20to\x20get\x20printer\x20:\x20No\x20des
SF:tinations\x20added\.")%r(Kerberos,1,"\x01");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Oracle Solaris 10 (94%), Oracle Solaris 11 (94%), Oracle Solaris 11 or OpenIndiana (93%), Sun Solaris 11.3 (93%), OpenIndiana oi_147 - oi_148 (92%), Nexenta OS 3.0 - 3.1.2 (OpenSolaris snv_130 - snv_134f) (92%), Sun Solaris 11 (snv_151a) or OpenIndiana oi_147 (92%), Sun Solaris 11 (snv_151a) or OpenIndiana oi_147 - oi_151a (92%), Sun OpenSolaris snv_129 (91%), Solaris 12 (90%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 111/tcp)
HOP RTT       ADDRESS
1   116.76 ms 10.10.14.1
2   113.49 ms 10.10.10.76

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 167.95 seconds


---

Seeing that there is a finger service available, we might be able to use this to check for valid usernames within the system. For this, I would be using metasploit. Initially, there is nothing interesting on the list of usernames that metasploit showed 

![](/assets/uploads/htb-sunday/image.png)

But changing the wordlist to a bigger and better wordlist revealed 2 more usernames that is particularly interseting: sammy and sunny

![](/assets/uploads/htb-sunday/image-1.png)

Knowing that sammy and sunny are valid usernames, we could now try to bruteforce their passwords via ssh using hydra. With this, we get the password for sunny but not for sammy.

![](/assets/uploads/htb-sunday/image-2.png)

![](/assets/uploads/htb-sunday/image-3.png)

Now we can ssh into sunny and investigate the machine further. Looking at /backups we see that there are 2 files. 

![](</assets/uploads/htb-sunday/2024-08-09 12_30_33.png>)

Copying these files and trying to crack them using john reveals the password for sammy. In this case, we can also realize that it might have been possible to just directly bruteforce the password of sammy with a really good wordlist as it was not very complex  

![](</assets/uploads/htb-sunday/2024-08-09 12_30_27.png>)

So now logging in as sammy gives us the user flag

![](</assets/uploads/htb-sunday/2024-08-09 12_30_53.png>)

Looking for privilege escalation paths via sudo -l, we see that we have ALL which means we can just sudo su and we get can get root

![](</assets/uploads/htb-sunday/2024-08-09 12_31_32.png>)

Another path is to use the wget binary that we can run as root. There is a known local privilege escalation path from gtfobins

![](/assets/uploads/htb-sunday/image-4.png)

![](</assets/uploads/htb-sunday/2024-08-09 12_32_59.png>)

