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
- array-trimming
- remove-rows-columns
- data-cleanup
- header-removal
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Drop Array
- Remove Rows
- Remove Columns
- Skip Rows
tags:
- drop
- dynamic-arrays
- removal
- array-manipulation
- excel-365
---

# DROP

## Description

**DROP** removes a specified number of rows or columns from the start or end of an array, returning the remainder. Need to skip the header row? `=DROP(Data, 1)`. Need to remove the last 2 columns? `=DROP(Data, , -2)`. This function provides the cleanest way to exclude leading or trailing portions of arrays without complex offset calculations.

The function handles both row and column removal in a single call. Positive numbers remove from the start (first N rows/columns), while negative numbers remove from the end (last N rows/columns). You can specify rows only, columns only, or both simultaneously. This makes DROP ideal for header removal, trimming unwanted data, and preparing arrays for combination with other datasets.

**DROP in Google Sheets:** Google Sheets added DROP in 2022, providing identical functionality to Excel. The syntax, behavior, and positive/negative number interpretation are the same. Formulas using DROP are fully portable between Excel 365/2021 and Google Sheets.

**Complement to TAKE:** DROP and TAKE are inverse operations. DROP removes the first/last N elements; TAKE keeps them. Use DROP when you know what to remove (like a header); use TAKE when you know what to keep (like top 10 results).

## Syntax

> [!f(x)] DROP Syntax
>
> ```
> =DROP(array, [rows], [columns])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to remove rows/columns. Can be a cell range, named range, or array returned by another function. |
| `rows` | No | Number of rows to remove. Positive = from start, negative = from end. Omit or use empty to keep all rows. |
| `columns` | No | Number of columns to remove. Positive = from start, negative = from end. Omit or use empty to keep all columns. |

### Return Value

Returns an array with the specified rows and/or columns removed. The remaining elements maintain their relative positions.

## Examples

> [!f(x)] DROP Examples

### Example 1: Remove Header Row
```
=DROP(A1:E100, 1)
```
**Result:** Rows 2-100 (header row removed)

**Explanation:** Most common use - skip header for calculations. All columns included.

---

### Example 2: Remove Last Rows
```
=DROP(A1:E100, -5)
```
**Result:** All rows except last 5

**Explanation:** Negative number removes from end. Remove footer or recent incomplete data.

---

### Example 3: Remove First Columns
```
=DROP(A1:E100, , 2)
```
**Result:** All rows but columns C-E only (A-B removed)

**Explanation:** Empty rows parameter keeps all rows. Remove leftmost columns like row numbers.

---

### Example 4: Remove Last Columns
```
=DROP(A1:E100, , -1)
```
**Result:** All data except rightmost column

**Explanation:** Remove trailing column (perhaps totals or calculated fields).

---

### Example 5: Rows and Columns Together
```
=DROP(A1:E100, 1, 1)
```
**Result:** Remove header row AND first column

**Explanation:** Both transformations in one formula. Useful for pivot-like data.

---

### Example 6: Header and Footer Removal
```
=DROP(DROP(Data, 1), -1)
```
**Result:** Data without first or last row

**Explanation:** Nested DROP removes from both ends. Remove header and footer.

---

### Example 7: Prepare for VSTACK
```
=VSTACK(DROP(Table1, 1), DROP(Table2, 1))
```
**Result:** Two tables combined, each without its header

**Explanation:** Remove headers before combining tables to avoid duplicate headers.

---

### Example 8: Remove Multiple Rows
```
=DROP(ImportData, 3)
```
**Result:** Data starting from row 4

**Explanation:** Skip multiple header/metadata rows common in imported files.

---

### Example 9: With FILTER
```
=DROP(FILTER(Data, Criteria), 1)
```
**Result:** Filtered data minus first matching row

**Explanation:** Apply DROP to filtered results. Useful when first match is unwanted.

---

### Example 10: Data Preparation Pipeline
```
=SORT(DROP(FILTER(RawData, Valid), 1), 2, -1)
```
**Result:** Valid data, minus header, sorted by column 2

**Explanation:** Chain DROP with other array functions for data transformation pipelines.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Rows/columns parameter is 0 | Zero is invalid; use positive or negative numbers |
| `#CALC!` | Removing more rows/columns than exist | Result would be empty; reduce drop count |
| `#SPILL!` | Not enough room for result array | Clear cells in the spill range |
| `#REF!` | Array reference is invalid | Verify the source array exists |

## Use Cases

### [[Header Row Removal]]

**Scenario:** Combine data from multiple tables, each with its own header row.

**Implementation:**
```
=VSTACK(Table1, DROP(Table2, 1), DROP(Table3, 1))
```

**Business Application:** Data consolidation where headers would repeat. Keep one header, drop others.

**Technical Details:** Table1 keeps header; subsequent tables drop theirs before stacking.

---

### [[Metadata Row Removal]]

**Scenario:** Imported data has title rows, column descriptions, or other metadata before actual data.

**Implementation:**
```
=DROP(ImportedData, 4)
```

**Business Application:** Clean up data exports from systems that add header information.

**Technical Details:** Drop appropriate number of rows to reach actual data start.

---

### [[Column Cleanup]]

**Scenario:** Remove automatically added columns (row numbers, timestamps) before analysis.

**Implementation:**
```
=DROP(ExportedData, , 2)
```

**Business Application:** Prepare exported data by removing system columns.

**Technical Details:** Empty first parameter preserves all rows while removing columns.

---

### [[Trailing Data Removal]]

**Scenario:** Report includes summary rows at bottom that shouldn't be included in analysis.

**Implementation:**
```
=DROP(Report, , -3)
```

**Business Application:** Clean up reports with footer totals or notes.

**Technical Details:** Negative removes from end. Know your footer row count.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Negative numbers:** Fully supported for end-relative removal
- **Array behavior:** Automatic spilling

### Google Sheets
- **Availability:** Added in 2022
- **Negative numbers:** Fully supported
- **Array behavior:** Identical spill behavior

### Key Differences
DROP works identically across both platforms. Formulas are fully portable.

## Tips and Best Practices

1. **Use for header removal:** Most common pattern is `=DROP(Data, 1)` to skip header row.

2. **Negative for trailing removal:** Negative numbers remove from end. More reliable than calculating positions.

3. **Combine with VSTACK for merging:** Remove headers from subsequent tables before vertical stacking.

4. **Chain with other functions:** DROP works on any array, including FILTER, SORT, UNIQUE results.

5. **Cannot drop all:** Dropping more rows/columns than exist causes error. Check your data.

6. **Consider TAKE inverse:** If you need to keep N rows, TAKE may be clearer than DROP.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TAKE]] | Keeps first/last rows or columns | When keeping is easier than removing |
| [[CHOOSEROWS]] | Selects specific rows | For non-contiguous row selection |
| [[CHOOSECOLS]] | Selects specific columns | For non-contiguous column selection |
| [[OFFSET]] | Returns offset range | For legacy compatibility |

### Commonly Used Together

**[[VSTACK]]** - Stack after header removal
```
=VSTACK(DROP(A, 1), DROP(B, 1))
```

**[[TAKE]]** - Drop then take for middle slice
```
=TAKE(DROP(Data, 1), 5)
```

**[[FILTER]]** - Filter then drop
```
=DROP(FILTER(Data, Criteria), 1)
```

## Official Documentation

- **Microsoft Excel:** [DROP function](https://support.microsoft.com/en-us/office/drop-function-1cb4e151-9e17-4838-abe5-9ba48d8c6a34)
- **Google Sheets:** [DROP function](https://support.google.com/docs/answer/13190082)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions |
| Excel 2021 | October 2021 | Included in perpetual license |
| Google Sheets | 2022 | Added to match Excel |
| Excel 2019 | Not available | Use OFFSET or INDEX workarounds |

---

*Last updated: 2026-01-10*
