---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- text-functions
- array-functions
subTopics:
- array-conversion
- text-conversion
- data-display
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Array To Text
- Convert Array to String
tags:
- excel-only
- text-conversion
- arrays
- dynamic-arrays
---

# ARRAYTOTEXT

## Description

**ARRAYTOTEXT** converts an array or range into a text string, providing a readable representation of the array's contents. This function is invaluable when you need to display array results as a single text value, debug complex array formulas, or create human-readable summaries of multi-cell data. It essentially "flattens" an array into a comma-separated or formatted text string that can be stored in one cell or used in text operations.

The function offers two output formats: a "concise" format (format=0) that produces a simple comma-separated list suitable for display, and a "strict" format (format=1) that produces output which could theoretically be parsed back into an array. The concise format is what you'd show to end users; the strict format preserves more technical detail including quotes around text values and curly braces around the entire array.

**Important gotcha:** ARRAYTOTEXT does NOT work with 3D references (references spanning multiple sheets) and will return #VALUE! if you attempt this. Also, very large arrays may produce truncated results or errors due to Excel's text string length limits (32,767 characters maximum per cell). If your array contains errors, those error values will be converted to their text representations (e.g., "#N/A").

**Platform note:** ARRAYTOTEXT is an Excel-exclusive function introduced with the dynamic arrays feature in Microsoft 365. It is NOT available in Google Sheets, LibreOffice Calc, or older Excel versions (2019 and earlier). If cross-platform compatibility is required, you'll need alternative approaches using TEXTJOIN or custom concatenation formulas.

## Syntax

> [!f(x)] ARRAYTOTEXT Syntax
>
> ```
> =ARRAYTOTEXT(array, [format])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The array or range to convert to text. Can be a cell range, an array constant, or a formula that returns an array. |
| `format` | No | Determines the output format. 0 (or omitted) = concise format with simple comma separation. 1 = strict format with quotes around text and curly braces around the array. Default is 0. |

### Return Value

Returns a text string representing the contents of the array. The format depends on the format parameter: concise format produces clean, readable output; strict format produces technically precise output with delimiters.

## Examples

> [!f(x)] ARRAYTOTEXT Examples

### Example 1: Basic Array Conversion
```
=ARRAYTOTEXT({1,2,3,4,5})
```
**Result:** `1, 2, 3, 4, 5`

**Explanation:** The simplest use case - converting a horizontal array constant to text. Each element is separated by a comma and space. This concise format (default) is clean and human-readable.

---

### Example 2: Converting a Range to Text
```
=ARRAYTOTEXT(A1:A5)
```
**Result:** `Apple, Banana, Cherry, Date, Elderberry` (if A1:A5 contains these fruits)

**Explanation:** Converts a vertical range of cells into a single comma-separated text string. This is incredibly useful for creating summary cells that display all values from a column without needing complex TEXTJOIN formulas.

---

### Example 3: Strict Format for Technical Output
```
=ARRAYTOTEXT({1,2,3,4,5}, 1)
```
**Result:** `{1,2,3,4,5}`

**Explanation:** With format=1, the output includes curly braces and could be used as an array constant if pasted back into a formula. Numbers remain unquoted, maintaining their numeric representation.

---

### Example 4: Strict Format with Text Values
```
=ARRAYTOTEXT({"Apple","Banana","Cherry"}, 1)
```
**Result:** `{"Apple","Banana","Cherry"}`

**Explanation:** In strict mode, text values are wrapped in double quotes, distinguishing them from numeric values. This preserves the data type information in the output.

---

### Example 5: Two-Dimensional Array
```
=ARRAYTOTEXT({1,2,3;4,5,6;7,8,9})
```
**Result:** `1, 2, 3; 4, 5, 6; 7, 8, 9`

**Explanation:** For 2D arrays, semicolons separate rows while commas separate columns. This maintains the structure visually, making it easy to understand the array's dimensions.

---

### Example 6: Mixed Data Types
```
=ARRAYTOTEXT({100,"Text",TRUE,#N/A})
```
**Result:** `100, Text, TRUE, #N/A`

**Explanation:** ARRAYTOTEXT handles mixed data types gracefully. Numbers, text, Booleans, and even errors are all converted to their text representations. Note that in concise format, the quotes around "Text" are removed.

---

### Example 7: Displaying Dynamic Array Results
```
=ARRAYTOTEXT(UNIQUE(B2:B100))
```
**Result:** `Sales, Marketing, Engineering, Support` (unique departments from range)

**Explanation:** Particularly powerful when combined with dynamic array functions. Here, UNIQUE returns an array of distinct values, and ARRAYTOTEXT displays them all in one cell. Perfect for dashboards.

---

### Example 8: Debugging FILTER Results
```
=ARRAYTOTEXT(FILTER(A2:C10, D2:D10="Active"))
```
**Result:** `John, Sales, 50000; Jane, Marketing, 55000; Bob, Engineering, 60000`

**Explanation:** When debugging FILTER formulas, ARRAYTOTEXT shows exactly what the filter returned in a single cell. This is invaluable for troubleshooting complex filtering logic.

---

### Example 9: Creating a Summary Label
```
="Selected items: " & ARRAYTOTEXT(FILTER(A:A, B:B=TRUE))
```
**Result:** `Selected items: Item1, Item3, Item7`

**Explanation:** Combines static text with the array-to-text conversion to create descriptive labels. Useful for showing selected checkboxes, filtered options, or dynamic summaries.

---

### Example 10: Comparing Concise vs Strict Format
```
=ARRAYTOTEXT({"Hello",123,TRUE}, 0) & " vs " & ARRAYTOTEXT({"Hello",123,TRUE}, 1)
```
**Result:** `Hello, 123, TRUE vs {"Hello",123,TRUE}`

**Explanation:** Side-by-side comparison shows the difference clearly. Concise (0) removes quotes and braces for readability; strict (1) preserves technical structure. Use concise for display, strict for documentation or debugging.

---

### Example 11: Handling Empty Cells
```
=ARRAYTOTEXT(A1:A5)
```
Where A1="Apple", A2="", A3="Cherry", A4="", A5="Elderberry"

**Result:** `Apple, , Cherry, , Elderberry`

**Explanation:** Empty cells appear as blank entries between commas. If you need to skip blanks, wrap the range in FILTER first: `=ARRAYTOTEXT(FILTER(A1:A5, A1:A5<>""))`.

---

### Example 12: Nested in Conditional Logic
```
=IF(COUNTA(A1:A10)>0, ARRAYTOTEXT(A1:A10), "No data")
```
**Result:** Array contents as text, or "No data" if range is empty

**Explanation:** Defensive formula that checks for data before attempting conversion. Prevents awkward empty output or potential errors when working with dynamic ranges.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Attempting to use 3D reference (multiple sheets) | Use only single-sheet ranges; reference one sheet at a time |
| `#VALUE!` | Result string exceeds 32,767 characters | Reduce array size or use multiple ARRAYTOTEXT calls for portions of the data |
| `#NAME?` | Function not recognized (older Excel version) | ARRAYTOTEXT requires Microsoft 365; use TEXTJOIN as alternative |
| `#CALC!` | Array is too large for Excel to process | Break array into smaller chunks; consider different approach |
| Unexpected output | Wrong format parameter | Use 0 for concise (default) or 1 for strict; other values default to 0 |

## Use Cases

### [[Dashboard Summary Display]]

**Scenario:** Create a dashboard cell that shows all selected filter values from a slicer-like interface.

**Implementation:**
```
="Active Filters: " & IF(COUNTA(FilterSelections)=0, "None", ARRAYTOTEXT(FILTER(FilterOptions, FilterSelections=TRUE)))
```

**Business Application:** Executive dashboards often need to display which filters are currently active without taking up multiple cells. This single-cell approach keeps the layout clean while providing full visibility.

**Technical Details:** The nested IF handles the edge case of no selections. FILTER extracts only the selected items, and ARRAYTOTEXT converts them to a readable comma-separated list.

---

### [[Formula Debugging and Documentation]]

**Scenario:** Document what a complex array formula returns for troubleshooting or training purposes.

**Implementation:**
```
=ARRAYTOTEXT(SORT(UNIQUE(FILTER(Sales[Product], Sales[Region]="West"))), 1)
```

**Business Application:** When building complex reports, developers need to verify intermediate results. ARRAYTOTEXT with strict format creates output that can be pasted into documentation or compared against expected values.

**Technical Details:** Strict format (1) is preferred for debugging because it shows data types explicitly. This helps identify issues like numbers stored as text or unexpected empty values.

---

### [[Dynamic Email Content Generation]]

**Scenario:** Generate email body content listing all items requiring attention.

**Implementation:**
```
="Dear Team, the following items need review: " & ARRAYTOTEXT(FILTER(Items[Name], Items[Status]="Pending")) & ". Please address these by end of day."
```

**Business Application:** Automated notification systems can pull data directly from Excel and format it into readable messages. This reduces manual work and ensures no items are missed.

**Technical Details:** Keep array results manageable for email readability. Consider adding TAKE(array, 10) to limit output, with a count indicator if more exist.

## Platform Differences

### Microsoft Excel (365/2021+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Format parameter | Full support (0 and 1) |
| Dynamic arrays | Full support |
| Large arrays | Up to calculation limits |

### Google Sheets

| Feature | Support |
|---------|---------|
| ARRAYTOTEXT | NOT AVAILABLE |
| Alternative | Use TEXTJOIN with dynamic ranges |

**Workaround for Google Sheets:**
```
=TEXTJOIN(", ", TRUE, A1:A10)
```
This provides similar functionality for concise format. For strict format, manual construction is required.

### Other Platforms

ARRAYTOTEXT is exclusive to Microsoft Excel 365 and Excel 2021+. It is not available in:
- Google Sheets (use TEXTJOIN or JOIN instead)
- LibreOffice Calc
- Apple Numbers
- Excel Online (limited support)
- Excel 2019 and earlier

## Tips and Best Practices

1. **Use concise format (0) for user-facing output:** The default format produces cleaner, more readable text. Only use strict format (1) when you need technical precision or debugging.

2. **Combine with FILTER for meaningful output:** Raw ranges often contain blanks or irrelevant data. Wrap your range in FILTER to show only relevant values: `=ARRAYTOTEXT(FILTER(range, criteria))`.

3. **Watch the character limit:** Excel cells can hold maximum 32,767 characters. For large arrays, consider summarizing (using TAKE) or splitting across multiple cells.

4. **Handle errors gracefully:** If your array might contain errors, they'll appear as text like "#N/A". Consider wrapping source data in IFERROR if you prefer custom error text.

5. **Use for debugging, then remove:** ARRAYTOTEXT is excellent for checking intermediate array results during development. Once formulas are working, remove debug cells to keep workbooks clean.

6. **Consider TEXTJOIN for simple cases:** If you just need to concatenate a single-column range with a delimiter, TEXTJOIN may be more intuitive and is available in more Excel versions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VALUETOTEXT]] | Converts a single value to text | When working with single values, not arrays |
| [[TEXTJOIN]] | Joins text with delimiter | When you need more control over delimiter or want to ignore blanks |
| [[CONCAT]] | Concatenates text values | For simple concatenation without formatting |
| [[TEXT]] | Formats a value as text with format code | When you need specific number/date formatting |

### Commonly Used Together

**[[UNIQUE]]** - Extract distinct values before converting to text

*Display all unique categories in one cell:*
```
=ARRAYTOTEXT(UNIQUE(Products[Category]))
```
Shows each category once, comma-separated. Perfect for filter displays.

---

**[[FILTER]]** - Filter data before converting to text

*Show only active items:*
```
=ARRAYTOTEXT(FILTER(Tasks[Name], Tasks[Status]="Active"))
```
Displays only relevant items rather than entire ranges.

---

**[[SORT]]** - Sort array before displaying

*Alphabetized list in one cell:*
```
=ARRAYTOTEXT(SORT(UNIQUE(Employees[Department])))
```
Combines deduplication, sorting, and text conversion for clean output.

---

**[[TAKE]]** - Limit array size before converting

*Show top 5 items only:*
```
=ARRAYTOTEXT(TAKE(SORT(Scores, , -1), 5))
```
Prevents overly long text strings by limiting to most relevant items.

## Official Documentation

- **Microsoft Excel:** [ARRAYTOTEXT function](https://support.microsoft.com/en-us/office/arraytotext-function-9cdcad46-2fa5-4c6b-ac92-14e7bc862b8b)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2020 (with dynamic arrays) | Initial release as part of dynamic array functions |
| Excel 2021 | 2021 | Included in perpetual license version |
| Excel Online | 2020 | Full support in web version |
| Earlier versions | Not available | No support in Excel 2019 or earlier |

---

*Last updated: 2026-01-10*
