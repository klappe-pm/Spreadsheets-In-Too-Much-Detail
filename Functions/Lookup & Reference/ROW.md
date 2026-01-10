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
- row-number
- dynamic-formulas
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Row Number
- Row Position
- Get Row
tags:
- lookup
- reference
- row
- position
- dynamic-reference
---

# ROW

## Description

**ROW** returns the row number of a cell reference. At its simplest, `=ROW()` tells you which row your formula is in—put it in cell B5 and it returns 5. Give it a reference like `=ROW(A10)` and it returns 10. This seemingly basic function is actually a cornerstone of dynamic formula design, enabling everything from sequential numbering to conditional formatting rules to creating row-based patterns that automatically adjust when data moves.

The power of ROW becomes apparent when you stop thinking of it as "get a row number" and start thinking of it as "generate a number based on position." Need to number rows 1, 2, 3 regardless of where your data starts? Use `=ROW()-ROW($A$1)` (if your data starts in row 1) or simply `=ROW(A1)` and drag down. Need alternating colors? Use `=MOD(ROW(),2)=0` in conditional formatting. Need to create a sequence without SEQUENCE function? ROW can build arrays in older Excel and Sheets.

**When given a range, ROW returns an array of row numbers.** `=ROW(A1:A5)` returns {1;2;3;4;5} as a vertical array. In modern Excel with dynamic arrays, this spills automatically. In Google Sheets and older Excel, you'd wrap this in an array formula or use it within functions that expect arrays (like SUMPRODUCT, INDEX, or MATCH). This array behavior makes ROW essential for creating numbered sequences and for advanced array formula patterns.

**A crucial detail:** ROW returns the row number in the spreadsheet, not the position within your data range. If your table starts in row 5, the first data row is still row 5, not row 1. This trips up users expecting "relative" row numbers. For relative positioning, subtract the starting row: `=ROW()-ROW($A$4)` gives you 1, 2, 3... for data starting in row 5. Understanding this absolute vs. relative distinction is key to using ROW effectively.

## Syntax

> [!f(x)] ROW Syntax
>
> ```
> =ROW([reference])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `reference` | No | The cell or range of cells for which you want the row number. If omitted, returns the row number of the cell containing the formula. |

### Return Value

Returns a number representing the row number of the reference. If reference is a range, returns an array of row numbers (one for each row in the range). If reference is omitted, returns the row number of the cell containing the ROW function.

## Examples

> [!f(x)] ROW Examples

### Example 1: Basic Row Number of Current Cell
```
=ROW()
```
**Result:** Returns the row number where this formula is located (e.g., 7 if in row 7)

**Explanation:** With no argument, ROW returns the row number of the cell containing the formula. This is useful for self-referencing formulas and creating row-dependent calculations without hardcoding values.

---

### Example 2: Row Number of a Specific Cell
```
=ROW(B15)
```
**Result:** 15

**Explanation:** Returns the row number of cell B15, which is 15. The column (B) is irrelevant—ROW only cares about the row. This is useful when you need to reference a specific row number in calculations.

---

### Example 3: Creating Sequential Numbers Starting from 1
```
=ROW()-ROW($A$1)+1
```
**Result:** 1, 2, 3, 4... when dragged down starting from row 1

**Explanation:** This pattern creates sequential numbers starting from 1 regardless of where your actual data begins. If your header is in row 1 and data starts in row 2, place this in row 2 and drag down. The formula subtracts the starting row reference and adds 1.

---

### Example 4: Row Number of a Range (Array Result)
```
=ROW(A1:A5)
```
**Result:** {1;2;3;4;5} (vertical array)

**Explanation:** When given a multi-row range, ROW returns an array containing each row number. In Excel 365, this spills automatically. In Google Sheets, wrap in ARRAYFORMULA or use within array-aware functions. This is powerful for generating sequences.

---

### Example 5: Creating Alternating Row Patterns
```
=IF(MOD(ROW(),2)=0,"Even","Odd")
```
**Result:** "Even" in even-numbered rows, "Odd" in odd-numbered rows

**Explanation:** Combining ROW with MOD creates alternating patterns. MOD(ROW(),2) returns 0 for even rows and 1 for odd rows. This pattern is commonly used in conditional formatting to create zebra-striped tables for readability.

---

### Example 6: Using ROW in Conditional Formatting
```
=MOD(ROW(),2)=1
```
**Result:** Applied as conditional formatting rule, highlights odd rows

**Explanation:** In conditional formatting, apply this formula to alternate row highlighting. Select your range, create a new rule using formula, and enter this. The rule applies to rows where the row number divided by 2 has a remainder of 1 (odd rows).

---

### Example 7: Relative Row Position Within a Table
```
=ROW()-ROW(DataTable[#Headers])
```
**Result:** 1 for first data row, 2 for second, etc.

**Explanation:** When working with Excel Tables, this returns the relative row position within the table data (excluding headers). `DataTable[#Headers]` references the header row, so subtracting it gives you the data row number. Useful for row-based calculations in structured tables.

---

### Example 8: Creating a Sequence Without SEQUENCE Function
```
=ROW(INDIRECT("1:"&A1))
```
**Result:** Array from 1 to the value in A1

**Explanation:** In older Excel versions without SEQUENCE, this creates a numeric sequence. If A1 contains 10, this returns {1;2;3;4;5;6;7;8;9;10}. INDIRECT creates a reference to rows 1:10, and ROW returns their row numbers. Use within array formulas.

---

### Example 9: ROW with INDEX for Dynamic References
```
=INDEX(B:B, ROW()+5)
```
**Result:** Returns value from column B, 5 rows below current row

**Explanation:** ROW provides dynamic positioning for INDEX. If this formula is in row 3, it returns the value from B8 (3+5=8). As the formula is copied down, it continues referencing 5 rows ahead. Useful for offset calculations without OFFSET function.

---

### Example 10: Checking If Row Is Within Data Range
```
=IF(ROW()<=COUNTA(A:A), "Has Data", "Empty")
```
**Result:** "Has Data" if current row is within data range, "Empty" otherwise

**Explanation:** Compares the current row number against the count of non-empty cells in column A. Useful for formulas that should only operate on rows containing data, preventing calculations on empty rows.

---

### Example 11: Every Nth Row Selection
```
=IF(MOD(ROW()-1,3)=0, A1, "")
```
**Result:** Shows value every 3rd row, blank otherwise

**Explanation:** This pattern selects every Nth row. MOD(ROW()-1,3)=0 is true for rows 1, 4, 7, 10, etc. Adjust the -1 and divisor to change the starting point and interval. Useful for sampling data or creating summary rows.

---

### Example 12: ROW in Array Formula for Lookup
```
=INDEX(B:B, MATCH(MAX(IF(A:A="Target", ROW(A:A))), ROW(A:A), 0))
```
**Result:** Returns value from column B for the LAST occurrence of "Target" in column A

**Explanation:** This array formula finds the last match rather than the first. IF creates an array of row numbers where A="Target" (and FALSE elsewhere), MAX finds the highest row number, MATCH locates that row, and INDEX returns the corresponding B value.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Reference points to a deleted row or invalid range | Check that referenced cells exist. If rows were deleted, update the reference. |
| `#VALUE!` | Reference contains multiple non-contiguous areas (in some contexts) | Use a single contiguous range, or handle multiple areas separately. |
| `Unexpected array` | ROW(range) returns array but formula expects single value | Wrap in INDEX to get first value, or use in array-aware context. In older Excel, enter as array formula (Ctrl+Shift+Enter). |
| `Wrong number` | Expecting relative position but ROW returns absolute row number | Subtract starting row: `=ROW()-ROW($A$StartRow)+1` for relative numbering. |
| `#NAME?` | Misspelled function name | Verify spelling is ROW, not ROWS (different function). |

## Use Cases

### [[Sequential Row Numbering]]

**Scenario:** You need to add automatic row numbers to a data table that will maintain correct numbering even when rows are inserted, deleted, or filtered.

**Implementation:**
```
For data starting in row 2 with header in row 1:
=ROW()-1

Or more robust version:
=IF(A2<>"", ROW()-ROW($A$1), "")
```

**Business Application:** Invoice line items, inventory lists, employee rosters, or any table requiring persistent sequential numbering. Unlike manually entered numbers, ROW-based numbering adjusts automatically during sorting and filtering operations when used with Excel Tables.

**Technical Details:** The formula `=ROW()-1` assumes data starts in row 2. For tables starting elsewhere, use `=ROW()-ROW($Header$Row)`. The IF wrapper prevents numbers from appearing in empty rows. In Excel Tables, `=ROW()-ROW(TableName[#Headers])` provides reliable relative numbering.

---

### [[Conditional Formatting Patterns]]

**Scenario:** Apply visual formatting patterns to a table—alternating row colors, highlighting every 5th row, or creating gradient effects based on row position.

**Implementation:**
```
Alternating rows (zebra stripes):
=MOD(ROW(),2)=0

Every 5th row highlighted:
=MOD(ROW(),5)=0

Highlight rows based on relative position (top 10 rows of data):
=ROW()-ROW($A$1)<=10
```

**Business Application:** Financial reports with alternating row colors for readability, weekly summaries highlighting every 7th row, or dashboards with visual hierarchy based on data position.

**Technical Details:** In conditional formatting, select the entire range first, then apply the formula rule. The formula is evaluated for each cell in the range, with ROW() returning that cell's row. Use absolute references ($) for anchor points to ensure consistent behavior across the selection.

---

### [[Dynamic Array Generation]]

**Scenario:** Create numbered sequences or position-based arrays for use in complex array formulas, especially in environments without the SEQUENCE function.

**Implementation:**
```
Create sequence 1 to N:
=ROW(INDIRECT("1:"&N))

Create array of row positions for a range:
=ROW(A1:A100)

Use in SUMPRODUCT for position-weighted calculations:
=SUMPRODUCT(B2:B100, ROW(B2:B100)-1)
```

**Business Application:** Weighted averages where position matters (declining balance calculations), generating test data sequences, creating arrays for complex INDEX/MATCH operations.

**Technical Details:** ROW returning arrays is essential for many advanced techniques. In Excel 365, dynamic arrays spill automatically. In Google Sheets, use ARRAYFORMULA wrapper. In older Excel, these work within array-aware functions like SUMPRODUCT or require Ctrl+Shift+Enter entry.

---

### [[Formula Auditing and Documentation]]

**Scenario:** Create self-documenting spreadsheets where formulas reference their own position for validation, logging, or debugging purposes.

**Implementation:**
```
Self-documenting formula location:
="This formula is in row "&ROW()&", column "&COLUMN()

Validation check:
=IF(ROW()<>EXPECTED_ROW, "WARNING: Formula moved!", CalculationResult)

Change tracking:
=IF(A2<>PreviousVersion!A2, "Changed in row "&ROW(), "")
```

**Business Application:** Audit trails in financial models, change detection in data reconciliation, quality control in template distribution where formula integrity must be verified.

**Technical Details:** Self-referencing formulas using ROW() help identify when formulas have been moved or copied incorrectly. This is particularly valuable in large financial models where formula consistency across rows is critical for accuracy.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Array behavior:** Returns array when given range; requires Ctrl+Shift+Enter in pre-365 versions for array context
- **Dynamic arrays (365):** Automatically spills when ROW(range) is entered normally
- **With structured references:** Works with Table references like `ROW(Table1[@Column])`
- **Performance:** Extremely fast; recalculates only when row structure changes

### Google Sheets

- **Availability:** All versions
- **Array behavior:** Returns array when given range; often requires ARRAYFORMULA wrapper for spilling
- **Syntax:** Identical to Excel
- **Performance:** Highly optimized; no concerns with ROW calculations
- **Named ranges:** Works with named ranges: `ROW(MyNamedRange)`

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Array formula entry | Ctrl+Shift+Enter (legacy) / Auto (365) | ARRAYFORMULA wrapper or auto |
| Structured table references | Full support | Limited (no Tables) |
| Maximum row number | 1,048,576 | 10,000,000 (varies by plan) |
| Volatile recalculation | No (only recalcs when structure changes) | No |

## Tips and Best Practices

1. **Use ROW() without arguments for self-reference:** When you need the current row number, `=ROW()` is cleaner and more maintainable than `=ROW(A5)` with a specific reference that might need updating.

2. **Create relative numbering with subtraction:** `=ROW()-ROW($StartCell)+1` provides 1, 2, 3... numbering regardless of where your data actually starts. Lock the start cell reference with $ signs.

3. **Combine with MOD for patterns:** `MOD(ROW(),N)` creates repeating patterns every N rows. This is the foundation for alternating formats, cyclic selections, and periodic calculations.

4. **Remember ROW returns absolute position:** If your data starts in row 10, the first data row is row 10, not row 1. Plan accordingly when using ROW in calculations that expect relative positioning.

5. **Use ROW(range) for sequence generation:** Before SEQUENCE existed, `ROW(INDIRECT("1:"&N))` was the standard way to generate 1 to N sequences. It still works in all versions and platforms.

6. **Prefer ROW over hardcoded numbers:** Instead of `=INDEX(A:A,15)`, consider `=INDEX(A:A,ROW()+offset)` for formulas that should be position-aware and copyable.

7. **Test with filtered data:** ROW returns the actual spreadsheet row, not the visible row number when filtering. If you need visible row numbers, use SUBTOTAL-based approaches or AGGREGATE function.

8. **Use in INDIRECT for dynamic references:** `=INDIRECT("A"&ROW())` creates a reference to column A in the current row—useful for building dynamic references in complex formulas.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COLUMN]] | Returns the column number of a reference | When you need column position instead of row position |
| [[ROWS]] | Returns the number of rows in a range | When you need to count rows, not get a row number |
| [[CELL]] | Returns information about a cell's formatting, location, or contents | When you need multiple types of cell information beyond just row number |
| [[ADDRESS]] | Creates a cell address as text from row and column numbers | When you need to build a cell reference string, not just get a row number |

### Commonly Used Together

**[[COLUMN]]** - Get column number

*Combined with ROW for complete position tracking:*
```
="Row "&ROW()&", Column "&COLUMN()
```
Together, ROW and COLUMN provide complete cell positioning information for navigation, auditing, and dynamic formulas.

---

**[[INDEX]]** - Return value at position

*Combined with ROW for dynamic row references:*
```
=INDEX(DataRange, ROW()-StartRow+1, 1)
```
ROW provides the dynamic row number that INDEX uses to retrieve values, enabling formulas that automatically adjust based on their position.

---

**[[INDIRECT]]** - Create reference from text

*Combined with ROW for dynamic cell references:*
```
=INDIRECT("A"&ROW())
```
ROW provides the row number that INDIRECT incorporates into a cell reference string, useful for self-referencing formulas.

---

**[[MOD]]** - Return remainder after division

*Combined with ROW for repeating patterns:*
```
=MOD(ROW(),3)
```
Creates cyclic patterns: 1,2,0,1,2,0... Used in conditional formatting and for selecting every Nth row.

---

**[[SEQUENCE]]** - Generate array of sequential numbers

*Alternative to ROW for sequence generation:*
```
=SEQUENCE(10) vs =ROW(INDIRECT("1:10"))
```
SEQUENCE is the modern replacement for ROW-based sequence generation in Excel 365 and recent Google Sheets.

## Official Documentation

- **Microsoft Excel:** [ROW function](https://support.microsoft.com/en-us/office/row-function-3a63b74a-c4d0-4093-b49a-e76eb49a6d8d)
- **Google Sheets:** [ROW function](https://support.google.com/docs/answer/3093316)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original implementation |
| Excel 2007 | Enhanced | Support for larger worksheets (1,048,576 rows) |
| Excel 365 | Dynamic Arrays | ROW(range) now spills automatically without Ctrl+Shift+Enter |
| Google Sheets | Original launch (2006) | Full compatibility with Excel behavior |

---

*Last updated: 2026-01-10*
