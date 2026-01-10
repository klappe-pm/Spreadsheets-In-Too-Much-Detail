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
  - row-processing
  - functional-programming
  - data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - by row
  - row iterator
  - row-wise function
tags:
  - google-sheets-only
  - lambda
  - arrays
  - row-operations
  - functional
---

# BYROW

## Description

**BYROW** is a powerful lambda helper function in Google Sheets that applies a custom function to each row of a given array or range, returning a single column where each cell contains the result of processing its corresponding row. Think of it as a row-wise aggregator: if you have a table with 100 rows, BYROW processes each row independently through your specified function and returns 100 results arranged vertically. This solves one of the most persistent challenges in spreadsheet formulas: performing row-by-row operations that aggregate or transform multiple columns into a single result per row. Before BYROW, such operations often required helper columns, complex array formulas, or copying formulas down hundreds of rows. BYROW consolidates this into a single, elegant formula.

The primary use case for BYROW is when you need to apply logic across columns within each row independently. For example, calculating the maximum value across multiple columns for each row, counting how many cells in each row meet a condition, or performing custom calculations that span multiple columns are all natural fits for BYROW. The function truly shines when combined with complex LAMBDA expressions that perform multi-step calculations, conditional logic, or custom aggregations. Unlike ARRAYFORMULA, which processes columns in parallel but struggles with row-wise aggregation, BYROW explicitly iterates through rows, making it the right tool for per-row calculations that involve multiple columns. This enables clean, maintainable formulas that replace what would otherwise be copied formulas in every row.

There are several critical gotchas to understand when working with BYROW. First, the LAMBDA function you provide must accept exactly one parameter (the row being processed, which is a horizontal array) and must return a single value; attempting to return an array from the LAMBDA will cause errors. Second, BYROW always returns a single column regardless of input dimensions, so if you need multiple output columns per row, you must use a different approach like combining BYROW with HSTACK or using MAP instead. Third, performance can degrade significantly with large datasets (thousands of rows) because BYROW processes each row sequentially; consider whether native array functions might be faster for your specific use case. Fourth, when your LAMBDA references cells outside the input range, ensure those references are properly structured to work correctly as BYROW iterates through rows. Fifth, empty rows are still processed and passed to your LAMBDA, so include defensive logic to handle rows without data.

From a platform perspective, BYROW is available in both Google Sheets and Microsoft Excel 365. The syntax and core functionality are identical between platforms, making formulas portable. However, there may be subtle differences in edge case handling, particularly with mixed data types, error propagation, or very large arrays. This documentation focuses on Google Sheets behavior. If you are using Excel 365, the same formulas should work, but always verify with your specific data. For users of older Excel versions without dynamic array and LAMBDA support, BYROW is not available, and you would need to use helper columns, VBA macros, or copy formulas down each row manually.

## Syntax

> [!f(x)] BYROW Syntax
>
> ```
> =BYROW(array_or_range, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array_or_range` | Yes | The array or range to process row by row. Can be a direct range reference (A1:E100), a named range, the result of another function, or a literal array constructed with curly braces. Each row of this array is passed sequentially to the LAMBDA function as a horizontal array. The array can have any number of rows and columns. |
| `lambda` | Yes | A LAMBDA function that defines what operation to perform on each row. The LAMBDA must accept exactly one parameter (representing a single row as a horizontal array) and must return a single value. The syntax is `LAMBDA(row_parameter, expression_using_that_parameter)`. The parameter name is arbitrary (row, r, data, etc.). |

### Return Value

Returns a single-column array (N rows by 1 column) where N equals the number of rows in the input array. Each cell in the output column contains the result of applying the LAMBDA function to the corresponding row in the input. If the LAMBDA returns non-scalar values, BYROW will error.

## Examples

> [!f(x)] BYROW Examples

### Example 1: Sum Across Columns for Each Row
```
=BYROW(B2:F100, LAMBDA(row, SUM(row)))
```
**Result:** Returns a column of 99 values, each being the sum of its respective row across columns B through F

**Explanation:** This is the most fundamental BYROW usage. The LAMBDA receives each row as a horizontal array named `row`, then applies SUM to it. The output is equivalent to having `=SUM(B2:F2)`, `=SUM(B3:F3)`, etc. in separate cells, but consolidated into a single formula that returns all results.

---

### Example 2: Find Maximum Value Per Row
```
=BYROW(A1:E50, LAMBDA(r, MAX(r)))
```
**Result:** Returns 50 values, each being the maximum value found across columns A-E in that row

**Explanation:** MAX processes each row independently. This is perfect for finding the highest score, best performance, or peak value across multiple categories for each record.

---

### Example 3: Count Non-Empty Cells Per Row
```
=BYROW(B2:G100, LAMBDA(row, COUNTA(row)))
```
**Result:** Returns 99 values indicating how many non-empty cells are in each row

**Explanation:** COUNTA counts cells containing any value (numbers, text, errors). This is useful for data completeness analysis - determining how many fields are filled in for each record.

---

### Example 4: Calculate Row Average
```
=BYROW(C2:H50, LAMBDA(r, AVERAGE(r)))
```
**Result:** Returns 49 values, each being the average of the corresponding row

**Explanation:** AVERAGE calculates the arithmetic mean across all columns for each row. Useful for calculating average scores, ratings, or metrics per entity.

---

### Example 5: Count Values Meeting Condition
```
=BYROW(B2:F100, LAMBDA(row, COUNTIF(row, ">90")))
```
**Result:** Returns 99 values showing how many cells in each row exceed 90

**Explanation:** COUNTIF within BYROW enables conditional counting per row. Great for counting passing grades, values above threshold, or qualifying entries per record.

---

### Example 6: Check If Any Value Exceeds Threshold
```
=BYROW(B2:E100, LAMBDA(r, IF(MAX(r) > 100, "ALERT", "OK")))
```
**Result:** Returns "ALERT" or "OK" for each row based on whether any column value exceeds 100

**Explanation:** Combining MAX with IF creates a row-level status indicator. This pattern is useful for flagging records that need attention, quality control alerts, or threshold violations.

---

### Example 7: Find Position of Maximum Value in Row
```
=BYROW(A2:E50, LAMBDA(row, MATCH(MAX(row), row, 0)))
```
**Result:** Returns the column position (1-5) of the maximum value within each row

**Explanation:** MATCH finds where the maximum value occurs within the row. Useful for identifying which category performed best, which month had highest sales, etc.

---

### Example 8: Calculate Percentage of Positive Values
```
=BYROW(B2:G100, LAMBDA(row, COUNTIF(row, ">0") / COUNTA(row)))
```
**Result:** Returns the fraction of positive values in each row as a decimal

**Explanation:** Dividing COUNTIF by COUNTA gives the proportion. Format output cells as percentages. Useful for calculating completion rates, positive response rates, or success ratios per record.

---

### Example 9: Concatenate Row Values with Delimiter
```
=BYROW(A2:D100, LAMBDA(r, TEXTJOIN(" - ", TRUE, r)))
```
**Result:** Returns 99 strings, each containing the row values joined by " - "

**Explanation:** TEXTJOIN aggregates text across columns for each row. This creates composite strings like "John - Smith - Marketing - Active" per record.

---

### Example 10: Calculate Standard Deviation Per Row
```
=BYROW(B2:H50, LAMBDA(row, STDEV(row)))
```
**Result:** Returns 49 standard deviation values, one per row

**Explanation:** STDEV measures the spread of values within each row. Useful for identifying records with high variability, consistency analysis, or quality control metrics.

---

### Example 11: Calculate Weighted Sum Using External Weights
```
=BYROW(B2:F100, LAMBDA(row, SUMPRODUCT(row, $B$1:$F$1)))
```
**Result:** Returns 99 weighted sums, where each row is weighted by values in row 1

**Explanation:** The LAMBDA references an external weight row ($B$1:$F$1) using absolute references. This enables weighted scoring, grade calculations, or index construction where weights are defined once.

---

### Example 12: Determine Median Across Columns Per Row
```
=BYROW(C2:G100, LAMBDA(r, MEDIAN(r)))
```
**Result:** Returns 99 median values, each being the middle value of its row

**Explanation:** MEDIAN finds the central value when row data is sorted. More robust than AVERAGE when rows might contain outliers.

---

### Example 13: Check If All Values Meet Condition
```
=BYROW(B2:E100, LAMBDA(row, IF(COUNTIF(row, ">=60") = COUNTA(row), "PASS", "FAIL")))
```
**Result:** Returns "PASS" if all non-empty values in the row are at least 60, otherwise "FAIL"

**Explanation:** Comparing COUNTIF to COUNTA determines if all values meet the condition. Useful for determining if a student passed all subjects, all departments met targets, etc.

---

### Example 14: Calculate Range (Spread) of Row Values
```
=BYROW(B2:G50, LAMBDA(r, MAX(r) - MIN(r)))
```
**Result:** Returns 49 values representing the spread (max minus min) for each row

**Explanation:** The range measures variability within each row. High ranges indicate inconsistent performance; low ranges indicate consistency across columns.

---

### Example 15: Find First Non-Empty Value in Row
```
=BYROW(A2:F100, LAMBDA(row, INDEX(row, MATCH(TRUE, row<>"", 0))))
```
**Result:** Returns the first non-empty value from each row

**Explanation:** INDEX/MATCH within LAMBDA finds the first populated cell. Useful for extracting priority values, first responses, or primary identifiers from sparse data.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | LAMBDA returns an array instead of a single value | Ensure your LAMBDA expression evaluates to a single value. Use aggregation functions like SUM, AVERAGE, MAX, MIN, COUNT, or expressions that return single results. |
| `#NAME?` | LAMBDA syntax is incorrect or parameter name is invalid | Check LAMBDA structure: `LAMBDA(param_name, expression)`. Parameter names cannot be cell references, numbers, or reserved function names. |
| `#REF!` | Output is trying to spill into cells with existing data | Clear cells below the formula to make room for the output column. BYROW needs N empty cells where N is the number of input rows. |
| `#N/A` | Lookup function inside LAMBDA fails for one or more rows | Wrap lookups in IFERROR: `LAMBDA(r, IFERROR(lookup_expression, "Not found"))`. Handle rows that may not have matching values. |
| `#ERROR!` | Dimension mismatch in LAMBDA expression | When referencing external ranges in LAMBDA, ensure they have compatible column dimensions with the row being processed (same number of columns). |
| `Formula parse error` | Missing comma between LAMBDA parameters, or missing parentheses | Review LAMBDA syntax: comma between parameter name and expression, matching parentheses, proper function nesting. |
| Unexpected results | Empty rows processed as zeros or causing aggregation issues | Add conditional logic: `LAMBDA(r, IF(COUNTA(r)=0, "", your_calculation))` to handle empty rows appropriately. |
| Performance issues | Processing thousands of rows with complex LAMBDA | Simplify LAMBDA logic, use bounded ranges instead of entire columns, or consider alternative approaches like helper columns for very large datasets. |

## Use Cases

### [[Grade Calculation: Multi-Component Scoring]]

**Scenario:** A teacher has a gradebook where each row is a student and columns represent different assignment categories (Homework, Quizzes, Midterm, Final, Participation). Each category has a different weight, and the teacher needs to calculate weighted final grades for all students.

**Implementation:**
```
=BYROW(B2:F50, LAMBDA(scores, SUMPRODUCT(scores, $B$1:$F$1) / SUM($B$1:$F$1)))
```
Where row 1 contains weights: B1=20, C1=20, D1=25, E1=30, F1=5

**Business Application:** Calculates weighted average grades for all students in a single formula. When weights change, only row 1 needs updating, and all grades recalculate automatically. The formula scales to any number of students without modification. Teachers can add letter grade conversion by wrapping in additional logic.

**Technical Details:** SUMPRODUCT multiplies each score by its corresponding weight, and dividing by SUM of weights normalizes the result. Absolute references ($B$1:$F$1) ensure weights apply to each row. Output can be formatted as percentage and combined with IFS for letter grade assignment.

---

### [[Data Validation: Row Completeness Check]]

**Scenario:** A data team receives customer records where each row should have values in multiple required fields (Name, Email, Phone, Address, ID). They need to flag incomplete records and calculate completeness percentages.

**Implementation:**
```
=BYROW(B2:F500, LAMBDA(record, IF(COUNTA(record) = 5, "Complete", "Incomplete")))
=BYROW(B2:F500, LAMBDA(record, COUNTA(record) / 5))
```

**Business Application:** First formula flags each record as Complete or Incomplete for quick filtering. Second formula shows completeness percentage for quality reporting. Data team can quickly identify records needing follow-up, report overall data quality metrics, and prioritize data cleanup efforts.

**Technical Details:** COUNTA counts non-empty cells regardless of content type (text, numbers, dates). The magic number 5 represents required fields; use COLUMNS(B2:F2) for dynamic column counting. For more sophisticated validation, combine with REGEXMATCH to verify data formats.

---

### [[Financial Analysis: Portfolio Performance Metrics]]

**Scenario:** An investment analyst tracks portfolio holdings where each row is a different stock and columns represent monthly returns. They need to calculate annualized return, volatility, and Sharpe ratio for each holding.

**Implementation:**
```
=BYROW(C2:N100, LAMBDA(returns, AVERAGE(returns) * 12))
=BYROW(C2:N100, LAMBDA(returns, STDEV(returns) * SQRT(12)))
=BYROW(C2:N100, LAMBDA(returns, (AVERAGE(returns) * 12 - 0.02) / (STDEV(returns) * SQRT(12))))
```

**Business Application:** Creates per-holding performance metrics without individual formulas. Annualized return shows expected yearly gain, volatility shows risk level, and Sharpe ratio shows risk-adjusted returns (assuming 2% risk-free rate). Analysts can quickly compare holdings, identify best risk-adjusted performers, and make rebalancing decisions.

**Technical Details:** Returns are assumed monthly; multiplying by 12 annualizes average, multiplying standard deviation by SQRT(12) annualizes volatility. Sharpe ratio formula is (annualized return - risk-free rate) / annualized volatility. Replace 0.02 with cell reference for adjustable risk-free rate.

---

### [[Inventory Management: Multi-Location Stock Status]]

**Scenario:** A retailer tracks inventory across multiple warehouse locations. Each row is a product, and columns represent stock levels at different locations. They need to calculate total inventory, identify stockouts, and flag products needing redistribution.

**Implementation:**
```
=BYROW(C2:H200, LAMBDA(stock, SUM(stock)))
=BYROW(C2:H200, LAMBDA(stock, COUNTIF(stock, "=0")))
=BYROW(C2:H200, LAMBDA(stock, IF(AND(SUM(stock) > 100, MIN(stock) = 0), "REDISTRIBUTE", "OK")))
```

**Business Application:** First formula shows total inventory per product across all locations. Second counts how many locations are out of stock. Third identifies products with high total inventory but zero stock at some locations, flagging redistribution opportunities. Operations can optimize inventory distribution and prevent stockouts.

**Technical Details:** The redistribution logic uses AND to check two conditions: sufficient total inventory (>100 units) and at least one stockout (MIN=0). Thresholds can be adjusted or referenced from cells. For more complex logic, include reorder points as an additional column.

---

## Platform Differences

### Microsoft Excel

- **Availability:** BYROW is available in Excel 365 (Microsoft 365 subscription versions)
- **Syntax:** Identical to Google Sheets: `=BYROW(array, LAMBDA(param, expression))`
- **Behavior:** Functions the same way, processing each row through the LAMBDA
- **Not available in:** Excel 2019, Excel 2016, Excel for Mac (non-365 versions), or earlier versions
- **Alternative for older Excel:** Copy formulas down rows, use VBA macros, or create helper columns

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
| Large array performance | May slow with 5,000+ rows | Generally better optimization |
| Spill behavior | Automatic | Automatic |
| Named function support | Can use custom named functions in LAMBDA | Same capability |

## Tips and Best Practices

1. **Name Your LAMBDA Parameter Descriptively:** Use `row`, `record`, `data`, or context-specific names like `scores` or `prices`. This makes complex formulas self-documenting and easier to maintain.

2. **Test LAMBDA Logic on a Single Row First:** Before wrapping in BYROW, verify your LAMBDA expression works by applying it to one row manually. Debug the logic before scaling to all rows.

3. **Handle Empty Rows Gracefully:** Add defensive logic for rows without data: `LAMBDA(r, IF(COUNTA(r)=0, "", your_calculation))`. This prevents unexpected zeros or errors in your output.

4. **Use Absolute References for External Data:** When your LAMBDA references cells outside the input array (weights, thresholds, lookup ranges), use absolute references ($A$1) so they apply consistently to each row.

5. **Consider Performance for Large Datasets:** BYROW processes sequentially, which can be slow for thousands of rows. For simple operations, native array functions or ARRAYFORMULA might be faster. Test with your actual data size.

6. **Remember BYROW Returns a Column:** Plan your sheet layout knowing that BYROW output is vertical. If you need horizontal output, wrap in TRANSPOSE: `=TRANSPOSE(BYROW(...))`.

7. **Combine with IFERROR for Robust Lookups:** When using lookup functions inside LAMBDA, wrap in IFERROR to handle cases where lookups fail: `LAMBDA(r, IFERROR(INDEX(...), "N/A"))`.

8. **Use Named Ranges for Cleaner Formulas:** Instead of `=BYROW(B2:G1000, ...)`, define named ranges and use `=BYROW(SalesData, ...)`. This improves readability and makes range updates easier.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BYCOL]] | Applies LAMBDA to each column, returning a row | When you need column-wise processing instead of row-wise |
| [[MAP]] | Applies LAMBDA to each cell individually | When processing element-by-element, not row-by-row |
| [[REDUCE]] | Accumulates array values into a single result | When you need one total result, not one per row |
| [[SCAN]] | Like REDUCE but returns intermediate results | When you need cumulative results across the array |
| [[MAKEARRAY]] | Generates array using row/column positions | When creating arrays rather than processing existing ones |
| [[ARRAYFORMULA]] | Applies formulas to arrays | For column-wise operations; does not do row-wise aggregation well |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the row processing function

*BYROW requires LAMBDA to define what happens to each row:*
```
=BYROW(A1:E100, LAMBDA(row, your_expression))
```
Without LAMBDA, BYROW cannot function. The LAMBDA encapsulates your row-processing logic.

---

**[[HSTACK]]** - Add BYROW results as new columns

*Append calculated column to original data:*
```
=HSTACK(A1:E100, BYROW(B1:E100, LAMBDA(r, SUM(r))))
```
Creates a new column containing row totals alongside the original data.

---

**[[FILTER]]** - Filter rows based on BYROW results

*Select rows meeting calculated criteria:*
```
=FILTER(A1:E100, BYROW(B1:E100, LAMBDA(r, SUM(r))) > 1000)
```
Returns only rows where the sum across columns exceeds 1000.

---

**[[SORT]]** - Sort by calculated row values

*Order data by BYROW results:*
```
=SORT(A1:E100, BYROW(B1:E100, LAMBDA(r, AVERAGE(r))), FALSE)
```
Sorts rows by their calculated row average in descending order.

---

**[[IFERROR]]** - Handle errors in row processing

*Graceful error handling:*
```
=BYROW(A1:E100, LAMBDA(r, IFERROR(calculation, 0)))
```
Returns 0 instead of error if a row calculation fails.

---

**[[SUMPRODUCT]]** - Weighted calculations per row

*Calculate weighted sums using external weights:*
```
=BYROW(B2:F100, LAMBDA(r, SUMPRODUCT(r, $B$1:$F$1)))
```
Each row is multiplied by weights defined in row 1.

## Official Documentation

- **Google Sheets:** [BYROW function](https://support.google.com/docs/answer/12570929)
- **Microsoft Excel:** [BYROW function](https://support.microsoft.com/en-us/office/byrow-function-2e04c677-78c8-4e6b-8c10-a4602f2602bb)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2022 | Introduced as part of the LAMBDA helper function family |
| Excel 365 | 2022 | Added with LAMBDA function ecosystem |
| Excel 2021 | N/A | Not available in perpetual license version |
| Excel Online | 2022 | Available in web version |

---

*Last updated: 2026-01-10*
