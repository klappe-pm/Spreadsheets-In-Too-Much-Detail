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
- common-log
- base-ten
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Common Logarithm
- Base 10 Log
- Decimal Logarithm
tags:
- logarithm
- log10
- common-log
- base-ten
- scientific
---

# LOG10

## Description

**LOG10** returns the base-10 logarithm of a number—the power to which 10 must be raised to equal that number. LOG10(100) = 2 because 10^2 = 100. LOG10(1000) = 3 because 10^3 = 1000. This is the "common logarithm" widely used in science, engineering, and everyday calculations involving orders of magnitude.

Base-10 logarithms are natural for human understanding because our number system is decimal. LOG10 tells you "how many digits" in a sense—LOG10(999) is about 3, LOG10(1000) is exactly 3, LOG10(1001) is slightly over 3. This makes LOG10 perfect for **scientific notation, decibel calculations, pH measurements, and Richter scale magnitudes**.

**LOG10 vs LOG vs LN:** LOG10 is specifically base 10. LOG defaults to base 10 if no base is specified, making LOG(x) equivalent to LOG10(x). LN is the natural logarithm (base e). In spreadsheets, LOG10 is clearer for intent when you specifically want base-10. Some programming languages have log() as natural log, so LOG10 avoids ambiguity.

The key property of logarithms applies: **LOG10(A*B) = LOG10(A) + LOG10(B)**. This transforms multiplication into addition, which is why logarithmic scales (like decibels) convert ratios into additive differences. Doubling a sound's intensity adds about 3 dB because LOG10(2) is approximately 0.3, times 10 gives 3 dB.

## Syntax

> [!f(x)] LOG10 Syntax
>
> ```
> =LOG10(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The positive number for which you want the base-10 logarithm. Must be greater than zero. Represents the value in the equation 10^x = number, where x is the result. |

### Return Value

Returns the base-10 logarithm of the number—the exponent to which 10 must be raised to produce the number. Returns #NUM! for zero or negative numbers.

## Examples

> [!f(x)] LOG10 Examples

### Example 1: Exact Power of 10
```
=LOG10(100)
```
**Result:** 2

**Explanation:** 10^2 = 100, so LOG10(100) = 2. Powers of 10 give integer logarithms.

---

### Example 2: Larger Power of 10
```
=LOG10(1000000)
```
**Result:** 6

**Explanation:** 10^6 = 1,000,000. LOG10 gives 6, confirming this is a million (10 to the 6th).

---

### Example 3: Value of 1
```
=LOG10(1)
```
**Result:** 0

**Explanation:** 10^0 = 1. Any number to the power of 0 equals 1, so LOG10(1) = 0.

---

### Example 4: Value Less Than 1
```
=LOG10(0.01)
```
**Result:** -2

**Explanation:** 10^(-2) = 0.01. Negative logarithms indicate values less than 1.

---

### Example 5: Non-Power of 10
```
=LOG10(50)
```
**Result:** 1.699... (approximately)

**Explanation:** 10^1.699 is approximately 50. Most numbers produce non-integer logarithms.

---

### Example 6: Decibel Calculation
```
=10 * LOG10(Power2/Power1)
```
**Result:** Gain in decibels

**Explanation:** Decibels measure ratios logarithmically. A power ratio of 100 = 10*LOG10(100) = 20 dB.

---

### Example 7: pH Calculation
```
=-LOG10(HydrogenIonConcentration)
```
**Result:** pH value

**Explanation:** pH is the negative log of hydrogen ion concentration. [H+] of 10^-7 gives pH 7 (neutral).

---

### Example 8: Order of Magnitude
```
=INT(LOG10(Value))
```
**Result:** Order of magnitude

**Explanation:** For 5000, LOG10 = 3.7, INT gives 3. The number is in the thousands (10^3 scale).

---

### Example 9: Number of Digits
```
=INT(LOG10(ABS(Number))) + 1
```
**Result:** Number of digits (for positive integers)

**Explanation:** 999 gives LOG10 = 2.999, INT = 2, +1 = 3 digits. 1000 gives LOG10 = 3, INT = 3, +1 = 4 digits.

---

### Example 10: Scientific Notation Exponent
```
=INT(LOG10(ABS(Value)))
```
**Result:** Exponent for scientific notation

**Explanation:** For 4,500,000, LOG10 = 6.65, INT = 6. Scientific notation: 4.5 x 10^6.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number is zero or negative | LOG10 is only defined for positive numbers. Validate input before calculation. |
| `#VALUE!` | Non-numeric input | Ensure the argument is a number, not text. |
| `#NAME?` | Misspelled function name | Verify spelling: LOG10, not LOG_10 or LOGTEN. |
| `Unexpected negative` | Value between 0 and 1 | This is correct—LOG10(0.5) is approximately -0.3. Values < 1 have negative logs. |
| `Result not integer` | Value isn't power of 10 | Most values produce decimal logarithms. Only exact powers of 10 give integers. |

## Use Cases

### [[Decibel and Sound Level Calculations]]

**Scenario:** Calculate sound levels, signal gains, and audio engineering metrics.

**Implementation:**
```
=10 * LOG10(PowerRatio)                            → Power ratio in dB
=20 * LOG10(VoltageRatio)                          → Voltage ratio in dB
=10 * LOG10(SUM(POWER(10, dBLevels/10)))           → Combine sound sources
```

**Business Application:** Audio engineering, telecommunications, signal processing. Decibels are the standard unit for logarithmic ratios.

**Technical Details:** Decibels use LOG10 because a 10x power increase = 10 dB, 100x = 20 dB. For voltage/amplitude, multiply by 20 instead of 10 because power is proportional to voltage squared.

---

### [[Scientific Notation and Data Formatting]]

**Scenario:** Express or analyze very large or small numbers using scientific notation.

**Implementation:**
```
=INT(LOG10(ABS(Value)))                            → Exponent for scientific notation
=Value / POWER(10, INT(LOG10(ABS(Value))))         → Mantissa (1 to 10)
=TEXT(Value, "0.00E+00")                           → Format as scientific (alternative)
```

**Business Application:** Scientific data, financial large numbers, technical specifications. Scientific notation makes very large/small numbers readable.

**Technical Details:** In scientific notation, 4,500,000 = 4.5 x 10^6. LOG10 gives the exponent (6), and dividing by 10^6 gives the mantissa (4.5).

---

### [[pH and Chemistry Calculations]]

**Scenario:** Calculate pH, pKa, and other negative log-based chemistry metrics.

**Implementation:**
```
=-LOG10(H_concentration)                           → pH from [H+]
=POWER(10, -pH)                                    → [H+] from pH
=-LOG10(Ka)                                        → pKa from acid constant
```

**Business Application:** Chemistry, water quality, food science, pharmaceutical development. pH scale is universally used for acidity.

**Technical Details:** pH is defined as -LOG10([H+]). Neutral water has [H+] = 10^-7, so pH = 7. Each pH unit represents a 10x change in hydrogen ion concentration.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Precision:** 15 significant digits
- **Alternative:** LOG(x) with no base also gives base-10
- **Array support:** Works in array formulas and dynamic arrays

### Google Sheets
- **Availability:** All versions
- **Precision:** Same as Excel
- **Alternative:** LOG(x) also defaults to base 10
- **Array support:** Works with ARRAYFORMULA

### Both Platforms
- Identical behavior for all valid inputs
- Same #NUM! error for non-positive inputs
- Same mathematical precision
- LOG10(x) = LOG(x) = LOG(x, 10)

## Tips and Best Practices

1. **LOG10 is the common logarithm:** When scientists say "log" without specifying base, they usually mean log base 10. LOG10 makes this explicit in your formulas.

2. **Decibel calculations use LOG10:** The formula is dB = 10 * LOG10(power_ratio) or 20 * LOG10(voltage_ratio). This is essential for audio and signal processing.

3. **INT(LOG10(x)) gives order of magnitude:** For 5000, this returns 3 (thousands). For 500, returns 2 (hundreds). Quick way to classify numbers by scale.

4. **Number of digits = INT(LOG10(n)) + 1:** For positive integers, this formula counts decimal digits. 999 has 3 digits, 1000 has 4.

5. **Negative log is common in chemistry:** pH = -LOG10([H+]). The negative makes the scale intuitive (higher pH = less acidic).

6. **Check for positive input:** LOG10 of zero or negative is undefined. Use IFERROR or IF to handle potential bad data.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[LOG]] | Logarithm with any base | When you need a different base (2, e, etc.) |
| [[LN]] | Natural logarithm (base e) | For calculus, continuous growth, statistical formulas |
| [[POWER]] | Exponentiation | The inverse operation—10^LOG10(x) = x |

### Commonly Used Together

**[[POWER]]** - Inverse operation

*Convert back from logarithm:*
```
=POWER(10, LOG10(Value))  → Returns Value
=POWER(10, dB/10)         → Ratio from decibels
```
POWER reverses the logarithm transformation.

---

**[[INT]]** - Extract order of magnitude

*Classify by scale:*
```
=INT(LOG10(Revenue))
```
Returns 3 for thousands, 6 for millions, 9 for billions.

---

**[[ABS]]** - Handle sign for digit counting

*Count digits in any number:*
```
=INT(LOG10(ABS(Number))) + 1
```
ABS handles negative numbers; LOG10 only takes positive.

---

**[[TEXT]]** - Scientific notation formatting

*Alternative display:*
```
=TEXT(Value, "0.00E+00")
```
Formats using scientific notation directly.

---

**[[IFERROR]]** - Handle invalid inputs

*Safe calculation:*
```
=IFERROR(LOG10(A1), "N/A")
```
Returns "N/A" for zero or negative values.

## Official Documentation

- **Microsoft Excel:** [LOG10 function](https://support.microsoft.com/en-us/office/log10-function-c75b881b-49dd-44fb-b6f4-37e3486a0211)
- **Google Sheets:** [LOG10 function](https://support.google.com/docs/answer/3093423)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
