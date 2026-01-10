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
- floor-functions
- precision
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Floor Math
- Round Down Math
- FLOOR Mathematical
tags:
- rounding
- floor
- precision
- financial
- excel-2013
---

# FLOOR.MATH

## Description

**FLOOR.MATH** rounds a number down to the nearest integer or to the nearest multiple of a specified significance. Unlike simple truncation, floor rounds toward negative infinity—meaning positive numbers get smaller and negative numbers get more negative (unless you tell it otherwise). It's the mathematical equivalent of "always round down to the previous [something]": previous dollar, previous 5 cents, previous hour.

This function was introduced in Excel 2013 to address the confusing behavior of FLOOR with negative numbers. The original FLOOR function's handling of negative values was inconsistent across Excel versions and differed from Google Sheets. FLOOR.MATH provides cleaner, more predictable behavior and adds explicit control over how negative numbers are rounded.

**The mode parameter for negative numbers:** By default, FLOOR.MATH rounds negative numbers toward negative infinity (e.g., -4.2 becomes -5). With mode set to any non-zero value, it rounds toward zero instead (e.g., -4.2 becomes -4). This flexibility is crucial for applications where you need consistent "rounding toward zero" behavior regardless of sign.

**When to use this vs. FLOOR:** Use FLOOR.MATH for new work—it's clearer and more consistent. The original FLOOR function remains for backward compatibility. In Google Sheets, FLOOR has similar functionality but with slightly different parameter handling. For cross-platform work, test negative number behavior carefully.

## Syntax

> [!f(x)] FLOOR.MATH Syntax
>
> ```
> =FLOOR.MATH(number, [significance], [mode])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value to be rounded down. |
| `significance` | No | The multiple to which you want to round. Default is 1 (round to nearest integer). Sign is ignored—only absolute value matters. |
| `mode` | No | Controls rounding direction for negative numbers. 0 or omitted = round toward negative infinity (away from zero). Non-zero = round toward zero. |

### Return Value

Returns the number rounded down to the nearest multiple of significance. Returns #VALUE! if parameters are non-numeric. Returns #DIV/0! if significance is 0.

## Examples

> [!f(x)] FLOOR.MATH Examples

### Example 1: Round Down to Integer
```
=FLOOR.MATH(4.9)
```
**Result:** 4

**Explanation:** With no significance specified, rounds down to the nearest integer. 4.9 becomes 4.

---

### Example 2: Round Down to Nearest 5
```
=FLOOR.MATH(17, 5)
```
**Result:** 15

**Explanation:** Rounds 17 down to the nearest multiple of 5. The previous multiple of 5 below 17 is 15.

---

### Example 3: Round Down to Nearest 0.5
```
=FLOOR.MATH(2.74, 0.5)
```
**Result:** 2.5

**Explanation:** Rounds down to nearest half. 2.74 rounds down to 2.5. Useful for conservative estimates.

---

### Example 4: Negative Number - Default Mode
```
=FLOOR.MATH(-4.2)
```
**Result:** -5

**Explanation:** Default mode rounds toward negative infinity. -4.2 rounds "down" away from zero to -5.

---

### Example 5: Negative Number - Mode = 1
```
=FLOOR.MATH(-4.2, 1, 1)
```
**Result:** -4

**Explanation:** With mode=1, rounds toward zero. -4.2 rounds up toward zero to -4. Often desired for truncation-like behavior.

---

### Example 6: Time Rounding - Hourly
```
=FLOOR.MATH(A1, TIME(1,0,0))
```
Where A1 contains 10:47 AM

**Result:** 10:00 AM

**Explanation:** Rounds time down to the previous whole hour. Useful for truncating to hour boundaries.

---

### Example 7: Round Down to Tens
```
=FLOOR.MATH(87, 10)
```
**Result:** 80

**Explanation:** Rounds down to the nearest 10. 87 becomes 80. Common for conservative estimates.

---

### Example 8: Round Down to Hundreds
```
=FLOOR.MATH(1234, 100)
```
**Result:** 1200

**Explanation:** Rounds down to the nearest hundred. 1234 becomes 1200. Useful for budget floors.

---

### Example 9: Exact Multiple (No Rounding Needed)
```
=FLOOR.MATH(25, 5)
```
**Result:** 25

**Explanation:** When number is already an exact multiple of significance, no rounding occurs. 25 is already a multiple of 5.

---

### Example 10: Zero Value
```
=FLOOR.MATH(0, 5)
```
**Result:** 0

**Explanation:** Zero is a multiple of any significance, so it returns zero unchanged.

---

### Example 11: Negative Significance (Ignored)
```
=FLOOR.MATH(17, -5)
```
**Result:** 15

**Explanation:** The sign of significance is ignored in FLOOR.MATH. -5 is treated as 5. This differs from original FLOOR.

---

### Example 12: Financial - Conservative Revenue
```
=FLOOR.MATH(revenue, 1000)
```
Where revenue = 45678

**Result:** 45000

**Explanation:** Round revenue down to nearest thousand for conservative reporting. Never overstate.

---

### Example 13: Age Calculation
```
=FLOOR.MATH((TODAY() - birthdate) / 365.25, 1)
```
**Result:** Age in whole years (rounded down)

**Explanation:** Age is typically expressed in completed years, not rounded. FLOOR.MATH ensures you don't overstate age.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure number, significance, and mode are all numeric values. |
| `#DIV/0!` | Significance is zero | Significance cannot be zero. Use 1 for integer rounding or specify a valid multiple. |
| `#NAME?` | Function not recognized | Ensure Excel 2013 or later. Use FLOOR in older versions or Google Sheets. |
| `Unexpected result` | Confusion about negative number handling | Understand mode parameter: 0 = toward negative infinity, non-zero = toward zero. |
| `Different results in Sheets` | Platform differences | Google Sheets FLOOR behaves differently. Test negative number cases specifically. |

## Use Cases

### [[Conservative Financial Estimates]]

**Scenario:** Round financial projections down to provide conservative estimates that won't overstate expected values.

**Implementation:**
```
Revenue floor: =FLOOR.MATH(projected_revenue, 10000)
Cost estimate: =CEILING.MATH(projected_cost, 10000)
```

**Business Application:** Budget planning, investor presentations, financial forecasting. Conservative estimates are often preferred for credibility.

**Technical Details:** Pair FLOOR.MATH for income/benefits with CEILING.MATH for costs/risks to create conservative scenarios.

---

### [[Time Truncation]]

**Scenario:** Truncate time entries to hour or minute boundaries for reporting or billing floors.

**Implementation:**
```
Hour boundary: =FLOOR.MATH(time_value, TIME(1,0,0))
15-min boundary: =FLOOR.MATH(time_value, TIME(0,15,0))
```

**Business Application:** Time tracking, shift reporting, attendance systems. Sometimes you need to truncate, not round.

**Technical Details:** Combined with CEILING.MATH, can create time ranges. Works with Excel time values (fractions of a day).

---

### [[Bin/Bucket Assignments]]

**Scenario:** Assign values to bins or ranges for histogram or grouping purposes.

**Implementation:**
```
Bin start: =FLOOR.MATH(value, bin_width)
Bin label: =FLOOR.MATH(value, 10) & "-" & (FLOOR.MATH(value, 10) + 9)
```

**Business Application:** Data analysis, reporting, customer segmentation. Group continuous data into discrete bins.

**Technical Details:** FLOOR.MATH gives the lower bound of each bin. Adding (bin_width - 1) gives the upper bound for display.

---

### [[Inventory Management]]

**Scenario:** Calculate available full units when partial units can't be sold.

**Implementation:**
```
Full cases: =FLOOR.MATH(total_units / units_per_case, 1)
Sellable units: =FLOOR.MATH(total_units, minimum_sellable_qty)
```

**Business Application:** Warehouse management, retail inventory, manufacturing. Can only sell complete units/cases.

**Technical Details:** Don't round up available inventory—you can't sell what you don't have. FLOOR.MATH ensures conservative counting.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Mode parameter:** Fully supported with 0/non-zero behavior
- **Significance sign:** Absolute value used (sign ignored)
- **Replaces:** FLOOR (for cleaner behavior)

### Google Sheets
- **Function name:** FLOOR (no FLOOR.MATH)
- **Syntax:** =FLOOR(value, [factor])
- **No mode parameter:** Different negative number handling
- **Behavior:** Rounds toward negative infinity for both positive and negative

### Key Platform Difference
```
Excel FLOOR.MATH(-4.2, 1, 0) = -5  (toward negative infinity)
Excel FLOOR.MATH(-4.2, 1, 1) = -4  (toward zero)
Sheets FLOOR(-4.2, 1) = -5         (toward negative infinity)
```
Google Sheets FLOOR matches Excel FLOOR.MATH with mode=0 for negative numbers.

### Compatibility Approach
For cross-platform work with negative numbers:
```
// Excel: Use FLOOR.MATH with explicit mode
// Sheets: Use FLOOR, test negative cases
// Both: Consider INT for toward-negative-infinity integer rounding
```

## Tips and Best Practices

1. **Default is toward negative infinity:** Without mode parameter, FLOOR.MATH rounds negative numbers away from zero (-4.2 becomes -5). Set mode=1 to round toward zero.

2. **Significance sign doesn't matter:** Unlike original FLOOR, the sign of significance is ignored. FLOOR.MATH(17, -5) = FLOOR.MATH(17, 5) = 15.

3. **Use 1 for integer floor:** `=FLOOR.MATH(4.9)` or `=FLOOR.MATH(4.9, 1)` both round down to the nearest integer.

4. **Test negative number cases:** If your data includes negative values, explicitly test both mode settings to ensure correct behavior.

5. **Pair with CEILING.MATH:** FLOOR.MATH and CEILING.MATH together create ranges: `[FLOOR(x), CEILING(x)]` contains x.

6. **Consider INT for integers:** `=INT(x)` is equivalent to `=FLOOR.MATH(x, 1)` for positive numbers, but INT always rounds toward negative infinity.

7. **Use for conservative estimates:** When you must not overstate (inventory, revenue, time), use FLOOR.MATH rather than ROUND.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FLOOR]] | Original floor function | Legacy compatibility (older Excel versions) |
| [[CEILING.MATH]] | Rounds up (toward positive infinity) | When you need to round up instead of down |
| [[MROUND]] | Rounds to nearest multiple | When you want nearest (not always down) |
| [[ROUNDDOWN]] | Rounds down by decimal places | When rounding by decimal places, not multiples |
| [[INT]] | Rounds down to integer | Simple truncation toward negative infinity |
| [[TRUNC]] | Truncates toward zero | When you want truncation regardless of sign |

### Commonly Used Together

**[[CEILING.MATH]]** - Complementary rounding up

*Create ranges:*
```
Lower bound: =FLOOR.MATH(value, 10)
Upper bound: =CEILING.MATH(value, 10)
```
Useful for binning or range calculations.

---

**[[TIME]]** - Create time significance

*Round to time boundaries:*
```
=FLOOR.MATH(time_value, TIME(1, 0, 0))  // Round down to hour
```
TIME function creates the fractional day value for time-based rounding.

---

**[[IF]]** - Conditional rounding

*Different rounding for different cases:*
```
=IF(A1>=0, FLOOR.MATH(A1, 5), FLOOR.MATH(A1, 5, 1))
```
Apply different mode based on sign or other conditions.

---

**[[ABS]]** - Work with absolute values

*Round magnitude, preserve sign:*
```
=SIGN(A1) * FLOOR.MATH(ABS(A1), 5)
```
Round absolute value down, then reapply original sign.

---

**[[INT]]** - Integer floor alternative

*Simpler integer rounding:*
```
=INT(A1)  // Same as FLOOR.MATH(A1, 1) for default mode
```
INT is simpler when you just need integer floor.

## Official Documentation

- **Microsoft Excel:** [FLOOR.MATH function](https://support.microsoft.com/en-us/office/floor-math-function-c302b599-fbdb-4177-ba19-2c2b1249a2f5)
- **Google Sheets:** [FLOOR function](https://support.google.com/docs/answer/3093487) (FLOOR.MATH not available; use FLOOR)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Introduced to replace inconsistent FLOOR behavior |
| Excel 2016+ | Maintained | No changes to functionality |
| Google Sheets | N/A | Use FLOOR function instead (different parameter structure) |

---

*Last updated: 2026-01-10*
