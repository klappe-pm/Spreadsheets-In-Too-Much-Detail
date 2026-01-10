---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- divisibility
- common-divisor
- number-theory
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Greatest Common Divisor
- Greatest Common Factor
- Highest Common Factor
tags:
- gcd
- gcf
- divisor
- factor
- number-theory
---

# GCD

## Description

**GCD** returns the greatest common divisor of two or more integers—the largest positive integer that divides all the given numbers without leaving a remainder. GCD(12, 18) returns 6 because 6 is the largest number that divides both 12 and 18 evenly. Also known as greatest common factor (GCF) or highest common factor (HCF).

This function is fundamental to **fraction simplification, ratio reduction, and number theory**. To simplify a fraction like 18/24, divide both numerator and denominator by their GCD (6), giving 3/4. GCD is also essential in cryptography, modular arithmetic, and any scenario involving common patterns in repeated intervals.

**GCD works with multiple numbers.** GCD(12, 18, 30) returns 6—the largest number dividing all three. The function accepts up to 255 arguments, making it practical for finding common factors across large sets of values. Each argument can be a number, cell reference, or range.

The mathematical foundation is Euclid's algorithm, one of the oldest algorithms still in use. The key property: GCD(a, b) = GCD(b, a mod b), recursively reducing to GCD(x, 0) = x. This makes GCD computation very efficient even for large numbers. The relationship GCD(a,b) * LCM(a,b) = a * b connects GCD to its partner function LCM.

## Syntax

> [!f(x)] GCD Syntax
>
> ```
> =GCD(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first integer (or range of integers). Decimals are truncated. Negative numbers are treated as their absolute values. |
| `number2, ...` | No | Additional integers to include. Up to 255 arguments. All must be non-negative after truncation. |

### Return Value

Returns the greatest common divisor—the largest integer that divides all arguments evenly. Returns 0 if any argument is 0 (or all are 0). Returns #VALUE! for non-numeric input. Returns #NUM! for negative results after truncation.

## Examples

> [!f(x)] GCD Examples

### Example 1: Two Numbers
```
=GCD(12, 18)
```
**Result:** 6

**Explanation:** 6 divides both 12 (12/6=2) and 18 (18/6=3) evenly. No larger number does.

---

### Example 2: Coprime Numbers
```
=GCD(8, 15)
```
**Result:** 1

**Explanation:** 8 and 15 share no common factors except 1. Numbers with GCD of 1 are called coprime or relatively prime.

---

### Example 3: One Divides the Other
```
=GCD(12, 48)
```
**Result:** 12

**Explanation:** When one number divides the other evenly, the GCD is the smaller number.

---

### Example 4: Three Numbers
```
=GCD(24, 36, 60)
```
**Result:** 12

**Explanation:** 12 is the largest number dividing 24, 36, and 60 evenly.

---

### Example 5: Simplify a Fraction
```
=A1/GCD(A1, B1) & "/" & B1/GCD(A1, B1)
```
**Result:** Simplified fraction

**Explanation:** For 18/24: GCD=6, result is "3/4". Dividing both by GCD gives lowest terms.

---

### Example 6: Same Number Twice
```
=GCD(15, 15)
```
**Result:** 15

**Explanation:** Any number's GCD with itself is that number.

---

### Example 7: Including Zero
```
=GCD(12, 0)
```
**Result:** 12

**Explanation:** GCD(n, 0) = n. Zero is divisible by all integers, so the GCD is the other number.

---

### Example 8: Using a Range
```
=GCD(A1:A5)
```
**Result:** GCD of all values in range

**Explanation:** If A1:A5 contains {12, 18, 24, 30, 36}, GCD returns 6.

---

### Example 9: Decimal Inputs (Truncated)
```
=GCD(12.9, 18.1)
```
**Result:** 6

**Explanation:** Decimals are truncated to 12 and 18 before calculation. GCD(12, 18) = 6.

---

### Example 10: Large Numbers
```
=GCD(123456, 789012)
```
**Result:** 12

**Explanation:** Euclid's algorithm handles large numbers efficiently. Both are divisible by 12.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure all arguments are numbers or numeric cell references. |
| `#NUM!` | Negative number after truncation | GCD requires non-negative integers. Use ABS() if working with potentially negative values. |
| `#NAME?` | Misspelled function name | Verify spelling: GCD, not GCF or HCF (though these are synonyms mathematically). |
| `Result of 1` | Numbers are coprime | Correct behavior—1 is the only common divisor for coprime numbers like 8 and 15. |
| `Result of 0` | One or more arguments is 0 | If all arguments are 0, GCD is 0. If any argument is 0, see Example 7. |

## Use Cases

### [[Fraction Simplification and Ratio Reduction]]

**Scenario:** Reduce fractions to lowest terms or simplify ratios.

**Implementation:**
```
=Numerator / GCD(Numerator, Denominator)           → Simplified numerator
=Denominator / GCD(Numerator, Denominator)         → Simplified denominator
=GCD(A1:A3) / A1 & ":" & GCD(A1:A3) / A2          → Simplified ratio
```

**Business Application:** Financial reporting, recipe scaling, unit conversions, display of proportions.

**Technical Details:** A fraction is in lowest terms when GCD(numerator, denominator) = 1. Divide both by their GCD to simplify. This is the standard algorithm for fraction reduction.

---

### [[Synchronization and Timing Problems]]

**Scenario:** Find when periodic events align or common intervals in schedules.

**Implementation:**
```
=LCM(CycleA, CycleB)                               → When cycles align
=GCD(IntervalA, IntervalB)                         → Common sub-interval
=GCD(AllFrequencies)                               → Fundamental frequency
```

**Business Application:** Scheduling, manufacturing batch timing, signal processing, astronomical calculations (planet alignments).

**Technical Details:** GCD finds the largest common period. LCM finds when cycles synchronize. Musical harmonics relate through GCD/LCM of frequencies.

---

### [[Data Validation and Pattern Detection]]

**Scenario:** Verify divisibility patterns or find common structure in data.

**Implementation:**
```
=IF(GCD(Value, 100)=Value, "Divides 100", "No")
=GCD(DataRange)=1                                  → Check if pairwise coprime
=Value / GCD(Value, StandardUnit)                  → Reduce to standard units
```

**Business Application:** Quality control, data cleaning, detecting measurement precision patterns.

**Technical Details:** If GCD of a dataset equals 1, values share no common factor (besides 1). A GCD > 1 suggests values might be multiples of a common unit.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2000+ (originally Analysis ToolPak), built-in since Excel 2007
- **Arguments:** Up to 255 numbers/ranges
- **Decimal handling:** Truncated to integers
- **Negative handling:** Treated as positive (absolute value)

### Google Sheets
- **Availability:** All versions
- **Arguments:** Similar limit
- **Decimal handling:** Same truncation
- **Negative handling:** Same behavior

### Both Platforms
- Identical results for same inputs
- Same GCD(n, 0) = n convention
- Same truncation of decimals
- Same error conditions

## Tips and Best Practices

1. **GCD for fraction simplification:** Divide both numerator and denominator by GCD(num, denom) to get lowest terms.

2. **GCD(a,b) * LCM(a,b) = a*b:** Use this identity to calculate LCM if you know GCD, or vice versa: LCM = (a*b)/GCD(a,b).

3. **GCD of 1 means coprime:** When GCD returns 1, the numbers share no common factors. Important in cryptography and number theory.

4. **Decimals are truncated:** GCD(6.9, 9.1) becomes GCD(6, 9) = 3. Round explicitly if needed before calling GCD.

5. **Works with ranges:** GCD(A1:A100) finds the GCD of all 100 values efficiently. Useful for analyzing data patterns.

6. **Handle negatives with ABS:** If your data might be negative, use GCD(ABS(A1), ABS(B1)) for safety.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[LCM]] | Least common multiple | When you need smallest common multiple instead of largest common divisor |
| [[MOD]] | Remainder after division | For checking divisibility or cycling through values |
| [[QUOTIENT]] | Integer division | For dividing and getting whole number result |

### Commonly Used Together

**[[LCM]]** - Partner function

*Related calculations:*
```
=LCM(A1, B1)                     → Least common multiple
=A1 * B1 / GCD(A1, B1)           → Same as LCM (identity)
```
GCD and LCM are complementary operations.

---

**[[MOD]]** - Divisibility check

*Verify divisibility:*
```
=MOD(LargeNumber, GCD(A1,B1))=0  → True if GCD divides it
```
MOD returns 0 when division is exact.

---

**[[ABS]]** - Handle negatives

*Safe GCD for any integers:*
```
=GCD(ABS(A1), ABS(B1))
```
Convert negatives to positives before GCD.

---

**[[INT]]/[[TRUNC]]** - Explicit truncation

*Control rounding:*
```
=GCD(TRUNC(A1), TRUNC(B1))
```
Make truncation explicit if decimals matter.

---

**[[IF]]** - Coprimality check

*Test for no common factors:*
```
=IF(GCD(A1,B1)=1, "Coprime", "Share factor " & GCD(A1,B1))
```
GCD of 1 indicates coprime numbers.

## Official Documentation

- **Microsoft Excel:** [GCD function](https://support.microsoft.com/en-us/office/gcd-function-d5107a51-69e3-461f-8e4c-ddfc21b5073a)
- **Google Sheets:** [GCD function](https://support.google.com/docs/answer/3093489)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Originally in Analysis ToolPak |
| Excel 2007+ | Built-in | No longer requires add-in |
| Google Sheets | Original launch | Always built-in |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
