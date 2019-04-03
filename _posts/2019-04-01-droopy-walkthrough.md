---
layout: post
title:  "Droopy: v0.2 Walkthrough"
---

**This post is an older post from the beginning of my cyber security studies, about 3 months ago.**

Welcome to the walkthrough about **Droopy**, a vulnerable boot2root machine that is available on Vulnhub <a href='https://www.vulnhub.com/entry/droopy-v02,143/'> Droopy (vulnhub link)</a>. This is the methodology of how I successfully obtained root permissions on the Droopy virtual machine, and obtain the flag that was hidden after getting root. Let's begin!

Firstly, we find our IP address using *ifconfig* and *ip r*.

A simple netdiscover scan (*netdiscover -r 192.168.72.0/24*) then revealed all the other addresses on the network. All the machines are on the local host-only network, so anything caught by the network scanners will either by my machine or the virtual machine or the local gateway.

![netdiscover scan](/assets/Droopy/1.png)

It seems like the vulnerable machine has an IP address of 192.168.72.130. We should then use a *nmap -Pn -v [IP Address]* scan.

![nmap scan](/assets/Droopy/2.png)

The nmap scan revealed that the only port open after scanning the first 1000 common ports is port 80 (HTTP). 

We’ll run a deeper nmap scan using *nmap -sV -A -v [IP Address]* but for now we’ll explore more into the HTTP port. A *dirb* (directory buster) scan should reveal any hidden URLs and potential directories:

![dirb scan](/assets/Droopy/3.png)

(There is more to this scan, but the image is cropped because the other parts are irrelevant. Also, because of some issues with the server I have to redownload the VM, which means the IP address has changed.)

Let’s have a look at the robots.txt file:

![robots](/assets/Droopy/4.png)

There are some interesting directories in this. The /includes/ directory has a lot of other subdirectories which have all the files to the website. 

The website by the way, is a simple Drupal web server, which is a content management system - more on that later. A navigation through the URLs in robots.txt didn’t yield me much, so I moved on.

![cmsmap](/assets/Droopy/5.png)

Once I had discovered that the web server was running on a Drupal CMS engine, I decided to use CMSmap to do some further enumeration of the server. 

The most interesting pieces of information was that the server is run using Apache, PHP and Drupal 7.30. Drupal 7.3 is interesting because generally CMS has many vulnerabilities. I searched it up on CVE and turns out there are a lot of vulnerabilities. 

The most interesting one is the first one because it allows for remote code execution. We want to create a limited shell on the server, so remote code execution is the best idea.

![exploits](/assets/Droopy/6.png)

After trying this for a while, I realised that it doesn’t seem to work. After a while I decided to give something else a try.

So while I was reading into that, I was a little distracted and thought that it would be a good idea to use Metasploit to infiltrate the system. 

I opened msfconsole and searched for Drupal related exploits. The Drupalgeddon 2 allows for remote code execution so let’s give it a try.

![metasploit](/assets/Droopy/7.png)

So after configuring the settings for the exploit, I tried to activate it but it failed repeatedly. 
After being unable to figure it out, I decided to change to another method. 

![failed exploit](/assets/Droopy/8.png)

Searching around the internet again, I realised that there was a possible SQL Injection that allowed creating an admin user on the server. 
It was a python script. I decided to give it a try:

![drupal sql](/assets/Droopy/9.png)

I succeeded in creating another account, and then logged into the system. After awhile I found the original admin - drupaladmin, and decided to take over his account for fun:

After navigating around the website, I noticed that it is possible to execute PHP code from the website. 

It required changing some of the admin settings to allow for PHP code to be allowed on the webpage. I downloaded the code for a reverse shell and pasted it in the body of an article to be executed: 

![drupaladmin](/assets/Droopy/10.png)

This allowed for the creation of a web shell, which can then be redirected back to create a shell.

![listener](/assets/Droopy/11.png)

![shell](/assets/Droopy/12.png)

Success! This allowed the creation of a limited reverse shell on my computer, which I used to navigate around. 

The commands that I typed to enumerate the system included:

Uname -a (revealed that it was a droopy linux kernel)
Cat /etc/passwd (showed some username)
Whoami (www-data)
Cat /proc/version
Cat /etc/issue
Ps aux (found that there is netcat on the system, which turns out to be useless)

![shell2](/assets/Droopy/13.png)

I tried to find some vulnerabilities using netcat, cron and apache, however they didn’t work.

I decided maybe there were some kernel-related vulnerabilities that will allow for privilege escalation, so I searched the name of the kernel. 

I downloaded the exploit and transferred it to the /tmp folder of the server using netcat -lp [PORT] > file. 
It was important to transfer to the /tmp folder because this directory has execute permissions for the limited shell user.

![privesc](/assets/Droopy/14.png)

Unfortunately, the SHELL file could not be executed using ./SHELL no matter how much I tried. 
It seems like the shell that was uploaded to the website has limited functionalities, so we found another PHP reverse shell that was more capable. This allowed us to execute the shell, and obtain root!

![success](/assets/Droopy/15.png)

![root](/assets/Droopy/16.png)

Now it is time to locate the flag. After a while of locating, I found a mail with a clue that seems to point at using the rockyou wordlist to bruteforce a password. 
Then I found the dave.tc file that is located in the /root home directory.

![mail](/assets/Droopy/17.png)

I created a simple python server to transfer the dave.tc file onto my own computer.

![simpleserver](/assets/Droopy/18.png)

After a long time of research, I discovered that the .tc file extension is truecrypt, and the password can be cracked using truecrack. 

The clues from the www-data file indicates that the password is in the rockyou.txt file that has academy in the name. After a while of searching and using truecrack, the password was found.

![dictionary attack](/assets/Droopy/19.png)

The output of the file was in /dev/mapper. I used the mount function to mount the file into a new directory in the /mnt folder. 
Opening the folder resulted in three files and a secret file.

![mount](/assets/Droopy/20.png)

After probing a bit, in the directory .secret/.top was the hidden flag!

![flag](/assets/Droopy/21.png)

Using cat flag.txt we revealed the flag and completed the challenge! Thank you for visiting and hope it helped! 

![flag2](/assets/Droopy/22.png)
