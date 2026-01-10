---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- cell-reference
- position-functions
- column-number
- dynamic-formulas
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Column Number
- Column Position
- Get Column
- Col Number
tags:
- lookup
- reference
- column
- position
- dynamic-reference
---

# COLUMN

## Description

**COLUMN** returns the column number of a cell reference. Enter `=COLUMN()` in cell D7 and it returns 4 (because D is the 4th column). Give it a reference like `=COLUMN(F1)` and it returns 6. While this seems trivially simple, COLUMN is a foundational function for building dynamic spreadsheets—enabling horizontal patterns, dynamic column selection, and formulas that adapt as your spreadsheet structure changes.

Think of COLUMN as ROW's horizontal counterpart. Where ROW enables vertical patterns and row-based numbering, COLUMN enables horizontal patterns and column-based calculations. Need to create a formula that references "3 columns to the right"? Use `=INDIRECT(ADDRESS(ROW(), COLUMN()+3))`. Need to highlight every other column? Use `=MOD(COLUMN(),2)=0` in conditional formatting. COLUMN transforms column letters into numbers that your formulas can calculate with.

**When given a range, COLUMN returns an array of column numbers.** `=COLUMN(A1:E1)` returns {1,2,3,4,5} as a horizontal array. In modern Excel with dynamic arrays, this spills automatically across cells. This array behavior makes COLUMN useful for generating horizontal sequences—especially in pre-SEQUENCE environments—and for array formulas that need to know column positions.

**The column letter vs. column number distinction matters.** Users often think in letters (A, B, C...) but formulas work with numbers (1, 2, 3...). Column A = 1, Column Z = 26, Column AA = 27, and so on. COLUMN bridges this gap, converting letter-based references into calculable numbers. This is essential when building INDEX formulas, INDIRECT references, or any formula that needs to programmatically select columns.

## Syntax

> [!f(x)] COLUMN Syntax
>
> ```
> =COLUMN([reference])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `reference` | No | The cell or range for which you want the column number. If omitted, returns the column number of the cell containing the formula. |

### Return Value

Returns a number representing the column number of the reference. Column A = 1, B = 2, C = 3, etc. If reference is a multi-column range, returns a horizontal array of column numbers. If reference is omitted, returns the column number of the cell containing the COLUMN function.

## Examples

> [!f(x)] COLUMN Examples

### Example 1: Basic Column Number of Current Cell
```
=COLUMN()
```
**Result:** Returns the column number where this formula is located (e.g., 4 if in column D)

**Explanation:** With no argument, COLUMN returns the column number of the cell containing the formula. In column A it returns 1, in column Z it returns 26, in column AA it returns 27. Useful for position-dependent calculations.

---

### Example 2: Column Number of a Specific Cell
```
=COLUMN(G15)
```
**Result:** 7

**Explanation:** Returns the column number of cell G15, which is 7 (G is the 7th letter). The row number (15) is irrelevant—COLUMN only cares about the column. Useful when you need to reference a specific column position in calculations.

---

### Example 3: Creating Horizontal Sequential Numbers
```
=COLUMN()-COLUMN($A1)+1
```
**Result:** 1, 2, 3, 4... when dragged right starting from column A

**Explanation:** This pattern creates sequential numbers starting from 1 regardless of which column you start in. By subtracting the anchor column and adding 1, you get relative column positions. Essential for matrix calculations and horizontal data layouts.

---

### Example 4: Column Numbers of a Range (Array Result)
```
=COLUMN(B1:F1)
```
**Result:** {2,3,4,5,6} (horizontal array)

**Explanation:** When given a multi-column range, COLUMN returns an array of column numbers. This spills horizontally in Excel 365 or within ARRAYFORMULA in Google Sheets. Useful for generating horizontal sequences without SEQUENCE.

---

### Example 5: Converting Column Number to Letter
```
=SUBSTITUTE(ADDRESS(1,COLUMN()),"$","")
```
**Result:** Returns column letter (e.g., "D1" becomes "D" after removing row and $)

**Explanation:** While COLUMN gives you the number, sometimes you need the letter. ADDRESS creates "$D$1", SUBSTITUTE removes the $ signs, and you can further process to extract just the letter. A more direct approach: `=LEFT(ADDRESS(1,COLUMN(),4),LEN(ADDRESS(1,COLUMN(),4))-1)`.

---

### Example 6: Using COLUMN in Conditional Formatting
```
=MOD(COLUMN(),2)=0
```
**Result:** Applied as conditional formatting rule, highlights even-numbered columns

**Explanation:** Creates alternating column highlighting (zebra stripes vertically). MOD(COLUMN(),2) returns 0 for even columns (B, D, F...) and 1 for odd columns (A, C, E...). Apply to your data range for visual column separation.

---

### Example 7: Dynamic Column Reference with INDEX
```
=INDEX($A$1:$Z$100, ROW(), COLUMN()-3)
```
**Result:** Returns value from 3 columns to the left in the same row

**Explanation:** If this formula is in column E, COLUMN()-3 = 2, so INDEX returns the value from column B of the data range. As you copy the formula right, it continues to look 3 columns back. Useful for creating offset references without OFFSET.

---

### Example 8: COLUMN with MATCH for Dynamic Column Selection
```
=INDEX(DataTable, MATCH("ProductA", A:A, 0), MATCH("Q2", 1:1, 0))
```
**Result:** Returns the intersection of ProductA row and Q2 column

**Explanation:** While this uses MATCH rather than COLUMN directly, understanding that MATCH returns a column number (just like COLUMN) is key. You can replace the second MATCH with `COLUMN(Q2_Cell)` if you have a reference to the Q2 header.

---

### Example 9: Creating Month Numbers Horizontally
```
=COLUMN()-COLUMN($B$1)+1
```
**Result:** 1, 2, 3... 12 when placed in B1:M1 and dragged right

**Explanation:** Perfect for 12-month headers where you need month numbers for MONTH/DATE functions. If your months start in column B, this returns 1 in B, 2 in C, etc. Combine with DATE for dynamic date generation.

---

### Example 10: COLUMN in Array Formula for Transposition Logic
```
=INDEX(SourceRange, COLUMN(A1), ROW(A1))
```
**Result:** Transposes data by swapping row and column references

**Explanation:** By using COLUMN for what's normally the row argument and ROW for the column argument, you create a manual transpose effect. Drag both down and right to transpose a range. COLUMN(A1) gives 1, and as you drag right, COLUMN(B1) gives 2, etc.

---

### Example 11: Every Nth Column Selection
```
=IF(MOD(COLUMN()-1,3)=0, A1, "")
```
**Result:** Shows value every 3rd column (A, D, G, J...), blank otherwise

**Explanation:** Selects every Nth column for sampling or summary purposes. MOD(COLUMN()-1,3)=0 is true for columns 1, 4, 7, 10, etc. Adjust the offset (-1) and divisor (3) to change the pattern.

---

### Example 12: Building Cell References Dynamically
```
=INDIRECT("R"&ROW()&"C"&COLUMN()+1, FALSE)
```
**Result:** Returns value from one column to the right using R1C1 notation

**Explanation:** Uses R1C1-style referencing with INDIRECT. COLUMN()+1 calculates the target column number. The FALSE parameter tells INDIRECT to expect R1C1 notation. This is sometimes cleaner than A1-style for programmatic references.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Reference points to a deleted column or invalid range | Check that referenced cells exist. If columns were deleted, update the reference. |
| `#VALUE!` | Using COLUMN in a context that can't handle the returned array | Wrap in INDEX to extract a single value, or use within array-aware functions. |
| `Unexpected array` | COLUMN(range) returns array but formula expects single value | In older Excel, enter as array formula (Ctrl+Shift+Enter) or use INDEX(COLUMN(range),1) for first value. |
| `Wrong number` | Expecting column letter but getting column number | COLUMN returns numbers, not letters. Use ADDRESS or SUBSTITUTE tricks to convert to letters if needed. |
| `#NAME?` | Misspelled function name | Verify spelling is COLUMN, not COLUMNS (different function that counts columns). |

## Use Cases

### [[Dynamic Dashboard Column Selection]]

**Scenario:** A financial dashboard lets users select which metric to display (Revenue, Costs, Profit, etc.) from a dropdown. The selected metric determines which column of data to show.

**Implementation:**
```
Dropdown in cell B1 containing metric names
Metric headers in row 1 of DataTable

Display formula:
=INDEX(DataTable, ROW()-HeaderRow, MATCH($B$1, DataTable[#Headers], 0))

Alternative using COLUMN if you have a reference:
=INDEX(DataTable, ROW()-HeaderRow, COLUMN(RevenueColumn)-COLUMN(DataTable)+1)
```

**Business Application:** Executive dashboards, KPI displays, and report generators where users interactively select which data columns to view. This eliminates the need for multiple static reports.

**Technical Details:** COLUMN converts a column reference into a number that INDEX can use. When your data structure changes, formulas using COLUMN adapt automatically because they reference the column position rather than hardcoded numbers.

---

### [[Horizontal Data Patterns]]

**Scenario:** Create formulas that generate patterns across columns—monthly headers, repeating category labels, or calculated sequences that flow horizontally.

**Implementation:**
```
Month numbers (1-12) across B1:M1:
=COLUMN()-COLUMN($B$1)+1

Month names:
=TEXT(DATE(2024, COLUMN()-COLUMN($B$1)+1, 1), "mmmm")

Quarter indicators (Q1, Q1, Q1, Q2, Q2, Q2...):
="Q"&ROUNDUP((COLUMN()-COLUMN($B$1)+1)/3, 0)
```

**Business Application:** Financial models with monthly columns, project timelines with weekly columns, or any horizontally-oriented time series where column position determines the time period.

**Technical Details:** The pattern `COLUMN()-COLUMN($AnchorColumn)+1` provides 1-based relative column numbering. Lock the anchor with $ so it doesn't shift when copying. Combine with date functions, text functions, or math functions for sophisticated patterns.

---

### [[Matrix-Style Calculations]]

**Scenario:** Perform calculations on data arranged in a matrix where both row and column positions matter—like a multiplication table, distance matrix, or cross-tabulated analysis.

**Implementation:**
```
Multiplication table:
=ROW()*COLUMN()

Distance from diagonal (useful for matrix operations):
=ABS(ROW()-COLUMN())

Mirror/symmetric matrix:
=INDEX(SourceMatrix, COLUMN(), ROW())
```

**Business Application:** Pricing matrices, correlation tables, from-to distance charts, and any analysis requiring awareness of both row and column position for each cell's calculation.

**Technical Details:** ROW() and COLUMN() together provide complete positional awareness. For matrices starting away from A1, use relative positioning: `(ROW()-$StartRow)` and `(COLUMN()-$StartCol)`. This enables complex matrix algebra in spreadsheets.

---

### [[Conditional Formatting Based on Column Position]]

**Scenario:** Apply visual formatting rules that depend on column position—highlighting specific columns, creating column-based color gradients, or marking every Nth column.

**Implementation:**
```
Highlight columns C, F, I (every 3rd starting from C):
=MOD(COLUMN()-3, 3)=0

Gradient based on column position (for color scales):
=COLUMN()/MAX_COLUMN

Highlight columns after a certain point:
=COLUMN()>COLUMN($E$1)
```

**Business Application:** Financial statements highlighting subtotal columns, project schedules marking milestone dates, or any report where column position carries semantic meaning that should be visually reinforced.

**Technical Details:** Conditional formatting formulas are evaluated for each cell in the selected range. COLUMN() returns that cell's column number. Use relative references (no $) for row-by-row variation, absolute references ($) for anchoring to specific columns.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Array behavior:** Returns array when given range; requires Ctrl+Shift+Enter in pre-365 versions
- **Dynamic arrays (365):** Automatically spills when COLUMN(range) is entered normally
- **With structured references:** Works with Table references like `COLUMN(Table1[@Column])`
- **Maximum column:** 16,384 (column XFD) in Excel 2007 and later

### Google Sheets

- **Availability:** All versions
- **Array behavior:** Returns array when given range; often needs ARRAYFORMULA wrapper
- **Syntax:** Identical to Excel
- **Maximum column:** 18,278 (column ZZZ)
- **Performance:** Highly optimized; no concerns with COLUMN calculations

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Array formula entry | Ctrl+Shift+Enter (legacy) / Auto (365) | ARRAYFORMULA wrapper or auto |
| Structured table references | Full support | Limited (no Tables) |
| Maximum column number | 16,384 (XFD) | 18,278 (ZZZ) |
| Column letter functions | No native function | No native function |

## Tips and Best Practices

1. **Use COLUMN() without arguments for self-reference:** When you need the current column number, `=COLUMN()` is cleaner than `=COLUMN(D5)` with a specific reference.

2. **Create relative column numbering:** `=COLUMN()-COLUMN($StartColumn)+1` gives 1, 2, 3... regardless of actual column letters. Always lock the start reference with $.

3. **Remember A=1, not A=0:** Unlike some programming languages, spreadsheet columns are 1-indexed. Column A is 1, not 0. Plan your calculations accordingly.

4. **Combine with MOD for horizontal patterns:** `MOD(COLUMN(),N)` creates repeating patterns across columns, just as MOD(ROW(),N) does vertically.

5. **Use COLUMN with INDEX for dynamic column selection:** `=INDEX(range, row, COLUMN()-offset)` creates formulas that automatically reference different columns based on position.

6. **Convert column numbers to letters when needed:** `=SUBSTITUTE(ADDRESS(1,COLUMN_NUMBER,4),"1","")` converts a column number back to its letter equivalent.

7. **COLUMN vs COLUMNS:** COLUMN returns the position of a column; COLUMNS returns how many columns are in a range. Don't confuse them.

8. **Test across different starting positions:** If your data doesn't start in column A, verify that your COLUMN-based formulas still work correctly by testing the edge cases.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ROW]] | Returns the row number of a reference | When you need row position instead of column position |
| [[COLUMNS]] | Returns the number of columns in a range | When you need to count columns, not get a column number |
| [[CELL]] | Returns information about a cell's formatting, location, or contents | When you need multiple types of cell information |
| [[ADDRESS]] | Creates a cell address as text from row and column numbers | When you need to build a cell reference string |

### Commonly Used Together

**[[ROW]]** - Get row number

*Combined with COLUMN for complete position tracking:*
```
="Row "&ROW()&", Column "&COLUMN()
```
Together, ROW and COLUMN provide complete cell positioning for navigation, auditing, and dynamic formulas.

---

**[[INDEX]]** - Return value at position

*Combined with COLUMN for dynamic column references:*
```
=INDEX(DataRange, 1, COLUMN()-StartCol+1)
```
COLUMN provides the column number that INDEX uses to retrieve values from different columns based on formula position.

---

**[[ADDRESS]]** - Create cell reference as text

*Combined with COLUMN to generate cell addresses:*
```
=ADDRESS(5, COLUMN())
```
Creates cell references like "$A$5", "$B$5", etc. based on the formula's column position.

---

**[[INDIRECT]]** - Create reference from text

*Combined with COLUMN for dynamic references:*
```
=INDIRECT(ADDRESS(ROW(), COLUMN()+1))
```
Uses COLUMN in address construction, then INDIRECT converts the text to an actual reference.

---

**[[MOD]]** - Return remainder after division

*Combined with COLUMN for repeating patterns:*
```
=MOD(COLUMN(),4)
```
Creates cyclic patterns across columns: 1,2,3,0,1,2,3,0... Used in conditional formatting and periodic column selection.

---

**[[MATCH]]** - Find position in a range

*Alternative to COLUMN for finding column positions:*
```
=MATCH("Header", 1:1, 0)
```
While COLUMN returns a known cell's position, MATCH finds a value's position—useful when you don't have a cell reference.

## Official Documentation

- **Microsoft Excel:** [COLUMN function](https://support.microsoft.com/en-us/office/column-function-44e8c754-711c-4df3-9da4-47a55042554f)
- **Google Sheets:** [COLUMN function](https://support.google.com/docs/answer/3093373)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original implementation |
| Excel 2007 | Enhanced | Support for wider worksheets (16,384 columns) |
| Excel 365 | Dynamic Arrays | COLUMN(range) now spills automatically |
| Google Sheets | Original launch (2006) | Full compatibility with Excel behavior |

---

*Last updated: 2026-01-10*
