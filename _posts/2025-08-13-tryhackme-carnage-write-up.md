---
date: 2025-08-07 12:55:04
layout: post
title: TryHackMe - Carnage - Write-Up
description: A walkthrough of the TryHackMe room Carnage
image: https://assets.tryhackme.com/additional/carnage/carnage.png
optimized_image: /assets/img/uploads/carnage.png
category: write-up
author: Michael
paginate: false
---
The TryHackMe room Carnage can be found [here](https://tryhackme.com/room/c2carnage).

> Eric Fischer from the Purchasing Department at Bartell Ltd has received an email from a known contact with a Word document attachment.  Upon opening the document, he accidentally clicked on "Enable Content."  The SOC Department immediately received an alert from the endpoint agent that Eric's workstation was making suspicious connections outbound. The pcap was retrieved from the network sensor and handed to you for analysis. 

Today we some interesting work ahead of us: we'll be looking at actual malicious traffic captured by a member of the InfoSec community.

As such, we should be very careful **NOT** to visit any of the addresses found in our investigation as they are known untrustworthy at best and outright malicious at worse.

> Are you ready for the journey?

Let's load the provided pcap file into Wireshark and get started!

### What was the date and time for the first HTTP connection to the malicious IP?

(**answer format**: yyyy-mm-dd hh:mm:ss)

We can find the answer to this by simply filtering for `http` in Wireshark.  Since we're looking for the first connection, it's the first packet that comes up.

![](/assets/img/uploads/first-http-visit-arrival-time.png)

We then take the "Arrival Time" for this packet and format it to the question's standard.

**Answer**: 2021-09-24 16:44:38

### What is the name of the zip file that was downloaded?

This information is readily found in the info category for the packet.

![](/assets/img/uploads/downloaded-zip-file.png)

**Answer**: `documents.zip`

### What was the domain hosting the malicious zip file?

The host domain is provided by the Hypertext Transfer Protocol section of the packet.

![](/assets/img/uploads/host-domain-documents-zip.png)

**Answer**: `attirenepal.com`

### Without downloading the file, what is the name of the file in the zip file?

We can look for the file name by following the HTTP stream of the download request packet, as such:

![](/assets/img/uploads/carnage-follow-http-stream.png)

Opening this up, we can see a filename with an Excel extension buried among the noise.  This is most likely our file.

![](/assets/img/uploads/carnage-downloaded-filename.png)

**Answer**: `chart-1530076591.xls`

### What is the name of the webserver of the malicious IP from which the zip file was downloaded?

We can actually find the answer to the next two questions with this window as well.

![](/assets/img/uploads/carnage-litespeed.png)

**Answer**: LiteSpeed

### What is the version of the webserver from the previous question?

![](/assets/img/uploads/carnage-server-version.png)

**Answer**: PHP/7.2.34

### Malicious files were downloaded to the victim host from multiple domains. What were the three domains involved with this activity?

This took a second for me to determine what search parameters would return this information.  The hint provided tells us to look in a very narrow time range, only 19 seconds.  Combining the provided time frame with the knowledge that we were looking domain names, I crafted the below search:

`dns and (frame.time >= "2021-09-24 16:45:11") && (frame.time <= "2021-09-24 16:45:30")`

![](/assets/img/uploads/carnage-malicious-domains.png)

This sent back 10 frames, which include our answers.

**Answer**: `finejewels.com.au`, `thietbiagt.com`, `new.americold.com`

### Which certificate authority issued the SSL certificate to the first domain from the previous question?

I found this by removing the `dns` filter from my search and and checking for the first TSL packet for `finejewels.com.au`.

![](/assets/img/uploads/carnage-no-dns-filter.png)

I followed the TCP stream and found reference to the certificate authority in the results:

![](/assets/img/uploads/carnage-godaddy.png)

**Answer**: GoDaddy

### What are the two IP addresses of the Cobalt Strike servers? Use VirusTotal (the Community tab) to confirm if IPs are identified as Cobalt Strike C2 servers. (answer format: enter the IP addresses in sequential order)

We're looking for information on IP addresses that the affected machine is making regular contact with.  We can begin our search by looking for `GET` requests by using the following filter: `http.request.method == "GET"`

Answer format: \*\*\*.\*\*\*.\*\*.\*\*\*, \*\*\*.\*\*\*.\*\*\*.\*\**

### What is the Host header for the first Cobalt Strike IP address from the previous question?

**Answer**:

### What is the domain name for the first IP address of the Cobalt Strike server? You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).

Answer format: \*\*\*\*\*\*\*\*\*.\*\*\**

### What is the domain name of the second Cobalt Strike server IP?  You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).

Answer format: \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*.\*\*\*

### What is the domain name of the post-infection traffic?

Answer format: \*\*\*\*\*\*\*\*\*\*\*.\*\*\*

### What are the first eleven characters that the victim host sends out to the malicious domain involved in the post-infection traffic? 

Answer format: \*\*\*\*\*\*\*\*\*\**

### What was the length for the first packet sent out to the C2 server?

Answer format: \*\**

### What was the Server header for the malicious domain from the previous question?

Answer format: \*\*\*\*\*\*/\*.\*.\*\* \*\*\*\*\*\*\*\* \*\*\*\*\*\*\*/\*.\*.\*\* \*\*\*_\*\*\*\*\*\*\*\*\*/\*.*

### The malware used an API to check for the IP address of the victim’s machine. What was the date and time when theDNSquery for the IP check domain occurred?

(**answer format**: yyyy-mm-dd hh:mm:ss UTC)

Answer format: \*\*\*\*\*\*\*\*\*\* \*\*:\*\*:\*\*

### What was the domain in the DNS query from the previous question?

**Answer**:

### Looks like there was some malicious spam (malspam) activity going on. What was the first MAIL FROM address observed in the traffic?

**Answer**: 

### How many packets were observed for the SMTPtraffic?

**Answer**: