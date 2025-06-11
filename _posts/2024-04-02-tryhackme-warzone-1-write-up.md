---
date: 2025-06-11 13:15:11
layout: post
title: TryHackMe - Warzone 1 - Write-Up
subtitle: "You received an IDS/IPS alert. Time to triage the alert to determine
  if its a true positive. "
description: A walkthrough of the TryHackMe room Warzone 1
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

### **What was the alert signature for *Malware Command and Control Activity Detected*?**

We begin by opening the provided `Zone1.pcap` file in Brim:

![Zone1.pcap in Brim](/assets/img/uploads/zone1.pcap.png "Zone1.pcap in Brim")

Right away, we can see an alert with the category "Malware Command and Control Activity Detected" in it, but to make it more clear, let's search the phrase and display only those results:

![ET MALWARE MirrorBlast CnC Activity M3](/assets/img/uploads/et-malware-mirrorblast-cnc-activity-m3.png "ET MALWARE MirrorBlast CnC Activity M3")

In this, we can see our answer in the `alert.signature` column:

**Answer:** `ET MALWARE MirrorBlast CnC Activity M3`

### **What is the source IP address? Enter your answer in a *defanged* format.** 

In the same image, we can see the answer...

![Source IP](/assets/img/uploads/src_ip.png "Source IP")

...but we'd like it defanged, or rendered inert, so it can't accidentally cause harm during our investigation. 

 That's no problem to do on our own -- just put brackets around each period -- but we can use a tool like [CyberChef](https://cyberchef.org/#recipe=Defang_IP_Addresses()) if ~~we're lazy~~ we want to be sure we did it right:

![Source IP defanged in CyberChef](/assets/img/uploads/src_ip-cyberchef-defanged.png "Source IP defanged in CyberChef")

**Answer:** 172\[.]16\[.]1\[.]102

### **What IP address was the destination IP in the alert? Enter your answer in a *defanged* format.**

Next to the source IP, we can find the destination IP:

![Destination IP](/assets/img/uploads/dest_ip.png "Destination IP")

And again we can use CyberChef to defang:

![Destination IP defanged in CyberChef](/assets/img/uploads/dest_ip-cyberchef-defanged.png "Destination IP defanged in CyberChef")

**Answer:** 169\[.]239\[.]128\[.]11

### **Still in VirusTotal, under *Community*, what threat group is attributed to this IP address?**

Switching to [VirusTotal](www.virustotal.com), we can search up the destination IP address and find some information about it:

![TA505](/assets/img/uploads/threat-group-on-virustotal-ta505.png "TA505")

TA505?  That sounds like a threat actor designation.

![](/assets/img/uploads/ta505-mitre-att-ck-.png)

Yep -- the MITRE ATT&CK framework has an [entry](https://attack.mitre.org/groups/G0092/) on these guys.  There's our answer.

**Answer:** TA505

### **What is the malware family?**

We can get the answer from the same page.  The *Community* tab keeps mentioning something called "MirrorBlast".

![](/assets/img/uploads/mirrorblast.png)

Seems heavily associated with TA505, worth looking into.

![](/assets/img/uploads/heimdal-mirrorblast-ta505.png)

The above article from [Heimdal](https://heimdalsecurity.com/blog/mirrorblast-the-new-phishing-campaign-targeting-financial-organizations/) confirms that TA505 is thought to be using MirrorBlast, much like VirusTotal associated to the above IP address. 

**Answer:** MirrorBlast

### **Do a search in VirusTotal for the domain from question 4. What was the majority file type listed under *Communicating Files*?**

This question asks up to switch to the *Relations* tab to look at *Communicating Files*.

![](/assets/img/uploads/communicating-files.png)

Almost every one of the file types is a Windows Installer file.

**Answer:** Windows Installer

### **Inspect the web traffic for the flagged IP address; what is the *user-agent* in the traffic?**

We'll turn to our friend Wireshark to inspect the provided PCAP file for web traffic.  We take the IP address we found earlier and search for HTTP requests it was involved with.

![](/assets/img/uploads/wireshark-1.png)

Opening the highlighted packet and scrolling down to the Hypertext Transfer Protocol section, we find the User-Agent.

![](/assets/img/uploads/wireshark-2.png)

**Answer:** REBOL View 2.7.8.3.1

### **Retrace the attack; there were multiple IP addresses associated with this attack. What were two other IP addresses? Enter the IP addressed *defanged* and in numerical order. (*format: IPADDR,IPADDR*)**

Scrolling through the HTTP logs, we do see the Source make contact with some other IP addresses:

![](/assets/img/uploads/wireshark-185.png)

![](/assets/img/uploads/wireshark-192.png)

**Answer**: 185\[.]10\[.]68\[.]235, 192\[.]36\[.]27\[.]92

### **What were the file names of the downloaded files? Enter the answer in the order to the IP addresses from the previous question. (*format: file.xyz,file.xyz*)**

The first file name takes a bit of digging, as we have to look in the response from 185\[.]10\[.]68\[.]235 to see the file's name...

![](/assets/img/uploads/filter.png)

...But the second one is plain as day without opening the packet.

![](/assets/img/uploads/10opd3r.png)

**Answer**: `filter.msi`, `10opd3r_load.msi`

### Inspect the traffic for the first downloaded file from the previous question. Two files will be saved to the same directory. What is the full file path of the directory and the name of the two files? (*format: C:\path\file.xyz,C:\path\file.xy*z)

To find our answers we'll want to follow the TCP stream of the packet where the `.msi` file was found.

![](/assets/img/uploads/follow-tcp-stream.png)

This will open the communication stream between both hosts:

![](/assets/img/uploads/tcp-stream.png)

The question tells us we should expect the files to be found at a path beginning `C:\`, so we'll search for that in `Find:` field.

In doing so we find our answers:

![](/assets/img/uploads/arab-file.png)

**Answer**: `C:\ProgramData\001\arab.bin`, `C:\ProgramData\001\arab.exe`

*Note*: We get the full path from `C:\ProgramData\001\arab.bin`, and since the question says both files are found in the same path, we can deduce the full path for `arab.exe`.

### **Now do the same and inspect the traffic from the second downloaded file. Two files will be saved to the same directory. What is the full file path of the directory and the name of the two files? (*format: C:\path\file.xyz,C:\path\file.xyz*)**

We need only repeat this process for the second downloaded file.

![](/assets/img/uploads/2nd-downloaded-file.png)

**Answer**: `C:\ProgramData\Local\Google\rebol-view-278-3-1.exe`, `C:\ProgramData\Local\Google\exemple.rb`

## Conclusion

With all this information we've come up with, we can confirm this is a true positive found by our IDS/IPS. 

Tools used: Brim, VirusTotal, Wireshark