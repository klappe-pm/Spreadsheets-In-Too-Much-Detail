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
- weibull-distribution
- continuous-distributions
- reliability-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- WEIBULL
- Weibull Distribution
- Rosin-Rammler Distribution
tags:
- statistics
- probability
- distribution-functions
- reliability-engineering
- failure-analysis
---

# WEIBULL.DIST

## Description

WEIBULL.DIST returns the Weibull distribution, a versatile continuous probability distribution widely used in reliability engineering, failure analysis, and survival studies. Named after Swedish engineer Waloddi Weibull, this distribution is characterized by two parameters: alpha (shape) and beta (scale). Its flexibility allows it to model increasing, constant, or decreasing failure rates, making it the go-to distribution for product lifetime analysis.

The shape parameter alpha (also called k or beta in some texts - be careful with notation!) determines the failure rate behavior. When alpha < 1, the failure rate decreases over time (early failures, then "burn-in" period ends). When alpha = 1, the failure rate is constant and the Weibull reduces to the exponential distribution (memoryless). When alpha > 1, the failure rate increases over time (wear-out failures). This flexibility makes Weibull applicable to infant mortality, random failures, and aging effects.

Use WEIBULL.DIST for product reliability analysis (time until failure), survival analysis (time until death or event), wind speed modeling (common in wind energy), particle size distributions (in materials science), and any scenario where you need a flexible model for positive continuous data. It excels when failure rates change over time, unlike the exponential distribution's constant rate assumption.

Both Excel and Google Sheets support WEIBULL.DIST with identical syntax, replacing the legacy WEIBULL function. The function can return either the probability density function (PDF) for visualizing the distribution shape, or the cumulative distribution function (CDF) for calculating failure probabilities. Note that Excel uses alpha for shape and beta for scale, which differs from some textbook conventions.

## Syntax

> [!info] WEIBULL.DIST Syntax
>
> ```excel
> =WEIBULL.DIST(x, alpha, beta, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The value at which to evaluate the distribution (must be >= 0) | Yes |
| `alpha` | The shape parameter (must be > 0) | Yes |
| `beta` | The scale parameter (must be > 0) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function; FALSE returns the probability density function | Yes |

## Examples

```excel
=WEIBULL.DIST(100, 2, 150, TRUE)
```
**Result:** `0.3588` (approximately)
**Explanation:** With shape=2 (increasing failure rate) and scale=150, there is about a 35.9% probability of failure by time 100. This models wear-out type failures.

```excel
=WEIBULL.DIST(100, 2, 150, FALSE)
```
**Result:** `0.0057` (approximately)
**Explanation:** Returns the probability density at x=100. This is the height of the PDF curve at that point, useful for plotting the failure time distribution.

```excel
=WEIBULL.DIST(50, 1, 100, TRUE)
```
**Result:** `0.3935` (approximately)
**Explanation:** With alpha=1, Weibull becomes exponential. This equals EXPON.DIST(50, 1/100, TRUE) = 1 - e^(-0.5) = 0.3935.

```excel
=1 - WEIBULL.DIST(500, 3, 400, TRUE)
```
**Result:** `0.1003` (approximately)
**Explanation:** The reliability function R(t) = 1 - F(t). About 10% of units survive past t=500 when the scale is 400 and shape is 3.

```excel
=WEIBULL.DIST(200, 0.5, 100, TRUE)
```
**Result:** `0.7534` (approximately)
**Explanation:** With alpha < 1 (decreasing failure rate), early failures dominate. By t=200, about 75% have already failed, with most failures occurring early.

```excel
=WEIBULL.DIST(150, 2, 150, TRUE)
```
**Result:** `0.6321` (approximately)
**Explanation:** When x equals beta (scale parameter), the CDF is always 1 - 1/e = 0.6321, regardless of alpha. This is a useful property for verification.

```excel
=WEIBULL.DIST(100, 3.5, 200, TRUE) - WEIBULL.DIST(50, 3.5, 200, TRUE)
```
**Result:** `0.0589` (approximately)
**Explanation:** Probability of failure between t=50 and t=100. With high alpha=3.5, failures concentrate later, so early-interval failure probability is low.

```excel
=WEIBULL.DIST(A2, $C$1, $D$1, TRUE)
```
**Result:** (Varies based on cells)
**Explanation:** Uses cell references with fixed alpha (C1) and beta (D1). Useful for creating reliability curves across many time points.

```excel
=(-$D$1^$C$1 * LN(1-0.5))^(1/$C$1)
```
**Result:** Median lifetime
**Explanation:** The inverse Weibull (no built-in function): x = beta * (-ln(1-p))^(1/alpha). This calculates the median (p=0.5) lifetime.

```excel
=$D$1 * EXP(GAMMALN(1+1/$C$1))
```
**Result:** Mean lifetime
**Explanation:** The mean of a Weibull distribution is beta * Gamma(1 + 1/alpha). Unlike symmetric distributions, mean differs from median.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | x is negative | Weibull is only defined for non-negative x values |
| `#NUM!` | alpha is zero or negative | Shape parameter must be positive |
| `#NUM!` | beta is zero or negative | Scale parameter must be positive |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references |
| `Conceptual error` | Confusing alpha and beta meanings | Alpha is shape (failure rate behavior); beta is scale (characteristic life) |

## Use Cases

### Product Reliability Estimation

**Scenario:** An automotive parts manufacturer tests components and finds that failure times follow a Weibull distribution with shape alpha=2.5 (indicating wear-out) and scale beta=80,000 miles. They want to know the probability of failure before the 50,000-mile warranty.

**Implementation:** Calculate warranty failure probability:
```excel
=WEIBULL.DIST(50000, 2.5, 80000, TRUE)
```
This returns approximately 0.165, meaning about 16.5% of parts will fail within warranty.

**Business Impact:** Warranty failure predictions directly affect financial reserves and pricing decisions. A 16.5% warranty claim rate can be costed and built into the product price, or the design can be improved to reduce this rate.

### Wind Energy Assessment

**Scenario:** A wind farm developer analyzes historical wind speed data for a potential site. Wind speeds typically follow a Weibull distribution with shape=2.1 and scale=7 m/s. They need to calculate the probability of wind speeds exceeding 12 m/s (turbine cut-in speed).

**Implementation:** Calculate probability of productive wind:
```excel
=1 - WEIBULL.DIST(12, 2.1, 7, TRUE)
```
This returns approximately 0.048, meaning about 4.8% of the time wind exceeds 12 m/s. (Note: For wind, shape ~2 is common and called the "Rayleigh distribution.")

**Business Impact:** Wind resource assessment determines project viability. Understanding the probability distribution of wind speeds enables accurate energy production forecasts and financial modeling.

### Medical Device Survival Analysis

**Scenario:** A medical device company tracks implant survival times. Data analysis indicates a Weibull distribution with alpha=1.8 (slight wear-out) and beta=10 years. They need to report 5-year and 10-year survival rates.

**Implementation:** Calculate survival probabilities:
```excel
5-year survival: =1 - WEIBULL.DIST(5, 1.8, 10, TRUE)  // Returns ~0.70
10-year survival: =1 - WEIBULL.DIST(10, 1.8, 10, TRUE)  // Returns ~0.37
```
About 70% survive 5 years and 37% survive 10 years.

**Business Impact:** Survival data supports regulatory submissions, physician communication, and patient expectations. Weibull analysis provides defensible, data-driven survival curves.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | WEIBULL.DIST | WEIBULL.DIST |
| Legacy support | WEIBULL still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Inverse function | Not built-in (calculate manually) | Same - not built-in |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Parameter naming | Alpha=shape, Beta=scale | Same convention |

Both platforms produce identical results. Neither provides a built-in inverse Weibull function; use the formula beta * (-LN(1-probability))^(1/alpha) for quantiles.

## Tips and Best Practices

1. **Interpret alpha (shape):** alpha < 1 means decreasing failure rate (burn-in); alpha = 1 means constant rate (exponential); alpha > 1 means increasing rate (wear-out). Most engineered products have alpha > 1.

2. **Remember the scale relationship:** When x = beta, CDF always equals 0.6321 (= 1 - 1/e), regardless of alpha. Beta is the "characteristic life" where 63.2% have failed.

3. **Calculate the inverse manually:** Excel lacks WEIBULL.INV. Use `=beta * (-LN(1-probability))^(1/alpha)` to find quantiles or generate random Weibull values.

4. **Relate to exponential:** When alpha=1, WEIBULL.DIST(x, 1, beta, TRUE) = EXPON.DIST(x, 1/beta, TRUE). Use this to verify calculations.

5. **Fit parameters from data:** Use Excel's Solver or regression on ln(-ln(1-F(x))) vs ln(x) to estimate alpha (slope) and beta (from intercept).

6. **Calculate mean lifetime:** Mean = beta * GAMMA(1 + 1/alpha) using Excel's EXP(GAMMALN(1+1/alpha)) for the gamma function.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[EXPON.DIST]] | Exponential distribution (Weibull with alpha=1) |
| [[GAMMA.DIST]] | Gamma distribution for positive skewed data |
| [[LOGNORM.DIST]] | Lognormal for multiplicative failure processes |
| [[WEIBULL]] | Legacy version of WEIBULL.DIST |

### Commonly Used Together

**[[EXPON.DIST]]** - Special case verification

*When alpha=1:*
```excel
=WEIBULL.DIST(100, 1, 200, TRUE)
=EXPON.DIST(100, 1/200, TRUE)
```
These are equal, confirming the exponential is a special Weibull case.

**[[GAMMALN]]** - Calculate mean lifetime

*Weibull mean formula:*
```excel
=beta * EXP(GAMMALN(1 + 1/alpha))
```
The mean of Weibull(alpha, beta) uses the gamma function.

**[[LN]]** - Calculate inverse (quantile)

*Find time for given failure probability:*
```excel
=beta * (-LN(1-probability))^(1/alpha)
```
Manual inverse Weibull calculation for reliability planning.

**[[RAND]]** - Generate random Weibull values

*Monte Carlo simulation:*
```excel
=beta * (-LN(RAND()))^(1/alpha)
```
Creates random lifetime values following the Weibull distribution.

## Official Documentation

- [Microsoft Excel WEIBULL.DIST](https://support.microsoft.com/en-us/office/weibull-dist-function-4e783c39-9325-49be-bbc9-a83ef82b45db)
- [Google Sheets WEIBULL](https://support.google.com/docs/answer/9116267)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced WEIBULL.DIST as replacement for WEIBULL |
| Excel 2013 | 2013 | Improved numerical precision for extreme parameters |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as WEIBULL |
| Google Sheets | 2010 | Added WEIBULL.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
