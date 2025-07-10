---
date: 2025-07-10 15:04:58
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

> Keep going. Our disgruntled IT was supposed to only install a service on this computer, so look for commands that are unrelated to that.

### Which user was created after the package from the previous task was installed?

**Answer**:

### A user was then later given sudo priveleges. When was the sudoers file updated? (Format: Month Day HH:MM:SS)

**Answer**:

### A script file was opened using the "vi" text editor. What is the name of this file?

**Answer**:

## Task 5 - Bomb has been planted. But when and where?

> That `bomb.sh` file is a huge red flag! While a file is already incriminating in itself, we still need to find out where it came from and what it contains. The problem is that the file does not exist anymore.

### What is the command used that created the file `bomb.sh`?

**Answer**:

### The file was renamed and moved to a different directory. What is the full path of this file now?

**Answer**:

### When was the file from the previous question last modified? (Format: Month Day HH:MM)

**Answer**:

### What is the name of the file that will get created when the file from the first question executes?

**Answer**:

### Task 6 - Following the fuse

> So we have a file and a motive. The question we now have is: how will this file be executed?
>
> Surely, he wants it to execute at some point?

### At what time will the malicious file trigger? (Format: HH:MM AM/PM)

**Answer**:

## Task 7 - Conclusion

> Thanks to you, we now have a good idea of what our disgruntled IT person was planning.
>
> We know that he had downloaded a previously prepared script into the machine, which will delete all the files of the installed service if the user has not logged in to this machine in the last 30 days. It’s a textbook example of a  “logic bomb”, that’s for sure.
>
> Look at you, second day on the job, and you’ve already solved 2 cases for me. Tell Sophie I told you to give you a raise.

## EOF

I really like these rooms which fully commit to a story!  It helps to feel more like a real event.