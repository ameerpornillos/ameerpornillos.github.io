---
layout: post
title: HackInOS Level 1 (VulnHub) - Complete Walkthrough and Guide
subtitle: Here is a complete walkthrough and tutorial on how to hack and penetrate HackInOS Level 1 (HackInOS 1) of VulnHub.
excerpt: Here is a complete walkthrough and tutorial on how to hack and penetrate HackInOS Level 1 (HackInOS 1) of VulnHub.
comments: false
thumbnail-img: https://i.imgur.com/7nSDVSz.png
categories:
- WalkThroughs 
tags:
- vulnhub
---

Here is a complete walkthrough and tutorial on how to hack
and penetrate HackInOS Level 1 (HackInOS: 1) of VulnHub.

![](https://i.imgur.com/7nSDVSz.png){: .mx-auto.d-block :}

## HackInOS Level 1 Description:

> HackinOS is a beginner level CTF style vulnerable machine. This VM was created for the author's university's cyber security community and all cyber security enthusiasts. The author would like to thank to Mehmet Oguz Tozkoparan, Ömer Faruk Senyayla and Tufan Gungor for their help during creating this lab.
**Date release:** 9 Mar 2019

**Author:** Fatih Çelik

**Download:** [VulnHub](https://www.vulnhub.com/entry/hackinos-1,295/)

## HackInOS Level 1 Walkthrough

Below is a full hacking video on how to solve and exploit HackInOS: 1 machine.
[HackInOS: 1 machine walkthrough](https://youtu.be/YaqxEXCqrgA)

Write-up on how the machine was compromised and exploited which led to reading the flag can also be read below.

### Reconnaissance

HackInOS Level 1 was found by conducting a live host identification on the target network using **netdiscover**, a simple ARP reconnaissance tool to find live hosts in a network.
~~~
netdiscover -r 172.20.86.1/24
~~~

Doing a **nmap** scan on HackInOS Level 1 machine's IP address found different ports available, namely: **port 22 for SSH and port 8000 for http**.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_nmap.png){: .mx-auto.d-block :} 

Browsing first to **http://172.20.86.130:8000** showed a Wordpress blog, however, it doesn't seem to look right as the images are not rendering as well as it has broken links pointing to **localhost**.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_wordpress.png){: .mx-auto.d-block :} 

Checking the source code reveals that the website uses **localhost** as its domain name.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_localhost.png){: .mx-auto.d-block :} 

With this information, the **/etc/host** file was then edited to point **localhost** to the IP address of the HackInOS machine, which in this case **172.20.86.130**.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_hosts_file.png){: .mx-auto.d-block :} 

### Enumeration

Browsing now to **http://localhost:8000**, shows that the website is now rendering correctly. A user named **Handsome_Container** was also found from the website. 

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_handsome_container.png){: .mx-auto.d-block :} 

Since the website was made using Wordpress CMS, **wpscan**, a Wordpress Security Scanner was then used to try to find existing vulnerabilities on the website, though this did not lead in finding any vulnerability that can be used to quickly gain remote code execution on the HackInOS machine.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_wpscan.png){: .mx-auto.d-block :} 

**dirb**, a web content scanner was also used to brute force for any available files and directory on the website.
~~~
dirb http://localhost:8000
~~~

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_dirb.png){: .mx-auto.d-block :} 

This resulted in finding the available URLs below that
existed on the HackInOS website. 

Out of the found URLs, the **http://localhost:8000/robots.txt** showed interesting available URLs, **http://localhost:8000/upload.php** and **http://localhost:8000/uploads/**.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_robots.txt.png){: .mx-auto.d-block :} 

Browsing to **http://localhost:8000/uploads/** showed a **403 Forbidden** message which means that we do not have permission on the said URL.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_uploads_directory.png){: .mx-auto.d-block :} 

Browsing to **http://localhost:8000/upload.php** showed a webpage wherein it accepts images to be uploaded on the website.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_upload.php_-1024x215.png){: .mx-auto.d-block :} 

Apparently, a comment with Github URL can be seen below by checking the source code of the webpage.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_vulnerable_machine_hint.png){: .mx-auto.d-block :} 

Accessing the Github link available at [https://github.com/fatihhcelik/Vulnerable-Machine---Hint/blob/master/upload.php](https://github.com/fatihhcelik/Vulnerable-Machine---Hint/blob/master/upload.php), led to finding out the source code of the** upload.php** file being used by the HackInOS website.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_upload_source_code.png){: .mx-auto.d-block :} 


### Vulnerability Identification

By examining the source code, a way to bypass its image upload was learned, as well as the location of the file that will be uploaded.

Checking the line 20 of the source code, how the file was
renamed into can be readily seen. The line means that the MD5 hash of the
filename combined with a random number (from 1 to 100) will be the exact
filename of the file when it is successfully uploaded.

~~~
$target_file = $target_dir .
md5(basename($_FILES["file]["name].$rand_number);
~~~

Line 27 to 32 shows a way to bypass the image checking of the upload. This can be done by mime or content type to be either in **image/png** or **image/gif**.
~~~
if($check["mime] == "image/png" || $check["mime] == "image/gif"){

    $uploadOk = 1;

}else{

    $uploadOk = 0;

    echo ":)";

} 
~~~

### Exploitation

Knowing all of this, a file that would give us remote code execution on the server was created.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_shell_php.png){: .mx-auto.d-block :} 

Everything went okay as the image check function was successfully bypassed thus resulting for the payload to be uploaded on the server.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_payload_successfully_uploaded-1024x365.png){: .mx-auto.d-block :} 

Additionally, the MD5 hash of the uploaded file which in
this case is cmd.php combined with a random number from 1 to 100 would be the
exact filename of the file that was uploaded.

Since the random number is only from 1 to 100, it can be
easily brute-forced. What needs to be done is to create a list of the possible
MD5 hashes then use that list in brute-forcing to figure out the correct hash.

A script was then coded that will output the said list of MD5 hashes.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_md5_hash_bruteforce-1024x186.png){: .mx-auto.d-block :} 

What the script does is that it will get all the possible MD5 hashes of cmd.php (which is the filename) combined with numbers from 1 to 100. A sample output of it can be seen below.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_md5_hashes.png){: .mx-auto.d-block :} 

**wfuzz**, a tool designed to brute-force web applications was then used to try to find the correct filename of the uploaded file using the list as its payload. 
~~~
wfuzz -w test.txt --hc 404 http://localhost:8000/uploads/FUZZ.php
~~~

This resulted in finding the correct filename for the file that was uploaded.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_correct_filename.png){: .mx-auto.d-block :} 

Accessing the uploaded file and testing it, it can be verified that there is now remote code execution on the server.
~~~
http://localhost:8000/uploads/746f07f7c763118f48fffd74dcabe9ed.php?cmd=cat /etc/passwd
~~~

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_rce-1024x150.png){: .mx-auto.d-block :} 

This was then used to gain a remote shell on the server as www-data user.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_www-data_shell-1024x655.png){: .mx-auto.d-block :} 

Searching for **suid** files on the server, resulted on finding that **tail** has suid permission. This means that we can use the command to read all files including root-only accessible files.
~~~
find / -uid 0 -perm -4000 -type f 2&gt;/dev/null
~~~

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_suid.png){: .mx-auto.d-block :} 

### Post Exploitation

Since, the tail is a command used to display the tail end of a text file or piped data. It can be also used to display the entire contents of a file by using its -c or --bytes parameter in combination with large NUM bytes number.

The said information was then used to read the contents of /etc/shadow file which contains the root password in encrypted (more like a hashed value) format.
~~~
tail -c1G /etc/shadow
~~~

This resulted on getting the shadow hash of the root user.
~~~
root:$6$qoj6/JJi$FQe/BZlfZV9VX8m0i25Suih5vi1S//OVNpd.PvEVYcL1bWSrF3XTVTF91n60yUuUMUcP65EgT8HfjLyjGHova/:17951:0:99999:7:::
~~~

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_etc_shadow-1024x355.png){: .mx-auto.d-block :}

**John the Ripper**, a password cracking tool was then used to crack the hash.
~~~
john hash --wordlist=/usr/share/wordlists/rockyou.txt
~~~

The shadow hash was successfully been cracked using john - and the root password was... **john**.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_root_password-1024x118.png){: .mx-auto.d-block :} 

The acquired password was then used to escalate privilege as root user.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_root_shell.png){: .mx-auto.d-block :} 

Interestingly, reading the flag available on the root directory it seems that it is just a fake flag (more like of a hint).
~~~
root@1afdd1f6b82c:~# cat flag
Life consists of details..
~~~

At first, I got confused as root access was successfully gained but the flag seems to be fake. A .port file on the root directory also seems to be a hint.
~~~
root@1afdd1f6b82c:~# cat .port
Listen to your friends..
7*
~~~

What was done next, is that I re-enumerated again the server which led to finding MySQL database credentials on the **/var/www/html/wp-config.php** file.
~~~
// ** MySQL settings - You can get this info from your web host ** //
 /** The name of the database for WordPress **/ define('DB_NAME', 'wordpress'); /*** MySQL database username **/ define('DB_USER', 'wordpress'); /*** MySQL database password **/ define('DB_PASSWORD', 'wordpress'); /*** MySQL hostname */define('DB_HOST', 'db:3306');
~~~

Interestingly, the database host points to **db** and running a **ping** command on it shows that it resolves to **experimental_db_1.experimental_default** machine with the IP address of **172.18.0.2**.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_db_host-1024x147.png){: .mx-auto.d-block :} 

The found Wordpress database credentials were then used to
login in remotely to the MySQL database.
~~~
mysql -h 172.18.0.2 -uwordpress -pwordpress
~~~

Checking the existing tables on the database shows a certain **host_ssh_cred**, which is kind of suspicious as it is not normal for a Wordpress database to have a table with that name.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_wordpress_tables.png){: .mx-auto.d-block :} 

The contents of **host_ssh_cred** were then read which resulted on showing an **id** and **password** (in md5 hash) for **hummingbirdscyber**.
~~~
select * from host_ssh_cred;

hummingbirdscyber:e10adc3949ba59abbe56e057f20f883e
~~~

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_host_ssh_cred.png){: .mx-auto.d-block :} 

The MD5 hash was then cracked being **123456** as its value.
~~~
john –format=RAW-md5 sshcred
~~~

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_host_ssh_cred_password-1024x267.png){: .mx-auto.d-block :} 

Since SSH is available on the HackInOS machine, the id and password (**hummingbirdscyber** and **123456**) were then tried if it's possible to login on the server with it.

This resulted on gaining access to the server as user **hummingbirdscyber**.

Output of the id command for **hummingbirdscyber** shows that it is a member of the **docker** group.

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_hummingbirdscyber-1024x68.png){: .mx-auto.d-block :} 

Upon learning this, available docker images were then checked.
~~~
docker images
~~~

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_docker_images-1024x127.png){: .mx-auto.d-block :} 

I then created a new docker container using the ubuntu image and mount the / directory of the host in a folder named as ameer.
~~~
docker run -v /:/ameer -i -t ubuntu /bin/bash
~~~

After that, the docker image was accessed and the contents of the /root/ameer/ were inspected.

From it, there is a flag file which contains the real flag. All that is left is to read the flag. 

![](https://ethicalhackers.club/wp-content/uploads/2019/03/hackinos_flag.png){: .mx-auto.d-block :} 

HackInOS Level 1 machine has successfully been exploited. 
