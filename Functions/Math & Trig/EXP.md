---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- exponential
- euler-number
- growth
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Exponential Function
- E Power
- Natural Exponential
tags:
- exponential
- exp
- euler
- growth
- compound
---

# EXP

## Description

**EXP** returns e raised to the power of a given number—it calculates e^x where e is Euler's number (approximately 2.71828). EXP(1) returns e itself (about 2.71828), EXP(2) returns e^2 (about 7.389), and EXP(0) returns 1 (any number to the zero power is 1). This is the natural exponential function, fundamental to mathematics, science, and finance.

EXP is the **inverse of the LN function**. While LN asks "what power of e gives this number?", EXP performs the exponentiation: EXP(x) = e^x. The identity LN(EXP(x)) = x and EXP(LN(x)) = x shows their inverse relationship. Together they form a powerful pair for solving exponential equations and transforming data.

**Why is e important?** The number e is the unique base where the rate of growth equals the current amount. If something grows continuously at 100% rate, after 1 unit of time it becomes e times larger. This makes e natural for **continuous compound interest, population dynamics, radioactive decay, and any process involving continuous change**. The formula for continuous compounding is FV = PV * EXP(r * t).

EXP grows very fast—EXP(10) is about 22,026; EXP(100) exceeds 10^43. Conversely, EXP of negative numbers gives fractions approaching zero: EXP(-1) ≈ 0.368, EXP(-10) ≈ 0.0000454. This makes EXP useful for probability distributions (normal, exponential), decay functions, and sigmoid transformations.

## Syntax

> [!f(x)] EXP Syntax
>
> ```
> =EXP(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The exponent to which e is raised. Can be any real number—positive (exponential growth), negative (exponential decay), or zero (returns 1). The result is e^number. |

### Return Value

Returns e raised to the power of the specified number. The result is always positive. Returns #VALUE! if the argument is non-numeric. Very large positive numbers may return #NUM! due to overflow (e^709 is about the limit).

## Examples

> [!f(x)] EXP Examples

### Example 1: EXP of Zero
```
=EXP(0)
```
**Result:** 1

**Explanation:** e^0 = 1. Any number to the zero power equals 1.

---

### Example 2: EXP of 1 (Value of e)
```
=EXP(1)
```
**Result:** 2.718281828...

**Explanation:** e^1 = e. This is how you get the value of Euler's number in spreadsheets.

---

### Example 3: EXP of 2
```
=EXP(2)
```
**Result:** 7.389056099...

**Explanation:** e^2 ≈ 7.389. Each unit increase in the exponent multiplies the result by e.

---

### Example 4: Negative Exponent
```
=EXP(-1)
```
**Result:** 0.367879441...

**Explanation:** e^(-1) = 1/e ≈ 0.368. Negative exponents give reciprocals.

---

### Example 5: Continuous Compound Interest
```
=Principal * EXP(Rate * Years)
```
**Result:** Future value with continuous compounding

**Explanation:** $1000 at 5% for 10 years: =1000 * EXP(0.05 * 10) = $1648.72. Compare to annual: 1000 * 1.05^10 = $1628.89.

---

### Example 6: Exponential Decay
```
=InitialAmount * EXP(-DecayRate * Time)
```
**Result:** Remaining amount after decay

**Explanation:** Radioactive decay, drug metabolism, cooling—all follow this pattern. Half-life occurs when EXP(-k*t) = 0.5.

---

### Example 7: Normal Distribution PDF
```
=EXP(-POWER(x - Mean, 2) / (2 * Variance)) / SQRT(2 * PI() * Variance)
```
**Result:** Normal probability density

**Explanation:** The bell curve formula uses EXP of a negative squared term, creating the characteristic symmetric decay.

---

### Example 8: Logistic/Sigmoid Function
```
=1 / (1 + EXP(-x))
```
**Result:** Sigmoid output (0 to 1)

**Explanation:** The logistic function transforms any real number to the range (0, 1). Used in probability, machine learning, and growth models.

---

### Example 9: Reverse Natural Log
```
=EXP(LN(Value))
```
**Result:** Original value

**Explanation:** EXP and LN are inverse functions. This identity always returns the original value (for positive inputs).

---

### Example 10: Convert Continuous to Discrete Rate
```
=EXP(ContinuousRate) - 1
```
**Result:** Equivalent discrete/periodic rate

**Explanation:** A 4.879% continuous rate = EXP(0.04879) - 1 = 5% annual discrete rate.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number, not text. |
| `#NUM!` | Exponent too large | EXP overflows around 709+. For such large values, work in log space. |
| `#NAME?` | Misspelled function name | Verify spelling: EXP, not EXPO or E. |
| `Unexpected near-zero` | Large negative exponent | Correct behavior—EXP(-20) ≈ 2 x 10^-9. Large negative exponents approach zero. |
| `Precision limit` | Very small results | EXP of very negative numbers may underflow to exactly 0. |

## Use Cases

### [[Continuous Compounding and Finance]]

**Scenario:** Calculate growth with continuous compounding or convert between rate conventions.

**Implementation:**
```
=PV * EXP(Rate * Time)                             → Future value, continuous
=EXP(ContinuousRate) - 1                           → Convert to discrete annual
=LN(1 + DiscreteRate)                              → Convert to continuous rate
=EXP(LN(FV/PV) / Time) - 1                         → Solve for discrete rate
```

**Business Application:** Option pricing (Black-Scholes), advanced fixed income, risk modeling. Continuous compounding simplifies calculus-based financial models.

**Technical Details:** Continuous compounding is the limit as compounding frequency approaches infinity. EXP(r) is always slightly larger than (1+r) for positive r, reflecting the additional value of infinitely frequent compounding.

---

### [[Probability and Statistical Distributions]]

**Scenario:** Implement probability distributions and statistical models.

**Implementation:**
```
=Lambda * EXP(-Lambda * x)                         → Exponential distribution PDF
=EXP(-POWER(x-Mu,2)/(2*Sigma^2))/(Sigma*SQRT(2*PI()))  → Normal PDF
=1 / (1 + EXP(-x))                                 → Logistic/sigmoid function
=EXP(x) / SUM(EXP(AllValues))                      → Softmax (for arrays)
```

**Business Application:** Risk analysis, survival analysis, machine learning, quality control. Many probability distributions use EXP in their formulas.

**Technical Details:** The exponential distribution models time between events. The normal distribution uses EXP of negative squared deviation. Softmax transforms scores into probabilities that sum to 1.

---

### [[Growth and Decay Modeling]]

**Scenario:** Model exponential growth (population, infections) or decay (radioactive, depreciation).

**Implementation:**
```
=InitialPopulation * EXP(GrowthRate * Time)        → Population growth
=InitialAmount * EXP(-DecayConstant * Time)        → Radioactive/exponential decay
=Capacity / (1 + EXP(-k * (Time - Midpoint)))      → Logistic growth (S-curve)
=LN(2) / DecayConstant                             → Half-life from decay constant
```

**Business Application:** Epidemiology, pharmacokinetics, marketing adoption curves, equipment depreciation.

**Technical Details:** Pure exponential growth is unsustainable; real systems often follow logistic (S-curve) growth that levels off. The logistic function combines EXP with a capacity limit.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Precision:** 15 significant digits
- **Overflow:** Returns #NUM! around EXP(709.78)
- **Underflow:** Very negative exponents return 0 (below 10^-307)

### Google Sheets
- **Availability:** All versions
- **Precision:** Same as Excel
- **Overflow/Underflow:** Same behavior
- **Array support:** Works with ARRAYFORMULA

### Both Platforms
- Identical behavior for all typical inputs
- Same precision and overflow limits
- EXP(1) gives the same value of e on both
- LN(EXP(x)) = x identity holds on both

## Tips and Best Practices

1. **EXP(1) gives you e:** Need Euler's number? Just use EXP(1) ≈ 2.71828. This is more precise than typing the constant.

2. **EXP is the inverse of LN:** EXP(LN(x)) = x and LN(EXP(x)) = x. Use this to solve exponential equations.

3. **Use for continuous compounding:** FV = PV * EXP(rate * time) is the formula for continuous compound growth. Slightly higher than discrete compounding.

4. **Sigmoid with EXP:** 1/(1+EXP(-x)) creates an S-curve from 0 to 1. Used in probability and machine learning.

5. **Watch for overflow:** EXP(709) is about 10^307; larger exponents overflow. Work in log space for very large calculations.

6. **Geometric mean via EXP:** EXP(AVERAGE(LN(values))) gives the geometric mean. Useful for averaging rates.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[LN]] | Natural logarithm | The inverse operation—LN reverses EXP |
| [[POWER]] | Any base to any power | When base isn't e (e.g., 2^x, 10^x) |
| [[LOG]] | Logarithm with any base | Inverse for non-e bases |

### Commonly Used Together

**[[LN]]** - Inverse operation

*Solve exponential equations:*
```
=LN(EXP(Value))  → Returns Value
=EXP(LN(Value))  → Returns Value
```
LN and EXP undo each other.

---

**[[PI]]** - Statistical formulas

*Normal distribution:*
```
=EXP(-x^2/2) / SQRT(2*PI())
```
Standard normal probability density.

---

**[[POWER]]** - Compare bases

*Different exponentials:*
```
=EXP(x)        → e^x (base e)
=POWER(10, x)  → 10^x (base 10)
=POWER(2, x)   → 2^x (base 2)
```
EXP is specifically for base e.

---

**[[AVERAGE]]** - Geometric mean

*Average multiplicative factors:*
```
=EXP(AVERAGE(LN(Values)))
```
Geometric mean using log-transform approach.

---

**[[SUM]]** - Softmax denominator

*Probability normalization:*
```
=EXP(Score) / SUM(EXP(AllScores))
```
Converts scores to probabilities.

## Official Documentation

- **Microsoft Excel:** [EXP function](https://support.microsoft.com/en-us/office/exp-function-c578f034-2c45-4c37-bc8c-329660a63abe)
- **Google Sheets:** [EXP function](https://support.google.com/docs/answer/3093411)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
