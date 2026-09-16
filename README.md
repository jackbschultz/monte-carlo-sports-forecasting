# Monte Carlo Sports Forecasting

> Probabilistic sports forecasting system using NFL play-by-play data,
> Monte Carlo simulation, probability calibration, leakage-safe backtesting,
> and fair-odds estimation.

**Status:** Active development and validation

This project explores probabilistic sports forecasting through simulation rather
than simply predicting winners. Historical play-by-play data is transformed into
team-level performance distributions, which are used to simulate NFL games and
first halves thousands to millions of times.

Model versions are evaluated out-of-sample and are promoted only when they improve
the metrics appropriate to the market being modeled.

---

## Current Models

### NFL Moneyline Model

**Current benchmark: V2.0**  
**Development branch: V2.4**

The moneyline system estimates offensive and defensive drive-outcome probabilities
and combines them into matchup-specific distributions for:

- Touchdowns
- Field goals
- Turnovers
- Empty drives

Monte Carlo simulation produces game-level score and win distributions. A
leakage-safe rolling logistic calibration layer then converts raw simulated win
probabilities into calibrated probabilities and fair American odds.

#### V2.0 Historical Validation

| Metric | V1.0 Raw | V2.0 Calibrated |
|---|---:|---:|
| Evaluation games | 1,355 | 1,355 |
| Brier Score | 0.22940 | **0.22786** |
| Log Loss | 0.65078 | **0.64816** |
| Calibration Error (ECE) | 6.70% | **4.59%** |
| Winner Accuracy | 63.91% | 63.39% |

V2.0 remains the benchmark because the project prioritizes probability quality
and calibration over classification accuracy alone.

Later experiments involving team pace, field position, and richer turnover
modeling were rejected after failing to improve the primary probability metrics.

---

### NFL First-Half Totals Model

**Current benchmark: V4.1**  
**Current challenger: V4.2**

This model estimates full first-half scoring distributions rather than a single
projected score.

V4.1 incorporates:

- Prior/current-season team shrinkage
- First-half pace
- First-half points per drive
- Offensive and defensive drive outcomes
- Historical drive-count variance
- 6-, 7-, and 8-point touchdown outcomes
- Non-offensive and special-teams scoring

The resulting Monte Carlo distribution can estimate probabilities such as:

`P(First-Half Total < 24.5)`

Those probabilities can then be converted into fair no-vig prices and compared
with sportsbook market prices.

**2023–2025 pooled calibration ECE: 2.68%**

V4.2 experiments with a stateful turnover-to-field-position mechanism and remains
a challenger until it passes the same out-of-sample validation process.

---

## Modeling Philosophy

This project uses a champion/challenger development process.

A feature is not retained simply because it is theoretically useful. Every
change must demonstrate value under leakage-safe historical testing.

Primary evaluation metrics include:

- Brier Score
- Log Loss
- Expected Calibration Error
- Mean Absolute Error
- Winner Accuracy
- Score and margin error

Rejected experiments remain documented so that unsuccessful research is preserved
rather than silently incorporated into later versions.

---

## Technology

- Python
- NumPy
- Pandas
- Polars
- scikit-learn
- nflreadpy
- Monte Carlo simulation
- Logistic / Platt probability calibration

---

## Long-Term Direction

The goal is to develop a modular multi-sport forecasting framework with shared
infrastructure for:

- Monte Carlo simulation
- Probability calibration
- Historical backtesting
- Vig removal
- Fair-price estimation
- Expected-value analysis
- Closing-line-value tracking
- Model versioning and experiment logging

> This repository is currently under active development. Model architecture,
> experiments, and validation methodology may change as research continues.
