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
- t-distribution
- inverse-functions
- critical-values
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- TINV
- Inverse t-Distribution
- t Critical Value
tags:
- statistics
- probability
- small-samples
- confidence-intervals
- hypothesis-testing
---

# T.INV

## Description

T.INV returns the left-tailed inverse of the Student's t-distribution, converting a probability into its corresponding t-value for a given degrees of freedom. While T.DIST tells you "what probability corresponds to this t-value?", T.INV works backward to answer "what t-value corresponds to this probability?" This function is essential for finding critical values in hypothesis testing and constructing confidence intervals when working with small samples where the population standard deviation is unknown.

The function takes a probability between 0 and 1 (exclusive) and a degrees of freedom value, then returns the t-value from the t-distribution that would produce that left-tail (cumulative) probability. Unlike the normal distribution which has fixed shape, the t-distribution's shape depends on degrees of freedom: fewer degrees of freedom mean fatter tails and more extreme critical values. As degrees of freedom increase, T.INV results approach the corresponding NORM.S.INV results.

Use T.INV when you need to find critical t-values for one-tailed hypothesis tests, calculate confidence interval bounds for small samples, or determine rejection regions for t-tests. It is fundamental when constructing confidence intervals for means from small samples (where you would use t* instead of z*), setting up one-sided hypothesis tests, and any situation where you have a probability threshold and need the corresponding t-value for a specific sample size.

Both Excel and Google Sheets implement T.INV identically. Note that T.INV returns the left-tail inverse (consistent with the modern statistical function naming), while the legacy TINV function returned the two-tailed inverse, which was a different calculation entirely. For two-tailed applications (like symmetric confidence intervals), use T.INV.2T instead, or calculate the appropriate one-tailed probability (e.g., for 95% two-tailed, use T.INV with probability 0.975).

## Syntax

> [!info] T.INV Syntax
>
> ```excel
> =T.INV(probability, deg_freedom)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `probability` | The probability associated with the left tail of the t-distribution (between 0 and 1, exclusive) | Yes |
| `deg_freedom` | The degrees of freedom (positive integer, typically n-1 for a single sample) | Yes |

## Examples

```excel
=T.INV(0.5, 10)
```
**Result:** `0`
**Explanation:** A probability of 0.5 always returns 0 for any degrees of freedom because the t-distribution is symmetric around 0, with exactly 50% below and 50% above.

```excel
=T.INV(0.975, 10)
```
**Result:** `2.228` (approximately 2.228138852)
**Explanation:** With 10 degrees of freedom, 97.5% of the t-distribution falls below t=2.228. This is the upper critical value for a 95% two-tailed confidence interval with n=11 observations.

```excel
=T.INV(0.95, 10)
```
**Result:** `1.812` (approximately 1.812461123)
**Explanation:** 95% of the distribution falls below t=1.812 for df=10. This is the critical value for a one-tailed test at the 5% significance level.

```excel
=T.INV(0.975, 30)
```
**Result:** `2.042` (approximately 2.042272456)
**Explanation:** With 30 degrees of freedom, the critical value drops to 2.042, closer to the normal distribution's 1.96. This shows how the t-distribution converges toward normal as df increases.

```excel
=T.INV(0.025, 10)
```
**Result:** `-2.228` (approximately -2.228138852)
**Explanation:** The 2.5th percentile for df=10. Note this is the negative of T.INV(0.975, 10), confirming the t-distribution's symmetry. Together, +/-2.228 defines the 95% confidence interval bounds.

```excel
=T.INV(0.99, 5)
```
**Result:** `3.365` (approximately 3.364930245)
**Explanation:** With only 5 degrees of freedom (sample size 6), the 99th percentile t-value is 3.365, much higher than the normal distribution's 2.326, reflecting greater uncertainty with small samples.

```excel
=AVERAGE(A:A) + T.INV(0.975, COUNT(A:A)-1) * STDEV.S(A:A) / SQRT(COUNT(A:A))
```
**Result:** (Varies based on data)
**Explanation:** Calculates the upper bound of a 95% confidence interval for the mean of data in column A. This is the standard formula using the t-distribution for small samples.

```excel
=T.INV(0.975, 1000)
```
**Result:** `1.962` (approximately 1.962339)
**Explanation:** With 1000 degrees of freedom, the t critical value is nearly identical to the normal distribution's 1.96, demonstrating convergence.

```excel
=T.INV(1-0.01, 19)
```
**Result:** `2.539` (approximately 2.539483)
**Explanation:** Calculates the critical value for a one-tailed test at the 1% significance level with 19 degrees of freedom (sample size 20). The formula 1-alpha makes the significance level explicit.

```excel
=IF(B2 > T.INV(0.95, C2), "Reject H0", "Fail to Reject")
```
**Result:** "Reject H0" or "Fail to Reject"
**Explanation:** Compares a calculated t-statistic (B2) to the critical value for a one-tailed test at alpha=0.05 with degrees of freedom in C2. This implements the classical hypothesis testing framework.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric arguments provided | Ensure probability and deg_freedom are numbers or cell references |
| `#NUM!` | Probability is less than or equal to 0 | Probability must be greater than 0 |
| `#NUM!` | Probability is greater than or equal to 1 | Probability must be less than 1 |
| `#NUM!` | Degrees of freedom is less than 1 | Degrees of freedom must be at least 1 |
| Confusion with TINV | Legacy TINV is two-tailed | T.INV is left-tailed; use T.INV.2T for two-tailed or adjust probability |

## Use Cases

### Constructing Confidence Intervals for Small Samples

**Scenario:** A pharmaceutical researcher measured drug effectiveness in 12 patients, finding a mean improvement of 15 points with a sample standard deviation of 6 points. They need a 95% confidence interval for the true mean improvement.

**Implementation:** Calculate the confidence interval using the t-distribution:
```excel
=T.INV(0.975, 11)  // Returns 2.201 for df=11
=15 - 2.201 * (6/SQRT(12))  // Lower bound: 11.19
=15 + 2.201 * (6/SQRT(12))  // Upper bound: 18.81
// 95% CI: (11.19, 18.81)
```

**Business Impact:** The researcher can report that the true mean improvement is between 11.19 and 18.81 points with 95% confidence. Note this uses 2.201 rather than 1.96 because of the small sample size; using the normal approximation would have produced a falsely narrow interval (12.60 to 17.40).

### One-Tailed Hypothesis Testing

**Scenario:** A training program claims to increase productivity by at least 10%. A company tests 25 employees and finds an 8% increase with standard deviation of 12%. At the 5% significance level, is the claim supported?

**Implementation:** Set up and perform the one-tailed t-test:
```excel
// Hypotheses: H0: mu >= 10%, H1: mu < 10%
=(8-10)/(12/SQRT(25))  // t-statistic = -0.833
=T.INV(0.05, 24)  // Critical value = -1.711
// Since -0.833 > -1.711, we fail to reject H0
```

**Business Impact:** The observed 8% increase is not significantly below the claimed 10% at the 5% level (the t-statistic doesn't reach the critical value). However, the company might want more data before fully committing to the training program, as the observed improvement was below the claim.

### Determining Required Sample Size

**Scenario:** A market researcher wants to estimate average customer spending within $5 of the true mean with 95% confidence. Preliminary data suggests a standard deviation of $40. How many customers should they survey?

**Implementation:** Solve for sample size iteratively:
```excel
// Margin of error formula: ME = t * (s/sqrt(n))
// Rearranging: n = (t * s / ME)^2
// Start with normal approximation then iterate
=((1.96 * 40) / 5)^2  // Initial estimate: 245.86, round to 246
=T.INV(0.975, 245)  // t-value for df=245: 1.970
=((1.970 * 40) / 5)^2  // Refined: 248.8, round to 249
```

**Business Impact:** The researcher needs approximately 249 responses. Using the t-distribution rather than the normal distribution adds a few more required responses, which is the appropriate adjustment for the uncertainty in estimating the standard deviation.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | T.INV | T.INV |
| Return value | Left-tail inverse | Left-tail inverse |
| Legacy TINV | Two-tailed inverse (different!) | Two-tailed inverse (different!) |
| Precision | 15 significant digits | 15 significant digits |
| Maximum df | Very large (10^10) | Very large |
| Array formulas | Supported with dynamic arrays | Natively supports arrays |

IMPORTANT: The legacy TINV function returns the two-tailed inverse, which means TINV(0.05, 10) returns the t-value where 5% is split between BOTH tails (2.5% each), equivalent to T.INV(0.975, 10). This is a common source of confusion when updating old spreadsheets.

## Tips and Best Practices

1. **Understand left-tail vs. two-tailed:** T.INV gives the left-tail inverse. For a 95% two-tailed confidence interval, use T.INV(0.975, df) for the upper bound (not T.INV(0.95, df)). For one-tailed tests at 5%, use T.INV(0.95, df).

2. **Know your degrees of freedom:** For one-sample problems, df = n - 1. For two-sample equal variance, df = n1 + n2 - 2. For paired samples, df = n_pairs - 1. Getting df wrong changes your critical value.

3. **Use T.INV.2T for symmetric intervals:** When you need t-values for two-tailed tests or symmetric confidence intervals, T.INV.2T(alpha, df) directly gives you the positive critical value, saving the 1 - alpha/2 calculation.

4. **Compare to normal critical values:** As df increases, T.INV approaches NORM.S.INV. At df=30, the difference is small; at df=120+, it is negligible. This helps verify calculations.

5. **Build confidence intervals correctly:** The formula is: mean +/- T.INV(1-alpha/2, n-1) * (stdev / sqrt(n)). Many errors come from forgetting to divide by sqrt(n) or using the wrong probability.

6. **Handle negative t-values:** T.INV(p, df) for p < 0.5 returns negative values. Due to symmetry, T.INV(p, df) = -T.INV(1-p, df). Use this to verify calculations.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[T.INV.2T]] | Two-tailed inverse of t-distribution (returns positive value for both tails) |
| [[T.DIST]] | Forward function (t-value to probability) |
| [[NORM.S.INV]] | Standard normal inverse (t-distribution with infinite df) |
| [[TINV]] | Legacy two-tailed inverse (different from T.INV!) |
| [[CONFIDENCE.T]] | Directly calculates confidence interval half-width |

### Commonly Used Together

**[[T.DIST]]** - The forward function that T.INV inverts

*Verifying inverse relationship:*
```excel
=T.DIST(T.INV(0.90, 15), 15, TRUE)
```
Returns 0.90, confirming T.INV and T.DIST are inverses.

**[[CONFIDENCE.T]]** - Calculates confidence interval margin of error directly

*Equivalent calculations:*
```excel
=T.INV(0.975, 24) * STDEV.S(A:A) / SQRT(25)  // Manual calculation
=CONFIDENCE.T(0.05, STDEV.S(A:A), 25)  // Direct function
```
Both return the same margin of error for a 95% confidence interval.

**[[AVERAGE]]**, **[[STDEV.S]]**, **[[COUNT]]** - Build confidence intervals

*Complete confidence interval formula:*
```excel
=AVERAGE(A:A) - T.INV(0.975, COUNT(A:A)-1) * STDEV.S(A:A) / SQRT(COUNT(A:A))
=AVERAGE(A:A) + T.INV(0.975, COUNT(A:A)-1) * STDEV.S(A:A) / SQRT(COUNT(A:A))
```
These formulas create the lower and upper bounds of a 95% CI.

## Official Documentation

- [Microsoft Excel T.INV](https://support.microsoft.com/en-us/office/t-inv-function-2908272b-4e61-4942-9df9-a25fec9b0e2e)
- [Google Sheets T.INV](https://support.google.com/docs/answer/6055847)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced T.INV as left-tail inverse; different from legacy TINV |
| Excel 2010 | 2010 | Added T.INV.2T for two-tailed inverse to match legacy TINV behavior |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as TINV (two-tailed) |
| Google Sheets | 2015 | Added T.INV with left-tail behavior for Excel compatibility |
