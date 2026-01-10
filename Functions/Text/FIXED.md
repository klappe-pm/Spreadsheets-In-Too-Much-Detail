---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- formatting
- decimals
- number-text
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- fixed-decimals
- format-number-text
---

# FIXED

## FIXED Description

The FIXED function rounds a number to a specified number of decimal places and converts the result to text, optionally including or omitting thousand separators. This function provides precise control over numeric text formatting, making it essential for creating labels, reports, and formatted output where the exact decimal precision and separator style must be explicitly controlled rather than relying on cell formatting.

FIXED performs two operations in sequence: first it rounds the number to the specified decimal places using standard rounding rules (half-up), then it converts the rounded value to a text string. This two-step process ensures that the text output matches the intended precision exactly. For example, FIXED(1234.567, 2) produces "1,234.57" - the number is rounded to two decimal places and formatted with thousand separators by default.

The third parameter of FIXED controls whether thousand separators appear in the output. When set to FALSE (the default) or omitted, separators are included for readability ("1,234.57"). When set to TRUE, separators are suppressed ("1234.57"). This control is valuable when generating output for systems that cannot parse comma-separated numbers or when alignment without separators is preferred.

FIXED is commonly used in data export scenarios, label generation, and formula-based report creation where numeric precision must be embedded in text strings. Since the output is text, not a number, FIXED results cannot be used directly in calculations. The function works identically across Excel and Google Sheets, with consistent behavior for decimal places, rounding, and separator handling.

## Syntax

```
=FIXED(number, [decimals], [no_commas])
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| number | Yes | The number to round and format as text. Can be a cell reference, literal number, or formula result. |
| decimals | No | The number of decimal places to display. Default is 2. Can be negative to round left of the decimal point. |
| no_commas | No | Logical value controlling thousand separators. FALSE or omitted includes commas; TRUE suppresses commas. |

## Examples

```
=FIXED(1234.567, 2)
// Returns: "1,234.57"
// Default formatting with commas and 2 decimal places

=FIXED(1234.567, 2, FALSE)
// Returns: "1,234.57"
// Explicit commas enabled (same as default)

=FIXED(1234.567, 2, TRUE)
// Returns: "1234.57"
// Commas suppressed for clean numeric string

=FIXED(1234.567, 0)
// Returns: "1,235"
// Rounded to whole number with commas

=FIXED(1234.567, 4)
// Returns: "1,234.5670"
// Extended to 4 decimal places with trailing zero

=FIXED(-1234.567, 2)
// Returns: "-1,234.57"
// Negative numbers include minus sign

=FIXED(0.5, 0)
// Returns: "1"
// Standard rounding: 0.5 rounds up to 1

=FIXED(1234567, -2)
// Returns: "1,234,600"
// Negative decimals round to hundreds place

=FIXED(A1, B1, C1)
// Returns: Formatted text based on cell values
// All parameters can be cell references

="Value: "&FIXED(SUM(A1:A10), 3, TRUE)
// Returns: "Value: 1234.567" (example)
// Concatenation without commas for clean output
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Non-numeric input for number parameter | Ensure input is numeric; use VALUE() to convert text numbers first |
| #VALUE! | Non-numeric or non-boolean for no_commas | Use TRUE/FALSE or 1/0 for the no_commas parameter |
| Unexpected rounding | Floating-point precision issues with certain decimals | Use ROUND() before FIXED for critical rounding requirements |
| Cannot calculate with result | FIXED returns text, not a number | Use VALUE() to convert back, or keep original numbers for calculations |
| Missing leading zero | Numbers less than 1 may need leading zero | FIXED includes leading zeros: FIXED(0.5,2) returns "0.50" |
| Locale separator issues | Thousand separator follows system locale in Excel | Use no_commas=TRUE for locale-independent output |

## Use Cases

### Precision Label Generation
**Business Context**: Scientific, engineering, or financial applications often require values displayed with exact decimal precision in labels or annotations where the precision itself conveys meaning.

**Implementation**: Use FIXED to ensure measurements, calculations, or financial figures appear with consistent decimal places regardless of the underlying value.

**Example Formula**:
```
=FIXED(A2, 4, TRUE)&" kg"
```

**Technical Details**: This displays a measurement like "12.3400 kg" where the four decimal places indicate measurement precision. The trailing zeros show that precision to four decimal places was measured, not just that the value happens to be 12.34.

### CSV and Export Data Preparation
**Business Context**: When preparing data for export to systems that parse numbers as text, controlling the exact format prevents parsing errors and ensures compatibility.

**Implementation**: Apply FIXED with no_commas=TRUE to create clean numeric strings suitable for data interchange.

**Example Formula**:
```
=FIXED(B2, 2, TRUE)
```

**Technical Details**: This converts 1234.567 to "1234.57" without thousand separators, which many import systems expect. Commas in numeric fields often cause parsing failures in CSV files or database imports.

### Aligned Numeric Reports
**Business Context**: Creating text-based reports or dashboards where numbers must align visually requires consistent formatting with explicit decimal places.

**Implementation**: Use FIXED with consistent decimal parameters across all numeric cells to ensure proper alignment when displayed in monospace fonts or text reports.

**Example Formula**:
```
=REPT(" ", 12-LEN(FIXED(A2, 2)))&FIXED(A2, 2)
```

**Technical Details**: This right-aligns a currency value in a 12-character field by prepending spaces. FIXED ensures consistent decimal places so all values align on the decimal point for professional report appearance.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Default decimals | 2 if omitted | 2 if omitted |
| Default no_commas | FALSE (commas shown) | FALSE (commas shown) |
| Thousand separator character | Locale-dependent (comma in US) | Always comma |
| Decimal separator | Locale-dependent (period in US) | Always period |
| Negative decimal support | Supported | Supported |
| Maximum decimals | Approximately 127 | Reasonable range supported |
| Rounding method | Half-up (standard) | Half-up (standard) |
| Array formula support | Supported | Automatic array expansion |

## Tips

1. **Versus TEXT function**: FIXED is simpler for basic decimal formatting, but TEXT offers more flexibility: `=TEXT(A1,"#,##0.00")` achieves similar results with format code control.

2. **Trailing zeros**: FIXED automatically adds trailing zeros: `FIXED(5, 3)` returns "5.000". This is useful for showing precision but may need adjustment for display purposes.

3. **Combine with SUBSTITUTE**: To use a different thousand separator: `=SUBSTITUTE(FIXED(A1,2),"," ," ")` replaces commas with spaces for European-style formatting.

4. **Performance consideration**: Since FIXED returns text, using it in large ranges that feed into calculations can impact performance. Format for display only at final output stage.

5. **Negative decimals trick**: `FIXED(12345, -3)` returns "12,000" - useful for rounding large numbers to thousands, millions: `FIXED(1234567,-6)&"M"` gives "1M".

## Related Functions

- [[TEXT]] - More flexible number-to-text formatting with custom format codes
- [[DOLLAR]] - Formats numbers as currency text with currency symbol
- [[ROUND]] - Rounds numbers without converting to text
- [[TRUNC]] - Truncates numbers to specified decimal places
- [[VALUE]] - Converts text numbers back to numeric values
- [[NUMBERVALUE]] - Parses formatted text back to numbers with locale control

## Official Documentation

- [Microsoft Excel FIXED Function](https://support.microsoft.com/en-us/office/fixed-function-ffd5723c-324c-45e9-8b96-e41be2a8274a)
- [Google Sheets FIXED Function](https://support.google.com/docs/answer/3093402)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Initial release |
| Excel 2007 | 2007 | No significant changes |
| Excel 2010 | 2010 | Improved locale handling |
| Excel 2013 | 2013 | No significant changes |
| Excel 2016 | 2016 | No significant changes |
| Excel 2019 | 2019 | No significant changes |
| Excel 365 | 2020+ | Dynamic array support |
| Google Sheets | 2006 | Available since launch with consistent formatting behavior |
