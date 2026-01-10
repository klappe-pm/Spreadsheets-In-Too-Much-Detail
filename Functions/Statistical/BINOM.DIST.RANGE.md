---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- binomial-distribution
- probability
- range-probability
- discrete-distribution
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Binomial Distribution Range
- Binomial Range Probability
- Cumulative Binomial Range
tags:
- statistical
- probability
- binomial
- distribution
- hypothesis-testing
---

# BINOM.DIST.RANGE

## Description

**BINOM.DIST.RANGE** calculates the probability of achieving a specified range of successes in a series of independent Bernoulli trials (yes/no, success/failure experiments). While BINOM.DIST calculates the probability of exactly k successes or at most k successes, BINOM.DIST.RANGE calculates the probability of getting between k1 and k2 successes (inclusive). This makes it ideal for questions like "What is the probability of getting between 40 and 60 heads when flipping a coin 100 times?" or "What is the chance that 15 to 20 customers out of 50 will make a purchase?"

The function is mathematically equivalent to summing individual binomial probabilities: P(k1 <= X <= k2) = SUM from i=k1 to k2 of C(n,i) * p^i * (1-p)^(n-i), where C(n,i) is the binomial coefficient (n choose i), p is the probability of success on each trial, and n is the number of trials. This summation can be tedious to calculate manually, especially for large ranges, making BINOM.DIST.RANGE extremely valuable. The function handles all the combinatorial mathematics internally, returning a single probability value between 0 and 1.

**Key assumptions of the binomial distribution:** BINOM.DIST.RANGE assumes each trial is independent (one outcome does not affect another), the probability of success p is constant across all trials, each trial has exactly two outcomes (success or failure), and the number of trials n is fixed. Violations of these assumptions make binomial calculations inappropriate. For example, if drawing cards without replacement (where each draw changes the probability), the hypergeometric distribution is more appropriate. If the probability changes over time, the binomial model does not apply.

**Platform availability note:** BINOM.DIST.RANGE is available in Excel 2013 and later versions, and in Google Sheets. Earlier Excel versions lack this function but can achieve the same result using BINOM.DIST with cumulative=TRUE subtraction: =BINOM.DIST(s2, trials, p, TRUE) - BINOM.DIST(s1-1, trials, p, TRUE). This workaround requires careful handling of the lower bound (s1-1, not s1).

## Syntax

> [!f(x)] BINOM.DIST.RANGE Syntax
>
> ```
> =BINOM.DIST.RANGE(trials, probability_s, number_s, [number_s2])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `trials` | Yes | The total number of independent trials. Must be a non-negative integer. |
| `probability_s` | Yes | The probability of success on each trial. Must be between 0 and 1 (inclusive). |
| `number_s` | Yes | The minimum number of successes (lower bound of the range). If number_s2 is omitted, this is the exact number of successes. |
| `number_s2` | No | The maximum number of successes (upper bound of the range). If omitted, function returns probability of exactly number_s successes. |

### Return Value

Returns a probability value between 0 and 1 representing the likelihood of achieving between number_s and number_s2 successes (inclusive) in the specified number of trials. If number_s2 is omitted, returns the probability of exactly number_s successes.

## Examples

> [!f(x)] BINOM.DIST.RANGE Examples

### Example 1: Basic Range Probability
```
=BINOM.DIST.RANGE(10, 0.5, 4, 6)
```
**Result:** 0.6562 (approximately)

**Explanation:** Probability of getting 4, 5, or 6 heads when flipping a fair coin 10 times. With p=0.5, values near the expected value (5) are most likely. This range covers about 66% of outcomes.

---

### Example 2: Exact Success Count (No Range)
```
=BINOM.DIST.RANGE(10, 0.5, 5)
```
**Result:** 0.2461 (approximately)

**Explanation:** When number_s2 is omitted, the function returns the probability of exactly that many successes. Here, exactly 5 heads in 10 flips has about 24.6% probability.

---

### Example 3: Quality Control Acceptance
```
=BINOM.DIST.RANGE(100, 0.02, 0, 3)
```
**Result:** 0.8590 (approximately)

**Explanation:** In a batch of 100 items with 2% defect rate, what's the probability of finding 0-3 defects? About 86% chance of passing an acceptance test allowing up to 3 defects.

---

### Example 4: Marketing Campaign Response Rate
```
=BINOM.DIST.RANGE(1000, 0.03, 25, 35)
```
**Result:** 0.5821 (approximately)

**Explanation:** If 3% of customers typically respond to marketing, what's the probability of getting 25-35 responses from 1000 contacts? About 58% chance, useful for setting realistic expectations.

---

### Example 5: Medical Trial Success Range
```
=BINOM.DIST.RANGE(50, 0.6, 28, 35)
```
**Result:** 0.7287 (approximately)

**Explanation:** For a treatment with 60% success rate tested on 50 patients, the probability of 28-35 successes (56%-70% rate) is about 73%. This helps assess whether trial outcomes support efficacy claims.

---

### Example 6: Sales Conversion Analysis
```
=BINOM.DIST.RANGE(200, 0.15, 25, 35)
```
**Result:** 0.6889 (approximately)

**Explanation:** With a 15% conversion rate and 200 leads, there's about 69% probability of achieving 25-35 sales. This range-based thinking helps set realistic targets and performance bands.

---

### Example 7: At Least N Successes (Using Maximum Bound)
```
=BINOM.DIST.RANGE(20, 0.3, 8, 20)
```
**Result:** 0.2277 (approximately)

**Explanation:** Probability of at least 8 successes in 20 trials with 30% success rate. By setting upper bound to n (20), we get cumulative probability from 8 upward.

---

### Example 8: At Most N Successes (Starting from Zero)
```
=BINOM.DIST.RANGE(20, 0.3, 0, 4)
```
**Result:** 0.2375 (approximately)

**Explanation:** Probability of at most 4 successes in 20 trials with 30% success rate. Starting from 0 gives cumulative probability up to and including 4.

---

### Example 9: Election Poll Margin Analysis
```
=BINOM.DIST.RANGE(1000, 0.52, 500, 540)
```
**Result:** 0.8265 (approximately)

**Explanation:** If true support is 52%, what's the probability a poll of 1000 people shows 50%-54% support? About 83% of polls would show results in this range, demonstrating polling uncertainty.

---

### Example 10: Sports Performance Probability
```
=BINOM.DIST.RANGE(162, 0.300, 45, 55)
```
**Result:** 0.6157 (approximately)

**Explanation:** For a baseball player with .300 batting average over 162 at-bats, the probability of getting 45-55 hits (27.8%-34.0% range) is about 62%. Small sample sizes create wide performance ranges.

---

### Example 11: Website Conversion Testing
```
Control: =BINOM.DIST.RANGE(500, 0.04, 18, 22)
Test:    =BINOM.DIST.RANGE(500, 0.05, 23, 27)
```
**Result:** Both around 0.45-0.55

**Explanation:** Comparing expected result ranges for control (4% rate) and test (5% rate) versions. Overlapping probability ranges indicate the need for larger sample sizes to detect the difference reliably.

---

### Example 12: Customer Satisfaction Survey
```
=BINOM.DIST.RANGE(50, 0.85, 40, 45)
```
**Result:** 0.7103 (approximately)

**Explanation:** If 85% of customers are typically satisfied, the probability that 40-45 out of 50 surveyed express satisfaction is about 71%. This helps interpret whether survey results are within normal variation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | probability_s < 0 or > 1 | Ensure probability is between 0 and 1 (e.g., use 0.05 for 5%, not 5). |
| `#NUM!` | trials is negative | Use a non-negative integer for number of trials. |
| `#NUM!` | number_s or number_s2 is negative | Success counts must be non-negative integers. |
| `#NUM!` | number_s > trials | Cannot have more successes than trials. Verify bounds. |
| `#NUM!` | number_s > number_s2 | Lower bound must not exceed upper bound. Check parameter order. |
| `#VALUE!` | Non-numeric arguments | Ensure all parameters are numbers or cell references to numbers. |
| `#NAME?` | Function not available (Excel 2010 or earlier) | Use BINOM.DIST workaround or upgrade Excel version. |

## Use Cases

### [[Quality Assurance Sampling]]

**Scenario:** A quality manager determines acceptance criteria for incoming shipments, specifying the maximum number of defects that will lead to accepting or rejecting a batch.

**Implementation:**
```
=BINOM.DIST.RANGE(sample_size, defect_rate, 0, max_defects)
```
Calculate probability of acceptance under various defect rates.

**Business Application:** Acceptance sampling plans balance inspection costs against quality risks. If you sample 100 items and accept batches with 0-2 defects, BINOM.DIST.RANGE tells you the acceptance probability for any defect rate. At 1% defects, acceptance is 92%. At 3% defects, it drops to 42%. This analysis helps set appropriate sample sizes and acceptance numbers to achieve desired producer and consumer risk levels.

**Technical Details:** Create Operating Characteristic (OC) curves by calculating acceptance probability across a range of quality levels. Compare different sampling plans (n=50, c=1 vs n=100, c=2) to optimize cost versus discrimination power. Consider double or sequential sampling for more efficient plans.

---

### [[A/B Testing Sample Size Planning]]

**Scenario:** A product manager plans an A/B test to detect whether a new feature improves conversion rates, needing to determine adequate sample sizes.

**Implementation:**
```
Control probability: =BINOM.DIST.RANGE(n, p_control, x1, x2)
Test probability:    =BINOM.DIST.RANGE(n, p_test, x1, x2)
```
Analyze overlap to assess detectability.

**Business Application:** Before running expensive A/B tests, use BINOM.DIST.RANGE to model expected outcomes. If control converts at 5% and you hope for 6% with the new feature, how often would 1000 users show an apparent improvement? If result distributions overlap 80%, you need more users. This prevents inconclusive tests and wasted development effort.

**Technical Details:** Calculate power by finding the probability that the test group exceeds the control group's likely range. For 80% power, the test group's expected range should exceed the control's upper bound with 80% probability. This determines minimum sample size requirements.

---

### [[Sports Analytics and Performance Bounds]]

**Scenario:** A sports analyst establishes expected performance ranges to identify statistically unusual performances that warrant investigation or celebration.

**Implementation:**
```
=BINOM.DIST.RANGE(attempts, skill_rate, lower_bound, upper_bound)
```
Calculate probability of outcomes within specified range.

**Business Application:** A basketball player's true free-throw percentage might be 80%. In a game with 10 attempts, what range should we expect? BINOM.DIST.RANGE(10, 0.8, 6, 10) = 95.3% shows that 6-10 makes is the 95% confidence range. Making only 4 has just 1.2% probability, suggesting an off night worth investigating (fatigue, injury, venue factors).

**Technical Details:** Build season-long expectation bands by aggregating game-by-game binomial probabilities. Track when players consistently over/underperform expectations to detect skill changes, playing time effects, or opponent difficulty factors.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2013 and later (not available in Excel 2010 or earlier)
- **Parameter order:** trials, probability_s, number_s, [number_s2]
- **Single value mode:** Omitting number_s2 returns exact probability
- **Precision:** 15 significant decimal digits
- **Maximum trials:** Limited by computational precision, effectively millions

### Google Sheets

- **Availability:** All versions since function introduction
- **Parameter order:** trials, probability_s, number_s, [number_s2]
- **Single value mode:** Omitting number_s2 returns exact probability
- **Precision:** 15 significant decimal digits
- **Maximum trials:** Limited by computational precision

### Key Difference Alert

BINOM.DIST.RANGE behaves identically in Excel (2013+) and Google Sheets. The primary compatibility concern is Excel 2010 and earlier, where the function does not exist.

**Workaround for older Excel versions:**
```
=BINOM.DIST(number_s2, trials, probability_s, TRUE) - BINOM.DIST(number_s - 1, trials, probability_s, TRUE)
```
This calculates cumulative probability up to number_s2 minus cumulative probability up to (number_s - 1), yielding the range probability.

## Tips and Best Practices

1. **Verify independence assumption.** Binomial distribution assumes each trial is independent. If outcomes influence each other (e.g., without-replacement sampling), the binomial model is inappropriate. Consider hypergeometric distribution for dependent sampling.

2. **Check for constant probability.** The success probability must be the same for every trial. If probability changes over time or depends on previous outcomes, binomial calculations give misleading results.

3. **Use reasonable trial counts.** Very large trial counts (millions) may cause computational precision issues. For large n, consider normal approximation: if np > 5 and n(1-p) > 5, the normal distribution with mean=np and variance=np(1-p) closely approximates the binomial.

4. **Understand inclusive bounds.** Both number_s and number_s2 are included in the range. For "strictly between" calculations, adjust bounds accordingly.

5. **Remember single-value mode.** Omitting number_s2 returns exact probability, equivalent to BINOM.DIST with cumulative=FALSE. This is useful but can cause confusion if you forget the parameter.

6. **Build probability tables.** Create tables showing probabilities for different success ranges and rates. This helps stakeholders understand uncertainty without requiring them to use the function directly.

7. **Combine with decision thresholds.** Rather than point estimates, communicate decisions using ranges. "We'll proceed if between 40 and 60 customers respond" acknowledges uncertainty better than "we need exactly 50."

8. **Validate with simulations.** For critical decisions, validate BINOM.DIST.RANGE results with Monte Carlo simulations. This catches any modeling assumption violations.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BINOM.DIST]] | Single point or cumulative binomial probability | For exact probability or probability up to a value (not a range) |
| [[BINOM.INV]] | Inverse binomial (find successes for given probability) | To find how many successes achieve a target cumulative probability |
| [[NEGBINOM.DIST]] | Negative binomial distribution | When counting trials until a specified number of successes |
| [[HYPGEOM.DIST]] | Hypergeometric distribution | For sampling without replacement (dependent trials) |

### Commonly Used Together

**[[BINOM.DIST]]** - Point and cumulative probabilities

*Compare exact vs range probabilities:*
```
Exact 5 successes: =BINOM.DIST(5, 10, 0.5, FALSE)
At most 5 successes: =BINOM.DIST(5, 10, 0.5, TRUE)
Range 4-6: =BINOM.DIST.RANGE(10, 0.5, 4, 6)
```
BINOM.DIST handles single points and cumulative from zero; BINOM.DIST.RANGE handles arbitrary ranges.

---

**[[BINOM.INV]]** - Inverse for goal-seeking

*Find required successes for probability threshold:*
```
=BINOM.INV(100, 0.5, 0.95)
```
Returns the minimum number of successes with cumulative probability >= 95%. Use with BINOM.DIST.RANGE to verify the range probabilities around this value.

---

**[[NORM.DIST]]** - Normal approximation for large n

*For large trial counts, normal approximation:*
```
Binomial: =BINOM.DIST.RANGE(1000, 0.5, 480, 520)
Normal approximation: =NORM.DIST(520.5, 500, SQRT(250), TRUE) - NORM.DIST(479.5, 500, SQRT(250), TRUE)
```
For large n where np > 5 and n(1-p) > 5, normal approximation is computationally efficient and accurate.

## Official Documentation

- **Microsoft Excel:** [BINOM.DIST.RANGE function](https://support.microsoft.com/en-us/office/binom-dist-range-function-17331329-74c7-4053-bb4c-6653a7421595)
- **Google Sheets:** [BINOM.DIST.RANGE function](https://support.google.com/docs/answer/9116279)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New function for range probability |
| Excel 2016+ | Continued support | No changes |
| Excel 365 | Continuous updates | Enhanced performance |
| Google Sheets | 2019 (approximately) | Added for Excel compatibility |

---

*Last updated: 2026-01-10*
