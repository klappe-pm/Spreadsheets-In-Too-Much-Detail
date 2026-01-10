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
- inverse-functions
- quantile-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- BETAINV
- Inverse Beta Distribution
- Beta Quantile Function
tags:
- statistics
- probability
- distribution-functions
- inverse-distribution
- percentiles
---

# BETA.INV

## Description

BETA.INV returns the inverse of the beta cumulative distribution function, answering the question: "Given a probability, what value from the beta distribution corresponds to that cumulative probability?" If BETA.DIST tells you the probability of being at or below a certain value, BETA.INV works backward to tell you what value corresponds to a specific probability. This makes it essential for calculating percentiles, confidence intervals, and threshold values for beta-distributed variables.

The function takes a probability (between 0 and 1) and returns the corresponding value from a beta distribution with specified alpha and beta shape parameters. For example, if you want to find the 90th percentile of project completion times in a PERT analysis, BETA.INV gives you the duration below which 90% of outcomes fall. The optional A and B parameters allow you to scale the result from the standard [0, 1] interval to any custom range [A, B].

Use BETA.INV when you need to determine threshold values, confidence bounds, or percentiles for bounded continuous data. Common applications include setting service level thresholds (what completion rate puts us in the top 10%?), establishing quality control limits for proportions, determining confidence intervals for success rates, and finding cut-off values in PERT project scheduling. Any time you have a target probability and need to find the corresponding value, BETA.INV is your tool.

Both Excel and Google Sheets implement BETA.INV identically, with the function replacing the legacy BETAINV function starting in Excel 2010. The function uses numerical methods to solve for the inverse since there is no closed-form solution for the beta distribution's inverse CDF. It is the mathematical inverse of BETA.DIST, meaning BETA.DIST(BETA.INV(p, alpha, beta, A, B), alpha, beta, TRUE, A, B) returns p, and vice versa.

## Syntax

> [!info] BETA.INV Syntax
>
> ```excel
> =BETA.INV(probability, alpha, beta, [A], [B])
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `probability` | The probability associated with the beta distribution (between 0 and 1, exclusive) | Yes |
| `alpha` | The first shape parameter of the distribution (must be > 0) | Yes |
| `beta` | The second shape parameter of the distribution (must be > 0) | Yes |
| `A` | Lower bound of the interval (default is 0) | No |
| `B` | Upper bound of the interval (default is 1) | No |

## Examples

```excel
=BETA.INV(0.5, 2, 3)
```
**Result:** `0.3857` (approximately 0.385728811)
**Explanation:** Returns the median (50th percentile) of a beta distribution with alpha=2 and beta=3. Half of values fall below 0.386 and half above. With alpha < beta, the median is below 0.5.

```excel
=BETA.INV(0.9, 2, 3)
```
**Result:** `0.6764` (approximately)
**Explanation:** Returns the 90th percentile. Only 10% of values from this beta distribution exceed 0.6764. Useful for setting upper performance thresholds.

```excel
=BETA.INV(0.1, 2, 3)
```
**Result:** `0.1429` (approximately)
**Explanation:** Returns the 10th percentile. Only 10% of values fall below 0.1429. Useful for identifying lower bounds or underperformance thresholds.

```excel
=BETA.INV(0.5, 5, 2)
```
**Result:** `0.7291` (approximately)
**Explanation:** For a right-skewed distribution (alpha=5 > beta=2), the median is 0.729, above the midpoint of 0.5. The distribution concentrates toward higher values.

```excel
=BETA.INV(0.75, 3, 3, 10, 20)
```
**Result:** `16.02` (approximately)
**Explanation:** Uses custom bounds A=10 and B=20. Returns the 75th percentile scaled to the [10, 20] interval. 75% of values fall at or below 16.02.

```excel
=BETA.INV(0.95, 4, 2, 5, 15)
```
**Result:** `14.04` (approximately)
**Explanation:** In a PERT analysis with optimistic=5 days, pessimistic=15 days, this returns the 95th percentile duration. Only 5% of outcomes would take longer than 14.04 days.

```excel
=BETA.INV(0.025, 2, 5)
```
**Result:** `0.0453` (approximately)
**Explanation:** Returns the 2.5th percentile, useful for calculating the lower bound of a 95% confidence interval.

```excel
=BETA.INV(0.975, 2, 5)
```
**Result:** `0.5527` (approximately)
**Explanation:** Returns the 97.5th percentile, the upper bound of a 95% confidence interval. Combined with the 2.5th percentile, 95% of values fall between 0.0453 and 0.5527.

```excel
=BETA.INV(0.5, 1, 1)
```
**Result:** `0.5`
**Explanation:** When alpha=beta=1, the beta distribution is uniform. The median of a uniform [0,1] distribution is exactly 0.5.

```excel
=BETA.INV(RAND(), 3, 4)
```
**Result:** (Random value each calculation)
**Explanation:** Generates random values following a beta distribution. RAND() provides uniform random probabilities, and BETA.INV transforms them to beta-distributed values. Useful for Monte Carlo simulations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Probability is less than 0 or greater than 1 | Probability must be strictly between 0 and 1 (exclusive) |
| `#NUM!` | Alpha or beta is less than or equal to 0 | Both shape parameters must be positive numbers greater than 0 |
| `#NUM!` | A is greater than or equal to B | Lower bound A must be strictly less than upper bound B |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references containing numbers |
| `#NUM!` | Probability equals exactly 0 or 1 | Use values like 0.0001 or 0.9999 instead of exact boundaries |

## Use Cases

### PERT Schedule Risk Analysis

**Scenario:** A construction project manager needs to determine the task duration that gives 90% confidence of on-time completion. The task has an optimistic estimate of 8 days, pessimistic of 20 days, and most likely of 12 days.

**Implementation:** Calculate the 90th percentile duration:
```excel
=BETA.INV(0.90, 3, 4, 8, 20)
```
Using standard PERT parameters (alpha=3, beta=4 for a slight left skew toward optimistic), this returns approximately 15.4 days. The manager can promise delivery in 16 days with 90% confidence.

**Business Impact:** Setting realistic deadlines based on probability analysis reduces missed commitments and improves stakeholder trust. Rather than using arbitrary "padding," teams can quantify schedule risk and make informed decisions about buffer time.

### Confidence Intervals for Success Rates

**Scenario:** A pharmaceutical company ran clinical trials with 45 successes out of 60 patients (75% success rate). They need to report the 95% confidence interval for the true success rate using Bayesian methods with a uniform prior.

**Implementation:** Using a beta distribution (uniform prior means alpha0=1, beta0=1):
```excel
Lower bound: =BETA.INV(0.025, 1+45, 1+15)  // 46, 16
Upper bound: =BETA.INV(0.975, 1+45, 1+15)
```
This returns approximately [0.628, 0.851], meaning we are 95% confident the true success rate is between 62.8% and 85.1%.

**Business Impact:** Proper confidence intervals prevent overconfidence in point estimates and support regulatory submissions. Stakeholders understand the range of plausible values, not just the observed rate.

### Service Level Threshold Setting

**Scenario:** A call center tracks agent performance scores bounded between 0% and 100%. Historical data suggests scores follow a beta distribution with alpha=4, beta=2. Management wants to set the threshold for "exceptional performance" at the 85th percentile.

**Implementation:** Find the 85th percentile score:
```excel
=BETA.INV(0.85, 4, 2)
```
This returns approximately 0.829, or 82.9%. Agents scoring above 82.9% would be classified as exceptional performers.

**Business Impact:** Data-driven thresholds ensure fair and consistent performance evaluation. The 85th percentile criterion means exactly 15% of agents qualify as exceptional, creating clear, defensible standards tied to actual performance distributions.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | BETA.INV | BETA.INV |
| Legacy support | BETAINV still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Default bounds | A=0, B=1 if omitted | A=0, B=1 if omitted |
| Convergence | Uses iterative numerical methods | Same approach |
| Edge cases | Returns #NUM! for probability at exact 0 or 1 | Same behavior |

Both platforms produce identical results. The numerical methods used to compute the inverse may differ internally, but final results match within floating-point precision.

## Tips and Best Practices

1. **Use BETA.INV with RAND() for simulations:** Generate beta-distributed random numbers with `=BETA.INV(RAND(), alpha, beta, A, B)`. This is the inverse transform sampling method, perfect for Monte Carlo analysis.

2. **Build confidence intervals:** For symmetric confidence intervals, use matching quantiles: 2.5% and 97.5% for 95% CI, 5% and 95% for 90% CI, 0.5% and 99.5% for 99% CI.

3. **Verify with BETA.DIST:** Always check your work by plugging BETA.INV results back into BETA.DIST. The returned probability should match your input probability.

4. **Avoid exact 0 or 1:** These extreme probabilities return errors. Use 0.0001 or 0.9999 for practical purposes when you need extreme quantiles.

5. **Scale parameters match BETA.DIST:** If you are using custom A and B bounds, ensure consistency between BETA.INV and BETA.DIST calls in your analysis.

6. **Remember parameter interpretation:** alpha influences the left side, beta influences the right side. Higher alpha shifts the distribution right; higher beta shifts it left.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BETA.DIST]] | Returns the beta cumulative distribution (inverse of BETA.INV) |
| [[GAMMA.INV]] | Inverse gamma distribution for positive unbounded variables |
| [[NORM.INV]] | Inverse normal distribution for unbounded symmetric data |
| [[BETAINV]] | Legacy version of BETA.INV (compatibility) |

### Commonly Used Together

**[[BETA.DIST]]** - Verify BETA.INV calculations

*Round-trip verification:*
```excel
=BETA.DIST(BETA.INV(0.75, 3, 4), 3, 4, TRUE)
```
Should return exactly 0.75, confirming the inverse relationship.

**[[RAND]]** - Generate random beta-distributed values

*Monte Carlo simulation:*
```excel
=BETA.INV(RAND(), 2, 5, 0, 100)
```
Creates random values following a beta distribution scaled to [0, 100].

**[[PERCENTILE.INC]]** - Compare theoretical to empirical percentiles

*Validation check:*
```excel
Theoretical: =BETA.INV(0.9, 2, 5)
Empirical: =PERCENTILE.INC(observed_data, 0.9)
```
Compare whether your data matches the theoretical distribution.

## Official Documentation

- [Microsoft Excel BETA.INV](https://support.microsoft.com/en-us/office/beta-inv-function-e84cb8aa-8df0-4cf6-9892-83a341d252eb)
- [Google Sheets BETAINV](https://support.google.com/docs/answer/3094019)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced BETA.INV as replacement for BETAINV with improved accuracy |
| Excel 2013 | 2013 | Enhanced numerical precision for extreme probabilities |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as BETAINV |
| Google Sheets | 2010 | Added BETA.INV alias for Excel compatibility |

---

*Last updated: 2026-01-10*
