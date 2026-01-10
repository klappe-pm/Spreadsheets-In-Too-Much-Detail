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
- ceiling
- multiples
- round-up
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Round Up to Multiple
- Ceiling Round
tags:
- rounding
- ceiling
- multiples
- round-up
- toward-infinity
---

# CEILING

## Description

**CEILING** rounds a number UP to the nearest multiple of a specified significance. Unlike ROUND which goes to the nearest value, CEILING always rounds away from zero toward positive infinity for positive numbers. CEILING(4.2, 1) returns 5, CEILING(4.2, 0.5) returns 4.5, CEILING(4.2, 5) returns 5. The function is essential for scenarios where you need to round up to specific increments.

**The "significance" parameter controls the increment:** CEILING doesn't just round up to the next integer—it rounds up to the next multiple of whatever significance you specify. CEILING(23, 10) gives 30 (next multiple of 10). CEILING(0.42, 0.25) gives 0.50 (next quarter). CEILING(time, TIME(0,15,0)) rounds time up to the next 15-minute mark. This flexibility makes CEILING powerful for real-world rounding scenarios.

**Negative number behavior differs by platform:** This is where CEILING gets tricky. In Excel (traditional CEILING), negative numbers round toward zero: CEILING(-4.2, -1) = -4 (not -5). Google Sheets' CEILING rounds away from zero (more negative): CEILING(-4.2, 1) = -4. Excel 2010+ added CEILING.MATH and CEILING.PRECISE with different behaviors. Always test your specific use case with negative numbers.

**CEILING vs FLOOR vs MROUND:** CEILING always rounds up (away from zero for positives). FLOOR always rounds down (toward zero for positives). MROUND goes to the nearest multiple. Choose based on your business rule: shipping weights often need CEILING (charge for next increment), age calculations often need FLOOR (complete years only), general estimation often needs MROUND (closest value).

## Syntax

> [!f(x)] CEILING Syntax
>
> ```
> =CEILING(number, significance)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value you want to round up. Can be positive, negative, or zero. |
| `significance` | Yes | The multiple to which you want to round. In Excel, must have the same sign as number (or errors occur). In Google Sheets, the sign doesn't matter for positive numbers. |

### Return Value

Returns the smallest multiple of 'significance' that is greater than or equal to 'number' (for positive numbers). Returns #NUM! if number and significance have different signs in Excel. Returns #VALUE! if either argument is non-numeric. Returns 0 if significance is 0.

## Examples

> [!f(x)] CEILING Examples

### Example 1: Round Up to Next Integer
```
=CEILING(4.2, 1)
```
**Result:** 5

**Explanation:** 4.2 rounds up to the next multiple of 1, which is 5. This is the most basic CEILING use.

---

### Example 2: Round Up to Next 5
```
=CEILING(23, 5)
```
**Result:** 25

**Explanation:** 23 rounds up to the next multiple of 5. The multiples are 20, 25, 30... so 23 goes to 25.

---

### Example 3: Round Up to Next 10
```
=CEILING(247, 10)
```
**Result:** 250

**Explanation:** 247 rounds up to the next multiple of 10 (250). Useful for presenting rounded estimates.

---

### Example 4: Round Up to Next 0.25 (Quarter)
```
=CEILING(3.42, 0.25)
```
**Result:** 3.50

**Explanation:** 3.42 is between 3.25 and 3.50. CEILING goes up to 3.50 (the next quarter).

---

### Example 5: Round Up to Next 0.05 (Nickel)
```
=CEILING(3.42, 0.05)
```
**Result:** 3.45

**Explanation:** 3.42 is between 3.40 and 3.45. CEILING rounds up to 3.45.

---

### Example 6: Already at a Multiple
```
=CEILING(25, 5)
```
**Result:** 25

**Explanation:** 25 is already a multiple of 5. CEILING returns it unchanged—no rounding needed.

---

### Example 7: Very Small Number
```
=CEILING(0.001, 1)
```
**Result:** 1

**Explanation:** Any positive non-zero value less than 1 rounds up to 1 when significance is 1.

---

### Example 8: Round Time to Next 15 Minutes
```
=CEILING(A1, TIME(0,15,0))
```
**Result:** Time rounded up to next 15-minute mark

**Explanation:** If A1 is 9:37 AM, result is 9:45 AM. TIME(0,15,0) represents 15 minutes as a time value.

---

### Example 9: Negative Number (Excel Behavior)
```
=CEILING(-4.2, -1)
```
**Result:** -4 (Excel)

**Explanation:** In Excel, both arguments must have the same sign. CEILING(-4.2, -1) rounds toward zero (less negative), giving -4.

---

### Example 10: Large Multiple
```
=CEILING(1234, 100)
```
**Result:** 1300

**Explanation:** 1234 rounds up to the next multiple of 100. Useful for budget estimates or summary figures.

---

### Example 11: Pricing to Next Dollar
```
=CEILING(Price, 1)
```
**Result:** Price rounded up to next whole dollar

**Explanation:** $4.01 becomes $5. Useful when you always want to round prices up.

---

### Example 12: Zero Significance
```
=CEILING(5, 0)
```
**Result:** 0 (or #DIV/0! in some versions)

**Explanation:** Significance of 0 makes the result undefined or zero. Avoid zero significance.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | In Excel, number and significance have different signs | Make both positive or both negative. Use CEILING.MATH for more control. |
| `#VALUE!` | Non-numeric input | Ensure both arguments are numbers. |
| `#NAME?` | Misspelled function name | Check spelling: CEILING, not CIELING or CELING. |
| `#DIV/0!` | Significance is 0 | Use a non-zero significance value. |
| `Unexpected negative result` | Misunderstanding negative behavior | Excel CEILING rounds negative numbers toward zero. Use CEILING.MATH with mode argument for different behavior. |
| `Result same as input` | Already at multiple | Not an error—input was already at a multiple of significance. |

## Use Cases

### [[Pricing and Financial Rounding]]

**Scenario:** Round prices, charges, and financial amounts up to standard increments.

**Implementation:**
```
=CEILING(CalculatedPrice, 0.99)               → Round to XX.99 (approximate)
=CEILING(ShippingWeight, 1)                    → Round weight up to next pound
=CEILING(BillableHours, 0.25)                  → Quarter-hour billing
=CEILING(InvoiceAmount, 0.05)                  → Round to nearest nickel
```

**Business Application:** Service billing (always round up), shipping calculations, minimum charges, payment processing.

**Technical Details:** Many business scenarios require "round up" logic—you can't ship 4.2 pounds at the 4-pound rate, and you can't bill 2.3 hours at the 2-hour rate. CEILING ensures you round to the next chargeable unit.

---

### [[Time Tracking and Scheduling]]

**Scenario:** Round times up to standard intervals for scheduling and billing.

**Implementation:**
```
=CEILING(ActualTime, TIME(0,15,0))            → Next 15 minutes
=CEILING(Duration, TIME(0,30,0))               → Next half hour
=CEILING(StartTime, TIME(1,0,0))               → Next full hour
```

**Business Application:** Meeting scheduling, billable time tracking, appointment booking, shift scheduling.

**Technical Details:** Time systems often work in fixed increments. CEILING ensures you get the next available slot or billable period, which is often required for conservative billing or scheduling.

---

### [[Inventory and Packaging]]

**Scenario:** Calculate quantities needed when items come in fixed package sizes.

**Implementation:**
```
=CEILING(ItemsNeeded, PackSize)               → Packages to order
=CEILING(Weight, ContainerCapacity)            → Containers needed
=CEILING(OrderQuantity, MinimumOrder)          → Meet minimum order quantity
```

**Business Application:** Inventory ordering, shipping container planning, bulk purchasing, warehouse management.

**Technical Details:** If you need 23 items and they come in packs of 10, you need 3 packs (30 items). CEILING(23, 10) = 30. This ensures you always have enough.

## Platform Differences

### Microsoft Excel
- **Basic CEILING:** Requires same sign for number and significance; rounds toward zero for negatives
- **CEILING.MATH (Excel 2013+):** More control with optional mode parameter for negative handling
- **CEILING.PRECISE (Excel 2010+):** Always rounds away from zero (toward positive infinity)
- **Sign restriction:** CEILING(negative, positive) returns #NUM! error

### Google Sheets
- **Availability:** All versions
- **Sign handling:** More flexible—CEILING(-4.2, 1) works and returns -4 (rounds toward zero)
- **Behavior:** Generally rounds toward positive infinity for positive, toward zero for negative
- **No CEILING.MATH:** Google Sheets doesn't have this variant

### Key Platform Difference

**For positive numbers:** Both platforms behave identically—round up to the next multiple.

**For negative numbers:**
- Excel requires same sign: `=CEILING(-4.2, -1)` returns -4
- Google Sheets allows positive significance: `=CEILING(-4.2, 1)` returns -4
- For Excel's "always toward positive infinity" behavior, use CEILING.PRECISE

## Tips and Best Practices

1. **CEILING always rounds up:** For positive numbers, this means larger. For negatives in standard CEILING, this means toward zero (less negative).

2. **Use CEILING.MATH in Excel for control:** The mode parameter lets you choose how negative numbers round. Mode = 0 or omitted rounds toward zero; Mode = -1 rounds away from zero.

3. **Watch the sign restriction in Excel:** CEILING(negative, positive) errors. Either make both negative or use CEILING.MATH/CEILING.PRECISE.

4. **For "always toward positive infinity":** In Excel, use CEILING.PRECISE. This rounds -4.2 to -4, and 4.2 to 5.

5. **Common significance values:** 1 (integers), 5, 10, 0.25 (quarters), 0.05 (nickels), TIME(0,15,0) (15 minutes).

6. **If already at multiple, no change:** CEILING(10, 5) = 10. The function returns the input when it's already at a multiple.

7. **Pair with FLOOR for range buckets:** CEILING gives upper bounds, FLOOR gives lower bounds. Together they define ranges.

8. **For nearest (not always up), use MROUND:** CEILING always goes up. If you want the closest multiple, MROUND is the right function.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FLOOR]] | Rounds DOWN to multiple | When you always need to round down |
| [[MROUND]] | Rounds to NEAREST multiple | When you want the closest value, not always up |
| [[ROUNDUP]] | Rounds up by decimal places | When thinking in decimal places, not multiples |
| [[INT]] | Truncates to integer | For simple truncation to whole numbers |
| [[CEILING.MATH]] | CEILING with mode control | When you need control over negative number handling |

### Commonly Used Together

**[[FLOOR]]** - Complementary function

*Compare behaviors:*
```
=FLOOR(23, 5)    → 20 (down)
=CEILING(23, 5)  → 25 (up)
=MROUND(23, 5)   → 25 (nearest)
```
Three options for rounding to 5.

---

**[[TIME]]** - For time-based multiples

*Time interval rounding:*
```
=CEILING(A1, TIME(0, 15, 0))
```
Round time up to next 15 minutes.

---

**[[MAX]]** - Ensure minimum values

*Combined ceiling with minimum:*
```
=MAX(CEILING(Amount, 5), MinimumCharge)
```
Round up but ensure at least minimum charge.

---

**[[IF]]** - Conditional rounding

*Apply CEILING conditionally:*
```
=IF(A1 > 0, CEILING(A1, 5), FLOOR(A1, 5))
```
Round up for positive, down for negative.

---

**[[MROUND]]** - Nearest multiple alternative

*Compare behaviors:*
```
=CEILING(12, 5)  → 15 (always up)
=MROUND(12, 5)   → 10 (nearest)
```
Different rounding strategies.

## Official Documentation

- **Microsoft Excel:** [CEILING function](https://support.microsoft.com/en-us/office/ceiling-function-0a5cd7c8-0720-4f0a-bd2c-c943e510899f)
- **Google Sheets:** [CEILING function](https://support.google.com/docs/answer/3093471)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Basic CEILING function |
| Excel 2010 | CEILING.PRECISE | Always rounds away from zero |
| Excel 2013 | CEILING.MATH | Added mode parameter for control |
| Google Sheets | Original launch | Slightly different negative handling |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
