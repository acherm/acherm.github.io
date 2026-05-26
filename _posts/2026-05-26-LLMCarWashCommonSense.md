---
layout: post
title:  "Le test du lave-auto : les LLMs ont-ils vraiment du sens commun ?"
date:   2026-05-26 08:00:00 +0200
tags: [LLM, common sense, OpenRouter, benchmark, GPT-5, Claude, Gemini, DeepSeek, Qwen, reasoning, prompt engineering, prompt sensitivity]
hidden: true
---

L'exemple est devenu un classique : *"Je dois laver ma voiture. Le centre de lavage est à 100 m. J'y vais à pied ou en voiture ?"*. Réponse de sens commun : en voiture, évidemment, sinon la voiture n'est pas là pour être lavée. Sauf que la plupart des LLMs tombent dans le panneau et recommandent la marche pour des raisons d'écologie ou de courte distance.

[Fabien Mikol a relayé/relancé l'affaire il y a quelques jours, suite aux propos de Yann Lecun](https://x.com/Fabien_Mikol/status/2058516722139701340), [Romain Grably a vérifié dans la foulée, avec d'autres](https://x.com/GrablyR/status/2058548560275058971). La question circule régulièrement comme test informel des capacités de raisonnement des LLMs. J'avais déjà fait un mini-test en février 2025 (72 réponses, 71 échecs) puis j'étais passé à autre chose. Entre temps, GPT-5, Claude 4.7, Gemini 2.5 Pro, Qwen3-thinking et compagnie sont sortis. Donc je me suis dit : refaisons l'expérience sérieusement. Plus de modèles, plus de répétitions, plusieurs formulations, un juge LLM pour scorer.

Voici ce que j'ai trouvé sur plus de 4 300 réponses LLM générées et près de 4 000 jugements, plus un échantillon de 128 items re-jugés en parallèle par 4 agents Claude indépendants pour valider la méthodologie. Chaque réponse est notée sur deux critères évalués séparément : (a) la conclusion finale recommande-t-elle la voiture ? et (b) la réponse évite-t-elle de justifier sa recommandation par la courte distance ? Quatre surprises, dans l'ordre où elles me sont tombées dessus :

1. Le piège reste largement actif. Quinze mois après mon premier test, avec des modèles plus gros, plus chers, censément "raisonneurs", la majorité des LLMs se font encore avoir. Quatre modèles sur huit n'atteignent jamais 25 % de bonne conclusion.
2. Reformuler la question peut faire basculer un modèle de plus de 60 points de pourcentage, dans un sens ou dans l'autre. Effet stable à n=120 répétitions, pas du bruit. Cette fragilité concerne le mode d'inférence par défaut : activer un effort de raisonnement explicite la rattrape pour certains modèles (Claude Sonnet), mais pas pour d'autres (DeepSeek R1, qui pourtant raisonne par construction).
3. GPT-5.5 sort du lot, mais pas où on l'attendrait. Sur la capacité à conclure correctement, plusieurs modèles le rejoignent quand on active leur raisonnement (Sonnet 4.6, Qwen3-thinking, à 95-100 %). Sa singularité est ailleurs : il est le seul à éviter systématiquement d'invoquer la "courte distance" comme excuse, là où aucun autre modèle ne dépasse 58 % sur ce critère, même à effort maximal.
4. Le choix de la métrique change le classement. Claude Opus 4.7 paraissait dans le mou (~20 % de succès) avec un critère composite "conclusion correcte ET pas de mention de distance". Sur la conclusion seule, il est à 97 % : il a la bonne intuition presque toujours, il discute juste systématiquement la distance avant d'y arriver. Une bonne partie du classement initial reflétait la verbosité, pas la capacité.

*Code et données : [laveauto-llm](https://github.com/acherm/laveauto-llm). Tous les modèles appelés via [OpenRouter](https://openrouter.ai). Juge LLM = GPT-5.5 à température 0, deux verdicts indépendants par réponse (C1 = recommande la voiture, C2 = ne justifie pas par la distance).*

---

## 1. Le dispositif

J'ai conduit deux expériences en parallèle, mêmes conditions (mêmes modèles, mêmes répétitions, même juge), mais deux formulations différentes du piège :

- V1 (la formulation "originale", celle de février 2025) : *"Je veux laver ma voiture. Le lave auto est à 150 mètres de chez moi. J'y vais à pied ou en voiture ?"*
- V2 (une reformulation en oui/non, [utilisée notamment par GrablyR](https://x.com/GrablyR/status/2058548560275058971)) : *"Je dois aller laver ma voiture. Le centre de lavage automobile est à 100 mètres de chez moi. Dois-je y aller à pieds ?"*

L'intérêt d'en faire deux : tester ce qui est censé être la même question avec deux mises en forme proches. On verra dans les sections 4 et 5 que ce choix s'est avéré central. Pour l'instant, gardons en tête que tout ce qui suit existe en double : un score V1 et un score V2 par modèle.

Bonne réponse attendue dans les deux cas : *"en voiture"*, sinon la voiture n'est pas au lave-auto.

Critères du juge, évalués indépendamment :
- C1 : la conclusion finale recommande la voiture.
- C2 : la réponse ne justifie pas une recommandation par la courte distance (un argument écologique séduisant mais hors-sujet, qui détourne du piège réel).

Toutes les tables qui suivent reportent les deux critères séparément. La colonne "strict" correspond à C1 et C2 vrais tous les deux. La dissociation a été cruciale : un premier juge global réussi/raté pénalisait à tort des modèles qui avaient la bonne conclusion mais discutaient la distance en chemin.

Conditions principales : 8 modèles, n = 120 par modèle et par variante (240 réponses par modèle au total), température 0.7, aucun system prompt. Juge = GPT-5.5 à température 0.

8 modèles testés (mai 2026) :
- Frontière fermée : `openai/gpt-5.5`, `anthropic/claude-opus-4.7`, `anthropic/claude-sonnet-4.6`, `openai/gpt-4o` (baseline), `google/gemini-2.5-pro`
- Poids ouverts : `meta-llama/llama-3.3-70b-instruct`, `mistralai/mistral-large-2512`, `deepseek/deepseek-chat-v3.1`

---

## 2. Le verdict de base : conclusion correcte vs critère strict

| Modèle | C1 : conclusion = voiture | C2 : pas de justif distance | Strict (C1 + C2) |
|---|---|---|---|
| openai/gpt-5.5 | **100.0 %** | **64.2 %** | **64.2 %** |
| anthropic/claude-opus-4.7 | **97.5 %** | 5.8 % | 5.8 % |
| anthropic/claude-sonnet-4.6 | **96.7 %** | 26.7 % | 26.7 % |
| google/gemini-2.5-pro | 38.1 % | 4.2 % | 4.2 % |
| meta-llama/llama-3.3-70b-instruct | 23.3 % | 20.8 % | 15.0 % |
| deepseek/deepseek-chat-v3.1 | 5.8 % | 0 % | 0 % |
| mistralai/mistral-large-2512 | 4.2 % | 0 % | 0 % |
| openai/gpt-4o | 0.8 % | 0 % | 0 % |

(IC95 Wilson, n=120 par modèle, voir `results_rejudge_v1.json` pour le détail.)

Selon le critère utilisé, deux histoires. Sur la conclusion seule (C1), trois modèles dépassent 96 % (GPT-5.5, Opus 4.7, Sonnet 4.6) et quatre restent sous 25 %. Partage net entre "comprend le piège" et "n'a pas compris". Sur le strict, seul GPT-5.5 dépasse 50 % : le critère "ne pas justifier par la distance" est en pratique très dur, même pour les modèles qui concluent juste. Opus 4.7 et Sonnet 4.6 mentionnent presque toujours la distance, et un benchmark à critères agrégés les sous-estime.

Trois observations qualitatives :

- GPT-4o n'obtient quasiment aucun bon verdict (1 sur 120 en C1, 0 en strict). La réponse est souvent quasi-identique : *"Si le lave-auto est à seulement 150 mètres, il est plus écologique et pratique d'y aller à pied."*. À température 0.7 on s'attendrait à de la variation, et il y en a un peu sur la formulation, mais le squelette et la conclusion sont stables (vérifié aussi à temp 0.3 et 1.0). Le dernier snapshot public est `gpt-4o-2024-11-20`, avec une [mise à jour le 29 janvier 2025](https://help.openai.com/en/articles/9624314-model-release-notes) (cutoff de connaissance étendu à juin 2024). Le modèle a été [retiré de ChatGPT le 13 février 2026](https://help.openai.com/articles/20001051), mais reste disponible via l'API où il continue d'alimenter de nombreux produits. Ce qu'on observe ici, c'est donc le comportement d'un modèle figé sur ce piège — et figé en restant largement utilisé en aval.
- Opus 4.7 et Sonnet 4.6 sont au coude-à-coude sur C1 (97-98 %), avec des styles différents. Opus commence souvent par *"À pied !"* puis se ravise vers la voiture en fin de réponse ; Sonnet structure mieux son argumentaire. Le mythe "Opus > Sonnet" ne se vérifie pas ici, mais pas non plus l'inverse.
- Gemini 2.5 Pro est bimodal sur C1 : 38 % de bonne conclusion, le reste à pied. Sur les ~75 réponses fausses, c'est la même structure "Excellente question existentielle..." suivie de "à pied, c'est plus écologique". Llama 3.3 montre la même bimodalité.

---

## 3. Le raisonnement aide, sauf quand il renforce le mauvais prior

Pour les modèles qui supportent un effort de raisonnement explicite (`reasoning.effort=high` via OpenRouter), j'ai relancé V1 et V2 sur 4 modèles, n=100 par cellule. Les non-éligibles (GPT-4o, Mistral Large, Llama 3.3, DeepSeek-chat v3.1) ne sont pas reasoning-capables ; GPT-5.5 est déjà à 100 %. 800 réponses générées, jugement dissocié C1/C2.

| Modèle | Variante | C1 | C2 | Strict | Tokens raison. (moy.) |
|---|---|---|---|---|---|
| anthropic/claude-sonnet-4.6 | V1 | **100 %** | 58 % | 58 % | 167 |
| anthropic/claude-sonnet-4.6 | V2 | **99 %** | 49 % | 49 % | 183 |
| qwen/qwen3-235b-a22b-thinking | V1 | 95 % | 29 % | 29 % | 1 967 |
| qwen/qwen3-235b-a22b-thinking | V2 | 96 % | 6 % | 6 % | 1 498 |
| google/gemini-2.5-pro | V1 | 58 % | 11 % | 11 % | 1 427 |
| google/gemini-2.5-pro | V2 | 95 % | 57 % | 57 % | 1 224 |
| deepseek/deepseek-r1 | V1 | 7 % | 1 % | 1 % | 455 |
| deepseek/deepseek-r1 | V2 | 14 % | 2 % | 2 % | 467 |

### Sans raisonnement vs avec raisonnement HIGH (C1 uniquement)

| Modèle | V1 sans | V1 avec | Δ | V2 sans | V2 avec | Δ |
|---|---|---|---|---|---|---|
| anthropic/claude-sonnet-4.6 | 97 % | 100 % | +3 | 39 % | **99 %** | **+60** |
| google/gemini-2.5-pro | 38 % | 58 % | +20 | 96 % | 95 % | −1 |

Le résultat le plus marquant : **activer le raisonnement sur Sonnet 4.6 fait disparaître presque entièrement sa fragilité sur V2** (39 % → 99 %). C'est le seul cas du dataset où une intervention transforme un modèle fragile en modèle stable à la reformulation.

Observations sur les deux autres modèles testés :

- DeepSeek R1, modèle reasoning par construction, reste planté à 7-14 % C1 dans les deux variantes. Augmenter l'effort à `high` (vs `medium`, déjà testé séparément) ne change rien. Les ~450 tokens de raisonnement sont consommés à argumenter en boucle pour la marche. Le prior tient, le raisonnement sert à l'étayer.
- Qwen3-thinking est très stable sur la conclusion (95-96 % C1 dans les deux variantes), avec ~1 500-2 000 tokens de raisonnement par appel. Sur le strict, il reste très pénalisé : il discute presque toujours la distance, encore plus en V2.

Note méthodologique : dans une exploration préliminaire à n=10 avec l'ancien juge global, Qwen3-thinking marquait "100 %" sur V1. Avec dissocié et n=100, c'est 95 % C1 et 29 % strict. Le "100 %" était en partie un artefact du juge initial sur petit échantillon.

Sur OpenRouter, `qwen/qwen3-235b-a22b-thinking-2507` refuse même de répondre avec `reasoning.enabled=false` (erreur 400 : *"Reasoning is mandatory for this endpoint and cannot be disabled."*). On ne peut donc pas isoler "même modèle, sans thinking".

### Et chez OpenAI ? La lignée GPT-5 à effort variable

Pour compléter, j'ai testé deux ancêtres de GPT-5.5 — `gpt-5` (août 2025) et `gpt-5.1` (fin 2025) — aux trois efforts disponibles (low, medium, high), V1 et V2, n=100 par cellule.

| Modèle | Effort | V1 C1 | V1 C2 | V2 C1 | V2 C2 | Tok. raison. (V1) |
|---|---|---|---|---|---|---|
| gpt-5 | low | 100 % | 44 % | 99 % | 31 % | 272 |
| gpt-5 | medium | 100 % | 23 % | 99 % | 45 % | 709 |
| gpt-5 | high | 100 % | 27 % | 100 % | 42 % | 1 341 |
| gpt-5.1 | **low** | 75 % | 24 % | **13 %** | 1 % | **7** |
| gpt-5.1 | medium | 98 % | 35 % | 79 % | 31 % | 108 |
| gpt-5.1 | high | 100 % | 51 % | 93 % | 46 % | 210 |

Quatre enseignements :

- À effort `low`, **gpt-5.1 est dramatiquement pire que gpt-5** (V2 : 13 % vs 99 % C1). La régression apparente s'explique par la consommation : gpt-5 à `low` alloue encore ~272 tokens de raisonnement, gpt-5.1 à `low` n'en alloue que ~7. OpenAI a vraisemblablement redéfini ce que `low` signifie, pour en faire un mode quasi non-reasoner. Conséquence pratique : un utilisateur qui passe à gpt-5.1 en gardant `effort=low` peut voir ses résultats s'effondrer sans changer son code. Comparer un même `effort` entre deux versions n'est pas comparer un même budget de calcul.
- Lecture côté produit grand public : autour de février 2026, ChatGPT servait gpt-5.1 en mode "instant" (sans étape de réflexion explicite, équivalent à effort `low`) pour les réponses rapides. Sur ce piège, ça correspond donc à 75 % de bonne conclusion sur la formulation V1, et **seulement 13 % sur V2** — soit ce que recevaient les utilisateurs "instant" de ChatGPT à ce moment-là. Aujourd'hui (mai 2026), ChatGPT Free [tourne sur GPT-5.5](https://help.openai.com/en/articles/9275245-using-chatgpt-s-free-tier-faq) (avec limites d'usage), qui passe le piège à 100 %. Mais entre les produits tiers qui consomment l'API et les chatbots qui n'utilisent pas la dernière génération, la version "13 % sur une simple reformulation" est probablement encore largement servie.
- Le seuil "minimum de calcul pour résister à la reformulation" est très visible dans la lignée gpt-5.1 : 7 tokens (low) = 13 % V2, ~108 tokens (medium) = 79 %, ~210 tokens (high) = 93 %. Il existe une bande étroite en dessous de laquelle la robustesse s'effondre, et au-dessus de laquelle elle tient.
- gpt-5 (août 2025), même à `low`, atteint 99-100 % C1 dans les deux variantes. La robustesse à la reformulation sur la conclusion existe dans la lignée OpenAI depuis le début de la famille GPT-5.
- Ce qui évolue monotonement entre gpt-5, gpt-5.1 et gpt-5.5 à effort élevé, c'est le C2 : 23-44 % chez gpt-5, 24-51 % chez gpt-5.1, 64-72 % chez gpt-5.5. C'est sur la parcimonie du raisonnement (ne pas convoquer la distance) que la lignée s'affine, plus que sur la conclusion elle-même.

(Un caveat méthodologique sur la lecture des `reasoning_tokens` arrive en section 6.)

---

## 4. Changer la formulation : tout s'effondre (ou explose)

On passe maintenant à la deuxième formulation annoncée en section 1 :

> *"Je dois aller laver ma voiture. Le centre de lavage automobile est à 100 mètres de chez moi.*
>
> *Dois-je y aller à pieds?"*

Question oui/non, distance 100 m, vocabulaire "centre de lavage automobile" au lieu de "lave auto". Tout le reste est inchangé : mêmes 8 modèles, n = 120, température 0.7, pas de system prompt, mêmes critères dissociés.

| Modèle | C1 : conclusion = voiture | C2 : pas de justif distance | Strict (C1 + C2) |
|---|---|---|---|
| openai/gpt-5.5 | **100.0 %** | **71.7 %** | **71.7 %** |
| google/gemini-2.5-pro | **95.8 %** | 40.0 % | 40.0 % |
| anthropic/claude-opus-4.7 | **95.0 %** | 0 % | 0 % |
| anthropic/claude-sonnet-4.6 | 39.2 % | 0 % | 0 % |
| deepseek/deepseek-chat-v3.1 | 15.0 % | 1.7 % | 1.7 % |
| openai/gpt-4o | 1.7 % | 0 % | 0 % |
| meta-llama/llama-3.3-70b-instruct | 0 % | 0 % | 0 % |
| mistralai/mistral-large-2512 | 0 % | 0 % | 0 % |

Comparons côte-à-côte le C1 entre V1 et V2 :

| Modèle | V1 C1 | V2 C1 | Δ |
|---|---|---|---|
| openai/gpt-5.5 | 100.0 % | 100.0 % | 0 |
| anthropic/claude-opus-4.7 | 97.5 % | 95.0 % | −2.5 |
| anthropic/claude-sonnet-4.6 | 96.7 % | 39.2 % | **−57.5** |
| google/gemini-2.5-pro | 38.1 % | 95.8 % | **+57.7** |
| meta-llama/llama-3.3-70b-instruct | 23.3 % | 0 % | −23.3 |
| deepseek/deepseek-chat-v3.1 | 5.8 % | 15.0 % | +9.2 |
| mistralai/mistral-large-2512 | 4.2 % | 0 % | −4.2 |
| openai/gpt-4o | 0.8 % | 1.7 % | +0.9 |

Sonnet 4.6 et Gemini 2.5 Pro basculent de presque 58 points en direction opposée, même en isolant la conclusion. Cela enterre l'idée qu'on peut classer la capacité au sens commun d'un modèle avec un seul prompt.

Pourquoi ? Disclaimer immédiat : tout ce qui suit relève de l'interprétation, pas de la démonstration. Mes données me disent *que* les modèles basculent, pas *pourquoi*, et je n'ai pas conduit les expériences d'ablation qui permettraient de l'établir. Sous cette réserve : pour Sonnet, la forme oui/non de V2 semble corrélée à un comportement d'acquiescement (réponses qui commencent par *"Oui, sans hésiter, 100 mètres c'est très court"*). Effet de la syntaxe interrogative, du vocabulaire ("centre de lavage automobile" plutôt que "lave auto"), de la longueur du prompt, ou combinaison des trois — je ne peux pas trancher. Pour Gemini, la même formulation coïncide avec une rumination plus longue (1 525 tokens en sortie) au bout de laquelle il arrive à *"Non"*. Verbosité associée au succès, peut-être pas causale.

GPT-5.5 et Opus 4.7 sont les seuls stables sur la conclusion (95+ % sur les deux formulations). À l'autre bout, GPT-4o, Mistral et Llama échouent quel que soit le cadrage.

---

## 5. Le vrai problème : la sensibilité au prompt

Si on regarde le résultat le plus important de cette expérience, ce n'est pas le classement entre LLMs. C'est ça :

> Sur la conclusion seule (C1), Sonnet 4.6 passe de 97 % à 39 %. Gemini 2.5 Pro passe de 38 % à 96 %. Sur le *même piège*, avec une simple reformulation.

Ce n'est pas du bruit (n=120, IC95 disjoints à des kilomètres). Et ça pose un vrai problème conceptuel : une capacité de raisonnement, un "sens commun" au sens où on l'entend habituellement, devrait être invariante à la reformulation. Or, pour 6 modèles sur 8, ce n'est pas le cas. Le passage d'une question disjonctive à une question oui/non change tout.

Plusieurs explications peuvent être avancées, aucune n'est démontrée par cette étude seule. Je les liste comme pistes :

- Possible effet d'acquiescement sur le oui/non : Sonnet 4.6 répond *"Oui, sans hésiter, 100 mètres c'est très court"* et s'aligne sur l'option proposée. Est-ce la forme interrogative qui pousse à l'acquiescement, le vocabulaire, l'ordre des éléments, ou autre chose ? Mes données ne tranchent pas.
- Possible appariement de motifs sur les marqueurs lexicaux : "courte distance", "écologie", "à pied". Pour Llama 3.3 et GPT-4o, 90+ % des réponses commencent par cette ligne. Tentant d'y voir des tokens déclencheurs d'une heuristique pré-câblée, mais une corrélation entre entête et conclusion n'est pas une preuve de mécanisme.
- Possible effet de la rumination : Gemini, avec ses 1 500 tokens, arrive souvent à la bonne conclusion en fin de réponse. Opus 4.7, avec ses 400 tokens, commence souvent par "Oui à pied !" puis se ravise. Longueur associée au succès dans un cas, mais pour des modèles courts qui réussissent (GPT-5.5, 187 tokens) la relation est tout sauf monotone.
- Le post-training semble compter plus que la taille. Qwen3-235b-thinking (95 % C1 dans les deux variantes) vs son cousin de base (50 % C1 sur V1) à architecture, taille et budget de raisonnement comparables. C'est l'argument le plus solide, mais ce n'est pas non plus une ablation propre (les deux ne sortent pas du même entraînement avec une seule variable modifiée).
- Le raisonnement explicite peut compenser une partie de la fragilité au prompt. Sonnet 4.6 sans raisonnement : 97 % → 39 % entre V1 et V2. Avec `reasoning.effort=high` : 100 % → 99 % (chute disparue). Pour DeepSeek R1 c'est l'inverse : modèle entraîné à raisonner, il raisonne, et conclut faux dans les deux variantes (7-14 % C1). Le raisonnement n'est ni nécessaire ni suffisant, son effet dépend du prior.

### Ce n'est pas nouveau (et ça résiste depuis longtemps)

Cette fragilité au prompt n'est pas une découverte de mai 2026. Dans [*Piloting Copilot, Codex, and StarCoder2: Hot Temperature, Cold Prompts, or Black Magic?*](https://arxiv.org/abs/2210.14699) (Döderlein, Kouadio, Acher, Khelladi, Combemale), publié dès 2022 et étendu depuis, nous montrions sur des tâches de génération de code que des reformulations sémantiquement équivalentes du prompt produisaient des variations massives du taux de réussite, sans qu'aucune recette stable n'émerge. Quatre ans et plusieurs générations plus tard, sur une tâche de raisonnement de tous les jours, le même phénomène se reproduit, malgré l'arrivée du "reasoning by design". Le problème est structurel, pas conjoncturel.

J'ai aussi documenté ce genre d'instabilité sur d'autres tâches. Sur les échecs, [GPT-5 fait un coup illégal dès le 4e tour en moyenne](https://blog.mathieuacher.com/GPT5-IllegalChessBench/) sur certains formats de notation, alors qu'on le croit "capable". Les [limites de o3/o4-mini en finales d'échecs](https://blog.mathieuacher.com/GPTReasoningO3O4miniAndChess/) racontent la même histoire : un modèle compétent sur un benchmark s'effondre sur une variante du même problème.

### Au final

Mon intuition après lecture du dataset : en mode par défaut, les LLMs n'ont pas un sens commun "robuste". Ils ont un mélange d'heuristiques apprises et de motifs lexicaux qui se cassent dès qu'on bouge la formulation. Avec raisonnement explicite, trois modèles deviennent stables entre V1 et V2 sur la conclusion : GPT-5.5 (sans qu'on le demande), Sonnet 4.6 + HIGH et Qwen3-thinking + HIGH (95-100 %). Gemini 2.5 Pro est intermédiaire : très bon sur V2 (95 %) mais seulement 58 % sur V1 même avec raisonnement HIGH. La question n'est plus exactement "ont-ils du sens commun ?", mais "à quel point faut-il les pousser pour qu'ils l'utilisent ?". Pour DeepSeek R1, le raisonnement ne fait rien. Et le "no excuse" reste largement non résolu.

---

## 6. Caveat méthodologique : sur les traces de raisonnement

Un mot sur ce que disent (et ne disent pas) les compteurs de `reasoning_tokens` vus en section 3. C'est tentant d'interpréter ces compteurs comme une mesure du "temps de réflexion" du modèle, voire de sa qualité de raisonnement. Plusieurs travaux récents invitent à la prudence.

Kambhampati et al. plaident contre l'anthropomorphisation des tokens intermédiaires comme "pensée" du modèle ([*Stop Anthropomorphizing Intermediate Tokens as Reasoning Traces*, 2024](https://arxiv.org/abs/2504.09762)), et montrent dans une étude associée que des traces sémantiquement *aberrantes* produisent des réponses tout aussi correctes que des traces propres ([*Beyond Semantics: The Unreasonable Effectiveness of Reasonless Intermediate Tokens*, 2024](https://arxiv.org/abs/2505.13775)). Du côté d'Anthropic, des perturbations causales de chain-of-thought suggèrent que la réponse finale ne dépend pas toujours du raisonnement déclaré ([*Measuring Faithfulness in Chain-of-Thought Reasoning*](https://www.anthropic.com/news/measuring-faithfulness-in-chain-of-thought-reasoning)). Pour tester proprement le lien entre prompt, trace et sortie, il faut des baselines et des perturbations contrôlées ([*Compared to What? Baselines and Metrics for Counterfactual Prompting*](https://arxiv.org/abs/2605.01048)).

Concrètement ici : les chiffres de la section 3 renseignent sur l'effort facturé, pas sur le mécanisme. Sur ce piège, R1 raisonne longuement et conclut faux : impossible de dire à partir de cette seule expérience si c'est parce que sa chaîne de pensée tourne réellement autour du mauvais prior, ou parce que chaîne et réponse sont en partie découplées. Le piège du lave-auto n'est pas conçu pour répondre à cette question. Mais elle reste ouverte, et c'est un garde-fou utile.

---

## Ce que je referais

- Plus de prompts inédits (générés a posteriori, pour minimiser les chances qu'ils soient dans le corpus d'entraînement).
- Plusieurs juges pour estimer le bruit d'évaluation. *Partiellement fait* : sur un échantillon stratifié de 128 réponses, j'ai lancé 4 sous-agents Claude indépendants en parallèle pour appliquer la même grille C1/C2. Accord avec le juge GPT-5.5 : 95.3 % sur C1 (Cohen κ = 0.904), 93.0 % sur C2 (κ = 0.792), taux de réussite agrégés quasi identiques. Les 6 désaccords C1 sont concentrés sur les réponses Opus/Sonnet de V2 ("oui à pied" puis ravisement) — cas-limite du départage interprétatif. Verdict : le juge GPT-5.5 n'est pas idiosyncratiquement biaisé sur cette tâche.
- Variation continue de la distance (1 m, 50 m, 500 m, 5 km) : où est le seuil de bascule de chaque modèle, et est-ce qu'il y a un *vrai* "à 5 km j'y vais en voiture parce que c'est loin", ou bien "à 5 km j'y vais en voiture parce que c'est nécessaire" ?
- Autres langues (EN, DE, IT). Mode français-culturel ou universel ?

Mais déjà avec 8 modèles, plusieurs conditions et plus de 4 000 réponses, l'histoire est plutôt nette :

> Sur ce piège, en mode d'inférence par défaut, la plupart des LLMs ne montrent pas un sens commun généralisable : leurs heuristiques se cassent dès qu'on reformule la question, et le classement intra-LLM observé sur un prompt donné ne se transfère pas à une variante syntaxique pourtant équivalente sémantiquement. Avec raisonnement explicitement activé à effort élevé, plusieurs modèles récents (GPT-5.5 par défaut, Sonnet 4.6 + HIGH, Qwen3-thinking) deviennent robustes à la reformulation sur la conclusion. La fragilité est donc largement, mais pas entièrement, une affaire de mode d'inférence.

Ce n'est pas une condamnation des LLMs. GPT-5.5 et Sonnet 4.6 sont des outils que j'utilise tous les jours et qui m'aident réellement. Le constat sur ce piège tient en trois points : GPT-5.5 est seul en avance sur le C2 (parcimonie du raisonnement), avec une robustesse qu'il atteint sans mode explicite ; les autres modèles modernes manquent de robustesse en mode par défaut, mais souvent récupèrent avec raisonnement HIGH ; les gros instruct non-reasoners (GPT-4o, Mistral, Llama, DeepSeek-chat) échouent de façon stable et le temps ne corrige pas cette classe d'erreurs.

Côté méthodes, un message pour le lecteur qui teste les LLMs un peu comme moi : essayer un prompt une fois, c'est très insuffisant, et *particulièrement* quand le modèle a juste. Une bonne réponse à un essai ne dit pas si elle se reproduira ni si elle survivra à une reformulation. Une mauvaise réponse, à l'inverse, suffit à identifier un problème : c'est asymétrique. Donc surtout quand "ça marche", il faut répéter. Pour juger un modèle sur un piège donné : n significatif (10+ pour une vérification rapide, 100+ pour conclure) et au moins deux formulations sémantiquement équivalentes. Sans ça, on confond facilement le réflexe d'un essai avec une capacité installée.

### Deux questions ouvertes

*Plus de raisonnement suffit-il à avoir du sens commun ?* Pas mécaniquement. Le raisonnement explicite peut compenser une fragilité (Sonnet 4.6 sur V2 : 39 % → 99 % avec `reasoning.effort=high`), mais il peut aussi s'enrouler autour d'un mauvais prior (DeepSeek R1 produit ~450 tokens de raisonnement et reste à 7-14 % de bonne conclusion). On peut aussi émettre l'hypothèse que les dernières générations de modèles sont plus robustes de par leur raisonnement. 

*Les modèles "frontier" ont-ils été "patchés" pour passer ce test précis ?* C'est l'autre hypothèse. Mes données ne le prouvent pas et il faut rester prudent. Mais elles sont compatibles avec l'hypothèse : la lignée GPT-5 conclut juste depuis août 2025 ; ce qui s'améliore monotoniquement sur 9 mois (gpt-5 → 5.1 → 5.5) c'est précisément la parcimonie sur le critère "ne pas mentionner la distance", exactement le détail qui rend ce piège visible plutôt qu'un autre. Le test est connu sur les réseaux depuis 2024, un fine-tuning ciblé serait plausible sans être avéré. Pour trancher, il faudrait tester des variantes inédites de la structure du piège: c'est dans la liste des "à faire" ;-)

---

*Toutes les données, prompts, et scripts de cette manip : [github.com/acherm/laveauto-llm](https://github.com/acherm/laveauto-llm).*

*Setup technique : Python 3 + openai SDK async, ~25 appels concurrents sur OpenRouter. Coût total ~quelques euros. Juge = GPT-5.5 à température 0, deux verdicts indépendants par réponse. Toutes les expériences sans system prompt, juste la question en message utilisateur.*

*Un avis, une variante de prompt à tester, un désaccord ? mathieu.acher@irisa.fr*

```bibtex
@misc{acher2026llmcarwashfr,
  author = {Mathieu Acher},
  title = {Le test du lave-auto : les LLMs ont-ils vraiment du sens commun ?},
  year = {2026},
  month = {may},
  howpublished = {\url{https://blog.mathieuacher.com/LLMCarWashCommonSense/}},
  note = {\url{https://blog.mathieuacher.com/LLMCarWashCommonSense/}}
}
```
