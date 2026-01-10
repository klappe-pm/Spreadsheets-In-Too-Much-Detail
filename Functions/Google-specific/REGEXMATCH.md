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
- data-validation
- text-analysis
- RE2-syntax
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- regex-match
- pattern-match
- text-pattern-test
tags:
- google-sheets
- regex
- text-matching
- validation
- boolean-function
- RE2
---

# REGEXMATCH

## REGEXMATCH Description

REGEXMATCH is a Google Sheets function that tests whether a text string matches a regular expression pattern and returns a Boolean result (TRUE or FALSE). This function is invaluable for data validation, pattern detection, conditional logic, and filtering operations where you need to identify text that conforms to specific patterns. Unlike simple text functions like FIND or SEARCH, REGEXMATCH harnesses the full power of regular expressions, enabling sophisticated pattern matching that can identify complex text structures such as email addresses, phone numbers, postal codes, or custom data formats.

The function operates using Google's RE2 regular expression engine, which provides a powerful yet predictable pattern matching system. RE2 supports standard regex metacharacters including character classes, quantifiers, anchors, groups, and alternation. Unlike some regex engines, RE2 guarantees linear-time execution and doesn't support backreferences or lookahead/lookbehind assertions, trading some advanced features for consistent performance. REGEXMATCH performs case-sensitive matching by default, though case-insensitive matching can be achieved using the (?i) inline flag.

When working with REGEXMATCH, the function examines whether the pattern exists anywhere within the text string unless anchored with ^ (start) or $ (end) metacharacters. This means REGEXMATCH("hello world", "wor") returns TRUE because "wor" appears within the text. For exact full-string matching, you must use both anchors: ^pattern$. The function returns TRUE if any part of the text matches the pattern, making it essential to understand anchor usage for precise matching requirements.

REGEXMATCH is exclusive to Google Sheets and has no direct equivalent in Microsoft Excel. Excel users must rely on alternative approaches such as combining ISNUMBER with SEARCH for simple pattern detection, using VBA custom functions, or implementing complex nested formulas for pattern validation. The availability of REGEXMATCH in Google Sheets provides a significant advantage for text validation and pattern-based filtering operations, particularly when working with data that requires format verification.

> [!f(x)] REGEXMATCH Syntax
>
> ```spreadsheets
> REGEXMATCH(text, regular_expression)
> ```
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string to test against the regular expression pattern. Can be a cell reference, text string in quotes, or formula returning text. | Yes |
> | `regular_expression` | The RE2-compatible regular expression pattern to match against the text. Must be enclosed in quotes if provided directly. | Yes |

## REGEXMATCH Examples

| Scenario | Formula | Result | Explanation |
|----------|---------|--------|-------------|
| Basic pattern match | `=REGEXMATCH("hello world", "world")` | TRUE | The pattern "world" is found within the text string |
| Pattern not found | `=REGEXMATCH("hello world", "universe")` | FALSE | The pattern "universe" does not exist in the text |
| Email validation (simple) | `=REGEXMATCH("user@example.com", "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}")` | TRUE | Text matches basic email address pattern |
| Phone number detection | `=REGEXMATCH("Call me at 555-123-4567", "\d{3}-\d{3}-\d{4}")` | TRUE | Detects US phone number format within text |
| Case-insensitive match | `=REGEXMATCH("Hello World", "(?i)hello")` | TRUE | The (?i) flag enables case-insensitive matching |
| Exact string match | `=REGEXMATCH("test", "^test$")` | TRUE | Anchors ^ and $ ensure complete string match |
| Partial vs exact | `=REGEXMATCH("testing", "^test$")` | FALSE | "testing" doesn't exactly match "test" with anchors |
| Starts with pattern | `=REGEXMATCH("invoice-2024-001", "^invoice")` | TRUE | The ^ anchor ensures text starts with "invoice" |
| Ends with pattern | `=REGEXMATCH("document.pdf", "\.pdf$")` | TRUE | The $ anchor ensures text ends with ".pdf" |
| Contains digits | `=REGEXMATCH("Order 12345", "\d+")` | TRUE | Pattern \d+ matches one or more digits |
| Only alphabetic | `=REGEXMATCH("HelloWorld", "^[A-Za-z]+$")` | TRUE | Validates text contains only letters |
| ZIP code validation | `=REGEXMATCH("90210", "^\d{5}(-\d{4})?$")` | TRUE | Matches 5-digit or ZIP+4 postal codes |
| URL detection | `=REGEXMATCH("Visit https://example.com", "https?://[^\s]+")` | TRUE | Detects HTTP or HTTPS URLs in text |
| Alternation pattern | `=REGEXMATCH("apple", "apple\|orange\|banana")` | TRUE | Matches if text contains any of the alternatives |
| Word boundary simulation | `=REGEXMATCH("cat catalog", "\\bcat\\b")` | TRUE | Matches whole word "cat" using word boundaries |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid regular expression syntax | Check regex pattern for unescaped special characters or syntax errors; test pattern in a regex validator |
| `#VALUE!` | Non-text input provided | Ensure the text parameter is a string; use TEXT() or TO_TEXT() to convert numbers |
| `#N/A` | Empty text string | Handle empty values with IF or IFERROR: `=IFERROR(REGEXMATCH(A1, "pattern"), FALSE)` |
| `#REF!` | Referenced cell doesn't exist | Verify cell references are valid and within sheet boundaries |
| `#VALUE!` | Unbalanced parentheses in regex | Count opening and closing parentheses to ensure they match |
| `#VALUE!` | Invalid escape sequence | Use double backslash for literal backslash; escape special regex characters properly |
| `#VALUE!` | Unsupported regex feature | RE2 doesn't support backreferences or lookahead/lookbehind; rewrite pattern accordingly |
| Formula returns FALSE unexpectedly | Case sensitivity issue | Add (?i) at the start of the pattern for case-insensitive matching |
| Formula returns TRUE unexpectedly | Missing anchors | Add ^ and $ anchors if exact full-string matching is required |
| `#ERROR!` | Circular reference | Ensure the formula doesn't reference its own cell directly or indirectly |

## Use Cases

### Data Validation for Form Inputs

**Implementation**: Use REGEXMATCH to validate that user-entered data conforms to expected formats before processing.

```spreadsheets
// Email validation
=REGEXMATCH(A2, "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$")

// Phone validation (multiple formats)
=REGEXMATCH(B2, "^(\+1[-.\s]?)?(\(?\d{3}\)?[-.\s]?)?\d{3}[-.\s]?\d{4}$")

// Date format validation (MM/DD/YYYY)
=REGEXMATCH(C2, "^(0[1-9]|1[0-2])/(0[1-9]|[12]\d|3[01])/\d{4}$")

// Combined validation status
=IF(AND(REGEXMATCH(A2, email_pattern), REGEXMATCH(B2, phone_pattern)), "Valid", "Invalid")
```

- **Business Application**: Ensure data quality in customer registration forms, survey responses, and data entry systems by validating format compliance before data is stored or processed, reducing errors and cleanup costs.
- **Technical Details**: Create named ranges for commonly used regex patterns to improve formula readability and maintenance. Consider creating a validation summary dashboard that shows which records pass or fail each validation rule.

### Filtering and Categorizing Data

**Implementation**: Apply REGEXMATCH in FILTER formulas or conditional formatting to categorize or extract records matching specific patterns.

```spreadsheets
// Filter rows where description contains product codes
=FILTER(A2:D100, REGEXMATCH(B2:B100, "^PRD-\d{4}-[A-Z]{2}$"))

// Categorize transaction types by description patterns
=IF(REGEXMATCH(A2, "(?i)(payment|paid|remit)"), "Payment",
  IF(REGEXMATCH(A2, "(?i)(refund|return|credit)"), "Refund",
    IF(REGEXMATCH(A2, "(?i)(charge|fee|debit)"), "Charge", "Other")))

// Count items matching pattern
=COUNTIF(ARRAYFORMULA(REGEXMATCH(A2:A100, "^\d{3}-[A-Z]{2}-\d{4}$")), TRUE)
```

- **Business Application**: Automatically categorize transaction descriptions, support tickets, or log entries based on pattern matching, enabling faster reporting and analysis without manual classification.
- **Technical Details**: For large datasets, apply REGEXMATCH within ARRAYFORMULA for efficient batch processing. Create pattern libraries for common categorization schemes that can be referenced across multiple worksheets.

### Log File Analysis and Parsing

**Implementation**: Use REGEXMATCH to identify and flag specific log entries, error codes, or event patterns in system logs.

```spreadsheets
// Identify error-level log entries
=REGEXMATCH(A2, "(?i)\b(error|critical|fatal|exception)\b")

// Detect IP address patterns
=REGEXMATCH(A2, "\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b")

// Find timestamp patterns
=REGEXMATCH(A2, "\d{4}-\d{2}-\d{2}[T\s]\d{2}:\d{2}:\d{2}")

// Filter logs for specific HTTP status codes
=FILTER(A2:C1000, REGEXMATCH(B2:B1000, "\b(4\d{2}|5\d{2})\b"))
```

- **Business Application**: Monitor system health by flagging error conditions, security events, or performance issues in application logs imported to Google Sheets for analysis and alerting.
- **Technical Details**: Combine REGEXMATCH with REGEXEXTRACT to first identify relevant entries and then extract specific data elements. Create conditional formatting rules using REGEXMATCH to visually highlight different log severity levels.

### Document and File Name Validation

**Implementation**: Validate file names, document identifiers, or asset codes against organizational naming conventions.

```spreadsheets
// Validate invoice numbers (INV-YYYY-NNNN format)
=REGEXMATCH(A2, "^INV-20[2-9]\d-\d{4}$")

// Check file extension
=REGEXMATCH(A2, "(?i)\.(pdf|docx?|xlsx?)$")

// Validate project codes
=REGEXMATCH(A2, "^[A-Z]{3}-\d{4}-[A-Z]{2}$")

// Comprehensive naming convention check
=AND(
  REGEXMATCH(A2, "^[A-Za-z0-9_-]+$"),
  NOT(REGEXMATCH(A2, "^[-_]")),
  NOT(REGEXMATCH(A2, "[-_]$")),
  LEN(A2)<=50
)
```

- **Business Application**: Enforce document management standards by validating that file names, project codes, and asset identifiers follow organizational conventions before upload or processing.
- **Technical Details**: Create validation templates that combine multiple REGEXMATCH checks with AND/OR logic to enforce complex naming rules. Document patterns in a reference sheet for easy maintenance and onboarding.

## Platform Differences

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| Native REGEXMATCH function | Yes - full support | No - not available |
| Alternative approach | N/A | ISNUMBER(SEARCH()) for simple patterns, VBA for complex |
| Regex engine | RE2 | N/A (VBA uses VBScript regex) |
| Case-insensitive flag | (?i) inline flag | N/A |
| Array formula support | Yes with ARRAYFORMULA | N/A |
| Performance | Linear-time guaranteed | Varies with VBA implementation |
| Unicode support | Full Unicode support | Limited in VBA alternatives |

**Excel Workarounds**: For basic pattern detection in Excel, users can combine `=ISNUMBER(SEARCH("pattern", A1))` for case-insensitive literal matching or `=ISNUMBER(FIND("pattern", A1))` for case-sensitive matching. For true regex support, Excel requires VBA custom functions or third-party add-ins, making Google Sheets significantly more capable for pattern matching directly in formulas.

## Tips and Best Practices

1. **Use anchors for precision**: Always use ^ (start) and $ (end) anchors when you need to match the entire string, not just find a pattern within it. `^pattern$` ensures exact matching.

2. **Enable case-insensitive matching**: Add `(?i)` at the beginning of your pattern to ignore case differences. Example: `=REGEXMATCH(A1, "(?i)hello")` matches "Hello", "HELLO", and "hello".

3. **Escape special characters**: Regular expression metacharacters (. * + ? ^ $ [ ] ( ) { } | \) must be escaped with backslash when matching literally. To match a period, use `\.` not `.`

4. **Test patterns incrementally**: Build complex patterns step by step, testing each component. Start with the core pattern and add refinements gradually to identify issues easily.

5. **Create named ranges for patterns**: Store frequently used patterns in named ranges for reusability and easier maintenance. This makes formulas more readable and patterns easier to update.

6. **Use character classes efficiently**: Use shorthand character classes like `\d` (digits), `\w` (word characters), `\s` (whitespace) instead of explicit ranges to make patterns more concise and readable.

7. **Handle empty cells gracefully**: Wrap REGEXMATCH in IFERROR or IF(ISBLANK()) to handle empty cells without errors: `=IF(ISBLANK(A1), FALSE, REGEXMATCH(A1, "pattern"))`

8. **Combine with ARRAYFORMULA for batch processing**: Apply REGEXMATCH across entire columns efficiently: `=ARRAYFORMULA(REGEXMATCH(A2:A100, "pattern"))` instead of copying formulas to each row.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[REGEXEXTRACT]] | Extract text matching a pattern | Returns the matched text instead of TRUE/FALSE |
| [[REGEXREPLACE]] | Replace text matching a pattern | Modifies text rather than testing for matches |
| [[SEARCH]] | Find text position (case-insensitive) | Uses simple wildcards (* ?) instead of full regex |
| [[FIND]] | Find text position (case-sensitive) | Literal text matching only, no pattern support |
| [[SUBSTITUTE]] | Replace specific text | Exact string replacement, no pattern matching |
| [[ISNUMBER]] | Test if value is numeric | Often combined with SEARCH/FIND for pattern detection |

### Commonly Used Together

**[[IF]]** - Conditional logic based on pattern matching

```spreadsheets
=IF(REGEXMATCH(A1, "^[A-Z]{3}-\d{4}$"), "Valid Code", "Invalid Format")
```

Applies different logic based on whether text matches a pattern.

**[[FILTER]]** - Extract rows matching pattern criteria

```spreadsheets
=FILTER(A2:D100, REGEXMATCH(B2:B100, "(?i)urgent"))
```

Returns only rows where column B contains the pattern "urgent" (case-insensitive).

**[[ARRAYFORMULA]]** - Apply pattern matching across ranges

```spreadsheets
=ARRAYFORMULA(REGEXMATCH(A2:A1000, "\d{3}-\d{2}-\d{4}"))
```

Tests entire column for Social Security Number format in a single formula.

**[[COUNTIF]]** - Count pattern matches

```spreadsheets
=COUNTIF(ARRAYFORMULA(REGEXMATCH(A2:A100, "^PRD-")), TRUE)
```

Counts how many cells match the pattern (start with "PRD-").

**[[IFERROR]]** - Handle regex errors gracefully

```spreadsheets
=IFERROR(REGEXMATCH(A1, B1), FALSE)
```

Returns FALSE if the regex pattern in B1 is invalid or A1 is not text.

## Official Documentation

- [Google Sheets REGEXMATCH Function](https://support.google.com/docs/answer/3098292)
- [Google RE2 Syntax Reference](https://github.com/google/re2/wiki/Syntax)

## Version History

| Version/Date | Changes |
|--------------|---------|
| 2006 | Initial release with Google Sheets launch |
| 2010 | Improved RE2 engine integration |
| 2013 | Enhanced Unicode support for international character matching |
| 2016 | Better error messages for invalid regex patterns |
| 2019 | Performance optimizations for large dataset operations |
| 2022 | Improved ARRAYFORMULA compatibility and memory efficiency |
| 2024 | Enhanced named capture group handling in related REGEX functions |
