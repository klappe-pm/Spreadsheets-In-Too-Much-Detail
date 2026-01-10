---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- covariance
- sample-statistics
- bivariate-statistics
- regression-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sample Covariance
- Covariance Sample
tags:
- statistical
- covariance
- sample
- regression
- unbiased-estimator
---

# COVARIANCE.S

## Description

**COVARIANCE.S** calculates the sample covariance between two datasets, providing an unbiased estimate of the population covariance when your data is a sample drawn from a larger population. Sample covariance uses n-1 (degrees of freedom) as the divisor rather than n, applying Bessel's correction to account for the fact that samples tend to underestimate population variability. This is the appropriate choice for most real-world analyses where you are working with a subset of possible observations rather than complete population data.

The distinction between population and sample covariance mirrors the choice between STDEV.P and STDEV.S for standard deviation. When you survey 100 customers from a database of 10,000, analyze monthly returns from a stock's trading history, or measure quality metrics from production samples, you are working with samples. Using COVARIANCE.S provides an unbiased estimate of what the covariance would be if you measured the entire population. The difference is most pronounced for small samples; with n=10, sample covariance is about 11% larger than population covariance. For large samples (n>100), the difference becomes negligible but still technically matters for statistical correctness.

Introduced in Excel 2010, COVARIANCE.S complements COVARIANCE.P (population covariance) with clear naming that immediately identifies the formula being used. Before 2010, Excel only offered COVAR (which calculates population covariance), leaving users who needed sample covariance to manually adjust results or write their own formulas. The ".S" suffix aligns with the naming convention used for STDEV.S, VAR.S, and other sample-based statistical functions, reducing confusion in complex analyses.

Covariance is a fundamental building block in multivariate statistics. It quantifies how two variables move together: positive covariance means above-average values tend to occur together; negative covariance means one variable tends to be above average when the other is below. However, covariance magnitude depends on the scales of both variables, making it difficult to interpret directly. For a scale-independent measure of association, divide covariance by the product of standard deviations to obtain the correlation coefficient. The formula is: r = Cov(X,Y) / (StdDev_X * StdDev_Y).

## Syntax

> [!f(x)] COVARIANCE.S Syntax
>
> ```
> =COVARIANCE.S(array1, array2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first sample dataset. Can be a cell range (A1:A100), named range, or array of numeric values. |
| `array2` | Yes | The second sample dataset. Must have the same number of data points as array1. Each position corresponds to the same observation in array1. |

### Return Value

Returns a numeric value representing the sample covariance. Can be positive, negative, or zero. Returns #N/A if arrays have different numbers of numeric data points. Returns #DIV/0! if either array contains fewer than 2 data points.

## Examples

> [!f(x)] COVARIANCE.S Examples

### Example 1: Basic Sample Covariance
```
=COVARIANCE.S(A2:A6, B2:B6)
```
Where A2:A6 contains: 1, 2, 3, 4, 5
And B2:B6 contains: 2, 4, 5, 4, 5

**Result:** 1.5

**Explanation:** Using n-1 (4) as divisor instead of n (5) makes sample covariance larger than population covariance. Result is COVARIANCE.P value (1.2) multiplied by 5/4 = 1.5.

---

### Example 2: Customer Survey Sample
```
=COVARIANCE.S(SatisfactionScores, PurchaseFrequency)
```
Where data represents 200 customers sampled from a 10,000 customer database

**Result:** Sample covariance estimating population relationship

**Explanation:** Since we are sampling to infer about all customers, sample covariance provides an unbiased estimate of the true population covariance.

---

### Example 3: Stock Return Analysis
```
=COVARIANCE.S(StockA_MonthlyReturns, StockB_MonthlyReturns)
```
Where returns are from the past 36 months

**Result:** Sample covariance of returns

**Explanation:** Historical returns are a sample of all possible returns. Sample covariance is appropriate for risk estimation and portfolio optimization using this historical data.

---

### Example 4: Comparing to Population Covariance
```
Sample: =COVARIANCE.S(A2:A11, B2:B11)
Population: =COVARIANCE.P(A2:A11, B2:B11)
Difference: =COVARIANCE.S(A2:A11, B2:B11) - COVARIANCE.P(A2:A11, B2:B11)
```
**Result:** Sample covariance is larger by factor of n/(n-1) = 10/9

**Explanation:** The formulas differ only in the divisor. Sample formula uses n-1 to provide an unbiased estimator of population covariance.

---

### Example 5: Minimum Sample Size
```
=COVARIANCE.S(A2:A3, B2:B3)
```
Where each range contains exactly 2 values

**Result:** Valid result (minimum requirement met)

**Explanation:** COVARIANCE.S requires at least 2 data pairs (dividing by n-1 = 1). With only 1 pair, it would divide by zero. More data provides more reliable estimates.

---

### Example 6: Single Data Point Error
```
=COVARIANCE.S(A2:A2, B2:B2)
```
**Result:** #DIV/0!

**Explanation:** With only one data point, n-1 = 0. Cannot calculate sample covariance with less than 2 observations.

---

### Example 7: Sample Correlation from Covariance
```
=COVARIANCE.S(X, Y) / (STDEV.S(X) * STDEV.S(Y))
```
**Result:** Same as =CORREL(X, Y)

**Explanation:** Correlation equals covariance divided by both standard deviations. Use STDEV.S (sample) to match COVARIANCE.S. Note: CORREL produces the same result regardless of population/sample consideration due to cancellation.

---

### Example 8: Experimental Data Analysis
```
=COVARIANCE.S(DrugDosage, ResponseLevel)
```
Where data represents 50 patients in a clinical trial

**Result:** Sample covariance estimating relationship in broader patient population

**Explanation:** Clinical trial patients are a sample of all potential patients. Sample covariance supports inference about the drug's effect in the general population.

---

### Example 9: Named Ranges for Clarity
```
=COVARIANCE.S(Temperature_Sample, Efficiency_Sample)
```
Where named ranges clearly identify data as samples

**Result:** Sample covariance between temperature and efficiency

**Explanation:** Clear naming conventions help ensure appropriate statistical functions are used. Names ending in "_Sample" remind users that sample formulas are appropriate.

---

### Example 10: Portfolio Risk Estimation
```
=w_A^2*VAR.S(A_returns) + w_B^2*VAR.S(B_returns) + 2*w_A*w_B*COVARIANCE.S(A_returns, B_returns)
```
**Result:** Estimated portfolio variance

**Explanation:** When using historical returns to estimate future portfolio risk, sample formulas are appropriate. The sample covariance term captures diversification effects.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Fewer than 2 data pairs provided | Ensure at least 2 numeric pairs exist. Sample covariance requires n >= 2 (divides by n-1) |
| `#N/A` | Arrays have different numbers of numeric data points | Check for blank cells or text creating length mismatch |
| `#VALUE!` | Non-numeric text entered directly as argument | Use cell references; direct text arguments cause errors |
| `Larger than COVARIANCE.P` | This is expected behavior | Sample covariance is always n/(n-1) times population covariance |
| `Result differs from manual calculation` | Using n instead of n-1 | Verify formula divides by n-1 for sample covariance |

## Use Cases

### [[Investment Portfolio Optimization]]

**Scenario:** A portfolio manager uses historical returns to construct an optimal portfolio, requiring sample covariance estimates for all asset pairs.

**Implementation:**
```
Sample covariance matrix elements:
=COVARIANCE.S(Asset_i_Returns, Asset_j_Returns)

Portfolio variance estimation:
=SUMPRODUCT(Weights, MMULT(CovarianceMatrix, Weights))
```

**Business Application:** Modern Portfolio Theory optimization minimizes expected portfolio variance for a target return. Using sample covariance acknowledges that historical returns are a sample of possible returns, not the complete population. Sample statistics provide unbiased estimates for forward-looking risk projections. Robust optimization techniques account for estimation uncertainty in covariance estimates.

**Technical Details:** Sample covariances are noisy estimates, especially with short histories. Shrinkage estimators blend sample covariance with structured estimates (like diagonal or single-factor covariance) to improve out-of-sample performance. Rolling windows (e.g., 60-month) capture time-varying relationships while maintaining sufficient sample size.

---

### [[A/B Testing Analysis]]

**Scenario:** A product team analyzes experimental data to understand relationships between user characteristics and response to a new feature.

**Implementation:**
```
=COVARIANCE.S(UserEngagement_Treatment, ConversionRate_Treatment)
=COVARIANCE.S(UserEngagement_Control, ConversionRate_Control)
```

**Business Application:** A/B test participants are a sample of all potential users. Sample covariance helps understand whether the treatment changes not just average outcomes but also the relationship between user behaviors. Different covariance structures in treatment vs. control groups may indicate the feature affects users differently based on their characteristics.

**Technical Details:** Compare covariance structures between groups using statistical tests for equality of covariance matrices. Bootstrap resampling provides confidence intervals for covariance differences. Large sample sizes are needed for reliable covariance estimation; prioritize sample size when designing experiments requiring covariance analysis.

---

### [[Scientific Research Data]]

**Scenario:** Researchers study the relationship between environmental factors and biological outcomes using field measurements from sample locations.

**Implementation:**
```
=COVARIANCE.S(Pollution_Levels, Species_Diversity)
```

**Business Application:** Field samples represent a sample from the broader environment. Sample covariance provides an unbiased estimate of the relationship that would be observed if all locations were measured. Understanding covariance helps identify potential environmental impacts and guides conservation priorities.

**Technical Details:** Spatial autocorrelation can violate independence assumptions, inflating effective sample size. Consider geostatistical methods that model spatial covariance explicitly. Report confidence intervals for covariance estimates using Fisher z-transformation or bootstrap methods.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2010 and later
- **No legacy equivalent:** COVAR calculates population covariance only
- **Minimum data:** Requires at least 2 data pairs (n >= 2)
- **Maximum data points:** Limited by worksheet size
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since function introduction
- **Formula:** Uses identical n-1 divisor formula
- **Minimum data:** Requires at least 2 data pairs
- **Maximum data points:** Limited by sheet size
- **Precision:** 15 significant decimal digits

### Key Difference Alert

COVARIANCE.S behaves identically between Excel and Google Sheets. The function was introduced to Excel in 2010, and Google Sheets implemented it for compatibility. There are no practical differences in behavior or results between platforms.

## Tips and Best Practices

1. **Default to sample covariance:** In most real-world analyses, you are working with samples rather than complete populations. When unsure, COVARIANCE.S is the safer choice for statistical inference.

2. **Match population/sample across functions:** Use COVARIANCE.S with VAR.S and STDEV.S for consistency. Mixing population and sample functions can produce statistically inconsistent results.

3. **Consider sample size:** Sample covariance estimates are unreliable with small samples. As a rule of thumb, aim for at least 30 observations for reasonably stable estimates. For covariance matrices with many variables, you need n >> p (observations much greater than variables).

4. **The difference shrinks with n:** For large samples, COVARIANCE.S and COVARIANCE.P converge. With n=1000, the difference is only 0.1%. For small samples (n<30), the 3%+ difference matters more.

5. **Understand what population means:** "Population" does not mean "all people on Earth." It means all observations in the group you are describing. All students in one specific class = population of that class. Students sampled to represent all high schoolers = sample.

6. **Report sample sizes:** Always document the sample size alongside covariance estimates. A covariance from n=100 is far more reliable than one from n=10.

7. **Check for outliers:** Covariance is sensitive to extreme values. One unusual observation can dramatically affect the result. Plot your data and investigate outliers before trusting covariance calculations.

8. **Consider shrinkage estimators:** For portfolio optimization and other applications where estimation error matters, shrinkage estimators (Ledoit-Wolf, etc.) improve on raw sample covariance by pulling estimates toward structured targets.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COVARIANCE.P]] | Population covariance (divides by n) | When data represents complete population |
| [[COVAR]] | Legacy population covariance | Only for backward compatibility |
| [[CORREL]] | Correlation coefficient | When you need scale-independent relationship measure |
| [[PEARSON]] | Pearson correlation | Alternative name for correlation |

### Commonly Used Together

**[[VAR.S]]** - Sample variance

*Portfolio variance with sample estimates:*
```
=w1^2*VAR.S(A) + w2^2*VAR.S(B) + 2*w1*w2*COVARIANCE.S(A, B)
```
Consistent use of sample formulas for all variance components.

---

**[[STDEV.S]]** - Sample standard deviation

*Calculating correlation from sample covariance:*
```
=COVARIANCE.S(X, Y) / (STDEV.S(X) * STDEV.S(Y))
```
Produces the same result as CORREL.

---

**[[LINEST]]** - Regression statistics

*Compare covariance-based slope to LINEST:*
```
Slope from covariance: =COVARIANCE.S(X, Y) / VAR.S(X)
Slope from LINEST: =INDEX(LINEST(Y, X), 1, 1)
```
Both should produce identical slopes for simple linear regression.

---

**[[COUNT]]** - Count numeric values

*Document sample size with covariance:*
```
="Covariance: " & COVARIANCE.S(X, Y) & " (n=" & COUNT(X) & ")"
```
Always report sample size for context and credibility.

## Official Documentation

- **Microsoft Excel:** [COVARIANCE.S function](https://support.microsoft.com/en-us/office/covariance-s-function-0a539b74-7371-42aa-a18f-1f5320314977)
- **Google Sheets:** [COVARIANCE function](https://support.google.com/docs/answer/9368490)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | Initial release | Introduced alongside COVARIANCE.P |
| Excel 2013+ | Continued support | No changes to functionality |
| Excel 365 | Current | Dynamic array support |
| Google Sheets | Post-2010 | Added for Excel compatibility |

---

*Last updated: 2026-01-10*
