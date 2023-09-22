---
layout: post
title: HackerOne h1-212 CTF Write-Up/Solution
subtitle: Here is my write-up/solution on how I managed to solve the HackerOne h1-212 CTF.
excerpt: Here is my write-up/solution on how I managed to solve the HackerOne h1-212 CTF.
comments: false
thumbnail-img: https://i.imgur.com/WbwX4Qq.png
categories:
- CTF 
---


Recently HackerOne conducted a h1-212 CTF wherein 3 winners will be selected from those who managed to solve the CTF and submitted write-up. Winners will get an all expenses paid trip to New York City to hack against HackerOne 1337 and a chance to earn up to $100,000 in bounties. For more details on the CTF, refer to [Hack your way to NYC this December for h1-212](https://www.hackerone.com/blog/hack-your-way-to-nyc-this-december-for-h1-212).

Winners are already been announced which can be seen here [H1-212 CTF results](https://www.hackerone.com/blog/h1-212-ctf-results).

Didn't win the CTF but I'm happy as I'm one of the solvers. (ツ)

Below is my write-up/solution on how I managed to solve the HackerOne h1-212 CTF.

## Instructions
An engineer of acme.org launched a new server for a new admin panel at [http://104.236.20.43/](http://104.236.20.43/). He is completely confident that the server can’t be hacked. He added a tripwire that notifies him when the flag file is read. He also noticed that the default Apache page is still there, but according to him that’s intentional and doesn’t hurt anyone. Your goal? Read the flag!

## Solution
Browsing on the given website URL [http://104.236.20.43/](http://104.236.20.43/) shows a default Apache page.

![](https://i.imgur.com/X4AqVc0.png){: .mx-auto.d-block :}

Accessing the flag file on [http://104.236.20.43/flag](http://104.236.20.43/flag) shows the **"You really thought it would be that easy? Keep digging!"** message.

![](https://i.imgur.com/GRLUI9l.png){: .mx-auto.d-block :}

The **/etc/hosts** file was then modified to add **104.236.20.43 admin.acme.org entry on it.**

![](https://i.imgur.com/Azp0GfI.png){: .mx-auto.d-block :}

**This was done to be able to access the 104.236.20.43 IP address locally using the admin.acme.org domain.**

**Note:** During testing, several other subdomains were added but only **admin.acme.org** seems to be the right subdomain that needs to be used to be able to solve the challenge.

Browsing to [http://admin.acme.org/](http://admin.acme.org/) and intercepting the request using Burp Suite will show an additional **"admin=no"** cookie.

![](https://i.imgur.com/o4olsjT.png){: .mx-auto.d-block :}

The cookie was then changed to **"admin=yes" which showed HTTP/1.1 405 Method Not Allowed.**

![](https://i.imgur.com/0qs1YDa.png){: .mx-auto.d-block :}

After several tests, it was found adding **Content-Type: application/json** on the header request will show a different error, which in this case **HTTP/1.1 418 I'm a teapot** with a **{"error":{"body":"unable to decode"}}** message.

![](https://i.imgur.com/qPeIKpX.png){: .mx-auto.d-block :}

Adding **{"body":"flag"}** JSON data on the request will result on showing **{"error":{"domain":"required"}}** message.

![](https://i.imgur.com/r9RT7D2.png){: .mx-auto.d-block :}

For the next step, **{"domain":"admin.acme.org****"}** JSON domain data was added which showed **{"error":{"domain":"incorrect value, .com domain expected"}}**.

![](https://i.imgur.com/QaUfdyz.png){: .mx-auto.d-block :}

Based on the error message, the domain JSON data needs to have **.com** on it, which leads on using **{"domain":"admin.acme.org.com****"} **for the request. This resulted on showing **{"error":{"domain":"incorrect value, sub domain should contain 212"}} **message which means that the domain needs to have **212** on it.

![](https://i.imgur.com/SgF0zfi.png){: .mx-auto.d-block :}

JSON domain request data was then changed to **{"domain":"212.admin.acme.org.com****"} **which now resulted on a valid request: **HTTP/1.1 200 OK** with message **{"next":"\/read.php?id=0"}**.

![](https://i.imgur.com/Ry04JYd.png){: .mx-auto.d-block :}

Accessing the [http://admin.acme.org/read.php?id=0](http://admin.acme.org/read.php?id=0) with the **admin=yes** cookie resulted on showing **{"data":"IA=="}**.

**Note:** **IA==** is base64 encoded string which is equals to whitespace.

By having this information, it was concluded that the JSON data domain needs to have a correct payload, in order to display data with contents.

Additionally, **reset.php** was found existing by using **Dirbuster **in searching for possible additional files that exists on [http://admin.acme.org](http://admin.acme.org).

![](https://i.imgur.com/q0ZHLMs.png){: .mx-auto.d-block :}

Accessing **reset.php** will show **{"result":"ok"} **message, and after doing additional tests, it was found that what it does is that it resets the **read.php** row value.

After several tests on the JSON domain data, it was learned that it is possible to use it to read files. For example, using JSON domain data **{"domain":"212.cat/.com"}** will result on

~~~
 **{"data":"PCFET0NUWVBFIEhUTUwgUFVCTElDICItLy9JRVRGLy9EVEQgSFRNTCAyLjAvL0VOIj4KPGh0bWw+PGhlYWQ+Cjx0aXRsZT40MDQgTm90IEZvdW5kPC90aXRsZT4KPC9oZWFkPjxib2R5Pgo8aDE+Tm90IEZvdW5kPC9oMT4KPHA+VGhlIHJlcXVlc3RlZCBVUkwgLy5jb20gd2FzIG5vdCBmb3VuZCBvbiB0aGlzIHNlcnZlci48L3A+CjwvYm9keT48L2h0bWw+Cg=="} **
~~~

which can be decoded to **"The requested URL /.com was not found on this server"** message.

![](https://i.imgur.com/tBpw70N.png){: .mx-auto.d-block :}

This means that it is need to dis-associate the **".com"** on the JSON domain data while retaining it there on the JSON request data, as it is required for the request to be valid.

After several trials, it was found that using either **whitespace** or **\n** will let us do this. The **\n** can also be used to separate it with a new line.

Now using either **{"body":"flag","domain":"212.cat/ .com"}** or **{"body":"flag","domain":"212.cat/\n.com"}** will show the home page contents of the web application.

**Note:** For payload with **\n**, it is needed to subtract 1 from the reported read row number to be able to see data.

From here, as well as decoding the resulting base64 data, it was found that the application is a **PROXY GLYPE** application, which is a web-based proxy script written in PHP.

Accessing the **INSTALL.txt** file (which was found by researching what files exist in a PROXY GLYPE application) by using the JSON data payload **{"body":"flag","domain":"212.cat/INSTALL.txt\n.com"} **will also show that it is using **glype version 1.2**.

![](https://i.imgur.com/KCdihlQ.png){: .mx-auto.d-block :}

![](https://i.imgur.com/EJ39hD9.png){: .mx-auto.d-block :}

Unfortunately, tinkering on the GLYPE application files did not showed that it will lead to finding the flag.

Next step that was done, is to find local accessible files using the vulnerable JSON domain data request.

For this, the payload that was used was **{"body":"flag","domain":"212.\nlocalhost/\n.com"}** which resulted on seeing the localhost homepage file contents coded on base64.

![](https://i.imgur.com/wuGxLyb.png){: .mx-auto.d-block :}

Decoding the resulting base64 data and examining its contents, showed that it is similar to [http://104.236.20.43/](http://104.236.20.43/) Apache2 Ubuntu Default Page homepage.

![](https://i.imgur.com/WbwX4Qq.png){: .mx-auto.d-block :}

This actually verified when the **flag** file was accessed using **{"body":"flag","domain":"212.\nlocalhost/flag\n.com"}** as the JSON data request payload.

![](https://i.imgur.com/E5QoYxK.png){: .mx-auto.d-block :}

![](https://i.imgur.com/6ii1Mm2.png){: .mx-auto.d-block :}

The base64 encoded data can be decoded to the same **"You really thought it would be that easy? Keep digging!"** message which was obtained earlier accessing through [http://104.236.20.43/flag](http://104.236.20.43/flag).

It is worth noting that the local files that are hosted on the server can be accessed using the vulnerable JSON domain data request on [http://admin.acme.org](http://admin.acme.org) using the **admin=yes** cookie.

Next step that was done was to find, the correct file path and use it on the payload.

Luckily, HackerOne did provided some sort of 1337 information regarding it on the [Hack your way to NYC this December for h1-212](https://www.hackerone.com/blog/hack-your-way-to-nyc-this-december-for-h1-212) post (I do not know if this is intended but if it is, seems like HackerOne is really generous and wanted to meet hackers from all over the world.)

![](https://i.imgur.com/5CVd2Jx.png){: .mx-auto.d-block :}

The payload was then changed to **{"body":"flag","domain":"212.\nlocalhost:1337/\n.com"}** using **localhost** **port 1337**, which was used to be able to access files hosted locally on the server on port 1337.

Using the payload resulted on getting a base64 data: **SG1tLCB3aGVyZSB3b3VsZCBpdCBiZT8K **which can be decoded to **"Hmm, where would it be?"**.

![](https://i.imgur.com/NXXYIO5.png){: .mx-auto.d-block :}

![](https://i.imgur.com/YKcVE8I.png){: .mx-auto.d-block :}

Modifying the payload to include flag file 
~~~
{"body":"flag","domain":"212.\nlocalhost:1337/flag\n.com"}
~~~
resulted on acquiring a base64 encoded data:

~~~
RkxBRzogQ0YsMmRzVlwvXWZSQVlRLlRERXBgdyJNKCVtVTtwOSs5RkR7WjQ4WCpKdHR7JXZTKCRnN1xTKTpmJT1QW1lAbmthPTx0cWhuRjxhcT1LNTpCQ0BTYip7WyV6IitAeVBiL25mRm5hPGUkaHZ7cDhyMlt2TU1GNTJ5OnovRGg7ezYK
~~~

which can be decoded into:

{% raw %}
FLAG: CF,2dsV\/]fRAYQ.TDEp`w"M(%mU;p9+9FD{Z48X*Jtt{%vS($g7\S):f%=P[Y@nka=&lt;tqhnF&lt;aq=K5:BC@Sb*{[%z"+@yPb/nfFna&lt;e$hv{p8r2[vMMF52y:z/Dh;{6
{% endraw %}

![](https://i.imgur.com/mcafTLh.png){: .mx-auto.d-block :}

![](https://i.imgur.com/qX9LRZk.png){: .mx-auto.d-block :}

Finally, flag was found and read!

Thank you HackerOne for the challenge.

Congrats to the winners. (ツ)
