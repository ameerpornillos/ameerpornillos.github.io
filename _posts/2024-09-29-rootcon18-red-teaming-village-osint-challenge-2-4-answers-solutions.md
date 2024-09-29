---
layout: post
title: (ROOTCON 18) Red Teaming Village's OSINT Challenge 2 and 4 Answers/Solutions 
subtitle: Here are the answers/solutions for RTV OSINT Challenges 2 and 4 from ROOTCON 18.
excerpt: Here are the answers/solutions for RTV OSINT Challenges 2 and 4 from ROOTCON 18.
comments: false
thumbnail-img: https://i.imgur.com/ITSxEun.png
categories:
- CTF 
---

In this article, we'll delve into the answers for the [**Red Teaming Village**](https://www.facebook.com/redteamingph) **Open Source Intelligence (OSINT)** Challenge 2 and 4 from [**ROOTCON 18**](https://www.facebook.com/rootcon).  The Challenge 2 is labeled as Hard, while the Challenge 4 is Easy.

These challenges posed certain difficulties, requiring resourcefulness, but were all designed to be solvable using a mobile phone. Most of the tasks revolved around geolocation, Google searching/dorking, and a bit of investigative research.

Based on an analysis of the challenge logs, we will share direct solutions to the questions where most participants struggled or failed to provide correct answers.

![(ROOTCON 18) Red Teaming Village's OSINT Challenge 2 and 4 Answers/Solutions](https://i.imgur.com/ITSxEun.png){: .mx-auto.d-block :}

## Challenge Statistics

Before diving into the challenge answers, let's first share some stats related to the challenges. The data below is based on the logs for OSINT Challenge 2 and 4.

A total of **156 participants**, identified by unique usernames, attempted the OSINT challenges. Note that this data only pertains to Challenges 2 and 4, meaning even more attendees likely participated in the full set of challenges.

The user with the highest number of submitted answers was **"nes1"** with **611 submissions**, followed by **"R"** with **555**, and **"aaaa"** with **547**.

![](https://i.imgur.com/gDOL68i.png){: .mx-auto.d-block :}

### Challenge Winners

#### OSINT Challenge 4 / Easy challenge (1 winner)

**treemarinecube** solved the challenge in approximately _34 minutes_.

#### OSINT Challenge 2 / Hard challenge (2 winners)
**gouda** solved the challenge in _4 hours and 1 minute_.

**ch0nk** solved the challenge in _6 hours and 40 minutes_.

First 2 users above are the winners for Challenge 2 but other solvers (answered all questions correctly) for the Hard challenge include **aya44**, **nes1**, **T*te** (sorry need to redact one character from your username as there might be chance this article might pop-up searching for that word 😆), **Jk** and **luminesc3nt**.

## Challenge Answers/Solutions

{: .box-note}
**Note:** The solutions provided below are only on areas where most participants commonly failed to find the correct answers. Also all images where taken from mobile phone, just to show all questions are solvable using it.

### OSINT Challenge 4 / Easy Challenge (challenge URL/code: Eyr3v1akmocNfobJ)

Q1: What is ROOTCON 18's theme this year?

Answer: **Connected Realities**

Q2: Where is Red Teaming Village located?
Give the address.
Hint: Try looking for it on social media.

Answer: **Block 31 Lot 37, Red Team Village, Internet, 31337**

Q3: What toll booth is this?
https://i.imgur.com/4VfvEEQ.png

Answer: **Balintawak Toll Plaza**


Q4: From where am I looking?
https://i.imgur.com/NOiuykg.png

Answer: **Sky Eye**

Q5: A crypto game was hacked this 2024 with a staggering amount of more than 17,000 ETH. It was later revealed the exploit was orchestrated by an ex-developer associated with the project. What is the transaction hash where the exploiter successfully drained more than 17,000 ETH?

Solution: Thought this was easy, but looks like many participants got stuck on this question. Just do a Google search for "NFT game hacked 2024" or related search and many articles will show - which eventually will led to figuring out that the question is about the crypto game called "Munchables" - where it was exploited for 17,413.96 ETH, amounting to approximately ~$63M during the time of the exploit.

According to analysis by ZachXBT: 
North Korean hackers disguised as developers were hired and implanted a vulnerability in the smart contract. Also four different developers employed by the Munchables team are related to the exploiter and are likely the same person. They recommended each other for work, regularly transferred funds to the same two exchange deposit addresses, and contributed to each other’s wallets.

Hack transaction:

[https://blastscan.io/tx/0x9a7e4d16ed15b0367b8ad677eaf1db6a2a54663610696d69e1b4aa1a08f55c95](https://blastscan.io/tx/0x9a7e4d16ed15b0367b8ad677eaf1db6a2a54663610696d69e1b4aa1a08f55c95)

Below is an example article giving more information about the hack (also providing direct answer to the question).

![](https://i.imgur.com/oAMMAnL.png)

Verification of the hack transaction
![](https://i.imgur.com/iS6S41z.png)

References:

[https://medium.com/d3ploy/the-munchables-heist-unpacking-the-62-million-exploit-949d811205e3](https://medium.com/d3ploy/the-munchables-heist-unpacking-the-62-million-exploit-949d811205e3)

[https://beosin.com/resources/analysis-of-blast-defi-project-munchables-hack?lang=en-US](https://beosin.com/resources/analysis-of-blast-defi-project-munchables-hack?lang=en-US)

Answer: **0x9a7e4d16ed15b0367b8ad677eaf1db6a2a54663610696d69e1b4aa1a08f55c95**

Q6: BreachForums v1 hacking forum database with record up to November 29th, 2022 was leaked. The leak exposed various data, including members' information, private messages, IP logs, cryptocurrency addresses, posts on the forum and more. What is the "email address" of the user "testuser" in the database leak? 


Solution: No need to download anything, you can directly solve this by just searching. You could search in Twitter for "breachedforums leaked" or something similar, and you should be able to see a website containing the database. 

![](https://i.imgur.com/hzkYxEO.png)

![](https://i.imgur.com/fwoBuOT.png)

From there you could use the website to find the information that you are looking for.

![](https://i.imgur.com/KRRJw3P.png)

Alternatively, you could also do Google dorking ideally using the "owner's" username (if you happen to get a chance reading through it via your researching) and the target user which is "testuser".

![](https://i.imgur.com/MMsz0wk.png)

![](https://i.imgur.com/2NoQBXD.png)


Answer: **testuser@testuser.com**

Q7: What is the name of this waterway?
https://i.imgur.com/eyQXWQy.png

Solution: Do a Google image search and a similar picture of it will appear. Access the website and check the picture's description to find out what waterway it is.

![](https://i.imgur.com/MTsZcCW.png)

![](https://i.imgur.com/SnoHOxk.png)


Answer: **Pasig River**

Q8: Can you tell where this place is?
https://i.imgur.com/NV1hmjQ.jpeg

Solution: Do a Google image search to find out more about the picture. You could directly see similar pictures and one of it contains the attribution info. Search that library digital collection to find out more about the picture (e.g. searching for Philippines or something similar related to the picture).

![](https://i.imgur.com/BAb0PWp.png)

![](https://i.imgur.com/PT8cnqW.png)

![](https://i.imgur.com/P2aMv4p.png)



Answer: **Fort Santiago**

Q9: Who is the photographer of this photo?
https://i.imgur.com/NV1hmjQ.jpeg (same photo in previous question)

Solution: Same steps with the previous question, the information is already available once you find the photo's source based from its attribution.

![](https://i.imgur.com/LG7Fcyq.png)

Answer: **Harrison Forman**

### OSINT Challenge 2 / Hard Challenge (challenge URL/code: pEemMaaxP5WpGhM8)

Q1: This 2024, there was a backdoor found in a utility that serves as a collection of open source tools and libraries used for compression. This utility is available on almost all Linux and Unix-like systems. The obfuscated and sophisticated backdoor allows someone with the right private key to hijack SSH connection and execute malicious commands.

What is the malicious actor's Edwards448 elliptic curve public key? (provide in single line with no spaces)

Solution: Don't think solution is needed since there are 70 unique users that managed to solve this. Though, in case needed, just do a Google search for "backdoor utility 2024" or similar phrase - which will end up figuring that the backdoor is about XZ Utils. Then searching for "xz utils public key" or something similar will show different articles related to the backdoor which has the attacker's public key.

Below is an example for it.

![](https://i.imgur.com/zazIcDU.png)

![](https://i.imgur.com/VHnUfp1.png)


Answer: **0a31fd3b2f1fc69292683252c8c1ac2834d1f2c975c4765eb1f6885888933e48100cb06c3abe14ee8955d24500c77f6e20d32c602b2c6d3100**


Q2: In 2016 and 2017, this hacker group published several hacking files, including zero-day exploits allegedly from a group classified as an APT and suspected to be a branch of the NSA. This hacker group also publicly shared their Bitcoin wallet address. What is the Bitcoin wallet address?

Solution: Search for hacker group related to NSA tool leaks in 2016 or 2017 to find out that the hacker group is the "Shadow Brokers". Then do a quick Google search for "shadow brokers bitoin address"; the result should give you the address.

![](https://i.imgur.com/tg0bWF2.png)

![](https://i.imgur.com/LT0uynP.png)

Alternatively, you could check the Shadow Brokers' posts in their Medium page. This will also show the Bitcoin wallet address.

[https://medium.com/@shadowbrokerss/dont-forget-your-base-867d304a94b1](https://medium.com/@shadowbrokerss/dont-forget-your-base-867d304a94b1)

![](https://i.imgur.com/4UQFIYF.png)

Answer: **19BY2XCgbDe6WtTVbTyzM9eR3LYr6VitWK**

Q3: What is the Bitcoin wallet address that transferred the most Bitcoin to the group's wallet during 2016 and 2017? (related to the previous question) And how many Bitcoin did it sent to the group's wallet address (round it to a whole number, in case in decimal figures)? (separate the answers with underscore ex. Wallet_100)

Solution: To solve, just examine the transaction with the highest amount of Bitcoins being sent to the Shadow Broker's bitcoin wallet address. There are only several transactions in 2016 and 2017, which makes this easier to spot. From there you would notice one particular address sending 1 Bitcoin multiple times (6 times in total).

![](https://i.imgur.com/CZ9JDbd.png)

Answer: **1GaMfZRGDy3W2Pe5zvGM7Z6Lc929CUXPeS_6**

Q4: What is the Bitcoin wallet address to which the group's wallet transferred its funds and emptied their wallet in 2017? (related to the previous question) And how many Bitcoin did it sent (include exact amount)? (separate the answers with underscore ex. Wallet_1.234)

Solution: Again just needed to examine the transactions in the group's wallet (there are only a number of transactions in 2017), and from there you would see that the group's bitcoin wallet address sent 10.37079948 BTC to another wallet address allegedly transferring its funds (which is part of the answer).

![](https://i.imgur.com/QUMnl3L.png)

![](https://i.imgur.com/v2j8elw.png)

Answer: **1PC2QxiAD5TxcXNbHvyETf5sa4LpyhRr9G_10.37079948**


Q5:
In the past, the woman near the base of this historical monument (far in background) held a block, which has apparently gone missing with an inscription on it. What was the inscription?
https://i.imgur.com/pIJ1Uxj.jpeg

Solution: Focusing on the monument will result in finding out that it is Legazpi–Urdaneta Monument in Intramuros, Manila. The answer can be directly seen in its Wikipedia page.

![](https://i.imgur.com/zUFqCtU.png)

![](https://i.imgur.com/ef7l5cc.png)

Answer: **XXIV JUNIO MDCXXI** though we also accepted the answer **XXIV JUNIO MDLXXI** as apparently, a participant pointed it out on a close-up image of the block which has the inscription (Thank you! 🫡).

Q6: Someone left a message connected to the landmark in the middle, far in the background of the picture. Can you figure out what it says?
https://i.imgur.com/QdigeXK.png

Solution: Doing a Google Image search will show relevant results, particularly "Rizal Park". Further examination will show that the "Japanese Garden" is indeed in "Rizal Park". From there check Rizal Park in Google Maps and look for "Japanese Garden" there should be a landmark near it that matches the picture - which is "Trece Martires de Bagumbayan Historical Marker". The correct answer is in the review section of the marker.

![](https://i.imgur.com/zCQehVI.png)

![](https://i.imgur.com/cYKQYKU.png)

![](https://i.imgur.com/LItNvQq.png)

Answer: **Labintatlong Martir ng Bagumbayan**

Q7: Can you identify the food in the picture? Use proper case for answer (example: Fried Chicken)
https://i.imgur.com/FvF1nEs.jpeg

Solution: Focus either on the logo or "Tea Co" (e.g. do Google search for "Tea Co Ramen") - this will lead to figuring out that the restaurant is "Ramen Nagi". From there, just do a search for its "menu". There are only few selections of ramen which makes this easier to answer.

![](https://i.imgur.com/yyhAlYC.png)

Answer: **Wonder Kakuni King**


Q8: Can you help find out the name of the green character in the image? (again use proper case)
https://i.imgur.com/hQA8UHc.png


Solution: Perform a Google image search focusing on the female character. This will reveal that the character is "Tinay Pinay" from "Pilipino FUNNY Komiks," as part of the logo is visible in the image. From there, conducting a video search for "Tinay Pinay Funny Komiks" or a similar phrase will lead you to a video with a matching thumbnail. By watching the video, you will uncover the answer.

Alternatively, an easier approach would be to read relevant wikis about the "Tinay Pinay", which also mention the green character’s name.

![](https://i.imgur.com/Xvq4eYg.png)

![](https://i.imgur.com/o2bWaWA.png)

![](https://i.imgur.com/FjgEY9H.png)

![](https://i.imgur.com/7cCpcYI.png)

Answer: **Bozz Lakitenga**

9) 
In one of the maps for an iconic and influential shooter game, there is a plaque commemorating a person for his significant contribution for the game and its community. Can you identify the map, this person's alias and favorite weapon in this same game? (Use ALL CAPS for alias, and separate the answers with underscore. ex. map_name_ALIAS_AK-47)
https://i.imgur.com/047ssi6.png


Solution: Doing a Google image search using the picture will give you result of "Dust 2"; and doing a bit more research, you would know that the filename is "de_dust2". Googling for "de_dust2 plaque" that the plaque is for "Justin DeJong" which was added as an easter egg for the map. The next part is just doing searches using "Justin DeJong". First page Google result will show his "alias" as well as an archive interview link of him, mentioning his favorite weapon.

![](https://i.imgur.com/Vei45ic.png)

![](https://i.imgur.com/nwsUPk8.png)

![](https://i.imgur.com/k4DQDxl.png)

Accessing the site will give you an exact match of the plaque in one of its links.

![](https://i.imgur.com/I9rksxp.png)

First page Google result of searching for the found name will give you his "alias".

![](https://i.imgur.com/Bkp3mV3.png)

Reference section will lead to an archive interview naming his favorite weapon.

![](https://i.imgur.com/c6TQWmK.png)

![](https://i.imgur.com/EgrtYjx.png)

Answer: **de_dust2_N0TH1NG_MP5**

Q10: Our agent Minsc has gone missing; apparently, the only clue we have is the image below. https://i.imgur.com/0i953aO.png

Solution: Examining the image using Google image search, you would see that it is part of "Rick Astley - Never Gonna Give You Up" video which official music video is available via YouTube. https://www.youtube.com/watch?v=dQw4w9WgXcQ

From there, just search "Minsc" in YouTube comments - and the answer will show up. You could also use apps to make this easier.

![](https://i.imgur.com/hu4V4yR.png)

![](https://i.imgur.com/84hK0ZN.png)

Anser: **N3v3r_3v3r_g0nna_l3t_U_d0wn**


Thanks for reading! · (*ˊᗜˋ*)/ᵗᑋᵃᐢᵏ ᵞᵒᵘ*

{: .box-success}
**P.S.:** I would like to thank the whole RTV team/operators, especially to Shav; as he played a key role in staffing the booth, maintaining the presentations/display, and providing support to attendees during the OSINT challenges (RTV Day 1). A big shoutout to all the friends, colleagues and acquaintances I met at the conference — it was great seeing everyone! Congratulations to all the winners as well. 🫡

Thanks also to the CyberShield booth for these prizes below from solving some of their challenges. Had fun. Hopefully, in the next iteration of ROOTCON, more booths will offer security related technical challenges with prizes.

![](https://i.imgur.com/1bPpUm1.png)

Cheers! 🍻
