---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- weighted-sum
- array-calculation
- conditional-sum
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sum of Products
- Weighted Sum
- Array Multiplication Sum
tags:
- sumproduct
- array
- weighted-sum
- conditional
- matrix
---

# SUMPRODUCT

## Description

**SUMPRODUCT** multiplies corresponding elements across arrays (or ranges), then sums those products. Given two arrays {1,2,3} and {4,5,6}, SUMPRODUCT first multiplies element-by-element (1x4, 2x5, 3x6 = 4, 10, 18), then sums the results (4+10+18 = 32). This operation is mathematically known as the "dot product" or "inner product" of vectors—a fundamental concept in linear algebra with countless practical applications.

This function is a **Swiss Army knife for spreadsheet power users**. Beyond its mathematical purpose, SUMPRODUCT serves as a conditional aggregation engine, a weighted average calculator, and an array formula workaround. Before FILTER, XLOOKUP, and dynamic arrays existed, SUMPRODUCT was how experts accomplished complex multi-criteria calculations without Ctrl+Shift+Enter array formulas. It's still valuable for backward compatibility and cross-platform consistency.

**The key insight: Boolean conditions become 1s and 0s.** When you include a logical test like (A1:A10="Yes"), it creates an array of TRUE/FALSE values. In a numeric context, TRUE becomes 1 and FALSE becomes 0. Multiplying by 0 eliminates that element from the sum. This is how `=SUMPRODUCT((Region="East")*(Sales))` sums only East region sales—non-East rows multiply by 0 and vanish.

**Arrays must be the same size.** This is SUMPRODUCT's main constraint. All range arguments must have the same dimensions—same number of rows and columns. Mismatched sizes return #VALUE! error. Also note that text values in the arrays cause errors unless you're using the Boolean multiplication trick (which converts to numeric 0/1 first).

## Syntax

> [!f(x)] SUMPRODUCT Syntax
>
> ```
> =SUMPRODUCT(array1, [array2], [array3], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first array or range. Elements in this array are multiplied by corresponding elements in other arrays. |
| `array2, ...` | No | Additional arrays or ranges to multiply with array1. All arrays must have identical dimensions (same rows and columns). Up to 255 arrays supported. |

### Return Value

Returns a single number: the sum of the element-wise products of all input arrays. If only one array is provided, simply returns the sum of that array. Returns #VALUE! if arrays have different dimensions or contain non-numeric text (except in Boolean contexts).

## Examples

> [!f(x)] SUMPRODUCT Examples

### Example 1: Basic Two-Array Calculation
```
=SUMPRODUCT({1,2,3}, {4,5,6})
```
**Result:** 32

**Explanation:** Multiplies corresponding elements: (1x4) + (2x5) + (3x6) = 4 + 10 + 18 = 32. This is the mathematical dot product of two vectors.

---

### Example 2: Multiply Ranges and Sum
```
=SUMPRODUCT(A1:A5, B1:B5)
```
**Result:** Sum of (A1xB1 + A2xB2 + A3xB3 + A4xB4 + A5xB5)

**Explanation:** Each row's values are multiplied together, then all products are summed. Perfect for calculating total revenue from quantity and price columns.

---

### Example 3: Weighted Sum (Weighted Average Numerator)
```
=SUMPRODUCT(Scores, Weights)
```
**Result:** Sum of score x weight products

**Explanation:** For a weighted average, multiply each score by its weight, sum the products, then divide by sum of weights. This formula is the numerator of that calculation.

---

### Example 4: Complete Weighted Average
```
=SUMPRODUCT(Grades, Credits) / SUM(Credits)
```
**Result:** GPA (weighted average grade)

**Explanation:** Divides the weighted sum by total weights to get the weighted average. Common for GPA calculations, portfolio returns, and any weighted metric.

---

### Example 5: Single Condition Sum
```
=SUMPRODUCT((Region="East")*Sales)
```
**Result:** Total sales for East region

**Explanation:** (Region="East") creates {TRUE,FALSE,TRUE,...}. Multiplying by Sales converts TRUE to 1 (keeps value) and FALSE to 0 (eliminates value). Sum includes only East sales.

---

### Example 6: Multiple Conditions (AND Logic)
```
=SUMPRODUCT((Region="East")*(Year=2024)*Sales)
```
**Result:** Sales for East region in 2024

**Explanation:** Both conditions must be TRUE (1) for a row to count. If either is FALSE (0), the product is 0. This is equivalent to SUMIFS but works for more complex scenarios.

---

### Example 7: OR Logic with Addition
```
=SUMPRODUCT(((Region="East")+(Region="West"))*Sales)
```
**Result:** Sales for East OR West regions

**Explanation:** Addition creates OR logic: East=TRUE(1) + West=FALSE(0) = 1, or East=FALSE(0) + West=TRUE(1) = 1. Either satisfies the condition. Be careful: both TRUE would give 2, doubling that row.

---

### Example 8: Count with Conditions (COUNTIFS Alternative)
```
=SUMPRODUCT((Status="Complete")*(Amount>1000)*1)
```
**Result:** Count of complete transactions over $1000

**Explanation:** With no value column to sum, multiplying by 1 counts how many rows match all conditions. Each matching row contributes 1 to the sum.

---

### Example 9: Three-Way Product
```
=SUMPRODUCT(Quantity, UnitPrice, TaxRate)
```
**Result:** Total including tax across all items

**Explanation:** Multiplies three corresponding elements: Qty x Price x TaxRate for each row, then sums. Extends naturally to any number of arrays.

---

### Example 10: Using with Arrays in Formulas
```
=SUMPRODUCT(ABS(A1:A10 - AVERAGE(A1:A10)))
```
**Result:** Sum of absolute deviations from mean

**Explanation:** SUMPRODUCT evaluates the inner formula as an array, applies ABS to each element, then sums. This calculates total absolute deviation without needing Ctrl+Shift+Enter.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Arrays have different dimensions | Ensure all ranges have the same number of rows and columns. A1:A10 and B1:B5 cannot be used together. |
| `#VALUE!` | Text in non-condition context | Non-numeric text in arrays causes errors. Use Boolean conditions that convert to 0/1, or clean data first. |
| `#NAME?` | Misspelled function name | Verify spelling: SUMPRODUCT, not SUMPRODUCTS or SUM_PRODUCT. |
| `Unexpected high value` | OR logic doubling | When using addition for OR, rows matching multiple conditions are counted multiple times. Use MAX or restructure logic. |
| `Zero result` | All conditions FALSE | If no rows match your criteria, result is 0. Verify conditions are correct and data exists. |
| `#REF!` | Invalid range reference | Check that referenced ranges exist and are properly formatted. |

## Use Cases

### [[Conditional Summation with Multiple Criteria]]

**Scenario:** Sum values based on multiple conditions, similar to SUMIFS but with more flexibility.

**Implementation:**
```
=SUMPRODUCT((Year=2024)*(Department="Sales")*Revenue)
=SUMPRODUCT((Date>=StartDate)*(Date<=EndDate)*Amount)
=SUMPRODUCT((Category=TargetCategory)*(Qty>MinQty)*Total)
```

**Business Application:** Financial reporting, sales analysis, inventory management. Any scenario requiring filtered aggregation with multiple criteria.

**Technical Details:** Conditions use multiplication for AND logic. Each condition creates a 0/1 array; multiplying them together gives 1 only where all conditions are TRUE. The final multiplication with the value column sums only matching rows.

---

### [[Weighted Calculations and Scoring]]

**Scenario:** Calculate weighted averages, composite scores, or blended metrics.

**Implementation:**
```
=SUMPRODUCT(TestScores, TestWeights) / SUM(TestWeights)
=SUMPRODUCT(Ratings, Importance) / SUM(Importance)
=SUMPRODUCT(Returns, PortfolioWeights)
```

**Business Application:** Academic grading, employee performance reviews, investment portfolio analysis, product scoring models. Any weighted average or composite index.

**Technical Details:** The pattern is always SUMPRODUCT(values, weights) / SUM(weights). If weights don't sum to 1 (or 100%), the division normalizes the result. For portfolio returns where weights already sum to 1, skip the division.

---

### [[Array Formulas Without Ctrl+Shift+Enter]]

**Scenario:** Perform array calculations in older Excel versions or for compatibility.

**Implementation:**
```
=SUMPRODUCT(LEN(A1:A100))                    → Sum of all text lengths
=SUMPRODUCT((A1:A100>0)/COUNTIF(A1:A100,">0"))  → Average of positive values
=SUMPRODUCT(MAX((A1:A10)*(B1:B10)))           → Max of element-wise products
```

**Business Application:** Complex calculations that would otherwise require array formulas. Maintains compatibility with older Excel versions and Google Sheets.

**Technical Details:** SUMPRODUCT automatically evaluates its arguments as arrays. This was invaluable before dynamic arrays. For non-summation results (like MAX inside SUMPRODUCT), the result applies to the summed array.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 4.0
- **Array limit:** Up to 255 arrays
- **Size limit:** Arrays can have up to 65,536 rows (Excel 2007+)
- **Dynamic arrays:** In Excel 365, works seamlessly with spilled ranges
- **Performance:** Optimized but can slow with very large ranges and many conditions

### Google Sheets
- **Availability:** All versions
- **Array limit:** Similar to Excel
- **Size limit:** Up to 10 million cells total in a spreadsheet
- **ARRAYFORMULA:** Not needed for SUMPRODUCT—it handles arrays natively
- **Performance:** Comparable to Excel for typical usage

### Both Platforms
- Identical syntax and behavior for standard use cases
- Same Boolean multiplication trick works identically
- Same requirement for equal-sized arrays
- Same error conditions

## Tips and Best Practices

1. **Use double negation for cleaner conditions:** `--` converts TRUE/FALSE to 1/0 explicitly. `=SUMPRODUCT(--(A:A="X"), B:B)` is equivalent to `=SUMPRODUCT((A:A="X")*B:B)` but some find it clearer.

2. **Avoid entire column references:** `SUMPRODUCT(A:A, B:B)` can be extremely slow because it processes a million+ cells. Use specific ranges like A1:A1000.

3. **OR logic needs care:** Addition for OR (condition1 + condition2) can double-count rows matching both. For exclusive OR, use `=SUMPRODUCT(((A="X")+(B="Y")>0)*C)` to cap at 1.

4. **Consider SUMIFS for simple cases:** For basic multi-criteria sums, SUMIFS is faster and clearer. Reserve SUMPRODUCT for complex calculations SUMIFS can't handle.

5. **Parentheses matter:** Each condition needs its own parentheses: `(A="X")*(B>0)` not `A="X"*B>0`. The latter gives unexpected results due to operator precedence.

6. **Use for weighted averages:** The pattern `=SUMPRODUCT(Values, Weights)/SUM(Weights)` is the cleanest way to calculate weighted averages in spreadsheets.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SUMIFS]] | Sum with multiple conditions | When conditions are simple equality/comparison and performance matters |
| [[MMULT]] | Matrix multiplication | For true matrix math (result is an array, not a single value) |
| [[SUM]] | Simple sum | When you just need to add values without multiplication or conditions |
| [[PRODUCT]] | Multiply all values | When you need the product of a range, not sum of products |

### Commonly Used Together

**[[SUM]]** - Complete the weighted average

*Weighted average formula:*
```
=SUMPRODUCT(Values, Weights) / SUM(Weights)
```
SUMPRODUCT gives weighted sum; SUM normalizes by total weight.

---

**[[IF]]** - Conditional logic in arrays

*Complex conditions:*
```
=SUMPRODUCT(IF(Category="A", Qty*Price, 0))
```
Though Boolean multiplication is usually cleaner for SUMPRODUCT.

---

**[[ABS]]** - Absolute value in arrays

*Sum of absolute deviations:*
```
=SUMPRODUCT(ABS(Actual - Forecast))
```
Total absolute error without array formula.

---

**[[LEN]]** - Text length calculations

*Total characters in range:*
```
=SUMPRODUCT(LEN(TextRange))
```
Sum of character counts across all cells.

---

**[[ISNUMBER]]** - Filter numeric values

*Sum only numeric cells:*
```
=SUMPRODUCT(ISNUMBER(A1:A100)*A1:A100)
```
Excludes text and errors from sum.

## Official Documentation

- **Microsoft Excel:** [SUMPRODUCT function](https://support.microsoft.com/en-us/office/sumproduct-function-16753e75-9f68-4874-94ac-4d2145a2fd2e)
- **Google Sheets:** [SUMPRODUCT function](https://support.google.com/docs/answer/3094294)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 4.0 (1992) | Original function |
| Excel 2007+ | All subsequent | Increased row limit |
| Excel 365 | Dynamic arrays | Works with spilled ranges |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
