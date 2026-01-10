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
- exponential-distribution
- continuous-distributions
- waiting-time-distributions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- EXPONDIST
- Exponential Distribution
- Waiting Time Distribution
tags:
- statistics
- probability
- distribution-functions
- reliability-engineering
- queueing-theory
---

# EXPON.DIST

## Description

EXPON.DIST returns the exponential distribution, a continuous probability distribution that models the time between events in a Poisson process. If events occur randomly and independently at a constant average rate (like customer arrivals, machine failures, or radioactive decay), the exponential distribution describes the waiting time until the next event. It is characterized by the "memoryless" property: the probability of waiting another hour is the same whether you have already waited 5 minutes or 5 hours.

The distribution is defined by a single parameter lambda (the rate parameter), which represents the average number of events per unit time. The mean waiting time is 1/lambda. For example, if customers arrive at an average rate of 10 per hour (lambda=10), the average time between arrivals is 1/10 hour = 6 minutes. The exponential distribution is always right-skewed, with the mode at zero and a long tail extending toward infinity.

Use EXPON.DIST for reliability analysis (time until equipment failure), queueing theory (waiting times in lines), survival analysis (time until an event), and any situation modeling intervals between random events. It is essential for service operations planning (how long until the next customer?), maintenance scheduling (when will this part fail?), and risk assessment (expected time until a rare event).

Both Excel and Google Sheets implement EXPON.DIST identically, replacing the legacy EXPONDIST function. The function can return either the probability density function (PDF) showing relative likelihood at a specific time, or the cumulative distribution function (CDF) giving the probability of the event occurring by that time. Note that the lambda parameter is the rate (events per time), not the mean time between events.

## Syntax

> [!info] EXPON.DIST Syntax
>
> ```excel
> =EXPON.DIST(x, lambda, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The value at which to evaluate the distribution (must be >= 0) | Yes |
| `lambda` | The rate parameter, equal to 1/mean (must be > 0) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function; FALSE returns the probability density function | Yes |

## Examples

```excel
=EXPON.DIST(1, 0.5, TRUE)
```
**Result:** `0.3935` (approximately 0.393469340)
**Explanation:** With a rate of 0.5 events per unit time (mean wait of 2 units), there is a 39.35% probability that the event occurs within 1 time unit.

```excel
=EXPON.DIST(1, 0.5, FALSE)
```
**Result:** `0.3033` (approximately)
**Explanation:** Returns the probability density at x=1 for lambda=0.5. This is the height of the distribution curve at that point, useful for plotting.

```excel
=EXPON.DIST(6, 10/60, TRUE)
```
**Result:** `0.6321` (approximately)
**Explanation:** If customers arrive at 10 per hour (lambda=10/60=0.167 per minute), there is a 63.21% probability that a customer arrives within 6 minutes.

```excel
=1 - EXPON.DIST(30, 1/20, TRUE)
```
**Result:** `0.2231` (approximately)
**Explanation:** With mean time between failures of 20 hours (lambda=1/20), the probability of surviving past 30 hours is about 22.31%. This is the reliability function R(t) = 1 - F(t).

```excel
=EXPON.DIST(10, 0.1, TRUE)
```
**Result:** `0.6321` (approximately)
**Explanation:** This result (approximately 1 - 1/e = 0.6321) always appears when x equals the mean (here, mean = 1/0.1 = 10). About 63.21% of events occur by the mean time.

```excel
=EXPON.DIST(2, 1, TRUE) - EXPON.DIST(1, 1, TRUE)
```
**Result:** `0.2325` (approximately)
**Explanation:** Probability that the event occurs between time 1 and time 2. Subtract cumulative probabilities to find interval probability.

```excel
=-LN(1-0.9)/0.05
```
**Result:** `46.05` (approximately)
**Explanation:** This formula (inverse exponential) finds the time by which 90% of events will occur when lambda=0.05. Excel lacks a built-in EXPON.INV function.

```excel
=EXPON.DIST(0, 0.5, FALSE)
```
**Result:** `0.5`
**Explanation:** The PDF at x=0 equals lambda. The exponential distribution has its highest density at zero, then decreases monotonically.

```excel
=EXPON.DIST(24, 1/168, TRUE)
```
**Result:** `0.1331` (approximately)
**Explanation:** If equipment has a mean time between failures of 168 hours (one week), there is a 13.31% chance of failure within the first 24 hours.

```excel
=1 - EXPON.DIST(1/B2, B2, TRUE)
```
**Result:** `0.3679` (approximately 1/e)
**Explanation:** The probability of surviving past the mean time is always 1/e (about 36.79%), regardless of the rate parameter. This is a fundamental property of the exponential distribution.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | x is negative | Time value must be zero or positive |
| `#NUM!` | lambda is zero or negative | Rate parameter must be positive |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references containing numbers |
| `Conceptual error` | Confusing lambda with mean | Lambda is the rate (events per time); mean is 1/lambda |
| `Unexpected results` | Using wrong time units | Ensure x and lambda use consistent time units |

## Use Cases

### Call Center Staffing

**Scenario:** A call center receives an average of 30 calls per hour (one every 2 minutes). Management wants to know the probability that no calls arrive during a 5-minute period when an agent takes a break.

**Implementation:** Calculate probability of zero arrivals in 5 minutes:
```excel
=1 - EXPON.DIST(5, 0.5, TRUE)
```
With lambda=0.5 calls per minute (30 per hour), the probability of waiting more than 5 minutes for the next call is about 8.2%.

**Business Impact:** Understanding arrival patterns helps optimize staffing and break schedules. If 8.2% of 5-minute windows have no calls, managers can schedule breaks during predicted slow periods or stagger breaks to maintain coverage.

### Equipment Reliability Analysis

**Scenario:** A manufacturing plant has equipment with a mean time between failures (MTBF) of 500 hours. The maintenance team needs to determine the probability that a newly installed unit survives its first 720 hours (one month of operation).

**Implementation:** Calculate survival probability:
```excel
=1 - EXPON.DIST(720, 1/500, TRUE)
```
This returns approximately 0.237, meaning there is only a 23.7% chance the equipment survives the full month without failure.

**Business Impact:** Reliability predictions inform maintenance strategies and spare parts inventory. A 76.3% failure probability within the first month suggests either improved maintenance intervals or having replacement units readily available.

### Service Time Estimation

**Scenario:** A bank knows that customer service times follow an exponential distribution with a mean of 8 minutes per customer. The branch manager wants to know the probability that a customer will be served within 10 minutes.

**Implementation:** Calculate probability of service completion:
```excel
=EXPON.DIST(10, 1/8, TRUE)
```
This returns approximately 0.713, meaning about 71.3% of customers will be served within 10 minutes.

**Business Impact:** Service time probabilities help set customer expectations and staff service level targets. The bank can advertise "average service time under 10 minutes" while understanding that about 29% of customers will wait longer.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | EXPON.DIST | EXPON.DIST |
| Legacy support | EXPONDIST still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Inverse function | Not built-in (use -LN(1-p)/lambda) | Same - not built-in |
| Parameter interpretation | Lambda is rate (1/mean) | Same interpretation |

Both platforms produce identical results. Neither provides a built-in inverse exponential function; use the formula -LN(1-probability)/lambda to calculate quantiles.

## Tips and Best Practices

1. **Remember lambda is the rate, not the mean:** Lambda = events per unit time. Mean = 1/lambda. If average time between events is 10 minutes, lambda = 0.1 per minute, not 10.

2. **Use consistent time units:** If lambda is in events per hour, x must be in hours. Mixing minutes and hours is a common error.

3. **Calculate the inverse manually:** Excel lacks EXPON.INV. Use `=-LN(1-probability)/lambda` to find the time corresponding to a cumulative probability.

4. **Check the memoryless property:** The probability of waiting another t units is the same regardless of how long you have already waited. If this does not fit your scenario, use a different distribution (like Weibull).

5. **Relate to Poisson:** If events follow a Poisson process (random, independent, constant rate), inter-arrival times follow the exponential distribution. EXPON.DIST and POISSON.DIST are mathematically related.

6. **Use 1-F(t) for reliability:** The reliability function (probability of surviving past time t) is simply 1-EXPON.DIST(t, lambda, TRUE).

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[POISSON.DIST]] | Poisson distribution for counts of events (related process) |
| [[GAMMA.DIST]] | Generalization of exponential (sum of exponentials) |
| [[WEIBULL.DIST]] | Flexible failure time distribution (exponential when shape=1) |
| [[EXPONDIST]] | Legacy version of EXPON.DIST |

### Commonly Used Together

**[[POISSON.DIST]]** - Related distribution for event counts

*If arrivals follow Poisson:*
```excel
Arrivals in time t: =POISSON.DIST(k, lambda*t, FALSE)
Time until next arrival: =EXPON.DIST(t, lambda, TRUE)
```
These two functions describe the same underlying random process from different perspectives.

**[[LN]]** - Calculate inverse exponential

*Find time for given probability:*
```excel
=-LN(1-0.95)/lambda
```
This returns the 95th percentile - the time by which 95% of events will have occurred.

**[[RAND]]** - Generate random exponential values

*Monte Carlo simulation:*
```excel
=-LN(RAND())/lambda
```
Generates random waiting times following the exponential distribution.

## Official Documentation

- [Microsoft Excel EXPON.DIST](https://support.microsoft.com/en-us/office/expon-dist-function-4c12ae24-e563-4155-bf3e-8b78b6ae140e)
- [Google Sheets EXPON.DIST](https://support.google.com/docs/answer/3094403)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced EXPON.DIST as replacement for EXPONDIST |
| Excel 2013 | 2013 | Improved numerical precision |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as EXPONDIST |
| Google Sheets | 2010 | Added EXPON.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
