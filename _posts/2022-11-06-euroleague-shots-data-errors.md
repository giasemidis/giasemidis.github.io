---
layout: post
title: Errors in Euroleague shot location data
subtitle:
tags: [euroleague, basketball, analytics, data, shooting data]
readtime: true
social-share: true
---

A recent controversy broke out in the Euroleague game of Real Madrid against Virtus Bologna, where an extra point was suddenly added to Real Madrid's score, see [here](https://twitter.com/3StepsBasket/status/1585747489206173696), followed by errors in the game statistics, see also [here](https://twitter.com/3StepsBasket/status/1585968030240022528). These errors reminded me of errors and inconsistencies I had encountered in the Euroleague's shot data a while ago when I was exploring the dataset.

I had never published any analysis, so let's get into this once again. I am using data that is fetched from the official Euroleague API, see my [Medium blogpost](https://g-giasemidis.medium.com/create-euroleague-shot-charts-in-python-7ba4aa574644) for an introduction, code is available on [github](https://github.com/giasemidis/euroleague-shot-charts).

First, let's get familiar with the dataset. Here is a sample of the data.

![data-sample](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/shot-data-head-example.png)

The columns of interest are:

-   *ID_ACTION*: an ID of the type of shot, with unique values being '2FGA', '2FGAB', '2FGM', '3FGA', '3FGAB', '3FGM', 'DUNK', 'FTM', 'LAYUPATT', 'LAYUPMD'.
-   *ACTION*: the type of shot, with unique values being 'Dunk', 'Free Throw In', 'Layup Made', 'Missed Layup', 'Missed Three Pointer', 'Missed Two Pointer', 'Three Pointer', 'Two Pointer'.
-   *COORD_X*: The x-coordinate of the court, the available values are extended from -740 to 746 units. Given that the width of a FIBA court is 15m, it should be extended from -750cm to 750cm when the basket is centred at x=0 and the units in the data must be cm.
-   *COORD_Y*: The y-coordinate of the court, the available values are extended between -156 to 1304 units (very likely cm as above). The basketball hoop sits at coordinates (0, 0), negative values indicate behind the basket area. The minimum value, -156, is consistent, given that the basket is at 1.2m from the baseline and the basketball hoop has radius 46cm, i.e. 166cm available space behind the basket. Finally, the length of a FIBA half-court is 14m, meaning that the total available length in the data, (1304 + 156 = 1460cm), just extends the 14m length, because a few shots were taken from the other half of the court.
-   *ΖΟΝΕ*: The zone on the court of the court, having unique values 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J'.

In another [blogpost](https://g-giasemidis.medium.com/create-euroleague-shot-charts-in-python-7ba4aa574644), I explained how to draw shot charts on a basketball court plot, focusing on made and missed shots or the density of total shots. Here, I am focusing on three-pointers and two-pointers.

## Errors in data
I started by plotting two-point field goals (FG) attempts (including layups, dunks, etc.) and three-point fields goal attempts, see the next two figures. It becomes obvious that the location shot data is not consistent. Many shots tagged as three pointers are well inside the arc, many are even inside the paint area. And vice-versa, many shots tagged as two-pointers are outside the arc, particularly one shot is well outside.

|![two-point-fga](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/two-point-fg-attempted-since-2010-short-charts.png)|
|:--:|
|Fig. 1: Two-point FGA since season 2010.|

|![three-point-fga](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/three-point-fg-attempted-since-2010-short-charts.png)|
|:--:|
|Fig. 2: Three-point FGA since season 2010.|

I understand that the distinction shouldn't be made by visually comparing the location of the shot to the line of the arc. But, here there are clear inconsistencies independent of the location of the arc. Shots made at similar positions have been tagged differently, raising concerns about the validity of the dataset.

I particularly investigated that long "two-pointer" from Figure 1. According to the collected data, this shot is from the 2019-2020 season, the game of Khimki Moscow Region against Red Star Belgrade and was made by B. Simanic. I found the game details, the shooting-chart and play-by-play, on the official Euroleague site.

The [official shooting chart](https://www.euroleaguebasketball.net/euroleague/game-center/2019-20/khimki-moscow-region-crvena-zvezda-mts-belgrade/E2019/212/#shooting-chart) for B. Simanic from that game is shown below. It becomes obvious that the player had a missed and a made 3-point shots.

|![simanic-shot-chart](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/simanic-shot-chart-khimki-red-start-2019-200.png)|
|:--:|
|Fig. 3: Official Euroleague's shot chart of B. Simanic from the Khimki vs Red Star game in season 2019-2020.|

Then, I looked at the [official stat sheet](https://www.euroleaguebasketball.net/euroleague/game-center/2019-20/khimki-moscow-region-crvena-zvezda-mts-belgrade/E2019/212/#boxscore) from the game and the player has 0/1 2PT FG and 1/1 3PT FG. The missed shot, the one close to the half court line, is counted in the official stat as a 2 pointer. This is consistent to our dataset. I also looked at the official [play-by-play](https://www.euroleaguebasketball.net/euroleague/game-center/2019-20/khimki-moscow-region-crvena-zvezda-mts-belgrade/E2019/212/#play-by-play) and the shot taken at time 4:29 by Simanic is recorded as "Missed Two Pointer (0/1 - 0 pt)". This is the same time-stamp that the "long" two pointer in my dataset.

Could it be that the data was faulty during a particular period due to a systematic error in data collection? I looked into the distribution of inconsistent shots in each season. I focused on registered three-pointers in areas of the court well inside the arc, a box of size `[-400, 400] x [0, 400]` according to the coordinates in the above plots. Similarly, I focused on registered two-pointers well outside the arc, in areas where the y-coordinate is greater than 700. In both cases, I wanted to avoid edge cases near the arc. The distribution of falsely registered type of FG based on their location on the floor is shown in Figure 4. It is evident, that these errors occur systematically every season (season 2022 is just in its 6th round).

|![distribution-false-shots](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/distribution-falsely-registered-shots-by-year.png)|
|:--:|
|Fig. 4: Distribution of falsely registered type of FG based on their location on the court.|


## Conclusion
So, the errors seem to be systematic and not due to an error in data processing or and API malfunction. The official Euroleague data is recorded in an inconsistent way.

I would be interested in knowing the source of such systematic errors and how Euroleague will attempt to address them.

Once I find a systematic way to clean up the data in a way that doesn't bias the dataset, I will write-up about the mid-range and the three-point patterns in Euroleague.
