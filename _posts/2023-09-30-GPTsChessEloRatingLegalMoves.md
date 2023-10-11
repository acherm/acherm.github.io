---
layout: post
title:  "(Unfinished Draft) Debunking the Chessboard: Confronting GPTs Against Chess Engines to Estimate Elo Ratings and Assess Legal Move Abilities"
date:   2023-09-30 011:54:29 +0200
tags: [ChatGPT, LLM, generative AI, variability, programming]
draft: true
---

Can GPTs like ChatGPT-4 play legal moves and finish chess games? What is the actual Elo rating of GPTs? There have been some hypes, (subjective) assessment, and buzz lately from "GPT is capable of beating 99% of players?" to "GPT plays lots of illegal moves" to "here is a magic prompt with Magnus Carlsen in the headers". There are more or less solid anecdotes here and there, with counter-examples showing impressive failures or magnified stories on how GPTs can play chess well. I've resisted for a long time, but I've decided to do it seriously! I have synthesized hundreds of games with different variants of GPT, different prompt strategies, against different chess engines (with various skills). This post is here to document the variability space of experiments I have explored so far... and the underlying insights and results. The tldr; is that `gpt-3.5-turbo-instruct` operates around 1750 Elo and, though there are avoidable errors, is often capable of playing end-to-end legal moves, even with black pieces or when the game starts with strange openings. ChatGPT-3.5-turbo and more surprisingly ChatGPT-4, however, are much more brittle. Please do not stop to the tldr; and read the entire blog posts: there are subtilities and findings worth discussing! 


## GPTs and Chess since the 2020 prehistoric days 

If you read this blog or follow me on Twitter, you know I have been interested for a while about generative AI technologies and chess. For instance,  I have reported on [GPT-2](https://blog.mathieuacher.com/GTP2AndChess/), one of the first promising version of GPT and trained on 3.5 million chess games with 146 TPUs (ouch!). This specialized version of GPT-2 played well during standard openings due to its training data but quickly blunders thereafter. While it consistently makes legal moves without understanding chess (even 30 moves into a game!), its performance was easily outclassed by traditional chess engines and human players --  [see some games in the blog post](https://blog.mathieuacher.com/GTP2AndChess/). At that time, it was impressive to see that transformers and a text completion engine can partially learn chess rules from pure text. 

Since then, I have publicly shared and discussed some experiments like a prototype to play chess games with GPT-3 https://twitter.com/acherm/status/1616477887607242752 including some analysis that basically shows that GPT-3 proposes lots of illegal moves. I have also shown some failures of ChatGPT-4 https://twitter.com/acherm/status/1636441376375095306 Even this summer ChatGPT-4 has been defeated by Chessman Elite (beginner level, instantaneous answer, coming from the 90s): https://twitter.com/acherm/status/1691123390298443780 

Obviously, I'm not alone to share chess-related stuff about GPTs, there are many examples here and there: eg https://twitter.com/GothamChess/status/1624755982105513992 https://www.youtube.com/watch?v=svlIYFpsWs0 In general, I like the interactions and discussions we can have on this subject, with interesting recommendations and food for thoughts. I also much appreciate the interventions of Gary Marcus that heroically tries to ponder some crazy over-claims of GPT afficianados! 

Overall, I have compiled a long list of chess+GPT things to explore and, well, I've resisted for a long time. But I've decided to do it more seriously! 



## Signals and related work in the X/Twitter space  

The basic reason is that Grant Slatton reported, on a quite viral tweet https://twitter.com/GrantSlatton/status/1703913578036904431, that "the new GPT model, gpt-3.5-turbo-instruct, can play chess around 1800 Elo." Interestingly, Grant had previously reported that ChatGPT-4 https://twitter.com/GrantSlatton/status/1699441158450270472 cannot play chess, on par with my past observations and informal experiments. Following Grant's tweet, Joel Eriksson also suggested that "GPT-3.5 playing chess against Stockfish set to 1700 ELO." https://twitter.com/OwariDa/status/1704170914659582281 and shared some code. Though I completely disagree with "Ping @ylecun, @GaryMarcus & others who claim LLMs are incapable of reasoning, and don't create world models", I toke it as a reproducibility challenge ;-) 

Another signal that caught my attention was an experiment suggesting that if you throw the first random moves, GPTs fall apart: https://twitter.com/GaryMarcus/status/1706672864970363106. (Remarkably, the tweet has been deleted since then, with nice clarifications: https://twitter.com/paul_cal/status/1707303586932072862)

An issue of all these signals in the X/Twitter space is that people very quickly jump to strong conclusions or interpretation or excuses. There are also interesting hypothesis like "it appears this was just the RLHF'd chat models. The pure completion model succeeds." to explain the sudden improvement https://twitter.com/GrantSlatton/status/1703913578036904431. Overall, all of this calls to investigate seriously some interesting questions, in short to reproduce and replicate experiments! 



## Questions and method

A first natural question is to assess the Elo rating of GPTs. Beforehand and hence a pre-requisite question is to assess the ability of GPTs to play legal moves. After all, playing illegal moves in practical chess settings has the consequence of losing games and decreasing your Elo rating! 

A general issue with all these experiments I've read on X/Twitter, including mine, is that we're not sure about the generalization of results. Most of experiments have been done on specific GPTs, with specific prompts, against specific engines, with only a few games. For instance, "Reports of illegal moves are pretty common so I find this hard to swallow, Wondering about your methods and sample size." https://twitter.com/GaryMarcus/status/1704112674932502767 

Hence, there is an opportunity to automate the experiments (e.g., for synthesizing games of GPTs) and then expand the set of experiments. 

## Variability space of experiments 

In order to assess ability of playing legal moves and Elo rating, one essentially needs to play GPTs against different opponents. Many^many experiments are possible. If we think deep and consider every detail, there are numerous variation points together with possible alternatives and values. I'm detailing them hereafter. 

### GPT model: gpt-3.5-turbo-instruct, text-davinci-003, gpt-4, gpt-3.5-turbo 

I only considered OpenAI GPT models. "Open source" models, such as LLaMa2, Falco, or Mistral can be used as well, but I have doubts they've been trained on chess games. 

I have selected:

* `gpt-3.5-turbo-instruct` since this model has caught recent attention. This model can be used through the OpenAI API. Interestingly, this model only serves for text completion and has not been tuned for "chat". 
* `text-davinci-003` an early model for text completion, also available through OpenAI API. 
* `gpt-4`: as the name does not suggest, this model is used for chat purposes, for interacting through different messages. Hence, it should be understood as "ChatGPT-4", the model used as part of ChatGPT Plus. 
* `gpt-3.5-turbo`: this model is also used for chat purposes and should be understood as "ChatGPT-3.5-turbo" (i.e., an alternative to "ChatGPT-4")

### Temperature: 0, 0.8

All these GPT models come with hyperparameters. I have considered: 

* temperature: between 0 and 1 (or 2 in chat mode)... https://platform.openai.com/docs/api-reference/completions "Lower values for temperature result in more consistent outputs, while higher values generate more diverse and creative results." I have mainly considered 0 to have consistent results and prior experiments rely on 0 value as well. More creativity (and higher temperature) can intuitively lead to either verbose answers or to textual moves that have more chances to be illegal. For the record, I've tried `gpt-3.5-turbo-instruct` with temperature 0.8 as well. 
* max_tokens: 5 or 6. Same value as for prior experiments. A move in PGN notation is usually 3 characters (`Nf6`), but there can be moves like `exd8=Q` . So 5 or 6 value seems reasonable. In chat mode, we also want to avoid explanations; we only want parsable moves! 

Taking a step back, the setting of temperature value (or max_tokens) is something impossible in the ChatGPT Web interface: you have to go through the APIs to be able to control. Hence, a retrospective hypothesis is that prior experiments used ChatGPT-3.5 or ChatGPT-4 with unknown, uncontrolled temperatures and max_tokens, both can influence the quality of the answers. In our personal research on code synthesis, we have shown that [temperature can have a large influence on the quality of the generated code by Codex and Copilot](https://arxiv.org/abs/2210.14699). It's the advantage of carefully doing experiments and automating: one can at least control some variability of the experiments! 

### Prompts: basic, altered, chat-oriented

Prompts aka the instructions you're giving to GPTs obviously matter. I use the following prompt for `gpt-3.5-turbo-instruct` 

``` 
[Event "FIDE World Championship Match 2024"]
[Site "Los Angeles, USA"]
[Date "2024.12.01"]
[Round "5"]
[White "Carlsen, Magnus"]
[Black "Nepomniachtchi, Ian"]
[Result "1-0"]
[WhiteElo "2885"]
[WhiteTitle "GM"]
[WhiteFideId "1503014"]
[BlackElo "2812"]
[BlackTitle "GM"]
[BlackFideId "4168119"]
[TimeControl "40/7200:20/3600:900+30"]
[UTCDate "2024.11.27"]
[UTCTime "09:01:25"]
[Variant "Standard"]

1.
```

with the idea of concatenating played moves and transmits the game in PGN format: GPTs have to complete the rest. Prompt engineering is clearly a thing, and a bit of mystery. There have been some discussions about the effectiveness of this specific prompt (like the presence of Carlsen as a white player in the headers would force GPTs to play well). Why not. I have tried an altered prompt:

```
[Event "Chess tournament"]
[Site "Rennes FRA"]
[Date "2023.12.09"]
[Round "7"]
[White "MVL, Magnus"]
[Black "Ivanchuk, Ian"]
[Result "1-0"]
[WhiteElo "2737"]
[BlackElo "2612"]

1.
```

to explore a bit the effect of PGN headers. Also, as a side note, I have early tried other prompts, more directive ("You play a chess game..."), and there were less effective. What seems effective with the original prompt is that the text completion of `gpt-3.5-turbo-instruct`  is well-suited. It puts a "PGN context" and calls the attention into completing the moves in chess game notation. 



#### Prompting in chat mode 

The story is different with "chat" GPTs like `gpt-4` and `gpt-3.5-turbo`. These models have been fine-tuned for chatting and also the exposed API differs https://platform.openai.com/docs/guides/gpt/chat-completions-api. Early experiments with the original or the altered prompt lead to the following situation: `gpt-4` and `gpt-3.5-turbo` tend to be verbose and in "explanations" mode, eventually not playing moves or playing dubious, non parseable moves! 

Hence, an idea is to leverage API facilities and set a "role" (see below) while providing a series of messages that are moves played alternatively by the chess engine or the GPT. Basically something like: 

```
[{'role': 'system', 'content': "You are a professional, top international grand-master chess player. We are playing a serious chess game, using PGN notation. When it's your turn, you have to play your move using PGN notation."}, {'role': 'user', 'content': '1. d4'}, {'role': 'user', 'content': '1... d5'}, {'role': 'user', 'content': '2. Qd3'}, {'role': 'user', 'content': '2... Nf6'}, {'role': 'user', 'content': '3. Nf3'}]
```

This strategy proved to be effective and can be automated, though evils are in the details (more details hereafter). Something I did not explore much is to vary the "role" content. 

It should be noted that this strategy differs from the original prompt of GPT models for text completion. Engineering "prompts" (and interactions) is definitely important!



### White or black pieces: true or false

Something that has been surprisingly not considered so far is the ability to play with black pieces. Playing with white pieces is in theory (or in the practical world of chess human players) an advantage, and it's good to balance with games with black pieces. Also, starting with black pieces brings some diversity. 

### Opponents and chess engines: random, SF with skill levels

GPTs can play chess, but against who? 

I considered two variants:

* a random chess engine: An automated procedure that picks a random move across all possible legal moves in a given chess position. On the one hand, this engine is very weak from a chess level perspective: I don't know what is the Elo rating but it would be very low. On the other hand, a random engine plays very strange moves that may cause issues for GPTs, since not in the usual training set. 
* the Stockfish chess engine at different "skills" and in different settings (see details below)

#### Stockfishes

Stockfish (SF) is a state-of-the-art, open source chess engine. SF with full power, even on a basic computer, is unbeatable even for top grand-masters and the estimated Elo is more than 3500 (Magnus Carlsen is still dreaming of reaching the incredible 2900). Hence, an idea is to weaken SF rather than to optimize it https://github.com/official-stockfish/Stockfish/wiki/Stockfish-FAQ#optimal-settings

There are lots of parameters of SF that can be controlled, both having an effect on the Elo rating. 

I have played a bit with `depth`, `Elo rating` , and `Skill level` . The conclusion is that it's easy to perform mistakes (e.g., setting Elo rating, keeping the depth to 15, and letting only 1 second to play), since it's highly sensitive to your hardware and workload. Hence, a better strategy is to use `Skill level` and rely on Elo estimates here: https://github.com/official-stockfish/Stockfish/issues/3635#issuecomment-1159552166

By the way, super interesting discussion in this issue! What I've learned is that the estimates have to be taken with caution, since the numbers have been calibrated for a specific version of SF. I have also used `depth=20` (instead of 15).  (By the way, an immediate threat to my experiments is the SF version: I've exclusively used ` ./stockfish/stockfish/stockfish-ubuntu-x86-64-avx2
Stockfish 16`. In general, there are other factors that can influence the result: hardware, SF settings such as Hash, Threads, etc.). Yet, it is the best method so far I know of. 

I have tried different values of Skill level, from estimates of 1300 Elo to 2000 Elo, to make vary the strength of opponents. 

#### Diversity of openings: uncontrolled, predetermined 

A threat I was anticipating is the lack of diversity for the first moves (eg always the same opening and series of moves), either due to SF or GPTs. Hence I have started programming ways to diversity with known, predetermined openings, eg: 

```
###### case known first moves (to diversify a bit the openings)
def diversify_with_knownopenings():
    nmove = 2
    base_pgns = [
    '1. e4 e5 2.', 
    '1. d4 Nf6 2.',
    '1. e4 c5 2.',
    '1. d4 d5 2.',
    '1. e4 e6 2.'] 
```

However, SF has in fact some "randomness" in the search procedure, even for the first moves. Overall, games tend to be diverse enough and not to be the repetition of already played games. Hence, there is no need to control first moves for reaching diversity. 



### Random first moves: n=10 or n=20

Yet another variation point of the experiment is to start the game with random first moves. Let say, for instance, the n=10 first moves. And then, and only then, letting GPTs and SF play. 

The idea is that the n=10 random moves make the games strange and out of distribution of what GPTs have learned. Hence, the text completion can fail and produce illegal moves. An experiment suggested it https://twitter.com/GaryMarcus/status/1706672864970363106

Here, we have to find (counter-)examples of failing cases. 

## Sampling the variability space

If we consider all possible "values" of:

* GPT-model
* temperature
* max_token
* prompt 
* white or black pieces
* engines with skill levels
* random first moves or not 

and combine all possible values, the number of experiments is huge. Let say: 4 possible values for GPT-model, 5 values for temperature, 3 values for max_token (I'm oversimplifying!), 2 values for prompt, 2 values for white/black pieces, 4 values for engines, and 3 values for random first moves or not... **It's already *2880 experiments*  and only one game per experiment!**

Unfortunately, playing only one game per experiment would be a huge threat for measuring ability to play legal moves or for assessing Elo rating. Both GPTs and SFs have randomness and are stochastic, while considering a limited variability of chess game (only 1 among billions^billions possible) would not be representative at all. The point is we also need also to repeat experiments and play several games to draw solid conclusions. 10 games, 2880 experiments and settings: 28800 games. Why not. And not sure 10 is good enough. 

If it was a just a question of computing cost, perhaps it's affordable -- one can envision to parallelize the experiments. However, there is another detail: $$$$$$$. You have to subscribe and pay when using the OpenAI API, typically a relatively small amount of dollars per request and tokens. However, thousands of games quickly cost a lot. In particular, `gpt-4` (chat mode) costs a lot since the price per token is higher comparatively and also the fact of concatenating moves, in long games, makes a "chat" session not cheap at all. 

Hence, there is no escape: I can hardly, as an individual, run all possible experiments and cover the whole variability space. A **sample** of the experiments is rather needed. 

I used various strategies to reduce the variability dimensions:

* I have limited the usage of some GPT models (eg `text-davinci-003`) since it was mainly to confirm some counter-examples (eg there is limited hope to improve the situation with higher temperatures)
* I have partly explored the temperature for some GPT models and max_tokens was closed to 5 or 6. 
* for the experiments with random first moves, I was seeking counter-examples that GPT models would fail. Some other experiments already showed some weaknesses, no need to go further. 

I limited the budget to 100$. 

### Dataset 

I've compiled 858 games using a combination of GPT models, configurations of chess engines and other variation points previously described:

 * gpt-3.5-turbo-instruct : 573 games, among 401 with white piece and 172 with black pieces

 * gpt-4 : 179 games, among 116 with white piece and 63 with black pieces

 * gpt-3.5-turbo : 53 games, among 53 with white piece and 0 with black pieces

 * text-davinci-003 : 73 games, among 43 with white piece and 30 with black pieces

   

## Analysis and insights 



### text-davinci-003

Let's start with text-davinci-003, a relatively old model that serves for text completion. 

Out of 73 games with davinci (with white pieces: 43, with black pieces: 30), 72 games contain illegal moves. There are 21 games against random chess engine among the 72 and so they also contains illegal moves. The only game with no illegal move is a defeat against Stockfish (Skill level 3) in 16 moves. 

Games were short. The longest game was only 20 moves. The shortest game was 5 moves. The average and median game length was 12 moves. (Terminology note: move is one ply white piece and one ply black piece). In general, actual chess games are more around 40 moves on average. 

A qualitative analysis of illegal moves shows that the moves are syntactically correct, but simply impossible on the chessboard (typically, some pieces cannot move to the indicated square).

I did not insist much with `text-davinci-003`. Altered prompt had no specific effect and the overall conclusion is that **text-davinci-003 is unable to play an entire game because of illegal moves quickly generated, against basic SF engine or random chess engine**  It is not a big surprise and seems on par with what has been reported: a weak baseline, certainly closed to the specialized [GPT-2](https://blog.mathieuacher.com/GTP2AndChess/) 



### gpt-3.5-turbo-instruct 

With the model `gpt-3.5-turbo-instruct` (recall: text completion) I compiled 573 games, among 401 with white pieces and 172 with black pieces. 

It's the model that caught attention lately. 

Let's first focus on games played against SF at various skills. 

#### Move ability against SF

Out of 512 games against SF, 440 were legal games and 72 were illegal games, hence 14% of illegal games. 

|          | illegal_move |
| :------- | -----------: |
| 1-0      |           49 |
| 1-       |            4 |
| Rxh5     |            1 |
| {The     |            1 |
| gxh8=    |            1 |
| {[%clk   |            1 |
| bxc8=    |            1 |
| Nxa3     |            1 |
| axb8=    |            1 |
| bxc3     |            1 |
| Qa7      |            1 |
| dxe8=    |            1 |
| {(Leko-G |            1 |
| Qxc6+    |            1 |
| Kb3      |            1 |
| {M.      |            1 |
| cxb8=    |            1 |
| Bc5      |            1 |
| f7       |            1 |
| exf8=    |            1 |
| {This    |            1 |

We can categorize the illegal moves as follows.

By far the most frequent pattern is "1-0" or "1-": it's clearly not a legal move, it means white pieces have won! How to interpret this? A first interpretation is simply the inability of `gpt-3.5-turbo-instruct` to play a legal move in a middle of a game. As a text, statistical-based completion engine, it's not that surprising after all to complete the game as such. A second interpretation is that `gpt-3.5-turbo-instruct` resigns on purpose. It's totally possible in chess to stop the game (and resign) since you consider that you're losing and have no chance to save the game. Interestingly, all games but one are with black pieces, going into the direction of this hypothesis. Furthermore, most of the games are either forced check mates or clear superior position. In detail, I've used SF to analyze the 48 games with 1-0: 19 games lead to forced mates, 25 games with centipawns greater than 5 (significant advantage!), and only 4 games that are unclear. Stated differently, the judgment of `gpt-3.5-turbo-instruct` is quite accurate, except in 4 games that should be continued. There is also a third case: 1-0 but with white piece. Like: hey, I'm winning, I dare to give the next moves, so 1-0, no discussion. 

You can interpret as you want, the interpretations make sense, and are more or less supportive of `gpt-3.5-turbo-instruct`. My interpretation is in-between: it's an accidental error due to the very nature of the statistical-based completion, but the assessment of the position looks very accurate... Basically `gpt-3.5-turbo-instruct` finds that the best completion is to consider it as winning. A possible workaround is to ask `gpt-3.5-turbo-instruct` another completion (and move) when encountering 1-0. I didn't implement this workaround, since it's a grey zone: Are we here to assess GPT or to assess a corrected GPT? Is it fair to allow GPT to propose another move, when the move is illegal in the first place?



A second pattern is the incorrect promotion. Instead of eg `gxh8=` the correct move is most likely `gxh8=Q` for promoting a queen (can well be a rook, a bishop, or knight...), but as `Q` is not specified, it's an illegal move. There are two possible fixes. First is related to max_tokens: experiments with this model and GPT-4, see hereafter, show that increasing max_tokens was not that helpful. A second approach is to help the GPT model and automatically modify the promotion as a queen, thus rewriting and fixing the syntactical move. It's again a grey zone: Are we assessing GPTs? Or can we take the liberty to fix outcomes of GPT? 

A third pattern is related to a kind of unfinished comment: `{` is the starting of a PGN comment. Yea, it's really a text completion! 

A fourth pattern concerns incorrect moves (no 1-0, no 1-, no incorrect promotion, or starting comment). I've manually analyzed the 8 mistakes:

* 3 cases where a piece is "pinned", that is, a piece cannot be moved, since otherwise the king is checked (eg Nxa3 is not possible here: https://lichess.org/foSmbmil#28 or another example here: https://lichess.org/iuVNGc8W#94)
* 3 cases where a piece is jumping another (ie there is an intermediate piece before the targeted square) eg https://lichess.org/0GAW3gLY#28 and Qxc6 is not possible
* 1 case where a pawn takes another pawn of the same side (bxc3 is not possible here https://lichess.org/ILffB9ri#26)
* 1 case where the square is not reachable (f7 is not possible: https://lichess.org/O5FD3mc2#348 after 174 games)

The mistakes are illegal moves. I don't necessarily want to rank illegal moves, but I found the three with pinned pieces a bit tricky. Anyway, rules are rules. 

A conclusion is that: 

* On the one hand, `gpt-3.5-turbo-instruct` is not 100% robust and can generate illegal moves in numerous situations. Though some "easy" fixes/workarounds are possible (by rewriting the generated text or "fairly" re-asking an actual move), there are some invalid moves that remain. If we consider "1-0", "comments", and "unspecified promotions" as fixable, then 1.5% of games still contain illegal moves. A possible general strategy is to ask GPT several ordered answers/completions, and choose the next move if the first try is illegal. In any case, some effort is needed to fix GPT and there is no guarantee it will be 100% accurate. 

* On the other hand, `gpt-3.5-turbo-instruct` is capable of playing very long games (up to 174 moves!) with an average game length of 50 moves (median is 44 moves). It's remarkable since an hypothesis is that GPT has more and more difficulties once a game progress, having more chances to encounter rare positions that are not in the training set.  Another remarkable perspective is to have a look at the frequency of legal moves: "only" 0.28% of moves are illegal (out of 25781 moves played against SF), and "only" 0.089% if we do not consider moves "1-0".  

  ![](/assets/gpt35_instruct_len_games_nmoves.png) 

* Obviously, all these numbers should be put in perspective. In a more critical domain, such errors can be  dramatic and considered as unacceptable. 





#### Elo rating against SF 

What is the rating of `gpt-3.5-turbo-instruct`? 

A basic estimate is to have a look at the scores against SF. For instance, `gpt-3.5-turbo-instruct` scores 148.5 for games with only legal moves out of 440 games (one win is 1, one draw is 0.5). A score of 33.75%: `score = (total_wins + (total_draws * 0.5)) / (total_wins + total_losses + total_draws)` 

In addition to the raw score, we have to take the estimated ratings of opponents (SF) into account. In detail, here is the distribution of games against different SF at different skills, together with Elo estimates (see previous explanations).

![](/assets/stockfish_elo_distribution.png) 





#### Robustness and Elo rating vs random engine

A set of experiments is related to games against a random chess engine that picks randomly a valid move across possible legal moves. The objective was first to assess ability of `gpt-3.5-turbo-instruct` to play legal moves, even with a strange opponent. 

Out of 61 total games, there were 45 legal games and 16 illegal games. (An illegal game is a game that contains an illegal move due to GPT). 

A manual analysis shows that illegal moves enter in the "accidental" category:

```
hxg8=      4
gxh8=      3
{[%clk     2
dxc8=      1
gxf8=      1
exf8=      1
cxd8=      1
{The       1
gxh1=      1
bxc1=      1
```



A first and main pattern is the incorrect promotion (as previously against SF). A second pattern of illegal moves is a kind of comment or strange textual "artefacts". 

Remarkably, the model won 98.89 % of *legal games* (44 games). 
The only not won game is a draw: https://lichess.org/bE99x52y is due to repetition. 

The legal games were short, mainly due to the poor level of a random chess engine: longest game was 33 moves, the shortest game was 2 moves (fun: `1. g4 e5 2. f4 Qh4# 0-1`), the average game length was 14.4 moves, the median game length was 14 moves.

The overall conclusion is that random chess engines are not disturbing much `gpt-3.5-turbo-instruct` and causing specific difficulty. The illegal moves can somehow been fixed (with a standard, non-AI rewriting procedure) and were already present against SF. **If we ignore accidental errors, `gpt-3.5-turbo-instruct` quickly destroys a random chess engine.**  





Score 100.0 % for games with only legal moves
6 legal games and 17 illegal games (out of 23 total games)
Illegal moves are           6
Bxd6      1
fxg7      1
exf6      1
Nf7+      1
Qxf8#     1
fxg8=Q    1
Qd5       1
...
cxd6      1
gxh8=Q    1
Name: illegal_move, dtype: int64
Quite severe example: https://lichess.org/FMoHsC7m with Bxd6













 







 







 














