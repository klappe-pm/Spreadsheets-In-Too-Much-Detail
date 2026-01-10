---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- information
subTopics:
- sheet-count
- workbook-structure
- 3d-references
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sheet Count
- Number of Sheets
- Count Sheets
tags:
- information
- sheet-count
- workbook
- 3d-reference
- structure
---

# SHEETS

## Description

**SHEETS** returns the number of sheets in a reference. Called without arguments, it returns the total count of all sheets in the current workbook—visible, hidden, and very hidden alike. Called with a 3D reference (a reference spanning multiple sheets), it returns how many sheets are included in that range. This simple counting function is essential for dynamic workbook navigation, validation, and structure-aware formulas.

The function answers the question "how many sheets are there?" in two contexts. At the workbook level, `=SHEETS()` tells you the total sheet count—useful for building navigation systems, validating workbook structure, or calculating relative positions. At the reference level, `=SHEETS(Sheet1:Sheet5!A1)` tells you how many sheets are spanned by a 3D reference—useful for verifying cross-sheet aggregations.

**Key distinction from SHEET:** SHEETS (plural) counts sheets. SHEET (singular) returns the position number of a specific sheet. They work together beautifully: SHEET tells you "I'm on sheet 3" while SHEETS tells you "there are 10 sheets total." Together, they enable formulas like "This is sheet 3 of 10" or "I'm 30% through the workbook."

**Counting behavior:** Hidden sheets are counted. Very hidden sheets (xlVeryHidden) are also counted. Chart sheets are counted. Macro sheets are counted. Essentially, if it appears in the sheet collection of the workbook, SHEETS counts it. This can be surprising when your workbook has hidden configuration sheets that inflate the count.

## Syntax

> [!f(x)] SHEETS Syntax
>
> ```
> =SHEETS([reference])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `reference` | No | A reference, possibly a 3D reference spanning multiple sheets. If omitted, returns the total number of sheets in the workbook containing the formula. When a 3D reference is provided (like Sheet1:Sheet5!A1), returns the count of sheets in that range. |

### Return Value

Returns a positive integer representing the count of sheets. When called without arguments, this is the total workbook sheet count. When called with a 3D reference, it's the number of sheets spanned by that reference. Returns #REF! if the reference is invalid.

## Examples

> [!f(x)] SHEETS Examples

### Example 1: Count All Sheets in Workbook
```
=SHEETS()
```
**Result:** 12 (if workbook contains 12 sheets)

**Explanation:** Without arguments, SHEETS returns the total count of all sheets in the workbook. Includes visible, hidden, and very hidden sheets. This is the most common usage.

---

### Example 2: Count Sheets in 3D Reference
```
=SHEETS(January:December!A1)
```
**Result:** 12 (if there are 12 monthly sheets)

**Explanation:** When given a 3D reference, SHEETS counts how many sheets are spanned. Useful for verifying that aggregation formulas cover the expected number of sheets.

---

### Example 3: Validate 3D Reference Coverage
```
=IF(SHEETS(Q1:Q4!A1)=4, "All quarters included", "Missing quarters")
```
**Result:** Validation message based on sheet count

**Explanation:** Checks that a quarterly 3D reference actually includes exactly 4 sheets. Catches errors where sheets were renamed, deleted, or reordered.

---

### Example 4: Display Position in Workbook
```
="Sheet " & SHEET() & " of " & SHEETS()
```
**Result:** "Sheet 3 of 12"

**Explanation:** Combines SHEET (current position) with SHEETS (total count) for navigation display. Classic pattern for table of contents or header information.

---

### Example 5: Calculate Progress Through Workbook
```
=SHEET()/SHEETS()
```
**Result:** 0.25 (if on sheet 3 of 12)

**Explanation:** Returns a percentage indicating how far through the workbook the current sheet is. Format as percentage for "25% complete" style display.

---

### Example 6: Calculate Remaining Sheets
```
=SHEETS()-SHEET()
```
**Result:** 9 (if on sheet 3 of 12)

**Explanation:** Simple arithmetic to determine how many sheets remain after the current one. Useful for progress indicators or workbook navigation.

---

### Example 7: Dynamic Summary Range
```
=SUM(INDIRECT("Sheet1:Sheet"&SHEETS()&"!A1"))
```
**Result:** Sum of A1 across all sheets

**Explanation:** Builds a 3D reference dynamically using SHEETS to determine the ending sheet. Automatically includes any new sheets added to the end.

---

### Example 8: Verify No Sheets Added or Removed
```
=IF(SHEETS()=ExpectedCount, "Structure OK", "Sheet count changed: "&SHEETS())
```
Where ExpectedCount is a named range set to the expected number
**Result:** Validation of workbook structure

**Explanation:** Template integrity check. If someone adds or removes sheets, this formula alerts users. Essential for controlled templates.

---

### Example 9: Check if Workbook is Large
```
=IF(SHEETS()>20, "Large workbook - performance may be affected", "")
```
**Result:** Warning for large workbooks

**Explanation:** Performance notification based on sheet count. Large workbooks with many sheets can be slow; this provides user awareness.

---

### Example 10: Calculate Average Per Sheet
```
=SUM(Sheet1:Sheet12!B:B)/SHEETS(Sheet1:Sheet12!A1)
```
**Result:** Average total per sheet

**Explanation:** Uses SHEETS to get the divisor for calculating per-sheet averages. The 3D SUM gets the total; SHEETS provides the count.

---

### Example 11: With Single Sheet Reference
```
=SHEETS(DataSheet!A1)
```
**Result:** 1

**Explanation:** A single-sheet reference (not a 3D range) returns 1, since only one sheet is referenced. Not particularly useful, but consistent behavior.

---

### Example 12: Conditional Logic Based on Sheet Count
```
=IF(SHEETS()<=3, "Small workbook", IF(SHEETS()<=10, "Medium workbook", "Large workbook"))
```
**Result:** Classification of workbook size

**Explanation:** Categorizes workbook complexity by sheet count. Useful for documentation or for adjusting behavior based on workbook scale.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Referenced sheet in 3D range doesn't exist | Verify all sheets in the 3D reference range exist. One deleted sheet breaks the entire reference. |
| `Unexpected count` | Hidden sheets are counted | Remember that SHEETS includes hidden and very hidden sheets. Visible tab count may differ. |
| `Count doesn't match visible tabs` | Chart sheets or very hidden sheets exist | Chart sheets and xlVeryHidden sheets count but may not be visible in normal view. |
| `Count changed unexpectedly` | Sheets added/removed by other users | SHEETS dynamically reflects current workbook structure. In shared workbooks, count may change. |
| `#VALUE!` | Invalid reference provided | Ensure the reference is a valid cell or range reference, possibly with 3D syntax. |

## Use Cases

### [[Workbook Navigation System]]

**Scenario:** Create a table of contents that shows current position and total sheets for easy navigation.

**Implementation:**
```
Header formula:
="Page " & SHEET() & " of " & SHEETS()

Navigation bar:
="<< First | " & IF(SHEET()>1, "< Previous | ", "") & IF(SHEET()<SHEETS(), "Next > | ", "") & "Last >>"
```

**Business Application:** Multi-page reports requiring navigation aids. Training workbooks where users progress through sheets. Any large workbook needing orientation information.

**Technical Details:** Combine with HYPERLINK to make navigation clickable. Use INDIRECT with SHEET() calculations to build actual links to first, previous, next, and last sheets.

---

### [[Dynamic 3D Reference Building]]

**Scenario:** Create summary formulas that automatically include all data sheets.

**Implementation:**
```
Assuming data sheets are named Sheet1 through SheetN:
=SUM(INDIRECT("Sheet1:Sheet"&SHEETS()&"!A1:A100"))

Or with fixed end reference:
=SUM(INDIRECT("DataStart:DataEnd!A1:A100"))
Then verify: =IF(SHEETS(DataStart:DataEnd!A1)=ExpectedCount, Result, "Check sheets")
```

**Business Application:** Summary dashboards that automatically aggregate across all data sheets. Monthly totals that auto-include new months as sheets are added.

**Technical Details:** INDIRECT with 3D references can be slow for large data sets. Consider using Power Query for complex multi-sheet aggregation. SHEETS helps validate that the expected number of sheets are being included.

---

### [[Template Integrity Validation]]

**Scenario:** Ensure critical workbook templates haven't been modified by checking sheet count.

**Implementation:**
```
In a validation cell:
=IF(AND(SHEETS()=12, SHEET(Config!A1)=1, SHEET(Output!A1)=SHEETS()),
    "Template structure verified",
    "WARNING: Template has been modified - expected 12 sheets")
```

**Business Application:** Financial templates requiring strict structure. Compliance documents where sheet organization matters. Any template where unauthorized modifications cause issues.

**Technical Details:** Combine SHEETS (for total count) with SHEET (for specific positions) to fully validate structure. Consider storing expected values in a protected configuration sheet.

---

### [[Progress Tracking Across Sheets]]

**Scenario:** Show users their progress through a multi-sheet workflow or data entry process.

**Implementation:**
```
Progress percentage:
=TEXT(SHEET()/SHEETS(), "0%") & " complete"

Progress bar (using REPT):
=REPT("=", SHEET()) & REPT("-", SHEETS()-SHEET())

Steps remaining:
=SHEETS()-SHEET() & " steps remaining"
```

**Business Application:** Data entry workflows spanning multiple sheets. Training modules where each sheet is a lesson. Onboarding checklists with sheet-per-task structure.

**Technical Details:** The visual progress bar uses REPT to create a string of characters representing completed (=) and remaining (-) steps. Adjust characters and styling as needed.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2013 and later (not available in Excel 2010 or earlier)
- **Hidden sheets:** Included in count (hidden and very hidden)
- **Chart sheets:** Included in count
- **3D references:** Fully supported for counting sheets in range
- **Macro sheets:** Included in count (Excel 4.0 macro sheets)

### Google Sheets

- **Availability:** Fully supported
- **Hidden sheets:** Included in count
- **3D references:** Not supported in Google Sheets (no 3D reference syntax)
- **Chart sheets:** Google Sheets doesn't have separate chart sheets
- **Behavior:** Returns total sheet count; cannot count sheets in a range

### Key Difference Alert

The most significant difference is 3D reference support. Excel allows `=SHEETS(Sheet1:Sheet5!A1)` to count sheets in a range, which is useful for validating cross-sheet formulas. Google Sheets doesn't have 3D reference syntax at all, so SHEETS can only return the total workbook sheet count. This limits its utility in Google Sheets for validation scenarios.

## Tips and Best Practices

1. **Remember hidden sheets count:** Your workbook might show 5 tabs but SHEETS returns 8 because of hidden configuration sheets. Check for hidden sheets if the count seems wrong.

2. **Pair with SHEET for navigation:** The combination `="Sheet " & SHEET() & " of " & SHEETS()` is a spreadsheet classic for good reason. Use it for orientation.

3. **Validate 3D references:** When building cross-sheet formulas, use SHEETS to verify you're capturing the expected number of sheets. `=SHEETS(Q1:Q4!A1)` should equal 4 for quarterly data.

4. **Use for template protection:** Store expected sheet count and verify with SHEETS. Alert users if structure has been modified.

5. **Consider performance in large workbooks:** While SHEETS itself is fast, formulas that iterate through all sheets can be slow. Use sheet count awareness to warn users about performance.

6. **Google Sheets limitation:** Remember that 3D references don't exist in Google Sheets. SHEETS() will only return total count, not count within a range.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SHEET]] | Returns position number of a specific sheet | When you need to know which sheet you're on, not how many exist |
| [[GET.WORKBOOK]] | XLM function returning sheet count and names | When you need sheet names array, not just count (Excel only) |
| [[COUNTA]] | Counts non-empty cells | Different use case, but often confused with sheet counting |
| [[ROWS]] / [[COLUMNS]] | Count rows/columns in range | Analogous counting functions for range dimensions |

### Commonly Used Together

**[[SHEET]]** - Current sheet position

*Combined with SHEETS for navigation display:*
```
="Sheet " & SHEET() & " of " & SHEETS()
```
The classic position indicator.

---

**[[INDIRECT]]** - Build dynamic references

*Combined with SHEETS for dynamic 3D references:*
```
=SUM(INDIRECT("Sheet1:Sheet"&SHEETS()&"!A1"))
```
Create 3D references that adapt to sheet count.

---

**[[IF]]** - Conditional logic

*Combined with SHEETS for structure validation:*
```
=IF(SHEETS()=12, "Monthly template OK", "Check sheet count")
```
Validate workbook structure.

---

**[[TEXT]]** - Format calculations

*Combined with SHEETS for progress display:*
```
=TEXT(SHEET()/SHEETS(), "0%") & " complete"
```
Format progress as percentage.

---

**[[REPT]]** - Create visual elements

*Combined with SHEETS for progress bars:*
```
=REPT("|", SHEET()) & REPT(".", SHEETS()-SHEET())
```
Visual representation of progress through workbook.

## Official Documentation

- **Microsoft Excel:** [SHEETS function](https://support.microsoft.com/en-us/office/sheets-function-770515eb-e1e8-45ce-8066-b557e5e4b80b)
- **Google Sheets:** [SHEETS function](https://support.google.com/docs/answer/3093057)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 | Original introduction | New function alongside SHEET |
| Excel 2016+ | All subsequent | No changes to functionality |
| Excel 365 | Current | Unchanged behavior |
| Google Sheets | Original launch | Available but without 3D reference support |

---

*Last updated: 2026-01-10*
