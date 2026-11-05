---
layout: post
title:  "Build Variants × Differential Testing: A Severe OpenSSH Bug"
date:   2026-11-05 10:00:00 +0100
tags: [OpenSSH, sshd, security, differential testing, build variants, variability, feature interaction, configurable systems, glibc, musl, PAM]
---


OpenSSH on musl Linux is in big trouble (lots of Docker images impacted). And there is an impressive, pedagogical write-up on [sshhole.com](https://sshhole.com/). [Original X thread here](https://x.com/kultekin/status/2118306312736116835).
My software engineering take is that it's a by-the-book differential testing over variants with 3 ideas: 
1. Build *variants*: sshd × {glibc, musl} × {PAM, no PAM} 
2. Feature *interaction*: each is safe alone.. musl + no PAM lets locked accounts in with any 257+ char password 
3. Differential *testing*: same logins on all variants, one disagrees  

Poum!

Worth teaching
