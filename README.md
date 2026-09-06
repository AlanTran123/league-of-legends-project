# League of Legends Objective Control and Match Outcomes

Authors: Alan Tran, Alexander Phan

## Introduction

### General Introduction

League of Legends is a multiplayer online battle arena game where two teams compete to destroy the opposing team's base. In professional matches, teams gain advantages through kills, gold, map pressure, and objectives such as dragons, Rift Heralds, Barons, and towers. These objectives can help teams build control over the map and increase their chances of winning.

This project uses professional League of Legends esports match data from Oracle's Elixir. The dataset contains match, player, and team statistics from professional games between 2014 and 2026. After selecting the columns relevant to our project, our dataset contains **1,215,564 rows** and **35 columns**.

The central question of this project is:

**How important is early-game objective control in determining whether a professional League of Legends team wins?**

This question is important because objectives are a major part of professional League of Legends strategy. By analyzing objective-control statistics, we can better understand whether teams that secure early objectives and major objectives are more likely to win.

### Introduction of Columns

The main columns relevant to our question are:

- `gameid`: Unique identifier for each match.
- `league`: The professional league or tournament the match came from.
- `year`: The year the match was played.
- `side`: Whether the team played on blue side or red side.
- `teamname`: The name of the team.
- `position`: Used to identify team-level rows versus player-level rows.
- `result`: The match outcome, where 1 means the team won and 0 means the team lost.
- `firstdragon`: Whether the team secured the first dragon.
- `firstherald`: Whether the team secured the first Rift Herald.
- `firstbaron`: Whether the team secured the first Baron.
- `firsttower`: Whether the team destroyed the first tower.
- `dragons`: Total number of dragons secured by the team.
- `heralds`: Total number of Rift Heralds secured by the team.
- `barons`: Total number of Barons secured by the team.
- `towers`: Total number of towers destroyed by the team.
- `goldat10`: Team gold at 10 minutes.
- `golddiffat10`: Team gold difference at 10 minutes.
- `xpdiffat10`: Team experience difference at 10 minutes.
- `csdiffat10`: Team creep score difference at 10 minutes.
- `killsat10`: Team kills at 10 minutes.
- `deathsat10`: Team deaths at 10 minutes.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

To focus on our project question, we kept only the columns related to match identity, team information, objective control, early-game statistics, and match outcome. Since our analysis is about whether teams win, we filtered the dataset to only include team-level rows instead of individual player rows.

We also limited the dataset to matches from 2023 and later because League of Legends changes often through patches, champion balance updates, and strategy changes. This makes our analysis more focused on the modern professional game.

For cleaning, we kept only rows marked as complete, converted the `date` column into datetime format, and converted objective and early-game columns into numeric values. We then dropped rows with missing values in the key columns needed for our analysis, such as `result`, `firstdragon`, `firsttower`, `dragons`, `towers`, `goldat10`, and `golddiffat10`.

After cleaning, our dataset contains **71,374 rows**.

Below is the head of our cleaned DataFrame:

| gameid | league | year | side | teamname | result | firstdragon | firsttower | dragons | towers | goldat10 | golddiffat10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ESPORTSTMNT01_2984361 | LHE | 2023 | Blue | Eclipse Gaming | 1 | 0 | 0 | 2 | 7 | 14821 | 486 |
| ESPORTSTMNT01_2984361 | LHE | 2023 | Red | Wolf Club Esports | 0 | 1 | 1 | 3 | 8 | 14335 | -486 |
| ESPORTSTMNT01_2984408 | LHE | 2023 | Blue | Wolf Club Esports | 0 | 1 | 1 | 3 | 5 | 16684 | 1129 |
| ESPORTSTMNT01_2984408 | LHE | 2023 | Red | Eclipse Gaming | 1 | 0 | 0 | 2 | 10 | 15555 | -1129 |
| ESPORTSTMNT01_2984426 | LHE | 2023 | Blue | Eclipse Gaming | 0 | 1 | 1 | 1 | 4 | 15434 | -420 |

### Univariate Analysis

For our univariate analysis, we looked at the distribution of major objectives secured by teams. This plot compares `dragons`, `towers`, and `barons`.

<iframe
  src="assets/objectives_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The graph shows that towers have the widest range because teams can destroy many towers in one game. Barons are mostly concentrated around 0 or 1 because Baron appears later in the game and is taken less often. Dragons fall in between because teams can secure multiple dragons, but not as many as towers.

This matters because each objective is measured on a different scale. For example, securing 1 Baron is not the same type of achievement as destroying 1 tower.

### Bivariate Analysis

For our bivariate analysis, we compared first-objective control with match result. Specifically, we looked at how win rate changes depending on whether a team secured `firstdragon`, `firsttower`, or `firstbaron`.

<iframe
  src="assets/first_objective_win_rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

For each objective, teams that secured the objective first generally had a higher win rate than teams that did not. This suggests that early objective control is associated with winning.

We also compared total objective control between winning and losing teams.

<iframe
  src="assets/total_objectives_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This plot shows that winning teams usually secured more total major objectives than losing teams. This supports the idea that objective control is strongly connected to match outcomes.

### Interesting Aggregates

The table below compares average objective-control statistics for winning and losing teams.

| Result | Team Rows | Avg Dragons | Avg Towers | Avg Barons | Avg First Objectives | Avg Total Major Objectives | Avg Gold Diff at 10 | Avg Kills at 10 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Loss | 35,687 | 1.38 | 2.79 | 0.22 | 0.85 | 4.39 | -659.33 | 1.87 |
| Win | 35,687 | 3.02 | 9.28 | 1.11 | 2.08 | 13.41 | 659.33 | 2.67 |

Winning teams had higher averages for every objective-control statistic. They averaged more dragons, towers, Barons, first objectives, and total major objectives. Winning teams also had a positive average gold difference at 10 minutes, while losing teams had a negative average gold difference.

This table is significant because it gives a clear summary of the relationship between objective control and winning.

## Assessment of Missingness

### NMAR Analysis

For our missingness analysis, we focused on the column `goldat10`, which represents a team's total gold at 10 minutes. This column is important because it measures early-game strength, which connects directly to our project question about early-game objective control and winning.

We do not believe `goldat10` is NMAR. The value of `goldat10` itself does not seem like the main reason it would be missing. Instead, missingness in this column is more likely related to data collection issues, league reporting differences, or whether certain professional leagues consistently recorded early-game gold statistics.

An example of a column that could be NMAR is `split`. Some rows may be missing `split` because the match was not part of a traditional spring or summer split, such as an international tournament, playoffs, or another special event. In that case, the missingness could depend on the missing value itself because some competitions may not naturally have a split label.

Additional data that could help explain this missingness would be a column describing the match type or event type, such as whether the match was part of a regular season, playoffs, promotion tournament, or international tournament. This extra information could help make the missingness easier to explain using observed columns.

### Missingness Dependency

For our missingness dependency tests, we tested whether the missingness of `goldat10` depends on other columns. We used total variation distance, or TVD, as the test statistic because the columns we compare against are categorical.

TVD measures how different two distributions are. In this case, it compares the distribution of another column when `goldat10` is missing versus when `goldat10` is not missing.

### Test 1: Missingness of `goldat10` vs. `league`

**Null Hypothesis:** The distribution of `league` is the same when `goldat10` is missing and when `goldat10` is not missing.

**Alternative Hypothesis:** The distribution of `league` is different when `goldat10` is missing and when `goldat10` is not missing.

**Test Statistic:** Total variation distance between the league distribution of rows where `goldat10` is missing and rows where `goldat10` is not missing. 

**Significance Level:** 0.05

<iframe
  src="assets/missing_league.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed TVD was about **0.8813**, and the p-value was about **0.048**. Since the p-value is below 0.05, we reject the null hypothesis. This suggests that the missingness of `goldat10` likely depends on `league`.

In other words, some leagues appear more likely to have missing early-game gold data than others.

### Test 2: Missingness of `goldat10` vs. `side`

**Null Hypothesis:** The distribution of `side` is the same when `goldat10` is missing and when `goldat10` is not missing.

**Alternative Hypothesis:** The distribution of `side` is different when `goldat10` is missing and when `goldat10` is not missing.

**Test Statistic:** Total variation distance between the side distribution of rows where `goldat10` is missing and rows where `goldat10` is not missing.

**Significance Level:** 0.05

<iframe
  src="assets/missing_side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed TVD was **0.0**, and the p-value was **1.0**. Since the p-value is greater than 0.05, we fail to reject the null hypothesis. This means we do not have evidence that the missingness of `goldat10` depends on `side`.

This makes sense because whether a team plays on blue side or red side should not affect whether early-game gold data is recorded.

### Missingness Test Summary

| Compared Column | Observed TVD | P-value | Conclusion |
|---|---:|---:|---|
| `league` | 0.8813 | 0.048 | `goldat10` missingness likely depends on `league` |
| `side` | 0.0000 | 1.000 | `goldat10` missingness does not appear to depend on `side` |

## Hypothesis Testing

For our hypothesis test, we want to determine whether teams with stronger objective control have a different win rate than teams with weaker objective control.

We define objective control using `total_major_objectives`, which is the sum of `dragons`, `towers`, and `barons`. Teams at or above the median number of total major objectives are placed in the **high objective-control** group, while teams below the median are placed in the **low objective-control** group.

**Null Hypothesis:** Teams with stronger objective control and teams with weaker objective control have the same win rate.

**Alternative Hypothesis:** Teams with stronger objective control and teams with weaker objective control have different win rates.

**Test Statistic:** Absolute difference in win rates between the high objective-control group and the low objective-control group.

\[
|\text{high objective-control win rate} - \text{low objective-control win rate}|
\]

**Significance Level:** 0.05

The high objective-control group had a win rate of **0.93**, while the low objective-control group had a win rate of **0.03**. The observed difference in win rates was about **0.903**.

To run the hypothesis test, we used a permutation test. Under the null hypothesis, objective-control group and match result are unrelated, so we shuffled the `result` column many times and recalculated the difference in win rates for each simulation.

<iframe
  src="assets/hypothesis_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The p-value was **0.0**, meaning none of the 1000 shuffled simulations produced a difference as large as the observed difference. We interpret this as **p-value < 0.001**.

Since the p-value is below our significance level of 0.05, we reject the null hypothesis. This suggests that win rate differs between teams with stronger and weaker objective control. In other words, teams with stronger objective control are much more likely to win.

## Framing a Prediction Problem

Our prediction problem is:

**Can we predict whether a professional League of Legends team wins using objective-control and early-game statistics?**

The response variable is `result`, where `1` means the team won and `0` means the team lost. This is a **binary classification** problem because there are only two possible outcomes: win or loss.

For this prediction problem, we are using match statistics that would be available after objective-control and early-game statistics have been recorded. These include columns such as `dragons`, `towers`, `barons`, `firstdragon`, `firsttower`, `firstbaron`, `golddiffat10`, `xpdiffat10`, `csdiffat10`, `killsat10`, and `deathsat10`.

We chose these features because they are directly related to our project question. Objective-control columns help measure how much control a team had over major map objectives, while early-game statistics help measure whether a team had an early advantage.

We will use **accuracy** to evaluate our model because the response variable is balanced. In our cleaned dataset, 50% of team rows are wins and 50% are losses, so accuracy is an appropriate metric for measuring overall prediction performance.

## Baseline Model

For our baseline model, we predicted `result` using only two features: `firstdragon` and `firsttower`.

Both features are **quantitative binary features** because they are already represented as 0 or 1. A value of 1 means the team secured that first objective, and a value of 0 means the team did not. Since these columns are already numeric, we did not need to apply any additional encoding.

We used a decision tree classifier for the baseline model because our response variable, `result`, is binary. The model predicts whether a team won or lost.

The baseline model was trained using an sklearn Pipeline. We split the data into training and test sets, then fit the decision tree on the training data and evaluated it using accuracy.

| Dataset | Accuracy |
|---|---:|
| Training | 0.70 |
| Test | 0.70 |

The baseline model had a training accuracy of **0.70** and a test accuracy of **0.70**. Since the training and test accuracies are the same, the model does not appear to be overfitting.

However, this baseline model is only moderately strong. It performs better than random guessing, but it only uses `firstdragon` and `firsttower`, so it misses other important objective-control information such as total dragons, towers, Barons, and early-game gold difference.


## Final Model

For our final model, we used a decision tree classifier again, but we added more informative features related to objective control and early-game advantage.

The baseline model only used `firstdragon` and `firsttower`. For the final model, we kept these two features and added three engineered features:

- `first_objectives_secured`: the total number of first objectives secured by a team, calculated using `firstdragon`, `firsttower`, and `firstbaron`.
- `total_major_objectives`: the total number of major objectives secured by a team, calculated using `dragons`, `towers`, and `barons`.
- `early_gold_lead`: whether the team had a positive gold difference at 10 minutes.

These features are useful because they summarize objective control more completely than the baseline model. Instead of only checking whether a team secured the first dragon or first tower, the final model also considers overall objective control and early gold advantage.

All of the features used in the final model are quantitative features. Some are binary, such as `firstdragon`, `firsttower`, and `early_gold_lead`, while others are counts, such as `first_objectives_secured` and `total_major_objectives`.

We used a `FunctionTransformer` inside an sklearn Pipeline to create the engineered features, then trained a `DecisionTreeClassifier`. We tuned the `max_depth` hyperparameter by testing different depths from 1 to 10 and selecting the smallest depth with the best validation accuracy. The best depth was **6**.

| Dataset | Baseline Accuracy | Final Model Accuracy |
|---|---:|---:|
| Training | 0.70 | 0.95 |
| Test | 0.70 | 0.95 |

The final model improved from **0.70** test accuracy to **0.95** test accuracy. This is a large improvement over the baseline model.

The training and test accuracies were both 0.95, so the final model does not show obvious overfitting. Overall, the final model performs much better because it uses stronger features that capture both objective control and early-game advantage.
