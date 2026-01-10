---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- linear-regression
- multiple-regression
- regression-statistics
- array-function
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Linear Regression
- Multiple Regression
- Least Squares Regression
- Regression Array
tags:
- statistical
- regression
- multiple-regression
- array-function
- advanced
---

# LINEST

## Description

**LINEST** is the most comprehensive regression function in spreadsheets, returning an array of statistics that fully describe a linear regression model. Unlike SLOPE and INTERCEPT which return single values, LINEST returns the regression coefficients plus optional statistics including standard errors, R-squared, F-statistic, degrees of freedom, and sum of squared residuals. For serious regression analysis, LINEST provides everything needed for statistical inference, hypothesis testing, and model evaluation in one function.

What makes LINEST particularly powerful is its ability to handle **multiple regression** with several predictor variables (X1, X2, X3, ...), not just simple regression with one predictor. When your Y depends on multiple factors, LINEST estimates all coefficients simultaneously, properly accounting for the correlations between predictors. This is essential for real-world analysis where outcomes are rarely determined by a single variable.

The function returns results in a specific array layout. For simple regression with stats=FALSE, you get two values: [slope, intercept]. With stats=TRUE, you get a 5-row by (m+1)-column array where m is the number of X variables. The first row contains coefficients (slopes in reverse order, then intercept); subsequent rows contain standard errors, R-squared, F-statistic, degrees of freedom, and sums of squares. Understanding this layout is essential for extracting the specific statistics you need.

LINEST uses ordinary least squares (OLS) to find coefficients that minimize the sum of squared residuals. This assumes linear relationships, independent observations, constant variance (homoscedasticity), and normally distributed errors. Violations of these assumptions do not break the calculation but affect the reliability of the results. The optional statistics (standard errors, F-statistic) assume these conditions hold; interpret them cautiously when assumptions are violated.

## Syntax

> [!f(x)] LINEST Syntax
>
> ```
> =LINEST(known_y's, [known_x's], [const], [stats])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `known_y's` | Yes | The dependent variable data (Y values). Range or array of numeric values. |
| `known_x's` | No | The independent variable(s). For multiple regression, provide multiple columns. If omitted, uses {1,2,3,...n} as X. |
| `const` | No | TRUE (default) to calculate the intercept normally; FALSE to force the intercept to zero (line through origin). |
| `stats` | No | FALSE (default) to return only coefficients; TRUE to return full regression statistics in 5-row array. |

### Return Value

Returns an array. Size depends on parameters:
- Simple regression, stats=FALSE: 1 row x 2 columns [slope, intercept]
- Simple regression, stats=TRUE: 5 rows x 2 columns with complete statistics
- Multiple regression with m variables, stats=TRUE: 5 rows x (m+1) columns

Returns #REF! if the output array does not fit in selected cells (legacy Excel). Returns #N/A if inputs are invalid.

## Output Array Layout (stats=TRUE)

For simple regression (one X variable):

| Row | Column 1 | Column 2 |
|-----|----------|----------|
| 1 | Slope (b1) | Intercept (b0) |
| 2 | Std Error of Slope | Std Error of Intercept |
| 3 | R-squared | Std Error of Y Estimate (STEYX) |
| 4 | F-statistic | Degrees of Freedom (n-2) |
| 5 | Regression Sum of Squares | Residual Sum of Squares |

For multiple regression with m X variables, coefficients appear in reverse order: [bm, bm-1, ..., b1, b0] in row 1.

## Examples

> [!f(x)] LINEST Examples

### Example 1: Simple Regression Coefficients Only
```
=LINEST(B2:B11, A2:A11)
```
**Result:** Array with [slope, intercept]

**Explanation:** Returns the same values as SLOPE and INTERCEPT in a single array. Select 1 row x 2 columns, then enter as array formula (Ctrl+Shift+Enter in legacy Excel).

---

### Example 2: Simple Regression with Full Statistics
```
=LINEST(B2:B11, A2:A11, TRUE, TRUE)
```
**Result:** 5-row x 2-column array with complete statistics

**Explanation:** Returns slope, intercept, their standard errors, R-squared, standard error of estimate, F-statistic, degrees of freedom, and sum of squares.

---

### Example 3: Extract Specific Statistics
```
Slope: =INDEX(LINEST(Y, X, TRUE, TRUE), 1, 1)
Intercept: =INDEX(LINEST(Y, X, TRUE, TRUE), 1, 2)
R-squared: =INDEX(LINEST(Y, X, TRUE, TRUE), 3, 1)
F-statistic: =INDEX(LINEST(Y, X, TRUE, TRUE), 4, 1)
Standard Error: =INDEX(LINEST(Y, X, TRUE, TRUE), 3, 2)
```
**Result:** Individual statistics extracted from the array

**Explanation:** Use INDEX to pull specific statistics from the LINEST array without displaying the entire output.

---

### Example 4: Multiple Regression (Two Predictors)
```
=LINEST(Sales, Advertising:Price, TRUE, TRUE)
```
Where Advertising and Price are adjacent columns

**Result:** 5-row x 3-column array

**Explanation:** Returns coefficients for both predictors plus intercept. Row 1: [Price coefficient, Advertising coefficient, Intercept].

---

### Example 5: Force Through Origin
```
=LINEST(Y, X, FALSE, TRUE)
```
**Result:** Regression forced through origin (0,0)

**Explanation:** With const=FALSE, the intercept is zero. Use when theory demands Y=0 when X=0 (e.g., distance = rate * time).

---

### Example 6: T-Test on Slope
```
Slope: =INDEX(LINEST(Y, X, TRUE, TRUE), 1, 1)
SE_Slope: =INDEX(LINEST(Y, X, TRUE, TRUE), 2, 1)
t_stat: =Slope / SE_Slope
df: =INDEX(LINEST(Y, X, TRUE, TRUE), 4, 2)
p_value: =T.DIST.2T(ABS(t_stat), df)
```
**Result:** P-value for testing if slope differs from zero

**Explanation:** Divide coefficient by its standard error to get t-statistic. Compare to t-distribution for significance testing.

---

### Example 7: Confidence Interval for Slope
```
Slope: =INDEX(LINEST(Y, X, TRUE, TRUE), 1, 1)
SE: =INDEX(LINEST(Y, X, TRUE, TRUE), 2, 1)
df: =INDEX(LINEST(Y, X, TRUE, TRUE), 4, 2)
t_critical: =T.INV.2T(0.05, df)
CI_Lower: =Slope - t_critical * SE
CI_Upper: =Slope + t_critical * SE
```
**Result:** 95% confidence interval for the slope

**Explanation:** The interval provides a range of plausible values for the true population slope.

---

### Example 8: Using LINEST for Polynomial Regression
```
=LINEST(Y, X^{1,2,3}, TRUE, TRUE)
```
Where X^{1,2,3} creates [X, X^2, X^3] columns

**Result:** Coefficients for cubic polynomial: Y = b0 + b1*X + b2*X^2 + b3*X^3

**Explanation:** LINEST handles polynomial regression by treating X, X^2, X^3 as separate predictors.

---

### Example 9: Comparing Regression Models
```
Model 1 R-sq: =INDEX(LINEST(Y, X1, TRUE, TRUE), 3, 1)
Model 2 R-sq: =INDEX(LINEST(Y, X1:X2, TRUE, TRUE), 3, 1)
Improvement: =Model2_Rsq - Model1_Rsq
```
**Result:** R-squared improvement from adding a predictor

**Explanation:** Compare R-squared values to assess whether additional predictors improve model fit.

---

### Example 10: No X Variable (Time Trend)
```
=LINEST(Revenue,, TRUE, TRUE)
```
**Result:** Regression against implicit sequence {1, 2, 3, ...}

**Explanation:** When known_x's is omitted, LINEST uses row indices as X. Useful for time trend analysis.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Output array does not fit in selected cells (legacy Excel) | Select appropriately sized range before entering formula |
| `#N/A` | Insufficient data or perfect multicollinearity | Ensure adequate data points and independent X variables |
| `#VALUE!` | Non-numeric data in inputs | Verify all inputs are numeric |
| `Wrong dimensions` | Not entering as array formula (legacy Excel) | Use Ctrl+Shift+Enter; Excel 365 handles automatically |
| `Coefficients in wrong order` | Expected left-to-right but LINEST returns right-to-left | Remember: last X variable appears first in output |

## Use Cases

### [[Complete Regression Analysis]]

**Scenario:** A statistician performs full regression analysis including hypothesis testing on coefficients and model significance.

**Implementation:**
```
Full stats: =LINEST(Y, X, TRUE, TRUE)
R-squared: =INDEX(LINEST(...), 3, 1)
F-statistic: =INDEX(LINEST(...), 4, 1)
F-pvalue: =F.DIST.RT(F_stat, 1, df)
```

**Business Application:** LINEST provides all statistics needed for formal regression analysis without external software. Report coefficients with standard errors, test significance using t-tests on individual slopes, and assess overall model fit with F-test. This supports rigorous, defensible quantitative analysis.

**Technical Details:** F-test null hypothesis is that all slopes equal zero. Rejecting this means at least one predictor is significant. For multiple regression, use partial F-tests to assess individual predictor contributions. Adjusted R-squared (not directly provided) can be calculated from R-squared and degrees of freedom.

---

### [[Multiple Regression Modeling]]

**Scenario:** A marketing analyst builds a model predicting sales from multiple factors including price, advertising, and seasonality.

**Implementation:**
```
=LINEST(Sales, Price:Advertising:SeasonIndex, TRUE, TRUE)
```
Extract coefficients for each factor and their standard errors.

**Business Application:** Real business outcomes depend on multiple factors. Multiple regression disentangles their effects, revealing how each factor influences sales while controlling for others. This enables targeted optimization: increase advertising only if its coefficient is significant and large enough to justify the cost.

**Technical Details:** Check for multicollinearity (high correlation between X variables) which inflates standard errors. Consider variable selection using stepwise methods or information criteria. Verify assumptions using residual analysis.

---

### [[Model Validation and Comparison]]

**Scenario:** A data scientist compares competing models to determine which best explains the outcome variable.

**Implementation:**
```
Model A R-sq: =INDEX(LINEST(Y, XA, TRUE, TRUE), 3, 1)
Model B R-sq: =INDEX(LINEST(Y, XA:XB, TRUE, TRUE), 3, 1)
Model C R-sq: =INDEX(LINEST(Y, XA:XB:XC, TRUE, TRUE), 3, 1)
```
Compare R-squared and adjusted R-squared across models.

**Business Application:** Not all predictors add value. LINEST enables systematic model comparison. Higher R-squared is better, but adding irrelevant predictors always increases R-squared. Use adjusted R-squared or F-tests to determine if added complexity is justified.

**Technical Details:** Adjusted R-squared = 1 - (1-R^2)*(n-1)/(n-k-1), where k is number of predictors. F-test for comparing nested models: F = ((R2_full - R2_reduced)/q) / ((1-R2_full)/(n-k-1)), where q is number of added predictors.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Array formula behavior:** Legacy versions require Ctrl+Shift+Enter; Excel 365 spills automatically
- **Multiple regression:** Supports multiple X columns
- **Maximum dimensions:** Limited by worksheet size
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Array formula behavior:** Native array support; spills automatically
- **Multiple regression:** Supports multiple X columns
- **Maximum dimensions:** Limited by sheet size
- **Precision:** 15 significant decimal digits

### Key Difference Alert

LINEST behaves identically between Excel and Google Sheets for the core calculation. The main difference is array formula handling: Excel 365 and Google Sheets spill results automatically, while legacy Excel requires selecting the output range and pressing Ctrl+Shift+Enter.

## Tips and Best Practices

1. **Use stats=TRUE for serious analysis:** The coefficient-only output is rarely sufficient. Standard errors and R-squared are essential for interpretation and inference.

2. **Remember the reverse order:** For multiple regression, coefficients appear in reverse order: last X variable first. This catches many users off guard.

3. **Extract with INDEX:** Rather than displaying the entire array, use INDEX(LINEST(...), row, col) to extract specific statistics where needed.

4. **Check standard errors:** Large standard errors relative to coefficients indicate imprecise estimates. The t-statistic (coefficient/SE) should exceed ~2 for significance at 5% level.

5. **Verify assumptions:** LINEST calculates statistics assuming linear relationships, constant variance, and normal errors. Check residual plots before trusting inference.

6. **Use const=FALSE carefully:** Forcing the intercept to zero is appropriate only when theory demands it. Otherwise, it can severely distort slope estimates.

7. **Multiple regression requires more data:** Rule of thumb: at least 10-20 observations per predictor variable. Fewer observations lead to unstable estimates.

8. **Calculate adjusted R-squared:** Standard R-squared always increases with more predictors. Adjusted R-squared penalizes complexity and is better for model comparison.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SLOPE]] | Returns slope only | When you just need the slope coefficient |
| [[INTERCEPT]] | Returns intercept only | When you just need the intercept |
| [[RSQ]] | Returns R-squared only | When you only need goodness of fit |
| [[TREND]] | Returns predicted values | When you need predictions, not coefficients |
| [[LOGEST]] | Exponential regression | When relationship is exponential, not linear |

### Commonly Used Together

**[[INDEX]]** - Extract from array

*Get specific statistics:*
```
=INDEX(LINEST(Y, X, TRUE, TRUE), 3, 1)  // R-squared
```
Essential for pulling individual values from LINEST output.

---

**[[T.DIST.2T]]** - T-distribution p-value

*Test coefficient significance:*
```
=T.DIST.2T(ABS(coefficient/SE), df)
```
Calculates p-value for hypothesis test on coefficients.

---

**[[T.INV.2T]]** - T critical value

*Build confidence intervals:*
```
=coefficient +/- T.INV.2T(0.05, df) * SE
```
Creates 95% confidence interval for coefficients.

---

**[[F.DIST.RT]]** - F-distribution p-value

*Test overall model significance:*
```
=F.DIST.RT(F_statistic, df1, df2)
```
P-value for testing if all slopes equal zero.

## Official Documentation

- **Microsoft Excel:** [LINEST function](https://support.microsoft.com/en-us/office/linest-function-84d7d0d9-6e50-4101-977a-fa7abf772b6d)
- **Google Sheets:** [LINEST function](https://support.google.com/docs/answer/3094249)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2007 | Continued support | Improved numerical accuracy |
| Excel 2010+ | All subsequent versions | Enhanced stability |
| Excel 365 | Continuous updates | Dynamic array spilling |
| Google Sheets | Original launch (2006) | Full support with native arrays |

---

*Last updated: 2026-01-10*
