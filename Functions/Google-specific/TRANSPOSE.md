---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - array-manipulation
  - data-reshaping
subTopics:
  - matrix-operations
  - row-column-swap
  - data-orientation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - flip rows columns
  - rotate array
  - matrix transpose
tags:
  - google-sheets-only
  - arrays
  - matrix
  - transformation
  - layout
---

# TRANSPOSE

## Description

**TRANSPOSE** is a fundamental array manipulation function in Google Sheets that swaps the rows and columns of a given range or array. What was a row becomes a column, and what was a column becomes a row. If you have a 3x5 array (3 rows, 5 columns), TRANSPOSE converts it to a 5x3 array (5 rows, 3 columns). The element that was at position (row 2, column 3) moves to position (row 3, column 2). This operation is essential for data restructuring, enabling you to convert horizontal data layouts to vertical ones and vice versa, which is often necessary for analysis, charting, or compatibility with functions that expect data in a specific orientation.

The primary use cases for TRANSPOSE include converting row-based data to column-based (or vice versa) for analysis, reshaping data for charting tools that expect specific orientations, preparing data for functions like VLOOKUP that require vertical lookup ranges, and performing matrix operations in mathematical or scientific applications. For example, if you receive data with dates across columns (Jan, Feb, Mar as column headers), but your charts expect dates in rows, TRANSPOSE quickly restructures the data. TRANSPOSE is also crucial in linear algebra contexts where matrix transposition is a fundamental operation for calculations like matrix multiplication or creating symmetric matrices.

There are several important considerations when working with TRANSPOSE. First, TRANSPOSE swaps dimensions entirely; a single-row range becomes a single-column range and vice versa. Second, the output requires sufficient empty cells to spill into; a 5x10 range transposed to 10x5 needs 10 rows and 5 columns of clear space. Third, TRANSPOSE creates a dynamic link to the source data; changes in the source automatically reflect in the transposed output. Fourth, cell formatting does not transfer; the transposed cells adopt the formatting of their destination cells. Fifth, for very large arrays, TRANSPOSE may impact calculation performance, though it is generally efficient for typical spreadsheet sizes.

From a platform perspective, TRANSPOSE is available in both Google Sheets and Microsoft Excel, and has been for many years in both platforms. It is one of the oldest array functions, predating the modern dynamic array capabilities. The syntax is identical between platforms, making formulas fully portable. Unlike newer functions like LAMBDA or BYROW, TRANSPOSE works in older Excel versions as well, though the spilling behavior differs: in legacy Excel, TRANSPOSE required Ctrl+Shift+Enter for array entry and pre-selected output range, while in Excel 365 and Google Sheets it spills automatically.

## Syntax

> [!f(x)] TRANSPOSE Syntax
>
> ```
> =TRANSPOSE(array_or_range)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array_or_range` | Yes | The array or range to transpose. Can be a range reference (A1:E10), a named range, the result of another function that returns an array, or a literal array using curly braces. Single cells are valid but produce no visible change. |

### Return Value

Returns an array where rows and columns are swapped. An input of R rows by C columns produces output of C rows by R columns. Cell at input position (i, j) appears at output position (j, i). The output automatically spills into adjacent cells.

## Examples

> [!f(x)] TRANSPOSE Examples

### Example 1: Transpose a Column to Row
```
=TRANSPOSE(A1:A5)
```
**Result:** Converts vertical data in A1:A5 into a horizontal row

**Explanation:** Five cells in a column become five cells in a row. This is the simplest transpose use case, converting vertical lists to horizontal.

---

### Example 2: Transpose a Row to Column
```
=TRANSPOSE(A1:E1)
```
**Result:** Converts horizontal data in A1:E1 into a vertical column

**Explanation:** Five cells in a row become five cells in a column. Useful when you need to stack horizontal data vertically.

---

### Example 3: Transpose a Table
```
=TRANSPOSE(A1:D10)
```
**Result:** A 10x4 table becomes a 4x10 table

**Explanation:** What were 10 rows become 10 columns, and 4 columns become 4 rows. Headers that were in row 1 now appear in column 1.

---

### Example 4: Transpose with Headers
```
=TRANSPOSE(A1:E6)
```
**Result:** Table with column headers becomes table with row headers

**Explanation:** If A1:E1 contained "Name, Age, City, Phone, Email", these now appear in A1:A5 as row labels. The data rotates accordingly.

---

### Example 5: Combined with FILTER
```
=TRANSPOSE(FILTER(A2:A100, B2:B100 = "Active"))
```
**Result:** Filtered column results displayed as a row

**Explanation:** FILTER returns a vertical list of matching values; TRANSPOSE converts this to horizontal. Useful for creating comma-separated lists or horizontal displays.

---

### Example 6: Transpose for VLOOKUP Compatibility
```
=VLOOKUP(D1, TRANSPOSE(1:2), 2, FALSE)
```
**Result:** Looks up values in transposed data (turning columns to rows)

**Explanation:** If your lookup table is horizontal (values in row 1, results in row 2), transpose it for VLOOKUP which expects vertical structure.

---

### Example 7: Create Column Headers from Row Data
```
=TRANSPOSE(UNIQUE(B2:B100))
```
**Result:** Unique values from a column displayed as column headers in a row

**Explanation:** UNIQUE returns unique values vertically; TRANSPOSE spreads them horizontally. Useful for building dynamic headers.

---

### Example 8: Matrix Transpose for Calculations
```
=MMULT(A1:B3, TRANSPOSE(A1:B3))
```
**Result:** Matrix multiplied by its transpose (creates symmetric matrix)

**Explanation:** In linear algebra, multiplying a matrix by its transpose is a common operation. TRANSPOSE provides the transposed matrix for MMULT.

---

### Example 9: Combine Transposed Arrays
```
=HSTACK(TRANSPOSE(A1:A5), TRANSPOSE(B1:B5))
```
**Result:** Two columns placed side by side after transposition

**Explanation:** Each column becomes a row; HSTACK combines them horizontally. Creates a 2x5 array from two 5x1 arrays.

---

### Example 10: Reverse Row and Column Orientation for Charts
```
=TRANSPOSE(ChartData)
```
**Result:** Data restructured for charting

**Explanation:** Many charts expect data in specific orientations. If your chart expects categories in columns but you have them in rows, TRANSPOSE fixes the layout.

---

### Example 11: Transpose Query Results
```
=TRANSPOSE(QUERY(Data, "SELECT A, SUM(B) GROUP BY A"))
```
**Result:** Query summary displayed horizontally instead of vertically

**Explanation:** QUERY returns grouped data vertically; TRANSPOSE displays it horizontally. Useful for dashboard layouts.

---

### Example 12: Build Horizontal Lists
```
=TEXTJOIN(", ", TRUE, TRANSPOSE(A1:A10))
```
**Result:** Comma-separated string of transposed values

**Explanation:** While TEXTJOIN works on columns directly, this demonstrates combining TRANSPOSE with other functions. The transpose does not change TEXTJOIN behavior but shows function chaining.

---

### Example 13: Dynamic Transpose with OFFSET
```
=TRANSPOSE(OFFSET(A1, 0, 0, COUNTA(A:A), 1))
```
**Result:** Transposes a dynamically-sized column

**Explanation:** OFFSET creates a reference to the populated portion of column A; TRANSPOSE converts it to a row. Adapts automatically as data grows.

---

### Example 14: Cross-Tab Conversion
```
=TRANSPOSE(B2:M2)
```
**Result:** Monthly values in columns become monthly values in rows

**Explanation:** Common in financial reports where months are columns (Jan-Dec in B-M). Transpose restructures for analysis that expects time in rows.

---

### Example 15: Nested Transpose (Identity Operation)
```
=TRANSPOSE(TRANSPOSE(A1:D10))
```
**Result:** Original array unchanged

**Explanation:** Transposing twice returns the original orientation. Demonstrates that TRANSPOSE is its own inverse. Useful for understanding the operation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Output would spill into cells with existing data | Clear cells where the transposed output needs to appear. A 5x10 input needs 10 rows and 5 columns clear. |
| `#VALUE!` | Array argument is invalid or contains errors | Check the source range for errors. If source has #N/A or other errors, they propagate to output. |
| `#REF!` | Source range is invalid or deleted | Verify the source range exists. Named ranges might have been deleted or renamed. |
| Array spill issue | In legacy Excel, not entered as array formula | Use Ctrl+Shift+Enter in older Excel versions. In Excel 365 and Google Sheets, this is automatic. |
| Unexpected size | Misunderstanding row/column swap | Remember: RxC input produces CxR output. 3 rows x 10 columns becomes 10 rows x 3 columns. |
| Formatting issues | Transposed cells have wrong formatting | Cell formatting stays with destination cells, not source. Manually format transposed area if needed. |

## Use Cases

### [[Data Restructuring: Converting Report Formats]]

**Scenario:** A finance team receives monthly reports where dates are column headers (Jan, Feb, Mar...) but their analysis tools and charts expect dates as row headers with metrics across columns.

**Implementation:**
```
=TRANSPOSE(A1:M13)
```
Where A1:M13 contains the monthly report with months in B1:M1 and row headers in A1:A13

**Business Application:** Instantly restructures reports from column-based months to row-based months. Finance can now analyze trends more easily, as each row represents a time period. Charts automatically work with time on the x-axis. This saves hours of manual data restructuring.

**Technical Details:** The 13x13 array (including headers) transposes to a new 13x13 array with swapped orientations. Column headers become row headers. Consider using HSTACK/VSTACK to add new headers if needed.

---

### [[Data Analysis: Preparing Data for VLOOKUP]]

**Scenario:** An analyst has a reference table where categories are in the first row and values are in the second row (horizontal layout), but needs to use VLOOKUP which requires the lookup values in the first column.

**Implementation:**
```
=VLOOKUP(SearchValue, TRANSPOSE(ReferenceTable), 2, FALSE)
```
Or transpose the table separately:
```
=TRANSPOSE(B1:G2)
```
Then use VLOOKUP on the transposed result

**Business Application:** Enables use of VLOOKUP without restructuring source data permanently. The reference table can remain in its natural horizontal format while lookups work correctly. Useful when you cannot control the format of source data.

**Technical Details:** TRANSPOSE converts the 2-row reference to a 2-column reference compatible with VLOOKUP. For large reference tables, consider creating a separate transposed version to avoid recalculating on each lookup.

---

### [[Dashboard Design: Flexible Data Layout]]

**Scenario:** A dashboard designer needs to display the same data in different orientations for different views: a horizontal summary bar and a vertical detail list.

**Implementation:**
```
=TRANSPOSE(SummaryData)
=QUERY(TRANSPOSE(MetricsTable), "SELECT * WHERE Col1 > 0")
```

**Business Application:** Creates flexible dashboard layouts without duplicating source data. The same metrics can appear horizontally in a summary section and vertically in a detailed table. Changes to source data update both views automatically.

**Technical Details:** TRANSPOSE works with QUERY, FILTER, and other functions to create dynamic views. Consider using named ranges for source data to make formulas more readable and maintainable.

---

### [[Survey Analysis: Response Matrix Restructuring]]

**Scenario:** Survey results come with respondents in rows and questions in columns, but analysis requires questions in rows and respondents in columns for certain statistical operations.

**Implementation:**
```
=TRANSPOSE(SurveyResponses)
```
Optionally combined with aggregation:
```
=BYCOL(TRANSPOSE(B2:K100), LAMBDA(col, AVERAGE(col)))
```

**Business Application:** Enables flexible analysis of survey data. Transpose once for question-centric analysis (how did everyone answer question 3?), or keep original for respondent-centric analysis (how did person A answer all questions?). Statistical functions can be applied in either orientation.

**Technical Details:** After transposing, BYCOL calculates averages per original respondent (now columns). The transpose changes the natural aggregation axis, enabling different analytical perspectives.

## Platform Differences

### Microsoft Excel

- **Availability:** TRANSPOSE is available in all versions of Excel
- **Syntax:** Identical to Google Sheets
- **Legacy behavior (Excel 2019 and earlier):** Requires Ctrl+Shift+Enter to enter as array formula, and you must pre-select the output range
- **Excel 365 behavior:** Automatic spilling like Google Sheets; no special entry required
- **Alternative:** Copy and Paste Special > Transpose (manual, one-time operation)

### Google Sheets

- **Availability:** Available in all versions of Google Sheets
- **Specific Behavior:** Automatic spilling; results expand into adjacent cells
- **Dynamic updates:** Transposed output updates automatically when source changes
- **Integration:** Works with all Google Sheets functions including QUERY, FILTER, and array literals

### Key Differences

| Feature | Google Sheets | Excel 365 | Excel 2019 and earlier |
|---------|---------------|-----------|------------------------|
| Auto-spill | Yes | Yes | No (requires CSE) |
| Syntax | =TRANSPOSE(range) | =TRANSPOSE(range) | {=TRANSPOSE(range)} |
| Dynamic updates | Yes | Yes | Yes |
| Pre-select output | Not required | Not required | Required |
| Performance | Good | Good | Good |

## Tips and Best Practices

1. **Plan for Output Space:** Before transposing, ensure sufficient empty cells for the output. A 5x20 range needs 20 rows and 5 columns clear for the transposed result.

2. **Use Named Ranges:** Instead of `=TRANSPOSE(A1:M13)`, define a named range like "MonthlyData" and use `=TRANSPOSE(MonthlyData)`. This improves readability and maintenance.

3. **Combine with Dynamic Functions:** TRANSPOSE works well with FILTER, UNIQUE, SORT, and other dynamic array functions. Transpose results to fit your layout needs.

4. **Remember Formatting Does Not Transfer:** Transposed cells inherit destination cell formatting. Apply formatting after transposing or format the destination area in advance.

5. **Double Transpose Returns Original:** `=TRANSPOSE(TRANSPOSE(A1:C10))` returns the original orientation. Useful for understanding the operation, but rarely needed in practice.

6. **Consider Paste Special for One-Time Operations:** If you need a static transposed copy (not dynamic), use Copy > Paste Special > Transpose. This creates values, not formulas.

7. **Use for Chart Data Preparation:** Many charts expect specific data orientations. TRANSPOSE can quickly restructure data without rearranging source data.

8. **Single Row/Column Behavior:** Transposing a single row creates a single column and vice versa. This is the most common use case.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| Paste Special > Transpose | Manual one-time transpose | When you need static values, not a dynamic formula link |
| [[WRAPCOLS]] / [[WRAPROWS]] | Reshape arrays by wrapping | When converting 1D to 2D with specific dimensions |
| [[TOCOL]] / [[TOROW]] | Convert array to single column/row | When flattening multi-dimensional arrays |
| Custom reshaping | Using INDEX with arithmetic | For complex reshaping beyond simple transpose |

### Commonly Used Together

**[[FILTER]]** - Filter then transpose results

*Convert filtered column to row:*
```
=TRANSPOSE(FILTER(A2:A100, B2:B100 = "Active"))
```
Active items displayed horizontally instead of vertically.

---

**[[UNIQUE]]** - Create horizontal unique list

*Unique values as column headers:*
```
=TRANSPOSE(UNIQUE(B2:B100))
```
Unique categories become horizontal headers for pivot-style layouts.

---

**[[SORT]]** - Sort then transpose

*Sorted data in alternative orientation:*
```
=TRANSPOSE(SORT(A1:A20))
```
Sorted vertical list displayed as horizontal row.

---

**[[QUERY]]** - Restructure query results

*Query results in horizontal format:*
```
=TRANSPOSE(QUERY(Data, "SELECT A, SUM(B) GROUP BY A"))
```
Grouped sums displayed as row instead of column.

---

**[[MMULT]]** - Matrix multiplication with transpose

*Multiply matrix by its transpose:*
```
=MMULT(A1:C3, TRANSPOSE(A1:C3))
```
Common linear algebra operation for creating symmetric matrices.

---

**[[HSTACK]] / [[VSTACK]]** - Combine with transposed arrays

*Build complex layouts:*
```
=VSTACK(Headers, TRANSPOSE(DataRow))
```
Add headers above transposed data for complete table.

## Official Documentation

- **Google Sheets:** [TRANSPOSE function](https://support.google.com/docs/answer/3094262)
- **Microsoft Excel:** [TRANSPOSE function](https://support.microsoft.com/en-us/office/transpose-function-ed039415-ed8a-4a81-93e9-4b6dfac76027)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | Original launch | Core function available since inception |
| Excel | Excel 2000 or earlier | Long-standing function; array formula in legacy versions |
| Excel 365 | 2019+ | Added dynamic array spilling behavior |
| Excel Online | Available | Works with dynamic arrays |

---

*Last updated: 2026-01-10*
