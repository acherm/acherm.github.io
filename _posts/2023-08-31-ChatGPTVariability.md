---
layout: post
title:  "ChatGPT for Programming Variability and Variants"
date:   2023-08-30 011:54:29 +0200
tags: [ChatGPT, LLM, generative AI, variability, programming]
---

I presented at [27th ACM International Systems and Software Product Line Conference](https://2023.splc.net/) (SPLC 2023) the paper ["On Programming Variability with Large Language Model-based Assistant"](https://inria.hal.science/hal-04153310/) joint work with José A. Galindo and Jean-Marc Jézéquel. 
In short how LLMs and ChatGPT can be concretely and originally used for programming software variability and variants. 
I first showed how ChatGPT can assist developers in implementing variability in different programming languages (C, Rust, Java, TikZ, etc.) and mechanisms (conditional compilation, feature toggles, command-line parameters, template, etc.). With "features as prompts", there is hope to raise the level of abstraction, increase automation, and bring more flexibility when synthesizing and exploring software variants. In a sense, generative AI meets generative programming. 

![](/assets/generativeAIPorgramming.png)

I then demonstrated how to transform an unfamiliar code (written in TikZ, and without comments) into something variable and configurable.
A fun session with a fixed cat written in TikZ that is gradually made configurable.

![](/assets/catTikzVariation.png)


Sadly, I haven't the time to present how to program variability in the context of reproducibility and floating-points. 
With a few prompts, you can vary types of a variable, make vary flags of a compiler (eg gcc or clang), and synthesize a generator that compiles and executes all variants, while storing all results in a CSV.  /VariantsAndLLM/

![](/assets/floatingPointVariation.png)


More details in the paper or in the appendix of slides or in the Github:
 * Preprint: https://inria.hal.science/hal-04153310/
 * Slides: https://slideshare.net/acher/on-programming-variability-with-large-language-modelbased-assistant
 * Git: https://github.com/acherm/progvary-withgpt/

These are early results, and there are many open challenges and questions. 
I got great feedbacks and suggestions in the reviews or at the conference. 
This paper differs from the [other I presented about reengineering variants](/VariantsAndLLM/): here we didn't start with variants in the first place, we aim to program and create them! 
I am very excited by this research direction, i.e, how **ChatGPT and LLM-based assistant can help exploring variants' space with software!**  

Abstract: 
> Programming variability is central to the design and implementation of software systems that can adapt to a variety of contexts and requirements, providing increased flexibility 
> and customization. Managing the complexity that arises from having multiple features, variations, and possible configurations is known to be highly challenging for software 
> developers. In this paper, we explore how large language model (LLM)-based assistants can support the programming of variability. We report on new approaches made possible with
> LLM-based assistants, like: features and variations can be implemented as prompts; augmentation of variability out of LLM-based domain knowledge; seamless implementation of
> variability in different kinds of artefacts, programming languages, and frameworks, at different binding times (compile-time or run-time). 
> We are sharing our data (prompts, sessions, generated code, etc.) to support the assessment of the effectiveness and robustness of LLMs for variability-related tasks.


  







 















*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2023chatgptvariability,
  author = {Mathieu Acher},
  title = {ChatGPT for Programming Variability and Variants},
  year = {2023},
  month = {aug},
  howpublished = {\url{https://blog.mathieuacher.com/ChatGPTVariability/}},
  note = {\url{https://blog.mathieuacher.com/ChatGPTVariability/}}
}
```
