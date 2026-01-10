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
- floor
- multiples
- round-down
- truncation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Round Down to Multiple
- Floor Round
tags:
- rounding
- floor
- multiples
- round-down
- toward-zero
---

# FLOOR

## Description

**FLOOR** rounds a number DOWN to the nearest multiple of a specified significance. Unlike ROUND which goes to the nearest value, FLOOR always rounds toward zero for positive numbers. FLOOR(4.8, 1) returns 4, FLOOR(4.8, 0.5) returns 4.5, FLOOR(27, 5) returns 25. The function is essential for scenarios where you need conservative, truncated values that never exceed the original.

**The "significance" parameter controls the increment:** FLOOR doesn't just truncate to integers—it rounds down to the nearest multiple of whatever significance you specify. FLOOR(27, 10) gives 20 (previous multiple of 10). FLOOR(0.67, 0.25) gives 0.50 (previous quarter). FLOOR(time, TIME(0,15,0)) rounds time down to the previous 15-minute mark. This makes FLOOR ideal for calculating completed intervals.

**Negative number behavior differs by platform:** Like CEILING, FLOOR has platform-specific behavior for negative numbers. In Excel (traditional FLOOR), negative numbers round toward zero: FLOOR(-4.2, -1) = -4 (not -5). Google Sheets' FLOOR rounds away from zero (more negative): FLOOR(-4.2, 1) = -5. Excel 2010+ added FLOOR.MATH and FLOOR.PRECISE with different behaviors. Always test negative cases.

**FLOOR vs CEILING vs INT:** FLOOR rounds down to a multiple. CEILING rounds up to a multiple. INT simply truncates to an integer (no multiple choice). For age calculations, use FLOOR with significance 1 (or INT)—you're 29 until you complete your 30th year. For shipping quotes that round up, use CEILING. For nearest-neighbor rounding, use MROUND.

## Syntax

> [!f(x)] FLOOR Syntax
>
> ```
> =FLOOR(number, significance)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value you want to round down. Can be positive, negative, or zero. |
| `significance` | Yes | The multiple to which you want to round. In Excel, must have the same sign as number (or errors occur). In Google Sheets, the sign doesn't matter for positive numbers. |

### Return Value

Returns the largest multiple of 'significance' that is less than or equal to 'number' (for positive numbers). Returns #NUM! if number and significance have different signs in Excel. Returns #VALUE! if either argument is non-numeric. Returns 0 if significance is 0.

## Examples

> [!f(x)] FLOOR Examples

### Example 1: Round Down to Integer
```
=FLOOR(4.8, 1)
```
**Result:** 4

**Explanation:** 4.8 rounds down to the previous multiple of 1, which is 4. This is equivalent to TRUNC or INT for positive numbers.

---

### Example 2: Round Down to Previous 5
```
=FLOOR(27, 5)
```
**Result:** 25

**Explanation:** 27 rounds down to the previous multiple of 5. The multiples are 25, 30, 35... so 27 goes to 25.

---

### Example 3: Round Down to Previous 10
```
=FLOOR(247, 10)
```
**Result:** 240

**Explanation:** 247 rounds down to the previous multiple of 10 (240). Useful for conservative estimates.

---

### Example 4: Round Down to Previous 0.25 (Quarter)
```
=FLOOR(3.67, 0.25)
```
**Result:** 3.50

**Explanation:** 3.67 is between 3.50 and 3.75. FLOOR goes down to 3.50 (the previous quarter).

---

### Example 5: Round Down to Previous 0.05 (Nickel)
```
=FLOOR(3.47, 0.05)
```
**Result:** 3.45

**Explanation:** 3.47 is between 3.45 and 3.50. FLOOR rounds down to 3.45.

---

### Example 6: Already at a Multiple
```
=FLOOR(25, 5)
```
**Result:** 25

**Explanation:** 25 is already a multiple of 5. FLOOR returns it unchanged—no rounding needed.

---

### Example 7: Round Time to Previous 15 Minutes
```
=FLOOR(A1, TIME(0,15,0))
```
**Result:** Time rounded down to previous 15-minute mark

**Explanation:** If A1 is 9:37 AM, result is 9:30 AM. TIME(0,15,0) represents 15 minutes as a time value.

---

### Example 8: Age Calculation
```
=FLOOR((TODAY() - BirthDate) / 365.25, 1)
```
**Result:** Complete years of age

**Explanation:** You don't turn 30 until you've completed 30 years. FLOOR ensures only complete years count.

---

### Example 9: Negative Number (Excel Behavior)
```
=FLOOR(-4.8, -1)
```
**Result:** -4 (Excel)

**Explanation:** In Excel, both arguments must have the same sign. FLOOR(-4.8, -1) rounds toward zero (less negative), giving -4.

---

### Example 10: Large Multiple
```
=FLOOR(1234, 100)
```
**Result:** 1200

**Explanation:** 1234 rounds down to the previous multiple of 100. Good for conservative budget figures.

---

### Example 11: Small Fractional Significance
```
=FLOOR(1.999, 0.1)
```
**Result:** 1.9

**Explanation:** 1.999 rounds down to 1.9 (the previous tenth). Note: not 2.0.

---

### Example 12: Zero Input
```
=FLOOR(0, 5)
```
**Result:** 0

**Explanation:** Zero is a multiple of every number. FLOOR returns 0.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | In Excel, number and significance have different signs | Make both positive or both negative. Use FLOOR.MATH for more control. |
| `#VALUE!` | Non-numeric input | Ensure both arguments are numbers. |
| `#NAME?` | Misspelled function name | Check spelling: FLOOR, not FLOR or FLORR. |
| `#DIV/0!` | Significance is 0 | Use a non-zero significance value. |
| `Unexpected negative result` | Misunderstanding negative behavior | Excel FLOOR rounds negative numbers toward zero. Use FLOOR.MATH with mode argument for different behavior. |
| `Result same as input` | Already at multiple | Not an error—input was already at a multiple of significance. |

## Use Cases

### [[Age and Duration Calculations]]

**Scenario:** Calculate completed intervals (age, tenure, elapsed time).

**Implementation:**
```
=FLOOR((TODAY() - BirthDate) / 365.25, 1)     → Complete years of age
=FLOOR(YEARFRAC(StartDate, TODAY()), 1)       → Years of tenure
=FLOOR(ElapsedMinutes / 60, 1)                 → Complete hours
=FLOOR(DaysEmployed / 30, 1)                   → Complete months (approx)
```

**Business Application:** Age verification, service anniversaries, tenure-based benefits, probation period tracking.

**Technical Details:** When counting "completed" intervals, you need FLOOR behavior—someone isn't 30 until they've completed 30 full years. FLOOR ensures you only count complete intervals.

---

### [[Pricing and Financial Truncation]]

**Scenario:** Round prices down for customer-favorable calculations.

**Implementation:**
```
=FLOOR(Price, 0.01)                            → Truncate to cents
=FLOOR(Discount * OriginalPrice, 0.05)         → Round discount to nickel
=FLOOR(CashBack, 1)                            → Whole dollar cash back only
=FLOOR(InterestEarned, 0.01)                   → Don't round up interest owed
```

**Business Application:** Customer-favorable rounding, interest calculations, refund amounts, loyalty rewards.

**Technical Details:** Some regulations require rounding in the customer's favor. FLOOR ensures you never charge more than justified or pay out more interest than earned.

---

### [[Time Tracking and Intervals]]

**Scenario:** Calculate completed time intervals for scheduling and billing.

**Implementation:**
```
=FLOOR(ActualTime, TIME(0,15,0))              → Previous 15-minute mark
=FLOOR(Duration, TIME(0,30,0))                 → Complete half-hours
=FLOOR(WorkHours, 1)                           → Complete hours only
```

**Business Application:** Time card validation (don't round up employee hours), interval-based scheduling, period completion tracking.

**Technical Details:** When you need to know how many complete intervals have passed, FLOOR gives the answer. A 47-minute meeting has only completed the :30 interval, not the :45.

## Platform Differences

### Microsoft Excel
- **Basic FLOOR:** Requires same sign for number and significance; rounds toward zero for negatives
- **FLOOR.MATH (Excel 2013+):** More control with optional mode parameter for negative handling
- **FLOOR.PRECISE (Excel 2010+):** Always rounds toward negative infinity
- **Sign restriction:** FLOOR(negative, positive) returns #NUM! error

### Google Sheets
- **Availability:** All versions
- **Sign handling:** More flexible—FLOOR(-4.8, 1) works and returns -5 (rounds away from zero)
- **Behavior:** Generally rounds toward negative infinity for all numbers
- **No FLOOR.MATH:** Google Sheets doesn't have this variant

### Key Platform Difference

**For positive numbers:** Both platforms behave identically—round down to the previous multiple.

**For negative numbers:**
- Excel requires same sign: `=FLOOR(-4.8, -1)` returns -4 (toward zero)
- Google Sheets allows positive significance: `=FLOOR(-4.8, 1)` returns -5 (away from zero)
- For Excel's "always toward negative infinity" behavior, use FLOOR.PRECISE or FLOOR.MATH with mode = -1

## Tips and Best Practices

1. **FLOOR always rounds down:** For positive numbers, this means smaller. For negatives in standard Excel FLOOR, this means toward zero (less negative).

2. **Use FLOOR.MATH in Excel for control:** The mode parameter lets you choose how negative numbers round. Mode = 0 or omitted rounds toward zero; Mode = -1 rounds away from zero.

3. **Watch the sign restriction in Excel:** FLOOR(negative, positive) errors. Either make both negative or use FLOOR.MATH/FLOOR.PRECISE.

4. **For "always toward negative infinity":** In Excel, use FLOOR.PRECISE. This rounds -4.2 to -5, and 4.8 to 4.

5. **Common significance values:** 1 (integers), 5, 10, 0.25 (quarters), 0.05 (nickels), TIME(0,15,0) (15 minutes).

6. **If already at multiple, no change:** FLOOR(10, 5) = 10. The function returns the input when it's already at a multiple.

7. **Pair with CEILING for range buckets:** FLOOR gives lower bounds, CEILING gives upper bounds. Together they define ranges.

8. **For nearest (not always down), use MROUND:** FLOOR always goes down. If you want the closest multiple, MROUND is the right function.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CEILING]] | Rounds UP to multiple | When you always need to round up |
| [[MROUND]] | Rounds to NEAREST multiple | When you want the closest value, not always down |
| [[ROUNDDOWN]] | Rounds down by decimal places | When thinking in decimal places, not multiples |
| [[INT]] | Truncates to integer | For simple integer truncation (significance 1) |
| [[TRUNC]] | Truncates to decimal places | For decimal truncation without multiple |
| [[FLOOR.MATH]] | FLOOR with mode control | When you need control over negative number handling |

### Commonly Used Together

**[[CEILING]]** - Complementary function

*Compare behaviors:*
```
=FLOOR(27, 5)    → 25 (down)
=CEILING(27, 5)  → 30 (up)
=MROUND(27, 5)   → 25 (nearest)
```
Three options for rounding to 5.

---

**[[TIME]]** - For time-based multiples

*Time interval rounding:*
```
=FLOOR(A1, TIME(0, 15, 0))
```
Round time down to previous 15 minutes.

---

**[[INT]]** - Simpler integer version

*Compare behaviors:*
```
=FLOOR(4.8, 1)   → 4 (explicit multiple)
=INT(4.8)        → 4 (implicit multiple of 1)
```
INT is shorthand for FLOOR with significance 1.

---

**[[IF]]** - Conditional rounding

*Apply FLOOR conditionally:*
```
=IF(A1 > 100, FLOOR(A1, 10), A1)
```
Round down only for values over 100.

---

**[[MOD]]** - Understand the rounding

*See the remainder:*
```
=MOD(27, 5)       → 2 (the amount FLOOR removed)
=27 - MOD(27, 5)  → 25 (same as FLOOR)
```
MOD shows what FLOOR truncates.

## Official Documentation

- **Microsoft Excel:** [FLOOR function](https://support.microsoft.com/en-us/office/floor-function-14bb497c-24f2-4e04-b327-b0b4de5a8886)
- **Google Sheets:** [FLOOR function](https://support.google.com/docs/answer/3093487)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Basic FLOOR function |
| Excel 2010 | FLOOR.PRECISE | Always rounds toward negative infinity |
| Excel 2013 | FLOOR.MATH | Added mode parameter for control |
| Google Sheets | Original launch | Rounds toward negative infinity (different from Excel) |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
