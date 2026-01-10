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
- negative-binomial-distribution
- discrete-distributions
- waiting-time-distributions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- NEGBINOMDIST
- Negative Binomial Distribution
- Pascal Distribution
tags:
- statistics
- probability
- distribution-functions
- quality-control
- sales-forecasting
---

# NEGBINOM.DIST

## Description

NEGBINOM.DIST returns the negative binomial distribution, which models the number of failures that occur before a specified number of successes in a sequence of independent trials. While the binomial distribution asks "How many successes in n trials?", the negative binomial asks "How many trials (or failures) until r successes?" It is sometimes called the Pascal distribution when counting total trials, or the Polya distribution in certain formulations.

The distribution is characterized by two parameters: the number of successes required (r) and the probability of success on each trial (p). Unlike the binomial where trial count is fixed, the negative binomial has a random number of trials with a fixed number of successes. Common formulations count either the number of failures before r successes (Excel's approach) or the total number of trials until r successes.

Use NEGBINOM.DIST for modeling waiting times in terms of trials (how many calls until 5 sales?), quality control (how many items until 3 defects?), insurance (how many policies until 10 claims?), and ecological studies (how many quadrats until finding 5 specimens?). It is particularly useful for overdispersed count data where the variance exceeds the mean, which the Poisson cannot accommodate.

Both Excel and Google Sheets support NEGBINOM.DIST with identical syntax, replacing the legacy NEGBINOMDIST function. The function returns the probability of exactly number_f failures before the number_s-th success. It can return either the probability mass function (PMF) or the cumulative distribution function (CDF).

## Syntax

> [!info] NEGBINOM.DIST Syntax
>
> ```excel
> =NEGBINOM.DIST(number_f, number_s, probability_s, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `number_f` | The number of failures before the number_s-th success | Yes |
| `number_s` | The target number of successes (must be >= 1) | Yes |
| `probability_s` | The probability of success on each trial (between 0 and 1) | Yes |
| `cumulative` | TRUE returns the cumulative probability; FALSE returns the exact probability | Yes |

## Examples

```excel
=NEGBINOM.DIST(5, 3, 0.5, FALSE)
```
**Result:** `0.1172` (approximately)
**Explanation:** Probability of exactly 5 failures before achieving 3 successes, with 50% success rate. About 11.7% of the time, you will have exactly 5 failures before the 3rd success.

```excel
=NEGBINOM.DIST(5, 3, 0.5, TRUE)
```
**Result:** `0.6563` (approximately)
**Explanation:** Probability of 5 or fewer failures before 3 successes. About 65.6% of trials will achieve 3 successes with at most 5 failures (i.e., within 8 total trials).

```excel
=NEGBINOM.DIST(0, 1, 0.8, FALSE)
```
**Result:** `0.8`
**Explanation:** Probability of zero failures before 1 success, with 80% success rate. This simply equals the success probability - you succeed on the first try.

```excel
=NEGBINOM.DIST(10, 5, 0.3, TRUE)
```
**Result:** `0.5154` (approximately)
**Explanation:** With only 30% success rate, probability of reaching 5 successes with 10 or fewer failures. About 51.5% will achieve this within 15 total trials.

```excel
=1 - NEGBINOM.DIST(19, 5, 0.3, TRUE)
```
**Result:** `0.1218` (approximately)
**Explanation:** Probability of needing more than 19 failures (20+ failures, or 25+ total trials) before 5 successes. About 12.2% will need this many attempts.

```excel
=NEGBINOM.DIST(2, 1, 0.1, FALSE)
```
**Result:** `0.081`
**Explanation:** With 10% success rate, probability of exactly 2 failures before 1st success. Equals 0.9 * 0.9 * 0.1 = 0.081 (fail, fail, succeed).

```excel
=NEGBINOM.DIST(5, 10, 0.6, TRUE) - NEGBINOM.DIST(2, 10, 0.6, TRUE)
```
**Result:** `0.3678` (approximately)
**Explanation:** Probability of between 3 and 5 failures before 10 successes. About 36.8% will fall in this range.

```excel
=NEGBINOM.DIST(A2, 5, B2, FALSE)
```
**Result:** (Varies based on cell values)
**Explanation:** Uses cell references for dynamic calculations. A2 contains number of failures, B2 contains success probability. Fixed at 5 required successes.

```excel
=NEGBINOM.DIST(10, 1, 0.1, TRUE)
```
**Result:** `0.6862` (approximately)
**Explanation:** With number_s=1, this is the geometric distribution. Probability of first success within 11 trials (10 failures + 1 success).

```excel
=SUM(NEGBINOM.DIST(ROW(1:21)-1, 5, 0.3, FALSE))
```
**Result:** Approaches 1 as more terms are added
**Explanation:** Sums PMF values for 0-20 failures. Confirms probabilities sum to 1 (theoretically infinite sum, but most mass is captured early).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | number_f is negative | Number of failures must be non-negative (0 or greater) |
| `#NUM!` | number_s is less than 1 | Number of required successes must be at least 1 |
| `#NUM!` | probability_s is not between 0 and 1 | Success probability must be in range (0, 1) |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references |
| `Conceptual error` | Confusing failures with total trials | Excel counts failures, not total trials. Total trials = failures + successes |

## Use Cases

### Sales Call Productivity Analysis

**Scenario:** A sales team knows that each cold call has a 10% probability of resulting in a sale. Management wants to know the probability distribution of calls needed to make 5 sales.

**Implementation:** Calculate probability of making 5 sales with at most 50 failures (55 total calls):
```excel
=NEGBINOM.DIST(50, 5, 0.1, TRUE)
```
This returns approximately 0.758, meaning about 76% of salespeople will achieve 5 sales within 55 calls.

**Business Impact:** Understanding the distribution of calls-to-sales helps set realistic daily call targets and manage expectations. If the 90th percentile requires 80 calls for 5 sales, quotas and training can be calibrated accordingly.

### Quality Control Run Length Analysis

**Scenario:** A manufacturing process produces items with 2% defect rate. Quality inspectors want to know the probability distribution of good items produced before finding 3 defects (which triggers process review).

**Implementation:** Calculate probability of at least 200 good items before 3 defects:
```excel
=1 - NEGBINOM.DIST(199, 3, 0.02, TRUE)
```
This returns approximately 0.323, meaning about 32% of production runs will produce 200+ good items before the third defect triggers review.

**Business Impact:** Run length analysis helps calibrate quality control procedures. If reviews are too frequent (most runs trigger quickly), the defect threshold might be increased. If too infrequent, it might be lowered.

### Insurance Claims Modeling

**Scenario:** An insurance actuary models claims for a policy portfolio. Each month, there is a 15% chance of at least one claim. They want to model months until 5 claims occur for reserve planning.

**Implementation:** Calculate probability of 5 claims occurring within 24 months (with 19 or fewer claim-free months):
```excel
=NEGBINOM.DIST(19, 5, 0.15, TRUE)
```
This returns approximately 0.341, meaning about 34% probability of reaching 5 claims within 2 years.

**Business Impact:** Claims occurrence modeling supports reserve calculations and reinsurance decisions. Understanding the distribution of time-to-claims enables more accurate financial planning.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | NEGBINOM.DIST | NEGBINOM.DIST |
| Legacy support | NEGBINOMDIST still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Parameterization | Counts failures before r-th success | Same approach |
| Cumulative option | Required parameter | Required parameter |

Both platforms produce identical results. The function counts failures before the target number of successes, not total trials.

## Tips and Best Practices

1. **Remember Excel counts failures:** The number_f parameter is failures, not total trials. Total trials = number_f + number_s.

2. **Relate to geometric distribution:** When number_s=1, the negative binomial reduces to the geometric distribution - trials until first success.

3. **Use for overdispersed count data:** When variance exceeds mean (overdispersion), the negative binomial often fits better than Poisson.

4. **Calculate expected failures:** The expected number of failures = number_s * (1-probability_s) / probability_s.

5. **Compare to binomial:** Negative binomial with fixed r successes answers "how many trials?" Binomial with fixed n trials answers "how many successes?"

6. **Use cumulative for "at most" questions:** NEGBINOM.DIST(k, r, p, TRUE) gives probability of at most k failures (completing r successes within k+r trials).

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BINOM.DIST]] | Binomial distribution for fixed number of trials |
| [[POISSON.DIST]] | Poisson for rare events (limiting case) |
| [[HYPGEOM.DIST]] | Hypergeometric for sampling without replacement |
| [[NEGBINOMDIST]] | Legacy version of NEGBINOM.DIST |

### Commonly Used Together

**[[BINOM.DIST]]** - Compare different problem framings

*Different but related questions:*
```excel
P(5 successes in 10 trials): =BINOM.DIST(5, 10, 0.3, FALSE)
P(5 failures before 5 successes): =NEGBINOM.DIST(5, 5, 0.3, FALSE)
```
Both involve 5 successes and 5 failures, but framed differently.

**[[COMBIN]]** - Understand the combinatorial structure

*Manual calculation:*
```excel
=COMBIN(number_f + number_s - 1, number_s - 1) * probability_s^number_s * (1-probability_s)^number_f
```
This equals NEGBINOM.DIST(number_f, number_s, probability_s, FALSE).

**[[AVERAGE]]** - Calculate expected value

*Expected failures:*
```excel
=number_s * (1-probability_s) / probability_s
```
The theoretical mean number of failures before r successes.

## Official Documentation

- [Microsoft Excel NEGBINOM.DIST](https://support.microsoft.com/en-us/office/negbinom-dist-function-c8239f89-c2d0-45bd-b6af-172e570f8599)
- [Google Sheets NEGBINOM.DIST](https://support.google.com/docs/answer/9116089)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced NEGBINOM.DIST with cumulative parameter as replacement for NEGBINOMDIST |
| Excel 2013 | 2013 | Improved numerical stability for extreme parameters |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as NEGBINOMDIST |
| Google Sheets | 2010 | Added NEGBINOM.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
