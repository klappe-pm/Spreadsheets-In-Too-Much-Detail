---
categories:
- spreadsheet-functions
subCategories:
- sheets
topics:
- google-specific
- text-functions
- pattern-matching
subTopics:
- regular-expressions
- text-replacement
- data-cleaning
- RE2-syntax
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- regex-replace
- pattern-replace
- text-substitution
tags:
- google-sheets
- regex
- text-replacement
- data-transformation
- RE2
- text-processing
---

# REGEXREPLACE

## REGEXREPLACE Description

REGEXREPLACE is a Google Sheets function that replaces text matching a regular expression pattern with a specified replacement string. This powerful text manipulation function enables sophisticated find-and-replace operations that go far beyond simple text substitution, allowing you to identify complex patterns and transform them in a single operation. Common applications include cleaning imported data, standardizing formats, extracting or removing specific text patterns, and performing batch text transformations that would be impossible with standard SUBSTITUTE operations.

The function leverages Google's RE2 regular expression engine for pattern matching, providing consistent and predictable linear-time performance. RE2 supports standard metacharacters including character classes, quantifiers, anchors, groups, and alternation. The replacement string can reference captured groups from the pattern using `$1`, `$2`, etc., enabling powerful text rearrangement and extraction capabilities. This group referencing feature makes REGEXREPLACE particularly valuable for reformatting data, swapping text elements, or building new strings from captured components.

REGEXREPLACE performs case-sensitive matching by default, replacing all occurrences of the pattern within the text. Unlike SUBSTITUTE which requires you to specify an occurrence number, REGEXREPLACE will replace every match unless you craft your pattern to be more specific. For case-insensitive replacement, include the `(?i)` inline flag at the start of your pattern. When the pattern is not found, the original text is returned unchanged, making the function safe to use even when matches are uncertain.

This function is exclusive to Google Sheets and has no direct equivalent in Microsoft Excel. Excel users must rely on the SUBSTITUTE function for literal replacements, nested formulas for simple pattern-based transformations, or VBA custom functions for true regex replacement capabilities. The availability of REGEXREPLACE in Google Sheets provides substantial advantages for data cleaning, format standardization, and complex text transformation tasks.

> [!f(x)] REGEXREPLACE Syntax
>
> ```spreadsheets
> REGEXREPLACE(text, regular_expression, replacement)
> ```
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string in which to perform replacements. Can be a cell reference, text string in quotes, or formula returning text. | Yes |
> | `regular_expression` | The RE2-compatible regular expression pattern to search for in the text. Must be enclosed in quotes if provided directly. | Yes |
> | `replacement` | The replacement text to substitute for matched patterns. Can include group references ($1, $2, etc.) to insert captured text. Use empty string ("") to delete matches. | Yes |

## REGEXREPLACE Examples

| Scenario | Formula | Result | Explanation |
|----------|---------|--------|-------------|
| Simple replacement | `=REGEXREPLACE("hello world", "world", "universe")` | hello universe | Replaces literal text "world" with "universe" |
| Remove all digits | `=REGEXREPLACE("Order-12345-ABC", "\d", "")` | Order--ABC | Removes all digit characters from text |
| Remove multiple spaces | `=REGEXREPLACE("hello   world", "\s+", " ")` | hello world | Collapses multiple spaces to single space |
| Remove non-alphanumeric | `=REGEXREPLACE("Hello! World?", "[^a-zA-Z0-9\s]", "")` | Hello World | Strips punctuation while keeping letters, numbers, spaces |
| Case-insensitive replace | `=REGEXREPLACE("Hello HELLO hello", "(?i)hello", "hi")` | hi hi hi | Replaces all case variations with lowercase "hi" |
| Capture group reference | `=REGEXREPLACE("John Smith", "(\w+) (\w+)", "$2, $1")` | Smith, John | Swaps first and last name using groups |
| Format phone number | `=REGEXREPLACE("5551234567", "(\d{3})(\d{3})(\d{4})", "($1) $2-$3")` | (555) 123-4567 | Reformats 10 digits into standard phone format |
| Extract domain from email | `=REGEXREPLACE("user@example.com", ".*@", "")` | example.com | Removes everything up to and including @ |
| Add prefix to pattern | `=REGEXREPLACE("cat", "^", "my ")` | my cat | Uses start anchor to add prefix |
| Add suffix to pattern | `=REGEXREPLACE("cat", "$", "s")` | cats | Uses end anchor to add suffix |
| Replace first word | `=REGEXREPLACE("one two three", "^\w+", "1")` | 1 two three | Replaces only the first word |
| Mask credit card | `=REGEXREPLACE("4111222233334444", "(\d{4})(\d{8})(\d{4})", "$1********$3")` | 4111********4444 | Masks middle digits of credit card |
| Clean HTML tags | `=REGEXREPLACE("<p>Hello</p>", "<[^>]+>", "")` | Hello | Strips HTML tags from text |
| Standardize date format | `=REGEXREPLACE("12-25-2024", "(\d{2})-(\d{2})-(\d{4})", "$3-$1-$2")` | 2024-12-25 | Converts MM-DD-YYYY to YYYY-MM-DD |
| Remove file extension | `=REGEXREPLACE("document.pdf", "\.[^.]+$", "")` | document | Removes the last file extension |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid regular expression syntax | Check regex pattern for unescaped special characters or syntax errors; validate pattern in regex tester |
| `#VALUE!` | Non-text input in text parameter | Convert numeric or date values to text using TEXT() or TO_TEXT() before processing |
| `#VALUE!` | Invalid group reference in replacement | Ensure $1, $2, etc. reference existing capture groups; check parentheses count in pattern |
| `#VALUE!` | Unbalanced parentheses in pattern | Count opening and closing parentheses to ensure they match correctly |
| `#VALUE!` | Invalid escape sequence | Use proper escaping: `\\` for literal backslash, `\$` if needed for literal dollar sign |
| `#VALUE!` | Unsupported RE2 feature used | RE2 doesn't support backreferences in pattern or lookahead/lookbehind; rewrite pattern |
| `#REF!` | Referenced cell doesn't exist | Verify all cell references are valid and within sheet boundaries |
| `#ERROR!` | Circular reference detected | Ensure the formula doesn't reference its own cell directly or indirectly |
| Unexpected results | Case sensitivity | Add (?i) at pattern start for case-insensitive matching |
| All text replaced | Pattern too broad | Make pattern more specific using anchors, character limits, or additional constraints |

## Use Cases

### Data Cleaning and Standardization

**Implementation**: Use REGEXREPLACE to clean imported data by removing unwanted characters, standardizing formats, and normalizing text structure.

```spreadsheets
// Remove leading/trailing whitespace and collapse internal spaces
=TRIM(REGEXREPLACE(A2, "\s+", " "))

// Clean phone numbers to digits only
=REGEXREPLACE(A2, "[^\d]", "")

// Standardize US phone format
=REGEXREPLACE(REGEXREPLACE(A2, "[^\d]", ""), "^1?(\d{3})(\d{3})(\d{4})$", "($1) $2-$3")

// Remove special characters except alphanumeric and basic punctuation
=REGEXREPLACE(A2, "[^\w\s.,!?'-]", "")

// Normalize email addresses to lowercase (when combined with LOWER)
=LOWER(REGEXREPLACE(A2, "\s", ""))
```

- **Business Application**: Prepare imported customer data, survey responses, or external datasets for analysis by ensuring consistent formatting, removing noise characters, and standardizing input variations that could cause duplicate or matching issues.
- **Technical Details**: Chain multiple REGEXREPLACE operations for complex cleaning pipelines. Create a data cleaning template sheet with progressive transformation columns showing before/after states for quality assurance.

### Format Conversion and Transformation

**Implementation**: Transform data between different format standards such as date formats, currency representations, or identifier structures.

```spreadsheets
// Convert MM/DD/YYYY to YYYY-MM-DD (ISO format)
=REGEXREPLACE(A2, "^(\d{1,2})/(\d{1,2})/(\d{4})$", "$3-$1-$2")

// Convert name format: "John Smith" to "Smith, John"
=REGEXREPLACE(A2, "^(\S+)\s+(.+)$", "$2, $1")

// Convert camelCase to snake_case
=LOWER(REGEXREPLACE(A2, "([a-z])([A-Z])", "$1_$2"))

// Add thousand separators (simplified approach)
=REGEXREPLACE(TEXT(A2, "0"), "(\d)(?=(\d{3})+$)", "$1,")

// Convert product code format: "ABC123" to "ABC-123"
=REGEXREPLACE(A2, "([A-Z]+)(\d+)", "$1-$2")
```

- **Business Application**: Ensure data compatibility when integrating systems with different format requirements, generating reports for different regional standards, or preparing data for import into external applications with specific format expectations.
- **Technical Details**: Test format conversions with edge cases including boundary values, varying input lengths, and missing components. Document expected input formats and provide validation before transformation.

### Content Redaction and Privacy

**Implementation**: Mask or redact sensitive information such as personal identifiers, financial data, or confidential content while preserving document structure.

```spreadsheets
// Mask SSN showing only last 4 digits
=REGEXREPLACE(A2, "^\d{3}-?\d{2}-?(\d{4})$", "XXX-XX-$1")

// Redact credit card numbers
=REGEXREPLACE(A2, "\b(\d{4})\d{8}(\d{4})\b", "$1********$2")

// Mask email addresses
=REGEXREPLACE(A2, "([a-zA-Z0-9])[a-zA-Z0-9.]*(@[a-zA-Z0-9.-]+)", "$1***$2")

// Redact phone numbers
=REGEXREPLACE(A2, "\b(\d{3})[-.\s]?\d{3}[-.\s]?(\d{4})\b", "$1-XXX-$2")

// Remove IP addresses from logs
=REGEXREPLACE(A2, "\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b", "[IP REDACTED]")
```

- **Business Application**: Prepare data for sharing with third parties, create sanitized versions of documents for compliance audits, or generate sample datasets that protect personally identifiable information while maintaining realistic data structure.
- **Technical Details**: Implement consistent redaction patterns across entire datasets. Verify redaction completeness by using REGEXMATCH to check for remaining sensitive patterns. Consider creating separate redaction templates for different data types.

### Text Extraction and URL Processing

**Implementation**: Extract, modify, or standardize URLs, file paths, and embedded structured data within text content.

```spreadsheets
// Extract domain from URL
=REGEXREPLACE(A2, "^https?://([^/]+).*$", "$1")

// Convert HTTP to HTTPS
=REGEXREPLACE(A2, "^http://", "https://")

// Remove URL parameters
=REGEXREPLACE(A2, "\?.*$", "")

// Extract file name from path
=REGEXREPLACE(A2, "^.*/([^/]+)$", "$1")

// Remove tracking parameters from URLs
=REGEXREPLACE(A2, "[?&](utm_[^&]*|ref=[^&]*)", "")

// Normalize URL slashes
=REGEXREPLACE(A2, "/+", "/")
```

- **Business Application**: Clean affiliate links, standardize URLs for deduplication, extract domain names for categorization, or prepare URLs for analytics processing by removing irrelevant parameters.
- **Technical Details**: Handle edge cases such as URLs without protocols, relative paths, and encoded characters. Consider URL decoding requirements before processing and validate transformed URLs maintain proper structure.

## Platform Differences

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| Native REGEXREPLACE function | Yes - full support | No - not available |
| Alternative approach | N/A | SUBSTITUTE for literal, VBA for regex |
| Regex engine | RE2 | N/A (VBA uses VBScript regex) |
| Capture group references | $1, $2, $3, etc. | N/A |
| Case-insensitive flag | (?i) inline flag | N/A |
| Replace all occurrences | Automatic | SUBSTITUTE requires occurrence number |
| Array formula support | Yes with ARRAYFORMULA | N/A |

**Excel Workarounds**: Excel users must use SUBSTITUTE for simple literal replacements (one pattern at a time) or combine multiple nested SUBSTITUTE calls. For true regex replacement, Excel requires VBA custom functions using the VBScript.RegExp object or third-party add-ins. The SUBSTITUTE function also requires specifying which occurrence to replace unless nested multiple times, making global replacements tedious.

## Tips and Best Practices

1. **Test patterns with REGEXMATCH first**: Before using REGEXREPLACE, verify your pattern matches correctly using REGEXMATCH. This helps identify pattern issues before adding replacement complexity.

2. **Use capture groups for rearrangement**: Enclose pattern parts in parentheses () to create groups, then reference them in the replacement with $1, $2, etc. This enables powerful text restructuring without losing content.

3. **Delete matches with empty replacement**: Use an empty string ("") as the replacement to remove matched patterns entirely: `=REGEXREPLACE(A1, "pattern_to_remove", "")`

4. **Handle case sensitivity explicitly**: Add (?i) at the start of your pattern for case-insensitive matching: `=REGEXREPLACE(A1, "(?i)pattern", "replacement")`

5. **Escape special regex characters**: When matching literal special characters (. * + ? ^ $ [ ] ( ) { } | \), escape them with backslash. To match a period, use `\.`

6. **Chain replacements for complex transformations**: Use nested REGEXREPLACE calls for multi-step transformations: `=REGEXREPLACE(REGEXREPLACE(A1, "pattern1", "rep1"), "pattern2", "rep2")`

7. **Use anchors for precise replacements**: Apply ^ (start) and $ (end) anchors to ensure replacements occur only at specific positions, preventing unintended matches mid-string.

8. **Validate results with conditional formatting**: Create conditional formatting rules using REGEXMATCH to highlight cells where transformation may have failed or produced unexpected results, enabling quick quality review.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[REGEXMATCH]] | Test if pattern exists in text | Returns TRUE/FALSE instead of modified text |
| [[REGEXEXTRACT]] | Extract text matching pattern | Returns matched text only, doesn't replace |
| [[SUBSTITUTE]] | Replace literal text occurrences | No pattern matching, exact text only |
| [[REPLACE]] | Replace text by position | Uses character positions, not patterns |
| [[TRIM]] | Remove extra whitespace | Specific whitespace handling only |
| [[CLEAN]] | Remove non-printable characters | Limited to specific character codes |

### Commonly Used Together

**[[REGEXMATCH]]** - Validate before replacement

```spreadsheets
=IF(REGEXMATCH(A1, "^\d{10}$"), REGEXREPLACE(A1, "(\d{3})(\d{3})(\d{4})", "($1) $2-$3"), A1)
```

Formats phone number only if it matches expected 10-digit pattern.

**[[ARRAYFORMULA]]** - Apply replacement across ranges

```spreadsheets
=ARRAYFORMULA(REGEXREPLACE(A2:A100, "\s+", " "))
```

Normalizes whitespace across entire column in single formula.

**[[TRIM]]** - Clean up after replacement

```spreadsheets
=TRIM(REGEXREPLACE(A1, "[^\w\s]", " "))
```

Removes punctuation and cleans resulting extra spaces.

**[[LOWER]]** / **[[UPPER]]** - Standardize case

```spreadsheets
=UPPER(REGEXREPLACE(A1, "\s+", "_"))
```

Creates uppercase snake_case from spaced text.

**[[IFERROR]]** - Handle replacement errors gracefully

```spreadsheets
=IFERROR(REGEXREPLACE(A1, B1, C1), A1)
```

Returns original text if pattern or replacement is invalid.

**[[SUBSTITUTE]]** - Combine with literal replacements

```spreadsheets
=SUBSTITUTE(REGEXREPLACE(A1, "(\d{3})(\d{4})", "$1-$2"), ".", "")
```

Chain pattern-based and literal replacements for complex transformations.

## Official Documentation

- [Google Sheets REGEXREPLACE Function](https://support.google.com/docs/answer/3098245)
- [Google RE2 Syntax Reference](https://github.com/google/re2/wiki/Syntax)

## Version History

| Version/Date | Changes |
|--------------|---------|
| 2006 | Initial release with Google Sheets launch |
| 2010 | Improved RE2 engine integration and capture group handling |
| 2013 | Enhanced Unicode support for international text processing |
| 2016 | Better error messages for invalid patterns and group references |
| 2019 | Performance optimizations for large text processing |
| 2022 | Improved ARRAYFORMULA compatibility and memory efficiency |
| 2024 | Enhanced named capture group support and pattern debugging |
