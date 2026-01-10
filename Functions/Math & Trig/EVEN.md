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
- even
- integers
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Round to Even Integer
- Next Even Number
tags:
- rounding
- even
- integers
- round-up
- away-from-zero
---

# EVEN

## Description

**EVEN** rounds a number UP to the nearest even integer, always rounding **away from zero**. EVEN(3) returns 4, EVEN(2.1) returns 4, EVEN(-3) returns -4 (not -2). Unlike CEILING which rounds toward zero for negatives, EVEN consistently rounds away from zero for both positive and negative numbers.

**The "away from zero" behavior is key:** For positive numbers, EVEN rounds up (toward positive infinity). For negative numbers, EVEN rounds "up" in magnitude (toward negative infinity, away from zero). This makes EVEN predictable: the result's absolute value is always >= the input's absolute value.

**Why use EVEN?** Several practical scenarios require even numbers: pair-based inventory counting, even spacing in layouts, certain engineering calculations, and when working with data that naturally comes in pairs. It's also useful for buffer calculations where you want symmetric allowances.

**Relationship to ODD:** EVEN and ODD are complementary functions. EVEN rounds to the next even integer away from zero; ODD rounds to the next odd integer away from zero. Together they cover all integer rounding scenarios where parity matters.

## Syntax

> [!f(x)] EVEN Syntax
>
> ```
> =EVEN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value to round to the next even integer. Can be positive, negative, or zero. Decimals are rounded up in magnitude to the next even integer. |

### Return Value

Returns the smallest even integer whose absolute value is >= the absolute value of the input. Always rounds away from zero. EVEN(0) returns 0. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] EVEN Examples

### Example 1: Positive Integer to Even
```
=EVEN(3)
```
**Result:** 4

**Explanation:** 3 is odd, so round up to the next even integer: 4.

---

### Example 2: Already Even
```
=EVEN(4)
```
**Result:** 4

**Explanation:** 4 is already even. EVEN returns it unchanged.

---

### Example 3: Decimal to Even
```
=EVEN(2.1)
```
**Result:** 4

**Explanation:** 2.1 rounds up. Even though 2 is even, 2.1 > 2, so it goes to the next even: 4.

---

### Example 4: Small Decimal
```
=EVEN(0.5)
```
**Result:** 2

**Explanation:** 0.5 rounds up away from zero to the first positive even integer: 2.

---

### Example 5: Negative Integer
```
=EVEN(-3)
```
**Result:** -4

**Explanation:** -3 is odd. "Up" in magnitude (away from zero) means -4, not -2. This is key EVEN behavior.

---

### Example 6: Negative Decimal
```
=EVEN(-2.1)
```
**Result:** -4

**Explanation:** -2.1 is below -2 (which is even). Away from zero means -4.

---

### Example 7: Zero
```
=EVEN(0)
```
**Result:** 0

**Explanation:** 0 is even. EVEN returns 0.

---

### Example 8: Negative Already Even
```
=EVEN(-4)
```
**Result:** -4

**Explanation:** -4 is already even. EVEN returns it unchanged.

---

### Example 9: Large Number
```
=EVEN(99)
```
**Result:** 100

**Explanation:** 99 is odd. The next even integer is 100.

---

### Example 10: Very Small Positive
```
=EVEN(0.001)
```
**Result:** 2

**Explanation:** Any positive non-zero number rounds up to at least 2 (the first positive even integer).

---

### Example 11: Compare to CEILING
```
=EVEN(3)     → 4
=CEILING(3, 2) → 4
```
**Result:** Both return 4 for positive numbers

**Explanation:** For positive numbers, EVEN and CEILING(..., 2) give similar results. But they differ for negatives: EVEN(-3) = -4, but CEILING(-3, 2) = -2.

---

### Example 12: Pair Packaging
```
=EVEN(ItemCount)
```
**Result:** Items needed to complete pairs

**Explanation:** If you have 7 items that ship in pairs, you'd need to round to 8 for full pair packaging.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text values cause this error. |
| `#NAME?` | Misspelled function name | Check spelling: EVEN, not EVN. |
| `Expected -2, got -4` | Misunderstanding direction | EVEN rounds AWAY from zero for negatives. -3 becomes -4, not -2. |
| `Decimal input gave unexpected result` | Non-integer behavior | EVEN rounds decimals up to the next even integer away from zero. EVEN(3.1) = 4, EVEN(-3.1) = -4. |

## Use Cases

### [[Pair-Based Inventory and Packaging]]

**Scenario:** Calculate quantities when items are sold or packed in pairs.

**Implementation:**
```
=EVEN(Quantity)                           → Pairs needed
=EVEN(OrderSize) / 2                      → Number of pairs
=EVEN(HeadCount)                          → People for pair activities
```

**Business Application:** Shoe inventory (pairs), glove inventory, couple seating, two-person team assignments.

**Technical Details:** When items must be in pairs, EVEN ensures you have complete pairs. Odd quantities round up to the next pair count.

---

### [[Symmetrical Buffer Calculations]]

**Scenario:** Create equal-sized buffers on both sides of something.

**Implementation:**
```
=EVEN(MarginNeeded)                       → Symmetrical margins
=EVEN(PaddingTotal) / 2                   → Each side's padding
=EVEN(ToleranceRange)                     → Equal plus/minus tolerance
```

**Business Application:** Design layout spacing, engineering tolerances, scheduling buffers.

**Technical Details:** When you need equal buffers on both sides, the total must be even. EVEN ensures this, allowing clean division by 2.

---

### [[Grid and Layout Calculations]]

**Scenario:** Ensure dimensions work for two-column or bilateral layouts.

**Implementation:**
```
=EVEN(ColumnCount)                        → Even columns for bilateral symmetry
=EVEN(CEILING(Items / Rows, 1))           → Columns needed (must be even)
=EVEN(GridWidth / CellWidth)              → Even cell count
```

**Business Application:** Print layout, grid design, bilateral visual presentations, seating charts.

**Technical Details:** Many layouts require even numbers for symmetry (left-right balance). EVEN ensures dimensions meet this requirement.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Behavior:** Rounds away from zero for both positive and negative numbers
- **Zero:** Returns 0
- **Precision:** Works with full double precision

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel—rounds away from zero
- **Zero:** Returns 0
- **Precision:** Same as Excel

### Both Platforms
- Identical mathematical behavior
- Same treatment of negative numbers
- No functional differences between platforms

## Tips and Best Practices

1. **Remember: EVEN rounds away from zero:** This is different from CEILING. EVEN(-3) = -4, not -2.

2. **Use for pair requirements:** When items come in pairs (shoes, gloves, socks), EVEN gives the count needed for complete pairs.

3. **Combine with INT for different behavior:** If you want truncate-to-even, you'd need a custom formula, not EVEN.

4. **Zero is even:** EVEN(0) = 0. This is mathematically correct—zero is divisible by 2.

5. **For multiples of 2, consider CEILING:** `=CEILING(x, 2)` also rounds up to even for positive numbers, but handles negatives differently.

6. **EVEN of even is unchanged:** EVEN(4) = 4. If already even and integer, no change occurs.

7. **For "round to nearest even":** EVEN doesn't do this—it always rounds UP. For nearest-even (banker's rounding), you'd need a different approach.

8. **Pair with ODD:** If you need to alternate between even and odd outcomes, these functions complement each other.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ODD]] | Rounds to next odd integer away from zero | When you need odd numbers instead of even |
| [[CEILING]] | Rounds up to multiple | For multiples other than 2, or different negative handling |
| [[MROUND]] | Rounds to nearest multiple | When you want nearest (not always up) |
| [[INT]] | Rounds down to integer | When you need truncation, not even numbers |
| [[ROUNDUP]] | Rounds up by decimal places | When you need decimal precision, not even integers |

### Commonly Used Together

**[[ODD]]** - Complementary function

*Alternate between even and odd:*
```
=IF(ISEVEN(RowNumber), EVEN(Value), ODD(Value))
```
Choose even or odd based on context.

---

**[[ISEVEN]]** - Check if already even

*Conditional processing:*
```
=IF(ISEVEN(A1), A1, EVEN(A1))
```
Only apply EVEN if not already even.

---

**[[ABS]]** - Work with magnitude

*Understand the rounding:*
```
Before: ABS(-3.5) = 3.5
After EVEN: ABS(EVEN(-3.5)) = 4
```
EVEN increases absolute value.

---

**[[CEILING]]** - Alternative for multiples

*Compare behaviors:*
```
=EVEN(3)        → 4
=CEILING(3, 2)  → 4 (same for positive)
=EVEN(-3)       → -4
=CEILING(-3, 2) → -2 (different for negative!)
```

---

**[[MOD]]** - Check evenness

*Test result:*
```
=MOD(EVEN(A1), 2)    → Always 0 (confirming even)
```
Verify that results are indeed even.

## Official Documentation

- **Microsoft Excel:** [EVEN function](https://support.microsoft.com/en-us/office/even-function-197b5f06-c795-4c1e-8696-3c3b8a646cf9)
- **Google Sheets:** [EVEN function](https://support.google.com/docs/answer/3093510)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
