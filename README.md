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
