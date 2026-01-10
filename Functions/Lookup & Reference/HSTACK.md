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
- data-combination
- dynamic-arrays
- horizontal-stacking
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Horizontal Stack
- Stack Horizontally
- Combine Columns
- Column Append
tags:
- array
- stack
- combine
- dynamic-arrays
- horizontal
---

# HSTACK

## Description

**HSTACK (Horizontal Stack)** combines multiple arrays side by side, appending them horizontally into a single array. Think of it as placing arrays next to each other—Array A on the left, Array B in the middle, Array C on the right. If you have employee IDs in one column, names in another, and departments in a third, HSTACK merges them into a unified table. It's the horizontal counterpart to VSTACK, which stacks arrays vertically.

The function accepts any number of arrays and joins them left to right in the order specified. Each array becomes a set of columns in the result. You can combine ranges, single values, formula results, or literal arrays. HSTACK is essential for building dynamic tables from separate data sources, adding calculated columns to datasets, or restructuring data layouts without manual copy-paste operations.

**Key behaviors to understand:** All arrays must have the same number of rows, or HSTACK will pad shorter arrays with #N/A errors to match the tallest array. Single values are treated as 1-row arrays and will be padded accordingly when combined with multi-row arrays. HSTACK preserves the column order of each input array—columns from the first array come first, then columns from the second, and so on. The result is a dynamic array that spills into adjacent cells.

**Platform availability:** HSTACK is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically. For older Excel versions, workarounds involve complex CHOOSE with column index calculations or manual range concatenation—neither approaches HSTACK's elegance.

## Syntax

> [!f(x)] HSTACK Syntax
>
> ```
> =HSTACK(array1, [array2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first array or range. This will form the leftmost columns of the result. |
| `array2, ...` | No | Additional arrays or ranges to append horizontally. Each subsequent array is added to the right. You can include as many arrays as needed. |

### Return Value

Returns a single array containing all input arrays combined side by side. The result has as many rows as the tallest input array and as many columns as the sum of all input array columns. Arrays with fewer rows are padded with #N/A at the bottom.

## Examples

> [!f(x)] HSTACK Examples

### Example 1: Basic Two-Array Stack
```
=HSTACK(A1:A10, B1:B10)
```
**Result:** Returns a 10-row by 2-column array combining columns A and B

**Explanation:** The simplest HSTACK—placing two single-column ranges side by side. Equivalent to referencing A1:B10, but powerful when the sources are non-adjacent or formula results.

---

### Example 2: Combine Multiple Ranges
```
=HSTACK(A1:B10, D1:D10, F1:H10)
```
**Result:** Returns a 10-row by 6-column array (2+1+3 columns)

**Explanation:** Combines three non-adjacent ranges into one continuous table. Column order in result: A, B, D, F, G, H. This is invaluable when source data is scattered across a worksheet.

---

### Example 3: Add a Calculated Column
```
=HSTACK(A1:C10, B1:B10*C1:C10)
```
**Result:** Returns original 3 columns plus a 4th column with B*C calculations

**Explanation:** The second argument is a formula result (multiplication of two columns). HSTACK appends this calculated array as a new column. Perfect for adding totals, percentages, or other computations to existing data.

---

### Example 4: Include Static Values
```
=HSTACK(A1:B10, {"Status";"Active";"Active";"Pending";"Active";"Closed";"Active";"Pending";"Active";"Active"})
```
**Result:** Adds a Status column to the original data

**Explanation:** You can stack literal arrays (constants) with ranges. The array constant uses semicolons to separate rows. This adds fixed data as a new column.

---

### Example 5: Single Value with Array (Padding Behavior)
```
=HSTACK(A1:A10, "Category")
```
**Result:** 10-row by 2-column array; column 2 has "Category" in row 1 and #N/A in rows 2-10

**Explanation:** A single value is treated as a 1-row array. HSTACK pads it with #N/A to match the 10-row array. To fill all rows with the same value, use a filled array instead.

---

### Example 6: Fill Column with Single Value
```
=HSTACK(A1:A10, IF(SEQUENCE(10), "Active", ""))
```
**Result:** 10-row array with "Active" in every row of column 2

**Explanation:** To avoid #N/A padding, create a filled array. `IF(SEQUENCE(10), "Active", "")` creates 10 "Active" values. Now HSTACK has matching row counts.

---

### Example 7: Combine Formula Results
```
=HSTACK(FILTER(A1:C100, B1:B100>1000), FILTER(D1:D100, B1:B100>1000))
```
**Result:** Filtered data from two different column groups, side by side

**Explanation:** HSTACK works with any array-returning function. Here, two FILTER results (which must have matching row counts since they use the same criteria) are combined horizontally.

---

### Example 8: Build Dynamic Table with Headers
```
=VSTACK(
  {"ID", "Name", "Revenue", "Growth"},
  HSTACK(A2:A100, B2:B100, C2:C100, C2:C100/D2:D100-1)
)
```
**Result:** Complete table with headers and calculated column

**Explanation:** Combine VSTACK and HSTACK to build complete tables. HSTACK creates the data rows (4 columns including calculation), VSTACK adds the header row on top.

---

### Example 9: Arrays with Different Row Counts
```
=HSTACK(A1:A10, B1:B5)
```
**Result:** 10-row by 2-column array; column B has #N/A in rows 6-10

**Explanation:** When arrays have different row counts, HSTACK pads shorter ones with #N/A. The result matches the tallest array. Use EXPAND on shorter arrays if you want different padding.

---

### Example 10: Combine with Row Numbers
```
=HSTACK(SEQUENCE(ROWS(Data)), Data)
```
**Result:** Adds a row number column to the left of Data

**Explanation:** SEQUENCE generates row numbers (1, 2, 3...). HSTACK places this before the data, adding an automatic ID column. Useful for tracking row positions after filtering or sorting.

---

### Example 11: Merge Sorted and Filtered Data
```
=HSTACK(
  SORT(FILTER(A1:B100, C1:C100="Active"), 2, 1),
  FILTER(D1:D100, C1:C100="Active")
)
```
**Result:** Sorted filtered columns A:B combined with filtered column D

**Explanation:** Complex data manipulations can feed into HSTACK. Just ensure row counts match (same FILTER criteria here ensures that). The first array is sorted; the second maintains original filter order.

---

### Example 12: Create Lookup Table from Separate Lists
```
=HSTACK(ProductIDs, ProductNames, ProductPrices)
```
**Result:** Three named ranges combined into a lookup table

**Explanation:** If your data is in separate named ranges (perhaps from different sources), HSTACK unifies them. The result can be used with XLOOKUP, INDEX/MATCH, or other lookup functions.

---

### Example 13: Add Multiple Calculated Columns
```
=HSTACK(
  A1:D100,
  C1:C100*D1:D100,
  (C1:C100*D1:D100)*0.1,
  C1:C100*D1:D100*1.1
)
```
**Result:** Original data plus Total, Tax, and Grand Total columns

**Explanation:** Stack multiple calculations at once. Each formula becomes an additional column. This creates a complete invoice-style layout from base data.

---

### Example 14: Combine with UNIQUE
```
=HSTACK(UNIQUE(A2:A100), COUNTIF(A2:A100, UNIQUE(A2:A100)))
```
**Result:** Unique values with their occurrence counts

**Explanation:** Creates a frequency table. UNIQUE extracts distinct values; COUNTIF counts each. HSTACK puts them side by side. Note: This relies on UNIQUE returning the same order both times.

---

### Example 15: Dynamic Column Addition Based on Condition
```
=IF(ShowDetails, HSTACK(Summary, Details), Summary)
```
**Result:** Either summary alone or summary with details columns

**Explanation:** Conditional HSTACK based on a toggle. When ShowDetails is TRUE, extra columns are included. Useful for reports with expandable detail levels.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A in results` | Arrays have different row counts; shorter ones are padded | Use EXPAND to pad shorter arrays with preferred values, or ensure arrays have matching rows |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula |
| `#NAME?` | Function not available in your Excel version | HSTACK requires Excel 365/2021 or Google Sheets |
| `#REF!` | One of the source ranges is invalid | Verify all array references exist and are correctly formatted |
| `#VALUE!` | Invalid array structure | Ensure all inputs are valid ranges or arrays |

## Use Cases

### [[Data Consolidation from Multiple Sources]]

**Scenario:** Customer data is split across multiple systems—IDs from the CRM, contact info from the email system, purchase history from the order system. HSTACK combines them into a unified view.

**Implementation:**
```
=HSTACK(
  CRM_CustomerIDs,
  EmailSystem_Contacts,
  OrderSystem_Purchases
)
```
Or with specific columns from each:
```
=HSTACK(
  CHOOSECOLS(CRMData, 1, 3),
  CHOOSECOLS(EmailData, 2, 4, 5),
  CHOOSECOLS(OrderData, 3)
)
```

**Business Application:** Customer 360 views, data integration, unified reporting. Different data sources rarely align in structure; HSTACK enables ad-hoc combinations.

**Technical Details:** Ensure row alignment—each source must represent the same entities in the same order. If not aligned, use XLOOKUP to match records before stacking.

---

### [[Building Dynamic Reports with Calculations]]

**Scenario:** A financial report needs base data columns plus calculated metrics (margins, growth rates, variances). HSTACK adds calculated columns dynamically.

**Implementation:**
```
=HSTACK(
  FinancialData,
  Revenue - Costs,
  (Revenue - Costs) / Revenue,
  (CurrentYear - PriorYear) / PriorYear
)
```
With headers:
```
=VSTACK(
  HSTACK(BaseHeaders, {"Margin", "Margin %", "YoY Growth"}),
  HSTACK(Data, Calc1, Calc2, Calc3)
)
```

**Business Application:** Financial reporting, KPI dashboards, variance analysis. Calculations stay linked to source data without manual column creation.

**Technical Details:** Each calculation must return the same number of rows as the base data. For row-by-row calculations, array operations naturally align. For aggregates, expand single values to match row count.

---

### [[Creating Structured Arrays for Functions]]

**Scenario:** Functions like XLOOKUP, INDEX, or FILTER work with arrays. HSTACK builds these arrays dynamically from scattered sources.

**Implementation:**
```
Lookup Table:
=HSTACK(ProductCodes, ProductNames, Prices)

Then used with XLOOKUP:
=XLOOKUP(SearchValue, CHOOSECOLS(LookupTable, 1), CHOOSECOLS(LookupTable, 2, 3))

Or define a named range using HSTACK:
LookupData = HSTACK(Sheet1!A:A, Sheet2!B:B, Sheet3!C:C)
```

**Business Application:** Dynamic lookup tables, reference data management, formula infrastructure. Build structured data for function consumption.

**Technical Details:** HSTACK in named ranges creates dynamic lookup tables that update as source data changes. Be mindful of performance with very large stacked ranges.

---

### [[Side-by-Side Comparison Reports]]

**Scenario:** Compare this year vs. last year, actual vs. budget, or different product lines by placing data sets side by side.

**Implementation:**
```
=HSTACK(
  FILTER(Data, Year=2024),
  FILTER(Data, Year=2023),
  FILTER(Data, Year=2024) - FILTER(Data, Year=2023)
)
```
With labeled sections:
```
=HSTACK(
  VSTACK("2024 Actual", Data2024),
  VSTACK("2023 Actual", Data2023),
  VSTACK("Variance", Data2024-Data2023)
)
```

**Business Application:** Period comparisons, variance reporting, competitive analysis. Visual side-by-side layouts aid analysis.

**Technical Details:** Ensure compared datasets have identical structures. The variance calculation relies on matching row order and count.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill
- **Row padding:** #N/A for mismatched row counts
- **Performance:** Efficient for reasonable array sizes

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand
- **Row padding:** Same behavior as Excel
- **Syntax:** Identical to Excel
- **Array literals:** Use semicolons for rows, commas for columns

### Both Platforms

- Combines left to right in order specified
- Pads shorter arrays with #N/A to match tallest
- Works with ranges, arrays, and formula results
- No practical limit on number of arrays to combine

## Tips and Best Practices

1. **Ensure matching row counts:** Arrays with different rows get #N/A padding. Use EXPAND or matching FILTER criteria to align row counts before stacking.

2. **Use EXPAND for custom padding:** Instead of #N/A, pad with zeros or blanks: `HSTACK(Array1, EXPAND(Array2, ROWS(Array1), , 0))`.

3. **Add row numbers with SEQUENCE:** `HSTACK(SEQUENCE(n), Data)` prepends row numbers, useful for tracking positions after transformations.

4. **Combine with VSTACK for complete tables:** `VSTACK(Headers, HSTACK(...data...))` builds tables with headers in one formula.

5. **Name your HSTACK results:** Create named ranges using HSTACK formulas for reusable dynamic data structures.

6. **Check column order:** Arrays combine left to right. If column order matters, arrange inputs accordingly.

7. **Handle formula result arrays:** HSTACK accepts FILTER, SORT, UNIQUE results—just ensure row alignment.

8. **Use with CHOOSECOLS for selective combination:** `HSTACK(CHOOSECOLS(A, 1, 3), CHOOSECOLS(B, 2))` combines specific columns from different sources.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VSTACK]] | Combines arrays vertically (stacks rows) | When you need to stack data top-to-bottom instead of side-by-side |
| [[CHOOSECOLS]] | Extracts specific columns from array | When you need to reorder or subset columns, not combine arrays |
| [[INDEX]] | Returns portions of arrays | When HSTACK isn't available or for single-value extraction |

### Commonly Used Together

**[[VSTACK]]** - Combine arrays vertically

*Combined with HSTACK for complete table construction:*
```
=VSTACK(HSTACK(Header1, Header2), HSTACK(Data1, Data2))
```
Build headers and data separately, then stack.

---

**[[CHOOSECOLS]]** - Extract specific columns

*Combined with HSTACK for selective column combination:*
```
=HSTACK(CHOOSECOLS(Table1, 1, 3), CHOOSECOLS(Table2, 2, 4))
```
Extract specific columns from each source before combining.

---

**[[FILTER]]** - Filter rows by criteria

*Combined with HSTACK to merge filtered results:*
```
=HSTACK(FILTER(A:B, C:C>0), FILTER(D:E, C:C>0))
```
Same filter applied to different column groups, then combined.

---

**[[EXPAND]]** - Pad arrays to specific dimensions

*Combined with HSTACK to prevent #N/A padding:*
```
=HSTACK(Array1, EXPAND(Array2, ROWS(Array1), , 0))
```
Pad shorter array with zeros instead of #N/A.

---

**[[SEQUENCE]]** - Generate sequences

*Combined with HSTACK to add row numbers:*
```
=HSTACK(SEQUENCE(ROWS(Data)), Data)
```
Prepend sequential row numbers to data.

---

**[[SORT]]** - Sort arrays

*Combined with HSTACK after stacking:*
```
=SORT(HSTACK(A, B, C), 2, 1)
```
Stack first, then sort the combined result.

## Official Documentation

- **Microsoft Excel:** [HSTACK function](https://support.microsoft.com/en-us/office/hstack-function-98c4ab76-10fe-4b4f-8d5f-af1c125fe8c2)
- **Google Sheets:** [HSTACK function](https://support.google.com/docs/answer/13190683)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added to match Excel's dynamic array capabilities |

---

*Last updated: 2026-01-10*
