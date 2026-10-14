# A/B Test Analysis: Cookie Cats

Does moving the first progression gate from level 30 to level 40 affect player retention in the mobile puzzle game *Cookie Cats*?

## TL;DR

| | |
|---|---|
| **Question** | Does gate placement (level 30 vs. 40) change player retention? |
| **Primary metric** | 7-day retention |
| **Result** | 7-day retention fell from **19.02%** (gate_30) to **18.20%** (gate_40): **-0.82 pp** (95% CI: -1.33 to -0.31 pp, p = 0.0016) |
| **Significance** | Holds after Bonferroni correction (adjusted α = 0.0167); confirmed by z-test, chi-square and bootstrap |
| **Recommendation** | **Keep the gate at level 30** |
| **Main caveat** | Mild/borderline Sample Ratio Mismatch (p = 0.0086, above the common p < 0.001 alert threshold); no link to `userid` found |

## Key findings

- **7-day retention:** statistically significant drop with gate_40, but small in relative terms (-4.3% relative lift, Cohen's h = -0.021). The minimum detectable effect at 80% power is h ≈ 0.0187, so the observed effect sits just above what the experiment could reliably detect, and the CI is fairly wide.
- **1-day retention:** same direction (44.82% vs. 44.23%) but not significant after correction (p = 0.074).
- **Rounds played:** no evidence of a difference (Mann-Whitney p = 0.051, borderline), so the drop appears tied to players coming back, not to overall engagement.
- **SRM investigation:** the group split (49.56% / 50.44%) is mildly off 50/50. I checked whether the imbalance concentrates in a `userid` range (decile test, p = 0.605) or in odd/even ids (p = 0.122) and found no evidence for either. The notebook lists a concrete next step (testing the platform's real bucketing key).

## What this project demonstrates

- Fixing hypotheses, metrics and α in an analysis plan **before** running the tests
- Data-quality checks: duplicates, outliers, **Sample Ratio Mismatch**
- Defensible outlier handling (keeping legitimate heavy users, removing one implausible record)
- Two-proportion z-test and chi-square for binary metrics; Welch's t-test (log scale), Mann-Whitney and bootstrap for a skewed continuous metric
- Going beyond p-values: confidence intervals, relative lift, effect size (Cohen's h), minimum detectable effect
- Multiple-comparison correction (Bonferroni)
- A clear business recommendation with explicit limitations

## Repository contents

```
.
├── cookie_cats_ab_test.ipynb   # full analysis (outputs included)
├── requirements.txt
└── README.md
```

## How to reproduce

1. Download the dataset from Kaggle ("Mobile Games A/B Testing - Cookie Cats") and save it as `cookie_cats.csv` in the project folder.
2. Install dependencies and open the notebook:

```bash
pip install -r requirements.txt
jupyter notebook cookie_cats_ab_test.ipynb
```

In Google Colab, the first data cell prompts you to upload the CSV.

## Limitations

- Only 7 days of retention; long-term retention and revenue impact are unknown.
- Mild SRM present; the bucketing key used by the experimentation platform is not in the data.
- No segmentation (platform, country, new vs. veteran players).

## Tools

Python, pandas, NumPy, SciPy, statsmodels, Matplotlib
