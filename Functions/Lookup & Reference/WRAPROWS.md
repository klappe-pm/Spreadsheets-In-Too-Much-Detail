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
- array-reshaping
- dynamic-arrays
- data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Wrap Rows
- Row Wrap
- Reshape to Rows
- List to Grid
tags:
- array
- wrap
- reshape
- dynamic-arrays
- transformation
---

# WRAPROWS

## Description

**WRAPROWS** reshapes a single row or column of data into a two-dimensional array by wrapping values into rows of a specified width. Think of it as taking a long list and filling it into a grid row by row—values fill across the first row, then continue at the start of the second row, and so on. If you have 12 months of data in a single column, WRAPROWS can transform it into a 3-row by 4-column grid (filling each row with 4 values before moving to the next row).

The function reads values from the source vector (row or column) sequentially and places them into the result array row by row, from left to right within each row, then top to bottom across rows. The wrap_count parameter determines how many values go in each row before wrapping to the next. This is the natural "reading order" flow—like reading text from a book—and is the opposite of WRAPCOLS, which fills column by column instead.

**Key behaviors to understand:** If the source doesn't divide evenly by wrap_count, the last row will have fewer values, and those empty positions are filled with pad_with (default #N/A). For example, wrapping 10 values with wrap_count=3 creates a 4x3 array: three full rows of 3 values, plus a fourth row with 1 value and 2 #N/A padding cells. WRAPROWS only accepts one-dimensional input (either a single row or single column)—not two-dimensional arrays.

**Platform availability:** WRAPROWS is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier versions. Both platforms implement the function identically. Before these functions existed, reshaping data required complex INDEX/MOD/INT formulas—WRAPROWS makes this transformation trivially simple.

## Syntax

> [!f(x)] WRAPROWS Syntax
>
> ```
> =WRAPROWS(vector, wrap_count, [pad_with])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `vector` | Yes | The source row or column of values to wrap. Must be one-dimensional (either a single row or single column, not a 2D array). |
| `wrap_count` | Yes | The number of values per row in the result. Each row will contain up to this many values before wrapping to the next row. Must be a positive integer. |
| `pad_with` | No | The value to use for padding when the source doesn't divide evenly. Defaults to #N/A if omitted. Can be any value: number, text, blank (""), etc. |

### Return Value

Returns a two-dimensional array with wrap_count columns and enough rows to contain all source values. Values are filled row by row (across then down). If the source count isn't a multiple of wrap_count, the last row is padded with pad_with values.

## Examples

> [!f(x)] WRAPROWS Examples

### Example 1: Basic Row Wrapping
```
=WRAPROWS(A1:A12, 4)
```
**Result:** Returns a 3-row by 4-column array

**Explanation:** A list of 12 values becomes a 3x4 grid. Values fill across: A1, A2, A3, A4 in row 1, then A5, A6, A7, A8 in row 2, etc. Each row has exactly 4 values because 12 divides evenly by 4.

---

### Example 2: Wrapping with Padding
```
=WRAPROWS(A1:A10, 4)
```
**Result:** Returns a 3-row by 4-column array; last row has 2 values and 2 #N/A

**Explanation:** 10 values with wrap_count=4 means 3 rows needed (ceiling of 10/4). First two rows have 4 values each (8 total), the third row has 2 values and 2 #N/A padding cells.

---

### Example 3: Custom Pad Value
```
=WRAPROWS(A1:A10, 4, 0)
```
**Result:** Same shape as above, but padding is 0 instead of #N/A

**Explanation:** Specifying pad_with=0 fills incomplete positions with zeros. Useful when the result will be used in calculations where #N/A would cause errors like in SUM or AVERAGE.

---

### Example 4: Pad with Empty Strings
```
=WRAPROWS(A1:A10, 4, "")
```
**Result:** Padding cells appear blank

**Explanation:** Empty string padding looks cleaner for display purposes. The cells contain "" but appear blank, making the display more professional.

---

### Example 5: Wrap a Row into Rows
```
=WRAPROWS(A1:L1, 4)
```
**Result:** A 3-row by 4-column array from a horizontal source

**Explanation:** WRAPROWS works with either rows or columns as input. This horizontal row of 12 values becomes a 3x4 grid using the same row-first fill logic.

---

### Example 6: Create Sequential Grid
```
=WRAPROWS(SEQUENCE(12), 4)
```
**Result:** Returns {1,2,3,4; 5,6,7,8; 9,10,11,12}

**Explanation:** Numbers 1-12 arranged into a 3x4 grid. Values 1,2,3,4 fill the first row; 5,6,7,8 fill the second; etc. Reading across rows gives sequential values—the natural reading order.

---

### Example 7: Transform List to Table Format
```
=WRAPROWS(ContactFields, 3)
```
**Result:** Contact data arranged in records of 3 fields each

**Explanation:** If data arrives as Name, Phone, Email, Name, Phone, Email... in a single column, wrapping by 3 creates a table with Name, Phone, Email columns and one row per contact.

---

### Example 8: Create Matrix from Sequence
```
=WRAPROWS(SEQUENCE(16), 4)
```
**Result:** A 4x4 matrix numbered 1-16

**Explanation:** Perfect for creating numbered grids, game boards, or reference matrices. Values read naturally left-to-right, top-to-bottom.

---

### Example 9: Wrap Monthly Data into Quarters
```
=WRAPROWS(MonthlyData, 3)
```
**Result:** Each row contains one quarter (3 months)

**Explanation:** 12 months of data becomes 4 rows, each representing a quarter. Jan, Feb, Mar in row 1; Apr, May, Jun in row 2; etc. Easy quarterly comparisons.

---

### Example 10: Display Items in Grid Layout
```
=WRAPROWS(ProductList, 5, "-")
```
**Result:** Products displayed 5 per row, empty slots marked with "-"

**Explanation:** Long product list displayed in a grid with 5 items per row. If there are 23 products, you get 5 rows with 5, 5, 5, 5, and 3 items (plus 2 dashes).

---

### Example 11: Combine with FILTER
```
=WRAPROWS(FILTER(Items, Category="Electronics"), 4, "")
```
**Result:** Electronics items displayed 4 per row

**Explanation:** FILTER extracts electronics items as a column. WRAPROWS reshapes for grid display. Variable result size based on filter matches.

---

### Example 12: Data Import Parsing
```
=WRAPROWS(TEXTSPLIT(ImportedText, ","), FieldCount, "")
```
**Result:** Comma-separated import parsed into table rows

**Explanation:** TEXTSPLIT breaks the import into individual values. WRAPROWS arranges them into records with the expected number of fields per row.

---

### Example 13: Weekly Schedule Layout
```
=WRAPROWS(AllTimeSlots, 7)
```
**Result:** Time slots arranged with 7 per row (one week)

**Explanation:** Daily time slots for 4 weeks (28 slots) become a 4x7 grid. Each row is a week; each column is a day of the week.

---

### Example 14: Calculated Column Count
```
=LET(
  data, A1:A100,
  rows, 5,
  cols, CEILING(ROWS(data)/rows, 1),
  WRAPROWS(data, cols, "")
)
```
**Result:** Data distributed into exactly 5 rows

**Explanation:** To control the number of OUTPUT rows (rather than columns per row), calculate the required wrap_count. LET keeps the formula clean.

---

### Example 15: Add Row Headers to Wrapped Data
```
=HSTACK(
  {"Q1"; "Q2"; "Q3"; "Q4"},
  WRAPROWS(MonthlyData, 3)
)
```
**Result:** Quarterly labels next to quarterly data (3 months per row)

**Explanation:** Wrap monthly data into quarters (3 months each), then add quarter labels on the left with HSTACK.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Vector is 2D (multiple rows AND multiple columns) | WRAPROWS only accepts 1D input. Use FLATTEN first for 2D arrays, or select just one row/column. |
| `#VALUE!` | wrap_count is zero or negative | wrap_count must be a positive integer (1 or greater). |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula. |
| `#NAME?` | Function not available in your Excel version | WRAPROWS requires Excel 365/2021 or Google Sheets. |
| `#N/A in results` | Source count not divisible by wrap_count | This is expected—use pad_with parameter to specify different padding. |

## Use Cases

### [[List to Table Conversion]]

**Scenario:** Data arrives as a single stream (from import, API, or concatenated source) but represents structured records. Each N values form one record that should be a row.

**Implementation:**
```
Contact data (Name, Email, Phone per person):
=WRAPROWS(RawContactData, 3, "")

Product import (SKU, Name, Price, Stock per item):
=WRAPROWS(ImportData, 4, "N/A")

Add headers:
=VSTACK({"Name", "Email", "Phone"}, WRAPROWS(RawContactData, 3, ""))
```

**Business Application:** Data import processing, ETL transformations, API response parsing. Raw sequential data needs structure for analysis.

**Technical Details:** wrap_count must match the data's actual record size. If records have 5 fields but you wrap by 4, data will misalign. Validate record structure before wrapping.

---

### [[Grid and Matrix Displays]]

**Scenario:** Creating numbered grids, game boards, seating charts, or any spatial layout where sequential items need to be arranged in reading order.

**Implementation:**
```
10x10 numbered grid:
=WRAPROWS(SEQUENCE(100), 10)

Bingo card (5x5):
=WRAPROWS(SORTBY(SEQUENCE(25), RANDARRAY(25)), 5)

Seating chart with labels:
=WRAPROWS(SeatCodes, SeatsPerRow, "Aisle")
```

**Business Application:** Event planning, gaming, warehouse layouts. Visual grids help with spatial organization and reference.

**Technical Details:** For random arrangements (like bingo), sort the sequence by random values before wrapping. The wrap preserves whatever order the input has.

---

### [[Time-Period Layouts]]

**Scenario:** Arrange time-series data into meaningful groups—daily data into weeks, monthly data into quarters, hourly data into days.

**Implementation:**
```
Daily data into weeks (7 days per row):
=WRAPROWS(DailyValues, 7, "")

Monthly data into years (12 months per row):
=WRAPROWS(MonthlyRevenue, 12, 0)

With period labels:
=HSTACK({"Week 1";"Week 2";"Week 3";"Week 4"}, WRAPROWS(DailyData, 7, ""))
```

**Business Application:** Financial reporting, operations analysis, trend visualization. Period-based layouts facilitate comparison and pattern recognition.

**Technical Details:** Time-period wrap counts: 7 (weekly), 12 (yearly from monthly), 4 (quarterly from annual quarters), 24 (daily from hourly). Align data start with period start.

---

### [[Multi-Column Report Layout]]

**Scenario:** Reports need to display lists in multiple columns to save space and improve readability—like newspaper columns or catalog layouts.

**Implementation:**
```
3-column product listing:
=WRAPROWS(ProductNames, CEILING(COUNT(ProductNames)/3, 1), "")

Names in 4 columns, 20 rows max (80 names):
=WRAPROWS(TAKE(Names, MIN(80, ROWS(Names))), 4, "")
```

**Business Application:** Printed reports, catalogs, directories. Space-efficient layouts fit more information per page.

**Technical Details:** To achieve N columns with variable-length data, calculate wrap_count as `CEILING(total/N, 1)`. This may result in fewer rows if total is small.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill
- **Input requirement:** Must be 1D (single row or column)
- **Default padding:** #N/A

### Google Sheets

- **Availability:** All current versions
- **Dynamic arrays:** Results automatically expand
- **Input requirement:** Same as Excel—must be 1D
- **Default padding:** #N/A (same as Excel)
- **Syntax:** Identical to Excel

### Both Platforms

- Fills row by row (across, then down)
- wrap_count determines columns in output
- Incomplete final row is padded
- Works with formula results (FILTER, SORT, SEQUENCE, etc.)
- Natural reading order (left-to-right, top-to-bottom)

## Tips and Best Practices

1. **Remember the fill direction:** WRAPROWS fills ACROSS each row before moving DOWN. Values read horizontally in sequence—like reading a book. For column-first filling, use WRAPCOLS.

2. **Use pad_with for clean output:** Default #N/A padding can be distracting or break calculations. Use "" for blank appearance, 0 for numeric contexts, or descriptive text.

3. **Calculate wrap_count for target rows:** To get N rows, use `CEILING(COUNT(data)/N, 1)` as wrap_count. This calculates how many columns fit in N rows.

4. **Only 1D input allowed:** WRAPROWS rejects 2D arrays. Use FLATTEN first to convert 2D to 1D, then WRAPROWS to reshape: `=WRAPROWS(FLATTEN(2DArray), columns)`.

5. **Combine with VSTACK/HSTACK for headers:** After wrapping, use VSTACK to add column headers or HSTACK to add row labels.

6. **Use SEQUENCE to understand the pattern:** `=WRAPROWS(SEQUENCE(12), 4)` clearly shows {1,2,3,4; 5,6,7,8; 9,10,11,12}—reading order.

7. **Match wrap_count to record structure:** When converting streamed data to records, wrap_count MUST match the number of fields per record or data will misalign.

8. **WRAPROWS is reading order, WRAPCOLS is column order:** If you're unsure which to use, WRAPROWS is usually more intuitive because it matches how we read.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WRAPCOLS]] | Wraps vector into columns (fills column by column) | When you want vertical-first filling (down then right) |
| [[TOCOL]] | Converts array to single column | When going the opposite direction—2D to 1D |
| [[TOROW]] | Converts array to single row | When converting 2D to 1D horizontally |
| [[FLATTEN]] | Flattens array to single column | Similar to TOCOL; use before WRAPROWS to reshape any array |

### Commonly Used Together

**[[SEQUENCE]]** - Generate sequential numbers

*Combined with WRAPROWS for numbered grids:*
```
=WRAPROWS(SEQUENCE(20), 5)
```
Creates a 4x5 grid numbered 1-20 (reading across rows).

---

**[[VSTACK]]** - Add headers to wrapped result

*Combined with WRAPROWS for labeled tables:*
```
=VSTACK({"Col1", "Col2", "Col3", "Col4"}, WRAPROWS(Data, 4, ""))
```
Add column headers above the wrapped data.

---

**[[HSTACK]]** - Add row labels to wrapped result

*Combined with WRAPROWS for row-labeled display:*
```
=HSTACK({"Row1"; "Row2"; "Row3"}, WRAPROWS(Data, 4, ""))
```
Add row labels to the left of wrapped data.

---

**[[FLATTEN]]** - Convert 2D to 1D

*Combined with WRAPROWS for complete restructuring:*
```
=WRAPROWS(FLATTEN(2DArray), NewColumnCount)
```
Flatten any array, then wrap to new dimensions.

---

**[[FILTER]]** - Filter before wrapping

*Combined with WRAPROWS for multi-column filtered lists:*
```
=WRAPROWS(FILTER(List, Criteria), 5, "")
```
Display filtered results in rows of 5.

---

**[[TEXTSPLIT]]** - Parse text before wrapping

*Combined with WRAPROWS for import processing:*
```
=WRAPROWS(TEXTSPLIT(CSVData, ","), FieldsPerRecord)
```
Split imported text, then wrap into records.

## Official Documentation

- **Microsoft Excel:** [WRAPROWS function](https://support.microsoft.com/en-us/office/wraprows-function-796825f3-975a-4f38-9a51-be37ad9e286a)
- **Google Sheets:** [WRAPROWS function](https://support.google.com/docs/answer/13190700)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2022) | Introduced as part of dynamic array function expansion |
| Excel | Excel 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added to match Excel's dynamic array capabilities |

---

*Last updated: 2026-01-10*
