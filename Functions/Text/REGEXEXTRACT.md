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
  - data-extraction
  - text-parsing
  - regex
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - regex-extract
  - pattern-extract
  - regex-capture
tags:
  - text
  - regex
  - pattern-matching
  - extraction
  - google-sheets
  - sheets-only
---

# REGEXEXTRACT

## Description

REGEXEXTRACT is a powerful Google Sheets-exclusive function that extracts the first matching substring from a text string based on a regular expression pattern. Unlike simple text functions like LEFT, RIGHT, or MID that require fixed positions or lengths, REGEXEXTRACT uses pattern matching to find and extract text based on character patterns, making it invaluable for parsing unstructured or semi-structured data. The function returns the portion of the text that matches the specified regular expression pattern, or the first captured group if the pattern contains capturing parentheses. This enables sophisticated text extraction operations that would be extremely complex or impossible with traditional text functions alone.

The primary use case for REGEXEXTRACT is extracting specific pieces of information from text strings that follow predictable patterns but vary in position or length. Common applications include extracting email addresses from contact information, pulling phone numbers from text blocks, isolating product codes or SKUs from descriptions, parsing dates from various text formats, extracting domain names from URLs, pulling numeric values from mixed alphanumeric strings, and capturing specific segments from structured identifiers like invoice numbers or serial codes. The function is particularly valuable when dealing with imported data that contains mixed information in single cells, or when parsing log files, system outputs, or data exported from other applications.

Several critical aspects of REGEXEXTRACT require careful attention. First, the function uses RE2 regular expression syntax, which differs slightly from other regex flavors (no lookahead/lookbehind, no backreferences in the pattern itself). Second, REGEXEXTRACT returns only the FIRST match found in the text; if you need all matches, you must use REGEXEXTRACT with SUBSTITUTE in a loop or use a custom function. Third, if your pattern contains capturing groups (parentheses), the function returns the content of the first captured group rather than the entire match—this is incredibly useful for extracting specific portions but can be confusing if unexpected. Fourth, the function returns a #N/A error when no match is found, so wrapping it in IFERROR is often necessary. Fifth, the function is case-sensitive by default; use (?i) at the start of your pattern for case-insensitive matching. Finally, special regex characters (. * + ? ^ $ { } [ ] \ | ( )) must be escaped with backslash if you want to match them literally.

REGEXEXTRACT is exclusively available in Google Sheets and has no direct equivalent in Microsoft Excel. Excel users must use combinations of FIND, SEARCH, MID, LEFT, RIGHT, and nested formulas to achieve similar results, or utilize VBA/Power Query for regex functionality. For Excel 365 users, the TEXTBEFORE and TEXTAFTER functions provide simpler alternatives for common extraction scenarios, but still lack true regex pattern matching. When building spreadsheets that must work across both platforms, avoid REGEXEXTRACT and use alternative approaches, or maintain separate formulas for each platform. Google Sheets' implementation uses the RE2 regex engine developed by Google, which is highly optimized for performance but lacks some advanced features found in PCRE (Perl-Compatible Regular Expressions).

## Syntax

> [!f(x)] REGEXEXTRACT Syntax
>
> ```
> =REGEXEXTRACT(text, regular_expression)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The input string from which to extract matching text. Can be a cell reference, a text string in quotes, or a formula that returns text. If the value is not text, it will be converted to text before pattern matching. | Yes |
> | `regular_expression` | The regular expression pattern to match against the text. Must be a valid RE2 regular expression enclosed in quotes. If the pattern contains capturing groups, returns the first captured group; otherwise returns the entire match. | Yes |

## Regular Expression Quick Reference

| Pattern | Meaning | Example |
|---------|---------|---------|
| `.` | Any single character except newline | `a.c` matches "abc", "a1c" |
| `*` | Zero or more of preceding element | `ab*c` matches "ac", "abc", "abbc" |
| `+` | One or more of preceding element | `ab+c` matches "abc", "abbc" but not "ac" |
| `?` | Zero or one of preceding element | `ab?c` matches "ac", "abc" |
| `^` | Start of string | `^abc` matches "abc" at start only |
| `$` | End of string | `abc$` matches "abc" at end only |
| `[abc]` | Any character in brackets | `[aeiou]` matches any vowel |
| `[^abc]` | Any character NOT in brackets | `[^0-9]` matches any non-digit |
| `[a-z]` | Character range | `[A-Za-z]` matches any letter |
| `\d` | Any digit (0-9) | `\d{3}` matches three digits |
| `\D` | Any non-digit | `\D+` matches one or more non-digits |
| `\w` | Word character (letter, digit, underscore) | `\w+` matches words |
| `\W` | Non-word character | `\W` matches punctuation, spaces |
| `\s` | Whitespace (space, tab, newline) | `\s+` matches whitespace runs |
| `\S` | Non-whitespace | `\S+` matches non-space runs |
| `{n}` | Exactly n occurrences | `\d{4}` matches exactly 4 digits |
| `{n,}` | n or more occurrences | `\d{3,}` matches 3+ digits |
| `{n,m}` | Between n and m occurrences | `\d{2,4}` matches 2-4 digits |
| `(...)` | Capturing group | `(\d+)` captures digits |
| `(?:...)` | Non-capturing group | `(?:ab)+` groups without capturing |
| `\|` | Alternation (OR) | `cat\|dog` matches "cat" or "dog" |
| `(?i)` | Case-insensitive mode | `(?i)abc` matches "ABC", "Abc" |
| `\\` | Escaped backslash | `\\d` matches literal "\d" |

## Examples

### Example 1: Extract Email Address from Text
```
=REGEXEXTRACT(A1, "[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}")
```
Where A1 contains "Contact John at john.doe@company.com for details"

**Result:** `john.doe@company.com`

**Explanation:** This pattern matches the standard email format: one or more allowed characters before the @, a domain name with dots, and a 2+ letter TLD. The pattern [A-Za-z0-9._%+-]+ matches the local part, @ matches literally, [A-Za-z0-9.-]+ matches the domain, \. matches the literal dot, and [A-Za-z]{2,} matches the TLD.

---

### Example 2: Extract Phone Number with Area Code
```
=REGEXEXTRACT(A1, "\(?(\d{3})\)?[-.\s]?(\d{3})[-.\s]?(\d{4})")
```
Where A1 contains "Call me at (555) 123-4567 anytime"

**Result:** `(555) 123-4567`

**Explanation:** This pattern matches phone numbers in various formats: (555) 123-4567, 555-123-4567, 555.123.4567, or 555 123 4567. The \(? and \)? make parentheses optional, \d{3} matches exactly 3 digits, and [-.\s]? matches optional separators (hyphen, period, or space).

---

### Example 3: Extract Numbers from Mixed Text
```
=REGEXEXTRACT(A1, "\d+\.?\d*")
```
Where A1 contains "The price is $149.99 per unit"

**Result:** `149.99`

**Explanation:** This pattern matches numeric values including decimals. \d+ matches one or more digits, \.? optionally matches a decimal point (escaped because . is special in regex), and \d* matches zero or more digits after the decimal. This extracts the first complete number found.

---

### Example 4: Extract Domain from URL
```
=REGEXEXTRACT(A1, "https?://(?:www\.)?([^/]+)")
```
Where A1 contains "https://www.example.com/page/info"

**Result:** `example.com`

**Explanation:** This pattern uses a capturing group to extract just the domain. https? matches http or https, (?:www\.)? is a non-capturing group for optional www., and ([^/]+) captures everything up to the next slash. Because of the capturing group, only the domain is returned.

---

### Example 5: Extract Text Between Brackets
```
=REGEXEXTRACT(A1, "\[([^\]]+)\]")
```
Where A1 contains "Reference [ABC-12345] approved"

**Result:** `ABC-12345`

**Explanation:** The pattern \[ matches a literal opening bracket (escaped), ([^\]]+) captures one or more characters that are NOT closing brackets, and \] matches the literal closing bracket. The capturing group ensures only the content between brackets is returned.

---

### Example 6: Extract Product Code Pattern
```
=REGEXEXTRACT(A1, "[A-Z]{2,3}-\d{4,6}")
```
Where A1 contains "Order SKU: ABC-123456 quantity 5"

**Result:** `ABC-123456`

**Explanation:** This pattern matches a specific code format: 2-3 uppercase letters, hyphen, 4-6 digits. [A-Z]{2,3} matches 2-3 capital letters, - matches literally, and \d{4,6} matches 4-6 digits. Adjust the quantifiers to match your specific code format.

---

### Example 7: Extract Date in Various Formats
```
=REGEXEXTRACT(A1, "\d{1,2}[/-]\d{1,2}[/-]\d{2,4}")
```
Where A1 contains "Invoice dated 12/25/2024 is due"

**Result:** `12/25/2024`

**Explanation:** This flexible pattern matches dates with slashes or hyphens: \d{1,2} matches 1-2 digit day/month, [/-] matches either separator, and \d{2,4} matches 2-4 digit year. This captures MM/DD/YYYY, DD-MM-YY, and similar formats.

---

### Example 8: Extract First Word Only
```
=REGEXEXTRACT(A1, "^\s*(\w+)")
```
Where A1 contains "   Hello World and Everyone"

**Result:** `Hello`

**Explanation:** The ^ anchors to the start, \s* skips any leading whitespace, and (\w+) captures the first word (letters, digits, underscores). This is useful for extracting first names, primary identifiers, or initial keywords from text.

---

### Example 9: Extract Hashtags from Social Media
```
=REGEXEXTRACT(A1, "#(\w+)")
```
Where A1 contains "Great product! #recommended #musthave"

**Result:** `recommended`

**Explanation:** This matches a hashtag pattern where # matches literally and (\w+) captures the tag text. Note that this returns only the first hashtag found. The capturing group excludes the # symbol from the result.

---

### Example 10: Extract IP Address
```
=REGEXEXTRACT(A1, "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}")
```
Where A1 contains "Server 192.168.1.100 is online"

**Result:** `192.168.1.100`

**Explanation:** This pattern matches IPv4 addresses: four groups of 1-3 digits separated by dots. \d{1,3} matches each octet and \. matches literal dots. Note this doesn't validate the range (0-255); it only extracts the pattern.

---

### Example 11: Extract Currency Amount with Symbol
```
=REGEXEXTRACT(A1, "[$€£¥]\s*[\d,]+\.?\d*")
```
Where A1 contains "Total cost: $1,234.56 USD"

**Result:** `$1,234.56`

**Explanation:** [$€£¥] matches common currency symbols, \s* allows optional space, [\d,]+ matches digits with optional commas, \.? optionally matches decimal point, and \d* matches decimal digits. This extracts the complete formatted currency value.

---

### Example 12: Case-Insensitive Extraction
```
=REGEXEXTRACT(A1, "(?i)error[:\s]+(.+)")
```
Where A1 contains "ERROR: Connection timeout occurred"

**Result:** `Connection timeout occurred`

**Explanation:** The (?i) flag at the start makes the entire pattern case-insensitive, matching "error", "ERROR", or "Error". The capturing group (.+) extracts everything after "error:" and any whitespace.

---

### Example 13: Extract ZIP Code (US Format)
```
=REGEXEXTRACT(A1, "\d{5}(?:-\d{4})?")
```
Where A1 contains "Ship to NYC, NY 10001-1234"

**Result:** `10001-1234`

**Explanation:** This matches US ZIP codes in both 5-digit and ZIP+4 formats. \d{5} matches the base 5 digits, and (?:-\d{4})? optionally matches the hyphen and 4-digit extension. The non-capturing group (?:...) ensures the whole match is returned.

---

### Example 14: Extract Text After Specific Word
```
=REGEXEXTRACT(A1, "Status:\s*(.+)")
```
Where A1 contains "Order Status: Processing - Expected 3 days"

**Result:** `Processing - Expected 3 days`

**Explanation:** This pattern finds "Status:" followed by optional whitespace \s*, then captures everything that follows with (.+). This is useful for extracting values from key-value formatted text.

---

### Example 15: Extract Time from Text
```
=REGEXEXTRACT(A1, "\d{1,2}:\d{2}(?::\d{2})?\s*(?:AM|PM|am|pm)?")
```
Where A1 contains "Meeting scheduled for 2:30 PM today"

**Result:** `2:30 PM`

**Explanation:** This pattern matches time in various formats. \d{1,2}:\d{2} matches hours and minutes, (?::\d{2})? optionally matches seconds, \s* allows optional space, and (?:AM|PM|am|pm)? optionally matches AM/PM indicators.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | No match found in the text for the given pattern | Verify pattern matches expected text; use IFERROR to handle no-match cases gracefully |
| `#REF!` | Invalid regular expression syntax | Check for unescaped special characters; verify bracket/parenthesis balance |
| `#VALUE!` | Empty text or pattern argument | Ensure both arguments contain valid values; check for empty cells |
| `Function not recognized` | Using in Excel instead of Google Sheets | REGEXEXTRACT is Google Sheets only; use VBA or alternative functions in Excel |
| `Returns wrong portion` | Capturing group returns different content than expected | Review capturing group placement; use (?:...) for grouping without capturing |
| `Case sensitivity issues` | Pattern doesn't match due to case differences | Add (?i) at start of pattern for case-insensitive matching |
| `Partial match returned` | Greedy quantifiers matching less than expected | Use more specific patterns or anchor with ^ and $ |
| `Special character not matching` | Forgetting to escape regex metacharacters | Escape . * + ? [ ] ( ) { } ^ $ \| \ with backslash |
| `Backslash errors` | Single backslash not working | In Sheets, use single backslash; double-check escape sequences |
| `Only first match returned` | Function only returns first match by design | Use REGEXEXTRACT with array formulas or custom functions for all matches |

## Use Cases

### [[Email and Contact Data Extraction]]
- **Implementation**: Extract email addresses, phone numbers, and social media handles from unstructured text fields imported from various sources. Use patterns like `[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}` for emails and `\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}` for phone numbers.
- **Business Application**: Clean and normalize contact databases by extracting standardized contact information from notes fields, email signatures, or imported data. Enable automated lead processing by parsing contact details from form submissions or web scrapes.
- **Technical Details**: Combine with IFERROR to handle missing data gracefully. Use TRIM on results to remove extra whitespace. For multiple contacts in one cell, consider using SPLIT first or implementing array formulas. Validate extracted emails with REGEXMATCH before using.

### [[Product and Inventory Code Parsing]]
- **Implementation**: Extract SKUs, part numbers, serial numbers, and product codes from descriptions, filenames, or imported data. Pattern examples: `[A-Z]{2,4}-\d{4,8}` for typical SKU format, `SN[:\s]*([A-Z0-9]+)` for serial numbers with prefix.
- **Business Application**: Automate inventory management by parsing product identifiers from shipping manifests, supplier catalogs, or scanned documents. Enable lookup operations by extracting standardized codes from varied input formats.
- **Technical Details**: Adjust patterns to match your specific code format. Use capturing groups to exclude prefixes/suffixes. Consider multiple REGEXEXTRACT calls for different code types in the same data.

### [[Log File and System Data Analysis]]
- **Implementation**: Parse timestamps, error codes, IP addresses, and status messages from log entries and system outputs. Patterns: `\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}` for ISO timestamps, `(?:ERROR|WARN|INFO):\s*(.+)` for log messages.
- **Business Application**: Create spreadsheet-based log analyzers for troubleshooting and monitoring. Extract actionable data from system exports without specialized log analysis tools.
- **Technical Details**: Handle multi-line log entries with careful pattern design. Use REGEXMATCH first to filter relevant rows, then REGEXEXTRACT for data extraction. Consider performance with large datasets.

### [[Financial and Transaction Data Parsing]]
- **Implementation**: Extract amounts, account numbers, transaction codes, and dates from financial statements, bank exports, or payment records. Patterns: `[$€£]\s*[\d,]+\.?\d{0,2}` for currency, `\d{4}[-\s]?\d{4}[-\s]?\d{4}[-\s]?\d{4}` for card numbers.
- **Business Application**: Automate reconciliation processes by extracting transaction details from varied statement formats. Parse invoice data for accounts payable automation.
- **Technical Details**: Be careful with currency parsing across locales (comma vs. period decimals). Use VALUE() on extracted amounts for calculations. Mask sensitive data appropriately after extraction.

## Platform Differences

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| **Function Availability** | Native function, fully supported | NOT AVAILABLE - no built-in equivalent |
| **Regex Engine** | RE2 (Google's regex library) | N/A |
| **Alternative in Excel** | N/A | Combinations of FIND, MID, LEFT, RIGHT; VBA; Power Query |
| **Lookahead/Lookbehind** | NOT supported (RE2 limitation) | N/A |
| **Backreferences** | NOT supported in pattern | N/A |
| **Case Sensitivity** | Case-sensitive by default; use (?i) flag | N/A |
| **Capturing Groups** | Supported; returns first group content | N/A |
| **Array Formula Support** | Works with ARRAYFORMULA | N/A |
| **Performance** | Optimized RE2 engine | N/A |

**Excel Alternatives:**

For Excel users needing similar functionality:
1. **VBA Custom Function**: Create a User-Defined Function using VBScript.RegExp
2. **Power Query**: Use M language with Text.RegexMatch/RegexReplace
3. **FILTERXML with SUBSTITUTE**: Complex workaround for simple patterns
4. **TEXTAFTER/TEXTBEFORE (Excel 365)**: For simpler delimiter-based extraction
5. **Manual formulas**: Nested MID, FIND, SEARCH, LEN combinations

**Cross-Platform Strategy:**

When spreadsheets must work in both environments:
- Avoid REGEXEXTRACT entirely
- Use TEXTBEFORE/TEXTAFTER where applicable (Excel 365)
- Create lookup tables for pattern-based categorization
- Document Sheets-only features clearly

## Tips and Best Practices

1. **Always Wrap in IFERROR**: REGEXEXTRACT returns #N/A when no match is found. Use `=IFERROR(REGEXEXTRACT(A1, pattern), "")` to return empty string or a default value instead.

2. **Test Patterns Incrementally**: Build complex patterns step by step. Start with a simple pattern that matches, then add specificity. Use a regex testing tool (like regex101.com with Golang/RE2 flavor) to develop patterns before implementing.

3. **Use Capturing Groups Strategically**: When you need only part of a match, wrap that part in parentheses. `https?://([^/]+)` returns just the domain, not the protocol. Use `(?:...)` for grouping without capturing.

4. **Escape Special Characters**: Remember that . * + ? ^ $ { } [ ] \ | ( ) have special meanings. To match them literally, precede with backslash: `\$\d+` matches "$100".

5. **Handle Case Sensitivity**: Add `(?i)` at the start of your pattern for case-insensitive matching. `(?i)error` matches "error", "ERROR", and "Error".

6. **Be Specific to Avoid False Matches**: Overly broad patterns may match unintended text. `\d+` in "Order #123 placed on 12/15/2024" matches "123" but might grab dates too. Add context: `#(\d+)` captures only the order number.

7. **Combine with Other Functions**: Use REGEXEXTRACT within larger formulas: `=VALUE(REGEXEXTRACT(A1, "\d+\.?\d*"))` extracts and converts to number. Chain with TRIM, UPPER, LOWER for cleaning.

8. **Document Your Patterns**: Complex regex patterns are hard to understand later. Add comments in adjacent cells or use named ranges for common patterns like `EmailPattern` containing the regex string.

9. **Consider Performance**: Regex operations can be slow on large datasets. Minimize REGEXEXTRACT calls in heavily used spreadsheets. Consider extracting to helper columns once rather than using in multiple formulas.

10. **Know RE2 Limitations**: Google Sheets uses RE2 which lacks lookahead (?=), lookbehind (?<=), and backreferences (\1). Design patterns accordingly or use workarounds with multiple extraction steps.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from REGEXEXTRACT |
|----------|-------------|----------------------------------|
| [[REGEXMATCH]] | Tests if text matches a pattern, returns TRUE/FALSE | Returns Boolean for testing, not extracted text |
| [[REGEXREPLACE]] | Replaces text matching a pattern | Modifies text rather than extracting it |
| [[MID]] | Extracts text by position and length | Requires known fixed positions, no pattern matching |
| [[LEFT]]/[[RIGHT]] | Extracts from start/end of text | Fixed positions only, no pattern matching |
| [[FIND]]/[[SEARCH]] | Locates position of substring | Returns position number, not extracted text |
| [[SPLIT]] | Divides text by delimiter | Delimiter-based, not pattern-based |

### Commonly Used Together

**[[IFERROR]]** - Handle no-match cases gracefully

*Wrap REGEXEXTRACT to avoid #N/A errors:*
```
=IFERROR(REGEXEXTRACT(A1, "\d+"), "No number found")
```
Returns a friendly message or default value when the pattern doesn't match.

---

**[[REGEXMATCH]]** - Pre-filter before extraction

*Check if pattern exists before extracting:*
```
=IF(REGEXMATCH(A1, "@"), REGEXEXTRACT(A1, "[^@]+@[^@]+"), "No email")
```
Validates pattern existence before attempting extraction.

---

**[[ARRAYFORMULA]]** - Apply extraction to entire columns

*Extract from multiple cells at once:*
```
=ARRAYFORMULA(IFERROR(REGEXEXTRACT(A2:A100, "\d+"), ""))
```
Processes an entire range efficiently with single formula.

---

**[[VALUE]]** - Convert extracted numbers to numeric type

*Extract and convert to number for calculations:*
```
=VALUE(REGEXEXTRACT(A1, "\d+\.?\d*"))
```
Enables mathematical operations on extracted numeric strings.

---

**[[TRIM]]** - Clean extracted text

*Remove extra whitespace from extracted text:*
```
=TRIM(REGEXEXTRACT(A1, "Name:\s*(.+)"))
```
Ensures clean output without leading/trailing spaces.

---

**[[SUBSTITUTE]]** - Combine with extraction for complex parsing

*Extract all matches iteratively:*
```
=REGEXEXTRACT(SUBSTITUTE(A1, B1, ""), pattern)
```
Extract first match, remove it, extract next (for multiple matches).

## Official Documentation

- **Google Sheets**: [REGEXEXTRACT function](https://support.google.com/docs/answer/3098244)
- **RE2 Syntax**: [RE2 Regular Expression Syntax](https://github.com/google/re2/wiki/Syntax)

## Version History

| Platform | Version/Date | Changes |
|----------|--------------|---------|
| Google Sheets | 2006 | Original function release |
| Google Sheets | 2012 | Improved RE2 engine performance |
| Google Sheets | 2015 | Better Unicode support |
| Google Sheets | 2018 | Enhanced ARRAYFORMULA compatibility |
| Google Sheets | 2020 | Performance optimizations for large datasets |
| Google Sheets | 2022 | Continued stability improvements |
| Microsoft Excel | N/A | Function not available in any version |

---

*Last updated: 2026-01-10*
