# Predicting Restaurant Health Code Failures: A Discrete-Time Survival Analysis

Discrete-time survival analysis of Seattle restaurant health inspections,
modeling time-to-first-failure as a function of violation severity and
inspection history.

## Question

What predicts *when* a restaurant first receives an unsatisfactory health
inspection result — and how much does violation severity alone tell us
about that risk over time?

## Data

[Seattle Food Establishment Inspection Data](https://data.seattle.gov/) —
a public dataset of routine and complaint-driven health inspections for
Seattle restaurants, including inspection results and violation point
totals. (Raw data isn't committed to this repo — see `data/README.md` for
how to obtain it.)

## Approach

- Defined **failure** as a restaurant's first "Unsatisfactory" inspection
  result.
- Restricted to **routine inspections in Seattle** so restaurants are
  compared on a consistent inspection cadence.
- Excluded restaurants whose *first observed* inspection was already a
  failure (left-censored — their risk before entering the dataset is
  unknown), and stopped tracking a restaurant once it recorded its first
  failure (it's no longer "at risk" for a *first* failure after that).
- Modeled the hazard of first failure using **discrete-time logistic
  regression**, with each restaurant-inspection-period as one observation.

## Key findings

| Predictor | Coefficient | p-value | Interpretation |
|---|---|---|---|
| Violation points | 0.39 | < .001 | Each additional violation point meaningfully raises the odds of failing that period |
| Time since entry | 0.07 | < .001 | Longer-tenured restaurants have a slightly higher hazard of first failure |
| Month (time trend) | 0.0009 | .143 | No significant broad trend over the observation window |

- Pseudo R² of 0.54 — violation points and inspection tenure explain a
  substantial share of when restaurants first fail.
- Only ~25% of restaurants in the dataset never received an unsatisfactory
  result during the observation window.
- ~2,769 restaurants were excluded as left-censored (failed on their first
  observed inspection).

## What I'd explore next

- Whether **prior** violation history (not just the current inspection)
  predicts first failure
- A Cox proportional hazards model as a robustness check against the
  discrete-time logistic approach
- Restaurant-level covariates (cuisine type, neighborhood, chain vs.
  independent)

## Tools

Python · pandas · statsmodels · matplotlib

## Running this notebook

```bash
pip install -r requirements.txt
jupyter notebook restaurant_inspection_survival_analysis.ipynb
```

See `data/README.md` for how to get the underlying dataset.
