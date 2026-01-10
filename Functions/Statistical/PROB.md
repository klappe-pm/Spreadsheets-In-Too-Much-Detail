---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- probability
- discrete-distribution
- probability-mass
- custom-distribution
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Probability Distribution
- Discrete Probability
- PMF Calculation
tags:
- probability
- discrete
- distribution
- statistics
- pmf
---

# PROB

## Description

**PROB** calculates the probability that values in a range fall within specified limits, given an associated set of probabilities. You provide a list of possible values, their corresponding probabilities, and optionally upper and lower bounds. PROB returns the sum of probabilities for all values within your specified range.

Use PROB when you have a **custom discrete probability distribution** and want to calculate cumulative probabilities within bounds. Unlike standard distribution functions (NORM.DIST, BINOM.DIST), PROB works with any user-defined distribution—you specify exactly what values are possible and how likely each is.

The function is essentially a conditional sum of probabilities: it adds up the probabilities for all x-values where lower_limit <= x <= upper_limit. If you omit the upper_limit, PROB returns the probability of exactly the lower_limit value—effectively a probability mass function lookup.

**Critical requirement: probabilities must sum to 1** (or less, though that would represent an incomplete distribution). If the sum of your prob_range exceeds 1, PROB returns #NUM! error. Each probability must be between 0 and 1 inclusive.

## Syntax

> [!f(x)] PROB Syntax
>
> ```
> =PROB(x_range, prob_range, [lower_limit], [upper_limit])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x_range` | Yes | The range of numeric values (possible outcomes). Must match size of prob_range. |
| `prob_range` | Yes | The probabilities associated with each value in x_range. Must sum to <= 1, each value 0 to 1. |
| `lower_limit` | No | The lower bound for summing probabilities. If omitted, returns P(X = upper_limit). |
| `upper_limit` | No | The upper bound. If omitted, returns P(X = lower_limit). |

### Return Value

Returns the probability (0 to 1) that a value from x_range falls within the specified limits, according to prob_range. Returns #NUM! if probabilities sum > 1 or any probability is outside [0,1]. Returns #VALUE! if ranges don't match in size.

## Examples

> [!f(x)] PROB Examples

### Example 1: Exact Value Probability
```
=PROB({1,2,3,4,5,6}, {1/6,1/6,1/6,1/6,1/6,1/6}, 3)
```
**Result:** 0.1667 (1/6)

**Explanation:** Fair die probability of rolling exactly 3. One value out of six equally likely outcomes.

---

### Example 2: Range of Values
```
=PROB({1,2,3,4,5,6}, {1/6,1/6,1/6,1/6,1/6,1/6}, 2, 4)
```
**Result:** 0.5 (3/6)

**Explanation:** Probability of rolling 2, 3, or 4 on fair die. Three outcomes out of six: 3/6 = 0.5.

---

### Example 3: Custom Distribution
```
=PROB({10,20,30,40}, {0.1,0.3,0.4,0.2}, 20, 30)
```
**Result:** 0.7

**Explanation:** P(20) + P(30) = 0.3 + 0.4 = 0.7. Sum of probabilities for values in range.

---

### Example 4: Cell Ranges
```
=PROB(A1:A10, B1:B10, 5, 15)
```
**Result:** Sum of probabilities for x_values between 5 and 15

**Explanation:** Standard usage with cell ranges for values and their probabilities.

---

### Example 5: Single Bound (Lower Only)
```
=PROB({1,2,3,4,5}, {0.1,0.2,0.3,0.25,0.15}, 3)
```
**Result:** 0.3

**Explanation:** When only one limit given, returns P(X = limit), which is 0.3 for x=3.

---

### Example 6: Biased Die
```
=PROB({1,2,3,4,5,6}, {0.1,0.1,0.1,0.2,0.2,0.3}, 5, 6)
```
**Result:** 0.5

**Explanation:** Loaded die favoring high numbers. P(5 or 6) = 0.2 + 0.3 = 0.5.

---

### Example 7: Full Range
```
=PROB({1,2,3}, {0.2,0.5,0.3}, 1, 3)
```
**Result:** 1.0

**Explanation:** All values in range. Sum of all probabilities = 1 (complete distribution).

---

### Example 8: No Values in Range
```
=PROB({1,2,3}, {0.2,0.5,0.3}, 5, 10)
```
**Result:** 0

**Explanation:** No x-values fall between 5 and 10, so probability is 0.

---

### Example 9: Partial Distribution
```
=PROB({1,2,3}, {0.2,0.3,0.4}, 2)
```
**Result:** 0.3

**Explanation:** Probabilities sum to 0.9 (not 1), but still valid. Returns P(X=2) = 0.3.

---

### Example 10: Survey Response Distribution
```
=PROB({1,2,3,4,5}, {0.05,0.1,0.35,0.35,0.15}, 4, 5)
```
**Result:** 0.5

**Explanation:** Probability of "satisfied" (4) or "very satisfied" (5) responses.

---

### Example 11: Inventory Demand
```
=PROB(DemandLevels, DemandProbabilities, 0, SafetyStock)
```
**Result:** Probability demand stays within safety stock

**Explanation:** Using named ranges for inventory analysis.

---

### Example 12: Quality Control
```
=PROB(DefectCounts, DefectProbs, 0, AcceptableLimit)
```
**Result:** Probability of acceptable quality

**Explanation:** P(defects <= acceptable limit) for lot acceptance.

---

### Example 13: Sales Scenarios
```
=PROB({100,200,300,400,500}, {0.1,0.2,0.4,0.2,0.1}, 300, 500)
```
**Result:** 0.7

**Explanation:** Probability of meeting or exceeding 300 units in sales.

---

### Example 14: Complementary Probability
```
=1 - PROB({1,2,3,4,5,6}, {1/6,1/6,1/6,1/6,1/6,1/6}, 1, 4)
```
**Result:** 0.333 (2/6)

**Explanation:** P(X > 4) = 1 - P(X <= 4). Complementary probability calculation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Probabilities sum > 1 | Ensure sum of prob_range <= 1. Check for duplicate entries. |
| `#NUM!` | Probability outside [0,1] | Each probability must be between 0 and 1 inclusive. Check for negative values. |
| `#VALUE!` | Mismatched range sizes | x_range and prob_range must have same number of elements. |
| `#VALUE!` | Non-numeric data | Ensure all values are numeric. Check for text in ranges. |
| `#NAME?` | Misspelled function | Check spelling: PROB, not PROBABILITY. |
| `Returns 0` | No values in range | Specified limits don't capture any x_values. Verify limits match your data. |

## Use Cases

### [[Custom Probability Distributions]]

**Scenario:** Work with empirical or theoretical discrete distributions not built into Excel.

**Implementation:**
```
=PROB(Outcomes, Probabilities, LowerBound, UpperBound)
=PROB(SalesLevels, HistoricalFrequencies/SUM(HistoricalFrequencies), Target)
=PROB({0,1,2,3,4,5}, POISSON.DIST({0,1,2,3,4,5}, Lambda, FALSE), 0, 2)
```

**Business Application:** Custom risk models, empirical distributions, simulation validation. Work with any discrete distribution.

**Technical Details:** Convert frequency data to probabilities by dividing by sum. Ensure probabilities sum to 1 for complete distributions.

---

### [[Inventory and Demand Planning]]

**Scenario:** Calculate probability of demand falling within capacity or stock levels.

**Implementation:**
```
=PROB(DemandScenarios, DemandLikelihoods, 0, InventoryLevel)
=PROB(OrderQuantities, OrderProbabilities, MinOrder, MaxCapacity)
=1 - PROB(Demand, DemandProb, 0, CurrentStock)  → Stockout probability
```

**Business Application:** Safety stock calculations, capacity planning, service level analysis. Match supply capabilities to demand uncertainty.

**Technical Details:** Historical demand frequencies make excellent probability inputs. Group demand into bins and calculate relative frequencies.

---

### [[Risk Assessment]]

**Scenario:** Evaluate probabilities of different risk scenarios.

**Implementation:**
```
=PROB(LossAmounts, LossProbabilities, 0, AcceptableLoss)
=PROB(ProjectDelays, DelayProbabilities, 1, MaxDelay)
=PROB(Severities, SeverityProbs, HighSeverityThreshold, MAX(Severities))
```

**Business Application:** Risk management, insurance, project planning. Quantify probabilities of adverse outcomes.

**Technical Details:** Scenario analysis often produces discrete outcome sets. PROB aggregates scenarios meeting specified criteria.

---

### [[Gaming and Simulation]]

**Scenario:** Calculate probabilities for games, simulations, or random events.

**Implementation:**
```
=PROB({1,2,3,4,5,6}, {1/6,1/6,1/6,1/6,1/6,1/6}, 5, 6)  → High roll probability
=PROB(CardValues, CardProbabilities, 21, 21)             → Blackjack probability
=PROB(SpinResults, WheelProbabilities, WinningNumber)    → Roulette probability
```

**Business Application:** Gaming analysis, Monte Carlo validation, probability education.

**Technical Details:** PROB is ideal for discrete games with known outcome distributions.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 2003
- **Array handling:** Accepts range references or array constants
- **Precision:** Full floating-point precision
- **Limits:** No practical limit on array size

### Google Sheets
- **Availability:** All versions
- **Array handling:** Same as Excel
- **Syntax:** Identical to Excel
- **Performance:** Efficient for typical distributions

### Both Platforms
- Identical calculation
- Same error conditions
- Same handling of bounds
- Same requirement for probability sum <= 1

## Tips and Best Practices

1. **Verify probabilities sum to 1:** Use =SUM(prob_range) to check before using PROB. Sum > 1 causes error.

2. **Use for discrete distributions only:** PROB is for discrete values. For continuous distributions, use appropriate functions (NORM.DIST, etc.).

3. **Convert frequencies to probabilities:** Divide frequencies by total count: freq/SUM(frequencies).

4. **Single limit returns exact probability:** PROB(x, p, value) returns P(X = value), not P(X <= value).

5. **Calculate complements:** P(X > k) = 1 - PROB(x, p, min, k). Useful for "at least" or "greater than" scenarios.

6. **Match array sizes exactly:** x_range and prob_range must have identical dimensions.

7. **Handle edge cases:** If limits don't match any x-values, PROB returns 0 (not an error).

8. **Order doesn't matter:** Values in x_range don't need to be sorted; PROB scans all values.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BINOM.DIST]] | Binomial distribution | For success/failure trials with known n and p |
| [[POISSON.DIST]] | Poisson distribution | For count data with known average rate |
| [[NORM.DIST]] | Normal distribution | For continuous normal distributions |
| [[SUMPRODUCT]] | Flexible summation | For custom conditional probability calculations |

### Commonly Used Together

**[[SUMPRODUCT]]** - Alternative calculation

*Manual probability sum:*
```
=SUMPRODUCT((x_range>=lower)*(x_range<=upper)*prob_range)
```
Equivalent to PROB; sometimes more flexible.

---

**[[SUM]]** - Verify probabilities

*Check distribution validity:*
```
=SUM(prob_range)  → Should equal 1 for complete distribution
```
Always verify before using PROB.

---

**[[FREQUENCY]]** - Create probabilities from data

*Empirical distribution:*
```
Frequencies: =FREQUENCY(data, bins)
Probabilities: =Frequencies/SUM(Frequencies)
```
Convert raw data to PROB-compatible distribution.

---

**[[COUNTIF]]** - Alternative approach

*Count-based probability:*
```
=COUNTIF(data, ">="&lower)/COUNT(data)
```
For empirical probability without explicit prob_range.

---

**[[IF]]** - Conditional probability logic

*Custom probability calculations:*
```
=IF(PROB(x, p, lower, upper) > 0.5, "Likely", "Unlikely")
```
Use PROB results in decision formulas.

## Official Documentation

- **Microsoft Excel:** [PROB function](https://support.microsoft.com/en-us/office/prob-function-9ac30561-c81c-4259-8253-34f0a238fc49)
- **Google Sheets:** [PROB function](https://support.google.com/docs/answer/3094068)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2003 | Original implementation |
| Excel 2007+ | All subsequent | No changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
