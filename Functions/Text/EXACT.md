---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- comparison
- validation
- case-sensitivity
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- case-sensitive-compare
- string-match
---

# EXACT

## EXACT Description

The EXACT function performs a case-sensitive comparison between two text strings, returning TRUE if they are identical in every respect and FALSE otherwise. Unlike standard equality operators (=) in Excel and Google Sheets which ignore case differences, EXACT distinguishes between uppercase and lowercase letters, making it essential for scenarios where precise text matching is required, such as password validation, code verification, or data quality checks.

EXACT compares strings character by character, examining not only the letters but also their case, making "Hello" and "hello" distinctly different values. The function treats numbers as text when comparing, so the number 100 and the text "100" are considered equal by EXACT. This behavior makes EXACT particularly useful for validating user input, verifying data entry accuracy, and implementing case-sensitive business logic in spreadsheets.

When working with EXACT, it is important to understand that the function evaluates the actual content of cells, not their displayed formatting. Leading and trailing spaces are significant and will cause two otherwise identical strings to be considered different. Similarly, non-printing characters and special Unicode characters are compared literally, which can lead to unexpected FALSE results when strings appear visually identical but contain hidden character differences.

EXACT is commonly used in data validation workflows, quality assurance processes, and conditional formatting rules where case sensitivity matters. It pairs well with functions like IF, SUMPRODUCT, and COUNTIF to create sophisticated validation systems. The function accepts exactly two arguments and cannot be extended to compare more than two strings simultaneously, though nested EXACT functions or array formulas can achieve multi-string comparisons when needed.

## Syntax

```
=EXACT(text1, text2)
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| text1 | Yes | The first text string to compare. Can be a cell reference, text in quotation marks, or a formula that returns text. |
| text2 | Yes | The second text string to compare against the first. Can be a cell reference, text in quotation marks, or a formula that returns text. |

## Examples

```
=EXACT("Hello", "Hello")
// Returns: TRUE
// Both strings are identical in content and case

=EXACT("Hello", "hello")
// Returns: FALSE
// Strings differ in case - "H" vs "h"

=EXACT("Apple", "APPLE")
// Returns: FALSE
// Case-sensitive comparison treats these as different

=EXACT(A1, B1)
// Returns: TRUE or FALSE
// Compares cell contents case-sensitively

=EXACT("Password123", "password123")
// Returns: FALSE
// Useful for password validation where case matters

=EXACT(100, "100")
// Returns: TRUE
// Numbers are converted to text for comparison

=EXACT("  Hello", "Hello")
// Returns: FALSE
// Leading spaces make the strings different

=EXACT(UPPER(A1), UPPER(B1))
// Returns: TRUE if strings match ignoring case
// Workaround for case-insensitive EXACT comparison

=IF(EXACT(A1, "APPROVED"), "Valid", "Invalid")
// Returns: "Valid" only if A1 contains exactly "APPROVED"
// Conditional logic based on exact match

=SUMPRODUCT(--(EXACT(A1:A10, "Yes")))
// Returns: Count of cells containing exactly "Yes"
// Counts case-sensitive matches in a range
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Unexpected FALSE | Hidden spaces or non-printing characters in one of the strings | Use TRIM() or CLEAN() to remove extraneous characters before comparison |
| Unexpected FALSE | Different Unicode characters that appear identical (e.g., different dash types) | Use CODE() to examine character codes and identify hidden differences |
| Type mismatch confusion | Comparing a number stored as text with an actual number | Both values are converted to text, so this typically works; verify cell formats if issues persist |
| Case sensitivity ignored | Using = operator instead of EXACT | Replace = with EXACT for case-sensitive comparisons |
| #VALUE! error | Comparing to an error value | Wrap in IFERROR or check for errors before comparison |
| FALSE with formatted numbers | Number formatting differences (e.g., currency symbols) | Compare raw values, not formatted display text |

## Use Cases

### Password and Code Validation
**Business Context**: Organizations need to verify that users enter passwords, product codes, or authorization codes exactly as specified, including correct capitalization.

**Implementation**: Use EXACT to compare user input against stored values where case sensitivity is critical for security or identification purposes.

**Example Formula**:
```
=IF(EXACT(B2, $D$1), "Access Granted", "Access Denied")
```

**Technical Details**: This approach ensures that "Admin123" is not accepted when "admin123" was entered, maintaining security standards and preventing unauthorized access through case variations.

### Data Quality Assurance
**Business Context**: Data teams need to identify discrepancies between datasets where case differences indicate potential errors or inconsistencies in data entry.

**Implementation**: Compare imported data against master records to flag entries that differ only in case, which may indicate data entry errors or system inconsistencies.

**Example Formula**:
```
=IF(AND(A2=B2, NOT(EXACT(A2, B2))), "Case Mismatch", "OK")
```

**Technical Details**: This formula identifies records that match when ignoring case but differ in capitalization, highlighting potential standardization issues without flagging completely different values.

### Duplicate Detection with Case Sensitivity
**Business Context**: Product catalogs, username systems, and inventory databases often require case-sensitive uniqueness checks where "SKU-abc" and "SKU-ABC" must be treated as distinct items.

**Implementation**: Use EXACT within COUNTIF or SUMPRODUCT to count exact matches rather than case-insensitive matches.

**Example Formula**:
```
=SUMPRODUCT((EXACT($A$2:$A$100, A2))*1)-1
```

**Technical Details**: This formula counts exact duplicates of the value in A2 within the range, subtracting 1 to exclude the cell itself. Values differing only in case are not counted as duplicates.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Array formula support | Supported with Ctrl+Shift+Enter (legacy) or automatic (365) | Automatic array expansion |
| Number comparison | Numbers converted to text | Numbers converted to text |
| Empty cell handling | Empty cells treated as "" | Empty cells treated as "" |
| Error value comparison | Returns error if either argument is an error | Returns error if either argument is an error |
| Unicode support | Full Unicode support | Full Unicode support |
| Maximum string length | 32,767 characters | 50,000 characters |
| Performance with large ranges | Optimized in Excel 365 | Generally fast |

## Tips

1. **Combine with TRIM**: Always use `EXACT(TRIM(A1), TRIM(B1))` when comparing user-entered data to eliminate hidden space issues that cause unexpected FALSE results.

2. **Case-insensitive alternative**: To make EXACT case-insensitive, wrap both arguments in UPPER or LOWER: `=EXACT(UPPER(A1), UPPER(B1))`.

3. **Multiple comparisons**: For comparing more than two strings, nest EXACT functions with AND: `=AND(EXACT(A1,B1), EXACT(B1,C1))`.

4. **Wildcard workaround**: EXACT does not support wildcards. For pattern matching with case sensitivity, use FIND (which is case-sensitive) instead.

5. **Debugging mismatches**: When EXACT returns FALSE unexpectedly, use `=CODE(MID(A1,1,1))` to compare character codes and identify hidden character differences.

## Related Functions

- [[FIND]] - Case-sensitive search for a string within another string, returns position
- [[SEARCH]] - Case-insensitive search for a string within another string
- [[UPPER]] - Converts text to uppercase, useful for case-insensitive comparisons
- [[LOWER]] - Converts text to lowercase
- [[TRIM]] - Removes extra spaces from text, important for accurate EXACT comparisons
- [[CLEAN]] - Removes non-printable characters that may cause EXACT to return FALSE
- [[DELTA]] - Similar comparison function for numbers, returns 1 if equal, 0 if not

## Official Documentation

- [Microsoft Excel EXACT Function](https://support.microsoft.com/en-us/office/exact-function-d3087698-fc15-4a15-9631-12575cf29926)
- [Google Sheets EXACT Function](https://support.google.com/docs/answer/3094062)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Initial release |
| Excel 2007 | 2007 | No significant changes |
| Excel 2010 | 2010 | Improved Unicode handling |
| Excel 2013 | 2013 | No significant changes |
| Excel 2016 | 2016 | No significant changes |
| Excel 2019 | 2019 | No significant changes |
| Excel 365 | 2020+ | Dynamic array support for spilled results |
| Google Sheets | 2006 | Available since launch with full functionality |
