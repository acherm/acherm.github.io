---
layout: post
title: Standalone Solution for Loading Models with Xtext  
tags: [xtext, java, parsing, model] 
---

[Xtext](https://www.eclipse.org/Xtext/) is an open-source and popular framework for the development of domain-specific languages. 
The contract is appealing: specify a grammar and you get for free (no effort) a parser (ANTLR), a metamodel, a comprehensive editor (working in Eclipse, or even in the Web), and lots of facilities for writing a compiler/an interpreter. 

For instance, you can write a compiler (in Java or Xtend) that generates on-the-fly when a user edits his or her programs (with the generated editor of course). Sometimes, there is the need to have a standalone solution because you want to develop outside Eclipse or simply because you want to integrate your compiler as part of a bigger thing (e.g., a Web application). 

ParserHelper is nice for unit testing your compiler (see [tutorial](https://www.eclipse.org/Xtext/documentation/103_domainmodelnextsteps.html#tutorial-unit-tests)). 
However ParserHelper depends on @RunWith(XtextRunner). 
So outside @Test methods, you cannot use it since dependency injection does not apply.

A possible workaround is to edit .mwe2 file and generate a "Main" class:
```
language = StandardLanguage {
            name = "org.xtext.example.mydsl.Mml"
            fileExtensions = "mml"
...            
            generator = {
                generateJavaMain = true 
            }            
            
        } 
```
It injects for you the good dependencies and so you have an executable, standalone Java program. 

Another solution is to use an helper function that loads the model out of a string content:
[see this gist](https://gist.github.com/acherm/bf7896a81d8db2c3726e82e8d4d921ec)

The principle is to write the string into a temporary file and parses this file (URI) to obtain a model. 
Then you can traverse your model and compile it. 
I provide similar facilities as part of an [MDE/DSL/SPL course at University of Rennes 1] (https://github.com/acherm/teaching-MDE-IL1819/blob/master/VideoGenTransformer/src/VideoGenHelper.xtend)

I thank [Manuel Leduc](https://mleduc.xyz/) for his nice advices about DSL and Xtext.


 





