---
layout: post
title:  "Thanks ChatGPT for the tricks with PlantUML layout"
date:   2023-10-10 011:54:29 +0200
tags: [modelling, UML, language, ChatGPT, LLM, layout]
---

Customizing the layout (or look and feel) of PlantUML diagrams is tricky. 
There is a dense and comprehensive [documentation](https://plantuml.com/class-diagram), but there is still a gap between what we want and what we get. 
There are tricks and hacks to overcome default behavior or missing features of PlantUML. 
Here I am sharing a fun session with ChatGPT that have successfully assisted me in a quite creative way! 
The solution seems not to be in the documentation, and the "hack" can certainly be systematically applied. 
Or did I miss something with PlantUML?

## Session and first tries


I had this PlantUML model and class diagram:

```
@startuml
class Model {
}

class Poll {
    +name: String
}

class Question {
    +id: String
    +text: String
}

class Option {
    +id: String
    +text: String
}

Model --> "1..* polls" Poll
Poll --> "1..* questions" Question
Question --> "1..* options" Option
@enduml
``` 

The layout was not as I wanted.

![first layout](/assets/plantUMLFirstLayout.png)

So, I asked the right question to ChatGPT, with a perfect English: 

<blockquote style='display: inline; quotes: "\201C""\201D""\2018""\2019"; border-left: 10px solid #ccc;'>How to change the layout and have a horizontal?</blockquote>

ChatGPT recommended to use `left to right direction`. 
Indeed, it's something I've used in the past and actually written in the documentation. 

![second layout](/assets/plantSecondLayout.png)

Unfortunately, the cardinality and roles' names are not really readable. The association should be longer and well, we need more space. 

<blockquote style='display: inline; quotes: "\201C""\201D""\2018""\2019"; border-left: 10px solid #ccc;'>How to enlarge space from left to right?<blockquote>

The recommendation about `packagePadding` parameter was not helpful. 

## Creativity and tricks: a signal

`It has no visible effect.. any other idea?` 

And here it's beginning to be interested.

```
@startuml
left to right direction

skinparam classBackgroundColor Transparent
skinparam classBorderColor Transparent
skinparam shadowing false

class Model {
}

class Spacer1 << (I,transparent) >> {
}

class Poll {
    +name: String
}

class Spacer2 << (I,transparent) >> {
}

class Question {
    +id: String
    +text: String
}

class Spacer3 << (I,transparent) >> {
}

class Option {
    +id: String
    +text: String
}

Model -[hidden]-> Spacer1
Spacer1 -[hidden]-> Poll
Poll -[hidden]-> Spacer2
Spacer2 -[hidden]-> Question
Question -[hidden]-> Spacer3
Spacer3 -[hidden]-> Option

Model --> "1..* polls" Poll
Poll --> "1..* questions" Question
Question --> "1..* options" Option
@enduml
```

![third layout](/assets/plantThirdLayout.png)

It's not working, but the idea is brilliant: faking elements to enlarge spaces and forcing the layout to move the cardinalities/roles' names...
and then hiding these elements! 

## Complaining 

A magical recipe in prompt engineering is of course to complain

<blockquote style='display: inline; quotes: "\201C""\201D""\2018""\2019"; border-left: 10px solid #ccc;'>not working</blockquote>

and here is the new proposition with "notes" to fake modeling elements. 

```
@startuml
left to right direction

class Model {
}

note " " as N1

class Poll {
    +name: String
}

note " " as N2

class Question {
    +id: String
    +text: String
}

note " " as N3

class Option {
    +id: String
    +text: String
}

Model -[hidden]-> N1
N1 -[hidden]-> Poll
Poll -[hidden]-> N2
N2 -[hidden]-> Question
Question -[hidden]-> N3
N3 -[hidden]-> Option

Model --> "1..* polls" Poll
Poll --> "1..* questions" Question
Question --> "1..* options" Option
@enduml
```

Very clever ChatGPT! 
A remaining issue is that "notes" are visible...
And from what I have learned in prior interactions (see above) it's certainly possible to hide elements.

## ChatGPT + Human to hack 

<blockquote style='display: inline; quotes: "\201C""\201D""\2018""\2019"; border-left: 10px solid #ccc;'>can't you use skinParams now to make notes invisible?</blockquote>

and here is the final result

```
@startuml
left to right direction

skinparam noteBackgroundColor Transparent
skinparam noteBorderColor Transparent
skinparam noteFontColor Transparent
skinparam shadowing false

class Model {
}

note " " as N1

class Poll {
    +name: String
}

note " " as N2

class Question {
    +id: String
    +text: String
}

note " " as N3

class Option {
    +id: String
    +text: String
}

Model -[hidden]-> N1
N1 -[hidden]-> Poll
Poll -[hidden]-> N2
N2 -[hidden]-> Question
Question -[hidden]-> N3
N3 -[hidden]-> Option

Model --> "1..* polls" Poll
Poll --> "1..* questions" Question
Question --> "1..* options" Option
@enduml
```

Absolutely perfect! 

![final layout](/assets/plantfinalLayout.png)

## Conclusion 

ChatGPT was a clever assistant to customize the layout of a PlantUML diagram.
Mastering the documentation (aka RTFM) was not in my capacity... 
**Teaming up with ChatGPT** was a way to (1) speed up or remember some knowledge of PlantUML; (2) get inspiration of some possible strategies. 
At the end, I used what I've learned about PlantUML to eventually adapt the inspired and creative proposition of ChatGPT. 
And retrospectively I have impression the proposed "tricks" of faking model elements and then hiding them is a very effective strategy for PlantUML. 
What are your best practices for PlantUML layouts? Or am I just an ignorant of how to properly use PlantUML?





















 







 







 














