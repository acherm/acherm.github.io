---
layout: post
title:  "Corners, coups francs, fantasmes et surprises dans les grands championnats européens de football : ce que les données disent vraiment"
date:   2026-03-18 11:00:00 +0100
tags: [football, data analysis, corners, set pieces, Ligue 1, Premier League, Marseille, Arsenal, xG, Understat]
hidden: true
---

On parle souvent des corners et des coups de pied arrêtés (CPA) au café du commerce, sur les réseaux sociaux, ou dans les médias. "Arsenal est injouable sur corners", "L'OM est nul sur coups francs", etc. Des impressions, des ressentis. Et parfois des surprises : saviez-vous que Tottenham, à la lutte pour le maintien en Premier League, est sur le podium européen des buts sur corners cette saison, juste derrière Arsenal et l'Inter ? Non ? Oui ? 
Et si on allait vérifier ?
J'ai analysé **5 saisons de données** sur les 5 grands championnats européens (Premier League, Ligue 1, Serie A, Bundesliga, La Liga). Chaque tir, chaque but, chaque expected goal (xG) sur corner, coup franc indirect, coup franc direct. 484 équipes-saisons. Voici ce que j'ai trouvé en 10 points clés, et certains résultats me semblent surprenants, méconnus, ou contre-intuitifs.

*Données : Understat via understatapi. 5 ligues européennes, 5 saisons (2021-2026), 484 équipes-saisons. Note : la saison 2025-26 est en cours (~26-30 journées selon les championnats). Les tendances sont claires mais les chiffres évolueront. Je mettrai à jour en fin de saison.*

## 1. Tottenham, roi des corners... et 16e de Premier League

C'est l'histoire la plus folle de cette saison (en cours, ~30 journées de PL jouées). Tottenham est **3e européen** sur les buts marqués sur corners : 14 buts, derrière Arsenal (16) et l'Inter (15). Conversion de 21.5%, la meilleure d'Europe.

Le problème ? Les Spurs sont actuellement **16es de Premier League** avec 30 points.

<img src="/assets/fr_tottenham_open_vs_corners.png" alt="Tottenham vs Arsenal, Jeu ouvert vs Corners">

Le graphique est sans appel. Tottenham est isolé en haut à gauche : beaucoup de buts sur corners, très peu en jeu ouvert (25 buts, 14e du championnat). Arsenal, lui, est en haut à droite : 16 buts sur corners ET 39 en jeu ouvert.

**35.0% des buts de Tottenham viennent de phases arrêtées.** C'est le taux le plus élevé parmi les clubs des grands championnats (hors relégables). Quand plus d'un tiers de tes buts dépendent de situations de jeu qui représentent ~15% des tirs, tu construis sur du sable.

À titre de comparaison, le Bayern Munich, meilleure attaque d'Europe, n'est qu'à 15.6% de dépendance aux CPA. Parce que quand tu marques 65 buts en jeu ouvert, les corners sont un bonus, pas une béquille.

Et puis il y a Arsenal, l'ovni : **16 buts sur corners (1er en Europe)**, mais également 39 en jeu ouvert, 70 points (1er de Premier League). Arsenal ne se contente pas d'être redoutable offensivement sur CPA, ils le sont aussi dans le jeu. Lors du match de Ligue des Champions contre le Bayer Leverkusen, [le compte officiel du Bayer 04](https://x.com/bayer04_en/status/2031723079886401891) a veinement interdit à Arsenal d'obtenir des corners. Des voies s'élèvent même en Premier League. 
Dans une interview explosive, John Obi Mikel a carrément accusé Arsenal de tricher ou que le succès d'Arsenal dépend uniquement des corners: https://talksport.com/football/4043546/john-obi-mikel-interview-arsenal-chelsea-mourinho-nigeria/. 
Tottenham est un parfait contre-exemple : il n'y a pas que les corners... 
Et on peut aussi penser que Tottenham est en grand danger, car sans les CPA la situation serait encore plus compliquée que la 16ème place. 

---

## 2. Elche 2022-23 : 107 tirs sur corners, l'anatomie du désespoir

Outre Tottenham en 2025-26, il y a d'autres cas extrêmes : Elche, saison 2022-23, La Liga. L'équipe a été reléguée avec 25 points, et pourtant, elle a tiré **107 fois sur corners**, le record de La Liga cette saison-là.

<img src="/assets/fr_elche_corners_2022_23.png" alt="Distribution des tirs sur corners, La Liga 2022-23">

Le z-score est de 2.26, un outlier statistique. Mais le graphique de droite est encore plus parlant : **26.4% de tous les tirs d'Elche venaient de corners**. La moyenne de La Liga est à 15.6%. Personne d'autre ne dépasse 19%.

Pourquoi ? Parce qu'Elche ne savait rien faire d'autre.
Seulement 406 tirs au total (le plus bas de La Liga), une création en jeu ouvert inexistante. Les corners étaient leur seule route vers le but. Lucas Boyé, leur attaquant de pointe, a tiré 19 fois sur corner à lui seul, un homme-cible qui faisait tout le boulot aérien.

Quand tu ne sais rien faire d'autre, il te reste les corners. Tottenham 2025-26 est une version moins extrême du même syndrome.

Rassurez-vous, je n'ai pas regardé tous les matchs de Elche. En fait, je pense que j'ai dû en regarder à peu près zéro. Mais les statistiques sont assez claires (j'ai regardé également si c'était une anomalie des données ou par exemple un match mal renseigné peut explique ce haut score en corners, mais non, a priori). Ceci dit, **je cherche des suiveurs ou mieux supporters de Elche, car c'est un phénomène assez incroyable**, et malgré quelques recherches ici et là, je n'ai rien trouvé qui relate cet exploit de Elche. 

---

## 3. L'OM, de la honte à la renaissance (partielle) sur corners

En novembre 2025, [@Rubenzf911](https://x.com/Rubenzf911/status/1985468834401116253) posait la question que beaucoup de supporters marseillais avaient en tête :

> *"Les CPA de l'OM est-ce qu'un jour on va tirer la sonnette d'alarme svp?"*

La sonnette, les données l'ont tirée depuis un moment. Voici l'évolution de Marseille sur 5 saisons :

<img src="/assets/fr_table_marseille_evolution.png" alt="Évolution Marseille, 5 saisons">

Le tableau raconte une histoire en trois actes :

**Acte 1, La gloire (2022-23)** : 17 buts sur CPA (1er de Ligue 1 !), 11 sur corners seuls, 14.9 xG. L'OM surperformait légèrement (+2.1 buts vs xG). Saison à 73 points, la meilleure des 5 années.

**Acte 2, La chute (2023-24 et 2024-25)** : effondrement à 6 puis 7 buts CPA. Mais attention au xG de 2023-24 : **13.0 xG pour seulement 6 buts** (-7.0 de sous-performance). L'OM créait de bonnes occasions sur CPA, il ne les convertissait juste pas. C'était de la malchance autant que de l'inefficacité. La première saison de De Zerbi est d'un certain point de vue une sacré performance : malgré des CPA inefficaces l'OM a quand même fini 2ème.

**Acte 3, La remontée (2025-26)** : 7 buts CPA, 4e de Ligue 1. Les corners vont mieux (6 buts, 3e en L1). On peut donc se dire que le travail du staff (avec en ligne de mire Pancho Abardonado, apparemment entraîneur en charge des CPA) est inutilement critiqué https://x.com/TeamOM_Officiel/status/2029306295904272504

Mais un trou noir persiste.

<img src="/assets/fr_marseille_evolution_plot.png" alt="Évolution Marseille, Buts CPA vs xG">

---

## 4. Le trou noir marseillais : 0 but sur coups francs indirects

Zéro. 21 tirs issus de coups francs indirects cette saison, pas un seul but. Un xG par tir de 0.060, le pire des 6 premiers de Ligue 1. L'OM ne crée même pas de bonnes occasions sur ces phases de jeu.

<img src="/assets/fr_table_top10_ligue1.png" alt="Top 10 Ligue 1, Phases arrêtées">

Regardons Lorient : 10e au classement, 37 points, et pourtant à égalité avec le PSG et Lens sur les buts CPA (10 chacun). Lorient marque 4 buts sur coups francs indirects. L'OM : zéro. Même Metz, dernier de Ligue 1 (13 points), a marqué 3 buts sur CPA hors corners.

La comparaison est cruelle mais pose des questions.

Soyons justes : le travail du staff a clairement amélioré les corners (de 3 buts / 15e en L1 à 6 buts / 3e). Mais les coups francs indirects (rentrées en touche longues, combinaisons sur coups francs excentrés) restent un chantier ouvert. Les xG le confirment : 0.060 par tir sur CF indirects, contre 0.097 sur corners. 
Il suffirait d'un ou deux buts sur coup franc pour remettre l'OM à niveau me direz-vous... Même pas : L'OM ne crée tout simplement pas de bonnes occasions sur ces phases-là. 

---

## 5. Bayern Munich : la meilleure attaque d'Europe, point final

Si vous cherchez un modèle, c'est le Bayern. Pas parce qu'ils sont les meilleurs sur CPA (Arsenal et Dortmund font 19 buts CPA contre 14), mais parce qu'ils sont les meilleurs **partout en même temps**.

<img src="/assets/fr_bayern_contexte_europeen.png" alt="Bayern en contexte européen">

| Métrique | Bayern | Rang /96 |
|----------|--------|---------|
| Points/match | 2.58 | **1er** |
| Buts/match | 3.46 | **1er** |
| Buts en jeu ouvert | 65 | **1er** |
| Buts CPA/match | 0.54 | 2e |
| Conversion corners | 20.0% | 2e |

C'est le **seul club** situé en haut à droite de tous les nuages de points : élite en jeu ouvert ET en phases arrêtées. Arsenal est co-leader sur CPA (19 buts, à égalité avec Dortmund) mais relativement modeste en jeu ouvert (1.26 but/match). Barcelone est fort en jeu ouvert mais moyen sur CPA. Inter est bon partout mais à un cran en dessous. Le Bayern n'a aucun angle mort.

Et contrairement à Dortmund ou Tottenham, cette efficacité est soutenue par les xG : 9.8 xG CPA pour 14 buts, une surperformance de +4.2, élevée mais pas délirante.

---

## 6. Dortmund : un château de cartes construit sur corners?

Borussia Dortmund est co-leader européen des buts sur phases arrêtées cette saison avec Arsenal : **19 buts** (13 corners, 6 coups francs indirects). Impressionnant ? Oui. Durable ? Probablement pas.

<img src="/assets/fr_bundesliga_big3_evolution.png" alt="Bundesliga Big 3, Évolution 5 saisons">

Le problème est dans les xG : Dortmund a marqué 19 buts pour seulement 11.8 xG. C'est une surperformance de +7.2, la deuxième plus élevée de tout notre dataset (484 équipes-saisons). La seule saison comparable ? Union Berlin 2022-23, avec +10.5 de surperformance sur CPA. L'année suivante, Union Berlin s'est écroulé.

<img src="/assets/fr_table_top20_all_seasons.png" alt="Top 20 meilleures saisons CPA en Europe">

Mais il y a peut-être un signal d'alarme et une "dépendance" : **34.5% des buts de Dortmund viennent de CPA**. Seulement 32 buts en jeu ouvert en 26 matchs, le pire de leur 5 dernières saisons. Les phases arrêtées masquent une attaque en jeu ouvert qui tourne au ralenti. Il reste 8 journées en Bundesliga : la fin de saison dira si cette surperformance tient ou si elle se corrige, comme elle l'a fait historiquement pour d'autres clubs.

---

## 7. Lens : la machine à corners qui s'est enrayée en novembre

Lens est 2e de Ligue 1 avec 56 points et leader sur les corners en L1 (10 buts). Mais il y a un avant et un après novembre.

Sur X (ex-Twitter), [@Laurentmazure](https://x.com/Laurentmazure/status/2032907605606080960) s'agaçait récemment :

> *"Il faudra aussi, un jour, que les médias et ceux qui commentent les matchs du #RCLens (cc Josse-Bravo) arrêtent de dire que le Racing est "efficace" et toujours "dangereux" sur CPA. C'était le cas jusqu'en novembre. Depuis, Lens n'a marqué que 1 de ses 26 derniers buts sur CPA !"*

Et [@MechTuyot](https://x.com/MechTuyot/status/2032909166894092442) de renchérir :

> *"Ah ça… tu marques deux buts en 3 matchs sur cpa, t'es catalogué équipe de cpa pour les 24 mois qui suivent même si tu perds ton tireur et ton mec qui met des têtes… peu importe le championnat et l'équipe."*

Les données leur donnent entièrement raison. Regardez la courbe.

<img src="/assets/fr_lens_timeline.png" alt="Lens, Timeline CPA">

La courbe cumulative est brutale : **9 buts CPA en 14 matchs jusqu'en novembre, puis 1 seul but en 12 matchs depuis**. La courbe se fige complètement après le 22 novembre.

| Période | Matchs | Buts CPA | Conversion | xG CPA |
|---------|--------|---------|-----------|--------|
| Août-Novembre | 14 | **9** | 15.5% | 11.3 |
| Décembre-Mars | 12 | **1** | 1.9% | 5.7 |

Ce n'est pas un problème de volume. Lens continue de tirer (47 tirs CPA depuis décembre). La conversion est passée de 15.5% à 1.9%. Le xG aussi a chuté de moitié. La qualité des occasions s'est dégradée, pas juste la finition. L'observation de @MechTuyot est pertinente : est-ce qu'un tireur ou un "mec qui met des têtes" a été perdu ? Les données ne disent pas qui, mais elles confirment le quand (fin novembre) et l'ampleur (effondrement total). 
Il sera intéressant de voir si Lens retrouve son efficacité d'ici la fin de saison, ou si le décrochage est structurel.h

Le plus ironique : les commentateurs continuent de vanter les CPA lensoises en mars, alors que ça fait **quatre mois** que ça ne fonctionne plus. Les réputations survivent longtemps aux faits, et c'est exactement pour ça qu'on a besoin de données. 
**Message de service: les commentaires/analystes/émissions foot, qui cumulent des milliers d'heures, gagneraient à s'appuyer sur de telles données pour alimenter les débats.** C'est en tout cas un point de départ intéressant pour de nombreux débats. 

---

## 8. La Ligue 1, dernière de la classe sur phases arrêtées

Ce n'est pas juste un problème de Marseille ou de tel club. C'est tout le championnat qui est en retard.

<img src="/assets/fr_ligues_evolution_5saisons.png" alt="Moyennes par ligue, Évolution 5 saisons">

| Ligue | Buts CPA/équipe | Conv. CPA | xG/tir CPA |
|-------|----------------|-----------|-----------|
| **Premier League** | **8.9** | **9.2%** | **0.111** |
| Bundesliga | 7.8 | 9.3% | 0.099 |
| Serie A | 7.8 | 8.3% | 0.100 |
| La Liga | 6.4 | 7.3% | 0.096 |
| **Ligue 1** | **5.3** | **6.7%** | **0.093** |

La Ligue 1 est **dernière sur tout** : moins de buts, moins bonne conversion, moins bonne qualité d'occasion (xG/tir). Et l'écart se creuse : en 2021-22, la L1 était à 9.8 buts CPA par équipe, devant la Premier League (9.2). En 2025-26, elle est à 5.3 contre 8.9. Le décrochage est spectaculaire.

Est-ce un problème tactique ? De qualité des tireurs de corners ? De qualité des défenses ? De gabarit des défenseurs/attaquants sur les phases aériennes ? Probablement un peu de tout. Mais les données sont claires : à investissement égal dans les CPA, un club de Ligue 1 en tirera significativement moins qu'un club de Premier League.

---

## 9. La conversion sur CPA ne prédit rien pour les grosses équipes

C'est peut-être le résultat le plus contre-intuitif de toute l'analyse. On pourrait penser que les équipes qui convertissent le mieux sur CPA sont les meilleures au classement. C'est vrai... en moyenne. Mais en regardant de plus près, c'est beaucoup plus nuancé.

<img src="/assets/fr_analyse_nonlineaire.png" alt="Analyse non-linéaire">

J'ai découpé les 484 équipes-saisons en quartiles de performance (par points, normalisés au sein de chaque ligue-saison). Voici les corrélations entre conversion CPA et points **au sein de chaque tier** :

| Tier | Conv. CPA vs Points (r) |
|------|------------------------|
| Bas 25% | +0.12 |
| 25-50% | +0.05 |
| 50-75% | +0.12 |
| **Haut 25%** | **-0.11** |

**Pour les équipes du top 25%, la conversion CPA est légèrement négativement corrélée avec les points.** Autrement dit, parmi les bonnes équipes, mieux convertir les CPA ne te fait pas monter au classement. Ça peut même être le signe d'une sur-dépendance (hello Tottenham).

Les exemples sont parlants :
- Juventus 2024-25 : 2.7% de conversion CPA, 70 points (4e de Serie A)
- Tottenham 2025-26 : 21.5% de conversion CPA, 30 points (16e de PL)
- Marseille 2024-25 : 6.8% de conversion CPA, 65 points (2e de Ligue 1)

La corrélation glissante (graphique en bas à droite) montre que l'effet de la conversion CPA est maximal entre 30 et 50 points :les équipes de milieu de tableau. Pour elles, les phases arrêtées peuvent faire la différence entre le maintien et la relégation. Mais au-delà de 65 points, l'effet disparaît complètement.

Entre les groupes, l'écart est massif : les 10% meilleurs convertisseurs CPA ont en moyenne 16.8 points de plus que les 10% pires (p<0.001). Mais à l'intérieur d'un même groupe de niveau, la conversion CPA n'explique rien.

---

## 10. Le paradoxe marseillais : et si améliorer les CPA ne changeait rien ?

Résumons la situation de l'OM. Conversion globale de 14.2% (excellente, bien au-dessus de la moyenne). Conversion CPA de 7.6% (médiocre). Zéro but sur coups francs indirects. Le diagnostic est clair : un trou dans la raquette.

Mais les données disent aussi que pour une équipe du calibre de Marseille (top 25% de Ligue 1), **améliorer la conversion CPA ne se traduit pas mécaniquement en points supplémentaires**. L'OM est 2e en 2024-25 avec la pire efficacité corners du top 15. Il est 3e en 2025-26 avec une efficacité nettement meilleure. Le classement n'a pas bougé proportionnellement.

Si l'OM marquait 3-4 buts de plus sur coups francs indirects cette saison (ce qui serait simplement la moyenne de Ligue 1), cela ferait peut-être 2-3 points de plus.
Des scénarios de match peuvent changer également. Mais est-ce que c'est ça qui comble un écart de 8 points avec le PSG et Lens ? Probablement pas.

Les autres (vraies) leviers, c'est le jeu ouvert, la solidité défensive, et arrêter de surperformer ses xPoints de 5 points, ce qui finit souvent par se corriger.

Et c'est exactement ce que dit Habib Beye, [relayé par @MassiliaZone](https://x.com/MassiliaZone/status/2032092476702670859) :

> *"J'accorde beaucoup d'importance au travail mais les situations doivent s'ouvrir aussi sur autre chose que des coups de pieds arrêtés."*

L'entraîneur de l'OM a la même lecture que les données. Les CPA, c'est bien, il faut y travailler (et le staff ne le fait certainement pas assez). Mais le plafond de Marseille ne se joue pas forcément là. Beye le sait, les xG le confirment.

**L'OM doit d'avantage investir dans les CPA, surtout les coups francs, mais le "retour sur investissement" risque d'être modéré. Ce n'est pas le seul levier qui change le plafond de Marseille.**

---

## En résumé

Les CPA sont un bonus, pas un moteur. Tottenham est le meilleur d'Europe sur corners et lutte pour le maintien. L'OM était un des pires de Ligue 1 sur corners en 2024-25 et a fini 2e. Les données de 484 équipes-saisons le confirment : pour les bonnes équipes, la conversion CPA ne prédit rien.

Ce qui distingue Arsenal ou le Bayern, ce n'est pas uniquement d'être bons sur CPA. C'est d'être bons partout. Les phases arrêtées ne sont qu'une cerise sur un gâteau qui doit d'abord exister. Lens est inefficace depuis novembre sur CPA, mais est un très beau et méritant 2ème.  

Bien sûr, sur 2025-26, l'échantillon reste fragile (26-30 journées), et certaines tendances (la surperformance de Dortmund, l'effondrement de Lens, la conversion de Tottenham) pourraient se corriger d'ici la fin de saison. Les quatre saisons précédentes, elles, sont complètes et solides. C'est tout l'intérêt de croiser les analyses. 

On peut parler de football avec des données sans trahir le jeu. Les CPA sont un sujet parfait : assez technique pour que les chiffres apportent quelque chose, assez visible pour que tout le monde ait un avis. Et souvent, les données confirment l'intuition des supporters et des coachs ou des "experts". A titre d'exemple, j'étais persuadé que l'OM était catastrophique sur CPA. C'est un peu plus subtile que ce que je pensais. Aussi, parfois, les données corrigent les avis. 


**Les données sont un point de départ, un indice, du grain à moudre, pas un jugement final. Elles ne disent pas tout, mais poussent parfois à changer ou raffiner son jugement initial ou un avis tranché. Avec la démocratisation des outils et des données, on peut espérer des débats foot un peu moins à l'instinct et un peu plus étayés. Et pourquoi pas, plus passionnants.**

---

*Les données et le code source de cette analyse sont disponibles sur GitHub. Toutes les visualisations ont été générées avec Python (matplotlib, soccerdata, scipy). Les xG proviennent d'Understat. Les simulations Monte Carlo pour les xPoints utilisent 5000 itérations par match avec distribution de Poisson.*

*La saison 2025-26 n'est pas terminée : les chiffres actuels reflètent ~26-30 journées selon les championnats. Les tendances identifiées sont robustes, mais les classements finaux et certaines surperformances (Dortmund, Tottenham) pourraient évoluer. Je publierai une mise à jour en fin de saison pour voir ce qui a tenu et ce qui s'est corrigé.*

*Vous avez un avis, une question, envie de partager l'article sur d'autres medium ? Ou simplement savoir comment je m'y suis pris ? Contactez-moi mathieu.acher@irisa.fr*
