---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- dynamic-arrays
- array-manipulation
subTopics:
- row-selection
- array-extraction
- data-reshaping
- row-reordering
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Choose Rows
- Row Selector
- Extract Rows
- Pick Rows
tags:
- rows
- dynamic-arrays
- extraction
- array-manipulation
- excel-365
---

# CHOOSEROWS

## Description

**CHOOSEROWS** extracts specific rows from an array or range by their row numbers. Specify which rows you want and in what order, and CHOOSEROWS returns a new array containing just those rows. Need rows 1, 5, and 10 from a 100-row dataset? Just `=CHOOSEROWS(Data, 1, 5, 10)`. This function provides precise row selection without complex INDEX formulas or helper columns.

The function transforms how we work with row-based data extraction. Before CHOOSEROWS, extracting specific non-contiguous rows required multiple INDEX formulas vertically stacked, or complex array formulas with ROW and IF. Now a single function handles row selection, reordering, and even row duplication. Need the last row first? CHOOSEROWS does that with negative indexing.

**CHOOSEROWS in Google Sheets:** Google Sheets added CHOOSEROWS in 2022, providing identical functionality to Excel. The syntax, behavior, and array handling are the same across platforms. Formulas using CHOOSEROWS are fully portable between Excel 365/2021 and Google Sheets. Both platforms support negative row numbers for counting from the end.

**Complement to CHOOSECOLS:** CHOOSEROWS is the row-focused counterpart to CHOOSECOLS, which extracts specific columns. Together, they provide complete control over array subsetting. Use CHOOSEROWS to select rows, CHOOSECOLS to select columns, and combine them for precise row-column intersection extraction.

## Syntax

> [!f(x)] CHOOSEROWS Syntax
>
> ```
> =CHOOSEROWS(array, row_num1, [row_num2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to extract rows. Can be a cell range, named range, or array returned by another function. |
| `row_num1` | Yes | The first row number to return. Positive numbers count from top (1=first row), negative numbers count from bottom (-1=last row). |
| `row_num2, ...` | No | Additional row numbers to include. Order specified is order returned. Same row can be listed multiple times. Up to 253 row arguments supported. |

### Return Value

Returns a new array containing only the specified rows in the order listed. Column count matches the source array. Row count equals the number of row arguments provided.

## Examples

> [!f(x)] CHOOSEROWS Examples

### Example 1: Extract Single Row
```
=CHOOSEROWS(A1:E10, 5)
```
**Result:** Only the fifth row as a single-row array

**Explanation:** Simplest use case - extract one row from a multi-row range. Returns all columns from that row.

---

### Example 2: Extract Multiple Specific Rows
```
=CHOOSEROWS(A1:E100, 1, 10, 50, 100)
```
**Result:** Four-row array containing rows 1, 10, 50, and 100

**Explanation:** Non-contiguous row selection in one formula. Perfect for sampling or extracting specific records.

---

### Example 3: Extract Header and Specific Data Rows
```
=CHOOSEROWS(DataTable, 1, 15, 30, 45)
```
**Result:** Header row plus three specific data rows

**Explanation:** Common pattern - include header (row 1) with selected data rows for reports or analysis.

---

### Example 4: Using Negative Numbers (From End)
```
=CHOOSEROWS(A1:E100, -1)
```
**Result:** The last row of the array

**Explanation:** Negative numbers count from the end. -1 is last row, -2 is second-to-last. Useful when row count varies.

---

### Example 5: Get First and Last Rows
```
=CHOOSEROWS(Data, 1, -1)
```
**Result:** Two-row array with first and last rows

**Explanation:** Combine positive and negative indexing. Great for seeing range of values or data boundaries.

---

### Example 6: Reverse Row Order
```
=CHOOSEROWS(A1:A5, -1, -2, -3, -4, -5)
```
**Result:** All five rows in reverse order

**Explanation:** Negative numbers in decreasing order reverses the array. Last row becomes first, etc.

---

### Example 7: Duplicate Rows
```
=CHOOSEROWS(A1:E3, 1, 2, 2, 3)
```
**Result:** Four-row array where row 2 appears twice

**Explanation:** Same row can be referenced multiple times. Useful for creating templates or specific layouts.

---

### Example 8: Extract from Sorted Results
```
=CHOOSEROWS(SORT(A1:E100, 3, -1), 1, 2, 3)
```
**Result:** Top 3 rows after sorting by column 3 descending

**Explanation:** CHOOSEROWS works on function results. Sort first, then extract top rows. Simpler than TAKE for specific counts.

---

### Example 9: With FILTER Results
```
=CHOOSEROWS(FILTER(Data, Criteria), 1, -1)
```
**Result:** First and last rows of filtered data

**Explanation:** Apply CHOOSEROWS to filtered results. See the first and last matching records.

---

### Example 10: Dynamic Row Selection
```
=CHOOSEROWS(Data, SEQUENCE(5))
```
**Result:** First five rows

**Explanation:** Row numbers can come from formulas. SEQUENCE(5) produces {1;2;3;4;5}, selecting first five rows.

---

### Example 11: Sample Every 10th Row
```
=CHOOSEROWS(Data, SEQUENCE(10, 1, 1, 10))
```
**Result:** Rows 1, 11, 21, 31, ... (every 10th row, first 10 occurrences)

**Explanation:** SEQUENCE generates 1, 11, 21... for systematic sampling. Adjust step parameter for different intervals.

---

### Example 12: Combine with VSTACK
```
=VSTACK(CHOOSEROWS(Table1, 1), CHOOSEROWS(Table2, 1))
```
**Result:** Headers from two tables stacked vertically

**Explanation:** Extract specific rows from multiple sources, then combine. Useful for creating summary views.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Row number is 0 | Row numbers must be positive (1+) or negative (-1 and below), not zero |
| `#VALUE!` | Row number exceeds array rows | Ensure row numbers are within array bounds (use negative for end-relative) |
| `#SPILL!` | Not enough room for result array | Clear cells in the spill range |
| `#REF!` | Array reference is invalid | Verify the source array exists and is accessible |
| `#CALC!` | Result array too large | Reduce the number of rows or source array size |

## Use Cases

### [[Header Preservation with Data Sampling]]

**Scenario:** Large dataset needs to be sampled for testing, but header row must always be included.

**Implementation:**
```
=CHOOSEROWS(FullData, 1, SEQUENCE(50, 1, 2, 10)+0)
```
Returns header (row 1) plus every 10th row starting from row 2.

**Business Application:** Create representative test datasets that maintain proper structure. Header row ensures column identification in the sample.

**Technical Details:** SEQUENCE generates sample row numbers. Add 0 to convert SEQUENCE array to values for CHOOSEROWS. First parameter is count, fourth is step.

---

### [[Top and Bottom Analysis]]

**Scenario:** Compare best and worst performers from sorted data without displaying all middle records.

**Implementation:**
```
=LET(
  sorted, SORT(SalesData, 3, -1),
  top5, CHOOSEROWS(sorted, 1, 2, 3, 4, 5),
  bottom5, CHOOSEROWS(sorted, -5, -4, -3, -2, -1),
  VSTACK(top5, bottom5)
)
```

**Business Application:** Executive summaries showing extremes without overwhelming data. Focus attention on best practices and problem areas.

**Technical Details:** Sort once, then use CHOOSEROWS twice with positive and negative indices. VSTACK combines results.

---

### [[Specific Record Lookup]]

**Scenario:** Dashboard needs to display specific records by their position in a sorted or filtered list.

**Implementation:**
```
=CHOOSEROWS(FILTER(Orders, Orders[Status]="Pending"), 1, 2, 3, 4, 5)
```
Returns first 5 pending orders.

**Business Application:** Priority lists showing only the most relevant records. Dashboard widgets with limited display space.

**Technical Details:** FILTER reduces data first, then CHOOSEROWS extracts specific positions. Consider TAKE for simpler "first N" cases.

---

### [[Template Row Duplication]]

**Scenario:** Create a report template where certain format rows need to repeat for each data section.

**Implementation:**
```
=CHOOSEROWS(Template, 1, 2, 2, 3, 2, 2, 4)
```
Returns rows with row 2 repeated multiple times.

**Business Application:** Generate structured reports with repeating sections. Maintain consistent formatting through row duplication.

**Technical Details:** Same row number can appear multiple times in arguments. Useful for forms, templates, or structured documents.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Maximum rows:** 253 row arguments
- **Negative numbers:** Fully supported for end-relative counting
- **Array behavior:** Automatic spilling

### Google Sheets
- **Availability:** Added in 2022
- **Maximum rows:** Same 253 argument limit
- **Negative numbers:** Fully supported
- **Array behavior:** Identical spill behavior

### Key Differences
CHOOSEROWS works identically across both platforms. Formulas are fully portable between Excel 365/2021 and Google Sheets. The main compatibility issue is with older Excel versions (2019 and earlier) where CHOOSEROWS doesn't exist.

### Alternatives for Older Excel
Before CHOOSEROWS, extracting specific rows required:
- INDEX with row number: `=INDEX(Data, 5, 0)` for single row
- VSTACK with multiple INDEX calls for multiple rows
- Complex array formulas with ROW and IF

## Tips and Best Practices

1. **Use negative indices for end-relative selection:** -1 for last row, -2 for second-to-last. Formulas work regardless of data size changes.

2. **Include headers explicitly:** When extracting data rows, add row 1 first if you need column headers: `=CHOOSEROWS(Data, 1, 5, 10)`.

3. **Combine with SEQUENCE for patterns:** `=CHOOSEROWS(Data, SEQUENCE(5,1,10,5))` gets rows 10, 15, 20, 25, 30.

4. **Order determines output order:** Rows appear in the order listed. Use this for custom sorting without SORT.

5. **Pair with CHOOSECOLS:** CHOOSEROWS for rows, CHOOSECOLS for columns. Use both for precise cell extraction.

6. **Consider TAKE for sequential rows:** `=TAKE(Data, 5)` is simpler than CHOOSEROWS for first 5 rows. Use CHOOSEROWS for non-sequential selection.

7. **Works with any array:** CHOOSEROWS accepts FILTER, SORT, or any array function output. Build extraction pipelines.

8. **Zero is invalid:** Row 0 causes #VALUE! error. Rows are 1-indexed (first row is 1, not 0).

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CHOOSECOLS]] | Extracts specific columns by number | When you need to select columns, not rows |
| [[TAKE]] | Takes first/last rows or columns | When extracting sequential rows from start or end |
| [[DROP]] | Removes first/last rows or columns | When removing rows is easier than selecting them |
| [[INDEX]] | Returns value at specific position | When you need a single cell value, not entire row |

### Commonly Used Together

**[[CHOOSECOLS]]** - Column selection counterpart

*Extract specific rows and columns:*
```
=CHOOSECOLS(CHOOSEROWS(Data, 1, 5, 10), 2, 4)
```
First select rows, then columns from the result.

---

**[[VSTACK]]** - Combine row selections

*Merge rows from multiple sources:*
```
=VSTACK(CHOOSEROWS(Table1, 1), CHOOSEROWS(Table2, 1))
```
Stack rows from different tables vertically.

---

**[[FILTER]]** - Row filtering before selection

*Filter then select positions:*
```
=CHOOSEROWS(FILTER(Data, Criteria), 1, 2, 3)
```
Reduce to matching rows first, then pick specific positions.

---

**[[SORT]]** - Sort before row extraction

*Sort then select top/bottom:*
```
=CHOOSEROWS(SORT(Data, 2, -1), 1, 2, 3, -3, -2, -1)
```
Sort by column 2, get top 3 and bottom 3.

---

**[[SEQUENCE]]** - Dynamic row numbers

*Select every Nth row:*
```
=CHOOSEROWS(Data, SEQUENCE(10, 1, 1, 5))
```
Rows 1, 6, 11, 16... (every 5th row, 10 times).

---

**[[LET]]** - Named intermediate results

*Readable multi-step extraction:*
```
=LET(sorted, SORT(Data,3), rows, CHOOSEROWS(sorted,1,2,3), rows)
```
Break complex operations into named steps.

## Official Documentation

- **Microsoft Excel:** [CHOOSEROWS function](https://support.microsoft.com/en-us/office/chooserows-function-51ace882-9bab-4a44-9625-7274ef7507a3)
- **Google Sheets:** [CHOOSEROWS function](https://support.google.com/docs/answer/13196657)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions release |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for web | 2022 | Full feature parity |
| Google Sheets | 2022 | Added to match Excel functionality |
| Excel 2019 | Not available | Must use INDEX/VSTACK combinations |

---

*Last updated: 2026-01-10*
