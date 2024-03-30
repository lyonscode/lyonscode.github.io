---
date: 2024-03-30 09:54:18
layout: post
title: TryHackMe - Tardigrade - Write-Up
subtitle: Can you find all the basic persistence mechanisms in this Linux endpoint?
description: A write-up and walkthrough of the TryHackMe room Tardigrade
image: /assets/img/uploads/pexels-tima-miroshnichenko-5380594-resized.jpg
optimized_image: /assets/img/uploads/pexels-tima-miroshnichenko-5380594.jpg
category: "{{slug}}"
tags:
  - tryhackme
  - tardigrade
  - write-up
author: Michael
paginate: false
---
This room can be found [here](https://tryhackme.com/r/room/tardigrade).

> A server has been compromised, and the security team has decided to isolate the machine until it's been thoroughly cleaned up. Initial checks by the Incident Response team revealed that there are five different backdoors. It's your job to find and remediate them before giving the signal to bring the server back to production.

Today we'll be looking at the TryHackMe room Tardigrade and trying discover all the modes of persistence left by a malicious actor.  Let's get started!

### Task 1: Connect to the machine via SSH

To begin, we'll connect to the compromised server.  We've been given the following credentials to gain access:

> user: giorgio
>
> password: armani

We've also been told that the **giorgio** account has **root access** to the server -- nice! Though perhaps also nice for our malicious friend...

##### What is the server's OS version?

Our first question asks us to find the verify the version of operating system installed on the server.

An easy way to access that information on Linux is with the command `lsb_release -a`

{% figure {"image": "/assets/img/uploads/lsb_release-a.png"} %}