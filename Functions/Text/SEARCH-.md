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
- case-insensitive-search
- wildcard-matching
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Search Text
- Case Insensitive Find
- Wildcard Search
- Text Position
tags:
- text
- search
- find
- position
- case-insensitive
- wildcards
---

# SEARCH

## Description

**SEARCH** locates one text string within another and returns the position of its first character, performing a case-insensitive match. This flexibility makes SEARCH the preferred choice when you need to find text regardless of capitalization. "APPLE", "Apple", and "apple" are all treated as equivalent matches. Additionally, SEARCH supports wildcard characters (? and *), enabling pattern matching that FIND cannot perform.

The function returns positions using 1-based indexing, where the first character is position 1. When the search text is found, SEARCH returns the starting position; when not found, it returns a #VALUE! error. An optional third parameter specifies where to begin searching, allowing you to find subsequent occurrences of a pattern within the same string.

**Key capability:** SEARCH supports two wildcards: ? matches any single character, and * matches any sequence of characters (including none). This enables powerful pattern matching: "a?c" finds "abc", "aXc", "a1c", etc. "a*z" finds "az", "abcz", "a123xyz", etc. To search for literal ? or * characters, precede them with a tilde (~): "~?" finds an actual question mark.

**Platform consideration:** Excel and Google Sheets implement SEARCH identically. Both are case-insensitive, both support wildcards with the same syntax, both use 1-based positioning, and both return #VALUE! when no match is found. Formulas are fully portable between platforms.

## Syntax

> [!f(x)] SEARCH Syntax
>
> ```
> =SEARCH(find_text, within_text, [start_num])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `find_text` | Yes | The text you want to find. Case-insensitive. Supports wildcards (? for single char, * for any sequence). |
| `within_text` | Yes | The text string in which to search. Can be a cell reference or literal text. |
| `start_num` | No | The position to start searching. Default is 1 (beginning). Use to find subsequent occurrences. |

### Return Value

Returns the position of the first character of the matched text within within_text, as a number. Returns 1 for the first character position. Returns #VALUE! if no match is found or if start_num is invalid.

## Examples

> [!f(x)] SEARCH Examples

### Example 1: Basic Case-Insensitive Search
```
=SEARCH("world", "Hello World")
```
**Result:** 7

**Explanation:** "world" (lowercase) matches "World" in the string. Case does not matter.

---

### Example 2: Compare to FIND's Case Sensitivity
```
=SEARCH("APPLE", "I have an apple")
```
**Result:** 12

**Explanation:** "APPLE" matches "apple" in the string. FIND would return #VALUE! for this same search.

---

### Example 3: Single Character Wildcard (?)
```
=SEARCH("b?t", "The bat flew to the bot")
```
**Result:** 5

**Explanation:** "b?t" matches "bat" starting at position 5. The ? matches any single character.

---

### Example 4: Multi-Character Wildcard (*)
```
=SEARCH("b*t", "The best bet")
```
**Result:** 5

**Explanation:** "b*t" matches "best" starting at position 5. The * matches "es" (any sequence).

---

### Example 5: Wildcard at End
```
=SEARCH("doc*", "document.pdf")
```
**Result:** 1

**Explanation:** "doc*" matches "document.pdf" starting at position 1. The * matches everything after "doc".

---

### Example 6: Cell Reference
```
=SEARCH("@", A1)
```
Where A1 contains "user@EXAMPLE.com"

**Result:** 5

**Explanation:** Finds the @ symbol at position 5. SEARCH works identically to FIND for non-letter searches.

---

### Example 7: Using Start Position
```
=SEARCH("e", "Hello Everyone", 5)
```
**Result:** 7

**Explanation:** Starting at position 5, finds "E" in "Everyone" at position 7 (case-insensitive match).

---

### Example 8: Find Second Occurrence
```
=SEARCH("o", "Hello World", SEARCH("o", "Hello World")+1)
```
**Result:** 8

**Explanation:** First SEARCH returns 5. Starting at 6, the second "o" is found at position 8.

---

### Example 9: Check If Contains (Case-Insensitive)
```
=ISNUMBER(SEARCH("error", A1))
```
Where A1 contains "ERROR: File not found"

**Result:** TRUE

**Explanation:** "error" matches "ERROR" case-insensitively. ISNUMBER converts the position to TRUE or #VALUE! to FALSE.

---

### Example 10: Finding Literal Asterisk
```
=SEARCH("~*", "Price: $5*each")
```
**Result:** 11

**Explanation:** ~* searches for a literal asterisk, not a wildcard. Found at position 11.

---

### Example 11: Finding Literal Question Mark
```
=SEARCH("~?", "Is this correct?")
```
**Result:** 16

**Explanation:** ~? searches for a literal question mark, not a wildcard. Found at position 16.

---

### Example 12: Pattern Matching for File Types
```
=SEARCH("*.pdf", A1)
```
Where A1 contains "report_2024.pdf"

**Result:** 1

**Explanation:** The pattern matches any filename ending in ".pdf". Returns 1 because pattern matches from start.

---

### Example 13: Error Handling
```
=IFERROR(SEARCH("xyz", A1), "Not Found")
```
Where A1 contains "Hello World"

**Result:** "Not Found"

**Explanation:** "xyz" is not in the string, so SEARCH returns #VALUE!. IFERROR catches this.

---

### Example 14: Extract After Pattern Match
```
=MID(A1, SEARCH("ID:", A1)+3, 10)
```
Where A1 contains "Customer ID:12345678"

**Result:** "12345678" (first 10 chars after "ID:")

**Explanation:** Finds "ID:" position, then extracts characters after it. Case-insensitive so "ID:", "id:", "Id:" all work.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | find_text not found in within_text | Use IFERROR to handle not-found cases. |
| `#VALUE!` | start_num less than 1 or greater than within_text length | Ensure start_num is within valid range. |
| `#VALUE!` | Unescaped wildcard when searching for literal | Use ~* to find asterisk, ~? to find question mark. |
| `Unexpected match position` | Wildcard matched earlier than expected | Wildcards are greedy; * matches as much as possible. Refine pattern. |
| `Always returns 1` | find_text is empty string or * alone | * matches empty string at position 1. Validate input. |

## Use Cases

### [[Case-Insensitive Text Classification]]

**Scenario:** Customer feedback data needs to be categorized by keywords, but customers use varied capitalization: "URGENT", "urgent", "Urgent" should all be treated identically.

**Implementation:**
```
Contains Urgent: =ISNUMBER(SEARCH("urgent", A2))
Contains Error: =ISNUMBER(SEARCH("error", A2))
Category: =IF(ISNUMBER(SEARCH("urgent", A2)), "High Priority", IF(ISNUMBER(SEARCH("question", A2)), "Inquiry", "General"))
```

**Business Application:** Support ticket classification, feedback analysis, and automated routing. Case-insensitive matching ensures consistent categorization regardless of how customers typed.

**Technical Details:** Use ISNUMBER(SEARCH()) pattern for boolean checks. Chain IF statements or use IFS for multiple categories. Combine with SUMPRODUCT for counting matches across a range.

---

### [[File Type Detection with Wildcards]]

**Scenario:** A file inventory needs to identify files by extension patterns. Some files have multiple extensions or variations that need matching.

**Implementation:**
```
Is Image: =OR(ISNUMBER(SEARCH("*.jpg", A2)), ISNUMBER(SEARCH("*.png", A2)), ISNUMBER(SEARCH("*.gif", A2)))
Is Document: =OR(ISNUMBER(SEARCH("*.doc*", A2)), ISNUMBER(SEARCH("*.pdf", A2)))
```

**Business Application:** File management, storage analysis, and compliance auditing. Pattern matching handles extension variations (.doc, .docx) elegantly.

**Technical Details:** *.doc* matches .doc, .docx, .docm, etc. Combine multiple SEARCH patterns with OR for comprehensive type detection. Consider case variations handled automatically.

---

### [[Flexible Data Extraction]]

**Scenario:** Data fields have varying formats but consistent patterns. Extract values that follow keywords regardless of exact formatting.

**Implementation:**
```
After "Total:": =MID(A2, SEARCH("total:", A2)+6, 20)
After any "ID" format: =MID(A2, SEARCH("ID?", A2)+3, 10)
```

**Business Application:** Report parsing, data extraction from text, and format normalization. Flexible patterns handle human-entered data with formatting variations.

**Technical Details:** SEARCH finds the pattern position, MID/RIGHT extracts content after it. Adjust extraction length for your data. Use TRIM on results to clean spacing.

---

### [[User Input Validation]]

**Scenario:** Form inputs need validation that allows for common user variations. Email domains, product codes, or reference numbers might be entered in any case.

**Implementation:**
```
Valid Domain: =ISNUMBER(SEARCH("@company.com", A2))
Has Required Prefix: =ISNUMBER(SEARCH("REF-*-2024", A2))
Valid Format: =AND(ISNUMBER(SEARCH("???-????", A2)), LEN(A2)=8)
```

**Business Application:** Form validation, data entry quality control, and automated data cleaning. Case-insensitive checks prevent false rejections from capitalization differences.

**Technical Details:** Combine SEARCH patterns with AND/OR for complex validations. Use LEN checks alongside pattern matching for strict format validation.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 onward)
- **Case sensitivity:** Always case-insensitive
- **Wildcards:** Supports ? (single char) and * (any sequence)
- **Escape character:** ~ before ? or * for literal matching
- **SEARCHB:** Available for byte-based positioning in DBCS languages

### Google Sheets
- **Availability:** All versions since launch
- **Behavior:** Identical to Excel in all parameters
- **Array support:** Works with ARRAYFORMULA for range processing
- **Wildcards:** Same syntax and behavior as Excel
- **SEARCHB:** Not available (Excel only)

### Key Difference Alert
Both platforms implement SEARCH identically. Excel offers SEARCHB for byte-counting in double-byte character sets (Asian languages), which Google Sheets lacks. For Western text and standard Unicode usage, no differences exist. Formulas are fully portable.

## Tips and Best Practices

1. **Use SEARCH for user-facing data:** When matching user input where capitalization varies, SEARCH's case-insensitivity prevents frustrating mismatches.

2. **Leverage wildcards for patterns:** Use ? for single unknown characters, * for any sequence. "A?C" matches "ABC", "AXC", etc. "A*Z" matches "AZ", "A123Z", etc.

3. **Escape wildcards with tilde:** To find literal ? or *, use ~? or ~*. This is easy to forget and causes unexpected results.

4. **ISNUMBER for boolean checks:** `=ISNUMBER(SEARCH("text", A1))` cleanly returns TRUE/FALSE. Preferred over comparing to a number or using error handling.

5. **Handle #VALUE! errors:** SEARCH returns #VALUE! when not found. Always use IFERROR for production formulas: `=IFERROR(SEARCH("x", A1), 0)`.

6. **Use FIND when case matters:** For case-sensitive matching or when wildcards would cause problems, use FIND instead of SEARCH.

7. **Start position for nth occurrence:** Find the second match: `=SEARCH("x", A1, SEARCH("x", A1)+1)`. Each call finds the next occurrence.

8. **Wildcards are greedy:** * matches as much as possible. "a*b" in "aXbYb" matches "aXbYb", not just "aXb". Design patterns carefully.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FIND]] | Case-sensitive text search | When exact case matching is required; no wildcard support |
| [[MATCH]] | Finds position in array | When searching in a range rather than within text; supports wildcards |

### Commonly Used Together

**[[LEFT]]** - Extracts left characters

*Extract text before found pattern:*
```
=LEFT(A1, SEARCH("-", A1)-1)
```
Find delimiter with SEARCH, extract everything before it.

---

**[[MID]]** - Extracts middle characters

*Extract text after found pattern:*
```
=MID(A1, SEARCH("@", A1)+1, LEN(A1))
```
Find pattern, extract everything after it.

---

**[[RIGHT]]** - Extracts right characters

*Combined with LEN and SEARCH:*
```
=RIGHT(A1, LEN(A1)-SEARCH(":", A1))
```
Alternative to MID for extracting after a found position.

---

**[[LEN]]** - String length

*Calculate remaining length:*
```
=LEN(A1)-SEARCH(" ", A1)
```
Determine how many characters follow the found position.

---

**[[IFERROR]]** - Error handling

*Handle not-found cases:*
```
=IFERROR(SEARCH("xyz", A1), -1)
```
Return alternative value when pattern is not found.

---

**[[ISNUMBER]]** - Check for number result

*Boolean check for pattern presence:*
```
=ISNUMBER(SEARCH("error", A1))
```
Returns TRUE if pattern is found, FALSE if not. Clean boolean result.

---

**[[FIND]]** - Case-sensitive alternative

*When case matters:*
```
Case-insensitive: =SEARCH("Apple", A1)
Case-sensitive: =FIND("Apple", A1)
```
FIND only matches exact case; SEARCH matches any case.

---

**[[SUBSTITUTE]]** - Text replacement

*Combine for pattern-based operations:*
```
=SUBSTITUTE(A1, MID(A1, SEARCH("ID:", A1), 10), "ID:MASKED")
```
Find pattern position, then replace matched text.

## Official Documentation

- **Microsoft Excel:** [SEARCH function](https://support.microsoft.com/en-us/office/search-searchb-functions-9ab04538-0e55-4719-a72e-b6f54513b495)
- **Google Sheets:** [SEARCH function](https://support.google.com/docs/answer/3094154)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function with wildcard support |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | Identical implementation to Excel |

---

*Last updated: 2026-01-10*
