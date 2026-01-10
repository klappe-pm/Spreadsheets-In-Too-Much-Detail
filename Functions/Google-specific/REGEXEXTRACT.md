---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - text-manipulation
  - regular-expressions
subTopics:
  - pattern-extraction
  - text-parsing
  - data-extraction
  - regex
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - regex-extract
  - pattern-extract
  - extract-pattern
  - regex-match
tags:
  - google-sheets-only
  - regex
  - text-functions
  - pattern-matching
  - data-extraction
---

# REGEXEXTRACT

## Description

The **REGEXEXTRACT function** is a Google Sheets-exclusive function that extracts the first substring matching a regular expression pattern from a text string. This powerful text manipulation function enables sophisticated pattern-based extraction that goes far beyond what simple text functions like LEFT, RIGHT, or MID can achieve. REGEXEXTRACT is essential for parsing structured data like extracting phone numbers from text, isolating email domains, pulling numbers from mixed alphanumeric strings, and any scenario requiring pattern-based text extraction.

The function uses RE2 regular expression syntax, which supports most common regex features including character classes, quantifiers, anchors, groups, and alternation. When the pattern contains capturing groups (parentheses), REGEXEXTRACT returns only the portion matching the first capturing group, enabling precise extraction of specific parts within a larger pattern. If there are multiple capturing groups, the function returns an array with one element per group, allowing extraction of multiple components in a single formula.

REGEXEXTRACT searches for the pattern anywhere within the text and returns the first match found. If no match exists, the function returns an error (#N/A), making it important to wrap in IFERROR for robust formulas. The function is case-sensitive by default, but case-insensitive matching can be achieved using the (?i) flag at the start of the pattern. This combination of power and flexibility makes REGEXEXTRACT invaluable for data cleaning, parsing, and transformation tasks.

REGEXEXTRACT is exclusively available in Google Sheets and has no direct equivalent in Microsoft Excel. Excel users must use combinations of functions like FIND, MID, LEFT, RIGHT, or resort to VBA/Power Query for regex capabilities. Excel 365 does not include native regex functions, making REGEXEXTRACT a significant differentiator for Google Sheets in text processing scenarios.

## Syntax

> [!f(x)] REGEXEXTRACT Syntax
>
> ```
> =REGEXEXTRACT(text, regular_expression)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string to search within. Can be a literal string in quotes, a cell reference, or an expression returning text. |
| `regular_expression` | Yes | The regular expression pattern to match and extract. Must be a valid RE2 regex pattern. Use capturing groups (parentheses) to specify exactly which part to extract. |

### Return Value

Returns the first substring matching the regular expression. If the pattern contains capturing groups, returns the content of the first capturing group (or an array if multiple groups). Returns #N/A error if no match is found. Returns #REF! if the regex is invalid.

## Examples

> [!f(x)] REGEXEXTRACT Examples

### Example 1: Extract Numbers from Text
```
=REGEXEXTRACT("Order #12345 confirmed", "\d+")
```
**Scenario:** Pull the order number from a confirmation message
**Result:** "12345"

**Explanation:** `\d+` matches one or more digits. REGEXEXTRACT finds the first sequence of digits in the text.

---

### Example 2: Extract Email Domain
```
=REGEXEXTRACT("user@example.com", "@(.+)")
```
**Scenario:** Get the domain portion of an email address
**Result:** "example.com"

**Explanation:** The capturing group `(.+)` captures everything after @. Without the group, it would include the @.

---

### Example 3: Extract First Word
```
=REGEXEXTRACT("Hello World", "^\w+")
```
**Scenario:** Get the first word from a string
**Result:** "Hello"

**Explanation:** `^` anchors to the start, `\w+` matches word characters. This extracts the first word.

---

### Example 4: Extract Phone Number
```
=REGEXEXTRACT("Call me at 555-123-4567 today", "\d{3}-\d{3}-\d{4}")
```
**Scenario:** Pull a formatted phone number from text
**Result:** "555-123-4567"

**Explanation:** The pattern matches exactly 3 digits, hyphen, 3 digits, hyphen, 4 digits.

---

### Example 5: Extract Text Between Brackets
```
=REGEXEXTRACT("Status: [APPROVED]", "\[([^\]]+)\]")
```
**Scenario:** Get the content inside square brackets
**Result:** "APPROVED"

**Explanation:** `\[` matches opening bracket, `([^\]]+)` captures non-bracket characters, `\]` matches closing bracket.

---

### Example 6: Extract Multiple Groups
```
=REGEXEXTRACT("John Smith (john@email.com)", "(\w+) (\w+) \((.+)\)")
```
**Scenario:** Extract first name, last name, and email as separate values
**Result:** {"John", "Smith", "john@email.com"} (array of 3 values)

**Explanation:** Three capturing groups create three output values. The formula spills to adjacent cells.

---

### Example 7: Extract Date Components
```
=REGEXEXTRACT("Date: 12/25/2024", "(\d{1,2})/(\d{1,2})/(\d{4})")
```
**Scenario:** Extract month, day, year as separate values
**Result:** {"12", "25", "2024"}

**Explanation:** Three capturing groups isolate each date component. Results are text, not numbers.

---

### Example 8: Case-Insensitive Extraction
```
=REGEXEXTRACT("Email: USER@EXAMPLE.COM", "(?i)email:\s*(.+)")
```
**Scenario:** Extract email regardless of "Email" capitalization
**Result:** "USER@EXAMPLE.COM"

**Explanation:** `(?i)` makes the pattern case-insensitive. "EMAIL:", "email:", "Email:" all match.

---

### Example 9: Extract URL from Text
```
=REGEXEXTRACT("Visit https://www.example.com/page for more", "https?://[^\s]+")
```
**Scenario:** Pull a URL from surrounding text
**Result:** "https://www.example.com/page"

**Explanation:** `https?://` matches http or https, `[^\s]+` matches non-whitespace characters until a space.

---

### Example 10: Extract Last Word
```
=REGEXEXTRACT("Hello Beautiful World", "\w+$")
```
**Scenario:** Get the last word from a string
**Result:** "World"

**Explanation:** `\w+` matches word characters, `$` anchors to the end. Together they capture the last word.

---

### Example 11: With IFERROR for Missing Matches
```
=IFERROR(REGEXEXTRACT(A1, "\d+"), "No number found")
```
**Scenario:** Extract number but handle cases where no number exists
**Result:** The number if found, or "No number found"

**Explanation:** IFERROR catches the #N/A that REGEXEXTRACT returns when no match exists.

---

### Example 12: Extract Specific Pattern from Log
```
=REGEXEXTRACT("2024-01-15 ERROR: File not found (code: 404)", "code:\s*(\d+)")
```
**Scenario:** Extract error code from a log message
**Result:** "404"

**Explanation:** The pattern matches "code:" followed by optional whitespace and captures the digits.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | No match found for the pattern | Verify pattern matches expected text; use IFERROR for graceful handling |
| `#REF!` | Invalid regular expression syntax | Check regex syntax; escape special characters properly |
| `#VALUE!` | Empty text or invalid parameters | Ensure text is not empty; verify cell references |
| `Extracting wrong portion` | Capturing group issue | Ensure parentheses surround the part you want to extract |
| `Only partial match` | Greedy vs. lazy quantifiers | Use `+?` or `*?` for lazy matching to avoid over-matching |
| `Case mismatch` | Pattern is case-sensitive by default | Add `(?i)` at pattern start for case-insensitive matching |
| `Special characters not matching` | Unescaped special characters | Escape special regex characters with backslash: `\.` `\[` `\(` |
| `Only first match returned` | Function returns first match only | Use REGEXEXTRACT multiple times or other approaches for all matches |

## Use Cases

### [[Parsing Email Addresses]]

**Scenario:** A CRM system has email addresses mixed with other text in a notes field. The marketing team needs to extract clean email addresses, separate them into username and domain parts, and identify domain types (gmail, corporate, etc.).

**Implementation:**
```
Email parsing structure:
Column A: Raw text containing email
Column B: Extracted email
Column C: Username part
Column D: Domain part
Column E: Domain type

Extract email from text (B2):
=IFERROR(REGEXEXTRACT(A2, "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"), "No email found")

Extract username (C2):
=IFERROR(REGEXEXTRACT(B2, "(.+)@"), "")

Extract domain (D2):
=IFERROR(REGEXEXTRACT(B2, "@(.+)"), "")

Classify domain type (E2):
=IF(REGEXMATCH(D2, "gmail|yahoo|hotmail|outlook"), "Personal",
  IF(REGEXMATCH(D2, "edu"), "Education", "Corporate"))

Validate email format:
=REGEXMATCH(B2, "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$")
```

**Business Application:** Email extraction from unstructured data is essential for contact management. Parsing into components enables domain-based segmentation, bounce rate analysis by domain type, and data quality validation. Corporate vs. personal email classification helps prioritize B2B leads.

**Technical Details:** The email regex pattern matches standard email formats but may not cover all valid addresses. Adjust pattern for specific needs. Username extraction uses `(.+)@` to capture everything before @. Domain extraction uses `@(.+)` to capture everything after.

---

### [[Log File Analysis]]

**Scenario:** A system generates log files with structured text entries. Analysts need to extract timestamps, error codes, user IDs, and message types from each log line for analysis and alerting.

**Implementation:**
```
Log analysis structure:
Log format: "2024-01-15 14:30:45 [ERROR] User:12345 - Database connection failed (code:500)"

Column A: Raw log line
Column B: Timestamp
Column C: Log Level
Column D: User ID
Column E: Error Code
Column F: Message

Timestamp (B2):
=IFERROR(REGEXEXTRACT(A2, "^(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})"), "")

Log Level (C2):
=IFERROR(REGEXEXTRACT(A2, "\[(ERROR|WARN|INFO|DEBUG)\]"), "UNKNOWN")

User ID (D2):
=IFERROR(REGEXEXTRACT(A2, "User:(\d+)"), "System")

Error Code (E2):
=IFERROR(REGEXEXTRACT(A2, "code:(\d+)"), "N/A")

Message (F2):
=IFERROR(REGEXEXTRACT(A2, "\] .+ - (.+?) \("), "")

Filter ERROR logs:
=FILTER(A:F, C:C="ERROR")
```

**Business Application:** Log analysis is critical for system monitoring, debugging, and compliance. Extracting structured fields from log text enables filtering by severity, tracking errors by user, and identifying patterns. Automated extraction turns raw logs into actionable data.

**Technical Details:** Log patterns vary; adjust regex for your specific format. Use alternation `(ERROR|WARN|INFO)` for known values. Lazy quantifiers `+?` prevent over-matching. Combine with FILTER for targeted analysis.

---

### [[Data Cleaning and Standardization]]

**Scenario:** A database import contains messy data with phone numbers, IDs, and codes in various formats. The data team needs to extract and standardize these values for clean import into a new system.

**Implementation:**
```
Data cleaning structure:
Column A: Messy input data
Column B: Extracted phone (standardized)
Column C: Extracted ID
Column D: Extracted code

Extract phone number (various formats) (B2):
=IFERROR(REGEXEXTRACT(A2, "\(?(\d{3})\)?[-.\s]?(\d{3})[-.\s]?(\d{4})"), "")
Then: =IFERROR(REGEXEXTRACT(B2, "(\d{3})(\d{3})(\d{4})"), "") to get just digits

Standardize phone format:
=IF(B2<>"", "("&LEFT(B2,3)&") "&MID(B2,4,3)&"-"&RIGHT(B2,4), "")

Extract ID with prefix:
=IFERROR(REGEXEXTRACT(A2, "ID[:\s]?([A-Z]{2}\d{6})"), "")

Extract currency amount:
=IFERROR(VALUE(REGEXEXTRACT(A2, "\$?([\d,]+\.?\d*)")), 0)

Extract date in various formats:
=IFERROR(REGEXEXTRACT(A2, "(\d{1,2}[/-]\d{1,2}[/-]\d{2,4})"), "")

Clean and validate:
=IF(AND(B2<>"", C2<>""), "Complete", "Missing data")
```

**Business Application:** Data migration and integration projects frequently encounter inconsistent formats. REGEXEXTRACT identifies and extracts valid data from messy inputs, enabling automated cleaning that would otherwise require manual review of thousands of records.

**Technical Details:** Phone patterns account for various formats: (555) 123-4567, 555.123.4567, 555-123-4567. Multiple capturing groups extract components separately. VALUE() converts extracted numeric strings to numbers. Combine with IFERROR for records without matches.

---

### [[URL Parsing and Analysis]]

**Scenario:** A marketing analytics team has URLs from various campaigns and needs to extract domain, path, parameters, and campaign codes for attribution analysis.

**Implementation:**
```
URL analysis structure:
Column A: Full URL
Column B: Protocol
Column C: Domain
Column D: Path
Column E: UTM Source

Full URL example: "https://www.example.com/products/item?utm_source=google&utm_medium=cpc"

Protocol (B2):
=IFERROR(REGEXEXTRACT(A2, "^(https?)://"), "")

Domain (C2):
=IFERROR(REGEXEXTRACT(A2, "://([^/]+)"), "")

Path (D2):
=IFERROR(REGEXEXTRACT(A2, "://[^/]+(/[^?]*)"), "/")

UTM Source (E2):
=IFERROR(REGEXEXTRACT(A2, "utm_source=([^&]+)"), "direct")

UTM Medium:
=IFERROR(REGEXEXTRACT(A2, "utm_medium=([^&]+)"), "")

UTM Campaign:
=IFERROR(REGEXEXTRACT(A2, "utm_campaign=([^&]+)"), "")

Any parameter value:
=IFERROR(REGEXEXTRACT(A2, "paramname=([^&]+)"), "")

Domain without www:
=REGEXEXTRACT(C2, "^(?:www\.)?(.+)")
```

**Business Application:** Marketing attribution depends on URL parameter extraction. UTM parameters identify traffic sources, campaigns, and mediums. Clean domain extraction enables site-level analysis. Path extraction shows which content drives conversions.

**Technical Details:** URL structure is protocol://domain/path?parameters. `[^/]+` matches until slash. `[^&]+` matches until & (next parameter). `(?:www\.)?` is a non-capturing group that optionally matches "www." without including it in the result.

## Platform Differences

REGEXEXTRACT is **exclusively available in Google Sheets** with no native equivalent in Microsoft Excel.

### Google Sheets
- **Availability:** All versions since introduction
- **Regex Engine:** RE2 (fast, safe, but no backreferences or lookahead)
- **Multiple Captures:** Returns array when multiple capturing groups used
- **Integration:** Works with ARRAYFORMULA for batch processing

### Microsoft Excel
- **Native Regex Functions:** None available
- **Alternatives:**
  - **VBA:** Use RegExp object in custom functions
  - **Power Query:** M language has Text.RegexMatch
  - **FIND/MID/LEFT/RIGHT:** Combine for simple patterns
  - **Flash Fill:** Can learn patterns but not formula-based
- **Limitations:** No formula-based regex without VBA

### Key Differences Summary

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| Native Regex Function | Yes | No |
| Formula-based Extraction | Yes | No (VBA only) |
| Capturing Groups | Supported | N/A |
| Case Insensitivity Flag | Yes ((?i)) | N/A |
| Batch Processing | ARRAYFORMULA | N/A |

## Tips and Best Practices

1. **Always use IFERROR for production formulas:** REGEXEXTRACT returns #N/A when no match is found. Wrap in IFERROR for graceful handling: `=IFERROR(REGEXEXTRACT(...), "default")`.

2. **Use capturing groups to extract specific parts:** Parentheses `()` define what gets returned. `@(.+)` returns just the domain, while `@.+` returns "@domain".

3. **Escape special regex characters:** Characters like `.` `[` `]` `(` `)` `{` `}` `*` `+` `?` `^` `$` `|` `\` have special meaning. Escape them with backslash when matching literally.

4. **Use `(?i)` for case-insensitive matching:** Place at the start of the pattern: `(?i)email` matches "EMAIL", "Email", "email".

5. **Test patterns on sample data first:** Regex can be tricky. Test on a variety of inputs before applying to entire columns.

6. **Combine with ARRAYFORMULA for batch processing:** `=ARRAYFORMULA(IFERROR(REGEXEXTRACT(A2:A, "\d+"), ""))` processes entire columns.

7. **Use non-capturing groups for structure without extraction:** `(?:prefix)` groups without including in result.

8. **Remember REGEXEXTRACT returns text:** Even extracted numbers are text. Use VALUE() to convert to numbers if needed.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[REGEXMATCH]] | Tests if pattern matches (returns TRUE/FALSE) | When you need to check if pattern exists, not extract |
| [[REGEXREPLACE]] | Replaces matching patterns | When transforming text, not extracting |
| [[MID]] / [[LEFT]] / [[RIGHT]] | Extract by position | For fixed-position extraction (simpler, no regex) |
| [[SPLIT]] | Split by delimiter | When delimiter-based, not pattern-based |

### Commonly Used Together

**[[IFERROR]]** - Handle no-match cases

*Graceful handling of missing matches:*
```
=IFERROR(REGEXEXTRACT(A1, "\d+"), "No number")
```
Essential for robust REGEXEXTRACT formulas.

---

**[[REGEXMATCH]]** - Check before extracting

*Verify pattern exists:*
```
=IF(REGEXMATCH(A1, "\d+"), REGEXEXTRACT(A1, "\d+"), "None")
```
Alternative to IFERROR when you want specific logic.

---

**[[VALUE]]** - Convert extracted text to number

*Extract and convert number:*
```
=VALUE(REGEXEXTRACT(A1, "\d+"))
```
REGEXEXTRACT returns text; VALUE converts to number.

---

**[[ARRAYFORMULA]]** - Batch processing

*Extract from entire column:*
```
=ARRAYFORMULA(IFERROR(REGEXEXTRACT(A2:A, "\d+"), ""))
```
Process many cells with one formula.

---

**[[SUBSTITUTE]]** - Clean before extracting

*Remove unwanted characters first:*
```
=REGEXEXTRACT(SUBSTITUTE(A1, ",", ""), "\d+")
```
Pre-process text before pattern matching.

## Official Documentation

- **Google Sheets:** [REGEXEXTRACT function](https://support.google.com/docs/answer/3098244)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | ~2012 | Original implementation with RE2 engine |
| Google Sheets | 2015 | Performance improvements |
| Google Sheets | 2018 | Better ARRAYFORMULA support |
| Google Sheets | 2020 | Enhanced error handling |

---

*Last updated: 2026-01-10*
