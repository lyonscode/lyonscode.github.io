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
>
> Investigate the contacted domains.\
> Investigate the domains by using VirusTotal.\
> According to VirusTotal, there is a domain marked as malicious/suspicious.

### What is the full URL of the malicious/suspicious domain address?

### Enter your answer in defanged format.

We can begin by taking a look at the provided `teamwork.pcap` file with TShark.

![](/assets/img/uploads/tshark-r-teamwork.pcap.png)

A simple `tshark -r teamwork.pcap` pours the contents out for us, but as there are almost 800 packets in this capture, it's better if we can use some of TShark's filters to better find what we're seeking.

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

Answer format: \*\*\*\*\*\*\*\*\*\* \*\*:\*\*:\*\* \*\**

### Which known service was the domain trying to impersonate?

Answer format: \*\*\*\*\*\*

### What is the IP address of the malicious domain?

### Enter your answer in defanged format.

Answer format: \*\*\*\*.\*\*\*\*\*.\*\*\*\*\*.\*\*\*\*

### What is the email address that was used?

### Enter your answer in defanged format. (**format:** aaa\[at]bbb\[.]ccc)

Answer format: \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*.\*\*\*\*

### Congratulations! You have finished the first challenge room, but there is one more ticket before calling it out a day!

* [TShark Challenge II: Directory](https://lyonscode.github.io/tryhackme-challenge-ii-directory-write-up/)