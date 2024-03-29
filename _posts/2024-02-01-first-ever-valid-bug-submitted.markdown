---
layout: post
title:  "[Bug Bounty] My First Ever Valid Bug Submitted!"
date:   2024-02-01
categories: Bug-Bounty
---

# > Quick Background Story

As I have detailed in my about page, I have been a penetration tester for quite some time now and I have been signed up to HackerOne ever since 2022, but I have not touched it ever since I have signed up. This is mainly because I had this perspective that hunting bugs on public programs such as the ones listed on HackerOne, Bugcrowd, etc. is very very VERY difficult to do. To be honest, up to this date I still see it like that since there are thousands and thousands of people hunting on the same public targets and are deparately looking for get paid for their submissions. But last October 2023, I have stumbled upon my old HackerOne account and decided to start and try looking for bugs.

# > Introduction

In this blog post (as the title says already), I just want to share about my first ever valid bug submitted as this was a very happy and hype moment for me as I thought that I would not be able to find a valid bug in a few (million) years. So, my first valid bug submitted is nothing very exciting like an RCE or getting paid $10,000. It was just an open redirect on a VDP (so I didn't get paid even 1 cent).

# > What is Open Redirect?

An Open Redirect is a type of web application security issue. It occurs when a web application or server uses an unvalidated user-submitted link to redirect the user to a given website or page. Basically from the words itself, you can REDIRECT a user to another website without any validation (OPENLY). 

<center><img src="/assets/uploads/bb-my-first-ever-valid-bug-submitted/open redirect.png" width="70%"></center>

So with open redirect, an attacker can send phishing emails/messages to users that contains a link with the company's domain. However when the victim visits the link, he/she will then be redirected from the company' domain to the attacker’s site that may contain a form to retrieve user information such as usernames and passwords.

# > Bug Details

So how did I find this bug? Well, I first performed subdomain enumeration on the target and found just one subdomain. For the purpose of this post, lets just call it "https://randomsubdomain.REDACTED.com/". Initially this subdomain was not very exciting as there was nothing popping on the page that looks interesting to me and there were not a lot of parameters to try and input anything. However when I appended a URL to this subdomain, I got redirected to that URL!

For example, when I appended "google.com" (https://randomsubdomain.REDACTED.com/google.com), I get redirected to https://google.com/! 

Finding this, I immediately opened my HackerOne account all excited and I just made a very very rough report and immediately clicked the submit button! After a few hours, I have received an email notification from a HackerOne triager that my report was successfully validated and the status of the report was changed to "Pending program review". 

<center><img src="/assets/uploads/bb-my-first-ever-valid-bug-submitted/pending-program-review-comment.png"></center>

After about a week, I received another email notification from the program's cybersecurity team and they have changed the status of my report was to triaged.

<center><img src="/assets/uploads/bb-my-first-ever-valid-bug-submitted/triage-comment.png"></center>

As of today, my report is still tagged as triaged as I think that there are other reports that they are prioritizing to fix. 

<center><img src="/assets/uploads/bb-my-first-ever-valid-bug-submitted/hackerone-status-report.png"></center>