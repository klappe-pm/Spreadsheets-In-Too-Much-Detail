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
- chi-squared-distribution
- hypothesis-testing
- goodness-of-fit
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- CHIDIST
- Chi-Square Distribution
- Chi-Squared
tags:
- statistics
- probability
- categorical-data
- independence-testing
- variance-testing
---

# CHISQ.DIST

## Description

CHISQ.DIST returns the chi-squared distribution for a specified value and degrees of freedom. The chi-squared distribution is a probability distribution that arises when you sum the squares of independent standard normal random variables. It is always positive (since you are squaring values) and right-skewed, meaning it has a long tail extending to the right. As the degrees of freedom increase, the distribution becomes more symmetric and begins to resemble a normal distribution. Think of it as measuring "total squared deviation" - larger chi-squared values indicate more deviation from what is expected.

The function can return either the cumulative distribution function (CDF) or the probability density function (PDF) depending on the cumulative parameter. When cumulative is TRUE, it returns the probability that a chi-squared random variable is less than or equal to your specified value. This is the left-tail probability. When cumulative is FALSE, it returns the probability density at that point, which is useful for graphing the distribution shape but rarely for direct probability calculations.

Use CHISQ.DIST when working with chi-squared test statistics, which appear in three main contexts: (1) Testing whether categorical data fits an expected pattern (goodness-of-fit tests), (2) Testing whether two categorical variables are independent (contingency table analysis), and (3) Testing hypotheses about population variance. It is fundamental in survey analysis, genetics research, quality control, and any situation where you are comparing observed frequencies to expected frequencies.

Both Excel and Google Sheets implement CHISQ.DIST identically. Excel 2010 introduced CHISQ.DIST to replace the older CHIDIST function, adding the ability to return probability density in addition to cumulative probability. Note that the legacy CHIDIST returned the RIGHT-tail probability, while CHISQ.DIST returns the LEFT-tail probability (consistent with other modern distribution functions). This is a critical difference when migrating old spreadsheets.

## Syntax

> [!info] CHISQ.DIST Syntax
>
> ```excel
> =CHISQ.DIST(x, deg_freedom, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The chi-squared value at which to evaluate the distribution (must be non-negative) | Yes |
| `deg_freedom` | The degrees of freedom (positive integer) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function (left-tail); FALSE returns the probability density function | Yes |

## Examples

```excel
=CHISQ.DIST(3.84, 1, TRUE)
```
**Result:** `0.95` (approximately 0.949973983)
**Explanation:** With 1 degree of freedom, a chi-squared value of 3.84 corresponds to approximately the 95th percentile. This is the critical value for a chi-squared test at the 5% significance level with df=1.

```excel
=CHISQ.DIST(5.99, 2, TRUE)
```
**Result:** `0.95` (approximately 0.949854839)
**Explanation:** With 2 degrees of freedom, the 95th percentile chi-squared value is approximately 5.99. This is used for independence tests in 2x2 contingency tables (df = (rows-1)(cols-1) = 1*1 for 2x2, but this example shows df=2).

```excel
=1 - CHISQ.DIST(7.81, 3, TRUE)
```
**Result:** `0.05` (approximately 0.050032)
**Explanation:** To find the right-tail probability (traditional chi-squared p-value), subtract the left-tail from 1. About 5% of the chi-squared distribution with df=3 exceeds 7.81.

```excel
=CHISQ.DIST(0, 5, TRUE)
```
**Result:** `0`
**Explanation:** At x=0, the cumulative probability is 0 because the chi-squared distribution only takes non-negative values, and no probability mass is at or below zero.

```excel
=CHISQ.DIST(10, 10, TRUE)
```
**Result:** `0.5595` (approximately 0.559506714)
**Explanation:** For a chi-squared distribution with df=10, a value of 10 is slightly above the median (which is approximately 9.34 for df=10). About 55.95% of the distribution falls at or below 10.

```excel
=CHISQ.DIST(5, 2, FALSE)
```
**Result:** `0.0410` (approximately 0.041042499)
**Explanation:** The probability density at chi-squared=5 for df=2. This is the height of the probability curve at that point, useful for graphing but not for calculating probabilities directly.

```excel
=CHISQ.DIST(15.51, 5, TRUE)
```
**Result:** `0.99` (approximately 0.99131)
**Explanation:** With 5 degrees of freedom, 99% of chi-squared values fall at or below 15.51. This is the critical value for the 1% significance level.

```excel
=IF(1-CHISQ.DIST(B2, C2, TRUE) < 0.05, "Significant", "Not Significant")
```
**Result:** "Significant" or "Not Significant"
**Explanation:** Tests whether a chi-squared statistic (B2) with given degrees of freedom (C2) is significant at the 5% level. The 1- converts to right-tail probability for the traditional chi-squared test.

```excel
=CHISQ.DIST(20, 5, TRUE) - CHISQ.DIST(10, 5, TRUE)
```
**Result:** `0.0696` (approximately)
**Explanation:** About 6.96% of the chi-squared distribution with df=5 falls between 10 and 20. This shows how to calculate probabilities for ranges.

```excel
=CHISQ.DIST(SUM((A2:A6-B2:B6)^2/B2:B6), 4, TRUE)
```
**Result:** (Varies based on data)
**Explanation:** Calculates the cumulative probability for a goodness-of-fit test. A2:A6 contains observed frequencies and B2:B6 contains expected frequencies. The degrees of freedom is categories minus 1.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric arguments provided | Ensure x and deg_freedom are numbers or cell references containing numbers |
| `#NUM!` | x is negative | Chi-squared values must be non-negative (0 or greater) |
| `#NUM!` | Degrees of freedom is less than 1 | Degrees of freedom must be at least 1 |
| Confusion with CHIDIST | CHIDIST returns right-tail | CHISQ.DIST returns left-tail; use 1-CHISQ.DIST for right-tail p-values |
| Wrong degrees of freedom | Miscounted categories or cells | For goodness-of-fit: df = categories - 1; For independence: df = (rows-1)(cols-1) |

## Use Cases

### Survey Response Analysis (Goodness-of-Fit)

**Scenario:** A company surveyed 500 customers about product preference among 4 options. The results were A:150, B:100, C:125, D:125. Marketing assumed equal preference (125 each). Is the observed distribution significantly different from equal preference?

**Implementation:** Calculate the chi-squared statistic and p-value:
```excel
// Calculate chi-squared: sum of (observed-expected)^2/expected
=((150-125)^2/125)+((100-125)^2/125)+((125-125)^2/125)+((125-125)^2/125)
// Returns 10
// p-value (right-tail for chi-squared test)
=1-CHISQ.DIST(10, 3, TRUE)  // df = 4 categories - 1 = 3
// Returns 0.0186
```

**Business Impact:** With p = 0.0186 < 0.05, the preference distribution is significantly different from equal. Product A appears more popular (150 vs expected 125), while Product B is less popular (100 vs expected 125). Marketing can reallocate resources accordingly.

### Contingency Table Independence Test

**Scenario:** A retailer wants to know if purchase decision (Buy/Don't Buy) is independent of the marketing channel (Email/Social/Direct). They collected data on 1000 customer interactions.

**Implementation:** Create the contingency table and calculate chi-squared:
```excel
// Observed: Email(Buy:120, NoBuy:180), Social(Buy:200, NoBuy:150), Direct(Buy:180, NoBuy:170)
// Expected = (row total * column total) / grand total
// Chi-squared = sum of (O-E)^2/E for each cell
// degrees of freedom = (2-1)(3-1) = 2
=1-CHISQ.DIST(chi_squared_value, 2, TRUE)
```

**Business Impact:** If the p-value is below 0.05, the channel significantly affects purchase behavior. The retailer can optimize marketing spend by focusing on channels that drive more purchases, supported by statistical evidence.

### Manufacturing Variance Testing

**Scenario:** A precision parts manufacturer claims their process has a variance of no more than 0.04 mm^2. A sample of 25 parts yields a sample variance of 0.06 mm^2. At the 5% significance level, does the evidence suggest the variance exceeds the claim?

**Implementation:** Perform a chi-squared test for variance:
```excel
// Chi-squared statistic = (n-1)*s^2/sigma^2_0
=(24*0.06)/0.04  // Returns 36
// Right-tail p-value (testing if variance is larger)
=1-CHISQ.DIST(36, 24, TRUE)  // Returns 0.0547
```

**Business Impact:** With p = 0.0547 > 0.05, we cannot reject the claim at the 5% level, though it is close. The manufacturer may want to investigate process consistency proactively, as the observed variance is notably higher than claimed.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | CHISQ.DIST | CHISQ.DIST (also CHIDIST with different behavior) |
| Return value | Left-tail probability | Left-tail probability |
| Legacy CHIDIST | Right-tail probability | Right-tail probability |
| Precision | 15 significant digits | 15 significant digits |
| Maximum df | Very large values supported | Very large values supported |
| Array formulas | Supported with dynamic arrays | Natively supports arrays |

CRITICAL: The legacy CHIDIST function returns RIGHT-tail probability, while CHISQ.DIST returns LEFT-tail probability. For traditional chi-squared tests where you want P(X > observed), use =1-CHISQ.DIST(x, df, TRUE) or the legacy CHIDIST.

## Tips and Best Practices

1. **Remember the tail direction:** For most chi-squared hypothesis tests, you want the right-tail probability (values MORE extreme than observed). Use =1-CHISQ.DIST(x, df, TRUE) or CHISQ.DIST.RT for this traditional p-value.

2. **Get degrees of freedom right:** For goodness-of-fit tests, df = number of categories - 1 (minus additional for each estimated parameter). For independence tests, df = (rows-1) * (columns-1). This is a common source of errors.

3. **Check expected frequencies:** Chi-squared tests assume expected frequencies are not too small (typically at least 5). With small expected values, consider combining categories or using exact tests instead.

4. **Use CHISQ.TEST for direct calculations:** When you have observed and expected ranges, CHISQ.TEST directly returns the right-tail p-value, saving the manual calculation of the test statistic.

5. **Understand the distribution shape:** Chi-squared is always non-negative and right-skewed. With df=1, it looks like a reversed J-shape. As df increases, it becomes more symmetric and approximately normal for df > 30.

6. **Know critical values:** Common critical values: df=1: 3.84 (95%), 6.63 (99%); df=2: 5.99, 9.21; df=3: 7.81, 11.34. Memorizing a few helps validate calculations quickly.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[CHISQ.DIST.RT]] | Returns right-tail probability (traditional chi-squared p-value) |
| [[CHISQ.INV]] | Inverse of CHISQ.DIST (probability to chi-squared value) |
| [[CHISQ.INV.RT]] | Right-tail inverse of chi-squared distribution |
| [[CHISQ.TEST]] | Performs chi-squared test directly on observed/expected ranges |
| [[CHIDIST]] | Legacy function returning right-tail probability |

### Commonly Used Together

**[[CHISQ.INV]]** - Finds critical values for chi-squared tests

*Finding the critical value at 5% significance with df=5:*
```excel
=CHISQ.INV(0.95, 5)  // Returns 11.07
// Values above 11.07 are in the rejection region
```

**[[CHISQ.TEST]]** - Direct hypothesis testing without manual calculation

*Testing independence in a contingency table:*
```excel
=CHISQ.TEST(observed_range, expected_range)
```
Returns the right-tail p-value directly, equivalent to the manual chi-squared calculation.

**[[SUMPRODUCT]]** - Calculate chi-squared statistic from ranges

*Computing chi-squared from observed and expected values:*
```excel
=SUMPRODUCT((observed-expected)^2/expected)
```
This calculates the test statistic for use with CHISQ.DIST.

## Official Documentation

- [Microsoft Excel CHISQ.DIST](https://support.microsoft.com/en-us/office/chisq-dist-function-8486b05e-5c05-4942-a9ea-f6b341518732)
- [Google Sheets CHISQ.DIST](https://support.google.com/docs/answer/6055811)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced CHISQ.DIST returning left-tail probability with cumulative parameter |
| Excel 2010 | 2010 | Added CHISQ.DIST.RT for right-tail probability |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as CHIDIST (right-tail only) |
| Google Sheets | 2015 | Added CHISQ.DIST with left-tail probability and density option |
