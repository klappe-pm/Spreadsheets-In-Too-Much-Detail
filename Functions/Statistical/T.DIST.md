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
- small-sample-statistics
- hypothesis-testing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- TDIST
- Student's t-distribution
- t-Distribution
tags:
- statistics
- probability
- small-samples
- hypothesis-testing
- degrees-of-freedom
---

# T.DIST

## Description

T.DIST returns the Student's t-distribution for a specified value and degrees of freedom. The t-distribution is a probability distribution that resembles the normal distribution but has heavier tails, meaning extreme values are more likely. It was developed by William Sealy Gosset (publishing under the pseudonym "Student") to address a fundamental problem: when you have a small sample, you cannot reliably estimate the population's true variability, so you need a distribution that accounts for this extra uncertainty. As sample size increases, the t-distribution gradually approaches the normal distribution.

The function can return either the cumulative distribution function (CDF) or the probability density function (PDF) depending on the cumulative parameter. When cumulative is TRUE, it returns the probability that a randomly selected value from the t-distribution will be less than or equal to your specified value. This is the left-tail probability, essential for hypothesis testing. When cumulative is FALSE, it returns the probability density at that point, useful for graphing the distribution shape.

Use T.DIST when working with small samples (typically n < 30) where the population standard deviation is unknown and must be estimated from the sample. It is fundamental for t-tests (comparing means), constructing confidence intervals for small samples, regression analysis (testing coefficient significance), and any hypothesis testing situation where you are using the sample standard deviation as an estimate. The degrees of freedom parameter (typically n-1 for a single sample) controls how "fat" the tails are, with fewer degrees of freedom producing heavier tails.

Both Excel and Google Sheets implement T.DIST identically. Excel 2010 introduced T.DIST to replace the older TDIST function, adding important improvements: T.DIST returns the left-tail probability (like the normal distribution functions), whereas the legacy TDIST returned the right-tail probability, causing frequent confusion. Google Sheets supports both naming conventions but TDIST uses the legacy right-tail behavior.

## Syntax

> [!info] T.DIST Syntax
>
> ```excel
> =T.DIST(x, deg_freedom, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The value at which to evaluate the distribution | Yes |
| `deg_freedom` | The degrees of freedom (positive integer, typically n-1) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function (left-tail); FALSE returns the probability density function | Yes |

## Examples

```excel
=T.DIST(0, 10, TRUE)
```
**Result:** `0.5`
**Explanation:** Like the normal distribution, the t-distribution is symmetric around 0. At x=0, exactly 50% of the distribution falls to the left, so the cumulative probability is 0.5.

```excel
=T.DIST(2, 10, TRUE)
```
**Result:** `0.9633` (approximately 0.963305891)
**Explanation:** With 10 degrees of freedom, about 96.33% of the t-distribution falls at or below t=2. Compare to the normal distribution where about 97.72% falls below z=2; the t-distribution has fatter tails.

```excel
=T.DIST(1.96, 30, TRUE)
```
**Result:** `0.9702` (approximately 0.970159627)
**Explanation:** With 30 degrees of freedom, the t-distribution closely approximates the normal, where t=1.96 corresponds to about 97.5%. At df=30, it is 97.02%.

```excel
=T.DIST(1.96, 1000, TRUE)
```
**Result:** `0.9750` (approximately 0.974999)
**Explanation:** With 1000 degrees of freedom, the t-distribution is nearly identical to the normal distribution. This demonstrates how the t-distribution converges to the normal as df increases.

```excel
=T.DIST(-2.228, 10, TRUE)
```
**Result:** `0.025` (approximately 0.0250005)
**Explanation:** For a t-test with 10 degrees of freedom at the 5% significance level (two-tailed), the critical value is about -2.228. This confirms that 2.5% of the distribution falls below this value.

```excel
=T.DIST(0, 5, FALSE)
```
**Result:** `0.3796` (approximately 0.379607)
**Explanation:** The probability density at t=0 for 5 degrees of freedom. This is the peak height of the t-distribution curve, which is lower than the normal distribution's peak (0.399) because probability is spread more into the tails.

```excel
=1 - T.DIST(2.5, 15, TRUE)
```
**Result:** `0.0122` (approximately 0.012246)
**Explanation:** To find the right-tail probability (values greater than 2.5), subtract the left-tail probability from 1. About 1.22% of the distribution with 15 df exceeds t=2.5.

```excel
=2 * (1 - T.DIST(ABS(B2), C2, TRUE))
```
**Result:** (Varies based on B2 and C2)
**Explanation:** Calculates a two-tailed p-value for a t-test. B2 contains the t-statistic and C2 contains the degrees of freedom. The ABS function handles negative t-statistics.

```excel
=T.DIST(2, 5, TRUE) - T.DIST(-2, 5, TRUE)
```
**Result:** `0.8979` (approximately 0.897889)
**Explanation:** About 89.79% of the t-distribution with 5 degrees of freedom falls between -2 and 2. Compare to the normal distribution where about 95.45% falls in this range; the t-distribution's fat tails mean less probability in the center.

```excel
=IF(T.DIST(D2, E2, TRUE) < 0.05, "Significant", "Not Significant")
```
**Result:** "Significant" or "Not Significant"
**Explanation:** Tests whether a t-statistic (D2) with given degrees of freedom (E2) falls in the lower 5% tail. Useful for one-tailed hypothesis tests.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric arguments provided | Ensure x and deg_freedom are numbers or cell references containing numbers |
| `#NUM!` | Degrees of freedom is less than 1 | Degrees of freedom must be at least 1 (for n=2 sample size) |
| `#NUM!` | Degrees of freedom is not an integer | Use a positive integer; Excel truncates decimals but some versions may error |
| Unexpected result | Confused with legacy TDIST | T.DIST returns left-tail probability; TDIST returns right-tail. They are not interchangeable |
| Wrong p-value | Used one-tailed instead of two-tailed | For two-tailed tests, multiply by 2 or use T.DIST.2T |

## Use Cases

### Two-Sample T-Test for Marketing Campaign

**Scenario:** A marketing team tested two email subject lines. Version A had 45 responses with mean $125 purchase and standard deviation $42. Version B had 52 responses with mean $138 purchase and standard deviation $39. They need to determine if the $13 difference is statistically significant.

**Implementation:** Calculate the t-statistic and p-value:
```excel
// t-statistic (simplified equal variance formula)
=($138-$125)/SQRT(($42^2/45)+($39^2/52))  // Returns ~1.577
// degrees of freedom (Welch's approximation)
=((42^2/45+39^2/52)^2)/((42^2/45)^2/44+(39^2/52)^2/51)  // Returns ~89.7
// Two-tailed p-value
=2*(1-T.DIST(1.577, 90, TRUE))  // Returns ~0.118
```

**Business Impact:** With a p-value of 0.118 (above the typical 0.05 threshold), the team cannot conclude Version B is significantly better. They might need more data before committing resources to Version B, or the $13 difference might be due to random chance.

### Regression Coefficient Significance

**Scenario:** A data analyst built a regression model predicting sales from advertising spend. The coefficient for advertising is 2.5 with a standard error of 0.8, and the model was built on 50 data points (48 degrees of freedom for a single predictor). Is advertising significantly related to sales?

**Implementation:** Calculate the t-statistic and p-value:
```excel
=2.5/0.8  // t-statistic = 3.125
=2*(1-T.DIST(3.125, 48, TRUE))  // Two-tailed p-value ~0.003
```

**Business Impact:** With p < 0.01, the advertising coefficient is highly significant. Each dollar spent on advertising is associated with $2.50 in sales, and this relationship is very unlikely to be due to chance. The company can confidently include advertising in their sales forecasting model.

### Quality Control with Small Batches

**Scenario:** A specialty food producer makes small batches of 15 jars per batch. A sample of 8 jars from today's batch has a mean weight of 498g with a standard deviation of 5g. The target is 500g. Is this batch significantly underweight?

**Implementation:** Calculate the one-sample t-test:
```excel
=(498-500)/(5/SQRT(8))  // t-statistic = -1.131
=T.DIST(-1.131, 7, TRUE)  // One-tailed p-value ~0.148
```

**Business Impact:** With p = 0.148, the batch is not significantly underweight at the 5% level. The 2g difference from target could easily be random variation. However, the producer might still monitor trends across batches to catch systematic drift.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | T.DIST | T.DIST (also TDIST with different behavior) |
| Return value | Left-tail probability | Left-tail probability |
| Legacy TDIST | Right-tail probability | Right-tail probability (different from T.DIST!) |
| Precision | 15 significant digits | 15 significant digits |
| Maximum df | Very large (10^10) | Very large |
| Array formulas | Supported with dynamic arrays | Natively supports arrays |

CRITICAL: The legacy TDIST function returns RIGHT-tail probability in both Excel and Google Sheets, while T.DIST returns LEFT-tail probability. This is a common source of errors when migrating old spreadsheets. Always verify which function is being used.

## Tips and Best Practices

1. **Remember the degrees of freedom rule:** For a one-sample t-test, df = n - 1. For a two-sample t-test with equal variances, df = n1 + n2 - 2. For unequal variances (Welch's t-test), use the Welch-Satterthwaite approximation.

2. **Use T.DIST, not TDIST:** The modern T.DIST function returns left-tail probability (consistent with normal distribution functions). The legacy TDIST returns right-tail probability, causing frequent confusion. Stick with T.DIST for new work.

3. **Calculate two-tailed p-values correctly:** For a two-tailed test, use =2*(1-T.DIST(ABS(t), df, TRUE)) or use the dedicated T.DIST.2T function which handles this automatically.

4. **Know when to use t vs. normal:** Use the t-distribution when: (1) sample size is small (n < 30), AND (2) population standard deviation is unknown (using sample stdev). For large samples or known population stdev, the normal distribution is appropriate.

5. **Watch the convergence:** At 30+ degrees of freedom, the t-distribution is very close to normal. At 120+ degrees of freedom, the differences are negligible for most practical purposes.

6. **Consider the T.DIST.RT and T.DIST.2T variants:** Excel provides T.DIST.RT for right-tail probabilities and T.DIST.2T for two-tailed probabilities, which can simplify formulas and reduce errors.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[T.DIST.RT]] | Returns the right-tail probability of the t-distribution |
| [[T.DIST.2T]] | Returns the two-tailed probability of the t-distribution |
| [[T.INV]] | Inverse of T.DIST (probability to t-value) |
| [[T.INV.2T]] | Two-tailed inverse of the t-distribution |
| [[NORM.S.DIST]] | Standard normal distribution (t-distribution with infinite df) |

### Commonly Used Together

**[[T.INV]]** - The inverse function for finding critical t-values

*Finding critical value for 95% confidence interval:*
```excel
=T.INV(0.975, 24)  // Returns 2.064 for df=24, 95% two-tailed
=T.DIST(T.INV(0.975, 24), 24, TRUE)  // Verifies by returning 0.975
```

**[[T.TEST]]** - Performs a complete t-test and returns the p-value directly

*Comparing two data ranges:*
```excel
=T.TEST(A2:A20, B2:B20, 2, 2)  // Two-tailed, equal variance
```
This replaces the manual calculation of t-statistic, df, and T.DIST.

**[[STDEV.S]]** and **[[AVERAGE]]** - Calculate inputs for t-tests

*One-sample t-test formula:*
```excel
=(AVERAGE(A:A) - hypothesized_mean) / (STDEV.S(A:A) / SQRT(COUNT(A:A)))
```
Returns the t-statistic for use with T.DIST.

## Official Documentation

- [Microsoft Excel T.DIST](https://support.microsoft.com/en-us/office/t-dist-function-4329459f-ae91-48c2-bba8-1ead1c6c21b2)
- [Google Sheets T.DIST](https://support.google.com/docs/answer/6055837)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced T.DIST returning left-tail probability; replaced confusing TDIST |
| Excel 2010 | 2010 | Added T.DIST.RT and T.DIST.2T for right-tail and two-tailed probabilities |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as TDIST (right-tail only) |
| Google Sheets | 2015 | Added T.DIST with left-tail probability for Excel compatibility |
