---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - lambda-functions
  - aggregation
subTopics:
  - accumulation
  - functional-programming
  - custom-aggregation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - reduce function
  - accumulator
  - fold function
tags:
  - google-sheets-only
  - lambda
  - arrays
  - aggregation
  - functional
---

# REDUCE

## Description

**REDUCE** is a powerful lambda-based function in Google Sheets that processes an array by applying a custom accumulator function to each element, carrying forward a running result that eventually produces a single final value. Think of it as a customizable aggregation engine: while SUM always adds and MAX always finds the maximum, REDUCE lets you define any accumulation logic. You provide an initial value, an array to process, and a LAMBDA that specifies how to combine the accumulated result with each new element. REDUCE iterates through the array element by element, updating the accumulator with each step, and returns the final accumulated value. This makes it possible to create custom aggregations that are not built into spreadsheet functions.

The primary use cases for REDUCE include custom aggregation formulas (like product of values, running comparisons, or conditional accumulation), building complex values step by step (like concatenating with custom logic), and implementing algorithms that require tracking state while processing data. For example, calculating the product of all numbers in a range is not a built-in function, but with REDUCE it is simple: `=REDUCE(1, A1:A10, LAMBDA(acc, val, acc*val))`. REDUCE brings the functional programming "fold" or "reduce" pattern to spreadsheets, enabling sophisticated data processing that would otherwise require helper columns, iterative VBA, or complex nested formulas.

There are several important gotchas when working with REDUCE. First, the initial value matters significantly; for multiplication you need 1, for addition you need 0, for string concatenation you need "". Second, the LAMBDA must accept exactly two parameters: the accumulator (running result) and the current element. Third, REDUCE processes elements in order (row by row, left to right within rows), which matters for order-dependent operations. Fourth, unlike SUM or AVERAGE which ignore empty cells, REDUCE passes empty cells to your LAMBDA, so you must handle them explicitly. Fifth, REDUCE always returns a single value, not an array; if you need intermediate results, use SCAN instead.

From a platform perspective, REDUCE is available in both Google Sheets and Microsoft Excel 365. The syntax and functionality are identical between platforms. REDUCE was introduced as part of the LAMBDA function family in 2022. For users of older Excel versions without LAMBDA support, REDUCE is not available, and custom aggregations must be done using helper columns, VBA, or creative combinations of built-in functions. When building cross-platform spreadsheets, REDUCE formulas work seamlessly in both modern Excel and Google Sheets.

## Syntax

> [!f(x)] REDUCE Syntax
>
> ```
> =REDUCE(initial_value, array, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `initial_value` | Yes | The starting value for the accumulator before processing any array elements. This value is passed as the first accumulator value to the LAMBDA. Type and value depend on your aggregation: 0 for sums, 1 for products, "" for strings, etc. |
| `array` | Yes | The array or range to process. Elements are passed to the LAMBDA one at a time in row-major order (left to right, top to bottom). Can be a range reference, named range, or array from another function. |
| `lambda` | Yes | A LAMBDA function with exactly two parameters: the first receives the current accumulator value, the second receives the current array element. The LAMBDA returns the new accumulator value. Syntax: `LAMBDA(accumulator, element, expression)`. |

### Return Value

Returns a single value: the final accumulator value after processing all elements in the array. The type depends on what your LAMBDA returns; it could be a number, text, boolean, or any other value type.

## Examples

> [!f(x)] REDUCE Examples

### Example 1: Product of All Values
```
=REDUCE(1, A1:A10, LAMBDA(acc, val, acc * val))
```
**Result:** The product of all values in A1:A10

**Explanation:** Starting with 1, each value is multiplied into the accumulator. This creates a PRODUCT function using REDUCE. The initial value of 1 is crucial; 0 would make the result always 0.

---

### Example 2: Custom Sum (Demonstrative)
```
=REDUCE(0, A1:A10, LAMBDA(total, num, total + num))
```
**Result:** The sum of all values (equivalent to SUM)

**Explanation:** Starting with 0, each value is added. This demonstrates REDUCE logic; in practice, use SUM for simple addition as it is faster.

---

### Example 3: Count of Values Greater Than Threshold
```
=REDUCE(0, A1:A50, LAMBDA(count, val, count + IF(val > 100, 1, 0)))
```
**Result:** Count of values exceeding 100

**Explanation:** Accumulator starts at 0. For each value over 100, add 1 to the count. This is a custom COUNTIF implementation.

---

### Example 4: Maximum Value (Demonstrative)
```
=REDUCE(-9999999, A1:A100, LAMBDA(max, val, IF(val > max, val, max)))
```
**Result:** The maximum value in the range

**Explanation:** Starting with a very small number, each value is compared; if larger, it becomes the new max. Demonstrates conditional accumulation.

---

### Example 5: Concatenate All Text with Delimiter
```
=REDUCE("", A1:A10, LAMBDA(acc, cell, IF(acc="", cell, acc & ", " & cell)))
```
**Result:** All values joined by ", " (e.g., "Apple, Banana, Cherry")

**Explanation:** Starts with empty string. First value becomes the accumulator; subsequent values are appended with comma separator. Handles first element without leading comma.

---

### Example 6: Sum Only Positive Values
```
=REDUCE(0, A1:A50, LAMBDA(sum, val, sum + IF(val > 0, val, 0)))
```
**Result:** Sum of only positive values

**Explanation:** Only adds positive values to the accumulator; negative values add 0. Creates a conditional sum.

---

### Example 7: Count Unique Values (Approximation)
```
=REDUCE("", A1:A100, LAMBDA(seen, val, IF(ISERROR(FIND("|"&val&"|", seen)), seen&"|"&val&"|", seen)))
```
**Result:** A string containing unique values, which can be parsed for count

**Explanation:** Builds a string of unique values separated by |. Checks if value already exists before adding. More of a demonstration; UNIQUE function is better for this.

---

### Example 8: Running AND (All Values TRUE)
```
=REDUCE(TRUE, A1:A20, LAMBDA(all, val, AND(all, val)))
```
**Result:** TRUE only if all values are TRUE

**Explanation:** Starts with TRUE. ANDs with each value. If any value is FALSE, result becomes FALSE permanently.

---

### Example 9: Running OR (Any Value TRUE)
```
=REDUCE(FALSE, A1:A20, LAMBDA(any, val, OR(any, val)))
```
**Result:** TRUE if any value is TRUE

**Explanation:** Starts with FALSE. ORs with each value. Once TRUE is found, result stays TRUE.

---

### Example 10: Calculate Factorial
```
=REDUCE(1, SEQUENCE(A1), LAMBDA(fact, n, fact * n))
```
**Result:** Factorial of the number in A1 (e.g., 5! = 120)

**Explanation:** SEQUENCE generates 1 to A1. REDUCE multiplies them all together. Starting value of 1 ensures correct result.

---

### Example 11: Nested Sum of Absolute Values
```
=REDUCE(0, A1:C10, LAMBDA(sum, val, sum + ABS(val)))
```
**Result:** Sum of absolute values across the entire range

**Explanation:** ABS converts each value to positive before adding. Works on multi-column ranges, processing row by row.

---

### Example 12: Find First Non-Empty Value
```
=REDUCE("", A1:A50, LAMBDA(first, val, IF(AND(first="", val<>""), val, first)))
```
**Result:** The first non-empty value in the range

**Explanation:** Once a non-empty value is found and stored, the accumulator stops updating. Returns first non-empty cell.

---

### Example 13: Sum with Running Maximum Cap
```
=REDUCE(0, A1:A20, LAMBDA(sum, val, MIN(sum + val, 1000)))
```
**Result:** Sum capped at 1000

**Explanation:** Adds each value but caps the result at 1000. Once ceiling is reached, sum cannot increase further.

---

### Example 14: Weighted Sum
```
=REDUCE(0, SEQUENCE(ROWS(A1:A10)), LAMBDA(sum, i, sum + INDEX(A1:A10, i) * INDEX(B1:B10, i)))
```
**Result:** Sum of (value * weight) pairs from columns A and B

**Explanation:** Uses SEQUENCE to iterate by index, then accesses both arrays by position. Alternative to SUMPRODUCT with more control.

---

### Example 15: Build JSON-Like String
```
=REDUCE("{", A1:B5, LAMBDA(json, pair, json & """" & INDEX(pair, 1) & """: """ & INDEX(pair, 2) & """, "))
```
**Result:** A JSON-like string built from key-value pairs

**Explanation:** Starts with opening brace, adds each key-value pair in JSON format. Demonstrates building complex strings element by element.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | LAMBDA does not have exactly two parameters | REDUCE LAMBDA must accept accumulator and element parameters. Check for correct parameter count. |
| `#NAME?` | LAMBDA syntax is incorrect or parameter names are invalid | Verify LAMBDA structure. Parameter names cannot be cell references or reserved words. |
| `#VALUE!` | Incompatible types in accumulation (e.g., adding string to number) | Ensure your LAMBDA handles all possible value types in the array, including empty cells and errors. |
| `#ERROR!` | Array is empty | Provide a valid array with at least one element, or handle empty arrays with IFERROR. |
| Wrong result | Initial value is incorrect for the operation | Use 0 for sums, 1 for products, "" for concatenation, TRUE for AND, FALSE for OR. |
| Unexpected behavior | Empty cells processed as 0 or "" | Add explicit empty cell handling: `IF(val="", acc, acc+val)` |
| Performance issues | Very large arrays with complex LAMBDA | Simplify LAMBDA logic or reduce array size. REDUCE processes sequentially. |

## Use Cases

### [[Financial Analysis: Compound Interest Calculation]]

**Scenario:** A financial analyst needs to calculate compound growth over multiple periods with varying interest rates each period, which cannot be done with simple FV function.

**Implementation:**
```
=REDUCE(InitialInvestment, InterestRates, LAMBDA(balance, rate, balance * (1 + rate)))
```
Where InitialInvestment is a cell with starting amount and InterestRates is a column of period rates

**Business Application:** Calculates ending balance after applying different interest rates each period. Handles variable-rate investments, adjustable-rate loans, or multi-period investment simulations. Each period's rate is applied to the growing balance.

**Technical Details:** Starting with the initial investment, each rate is applied to the current balance. The result compounds naturally through the reduction. This pattern extends to any multiplicative growth scenario.

---

### [[Inventory Management: Running Stock Calculation]]

**Scenario:** A warehouse manager needs to calculate current stock level by processing a sequence of receipts (positive) and shipments (negative), stopping if stock ever goes negative.

**Implementation:**
```
=REDUCE(InitialStock, Transactions, LAMBDA(stock, txn,
  LET(newStock, stock + txn, IF(newStock < 0, "STOCKOUT at "&stock, newStock))))
```

**Business Application:** Tracks stock level through transactions while detecting stockout conditions. Unlike simple SUM, this can identify when stock would go negative and flag the error. Useful for validating transaction sequences.

**Technical Details:** Each transaction adjusts the running stock. If stock goes negative, the accumulator becomes an error message including the last valid stock level. This stateful processing is difficult without REDUCE.

---

### [[Text Processing: Custom Concatenation with Logic]]

**Scenario:** A data team needs to build a formatted string from multiple fields, but only including non-empty values and applying different formatting based on field type.

**Implementation:**
```
=REDUCE("", DataRow, LAMBDA(result, field,
  IF(field = "", result,
    IF(result = "", field,
      result & " | " & field))))
```

**Business Application:** Creates clean, formatted output strings that skip empty fields. Avoids ugly "| | |" patterns when data has gaps. Produces professional-looking concatenated displays.

**Technical Details:** Empty fields are skipped entirely. Non-empty fields are added with separator, except the first field which has no leading separator. This pattern handles variable-length data gracefully.

---

### [[Quality Control: Cumulative Defect Rate]]

**Scenario:** A quality engineer tracks defect counts over production batches and needs to calculate various cumulative metrics: total defects, running defect rate, and defect density.

**Implementation:**
```
=REDUCE({0,0}, HSTACK(Defects, BatchSizes), LAMBDA(acc, row,
  LET(
    totalDefects, INDEX(acc,1) + INDEX(row,1),
    totalUnits, INDEX(acc,2) + INDEX(row,2),
    HSTACK(totalDefects, totalUnits)
  )))
```

**Business Application:** Tracks both cumulative defects and cumulative units to calculate overall defect rate. The compound accumulator maintains multiple running totals simultaneously. Final rate is total defects / total units.

**Technical Details:** The accumulator is an array with two values (defects, units). Each iteration updates both. This demonstrates tracking multiple values through REDUCE using array accumulators.

## Platform Differences

### Microsoft Excel

- **Availability:** REDUCE is available in Excel 365 (Microsoft 365 subscription versions)
- **Syntax:** Identical to Google Sheets
- **Behavior:** Same functionality; processes array with accumulator
- **Not available in:** Excel 2019, Excel 2016, Excel 2021, or earlier versions
- **Alternative for older Excel:** VBA loops, helper columns, or creative use of aggregate functions

### Google Sheets

- **Availability:** Available in all Google Sheets (introduced 2022)
- **Specific Behavior:** This documentation focuses on Google Sheets behavior
- **Processing order:** Row-major order (left to right, top to bottom)
- **Integration:** Works with all Google Sheets functions including QUERY and other LAMBDA functions

### Key Differences

| Feature | Google Sheets | Excel 365 |
|---------|---------------|-----------|
| Availability | All users | Microsoft 365 subscribers only |
| Syntax | Identical | Identical |
| Processing order | Row-major | Row-major |
| Performance | Good | Generally slightly better |
| Empty cell handling | Passed to LAMBDA | Passed to LAMBDA |
| Initial value types | Any | Any |

## Tips and Best Practices

1. **Choose Initial Value Carefully:** The initial value determines the starting point of accumulation. Use 0 for sums, 1 for products, "" for strings, TRUE for AND operations, FALSE for OR operations.

2. **Handle Empty Cells:** Unlike SUM or AVERAGE, REDUCE passes empty cells to your LAMBDA. Add checks like `IF(val="", acc, ...)` to handle empties appropriately.

3. **Name Parameters Descriptively:** Use meaningful names like `total`, `product`, `result` for the accumulator and `item`, `value`, `element` for the current value.

4. **Use SCAN for Debugging:** If your REDUCE produces unexpected results, try SCAN with the same logic to see all intermediate values and identify where things go wrong.

5. **Consider Built-in Functions First:** For standard operations (sum, product, max, min), built-in functions are faster. Use REDUCE for custom logic that cannot be expressed otherwise.

6. **LET Simplifies Complex Logic:** When your LAMBDA is complex, use LET inside it to name intermediate calculations and improve readability.

7. **Array Accumulators for Multiple Values:** The accumulator can be an array like `{total, count}` to track multiple running values simultaneously.

8. **Order Matters for Some Operations:** Remember that REDUCE processes left-to-right, top-to-bottom. For order-dependent operations, ensure your data is arranged correctly.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SCAN]] | Like REDUCE but returns all intermediate results | When you need to see the running accumulator values, not just the final result |
| [[SUM]] / [[PRODUCT]] / [[MAX]] / [[MIN]] | Built-in aggregation functions | For standard aggregations; faster and simpler than REDUCE |
| [[SUMPRODUCT]] | Sum of element-wise products | For weighted sums or conditional sums where this pattern fits |
| [[AGGREGATE]] | Aggregation with options for ignoring errors/hidden | When you need standard aggregation with error handling |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the accumulation function

*REDUCE requires LAMBDA to define accumulation logic:*
```
=REDUCE(0, A1:A100, LAMBDA(acc, val, acc + val))
```
The LAMBDA specifies how to combine accumulator with each element.

---

**[[LET]]** - Structure complex accumulation logic

*Use LET within LAMBDA for clarity:*
```
=REDUCE(0, A1:A100, LAMBDA(acc, val,
  LET(cleaned, IF(ISNUMBER(val), val, 0), acc + cleaned)))
```
Names intermediate values for readability.

---

**[[SEQUENCE]]** - Generate input for iteration

*Create a sequence to reduce over:*
```
=REDUCE(1, SEQUENCE(10), LAMBDA(fact, n, fact * n))
```
Calculates 10! (factorial) by reducing over 1-10.

---

**[[IF]]** - Conditional accumulation

*Apply different logic based on values:*
```
=REDUCE(0, A1:A50, LAMBDA(sum, val, sum + IF(val > 0, val, 0)))
```
Only accumulates values meeting a condition.

---

**[[SCAN]]** - See intermediate results

*Debug or display running values:*
```
=SCAN(0, A1:A10, LAMBDA(acc, val, acc + val))
```
Shows the running total at each step, not just the final result.

---

**[[INDEX]]** - Access array elements by position

*Work with parallel arrays:*
```
=REDUCE(0, SEQUENCE(10), LAMBDA(sum, i, sum + INDEX(Values,i) * INDEX(Weights,i)))
```
Access corresponding elements from multiple arrays during reduction.

## Official Documentation

- **Google Sheets:** [REDUCE function](https://support.google.com/docs/answer/12570919)
- **Microsoft Excel:** [REDUCE function](https://support.microsoft.com/en-us/office/reduce-function-42e39910-b345-45f3-84b8-0642b568b7cb)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2022 | Introduced as part of the LAMBDA helper function family |
| Excel 365 | 2022 | Added with LAMBDA function ecosystem |
| Excel 2021 | N/A | Not available in perpetual license version |
| Excel Online | 2022 | Available in web version |

---

*Last updated: 2026-01-10*
