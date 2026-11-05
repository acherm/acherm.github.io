---
layout: post
title:  "Build Variants × Differential Testing: A Severe OpenSSH Bug"
date:   2026-11-05 10:00:00 +0100
tags: [OpenSSH, sshd, security, differential testing, build variants, variability, feature interaction, configurable systems, glibc, musl, PAM]
---

A severe OpenSSH bug, in three steps ([original thread](https://x.com/kultekin/status/2118306312736116835)):

1. 🧱 **Build variants.** sshd × {glibc, musl} × {PAM, no PAM}: four builds of the same sshd.
2. 🧩 **Feature interaction.** Each option is safe alone. But musl + no PAM lets locked accounts in with any password of 257+ characters.
3. ⚖️ **Differential testing.** Same logins on all variants. One disagrees.

Poum!

No oracle needed: the variants are each other's oracle. And the bug lives in the combination, not in any single option. Test a single build and you never see it.
