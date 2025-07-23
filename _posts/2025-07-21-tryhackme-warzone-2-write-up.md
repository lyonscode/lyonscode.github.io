---
date: 2025-07-21 14:15:11
layout: post
title: TryHackMe - Warzone 2 - Write-Up
description: "You received another IDS/IPS alert. Time to triage the alert to
  determine if its a true positive. "
image: https://assets.tryhackme.com/additional/jrsecanalyst/task2.png
optimized_image: /assets/img/uploads/task2-760x399.png
category: write-up
tags:
  - warzone-2
  - tryhackme
  - write-up
  - walkthrough
  - incident-response
  - infosec
  - cybersecurity
  - soc-analyst
author: Michael
paginate: false
---
The TryHackMe room "Warzone 2" can be found [here](https://tryhackme.com/room/warzonetwo).

> You work as a Tier 1 Security Analyst L1 for a Managed Security Service Provider (MSSP). Again, you're tasked with monitoring network alerts.
>
> An alert triggered: **Misc activity**, **A Network Trojan Was Detected**, and **Potential Corporate Privacy Violation**. 
>
> The case was assigned to you. Inspect the PCAP and retrieve the artifacts to confirm this alert is a true positive.

This room is a follow-up to "Warzone 1".  Let's get started!

### What was the alert signature for **A Network Trojan was Detected**?

We begin by opening the provided `.pcap` file in Brim...

![](/assets/img/uploads/brim.png)

...and searching the phrase "A Network Trojan was Detected".

![](/assets/img/uploads/a-network-trojan-was-detected.png)

**Answer**: ET MALWARE Likely Evil EXE download from MSXMLHTTP non-exe extension M2

### What was the alert signature for **Potential Corporate Privacy Violation**?

We rinse and repeat to find this answer.

![](/assets/img/uploads/potential-corporate-privacy-violation.png)

**Answer**: ET POLICY PE EXE or DLL Windows file download HTTP

### What was the IP to trigger either alert? Enter your answer in a **defanged** format.

For a cleaner look at the info, we'll open the Brim Log Detail window by double-clicking the alert.

![](/assets/img/uploads/src_ip-warzone-2.png)

In the `src_ip` field we find our answer.  We can then defang the IP address by using [CyberChef](https://gchq.github.io/CyberChef/#recipe=Defang_IP_Addresses()&input=MTg1LjExOC4xNjQuOA).

![](/assets/img/uploads/defanged-ip-warzone-2.png)

**Answer**: 185\[.]118\[.]164\[.]8

### Provide the full URI for the malicious downloaded file. In your answer, **defang** the URI.

I found the answer to this question by clicking on "File Activity" under "Queries"...

![](/assets/img/uploads/file-activity-warzone-2.png)

...which brought me to the filename: `gap1.cab`.

I then searched for references to that name in the `.pcap` file.

![](/assets/img/uploads/search-gap1.cab.png)

We're looking for the full URI of the downloaded file, so I opened the `http` packet in more detail.

![](/assets/img/uploads/host-and-uri-warzone-2.png)

We can see `host` and `uri` fields in the Detail window.  Combining these two fields gets us our answer.  [CyberChef ](https://gchq.github.io/CyberChef/#recipe=Defang_URL(true,true,true,'Valid%20domains%20and%20full%20URLs')&input=YXdoOTNkaGt5bHBzNXVsbnEtYmUuY29tL2N6d2loL2Z4bGEucGhwP2w9Z2FwMS5jYWI)can once again defang (or we can do it ourselves).

**Answer**: awh93dhkylps5ulnq-be\[.]com/czwih/fxla\[.]php?l=gap1\[.]cab

### What is the name of the payload within the cab file?

To find this answer, we start by going back to the search for `gap1.cab` and selecting the `files` packet.

![](/assets/img/uploads/search-gap1.cab-2.png)

We pull the MD5 hash so we can put it into VirusTotal and learn more what's been uncovered about this file.

**Answer**:

### What is the user-agent associated with this network traffic?

**Answer**:

### What other domains do you see in the network traffic that are labelled as malicious by VirusTotal? Enter the domains **defanged** and in alphabetical order. (**format: domain\[.]zzz,domain\[.]zzz**)

**Answer**:

### There are IP addresses flagged as **Not Suspicious Traffic**. What are the IP addresses? Enter your answer in numerical order and **defanged**. (format: IPADDR,IPADDR)

**Answer**:

### For the first IP address flagged as Not Suspicious Traffic. According to VirusTotal, there are several domains associated with this one IP address that was flagged as malicious. What were the domains you spotted in the network traffic associated with this IP address? Enter your answer in a **defanged** format. Enter your answer in alphabetical order, in a defanged format. (**format: domain\[.]zzz,domain\[.]zzz,etc**)

**Answer**:

### Now for the second IP marked as Not Suspicious Traffic. What was the domain you spotted in the network traffic associated with this IP address? Enter your answer in a **defanged** format. (format: domain\[.]zzz)

**Answer**: