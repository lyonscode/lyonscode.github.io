---
date: 2025-07-10 14:56:24
layout: post
title: TryHackMe - Disgruntled - Write-Up
subtitle: Use your Linux forensics knowledge to investigate an incident.
description: A walkthrough of the TryHackMe room Disgruntled
image: https://tryhackme-images.s3.amazonaws.com/room-icons/03de138b8dfa8f5b003298c17b73fbd8.png
category: write-up
tags:
  - disgruntled
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
The TryHackMe room Disgruntled can be found [here](https://tryhackme.com/room/disgruntled).

> Hey, kid! Good, you’re here!
>
> Not sure if you’ve seen the news, but an employee from the IT department of one of our clients (CyberT) got arrested by the police. The guy was running a successful phishing operation as a side gig.
>
> CyberT wants us to check if this person has done anything malicious to any of their assets. Get set up, grab a cup of coffee, and meet me in the conference room.

Task 1 was introduction, let's move on to Task 2!

## Task 2 - Linux Forensics review



## Task 3 - Nothing suspicious... So far

> Here’s the machine our disgruntled IT user last worked on. Check if there’s anything our client needs to be worried about.
>
> My advice: Look at the privileged commands that were run. That should get you started.

### The user installed a package on the machine using elevated privileges. According to the logs, what is the full COMMAND?

**Answer**:

### What was the present working directory (PWD) when the previous command was run?

**Answer**:

## Task 4 - Let’s see if you did anything bad

## Task 5 - Bomb has been planted. But when and where?

## Task 6 - Following the fuse

## Task 7 - Conclusion

> Thanks to you, we now have a good idea of what our disgruntled IT person was planning.
>
> We know that he had downloaded a previously prepared script into the machine, which will delete all the files of the installed service if the user has not logged in to this machine in the last 30 days. It’s a textbook example of a  “logic bomb”, that’s for sure.
>
> Look at you, second day on the job, and you’ve already solved 2 cases for me. Tell Sophie I told you to give you a raise.