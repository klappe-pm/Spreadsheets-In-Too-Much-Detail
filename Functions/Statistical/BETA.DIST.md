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
- beta-distribution
- continuous-distributions
- bounded-distributions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- BETADIST
- Beta Distribution
- Beta Probability Distribution
tags:
- statistics
- probability
- distribution-functions
- bounded-random-variables
- bayesian-statistics
---

# BETA.DIST

## Description

BETA.DIST returns the beta probability distribution, a continuous probability distribution defined on the interval [0, 1] or a custom interval [A, B]. The beta distribution is remarkably flexible, capable of taking on a wide variety of shapes depending on its two shape parameters (alpha and beta). Unlike the normal distribution which extends infinitely in both directions, the beta distribution is bounded, making it ideal for modeling proportions, probabilities, percentages, and any random variable constrained to a finite range.

The shape parameters alpha and beta control the distribution's form. When both parameters equal 1, the distribution is uniform (flat). When alpha = beta > 1, the distribution is symmetric and bell-shaped. When alpha > beta, the distribution skews left (more mass toward higher values). When alpha < beta, it skews right (more mass toward lower values). Values less than 1 create U-shaped or J-shaped distributions with mass concentrated at the boundaries. This flexibility makes the beta distribution a "distribution of distributions" - it can approximate many other distributions within a bounded interval.

Use BETA.DIST when modeling percentages, proportions, or probabilities. Classic applications include project completion rates (what is the probability a project is 80% done?), defect rates in manufacturing, click-through rates in marketing, batting averages in sports, and prior distributions in Bayesian statistics. It is also used in PERT analysis for project management, where task durations are bounded between optimistic and pessimistic estimates. Any scenario where your random variable must stay between two known bounds is a candidate for the beta distribution.

Both Excel and Google Sheets support BETA.DIST with identical syntax, replacing the legacy BETADIST function (without the period) introduced in earlier versions. The function can return either the cumulative distribution function (CDF) for probability calculations or the probability density function (PDF) for visualizing the distribution shape. The optional A and B parameters allow you to scale the standard [0, 1] beta distribution to any interval [A, B], which is useful for PERT analysis where task times fall between minimum and maximum values.

## Syntax

> [!info] BETA.DIST Syntax
>
> ```excel
> =BETA.DIST(x, alpha, beta, cumulative, [A], [B])
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The value at which to evaluate the distribution, between A and B | Yes |
| `alpha` | The first shape parameter of the distribution (must be > 0) | Yes |
| `beta` | The second shape parameter of the distribution (must be > 0) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function; FALSE returns the probability density function | Yes |
| `A` | Lower bound of the interval (default is 0) | No |
| `B` | Upper bound of the interval (default is 1) | No |

## Examples

```excel
=BETA.DIST(0.5, 2, 3, TRUE)
```
**Result:** `0.6875`
**Explanation:** Returns the probability that a beta-distributed random variable with alpha=2 and beta=3 is less than or equal to 0.5. About 68.75% of values fall at or below the midpoint. With alpha < beta, the distribution skews toward lower values.

```excel
=BETA.DIST(0.5, 2, 3, FALSE)
```
**Result:** `1.5`
**Explanation:** Returns the probability density at x=0.5 for a beta distribution with alpha=2, beta=3. This is the height of the probability curve at that point. Note that PDF values can exceed 1, unlike probabilities.

```excel
=BETA.DIST(0.25, 5, 2, TRUE)
```
**Result:** `0.0376` (approximately)
**Explanation:** With alpha=5 and beta=2, the distribution is skewed toward higher values. Only about 3.76% of values fall at or below 0.25, confirming the rightward skew.

```excel
=BETA.DIST(0.8, 5, 2, TRUE)
```
**Result:** `0.6630` (approximately)
**Explanation:** For the same distribution (alpha=5, beta=2), about 66.3% of values are 0.8 or less. The remaining 33.7% fall between 0.8 and 1.0, showing the concentration toward higher values.

```excel
=BETA.DIST(15, 3, 4, TRUE, 10, 20)
```
**Result:** `0.6563` (approximately)
**Explanation:** Uses custom bounds A=10 and B=20 instead of the default [0,1]. Returns the probability of being at or below 15 in a beta distribution scaled to the interval [10, 20]. Useful for PERT analysis.

```excel
=BETA.DIST(0.5, 1, 1, FALSE)
```
**Result:** `1`
**Explanation:** When alpha=1 and beta=1, the beta distribution becomes a uniform distribution. The PDF is constant at 1 across the entire [0,1] interval.

```excel
=BETA.DIST(0.9, 0.5, 0.5, FALSE)
```
**Result:** `1.061` (approximately)
**Explanation:** When both alpha and beta are less than 1 (here 0.5 each), the distribution is U-shaped with mass concentrated at both boundaries. This is useful for modeling bimodal phenomena.

```excel
=1 - BETA.DIST(0.7, 3, 3, TRUE)
```
**Result:** `0.1174` (approximately)
**Explanation:** Calculates the probability of being greater than 0.7. With alpha=beta=3, the distribution is symmetric. About 11.74% of values exceed 0.7.

```excel
=BETA.DIST(0.6, 2, 5, TRUE) - BETA.DIST(0.3, 2, 5, TRUE)
```
**Result:** `0.5765` (approximately)
**Explanation:** Finds the probability of falling between 0.3 and 0.6 by subtracting the lower CDF from the upper CDF. About 57.65% of values fall in this range.

```excel
=BETA.DIST(B2, $C$1, $D$1, TRUE)
```
**Result:** (Varies based on cell values)
**Explanation:** Uses cell references for dynamic calculations. B2 contains the x value, C1 contains alpha, and D1 contains beta. Useful for sensitivity analysis or creating distribution charts.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Alpha or beta is less than or equal to 0 | Both shape parameters must be positive numbers greater than 0 |
| `#NUM!` | x is less than A or greater than B | The value x must fall within the bounds [A, B]; check your input value |
| `#NUM!` | A is greater than or equal to B | The lower bound A must be strictly less than the upper bound B |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references containing numbers |
| `Unexpected density > 1` | Not an error - PDF values can exceed 1 | Remember that PDF values are densities, not probabilities; only the area under the curve must equal 1 |

## Use Cases

### PERT Project Duration Estimation

**Scenario:** A project manager uses PERT (Program Evaluation and Review Technique) to estimate task duration. For a software development task, the optimistic time is 5 days, the pessimistic time is 15 days, and the most likely time is 8 days. What is the probability of completing the task within 10 days?

**Implementation:** Using the PERT beta distribution with custom bounds:
```excel
=BETA.DIST(10, 3, 5, TRUE, 5, 15)
```
The alpha and beta parameters (3 and 5) are derived from the PERT assumption that skews toward the most likely value. This returns approximately 0.656, indicating about a 65.6% chance of finishing within 10 days.

**Business Impact:** PERT analysis helps project managers set realistic deadlines, allocate resources appropriately, and communicate probabilistic schedules to stakeholders rather than false certainty. By understanding the distribution of possible outcomes, teams can build appropriate buffer time and manage client expectations.

### Marketing Conversion Rate Analysis

**Scenario:** An e-commerce company has historical data showing their email campaigns convert at rates between 2% and 8%, with most campaigns converting around 4%. They want to model the probability that a new campaign will achieve at least a 5% conversion rate.

**Implementation:** Model the conversion rate with a beta distribution scaled to [0.02, 0.08]:
```excel
=1 - BETA.DIST(0.05, 3, 4, TRUE, 0.02, 0.08)
```
This returns approximately 0.339, indicating about a 34% chance of achieving 5% or better conversion.

**Business Impact:** Understanding conversion rate probability distributions helps marketing teams set realistic goals, calculate expected revenue ranges, and make informed decisions about campaign investments. Rather than planning around a single expected value, they can prepare for the range of likely outcomes.

### Quality Control Defect Rate Modeling

**Scenario:** A manufacturing plant historically produces batches with defect rates between 0% and 5%. Based on process capability studies, the defect rate typically centers around 1.5% but varies. Quality engineers want to calculate the probability of a batch exceeding the 3% threshold that triggers rework.

**Implementation:** Model the defect rate with an appropriate beta distribution:
```excel
=1 - BETA.DIST(0.03, 2, 8, TRUE, 0, 0.05)
```
With parameters suggesting defect rates cluster toward the lower end, this returns approximately 0.168, indicating about a 16.8% chance of exceeding 3%.

**Business Impact:** This analysis helps quality teams allocate inspection resources, set appropriate control limits, and calculate the expected cost of rework. Understanding the probability distribution of quality outcomes supports data-driven decisions about process improvement investments.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | BETA.DIST | BETA.DIST |
| Legacy support | BETADIST still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Default bounds | A=0, B=1 if omitted | A=0, B=1 if omitted |
| Parameter validation | Returns #NUM! for invalid parameters | Same behavior |
| Localization | Function name varies by language | English only |

Both platforms produce identical results within floating-point precision limits. The function behavior is mathematically equivalent between Excel and Google Sheets.

## Tips and Best Practices

1. **Understand the shape parameters:** Alpha controls the left tail, beta controls the right tail. When alpha > beta, the distribution shifts right; when alpha < beta, it shifts left. Equal values create symmetric distributions.

2. **Use custom bounds for real-world scaling:** The A and B parameters let you scale the distribution to any interval. For PERT analysis, set A to the optimistic estimate and B to the pessimistic estimate.

3. **Start with simple parameter values:** alpha=beta=2 gives a nice symmetric distribution. Adjust from there based on your data's characteristics. Larger values create more concentrated (less variable) distributions.

4. **Remember the beta-binomial relationship:** The beta distribution is the conjugate prior for binomial proportions in Bayesian statistics. If you have k successes in n trials, the posterior is Beta(alpha+k, beta+n-k).

5. **Validate your parameters from data:** If you have historical data, calculate the sample mean and variance to estimate appropriate alpha and beta values using the method of moments or maximum likelihood estimation.

6. **Use the PDF (cumulative=FALSE) for visualization:** When creating distribution charts, use FALSE to get the probability density. Use TRUE for actual probability calculations.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BETA.INV]] | Returns the inverse of BETA.DIST (value for a given probability) |
| [[GAMMA.DIST]] | Gamma distribution for positive unbounded variables |
| [[NORM.DIST]] | Normal distribution for unbounded symmetric data |
| [[BETADIST]] | Legacy version of BETA.DIST (compatibility) |

### Commonly Used Together

**[[BETA.INV]]** - The mathematical inverse of BETA.DIST

*Finding the value at a specific percentile:*
```excel
=BETA.INV(0.9, 2, 5, 0, 1)
```
Returns the 90th percentile of the beta distribution. Use BETA.DIST to verify: BETA.DIST(result, 2, 5, TRUE) returns 0.9.

**[[AVERAGE]]** and **[[VAR.S]]** - Estimate parameters from data

*The beta distribution mean is alpha/(alpha+beta):*
```excel
Mean: =C1/(C1+D1)  // where C1=alpha, D1=beta
Variance: =(C1*D1)/(((C1+D1)^2)*(C1+D1+1))
```
Use sample mean and variance to solve for alpha and beta parameters.

**[[IF]]** - Threshold-based decisions

*Quality control decision:*
```excel
=IF(BETA.DIST(0.03, 2, 8, TRUE, 0, 0.05) > 0.95, "Process Capable", "Review Process")
```
Makes decisions based on the probability of meeting quality thresholds.

## Official Documentation

- [Microsoft Excel BETA.DIST](https://support.microsoft.com/en-us/office/beta-dist-function-11188c9c-780a-42c7-ba43-9ecb5a878d31)
- [Google Sheets BETADIST](https://support.google.com/docs/answer/3094019)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced BETA.DIST as replacement for BETADIST with improved accuracy |
| Excel 2013 | 2013 | Enhanced precision for extreme parameter values |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as BETADIST |
| Google Sheets | 2010 | Added BETA.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
