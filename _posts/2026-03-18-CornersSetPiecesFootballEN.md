---
layout: post
title:  "Corners, Free Kicks, and Set Pieces Across Europe's Top Football Leagues: What the Data Actually Says"
date:   2026-03-18 12:00:00 +0100
tags: [football, data analysis, corners, set pieces, Ligue 1, Premier League, Marseille, Arsenal, xG, Understat]
---

Corners and set pieces are one of football's most debated topics. "Arsenal are unstoppable from corners," "Marseille are hopeless on free kicks," and so on. Opinions, gut feelings, conventional wisdom. But sometimes the data surprises: did you know that Tottenham, fighting relegation in the Premier League, are on the European podium for corner goals this season, just behind Arsenal and Inter?
What if we actually checked?
I analyzed **5 seasons of data** across Europe's top 5 leagues (Premier League, Ligue 1, Serie A, Bundesliga, La Liga). Every shot, every goal, every expected goal (xG) from corners, indirect free kicks, and direct free kicks. 484 team-seasons. Here's what I found in 10 key takeaways, and some results are surprising, underappreciated, or counter-intuitive.

*Data: Understat via understatapi. 5 European leagues, 5 seasons (2021-2026), 484 team-seasons. Note: the 2025-26 season is ongoing (~26-30 matchdays depending on the league). Trends are clear but figures will evolve. I'll update at the end of the season.*

## 1. Tottenham, kings of corners... and 16th in the Premier League

This is the wildest story of the current season (~30 PL matchdays played). Tottenham are **3rd in Europe** for goals scored from corners: 14 goals, behind Arsenal (16) and Inter (15). A 21.5% conversion rate, the best in Europe.

The problem? Spurs are currently **16th in the Premier League** with 30 points.

<img src="/assets/en_tottenham_open_vs_corners.png" alt="Tottenham vs Arsenal: Open Play vs Corner Goals">

The chart speaks for itself. Tottenham are isolated in the top left: lots of corner goals, very few from open play (25 goals, 14th in the league). Arsenal sit in the top right: 16 corner goals AND 39 from open play.

**35.0% of Tottenham's goals come from set pieces.** That's the highest rate among major league clubs (excluding relegation-zone teams). When over a third of your goals depend on situations that account for ~15% of all shots, you're building on sand.

For comparison, Bayern Munich, Europe's best attack, are at just 15.6% set piece dependency. Because when you score 65 open play goals, corners are a bonus, not a crutch.

And then there's Arsenal, the outlier: **16 corner goals (1st in Europe)**, plus 39 from open play, 70 points (1st in the Premier League). Arsenal aren't just lethal offensively from set pieces, they dominate defensively too. In their Champions League match against Bayer Leverkusen, [Bayer 04's official account](https://x.com/bayer04_en/status/2031723079886401891) noted that Arsenal conceded zero corners. In a [controversial interview](https://talksport.com/football/4043546/john-obi-mikel-interview-arsenal-chelsea-mourinho-nigeria/), John Obi Mikel went as far as accusing Arsenal of cheating or suggesting their success depends solely on corners. Tottenham are the perfect counter-example: there's far more to football than corners.
One could also argue Tottenham are in serious danger: without set pieces, their situation would be even worse than 16th place.

---

## 2. Elche 2022-23: 107 corner shots, the anatomy of desperation

Beyond Tottenham in 2025-26, there are other extreme cases. Elche, 2022-23 season, La Liga. The team was relegated with 25 points, yet they fired **107 shots from corners**, a La Liga record that season.

<img src="/assets/en_elche_corners_2022_23.png" alt="Corner shot distribution, La Liga 2022-23">

The z-score is 2.26, a statistical outlier. But the right-hand chart is even more telling: **26.4% of all Elche's shots came from corners**. The La Liga average is 15.6%. No one else exceeds 19%.

Why? Because Elche couldn't do anything else. Just 406 total shots (the lowest in La Liga), non-existent open play creation. Corners were their only route to goal. Lucas Boye, their target man, took 19 corner shots alone, shouldering the entire aerial workload.

When you can't do anything else, you're left with corners. Tottenham 2025-26 is a less extreme version of the same syndrome.

For the record, I haven't watched a single Elche match. But the statistics are quite clear (I also checked whether this was a data anomaly or a single misrecorded match inflating the count, but no). **I'm looking for Elche followers or supporters, because this is a truly remarkable phenomenon**, and despite some searching, I haven't found anything documenting this Elche exploit.

---

## 3. Marseille, from shame to (partial) renaissance on corners

In November 2025, [@Rubenzf911](https://x.com/Rubenzf911/status/1985468834401116253) asked the question many Marseille supporters had on their minds:

> *"OM's set pieces, is anyone ever going to sound the alarm?"*

The alarm, the data had been ringing it for a while. Here's Marseille's evolution over 5 seasons:

<img src="/assets/en_table_marseille_evolution.png" alt="Marseille evolution, 5 seasons">

The table tells a three-act story:

**Act 1, The glory (2022-23)**: 17 dead ball goals (1st in Ligue 1!), 11 from corners alone, 14.9 xG. OM were slightly overperforming (+2.1 goals vs xG). A 73-point season, their best in 5 years.

**Act 2, The collapse (2023-24 and 2024-25)**: a crash to 6 then 7 dead ball goals. But note the 2023-24 xG: **13.0 xG for just 6 goals** (-7.0 underperformance). OM were creating good set piece chances, they just weren't converting them. It was bad luck as much as inefficiency. De Zerbi's first season was, from this perspective, quite a feat: despite dreadful set pieces, OM still finished 2nd.

**Act 3, The recovery (2025-26)**: 7 dead ball goals, 4th in Ligue 1. Corners are improving (6 goals, 3rd in L1). One could argue the staff's work (particularly Pancho Abardonado, apparently the set piece coach) has been [unfairly criticized](https://x.com/TeamOM_Officiel/status/2029306295904272504).

But a black hole remains.

<img src="/assets/en_marseille_evolution_plot.png" alt="Marseille evolution: DB Goals vs xG">

---

## 4. Marseille's black hole: 0 goals from indirect free kicks

Zero. 21 shots from indirect free kicks this season, not a single goal. An xG per shot of 0.060, the worst among Ligue 1's top 6. OM aren't even creating good chances from these situations.

<img src="/assets/en_table_top10_ligue1.png" alt="Top 10 Ligue 1, Dead Ball Goals">

Look at Lorient: 10th in the table, 37 points, yet **level with PSG and Lens** on dead ball goals (10 each). Lorient score 4 from indirect free kicks. Marseille: zero. Even Metz, bottom of Ligue 1 (13 points), have scored 3 non-corner dead ball goals.

The comparison is harsh but raises questions.

To be fair, the coaching staff have clearly improved corners (from 3 goals / 15th in L1 to 6 goals / 3rd). But indirect free kicks (long throw-ins, set plays from wide free kicks) remain an open construction site. The xG confirms it: 0.060 per shot on indirect FKs, versus 0.097 on corners. You might think one or two free kick goals would bring OM back to average. Not even: OM simply aren't creating good chances from these situations.

---

## 5. Bayern Munich: the best attack in Europe, period

If you're looking for a model, it's Bayern. Not because they're the best from set pieces (Arsenal and Dortmund have 19 goals vs 14), but because they're the best **at everything simultaneously**.

<img src="/assets/en_bayern_european_context.png" alt="Bayern in European context">

| Metric | Bayern | Rank /96 |
|--------|--------|---------|
| Points/match | 2.58 | **1st** |
| Goals/match | 3.46 | **1st** |
| Open play goals | 65 | **1st** |
| DB goals/match | 0.54 | 2nd |
| Corner conversion | 20.0% | 2nd |

They're the **only club** positioned in the top right of every scatter plot: elite in open play AND from set pieces. Arsenal are co-leaders on set pieces (19 goals, tied with Dortmund) but relatively modest in open play (1.26 goals/match). Barcelona are strong in open play but average from set pieces. Inter are good at both but a notch below. Bayern have no blind spots.

And unlike Dortmund or Tottenham, this efficiency is backed by xG: 9.8 DB xG for 14 goals, an overperformance of +4.2, high but not outrageous.

---

## 6. Dortmund: a house of cards built on corners?

Borussia Dortmund co-lead Europe in dead ball goals this season with Arsenal: **19 goals** (13 corners, 6 indirect free kicks). Impressive? Yes. Sustainable? Probably not.

<img src="/assets/en_bundesliga_big3_evolution.png" alt="Bundesliga Big 3, 5-season evolution">

The problem is in the xG: Dortmund have scored 19 goals from just 11.8 xG. That's an overperformance of +7.2, the second highest in our entire dataset (484 team-seasons). The only comparable season? Union Berlin 2022-23, with +10.5 overperformance on set pieces. The following year, Union Berlin collapsed.

<img src="/assets/en_table_top20_all_seasons.png" alt="Top 20 dead ball seasons in Europe">

But there's perhaps a warning signal and a "dependency": **34.5% of Dortmund's goals come from set pieces**. Just 32 open play goals in 26 matches, their worst in 5 seasons. Set pieces are masking an open play attack that's sputtering. 8 Bundesliga matchdays remain: the end of the season will tell whether this overperformance holds or corrects, as it has historically for other clubs.

---

## 7. Lens: the corner machine that broke down in November

Lens are 2nd in Ligue 1 with 56 points and L1's corner leader (10 goals). But there's a before and after November.

On X (formerly Twitter), [@Laurentmazure](https://x.com/Laurentmazure/status/2032907605606080960) recently complained:

> *"At some point, the media and pundits covering RC Lens need to stop saying the team is 'effective' and 'dangerous' from set pieces. That was true until November. Since then, Lens have scored just 1 of their last 26 goals from set pieces!"*

And [@MechTuyot](https://x.com/MechTuyot/status/2032909166894092442) added:

> *"That's the thing... you score two goals in 3 matches from set pieces, and you're labeled a set piece team for the next 24 months, even if you lose your taker and your aerial threat... regardless of the league or the team."*

The data proves them entirely right. Look at the curve.

<img src="/assets/en_lens_timeline.png" alt="Lens, Dead Ball Timeline">

The cumulative curve is brutal: **9 dead ball goals in 14 matches through November, then just 1 in 12 matches since**. The curve flatlines completely after November 22.

| Period | Matches | DB Goals | Conversion | DB xG |
|--------|---------|---------|-----------|-------|
| Aug-Nov | 14 | **9** | 15.5% | 11.3 |
| Dec-Mar | 12 | **1** | 1.9% | 5.7 |

It's not a volume problem. Lens keep shooting (47 dead ball shots since December). Conversion dropped from 15.5% to 1.9%. The xG also halved. The quality of chances has degraded, not just the finishing. @MechTuyot's observation is relevant: was a set piece taker or an aerial threat lost? The data doesn't say who, but it confirms the when (late November) and the magnitude (total collapse).
It will be interesting to see whether Lens recover their efficiency by the end of the season, or whether the drop-off is structural.

The most ironic part: pundits continue praising Lens' set pieces in March, when they haven't worked for **four months**. Reputations outlive facts, and that's precisely why we need data.

---

## 8. Ligue 1, bottom of the class on set pieces

This isn't just a Marseille problem or any single club's issue. It's the **entire league** that's behind.

<img src="/assets/en_leagues_evolution_5seasons.png" alt="League averages, 5-season evolution">

| League | DB Goals/team | DB Conv | xG/DB shot |
|--------|-------------|---------|-----------|
| **Premier League** | **8.9** | **9.2%** | **0.111** |
| Bundesliga | 7.8 | 9.3% | 0.099 |
| Serie A | 7.8 | 8.3% | 0.100 |
| La Liga | 6.4 | 7.3% | 0.096 |
| **Ligue 1** | **5.3** | **6.7%** | **0.093** |

Ligue 1 is **last on every metric**: fewer goals, worse conversion, lower shot quality (xG/shot). And the gap is widening: in 2021-22, L1 averaged 9.8 DB goals per team, ahead of the Premier League (9.2). By 2025-26, it's 5.3 versus 8.9. The decline is spectacular.

Is it a tactical issue? Corner-taking quality? Defensive quality? The physique of attackers/defenders in aerial duels? Probably a bit of everything. But the data is clear: for equal investment in set pieces, a Ligue 1 club will get significantly less return than a Premier League club.

---

## 9. Dead ball conversion predicts nothing for top teams

This may be the most counter-intuitive finding of the entire analysis. You'd think the teams that convert best from set pieces are the ones at the top of the table. That's true... on average. But look closer, and it's far more nuanced.

<img src="/assets/en_nonlinear_analysis.png" alt="Non-linear analysis">

I split the 484 team-seasons into performance quartiles (by points, normalized within each league-season). Here are the correlations between dead ball conversion and points **within each tier**:

| Tier | DB Conv vs Points (r) |
|------|----------------------|
| Bottom 25% | +0.12 |
| 25-50% | +0.05 |
| 50-75% | +0.12 |
| **Top 25%** | **-0.11** |

**For teams in the top 25%, dead ball conversion is slightly negatively correlated with points.** In other words, among good teams, converting more from set pieces doesn't help you climb the table. It can even signal over-dependency (hello Tottenham).

The examples speak for themselves:
- Juventus 2024-25: 2.7% DB conversion, 70 points (4th in Serie A)
- Tottenham 2025-26: 21.5% DB conversion, 30 points (16th in PL)
- Marseille 2024-25: 6.8% DB conversion, 65 points (2nd in Ligue 1)

The rolling correlation (bottom right chart) shows the effect of DB conversion is **strongest between 30 and 50 points**, i.e. mid-table teams. For them, set pieces can make the difference between survival and relegation. But above 65 points, the effect vanishes entirely.

Between groups, the gap is massive: the top 10% DB converters average **16.8 more points** than the bottom 10% (p<0.001). But within a given performance group, DB conversion explains nothing.

---

## 10. The Marseille paradox: what if improving set pieces changes nothing?

Let's summarize OM's situation. Overall conversion of 14.2% (excellent, well above average). Dead ball conversion of 7.6% (poor). Zero goals from indirect free kicks. The diagnosis is clear: a gap in the armor.

But the data also says that for a team of Marseille's caliber (top 25% in Ligue 1), **improving dead ball conversion doesn't mechanically translate to more points**. OM finished 2nd in 2024-25 with one of the worst corner records in L1's top 15. They're 3rd in 2025-26 with significantly better efficiency. The table hasn't shifted proportionally.

If OM scored 3-4 more goals from indirect free kicks this season (which would simply be the Ligue 1 average), it might mean 2-3 extra points. Match scenarios could change too. But is that what closes an 8-point gap to PSG and Lens? Probably not.

The real levers are open play quality, defensive solidity, and not overperforming xPoints by 5 points, which tends to correct over time.

And that's exactly what Habib Beye says, [relayed by @MassiliaZone](https://x.com/MassiliaZone/status/2032092476702670859):

> *"I value hard work, but situations need to open up beyond just set pieces."*

Marseille's coach reads it the same way as the data. Set pieces are worth working on (and the staff probably don't do it enough). But Marseille's ceiling doesn't depend on it. Beye knows it, the xG confirms it.

**OM should invest more in set pieces, especially free kicks, but the return on investment is likely to be moderate. It's not the only lever that changes Marseille's ceiling.**

---

## In summary

Set pieces are a bonus, not an engine. Tottenham are on Europe's corner podium (14 goals, 3rd) and are fighting relegation. Marseille were among Ligue 1's worst from corners in 2024-25 and finished 2nd. Data from 484 team-seasons confirms it: for good teams, dead ball conversion predicts nothing.

What sets Arsenal or Bayern apart isn't just being good from set pieces. It's being good at everything. Set pieces are merely the icing on a cake that needs to exist first. Lens have been ineffective from set pieces since November, but are a deserving and impressive 2nd.

Of course, the 2025-26 sample remains fragile (26-30 matchdays), and some trends (Dortmund's overperformance, Lens' collapse, Tottenham's conversion) could correct by season's end. The four previous seasons are complete and robust. That's the whole point of cross-referencing analyses.

You can talk about football with data without betraying the game. Set pieces are a perfect subject: technical enough for numbers to add value, visible enough for everyone to have an opinion. And often, data confirms the intuition of supporters and coaches. For instance, I was convinced Marseille were catastrophic on set pieces. It's a bit more subtle than I thought. Sometimes, data refines our views.

**Data is a starting point, a clue, food for thought, not a final verdict. It doesn't tell everything, but sometimes pushes you to change or refine a snap judgement. With the democratization of tools and data, we can hope for football debates that are a little less instinct-driven and a little more evidence-based. And why not, more compelling.**

---

*Data and source code for this analysis are available on GitHub. All visualizations were generated with Python (matplotlib, soccerdata, scipy). xG data comes from Understat. Monte Carlo simulations for xPoints use 5,000 iterations per match with a Poisson distribution.*

*The 2025-26 season is not over: current figures reflect ~26-30 matchdays depending on the league. The trends identified are robust, but final standings and some overperformances (Dortmund, Tottenham) may shift. I'll publish an update at the end of the season to see what held up and what corrected.*

*Have an opinion, a question, or want to share this article? Want to know how I did it? Contact me at mathieu.acher@irisa.fr*
