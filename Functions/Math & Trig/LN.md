---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- natural-logarithm
- exponential
- calculus
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Natural Logarithm
- Log Base e
- Ln Function
tags:
- natural-log
- ln
- euler
- exponential
- calculus
---

# LN

## Description

**LN** returns the natural logarithm of a number—the logarithm to base e (Euler's number, approximately 2.71828). LN asks "what power of e gives this number?" LN(e) = 1, LN(e^2) = 2, LN(1) = 0. The natural logarithm is called "natural" because it arises naturally in calculus, continuous growth, and the fundamental mathematics of change.

The natural logarithm is the **inverse of the EXP function**. EXP(x) calculates e^x, and LN reverses this: LN(EXP(x)) = x and EXP(LN(x)) = x. This inverse relationship is essential for solving equations involving exponential growth, continuous compounding, and many scientific formulas.

**Why is e special?** The number e is the base rate of growth shared by all continually growing processes. When something grows at a continuous rate r, after time t the growth factor is e^(rt). This makes e and LN fundamental to **continuous compound interest, population dynamics, radioactive decay, and differential equations**. The formula for continuous compounding is FV = PV * e^(rt) = PV * EXP(r*t).

**LN vs LOG10:** Both are logarithms, but with different bases. LN uses base e (natural), LOG10 uses base 10 (common). They're related: LN(x) = LOG10(x) * LN(10), and LOG10(x) = LN(x) / LN(10). Use LN for calculus-based formulas, continuous growth, and most statistical functions. Use LOG10 for order of magnitude, decibels, and pH.

## Syntax

> [!f(x)] LN Syntax
>
> ```
> =LN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The positive number for which you want the natural logarithm. Must be greater than zero. This is the value for which you're finding the exponent in the equation e^x = number. |

### Return Value

Returns the natural logarithm (base e) of the number—the exponent to which e must be raised to produce the number. Returns #NUM! for zero or negative numbers.

## Examples

> [!f(x)] LN Examples

### Example 1: Natural Log of e
```
=LN(2.71828)
```
**Result:** 1 (approximately)

**Explanation:** e^1 = e, so LN(e) = 1. This is the defining property of the natural log.

---

### Example 2: Natural Log of 1
```
=LN(1)
```
**Result:** 0

**Explanation:** e^0 = 1 (any number to the zero power is 1), so LN(1) = 0.

---

### Example 3: Natural Log of e Squared
```
=LN(EXP(2))
```
**Result:** 2

**Explanation:** EXP(2) = e^2 ≈ 7.389. LN(e^2) = 2. LN is the inverse of EXP.

---

### Example 4: Continuous Compound Growth Rate
```
=LN(FutureValue / PresentValue) / Time
```
**Result:** Continuous growth rate

**Explanation:** If $100 grows to $150 in 5 years with continuous compounding, the rate is LN(150/100)/5 = LN(1.5)/5 ≈ 0.0811 or 8.11% continuous.

---

### Example 5: Doubling Time with Continuous Growth
```
=LN(2) / ContinuousRate
```
**Result:** Time to double

**Explanation:** LN(2) ≈ 0.693. At 10% continuous rate: 0.693/0.10 = 6.93 years to double.

---

### Example 6: Convert Discrete to Continuous Rate
```
=LN(1 + DiscreteRate)
```
**Result:** Equivalent continuous rate

**Explanation:** A 5% annual discrete rate = LN(1.05) ≈ 0.0488 or 4.88% continuous rate.

---

### Example 7: Geometric Mean via LN
```
=EXP(AVERAGE(LN(Values)))
```
**Result:** Geometric mean of values

**Explanation:** The geometric mean equals EXP of the average of LN values. Essential for averaging rates and ratios.

---

### Example 8: Log-Normal Distribution
```
=LN(DataPoint)
```
**Result:** Log-transformed data

**Explanation:** Many financial and natural datasets follow log-normal distributions. Taking LN normalizes them for statistical analysis.

---

### Example 9: Half-Life Calculation
```
=LN(0.5) / DecayConstant
```
**Result:** Half-life

**Explanation:** Half-life is when quantity reaches 50%. LN(0.5) ≈ -0.693. Common in radioactive decay and drug metabolism.

---

### Example 10: Elasticity Calculation
```
=LN(Q2/Q1) / LN(P2/P1)
```
**Result:** Price elasticity of demand

**Explanation:** Elasticity measures percentage change in quantity relative to percentage change in price. Using LN gives arc elasticity.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number is zero or negative | LN is only defined for positive numbers. Check input validity. |
| `#VALUE!` | Non-numeric input | Ensure the argument is a number, not text. |
| `#NAME?` | Misspelled function name | Verify spelling: LN, not LIN or LOG_E. |
| `Result is negative` | Input is between 0 and 1 | Correct behavior—LN(0.5) ≈ -0.693. Values < 1 have negative natural logs. |
| `Confused with LOG10` | Different base | LN uses base e, LOG10 uses base 10. LN(10) ≈ 2.303, LOG10(10) = 1. |

## Use Cases

### [[Continuous Compound Interest and Growth]]

**Scenario:** Calculate growth with continuous compounding or convert between rate types.

**Implementation:**
```
=PV * EXP(Rate * Time)                             → Future value with continuous compounding
=LN(FV / PV) / Time                                → Continuous rate from growth
=LN(1 + AnnualRate)                                → Convert annual to continuous
=EXP(ContinuousRate) - 1                           → Convert continuous to annual
```

**Business Application:** Investment analysis, option pricing, advanced financial modeling. Continuous compounding is the limit of more frequent compounding.

**Technical Details:** Continuous compounding means interest is added infinitely often. The formula FV = PV * e^(rt) is cleaner for calculus than discrete compounding. Black-Scholes option pricing uses continuous rates.

---

### [[Statistical Transformations and Normalization]]

**Scenario:** Transform skewed data for statistical analysis.

**Implementation:**
```
=LN(DataPoint)                                     → Log transformation
=EXP(AVERAGE(LN(Values)))                          → Geometric mean
=EXP(PERCENTILE(LN(Values), 0.5))                  → Geometric median
=STDEV(LN(Values))                                 → Log-space standard deviation
```

**Business Application:** Financial analysis, biological data, income distributions. Many real-world datasets are log-normally distributed.

**Technical Details:** When data spans orders of magnitude or is right-skewed, LN transformation often creates a normal distribution suitable for parametric statistics. The geometric mean is the appropriate average for ratios and rates.

---

### [[Growth Rate and Elasticity Analysis]]

**Scenario:** Calculate growth rates and economic elasticities using logarithmic differences.

**Implementation:**
```
=LN(Value2) - LN(Value1)                           → Log difference (approx % change)
=(LN(Q2) - LN(Q1)) / (LN(P2) - LN(P1))             → Price elasticity
=LN(2) / GrowthRate                                → Doubling time
=LN(0.5) / DecayRate                               → Half-life
```

**Business Application:** Economics, market research, growth analysis. Log differences approximate percentage changes and are additive.

**Technical Details:** For small changes, LN(1+r) ≈ r. Log differences are the standard for calculating returns in finance (log returns) because they're time-additive: total log return = sum of period log returns.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Precision:** 15 significant digits
- **Value of e:** EXP(1) gives e ≈ 2.718281828459045
- **Array support:** Works in array formulas and dynamic arrays

### Google Sheets
- **Availability:** All versions
- **Precision:** Same as Excel
- **Value of e:** Same as Excel
- **Array support:** Works with ARRAYFORMULA

### Both Platforms
- Identical behavior for all valid inputs
- Same #NUM! error for non-positive inputs
- Same mathematical precision
- LN(EXP(x)) = x and EXP(LN(x)) = x for all valid x

## Tips and Best Practices

1. **LN is the inverse of EXP:** LN(EXP(x)) = x and EXP(LN(x)) = x. Use this relationship to solve exponential equations.

2. **Use LN for continuous growth:** When growth is continuous (finance, biology, physics), LN provides the natural framework. Discrete growth uses LOG with appropriate base.

3. **Geometric mean via LN:** EXP(AVERAGE(LN(values))) gives the geometric mean, which is the proper average for ratios, rates, and multiplicative processes.

4. **LN differences approximate percentage changes:** LN(new) - LN(old) ≈ (new-old)/old for small changes. This is why log returns are used in finance.

5. **Know the special values:** LN(1) = 0, LN(e) = 1, LN(e^n) = n. These help verify your formulas.

6. **Convert between log bases:** LN(x) = LOG10(x) * 2.303... (where 2.303 ≈ LN(10)). Use LOG(x, base) for arbitrary bases.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[EXP]] | e raised to a power | The inverse operation—EXP is to LN as exponent is to logarithm |
| [[LOG]] | Logarithm with any base | When you need a different base than e |
| [[LOG10]] | Base-10 logarithm | For scientific notation, decibels, pH |

### Commonly Used Together

**[[EXP]]** - Inverse operation

*Reverse natural log:*
```
=EXP(LN(Value))  → Returns Value
=EXP(Rate * Time)  → Continuous compound growth
```
EXP and LN are inverse functions.

---

**[[AVERAGE]]** - Geometric mean calculation

*Average of log-transformed data:*
```
=EXP(AVERAGE(LN(A1:A100)))  → Geometric mean
```
Log transform, average, then exponentiate back.

---

**[[STDEV]]** - Log-space statistics

*Variability in log scale:*
```
=STDEV(LN(Values))
```
Standard deviation of log values for multiplicative data.

---

**[[POWER]]** - Alternative for other bases

*Exponential with any base:*
```
=POWER(Base, Exponent)
=EXP(Exponent * LN(Base))  → Equivalent formula
```
Any exponential can be written using e and LN.

---

**[[IFERROR]]** - Handle invalid inputs

*Safe calculation:*
```
=IFERROR(LN(A1), "Invalid")
```
Gracefully handle zero or negative values.

## Official Documentation

- **Microsoft Excel:** [LN function](https://support.microsoft.com/en-us/office/ln-function-81fe1ed7-dac9-4acd-ba1d-07a142c6118f)
- **Google Sheets:** [LN function](https://support.google.com/docs/answer/3093422)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
