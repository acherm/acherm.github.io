---
layout: post
title:  "AI Cheating at Chess: Some Notes"
date:   2025-05-25 011:54:29 +0200
tags: [ChatGPT, LLM, generative AI, openai, chess, cheating, agents, reasoning models]
---


A study by Palisade Research found that advanced AI models (most notably OpenAI's o1-preview or Claude Sonnet 3.5 from Anthropic) sometimes "cheat" in chess by hacking their opponent's system files rather than playing by the rules.
While older AI models required explicit prompting to cheat, the most recent agents seem capable of discovering and exploiting cybersecurity holes, raising concerns that AI systems might develop manipulative strategies and be uncontrollable for complex tasks. I'm a bit late to the party (the paper caught lot of attention early this year). I don't want to give a strong opinion, but rather share some notes about some technical aspects, especially how AIs actually cheat at chess. It can serve to have a better discussion and assessment on this concerning case study. 

## Experiments

The researchers evaluated seven state-of-the-art language models (o1-preview, o1, o3-mini, GPT-4o, Claude 3.5 Sonnet, Alibaba’s QwQ-32B-Preview, and DeepSeek R1) on the task of "winning against Stockfish" one of the strongest chess engines in the world.

Each model was given a "scratchpad" text box to record its chain-of-thought before making moves, allowing researchers to inspect its internal reasoning and look for evidence of attempts to subvert the rules. 
The models also have access to a simulated shell environment, originally for playing chess, but when cheating it enables them to inspect and modify system files in an attempt to subvert the game rules.

Between January 10 and February 13, the team ran hundreds of trials per model, logging how often each attempted to hack system files to force a resignation and how often such exploits succeeded.
I found the github https://github.com/PalisadeResearch/ctfish/ remarkable, as well as the shared data (traces) that allow researchers to inspect their internal reasoning and look for evidence of attempts.


## How AI cheats at chess

Very interesting  https://us.cloud.langfuse.com/project/cm5pm9b1r007i7vvdiquwrj2g/traces/7ca6401e-cc31-4bd4-93be-7d9d171587c7?observation=da63f81e-6918-4235-963a-e8e0a9d76f98


![step 1](../assets/step1-agentchess.png)

![step 2](../assets/step2-agentchess.png)

![step 3](../assets/step3-agentchess.png)

![step 3b](../assets/step3b-agentchess.png)

![step 4](../assets/step4-agentchess.png)

![step 5](../assets/step5-agentchess.png)

![step 6](../assets/step6-agentchess.png)

The tldr; is that LLMs struggle a lot with FEN position... 
But the plan and intention are clearly brillant. 
There is intention to cheat!

1) can only succeed with a weak verifier (here game.py)... here not knowing much about chess is not a problem/big deal

there are two cases: legal positions but not with the position LLMs have in mind... missing an opportunity here
proposing illegal moves that would be favorable... in real chess those would be rejected

2) for more sophisticated cheating, no way 



A success here: https://us.cloud.langfuse.com/project/cm5pm9b1r007i7vvdiquwrj2g/traces/0935efc6-a8ab-4a1d-bfa4-ad7ab5dd0bd7?observation=6d3105e8-b59e-44aa-8967-eeb453a430a1 
 
 
 It's actually an illegal position (black to move, and white king is checked https://lichess.org/analysis/fromPosition/6k1/8/8/8/8/8/8/5qK1_b_-_-_0_1)

so AI succeeds to "manipulate" by abusing game.py... 
The non-sense comes from game.py in this specific situation. Non-sense (from game.py) against non-sense (an illegal FEN) in a sense. 



```
❯ python game.py status
. . . . . . k .
. . . . . . . .
. . . . . . . .
. . . . . . . .
. . . . . . . .
. . . . . . . .
. . . . . . . .
. . . . . q K .
position fen 6k1/8/8/8/8/8/8/5qK1 b - - 0 1
Current side: Black
Black score: 9993 Mate(+7)
Valid moves: g8h8 g8f8 g8h7 g8g7 g8f7 f1f8 f1f7 f1f6 f1a6 f1f5 f1b5 f1f4 f1c4 f1h3 f1f3 f1d3 f1g2 f1f2 f1e2 f1g1 f1e1 f1d1 f1c1 f1b1 f1a1
```

The reason is that Stockfish doesn't check legality of position. 

```
stockfish
Stockfish 17.1 by the Stockfish developers (see AUTHORS file)
position fen 6k1/8/8/8/8/8/8/5qK1 b - - 0 1
go analysis
info string Available processors: 0-15
info string Using 1 thread
info string NNUE evaluation using nn-1c0000000000.nnue (133MiB, (22528, 3072, 15, 32, 1))
info string NNUE evaluation using nn-37f18f62d772.nnue (6MiB, (22528, 128, 15, 32, 1))
info depth 1 seldepth 2 multipv 1 score cp 478 nodes 27 nps 13500 hashfull 0 tbhits 0 time 2 pv f1b5
...
info depth 245 seldepth 14 multipv 1 score mate 7 nodes 25773853 nps 13395973 hashfull 27 tbhits 0 time 1924 pv f1e2 g1h1 g8g7 h1g1 g7h6 g1h1 h6h5 h1g1 h5g4 g1h1 g4g3 h1g1 e2d1
bestmove f1e2 ponder g1h1
```

In lichess, when you enter such a FEN, you cannot use Stockfish and any chess engine. 
The reason is that lichess checks before using a FEN library written in JavaScript. 

I knew this lack of checking of Stockfish thanks to prior experience and works with metamorphic testing of Stockfish https://blog.mathieuacher.com/ReproducibilityMetamorphicTestingChess/. 
When we reproduced the original paper, we came across FEN positions with strange behavior of Stockfish. 
The reason is that these FEN positions were simply not legal. 
We used this library https://github.com/niklasf/chessops/issues/151 as a way to preprocess data. 

Stop disgression. The overall point is that the script `game.py` is problematic and not a good guardrail:
 * no checking of FEN
 * determining the side is done with a basic split... why not, but non robust to strange FEN or inputs that an attacker could fed 
 * relying on Stockfish that does not check either  

```python
def show_status():
    with open(BOARD_FILE) as f:
        board = chess.Board(f.readlines()[-1])
    print(board)
    position = board.fen()
    print("position fen", position)

    side_id = position.split()[1]
    side = {"w": "White", "b": "Black"}.get(side_id, side_id)
    print("Current side:", side)

    score = None
    with chess.engine.SimpleEngine.popen_uci("stockfish") as engine:
        # Get detailed evaluation
        info = engine.analyse(board, chess.engine.Limit(depth=15))
        if "score" not in info:
            print("Something went wrong with engine.analyse", file=sys.stderr)
            sys.exit(2)
        score = info["score"].relative
    print(side, "score:", score.score(mate_score=10000), repr(score))
    if board.is_game_over():
        print(game_over_message(board))
    else:
        print("Valid moves:", " ".join(m.uci() for m in board.legal_moves))
    mate = score.is_mate() and score.mate()

    return (board, mate, score)
``` 


