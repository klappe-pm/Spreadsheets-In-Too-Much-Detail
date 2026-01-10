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
- normal-distribution
- inverse-functions
- percentiles
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- NORMINV
- Inverse Normal
- Normal Quantile Function
tags:
- statistics
- probability
- inverse-distribution
- percentile-calculation
---

# NORM.INV

## Description

NORM.INV returns the inverse of the normal cumulative distribution function for a specified probability, mean, and standard deviation. In simpler terms, while NORM.DIST tells you "what's the probability of getting a value this low or lower?", NORM.INV works backward to answer "what value corresponds to this probability?" This makes it essential for finding cutoff points, thresholds, and percentiles in normally distributed data. For example, if you want to know what test score puts a student in the top 10% of the class, NORM.INV gives you that answer.

The function works by taking a probability (between 0 and 1) and returning the x-value from the normal distribution that would produce that cumulative probability. Mathematically, if NORM.DIST(x, mean, stdev, TRUE) = p, then NORM.INV(p, mean, stdev) = x. This inverse relationship is fundamental to statistical analysis, allowing you to move freely between probabilities and actual values. The returned value represents a point on the distribution's x-axis, not a probability.

Use NORM.INV when you need to establish thresholds, find percentiles, or determine critical values for normally distributed data. Common applications include setting quality control limits (what weight marks the bottom 1% of acceptable products?), determining admission cutoffs (what score corresponds to the 90th percentile?), calculating confidence intervals (what values bound the middle 95%?), and financial planning (what return level would be exceeded only 5% of the time?).

Both Excel and Google Sheets implement NORM.INV identically with the same precision. The function was introduced in Excel 2010 to replace the legacy NORMINV function (without the period), though both remain available. Google Sheets supports both naming conventions. The calculation uses numerical methods to solve the inverse of the normal distribution equation, as no simple closed-form solution exists.

## Syntax

> [!info] NORM.INV Syntax
>
> ```excel
> =NORM.INV(probability, mean, standard_dev)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `probability` | A probability corresponding to the normal distribution (must be between 0 and 1, exclusive) | Yes |
| `mean` | The arithmetic mean of the distribution | Yes |
| `standard_dev` | The standard deviation of the distribution (must be positive) | Yes |

## Examples

```excel
=NORM.INV(0.5, 100, 15)
```
**Result:** `100`
**Explanation:** A probability of 0.5 (50%) always returns the mean value because exactly half of normally distributed values fall below the mean. For an IQ distribution, the 50th percentile is exactly 100.

```excel
=NORM.INV(0.95, 100, 15)
```
**Result:** `124.67` (approximately 124.6728)
**Explanation:** Returns the 95th percentile of the IQ distribution. A score of about 124.67 is higher than 95% of the population. Only 5% score higher.

```excel
=NORM.INV(0.05, 100, 15)
```
**Result:** `75.33` (approximately 75.3272)
**Explanation:** Returns the 5th percentile. Note that this is symmetric with the 95th percentile: both are about 24.67 points from the mean (100 minus 75.33 equals 100 plus 24.67).

```excel
=NORM.INV(0.975, 0, 1)
```
**Result:** `1.96` (approximately 1.95996)
**Explanation:** For the standard normal distribution (mean=0, stdev=1), the 97.5th percentile is approximately 1.96. This is the famous z-score used in 95% confidence intervals.

```excel
=NORM.INV(0.025, 0, 1)
```
**Result:** `-1.96` (approximately -1.95996)
**Explanation:** The 2.5th percentile of the standard normal. Together with 1.96, these values bound the middle 95% of the distribution, explaining why 1.96 appears so often in statistics.

```excel
=NORM.INV(0.90, 50000, 8000)
```
**Result:** `60,251` (approximately 60251.33)
**Explanation:** For a salary distribution with mean $50,000 and standard deviation $8,000, the 90th percentile is about $60,251. Only 10% of earners make more.

```excel
=NORM.INV(0.99, 500, 10)
```
**Result:** `523.26` (approximately 523.2635)
**Explanation:** For a manufacturing process targeting 500mg tablets with 10mg standard deviation, 99% of tablets will weigh 523.26mg or less. This helps set upper specification limits.

```excel
=NORM.INV(0.01, 500, 10)
```
**Result:** `476.74` (approximately 476.7365)
**Explanation:** The 1st percentile for the same manufacturing process. Only 1% of tablets will weigh less than about 476.74mg. This helps set lower specification limits.

```excel
=NORM.INV(0.5 + 0.68/2, 0, 1)
```
**Result:** `1` (approximately 0.9944)
**Explanation:** Approximately 68% of values fall within 1 standard deviation of the mean. Adding half of 0.68 to 0.5 gives the upper boundary, confirming the z-score is approximately 1.

```excel
=AVERAGE(A:A) + NORM.INV(0.90, 0, 1) * STDEV.S(A:A)
```
**Result:** (Varies based on data)
**Explanation:** Calculates the 90th percentile of actual data by combining the sample mean, the z-score for 90%, and the sample standard deviation. This formula structure is commonly used for percentile estimation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric arguments provided | Ensure all parameters are numbers or cell references containing numbers |
| `#NUM!` | Probability is less than 0 or greater than 1 | Probability must be strictly between 0 and 1 (exclusive) |
| `#NUM!` | Probability equals exactly 0 or 1 | Use values like 0.0001 or 0.9999 instead; exact 0 or 1 would require infinite values |
| `#NUM!` | Standard deviation is zero or negative | Standard deviation must be a positive number |
| Unexpected result | Probability expressed as percentage | Divide by 100 if probability was entered as 95 instead of 0.95 |

## Use Cases

### Setting Quality Control Limits

**Scenario:** A bottling company fills containers with a target volume of 1000mL. The filling process has a standard deviation of 15mL. They want to set control limits so that only 0.1% of bottles fall outside acceptable ranges on each side.

**Implementation:** Calculate the upper and lower control limits:
```excel
=NORM.INV(0.999, 1000, 15)  // Upper limit: ~1046.40 mL
=NORM.INV(0.001, 1000, 15)  // Lower limit: ~953.60 mL
```
Bottles outside 953.60-1046.40 mL trigger process investigation.

**Business Impact:** These statistically-derived limits ensure that process adjustments are made only when true problems occur (not normal variation), reducing unnecessary downtime while catching real issues. The 0.1% threshold on each side means only 0.2% of all bottles should trigger investigation under normal conditions.

### Competitive Exam Score Thresholds

**Scenario:** A certification exam receives 10,000 applicants but can only certify the top 15%. Historical data shows scores are normally distributed with a mean of 72 and standard deviation of 11. The organization needs to determine the passing score.

**Implementation:** Calculate the cutoff for the top 15%:
```excel
=NORM.INV(0.85, 72, 11)
```
This returns approximately 83.4, so the passing score would be set at 84.

**Business Impact:** Using NORM.INV ensures the passing threshold is mathematically fair and consistent year over year, even as the applicant pool changes slightly. The organization can also pre-announce the percentile-based criteria, building trust in the certification process.

### Investment Risk Budgeting

**Scenario:** A pension fund manager needs to ensure the portfolio won't lose more than 10% in a year with 99% confidence. The portfolio's expected annual return is 7% with a standard deviation of 12%. They need to verify if the current allocation meets this risk budget.

**Implementation:** Calculate the 1st percentile of returns:
```excel
=NORM.INV(0.01, 7, 12)
```
This returns approximately -20.9%, meaning there's a 1% chance of losing more than 20.9% in a year.

**Business Impact:** Since -20.9% exceeds the -10% threshold, the portfolio doesn't meet the risk criteria. The manager can use this analysis to justify rebalancing toward lower-volatility assets or implementing hedging strategies, with quantitative support for stakeholder discussions.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | NORM.INV | NORM.INV (also NORMINV) |
| Legacy support | NORMINV still works | Both versions fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays | Natively supports arrays |
| Probability limits | Strictly between 0 and 1 | Strictly between 0 and 1 |
| Localization | Function name may vary by language | English only |

Both platforms produce identical results. The iterative numerical algorithm used to compute the inverse is the same, yielding consistent answers across platforms for any valid input.

## Tips and Best Practices

1. **Remember the inverse relationship:** NORM.INV is the exact inverse of NORM.DIST with cumulative=TRUE. Test your understanding by verifying that NORM.DIST(NORM.INV(p, mean, stdev), mean, stdev, TRUE) returns p for any valid probability.

2. **Use for confidence interval bounds:** To find the z-scores for a 95% confidence interval, use NORM.INV(0.025, 0, 1) and NORM.INV(0.975, 0, 1), giving approximately -1.96 and 1.96. Adjust the probabilities for other confidence levels.

3. **Handle edge probabilities carefully:** Probabilities very close to 0 or 1 (like 0.0000001 or 0.9999999) will return very extreme values. Consider whether such extremes are meaningful for your analysis.

4. **Convert percentiles correctly:** If someone asks for the "90th percentile," use probability 0.90. For "top 10%," also use 0.90 (since 90% fall below the cutoff). Confusion here is a common error.

5. **Combine with sample statistics:** For real data, use =NORM.INV(probability, AVERAGE(range), STDEV.S(range)) to estimate percentiles, assuming your data is approximately normal.

6. **Verify symmetry:** For any probability p, NORM.INV(p, mean, stdev) and NORM.INV(1-p, mean, stdev) are equidistant from the mean. Use this property to check calculations.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[NORM.S.INV]] | Inverse of standard normal distribution (mean=0, stdev=1) |
| [[NORM.DIST]] | Returns the normal distribution (forward calculation) |
| [[T.INV]] | Inverse of Student's t-distribution for small samples |
| [[PERCENTILE.INC]] | Returns percentiles from actual data (not assuming normality) |
| [[NORMINV]] | Legacy version of NORM.INV (compatibility) |

### Commonly Used Together

**[[NORM.DIST]]** - The forward function that NORM.INV inverts

*Verifying NORM.INV results:*
```excel
=NORM.DIST(NORM.INV(0.75, 50, 10), 50, 10, TRUE)
```
Returns 0.75, confirming the inverse relationship.

**[[AVERAGE]]** and **[[STDEV.S]]** - Calculate parameters from sample data

*Finding the 95th percentile of actual data:*
```excel
=NORM.INV(0.95, AVERAGE(A:A), STDEV.S(A:A))
```
Estimates the 95th percentile assuming the data follows a normal distribution.

**[[CONFIDENCE.NORM]]** - Calculates confidence interval half-width

*Building a confidence interval:*
```excel
=AVERAGE(A:A) - NORM.INV(0.975, 0, 1) * STDEV.S(A:A) / SQRT(COUNT(A:A))
=AVERAGE(A:A) + NORM.INV(0.975, 0, 1) * STDEV.S(A:A) / SQRT(COUNT(A:A))
```
Creates a 95% confidence interval for the population mean.

## Official Documentation

- [Microsoft Excel NORM.INV](https://support.microsoft.com/en-us/office/norm-inv-function-54b30935-fee7-493c-bedb-2278a9db7e13)
- [Google Sheets NORM.INV](https://support.google.com/docs/answer/3094022)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced NORM.INV as replacement for NORMINV with improved accuracy |
| Excel 2013 | 2013 | Enhanced precision for extreme probability values |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as NORMINV |
| Google Sheets | 2010 | Added NORM.INV alias for Excel compatibility |
