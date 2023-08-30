---
layout: post
title:  "ChatGPT for Reengineering Variants"
date:   2023-08-28 011:54:29 +0200
tags: [LLM, generative AI, variability, reengineering]
---

I just presented at 27th ACM International Systems and Software Product Line Conference (SPLC 2023, VariVolution workshop) the paper "Generative AI for Reengineering Variants into Software Product Lines: An Experience Report" joint work with Jabier Martinez. 
Basically using ChatGPT to analyze a bunch of software variants that differ a bit.
A short overview in this post, slides, paper, git repo at the end. 
Creating or maintaining software usually leads to customize and assemble pieces of code... But it can be a mess! To take an analogy, it's much more convenient to structure reusable pieces of a puzzle that you can systematically combine... Even kids do that!

![](assets/puzzleKids.jpeg)

We are interested in the following problem: given a set of variants (Java, C, SVG, UML, state charts, etc.) how to build a configurable program (a software product line aka SPL) that allows you to retrieve/derive them?

![](assets/problemVariants.jpeg)

For instance let's say you have three variants written in Java. What would be the Java program that can be configured to retrieve them? You can do it manually but it's error-prone and time-consuming. Or use tools like BUT4Reuse but4reuse.github.io/. Let's use LLM and ChatGPT!
![](assets/javaVariants.jpeg)

The idea is to fed to ChatGPT the three variants and ask relevant questions... for instance, synthesizing an annotated program. If the option is only the name of the variant, it has limited interest. We can ask to refactor the code to annotate with meaningful features!

![](assets/promptJavaVariants1.jpeg)
![](assets/promptJavaVariants2.jpeg)

LLM and ChatGPT can be interesting to synthesize template-based programs, identify interesting features, refactor and expand... like with the case of Java variants! 
For other formalism and systems, our experience is mixed: there are lots of inaccuracies. And size of variants is problematic. 

ChatGPT works out of the box and is somehow language agnostic. But it won't replace other assistants that are more reliable/deterministic... neither developers obviously ;) Hence a key open question is how and when to integrate ChatGPT for specific re-engineering tasks.

![](summaryVariants.jpeg)

As a final note,  re-engineering code variants is challenging for generative AI/LLM... It would be fantastic to develop dedicated benchmarks to assess LLM in the future! The repo github.com/acherm/variantsGP… is a good starting point (contains prompts, code, etc.)
 * Slides: https://www.slideshare.net/acher/generative-ai-for-reengineering-variants-into-software-product-lines-an-experience-report
 * Paper: https://inria.hal.science/hal-04160693/
 * Github: https://github.com/acherm/variantsGPT


  







 














