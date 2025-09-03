---
date: 2025-08-22 14:26:13
layout: post
title: "TryHackMe - TShark Challenge I: Teamwork - Write-Up"
subtitle: Put your TShark skills into practice and analyse some network traffic.
description: Put your TShark skills into practice and analyse some network traffic.
image: /assets/img/uploads/tshark-challenge-1-760x399.png
optimized_image: /assets/img/uploads/tshark-challenge-1-760x399.png
category: write-up
tags:
  - tshark
  - tshark challenge
  - tryhackme
  - write-up
  - walkthrough
  - linux
  - incident-response
  - infosec
  - cybersecurity
  - soc-analyst
author: Michael
paginate: false
---
The TryHackMe room TShark Challenge I: Teamwork can be found [here](https://tryhackme.com/room/tsharkchallengesone).

> An alert has been triggered: "The threat research team discovered a suspicious domain that could be a potential threat to the organisation."

Today we're looking at some malicous traffic with TShark.

> Investigate the contacted domains.\
> Investigate the domains by using VirusTotal.\
> According to VirusTotal, there is a domain marked as malicious/suspicious.

Let's get started!

### What is the full URL of the malicious/suspicious domain address?

### Enter your answer in defanged format.

We can begin by taking a look at the provided `teamwork.pcap` file with TShark.

![](/assets/img/uploads/tshark-r-teamwork.pcap.png)

A simple `tshark -r teamwork.pcap` pours the contents out for us, but as there are almost 800 packets in this capture, it's better if we can use some of TShark's filters to find what we're seeking.

For a more refined search, we can use `tshark -r teamwork.pcap -T fields -e http.request.full_uri | awk NF | sort -r | uniq -c | sort -r`.  This command breaks down as follows:

* `tshark -r teamwork.pcap` - Our base command, reading the `teamwork.pcap` file
* `-T fields -e http.request.full_uri` - This creates tells TShark to only display certain fields from the packets, in this case `http.request.full_uri` in order to get a list of the URLs
* `awk NF` - Built-in Linux tool removing empty lines
* `sort -r` - Built-in Linux tool to sort the items in reverse order
* `uniq -c` - Built-in Linux tool to count number of unique items
* `sort -r` - Sorted again to put in order of highest count to lowest

Running this command gets us to our answer.  What domain appears over and over again here?

![](/assets/img/uploads/tshark-http.request.full_uri.png)

We can then use [CyberChef](https://gchq.github.io/CyberChef/#recipe=Defang_URL(true,true,true,'Valid%20domains%20and%20full%20URLs')&input=aHR0cDovL3d3dy5wYXlwYWwuY29tNHVzd2ViYXBwc3Jlc2V0YWNjb3VudHJlY292ZXJ5LnRpbWVzZWF3YXlzLmNvbS8) to defang.

**Answer**: hxxp\[://]www\[.]paypal\[.]com4uswebappsresetaccountrecovery\[.]timeseaways\[.]com/

### When was the URL of the malicious/suspicious domain address first submitted to VirusTotal?

The answer to this question is easily found by looking up the found domain in [VirusTotal](https://www.virustotal.com/gui/url/16db0aadc2423a67cd3a01af39655146b0f15d20dc2fd0e14b325026d8d1717e/details).  The answer is in the Details tab.

![](/assets/img/uploads/tshark-1-virustotal.png)

**Answer**: 2017-04-17 22:52:53 UTC

### Which known service was the domain trying to impersonate?

The answer is clear by looking at the domain.

![](/assets/img/uploads/tshark-1-fake-paypal.png)

**Answer**: PayPal

### What is the IP address of the malicious domain?

### Enter your answer in defanged format.

We can modify our existing query by adding `-e ip.dst` and drop the second `sort -r` to bring up the IP addresses for all previously found domains.  The command looks like this `tshark -r teamwork.pcap -T fields -e http.request.full_uri -e ip.dst | awk NF | sort -r | uniq -c`.

All the "PayPal" domains point to the same IP address.

![](/assets/img/uploads/tshark-1-fake-paypal-ip-address.png)

[CyberChef](https://cyberchef.org/#recipe=Defang_IP_Addresses()&input=MTg0LjE1NC4xMjcuMjI2) can defang for us.

Answer format: \*\*\*\*.\*\*\*\*\*.\*\*\*\*\*.\*\*\*\*

### What is the email address that was used?

### Enter your answer in defanged format. (**format:** aaa\[at]bbb\[.]ccc)

Answer format: \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*.\*\*\*\*

### Congratulations! You have finished the first challenge room, but there is one more ticket before calling it out a day!

Part II of this room can be found at the below link:

* [TShark Challenge II: Directory](https://lyonscode.github.io/tryhackme-challenge-ii-directory-write-up/)