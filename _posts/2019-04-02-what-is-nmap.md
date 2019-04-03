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

Please note: this post mainly focuses on the Nmap tool that is available on Linux.

**Help Commands**

The simplest command when attempting to understand a new tool is to use the help function or the manual:

```shell
# Basic help commands
nmap -h
nmap --help

# Manual for nmap
man nmap
```

There are a wide range of different techniques that can be used to adjust the output to what the user wants from each scan. In some cirumstances, if the nmap output is needed to be passed/piped into additional outputs, the *-oG* flag would be used, for example.

Using the *nmap -h* command will reveal the various options for nmap, all of which are categorised for simplicity. These are:

 - Target Specification
 - Host Discovery
 - Scan Techniques
 - Port Specification and Scan Order
 - Service/Version Detection
 - Script Scan
 - OS Detection
 - Timing and Performance
 - Firewall/IDS Evasion and Spoofing
 - Output
 - Miscellaneous

 In most circumstances, depending on the users needs, incorporating a several options from the necessary categories would be sufficient for a scan.

**Basic Port Scans**

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

Since the default scan only scans the most common ports, the amount of traffic it generates, in addition to the time taken is minimal. However, in circumstances simply scanning the common ports do not reveal the complete picture of all the services that are running. Thus, it is recommended to do a **full port scan** using the *-p* flag to ensure that other ports are accounted for. This flag can also allow for single or ranged port scanning, as shown below:

```shell
# Syntax for full port scans
nmap -p- [IP Address]
nmap -p 1-65535 [IP Address]

# Scanning only a single port
nmap -p 80 [IP Address]

# Scanning a range of ports
nmap -p 1-1024 [IP Address]
```

**Scenario-based Scanning**

Here is a common example of how to use nmap in penetration testing engagements:

*Discovering more information about a completely unknown machine*

```shell
# First, use a connect scan of the most common 1000 ports
nmap -sT -A [IP Address] -oG [Output File]
# -sT: Connect Scan
# -A: All -> OS and version detection, script scans and traceroute
# -oG: Output results to a file that is grepable (linux)

# Afterwards, use a deeper scan to enumerate all the ports
nmap -sT -p- [IP Address] -oG [Output File]

# The grepable file can be used in scripts for further enumeration.
``` 
*Network Sweeping*

In a situation where a user is within an unknown network and has little to no idea what is in the network, it is possible to conduct a network sweeping scan by sending **ICMP ping requests** to each IP address in the network to discover which machines are currently available.

Some machines can be down, filtered or block ICMP requests which may cause the results to differ, but it is generally considered a good reference point to conduct a network sweeping scan.

```shell
# -sn: Ping Scan - disable port scan
# -oA: Outputs the result in all formats (ideally should be specific)
nmap -sn 192.168.72.0-254 -oA [Output File]
```
If the output is in greppable format (-oG), some operations can be performed on it to compile all the IP addresses in a format that can be used in vulnerability scanning.

*Script Scanning*

All the scripts available to Nmap is located in the /usr/share/nmap/scripts directory (Kali Linux). The script files are in .nse format, which stands for the Nmap Scripting Engine. The NSE allows users to create and share their own scripts, all of which aim to enhance the performance of networking tasks.

It is recommended to use the *--script-updatedb* flag to update the database before using.

```shell
# Find scripts that are useful for the circumstance
ls -la /usr/share/nmap/scripts | grep [keyword]

# An example syntax of scripting
nmap -iL [input file] --script=[list of scripts] -oG [output file]
# -iL: insert input file if IP range
# --script=: script notation
# list of scripts: comma separated full names of scripts
#                  can also use * wild card notation
```

**How to Download?**

Visit the download page of the official website at <a href='https://nmap.org/download.html'>https://nmap.org/download.html</a> and follow the instructions for your operating system. Some Linux distributions will have Nmap pre-installed (such as Kali Linux).

Thank you for visiting my website! I will be posting more content about Nmap in the future so stay tuned!
