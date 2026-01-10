---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - text-manipulation
subTopics:
  - text-concatenation
  - delimiter-joining
  - array-to-text
  - string-building
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - join-text
  - concatenate-array
  - delimiter-join
  - array-join
tags:
  - google-sheets-only
  - text-functions
  - concatenation
  - delimiters
  - array-functions
---

# JOIN

## Description

The **JOIN function** is a Google Sheets-exclusive function that concatenates the elements of one or more arrays using a specified delimiter, returning a single text string. This function is the inverse of SPLIT—while SPLIT breaks text into an array using a delimiter, JOIN combines array elements back into text with a delimiter between each element. JOIN is essential for creating comma-separated lists from column data, building dynamic text strings from multiple values, and reconstructing data after transformation.

The function takes a delimiter string as its first argument, followed by one or more ranges or arrays to join. Each element in the specified ranges becomes part of the output string, separated by the delimiter. Unlike simple concatenation with the ampersand (&) operator, JOIN automatically handles arrays and inserts the delimiter between every element without manual intervention. This makes JOIN invaluable for converting vertical lists to horizontal comma-separated values, building SQL IN clauses, creating formatted output strings, and any situation requiring systematic combination of multiple values.

JOIN processes multiple ranges in the order they're provided, concatenating all elements from the first range before moving to the second, and so on. Empty cells are typically included in the join operation, which may result in consecutive delimiters if blank cells exist within the range. To handle empty cells, JOIN is often combined with FILTER to exclude blanks before joining. The delimiter can be any string—a single character like "," or a multi-character sequence like ", " or even " and ".

While Excel has a similar TEXTJOIN function (introduced in Excel 2016), it differs from Google Sheets' JOIN in significant ways, including an option to ignore empty cells built into the function itself. Excel does not have a function specifically named JOIN, making this a Google Sheets-specific function name. For cross-platform compatibility, consider that the function may need to be rewritten using TEXTJOIN when moving formulas to Excel.

## Syntax

> [!f(x)] JOIN Syntax
>
> ```
> =JOIN(delimiter, value_or_array1, [value_or_array2, ...])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `delimiter` | Yes | The text string to insert between joined elements. Can be empty ("") for no delimiter, a single character (","), or a multi-character string (" - "). The delimiter appears between every element but not before the first or after the last. |
| `value_or_array1` | Yes | The first value, cell reference, or range to include in the joined result. Can be a single value, a row range (A1:E1), a column range (A1:A10), or a multi-dimensional range. |
| `value_or_array2, ...` | No | Additional values, cell references, or ranges to append to the joined result. Multiple ranges are processed in order, with all elements from each range joined before moving to the next. |

### Return Value

Returns a single text string containing all elements from the specified ranges, with the delimiter inserted between each element. If all inputs are empty, returns an empty string. Numbers and dates in the input are converted to text representation.

## Examples

> [!f(x)] JOIN Examples

### Example 1: Basic Comma Join
```
=JOIN(",", A1:A5)
```
**Scenario:** A1:A5 contains: apple, banana, cherry, date, elderberry
**Result:** "apple,banana,cherry,date,elderberry"

**Explanation:** The function takes each value in the column range and combines them into a single string with commas between. This is the most common JOIN use case.

---

### Example 2: Join with Comma-Space
```
=JOIN(", ", A1:A5)
```
**Scenario:** Same data as above
**Result:** "apple, banana, cherry, date, elderberry"

**Explanation:** Using ", " (comma-space) as the delimiter creates more readable output with proper spacing after each comma.

---

### Example 3: Join Row Range
```
=JOIN(" | ", B1:F1)
```
**Scenario:** B1:F1 contains headers: Name, Email, Phone, Address, Notes
**Result:** "Name | Email | Phone | Address | Notes"

**Explanation:** JOIN works with horizontal ranges as well as vertical. The pipe character with spaces creates a visually distinct separation.

---

### Example 4: Join Multiple Ranges
```
=JOIN(",", A1:A3, B1:B3)
```
**Scenario:** A1:A3 = 1,2,3 and B1:B3 = 4,5,6
**Result:** "1,2,3,4,5,6"

**Explanation:** Multiple ranges are joined sequentially. First all elements from the first range, then all elements from subsequent ranges.

---

### Example 5: Join Without Delimiter
```
=JOIN("", A1:A5)
```
**Scenario:** A1:A5 contains: a, b, c, d, e
**Result:** "abcde"

**Explanation:** An empty string as delimiter concatenates all values directly with no separator, similar to using multiple & operators.

---

### Example 6: Join with Newline Character
```
=JOIN(CHAR(10), A1:A5)
```
**Scenario:** Creating multi-line text in a single cell
**Result:** Each value on its own line (with cell wrap enabled)

**Explanation:** CHAR(10) is the newline character. Combined with JOIN, it creates multi-line cell content from an array. Enable text wrap to see multiple lines.

---

### Example 7: Join and Reverse Split
```
=JOIN("-", SPLIT(A1, ","))
```
**Scenario:** A1 contains "a,b,c,d" and you want "a-b-c-d"
**Result:** "a-b-c-d"

**Explanation:** SPLIT breaks text apart, JOIN puts it back together with a different delimiter. This pattern is common for delimiter conversion.

---

### Example 8: Filter Before Joining (Exclude Blanks)
```
=JOIN(", ", FILTER(A1:A10, A1:A10<>""))
```
**Scenario:** A1:A10 has some blank cells mixed with values
**Result:** Only non-empty values, joined with commas

**Explanation:** FILTER removes empty cells before JOIN processes the array. This prevents double delimiters from blank cells.

---

### Example 9: Join with UNIQUE for Distinct Values
```
=JOIN(", ", UNIQUE(A1:A20))
```
**Scenario:** A1:A20 contains repeated values
**Result:** Distinct values only, comma-separated

**Explanation:** UNIQUE removes duplicates first, then JOIN combines the unique values. Useful for creating deduplicated lists.

---

### Example 10: Building SQL IN Clause
```
="SELECT * FROM users WHERE id IN ('" & JOIN("','", A1:A10) & "')"
```
**Scenario:** A1:A10 contains: 101, 102, 103, 104, 105
**Result:** "SELECT * FROM users WHERE id IN ('101','102','103','104','105')"

**Explanation:** JOIN with "','" creates properly quoted list items for SQL. The outer quotes complete the first and last items.

---

### Example 11: Join with "and" for Natural Language
```
=IF(COUNTA(A1:A5)=1, A1,
  IF(COUNTA(A1:A5)=2, JOIN(" and ", A1:A2),
    JOIN(", ", OFFSET(A1,0,0,COUNTA(A1:A5)-1)) & ", and " & INDEX(A1:A5, COUNTA(A1:A5))))
```
**Scenario:** Creating "A, B, and C" style lists
**Result:** "apple, banana, and cherry"

**Explanation:** This complex formula creates Oxford comma lists ("A, B, and C"). Simple JOIN doesn't handle this case—you need to treat the last element specially.

---

### Example 12: Join Sorted Values
```
=JOIN(", ", SORT(A1:A10, 1, TRUE))
```
**Scenario:** Create alphabetically sorted comma list
**Result:** Values joined in A-Z order

**Explanation:** SORT orders the values before JOIN combines them. Useful for creating consistent, ordered lists.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid delimiter or empty first argument | Ensure delimiter is specified (use "" for no delimiter) |
| `#REF!` | Referenced range was deleted | Restore range or update formula reference |
| `Double delimiters in output` | Empty cells in the source range | Use FILTER to exclude blanks: FILTER(A:A, A:A<>"") |
| `Numbers appearing as text incorrectly` | Numeric formatting not preserved | JOIN converts to text; format may be lost |
| `Dates showing as serial numbers` | Dates converted to underlying values | Use TEXT to format dates before JOIN |
| `Result too long` | Too many elements or very long delimiter | Cell display has limits; data is still complete |
| `#N/A in output` | NA values in source range | Filter out or handle #N/A before joining |
| `Unexpected order of elements` | Multiple ranges join sequentially | Elements from first range come before second |

## Use Cases

### [[Creating Dynamic Email Lists]]

**Scenario:** A project manager needs to create email distribution lists from spreadsheet data. Team members are listed in a column with their email addresses, and the manager needs comma-separated lists for email clients, meeting invites, and notification systems.

**Implementation:**
```
Email list structure:
Column A: Team Member Name
Column B: Email Address
Column C: Project Assignment
Column D: Active (TRUE/FALSE)

All Active Team Emails:
=JOIN("; ", FILTER(B:B, D:D=TRUE, B:B<>""))

Project-Specific Email List:
=JOIN("; ", FILTER(B:B, C:C="Project Alpha", D:D=TRUE, B:B<>""))

Formatted Name <Email> List:
=JOIN("; ", ARRAYFORMULA(FILTER(A:A & " <" & B:B & ">", D:D=TRUE, B:B<>"")))

BCC-Ready List (comma-separated):
=JOIN(", ", FILTER(B:B, D:D=TRUE, B:B<>""))

With domain filtering (only company emails):
=JOIN("; ", FILTER(B:B, REGEXMATCH(B:B, "@company\.com$"), D:D=TRUE))
```

**Business Application:** Email distribution lists are constantly needed but tedious to compile manually. JOIN with FILTER creates dynamic lists that update automatically as team members are added or status changes. Different formats (semicolon for Outlook, comma for Gmail) are easily switched by changing the delimiter.

**Technical Details:** Email clients vary in delimiter requirements—Outlook prefers semicolons, Gmail accepts commas. The "Name <email>" format is supported by most email clients for display names. FILTER excludes inactive members and blanks. REGEXMATCH enables domain filtering for internal communications.

---

### [[Building Dynamic QUERY Clauses]]

**Scenario:** A data analyst needs to build QUERY function WHERE clauses dynamically based on user selections. Filter criteria are selected via checkboxes or dropdowns, and the QUERY needs to incorporate all selected values.

**Implementation:**
```
Filter selection structure:
Column A: Available Values (Product categories)
Column B: Checkbox (TRUE/FALSE for selected)
Data in Sheet2: Product data with Category column

Building WHERE IN clause:
="SELECT * WHERE Category IN ('" & JOIN("','", FILTER(A:A, B:B=TRUE)) & "')"

Full QUERY with dynamic filter:
=QUERY(Sheet2!A:F,
  "SELECT A, B, C WHERE D IN ('" &
  JOIN("','", FILTER(A:A, B:B=TRUE)) &
  "')")

Handling all selected (empty filter means all):
=QUERY(Sheet2!A:F,
  "SELECT A, B, C" &
  IF(COUNTA(FILTER(A:A, B:B=TRUE))>0,
    " WHERE D IN ('" & JOIN("','", FILTER(A:A, B:B=TRUE)) & "')",
    ""))

Multiple filter criteria:
="SELECT * WHERE Category IN ('" & JOIN("','", SelectedCategories) &
  "') AND Status IN ('" & JOIN("','", SelectedStatuses) & "')"
```

**Business Application:** Static QUERY functions limit analysis flexibility. Dynamic WHERE clauses enable interactive dashboards where users select filter values via checkboxes, and the QUERY automatically incorporates their selections. This creates a powerful filter interface without complex scripting.

**Technical Details:** QUERY's WHERE IN syntax requires single-quoted values separated by commas within parentheses. JOIN with "','" creates this format, with outer quotes completing the first and last values. Handle the empty selection case to avoid syntax errors when no filters are selected.

---

### [[Generating Formatted Reports]]

**Scenario:** A customer service team needs to generate ticket summaries that combine multiple data fields into readable paragraphs. Each ticket has details across columns that need to be combined into a narrative format for email responses or documentation.

**Implementation:**
```
Ticket data structure:
Column A: Ticket ID
Column B: Customer Name
Column C: Issue Type
Column D: Priority
Column E: Status
Column F: Assigned Agent
Column G: Summary text

Formatted ticket summary (one cell):
="Ticket #" & A2 & " - " & C2 & CHAR(10) &
  "Customer: " & B2 & CHAR(10) &
  "Priority: " & D2 & " | Status: " & E2 & CHAR(10) &
  "Assigned to: " & F2

List of action items from column:
=JOIN(CHAR(10) & "- ", FILTER(Actions!A:A, Actions!B:B=A2))

Comma-separated tag list:
="Tags: " & JOIN(", ", FILTER(Tags!A:A, Tags!B:B=A2))

Multi-field summary with JOIN:
=JOIN(" | ", FILTER({B2, C2, D2, E2}, {B2, C2, D2, E2}<>""))
```

**Business Application:** Raw data in columns isn't suitable for customer communication or documentation. JOIN with CHAR(10) creates multi-line formatted text in single cells. This enables automatic generation of email templates, status updates, and reports from structured data.

**Technical Details:** CHAR(10) creates line breaks within cells. Enable text wrapping to see multiple lines. The {B2, C2, D2, E2} array syntax allows joining specific cells. FILTER removes blanks from the combined fields.

---

### [[Creating Delimited Export Strings]]

**Scenario:** A developer needs to export data from Google Sheets in specific formats for import into other systems. Some systems require pipe-delimited values, others need tab-separated or custom delimiters. The export format needs to match the target system's requirements.

**Implementation:**
```
Export data structure:
Columns A-E: Data fields to export

Pipe-delimited row:
=JOIN("|", A2:E2)

Tab-delimited row:
=JOIN(CHAR(9), A2:E2)

CSV with quoted values:
=""""" & JOIN(""",""", A2:E2) & """"

Fixed-width with padding (requires helper):
=JOIN("", ARRAYFORMULA(REPT(" ", {20,15,10,10,30}-LEN(A2:E2)) & A2:E2))

JSON array format:
="[""" & JOIN(""",""", A2:E2) & """]"

All rows combined (export block):
=JOIN(CHAR(10), ARRAYFORMULA(A2:A100 & "|" & B2:B100 & "|" & C2:C100))
```

**Business Application:** Data integration often requires specific delimiter formats. While Google Sheets can export CSV, custom delimiters, quoted CSV, and other formats require formula-based generation. JOIN creates the formatted output that can then be copied to the target system.

**Technical Details:** CHAR(9) is the tab character. Quoted CSV requires careful escaping of quote characters using doubled quotes. For full exports, combine row-wise JOIN with CHAR(10) joins between rows. Consider Apps Script for complex transformations or large datasets.

## Platform Differences

JOIN is a **Google Sheets-exclusive function**. Excel has TEXTJOIN which offers similar but not identical functionality.

### Google Sheets
- **Function Name:** JOIN
- **Syntax:** JOIN(delimiter, value_or_array1, ...)
- **Empty Cell Handling:** Includes empty cells (creates consecutive delimiters)
- **Range Support:** Multiple ranges supported as separate arguments
- **Availability:** All versions

### Microsoft Excel
- **Function Name:** TEXTJOIN (no JOIN function)
- **Syntax:** TEXTJOIN(delimiter, ignore_empty, text1, ...)
- **Empty Cell Handling:** Built-in ignore_empty parameter
- **Range Support:** Similar range support
- **Availability:** Excel 2016+ and Excel 365

### Key Differences Summary

| Feature | Google Sheets (JOIN) | Microsoft Excel (TEXTJOIN) |
|---------|---------------------|--------------------------|
| Function Name | JOIN | TEXTJOIN |
| Ignore Empty Option | Not built-in (use FILTER) | Built-in parameter |
| Multiple Ranges | Unlimited arguments | 252 arguments max |
| Syntax Simplicity | Simpler | More verbose |
| Legacy Support | All versions | 2016+ only |

## Tips and Best Practices

1. **Include spaces in delimiters for readability:** Use ", " rather than "," when creating lists for human reading. The space after the comma significantly improves readability.

2. **Filter out empty cells before joining:** Use `JOIN(", ", FILTER(A:A, A:A<>""))` to prevent double delimiters from blank cells. This is essential for clean output.

3. **Use CHAR(10) for multi-line output:** CHAR(10) creates newlines within cells. Combined with JOIN, it creates multi-line cell content. Enable text wrapping in the cell to see the lines.

4. **Combine with UNIQUE for distinct lists:** Use `JOIN(", ", UNIQUE(...))` to create deduplicated comma-separated lists, avoiding repetition in the output.

5. **Format dates and numbers before joining:** JOIN converts values to text, which may lose formatting. Use TEXT(A1, "MM/DD/YYYY") for dates or TEXT(A1, "$#,##0.00") for currency before joining.

6. **Consider TEXTJOIN for Excel compatibility:** If your spreadsheet may be used in Excel, consider whether the formula needs to be rewritten. Document the Excel alternative in comments.

7. **Handle the Oxford comma case separately:** JOIN doesn't handle "A, B, and C" format automatically. For natural language lists, build a custom formula that treats the last element differently.

8. **Use TRANSPOSE when needed:** If you need to join values that are in a row range when working with column-oriented functions, use TRANSPOSE to flip the orientation.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SPLIT]] | Divides text into an array | Reverse of JOIN—breaking text into parts |
| [[CONCATENATE]] | Joins individual values | When joining specific cells, not ranges |
| [[TEXTJOIN]] | Excel's equivalent function | In Excel environments |
| [[CONCAT]] | Simple concatenation | For basic joining without delimiter |

### Commonly Used Together

**[[SPLIT]]** - Inverse operation

*Split and rejoin with different delimiter:*
```
=JOIN("-", SPLIT(A1, ","))
```
Converts comma-separated to hyphen-separated text.

---

**[[FILTER]]** - Remove blanks before joining

*Join only non-empty values:*
```
=JOIN(", ", FILTER(A:A, A:A<>""))
```
Prevents double delimiters from empty cells.

---

**[[UNIQUE]]** - Deduplicate before joining

*Create list of unique values:*
```
=JOIN(", ", UNIQUE(A1:A100))
```
Ensures no repeated values in the joined output.

---

**[[SORT]]** - Order values before joining

*Create sorted comma list:*
```
=JOIN(", ", SORT(A1:A100))
```
Produces alphabetically ordered output.

---

**[[TRANSPOSE]]** - Change array orientation

*Join row values as if they were a column:*
```
=JOIN(", ", TRANSPOSE(A1:E1))
```
Useful when source data orientation doesn't match expected input.

---

**[[CHAR]]** - Insert special characters

*Create multi-line output:*
```
=JOIN(CHAR(10), A1:A10)
```
CHAR(10) is newline, CHAR(9) is tab.

## Official Documentation

- **Google Sheets:** [JOIN function](https://support.google.com/docs/answer/3094077)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | ~2010 | Original implementation |
| Google Sheets | 2014 | Performance improvements for large arrays |
| Google Sheets | 2018 | Better handling of mixed data types |
| Google Sheets | 2020 | Improved compatibility with array formulas |

---

*Last updated: 2026-01-10*
