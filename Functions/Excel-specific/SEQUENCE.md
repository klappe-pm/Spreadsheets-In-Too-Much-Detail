---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- dynamic-arrays
- generation
subTopics:
- number-sequences
- array-generation
- row-column-numbers
- counting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sequence Function
- Number Generator
- Array Generator
- Series Creator
tags:
- sequence
- dynamic-arrays
- generation
- numbers
- excel-365
- automation
---

# SEQUENCE

## Description

**SEQUENCE** generates an array of sequential numbers with customizable dimensions, starting point, and increment. Enter a single formula, and it spills an entire matrix of numbers across your worksheet. Need row numbers 1 through 100? Just `=SEQUENCE(100)`. Need a 5x5 grid starting at 0 with increments of 2? That's `=SEQUENCE(5, 5, 0, 2)`. It eliminates the tedious process of filling down or dragging formulas to create numbered lists.

The function's power multiplies when combined with other formulas. Use SEQUENCE to generate index numbers for INDEX, create date series for reports, build multiplication tables, generate test data, or create dynamic header rows. Before SEQUENCE, creating a 2D array of numbers required complex array formulas or manual data entry. Now it's a single, elegant formula that automatically adjusts when you change parameters.

**SEQUENCE in Google Sheets:** Google Sheets added SEQUENCE support in 2022, making this function fully cross-platform compatible. The syntax and behavior are identical between Excel and Sheets. Whether you're generating row numbers, creating calendar grids, or building multiplication tables, your SEQUENCE formulas work the same in both environments. The only consideration is ensuring recipient spreadsheet applications support dynamic arrays.

**Integration with other functions:** SEQUENCE becomes even more powerful as input to other functions. Feed SEQUENCE to INDEX to extract every nth row. Use it with INDIRECT to reference dynamic ranges. Combine with DATE to generate date sequences. Pass it to TEXT for formatted number series. The humble number sequence becomes the foundation for sophisticated dynamic calculations that automatically expand as needs change.

## Syntax

> [!f(x)] SEQUENCE Syntax
>
> ```
> =SEQUENCE(rows, [columns], [start], [step])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rows` | Yes | Number of rows to generate. Must be a positive integer. The sequence fills rows first, then columns. |
| `columns` | No | Number of columns to generate. Default is 1 (single column). Must be a positive integer if specified. |
| `start` | No | The first number in the sequence. Default is 1. Can be any number, including negative or decimal. |
| `step` | No | The increment between consecutive numbers. Default is 1. Can be negative (counting down) or decimal. Zero step creates constant arrays. |

### Return Value

Returns a spilled array of numbers with the specified dimensions. Array fills left-to-right across columns, then top-to-bottom across rows. Total count of numbers is rows * columns.

## Examples

> [!f(x)] SEQUENCE Examples

### Example 1: Simple Sequential List
```
=SEQUENCE(10)
```
**Result:** Column of numbers 1 through 10

**Explanation:** The most basic use. One parameter creates a single-column array of that many sequential numbers starting at 1. Perfect for generating row numbers or simple lists.

---

### Example 2: Two-Dimensional Grid
```
=SEQUENCE(3, 4)
```
**Result:** 3 rows by 4 columns, numbers 1-12 filling left-to-right, top-to-bottom

**Explanation:** Creates a matrix of sequential numbers. First row: 1,2,3,4. Second row: 5,6,7,8. Third row: 9,10,11,12. Useful for grid-based calculations.

---

### Example 3: Custom Starting Number
```
=SEQUENCE(5, 1, 100)
```
**Result:** 100, 101, 102, 103, 104 (column)

**Explanation:** Third parameter sets the starting value. Start at 100 instead of 1. Common for invoice numbers, order IDs, or any series not starting at 1.

---

### Example 4: Custom Increment (Step)
```
=SEQUENCE(6, 1, 0, 5)
```
**Result:** 0, 5, 10, 15, 20, 25

**Explanation:** Fourth parameter controls the step between numbers. Here, counting by 5s starting from 0. Useful for even numbers (step 2), decades (step 10), etc.

---

### Example 5: Counting Backwards
```
=SEQUENCE(5, 1, 10, -1)
```
**Result:** 10, 9, 8, 7, 6

**Explanation:** Negative step creates descending sequences. Starts at 10 and decrements by 1. Useful for countdowns, ranking from high to low, or reverse order lists.

---

### Example 6: Decimal Sequences
```
=SEQUENCE(5, 1, 0.1, 0.1)
```
**Result:** 0.1, 0.2, 0.3, 0.4, 0.5

**Explanation:** Both start and step can be decimals. Creates fractional sequences for precise calculations, percentage steps, or measurement increments.

---

### Example 7: Date Sequence
```
=SEQUENCE(7, 1, DATE(2024,1,1), 1)
```
**Result:** Seven consecutive dates starting January 1, 2024

**Explanation:** Since Excel dates are numbers, SEQUENCE creates date series. Each step of 1 adds one day. Format cells as dates to display properly.

---

### Example 8: Hourly Time Sequence
```
=SEQUENCE(24, 1, 0, 1/24)
```
**Result:** 24 time values representing each hour (0:00, 1:00, 2:00...)

**Explanation:** Time in Excel is fraction of a day. Step of 1/24 = one hour. Format as time to display hours. Use 1/1440 for minute increments.

---

### Example 9: Multiplication Table
```
=SEQUENCE(10, 1, 1, 1)*SEQUENCE(1, 10, 1, 1)
```
**Result:** 10x10 multiplication table

**Explanation:** Multiply a 10x1 column by a 1x10 row. Array multiplication creates the full 10x10 grid. Row 3, Column 5 = 15. Classic use of SEQUENCE for mathematical tables.

---

### Example 10: Generate Row Numbers for Data
```
=SEQUENCE(ROWS(A2:A100))
```
**Result:** Numbers 1 through 99 matching data row count

**Explanation:** Dynamic row numbering that adjusts to data. Combine with structured tables or COUNTA for truly dynamic counts that grow with your data.

---

### Example 11: Month Numbers for the Year
```
=SEQUENCE(12, 1, DATE(2024,1,1), 30.44)
```
**Result:** Approximate first of each month in 2024

**Explanation:** Average days per month is ~30.44. Not perfect for exact dates (use EDATE instead), but illustrates non-integer steps for approximate sequences.

---

### Example 12: Alphabetical Sequence
```
=CHAR(SEQUENCE(26, 1, 65, 1))
```
**Result:** Letters A through Z

**Explanation:** ASCII code 65 = "A". CHAR converts the sequence 65-90 into letters. Clever combination creates alphabet arrays for headers or labels.

---

### Example 13: INDEX with SEQUENCE for Every Nth Row
```
=INDEX(Data, SEQUENCE(ROWS(Data)/3, 1, 1, 3))
```
**Result:** Every 3rd row from Data range

**Explanation:** SEQUENCE with step 3 generates 1,4,7,10... INDEX uses these as row numbers to extract every third row. Powerful data sampling technique.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Rows or columns is zero or negative | Both dimensions must be positive integers |
| `#SPILL!` | Not enough empty cells for the array | Clear cells in the spill range or move the formula |
| `#CALC!` | Result would be too large | Excel has limits on spill array sizes; reduce dimensions |
| `Non-integer rows/columns` | Decimal values for row or column count | Use INT() or ROUND() to ensure whole numbers |
| `Unexpected results` | Step of 0 creates constant array | All values will be the start value when step is 0 |

## Use Cases

### [[Dynamic Report Row Numbering]]

**Scenario:** Monthly reports need row numbers that automatically adjust as data rows are added or removed, maintaining consistent numbering without manual updates.

**Implementation:**
```
=SEQUENCE(COUNTA(B2:B1000))
```
Where column B contains the data. Row numbers automatically match data count.

**Business Application:** Reports stay clean with consecutive numbering regardless of data volume. No more broken sequences or manual renumbering when rows are inserted or deleted.

**Technical Details:** COUNTA counts non-empty cells, making the sequence truly dynamic. For structured tables, use ROWS(Table1) for better reliability.

---

### [[Calendar and Date Grid Generation]]

**Scenario:** Create monthly calendar grids showing all dates for a given month in standard 7-column (Sun-Mon-Tue-Wed-Thu-Fri-Sat) layout.

**Implementation:**
```
=LET(
  first_day, DATE(2024, 1, 1),
  day_offset, WEEKDAY(first_day, 2),
  SEQUENCE(6, 7, first_day - day_offset + 1)
)
```

**Business Application:** Generate calendar views for project planning, scheduling interfaces, or leave tracking. The formula adapts to any month/year input.

**Technical Details:** Adjust WEEKDAY type parameter (2=Monday start, 1=Sunday start) based on regional preferences. Format result cells as dates showing only day number.

---

### [[Test Data Generation]]

**Scenario:** Quality assurance needs sample datasets with sequential IDs, varied amounts, and systematic patterns for testing formulas and reports.

**Implementation:**
```
Invoice IDs: =SEQUENCE(100, 1, 10001)
Test amounts: =SEQUENCE(100, 1, 100, 50)
Department codes: =MOD(SEQUENCE(100)-1, 5)+1
```

**Business Application:** Quickly generate realistic test data for validating reports, testing formulas, or demonstrating systems. No need for manual data entry or external data sources.

**Technical Details:** Combine SEQUENCE with MOD for cycling patterns, RANDBETWEEN for variation, or TEXT for formatted strings. Build complete test datasets with multiple columns of synthetic data.

---

### [[Batch Processing Indices]]

**Scenario:** Process large datasets in batches of 1000 records, generating start and end indices for each batch automatically.

**Implementation:**
```
Batch starts: =SEQUENCE(ROUNDUP(ROWS(Data)/1000,0), 1, 1, 1000)
Batch ends: =MIN(SEQUENCE(ROUNDUP(ROWS(Data)/1000,0), 1, 1, 1000)+999, ROWS(Data))
```

**Business Application:** Break large operations into manageable chunks for processing, exports, or API calls that have record limits.

**Technical Details:** The MIN function caps the final batch at actual data count. Combine with INDEX to extract each batch dynamically.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Array behavior:** Automatic spill to adjacent cells
- **Maximum size:** Limited by Excel's calculation engine and available memory
- **Performance:** Highly optimized for large sequences

### Google Sheets
- **Availability:** Added in 2022, available to all users
- **Array behavior:** Identical spill behavior
- **Maximum size:** Similar practical limits
- **Performance:** Comparable to Excel

### Key Differences
SEQUENCE works identically across both platforms. Formulas are fully portable. The only consideration is older versions of Excel (2019 and earlier) that don't support dynamic arrays, where SEQUENCE won't work at all.

### Alternatives for Older Excel
In Excel 2019 and earlier, create sequences using:
- Fill down with relative references (A1+1)
- ROW() function for row numbers: `=ROW()-ROW($A$1)+1`
- Array formulas with ROW/COLUMN and INDIRECT

## Tips and Best Practices

1. **Start simple:** Begin with just the rows parameter to understand basic behavior. Add columns, start, and step as needed.

2. **Remember the fill order:** SEQUENCE fills left-to-right, then top-to-bottom. A 3x4 sequence puts 1-4 in row 1, 5-8 in row 2, 9-12 in row 3.

3. **Use for dynamic row numbers:** Instead of manually typing 1,2,3..., use SEQUENCE with COUNTA or ROWS to create self-adjusting numbering.

4. **Combine with CHAR for letters:** `=CHAR(SEQUENCE(26,1,65))` creates A-Z. `=CHAR(SEQUENCE(26,1,97))` creates a-z.

5. **Create date series with proper steps:** For dates, step=1 means daily, step=7 means weekly. For months, use EDATE instead for accuracy.

6. **Generate test data efficiently:** SEQUENCE + random functions creates instant test datasets without external data sources.

7. **Use for INDEX/MATCH patterns:** SEQUENCE generates row/column numbers for extracting data at regular intervals or positions.

8. **Zero step for constant arrays:** `=SEQUENCE(10,5,0,0)` creates a 10x5 array of zeros. Useful as a template for other calculations.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[RANDARRAY]] | Generates random number array | When you need random values instead of sequential |
| [[ROW]] | Returns row number of reference | When you need just current row number, not a sequence |
| [[COLUMN]] | Returns column number of reference | When you need current column number |
| [[MAKEARRAY]] | Creates array via LAMBDA | When sequence logic is complex or non-linear |

### Commonly Used Together

**[[INDEX]]** - Extract values at sequence positions

*Get every nth row from data:*
```
=INDEX(Data, SEQUENCE(ROWS(Data)/2, 1, 2, 2))
```
Returns every 2nd row starting from row 2.

---

**[[DATE]]** - Create date sequences

*Generate month-end dates:*
```
=EOMONTH(DATE(2024,1,1), SEQUENCE(12,1,0,1))
```
Creates array of last day of each month.

---

**[[CHAR]]** - Convert to characters

*Create letter labels:*
```
=CHAR(64+SEQUENCE(1,10))
```
Generates A through J as column headers.

---

**[[TEXT]]** - Format sequence values

*Create formatted ID numbers:*
```
=TEXT(SEQUENCE(100,1,1),"INV-0000")
```
Generates INV-0001, INV-0002, etc.

---

**[[IF]]** - Conditional sequence logic

*Skip sequence values:*
```
=IF(MOD(SEQUENCE(20),2)=0, SEQUENCE(20), "")
```
Shows only even numbers, blanks for odd.

---

**[[SUMPRODUCT]]** - Calculate with sequences

*Sum of first n integers:*
```
=SUM(SEQUENCE(100))
```
Returns 5050 (sum of 1-100). Faster than writing complex formulas.

## Official Documentation

- **Microsoft Excel:** [SEQUENCE function](https://support.microsoft.com/en-us/office/sequence-function-57467a98-57e0-4817-9f14-2eb78519ca90)
- **Google Sheets:** [SEQUENCE function](https://support.google.com/docs/answer/12405893)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2019 (Insider), 2020 (General) | Part of dynamic array functions release |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for web | 2020 | Full feature parity |
| Google Sheets | 2022 | Added to match Excel functionality |
| Excel 2019 | Not available | Must use ROW/COLUMN or fill methods instead |

---

*Last updated: 2026-01-10*
