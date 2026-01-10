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
- continuous-distributions
- waiting-time-distributions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- GAMMADIST
- Gamma Distribution
- Erlang Distribution
tags:
- statistics
- probability
- distribution-functions
- reliability-engineering
- queueing-theory
---

# GAMMA.DIST

## Description

GAMMA.DIST returns the gamma distribution, a versatile continuous probability distribution for modeling positive, right-skewed data. The gamma distribution is a family of distributions characterized by two parameters: alpha (shape) and beta (scale or rate). It generalizes several important distributions: when alpha=1, it becomes the exponential distribution; when alpha is a positive integer, it becomes the Erlang distribution (sum of alpha exponential random variables); and the chi-squared distribution is a special case with alpha=k/2 and beta=2.

The shape parameter alpha controls the distribution's form. When alpha < 1, the distribution is strongly right-skewed with the mode at zero. When alpha = 1, it reduces to the exponential distribution. When alpha > 1, the distribution becomes more bell-shaped with the mode moving away from zero. The scale parameter beta stretches or compresses the distribution along the x-axis. Larger beta values spread the distribution over a wider range.

Use GAMMA.DIST for modeling waiting times (time until multiple events occur), insurance claims (typically right-skewed with a long tail of large claims), rainfall amounts, lifetime data in reliability analysis, and any positive continuous data that shows right skew. It is particularly useful when you need a more flexible distribution than the exponential but want to stay within the same family of distributions for theoretical consistency.

Both Excel and Google Sheets support GAMMA.DIST with identical syntax, replacing the legacy GAMMADIST function. The function can return either the probability density function (PDF) or the cumulative distribution function (CDF). Note that some texts use the rate parameterization (1/beta) instead of scale; Excel uses the scale parameterization where the mean is alpha*beta.

## Syntax

> [!info] GAMMA.DIST Syntax
>
> ```excel
> =GAMMA.DIST(x, alpha, beta, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The value at which to evaluate the distribution (must be >= 0) | Yes |
| `alpha` | The shape parameter of the distribution (must be > 0) | Yes |
| `beta` | The scale parameter of the distribution (must be > 0) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function; FALSE returns the probability density function | Yes |

## Examples

```excel
=GAMMA.DIST(5, 2, 2, TRUE)
```
**Result:** `0.7127` (approximately)
**Explanation:** Returns the probability that a gamma-distributed random variable with alpha=2 and beta=2 is less than or equal to 5. About 71.3% of values fall at or below 5.

```excel
=GAMMA.DIST(5, 2, 2, FALSE)
```
**Result:** `0.0821` (approximately)
**Explanation:** Returns the probability density at x=5. This is the height of the PDF curve at that point, useful for plotting the distribution shape.

```excel
=GAMMA.DIST(10, 1, 10, TRUE)
```
**Result:** `0.6321` (approximately)
**Explanation:** With alpha=1, the gamma distribution reduces to exponential. This equals EXPON.DIST(10, 0.1, TRUE). Mean = alpha*beta = 10.

```excel
=GAMMA.DIST(5, 5, 1, TRUE)
```
**Result:** `0.5595` (approximately)
**Explanation:** The sum of 5 exponential(rate=1) random variables follows this gamma distribution. About 55.95% of such sums are 5 or less.

```excel
=1 - GAMMA.DIST(10, 3, 2, TRUE)
```
**Result:** `0.1847` (approximately)
**Explanation:** Probability of exceeding 10 when mean = alpha*beta = 6. About 18.5% of values exceed 10 in this right-skewed distribution.

```excel
=GAMMA.DIST(20, 10, 2, TRUE) - GAMMA.DIST(15, 10, 2, TRUE)
```
**Result:** `0.4163` (approximately)
**Explanation:** Probability of falling between 15 and 20. With alpha=10, beta=2, the mean is 20 and the distribution is approximately bell-shaped.

```excel
=GAMMA.DIST(0.5, 0.5, 1, FALSE)
```
**Result:** `0.6065` (approximately)
**Explanation:** When alpha < 1, the PDF approaches infinity as x approaches 0. At x=0.5, the density is still quite high, showing the strong right skew.

```excel
=GAMMA.DIST(A2, AVERAGE(A:A)^2/VAR.S(A:A), VAR.S(A:A)/AVERAGE(A:A), TRUE)
```
**Result:** (Varies based on data)
**Explanation:** Fits a gamma distribution to data using the method of moments. Alpha = mean^2/variance, Beta = variance/mean. Useful for estimating parameters from sample data.

```excel
=GAMMA.DIST(100, 9, 10, TRUE)
```
**Result:** `0.5874` (approximately)
**Explanation:** Models waiting time until 9th event when events occur at rate 0.1 per time unit. About 58.7% of the time, the 9th event occurs within 100 time units.

```excel
=GAMMA.DIST(B2, C2, D2, TRUE)
```
**Result:** (Varies based on cell values)
**Explanation:** Uses cell references for flexible modeling. B2 contains x, C2 contains alpha, D2 contains beta. Useful for sensitivity analysis or creating interactive charts.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | x is negative | The gamma distribution is only defined for non-negative x values |
| `#NUM!` | alpha is zero or negative | Shape parameter must be positive |
| `#NUM!` | beta is zero or negative | Scale parameter must be positive |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references containing numbers |
| `Conceptual error` | Confusing scale and rate parameterizations | Excel uses scale (beta); some texts use rate (1/beta). Mean = alpha*beta in Excel |

## Use Cases

### Insurance Claim Size Modeling

**Scenario:** An insurance company models claim sizes for auto accidents. Historical data shows claims average $5,000 with a variance of $25,000,000 (standard deviation of $5,000). They want to calculate the probability of a claim exceeding $15,000.

**Implementation:** Estimate gamma parameters and calculate:
```excel
Alpha: =5000^2/25000000 = 1
Beta: =25000000/5000 = 5000
Probability: =1 - GAMMA.DIST(15000, 1, 5000, TRUE)
```
With alpha=1, this is actually an exponential distribution. The probability of exceeding $15,000 is about 5%.

**Business Impact:** Understanding claim size distributions helps set premiums, determine reinsurance needs, and assess reserve requirements. The gamma distribution's flexibility accommodates the heavy right tail typical of insurance claims.

### Queueing System Analysis

**Scenario:** A hospital emergency room tracks patient wait times. They estimate that each of 4 sequential process steps takes an average of 5 minutes with exponential distribution. They want to know the probability of total processing time exceeding 30 minutes.

**Implementation:** The sum of 4 exponential random variables follows a gamma distribution:
```excel
=1 - GAMMA.DIST(30, 4, 5, TRUE)
```
This returns approximately 0.151, meaning about 15.1% of patients wait more than 30 minutes.

**Business Impact:** Queueing analysis helps hospitals staff appropriately and set patient expectations. Understanding the distribution of total service times enables better capacity planning and service level guarantees.

### Rainfall Amount Prediction

**Scenario:** A water utility models daily rainfall amounts for reservoir planning. Historical data suggests rainfall (on rainy days) follows a gamma distribution with alpha=0.8 and beta=10mm. They need the probability that a rainy day brings more than 20mm.

**Implementation:** Calculate probability of heavy rainfall:
```excel
=1 - GAMMA.DIST(20, 0.8, 10, TRUE)
```
This returns approximately 0.107, indicating about 10.7% of rainy days produce more than 20mm.

**Business Impact:** Rainfall distribution modeling supports water supply planning, flood risk assessment, and infrastructure design. The gamma distribution captures the characteristic pattern of many light-rain days and occasional heavy storms.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | GAMMA.DIST | GAMMA.DIST |
| Legacy support | GAMMADIST still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Parameterization | Scale (beta = scale) | Same - scale parameterization |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Related functions | GAMMA.INV for inverse | Same availability |

Both platforms produce identical results. The scale parameterization (not rate) is used, where mean = alpha * beta and variance = alpha * beta^2.

## Tips and Best Practices

1. **Remember the mean formula:** Mean = alpha * beta. Variance = alpha * beta^2. Use these to estimate parameters from sample data.

2. **Use method of moments:** To fit a gamma distribution to data: alpha = mean^2/variance, beta = variance/mean. This provides quick parameter estimates.

3. **Relate to exponential:** When alpha=1, gamma becomes exponential with scale=beta (or rate=1/beta). Use this relationship to check your work.

4. **Model sums of exponentials:** If you need the distribution of the sum of k exponential random variables with the same rate, use gamma with alpha=k.

5. **Check the shape:** When alpha > 1, the distribution is unimodal with mode at (alpha-1)*beta. When alpha <= 1, the mode is at zero.

6. **Use for lifetime data:** The gamma distribution is popular in reliability engineering because it is flexible, always positive, and includes exponential as a special case.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[GAMMA.INV]] | Returns the inverse of GAMMA.DIST (value for a given probability) |
| [[EXPON.DIST]] | Exponential distribution (gamma with alpha=1) |
| [[CHISQ.DIST]] | Chi-squared distribution (gamma with alpha=k/2, beta=2) |
| [[WEIBULL.DIST]] | Alternative flexible positive distribution |
| [[GAMMADIST]] | Legacy version of GAMMA.DIST |

### Commonly Used Together

**[[GAMMA.INV]]** - Find percentiles and thresholds

*Calculate the 95th percentile:*
```excel
=GAMMA.INV(0.95, 3, 5)
```
Returns the value below which 95% of the distribution falls.

**[[GAMMALN]]** - Logarithm of gamma function

*For large values or custom calculations:*
```excel
=EXP(GAMMALN(alpha))
```
Computes the gamma function, useful for building custom probability formulas.

**[[AVERAGE]]** and **[[VAR.S]]** - Estimate parameters

*Fit gamma to data:*
```excel
Alpha: =AVERAGE(data)^2/VAR.S(data)
Beta: =VAR.S(data)/AVERAGE(data)
```
Method of moments estimation for gamma parameters.

## Official Documentation

- [Microsoft Excel GAMMA.DIST](https://support.microsoft.com/en-us/office/gamma-dist-function-9b6f1538-d11c-4d5f-8966-21f6a2201def)
- [Google Sheets GAMMADIST](https://support.google.com/docs/answer/3094410)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced GAMMA.DIST as replacement for GAMMADIST |
| Excel 2013 | 2013 | Improved numerical precision for extreme parameters |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as GAMMADIST |
| Google Sheets | 2010 | Added GAMMA.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
