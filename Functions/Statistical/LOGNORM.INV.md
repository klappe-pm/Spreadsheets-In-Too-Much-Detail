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
- lognormal-distribution
- inverse-functions
- quantile-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- LOGINV
- Inverse Lognormal Distribution
- Lognormal Quantile Function
tags:
- statistics
- probability
- distribution-functions
- inverse-distribution
- financial-modeling
---

# LOGNORM.INV

## Description

LOGNORM.INV returns the inverse of the lognormal cumulative distribution function, answering the question: "Given a cumulative probability, what value from the lognormal distribution corresponds to that probability?" If LOGNORM.DIST tells you the probability of being at or below a certain value, LOGNORM.INV works backward to tell you what value corresponds to a specific probability. This is essential for calculating percentiles, confidence bounds, and threshold values for lognormally-distributed variables.

The function takes a probability (between 0 and 1) and returns the corresponding value from a lognormal distribution with specified mean and standard deviation of the underlying normal distribution (the log-transformed variable). For example, if stock returns are lognormally distributed, LOGNORM.INV can tell you the price level below which 95% of outcomes fall - essential for Value at Risk (VaR) calculations.

Use LOGNORM.INV for financial risk analysis (calculating VaR and expected shortfall), insurance reserve calculations, setting income thresholds (what income level marks the top 1%?), reliability engineering (what lifetime covers 90% of products?), and generating lognormally-distributed random numbers for Monte Carlo simulations. Any time you need to find the value corresponding to a specific percentile of a lognormal distribution, LOGNORM.INV is your tool.

Both Excel and Google Sheets implement LOGNORM.INV identically, replacing the legacy LOGINV function. It is the mathematical inverse of LOGNORM.DIST, meaning LOGNORM.DIST(LOGNORM.INV(p, mean, standard_dev), mean, standard_dev, TRUE) returns p. The function leverages the relationship between lognormal and normal distributions: LOGNORM.INV(p, mean, sd) = EXP(NORM.INV(p, mean, sd)).

## Syntax

> [!info] LOGNORM.INV Syntax
>
> ```excel
> =LOGNORM.INV(probability, mean, standard_dev)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `probability` | The cumulative probability (between 0 and 1, exclusive) | Yes |
| `mean` | The mean of ln(x), the natural log of x | Yes |
| `standard_dev` | The standard deviation of ln(x) (must be > 0) | Yes |

## Examples

```excel
=LOGNORM.INV(0.5, 2, 0.5)
```
**Result:** `7.389` (approximately, equals e^2)
**Explanation:** The median of a lognormal distribution always equals e^mean. With mean=2, the median is e^2 = 7.389. Half of values fall below this point.

```excel
=LOGNORM.INV(0.95, 3, 0.8)
```
**Result:** `53.83` (approximately)
**Explanation:** The 95th percentile of a lognormal with mean=3, sd=0.8 in log-space. Only 5% of values exceed 53.83.

```excel
=LOGNORM.INV(0.05, 3, 0.8)
```
**Result:** `7.50` (approximately)
**Explanation:** The 5th percentile, useful for lower confidence bounds. Combined with the 95th percentile, this defines a 90% confidence interval from 7.50 to 53.83.

```excel
=LOGNORM.INV(0.99, 4, 1)
```
**Result:** `554.1` (approximately)
**Explanation:** The 99th percentile with high variance (sd=1 in log-space). The wide spread reflects the heavy right tail of lognormal distributions with high variance.

```excel
=LOGNORM.INV(0.5, 0, 1)
```
**Result:** `1`
**Explanation:** The standard lognormal (mean=0, sd=1) has median e^0 = 1. This is analogous to the standard normal having median 0.

```excel
=EXP(NORM.INV(0.9, 3, 0.5))
```
**Result:** Same as LOGNORM.INV(0.9, 3, 0.5)
**Explanation:** Demonstrates the relationship between lognormal and normal inverses. LOGNORM.INV equals e raised to the power of NORM.INV.

```excel
=LOGNORM.INV(0.01, 10.5, 0.7)
```
**Result:** `7,133` (approximately)
**Explanation:** The 1st percentile. With mean=10.5 (median around $36,000 if this represents income), about 1% of incomes fall below $7,133.

```excel
=LOGNORM.INV(RAND(), 4, 0.6)
```
**Result:** (Random value each calculation)
**Explanation:** Generates random lognormally-distributed values using inverse transform sampling. Perfect for Monte Carlo simulations of stock prices or incomes.

```excel
=LOGNORM.INV(0.975, B2, C2) - LOGNORM.INV(0.025, B2, C2)
```
**Result:** (Width of 95% CI)
**Explanation:** Calculates the width of the central 95% interval. Uses cell references for mean and standard deviation parameters.

```excel
=LOGNORM.INV(0.99, AVERAGE(LN(A:A)), STDEV.S(LN(A:A)))
```
**Result:** (99th percentile of fitted distribution)
**Explanation:** Fits a lognormal to data and returns the 99th percentile. Useful for VaR calculations or extreme value analysis.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Probability is less than 0 or greater than 1 | Probability must be strictly between 0 and 1 (exclusive) |
| `#NUM!` | Probability equals exactly 0 or 1 | Use 0.0001 or 0.9999 for extreme quantiles |
| `#NUM!` | Standard deviation is zero or negative | Standard deviation must be positive |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references |
| `Conceptual error` | Misunderstanding parameters | Mean and sd refer to ln(X), not X itself |

## Use Cases

### Value at Risk (VaR) Calculation

**Scenario:** A portfolio manager models daily portfolio returns as lognormally distributed with ln(returns) having mean=0.0005 and standard deviation=0.02. They need to calculate the 1-day VaR at the 99% confidence level for a $10 million portfolio.

**Implementation:** Find the 1st percentile of the return distribution:
```excel
=B2 * (1 - LOGNORM.INV(0.01, 0.0005, 0.02))
```
Where B2 contains $10,000,000. The LOGNORM.INV portion returns approximately 0.9540, meaning a 4.6% loss at the 1st percentile, or about $460,000 VaR.

**Business Impact:** VaR calculations are required for regulatory compliance and internal risk management. The lognormal model ensures returns cannot fall below -100% (total loss), making it more realistic than normal distribution assumptions.

### Income Threshold Analysis

**Scenario:** An economist studies income distribution that follows a lognormal pattern. Historical data shows ln(income) has mean=10.8 and standard deviation=0.9. They need to determine the income threshold for the top 10% of earners.

**Implementation:** Find the 90th percentile income:
```excel
=LOGNORM.INV(0.9, 10.8, 0.9)
```
This returns approximately $154,600, meaning 10% of earners make more than this amount.

**Business Impact:** Income percentile analysis supports tax policy design, market segmentation for luxury goods, and salary benchmarking. The lognormal accurately captures the characteristic right skew of income distributions.

### Product Warranty Threshold

**Scenario:** A manufacturer models product lifetimes with a lognormal distribution (ln(hours) mean=8.5, sd=0.6). They want to set a warranty period that covers 95% of failures - meaning only 5% of products should fail after the warranty expires.

**Implementation:** Find the 95th percentile of lifetime:
```excel
=LOGNORM.INV(0.95, 8.5, 0.6)
```
This returns approximately 13,400 hours. Setting warranty at this level means 95% of products will fail during warranty (if they fail at all).

**Business Impact:** Warranty decisions directly affect customer satisfaction, reputation, and costs. Using the actual lifetime distribution ensures warranties are neither too conservative (losing sales) nor too aggressive (excessive warranty claims).

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | LOGNORM.INV | LOGNORM.INV |
| Legacy support | LOGINV still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Parameter interpretation | Mean/SD of ln(X) | Same interpretation |
| Edge cases | Returns #NUM! for probability at exact 0 or 1 | Same behavior |

Both platforms produce identical results. The key is understanding that parameters describe the underlying normal distribution of ln(X).

## Tips and Best Practices

1. **Use the median relationship:** LOGNORM.INV(0.5, mean, sd) always equals e^mean. This is a useful check and provides intuitive interpretation.

2. **Generate random lognormal values:** For Monte Carlo simulations, use `=LOGNORM.INV(RAND(), mean, sd)`. This creates random values following the lognormal distribution.

3. **Build confidence intervals:** Use matching percentiles: 2.5% and 97.5% for 95% CI, 5% and 95% for 90% CI, 0.5% and 99.5% for 99% CI.

4. **Verify via normal relationship:** LOGNORM.INV(p, m, s) = EXP(NORM.INV(p, m, s)). Use this to verify calculations.

5. **Calculate from data:** To fit a lognormal and find percentiles, use AVERAGE(LN(data)) and STDEV.S(LN(data)) as parameters.

6. **Remember the asymmetry:** Lognormal confidence intervals are not symmetric around the median. The upper distance from median to 97.5th percentile exceeds the lower distance.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[LOGNORM.DIST]] | Returns the lognormal distribution (inverse of LOGNORM.INV) |
| [[NORM.INV]] | Inverse normal distribution (log of LOGNORM.INV) |
| [[GAMMA.INV]] | Inverse gamma for alternative positive distributions |
| [[LOGINV]] | Legacy version of LOGNORM.INV |

### Commonly Used Together

**[[LOGNORM.DIST]]** - Verify LOGNORM.INV calculations

*Round-trip verification:*
```excel
=LOGNORM.DIST(LOGNORM.INV(0.75, 3, 0.5), 3, 0.5, TRUE)
```
Should return exactly 0.75, confirming the inverse relationship.

**[[RAND]]** - Generate random lognormal values

*Monte Carlo simulation:*
```excel
=LOGNORM.INV(RAND(), 4, 0.5)
```
Creates random values following a lognormal distribution.

**[[LN]]** and **[[EXP]]** - Work with the normal-lognormal relationship

*Alternative calculation:*
```excel
=EXP(NORM.INV(0.9, mean, sd))
```
This equals LOGNORM.INV(0.9, mean, sd), showing the exponential relationship.

## Official Documentation

- [Microsoft Excel LOGNORM.INV](https://support.microsoft.com/en-us/office/lognorm-inv-function-fe79751a-f1f2-4af8-a0a1-e151b2d4f600)
- [Google Sheets LOGINV](https://support.google.com/docs/answer/3094020)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced LOGNORM.INV as replacement for LOGINV |
| Excel 2013 | 2013 | Improved numerical precision |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as LOGINV |
| Google Sheets | 2010 | Added LOGNORM.INV alias for Excel compatibility |

---

*Last updated: 2026-01-10*
