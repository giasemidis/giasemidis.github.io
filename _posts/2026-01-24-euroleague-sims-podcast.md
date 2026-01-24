---
layout: post
title: "The EuroLeague Multiverse - Simulating the Season"
subtitle: A deep dive into the making of the Euroleague Simulations
tags: [euroleague, basketball, simulations, analytics, podcast]
readtime: true
social-share: true
---


Last year, I had the pleasure of presenting at the [Data Science Festival 2025 in London](https://datasciencefestival.com/session/euroleague-basketball-analytics-with-python-knowledge-graphs-and-simulations/), where I talked about advanced analytics in European basketball, particularly the Euroleague. While the full talk covered a wide range of topics, one specific segment sparked the most debate: the simulation engine used to predict the Euroleague's Final Four.

To make this technical deep dive more accessible, I have generated an AI-hosted podcast episode using the content from that specific segment.

## 🎧 Listen to the Podcast: The Data Paint

In this 15-minute episode, our AI hosts, Alex and Sarah, break down the "recipe" for simulating the EuroLeague season. They discuss how we moved beyond simple win/loss predictions to creating a "multiverse" of outcomes.

**Audio File:**
![Audio](media/euroleague-sims-ai-podcast.m4a)

**Key Topics Covered:**

* **The Simulation Recipe:** The three elements required for creating a simulation model
* **The Game Prediction model:** How we combined historical ELO ratings with current Net Ratings to solve the "cold start" problem, include historical team dynamics and capture team current momentum.
* **The Multiverse** Why we ran **10,000 stochastic iterations** (universes) to smooth out variance and identify the true favourites for the Final Four.
* **Q&A** A candid look at why European basketball analytics lags behind the NBA and European football due to expensive, gatekept data.

***

## 📺 Watch the Original Presentation

If you prefer to see full presentation, you can watch the original video below. The podcast above focuses specifically on the **Simulation Methodology** and **Q&A** segment, which runs from timestamp **26:37 to 33:50**.

![You Tube](https://www.youtube.com/watch?v=m9oUu1wHsDM)
*(Note: Skip to 26:37 for the start of the simulation discussion)*

### A Note on Democratising Data

As discussed in the Q&A, a major goal of this project is to democratise access to EuroLeague data. Unlike the NBA, where data is either free or heavily invested in, European basketball data is often expensive and hard to reach.

I encourage you to check out the open-source tools, like the [Euroleague-API](https://pypi.org/project/euroleague-api/) mentioned in the talk and help us build a better analytics culture for the game we love.
