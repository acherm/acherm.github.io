---
layout: post
title:  "Can Coding Agents Master Printf-Oriented Programming?"
date:   2026-03-05 11:00:00 +0100
tags: [LLM, coding agents, C, printf, software engineering, generative AI, llm4code, Claude Code, Codex, chess, chess engine, compilers]
---

Can coding agents understand and apply a programming paradigm they've (almost certainly) never seen in training data? Not a standard design pattern, not something from a textbook, but an esoteric hack from an obfuscated C contest: Printf-Oriented Programming (aka POP). The idea is that printf is Turing complete, but can coding agents leverage that fact to write real programs *in* `printf`?
I tried. The answer is: it's complicated, fascinating, and ultimately led me somewhere I didn't expect.

## Printf-Oriented Programming

[Printf-oriented programming (POP)](https://github.com/carlini/printf-tac-toe) is a brilliant paradigm invented by Nicholas Carlini for the [IOCCC 2020](https://www.ioccc.org/).
The idea: encode a program in `printf`'s format string and arguments, exploiting specifiers like `%hhn` (write the byte count to a pointer) and `%.*s` (conditional output via precision) to implement memory access and control flow. One `printf` invocation becomes a tiny virtual machine.
Carlini demonstrated this with [printf-tac-toe](https://github.com/carlini/printf-tac-toe): a fully playable tic-tac-toe where `printf` *is* the execution engine, not just the renderer.

Can we go beyond tic-tac-toe? Can we scale POP to *chess*? Can coding agents master this esoteric paradigm? I gave the challenge to two agents:

 * [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (Claude Opus 4.6)
 * [Codex CLI](https://github.com/openai/codex) (GPT-5.2)

## Act 1: The Illusion of Success

First impressions were exciting. Both agents produced working chess programs with the signature POP loop:

```c
int main(void) {
    while (*d) printf(fmt, arg);
}
```

Claude Code's version (~14 KB) chains side-effect calls in the `arg` macro. Codex's version (~31 KB) uses eight `pop_a*` functions with `%hhn` directives. Both compile, both play chess, both handle legal move validation, check/checkmate/stalemate. I was ready to write a triumphant blog post about how coding agents had conquered POP.

Then I looked more carefully.

The chess logic (move generation, legal filtering, check detection, AI) was implemented in conventional C helper functions (~400 lines of `if`, `switch`, `for`, `while`). These helpers run as side effects during argument evaluation, *before* `printf` ever touches the format string. The format string itself mostly just renders pre-computed buffers via `%s`. The `while(*d) printf(...)` loop is there, sure, but the *spirit* of POP (having `printf` act as the execution engine, performing state transitions through format specifiers) was largely absent.
It's like claiming you wrote a program in assembly while actually calling a high-level library for everything interesting. The structure is POP-shaped. The substance is C with `printf` as a display layer.

## Act 2: Trying to Force POP Purity

So I pushed back. I asked the agents to use real POP techniques: `%hhn` for state mutation, `%.*s` for conditional branching, format-string-driven control flow. I gave detailed feedback, pointed at Carlini's techniques, explained what was and wasn't POP-pure.

It didn't go well.

The agents would acknowledge my feedback, propose elaborate plans, and then... produce code that was still fundamentally C-with-a-printf-wrapper. Sometimes they'd move *one* thing into the format string (say, loop termination via `%hhn`) while keeping everything else in conventional C. Other times they'd spend hours designing complex architectures that simply didn't compile. Claude Code's attempt to "print all legal moves using only printf logics" was, in its own way, a spectacular failure.

I started with ad-hoc review criteria, then converged to a formal **POP purity checklist** to make feedback precise and repeatable. Five dimensions, each scored 0-2:

| Dimension | What it measures |
|---|---|
| **Loop purity** | Is the main loop `while (*d) printf(fmt, ARGS)` with no other control flow? |
| **Mutation purity** | Are all state transitions via `%hhn` writes? |
| **Control/branch purity** | Are branches done via `%.*s` formatting-time selection? |
| **Engine purity** | Does `printf` act as the execution engine, not just renderer? |
| **No C-script ROM** | No hardcoded C-side logic or lookup tables? |

This rubric is an operational audit, not a formal proof of semantic purity.

Score 9-10: POP-pure. Score 6-8: hybrid. Below 6: POP-shaped at best. My manual estimate for both chess implementations? Around 4-5. Hybrid at best, and that's being generous.


## Act 3: Scaling Down

Chess was clearly too ambitious for strict POP. I took a step back. What about tic-tac-toe itself, not copying Carlini's, but having the agent write one from scratch in the POP style?

Still difficult. The agents could produce *working* tic-tac-toe programs, but achieving POP purity required iteration after iteration. Each version got a little closer: first basic display, then input handling, then playability with comparisons (hybrid mode), and finally (after much steering) a version using only bitwise operations (no comparisons!) to detect wins, draws, and quit. That last version is genuinely impressive: it implements win detection via 8-way AND/OR checks over nibble bitmasks, all without a single `<`, `>`, or `==`. But it took a lot of human guidance to get there.

The checklist evolved along the way. I refined it into a set of **antipatterns and violations**: per-iteration helper calls, C-side semantic operators, host-driven state snapshots, C-side expression evaluation where the format string should do the work. And here's where it got interesting: the coding agent could *operationalize* this checklist as an automated scoring script. The same agent that struggled to write POP-pure code could write a tool to *evaluate* POP purity. Easier to be a critic than an artist, apparently.

## Act 4: The Compiler Idea

At some point during the back-and-forth, I had a realization: if coding agents can't reliably *intuit* how to write POP, maybe they can build a *deterministic translator*: a compiler from a restricted C subset to POP. Clear rules, no creativity required in the output. You write your logic in a constrained source language, and the compiler mechanically transforms it into `printf`-driven code.

Codex ran with this idea and produced `c2pop.py`, now roughly a ~2,700-line Python compiler. The input language (called "CPOP subset") supports a tape (`d[N]`), one canonical loop (`while (d[0]) { ... }`), assignments, reads, prints, and conditionals. The compiler offers four backends with different tradeoffs (not a strict purity ladder):

- **simple**: straightforward lowering; often hybrid when expressions are still evaluated in C
- **micro**: single-call printf lowering for selected boolean forms (experimental)
- **phase**: multi-phase dispatcher, one source statement per phase (explicitly hybrid)
- **vm**: VM backend with `%hhn`-driven phase state and format-pointer updates; highest-purity path for supported forms

Each generated file gets an automatic purity score and a "cheat audit" listing any POP violations. For supported programs, the vm-pure vm backend can report 10/10 POP-pure output, especially when host-snapshot input is avoided.

The tradeoff? The generated POP code is *very* long. A 20-line CPOP source can produce hundreds of lines of format string gymnastics. In practice, strict generated examples are strongest on tic-tac-toe/snake-scale programs; for chess-scale behavior, what remains POP-pure is closer to a state displayer driven by a POP byte-VM than a full engine: you lose the AI, the legal move generation, all the things that make chess interesting. The scope reduction required for POP purity is dramatic.

## A Confession

For the Codex branch documented here, the core iterations happened quickly: from March 3, 2026 to March 5, 2026. Throughout this post, I'm quite demanding about what should count as "real" POP, what's acceptable and what's not. But I should be honest: I am *not* a POP expert. I can spot violations, I can articulate what feels wrong, but I often have no concrete idea how to fix the issues myself. I'm a critic without a solution, relying on coding agents to figure out the "how" while I insist on the "what". That tension runs through the entire experiment. This is also very much a work in progress, or perhaps a work in "lambo": I'm reflecting on where I am, where the coding agents are, and what's still missing. It's genuinely unfinished.

## Can You Do Better?

I'm pretty sure someone can. Either from scratch (without a coding agent, just raw POP expertise and patience) or *with* a coding agent but with better prompting, better domain knowledge, or a smarter iterative strategy than mine. I would be genuinely interested to know: what level of detail and instructions would it take to realize the vision of a POP-pure chess engine? How much POP expertise does a human need to bring? How much can the agent figure out on its own if guided differently?

I'm expecting someone will do it. If you do, please let me know.

## What's Still Open (A Lot)

This is ongoing work (really: unfinished work), and many directions remain:

- **POP skills**: Can we teach coding agents POP as a first-class skill, perhaps through few-shot examples or fine-tuning?
- **POP harness**: A systematic test suite for POP compliance, beyond the current checklist
- **A better compiler**: `c2pop.py` works but the output is verbose; can we optimize the generated format strings?
- **Formalizing POP**: What *is* POP, precisely? What's the grammar of valid POP programs? What subset of computation can POP express efficiently? There's a real PL theory question hiding here.
- **POP for other games**: Scaling beyond tic-tac-toe toward more complex programs while maintaining purity
- **Other sources of information**: There is a Brainfuck interpreter implemented in POP https://github.com/HexHive/printbf. Maybe a coding agent could learn POP by studying that code, or by learning Brainfuck first and then translating to POP.

Repositories:
- Claude Code version: [https://github.com/acherm/printf-chess](https://github.com/acherm/printf-chess)
- Codex version (with compiler, benchmarks, examples): [https://github.com/acherm/printf-chess-codex](https://github.com/acherm/printf-chess-codex)

## Why POP Matters as a Benchmark

Beyond the fun of it, I think POP is a *fantastic* benchmark for coding agents, and here is one of the central messages of this post.

Most coding benchmarks test whether an agent can produce correct, working code. POP tests something deeper: can the agent understand a *non-trivial programming paradigm* from a single example, grasp its underlying principles (not just its surface structure), and creatively apply them to new problems?

The results are humbling. Coding agents in 2026 can [build chess engines from scratch in 12 languages](https://blog.mathieuacher.com/FromScratchChessEnginesPolyglot/), including [LaTeX](https://blog.mathieuacher.com/TeXCCChessEngine/), COBOL, and Brainfuck. But when asked to encode logic *inside a format string*, they default to what they know: conventional C with a POP-shaped wrapper. They can mimic the structure without grasping the essence. Getting from "it looks like POP" to "it *is* POP" required extensive human guidance, a formalized checklist, and ultimately a compiler to do the translation mechanically.

That gap between surface imitation and deep understanding is exactly what makes POP a useful probe. It tests creativity, adaptability, and mastery of programming languages and compilers in a "strange" zone where standard training data won't help much. And the fact that the journey led to automated scoring tools, a formal purity rubric, and a C-to-POP compiler is, in its own way, a success story. Just not the one I expected to write.

As I said: this is unfinished. I don't know yet whether a POP-pure chess engine is achievable with current coding agents, or whether it requires a fundamentally different approach. But the exploration itself has been worth it, and I'll keep pushing. Hopefully someone else will too.

```bibtex
@misc{acher2026printfchess,
  author = {Mathieu Acher},
  title = {Can Coding Agents Master Printf-Oriented Programming?},
  year = {2026},
  month = {mar},
  howpublished = {\url{https://blog.mathieuacher.com/PrintfOrientedProgrammingCodingAgents/}},
  note = {\url{https://blog.mathieuacher.com/PrintfOrientedProgrammingCodingAgents/}}
}
```
