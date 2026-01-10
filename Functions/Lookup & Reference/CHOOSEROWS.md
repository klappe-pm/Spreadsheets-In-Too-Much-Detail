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
- row-selection
- data-extraction
- dynamic-arrays
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Choose Rows
- Select Rows
- Row Picker
- Extract Rows
tags:
- array
- rows
- extraction
- dynamic-arrays
- data-manipulation
---

# CHOOSEROWS

## Description

**CHOOSEROWS** extracts specific rows from an array or range and returns them in the order you specify. It's the row-oriented counterpart to CHOOSECOLS—while CHOOSECOLS picks columns, CHOOSEROWS picks rows. Point at a table and say "give me rows 1, 5, and 3, in that order" and CHOOSEROWS delivers exactly those rows. This function is essential for creating custom data subsets, extracting specific records, or reordering data vertically without modifying your source.

The power of CHOOSEROWS comes from its complete flexibility: select any rows in any order, repeat rows for comparison layouts, or use negative numbers to count from the bottom of the array. Need the last row without knowing how many rows exist? Use -1. Want the first and last rows together? Combine 1 and -1. Want to reverse the entire dataset? List the row numbers backwards. This eliminates complex INDEX+MATCH workarounds and manual row copying.

**Key behaviors to understand:** CHOOSEROWS returns a dynamic array that spills automatically. If you request a row number that doesn't exist (like row 50 from a 20-row range), you'll get a #VALUE! error. All columns from the source array are preserved—CHOOSEROWS only affects which rows appear. Negative row numbers work intuitively: -1 is the last row, -2 is second-to-last, and so forth. When combined with CHOOSECOLS, you can extract any rectangular subset from a larger dataset.

**Platform availability:** CHOOSEROWS is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically, including support for negative row indices. For older Excel versions, you'll need to use INDEX with row arrays, OFFSET, or helper columns to achieve similar results—none as elegant as CHOOSEROWS.

## Syntax

> [!f(x)] CHOOSEROWS Syntax
>
> ```
> =CHOOSEROWS(array, row_num1, [row_num2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to extract rows. Can be a cell range, named range, or array returned by another formula. |
| `row_num1` | Yes | The row number of the first row to return. Use positive numbers to count from the top (1 = first row) or negative numbers to count from the bottom (-1 = last row). |
| `row_num2, ...` | No | Additional row numbers to return. You can specify as many as needed. Rows are returned in the order specified. The same row can be listed multiple times. |

### Return Value

Returns an array containing the specified rows from the source array, in the order specified. The result has as many rows as row numbers provided and the same number of columns as the source array. Returns #VALUE! if any row number is 0 or refers to a non-existent row position.

## Examples

> [!f(x)] CHOOSEROWS Examples

### Example 1: Extract a Single Row
```
=CHOOSEROWS(A1:E10, 3)
```
**Result:** Returns the 3rd row (A3:E3) as a single-row array with 5 columns

**Explanation:** The simplest use of CHOOSEROWS—extracting just one row from a range. This returns all columns from row 3 of the source range. While simple, this pattern is powerful when the source is a dynamic array from FILTER or SORT, and you need a specific row from the result.

---

### Example 2: Extract Multiple Rows in Original Order
```
=CHOOSEROWS(A1:D20, 1, 5, 10, 15)
```
**Result:** Returns rows 1, 5, 10, and 15 as a 4-row by 4-column array

**Explanation:** This extracts four specific rows, skipping all others. Useful when you have a large dataset but only need specific records—perhaps header row (1), and quarterly summary rows (5, 10, 15). The result is a compact 4-row output.

---

### Example 3: Reorder Rows
```
=CHOOSEROWS(A1:D5, 5, 3, 1, 4, 2)
```
**Result:** Returns all 5 rows but in the order: 5th, 3rd, 1st, 4th, 2nd

**Explanation:** CHOOSEROWS doesn't just extract—it reorders. Here, we take a 5-row range and completely rearrange the row order. This is useful for custom sorting that doesn't follow alphabetical or numerical rules, or for creating specific presentation orders.

---

### Example 4: Using Negative Numbers for Bottom-Up Selection
```
=CHOOSEROWS(A1:D100, -1, -2, -3)
```
**Result:** Returns the last three rows in reverse order (row 100, 99, 98)

**Explanation:** Negative row numbers count from the bottom of the array. -1 is the last row, -2 is second-to-last, and so on. This is invaluable when you don't know (or don't want to hardcode) the total row count but need rows from the end—like "most recent entries" in a growing dataset.

---

### Example 5: Combine Positive and Negative Row References
```
=CHOOSEROWS(A1:D50, 1, 2, -2, -1)
```
**Result:** Returns the first two rows and the last two rows (rows 1, 2, 49, 50)

**Explanation:** Mix positive and negative references to get rows from both ends. This is perfect for "header + footer" extractions, or when you need the beginning and end of a dataset for comparison. If rows are added in the middle, the formula still captures the correct first and last rows.

---

### Example 6: Repeat a Row
```
=CHOOSEROWS(A1:D10, 1, 1, 1)
```
**Result:** Returns the first row three times (a 3-row by 4-column array)

**Explanation:** The same row can be specified multiple times. This creates a result where the row is duplicated. Useful for creating comparison layouts, filling placeholder rows, or testing scenarios where you need repeated data.

---

### Example 7: Extract Rows from a Filtered Result
```
=CHOOSEROWS(FILTER(A1:E100, B1:B100="Active"), 1, 5, 10)
```
**Result:** Returns rows 1, 5, and 10 from the filtered dataset

**Explanation:** CHOOSEROWS works seamlessly with other dynamic array functions. Here, FILTER returns all rows where column B equals "Active", and CHOOSEROWS then extracts specific rows from that filtered result. This is powerful for getting specific records from a pre-filtered list.

---

### Example 8: Get Header and Data Rows Separately
```
Header: =CHOOSEROWS(DataWithHeader, 1)
Data: =CHOOSEROWS(DataWithHeader, SEQUENCE(ROWS(DataWithHeader)-1, 1, 2))
```
**Result:** Separates header row from data rows

**Explanation:** The first formula gets just the header (row 1). The second uses SEQUENCE to generate row numbers starting at 2 through the last row, effectively getting all data rows without the header. This is useful when you need to process headers and data differently.

---

### Example 9: Reverse All Rows
```
=CHOOSEROWS(A1:D10, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1)
```
**Result:** Returns all 10 rows in reverse order (bottom to top)

**Explanation:** List row numbers in reverse order to flip the entire array vertically. For dynamic reversal regardless of row count, use: `=CHOOSEROWS(A1:D10, SEQUENCE(10, 1, 10, -1))` which counts down from 10 to 1.

---

### Example 10: Select Every Other Row
```
=CHOOSEROWS(A1:D20, 1, 3, 5, 7, 9, 11, 13, 15, 17, 19)
```
**Result:** Returns odd-numbered rows (1, 3, 5, etc.)

**Explanation:** By listing specific row numbers, you can create patterns. This extracts every other row, which might be useful when alternating rows contain different types of data (like merged row pairs) or for sampling.

---

### Example 11: Dynamic Row Selection with SEQUENCE
```
=CHOOSEROWS(A1:D100, SEQUENCE(10, 1, 1, 5))
```
**Result:** Returns rows 1, 6, 11, 16, 21, 26, 31, 36, 41, 46

**Explanation:** SEQUENCE(10, 1, 1, 5) generates 10 numbers starting at 1 and incrementing by 5: {1;6;11;16;...}. CHOOSEROWS then extracts those specific rows. This pattern is useful for systematic sampling at regular intervals.

---

### Example 12: Get First and Last Row Together
```
=CHOOSEROWS(DataRange, 1, -1)
```
**Result:** Returns a 2-row array with the first and last rows

**Explanation:** A common pattern for getting boundary rows. Perfect for comparing the earliest and latest entries, or for creating a summary that shows the start and end of a dataset.

---

### Example 13: Combine with CHOOSECOLS for Precise Extraction
```
=CHOOSECOLS(CHOOSEROWS(A1:F100, 1, 10, 20, 30), 1, 3, 5)
```
**Result:** Returns columns 1, 3, and 5 from rows 1, 10, 20, and 30

**Explanation:** CHOOSEROWS first extracts the needed rows (4 rows), then CHOOSECOLS extracts the needed columns (3 columns). The result is a precisely targeted 4x3 subset of the original 100x6 data. This combination can extract any rectangular subset from any position.

---

### Example 14: Working with Sorted Data
```
=CHOOSEROWS(SORT(A2:D100, 3, -1), SEQUENCE(5))
```
**Result:** Returns the top 5 rows after sorting by column 3 descending

**Explanation:** SORT arranges data with highest values in column 3 first. CHOOSEROWS with SEQUENCE(5) extracts rows 1-5, giving you the "top 5" items. This is similar to using TAKE, but CHOOSEROWS offers more flexibility for non-contiguous selections.

---

### Example 15: Extract Random Sample of Rows
```
=CHOOSEROWS(Data, SORTBY(SEQUENCE(ROWS(Data)), RANDARRAY(ROWS(Data))))
```
**Result:** Returns all rows in random order (shuffle)

**Explanation:** This advanced pattern randomizes row order. SEQUENCE generates row numbers, RANDARRAY creates random values, and SORTBY reorders the row numbers randomly. CHOOSEROWS then extracts rows in this randomized order. Use with TAKE to get a random sample of N rows.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Row number is 0 | Row numbers must be positive (1 or greater) or negative (-1 or less). Zero is not valid. |
| `#VALUE!` | Row number exceeds array dimensions | If array has 50 rows, requesting row 51 or -51 fails. Check your row count with ROWS(). |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula. |
| `#NAME?` | Function not available in your Excel version | CHOOSEROWS requires Excel 365/2021 or Google Sheets. Not available in Excel 2019 or earlier. |
| `#REF!` | Source range is invalid | Verify that the array reference exists and is correctly formatted. |
| `#CALC!` | Array is empty or has no rows | Ensure the source array contains data. An empty result from FILTER could cause this. |

## Use Cases

### [[Top/Bottom N Analysis]]

**Scenario:** After sorting data by performance metrics, extract the top 10 and bottom 10 performers for a summary report, without showing the hundreds of rows in between.

**Implementation:**
```
Top 10 Performers:
=CHOOSEROWS(SORT(PerformanceData, 4, -1), SEQUENCE(10))

Bottom 10 Performers:
=CHOOSEROWS(SORT(PerformanceData, 4, 1), SEQUENCE(10))

Combined View (Top 5 and Bottom 5):
=CHOOSEROWS(SORT(PerformanceData, 4, -1), 1, 2, 3, 4, 5, -5, -4, -3, -2, -1)
```

**Business Application:** Performance reviews, sales rankings, quality metrics. Executives often want to see the best and worst performers without scrolling through entire datasets. This pattern instantly surfaces the extremes.

**Technical Details:** SORT arranges the data first (descending for top, ascending for bottom). SEQUENCE(10) generates row numbers 1-10. The combined view uses positive numbers for top rows and negative for bottom rows from the same sorted result.

---

### [[Data Sampling for Analysis]]

**Scenario:** A dataset has 10,000 rows, but you only need a representative sample for preliminary analysis—perhaps every 100th row, or a specific random sample.

**Implementation:**
```
Every 100th Row:
=CHOOSEROWS(LargeDataset, SEQUENCE(100, 1, 1, 100))

First, Middle, and Last Records:
=CHOOSEROWS(LargeDataset, 1, ROUNDUP(ROWS(LargeDataset)/2, 0), -1)

Random Sample of 50 Rows:
=TAKE(CHOOSEROWS(Data, SORTBY(SEQUENCE(ROWS(Data)), RANDARRAY(ROWS(Data)))), 50)
```

**Business Application:** Preliminary data analysis, quality spot-checks, audit sampling. When you can't analyze everything, systematic or random sampling provides insights without processing every record.

**Technical Details:** SEQUENCE(100, 1, 1, 100) generates {1, 101, 201, 301...}, selecting every 100th row. The random sample shuffles all row numbers then takes the first 50. For reproducible random samples, consider seeding with specific RANDARRAY parameters if supported.

---

### [[Custom Report Row Selection]]

**Scenario:** A financial report needs specific rows from a larger dataset: the header row, quarterly totals (rows 4, 8, 12, 16), and the annual total (last row). CHOOSEROWS extracts exactly these rows.

**Implementation:**
```
=CHOOSEROWS(FinancialData, 1, 4, 8, 12, 16, -1)
```
Or with named reference:
```
Summary Rows: 1, 4, 8, 12, 16 (stored in cells or as array)
=CHOOSEROWS(FinancialData, SummaryRowNumbers)
```

**Business Application:** Financial reporting, structured data extraction, custom summaries. When source data has a known structure (headers, subtotals, totals), CHOOSEROWS extracts just the summary rows for executive dashboards.

**Technical Details:** Store row numbers in a range and reference that range in CHOOSEROWS for easy maintenance. If the structure changes (quarterly becomes monthly), update the row numbers in one place rather than editing the formula.

---

### [[Before/After Comparison]]

**Scenario:** Track changes by comparing specific snapshots from time-series data. Extract the first occurrence (baseline), latest occurrence (current), and key milestones in between.

**Implementation:**
```
Baseline vs Current:
=CHOOSEROWS(TimeSeriesData, 1, -1)

Key Milestones (Start, Q1, Q2, Q3, Current):
=CHOOSEROWS(TimeSeriesData, 1, 13, 26, 39, -1)
```

**Business Application:** Trend analysis, progress tracking, change management. Comparing where you started, where you are, and key points along the way tells the story of progress without overwhelming detail.

**Technical Details:** Using -1 for the last row means the formula automatically adjusts as new data is added. Combine with conditional formatting to highlight changes between the comparison points.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill into adjacent cells
- **Negative indices:** Fully supported (-1 for last row, etc.)
- **Array size limits:** Subject to Excel's grid limits (1,048,576 rows)
- **Recalculation:** Part of the calculation chain; updates when source data changes

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand (spill)
- **Negative indices:** Fully supported, identical behavior to Excel
- **Array size limits:** Subject to Sheets' limits (up to 10 million cells per spreadsheet)
- **Performance:** Efficient for most use cases; very large arrays may slow calculation

### Both Platforms

- Row numbers must be non-zero integers
- Same row can be repeated in the output
- Works with arrays from other functions (FILTER, SORT, UNIQUE, etc.)
- Returns #VALUE! for invalid row references

## Tips and Best Practices

1. **Use negative indices for end-relative selection:** When you need the last few rows but don't want to count or hardcode positions, use -1, -2, -3. This makes formulas resilient to data growth.

2. **Combine with CHOOSECOLS for precise extraction:** Use CHOOSEROWS for row selection and CHOOSECOLS for column selection to extract any rectangular subset: `=CHOOSECOLS(CHOOSEROWS(Data, 1, 5, 10), 2, 4)`.

3. **Use SEQUENCE for calculated row lists:** When you need the first N rows, every Nth row, or a counted sequence, generate row numbers with SEQUENCE: `=CHOOSEROWS(Data, SEQUENCE(5))` gets first 5 rows.

4. **Chain with FILTER, SORT, UNIQUE:** CHOOSEROWS works on any array, including results from other functions. Filter first, then extract specific rows from the filtered result.

5. **Store row numbers in cells for flexibility:** Instead of hardcoding `CHOOSEROWS(Data, 1, 5, 10)`, reference cells containing row numbers. This allows users to change selections without editing formulas.

6. **Remember rows are relative to the array, not the sheet:** Row 1 in CHOOSEROWS is the first row of the specified array, not row 1 of the worksheet.

7. **Handle empty results from FILTER:** If FILTER returns no matches and you apply CHOOSEROWS, you'll get #CALC!. Wrap in IFERROR or check for empty results first.

8. **For older Excel versions, use INDEX alternatives:** `=INDEX(Data, {1;5;10}, 0)` can return multiple rows in older Excel versions, though less elegantly.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CHOOSECOLS]] | Extracts specific columns from an array | When you need to select columns instead of rows, or combine with CHOOSEROWS for both |
| [[INDEX]] | Returns value or reference from array position | When you need a single value or when CHOOSEROWS isn't available (older Excel) |
| [[TAKE]] | Returns rows/columns from start or end of array | When you need a contiguous block from the beginning or end, not scattered selections |
| [[DROP]] | Removes rows/columns from start or end of array | When you want to exclude leading or trailing rows rather than select specific ones |

### Commonly Used Together

**[[FILTER]]** - Filter rows based on criteria

*Combined with CHOOSEROWS to filter then select specific rows:*
```
=CHOOSEROWS(FILTER(A1:F100, C1:C100>1000), 1, 5, 10)
```
First FILTER selects rows meeting criteria, then CHOOSEROWS extracts specific rows from the filtered result.

---

**[[SORT]]** - Sort array by specified columns

*Combined with CHOOSEROWS for sorted subset:*
```
=CHOOSEROWS(SORT(Data, 2, -1), SEQUENCE(10))
```
SORT arranges the data by column 2 descending, then CHOOSEROWS gets the top 10 rows.

---

**[[CHOOSECOLS]]** - Extract specific columns

*Combined with CHOOSEROWS for precise rectangular extraction:*
```
=CHOOSECOLS(CHOOSEROWS(Data, 1, 5, 10, 15), 2, 4, 6)
```
Extract rows 1, 5, 10, 15 and columns 2, 4, 6—any subset from any position.

---

**[[VSTACK]]** - Combine arrays vertically

*Combined with CHOOSEROWS to merge selected rows from multiple tables:*
```
=VSTACK(CHOOSEROWS(Table1, 1, 5), CHOOSEROWS(Table2, 1, 5))
```
Extract specific rows from each table and stack them together.

---

**[[SEQUENCE]]** - Generate number sequences

*Combined with CHOOSEROWS for dynamic row selection:*
```
=CHOOSEROWS(Data, SEQUENCE(10, 1, 1, 2))
```
SEQUENCE(10, 1, 1, 2) generates {1,3,5,7,...19}, selecting odd-numbered rows 1-19.

---

**[[ROWS]]** - Count rows in a range

*Combined with CHOOSEROWS for relative positioning:*
```
=CHOOSEROWS(Data, ROWS(Data)-2, ROWS(Data)-1, ROWS(Data))
```
Get the last 3 rows using ROWS to determine the count. (Though negative indices are simpler: -3, -2, -1)

## Official Documentation

- **Microsoft Excel:** [CHOOSEROWS function](https://support.microsoft.com/en-us/office/chooserows-function-51ace882-9bab-4a44-9625-7274ef7507a3)
- **Google Sheets:** [CHOOSEROWS function](https://support.google.com/docs/answer/13196659)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added to match Excel's dynamic array capabilities |

---

*Last updated: 2026-01-10*
