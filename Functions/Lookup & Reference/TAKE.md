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
- row-selection
- column-selection
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Take Rows
- Take Columns
- Get First N
- Get Last N
tags:
- array
- take
- extraction
- dynamic-arrays
- data-manipulation
---

# TAKE

## Description

**TAKE** returns a specified number of contiguous rows and/or columns from the start or end of an array. It's the "give me the first N" or "give me the last N" function—perfect for extracting top performers, recent records, or a subset from either end of your data. If DROP is about removing edges, TAKE is about keeping them. Together, they form the complete toolkit for array slicing.

The brilliance of TAKE lies in its intuitive sign convention: positive numbers take from the BEGINNING (first rows from top, first columns from left), while negative numbers take from the END (last rows from bottom, last columns from right). So `TAKE(Data, 5)` gets the first 5 rows, while `TAKE(Data, -5)` gets the last 5 rows. You can specify both row and column counts: `TAKE(Data, 3, 2)` gets the first 3 rows and first 2 columns—a top-left corner extraction.

**Key behaviors to understand:** TAKE returns contiguous blocks—you can't skip rows or columns in the middle (use CHOOSEROWS/CHOOSECOLS for that). If you request more rows or columns than exist, you'll get a #CALC! error. Taking from one dimension while keeping all of another is done by omitting that parameter or using a count equal to the full dimension. TAKE pairs naturally with SORT for "top N" queries and with DROP for "middle section" extractions.

**Platform availability:** TAKE is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically. For older Excel versions, INDEX with calculated ranges or complex OFFSET formulas were the workarounds—significantly more cumbersome than TAKE.

## Syntax

> [!f(x)] TAKE Syntax
>
> ```
> =TAKE(array, rows, [columns])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The source array or range from which to take rows and/or columns. Can be a cell range, named range, or array returned by another formula. |
| `rows` | Yes | The number of rows to return. Positive numbers take from the top; negative numbers take from the bottom. Cannot be zero. |
| `columns` | No | The number of columns to return. Positive numbers take from the left; negative numbers take from the right. If omitted, all columns are returned. Cannot be zero. |

### Return Value

Returns an array containing the specified contiguous rows and/or columns from the source array. The result has abs(rows) rows and abs(columns) columns (or all columns if columns is omitted). Returns #CALC! if you request more rows or columns than exist in the source array.

## Examples

> [!f(x)] TAKE Examples

### Example 1: Take First N Rows
```
=TAKE(A1:D100, 10)
```
**Result:** Returns the first 10 rows (rows 1-10) with all 4 columns

**Explanation:** The most common TAKE pattern—getting the first N rows from a dataset. All columns are included since the columns parameter is omitted. This is perfect for previewing data or limiting output size.

---

### Example 2: Take Last N Rows
```
=TAKE(A1:D100, -10)
```
**Result:** Returns the last 10 rows (rows 91-100) with all 4 columns

**Explanation:** Negative row count takes from the bottom. Ideal for getting recent entries from chronologically ordered data, or the "bottom N" after sorting ascending.

---

### Example 3: Take First N Columns
```
=TAKE(A1:J100, 100, 3)
```
**Result:** Returns all 100 rows but only the first 3 columns (A, B, C)

**Explanation:** Keep all rows (specifying the full row count) but limit columns. This extracts a vertical slice from the left side of the data.

---

### Example 4: Take Last N Columns
```
=TAKE(A1:J100, 100, -3)
```
**Result:** Returns all 100 rows but only the last 3 columns (H, I, J)

**Explanation:** Negative column count takes from the right. Useful when the rightmost columns contain the most relevant data (like recent months in a time series).

---

### Example 5: Take a Corner Block (First Rows and Columns)
```
=TAKE(A1:J100, 5, 3)
```
**Result:** Returns the top-left 5x3 block (rows 1-5, columns A-C)

**Explanation:** Specify both row and column counts to extract a rectangular corner. This gets the first 5 rows and first 3 columns—a common pattern for data previews.

---

### Example 6: Take Bottom-Right Corner
```
=TAKE(A1:J100, -5, -3)
```
**Result:** Returns the bottom-right 5x3 block (rows 96-100, columns H-J)

**Explanation:** Negative values for both parameters extracts from the bottom-right corner. Useful for getting recent data from the end of a time-based table where newest is at the bottom-right.

---

### Example 7: Top N After Sorting (Top 5 Sales)
```
=TAKE(SORT(SalesData, 3, -1), 5)
```
**Result:** Returns the 5 rows with highest values in column 3

**Explanation:** SORT arranges data by column 3 descending (highest first), then TAKE gets the first 5 rows. This is the classic "Top N" query pattern. Combine with FILTER to get top N within a category.

---

### Example 8: Bottom N After Sorting (Worst 10 Performers)
```
=TAKE(SORT(PerformanceData, 4, 1), 10)
```
**Result:** Returns the 10 rows with lowest values in column 4

**Explanation:** SORT ascending puts lowest values first, then TAKE gets those first 10 rows. Alternatively, sort descending and use `TAKE(..., -10)` for the last 10.

---

### Example 9: Most Recent N Entries
```
=TAKE(Data, -20)
```
**Result:** Returns the last 20 rows (most recent if data is chronological)

**Explanation:** For data where newest entries are added at the bottom, taking from the end gives you the most recent records. No sorting needed if data is already chronologically ordered.

---

### Example 10: TAKE with FILTER for Top N in Category
```
=TAKE(SORT(FILTER(Data, Region="East"), Revenue, -1), 10)
```
**Result:** Top 10 revenue records in the East region

**Explanation:** FILTER extracts East region rows, SORT orders by revenue descending, TAKE gets the first 10. This layered approach is powerful for segmented analysis.

---

### Example 11: Extract Middle Section with TAKE and DROP
```
=DROP(TAKE(Data, 50), 20)
```
**Result:** Returns rows 21-50 (middle section)

**Explanation:** TAKE(Data, 50) gets rows 1-50. DROP(..., 20) removes the first 20, leaving rows 21-50. This TAKE-then-DROP pattern extracts any contiguous middle section.

---

### Example 12: Take Single Row (First Row)
```
=TAKE(A1:D100, 1)
```
**Result:** Returns just the first row as a 1x4 array

**Explanation:** Taking 1 row gives you just the first row. Useful for extracting headers or the top-ranked item. For the last row, use `TAKE(Data, -1)`.

---

### Example 13: Take Single Column (Last Column)
```
=TAKE(Data, ROWS(Data), -1)
```
**Result:** Returns all rows of the last column only

**Explanation:** Specify full row count (using ROWS) and -1 for columns to extract just the rightmost column. This is useful when the last column contains results or totals.

---

### Example 14: Limit Results from UNIQUE
```
=TAKE(UNIQUE(CategoryColumn), 5)
```
**Result:** Returns the first 5 unique values

**Explanation:** UNIQUE might return many values; TAKE limits to the first 5. Useful for dropdown lists or quick summaries. For random sample of unique values, combine with sorting by RANDARRAY.

---

### Example 15: Dynamic Top N with Variable Count
```
=TAKE(SORT(Data, 2, -1), TopNCount)
```
**Result:** Returns top N rows where N is specified in cell TopNCount

**Explanation:** Reference a cell for the row count, allowing users to change how many results appear without editing the formula. Combine with a spinner or dropdown for interactive top-N analysis.

---

### Example 16: Handle Empty Results
```
=IFERROR(TAKE(FILTER(Data, Criteria), 5), "No results")
```
**Result:** Returns top 5 matching rows, or "No results" if filter is empty

**Explanation:** If FILTER returns no rows, TAKE fails with #CALC!. Wrapping in IFERROR provides a graceful fallback. Alternatively, use FILTER's if_empty parameter.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#CALC!` | Requested more rows/columns than exist in the array | If array has 50 rows and you TAKE 60, error occurs. Ensure count doesn't exceed source dimensions. |
| `#CALC!` | Row or column count is zero | TAKE doesn't accept 0. To keep all rows or columns, omit the parameter or use the full count. |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula. |
| `#NAME?` | Function not available in your Excel version | TAKE requires Excel 365/2021 or Google Sheets. Not available in Excel 2019 or earlier. |
| `#VALUE!` | Non-numeric row or column arguments | Ensure counts are numbers or cells containing numbers. |
| `#REF!` | Source range is invalid | Verify that the array reference exists and is correctly formatted. |

## Use Cases

### [[Top N Analysis and Leaderboards]]

**Scenario:** Sales managers need leaderboards showing top 10 salespeople, top 5 products, or worst 10 performing regions—updated dynamically as data changes.

**Implementation:**
```
Top 10 Salespeople by Revenue:
=TAKE(SORT(SalesData, RevenueColumn, -1), 10)

Top 5 Products by Units Sold:
=TAKE(SORT(ProductData, UnitsColumn, -1), 5)

Bottom 10 Regions by Growth Rate:
=TAKE(SORT(RegionData, GrowthColumn, 1), 10)

Top N with Dynamic Count:
=TAKE(SORT(Data, SortCol, -1), TopNCell)
```

**Business Application:** Sales contests, performance dashboards, KPI monitoring. Leaderboards motivate teams and highlight both successes and areas needing attention.

**Technical Details:** Always SORT before TAKE for top/bottom N. Descending (-1) sort with positive TAKE for top N. Ascending (1) sort with positive TAKE for bottom N. Reference a cell for the count to make it user-adjustable.

---

### [[Data Preview and Sampling]]

**Scenario:** Large datasets need previewing before analysis. Analysts want to see the first N rows to understand structure, or a sample to spot-check data quality.

**Implementation:**
```
Preview First 10 Rows:
=TAKE(LargeDataset, 10)

Preview Last 10 (Most Recent):
=TAKE(LargeDataset, -10)

Preview with Headers:
=VSTACK(Headers, TAKE(DROP(Data, 1), 10))

Sample from Both Ends:
=VSTACK(TAKE(Data, 5), TAKE(Data, -5))
```

**Business Application:** Data exploration, import validation, report development. Understanding data before building complex analysis saves time and prevents errors.

**Technical Details:** For true random sampling, combine with SORTBY and RANDARRAY: `=TAKE(SORTBY(Data, RANDARRAY(ROWS(Data))), SampleSize)`. This shuffles rows randomly, then takes the first N.

---

### [[Limiting Report Output]]

**Scenario:** Reports should show at most N items to fit in designated spaces, such as dashboard tiles or email summaries.

**Implementation:**
```
Show at most 15 items:
=TAKE(FilteredData, MIN(15, ROWS(FilteredData)))

Compact table for email:
=VSTACK(Headers, TAKE(SORT(Data, DateCol, -1), 5))

Dashboard tile with top results:
=TAKE(SORT(FILTER(Data, Status="Open"), Priority, 1), 8)
```

**Business Application:** Executive summaries, email reports, dashboard widgets. Space-constrained outputs need bounded result sets.

**Technical Details:** Use MIN with ROWS to handle cases where fewer rows exist than the limit: `TAKE(Data, MIN(10, ROWS(Data)))` prevents errors when data has fewer than 10 rows.

---

### [[Time-Based Data Extraction]]

**Scenario:** Analysis focuses on recent periods—last 30 days of transactions, most recent quarter's data, or the latest N entries.

**Implementation:**
```
Last 30 Entries (chronologically ordered data):
=TAKE(TransactionLog, -30)

Most Recent Quarter (if data has 4 quarters per year, 90 days approx):
=TAKE(DailyData, -90)

Latest N with Date Validation:
=TAKE(FILTER(Data, DateColumn>=TODAY()-30), -10)
```

**Business Application:** Trend analysis, recent performance review, real-time monitoring. Focus on current/recent data for timely decision making.

**Technical Details:** For data not chronologically ordered, first SORT by date descending, then TAKE the first N: `=TAKE(SORT(Data, DateColumn, -1), 30)`.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill into adjacent cells
- **Error on over-take:** #CALC! when requesting more than exists
- **Zero not allowed:** Row and column counts must be non-zero

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand (spill)
- **Error behavior:** Same as Excel—error when over-taking
- **Zero not allowed:** Same restriction as Excel
- **Syntax:** Identical to Excel

### Both Platforms

- Positive numbers take from start (top/left)
- Negative numbers take from end (bottom/right)
- Cannot take zero rows or columns
- Works with any array including formula results
- Returns contiguous blocks only (use CHOOSEROWS/CHOOSECOLS for non-contiguous)

## Tips and Best Practices

1. **Remember the sign convention:** Positive = take from start (top/left), Negative = take from end (bottom/right). It's the opposite of removing from that edge.

2. **Combine with SORT for Top/Bottom N:** `=TAKE(SORT(Data, Col, -1), N)` is the standard "top N" pattern. Sort descending, take from top.

3. **Use MIN to prevent over-take errors:** `TAKE(Data, MIN(10, ROWS(Data)))` safely takes up to 10 rows, or all rows if fewer than 10 exist.

4. **Pair with DROP for middle sections:** `DROP(TAKE(Data, 50), 20)` extracts rows 21-50. This TAKE-then-DROP pattern is powerful for middle extraction.

5. **Omit columns to keep all columns:** `TAKE(Data, 5)` keeps all columns while limiting rows. Only specify columns when you need to limit that dimension.

6. **Use for pagination:** `DROP(TAKE(Data, Page*PageSize), (Page-1)*PageSize)` extracts page N of PageSize rows each.

7. **Reference cells for dynamic counts:** `TAKE(Data, CountCell)` allows users to change the extraction count without editing formulas.

8. **Layer with FILTER for conditional Top N:** `TAKE(SORT(FILTER(Data, Condition), Col, -1), N)` gets top N within a filtered subset.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DROP]] | Removes rows/columns from edges | When you want to EXCLUDE from edges rather than INCLUDE. DROP and TAKE are complements. |
| [[CHOOSEROWS]] | Select specific rows by number | When you need non-contiguous rows, not just first/last N |
| [[CHOOSECOLS]] | Select specific columns by number | When you need non-contiguous columns, not just first/last N |
| [[INDEX]] | Returns value or range subset | When TAKE isn't available (older Excel) or for specific cell extraction |

### Commonly Used Together

**[[SORT]]** - Sort array by columns

*Combined with TAKE for top/bottom N:*
```
=TAKE(SORT(Data, 3, -1), 10)
```
Sort descending by column 3, then take top 10 rows.

---

**[[FILTER]]** - Filter rows by criteria

*Combined with TAKE for top N within filtered set:*
```
=TAKE(SORT(FILTER(Data, Criteria), Col, -1), 5)
```
Filter first, sort, then take top 5.

---

**[[DROP]]** - Remove rows/columns from edges

*Combined with TAKE for middle section extraction:*
```
=DROP(TAKE(Data, 50), 20)
```
Take first 50, drop first 20, leaving rows 21-50.

---

**[[VSTACK]]** - Combine arrays vertically

*Combined with TAKE for header + limited data:*
```
=VSTACK(Headers, TAKE(Data, 10))
```
Stack headers with first 10 data rows.

---

**[[ROWS]]/[[COLUMNS]]** - Count dimensions

*Combined with TAKE for safe extraction:*
```
=TAKE(Data, MIN(10, ROWS(Data)))
```
Take up to 10 rows without error if fewer exist.

---

**[[EXPAND]]** - Pad arrays to dimensions

*Combined with TAKE for fixed-size output:*
```
=EXPAND(TAKE(Data, MIN(10, ROWS(Data))), 10, , "")
```
Take up to 10 rows, pad to exactly 10 if fewer.

## Official Documentation

- **Microsoft Excel:** [TAKE function](https://support.microsoft.com/en-us/office/take-function-25382ff1-5da1-4f78-ab43-f33bd2e4e003)
- **Google Sheets:** [TAKE function](https://support.google.com/docs/answer/13190080)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added to match Excel's dynamic array capabilities |

---

*Last updated: 2026-01-10*
