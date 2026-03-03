---
layout: post
title:  "Coding Agents Master Printf-Oriented Programming"
date:   2026-03-03 11:00:00 +0200
tags: [chess, chess engine, LLM, coding agents, C, printf, software engineering, generative AI, llm4code, Claude Code, Codex]
---

What happens when you ask coding agents to implement a chess engine in C where the entire game loop is a single `printf` call? No `if`, no `switch`, no explicit control flow in `main()` -- just `while(*d) printf(fmt, arg);`. Sounds absurd? It works. And two different coding agents pulled it off.

## Printf-Oriented Programming

[Printf-oriented programming (POP)](https://github.com/carlini/printf-tac-toe) is an esoteric (and brilliant) programming paradigm invented by Nicholas Carlini for the [IOCCC 2020](https://www.ioccc.org/). 
The idea: encode a program in printf’s format string and arguments, exploiting its specifiers to implement memory access and control flow. It transforms one printf invocation into a tiny virtual machine.
Carlini demonstrated the concept with [printf-tac-toe](https://github.com/carlini/printf-tac-toe): a fully playable tic-tac-toe game in a single `printf` call.

Can we go beyond tic-tac-toe? Can we scale POP to *chess*? Can coding agents master this esoteric paradigm?

## The Experiment

It is not part of the [series of experiments asking coding agents to build chess engines from scratch](https://blog.mathieuacher.com/FromScratchChessEnginesPolyglot/), though definitely related. 
Let's say it's another fun project in the same spirit: pushing coding agents to implement chess engines in increasingly esoteric ways. After LaTeX, COBOL, Brainfuck, and more, why not POP?
Here my questioning was more: are coding agents capable of understanding and applying a non-standard programming paradigm, given a single example? POP is not something you learn in school or see in typical codebases. 
It's a clever hack from an obfuscated C contest. If I show it to them, can they grasp the underlying principles and extend it to a much more complex problem?

I gave two coding agents the same challenge: read Carlini's printf-tac-toe repository and implement a chess engine in the same style. 
It has not been done. And chess is orders of magnitude more complex than tic-tac-toe: 64 squares, 6 piece types, more complex move generation and rules. I also asked for a random AI opponent, legal move validation, and check/checkmate/stalemate detection, all within the constraints of a single `printf` loop. 
The two agents:

 * [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (Claude Opus 4.6)
 * [Codex CLI](https://github.com/openai/codex) (GPT-5.2)

Both succeeded.

## Claude Code's Take

Claude Code produced a `printf_chess.c` (~14 KB) where `main()` is literally:

```c
int main(void) {
    while (*d) printf(fmt, arg);
}
```

All game state lives in a single `char d[140]` array. The `arg` macro expands to a chain of side-effect calls (`domv()`, `upd()`, ...) followed by the actual `%s` arguments for 64 board squares, an evaluation score, and a turn indicator. Helper functions (~400 lines) handle move generation, legal move filtering, check/checkmate/stalemate detection, and a random AI opponent. Initialization and teardown happen via first-call detection and `atexit()`.

The process was not entirely smooth. At one point, I asked Claude Code to "print all legal moves at each turn, using only printf logics". It spent hours designing complex plans and failed. The incremental approach worked better: first add an evaluation display, then legal move generation, then a random AI. My contribution was limited to steering, not coding.

Source: [https://github.com/acherm/printf-chess](https://github.com/acherm/printf-chess)

## Codex's Take

Codex took a different architectural route. Its `printf_chess.c` (~31 KB) uses eight `pop_a*` functions as `printf` arguments, with a monster format string (~600 lines) full of `%hhn` directives that write state bytes through the number of characters printed so far. A lookup table of tokens (`""`, `"x"`, `"xx"`) controls what gets written to each state slot.

```c
while (*d)
    printf(fmt, pop_a1_zero(), pop_a2_screen(), pop_a3_run_tok(),
           pop_a4_run_ptr(), pop_a5_side_tok(), pop_a6_side_ptr(),
           pop_a7_boot_tok(), pop_a8_boot_ptr());
```

The chess logic is more sophisticated: negamax with alpha-beta pruning at depth 3, proper material evaluation, and support for pawn promotion. Yet the outer control flow (the game loop, state transitions, screen rendering) follows the POP paradigm.

Source: [https://github.com/acherm/printf-chess-codex](https://github.com/acherm/printf-chess-codex)


## Two Agents, Two Architectures, Same Paradigm

Both agents understood the POP paradigm from Carlini's example and scaled it from tic-tac-toe to chess. Both produce playable games with legal move validation, check/checkmate/stalemate detection, and a computer opponent. Neither implements castling or en passant (fair enough for a single `printf`). Yet the internal designs differ: Claude Code's approach is closer to the original printf-tac-toe style (one big `arg` macro with comma-chained side effects), while Codex leverages `%hhn` format-string writes for state mutation, a more "format-string-native" technique.

What I find remarkable is that both agents grasped an esoteric programming paradigm from a single example and extended it to a problem orders of magnitude more complex. POP is not in any textbook, not a standard design pattern. It's a clever hack from an obfuscated C contest. And yet, when pointed at the right reference, coding agents can learn it and run with it.

After [LaTeX](https://blog.mathieuacher.com/TeXCCChessEngine/), [COBOL, Brainfuck, and 9 other languages](https://blog.mathieuacher.com/FromScratchChessEnginesPolyglot/), printf-oriented programming is another reminder that coding agents in 2026 can handle much more than boilerplate CRUD.
Github repositories for both implementations are available, and you can try them out yourself. It's a fun demonstration of the creativity and adaptability of coding agents when given the right challenge and reference. 
There is also a Rust and Java version of POP, but that's for another post (or not).


```bibtex
@misc{acher2026printfchess,
  author = {Mathieu Acher},
  title = {Coding Agents Master Printf-Oriented Programming},
  year = {2026},
  month = {mar},
  howpublished = {\url{https://blog.mathieuacher.com/PrintfOrientedProgrammingCodingAgents/}},
  note = {\url{https://blog.mathieuacher.com/PrintfOrientedProgrammingCodingAgents/}}
}
```
