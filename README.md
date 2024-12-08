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

|    | teamname                     |   golddiffat10 |   golddiffat25 |
|---:|:-----------------------------|---------------:|---------------:|
| 10 | BRION Challengers            |           1523 |             88 |
| 22 | T1 Esports Academy           |          -1619 |          -7280 |
| 46 | KT Rolster Challengers       |           -103 |           4145 |
| 70 | Dplus KIA Challengers        |            337 |          -1160 |
| 94 | Kwangdong Freecs Challengers |           -901 |           2516 |

Below is a histogram showing the distribution of gold differences at 15 minutes. The distribution follows a normal distribution, and my question is trying to essentially trying to predict what this histogram looks like at the end of game rather than at fifteen minutes.

<iframe
  src="assets/golddiffat15.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

Below is the same histogram as above, just showing the gold difference at 25 minutes instead of 15 minutes. Notice that the distribution has spread out further than compared to at 15 minutes, but still follows a normal distribution.

<iframe
  src="assets/golddiffat25.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

Below is a scatterplot comparing the gold difference at 15 minutes vs. 25 minutes. There is a clear positive linear association, i.e. having a gold lead at 15 minutes implies that team will have a gold lead at 25 minutes, and vice versa, but there are many outliers. 

<iframe
  src="assets/golddiffat15vs25.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

Below is a pivot table I constructed to show the correlation between having a positive and negative gold difference at ten minutes to winning. There is a strong correlation between winning and having a greater positive gold difference, so its important to see how often a positive gold difference actually converts into a win.

|        |   Negative Gold Difference |   Positive Gold Difference |
|:-------|---------------------------:|---------------------------:|
| Losses |                       3031 |                       1552 |
| Wins   |                       1574 |                       3355 |

I did not want to impute any values, such as golddiffat25, because I did not have the data necessary to do so, and using mean imputation or other kinds of imputation would be essentially making up data that would take away from the real data. Therefore, I did not impute any values.

## Framing a Prediction Problem
As previously stated, I am trying to predict what the final gold difference is at the end of the game. This is a classic regression problem, and the response variable is the blue side final gold difference. I am using R-squared to measure the performance of my model, as I am using multiple features in my model. I will set my "time of prediction" to 25-30 minutes, so the information I have available will be the "snapshots" at 10, 15, 20, and 25 minutes, along with team information, but I will not have the result of the game or how long the game will last. 


## Baseline Model
The base model I used for the prediction is a LinearRegression model using "golddiffat25" and "killsat25," which are both quantitative variables. This base model had an R-squared of 0.708. This model is decent enough, but it lacks critical information about game states, assuming that gold and kills are the *only* things that matter. 

## Final Model
For my final model, I added three variables:
1. xpdiffat25 - quantitative
2. firstbaron - qualitative
3. region - qualitative

I added these variables because I believe these variables help predict the game state better, as these are extra variables aside from a gold lead that are associated with winning. It should be noted that there is no timestamp associated with the "firstbaron" column, so it is possible that the first baron is taken after 25 minutes, but typically the first baron is taken close enough to 25 minutes that I felt comfortable including it. The region variable was added because there is an generally held association that certain regions convert leads better, and I wanted to factor this in. 

Instead of using a LinearRegression model, I tested both Ridge and Lasso models so I could try extra hyperparameters that LinearRegression does not allow for. I used a GridSearchCV to iterate through three hyperparameters: alpha, tolerance, and max iterations. What I found was that an Alpha of 1, Max Iterations of 1000, and a tolerance of 0.001. This resulted in an R-squared of 0.775, compared to the baseline model performance of 0.708, a marked improvement.

