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
- inverse-functions
- quantile-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- CRITBINOM
- Inverse Binomial Distribution
- Binomial Quantile
tags:
- statistics
- probability
- distribution-functions
- inverse-distribution
- hypothesis-testing
---

# BINOM.INV

## Description

BINOM.INV returns the smallest value for which the cumulative binomial distribution is greater than or equal to a specified probability. In simpler terms, it answers: "How many successes do I need to achieve to reach or exceed a target cumulative probability?" This is the inverse of BINOM.DIST with cumulative=TRUE, converting a probability into a count of successes.

Unlike continuous distribution inverses that return exact values, BINOM.INV returns integers because the binomial distribution is discrete. If you specify a probability of 0.75, BINOM.INV finds the smallest number of successes k such that P(X <= k) >= 0.75. This makes it perfect for setting thresholds, establishing critical values for hypothesis tests, and determining minimum required successes to achieve confidence levels.

Use BINOM.INV for quality control (how many defects can we tolerate and still be 95% confident the batch is acceptable?), setting performance quotas (how many sales constitute top 10% performance?), determining sample sizes, and establishing pass/fail criteria. It is essential for anyone designing acceptance sampling plans, setting control limits, or conducting binomial hypothesis tests.

Both Excel and Google Sheets implement BINOM.INV identically, with the function replacing the legacy CRITBINOM function (short for "critical binomial"). The function uses an algorithm that searches for the smallest integer satisfying the probability criterion, making it computationally efficient even for large numbers of trials.

## Syntax

> [!info] BINOM.INV Syntax
>
> ```excel
> =BINOM.INV(trials, probability_s, alpha)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `trials` | The number of independent trials (must be >= 0) | Yes |
| `probability_s` | The probability of success on each trial (between 0 and 1) | Yes |
| `alpha` | The cumulative probability threshold (between 0 and 1) | Yes |

## Examples

```excel
=BINOM.INV(10, 0.5, 0.5)
```
**Result:** `5`
**Explanation:** In 10 fair coin flips, you need at least 5 heads for the cumulative probability to reach or exceed 50%. This is the median of the distribution.

```excel
=BINOM.INV(10, 0.5, 0.95)
```
**Result:** `8`
**Explanation:** You need at least 8 heads in 10 fair coin flips for the cumulative probability to exceed 95%. Getting 8 or more heads is unusual (happens less than 5% of the time in the upper tail).

```excel
=BINOM.INV(100, 0.3, 0.9)
```
**Result:** `36`
**Explanation:** In 100 trials with 30% success rate, you need at least 36 successes to reach the 90th percentile. Only 10% of the time will you get more than 36 successes.

```excel
=BINOM.INV(20, 0.1, 0.05)
```
**Result:** `0`
**Explanation:** With 20 trials and 10% success rate, even 0 successes has cumulative probability greater than 5% (actually about 12%). Returns 0 as the threshold.

```excel
=BINOM.INV(50, 0.02, 0.99)
```
**Result:** `4`
**Explanation:** In 50 trials with 2% defect rate, you need to find at most 4 defects to be 99% confident. This helps set acceptance criteria for quality inspection.

```excel
=BINOM.INV(1000, 0.05, 0.975) - BINOM.INV(1000, 0.05, 0.025)
```
**Result:** `23` (approximately)
**Explanation:** Calculates the width of the middle 95% of the distribution. With 1000 trials at 5%, the central 95% spans about 23 values.

```excel
=BINOM.INV(B2, C2, 0.95)
```
**Result:** (Varies based on cell values)
**Explanation:** Uses cell references for dynamic calculations. B2 contains trials, C2 contains probability. Returns the 95th percentile for any binomial distribution.

```excel
=BINOM.INV(100, 0.5, 0.5) - 0.5*100
```
**Result:** `0`
**Explanation:** Verifies that the median equals the expected value (n*p) when both n and p make n*p an integer. For a symmetric binomial (p=0.5), median equals mean.

```excel
=BINOM.INV(ROWS(A:A)-1, COUNTIF(A:A,">0")/ROWS(A:A), 0.9)
```
**Result:** (Varies based on data)
**Explanation:** Estimates the 90th percentile from actual data. Counts positive values to estimate probability, then calculates the expected 90th percentile.

```excel
=IF(B5 > BINOM.INV(100, 0.5, 0.975), "Significant", "Not Significant")
```
**Result:** "Significant" or "Not Significant"
**Explanation:** Simple hypothesis test. If observed successes B5 exceeds the 97.5th percentile of expected successes under null hypothesis, the result is significant.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | trials is negative | Number of trials must be zero or positive |
| `#NUM!` | probability_s is less than 0 or greater than 1 | Probability must be between 0 and 1 inclusive |
| `#NUM!` | alpha is less than 0 or greater than 1 | Alpha must be between 0 and 1 |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references containing numbers |
| `Returns 0 unexpectedly` | Alpha is very small | Even 0 successes may have cumulative probability exceeding alpha |

## Use Cases

### Acceptance Sampling Plan Design

**Scenario:** A quality manager needs to design an inspection plan for incoming materials. Batches should have no more than 2% defects. They sample 100 items and want to establish an acceptance number (maximum defects allowed) that gives 95% probability of accepting a good batch.

**Implementation:** Find the acceptance number for 2% defect rate:
```excel
=BINOM.INV(100, 0.02, 0.95)
```
This returns 4, meaning the manager should accept batches with 4 or fewer defects. For a 2% defect rate, about 95% of good batches will have 4 or fewer defects in a sample of 100.

**Business Impact:** Well-designed sampling plans balance inspection costs against quality risks. Using BINOM.INV ensures that acceptance criteria are statistically justified, not arbitrary, reducing both false rejections of good batches and false acceptances of bad ones.

### Sales Quota Setting

**Scenario:** A sales manager knows that each sales call has a 20% probability of closing a deal. With 50 calls per week expected, they want to set a quota that identifies the top 10% of performers.

**Implementation:** Find the 90th percentile of expected sales:
```excel
=BINOM.INV(50, 0.20, 0.90)
```
This returns 14, meaning salespeople who close 14 or more deals per week are in the top 10%. The quota should be set at 14 to identify exceptional performers.

**Business Impact:** Data-driven quotas are fair and defensible. Rather than arbitrary targets, this approach ensures quotas reflect actual probability distributions, improving morale by setting achievable yet challenging goals.

### Clinical Trial Stopping Rules

**Scenario:** A clinical trial tests a treatment with expected 60% response rate. After enrolling 30 patients, researchers want to stop early for success if the observed response rate is significantly higher than expected (p < 0.05).

**Implementation:** Determine the critical value for early stopping:
```excel
=BINOM.INV(30, 0.6, 0.95) + 1
```
This returns 23 (22 is the 95th percentile, so 23+ responses would be in the top 5%). If 23 or more patients respond, the trial can stop early with significant evidence of efficacy.

**Business Impact:** Adaptive trial designs save time and resources while protecting patients. BINOM.INV helps establish clear, pre-specified stopping rules that maintain statistical rigor.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | BINOM.INV | BINOM.INV |
| Legacy support | CRITBINOM still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Return type | Always returns integer | Always returns integer |
| Edge cases | Returns 0 for very small alpha | Same behavior |
| Maximum trials | Limited by numeric precision | Similar limitations |

Both platforms produce identical results. The function is well-standardized across implementations.

## Tips and Best Practices

1. **Remember it returns integers:** Unlike continuous inverse functions, BINOM.INV always returns whole numbers because you cannot have 6.5 successes.

2. **Use for one-tailed tests:** BINOM.INV(n, p, 0.95) gives the upper critical value for a one-tailed test. For two-tailed, use 0.975 for upper and 0.025 for lower.

3. **Verify with BINOM.DIST:** Check that BINOM.DIST(BINOM.INV(n, p, alpha), n, p, TRUE) >= alpha. This confirms the inverse relationship.

4. **For lower tails, use directly:** BINOM.INV(n, p, 0.05) gives you the value where P(X <= k) just exceeds 5%, useful for lower control limits.

5. **Account for discrete jumps:** Because the distribution is discrete, the cumulative probability may "jump" past your alpha. The exact probability may be higher than alpha, not exactly equal.

6. **Combine with BINOM.DIST for exact probabilities:** After finding the critical value with BINOM.INV, use BINOM.DIST to get the exact probability at that value.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BINOM.DIST]] | Returns the binomial probability distribution (inverse of BINOM.INV) |
| [[NORM.INV]] | Inverse normal distribution for continuous approximation |
| [[NEGBINOM.DIST]] | Negative binomial distribution for trials until r successes |
| [[CRITBINOM]] | Legacy version of BINOM.INV |

### Commonly Used Together

**[[BINOM.DIST]]** - Verify critical values

*Check the cumulative probability:*
```excel
=BINOM.DIST(BINOM.INV(100, 0.3, 0.95), 100, 0.3, TRUE)
```
Returns a probability >= 0.95, confirming the inverse calculation.

**[[IF]]** - Build decision rules

*Hypothesis test implementation:*
```excel
=IF(observed > BINOM.INV(n, p0, 0.95), "Reject H0", "Fail to Reject")
```
Creates an automated hypothesis test comparing observed to critical value.

**[[RAND]]** - Generate random binomial outcomes

*Simulation via inverse transform:*
```excel
=BINOM.INV(n, p, RAND())
```
Generates random values following the binomial distribution.

## Official Documentation

- [Microsoft Excel BINOM.INV](https://support.microsoft.com/en-us/office/binom-inv-function-80a0370c-95a6-4571-a7a0-fa93aaf24dd4)
- [Google Sheets BINOM.INV](https://support.google.com/docs/answer/9116016)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced BINOM.INV as replacement for CRITBINOM |
| Excel 2013 | 2013 | Improved numerical stability for large trials |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as CRITBINOM |
| Google Sheets | 2010 | Added BINOM.INV alias for Excel compatibility |

---

*Last updated: 2026-01-10*
