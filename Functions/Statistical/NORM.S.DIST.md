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
- standard-normal-distribution
- z-scores
- continuous-distributions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- NORMSDIST
- Standard Normal Distribution
- Z-Distribution
- Phi Function
tags:
- statistics
- probability
- z-score
- standardization
---

# NORM.S.DIST

## Description

NORM.S.DIST returns the standard normal distribution for a specified z-value. The standard normal distribution is a special case of the normal distribution where the mean is exactly 0 and the standard deviation is exactly 1. This standardized version serves as a universal reference, allowing statisticians to work with any normal distribution by first converting values to z-scores. Think of it as a "master template" that applies to all bell curves regardless of their original scale. When you hear statisticians mention "z-scores" or "looking up values in the z-table," they are working with this distribution.

The function takes a z-value (how many standard deviations a value is from the mean) and returns either the cumulative probability or the probability density. When cumulative is TRUE, it returns the probability that a randomly selected value from the standard normal distribution will be less than or equal to z. This is equivalent to the area under the bell curve to the left of z. When cumulative is FALSE, it returns the height of the curve at that point, which is useful for graphing but rarely for probability calculations.

Use NORM.S.DIST when you are working with z-scores directly, when you want simpler formulas (since you do not need mean and standard deviation parameters), or when following statistical tables and textbooks that reference the standard normal. It is particularly valuable in hypothesis testing (calculating p-values from test statistics), constructing confidence intervals, quality control (calculating probabilities for standardized measurements), and any situation where you have already converted your data to z-scores using the STANDARDIZE function or manual calculation.

Both Excel and Google Sheets implement NORM.S.DIST identically. The function replaced the older NORMSDIST function starting in Excel 2010. Note that the legacy NORMSDIST function only returned cumulative probabilities (equivalent to cumulative=TRUE), while NORM.S.DIST adds the option for probability density. Mathematically, NORM.S.DIST(z, TRUE) is equivalent to NORM.DIST(z, 0, 1, TRUE), but the simpler syntax is often preferred.

## Syntax

> [!info] NORM.S.DIST Syntax
>
> ```excel
> =NORM.S.DIST(z, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `z` | The z-value (standardized value) for which you want the distribution | Yes |
| `cumulative` | TRUE returns the cumulative distribution function; FALSE returns the probability density function | Yes |

## Examples

```excel
=NORM.S.DIST(0, TRUE)
```
**Result:** `0.5`
**Explanation:** A z-score of 0 means the value equals the mean. Exactly 50% of the standard normal distribution falls at or below the mean, so the cumulative probability is 0.5.

```excel
=NORM.S.DIST(1, TRUE)
```
**Result:** `0.8413` (approximately 0.841344746)
**Explanation:** About 84.13% of values in the standard normal distribution fall at or below z=1 (one standard deviation above the mean). This is one of the most frequently referenced values in statistics.

```excel
=NORM.S.DIST(-1, TRUE)
```
**Result:** `0.1587` (approximately 0.158655254)
**Explanation:** About 15.87% of values fall at or below z=-1 (one standard deviation below the mean). Note that 0.8413 + 0.1587 does not equal 1 because they overlap at z=0.

```excel
=NORM.S.DIST(1.96, TRUE)
```
**Result:** `0.975` (approximately 0.97500210)
**Explanation:** The famous 1.96 z-score corresponds to approximately the 97.5th percentile. This is why 1.96 is used for 95% confidence intervals: 2.5% falls in each tail, leaving 95% in the middle.

```excel
=NORM.S.DIST(2, TRUE) - NORM.S.DIST(-2, TRUE)
```
**Result:** `0.9545` (approximately 0.954499736)
**Explanation:** About 95.45% of values fall between z=-2 and z=2 (within two standard deviations of the mean). This confirms the "95% within 2 standard deviations" rule of thumb.

```excel
=NORM.S.DIST(0, FALSE)
```
**Result:** `0.3989` (approximately 0.398942280)
**Explanation:** The probability density at z=0 (the peak of the standard normal curve) is approximately 0.3989. This is the maximum height of the standard normal bell curve.

```excel
=1 - NORM.S.DIST(1.645, TRUE)
```
**Result:** `0.05` (approximately 0.04998491)
**Explanation:** About 5% of the distribution falls above z=1.645. This z-score is used for one-tailed tests at the 5% significance level.

```excel
=2 * (1 - NORM.S.DIST(ABS(B2), TRUE))
```
**Result:** (Varies based on B2)
**Explanation:** Calculates a two-tailed p-value for any z-score. The ABS function handles both positive and negative test statistics, and multiplying by 2 accounts for both tails.

```excel
=NORM.S.DIST(STANDARDIZE(85, 100, 15), TRUE)
```
**Result:** `0.1587` (approximately)
**Explanation:** First standardizes the value 85 (mean 100, stdev 15) to a z-score of -1, then finds the cumulative probability. This is equivalent to NORM.DIST(85, 100, 15, TRUE).

```excel
=IF(NORM.S.DIST(C2, TRUE) < 0.05, "Significant", "Not Significant")
```
**Result:** "Significant" or "Not Significant"
**Explanation:** Tests whether a z-score in cell C2 falls in the lower 5% tail. Common in one-tailed hypothesis testing for detecting unusually low values.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric z-value provided | Ensure z is a number or a cell reference containing a number |
| `#VALUE!` | Cumulative parameter is not TRUE/FALSE | Use TRUE, FALSE, 1, or 0 for the cumulative parameter |
| Very small result (near 0) | z-value is very negative (below -8) | This is mathematically correct; probabilities become extremely small in the tails |
| Result very close to 1 | z-value is very positive (above 8) | This is mathematically correct; almost all probability is below extreme positive z-values |
| Unexpected result | Confused density (FALSE) with cumulative (TRUE) | For probability calculations, almost always use TRUE |

## Use Cases

### Hypothesis Testing P-Value Calculation

**Scenario:** A researcher tests whether a new teaching method improves test scores. The test statistic (z-score) comparing the treatment group to historical averages is 2.34. They need to calculate the p-value for a two-tailed test.

**Implementation:** Calculate the two-tailed p-value:
```excel
=2 * (1 - NORM.S.DIST(2.34, TRUE))
```
This returns approximately 0.0193, meaning there is about a 1.93% probability of observing results this extreme if the teaching method had no effect.

**Business Impact:** With p < 0.05, the researcher can reject the null hypothesis and conclude the teaching method likely has a real effect. The school can justify investing in the new method with statistical evidence, and the finding may be publishable in academic journals.

### Quality Control Z-Score Monitoring

**Scenario:** A semiconductor manufacturer monitors wafer thickness using z-scores calculated from historical production data. They want to flag any wafer with less than 0.5% probability of occurring naturally, indicating potential equipment malfunction.

**Implementation:** Create a monitoring formula:
```excel
=IF(OR(NORM.S.DIST(D2, TRUE) < 0.005, NORM.S.DIST(D2, TRUE) > 0.995), "ALERT", "OK")
```
Where D2 contains the z-score for current wafer thickness.

**Business Impact:** This system catches genuine process deviations (which are rare under normal conditions) while avoiding false alarms from normal variation. The 0.5% threshold means roughly 1 in 200 good wafers triggers investigation, a manageable rate that still catches real problems quickly.

### Financial Value at Risk (VaR)

**Scenario:** A trading desk calculates daily Value at Risk using the parametric method. They need to determine what z-score corresponds to a 99% confidence level to apply to their portfolio's daily return volatility.

**Implementation:** First find the z-score threshold, then apply it:
```excel
=NORM.S.INV(0.01)  // Returns -2.326
=Portfolio_StdDev * 2.326  // Daily VaR at 99% confidence
```
Verify: `=NORM.S.DIST(-2.326, TRUE)` returns 0.01, confirming 1% of returns fall below this threshold.

**Business Impact:** The trading desk can report a statistically valid VaR figure to risk management and regulators. Saying "We have 99% confidence our daily loss will not exceed $X" is directly supported by NORM.S.DIST calculations.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | NORM.S.DIST | NORM.S.DIST (also NORMSDIST) |
| Legacy support | NORMSDIST still works (cumulative only) | Both versions fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays | Natively supports arrays |
| Cumulative parameter | Required (TRUE/FALSE or 1/0) | Required (TRUE/FALSE or 1/0) |
| Extreme z handling | Accurate to about z = +/- 37 | Similar precision |

Both platforms return identical results for standard z-values. The legacy NORMSDIST function (without the period) in both platforms only supports cumulative probability, while NORM.S.DIST supports both cumulative and density modes.

## Tips and Best Practices

1. **Use TRUE for probability calculations:** The cumulative distribution (TRUE) is what you need 99% of the time. Use FALSE only when plotting the distribution curve or doing specialized mathematical work.

2. **Remember common z-score probabilities:** Z=1 gives 84.1%, z=2 gives 97.7%, z=-1 gives 15.9%, z=-2 gives 2.3%. Memorizing these helps you quickly validate results and catch errors.

3. **Combine with STANDARDIZE for real data:** When your data is not already standardized, use =NORM.S.DIST(STANDARDIZE(value, mean, stdev), TRUE) to get the same result as NORM.DIST but with the standardization step visible.

4. **Calculate two-tailed p-values correctly:** For a z-score z, the two-tailed p-value is =2*(1-NORM.S.DIST(ABS(z), TRUE)). Using ABS() ensures the formula works for both positive and negative test statistics.

5. **Understand the density interpretation:** The probability density (cumulative=FALSE) is NOT a probability. It represents the relative likelihood at a point. The area under the curve between two points gives actual probabilities.

6. **Use symmetry to your advantage:** NORM.S.DIST(-z, TRUE) = 1 - NORM.S.DIST(z, TRUE). This symmetry property lets you verify calculations and simplify formulas.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[NORM.DIST]] | Normal distribution with customizable mean and standard deviation |
| [[NORM.S.INV]] | Inverse of NORM.S.DIST (probability to z-score) |
| [[STANDARDIZE]] | Converts values to z-scores for use with NORM.S.DIST |
| [[PHI]] | Returns the density of the standard normal (same as NORM.S.DIST with FALSE) |
| [[GAUSS]] | Returns the probability between 0 and z (not cumulative from negative infinity) |

### Commonly Used Together

**[[STANDARDIZE]]** - Converts raw values to z-scores

*Complete standardization workflow:*
```excel
=NORM.S.DIST(STANDARDIZE(B2, AVERAGE(B:B), STDEV.S(B:B)), TRUE)
```
This standardizes B2 based on the column's statistics, then finds the cumulative probability.

**[[NORM.S.INV]]** - The inverse function, converting probabilities to z-scores

*Finding critical values:*
```excel
=NORM.S.INV(0.95)  // Returns 1.645 for one-tailed 95% confidence
=NORM.S.DIST(NORM.S.INV(0.95), TRUE)  // Verifies by returning 0.95
```

**[[ABS]]** - Essential for two-tailed hypothesis tests

*Two-tailed p-value calculation:*
```excel
=2 * (1 - NORM.S.DIST(ABS(test_statistic), TRUE))
```
The ABS function ensures the formula works regardless of the test statistic's sign.

## Official Documentation

- [Microsoft Excel NORM.S.DIST](https://support.microsoft.com/en-us/office/norm-s-dist-function-1e787282-3832-4520-a9ae-bd2a8d99ba88)
- [Google Sheets NORM.S.DIST](https://support.google.com/docs/answer/3094103)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced NORM.S.DIST with cumulative parameter; replaced NORMSDIST |
| Excel 2013 | 2013 | Enhanced precision for extreme z-values |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as NORMSDIST (cumulative only) |
| Google Sheets | 2010 | Added NORM.S.DIST with cumulative parameter option |
