---
categories:
  - spreadsheet-functions
subCategories:
  - excel
topics:
  - text-manipulation
  - text-parsing
subTopics:
  - delimiter-splitting
  - text-to-columns
  - array-functions
  - dynamic-arrays
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - text-split
  - split-text
  - text-to-array
  - delimiter-split
tags:
  - text
  - splitting
  - parsing
  - arrays
  - dynamic-arrays
  - excel-365
  - excel-only
---

# TEXTSPLIT

## Description

TEXTSPLIT is a powerful Excel 365-exclusive dynamic array function that splits a text string into an array of substrings based on specified column and/or row delimiters. Unlike the traditional Text to Columns feature which modifies data in place, TEXTSPLIT is a formula that creates a dynamic array output, automatically spilling results across multiple cells while leaving the original data intact. This function can split text horizontally across columns, vertically down rows, or both simultaneously to create a two-dimensional array, making it exceptionally versatile for parsing complex structured text data.

The function is particularly valuable for parsing structured text data such as CSV values within a cell, multi-line text entries, delimited lists, key-value pairs, and any text format where components are separated by consistent delimiters. TEXTSPLIT can handle multiple delimiters simultaneously (treating any of them as split points), optionally ignore empty values created by consecutive delimiters, perform case-insensitive delimiter matching, and apply padding to ensure rectangular array output. The resulting dynamic array integrates seamlessly with other Excel functions like INDEX, FILTER, SORT, and UNIQUE for further processing.

Several critical aspects require attention when working with TEXTSPLIT. First, the function returns a dynamic array that "spills" into adjacent cells—these cells must be empty or a #SPILL! error occurs. Second, col_delimiter and row_delimiter parameters can each accept multiple delimiters as arrays, treating any of them as split points. Third, when ignore_empty is TRUE, consecutive delimiters don't create empty array elements, which is useful for inconsistently formatted data but may shift element positions. Fourth, the pad_with parameter fills jagged arrays with a specified value to create rectangular output, essential when row lengths vary. Fifth, empty delimiters or no matches return the original text as a single-element array.

TEXTSPLIT is available exclusively in Microsoft Excel 365 (Microsoft 365 subscription) and Excel for the web, introduced in August 2022. It is NOT available in Excel 2019, Excel 2016, or earlier perpetual license versions. Google Sheets has a SPLIT function with similar but more limited functionality—it splits by column delimiter only and doesn't support row delimiters or many of TEXTSPLIT's advanced features. When building cross-platform spreadsheets, either provide alternative formulas using Google Sheets' SPLIT function (for simpler cases), or document that advanced TEXTSPLIT features require Excel 365.

## Syntax

> [!f(x)] TEXTSPLIT Syntax
>
> ```excel
> =TEXTSPLIT(text, [col_delimiter], [row_delimiter], [ignore_empty], [match_mode], [pad_with])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string to split. Can be a cell reference, literal text in quotes, or a formula returning text. | Yes |
> | `col_delimiter` | The delimiter(s) that mark where to split text into columns (horizontal splitting). Can be a single character, multiple characters, or an array of delimiters. Omit or use empty string "" to not split by columns. | No |
> | `row_delimiter` | The delimiter(s) that mark where to split text into rows (vertical splitting). Can be a single character, multiple characters, or an array of delimiters. Omit to not split by rows. | No |
> | `ignore_empty` | TRUE to ignore consecutive delimiters (don't create empty elements); FALSE (default) to create empty elements for each delimiter. | No |
> | `match_mode` | 0 = case-sensitive matching (default); 1 = case-insensitive matching. | No |
> | `pad_with` | Value to use for padding when rows have different numbers of columns. Default is #N/A. Specify "" for empty string, 0, or any value. | No |

## Examples

### Example 1: Basic Comma-Separated Split
```excel
=TEXTSPLIT(A1, ",")
```
Where A1 contains "Apple,Banana,Cherry"

**Result:** `Apple` | `Banana` | `Cherry` (spills across three columns)

**Explanation:** The simplest form of TEXTSPLIT divides text at each comma, creating a horizontal array. Each segment becomes a separate cell in the spill range.

---

### Example 2: Split with Space Delimiter
```excel
=TEXTSPLIT(A1, " ")
```
Where A1 contains "The quick brown fox"

**Result:** `The` | `quick` | `brown` | `fox` (four columns)

**Explanation:** Using a space as delimiter splits the sentence into individual words. Each word occupies its own cell in the resulting array.

---

### Example 3: Split into Rows (Vertical Split)
```excel
=TEXTSPLIT(A1, , CHAR(10))
```
Where A1 contains "Line 1[newline]Line 2[newline]Line 3"

**Result:**
```
Line 1
Line 2
Line 3
```
(spills down three rows)

**Explanation:** Using CHAR(10) (line feed) as the row_delimiter splits multi-line text into separate rows. Note the col_delimiter is omitted (just a comma before row_delimiter).

---

### Example 4: Two-Dimensional Split (Rows and Columns)
```excel
=TEXTSPLIT(A1, ",", ";")
```
Where A1 contains "A1,B1,C1;A2,B2,C2;A3,B3,C3"

**Result:**
```
A1 | B1 | C1
A2 | B2 | C2
A3 | B3 | C3
```
(3x3 array)

**Explanation:** Semicolons split into rows, commas split into columns within each row. This creates a complete two-dimensional array from a single text string.

---

### Example 5: Multiple Column Delimiters
```excel
=TEXTSPLIT(A1, {",", ";", "|"})
```
Where A1 contains "Apple,Banana;Cherry|Date"

**Result:** `Apple` | `Banana` | `Cherry` | `Date` (four columns)

**Explanation:** When col_delimiter is an array, ANY of the specified characters acts as a split point. Useful for parsing data with inconsistent delimiters.

---

### Example 6: Ignore Empty Values
```excel
=TEXTSPLIT(A1, ",", , TRUE)
```
Where A1 contains "Apple,,Banana,,,Cherry"

**Result:** `Apple` | `Banana` | `Cherry` (three columns)

**Explanation:** With ignore_empty set to TRUE, consecutive commas don't create empty cells. Without this, the result would include empty elements between consecutive delimiters.

---

### Example 7: Keep Empty Values (Default)
```excel
=TEXTSPLIT(A1, ",")
```
Where A1 contains "A,,B"

**Result:** `A` | (empty) | `B` (three columns)

**Explanation:** By default (ignore_empty = FALSE), consecutive delimiters create empty elements in the array. This preserves positional consistency in structured data.

---

### Example 8: Case-Insensitive Delimiter Matching
```excel
=TEXTSPLIT(A1, "AND", , , 1)
```
Where A1 contains "FirstANDSecondandThirdAND Fourth"

**Result:** `First` | `Second` | `Third` | ` Fourth` (four columns)

**Explanation:** With match_mode set to 1, the delimiter "AND" matches "AND", "and", "And", etc. All case variations are treated as split points.

---

### Example 9: Padding Jagged Arrays
```excel
=TEXTSPLIT(A1, ",", ";", , , "-")
```
Where A1 contains "A,B,C;D,E;F"

**Result:**
```
A | B | C
D | E | -
F | - | -
```

**Explanation:** When rows have different numbers of elements, pad_with specifies what to fill in missing positions. Here "-" fills the gaps to create a rectangular 3x3 array.

---

### Example 10: Pad with Empty String
```excel
=TEXTSPLIT(A1, ",", ";", , , "")
```
Where A1 contains "1,2,3;4,5"

**Result:**
```
1 | 2 | 3
4 | 5 | (empty)
```

**Explanation:** Padding with "" creates empty cells rather than the default #N/A, which is often preferred for clean output or further formula processing.

---

### Example 11: Split and Extract Specific Element
```excel
=INDEX(TEXTSPLIT(A1, ","), 1, 2)
```
Where A1 contains "Red,Green,Blue"

**Result:** `Green`

**Explanation:** Combine TEXTSPLIT with INDEX to extract a specific element from the split result. INDEX(array, row, column) retrieves the element at the specified position.

---

### Example 12: Split Path and Get Filename
```excel
=INDEX(TEXTSPLIT(A1, "\"), , COLUMNS(TEXTSPLIT(A1, "\")))
```
Where A1 contains "C:\Users\John\Documents\file.txt"

**Result:** `file.txt`

**Explanation:** This dynamically extracts the last segment of a path by using COLUMNS to count the split elements and INDEX to retrieve the last one.

---

### Example 13: Parse CSV Row into Cells
```excel
=TEXTSPLIT(A1, ",")
```
Where A1 contains "John,Doe,30,New York,Engineer"

**Result:** `John` | `Doe` | `30` | `New York` | `Engineer`

**Explanation:** CSV data stored in single cells can be parsed into proper spreadsheet columns. Each comma-separated value becomes a separate cell.

---

### Example 14: Parse Key-Value Pairs
```excel
=TEXTSPLIT(A1, "=", "&")
```
Where A1 contains "name=John&age=30&city=NYC"

**Result:**
```
name | John
age  | 30
city | NYC
```

**Explanation:** URL query strings and similar key-value formats split with "&" for rows and "=" for columns, creating a structured table of keys and values.

---

### Example 15: Split Email Addresses
```excel
=TEXTSPLIT(A1, {"@", "."})
```
Where A1 contains "user@company.com"

**Result:** `user` | `company` | `com`

**Explanation:** Multiple delimiters split the email into username, domain name, and TLD. Useful for domain analysis or email categorization.

---

### Example 16: Handle Multi-Character Delimiter
```excel
=TEXTSPLIT(A1, " - ")
```
Where A1 contains "Part A - Part B - Part C"

**Result:** `Part A` | `Part B` | `Part C`

**Explanation:** Delimiters can be multiple characters. Using " - " (space-hyphen-space) splits on that specific pattern while preserving hyphens within words.

---

### Example 17: Split Then Join Subset
```excel
=TEXTJOIN(",", TRUE, INDEX(TEXTSPLIT(A1, ","), 1, {1,3,5}))
```
Where A1 contains "A,B,C,D,E,F"

**Result:** `A,C,E`

**Explanation:** Complex parsing: split the text, select specific elements using INDEX with array of column numbers, then rejoin selected elements with TEXTJOIN.

---

### Example 18: Count Split Elements
```excel
=COLUMNS(TEXTSPLIT(A1, ","))
```
Where A1 contains "Apple,Banana,Cherry,Date"

**Result:** `4`

**Explanation:** COLUMNS counts the number of columns in the split result, effectively counting how many delimited segments exist in the text.

---

### Example 19: Vertical Split of Bullet Points
```excel
=TEXTSPLIT(A1, , "• ")
```
Where A1 contains "• First item• Second item• Third item"

**Result:**
```
(empty or leading text)
First item
Second item
Third item
```

**Explanation:** Split on bullet character plus space to parse bulleted lists. The first element may be empty if text starts with the delimiter.

---

### Example 20: Transpose Horizontal to Vertical
```excel
=TEXTSPLIT(A1, , ",")
```
Where A1 contains "A,B,C,D,E"

**Result:**
```
A
B
C
D
E
```
(vertical, five rows)

**Explanation:** Using the delimiter as row_delimiter instead of col_delimiter creates a vertical array from horizontally delimited text.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#SPILL!` | Adjacent cells not empty for spill range | Clear cells that would be occupied by the dynamic array result |
| `#VALUE!` | Invalid parameter type or empty text | Verify text input is valid; check delimiter parameters |
| `#NAME?` | Function not recognized (older Excel version) | TEXTSPLIT requires Excel 365; not available in Excel 2019 or earlier |
| `#N/A in results` | Default padding for jagged arrays | Use pad_with parameter to specify preferred fill value like "" |
| `Unexpected empty cells` | Consecutive delimiters creating empty elements | Set ignore_empty to TRUE to skip empty elements |
| `Missing splits` | Case sensitivity with delimiter | Set match_mode to 1 for case-insensitive matching |
| `Wrong dimensions` | Delimiter not found in text | Text is returned as single-element array; verify delimiter exists |
| `Formula returns original text` | Delimiter doesn't match (case or character mismatch) | Check delimiter exact characters; consider case sensitivity setting |
| `Array too large` | Many delimiters creating huge array | Consider filtering results or splitting in stages |
| `Inconsistent column counts` | Different row lengths without padding | Use pad_with parameter to create rectangular array |

## Use Cases

### [[CSV and Delimited Data Parsing]]
- **Implementation**: Parse CSV data stored in single cells using `=TEXTSPLIT(cell, ",")`. Handle embedded commas with proper quoting awareness (may require preprocessing). Split multi-row CSV data using `=TEXTSPLIT(cell, ",", CHAR(10))`.
- **Business Application**: Import and parse text exports from other systems. Process data extracts where multi-value fields are stored in single cells. Prepare imported data for proper spreadsheet analysis.
- **Technical Details**: Quoted fields with embedded delimiters require special handling (TEXTSPLIT doesn't natively handle CSV quoting rules). Use ignore_empty for data with inconsistent delimiter spacing. Chain with TRIM for cleanup.

### [[Multi-Line Text Processing]]
- **Implementation**: Split multi-line cell content using `=TEXTSPLIT(cell, , CHAR(10))` for line feed or `=TEXTSPLIT(cell, , CHAR(13)&CHAR(10))` for Windows-style line breaks.
- **Business Application**: Parse notes fields with bullet points or numbered items. Extract individual addresses from address blocks. Process multi-line form submissions.
- **Technical Details**: Different systems use different line break characters (LF, CR+LF, CR). Test which character your data uses. Use CLEAN function to normalize before splitting if needed.

### [[URL and Query String Parsing]]
- **Implementation**: Parse query parameters: `=TEXTSPLIT(query_string, "=", "&")` creates key-value table. Split URL components: `=TEXTSPLIT(url, {"://", "/", "?", "&", "="})` for comprehensive parsing.
- **Business Application**: Analyze marketing URLs to extract campaign parameters. Parse API endpoint data. Create reports on URL patterns and parameters.
- **Technical Details**: URLs may be URL-encoded; decode first with SUBSTITUTE or custom function. Query strings are case-sensitive. Handle missing parameters with pad_with.

### [[Hierarchical Data Flattening]]
- **Implementation**: Split hierarchical paths like `=TEXTSPLIT(A1, ">")` for breadcrumbs or `=TEXTSPLIT(A1, ".")` for dotted notation. Extract specific levels with INDEX.
- **Business Application**: Parse category hierarchies for product classification. Analyze organizational structures from path notation. Create drill-down reports from hierarchical codes.
- **Technical Details**: Use COLUMNS or ROWS to count hierarchy depth. Handle variable-depth hierarchies with pad_with. Combine with INDIRECT for dynamic referencing.

## Platform Differences

| Feature | Excel 365 | Excel 2019/Earlier | Google Sheets |
|---------|-----------|-------------------|---------------|
| **Function Availability** | Native TEXTSPLIT function | NOT AVAILABLE | SPLIT function (different) |
| **Column Splitting** | Full support | N/A | Supported in SPLIT |
| **Row Splitting** | Supported via row_delimiter | N/A | NOT AVAILABLE |
| **2D Array Output** | Supported | N/A | NOT AVAILABLE |
| **Multiple Delimiters** | Array of delimiters | N/A | Limited support |
| **Ignore Empty** | ignore_empty parameter | N/A | Third parameter in SPLIT |
| **Case Insensitivity** | match_mode parameter | N/A | NOT AVAILABLE |
| **Padding Control** | pad_with parameter | N/A | NOT AVAILABLE |
| **Dynamic Array** | Spills automatically | N/A | Also spills |

**Google Sheets SPLIT Comparison:**
```
Google Sheets: =SPLIT(text, delimiter, [split_by_each], [remove_empty_text])
Excel 365:     =TEXTSPLIT(text, col_delimiter, [row_delimiter], [ignore_empty], [match_mode], [pad_with])
```

Key differences:
- Google Sheets SPLIT only splits horizontally (columns)
- Google Sheets split_by_each treats each character as separate delimiter
- Excel TEXTSPLIT supports both row and column splitting
- Excel has more control over matching and padding

**Alternative for Pre-365 Excel:**

For simple comma split:
```excel
=TRIM(MID(SUBSTITUTE(A1,",",REPT(" ",100)),((COLUMN()-COLUMN($A$1))*100)+1,100))
```
This complex formula splits by comma using character padding technique.

**Cross-Platform Strategy:**
- For column-only splits, Google Sheets SPLIT may be compatible
- Document Excel 365 requirements for row splitting features
- Consider maintaining separate formulas for each platform
- Pre-process data in Excel 365 if advanced features are needed

## Tips and Best Practices

1. **Clear Spill Range**: Ensure cells where the result will spill are empty. TEXTSPLIT creates dynamic arrays that occupy multiple cells automatically.

2. **Use Pad_with for Consistent Arrays**: When splitting text with variable segment counts, use pad_with to create rectangular arrays: `=TEXTSPLIT(A1, ",", ";", , , "")`. This prevents #N/A values.

3. **Combine with INDEX for Element Access**: Extract specific elements: `=INDEX(TEXTSPLIT(A1, ","), 1, 2)` gets the second element. This is cleaner than multiple TEXTBEFORE/TEXTAFTER calls.

4. **Handle Empty Values Appropriately**: Set ignore_empty to TRUE when consecutive delimiters should be skipped: `=TEXTSPLIT(A1, ",", , TRUE)`. Leave FALSE when position matters.

5. **Use COLUMNS/ROWS for Element Counting**: Count split elements: `=COLUMNS(TEXTSPLIT(A1, ","))` returns how many segments exist. Useful for validation or dynamic processing.

6. **Check for Delimiter Existence First**: Before splitting, verify delimiter exists: `=IF(ISNUMBER(FIND(",", A1)), TEXTSPLIT(A1, ","), A1)`. This handles cases where text might not be delimited.

7. **Use Array Delimiters for Flexibility**: Accept multiple possible delimiters: `=TEXTSPLIT(A1, {",", ";", "|"})`. Any of these characters will act as split points.

8. **Consider Case Sensitivity**: For text delimiters, set match_mode to 1 if case may vary: `=TEXTSPLIT(A1, "AND", , , 1)` matches "AND", "and", "And".

9. **Combine with Other Array Functions**: TEXTSPLIT output works with FILTER, SORT, UNIQUE, etc.: `=UNIQUE(TEXTSPLIT(A1, ","))` removes duplicate values after splitting.

10. **Wrap in IFERROR for Robustness**: Handle potential errors gracefully: `=IFERROR(TEXTSPLIT(A1, ","), A1)` returns original text if split fails.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from TEXTSPLIT |
|----------|-------------|-------------------------------|
| [[TEXTBEFORE]] | Extracts text before a delimiter | Returns single value, not array |
| [[TEXTAFTER]] | Extracts text after a delimiter | Returns single value, not array |
| [[MID]] | Extracts text by position and length | Position-based, not delimiter-based |
| [[LEFT]]/[[RIGHT]] | Extracts from start/end of text | Character count based |
| [[FILTERXML]] | Parses XML structure | Specific to XML data format |

### Commonly Used Together

**[[INDEX]]** - Extract specific element from split result

*Access a specific position in the split array:*
```excel
=INDEX(TEXTSPLIT(A1, ","), 1, 3)
```
Returns the third element from comma-separated text.

---

**[[COLUMNS]]/[[ROWS]]** - Count split elements

*Determine how many segments were created:*
```excel
=COLUMNS(TEXTSPLIT(A1, ","))
```
Returns the number of comma-separated values.

---

**[[TEXTJOIN]]** - Rejoin split elements

*Recombine selected elements with new delimiter:*
```excel
=TEXTJOIN("-", TRUE, TEXTSPLIT(A1, ","))
```
Converts comma-separated to hyphen-separated.

---

**[[FILTER]]** - Filter split results

*Select specific elements meeting criteria:*
```excel
=FILTER(TEXTSPLIT(A1, ","), LEN(TEXTSPLIT(A1, ","))>3)
```
Returns only elements longer than 3 characters.

---

**[[UNIQUE]]** - Remove duplicates from split

*Get unique values from delimited list:*
```excel
=UNIQUE(TEXTSPLIT(A1, ","))
```
Returns each unique value once.

---

**[[SORT]]** - Sort split elements

*Alphabetize split results:*
```excel
=SORT(TEXTSPLIT(A1, ","))
```
Returns elements in sorted order.

## Official Documentation

- **Microsoft Excel**: [TEXTSPLIT function](https://support.microsoft.com/en-us/office/textsplit-function-b1ca414e-4c21-4ca0-b1b7-bdecace8a6e7)

## Version History

| Platform | Version/Date | Changes |
|----------|--------------|---------|
| Excel 365 | August 2022 | Original function release for Microsoft 365 subscribers |
| Excel for Web | August 2022 | Simultaneous release with desktop Excel 365 |
| Excel for Mac 365 | August 2022 | Included in Microsoft 365 for Mac |
| Excel 2019 | N/A | Function not available |
| Excel 2016 | N/A | Function not available |
| Excel Online | August 2022 | Available in web version |
| Google Sheets | N/A | Has SPLIT function with limited features |

---

*Last updated: 2026-01-10*
