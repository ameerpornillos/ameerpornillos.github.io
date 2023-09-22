---
layout: post
title: Google CTF 2017 (Quals) Write-Up
subtitle: Here is my write-up for Google CTF 2017 (Quals) - Mindreader challenge.
comments: false
categories:
- CTF 
---

It was my first time participating in Google CTF which I think was quite hard (though enjoyed it), which is probably the reason why it was entertaining reading [tweets regarding #GoogleCTF](https://twitter.com/hashtag/googlectf).

One of the most entertaining tweets probably came from [rfc](https://twitter.com/rfchacks/status/876185770406453248).

![](https://i.imgur.com/RddUvXj.png){: .mx-auto.d-block :}

I’ve only answered one challenge which was "**mindreader**" (which actually ate a lot of my time solving it) hence write-up can be read below.

![](https://i.imgur.com/NLNEgVj.png){: .mx-auto.d-block :}

**Mindreader** is categorized as an "**Easy**" challenge which can give 50 pts.

**Note:** All of the challenges started with 500 pts. (with the exception of the bonus Sanity Check - "Start Here – FAQ"). Points lessen when a team was able to solve it (which is why Mindreader became 50 pts, as there were quite a number of teams able to solve it.)

Visiting [https://mindreader.web.ctfcompetition.com/](https://mindreader.web.ctfcompetition.com/) will give you a simple web page.

![](https://i.imgur.com/6eZHB81.png){: .mx-auto.d-block :}

If you happen to check the source code, you can see nothing that is interesting.

![](https://i.imgur.com/eIfxWAC.png){: .mx-auto.d-block :}

Same thing with the robots.txt file.

![](https://i.imgur.com/cro9QKF.png){: .mx-auto.d-block :}

Entering value on the form (e.g. flag) will move you to the following URL: [https://mindreader.web.ctfcompetition.com/?f=flag](https://mindreader.web.ctfcompetition.com/?f=flag)

![](https://i.imgur.com/iyLSTZ3.png){: .mx-auto.d-block :}

The first part of finding its vulnerability was actually easy, as entering a valid internal Linux file seems that the form can be used to read it.

![](https://i.imgur.com/78ZYwjg.png){: .mx-auto.d-block :}

This validates that the vulnerability is [Local File Inclusion](https://en.wikipedia.org/wiki/File_inclusion_vulnerability#Local_File_Inclusion) (LFI).

{: .box-note}
Local File Inclusion (LFI) is similar to a Remote File Inclusion vulnerability except instead of including remote files, only local files i.e. files on the current server can be included for execution. This issue can still lead to remote code execution by including a file that contains attacker-controlled data such as the web server's access logs. – from [Wikipedia](https://en.wikipedia.org/wiki/File_inclusion_vulnerability#Local_File_Inclusion)

Browsing on **/etc/issue** [https://mindreader.web.ctfcompetition.com/?f=/etc/issue](https://mindreader.web.ctfcompetition.com/?f=/etc/issue) will show that the web application is using Debian 8 as its server.

![](https://i.imgur.com/fQ3e3nL.png){: .mx-auto.d-block :}

What’s more exciting is that, it seems that the account has root privileges (or account with elevated privileges) as I was able to read even the **/etc/shadow** (a file that stores actual password in encrypted format of the user’s account) as well as the root’s .bashrc file (**/root/.bashrc**) (.bashrc is a shell script that Bash runs whenever it is started interactively).

![](https://i.imgur.com/7wCXBsQ.png){: .mx-auto.d-block :}

![](https://i.imgur.com/atFw0lU.png){: .mx-auto.d-block :}

With this, it was concluded that the vulnerability must be used to find where the flag is (and this is the part which ate a lot of my time).

It took me long to find out that the web page was running on [**Google App Engine** ](https://cloud.google.com/appengine/), this was verified by using **main.py** as the value for the the LFI vulnerable URL.

Browsing to [https://mindreader.web.ctfcompetition.com/?f=main.py](https://mindreader.web.ctfcompetition.com/?f=main.py) will show the web application source code.

![](https://i.imgur.com/CIgrVHR.png){: .mx-auto.d-block :}

Notice the highlighted line on the screenshot. This line tells that the flag can be found on the environment with the value of "**FLAG**". (Note that image above with the source code was recent, as during the CTF the **HALL_OF_SHAME** has values – which I think was those users who heavily brute-forced or [DOS](https://en.wikipedia.org/wiki/Denial-of-service_attack)d the challenge. Which made players unable to access it for quite some time.)

With the flag is on the environment value, simply by just reading the **/proc/self/environ** will give out the flag.

However, if you notice on the source code requests with **proc**, **random**, **zero**, **stdout** and **stderr** values will get **403 error**.

![](https://i.imgur.com/60EDJoU.png){: .mx-auto.d-block :}

So browsing to the URL, [https://mindreader.web.ctfcompetition.com/?f=/proc/self/environ](https://mindreader.web.ctfcompetition.com/?f=/proc/self/environ) will give a forbidden 403 page.

![](https://i.imgur.com/4iDJAUV.png){: .mx-auto.d-block :}

This is where I got stucked (for so many hours). (╯°□°）╯︵ ┻━┻

So what I did next was to check the files which the environment might be included.

Files I have checked where: (I actually checked so many files, but listed below are those what I’ve actually remembered as I’ve forgot the others.)

- /etc/environment
- /etc/bash.bashrc
- /root/.bashrc
- ..and more

So far nothing on this file can relate to the **FLAG environment**.

I also did checked the compiled python source code of the web application which can be found at [https://mindreader.web.ctfcompetition.com/?f=main.pyc](https://mindreader.web.ctfcompetition.com/?f=main.pyc).

But unfortunately did not find anything (though found **/home/vmagent/app/** which means that the web application was on that path).

I even researched and checked if there are available files on the Google App Engine format sample [https://github.com/GoogleCloudPlatform/getting-started-python/tree/master/1-hello-world](https://github.com/GoogleCloudPlatform/getting-started-python/tree/master/1-hello-world) that are also available for the web application and will show interesting data.

![](https://i.imgur.com/3Wd5mDa.png){: .mx-auto.d-block :}

Unfortunately, only **requirements.txt** exist and it didn’t help me find the FLAG (though learned that web application was running on gunicorn 19.7.1 and uses flask 0.12.1.

![](https://i.imgur.com/crtmCnJ.png){: .mx-auto.d-block :}

Researched again was done but this time only focused on the "**proc**" as it feels like I’ve been looking for a needle in a haystack.

From the **man pages of proc(5)** as written on the [**Linux Programmer’s Manual**](http://man7.org/linux/man-pages/man5/proc.5.html), it was read that:

{: .box-note}
/proc/self/fd/N is approximately the same as /dev/fd/N in some UNIX and UNIX-like systems. Most Linux MAKEDEV scripts symbolically link /dev/fd to /proc/self/fd, in fact.
This means that there is a symbolic link from **/proc/sef/fd/N** to **/dev/fd/N** with that information, it was concluded that if the symlink does exist on the server, then it is possible to call **/proc/self/environ** by using **/dev/fd**.

After learning this, it was first tried if **/dev/fd** can be accessed. Browsing to **/dev/fd/8** [https://mindreader.web.ctfcompetition.com/?f=/dev/fd/8](https://mindreader.web.ctfcompetition.com/?f=/dev/fd/8) gives an output which means that **/dev/fd** is accessible.

![](https://i.imgur.com/wJLQNCL.png){: .mx-auto.d-block :}

URL should then be crafted to read **/proc/self/environ** using **/dev/fd**, for this **/dev/fd/../environ** value was used.

Using the URL [https://mindreader.web.ctfcompetition.com/?f=/dev/fd/../environ](https://mindreader.web.ctfcompetition.com/?f=/dev/fd/../environ) the flag was revealed!

![](https://i.imgur.com/dyTakXW.png){: .mx-auto.d-block :}

FLAG=**CTF{ee02d9243ed6dfcf83b8d520af8502e1}**

Mission complete. (•̀ᴗ•́)و ̑̑
