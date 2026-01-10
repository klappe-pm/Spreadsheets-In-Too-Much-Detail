---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
- math-trig
subTopics:
- combinations
- combinatorics
- probability
- counting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Combinations
- N Choose K
- Binomial Coefficient
tags:
- combinations
- combinatorics
- probability
- counting
- binomial
- lottery
---

# COMBIN

## Description

**COMBIN** calculates the number of combinations of choosing k items from a set of n items, where order does not matter. This is the famous "n choose k" calculation, mathematically written as C(n,k) or the binomial coefficient. COMBIN(10, 3) = 120 because there are 120 different ways to select 3 items from 10 when order is irrelevant. If you have 10 books and want to take 3 on vacation, there are 120 possible selections.

The distinction between **combinations and permutations** is fundamental. Combinations ignore order; permutations count it. Choosing {A, B, C} is the same combination as choosing {C, B, A}—both count as one combination but six permutations. The formula COMBIN(n,k) = n! / (k! * (n-k)!) divides out the k! arrangements of the selected items. COMBIN(5,3) = 10 while PERMUT(5,3) = 60.

COMBIN is essential for **probability calculations, lottery odds, and statistical distributions**. What are the odds of winning a lottery where you pick 6 numbers from 49? You need to match one winning combination out of COMBIN(49,6) = 13,983,816 possibilities. The binomial distribution, hypergeometric distribution, and many statistical tests are built on combination counts.

Key properties: COMBIN(n, 0) = 1 (one way to choose nothing), COMBIN(n, n) = 1 (one way to choose everything), and COMBIN(n, k) = COMBIN(n, n-k) (symmetry—choosing 3 from 10 leaves 7 behind, equivalent selection). Both n and k must be non-negative integers with k <= n. Unlike FACT, COMBIN rarely overflows because the factorial divisions cancel much of the growth.

## Syntax

> [!f(x)] COMBIN Syntax
>
> ```
> =COMBIN(number, number_chosen)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The total number of items (n). Must be a non-negative integer. Decimals are truncated. |
| `number_chosen` | Yes | The number of items to choose (k). Must be a non-negative integer <= number. Decimals are truncated. |

### Return Value

Returns n! / (k! * (n-k)!)—the number of ways to choose k items from n items without regard to order. Returns 1 for COMBIN(n, 0) or COMBIN(n, n). Returns #NUM! if k > n or if either argument is negative. Returns #VALUE! for non-numeric input.

## Examples

> [!f(x)] COMBIN Examples

### Example 1: Basic Combination
```
=COMBIN(5, 2)
```
**Result:** 10

**Explanation:** There are 10 ways to choose 2 items from 5: {1,2}, {1,3}, {1,4}, {1,5}, {2,3}, {2,4}, {2,5}, {3,4}, {3,5}, {4,5}.

---

### Example 2: Lottery Odds (6 from 49)
```
=COMBIN(49, 6)
```
**Result:** 13,983,816

**Explanation:** The odds of winning a standard 6/49 lottery are 1 in nearly 14 million. This is why jackpots get so large.

---

### Example 3: Choose None
```
=COMBIN(10, 0)
```
**Result:** 1

**Explanation:** There is exactly one way to choose zero items: take nothing. This is mathematically necessary for formulas to work.

---

### Example 4: Choose All
```
=COMBIN(10, 10)
```
**Result:** 1

**Explanation:** There is exactly one way to choose all items: take everything. COMBIN(n, n) always equals 1.

---

### Example 5: Symmetry Property
```
=COMBIN(10, 3) = COMBIN(10, 7)
```
**Result:** Both return 120

**Explanation:** Choosing 3 items from 10 automatically leaves 7 behind. The number of ways is identical. COMBIN(n, k) = COMBIN(n, n-k).

---

### Example 6: Card Hands (5 from 52)
```
=COMBIN(52, 5)
```
**Result:** 2,598,960

**Explanation:** There are about 2.6 million possible 5-card poker hands from a standard deck.

---

### Example 7: Committee Selection
```
=COMBIN(12, 4)
```
**Result:** 495

**Explanation:** If a club has 12 members and needs to form a 4-person committee, there are 495 possible committees.

---

### Example 8: Probability Calculation
```
=COMBIN(Successes, k) * COMBIN(Failures, n-k) / COMBIN(Total, n)
```
**Result:** Hypergeometric probability

**Explanation:** The probability of drawing exactly k successes when sampling n items from a population with known success/failure counts.

---

### Example 9: Pascal's Triangle Row
```
=COMBIN(5, 0), COMBIN(5, 1), COMBIN(5, 2), COMBIN(5, 3), COMBIN(5, 4), COMBIN(5, 5)
```
**Result:** 1, 5, 10, 10, 5, 1

**Explanation:** Row 5 of Pascal's triangle. Each entry is COMBIN(5, k) for k from 0 to 5.

---

### Example 10: Binary Strings with k Ones
```
=COMBIN(8, 3)
```
**Result:** 56

**Explanation:** How many 8-bit binary numbers have exactly 3 ones (and 5 zeros)? 56 ways to position the three ones.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | k > n (choosing more than available) | Ensure number_chosen <= number. Cannot choose 7 items from 5. |
| `#NUM!` | Negative argument | Both arguments must be non-negative. Check for negative cell references. |
| `#VALUE!` | Non-numeric input | Ensure both arguments are numbers, not text. |
| `#NAME?` | Misspelled function name | Verify spelling: COMBIN, not COMBINE, COMB, or COMBINATION. |
| `Unexpected result` | Confusion with PERMUT | COMBIN ignores order, PERMUT counts it. Use the correct function for your problem. |

## Use Cases

### [[Probability and Odds Calculation]]

**Scenario:** Calculate probabilities for lotteries, games, and random events.

**Implementation:**
```
=1/COMBIN(TotalNumbers, PickCount)                 → Lottery odds
=COMBIN(Favorable, Successes)/COMBIN(Total, Sample) → Selection probability
=COMBIN(n, k) * p^k * (1-p)^(n-k)                  → Binomial probability component
```

**Business Application:** Gaming odds, risk assessment, quality sampling probabilities, marketing campaign response rates.

**Technical Details:** For lottery odds, calculate COMBIN(total numbers, numbers to pick). For bonus balls, multiply: COMBIN(49,6) * COMBIN(10,1). The binomial distribution uses COMBIN for the "n choose k" component.

---

### [[Sampling and Survey Design]]

**Scenario:** Determine the number of possible samples or calculate sampling probabilities.

**Implementation:**
```
=COMBIN(Population, SampleSize)                    → Possible samples
=COMBIN(Subgroup, Selected)*COMBIN(Rest, Others)/COMBIN(Pop, Sample) → Subgroup probability
=1 - COMBIN(NonDefective, Sample)/COMBIN(Total, Sample) → Probability of finding at least one defective
```

**Business Application:** Quality control sampling, survey methodology, audit sample selection, A/B test group sizing.

**Technical Details:** When sampling without replacement from a finite population, use the hypergeometric distribution based on combinations. This differs from binomial when sampling represents a significant portion of the population.

---

### [[Combinatorial Analysis and Counting]]

**Scenario:** Count the number of ways to select, arrange, or organize items.

**Implementation:**
```
=COMBIN(Employees, TeamSize)                       → Possible teams
=COMBIN(Ingredients, Selected)                     → Recipe combinations
=COMBIN(Questions, ToAnswer)                       → Exam question choices
=PRODUCT(COMBIN(n1,k1), COMBIN(n2,k2))            → Multi-stage selection
```

**Business Application:** Team formation, menu planning, test design, project staffing, resource allocation.

**Technical Details:** For multi-stage selection (choose 3 from group A AND 2 from group B), multiply the combinations. For either/or selections, add combinations. This is the multiplication and addition principles of counting.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Maximum values:** Limited by numeric precision; very large combinations may lose precision
- **Decimal handling:** Both arguments truncated to integers
- **Related:** COMBINA for combinations with repetition (Excel 2013+)

### Google Sheets
- **Availability:** All versions
- **Maximum values:** Same limitations
- **Decimal handling:** Same truncation
- **Related:** Same functions available

### Both Platforms
- Identical results for all valid inputs
- Same C(n,0) = 1 and C(n,n) = 1 conventions
- Same error conditions
- Same relationship to PERMUT and FACT

## Tips and Best Practices

1. **COMBIN for "how many groups" questions:** When order doesn't matter (teams, committees, selections), use COMBIN. When order matters (rankings, arrangements, sequences), use PERMUT.

2. **Use symmetry to verify:** COMBIN(n, k) = COMBIN(n, n-k). If COMBIN(20, 2) = 190, then COMBIN(20, 18) = 190 too. Useful sanity check.

3. **COMBIN avoids overflow better than FACT:** COMBIN(100, 3) = 161,700 works fine. Computing 100!/(3!*97!) with FACT would overflow despite the same mathematical result.

4. **For probability, divide combinations:** P(event) often = (favorable combinations) / (total combinations). COMBIN(favorable, k) / COMBIN(total, k).

5. **Pascal's triangle identity:** COMBIN(n, k) = COMBIN(n-1, k-1) + COMBIN(n-1, k). Each entry is the sum of the two entries above it.

6. **Use COMBINA for combinations with repetition:** If items can be chosen multiple times (like scoops of ice cream), use COMBINA(n, k) = COMBIN(n+k-1, k).

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERMUT]] | Permutations P(n,k) | When order matters (arrangements, rankings) |
| [[COMBINA]] | Combinations with repetition | When items can be selected multiple times |
| [[FACT]] | Factorial n! | For the underlying factorial calculation |

### Commonly Used Together

**[[PERMUT]]** - Order-sensitive counting

*Compare approaches:*
```
=COMBIN(10, 3)                    → 120 (groups, order ignored)
=PERMUT(10, 3)                    → 720 (arrangements, order matters)
```
PERMUT = COMBIN * k! because each combination has k! orderings.

---

**[[FACT]]** - Manual combination formula

*Alternative calculation:*
```
=FACT(n)/(FACT(k)*FACT(n-k))      → Same as COMBIN(n, k)
```
COMBIN is simpler and avoids intermediate overflow.

---

**[[BINOM.DIST]]** - Binomial probability

*Uses COMBIN internally:*
```
=BINOM.DIST(k, n, p, FALSE)
=COMBIN(n,k) * p^k * (1-p)^(n-k)  → Same calculation
```
BINOM.DIST handles the full binomial distribution calculation.

---

**[[HYPGEOM.DIST]]** - Hypergeometric probability

*Sampling without replacement:*
```
=HYPGEOM.DIST(k, n, K, N, FALSE)
```
Uses combinations to calculate probability of k successes when sampling n items from N total containing K successes.

---

**[[PRODUCT]]** - Multi-stage combinations

*Multiple selection stages:*
```
=PRODUCT(COMBIN(10,3), COMBIN(8,2))
```
Multiply combinations for independent selections.

## Official Documentation

- **Microsoft Excel:** [COMBIN function](https://support.microsoft.com/en-us/office/combin-function-12a3f276-0a21-423a-8de6-06990aaf638a)
- **Google Sheets:** [COMBIN function](https://support.google.com/docs/answer/3093400)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2013+ | COMBINA added | Combinations with repetition |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
