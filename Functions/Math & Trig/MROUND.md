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
- mround
- multiples
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Round to Multiple
- Multiple Round
tags:
- rounding
- mround
- multiples
- nearest
---

# MROUND

## Description

**MROUND** rounds a number to the **nearest** multiple of a specified value. Unlike CEILING (always up) or FLOOR (always down), MROUND goes to whichever multiple is closest. MROUND(14, 5) returns 15 (nearer than 10), MROUND(12, 5) returns 10 (nearer than 15), and MROUND(12.5, 5) returns 15 (rounds up at the midpoint).

**Standard rounding behavior:** MROUND uses conventional "round half up" behavior. When a number is exactly halfway between two multiples, it rounds UP (away from zero). MROUND(7.5, 5) returns 10, not 5. This is consistent with ROUND and most people's expectations of rounding.

**Flexible multiple parameter:** You can round to any positive multiple: MROUND(23, 10) gives 20, MROUND(0.47, 0.25) gives 0.50, MROUND(time, TIME(0,15,0)) rounds time to the nearest 15-minute mark. This makes MROUND extremely versatile for business scenarios requiring "nearest" rounding to non-decimal boundaries.

**MROUND vs ROUND:** ROUND controls decimal places (round to 2 decimals). MROUND controls the multiple (round to nearest 5, 0.25, etc.). Use ROUND when you think in terms of decimal places; use MROUND when you think in terms of multiples or increments.

## Syntax

> [!f(x)] MROUND Syntax
>
> ```
> =MROUND(number, multiple)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value you want to round. Can be positive, negative, or zero. |
| `multiple` | Yes | The multiple to which you want to round. Must have the same sign as number (or be zero). The result will be the multiple of this value closest to number. |

### Return Value

Returns the multiple of 'multiple' that is closest to 'number'. Uses "round half up" (away from zero at midpoint). Returns #NUM! if number and multiple have different signs. Returns 0 if multiple is 0.

## Examples

> [!f(x)] MROUND Examples

### Example 1: Round to Nearest 5 (Goes Up)
```
=MROUND(14, 5)
```
**Result:** 15

**Explanation:** 14 is between 10 and 15. It's closer to 15 (distance 1) than to 10 (distance 4), so MROUND returns 15.

---

### Example 2: Round to Nearest 5 (Goes Down)
```
=MROUND(12, 5)
```
**Result:** 10

**Explanation:** 12 is between 10 and 15. It's closer to 10 (distance 2) than to 15 (distance 3), so MROUND returns 10.

---

### Example 3: Exactly at Midpoint
```
=MROUND(12.5, 5)
```
**Result:** 15

**Explanation:** 12.5 is exactly halfway between 10 and 15. MROUND rounds up (away from zero) at midpoint, so it returns 15.

---

### Example 4: Round to Nearest 10
```
=MROUND(247, 10)
```
**Result:** 250

**Explanation:** 247 is between 240 and 250. It's closer to 250 (distance 3) than to 240 (distance 7).

---

### Example 5: Round to Nearest 0.25 (Quarter)
```
=MROUND(3.42, 0.25)
```
**Result:** 3.5

**Explanation:** 3.42 is between 3.25 and 3.50. It's closer to 3.50 (distance 0.08) than to 3.25 (distance 0.17).

---

### Example 6: Round to Nearest 0.05
```
=MROUND(3.47, 0.05)
```
**Result:** 3.45

**Explanation:** 3.47 is between 3.45 and 3.50. It's closer to 3.45 (distance 0.02) than to 3.50 (distance 0.03).

---

### Example 7: Negative Number
```
=MROUND(-14, -5)
```
**Result:** -15

**Explanation:** -14 is between -15 and -10. It's closer to -15 (distance 1) than to -10 (distance 4). Note: multiple must also be negative.

---

### Example 8: Round Time to Nearest 15 Minutes
```
=MROUND(A1, TIME(0,15,0))
```
**Result:** Time rounded to nearest 15-minute mark

**Explanation:** If A1 is 9:37 AM, result is 9:30 AM. If A1 is 9:38 AM, result is 9:45 AM.

---

### Example 9: Round to Nearest Dollar
```
=MROUND(Price, 1)
```
**Result:** Price rounded to nearest whole dollar

**Explanation:** $4.49 becomes $4, $4.50 becomes $5.

---

### Example 10: Round to Nearest Half Dollar
```
=MROUND(Price, 0.5)
```
**Result:** Price rounded to nearest 50 cents

**Explanation:** $4.24 becomes $4.00, $4.25 becomes $4.50, $4.74 becomes $4.50.

---

### Example 11: Error - Mismatched Signs
```
=MROUND(-14, 5)
```
**Result:** #NUM!

**Explanation:** In Excel, number and multiple must have the same sign. Use MROUND(-14, -5) for negative numbers, or use ABS with SIGN.

---

### Example 12: Large Rounding Multiple
```
=MROUND(4567, 100)
```
**Result:** 4600

**Explanation:** 4567 is between 4500 and 4600. It's closer to 4600 (distance 33) than to 4500 (distance 67).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number and multiple have different signs | Both must have same sign in Excel. Use `=SIGN(A1)*MROUND(ABS(A1), ABS(B1))` for mixed. |
| `#VALUE!` | Non-numeric input | Ensure both arguments are numbers. |
| `#NAME?` | Misspelled function name | Check spelling: MROUND, not MULTIROUND or MRAND. |
| `Result is 0` | Multiple is 0 | MROUND(x, 0) returns 0. Ensure multiple is non-zero for meaningful results. |
| `Unexpected rounding direction` | Midpoint behavior | MROUND rounds UP at exact midpoints. 7.5 with multiple 5 gives 10, not 5. |

## Use Cases

### [[Pricing and Financial Rounding]]

**Scenario:** Round prices to standard increments.

**Implementation:**
```
=MROUND(CalculatedPrice, 0.99)            → Round to XX.99 (approximate)
=MROUND(Amount, 0.05)                      → Round to nearest nickel
=MROUND(Payment, 10)                       → Round to nearest $10
=MROUND(Estimate, 100)                     → Round estimate to nearest $100
```

**Business Application:** Retail pricing, invoice rounding, estimate presentation, tip calculations.

**Technical Details:** MROUND is ideal for "psychological pricing" where you want prices at familiar points ($4.99, $5.50, etc.). Unlike CEILING/FLOOR, it picks the genuinely closest value.

---

### [[Time Tracking and Scheduling]]

**Scenario:** Round times to standard intervals.

**Implementation:**
```
=MROUND(ActualTime, TIME(0,15,0))         → Nearest 15 minutes
=MROUND(Duration, TIME(0,30,0))            → Nearest half hour
=MROUND(StartTime, TIME(0,5,0))            → Nearest 5 minutes
```

**Business Application:** Meeting scheduling, time card rounding, appointment booking, shift time standardization.

**Technical Details:** Many scheduling systems work in fixed increments. MROUND finds the closest slot, which is often the most natural choice for time rounding.

---

### [[Measurement and Reporting]]

**Scenario:** Round measurements to practical precision levels.

**Implementation:**
```
=MROUND(Measurement, 0.125)               → Nearest 1/8 inch
=MROUND(Weight, 0.5)                       → Nearest half pound
=MROUND(Temperature, 0.5)                  → Nearest half degree
=MROUND(Metric, 5)                         → Nearest 5 units
```

**Business Application:** Quality control reporting, manufacturing measurements, scientific data presentation.

**Technical Details:** Real-world measurements often have practical limits on meaningful precision. MROUND helps present data at appropriate granularity.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 2000
- **Sign requirement:** Number and multiple must have the same sign; returns #NUM! otherwise
- **Zero multiple:** Returns 0
- **Midpoint:** Rounds away from zero (up for positive, down for negative)

### Google Sheets
- **Availability:** All versions
- **Sign requirement:** Same as Excel—same sign required
- **Zero multiple:** Returns 0
- **Midpoint:** Same rounding behavior

### Both Platforms
- Identical mathematical behavior
- Same sign restrictions
- Same midpoint handling
- No functional differences between platforms

## Tips and Best Practices

1. **MROUND finds the nearest:** Unlike CEILING (up) or FLOOR (down), MROUND goes to whichever multiple is closest.

2. **Same sign required:** Both arguments must be positive or both must be negative in Excel. For mixed scenarios, use `=SIGN(A1)*MROUND(ABS(A1), ABS(B1))`.

3. **Midpoint rounds up:** At exact midpoints, MROUND rounds away from zero. MROUND(2.5, 1) = 3, MROUND(-2.5, -1) = -3.

4. **Use for time intervals:** TIME(0,15,0) represents 15 minutes. MROUND with time is powerful for scheduling.

5. **MROUND vs ROUND:** ROUND(x, decimals) controls decimal places. MROUND(x, multiple) controls the increment. Different use cases.

6. **Common multiples:** 5, 10, 0.25, 0.05, 0.5 are typical. Think about what increment makes sense for your domain.

7. **Combine with CEILING/FLOOR:** Use MROUND for "nearest," CEILING for "next up," FLOOR for "next down"—each has its place.

8. **Works with negative multiples:** For negative numbers, use negative multiple. MROUND(-14, -5) = -15.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CEILING]] | Rounds UP to multiple | When you always need to round up |
| [[FLOOR]] | Rounds DOWN to multiple | When you always need to round down |
| [[ROUND]] | Rounds to decimal places | When you need precision control by decimals, not multiples |
| [[ROUNDDOWN]] | Rounds down by decimal places | For decimal truncation |
| [[ROUNDUP]] | Rounds up by decimal places | For decimal ceiling |

### Commonly Used Together

**[[CEILING]] and [[FLOOR]]** - Directional alternatives

*Compare behaviors:*
```
=FLOOR(12, 5)    → 10 (down)
=MROUND(12, 5)   → 10 (nearest, which is down)
=CEILING(12, 5)  → 15 (up)
```
Three options for rounding to 5.

---

**[[TIME]]** - For time-based multiples

*Time interval rounding:*
```
=MROUND(A1, TIME(0, 15, 0))
```
Round time to nearest 15 minutes.

---

**[[SIGN]] and [[ABS]]** - Handle mixed signs

*Universal sign handling:*
```
=SIGN(A1) * MROUND(ABS(A1), ABS(B1))
```
Works regardless of signs.

---

**[[IF]]** - Conditional rounding

*Round only when needed:*
```
=IF(A1 > 100, MROUND(A1, 10), A1)
```
Apply MROUND conditionally.

---

**[[ROUND]]** - Decimal precision alternative

*Different approach:*
```
=ROUND(A1, 2)     → Rounds to 0.01 (2 decimal places)
=MROUND(A1, 0.01) → Same result, different thinking
```
Two ways to get the same result.

## Official Documentation

- **Microsoft Excel:** [MROUND function](https://support.microsoft.com/en-us/office/mround-function-c299c3b0-15a5-426d-aa4b-d2d5b3baf427)
- **Google Sheets:** [MROUND function](https://support.google.com/docs/answer/3093426)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Introduced (requires Analysis ToolPak in older versions) |
| Excel 2007+ | Built-in | No longer requires ToolPak |
| Google Sheets | Original launch | Matches Excel behavior |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
