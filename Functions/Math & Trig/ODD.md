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
- odd
- integers
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Round to Odd Integer
- Next Odd Number
tags:
- rounding
- odd
- integers
- round-up
- away-from-zero
---

# ODD

## Description

**ODD** rounds a number UP to the nearest odd integer, always rounding **away from zero**. ODD(2) returns 3, ODD(2.1) returns 3, ODD(-2) returns -3 (not -1). Like EVEN, ODD consistently rounds away from zero for both positive and negative numbers, ensuring the result's magnitude is always >= the input's magnitude.

**The "away from zero" behavior is crucial:** For positive numbers, ODD rounds up (toward positive infinity). For negative numbers, ODD rounds "up" in magnitude (toward negative infinity, away from zero). This makes ODD predictable and symmetric with EVEN.

**Why use ODD?** Odd numbers appear in specific contexts: certain grid layouts require odd dimensions for a center element, some algorithms need odd-sized arrays, odd/even page layouts differ, and some counting/grouping scenarios specifically need odd quantities. ODD ensures you get the next odd number when needed.

**Relationship to EVEN:** ODD and EVEN are complementary functions. They both round away from zero to the next integer of their respective parity. Every integer is either odd or even, so between these two functions, you can control integer rounding based on parity requirements.

## Syntax

> [!f(x)] ODD Syntax
>
> ```
> =ODD(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value to round to the next odd integer. Can be positive, negative, or zero. Decimals are rounded up in magnitude to the next odd integer. |

### Return Value

Returns the smallest odd integer whose absolute value is >= the absolute value of the input. Always rounds away from zero. ODD(0) returns 1. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] ODD Examples

### Example 1: Even Integer to Odd
```
=ODD(2)
```
**Result:** 3

**Explanation:** 2 is even, so round up to the next odd integer: 3.

---

### Example 2: Already Odd
```
=ODD(3)
```
**Result:** 3

**Explanation:** 3 is already odd. ODD returns it unchanged.

---

### Example 3: Decimal to Odd
```
=ODD(3.1)
```
**Result:** 5

**Explanation:** 3.1 is greater than 3 (odd), so it rounds up to the next odd: 5.

---

### Example 4: Small Decimal
```
=ODD(0.5)
```
**Result:** 1

**Explanation:** 0.5 rounds up away from zero to the first positive odd integer: 1.

---

### Example 5: Zero
```
=ODD(0)
```
**Result:** 1

**Explanation:** Zero rounds up (away from zero) to the first positive odd integer: 1. Note: This is different from EVEN(0) = 0.

---

### Example 6: Negative Even
```
=ODD(-2)
```
**Result:** -3

**Explanation:** -2 is even. "Up" in magnitude (away from zero) means -3, not -1. This is key ODD behavior.

---

### Example 7: Negative Odd
```
=ODD(-3)
```
**Result:** -3

**Explanation:** -3 is already odd. ODD returns it unchanged.

---

### Example 8: Negative Decimal
```
=ODD(-2.5)
```
**Result:** -3

**Explanation:** -2.5 rounds away from zero to -3 (the odd integer further from zero than -2).

---

### Example 9: Large Number
```
=ODD(100)
```
**Result:** 101

**Explanation:** 100 is even. The next odd integer is 101.

---

### Example 10: Very Small Positive
```
=ODD(0.001)
```
**Result:** 1

**Explanation:** Any positive non-zero number rounds up to at least 1 (the first positive odd integer).

---

### Example 11: Compare to EVEN
```
=ODD(2.5)   → 3
=EVEN(2.5)  → 4
```
**Result:** ODD gives 3, EVEN gives 4

**Explanation:** ODD and EVEN round to their respective parities, always away from zero.

---

### Example 12: Grid Dimension for Center Element
```
=ODD(ColumnCount)
```
**Result:** Odd number of columns ensuring a center column exists

**Explanation:** A grid needs odd dimensions to have a true center element. ODD ensures this.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text values cause this error. |
| `#NAME?` | Misspelled function name | Check spelling: ODD, not ODDD. |
| `Expected -1, got -3` | Misunderstanding direction | ODD rounds AWAY from zero for negatives. -2 becomes -3, not -1. |
| `Expected 0, got 1` | ODD(0) behavior | ODD(0) = 1, not 0. Zero isn't odd, so it rounds to the first odd (1). |

## Use Cases

### [[Grid Layouts Requiring Center Element]]

**Scenario:** Create grids where a center row or column is needed.

**Implementation:**
```
=ODD(ColumnCount)                         → Ensure center column exists
=ODD(RowCount)                            → Ensure center row exists
=ODD(ArraySize)                           → Odd-sized array for centering
```

**Business Application:** Calendar layouts, grid-based games (tic-tac-toe), form layouts with center column, seating charts with center aisle.

**Technical Details:** An odd-dimensioned grid has a true center (element at position (n+1)/2). Even-dimensioned grids have a center gap, not a center element.

---

### [[Page Layout and Printing]]

**Scenario:** Handle odd/even page numbering and layout.

**Implementation:**
```
=ODD(PageCount)                           → Ensure odd total for booklet
=IF(ISODD(PageNumber), ODD_MARGIN, EVEN_MARGIN)  → Alternating margins
=ODD(SectionPages)                        → Odd pages per section
```

**Business Application:** Book layout, booklet printing (odd total pages for proper binding), alternating headers/footers.

**Technical Details:** Booklets often need odd total page counts. ODD ensures sections end on odd numbers when required.

---

### [[Algorithm and Data Structure Requirements]]

**Scenario:** Ensure data structures meet odd-size requirements.

**Implementation:**
```
=ODD(WindowSize)                          → Odd window for median filters
=ODD(KernelSize)                          → Odd kernel for image convolution
=ODD(SampleCount)                         → Odd samples for center-based analysis
```

**Business Application:** Statistical calculations, image processing, signal analysis.

**Technical Details:** Many algorithms require odd-sized windows/kernels for a definite center element. Median filters, for example, work best with odd window sizes.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Behavior:** Rounds away from zero for both positive and negative numbers
- **Zero:** Returns 1 (not 0, since 0 isn't odd)
- **Precision:** Works with full double precision

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel—rounds away from zero
- **Zero:** Returns 1
- **Precision:** Same as Excel

### Both Platforms
- Identical mathematical behavior
- Same treatment of zero (returns 1)
- Same treatment of negative numbers
- No functional differences between platforms

## Tips and Best Practices

1. **Remember: ODD rounds away from zero:** ODD(-2) = -3, not -1. This is parallel to EVEN's behavior.

2. **ODD(0) = 1, not 0:** Zero is not odd, so it rounds to the first odd integer. This differs from EVEN(0) = 0.

3. **Use for center-requiring grids:** When you need a definite center (row, column, element), ensure odd dimensions with ODD.

4. **Pair with ISODD for checking:** Use ISODD to check if a value is already odd before applying ODD.

5. **ODD and EVEN are complementary:** They cover all parity-based rounding needs. Every integer is odd or even.

6. **ODD of odd is unchanged:** ODD(5) = 5. If already odd and integer, no change occurs.

7. **For "round to nearest odd":** ODD doesn't do this—it always rounds away from zero. "Nearest odd" would require custom logic.

8. **Consider for kernel sizes:** Image processing kernels (3x3, 5x5, etc.) need odd dimensions. ODD ensures this.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[EVEN]] | Rounds to next even integer away from zero | When you need even numbers instead of odd |
| [[CEILING]] | Rounds up to multiple | For arbitrary multiples, not just odd integers |
| [[MROUND]] | Rounds to nearest multiple | When you want nearest, not always away from zero |
| [[INT]] | Rounds down to integer | When you need truncation without parity concern |
| [[ROUNDUP]] | Rounds up by decimal places | When you need decimal precision |

### Commonly Used Together

**[[EVEN]]** - Complementary function

*Choose based on parity:*
```
=IF(condition, ODD(Value), EVEN(Value))
```
Round to odd or even based on criteria.

---

**[[ISODD]]** - Check if already odd

*Conditional processing:*
```
=IF(ISODD(A1), A1, ODD(A1))
```
Only apply ODD if not already odd.

---

**[[INT]]** - Create odd from any integer

*Alternative approach:*
```
=INT(A1) + 1 - MOD(INT(A1) + 1, 2)
```
A way to get the nearest odd without away-from-zero behavior (though more complex).

---

**[[MOD]]** - Check oddness

*Verify result:*
```
=MOD(ODD(A1), 2)    → Always 1 (confirming odd)
```
Confirm that results are indeed odd.

---

**[[ABS]]** - Work with magnitude

*Understand the rounding:*
```
Before: ABS(-2.5) = 2.5
After ODD: ABS(ODD(-2.5)) = 3
```
ODD increases absolute value to the next odd magnitude.

## Official Documentation

- **Microsoft Excel:** [ODD function](https://support.microsoft.com/en-us/office/odd-function-deae64eb-e08a-4c88-8b40-6d0b42575c98)
- **Google Sheets:** [ODD function](https://support.google.com/docs/answer/3093507)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
