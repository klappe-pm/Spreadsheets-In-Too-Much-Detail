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
- column-selection
- array-extraction
- data-reshaping
- column-reordering
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Choose Columns
- Column Selector
- Extract Columns
- Pick Columns
tags:
- columns
- dynamic-arrays
- extraction
- array-manipulation
- excel-365
---

# CHOOSECOLS

## Description

**CHOOSECOLS** extracts specific columns from an array or range by their column numbers. Specify which columns you want and in what order, and CHOOSECOLS returns a new array containing just those columns. Want columns 1, 3, and 5 from a 10-column dataset? Just `=CHOOSECOLS(Data, 1, 3, 5)`. This function provides surgical precision for column selection without complex INDEX formulas or helper columns.

The function revolutionizes how we work with multi-column data. Before CHOOSECOLS, extracting non-contiguous columns required multiple INDEX formulas horizontally stacked, or complex array formulas. Now a single function call handles column selection, reordering, and even column duplication. Need column 5 first, then column 1? CHOOSECOLS does that. Need column 3 repeated twice? CHOOSECOLS handles that too.

**CHOOSECOLS in Google Sheets:** Google Sheets added CHOOSECOLS in 2022, providing identical functionality to Excel. The syntax, behavior, and array handling are the same across platforms. Formulas using CHOOSECOLS are fully portable between Excel 365/2021 and Google Sheets. Both platforms support negative column numbers for counting from the end.

**Complement to CHOOSEROWS:** CHOOSECOLS is the column-focused counterpart to CHOOSEROWS, which extracts specific rows. Together, they provide complete control over array subsetting. Combine them to extract specific row-column intersections, or use with other array functions like FILTER, SORT, and UNIQUE to build powerful data transformation pipelines.

## Syntax

> [!f(x)] CHOOSECOLS Syntax
>
> ```
> =CHOOSECOLS(array, col_num1, [col_num2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to extract columns. Can be a cell range, named range, or array returned by another function. |
| `col_num1` | Yes | The first column number to return. Positive numbers count from left (1=first column), negative numbers count from right (-1=last column). |
| `col_num2, ...` | No | Additional column numbers to include. Order specified is order returned. Same column can be listed multiple times. Up to 253 column arguments supported. |

### Return Value

Returns a new array containing only the specified columns in the order listed. Row count matches the source array. Column count equals the number of column arguments provided.

## Examples

> [!f(x)] CHOOSECOLS Examples

### Example 1: Extract Single Column
```
=CHOOSECOLS(A1:E10, 3)
```
**Result:** Only the third column (C1:C10) as a standalone array

**Explanation:** Simplest use case - extract one column from a multi-column range. Similar to INDEX with column argument, but more readable.

---

### Example 2: Extract Multiple Specific Columns
```
=CHOOSECOLS(A1:F20, 1, 3, 5)
```
**Result:** Three-column array containing columns 1, 3, and 5

**Explanation:** Non-contiguous column selection in one formula. No need for multiple INDEX calls or HSTACK combinations.

---

### Example 3: Reorder Columns
```
=CHOOSECOLS(A1:D10, 4, 3, 2, 1)
```
**Result:** All four columns in reverse order

**Explanation:** Column order in output matches argument order. Column 4 becomes first, column 1 becomes last. Instant column reordering.

---

### Example 4: Using Negative Numbers (From End)
```
=CHOOSECOLS(A1:E10, -1, -2)
```
**Result:** Last two columns in reverse order (E then D)

**Explanation:** Negative numbers count from the end. -1 is last column, -2 is second-to-last. Useful when column count varies.

---

### Example 5: Mix Positive and Negative
```
=CHOOSECOLS(Data, 1, 2, -1)
```
**Result:** First column, second column, and last column

**Explanation:** Combine counting from start and end. Gets first two columns plus the last, regardless of how many columns exist in Data.

---

### Example 6: Duplicate a Column
```
=CHOOSECOLS(A1:C10, 1, 2, 2, 3)
```
**Result:** Four-column array where column 2 appears twice

**Explanation:** Same column can be referenced multiple times. Useful for creating calculated column placeholders or specific layouts.

---

### Example 7: Extract from Function Result
```
=CHOOSECOLS(SORT(A1:E100, 3, -1), 1, 3)
```
**Result:** Columns 1 and 3 from the sorted data

**Explanation:** CHOOSECOLS works on any array, including results from other functions. Sort the data, then extract only the columns you need.

---

### Example 8: With FILTER
```
=CHOOSECOLS(FILTER(A1:E100, C1:C100>1000), 1, 2)
```
**Result:** First two columns from filtered rows only

**Explanation:** Chain FILTER and CHOOSECOLS for precise data extraction. Filter rows first, then select specific columns from the result.

---

### Example 9: Dynamic Column Selection
```
=CHOOSECOLS(Data, SEQUENCE(3))
```
**Result:** First three columns

**Explanation:** Column numbers can come from formulas. SEQUENCE(3) produces {1;2;3}, selecting first three columns dynamically.

---

### Example 10: Select Columns from Named Range
```
=CHOOSECOLS(SalesData, 1, 5, 7)
```
**Result:** Columns 1, 5, and 7 from the named range

**Explanation:** Works with named ranges for cleaner, more maintainable formulas. The named range can be a table or standard range.

---

### Example 11: Exclude Middle Columns
```
=CHOOSECOLS(A1:G10, 1, 2, 6, 7)
```
**Result:** First two and last two columns, skipping columns 3-5

**Explanation:** Select specific columns to exclude middle ones. Alternative to DROP when you need non-contiguous exclusion.

---

### Example 12: Combine with HSTACK for Complex Layouts
```
=HSTACK(CHOOSECOLS(Table1, 1, 2), CHOOSECOLS(Table2, 3, 4))
```
**Result:** Columns 1-2 from Table1 next to columns 3-4 from Table2

**Explanation:** Use CHOOSECOLS to extract specific columns from different sources, then HSTACK to combine them horizontally.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Column number is 0 | Column numbers must be positive (1+) or negative (-1 and below), not zero |
| `#VALUE!` | Column number exceeds array columns | Ensure column numbers are within array bounds (use negative for end-relative) |
| `#SPILL!` | Not enough room for result array | Clear cells in the spill range |
| `#REF!` | Array reference is invalid | Verify the source array exists and is accessible |
| `#CALC!` | Result array too large | Reduce the number of columns or source array size |

## Use Cases

### [[Report Column Customization]]

**Scenario:** Master data table has 15 columns, but different departments need different column subsets for their reports.

**Implementation:**
```
Sales Report: =CHOOSECOLS(MasterData, 1, 3, 8, 9, 15)
Operations Report: =CHOOSECOLS(MasterData, 1, 4, 5, 6, 7)
Executive Summary: =CHOOSECOLS(MasterData, 1, 3, -2, -1)
```

**Business Application:** Maintain one source of truth while generating department-specific views. When master data updates, all reports update automatically with their respective column selections.

**Technical Details:** Each formula extracts only relevant columns. Use named ranges for the source to make formulas more readable and maintenance easier.

---

### [[Data Export Preparation]]

**Scenario:** System integration requires specific column order that differs from the source spreadsheet layout.

**Implementation:**
```
=CHOOSECOLS(CustomerData, 5, 1, 2, 8, 9, 3)
```
Reorders columns to match target system's expected format.

**Business Application:** Prepare data exports without manually copying and rearranging columns. Reduce errors in data integration by using formula-based column mapping.

**Technical Details:** Document the column mapping for maintenance. Consider using LET with named positions for clarity.

---

### [[Removing Sensitive Columns]]

**Scenario:** Share dataset externally but exclude columns containing confidential information (salary, SSN, personal data).

**Implementation:**
```
=LET(
  all_data, EmployeeTable,
  public_cols, CHOOSECOLS(all_data, 1, 2, 5, 8, 9),
  public_cols
)
```

**Business Application:** Create sanitized views for external sharing without duplicating data. Changes to source automatically reflect in the public view (minus confidential columns).

**Technical Details:** Consider naming the excluded columns in documentation. For dynamic exclusion, combine with SEQUENCE and FILTER to exclude specific column numbers.

---

### [[Column Subset for Analysis]]

**Scenario:** Statistical analysis requires only specific variables from a larger dataset with 50+ columns.

**Implementation:**
```
=CHOOSECOLS(RawData, 1, 15, 23, 24, 25, 42, 45)
```

**Business Application:** Feed only relevant variables to analysis functions. Reduce complexity and improve performance by working with smaller arrays.

**Technical Details:** Consider using array of column numbers stored in cells for flexible analysis configuration without formula changes.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Maximum columns:** 253 column arguments
- **Negative numbers:** Fully supported for end-relative counting
- **Array behavior:** Automatic spilling

### Google Sheets
- **Availability:** Added in 2022
- **Maximum columns:** Same 253 argument limit
- **Negative numbers:** Fully supported
- **Array behavior:** Identical spill behavior

### Key Differences
CHOOSECOLS works identically across both platforms. Formulas are fully portable between Excel 365/2021 and Google Sheets. The main compatibility issue is with older Excel versions (2019 and earlier) where CHOOSECOLS doesn't exist.

### Alternatives for Older Excel
Before CHOOSECOLS, extracting specific columns required:
- INDEX with column number: `=INDEX(Data,,3)` for single column
- HSTACK with multiple INDEX calls for multiple columns
- Complex array formulas with COLUMN and IF

## Tips and Best Practices

1. **Use negative numbers for end-relative selection:** When column count varies, use -1 for last column, -2 for second-to-last. Formulas stay valid as data structure changes.

2. **Combine with SEQUENCE for ranges:** `=CHOOSECOLS(Data, SEQUENCE(5))` gets first 5 columns. More flexible than hardcoding column numbers.

3. **Order matters:** Columns appear in output in the order you list them. Use this for reordering without separate steps.

4. **Pair with CHOOSEROWS:** Use CHOOSEROWS for row selection and CHOOSECOLS for columns. Together they provide complete array subsetting.

5. **Works with any array source:** CHOOSECOLS accepts results from FILTER, SORT, UNIQUE, or any array-returning function. Build transformation pipelines.

6. **Document column selections:** In complex workbooks, add comments explaining which columns map to what. Column numbers are less self-documenting than column letters.

7. **Consider TAKE for sequential columns:** If extracting first N or last N columns, TAKE may be simpler: `=TAKE(Data,,3)` for first 3 columns.

8. **Use with HSTACK for merging:** Extract columns from multiple sources and combine with HSTACK for custom layouts.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CHOOSEROWS]] | Extracts specific rows by number | When you need to select rows, not columns |
| [[TAKE]] | Takes first/last rows or columns | When extracting sequential columns from start or end |
| [[DROP]] | Removes first/last rows or columns | When removing columns is easier than selecting them |
| [[INDEX]] | Returns value at specific position | When you need a single cell value, not entire column |

### Commonly Used Together

**[[CHOOSEROWS]]** - Row selection counterpart

*Extract specific rows and columns:*
```
=CHOOSECOLS(CHOOSEROWS(Data, 1, 5, 10), 2, 4)
```
First select rows, then columns from the result.

---

**[[HSTACK]]** - Combine column selections

*Merge columns from multiple sources:*
```
=HSTACK(CHOOSECOLS(Table1, 1, 2), CHOOSECOLS(Table2, 3))
```
Build custom layouts from multiple data sources.

---

**[[FILTER]]** - Row filtering before column selection

*Filter then select columns:*
```
=CHOOSECOLS(FILTER(Data, Criteria), 1, 3, 5)
```
Reduce rows first, then extract specific columns.

---

**[[SORT]]** - Sort before column extraction

*Sort then select columns:*
```
=CHOOSECOLS(SORT(Data, 2, -1), 1, 2)
```
Sort by any column, then return only the columns you need.

---

**[[SEQUENCE]]** - Dynamic column numbers

*Select first N columns:*
```
=CHOOSECOLS(Data, SEQUENCE(5))
```
Generate column numbers programmatically.

---

**[[LET]]** - Named intermediate results

*Readable multi-step extraction:*
```
=LET(sorted, SORT(Data,3), cols, CHOOSECOLS(sorted,1,2,5), cols)
```
Break complex operations into named steps.

## Official Documentation

- **Microsoft Excel:** [CHOOSECOLS function](https://support.microsoft.com/en-us/office/choosecols-function-bf117976-2722-4466-9b9a-1c01ed9aebff)
- **Google Sheets:** [CHOOSECOLS function](https://support.google.com/docs/answer/13196659)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions release |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for web | 2022 | Full feature parity |
| Google Sheets | 2022 | Added to match Excel functionality |
| Excel 2019 | Not available | Must use INDEX/HSTACK combinations |

---

*Last updated: 2026-01-10*
