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
  - text-after-delimiter
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - text-after
  - after-delimiter
  - extract-after
tags:
  - text
  - extraction
  - parsing
  - delimiter
  - excel-365
  - excel-only
---

# TEXTAFTER

## Description

TEXTAFTER is an Excel 365-exclusive function that extracts and returns all text occurring after a specified delimiter string within the source text. This function provides an elegant, single-formula solution for a common text parsing task that previously required complex combinations of FIND, MID, and LEN functions. TEXTAFTER searches for the delimiter within the text and returns everything that follows it, making it invaluable for extracting portions of structured text strings such as file extensions from filenames, domain names from email addresses, last names when a delimiter separates name parts, and any scenario where you need text appearing after a specific separator or marker.

The function is particularly powerful due to its optional parameters that handle real-world data complexity. You can specify which occurrence of the delimiter to use (extracting after the 2nd comma, for example), control case sensitivity of the delimiter search, define search direction (from the beginning or end of the text), and specify what to return when the delimiter is not found (error or empty string). This flexibility makes TEXTAFTER suitable for parsing complex paths (extracting filenames from full paths by searching from the end), processing email addresses, splitting names at specific delimiters, extracting data from structured codes, and handling any text where content after a specific marker needs extraction.

Several critical aspects require attention when working with TEXTAFTER. First, the function returns a #N/A error by default when the delimiter is not found in the text—use the if_not_found parameter or wrap in IFERROR to handle this gracefully. Second, instance_num allows extracting after the Nth occurrence (positive) or Nth-from-end occurrence (negative), enabling both forward and reverse parsing. Third, when match_mode is set to 1 (case-insensitive), "A" and "a" are treated as the same delimiter. Fourth, the match_end parameter allows searching from the end of the string, which is essential for extracting filenames from paths where the path separator appears multiple times. Fifth, TEXTAFTER returns empty string "" when text appears immediately after the final delimiter and if_not_found is set to return empty.

TEXTAFTER is available exclusively in Microsoft Excel 365 (Microsoft 365 subscription) and Excel for the web, introduced in August 2022. It is NOT available in Excel 2019, Excel 2016, or earlier perpetual license versions. Google Sheets does not have this function natively; Sheets users must use combinations of FIND/SEARCH, MID, LEN, and IF, or leverage REGEXEXTRACT for pattern-based extraction. When building cross-platform spreadsheets, either provide alternative formulas for non-365 Excel and Sheets users, or document that the spreadsheet requires Excel 365.

## Syntax

> [!f(x)] TEXTAFTER Syntax
>
> ```excel
> =TEXTAFTER(text, delimiter, [instance_num], [match_mode], [match_end], [if_not_found])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string to search within. Can be a cell reference, literal text in quotes, or a formula returning text. | Yes |
> | `delimiter` | The text string marking where to start extraction. Everything after this delimiter is returned. Can be a single character or multiple characters. Can also be an array of delimiters to match any of them. | Yes |
> | `instance_num` | Which occurrence of the delimiter to extract after. Default is 1 (first occurrence). Positive numbers count from the beginning; negative numbers count from the end (-1 = last occurrence). | No |
> | `match_mode` | Controls case sensitivity: 0 = case-sensitive (default), 1 = case-insensitive. | No |
> | `match_end` | Where to start searching: 0 = from the beginning (default), 1 = from the end of the text. | No |
> | `if_not_found` | Value to return if the delimiter is not found. Default behavior returns #N/A error. Specify "" for empty string, custom text, or any value. | No |

## Examples

### Example 1: Extract File Extension from Filename
```excel
=TEXTAFTER(A1, ".")
```
Where A1 contains "document.pdf"

**Result:** `pdf`

**Explanation:** The function finds the period delimiter and returns everything after it—the file extension. This simple form extracts text after the first occurrence of the delimiter.

---

### Example 2: Extract Domain from Email Address
```excel
=TEXTAFTER(A1, "@")
```
Where A1 contains "user.name@company.com"

**Result:** `company.com`

**Explanation:** The @ symbol serves as the delimiter, and TEXTAFTER returns the complete domain portion of the email address including the TLD.

---

### Example 3: Extract Last Name After Comma
```excel
=TEXTAFTER(A1, ", ")
```
Where A1 contains "Smith, John"

**Result:** `John`

**Explanation:** When names are stored as "Last, First", TEXTAFTER with ", " as delimiter extracts the first name. Note the space after the comma to avoid leading space in the result.

---

### Example 4: Extract After Second Delimiter Occurrence
```excel
=TEXTAFTER(A1, "/", 2)
```
Where A1 contains "folder/subfolder/file.txt"

**Result:** `file.txt`

**Explanation:** The instance_num parameter (2) specifies extracting after the second slash, skipping "folder/" and returning what follows the second delimiter.

---

### Example 5: Extract After Last Delimiter (Negative Instance)
```excel
=TEXTAFTER(A1, "/", -1)
```
Where A1 contains "C:/Users/John/Documents/file.txt"

**Result:** `file.txt`

**Explanation:** Using -1 for instance_num finds the last occurrence of the delimiter and extracts after it. This is perfect for extracting filenames from full file paths regardless of how many folders deep.

---

### Example 6: Case-Insensitive Matching
```excel
=TEXTAFTER(A1, "after:", 1, 1)
```
Where A1 contains "AFTER: Important message"

**Result:** ` Important message`

**Explanation:** With match_mode set to 1, the search is case-insensitive. The delimiter "after:" matches "AFTER:" in the text. Note the leading space in the result—use TRIM if needed.

---

### Example 7: Handle Missing Delimiter Gracefully
```excel
=TEXTAFTER(A1, "@", 1, 0, 0, "No email")
```
Where A1 contains "Not an email address"

**Result:** `No email`

**Explanation:** When the delimiter isn't found, instead of returning #N/A error, the if_not_found parameter specifies returning "No email". This prevents error propagation in formulas.

---

### Example 8: Return Empty String If Not Found
```excel
=TEXTAFTER(A1, "@", 1, 0, 0, "")
```
Where A1 contains "No delimiter here"

**Result:** (empty string)

**Explanation:** Setting if_not_found to "" returns an empty string instead of an error when the delimiter is not found. Useful for conditional processing or clean output.

---

### Example 9: Extract From End of String
```excel
=TEXTAFTER(A1, " ", 1, 0, 1)
```
Where A1 contains "The quick brown fox"

**Result:** (empty string, as nothing follows the last space when searching from end)

**Better Example:**
```excel
=TEXTAFTER(A1, " ", -1)
```
Where A1 contains "The quick brown fox"

**Result:** `fox`

**Explanation:** Using instance_num of -1 extracts after the last space, giving the last word. The match_end parameter is less commonly used than negative instance_num.

---

### Example 10: Multiple Possible Delimiters (Array)
```excel
=TEXTAFTER(A1, {"-", "_", " "})
```
Where A1 contains "product-code-123"

**Result:** `code-123`

**Explanation:** When delimiter is an array, TEXTAFTER finds whichever delimiter appears first and extracts after it. The hyphen is found first, so text after the first hyphen is returned.

---

### Example 11: Extract SKU Component After Prefix
```excel
=TEXTAFTER(A1, "SKU-")
```
Where A1 contains "Product: SKU-ABC123-XL"

**Result:** `ABC123-XL`

**Explanation:** The delimiter can be multiple characters. Here "SKU-" is the delimiter, and everything after it is extracted. This is useful for parsing structured product codes.

---

### Example 12: Parse Log Entry Timestamp
```excel
=TEXTAFTER(A1, "] ")
```
Where A1 contains "[2024-01-15 14:30:25] Error occurred in module"

**Result:** `Error occurred in module`

**Explanation:** For log entries with bracketed timestamps, using "] " as delimiter extracts the message portion after the timestamp block.

---

### Example 13: Extract Query String from URL
```excel
=TEXTAFTER(A1, "?")
```
Where A1 contains "https://example.com/page?id=123&name=test"

**Result:** `id=123&name=test`

**Explanation:** For URLs with query parameters, TEXTAFTER with "?" delimiter extracts the entire query string. Further parsing can then split individual parameters.

---

### Example 14: Chain Multiple TEXTAFTER Calls
```excel
=TEXTAFTER(TEXTAFTER(A1, "Name: "), ", Age")
```
Where A1 contains "Name: John Smith, Age: 30"

**Result:** `John Smith`

**Explanation:** By nesting TEXTAFTER functions, you can extract text between two different delimiters. First extract after "Name: ", then extract before ", Age" (using TEXTBEFORE for the second step would be cleaner).

---

### Example 15: Combine with TEXTBEFORE for Middle Extraction
```excel
=TEXTBEFORE(TEXTAFTER(A1, "<"), ">")
```
Where A1 contains "Email: <user@example.com>"

**Result:** `user@example.com`

**Explanation:** TEXTAFTER extracts after "<", then TEXTBEFORE takes everything before ">", effectively extracting the content between angle brackets.

---

### Example 16: Extract All Text After Colon, Trim Whitespace
```excel
=TRIM(TEXTAFTER(A1, ":"))
```
Where A1 contains "Status:   Active  "

**Result:** `Active`

**Explanation:** Combining with TRIM removes any leading or trailing whitespace from the extracted text, creating clean output.

---

### Example 17: Extract After Third-to-Last Delimiter
```excel
=TEXTAFTER(A1, ".", -3)
```
Where A1 contains "1.2.3.4.5.6.7"

**Result:** `5.6.7`

**Explanation:** Using instance_num of -3 counts from the end, finding the third-to-last period and returning everything after it. Useful for extracting portions of version numbers or IP addresses.

---

### Example 18: Case-Sensitive vs Insensitive Comparison
```excel
=TEXTAFTER(A1, "ERROR", 1, 0)
```
Where A1 contains "An error occurred: error details"

**Result:** `#N/A` (delimiter "ERROR" not found - case mismatch)

```excel
=TEXTAFTER(A1, "ERROR", 1, 1)
```
**Result:** ` occurred: error details` (case-insensitive matches "error")

**Explanation:** Default case-sensitive matching (0) fails because "ERROR" doesn't match "error". Setting match_mode to 1 enables case-insensitive matching.

---

### Example 19: Extract Filename from Windows Path
```excel
=TEXTAFTER(A1, "\", -1)
```
Where A1 contains "C:\Users\John\Documents\Report.xlsx"

**Result:** `Report.xlsx`

**Explanation:** Using -1 for instance_num finds the last backslash (folder separator) and returns the filename. Essential for file path parsing.

---

### Example 20: Dynamic Delimiter from Another Cell
```excel
=TEXTAFTER(A1, B1)
```
Where A1 contains "Key=Value" and B1 contains "="

**Result:** `Value`

**Explanation:** The delimiter doesn't have to be hardcoded—it can reference a cell, allowing dynamic parsing based on user input or configuration settings.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Delimiter not found in text | Use if_not_found parameter to specify fallback value, or wrap in IFERROR |
| `#VALUE!` | Invalid instance_num (0 is not valid) | Use positive integers for forward search, negative for reverse; never 0 |
| `#VALUE!` | Empty delimiter string | Delimiter cannot be empty; must be at least one character |
| `#NAME?` | Function not recognized (older Excel version) | TEXTAFTER requires Excel 365; not available in Excel 2019 or earlier |
| `#CALC!` | Circular reference or calculation error | Check for circular dependencies in formula |
| `Unexpected result` | Case sensitivity mismatch | Set match_mode to 1 for case-insensitive matching |
| `Wrong portion extracted` | Multiple delimiters, wrong instance selected | Use instance_num to specify correct occurrence; use negative for end-based |
| `Leading/trailing spaces` | Text contains whitespace around delimiter | Combine with TRIM to clean results |
| `Array formula issues` | Using array delimiter incorrectly | Ensure array syntax {"-", "/"} is correct |
| `Returns error in some cells` | Mixed data with some missing delimiters | Use if_not_found parameter for consistent handling |

## Use Cases

### [[File Path and Name Parsing]]
- **Implementation**: Extract filenames from full file paths using `=TEXTAFTER(path, "\", -1)` or `=TEXTAFTER(path, "/", -1)` depending on path format. Extract file extensions with `=TEXTAFTER(filename, ".", -1)`. Parse folder names by combining with TEXTBEFORE.
- **Business Application**: Process file listings for document management systems. Generate reports from file exports. Parse download logs or file upload records. Create file inventories with separated name and extension columns.
- **Technical Details**: Windows paths use "\", Unix/Mac paths use "/". Use -1 for instance_num to get the last segment (filename). For extension, use -1 to handle files like "backup.2024.zip" correctly. Consider IF to handle paths without the expected delimiters.

### [[Contact and Address Data Parsing]]
- **Implementation**: Extract domains from email addresses with `=TEXTAFTER(email, "@")`. Parse "Last, First" names with `=TEXTAFTER(name, ", ")`. Extract ZIP codes from "City, State ZIP" with `=TEXTAFTER(location, " ", -1)`.
- **Business Application**: Clean and normalize contact databases. Prepare mailing lists by parsing address components. Segment email lists by domain for analysis. Standardize name formats for mail merge.
- **Technical Details**: Use TRIM to remove extra spaces. Handle edge cases where delimiter might be missing. Consider case sensitivity for email domains. Chain with other functions like UPPER, LOWER for normalization.

### [[Log File and Data Processing]]
- **Implementation**: Extract log messages after timestamps: `=TEXTAFTER(log_line, "] ")`. Parse key-value pairs: `=TEXTAFTER(kv_pair, "=")`. Extract error details from structured log formats.
- **Business Application**: Analyze application logs in Excel for error patterns. Process exported data from systems with structured output. Create summaries of log entries by extracting relevant portions.
- **Technical Details**: Log formats vary; adjust delimiter to match your format. Use instance_num for logs with multiple delimiters. Combine with TEXTBEFORE to extract specific segments. Consider FILTERXML for XML-structured logs.

### [[URL and Web Data Parsing]]
- **Implementation**: Extract query strings: `=TEXTAFTER(url, "?")`. Get domain from URL: `=TEXTBEFORE(TEXTAFTER(url, "://"), "/")`. Parse specific URL parameters by chaining extractions.
- **Business Application**: Analyze web traffic by parsing URLs. Extract campaign parameters from marketing URLs. Process API responses containing URLs. Build URL component reports for SEO analysis.
- **Technical Details**: URLs may or may not have query strings—use if_not_found for graceful handling. Protocol (http/https) varies; extract after "://" for domain. Multiple parameters separated by "&" require iterative parsing or TEXTSPLIT.

## Platform Differences

| Feature | Excel 365 | Excel 2019/Earlier | Google Sheets |
|---------|-----------|-------------------|---------------|
| **Function Availability** | Native function | NOT AVAILABLE | NOT AVAILABLE |
| **Syntax** | =TEXTAFTER(text, delimiter, ...) | N/A | N/A |
| **Array Delimiters** | Supported {"-", "_"} | N/A | N/A |
| **Negative Instance** | Supported (-1 = last) | N/A | N/A |
| **Case Sensitivity Option** | match_mode parameter | N/A | N/A |
| **If Not Found** | if_not_found parameter | N/A | N/A |

**Alternative for Pre-365 Excel:**
```excel
=MID(A1, FIND("@", A1) + 1, LEN(A1))
```
This extracts after "@" in older Excel versions but lacks TEXTAFTER's flexibility.

For last occurrence:
```excel
=RIGHT(A1, LEN(A1) - FIND("☺", SUBSTITUTE(A1, "/", "☺", LEN(A1) - LEN(SUBSTITUTE(A1, "/", "")))))
```
This complex formula finds the last "/" using SUBSTITUTE to replace the Nth occurrence with a unique character.

**Alternative for Google Sheets:**
```
=REGEXEXTRACT(A1, "@(.+)")
```
Uses regex to capture everything after "@". More flexible but syntax differs significantly.

Or using traditional functions:
```
=MID(A1, FIND("@", A1) + 1, LEN(A1))
```

**Cross-Platform Strategy:**
- For files used across platforms, provide both formulas with comments
- Consider pre-processing in Excel 365, then sharing results
- Document Excel 365 requirements clearly
- Use helper columns with platform-neutral formulas when possible

## Tips and Best Practices

1. **Always Handle Missing Delimiters**: Use the if_not_found parameter to specify what to return when the delimiter isn't found: `=TEXTAFTER(A1, "@", 1, 0, 0, "")`. This prevents #N/A errors in your data.

2. **Use Negative Instance for End-Based Extraction**: When you need text after the LAST occurrence (like filenames from paths), use -1: `=TEXTAFTER(path, "\", -1)`. This is more reliable than counting occurrences.

3. **Consider Case Sensitivity**: If your delimiter might appear in different cases, set match_mode to 1 for case-insensitive matching. This is especially important for user-entered data.

4. **Combine with TRIM for Clean Output**: Wrap TEXTAFTER in TRIM to remove leading/trailing spaces from results: `=TRIM(TEXTAFTER(A1, ":"))`.

5. **Chain with TEXTBEFORE for Middle Extraction**: Extract text between two delimiters by nesting: `=TEXTBEFORE(TEXTAFTER(A1, "start"), "end")` extracts text between "start" and "end".

6. **Use Array Delimiters for Multiple Possible Separators**: When data might use different delimiters, specify an array: `=TEXTAFTER(A1, {"-", "_", " "})`. The function finds whichever appears first.

7. **Validate Your Results**: Spot-check extracted data, especially with variable-format source data. Edge cases like missing delimiters or multiple consecutive delimiters can produce unexpected results.

8. **Document Excel Version Requirements**: If sharing spreadsheets, clearly note that TEXTAFTER requires Excel 365. Provide alternative formulas for users on older versions.

9. **Use Named Ranges for Delimiters**: If using the same delimiter across many formulas, define it as a named range for easy updating and clarity.

10. **Consider TEXTSPLIT for Multiple Extractions**: If you need to parse text into multiple components, TEXTSPLIT may be more efficient than multiple TEXTAFTER/TEXTBEFORE calls.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from TEXTAFTER |
|----------|-------------|-------------------------------|
| [[TEXTBEFORE]] | Extracts text before a delimiter | Returns text before delimiter, not after |
| [[TEXTSPLIT]] | Splits text by delimiters into array | Returns all segments, not just portion after delimiter |
| [[MID]] | Extracts text by position and length | Position-based, not delimiter-based |
| [[RIGHT]] | Extracts characters from end of text | Character count based, not delimiter-based |
| [[FIND]]/[[SEARCH]] | Locates position of text | Returns position number, not extracted text |

### Commonly Used Together

**[[TEXTBEFORE]]** - Extract text before a delimiter

*Combine to extract text between two delimiters:*
```excel
=TEXTBEFORE(TEXTAFTER(A1, "<"), ">")
```
Extracts content between angle brackets.

---

**[[TRIM]]** - Clean whitespace from extracted text

*Remove extra spaces from extraction result:*
```excel
=TRIM(TEXTAFTER(A1, ":"))
```
Ensures clean output without leading/trailing spaces.

---

**[[IFERROR]]** - Handle cases where delimiter not found

*Provide fallback when extraction fails:*
```excel
=IFERROR(TEXTAFTER(A1, "@"), "No email")
```
Alternative to if_not_found parameter for complex fallback logic.

---

**[[LEN]]** - Calculate length of extracted text

*Measure the extracted portion:*
```excel
=LEN(TEXTAFTER(A1, "ID: "))
```
Useful for validation or further processing.

---

**[[TEXTSPLIT]]** - Split text into multiple parts

*Parse text into array, then reference specific elements:*
```excel
=INDEX(TEXTSPLIT(A1, "-"), 2)
```
For complex parsing, TEXTSPLIT may be more efficient.

---

**[[UPPER]]/[[LOWER]]** - Normalize case of extracted text

*Standardize extracted text:*
```excel
=UPPER(TEXTAFTER(A1, "@"))
```
Normalizes domain extraction for comparison purposes.

## Official Documentation

- **Microsoft Excel**: [TEXTAFTER function](https://support.microsoft.com/en-us/office/textafter-function-c8db2546-5b51-416a-9690-f76a6b838f80)

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
