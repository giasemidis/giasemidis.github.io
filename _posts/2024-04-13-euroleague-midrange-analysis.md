---
layout: post
title: Mid-range analysis of Euroleague - Part II
subtitle: And its impact on teams' performance, player stats and a bonus
tags: [basketball,euroleague,analytics,midrange]
readtime: true
social-share: true
---

In a previous blogspot, [Mid-range analysis of Euroleague - And its impact on teams' performance](https://giasemidis.github.io/2023/12/31/euroleague-midrange-analysis.html), after the first half of the Euroleague season, I presented an analysis of mid-range shots and its impact on team performance and players stats. Here, I repeat the analysis with the full data of the regular season 2023-2024.

In summary, I am trying to address the following hypothesis; *Shooting fewer mid-range attempts increases the probability of winning*.

This analysis was motivated simply by occasionally plotting the shooting charts of teams, see for example [this tweet](https://x.com/g_giase/status/1713184964483686494?s=20), where I started to notice a pattern that winning teams shoot far fewer mid-range buckets than the losing team. However, this observation was based on a handful of games and I wanted to properly examine the subject.

Having complete data from a full season, we are ready to draw a conclusive answer to the question.

## Definition of mid-range

Traditionally, the mid-range shot is defined as any shooting attempt outside the key area and inside the three-point arc. We plot all mid-range shots of the Euroleague regular season 2023-2024 in the following figure.

|![mid-range-shots](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/mid-range-fgs-shot-chart-regular-E-2023-2024.png)|
|:--:|
|Fig. 1: Mid-range shots of the regular Euroleague season 2023-2024.|

## Data collection

The shooting data is collected using the [Euroleague Python API](https://pypi.org/project/euroleague-api/) for all games in the regular season of Euroleague 2023-2024. A sample of the data is shown below.

|![shot-data-sample](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/shot-data-sample.png)|
|:--:|
|Fig. 2: Sample of the shooting data|

## Mid-range shooting by team

First, we estimate the percentage of mid-range shots out of total field goals (FGs) by team (volume of mid-range), see next figure. Red Star and Real Madrid take the least mid-range attempts, with less than 6.5% of their total FG attempts being mid-range. On the other hand, Monaco, Partizan and Bayern attempt the most mid-range shots with more than 14% of their FGs.

|![perc-mid-range](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/midrange-volumne-team-barchart-regular-e2023-2024.png)|
|:--:|
|Fig. 3: Percentage of mid-range shots from total field goals by team - regular season 2023-2024.|

It is interesting that two teams that finished in the top 3 are in the opposite sides of the spectrum, Monaco and Real Madrid.

## Impact on team's performance

Next, we search for correlations between a teams' percentage of midrange volume and their win percentage. The correlation value is $-0.15$, negative, but very weak, almost insignificant.

Also, the linear relationship between mid-range volume percentage and win percentage is plotted in the next figure, where the slope of the trend line is negative, but still very low in absolute value, and has no statistical significance.

|![perc-mid-range](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/midrange-volumne-vs-win-percentage-regular-e2023-2024.png)|
|:--:|
|Fig. 4: Percentage of mid-range shots against the win percentage|

Monaco is having both high volume and accuracy, Mike James being one of the reasons for this performance. On the other hand, Real Madrid, despite shooting with the best accuracy, its play-style does not include many mid-range shots.

Finally, we estimate the probability for a team to win when it shoots fewer mid-range attempts from their opponent. To estimate this, we record which team won and which team shot fewer mid-range attempts in each game. The probability of a team to win while shooting fewer mid-range is 0.49, a marginal increase from 0.48 in the first half of the season, but still almost identical to a flip coin, indicating the insignificant role of shooting mid-range to a team's performance.

From the above, we conclude that there is no strong direct relationship between mid-range volume and team performance.

## Mid-range shooting by player

Which players are the most prolific mid-range shooters? And which players are the best mid-range shooters? We estimate the percentage of mid-range shots taken relative to the total FGs attempted by a player (with minimum of 147 FGs). This is the mid-range volume. (note that 147 is the median of FG attempted in the league, so we only include the top half of the shooters in terms of volume in the plot for graphical reasons)

We also estimate the accuracy of mid-range shots by player, i.e. the number of mid-range shots made divided the total mid-range shots taken by a player (with minimum of 147 FGs).

|![perc-mid-range-player](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/midrange-accuracy-vs-midrange-volume-player-regular-e2023-2024.png)|
|:--:|
|Fig. 5: Mid-range accuracy vs mid-range volumne by player for the regular season of 2023-2024.|

John Brown is the most prolific mid-range shooter, followed by a cluster of players around Kevin Punter. On the other hand, Tamir Blatt, Matthew Castello and Gabriel Deck are the most efficient mid-range shooters.

## Conclusion

There seems to be no relationship between mid-range attempts volume and team performance (expressed via the win percentage). Hence, the hypothesis cannot be validated from this season's data. Teams with high mid-range volume have efficient mid-range shooters and seem to exploit their efficiency.

Teams, like Monaco, Partizan and Panathinaikos, adjust their play style to their best players give them freedoms and create many mid-range shots. Remember, a mid-range shot is inefficient only if taken by mediocre shooters.

## Bonus

As a bonus, we present the leading scorers by zone (as defined by the Euroleague API shooting data) for the Euroleague regular season 2023-2024.

M. Lessort is dominating the rim. Great mid-range shooting guards, such as K. Nunn, K. Punter and M. James are dominant in the mid-range. M. Howard is the leading from the three point line.

|![leadings-scorers-by-zone](https://raw.githubusercontent.com/giasemidis/giasemidis.github.io/master/_posts/figures/leading-scorers-by-zone-regular-e2023-2024.png)|
|:--:|
|Fig. 6: Leading scorers by zone regular season 2023-2024|
