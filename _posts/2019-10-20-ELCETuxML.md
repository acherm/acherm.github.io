---
layout: post
title:  "Talk at Embedded Linux Conference Europe 2019"
date:   2019-10-30 011:54:29 +0200
tags: [linux, open source, lyon, dissemination] 
---

I gave a [talk](https://sched.co/TLKI) at [Embedded Linux Conference Europe 2019 (co-located with Open Source Summit 2019)](https://events.linuxfoundation.org/events/open-source-summit-europe-2019/) in Lyon about "Learning the Linux Kernel Configuration Space: Results and Challenges".
The conference was a blast, maybe one of the best conference I've attended: diversity of topics, quality of presenters, in-depth and technical content, and many^many exchanges with people that want to understand, share, and help. 

I usually attend academic conferences, and I must say OSS+ELC is different in terms of scale. 
The number of attendees is incredible (around 2000), with many parallel tracks, demonstrations of vendors, keynotes. 
Maybe [SIGCSE 2018](https://www.sigcse2018.sigcse.org/) I've attended in Baltimore was similar, otherwise it's more around 300 people (I never attended ICSE though).  

I enjoyed many talks, just to name a few:
 * KernelCI: an effort to lead the (fragmented) testing effort of the kernel, now supported by the Linux foundation and companies like Google or Redhat
 * [Buildroot evolution](https://sched.co/TLMK), a very clear description of the build system, new features and release/testing process
 * [Continuous documentation](https://sched.co/TLBL) that nicely discusses many aspects of documentation. I particularly appreciated the advices about writing styles. Stupid biases are unfortunately present in technical writing. Think about the use of "whitelist/blacklist", "master/slave" or "hey guys", how they translate to other languages, and how they can hurt people. The speaker suggests to make pull requests to correct open source projects: very good idea. Btw I think the correction can be partly automated: a bunch of queries over Github to identify bad smells in README.md (with [BigQuery](https://codelabs.developers.google.com/codelabs/bigquery-github/index.html?index=..%2F..index)), a bot to replace the text, send a PR with an educational message, and we are done!
 * keynote of Linus Torvalds with interesting discussions about the Linux project and its "random crazy user bugs", the importance of commits messages (more important than code!?), subsurface, and git (Linus wanted to prove to himself he was able to do more than one successful project)
 * insightful report about boot time reduction and actually Linux kernel size reduction at Bootlin. Configuration options all along as [nicely exposed by Michael Opdenacker](https://sched.co/TLN3)
 * a session about [low spec embedded linux](https://sched.co/TLM2) with many discussions about kernel size 
 * a great presentation about Linux kernel documentation by Jonathan Corbet with many recommendations on how to improve the situation (process, tool, organization, etc.)
 * a crystal clear tutorial of Linux permissions (hint: giving the root permission is not the only path)
 * a crazy talk about using Linux with Android 
 * very impressive showcases, for example, state-of-the-art computer vision systems running on very small devices. My prefered one is the [Tux over a chessboard and a laser](https://twitter.com/acherm/status/1189328054721695745)
 * [automated testing summit 2019](https://elinux.org/Automated_Testing_Summit_2019) a full-day workshop about testing the Linux kernel (it was the main topic) and the ongoing effort to unify the forces (e.g., KernelCI). There are impressive tools/ideas developed by an industry-driven and passionate community.

I mainly attended Linux-related talks and I've been lucky to meet many interesting people that really want to collaborate.
OSS+ELC was definitely not like academic events: I am not saying it's "better", it's just different and complementary. But maybe there are lessons to learn here ;) 
 
By the way, the abstract of the talk can be found below:

> Given a configuration, can humans know in advance the size, the compilation time, or the boot time of a Linux kernel?
> Owing to the huge complexity of Linux (there are more than 15000 options with hard constraints and subtle interactions), machines should rather assist contributors and integrators in mastering the configuration space of the kernel.
> In this talk, Mathieu Acher will introduce TuxML an OSS tool based on Docker/Python to massively gather data about thousands of kernel configurations. Mathieu will describe how 200K+ configurations have been automatically built and how machine learning can exploit this information to predict properties of unseen Linux configurations, with different use cases (identification of influential/buggy options, finding of small kernels, etc.)
> The vision is that a continuous understanding of the configuration space is undoubtedly beneficial for the Linux community, yet several technical challenges remain in terms of infrastructure and automation.

[Slides/papers related to the talk can be found online](https://elinux.org/ELC_Europe_2019_Presentations). 
**[Youtube video of the talk is online](https://youtu.be/UBghs-cwQX4)** 
