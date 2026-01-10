---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- array-manipulation
- column-selection
- data-extraction
- dynamic-arrays
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Choose Columns
- Select Columns
- Column Picker
- Extract Columns
tags:
- array
- columns
- extraction
- dynamic-arrays
- data-manipulation
---

# CHOOSECOLS

## Description

**CHOOSECOLS** extracts specific columns from an array or range and returns them in the order you specify. Think of it as a column-picking tool—you point at a table and say "give me columns 1, 4, and 2, in that order" and CHOOSECOLS delivers exactly that. It's particularly powerful for reorganizing data on the fly, extracting only the columns you need from a large dataset, or reordering columns without physically restructuring your source data. This function is part of the modern dynamic array family introduced in Excel 365 and Google Sheets.

The beauty of CHOOSECOLS lies in its flexibility: you can select any columns in any order, repeat columns, or even use negative numbers to count from the right side of the array. Need the last column without knowing how many columns exist? Use -1. Want to reverse column order? Specify them backwards. Want the same column twice for comparison purposes? Just list it twice. This makes CHOOSECOLS invaluable for creating custom views of data without altering the original structure.

**Key behaviors to understand:** CHOOSECOLS returns a dynamic array that spills into adjacent cells automatically. If you request a column number that doesn't exist (like column 10 from a 5-column range), you'll get a #VALUE! error. The function preserves all rows from the original array—it only affects which columns appear in the output. When using negative column numbers, -1 refers to the last column, -2 to the second-to-last, and so on.

**Platform availability:** CHOOSECOLS is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically, including support for negative column indices. For older Excel versions, you'll need to use INDEX with column arrays or helper formulas to achieve similar results, though less elegantly.

## Syntax

> [!f(x)] CHOOSECOLS Syntax
>
> ```
> =CHOOSECOLS(array, col_num1, [col_num2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to extract columns. Can be a cell range, named range, or array returned by another formula. |
| `col_num1` | Yes | The column number of the first column to return. Use positive numbers to count from the left (1 = first column) or negative numbers to count from the right (-1 = last column). |
| `col_num2, ...` | No | Additional column numbers to return. You can specify as many as needed. Columns are returned in the order specified. The same column can be listed multiple times. |

### Return Value

Returns an array containing the specified columns from the source array, in the order specified. The result has the same number of rows as the source array and as many columns as column numbers provided. Returns #VALUE! if any column number is 0 or refers to a non-existent column position.

## Examples

> [!f(x)] CHOOSECOLS Examples

### Example 1: Extract a Single Column
```
=CHOOSECOLS(A1:E10, 3)
```
**Result:** Returns column C (the 3rd column) as a single-column array with 10 rows

**Explanation:** The simplest use of CHOOSECOLS—extracting just one column from a range. This returns the entire third column of the range A1:E10, which is the values in C1:C10. While you could achieve the same with a simple range reference (C1:C10), CHOOSECOLS becomes powerful when the source is itself a dynamic array or when you need flexibility.

---

### Example 2: Extract Multiple Columns in Original Order
```
=CHOOSECOLS(A1:F20, 1, 3, 5)
```
**Result:** Returns columns A, C, and E as a 20-row by 3-column array

**Explanation:** This extracts columns 1, 3, and 5 from the source range, effectively removing columns B, D, and F from the output. This is useful when you have a dataset with many columns but only need specific ones for a report or analysis. The result spills into a 20x3 area.

---

### Example 3: Reorder Columns
```
=CHOOSECOLS(A1:D10, 4, 2, 3, 1)
```
**Result:** Returns columns in order D, B, C, A (reversed and rearranged)

**Explanation:** CHOOSECOLS doesn't just extract—it reorders. Here, we take a 4-column range and output the columns in a completely different order. This is invaluable when you receive data in one format but need to present it in another, without modifying the source data.

---

### Example 4: Using Negative Numbers for Right-to-Left Selection
```
=CHOOSECOLS(A1:G10, -1, -2, -3)
```
**Result:** Returns the last three columns in reverse order (G, F, E)

**Explanation:** Negative column numbers count from the right side of the array. -1 is the last column, -2 is second-to-last, and so on. This is extremely useful when you don't know (or don't want to hardcode) the total number of columns but need columns from the end.

---

### Example 5: Combine Positive and Negative Column References
```
=CHOOSECOLS(A1:H10, 1, 2, -1)
```
**Result:** Returns the first two columns and the last column (A, B, H)

**Explanation:** You can mix positive and negative references in the same formula. This grabs the identifier columns from the start of the data and the result column from the end, regardless of how many columns exist in between. If columns are added in the middle, the formula still works correctly.

---

### Example 6: Repeat a Column
```
=CHOOSECOLS(A1:D10, 1, 2, 1)
```
**Result:** Returns columns A, B, A (column A appears twice)

**Explanation:** The same column can be specified multiple times. This is useful for creating comparison layouts or when a column needs to appear in multiple positions in the output. The result is a 3-column array where columns 1 and 3 are identical.

---

### Example 7: Extract Columns from a Dynamic Array Result
```
=CHOOSECOLS(FILTER(A1:E100, B1:B100="Active"), 1, 4, 5)
```
**Result:** Returns columns 1, 4, and 5 from the filtered result

**Explanation:** CHOOSECOLS works seamlessly with other dynamic array functions. Here, FILTER returns all rows where column B equals "Active", and CHOOSECOLS then extracts only the columns we need from that filtered result. This creates a clean, focused view of the filtered data.

---

### Example 8: Create a Lookup Table with Selected Columns
```
=CHOOSECOLS(SORT(A2:F100, 1, 1), 1, 3)
```
**Result:** Returns a sorted, two-column array with just columns 1 and 3

**Explanation:** Combining SORT and CHOOSECOLS creates a clean lookup reference. SORT arranges the data by the first column, then CHOOSECOLS extracts just the columns needed for the lookup (perhaps an ID and a name), eliminating extraneous data.

---

### Example 9: Extract Columns Using SEQUENCE
```
=CHOOSECOLS(A1:J10, SEQUENCE(5))
```
**Result:** Returns the first 5 columns (equivalent to A1:E10)

**Explanation:** You can use SEQUENCE or other array functions to generate the column numbers dynamically. SEQUENCE(5) produces {1;2;3;4;5}, so CHOOSECOLS returns columns 1 through 5. This is useful when the number of columns to extract is itself calculated.

---

### Example 10: Select Every Other Column
```
=CHOOSECOLS(A1:H10, 1, 3, 5, 7)
```
**Result:** Returns columns A, C, E, and G (odd-numbered columns)

**Explanation:** By explicitly listing column numbers, you can create patterns. This extracts every other column, which might be useful when alternating columns contain related data (like actual vs. budget for multiple months).

---

### Example 11: Reverse All Columns
```
=CHOOSECOLS(A1:E10, 5, 4, 3, 2, 1)
```
**Result:** Returns all columns in reverse order (E, D, C, B, A)

**Explanation:** List the columns in reverse order to flip the entire array horizontally. For dynamic reversal regardless of column count, you could combine with SEQUENCE: `=CHOOSECOLS(A1:E10, SEQUENCE(5, 1, 5, -1))` counts down from 5 to 1.

---

### Example 12: Working with Named Ranges
```
=CHOOSECOLS(SalesData, 1, 4, 6)
```
**Result:** Extracts columns 1, 4, and 6 from the named range SalesData

**Explanation:** CHOOSECOLS works with named ranges, making formulas more readable. If SalesData refers to a large table, this formula extracts just the columns needed for a specific analysis without referencing specific cell addresses.

---

### Example 13: Combine with HSTACK for Complex Layouts
```
=HSTACK(CHOOSECOLS(TableA, 1, 2), CHOOSECOLS(TableB, 3, 4))
```
**Result:** Combines selected columns from two different tables side by side

**Explanation:** CHOOSECOLS pairs naturally with HSTACK to build custom arrays from multiple sources. Extract the columns you need from each source, then stack them horizontally to create a unified view.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Column number is 0 | Column numbers must be positive (1 or greater) or negative (-1 or less). Zero is not valid. |
| `#VALUE!` | Column number exceeds array dimensions | If array has 5 columns, requesting column 6 or -6 fails. Check your column count. |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula. |
| `#NAME?` | Function not available in your Excel version | CHOOSECOLS requires Excel 365/2021 or Google Sheets. Not available in Excel 2019 or earlier. |
| `#REF!` | Source range is invalid | Verify that the array reference exists and is correctly formatted. |
| `#CALC!` | Array is empty or has no columns | Ensure the source array contains data. An empty result from FILTER could cause this. |

## Use Cases

### [[Report Column Customization]]

**Scenario:** Your raw data export contains 15 columns, but your weekly report only needs 5 specific columns in a particular order. Rather than manually copying and rearranging, CHOOSECOLS automates the extraction.

**Implementation:**
```
Raw Data: Columns A-O with various data points
Report Columns Needed: ID, Name, Status, Amount, Date (columns 1, 3, 8, 12, 14)

=CHOOSECOLS(RawDataExport, 1, 3, 8, 12, 14)
```

**Business Application:** Reporting, data transformation, and dashboard creation. When source data has more columns than needed, CHOOSECOLS creates a clean subset. When source column order doesn't match output requirements, CHOOSECOLS reorders without restructuring the source.

**Technical Details:** The extracted array can feed directly into other functions, charts, or be referenced by other formulas. If source data structure changes (columns added/removed), you may need to update the column numbers. Consider using named constants or helper cells to store column numbers for easier maintenance.

---

### [[Dynamic Dashboard Data Preparation]]

**Scenario:** A dashboard displays data from a large table, but users select which columns to view via dropdown menus. CHOOSECOLS dynamically extracts the selected columns.

**Implementation:**
```
Column Selector Dropdowns: B2=1, B3=4, B4=7 (user-selected column numbers)

=CHOOSECOLS(DataTable, B2, B3, B4)
```
Or for variable number of selections:
```
=CHOOSECOLS(DataTable, FILTER(B2:B10, B2:B10<>""))
```

**Business Application:** Interactive dashboards, user-customizable reports, and data exploration tools. Users can configure their view without modifying formulas or source data.

**Technical Details:** Combine with data validation dropdown lists that show column names but return column numbers. Error handling with IFERROR may be needed for when users haven't selected all columns. Consider IFNA to handle cases where fewer columns are selected than expected.

---

### [[Data Integration from Multiple Sources]]

**Scenario:** Combining data from multiple worksheets or workbooks that have different column structures. Extract matching columns from each source to create a unified dataset.

**Implementation:**
```
Source 1: Has columns Customer, Product, Amount, Date, Region
Source 2: Has columns Customer, Region, Product, Date, Amount

Unified Output:
=VSTACK(
  CHOOSECOLS(Source1Data, 1, 2, 3, 4),
  CHOOSECOLS(Source2Data, 1, 3, 5, 4)
)
```

**Business Application:** Data consolidation, ETL processes within spreadsheets, and creating unified views from disparate data sources. Each source may have different column orders, but CHOOSECOLS normalizes them.

**Technical Details:** When stacking with VSTACK, ensure all CHOOSECOLS results have the same number of columns. The row counts can differ. Use IFERROR around each CHOOSECOLS if sources might be empty or missing.

---

### [[Removing Sensitive Columns for Sharing]]

**Scenario:** A dataset contains confidential columns (salary, SSN, personal info) that shouldn't be included when sharing with certain stakeholders. CHOOSECOLS creates a sanitized version.

**Implementation:**
```
Full Dataset: A1:J100 (columns include sensitive data in positions 4, 7, 9)
Shareable Columns: 1, 2, 3, 5, 6, 8, 10 (excluding sensitive columns)

=CHOOSECOLS(EmployeeData, 1, 2, 3, 5, 6, 8, 10)
```

**Business Application:** Data governance, privacy compliance, and controlled information sharing. Instead of manually deleting columns before sharing (risking mistakes), CHOOSECOLS programmatically extracts only appropriate columns.

**Technical Details:** Consider creating a named formula or defined name for the "shareable" configuration so it's easy to update if column structure changes. The output can be copied as values for distribution, or the sheet containing the formula can be shared while hiding the source data sheet.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill into adjacent cells
- **Negative indices:** Fully supported (-1 for last column, etc.)
- **Array size limits:** Subject to Excel's grid limits (16,384 columns)
- **Recalculation:** Part of the calculation chain; updates when source data changes

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand (spill)
- **Negative indices:** Fully supported, identical behavior to Excel
- **Array size limits:** Subject to Sheets' limits (up to 10 million cells per spreadsheet)
- **Performance:** Efficient for most use cases; very large arrays may slow calculation

### Both Platforms

- Column numbers must be non-zero integers
- Same column can be repeated in the output
- Works with arrays from other functions (FILTER, SORT, UNIQUE, etc.)
- Returns #VALUE! for invalid column references

## Tips and Best Practices

1. **Use negative indices for end-relative selection:** When you need the last few columns but don't want to count or hardcode positions, use -1, -2, -3. This makes formulas resilient to columns being added in the middle.

2. **Combine with CHOOSEROWS for precise extraction:** Use CHOOSECOLS for column selection and CHOOSEROWS for row selection to extract any rectangular subset from a larger array.

3. **Chain with other array functions:** CHOOSECOLS works beautifully with FILTER, SORT, UNIQUE, and other dynamic array functions. Build pipelines: `=CHOOSECOLS(SORT(FILTER(...), ...), ...)`.

4. **Store column numbers in cells for flexibility:** Instead of hardcoding column numbers, reference cells containing the numbers. This allows users to change selections without editing formulas.

5. **Use SEQUENCE for calculated column lists:** When you need the first N columns or every Nth column, generate the column numbers with SEQUENCE: `=CHOOSECOLS(Data, SEQUENCE(3))` gets first 3 columns.

6. **Remember columns are relative to the array, not the sheet:** Column 1 in CHOOSECOLS is the first column of the specified array, not column A of the worksheet.

7. **Handle potential errors with IFERROR:** Wrap in IFERROR when column numbers might be invalid or when source arrays might be empty: `=IFERROR(CHOOSECOLS(Data, 1, 5), "No data")`.

8. **For Excel 2019 and earlier, use INDEX alternatives:** `=INDEX(Data, 0, {1,3,5})` can return multiple columns in older Excel versions, though less elegantly.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CHOOSEROWS]] | Extracts specific rows from an array | When you need to select rows instead of columns, or combine with CHOOSECOLS for both |
| [[INDEX]] | Returns value or reference from array position | When you need a single value or when CHOOSECOLS isn't available (older Excel) |
| [[TAKE]] | Returns rows/columns from start or end of array | When you need a contiguous block from the beginning or end, not scattered selections |
| [[DROP]] | Removes rows/columns from start or end of array | When you want to exclude leading or trailing columns rather than select specific ones |

### Commonly Used Together

**[[FILTER]]** - Filter rows based on criteria

*Combined with CHOOSECOLS to filter then select columns:*
```
=CHOOSECOLS(FILTER(A1:F100, B1:B100>1000), 1, 3, 5)
```
First FILTER selects rows meeting criteria, then CHOOSECOLS extracts the needed columns from the filtered result.

---

**[[SORT]]** - Sort array by specified columns

*Combined with CHOOSECOLS for sorted, column-reduced output:*
```
=CHOOSECOLS(SORT(Data, 2, 1), 1, 2, 4)
```
SORT arranges the data, then CHOOSECOLS picks which columns to display.

---

**[[HSTACK]]** - Combine arrays horizontally

*Combined with CHOOSECOLS to merge column selections from different tables:*
```
=HSTACK(CHOOSECOLS(Table1, 1, 3), CHOOSECOLS(Table2, 2, 5))
```
Extract specific columns from multiple sources and combine them side by side.

---

**[[VSTACK]]** - Combine arrays vertically

*Combined with CHOOSECOLS to normalize column order before stacking:*
```
=VSTACK(CHOOSECOLS(Sheet1Data, 1, 3, 4), CHOOSECOLS(Sheet2Data, 1, 4, 2))
```
When source tables have different column orders, CHOOSECOLS aligns them before vertical stacking.

---

**[[UNIQUE]]** - Return unique values

*Combined with CHOOSECOLS to get unique values from specific columns:*
```
=UNIQUE(CHOOSECOLS(Data, 2, 3))
```
Extract columns 2 and 3, then find unique combinations in those columns.

---

**[[SEQUENCE]]** - Generate number sequences

*Combined with CHOOSECOLS for dynamic column selection:*
```
=CHOOSECOLS(Data, SEQUENCE(5))
```
SEQUENCE(5) generates {1,2,3,4,5}, dynamically selecting the first 5 columns.

## Official Documentation

- **Microsoft Excel:** [CHOOSECOLS function](https://support.microsoft.com/en-us/office/choosecols-function-bf117976-2722-4466-9b9a-1c01ed9aebff)
- **Google Sheets:** [CHOOSECOLS function](https://support.google.com/docs/answer/13196243)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added to match Excel's dynamic array capabilities |

---

*Last updated: 2026-01-10*
