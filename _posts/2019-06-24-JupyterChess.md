---
layout: post
title: Jupyter and chess analysis
tags: [chess, python, data, stockfish, jupyter] 
---

[Jupyter](https://jupyter.org/) notebooks are awesome and widely used in data science. 
You typically share Web documents that contain live code and visualizations. 
In fact, you can do much more: I bet Jupyter will be the new Emacs and an operating system per se ;) 
So what about reading chess engines inside Jupyter? 

With a few lines of code, you can read a PGN (portable game notation) file, loads the game you want, and visualize the moves (you can get back, move forward, etc.). Below is a demonstration:

![Extraction process](/assets/jupyerchess.gif)

I've also shared a notebook that offers facilities to produce a GIF of a game, to analyze a whole game with Stockfish. 
As a proof of concept, I used the games of Alpazero against... Stockfish to visualize when Stockfish changed its evaluation (aka when AlphaZero lost the mind of Stockfish... and certainly many chess players).

![Extraction process](/assets/stockfish-evo.png)

We can certainly consider a feature-rich chess reader with live streaming (you can embed Youtube videos!), online analysis with chess engines, etc. 

What's the point of having a chess reader inside Jupyter? Obviously, there are better chess readers (go to lichess!). But technically I found impressive to build a shareable, Web application only with a few lines of Python and its very strong ecosystem (thanks to libraries like pgnchess, SVG, etc.). You can easily and programmatically plug data analysis of it. For me, this short experiment mostly shows the potential of Jupyter: I'm not saying it's good to play games or read your emails inside Jupyter, but it seems feasible ;-) 












