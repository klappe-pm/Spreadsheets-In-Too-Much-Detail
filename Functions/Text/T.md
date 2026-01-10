---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- type-conversion
- validation
- compatibility
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- text-value
- return-text
---

# T

## T Description

The T function examines a value and returns it as text if it is already text, or returns an empty string ("") if the value is not text. This simple but powerful function serves as a type-checking mechanism that ensures only genuine text values pass through while filtering out numbers, dates, logical values, and errors. Originally designed for compatibility with other spreadsheet applications, T remains useful for data validation and formula debugging scenarios.

T operates by evaluating the data type of its argument rather than attempting any conversion. When the argument is text (a string), T returns that text unchanged. When the argument is any other data type including numbers, Boolean values (TRUE/FALSE), error values, or empty cells, T returns an empty string. This behavior distinguishes T from functions like TEXT, which actively converts values to text representations.

The primary use case for T has evolved from its original compatibility purpose to serving as a diagnostic tool for identifying data types in mixed datasets. When troubleshooting formulas that behave unexpectedly due to type mismatches, T can help identify which cells contain genuine text versus numbers formatted as text or other data types that might appear similar in the spreadsheet interface.

T is particularly valuable in scenarios involving data import or user input where the expected data type is text but the actual content may vary. By wrapping cell references in T, formula authors can ensure consistent handling of mixed data types, preventing errors that might occur when text functions encounter non-text values. The function works identically across Excel and Google Sheets, making it a reliable choice for cross-platform spreadsheet development.

## Syntax

```
=T(value)
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| value | Yes | The value to test and potentially return. Can be a cell reference, a literal value, or a formula result. If the value is text, it is returned; otherwise, an empty string is returned. |

## Examples

```
=T("Hello World")
// Returns: "Hello World"
// Text input is returned unchanged

=T(123)
// Returns: "" (empty string)
// Numbers are not text, so empty string is returned

=T(TRUE)
// Returns: "" (empty string)
// Boolean values are not text

=T(A1)
// Returns: Text content of A1, or "" if A1 contains non-text
// Tests cell content type

=T(DATE(2024,1,15))
// Returns: "" (empty string)
// Date values are numeric internally, not text

=T("")
// Returns: "" (empty string)
// Empty string is technically text but appears empty

=T(A1)&T(B1)&T(C1)
// Returns: Concatenation of text values only
// Non-text cells contribute nothing to the result

=IF(T(A1)="", "Not Text", "Is Text")
// Returns: "Not Text" or "Is Text"
// Type checking for text values

=LEN(T(A1))
// Returns: Length of text in A1, or 0 if A1 is not text
// Safely get text length without errors

=T(INDIRECT("A"&ROW()))
// Returns: Text from dynamically referenced cell
// Works with indirect references
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Unexpected empty result | Cell contains a number formatted to look like text | Use TEXT() to convert numbers to text strings instead |
| Empty result from dates | Date values are stored as numbers internally | Use TEXT(date, "format") to convert dates to text |
| Empty result from formulas | Formula returns a number, not text | Wrap formula results in TEXT() for guaranteed text output |
| #REF! error | Invalid cell reference passed to T | Verify cell references are valid before using T |
| Confusion with TEXT function | T does not convert; it filters | Use TEXT() for conversion, T() for type filtering |
| Empty result from TRUE/FALSE | Boolean values are not considered text | Use TEXT() or IF() to convert booleans to text strings |

## Use Cases

### Data Type Validation
**Business Context**: When importing data from external sources, ensuring that text fields contain actual text values rather than numbers or other data types is critical for downstream processing.

**Implementation**: Use T to validate that imported data contains genuine text values, flagging cells that contain unexpected data types.

**Example Formula**:
```
=IF(T(A2)=A2, "Valid Text", "Not Text")
```

**Technical Details**: This formula compares the original value with its T result. If they match, the cell contains text. If T returns empty but A2 has content, the cell contains non-text data (number, date, boolean).

### Safe Text Concatenation
**Business Context**: Building composite strings from cells that may contain mixed data types, where non-text values should be silently excluded rather than causing errors.

**Implementation**: Wrap each cell reference in T when concatenating to ensure only text values contribute to the final result.

**Example Formula**:
```
=T(A2)&" "&T(B2)&" "&T(C2)
```

**Technical Details**: If A2 contains "John", B2 contains 42, and C2 contains "Smith", the result is "John  Smith" (note the extra space where the number was excluded). This prevents numeric values from appearing in text-only contexts.

### Formula Debugging and Type Checking
**Business Context**: When complex formulas produce unexpected results, identifying which cells contain text versus numbers stored as text helps diagnose type-related issues.

**Implementation**: Create a diagnostic row or column using T to reveal the true data type of each cell in a dataset.

**Example Formula**:
```
=IF(LEN(T(A2))>0, "TEXT", IF(ISNUMBER(A2), "NUMBER", "OTHER"))
```

**Technical Details**: This formula categorizes cell content into TEXT, NUMBER, or OTHER (for booleans, errors, blanks), helping identify data type inconsistencies that may cause formula failures.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Handling of numbers | Returns "" for all numeric types | Returns "" for all numeric types |
| Handling of booleans | Returns "" for TRUE/FALSE | Returns "" for TRUE/FALSE |
| Handling of errors | Returns "" (does not propagate error) | Returns "" (does not propagate error) |
| Empty cell handling | Returns "" for empty cells | Returns "" for empty cells |
| Array formula support | Supported | Supported with automatic expansion |
| Text formatted as numbers | Returns "" (evaluates underlying type) | Returns "" (evaluates underlying type) |
| Performance | Highly optimized | Highly optimized |

## Tips

1. **Type verification**: Use `=T(A1)=A1` to create a TRUE/FALSE test for whether a cell contains text (returns TRUE for text, FALSE for numbers/dates/booleans).

2. **Default value pattern**: Combine T with IF for default values: `=IF(T(A1)="", "Default", T(A1))` provides fallback text when non-text values are encountered.

3. **Filter text from ranges**: In array formulas, T can help isolate text values: `=FILTER(A1:A10, T(A1:A10)<>"")` returns only cells containing actual text.

4. **Preserve empty strings**: Remember that T("") returns "" - it considers empty strings as valid text, just zero-length text.

5. **Not a converter**: T does not convert anything to text. For conversion, use TEXT(), CONCATENATE with "", or the ampersand operator with empty string (&"").

## Related Functions

- [[TEXT]] - Converts numbers to text with specified formatting
- [[VALUE]] - Converts text representations of numbers to actual numbers
- [[N]] - Returns numbers as numbers, non-numbers as 0 (counterpart to T)
- [[ISTEXT]] - Returns TRUE if value is text, FALSE otherwise (boolean check)
- [[ISNUMBER]] - Returns TRUE if value is a number
- [[TYPE]] - Returns a number indicating the data type of a value
- [[CLEAN]] - Removes non-printable characters from text

## Official Documentation

- [Microsoft Excel T Function](https://support.microsoft.com/en-us/office/t-function-fb83aeec-45e7-4924-af95-53e073541228)
- [Google Sheets T Function](https://support.google.com/docs/answer/3094305)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Initial release for Lotus 1-2-3 compatibility |
| Excel 2007 | 2007 | No significant changes |
| Excel 2010 | 2010 | No significant changes |
| Excel 2013 | 2013 | No significant changes |
| Excel 2016 | 2016 | No significant changes |
| Excel 2019 | 2019 | No significant changes |
| Excel 365 | 2020+ | Dynamic array support |
| Google Sheets | 2006 | Available since launch with full compatibility |
