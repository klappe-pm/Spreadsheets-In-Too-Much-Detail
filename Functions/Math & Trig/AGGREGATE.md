---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- math-trig
subTopics:
- aggregate-functions
- error-handling
- data-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Aggregate Function
- Error-Ignoring Calculations
- Flexible Aggregation
tags:
- aggregation
- statistics
- error-handling
- hidden-rows
- excel-only
---

# AGGREGATE

## Description

**AGGREGATE** is Excel's Swiss Army knife for calculations—a single function that can perform 19 different operations (AVERAGE, COUNT, MAX, MIN, SUM, and more) while selectively ignoring errors, hidden rows, or nested AGGREGATE/SUBTOTAL results. Think of it as SUBTOTAL's more powerful cousin that adds error-handling superpowers and additional statistical functions.

The function's true value emerges when working with messy, real-world data. Standard functions like SUM or AVERAGE fail entirely if they encounter a single #DIV/0!, #N/A, or #VALUE! error in the range. AGGREGATE can skip right over those errors and calculate with the valid values. Similarly, when you've filtered data or hidden rows manually, AGGREGATE can optionally ignore those hidden values—critical for accurate reporting on filtered datasets.

**The complexity trade-off:** AGGREGATE's power comes at the cost of readability. The function uses numeric codes (1-19) for operations and (0-7) for options, making formulas cryptic: `=AGGREGATE(9, 6, A1:A100)` is a SUM that ignores errors, but you'd never guess that from reading it. Many users prefer writing `=SUMIF` with error checking or using helper columns, sacrificing flexibility for clarity.

**Excel-only function:** AGGREGATE does not exist in Google Sheets. For Sheets users, you'll need to use alternatives like SUMIF combined with ISERROR, or QUERY functions with WHERE clauses. This makes AGGREGATE problematic for cross-platform workbooks—consider using more universal approaches if your files need to work in both applications.

## Syntax

> [!f(x)] AGGREGATE Syntax
>
> ```
> =AGGREGATE(function_num, options, ref1, [ref2], ...)
> ```
>
> Or for functions that require k (like LARGE, SMALL, PERCENTILE):
>
> ```
> =AGGREGATE(function_num, options, array, k)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `function_num` | Yes | A number 1-19 specifying which function to perform. See function table below. |
| `options` | Yes | A number 0-7 specifying what to ignore. See options table below. |
| `ref1` | Yes | The first numeric argument for the function. For reference form, this is the range. For array form, this is the array. |
| `ref2, ...` | No | Additional ranges for reference form functions (up to 253 additional arguments). |
| `k` | Conditional | Required for LARGE, SMALL, PERCENTILE.INC, QUARTILE.INC, PERCENTILE.EXC, QUARTILE.EXC. Specifies which value to return. |

### Function Numbers (function_num)

| Number | Function | Description |
|--------|----------|-------------|
| 1 | AVERAGE | Arithmetic mean |
| 2 | COUNT | Count of numbers |
| 3 | COUNTA | Count of non-empty cells |
| 4 | MAX | Maximum value |
| 5 | MIN | Minimum value |
| 6 | PRODUCT | Product of all values |
| 7 | STDEV.S | Sample standard deviation |
| 8 | STDEV.P | Population standard deviation |
| 9 | SUM | Sum of values |
| 10 | VAR.S | Sample variance |
| 11 | VAR.P | Population variance |
| 12 | MEDIAN | Median value |
| 13 | MODE.SNGL | Most frequent value |
| 14 | LARGE | K-th largest value |
| 15 | SMALL | K-th smallest value |
| 16 | PERCENTILE.INC | Percentile (inclusive) |
| 17 | QUARTILE.INC | Quartile (inclusive) |
| 18 | PERCENTILE.EXC | Percentile (exclusive) |
| 19 | QUARTILE.EXC | Quartile (exclusive) |

### Options (options parameter)

| Number | Behavior |
|--------|----------|
| 0 | Ignore nested SUBTOTAL and AGGREGATE functions |
| 1 | Ignore hidden rows, nested SUBTOTAL and AGGREGATE |
| 2 | Ignore error values, nested SUBTOTAL and AGGREGATE |
| 3 | Ignore hidden rows, error values, nested SUBTOTAL and AGGREGATE |
| 4 | Ignore nothing |
| 5 | Ignore hidden rows |
| 6 | Ignore error values |
| 7 | Ignore hidden rows and error values |

### Return Value

Returns the result of the specified function applied to the range/array, with specified values ignored. Returns #VALUE! if function_num or options are invalid.

## Examples

> [!f(x)] AGGREGATE Examples

### Example 1: SUM Ignoring Errors
```
=AGGREGATE(9, 6, A1:A10)
```
Where A1:A10 contains {10, 20, #DIV/0!, 30, #N/A, 40, 50, #VALUE!, 60, 70}

**Result:** 280

**Explanation:** Function 9 = SUM, Option 6 = ignore errors. Sums only valid numbers (10+20+30+40+50+60+70), skipping all three error values.

---

### Example 2: AVERAGE Ignoring Hidden Rows
```
=AGGREGATE(1, 5, B1:B100)
```
**Result:** Average of visible rows only

**Explanation:** Function 1 = AVERAGE, Option 5 = ignore hidden rows. Perfect for calculating averages on filtered data without including filtered-out values.

---

### Example 3: MAX Ignoring Both Errors and Hidden Rows
```
=AGGREGATE(4, 7, C1:C50)
```
**Result:** Maximum value from visible, error-free cells

**Explanation:** Function 4 = MAX, Option 7 = ignore hidden rows AND errors. The most comprehensive ignore option for finding maximum values in messy filtered data.

---

### Example 4: COUNT Ignoring Errors
```
=AGGREGATE(2, 6, A1:A10)
```
Where A1:A10 contains {10, 20, #DIV/0!, 30, #N/A, 40, 50, #VALUE!, 60, 70}

**Result:** 7

**Explanation:** Function 2 = COUNT, Option 6 = ignore errors. Counts only the 7 valid numbers, not the 3 error values.

---

### Example 5: LARGE - Third Largest Value
```
=AGGREGATE(14, 6, A1:A100, 3)
```
**Result:** Third largest value in range, ignoring errors

**Explanation:** Function 14 = LARGE, requires k parameter. Returns 3rd largest value while ignoring any error cells in the range.

---

### Example 6: SMALL - Second Smallest Value
```
=AGGREGATE(15, 6, B1:B50, 2)
```
**Result:** Second smallest value in range, ignoring errors

**Explanation:** Function 15 = SMALL, requires k parameter. Useful for finding near-minimum values in data with potential errors.

---

### Example 7: MEDIAN Ignoring Errors
```
=AGGREGATE(12, 6, SalesData)
```
**Result:** Median of valid values in named range SalesData

**Explanation:** Function 12 = MEDIAN, Option 6 = ignore errors. MEDIAN function normally fails with any error; AGGREGATE handles it gracefully.

---

### Example 8: Standard Deviation with Error Handling
```
=AGGREGATE(7, 6, A1:A1000)
```
**Result:** Sample standard deviation of valid values

**Explanation:** Function 7 = STDEV.S (sample). Calculates standard deviation while skipping error cells that would otherwise break the calculation.

---

### Example 9: PERCENTILE - 90th Percentile
```
=AGGREGATE(16, 6, Scores, 0.9)
```
**Result:** 90th percentile of Scores range, ignoring errors

**Explanation:** Function 16 = PERCENTILE.INC. The k value (0.9) specifies 90th percentile. Errors in the data are ignored.

---

### Example 10: QUARTILE - First Quartile
```
=AGGREGATE(17, 6, DataRange, 1)
```
**Result:** First quartile (25th percentile) of DataRange

**Explanation:** Function 17 = QUARTILE.INC. k=1 means first quartile. Options 6 ignores any error values in the range.

---

### Example 11: Product of Values Ignoring Errors
```
=AGGREGATE(6, 6, A1:A5)
```
Where A1:A5 contains {2, 3, #N/A, 4, 5}

**Result:** 120

**Explanation:** Function 6 = PRODUCT. Multiplies 2*3*4*5 = 120, skipping the #N/A error. Regular PRODUCT would return #N/A.

---

### Example 12: Combining with Other Functions
```
=IF(AGGREGATE(2, 6, A:A) > 0, AGGREGATE(1, 6, A:A), "No data")
```
**Result:** Average if there's valid data, otherwise "No data"

**Explanation:** First AGGREGATE counts valid numbers; if count > 0, second AGGREGATE calculates average. Both ignore errors.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | function_num not 1-19, or options not 0-7 | Verify numeric codes are in valid range. Common mistake: using 0 for function_num (there is no function 0). |
| `#VALUE!` | Missing k parameter for LARGE/SMALL/PERCENTILE/QUARTILE | Functions 14-19 require the k parameter. Add the fourth argument. |
| `#NUM!` | k value out of range | For LARGE/SMALL, k must be <= count of values. For PERCENTILE, k must be 0-1. |
| `#NAME?` | Using in Google Sheets | AGGREGATE is Excel-only. Use alternative functions in Sheets. |
| `Wrong result` | Using wrong options number | Option 6 ignores errors only. Option 7 ignores errors AND hidden rows. Verify your option matches your need. |
| `Not ignoring subtotals` | Options 4-7 don't ignore nested AGGREGATE/SUBTOTAL | Use options 0-3 if you need to ignore nested aggregate functions. |

## Use Cases

### [[Error-Tolerant Financial Reporting]]

**Scenario:** Calculate totals, averages, and other metrics from financial data that contains #DIV/0! errors from ratio calculations.

**Implementation:**
```
Total Revenue: =AGGREGATE(9, 6, Revenue)
Average Margin: =AGGREGATE(1, 6, Margins)
Highest Sale: =AGGREGATE(4, 6, Sales)
```

**Business Application:** Monthly financial reports, sales dashboards, KPI tracking where source data may have calculation errors that shouldn't break summary statistics.

**Technical Details:** Option 6 ensures calculations proceed despite errors. Finance teams can fix errors at leisure without blocking report generation.

---

### [[Filtered Data Analysis]]

**Scenario:** Calculate statistics only on visible (filtered) rows when using Excel's AutoFilter feature.

**Implementation:**
```
Visible Sum: =AGGREGATE(9, 5, A:A)
Visible Average: =AGGREGATE(1, 5, B:B)
Visible Count: =AGGREGATE(2, 5, C:C)
```

**Business Application:** Interactive dashboards where users filter data and need real-time summary statistics that update based on current filter state.

**Technical Details:** Option 5 ignores hidden rows. Unlike SUBTOTAL, AGGREGATE provides additional functions (MEDIAN, PERCENTILE, etc.) that respect filters.

---

### [[Robust Statistical Analysis]]

**Scenario:** Perform statistical analysis (median, quartiles, percentiles) on datasets that may contain error values.

**Implementation:**
```
Median: =AGGREGATE(12, 6, DataRange)
Q1: =AGGREGATE(17, 6, DataRange, 1)
Q3: =AGGREGATE(17, 6, DataRange, 3)
P90: =AGGREGATE(16, 6, DataRange, 0.9)
```

**Business Application:** Quality control analysis, performance benchmarking, salary surveys where some calculated fields may error but analysis must continue.

**Technical Details:** Standard MEDIAN, QUARTILE, PERCENTILE functions fail on any error. AGGREGATE allows these calculations to proceed with valid data.

---

### [[Dynamic Top/Bottom N Analysis]]

**Scenario:** Find the nth largest or smallest values in a dataset that may contain errors.

**Implementation:**
```
Top 1: =AGGREGATE(14, 6, Sales, 1)
Top 2: =AGGREGATE(14, 6, Sales, 2)
Top 3: =AGGREGATE(14, 6, Sales, 3)
Bottom 3: =AGGREGATE(15, 6, Sales, 3)
```

**Business Application:** Identifying top performers, worst performers, outliers in sales data, student scores, or any ranking scenario.

**Technical Details:** LARGE and SMALL functions (14, 15) require the k parameter specifying rank. Option 6 ensures errors don't corrupt rankings.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Full support:** All 19 functions and all 8 options available
- **Performance:** Efficient for large datasets
- **Array form:** Supports array calculations for k-functions

### Google Sheets
- **AGGREGATE is NOT available in Google Sheets**
- **Alternative for error handling:** Use SUMIF/AVERAGEIF with ISERROR, or QUERY
- **Alternative for hidden rows:** Use SUBTOTAL with visible-only options
- **Example workaround:**
```
// Instead of =AGGREGATE(9, 6, A:A) for sum ignoring errors:
=SUMPRODUCT((A1:A100)*(NOT(ISERROR(A1:A100))))
```

### Compatibility Consideration
If workbooks need to function in both Excel and Google Sheets, avoid AGGREGATE and use more universal approaches:
- For error handling: Helper columns with IFERROR
- For visible rows: SUBTOTAL functions
- For both: Custom JavaScript/Apps Script in Sheets

## Tips and Best Practices

1. **Create a reference card:** Keep a cheat sheet of function numbers (9=SUM, 1=AVERAGE, 4=MAX, etc.) until you memorize the common ones.

2. **Use named constants:** In Excel, define names like `SUM_FUNC=9`, `IGNORE_ERRORS=6` to make formulas readable: `=AGGREGATE(SUM_FUNC, IGNORE_ERRORS, A:A)`.

3. **Default to Option 6:** For most use cases, Option 6 (ignore errors only) is the safest choice. Only use hidden-row options (5, 7) when specifically working with filtered data.

4. **Remember k-functions:** Functions 14-19 (LARGE, SMALL, PERCENTILE, QUARTILE) require a fourth parameter. This is the most common AGGREGATE error.

5. **Test with known errors:** Before relying on AGGREGATE, test your formula with intentional errors to verify it behaves as expected.

6. **Consider readability:** For simple cases, `=SUMIF(A:A, "<>#N/A")` might be clearer than `=AGGREGATE(9, 6, A:A)`. Choose based on your audience.

7. **Don't use in shared Sheets files:** If any user might open your file in Google Sheets, avoid AGGREGATE entirely.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SUBTOTAL]] | Performs 11 functions with hidden-row options | When you don't need error handling and want simpler syntax |
| [[SUM]] | Simple sum | When data is clean with no errors |
| [[AVERAGE]] | Simple average | When data is clean with no errors |
| [[SUMIF]] | Conditional sum | When filtering by criteria rather than errors |

### Commonly Used Together

**[[IFERROR]]** - Handle individual errors

*Wrap problematic source calculations:*
```
=IFERROR(A1/B1, 0)  // In source data
=SUM(C:C)           // Then sum normally
```
Alternative approach when AGGREGATE isn't available.

---

**[[SUBTOTAL]]** - Simpler filtered calculations

*For basic operations without error handling:*
```
=SUBTOTAL(9, A:A)   // Sum of visible rows (simpler than AGGREGATE(9,5,A:A))
```
Use when errors aren't a concern.

---

**[[LARGE]] / [[SMALL]]** - Rank functions

*Standard functions when errors aren't present:*
```
=LARGE(A:A, 3)      // Third largest
=AGGREGATE(14, 6, A:A, 3)  // Same, but ignores errors
```
AGGREGATE versions add error tolerance.

---

**[[FILTER]]** - Dynamic filtering (Excel 365)

*Modern alternative for filtered calculations:*
```
=SUM(FILTER(A:A, NOT(ISERROR(A:A))))  // Sum non-error values
```
More readable for users familiar with dynamic arrays.

## Official Documentation

- **Microsoft Excel:** [AGGREGATE function](https://support.microsoft.com/en-us/office/aggregate-function-43b9278e-6aa7-4f17-92b6-e19993fa26df)
- **Google Sheets:** Not available - use [SUBTOTAL](https://support.google.com/docs/answer/3093649) as partial alternative

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Full implementation with all 19 functions |
| Excel 2016 | Enhanced | Performance improvements |
| Google Sheets | N/A | Not implemented |

---

*Last updated: 2026-01-10*
