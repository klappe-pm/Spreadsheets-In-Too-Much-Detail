---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- lambda-functions
- array-functions
subTopics:
- column-processing
- functional-programming
- array-iteration
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- By Column
- Column Iterator
- Column Lambda
tags:
- excel-only
- lambda
- arrays
- dynamic-arrays
- functional
---

# BYCOL

## Description

**BYCOL** applies a LAMBDA function to each column in an array and returns a horizontal array of results. Think of it as a column-by-column processor: you give it a multi-column range and a function, and it runs that function on each column independently, collecting the results into a single row. This is the column-oriented counterpart to BYROW, which processes rows instead.

BYCOL is part of Excel's functional programming toolkit introduced alongside LAMBDA. It enables operations that would otherwise require helper columns or complex array formulas. For example, calculating the maximum value in each column of a table normally requires a separate MAX formula for each column. With BYCOL, one formula handles all columns dynamically, automatically expanding when new columns are added.

**Key gotcha:** The LAMBDA function you provide must accept a single parameter (the column array) and return a single value. If your LAMBDA returns an array, BYCOL will only take the first value from each result. Also, BYCOL processes columns left-to-right and cannot skip columns or process them conditionally - it applies to every column in the input array.

**Platform note:** BYCOL is exclusively available in Microsoft 365 Excel and Excel 2024+. It requires the LAMBDA function, which is also Excel-exclusive. Google Sheets has a different implementation of LAMBDA helper functions with different syntax. This function will not work in Excel 2019 or earlier, LibreOffice Calc, or Apple Numbers.

## Syntax

> [!f(x)] BYCOL Syntax
>
> ```
> =BYCOL(array, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The array or range to process column by column. Can be a cell range (e.g., A1:D10), an array constant, or a formula that returns an array. |
| `lambda` | Yes | A LAMBDA function that takes one parameter (a column array) and returns a single value. The lambda is called once for each column in the array. |

### Return Value

Returns a horizontal (single-row) array where each element is the result of applying the lambda function to the corresponding column from the input array. The number of elements equals the number of columns in the input array.

## Examples

> [!f(x)] BYCOL Examples

### Example 1: Sum Each Column
```
=BYCOL(A1:D10, LAMBDA(col, SUM(col)))
```
**Result:** `{150, 200, 175, 225}` (horizontal array of column sums)

**Explanation:** The most fundamental BYCOL pattern. The lambda receives each column as an array, sums it, and returns the total. Result is a 4-element horizontal array showing each column's sum.

---

### Example 2: Maximum Value Per Column
```
=BYCOL(B2:F20, LAMBDA(c, MAX(c)))
```
**Result:** `{95, 88, 100, 92, 97}` (maximum value from each column)

**Explanation:** Finds the highest value in each of 5 columns. This replaces having 5 separate MAX formulas and automatically adapts if you expand the range to include more columns.

---

### Example 3: Count Non-Empty Cells Per Column
```
=BYCOL(A:E, LAMBDA(column, COUNTA(column)))
```
**Result:** `{150, 148, 152, 140, 145}` (count of non-empty cells per column)

**Explanation:** Applies COUNTA to entire columns to count entries. Useful for data validation to ensure all columns have similar record counts.

---

### Example 4: Average Excluding Zeros
```
=BYCOL(C2:G100, LAMBDA(x, AVERAGEIF(x, "<>0")))
```
**Result:** `{45.5, 52.3, 48.1, 50.0, 47.8}` (averages excluding zero values)

**Explanation:** The lambda can contain any function, including conditional ones. Here, AVERAGEIF calculates the average of non-zero values in each column.

---

### Example 5: Standard Deviation Per Column
```
=BYCOL(Data, LAMBDA(c, STDEV.P(c)))
```
**Result:** Population standard deviation for each column in named range "Data"

**Explanation:** Statistical functions work naturally within BYCOL. Using a named range makes the formula cleaner and more maintainable.

---

### Example 6: Check if Column Contains Specific Value
```
=BYCOL(A1:E50, LAMBDA(col, COUNTIF(col, "Error")>0))
```
**Result:** `{TRUE, FALSE, TRUE, FALSE, FALSE}` (which columns contain "Error")

**Explanation:** Returns a Boolean array indicating which columns contain the word "Error". Useful for data quality checks across multiple columns.

---

### Example 7: First Non-Empty Value Per Column
```
=BYCOL(A1:D20, LAMBDA(c, FILTER(c, c<>"", "Empty")))
```
**Result:** First non-empty value from each column, or "Empty" if all blank

**Explanation:** FILTER inside the lambda finds non-empty values. Since BYCOL takes only the first result when lambda returns an array, this effectively gets the first non-empty value.

---

### Example 8: Column-wise Percentage of Total
```
=BYCOL(B2:E10, LAMBDA(c, SUM(c)/SUM(B2:E10)*100))
```
**Result:** `{25.5, 30.2, 22.1, 22.2}` (each column's percentage of grand total)

**Explanation:** Calculates what percentage each column contributes to the overall total. The lambda references both its parameter (c) and the full range for the grand total.

---

### Example 9: Combine with BYROW for Matrix Operations
```
=BYCOL(A1:D4, LAMBDA(c, SUM(BYROW(c, LAMBDA(r, r*2)))))
```
**Result:** Sum of each column after doubling all values

**Explanation:** BYCOL and BYROW can be nested for complex matrix operations. Here, each value is doubled before summing by column.

---

### Example 10: Conditional Counting with Multiple Criteria
```
=BYCOL(B2:F100, LAMBDA(col, SUMPRODUCT((col>50)*(col<100))))
```
**Result:** Count of values between 50 and 100 in each column

**Explanation:** SUMPRODUCT inside the lambda enables multi-criteria counting. This counts how many values fall within the specified range in each column.

---

### Example 11: Text Analysis - Average Word Length
```
=BYCOL(Comments, LAMBDA(c, AVERAGE(LEN(c)-LEN(SUBSTITUTE(c," ",""))+1)))
```
**Result:** Average word count per comment in each column

**Explanation:** Advanced text analysis calculating average words per cell. LEN and SUBSTITUTE count spaces to estimate word counts.

---

### Example 12: Dynamic Column Headers from Data
```
=BYCOL(A2:E2, LAMBDA(header, header & " (" & COUNTA(A3:E100) & ")"))
```
**Result:** `{"Name (98)", "Age (98)", "City (98)", "Status (98)", "Score (98)"}`

**Explanation:** Creates dynamic headers that include row counts. Useful for dashboards showing data freshness.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Lambda doesn't return a single value per column | Ensure your lambda reduces each column to one value (use SUM, MAX, COUNT, etc.) |
| `#NAME?` | LAMBDA function not recognized | Requires Excel 365/2024+; not available in earlier versions |
| `#CALC!` | Calculation error within the lambda | Debug by testing your lambda on a single column first |
| `#N/A` | Lambda returns #N/A for some columns | Add error handling: `IFERROR(expression, fallback)` inside lambda |
| `#SPILL!` | No room for the horizontal result array | Ensure enough empty cells to the right of the formula cell |

## Use Cases

### [[Multi-Column Statistics Dashboard]]

**Scenario:** Create a summary row showing key statistics for each numeric column in a data table.

**Implementation:**
```
=BYCOL(SalesData[Jan]:SalesData[Dec], LAMBDA(month,
    ROUND(AVERAGE(month), 2)
))
```

**Business Application:** Monthly sales dashboards often need summary statistics (average, total, max, min) for each month. BYCOL generates all 12 monthly averages in one formula, automatically updating when data changes.

**Technical Details:** When using structured table references, BYCOL processes each column of the table. The result spills horizontally, perfectly aligning with the source columns if placed in the same row structure.

---

### [[Data Quality Validation]]

**Scenario:** Check multiple columns for data quality issues like nulls, duplicates, or out-of-range values.

**Implementation:**
```
=BYCOL(ImportedData, LAMBDA(col,
    COUNTBLANK(col) & " blanks, " &
    (ROWS(col)-ROWS(UNIQUE(col))) & " dupes"
))
```

**Business Application:** Before processing imported data, quality checks identify problematic columns. This formula creates a quality summary for each column in one pass.

**Technical Details:** The lambda returns a concatenated string with multiple metrics. While BYCOL typically returns numbers, text results work fine and can include rich diagnostic information.

---

### [[Rolling Window Calculations]]

**Scenario:** Calculate 3-month rolling averages across a year of monthly columns.

**Implementation:**
```
=BYCOL(OFFSET(MonthlyData,,SEQUENCE(1,10)-1,,3), LAMBDA(window, AVERAGE(window)))
```

**Business Application:** Financial analysis often requires rolling metrics. This generates rolling 3-month averages across 12 months of data, producing 10 rolling values.

**Technical Details:** OFFSET with SEQUENCE creates sliding 3-column windows. BYCOL then averages each window. This dynamic approach adjusts automatically as months are added.

## Platform Differences

### Microsoft Excel (365/2024+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Complex lambdas | Full support |
| Nested with BYROW | Full support |
| Named LAMBDA functions | Full support |

### Google Sheets

| Feature | Support |
|---------|---------|
| BYCOL | NOT AVAILABLE |
| Alternative | Use BYROW with TRANSPOSE, or array formulas |

**Workaround for Google Sheets:**
```
=TRANSPOSE(BYROW(TRANSPOSE(A1:D10), LAMBDA(row, SUM(row))))
```
This transposes data, uses BYROW, then transposes back. Functional but less elegant.

### Other Platforms

BYCOL is exclusive to Microsoft Excel 365 and Excel 2024+. It is not available in:
- Google Sheets (different LAMBDA implementation)
- LibreOffice Calc
- Apple Numbers
- Excel Online (requires 365 subscription)
- Excel 2019 and earlier

## Tips and Best Practices

1. **Always return a single value from your lambda:** BYCOL expects each lambda call to produce one result. If your lambda returns an array, only the first element is used.

2. **Test your lambda independently first:** Before wrapping in BYCOL, test your calculation on a single column: `=LAMBDA(c, SUM(c))(A1:A10)`. This helps debug issues.

3. **Use named ranges for cleaner formulas:** `=BYCOL(SalesData, LAMBDA(c, AVERAGE(c)))` is more readable than cell references.

4. **Consider BYROW for row operations:** If you need to process rows instead of columns, BYROW is the counterpart function.

5. **Combine with HSTACK/VSTACK for complex layouts:** BYCOL results can be stacked with other arrays to build comprehensive summary tables.

6. **Add error handling within the lambda:** `LAMBDA(c, IFERROR(AVERAGE(c), 0))` prevents one bad column from breaking the entire result.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BYROW]] | Applies lambda to each row | When you need row-by-row processing |
| [[MAP]] | Applies lambda to each element | When processing individual cells, not entire rows/columns |
| [[REDUCE]] | Accumulates values across array | When you need a single result from an entire array |
| [[SCAN]] | Like REDUCE but returns intermediate results | When you need running totals or cumulative calculations |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the column processing logic

*Define reusable column processor:*
```
=BYCOL(Data, LAMBDA(c, PERCENTILE(c, 0.9)))
```
Every BYCOL requires a LAMBDA. Name frequently-used lambdas for reuse.

---

**[[HSTACK]]** - Combine BYCOL results with other arrays

*Add row labels to statistics:*
```
=HSTACK({"Sum";"Average";"Max"}, VSTACK(BYCOL(Data, LAMBDA(c,SUM(c))), BYCOL(Data, LAMBDA(c,AVERAGE(c))), BYCOL(Data, LAMBDA(c,MAX(c)))))
```
Builds a summary table with labels and multiple BYCOL results.

---

**[[BYROW]]** - Process rows while BYCOL processes columns

*Row and column summaries together:*
```
Column totals: =BYCOL(A1:D10, LAMBDA(c, SUM(c)))
Row totals: =BYROW(A1:D10, LAMBDA(r, SUM(r)))
```
Use both to create summary rows AND columns for a data table.

---

**[[MAKEARRAY]]** - Generate arrays for BYCOL to process

*Create and process a calculated array:*
```
=BYCOL(MAKEARRAY(5,5,LAMBDA(r,c,r*c)), LAMBDA(col, SUM(col)))
```
MAKEARRAY generates the source array, BYCOL summarizes each column.

## Official Documentation

- **Microsoft Excel:** [BYCOL function](https://support.microsoft.com/en-us/office/bycol-function-2dc5af2a-b19b-4998-8b83-7d5cc8aa588d)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | March 2022 | Released with LAMBDA helper functions |
| Excel 2024 | 2024 | Included in perpetual license version |
| Excel Online | 2022 | Full support in web version |
| Earlier versions | Not available | Requires LAMBDA support, not in Excel 2019 or earlier |

---

*Last updated: 2026-01-10*
