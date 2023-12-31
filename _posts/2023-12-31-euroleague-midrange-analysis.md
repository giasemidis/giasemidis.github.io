---
layout: post
title: Mid-range analysis of Euroleague
subtitle: And its impact on teams' performance
tags: [basketball,euroleague,analytics,midrange]
readtime: true
social-share: true
---

The play style of basketball has evolved over the past two decades. Mid-range shots have drastically decreased in favour of more efficient three-point attempts in the NBA, an excellent overview of the topic is presented in the book [Sprawlball: A Visual Tour of the New Era of the NBA](https://www.goodreads.com/en/book/show/40796173) by Kirk Goldsberry.

However, how are things evolving in European basketball and Euroleague? Here, I don't aim to examine any historical patterns over the years (which will be the subject of another blogpost), but I rather focus on the impact of volume of mid-range shots on a team's performance in the first half of the 2023-2024 Euroleague season (analysis will be updated as the season progresses). The analysis can be found on [this GitHub notebook](https://github.com/giasemidis/euroleague-shot-charts/blob/main/notebooks/mid-range-analysis.ipynb).

This analysis was motivated simply by occasionally plotting the shooting charts of teams, see for example [this tweet](https://x.com/g_giase/status/1713184964483686494?s=20), where I started to notice a pattern that winning teams shoot far fewer mid-range buckets than the losing team. However, this observation was based on a handful of games and I wanted to properly examine the subject.

In summary, I am trying to address the following hypothesis; *Shooting fewer mid-range attempts increases the probability of winning*.

## Definition of mid-range

Traditionally, the mid-range shot is defined as any shooting attempt outside the key area and inside the three-point arc. We plot all mid-range shots of the first half of the season 2023-2024 in the following figure.

|![mid-range-shots](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/mid-range-fgs-shot-chart-up-to-round-17-2023-2024.png)|
|:--:|
|Fig. 1: Mid-range shots of the first half of the 2023-2024 season|

## Data collection

The shooting data is collected using the [Euroleague Python API](https://pypi.org/project/euroleague-api/) for all games in the fist half of the 2023-2024 season. A sample of the data is shown below.

|![shot-data-sample](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/shot-data-sample.png)|
|:--:|
|Fig. 2: Sample of the shooting data|

## Mid-range shooting by team

First, we estimate the percentage of mid-range shots out of total field goals (FGs) by team, see next figure. Red Star, Real Madrid  and Olympiacos take the least mid-range attempts, with less than 7.2% of their total FG attempts being mid-range. On the other hand, Valencia, Partizan, Bayern and Monaco attempt the most mid-range shots with more than 13.2% of their FGs.

|![perc-mid-range](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/mid-range-fgs-perc-up-to-round-17-2023-2024.png)|
|:--:|
|Fig. 3: Percentage of mid-range shots from total field goals by each team until round 17 of season 2023-2024.|

## Impact on teams performance

Next, we search for correlations between a teams' percentage of midrange volume to their win percentage so far in the season. The correlation value is $-0.095$, negative, but very weak.

Also, the linear relationship between between mid-range percentage and win percentage, see next figure, where the slope of the trend line is negative, but still very low in absolute value, and has no statistical significance.

|![perc-mid-range](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/mid-range-perc-vs-win-perc-scatter-plot.png)|
|:--:|
|Fig. 4: Percentage of mid-range shots against the win percentage|

Finally, we estimate the probability for a team to win when it shoots fewer mid-range attempts from their opponent. To estimate this, we record which team won and which team shot fewer mid-range attempts  in each game. The probability of a team to win while shooting fewer mid-range is 0.48.

From the above, we conclude that there is no strong direct relationship between mid-range volume and team performance. This analysis will be updated towards the end of the season.

## Mid-range shooting by player

Which players are the most prolific mid-range shooters? We estimate the percentage of mid-range shots taken relative to the total FGs attempted by the player (with minimum of 80 FGs). Here are the top 10 mid-range shooters based on relative volume of their shots.

|RANK| PLAYER                   | TEAM                          |  Mid-range %|
|---:|:-------------------------|:------------------------------|------------:|
|  1 | BROWN, JOHN              | AS Monaco                     |        35.1 |
|  2 | DAVIES, BRANDON          | Valencia Basket               |        28.7 |
|  3 | PUNTER, KEVIN            | Partizan Mozzart Bet Belgrade |        28.0 |
|  4 | SPAGNOLO, MATTEO         | ALBA Berlin                   |        27.5 |
|  5 | BOLMARO, LEANDRO         | FC Bayern Munich              |        26.1 |
|  6 | VESELY, JAN              | FC Barcelona                  |        26.1 |
|  7 | LUWAWU-CABARROT, TIMOTHE | LDLC ASVEL Villeurbanne       |        24.7 |
|  8 | BOOKER, DEVIN            | FC Bayern Munich              |        24.2 |
|  9 | BELINELLI, MARCO         | Virtus Segafredo Bologna      |        23.9 |
| 10 | JAMES, MIKE              | AS Monaco                     |        23.5 |

We observe that among the top 5 are players from the cluster of high volume mid-range teams, Monaco, Valencia, Partizan and Bayern.

Monaco, Partizan and Valencia have players of the quality of John Brown, Mike James, Brandon Davies and Kevin Punter who are mid-range specialists and exploit their high efficiency from the mid-range distance.

## Conclusion

There seems to be no relationship between mid-range attempts volume and team performance (expressed via the win percentage). Hence, my hypothesis cannot be validated from this season's data. Teams with high mid-range volume have efficient mid-range shooters and seem to exploit their efficiency.

## Bonus

As a bonus, we present the leading scorers by zone (as defined by the shoti data) for Euroleague until round 17 (first half of the season) 2023-2024.

Poirier is dominant under the rim. Toko Shengelia is dominant in the low post area. Mike James is the king of the right side of mid range.

|![leadings-scorers-by-zone](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/leading-scorers-by-zone-round-17-2023-2024.png)|
|:--:|
|Fig. 5: Leading scorers by zone (first half of 2023-2024 season)|
