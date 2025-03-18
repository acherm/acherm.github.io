---
layout: post
title:  "Sizecoding a cinematic experience fitting into 256 bytes"
date:   2025-01-22 011:54:29 +0200
tags: [programming, art, minification, wasm, sizecoding, code golf]
---

I came across a [video](https://www.youtube.com/watch?v=J6StCkzpLoQ) by the amazing Laurie Wired about the art of fitting a quite impressive cinematic experience into 256 bytes. 
Yes, 256 bytes. I'm usually interested by code minification/golfing (see, e.g., a chess puzzle resolver fitting in a Tweet here: https://blog.mathieuacher.com/ProgrammingChessPuzzles/).
I struggled a bit to find the mentioned code, and to make it work. The basic idea is using a language with a Rust-like syntax and with a few lines of code compile into WASM (Web assembly), and eventually execute in a browser using microW8. 
I'm quickly sharing here my experiment/experience, and hope it might be useful to someone else. 


[Sizecoding](http://www.sizecoding.org/) is the art of creating very tiny programs (a few bytes) for most popular types of CPUs. 
Plaforms like DOS or fantasy consoles are good candidates for sizecoding, and in general it is fun and challenging to make a program fit into such a small space. 
The project is called "Encounter" and was part of the Outline 2024 Demoscene party in the Netherlands. 
Specifically, you can find the solution here: https://github.com/ilmenit/sizecoding/blob/main/Encounter/Encounter.md 
as well as other solutions in the `sizecoding` repo. 

Here is the final result, impressive graphics demo with wave simulations, sky and water shaders, and transparency:

<iframe width="560" height="315" src="https://www.youtube.com/embed/4QY9WqbS61g" frameborder="0" allowfullscreen></iframe>


There was no special instructions on how to **build** the project, except some pointers to tools like CurlyWAS or microw8. 
Here is a script to automate (you need basic tools like Cargo, git, wasm-tools, etc.)

```bash
#!/bin/bash

set -e  # Exit on error

# Step 1: Clone necessary repositories
git clone https://github.com/exoticorn/curlywas
git clone https://github.com/ilmenit/sizecoding
git clone https://github.com/exoticorn/microw8

# Step 2: Build CurlyWAS
cd curlywas
cargo build --release
cd ..

# Step 3: Prepare Encounter project
cd sizecoding/Encounter
mkdir -p include
cp ../../microw8/examples/include/microw8-api.cwa include/

# Step 4: Compile Encounter with CurlyWAS
../../curlywas/target/release/curlywas encounter.cwa -o encounter.wasm

# Step 5: Optimize the WASM file for size
wasm-opt -Oz --strip-debug --strip-producers -o encounter_opt.wasm encounter.wasm
```

I get:
```
❯ stat -c %s sizecoding/Encounter/encounter_opt.wasm
559
``` 

And then it's working, just upload the wasm file using https://exoticorn.github.io/microw8/

I still don't know how to reduce the size further and obtain 256 bytes, but well, not that far. 
Anyway, an impressive demonstration of the art of sizecoding!
