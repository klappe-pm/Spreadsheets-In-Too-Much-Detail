---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- text-search
- position-finding
- case-sensitive-search
- string-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Find Text
- Locate Text
- Case Sensitive Search
- Text Position
tags:
- text
- search
- find
- position
- case-sensitive
- location
---

# FIND

## Description

**FIND** locates one text string within another and returns the position of its first character. The function is case-sensitive, meaning "Apple" and "apple" are treated as completely different strings. This precision makes FIND essential for tasks requiring exact matching, such as parsing structured data, validating formats, or extracting substrings where case matters. When you need to know exactly where a character or substring appears, FIND delivers that position as a number.

The function returns the position using 1-based indexing, where the first character is position 1. If the search text is found, FIND returns the position of its first character within the larger string. If not found, it returns a #VALUE! error, which is important for error handling in formulas. An optional third parameter allows you to start the search from a position other than the beginning, enabling you to find second, third, or nth occurrences.

**Critical gotcha:** FIND does not support wildcards. Unlike SEARCH, you cannot use ? or * for pattern matching. Every character in the find_text must match exactly. Additionally, FIND cannot locate empty strings in a meaningful way; =FIND("", A1) returns 1, which represents the position before the first character. The case sensitivity is absolute; there is no option to make FIND case-insensitive (use SEARCH instead).

**Platform consideration:** Excel and Google Sheets implement FIND identically. Both are case-sensitive, both use 1-based positioning, both return #VALUE! when text is not found, and both support the optional start position. Formulas are fully portable between platforms with no behavioral differences.

## Syntax

> [!f(x)] FIND Syntax
>
> ```
> =FIND(find_text, within_text, [start_num])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `find_text` | Yes | The text you want to find. Case-sensitive. Does not support wildcards. |
| `within_text` | Yes | The text string in which to search. Can be a cell reference or literal text. |
| `start_num` | No | The position to start searching. Default is 1 (beginning). Use to find subsequent occurrences. |

### Return Value

Returns the position of the first character of find_text within within_text, as a number. Returns 1 for the first character position. Returns #VALUE! if find_text is not found or if start_num is invalid.

## Examples

> [!f(x)] FIND Examples

### Example 1: Basic Text Location
```
=FIND("World", "Hello World")
```
**Result:** 7

**Explanation:** "World" begins at the 7th character of "Hello World". Spaces count as characters.

---

### Example 2: Case Sensitivity Demonstration
```
=FIND("world", "Hello World")
```
**Result:** #VALUE!

**Explanation:** "world" (lowercase) is not found because FIND is case-sensitive. "World" with capital W would be found.

---

### Example 3: Finding a Single Character
```
=FIND("@", "user@example.com")
```
**Result:** 5

**Explanation:** The @ symbol is at position 5. Useful for parsing email addresses.

---

### Example 4: Using Start Position
```
=FIND("o", "Hello World", 5)
```
**Result:** 8

**Explanation:** Starting search at position 5, the first "o" found is at position 8 (in "World"). The "o" in "Hello" at position 5 is at the start boundary so not matched.

---

### Example 5: Find Second Occurrence
```
=FIND("l", "Hello World", FIND("l", "Hello World")+1)
```
**Result:** 4

**Explanation:** First FIND("l") returns 3. Adding 1 and using as start position finds the next "l" at position 4.

---

### Example 6: Cell Reference
```
=FIND("-", A1)
```
Where A1 contains "123-456-7890"

**Result:** 4

**Explanation:** Finds the first hyphen at position 4. Fundamental for parsing formatted data.

---

### Example 7: Find Space in Name
```
=FIND(" ", "John Smith")
```
**Result:** 5

**Explanation:** The space between first and last name is at position 5. Essential for name parsing.

---

### Example 8: Extract First Name Using FIND
```
=LEFT(A1, FIND(" ", A1)-1)
```
Where A1 contains "John Smith"

**Result:** "John"

**Explanation:** FIND locates the space, then LEFT extracts everything before it. Classic name parsing pattern.

---

### Example 9: Extract Domain from Email
```
=MID(A1, FIND("@", A1)+1, LEN(A1))
```
Where A1 contains "user@example.com"

**Result:** "example.com"

**Explanation:** FIND locates @, MID extracts everything after it. Domain extraction formula.

---

### Example 10: Check If Text Contains Substring
```
=ISNUMBER(FIND("error", A1))
```
Where A1 contains "No errors found"

**Result:** FALSE

**Explanation:** "error" is not found (case-sensitive), so FIND returns #VALUE!, which ISNUMBER converts to FALSE. Use SEARCH for case-insensitive check.

---

### Example 11: Error Handling with IFERROR
```
=IFERROR(FIND("x", A1), 0)
```
Where A1 contains "Hello"

**Result:** 0

**Explanation:** "x" is not found, so FIND returns #VALUE!. IFERROR catches this and returns 0 instead.

---

### Example 12: Find Multiple Delimiters
```
=MIN(IFERROR(FIND("-", A1), 999), IFERROR(FIND("/", A1), 999), IFERROR(FIND(".", A1), 999))
```
Where A1 contains "2024-01-15"

**Result:** 5 (position of first delimiter found)

**Explanation:** Finds whichever delimiter appears first. IFERROR handles cases where a delimiter is absent.

---

### Example 13: Parse File Extension
```
=MID(A1, FIND(".", A1)+1, LEN(A1))
```
Where A1 contains "document.pdf"

**Result:** "pdf"

**Explanation:** Finds the period, then extracts everything after it. For multiple periods, find the last one instead.

---

### Example 14: Find Last Occurrence
```
=LEN(A1)-FIND("~", SUBSTITUTE(A1, "/", "~", LEN(A1)-LEN(SUBSTITUTE(A1, "/", ""))))+1
```
Where A1 contains "path/to/file/name.txt"

**Result:** 14 (position of last "/")

**Explanation:** Complex technique to find last occurrence. Substitute all but last with unique char, then find that char.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | find_text not found in within_text | Use IFERROR to handle not-found cases. Check for case sensitivity issues. |
| `#VALUE!` | start_num less than 1 or greater than within_text length | Ensure start_num is within valid range. |
| `#VALUE!` | within_text is empty | Check for empty cells before applying FIND. |
| `Wrong position returned` | Case sensitivity not accounted for | Use SEARCH for case-insensitive matching, or use UPPER/LOWER on both texts. |
| `Always returns 1` | find_text is empty string | Empty string is found at position 1 by definition. Validate input. |

## Use Cases

### [[Email Address Parsing]]

**Scenario:** A contact database has email addresses in a single column. Need to separate username and domain into separate columns for analysis and validation.

**Implementation:**
```
Username: =LEFT(A2, FIND("@", A2)-1)
Domain: =MID(A2, FIND("@", A2)+1, LEN(A2))
Is Gmail: =ISNUMBER(FIND("gmail", LOWER(A2)))
```

**Business Application:** Email marketing segmentation, domain analysis for lead scoring, and data quality validation. Separate components enable targeted filtering and analysis.

**Technical Details:** FIND locates the @ symbol exactly once per email. For validation, wrap in IFERROR to catch emails without @. The LOWER in the Gmail check enables case-insensitive domain matching.

---

### [[Name Field Splitting]]

**Scenario:** Customer names are stored as "FirstName LastName" in a single field. Need to split into separate First and Last name columns for personalization and sorting.

**Implementation:**
```
First Name: =LEFT(A2, FIND(" ", A2)-1)
Last Name: =MID(A2, FIND(" ", A2)+1, LEN(A2))
```
For names with middle names:
```
First Name: =LEFT(A2, FIND(" ", A2)-1)
Last Name: =MID(A2, FIND(" ", A2, FIND(" ", A2)+1)+1, LEN(A2))
```

**Business Application:** Mail merge personalization, alphabetical sorting by last name, and CRM data organization. Separated name components enable more flexible use.

**Technical Details:** The basic formula assumes single space between names. For middle names, find the second space by starting the search after the first space. Handle edge cases (no space, multiple spaces) with IFERROR.

---

### [[File Path Parsing]]

**Scenario:** System logs contain full file paths. Need to extract file name, extension, or directory portions for analysis and reporting.

**Implementation:**
```
File Name (simple): =MID(A2, FIND("~", SUBSTITUTE(A2, "\", "~", LEN(A2)-LEN(SUBSTITUTE(A2, "\", ""))))+1, LEN(A2))
Extension: =MID(A2, FIND(".", A2)+1, LEN(A2))
```

**Business Application:** Log file analysis, file inventory management, and storage reporting. Parsing paths enables grouping by directory, extension, or naming pattern.

**Technical Details:** Finding the last backslash requires the SUBSTITUTE trick since FIND only finds first occurrence. For extensions, handle files with multiple periods by finding the last period.

---

### [[Data Validation and Format Checking]]

**Scenario:** Validate that entered data matches expected formats: email contains @, phone has hyphens in right positions, date has proper separators.

**Implementation:**
```
Valid Email: =AND(ISNUMBER(FIND("@", A2)), FIND("@", A2)>1, FIND(".", A2, FIND("@", A2))>FIND("@", A2)+1)
Valid Phone Format: =AND(FIND("-", A2)=4, FIND("-", A2, 5)=8)
Has Extension: =ISNUMBER(FIND(".", A2))
```

**Business Application:** Data entry validation, import file verification, and data quality dashboards. Prevent bad data from entering systems and identify existing data issues.

**Technical Details:** Combine multiple FIND calls with AND logic to validate format requirements. Use ISNUMBER to convert FIND errors to FALSE for logical tests. Layer validations for complex rules.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 onward)
- **Case sensitivity:** Always case-sensitive, no option to change
- **Wildcards:** Not supported (use SEARCH for wildcards)
- **Return type:** Returns number; #VALUE! if not found
- **FINDB:** Available for byte-based positioning in DBCS languages

### Google Sheets
- **Availability:** All versions since launch
- **Behavior:** Identical to Excel in all parameters
- **Array support:** Works with ARRAYFORMULA for range processing
- **Wildcards:** Same as Excel (not supported)
- **FINDB:** Not available (Excel only)

### Key Difference Alert
Both platforms implement FIND identically for standard use. Excel offers FINDB for byte-counting in double-byte character sets (Asian languages), which Google Sheets lacks. For Western text and standard Unicode, no differences exist. Formulas are fully portable.

## Tips and Best Practices

1. **Always handle #VALUE! errors:** FIND returns #VALUE! when text is not found. Wrap in IFERROR or IF(ISNUMBER()) for robust formulas: `=IFERROR(FIND("x", A1), 0)`.

2. **Use SEARCH for case-insensitive matching:** If case does not matter, use SEARCH instead of FIND. SEARCH is identical except for case sensitivity.

3. **Find nth occurrence with start position:** To find the second occurrence, use the first result + 1 as start position: `=FIND("x", A1, FIND("x", A1)+1)`.

4. **Combine with LEFT, MID, RIGHT:** FIND provides positions; extraction functions use those positions: `=LEFT(A1, FIND("-", A1)-1)` extracts before the hyphen.

5. **Use ISNUMBER for boolean checks:** `=ISNUMBER(FIND("text", A1))` returns TRUE if text is present, FALSE if not. Cleaner than comparing FIND to a number.

6. **Empty find_text returns 1:** FIND("", A1) always returns 1. Validate input if empty search strings are possible.

7. **Finding last occurrence requires tricks:** FIND only finds the first. For last occurrence, use SUBSTITUTE to mark the last instance, then find that marker.

8. **Position is 1-based:** First character is position 1, not 0. Remember this when calculating lengths or positions for other functions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SEARCH-]] | Case-insensitive text search | When case matching is not required; also supports wildcards |
| [[MATCH]] | Finds position in array | When searching in a range rather than within text |

### Commonly Used Together

**[[LEFT]]** - Extracts left characters

*Extract text before found position:*
```
=LEFT(A1, FIND("-", A1)-1)
```
Classic pattern: find delimiter, extract everything before it.

---

**[[MID]]** - Extracts middle characters

*Extract text after found position:*
```
=MID(A1, FIND("@", A1)+1, LEN(A1))
```
Find delimiter, extract everything after it.

---

**[[RIGHT]]** - Extracts right characters

*Combined with LEN and FIND:*
```
=RIGHT(A1, LEN(A1)-FIND("@", A1))
```
Alternative to MID for extracting after a found position.

---

**[[LEN]]** - String length

*Calculate remaining length:*
```
=LEN(A1)-FIND(" ", A1)
```
Determine how many characters follow the found position.

---

**[[IFERROR]]** - Error handling

*Handle not-found cases:*
```
=IFERROR(FIND("x", A1), -1)
```
Return alternative value when find_text is not present.

---

**[[ISNUMBER]]** - Check for number result

*Boolean check for text presence:*
```
=ISNUMBER(FIND("error", A1))
```
Returns TRUE if text is found, FALSE if not.

---

**[[SEARCH-]]** - Case-insensitive alternative

*When case doesn't matter:*
```
Case-sensitive: =FIND("Apple", A1)
Case-insensitive: =SEARCH("Apple", A1)
```
SEARCH finds "Apple", "apple", "APPLE" equally.

---

**[[SUBSTITUTE]]** - Find last occurrence helper

*Mark last occurrence for finding:*
```
=SUBSTITUTE(A1, "/", "~", LEN(A1)-LEN(SUBSTITUTE(A1, "/", "")))
```
Replaces only the last occurrence with a unique marker for FIND.

## Official Documentation

- **Microsoft Excel:** [FIND function](https://support.microsoft.com/en-us/office/find-findb-functions-c7912941-af2a-4bdf-a553-d0d89b0a0628)
- **Google Sheets:** [FIND function](https://support.google.com/docs/answer/3094126)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | Identical implementation to Excel |

---

*Last updated: 2026-01-10*
