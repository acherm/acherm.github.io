---
layout: post
title:  "Chess Piece Values (in Stockfish)"
date:   2023-02-16 011:54:29 +0200
tags: [chess, stockfish, alphazero]
---

What is the value of a queen, a bishop, or a knight in chess? 
There have been several attempts in the past, mainly in books to help assessing positions and taking a decision (eg when exchanging pieces). We'll all agree that you should not give your queen for a pawn in general, but chess is full of subtility and beauty! A Twitter thread nicely summarizes the different attempts https://twitter.com/martinmbauer/status/1624329951200649217. This nice Wikipedia article https://en.wikipedia.org/wiki/Chess_piece_relative_value lists different proposals, including the article "Assessing Game Balance with AlphaZero: Exploring Alternative Rule Sets in Chess" from Deepmind and Vladimir Kramnik as co-author (funny affiliation: "World Chess Champion
2000–2007"). I have been intrigued to know how Stockfish (the strongest, open source chess engine) encodes and deals with chess piece values. 
So here we go, let's dig into the source code and git repository. 


## Relative values in Stockfish 

A short investigation shows that `src/types.h` is a promising entry point. 
More specifically, these lines of code:
https://github.com/official-stockfish/Stockfish/blob/master/src/types.h#L192-L196
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
| Pawn | 1.00 |
| Knight | 6.20 |
| Bishop | 6.55 |
| Rook | 10.13 |
| Queen | 20.14 | 

Note: The values in the Values column have been rounded to 2 digits.

For end-games, the relative trend is similar:

| Piece | Value |
| ----- | ----- |
| Pawn | 1.00 |
| Knight | 4.90 |
| Bishop | 5.21 |
| Rook | 7.95 |
| Queen | 15.63 | 

Note: The values in the Value column have been rounded to 2 digits.

but all kinds of pieces but pawns are less powerful (intuition: a pawn can be promoted to a pawn). 
Numbers concur with the answer in stackexchange: https://chess.stackexchange.com/a/27391 


## Evolution of pieces' values
I have then investigated whether the pieces' values have evolved during Stockfish development. 
For tracking `src/types.h` over different commits, I've written a Bash script (with ChatGPT of course!):

 
 ```
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
``` 

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

Quite stable, though pawns' values have evolved more than other pieces. And clearly an effort of the contributors/devs of Stockfish to carefully and empirically tune these values. 

## Where are static values used? 

The different values above in `src/types.h` are used in the code base of Stockfish, in `material.cpp`, `search.cpp` and `evaluate.cpp` mainly through `non_pawn_material` or `nonPawnMaterial`   
For example: https://github.com/official-stockfish/Stockfish/blob/master/src/evaluate.cpp#L916-L934 
``` 
// For pure opposite colored bishops endgames use scale factor
// based on the number of passed pawns of the strong side.
if (   pos.non_pawn_material(WHITE) == BishopValueMg
    && pos.non_pawn_material(BLACK) == BishopValueMg)
    sf = 18 + 4 * popcount(pe->passed_pawns(strongSide));
// For every other opposite colored bishops endgames use scale factor
// based on the number of all pieces of the strong side.
else
    sf = 22 + 3 * pos.count<ALL_PIECES>(strongSide);
```

## What's next? 

This short blog post is actually the starting point of a (hopefully longer) series, where I will try to report on my understanding of the Stockfish source code. 
My writing is most probably inaccurate and incomplete. But it's good to me to keep traces of my knowledge. And secretly I hope to have feedbacks ;) 

Right now there are many open questions:
 * How are (*static*) chess piece values leveraged? I've only scratched the surface. Frankly speaking, I was surprised to see relative values somehow hardcoded in the code base. Neural networks (with NNUE) have I think a strong role in assessing chess positions, and the values are typically learned, not even explicitly (a surrogate is certainly needed to actually compute these values out of this blackbox, see next point). How these static values participate to the search/evaluation function and combine with the NN is a question for which I don't have answer. 
 * How are these values updated as part of Stockfish evolution? I'm suspecting some empirical tuning, but how it's made is unclear right now. Looking at the source code of Stockfish I'm always amazed and intrigued by the apparently ad-hoc values that are present here and there. Don't get me wrong, these values come from some human chess knowledge and/or empirical tuning, and thus not from nowhere. But we are far from the over-simplified explanations that a chess engine is "just" a black-box that have learnt a magical function evaluation. There are lots of engineering and community effort, especially for an hybrid chess engine like Stockfish. 
 * In the article about AlphaZero, the method to compute chess pieces' values is described in Section 3.8. The general principle is to compute a linear
model that predicts the game outcome based on the difference in numbers of each piece. As acknowledged, it is an approximation (and has the merit of being quite general to other chess variants). We can certainly apply similar methods for Stockfish (using games eg of Stockfish against itself).  

  

  







 














