---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - text-manipulation
  - data-conversion
subTopics:
  - text-to-number
  - locale-conversion
  - decimal-separator
  - international-numbers
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - text-to-number
  - parse-number
  - locale-number
  - international-number-conversion
tags:
  - text
  - conversion
  - numbers
  - locale
  - international
  - excel
  - google-sheets
---

# NUMBERVALUE

## Description

NUMBERVALUE is a text function that converts a text representation of a number to an actual numeric value, with explicit control over the decimal separator and group separator characters used in the text. Unlike the simpler VALUE function, which relies on the system's regional settings to interpret numbers, NUMBERVALUE allows you to specify exactly what characters represent the decimal point and thousands separator. This makes it invaluable for parsing numbers formatted according to different locale conventions, such as European formats that use comma as decimal separator and period as thousands separator (opposite of US convention).

NUMBERVALUE is essential when working with international data where number formats vary by country or region. In the United States, one thousand and a half is written as "1,000.5" but in Germany, France, and many other countries, the same number is written as "1.000,5" (comma for decimals, period for thousands). When such data is imported as text, standard conversion functions may fail or produce wrong results. NUMBERVALUE handles these conversions correctly by letting you specify which character is which. Common use cases include parsing imported financial data from international sources, converting web-scraped numbers, processing data from legacy systems with non-standard formats, and normalizing numbers from multi-locale datasets.

There are several important gotchas when using NUMBERVALUE. First, the function returns a #VALUE! error if the text contains invalid characters after accounting for the separators. Second, currency symbols and other non-numeric characters (except separators and minus signs) must be removed before using NUMBERVALUE. Third, if you specify the wrong separators for your data, results will be incorrect, typically off by factors of 1000 or returning errors. Fourth, the group_separator is optional and defaults to an empty string, meaning no thousands separator is expected. Fifth, spaces in the text may cause errors unless they match the group_separator specification. Sixth, negative numbers can use a leading minus sign but parenthetical negatives like "(100)" are not supported directly.

Regarding platform availability, NUMBERVALUE is available in Microsoft Excel (2013 and later) and Google Sheets. The syntax is identical on both platforms. In older Excel versions, complex workarounds using SUBSTITUTE and VALUE were required to achieve the same result. Both platforms support the same three-parameter syntax and handle the conversion identically. For maximum compatibility across Excel versions, consider providing VALUE-based alternatives for older workbooks.

## Syntax

> [!f(x)] NUMBERVALUE Syntax
>
> ```excel
> =NUMBERVALUE(text, [decimal_separator], [group_separator])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string representing a number to convert. Can be a literal string in quotes, a cell reference, or an expression that returns text. | Yes |
> | `decimal_separator` | The character used as the decimal point in the text. Default is the period (.) based on system settings. Common values: "." (US) or "," (European). | No |
> | `group_separator` | The character used as the thousands/grouping separator in the text. Default is empty (no group separator expected). Common values: "," (US) or "." (European) or " " (space). | No |

## Examples

### Example 1: Basic Conversion (No Separators)
```excel
=NUMBERVALUE("1234")
```
**Result:** `1234`

**Explanation:** Simple text-to-number conversion without any separators. Equivalent to VALUE() for plain numeric strings.

---

### Example 2: US Format with Comma Thousands
```excel
=NUMBERVALUE("1,234.56", ".", ",")
```
**Result:** `1234.56`

**Explanation:** Parses US-formatted number with period decimal and comma thousands separator. The commas are removed and the value is returned as a number.

---

### Example 3: European Format (Comma Decimal)
```excel
=NUMBERVALUE("1.234,56", ",", ".")
```
**Result:** `1234.56`

**Explanation:** Parses European-formatted number where comma is decimal and period is thousands separator. Common format in Germany, France, Italy, Spain, and many other countries.

---

### Example 4: Space as Group Separator
```excel
=NUMBERVALUE("1 234 567,89", ",", " ")
```
**Result:** `1234567.89`

**Explanation:** Some locales (including France and Russia) use space as thousands separator. The space group_separator handles this format correctly.

---

### Example 5: No Thousands Separator
```excel
=NUMBERVALUE("1234,56", ",")
```
**Result:** `1234.56`

**Explanation:** When there's no thousands separator in the source text, omit the third parameter or use "". Only the decimal separator needs specification.

---

### Example 6: Cell Reference with European Data
```excel
=NUMBERVALUE(A1, ",", ".")
```
Where A1 contains "12.345,67"

**Result:** `12345.67`

**Explanation:** NUMBERVALUE works with cell references. The formula converts the European-formatted text in A1 to a numeric value.

---

### Example 7: Negative Number
```excel
=NUMBERVALUE("-1.234,56", ",", ".")
```
**Result:** `-1234.56`

**Explanation:** Negative numbers with leading minus sign are handled correctly. The minus sign is preserved in the numeric result.

---

### Example 8: Percentage Conversion
```excel
=NUMBERVALUE(SUBSTITUTE("75,5%", "%", ""), ",")/100
```
**Result:** `0.755`

**Explanation:** Percentage symbols must be removed before NUMBERVALUE. SUBSTITUTE removes "%", then NUMBERVALUE converts, then divide by 100 for the decimal value.

---

### Example 9: Default Separators (US Locale)
```excel
=NUMBERVALUE("123.45")
```
**Result:** `123.45`

**Explanation:** With no separator parameters, NUMBERVALUE uses defaults based on system locale. In US systems, period is decimal separator.

---

### Example 10: Swiss Format (Apostrophe Separator)
```excel
=NUMBERVALUE("1'234'567.89", ".", "'")
```
**Result:** `1234567.89`

**Explanation:** Switzerland sometimes uses apostrophe as thousands separator. NUMBERVALUE handles any character as group separator.

---

### Example 11: Indian Lakhs/Crores Format
```excel
=NUMBERVALUE("12,34,567.89", ".", ",")
```
**Result:** `1234567.89`

**Explanation:** Indian number formatting groups differently (lakhs and crores), but since commas are still the separator, NUMBERVALUE removes them regardless of position.

---

### Example 12: Converting Scientific Notation
```excel
=NUMBERVALUE("1,23E+06", ",")
```
**Result:** `1230000`

**Explanation:** NUMBERVALUE can parse scientific notation with locale-specific decimal separators. The "E" notation is recognized.

---

### Example 13: Cleaning Currency Before Conversion
```excel
=NUMBERVALUE(SUBSTITUTE(SUBSTITUTE(A1, "€", ""), " ", ""), ",", ".")
```
Where A1 contains "€ 1.234,56"

**Result:** `1234.56`

**Explanation:** Currency symbols and extra spaces must be removed first using SUBSTITUTE, then NUMBERVALUE converts the cleaned text.

---

### Example 14: Array Formula for Column Conversion
```excel
=ARRAYFORMULA(NUMBERVALUE(A2:A100, ",", "."))
```
**Result:** Column of numeric values converted from European format

**Explanation:** In Google Sheets, ARRAYFORMULA applies NUMBERVALUE to an entire column, converting all values at once.

---

### Example 15: Error Handling with IFERROR
```excel
=IFERROR(NUMBERVALUE(A1, ",", "."), 0)
```
**Result:** Converted number or 0 if conversion fails

**Explanation:** Invalid text produces #VALUE! error. IFERROR provides a fallback value for robust formulas.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Text contains non-numeric characters (currency, letters) | Use SUBSTITUTE to remove currency symbols, letters, and invalid characters before conversion |
| `#VALUE!` | Wrong separator characters specified | Verify which character is decimal vs. thousands in source data; swap if needed |
| `#VALUE!` | Multiple decimal separators in text | Ensure only one decimal separator exists; may indicate data quality issue |
| `#VALUE!` | Empty text or cell | Check for empty values; use IF to handle blanks |
| `Wrong result` | Separators swapped (1.234 interpreted as 1234) | Double-check which character is decimal vs. group separator in source |
| `#NAME?` | Using in Excel 2010 or earlier | NUMBERVALUE requires Excel 2013+; use SUBSTITUTE+VALUE workaround |

## Use Cases

### [[International Financial Data Import]]
- **Implementation**: Parse financial data from international sources where number formats differ from local conventions. Create conversion formulas specific to each source country's format.
- **Business Application**: Global corporations importing financial statements, stock prices, or economic data from multiple countries need consistent numeric values regardless of source formatting.
- **Technical Details**: Document the format used by each data source. Create named formula patterns for common formats (US, European, Swiss, etc.). Consider creating a format lookup table.

### [[Web Data Parsing]]
- **Implementation**: Convert numbers scraped from websites or copied from web applications where formats may not match local settings.
- **Business Application**: Price monitoring systems, competitor analysis tools, and data aggregation from web sources need to parse numbers from various formatting conventions.
- **Technical Details**: Web data often includes currency symbols and extra formatting. Chain SUBSTITUTE calls to clean data, then apply NUMBERVALUE. Test with sample data from target sources.

### [[Legacy System Data Migration]]
- **Implementation**: Convert numbers from legacy systems that may use non-standard or locale-specific formatting during data migration projects.
- **Business Application**: ERP migrations, database consolidations, and system upgrades often involve data from older systems with different number formatting.
- **Technical Details**: Analyze legacy data to identify formatting patterns. Create validation rules to catch conversion errors. Build conversion audit trails.

### [[Multi-Locale Data Consolidation]]
- **Implementation**: Standardize numeric data from multiple regional offices or international subsidiaries that report in local formats.
- **Business Application**: Consolidating reports from European and American offices, merging datasets from international research studies, or combining data from multinational supply chains.
- **Technical Details**: Identify the source region for each dataset. Apply appropriate NUMBERVALUE parameters based on source. Validate converted totals against known control totals.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | Excel 2013+ | All versions |
| **Function Syntax** | Identical | Identical |
| **Default Decimal** | System locale | System locale |
| **Default Group Sep** | Empty string | Empty string |
| **Array Support** | With dynamic arrays (365) | With ARRAYFORMULA |
| **Error on Invalid** | #VALUE! | #VALUE! |
| **Scientific Notation** | Supported | Supported |

**Excel Pre-2013 Workaround:**

For Excel 2010 and earlier, use this pattern:
```excel
=VALUE(SUBSTITUTE(SUBSTITUTE(A1, ".", ""), ",", "."))
```
This converts European format (comma decimal) to US format, then VALUE converts to number.

**Key Notes:**

- NUMBERVALUE is fully compatible between modern Excel and Google Sheets
- Syntax and behavior are identical
- Both platforms handle the same edge cases similarly

## Tips and Best Practices

1. **Know Your Source Format**: Always verify whether the source data uses period or comma as decimal separator before writing formulas. Getting this wrong produces incorrect results without errors.

2. **Clean Data First**: Remove currency symbols, percentage signs, and any non-numeric characters using SUBSTITUTE before applying NUMBERVALUE. The function expects clean numeric text.

3. **Handle Empty Cells**: Wrap in IFERROR or IF to handle blank cells: `=IF(A1="", 0, NUMBERVALUE(A1, ",", "."))`.

4. **Document Format Assumptions**: Comment or document what format each NUMBERVALUE formula expects, especially in workbooks processing data from multiple sources.

5. **Validate Conversions**: Spot-check converted values against originals, especially for the first batch. A misspecified separator can cause subtle errors.

6. **Use Helper Columns**: When processing raw imported data, create a helper column for the conversion rather than modifying the source. This preserves the original for verification.

7. **Consider Regional Variations**: Even within "European format," there are variations (comma vs. period vs. space vs. apostrophe). Identify the specific source convention.

8. **Test with Edge Cases**: Include test values with zero, negative numbers, very large numbers, and numbers with no decimals to ensure complete coverage.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from NUMBERVALUE |
|----------|-------------|--------------------------------|
| [[VALUE]] | Converts text to number | Uses system locale; no separator control |
| [[N]] | Returns number from value | More limited; primarily for Excel compatibility |
| [[CONVERT]] | Unit conversion | Converts units, not text formats |
| [[TEXT]] | Number to formatted text | Opposite direction; numbers to text |

### Commonly Used Together

**[[SUBSTITUTE]]** - Clean text before conversion

*Remove currency and unwanted characters:*
```excel
=NUMBERVALUE(SUBSTITUTE(SUBSTITUTE(A1, "$", ""), ",", ""), ".")
```
Remove dollar sign and commas before converting.

**[[VALUE]]** - Simple conversion

*When locale matches system settings:*
```excel
=VALUE(A1)
```
Use VALUE when separators match system locale; NUMBERVALUE when they differ.

**[[IFERROR]]** - Handle conversion failures

*Provide fallback for invalid data:*
```excel
=IFERROR(NUMBERVALUE(A1, ",", "."), 0)
```
Return 0 (or another default) when conversion fails.

**[[TRIM]]** - Remove extra spaces

*Clean spaces before conversion:*
```excel
=NUMBERVALUE(TRIM(A1), ",", ".")
```
Remove leading/trailing spaces that might cause errors.

**[[TEXT]]** - Reverse operation

*Convert numbers back to formatted text:*
```excel
=TEXT(A1, "#,##0.00")
```
After NUMBERVALUE converts to number, TEXT can format for display.

**[[IF]]** - Conditional conversion

*Handle different formats:*
```excel
=IF(B1="Europe", NUMBERVALUE(A1, ",", "."), NUMBERVALUE(A1, ".", ","))
```
Apply different conversions based on source indicator.

## Official Documentation

- **Microsoft Excel**: [NUMBERVALUE function](https://support.microsoft.com/en-us/office/numbervalue-function-1b05c8cf-2bfa-4437-af70-596c7ea7d879)
- **Google Sheets**: [NUMBERVALUE function](https://support.google.com/docs/answer/3094291)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Initial release |
| Excel 2016 | 2016 | Performance improvements |
| Excel 365 | 2018+ | Dynamic array support |
| Google Sheets | Original | Available since early versions |

---

*Last updated: 2026-01-10*
