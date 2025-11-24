---
layout: post
title:  "La Parole aux Machines (Philosophie des grands modèles de langage) par Monsieur Phi"
date:   2025-11-23 011:54:29 +0200
tags: [ChatGPT, LLM, generative AI, openai, chess, french]
---


"La Parole aux Machines" est un livre d'utilité publique. 
L'ouvrage permet de comprendre l'intelligence artificielle générative et les grands modèles de langage (aka LLM) en amenant le lecteur vers des questionnements philosophiques (la compréhension, le fonctionnalisme, la conscience, l'alignement, l'autonomie, etc.) extrêmement importants pour la société actuelle et future. Monsieur Phi (aka Thibaut Giraud) a un talent rare : qui peut se targuer de maîtriser différents aspects techniques et philosophiques de l'IA générative et des LLMs, tout en étant extrêmement pédagogue ? T. Giraud réussit, à mon sens, la prouesse d'intéresser à la fois des personnes totalement ignorantes de l'IA générative, mais également des ingénieurs ou chercheurs du domaine. Nous avons urgemment besoin de philosophie (et d'expertise et de sciences informatiques) pour adresser les nombreuses questions que posent l'usage des IAs génératives, et ce livre est au bon niveau de détails et d'abstraction, loin de la médiocrité des ignorants et fainéants qui pullulent dans l'espace médiatique. J'en dis un peu plus dans ce court article (pour une fois en français !), et j'espère que ce ce livre sera lu et discuté intensément par le plus grand nombre. 


## Nous avons perdu le monopole du langage

Le livre commence avec une première phrase qui résonne encore: "Nous avons perdu le monopole du langage".
Nous, êtres humains, pouvons maintenant communiquer avec des êtres artificiels, dans notre langage (par exemple, en anglais ou en français, avec du texte ou des images). 
C'est certainement une capacité des machines qui paraîtra évidente et non surprenante dans quelques années et pour les futures générations, mais il faudra s'en souvenir : nous vivons un moment historique et transformatif, à de multiples niveaux, surtout quand on pense à la puissance du langage et à la difficulté d'atteindre la maîtrise actuelle des LLMs. 
La suite du livre détaille (1) comment, techniquement parlant, les chercheurs et ingénieurs sont arrivés à une telle prouesse et (2) des différents impacts philosophiques et sociétaux qu'introduisent une telle capacité. Car oui, "maîtriser" le langage ouvre de nombreuses perspectives, dans des tas de domaines et tâches, pour le meilleur, mais potentiellement pour le pire. 

T. Giraud déroule les chapitres avec une perspective historique, de GPT-1 à DeepSeek-R1 en passant par GPT-2, Claude, o3, et évidemment ChatGPT. C'est très bien vu de sa part, car l'histoire de l'IA générative, certes très récente et courte, permet de comprendre les progrès qui ont été réalisés à chaque grande étape, et des questions philosophiques qui se sont posées. 
Tout va très très vite (ChatGPT est sorti en novembre 2022), mais cela aurait pu être pire : je n'ose imaginer si un GPT-5 était sorti dès novembre 2022, sans apprécier et comprendre (partiellement) les progrès intermédiaires. Heureusement, on a eu un peu de temps pour comprendre certaines choses fondamentales, et T. Giraud en profite pour les disséquer. 

## Apprécier la perspective historique 

J'ai particulièrement apprécié le chapitre sur GPT-2, car c'est via ce modèle que j'ai eu la sensation que quelque chose se passait... En expérimentant avec GPT-2 et le jeu d'échecs (une importante thématique du livre de T. Giraud !), j'écrivais tout début 2020:

> Oui, GPT-2 n'a pas appris la signification complexe des échecs, mais il a appris quelque chose à partir de textes uniquement, ce qui s'apparente en quelque sorte au bluff (proche de l'effet ELIZA). L'approche brutale de GPT-2 peut être appliquée à de nombreux domaines (musique, poésie, recettes, etc.), puisqu'il existe des données textuelles. Je suppose que l'efficacité qui en résulte (effet ELIZA ?) peut varier considérablement et vaut la peine d'être essayée, mais pour le domaine des échecs, certaines « pièces » du processus d'apprentissage manquent ;)


GPT-2 a été un premier signal, et T. Giraud explique très bien son fonctionnement pour prédire les prochains "tokens". Aucun élément mathématique ou formel dans les explications, et pourtant, les explications sont à mon sens convaincantes et suffisantes pour comprendre comment marchent techniquement les LLMs, montrant au passage les (grandes) limites de GPT-2. Le livre explique aussi très bien qu'il ne faut pas sous-estimer cette tâche de prédiction de textes, qui paraît "simple" a priori, mais qui en fait ne l'est pas, et surtout qui ouvre de nombreuses perspectives sur d'autres tâches plus complexes, comme le résumé de texte, la traduction, des réponses à des questions diverses et variées, de la génération de programmes informatiques, etc.

Le chapitre suivant suit l'évolution historique, et les progrès avec GPT-3 et ChatGPT, qui atteignent un niveau de maîtrise du langage remarquable. Un véritable tremblement de terre par rapport à GPT-2. 
L'importance et la sensibilité au "prompt" sont par ailleurs très bien expliquées. 

## ChatGPT est nul... vraiment ?

J'ai particulèrement aimé l'idée qu’on ne peut pas conclure à une incapacité générale de ChatGPT pour une tâche juste parce qu’on trouve des exemples où il se trompe lourdement. 
Cette thématique revient très souvent dans le livre. Elle n'est pas si simple à comprendre et à appréhender.


Ceci dit, certaines personnes, parfois très présentes dans l'espace médiatique, se sont moqués de ChatGPT (pourquoi pas !) mais ont malheureusement immédiatement conclu et généralisé que ces LLMs étaient incapables de disserter sur de la philosophie, de résoudre certains problèmes logiques, etc.
Entendons-nous bien, il y a des limites fondamentales aux LLMs (j'y reviendrai !), mais trop de personnes, y compris chez certains universitaires, ont sous-estimé les capacités des LLMs, avec des impacts importants sur la qualité du débat. 
Ce passage dit les termes: 


> Ce ne sont pas les pires, bien au contraire je les ai pris comme exemples parce que ce sont des universitaires, donc a priori des personnes compétentes quand elles font le choix de s’exprimer sur un sujet dans l’espace médiatique. La méconnaissance dont ils font preuve n’en est que plus frappante. Mais il faut bien garder en tête que de tels cas sont symptomatiques d’une tendance courante dans le traitement médiatique du sujet : des milliers d’articles de presse et d’interventions médiatiques du même genre, dans le même ton, diffusant les mêmes erreurs, ont profondément déformé la perception des LLM dans l’espace public.


Deux erreurs sont généralement faites. 
La première, c'est d'utiliser des LLMs dépréciés, pas du tout à jour par rapport à l'état de l'art... Par exemple utiliser des LLMs de 2023 alors qu'on est en 2025... Ou de recycler des exemples de 2023 en 2025 sans vérifier et reproduire... dans un domaine où tout va très très vite. 

La deuxième, plus subtile et intéressante, c'est de spécifier un mauvais "prompt" et d'expérimenter de manière non rigoureuse, jusqu'à conclure de manière générale. 
C'est notamment démontré dans un chapitre où il est beaucoup question des dissertations en philosophie (typiquement au baccalauréat). T. Giraud, docteur et ayant largement enseigné la philosophie, montre l'effet du "prompt" sur la qualité des résultats, allant à l'encontre de certaines conclusions sur l'incapacité des LLMs à produire des textes intéressants. 
Il ne reste plus, alors, pour les détracteurs-fainéants que l'argument subjectif comme quoi il manque quelque chose (un "je-ne-sais-quoi") aux LLMs pour vraiment penser et raisonner, comme nous les humains. Le fameux "je-ne-sais-quoi" qui est utilisé dans ces vidéos, et qui résonne à de nombreuses reprises dans le livre.
T. Giraud introduit alors des concepts philosophiques importants (fonctionnalisme, computationalisme, etc.) avec des expériences de pensée (par exemple, la très critiquée expérience de la chambre chinoise) pour discuter des critiques faites aux LLMs. 

## LLM et jeu d'échecs

Une autre démonstration sur l'importance du "prompt" concerne les LLMs et les échecs. 
C'est évidemment mon chapitre préféré, et pas seulement parce que j'y suis souvent cité ;-) ayant travaillé sur ce sujet et collaboré avec Monsieur Phi pour son excellente vidéo: https://www.youtube.com/watch?v=6D1XIbkm4JE 

L'histoire est que les premières expérimentations ont montré que les LLMs étaient incapables de jouer correctement aux échecs, au point de proposer des coups illégaux. 
Donc la conclusion possible eut été, comme en dissertation de philosophie, les LLMs sont nuls (aux échecs). Point barre, nul nul nul, fin de l'histoire. Et pourquoi pas, jouer aux échecs en complétant du texte, c'est peut-être comme jouer à l'aveugle dans un espace des possibles monstrueux...
Or, il s'avère qu'avec un "prompt" plus adapté, utilisant le langage des échecs (PGN), le niveau de certains LLMs, notamment le gpt-3.5-turbo-instruct, était plus qu'intéressant (1750 Elo environ). 
Plus de détails ici: https://blog.mathieuacher.com/GPTsChessEloRatingLegalMoves/ (en anglais)
De l'importance de la démarche scientifique et des expériences en Informatique, surtout quand on s'intéresse à des objets complexes comme les LLMs. J'en dis un peu plus dans une vidéo captation pour la fête de la science à l'INSA Rennes: https://www.youtube.com/watch?v=TtGiT-tWdmE 

Alors un LLM avec un bon prompt ne permet pas de rivaliser avec les forts joueurs d'échecs, encore moins avec les IAs spécialisés comme Stockfish, mais au moins à clouer un peu le bec aux personnes qui accusent les LLMs d'être des perroquets stochastiques. 
Surtout qu'il y a de plus en plus de preuves que les LLMs construisent des représentations internes qui vont au-delà 

## Perroquet?

C'est d'ailleurs une autre thématique très bien couverte dans le livre: se cantonner à "LLM = perroquet qui récite quelque chose au hasard sans comprendre", c'est se priver d'un débat. 



> Parler encore de perroquets stochastiques en 2023 ressemble à du déni pur et simple ou à une méconnaissance de l’état de l’art. Pour emprunter les mots de Neel Nanda, directeur d’une équipe de recherche à DeepMind, interviewé en 2024 :
> « Je ne comprends vraiment pas les personnes qui maintiennent encore la perspective des perroquets stochastiques de nos jours, pour être honnête. Je pense que ça a été largement falsifié » (Scarfe et Nanda, 2024).
> Notez bien que rejeter la thèse des perroquets stochastiques n’implique pas que les LLM accèdent à une forme de compréhension. L’implication ne va que dans l’autre sens : si les LLM sont des perroquets 
> stochastiques, alors ils ne comprennent rien ; accepter cette thèse, c’est fermer d’avance toute 
> discussion sur le sujet. En la rejetant, au contraire, on ne fait qu’ouvrir la réflexion sur la 
> possibilité d’une forme de compréhension dans les LLM, sans trancher a priori en faveur d’une conclusion.


## Ne pas clore le débat...

Justement, T. Giraud ne veut pas clore le débat, mais bien au contraire l'étendre à des questions encore plus précises et passionnantes, qui suivent par ailleurs les avancées récentes en IA générative (par exemple les modèles de raisonnement): Est-ce que les LLMs pensent? Est-ce que les LLMs ont une conscience? Est-ce qu'on peut contrôler des LLMs? Doit-on avoir peur des LLMs? 
Toutes ces questions sont abordées dans les chapitres suivants, avec toujours un angle technique et philosophique. Et c'est toujours aussi pédagogique et passionant ! 

Un questionnement particulièrement intéressant concerne l'alignement des LLMs. 
Comme bien expliqué, la difficulté est qu'on ne programme plus explicitement un comportement souhaité (avec des lignes de code), mais il émerge à partir d'un apprentissage statistique sur des données très larges et variées (potentiellement tout le Web) et avec également des mécanismes pour prendre en compte les préférences (e.g., RLHF). 
Les instructions (le "prompt") ne sont pas toujours suivis, et de nombreuses expériences montrent qu'un système à base de LLMs peut ne pas suivre les instructions ou passer par des sous-objectifs intermédiaires très dérangeants. De nombreux exemples sont données. Un exemple supplémentaire qui n'est pas dans le livre est ce système qui triche aux échecs, car le système comprend qu'il n'a aucune chance si une partie normale est jouée. Plus de détails ici: https://blog.mathieuacher.com/AICheatingInChessSomeNotes/ 

Le problème de l'alignement est une instance d'un problème plus général, celui de la spécification et de la vérification/validation en informatique. En réalité, ce problème existe déjà quand on programme de manière classique: comment s'assurer que le logiciel développé, qui peut contenir des millions de ligne de code, et fonctionnant sur des processeurs pas forcément déterministes, va produire le bon résultat, et dans un temps raisonnable? Si on prend un programme d'échecs comme Stockfish, dur de déterminer a priori le coup qui va être proposé, à part en l'exécutant pour de bon. 
C'est encore pire avec les LLMs: on découvre la (bonne ou mauvaise) surprise de la réponse au moment de son utilisation et du "prompt" utilisé 
Disons qu'avec les LLMs le problème de la vérification est difficile, et encore plus critique peut-être, car plusieurs couches d'incertitude s'ajoutent. Comment spécifier clairement ce que l'on veut, comment tester et vérifier ce qu'on a effectivement construit, et comment rester lucide sur les limites de nos systèmes : un vaste programme qui va occuper de nombreux chercheurs (dont je fais partie !). 

## Aller plus loin

Quelques éléments techniques auraient pu être développés dans le livre. Par exemple, à propos de l'échantillonnage sur les probabilités de distribution (et notamment l'hyperparamètre de température) qui a à la fois un impact sur la créativité des LLMs (bien !) mais aussi qui a tendance à rendre encore plus incertaine les réponses (pas bien !). 
Ou sur le fait que les systèmes d'IA ne reposent plus sur un LLM seul, mais sur une combinaison d'outils plus "traditionnels", comprenant du code et des programmes, et d'autres techniques d'IA (e.g., apprentissage automatique supervisé). 
La raison est que les LLMs seuls ont quand même de sacrés limites, notamment pour raisonner et planifier... Les approches autorégressives comme les LLMs, qui prédisent mot après mot sans véritable modèle explicite du monde ni de leurs propres objectifs, se heurtent à des limites fondamentales dès qu'il s’agit de garantir une cohérence globale, une planification fiable ou le respect strict de contraintes complexes.
Pas étonnant donc d'avoir des systèmes hybrides, agentiques avec un ou plusieurs LLMs.

Ces éléments techniques ne sont pas forcément cruciaux pour la compréhension du propos, et pour sûr, cela ne simplifie pas notre problème de complexité des IAs génératives. 

## Conclusion (achetez, lisez, offrez le livre)

Le livre se conclue remarquablement par un résumé, quasiment une mise en garde:


> En somme, il faut en philosophes commencer par reconnaître notre ignorance : les systèmes d’IA que nous > sommes en train de construire vont avoir un impact significatif sur nos existences futures, et nous ne > savons pas dans quelle mesure nous pourrons les contrôler. C’est une situation inédite et perturbante.


Je le traduirais ainsi: Nous avons urgemment besoin de philosophie (et d'expertise) pour adresser les nombreuses questions que posent l'usage des IAs génératives, et ce livre est au bon niveau de détail et d'abstraction, loin de la médiocrité des ignorants et fainéants qui pullulent dans l'espace médiatique.

C'est un livre par ailleurs très bien écrit. Ce n'est absolument pas un "simple" transcript des nombreuses vidéos de Monsieur Phi sur le sujet. (Même si cela avait été le cas, la qualité eut été au rendez-vous.) Le soin apporté aux liens entre les chapitres, la perspective historique, et les nombreuses explications précises, sourcées, et pédagogiques me font dire que c'est un livre utile à la fois à un public connaissant l'IA et en général à tout public. Pour continuer ensemble à réfléchir à ces machines un peu spéciales...








