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
- gamma-distribution
- inverse-functions
- quantile-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- GAMMAINV
- Inverse Gamma Distribution
- Gamma Quantile Function
tags:
- statistics
- probability
- distribution-functions
- inverse-distribution
- percentiles
---

# GAMMA.INV

## Description

GAMMA.INV returns the inverse of the gamma cumulative distribution function, answering the question: "Given a cumulative probability, what value from the gamma distribution corresponds to that probability?" If GAMMA.DIST tells you the probability of being at or below a certain value, GAMMA.INV works backward to tell you what value corresponds to a specific probability. This is essential for calculating percentiles, confidence bounds, and threshold values for gamma-distributed variables.

The function takes a probability (between 0 and 1) and returns the corresponding value from a gamma distribution with specified alpha (shape) and beta (scale) parameters. For example, if you want to find the 90th percentile of insurance claim sizes modeled by a gamma distribution, GAMMA.INV gives you the claim amount below which 90% of claims fall.

Use GAMMA.INV when you need to determine threshold values for gamma-distributed data, such as finding warranty periods (time by which a certain percentage of products will have failed), setting insurance reserves (claim amount that covers a target percentage of claims), establishing safety margins for processes with gamma-distributed outcomes, or generating random gamma-distributed values for Monte Carlo simulations.

Both Excel and Google Sheets implement GAMMA.INV identically, replacing the legacy GAMMAINV function. The function uses numerical methods to find the inverse since there is no closed-form solution for most gamma distribution parameters. It is the mathematical inverse of GAMMA.DIST, meaning GAMMA.DIST(GAMMA.INV(p, alpha, beta), alpha, beta, TRUE) returns p.

## Syntax

> [!info] GAMMA.INV Syntax
>
> ```excel
> =GAMMA.INV(probability, alpha, beta)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `probability` | The cumulative probability (between 0 and 1, exclusive) | Yes |
| `alpha` | The shape parameter of the distribution (must be > 0) | Yes |
| `beta` | The scale parameter of the distribution (must be > 0) | Yes |

## Examples

```excel
=GAMMA.INV(0.5, 3, 2)
```
**Result:** `5.348` (approximately)
**Explanation:** Returns the median of a gamma distribution with alpha=3 and beta=2. Half of values fall below 5.348 and half above. The mean is 6 (alpha*beta), so the median is below the mean, reflecting right skew.

```excel
=GAMMA.INV(0.9, 3, 2)
```
**Result:** `9.803` (approximately)
**Explanation:** Returns the 90th percentile. Only 10% of values from this gamma distribution exceed 9.803. Useful for setting upper tolerance limits.

```excel
=GAMMA.INV(0.95, 1, 10)
```
**Result:** `29.96` (approximately)
**Explanation:** With alpha=1, the gamma is exponential. This equals -10*LN(1-0.95) = 29.96, the 95th percentile of an exponential distribution with mean 10.

```excel
=GAMMA.INV(0.99, 5, 2)
```
**Result:** `20.09` (approximately)
**Explanation:** The 99th percentile for a gamma with shape=5, scale=2. Mean is 10, so 99% of values are below twice the mean, indicating the tail is not extremely heavy.

```excel
=GAMMA.INV(0.025, 10, 5)
```
**Result:** `26.22` (approximately)
**Explanation:** The 2.5th percentile, useful for lower confidence bounds. Combined with the 97.5th percentile, this defines a 95% confidence interval.

```excel
=GAMMA.INV(0.975, 10, 5)
```
**Result:** `80.74` (approximately)
**Explanation:** The 97.5th percentile. With the 2.5th percentile at 26.22, the middle 95% of this distribution spans from 26.22 to 80.74.

```excel
=GAMMA.INV(0.5, 0.5, 2)
```
**Result:** `0.455` (approximately)
**Explanation:** When alpha < 1, the distribution is highly skewed right with a mode at 0. The median (0.455) is much less than the mean (1.0).

```excel
=GAMMA.INV(0.632, 1, B2)
```
**Result:** B2 (the value in cell B2)
**Explanation:** For an exponential distribution (alpha=1), the 63.2nd percentile always equals beta. This is a useful check: P(X <= beta) = 1 - 1/e = 0.632.

```excel
=GAMMA.INV(RAND(), 4, 3)
```
**Result:** (Random value each calculation)
**Explanation:** Generates random values following a gamma distribution with alpha=4 and beta=3. Uses inverse transform sampling for Monte Carlo simulations.

```excel
=GAMMA.INV(0.5, C2, D2)
```
**Result:** (Varies based on cell values)
**Explanation:** Uses cell references for flexible modeling. C2 contains alpha, D2 contains beta. Returns the median for any gamma distribution specified.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Probability is less than 0 or greater than 1 | Probability must be strictly between 0 and 1 (exclusive) |
| `#NUM!` | Probability equals exactly 0 or 1 | Use 0.0001 or 0.9999 for extreme quantiles |
| `#NUM!` | Alpha is zero or negative | Shape parameter must be positive |
| `#NUM!` | Beta is zero or negative | Scale parameter must be positive |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references containing numbers |

## Use Cases

### Warranty Period Determination

**Scenario:** An electronics manufacturer knows that product lifetimes follow a gamma distribution with alpha=3 and beta=2 years. They want to set a warranty period such that no more than 5% of products fail within the warranty period.

**Implementation:** Find the 5th percentile of the lifetime distribution:
```excel
=GAMMA.INV(0.05, 3, 2)
```
This returns approximately 1.97 years. Setting a 2-year warranty ensures that about 5% of products will fail during the warranty period.

**Business Impact:** Warranty period decisions directly affect customer satisfaction and costs. Using the actual lifetime distribution ensures warranties are both competitive and economically sustainable.

### Insurance Reserve Calculation

**Scenario:** An insurance company models claim severity with a gamma distribution (alpha=2, beta=$10,000). They need to hold reserves sufficient to cover 95% of potential claims.

**Implementation:** Find the 95th percentile of claim amounts:
```excel
=GAMMA.INV(0.95, 2, 10000)
```
This returns approximately $36,889. Reserves should be set to at least this amount to cover 95% of claims.

**Business Impact:** Adequate reserves are critical for solvency. Using gamma distribution percentiles provides a principled approach to reserve setting that satisfies regulators and protects policyholders.

### Process Control Limit Setting

**Scenario:** A manufacturing process has cycle times that follow a gamma distribution with alpha=5 and beta=4 minutes (mean of 20 minutes). Quality engineers want to set an upper control limit at the 99th percentile to identify process anomalies.

**Implementation:** Calculate the 99th percentile cycle time:
```excel
=GAMMA.INV(0.99, 5, 4)
```
This returns approximately 42.6 minutes. Any cycle time exceeding 42.6 minutes should trigger investigation.

**Business Impact:** Control limits based on actual process distributions minimize false alarms while catching genuine anomalies. This improves both quality and operational efficiency.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | GAMMA.INV | GAMMA.INV |
| Legacy support | GAMMAINV still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Parameterization | Scale (beta) | Same - scale parameterization |
| Convergence | Iterative numerical method | Same approach |
| Edge cases | Returns #NUM! for probability at exact 0 or 1 | Same behavior |

Both platforms produce identical results. The numerical methods may differ internally, but outputs match within floating-point precision.

## Tips and Best Practices

1. **Use for Monte Carlo simulations:** Generate gamma-distributed random numbers with `=GAMMA.INV(RAND(), alpha, beta)`. This is the inverse transform sampling method.

2. **Build confidence intervals:** Use matching percentiles for symmetric intervals: 2.5% and 97.5% for 95% CI, 5% and 95% for 90% CI.

3. **Verify with GAMMA.DIST:** Always check: GAMMA.DIST(GAMMA.INV(p, a, b), a, b, TRUE) should return p. Use this to validate calculations.

4. **Relate to chi-squared:** For chi-squared with k degrees of freedom, use GAMMA.INV(p, k/2, 2). This equals CHISQ.INV(p, k).

5. **Check the exponential special case:** When alpha=1, GAMMA.INV(p, 1, beta) = -beta*LN(1-p). Use this for verification.

6. **Avoid exact 0 or 1:** These extreme probabilities return errors. Use 0.0001 or 0.9999 for practical extreme quantiles.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[GAMMA.DIST]] | Returns the gamma distribution (inverse of GAMMA.INV) |
| [[CHISQ.INV]] | Inverse chi-squared (gamma with alpha=df/2, beta=2) |
| [[NORM.INV]] | Inverse normal distribution for symmetric data |
| [[GAMMAINV]] | Legacy version of GAMMA.INV |

### Commonly Used Together

**[[GAMMA.DIST]]** - Verify GAMMA.INV calculations

*Round-trip verification:*
```excel
=GAMMA.DIST(GAMMA.INV(0.75, 3, 4), 3, 4, TRUE)
```
Should return exactly 0.75, confirming the inverse relationship.

**[[RAND]]** - Generate random gamma-distributed values

*Monte Carlo simulation:*
```excel
=GAMMA.INV(RAND(), 2, 5)
```
Creates random values following a gamma distribution with shape=2, scale=5.

**[[GAMMALN]]** - Related gamma function calculations

*For custom probability calculations:*
```excel
=EXP(GAMMALN(alpha))
```
Computes the gamma function, needed for some advanced calculations.

## Official Documentation

- [Microsoft Excel GAMMA.INV](https://support.microsoft.com/en-us/office/gamma-inv-function-74991443-c2b0-4be5-aaab-1aa4d71fbb18)
- [Google Sheets GAMMAINV](https://support.google.com/docs/answer/3094411)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced GAMMA.INV as replacement for GAMMAINV |
| Excel 2013 | 2013 | Improved numerical precision for extreme parameters |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as GAMMAINV |
| Google Sheets | 2010 | Added GAMMA.INV alias for Excel compatibility |

---

*Last updated: 2026-01-10*
