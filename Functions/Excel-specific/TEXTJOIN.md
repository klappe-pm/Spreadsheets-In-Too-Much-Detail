---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
- excel-specific
subTopics:
- text-concatenation
- string-joining
- data-formatting
- text-manipulation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Text Join
- Join Text
- Concatenate with Delimiter
- String Concatenation
tags:
- text-functions
- concatenation
- delimiters
- data-formatting
- string-manipulation
---

# TEXTJOIN

## Description

**TEXTJOIN** combines text from multiple cells or ranges into a single string, inserting a specified delimiter between each value. It's the modern, powerful replacement for chaining CONCATENATE or ampersand (&) operations. Instead of writing `=A1&", "&A2&", "&A3&", "&A4`, you simply write `=TEXTJOIN(", ", TRUE, A1:A4)`. The function handles ranges of any size, making it perfect for building comma-separated lists, creating compound keys, or assembling formatted strings from scattered data.

The killer feature of TEXTJOIN is the `ignore_empty` parameter. When set to TRUE, empty cells are skipped entirely—no awkward double delimiters ("John,,Mary") or trailing commas. This solves one of the most frustrating problems in text concatenation: gracefully handling incomplete data. Set to FALSE if you explicitly want placeholders for empty values, but TRUE is the correct choice for most real-world use cases.

**Critical limitation: the 32,767 character limit.** TEXTJOIN returns a #VALUE! error if the resulting string exceeds 32,767 characters—Excel and Sheets' maximum cell text length. For large datasets, this limit can be reached surprisingly quickly. If you're joining thousands of values, you may need to split the operation across multiple cells or use a different approach (like exporting to a text file via VBA/Apps Script).

TEXTJOIN was introduced in Excel 2019 and Excel 365, making it relatively new compared to older text functions. Google Sheets has supported it since 2016. Users on Excel 2016 or earlier don't have access to TEXTJOIN and must use workarounds like CONCATENATE, the ampersand operator, or complex array formulas. When sharing spreadsheets with users on older Excel versions, be aware that TEXTJOIN formulas will show as #NAME? errors.

## Syntax

> [!f(x)] TEXTJOIN Syntax
>
> ```
> =TEXTJOIN(delimiter, ignore_empty, text1, [text2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `delimiter` | Yes | The character(s) to insert between each text value. Use "" for no delimiter. Can be a single character (","), multiple characters (" - "), or a cell reference. |
| `ignore_empty` | Yes | TRUE to skip empty cells (recommended for most cases); FALSE to include empty cells as zero-length strings (resulting in consecutive delimiters). |
| `text1` | Yes | First text value, cell, or range to join. |
| `text2, ...` | No | Additional text values, cells, or ranges. Up to 252 arguments in Excel, unlimited ranges in Google Sheets. |

### Return Value

Returns a single text string containing all values joined by the delimiter. Empty if all inputs are empty and ignore_empty is TRUE. Returns #VALUE! if result exceeds 32,767 characters.

## Examples

> [!f(x)] TEXTJOIN Examples

### Example 1: Basic Comma-Separated List
```
=TEXTJOIN(", ", TRUE, A1:A5)
```
Where A1:A5 contains: Apple, Orange, Banana, Grape, Mango

**Result:** `Apple, Orange, Banana, Grape, Mango`

**Explanation:** The most common use case—creating a comma-separated list from a column of values. TRUE ignores any empty cells in the range.

---

### Example 2: Handling Empty Cells (ignore_empty = TRUE)
```
=TEXTJOIN(", ", TRUE, A1:A5)
```
Where A1="John", A2="", A3="Mary", A4="", A5="Bob"

**Result:** `John, Mary, Bob`

**Explanation:** Empty cells are skipped entirely—no extra commas or spaces. This is why TRUE for ignore_empty is almost always preferred.

---

### Example 3: Keeping Empty Cells (ignore_empty = FALSE)
```
=TEXTJOIN(", ", FALSE, A1:A5)
```
Where A1="John", A2="", A3="Mary", A4="", A5="Bob"

**Result:** `John, , Mary, , Bob`

**Explanation:** With FALSE, empty cells become zero-length strings, but the delimiter still appears. Use this when you need position-sensitive output or placeholders.

---

### Example 4: No Delimiter (Concatenation)
```
=TEXTJOIN("", TRUE, B1:B10)
```
**Result:** All values concatenated with no separator

**Explanation:** Empty string delimiter effectively concatenates all values together. Useful for building codes, IDs, or merging split text.

---

### Example 5: Multi-Character Delimiter
```
=TEXTJOIN(" | ", TRUE, C1:C4)
```
Where C1:C4 contains: Product, Price, Quantity, Total

**Result:** `Product | Price | Quantity | Total`

**Explanation:** Delimiter can be any text, not just single characters. Great for creating formatted headers or visual separators.

---

### Example 6: Line Break Delimiter
```
=TEXTJOIN(CHAR(10), TRUE, D1:D5)
```
**Result:** Each value on a new line (within one cell)

**Explanation:** CHAR(10) is the line break character. Combined with cell wrap text formatting, this puts each value on its own line within a single cell. Excellent for creating formatted lists.

---

### Example 7: Building Email Lists
```
=TEXTJOIN("; ", TRUE, EmailColumn)
```
Where EmailColumn contains multiple email addresses

**Result:** `john@example.com; mary@example.com; bob@example.com`

**Explanation:** Creating semicolon-separated email lists for mail merge, Outlook distribution, or CRM imports. Empty cells (people without email) are automatically skipped.

---

### Example 8: Multiple Ranges
```
=TEXTJOIN(", ", TRUE, A1:A3, C1:C3, E1:E3)
```
**Result:** Values from all three ranges combined into one list

**Explanation:** TEXTJOIN can combine multiple non-contiguous ranges. Useful when data is spread across different areas of the worksheet.

---

### Example 9: Mixing Cells and Text
```
=TEXTJOIN(" ", TRUE, A1, "of", B1, "in", C1)
```
Where A1="John", B1="Sales", C1="Chicago"

**Result:** `John of Sales in Chicago`

**Explanation:** Mix cell references with literal text to build formatted sentences. The delimiter (space) goes between ALL elements, not just cells.

---

### Example 10: Creating a Compound Key
```
=TEXTJOIN("-", TRUE, A2, B2, C2)
```
Where A2="2024", B2="Q1", C2="001"

**Result:** `2024-Q1-001`

**Explanation:** Build unique identifiers by combining multiple fields. Excellent for creating keys for VLOOKUP or INDEX/MATCH across multiple criteria.

---

### Example 11: Conditional Text Joining with IF
```
=TEXTJOIN(", ", TRUE, IF(B2:B10="Active", A2:A10, ""))
```
**Result:** Comma-separated list of names where status is "Active"

**Explanation:** Combine with IF to conditionally include values. This array formula (may need Ctrl+Shift+Enter in older Excel) filters values before joining. Empty strings from non-matches are ignored due to TRUE.

---

### Example 12: Joining Numbers with Formatting
```
=TEXTJOIN(" + ", TRUE, TEXT(A1:A5, "#,##0"))
```
Where A1:A5 contains: 1000, 2500, 750, 1200, 500

**Result:** `1,000 + 2,500 + 750 + 1,200 + 500`

**Explanation:** Use TEXT function to format numbers before joining. Without TEXT, numbers would display without thousand separators.

---

### Example 13: Building SQL IN Clause
```
="SELECT * FROM Products WHERE ID IN ('" & TEXTJOIN("','", TRUE, A2:A10) & "')"
```
Where A2:A10 contains product IDs: P001, P002, P003, etc.

**Result:** `SELECT * FROM Products WHERE ID IN ('P001','P002','P003')`

**Explanation:** Useful for generating SQL queries from spreadsheet data. The TEXTJOIN creates the comma-separated quoted list inside the IN clause.

---

### Example 14: Delimiter from Cell Reference
```
=TEXTJOIN(G1, TRUE, A1:A5)
```
Where G1 contains the desired delimiter (perhaps "," or "; " or " | ")

**Result:** Values joined by whatever delimiter is in G1

**Explanation:** Dynamic delimiter that users can change without editing the formula. Great for user-configurable reports.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Result exceeds 32,767 characters | Split the range into smaller pieces and join in multiple cells, or use alternative methods for very large datasets. |
| `#NAME?` | Excel version doesn't support TEXTJOIN (pre-2019) | Use CONCATENATE, CONCAT, or ampersand workarounds. Consider upgrading to Excel 365. |
| `#VALUE!` | Array formula issues in older Excel | Enter with Ctrl+Shift+Enter for array formulas with IF conditions. |
| Extra delimiters | ignore_empty set to FALSE with empty cells | Change to TRUE unless you specifically need placeholders for empty positions. |
| Numbers appear unformatted | TEXTJOIN converts numbers to text directly | Wrap in TEXT() with format code: `TEXTJOIN(", ", TRUE, TEXT(A1:A5, "#,##0"))` |
| Missing values | Cells contain spaces instead of being truly empty | Use TRIM before joining, or clean data first. Spaces are not empty. |

## Use Cases

### [[Creating Mailing Labels and Address Lines]]

**Scenario:** Convert structured address data (name, street, city, state, zip in separate columns) into formatted address lines for labels or mail merge.

**Implementation:**
```
Full address:
=TEXTJOIN(", ", TRUE, A2, B2, C2 & " " & D2 & " " & E2)

Or multi-line format:
=A2 & CHAR(10) & TEXTJOIN(CHAR(10), TRUE, B2, C2 & ", " & D2 & " " & E2)
```
Where columns are: Name, Street, City, State, ZIP

**Business Application:** Marketing teams create mailing lists, HR prepares employee directories, and customer service formats addresses for shipping labels. TEXTJOIN handles missing apartment numbers or suite information gracefully.

**Technical Details:** Use CHAR(10) for line breaks within a cell (ensure cell has wrap text enabled). Consider TRIM() on inputs to remove extra spaces from source data.

---

### [[Building Dynamic Query Strings]]

**Scenario:** Generate filter strings, SQL queries, or URL parameters from selected values in a spreadsheet.

**Implementation:**
```
URL parameters:
="https://api.example.com/search?" & TEXTJOIN("&", TRUE, IF(B2:B10<>"", A2:A10&"="&B2:B10, ""))

WHERE clause:
="WHERE status IN ('" & TEXTJOIN("','", TRUE, FILTER(A2:A20, B2:B20="Selected")) & "')"
```

**Business Application:** Data analysts build API calls, database queries, or report filters directly from spreadsheet criteria. Sales teams generate custom product URLs for customers.

**Technical Details:** URL parameters need encoding for special characters—TEXTJOIN handles the joining, but you may need ENCODEURL for values with spaces or special characters.

---

### [[Creating Readable Audit Trails]]

**Scenario:** Combine multiple timestamps, actions, and usernames into a single audit history field for each record.

**Implementation:**
```
=TEXTJOIN(CHAR(10), TRUE,
    IF(B2:B20<>"",
        TEXT(A2:A20, "YYYY-MM-DD HH:MM") & " - " & B2:B20 & " - " & C2:C20,
    ""))
```
Where columns are: Timestamp, Action, Username

**Business Application:** Compliance teams create readable change logs, IT tracks system access history, and project managers compile activity summaries—all consolidated into single cells for easy viewing.

**Technical Details:** Line breaks (CHAR(10)) create multi-line entries. Filter conditions with IF exclude blank rows. TEXT function formats timestamps consistently.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2019, Excel 365, and Excel for Mac 2019+. NOT available in Excel 2016 or earlier.
- **Specific Behavior:** Maximum 252 text arguments. Returns #NAME? error in older versions.
- **Character limit:** 32,767 characters maximum result.

### Google Sheets
- **Availability:** Native support since 2016. Available in all versions.
- **Specific Behavior:** Functionally identical. No practical argument limit.
- **Character limit:** Same 32,767 character cell limit applies.

### Key Differences
- Excel introduced TEXTJOIN later, so compatibility with older Excel versions is a real concern.
- Google Sheets users have had TEXTJOIN longer and may expect it to work in shared Excel files.
- When sharing between platforms, test to ensure TEXTJOIN formulas display correctly.

## Tips and Best Practices

1. **Almost always use TRUE for ignore_empty:** Unless you specifically need position-aware output (rare), TRUE creates cleaner results and handles incomplete data gracefully.

2. **Use CHAR(10) for multi-line results:** Combined with cell wrap text, you can create formatted multi-line content in single cells. Remember to set row height appropriately.

3. **Combine with IF for conditional joining:** `=TEXTJOIN(", ", TRUE, IF(condition, values, ""))` filters before joining. This is an array formula in older Excel.

4. **Format numbers before joining:** TEXTJOIN converts numbers to text directly without formatting. Use TEXT() to control number appearance.

5. **Watch the character limit:** For large datasets, the 32,767 character limit can cause #VALUE! errors. Test with full data before deploying.

6. **Store delimiters in cells for flexibility:** Using a cell reference for the delimiter lets users change output format without editing formulas.

7. **Use SUBSTITUTE to modify after joining:** Need to clean up the result? `=SUBSTITUTE(TEXTJOIN(...), "old", "new")` for post-processing.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CONCAT]] | Concatenates without delimiter | When you just need to join values with no separator and no empty handling |
| [[CONCATENATE]] | Legacy concatenation (being deprecated) | Only for backward compatibility with very old spreadsheets |
| [[JOIN]] | Google Sheets equivalent with simpler syntax | Google Sheets only; TEXTJOIN is preferred for cross-platform |
| [[TEXTSPLIT]] | Reverse operation—splits text by delimiter | When you need to break a delimited string back into parts |

### Commonly Used Together

**[[IF]]** - Conditional Inclusion

*Combined for filtered joining:*
```
=TEXTJOIN(", ", TRUE, IF(B2:B10="Active", A2:A10, ""))
```
Only include values where corresponding column meets criteria.

---

**[[FILTER]]** - Dynamic Array Filtering

*Combined for cleaner conditional joining:*
```
=TEXTJOIN(", ", TRUE, FILTER(A2:A20, B2:B20="Yes"))
```
FILTER returns only matching values, TEXTJOIN combines them. Cleaner than IF approach in modern Excel.

---

**[[TEXT]]** - Number Formatting

*Combined for formatted numeric lists:*
```
=TEXTJOIN(", ", TRUE, TEXT(A1:A5, "$#,##0.00"))
```
Format numbers as currency, dates, or other formats before joining.

---

**[[CHAR]]** - Special Characters

*Combined for multi-line output:*
```
=TEXTJOIN(CHAR(10), TRUE, A1:A5)
```
CHAR(10) = line break, CHAR(9) = tab. Create formatted multi-line cells.

---

**[[SUBSTITUTE]]** - Text Replacement

*Combined for post-processing:*
```
=SUBSTITUTE(TEXTJOIN(", ", TRUE, A1:A5), "  ", " ")
```
Clean up double spaces or replace specific text in the joined result.

---

**[[UNIQUE]]** - Remove Duplicates

*Combined for unique value lists:*
```
=TEXTJOIN(", ", TRUE, UNIQUE(A2:A100))
```
Join only unique values from a range. Requires Excel 365 or Google Sheets.

## Official Documentation

- **Microsoft Excel:** [TEXTJOIN function](https://support.microsoft.com/en-us/office/textjoin-function-357b449a-ec91-49d0-80c3-0e8fc845691c)
- **Google Sheets:** [TEXTJOIN function](https://support.google.com/docs/answer/7013992)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2019 | Built-in function | First generally available version |
| Excel 365 | Built-in function | Included in subscription versions |
| Excel Online | Supported | Full functionality in browser |
| Excel 2016 & earlier | NOT AVAILABLE | Use CONCATENATE or custom functions |
| Google Sheets | 2016 | Native support since early adoption |

---

*Last updated: 2026-01-10*
