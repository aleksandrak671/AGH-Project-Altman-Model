# AGH Project Altman Model

University project applying Altman's Z-score model to Polish company bankruptcy data, with a comparison against a locally fitted LDA model.

## Dataset

Polish Companies Bankruptcy Data (UCI ML Repository) — `5year` file.  
5910 firms, 66 variables. Heavily imbalanced: 6.9% bankrupts (410 firms).  
A balanced sample of 100 bankrupts and 100 surviving firms was drawn for evaluation.

## Method

The original Altman (1968) formula was applied:

```
Z = 1.2*X1 + 1.4*X2 + 3.3*X3 + 0.6*X4 + 0.99*X5
```

| Variable | Definition | Dataset column |
|----------|-----------|----------------|
| X1 | Working capital / Total assets | Attr3 |
| X2 | Retained earnings / Total assets | Attr6 |
| X3 | EBIT / Total assets | Attr7 |
| X4 | Book equity / Total liabilities | Attr8 |
| X5 | Sales / Total assets | Attr9 |

**Note:** X4 uses book value instead of market value (original model requires listed companies).

Classification thresholds: Z < 1.8 = bankrupt, Z > 3.0 = safe, 1.8–3.0 = grey zone.

## Results

| Metric | Value |
|--------|-------|
| Classified firms (out of 200) | 162 |
| Grey zone (no decision) | 38 |
| Accuracy (on classified) | 72.2% |
| Sensitivity (bankrupts detected) | 71.3% |
| Specificity (survivors detected) | 73.2% |
| Type I error (bankrupt misclassified as safe) | 28.8% |
| Type II error (survivor misclassified as bankrupt) | 26.8% |

A locally fitted LDA on the same sample achieved only 64.5% accuracy, likely due to the small training set (200 firms).

## Key Takeaways

- The model performs below its original reported accuracy, expected given it was built on US firms from the 1960s with different accounting standards.
- Type I error of ~29% is high from a credit risk perspective — nearly 1 in 3 bankrupts passes as safe.
- The grey zone accounts for 19% of firms, leaving a significant share without a clear verdict.
- Altman's Z-score can serve as an early warning signal but should not be used as a standalone decision tool for Polish firms.

## Requirements

R with the `MASS` package.
