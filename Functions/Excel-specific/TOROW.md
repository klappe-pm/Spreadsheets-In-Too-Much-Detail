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
- array-flattening
- row-conversion
- reshape
- data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- To Row
- Flatten to Row
- Array to Row
- Rowize
tags:
- torow
- dynamic-arrays
- flattening
- transformation
- excel-365
---

# TOROW

## Description

**TOROW** flattens a two-dimensional array into a single row by reading values row by row or column by column. A 3x4 array becomes a 12-column single row. This function is essential for converting grid data into horizontal list format for array operations that require single-row input.

The function provides control over reading order (by row or by column) and handling of blank/error cells (ignore, include, or errors only). This flexibility makes TOROW invaluable for data transformation and creating horizontal arrays.

**TOROW in Google Sheets:** Google Sheets added TOROW in 2022, providing identical functionality to Excel. The syntax, parameters, and behavior are the same. Formulas are fully portable.

**Complement to TOCOL:** TOROW produces horizontal output; TOCOL produces vertical output. Both flatten 2D arrays but in different orientations.

## Syntax

> [!f(x)] TOROW Syntax
>
> ```
> =TOROW(array, [ignore], [scan_by_column])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The array to flatten into a single row. |
| `ignore` | No | 0 = keep all (default), 1 = ignore blanks, 2 = ignore errors, 3 = ignore blanks and errors. |
| `scan_by_column` | No | FALSE = read by row (default), TRUE = read by column. |

### Return Value

Returns a single-row array containing all values from the input array.

## Examples

> [!f(x)] TOROW Examples

### Example 1: Basic Flatten to Row
```
=TOROW(A1:C4)
```
**Result:** 12-column row (reads row by row)

**Explanation:** Flattens 3x4 grid into single row. A1, B1, C1, A2, B2, C2...

---

### Example 2: Ignore Blanks
```
=TOROW(A1:C10, 1)
```
**Result:** Row with blank cells removed

**Explanation:** Only non-empty cells included in output.

---

### Example 3: Ignore Errors
```
=TOROW(A1:C10, 2)
```
**Result:** Row without error cells

**Explanation:** #N/A, #VALUE!, etc. removed from output.

---

### Example 4: Ignore Both
```
=TOROW(A1:C10, 3)
```
**Result:** Only values (no blanks or errors)

**Explanation:** Clean data only in horizontal output.

---

### Example 5: Scan by Column
```
=TOROW(A1:C4, , TRUE)
```
**Result:** Row reading A1:A4, B1:B4, C1:C4

**Explanation:** Read down columns instead of across rows.

---

### Example 6: Create Horizontal Headers
```
=TOROW(A1:A10)
```
**Result:** Convert column to row

**Explanation:** Transform vertical list to horizontal headers.

---

### Example 7: Concatenate All Values
```
=TEXTJOIN(", ", TRUE, TOROW(A1:C10, 1))
```
**Result:** All values as comma-separated text

**Explanation:** Flatten, then join as text.

---

### Example 8: Count Unique
```
=COLUMNS(UNIQUE(TOROW(A1:C10, 1)))
```
**Result:** Count of unique non-blank values

**Explanation:** Flatten, dedupe, count columns.

---

### Example 9: Horizontal Lookup Array
```
=XLOOKUP("target", TOROW(A1:C5), TOROW(D1:F5))
```
**Result:** Lookup across flattened horizontal data

**Explanation:** Enable horizontal lookups on grid data.

---

### Example 10: For HSTACK
```
=HSTACK(Header, TOROW(DataGrid))
```
**Result:** Header plus flattened data horizontally

**Explanation:** Combine label with flattened values.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#SPILL!` | No room for output | Clear target cells |
| `Empty result` | All values ignored | Check ignore parameter |

## Use Cases

### [[Header Row Creation]]

**Scenario:** Convert column of labels to row of headers.

**Implementation:**
```
=TOROW(LabelColumn)
```

**Business Application:** Dynamic header generation, pivot layouts.

---

### [[Horizontal Data Comparison]]

**Scenario:** Flatten grid for horizontal analysis or matching.

**Implementation:**
```
=TOROW(GridData, 1)
```

**Business Application:** Cross-reference comparisons, horizontal patterns.

---

### [[Text Concatenation Prep]]

**Scenario:** Combine all grid values into single text string.

**Implementation:**
```
=TEXTJOIN("; ", TRUE, TOROW(DataGrid, 3))
```

**Business Application:** Summary strings, export formatting.

## Platform Differences

Both Excel and Google Sheets support TOROW identically since 2022.

## Tips and Best Practices

1. **Choose ignore parameter wisely:** 0 keeps everything, 1 removes blanks, 2 removes errors, 3 removes both.

2. **Default is row-by-row:** Values read left to right, top to bottom unless scan_by_column is TRUE.

3. **Use with TEXTJOIN:** TOROW plus TEXTJOIN creates comma-separated lists from grids.

4. **Transpose alternative:** TOROW can effectively transpose single columns to rows.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TOCOL]] | Flatten to single column | When vertical output needed |
| [[WRAPCOLS]] | Wrap into columns | When reshaping to grid |
| [[WRAPROWS]] | Wrap into rows | When reshaping to grid |

## Official Documentation

- **Microsoft Excel:** [TOROW function](https://support.microsoft.com/en-us/office/torow-function-b90d0964-a7d9-44b7-816b-ffa5c2fe2289)
- **Google Sheets:** [TOROW function](https://support.google.com/docs/answer/13190684)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions |
| Google Sheets | 2022 | Added to match Excel |

---

*Last updated: 2026-01-10*
