---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- logarithm
- base-conversion
- exponential-inverse
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Logarithm
- Log Base
- General Logarithm
tags:
- logarithm
- log
- base
- exponential
- inverse
---

# LOG

## Description

**LOG** returns the logarithm of a number to a specified base—it answers the question "what power must I raise the base to in order to get this number?" LOG(100, 10) = 2 because 10^2 = 100. LOG(8, 2) = 3 because 2^3 = 8. If you omit the base, LOG defaults to base 10, making it equivalent to LOG10 in that case.

Logarithms are fundamental to **exponential analysis, growth rate calculations, and scientific notation**. They transform multiplicative relationships into additive ones—a property that makes them invaluable for analyzing compound growth, comparing orders of magnitude, and working with very large or small numbers. In the equation y = b^x, the logarithm solves for x: x = LOG(y, b).

**The relationship between logarithms and exponents is inverse.** LOG(POWER(base, exponent), base) = exponent, and POWER(base, LOG(value, base)) = value. This inverse relationship is how we solve for unknown exponents in equations. Financial calculations often require solving for the number of periods (n in FV = PV * (1+r)^n), which means n = LOG(FV/PV, 1+r).

**There are three main logarithm functions in spreadsheets.** LOG with a specified base handles any base. LOG10 is specifically base-10 (common logarithm). LN is the natural logarithm (base e, approximately 2.71828). LOG(x) with no base argument equals LOG10(x) in Excel/Sheets. All logarithms are related: LOG(x, b) = LN(x) / LN(b) = LOG10(x) / LOG10(b).

## Syntax

> [!f(x)] LOG Syntax
>
> ```
> =LOG(number, [base])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The positive real number for which you want the logarithm. Must be greater than zero (logarithm of zero or negative numbers is undefined). |
| `base` | No | The base of the logarithm. Must be positive and not equal to 1. Default is 10 if omitted. Common bases: 10 (common log), e (natural log), 2 (binary log). |

### Return Value

Returns the logarithm of the number to the specified base. The result is the exponent to which you must raise the base to get the number. Returns #NUM! for zero or negative numbers. Returns #DIV/0! if base is 1.

## Examples

> [!f(x)] LOG Examples

### Example 1: Base 10 Logarithm (Default)
```
=LOG(100)
```
**Result:** 2

**Explanation:** With no base specified, LOG uses base 10. 10^2 = 100, so LOG(100) = 2.

---

### Example 2: Base 10 Explicit
```
=LOG(1000, 10)
```
**Result:** 3

**Explanation:** 10^3 = 1000. The LOG tells us the power of 10 needed to make 1000.

---

### Example 3: Base 2 (Binary Logarithm)
```
=LOG(8, 2)
```
**Result:** 3

**Explanation:** 2^3 = 8. Base-2 logarithms are common in computer science for analyzing algorithm complexity and binary data.

---

### Example 4: Base 2 for Bits Calculation
```
=LOG(256, 2)
```
**Result:** 8

**Explanation:** 2^8 = 256. This tells us 256 values require 8 bits to represent (0-255).

---

### Example 5: Non-Integer Result
```
=LOG(50, 10)
```
**Result:** 1.699... (approximately)

**Explanation:** 10^1.699 approximately equals 50. Logarithms often produce non-integer results when the number isn't an exact power of the base.

---

### Example 6: Solving for Time in Compound Interest
```
=LOG(FutureValue/PresentValue, 1 + Rate)
```
**Result:** Number of periods needed

**Explanation:** If you want $1000 to grow to $2000 at 7% annual rate: =LOG(2000/1000, 1.07) = LOG(2, 1.07) = 10.24 years.

---

### Example 7: Base e (Natural Logarithm)
```
=LOG(7.389, EXP(1))
```
**Result:** 2 (approximately)

**Explanation:** e^2 is approximately 7.389. LOG(e^2, e) = 2. Note: LN(7.389) is the direct way to get natural log.

---

### Example 8: Change of Base Formula
```
=LOG(100, 10) = LN(100) / LN(10)
```
**Result:** Both equal 2

**Explanation:** Any logarithm can be computed using another base: LOG(x, b) = LN(x) / LN(b). This is the change of base formula.

---

### Example 9: Order of Magnitude
```
=INT(LOG(Value, 10))
```
**Result:** Order of magnitude (power of 10)

**Explanation:** For 5000, LOG gives 3.7, INT gives 3. The value is in the thousands (10^3 order).

---

### Example 10: Inverse of POWER
```
=LOG(POWER(3, 4), 3)
```
**Result:** 4

**Explanation:** POWER(3, 4) = 81, LOG(81, 3) = 4. Logarithm reverses exponentiation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number is zero or negative | Logarithm is only defined for positive numbers. Check that input is greater than zero. |
| `#DIV/0!` | Base is 1 | LOG base 1 is undefined (any power of 1 equals 1). Use a base other than 1. |
| `#NUM!` | Base is zero or negative | Base must be positive and not equal to 1. |
| `#VALUE!` | Non-numeric input | Ensure arguments are numbers, not text. |
| `#NAME?` | Misspelled function name | Verify spelling: LOG, not LOGG or LOGARITHM. |

## Use Cases

### [[Growth Rate and Time-to-Target Calculations]]

**Scenario:** Determine how long it takes for investments or metrics to reach target values.

**Implementation:**
```
=LOG(TargetValue/CurrentValue, 1 + GrowthRate)     → Periods to reach target
=LOG(2, 1.07)                                      → Years to double at 7%
=LOG(GoalSales/CurrentSales, 1 + MonthlyGrowth)    → Months to sales goal
```

**Business Application:** Investment planning, sales forecasting, resource planning. Any scenario where you need to know when a growing quantity will reach a threshold.

**Technical Details:** This solves FV = PV * (1+r)^n for n. The doubling time (Rule of 72 approximation) is exactly LOG(2, 1+r). For quick estimates, 72/r gives approximate years to double at r% annual rate.

---

### [[Order of Magnitude and Scale Analysis]]

**Scenario:** Determine the scale or order of magnitude of values for classification or analysis.

**Implementation:**
```
=INT(LOG(Value, 10))                               → Order of magnitude
=LOG(LargeValue, 10) - LOG(SmallValue, 10)         → Decades of difference
=POWER(10, ROUND(LOG(Value, 10), 0))               → Nearest power of 10
```

**Business Application:** Data classification, visualization scaling, scientific notation, comparing vastly different quantities.

**Technical Details:** LOG(x, 10) gives the number of digits minus 1 (for integers). The difference of logs equals the log of the ratio—useful for understanding relative scale.

---

### [[Information Theory and Computer Science]]

**Scenario:** Calculate bits needed, entropy, or algorithm complexity measures.

**Implementation:**
```
=CEILING(LOG(NumPossibilities, 2), 1)              → Bits needed to represent
=LOG(N, 2)                                         → Log base 2 for algorithm analysis
=-SUMPRODUCT(Probabilities * LOG(Probabilities, 2)) → Shannon entropy
```

**Business Application:** System design, password strength analysis, compression algorithms, database optimization.

**Technical Details:** Binary logarithm (base 2) measures information in bits. LOG(1024, 2) = 10 means 1024 items need 10 bits. Entropy calculations use log base 2 for bit-based measures.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Default base:** 10 when base argument is omitted
- **Precision:** 15 significant digits
- **Array support:** Works in array formulas and with dynamic arrays

### Google Sheets
- **Availability:** All versions
- **Default base:** 10 when base argument is omitted (same as Excel)
- **Precision:** Same as Excel
- **Array support:** Works with ARRAYFORMULA

### Both Platforms
- Identical behavior for all valid inputs
- Same default base (10)
- Same error conditions (#NUM!, #DIV/0!)
- Same mathematical precision

## Tips and Best Practices

1. **Remember the default base is 10:** LOG(100) = 2 uses base 10. For natural log, use LN(). For base 2, explicitly specify LOG(x, 2).

2. **Use LN for natural logarithm:** While LOG(x, EXP(1)) works, LN(x) is cleaner and avoids floating-point issues with representing e.

3. **Change of base formula:** LOG(x, b) = LN(x)/LN(b) = LOG10(x)/LOG10(b). Useful when you only have one log function available.

4. **Logarithms convert multiplication to addition:** LOG(A*B) = LOG(A) + LOG(B). This is why logarithmic scales are used for multiplicative data.

5. **Solve exponential equations with LOG:** To solve b^x = y for x, use x = LOG(y, b). This is the inverse of exponentiation.

6. **Check for positive input:** LOG of zero or negative is undefined. Use IF or IFERROR to handle edge cases in your data.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[LOG10]] | Base-10 logarithm | When you always want base 10 (clearer intent) |
| [[LN]] | Natural logarithm (base e) | For calculus, growth modeling, continuous compounding |
| [[EXP]] | e raised to a power | Inverse of LN |
| [[POWER]] | Exponentiation | The inverse operation of LOG |

### Commonly Used Together

**[[POWER]]** - Inverse operation

*Verify logarithm:*
```
=POWER(base, LOG(number, base))  → Returns number
=LOG(POWER(base, exp), base)     → Returns exp
```
POWER and LOG are inverse functions.

---

**[[EXP]]** - Inverse of natural log

*Reverse log transformation:*
```
=EXP(LN(Value))  → Returns Value
=POWER(10, LOG(Value))  → Returns Value for base 10
```
Use to convert from log scale back to original.

---

**[[LN]]** - Natural logarithm

*Change of base:*
```
=LN(Number) / LN(Base)  → Same as LOG(Number, Base)
```
Alternative calculation using natural log.

---

**[[INT]]** - Order of magnitude

*Classify by scale:*
```
=INT(LOG(Value, 10))
```
Get the power of 10 (hundreds, thousands, millions).

---

**[[IFERROR]]** - Handle invalid inputs

*Safe logarithm:*
```
=IFERROR(LOG(A1, 10), "Invalid")
```
Handle zero or negative inputs gracefully.

## Official Documentation

- **Microsoft Excel:** [LOG function](https://support.microsoft.com/en-us/office/log-function-4e82f196-1ca9-4747-8fb0-6c4a3abb3280)
- **Google Sheets:** [LOG function](https://support.google.com/docs/answer/3093495)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
