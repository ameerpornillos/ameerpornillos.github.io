---
layout: post
title: My OSCP Penetration Testing with Kali Linux Experience and Review
subtitle: I managed to pass the Offensive Security Certified Professional (OSCP) certification exam! Here is my experience and review on the PWK course.
comments: false
categories:
- Certifications 
tags:
- oscp
- pwk
---

I managed to pass the Offensive Security Certified Professional (OSCP) certification exam! Here is my experience and review on the Penetration Testing with Kali Linux (PWK) course.

![oscp](https://i.imgur.com/cG6bteO.jpg){: .mx-auto.d-block :}

Last couple of months; I have been super busy taking the Offensive Security’s [Penetration Testing Training with Kali Linux](https://www.offensive-security.com/information-security-training/penetration-testing-training-kali-linux/) course (I took the 2 months lab time access) in preparation for the [Offensive Security Certified Professional (OSCP)](https://www.offensive-security.com/information-security-certifications/oscp-offensive-security-certified-professional/) certification.

Good news is that just last week, I have received an e-mail from Offensive Security that I have **successfully completed the Penetration Testing with Kali Linux certification exam** and **obtained the Offensive Security Certified Professional (OSCP) certification**. It feels good being able to pass the OSCP exam and managed to pass it in one take.

With this, I will be sharing my experience/s regarding the course, in doing the labs, preparing for the exam and maybe some tips and tricks on passing it.

![oscp_passed1](https://i.imgur.com/yTiycYM.jpg){: .mx-auto.d-block :}

**Update:** Last week, my mailed hard copy [OSCP](https://www.youracclaim.com/users/ameer-pornillos) certificate and ID arrived.

> Just arrived. Thank you [@offsectraining](https://twitter.com/offsectraining) team! I tried harder and now an [OSCP](https://twitter.com/hashtag/OSCP?src=hash). It was a great experience! My review: [https://t.co/YvlrlcXfJF](https://t.co/YvlrlcXfJF) [pic.twitter.com/DER0bbNWsd](https://t.co/DER0bbNWsd)
> — Ameer (@ameerpornillos) [October 21, 2016](https://twitter.com/ameerpornillos/status/789304681549803520)

### What is my background?
Before discussing my experience regarding the course, I would like to give brief information about my I.T. background which may somehow help or encourage other people with the same background to try the course.

I have been doing freelance web developing and advertising during these past years, though before I used to work as a Technical Product Engineer for a software security company. Working on the software security company, I have developed several I.T. skills that are valuable up to now - this includes working with different operating systems, networking, scripting, programming and producing reports. During that time, I also did manage to get several I.T. certifications which includes Cisco Certified Network Associate (CCNA), CompTIA Network+, Microsoft Certified Technology Specialist (MCTS) (Small Business Server 2008) and Microsoft Certified Professional (MCP).

These skills made me comfortable working with Kali Linux (plus other operating systems) and working on command line.

Being a freelance web developer/advertiser made me knowledgeable working on different content management systems (CMS) and web programming languages like PHP, HTML, CSS and JavaScript which exposed me on different relational database management system (RDBMS) like MySQL and MSSQL. This also made me knowledgeable on different web application attacks such as SQL Injection (SQLi), Cross-site scripting (XSS) and Cross-Site Request Forgery (CSRF).

I came from a technical marketing background so I needed to learn more to cope up with my lack of knowledge in penetration testing. I have been studying and self training on penetration testing these past few months, now wanting to test my knowledge and skills I found the OSCP certification. Based from my conducted research OSCP is the "certification" on penetration testing. I have also read "horror" stories regarding the certification. With this I have decided to take the PWK course and prove my skills by earning the OSCP certification.

I signed up for the 60 days Lab access. I figured that the time frame is best for me - as it is not too long and not too short. The time frame gave me a sense of urgency which made me serious taking the course.

### Skills Recommendations

Before starting the PWK course and getting the OSCP certification, I recommend having knowledge on working with Linux/command line, Bash scripting, a scripting language either Perl or Python, TCP/IP networking and Assembly language. I did not list any automatic exploitation tools or mass vulnerability scanners like Nessus, Acunetix, HP WebInspect, OpenVAS and many others since the certification exam restricts usage of these tools. **You need to know how to conduct penetration testing manually and not just doing clicks to be able to pass the exam.**

### What the course is about?

**Penetration Testing with Kali Linux (PWK)** is a self-paced online penetration testing course where a student can conduct hands-on penetration tests/vulnerability assessments on a specially crafted Lab network which simulates a real corporate environment.

The course includes videos and document materials which introduces/teaches a student ethical hacking techniques and tools. These techniques and tools can be used and executed on the Lab network. Upon completing of the course materials, the student will have the basic skills to penetrate vulnerable systems in the Lab.

The objective is to hack and gain administrative/root access to the machines (access the trophy - proof.txt).

**Offensive Security Certified Professional (OSCP)** is the certification obtained upon passing the exam.

### Accessing the Lab Network

The Lab network is where the action happens. The Lab environment is the place where a student can test and sharpen his/her penetration testing skills. It consists of diverse systems including Windows, Linux, Solaris, FreeBSD, etc. which makes it more challenging.

Below is the simplified diagram of the Lab network. Additional networks can be unlocked as the student progresses doing the course.

![oscp-lab-network](https://i.imgur.com/P1PA73H.jpg){: .mx-auto.d-block :}

Tunneling and pivoting techniques must be used in order to access different networks.

My personal goal is to exploit the top-tier machines - **Pain**, **Humble** and **Sufferance** and unlock all networks (which I did). During the labs, I have managed to exploit over 24 machines including the hardest/toughest (top tier) machines namely **Pain**, **Humble** and **Sufferance**.

I also did manage to unlock and gain access to all the networks which includes the **Development Network**, **I.T. Department** and **Administrative Department**.

One strategy that I have implemented in doing the Labs is to get the low hanging fruit first then take on the top tier machines. Doing this method build up my knowledge on finding vulnerabilities and exploits plus knowing what penetration tools to use - these skills then progressively increased - which I have used to take down the top tier machines.

I also did learn how to develop my own set of scripts/tools that can assist me in finding vulnerabilities and exploits.

Overall the Lab did provide a great learning experience as I was able to learn and test different attack vectors.

### Taking the Exam
I scheduled the exam just before my Lab time was about to end.

{: .box-note}
**Take note that this exam is way different from other theory-based/memorize type of exam (where you can potentially just need to memorize answers from dumps). In this exam, you actually need to prove that you can actually conduct penetration tests plus have the skills to hack and exploit the exam machines.**

During the exam, a student is given 24 hours to hack 5 machines. Each of the machines has its own number of points (depending on the specific set of objectives).  Total exam score is 100 points and a minimum score of 70 points is needed to be able to pass the exam. Usage of automatic exploitation tools (SQLmap, SQLninja, etc.) and mass vulnerability scanners (Nessus, OpenVas, etc.) are restricted in the exam - and Metasploit is only allowed on only one target of choice. Another 24 hours (after exam period) is given to complete the penetration testing report on the exam machines.

Honestly, the exam was tough and intense. It made me push my way of thinking to the extreme.

I did manage to stick to my plan - to first gain root shell/administrative access on the machines that give the highest possible points. This means no quitting (keep on focusing/working) until the machine is completely successfully exploited. Using this strategy, I was able to acquire points needed to pass the exam.

### Tips and Tricks
I want to list couple of things that helped me prepare on the Labs and eventually pass the exam.

 - Practice, practice and practice. Keep practicing exploiting machines in the Lab. Practicing exploiting lab machines will increase your perception on finding vulnerabilities and exploits. Eventually you will get an “eye” which will help you know what specific exploit or vulnerability to use on a particular system.
 - Enumerate, enumerate and enumerate. You need to know your target and find out as much as you can about it.
 - Document, document and document. Keep good notes and try to document steps you have taken to exploit a machine.
 - Take a good sleep before the exam. (Exam is 24 hours long - you won’t be getting much of it.)
 - Have a back-up Internet connection ready when taking the exam. Internet connectivity is quite crucial during the exam and it is wise to be ready in cases where your primary Internet connection fails. For me I have my reserved Pocket WiFi ready, just in case that my Internet connection drops (I'm also ready even when there is power outage - if ever that happened during the exam, I can manage to continue working/answering for at least 3 to 5 hours using laptop/backup battery/Pocket WiFi/Power Bank). I live in the Philippines and there are cases when the Internet connection just fails (especially now it's rainy season in here) (though I’m quite happy that my Internet connection didn’t failed on me during the exam). Losing Internet connection when taking the exam means losing time to answer the exam.

### Conclusion
![oscp-banner](https://i.imgur.com/Ap2cE40.jpg){: .mx-auto.d-block :}

The OSCP exam and course is really amazing. It provides great value for money plus what you will learn is top-notch. The course was really tough and made me question my abilities. You will be trained to think for yourself (hence the **Try Harder** motto) and push your problem solving skills to a different level.

In this certification, you really need to work hard (really hard) and be dedicated in learning - which made me highly-respect people who obtained the cert (having a first-hand experience, I know how difficult it was).

I recommend this course for those who are interested in I.T. Security (especially penetration testers) and those who are currently looking for a more challenging I.T. Security certification to obtain.

Achieving Offensive Security Certified Professional (OSCP) certification has been a great learning and fulfilling experience.
