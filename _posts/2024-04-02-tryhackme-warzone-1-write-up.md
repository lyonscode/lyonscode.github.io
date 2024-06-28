---
date: 2024-04-02 12:28:39
layout: post
title: TryHackMe - Warzone 1 - Write-Up
subtitle: "You received an IDS/IPS alert. Time to triage the alert to determine
  if its a true positive. "
description: A write-up and walkthrough of the TryHackMe room Warzone 1
image: /assets/img/uploads/task2-760x399.png
optimized_image: /assets/img/uploads/task2.png
category: write-up
tags:
  - warzone-1
  - tryhackme
  - write-up
  - walkthrough
  - linux
  - incident-response
  - infosec
  - cybersecurity
  - soc-analyst
  - ids
  - ips
  - brim
  - wireshark
  - logs
  - log-analysis
author: Michael
paginate: false
---
The TryHackMe room Warzone 1 can be found [here](https://tryhackme.com/r/room/warzoneone).

> You work as a Tier 1 Security Analyst L1 for a Managed Security Service Provider (MSSP). Today you're tasked with monitoring network alerts.
>
> A few minutes into your shift, you get your first network case: **Potentially Bad Traffic** and **Malware Command and Control Activity detected**.  Your race against the clock starts. Inspect the PCAP and retrieve the artifacts to confirm this alert is a true positive. 

Today we’ll be looking at the TryHackMe room Warzone 1 and trying to determine if an alert we received is a false positive or the symptom of something more serious. Let’s get started!

Booting up the VM, we are greeted with a Linux desktop:

![Linux desktop](/assets/img/uploads/initial-desktop.png "Linux desktop")

Of note, this desktop contains a packet capture file called `Zone1.pcap` and folder titled "Tools" which contains an installation of Brim and Wireshark -- both of which will prove critical in our log analysis.

Let's look at our first challenge question.

**What was the alert signature for *Malware Command and Control Activity Detected*?**

We begin by opening the provided `Zone1.pcap` file in Brim:

![Zone1.pcap in Brim](/assets/img/uploads/zone1.pcap.png "Zone1.pcap in Brim")

Right away, we can see an alert with the category "Malware Command and Control Activity Detected" in it, but to make it more clear, let's search the phrase and display only those results:

![ET MALWARE MirrorBlast CnC Activity M3](/assets/img/uploads/et-malware-mirrorblast-cnc-activity-m3.png "ET MALWARE MirrorBlast CnC Activity M3")

In this, we can see our answer in the `alert.signature` column:

**Answer:** `ET MALWARE MirrorBlast CnC Activity M3`

**What is the source IP address? Enter your answer in a *defanged* format.** 

In the same image, we can see the answer...

![Source IP](/assets/img/uploads/src_ip.png "Source IP")

...but we'd like it defanged, or rendered inert so it can't accidentally cause harm during our investigation. 

 That's no problem to do on our own -- just put brackets around each period -- but we can use a tool like [CyberChef](https://cyberchef.org/#recipe=Defang_IP_Addresses()) if ~~we're lazy~~ we want to be sure we did it right:



**Answer:** 172\[.]16\[.]1\[.]102

**What IP address was the destination IP in the alert? Enter your answer in a *defanged* format.**

**Still in VirusTotal, under *Community*, what threat group is attributed to this IP address?**

**What is the malware family?**

**Do a search in VirusTotal for the domain from question 4. What was the majority file type listed under *Communicating Files*?**

**Inspect the web traffic for the flagged IP address; what is the *user-agent* in the traffic?**

**Retrace the attack; there were multiple IP addresses associated with this attack. What were two other IP addresses? Enter the IP addressed *defanged* and in numerical order. (*format: IPADDR,IPADDR*)**

**What were the file names of the downloaded files? Enter the answer in the order to the IP addresses from the previous question. (*format: file.xyz,file.xyz*)**

**Inspect the traffic for the first downloaded file from the previous question. Two files will be saved to the same directory. What is the full file path of the directory and the name of the two files? (*format: C:\path\file.xyz,C:\path\file.xy*z)**

**Now do the same and inspect the traffic from the second downloaded file. Two files will be saved to the same directory. What is the full file path of the directory and the name of the two files? (*format: C:\path\file.xyz,C:\path\file.xyz*)**

Tools used: Brim, Wireshark