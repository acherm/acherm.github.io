---
layout: post
title:  "A PhD Defense on Legacy Software Modernization: from Esope to Fortran 2008"
date:   2026-03-24 011:54:29 +0200
tags: [legacy software, modernization, Fortran, Esope, parsing, software migration, PhD defense, LLM4Code]
---

I had the pleasure of serving as a reviewer for Younoussa Sow's PhD defense in Lille, on a topic that is both challenging and fascinating: the automatic migration of Esope to Fortran 2008. This CIFRE PhD, carried out with [Framatome](https://www.framatome.com/) and co-supervised by [Nicolas Anquetil](https://chercheurs.lille.inria.fr/~nanqueti/nicolasAnquetil.html) and [Stéphane Ducasse](http://stephane.ducasse.free.fr/), tackles a major modernization challenge: migrating around one million lines of scientific code built on Esope, a proprietary extension on top of Fortran 77.

What makes this work especially difficult is that the Esope ecosystem is closed and "legacy". 
That makes parsing, transforming, and modernizing the code particularly hard, from a software engineering perspective.
The defense strongly resonated with my own current interests in RPG and COBOL modernization, and more broadly with the challenges posed by legacy or unusual/esoteric languages. No LLMs in this thesis, but many questions that connect with ongoing discussions around software migration and modernization, including in the context of LLM4Code.

It was also a great opportunity to exchange, the day before, on these topics. The defense was a reminder of two important things. First, how valuable it is to attend PhD defenses in person, to better understand the story, context, collaboration dynamics, and difficulties behind the work. Second, how essential computer science is to the evolution of critical systems across many domains... and how much we need computer scientists to make these systems evolve.

Two major publications from the thesis:
 * Younoussa Sow, Nicolas Anquetil, Léandre Brault, Stéphane Ducasse -- Migrating Esope to Fortran 2008 using model transformations (SANER 2026)
 * Younoussa Sow, Larisa Safina, Léandre Brault, Papa Ibou Diouf, Stéphane Ducasse, Nicolas Anquetil -- Parsing Fortran-77 with proprietary extensions (ICSME 2023, pp. 453–462)

Congratulations to Younoussa Sow for the defense, and thanks to the whole team for the discussions!


*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2026phddefenseesope,
  author = {Mathieu Acher},
  title = {A PhD Defense on Legacy Software Modernization: from Esope to Fortran 2008},
  year = {2026},
  month = {mar},
  howpublished = {\url{https://blog.mathieuacher.com/PhDDefenseLegacySoftwareModernizationEsopeFortran/}},
  note = {\url{https://blog.mathieuacher.com/PhDDefenseLegacySoftwareModernizationEsopeFortran/}}
}
```
