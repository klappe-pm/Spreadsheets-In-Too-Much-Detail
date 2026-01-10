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
- hypergeometric-distribution
- discrete-distributions
- sampling-without-replacement
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- HYPGEOMDIST
- Hypergeometric Distribution
- Sampling Without Replacement
tags:
- statistics
- probability
- distribution-functions
- quality-control
- finite-population-sampling
---

# HYPGEOM.DIST

## Description

HYPGEOM.DIST returns the hypergeometric distribution, which models the probability of k successes in n draws from a finite population of size N containing K successes, when sampling is done without replacement. Unlike the binomial distribution where each trial is independent, the hypergeometric distribution accounts for the changing probability after each draw. Think of it as drawing cards from a deck: once you draw an ace, there are fewer aces remaining, which affects the probability of drawing another ace.

The hypergeometric distribution is characterized by four parameters: the sample size (n), the number of successes in the sample (k), the population size (N), and the number of successes in the population (K). The key distinction from the binomial is that trials are not independent - each draw changes the composition of the remaining population. As the population size grows very large relative to the sample size, the hypergeometric approaches the binomial distribution.

Use HYPGEOM.DIST for quality inspection when sampling from finite batches without replacement (lot acceptance sampling), analyzing lottery or card game probabilities, ecological population studies (capture-recapture), audit sampling from finite document populations, and any scenario where you sample without replacement from a known, finite population. It is essential for accurate probability calculations when the sample is a significant fraction of the population.

Both Excel and Google Sheets support HYPGEOM.DIST with identical syntax, replacing the legacy HYPGEOMDIST function. The function can return either the probability mass function (PMF) for exactly k successes, or the cumulative distribution function (CDF) for k or fewer successes.

## Syntax

> [!info] HYPGEOM.DIST Syntax
>
> ```excel
> =HYPGEOM.DIST(sample_s, number_sample, population_s, number_pop, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `sample_s` | The number of successes in the sample | Yes |
| `number_sample` | The size of the sample (n) | Yes |
| `population_s` | The number of successes in the population (K) | Yes |
| `number_pop` | The population size (N) | Yes |
| `cumulative` | TRUE returns the cumulative probability; FALSE returns the exact probability | Yes |

## Examples

```excel
=HYPGEOM.DIST(4, 12, 20, 40, FALSE)
```
**Result:** `0.1504` (approximately)
**Explanation:** Probability of drawing exactly 4 red balls when selecting 12 balls from an urn containing 20 red and 20 white balls (40 total). About 15% chance.

```excel
=HYPGEOM.DIST(4, 12, 20, 40, TRUE)
```
**Result:** `0.2311` (approximately)
**Explanation:** Probability of drawing 4 or fewer red balls in the same scenario. About 23% of samples will have 4 or fewer reds.

```excel
=HYPGEOM.DIST(1, 5, 4, 52, FALSE)
```
**Result:** `0.2995` (approximately)
**Explanation:** Probability of drawing exactly 1 ace when dealt 5 cards from a standard 52-card deck. About 30% of 5-card hands have exactly one ace.

```excel
=1 - HYPGEOM.DIST(0, 5, 4, 52, TRUE)
```
**Result:** `0.3412` (approximately)
**Explanation:** Probability of drawing at least 1 ace in 5 cards. About 34% of hands contain at least one ace. Equals 1 - P(zero aces).

```excel
=HYPGEOM.DIST(0, 50, 5, 1000, FALSE)
```
**Result:** `0.7697` (approximately)
**Explanation:** Probability of finding zero defects when sampling 50 items from a batch of 1000 with 5 defects. About 77% of such samples show no defects.

```excel
=HYPGEOM.DIST(2, 10, 20, 100, TRUE) - HYPGEOM.DIST(0, 10, 20, 100, TRUE)
```
**Result:** `0.5573` (approximately)
**Explanation:** Probability of finding 1 or 2 successes in a sample of 10 from population of 100 with 20 successes. Subtract P(0) from P(<=2).

```excel
=HYPGEOM.DIST(3, 10, 50, 100, FALSE)
```
**Result:** `0.1472` (approximately)
**Explanation:** Compare to BINOM.DIST(3, 10, 0.5, FALSE) = 0.1172. With small sample relative to population, hypergeometric is slightly higher due to dependence effects.

```excel
=HYPGEOM.DIST(5, 10, 100, 1000, FALSE)
```
**Result:** `0.0261` (approximately)
**Explanation:** When sample is small relative to population (10/1000 = 1%), this nearly equals BINOM.DIST(5, 10, 0.1, FALSE) = 0.0264. The distributions converge.

```excel
=HYPGEOM.DIST(5, 5, 10, 20, FALSE)
```
**Result:** `0.0163` (approximately)
**Explanation:** Probability of drawing all 5 successes when sample equals half the success items. =COMBIN(10,5)*COMBIN(10,0)/COMBIN(20,5).

```excel
=HYPGEOM.DIST(B2, C2, D2, E2, FALSE)
```
**Result:** (Varies based on cell values)
**Explanation:** Uses cell references for dynamic calculations. Useful for building lookup tables or analyzing various sampling scenarios.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | sample_s > number_sample | Cannot have more successes than sample size |
| `#NUM!` | sample_s > population_s | Cannot have more successes than exist in population |
| `#NUM!` | number_sample > number_pop | Sample cannot exceed population size |
| `#NUM!` | number_sample - sample_s > number_pop - population_s | Not enough failures in population to fill sample |
| `#VALUE!` | Non-numeric arguments provided | Ensure all arguments are numbers or cell references |
| `#NUM!` | Any parameter is negative | All parameters must be non-negative |

## Use Cases

### Quality Control Lot Acceptance

**Scenario:** A manufacturer receives a shipment of 500 components. Quality control samples 50 items without replacement. If the batch actually contains 15 defective items (3% defect rate), what is the probability of finding 2 or fewer defects in the sample?

**Implementation:** Calculate acceptance probability:
```excel
=HYPGEOM.DIST(2, 50, 15, 500, TRUE)
```
This returns approximately 0.809, meaning about 81% of such samples will pass if the acceptance criterion is "2 or fewer defects."

**Business Impact:** Understanding hypergeometric probabilities helps design effective sampling plans. The 19% rejection rate (false positive for good batches) may be acceptable, or the plan may need adjustment based on cost-benefit analysis.

### Card Game Strategy

**Scenario:** A poker player needs to calculate the probability of completing a flush. They have 4 hearts after the flop, with 2 more cards to come. What is the probability of drawing at least one more heart?

**Implementation:** After seeing 3 community cards + 2 hole cards = 5 cards, there are 47 cards remaining with 9 hearts unseen:
```excel
=1 - HYPGEOM.DIST(0, 2, 9, 47, TRUE)
```
This returns approximately 0.350, giving about a 35% chance of completing the flush.

**Business Impact:** Accurate probability calculations are essential for poker strategy. Players use these odds to determine correct betting actions and expected value of different plays.

### Audit Sampling

**Scenario:** An auditor must test 100 transactions from a population of 2,000. If 40 transactions actually contain errors (2% error rate), what is the probability of detecting at least one error?

**Implementation:** Calculate detection probability:
```excel
=1 - HYPGEOM.DIST(0, 100, 40, 2000, TRUE)
```
This returns approximately 0.867, meaning an 86.7% chance of finding at least one error if errors exist at this rate.

**Business Impact:** Audit sampling design requires understanding detection probabilities. The hypergeometric distribution is more accurate than binomial when sample size is substantial relative to population.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | HYPGEOM.DIST | HYPGEOM.DIST |
| Legacy support | HYPGEOMDIST still works | Both versions supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with dynamic arrays (365) or CSE | Native array support |
| Parameter order | sample_s, number_sample, population_s, number_pop | Same order |
| Cumulative parameter | Required | Required |

Both platforms produce identical results. Parameter names may vary in documentation but the order and meaning are consistent.

## Tips and Best Practices

1. **Use when sampling without replacement:** If each sampled item is not returned to the population before the next draw, use hypergeometric. If items are replaced (or population is effectively infinite), use binomial.

2. **Check the rule of thumb:** When sample size is less than 5-10% of population size, the hypergeometric is well-approximated by the binomial, which may be simpler to work with.

3. **Remember the constraints:** sample_s cannot exceed sample size, population success count, or population size minus sample size plus sample success count.

4. **Verify with combinatorics:** HYPGEOM.DIST(k, n, K, N, FALSE) = COMBIN(K,k)*COMBIN(N-K,n-k)/COMBIN(N,n). This can help debug unexpected results.

5. **Calculate both directions:** P(at least k) = 1 - HYPGEOM.DIST(k-1, n, K, N, TRUE). P(exactly k or more) needs the complement of the CDF.

6. **Compare to binomial:** For intuition, compare HYPGEOM.DIST(k, n, K, N, FALSE) to BINOM.DIST(k, n, K/N, FALSE). The difference shows the impact of sampling without replacement.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BINOM.DIST]] | Binomial distribution for sampling with replacement or infinite population |
| [[NEGBINOM.DIST]] | Negative binomial for trials until r successes |
| [[POISSON.DIST]] | Poisson distribution for rare events in large populations |
| [[HYPGEOMDIST]] | Legacy version of HYPGEOM.DIST |

### Commonly Used Together

**[[BINOM.DIST]]** - Compare with replacement vs. without replacement

*Understanding the difference:*
```excel
Without replacement: =HYPGEOM.DIST(5, 20, 50, 100, FALSE)
With replacement: =BINOM.DIST(5, 20, 0.5, FALSE)
```
Compare results to see when sampling method matters.

**[[COMBIN]]** - Calculate underlying combinations

*Verify probability calculation:*
```excel
=COMBIN(K, k) * COMBIN(N-K, n-k) / COMBIN(N, n)
```
This equals HYPGEOM.DIST(k, n, K, N, FALSE) and shows the combinatorial structure.

**[[IF]]** - Build acceptance sampling decisions

*Quality control rule:*
```excel
=IF(HYPGEOM.DIST(3, 50, 20, 500, TRUE) > 0.95, "Accept", "Increase Sample")
```
Tests whether a sampling plan achieves desired confidence level.

## Official Documentation

- [Microsoft Excel HYPGEOM.DIST](https://support.microsoft.com/en-us/office/hypgeom-dist-function-6dbd547f-1d12-4b1f-8b70-c2d7a96f7e28)
- [Google Sheets HYPGEOM.DIST](https://support.google.com/docs/answer/9116267)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced HYPGEOM.DIST with cumulative parameter as replacement for HYPGEOMDIST |
| Excel 2013 | 2013 | Improved numerical stability for large populations |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as HYPGEOMDIST |
| Google Sheets | 2010 | Added HYPGEOM.DIST alias for Excel compatibility |

---

*Last updated: 2026-01-10*
