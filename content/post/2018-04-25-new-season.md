---
title: "Updating the Model for 2018"
description: "Super Netball Predictions"
date: "2018-04-25"
tags: ["r", "sports analytics"]
categories: ["blog", "Super Netball"]
---

<!-- Time-stamp: <2018-04-25 20:09:48 (slane)> -->

[This post]({{< relref "2018-04-22-introduction.md" >}}) described the basics of my model to predict Super Netball score differentials. In this post, I'm going to discuss briefly how I'm using it for the 2018 season.

First of all, I modelled the entire 2017 season using [this code](https://github.com/SteveLane/sn-team-abilities/blob/master/stan/basic-model.stan). From the model, I can extract predicted abilities at the end of the 2017 season, which I can use as priors in the 2018 season.

Due to changes in team rosters and other changes that my model doesn't capture, before using these in my model, I shrink them towards the overall mean ability. This means that the predictions for the first round should not be too outrageous. Note, that the first round predictions will be entirely driven by these prior abilities, as there is no match data to update the random walk model.

Because I am using a hierarchical model fit using MCMC, I need priors for the standard deviations of the abilities, and model parameters. For these, I use the posterior estimates from the 2017 model. Taking the posterior, I fit a Gamma distribution, and estimate its parameters: these are then fed as priors into the [2018 season model](https://github.com/SteveLane/sn-team-abilities/blob/master/stan/season_2018.stan).
