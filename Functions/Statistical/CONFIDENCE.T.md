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
- t-distribution
- margin-of-error
- small-samples
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- T Confidence Interval
- Student T Interval
- Confidence T Distribution
- Unknown Sigma Interval
tags:
- statistical
- confidence-interval
- t-distribution
- margin-of-error
- estimation
---

# CONFIDENCE.T

## Description

**CONFIDENCE.T** calculates the margin of error for a confidence interval around a population mean when the population standard deviation is unknown and must be estimated from the sample. This function uses the Student's t-distribution, which has heavier tails than the normal distribution to account for the additional uncertainty introduced by estimating sigma. The result is a wider, more conservative interval than CONFIDENCE.NORM would produce, reflecting the reality that we're uncertain about both the mean AND the standard deviation.

The mathematical formula is: CONFIDENCE.T = t * (s / sqrt(n)), where t is the critical value from the t-distribution with n-1 degrees of freedom, s is the sample standard deviation (used as an estimate for population sigma), and n is the sample size. The t-distribution approaches the normal distribution as sample size increases, so CONFIDENCE.T and CONFIDENCE.NORM converge for large samples. For small samples (typically n < 30), the difference is substantial and CONFIDENCE.T is essential.

**Why t-distribution matters:** When we estimate sigma from our sample, we introduce additional sampling variability. The sample standard deviation is itself a random variable that fluctuates from sample to sample. The t-distribution accounts for this "extra" uncertainty, producing wider intervals that honestly reflect our knowledge. Using the normal distribution when sigma is estimated underestimates uncertainty and leads to confidence intervals that fail to achieve their stated coverage rate.

**Practical implication:** CONFIDENCE.T is almost always the appropriate choice in real-world analysis. True population standard deviations are rarely known. Even when historical data exists, conditions may have changed. Using CONFIDENCE.T is conservative - if sigma actually were known, the interval would be slightly wider than necessary, but this is far better than the alternative of underestimating uncertainty.

## Syntax

> [!f(x)] CONFIDENCE.T Syntax
>
> ```
> =CONFIDENCE.T(alpha, standard_dev, size)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `alpha` | Yes | The significance level, equal to 1 minus the confidence level. For 95% confidence, alpha = 0.05. Must be > 0 and < 1. |
| `standard_dev` | Yes | The sample standard deviation (s), used as an estimate for population sigma. Must be positive. |
| `size` | Yes | The sample size (n). Must be >= 2 (need at least 2 observations to estimate standard deviation). Determines degrees of freedom (n-1). |

### Return Value

Returns the margin of error (half-width of confidence interval) as a positive number. The interval is [sample_mean - margin, sample_mean + margin]. Returns #NUM! if alpha is out of range, standard_dev <= 0, or size < 2.

## Examples

> [!f(x)] CONFIDENCE.T Examples

### Example 1: 95% Confidence Interval with Small Sample
```
=CONFIDENCE.T(0.05, 10, 15)
```
**Result:** 5.53 (approximately)

**Explanation:** With only 15 observations and sample SD of 10, the 95% CI margin is 5.53. Compare to CONFIDENCE.NORM(0.05, 10, 15) = 5.06. The t-based interval is 9% wider.

---

### Example 2: Comparison with Large Sample
```
T (n=100): =CONFIDENCE.T(0.05, 10, 100)   Result: 1.98
NORM:      =CONFIDENCE.NORM(0.05, 10, 100) Result: 1.96
```
**Result:** Nearly identical for large samples

**Explanation:** With n=100, the t-distribution closely approximates normal. The difference (0.02) is negligible. For large samples, either function works.

---

### Example 3: Very Small Sample
```
=CONFIDENCE.T(0.05, 10, 5)
```
**Result:** 12.43 (approximately)

**Explanation:** With only 5 observations, the t-critical value (2.776 for df=4) is much larger than z=1.96. The wide interval reflects genuine uncertainty with tiny samples.

---

### Example 4: 99% Confidence with Moderate Sample
```
=CONFIDENCE.T(0.01, 10, 30)
```
**Result:** 4.98 (approximately)

**Explanation:** Higher confidence level requires wider interval. With df=29, the 99% t-critical value is 2.756, yielding margin of 4.98.

---

### Example 5: Clinical Trial Example
```
=CONFIDENCE.T(0.05, 8.5, 25)
```
**Result:** 3.51 (approximately)

**Explanation:** Blood pressure reduction in a 25-patient trial has sample SD of 8.5 mmHg. The 95% CI for mean reduction has margin +/- 3.51 mmHg.

---

### Example 6: Effect of Sample Size on Margin
```
n=10:  =CONFIDENCE.T(0.05, 10, 10)   Result: 7.15
n=20:  =CONFIDENCE.T(0.05, 10, 20)   Result: 4.68
n=50:  =CONFIDENCE.T(0.05, 10, 50)   Result: 2.85
n=100: =CONFIDENCE.T(0.05, 10, 100)  Result: 1.98
```
**Result:** Margin decreases with larger samples

**Explanation:** Both the decreasing t-critical value and the sqrt(n) in the denominator contribute to narrower intervals as n grows.

---

### Example 7: Building Complete Confidence Interval
```
Sample data in A1:A30
Mean: =AVERAGE(A1:A30)
SD: =STDEV.S(A1:A30)
Margin: =CONFIDENCE.T(0.05, STDEV.S(A1:A30), COUNT(A1:A30))
Lower: =AVERAGE(A1:A30) - CONFIDENCE.T(0.05, STDEV.S(A1:A30), COUNT(A1:A30))
Upper: =AVERAGE(A1:A30) + CONFIDENCE.T(0.05, STDEV.S(A1:A30), COUNT(A1:A30))
```
**Result:** Complete interval from raw data

**Explanation:** This workflow calculates all components from raw data. The interval is [mean - margin, mean + margin].

---

### Example 8: Quality Control Inspection
```
=CONFIDENCE.T(0.05, 0.08, 20)
```
**Result:** 0.037 (approximately)

**Explanation:** Measuring 20 parts with sample SD of 0.08mm, the 95% CI margin for mean dimension is +/- 0.037mm. The interval quantifies measurement precision.

---

### Example 9: Employee Survey Analysis
```
=CONFIDENCE.T(0.05, 1.2, 45)
```
**Result:** 0.361 (approximately)

**Explanation:** Job satisfaction scores (1-5 scale) from 45 employees have sample SD of 1.2. The 95% CI margin of 0.36 points helps interpret whether the mean differs significantly from targets.

---

### Example 10: Minimum Sample Size (n=2)
```
=CONFIDENCE.T(0.05, 10, 2)
```
**Result:** 89.6 (approximately)

**Explanation:** With only 2 observations (df=1), the t-critical value is 12.71. The resulting interval is extremely wide, demonstrating why tiny samples provide little information.

---

### Example 11: Verifying the Calculation
```
Manual t-value: =T.INV.2T(0.05, n-1)
Manual margin: =T.INV.2T(0.05, 29) * 10 / SQRT(30)
CONFIDENCE.T: =CONFIDENCE.T(0.05, 10, 30)
```
**Result:** All approaches give same margin (3.73 approximately)

**Explanation:** CONFIDENCE.T combines the t-critical value lookup and margin calculation. Manual verification uses T.INV.2T for the two-tailed critical value.

---

### Example 12: Financial Returns Analysis
```
=CONFIDENCE.T(0.05, 0.025, 60)
```
**Result:** 0.0065 (approximately)

**Explanation:** 60 months of returns with sample SD of 2.5% monthly. The 95% CI for mean monthly return has margin +/- 0.65%, helping assess whether performance differs from benchmarks.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | alpha <= 0 or alpha >= 1 | Alpha must be strictly between 0 and 1. Use 0.05 for 95% confidence. |
| `#NUM!` | standard_dev <= 0 | Standard deviation must be positive. Check for data errors. |
| `#NUM!` | size < 2 | Need at least 2 observations to estimate SD and have df >= 1. |
| `#VALUE!` | Non-numeric arguments | All parameters must be numbers. Remove text or convert. |
| `#DIV/0!` | size = 1 | Cannot calculate t-interval with single observation. Need n >= 2. |
| `Very wide interval` | Very small sample size | Expected for small n. Either accept wide interval or collect more data. |

## Use Cases

### [[Laboratory Experiment Analysis]]

**Scenario:** A researcher analyzes experimental results from a limited number of trials, needing to estimate the true effect with appropriate uncertainty quantification.

**Implementation:**
```
Margin: =CONFIDENCE.T(0.05, sample_sd, n)
CI: [sample_mean - margin, sample_mean + margin]
```

**Business Application:** Drug efficacy studies, material testing, and process experiments typically have limited samples due to cost or ethical constraints. CONFIDENCE.T provides honest uncertainty estimates, informing whether results are conclusive or if additional testing is needed. A wide interval with a clinically meaningful effect may still support further investigation.

**Technical Details:** Verify normal distribution assumption for small samples using normality tests or Q-Q plots. Non-normal data may require transformation or non-parametric bootstrap intervals. Report degrees of freedom alongside the interval.

---

### [[A/B Test Result Interpretation]]

**Scenario:** A data analyst constructs confidence intervals for treatment effects in A/B tests where the sample size varies and population variance is unknown.

**Implementation:**
```
=CONFIDENCE.T(0.05, pooled_sd, n_treatment)
```

**Business Application:** A/B tests with modest traffic may have only hundreds of conversions. Using CONFIDENCE.T acknowledges that estimated conversion rates have uncertainty. If the confidence interval for conversion difference excludes zero, the treatment effect is statistically significant. Overlapping intervals between groups suggest no significant difference.

**Technical Details:** For comparing two means, more complex formulas apply (two-sample t-interval). CONFIDENCE.T is for single-mean intervals. For two-group comparisons, calculate the difference's standard error and use T.INV.2T for critical values.

---

### [[Financial Performance Estimation]]

**Scenario:** A portfolio manager estimates expected returns and their uncertainty based on historical data, which represents a sample of possible future scenarios.

**Implementation:**
```
Expected return estimate: =AVERAGE(historical_returns)
Margin: =CONFIDENCE.T(0.05, STDEV.S(historical_returns), COUNT(historical_returns))
```

**Business Application:** Historical returns are a sample from an unknown distribution of future possibilities. A 95% confidence interval communicates the range of plausible expected returns given historical variability. Wide intervals counsel caution in projections; narrow intervals (large n, stable returns) support more confident planning.

**Technical Details:** Financial returns often exhibit fat tails (non-normality), which the t-distribution's heavy tails partially accommodate. For extreme non-normality, consider bootstrap confidence intervals or robust methods.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2010 and later (not in earlier versions)
- **Minimum size:** 2 (requires at least df=1)
- **Precision:** 15 significant decimal digits
- **Truncation:** Non-integer size is truncated

### Google Sheets

- **Availability:** All versions since function introduction
- **Minimum size:** 2
- **Precision:** 15 significant decimal digits
- **Behavior:** Identical to Excel

### Key Difference Alert

CONFIDENCE.T behaves identically in Excel (2010+) and Google Sheets. Both platforms:
- Require size >= 2 (df = n-1 >= 1)
- Use the same t-distribution calculations
- Return identical results

**Important:** Excel 2007 and earlier lack CONFIDENCE.T entirely. Use T.INV.2T manually:
```
=T.INV.2T(alpha, size-1) * standard_dev / SQRT(size)
```

## Tips and Best Practices

1. **Use CONFIDENCE.T by default.** Unless you genuinely know the population SD (rare), CONFIDENCE.T is the appropriate choice. It's conservative and honest about uncertainty.

2. **Understand degrees of freedom.** df = n-1 for single-sample intervals. Low df means heavy-tailed t-distribution and wider intervals. df >= 30 is roughly where t approximates normal.

3. **Check sample size requirements.** Size must be >= 2. More importantly, ensure your sample is large enough to make the interval practically useful. Very wide intervals may indicate insufficient data.

4. **Report complete intervals, not just margins.** State "mean = 45.2, 95% CI [42.3, 48.1]" rather than "margin = 2.9". Context matters.

5. **Verify normality for small samples.** The t-interval assumes normally distributed data (or large n). For small samples from non-normal distributions, bootstrap methods may be more appropriate.

6. **Compare with CONFIDENCE.NORM.** For educational purposes, compare T and NORM results to see how sample size affects the choice of distribution.

7. **Use for sample size planning.** Rearrange to find required n for target margin, remembering that the t-critical value also depends on n. Iteration or tables help.

8. **Consider practical significance.** A narrow interval is good, but ask whether the range of plausible values is practically meaningful for decision-making.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CONFIDENCE.NORM]] | Normal distribution confidence interval | Only when population SD is truly known (rare) |
| [[T.INV.2T]] | Two-tailed t-distribution inverse | For manual interval calculations |
| [[T.DIST.2T]] | Two-tailed t-distribution probability | For hypothesis testing related to intervals |
| [[STDEV.S]] | Sample standard deviation | To estimate SD for use with CONFIDENCE.T |

### Commonly Used Together

**[[AVERAGE]]** and **[[STDEV.S]]** - Sample statistics

*Complete interval from data:*
```
Mean: =AVERAGE(data)
SD: =STDEV.S(data)
n: =COUNT(data)
Margin: =CONFIDENCE.T(0.05, STDEV.S(data), COUNT(data))
Lower: =AVERAGE(data) - CONFIDENCE.T(0.05, STDEV.S(data), COUNT(data))
Upper: =AVERAGE(data) + CONFIDENCE.T(0.05, STDEV.S(data), COUNT(data))
```

---

**[[T.INV.2T]]** - Manual verification

*Verify CONFIDENCE.T:*
```
t-critical: =T.INV.2T(alpha, n-1)
Manual: =T.INV.2T(0.05, 29) * sd / SQRT(30)
Should equal: =CONFIDENCE.T(0.05, sd, 30)
```

---

**[[T.TEST]]** - Related hypothesis testing

*Test whether mean differs from target:*
```
Confidence interval approach: Check if target is outside [mean - margin, mean + margin]
Hypothesis test approach: =T.TEST(data, target_range, 2, 1)
```
Both approaches are mathematically equivalent.

## Official Documentation

- **Microsoft Excel:** [CONFIDENCE.T function](https://support.microsoft.com/en-us/office/confidence-t-function-e8edd88c-e48d-4e18-963e-28e0b9ac0e73)
- **Google Sheets:** [CONFIDENCE.T function](https://support.google.com/docs/answer/9116217)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | New function for t-based intervals |
| Excel 2013+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | 2019 (approximately) | Added for Excel compatibility |

---

*Last updated: 2026-01-10*
