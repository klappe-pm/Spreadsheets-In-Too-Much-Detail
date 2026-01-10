---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- information
subTopics:
- workbook-properties
- sheet-names
- macro-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Get Workbook
- XLM Workbook Function
- Sheet Names Function
tags:
- information
- workbook-properties
- sheet-names
- xlm-macro
- legacy
- excel-only
---

# GET.WORKBOOK

## Description

**GET.WORKBOOK** is an Excel 4.0 macro function (XLM macro function) that retrieves information about workbooks and their sheets. Its most valuable capability is returning a list of all sheet names in a workbook—something no standard worksheet function can do natively. Like GET.CELL, it cannot be entered directly in cells but must be accessed through named ranges, making it a powerful but somewhat hidden Excel feature.

The most common use of GET.WORKBOOK is with type_num 1, which returns an array of all sheet names in the workbook. This seemingly simple capability solves a common problem: creating dynamic references to sheets, building navigation menus, or validating sheet names—all without VBA. The sheet names are returned with the workbook name prefix (e.g., "[Budget.xlsx]Sheet1"), which requires parsing but provides unambiguous identification.

**Why it matters:** Before GET.WORKBOOK, the only way to get a list of sheet names was through VBA code. GET.WORKBOOK offers a formula-based approach that's easier to maintain and doesn't require macro-enabled workbooks. For building table of contents pages, dropdown lists of sheets, or formulas that reference sheets dynamically, GET.WORKBOOK is the key ingredient.

**Critical limitations:** Like all XLM functions, GET.WORKBOOK only works in Excel—no Google Sheets equivalent exists. It must be used through named ranges defined in Name Manager. The array it returns requires INDEX to extract individual elements, and it updates automatically when sheets are added, deleted, or renamed, though formulas referencing it may need manual refresh in some scenarios.

## Syntax

> [!f(x)] GET.WORKBOOK Syntax
>
> ```
> =GET.WORKBOOK(type_num, [book_name])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `type_num` | Yes | A number from 1 to 38 specifying what workbook information to return. Type 1 (sheet names array) is by far the most commonly used. |
| `book_name` | No | The name of the workbook to query. If omitted, uses the workbook containing the formula. Format: "Book1.xlsx" or full path. |

### Type Number Reference

| Type_num | Returns |
|----------|---------|
| 1 | Array of sheet names in workbook (most common) |
| 2 | Workbook name as text |
| 3 | Number of sheets in workbook |
| 4 | Number of documents open |
| 5 | TRUE if workbook is read-only |
| 6 | TRUE if workbook is password-protected structure |
| 7 | TRUE if workbook window is locked |
| 8 | TRUE if workbook has been changed since last save |
| 9 | TRUE if file is an add-in |
| 10 | TRUE if workbook is protected (sheets level) |
| 14 | Recalculation mode: 1=automatic, 2=automatic except tables, 3=manual |
| 16 | Returns TRUE if iteration is on |
| 32 | Returns array of selected sheets (if multiple selected) |
| 37 | Returns workbook full path and name |
| 38 | Returns sheet tab color index |

### Return Value

Depends on type_num. Type 1 returns a horizontal array of text strings (sheet names). Types 2, 37 return text. Types 3, 4 return numbers. Types 5-10, 16 return Boolean values.

## Examples

> [!f(x)] GET.WORKBOOK Examples

### Example 1: Get Array of All Sheet Names
**Named range definition:**
```
Name: SheetNames
Refers to: =GET.WORKBOOK(1)
```
**Result:** Array like {"[Book1.xlsx]Sheet1", "[Book1.xlsx]Sheet2", "[Book1.xlsx]Data"}

**Explanation:** Type 1 returns a horizontal array containing all sheet names. Each name includes the workbook name in brackets. This is the foundation for most GET.WORKBOOK applications.

---

### Example 2: Extract Single Sheet Name with INDEX
**Named range setup:**
```
Name: SheetList
Refers to: =GET.WORKBOOK(1)
```
**Worksheet formula:**
```
=INDEX(SheetList, 1, 2)
```
**Result:** "[Book1.xlsx]Sheet2" (the second sheet name)

**Explanation:** Since GET.WORKBOOK(1) returns a horizontal array, use INDEX with row 1 and the desired column number to extract specific sheet names. Column 1 = first sheet, Column 2 = second sheet, etc.

---

### Example 3: Get Clean Sheet Name (Without Workbook Prefix)
**Named range:**
```
Name: SheetList
Refers to: =GET.WORKBOOK(1)
```
**Worksheet formula:**
```
=REPLACE(INDEX(SheetList,1,1),1,FIND("]",INDEX(SheetList,1,1)),"")
```
**Result:** "Sheet1" (without "[Book1.xlsx]")

**Explanation:** Removes the workbook prefix by finding the "]" character and replacing everything before (and including) it with nothing. This gives you clean sheet names for display or further processing.

---

### Example 4: Count Total Sheets in Workbook
**Named range:**
```
Name: SheetCount
Refers to: =GET.WORKBOOK(3)
```
**Result:** 5 (or however many sheets exist)

**Explanation:** Type 3 directly returns the count of sheets. Simpler than using COUNTA on the sheet names array when you only need the count.

---

### Example 5: Get Full Workbook Path and Filename
**Named range:**
```
Name: FullPath
Refers to: =GET.WORKBOOK(37)
```
**Result:** "C:\Users\Name\Documents\[Budget.xlsx]Sheet1"

**Explanation:** Type 37 returns the complete file path including drive, folders, filename, and active sheet. Useful for file documentation or building external references.

---

### Example 6: Check if Workbook is Read-Only
**Named range:**
```
Name: IsReadOnly
Refers to: =GET.WORKBOOK(5)
```
**Result:** TRUE or FALSE

**Explanation:** Type 5 indicates whether the workbook was opened as read-only. Useful for user notifications or conditional logic based on edit permissions.

---

### Example 7: Check if Workbook Has Unsaved Changes
**Named range:**
```
Name: HasChanges
Refers to: =GET.WORKBOOK(8)
```
**Result:** TRUE if modified since last save

**Explanation:** Type 8 detects unsaved changes. Could be used in dashboard displays or before performing certain operations that assume saved state.

---

### Example 8: Create Table of Contents
**Setup:**
```
Name: SheetList
Refers to: =GET.WORKBOOK(1)
```
**In cell A1 and drag down:**
```
=IFERROR(REPLACE(INDEX(SheetList,1,ROW()),1,FIND("]",INDEX(SheetList,1,ROW())),""),"")
```
**Result:** Vertical list of all sheet names

**Explanation:** Using ROW() makes each cell reference a different element from the sheet names array. Creates an automatic table of contents that updates when sheets are added/removed.

---

### Example 9: Create Hyperlinks to Each Sheet
```
=HYPERLINK("#'" & A1 & "'!A1", A1)
```
Where A1 contains the clean sheet name from Example 8
**Result:** Clickable link that jumps to each sheet

**Explanation:** Combine the extracted sheet name with HYPERLINK to create navigation links. The "#'SheetName'!A1" syntax creates an internal workbook link.

---

### Example 10: Dynamic Sheet Reference with INDIRECT
```
=INDIRECT("'" & REPLACE(INDEX(SheetList,1,B1),1,FIND("]",INDEX(SheetList,1,B1)),"") & "'!A1")
```
Where B1 contains sheet number to reference
**Result:** Value from cell A1 of the selected sheet

**Explanation:** Builds a dynamic sheet reference from the sheet names array. Change B1 to switch which sheet is referenced. Essential for building sheet-switching dashboards.

---

### Example 11: Get Workbook Name Only
**Named range:**
```
Name: WorkbookName
Refers to: =GET.WORKBOOK(2)
```
**Result:** "Budget.xlsx"

**Explanation:** Type 2 returns just the workbook filename without path. Cleaner than parsing type 37 when you only need the filename.

---

### Example 12: Validate Sheet Name Exists
```
=NOT(ISERROR(MATCH("[" & WorkbookName & "]" & E1, SheetList, 0)))
```
Where E1 contains a sheet name to validate
**Result:** TRUE if sheet exists, FALSE if not

**Explanation:** Uses MATCH to search for a sheet name in the SheetList array. Returns FALSE (not found) or TRUE (found). Useful for data validation and error prevention.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NAME?` | Entering GET.WORKBOOK directly in worksheet | Create a named range in Name Manager that references the GET.WORKBOOK formula. |
| `#REF!` | Sheet was deleted but formula still references it | Update formulas or refresh the GET.WORKBOOK named range. |
| `#VALUE!` | Using unsupported type_num | Use valid type numbers (1-38, though not all are documented). |
| `Array in cell` | Using GET.WORKBOOK(1) without INDEX | Use INDEX(SheetList, 1, n) to extract individual sheet names from the array. |
| `Empty result` | Workbook not saved yet | GET.WORKBOOK(2) and (37) return empty on unsaved files. Save the workbook first. |
| `Only first name shows` | Array formula issue in older Excel | May need to enter as array formula (Ctrl+Shift+Enter) in older Excel versions. |

## Use Cases

### [[Dynamic Table of Contents]]

**Scenario:** Create a navigation page that automatically lists all sheets with clickable links.

**Implementation:**
```
Step 1: Create named range
Name: SheetList
Refers to: =GET.WORKBOOK(1)

Step 2: In column A, extract sheet names (drag down)
=IFERROR(REPLACE(INDEX(SheetList,1,ROW()),1,FIND("]",INDEX(SheetList,1,ROW())),""),"")

Step 3: In column B, create hyperlinks
=IF(A1<>"",HYPERLINK("#'"&A1&"'!A1","Go to: "&A1),"")
```

**Business Application:** Large workbooks with many tabs become navigable. Users can find sheets without scrolling through dozens of tabs. Essential for report templates, dashboards, and multi-department workbooks.

**Technical Details:** ROW() in the INDEX function makes each row reference a different sheet. The IFERROR handles rows beyond the sheet count. Hyperlinks use the internal reference format #'SheetName'!Cell.

---

### [[Sheet Name Dropdown Selector]]

**Scenario:** Let users select a sheet from a dropdown list, then display data from that sheet dynamically.

**Implementation:**
```
Step 1: Create named range for clean sheet names
Name: CleanSheets
Refers to: =SUBSTITUTE(SUBSTITUTE(GET.WORKBOOK(1),"[",""),CELL("filename")&"]","")

Step 2: Use in data validation
Data > Data Validation > List: =CleanSheets

Step 3: Reference selected sheet
=INDIRECT("'"&D1&"'!A1:C10")
```
Where D1 is the cell with the dropdown

**Business Application:** Interactive dashboards where users view different data sets by selecting sheets. Report generators that pull data from selected worksheets.

**Technical Details:** The SUBSTITUTE approach removes the "[workbook.xlsx]" prefix. Note that INDIRECT with the sheet name builds a dynamic reference that changes based on the dropdown selection.

---

### [[Sheet Existence Validation]]

**Scenario:** Before referencing a sheet programmatically, verify it exists to prevent #REF! errors.

**Implementation:**
```
Named range: SheetList
Refers to: =GET.WORKBOOK(1)

Validation formula:
=IF(ISERROR(MATCH("[" & GET.WORKBOOK(2) & "]" & InputSheet, SheetList, 0)),
    "Sheet not found",
    INDIRECT("'" & InputSheet & "'!A1"))
```
Where InputSheet is the user-entered sheet name

**Business Application:** Error-proofing spreadsheets that reference sheets by name from user input. Import routines that map data to specific sheets.

**Technical Details:** MATCH searches for the constructed sheet name in the array. If not found, ISERROR catches the #N/A and displays a friendly message instead of an error.

---

### [[Multi-Sheet Data Aggregation]]

**Scenario:** Sum or aggregate a specific cell across all sheets in the workbook.

**Implementation:**
```
Named range: SheetCount
Refers to: =GET.WORKBOOK(3)

Named range: SheetList
Refers to: =GET.WORKBOOK(1)

Aggregation formula (using SUMPRODUCT and INDIRECT):
=SUMPRODUCT(IFERROR(INDIRECT("'"&REPLACE(INDEX(SheetList,1,ROW(INDIRECT("1:"&SheetCount))),1,FIND("]",INDEX(SheetList,1,ROW(INDIRECT("1:"&SheetCount)))),"")&"'!B5"),0))
```
**Result:** Sum of cell B5 from every sheet

**Business Application:** Consolidating monthly data across monthly sheets. Totaling budgets across department sheets. Any scenario requiring cross-sheet aggregation.

**Technical Details:** This complex formula builds INDIRECT references to the same cell on each sheet, then sums them. It's array-like behavior that dynamically adjusts to the number of sheets. Consider using 3D references or Power Query for simpler alternatives in some cases.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 4.0 (1992) to current Excel 365
- **Access method:** Must be used through named ranges (Name Manager), not directly in cells
- **Array return:** Type 1 returns a horizontal array requiring INDEX to extract elements
- **Automatic updates:** Sheet list updates when sheets are added/removed (formulas may need refresh)
- **Path format:** Returns Windows-style paths (C:\folder\...) or Mac-style as appropriate

### Google Sheets

- **Not available:** Google Sheets does not support XLM macro functions
- **No equivalent:** There is no built-in Google Sheets function to get a list of sheet names
- **Alternative:** Use Apps Script: `SpreadsheetApp.getActiveSpreadsheet().getSheets()` returns sheet objects
- **Apps Script example:**
  ```javascript
  function getSheetNames() {
    return SpreadsheetApp.getActiveSpreadsheet()
      .getSheets()
      .map(sheet => sheet.getName());
  }
  ```

### Key Difference Alert

GET.WORKBOOK is completely Excel-specific with no Google Sheets formula equivalent. In Google Sheets, getting sheet names requires Apps Script or third-party add-ons. This is a significant limitation for users who need to build cross-platform compatible workbooks with dynamic sheet navigation or aggregation.

## Tips and Best Practices

1. **Always parse the sheet names:** GET.WORKBOOK(1) returns names like "[Book.xlsx]Sheet1". Use REPLACE and FIND to extract just the sheet name for display and INDIRECT references.

2. **Create reusable named ranges:** Set up SheetList, SheetCount, and WorkbookName as named ranges once, then reference them throughout your workbook.

3. **Handle hidden sheets:** GET.WORKBOOK(1) includes hidden sheets in its list. If you need only visible sheets, you may need VBA to filter them.

4. **Use with INDIRECT for dynamic references:** The combination of GET.WORKBOOK and INDIRECT enables powerful dynamic sheet referencing without VBA.

5. **Account for special characters in sheet names:** Sheet names with spaces or special characters need single quotes in references: `INDIRECT("'"&SheetName&"'!A1")`.

6. **Consider alternatives for simple cases:** For fixed sheet references or simple 3D formulas (=SUM(Sheet1:Sheet12!A1)), standard functions may be simpler than GET.WORKBOOK.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[GET.CELL]] | Returns cell-level properties | When you need cell info rather than workbook info |
| [[GET.DOCUMENT]] | Returns document properties (another XLM function) | When you need document metadata |
| [[CELL]] | Standard worksheet function for cell info | When CELL's limited workbook info (filename) suffices |
| [[SHEETS]] | Returns count of sheets in a reference | When you only need sheet count (modern alternative to type 3) |

### Commonly Used Together

**[[INDEX]]** - Extract elements from arrays

*Combined with GET.WORKBOOK for sheet name extraction:*
```
=INDEX(SheetList, 1, 3)
```
Get the 3rd sheet name from the SheetList array.

---

**[[INDIRECT]]** - Build dynamic references

*Combined with GET.WORKBOOK for dynamic sheet references:*
```
=INDIRECT("'" & CleanSheetName & "'!A1")
```
Reference cells on sheets identified by GET.WORKBOOK.

---

**[[REPLACE]] / [[FIND]]** - Parse sheet names

*Combined with GET.WORKBOOK to clean up names:*
```
=REPLACE(INDEX(SheetList,1,1),1,FIND("]",INDEX(SheetList,1,1)),"")
```
Remove the "[workbook.xlsx]" prefix from sheet names.

---

**[[HYPERLINK]]** - Create navigation links

*Combined with GET.WORKBOOK for table of contents:*
```
=HYPERLINK("#'" & SheetName & "'!A1", "Go to " & SheetName)
```
Create clickable links to each sheet.

---

**[[MATCH]]** - Find sheet position or validate existence

*Combined with GET.WORKBOOK for sheet validation:*
```
=MATCH("[Book.xlsx]DataSheet", SheetList, 0)
```
Find which position a sheet is in, or check if it exists.

## Official Documentation

- **Microsoft Excel:** [XL: List of Macro Functions](https://support.microsoft.com/en-us/office/xl-list-of-macro-functions-that-work-with-name-create-bb53aace-a98c-4c24-9e2f-24c63eb06d09)
- **Microsoft Support:** [Using GET.WORKBOOK to list worksheets](https://support.microsoft.com/en-us/topic/xl-list-of-macro-functions-that-work-with-name-create-bb53aace-a98c-4c24-9e2f-24c63eb06d09)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 4.0 | 1992 | Introduced as XLM macro function |
| Excel 5.0+ | 1993+ | Continued support through VBA era |
| Excel 2007+ | All versions | Works through named ranges in modern Excel |
| Excel 365 | Current | Full support; still the only formula-based way to get sheet names |
| Google Sheets | Not available | No support for XLM functions |

---

*Last updated: 2026-01-10*
