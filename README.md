# DATASCI-3000-Project - NFL Performance & Salary Analysis
This repository contains machine learning workflows designed to analyze NFL data at both the player and team levels. The project aims to predict key metrics such as player salaries, future performance, team win percentages, and playoff appearances using various statistical models and feature selection techniques.

## Group Members
- David Gibb
- Clayton McCormack
- Blake Weigel-Dumas
- Aiden Paleczny

## Dataset links
```
https://www.kaggle.com/datasets/philiphyde1/nfl-stats-1999-2022?select=weekly_player_stats_oNense.csv

https://www.kaggle.com/datasets/f4k25g/nfl-salaries

https://www.kaggle.com/datasets/lukebukowski/nfl-salary-cap-spending-2013-2022
```

## Repository Contents
1. Player_Analysis.ipynb
- This notebook focuses on individual player evaluation, segmented by position (QB, RB, WR).

    - Objectives:

        - Salary Prediction: Predict a player's Salary Cap Percentage (cap_percent) based on performance metrics.

        - Future Performance: Predict a player's key statistic for the following year (e.g., passer_rating_next for QBs).

    - Methodology:

        - Data Cleaning: Removes highly correlated features and redundancy (e.g., removing career_ stats to focus on season stats).

        - Modeling: Implements LinearRegression, Ridge, RandomForestRegressor, and GradientBoostingRegressor.

        - Evaluation: Compares models using R², RMSE, and MAE.

        - Feature Importance: Visualizes coefficients (Linear/Ridge) and feature importance (Tree-based models) to determine what drives salary and future success.

2. Team_Analysis.ipynb
- This notebook performs a high-level analysis of team success factors.

    - Objectives:

        - Regression Targets: Predict win_pct, total_points_scored (Offense), and total_points_allowed (Defense).

        - Classification Target: Predict Playoffs status (Yes/No).

    - Methodology:

        - Feature Engineering: Compares model performance using three distinct feature sets:

            1. Salary Only: Can money buy wins?

            2. Non-Salary: Performance statistics only.

            3. Combined: Both financial and performance data.

        - Feature Selection: Systematically tests multiple selection methods to optimize model input:

            - Variance Threshold

            - SelectKBest

            - Recursive Feature Elimination (RFE)

            - LASSO (L1 Regularization)

            - Random Forest Importance

    - Outputs: Exports correlation matrices and comparison tables identifying the best feature selection method for each target variable.
    
3. merge_player_data.ipynb
- This notebook is a data processing pipeline that joins individual player performance statistics with their financial (salary cap) data.

- Inputs: It reads raw player stats (yearly_player_stats_offense.csv, yearly_player_stats_defense.csv) and a salary table (2014-thru-2020-cap-tables.csv).

- Cleaning:

    - It standardizes player names (e.g., converting to uppercase, removing punctuation) to ensure matches between the stats database and the salary database.

    - It maps "slang" or full team names (e.g., "seattle-seahawks") to official NFL abbreviations (e.g., "SEA").

    - Merging: It performs a Left Merge to attach salary info (cap_hit, cap_percent) to the performance stats based on Player Name, Team, Season, and Position.

- Outputs: It saves four processed CSV files to ../data/processed/:

    - offense_players_salary_only.csv & defense_players_salary_only.csv (Clean data for modeling).

    - offense_players_all.csv & defense_players_all.csv (Full datasets including players without salary info).

4. merge_team_data.ipynb
- This notebook prepares the team-level dataset by unifying offensive stats, defensive stats, and positional spending distributions.

- Inputs: It reads yearly and weekly team stats (yearly_team_stats_offense.csv, etc.) and a positional salary spreadsheet (NFL Salary By Position Group.xlsx).

- Normalization: It filters data to include only the Regular Season (REG) and standardizes team names (e.g., mapping "ARI" to "Cardinals").

- Conflict Resolution: It renames overlapping column names to distinguish between offensive and defensive metrics (e.g., renaming fumble to fumble_off and fumble_def).

- Feature Engineering: It calculates a critical missing metric, total_points_allowed, by aggregating weekly defensive data, as this was missing from the yearly summary.

- Merging: It merges the offense, defense, and salary datasets into a single master file.

- Output: It saves the final dataset used for team analysis to ../data/processed/team_season_merged_corrected.csv.

## Data Processing
The notebooks assume the existence of a data/processed/ directory containing:

- offense_players_salary_only.csv

- defense_players_salary_only.csv

- team_season_merged_corrected.csv

Data preprocessing steps include:

- Correlation Analysis: Automatic removal of features with >0.97 correlation to prevent multicollinearity.

- Leakage Removal: Stripping columns that directly reveal the target (e.g., removing wins when predicting win_pct).

- Aggregation: Preferring per-game averages over raw season totals to account for missed games.

## Requirements
To run these notebooks, you will need the following Python libraries:

```
pandas
numpy
matplotlib
seaborn
scikit-learn
```

## Usage
1. Ensure your processed data files are located in the ../data/processed/ directory relative to the notebooks.

2. Run Player_Analysis.ipynb to generate position-specific model summaries.

3. Run Team_Analysis.ipynb to generate feature selection comparisons and team-level predictions.

4. Outputs (graphs and CSV summaries) are saved to the model_outputs/ directory.