---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - text-manipulation
  - pattern-matching
subTopics:
  - regular-expressions
  - data-validation
  - text-testing
  - regex
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - regex-match
  - pattern-match
  - regex-test
tags:
  - text
  - regex
  - pattern-matching
  - validation
  - google-sheets
  - sheets-only
---

# REGEXMATCH

## Description

REGEXMATCH is a Google Sheets-exclusive function that tests whether a text string matches a regular expression pattern, returning TRUE if any part of the text matches the pattern and FALSE otherwise. Unlike REGEXEXTRACT which pulls matching text out of a string, REGEXMATCH simply answers the question "does this pattern exist somewhere in this text?" This makes it the ideal function for data validation, conditional logic, filtering, and any scenario where you need to test for the presence of a pattern without extracting it. The function evaluates to a Boolean result, making it perfect for use within IF statements, FILTER functions, conditional formatting rules, and data validation criteria.

The primary use cases for REGEXMATCH center around validation and conditional operations. Common applications include validating email addresses, phone numbers, or postal codes against expected formats; checking if text contains specific keywords or character patterns; filtering datasets to show only rows matching certain criteria; creating conditional formatting rules based on pattern matching; validating user input in data entry sheets; identifying rows containing errors, special codes, or flags; and categorizing text data based on pattern characteristics. REGEXMATCH is particularly powerful when combined with functions like IF, IFS, FILTER, and SUMIF to create sophisticated conditional logic based on pattern matching rather than exact string comparison.

Several important considerations apply when working with REGEXMATCH. First, the function returns TRUE if the pattern matches ANY part of the text, not necessarily the entire text—use ^ and $ anchors if you need to match the complete string. Second, like other Sheets regex functions, REGEXMATCH uses RE2 syntax which lacks lookahead/lookbehind and backreferences. Third, the function is case-sensitive by default; prepend (?i) to your pattern for case-insensitive matching. Fourth, be careful with overly broad patterns that might match unintended text—testing for \d+ (any digits) will match on any string containing numbers. Fifth, when used in FILTER or conditional formatting, ensure the pattern reliably identifies only the intended rows. Sixth, empty strings will return FALSE for most patterns (except patterns designed to match empty strings), but cells with spaces may match patterns like \s or ..

REGEXMATCH is available exclusively in Google Sheets and has no direct equivalent function in Microsoft Excel. Excel users seeking similar functionality must use the SEARCH or FIND functions combined with ISNUMBER for simple substring checks, or resort to VBA, Power Query, or complex nested formulas for true pattern matching. This significant feature gap makes REGEXMATCH one of the key advantages Google Sheets holds over Excel for text processing tasks. When building spreadsheets that must work across both platforms, avoid REGEXMATCH and instead use SEARCH/FIND combinations like =ISNUMBER(SEARCH("pattern", A1)) for basic substring detection, accepting the limitation of no true regex pattern matching.

## Syntax

> [!f(x)] REGEXMATCH Syntax
>
> ```
> =REGEXMATCH(text, regular_expression)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string to test against the regular expression. Can be a cell reference, literal text in quotes, or a formula returning text. Non-text values are converted to text before matching. Empty cells are treated as empty strings. | Yes |
> | `regular_expression` | The regular expression pattern to test for. Must be a valid RE2 regular expression in quotes. Returns TRUE if any portion of text matches the pattern, FALSE otherwise. | Yes |

## Examples

### Example 1: Validate Email Format
```
=REGEXMATCH(A1, "^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$")
```
Where A1 contains "user@example.com"

**Result:** `TRUE`

**Explanation:** This pattern validates the complete email format. The ^ and $ anchors ensure the entire cell content must match (not just contain a valid email). This distinguishes between "user@example.com" (TRUE) and "Contact: user@example.com" (FALSE because of extra text).

---

### Example 2: Check for Phone Number Presence
```
=REGEXMATCH(A1, "\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}")
```
Where A1 contains "Call us at (555) 123-4567 for support"

**Result:** `TRUE`

**Explanation:** This tests whether text contains a phone number pattern, regardless of where it appears in the string. Unlike the validation example, no anchors are used, so it finds the phone number within the longer text.

---

### Example 3: Test for Any Numeric Content
```
=REGEXMATCH(A1, "\d")
```
Where A1 contains "Item #42 purchased"

**Result:** `TRUE`

**Explanation:** The simple pattern \d matches any single digit. If any digit exists anywhere in the text, the function returns TRUE. This is useful for quick checks like "does this text contain any numbers?"

---

### Example 4: Case-Insensitive Keyword Detection
```
=REGEXMATCH(A1, "(?i)urgent|priority|asap")
```
Where A1 contains "This is an URGENT request"

**Result:** `TRUE`

**Explanation:** The (?i) flag enables case-insensitive matching. The | operator provides OR logic between multiple keywords. This matches "urgent", "URGENT", "Priority", "ASAP", or any case variation.

---

### Example 5: Validate US ZIP Code Format
```
=REGEXMATCH(A1, "^\d{5}(-\d{4})?$")
```
Where A1 contains "12345"

**Result:** `TRUE`

Where A1 contains "12345-6789"

**Result:** `TRUE`

Where A1 contains "1234"

**Result:** `FALSE`

**Explanation:** This validates US ZIP codes in either 5-digit or ZIP+4 format. The ^ and $ anchors ensure the entire cell must match. The (-\d{4})? makes the 4-digit extension optional.

---

### Example 6: Use with IF for Conditional Logic
```
=IF(REGEXMATCH(A1, "^[A-Z]{2,3}-\d{4,6}$"), "Valid SKU", "Invalid Format")
```
Where A1 contains "ABC-12345"

**Result:** `Valid SKU`

**Explanation:** REGEXMATCH returns TRUE/FALSE, making it perfect for IF conditions. This validates a product SKU format and returns appropriate feedback. The pattern requires 2-3 uppercase letters, hyphen, and 4-6 digits.

---

### Example 7: Filter Rows Containing URLs
```
=FILTER(A2:B10, REGEXMATCH(A2:A10, "https?://"))
```
Data in A2:A10 contains various text, some with URLs

**Result:** Array of rows where column A contains URLs

**Explanation:** REGEXMATCH works beautifully with FILTER to select rows matching a pattern. This filters for any row containing "http://" or "https://". The entire A:B columns from matching rows are returned.

---

### Example 8: Validate Date Format (MM/DD/YYYY)
```
=REGEXMATCH(A1, "^(0[1-9]|1[0-2])/(0[1-9]|[12]\d|3[01])/(19|20)\d{2}$")
```
Where A1 contains "12/25/2024"

**Result:** `TRUE`

**Explanation:** This comprehensive pattern validates proper date format with sensible ranges: months 01-12, days 01-31, and years 1900-2099. Note this doesn't validate actual dates (Feb 30 would pass), just format.

---

### Example 9: Detect Credit Card Number Pattern
```
=REGEXMATCH(A1, "\d{4}[-\s]?\d{4}[-\s]?\d{4}[-\s]?\d{4}")
```
Where A1 contains "Payment: 4111-1111-1111-1111"

**Result:** `TRUE`

**Explanation:** This detects a 16-digit pattern that could be a credit card number, with optional hyphens or spaces between groups. Useful for identifying potential PII in datasets for compliance reviews.

---

### Example 10: Check for Special Characters
```
=REGEXMATCH(A1, "[^A-Za-z0-9\s]")
```
Where A1 contains "Hello World!"

**Result:** `TRUE`

**Explanation:** The [^...] creates a negated character class—matching anything NOT in the list. This returns TRUE if text contains any character other than letters, numbers, or whitespace. The exclamation mark triggers TRUE here.

---

### Example 11: Validate Strong Password Pattern
```
=REGEXMATCH(A1, "^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$")
```

**Note:** This pattern uses lookahead (?=...) which is NOT supported in Google Sheets RE2.

**Working Alternative:**
```
=AND(REGEXMATCH(A1, "[a-z]"), REGEXMATCH(A1, "[A-Z]"), REGEXMATCH(A1, "\d"), LEN(A1)>=8)
```
Where A1 contains "Password1"

**Result:** `TRUE`

**Explanation:** Since RE2 doesn't support lookahead, use multiple REGEXMATCH calls combined with AND. This validates: at least one lowercase, one uppercase, one digit, and minimum 8 characters.

---

### Example 12: Detect HTML Tags
```
=REGEXMATCH(A1, "<[^>]+>")
```
Where A1 contains "Click <a href='link'>here</a> for more"

**Result:** `TRUE`

**Explanation:** This pattern matches HTML tags: < followed by one or more characters that aren't >, then >. Useful for identifying cells with embedded HTML that might need cleaning.

---

### Example 13: Check for Valid Hex Color Code
```
=REGEXMATCH(A1, "^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$")
```
Where A1 contains "#FF5733"

**Result:** `TRUE`

Where A1 contains "#F53"

**Result:** `TRUE`

**Explanation:** Validates CSS hex color codes in both 6-character (#FF5733) and 3-character (#F53) shorthand formats. The ^ and $ ensure the entire content is just the color code.

---

### Example 14: Conditional Formatting Formula
```
=REGEXMATCH($A1, "(?i)error|fail|exception")
```
Applied as conditional formatting rule to highlight rows

**Result:** Cells in rows containing error indicators are highlighted

**Explanation:** When used in conditional formatting, REGEXMATCH enables pattern-based highlighting. This highlights any row where column A contains "error", "fail", or "exception" in any case.

---

### Example 15: SUMIF with Pattern Matching
```
=SUMPRODUCT((REGEXMATCH(A2:A100, "^SALE-"))*B2:B100)
```
Where A column contains transaction codes and B column contains amounts

**Result:** Sum of amounts where transaction code starts with "SALE-"

**Explanation:** Since SUMIF doesn't support regex, use SUMPRODUCT with REGEXMATCH. The TRUE/FALSE values become 1/0, multiplied by amounts, then summed. Only "SALE-" prefixed rows contribute to the total.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Empty or invalid regular expression | Check pattern syntax; ensure quotes around pattern |
| `#REF!` | Invalid regex syntax (unbalanced brackets, etc.) | Validate regex pattern; check for unescaped special characters |
| `#NAME?` | Function name misspelled or using in Excel | Verify spelling; REGEXMATCH is Sheets-only |
| `Always returns FALSE` | Pattern doesn't match as expected | Test pattern with simpler text; check case sensitivity |
| `Always returns TRUE` | Pattern too broad, matches unintended text | Make pattern more specific; add anchors (^, $) if needed |
| `Case mismatch` | Pattern is case-sensitive by default | Add (?i) at start of pattern for case-insensitive matching |
| `Anchor confusion` | Expecting full-text match without ^ and $ | Use ^pattern$ to match entire cell content |
| `Lookahead error` | Using (?=...) or (?!...) not supported in RE2 | Use multiple REGEXMATCH calls with AND/OR instead |
| `Backslash issues` | Incorrect escaping of special characters | Use single backslash in Sheets: \d, \w, \., etc. |
| `FILTER/ARRAYFORMULA issues` | Range mismatch or incorrect array handling | Ensure REGEXMATCH range matches other array ranges |

## Use Cases

### [[Data Validation and Input Checking]]
- **Implementation**: Use REGEXMATCH in data validation rules or helper columns to verify that entered data matches expected formats. Apply patterns for emails (`^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$`), phone numbers, postal codes, product codes, and other structured data.
- **Business Application**: Ensure data quality at entry point by immediately flagging incorrectly formatted data. Reduce errors in customer databases, inventory systems, and form responses. Enable automated data quality dashboards showing percentage of valid entries.
- **Technical Details**: Create validation columns with REGEXMATCH formulas, then use conditional formatting to highlight invalid entries. Combine with IF for descriptive error messages. For data validation dropdown restrictions, consider custom Apps Script.

### [[Conditional Row Filtering and Selection]]
- **Implementation**: Use REGEXMATCH within FILTER to dynamically select rows matching complex patterns. Combine with AND/OR for multi-criteria filtering. Apply to log analysis, transaction review, and dynamic report generation.
- **Business Application**: Create dynamic views of datasets showing only relevant entries without modifying source data. Enable ad-hoc analysis of error logs, support tickets, or transaction records based on pattern-based criteria.
- **Technical Details**: FILTER syntax: `=FILTER(data_range, REGEXMATCH(criteria_column, "pattern"))`. For multiple conditions: `=FILTER(A:C, REGEXMATCH(A:A, "pattern1"), REGEXMATCH(B:B, "pattern2"))`. Performance degrades on very large datasets.

### [[Pattern-Based Categorization]]
- **Implementation**: Use nested IF with REGEXMATCH to categorize text into groups based on patterns. Create classification logic that assigns categories based on detected patterns in product codes, descriptions, or identifiers.
- **Business Application**: Automate categorization of support tickets by keyword patterns, classify products by SKU pattern, segment customers by ID format, or route transactions by reference code patterns.
- **Technical Details**: `=IFS(REGEXMATCH(A1, "pattern1"), "Category1", REGEXMATCH(A1, "pattern2"), "Category2", TRUE, "Other")`. Order patterns from most specific to least. Use capturing with REGEXEXTRACT if you need the matched text.

### [[Conditional Formatting and Visual Indicators]]
- **Implementation**: Apply REGEXMATCH formulas in conditional formatting rules to visually highlight cells or rows matching patterns. Create heat maps based on pattern presence, error highlighting, or status indicators.
- **Business Application**: Instantly see rows containing errors, urgent items, overdue dates, or specific code patterns. Enable quick visual scanning of large datasets for items requiring attention.
- **Technical Details**: In conditional formatting, use formula rule like `=REGEXMATCH($A1, "(?i)error|warning")`. Use $ appropriately to lock columns when highlighting entire rows. Combine with other conditions using AND/OR.

## Platform Differences

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| **Function Availability** | Native function, fully supported | NOT AVAILABLE |
| **Alternative in Excel** | N/A | ISNUMBER(SEARCH()) for substrings, VBA for regex |
| **Regex Engine** | RE2 (fast, limited features) | N/A |
| **Case Sensitivity** | Case-sensitive by default; use (?i) | N/A |
| **Use in FILTER** | Works directly | N/A |
| **Use in Conditional Formatting** | Works directly | N/A |
| **Array Formula Support** | Full ARRAYFORMULA compatibility | N/A |
| **Data Validation** | Usable in custom formulas | N/A |

**Excel Workarounds:**

For basic substring detection in Excel:
```excel
=ISNUMBER(SEARCH("substring", A1))
```

For case-sensitive substring:
```excel
=ISNUMBER(FIND("substring", A1))
```

For pattern matching in Excel, options include:
1. **VBA UDF**: Create custom function using VBScript.RegExp
2. **Power Query**: Use Text.Contains, Text.StartsWith, or M language regex
3. **Complex formulas**: Nested SEARCH/FIND/MID combinations
4. **Helper columns**: Break down pattern matching into multiple steps

**Cross-Platform Alternative:**

When you must support both platforms, replace:
```
=REGEXMATCH(A1, "keyword")
```

With:
```
=ISNUMBER(SEARCH("keyword", A1))
```

Note: This only works for literal substring matching, not true pattern matching.

## Tips and Best Practices

1. **Use Anchors for Full-Cell Validation**: ^pattern$ ensures the entire cell matches, not just a substring. Without anchors, "abc@email.com" would match in "Contact abc@email.com for info" which may not be desired.

2. **Start Simple, Then Refine**: Begin with a basic pattern and test. Then add specificity. Debugging complex patterns is much harder than building up incrementally.

3. **Combine Multiple Checks for Complex Validation**: Since RE2 lacks lookahead, use AND/OR with multiple REGEXMATCH calls: `=AND(REGEXMATCH(A1, "[A-Z]"), REGEXMATCH(A1, "\d"), LEN(A1)>=8)`.

4. **Use (?i) for Case Insensitivity**: Instead of writing [Aa][Bb][Cc], prepend (?i) to the pattern: `(?i)abc` matches "abc", "ABC", "Abc", etc.

5. **Be Careful with Greedy Patterns**: Patterns like .* will match as much as possible. This rarely affects REGEXMATCH (which only returns TRUE/FALSE) but matters if you're designing patterns for REGEXEXTRACT.

6. **Test Edge Cases**: Empty cells, cells with only spaces, special characters, very long text—test these scenarios to ensure your pattern behaves correctly.

7. **Use in Conditional Formatting Wisely**: REGEXMATCH formulas in conditional formatting evaluate on every cell update. Complex patterns on large ranges can slow performance.

8. **Combine with FILTER for Dynamic Analysis**: `=FILTER(Data, REGEXMATCH(Column, "pattern"))` creates powerful dynamic views without modifying source data.

9. **Document Pattern Purpose**: Add a comment cell near complex REGEXMATCH formulas explaining what the pattern matches. Future you (or colleagues) will appreciate it.

10. **Know What You're Matching**: Understand whether you're testing for "contains this pattern" (no anchors) or "is exactly this pattern" (with ^$). This distinction is the source of many bugs.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from REGEXMATCH |
|----------|-------------|--------------------------------|
| [[REGEXEXTRACT]] | Extracts text matching a pattern | Returns matched text, not TRUE/FALSE |
| [[REGEXREPLACE]] | Replaces text matching a pattern | Modifies text rather than testing it |
| [[SEARCH]] | Finds position of substring (case-insensitive) | Returns position number, not Boolean; no regex support |
| [[FIND]] | Finds position of substring (case-sensitive) | Returns position number, not Boolean; no regex support |
| [[COUNTIF]] | Counts cells matching criteria | Supports wildcards (* ?) but not full regex |

### Commonly Used Together

**[[IF]]** - Conditional logic based on pattern match

*Create conditional outputs based on pattern detection:*
```
=IF(REGEXMATCH(A1, "(?i)approved"), "Ready", "Pending Review")
```
Returns different values based on whether pattern is found.

---

**[[FILTER]]** - Select rows matching patterns

*Filter data to show only matching rows:*
```
=FILTER(A:C, REGEXMATCH(A:A, "ERROR|WARN"))
```
Returns all columns for rows where column A contains "ERROR" or "WARN".

---

**[[IFS]]** - Multiple pattern categorization

*Categorize based on multiple patterns:*
```
=IFS(REGEXMATCH(A1, "^A"), "Group A", REGEXMATCH(A1, "^B"), "Group B", TRUE, "Other")
```
Assigns categories based on which pattern matches first.

---

**[[AND]]/[[OR]]** - Combine multiple pattern checks

*Check for multiple pattern conditions:*
```
=AND(REGEXMATCH(A1, "[A-Z]"), REGEXMATCH(A1, "\d"), NOT(REGEXMATCH(A1, "[!@#$%]")))
```
Validates text has uppercase and digit but no special characters.

---

**[[SUMPRODUCT]]** - Sum values for pattern-matching rows

*Aggregate based on pattern criteria:*
```
=SUMPRODUCT((REGEXMATCH(A2:A100, "^INV-"))*B2:B100)
```
Sums column B only for rows where column A matches the pattern.

---

**[[COUNTIF]] workaround** - Count pattern matches

*Count rows matching a pattern:*
```
=SUMPRODUCT(1*REGEXMATCH(A2:A100, "pattern"))
```
Counts how many cells match the pattern (COUNTIF doesn't support regex).

## Official Documentation

- **Google Sheets**: [REGEXMATCH function](https://support.google.com/docs/answer/3098292)
- **RE2 Syntax Reference**: [RE2 Regular Expression Syntax](https://github.com/google/re2/wiki/Syntax)

## Version History

| Platform | Version/Date | Changes |
|----------|--------------|---------|
| Google Sheets | 2006 | Original function release |
| Google Sheets | 2012 | RE2 engine improvements |
| Google Sheets | 2015 | Better Unicode pattern support |
| Google Sheets | 2018 | Improved ARRAYFORMULA integration |
| Google Sheets | 2020 | Performance optimizations |
| Google Sheets | 2022 | Stability improvements |
| Microsoft Excel | N/A | Function not available in any version |

---

*Last updated: 2026-01-10*
