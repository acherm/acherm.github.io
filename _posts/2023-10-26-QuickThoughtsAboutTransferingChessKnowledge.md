---
layout: post
title:  "Quick Thoughts about \"Bridging the Human–AI Knowledge Gap:
Concept Discovery and Transfer in AlphaZero\""
date:   2023-10-26 011:54:29 +0200
tags: [chess, AI, AlphaZero, artificial intelligence]
---

DeepMind released once again a fascinating paper (preprint: https://arxiv.org/abs/2310.16410) that develops new interpretability techniques to communicate chess concepts (extracted from [AlphaZero](https://blog.mathieuacher.com/MuZeroChess/)) to humans. Four top players, including Vladimir Kramnik and Maxime Vachier-Lagrave, improved chess puzzle solving after being exposed. Evidence is weak, but it's a signal of a possible revolution. Quick thoughts...

First, the paper is interesting beyond chess: distilling knowledge out of super-intelligent AI in teachable and actionable form is a super exciting topic, with many applications in science and complex domains. The paper is a step in that direction, with chess as a "testbed". Let's imagine domain=chemistry and super-intelligent AI=AlphaFold2. We are not there yet: not sure scientists will agree AlphaFold2 is super-intelligent, and not sure we're capable of extracting knowledge from it. But it's a fascinating direction! 

Second, we're touching scenarios I've early evoked with AlphaZero: people exposed to super AIs may have competing advantages. It's not really doping (that suggests cheating!), but who have the means to synthesize/leverage these systems can make huge differences. Are recent and excellent results of MVL related to the fact he has been exposed? I personally don't think so, but it's a fascinating question. 
And again, the applicability goes beyond chess! 

My two first points are positive, but I have some concerns about the paper.
In fact, evidence is weak: 
 * n=4 participants, all top grand-masters, not sure if it will transfer to common mortals (e.g., novices) 
 * many biases and uncontrolled factors about, e.g., conditions of puzzle solving. 
 * the first participant, despite being a top GM, originally resolved 0 puzzle... strange?!
 * number of puzzles is low: 36 is the total, it's unclear how much unseen puzzles used for testing have been used among 36. 
 * why 48 puzzles for the third participant and 36 for the others?
 * quick exposure to explanations ("Each grandmaster was asked to spend two hours on Phase 1, one hour on Phase 2 and two hours on Phase 3)
 * if participants knew puzzles are related to AlphaZero evaluation (and I think they did!), they may have been biased to solve them in a particular way. 
 * there is also the risk that puzzles are of a certain kind (e.g., tactical) and that participants benefit from good knowledge of this kind of puzzles. In a sense, puzzles really fit the knowledge! 

I know it's hard to carefully assess these factors, but I think it's important to be aware of them.

Another interrogation I have is about the previous exposures top players had. 
Such players have already been exposed to Stockfish with NNUE (and super-intelligent AIs). 
In fact, since the release of, e.g., Chessbase in the 1990s, we can consider the chess game has been revolutionized by super-intelligent AIs.
There has been entire books about dissecting AlphaZero games, such as [Game Changer](https://www.alphaechecs.fr/produit/game-changer) by Matthew Sadler and Natasha Regan. 
So, how much of the improvements is due to this new exposure? What do books fail to communicate? 
Section 6.2 and appendices are interesting in that regard, but I'm not sure if it's enough to answer the question. 

Besides, we have super-intelligent chess engines for a while now. Stockfish, without neural networks, was already super strong and actually full of AIs...
We had the Stockfish source code, and in a sense, the means to inspect its behavior. The proposed technique operates over the black box of AlphaZero, a less favorable scenario at first glance. 
What is fascinating is the ability to find unsupervised concepts out of this. 
Strangely speaking, at least to me and at the moment, is that it seems harder to achieve similar interpretability results with Stockfish, despite being a more white box. 

To summarize, the paper is a fascinating step in the direction of extracting knowledge from super-intelligent AIs, but I think it's important to be aware of the limitations of the study. 
There is a signal, and it's worth replicating the study with more participants and more conditions. 
Can it transfer to novices? Should chess players start by learning from AlphaZero in the first place? Can the interpretability techniques transfer to other domains? 
Right now it reads like super-intelligent teachers have been synthesized for chess... Extraordinary claims require extraordinary evidence. New era? Who wants this drug?  

(note: this blog used to deliver long content, but I'm trying to experiment with shorter posts, quick reactions, etc. A bit like in X/Twitter, but with more space and less noise. Let's see how it goes.)






























 







 







 















*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2023quickthoughtsabouttransferingchessknowledge,
  author = {Mathieu Acher},
  title = {Quick Thoughts about "Bridging the Human–AI Knowledge Gap: Concept Discovery and Transfer in AlphaZero},
  year = {2023},
  month = {oct},
  howpublished = {\url{https://blog.mathieuacher.com/QuickThoughtsAboutTransferingChessKnowledge/}},
  note = {\url{https://blog.mathieuacher.com/QuickThoughtsAboutTransferingChessKnowledge/}}
}
```
