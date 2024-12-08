# 2022-league-of-legends-analysis
an analysis of Pro League of Legends matches from 2022. Homework for EECS 398 at the University of Michigan.


## Introduction
This is an analysis of all the professional League of Legends matches played in 2022. There are 150180 rows in the dataset. This analysis will be focusing on the "snowballing" effect inherent to League of Legends, where the team that initially gains the lead gains an advantage that further extends the leads. This is why the dataset is interesting; having a twenty point lead in the fourth quarter of a basketball game does not necessarily mean the team with the lead will score more points in the fourth quarter, but in a League of Legends game, a team with a significant gold advantage has a statistical advantage in their characters having a higher level and better items. I want to investigate the effect of snowballing by using the gold difference at 25 minutes to predict the gold difference at the end of the game. The columns of the dataset I will be using are as follows:
1. golddiffat10 - the amount of gold team X has relative to the other team at 10 minutes.
2. golddiffat25 - the amount of gold team X has relative to the other team at 25 minutes.
3. killsat25 - the amount of kills team X has at 25 minutes.
4. xpdiffat 25 - the amount of experience team X has relative to the other team at 25 minutes.
6. totalgold - the amount of gold team X has at the end of the game.
7. result - whether team X wins the game. 
8. firstbaron - whether team X was the first team to kill the Baron Nashor.
9. league - the pro league in which a match was played.

## Data Cleaning and Exploratory Data Analysis

## Framing a Prediction Problem

## Baseline Model

## Final Model
