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

Of note, this desktop contains a packet capture file and folder titled "Tools" which contains an installation of Brim and Wireshark -- both of which will prove critical in our log analysis.

Let's look at our first challenge question.

**What was the alert signature for Malware Command and Control Activity Detected?**

We begin by opening the provided `Zone1.pcap` file in Brim:

![Zone1.pcap in Brim](/assets/img/uploads/zone1.pcap.png "Zone1.pcap in Brim")

"Malware Command and Control Activity Detected"