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
- population-statistics
- bivariate-statistics
- regression-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Population Covariance
- Covariance Population
tags:
- statistical
- covariance
- population
- regression
- bivariate
---

# COVARIANCE.P

## Description

**COVARIANCE.P** calculates the population covariance between two datasets, measuring how two variables move together when your data represents the entire population of interest. Population covariance uses n (the count of data pairs) as the divisor, making it the appropriate choice when you have complete data rather than a sample. If you are working with a sample drawn from a larger population, use COVARIANCE.S instead, which applies Bessel's correction (dividing by n-1) to provide an unbiased estimate of the true population covariance.

Covariance quantifies the joint variability of two variables. When both variables tend to be above (or below) their respective means at the same time, covariance is positive. When one variable tends to be above its mean while the other is below, covariance is negative. When there is no systematic relationship, covariance is zero. The magnitude of covariance depends on the scales of both variables, which makes it difficult to interpret directly. A covariance of 50 might be large or small depending on whether your variables are in dollars or millions of dollars. For interpretable relationship strength, divide covariance by the product of standard deviations to get correlation.

This function was introduced in Excel 2010 as part of Microsoft's effort to improve statistical function naming. COVARIANCE.P replaces the legacy COVAR function with a name that explicitly indicates population covariance is being calculated. Both functions produce identical results: COVARIANCE.P(array1, array2) equals COVAR(array1, array2). The ".P" suffix follows the pattern established for other statistical functions like STDEV.P, VAR.P, and STDEV.S, VAR.S, making it immediately clear whether population or sample formulas are being used.

The mathematical formula for population covariance is: Cov_P(X,Y) = SUM((Xi - mean_X) * (Yi - mean_Y)) / n. This equals the average of the products of deviations from means. Covariance appears in many statistical formulas: the regression slope equals Cov(X,Y)/Var(X), portfolio variance includes covariance terms, and principal component analysis uses the covariance matrix to identify underlying factors in multivariate data.

## Syntax

> [!f(x)] COVARIANCE.P Syntax
>
> ```
> =COVARIANCE.P(array1, array2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first population dataset. Can be a cell range (A1:A100), named range, or array of values. Must contain numeric values. |
| `array2` | Yes | The second population dataset. Must have the same number of data points as array1. Each position corresponds to the same observation in array1. |

### Return Value

Returns a numeric value representing the population covariance. Can be positive (variables move together), negative (variables move oppositely), or zero (no linear relationship). Returns #N/A if arrays have different numbers of numeric data points. Returns #DIV/0! if either array is empty.

## Examples

> [!f(x)] COVARIANCE.P Examples

### Example 1: Basic Positive Covariance
```
=COVARIANCE.P(A2:A6, B2:B6)
```
Where A2:A6 contains: 1, 2, 3, 4, 5
And B2:B6 contains: 2, 4, 5, 4, 5

**Result:** 1.2

**Explanation:** The variables tend to increase together. Mean of X = 3, mean of Y = 4. Deviations from means multiply to positive values more often than negative, yielding positive covariance.

---

### Example 2: Strong Negative Covariance
```
=COVARIANCE.P(A2:A6, B2:B6)
```
Where A2:A6 contains: 1, 2, 3, 4, 5
And B2:B6 contains: 10, 8, 6, 4, 2

**Result:** -4.0

**Explanation:** As X increases, Y decreases proportionally. This is a perfect negative linear relationship, reflected in the negative covariance.

---

### Example 3: Zero Covariance
```
=COVARIANCE.P(A2:A11, B2:B11)
```
Where A2:A11 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
And B2:B11 contains: 5, 5, 5, 5, 5, 5, 5, 5, 5, 5

**Result:** 0

**Explanation:** When one variable is constant (no variation), it cannot covary with anything. Covariance is zero whenever either variable has zero variance.

---

### Example 4: All Students in a Class (Population Context)
```
=COVARIANCE.P(MidtermScores, FinalScores)
```
Where both ranges contain scores for all 30 students in one specific class

**Result:** Population covariance of exam scores

**Explanation:** Since we have data for the entire population (all students in this specific class), population covariance is appropriate. We are not inferring about a larger group.

---

### Example 5: Manufacturing Process (Complete Run)
```
=COVARIANCE.P(Temperature, Yield)
```
Where data represents all batches in a production run

**Result:** Covariance between temperature settings and product yield

**Explanation:** If analyzing a complete production run (not sampling from ongoing production), population covariance describes the actual relationship in that run.

---

### Example 6: Converting to Correlation
```
=COVARIANCE.P(X, Y) / (STDEV.P(X) * STDEV.P(Y))
```
**Result:** Same as =CORREL(X, Y)

**Explanation:** The correlation formula standardizes covariance by dividing by both standard deviations. Note: use STDEV.P (population) to match COVARIANCE.P.

---

### Example 7: Regression Slope from Covariance
```
=COVARIANCE.P(X, Y) / VAR.P(X)
```
**Result:** Same as =SLOPE(Y, X)

**Explanation:** The simple linear regression slope equals Cov(X,Y)/Var(X). This demonstrates how covariance underlies regression analysis.

---

### Example 8: Comparing Population and Sample Covariance
```
Population: =COVARIANCE.P(A2:A11, B2:B11)
Sample: =COVARIANCE.S(A2:A11, B2:B11)
Ratio: =COVARIANCE.S(A2:A11, B2:B11) / COVARIANCE.P(A2:A11, B2:B11)
```
**Result:** Ratio = n/(n-1) = 10/9 = 1.111

**Explanation:** Sample covariance is always larger than population covariance by the factor n/(n-1). This correction makes the sample estimate unbiased.

---

### Example 9: Covariance Matrix Element
```
=COVARIANCE.P(OFFSET($A$2:$A$100,0,COLUMN()-2), OFFSET($A$2:$A$100,0,ROW()-2))
```
**Result:** One element of a covariance matrix

**Explanation:** Build a covariance matrix by calculating COVARIANCE.P for each pair of variables. The diagonal elements are variances (variable with itself).

---

### Example 10: Two-Asset Portfolio Variance
```
=w_A^2*VAR.P(A) + w_B^2*VAR.P(B) + 2*w_A*w_B*COVARIANCE.P(A, B)
```
Where w_A and w_B are portfolio weights summing to 1

**Result:** Variance of the combined portfolio

**Explanation:** Portfolio variance depends on individual variances plus the covariance term. Low covariance between assets reduces portfolio variance for a given expected return.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Arrays have different numbers of numeric data points | Verify both arrays have equal length; check for blank or text cells creating mismatch |
| `#DIV/0!` | One or both arrays contain no numeric values | Ensure data exists; check for all-text or empty ranges |
| `#VALUE!` | Non-numeric text provided directly as argument | Use cell references instead of typing text directly in the formula |
| `Unexpected magnitude` | Result scale depends on variable units | This is correct behavior; covariance is scale-dependent. Use CORREL for standardized measure |
| `Zero when relationship exists` | Non-linear relationship present | Covariance measures linear association only. Plot data to check for curved patterns |

## Use Cases

### [[Covariance Matrix Construction]]

**Scenario:** A quantitative analyst builds a covariance matrix for portfolio optimization, requiring covariance between every pair of assets.

**Implementation:**
```
For each cell (i,j) in matrix:
=COVARIANCE.P(INDEX(Returns,,i), INDEX(Returns,,j))
```
Or use OFFSET formulas for dynamic matrix building.

**Business Application:** The covariance matrix is the foundation of mean-variance portfolio optimization. Markowitz optimization minimizes portfolio variance subject to return targets, which requires the complete covariance matrix. Eigenvalue decomposition of the covariance matrix identifies principal components for factor-based risk models.

**Technical Details:** A covariance matrix is symmetric (Cov(X,Y) = Cov(Y,X)) and positive semi-definite. The diagonal contains variances (Cov(X,X) = Var(X)). For n assets, calculate n*(n+1)/2 unique values. Use COVARIANCE.P when analyzing a specific historical period rather than estimating future covariance.

---

### [[Multivariate Quality Analysis]]

**Scenario:** A process engineer examines relationships between multiple quality metrics to understand how product characteristics interact.

**Implementation:**
```
=COVARIANCE.P(Thickness, Strength)
=COVARIANCE.P(Thickness, Flexibility)
=COVARIANCE.P(Strength, Flexibility)
```

**Business Application:** Understanding covariance structure helps optimize processes. If thickness and strength have positive covariance, adjustments that increase thickness will tend to increase strength. If they have negative covariance, trade-offs must be managed. Process improvements can target reducing unwanted covariances (making quality dimensions more independent).

**Technical Details:** Visualize relationships with scatter plot matrices. Consider principal component analysis (PCA) to identify underlying factors driving multiple metrics. Control charts for multivariate data (T-squared) use the covariance structure.

---

### [[Census Data Analysis]]

**Scenario:** A demographer analyzes complete census data to understand relationships between population characteristics in a defined region.

**Implementation:**
```
=COVARIANCE.P(Income, EducationYears)
```

**Business Application:** Census data represents a complete population for the defined geographic and time scope. Population covariance is appropriate because we are describing the actual population, not inferring about a larger one. Understanding covariance between demographic variables informs policy decisions about resource allocation and program targeting.

**Technical Details:** Even with census data, consider whether your analysis question requires population or sample treatment. If inferring about future populations or different regions, sample covariance might be more appropriate. Weight calculations by population size when combining geographic units.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2010 and later
- **Legacy equivalent:** COVAR (produces identical results)
- **Maximum data points:** Limited by worksheet size (1,048,576 rows)
- **Array formula support:** Dynamic arrays in Excel 365; CSE arrays in earlier versions
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since function introduction
- **Alternative:** COVAR (also available)
- **Maximum data points:** Limited by sheet size
- **Array formula support:** Native array support
- **Precision:** 15 significant decimal digits

### Key Difference Alert

COVARIANCE.P behaves identically between Excel and Google Sheets. The function was introduced in Excel 2010 as part of statistical function improvements. Google Sheets implemented it for compatibility. Both platforms also support the legacy COVAR function with identical behavior.

## Tips and Best Practices

1. **Choose .P or .S deliberately:** Use COVARIANCE.P when data represents the complete population; use COVARIANCE.S for samples. The choice affects results, especially for small datasets. When in doubt, .S is usually safer for statistical inference.

2. **Use consistent population/sample across functions:** If using COVARIANCE.P, pair it with VAR.P and STDEV.P for calculations. Mixing population and sample functions produces inconsistent results.

3. **Understand the relationship to correlation:** Covariance divided by both standard deviations equals correlation. If you need to interpret relationship strength, correlation is more intuitive. Covariance is best for calculations.

4. **Mind the units:** Covariance has units of (X units) * (Y units). Height in cm and weight in kg produces covariance in cm*kg. This makes covariance hard to interpret and impossible to compare across different variable pairs.

5. **Check data alignment:** Each position in array1 must correspond to the same observation in array2. Time series must be aligned by date; cross-sectional data must be matched by subject ID.

6. **Covariance and regression:** The simple linear regression slope equals Cov(X,Y)/Var(X). This connects covariance to prediction and helps understand regression geometrically.

7. **Zero covariance does not mean independence:** Variables can have zero covariance but strong non-linear relationships. Consider a U-shaped relationship where high Y occurs with both very low and very high X. Always visualize your data.

8. **Beware of outliers:** Like correlation, covariance is sensitive to extreme values. A single unusual observation can dominate the result. Robust alternatives exist but are not built into spreadsheets.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COVARIANCE.S]] | Sample covariance (divides by n-1) | When data is a sample from larger population |
| [[COVAR]] | Legacy population covariance (identical to COVARIANCE.P) | Backward compatibility; .P is preferred for clarity |
| [[CORREL]] | Correlation coefficient | When you need scale-independent relationship measure |
| [[PEARSON]] | Pearson correlation | Alternative name for CORREL |

### Commonly Used Together

**[[VAR.P]]** - Population variance

*Portfolio variance calculation:*
```
=w1^2*VAR.P(Asset1) + w2^2*VAR.P(Asset2) + 2*w1*w2*COVARIANCE.P(Asset1, Asset2)
```
Variance and covariance together determine portfolio risk.

---

**[[STDEV.P]]** - Population standard deviation

*Converting covariance to correlation:*
```
=COVARIANCE.P(X, Y) / (STDEV.P(X) * STDEV.P(Y))
```
Standardizing covariance produces the correlation coefficient.

---

**[[SLOPE]]** - Regression slope

*Verify slope calculation from covariance:*
```
=COVARIANCE.P(X, Y) / VAR.P(X)  // Equals SLOPE(Y, X)
```
Demonstrates that regression slope is covariance divided by x-variance.

---

**[[MMULT]]** - Matrix multiplication

*Portfolio variance with many assets:*
```
=MMULT(MMULT(Weights, CovarianceMatrix), TRANSPOSE(Weights))
```
For portfolios with many assets, matrix multiplication with the covariance matrix is more efficient.

## Official Documentation

- **Microsoft Excel:** [COVARIANCE.P function](https://support.microsoft.com/en-us/office/covariance-p-function-6f0e1e6d-956d-4e4b-9943-cfef0bf9edfc)
- **Google Sheets:** [COVARIANCE function](https://support.google.com/docs/answer/9368490)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | Initial release | Introduced as replacement for COVAR |
| Excel 2013+ | Continued support | No changes to functionality |
| Excel 365 | Current | Dynamic array support |
| Google Sheets | Post-2010 | Added for Excel compatibility |

---

*Last updated: 2026-01-10*
