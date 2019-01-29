---
layout: post
title: Jupyter and Markdown 
tags: [data, python, jupyter, markdown] 
---

You can write Markdown as part of Jupyter notebooks. 
The basic idea is to use a special "Cell Type". 
An important feature I struggle to activate is the ability to call a variable inside Markdown. 

Something like
```
maxv = 1.2 # after a long computation
```
then you want to write Markdown that refers to maxv
```
The maximum value found is {{maxv}} and suggests that blabla
```

It should be rendered as 
```
The maximum value found is 1.2 and suggests that blabla
```

After some tries and errors with pip3, jupyter, etc. as well as some cryptic messages (compatibility issues, validation that does not match, etc.)
I succeed to activate it with:

```jupyter nbextension enable python-markdown/main```

(the "main" is important and actually refers to main.js)
 





