---
layout: post
title:  "Seckids: Basic Introduction to Nmap"
---

Welcome to the first of the **'Security:Kids'** series where I try to explain many concepts of cyber security in a simplistic and interesting manner. This topic for this blog post is to introduce readers to **Nmap**, otherwise known as the *Network Mapper*.

<img src="/assets/Nmap/1.png" alt="nmap logo" width="350"/> 

**What is Nmap?**

The *Network Mapper* is a tool developed and used for:
 - Network Exploration
 - Security Auditing
 - Port Scanning
 - Operating System and Version Detection
 - Basic vulnerability scanning...and many more!

Nmap is one of the oldest tools used in penetration testing, having just celebrated its 20th birthday on September 1st, 2017. It is still being actively developed despite its age, and continues to be a versatile and irreplaceable tool in a penetration tester's arsenal.

**Help Commands**

The simplest command when attempting to understand a new tool is to use the help function or the manual:

```shell
# Basic help commands
nmap -h
nmap --help

# Manual for nmap
man nmap
```
**Basic Scan**

The most basic port scanning technique is to scan a single IP address using the default nmap command. It scans the 1000 most popular TCP ports and returns the output.

```shell
# The default (basic) port scanning command: nmap [IP Address]
nmap 192.168.72.141

# Output
Starting Nmap 7.70 ( https://nmap.org ) at 2019-04-02 11:00 EDT
Nmap scan report for 192.168.72.141
Host is up (0.000095s latency).
Not shown: 996 closed ports # 4 open ports in the most popular 1000 ports
# The state indicates whether the port is currently open closed or filtered. 
# The service is what is most commonly running on that port
# or what nmap believes is running on that port. 
PORT    STATE SERVICE
22/tcp  open  ssh 
80/tcp  open  http
110/tcp open  pop3
143/tcp open  imap
MAC Address: 00:0C:29:8A:AB:A8 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 13.28 seconds
```

**Network Sweeping**

In a situation where a user is within an unknown network and has little to no idea what is in the network, it is possible to conduct a network sweeping scan by sending **ICMP ping requests** to each IP address in the network to discover which machines are currently available.

Some machines can be down, filtered or block ICMP requests which may cause the results to differ, but it is generally considered a good reference point to conduct a network sweeping scan.

```shell
# nmap -sn [IP Range]
# sn flag: Ping Scan - disable port scan
nmap -sn 192.168.72.0-254
```
**How to Download?**

Visit the download page of the official website at <a href='https://nmap.org/download.html'>https://nmap.org/download.html</a> and follow the instructions for your operating system. Some Linux distributions will have Nmap pre-installed (such as Kali Linux).


