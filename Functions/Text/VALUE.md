---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- conversion
- parsing
- numbers
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- text-to-number
- parse-number
---

# VALUE

## VALUE Description

The VALUE function converts a text string that represents a number into an actual numeric value that can be used in mathematical calculations. This conversion is essential when working with data imported from external sources, text files, or databases where numeric values may be stored or formatted as text strings. VALUE intelligently parses various number formats including percentages, currencies, dates, and times, making it a versatile tool for data transformation.

VALUE recognizes and converts a wide range of text formats into their numeric equivalents. A text string like "1234.56" becomes the number 1234.56, while "50%" becomes 0.5, and recognized date strings become Excel serial date numbers. The function respects regional settings for decimal separators and thousands separators, so "1,234.56" works in US/UK locales while "1.234,56" works in European locales when system settings are configured appropriately.

When VALUE encounters text that cannot be interpreted as a number, it returns a #VALUE! error. This includes text containing letters, invalid number formats, or mixed content that does not conform to recognized numeric patterns. Understanding what VALUE can and cannot parse is critical for error-free data processing, as silent failures or unexpected conversions can propagate through dependent calculations.

VALUE is frequently used in data cleaning workflows, ETL processes, and formula development where type consistency is required. While modern versions of Excel and Google Sheets often perform implicit text-to-number conversion in mathematical contexts, VALUE provides explicit control over the conversion process and is necessary when automatic conversion does not occur or when predictable behavior is required.

## Syntax

```
=VALUE(text)
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| text | Yes | The text string to convert to a number. Can be a cell reference containing text, a text string in quotation marks, or a formula that returns text. Must represent a valid number, date, time, or percentage format. |

## Examples

```
=VALUE("1234")
// Returns: 1234
// Basic text-to-number conversion

=VALUE("1234.56")
// Returns: 1234.56
// Decimal numbers are parsed correctly

=VALUE("$1,234.56")
// Returns: 1234.56
// Currency symbols and thousand separators are handled

=VALUE("50%")
// Returns: 0.5
// Percentages are converted to decimal equivalents

=VALUE("1/15/2024")
// Returns: 45306 (or similar serial date number)
// Date strings become Excel date serial numbers

=VALUE("2:30 PM")
// Returns: 0.604166667
// Time strings become decimal fractions of a day

=VALUE(A1)
// Returns: Numeric value if A1 contains valid number text
// Common usage with cell references

=VALUE(LEFT(A1, 5))
// Returns: Number from first 5 characters
// Combined with text extraction functions

=VALUE(SUBSTITUTE(A1, ",", ""))
// Returns: Number with commas removed
// Preprocessing text before conversion

=IFERROR(VALUE(A1), 0)
// Returns: Converted number or 0 if conversion fails
// Error handling for invalid text
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Text contains non-numeric characters | Remove letters and invalid characters using SUBSTITUTE or CLEAN before conversion |
| #VALUE! | Empty cell or empty string | Check for empty values with IF(A1="", 0, VALUE(A1)) |
| #VALUE! | Unrecognized number format | Ensure text matches regional number formatting expectations |
| #VALUE! | Multiple decimal separators | Clean data to have only one decimal point |
| #VALUE! | Leading/trailing spaces with special characters | Use TRIM() to remove spaces before conversion |
| Unexpected date number | Date string converted to serial number | This is expected behavior; use DATEVALUE for explicit date conversion |

## Use Cases

### Data Import Cleanup
**Business Context**: When importing data from CSV files, databases, or web sources, numeric columns often arrive as text strings that cannot be summed or used in calculations without conversion.

**Implementation**: Apply VALUE to imported text columns to convert them to usable numbers, often combined with error handling to manage invalid entries.

**Example Formula**:
```
=IFERROR(VALUE(TRIM(CLEAN(A2))), "Invalid")
```

**Technical Details**: This formula removes non-printable characters with CLEAN, trims whitespace, converts to number, and returns "Invalid" for unconvertible entries. This approach handles most common import issues.

### Extracting Numbers from Mixed Text
**Business Context**: Product codes, reference numbers, or identifiers often contain embedded numbers that need to be extracted and converted for sorting, comparison, or calculation purposes.

**Implementation**: Use text functions to extract the numeric portion, then VALUE to convert it for mathematical operations.

**Example Formula**:
```
=VALUE(MID(A2, FIND("-", A2)+1, 5))
```

**Technical Details**: This extracts 5 characters after the first hyphen and converts them to a number. Useful for codes like "PROD-12345-A" where "12345" needs to be extracted as a number.

### Currency and Percentage Normalization
**Business Context**: Financial data from different sources may include currency symbols, thousand separators, or percentage signs that prevent direct calculation.

**Implementation**: Use VALUE to convert formatted currency and percentage strings into clean numbers for consistent analysis.

**Example Formula**:
```
=VALUE(SUBSTITUTE(SUBSTITUTE(A2, "$", ""), ",", ""))
```

**Technical Details**: This removes dollar signs and commas before conversion, handling values like "$1,234,567.89" that VALUE alone might not parse correctly depending on regional settings.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic number conversion | Fully supported | Fully supported |
| Currency symbol handling | Recognizes locale currency | Recognizes locale currency |
| Percentage conversion | Converts "50%" to 0.5 | Converts "50%" to 0.5 |
| Date string parsing | Converts to serial date number | Converts to serial date number |
| Time string parsing | Converts to decimal day fraction | Converts to decimal day fraction |
| Thousand separator handling | Follows regional settings | Follows regional settings |
| Scientific notation | Supports "1.23E+10" format | Supports "1.23E+10" format |
| Negative number formats | Supports "-123" and "(123)" | Supports "-123" and "(123)" |
| Array formula support | Supported | Automatic array expansion |

## Tips

1. **Preprocessing**: Always clean text before conversion with TRIM and CLEAN: `=VALUE(TRIM(CLEAN(A1)))` handles most whitespace and invisible character issues.

2. **Regional awareness**: VALUE uses system locale settings. A formula that works on a US system may fail on a European system due to different decimal/thousand separators.

3. **Versus multiplication by 1**: While `A1*1` or `A1+0` can also convert text to numbers, VALUE provides clearer intent and better error messages when conversion fails.

4. **Date ambiguity**: VALUE converts date strings using regional date format settings. "1/2/2024" may be January 2nd or February 1st depending on locale.

5. **Nested in calculations**: You do not need VALUE when using text numbers in math operations like `=A1+B1` as Excel/Sheets auto-convert. Use VALUE for explicit control or in non-math contexts.

## Related Functions

- [[TEXT]] - Converts numbers to formatted text (reverse of VALUE)
- [[DATEVALUE]] - Specifically converts date text to serial date numbers
- [[TIMEVALUE]] - Specifically converts time text to decimal fractions
- [[NUMBERVALUE]] - Converts text with specific decimal/group separators
- [[T]] - Returns text if value is text, otherwise empty string
- [[N]] - Converts value to number, returning 0 for non-numbers
- [[INT]] - Truncates to integer after conversion

## Official Documentation

- [Microsoft Excel VALUE Function](https://support.microsoft.com/en-us/office/value-function-257d0108-07dc-437d-ae1c-bc2d3953d8c2)
- [Google Sheets VALUE Function](https://support.google.com/docs/answer/3094220)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Initial release |
| Excel 2007 | 2007 | Improved international format support |
| Excel 2010 | 2010 | Enhanced currency symbol recognition |
| Excel 2013 | 2013 | Better handling of regional formats |
| Excel 2016 | 2016 | No significant changes |
| Excel 2019 | 2019 | No significant changes |
| Excel 365 | 2020+ | Dynamic array support |
| Google Sheets | 2006 | Available since launch |
