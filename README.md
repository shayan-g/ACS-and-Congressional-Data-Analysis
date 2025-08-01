# ACS & Congressional Data Analysis

*A Jupyter-Notebook investigation of how **education** and **political-party affiliation** relate to very-high household income in U.S. Congressional districts (ACS 2021).*

---

## Research Question

> **Does the larger share of \$200 K-plus households in Democratic-held districts stem from those districts having a more highly educated population—or does the gap persist after we control for education?**

| Hypothesis | Statement |
| ---------- | --------- |
| **Null (H₀)** | Democratic districts have a higher share of \$200 K+ households **regardless** of bachelor-degree attainment. |
| **Alternative (H₁)** | The higher \$200 K+ share in Democratic districts is **explained by** their higher bachelor-degree attainment. |

---

## Summary of Findings

| Metric | Democratic&nbsp;(mean) | Republican&nbsp;(mean) | Significance |
| ------ | --------------------- | ---------------------- | ------------ |
| % Households ≥ \$200 K | **11.8 %** | 7.3 % | *p* < 0.001 |
| % Population (25+) w/ 4-yr degree | **22.5 %** | 19.2 % | *p* < 0.001 |
| **OLS (dependent: % ≥ \$200 K)** | β̂ = 0.82 for degree %, β̂ = 1.82 for party dummy (D) | *R² = 0.64* | both *p* < 0.001 |

> Even after controlling for education, Democratic districts average **≈ 1.8 percentage-points** more \$200 K+ households, hinting at additional explanatory factors (industry mix, urbanization, cost of living, etc.).  
> See the notebook for full diagnostics, VIF scores, and robustness checks.

---

## Methodology

1. **Acquisition**

   * Pull household-income distribution, bachelor’s totals, and age-25+ denominators via the Census API.
   * Scrape the party roster once and save as CSV.

2. **Transformation**

   ```python
   # Key engineered metrics
   df["pct_high_income"]   = 100 * df["$200k+"]/ df["Total Households"]
   df["pct_bachelor_plus"] = 100 * df["4 Year Degree"] / df["Pop_25_plus"]
   ```

   * Strip district strings and normalize codes.
   * Convert all relevant columns to numeric types.
   * Join on `State` & `district number`.

3. **Exploratory Analysis**

   * Box/violin plots for income & education by party.
   * Compute Pearson correlation (≈ 0.79 between high-income % and degree %).

4. **Statistical Testing**

   * Welch two-sample *t*-tests for party differences.
   * OLS regression model:
     ```
     % ≥ $200 K ~ pct_bachelor_plus + party_dummy
     ```
   * Check Variance Inflation Factor (VIF < 1.1 → low multicollinearity, though we are suspicious of this result)

---

## Limitations & Next Steps

* **Cost-of-living adjustments:** \$200 K in NYC ≠ \$200 K in rural areas.
* **Additional covariates:** Include industry mix, urbanization, race/ethnicity, age structure, unionization.
* **Time-series extension:** Expand to ACS 2010–2024 to assess stability.

Contributions—new features, bug fixes, deeper econometric approaches—are very welcome via pull request!
