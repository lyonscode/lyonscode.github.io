---
date: 2024-03-30 09:54:18
layout: post
title: TryHackMe - Tardigrade - Write-Up
subtitle: Can you find all the basic persistence mechanisms in this Linux endpoint?
description: A write-up and walkthrough of the TryHackMe room Tardigrade
image: /assets/img/uploads/pexels-tima-miroshnichenko-5380594-resized.jpg
category: "{{slug}}"
tags:
  - tryhackme
  - tardigrade
  - write-up
author: Michael
paginate: false
---




A server has been compromised, and the security team has decided to isolate the machine until it's been thoroughly cleaned up. Initial checks by the Incident Response team revealed that there are five different backdoors. It's your job to find and remediate them before giving the signal to bring the server back to production.

Today we'll be looking at the TryHackMe room Tardigrade and trying discover all the modes of persistence left by a malicious actor.  Let's get started!

To begin, we'll connect to the compromised server