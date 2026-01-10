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
- binomial-distribution
- discrete-distributions
- success-failure-trials
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- BINOMDIST
- Binomial Distribution
- Bernoulli Trials
tags:
- statistics
- probability
- distribution-functions
- discrete-probability
- quality-control
---

# BINOM.DIST

## Description

BINOM.DIST returns the binomial distribution probability for a specified number of successes in a fixed number of independent trials, where each trial has only two possible outcomes (success or failure) with the same probability of success. This is one of the most fundamental and widely-used probability distributions, applicable whenever you count successes in repeated yes/no experiments. Think coin flips, defective items in a batch, customers who make a purchase, or patients who respond to treatment.

The binomial distribution requires three conditions: (1) a fixed number of trials n, (2) each trial is independent of others, (3) each trial has the same probability of success p. The function can return either the probability mass function (PMF) for exactly k successes, or the cumulative distribution function (CDF) for k or fewer successes. The PMF answers "What is the probability of exactly 7 heads in 10 coin flips?" while the CDF answers "What is the probability of 7 or fewer heads?"

Use BINOM.DIST for quality control (probability of finding k defects), marketing (probability of k conversions), clinical trials (probability of k positive responses), reliability engineering (probability of k failures), and any scenario involving counting successes in repeated independent trials. It forms the foundation for hypothesis testing about proportions, A/B testing analysis, and acceptance sampling in manufacturing.

Both Excel and Google Sheets support BINOM.DIST with identical syntax, replacing the legacy BINOMDIST function. The function handles the combinatorial mathematics internally, calculating the number of ways to arrange k successes among n trials and multiplying by the appropriate probabilities. For large n, the binomial distribution can be approximated by the normal distribution (when np and n(1-p) are both greater than 5), but BINOM.DIST gives exact results.

## Syntax

> [!info] BINOM.DIST Syntax
>
> ```excel
> =BINOM.DIST(number_s, trials, probability_s, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `number_s` | The number of successes in trials (must be >= 0 and <= trials) | Yes |
| `trials` | The number of independent trials (must be >= 0) | Yes |
| `probability_s` | The probability of success on each trial (between 0 and 1) | Yes |
| `cumulative` | TRUE returns the cumulative probability (k or fewer); FALSE returns the exact probability | Yes |

## Examples

```excel
=BINOM.DIST(6, 10, 0.5, FALSE)
```
**Result:** `0.2051` (approximately 0.205078125)
**Explanation:** Returns the probability of getting exactly 6 heads in 10 fair coin flips. About 20.5% of the time, you will get exactly 6 heads.

```excel
=BINOM.DIST(6, 10, 0.5, TRUE)
```
**Result:** `0.8281` (approximately)
**Explanation:** Returns the probability of getting 6 or fewer heads in 10 fair coin flips. About 82.8% of the time, you will get 6 or fewer heads.

```excel
=BINOM.DIST(0, 10, 0.1, FALSE)
```
**Result:** `0.3487` (approximately)
**Explanation:** Probability of zero defects in 10 items when each has a 10% defect rate. Even with a 10% defect rate, about 34.9% of batches will have no defects at all.

```excel
=1 - BINOM.DIST(2, 10, 0.1, TRUE)
```
**Result:** `0.0702` (approximately)
**Explanation:** Probability of more than 2 defects in 10 items with 10% defect rate. Only about 7% of batches will have 3 or more defects.

```excel
=BINOM.DIST(8, 20, 0.3, TRUE) - BINOM.DIST(4, 20, 0.3, TRUE)
```
**Result:** `0.5034` (approximately)
**Explanation:** Probability of between 5 and 8 successes (inclusive) in 20 trials with 30% success rate. Subtract cumulative probability at 4 from cumulative at 8.

```excel
=BINOM.DIST(5, 5, 0.9, FALSE)
```
**Result:** `0.5905` (approximately)
**Explanation:** Probability that all 5 trials succeed when success rate is 90%. Equals 0.9^5. Only about 59% of the time will all 5 succeed.

```excel
=BINOM.DIST(0, 100, 0.01, TRUE)
```
**Result:** `0.3660` (approximately)
**Explanation:** Probability of zero failures in 100 trials with 1% failure rate. Even with high reliability, about 63% of batches will have at least one failure.

```excel
=BINOM.DIST(50, 100, 0.5, TRUE)
```
**Result:** `0.5398` (approximately)
**Explanation:** Probability of 50 or fewer heads in 100 fair coin flips. Slightly more than 50% due to the discrete nature of the distribution.

```excel
=BINOM.DIST(ROUND(A1*B1,0), B1, A1, FALSE)
```
**Result:** (Varies based on cell values)
**Explanation:** Calculates probability of the expected number of successes. A1 contains probability, B1 contains trials. Useful for finding the most likely outcome.

```excel
=SUM(BINOM.DIST(ROW(1:11)-1, 10, 0.3, FALSE))
```
**Result:** `1.0`
**Explanation:** Sums probabilities for all possible outcomes (0 through 10 successes). Confirms that total probability equals 1, useful for verification.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | number_s is negative or greater than trials | Successes must be between 0 and the number of trials |
| `#NUM!` | trials is negative | Number of trials must be zero or positive |
| `#NUM!` | probability_s is less than 0 or greater than 1 | Probability must be between 0 and 1 inclusive |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references containing numbers |
| `#NUM!` | Arguments are non-integers (decimals truncated) | The function truncates decimals; use ROUND if you need specific rounding |

## Use Cases

### Quality Control Acceptance Sampling

**Scenario:** A manufacturer receives shipments of 1000 components and samples 50 items for inspection. They want to accept shipments with 2% or fewer defects. If they find 2 or fewer defects in the sample, they accept the shipment. What is the probability of correctly accepting a good shipment?

**Implementation:** Calculate probability of 2 or fewer defects when true defect rate is 2%:
```excel
=BINOM.DIST(2, 50, 0.02, TRUE)
```
This returns approximately 0.922, meaning 92.2% of good shipments will be correctly accepted. The complementary 7.8% represents producer's risk (good shipments rejected).

**Business Impact:** Acceptance sampling balances inspection costs against quality risks. Understanding these probabilities helps negotiate fair quality agreements with suppliers and set appropriate sample sizes to achieve desired confidence levels.

### Marketing Campaign Response Analysis

**Scenario:** An email marketing campaign has a historical click-through rate of 3%. The marketing team sends 500 emails and observes 20 clicks. They want to know if this is unusually high (indicating a better-than-normal campaign).

**Implementation:** Calculate the probability of 20 or more clicks under the normal 3% rate:
```excel
=1 - BINOM.DIST(19, 500, 0.03, TRUE)
```
This returns approximately 0.126, meaning there is a 12.6% chance of seeing 20 or more clicks by random chance. This is not statistically significant at the 5% level.

**Business Impact:** Data-driven marketing requires distinguishing real improvements from random variation. By calculating p-values with BINOM.DIST, teams avoid false positives and can confidently identify genuinely successful campaigns.

### Clinical Trial Power Analysis

**Scenario:** A pharmaceutical company is designing a clinical trial for a new drug expected to have a 70% response rate compared to 50% for the placebo. With 30 patients per group, what is the probability of observing at least 21 responders in the treatment group?

**Implementation:** Calculate probability of 21 or more successes:
```excel
=1 - BINOM.DIST(20, 30, 0.7, TRUE)
```
This returns approximately 0.554, meaning there is about a 55.4% chance of observing 21+ responders if the true rate is 70%.

**Business Impact:** Power analysis helps determine appropriate sample sizes before expensive trials begin. Understanding these probabilities ensures trials are adequately powered to detect meaningful treatment effects.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | BINOM.DIST | BINOM.DIST |
| Legacy support | BINOMDIST still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Maximum trials | Limited by numeric overflow | Similar limitations |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Decimal truncation | Truncates number_s and trials to integers | Same behavior |

Both platforms produce identical results. For very large numbers of trials (thousands), both may lose precision due to the extreme values involved in the calculations.

## Tips and Best Practices

1. **Use cumulative=TRUE for "at most" questions:** To find P(X <= k), use cumulative=TRUE directly. For P(X < k), use BINOM.DIST(k-1, n, p, TRUE).

2. **Use 1 minus cumulative for "at least" questions:** P(X >= k) = 1 - BINOM.DIST(k-1, n, p, TRUE). Do not use 1 - BINOM.DIST(k, n, p, TRUE), which gives P(X > k).

3. **Check independence assumption:** The binomial model assumes trials are independent. If outcomes influence each other (like sampling without replacement from a small population), use HYPGEOM.DIST instead.

4. **For ranges, subtract CDFs:** P(a <= X <= b) = BINOM.DIST(b, n, p, TRUE) - BINOM.DIST(a-1, n, p, TRUE).

5. **Verify with BINOM.DIST.RANGE:** For probability of a range of successes, Excel and Sheets offer BINOM.DIST.RANGE(n, p, s, e) which is more direct than subtracting CDFs.

6. **Expected value is n*p:** The mean of a binomial distribution is trials times probability. Use this to sanity-check your results.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BINOM.INV]] | Returns the inverse of BINOM.DIST (minimum successes for cumulative probability) |
| [[BINOM.DIST.RANGE]] | Returns probability of success count falling within a range |
| [[NEGBINOM.DIST]] | Negative binomial for trials until r successes |
| [[HYPGEOM.DIST]] | Hypergeometric for sampling without replacement |
| [[BINOMDIST]] | Legacy version of BINOM.DIST |

### Commonly Used Together

**[[BINOM.INV]]** - Find critical values for hypothesis tests

*Determine success threshold for significance:*
```excel
=BINOM.INV(100, 0.5, 0.95)
```
Returns 58, the minimum number of successes needed for cumulative probability to exceed 95%.

**[[COMBIN]]** - Understand the underlying combinations

*Calculate binomial coefficient:*
```excel
=COMBIN(10, 6) * 0.5^6 * 0.5^4
```
This equals BINOM.DIST(6, 10, 0.5, FALSE), showing the mathematical relationship.

**[[AVERAGE]]** and **[[STDEV.S]]** - Compare to observed data

*Theoretical vs. observed:*
```excel
Expected: =trials * probability
Std Dev: =SQRT(trials * probability * (1-probability))
```
Compare these to actual sample statistics to assess model fit.

## Official Documentation

- [Microsoft Excel BINOM.DIST](https://support.microsoft.com/en-us/office/binom-dist-function-c5ae37b6-f39c-4be2-94c2-509a1480770c)
- [Google Sheets BINOM.DIST](https://support.google.com/docs/answer/3093990)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced BINOM.DIST as replacement for BINOMDIST |
| Excel 2013 | 2013 | Added BINOM.DIST.RANGE for range probabilities |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as BINOMDIST |
| Google Sheets | 2010 | Added BINOM.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
