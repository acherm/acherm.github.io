---
layout: post
title:  "Teaching Reproducibility and Embracing Variability: From Floating-Point Experiments to Replicating Research"
date:   2025-07-30 011:54:29 +0200
tags: [reproducibility, variability, replicability, metascience, software engineering, teaching, chess, soccer, football]
---

A short pause in the summer break... for a presentation at the 2025 ACM Conference on Reproducibility and Replicability (https://acm-rep.github.io/2025/), a super important but overlooked topic in science! I presented the paper entitled "Teaching Reproducibility and Embracing Variability: From Floating-Point Experiments to Replicating Research"

🇨🇦 The conference took place in Vancouver, but I presented remotely from France — live at 11:50 PM, and without the heatwave this time 😉.
🎓 I had the chance to present a 26-hour hands-on course I taught at INSA Rennes, entirely focused on reproducibility and variability in computational experiments.

The course blends scientific inquiry, advanced software engineering, and variability analysis. Students don't just talk about reproducibility — they reproduce and replicate actual research. This year, they explored:
⚽️ Home advantage in football during COVID https://blog.mathieuacher.com/FootballAnalysis-xG-COVIDHome/
♟️ Large language models playing chess https://blog.mathieuacher.com/GPTsChessEloRatingLegalMoves/
🔋 Energy consumption across programming languages (SCP 2021)

From a "Hello World" on floating-point arithmetic to full replications of published studies, students identified subtle reproducibility issues (like shifting library defaults) and explored meaningful variations — sometimes even spotting flaws in my own prior work!

📄 Read more in the paper: https://hal.science/hal-05190848v1
📊 Slides available here: https://www.slideshare.net/slideshow/teaching-reproducibility-and-embracing-variability-from-floating-point-experiments-to-replicating-research/282135360

🤝 This is joint work with Arnaud Gotlieb, Helge Spieker, and Gauthier Le Bartz Lyan, as part of the RIPOST team (Resilient and Reproducible Software — https://diverse-project.github.io/ripost_ea/), a collaborative effort between Simula Research Laboratory and Inria.

💡 Teaching reproducibility means embracing variability, critical thinking, and learning from students.
Can't wait to... replicate the course again at INSA Rennes in Fall 2025!

I have two remaining questions:
 * What's your hello world for reproducibility? Ideally simple to "get", engaging, and covering many technical and methodological aspects of reproducibility...
 * What are the success criteria of a reproducibility course? I had some evidence that the course went well (mainly based on high-quality works of students and their ability to reproduce and replicate in a very interesting way), but I'd like to have a kind of protocol to better assess the course. 

Raw transcript: 
```I'm Mathieu, I'm from France, it's around midnight remotely.
 And joint work with Arnaud Helgue and Gauthier. So I will try to share with you
 you the story of this course about reproducibility and how we come across
 across this yellow world floating point example and to
 more realistic scenario that aim to engage students in replication
 replicating research. So the story is as follows. We have a joint research team devoted to reproducibility. It's a collaboration between Inria and Simula. So between Rennes in Britany (France) and Oslo in Norway.
We try to think about resilience software
 software system. So software that are able to resist to some changes
 changes and that are quite reproducible regarding the output.
 When it's possible, of course, with the software engineering taste. And we had lots of discussion on reproducibility
 of course and at some point about how to teach reproducibility so to PhD students but also to undergraduate or graduate students. 
 On the other hand I'm professor at
 at INSA Rennes, which is one of the top engineering schools in France.
 and students are very strong, they have a great background in computer science
 in computer science, cybersecurity, data science, software engineering, et cetera. But I observed that there are
 are not necessarily interested by doing a PhD and by extension research is not the primary goal.
 So, I was a bit frustrated and I was thinking, "Okay, what about joining the two-person
 perspectives and trying to integrate reproducibility in a curriculum.
 in Rennes, at INSA. So there was a great tutorial last year at REP,
 about how to teach reproducibility with some strategies and with many services.
 surveys and advices by Fund et al. Notably there are several MOOCs
 There are several initiatives for reproducing papers and so on. But yeah, the efforts remain.
 scattered and I think that there is room for innovation and you have also to take into account the specifics of your teaching context.
 When discussing with my colleagues and based on experience here and there,
 I have I would say specific motivation one is
 I was thinking that reproducibilty was a way to concretely practice science because I was frustrating that students don't really read scientific papers or
 don't really try to revisit some paper or to analyze some data and
 and so on. So there was a kind of gap between what we are doing in research and what students
 are doing at the engineering school. So that's the first motivation.
 Also, I was thinking that in software engineering, reproducibility is a very important part
 will be more and more a key property. So many tools like version control, continuous integration,
 and infrastructure automation are actually related to reproducibility. So it's a good excuse to teach advanced
 and software engineering stuff. And a certain motivation is that
 I am an expert in software viability and I'm seeing a
 a deep connection with reproducibility and viability I will explain that afterwards so that is
 That is basically the three motivations we had. So we designed a reproducible course with two complementary steps. So the full course is about 26
 six hours with 20 students so
 medium size class, right? And there are two main steps. One is to have
 have a kind of hello world to motivate students through a kind of simple
 example on the challenges of reproducibility
 and especially on many sources of variability that can alter the results.
 So, I will explain afterwards, but basically we designed a floating-point arithmetic problem
 students vary programming language, compiler flag, numeric precision and so on and this way they can
 they can feel the challenges of reproducing an experiment.
 but also with the idea of exploring new ideas and new
 factors that may affect results. And then once you have done that, students
 students and kind of ready to go into the next step, which is more ambitious and more realistic,
 which is actually replicate research. So we propose three papers
 and students have to reproduce and replicate papers. So in the rest of the talk, I will...
 give some details about the Hello World floating point experiments.
 So the first part was quite simple.
 I first ask the question, is x plus y plus z equal to x plus y plus z with associativity?
 Most of the students were aware of that and then we find the question and 
 we asked the student how often x plus y plus z equals x plus y plus z.
 - Thank you, sir. You can address this question in different programming languages?
 with different compilers, with different libraries, with different computing environments, with different CPUs and whatnot.
 or whatever and perhaps perhaps you will get different results like different
 different percentage. So here you can see a small Python script
 That is quite okay, I would say, reasonable.
 But if you go into the details, the way you make the random,
 the absence of the seed and the fact that you select Python.
 can be revisited somehow and maybe you can have different results.
 So actually students play this game, right? They choose a programming language like
 They choose Python, Rust, Java, C or whatever. They even try with Scratch.
 each group shares the solution through a git and
 of course I ask the students that their works are reproducible
 So that out of the readme, out of all the artifacts including Dockerfile and so on, you can reproduce the results. And then, group of students can confront
 their results and discuss why their percentage differ and what are the cause of
 of these discrepancies. So it could be about the type of the variable
 variables you use, about the strategy for picking random numbers. It could be about compiler
 of flags, precision you're asking and so on. So basically students identify viability
 factors. And then out of that they really try to understand how
 how variability factors impact the results. So I'll tell them
 them some product line techniques in the software engineering field in which
 in which you can make some templates and create variations of an experiment.
 collect some results and then analyze it. So here it's an excerpt of one group that
 test different random strategy you see so there is random there is
 there is uniform with different min and max values and of course all this
 stuff can have an influence on the results. So that's the first thing.
 thing and and then there are also different styles to implement variability you can use a common
 command line interface. I ask them some exercise and so on. Again, that's the same spirit.
 spirit, they try to vary something and see the effect and analyze the results and basically provide a summary of
 of what they have learned. And it's just Python code, but there is...
 There is also the Dockerfile that are also subject to variability. And there are github actions that automate everything.
 So the results we got here is that students explored diverse variabilities.
 more details in the paper, but most focused on traditional viability factors like
 like number of repetitions, value range for picking random values, and occasionally they
 they replicate somehow testing beyond associativity, they test like commutativity or something
 or strange numbers like p sometimes. Overall, they tend to converge.
 There was a kind of consensus. So it was, I think, a good era
 because it challenges students to master software engineering tools and methods to automate
 automate and to identify variability factors. Of course, it has limits because it does not fully capture the
 the complexity of reward reproducibility but in a sense it's a good prerequisite before practice
 practicing reproducibility and science in a more realistic skin eye. And that's the second step of the course.
 so they have something like more than 12 hours to do that, they have to reproduce and replicate
 an actual research paper. So that's the slide I presented, there is a bit of French. They
 They have to choose one project out of three available, so there are three, and they have
 reproduce first the results of the paper so exactly the same result and then replicate so identify variability factors that
 that are interesting, that are making a new...
 kind of analytical method or new hypothesis and then they have to answer with the
 whether it confirms original results. So I propose this year three projects, one about
 One about chess and large language model, one about energy efficiency and programming
 languages and other about home advantage in football. I will focus on this subject
 second project because it gives I think the most interesting results.
 The research content is actually based on a blog post I wrote myself. I was the instructor
 of this course. And I basically show that home advantage was quite a
 of new during COVID and you close source stadium basically.
 And I provide evidence of that based on results, expected goals, expected points, and so on.
 and so on. Okay. And then students have to reproduce the
 the results of my blog post. And I must precise that
 the source code was not available. It wasn't proposed. They have to re-implement.
 did re-implement what I did and they found quite similar reasons
 but also notable differences. So for instance, they found that p values are twice as high as m
 as my in my original blog post for some aspects of the results. And I
 I was surprised by that actually and after investigation with students we found out that
 that it was due to the version of SciPy I used in 2021, which was 1.6.
 in which the default parameter about
 man Whitney test change the
 the default value of the two-sided method. And that explain why
 it was a but it
 It was very interesting to have this result by the students because it changes
 it changes a bit the significance of the results. So that's the first, I think,
 great result because basically students reproduce greatly my work and
 In a sense it backfires to me, right? And another key finding is that they
 replicate my original blog post by considering years of
 of 2022, after COVID basically, they also consider new leagues
 and so on and it confirms somehow the results I have met but they're also
 also new insights. So that was very interesting. And I think worth publishing!
 So I will skip because I'm running out of time, but basically the conclusion is that
 we have designed a self-contained reproducibility course with advanced
 software engineering with the excuse of practicing science including
 critical thinking and replication and viability is at the heart of the process. Two-step
 steps approach earlier world for reproducibility and a more ambitious reproducibility
 reproduction and replication of actual research. I think the course was a success because high quality works with Git
 repository. Students were able to pinpoint property stability issues and to replicate and explore new
 new hypotheses. So we learn a lot from students, which is good news, and from teaching. And I can't wait to replicate the course next fall in upcoming months.
 So thank you for your attention.
 answer questions. Thank you very much.
 very much for the presentation. Yes?
 Yes, let's take one or two quick questions.
 - Hello, is this the right mic? Yeah, okay, this is Freida Fund.
 I was wondering if you could comment a little bit on the motivations of students taking this course, what do you think they were hoping to
 get out of a course on reproducibility? Why did they choose to sign up for this course?
 - Okay, good question. So this course was kind of optional course,
 But you know, they have to take some optional courses.
 I don't know what the specific motivation, but I guess the idea of conducting
 rigorous experiments knowing a bit more about reproducibility and engaging
 with real scientific papers was
 was key to their choices. Before choosing the courses, I have the opportunity
 opportunity to present and to tease them what will be the content of the course.
 I guess it was like this. And now, an informal feedback I have is they want more
 courses in which you can play with papers or produce papers. Great, thank you.```




