---
layout: post
title: Timisoara CTF 2018 Quals Write-Up
subtitle: Here are my write-ups for Timisoara CTF (Capture the Flag) 2018 Quals.
comments: false
categories:
- CTF 
---

Recently participated on Timisoara CTF 2018 Quals, which is an online qualifier round international jeopardy-style cybersecurity competition, dedicated to high-school students, community-organized in Timisoara, Romania, under Banat IT Association’s coordination.

Me and my team mate managed to finish at **#15 out of 460 teams** (under Just for fun Scoreboard -- category is for non-eligible teams since the CTF is originally dedicated to high-school students but it allowed non-eligible teams to join provided they will be on Just for fun Scoreboard). We have managed to solve 44.3% of the challenges or (1,961 out of 4,431 total score).

![](https://i.imgur.com/kXWZ1nG.png){: .mx-auto.d-block :}

Below are the write-ups for some of the challenges on it.
## Forensics
### Neurosurgery
![](https://i.imgur.com/tlKTeA6.png){: .mx-auto.d-block :}

**Note:** My team mate actually managed to solved this first. (Hi Mon! ^_^) I ran some problems with the volatility tool that I’ve used which ended up not being able to extract or dump files properly even with correct volatility profile and inode. It was just later after the challenge was solved, when we compare findings that I found out that there is a problem with the volatility I’m using particularly related to python libraries.
<h4>TLDR:</h4>

- Download the image and find out the OS and kernel version that it uses. (Ubuntu 16.04.4 LTS GNU/Linux 4.4.0-116-generic x86_64)
- Build a volatility profile.
- Use the volatility profile to examine the image.
- Extract the suspicious file that was running.
- Run the file locally to get the flag.

<h4>Solution:</h4>
The first step to solve the challenge, was to examine it. Running **file** command, will show that it is an **ELF 64-bit LSB core file x86-64, version 1 (SYSV)**.

![](https://i.imgur.com/RvTQUDU.png){: .mx-auto.d-block :}

By using **strings** command, it can be seen that the core dump file is using **Ubuntu 16.04.4 LTS** as its operating system and is on kernel version **4.4.0-116-generic**.

![](https://i.imgur.com/j7kbXPl.png){: .mx-auto.d-block :}

Based on the file, it was already known that it is needed to do memory forensics and analysis on it, and for that the [volatility](https://en.wikipedia.org/wiki/Volatility_(memory_forensics)) tool was used.

Upon learning this, quick Google search for downloading the volatility profile for Ubuntu 16.04.4 LTS GNU/Linux 4.4.0-116-generic x86_64 was done, but unfortunately didn’t found any of it.

Next thing that was done, was to create a volatility profile based on the operating system and kernel version. Steps for it is already available on [Volatility Wiki on Creating a New Profile](https://github.com/volatilityfoundation/volatility/wiki/Linux).

Hence, built a machine with an operating system and kernel version (Ubuntu 16.04.4 LTS GNU/Linux 4.4.0-116-generic x86_64) similar to the image file and then use it to create a volatility profile. More info can be found on [Volatility Wiki on Making the Profile](https://github.com/volatilityfoundation/volatility/wiki/Linux).

![](https://i.imgur.com/KHOS8XT.png){: .mx-auto.d-block :}

Created profile was then imported to volatility.

![](https://i.imgur.com/qxY4A6t.png){: .mx-auto.d-block :}

Using volatility and the created profile to examine the process running on the image file shows a rather suspicious file named **ht0p**.

![](https://i.imgur.com/8Ku1h7l.png){: .mx-auto.d-block :}

This can be verified by checking the commands entered on the machine, where it shows that **panda** user renamed the suleanu file into ht0p, deleted the .bash_history file and run the suspicious ht0p file in background, after that the regular htop file was also been run.

![](https://i.imgur.com/3T7uvzd.png){: .mx-auto.d-block :}

The suspicious file was then searched for its inode (**0xffff88007bd8e698**) and then use it to dump it.

![](https://i.imgur.com/pxrwO0z.png){: .mx-auto.d-block :}

Running the suspicious file would then result on getting the flag.

![](https://i.imgur.com/vdyuEec.png){: .mx-auto.d-block :}

**timctf{ch4nc3_f4vors_th3_pr3p4red_m1nd}**

To be continued…

More write-ups for the challenges will be added when there is available time.
