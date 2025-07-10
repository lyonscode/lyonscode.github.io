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

We're also given credentials and addresses to access OpenCTI and the MITRE ATT&CK Navigator:

![](/assets/img/uploads/thm-creds.png)

Let's get to work!

### What kind of phishing campaign does APT X use as part of their TTPs?

The answer to this question is found in the first paragraph of the report.

![](/assets/img/uploads/spear-phishing-emails.png)

**Answer**: spear-phishing emails

### What is the name of the malware used by APT X?

The answer to this question is actually in the title of the report, but it's spelled out clearly for us towards the end of the first page.

![](/assets/img/uploads/malware-called-usbferry.png)

**Answer**: USBferry

### What is the malware's STIX ID?

To find the answer to this question, we will move away from the report and log in to OpenCTI with the provided credentials.

![](/assets/img/uploads/opencti-dashboard.png)

(God, I love a good dashboard.)

After searching for USBferry, we get a hit with the STIX ID we are looking for.

![](/assets/img/uploads/usbferry-stix-id.png)

**Answer**: malware--5d0ea014-1ce9-5d5c-bcc7-f625a07907d0

### With the use of a USB, what technique did APT X use for initial access?

The word "technique" is a bit of a hint that we should look at the MITRE ATT&CK Navigator.

Pivoting over to the provided Navigator IP we are presented with a breakdown of the Tactics, Techniques, and Procedures related to the USBferry malware.

Under the "Initial Access" column, we find one highlighted technique: our answer.

![](/assets/img/uploads/mitre-att-ck-navigator.png)

**Answer**: Replication Through Removable Media

### What is the identity of APT X?

Now, we can easily find the true assigned name of APT X by looking at Chen's report, but that feels outside the intended spirit of the room.

Instead, let's pull it from the OpenCTI description of USBferry.

![](/assets/img/uploads/tropic-trooper.png)

**Answer**: Tropic Trooper

(Hey, that's why the room's called "Trooper"!)

### On OpenCTI, how many Attack Pattern techniques are associated with the APT?

So, this is a weird one.

We *should* be able to find the answer by going to the "Knowledge" section of the OpenCTI entry for USBferry.

![](/assets/img/uploads/attack-pattern-11.png)

On this page it clearly labels 11 Attack Patterns. As such, the answer should be "11," right?  Apparently not.

The question does not accept "11" as an answer.  I tried also putting "12" (maybe they were including all Total Relations?) and, no, that is also wrong.

Stumped, as 11 is clearly the number of Attack Patterns given by the OpenCTI page, I looked to confirm my answer in another walkthrough.

I consulted 0x4C1D's [write-up](https://medium.com/@0x4C1D/try-hack-me-trooper-walkthrough-d4ddecd7254a) from 2023 and found the author had been provided a very different answer:

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*DhgBFt4NY4qNtySTOoIXMA.png)

It would seem that the OpenCTI page used to report **39** attack patterns instead of the 11 I was given.

And sure enough, the Trooper room accepts "39" as the answer.

It is unclear what has caused this discrepancy.  All I know is that the information provided by the TryHackMe OpenCTI page has updated, but the sought-after answer on the page has not.  Curious!

**Answer**: 39

### What is the name of the tool linked to the APT?

Again, we find ourselves with a problem.

The answer to this question should clearly be found under the "Tools" section, but when I got there, there are no entries.

![](/assets/img/uploads/tools-empty.png)

Comparing this to our known-good walkthrough, we can find the answer in the same section.

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*7qWm_djuIo7DZVQQDXhYqQ.png)

At least we know where to find the answers, even if they're not showing up correctly!

**Answer**: BITSAdmin

### Load up the Navigator. What is the sub-technique used by the APT under Valid Accounts?

Back in the Navigator, we can easily find the answer the "Valid Accounts" technique of *Initial Access*.

![](/assets/img/uploads/local-accounts.png)

**Answer**: Local Accounts

### Under what Tactics does the technique above fall?

Easiest way to do this is to right click "Valid Accounts" (note that the question asks about the technique, not the sub-technique) and clicking "view technique".

![]()



**Answer**: \_\_\_\_\_\__ \_\_\_\_\_\_, \_\_\_\_\_\_\_\_\_\_\_,  \_\_\_\_\_\_\_ \_\_\_\_\_\__ \_\_\_ \_\_\_\_\_\_\_\_\_ \_\_\_\_\_\_\_\_\_\_

### What technique is the group known for using under the tactic Collection?

**Answer**:

## EOF

I'm noticing that while TryHackMe's rooms are usually pretty stellar, they do have a tendency to ask for dated answers.  It happened with the Attack Patterns question in this room, and I had a similar issue when completing the [Friday Overtime](https://lyonscode.github.io/tryhackme-friday-overtime-write-up/) room.

I can only speculate, but I know that threat intelligence is subject to regular updates as investigations continue and more information comes to light.  I imagine it can be quite difficult to update these rooms as the details change.

Nonetheless, I very much enjoyed this room and working with OpenCTI in particular!

Tools used: OpenCTI, MITRE ATT&CK Navigator