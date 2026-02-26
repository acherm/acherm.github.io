---
layout: post
title:  "Stockfish(1): Chess Piece Values"
date:   2023-02-16 011:54:29 +0200
tags: [chess, stockfish, alphazero]
---

What is the value of a queen, a bishop, or a knight in chess? 
There have been several attempts in the past, mainly in books to help assessing positions and taking a decision (e.g., when exchanging pieces). We'll all agree that you should not give your queen for a pawn in general, but chess is full of subtlety and beauty! A [Twitter thread](https://twitter.com/martinmbauer/status/1624329951200649217) nicely summarizes the different attempts. This [nice Wikipedia article](https://en.wikipedia.org/wiki/Chess_piece_relative_value) lists different proposals, including the article "Assessing Game Balance with [AlphaZero: Exploring Alternative Rule Sets in Chess" from Deepmind](https://arxiv.org/abs/2009.04374) and Vladimir Kramnik as co-author (funny affiliation: "World Chess Champion
2000–2007"). I have been intrigued to know how [Stockfish](https://stockfishchess.org/) (the strongest, open source chess engine) encodes and deals with chess piece values. 
So here we go, let's dig into the source code and [git repository](https://github.com/official-stockfish/Stockfish)! 


## Relative values in Stockfish 

A short investigation shows that `src/types.h` is a promising entry point. 
More specifically, the following [lines of code](https://github.com/official-stockfish/Stockfish/blob/master/src/types.h#L192-L196):
```
PawnValueMg   = 126,   PawnValueEg   = 208,
KnightValueMg = 781,   KnightValueEg = 854,
BishopValueMg = 825,   BishopValueEg = 915,
RookValueMg   = 1276,  RookValueEg   = 1380,
QueenValueMg  = 2538,  QueenValueEg  = 2682,
```

`Md` means middle game and `Eg` means end-game. When a middle game starts and stops is another story, but it makes sense to distinguish both phases. 
In fact some proposals listed in the Wikipedia article also make this distinction. 
Overall, we obtain the following relative values in middle game, assuming a pawn has value 1: 

| Piece | Value |
| ----- | ----- |
| Pawn  | 1.00 |
| Knight | 6.20 |
| Bishop |  6.55 |
| Rook |  10.13 |
| Queen | 20.14 | 

(Note: The values in the Value column have been rounded to 2 digits.)

The usual piece values are pawn=1, knight=3, bishop=3, rook=5, queen=10 (see [a list of piece values' proposal](https://en.wikipedia.org/wiki/Chess_piece_relative_value) or the [AlphaZero article](https://arxiv.org/abs/2009.04374)). 
Hence, I may have missed something, but the relative values are a bit different w.r.t pawns. Otherwise, it's in line with what have been proposed. 
Another (important) note: *how* these relative values are leveraged as part of the search/eval functions should be taken into account. 
I can imagine, for instance, some heuristics to augment or downgrade relative values depending on some positions or other variables/parameters' values (e.g., thresholds). 
Stated differently, pieces' values are tuned to work along other factors in Stockfish's algorithm (there are many other factors!)... 
And not necessarily to reflect on the way human chess players internalize the actual values of pieces. 
There is an interesting [reddit discussion](https://www.reddit.com/r/chess/comments/e57lqz/stockfish_doesnt_use_the_traditional_piece_values/).

For end-games, the *relative* trend is similar:

| Piece | Value |
| ----- | ----- |
| Pawn | 1.00 |
| Knight | 4.90 |
| Bishop |  5.21 |
| Rook | 7.95 |
| Queen | 15.63 | 

(Note: The values in the Value column have been rounded to 2 digits.)

Yet, all kinds of pieces, but pawns are less powerful (intuition: a pawn is more likely to be promoted to a pawn in end-game). 
Numbers concur with the [answer in stackexchange](https://chess.stackexchange.com/a/27391).


## Evolution of pieces' values
I have then investigated whether the pieces' values have evolved during Stockfish development. 
For tracking `src/types.h` over different commits, I've written a Bash script (with ChatGPT of course!):

 
{% highlight bash %}
#!/bin/bash

FILE=$1

# Get the list of all the commit hashes that modified the file
COMMITS=$(git log --format=%H --follow -- "$FILE")

# Loop through each commit and show the diff between it and the previous commit
PREV_COMMIT=""
for COMMIT in $COMMITS; do
  if [ -n "$PREV_COMMIT" ]; then
    echo "Diff for commit $COMMIT:"
    git diff "$PREV_COMMIT" "$COMMIT" -- "$FILE"
    echo ""
  fi
  PREV_COMMIT=$COMMIT
done
{% endhighlight %}

It gives the following results:

```
./git_full_diff.sh src/types.h > diffs.txt
cat diffs.txt | grep -e "+  QueenValueMg"
+  QueenValueMg  = 2529,  QueenValueEg  = 2687,
+  QueenValueMg  = 2528,  QueenValueEg  = 2698,
+  QueenValueMg  = 2527,  QueenValueEg  = 2697,
+  QueenValueMg  = 2500,  QueenValueEg  = 2670,
+  QueenValueMg  = 2526,  QueenValueEg  = 2646,
+  QueenValueMg  = 2513,  QueenValueEg  = 2648,
+  QueenValueMg  = 2513,  QueenValueEg  = 2650,
+  QueenValueMg  = 2521,  QueenValueEg  = 2658,
+  QueenValueMg  = 2521,  QueenValueEg  = 2558,
+  QueenValueMg  = 2521,  QueenValueEg  = 2558
cat diffs.txt | grep -e "+  PawnValueMg"
+  PawnValueMg   = 124,   PawnValueEg   = 206,
+  PawnValueMg   = 128,   PawnValueEg   = 213,
+  PawnValueMg   = 136,   PawnValueEg   = 208,
+  PawnValueMg   = 142,   PawnValueEg   = 207,
+  PawnValueMg   = 175,   PawnValueEg   = 240,
+  PawnValueMg   = 171,   PawnValueEg   = 240,
+  PawnValueMg   = 188,   PawnValueEg   = 248,
+  PawnValueMg   = 198,   PawnValueEg   = 258,
```

It is quite stable, though pawns' values have evolved more than other pieces. And clearly there is an effort of the contributors/devs of Stockfish to carefully and empirically tune these values. 

## Where are static values used? 

The different values above in `src/types.h` are used in the code base of Stockfish, in `material.cpp`, `search.cpp` and `evaluate.cpp` mainly through `non_pawn_material` or `nonPawnMaterial` 
For example: https://github.com/official-stockfish/Stockfish/blob/master/src/evaluate.cpp#L916-L934 
{% highlight C %}
// For pure opposite colored bishops endgames use scale factor
// based on the number of passed pawns of the strong side.
if (   pos.non_pawn_material(WHITE) == BishopValueMg
    && pos.non_pawn_material(BLACK) == BishopValueMg)
    sf = 18 + 4 * popcount(pe->passed_pawns(strongSide));
// For every other opposite colored bishops endgames use scale factor
// based on the number of all pieces of the strong side.
else
    sf = 22 + 3 * pos.count<ALL_PIECES>(strongSide);
{% endhighlight %}

## What's next? 

Right now there are many open questions:
 * How are (*static*) chess piece values leveraged? I've only scratched the surface. Frankly speaking, I was surprised to see relative values somehow hardcoded in the code base. Neural networks (with [NNUE](https://www.chessprogramming.org/NNUE)) have I think a stronger role/potential in assessing chess positions, and the values are typically learned, not even explicitly (a surrogate is certainly needed to actually compute these values out of this blackbox, see next point). How these static values participate in the search/evaluation function and combine with the NN is a question for which I don't have answer. I have a hypothesis: *(H1) there is "classical" evaluation and a NNUE evaluation, the two are simply mutually exclusive and there is no usage of static values by NNUE.* 
 Another hypothesis is that *(H2) these relative piece values cannot be interpreted "as such" (as absolute values) and without understanding their exact exploitation as part of Stockfish*
 * How are these values updated as part of Stockfish evolution? I'm suspecting some empirical tuning, but how it's made is unclear right now. Looking at the source code of Stockfish I'm always amazed and intrigued by the apparently ad-hoc values that are present here and there. Don't get me wrong, these values come from some human chess knowledge and/or empirical tuning, and thus not from nowhere. But we are far from the over-simplified explanations that a chess engine is "just" a black-box that have learned a magical function evaluation. There are lots of engineering and community effort, especially for a hybrid chess engine like Stockfish. I have a hypothesis: *(H3) the tuning of Stockfish occurs in many other places and for other "variables" than static relative piece values that are not that often modified.* 
 * In the [article about AlphaZero](https://arxiv.org/abs/2009.04374), the method to compute chess pieces' values is described in Section 3.8. The general principle is to compute a linear
model that predicts the game outcome based on the difference in numbers of each piece. As acknowledged, it is an approximation (and has the merit of being quite general to other chess variants). We can certainly apply/adapt similar methods for Stockfish (using games eg of Stockfish against itself).  

Overall, I am not sure static chess pieces' values, as encoded in Stockfish, can be transferred to human chess players' mental model. 
The encoding serves a specific computational purpose and pieces' values interact with a lot of other factors as part of Stockfish algorithm. 
This short blog post is actually the starting point of a (hopefully longer) series, where I will try to report on my understanding of the Stockfish source code. 
My writing is most probably inaccurate and incomplete (e.g., I have written 3 hypotheses that need more research). 
But it's good to me to keep traces of my current knowledge. And secretly I hope to have feedbacks ;)

  







 















*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2023chesspiecesvalues,
  author = {Mathieu Acher},
  title = {Stockfish(1): Chess Piece Values},
  year = {2023},
  month = {feb},
  howpublished = {\url{https://blog.mathieuacher.com/ChessPiecesValues/}},
  note = {\url{https://blog.mathieuacher.com/ChessPiecesValues/}}
}
```
