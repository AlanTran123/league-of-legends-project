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

PASTE YOUR REAL `teams_clean[display_cols].head().to_markdown(index=False)` TABLE HERE.

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
