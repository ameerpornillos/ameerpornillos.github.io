---
layout: post
title: W34kn3ss Level 1 (VulnHub) - Complete Walkthrough and Guide
subtitle: Here is a complete walkthrough and tutorial on how to hack and penetrate W34kn3ss Level 1 (W34kn3ss 1) of VulnHub.
excerpt: Here is a complete walkthrough and tutorial on how to hack and penetrate W34kn3ss Level 1 (W34kn3ss 1) of VulnHub.
comments: false
thumbnail-img: https://i.imgur.com/NXvgvbN.png
categories:
- WalkThroughs 
tags:
- vulnhub
---

Here is a complete walkthrough and tutorial on how to hack and penetrate W34kn3ss Level 1 (W34kn3ss: 1) of VulnHub.

![](https://i.imgur.com/NXvgvbN.png){: .mx-auto.d-block :}

## **W34kn3ss Level 1 Description**

> The matrix is controlling this machine, neo is trying to escape from it and take back the control on it , your goal is to help neo to gain access as a "root" to this machine , through this machine you will need to perform a hard enumration on the target and understand what is the main idea of it , and exploit every possible "weakness" that you can found , also you will be facing some upnormal behaviours during exploiting this machine.This machine was made for Jordan's Top hacker 2018 CTF , we tried to make it simulate a real world attacks "as much as possible" in order to improve your penetration testing skills , also we but a little tricky techniques on it so you can learn more about some unique skills.The machine was tested on vmware (player / workstation) and works without any problems , so we recommend to use VMware to run it , Also works fine using virtualbox.Difficulty: Intermediate , you need to think out of the box and collect all the puzzle pieces in order to get the job done.The machine is already got DHCP enabled , so you will not have any problems with networking.Happy Hacking!

**Author:** askar

**Download:** [[VulnHub]](https://www.vulnhub.com/entry/w34kn3ss-1,270/)

## W34kn3ss Level 1 Walkthrough

Below is a full hacking walkthrough video on how to solve and exploit W34kn3ss: 1 machine.

[ W34kn3ss: 1 machine walkthrough](https://youtu.be/sSqbkGvteP4)

Write-up on how the machine was compromised and exploited can also be read below.

### Reconnaissance

W34kn3ss Level 1 was found by conducting a live host identification on the target network using **netdiscover**, a simple ARP reconnaissance tool to find live hosts in a network.

~~~
netdiscover -r 172.20.86.1/24
~~~

Doing an **nmap scan** on W34kn3ss Level 1 machine's IP address found different ports available, namely: **port 22 for SSH, port 80 for HTTP and port 443 for SSL/HTTPs**.

![](https://i.imgur.com/bY4jgjD.png){: .mx-auto.d-block :} 

Browsing first to **http://172.20.86.130** showed an Apache2 Ubuntu Default page - which is pretty much the same webpage when browsed using its HTTPs version.

![](https://i.imgur.com/oluJWQp.png){: .mx-auto.d-block :} 

Examining the nmap scan revealed that the machine is using **weakness.jth** for its common name; and this information leads to adding weakness.jth to the host file pointing to the machine's IP address.

~~~
Edit /etc/host file and add weakness.jth and the machine's IP address.
~~~

![](https://i.imgur.com/PAsQGsG.png){: .mx-auto.d-block :} 

Browsing now to **http://weakness.jth**, a different landing page can be seen - wherein a "**keep following the white rabbit :D**" message was shown as well as an ascii art rabbit. The message was shown that is was written by **n30**.

![](https://i.imgur.com/j0QStsX.png){: .mx-auto.d-block :} 

### Enumeration

**dirb**, a web content scanner was then used to brute force for any available files and directory on the website.

~~~
dirb http://weakness.jth
~~~

![](https://i.imgur.com/H1ve7X5.png){: .mx-auto.d-block :} 

This resulted on finding the available URLs below that existed
on the weakness.jth website.
~~~
http://weakness.jth/index.html
http://weakness.jth/robots.txt
http://weakness.jth/private/index.html
http://weakness.jth/server-status (CODE:403)
~~~

Browsing to **http://weakness.jth/robots.txt** seems to be just a rabbit hole as it just showed a "**Forget it !!**" message.

![](https://i.imgur.com/u1NiYtS.png){: .mx-auto.d-block :} 

However, browsing to **http://weakness.jth/private** leads to finding a "**Cute File Browser with jQuery, AJAX and PHP**" webpage that shows available **notes.txt** and **mykey.pub** files on it.

![](https://i.imgur.com/UUlsCDu.png){: .mx-auto.d-block :} 

The **mykey.pub** file shows a public SSH rsa key.

~~~
ssh-rsa
AAAAB3NzaC1yc2EAAAABIwAAAQEApC39uhie9gZahjiiMo+k8DOqKLujcZMN1bESzSLT8H5jRGj8n1FFqjJw27Nu5JYTI73Szhg/uoeMOfECHNzGj7GtoMqwh38clgVjQ7Qzb47/kguAeWMUcUHrCBz9KsN+7eNTb5cfu0O0QgY+DoLxuwfVufRVNcvaNyo0VS1dAJWgDnskJJRD+46RlkUyVNhwegA0QRj9Salmpssp+z5wq7KBPL1S982QwkdhyvKg3dMy29j/C5sIIqM/mlqilhuidwo1ozjQlU2+yAVo5XrWDo0qVzzxsnTxB5JAfF7ifoDZp2yczZg+ZavtmfItQt1Vac1vSuBPCpTqkjE/4Iklgw==
root@targetcluster
~~~

While the notes.txt showed information that the key was generated using "**openssl 0.9.8c-1**".

![](https://i.imgur.com/xdZ1EkI.png){: .mx-auto.d-block :} 

### Vulnerability Identification

A research on "**openssl 0.9.8c-1**" was conducted wherein it leads to finding out that it is connected to **Debian OpenSSL Predictable PRNG vulnerability (CVE-2008-0166)**.

A quick definition of the vulnerability it is that: 

> OpenSSL 0.9.8c-1 up to versions before 0.9.8g-9 on Debian-based operating systems uses a random number generator that generates predictable numbers, which makes it easier for remote attackers to conduct brute force guessing attacks against cryptographic keys.

g0tmi1k has an interesting Github page on it made a couple years ago which is available at [https://github.com/g0tmi1k/debian-ssh](https://github.com/g0tmi1k/debian-ssh).

From the Github page there is available common keys, and since the public key uses RSA 2048 then the corresponding file (debian SSH RSA-2048) was downloaded which filename is debian_ssh_rsa_2048_x86.tar.bz2 at [https://github.com/g0tmi1k/debian-ssh/blob/master/common_keys/debian_ssh_rsa_2048_x86.tar.bz2](https://github.com/g0tmi1k/debian-ssh/blob/master/common_keys/debian_ssh_rsa_2048_x86.tar.bz2).

The downloaded
file was then extracted.

~~~
tar xvf
debian_ssh_rsa_2048_x86.tar.bz2
~~~

The contents of the **mypub.key** was then searched from the extracted files.

~~~
grep -r "ssh-rsa
AAAAB3NzaC1yc2EAAAABIwAAAQEApC39uhie9gZahjiiMo+k8DOqKLujcZMN1bESzSLT8H5jRGj8n1FFqjJw27Nu5JYTI73Szhg/uoeMOfECHNzGj7GtoMqwh38clgVjQ7Qzb47/kguAeWMUcUHrCBz9KsN+7eNTb5cfu0O0QgY+DoLxuwfVufRVNcvaNyo0VS1dAJWgDnskJJRD+46RlkUyVNhwegA0QRj9Salmpssp+z5wq7KBPL1S982QwkdhyvKg3dMy29j/C5sIIqM/mlqilhuidwo1ozjQlU2+yAVo5XrWDo0qVzzxsnTxB5JAfF7ifoDZp2yczZg+ZavtmfItQt1Vac1vSuBPCpTqkjE/4Iklgw=="
~~~

This resulted on finding that the **4161de56829de2fe64b9055711f531c1-2537.pub** is the same with the **mypub.key**.

![](https://i.imgur.com/FujyPg4.png){: .mx-auto.d-block :} 

### Exploitation

With this information, SSH login using **4161de56829de2fe64b9055711f531c1-2537** RSA private key was used to try to login on the W34kn3ss Level 1 machine. **n30** was used as the user, as it is the only possible username found (check the information above on the "keep following the white rabbit :D" webpage to see that message was made by n30).

~~~
ssh -i 4161de56829de2fe64b9055711f531c1-2537
n30@weakness.jth
~~~

This resulted on being able to login on the machine as the user **n30**.

![](https://i.imgur.com/sANOyfJ.png){: .mx-auto.d-block :} 

Through the user access, and checking the files in the directory of n30 user, a **user.txt** file can be seen which contains the first flag for the W34kn3ss Level 1 machine.

~~~
cat user.txt
25e3cd678875b601425c9356c8039f68
~~~

![](https://i.imgur.com/Rt6LwvN.png){: .mx-auto.d-block :} 

Additionally, a **.sudo_as_admin_successful** file can also be seen from the directory of **n30** user which means that the user can sudo with root permissions, though unfortunately at this point the password of the user is not yet known.

![](https://i.imgur.com/JsqfRaf.png){: .mx-auto.d-block :} 

The is also a suspicious executable file with filename of **code**, and from examining it, it is a **python 2.7 byte-compiled** file.

![](https://i.imgur.com/HkH8saq.png){: .mx-auto.d-block :} 

### Post Exploitation

The code file was then downloaded on the attacking machine and examined.

Running the code file shows that it generates a unique hash
for a hardcoded login info on the program.

![](https://i.imgur.com/7iHIbIP.png){: .mx-auto.d-block :} 

Since the file is a python byte-compiled file, we can
reverse engineer it and translate it back to its python source code so it can
be examined further.

For this, [uncompyle6](https://github.com/rocky/python-uncompyle6), a native Python cross-version decompiler and fragment decompiler was used. 

> uncompyle6 can be used to translate Python bytecode back into equivalent Python source code. It accepts Python bytecodes from Python version 1.3 to version 3.7, spanning over 22 years of Python releases.

Using uncompyle6 resulted on acquiring the equivalent Python
source code of the file.

Examining the source code of the file, it can be verified
that a hardcoded login info which is a character by character sequence of
string does exist.

![](https://i.imgur.com/hlHYTOK.png){: .mx-auto.d-block :} 

While we can manually acquire the whole string, we can also
just edit the python source code to display it for us.

![](https://i.imgur.com/fH1gDLF.png){: .mx-auto.d-block :} 

This resulted on getting the login info: **n30:dMASDNB!!#B!#!#33**.

Going back to the SSH session on user n30, we can use **sudo su** command to escalate n30 user to root using the password: **dMASDNB!!#B!#!#33**.

This resulted on getting root access on the W34kn3ss Level 1
machine!

Finally, we are able to read the flag on the root directory.

~~~
cat root.txt
a1d2fab76ec6af9b651d4053171e042e
~~~

![](https://i.imgur.com/6Rh7TiF.png){: .mx-auto.d-block :} 

W34kn3ss Level 1 machine has successfully been exploited.
