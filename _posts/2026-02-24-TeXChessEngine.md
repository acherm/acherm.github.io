---
layout: post
title:  "TeXCCChess: How Coding Agents Wrote a Chess Engine in Pure TeX"
date:   2026-02-24 11:00:00 +0200
tags: [chess, chess engine, LLM, coding agents, LaTeX, TeX, pdfLaTeX, Elo, software engineering, generative AI, llm4code, Claude Code]
---

What happens when you ask a 2026 coding agent like Claude Code to build a chess engine from scratch (with no plan, no architecture document, no step-by-step guidance) in a language that was never designed for this purpose? Building a chess engine is a non-trivial software engineering challenge: it involves board representation, move generation with dozens of special rules (castling, en passant, promotion), recursive tree search with pruning, evaluation heuristics, as well as a way to assess engine correctness and performance, including Elo rating. Doing it from scratch, with minimal human guidance, is a serious test of what coding agents can do today. Doing it in LaTeX's macro language, which has no arrays, no functions with return values, no convenient local variables or stack frames, and no built-in support for complex data structures or algorithms? More than that, as far as I can tell, it has never been done before (I could not find any existing TeX chess engine on CTAN, GitHub, or TeX.SE). Yet, the coding agent built a functional chess engine in pure TeX that runs on `pdflatex` and reaches around 1280 Elo (the level of a casual tournament player). This post dives deep into how this engine, called TeXCCChess, works, the TeX-specific challenges encountered during development. You can play against it in [Overleaf](https://www.overleaf.com/docs?snip_uri=https%3A%2F%2Fgithub.com%2Facherm%2Fagentic-chessengine-latex-TeXCCChess%2Freleases%2Fdownload%2Fv1.0%2FTeXCCChess.zip&engine=pdflatex&main_document=chess-game.tex) (see demo https://youtu.be/ngHMozcyfeY) or your local TeX installation https://youtu.be/Tg4r_bu0ANY, while the source code is available on GitHub https://github.com/acherm/agentic-chessengine-latex-TeXCCChess/

## Motivation


Among all the engines produced in my experiment of [asking coding agents to build chess engines from scratch across 12 programming languages](https://blog.mathieuacher.com/FromScratchChessEnginesPolyglot/), TeXCCChess is perhaps the most surprising and delightful. It is a chess engine written entirely in TeX (the macro language behind LaTeX) and it runs on `pdflatex`. 
Yes, LaTeX is usually used for writing reports or  scientific papers, not at all for writing chess engines. Between, why TeX?
The honest answer: because it shouldn't work and has never been done before. TeX has no arrays, no functions with return values, no convenient local variables or stack frames, no integers bigger than 2^{31}-1, no bitwise operations. Macro expansion can recurse, but you get no call stack and deep recursion quickly hits engine limits. What TeX does have is a Turing-complete macro expansion engine and, with e-TeX extensions (used by modern pdfTeX), up to 32,768 integer registers called `\count`. That turns out to be *just barely enough* to implement a chess engine.
I could not find any prior chess engine in TeX (I searched CTAN, GitHub, and TeX Stack Exchange). There exist LaTeX packages for *rendering* chess positions and games (the excellent `chessboard` and `xskak` packages), but nothing that actually *plays chess*. TeXCCChess appears to be the first.

If this is indeed the first full TeX chess engine, it is very unlikely the model memorized one verbatim. That makes TeXCCChess a useful counterpoint to the plausible critique that "coding agents just regurgitate training data". Of course, the model may have seen discussions about chess programming in TeX, or small macro-expansion tricks, but a full working engine is a different artifact. (We will see in other posts that for engines in more mainstream languages, the pure memorization hypothesis is also questionable given the diversity of architectures and features, but here it is even more so).

TeXCCChess was built by [Claude Code](https://docs.anthropic.com/en/docs/claude-code), Anthropic's agentic coding tool, powered by Claude Opus 4.6. The "CC" in the name stands for Claude Code (and if you squint, an allusion to the Chaos Computer Club). A second variant was independently built by [Codex CLI](https://github.com/openai/codex) (powered by GPT-5.2), but it is out of the scope of this post (I will cover it in a future post, along with engines in other languages). 

The challenge I gave Claude Code was deliberately vague: *I want to build a chess engine in LaTeX... at the end, I want to test this chess engine and assess its Elo rating, typically by playing games against chess engines of "similar" levels.* I did not specify the architecture, the search algorithm, or the data structures. The agent made all those decisions itself, discovered the TeX pitfalls the hard way, and iteratively debugged its own code across multiple sessions.

## The Architecture at a Glance

The engine is ~2,100 lines of pure TeX code in a single file (`chess-engine.tex`). Everything happens during `pdflatex` compilation. Here is the high-level architecture:

- **Board representation:** 64 TeX `\count` registers (`\count200` through `\count263`)
- **Piece encoding:** integers from -6 to +6 (positive = white, negative = black, 0 = empty)
- **Move generation:** pseudo-legal generation + legality filtering via make/unmake
- **Search:** depth-3 [negamax](https://chessprogramming.org/Negamax) with [alpha-beta pruning](https://chessprogramming.org/Alpha-Beta) + [quiescence search](https://chessprogramming.org/Quiescence_Search)
- **Evaluation:** material counting + [piece-square tables](https://chessprogramming.org/Piece-Square_Tables) ([Simplified Eval Function](https://chessprogramming.org/Simplified_Evaluation_Function))
- **UCI support:** a thin Python wrapper bridges `pdflatex` to the [UCI](https://chessprogramming.org/UCI) protocol

What emerges from this design is something resembling a **tiny virtual machine** built on top of TeX's macro expansion engine. The `\count` registers serve as RAM (with dedicated address ranges for the board (200-263), scratch computation (188-194), and the search call stack (10000+)). The `\csname` lookup tables act as a read-only ROM for precomputed data (file/rank mappings, piece-square tables, material values). Token lists (`\movelist`, `\legalmovelist`) serve as dynamically allocated buffers. Macros like `\makemove`/`\unmakemove` and `\pushstate`/`\popstate` are the instruction set. TeX's `\ifnum` and `\loop` primitives provide the control flow. The whole thing is a register machine with no stack frames, no heap, and no garbage collector (just flat integer registers and name-based indirection). pdflatex is, in effect, the CPU executing this VM.

The following diagram (generated by Codex during a code exploration session) shows how the components of this tiny VM connect (from the TeX macros through memory registers to the UCI bridge):

![TeXCCChess architecture: from TeX macros through memory registers to UCI bridge](/assets/texcc-architecture.png)
([Mermaid source](/assets/texcc-architecture.mmd))

And here is the full game flow (from the moment you call `\playmove{e2e4}` to the engine's response). It shows move parsing, pseudo-legal generation with piece-specific dispatchers, legality filtering via make/unmake + attack detection, and the search tree (depth-3 negamax cascading through searchB, searchC, and quiescence):

![TeXCCChess game flow: from playmove through move generation, legality filtering, and depth-3 search](/assets/texcc-gameflow.png)
([Mermaid source](/assets/texcc-gameflow.mmd))

## Board Representation: 64 Count Registers

In most languages, you'd use an array. In TeX, you use `\count` registers. The board occupies registers 200 through 263, one per square:

```tex
% Square index: (rank-1)*8 + file, where file a=1..h=8, rank 1..8
% So a1=1, b1=2, ..., h1=8, a2=9, ..., h8=64
% Stored in \count registers 200..263

\def\setsq#1#2{\global\count\numexpr199+#1\relax=#2\relax}
\def\getsq#1{\the\count\numexpr199+#1\relax}
```

Pieces are encoded as signed integers:

| Value | Piece |
|------:|-------|
| 1 | White Pawn |
| 2 | White Knight |
| 3 | White Bishop |
| 4 | White Rook |
| 5 | White Queen |
| 6 | White King |
| -1..-6 | Black equivalents |
| 0 | Empty square |

Setting up the starting position is just a series of register assignments:

```tex
\def\initboard{%
  % Clear all squares
  \count190=1\relax
  \loop\ifnum\count190<65
    \setsq{\the\count190}{0}%
    \advance\count190 by 1
  \repeat
  % White pieces (rank 1)
  \setsq{1}{4}%  a1 = wR
  \setsq{2}{2}%  b1 = wN
  \setsq{3}{3}%  c1 = wB
  \setsq{4}{5}%  d1 = wQ
  \setsq{5}{6}%  e1 = wK
  ...
}
```

A critical design decision is how to extract the file and rank from a square index. In most languages, you'd write `file = (sq - 1) % 8 + 1` and `rank = (sq - 1) / 8 + 1`. But TeX's `\numexpr` division **rounds** instead of truncating: `63/8` gives 8 (rounding 7.875 up), not 7. This was the source of one of the first nasty bugs. The fix: precompute lookup tables at load time using `\divide` (which truncates) and store results in `\csname` control sequences:

```tex
\count190=1\relax
\loop\ifnum\count190<65
  \count191=\count190\relax \advance\count191 by -1\relax % 0-based index
  \count192=\count191\relax \divide\count192 by 8\relax   % rank 0-based (TRUNCATING)
  ...
  \advance\count192 by 1\relax % rank 1-based (1..8)
  \advance\count194 by 1\relax % file 1-based (1..8)
  \expandafter\edef\csname sq@file@\the\count190\endcsname{\the\count194}%
  \expandafter\edef\csname sq@rank@\the\count190\endcsname{\the\count192}%
  \advance\count190 by 1\relax
\repeat
```

This pattern (precompute into `\csname` tables at load time, look up by name expansion at runtime) is used throughout the engine. It is the TeX equivalent of a hash map.

## Move Generation

Move generation follows the standard pseudo-legal approach: generate all possible moves, then filter out those that leave the king in check.

Each piece type has its own generator macro. For sliding pieces (bishop, rook, queen), ray-casting is done via recursive macro expansion along each direction, stopping when hitting a piece or the edge of the board. For knights, all 8 L-shaped jumps are checked with boundary validation. Pawns have the most complex logic: single pushes, double pushes from the starting rank, diagonal captures, en passant, and promotion.

Moves are stored in a TeX token list as `\moveentry{from}{to}{flags}`, where flags encode special moves:

| Flag | Meaning |
|-----:|---------|
| 0 | Normal move |
| 1 | En passant |
| 2 | Castling |
| 3 | Promotion (always to queen) |

Legal move filtering works by making each pseudo-legal move on the board, checking if the king is attacked, then unmaking the move. Castling gets special treatment: three squares must be checked (the king's origin, transit square, and destination) since the king cannot castle out of, through, or into check:

```tex
\def\fl@testcastling#1#2#3{%
  % King must not be in check now
  \findkingsquare{\the\sidetomove}%
  \issquareattacked{\the\kingsquare}{\the\fl@oppcolor}%
  \ifnum\attackresult=1 \fl@legal=0\relax \fi
  \ifnum\fl@legal=1
    % Transit square must not be attacked
    \ifnum#2>#1 \fl@transit=\numexpr#1+1\relax
    \else \fl@transit=\numexpr#1-1\relax \fi
    \issquareattacked{\the\fl@transit}{\the\fl@oppcolor}%
    \ifnum\attackresult=1 \fl@legal=0\relax \fi
  \fi
  ...
}
```

## The State Stack Problem

A chess engine needs to make a move, search deeper, then unmake the move. In a normal language, local variables or a call stack handle this naturally. TeX has grouping-based scoping, but the engine's design uses global state throughout (every `\count` register, every `\newcount` variable, every `\newif` flag is global).

The solution: a hand-rolled state stack using high-numbered `\count` registers (starting at `\count10000`) to avoid collisions with the `\newcount` allocator. Each search depth gets 9 register slots to save and restore the full game state:

```tex
% Depth 0: \count10000-\count10008
% Depth 1: \count10009-\count10017
% Depth 2: \count10018-\count10026
% Depth 3: \count10027-\count10035

\def\pushstate#1{%
  \count\numexpr10000+9*#1+0\relax=\save@captured\relax
  \count\numexpr10000+9*#1+1\relax=\save@castleWK\relax
  \count\numexpr10000+9*#1+2\relax=\save@castleWQ\relax
  ...
}
```

The ordering is critical and was a source of subtle bugs: `\pushstate` must be called *after* `\makemove` (which populates the `\save@*` registers) but *before* `\updategamestate` (which modifies them). Reverse for unmake: `\popstate` first, then `\unmakemove`. Getting this wrong produces moves that look almost right but occasionally corrupt castling rights or en passant state (the kind of bug that shows up once every 50 games).

## Search: Depth-3 Negamax with Alpha-Beta

TeX can recurse via macro expansion, but there is no call stack, no local variables, and deep recursion risks hitting engine limits. So the search uses three explicit loop levels, one for each depth:

- **loopA** (depth 0): iterates over our candidate moves
- **loopB** (depth 1): iterates over the opponent's replies
- **loopC** (depth 2): iterates over our counter-replies, capped at 12 moves for speed

Each depth level stores its moves in separate indexed `\csname` slots (`mm@0@mf@N`, `mm@1@mf@N`, `mm@2@mf@N`) because the inner search clobbers the global move-tracking variables. After returning from an inner search, the current move must be re-read from its csname storage:

```tex
\def\mm@loopA{%
  \ifnum\mm@idx>\mm@total\else
    \mm@curfrom=\csname mm@0@mf@\the\mm@idx\endcsname\relax
    ...
    \mm@searchB  % inner search clobbers mm@curfrom etc.
    \mm@score=-\mm@retval\relax
    % Re-read move (inner search clobbers curfrom/curto/curflags)
    \mm@curfrom=\csname mm@0@mf@\the\mm@idx\endcsname\relax
    \mm@curto=\csname mm@0@mt@\the\mm@idx\endcsname\relax
    ...
}
```

Alpha-beta pruning is implemented using `\ifnum` comparisons. The `>=` operator (for cutoff: `alpha >= beta`) requires a TeX idiom since there is no `\ifge` primitive:

```tex
\ifnum\mm@alphaB<\mm@betaB\relax\else
  \mm@cutoffBtrue   % alpha >= beta, prune
\fi
```

Each depth level has its own cutoff flag (`\ifmm@cutoffB`, `\ifmm@cutoffC`, `\ifmm@qcutoff`), which are TeX booleans declared with `\newif` at the top level (declaring them inside a macro causes a "already defined" error on the second call).

### Move Ordering

Good alpha-beta depends on examining the best moves first. The engine uses a 4-pass [MVV-LVA](https://chessprogramming.org/MVV-LVA) (Most Valuable Victim - Least Valuable Attacker) ordering:

1. Promotions + captures of queens/rooks
2. Captures of bishops/knights
3. Captures of pawns + en passant
4. Quiet moves

This is implemented by iterating the legal move list four times, each time extracting moves matching the current priority level. At depth C (the deepest full-width level), the move list is capped at 12 (since captures are sorted first, all captures are always searched even under the cap).

### Quiescence Search

At the leaves of the depth-3 search, a quiescence search extends the analysis for captures only. This prevents the [horizon effect](https://chessprogramming.org/Horizon_Effect) (e.g., stopping evaluation right after we hang a piece but before the opponent recaptures):

```tex
\def\mm@quiesce{%
  \evaluate
  \mm@standpat=\evalscore\relax
  \ifnum\mm@standpat<\mm@qbeta\relax
    \ifnum\mm@standpat>\mm@qalpha\relax
      \mm@qalpha=\mm@standpat\relax
    \fi
    \generatelegal
    \mm@copycapturesonly
    ...
  \else
    \global\mm@retval=\mm@qbeta\relax  % stand-pat cutoff
  \fi
}
```

## Evaluation

The evaluation function scans all 64 squares and sums material values plus piece-square table (PST) bonuses. The PSTs follow the well-known [Simplified Evaluation Function](https://chessprogramming.org/Simplified_Evaluation_Function) from the [Chess Programming Wiki](https://chessprogramming.org/): pawns are rewarded for advancing toward the center, knights prefer central squares, kings prefer the corners in the middlegame, etc.

```tex
\expandafter\def\csname piecemat@1\endcsname{100}%  pawn
\expandafter\def\csname piecemat@2\endcsname{320}%  knight
\expandafter\def\csname piecemat@3\endcsname{330}%  bishop
\expandafter\def\csname piecemat@4\endcsname{500}%  rook
\expandafter\def\csname piecemat@5\endcsname{900}%  queen
\expandafter\def\csname piecemat@6\endcsname{20000}% king
```

For black pieces, the PST is accessed via a mirror table (`sq@mirror@N`) that flips the board vertically, so the same white-perspective tables work for both sides.

The evaluation is computed from scratch on every call (no incremental updates). This is slow, but incrementality would require tracking piece-list data structures that are painful to manage in TeX's global-state model.

## TeX-Specific Pitfalls

Building a chess engine in TeX surfaced a collection of language-specific traps that Claude Code had to discover and work around:

**`\numexpr` division rounds, it does not truncate.** `\numexpr 63/8\relax` gives 8, not 7. This broke coordinate extraction (file/rank from square index) until the fix: use `\divide` with a register (which truncates), or precompute into lookup tables.

**Digits in a normal control sequence name are not part of the name.** `\ray@t1` is parsed as `\ray@t` followed by the digit `1`, not a single control sequence `\ray@t1`. (You *can* include digits via `\csname ray@t1\endcsname`, but `\def` doesn't work that way.) Solution: use letters instead of digits (`\ray@ta`, `\ray@tb`).

**The `@` character in macro names requires `\makeatletter`.** All internal macros use `@` in their names (e.g., `\eval@sq`, `\mm@alpha`). Without `\makeatletter`, TeX treats `@` as a regular character and the macros silently fail.

**`\newif` inside macros fails on the second call.** Since `\newif` creates a global flag, calling it again raises "already defined". All boolean flags must be declared once at the global level.

**Register collision is the TeX equivalent of variable shadowing bugs.** Since everything is global, a macro that uses `\count190` as a loop counter will silently corrupt any outer macro also using `\count190`. The engine reserves specific register ranges for specific purposes: 200-263 for the board, 188-194 for scratch math, 10000+ for the search state stack.

## Playing Against TeXCCChess

The source code, instructions, and all tooling are available on GitHub: [**acherm/agentic-chessengine-latex-TeXCCChess**](https://github.com/acherm/agentic-chessengine-latex-TeXCCChess). You can play interactively by editing a `.tex` file and recompiling with `pdflatex` (works locally and on [Overleaf](https://www.overleaf.com/docs?snip_uri=https%3A%2F%2Fgithub.com%2Facherm%2Fagentic-chessengine-latex-TeXCCChess%2Freleases%2Fdownload%2Fv1.0%2FTeXCCChess.zip&engine=pdflatex&main_document=chess-game.tex)), or run automated Elo tournaments against Stockfish via the UCI wrapper. The README covers everything: setup, interactive play, UCI mode, and tournament scripts.

Here is a demo of an interactive game against TeXCCChess in Overleaf (note: depth > 0 may take too much time for Overleaf's limits, so the engine is set to depth default=0 for this demo):

<iframe width="560" height="315" src="https://www.youtube.com/embed/ngHMozcyfeY" frameborder="0" allowfullscreen></iframe>

And here is a demo of an interactive game against TeXCCChess in local (here the engine is set to depth=3 negamax + quiescence):

<iframe width="560" height="315" src="https://www.youtube.com/embed/Tg4r_bu0ANY" frameborder="0" allowfullscreen></iframe>

## Strength and Elo

**Estimated strength: ~1280 Elo** (95% CI: ~1225–1345, measured against Stockfish at 120+1 time control, CCRL 40/4 scale).

All matches used Stockfish with `UCI_LimitStrength=true` at the **120+1s** time control, which is the time control Stockfish's `UCI_Elo` scale was calibrated against (anchored to [CCRL 40/4](https://www.computerchess.org.uk/ccrl/404/)). Test conditions: no opening book, single-threaded, on an Apple M3 Max. All game records (PGN) are available on [GitHub](https://github.com/acherm/agentic-chessengine-latex-TeXCCChess).

### Evaluation details

A **100-game baseline** against Stockfish set to 1320 Elo gave **45 wins, 7 draws, 48 losses** (48.5% score, estimated Elo ~1310). An **extended evaluation** (3 x 50 games against Stockfish at 1320, 1400, and 1500 Elo) produced a combined estimate of ~1260 Elo. A small number of games ended due to UCI protocol issues (illegal moves); after removing those, the estimate stabilizes at **~1285 Elo** (95% CI: ~1225–1345).

Overall, the engine performs around **1280–1300 Elo** at 120+1. For reference, this is roughly the level of a casual tournament player who has studied some openings and does not hang pieces in obvious ways. The engine loses convincingly to Stockfish above 1500.

The main limitations are:
- **Search depth**: capped at 3 plies + quiescence. Deeper search would require either much faster execution (highly challenging in TeX) or a time budget of minutes per move.
- **No opening book**: the engine starts from scratch every game.
- **Simplified evaluation**: material + PSTs only; no pawn structure, king safety, or mobility terms.
- **Always promotes to queen**: no under-promotion.

## The Development Process: 5 Sessions, 10 Days

The entire development of TeXCCChess is traceable through 5 Claude Code sessions spanning 10 days (Feb 13-23, 2026). It started with a single sentence: *"I want to implement a chess engine in LaTeX... and have the ability to play with this engine."* Here is what happened, session by session.

### Session 1: The Plan

Claude Code entered plan mode and designed a 17-step implementation across 4 files. No code was written yet, just the architecture: `\count` registers for the board, signed integers for pieces, token lists for moves, a test suite to validate correctness. 19 API calls.

### Session 2: Building the Random-Move Engine

The agent implemented the full plan in one session: board representation, move generation for all piece types (including castling, en passant, promotion), legal move filtering, attack detection, game-end detection, and a random move picker. 122 API calls.

The first compilation of the test suite: **3 PASS, 6 FAIL**. The culprit: `sqfile(64)` returned 0 instead of 8. The `\numexpr` rounding bug (`63/8` gives 8 in TeX, not 7) had silently corrupted every file/rank extraction. The agent rewrote coordinate math using precomputed lookup tables. After the fix: **all 23 tests pass**.

A notable design decision: the agent chose hash-based RNG seeding for Overleaf compatibility. Rather than writing state to auxiliary files (which Overleaf doesn't support well), the engine hashes all prior user moves to create a deterministic seed. This means recompiling the same `.tex` file always produces the same game.

I confirmed: *"working fine!"* on Overleaf after testing.

### Session 3: UCI & Elo Infrastructure

To measure Elo, the engine needed to speak the UCI protocol. The agent created `chess-uci.py` (the Python wrapper), `run-elo-test.sh` (tournament orchestration), and `pgn2latex.py` (PGN-to-LaTeX converter for game visualization). 140 API calls.

A tricky bug: `cutechess-cli` outputs Standard Algebraic Notation (SAN: `Nf3`, `O-O`, `exd5`) but the PGN parser only handled coordinate notation (`g1f3`). The agent had to build a full SAN resolver (disambiguating moves by file or rank, handling castling variants (`O-O` vs `0-0`), stripping check/mate annotations).

### Session 4: Depth-2 Minimax

The random-move engine was replaced by a depth-2 minimax with material evaluation. 157 API calls.

This session exposed the most insidious bug of the project: **register collision**. LaTeX's `\newcount` allocator had already reached register 360+, so the state stack at `\count300-317` overlapped with LaTeX-internal registers. The symptom was subtle: the minimax search appeared to work but produced wrong moves. The agent traced it to a corrupted board value (square 17 showing value 8 instead of the expected piece), identified the collision, and moved the entire state stack to `\count10000+`.

First Elo test: 10 games vs Stockfish 800, result: **1 win, 0 draws, 9 losses** (~500-600 Elo). The agent noted: *"depth-2 only takes 0.16 seconds, there's a massive time budget remaining."*

### Session 5: The Big Push (PSTs, Alpha-Beta, Depth-3, Quiescence)

This was the largest and most dramatic session: 2,650 lines of conversation, 212 API calls, 19 edits to `chess-engine.tex`, 8 test suite runs, 5 tournament attempts.

The session hit two API errors early on: *"Claude's response exceeded the 32000 output token maximum"* (the agent tried to write the entire PST + search implementation in one go). Recovery: *"I'll break this into smaller steps."*

The Elo progression within this single session tells the story:

| Stage | Result vs Stockfish 800 | Est. Elo |
|-------|------------------------|----------|
| Initial depth-3 (no cap) | Timeout on move 3, games crash | — |
| After alpha-beta cutoff fix (`>=` instead of `>`) | Still too slow | — |
| Depth-2 + quiescence only | 0W/2D/8L | ~420 |
| Depth-3 + AB + quiescence, capped at 12 moves | **5W/1D/4L** | **~835** |

The initial depth-3 implementation was a disaster: move 3 took 30 seconds in complex positions, hitting the UCI timeout. Games ended with *"illegal move: 0000"* (the timeout fallback). The agent spent over 30 turns debugging (fixing alpha-beta cutoffs (the TeX idiom for `>=` is `\ifnum X < Y \relax\else`, since there is no `\ifge` primitive), then pragmatically capping depth-C at 12 moves). Since MVV-LVA sorts captures first, all tactical moves are always searched even under the cap. Timing dropped to 0.5-3.5s per move.

Then a measurement bug: the agent set `UCI_Elo: 800` for Stockfish, but recent Stockfish versions enforce a minimum `UCI_Elo` around 1300-1350 (the exact floor depends on version and build) and silently clamp values below it. The "5.5/10 vs Stockfish 800" result was actually against a much stronger opponent. After correcting the configuration (verified by inspecting Stockfish's `uci` option output), a preliminary 100-game tournament vs Stockfish 1320 confirmed the engine was in the ~1280 Elo range. A more rigorous evaluation using the Stockfish UCI_Elo calibration time control (120+1s) is reported in the [Strength and Elo](#strength-and-elo) section above.

### Effort Summary

| Session | Goal | API Calls | % of total cost |
|---------|------|-----------|-----------------|
| 1 | Architecture plan | 19 | 2% |
| 2 | Random-move engine | 122 | 24% |
| 3 | UCI + Elo infra | 140 | 22% |
| 4 | Depth-2 minimax | 157 | 14% |
| 5 | PST + AB + D3 + quiescence | 212 | 38% |
| **Total** | | **~650** | **100%** |

In total: 5 sessions, ~650 API calls, ~53 pdflatex compilations, ~22 test suite runs. The engine grew from 0 to 1,342 lines (random moves) to 2,093 lines (depth-3 + quiescence), and from ~300 Elo to ~1280 Elo. Session 5 (the "big push" to depth-3 with quiescence) consumed the most resources (38% of the total), reflecting the difficulty of getting search, pruning, and move ordering right in TeX. The initial implementation (session 2) was the second largest at 24%, which makes sense: building a full rule-compliant chess engine from scratch is the foundational effort.

### Elo Progression

| Milestone | Elo | Lines of TeX |
|-----------|-----|-------------|
| Random move picker | ~300 | 1,342 |
| Depth-2 minimax + material eval | ~550 | ~1,500 |
| Depth-3 + alpha-beta + quiescence + PSTs | **~1280** | 2,093 |

~1000 Elo gained from better search and evaluation (a massive improvement from pure TeX macro optimization, all discovered and implemented by the coding agent).


## What Makes TeXCCChess Remarkable

1. **It exists.** As far as I can tell, there is no prior chess engine in TeX. A coding agent synthesized one from scratch, with no known example to draw from.
2. **It plays legal chess (with high confidence).** Castling, en passant, promotion, check detection, stalemate, and the 50-move rule are implemented and validated with a 23-case test suite (though not all rules have exhaustive coverage). Threefold repetition and insufficient-material draws are not yet implemented. Beyond unit tests, the engine completed hundreds of tournament games against Stockfish; in rare occasions, an illegal move or protocol error occurred, though it is unclear whether these stem from the TeX engine logic or from the UCI wrapper. This gives reasonable confidence that the core move generation and rule handling are robust, if not formally verified.
3. **It thinks.** Three plies of lookahead with alpha-beta pruning and quiescence search, using only TeX macro expansion and integer registers.
4. **It is entirely self-contained.** The core engine is a single `.tex` file. No shell-escape, no LuaTeX, no external computation.
5. **It produces a typeset PDF.** You play by editing a `.tex` file and compiling with `pdflatex`. The output is a beautifully typeset chess document (because, after all, it's still TeX running inside the LaTeX ecosystem).
6. **It is neither a translation nor a copy of an existing chess engine.** There is no existing TeX chess engine to copy from, and you cannot mechanically translate a C or Java engine into TeX (no arrays, no conventional recursion with a call stack, no convenient local variables). The underlying *algorithms* are well-known (alpha-beta, quiescence, MVV-LVA, PSTs); if there is a "translation", it happens at this abstraction level. The creativity lies in finding TeX-native ways to encode them: register-based state stacks, csname lookup tables, explicit loop unrolling for search depth.

The complete source code and Elo assessment infrastructure are available at [**github.com/acherm/agentic-chessengine-latex-TeXCCChess**](https://github.com/acherm/agentic-chessengine-latex-TeXCCChess/).

*This is part of a series on AI-generated chess engines across programming languages. The next post will cover another engine from the polyglot experiment. Stay tuned.*

*Feel free to contact me: mathieu.acher@irisa.fr*

*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2026texccchess,
  author = {Mathieu Acher},
  title = {TeXCCChess: How Coding Agents Wrote a Chess Engine in Pure TeX},
  year = {2026},
  month = {feb},
  howpublished = {\url{https://blog.mathieuacher.com/TeXChessEngine/}},
  note = {\url{https://blog.mathieuacher.com/TeXChessEngine/}}
}
```

