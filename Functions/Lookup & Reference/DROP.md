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
- data-extraction
- dynamic-arrays
- row-removal
- column-removal
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Drop Rows
- Drop Columns
- Remove Rows
- Array Trim
tags:
- array
- drop
- extraction
- dynamic-arrays
- data-manipulation
---

# DROP

## Description

**DROP** removes a specified number of rows and/or columns from the beginning or end of an array. Think of it as trimming the edges off your data—need to skip the first 5 header rows? DROP them. Want to exclude the last 2 columns of totals? DROP them. It's the inverse of TAKE: where TAKE says "give me this many," DROP says "remove this many." This complementary relationship makes them essential partners for array manipulation.

The elegance of DROP comes from its simplicity and the clever use of positive and negative numbers. Positive row numbers drop from the TOP of the array; negative row numbers drop from the BOTTOM. Similarly, positive column numbers drop from the LEFT; negative column numbers drop from the RIGHT. This intuitive system means `DROP(Data, 2)` removes the first 2 rows, while `DROP(Data, -2)` removes the last 2 rows. You can combine both: `DROP(Data, 1, -1)` removes the first row and the last column simultaneously.

**Key behaviors to understand:** DROP returns what remains after removal—not what was removed. If you drop more rows or columns than exist in the array, you'll get a #CALC! error. Dropping zero rows or zero columns is valid and returns the unchanged array in that dimension. DROP preserves the relative order of all remaining elements; it simply slices off the specified edges. The result is a dynamic array that spills into adjacent cells.

**Platform availability:** DROP is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically. For older Excel versions, combinations of INDEX, OFFSET, and ROW/COLUMN functions can simulate DROP, but with significantly more complexity.

## Syntax

> [!f(x)] DROP Syntax
>
> ```
> =DROP(array, rows, [columns])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to drop rows and/or columns. Can be a cell range, named range, or array returned by another formula. |
| `rows` | Yes | The number of rows to drop. Positive numbers drop from the top; negative numbers drop from the bottom. Use 0 to keep all rows. |
| `columns` | No | The number of columns to drop. Positive numbers drop from the left; negative numbers drop from the right. Use 0 or omit to keep all columns. Defaults to 0. |

### Return Value

Returns an array containing the remaining data after dropping the specified rows and/or columns. The result has (original rows - abs(rows)) rows and (original columns - abs(columns)) columns. Returns #CALC! if you attempt to drop more rows or columns than exist in the array.

## Examples

> [!f(x)] DROP Examples

### Example 1: Drop the First Row (Header Removal)
```
=DROP(A1:D10, 1)
```
**Result:** Returns rows 2-10 of the original range (9 rows by 4 columns)

**Explanation:** This is the most common use of DROP—removing a header row. If A1:D10 contains a header in row 1 followed by 9 rows of data, this formula returns just the data. It's cleaner than manually adjusting range references like A2:D10, especially when the source is a dynamic array.

---

### Example 2: Drop Multiple Rows from the Top
```
=DROP(A1:D20, 5)
```
**Result:** Returns rows 6-20 (15 rows by 4 columns)

**Explanation:** Drops the first 5 rows, leaving everything from row 6 onward. Useful when your data has multiple header rows or metadata rows at the top that you want to exclude from processing.

---

### Example 3: Drop Rows from the Bottom
```
=DROP(A1:D20, -3)
```
**Result:** Returns rows 1-17 (17 rows by 4 columns), excluding the last 3 rows

**Explanation:** Negative row numbers remove from the bottom. This is perfect for excluding footer rows, total rows, or incomplete recent entries that you want to ignore.

---

### Example 4: Drop Columns from the Left
```
=DROP(A1:F10, 0, 2)
```
**Result:** Returns columns C through F (10 rows by 4 columns)

**Explanation:** The `0` for rows means "keep all rows." The `2` for columns drops the first 2 columns. Useful when the first columns contain row identifiers or metadata you don't need in your output.

---

### Example 5: Drop Columns from the Right
```
=DROP(A1:F10, 0, -2)
```
**Result:** Returns columns A through D (10 rows by 4 columns)

**Explanation:** Negative column numbers remove from the right. This drops the last 2 columns—useful for excluding calculated columns, notes, or totals from the output.

---

### Example 6: Drop Rows from Both Ends
```
=DROP(DROP(A1:D20, 2), -2)
```
**Result:** Returns rows 3-18 (16 rows), excluding first 2 and last 2 rows

**Explanation:** Nest DROP functions to remove from both ends. The inner DROP removes 2 rows from the top; the outer DROP removes 2 rows from the bottom of what remains. This is useful for excluding header and footer sections simultaneously.

---

### Example 7: Drop Rows and Columns Simultaneously
```
=DROP(A1:F20, 1, 1)
```
**Result:** Returns rows 2-20 and columns B-F (19 rows by 5 columns)

**Explanation:** Specify both rows and columns to trim from both dimensions at once. Here, we drop the first row (likely a header) and the first column (perhaps row numbers or IDs).

---

### Example 8: Drop First Row and Last Column
```
=DROP(DROP(A1:E10, 1), 0, -1)
```
**Result:** Returns rows 2-10 and columns A-D

**Explanation:** The inner DROP removes the header row. The outer DROP keeps all rows (0) but removes the last column. This pattern is common when you have headers to remove and a totals column to exclude.

---

### Example 9: Using DROP with FILTER Results
```
=DROP(FILTER(A1:E100, B1:B100="Active"), 0, 2)
```
**Result:** Returns filtered rows with the first 2 columns removed

**Explanation:** DROP works seamlessly with dynamic array results. Here, FILTER extracts active records, then DROP removes the first two columns (perhaps the Active flag column and an ID column aren't needed in the final output).

---

### Example 10: Remove Header from Sorted Data
```
=DROP(SORT(A1:D100, 2, 1), 1)
```
**Result:** Returns sorted data without the header row... BUT WAIT!

**Explanation:** This example shows a **common mistake**. SORT includes all rows in the sort, including the header. The header likely won't stay at row 1 after sorting. Instead, sort just the data: `=SORT(DROP(A1:D100, 1), 2, 1)` drops the header FIRST, then sorts.

---

### Example 11: Process All But Most Recent Entries
```
=DROP(DataRange, -10)
```
**Result:** Returns all rows except the last 10

**Explanation:** When you want to analyze historical data but exclude recent, possibly incomplete entries, drop from the bottom. If your data grows daily, this formula always excludes the most recent 10 entries without manual updates.

---

### Example 12: Drop with VSTACK for Conditional Headers
```
=VSTACK(CHOOSEROWS(Data, 1), DROP(Data, 2))
```
**Result:** Returns header row plus all rows except row 2

**Explanation:** Sometimes you need to keep the header but drop a specific row. This combines CHOOSEROWS to grab row 1, then DROP to skip rows 1-2, and VSTACK to recombine them. Row 2 (perhaps a subheader or summary) is excluded.

---

### Example 13: Trim Borders from Imported Data
```
=DROP(DROP(DROP(DROP(RawImport, 3), -2), 0, 1), 0, -1)
```
**Result:** Removes first 3 rows, last 2 rows, first column, and last column

**Explanation:** When importing data with formatting borders, you might need to trim multiple edges. While complex, this nested DROP approach is more readable than calculating INDEX ranges manually. The pattern is: top, bottom, left, right.

---

### Example 14: Dynamic Column Count Handling
```
=DROP(DataTable, 0, COLUMNS(DataTable)-3)
```
**Result:** Returns only the last 3 columns

**Explanation:** Calculate how many columns to drop dynamically. If the table has 10 columns and you want the last 3, drop 7 from the left. This is similar to using TAKE with -3, but demonstrates DROP's flexibility for calculated scenarios.

---

### Example 15: Pair with TAKE for Middle Section
```
=DROP(TAKE(Data, 50), 10)
```
**Result:** Returns rows 11-50 (the middle section)

**Explanation:** TAKE(Data, 50) gets the first 50 rows. DROP(..., 10) removes the first 10 of those, leaving rows 11-50. This TAKE-then-DROP pattern extracts any contiguous section from the middle of an array.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#CALC!` | Dropping more rows/columns than exist in the array | If array has 10 rows and you DROP 15, error occurs. Verify array dimensions first. |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula. |
| `#NAME?` | Function not available in your Excel version | DROP requires Excel 365/2021 or Google Sheets. Not available in Excel 2019 or earlier. |
| `#VALUE!` | Non-numeric row or column arguments | Ensure row and column counts are numbers or cells containing numbers. |
| `#REF!` | Source range is invalid | Verify that the array reference exists and is correctly formatted. |
| `Empty result` | Dropped all rows or all columns | If you drop exactly as many as exist, you get an empty array (which may cause downstream errors). |

## Use Cases

### [[Header and Footer Removal]]

**Scenario:** Imported data has 3 header rows at the top and 2 summary rows at the bottom. You need just the data rows for analysis.

**Implementation:**
```
Remove top 3 rows:
=DROP(ImportedData, 3)

Remove top 3 and bottom 2:
=DROP(DROP(ImportedData, 3), -2)

Alternative with defined names:
HeaderRows = 3
FooterRows = 2
=DROP(DROP(ImportedData, HeaderRows), -FooterRows)
```

**Business Application:** Data import cleanup, report processing, ETL workflows. Most real-world data has metadata rows that need stripping before analysis. DROP provides a clean, maintainable approach.

**Technical Details:** When header/footer row counts change, update the DROP parameters rather than editing all range references. For data with dynamic header sizes, consider using MATCH to find the data start row and calculate the drop count.

---

### [[Data Pipeline Processing]]

**Scenario:** Building a processing pipeline where each step transforms the data. Source data includes ID columns and status columns that aren't needed in the final output.

**Implementation:**
```
Raw Data: ID, Name, Email, Phone, Status, Notes (6 columns)
Output Needed: Name, Email, Phone (middle 3 columns)

=DROP(DROP(RawData, 0, 1), 0, -2)
```
Or equivalently:
```
=CHOOSECOLS(RawData, 2, 3, 4)
```

**Business Application:** Report generation, data export preparation, API data formatting. When source data has more columns than needed, DROP cleanly removes the extras.

**Technical Details:** For complex column removal (non-contiguous), CHOOSECOLS may be more appropriate. DROP excels when you need to remove contiguous sections from edges. Combine approaches: `=DROP(CHOOSECOLS(Data, 1, 3, 5), 1)` selects columns then drops the header.

---

### [[Time Series Data Analysis]]

**Scenario:** Analyzing historical data but need to exclude the most recent N periods (incomplete data, provisional figures, or outliers).

**Implementation:**
```
Exclude last 7 days (daily data):
=DROP(DailyData, -7)

Exclude last 4 quarters (quarterly data):
=DROP(QuarterlyData, -4)

Calculate averages excluding recent data:
=AVERAGE(DROP(VALUES, -10))
```

**Business Application:** Financial analysis, forecasting, trend analysis. Recent data often needs exclusion—it may be provisional, incomplete, or create edge effects in calculations.

**Technical Details:** Using negative row values means formulas work regardless of total row count. As new data is added, the formula automatically adjusts to continue excluding the specified number of recent entries.

---

### [[Multi-Source Data Consolidation]]

**Scenario:** Combining data from multiple sheets where each has headers. You need to keep headers from the first source but drop headers from subsequent sources before stacking.

**Implementation:**
```
=VSTACK(
  Sheet1Data,
  DROP(Sheet2Data, 1),
  DROP(Sheet3Data, 1),
  DROP(Sheet4Data, 1)
)
```

**Business Application:** Multi-department consolidation, monthly report aggregation, data warehouse loading. When combining datasets, duplicate headers must be removed.

**Technical Details:** The first dataset keeps its header. All subsequent datasets have their headers dropped before VSTACK combines them. If datasets might have different column orders, use CHOOSECOLS to normalize before stacking.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill into adjacent cells
- **Performance:** Efficient even with large arrays
- **Error behavior:** #CALC! when dropping more than exists
- **Workaround for older versions:** Use INDEX with calculated row/column ranges

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand (spill)
- **Performance:** Efficient for most use cases
- **Error behavior:** Same as Excel—error when over-dropping
- **Alternative:** OFFSET with ROWS/COLUMNS calculations (less elegant)

### Both Platforms

- Positive numbers drop from top/left
- Negative numbers drop from bottom/right
- Zero means "keep all" in that dimension
- Works with any array including formula results
- Returns empty result if all rows/columns dropped (not an error, but may cause downstream issues)

## Tips and Best Practices

1. **Remember the sign convention:** Positive = drop from start (top/left), Negative = drop from end (bottom/right). Think of it as moving the border inward from that edge.

2. **Use 0 to skip a dimension:** Need to drop columns but keep all rows? Use `DROP(Data, 0, 2)`. The 0 explicitly means "don't drop anything here."

3. **Combine DROP and TAKE for middle sections:** `DROP(TAKE(Data, 50), 10)` extracts rows 11-50. `TAKE(DROP(Data, 10), 40)` achieves the same result differently.

4. **Check array dimensions before dropping:** If you might drop more than exists, use MIN: `DROP(Data, MIN(5, ROWS(Data)-1))` ensures you never drop more than possible.

5. **Prefer DROP over complex INDEX formulas:** `DROP(A1:D100, 1)` is cleaner than `INDEX(A1:D100, 2, 0):INDEX(A1:D100, ROWS(A1:D100), COLUMNS(A1:D100))`.

6. **Nest DROP for multiple edges:** `DROP(DROP(Data, 2), -2)` removes from both ends. While verbose, it's clearer than calculating offsets manually.

7. **Use in data pipelines:** DROP at the start of a formula chain removes unwanted rows/columns before other functions process the clean data.

8. **Store drop counts in cells:** Reference cells for drop counts makes formulas flexible: `DROP(Data, DropCount)` where DropCount is a cell. Users can adjust without editing formulas.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TAKE]] | Returns rows/columns from start or end | When you want to KEEP a certain number rather than REMOVE. DROP and TAKE are inverses. |
| [[CHOOSEROWS]] | Extracts specific rows by number | When you need non-contiguous rows or specific row positions, not just edges. |
| [[CHOOSECOLS]] | Extracts specific columns by number | When you need non-contiguous columns or specific column positions, not just edges. |
| [[INDEX]] | Returns value or range subset | When DROP isn't available (older Excel) or for single-value extraction. |
| [[OFFSET]] | Returns a range offset from a starting point | Legacy alternative for dynamic ranges, but less intuitive than DROP. |

### Commonly Used Together

**[[TAKE]]** - Return rows/columns from start or end

*Combined with DROP for middle sections:*
```
=DROP(TAKE(Data, 100), 20)
```
TAKE gets first 100 rows, DROP removes first 20, leaving rows 21-100.

---

**[[VSTACK]]** - Combine arrays vertically

*Combined with DROP for consolidation without duplicate headers:*
```
=VSTACK(Sheet1Data, DROP(Sheet2Data, 1), DROP(Sheet3Data, 1))
```
Keep first dataset intact, drop headers from others before stacking.

---

**[[FILTER]]** - Filter rows based on criteria

*Combined with DROP to process filtered results:*
```
=DROP(FILTER(Data, Criteria), 0, 2)
```
Filter rows first, then drop leading columns from the result.

---

**[[SORT]]** - Sort array by columns

*Combined with DROP (in correct order):*
```
=SORT(DROP(Data, 1), 2, 1)
```
DROP header first, THEN sort. Sorting after dropping prevents header from being sorted into the data.

---

**[[HSTACK]]** - Combine arrays horizontally

*Combined with DROP for selective horizontal joins:*
```
=HSTACK(Table1, DROP(Table2, 0, 1))
```
Stack tables side by side, dropping the first column from Table2 (maybe a duplicate key column).

---

**[[ROWS]]/[[COLUMNS]]** - Count dimensions

*Combined with DROP for dynamic calculations:*
```
=DROP(Data, 0, COLUMNS(Data)-3)
```
Keep only the last 3 columns by calculating how many to drop.

## Official Documentation

- **Microsoft Excel:** [DROP function](https://support.microsoft.com/en-us/office/drop-function-1cb4e151-9e17-4838-abe5-9ba48d8c6a34)
- **Google Sheets:** [DROP function](https://support.google.com/docs/answer/13190081)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added to match Excel's dynamic array capabilities |

---

*Last updated: 2026-01-10*
