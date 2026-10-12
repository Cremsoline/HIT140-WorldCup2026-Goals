# HIT140 – Linear Regression 2.2: Goals Scored per Team per Match (FIFA World Cup 2026)

Charles Darwin University · HIT140 Foundations of Data Science · Assessment 3
Team members: Bijay and Susant

## Aim
Predict the number of goals a team scores in a single FIFA World Cup 2026 match, using only information available **before** the tournament started (11 June 2026).

## Dataset
- **208 rows**: one row per team per match (104 matches × 2 teams)
- **Target:** `goals`, the goals scored by the team in that match
- **8 explanatory variables:**

| # | Variable | Type | Description | Source |
|---|---|---|---|---|
| 1 | team_fifa_points | Shared | FIFA ranking points | FIFA/Coca-Cola Men's World Ranking, 11 June 2026 edition |
| 2 | team_market_value | Shared | Squad market value (€ millions) | Transfermarkt participants page, Wayback Machine snapshot 12 June 2026 |
| 3 | team_goals_pg_last10 | Shared | Goals per game, last 10 matches | results.csv |
| 4 | rest_days | Shared | Days since previous match | results.csv |
| 5 | opp_conceded_pg10 | Unique | Opponent's goals conceded per game, last 10 matches | results.csv |
| 6 | opp_gpg_competitive | Unique | Opponent's goals per competitive game since Dec 2022 | results.csv |
| 7 | opp_wc_apps_since1998 | Unique | Opponent's World Cup appearances 1998–2022 | results.csv |
| 8 | team_fail_to_score_rate | Unique | Share of matches since Dec 2022 with 0 goals | results.csv |

`results.csv` is the International Football Results dataset (Mart Jürisoo, github.com/martj42/international_results). All match-history variables use only matches played before 11 June 2026, to avoid data leakage.

## Repository structure

| Folder | Contents |
|---|---|
| `data/raw/` | Original data and saved source pages (FIFA ranking, Transfermarkt) |
| `data/processed/` | `reg_2_2_dataset.csv`, the final 208-row modelling dataset |
| `notebooks/` | `reg_2_2_goals.ipynb`, all code: preparation, EDA, model, evaluation |
| `outputs/figures/` | Charts used in the report |
| `outputs/model_results/` | Test metrics, cross-validation, coefficients, VIF |

## Method
1. Built the 208-row team-match dataset from `results.csv`
2. Calculated pre-tournament variables; joined FIFA points and market values
3. EDA: goal distribution, correlations, multicollinearity (VIF)
4. Ordinary least squares linear regression (scikit-learn and statsmodels)
5. Evaluation: 80/20 train-test split and 5-fold cross-validation, both grouped by match so the two rows of a match are never split

## Results (5-fold cross-validation, mean)

| MAE | RMSE | R² |
|---|---|---|
| 0.91 goals | 1.21 | 0.23 |

The model outperformed a mean-only baseline. Significant predictors (p < 0.05): opponent World Cup experience (−), squad market value (+) and opponent goals conceded in the last 10 matches (+).

## Limitations
- Goals are whole, non-negative and right-skewed, so residuals are not normal; a Poisson regression could be explored
- FIFA points and market value are strongly correlated (r = 0.81), although all VIFs are below 5
- The market value snapshot was captured one day into the tournament (only two matches played)

## How to run
1. Install Python with pandas, numpy, matplotlib, scikit-learn and statsmodels
2. Open `notebooks/reg_2_2_goals.ipynb` in Jupyter
3. Click **Run → Run All Cells**
