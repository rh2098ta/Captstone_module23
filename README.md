# Captstone_module23
# F1 Podium Probability — Module 23 (EDA + Baseline)

## Overview
This project explores whether we can predict the probability that a Formula 1 driver will finish on the **podium (top 3)** before a race starts. In Module 23, I focused on **data cleaning, exploratory data analysis (EDA), and a simple baseline model**. A fuller, polished analysis (extra models, interpretation, and presentation for non-technical audiences) will follow and uplaoded during/completion of Module 24.

## Research Question
**Can we estimate a driver's podium probability using only information available *before* the race (e.g., grid position, driver/team form, season/circuit info)?**

## Why this matters
Pre-race probabilities help teams plan strategy (qualifying focus, pit windows, driver selection) and give sponsors realistic expectations for race outcomes.

## Data
- **Source:** Kaggle – *Formula 1 World Championship (1950–2024)* by Rohan Rao.
- - **Core tables used:** `races`, `results`, `drivers`, `constructors` (optionally `qualifying` later).
> **Note:** I intentionally **exclude in-race or post-race information** (like pit stop durations or lap times) from features to avoid data leakage in the baseline.

## Leakage Policy (important)
Module 23, features use only **pre-race** information:
- **Allowed:** grid position, season year, driver’s historical starts & podiums **before** this race, team’s historical starts & podiums **before** this race.
- **Not allowed (baseline):** anything only known during/after the race (pit stops, lap times, final position, etc.).

## Method (Module 23)
1. **Load & clean** core tables (join by `raceId`, `driverId`, `constructorId`).
2. **Target:** `podium = 1` if `positionOrder <= 3`, else `0`.
3. **Feature engineering (pre-race only):**
   - `grid`, `year`
   - `driver_starts_so_far`, `driver_podiums_so_far`, `driver_podium_rate_so_far`
   - `team_starts_so_far`, `team_podiums_so_far`, `team_podium_rate_so_far`
   - Implementation ensures “so far” uses **shift(1)** and cumulative counts (no future info).
4. **EDA visuals (examples):**
   - Podium rate vs. starting grid (1 = pole)
   - Podium rate vs. binned driver experience/form
   - Podium rate vs. binned team momentum
5. **Baseline model:** Logistic Regression  
   - **Time-aware split**: train on earlier seasons, test on later seasons (more realistic than random split).
6. **Evaluation:**
   - **ROC-AUC** and **accuracy**
   - **Confusion matrix**
   - **Per-race “top-3 hit-rate”**: among each race’s top-3 predicted drivers, how many actually podiumed?

## Results (Module 23)
Replace the blanks with your outputs from the notebook:

- **ROC-AUC:** 0.918
- **Accuracy:** 0.892
- **Confusion matrix:**  [[4099  204]
 [ 339  402]]

## Findings so far 
- **Starting grid matters**: podium probability **decreases steadily** as grid position number increases.  
- **Recent form matters**: both driver and team **“so far” podium rates** add predictive signal.  
- **Baseline ranks contenders reasonably well**: even a simple Logistic Regression gets a useful **top-3 per-race** hit-rate.
