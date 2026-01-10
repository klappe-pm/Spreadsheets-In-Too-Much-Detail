---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - text-manipulation
  - string-functions
subTopics:
  - text-joining
  - array-concatenation
  - delimiter-joining
  - string-combination
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - array-join
  - text-concatenation
  - delimiter-concatenation
  - combine-array
tags:
  - text
  - concatenation
  - arrays
  - delimiters
  - google-sheets
  - excel
---

# JOIN

## Description

JOIN is a text function that concatenates the elements of one or more one-dimensional arrays using a specified delimiter between each element. Unlike basic concatenation functions that simply stick values together, JOIN allows you to specify what separator appears between values, whether a comma, space, hyphen, line break, or any other character or string. The function takes a delimiter as the first parameter followed by one or more arrays or ranges to join, and returns a single text string with all values combined and properly separated.

JOIN is invaluable when you need to combine multiple values into a single text string with consistent separators. Common use cases include creating comma-separated lists from ranges, building search strings by combining keywords, generating formatted addresses from component parts, creating concatenated keys from multiple fields, producing readable lists for reports, and transforming vertical data into horizontal comma-delimited strings. JOIN is particularly powerful in Google Sheets where it has long been available and is often used with FILTER, UNIQUE, and other array functions to create dynamic text summaries.

There are several important gotchas with JOIN. First, JOIN is a Google Sheets native function that was later added to Excel; the Excel equivalent TEXTJOIN offers more features like ignoring empty cells. Second, JOIN concatenates all values including empty cells, which can create multiple consecutive delimiters if your source range has blanks. Third, the function works with one-dimensional arrays (single rows or columns) by default; two-dimensional arrays are flattened. Fourth, numeric values are converted to text, but formatting (currency symbols, decimal places) is not preserved. Fifth, the delimiter itself can be a multi-character string, enabling complex separators like ", " (comma-space) or even line breaks using CHAR(10).

Regarding platform availability, JOIN is native to Google Sheets and has been available since the early days of the platform. In Microsoft Excel, JOIN is NOT available; instead, Excel uses TEXTJOIN (introduced in Excel 2016/365), which offers additional functionality like the option to ignore empty cells. For cross-platform compatibility, Google Sheets users should be aware that their JOIN formulas will need to be converted to TEXTJOIN when moving to Excel. The syntax differs: JOIN(delimiter, array) in Sheets versus TEXTJOIN(delimiter, ignore_empty, array) in Excel.

## Syntax

> [!f(x)] JOIN Syntax (Google Sheets)
>
> ```
> =JOIN(delimiter, value_or_array1, [value_or_array2, ...])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `delimiter` | The character or string to place between each value in the result. Can be empty string "" for direct concatenation without separators. | Yes |
> | `value_or_array1` | The first value, cell reference, or range to include in the joined string. | Yes |
> | `value_or_array2, ...` | Additional values, cell references, or ranges to include. Multiple arguments are processed in order. | No |

## Examples

### Example 1: Basic Range Join with Comma
```
=JOIN(", ", A1:A5)
```
Where A1:A5 contains: Apple, Banana, Cherry, Date, Elderberry

**Result:** `Apple, Banana, Cherry, Date, Elderberry`

**Explanation:** The simplest JOIN takes a range and combines all values with the specified delimiter (comma-space in this case). Creates a readable list from columnar data.

---

### Example 2: Join with No Delimiter
```
=JOIN("", A1:A3)
```
Where A1:A3 contains: A, B, C

**Result:** `ABC`

**Explanation:** Using an empty string as delimiter concatenates values directly with no separator. Useful for building codes or identifiers from component parts.

---

### Example 3: Join with Custom Separator
```
=JOIN(" - ", A1:A4)
```
Where A1:A4 contains: Red, Green, Blue, Yellow

**Result:** `Red - Green - Blue - Yellow`

**Explanation:** Any string can be used as a delimiter. Here " - " (space-hyphen-space) creates a visually clear separation between items.

---

### Example 4: Join with Line Break
```
=JOIN(CHAR(10), A1:A5)
```
Where A1:A5 contains addresses

**Result:** Multi-line text with each value on its own line

**Explanation:** CHAR(10) is the line break character. The result displays across multiple lines when the cell has "Wrap text" enabled. Great for creating multi-line address blocks.

---

### Example 5: Join Multiple Ranges
```
=JOIN(", ", A1:A3, B1:B3)
```
Where A1:A3 = 1, 2, 3 and B1:B3 = 4, 5, 6

**Result:** `1, 2, 3, 4, 5, 6`

**Explanation:** JOIN can accept multiple range arguments, processing them in order. All values from both ranges are combined with the delimiter.

---

### Example 6: Join Row Values
```
=JOIN(" | ", A1:E1)
```
Where A1:E1 contains header names

**Result:** `Column1 | Column2 | Column3 | Column4 | Column5`

**Explanation:** JOIN works equally well with row ranges as column ranges, creating horizontal-to-text conversion.

---

### Example 7: Join Numbers
```
=JOIN("-", 1, 2, 3, 4)
```
**Result:** `1-2-3-4`

**Explanation:** JOIN can take literal values as arguments, not just ranges. Numbers are automatically converted to text.

---

### Example 8: Join with FILTER Results
```
=JOIN(", ", FILTER(A:A, B:B="Active"))
```
**Result:** Comma-separated list of all values in A where B equals "Active"

**Explanation:** JOIN works excellently with FILTER to create dynamic text lists from filtered data. Common pattern in Google Sheets.

---

### Example 9: Join UNIQUE Values
```
=JOIN(", ", UNIQUE(A1:A100))
```
**Result:** Comma-separated list of unique values from the range

**Explanation:** Combining JOIN with UNIQUE creates a deduplicated list. Useful for summary displays and data validation lists.

---

### Example 10: Building Email Recipients
```
=JOIN("; ", A2:A20)
```
Where A2:A20 contains email addresses

**Result:** `email1@test.com; email2@test.com; email3@test.com...`

**Explanation:** Email systems often expect semicolon-separated recipient lists. JOIN creates the proper format from a column of addresses.

---

### Example 11: Creating Search Query
```
=JOIN("+", SPLIT(A1, " "))
```
Where A1 contains "hello world test"

**Result:** `hello+world+test`

**Explanation:** First SPLIT breaks text into words, then JOIN recombines with "+" for URL-style search query format.

---

### Example 12: Join with Conditional Values
```
=JOIN(", ", IF(B1:B10="Yes", A1:A10, ""))
```
**Result:** Comma-separated list of A values where B equals "Yes", but may include empty delimiters

**Explanation:** Array IF can filter values, but JOIN includes empties. For cleaner results, use FILTER instead.

---

### Example 13: Building File Path
```
=JOIN("/", "folder", A1, B1, "file.txt")
```
Where A1="subfolder", B1="2024"

**Result:** `folder/subfolder/2024/file.txt`

**Explanation:** JOIN constructs paths by combining folder names with "/" separator. Works for file paths, URLs, or hierarchical identifiers.

---

### Example 14: Creating SQL IN Clause
```
="IN ('" & JOIN("', '", A1:A5) & "')"
```
Where A1:A5 contains IDs

**Result:** `IN ('ID1', 'ID2', 'ID3', 'ID4', 'ID5')`

**Explanation:** Building SQL queries often requires formatted lists. JOIN creates the comma-separated values; surrounding text adds SQL syntax.

---

### Example 15: Handling Empty Cells
```
=JOIN(", ", FILTER(A1:A10, A1:A10<>""))
```
**Result:** Comma-separated list excluding empty cells

**Explanation:** JOIN includes empty cells by default. Wrapping the range in FILTER(...<>"") removes blanks before joining, preventing consecutive delimiters.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NAME?` | Using JOIN in Excel (not available) | Use TEXTJOIN in Excel instead |
| `#VALUE!` | Invalid delimiter type | Ensure delimiter is text; numbers work but should be converted |
| `#REF!` | Referenced range includes deleted cells | Update range reference to valid cells |
| `Consecutive delimiters` | Empty cells in source range | Use FILTER to remove blanks: JOIN(", ", FILTER(range, range<>"")) |
| `Very long string` | Large range with many values | Google Sheets has a cell content limit (50,000 chars); truncate if needed |
| `Numbers unformatted` | Numeric values lose formatting | Use TEXT() on numbers before JOIN if formatting matters |

## Use Cases

### [[Creating Readable Lists from Data]]
- **Implementation**: Use JOIN to convert columnar data into human-readable comma-separated or bulleted lists for reports, summaries, and presentations.
- **Business Application**: Executive summaries, dashboard text widgets, automated report generation, email body text, and document automation all benefit from converting data ranges into readable text lists.
- **Technical Details**: Choose appropriate delimiter for context: ", " for inline lists, CHAR(10) for vertical lists, " | " for visual separation. Consider wrapping in FILTER to remove empty values and UNIQUE to deduplicate.

### [[Building Dynamic Query Parameters]]
- **Implementation**: Construct database query strings, API parameters, or search queries by joining values with appropriate separators like "&", "=", or "+".
- **Business Application**: Dynamic data fetching, API integration, search URL construction, and programmatic query building in spreadsheet-based automation systems.
- **Technical Details**: Use JOIN with appropriate delimiters for target system (spaces for some searches, "+" for URLs, ";" for some databases). Consider URL encoding for special characters when building URLs.

### [[Address and Contact Formatting]]
- **Implementation**: Combine address components (street, city, state, zip) or contact information (name, title, company) into properly formatted strings with appropriate separators.
- **Business Application**: Mailing label generation, CRM contact display, address book exports, and document mail merge preparation.
- **Technical Details**: Use ", " for inline addresses, CHAR(10) for multi-line labels. Handle missing components by filtering blanks. Consider international address format variations.

### [[Data Export Preparation]]
- **Implementation**: Convert range data into delimited text strings for export, copy-paste operations, or text file generation.
- **Business Application**: Creating CSV-like output without file export, preparing data for systems expecting text input, generating clipboard-ready content, and creating text summaries for emails.
- **Technical Details**: Use "," for CSV-style, TAB (CHAR(9)) for tab-delimited, or custom delimiters as needed. Consider quoting values containing the delimiter character.

## Platform Differences

| Feature | Google Sheets | Excel |
|---------|---------------|-------|
| **Function Name** | JOIN | TEXTJOIN (JOIN not available) |
| **Availability** | All versions | Excel 2016/365+ (TEXTJOIN) |
| **Syntax** | JOIN(delimiter, arrays) | TEXTJOIN(delimiter, ignore_empty, arrays) |
| **Ignore Empty Option** | No (use FILTER workaround) | Yes (built-in second parameter) |
| **Multiple Arrays** | Yes | Yes |
| **Max Arguments** | Multiple | Multiple |
| **Max Result Length** | ~50,000 characters | 32,767 characters |

**Converting Between Platforms:**

From Google Sheets JOIN to Excel TEXTJOIN:
```
Sheets: =JOIN(", ", A1:A10)
Excel:  =TEXTJOIN(", ", TRUE, A1:A10)
```

The Excel TEXTJOIN second parameter (TRUE) tells it to ignore empty cells, replicating the common Sheets pattern of using JOIN with FILTER.

**Google Sheets Workaround for Empty Cells:**
```
=JOIN(", ", FILTER(A1:A10, A1:A10<>""))
```

**Excel Alternative:**
```
=TEXTJOIN(", ", TRUE, A1:A10)
```
The TRUE parameter handles empty cells automatically.

## Tips and Best Practices

1. **Filter Empty Cells**: Since JOIN includes empty cells (creating consecutive delimiters), wrap in FILTER when blanks may exist: `=JOIN(", ", FILTER(range, range<>""))`.

2. **Use CHAR(10) for Multi-Line**: Create multi-line text in a single cell with `=JOIN(CHAR(10), range)`. Remember to enable "Wrap text" formatting on the cell.

3. **Combine with UNIQUE for Deduplicated Lists**: `=JOIN(", ", UNIQUE(range))` creates a list of distinct values, useful for summaries.

4. **Combine with SORT for Ordered Lists**: `=JOIN(", ", SORT(range))` creates an alphabetically or numerically sorted list.

5. **Format Numbers Before Joining**: JOIN converts numbers to plain text. Use TEXT() if you need specific formatting: `=JOIN(", ", ARRAYFORMULA(TEXT(range, "$#,##0")))`.

6. **Consider Excel Compatibility**: If sharing with Excel users, note that JOIN won't work. Document the TEXTJOIN equivalent or use a compatible approach.

7. **Watch String Length**: Very large ranges can create strings exceeding cell limits. Monitor result length for large datasets.

8. **Use for Dynamic Labels**: JOIN dynamic arrays create labels that update automatically, useful for dashboard annotations and chart titles.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from JOIN |
|----------|-------------|-------------------------|
| [[TEXTJOIN]] | Excel's delimiter joining function | Has ignore_empty parameter; Excel only |
| [[CONCATENATE]] | Joins text strings | No delimiter parameter; joins directly |
| [[CONCAT]] | Modern concatenation | Accepts ranges but no delimiter support |
| [[& Operator]] | Inline concatenation | Manual delimiter insertion required |

### Commonly Used Together

**[[FILTER]]** - Filter before joining

*Remove empty cells or apply criteria:*
```
=JOIN(", ", FILTER(A:A, A:A<>""))
```
Filter out blanks before joining for clean results.

**[[UNIQUE]]** - Deduplicate before joining

*Create list of distinct values:*
```
=JOIN(", ", UNIQUE(A1:A100))
```
Produces comma-separated list without duplicates.

**[[SORT]]** - Sort before joining

*Create ordered list:*
```
=JOIN(", ", SORT(A1:A10))
```
Alphabetically or numerically sorted result.

**[[SPLIT]]** - Inverse operation

*Break apart what JOIN creates:*
```
=SPLIT("A,B,C", ",")
```
SPLIT breaks delimited strings; JOIN creates them.

**[[CHAR]]** - Special delimiter characters

*Use line breaks or tabs:*
```
=JOIN(CHAR(10), A1:A5)
```
CHAR(10) = line break, CHAR(9) = tab.

**[[ARRAYFORMULA]]** - Process arrays

*Format values before joining:*
```
=JOIN(", ", ARRAYFORMULA(TEXT(A1:A10, "0.00")))
```
Format numbers as text before joining.

## Official Documentation

- **Google Sheets**: [JOIN function](https://support.google.com/docs/answer/3094077)
- **Microsoft Excel**: [TEXTJOIN function](https://support.microsoft.com/en-us/office/textjoin-function-357b449a-ec91-49d0-80c9-0c076023f0ad) (Excel equivalent)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Google Sheets | Original | Available since launch |
| Google Sheets | 2016+ | Performance improvements for large arrays |
| Microsoft Excel | 2016 | TEXTJOIN introduced (not JOIN) |
| Excel 365 | 2018+ | TEXTJOIN dynamic array support |

---

*Last updated: 2026-01-10*
