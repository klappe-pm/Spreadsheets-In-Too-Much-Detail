---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- information
subTopics:
- cell-properties
- formatting-detection
- macro-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Get Cell
- XLM Cell Function
- Macro Cell Information
tags:
- information
- cell-properties
- formatting
- xlm-macro
- legacy
- excel-only
---

# GET.CELL

## Description

**GET.CELL** is an Excel 4.0 macro function (XLM macro function) that retrieves detailed information about a cell's properties, formatting, and attributes. It provides access to over 60 different cell properties that the standard CELL function cannot access—including font color, background color, cell formatting, protection status, and much more. Despite being a "legacy" function from 1992, GET.CELL remains invaluable because no modern worksheet function can detect cell formatting colors.

The function cannot be entered directly in worksheet cells. Instead, you must create a named range that references the GET.CELL formula, then use that named range in your worksheet formulas. This workaround is widely used because GET.CELL provides capabilities that Excel's modern worksheet functions still cannot match—most notably, detecting whether a cell is formatted with a specific fill color or font color.

**Why it still matters:** Want to sum all cells with a green background? Count all cells with red text? Identify cells formatted as bold? The CELL function can't do this. No modern worksheet function can. GET.CELL is the only native Excel method (without VBA) to access these formatting properties. That's why this 30+ year old function remains in heavy use today.

**Critical limitations:** GET.CELL only works in Excel—there's no Google Sheets equivalent. It must be used through named ranges, not directly in cells. And perhaps most importantly, it doesn't automatically recalculate when formatting changes. If you change a cell's color, formulas using GET.CELL won't update until you trigger recalculation (edit a cell, press F2+Enter on the GET.CELL source cell, or press Ctrl+Alt+F9).

## Syntax

> [!f(x)] GET.CELL Syntax
>
> ```
> =GET.CELL(type_num, reference)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `type_num` | Yes | A number from 1 to 66 specifying which property to return. Each number corresponds to a specific cell attribute. See the Type Number Reference table below. |
| `reference` | Yes | The cell to examine. Must be a cell reference, not a range. If a range is provided, only the upper-left cell is examined. |

### Type Number Reference (Most Common)

| Type_num | Returns |
|----------|---------|
| 1 | Absolute reference as text (e.g., "$A$1") |
| 2 | Row number |
| 3 | Column number |
| 4 | Cell type: 1=number, 2=text, 3=logical, 4=error, 5=array |
| 5 | Cell contents (value) |
| 6 | Formula as text (if cell contains formula) |
| 7 | Number format code as text |
| 8 | Horizontal alignment: 1=general, 2=left, 3=center, 4=right, 5=fill, 6=justify, 7=center across |
| 9 | Left border style (0-7) |
| 10 | Right border style (0-7) |
| 11 | Top border style (0-7) |
| 12 | Bottom border style (0-7) |
| 13 | Pattern/shade number (0-18) |
| 14 | TRUE if cell is locked |
| 15 | TRUE if formula is hidden |
| 16 | Column width in points |
| 17 | Row height in points |
| 18 | Font name as text |
| 19 | Font size in points |
| 20 | TRUE if bold |
| 21 | TRUE if italic |
| 22 | TRUE if underlined |
| 23 | TRUE if strikethrough |
| 24 | Font color number (1-56 from palette, 0=automatic) |
| 25 | TRUE if font is outlined (Mac only) |
| 26 | TRUE if font is shadowed (Mac only) |
| 38 | Background fill color number (1-56 from palette, 0=no fill) |
| 48 | TRUE if cell contains a formula |
| 49 | TRUE if cell is part of an array |
| 50 | Vertical alignment: 1=top, 2=center, 3=bottom, 4=justify |
| 51 | Cell prefix character (for text alignment compatibility) |
| 52 | Contents as displayed (including formatting) |
| 53 | Pivot table field name (if applicable) |
| 63 | Fill color as RGB value (newer Excel) |
| 64 | Pattern foreground color as RGB value |
| 65 | Pattern background color as RGB value |
| 66 | Font style as text |

### Return Value

Depends on the type_num parameter. Can return text (formulas, names, formats), numbers (colors, sizes, positions), or Boolean values (locked, hidden, bold, italic).

## Examples

> [!f(x)] GET.CELL Examples

### Example 1: Detect Background Color (Named Range Method)
**Step 1:** Define named range
```
Name: CellColor
Refers to: =GET.CELL(38, INDIRECT("RC", FALSE))
```
**Step 2:** Use in worksheet
```
=CellColor
```
**Result:** Returns color index (1-56) of cell's background color

**Explanation:** The most common GET.CELL use case. Type 38 returns the fill color index. INDIRECT("RC",FALSE) makes the reference relative to wherever the formula is placed. You must define this as a named range first (Formulas > Name Manager).

---

### Example 2: Detect Font Color
**Named range definition:**
```
Name: FontColor
Refers to: =GET.CELL(24, INDIRECT("RC", FALSE))
```
**Result:** Returns font color index number (1-56)

**Explanation:** Type 24 returns the font color from Excel's 56-color palette. Returns 0 for automatic/black. Useful for identifying cells with specific text colors like red for negative numbers or green for positive.

---

### Example 3: Check if Cell Contains Formula
**Named range definition:**
```
Name: HasFormula
Refers to: =GET.CELL(48, INDIRECT("RC", FALSE))
```
**Result:** TRUE if cell contains a formula, FALSE otherwise

**Explanation:** Type 48 returns a Boolean indicating whether the cell contains a formula. Modern Excel has ISFORMULA() which does the same thing, but GET.CELL was the original way and works in older versions.

---

### Example 4: Get Cell's Number Format
**Named range definition:**
```
Name: NumberFormat
Refers to: =GET.CELL(7, INDIRECT("RC", FALSE))
```
**Result:** Format code like "General", "#,##0.00", "mm/dd/yyyy"

**Explanation:** Type 7 returns the exact number format code applied to the cell. Useful for format validation or creating format-aware reports.

---

### Example 5: Check if Cell is Bold
**Named range definition:**
```
Name: IsBold
Refers to: =GET.CELL(20, INDIRECT("RC", FALSE))
```
**Result:** TRUE if font is bold

**Explanation:** Type 20 detects bold formatting. Combine with COUNTIF or SUMIF patterns to count or sum only bold cells in a range.

---

### Example 6: Get Column Width
**Named range definition:**
```
Name: ColWidth
Refers to: =GET.CELL(16, INDIRECT("RC", FALSE))
```
**Result:** Column width in points

**Explanation:** Type 16 returns column width. Useful for detecting hidden columns (width = 0) or for layout calculations.

---

### Example 7: Check if Cell is Protected/Locked
**Named range definition:**
```
Name: IsLocked
Refers to: =GET.CELL(14, INDIRECT("RC", FALSE))
```
**Result:** TRUE if cell has locked property set

**Explanation:** Type 14 shows the lock status of the cell. Note: This indicates the lock property, not whether the sheet protection is currently enabled.

---

### Example 8: Get Formula as Text
**Named range definition:**
```
Name: CellFormula
Refers to: =GET.CELL(6, INDIRECT("RC", FALSE))
```
**Result:** The formula in the cell as a text string (e.g., "=A1+B1")

**Explanation:** Type 6 extracts the actual formula. Modern Excel has FORMULATEXT() for this, but GET.CELL provides it as part of its comprehensive toolset.

---

### Example 9: Get Font Name
**Named range definition:**
```
Name: FontName
Refers to: =GET.CELL(18, INDIRECT("RC", FALSE))
```
**Result:** "Calibri", "Arial", or whatever font is applied

**Explanation:** Type 18 returns the font name. Useful for font standardization audits or detecting non-standard fonts in shared workbooks.

---

### Example 10: Check Cell Alignment
**Named range definition:**
```
Name: HAlign
Refers to: =GET.CELL(8, INDIRECT("RC", FALSE))
```
**Result:** 1=general, 2=left, 3=center, 4=right, etc.

**Explanation:** Type 8 returns horizontal alignment as a number. Use with CHOOSE to convert to text: `=CHOOSE(HAlign,"General","Left","Center","Right","Fill","Justify","Center Across")`.

---

### Example 11: Referencing a Specific Cell (Not Relative)
**Named range definition:**
```
Name: A1Color
Refers to: =GET.CELL(38, Sheet1!$A$1)
```
**Result:** Background color of specifically cell A1 on Sheet1

**Explanation:** Use absolute reference instead of INDIRECT("RC",FALSE) when you always want information about a specific cell regardless of where the formula is placed.

---

### Example 12: Sum Cells by Color
**Setup:** Create named range "CellColor" as shown in Example 1
**Formula in worksheet:**
```
=SUMPRODUCT((CellColor=6)*(A1:A100))
```
**Result:** Sum of cells with color index 6 (yellow)

**Explanation:** This classic pattern applies CellColor to each cell in the range and sums only those matching color index 6. Note: You need to enter CellColor in each cell first, or use a helper column approach.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NAME?` | Entering GET.CELL directly in worksheet | GET.CELL cannot be typed in cells. Create a named range using Name Manager that references the GET.CELL formula. |
| `#REF!` | Invalid reference or INDIRECT reference broken | Verify INDIRECT syntax. Use `INDIRECT("RC",FALSE)` for relative R1C1 reference. |
| `#VALUE!` | Invalid type_num | Use a number between 1 and 66. Verify the type_num is appropriate for your Excel version. |
| `Stale results` | Formatting changed but value didn't update | GET.CELL is not volatile. Press Ctrl+Alt+F9 to force full recalculation, or edit the cell to trigger update. |
| `Returns 0` | Checking color of unformatted cell | Color index 0 means automatic/default. Only explicitly colored cells return 1-56. |
| `Different results on Mac` | Some type_num values differ | Types 25, 26 (outlined, shadowed) are Mac-specific. Some RGB-related types may behave differently. |

## Use Cases

### [[Sum Cells by Background Color]]

**Scenario:** Total all expenses that have been highlighted in red to indicate over-budget items.

**Implementation:**
```
Step 1: Create named range "BgColor"
Refers to: =GET.CELL(38, INDIRECT("RC", FALSE))

Step 2: Create helper column (Column C) next to data (Column B)
Cell C1: =BgColor (drag down for all rows)

Step 3: Sum with criteria
=SUMPRODUCT((C1:C100=3)*(B1:B100))
```
Where color index 3 = red

**Business Application:** Visual exception tracking where users highlight problem items. Budget variance reports where over-budget items are colored. Project status dashboards using color coding.

**Technical Details:** Color index 3 is red, 4 is green, 5 is blue, 6 is yellow in the standard palette. The helper column captures the color at the time of entry; it won't auto-update if colors change. Consider VBA for fully dynamic color-based calculations.

---

### [[Count Formatted Items]]

**Scenario:** Count how many cells in a range are formatted as bold to identify emphasized or header rows.

**Implementation:**
```
Named range: IsBold
Refers to: =GET.CELL(20, INDIRECT("RC", FALSE))

Worksheet formula:
=SUMPRODUCT((IsBold)*1)
```
Applied to a range with the IsBold named range

**Business Application:** Template validation to ensure proper formatting. Style guide compliance checking. Identifying header rows in imported data.

**Technical Details:** Type 20 returns TRUE/FALSE for bold. Multiply by 1 or use double-negative (--IsBold) to convert Boolean to number for SUMPRODUCT.

---

### [[Formula Audit Report]]

**Scenario:** Create a report showing which cells contain formulas and what those formulas are.

**Implementation:**
```
Named range: CellFormula
Refers to: =GET.CELL(6, INDIRECT("RC", FALSE))

Named range: HasFormula
Refers to: =GET.CELL(48, INDIRECT("RC", FALSE))

Use CellFormula to display formulas as text
Use HasFormula to filter/count formula cells
```

**Business Application:** Spreadsheet documentation and audit trails. Error checking and formula review. Training materials showing formula construction.

**Technical Details:** Modern Excel has FORMULATEXT() and ISFORMULA() which are easier to use. GET.CELL is useful when you need both formula detection and other GET.CELL properties in the same report.

---

### [[Dynamic Formatting Detection]]

**Scenario:** Create a validation rule that checks if cells follow formatting standards (specific font, size, alignment).

**Implementation:**
```
Named range: FontCheck
Refers to: =AND(GET.CELL(18, INDIRECT("RC",FALSE))="Calibri", GET.CELL(19, INDIRECT("RC",FALSE))=11, GET.CELL(20, INDIRECT("RC",FALSE))=FALSE)

Returns TRUE if: Font is Calibri, Size is 11, Not bold
```

**Business Application:** Enforcing corporate style guides. Template quality assurance. Standardizing shared workbooks.

**Technical Details:** Combine multiple GET.CELL calls with AND/OR logic to create comprehensive formatting rules. Consider creating a dashboard that reports formatting compliance across a workbook.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 4.0 (1992) to current Excel 365
- **Access method:** Must be used through named ranges (Name Manager), not directly in cells
- **Color palette:** Type 24 and 38 return color index 1-56 from legacy palette
- **RGB colors:** Types 63-65 return RGB values in newer Excel versions
- **Volatility:** Non-volatile; doesn't recalculate automatically when formatting changes
- **Mac differences:** Types 25-26 are Mac-specific (outlined/shadowed fonts)

### Google Sheets

- **Not available:** Google Sheets does not support XLM macro functions
- **No equivalent:** There is no Google Sheets function to detect cell formatting colors
- **Alternative:** Use Apps Script (Google's JavaScript-based scripting) to access some cell properties
- **Apps Script example:** `SpreadsheetApp.getActiveCell().getBackground()` returns color

### Key Difference Alert

GET.CELL is completely Excel-specific with no Google Sheets equivalent. This is one of the most significant functionality gaps between the platforms. In Google Sheets, detecting cell colors or formatting requires Apps Script, which is a programming solution rather than a formula approach. This limitation affects any spreadsheet design that relies on color-coded data processing.

## Tips and Best Practices

1. **Always use INDIRECT("RC", FALSE) for relative references:** This makes the named range work correctly relative to wherever it's used, not relative to where it was defined.

2. **Force recalculation after format changes:** GET.CELL doesn't auto-update when formatting changes. Press Ctrl+Alt+F9 for full recalculation, or use a helper column that you manually refresh.

3. **Create a library of named ranges:** Build a set of commonly-used GET.CELL named ranges (CellColor, FontColor, IsBold, etc.) that you can use across workbooks.

4. **Use helper columns for range-based operations:** GET.CELL examines one cell at a time. For SUMIF/COUNTIF by color, create a helper column that captures each cell's color, then use standard functions on that column.

5. **Document your named ranges:** Since GET.CELL formulas are hidden in Name Manager, add comments or create a documentation sheet listing all GET.CELL-based names and their purposes.

6. **Consider VBA for complex color-based calculations:** For large datasets or frequently changing colors, VBA user-defined functions (UDFs) that use the Range.Interior.Color property are more maintainable.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CELL]] | Standard worksheet function for cell info | When you only need address, row, col, type, format (subset of GET.CELL capabilities) |
| [[ISFORMULA]] | Detects if cell contains formula | Modern Excel (2013+); easier than GET.CELL type 48 |
| [[FORMULATEXT]] | Returns formula as text | Modern Excel (2013+); easier than GET.CELL type 6 |
| [[ROW]] / [[COLUMN]] | Returns row/column number | Simpler than GET.CELL types 2 and 3 for basic needs |
| [[INFO]] | System/environment information | When you need OS, Excel version info rather than cell properties |

### Commonly Used Together

**[[INDIRECT]]** - Create dynamic cell references

*Combined with GET.CELL for relative references:*
```
=GET.CELL(38, INDIRECT("RC", FALSE))
```
INDIRECT("RC", FALSE) creates a relative R1C1 reference that moves with the formula.

---

**[[SUMPRODUCT]]** - Conditional calculations with arrays

*Combined with GET.CELL-based named ranges:*
```
=SUMPRODUCT((CellColor=6)*(Values))
```
Sum values where CellColor named range returns 6 (yellow).

---

**[[CHOOSE]]** - Convert numeric codes to text

*Combined with GET.CELL alignment results:*
```
=CHOOSE(GET.CELL(8,A1),"General","Left","Center","Right","Fill","Justify","Center Across")
```
Convert alignment number to readable text.

---

**[[IF]]** - Conditional logic based on formatting

*Combined with GET.CELL for format-based decisions:*
```
=IF(IsBold, "Header", "Data")
```
Classify cells based on bold formatting.

---

**[[AND]] / [[OR]]** - Combine multiple formatting checks

*Combined with GET.CELL for complex format validation:*
```
=AND(FontName="Arial", FontSize=10, NOT(IsBold))
```
Verify cell meets all formatting criteria.

## Official Documentation

- **Microsoft Excel:** [XL: List of Macro Functions](https://support.microsoft.com/en-us/office/xl-list-of-macro-functions-that-work-with-name-create-bb53aace-a98c-4c24-9e2f-24c63eb06d09)
- **Microsoft Support:** [GET.CELL macro function](https://support.microsoft.com/en-us/topic/xl-the-get-cell-macro-function-works-for-only-1-cell-at-a-time-83ffdeab-14ff-0d02-a6ed-efe3f08b15e9)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 4.0 | 1992 | Introduced as XLM macro function |
| Excel 5.0+ | 1993+ | Continued support, though XLM deprecated in favor of VBA |
| Excel 2007+ | All versions | Still works through named ranges despite being "legacy" |
| Excel 365 | Current | Full support; no changes to functionality |
| Google Sheets | Not available | No support for XLM functions |

---

*Last updated: 2026-01-10*
