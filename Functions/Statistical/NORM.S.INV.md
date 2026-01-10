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
- inverse-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- NORMSINV
- Inverse Standard Normal
- Z-Score Lookup
- Probit Function
tags:
- statistics
- probability
- z-score
- inverse-distribution
---

# NORM.S.INV

## Description

NORM.S.INV returns the inverse of the standard normal cumulative distribution, converting a probability into its corresponding z-score. While NORM.S.DIST tells you "what probability corresponds to this z-score?", NORM.S.INV works backward to answer "what z-score corresponds to this probability?" This function is essential for finding critical values in hypothesis testing, determining thresholds for standardized data, and constructing confidence intervals. For example, if you need the z-score where 95% of values fall below, NORM.S.INV(0.95) gives you approximately 1.645.

The function takes a probability between 0 and 1 (exclusive) and returns the z-score from the standard normal distribution that would produce that cumulative probability. The standard normal distribution has mean 0 and standard deviation 1, making it the universal reference for all normal distributions. The returned z-score tells you how many standard deviations above or below the mean a value at that percentile would be. Negative z-scores indicate values below the mean; positive z-scores indicate values above.

Use NORM.S.INV when you need to find critical z-values for statistical tests, convert percentiles to standardized scores, or work with statistical tables that reference the standard normal distribution. It is fundamental for constructing confidence intervals (finding z* values), setting control chart limits, determining sample size requirements, and any application where you start with a probability and need the corresponding standardized cutoff point.

Both Excel and Google Sheets implement NORM.S.INV identically with the same precision. The function replaced the legacy NORMSINV in Excel 2010, though both remain available. NORM.S.INV(probability) is equivalent to NORM.INV(probability, 0, 1), but the simpler syntax is preferred when working with standardized data. The calculation uses numerical methods since no closed-form solution exists for the inverse of the normal cumulative distribution function.

## Syntax

> [!info] NORM.S.INV Syntax
>
> ```excel
> =NORM.S.INV(probability)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `probability` | A probability corresponding to the standard normal distribution (must be between 0 and 1, exclusive) | Yes |

## Examples

```excel
=NORM.S.INV(0.5)
```
**Result:** `0`
**Explanation:** A probability of 0.5 returns z=0 because exactly 50% of the standard normal distribution falls at or below the mean (which is 0 for the standard normal).

```excel
=NORM.S.INV(0.975)
```
**Result:** `1.96` (approximately 1.95996398)
**Explanation:** The famous 1.96 z-score: 97.5% of the standard normal distribution falls at or below this value. This is the upper critical value for 95% confidence intervals.

```excel
=NORM.S.INV(0.025)
```
**Result:** `-1.96` (approximately -1.95996398)
**Explanation:** The lower bound for 95% confidence intervals. Together with 1.96, these z-scores capture the middle 95% of the distribution.

```excel
=NORM.S.INV(0.95)
```
**Result:** `1.645` (approximately 1.64485363)
**Explanation:** 95% of values fall below this z-score. Used for one-tailed tests at the 5% significance level or for calculating the 95th percentile of standardized data.

```excel
=NORM.S.INV(0.99)
```
**Result:** `2.326` (approximately 2.32634787)
**Explanation:** The 99th percentile z-score, used for 99% confidence intervals (one-sided) or 98% confidence intervals (two-sided with 1% in each tail).

```excel
=NORM.S.INV(0.995)
```
**Result:** `2.576` (approximately 2.57582930)
**Explanation:** With 99.5% below this z-score, this value is used for 99% two-sided confidence intervals (0.5% in each tail).

```excel
=NORM.S.INV(0.8413)
```
**Result:** `1` (approximately 0.99999847)
**Explanation:** Confirms that a z-score of 1 corresponds to roughly the 84.13th percentile. This validates the 68-95-99.7 rule where 84.13% falls within one standard deviation from below.

```excel
=NORM.S.INV(1 - 0.05/2)
```
**Result:** `1.96` (approximately)
**Explanation:** Calculates the upper critical z-value for a two-tailed test at alpha=0.05. The formula (1 - alpha/2) gives the correct percentile for the upper bound.

```excel
=AVERAGE(A:A) + NORM.S.INV(0.95) * STDEV.S(A:A)
```
**Result:** (Varies based on data)
**Explanation:** Estimates the 95th percentile of actual data by adding the 95th percentile z-score times the standard deviation to the mean. Assumes normally distributed data.

```excel
=NORM.S.INV(0.001)
```
**Result:** `-3.09` (approximately -3.09023231)
**Explanation:** The 0.1th percentile z-score. Only 0.1% of values fall below z = -3.09. Useful for extreme outlier detection or setting very conservative thresholds.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric probability provided | Ensure the argument is a number or cell reference containing a number |
| `#NUM!` | Probability is less than or equal to 0 | Probability must be greater than 0; use a very small positive number like 0.0001 instead |
| `#NUM!` | Probability is greater than or equal to 1 | Probability must be less than 1; use a value like 0.9999 instead |
| Unexpected result | Probability expressed as percentage | Divide by 100 if you entered 95 instead of 0.95 |
| Confusing result | Mixed up one-tailed vs. two-tailed | For two-tailed 95% CI, use 0.975 (not 0.95) for the upper bound |

## Use Cases

### Constructing Confidence Intervals

**Scenario:** A market researcher surveyed 400 customers and found an average satisfaction score of 7.2 out of 10 with a standard deviation of 1.5. They need to construct a 95% confidence interval for the true population mean.

**Implementation:** Calculate the margin of error using the appropriate z-score:
```excel
=NORM.S.INV(0.975)  // Returns 1.96
=1.96 * 1.5 / SQRT(400)  // Margin of error: 0.147
// Confidence interval: 7.2 +/- 0.147 = (7.053, 7.347)
```

**Business Impact:** The company can report with 95% confidence that the true customer satisfaction score lies between 7.05 and 7.35. This precision helps set realistic improvement targets and provides a defensible baseline for tracking changes over time.

### Statistical Power Analysis and Sample Size

**Scenario:** A pharmaceutical company is designing a clinical trial to detect a 10% improvement in treatment efficacy. They want 80% power at a 5% significance level and need to determine how many patients to enroll.

**Implementation:** Use z-scores for power analysis:
```excel
=NORM.S.INV(0.975)  // z_alpha/2 = 1.96 for two-sided test
=NORM.S.INV(0.80)   // z_beta = 0.84 for 80% power
// Sample size formula: n = 2 * ((z_alpha/2 + z_beta) * sigma / delta)^2
```

**Business Impact:** Accurate sample size calculation prevents underpowered trials (wasting resources on inconclusive studies) or overpowered trials (exposing more patients than necessary). The z-scores from NORM.S.INV are essential inputs to this critical planning step.

### Process Capability Analysis

**Scenario:** A manufacturing engineer needs to determine process capability indices (Cp and Cpk) for a machining process. The specification limits are 100 +/- 5 units, and they need to find what z-score corresponds to the acceptable defect rate of 0.1% per tail.

**Implementation:** Find the z-scores for specification limits:
```excel
=NORM.S.INV(0.999)  // Returns 3.09 for 0.1% in upper tail
=NORM.S.INV(0.001)  // Returns -3.09 for 0.1% in lower tail
// For Cp = 1, specifications should be 3 sigma apart
// For nearly zero defects (6-sigma), need z = 6
```

**Business Impact:** Understanding the relationship between z-scores and defect rates helps set realistic quality goals. A process with z = 3.09 has about 1 defect per 1000 units per tail. The engineer can quantify exactly how much process improvement is needed to meet quality targets.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | NORM.S.INV | NORM.S.INV (also NORMSINV) |
| Legacy support | NORMSINV still works | Both versions fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays | Natively supports arrays |
| Probability limits | Strictly between 0 and 1 | Strictly between 0 and 1 |
| Localization | Function name varies by language | English only |

Both platforms return identical results using the same numerical algorithm. The legacy NORMSINV function remains available in both platforms for backward compatibility with older spreadsheets.

## Tips and Best Practices

1. **Memorize key z-scores:** Know that 1.645 is for one-tailed 95% (or two-tailed 90%), 1.96 is for two-tailed 95%, and 2.576 is for two-tailed 99%. These appear constantly in statistical work.

2. **Understand the one-tailed vs. two-tailed distinction:** For a 95% two-tailed confidence interval, use NORM.S.INV(0.975) not NORM.S.INV(0.95). The 0.975 comes from 1 - (0.05/2).

3. **Use symmetry for lower bounds:** NORM.S.INV(p) = -NORM.S.INV(1-p). So NORM.S.INV(0.025) = -NORM.S.INV(0.975) = -1.96. This saves calculations and verifies results.

4. **Combine with sample statistics:** To estimate percentiles of real data, use =AVERAGE(range) + NORM.S.INV(probability) * STDEV.S(range). This assumes your data is approximately normal.

5. **Handle extreme probabilities carefully:** Probabilities very close to 0 or 1 (like 0.0000001 or 0.9999999) return very extreme z-scores. Consider whether such extremes are meaningful for your analysis.

6. **Verify with NORM.S.DIST:** Always confirm calculations: NORM.S.DIST(NORM.S.INV(p), TRUE) should return p. This inverse relationship is a powerful debugging tool.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[NORM.INV]] | Inverse normal for any mean and standard deviation |
| [[NORM.S.DIST]] | Forward function (z-score to probability) |
| [[T.INV]] | Inverse t-distribution for small samples |
| [[STANDARDIZE]] | Converts raw values to z-scores |
| [[NORMSINV]] | Legacy version of NORM.S.INV |

### Commonly Used Together

**[[NORM.S.DIST]]** - The forward function that NORM.S.INV inverts

*Verifying inverse relationship:*
```excel
=NORM.S.DIST(NORM.S.INV(0.90), TRUE)
```
Returns 0.90, confirming the functions are inverses.

**[[CONFIDENCE.NORM]]** - Calculates confidence interval half-width directly

*Manual confidence interval calculation:*
```excel
=NORM.S.INV(1 - 0.05/2) * standard_error
// Equivalent to CONFIDENCE.NORM(0.05, stdev, n)
```

**[[AVERAGE]]** and **[[STDEV.S]]** - Apply z-scores to real data

*Finding the 99th percentile of data:*
```excel
=AVERAGE(B:B) + NORM.S.INV(0.99) * STDEV.S(B:B)
```
Converts the theoretical z-score to an actual value based on your data's parameters.

## Official Documentation

- [Microsoft Excel NORM.S.INV](https://support.microsoft.com/en-us/office/norm-s-inv-function-d6d556b4-ab7f-49cd-b526-5a20918452b1)
- [Google Sheets NORM.S.INV](https://support.google.com/docs/answer/3094109)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced NORM.S.INV as replacement for NORMSINV |
| Excel 2013 | 2013 | Enhanced precision for probabilities near 0 and 1 |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as NORMSINV |
| Google Sheets | 2010 | Added NORM.S.INV alias for Excel compatibility |
