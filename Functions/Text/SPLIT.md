---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - text-manipulation
  - data-parsing
subTopics:
  - text-splitting
  - delimiter-parsing
  - string-tokenization
  - data-extraction
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - text-to-columns
  - string-split
  - delimiter-split
  - tokenize
tags:
  - text
  - parsing
  - splitting
  - delimiters
  - google-sheets
  - data-extraction
---

# SPLIT

## Description

SPLIT is a text function that divides a text string into separate parts based on a specified delimiter, returning the results as a horizontal array across multiple cells. Given a string like "apple,banana,cherry" and a delimiter of ",", SPLIT produces three separate values: "apple", "banana", and "cherry", each in its own cell. This is the programmatic equivalent of the "Text to Columns" feature, but as a formula that updates automatically when source data changes. SPLIT is fundamental for parsing imported data, extracting components from structured strings, and breaking apart concatenated values.

SPLIT is indispensable when working with data that arrives in delimited formats or when you need to extract specific parts from strings. Common use cases include parsing CSV-style data pasted into sheets, separating names into first and last name components, extracting parts of file paths or URLs, breaking apart address components, splitting date strings, tokenizing keywords or tags, and processing log file data. SPLIT is particularly powerful in Google Sheets, where it has been available for years, and is often combined with INDEX to extract specific parts from the split result.

There are several important gotchas when using SPLIT. First, SPLIT is native to Google Sheets but NOT available in Excel; Excel users must use TEXTSPLIT (Excel 365/2024) or the older Text to Columns feature. Second, SPLIT spills horizontally by default, requiring empty cells to the right; if cells are occupied, the formula may error or be truncated. Third, by default each character in the delimiter parameter is treated as a separate delimiter, not a multi-character delimiter; this behavior can be changed with the optional split_by_each parameter. Fourth, consecutive delimiters create empty strings in the output by default, though this can be controlled with the remove_empty parameter. Fifth, the number of output cells varies based on how many delimiters are in the source string.

Regarding platform availability, SPLIT is a Google Sheets native function that has been available since the early days of the platform. Microsoft Excel does NOT have a SPLIT function; instead, Excel 365 and Excel 2024 introduced TEXTSPLIT, which provides similar functionality with slightly different syntax. For cross-platform work, Google Sheets SPLIT formulas must be converted to TEXTSPLIT or alternative approaches when moving to Excel. Excel versions before 365 require manual Text to Columns operations or complex formula workarounds.

## Syntax

> [!f(x)] SPLIT Syntax (Google Sheets)
>
> ```
> =SPLIT(text, delimiter, [split_by_each], [remove_empty_text])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string to split. Can be a literal string in quotes, a cell reference, or an expression returning text. | Yes |
> | `delimiter` | The character(s) used to split the text. By default, each character in this string is a separate delimiter. | Yes |
> | `split_by_each` | TRUE (default) = each character in delimiter is a separate delimiter. FALSE = the entire delimiter string is used as a single delimiter. | No |
> | `remove_empty_text` | TRUE (default) = remove empty strings from the result. FALSE = keep empty strings created by consecutive delimiters. | No |

## Examples

### Example 1: Basic Comma Split
```
=SPLIT("apple,banana,cherry", ",")
```
**Result:** `apple | banana | cherry` (in three cells)

**Explanation:** The most common SPLIT usage separates comma-delimited values into individual cells. Each value becomes a separate cell to the right.

---

### Example 2: Split by Space (Words)
```
=SPLIT("Hello World Example", " ")
```
**Result:** `Hello | World | Example`

**Explanation:** Splitting by space breaks text into individual words. Useful for name parsing or word-level analysis.

---

### Example 3: Split Cell Reference
```
=SPLIT(A1, ",")
```
Where A1 contains "red,green,blue"

**Result:** `red | green | blue`

**Explanation:** SPLIT works with cell references, making it dynamic. Changes to A1 automatically update the split result.

---

### Example 4: Multiple Delimiter Characters
```
=SPLIT("a,b;c|d", ",;|")
```
**Result:** `a | b | c | d`

**Explanation:** By default, each character in the delimiter parameter is a separate delimiter. This splits on comma, semicolon, OR pipe.

---

### Example 5: Multi-Character Delimiter
```
=SPLIT("part1::part2::part3", "::", FALSE)
```
**Result:** `part1 | part2 | part3`

**Explanation:** Setting split_by_each to FALSE treats "::" as a single delimiter rather than splitting on each ":".

---

### Example 6: Handling Consecutive Delimiters
```
=SPLIT("a,,b,,,c", ",", TRUE, FALSE)
```
**Result:** `a | (empty) | b | (empty) | (empty) | c`

**Explanation:** With remove_empty_text as FALSE, consecutive delimiters create empty cells. This preserves positional information.

---

### Example 7: Removing Empty Results
```
=SPLIT("a,,b,,,c", ",", TRUE, TRUE)
```
**Result:** `a | b | c`

**Explanation:** With remove_empty_text as TRUE (default), empty strings from consecutive delimiters are removed.

---

### Example 8: Split and Index (Get Nth Part)
```
=INDEX(SPLIT(A1, ","), 1, 2)
```
Where A1 contains "first,second,third"

**Result:** `second`

**Explanation:** INDEX extracts a specific element from the SPLIT result. Column 2 gets the second part. This is the most common pattern for extracting one piece.

---

### Example 9: Split File Path
```
=SPLIT("/home/user/documents/file.txt", "/")
```
**Result:** `(empty) | home | user | documents | file.txt`

**Explanation:** Splitting a file path extracts each directory component. The leading empty string comes from the leading slash.

---

### Example 10: Split and Get Last Element
```
=INDEX(SPLIT(A1, "/"), 1, COUNTA(SPLIT(A1, "/")))
```
Where A1 contains a file path

**Result:** The filename (last component)

**Explanation:** COUNTA counts elements in the split result; INDEX with that number gets the last element. Useful for extracting filenames from paths.

---

### Example 11: Split Name into Parts
```
=SPLIT("John Michael Smith", " ")
```
**Result:** `John | Michael | Smith`

**Explanation:** Names can be split into first, middle, and last. Use INDEX to get specific parts: INDEX(SPLIT(A1, " "), 1, 1) for first name.

---

### Example 12: Split Email Address
```
=SPLIT("user@domain.com", "@")
```
**Result:** `user | domain.com`

**Explanation:** Splitting on "@" separates username from domain in email addresses.

---

### Example 13: Split URL into Components
```
=SPLIT("www.example.com/page/subpage", "/")
```
**Result:** `www.example.com | page | subpage`

**Explanation:** URLs can be parsed by splitting on "/" to extract domain and path components.

---

### Example 14: Parse Key-Value Pair
```
=INDEX(SPLIT("color=blue", "="), 1, 2)
```
**Result:** `blue`

**Explanation:** Split on "=" and index the second part to extract the value from a "key=value" format.

---

### Example 15: Split Tab-Separated Data
```
=SPLIT(A1, CHAR(9))
```
Where A1 contains tab-separated values

**Result:** Values split at tab characters

**Explanation:** CHAR(9) is the tab character. This splits TSV (tab-separated values) data into cells.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NAME?` | Using SPLIT in Excel (not available) | Use TEXTSPLIT in Excel 365/2024, or Text to Columns feature |
| `#REF!` | Result spills into cells that contain data | Clear cells to the right where split results will appear |
| `#VALUE!` | Empty text string | Handle empty strings with IF: =IF(A1="", "", SPLIT(A1, ",")) |
| `Too few results` | Using multi-char delimiter with split_by_each TRUE | Set split_by_each to FALSE for multi-character delimiters |
| `Too many results` | Using multi-char delimiter unintentionally | Each character is a delimiter by default; set split_by_each FALSE |
| `Missing empty cells` | Consecutive delimiters being removed | Set remove_empty_text to FALSE to preserve empty positions |

## Use Cases

### [[Data Import Parsing]]
- **Implementation**: Parse imported data that arrives as comma-separated, pipe-separated, or otherwise delimited strings. Convert single cells containing multiple values into proper columnar structure.
- **Business Application**: Processing exported data from other systems, parsing log files, handling data feeds, converting legacy formats, and cleaning up imported datasets.
- **Technical Details**: Identify the delimiter used in source data. Use INDEX(SPLIT(...), 1, n) to extract specific fields. Consider whether empty values should be preserved (set remove_empty_text accordingly).

### [[Name Parsing]]
- **Implementation**: Split full names into first name, middle name(s), and last name components. Handle varying numbers of parts gracefully.
- **Business Application**: CRM data cleaning, mailing list standardization, database normalization, and any application requiring separate first/last name fields.
- **Technical Details**: Names can have varying numbers of parts. Use INDEX(SPLIT(name, " "), 1, 1) for first name; last part requires counting elements. Consider that some names include prefixes, suffixes, or compound surnames.

### [[URL and Path Processing]]
- **Implementation**: Extract components from URLs, file paths, or hierarchical identifiers. Get domains, directories, filenames, or query parameters.
- **Business Application**: Web analytics, file management automation, API endpoint parsing, and any application working with structured identifiers.
- **Technical Details**: URLs split on "/" for path components, "?" for query strings, "&" for parameters, "=" for key-value pairs. File paths use "/" (Unix) or "\" (Windows). May need multiple SPLIT operations for full parsing.

### [[Tag and Keyword Processing]]
- **Implementation**: Parse comma-separated or otherwise delimited tags, keywords, or categories into individual items for analysis or matching.
- **Business Application**: Product categorization, content tagging systems, search keyword analysis, and social media hashtag processing.
- **Technical Details**: After SPLIT, use FILTER or other functions to find specific tags. COUNTA(SPLIT(...)) counts tags. JOIN(SPLIT(...)) can re-combine after filtering.

## Platform Differences

| Feature | Google Sheets | Excel |
|---------|---------------|-------|
| **Function Name** | SPLIT | TEXTSPLIT (Excel 365/2024) |
| **Availability** | All versions | Excel 365/2024 only |
| **Syntax** | SPLIT(text, delimiter, [split_by_each], [remove_empty]) | TEXTSPLIT(text, col_delimiter, [row_delimiter], ...) |
| **Default Behavior** | Each char is delimiter | Entire string is delimiter |
| **Empty Cell Handling** | remove_empty_text parameter | ignore_empty parameter |
| **Spill Direction** | Horizontal | Horizontal and Vertical options |
| **Legacy Excel** | Not available | Text to Columns (manual) |

**Converting Between Platforms:**

From Google Sheets SPLIT to Excel TEXTSPLIT:
```
Sheets: =SPLIT("a,b,c", ",")
Excel:  =TEXTSPLIT("a,b,c", ",")
```

For SPLIT with split_by_each FALSE:
```
Sheets: =SPLIT("a::b::c", "::", FALSE)
Excel:  =TEXTSPLIT("a::b::c", "::")  // Default behavior
```

**Excel Alternatives for Pre-365:**

1. **Text to Columns feature** (Data menu)
2. **Combination formulas:**
```excel
First part: =LEFT(A1, FIND(",", A1)-1)
Second part: =MID(A1, FIND(",", A1)+1, FIND(",", A1&",", FIND(",", A1)+1)-FIND(",", A1)-1)
```

## Tips and Best Practices

1. **Use INDEX for Single Values**: Instead of dealing with spilled arrays, use `=INDEX(SPLIT(text, ","), 1, N)` to get just the Nth element in a single cell.

2. **Understand split_by_each**: By default, "ab" as delimiter splits on "a" OR "b". Use FALSE as the third parameter when you want "ab" as a single delimiter.

3. **Handle Variable Lengths**: The number of results varies by input. Use COUNTA(SPLIT(...)) to count parts, and IFERROR for formulas that might exceed the available parts.

4. **Clear Destination Cells**: SPLIT spills horizontally. Ensure cells to the right are empty, or you'll get errors or truncated results.

5. **Get Last Element**: `=INDEX(SPLIT(A1, ","), 1, COUNTA(SPLIT(A1, ",")))` gets the last element regardless of how many exist.

6. **Preserve Empty Positions**: When data positions matter (like CSV columns), set remove_empty_text to FALSE to keep placeholder empties for consecutive delimiters.

7. **Chain with Other Functions**: SPLIT results work with FILTER, SORT, and other array functions: `=SORT(SPLIT(A1, ","))` alphabetizes the parts.

8. **Consider Excel Compatibility**: If workbooks will be used in Excel, document TEXTSPLIT equivalents or avoid SPLIT in favor of compatible approaches.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from SPLIT |
|----------|-------------|--------------------------|
| [[TEXTSPLIT]] | Excel's split function | Excel only; has row delimiter option |
| [[LEFT]]/[[MID]]/[[RIGHT]] | Extract by position | Position-based, not delimiter-based |
| [[REGEXEXTRACT]] | Extract by pattern | Pattern matching vs. delimiter splitting |
| [[Text to Columns]] | Excel feature | Manual/UI operation, not formula |

### Commonly Used Together

**[[INDEX]]** - Get specific element

*Extract one part from split result:*
```
=INDEX(SPLIT(A1, ","), 1, 2)
```
Get the second element from the split array.

**[[JOIN]]** - Reverse operation

*Combine split parts:*
```
=JOIN("-", SPLIT(A1, ","))
```
Split on comma, rejoin with different delimiter.

**[[FILTER]]** - Filter split results

*Select specific elements:*
```
=FILTER(SPLIT(A1, ","), SPLIT(A1, ",")<>"")
```
Filter out empty elements from split result.

**[[SORT]]** - Sort split results

*Alphabetize parts:*
```
=SORT(TRANSPOSE(SPLIT(A1, ",")))
```
Sort the split elements alphabetically (requires TRANSPOSE for vertical sort).

**[[COUNTA]]** - Count parts

*Determine number of elements:*
```
=COUNTA(SPLIT(A1, ","))
```
Count how many parts resulted from the split.

**[[TRIM]]** - Clean parts

*Remove whitespace:*
```
=ARRAYFORMULA(TRIM(SPLIT(A1, ",")))
```
Remove leading/trailing spaces from each part.

## Official Documentation

- **Google Sheets**: [SPLIT function](https://support.google.com/docs/answer/3094136)
- **Microsoft Excel**: [TEXTSPLIT function](https://support.microsoft.com/en-us/office/textsplit-function-b1ca414e-4c21-4ca0-b1b7-bdecace8a6e7) (Excel equivalent)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Google Sheets | Original | Available since launch |
| Google Sheets | 2016+ | Performance improvements |
| Microsoft Excel | 2022 | TEXTSPLIT introduced in Excel 365 |
| Excel 2024 | 2024 | TEXTSPLIT in perpetual license |

---

*Last updated: 2026-01-10*
