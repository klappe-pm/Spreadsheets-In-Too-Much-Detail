---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- probability-distributions
- f-distribution
- hypothesis-testing
- anova
- variance-testing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- FDIST
- F Distribution
- Fisher Distribution
- Variance Ratio Distribution
tags:
- statistics
- probability
- anova
- regression
- variance-comparison
---

# F.DIST

## Description

F.DIST returns the F probability distribution for a specified value and degrees of freedom. The F-distribution is a probability distribution that arises when comparing two variances or, equivalently, as the ratio of two chi-squared distributions. It is named after Sir Ronald Fisher, one of the founders of modern statistics. The F-distribution is always positive (since it is a ratio of positive quantities) and right-skewed, though it becomes more symmetric as the degrees of freedom increase. Think of it as measuring "relative variability" - an F-value of 1 means equal variances, while values significantly greater than 1 suggest one source of variability is larger than another.

The function can return either the cumulative distribution function (CDF) or the probability density function (PDF) depending on the cumulative parameter. When cumulative is TRUE, it returns the probability that an F random variable is less than or equal to your specified value. This is the left-tail probability. When cumulative is FALSE, it returns the probability density at that point, useful for graphing but rarely for direct probability calculations. The F-distribution has two degrees of freedom parameters: df1 for the numerator and df2 for the denominator.

Use F.DIST when working with F-test statistics, which appear in three main contexts: (1) Comparing two population variances (variance ratio test), (2) Analysis of Variance or ANOVA (testing if means differ across groups), and (3) Regression analysis (testing overall model significance and comparing nested models). It is fundamental in experimental design, quality control, financial analysis, and any situation where you are comparing variability between groups or testing if a regression model explains significant variation.

Both Excel and Google Sheets implement F.DIST identically. Excel 2010 introduced F.DIST to replace the older FDIST function, adding the ability to return probability density in addition to cumulative probability. Note that the legacy FDIST returned the RIGHT-tail probability, while F.DIST returns the LEFT-tail probability (consistent with other modern distribution functions). This is a critical difference when migrating old spreadsheets.

## Syntax

> [!info] F.DIST Syntax
>
> ```excel
> =F.DIST(x, deg_freedom1, deg_freedom2, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The F-value at which to evaluate the distribution (must be non-negative) | Yes |
| `deg_freedom1` | The numerator degrees of freedom (positive integer) | Yes |
| `deg_freedom2` | The denominator degrees of freedom (positive integer) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function (left-tail); FALSE returns the probability density function | Yes |

## Examples

```excel
=F.DIST(1, 5, 10, TRUE)
```
**Result:** `0.5434` (approximately 0.543449)
**Explanation:** With df1=5 and df2=10, an F-value of 1 corresponds to about the 54th percentile. Since F=1 represents equal variances, roughly half the distribution is above and half below, though not exactly 50% due to the distribution's asymmetry.

```excel
=F.DIST(3.33, 3, 20, TRUE)
```
**Result:** `0.95` (approximately 0.949996)
**Explanation:** With df1=3 (numerator) and df2=20 (denominator), about 95% of F-values fall at or below 3.33. This is the critical value for a one-tailed F-test at the 5% significance level.

```excel
=1 - F.DIST(4.26, 2, 15, TRUE)
```
**Result:** `0.0344` (approximately)
**Explanation:** To find the right-tail probability (traditional F-test p-value), subtract the left-tail from 1. About 3.44% of the F-distribution with df1=2, df2=15 exceeds 4.26.

```excel
=F.DIST(0, 5, 10, TRUE)
```
**Result:** `0`
**Explanation:** At x=0, the cumulative probability is 0 because the F-distribution only takes non-negative values, and no probability mass is at or below zero.

```excel
=F.DIST(2.5, 10, 10, TRUE)
```
**Result:** `0.9454` (approximately)
**Explanation:** When both degrees of freedom are equal, the distribution is still not symmetric. With df1=df2=10, about 94.54% of F-values fall at or below 2.5.

```excel
=F.DIST(1.5, 4, 30, FALSE)
```
**Result:** `0.3867` (approximately)
**Explanation:** The probability density at F=1.5 for df1=4, df2=30. This is the height of the probability curve at that point, useful for graphing but not for calculating probabilities directly.

```excel
=F.DIST(4.35, 4, 40, TRUE)
```
**Result:** `0.995` (approximately)
**Explanation:** With df1=4 and df2=40, 99.5% of F-values fall at or below 4.35. Only 0.5% of the distribution exceeds this value, making it useful for stringent significance tests.

```excel
=IF(1-F.DIST(B2, C2, D2, TRUE) < 0.05, "Significant", "Not Significant")
```
**Result:** "Significant" or "Not Significant"
**Explanation:** Tests whether an F-statistic (B2) with numerator df (C2) and denominator df (D2) is significant at the 5% level. The 1- converts to right-tail probability for the traditional F-test.

```excel
=F.DIST(5, 3, 50, TRUE) - F.DIST(2, 3, 50, TRUE)
```
**Result:** `0.0896` (approximately)
**Explanation:** About 8.96% of the F-distribution with df1=3, df2=50 falls between F=2 and F=5. This shows how to calculate probabilities for ranges.

```excel
=1 - F.DIST((VAR.S(A:A)/VAR.S(B:B)), COUNT(A:A)-1, COUNT(B:B)-1, TRUE)
```
**Result:** (Varies based on data)
**Explanation:** Calculates the p-value for an F-test comparing variances of two samples in columns A and B. The larger variance should be in the numerator for a one-tailed test.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric arguments provided | Ensure x and both df values are numbers or cell references |
| `#NUM!` | x is negative | F-values must be non-negative (0 or greater) |
| `#NUM!` | Either degrees of freedom is less than 1 | Both df1 and df2 must be at least 1 |
| Confusion with FDIST | FDIST returns right-tail | F.DIST returns left-tail; use 1-F.DIST for right-tail p-values |
| Wrong df order | Swapped numerator and denominator | df1 is numerator (between groups), df2 is denominator (within groups) |

## Use Cases

### ANOVA Testing in Product Research

**Scenario:** A consumer products company tested 4 different packaging designs with 5 focus groups each (20 total groups). The between-group mean square was 150 and the within-group mean square was 30. Is there a significant difference in customer ratings across packaging designs?

**Implementation:** Calculate the F-statistic and p-value:
```excel
// F-statistic = MS_between / MS_within
=150/30  // Returns 5
// Degrees of freedom: df1 = k-1 = 4-1 = 3; df2 = N-k = 20-4 = 16
// p-value (right-tail for ANOVA)
=1-F.DIST(5, 3, 16, TRUE)  // Returns 0.0124
```

**Business Impact:** With p = 0.0124 < 0.05, there is a significant difference in ratings across packaging designs. The company should conduct post-hoc tests to identify which specific designs differ, then invest in the most effective packaging before launch.

### Regression Model Significance

**Scenario:** A marketing analyst built a regression model with 3 predictors (advertising, price, distribution) using 50 observations. The regression sum of squares was 1200 and the residual sum of squares was 800. Is the overall model significant?

**Implementation:** Calculate the F-statistic for the regression:
```excel
// MS_regression = SSR/df_regression = 1200/3 = 400
// MS_residual = SSE/df_residual = 800/(50-3-1) = 800/46 = 17.39
// F-statistic = MS_regression / MS_residual
=400/17.39  // Returns 23.0
// p-value
=1-F.DIST(23, 3, 46, TRUE)  // Returns < 0.0001
```

**Business Impact:** The extremely small p-value indicates the regression model is highly significant. At least one predictor has a real relationship with sales. The analyst can proceed to examine individual coefficient significance and use the model for forecasting with confidence.

### Comparing Manufacturing Process Variability

**Scenario:** A quality engineer wants to compare variability between two production lines. Line A produced 25 samples with variance 0.025, and Line B produced 31 samples with variance 0.015. Is Line A significantly more variable than Line B at the 5% level?

**Implementation:** Perform the F-test for variances:
```excel
// F-statistic = larger variance / smaller variance
=0.025/0.015  // Returns 1.667
// df1 = n_A - 1 = 24; df2 = n_B - 1 = 30 (larger variance in numerator)
// One-tailed p-value (testing if Line A is MORE variable)
=1-F.DIST(1.667, 24, 30, TRUE)  // Returns 0.0873
```

**Business Impact:** With p = 0.0873 > 0.05, there is not sufficient evidence that Line A is significantly more variable than Line B at the 5% level. The engineer cannot conclude the lines have different process stability, though the observed difference might warrant monitoring.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | F.DIST | F.DIST (also FDIST with different behavior) |
| Return value | Left-tail probability | Left-tail probability |
| Legacy FDIST | Right-tail probability | Right-tail probability |
| Precision | 15 significant digits | 15 significant digits |
| Maximum df | Very large values supported | Very large values supported |
| Array formulas | Supported with dynamic arrays | Natively supports arrays |

CRITICAL: The legacy FDIST function returns RIGHT-tail probability, while F.DIST returns LEFT-tail probability. For traditional F-tests where you want P(X > observed), use =1-F.DIST(x, df1, df2, TRUE) or the legacy FDIST.

## Tips and Best Practices

1. **Remember the tail direction:** For most F-tests (ANOVA, variance comparison), you want the right-tail probability. Use =1-F.DIST(x, df1, df2, TRUE) or F.DIST.RT for this traditional p-value.

2. **Get degrees of freedom order right:** df1 is the numerator (typically "between groups" or "regression"), df2 is the denominator (typically "within groups" or "residual"). Swapping them gives incorrect results.

3. **Know the relationship to other distributions:** The F-distribution is related to chi-squared: F = (chi1/df1) / (chi2/df2). It is also related to the t-distribution: t^2 with df equals F with df1=1 and df2=df.

4. **Use F.TEST for direct variance comparison:** When comparing two sample ranges, F.TEST directly returns the two-tailed p-value, saving the manual variance calculation and F.DIST formula.

5. **Check assumptions:** F-tests assume normally distributed populations. With non-normal data, especially small samples, results may be unreliable. Consider robust alternatives like Levene's test for variance comparison.

6. **Interpret F-values:** F=1 means equal variances (or explained equals unexplained variance in regression). F>1 suggests more variability in numerator than denominator. The further from 1, the stronger the evidence against equal variances or model insignificance.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[F.DIST.RT]] | Returns right-tail probability (traditional F-test p-value) |
| [[F.INV]] | Inverse of F.DIST (probability to F-value) |
| [[F.INV.RT]] | Right-tail inverse of F-distribution |
| [[F.TEST]] | Performs F-test for comparing two variances directly |
| [[FDIST]] | Legacy function returning right-tail probability |

### Commonly Used Together

**[[F.INV]]** - Finds critical values for F-tests

*Finding the critical value at 5% significance:*
```excel
=F.INV(0.95, 3, 20)  // Returns 3.10
// F-values above 3.10 are in the rejection region for this one-tailed test
```

**[[F.TEST]]** - Direct hypothesis testing for variance equality

*Comparing variances of two samples:*
```excel
=F.TEST(A2:A30, B2:B30)
```
Returns the two-tailed p-value directly for testing whether variances are equal.

**[[VAR.S]]** - Calculate sample variances for manual F-test

*Computing F-statistic from two samples:*
```excel
=VAR.S(larger_variance_sample) / VAR.S(smaller_variance_sample)
```
This F-statistic is then used with F.DIST to find the p-value.

**[[ANOVA functions]]** - Excel's Data Analysis add-in provides complete ANOVA

*For complex ANOVA, use Data Analysis:*
While F.DIST handles individual F-tests, Excel's Data Analysis Toolpak provides complete ANOVA output with all sums of squares, mean squares, and p-values calculated automatically.

## Official Documentation

- [Microsoft Excel F.DIST](https://support.microsoft.com/en-us/office/f-dist-function-a887efdc-7c8e-46cb-a74a-f884cd29b25d)
- [Google Sheets F.DIST](https://support.google.com/docs/answer/6055816)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced F.DIST returning left-tail probability with cumulative parameter |
| Excel 2010 | 2010 | Added F.DIST.RT for right-tail probability |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as FDIST (right-tail only) |
| Google Sheets | 2015 | Added F.DIST with left-tail probability and density option |
