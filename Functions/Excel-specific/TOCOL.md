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
- column-conversion
- reshape
- data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- To Column
- Flatten to Column
- Array to Column
- Columnize
tags:
- tocol
- dynamic-arrays
- flattening
- transformation
- excel-365
---

# TOCOL

## Description

**TOCOL** flattens a two-dimensional array into a single column by reading values row by row or column by column. A 3x4 array becomes a 12-row single column. This function is essential for converting grid data into list format for XLOOKUP, FILTER, or other functions that work best with single-column data.

The function provides control over reading order (by row or by column) and handling of blank/error cells (ignore, include, or errors only). This flexibility makes TOCOL invaluable for data cleanup and transformation workflows.

**TOCOL in Google Sheets:** Google Sheets added TOCOL in 2022, providing identical functionality to Excel. The syntax, parameters, and behavior are the same. Formulas are fully portable.

**Complement to TOROW:** TOCOL produces vertical output; TOROW produces horizontal output. Both flatten 2D arrays but in different orientations.

## Syntax

> [!f(x)] TOCOL Syntax
>
> ```
> =TOCOL(array, [ignore], [scan_by_column])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The array to flatten into a single column. |
| `ignore` | No | 0 = keep all (default), 1 = ignore blanks, 2 = ignore errors, 3 = ignore blanks and errors. |
| `scan_by_column` | No | FALSE = read by row (default), TRUE = read by column. |

### Return Value

Returns a single-column array containing all values from the input array.

## Examples

> [!f(x)] TOCOL Examples

### Example 1: Basic Flatten
```
=TOCOL(A1:C4)
```
**Result:** 12-row column (reads row by row)

**Explanation:** Flattens 3x4 grid into single column. A1, B1, C1, A2, B2, C2...

---

### Example 2: Ignore Blanks
```
=TOCOL(A1:C10, 1)
```
**Result:** Column with blank cells removed

**Explanation:** Only non-empty cells included in output.

---

### Example 3: Ignore Errors
```
=TOCOL(A1:C10, 2)
```
**Result:** Column without error cells

**Explanation:** #N/A, #VALUE!, etc. removed from output.

---

### Example 4: Ignore Both
```
=TOCOL(A1:C10, 3)
```
**Result:** Only values (no blanks or errors)

**Explanation:** Clean data only in output.

---

### Example 5: Scan by Column
```
=TOCOL(A1:C4, , TRUE)
```
**Result:** Column reading A1:A4, B1:B4, C1:C4

**Explanation:** Read down columns instead of across rows.

---

### Example 6: Unique Values from Grid
```
=UNIQUE(TOCOL(A1:C10, 1))
```
**Result:** Unique non-blank values as column

**Explanation:** Flatten then deduplicate.

---

### Example 7: Count Non-Blank
```
=ROWS(TOCOL(A1:C10, 1))
```
**Result:** Count of non-blank cells

**Explanation:** Flatten ignoring blanks, count rows.

---

### Example 8: For XLOOKUP
```
=XLOOKUP("target", TOCOL(A1:C10, 1), TOCOL(D1:F10, 1))
```
**Result:** Lookup across flattened data

**Explanation:** Enable lookups in grid data.

---

### Example 9: Sum Grid After Cleaning
```
=SUM(TOCOL(A1:C10, 3))
```
**Result:** Sum of valid values only

**Explanation:** Ignore blanks and errors before summing.

---

### Example 10: Prepare for FILTER
```
=FILTER(TOCOL(A:C, 1), TOCOL(A:C, 1)>100)
```
**Result:** All values >100 from grid

**Explanation:** Flatten, then filter as single column.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#SPILL!` | No room for output | Clear target cells |
| `Empty result` | All values ignored | Check ignore parameter |

## Use Cases

### [[Data Normalization]]

**Scenario:** Convert wide-format data to long-format for analysis.

**Implementation:**
```
=TOCOL(WideData, 1)
```

**Business Application:** Prepare data for pivot tables or statistical analysis.

---

### [[Unique Value Extraction]]

**Scenario:** Get all unique values from a multi-column data set.

**Implementation:**
```
=UNIQUE(TOCOL(DataGrid, 1))
```

**Business Application:** Category lists, unique identifiers, tag extraction.

---

### [[Data Cleanup Pipeline]]

**Scenario:** Clean and consolidate fragmented data.

**Implementation:**
```
=SORT(UNIQUE(TOCOL(MessyData, 3)))
```

**Business Application:** Create clean master lists from messy source data.

## Platform Differences

Both Excel and Google Sheets support TOCOL identically since 2022.

## Tips and Best Practices

1. **Choose ignore parameter wisely:** 0 keeps everything, 1 removes blanks, 2 removes errors, 3 removes both.

2. **Default is row-by-row:** Values read left to right, top to bottom unless scan_by_column is TRUE.

3. **Chain with UNIQUE/SORT:** TOCOL then UNIQUE creates clean lists.

4. **Use for XLOOKUP on grids:** Flatten both lookup and return arrays.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TOROW]] | Flatten to single row | When horizontal output needed |
| [[WRAPCOLS]] | Wrap into columns | When reshaping to grid |
| [[WRAPROWS]] | Wrap into rows | When reshaping to grid |

## Official Documentation

- **Microsoft Excel:** [TOCOL function](https://support.microsoft.com/en-us/office/tocol-function-22839d9b-0b55-4fc1-b4e6-2761f8f122c4)
- **Google Sheets:** [TOCOL function](https://support.google.com/docs/answer/13190683)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions |
| Google Sheets | 2022 | Added to match Excel |

---

*Last updated: 2026-01-10*
