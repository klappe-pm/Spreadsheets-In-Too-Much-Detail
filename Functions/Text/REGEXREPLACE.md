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
  - text-replacement
  - data-cleaning
  - regex
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - regex-replace
  - pattern-replace
  - regex-substitute
tags:
  - text
  - regex
  - pattern-matching
  - replacement
  - cleaning
  - google-sheets
  - sheets-only
---

# REGEXREPLACE

## Description

REGEXREPLACE is a powerful Google Sheets-exclusive function that replaces all occurrences of text matching a regular expression pattern with a specified replacement string. Unlike the SUBSTITUTE function which only replaces exact literal text matches, REGEXREPLACE uses pattern matching to find and replace text based on character patterns, enabling sophisticated text transformation operations that would be extremely difficult or impossible with traditional text functions. The function processes the entire input text and replaces every instance where the pattern matches, making it ideal for bulk data cleaning, format standardization, and text transformation tasks.

The function excels in scenarios requiring flexible, pattern-based text manipulation rather than simple find-and-replace operations. Common use cases include removing unwanted characters from data (special characters, extra whitespace, HTML tags), standardizing formats (phone numbers, dates, identifiers), extracting and reformatting portions of text (using capture groups in replacement), cleaning imported data by stripping unwanted elements, redacting sensitive information, converting between naming conventions (camelCase to snake_case), and normalizing user-entered data. REGEXREPLACE is particularly valuable when dealing with data from multiple sources where the same information may be formatted differently and needs standardization.

Several critical aspects of REGEXREPLACE require careful attention. First, the function replaces ALL matches in the text, not just the first one—there's no option to limit replacements. Second, capture groups in the pattern can be referenced in the replacement string using $1, $2, etc., enabling powerful text rearrangement. Third, the function uses RE2 regex syntax which lacks lookahead, lookbehind, and backreferences in the pattern (but supports $n group references in replacement). Fourth, if the pattern doesn't match anything, the original text is returned unchanged—no error is thrown. Fifth, be careful with greedy quantifiers (.*) as they may match more text than intended. Sixth, special characters in the replacement string (like $ and \) may need escaping. Seventh, the function is case-sensitive by default; use (?i) at the start of your pattern for case-insensitive replacement.

REGEXREPLACE is available exclusively in Google Sheets and has no direct equivalent in Microsoft Excel's standard function library. Excel's SUBSTITUTE function only handles literal text replacement, and achieving regex-based replacement requires VBA custom functions, Power Query M language, or add-ins. For Excel 365 users, some cleanup tasks can be handled with combinations of TEXTBEFORE, TEXTAFTER, and SUBSTITUTE, but true pattern-based replacement remains unavailable without programming. When building cross-platform spreadsheets, consider whether the transformation can be accomplished with SUBSTITUTE for literal replacements, or document that certain data cleaning features are Sheets-only.

## Syntax

> [!f(x)] REGEXREPLACE Syntax
>
> ```
> =REGEXREPLACE(text, regular_expression, replacement)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The input string in which to perform replacements. Can be a cell reference, literal text in quotes, or a formula returning text. Non-text values are converted to text before processing. | Yes |
> | `regular_expression` | The regular expression pattern to match. Must be a valid RE2 regular expression enclosed in quotes. All portions of text matching this pattern will be replaced. May contain capturing groups for use in replacement. | Yes |
> | `replacement` | The text to substitute for each match. Can include $1, $2, etc. to reference captured groups from the pattern. Use $0 to reference the entire match. Literal $ should be escaped as $$. | Yes |

## Replacement String Special Characters

| Syntax | Meaning | Example |
|--------|---------|---------|
| `$0` | Entire matched text | Pattern: `\d+`, Replacement: `($0)` transforms "123" to "(123)" |
| `$1` | First capturing group | Pattern: `(\w+)@(\w+)`, Replacement: `$2-$1` transforms "user@domain" to "domain-user" |
| `$2, $3...` | Subsequent capturing groups | Reference matched groups in any order in replacement |
| `$$` | Literal dollar sign | Replacement: `$$100` produces "$100" |
| `\n` | Newline character | Replacement: `$1\n$2` puts groups on separate lines |
| `\t` | Tab character | Replacement: `$1\t$2` separates groups with tab |

## Examples

### Example 1: Remove All Non-Numeric Characters
```
=REGEXREPLACE(A1, "[^0-9]", "")
```
Where A1 contains "(555) 123-4567"

**Result:** `5551234567`

**Explanation:** The pattern [^0-9] matches any character that is NOT a digit. Replacing with empty string "" effectively removes all non-digit characters. This is essential for cleaning phone numbers, IDs, or any numeric data with formatting.

---

### Example 2: Remove Extra Whitespace
```
=REGEXREPLACE(A1, "\s+", " ")
```
Where A1 contains "Hello    World   How   Are   You"

**Result:** `Hello World How Are You`

**Explanation:** The pattern \s+ matches one or more whitespace characters (spaces, tabs, newlines). Replacing with a single space normalizes all whitespace runs to single spaces. Use TRIM() afterward to remove leading/trailing spaces.

---

### Example 3: Remove HTML Tags
```
=REGEXREPLACE(A1, "<[^>]+>", "")
```
Where A1 contains "<p>Hello <strong>World</strong></p>"

**Result:** `Hello World`

**Explanation:** This pattern matches HTML tags: < followed by one or more characters that aren't >, then >. Replacing with empty string strips all tags, leaving only the text content. Useful for cleaning web-scraped data.

---

### Example 4: Format Phone Number with Dashes
```
=REGEXREPLACE(REGEXREPLACE(A1, "[^0-9]", ""), "(\d{3})(\d{3})(\d{4})", "$1-$2-$3")
```
Where A1 contains "(555) 123 4567"

**Result:** `555-123-4567`

**Explanation:** This uses nested REGEXREPLACE: first removes all non-digits, then reformats using capture groups. The pattern (\d{3})(\d{3})(\d{4}) captures the three number groups, and $1-$2-$3 reassembles them with hyphens.

---

### Example 5: Mask Credit Card Numbers
```
=REGEXREPLACE(A1, "(\d{4})\d{8}(\d{4})", "$1********$2")
```
Where A1 contains "4111111111111111"

**Result:** `4111********1111`

**Explanation:** The pattern captures the first and last four digits in groups, matching the middle eight. The replacement shows first/last groups with asterisks hiding the middle portion. Essential for PII protection.

---

### Example 6: Convert CamelCase to snake_case
```
=LOWER(REGEXREPLACE(A1, "([a-z])([A-Z])", "$1_$2"))
```
Where A1 contains "getUserAccountBalance"

**Result:** `get_user_account_balance`

**Explanation:** The pattern finds lowercase followed by uppercase letters and inserts underscore between them. The outer LOWER() converts everything to lowercase. Useful for code or data format conversion.

---

### Example 7: Remove Leading Zeros
```
=REGEXREPLACE(A1, "^0+", "")
```
Where A1 contains "00042"

**Result:** `42`

**Explanation:** The pattern ^0+ matches one or more zeros at the start of the string. Replacing with empty string removes leading zeros. Note: This leaves "000" as empty string; use REGEXREPLACE(A1, "^0+(?=\d)", "") to keep at least one digit—but (?=) isn't supported, so use IF for that case.

---

### Example 8: Standardize Date Separators
```
=REGEXREPLACE(A1, "[/.-]", "-")
```
Where A1 contains "12/25.2024"

**Result:** `12-25-2024`

**Explanation:** The character class [/.-] matches forward slash, period, or hyphen (period doesn't need escaping inside brackets at certain positions). All date separators become hyphens for standardization.

---

### Example 9: Remove All Punctuation
```
=REGEXREPLACE(A1, "[[:punct:]]", "")
```
Where A1 contains "Hello, World! How's it going?"

**Result:** `Hello World Hows it going`

**Explanation:** The POSIX class [[:punct:]] matches all punctuation characters. This is cleaner than listing every punctuation mark individually. Note: RE2 supports POSIX classes.

---

### Example 10: Case-Insensitive Replacement
```
=REGEXREPLACE(A1, "(?i)error", "WARNING")
```
Where A1 contains "Error: An ERROR occurred. error detected."

**Result:** `WARNING: An WARNING occurred. WARNING detected.`

**Explanation:** The (?i) flag makes the pattern case-insensitive. All variations of "error" (Error, ERROR, error) are replaced with "WARNING". Useful for standardizing terminology.

---

### Example 11: Extract and Reformat Email Domain
```
=REGEXREPLACE(A1, ".*@(.+)\.(.+)", "Domain: $1 (TLD: $2)")
```
Where A1 contains "user@example.com"

**Result:** `Domain: example (TLD: com)`

**Explanation:** The pattern captures the domain name and TLD separately. The replacement reorganizes this information into a descriptive format. Demonstrates creative use of capture groups.

---

### Example 12: Add Prefix to All Numbers
```
=REGEXREPLACE(A1, "(\d+)", "Item-$1")
```
Where A1 contains "Order contains 5 widgets and 3 gadgets"

**Result:** `Order contains Item-5 widgets and Item-3 gadgets`

**Explanation:** The pattern captures each number, and replacement prepends "Item-" to each. This transforms all numeric values found in the text.

---

### Example 13: Remove URLs from Text
```
=REGEXREPLACE(A1, "https?://[^\s]+", "[URL REMOVED]")
```
Where A1 contains "Visit https://example.com for more info"

**Result:** `Visit [URL REMOVED] for more info`

**Explanation:** The pattern matches http:// or https:// followed by non-whitespace characters. URLs are replaced with a placeholder. Useful for content moderation or simplification.

---

### Example 14: Normalize Product Codes
```
=UPPER(REGEXREPLACE(A1, "[^A-Za-z0-9]", ""))
```
Where A1 contains "abc-123_def"

**Result:** `ABC123DEF`

**Explanation:** First removes all non-alphanumeric characters, then UPPER() normalizes to uppercase. Creates clean, standardized product codes from varied input formats.

---

### Example 15: Replace Multiple Spaces After Periods
```
=REGEXREPLACE(A1, "\.\s+", ". ")
```
Where A1 contains "Sentence one.   Sentence two.     Sentence three."

**Result:** `Sentence one. Sentence two. Sentence three.`

**Explanation:** Matches period followed by multiple spaces and replaces with period and single space. Normalizes sentence spacing in text documents.

---

### Example 16: Swap First and Last Names
```
=REGEXREPLACE(A1, "^(\S+)\s+(\S+)$", "$2, $1")
```
Where A1 contains "John Smith"

**Result:** `Smith, John`

**Explanation:** Captures first non-space sequence and second non-space sequence, then swaps them with comma. Converts "First Last" to "Last, First" format.

---

### Example 17: Escape Special Characters for HTML
```
=REGEXREPLACE(REGEXREPLACE(REGEXREPLACE(A1, "&", "&amp;"), "<", "&lt;"), ">", "&gt;")
```
Where A1 contains "Price: $5 < $10 & valid"

**Result:** `Price: $5 &lt; $10 &amp; valid`

**Explanation:** Nested REGEXREPLACE calls escape HTML special characters. Order matters: ampersand first, then others. Prepares text for safe HTML display.

---

### Example 18: Remove Duplicate Words
```
=REGEXREPLACE(A1, "(?i)\b(\w+)\s+\1\b", "$1")
```
Where A1 contains "The the quick brown fox fox"

**Result:** `The quick brown fox`

**Explanation:** Note: This uses backreference \1 which is NOT fully supported in RE2. For Google Sheets, you'd need a different approach or accept this limitation.

**Working Alternative (Limited):**
```
=TRIM(REGEXREPLACE(A1, "\s+", " "))
```
Combined with custom script for true duplicate detection.

---

### Example 19: Clean Currency Values for Calculation
```
=VALUE(REGEXREPLACE(A1, "[^0-9.]", ""))
```
Where A1 contains "$1,234.56"

**Result:** `1234.56` (as number)

**Explanation:** Removes everything except digits and decimal point, then VALUE() converts to number. Essential for processing currency data from text sources.

---

### Example 20: Add Thousand Separators to Numbers
```
=REGEXREPLACE(TEXT(VALUE(A1), "#,##0"), "(\d)(?=(\d{3})+(?!\d))", "$1,")
```

**Note:** This specific approach may not work due to lookahead limitations. Better approach:

```
=TEXT(VALUE(REGEXREPLACE(A1, "[^0-9]", "")), "#,##0")
```
Where A1 contains "1234567"

**Result:** `1,234,567`

**Explanation:** Clean the number first, convert to numeric value, then use TEXT for formatting. This hybrid approach avoids regex limitations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid regular expression syntax | Check for unbalanced brackets, parentheses, or invalid escape sequences |
| `#REF!` | Reference to non-existent capture group ($3 when only 2 groups exist) | Verify capture group numbers match pattern |
| `#NAME?` | Function name misspelled or using in Excel | Check spelling; REGEXREPLACE is Sheets-only |
| `No replacement occurs` | Pattern doesn't match any text in input | Test pattern separately with REGEXMATCH to verify it matches |
| `Too much replaced` | Greedy pattern matching more than intended | Use more specific patterns; consider non-greedy alternatives |
| `Replacement overwrites too much` | $ in replacement interpreted as group reference | Escape literal $ as $$ in replacement string |
| `Case issues` | Pattern is case-sensitive by default | Add (?i) at start of pattern for case-insensitive matching |
| `Backslash problems` | Incorrect escaping in pattern or replacement | Single backslash works in Sheets; check escape sequences |
| `Infinite loops (slow)` | Extremely complex pattern on large text | Simplify pattern; break into multiple operations |
| `Group reference not working` | Using \1 syntax instead of $1 in replacement | Use $1, $2, etc. for capture groups in replacement |

## Use Cases

### [[Data Cleaning and Normalization]]
- **Implementation**: Use REGEXREPLACE to strip unwanted characters, normalize whitespace, remove HTML/markup, and standardize formatting across datasets. Apply patterns like `[^A-Za-z0-9\s]` to remove special characters or `\s+` to normalize whitespace.
- **Business Application**: Clean imported data from various sources to enable proper analysis. Prepare data for database import by removing formatting that causes errors. Standardize customer-entered data for matching and deduplication.
- **Technical Details**: Chain multiple REGEXREPLACE calls for complex cleaning: `=TRIM(REGEXREPLACE(REGEXREPLACE(A1, "<[^>]+>", ""), "\s+", " "))`. Consider performance on large datasets; helper columns may be more efficient than nested formulas.

### [[Format Standardization]]
- **Implementation**: Reformat phone numbers, dates, postal codes, and identifiers to consistent formats using capture groups. Pattern `(\d{3})(\d{3})(\d{4})` with replacement `($1) $2-$3` creates standard phone format.
- **Business Application**: Ensure all records in databases follow the same format for professional appearance and consistent processing. Enable matching across systems with different format conventions.
- **Technical Details**: First strip all formatting with `[^0-9]` replacement, then apply desired format with capture groups. Handle edge cases (varying length inputs) with conditional logic or multiple pattern variants.

### [[Sensitive Data Redaction]]
- **Implementation**: Mask or remove PII like credit card numbers, SSNs, phone numbers, and email addresses. Use patterns to identify sensitive data and replace with masked versions or placeholder text.
- **Business Application**: Comply with data privacy regulations (GDPR, CCPA, PCI-DSS) by automatically redacting sensitive information before sharing or analysis. Create sanitized datasets for development or testing.
- **Technical Details**: Pattern `(\d{3})\d{2}(\d{4})` with `$1-XX-$2` masks SSN middle digits. Email: `([^@]+)@(.+)` with `***@$2` hides local part. Test thoroughly to ensure no sensitive data escapes patterns.

### [[Text Transformation and Reformatting]]
- **Implementation**: Convert between formats like camelCase/snake_case, rearrange name order, restructure identifiers, and transform text layouts. Use capture groups to extract components and replacement to reassemble.
- **Business Application**: Convert data formats for system integration. Transform names between "First Last" and "Last, First" for different system requirements. Reformat product codes, SKUs, or identifiers for inventory management.
- **Technical Details**: Name swap: `^(\S+)\s+(\S+)$` with `$2, $1`. CamelCase: `([a-z])([A-Z])` with `$1_$2` plus LOWER(). Consider edge cases and test with varied input formats.

## Platform Differences

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| **Function Availability** | Native function, fully supported | NOT AVAILABLE |
| **Alternative in Excel** | N/A | SUBSTITUTE for literal text only; VBA for regex |
| **Regex Engine** | RE2 (Google's library) | N/A |
| **Replacement Groups** | $1, $2, $3... and $0 | N/A |
| **Case Sensitivity** | Case-sensitive by default; use (?i) | N/A |
| **Replace All Matches** | Always replaces all (no "replace first" option) | N/A |
| **Lookahead/Lookbehind** | NOT supported | N/A |
| **Backreferences in Pattern** | NOT supported | N/A |
| **Array Formula Support** | Full ARRAYFORMULA compatibility | N/A |

**Excel Alternatives:**

For basic replacement in Excel:
```excel
=SUBSTITUTE(A1, "old", "new")
```

For multiple replacements:
```excel
=SUBSTITUTE(SUBSTITUTE(A1, "old1", "new1"), "old2", "new2")
```

For regex in Excel, options include:
1. **VBA Custom Function**: Create UDF using VBScript.RegExp with Replace method
2. **Power Query**: Use Text.Replace or M language for pattern-based replacement
3. **Manual formulas**: Complex combinations of SUBSTITUTE, MID, FIND, LEN
4. **Add-ins**: Third-party regex add-ins for Excel

**Cross-Platform Strategy:**

When building spreadsheets for both platforms:
- Use SUBSTITUTE where literal replacement suffices
- Document Sheets-only features clearly
- Consider pre-processing data with Sheets before sharing
- Create VBA equivalents for critical transformations in Excel

## Tips and Best Practices

1. **Test Patterns with REGEXMATCH First**: Before replacing, verify your pattern matches what you expect using REGEXMATCH. This prevents accidentally replacing nothing or replacing the wrong text.

2. **Use Capture Groups for Complex Transformations**: Instead of removing and rebuilding, capture the parts you want to keep and reassemble them: `(\d{4})-(\d{2})-(\d{2})` with `$2/$3/$1` converts date formats.

3. **Chain Multiple Operations When Needed**: Complex transformations often work better as multiple simpler REGEXREPLACE calls: first clean, then format, then validate.

4. **Remember $$ for Literal Dollar Signs**: If your replacement text needs a literal $, escape it as $$. Otherwise, $1, $2, etc. are interpreted as group references.

5. **Handle Empty Matches Carefully**: Patterns that can match empty strings (like .*) may behave unexpectedly. Be specific about what must be matched.

6. **Use (?i) for Case Insensitivity**: At pattern start, (?i) makes the entire pattern case-insensitive. `(?i)error` matches "Error", "ERROR", "error".

7. **Consider Performance on Large Datasets**: Complex regex on many cells can slow calculations. Extract to helper columns with simpler formulas, or process in batches.

8. **Document Your Patterns**: Complex regex patterns are hard to read later. Add comments in adjacent cells explaining what the pattern does and why.

9. **Validate Results**: After applying REGEXREPLACE, spot-check results and look for edge cases that weren't handled correctly. Test with empty cells, special characters, and unexpected formats.

10. **Combine with TRIM for Complete Cleaning**: After regex operations that affect whitespace, wrap in TRIM to ensure no leading/trailing spaces remain.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from REGEXREPLACE |
|----------|-------------|----------------------------------|
| [[SUBSTITUTE]] | Replaces literal text occurrences | No pattern matching; only exact text replacement |
| [[REGEXEXTRACT]] | Extracts text matching a pattern | Returns matched text rather than modifying original |
| [[REGEXMATCH]] | Tests if text matches a pattern | Returns TRUE/FALSE, doesn't modify text |
| [[REPLACE]] | Replaces text by position | Position-based, not pattern-based |
| [[TRIM]] | Removes leading/trailing whitespace | Limited scope; only affects ends of string |

### Commonly Used Together

**[[TRIM]]** - Clean up whitespace after regex operations

*Remove edge whitespace after replacement:*
```
=TRIM(REGEXREPLACE(A1, "\s+", " "))
```
Normalizes internal whitespace and removes leading/trailing spaces.

---

**[[UPPER]]/[[LOWER]]** - Standardize case with replacement

*Combine regex cleaning with case normalization:*
```
=UPPER(REGEXREPLACE(A1, "[^A-Za-z0-9]", ""))
```
Creates clean, uppercase-standardized values.

---

**[[VALUE]]** - Convert cleaned numbers to numeric type

*Enable calculations on cleaned data:*
```
=VALUE(REGEXREPLACE(A1, "[^0-9.]", ""))
```
Strips non-numeric characters and converts to number.

---

**[[IFERROR]]** - Handle potential regex errors

*Provide fallback for invalid inputs:*
```
=IFERROR(REGEXREPLACE(A1, pattern, replacement), A1)
```
Returns original value if regex operation fails.

---

**[[ARRAYFORMULA]]** - Apply replacement to entire columns

*Process multiple cells with single formula:*
```
=ARRAYFORMULA(REGEXREPLACE(A2:A100, "\s+", " "))
```
Efficiently applies transformation across range.

---

**[[IF]] with [[REGEXMATCH]]** - Conditional replacement

*Apply replacement only when pattern exists:*
```
=IF(REGEXMATCH(A1, "pattern"), REGEXREPLACE(A1, "pattern", "replacement"), A1)
```
Enables conditional transformation logic.

## Official Documentation

- **Google Sheets**: [REGEXREPLACE function](https://support.google.com/docs/answer/3098245)
- **RE2 Syntax Reference**: [RE2 Regular Expression Syntax](https://github.com/google/re2/wiki/Syntax)

## Version History

| Platform | Version/Date | Changes |
|----------|--------------|---------|
| Google Sheets | 2006 | Original function release |
| Google Sheets | 2012 | RE2 engine improvements |
| Google Sheets | 2015 | Enhanced capture group support |
| Google Sheets | 2018 | Better ARRAYFORMULA integration |
| Google Sheets | 2020 | Performance optimizations for large texts |
| Google Sheets | 2022 | Stability and Unicode improvements |
| Microsoft Excel | N/A | Function not available in any version |

---

*Last updated: 2026-01-10*
