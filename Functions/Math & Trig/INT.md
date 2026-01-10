---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- rounding
- floor-function
- integer
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Integer Function
- Floor Function
- Round Down
tags:
- integer
- floor
- round-down
- truncate
- whole-number
---

# INT

## Description

**INT** rounds a number down to the nearest integer—always toward negative infinity. INT(3.7) returns 3. INT(-3.7) returns -4 (not -3). This "floor" behavior means INT always goes down on the number line, regardless of whether the number is positive or negative. It's the mathematical floor function implemented in spreadsheets.

This function is essential for **extracting whole number portions, date extraction from datetime values, and any calculation requiring the largest integer less than or equal to a value**. INT strips the decimal portion in a consistent directional way—always rounding toward negative infinity. This makes it predictable for mathematical operations even when dealing with negative numbers.

**INT vs TRUNC: The critical difference for negative numbers.** For positive numbers, INT and TRUNC behave identically—INT(3.7) = TRUNC(3.7) = 3. But for negative numbers, they diverge dramatically. INT(-3.7) = -4 (rounds toward negative infinity), while TRUNC(-3.7) = -3 (truncates toward zero). Think of INT as "floor" (always down on the number line) and TRUNC as "chop off decimals" (always toward zero). Choose based on your specific need.

**A powerful use case: extracting the date portion from a datetime value.** Excel stores datetimes as numbers where the integer part is the date and the decimal part is the time. INT(datetime) extracts just the date, discarding the time portion. This works perfectly because datetimes are always positive (in modern dates), so INT's floor behavior simply removes the time fraction.

## Syntax

> [!f(x)] INT Syntax
>
> ```
> =INT(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The numeric value to round down to an integer. Can be positive or negative. If positive, returns the largest integer less than or equal to the value. If negative, returns the smallest integer less than or equal to the value (further from zero). |

### Return Value

Returns an integer representing the floor of the input—the largest integer that is less than or equal to the input value. For positive numbers, this truncates the decimal part. For negative numbers, this rounds away from zero (toward negative infinity).

## Examples

> [!f(x)] INT Examples

### Example 1: Positive Decimal
```
=INT(3.7)
```
**Result:** 3

**Explanation:** The integer portion of 3.7 is 3. INT removes the decimal, leaving the whole number. Same as TRUNC for positive numbers.

---

### Example 2: Negative Decimal - KEY DIFFERENCE
```
=INT(-3.7)
```
**Result:** -4

**Explanation:** INT rounds toward negative infinity. -4 is the largest integer less than or equal to -3.7. This is where INT differs from TRUNC, which would give -3.

---

### Example 3: Already an Integer
```
=INT(5)
```
**Result:** 5

**Explanation:** When the input is already an integer, INT returns it unchanged. No rounding needed.

---

### Example 4: Positive Number Less Than 1
```
=INT(0.999)
```
**Result:** 0

**Explanation:** Any positive number less than 1 has 0 as its floor. INT(0.001) = INT(0.5) = INT(0.999) = 0.

---

### Example 5: Negative Number Greater Than -1
```
=INT(-0.5)
```
**Result:** -1

**Explanation:** The floor of any number between -1 and 0 (exclusive) is -1. INT rounds toward negative infinity, so -0.5 goes to -1.

---

### Example 6: Extract Date from DateTime
```
=INT(DateTime)
```
**Result:** Date serial number (integer portion)

**Explanation:** DateTime 45000.75 (representing a date with time 6:00 PM) returns 45000 (just the date). This is the classic method to strip time from datetime values.

---

### Example 7: Calculate Complete Hours
```
=INT(TotalMinutes / 60)
```
**Result:** Whole hours

**Explanation:** 185 minutes / 60 = 3.083... hours. INT gives 3 complete hours. Use MOD(185, 60) for the remaining 5 minutes.

---

### Example 8: Floor Division
```
=INT(A1 / B1)
```
**Result:** Integer quotient (floor division)

**Explanation:** For positive numbers, equivalent to QUOTIENT. For negative results, INT gives the floor (toward negative infinity), while QUOTIENT truncates toward zero.

---

### Example 9: Compare INT vs TRUNC for Negative
```
=INT(-2.3)  vs  =TRUNC(-2.3)
```
**Result:** INT = -3, TRUNC = -2

**Explanation:** This is the critical distinction. INT floors (toward -infinity), TRUNC truncates (toward zero). INT(-2.3) = -3 because -3 is the floor. TRUNC(-2.3) = -2 because decimals are simply removed.

---

### Example 10: Age Calculation
```
=INT((TODAY() - BirthDate) / 365.25)
```
**Result:** Approximate age in years

**Explanation:** Divides days lived by average days per year, then INT gives completed years. Note: DATEDIF is more accurate for exact age.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text values cause errors. |
| `#NAME?` | Misspelled function name | Verify spelling: INT, not INTEGER or INTEG. |
| `Unexpected negative result` | Negative numbers floor toward -infinity | INT(-0.5) = -1, not 0. Use TRUNC if you want truncation toward zero. |
| `Wrong for negative division` | INT floors negative results | INT(-10/3) = -4, not -3. Use QUOTIENT or TRUNC(A/B) for truncation. |
| `Date still has time` | Cell formatting | INT strips time, but if cell format shows time, it might look unchanged. Format cell as Date only. |

## Use Cases

### [[Date Extraction from DateTime Values]]

**Scenario:** Extract just the date portion from datetime values for grouping, comparison, or display.

**Implementation:**
```
=INT(DateTimeValue)                          → Date only
=DateTimeValue - INT(DateTimeValue)          → Time only (as fraction)
=INT(A1) = INT(B1)                           → Compare dates ignoring time
```

**Business Application:** Transaction analysis, appointment scheduling, report date grouping. Any scenario where you need to work with dates independently of times.

**Technical Details:** Excel dates are integers; times are decimals 0 to 0.999. DateTime 45000.75 = date 45000 + time 0.75 (18:00). INT extracts the date cleanly because dates are positive.

---

### [[Floor Division and Grouping]]

**Scenario:** Divide items into groups, calculate how many complete units fit, or implement floor division.

**Implementation:**
```
=INT(Amount / 100) * 100                     → Round down to nearest 100
=INT(Score / 10)                             → Group by decade (0-9, 10-19, etc.)
=INT((Value - Min) / BinWidth)               → Histogram bin assignment
```

**Business Application:** Bucketing for analysis, tiered pricing, binning for histograms, chunk processing. Any grouping by numeric ranges.

**Technical Details:** INT(value/size)*size rounds down to the nearest multiple of size. This is "floor to multiple" behavior. For ceiling, use -INT(-value/size)*size or CEILING function.

---

### [[Time Calculations and Decomposition]]

**Scenario:** Break down time values into hours, minutes, seconds or calculate elapsed complete units.

**Implementation:**
```
=INT(TotalSeconds / 3600)                    → Complete hours
=INT(MOD(TotalSeconds, 3600) / 60)           → Complete minutes after hours
=INT((EndTime - StartTime) * 24)             → Elapsed complete hours
```

**Business Application:** Time tracking, payroll calculations, duration displays, scheduling. Converting between time formats.

**Technical Details:** Excel times are fractions of a day (0.5 = noon). Multiply by 24 for hours, by 1440 for minutes, by 86400 for seconds. INT extracts completed units.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Behavior:** Floors toward negative infinity (true floor function)
- **DateTime:** Works perfectly for date extraction from datetime
- **Precision:** 15 significant digits like all Excel numbers

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel—floors toward negative infinity
- **DateTime:** Same date extraction technique works
- **Precision:** Same as Excel

### Both Platforms
- Identical behavior for positive and negative numbers
- Same floor-toward-negative-infinity semantics
- Same #VALUE! error for non-numeric input
- Consistent for date/time operations

### Key Distinction from TRUNC

| Input | INT Result | TRUNC Result | Explanation |
|-------|-----------|--------------|-------------|
| 3.7 | 3 | 3 | Same for positive |
| 3.2 | 3 | 3 | Same for positive |
| -3.2 | -4 | -3 | INT floors, TRUNC truncates |
| -3.7 | -4 | -3 | INT floors, TRUNC truncates |
| -0.5 | -1 | 0 | Critical difference near zero |

## Tips and Best Practices

1. **Know when INT differs from TRUNC:** For positive numbers, they're identical. For negative numbers, INT floors (toward -infinity) while TRUNC truncates (toward zero). INT(-2.5) = -3, TRUNC(-2.5) = -2.

2. **Use INT for date extraction:** INT(datetime) is the classic way to strip time from a datetime value. Works because dates are positive integers plus time fractions.

3. **Floor to multiples:** INT(value/multiple)*multiple rounds down to the nearest multiple. INT(157/10)*10 = 150. Useful for bucketing and grouping.

4. **Combine with MOD for decomposition:** INT gives the quotient (complete units), MOD gives the remainder. Together they fully decompose division: n = d*INT(n/d) + MOD(n, d).

5. **Consider TRUNC for pure truncation:** If you want to simply remove decimals regardless of sign (toward zero), TRUNC is clearer for that intent. Reserve INT for true floor behavior.

6. **Watch negative results in division:** INT(-10/3) = -4, not -3. If you expect -3, use TRUNC(-10/3) or QUOTIENT(-10, 3) instead.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TRUNC]] | Truncates toward zero | When you want to remove decimals regardless of sign |
| [[FLOOR]] | Rounds down to multiple | When rounding to arbitrary multiples, not just integers |
| [[ROUNDDOWN]] | Rounds toward zero | Similar to TRUNC with decimal place control |
| [[QUOTIENT]] | Integer division toward zero | For integer division with truncation semantics |

### Commonly Used Together

**[[MOD]]** - Remainder after integer division

*Complete decomposition:*
```
Hours: =INT(Minutes / 60)
Remaining: =MOD(Minutes, 60)
```
INT gives quotient, MOD gives remainder.

---

**[[TRUNC]]** - Alternative truncation

*Compare behaviors:*
```
=INT(-3.7)   → -4 (floor)
=TRUNC(-3.7) → -3 (truncate)
```
Choose based on desired behavior for negative numbers.

---

**[[FLOOR]]** - Floor to multiple

*Round to nearest multiple:*
```
=FLOOR(A1, 0.25)  → Floor to nearest quarter
=INT(A1 / 0.25) * 0.25  → Equivalent formula
```
FLOOR is more direct for non-integer multiples.

---

**[[TEXT]]** - Format integer result

*Display formatted:*
```
=TEXT(INT(Hours), "0") & "h " & TEXT(MOD(Minutes, 60), "00") & "m"
```
Format INT results for display.

---

**[[IF]]** - Conditional on integer portion

*Act on whole number:*
```
=IF(INT(Value) >= Threshold, "Pass", "Fail")
```
Test based on integer portion only.

## Official Documentation

- **Microsoft Excel:** [INT function](https://support.microsoft.com/en-us/office/int-function-a6c4af9e-356d-4369-ab6a-cb1fd9d343ef)
- **Google Sheets:** [INT function](https://support.google.com/docs/answer/3093490)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
