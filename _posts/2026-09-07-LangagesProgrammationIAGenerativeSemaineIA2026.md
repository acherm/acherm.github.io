---
layout: post
title:  "Langages de programmation à l'ère de l'IA générative : COBOL, M&M's, français… ou peu importe ?"
date:   2026-09-07 12:00:00 +0200
tags: [LLM, coding agents, Claude Code, generative AI, llm4code, programming languages, COBOL, Brainfuck, esoteric languages, chess, vibe coding, talk, Semaine de l'IA]
hidden: true
---

Vendredi 4 septembre 2026, j'ai donné un exposé dans le cadre de la [Semaine de l'IA de l'Université de Rennes](https://intelligence-artificielle.univ-rennes.fr/semaine-ia-2026), devant quelque 230 étudiantes et étudiants (en cœur IA ou coloration IA), sur ce que les agents de codage font des langages de programmation, de Python à COBOL en passant par Brainfuck et les M&M's. L'enregistrement est en ligne, ainsi que les slides, en français et en anglais.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Aj5arTQzMQ4?start=822" title="Langages de programmation à l'ère de l'IA générative — Semaine de l'IA 2026" frameborder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- **Vidéo de l'exposé** : [youtube.com/watch?v=Aj5arTQzMQ4](https://www.youtube.com/watch?v=Aj5arTQzMQ4&t=822s) (l'exposé commence à 13 min 42 s)
- **Slides en français** : [blog.mathieuacher.com/languages-codingagents-semaineIA2026](https://blog.mathieuacher.com/languages-codingagents-semaineIA2026/)
- **Slides in English**: [blog.mathieuacher.com/languages-codingagents-semaineIA2026/en.html](https://blog.mathieuacher.com/languages-codingagents-semaineIA2026/en.html)
- **Sources** (reveal.js, vidéos et figures comprises) : [github.com/acherm/languages-codingagents-semaineIA2026](https://github.com/acherm/languages-codingagents-semaineIA2026)

Les slides sont auto-contenues : vidéos des jeux, extraits de console, figures des papiers, tout est dedans. Touche `S` pour les notes, `O` pour la vue d'ensemble. Une trentaine de slides de « backup » suivent les références, pour la discussion.

## Résumé

À l'ère de l'IA générative et du *vibe coding*, programmer peut commencer par une simple description en langue naturelle, en anglais ou en français, de ce que l'on souhaite obtenir. Des agents de codage peuvent aujourd'hui produire des sites web, des jeux, des solveurs, voire des compilateurs ou des outils plus ambitieux, avec des résultats parfois impressionnants. Pour tester leurs limites, on peut les confronter à des langages très différents : Python, Rust ou JavaScript, mais aussi COBOL, l'un des plus vieux langages encore utilisés, Brainfuck, un langage composé de seulement 8 caractères, ou même un langage à base de M&Ms.

Ces expériences donnent lieu à des démonstrations étonnantes : un jeu DOOM en COBOL, un jeu d'échecs en Brainfuck ou des programmes non triviaux écrits avec des bonbons colorés. L'intérêt est que ces programmes ne sont pas de simples copies retrouvées quelque part : ils sont générés à chaque fois, avec leurs propres choix, leurs surprises, leurs réussites et leurs défauts. Car tout ne fonctionne pas toujours : le programme peut être incorrect, fragile, incomplet, ou faire ce qui est demandé de manière très maladroite (par exemple jouer très mal aux échecs).

Derrière le spectacle, une question demeure : les langages de programmation vont-ils disparaître, ou devenir au contraire encore plus importants pour préciser, contrôler et comprendre ce que l'IA produit ?

## Pour aller plus loin

Les expériences de l'exposé sont détaillées dans des billets de ce blog et dans deux études :

- [CallOfAcher et FIFAcher](https://blog.mathieuacher.com/CallOfAcherFIFAcherJeuxIA/) : vibe coder des variantes de jeux avec mon neveu de 10 ans (jouables sur [ashfall](https://blog.mathieuacher.com/ashfall/) et [fifacher](https://blog.mathieuacher.com/fifacher/))
- [BFChess](https://blog.mathieuacher.com/BFChessChessEngineBrainfuck/) : un moteur d'échecs en Brainfuck
- [MnM Lang](https://blog.mathieuacher.com/CodingAgentsMnMLang/) : des programmes en M&M's, et le [triple interpréteur ArnoldC → MnM → Brainfuck](https://github.com/acherm/agentic-arnoldc)
- [TeXCCChess](https://blog.mathieuacher.com/TeXCCChessEngine/) : jouer aux échecs dans Overleaf
- [PL Catalog](https://blog.mathieuacher.com/PL-ultimate-llm/) : 14 000+ langages de programmation, avec Software Heritage et CodeCommons
- Mathieu Acher, Jean-Marc Jézéquel, [Do Programming Languages Still Matter to Your AI Coding Agent Teammate? Evidence at Scale from Chess Engines](https://arxiv.org/abs/2606.13763), 2026 : 34 moteurs d'échecs, 17 langages, 2 agents
- Mathieu Acher, Zohra Kaouter Kebaili, Jean-Baptiste Espinasse, Djamel Eddine Khelladi, Coding Agents Develop COBOL Systems: Evidence from a Human-Guided Multi-Case Study, 2026 : 16 systèmes COBOL créés de zéro, dont [DOOM](https://github.com/acherm/agentic-cobol-study) https://inria.hal.science/hal-05743207/



Merci aux organisatrices et organisateurs de la Semaine de l'IA, et au public pour les questions !

*Un avis, une idée, un bug trouvé ? mathieu.acher@irisa.fr*

```bibtex
@misc{acher2026langagesIAgenerative,
  author = {Mathieu Acher},
  title = {Langages de programmation à l'ère de l'IA générative : COBOL, M\&M's, français… ou peu importe ?},
  year = {2026},
  month = {sep},
  howpublished = {Exposé, Semaine de l'IA, Université de Rennes, 4 septembre 2026. \url{https://blog.mathieuacher.com/LangagesProgrammationIAGenerativeSemaineIA2026/}},
  note = {Vidéo : \url{https://www.youtube.com/watch?v=Aj5arTQzMQ4}}
}
```
