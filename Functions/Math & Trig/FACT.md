---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- factorial
- combinatorics
- permutations
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Factorial
- N Factorial
- Factorial Function
tags:
- factorial
- combinatorics
- permutations
- arrangements
- probability
---

# FACT

## Description

**FACT** calculates the factorial of a non-negative integer—the product of all positive integers up to and including that number. FACT(5) = 5! = 5 x 4 x 3 x 2 x 1 = 120. Factorial appears throughout mathematics, particularly in counting arrangements, permutations, combinations, and probability calculations.

Factorial grows extraordinarily fast. FACT(10) = 3,628,800. FACT(20) exceeds 2 quintillion (2.4 x 10^18). FACT(170) is about 7 x 10^306, near the spreadsheet's numeric limit. FACT(171) overflows to #NUM!. This explosive growth is why factorial appears in denominators to keep expressions finite—as in combinations and probability distributions.

**Factorial answers "how many ways?" questions.** How many ways can 5 people line up? FACT(5) = 120 ways. How many ways to arrange the letters in "HELLO"? It's more complex (repeated letters), but factorial is the foundation. The formula for permutations (P(n,r) = n!/(n-r)!) and combinations (C(n,r) = n!/r!(n-r)!) both use factorials.

By convention, **FACT(0) = 1**. This isn't arbitrary—it's necessary for mathematical consistency. Combinations C(n,n) = n!/n!0! must equal 1 (there's exactly one way to choose all items), which requires 0! = 1. The factorial function is also defined for non-integers via the gamma function, but FACT only works with integers (decimals are truncated).

## Syntax

> [!f(x)] FACT Syntax
>
> ```
> =FACT(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | A non-negative integer. Decimals are truncated (FACT(4.9) = FACT(4) = 24). Maximum is 170 due to spreadsheet numeric limits. |

### Return Value

Returns n! = n x (n-1) x (n-2) x ... x 2 x 1. Returns 1 for FACT(0) and FACT(1). Returns #NUM! for negative numbers or numbers > 170. Returns #VALUE! for non-numeric input.

## Examples

> [!f(x)] FACT Examples

### Example 1: Basic Factorial
```
=FACT(5)
```
**Result:** 120

**Explanation:** 5! = 5 x 4 x 3 x 2 x 1 = 120. The number of ways to arrange 5 distinct items.

---

### Example 2: Zero Factorial
```
=FACT(0)
```
**Result:** 1

**Explanation:** By definition, 0! = 1. This is needed for mathematical formulas to work consistently.

---

### Example 3: One Factorial
```
=FACT(1)
```
**Result:** 1

**Explanation:** 1! = 1. Only one way to arrange a single item.

---

### Example 4: Ten Factorial
```
=FACT(10)
```
**Result:** 3,628,800

**Explanation:** 10! = 3,628,800. Shows how quickly factorial grows—ten times larger input gives millions times larger output.

---

### Example 5: Decimal Input (Truncated)
```
=FACT(5.9)
```
**Result:** 120

**Explanation:** Decimals are truncated before calculation. FACT(5.9) = FACT(5) = 120.

---

### Example 6: Permutations P(n,r)
```
=FACT(N) / FACT(N - R)
```
**Result:** Number of permutations

**Explanation:** P(7,3) = FACT(7)/FACT(4) = 5040/24 = 210. The number of ways to arrange r items from n.

---

### Example 7: Combinations C(n,r)
```
=FACT(N) / (FACT(R) * FACT(N - R))
```
**Result:** Number of combinations

**Explanation:** C(7,3) = 7!/(3!*4!) = 5040/(6*24) = 35. Use COMBIN(7,3) for simpler syntax.

---

### Example 8: Large Factorial
```
=FACT(20)
```
**Result:** 2,432,902,008,176,640,000

**Explanation:** 20! is about 2.4 quintillion. Factorial growth is faster than exponential.

---

### Example 9: Maximum Factorial
```
=FACT(170)
```
**Result:** Approximately 7.26 x 10^306

**Explanation:** 170 is the largest input that doesn't overflow. FACT(171) returns #NUM!.

---

### Example 10: Arrangements with Repetition
```
=FACT(TotalLetters) / PRODUCT(FACT(RepeatCounts))
```
**Result:** Arrangements of items with repeats

**Explanation:** For "MISSISSIPPI" (11 letters: 1M, 4I, 4S, 2P): 11!/(1!*4!*4!*2!) = 34,650 arrangements.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Negative input | Factorial is undefined for negative integers. Ensure input is >= 0. |
| `#NUM!` | Input > 170 | Factorial of 171+ exceeds spreadsheet numeric limits. Consider logarithmic approach for very large factorials. |
| `#VALUE!` | Non-numeric input | Ensure argument is a number, not text. |
| `#NAME?` | Misspelled function name | Verify spelling: FACT, not FACTORIAL or FCT. |
| `Overflow in formula` | Intermediate factorial too large | When dividing factorials, simplify first or use COMBIN/PERMUT functions. |

## Use Cases

### [[Counting Arrangements and Permutations]]

**Scenario:** Calculate the number of ways to arrange items.

**Implementation:**
```
=FACT(N)                                           → Arrangements of N distinct items
=FACT(N) / FACT(N-R)                               → Permutations: r items from n (order matters)
=FACT(Total) / PRODUCT(FACT(Repeats))              → Arrangements with repeated items
```

**Business Application:** Scheduling, route planning, security (password possibilities), game design.

**Technical Details:** FACT(n) counts permutations of n distinct objects. For partial arrangements (choosing r from n), divide by FACT(n-r) to get P(n,r). Use PERMUT(n,r) for a direct function.

---

### [[Probability and Combinations]]

**Scenario:** Calculate probabilities using combinatorics.

**Implementation:**
```
=COMBIN(N, K) / COMBIN(Total, Sample)              → Probability of k successes
=FACT(N) / (FACT(K) * FACT(N-K))                   → Same as COMBIN(N,K)
=FACT(Favorable) * FACT(Unfavorable) / FACT(Total) → Arrangement probability
```

**Business Application:** Quality control (defect probability), gaming (odds), lottery calculations, statistical sampling.

**Technical Details:** Combination formula C(n,k) = n!/(k!(n-k)!). This counts ways to choose k items from n when order doesn't matter. Dividing combinations gives probabilities.

---

### [[Statistical Distributions]]

**Scenario:** Implement distribution formulas that use factorials.

**Implementation:**
```
=FACT(N) / (FACT(K) * FACT(N-K)) * P^K * (1-P)^(N-K)  → Binomial probability
=EXP(-Lambda) * Lambda^K / FACT(K)                 → Poisson probability
=FACT(N-1) * P * (1-P)^(N-1)                       → Negative binomial component
```

**Business Application:** Demand forecasting, insurance claims, quality control, call center staffing.

**Technical Details:** Many discrete distributions (binomial, Poisson, negative binomial) use factorials in their formulas. Excel's distribution functions (BINOM.DIST, POISSON.DIST) handle this internally.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Maximum input:** 170 (FACT(171) overflows)
- **Decimal handling:** Truncated to integer
- **Precision:** 15 significant digits (large factorials are approximate)

### Google Sheets
- **Availability:** All versions
- **Maximum input:** Same 170 limit
- **Decimal handling:** Same truncation
- **Precision:** Same as Excel

### Both Platforms
- Identical behavior for all valid inputs
- Same 0! = 1 convention
- Same overflow at 171
- Same truncation of decimals

## Tips and Best Practices

1. **Use COMBIN and PERMUT when possible:** COMBIN(n,r) and PERMUT(n,r) are more efficient than writing factorial ratios. They avoid intermediate overflow.

2. **Remember 0! = 1:** This is by definition and necessary for formulas to work. Not a bug!

3. **Watch for overflow:** FACT(171) and higher return #NUM!. For very large n, consider Stirling's approximation or work with logarithms.

4. **Decimals are truncated:** FACT(4.9) = FACT(4) = 24. Round or truncate explicitly if this matters.

5. **Simplify factorial ratios:** n!/k! = n x (n-1) x ... x (k+1). Computing directly avoids overflow when both factorials are large.

6. **Stirling's approximation for large n:** n! ≈ sqrt(2*pi*n) * (n/e)^n. Use for estimates when FACT overflows.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COMBIN]] | Combinations C(n,r) | When you need n choose r (avoids factorial overflow) |
| [[PERMUT]] | Permutations P(n,r) | When you need ordered arrangements from n |
| [[FACTDOUBLE]] | Double factorial | For n!! = n*(n-2)*(n-4)*... |

### Commonly Used Together

**[[COMBIN]]** - Combinations directly

*Compare approaches:*
```
=COMBIN(10, 3)                            → 120
=FACT(10)/(FACT(3)*FACT(7))               → 120 (same, more complex)
```
COMBIN is simpler and avoids intermediate overflow.

---

**[[PERMUT]]** - Permutations directly

*Compare approaches:*
```
=PERMUT(10, 3)                            → 720
=FACT(10)/FACT(7)                         → 720 (same)
```
PERMUT handles ordered arrangements efficiently.

---

**[[PRODUCT]]** - Factorial of a sequence

*Manual factorial:*
```
=PRODUCT(SEQUENCE(5))                     → 120 (same as FACT(5))
=PRODUCT({1,2,3,4,5})                     → 120
```
Alternative calculation method.

---

**[[GAMMALN]]** - Log of factorial

*For large values:*
```
=GAMMALN(N+1)                             → ln(N!)
=EXP(GAMMALN(N+1))                        → N! (but may overflow)
```
Gamma function extends factorial; GAMMALN handles larger values.

---

**[[EXP]]** - Stirling approximation

*Estimate large factorial:*
```
=SQRT(2*PI()*N) * POWER(N/EXP(1), N)
```
Approximates N! for large N without overflow.

## Official Documentation

- **Microsoft Excel:** [FACT function](https://support.microsoft.com/en-us/office/fact-function-ca8588c2-15f2-41c0-8e8c-c11bd471a4f3)
- **Google Sheets:** [FACT function](https://support.google.com/docs/answer/3093412)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
