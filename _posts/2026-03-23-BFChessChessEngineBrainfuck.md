---
layout: post
title:  "BFChess: A Chess Engine in Brainfuck, Built by a Coding Agent"
date:   2026-03-23 14:00:00 +0100
tags: [chess, chess engine, LLM, coding agents, Brainfuck, esoteric languages, software engineering, generative AI, llm4code, Claude Code]
redirect_from:
  - /BrainfuckChessEngine/
---

[BFChess](https://github.com/acherm/agentic-chessengine-brainfuck) is a UCI-compatible chess engine written entirely in [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck). The generated engine is 5.6 MB of raw Brainfuck code (eight distinct characters: `><+-.,[]`), produced by a 7,400-line Python compiler. Here is what a small excerpt of the engine looks like:
![A snippet of chess.bf: 5.6 MB of raw Brainfuck code made of eight characters](/assets/bfchess-snippet.png)
It implements depth-3 minimax search with alpha-beta pruning, full move generation (including castling, en passant, and promotion), and MVV-LVA evaluation with positional bonuses. It passes 11/11 perft validation positions. It is not strong: it beats random moves convincingly but loses every game against Stockfish at minimum settings. Each move takes between 45 seconds and 10 minutes to compute. The project shows that coding agents can operate in Brainfuck at a scale never achieved before for a chess engine, not through raw translation but by designing intermediate abstractions (a pointer-tracking emitter, a memory layout manager, runtime loop patterns) that make the task tractable. Yet despite a non-trivial implementation of search, evaluation, and special moves, the resulting engine is only capable of beating a random move generator, with a large gap to even the weakest conventional engines. Closing that gap, through human-AI co-design iteration, is an open and exciting direction.

Brainfuck has no variables, no functions, no arithmetic beyond increment/decrement, and no random memory access. Its only data structure is a linear tape of byte-sized cells. It is Turing-complete, so in theory one can write a chess engine in it. In practice, no one appears to have done so before (see [Novelty](#novelty-has-this-been-done-before) below).

This work is part of a broader experiment where I ask coding agents to build chess engines from scratch across [many programming languages](https://blog.mathieuacher.com/FromScratchChessEnginesPolyglot/). The [TeX engine](https://blog.mathieuacher.com/TeXCCChessEngine/) plays at roughly the level of a casual tournament player. Brainfuck is a significantly harder target, and the results reflect that.

## Novelty: Has This Been Done Before?

A fully functioning, openly available chess engine written in Brainfuck does not appear to exist in durable public form as of February 2026. The best-documented serious attempt was a 2019 community project announced on [TalkChess](https://talkchess.com/), with an intended design described as "a simplified micro-Max" and an initial GitHub repository named *chessfuck*. The repository link publicly shared in 2019 is now dead (HTTP 404), strongly suggesting deletion or privatization; the surviving technical record is limited to the TalkChess discussion thread itself.

What does exist, with primary sources, are (a) chess-adjacent Brainfuck utilities such as `chessboard.b`, which renders an ASCII chessboard from a FEN position string (Daniel B. Cristofani, brainfuck.org), and (b) small-board game implementations with AI (notably tic-tac-toe), which demonstrate the kinds of state encoding, control-flow tricks, and sentinel-based "subroutine" patterns that would be required to scale toward chess-like complexity.

Two "enablers" appear repeatedly in the chess-in-BF discussion: speed improvements via optimized interpreters and JITs (e.g., Eli Bendersky's 2017 BF JIT series), and hardware BF execution (e.g., a 256-core BF processor paper). These do not solve the human-factor problem of writing and maintaining BF at chess-engine scale, but they do change feasibility assumptions around runtime performance.

BFChess appears to be the first complete, publicly available implementation.

## Why Brainfuck?

Because it represents an extreme point on the language-capability spectrum. If a coding agent can produce a working chess engine in Brainfuck, that tells us something about the boundaries of what these tools can handle in severely constrained language environments. Unlike TeX (which at least has integer registers and macro expansion), Brainfuck offers essentially nothing to build on. Every abstraction must be constructed from scratch, using only pointer movement and cell increment/decrement.

## Architecture: A Python-to-BF Compiler

BFChess is not hand-written Brainfuck. Instead, a Python compiler (`generate.py` and ~7,400 lines of supporting modules) emits the 5.6 MB `chess.bf` file. The compiler tracks a virtual data pointer at generation time, so it can emit minimal `>` and `<` sequences to navigate between memory cells. It is, in effect, a code generator targeting an extremely constrained instruction set.

| Module | Lines | Role |
|--------|------:|------|
| `bf_movegen.py` | 4,470 | Move generation, search, evaluation |
| `bf_chess.py` | 498 | Board ops, UCI position parsing, move application |
| `bf_memory.py` | 262 | Memory layout (672 cell addresses) |
| `bf_emitter.py` | 185 | Core BF emitter with pointer tracking |
| `bf_uci.py` | 184 | UCI protocol loop |
| `bf_primitives.py` | 182 | Control flow (`compare_eq`, `switch_on_value`) |
| `bf_io.py` | 108 | I/O (stdin line reading, output) |
| `generate.py` | 39 | Entry point |
| `bfi.c` | 177 | RLE-optimized BF interpreter |
| **Total** | **~7,400** | |

The generated `chess.bf` is **5.6 MB** of raw Brainfuck. An RLE-optimized interpreter (`bfi.c`) compresses it at load time from ~3.8M characters to ~520K instructions (7x compression), with a precomputed jump table for O(1) bracket matching.

### Memory Layout

The engine uses 672 cells on the BF tape, each with a carefully assigned role:

```
 Cells 0-15:      Temporaries
 Cells 16-29:     Game state (side to move, castling rights, king positions, EP file)
 Cells 23-28:     Move generation hot cells (BEST_FROM, BEST_TO, HAVE_LEGAL)
 Cells 30-93:     Chess board (64 squares)
 Cells 100-227:   UCI input buffer (128 bytes)
 Cells 120-129:   Legality workspace (make/unmake)
 Cells 130-159:   Evaluation (scoring, attack detection, exchange, check)
 Cells 160-183:   Depth-2/3 search control
 Cells 400-471:   Depth-2 state backup (64 board + 8 state cells)
 Cells 600-671:   Depth-3 state backup
```

### The Fundamental Problem: No Random Access

Brainfuck has no `board[index]` where `index` is a runtime value. To read a square whose index is computed at runtime, the engine must execute a **64-way switch**: compare the index against 0, 1, 2, ..., 63 and copy from the matching fixed cell. This single constraint is the dominant cost driver. Every board read or write is ~20 KB of BF code: a 64-way cascade of comparisons and pointer movements.

This is what makes Brainfuck fundamentally different from TeX (which has 32K indexed `\count` registers) or even [M&M's language](https://blog.mathieuacher.com/CodingAgentsMnMLang/) (which at least has named variables).

## Features

Despite the language constraints, BFChess implements:

- **Depth-3 minimax search** with alpha-beta pruning and captures-first move ordering
- **Full move generation** for all piece types, including castling (kingside/queenside), en passant, and promotion
- **MVV-LVA evaluation** with positional bonuses: center control, piece activity, pawn advancement, check detection, exchange awareness, anti-repetition penalty
- **3-pass attack analysis** for legality checking, destination safety, and check detection, with zero code duplication (a single `is_attacked()` routine wrapped in a 3-iteration BF loop)
- **Stalemate detection** at all search depths
- **Perft validation**: 11/11 positions correct (including castling and EP edge cases)
- **UCI protocol**: `uci`, `isready`, `position startpos`, `domove`, `go`, `perft`, `quit`

### Size Optimization: From 119 MB to 5.6 MB

The naive approach (unrolling every board access at compile time) produced a 119 MB BF program. Four optimization strategies brought it down:

1. **Runtime loops** instead of compile-time unrolling (117 MB -> 1.7 MB)
2. **Runtime offset dispatch** for knight/king/sliding piece movement (1.7 MB -> 282 KB)
3. **Cell proximity**: placing frequently co-accessed cells near each other to minimize pointer travel
4. **Efficient encoding**: values >128 use decrements from 0 (2 characters) instead of 128+ increments

The depth-1 engine was 943 KB. Adding depth-2 and depth-3 search (each requiring full 72-cell state save/restore) brought the final size to 5.6 MB.

## How Strong Is It?

Short answer: not very. But it plays legal chess and beats random moves convincingly.

### vs Stockfish 18 (minimum settings)

```
Results: +0 =0 -10 / 10 (0%)
All 10 losses by checkmate (12-32 moves)
```

BFChess captures material and controls the center in the opening, but 3 plies of search is not enough to see the tactical threats Stockfish exploits. Here is a typical game:

```
1. d4 d5 2. e4 c6 3. Nc3 a6 4. Qf3 dxe4 5. Nxe4 Nd7 6. d5 Qa5+
7. Bd2 Qxd5 8. Nf6+ exf6 9. Qxd5 cxd5 10. Bxa6 ...
```

BFChess plays reasonable opening moves (d4, e4, Nc3) and grabs material when it can, but lacks the ability to defend against multi-move threats.

### vs Random Legal Moves

```
Results: +4 =6 -0 / 10 (70%)
4 wins by checkmate, 6 draws by stalemate, 0 losses
```

It dominates random play in the opening/middlegame, but 60% of games end in stalemate: the engine captures everything and accidentally traps the bare king. Stalemate positions forming beyond the 3-ply horizon remain invisible.

### Summary

| Opponent | Score | Win% |
|----------|-------|------|
| Random legal moves | +4 =6 -0 | 70% |
| Stockfish (minimum settings) | +0 =0 -10 | 0% |

The sample sizes are too small (10 games each) and the win rates too extreme (0% and 70%) for a meaningful Elo estimate. What we can say: BFChess is clearly stronger than random play, but clearly weaker than even the weakest Stockfish. For context, the [TeX engine](https://blog.mathieuacher.com/TeXCCChessEngine/) plays at roughly the level of a casual tournament player, with quiescence search and piece-square tables. The gap is real and largely comes from depth and evaluation quality.

## What Would It Take to Improve?

The main weaknesses are clear:

- **No quiescence search**: the engine can't see beyond the fixed 3-ply horizon, so it routinely walks into tactical threats
- **No piece-square tables**: evaluation is crude (center bonus + MVV-LVA), without the positional nuance that piece-square tables provide
- **No iterative deepening or time management**: every position gets the same fixed-depth search, whether it takes 45 seconds or 10 minutes
- **No opening book or endgame tablebase**
- **Stalemate blindness**: a major practical issue against weaker opponents

Adding quiescence search would likely have the biggest impact, but in Brainfuck, every additional search depth multiplies code size (each depth level needs its own 72-cell state backup region) and execution time. A single move already takes 45 to 600+ seconds on the RLE-optimized interpreter. The challenge is not just algorithmic: Brainfuck's computational overhead makes deeper search extremely expensive.

There is also a meta-challenge: **assessing the engine is itself costly**. Each 10-game tournament against Stockfish takes several hours of wall-clock time. Iterating on the evaluation function or search algorithm means regenerating the 5.6 MB BF file, then waiting hours for results. The feedback loop is orders of magnitude slower than for a normal chess engine, where one can run thousands of games overnight.

## The Effort: Sessions, Tokens, and Cost

BFChess was built using [Claude Code](https://docs.anthropic.com/en/docs/claude-code) powered by Claude Opus 4.6. Here is what the development looked like:

| Metric | Value |
|--------|-------|
| Calendar span | 30 days (Feb 21 - Mar 23, 2026) |
| Active working time | ~34 hours |
| Sessions | 17 |
| User messages | ~1,973 |
| Total API calls | ~2,700 (main + subagents) |
| Total tokens | ~221M (mostly cache reads) |
| Estimated API cost | ~$758 (at standard Opus pricing) |
| Git commits | 10 |

The token count is dominated by cache reads (~198M tokens): Claude Code reloads context at each turn, so the same codebase gets re-read hundreds of times. The actual novel output is ~344K tokens. At standard Anthropic API pricing, that's about $758, though with a Claude Max subscription the cost structure is different (flat monthly fee).

### What Kind of Instructions Were Given?

The approach was deliberately iterative. I did not hand Claude Code a specification. Instead, the development went roughly like this:

1. **"Build a chess engine in Brainfuck"**: the initial prompt. Claude Code produced the first working UCI engine (733 KB) in the first session.
2. **"Add depth-2 search, then depth-3"**: progressive deepening, each requiring new state management.
3. **Bug reports from testing**: most of my messages were reporting bugs found during Stockfish tournaments (cell collisions, wrong flag cells, stale values). Claude Code would diagnose and fix them, sometimes discovering that a "fix" introduced a new cell collision.
4. **"Run perft and compare to python-chess"**: verification-driven development. Perft caught several move generation bugs early.
5. **"Add castling", "Add en passant"**: feature requests, each requiring extensive make/unmake support.
6. **"Play 10 games against Stockfish and show me the PGN"**: assessment loops.

The hardest bugs were **cell collisions**: multiple code paths accidentally writing to the same memory cell. In a normal language, you'd get a clear error. In Brainfuck, one routine silently corrupts another routine's state, and the symptom shows up moves later. Claude Code found and fixed at least 9 such bugs across the development, including one where the `print_char` function was using cell 30 (= `BOARD_START`) as scratch space, corrupting the board every time any text was printed.

### The Meta-Problem: Feedback Loop Cost

The real bottleneck was not writing the code, but evaluating it. Each iteration of "change something, regenerate chess.bf, play games, see if it improved" takes hours. Some representative times:

- Generating `chess.bf`: ~1-2 minutes
- One move from the engine: 45 seconds (opening) to 600+ seconds (complex middlegame)
- A 10-game tournament: 3-8 hours
- A complete test-fix-retest cycle: half a day

This means the engine has seen far fewer iterations than a normal chess engine development project would involve. There is almost certainly low-hanging fruit in the evaluation function, but finding it requires patience and compute.

## What Does This Show?

As a chess engine, BFChess has no practical value: it is too slow and too weak. But as an artifact, it raises several points worth noting.

**On language boundaries.** This appears to be the first publicly available chess engine in Brainfuck. Chess engines have been written in C, Java, Rust, JavaScript, COBOL, TeX, and many other languages, but the esoteric end of the spectrum was largely unexplored. BFChess shows that the language boundary for chess engines extends to extremely minimal Turing-complete languages. The bottleneck is not expressiveness per se (Turing-completeness guarantees that), but the practical cost of encoding complex state management (672 cells, zero collisions) and search algorithms (3-ply minimax with full board save/restore) in a language with no abstraction facilities.

**On coding agents.** Building this required Claude Code to manage a flat memory layout across ~7,400 lines of code generator, reason about pointer arithmetic and cell allocation, debug silent corruption bugs where one routine overwrites another's state, and optimize code size from 119 MB to 5.6 MB. The hardest class of bugs (cell collisions) would be difficult for a human to track manually at this scale, yet the agent found and fixed at least 9 of them across 17 sessions. At the same time, the agent could not independently assess the quality of its output: it needed the human to run tournaments, report failures, and guide the iteration. The division of labor was roughly: the agent writes and debugs code, the human steers priorities and evaluates results.

**On staying in pure Brainfuck.** A parallel attempt was made with [Codex](https://github.com/openai/codex) (GPT-5.3) to build the same kind of engine. The result is instructive: the repository contains real Brainfuck kernels, but upon inspection the live engine also relies on Python for search (`src/search.py`) and several board-operation fast paths (`src/bf_board.py`). The Brainfuck components (`src/brainfuck_selector.bf`) are kept for backward compatibility but are not the active engine core. In other words, the agent quietly migrated critical logic back into Python, presumably to optimize for convenience or speed. The constraint of staying in pure Brainfuck was not enforced explicitly enough, and the agent did not maintain it on its own. This is not a comment on the quality of Codex as an agent, but it illustrates two broader points: (1) keeping all engine logic in Brainfuck is genuinely difficult, and agents under pressure to produce working code will naturally gravitate toward more expressive languages when given the opportunity; (2) when working with coding agents on constrained tasks, the human must actively verify that the constraints are respected, because the agent may "solve" the problem by relaxing them. This pattern is not unique to Brainfuck: a similar drift was observed in the [printf-oriented programming experiment](https://blog.mathieuacher.com/PrintfOrientedProgrammingCodingAgents/), where agents tasked with implementing logic purely through `printf` format strings would occasionally fall back to conventional C code. In the BFChess project with Claude Code, the constraint held because the architecture (Python compiler emitting BF, with the engine running solely through `./bfi chess.bf`) made it structurally impossible to smuggle Python logic into the runtime. The Codex attempt is documented in [its own repository](https://github.com/acherm/agentic-chessengine-brainfuck-codexfailure) for those interested in the details.

**On feasibility and cost.** The project consumed ~221M tokens (~$758 at API pricing) and ~34 hours of active work over 30 days. Most of that time was spent on the feedback loop (regenerate, test, diagnose), not on writing code. This suggests that for Brainfuck-scale complexity, the main barrier to improvement is not the agent's coding ability but the cost of evaluation: each iteration takes hours of wall-clock time, limiting how many hypotheses can be tested.

**As a starting point.** The architecture (Python compiler to BF engine) and the infrastructure (perft tests, Stockfish tournaments, RLE interpreter) are all in place. Improving the engine from here is a matter of adding features (quiescence search, piece-square tables, better move ordering), which is difficult but incremental.

## Trying It

```bash
git clone https://github.com/acherm/agentic-chessengine-brainfuck
cd chess-brainfuck-cc
make                # builds bfi + generates chess.bf
printf 'uci\nisready\nposition startpos\ngo\nquit\n' | ./bfi chess.bf
# -> bestmove d2d4
```

You can play interactively with `python3 play.py` or run tournaments with `python3 play_stockfish.py`. Patience is required.

## Summary

| | BFChess | [TeXCCChess](https://blog.mathieuacher.com/TeXCCChessEngine/) |
|---|---|---|
| Language | Brainfuck | TeX |
| Engine size | 5.6 MB (raw BF) | ~2,100 lines TeX |
| Compiler | 7,400 lines Python | N/A (direct TeX) |
| Search depth | 3 (minimax + alpha-beta) | 3 (negamax + alpha-beta + quiescence) |
| vs Stockfish (min) | 0/10 | Competitive |
| Move time | 45-600+ seconds | 2-30 seconds |
| Development cost | ~$758 / 221M tokens / 34h | TBD |

BFChess is weaker, slower, and harder to improve than TeXCCChess. But it exists: a complete, legal-move-generating, UCI-speaking, depth-3-searching chess engine in a language with eight instructions and no memory abstraction. It is a solid starting point and, to the best of our knowledge, the first of its kind. Making it stronger will require more work, more compute, and likely some creative rethinking of what is feasible within Brainfuck's severe constraints.

*BFChess was developed by [Mathieu Acher](https://mathieuacher.com) and [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (Opus 4.6). Source code: [https://github.com/acherm/agentic-chessengine-brainfuck](https://github.com/acherm/agentic-chessengine-brainfuck)*
