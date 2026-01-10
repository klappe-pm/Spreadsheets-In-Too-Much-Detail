---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- confidence-interval
- normal-distribution
- margin-of-error
- inferential-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Normal Confidence Interval
- Z-Interval
- Confidence Level Normal
- Margin of Error
tags:
- statistical
- confidence-interval
- normal-distribution
- margin-of-error
- estimation
---

# CONFIDENCE.NORM

## Description

**CONFIDENCE.NORM** calculates the margin of error for a confidence interval around a population mean, assuming the population standard deviation is known. The function returns the half-width of the confidence interval - add and subtract this value from your sample mean to get the complete interval. This function uses the normal (z) distribution, which is appropriate when either the population is normally distributed or the sample size is large (typically n >= 30) thanks to the Central Limit Theorem.

The mathematical formula is: CONFIDENCE.NORM = z * (sigma / sqrt(n)), where z is the critical value from the standard normal distribution for your confidence level (e.g., 1.96 for 95% confidence), sigma is the population standard deviation, and n is the sample size. This formula embodies the key insight that confidence interval width depends on three factors: desired confidence level (higher confidence = wider interval), population variability (more variability = wider interval), and sample size (larger samples = narrower interval).

**Critical assumption: Known population standard deviation.** CONFIDENCE.NORM requires the true population standard deviation as input, not a sample estimate. In practice, sigma is rarely known - you usually estimate it from your sample. When using a sample standard deviation, CONFIDENCE.T (based on the t-distribution) is more appropriate, especially for smaller samples. CONFIDENCE.NORM with estimated sigma is only approximately valid for large samples where the t-distribution converges to normal.

**Interpreting confidence intervals correctly:** A 95% confidence interval does NOT mean there's a 95% probability that the population mean lies within the interval. The population mean is fixed (though unknown); either it's in the interval or it isn't. Instead, 95% confidence means: if we repeated this sampling and interval construction process many times, about 95% of the resulting intervals would contain the true population mean. This subtle distinction matters for proper statistical interpretation.

## Syntax

> [!f(x)] CONFIDENCE.NORM Syntax
>
> ```
> =CONFIDENCE.NORM(alpha, standard_dev, size)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `alpha` | Yes | The significance level, equal to 1 minus the confidence level. For 95% confidence, alpha = 0.05. Must be greater than 0 and less than 1. |
| `standard_dev` | Yes | The population standard deviation (sigma). Must be positive. This should be a known value, not estimated from the sample. |
| `size` | Yes | The sample size (n). Must be a positive integer (>= 1). Larger samples produce narrower intervals. |

### Return Value

Returns the margin of error (half-width of confidence interval) as a positive number. Add and subtract this value from the sample mean to obtain the confidence interval. Returns #NUM! if alpha is not between 0 and 1, standard_dev <= 0, or size < 1.

## Examples

> [!f(x)] CONFIDENCE.NORM Examples

### Example 1: 95% Confidence Interval
```
=CONFIDENCE.NORM(0.05, 10, 100)
```
**Result:** 1.96

**Explanation:** For 95% confidence (alpha=0.05), population SD of 10, and sample size 100, the margin of error is 1.96. If sample mean is 50, the interval is 50 +/- 1.96 = (48.04, 51.96).

---

### Example 2: 99% Confidence Interval
```
=CONFIDENCE.NORM(0.01, 10, 100)
```
**Result:** 2.58 (approximately)

**Explanation:** Higher confidence (99%) requires wider interval. Margin increases from 1.96 to 2.58. The interval (47.42, 52.58) is wider but provides more confidence.

---

### Example 3: 90% Confidence Interval
```
=CONFIDENCE.NORM(0.10, 10, 100)
```
**Result:** 1.64 (approximately)

**Explanation:** Lower confidence (90%) allows narrower interval. Margin decreases to 1.64. The interval is more precise but less confident.

---

### Example 4: Effect of Sample Size
```
n=25:   =CONFIDENCE.NORM(0.05, 10, 25)   Result: 3.92
n=100:  =CONFIDENCE.NORM(0.05, 10, 100)  Result: 1.96
n=400:  =CONFIDENCE.NORM(0.05, 10, 400)  Result: 0.98
```
**Result:** Margin of error decreases as sample size increases

**Explanation:** Doubling sample size reduces margin by approximately 1/sqrt(2). To halve the margin, you need 4x the sample size. This guides sample size planning.

---

### Example 5: Effect of Standard Deviation
```
SD=5:   =CONFIDENCE.NORM(0.05, 5, 100)   Result: 0.98
SD=10:  =CONFIDENCE.NORM(0.05, 10, 100)  Result: 1.96
SD=20:  =CONFIDENCE.NORM(0.05, 20, 100)  Result: 3.92
```
**Result:** Margin of error increases proportionally with standard deviation

**Explanation:** Higher population variability means less precision. Doubling SD doubles the margin. More variable populations require larger samples for same precision.

---

### Example 6: Quality Control Example
```
=CONFIDENCE.NORM(0.05, 0.5, 36)
```
**Result:** 0.163 (approximately)

**Explanation:** Testing product weights with known SD of 0.5 grams, sample of 36 items. The 95% CI for mean weight has margin +/- 0.163 grams.

---

### Example 7: Customer Satisfaction Survey
```
=CONFIDENCE.NORM(0.05, 15, 400)
```
**Result:** 1.47 (approximately)

**Explanation:** Satisfaction scores have known SD of 15 points. With 400 respondents, the 95% CI margin is 1.47 points. Sample mean of 72 gives interval (70.53, 73.47).

---

### Example 8: Building Complete Confidence Interval
```
Lower bound: =sample_mean - CONFIDENCE.NORM(0.05, sd, n)
Upper bound: =sample_mean + CONFIDENCE.NORM(0.05, sd, n)
```
**Result:** Complete interval [lower, upper]

**Explanation:** The function returns half-width only. Add/subtract from sample mean for complete interval bounds.

---

### Example 9: Small Sample Warning
```
=CONFIDENCE.NORM(0.05, 10, 15)
```
**Result:** 5.06 (approximately)

**Explanation:** With only 15 observations and estimated (not known) SD, CONFIDENCE.T would be more appropriate. CONFIDENCE.NORM slightly understates the true margin for small samples.

---

### Example 10: Determining Required Sample Size
```
If target margin = 2 with SD=10 at 95% confidence:
Required n = (z * SD / margin)^2 = (1.96 * 10 / 2)^2 = 96.04
So n = 97 minimum
Verify: =CONFIDENCE.NORM(0.05, 10, 97) returns approximately 1.99
```
**Result:** Sample size planning formula

**Explanation:** Rearranging the confidence formula helps determine how many observations are needed to achieve a target precision.

---

### Example 11: Comparing CONFIDENCE.NORM and CONFIDENCE.T
```
NORM: =CONFIDENCE.NORM(0.05, 10, 30)  Result: 3.58
T:    =CONFIDENCE.T(0.05, 10, 30)     Result: 3.73
```
**Result:** CONFIDENCE.T gives wider interval

**Explanation:** For finite samples with estimated SD, the t-distribution accounts for additional uncertainty. Difference shrinks as n increases.

---

### Example 12: Manufacturing Process Monitoring
```
=CONFIDENCE.NORM(0.05, 2.3, 50)
```
**Result:** 0.637 (approximately)

**Explanation:** Process dimension has known historical SD of 2.3mm. Sample of 50 parts gives 95% CI margin of 0.637mm for estimating current mean dimension.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | alpha <= 0 or alpha >= 1 | Alpha must be strictly between 0 and 1. For 95% confidence, use 0.05 not 95. |
| `#NUM!` | standard_dev <= 0 | Standard deviation must be positive. Check for negative values or zero. |
| `#NUM!` | size < 1 | Sample size must be at least 1. Verify sample size is positive integer. |
| `#VALUE!` | Non-numeric arguments | All parameters must be numbers. Remove text or convert. |
| `Interval too wide` | Small sample or high variability | Increase sample size or accept wider interval. Cannot reduce SD. |
| `Inappropriate application` | Using estimated SD as if known | Consider CONFIDENCE.T when SD is estimated from sample, especially for small n. |

## Use Cases

### [[Market Research: Estimating Average Spending]]

**Scenario:** A market researcher estimates average customer spending with a confidence interval based on historical knowledge of spending variability.

**Implementation:**
```
Margin: =CONFIDENCE.NORM(0.05, known_sd, sample_size)
Lower bound: =sample_mean - margin
Upper bound: =sample_mean + margin
```

**Business Application:** Management wants to estimate average order value for budgeting. Historical data established that spending SD is approximately $25. A survey of 200 customers yields mean spending of $75. CONFIDENCE.NORM(0.05, 25, 200) = $3.46. The 95% CI is ($71.54, $78.46), informing revenue projections.

**Technical Details:** Using historical SD assumes spending variability hasn't changed. If market conditions shifted, the historical SD may not apply. Consider whether CONFIDENCE.T with current sample SD is more appropriate despite larger resulting interval.

---

### [[Quality Assurance: Process Mean Estimation]]

**Scenario:** A quality engineer estimates the current process mean for a manufacturing dimension where the process standard deviation is well-established from long-term process monitoring.

**Implementation:**
```
=CONFIDENCE.NORM(0.05, process_sd, sample_size)
```

**Business Application:** In mature manufacturing processes, sigma is often well-known from extensive historical data and is considered essentially "known." Daily samples estimate the current mean to detect process shifts. A tight confidence interval indicates the sample mean reliably estimates the true current process average, enabling informed decisions about process adjustments.

**Technical Details:** This application is one of few where "known sigma" is reasonable. SPC (Statistical Process Control) typically establishes sigma from extended baseline data. However, if there's evidence sigma has changed, re-estimation is needed.

---

### [[Sample Size Planning for Surveys]]

**Scenario:** A survey designer determines the sample size needed to achieve a target margin of error for estimating a population mean.

**Implementation:**
```
Required n = (z * sigma / target_margin)^2
Verify: =CONFIDENCE.NORM(alpha, sigma, calculated_n)
```

**Business Application:** Before conducting an expensive survey, determine how many respondents are needed. If satisfaction scores have SD of 15 points and you want margin of +/- 2 points at 95% confidence: n = (1.96 * 15 / 2)^2 = 216.1, so survey at least 217 people. This balances precision against survey costs.

**Technical Details:** Sample size formulas assume simple random sampling. Cluster or stratified sampling requires adjustments. Also consider expected response rate - if 50% response expected, invite 434 people to achieve 217 responses.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2010 and later
- **Legacy function:** CONFIDENCE (still works, same as CONFIDENCE.NORM)
- **Alpha range:** 0 < alpha < 1 (exclusive bounds)
- **Precision:** 15 significant decimal digits
- **Truncation:** Non-integer size is truncated to integer

### Google Sheets

- **Availability:** All versions since launch
- **Legacy function:** CONFIDENCE also supported
- **Alpha range:** 0 < alpha < 1 (exclusive bounds)
- **Precision:** 15 significant decimal digits
- **Behavior:** Identical to Excel

### Key Difference Alert

CONFIDENCE.NORM behaves identically between Excel (2010+) and Google Sheets. Both platforms:
- Use the same z-distribution calculation
- Require the same parameter ranges
- Support the legacy CONFIDENCE function name
- Return identical results

Excel 2007 and earlier only has CONFIDENCE (same as CONFIDENCE.NORM). The ".NORM" suffix was added in Excel 2010 when CONFIDENCE.T was introduced.

## Tips and Best Practices

1. **Verify sigma is truly known.** CONFIDENCE.NORM assumes population SD is known, not estimated. This is rarely true in practice. If sigma is estimated from your sample, use CONFIDENCE.T instead, especially for n < 30.

2. **Remember alpha = 1 - confidence level.** For 95% confidence, alpha = 0.05. For 99% confidence, alpha = 0.01. A common error is using 95 instead of 0.05.

3. **Interpret confidence correctly.** "95% confidence" means 95% of similarly constructed intervals would contain the true mean, not that there's 95% probability the mean is in THIS interval.

4. **Use for sample size planning.** Rearrange the formula to find required n: n = (z * sigma / margin)^2. This helps design studies with desired precision.

5. **Consider practical significance.** A narrow confidence interval isn't always meaningful. An interval of (49.9, 50.1) is precise but may not matter if the practical threshold is 55.

6. **Check assumptions.** The formula assumes random sampling from a normally distributed population (or large n for CLT). Verify these conditions apply.

7. **Report the complete interval.** CONFIDENCE.NORM returns only the margin. Always report the full interval [mean - margin, mean + margin] for clarity.

8. **Compare with CONFIDENCE.T.** For educational purposes, compare NORM and T results to understand how sample size affects which distribution to use.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CONFIDENCE.T]] | Confidence interval using t-distribution | When population SD is unknown and estimated from sample |
| [[CONFIDENCE]] | Legacy function (same as CONFIDENCE.NORM) | For backward compatibility with older workbooks |
| [[NORM.S.INV]] | Inverse standard normal | To find z-values for confidence levels manually |
| [[T.INV.2T]] | Two-tailed t-distribution inverse | For manual t-based interval calculations |

### Commonly Used Together

**[[AVERAGE]]** and **[[STDEV.S]]** - Sample statistics

*Build complete interval from data:*
```
Sample mean: =AVERAGE(data)
Sample SD: =STDEV.S(data)
Margin (if sigma known): =CONFIDENCE.NORM(0.05, known_sigma, COUNT(data))
Lower: =AVERAGE(data) - CONFIDENCE.NORM(0.05, known_sigma, COUNT(data))
Upper: =AVERAGE(data) + CONFIDENCE.NORM(0.05, known_sigma, COUNT(data))
```

---

**[[CONFIDENCE.T]]** - Comparison for unknown sigma

*Compare methods:*
```
Assuming known sigma: =CONFIDENCE.NORM(0.05, sd, n)
Using estimated sigma: =CONFIDENCE.T(0.05, sd, n)
```
When in doubt about whether sigma is "known," compare both and use the more conservative (larger) interval.

---

**[[NORM.S.INV]]** - Manual calculation

*Verify CONFIDENCE.NORM:*
```
z-value: =-NORM.S.INV(alpha/2)
Manual margin: =z * standard_dev / SQRT(size)
Should equal: =CONFIDENCE.NORM(alpha, standard_dev, size)
```

## Official Documentation

- **Microsoft Excel:** [CONFIDENCE.NORM function](https://support.microsoft.com/en-us/office/confidence-norm-function-7cec58a6-85bb-488d-91c3-63828d4fbfd4)
- **Google Sheets:** [CONFIDENCE function](https://support.google.com/docs/answer/3094067) (same as CONFIDENCE.NORM)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | New name; CONFIDENCE still works |
| Excel 2013+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Available as CONFIDENCE; CONFIDENCE.NORM added later |

---

*Last updated: 2026-01-10*
