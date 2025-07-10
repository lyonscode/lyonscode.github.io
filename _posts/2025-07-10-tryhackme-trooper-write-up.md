---
date: 2025-07-07 10:48:15
layout: post
title: TryHackMe - Trooper - Write-Up
subtitle: Use Cyber Threat Intelligence knowledge and skills to identify a
  threat based on a report.
description: A walkthrough of the TryHackMe room Trooper
image: https://tryhackme-images.s3.amazonaws.com/room-icons/4da07b9ac2204d8d95505bb9601527eb.png
optimized_image: null
category: write-up
tags:
  - trooper
  - tryhackme
  - write-up
  - walkthrough
  - incident-response
  - infosec
  - cybersecurity
  - soc-analyst
  - OpenCTI
  - MITRE-ATT&CK
author: Michael
paginate: false
---
The TryHackMe room Trooper can be found [here](https://tryhackme.com/room/trooper).

> A multinational technology company has been the target of several cyber attacks in the past few months. The attackers have been successful in stealing sensitive intellectual property and causing disruptions to the company's operations. A threat advisory report about similar attacks has been shared, and as a CTI analyst, your task is to identify the Tactics, Techniques, and Procedures (TTPs) being used by the Threat group and gather as much information as possible about their identity and motive. For this task, you will utilise the OpenCTI platform as well as the MITRE ATT&CK navigator....

We begin our investigation by reading through a [report](https://www.trendmicro.com/en_us/research/20/e/tropic-troopers-back-usbferry-attack-targets-air-gapped-environments.html) written by Joey Chen of Trend Micro.  This report is provided in-room by TryHackMe, but has the name of the threat group changed to "APT X".

### What kind of phishing campaign does APT X use as part of their TTPs?

The answer to this question is found in the first paragraph of the report.

![](/assets/img/uploads/spear-phishing-emails.png)

**Answer**: spear-phishing emails

### What is the name of the malware used by APT X?

The answer to this question is actually in the title of the report, but it's spelled out clearly for us towards the end of the first page.

![](/assets/img/uploads/malware-called-usbferry.png)

**Answer**: USBferry

### What is the malware's STIX ID?

**Answer**: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

### With the use of a USB, what technique did APT X use for initial access?

**Answer**: \_\_\_\_\_\_\_\_\_\__ \_\_\_\_\_\_\_ \_\_\_\_\_\_\_\_\_ \_\____

### What is the identity of APT X?

Now, we can easily find the true assigned name of APT X by looking at Chen's report 

**Answer**: \_\_\_\_\_\_ \_\_\_\_\_\__

### On OpenCTI, how many Attack Pattern techniques are associated with the APT?

**Answer**: __

### What is the name of the tool linked to the APT?

**Answer**: \_\_\_\_\_\_\_\__

### Load up the Navigator. What is the sub-technique used by the APT under Valid Accounts?

**Answer**: \_\_\_\_\_ \_\_\_\_\____

### Under what Tactics does the technique above fall?

**Answer**: \_\_\_\_\_\__ \_\_\_\_\_\_, \_\_\_\_\_\_\_\_\_\_\_,  \_\_\_\_\_\_\_ \_\_\_\_\_\__ \_\_\_ \_\_\_\_\_\_\_\_\_ \_\_\_\_\_\_\_\_\_\_

### What technique is the group known for using under the tactic Collection?

**Answer**:

## EOF

Tools used: OpenCTI, MITRE ATT&CK