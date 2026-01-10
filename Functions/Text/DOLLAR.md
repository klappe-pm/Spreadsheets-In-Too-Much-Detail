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
- currency
- financial
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- currency-format
- money-text
---

# DOLLAR

## DOLLAR Description

The DOLLAR function converts a number to text formatted as currency, using the dollar sign ($) and the specified number of decimal places. Unlike cell formatting which only changes the display while keeping the underlying numeric value, DOLLAR actually transforms the number into a text string with embedded currency formatting. This makes it useful for creating labels, reports, and exports where the currency symbol must be part of the actual cell content rather than just a display format.

DOLLAR automatically includes thousand separators (commas in US format) in the output text, providing professional-looking currency representations without additional formatting steps. The function rounds the number to the specified decimal places before converting to text, so DOLLAR(1234.567, 2) produces "$1,234.57" with proper rounding. Negative values are displayed in parentheses following standard accounting notation, such as "($500.00)" for negative five hundred.

The naming of this function can be misleading because DOLLAR does not necessarily produce output with a dollar sign in all locales. In Excel, the function uses the system's default currency symbol based on Windows regional settings, so users in Europe might see euro signs or pound symbols instead of dollar signs. Google Sheets, however, consistently uses the dollar sign regardless of locale, making cross-platform behavior somewhat inconsistent.

DOLLAR is particularly valuable when building text strings that include monetary values, creating export-ready reports, or when currency-formatted values need to be concatenated with other text. Since the result is text, not a number, the output cannot be used directly in calculations without first converting back to a number using VALUE or similar functions.

## Syntax

```
=DOLLAR(number, [decimals])
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| number | Yes | The number to convert to currency-formatted text. Can be a cell reference, a literal number, or a formula that returns a number. |
| decimals | No | The number of decimal places to include. Default is 2. Can be negative to round to the left of the decimal point (e.g., -1 rounds to nearest 10). |

## Examples

```
=DOLLAR(1234.567)
// Returns: "$1,234.57"
// Default 2 decimal places with rounding

=DOLLAR(1234.567, 2)
// Returns: "$1,234.57"
// Explicit 2 decimal places

=DOLLAR(1234.567, 0)
// Returns: "$1,235"
// No decimal places, rounds to nearest whole number

=DOLLAR(1234.567, 3)
// Returns: "$1,234.567"
// Three decimal places shown

=DOLLAR(-500, 2)
// Returns: "($500.00)"
// Negative values in accounting format with parentheses

=DOLLAR(1234567.89, 2)
// Returns: "$1,234,567.89"
// Thousand separators automatically included

=DOLLAR(45.678, -1)
// Returns: "$50"
// Negative decimals round to tens place

=DOLLAR(A1, B1)
// Returns: Currency text based on cell values
// Dynamic decimal places from cell reference

="Total: "&DOLLAR(SUM(A1:A10), 2)
// Returns: "Total: $1,234.56" (example)
// Concatenation with other text

=DOLLAR(0.5, 2)
// Returns: "$0.50"
// Small values formatted correctly
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Non-numeric text passed as number argument | Ensure input is a valid number or use VALUE() to convert text first |
| Unexpected currency symbol | Excel uses system locale currency | Use TEXT() with explicit format code for guaranteed dollar sign |
| Cannot calculate with result | DOLLAR returns text, not a number | Use VALUE(SUBSTITUTE(SUBSTITUTE(result,"$",""),",","")) to convert back |
| Wrong thousand separator | Regional settings affect output format | Consistent formatting may require TEXT() with explicit format codes |
| Negative number display issues | Parentheses format may not be desired | Use TEXT() for hyphen-minus negative format: TEXT(A1,"$#,##0.00") |
| #NUM! error | Decimal places argument too large or invalid | Keep decimals within reasonable range (typically -15 to 127) |

## Use Cases

### Financial Report Generation
**Business Context**: Creating text-based reports, invoices, or statements where currency values must appear as formatted text within cells that may be concatenated or exported.

**Implementation**: Use DOLLAR to format monetary totals for display in report headers, footers, or summary cells where text formatting is required.

**Example Formula**:
```
="Invoice Total: "&DOLLAR(SUM(C2:C50), 2)
```

**Technical Details**: This creates a text label with the sum of invoice items formatted as currency. The result might be "Invoice Total: $5,234.89" which can be displayed, printed, or exported as a complete text string.

### Export-Ready Data Preparation
**Business Context**: When exporting spreadsheet data to systems that require pre-formatted text rather than numeric values with separate formatting, DOLLAR ensures currency formatting survives the export process.

**Implementation**: Apply DOLLAR to monetary columns before export to CSV or text files where cell formatting would otherwise be lost.

**Example Formula**:
```
=DOLLAR(B2, 2)
```

**Technical Details**: Apply this to a column of prices before exporting. The exported file will contain "$1,234.56" as actual text rather than just "1234.56" which would lose the currency context.

### Conditional Currency Display
**Business Context**: Displaying currency values with different precision levels based on magnitude - showing cents for small amounts but rounding large amounts to whole dollars.

**Implementation**: Use IF logic with DOLLAR to vary decimal places based on the value size.

**Example Formula**:
```
=DOLLAR(A2, IF(A2<100, 2, 0))
```

**Technical Details**: This shows two decimal places for amounts under $100 and no decimals for larger amounts. A $45.67 displays as "$45.67" while $1234.56 displays as "$1,235" for cleaner large-number presentation.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Currency symbol | Uses system locale (may show other symbols) | Always shows dollar sign ($) |
| Negative format | Parentheses: ($500.00) | Parentheses: ($500.00) |
| Thousand separators | Locale-dependent (comma in US) | Always comma |
| Decimal separator | Locale-dependent (period in US) | Always period |
| Maximum decimals | Up to 127 | Reasonable range supported |
| Negative decimals | Supported (rounds left of decimal) | Supported |
| Default decimals | 2 if omitted | 2 if omitted |
| Array formula support | Supported | Automatic array expansion |

## Tips

1. **Explicit dollar sign**: If you need guaranteed dollar sign regardless of locale in Excel, use `=TEXT(A1, "$#,##0.00")` instead of DOLLAR.

2. **Different currencies**: DOLLAR is only for dollar-style formatting. For euros, pounds, or other currencies, use TEXT() with appropriate format codes: `=TEXT(A1, "[$]#,##0.00")`.

3. **Calculation warning**: Since DOLLAR returns text, formulas like `=DOLLAR(100,2)+DOLLAR(200,2)` will fail. Keep numbers as numbers for calculations, apply DOLLAR only for final display.

4. **Rounding behavior**: DOLLAR rounds using standard rules (0.5 rounds up). For specific rounding behavior, apply ROUND, ROUNDUP, or ROUNDDOWN before DOLLAR.

5. **Negative decimal trick**: Use negative decimals to round to significant figures: `=DOLLAR(1234567,-3)` returns "$1,235,000" - useful for summary reports.

## Related Functions

- [[TEXT]] - Converts numbers to text with custom format codes (more flexible than DOLLAR)
- [[FIXED]] - Formats a number as text with fixed decimal places, optional thousands separator
- [[VALUE]] - Converts currency text back to numbers
- [[ROUND]] - Rounds numbers before formatting
- [[YEN]] - Similar to DOLLAR but for Japanese Yen (Excel only)
- [[NUMBERVALUE]] - Parses formatted number text back to numbers

## Official Documentation

- [Microsoft Excel DOLLAR Function](https://support.microsoft.com/en-us/office/dollar-function-a6cd05d9-9740-4ad3-a469-8109d18ff611)
- [Google Sheets DOLLAR Function](https://support.google.com/docs/answer/3093317)

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
| Google Sheets | 2006 | Available since launch with consistent dollar sign behavior |
