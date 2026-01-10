---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- dynamic-arrays
- number-generation
- sequences
- array-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Generate Sequence
- Number Sequence
- Row Numbers
- Array Sequence
tags:
- dynamic-arrays
- sequences
- row-numbers
- array-formulas
- automation
- data-generation
---

# SEQUENCE

## Description

**SEQUENCE** generates an array of sequential numbers in a specified number of rows and columns. Unlike traditional formulas that return a single value, SEQUENCE returns multiple values that automatically "spill" into adjacent cells. You define the dimensions (rows and columns), the starting number, and the step increment between values—SEQUENCE handles the rest. It's one of the most versatile functions introduced with Excel's dynamic array engine.

At its simplest, `=SEQUENCE(10)` creates a column of numbers 1 through 10. Add parameters to control the shape and values: `=SEQUENCE(5, 3)` creates a 5-row by 3-column grid; `=SEQUENCE(10, 1, 0, 0.5)` creates 10 values starting at 0, incrementing by 0.5. The function eliminates the tedious process of dragging formulas down columns or manually entering sequential values, and because it's a single formula, edits are instantaneous across the entire sequence.

**Where SEQUENCE shines:** Creating row numbers for data tables, generating date sequences, building lookup arrays, creating test data, indexing into other arrays, and anywhere you need evenly-spaced numeric values. Combined with other functions like INDEX, SORTBY, FILTER, or TEXT, SEQUENCE becomes a building block for powerful dynamic solutions. `=SEQUENCE(12, 1, DATE(2024,1,1), 30)` generates 12 monthly dates; `=SEQUENCE(5)^2` produces squared values {1, 4, 9, 16, 25}.

**Important constraint:** SEQUENCE requires Excel 365, Excel 2021, or Google Sheets. Earlier Excel versions (2019 and before) don't support dynamic arrays and will show #NAME? errors. If sharing workbooks with legacy Excel users, you'll need traditional formulas with dragged references instead.

## Syntax

> [!f(x)] SEQUENCE Syntax
>
> ```
> =SEQUENCE(rows, [columns], [start], [step])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rows` | Yes | The number of rows to return. Must be a positive integer. If you want a single row with multiple columns, set rows=1 and specify columns. |
| `columns` | No | The number of columns to return. Defaults to 1 if omitted. Must be a positive integer. Total cells = rows × columns. |
| `start` | No | The first number in the sequence. Defaults to 1 if omitted. Can be any number including negatives, decimals, and dates. |
| `step` | No | The increment between consecutive values. Defaults to 1 if omitted. Can be positive, negative, or decimal. Use 0 to repeat the start value. |

### Return Value

Returns a dynamic array of numeric values with the specified dimensions. The array automatically spills into adjacent cells. If rows or columns is less than 1, returns #VALUE! error. If the spill range is blocked by existing data, returns #SPILL! error. Values flow left-to-right across columns, then top-to-bottom across rows.

## Examples

> [!f(x)] SEQUENCE Examples

### Example 1: Simple Column of 10 Numbers
```
=SEQUENCE(10)
```
**Result:** Column of {1; 2; 3; 4; 5; 6; 7; 8; 9; 10}

**Explanation:** The most basic usage—generates integers 1 through 10 in a single column. All optional parameters default: columns=1, start=1, step=1.

---

### Example 2: Row of Numbers
```
=SEQUENCE(1, 5)
```
**Result:** Row of {1, 2, 3, 4, 5}

**Explanation:** By setting rows=1 and columns=5, the sequence spreads horizontally instead of vertically. Useful for creating horizontal headers or lookup arrays.

---

### Example 3: Grid of Numbers (5 Rows x 3 Columns)
```
=SEQUENCE(5, 3)
```
**Result:**
```
 1   2   3
 4   5   6
 7   8   9
10  11  12
13  14  15
```

**Explanation:** Creates a 5x3 grid. Numbers fill left-to-right, then continue on the next row. Total values = 5 × 3 = 15.

---

### Example 4: Starting from Zero
```
=SEQUENCE(5, 1, 0)
```
**Result:** Column of {0; 1; 2; 3; 4}

**Explanation:** Start parameter changes the first value. Useful for zero-based indexing or when your sequence must begin at a specific number.

---

### Example 5: Custom Step (Counting by 5s)
```
=SEQUENCE(6, 1, 5, 5)
```
**Result:** Column of {5; 10; 15; 20; 25; 30}

**Explanation:** Starts at 5, increments by 5. Perfect for multiplication tables, interval-based sequences, or skip counting.

---

### Example 6: Negative Step (Countdown)
```
=SEQUENCE(10, 1, 10, -1)
```
**Result:** Column of {10; 9; 8; 7; 6; 5; 4; 3; 2; 1}

**Explanation:** Negative step creates a descending sequence. Great for countdowns, reverse rankings, or descending sort helpers.

---

### Example 7: Decimal Sequence
```
=SEQUENCE(5, 1, 0, 0.25)
```
**Result:** Column of {0; 0.25; 0.5; 0.75; 1}

**Explanation:** Both start and step can be decimals. This creates quarter-step intervals between 0 and 1.

---

### Example 8: Generating Dates (Monthly)
```
=SEQUENCE(12, 1, DATE(2024,1,1), 30)
```
**Result:** Column of dates approximately monthly (Jan 1, Jan 31, Mar 1, etc.)

**Explanation:** Since dates are numbers in Excel/Sheets, SEQUENCE can generate date sequences. Step of 30 approximates monthly intervals (not exact months—use EDATE for true months).

---

### Example 9: True Monthly Dates with EDATE
```
=EDATE(DATE(2024,1,1), SEQUENCE(12, 1, 0, 1))
```
**Result:** Column of {Jan 1, Feb 1, Mar 1, ... Dec 1, 2024}

**Explanation:** SEQUENCE generates month offsets 0-11, EDATE adds those months to the start date. This correctly handles varying month lengths.

---

### Example 10: Row Numbers for Data
```
=SEQUENCE(ROWS(A2:A100))
```
**Result:** Column of row numbers 1 through 99

**Explanation:** ROWS counts the data range, SEQUENCE generates matching row numbers. This dynamically adjusts if the data range changes.

---

### Example 11: Repeating the Same Value
```
=SEQUENCE(5, 3, 100, 0)
```
**Result:** 5x3 grid filled with 100

**Explanation:** When step=0, every value equals the start value. This creates a constant array, useful for certain array calculations.

---

### Example 12: Times for a Schedule
```
=TEXT(SEQUENCE(9, 1, TIME(8,0,0), TIME(1,0,0)), "h:mm AM/PM")
```
**Result:** {"8:00 AM"; "9:00 AM"; "10:00 AM"; ... "4:00 PM"}

**Explanation:** Generates hourly time slots starting at 8 AM. TIME(1,0,0) represents a 1-hour increment. TEXT formats the results.

---

### Example 13: Squared Numbers
```
=SEQUENCE(10)^2
```
**Result:** Column of {1; 4; 9; 16; 25; 36; 49; 64; 81; 100}

**Explanation:** SEQUENCE results can be used in calculations. Here each value is squared. This works because SEQUENCE returns an array that participates in array arithmetic.

---

### Example 14: Multiplication Table
```
=SEQUENCE(10, 1, 1, 1) * SEQUENCE(1, 10, 1, 1)
```
**Result:** 10x10 multiplication table

**Explanation:** A column (10,1) multiplied by a row (1,10) produces a 10x10 grid through array broadcasting. Each cell contains row × column.

---

### Example 15: Random Sample Indices
```
=SORTBY(SEQUENCE(100), RANDARRAY(100))
```
**Result:** Numbers 1-100 in random order

**Explanation:** Creates indices 1-100, sorts by random values. Useful for random sampling, shuffling data, or creating randomized test sequences.

---

### Example 16: Alphabet Letters
```
=CHAR(SEQUENCE(26, 1, 65, 1))
```
**Result:** Column of {"A"; "B"; "C"; ... "Z"}

**Explanation:** SEQUENCE generates ASCII codes 65-90 (uppercase A-Z), CHAR converts to letters. Demonstrates SEQUENCE's role as a building block.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NAME?` | Excel version doesn't support SEQUENCE (pre-365/2021) | Upgrade to Excel 365, Excel 2021, or use Google Sheets. No workaround exists in older Excel. |
| `#SPILL!` | Adjacent cells contain data blocking the array | Clear the cells where SEQUENCE needs to spill, or move the formula. |
| `#VALUE!` | Rows or columns is less than 1, or non-numeric | Ensure rows and columns are positive integers. |
| `#CALC!` | Array would be too large | Reduce rows × columns. Excel has limits on spill range size. |
| `Dates show as numbers` | Excel treating date sequence as plain numbers | Format cells as dates, or wrap in TEXT(). |
| `Unexpected grid shape` | Rows/columns swapped | Remember: first parameter is rows (vertical), second is columns (horizontal). |

## Use Cases

### [[Dynamic Row Numbering in Data Tables]]

**Scenario:** Create automatically updating row numbers for filtered or sorted data.

**Implementation:**
```
=SEQUENCE(ROWS(DataTable))
=SEQUENCE(COUNTA(A2:A)-COUNTBLANK(A2:A))
```

**Business Application:** Reports, dashboards, and data tables need row numbers that update automatically as data is added, removed, or filtered. SEQUENCE eliminates manual numbering.

**Technical Details:** Combine with FILTER to number only visible rows: `=SEQUENCE(ROWS(FILTER(Data, Criteria)))`. The sequence adjusts automatically when filter criteria change.

---

### [[Date Series Generation]]

**Scenario:** Generate lists of dates for scheduling, reporting periods, or time-based analysis.

**Implementation:**
```
=SEQUENCE(30, 1, TODAY(), 1)                    ' Next 30 days
=EDATE(StartDate, SEQUENCE(12, 1, 0, 1))        ' 12 months
=StartDate + SEQUENCE(52, 1, 0, 7)               ' 52 weeks
```

**Business Application:** Financial reporting periods, project schedules, appointment slots, and any application requiring regular date intervals.

**Technical Details:** For true calendar months, use EDATE with SEQUENCE providing month offsets. For fixed-day intervals, add SEQUENCE directly to a start date.

---

### [[Array Index Generation for INDIRECT/INDEX]]

**Scenario:** Create dynamic references to multiple rows or columns for lookup and aggregation.

**Implementation:**
```
=INDEX(DataRange, SEQUENCE(5), 1)               ' First 5 rows of column 1
=SUMPRODUCT(INDEX(Sales, SEQUENCE(12), Month))  ' Sum specific months
=INDIRECT("Sheet"&SEQUENCE(5)&"!A1")            ' Reference multiple sheets
```

**Business Application:** Creating summary tables that pull from multiple locations, building dynamic reports that adjust to data size, and cross-sheet aggregation.

**Technical Details:** SEQUENCE provides the row/column numbers that INDEX or INDIRECT use for reference. This pattern is powerful for consolidating data from multiple sources.

---

### [[Test Data and Sample Generation]]

**Scenario:** Generate sample data for testing formulas, demos, or prototyping.

**Implementation:**
```
=SEQUENCE(100, 1, 1000, 1)                       ' Sample IDs
=RANDBETWEEN(1, 100) * SEQUENCE(50)/SEQUENCE(50) ' Random values (trick for array)
=CHAR(RANDBETWEEN(65, 90) + SEQUENCE(10)*0)     ' Random letters (per row)
```

**Business Application:** Developers and analysts creating test scenarios, building demo spreadsheets, or generating synthetic data for privacy when real data can't be used.

**Technical Details:** SEQUENCE provides the framework; combine with RANDBETWEEN, CHAR, TEXT, and other functions to create diverse test data types.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365 (all versions), Excel 2021, Excel for web
- **Not available:** Excel 2019 and earlier (shows #NAME?)
- **Spill behavior:** Results automatically spill into adjacent cells
- **Max size:** Limited by available memory and worksheet size
- **Array arithmetic:** Full support for operations on SEQUENCE results

### Google Sheets
- **Availability:** All versions since launch
- **Syntax:** Identical to Excel
- **Spill behavior:** Automatic spilling, same as Excel 365
- **Performance:** Handles large sequences efficiently
- **Integration:** Works seamlessly with ARRAYFORMULA (though not required)

### Key Differences
- Excel 2019 and earlier completely lack this function
- Google Sheets has supported SEQUENCE longer than Excel
- Both platforms handle spilling identically
- Array calculation limits may differ between platforms
- LibreOffice Calc supports SEQUENCE in recent versions

## Tips and Best Practices

1. **Use SEQUENCE instead of dragging formulas:** A single `=SEQUENCE(1000)` replaces 1000 individual cells with ROW()-based formulas. It's faster, cleaner, and easier to maintain.

2. **Combine with ROWS() for dynamic sizing:** `=SEQUENCE(ROWS(DataRange))` automatically adjusts as your data grows or shrinks.

3. **Remember the spill concept:** SEQUENCE needs empty cells to spill into. If you get #SPILL!, check for data blocking the output range.

4. **Use for date generation with care:** Direct date sequences (step=1, 7, 30) work for day-based intervals. For true monthly/yearly intervals, combine with EDATE or EOMONTH.

5. **Build multiplication tables and grids:** `=SEQUENCE(n,1)*SEQUENCE(1,m)` creates an n×m multiplication table through array broadcasting.

6. **Create zero-based indices when needed:** Many programming contexts use zero-based indexing. Use `=SEQUENCE(n, 1, 0)` for 0 to n-1.

7. **Generate negative sequences:** Use negative step values for countdowns or descending sequences. `=SEQUENCE(10, 1, 100, -10)` gives {100, 90, 80, ...}.

8. **Combine with TEXT for formatted sequences:** `=TEXT(SEQUENCE(12, 1, DATE(2024,1,1), 30), "MMMM YYYY")` creates formatted month names.

9. **Use step=0 for constant arrays:** When you need an array of identical values, `=SEQUENCE(rows, cols, value, 0)` is more elegant than many alternatives.

10. **Test with small sequences first:** When building complex formulas, start with `=SEQUENCE(5)` to verify logic before scaling up.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[RANDARRAY]] | Generates array of random numbers | When you need random rather than sequential values |
| [[ROW]] | Returns the row number of a reference | For simple row numbering within a range (works in all Excel versions) |
| [[COLUMN]] | Returns the column number of a reference | For column numbering (works in all Excel versions) |
| [[MAKEARRAY]] | Creates array from lambda function | For complex, non-linear sequences |

### Commonly Used Together

**[[INDEX]]** - Retrieve specific values from a range

*Access multiple rows with SEQUENCE:*
```
=INDEX(DataRange, SEQUENCE(5), 1)
```
Returns the first 5 rows of column 1 from DataRange.

---

**[[FILTER]]** - Filter data based on criteria

*Number filtered results:*
```
=SEQUENCE(ROWS(FILTER(Data, Criteria)))
```
Creates row numbers that match the filtered output size.

---

**[[SORTBY]]** - Sort array by another array

*Shuffle a sequence randomly:*
```
=SORTBY(SEQUENCE(n), RANDARRAY(n))
```
Creates a random permutation of numbers 1 to n.

---

**[[EDATE]]** - Add months to a date

*Generate monthly date sequence:*
```
=EDATE(StartDate, SEQUENCE(12, 1, 0, 1))
```
Properly handles varying month lengths.

---

**[[TEXT]]** - Format numbers as text

*Format date sequence:*
```
=TEXT(SEQUENCE(7, 1, TODAY(), 1), "dddd")
```
Generates day names for the next week.

---

**[[CHAR]]** - Convert number to character

*Generate alphabet:*
```
=CHAR(SEQUENCE(26, 1, 65, 1))
```
Creates array of uppercase letters A-Z.

## Official Documentation

- **Microsoft Excel:** [SEQUENCE function](https://support.microsoft.com/en-us/office/sequence-function-57467a98-57e0-4817-9f14-2eb78519ca90)
- **Google Sheets:** [SEQUENCE function](https://support.google.com/docs/answer/9368244)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 365 (2018) | Introduced with dynamic arrays |
| Excel | Excel 2021 | Included in perpetual license version |
| Excel Online | Launch with dynamic arrays | Full support |
| Google Sheets | Original launch | Available from the beginning |
| LibreOffice Calc | 7.4+ | Added for compatibility |

---

*Last updated: 2026-01-10*
