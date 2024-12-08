# 2022-league-of-legends-analysis
an analysis of Pro League of Legends matches from 2022. Homework for EECS 398 at the University of Michigan.


## Introduction
This is an analysis of all the professional League of Legends games played in 2022. There are 150180 rows in the dataset, and each individual game is split up into twelve rows: ten rows to represent each player, and two rows to represent each team.  Each row contains either personal or team data, ranging from kills, gold earned, objectives, champions played, and much more. Most importantly for my personal question, there is also some time-based data, including data like gold at 10 minutes and kills at 10 minutes.

This analysis will be focusing on the "snowballing" effect inherent to League of Legends, where the team that initially gains the lead gains an advantage that further extends the leads. This is why the dataset is interesting; having a twenty point lead in the fourth quarter of a basketball game does not necessarily mean the team with the lead will score more points in the fourth quarter, but in a League of Legends game, a team with a significant gold advantage has a statistical advantage in their characters having a higher level and better items. I want to investigate the effect of snowballing by using the gold difference at 25 minutes to predict the gold difference at the end of the game. The columns of the dataset I will be using are as follows:
1. golddiffat10 - the amount of gold team X has relative to the other team at 10 minutes.
2. golddiffat25 - the amount of gold team X has relative to the other team at 25 minutes.
3. killsat25 - the amount of kills team X has at 25 minutes.
4. xpdiffat 25 - the amount of experience team X has relative to the other team at 25 minutes.
6. totalgold - the amount of gold team X has at the end of the game.
7. result - whether team X wins the game. 
8. firstbaron - whether team X was the first team to kill the Baron Nashor.
9. league - the pro league in which a match was played.

## Data Cleaning and Exploratory Data Analysis
I started by removing all the rows about individual players, as I did not feel that this data was important relative to my goal of predicting teamwide gold data. I also removed team two (red side) in this step, as gold difference is a zero sum statistic and I only need team one's data. I then removed all games that were not labelled as complete, as they did not contain enough data for me to run my model. Notably, they were missing the "golddiffat25" column, which is the main component of my model. I then also removed games that were less than 25 minutes long, for the same reason.
Below is a sample of my dataset:

| teamname                     |   golddiffat10 |   golddiffat25 |
|:-----------------------------|---------------:|---------------:|
| BRION Challengers            |           1523 |             88 |
| T1 Esports Academy           |          -1619 |          -7280 |
| KT Rolster Challengers       |           -103 |           4145 |
| Dplus KIA Challengers        |            337 |          -1160 |
| Kwangdong Freecs Challengers |           -901 |           2516 |

Below is a histogram showing the distribution of gold differences at 15 minutes. The distribution follows a normal distribution, and my question is trying to essentially trying to predict what this histogram looks like at the end of game rather than at fifteen minutes.

<iframe
  src="assets/golddiffat15.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

## Baseline Model

## Final Model
