---
layout: post
title:  "Coding Agents Build Chess Engines From Scratch in Rust, C++, COBOL, Rocq, LaTeX, Brainfuck, and More"
date:   2026-02-23 11:00:00 +0100
tags: [chess, chess engine, LLM, coding agents, programming languages, polyglot, Elo, benchmark, Rust, Java, COBOL, LaTeX, Rocq, Lean, Brainfuck, software engineering, generative AI, llm4code]
---

What happens when you ask coding agents to write a chess engine from scratch, with minimal guidance and you replicate the experiment across 12 programming languages: Rust? C++? COBOL?! Rocq!? LaTeX!!?? or even Brainfuck??!!
Over the past weeks, I have been running exactly this experiment. The short take-away: coding agents can now generate functional, UCI-compliant chess engines from scratch across a wide range of languages, some reaching over 2000 Elo. To my knowledge, this is the first time coding agents have been shown to produce non-trivial, end-to-end software of this complexity (with no architecture document, no step-by-step guidance) and across languages as diverse as Rust, COBOL, and LaTeX.
I couldn't find prior art for a full playing engine in LaTeX, Brainfuck, or Rocq ([formerly Coq; renamed with Rocq 9.0](https://rocq-prover.org/releases/9.0.0)), yet coding agents produced playable engines in all three. This is a *research preview* but the diversity of features, architectures, and performance is striking and raises many questions about coding agents' capabilities and programming languages.


## The Setup

The experiment is simple in principle. Take two AI coding agents ([Claude Code](https://docs.anthropic.com/en/docs/claude-code) (Claude Opus 4.6) and [Codex CLI](https://github.com/openai/codex) (GPT-5.2-Codex, reasoning effort xhigh)) and ask each to write a chess engine from scratch: *I want to build a chess engine in X programming language... at the end, I want to test this chess engine and assess its Elo rating, typically by playing games against chess engines of "similar" levels*. No detailed specifications, no step-by-step plan, no architecture document.

I had to answer some questions throughout sessions, but tried to be as non-technical as possible, letting the coding agents follow their own roadmaps through trial and error. I may "push" coding agents to improve their chess engine, but in a very agnostic way like "please improve the engine's strength".

So far, I did this across 12 programming languages (there will be a thirteenth, because 13 is special), producing over 20 engines in total (some languages have variants from each agent):

- **Mainstream:** Rust, C++, C, Java, Python
- **Proof assistants / formal methods:** Rocq (formerly Coq), Lean 4, Why3/OCaml
- **Esoteric / unusual for chess:** COBOL, LaTeX, SQL, Brainfuck

## Protocol

**Intervention policy:** I occasionally performed technical fixes to install tools or resolve environment issues, then restarted the agent. I did not intervene on chess-specific algorithms, data structures, or evaluation strategies unless the agent explicitly asked for confirmation. No specific incentives were given beyond "improve Elo" or "fix bugs."

**Stopping criterion:** Sessions ended when Elo stabilized, when improvement stalled, or when the engine was feature-rich enough to play real, end-to-end games. For some languages (e.g., Brainfuck), the engine was too basic (even though "playable"), so I pushed the agent to reach a certain level of functionality (e.g., legal move generation, basic search). 

**Evaluation:** Gauntlet matches against Stockfish at five UCI_Elo levels (1320, 1500, 1700, 1900, 2100) with `UCI_LimitStrength=true`, `Hash=16`, `Threads=1`. Each engine played 50 games (10 per level). Elo estimates use a logistic model with inverse-variance weighting across levels. Note: [Stockfish's UCI_Elo calibration targets 120s+1s](https://official-stockfish.github.io/docs/stockfish-wiki/UCI-%26-Commands.html); at other time controls the mapping shifts, so our estimates are useful but not absolute.
**Openings:** 20 fixed positions (EPD). **Tournament tool:** cutechess-cli with draw adjudication (move 200, score ≤5cp for 20 moves) and resignation (score ≥900cp for 6 moves). About time controls: 10+0.1s for fast engines (C, C++, Rust, Lean, COBOL, Why3), 30+0.3s for moderate engines (Python, Java), 120+2s for slow engines (LaTeX, SQL).
We also ran a Swiss-system tournament among the engines (e.g., 10 rounds, 560 games total) to get a more direct comparison.

## A Glimpse at the Results

Based on gauntlet matches against Stockfish (50 games per engine, logistic/inverse-variance method, confidence intervals to be published), the engines cluster into three performance tiers:

**Around 2000 Elo and above (strong club to expert level):** JavaCC (Java, Claude Code) leads at ~2200, followed by RustCC (Rust, Claude Code), CppCodex (C++, Codex), RustCodex (Rust, Codex), ChessPy (Python), PureC (C, Claude Code), and PureCCodex (C, Codex), all in the ~1950–2200 range. These engines typically implement [bitboards](https://chessprogramming.org/Bitboards), [PVS](https://chessprogramming.org/Principal_Variation_Search) with [aspiration windows](https://chessprogramming.org/Aspiration_Windows), [null-move pruning](https://chessprogramming.org/Null_Move_Pruning), [LMR](https://chessprogramming.org/Late_Move_Reductions), [killer moves](https://chessprogramming.org/Killer_Move), and [transposition tables](https://chessprogramming.org/Transposition_Table).

**Between 1500 and 2000 Elo (intermediate to strong club level):** Claudius (C++, Claude Code), Why3CC (Why3/OCaml, Claude Code), JavaCodex (Java, Codex), and PyCC (Python, Claude Code) fall in the ~1700–1900 range. Why3Codex (Why3/OCaml, Codex) and CoboChess (COBOL, Codex) sit around ~1530–1560. These engines implement solid search and evaluation but with fewer advanced features.

**Below 1500 Elo (beginner to intermediate):** ChessRocq (Rocq/OCaml, Claude Code) at ~1450, [TeXCCChess](https://blog.mathieuacher.com/TeXChessEngine/) (LaTeX, Claude Code) at ~1280, SQLChess (SQL, Claude Code) at ~1120, and TeXCodex (LaTeX, Codex) at ~900. These engines are constrained by their languages' computational limitations but still play complete games.

A Java engine at ~2200 Elo. A COBOL engine at ~1530. A LaTeX engine at ~1280. Let that sink in.

*note: Elo estimates are approximate. Some results may be revised as evaluation continues. Data, game records (PGN), and analysis tools will be made available*

## What's Remarkable

I couldn't find prior art for chess engines in several of these languages. To the best of my knowledge, nobody has written a chess engine in LaTeX, Brainfuck, or Rocq before. These are not natural choices for a computationally intensive, stateful program (LaTeX is a typesetting macro language, Rocq is a proof assistant, Brainfuck has 8 instructions and no data structures). Yet coding agents produced functional, playable engines in all three.

Most engines handle the core rules and pass a rule-based test suite. Engines in mainstream languages (Rust, C++, C, Java, Python) handle castling, en passant, promotion, and draw rules, and pass rule-based test suites. For more constrained languages (Brainfuck, SQL), correctness coverage varies: the engines are playable but may not handle every edge case. Evidence of correctness comes from rule-based test suites, [perft](https://chessprogramming.org/Perft) testing, and hundreds of tournament games with very low error rates. Draw rules and time control handling vary by engine and are documented per engine. Engines in Rust, C++, C, and Java implement features like bitboards, PVS, null-move pruning, LMR, killer moves, [SEE](https://chessprogramming.org/Static_Exchange_Evaluation), and transposition tables.

The engines exhibit a wide range of feature sets. Some use bitboard representations with magic move generation; others use [0x88](https://chessprogramming.org/0x88) mailbox arrays, TeX `\count` registers, or 64-character SQL strings. Search features range from depth-1 in Brainfuck to depth-25 PVS with aspiration windows in Rust. Evaluation functions range from material-only to [PeSTO](https://chessprogramming.org/PeSTO%27s_Evaluation_Function) [piece-square tables](https://chessprogramming.org/Piece-Square_Tables) with pawn structure, mobility, and king safety terms. One engine (CppCodex) even generated its own [Texel tuning](https://chessprogramming.org/Texel%27s_Tuning_Method) pipeline to optimize evaluation weights via machine learning.

Language impacts performance and expressiveness. On the same machine, using the same harness, the fastest engine is roughly four orders of magnitude faster than the slowest (131M nodes/sec in C++ at perft vs. 4,200 in SQL, though node counting is not strictly comparable across engines with different move generators). Rust and C++ engines reach depth 22–25 in 10 seconds; COBOL reaches depth 9; SQL is limited to depth 2. The language also shapes what the agent writes: bitboards in C/Rust, numeric masks in COBOL (which lacks bitwise operators), `\count` registers in LaTeX, recursive CTEs in SQL. Python engines reach ~2000 Elo despite being ~600x slower than C++ in raw perft, showing that algorithmic quality can compensate for raw speed to some extent.

These are not mere copies. Even within the same language, different agents produce engines with different architectures and feature sets. The agents select and combine standard chess programming techniques, adapting them to each language's constraints. This is synthesis, not regurgitation.

A chess engine is non-trivial software: it involves bitwise arithmetic, recursive search with pruning, hash tables, time management, many heuristics, some chess knowledge, and a communication protocol. The agents made all architectural decisions themselves, debugged their own code, wrote their own test suites, and gradually improved their strength (sometimes I had to push the agents). Generating 20 engines required hundreds of AI coding sessions, tens of thousands of interaction turns, and roughly $680 in API costs. Building a chess engine (even with an AI agent) is neither free nor trivial, and it sounds like an interesting milestone/challenge (and benchmark) to explore further. It also raises many questions related to coding agents' abilities and programming languages.

There has been a growing effort to [assess the Elo rating of LLMs playing chess directly](https://blog.mathieuacher.com/GPTsChessEloRatingLegalMoves/), i.e., evaluating how well a model can pick the next move in a position. This experiment suggests a complementary perspective: instead of asking an LLM to *play* chess, ask it to *write code* that plays chess. The Elo of the resulting engine becomes an indirect but meaningful measure of the model's coding ability, chess understanding, and capacity to integrate both. Of course, at the far end of the spectrum, an agent that simply shells out to Stockfish wouldn't tell us much; the challenge is building the engine from scratch, which is what makes the exercise interesting as a benchmark.

## What's Next

This blog post is a *research preview*. There is much, much more to share:

- In-depth analysis of each engine's architecture and the development process
- Head-to-head tournaments (e.g., we ran a Swiss-system tournament, 10 rounds with Swiss pairing across engines, 560 games total)
- Benchmark comparisons (perft speed, search depth under time control)
- AI session analysis (how many turns, tokens, and dollars each engine took to build)
- How far we can push toward state-of-the-art chess engines

Starting soon, I'll publish a series of blog posts (roughly one every three days) each diving deep into a specific engine. The first installment will cover [TeXCCChess](https://blog.mathieuacher.com/TeXChessEngine/), the LaTeX chess engine, because frankly, a chess engine that runs inside `pdflatex` deserves its own post.
It has never been done, and coding agents open a new path towards more possibilities and questions. There will be a thirteenth programming language, because 13 is special; and it will be a language that is both suited and challenging for chess programming.
A detailed report covering the full methodology, data, and analysis will also follow.

A year ago, having a coding agent produce a single working chess engine was possible, but required significant planning and technical intervention. Today, two agents produced over 20 engines across 12 languages, some in languages where no chess engine existed before, with no architecture document and minimal guidance. The frontier of what coding agents can build end-to-end has moved considerably. 

As a personal note, this experiment is a fascinating journey through programming languages, chess programming techniques, and the capabilities of coding agents. I could not imagine, when starting chess or programming, that I would one day see "agents" build chess engines in languages as diverse as Rust, COBOL, and LaTeX, in almost one-shot. It is also a strong signal that coding agents are changing the landscape of software engineering and science. 

Stay tuned (this post might be updated), and feel free to contact me (mathieu.acher@irisa.fr)


*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2026fromscratchchessenginespolyglot,
  author = {Mathieu Acher},
  title = {Coding Agents Build Chess Engines From Scratch in Rust, C++, COBOL, Rocq, LaTeX, Brainfuck, and More},
  year = {2026},
  month = {feb},
  howpublished = {\url{https://blog.mathieuacher.com/FromScratchChessEnginesPolyglot/}},
  note = {\url{https://blog.mathieuacher.com/FromScratchChessEnginesPolyglot/}}
}
```
