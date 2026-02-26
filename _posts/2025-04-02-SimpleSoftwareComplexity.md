---
layout: post
title:  "A Quick Take on 'Why Can't We Make Simple Software?' by Peter van Hardenberg"
date:   2025-04-01 11:54:29 +0200
tags: [software complexity, robustness, variability, software engineering, dependencies, abstractions, reproducibility]
---

I recently watched a great talk by Peter van Hardenberg (aka pvh) titled *"Why Can't We Make Simple Software?"*. The talk dives into the deep-rooted reasons behind the multiple kinds of complexity in software systems — even the seemingly simple ones. A brief summary and some thoughts in this post.

Peter walks through a range of issues:
 * robustness and how software generalizes to inputs... pvh elaborates on "defensive programming" and why simple solutions often fail to handle the full range of inputs and reach edge cases.
 * the compounding effects of scale: really enjoy the discussions on how scale (10^2, 10^4, 10^6, or 10^8 users to manage) affect software design and complexity management.
 * leaky abstractions (also related to accidental complexity or abstraction gap) when developers need to understand hidden details of an interface to use it effectively.
 * rebound effect: Organizations and individuals have a complexity "budget." When tools or techniques reduce complexity, we tend to "spend" that gain by tackling more complex problems or adding more features, thus returning to a similar level of perceived complexity as before. It's related to the Jevon's Paradox that is used in economis and Green IT, but I find it so true in software engineering as well. Also enjoyed the connection with [homeostasis](https://en.wikipedia.org/wiki/Homeostasis).
 * variability and combinatorial explosion (called "hyperspace" in the talk) when you typically have to test on 3 screens, 5 browsers, 2 operating systems, etc. It rings a bell to my own research work, where variability adds further complexity (but also added value) to software systems. May I suggest some publications: a call for removing variability (https://hal.science/hal-03882594), a study on testing JHipster configurations (https://inria.hal.science/hal-01829928), etc.
 * ... and more.


I especially appreciated the use of simple, intuitive examples to illustrate each point. These aren’t necessarily new problems, but they’re presented with unusual clarity and connect the dots with some concepts. It’s a refreshing, well-articulated perspective on problems most developers have run into but often don’t step back to analyze.

There is no silver bullet, but the suggestions of doing less with less (also: more constraints imposed to reduce complexity) or starting over (possibly from scratch) are intriguing. Long road ahead, but it's a good start to acknowledge the complexity and start addressing it.

Highly recommended viewing for anyone interested in building more resilient, understandable systems: 📺 [Watch the talk on YouTube](https://www.youtube.com/watch?v=czzAVuVz7u4)

*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2025simplesoftwarecomplexity,
  author = {Mathieu Acher},
  title = {A Quick Take on 'Why Can't We Make Simple Software?' by Peter van Hardenberg},
  year = {2025},
  month = {apr},
  howpublished = {\url{https://blog.mathieuacher.com/SimpleSoftwareComplexity/}},
  note = {\url{https://blog.mathieuacher.com/SimpleSoftwareComplexity/}}
}
```
