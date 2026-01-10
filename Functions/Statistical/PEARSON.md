---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- correlation
- linear-relationship
- regression-analysis
- bivariate-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Pearson Correlation Coefficient
- Pearson r
- Product-Moment Correlation
tags:
- statistical
- correlation
- pearson
- relationship
- bivariate
---

# PEARSON

## Description

**PEARSON** calculates the Pearson product-moment correlation coefficient between two datasets, measuring the strength and direction of the linear relationship between two variables. Named after statistician Karl Pearson, this coefficient (denoted as r) ranges from -1 to +1: +1 indicates perfect positive correlation (variables move together exactly), -1 indicates perfect negative correlation (variables move in opposite directions exactly), and 0 indicates no linear relationship.

PEARSON is **mathematically identical to CORREL**. Both functions compute the same Pearson correlation coefficient using the same formula: r = Cov(X,Y) / (StdDev_X * StdDev_Y). The existence of two functions with identical behavior is simply a matter of function naming conventions. Use whichever name feels more natural for your context. Some users prefer PEARSON because it explicitly names the statistic being calculated; others prefer CORREL for its brevity.

The Pearson correlation measures linear association specifically. A correlation of 0 means no linear relationship, but non-linear relationships (U-shaped, curved) can exist between variables with zero Pearson correlation. Always visualize your data with scatter plots before interpreting correlation. Anscombe's Quartet famously demonstrates how very different datasets can produce identical correlation values, making visualization essential.

**Correlation does not imply causation.** This fundamental statistical principle cannot be overstated. A high correlation between variables X and Y does not mean X causes Y. Both might be caused by an unobserved third variable (confounding), the relationship might be coincidental, or Y might cause X rather than the reverse. Establishing causation requires controlled experiments, temporal analysis, or sophisticated causal inference methods beyond simple correlation calculation.

## Syntax

> [!f(x)] PEARSON Syntax
>
> ```
> =PEARSON(array1, array2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first dataset. Range or array of numeric values. |
| `array2` | Yes | The second dataset. Must have the same number of data points as array1. Each position corresponds to a paired observation. |

### Return Value

Returns a numeric value between -1 and 1 representing the Pearson correlation coefficient. Returns #N/A if arrays have different numbers of numeric data points. Returns #DIV/0! if either array has zero variance.

## Examples

> [!f(x)] PEARSON Examples

### Example 1: Strong Positive Correlation
```
=PEARSON(A2:A11, B2:B11)
```
Where A2:A11 contains height data and B2:B11 contains corresponding weight data

**Result:** 0.85 (example value)

**Explanation:** A correlation of 0.85 indicates a strong positive linear relationship. Taller individuals tend to weigh more. Squaring this (0.72) shows 72% of weight variance is linearly associated with height.

---

### Example 2: Strong Negative Correlation
```
=PEARSON(Price, Demand)
```
**Result:** -0.78 (example value)

**Explanation:** Negative correlation shows that as price increases, demand tends to decrease. The strength (0.78) indicates a strong but not perfect relationship.

---

### Example 3: No Correlation
```
=PEARSON(RandomX, RandomY)
```
**Result:** Approximately 0 (within -0.1 to 0.1 for random data)

**Explanation:** Random, unrelated variables show correlation near zero. With small samples, random data can show spurious correlations by chance.

---

### Example 4: Comparison with CORREL
```
PEARSON: =PEARSON(A2:A100, B2:B100)
CORREL: =CORREL(A2:A100, B2:B100)
```
**Result:** Both return identical values

**Explanation:** PEARSON and CORREL are exactly the same function with different names. Use whichever you prefer.

---

### Example 5: Perfect Correlation
```
=PEARSON(X, 2*X + 5)
```
Where the second array is a linear transformation of the first

**Result:** 1.00

**Explanation:** Any perfect linear relationship produces correlation of exactly 1 (positive slope) or -1 (negative slope).

---

### Example 6: Converting to R-Squared
```
Correlation: =PEARSON(X, Y)
R-squared: =PEARSON(X, Y)^2
Verification: =RSQ(Y, X)
```
**Result:** R-squared equals PEARSON squared

**Explanation:** The coefficient of determination (R-squared) equals correlation squared. A correlation of 0.7 means R-squared of 0.49 (only 49% of variance explained).

---

### Example 7: Testing for Significance
```
r: =PEARSON(X, Y)
n: =COUNT(X)
t_stat: =r * SQRT((n-2)/(1-r^2))
p_value: =T.DIST.2T(ABS(t_stat), n-2)
```
**Result:** P-value for testing if correlation differs from zero

**Explanation:** Small p-values indicate the observed correlation is unlikely to occur by chance if the true correlation is zero.

---

### Example 8: Correlation Matrix Entry
```
=PEARSON(INDEX(Data,,1), INDEX(Data,,2))
```
**Result:** One cell of a correlation matrix

**Explanation:** Building a correlation matrix requires PEARSON (or CORREL) for each variable pair. Use dynamic references for systematic matrix construction.

---

### Example 9: Asset Return Correlation
```
=PEARSON(StockA_Returns, StockB_Returns)
```
**Result:** 0.45 (example value)

**Explanation:** In portfolio management, correlation between asset returns determines diversification benefit. Lower correlation provides more risk reduction.

---

### Example 10: Handling Missing Data
```
=PEARSON(A2:A20, B2:B20)
```
Where some cells contain blanks or text

**Result:** Correlation calculated from complete pairs only

**Explanation:** Pairs where either value is non-numeric are excluded from the calculation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Arrays have different numbers of numeric data points | Ensure both ranges have equal length and corresponding cells contain numbers |
| `#DIV/0!` | One or both arrays have zero variance (all identical values) | Correlation is undefined when there is no variation |
| `#VALUE!` | Non-numeric text provided directly as argument | Use cell references; direct text arguments cause errors |
| `Result = 0 but relationship exists` | Non-linear relationship present | Plot data; consider Spearman correlation for non-linear monotonic relationships |
| `Unexpected sign` | Data order mismatch | Verify array1[i] corresponds to array2[i] for all positions |

## Understanding Correlation vs. Causation

### Why Correlation is Not Causation

Finding a correlation between X and Y tells you they vary together but not why. Several possibilities exist:

1. **X causes Y:** The assumed interpretation, but rarely provable from correlation alone
2. **Y causes X:** Reverse causation (e.g., companies spend more on advertising because they have higher sales, not just the reverse)
3. **Z causes both X and Y:** Confounding variable (ice cream sales and drowning correlate because both are caused by summer)
4. **Coincidence:** With enough variables, random correlations appear by chance

### When Can We Infer Causation?

- **Randomized experiments:** Random assignment eliminates confounding
- **Temporal precedence:** Cause must precede effect (necessary but not sufficient)
- **Mechanism:** Plausible biological/physical/logical pathway
- **Dose-response:** More cause leads to more effect
- **Replication:** Relationship holds across different contexts

PEARSON tells you variables are associated. Establishing causation requires additional evidence.

## Use Cases

### [[Investment Portfolio Analysis]]

**Scenario:** A portfolio manager analyzes correlations between assets to optimize diversification and reduce portfolio risk.

**Implementation:**
```
=PEARSON(StockA_Returns, StockB_Returns)
```
Calculate for all asset pairs to build a correlation matrix.

**Business Application:** Modern Portfolio Theory uses correlations to determine optimal asset allocation. Assets with low or negative correlation reduce portfolio volatility for a given expected return. During market stress, correlations often increase ("correlation breakdown"), reducing diversification benefits when they are most needed.

**Technical Details:** Use rolling windows (e.g., 60-day) to capture time-varying correlations. Consider that correlations calculated from different frequencies (daily vs. monthly returns) may differ. Log returns often provide better-behaved correlation estimates than simple returns.

---

### [[Market Research: Variable Relationships]]

**Scenario:** A marketing researcher identifies which survey variables are most strongly related to customer satisfaction.

**Implementation:**
```
Var1 to Satisfaction: =PEARSON(Var1_Responses, Satisfaction_Scores)
Var2 to Satisfaction: =PEARSON(Var2_Responses, Satisfaction_Scores)
...
```
Rank variables by absolute correlation to identify key drivers.

**Business Application:** Correlation screening identifies which factors most strongly associate with satisfaction. High-correlation variables warrant deeper investigation as potential drivers. This focuses resources on the most impactful improvement areas.

**Technical Details:** Survey data often uses ordinal scales (1-5 ratings) where Pearson correlation technically assumes interval data. Consider Spearman rank correlation for robustness. Control for confounders using partial correlation. Statistical significance depends on sample size.

---

### [[Quality Control: Process Relationships]]

**Scenario:** A process engineer analyzes relationships between process parameters and product quality to identify control points.

**Implementation:**
```
=PEARSON(ProcessTemperature, ProductStrength)
=PEARSON(ProcessPressure, ProductStrength)
=PEARSON(RawMaterialProperty, ProductStrength)
```

**Business Application:** Understanding which process variables correlate with quality helps prioritize process control efforts. Strong correlations identify candidates for control parameters. Weak or zero correlations suggest the variable may not need tight control.

**Technical Details:** Time-lagged correlations may reveal delayed effects. Process data often has autocorrelation (consecutive observations are related), which can inflate correlation significance. Consider cross-correlation analysis for time series process data.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Equivalent function:** CORREL (identical results)
- **Maximum data points:** Limited by worksheet size
- **Empty cell handling:** Pairs with non-numeric values excluded
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Equivalent function:** CORREL (identical results)
- **Maximum data points:** Limited by sheet size
- **Empty cell handling:** Same as Excel
- **Precision:** 15 significant decimal digits

### Key Difference Alert

PEARSON behaves identically to CORREL in both Excel and Google Sheets. The two function names exist as synonyms for the same calculation. No practical differences exist between platforms or between PEARSON and CORREL.

## Tips and Best Practices

1. **PEARSON = CORREL:** These functions are identical. Use whichever name is clearer in your context. PEARSON explicitly names the statistic; CORREL is shorter.

2. **Always visualize first:** Before calculating correlation, create a scatter plot. Non-linear relationships, outliers, and clustering are invisible in a single correlation number but obvious in a plot.

3. **Correlation does not imply causation:** This cannot be repeated enough. Use correlation for description and hypothesis generation, not causal inference.

4. **Square for explained variance:** PEARSON squared equals R-squared. A "strong" correlation of 0.7 means only 49% of variance is explained. Always consider what this squared value implies.

5. **Consider sample size:** With small samples (n < 30), correlation estimates are unstable and can show spurious patterns by chance. Use significance testing and report confidence intervals.

6. **Watch for outliers:** A single extreme observation can dramatically change correlation. Plot data and investigate influential points.

7. **Check for restriction of range:** If your sample has limited variability in one variable (e.g., only high performers), correlation will be attenuated compared to the full population.

8. **Use appropriate correlation type:** Pearson assumes linear relationships and roughly normal distributions. For ordinal data or non-linear monotonic relationships, consider Spearman rank correlation.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CORREL]] | Same function (identical) | Personal preference; shorter name |
| [[RSQ]] | Returns r-squared | When you want explained variance (0-1) instead of correlation (-1 to +1) |
| [[COVARIANCE.S]] | Sample covariance | When you need unstandardized co-movement measure |
| [[COVARIANCE.P]] | Population covariance | For complete population data |

### Commonly Used Together

**[[RSQ]]** - Coefficient of determination

*Explained variance:*
```
Correlation: =PEARSON(X, Y)
R-squared: =PEARSON(X, Y)^2 or =RSQ(Y, X)
```
R-squared shows proportion of variance explained.

---

**[[SLOPE]] and [[INTERCEPT]]** - Regression line

*From correlation to prediction:*
```
Correlation: =PEARSON(X, Y)
Prediction: =INTERCEPT(Y, X) + SLOPE(Y, X) * NewX
```
Correlation indicates strength; slope enables prediction.

---

**[[STDEV.S]]** - Standard deviation

*Relationship between correlation and covariance:*
```
Correlation: =PEARSON(X, Y)
Covariance: =PEARSON(X, Y) * STDEV.S(X) * STDEV.S(Y)
```
Demonstrates how correlation standardizes covariance.

---

**[[T.DIST.2T]]** - Significance testing

*Test if correlation is significant:*
```
r = PEARSON(X, Y)
t = r * SQRT((n-2)/(1-r^2))
p = T.DIST.2T(ABS(t), n-2)
```
Determines if correlation is statistically significant.

## Official Documentation

- **Microsoft Excel:** [PEARSON function](https://support.microsoft.com/en-us/office/pearson-function-0c3e30fc-e5af-49c4-808a-3ef66e034c18)
- **Google Sheets:** [PEARSON function](https://support.google.com/docs/answer/3094052)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2007 | Continued support | Improved accuracy for edge cases |
| Excel 2010+ | All versions | No changes to core functionality |
| Excel 365 | Continuous updates | Dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel |

---

*Last updated: 2026-01-10*
