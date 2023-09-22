---
layout: post
title: WPICTF 2018 Write-Up
subtitle: Here are my write-ups for WPICTF (Capture the Flag) 2018.
excerpt: Here are my write-ups for WPICTF (Capture the Flag) 2018.
comments: false
thumbnail-img: https://i.imgur.com/32Oqc16.png
categories:
- CTF 
---

Got a bit of free time last week end and decided to poke around on several CTF challenges. Below are write-ups for some of the challenges on WPICTF (Worcester Polytechnic Institute Cyber Security Club) 2018.


## Pwn
### ezpz
The object of this challenge is to "pwn" or exploit the application that is running on a particular server.

The copy of the binary was then downloaded and examined.

Below is the description and properties of the binary:

**ezpz**: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=398b33bea32c53c41359dc62c173f541e41d513a, not stripped

**Arch: **  amd64-64-little

**RELRO:**  Partial RELRO

**Stack:**  No canary found

**NX:**     NX enabled

**PIE:**   PIE enabled

Running the binary, it shows that is looking for a correct password based from its output.

![ezpz](https://i.imgur.com/O3FsKYV.png){: .mx-auto.d-block :}

Analyzing the binary, it can be seen that it looking for a **EZPZPASSWORD** in the environment and entering the correct password will call the **sym.correct_pw** function.

![ezpz](https://i.imgur.com/32Oqc16.png){: .mx-auto.d-block :}

Additionally, the **sym.correct_pw** function seems to automatically output the flag.

![ezpz](https://i.imgur.com/HEQJjNM.png){: .mx-auto.d-block :}

Notice that when running the binary, it will show 3 memory addresses.

![ezpz](https://i.imgur.com/hUkcmZA.png){: .mx-auto.d-block :}

Apparently, the first memory address that is shown (highlighted on the screenshot above) points to the **correct_pw** function.

![ezpz](https://i.imgur.com/RdOaAST.png){: .mx-auto.d-block :}

It was also found that it will take 136 bytes of input to successfully reach the RIP register.

![ezpz](https://i.imgur.com/SGixGzU.png){: .mx-auto.d-block :}

Based from the information gathered, a script was then created to exploit the binary, which then was run on the actual server hosting the vulnerable application.

![ezpz](https://i.imgur.com/54T3EM5.png){: .mx-auto.d-block :}

This resulted on exploiting the application and getting the flag.

**WPI{3uffer_0verflows_r_ezpz_l3mon_5queazy}**
### shell-jail-1
For this challenge, we are given a private key which we can use to connect to a server using SSH.

Upon logging in, we can see three different files: access, access.c and flag.txt.

![shell-jail-1](https://i.imgur.com/ptFepwb.png){: .mx-auto.d-block :}

The flag.txt is available though the current user does not have permissions to read it.

Based from the files in the directory, we can already assume that the access binary which has suid permissions, is needed to be used in order to read the flag.

Checking the access.c source code, it can be seen that several strings are prohibited from being used, which include: *****, **sh**, **/**, **home**, **pc_owner**, **flag**, and **txt**.

![shell-jail-1](https://i.imgur.com/JXIvbK6.png){: .mx-auto.d-block :}

Even though several strings are blacklisted, this challenge can be easily solved by using wildcards, which in this case the "**?**" character was used.

Using **./access "cat fl?g.tx?"** command resulted on reading the flag.

![shell-jail-1](https://i.imgur.com/w7euc5g.png){: .mx-auto.d-block :}

**wpi{MaNY_WayS_T0_r3Ad}**
### shell-jail-2
This challenge is the same from the earlier one, except that it became more difficult, as upon running the binary, the environment PATH will be unset, which can be seen from screenshot below.

![shell-jail-2](https://i.imgur.com/ixv7iSR.png){: .mx-auto.d-block :}

This means that we cannot use the same method, as combined with the environment PATH being unset, the "**/**" character was also blacklisted.

![](https://i.imgur.com/fOycG4g.png){: .mx-auto.d-block :}

In this scenario our options are limited. Though no worries, as we can just use built in commands to solve this challenge.

For this challenge, the **source** command was used to bypass the restrictions.

Running **./access "source fl?g.tx?"** shows the flag.txt contents.

![shell-jail-2](https://i.imgur.com/0Mgpw5R.png){: .mx-auto.d-block :}

**wpi{p0s1x_sh3Lls_ar3_w13rD}**
## Reversing
### penguin
In this challenge, we need to reverse engineer the given binary to find the flag.

**penguin:** ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=cfd868d836ff29a109b2df14f586119135f344e1, stripped

Running the binary will show a message that you need to input the flag.

![penguin](https://i.imgur.com/UrNDLnY.png){: .mx-auto.d-block :}

A quick peek on the hex values, quickly shows that combination of flag strings is already available to be read.

![penguin](https://i.imgur.com/Re0G5wF.png){: .mx-auto.d-block :}

Arranging the strings will give the flag: **WPI{strings_only_gives_you_wings,_you_still_have_to_learn_how_to_fly._try_hexedit}**

Apparently, I also did solve this the harder way using gdb and reversing the binary.

This can be done by setting a breakpoint on the memory location where the inputted password is being checked.

![penguin](https://i.imgur.com/hehzI97.png){: .mx-auto.d-block :}

Notice above that the 0x555555554a7a (in my case) shows the second part of the flag. The binary checks part by part of your input and compare it to the correct flag strings.

By inputting part of the correct flag, (e.g. ./penguin WPI{strings_), then next part of the flag will appear on the same register.

![penguin](https://i.imgur.com/OzLdy5U.png){: .mx-auto.d-block :}

Doing this repeatedly, inputting each part of the flag that appears and combining it with the current input will result on getting the flag.

**WPI{strings_only_gives_you_wings,_you_still_have_to_learn_how_to_fly._try_hexedit}**
## Web
### vault
In this challenge, we are given a web application with a login form.

![vault](https://i.imgur.com/MQQSMP8.png){: .mx-auto.d-block :}

The goal here is to successfully login to the application. Apparently the **Sign Up** button is just a rick-roll prank as it will take you to the Rick Astley song "**Never Gonna Give You Up**" video on YouTube. This means that we need to somehow acquire or use the credentials of the active clients posted on the main page.

A quick check on the source code reveals part of the database information used for the application.

![vault](https://i.imgur.com/6i51z5G.png){: .mx-auto.d-block :}

This gives us information on the database table **client** which includes **id**, **clientname**, **hash** and **salt** as columns.

Using different characters on the login page, resulted on showing a **sqlite3.OperationalError** page.

The error page verified that the application is vulnerable to SQL injection. It also showed part of the source code being used by the application connection to its database and how it queries the login.

![vault](https://i.imgur.com/umZV5Ip.png){: .mx-auto.d-block :}

Using that information, the payload **'or(select count(id) from clients)=3--** was used, which will verify that there are 3 users (this information was based from the website front page).

This resulted on getting the Invalid password message:

![vault](https://i.imgur.com/PxrUO54.png){: .mx-auto.d-block :}

We can verify that the payload works by changing it to **'or(select count(id) from clients)=2--** (which we know is false).

This outputs the No such user in the database message:

![vault](https://i.imgur.com/H1L7GWz.png){: .mx-auto.d-block :}

Based from the results, we can already see that the web challenge is vulnerable to **blind SQL injection**.

A script was then created to exploit the vulnerability which was used to dump the **clientname**, **hash** and **salt** values.

![vault](https://i.imgur.com/pkIa8WJ.png){: .mx-auto.d-block :}

Complete values can be seen below.

- Client Name: Gaines
- Hash: ae6b2b347fd948b39a126e71decfc1cc411925a1ddc9f995949517d983fb027b
- Salt: leoczve

 

- Client Name: Goutham
- Hash: 6bad0bd9907898e3c7d6b2139241ac7591a4556b2f9fbc41ed15a31e6d2df738
- Salt: nepdrqs

 

- Client Name: Binam
- Hash: 49d790f22b2248638bf56f8a573c8e95eac2ed2f63a8f8eef97972d1b2d77bb7
- Salt: cseerlb

The SHA-256 hashes and salts were already available, so password cracking was attempted using different wordlists, but unfortunately it produced 0 results.

Since password cracking failed, direct login was then tried on the application. The idea is to use the found SQL injection vulnerability to trick the application in using a valid hash.

A new SHA-256 hash was then generated using **hello** as password.

2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824

Using the final payload: **'union select "2", "2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824", "" --** as the username and hello as password, the login was successful and flag was read. (2 was the correct id to use as the id 1 will just provide link to a rick-roll prank video)

![vault](https://i.imgur.com/arhZaoA.png){: .mx-auto.d-block :}

**WPI{y0ur_fl46_h45_l1k3ly_b31n6_c0mpr0m153d}**

