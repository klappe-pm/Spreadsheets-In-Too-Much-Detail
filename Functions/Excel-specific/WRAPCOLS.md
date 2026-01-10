---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- dynamic-arrays
- array-reshaping
subTopics:
- array-transformation
- column-wrapping
- reshape
- layout-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Wrap Columns
- Column Wrap
- Array to Columns
- Reshape to Columns
tags:
- wrapcols
- dynamic-arrays
- reshaping
- transformation
- excel-365
---

# WRAPCOLS

## Description

**WRAPCOLS** transforms a one-dimensional array into a two-dimensional array by wrapping values into columns of a specified length. It reads values from a single row or column and outputs them column by column, filling down each column before moving to the next. A 12-element list with wrap_count of 4 becomes a 4-row by 3-column array.

This function is essential for reshaping data layouts. Convert a single-column list into a multi-column grid, transform horizontal data into columnar format, or restructure data for specific report layouts. The wrap direction fills vertically within each column, then moves right to the next column.

**WRAPCOLS in Google Sheets:** Google Sheets added WRAPCOLS in 2023, providing the same functionality as Excel. The syntax and behavior are identical. Formulas are portable between platforms.

**Complement to WRAPROWS:** WRAPCOLS fills columns first (top to bottom, then right); WRAPROWS fills rows first (left to right, then down). Choose based on your desired reading order in the output.

## Syntax

> [!f(x)] WRAPCOLS Syntax
>
> ```
> =WRAPCOLS(vector, wrap_count, [pad_with])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `vector` | Yes | A single row or column array to wrap. Multi-dimensional arrays are not supported. |
| `wrap_count` | Yes | Number of values per column (column height). Must be positive integer. |
| `pad_with` | No | Value to fill incomplete columns. Default is #N/A. |

### Return Value

Returns a 2D array with wrap_count rows and enough columns to contain all values.

## Examples

> [!f(x)] WRAPCOLS Examples

### Example 1: Basic Column Wrap
```
=WRAPCOLS(A1:A12, 4)
```
**Result:** 4x3 array (4 rows, 3 columns)

**Explanation:** 12 values wrapped into columns of 4. First column: A1:A4, second: A5:A8, third: A9:A12.

---

### Example 2: Wrap with Padding
```
=WRAPCOLS(A1:A10, 4, 0)
```
**Result:** 4x3 array, last column padded with 0

**Explanation:** 10 values don't fill 3 complete columns of 4. Last two cells get 0 padding.

---

### Example 3: Single Column Per Value
```
=WRAPCOLS(A1:A6, 1)
```
**Result:** 1x6 horizontal array

**Explanation:** Wrap count of 1 transposes column to row.

---

### Example 4: Full Column (No Wrap)
```
=WRAPCOLS(A1:A10, 10)
```
**Result:** Same as input (single column)

**Explanation:** Wrap count equal to array length produces no change.

---

### Example 5: From Horizontal to Grid
```
=WRAPCOLS(1:1, 5)
```
**Result:** 5-row grid from row data

**Explanation:** Transform horizontal row into columnar grid format.

---

### Example 6: Calendar Layout
```
=WRAPCOLS(SEQUENCE(35), 7)
```
**Result:** 7x5 grid (7 rows, 5 columns)

**Explanation:** 35 days wrapped into weeks (7 days each).

---

### Example 7: Card Dealing Simulation
```
=WRAPCOLS(SORTBY(SEQUENCE(52), RANDARRAY(52)), 13)
```
**Result:** 13x4 array of shuffled cards

**Explanation:** Shuffle deck, deal 13 cards to each of 4 hands.

---

### Example 8: Empty String Padding
```
=WRAPCOLS(A1:A10, 4, "")
```
**Result:** Grid with blank cells for padding

**Explanation:** Empty string creates visually clean output.

---

### Example 9: With UNIQUE
```
=WRAPCOLS(UNIQUE(A:A), 10)
```
**Result:** Unique values in columnar grid

**Explanation:** Get unique values, display in organized columns.

---

### Example 10: Data Restructuring
```
=WRAPCOLS(TOCOL(A1:D10), 10)
```
**Result:** Flatten then rewrap data

**Explanation:** Convert multi-column to single column, then rewrap differently.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-vector input (2D array) | Input must be single row or column |
| `#VALUE!` | wrap_count is 0 or negative | Must be positive integer |
| `#SPILL!` | No room for output | Clear target cells |

## Use Cases

### [[List to Grid Conversion]]

**Scenario:** Convert single-column product list into multi-column display grid.

**Implementation:**
```
=WRAPCOLS(ProductList, 10, "")
```

**Business Application:** Create catalog layouts, multi-column menus, or organized displays.

---

### [[Data Chunking]]

**Scenario:** Split sequential data into fixed-size groups for processing.

**Implementation:**
```
=WRAPCOLS(TransactionIDs, 100)
```

**Business Application:** Batch processing, pagination, or group-based analysis.

---

### [[Calendar Generation]]

**Scenario:** Create weekly calendar grids from sequential date lists.

**Implementation:**
```
=WRAPCOLS(DateList, 7)
```

**Business Application:** Weekly planning views, schedule layouts.

## Platform Differences

Both Excel and Google Sheets support WRAPCOLS identically since 2022/2023.

## Tips and Best Practices

1. **Understand fill order:** Values fill down columns, then right to next column.

2. **Use padding for clean output:** Empty string "" creates visually clean grids.

3. **Consider WRAPROWS alternative:** If you want row-first filling, use WRAPROWS instead.

4. **Works with TOCOL/TOROW:** First flatten data, then reshape as needed.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WRAPROWS]] | Wrap into rows first | When horizontal reading order preferred |
| [[TOCOL]] | Flatten to single column | When you need to unwrap first |
| [[TOROW]] | Flatten to single row | When you need horizontal input |

## Official Documentation

- **Microsoft Excel:** [WRAPCOLS function](https://support.microsoft.com/en-us/office/wrapcols-function-d038b05a-57b7-4ee0-be94-cb1b8dc87d95)
- **Google Sheets:** [WRAPCOLS function](https://support.google.com/docs/answer/13190070)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions |
| Google Sheets | 2023 | Added to match Excel |

---

*Last updated: 2026-01-10*
