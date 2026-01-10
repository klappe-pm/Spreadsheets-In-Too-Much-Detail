---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- information
subTopics:
- sheet-position
- sheet-number
- workbook-structure
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sheet Number
- Sheet Position
- Get Sheet Index
tags:
- information
- sheet-number
- sheet-position
- workbook
- navigation
---

# SHEET

## Description

**SHEET** returns the sheet number (index position) of a referenced sheet within a workbook. Think of sheets as having invisible numbers based on their left-to-right position in the tab bar: the leftmost sheet is 1, the next is 2, and so on. SHEET reveals these position numbers, which is useful for dynamic sheet references, navigation formulas, and understanding workbook structure.

Called without arguments, SHEET returns the number of the current sheet—the one containing the formula. Called with a reference to another sheet (like `=SHEET(OtherSheet!A1)`), it returns that sheet's position number. This enables formulas that know where they are in the workbook or that can work with sheets by position rather than by name.

**Key distinction from SHEETS:** SHEET (singular) returns the position number of one specific sheet. SHEETS (plural) returns the total count of sheets in the workbook or within a 3D reference. They're complementary functions—SHEET tells you "which position is this sheet?" while SHEETS tells you "how many sheets are there?"

**Practical note:** Sheet numbers change when you reorder tabs. If Sheet3 is moved to the leftmost position, it becomes sheet 1. Formulas using SHEET will reflect these changes, which can be either useful (automatic adaptation) or problematic (unexpected formula behavior). Understanding this dynamic is essential for robust workbook design.

## Syntax

> [!f(x)] SHEET Syntax
>
> ```
> =SHEET([value])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | No | A reference to a sheet or a cell on that sheet. Can be a cell reference (Sheet2!A1), a range (Sheet2!A1:B10), or a defined name that refers to a specific sheet. If omitted, returns the sheet number of the sheet containing the formula. |

### Return Value

Returns a positive integer representing the sheet's position in the workbook. The leftmost visible sheet is 1, the next is 2, etc. Hidden sheets are counted in the numbering. Returns #REF! error if the referenced sheet doesn't exist.

## Examples

> [!f(x)] SHEET Examples

### Example 1: Get Current Sheet Number
```
=SHEET()
```
**Result:** 3 (if this formula is on the third sheet)

**Explanation:** Without arguments, SHEET returns the position of the sheet containing the formula. If you're on the third tab from the left, it returns 3. Simple and commonly used for self-aware formulas.

---

### Example 2: Get Another Sheet's Number
```
=SHEET(DataSheet!A1)
```
**Result:** 2 (if DataSheet is the second sheet)

**Explanation:** Returns the position number of the sheet referenced. The specific cell (A1) doesn't matter—SHEET only cares about which sheet is being referenced.

---

### Example 3: Using Sheet Name as Text (Excel 365)
```
=SHEET("Summary")
```
**Result:** 1 (if Summary is the first sheet)

**Explanation:** In newer Excel versions, you can pass a sheet name as text. This is useful when you have the sheet name in a variable or want to avoid creating an actual cell reference.

---

### Example 4: Check if Current Sheet is First
```
=IF(SHEET()=1, "This is the first sheet", "Not the first sheet")
```
**Result:** Contextual message based on sheet position

**Explanation:** Compare SHEET() to specific values to determine the current sheet's relative position. Useful for conditional logic based on workbook structure.

---

### Example 5: Calculate Sheets Before Current
```
=SHEET()-1
```
**Result:** Number of sheets to the left of the current sheet

**Explanation:** Simple arithmetic with SHEET. If you're on sheet 5, this returns 4—the count of sheets before this one.

---

### Example 6: Dynamic Sheet Navigation
```
=INDIRECT("Sheet"&(SHEET()+1)&"!A1")
```
**Result:** Value from cell A1 on the next sheet

**Explanation:** Uses SHEET() to build a dynamic reference to an adjacent sheet. If sheets are named "Sheet1", "Sheet2", etc., this references the next sequential sheet's A1 cell.

---

### Example 7: Reference Previous Sheet
```
=IF(SHEET()>1, INDIRECT("Sheet"&(SHEET()-1)&"!B5"), "No previous sheet")
```
**Result:** Value from B5 on the previous sheet, or message if on first sheet

**Explanation:** Checks if there's a previous sheet (position > 1) before attempting to reference it. Prevents #REF! errors on the first sheet.

---

### Example 8: Validate Sheet Position
```
=IF(SHEET(DataEntry!A1)=SHEETS()-1, "Second to last", "Other position")
```
**Result:** Position classification based on SHEET relative to total SHEETS

**Explanation:** Combines SHEET with SHEETS to understand relative position. Useful for templates where specific sheets should be in specific positions.

---

### Example 9: Sheet Number in Documentation
```
="This is sheet " & SHEET() & " of " & SHEETS()
```
**Result:** "This is sheet 3 of 12" (example)

**Explanation:** Creates a readable position indicator combining SHEET and SHEETS. Useful for report headers, navigation aids, and user orientation.

---

### Example 10: Conditional Formatting Based on Position
```
=SHEET()>5
```
Use as conditional formatting formula
**Result:** TRUE for sheets 6 and higher

**Explanation:** SHEET() in conditional formatting formulas enables position-aware formatting. Different visual styles for different sections of a multi-sheet workbook.

---

### Example 11: With Named Range
```
=SHEET(SalesData)
```
Where SalesData is a named range on Sheet4
**Result:** 4

**Explanation:** SHEET works with named ranges, returning the sheet number where that named range is defined. Useful for locating named ranges programmatically.

---

### Example 12: Array of Sheet Numbers for 3D Reference
```
=SHEET(Sheet1:Sheet5!A1)
```
**Result:** 1 (returns the first sheet in the range)

**Explanation:** When given a 3D reference, SHEET returns the number of the first sheet in that range. Use SHEETS for the count of sheets in a 3D reference.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Referenced sheet doesn't exist | Verify the sheet name exists and is spelled correctly. May occur after sheet deletion. |
| `#VALUE!` | Invalid sheet name text (in versions supporting text) | Ensure the text string exactly matches an existing sheet name. |
| `#NAME?` | Using sheet name without proper syntax | Use either a cell reference (Sheet1!A1) or text in quotes ("Sheet1") depending on your Excel version. |
| `Unexpected number` | Hidden sheets are counted | Remember that hidden (but not very hidden) sheets are included in the numbering. Sheet 3 might not be the third visible tab. |
| `Number changed` | Sheets were reordered | SHEET returns dynamic position. Reordering tabs changes the result. This is expected behavior. |

## Use Cases

### [[Dynamic Cross-Sheet Navigation]]

**Scenario:** Create formulas that automatically reference adjacent sheets for sequential data processing.

**Implementation:**
```
Reference next sheet's same cell:
=INDIRECT("Sheet"&(SHEET()+1)&"!"&ADDRESS(ROW(),COLUMN()))

Reference previous sheet's same cell:
=IF(SHEET()>1, INDIRECT("Sheet"&(SHEET()-1)&"!"&ADDRESS(ROW(),COLUMN())), 0)
```

**Business Application:** Monthly data sheets where each month references the previous month for comparisons. Running totals across sequential time periods. Step-by-step workflows where each sheet builds on the previous.

**Technical Details:** This pattern assumes consistent sheet naming (Sheet1, Sheet2, etc.). For custom-named sheets, use GET.WORKBOOK to get the actual names. INDIRECT makes the reference volatile—consider performance for large workbooks.

---

### [[Position-Aware Reporting]]

**Scenario:** Generate reports that include their position information for multi-sheet report packages.

**Implementation:**
```
Header: ="Report Section " & SHEET()-1 & " of " & SHEETS()-2
```
(Assuming first and last sheets are TOC and summary)

**Business Application:** Multi-section reports where each section is a sheet. Board packages, financial statements, or compliance reports with multiple distinct sections.

**Technical Details:** Subtract fixed sheets (like a table of contents or summary) from SHEET() and SHEETS() to show meaningful section numbers rather than raw sheet positions.

---

### [[Sheet Order Validation]]

**Scenario:** Ensure sheets are in the expected order for proper workbook functioning.

**Implementation:**
```
Validation formula:
=IF(AND(SHEET(Config!A1)=1, SHEET(Data!A1)=2, SHEET(Output!A1)=SHEETS()),
    "Structure OK",
    "ERROR: Sheets are out of order")
```

**Business Application:** Template integrity checking. Automated workbook validation. Preventing errors from accidental sheet reordering.

**Technical Details:** Define expected positions for critical sheets. Config might always need to be first, output last. This formula validates the structure matches expectations.

---

### [[Smart 3D Reference Boundaries]]

**Scenario:** Create 3D references that automatically adjust based on sheet positions.

**Implementation:**
```
If data sheets are 2 through second-to-last:
=SUM(INDIRECT("Sheet"&2&":Sheet"&(SHEETS()-1)&"!A1:A10"))
```

**Business Application:** Summary sheets that aggregate data from all data sheets regardless of how many exist. New data sheets automatically included in summaries.

**Technical Details:** Build 3D references dynamically using SHEET and SHEETS to define boundaries. This avoids hardcoding sheet names and adapts as sheets are added or removed.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2013 and later (not available in Excel 2010 or earlier)
- **Text argument:** Excel 365 and 2019 allow sheet name as text string
- **Hidden sheets:** Included in counting (hidden sheets still have position numbers)
- **Very hidden sheets:** Also included in counting
- **3D references:** Returns first sheet number in range
- **Defined names:** Correctly returns sheet where name is defined

### Google Sheets

- **Availability:** Fully supported
- **Text argument:** Does not support sheet name as text; must use cell reference
- **Syntax:** =SHEET(SheetName!A1) where SheetName is an actual sheet
- **Hidden sheets:** Included in numbering like Excel
- **Behavior:** Generally identical to Excel for common use cases

### Key Difference Alert

The main difference is text argument support. In Excel 365, you can write `=SHEET("DataSheet")` with the sheet name as a text string. In Google Sheets and older Excel versions, you must reference a cell on that sheet: `=SHEET(DataSheet!A1)`. This affects how you can use SHEET with dynamic sheet names.

## Tips and Best Practices

1. **Remember SHEET is dynamic:** Sheet positions change when you reorder tabs. Formulas using SHEET will automatically update, which may or may not be what you want.

2. **Hidden sheets count:** A sheet being hidden doesn't remove it from the count. Sheet number 3 might be hidden, making visible sheet 3 actually position 4.

3. **Use with SHEETS for relative positioning:** `=SHEET()/SHEETS()` gives a percentage through the workbook. `=SHEETS()-SHEET()` gives sheets remaining.

4. **Pair with INDIRECT for dynamic references:** SHEET() + INDIRECT lets you build references to adjacent or specific-position sheets without hardcoding names.

5. **Don't confuse with SHEETS:** SHEET (singular) = position of ONE sheet. SHEETS (plural) = COUNT of sheets. They're complementary, not interchangeable.

6. **Consider for navigation formulas:** SHEET() is perfect for "Next Sheet" / "Previous Sheet" navigation buttons or formulas that need positional awareness.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SHEETS]] | Returns count of sheets in workbook or 3D reference | When you need the total number of sheets, not a specific position |
| [[CELL]] | Returns cell properties including filename with sheet | When you need the sheet name as text (with filename parsing) |
| [[GET.WORKBOOK]] | XLM function returning sheet names array | When you need actual sheet names, not just position numbers |
| [[ROW]] / [[COLUMN]] | Returns row/column number | Analogous position functions for cells rather than sheets |

### Commonly Used Together

**[[SHEETS]]** - Total sheet count

*Combined with SHEET for relative position:*
```
="Sheet " & SHEET() & " of " & SHEETS()
```
Create position indicators showing current vs. total.

---

**[[INDIRECT]]** - Build dynamic references

*Combined with SHEET for adjacent sheet references:*
```
=INDIRECT("Sheet" & (SHEET()+1) & "!A1")
```
Reference cells on sheets by calculated position.

---

**[[IF]]** - Conditional logic based on position

*Combined with SHEET for position-aware behavior:*
```
=IF(SHEET()=1, "First sheet actions", "Other sheet actions")
```
Different formula behavior based on sheet position.

---

**[[ADDRESS]]** - Build cell addresses

*Combined with SHEET for fully dynamic references:*
```
=INDIRECT("Sheet"&SHEET()&"!"&ADDRESS(5,3))
```
Build complete cell references dynamically.

---

**[[ROW]] / [[COLUMN]]** - Current cell position

*Combined with SHEET for 3D positioning:*
```
="Position: Sheet "&SHEET()&", Row "&ROW()&", Column "&COLUMN()
```
Full 3D position information for any cell.

## Official Documentation

- **Microsoft Excel:** [SHEET function](https://support.microsoft.com/en-us/office/sheet-function-44718b6f-8b87-47a1-a9d6-b701c06cff24)
- **Google Sheets:** [SHEET function](https://support.google.com/docs/answer/3093056)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 | Original introduction | New function in Excel 2013 |
| Excel 2016+ | All subsequent | No changes to functionality |
| Excel 365 | Current | Added support for text sheet names as argument |
| Google Sheets | Original launch | Available since early Sheets versions |

---

*Last updated: 2026-01-10*
