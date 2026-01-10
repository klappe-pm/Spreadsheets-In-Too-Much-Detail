---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- sign-function
- polarity
- direction
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sign Function
- Signum
- Number Sign
tags:
- sign
- polarity
- positive
- negative
- direction
---

# SIGN

## Description

**SIGN** returns the sign of a number: 1 for positive numbers, -1 for negative numbers, and 0 for zero. It's the signum function from mathematics, reducing any number to just its directional component. SIGN(42) = 1, SIGN(-17.5) = -1, SIGN(0) = 0. Three possible outputs, infinite possible inputs.

This function is fundamental for **direction detection, polarity handling, and decomposing numbers into magnitude and sign**. Combined with ABS, you can split any number: `number = ABS(number) * SIGN(number)`. This decomposition is useful when you need to process magnitude and direction separately, then recombine them. SIGN extracts the directional essence of a value.

**SIGN shines in conditional logic simplification.** Instead of complex IF statements testing for positive, negative, and zero, SIGN provides a clean numeric indicator. Use SIGN(A1)*SomeValue to flip direction, SIGN(A1-B1) to determine which is larger, or (SIGN(A1)+1)/2 to convert to a 0/1 binary indicator for non-negative values.

The function is also valuable for **trend detection and change analysis**. SIGN(Today - Yesterday) tells you if a metric increased (1), decreased (-1), or stayed the same (0). Summing SIGN values across a series gives a simple "net direction" score—more positives than negatives means overall upward movement.

## Syntax

> [!f(x)] SIGN Syntax
>
> ```
> =SIGN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The numeric value whose sign you want to determine. Can be a literal number, cell reference, or formula result. Accepts any real number including decimals. |

### Return Value

Returns exactly one of three values:
- **1** if the number is positive (greater than zero)
- **-1** if the number is negative (less than zero)
- **0** if the number is exactly zero

## Examples

> [!f(x)] SIGN Examples

### Example 1: Positive Number
```
=SIGN(42)
```
**Result:** 1

**Explanation:** Any positive number returns 1. The magnitude doesn't matter—SIGN(0.001) and SIGN(999999) both return 1.

---

### Example 2: Negative Number
```
=SIGN(-17.5)
```
**Result:** -1

**Explanation:** Any negative number returns -1. SIGN(-0.0001) and SIGN(-999999) both return -1.

---

### Example 3: Zero
```
=SIGN(0)
```
**Result:** 0

**Explanation:** Zero is the only input that returns 0. Zero has no sign—it's neither positive nor negative.

---

### Example 4: Sign of a Difference
```
=SIGN(A1 - B1)
```
**Result:** 1 if A1 > B1, -1 if A1 < B1, 0 if equal

**Explanation:** Determines which value is larger. Returns 1 when A1 exceeds B1, -1 when B1 exceeds A1, and 0 when they're equal.

---

### Example 5: Decompose Number
```
=ABS(A1) * SIGN(A1)
```
**Result:** Original value of A1

**Explanation:** This identity shows how magnitude (ABS) and sign (SIGN) combine to form a number. Useful when processing these components separately then recombining.

---

### Example 6: Apply Sign of One Number to Another
```
=ABS(A1) * SIGN(B1)
```
**Result:** Magnitude of A1 with sign of B1

**Explanation:** Takes the size from A1 but the direction from B1. If A1=10 and B1=-5, result is -10.

---

### Example 7: Direction Indicator
```
=IF(SIGN(Change)=1, "Up", IF(SIGN(Change)=-1, "Down", "Flat"))
```
**Result:** Text describing direction

**Explanation:** While you could test Change directly, SIGN makes the three-way comparison explicit and readable.

---

### Example 8: Count Positive vs Negative
```
=SUMPRODUCT(SIGN(A1:A10))
```
**Result:** Net direction score

**Explanation:** If the range has 6 positive and 4 negative values, result is 6*1 + 4*(-1) = 2. Positive result means more positives than negatives.

---

### Example 9: Convert to Binary (Non-negative = 1)
```
=(SIGN(A1) + 1) / 2
```
**Result:** 1 for non-negative, 0.5 for zero, 0 for negative

**Explanation:** Maps the -1/0/1 range to 0/0.5/1. For strictly positive=1, negative=0, use MAX(SIGN(A1), 0).

---

### Example 10: Flip a Value's Direction
```
=A1 * SIGN(B1) * SIGN(A1)
```
**Result:** A1 with direction relative to B1

**Explanation:** When B1 is negative, this flips the sign of A1. When B1 is positive, A1 stays the same. Zero B1 zeros the result.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text values cause errors. |
| `#NAME?` | Misspelled function name | Verify spelling: SIGN, not SGN or SIGNUM. |
| `Unexpected 0` | Input is zero | SIGN(0) = 0 is correct. Check if zero input is expected. |
| `Unexpected 1 for negative` | Confusion with output meaning | SIGN returns -1 for negative, not 1. The output is the multiplier to apply to ABS(x) to get x. |
| `Formula error in array context` | Text mixed with numbers in range | SIGN applied to a range with text causes errors. Filter non-numeric values first. |

## Use Cases

### [[Trend Detection and Direction Analysis]]

**Scenario:** Determine whether metrics are increasing, decreasing, or stable.

**Implementation:**
```
=SIGN(CurrentValue - PreviousValue)         → 1=up, -1=down, 0=same
=SUMPRODUCT(SIGN(B2:B100 - B1:B99))          → Net direction over series
=COUNTIF(SIGN(Changes), 1)                   → Count of positive changes
```

**Business Application:** Stock price movement, sales trends, performance metrics, temperature changes. Any time series where direction matters more than magnitude.

**Technical Details:** SIGN reduces noisy data to pure directional signals. A stock that rises $5 and falls $2 has SIGN sequence {1, -1}, with sum = 0 for net neutral direction, even though the net change is +$3.

---

### [[Polarity-Aware Calculations]]

**Scenario:** Perform calculations that must respect the sign/direction of values.

**Implementation:**
```
=ABS(NewValue - OldValue) * SIGN(Expected)    → Change magnitude with expected direction
=SUMPRODUCT(ABS(Values), SIGN(Weights))       → Weighted sum respecting weight signs
=SIGN(A1) * CEILING(ABS(A1), 10)              → Round away from zero to nearest 10
```

**Business Application:** Financial calculations with directional requirements, engineering with force/direction separation, statistical adjustments with sign preservation.

**Technical Details:** Separating magnitude from sign allows independent processing. Calculate size, then apply direction. This is cleaner than conditional formulas handling positive and negative cases separately.

---

### [[Conditional Logic Simplification]]

**Scenario:** Replace complex IF statements with SIGN-based calculations.

**Implementation:**
```
=CHOOSE(SIGN(Value)+2, "Negative", "Zero", "Positive")
=ROW() + SIGN(A1) * 10                         → Row +10, same, or -10
=SIGN(A1-B1) * ABS(C1)                         → C1 magnitude with comparison direction
```

**Business Application:** Scorecards, rating systems, comparison indicators, formatting rules. Any three-way classification based on numeric sign.

**Technical Details:** SIGN maps to -1/0/1, which CHOOSE can use with offset (+2 gives 1/2/3 index). This avoids nested IFs and makes the three-case logic explicit.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Array support:** Works element-wise in array contexts and with dynamic arrays
- **Performance:** Extremely fast, negligible computation time
- **Complex numbers:** Use IMSIGN for complex number sign (not standard SIGN)

### Google Sheets
- **Availability:** All versions
- **Array support:** Works in ARRAYFORMULA context
- **Performance:** Equivalent to Excel
- **Complex numbers:** No native complex number support

### Both Platforms
- Identical behavior for all real numbers
- Same three return values (-1, 0, 1)
- Same #VALUE! error for non-numeric input
- No platform-specific quirks

## Tips and Best Practices

1. **Remember the three outputs:** SIGN only returns -1, 0, or 1. It's not a continuous function—it's a classifier that puts every number into one of three buckets.

2. **Use with ABS for decomposition:** `number = ABS(number) * SIGN(number)` is always true. This identity is useful when you need to process magnitude and direction separately.

3. **SIGN of a difference compares values:** SIGN(A-B) returns 1 if A>B, -1 if A<B, 0 if A=B. Cleaner than nested IFs for three-way comparison.

4. **Sum SIGN values for net direction:** SUMPRODUCT(SIGN(range)) tells you the balance of positive vs negative values. Positive sum means more positives overall.

5. **Map to other ranges:** Transform -1/0/1 to any three values: `=CHOOSE(SIGN(x)+2, "Low", "Medium", "High")` or use arithmetic: `(SIGN(x)+1)/2` maps to 0/0.5/1.

6. **Combine with conditional aggregation:** `=SUMPRODUCT((SIGN(Values)=1)*Values)` sums only positive values. More readable than SUMIF for some use cases.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ABS]] | Absolute value (magnitude) | When you need the size without direction |
| [[IF]] | Conditional logic | When you need more than three outcomes or non-numeric results |
| [[IMSIGN]] | Sign of complex number | For complex number operations in Excel |

### Commonly Used Together

**[[ABS]]** - Magnitude and sign decomposition

*Separate then recombine:*
```
=ABS(A1) * SIGN(B1)    → Size of A1, direction of B1
```
Apply one number's magnitude with another's direction.

---

**[[CHOOSE]]** - Map sign to values

*Three-way text output:*
```
=CHOOSE(SIGN(A1)+2, "Decrease", "No Change", "Increase")
```
Converts -1/0/1 to custom values (offset by 2 for valid index).

---

**[[SUMPRODUCT]]** - Aggregate sign analysis

*Net direction of a series:*
```
=SUMPRODUCT(SIGN(A2:A100 - A1:A99))
```
Counts ups minus downs across consecutive pairs.

---

**[[IF]]** - Conditional based on sign

*Direction-aware action:*
```
=IF(SIGN(Balance)<0, "Overdrawn", "OK")
```
Though SIGN=(-1) works, IF(Balance<0...) is often clearer for simple cases.

---

**[[CEILING]]/[[FLOOR]]** - Round away from/toward zero

*Directional rounding:*
```
=SIGN(A1) * CEILING(ABS(A1), Step)
```
Round away from zero to the nearest Step.

## Official Documentation

- **Microsoft Excel:** [SIGN function](https://support.microsoft.com/en-us/office/sign-function-109c932d-fcdc-4023-91f1-2dd0e916a1d8)
- **Google Sheets:** [SIGN function](https://support.google.com/docs/answer/3093145)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
