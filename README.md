# Bankruptcy Prediction — Altman Z-Score Model

AGH University project for the course **Credit and Operational Risk Methods**.  
Application of the **Altman Z-score model** to predict bankruptcy of Polish companies, with a comparison against a locally fitted **Linear Discriminant Analysis (LDA)**.

## Overview

The project applies the original Altman (1968) discriminant function to a sample of Polish firms and evaluates how well a model built on 1960s American data transfers to a different economic context. The key finding is that a locally re-estimated LDA outperforms the original Altman model when both are compared fairly on the same full sample.

## Data

**Polish Companies Bankruptcy Dataset** — UCI Machine Learning Repository  
[https://archive.ics.uci.edu/dataset/365/polish+companies+bankruptcy+data](https://archive.ics.uci.edu/dataset/365/polish+companies+bankruptcy+data)

File used: `5year` — financial ratios from year 5 of the forecasting period, with a binary label indicating bankruptcy after 1 year (5,910 firms, 66 variables).

The dataset is heavily imbalanced: bankruptcies account for only 6.9% of observations. A balanced sample of **100 bankrupt + 100 surviving firms** was drawn for analysis.

## Altman Z-Score Formula

$$Z = 1.2\,X_1 + 1.4\,X_2 + 3.3\,X_3 + 0.6\,X_4 + 0.99\,X_5$$

| Variable | Definition | Column in dataset |
|----------|------------|-------------------|
| X₁ | Working capital / Total assets | Attr3 |
| X₂ | Retained earnings / Total assets | Attr6 |
| X₃ | EBIT / Total assets | Attr7 |
| X₄ | Book equity / Total liabilities* | Attr8 |
| X₅ | Sales / Total assets | Attr9 |

*Original model uses market value of equity; book value used here as the dataset contains no market data.

**Classification zones:** Z > 3.0 → safe, 1.8 < Z < 3.0 → grey zone, Z < 1.8 → distress.

## Results

| Model | Sample | Accuracy |
|-------|--------|----------|
| Altman (excluding grey zone) | 162 / 200 firms | 72.2% |
| Altman (grey zone = misclassification) | 200 / 200 firms | 58.5% |
| LDA fitted on Polish data | 200 / 200 firms | 64.5% |

38 firms (19%) fell into the grey zone and received no decision from the Altman model. When evaluated fairly on the full 200-firm sample — treating the grey zone as a failed classification, since a bank must always make a decision — the locally fitted LDA (64.5%) outperforms the original Altman model (58.5%). This confirms that the original weights estimated on 1960s American firms do not transfer well to Polish data.

Type I error (bankrupt classified as safe): **28.8%** — nearly one in three bankruptcies was missed.

## Files

- `Model_Altmana.Rmd` — R Markdown source file
- `Model_Altmana.html` — compiled report
- `csv_result-5year.csv` — input dataset (download from UCI link above)

## Requirements

R packages:
```r
install.packages(c("MASS", "knitr", "rmarkdown"))
```

## Notes

The comparison between Altman and LDA is intentionally done on the **same full sample of 200 firms** to avoid the bias that arises when the grey zone is excluded, a methodological point flagged during the project review.
