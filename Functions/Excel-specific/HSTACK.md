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
- horizontal-combination
- array-merging
- column-joining
- data-consolidation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Horizontal Stack
- Column Combine
- Side by Side Arrays
- Horizontal Merge
tags:
- hstack
- dynamic-arrays
- merging
- horizontal
- excel-365
---

# HSTACK

## Description

**HSTACK** combines multiple arrays horizontally, placing them side by side. Each array becomes columns in the result, stacked left to right. Need to combine name column with address columns? `=HSTACK(Names, Addresses)`. Need three separate data sources side by side? `=HSTACK(Source1, Source2, Source3)`. The function creates unified datasets from separate column groups.

The function handles arrays of different heights by padding shorter ones with #N/A (or you can combine with EXPAND for custom padding). This makes HSTACK robust for real-world data where sources may have different row counts. Multiple arrays can be combined in a single call, making complex data assembly simple and readable.

**HSTACK in Google Sheets:** Google Sheets added HSTACK in 2022, providing identical functionality to Excel. The syntax, behavior, and padding rules are the same. Formulas using HSTACK are fully portable between Excel 365/2021 and Google Sheets.

**Complement to VSTACK:** HSTACK combines horizontally (columns side by side); VSTACK combines vertically (rows stacked). Together they provide complete array combination capabilities. Use HSTACK for adding columns; use VSTACK for adding rows.

## Syntax

> [!f(x)] HSTACK Syntax
>
> ```
> =HSTACK(array1, [array2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | First array or range to include. Becomes leftmost columns. |
| `array2, ...` | No | Additional arrays to combine. Added to the right of previous arrays. Up to 254 arrays supported. |

### Return Value

Returns a single array combining all input arrays horizontally. Row count equals the maximum row count among inputs; shorter arrays padded with #N/A.

## Examples

> [!f(x)] HSTACK Examples

### Example 1: Combine Two Ranges
```
=HSTACK(A1:A10, B1:B10)
```
**Result:** Two-column array combining both ranges

**Explanation:** Basic horizontal combination. Same as referencing A1:B10 if contiguous, but works for separate ranges.

---

### Example 2: Multiple Arrays
```
=HSTACK(Names, Departments, Salaries)
```
**Result:** Three data sources combined into single multi-column array

**Explanation:** Any number of arrays combined left to right in order specified.

---

### Example 3: Add Row Numbers
```
=HSTACK(SEQUENCE(ROWS(Data)), Data)
```
**Result:** Data with row numbers added as first column

**Explanation:** Generate row numbers, combine with data. Common pattern for indexed output.

---

### Example 4: Combine with Calculations
```
=HSTACK(Data, SUM(Data))
```
**Result:** Original data plus calculated total column

**Explanation:** HSTACK works with values and formulas. Add computed columns.

---

### Example 5: Different Height Arrays
```
=HSTACK(A1:A10, B1:B5)
```
**Result:** 10-row array; column B has #N/A in rows 6-10

**Explanation:** Shorter array padded with #N/A. Use EXPAND beforehand for custom padding.

---

### Example 6: Single Values as Columns
```
=HSTACK(Data, "Status", TODAY())
```
**Result:** Data plus constant text and date columns

**Explanation:** Single values broadcast down all rows. Add constant columns easily.

---

### Example 7: Combine FILTER Results
```
=HSTACK(FILTER(A:A, Criteria), FILTER(C:C, Criteria))
```
**Result:** Non-contiguous columns from filtered data

**Explanation:** Filter different columns with same criteria, combine results.

---

### Example 8: Build Report Layout
```
=HSTACK(CHOOSECOLS(Data,1,2), Spacer, CHOOSECOLS(Data,5,6))
```
**Result:** Selected columns with spacer in between

**Explanation:** Create formatted layouts with HSTACK. Spacer can be empty column.

---

### Example 9: Combine with VSTACK
```
=VSTACK(Headers, HSTACK(Col1, Col2, Col3))
```
**Result:** Headers above combined data columns

**Explanation:** Nest HSTACK inside VSTACK for complete table construction.

---

### Example 10: Cross-Sheet Combination
```
=HSTACK(Sheet1!A:B, Sheet2!C:D)
```
**Result:** Columns from different sheets side by side

**Explanation:** Cross-sheet data consolidation in single formula.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#SPILL!` | Not enough room for result array | Clear cells in the spill range |
| `#N/A in cells` | Arrays have different row counts | Expected behavior; use EXPAND for custom padding |
| `#REF!` | Array reference is invalid | Verify all source arrays exist |

## Use Cases

### [[Data Consolidation]]

**Scenario:** Combine customer data from separate columns/sources into unified view.

**Implementation:**
```
=HSTACK(CustomerIDs, CustomerNames, ContactInfo, AccountStatus)
```

**Business Application:** Create unified customer records from separate data stores.

---

### [[Report Building]]

**Scenario:** Construct reports combining data columns with calculated columns.

**Implementation:**
```
=HSTACK(ProductData, Revenue*Margin, Revenue-Costs)
```

**Business Application:** Build complete reports with both source data and calculations.

---

### [[Adding Metadata Columns]]

**Scenario:** Add source identifier, timestamps, or other metadata to data extracts.

**Implementation:**
```
=HSTACK(ExtractedData, "Source_A", NOW())
```

**Business Application:** Track data lineage by adding source information.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Maximum arrays:** 254
- **Padding:** #N/A for shorter arrays

### Google Sheets
- **Availability:** Added in 2022
- **Maximum arrays:** 254
- **Padding:** Same #N/A behavior

Both platforms work identically. Formulas are fully portable.

## Tips and Best Practices

1. **Order matters:** Arrays appear left to right in the order specified.

2. **Handle height differences:** Shorter arrays get #N/A padding. Use EXPAND for custom fill values.

3. **Add row numbers easily:** `=HSTACK(SEQUENCE(n), Data)` prepends row numbers.

4. **Single values broadcast:** Constants become full columns matching tallest array.

5. **Combine with CHOOSECOLS:** Select specific columns from sources before stacking.

6. **Consider EXPAND for alignment:** Match array heights before HSTACK to avoid #N/A.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VSTACK]] | Vertical combination | When adding rows instead of columns |
| [[CHOOSECOLS]] | Select specific columns | When you need subset before combining |
| [[EXPAND]] | Pad arrays to size | When custom padding needed |

## Official Documentation

- **Microsoft Excel:** [HSTACK function](https://support.microsoft.com/en-us/office/hstack-function-98c4ab76-10fe-4b4f-8c89-2e1f2d91d54d)
- **Google Sheets:** [HSTACK function](https://support.google.com/docs/answer/13190236)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array combining functions |
| Excel 2021 | October 2021 | Included in perpetual license |
| Google Sheets | 2022 | Added to match Excel |

---

*Last updated: 2026-01-10*
