---
layout: post
title:  "Linus Torvalds on LLM"
date:   2023-11-05 011:54:29 +0200
tags: [LLM, generative AI, linux, programming]
---

Great interview of Linus Torvalds on many topics (Rust, git, etc.) and also on LLM and programming tasks: https://www.youtube.com/watch?v=OvuEYtkOH88 (starting at 20:30). Here is the transcript (using `yt-dlp` and OpenAI Whisper):

Dirk Hohndel: I typically say artificial intelligence is autocorrect on steroids, because all a large language model does is it predicts what's the most likely next word that you're going to use, and then it extrapolates from there. So not really very intelligent. But obviously, the impact that it has on our lives and on the reality we live in is significant. Do you think we will see LLM written code that is submitted to you as a request? 

Linus: I'm convinced it's going to happen, yes. And it may well be happening already, maybe on a smaller scale, where people really use it more as a help in writing code. But it's clearly something where automation has always helped people write code. I mean, this is not anything new at all. We don't write machine code anymore. We don't even write Assembler. And now we're moving on from C to Rust. So I don't see this as something as revolutionary as all the news talk is every day about AI. It's not an area I obviously work with. I'm still very low level. I got into kernels because I love the low-level hardware details. And that's why I'm still there. 

Dirk: But so you say you expect this can help people write code. This can help people get started. But then if we look at the previous conversation and the challenges around code reviews and maintaining, so do you think that large language models will get to the point that they can help us review code, that they can help maintain subsystems? 

Linus: I hope. I hope, because that's certainly one of the areas which I see them really being able to shine, to find the obvious stupid bugs. Because I mean, how many people here are actually programmers in this room? A lot. So a fair number. A lot of the bugs, ivory, right. A lot of the bugs I see other people write, they're not like subtle bugs. A lot of them are just the stupid bugs that you did not think about. And you don't need any kind of higher intelligence to find them. But having tools that warn, I mean, we have compilers that warn about the obvious, really obvious ones. But having LLMs that warn about slightly more subtle cases where it may just say, this pattern does not look like the regular pattern. Are you sure this is what you mean? And the answer may be, no, that was not at all what I meant. You found an obvious bug. Thank you very much. So I do think that LLMs are going to be a big, you call them disparagingly, like autocorrects on steroids. And I actually think that they're way more than that. And how most people work is, we all are autocorrects on steroids to some degree. And I see that as a tool that can help us be better at what we do. But I've always been optimistic. The whole hopeful, what's the word? 

Dirk: Yes, yes, helpful, hopeful, and humble. 

Linus: Hopeful and humble, that's my middle name. 
Linus: But on the other hand, I mean, I have been so optimistic that 32 years ago, I was stupid enough to think that you can write a better kernel than anybody else. So you have to kind of be a bit too optimistic at times to make a difference. So my approach to LLMs really has been that, hey, this is wonderful. This is going to. 

Dirk: I love seeing the optimism. I don't necessarily share it. 

Linus: Now, a lot of people disagree with me. 

Dirk: But one of the things that I worry about in all this is, we see the hallucinations. And that's a technical term for LLMs. They do hallucinate. And they do make up stuff. And so the more they are being put into the position where they will automatically do things without an actual human being there to catch them, the more this becomes scary. Not scary as in they will rule the world and not in the sci-fi sense, but in the so many bugs that will happen and that will affect our lives or our code. 

Linus: Well, I see the bugs that happen without them every day. So that may be why I'm not so worried. I think we're doing those just fine on our own. 

Dirk: And I don't think I can end the AI topic on a better highlight. So let's go from there to a highlight.

I found it useful to have a transcript of the interview, so I am sharing it here. I hope it is useful to you too. 
Interestingly it reminds me of a [Slashdot interview](https://linux.slashdot.org/story/15/06/30/0058243/interviews-linus-torvalds-answers-your-question) where Linus said in early 2015:
"We'll get AI, and it will almost certainly be through something very much like recurrent neural networks. And the thing is, since that kind of AI will need training, it won't be "reliable" in the traditional computer sense. It's not the old rule-based prolog days, when people thought they'd *understand* what the actual decisions were in an AI."
Also interesting to read again this inspiring and impactful piece of Karpathy on [The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/) and his experiments on Linux source code (see eg https://cs.stanford.edu/people/karpathy/char-rnn/linux.txt). 

*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2023linustorvaldsllm,
  author = {Mathieu Acher},
  title = {Linus Torvalds on LLM},
  year = {2023},
  month = {nov},
  howpublished = {\url{https://blog.mathieuacher.com/LinusTorvaldsLLM/}},
  note = {\url{https://blog.mathieuacher.com/LinusTorvaldsLLM/}}
}
```
