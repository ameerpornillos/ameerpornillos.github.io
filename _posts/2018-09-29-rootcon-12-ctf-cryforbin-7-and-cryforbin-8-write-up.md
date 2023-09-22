---
layout: post
title: ROOTCON 12 CTF Cryforbin 7 and Cryforbin 8 Write-Up
subtitle: Write-up on how I was able to solve Cryforbin 7 and Cryforbin 8 challenges on ROOTCON 12 CTF.
comments: false
categories:
- CTF 
---

Attended ROOTCON 12 and had a great time playing in its CTF competition. Kudos and huge thanks to the ROOTCON goons, CTF organizers and challenge creators for the making the local CTF possible. Local CTF competition is quite rare here in Philippines, so I am really thankful for ROOTCON team to have this as part of their event challenges.

![](https://i.imgur.com/ZmsLjUZ.jpg){: .mx-auto.d-block :}

This was supposed to be write-ups on how I was able to solve two of the 200 pts Exploitation (needed to hack the vulnerable machines/servers) challenges (which was quite hard to do as vulnerable service is getting attacked back and forth by multiple teams and some of the attacks probably causing the server to stop responding or its service to not work properly as intended - this resulted on stressfully losing my shell access quite a number of times which I needed to re-do exploits again and again waiting for the right time to re-gain shell access which made me feel having a bit of mental fatigue but still thankful as with luck, managed to find a way to escalate privilege to root and read the flags given the circumstances) but unfortunately didn't have any screenshots of it and the challenges are only local network accessible, so I decided just to do write-ups for challenges that I have solved with similar points.

Below is the info on how I was able to solve Cryforbin 7 and Cryforbin 8 challenges.
## Cryforbin 7 (200 pts)
**Challenge Description:**

There is a secret message in this file.

Download the file here: http://10.0.2.170/shares/cryforbin7
### Solution
For this challenge, we are given a 2.15 GB file which we needed to find the secret message on it.

After initial examination of the file, I found that it is a memory dump file and I can use [volatility](https://github.com/volatilityfoundation/volatility).

Running the command:

~~~
vol.py -f cryforbin7 imageinfo
~~~

suggests that the profile to be used is the **WinXPSP2x86**.

![](https://i.imgur.com/Ed66eBM.png){: .mx-auto.d-block :}

To see what are the processes running, we can use the command:

~~~
vol.py -f cryforbin7 --profile=WinXPSP2x86 pslist
~~~

This will show you the running processes available on the dump. Although the list would be long, but we can see that there are **numerous cmd.exe and notepad.exe** running.

![](https://i.imgur.com/KaIhDhw.png){: .mx-auto.d-block :}

After learning about the numerous notepad.exe running, I used the command:

~~~
vol.py -f cryforbin7 --profile=WinXPSP2x86 notepad
~~~

to dump available entries on the notepad that are opened.

This will result on showing a few lines, and one of it is the **flag in ASCII art**.

![](https://i.imgur.com/qB1SwkS.png){: .mx-auto.d-block :}

ASCII art was FLAG_IS{TICSPRO} and correct flag was: {: .box-note}
**flag_is{TICSPRO}**

## Cryforbin 8 (200 pts)
**Challenge Description:**

A memory dump was taken of a workstation with a possible backdoor planted. Analyze the memory dump to determine the what was executed and provide its last two arguments in the format flag_is{first-second}

Download the file here: http://10.0.2.170/shares/cryforbin8
### Solution
Same with the Cryforbin7 file, the Cryforbin8 file is also 2.15 GB.

Since it is a memory dump, I just use volatility to read commands executed.

Using the command:

~~~
vol.py -f cryforbin8 --profile=WinXPSP2x86 cmdscan
~~~

showed an executed command: **net user /add update password **(which is most likely the possible backdoor planted as given in the challenge description)

![](https://i.imgur.com/j86rN7Y.png){: .mx-auto.d-block :}

Since it was stated in the description of the challenge that the format flag_is{first-second} which was the last two arguments executed, the flag is: {: .box-note}
**flag_is{ update-password}**


That's it. Hope you learn a thing or two.

---

A huge shoutout and thanks to my team mates in **[hsb] Ethical Hackers Club (2nd place)** - **AJ** and **Nathu**, as well as the other group of [hackstreetboys](https://ctftime.org/team/43377) that participated in the ROOTCON 12 CTF, [hsb] Team Harambae (Champion) and [hsb] hackdog (3rd place). I was really impressed with the group's growth on their skill level in CTFs seeing them in action playing this ROOTCON 12 CTF.
### ROOTCON 12 CTF Final Score:

![](https://i.imgur.com/eo5iUui.png){: .mx-auto.d-block :}

Greets to all friends I happen to saw on the ROOTCON 12. :)
