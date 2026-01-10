---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - lambda-functions
  - array-manipulation
subTopics:
  - column-processing
  - functional-programming
  - data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - by column
  - column iterator
  - column-wise function
tags:
  - google-sheets-only
  - lambda
  - arrays
  - column-operations
  - functional
---

# BYCOL

## Description

**BYCOL** is a powerful lambda helper function in Google Sheets that applies a custom function to each column of a given array or range, returning a single row where each cell contains the result of processing its corresponding column. Think of it as a column-wise aggregator: if you have a table with 5 columns, BYCOL processes each column independently through your specified function and returns 5 results arranged horizontally. This is fundamentally different from applying a function to the entire range at once because BYCOL ensures isolation between columns, making it perfect for calculating per-column statistics, performing column-level transformations, or creating summary rows that aggregate each column according to custom logic.

The primary use case for BYCOL is when you need to perform the same operation across multiple columns but cannot easily express this with traditional array formulas. For example, calculating the median of each column in a dataset is trivial with BYCOL (simply `=BYCOL(data, LAMBDA(col, MEDIAN(col)))`) but would otherwise require individual MEDIAN formulas for each column. BYCOL truly shines when combined with complex LAMBDA functions that perform multi-step calculations, conditional aggregations, or custom business logic on each column. It enables functional programming patterns in spreadsheets, where you define what to do with a column once, and BYCOL applies that logic uniformly across all columns in your data.

There are several critical gotchas to understand when working with BYCOL. First, the LAMBDA function you provide must accept exactly one parameter (the column being processed) and must return a single value; returning an array from the LAMBDA will cause errors. Second, BYCOL always returns a single row regardless of input dimensions, so if you need multiple output rows per column, you must use a different approach (like combining with VSTACK). Third, empty columns are still processed and passed to your LAMBDA, so you must handle empty inputs gracefully to avoid errors. Fourth, BYCOL processes columns left-to-right sequentially, which matters when your LAMBDA has side effects or when performance is critical with large datasets. Fifth, nested BYCOL calls (BYCOL within BYCOL) can quickly become complex and slow; consider alternative approaches for multi-dimensional iteration.

From a platform perspective, BYCOL is available in both Google Sheets and Microsoft Excel 365. While the syntax and basic functionality are identical between platforms, there may be subtle differences in how edge cases are handled, particularly with mixed data types or very large arrays. The examples and documentation in this file focus on Google Sheets behavior. If you are using Excel 365, the same formulas should work, but always test with your specific data. For users of older Excel versions without dynamic array support, BYCOL is not available, and you would need to use VBA or individual formulas per column.

## Syntax

> [!f(x)] BYCOL Syntax
>
> ```
> =BYCOL(array_or_range, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array_or_range` | Yes | The array or range to process column by column. Can be a direct range reference (A1:E100), a named range, the result of another function, or a literal array constructed with curly braces. Each column of this array is passed sequentially to the LAMBDA function. The array can have any number of rows and columns. |
| `lambda` | Yes | A LAMBDA function that defines what operation to perform on each column. The LAMBDA must accept exactly one parameter (representing a single column as a vertical array) and must return a single value. The syntax is `LAMBDA(column_parameter, expression_using_that_parameter)`. The parameter name is arbitrary (col, c, column, etc.). |

### Return Value

Returns a single-row array (1 row by N columns) where N equals the number of columns in the input array. Each cell in the output row contains the result of applying the LAMBDA function to the corresponding column in the input. If the LAMBDA returns non-scalar values, BYCOL will error.

## Examples

> [!f(x)] BYCOL Examples

### Example 1: Sum Each Column
```
=BYCOL(A1:E10, LAMBDA(col, SUM(col)))
```
**Result:** Returns a row of 5 values, each being the sum of its respective column (sum of A1:A10, sum of B1:B10, etc.)

**Explanation:** This is the most basic BYCOL usage. The LAMBDA receives each column as a vertical array named `col`, then applies SUM to it. The output is equivalent to having `=SUM(A1:A10)`, `=SUM(B1:B10)`, etc. in separate cells, but consolidated into a single formula.

---

### Example 2: Calculate Column Averages
```
=BYCOL(B2:F100, LAMBDA(c, AVERAGE(c)))
```
**Result:** Returns 5 values representing the average of each column B through F

**Explanation:** Replace `c` with any variable name you prefer - `column`, `data`, `x` all work. This calculates the arithmetic mean of each column, useful for summary statistics in reports.

---

### Example 3: Find Maximum Value Per Column
```
=BYCOL(A1:D50, LAMBDA(column, MAX(column)))
```
**Result:** Returns 4 values, each being the maximum value found in its respective column

**Explanation:** MAX processes each column independently. This is perfect for creating a "high watermark" row that shows the peak value in each category or metric.

---

### Example 4: Count Non-Empty Cells Per Column
```
=BYCOL(A2:G100, LAMBDA(col, COUNTA(col)))
```
**Result:** Returns 7 values indicating how many non-empty cells are in each column

**Explanation:** COUNTA counts cells containing any value (numbers, text, errors). This is useful for data completeness analysis - seeing which columns have missing data.

---

### Example 5: Calculate Median for Each Column
```
=BYCOL(B2:E50, LAMBDA(c, MEDIAN(c)))
```
**Result:** Returns 4 median values, one per column

**Explanation:** MEDIAN is a function that does not naturally work across columns in traditional formulas. BYCOL makes column-wise median calculation trivial.

---

### Example 6: Standard Deviation Per Column
```
=BYCOL(DataRange, LAMBDA(col, STDEV(col)))
```
**Result:** Returns standard deviation values for each column in the named range DataRange

**Explanation:** This enables statistical analysis by column without creating individual formulas. The named range makes the formula more readable and maintainable.

---

### Example 7: Conditional Count - Values Greater Than Threshold
```
=BYCOL(A1:D100, LAMBDA(c, COUNTIF(c, ">100")))
```
**Result:** Returns 4 counts showing how many values exceed 100 in each column

**Explanation:** COUNTIF works within BYCOL for conditional counting. This pattern is useful for threshold analysis, quality control (counting defects), or categorization metrics.

---

### Example 8: Percentage of Positive Values Per Column
```
=BYCOL(B2:F50, LAMBDA(col, COUNTIF(col, ">0") / COUNTA(col)))
```
**Result:** Returns percentages (as decimals) representing the fraction of positive values in each column

**Explanation:** The LAMBDA contains a compound expression: counting positive values divided by total non-empty values. Format the output cells as percentages for readability.

---

### Example 9: First Non-Empty Value in Each Column
```
=BYCOL(A1:E20, LAMBDA(c, INDEX(c, MATCH(TRUE, c<>"", 0))))
```
**Result:** Returns the first non-empty value from each column

**Explanation:** This uses INDEX/MATCH within the LAMBDA to find the first non-empty cell. Useful for extracting header rows, first entries, or priority values from sparse data.

---

### Example 10: Concatenate All Values in Each Column
```
=BYCOL(A1:C10, LAMBDA(col, TEXTJOIN(", ", TRUE, col)))
```
**Result:** Returns 3 strings, each containing all values from its column joined by commas

**Explanation:** TEXTJOIN aggregates text values with a delimiter. This creates a summary string per column, useful for creating compact reports or data exports.

---

### Example 11: Calculate Weighted Average Per Column
```
=BYCOL(B2:E100, LAMBDA(values, SUMPRODUCT(values, $A$2:$A$100) / SUM($A$2:$A$100)))
```
**Result:** Returns 4 weighted averages, each column weighted by the values in column A

**Explanation:** The LAMBDA references an external weight column ($A$2:$A$100) while processing each data column. This demonstrates that LAMBDAs can reference cells outside the input array.

---

### Example 12: Check If Any Value Exceeds Limit
```
=BYCOL(A1:D50, LAMBDA(c, IF(MAX(c) > 1000, "ALERT", "OK")))
```
**Result:** Returns "ALERT" or "OK" for each column based on whether any value exceeds 1000

**Explanation:** Combining MAX with IF creates a column-level alert system. This is useful for monitoring dashboards, quality control, or budget tracking.

---

### Example 13: Calculate Range (Max - Min) Per Column
```
=BYCOL(B2:G100, LAMBDA(col, MAX(col) - MIN(col)))
```
**Result:** Returns 6 values representing the spread (range) of each column

**Explanation:** Compound expressions work within LAMBDA. The range (maximum minus minimum) measures data variability for each column.

---

### Example 14: Custom Statistical Measure - Coefficient of Variation
```
=BYCOL(DataRange, LAMBDA(c, STDEV(c) / AVERAGE(c)))
```
**Result:** Returns coefficient of variation (relative standard deviation) for each column

**Explanation:** The coefficient of variation normalizes standard deviation by the mean, allowing comparison of variability across columns with different scales. Multiply by 100 for percentage format.

---

### Example 15: Combined with QUERY Result
```
=BYCOL(QUERY(SalesData, "SELECT B, C, D, E WHERE A = 'Active'"), LAMBDA(c, SUM(c)))
```
**Result:** Sums each column of the filtered QUERY result

**Explanation:** BYCOL can process the output of other array functions like QUERY. This first filters to active records, then calculates column totals on the filtered data.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | LAMBDA returns an array instead of a single value | Ensure your LAMBDA expression evaluates to a single value. Use aggregation functions like SUM, AVERAGE, MAX, MIN, or logical tests that return single results. |
| `#NAME?` | LAMBDA syntax is incorrect or parameter name is invalid | Check LAMBDA structure: `LAMBDA(param_name, expression)`. Parameter names cannot be cell references or reserved words. |
| `#REF!` | Output is trying to spill into cells with existing data | Clear cells to the right of the formula to make room for the output row. BYCOL needs N empty cells where N is the number of input columns. |
| `#N/A` | Lookup function inside LAMBDA fails for one or more columns | Wrap lookups in IFERROR: `LAMBDA(c, IFERROR(VLOOKUP(...), "Not found"))`. Handle cases where columns may not have matching values. |
| `#ERROR!` | Dimension mismatch in LAMBDA expression | When referencing external ranges in LAMBDA, ensure they have compatible dimensions with the column being processed. |
| `Formula parse error` | Missing comma between LAMBDA parameters, or missing parentheses | Review LAMBDA syntax carefully. Common issues: missing comma after parameter name, unbalanced parentheses, or incorrect function nesting. |
| Unexpected zeros | Empty columns are processed and aggregation functions return 0 | Add conditional logic: `LAMBDA(c, IF(COUNTA(c)=0, "", SUM(c)))` to handle empty columns appropriately. |

## Use Cases

### [[Financial Reporting: Column-Wise Totals and Metrics]]

**Scenario:** A finance team maintains a monthly budget spreadsheet where each column represents a department (Marketing, Sales, Operations, Engineering, etc.) and rows represent expense categories. They need to calculate multiple summary statistics for each department without creating individual formulas.

**Implementation:**
```
=BYCOL(B2:F50, LAMBDA(dept, SUM(dept)))
=BYCOL(B2:F50, LAMBDA(dept, AVERAGE(dept)))
=BYCOL(B2:F50, LAMBDA(dept, COUNTIF(dept, ">10000")))
```

**Business Application:** Creates a summary row showing total spend per department, average expense category size, and count of "major expenses" (over $10K). The formulas automatically adjust when new departments (columns) are added or expense categories (rows) change. Finance can quickly compare department spending patterns without manual calculation.

**Technical Details:** Each BYCOL formula produces a single row of results. Place them in consecutive rows to build a complete summary section. Using a named range for the data area makes formulas more maintainable and enables easy updates to the data extent.

---

### [[Quality Control: Per-Metric Analysis]]

**Scenario:** A manufacturing quality team tracks multiple quality metrics across production lines. Each column represents a different metric (defect rate, cycle time, yield percentage), and rows represent hourly readings. They need to calculate control limits and identify out-of-specification columns.

**Implementation:**
```
=BYCOL(Metrics, LAMBDA(m, AVERAGE(m)))
=BYCOL(Metrics, LAMBDA(m, AVERAGE(m) + 3*STDEV(m)))
=BYCOL(Metrics, LAMBDA(m, AVERAGE(m) - 3*STDEV(m)))
=BYCOL(Metrics, LAMBDA(m, IF(OR(MAX(m) > AVERAGE(m) + 3*STDEV(m), MIN(m) < AVERAGE(m) - 3*STDEV(m)), "OUT OF CONTROL", "OK")))
```

**Business Application:** Automatically calculates center line (average), upper control limit (+3 sigma), and lower control limit (-3 sigma) for each metric. The final formula flags metrics that have any readings outside control limits. This enables real-time quality monitoring without manual control chart construction.

**Technical Details:** Control limits are calculated using the 3-sigma rule. The OR function within the LAMBDA checks both upper and lower violations. For more sophisticated analysis, replace with custom control limit calculations based on your specific quality methodology.

---

### [[Survey Analysis: Response Statistics by Question]]

**Scenario:** An HR team conducts employee satisfaction surveys where each column represents a survey question (scored 1-5) and each row represents an employee response. They need quick statistical summaries for each question.

**Implementation:**
```
=BYCOL(Responses, LAMBDA(q, AVERAGE(q)))
=BYCOL(Responses, LAMBDA(q, MEDIAN(q)))
=BYCOL(Responses, LAMBDA(q, MODE(q)))
=BYCOL(Responses, LAMBDA(q, COUNTIF(q, "<=2") / COUNTA(q)))
```

**Business Application:** Calculates average score, median score, most common response, and percentage of negative responses (1-2 ratings) for each survey question. Leadership can quickly identify problem areas (low averages, high negative percentages) and track improvement over time by comparing survey waves.

**Technical Details:** MODE returns the most frequently occurring value. The negative response percentage uses COUNTIF to count low ratings divided by COUNTA for total responses. Format the percentage row as percentages for clarity.

---

### [[Sales Dashboard: Regional Performance Comparison]]

**Scenario:** A sales operations team has a dataset where each column represents a sales region and each row represents a customer transaction amount. They need to create a dashboard comparing regional performance.

**Implementation:**
```
=BYCOL(SalesData, LAMBDA(region, SUM(region)))
=BYCOL(SalesData, LAMBDA(region, COUNTIF(region, ">0")))
=BYCOL(SalesData, LAMBDA(region, AVERAGE(IF(region > 0, region))))
=BYCOL(SalesData, LAMBDA(region, MAX(region)))
=BYCOL(SalesData, LAMBDA(region, SUM(region) / SUM(SalesData)))
```

**Business Application:** Creates a comprehensive regional comparison showing: total revenue, number of transactions, average transaction size (excluding zeros), largest deal, and percentage of total company revenue. Sales leadership can quickly compare regional performance and identify top and underperforming regions.

**Technical Details:** The average calculation uses an IF inside AVERAGE to exclude zero values (indicating no sale). The percentage calculation references the entire SalesData range for the company total. These formulas automatically update as new transactions are added.

## Platform Differences

### Microsoft Excel

- **Availability:** BYCOL is available in Excel 365 (Microsoft 365 subscription versions)
- **Syntax:** Identical to Google Sheets: `=BYCOL(array, LAMBDA(param, expression))`
- **Behavior:** Functions the same way, processing each column through the LAMBDA
- **Not available in:** Excel 2019, Excel 2016, Excel for Mac (non-365 versions), or earlier versions
- **Alternative for older Excel:** Use individual column formulas, VBA macros, or Power Query for column-wise processing

### Google Sheets

- **Availability:** Available in all Google Sheets (introduced in 2022 with the LAMBDA function family)
- **Specific Behavior:** This documentation focuses on Google Sheets behavior
- **Calculation:** May have different performance characteristics than Excel for very large arrays
- **Integration:** Works seamlessly with other Google Sheets functions like QUERY, IMPORTRANGE, and ARRAYFORMULA

### Key Differences

| Feature | Google Sheets | Excel 365 |
|---------|---------------|-----------|
| Availability | All users | Microsoft 365 subscribers only |
| Syntax | Identical | Identical |
| Error handling | Same error types | Same error types |
| Large array performance | May slow with 10,000+ cell inputs | Generally better optimization |
| Spill behavior | Automatic | Automatic |

## Tips and Best Practices

1. **Name Your LAMBDA Parameter Meaningfully:** Use descriptive names like `col`, `column`, or `data` rather than single letters. This makes complex formulas more readable: `LAMBDA(salesColumn, SUM(salesColumn))` is clearer than `LAMBDA(x, SUM(x))`.

2. **Test LAMBDA Independently First:** Before wrapping in BYCOL, test your LAMBDA logic on a single column to verify it returns the expected single value. Create the LAMBDA portion separately to debug issues.

3. **Handle Empty Columns Gracefully:** Add defensive logic for columns that might be empty: `LAMBDA(c, IF(COUNTA(c)=0, "", your_calculation))`. This prevents zeros or errors from empty columns.

4. **Use Named Ranges for Maintainability:** Instead of `=BYCOL(B2:F100, ...)`, define a named range like "DataTable" and use `=BYCOL(DataTable, ...)`. This makes formulas easier to update when data ranges change.

5. **Combine Multiple Statistics with HSTACK:** Create comprehensive summary rows by horizontally stacking multiple BYCOL results: `={BYCOL(data, LAMBDA(c, SUM(c))); BYCOL(data, LAMBDA(c, AVERAGE(c)))}` produces a summary section.

6. **Remember BYCOL Returns a Row:** Plan your sheet layout knowing that BYCOL output is horizontal. If you need vertical output, wrap in TRANSPOSE: `=TRANSPOSE(BYCOL(...))`.

7. **Be Cautious with Volatile Functions:** If your LAMBDA contains volatile functions like NOW(), TODAY(), or RAND(), BYCOL will recalculate frequently, potentially impacting performance.

8. **Reference External Cells with Absolute References:** When your LAMBDA references cells outside the processed array (like a threshold or weight column), use absolute references ($A$1) to prevent unexpected behavior.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BYROW]] | Applies LAMBDA to each row, returning a column | When you need row-wise processing instead of column-wise |
| [[MAP]] | Applies LAMBDA to each cell individually | When processing element-by-element, not column-by-column |
| [[REDUCE]] | Accumulates array values into a single result | When you need one total result, not one per column |
| [[SCAN]] | Like REDUCE but returns intermediate results | When you need cumulative results, not column aggregates |
| [[MAKEARRAY]] | Generates array using row/column positions | When creating arrays rather than processing existing ones |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the column processing function

*BYCOL requires LAMBDA to define what happens to each column:*
```
=BYCOL(A1:D100, LAMBDA(column, your_expression))
```
Without LAMBDA, BYCOL cannot function. The LAMBDA encapsulates your column logic.

---

**[[TRANSPOSE]]** - Convert horizontal results to vertical

*Convert BYCOL's row output to a column:*
```
=TRANSPOSE(BYCOL(A1:D100, LAMBDA(c, SUM(c))))
```
Useful when you want column totals displayed vertically instead of horizontally.

---

**[[HSTACK]]** - Combine multiple BYCOL results

*Create a summary matrix from multiple BYCOL calculations:*
```
=VSTACK(BYCOL(data, LAMBDA(c, SUM(c))), BYCOL(data, LAMBDA(c, AVERAGE(c))))
```
Stacks sum row and average row vertically for comprehensive column statistics.

---

**[[QUERY]]** - Pre-filter data before column processing

*Calculate column statistics on filtered data:*
```
=BYCOL(QUERY(Data, "SELECT B,C,D,E WHERE A='Active'"), LAMBDA(c, SUM(c)))
```
QUERY filters rows first, then BYCOL processes each column of the result.

---

**[[IFERROR]]** - Handle errors in column processing

*Graceful error handling for BYCOL operations:*
```
=BYCOL(A1:E100, LAMBDA(c, IFERROR(AVERAGE(c), 0)))
```
Returns 0 instead of error if a column causes AVERAGE to fail.

## Official Documentation

- **Google Sheets:** [BYCOL function](https://support.google.com/docs/answer/12570930)
- **Microsoft Excel:** [BYCOL function](https://support.microsoft.com/en-us/office/bycol-function-2dc18bf2-3d36-4f84-bf6e-8ef1c6f88c7c)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2022 | Introduced as part of the LAMBDA helper function family |
| Excel 365 | 2022 | Added with LAMBDA function ecosystem |
| Excel 2021 | N/A | Not available in perpetual license version |
| Excel Online | 2022 | Available in web version |

---

*Last updated: 2026-01-10*
