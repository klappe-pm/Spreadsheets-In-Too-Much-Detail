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
- ceiling-functions
- precision
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Ceiling Math
- Round Up Math
- CEILING Mathematical
tags:
- rounding
- ceiling
- precision
- financial
- excel-2013
---

# CEILING.MATH

## Description

**CEILING.MATH** rounds a number up to the nearest integer or to the nearest multiple of a specified significance. Unlike simple rounding, ceiling always rounds away from zero toward positive infinity—unless you tell it otherwise for negative numbers. It's the mathematical equivalent of "always round up to the next [something]": next dollar, next 5 cents, next 15-minute increment, next hundred.

This function was introduced in Excel 2013 to address the confusing behavior of CEILING with negative numbers. The original CEILING function's handling of negative significance values was inconsistent across Excel versions and differed from Google Sheets, causing endless compatibility headaches. CEILING.MATH provides cleaner, more predictable behavior and adds explicit control over how negative numbers are rounded.

**The mode parameter for negative numbers:** By default, CEILING.MATH rounds negative numbers toward positive infinity (e.g., -4.2 becomes -4). With mode set to any non-zero value, it rounds away from zero instead (e.g., -4.2 becomes -5). This flexibility is crucial for financial and scientific applications where the direction of rounding for negative values matters.

**When to use this vs. CEILING:** Use CEILING.MATH for new work—it's clearer and more consistent. The original CEILING function remains for backward compatibility. In Google Sheets, CEILING has similar functionality but with slightly different parameter names. For cross-platform compatibility, carefully test rounding behavior with negative numbers.

## Syntax

> [!f(x)] CEILING.MATH Syntax
>
> ```
> =CEILING.MATH(number, [significance], [mode])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value to be rounded up. |
| `significance` | No | The multiple to which you want to round. Default is 1 (round to nearest integer). Sign is ignored—only absolute value matters. |
| `mode` | No | Controls rounding direction for negative numbers. 0 or omitted = round toward zero (up toward positive infinity). Non-zero = round away from zero (down toward negative infinity). |

### Return Value

Returns the number rounded up to the nearest multiple of significance. Returns #VALUE! if parameters are non-numeric. Returns #DIV/0! if significance is 0.

## Examples

> [!f(x)] CEILING.MATH Examples

### Example 1: Round Up to Integer
```
=CEILING.MATH(4.2)
```
**Result:** 5

**Explanation:** With no significance specified, rounds up to the nearest integer. 4.2 becomes 5.

---

### Example 2: Round Up to Nearest 5
```
=CEILING.MATH(17, 5)
```
**Result:** 20

**Explanation:** Rounds 17 up to the nearest multiple of 5. The next multiple of 5 above 17 is 20.

---

### Example 3: Round Up to Nearest 0.5
```
=CEILING.MATH(2.26, 0.5)
```
**Result:** 2.5

**Explanation:** Rounds up to nearest half. 2.26 rounds up to 2.5. Useful for currency with half-unit increments.

---

### Example 4: Negative Number - Default Mode
```
=CEILING.MATH(-4.2)
```
**Result:** -4

**Explanation:** Default mode rounds toward positive infinity. -4.2 rounds "up" toward zero to -4.

---

### Example 5: Negative Number - Mode = 1
```
=CEILING.MATH(-4.2, 1, 1)
```
**Result:** -5

**Explanation:** With mode=1, rounds away from zero. -4.2 rounds down to -5. This is often preferred for conservative estimates.

---

### Example 6: Time Rounding - 15 Minute Increments
```
=CEILING.MATH(A1, TIME(0,15,0))
```
Where A1 contains 10:23 AM (time value)

**Result:** 10:30 AM

**Explanation:** Rounds time up to the next 15-minute increment. 10:23 becomes 10:30. Essential for billing in quarter-hour blocks.

---

### Example 7: Price Rounding to Cents
```
=CEILING.MATH(24.553, 0.01)
```
**Result:** 24.56

**Explanation:** Rounds up to the nearest cent. Ensures prices always round up, never down—common in retail pricing.

---

### Example 8: Rounding to Hundreds
```
=CEILING.MATH(1234, 100)
```
**Result:** 1300

**Explanation:** Rounds up to the nearest hundred. 1234 becomes 1300. Useful for budget estimates or inventory ordering.

---

### Example 9: Exact Multiple (No Rounding Needed)
```
=CEILING.MATH(25, 5)
```
**Result:** 25

**Explanation:** When number is already an exact multiple of significance, no rounding occurs. 25 is already a multiple of 5.

---

### Example 10: Zero Value
```
=CEILING.MATH(0, 5)
```
**Result:** 0

**Explanation:** Zero is a multiple of any significance, so it returns zero unchanged.

---

### Example 11: Negative Significance (Ignored)
```
=CEILING.MATH(17, -5)
```
**Result:** 20

**Explanation:** The sign of significance is ignored in CEILING.MATH. -5 is treated as 5. This differs from original CEILING behavior.

---

### Example 12: Packaging Quantities
```
=CEILING.MATH(23, 6)
```
**Result:** 24

**Explanation:** If items come in packs of 6 and you need 23, you must buy 24 (4 packs). CEILING.MATH calculates minimum purchase.

---

### Example 13: Cell References
```
=CEILING.MATH(A1, B1, C1)
```
Where A1=-7.8, B1=2, C1=1

**Result:** -8

**Explanation:** All parameters can be cell references. With mode=1, -7.8 rounds away from zero to -8 (nearest multiple of 2).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure number, significance, and mode are all numeric values. |
| `#DIV/0!` | Significance is zero | Significance cannot be zero. Use 1 for integer rounding or specify a valid multiple. |
| `#NAME?` | Function not recognized | Ensure Excel 2013 or later. Use CEILING in older versions or Google Sheets. |
| `Unexpected result` | Confusion about negative number handling | Understand mode parameter: 0 = toward positive infinity, non-zero = away from zero. |
| `Different results in Sheets` | Platform differences | Google Sheets CEILING behaves differently. Test negative number cases specifically. |

## Use Cases

### [[Time Billing in Increments]]

**Scenario:** Round time entries up to the nearest billing increment (15 minutes, 30 minutes, 1 hour).

**Implementation:**
```
15-min billing: =CEILING.MATH(time_worked, TIME(0,15,0))
30-min billing: =CEILING.MATH(time_worked, TIME(0,30,0))
Hourly billing: =CEILING.MATH(time_worked, TIME(1,0,0))
```

**Business Application:** Law firms, consulting companies, contractors who bill in time increments. Ensures no underbilling for partial periods.

**Technical Details:** Works with Excel time values (fractions of a day). Combine with TEXT or TIME functions for display formatting.

---

### [[Inventory Order Quantities]]

**Scenario:** Calculate minimum order quantity when items must be ordered in specific pack sizes.

**Implementation:**
```
Packs needed: =CEILING.MATH(units_required, pack_size) / pack_size
Total units: =CEILING.MATH(units_required, pack_size)
```

**Business Application:** Purchasing, inventory management, manufacturing. Determine minimum order to meet requirements when suppliers have minimum quantities.

**Technical Details:** Divide result by pack_size to get number of packs. Consider combining with cost calculations for total spend.

---

### [[Price Rounding]]

**Scenario:** Round calculated prices up to standard pricing increments (nearest cent, nickel, dime, dollar).

**Implementation:**
```
To cent: =CEILING.MATH(price, 0.01)
To nickel: =CEILING.MATH(price, 0.05)
To dollar: =CEILING.MATH(price, 1)
To $5: =CEILING.MATH(price, 5)
```

**Business Application:** Retail pricing, invoice generation, cost estimation. Ensures conservative (higher) price estimates.

**Technical Details:** Upward rounding prevents accidental losses from rounding down. May require different handling for credits/refunds (negative amounts).

---

### [[Resource Capacity Planning]]

**Scenario:** Determine minimum resources needed when resources come in fixed capacities.

**Implementation:**
```
Servers needed: =CEILING.MATH(total_users / users_per_server, 1)
Rooms needed: =CEILING.MATH(attendees / room_capacity, 1)
```

**Business Application:** IT infrastructure planning, event management, manufacturing capacity. Can't have fractional servers or rooms.

**Technical Details:** Divide first, then ceiling to integer. Consider adding safety margin after calculation.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Mode parameter:** Fully supported with 0/non-zero behavior
- **Significance sign:** Absolute value used (sign ignored)
- **Replaces:** CEILING (for cleaner behavior)

### Google Sheets
- **Function name:** CEILING (no CEILING.MATH)
- **Syntax:** =CEILING(value, [factor])
- **No mode parameter:** Different negative number handling
- **Behavior:** Always rounds away from zero for both positive and negative

### Key Platform Difference
```
Excel CEILING.MATH(-4.2, 1, 0) = -4  (toward positive infinity)
Excel CEILING.MATH(-4.2, 1, 1) = -5  (away from zero)
Sheets CEILING(-4.2, 1) = -4         (toward positive infinity)
```
Google Sheets CEILING matches Excel CEILING.MATH with mode=0 for negative numbers.

### Compatibility Approach
For cross-platform work with negative numbers:
```
// Excel: Use CEILING.MATH with explicit mode
// Sheets: Use CEILING, test negative cases
// Both: Consider IF statement for explicit negative handling
```

## Tips and Best Practices

1. **Default is toward positive infinity:** Without mode parameter, CEILING.MATH rounds negative numbers toward zero (-4.2 becomes -4). Set mode=1 to round away from zero.

2. **Significance sign doesn't matter:** Unlike original CEILING, the sign of significance is ignored. CEILING.MATH(17, -5) = CEILING.MATH(17, 5) = 20.

3. **Use 1 for integer ceiling:** `=CEILING.MATH(4.2)` or `=CEILING.MATH(4.2, 1)` both round up to the nearest integer.

4. **Test negative number cases:** If your data includes negative values, explicitly test both mode settings to ensure correct behavior.

5. **Consider FLOOR.MATH for rounding down:** FLOOR.MATH is the complementary function that rounds toward negative infinity (down).

6. **Time rounding requires time values:** When rounding time, use TIME() function for significance: `=CEILING.MATH(A1, TIME(0,15,0))`.

7. **Combine with other functions for complex scenarios:** Use IF to apply different rounding for positive vs. negative, or different significance for different ranges.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CEILING]] | Original ceiling function | Legacy compatibility (older Excel versions) |
| [[FLOOR.MATH]] | Rounds down (toward negative infinity) | When you need to round down instead of up |
| [[MROUND]] | Rounds to nearest multiple | When you want nearest (not always up) |
| [[ROUNDUP]] | Rounds up by decimal places | When rounding by decimal places, not multiples |
| [[INT]] | Rounds down to integer | Simple truncation toward negative infinity |

### Commonly Used Together

**[[FLOOR.MATH]]** - Complementary rounding down

*Create ranges:*
```
Lower bound: =FLOOR.MATH(value, 10)
Upper bound: =CEILING.MATH(value, 10)
```
Useful for binning or range calculations.

---

**[[TIME]]** - Create time significance

*Round to time increments:*
```
=CEILING.MATH(time_value, TIME(0, 15, 0))  // Round to 15 min
```
TIME function creates the fractional day value for time-based rounding.

---

**[[IF]]** - Conditional rounding

*Different rounding for positive/negative:*
```
=IF(A1>=0, CEILING.MATH(A1, 5), CEILING.MATH(A1, 5, 1))
```
Apply different mode based on sign or other conditions.

---

**[[ABS]]** - Work with absolute values

*Round magnitude, preserve sign:*
```
=SIGN(A1) * CEILING.MATH(ABS(A1), 5)
```
Round absolute value up, then reapply original sign.

---

**[[TEXT]]** - Format results

*Display rounded values:*
```
=TEXT(CEILING.MATH(A1, 0.05), "$#,##0.00")
```
Format ceiling result for presentation.

## Official Documentation

- **Microsoft Excel:** [CEILING.MATH function](https://support.microsoft.com/en-us/office/ceiling-math-function-80f95d2f-b499-4eee-9f16-f795a8e306c8)
- **Google Sheets:** [CEILING function](https://support.google.com/docs/answer/3093471) (CEILING.MATH not available; use CEILING)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Introduced to replace inconsistent CEILING behavior |
| Excel 2016+ | Maintained | No changes to functionality |
| Google Sheets | N/A | Use CEILING function instead (different parameter structure) |

---

*Last updated: 2026-01-10*
