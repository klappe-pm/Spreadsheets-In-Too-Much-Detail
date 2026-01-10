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
- vertical-stacking
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Vertical Stack
- Stack Vertically
- Combine Rows
- Row Append
- Union Arrays
tags:
- array
- stack
- combine
- dynamic-arrays
- vertical
---

# VSTACK

## Description

**VSTACK (Vertical Stack)** combines multiple arrays vertically, appending them top to bottom into a single array. Think of it as stacking arrays on top of each other—Array A on top, Array B in the middle, Array C at the bottom. If you have January data in one range, February in another, and March in a third, VSTACK merges them into a continuous dataset. It's the vertical counterpart to HSTACK, which stacks arrays side by side.

The function accepts any number of arrays and joins them top to bottom in the order specified. Each array's rows are appended sequentially. You can combine ranges, single rows, formula results, or literal arrays. VSTACK is essential for consolidating data from multiple sources, adding header rows to datasets, appending new records, or building unified tables from separate pieces.

**Key behaviors to understand:** All arrays should have the same number of columns for clean results. If arrays have different column counts, VSTACK pads narrower arrays with #N/A errors on the right to match the widest array. Single values are treated as 1-row, 1-column arrays and padded accordingly. VSTACK preserves the row order within each array and maintains the order of arrays as specified. The result is a dynamic array that spills into adjacent cells.

**Platform availability:** VSTACK is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically. For older Excel versions, you could define ranges that span multiple areas or use complex INDEX/INDIRECT constructions—neither as clean as VSTACK.

## Syntax

> [!f(x)] VSTACK Syntax
>
> ```
> =VSTACK(array1, [array2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first array or range. This will form the top rows of the result. |
| `array2, ...` | No | Additional arrays or ranges to append vertically. Each subsequent array is added below the previous. You can include as many arrays as needed. |

### Return Value

Returns a single array containing all input arrays stacked vertically. The result has as many rows as the sum of all input array rows and as many columns as the widest input array. Arrays with fewer columns are padded with #N/A on the right.

## Examples

> [!f(x)] VSTACK Examples

### Example 1: Basic Two-Array Stack
```
=VSTACK(A1:D10, A15:D25)
```
**Result:** Returns a 21-row array combining both ranges vertically

**Explanation:** The simplest VSTACK—stacking two ranges on top of each other. Rows from A1:D10 come first (rows 1-10), then rows from A15:D25 (becoming rows 11-21 in the result).

---

### Example 2: Stack Multiple Ranges
```
=VSTACK(Jan_Data, Feb_Data, Mar_Data, Apr_Data)
```
**Result:** Combines four monthly datasets into one continuous array

**Explanation:** Stack any number of arrays. Named ranges make the formula readable. If each month has 30 rows, the result has 120 rows. Order matters—January is on top, April at bottom.

---

### Example 3: Add Header Row to Data
```
=VSTACK({"ID", "Name", "Amount", "Date"}, A2:D100)
```
**Result:** Data with header row prepended

**Explanation:** A literal array (the headers) is stacked above the data range. This is essential when data doesn't include headers or when building tables from formula results that lack headers.

---

### Example 4: Add Footer/Total Row
```
=VSTACK(Data, {"Total", "", SUM(C:C), ""})
```
**Result:** Data with a totals row appended at the bottom

**Explanation:** Stack a calculated row after the data. The literal array includes labels and formulas. This creates a complete table with summary row.

---

### Example 5: Combine Filtered Results
```
=VSTACK(
  FILTER(Data, Region="East"),
  FILTER(Data, Region="West")
)
```
**Result:** East region records followed by West region records

**Explanation:** Stack the results of different FILTER operations. Each filter returns a different subset; VSTACK combines them into a unified list. Useful for creating combined views of segmented data.

---

### Example 6: Arrays with Different Column Counts
```
=VSTACK(A1:C10, A15:D20)
```
**Result:** 16-row array with 4 columns; first 10 rows have #N/A in column 4

**Explanation:** The first array has 3 columns, the second has 4. VSTACK pads the narrower array with #N/A. Use EXPAND on narrower arrays to specify different padding if needed.

---

### Example 7: Build Complete Table from Parts
```
=VSTACK(
  HeaderRow,
  DROP(Sheet1Data, 1),
  DROP(Sheet2Data, 1),
  DROP(Sheet3Data, 1)
)
```
**Result:** Headers plus data from 3 sheets (with duplicate headers removed)

**Explanation:** Consolidate multi-sheet data. Take headers from one source, DROP headers from others to avoid duplicates, then VSTACK everything. This is a common pattern for data consolidation.

---

### Example 8: Single Value Stacking
```
=VSTACK("Header", A1:A5)
```
**Result:** 6-row array; "Header" in row 1, A1:A5 values in rows 2-6

**Explanation:** A single value becomes a 1-row array. VSTACK places it above the range. Since both have 1 column, no padding occurs. Perfect for adding a title to a single-column list.

---

### Example 9: Stack with Calculated Arrays
```
=VSTACK(
  SourceData,
  HSTACK(MAX(Col1)+1, "", SUM(Col3), AVERAGE(Col4))
)
```
**Result:** Data with a summary calculation row appended

**Explanation:** The second argument is a dynamically calculated row using HSTACK to build a multi-column array from individual calculations. VSTACK appends this summary row to the data.

---

### Example 10: Consolidate with Timestamps
```
=VSTACK(
  HSTACK(Data1, "Source1"),
  HSTACK(Data2, "Source2"),
  HSTACK(Data3, "Source3")
)
```
**Result:** Combined data with source identifier column

**Explanation:** Use HSTACK to add a source identifier column to each dataset, then VSTACK to combine them. This pattern tracks data origin in consolidated outputs.

---

### Example 11: Create Running Log
```
=VSTACK(ExistingLog, HSTACK(NOW(), NewEntry, UserName))
```
**Result:** Existing log with new timestamped entry added

**Explanation:** Append new entries to an existing log. Each new entry is constructed with HSTACK, then VSTACK adds it to the bottom. Note: This requires the formula to update ExistingLog reference after entry.

---

### Example 12: Stack Unique Values from Multiple Sources
```
=UNIQUE(VSTACK(List1, List2, List3))
```
**Result:** Unique values from all three lists combined

**Explanation:** First VSTACK combines all lists, then UNIQUE removes duplicates. This is powerful for merging reference lists or creating unified lookup values.

---

### Example 13: Conditional Stacking
```
=IF(IncludeArchive, VSTACK(CurrentData, ArchiveData), CurrentData)
```
**Result:** Current data only, or current plus archive based on toggle

**Explanation:** Use IF to conditionally include additional data. When IncludeArchive is TRUE, both datasets combine. When FALSE, only current data appears.

---

### Example 14: Stack with Empty Row Separator
```
=VSTACK(Data1, {"", "", "", ""}, Data2)
```
**Result:** Two datasets with a blank row between them

**Explanation:** Insert a blank row (array of empty strings) between datasets for visual separation. Useful for reports where you want visible breaks between sections.

---

### Example 15: Build Hierarchical Structure
```
=VSTACK(
  {"Category A"},
  HSTACK("  ", ItemsA),
  {"Category B"},
  HSTACK("  ", ItemsB)
)
```
**Result:** Categories with indented items below each

**Explanation:** Create visual hierarchy by stacking category headers and indented item lists. HSTACK adds indent spacing to items.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A in results` | Arrays have different column counts; narrower ones are padded | Use EXPAND to pad with preferred values, or ensure arrays have matching columns |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula |
| `#NAME?` | Function not available in your Excel version | VSTACK requires Excel 365/2021 or Google Sheets |
| `#REF!` | One of the source ranges is invalid | Verify all array references exist and are correctly formatted |
| `#CALC!` | One of the stacked arrays is empty | Handle empty arrays with IFERROR or check with ROWS() before stacking |

## Use Cases

### [[Multi-Source Data Consolidation]]

**Scenario:** Monthly data arrives in separate sheets or files—January on Sheet1, February on Sheet2, etc. VSTACK combines them into a unified dataset for annual analysis.

**Implementation:**
```
Annual Data (assuming headers on each sheet):
=VSTACK(
  Jan_Data,
  DROP(Feb_Data, 1),
  DROP(Mar_Data, 1),
  DROP(Apr_Data, 1),
  DROP(May_Data, 1),
  DROP(Jun_Data, 1)
)
```
Or with source tracking:
```
=VSTACK(
  HSTACK(Jan_Data, "January"),
  HSTACK(DROP(Feb_Data, 1), "February"),
  ...
)
```

**Business Application:** Financial consolidation, sales reporting, operational dashboards. Multiple time periods or departments combine into unified datasets.

**Technical Details:** DROP(Data, 1) removes header rows from subsequent datasets to avoid duplicate headers. HSTACK adds source identifiers. For large consolidations, consider performance—VSTACK with many large arrays can be slow.

---

### [[Building Dynamic Tables with Headers]]

**Scenario:** Formula results (from FILTER, SORT, UNIQUE) don't include headers. VSTACK adds headers to create complete, labeled tables.

**Implementation:**
```
Filtered Data with Headers:
=VSTACK(
  {"Product", "Region", "Sales", "Date"},
  FILTER(SalesData, Region="East")
)

Sorted Top N with Headers:
=VSTACK(
  {"Rank", "Name", "Score"},
  HSTACK(SEQUENCE(10), TAKE(SORT(Data, 2, -1), 10))
)
```

**Business Application:** Report generation, data presentation, dashboard tables. Professional output requires labeled columns.

**Technical Details:** Ensure header column count matches data columns. If data has dynamic column counts, construct headers dynamically or use EXPAND to align.

---

### [[Creating Lookup Reference Tables]]

**Scenario:** Lookup values are scattered across multiple lists or sheets. VSTACK consolidates them into a single reference for XLOOKUP or INDEX/MATCH.

**Implementation:**
```
Master Product List:
=UNIQUE(VSTACK(
  ProductList1,
  ProductList2,
  NewProducts,
  LegacyProducts
))

Reference Table for XLOOKUP:
=XLOOKUP(
  SearchValue,
  CHOOSECOLS(VSTACK(Table1, Table2, Table3), 1),
  CHOOSECOLS(VSTACK(Table1, Table2, Table3), 2, 3)
)
```

**Business Application:** Master data management, reference tables, lookup consolidation. Unified references ensure consistent lookups.

**Technical Details:** Wrap in UNIQUE to eliminate duplicates. For very large consolidated lookups, consider performance impact. Named ranges for the VSTACK result can improve formula readability.

---

### [[Adding Summary Rows to Data]]

**Scenario:** Tables need totals, averages, or other summary rows at the bottom. VSTACK appends calculated rows to data.

**Implementation:**
```
Sales Table with Totals:
=VSTACK(
  SalesData,
  HSTACK("TOTAL", "", SUM(SalesCol), AVERAGE(PriceCol), MAX(DateCol))
)

Data with Multiple Summary Rows:
=VSTACK(
  Data,
  HSTACK("Subtotal", SUM(Column1), SUM(Column2)),
  HSTACK("Tax (10%)", SUM(Column1)*0.1, SUM(Column2)*0.1),
  HSTACK("Grand Total", SUM(Column1)*1.1, SUM(Column2)*1.1)
)
```

**Business Application:** Financial reports, invoices, summary tables. Complete tables include both detail and summary.

**Technical Details:** Each summary row is constructed with HSTACK from individual calculations. Match column count and structure to the data above.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill
- **Column padding:** #N/A for mismatched column counts
- **Performance:** Efficient for reasonable array sizes

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand
- **Column padding:** Same behavior as Excel
- **Array syntax:** Semicolons for rows, commas for columns in literals
- **Alternative:** Can also use curly braces syntax: `{Array1; Array2}`

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function | VSTACK() | VSTACK() or {A;B} syntax |
| Array literal separator | Semicolon for rows | Semicolon for rows |
| Padding behavior | #N/A | #N/A |

### Google Sheets Alternative Syntax
```
={A1:C10; A15:C20}
```
This curly brace syntax with semicolons achieves the same as `VSTACK(A1:C10, A15:C20)` in Google Sheets.

## Tips and Best Practices

1. **Match column counts for clean results:** Arrays with different column counts get #N/A padding. Use EXPAND or CHOOSECOLS to normalize column counts before stacking.

2. **Use DROP to remove duplicate headers:** When combining datasets that each have headers, `DROP(Data, 1)` removes the header row before stacking.

3. **Add source identifiers with HSTACK:** `HSTACK(Data, "Source1")` adds a tracking column before stacking multiple sources.

4. **Combine with UNIQUE for deduplication:** `UNIQUE(VSTACK(...))` consolidates and removes duplicates in one formula.

5. **Build complete tables:** `VSTACK(Headers, Data, TotalRow)` constructs a full table with header, data, and summary.

6. **Check for empty arrays:** If any stacked array might be empty (like a FILTER with no results), wrap in IFERROR or handle the empty case.

7. **In Google Sheets, consider {A;B} syntax:** For simple stacking, `={Range1; Range2}` is more concise than `=VSTACK(Range1, Range2)`.

8. **Use named ranges for readability:** `VSTACK(JanuaryData, FebruaryData, MarchData)` is clearer than cell references.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[HSTACK]] | Combines arrays horizontally (side by side) | When you need to add columns, not rows |
| [[CHOOSEROWS]] | Select specific rows from array | When you need specific rows, not all rows from multiple arrays |
| [[INDEX]] | Returns portions of arrays | When VSTACK isn't available or for specific subset extraction |

### Commonly Used Together

**[[HSTACK]]** - Combine arrays horizontally

*Combined with VSTACK for complex table building:*
```
=VSTACK(
  HSTACK(Header1, Header2, Header3),
  HSTACK(Data1, Data2, Data3)
)
```
Build headers and data with HSTACK, combine with VSTACK.

---

**[[DROP]]** - Remove rows from edges

*Combined with VSTACK to avoid duplicate headers:*
```
=VSTACK(Sheet1Data, DROP(Sheet2Data, 1), DROP(Sheet3Data, 1))
```
Keep headers from first sheet only.

---

**[[UNIQUE]]** - Remove duplicates

*Combined with VSTACK for consolidated unique lists:*
```
=UNIQUE(VSTACK(List1, List2, List3))
```
Combine lists and deduplicate.

---

**[[FILTER]]** - Filter rows by criteria

*Combined with VSTACK to combine filtered subsets:*
```
=VSTACK(FILTER(Data, A="X"), FILTER(Data, A="Y"))
```
Stack different filtered results.

---

**[[EXPAND]]** - Pad arrays to dimensions

*Combined with VSTACK to normalize column counts:*
```
=VSTACK(EXPAND(Narrow, ROWS(Narrow), 5, ""), Wide)
```
Pad narrower array before stacking to avoid #N/A.

---

**[[SORT]]** - Sort arrays

*Combined with VSTACK for sorted consolidated data:*
```
=SORT(VSTACK(Data1, Data2), SortCol, 1)
```
Stack first, then sort the combined result.

## Official Documentation

- **Microsoft Excel:** [VSTACK function](https://support.microsoft.com/en-us/office/vstack-function-a4b86897-be0f-48fc-adca-fcc10d795a9c)
- **Google Sheets:** [VSTACK function](https://support.google.com/docs/answer/13190689)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added alongside Excel; Google Sheets also supports {A;B} syntax |

---

*Last updated: 2026-01-10*
