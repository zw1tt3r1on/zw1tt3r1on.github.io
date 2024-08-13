---
layout: post
title:  "[HTB] Cap"
date:   2024-08-02
categories: CTF
---

As usual let us start with our nmap scan

---

└─# nmap 10.10.10.143    
Starting Nmap 7.94 ( https://nmap.org ) at 2024-08-13 05:26 EDT
Nmap scan report for 10.10.10.143
Host is up (0.27s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

---

We see that we only have port 22 and 80, so this means we would be mainly attacking the web application in port 80. So running feroxbuster here to enumerate directories show us that there A LOT of directory listings available which is a bit suspicious for me.

![alt text](/assets/uploads/htb-jarvis/image.png)

I also saw that there is phpmyadmin page on /phpmyadmin. However I had no luck on logging in via bruteforce 

![alt text](/assets/uploads/htb-jarvis/image-1.png)

With this I proceeded to then downloaded all of the files that have directory listing and tried to look for maybe a username and password, however I also did not find anything. 

Now looking at the web application, this seems to be a normal looking application but one page stands out /room.php?cod=ID

![alt text](/assets/uploads/htb-jarvis/image-2.png)

Upon passing this to sqlmap, we see that this is vulnerable to sql injection and we can easily get a shell via --os-shell

![alt text](/assets/uploads/htb-jarvis/image-3.png)

![alt text](/assets/uploads/htb-jarvis/image-4.png)

With this, we can just establish a reverse shell using python

![alt text](/assets/uploads/htb-jarvis/image-5.png)

Since we are only www-data here, we need to look for a way to gain access to a user. Looking at sudo privileges, we can run the simpler.py script as pepper

![alt text](/assets/uploads/htb-jarvis/image-6.png)

Reading the script, there seems to be a ping functionality which is very susceptible to command injection if we are able to bypass the forbidden characters checking

---

def exec_ping():
    forbidden = ['&', ';', '-', '`', '||', '|']
    command = input('Enter an IP: ')
    for i in forbidden:
        if i in command:
            print('Got you')
            exit()
    os.system('ping ' + command)

---

However if we look at this, it is very difficult to bypass the checking of forbidden characters since all shell commands usually have these characters. An easier way is to just leverage the $() function of the shell. We can just execute $(bash somescript.sh) wherein somescript is a reverse shell script. Upon executing this, we get a reverse shell as pepper

![alt text](/assets/uploads/htb-jarvis/image-7.png)

![alt text](/assets/uploads/htb-jarvis/image-8.png)

Now that we are the user pepper let us get the user flag and look if we can escalate to root.

![alt text](/assets/uploads/htb-jarvis/image-9.png)

Enumerating the system, we see that there is a suid on the systemctl binary which we can leverage to get root

![alt text](/assets/uploads/htb-jarvis/image-11.png)

![alt text](/assets/uploads/htb-jarvis/image-10.png)