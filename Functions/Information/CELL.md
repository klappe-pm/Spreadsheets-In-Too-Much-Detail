---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- information
subTopics:
- cell-properties
- metadata
- formatting-info
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cell Information
- Cell Properties
- Cell Metadata
tags:
- information
- cell-properties
- metadata
- formatting
- location
---

# CELL

## Description

**CELL** retrieves information about a cell's properties, including its formatting, location, contents, and protection status. Think of it as asking Excel or Google Sheets to describe a cell rather than just read its value. You specify what information you want (address, column number, format type, etc.) and optionally which cell to examine, and CELL returns that specific property.

This function is invaluable for building dynamic formulas that adapt based on where they are or what's in neighboring cells. Need to know if a cell is formatted as currency? CELL can tell you. Want to extract the filename from a cell's full path? CELL handles that. Building a formula that needs to know its own row number or column position? CELL provides the answer. It's the introspection function for spreadsheets.

**Critical limitation:** CELL doesn't automatically update when formatting changes. If you change a cell's format, CELL might still show the old info_type value until you force recalculation (F9 in Excel, or editing any cell). This catches many users off guard. Additionally, the info_type parameter must be a text string exactly matching one of the predefined values—no abbreviations, no variations.

**Platform warning:** Google Sheets supports only a subset of CELL's info_type options. Excel offers about 12 different info types; Sheets provides only 6. If you're building cross-platform spreadsheets, stick to the common subset: "address", "col", "contents", "row", and "type". The formatting-related options like "format", "color", and "prefix" are Excel-only.

## Syntax

> [!f(x)] CELL Syntax
>
> ```
> =CELL(info_type, [reference])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `info_type` | Yes | A text string specifying what information to return. Must be one of the predefined values like "address", "col", "row", "type", "format", "filename", etc. Case-insensitive in most locales. |
| `reference` | No | The cell you want information about. If omitted, returns info about the cell where the formula is entered (behavior may vary). Only the upper-left cell is examined if a range is provided. |

### Info Type Values

| Info_type | Returns | Excel | Sheets |
|-----------|---------|-------|--------|
| `"address"` | Cell reference as text (e.g., "$A$1") | Yes | Yes |
| `"col"` | Column number of the cell | Yes | Yes |
| `"color"` | 1 if cell has negative number color formatting, 0 otherwise | Yes | No |
| `"contents"` | Value in the upper-left cell | Yes | Yes |
| `"filename"` | Full path and filename as text (empty if not saved) | Yes | No |
| `"format"` | Text code for cell's number format | Yes | No |
| `"parentheses"` | 1 if formatted with parentheses for positives, 0 otherwise | Yes | No |
| `"prefix"` | Label prefix character (' for left, " for right, ^ for center) | Yes | Yes (limited) |
| `"protect"` | 1 if cell is locked, 0 if unlocked | Yes | No |
| `"row"` | Row number of the cell | Yes | Yes |
| `"type"` | "b" for blank, "l" for label (text), "v" for value (number) | Yes | Yes |
| `"width"` | Column width rounded to integer | Yes | No |

### Return Value

Returns the requested property value. The type depends on info_type: text for "address", "filename", "format", "prefix"; number for "col", "row", "width", "color", "protect", "parentheses"; mixed for "contents" and "type".

## Examples

> [!f(x)] CELL Examples

### Example 1: Get Cell's Row Number
```
=CELL("row", B5)
```
**Result:** 5

**Explanation:** Returns the row number of cell B5. This is functionally similar to ROW(B5), but CELL provides a unified interface when you need multiple properties. Useful when you're already using CELL for other info and want consistency.

---

### Example 2: Get Cell's Column Number
```
=CELL("col", D10)
```
**Result:** 4

**Explanation:** Returns the column number of D10 (D is the 4th column). Again, similar to COLUMN(D10), but through the CELL interface. The column is returned as a number, not a letter.

---

### Example 3: Get Cell's Address as Text
```
=CELL("address", Sheet2!C15)
```
**Result:** "$C$15"

**Explanation:** Returns the cell reference as an absolute address text string. Note that it returns just the cell address without the sheet name, even when referencing another sheet. Useful for building dynamic references or logging formulas.

---

### Example 4: Determine Cell Content Type
```
=CELL("type", A1)
```
**Result:** "v" if A1 contains a number, "l" if text, "b" if blank

**Explanation:** Returns a single character indicating the data type: "v" for value (numbers, including dates and times which are numeric), "l" for label (text), "b" for blank. This is useful for data validation and conditional processing.

---

### Example 5: Get Cell's Contents
```
=CELL("contents", B2)
```
**Result:** Whatever value is in B2

**Explanation:** Returns the actual value of the cell, similar to simply referencing B2. The difference is subtle—CELL("contents") always returns the value, while a direct reference might return the formula in certain contexts. Rarely needed, but available.

---

### Example 6: Extract Filename from Path (Excel)
```
=CELL("filename", A1)
```
**Result:** "C:\Users\Name\Documents\[Budget.xlsx]Sheet1" (or empty if unsaved)

**Explanation:** Returns the full path, filename, and sheet name. Excel-only feature. Returns empty string if workbook hasn't been saved. Commonly used in templates that need to display or log their own filename.

---

### Example 7: Check Number Format Type (Excel)
```
=CELL("format", A1)
```
**Result:** "F2" for fixed 2 decimals, "C2" for currency 2 decimals, "G" for general, etc.

**Explanation:** Returns a code representing the cell's number format. "G" = General, "F0"-"F15" = Fixed decimals, "C0"-"C15" = Currency, "D1"-"D9" = Date formats, "S0"-"S4" = Scientific. Excel-only; invaluable for format-dependent logic.

---

### Example 8: Get Column Width (Excel)
```
=CELL("width", C1)
```
**Result:** 12 (or whatever the column width is)

**Explanation:** Returns the column width rounded to the nearest integer. The unit is the width of one character in the default font. Useful for reports that need to know layout dimensions. Excel-only feature.

---

### Example 9: Check Cell Protection Status (Excel)
```
=CELL("protect", A1)
```
**Result:** 1 if locked, 0 if unlocked

**Explanation:** Returns whether the cell is locked (will be protected when sheet protection is enabled). Note: This shows the lock property, not whether the sheet is currently protected. Excel-only.

---

### Example 10: Extract Sheet Name from Filename (Excel)
```
=MID(CELL("filename",A1),FIND("]",CELL("filename",A1))+1,255)
```
**Result:** "Sheet1" (or current sheet name)

**Explanation:** Classic formula pattern to extract just the sheet name from CELL's filename output. Uses FIND to locate the "]" character that precedes the sheet name, then MID to extract everything after it. Essential for templates that reference their own sheet name.

---

### Example 11: Build Dynamic Cell Reference
```
=INDIRECT("B" & CELL("row", A1))
```
**Result:** Value from column B in the same row as A1

**Explanation:** Combines CELL with INDIRECT to create dynamic references. Gets the row number from A1's position, then references column B in that row. Useful for formulas that need to "look sideways" at other columns dynamically.

---

### Example 12: Conditional Formatting Based on Cell Type
```
=IF(CELL("type",A1)="b", "Empty", IF(CELL("type",A1)="l", "Text", "Number"))
```
**Result:** Descriptive label of what's in A1

**Explanation:** Uses CELL's type detection to categorize cell contents. Returns human-readable labels instead of the cryptic b/l/v codes. Useful for data validation reports and user feedback.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid info_type string | Check spelling; use exact text like "row", "col", "address". Common mistake: "column" instead of "col". |
| `#NAME?` | Missing quotes around info_type | info_type must be a text string in quotes: CELL("row",A1) not CELL(row,A1). |
| `#REF!` | Reference points to deleted cells | Verify the referenced cell/range still exists. |
| `Empty result` | Using "filename" on unsaved workbook | Save the workbook first, then CELL("filename") will return the path. |
| `Stale value` | Format changed but CELL shows old format | Press F9 to force recalculation, or edit any cell to trigger update. |
| `#N/A or error` | Using Excel-only info_type in Google Sheets | Sheets only supports: address, col, contents, prefix, row, type. Avoid: color, filename, format, parentheses, protect, width. |

## Use Cases

### [[Dynamic Header Detection]]

**Scenario:** Automatically identify which row contains headers based on cell formatting or content type.

**Implementation:**
```
=IF(CELL("type",A1)="l", "Header Row", "Data Row")
```

**Business Application:** Template design where header position might vary. Automatic detection for import processes where data structure isn't fixed.

**Technical Details:** Assumes headers are text ("l") and data rows contain numbers ("v"). May need adjustment for text-heavy datasets. Combine with MATCH or scanning logic for robust detection.

---

### [[Self-Documenting Templates (Excel)]]

**Scenario:** Display the current filename and sheet name in a report footer that updates automatically.

**Implementation:**
```
=IFERROR(MID(CELL("filename",A1),FIND("[",CELL("filename",A1))+1,FIND("]",CELL("filename",A1))-FIND("[",CELL("filename",A1))-1),"Unsaved Document")
```

**Business Application:** Professional templates for invoices, reports, and forms that automatically display their filename for audit trails and version tracking.

**Technical Details:** Extracts text between "[" and "]" which contains just the filename without path or sheet. IFERROR handles unsaved workbooks gracefully.

---

### [[Format-Aware Calculations (Excel)]]

**Scenario:** Apply different logic based on whether a cell is formatted as currency, percentage, or general number.

**Implementation:**
```
=IF(LEFT(CELL("format",B1),1)="C", B1, IF(LEFT(CELL("format",B1),1)="P", B1*100, B1))
```

**Business Application:** Import validation that behaves differently based on source formatting. Currency fields might need different rounding than percentages.

**Technical Details:** CELL("format") returns codes starting with C for currency, P for percentage, F for fixed, etc. Parse the first character to determine format category.

---

### [[Row-Relative Formulas]]

**Scenario:** Create formulas that reference other cells relative to their own position without using ROW() directly.

**Implementation:**
```
=OFFSET(A1,CELL("row",A1)-1,0)
```

**Business Application:** Complex formula templates where understanding the formula's own position is needed for calculations. Cross-referencing data in parallel structures.

**Technical Details:** CELL("row") returns the row number of the referenced cell. Subtract 1 (since rows are 1-indexed) for use with OFFSET's row parameter.

## Platform Differences

### Microsoft Excel

- **Full info_type support:** All 12 info_type values work (address, col, color, contents, filename, format, parentheses, prefix, protect, row, type, width)
- **Filename returns path:** Full path including drive letter, folders, [filename], and sheet name
- **Format codes:** Returns detailed format codes (F2, C0-D9, etc.) for number format detection
- **Volatile behavior:** CELL is volatile in older Excel versions (recalculates with every change); semi-volatile in newer versions
- **Localization:** info_type strings may need translation in non-English Excel versions

### Google Sheets

- **Limited info_type support:** Only 6 values work (address, col, contents, prefix, row, type)
- **No filename access:** Cannot retrieve file or sheet names through CELL
- **No format detection:** Cannot determine cell's number format
- **No protection info:** Cannot check lock status
- **No column width:** Cannot retrieve column dimensions
- **English only:** info_type strings are English regardless of locale

### Key Difference Alert

The biggest practical limitation is Google Sheets' lack of "filename" support. Excel templates commonly use CELL("filename") for self-documenting reports, audit trails, and cross-sheet references. In Sheets, you need alternative approaches (Apps Script, manual entry, or IMPORTRANGE tricks) for similar functionality.

## Tips and Best Practices

1. **Check platform compatibility first:** Before building CELL-dependent formulas, verify your info_type works on all target platforms. Stick to address, col, row, type, and contents for cross-platform safety.

2. **Force recalculation after format changes:** CELL doesn't auto-update when formatting changes. Press Ctrl+Alt+F9 (full recalculation) in Excel or edit any cell to force update.

3. **Use for the upper-left cell only:** When passing a range to CELL, only the upper-left cell is examined. CELL("type",A1:D10) returns info about A1 only.

4. **Combine with INDIRECT for dynamic references:** CELL("address") returns text that can feed into INDIRECT for powerful dynamic formulas.

5. **Handle unsaved files gracefully:** CELL("filename") returns empty string on unsaved files. Always wrap in IFERROR or IF to provide fallback text.

6. **Consider ROW() and COLUMN() for simple cases:** If you just need row/column numbers, ROW() and COLUMN() are more readable and clearly convey intent. Use CELL when you need multiple properties or for consistency in complex formulas.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ROW]] | Returns row number of a reference | When you only need the row number; simpler and clearer |
| [[COLUMN]] | Returns column number of a reference | When you only need the column number |
| [[ADDRESS]] | Creates cell address from row/column numbers | When building addresses from numbers rather than extracting from existing cells |
| [[TYPE]] | Returns number indicating value type | When you need numeric type codes (1=number, 2=text, etc.) |
| [[INFO]] | Returns system environment information | When you need OS, Excel version, or directory info rather than cell info |
| [[GET.CELL]] | Macro function with extended cell info | When you need properties not available in CELL (Excel macro sheets only) |

### Commonly Used Together

**[[INDIRECT]]** - Convert text address to cell reference

*Combined with CELL for dynamic referencing:*
```
=INDIRECT(CELL("address",A1))
```
Get A1's address as text, then convert back to reference. Foundation for many dynamic formula patterns.

---

**[[MID]] / [[FIND]]** - Text extraction

*Combined with CELL for parsing filename:*
```
=MID(CELL("filename",A1),FIND("]",CELL("filename",A1))+1,31)
```
Extract sheet name from CELL's filename output.

---

**[[IF]] / [[IFS]]** - Conditional logic

*Combined with CELL for format-dependent logic:*
```
=IF(CELL("type",A1)="v", A1*2, "Not numeric")
```
Apply different operations based on cell content type.

---

**[[ROW]] / [[COLUMN]]** - Row and column references

*Alternative to CELL for simple cases:*
```
=ROW(A5)  vs  =CELL("row",A5)
```
Both return 5; ROW is clearer for simple row extraction.

---

**[[IFERROR]]** - Error handling

*Combined with CELL for robust formulas:*
```
=IFERROR(CELL("filename",A1),"File not saved")
```
Provides user-friendly fallback when CELL would return empty or error.

## Official Documentation

- **Microsoft Excel:** [CELL function](https://support.microsoft.com/en-us/office/cell-function-51bd39a5-f338-4dbe-a33f-955d67c2b2cf)
- **Google Sheets:** [CELL function](https://support.google.com/docs/answer/3093366)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function with full info_type support |
| Excel 2007+ | All subsequent | No changes to core functionality |
| Excel 365 | Current | Behavior unchanged; still semi-volatile |
| Google Sheets | Original launch | Limited subset of info_type values supported |

---

*Last updated: 2026-01-10*
