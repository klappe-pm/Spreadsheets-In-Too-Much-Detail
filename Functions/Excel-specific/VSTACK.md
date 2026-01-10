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
- vertical-combination
- array-stacking
- row-joining
- data-consolidation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Vertical Stack
- Row Combine
- Stack Arrays
- Append Rows
tags:
- vstack
- dynamic-arrays
- stacking
- vertical
- excel-365
---

# VSTACK

## Description

**VSTACK** combines multiple arrays vertically, stacking them on top of each other. Each array's rows are appended below the previous. Need to combine monthly data? `=VSTACK(January, February, March)`. Need to add a header row above data? `=VSTACK(Headers, Data)`. The function creates unified datasets by joining rows.

The function handles arrays of different widths by padding narrower ones with #N/A. This enables combining tables with different column counts. Multiple arrays can be stacked in a single call, making data consolidation from multiple sources straightforward.

**VSTACK in Google Sheets:** Google Sheets added VSTACK in 2022, providing identical functionality to Excel. Syntax, behavior, and padding rules are the same. Formulas are fully portable.

**Complement to HSTACK:** VSTACK combines vertically (rows stacked); HSTACK combines horizontally (columns side by side). Use VSTACK for appending rows; use HSTACK for adding columns.

## Syntax

> [!f(x)] VSTACK Syntax
>
> ```
> =VSTACK(array1, [array2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | First array or range. Becomes top rows. |
| `array2, ...` | No | Additional arrays stacked below. Up to 254 arrays supported. |

### Return Value

Returns a single array combining all inputs vertically. Column count equals the maximum among inputs; narrower arrays padded with #N/A.

## Examples

> [!f(x)] VSTACK Examples

### Example 1: Stack Two Ranges
```
=VSTACK(A1:C10, A15:C25)
```
**Result:** All rows combined vertically

---

### Example 2: Monthly Data Consolidation
```
=VSTACK(Jan_Data, Feb_Data, Mar_Data)
```
**Result:** Three months of data in one table

---

### Example 3: Add Header Row
```
=VSTACK({"Name","Dept","Salary"}, Data)
```
**Result:** Header row above data

---

### Example 4: Combine with DROP (No Duplicate Headers)
```
=VSTACK(Table1, DROP(Table2, 1), DROP(Table3, 1))
```
**Result:** Tables combined, headers from Table1 only

---

### Example 5: Add Totals Row
```
=VSTACK(Data, {"Total", SUM(B:B), SUM(C:C)})
```
**Result:** Data with totals row appended

---

### Example 6: Different Width Arrays
```
=VSTACK(A1:C10, A15:D20)
```
**Result:** Combined with #N/A padding for width differences

---

### Example 7: Cross-Sheet Stacking
```
=VSTACK(Sheet1!Data, Sheet2!Data, Sheet3!Data)
```
**Result:** Data from multiple sheets combined

---

### Example 8: Filtered Data Combination
```
=VSTACK(FILTER(Data, Region="East"), FILTER(Data, Region="West"))
```
**Result:** East and West regions combined

---

### Example 9: With SORT
```
=SORT(VSTACK(Q1Sales, Q2Sales), 1, 1)
```
**Result:** Combined and sorted data

---

### Example 10: Build Complete Table
```
=VSTACK(
  {"ID","Name","Amount"},
  Data,
  {"","Total",SUM(C:C)}
)
```
**Result:** Complete table with header, data, and footer

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#SPILL!` | Not enough room | Clear cells in spill range |
| `#N/A in cells` | Different column counts | Use EXPAND for custom padding |
| `#REF!` | Invalid reference | Verify source arrays |

## Use Cases

### [[Multi-Period Data Consolidation]]

**Scenario:** Combine monthly or quarterly data into annual dataset.

**Implementation:**
```
=VSTACK(Q1, Q2, Q3, Q4)
```

---

### [[Cross-Location Consolidation]]

**Scenario:** Combine data from multiple branches or regions.

**Implementation:**
```
=VSTACK(BranchA_Data, BranchB_Data, BranchC_Data)
```

---

### [[Table Construction]]

**Scenario:** Build complete tables with headers, data, and footers.

**Implementation:**
```
=VSTACK(HeaderRow, DataRange, TotalRow)
```

## Platform Differences

Both Excel and Google Sheets implement VSTACK identically since 2022.

## Tips and Best Practices

1. **Drop headers before stacking:** Use DROP to remove duplicate headers from subsequent tables.

2. **Width differences get #N/A:** Use EXPAND to customize padding values.

3. **Order matters:** Tables appear top to bottom in specified order.

4. **Combine with SORT:** Sort after stacking for unified ordering.

5. **Build complete tables:** Stack headers, data, and footers together.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[HSTACK]] | Horizontal combination | When adding columns |
| [[DROP]] | Remove rows | For header removal before stacking |
| [[EXPAND]] | Pad arrays | For custom padding |

## Official Documentation

- **Microsoft Excel:** [VSTACK function](https://support.microsoft.com/en-us/office/vstack-function-a4b86897-be0f-48fc-adca-fcc10d795a9c)
- **Google Sheets:** [VSTACK function](https://support.google.com/docs/answer/13190235)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array combining functions |
| Excel 2021 | October 2021 | Included in perpetual license |
| Google Sheets | 2022 | Added to match Excel |

---

*Last updated: 2026-01-10*
