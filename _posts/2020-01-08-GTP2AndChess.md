---
layout: post
title:  "GPT-2 and Chess"
date:   2020-01-08 011:54:29 +0200
tags: [artificial intelligence, chess, learning, natural language, gpt2]
---

Shawn Presser has [released](https://twitter.com/theshawwn/status/1214013710173425665) an intringuing chess engine based on deep learning-based language model (GPT-2). The model was trained on the Kingbase dataset (3.5 million chess games in PGN notation) in 24 hours using 146 TPUs (ouch!). The engine is purely based on *text* prediction with no concept of chess. Though GPT-2 has already delivered promising/bluffing results for text generation, one can be skeptical and wonder whether it does work for chess.

I have quickly experimented thanks to a nice notebook where you can play chess against the GPT-2 engine.
I've only played 6 games, but (surprinsingly) it's enough to draw some conclusions.
Let's analyse them case by case, following the order of the played games:
 * in the [first game](https://lichess.org/HLyQkZz7) I started with a rare/stupid opening just to see what's going on... GPT-2 has been trained on a corpus of games: it can excel for the very first moves (the opening) since many examples are available, especially for positions after 1.e4 or 1.d4 the most frequent moves. Despite 1.e4 e5 2. Bc4 Nf6 3.d4 (an infrequent line/sequence of moves), GPT-2 played OK. But quickly it became really bad and mate in 14 moves by playing quite normal.
 * second game is in the same spirit (see [here](https://lichess.org/eCxIwD2t)) I started with an infrequent move (1.b4), first moves of GPT-2 are OK, and then strong mistakes and mate in only 15 moves by playing as usual (as best as I can).
 * third was perhaps [more interesting:](https://lichess.org/VGsWKdY2) Quite classical opening this time with a spanish variation... Blunder after 12 moves (presumably since GPT-2 is out of the usual opening theory) and quickly winning for white
 * [game](https://lichess.org/25E56BCz): blunder 9th move directly after the "end" of the opening... another big mistake at 24th move in a desperate position. Pattern: GPT-2 seems very OK at the beginning and then quickly does mistakes/blunders. What is impressive is that the played moves of GPT-2 are legal even at move 30: it's truly remarkable since the engine has absolutely no knowledge about the rules of chess. And the kind of positions after move 15 (says) are clearly not in the corpus used to train the machine 
 * for the two remaining games, I've tried a specific strategy to win as quickly as possible. Here I didn't try to play as usual and in a perfect absolute way, but rather to exploit the "design" of GPT-2. First attempt: [mate in 12 moves](https://lichess.org/LmcIDaI9) and second one [mate in 8 moves](https://lichess.org/pG4S7RcF) I've voluntarily used weird/incorrect moves to minimize the number of moves to check mate... A bit like when you're playing against a beginner and you try the [coup du berger](https://fr.wikipedia.org/wiki/Coup_du_berger) (aka Scholar's mate in english, thanks Rick Rabiser!)

The general conclusion is that GPT-2 is very bad at chess: you can check mate in a few moves and against a "normal" player GPT-2 has no chance to even draw a position. Against Stockfish, AlphaZero or MuZero, the score would be 100000000 defeats in as many games. Nevertheless I'm quite impressed GPT-2 can play legal moves even at move 30: it's not in the corpus and the chess rules seem to have been learned purely out of texts games.
Future work may include:
 * comparison of GPT-2 with other language-based approaches (eg based on less complex methods like ngram) or with other textual inputs (eg why not using FEN notation at each move/position?)
 * rigorous assessment of the ability of GPT-2 to play legal moves. I only played 6 games with 30 moves maximum, it's worth trying on much more games eg for ending positions with only a few pieces or around move 40.
 * what's the minimum number of moves needed to check mate GPT-2? 

GPT-2 based chess engine is a fun experiment that may serve to further question and understand the current limits of artificial intelligence. Yea, GPT-2 has not learned the complex meaning of chess, but it has learned something purely out of texts, which is somehow bluffing (closed to [ELIZA effect](https://en.m.wikipedia.org/wiki/ELIZA_effect)). The brual approach of GPT-2 can be applied to many domains (music, poetry, recipes, etc.), since there is textual data. I guess the resulting effectiveness (ELIZA effect?) may highly vary and is worth trying, but for the chess domain some "pieces" in the learning process are missing ;)









*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2020gtp2andchess,
  author = {Mathieu Acher},
  title = {GPT-2 and Chess},
  year = {2020},
  month = {jan},
  howpublished = {\url{https://blog.mathieuacher.com/GTP2AndChess/}},
  note = {\url{https://blog.mathieuacher.com/GTP2AndChess/}}
}
```
