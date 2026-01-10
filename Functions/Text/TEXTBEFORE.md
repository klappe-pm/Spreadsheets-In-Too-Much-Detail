---
categories:
  - spreadsheet-functions
subCategories:
  - excel
topics:
  - text-manipulation
  - text-extraction
subTopics:
  - delimiter-extraction
  - string-parsing
  - text-before-delimiter
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - text-before
  - before-delimiter
  - extract-before
tags:
  - text
  - extraction
  - parsing
  - delimiter
  - excel-365
  - excel-only
---

# TEXTBEFORE

## Description

TEXTBEFORE is an Excel 365-exclusive function that extracts and returns all text occurring before a specified delimiter string within the source text. This function provides an elegant, single-formula solution for a common text parsing task that previously required complex combinations of FIND, LEFT, and LEN functions. TEXTBEFORE searches for the delimiter within the text and returns everything that precedes it, making it invaluable for extracting portions of structured text strings such as usernames from email addresses, first names from full names, file names without extensions, and any scenario where you need text appearing before a specific separator or marker.

The function is particularly powerful due to its optional parameters that handle real-world data complexity. You can specify which occurrence of the delimiter to find (extracting before the 2nd comma, for example), control case sensitivity of the delimiter search, define search direction (from the beginning or end of the text), and specify what to return when the delimiter is not found (error or empty string). This flexibility makes TEXTBEFORE suitable for parsing email addresses (extracting usernames), splitting names at specific delimiters, extracting base filenames from paths, isolating prefixes from product codes, and handling any text where content before a specific marker needs extraction.

Several critical aspects require attention when working with TEXTBEFORE. First, the function returns a #N/A error by default when the delimiter is not found in the text—use the if_not_found parameter or wrap in IFERROR to handle this gracefully. Second, instance_num allows extracting before the Nth occurrence (positive) or Nth-from-end occurrence (negative), enabling both forward and reverse parsing. Third, when match_mode is set to 1 (case-insensitive), "A" and "a" are treated as the same delimiter. Fourth, the match_end parameter allows searching from the end of the string, which can be useful for specific parsing scenarios. Fifth, TEXTBEFORE returns the original text unchanged (via if_not_found) if you want it to pass through text without the delimiter rather than error.

TEXTBEFORE is available exclusively in Microsoft Excel 365 (Microsoft 365 subscription) and Excel for the web, introduced in August 2022. It is NOT available in Excel 2019, Excel 2016, or earlier perpetual license versions. Google Sheets does not have this function natively; Sheets users must use combinations of FIND/SEARCH, LEFT, LEN, and IF, or leverage REGEXEXTRACT for pattern-based extraction. When building cross-platform spreadsheets, either provide alternative formulas for non-365 Excel and Sheets users, or document that the spreadsheet requires Excel 365.

## Syntax

> [!f(x)] TEXTBEFORE Syntax
>
> ```excel
> =TEXTBEFORE(text, delimiter, [instance_num], [match_mode], [match_end], [if_not_found])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string to search within. Can be a cell reference, literal text in quotes, or a formula returning text. | Yes |
> | `delimiter` | The text string marking where to stop extraction. Everything before this delimiter is returned. Can be a single character or multiple characters. Can also be an array of delimiters to match any of them. | Yes |
> | `instance_num` | Which occurrence of the delimiter to extract before. Default is 1 (first occurrence). Positive numbers count from the beginning; negative numbers count from the end (-1 = last occurrence). | No |
> | `match_mode` | Controls case sensitivity: 0 = case-sensitive (default), 1 = case-insensitive. | No |
> | `match_end` | Where to start searching: 0 = from the beginning (default), 1 = from the end of the text. | No |
> | `if_not_found` | Value to return if the delimiter is not found. Default behavior returns #N/A error. Specify "" for empty string, original text with A1, or any custom value. | No |

## Examples

### Example 1: Extract Username from Email Address
```excel
=TEXTBEFORE(A1, "@")
```
Where A1 contains "john.doe@company.com"

**Result:** `john.doe`

**Explanation:** The function finds the @ delimiter and returns everything before it—the email username. This is the most common use case for TEXTBEFORE with email data.

---

### Example 2: Extract First Name from Full Name
```excel
=TEXTBEFORE(A1, " ")
```
Where A1 contains "John Smith"

**Result:** `John`

**Explanation:** Using a space as the delimiter, TEXTBEFORE extracts the first name from a "First Last" formatted name.

---

### Example 3: Extract Filename Without Extension
```excel
=TEXTBEFORE(A1, ".", -1)
```
Where A1 contains "report.2024.pdf"

**Result:** `report.2024`

**Explanation:** Using -1 for instance_num finds the LAST period and returns everything before it. This correctly handles filenames with multiple periods, keeping "report.2024" and removing only ".pdf".

---

### Example 4: Extract Before Second Delimiter Occurrence
```excel
=TEXTBEFORE(A1, "-", 2)
```
Where A1 contains "ABC-123-XL-RED"

**Result:** `ABC-123`

**Explanation:** The instance_num of 2 specifies extracting before the second hyphen. This returns "ABC-123", including the first hyphen but stopping before the second.

---

### Example 5: Extract Before Last Delimiter (Negative Instance)
```excel
=TEXTBEFORE(A1, "/", -1)
```
Where A1 contains "C:/Users/John/Documents/file.txt"

**Result:** `C:/Users/John/Documents`

**Explanation:** Using -1 for instance_num finds the last slash and returns everything before it—the directory path without the filename. Essential for file path parsing.

---

### Example 6: Case-Insensitive Matching
```excel
=TEXTBEFORE(A1, "END", 1, 1)
```
Where A1 contains "Start here end of message"

**Result:** `Start here `

**Explanation:** With match_mode set to 1, the search is case-insensitive. The delimiter "END" matches "end" in the text. Note the trailing space in the result—use TRIM if needed.

---

### Example 7: Handle Missing Delimiter Gracefully
```excel
=TEXTBEFORE(A1, "@", 1, 0, 0, "Invalid Email")
```
Where A1 contains "Not an email address"

**Result:** `Invalid Email`

**Explanation:** When the delimiter isn't found, the if_not_found parameter returns "Invalid Email" instead of #N/A. This is essential for data validation and clean error handling.

---

### Example 8: Return Original Text If Delimiter Not Found
```excel
=TEXTBEFORE(A1, "@", 1, 0, 0, A1)
```
Where A1 contains "Plain text without delimiter"

**Result:** `Plain text without delimiter`

**Explanation:** Setting if_not_found to A1 returns the original text unchanged when no delimiter is found. Useful when you want to process mixed data where some entries lack the delimiter.

---

### Example 9: Return Empty String If Not Found
```excel
=TEXTBEFORE(A1, "@", 1, 0, 0, "")
```
Where A1 contains "No delimiter here"

**Result:** (empty string)

**Explanation:** Setting if_not_found to "" returns an empty string instead of an error. Useful for conditional processing or when blank output is preferred.

---

### Example 10: Multiple Possible Delimiters (Array)
```excel
=TEXTBEFORE(A1, {"-", "_", " "})
```
Where A1 contains "Product_Code_123"

**Result:** `Product`

**Explanation:** When delimiter is an array, TEXTBEFORE finds whichever delimiter appears first and extracts before it. The underscore is found first, so text before the first underscore is returned.

---

### Example 11: Extract Order Number Prefix
```excel
=TEXTBEFORE(A1, "-")
```
Where A1 contains "ORD-2024-00123"

**Result:** `ORD`

**Explanation:** Simple extraction of order type prefix before the first hyphen. Useful for categorizing orders or transactions by their prefix codes.

---

### Example 12: Parse URL Protocol
```excel
=TEXTBEFORE(A1, "://")
```
Where A1 contains "https://www.example.com/page"

**Result:** `https`

**Explanation:** The delimiter can be multiple characters. Here "://" is the delimiter, and everything before it (the protocol) is extracted.

---

### Example 13: Extract Text Before Specific Word
```excel
=TEXTBEFORE(A1, " - ")
```
Where A1 contains "Product Name - Description here"

**Result:** `Product Name`

**Explanation:** Using " - " (space-hyphen-space) as delimiter allows extraction of the product name portion from a formatted description string.

---

### Example 14: Parse Log Entry Level
```excel
=TEXTBEFORE(A1, "]")
```
Where A1 contains "[ERROR] Connection failed"

**Result:** `[ERROR`

**Explanation:** For log entries with bracketed levels, this extracts the level portion. Combine with TEXTAFTER(result, "[") to get just "ERROR".

---

### Example 15: Combine with TEXTAFTER for Middle Extraction
```excel
=TEXTBEFORE(TEXTAFTER(A1, "("), ")")
```
Where A1 contains "John Smith (Engineering)"

**Result:** `Engineering`

**Explanation:** First TEXTAFTER extracts after "(", then TEXTBEFORE takes everything before ")". This extracts the content within parentheses.

---

### Example 16: Extract Domain Without TLD
```excel
=TEXTBEFORE(TEXTAFTER(A1, "@"), ".")
```
Where A1 contains "user@company.com"

**Result:** `company`

**Explanation:** Nested functions: TEXTAFTER gets "company.com", then TEXTBEFORE gets "company" before the first period.

---

### Example 17: Extract Before Third-to-Last Delimiter
```excel
=TEXTBEFORE(A1, ".", -3)
```
Where A1 contains "192.168.1.100"

**Result:** `192`

**Explanation:** Using instance_num of -3 counts from the end, finding the third-to-last period and returning everything before it. Here it returns just the first octet of the IP address.

---

### Example 18: Extract Last Name from "Last, First" Format
```excel
=TEXTBEFORE(A1, ",")
```
Where A1 contains "Smith, John"

**Result:** `Smith`

**Explanation:** For names in "Last, First" format, TEXTBEFORE with comma delimiter extracts the last name.

---

### Example 19: Parse Key from Key-Value Pair
```excel
=TEXTBEFORE(A1, "=")
```
Where A1 contains "username=john_doe"

**Result:** `username`

**Explanation:** For key-value pairs separated by "=", TEXTBEFORE extracts the key portion. Use TEXTAFTER to get the value.

---

### Example 20: Dynamic Delimiter from Another Cell
```excel
=TEXTBEFORE(A1, B1)
```
Where A1 contains "Part A | Part B" and B1 contains " | "

**Result:** `Part A`

**Explanation:** The delimiter can reference a cell, allowing dynamic parsing based on user input or configuration. The cell B1 contains " | " (space-pipe-space) as the delimiter.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Delimiter not found in text | Use if_not_found parameter to specify fallback value, or wrap in IFERROR |
| `#VALUE!` | Invalid instance_num (0 is not valid) | Use positive integers for forward search, negative for reverse; never 0 |
| `#VALUE!` | Empty delimiter string | Delimiter cannot be empty; must be at least one character |
| `#NAME?` | Function not recognized (older Excel version) | TEXTBEFORE requires Excel 365; not available in Excel 2019 or earlier |
| `#CALC!` | Circular reference or calculation error | Check for circular dependencies in formula |
| `Unexpected result` | Case sensitivity mismatch | Set match_mode to 1 for case-insensitive matching |
| `Wrong portion extracted` | Multiple delimiters, wrong instance selected | Use instance_num to specify correct occurrence; use negative for end-based |
| `Leading/trailing spaces` | Text contains whitespace around delimiter | Combine with TRIM to clean results |
| `Array formula issues` | Using array delimiter incorrectly | Ensure array syntax {"-", "/"} is correct |
| `Returns error in some cells` | Mixed data with some missing delimiters | Use if_not_found parameter for consistent handling |

## Use Cases

### [[Email Address Processing]]
- **Implementation**: Extract usernames from email addresses using `=TEXTBEFORE(email, "@")`. Extract domain portions with TEXTAFTER. Combine to create user ID mappings or domain-based analytics.
- **Business Application**: Process email lists for user identification. Create lookup keys from email addresses. Segment users by email domain for analysis. Validate email format by checking for expected parts.
- **Technical Details**: Email usernames may contain periods and other characters; TEXTBEFORE handles this correctly. Use if_not_found to handle invalid emails gracefully. Combine with LOWER for case-insensitive matching.

### [[Name Parsing and Formatting]]
- **Implementation**: Extract first names from "First Last" format with `=TEXTBEFORE(name, " ")`. Extract last names from "Last, First" format with `=TEXTBEFORE(name, ",")`. Handle middle names using instance_num parameter.
- **Business Application**: Parse imported name data into separate fields. Prepare mail merge data with proper name formatting. Create standardized name fields from varied input formats.
- **Technical Details**: Names with multiple spaces or hyphens require careful instance_num handling. Consider cultural naming conventions (some names have prefixes or multiple parts). Use TRIM to clean extracted portions.

### [[File Path Parsing]]
- **Implementation**: Extract folder paths using `=TEXTBEFORE(path, "\", -1)`. Extract filenames without extensions with nested TEXTBEFORE and TEXTAFTER. Parse specific folder levels using instance_num.
- **Business Application**: Create file inventories with separated path components. Generate folder structure reports. Extract project or date information embedded in folder paths.
- **Technical Details**: Windows uses "\", Mac/Unix uses "/" as path separators. Use -1 for instance_num to get folder path without filename. Handle paths without extensions using if_not_found.

### [[Product Code and SKU Parsing]]
- **Implementation**: Extract product category prefixes with `=TEXTBEFORE(sku, "-")`. Parse multi-part codes using instance_num to stop at specific delimiters. Isolate specific segments by combining with TEXTAFTER.
- **Business Application**: Categorize products by SKU prefix for inventory analysis. Extract size, color, or version codes from structured product identifiers. Create pivot table groupings from parsed code segments.
- **Technical Details**: SKU formats vary by company; adjust delimiter and instance_num accordingly. Handle SKUs without expected delimiters using if_not_found. Consider uppercase normalization for consistent matching.

## Platform Differences

| Feature | Excel 365 | Excel 2019/Earlier | Google Sheets |
|---------|-----------|-------------------|---------------|
| **Function Availability** | Native function | NOT AVAILABLE | NOT AVAILABLE |
| **Syntax** | =TEXTBEFORE(text, delimiter, ...) | N/A | N/A |
| **Array Delimiters** | Supported {"-", "_"} | N/A | N/A |
| **Negative Instance** | Supported (-1 = last) | N/A | N/A |
| **Case Sensitivity Option** | match_mode parameter | N/A | N/A |
| **If Not Found** | if_not_found parameter | N/A | N/A |

**Alternative for Pre-365 Excel:**
```excel
=LEFT(A1, FIND("@", A1) - 1)
```
This extracts before "@" in older Excel versions but lacks TEXTBEFORE's flexibility.

For last occurrence:
```excel
=LEFT(A1, FIND("☺", SUBSTITUTE(A1, "/", "☺", LEN(A1) - LEN(SUBSTITUTE(A1, "/", "")))) - 1)
```
This complex formula finds the last "/" using SUBSTITUTE to replace the Nth occurrence with a unique character.

**Alternative for Google Sheets:**
```
=REGEXEXTRACT(A1, "(.+)@")
```
Uses regex to capture everything before "@". More flexible but syntax differs significantly.

Or using traditional functions:
```
=LEFT(A1, FIND("@", A1) - 1)
```

**Cross-Platform Strategy:**
- For files used across platforms, provide both formulas with comments
- Consider pre-processing in Excel 365, then sharing results
- Document Excel 365 requirements clearly
- Use helper columns with platform-neutral formulas when possible

## Tips and Best Practices

1. **Always Handle Missing Delimiters**: Use the if_not_found parameter to specify what to return when the delimiter isn't found: `=TEXTBEFORE(A1, "@", 1, 0, 0, "")`. This prevents #N/A errors from breaking your spreadsheet.

2. **Use Negative Instance for Last Occurrence**: When you need text before the LAST occurrence (like folder paths from full paths), use -1: `=TEXTBEFORE(path, "\", -1)`. This is more reliable than counting occurrences.

3. **Return Original on No Match**: Set if_not_found to A1 to return the original text unchanged when delimiter is missing: `=TEXTBEFORE(A1, "@", 1, 0, 0, A1)`. Useful for mixed data processing.

4. **Consider Case Sensitivity**: If your delimiter might appear in different cases, set match_mode to 1 for case-insensitive matching. Essential for text data from user input.

5. **Combine with TRIM for Clean Output**: Wrap TEXTBEFORE in TRIM to remove leading/trailing spaces from results: `=TRIM(TEXTBEFORE(A1, "-"))`.

6. **Chain with TEXTAFTER for Middle Extraction**: Extract text between two delimiters: `=TEXTBEFORE(TEXTAFTER(A1, "("), ")")` extracts content within parentheses.

7. **Use Array Delimiters for Multiple Possible Separators**: When data might use different delimiters, specify an array: `=TEXTBEFORE(A1, {"-", "_", " "})`. The function finds whichever appears first.

8. **Validate Edge Cases**: Test with empty cells, text without delimiters, and text where delimiter is at the start. These scenarios may produce unexpected results.

9. **Document Excel Version Requirements**: If sharing spreadsheets, clearly note that TEXTBEFORE requires Excel 365. Provide alternative formulas for users on older versions.

10. **Consider TEXTSPLIT for Multiple Extractions**: If you need to parse text into multiple components, TEXTSPLIT may be more efficient than multiple TEXTBEFORE/TEXTAFTER calls.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from TEXTBEFORE |
|----------|-------------|--------------------------------|
| [[TEXTAFTER]] | Extracts text after a delimiter | Returns text after delimiter, not before |
| [[TEXTSPLIT]] | Splits text by delimiters into array | Returns all segments, not just portion before delimiter |
| [[LEFT]] | Extracts characters from start of text | Character count based, not delimiter-based |
| [[MID]] | Extracts text by position and length | Position-based, not delimiter-based |
| [[FIND]]/[[SEARCH]] | Locates position of text | Returns position number, not extracted text |

### Commonly Used Together

**[[TEXTAFTER]]** - Extract text after a delimiter

*Combine to extract text between two delimiters:*
```excel
=TEXTBEFORE(TEXTAFTER(A1, "("), ")")
```
Extracts content between parentheses.

---

**[[TRIM]]** - Clean whitespace from extracted text

*Remove extra spaces from extraction result:*
```excel
=TRIM(TEXTBEFORE(A1, "@"))
```
Ensures clean output without leading/trailing spaces.

---

**[[IFERROR]]** - Handle cases where delimiter not found

*Provide fallback when extraction fails:*
```excel
=IFERROR(TEXTBEFORE(A1, "@"), A1)
```
Alternative to if_not_found parameter for complex fallback logic.

---

**[[LEN]]** - Calculate length of extracted text

*Measure the extracted portion:*
```excel
=LEN(TEXTBEFORE(A1, "@"))
```
Useful for validation or further processing.

---

**[[TEXTSPLIT]]** - Split text into multiple parts

*Parse text into array, then reference specific elements:*
```excel
=INDEX(TEXTSPLIT(A1, "-"), 1)
```
For complex parsing, TEXTSPLIT may be more efficient.

---

**[[LOWER]]/[[UPPER]]** - Normalize case of extracted text

*Standardize extracted text:*
```excel
=LOWER(TEXTBEFORE(A1, "@"))
```
Normalizes username extraction for comparison purposes.

## Official Documentation

- **Microsoft Excel**: [TEXTBEFORE function](https://support.microsoft.com/en-us/office/textbefore-function-d099c28a-dba8-448e-ac6c-f086d0fa1b29)

## Version History

| Platform | Version/Date | Changes |
|----------|--------------|---------|
| Excel 365 | August 2022 | Original function release for Microsoft 365 subscribers |
| Excel for Web | August 2022 | Simultaneous release with desktop Excel 365 |
| Excel for Mac 365 | August 2022 | Included in Microsoft 365 for Mac |
| Excel 2019 | N/A | Function not available |
| Excel 2016 | N/A | Function not available |
| Excel Online | August 2022 | Available in web version |
| Google Sheets | N/A | Function not available |

---

*Last updated: 2026-01-10*
