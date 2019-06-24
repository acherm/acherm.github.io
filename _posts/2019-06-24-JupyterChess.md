---
layout: post
title: Jupyter and chess analysis
tags: [chess, python, data, stockfish, jupyter] 
---

[Jupyter](https://jupyter.org/) notebooks are awesome and widely used in data science. 
You typically share Web documents that contain live code and visualizations. 
In fact, you can do much more: I bet Jupyter will be the new Emacs and an operating system per se ;) 
So what about reading chess engines inside Jupyter? 

With a few lines of code, you can read a [PGN](https://en.wikipedia.org/wiki/Portable_Game_Notation) (portable game notation) file, loads the game you want, and visualize the moves (you can get back, move forward, etc.). It's like a Web application, but within Jupyter. Below is a demonstration:

![Extraction process](/assets/jupyerchess.gif)

I've also [shared a notebook](https://github.com/acherm/chess-jupyter/blob/master/AZ-PGN.ipynb) that offers facilities to produce a GIF of a game, to analyze a whole game with Stockfish. 
As a proof of concept, I used the games of Alpazero against... Stockfish to visualize when Stockfish changed its evaluation (aka when AlphaZero lost the mind of Stockfish... and certainly many chess players).

![Extraction process](/assets/stockfish-evo.png)


We can certainly consider a feature-rich chess reader with live streaming (you can embed Youtube videos!), online analysis with chess engines, etc. 
Code is available on Github [here](https://github.com/acherm/chess-jupyter)

What's the point of having a chess reader inside Jupyter? Obviously, there are better chess readers (go to the awesome and open source [lichess](https://lichess.org/editor)!). But technically I found impressive to build a shareable, Web application only with a few lines of Python and its very strong ecosystem (thanks to libraries like [python-chess](https://python-chess.readthedocs.io/en/latest/pgn.html), [CairoSVG](https://cairosvg.org/), etc.). You can easily and programmatically plug data analysis of it. For me, this short experiment mostly shows the potential of Jupyter: I'm not saying it's good to play games or read your emails inside Jupyter, but it seems feasible ;-) 














