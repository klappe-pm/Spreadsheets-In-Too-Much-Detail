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
- poisson-distribution
- discrete-distributions
- count-data
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- POISSON
- Poisson Distribution
- Count Distribution
tags:
- statistics
- probability
- distribution-functions
- queueing-theory
- rare-events
---

# POISSON.DIST

## Description

POISSON.DIST returns the Poisson distribution, a discrete probability distribution that models the number of events occurring in a fixed interval of time or space, when events happen independently at a known average rate. Named after French mathematician Simeon Denis Poisson, it answers questions like "What is the probability of receiving exactly 5 customer calls in the next hour?" or "How likely is it that 3 or fewer errors occur on this page?"

The distribution is characterized by a single parameter lambda (the mean), which represents both the expected value and variance of the distribution. This elegant property means that if you know the average rate of occurrence, you can fully describe the distribution. The Poisson is appropriate when events are independent, occur at a constant average rate, and cannot happen simultaneously.

Use POISSON.DIST for modeling arrivals (customers, calls, website visits), defects (errors per page, flaws per unit area), accidents (per mile, per year), natural phenomena (radioactive decay, meteor strikes), and any count data arising from random, independent events at a constant rate. It forms the foundation of queueing theory and is essential for operations management, quality control, and risk assessment.

Both Excel and Google Sheets support POISSON.DIST with identical syntax, replacing the legacy POISSON function. The function can return either the probability mass function (PMF) for exactly k events, or the cumulative distribution function (CDF) for k or fewer events. For large lambda, the Poisson is well-approximated by a normal distribution with mean and variance both equal to lambda.

## Syntax

> [!info] POISSON.DIST Syntax
>
> ```excel
> =POISSON.DIST(x, mean, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The number of events (must be a non-negative integer) | Yes |
| `mean` | The expected number of events (lambda, must be > 0) | Yes |
| `cumulative` | TRUE returns the cumulative probability (k or fewer); FALSE returns the exact probability | Yes |

## Examples

```excel
=POISSON.DIST(5, 4, FALSE)
```
**Result:** `0.1563` (approximately)
**Explanation:** Probability of exactly 5 events when the average is 4 events. About 15.6% of periods will have exactly 5 events.

```excel
=POISSON.DIST(5, 4, TRUE)
```
**Result:** `0.7851` (approximately)
**Explanation:** Probability of 5 or fewer events when the average is 4. About 78.5% of periods will have at most 5 events.

```excel
=POISSON.DIST(0, 3, FALSE)
```
**Result:** `0.0498` (approximately, equals e^-3)
**Explanation:** Probability of zero events when average is 3. Even with an average of 3 events, about 5% of periods will have no events at all.

```excel
=1 - POISSON.DIST(10, 5, TRUE)
```
**Result:** `0.0137` (approximately)
**Explanation:** Probability of more than 10 events when average is 5. Only about 1.4% of periods will have 11+ events - twice the average is quite rare.

```excel
=POISSON.DIST(2, 2, FALSE) + POISSON.DIST(2, 2, FALSE)
```
**Result:** Incorrect approach
**Explanation:** This doubles the probability rather than finding a range. For ranges, use CDF subtraction.

```excel
=POISSON.DIST(5, 3, TRUE) - POISSON.DIST(2, 3, TRUE)
```
**Result:** `0.5578` (approximately)
**Explanation:** Probability of 3, 4, or 5 events when average is 3. Subtracting CDFs gives the probability of values in the range (3 to 5 inclusive).

```excel
=POISSON.DIST(0, 0.1, FALSE)
```
**Result:** `0.9048` (approximately)
**Explanation:** For rare events (average 0.1 per period), about 90.5% of periods have zero events. This is the "rare events" case Poisson handles well.

```excel
=POISSON.DIST(100, 100, TRUE)
```
**Result:** `0.5266` (approximately)
**Explanation:** With large lambda, the distribution becomes symmetric. About 52.7% of values fall at or below the mean, close to 50%.

```excel
=POISSON.DIST(ROUND(A2*B2,0), A2*B2, FALSE)
```
**Result:** (Varies based on cells)
**Explanation:** A2 is rate per unit, B2 is number of units. Calculates probability of the expected count, useful for understanding the mode.

```excel
=SUMPRODUCT((ROW(1:21)-1)*POISSON.DIST(ROW(1:21)-1, 5, FALSE))
```
**Result:** `5` (approximately)
**Explanation:** Calculates the expected value by summing x*P(x). Confirms that for Poisson, E[X] = lambda = 5.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | x is negative | Number of events must be non-negative (0 or greater) |
| `#NUM!` | mean is zero or negative | Lambda must be positive (use very small positive value for rare events) |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references |
| `Conceptual error` | Using non-integer x | The function truncates to integer, but x should conceptually be a count |
| `Wrong rate` | Mismatched time units | Ensure lambda matches the time period of interest |

## Use Cases

### Call Center Staffing

**Scenario:** A call center receives an average of 8 calls per 15-minute interval during peak hours. Management wants to know the probability of receiving more than 12 calls in a 15-minute period (which would overwhelm current staffing).

**Implementation:** Calculate probability of exceeding capacity:
```excel
=1 - POISSON.DIST(12, 8, TRUE)
```
This returns approximately 0.064, meaning about 6.4% of 15-minute periods will have more than 12 calls.

**Business Impact:** Staffing decisions require understanding peak demand probabilities. A 6.4% overflow rate might be acceptable, or additional staff might be needed. The Poisson model enables data-driven capacity planning.

### Website Error Monitoring

**Scenario:** A website averages 2.5 errors per hour under normal operation. The DevOps team wants to set an alert threshold that triggers investigation only for genuinely unusual error rates, not normal variation.

**Implementation:** Find the threshold that is exceeded only 5% of the time:
```excel
Start with: =1 - POISSON.DIST(5, 2.5, TRUE)  // Returns 0.042
Check: =1 - POISSON.DIST(4, 2.5, TRUE)  // Returns 0.109
```
With 5 errors as threshold, only 4.2% of normal hours would trigger false alarms.

**Business Impact:** Well-calibrated alerting thresholds reduce alert fatigue while catching genuine problems. Using the Poisson distribution ensures thresholds reflect actual statistical variation rather than arbitrary guesses.

### Insurance Claims Forecasting

**Scenario:** An insurance company has a portfolio averaging 12 claims per month. They need to know the probability of having 20 or more claims in a month (which would strain claims processing capacity).

**Implementation:** Calculate probability of high-claim month:
```excel
=1 - POISSON.DIST(19, 12, TRUE)
```
This returns approximately 0.020, meaning about 2% of months will have 20+ claims.

**Business Impact:** Claims volume forecasting supports staffing, cash flow management, and reinsurance decisions. Understanding the probability of high-volume periods enables proactive resource allocation.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | POISSON.DIST | POISSON.DIST |
| Legacy support | POISSON still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Maximum lambda | Limited by floating-point precision | Similar limitations |
| Non-integer x | Truncates to integer | Same behavior |

Both platforms produce identical results. For very large lambda values (hundreds or more), both may experience slight precision issues.

## Tips and Best Practices

1. **Match time periods:** Lambda must correspond to the same time period as your question. If arrivals are 4 per hour and you want probability for 30 minutes, use lambda = 2.

2. **Use for rare events:** Poisson works best when lambda is small to moderate. For large lambda (>20-30), normal approximation with mean=variance=lambda is accurate.

3. **Verify independence:** Poisson assumes events are independent. Clustered or correlated events violate this assumption and may need a different model.

4. **Remember variance equals mean:** This is both a property and a test. If your data has variance much greater than mean, consider negative binomial instead.

5. **Calculate ranges correctly:** P(a <= X <= b) = POISSON.DIST(b, lambda, TRUE) - POISSON.DIST(a-1, lambda, TRUE).

6. **Use for binomial approximation:** When n is large and p is small, binomial(n,p) is approximated by Poisson(np). This simplifies calculations.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[EXPON.DIST]] | Exponential distribution for time between events (related process) |
| [[BINOM.DIST]] | Binomial for fixed trials (Poisson is limiting case) |
| [[NEGBINOM.DIST]] | Negative binomial for overdispersed counts |
| [[POISSON]] | Legacy version of POISSON.DIST |

### Commonly Used Together

**[[EXPON.DIST]]** - Related distribution for waiting times

*Poisson arrivals mean exponential inter-arrival times:*
```excel
If arrivals are Poisson(5): Events per hour
Then wait times are Exponential(5): Mean wait = 1/5 hour = 12 min
```
These describe the same process from different perspectives.

**[[NORM.DIST]]** - Normal approximation for large lambda

*For large lambda:*
```excel
=POISSON.DIST(120, 100, TRUE)
// Approximately equals
=NORM.DIST(120, 100, SQRT(100), TRUE)
```
Normal with mean=lambda, sd=sqrt(lambda) approximates Poisson.

**[[FACT]]** - Understand the formula

*Manual calculation:*
```excel
=EXP(-lambda) * lambda^x / FACT(x)
```
This equals POISSON.DIST(x, lambda, FALSE) and shows the PMF formula.

## Official Documentation

- [Microsoft Excel POISSON.DIST](https://support.microsoft.com/en-us/office/poisson-dist-function-8fe148ff-39a2-46cb-abf3-7772695d9636)
- [Google Sheets POISSON.DIST](https://support.google.com/docs/answer/3094085)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced POISSON.DIST as replacement for POISSON with renamed cumulative parameter |
| Excel 2013 | 2013 | Improved numerical stability for large lambda |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as POISSON |
| Google Sheets | 2010 | Added POISSON.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
