---
layout: post
title:  "On the Longest Chess Game Ever(!?)"
date:   2020-04-10 011:54:29 +0200
tags: [chess, programming, sigbok]
---

Do you want to see a chess game with almost 18,000 half-moves? Really? OK, here is a [Youtube video of almost 5 hours](https://www.youtube.com/watch?v=XhnxuWKYm-w)
But wait: How is it possible? What's the point? In practice, the longest games are up to 250 moves and such games are really outliers (the mean is certainly less than 40 moves).
So Tom Murphy did it again with his [6th chess paper](http://tom7.org/chess/ in a row) at the very prestigious [SIGBOVIK 2020](http://sigbovik.org/2020/) (have a look at other papers, it's both funny and brilliant).
Tom generated a game of 17,697 plies (8849 moves), certainly the longest chess game ever.

# Tricks

8K moves is much, much more than 40. You can try to manually move some pieces on a chessboard and generate a long game, but I guess you won't try much ;)
The idea is of course to simulate a very long game with a *program*.
You can start quite naively with a program that generates a sequence of random, legal moves.
However, the game might end quickly since there could be an early checkmate or no material leading to draw along the way. Reaching 18K seems hard! 

The idea of Tom is to exploit the so-called 75-move rule. I was aware of the 50-move rule, but not the 75 one. Let me explain. 
The 50-move rule is well-known and states that a player can claim a draw if no capture has been made and no pawn has been moved in the last 50 moves. 
But "in 2014 FIDE amended the rules to eliminate the possibility that a game could continue without end. Rule 9.6b states that if 75 consecutive moves have been made without movement of any pawn or any capture, the game is drawn..." see https://en.wikipedia.org/wiki/Fifty-move_rule for more details.
You can use and abuse of this trick to generate sequence of moves with no capture during 75 moves. Basically, nothing special happens, it's just ridiculous (?) movement to make 75 moves without captures and thus increment the length of the game. 

Another trick is the ~~threefold fivefold repetition. Here I'm quoting the paper:

```
The 75-move rule is rarely applied in practice, but its counterpart, "threefold repetition" is often the cause of draws in chess.
This rule states that if the same position appears three game ends prematurely in a draw because of the 75-move rule.
times, the players can claim a draw:
       9.2.2. Positions are considered the same if and only if the
       same player has the move, pieces of the same kind
       and colour occupy the same squares and the possi-
       ble moves of all the pieces of both players are the
       same. [...]
Like the 75-move rule, this rule has an optional version (upon
three repetitions) and a mandatory one in 9.6:
      [The game is a draw if . . . ]
      9.6.1. the same position has appeared, as in 9.2.2, at
      least five times
```

Basically the game is so long because precisely Tom Murphy nicely exploits the 75-move rule and the 5-fold repetition.
There are of course other subtilities and hacks, and I let you discover them ;) 

## Viewing the game 

Before digging into the details, my first reaction was to view/see the generated game.
8K moves is quite unusual though. It might be for these reasons that the awesome [lichess](https://lichess.org/) open-source service has a 300 moves limit (see https://reddit.com/r/chess/comments/dgkp8c/til_there_is_a_300move_limit_for_games_on_lichess/ for a discussion) when importing a game in PGN format. I've heard Chessbase has the same 300 limitations.
I've tried many services until finding one that can truly open the PGN file (btw accessible here: http://tom7.org/chess/longest.pgn). 
@chesscom_fr fails, chesspastebin stops after 4400 moves https://chesspastebin.com/view/22411.
I've succeeded with https://ingram-braun.net/erga/online-pgn-viewer/ after a subtle edit.

Indeed, "1786. Qb4 Rg8" is considered ambigous as both rooks on h8 and g6 can move on g8. So you have to modify the PGN (see below). 
I was suspecting Tom did it on purpose ("Many chess programs fail to load the whole game, but this is because they decided not to implement the full glory of chess.")
but in fact he gave an interesting comment on Twitter https://twitter.com/tom7/status/1245909267720343554
```
You give me too much credit! :) This was actually a bug in my PGN code (it considered rook and rook-that-can-still-castle as different for disambiguation purposes).
Now fixed (should be Rgg8 since file-based disambiguation takes precedence), thanks!!
```

Anyway, I've also succeeded to open the game with my chess player reader made on top of Jupyter notebook (see my blog post: http://blog.mathieuacher.com/JupyterChess/) and with python chess library.

## Youtube and GIFs

I've generated three videos of the chess game (all uploaded to Youtube):
 * https://www.youtube.com/watch?v=XhnxuWKYm-w almost 5 hours, 1 second per ply... you can incrase the speed through parameters (x2 max?)
 * https://www.youtube.com/watch?v=Y0QurXfu-EY another variant, less than 3 minutes with a huge speed up (100 frames/plies per second)
 * https://www.youtube.com/watch?v=KXVPpRZ0UkU yet another variant, less than 4 minutes with 75 plies per second (to mimin the 75-rule and possible "repetitions")

I've tried to generate some GIFs, but it's hard to find a good tradeoff between size, quality and move speed of the resulting GIF file.
You can get a file of 1Gb and a visually unpleasant series of images. 
Some attempts (I assume you have generated the 17K+ positions as PGN files named output1.pgn, output2.pgn, ..., ouput17686.pgn):
 * with `convert` beware of the order of the file, the time/memory it can take... basically `convert $(for a in `ls *.png | sort -V`; do printf -- "-delay 50 %s " $a; done; ) output.gif` is OK, but it assumes that PNG files are 200x200, otherwise the process is killed (17K+ files to assemble!)
 * with `ffmpeg` beware of the order (again) `ffmpeg -framerate 10 -start_number 1 -i 'output%d.png' output.gif` is OK, but the file can be quite big
 * you can also try a conversion of mp4 file to GIF with `ffmpeg`
 * in any case, I recommend the use of the awesome gifsicle to optimize the size of the file, with something like `gifsicle -i output.gif --resize 100x100 -O3 --colors 10 -o output-opt.gif`

## Going further

Resources:
 * paper: [http://tom7.org/chess/longest.pdf](http://tom7.org/chess/longest.pdf)
 * source code: [https://sourceforge.net/p/tom7misc/svn/HEAD/tree/trunk/chess/longest.cc#l1](https://sourceforge.net/p/tom7misc/svn/HEAD/tree/trunk/chess/longest.cc#l1)
 * game (PGN format): [http://tom7.org/chess/longest.pgn](http://tom7.org/chess/longest.pgn)

Some ideas for future work:
 * obvious one: Can we generate a longer chess game? As stated in the paper, the generated game is clearly not unique... I think there is room to experiment further with the algorithm and the code. A formal proof that it is (if it is the case) the longest game would be great.
 * evolution of the game: the game is not a realistic one with series of moves that would look crazy to human and chess players... I'm wondering how a computer chess engine like Stockish would evaluate the quality of the moves and the overall game. I'm suspecting many "swings" can occur, with black winning, white winning, and so forth. Some challenges: chess engines implement the 50-rule and not the 75-rule... we have to scale up the process: with 17K plies to analyze, and at least a few seconds per position needed to compute the evaluation score, you certainly need several machines. I would like to integrate the score evolution as part of the Youtube video, as an additional piece of art.
 * sonification of the game: related to the previous point, maybe we can associate a sound to the (score) evolution of the game. It would be a long and wonderful movie. (by the way, in general, sonification of chess games based on the input of chess engines has not caught much attention: would love to explore this idea!) 

I really enjoy reading the paper, the idea, the algorithm, the source code. It is a piece of art. 





