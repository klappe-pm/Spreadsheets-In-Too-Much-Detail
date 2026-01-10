---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- information-functions
- cell-properties
subTopics:
- cell-attributes
- formatting-info
- metadata-extraction
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cell Information
- Cell Properties
- Cell Metadata
tags:
- excel-only
- information
- cell-properties
- metadata
---

# CELL

## Description

**CELL** returns information about the formatting, location, or contents of a cell. It is a metadata function that extracts properties you normally cannot access with formulas - things like the cell's address, column width, file path, number format, or protection status. When you need to know about a cell rather than its value, CELL is your function.

The function takes an info_type argument specifying what property to retrieve. There are over a dozen info_types covering address information, formatting codes, file locations, and value types. CELL can reference any single cell on any worksheet (though some info_types only work on the active sheet). When used without a reference, it returns information about the last-changed cell.

**Critical gotcha:** CELL does not automatically recalculate when formatting changes. If you change a cell's number format, the CELL formula referencing that cell will NOT update until you trigger a recalculation (Ctrl+Alt+F9 for full recalc). This catches many users off guard. Also, some info_types return different results on Mac versus Windows Excel.

**Platform note:** CELL is technically available in both Excel and Google Sheets, but Google Sheets only supports a subset of info_types ("address", "col", "contents", "row", "type"). Most useful info_types like "format", "filename", "prefix", and "width" are Excel-only. This documentation focuses on the full Excel implementation.

## Syntax

> [!f(x)] CELL Syntax
>
> ```
> =CELL(info_type, [reference])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `info_type` | Yes | A text value specifying what type of cell information you want. Must be one of the predefined info_type strings (see table below). |
| `reference` | No | The cell you want information about. If omitted, returns info about the last changed cell. If a range is given, uses the upper-left cell only. |

### Info_Type Values

| Info_Type | Returns |
|-----------|---------|
| `"address"` | Cell reference as absolute text (e.g., "$A$1") |
| `"col"` | Column number of the cell |
| `"color"` | 1 if cell is formatted with negative numbers in color, 0 otherwise |
| `"contents"` | Value contained in the upper-left cell of reference |
| `"filename"` | Full path and filename including sheet name; empty string if unsaved |
| `"format"` | Text code indicating number format (see format codes below) |
| `"parentheses"` | 1 if formatted with parentheses for positive/all values, 0 otherwise |
| `"prefix"` | Label prefix: ' (left), " (right), ^ (center), \ (fill), empty if not text |
| `"protect"` | 1 if cell is locked, 0 if unlocked |
| `"row"` | Row number of the cell |
| `"type"` | "b" if empty (blank), "l" if text (label), "v" if anything else (value) |
| `"width"` | Column width rounded to integer |

### Return Value

Returns a number or text string depending on the info_type requested. The specific return value format varies by info_type.

## Examples

> [!f(x)] CELL Examples

### Example 1: Get Cell Address
```
=CELL("address", B5)
```
**Result:** `$B$5`

**Explanation:** Returns the absolute reference as a text string. Useful when you need to construct formulas dynamically or create audit trails.

---

### Example 2: Get Column Number
```
=CELL("col", D10)
```
**Result:** `4`

**Explanation:** Returns the column number (D is the 4th column). Equivalent to COLUMN(D10) but using the CELL function.

---

### Example 3: Get Row Number
```
=CELL("row", D10)
```
**Result:** `10`

**Explanation:** Returns the row number. Equivalent to ROW(D10). These are included in CELL for completeness of cell metadata.

---

### Example 4: Get File Path and Name
```
=CELL("filename", A1)
```
**Result:** `C:\Users\Documents\[Budget.xlsx]Sheet1`

**Explanation:** Returns full path including filename and sheet name in brackets. Returns empty string if workbook hasn't been saved. Invaluable for audit trails and debugging linked workbooks.

---

### Example 5: Check Cell Type
```
=CELL("type", A1)
```
**Result:** `"v"` if A1 contains 100, `"l"` if A1 contains "Hello", `"b"` if A1 is empty

**Explanation:** Returns "b" for blank, "l" for label (text), "v" for value (numbers, dates, formulas returning numbers). Useful for data validation checks.

---

### Example 6: Get Number Format Code
```
=CELL("format", B2)
```
**Result:** `"F2"` (if B2 is formatted as Number with 2 decimals)

**Explanation:** Returns a coded representation of the number format. "F" means fixed decimal, "2" means 2 decimal places. See format code table below for complete list.

---

### Example 7: Check Cell Protection Status
```
=CELL("protect", C5)
```
**Result:** `1` if cell is locked (default), `0` if unlocked

**Explanation:** Tells you if the cell's Locked property is set. Note: cells are locked by default, but locking only takes effect when the worksheet is protected.

---

### Example 8: Get Column Width
```
=CELL("width", A1)
```
**Result:** `8` (default column width rounded to integer)

**Explanation:** Returns column width in character units. Useful for layout calculations or identifying columns that have been resized.

---

### Example 9: Get Text Alignment Prefix
```
=CELL("prefix", A1)
```
**Result:** `'` if left-aligned text, `"` if right-aligned, `^` if centered

**Explanation:** Returns the label prefix character indicating horizontal alignment. Only applies to text values with explicit prefixes (less common in modern Excel).

---

### Example 10: Check Negative Color Formatting
```
=CELL("color", B10)
```
**Result:** `1` if negative numbers appear in color (like red), `0` otherwise

**Explanation:** Detects if the cell's number format includes color formatting for negative values, such as the format "#,##0;[Red]-#,##0".

---

### Example 11: Construct Dynamic Reference
```
=INDIRECT(CELL("address", INDEX(Data, MATCH(Lookup, KeyColumn, 0), 2)))
```
**Result:** Returns the value at the dynamically determined cell

**Explanation:** CELL("address") returns a text reference that INDIRECT can then evaluate. This pattern enables complex dynamic referencing.

---

### Example 12: Extract Just the Sheet Name
```
=MID(CELL("filename", A1), FIND("]", CELL("filename", A1))+1, 100)
```
**Result:** `Sheet1` (just the sheet name extracted from full path)

**Explanation:** The filename result includes the sheet name after "]". This formula extracts just that sheet name portion.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid info_type string | Use exact info_type strings: "address", "col", "format", etc. (case-insensitive) |
| `#NAME?` | Info_type not in quotes | Info_type must be a text string in quotation marks |
| Empty result for "filename" | Workbook not saved | Save the workbook first; unsaved files return empty string |
| Stale format info | Changed format but CELL not updated | Press Ctrl+Alt+F9 to force full recalculation |
| Unexpected "format" code | Unfamiliar with format codes | Refer to format code table; codes are Excel-specific abbreviations |

## Format Codes Returned by CELL("format")

| Code | Meaning |
|------|---------|
| `G` | General format |
| `F0` | 0 decimal places (integer) |
| `F2` | 2 decimal places |
| `,0` | Thousands separator, 0 decimals |
| `,2` | Thousands separator, 2 decimals |
| `C0` | Currency, 0 decimals |
| `C2` | Currency, 2 decimals |
| `P0` | Percentage, 0 decimals |
| `P2` | Percentage, 2 decimals |
| `S2` | Scientific notation, 2 decimals |
| `D1` | Date: d-mmm-yy |
| `D2` | Date: dd-mmm-yy |
| `D3` | Date: d-mmm |
| `D4` | Date: m/d/yy |
| `D5` | Time: h:mm:ss |
| `D6` | Date: m/d/yy h:mm |
| `D7` | Date: mmm-yy |
| `D8` | Time: h:mm |
| `D9` | Time: h:mm:ss AM/PM |

Note: A "-" suffix indicates negative numbers are formatted in color.

## Use Cases

### [[Dynamic File Reference Tracking]]

**Scenario:** Create an audit trail showing which file and sheet contains specific calculations.

**Implementation:**
```
="Source: " & CELL("filename", DataRange)
```

**Business Application:** Financial reports and compliance documents often need to identify data sources. This automatically captures the full path, preventing manual documentation errors.

**Technical Details:** The result includes [filename] and sheet name. Extract just portions using text functions if you need components separately.

---

### [[Conditional Formatting Based on Cell Type]]

**Scenario:** Identify cells that contain unexpected data types for data quality reporting.

**Implementation:**
```
=IF(CELL("type", A1)="l", "Text found - expected number", "OK")
```

**Business Application:** Data imports often mix text and numbers incorrectly. This formula flags cells containing text ("l" = label) where numbers are expected.

**Technical Details:** "l" = label (text), "v" = value (number/formula), "b" = blank. Use in helper columns or with conditional formatting rules.

---

### [[Layout-Aware Formatting]]

**Scenario:** Adjust content based on column width to prevent overflow.

**Implementation:**
```
=IF(CELL("width", A1)>15, LongVersion, SUBSTITUTE(ShortVersion, " ", ""))
```

**Business Application:** Reports printed to PDF or shared via screenshots need to fit available space. This pattern adapts content to column width.

**Technical Details:** Width is in character units (approximately the width of a "0" in the default font). Remember: CELL won't auto-update when column width changes without recalc.

## Platform Differences

### Microsoft Excel (Windows)

| Feature | Support |
|---------|---------|
| All info_types | Full support |
| "filename" with path | Full Windows path format |
| "format" codes | Complete set |
| "protect" checking | Full support |

### Microsoft Excel (Mac)

| Feature | Support |
|---------|---------|
| Most info_types | Full support |
| "filename" format | Mac path format (uses colons) |
| "format" codes | Same as Windows |
| Some behaviors | May differ slightly from Windows |

### Google Sheets

| Feature | Support |
|---------|---------|
| "address" | Supported |
| "col" | Supported |
| "contents" | Supported |
| "row" | Supported |
| "type" | Supported (uses "t" for text instead of "l") |
| "filename" | NOT SUPPORTED |
| "format" | NOT SUPPORTED |
| "protect" | NOT SUPPORTED |
| "width" | NOT SUPPORTED |
| "color" | NOT SUPPORTED |
| "prefix" | NOT SUPPORTED |
| "parentheses" | NOT SUPPORTED |

Google Sheets has severely limited CELL functionality. Only basic positional info_types work.

### Other Platforms

- LibreOffice Calc: Supports most Excel info_types with some variations
- Apple Numbers: CELL function not available

## Tips and Best Practices

1. **Force recalculation after format changes:** CELL("format") won't update automatically. Press Ctrl+Alt+F9 or add a volatile function nearby to force updates.

2. **Save workbooks before using "filename":** Unsaved workbooks return empty strings for filename. Design your formulas to handle this case.

3. **Use absolute references in CELL:** `CELL("address", $A$1)` ensures consistent results even if the formula is copied.

4. **Combine with INDIRECT for dynamic references:** `=INDIRECT(CELL("address", target))` creates text references that can be evaluated.

5. **Remember range limitations:** When you pass a multi-cell range, CELL only examines the top-left cell. It cannot analyze an entire range.

6. **Use for debugging:** CELL provides insights into cell properties that aren't visible at the formula bar. Great for troubleshooting formatting issues.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[INFO]] | Returns system/environment information | For application-level info (not cell-level) |
| [[TYPE]] | Returns numeric type code for a value | When you just need data type, not cell metadata |
| [[ROW]] | Returns row number | Simpler when you only need row number |
| [[COLUMN]] | Returns column number | Simpler when you only need column number |

### Commonly Used Together

**[[INDIRECT]]** - Evaluate text references from CELL

*Create dynamic cell references:*
```
=INDIRECT(CELL("address", TargetCell))
```
Converts CELL's text address output back into a usable reference.

---

**[[ADDRESS]]** - Alternative for building cell references

*More control over reference style:*
```
=ADDRESS(CELL("row", A1), CELL("col", A1), 4)
```
ADDRESS offers more flexibility in reference formatting (relative vs absolute).

---

**[[TYPE]]** - Simpler type checking

*Compare approaches:*
```
CELL("type", A1) returns "l", "v", or "b"
TYPE(A1) returns 1 (number), 2 (text), 4 (logical), 16 (error), 64 (array)
```
TYPE gives more granular type information but doesn't check for blanks.

---

**[[ISBLANK]] / [[ISTEXT]] / [[ISNUMBER]]** - Clearer type testing

*More readable type checks:*
```
=ISTEXT(A1) instead of =CELL("type",A1)="l"
```
IS functions are clearer for simple type checking.

## Official Documentation

- **Microsoft Excel:** [CELL function](https://support.microsoft.com/en-us/office/cell-function-51bd39a5-f338-4dbe-a33f-b43c76a0c6ef)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 | Original function from earliest Excel versions |
| Excel 2007+ | Continued support | No significant changes |
| Excel 365 | Current | Full support maintained |
| Google Sheets | Limited | Only subset of info_types supported |

---

*Last updated: 2026-01-10*
