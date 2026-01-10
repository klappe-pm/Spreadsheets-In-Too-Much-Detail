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
- bivariate-statistics
- regression-analysis
- legacy-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Covariance
- Legacy Covariance
- Population Covariance
tags:
- statistical
- covariance
- regression
- legacy
- deprecated
---

# COVAR

## Description

**COVAR** calculates the covariance between two datasets, measuring how two variables change together. Covariance is the unstandardized measure of co-movement: positive covariance indicates that when one variable is above its mean, the other tends to be above its mean as well; negative covariance indicates they tend to move in opposite directions. Unlike correlation (which is bounded between -1 and 1), covariance has no upper or lower bounds and depends on the scale of the variables, making it harder to interpret directly but useful as a building block for other statistical measures.

This function calculates **population covariance**, dividing by n (the number of data pairs) rather than n-1. This is appropriate when your data represents the entire population of interest. If you have a sample from a larger population and want to estimate the population covariance, you should use COVARIANCE.S instead, which applies Bessel's correction by dividing by n-1. The distinction matters most for small samples; with large datasets, the difference becomes negligible.

**COVAR is a legacy function** maintained for backward compatibility with older spreadsheets. Microsoft introduced COVARIANCE.P and COVARIANCE.S in Excel 2010 to provide clearer naming that explicitly distinguishes between population and sample covariance. While COVAR continues to work identically to COVARIANCE.P, Microsoft recommends using the newer functions for new projects. The clearer naming reduces confusion about which formula is being applied, especially in collaborative environments where statistical literacy varies.

The mathematical relationship between covariance and correlation is fundamental: Correlation = Covariance / (StdDev_X * StdDev_Y). Thus, covariance carries the same sign as correlation but includes the scale of both variables. Covariance of height (in cm) and weight (in kg) will be numerically different from covariance of height (in inches) and weight (in pounds), even for the same data, because the units change. This scale-dependence is why correlation is typically preferred for interpretation, while covariance is used in mathematical calculations for variance of sums, portfolio theory, and regression coefficients.

## Syntax

> [!f(x)] COVAR Syntax
>
> ```
> =COVAR(array1, array2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first range of numeric values. Can be a cell range, named range, or array constant. |
| `array2` | Yes | The second range of numeric values. Must have the same number of data points as array1. |

### Return Value

Returns a numeric value representing the population covariance of the two arrays. The value can be positive, negative, or zero. Returns #N/A if arrays have different numbers of numeric data points. Returns #DIV/0! if either array is empty.

## Examples

> [!f(x)] COVAR Examples

### Example 1: Basic Positive Covariance
```
=COVAR(A2:A6, B2:B6)
```
Where A2:A6 contains: 1, 2, 3, 4, 5
And B2:B6 contains: 2, 4, 5, 4, 5

**Result:** 1.2

**Explanation:** Variables tend to increase together. When X is above its mean (3), Y tends to be above its mean (4). The positive covariance quantifies this co-movement.

---

### Example 2: Negative Covariance
```
=COVAR(A2:A6, B2:B6)
```
Where A2:A6 contains: 1, 2, 3, 4, 5
And B2:B6 contains: 10, 8, 7, 5, 2

**Result:** -4.0

**Explanation:** As X increases, Y decreases. Negative covariance indicates inverse relationship. The magnitude (-4) depends on the scales of both variables.

---

### Example 3: Zero Covariance (Independent Variables)
```
=COVAR(A2:A11, B2:B11)
```
Where A2:A11 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
And B2:B11 contains: 5, 3, 7, 2, 8, 4, 6, 5, 3, 7

**Result:** Approximately 0

**Explanation:** When variables have no systematic relationship, covariance approaches zero. This indicates independence in linear terms (though non-linear relationships may still exist).

---

### Example 4: Financial Asset Covariance
```
=COVAR(StockA_Returns, StockB_Returns)
```
Where both ranges contain daily percentage returns

**Result:** 0.00024 (example value)

**Explanation:** In portfolio theory, covariance between asset returns determines portfolio variance. Low or negative covariance between assets provides diversification benefit.

---

### Example 5: Relationship to Correlation
```
Covariance: =COVAR(A2:A100, B2:B100)
Correlation: =COVAR(A2:A100, B2:B100) / (STDEV.P(A2:A100) * STDEV.P(B2:B100))
```
**Result:** Correlation formula produces same result as =CORREL(A2:A100, B2:B100)

**Explanation:** Dividing covariance by the product of standard deviations standardizes it into correlation. This demonstrates the mathematical relationship between the measures.

---

### Example 6: Portfolio Variance Calculation
```
=w_A^2 * VAR.P(A_returns) + w_B^2 * VAR.P(B_returns) + 2 * w_A * w_B * COVAR(A_returns, B_returns)
```
**Result:** Portfolio variance

**Explanation:** Portfolio variance = w_A^2 * Var_A + w_B^2 * Var_B + 2 * w_A * w_B * Cov(A,B). Covariance is essential for calculating risk of combined assets.

---

### Example 7: Using Named Ranges
```
=COVAR(Temperature, Humidity)
```
Where Temperature and Humidity are named ranges of daily measurements

**Result:** Covariance between temperature and humidity readings

**Explanation:** Named ranges improve formula readability and maintainability in complex analyses.

---

### Example 8: Comparing to Sample Covariance
```
Population: =COVAR(A2:A11, B2:B11)
Sample: =COVARIANCE.S(A2:A11, B2:B11)
```
**Result:** Sample covariance is larger by factor of n/(n-1) = 10/9 = 1.111

**Explanation:** COVAR divides by n (population); COVARIANCE.S divides by n-1 (sample). For n=10, sample estimate is about 11% larger.

---

### Example 9: Handling Missing Data
```
=COVAR(A2:A10, B2:B10)
```
Where some cells contain blanks or text

**Result:** Covariance calculated from valid pairs only

**Explanation:** Pairs where either cell is non-numeric are excluded. Both cells in a pair must be numeric to be included in the calculation.

---

### Example 10: Verifying Against Manual Calculation
```
=SUMPRODUCT((A2:A6-AVERAGE(A2:A6))*(B2:B6-AVERAGE(B2:B6)))/COUNT(A2:A6)
```
**Result:** Same as =COVAR(A2:A6, B2:B6)

**Explanation:** Covariance formula manually computed: sum of (X - meanX)(Y - meanY) divided by n. Useful for understanding what COVAR calculates.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Arrays have different numbers of numeric data points | Ensure both ranges have equal size and corresponding cells contain numbers |
| `#DIV/0!` | One or both arrays are empty (no numeric values) | Verify ranges contain numeric data; check for all-text or all-blank ranges |
| `#VALUE!` | Non-numeric text provided as direct argument | Use cell references; direct text arguments cause errors |
| `Large unexpected value` | Scale difference between variables | This is not an error; covariance depends on variable scales. Use CORREL for standardized measure |
| `Zero result` | Variables are independent or one has zero variance | Check if relationship exists; verify data has variability |

## Use Cases

### [[Portfolio Risk Management]]

**Scenario:** A financial analyst calculates portfolio variance to assess investment risk, requiring covariance between all asset pairs in the portfolio.

**Implementation:**
```
Two-asset portfolio variance:
=w1^2*VAR.P(Asset1) + w2^2*VAR.P(Asset2) + 2*w1*w2*COVAR(Asset1, Asset2)
```

**Business Application:** Covariance is the building block of Modern Portfolio Theory. Harry Markowitz's Nobel Prize-winning work showed that portfolio risk depends not just on individual asset variances but critically on covariances between assets. Assets with low or negative covariance provide diversification benefits, reducing overall portfolio volatility without necessarily sacrificing expected returns.

**Technical Details:** Build a covariance matrix for n assets (n x n matrix). Portfolio variance = w' * Cov * w, where w is the weight vector and Cov is the covariance matrix. Use matrix multiplication (MMULT) for portfolios with many assets. Rolling covariances (e.g., 60-day) capture time-varying relationships.

---

### [[Quality Control: Correlated Defects]]

**Scenario:** A manufacturing engineer analyzes whether defects in different product components tend to occur together, indicating a common root cause.

**Implementation:**
```
=COVAR(DefectsComponentA, DefectsComponentB)
```

**Business Application:** Positive covariance between defect rates in different components suggests a common cause (raw material issue, shared process step, environmental factor). This guides root cause analysis more efficiently than investigating each defect type independently. Zero or negative covariance suggests independent failure modes requiring separate investigation.

**Technical Details:** Normalize by production volume before calculating covariance if batches differ in size. Consider time-lagged covariance if one defect might cause another with a delay. Statistical significance testing helps distinguish real relationships from random variation.

---

### [[Economic Indicator Analysis]]

**Scenario:** An economist examines the relationship between unemployment rates and consumer spending to understand economic cycles.

**Implementation:**
```
=COVAR(UnemploymentRate, ConsumerSpending)
```

**Business Application:** Negative covariance between unemployment and spending (expected relationship) confirms economic theory. Tracking covariance over time can identify structural changes in the economy. Changes in the strength of covariance may signal policy impacts or fundamental shifts in economic behavior.

**Technical Details:** Economic data is often non-stationary (trending over time), which can create spurious covariance. Difference the data or use detrending before analysis. Seasonal adjustment may be necessary for monthly or quarterly data. Consider lag structures since economic variables often have delayed relationships.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions; marked as legacy function since Excel 2010
- **Equivalent function:** COVARIANCE.P (recommended replacement)
- **Maximum data points:** Limited by worksheet size
- **Precision:** 15 significant decimal digits
- **Empty cell handling:** Pairs with non-numeric values excluded

### Google Sheets

- **Availability:** Fully supported since launch
- **Equivalent function:** COVARIANCE.P (also available as COVAR)
- **Maximum data points:** Limited by sheet size
- **Precision:** 15 significant decimal digits
- **Empty cell handling:** Same as Excel

### Key Difference Alert

COVAR functions identically in Excel and Google Sheets. The main practical difference is that Excel documentation explicitly marks COVAR as legacy and recommends COVARIANCE.P for new work, while Google Sheets treats both as equivalent current functions.

## Tips and Best Practices

1. **Use COVARIANCE.P or COVARIANCE.S for new work:** While COVAR works correctly, the newer function names are clearer about which formula is being applied. This reduces confusion in collaborative environments.

2. **Understand scale dependence:** Covariance is measured in the product of the units of both variables. Height (cm) and weight (kg) have covariance in cm*kg. This makes comparison across different variable pairs difficult; use correlation for comparability.

3. **Know when to use population vs. sample:** If your data represents the complete population of interest, COVAR (population covariance) is appropriate. If it is a sample from a larger population, use COVARIANCE.S for an unbiased estimate.

4. **Covariance is building block, not endpoint:** Covariance is typically used in calculations (portfolio variance, regression coefficients) rather than interpreted directly. For understanding relationship strength, use correlation.

5. **Check for data alignment:** Array1[i] must correspond to Array2[i]. Misaligned data (e.g., different time periods) produces meaningless results. Verify your data is properly paired.

6. **Consider centering for interpretation:** Covariance equals the average product of deviations from means. Visualizing centered data can help understand what covariance measures.

7. **Zero covariance means linear independence only:** Variables with zero covariance may still have strong non-linear relationships. Always plot your data.

8. **Be aware of outlier sensitivity:** Like correlation, covariance is sensitive to extreme values. One unusual data point can dramatically affect the result.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COVARIANCE.P]] | Population covariance (identical to COVAR) | Modern replacement with clearer naming |
| [[COVARIANCE.S]] | Sample covariance (divides by n-1) | When data is a sample from larger population |
| [[CORREL]] | Correlation coefficient | When you need scale-independent relationship measure |
| [[PEARSON]] | Pearson correlation (identical to CORREL) | Alternative name for correlation |

### Commonly Used Together

**[[VAR.P]]** - Population variance

*Use together for portfolio calculations:*
```
=w1^2*VAR.P(Asset1) + w2^2*VAR.P(Asset2) + 2*w1*w2*COVAR(Asset1, Asset2)
```
Portfolio variance requires both individual variances and pairwise covariances.

---

**[[STDEV.P]]** - Population standard deviation

*Convert covariance to correlation:*
```
=COVAR(X, Y) / (STDEV.P(X) * STDEV.P(Y))
```
Demonstrates the mathematical relationship: Correlation = Covariance / (SD_X * SD_Y).

---

**[[CORREL]]** - Correlation coefficient

*Compare raw covariance to standardized correlation:*
```
Covariance: =COVAR(X, Y)
Correlation: =CORREL(X, Y)
```
Covariance provides raw co-movement; correlation provides interpretable strength.

## Official Documentation

- **Microsoft Excel:** [COVAR function](https://support.microsoft.com/en-us/office/covar-function-50479552-2c03-4daf-bd71-a5ab88b2db03)
- **Google Sheets:** [COVAR function](https://support.google.com/docs/answer/3093993)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original function |
| Excel 2007 | Continued support | Increased row limit to 1 million+ |
| Excel 2010 | Marked as legacy | COVARIANCE.P and COVARIANCE.S introduced |
| Excel 2013+ | Backward compatibility | Continues to work identically |
| Google Sheets | Original launch (2006) | Full support; not marked as legacy |

---

*Last updated: 2026-01-10*
