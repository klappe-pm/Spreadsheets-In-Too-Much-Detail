---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
- number-conversion
subTopics:
- numeral-systems
- text-conversion
- formatting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- roman-numerals
- arabic-to-roman
tags:
- roman-numerals
- conversion
- text
- mathematical
- formatting
---

# ROMAN

## Description

ROMAN converts an Arabic numeral (standard number) to Roman numeral format, returned as text. For example, ROMAN(499) returns "CDXCIX" in classic form or more concise variants depending on the form parameter. Roman numerals use letters (I, V, X, L, C, D, M) to represent values, with specific rules for subtractive notation (like IV for 4 instead of IIII).

This function is useful for generating traditional numbering in documents, creating outline formats, page numbering in prefaces, copyright year displays, and academic or formal document styling. While Roman numerals aren't used for calculations, they remain standard in many contexts including movie sequel numbering (Rocky IV), Super Bowl designations (Super Bowl LVIII), clock faces, and formal document numbering.

A critical gotcha is that ROMAN only works with positive integers from 1 to 3999. Zero and negative numbers return errors because Roman numerals have no representation for zero or negative values. The function returns text, not a number, so ROMAN(5) + ROMAN(10) would cause an error (can't add text). The form parameter controls how concise the output is - classic form uses standard subtractive notation, while more concise forms use non-standard but shorter representations.

Both Excel and Google Sheets support ROMAN, but with a key difference: Excel offers 5 form options (0-4) for varying conciseness, while Google Sheets only supports classic form (form parameter is ignored). The function is case-sensitive in its output - all letters are uppercase.

## Syntax

> [!f(x)] ROMAN Syntax
>
> ```excel
> ROMAN(number, [form])
> ```
>
> Converts an Arabic numeral to Roman numeral as text.

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| number | The Arabic numeral to convert (1 to 3999) | Yes | - |
| form | Conciseness level: 0 or omitted = Classic, 1-3 = increasingly concise, 4 = most simplified | No | 0 (Classic) |

### Form Values (Excel Only)

| Form | Description | Example: ROMAN(499, form) |
|------|-------------|---------------------------|
| 0 or TRUE | Classic | CDXCIX |
| 1 | More concise | LDVLIV |
| 2 | Even more concise | XDIX |
| 3 | Very concise | VDIV |
| 4 or FALSE | Simplified | ID |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=ROMAN(1)` | I | One in Roman numerals |
| `=ROMAN(4)` | IV | Four (5-1 subtractive notation) |
| `=ROMAN(9)` | IX | Nine (10-1) |
| `=ROMAN(49)` | XLIX | 40+9 = XL+IX |
| `=ROMAN(99)` | XCIX | 90+9 = XC+IX |
| `=ROMAN(500)` | D | Five hundred |
| `=ROMAN(1000)` | M | One thousand |
| `=ROMAN(2024)` | MMXXIV | Year 2024 |
| `=ROMAN(3999)` | MMMCMXCIX | Maximum value |
| `=ROMAN(499,0)` | CDXCIX | Classic form |
| `=ROMAN(499,4)` | ID | Simplified (Excel only) |
| `=ROMAN(1776)` | MDCCLXXVI | US Declaration year |
| `="Chapter "&ROMAN(A1)` | Chapter IV | Combined with text for headings |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Number is less than 1 | Use a positive integer 1 or greater |
| #VALUE! | Number is greater than 3999 | Roman numerals max out at 3999 |
| #VALUE! | Number is not numeric | Provide a valid number |
| #VALUE! | Form is invalid (not 0-4 or TRUE/FALSE) | Use valid form values |

## Use Cases

### [[Document Formatting - Section Numbering]]
- **Implementation**: Create Roman numeral headings for formal documents
- **Business Application**: Legal documents, academic papers, book prefaces, and formal reports
- **Technical Details**: `="Section "&ROMAN(ROW()-StartRow+1)&": "&SectionTitle` generates "Section IV: Introduction" style headings

### [[Copyright and Year Display]]
- **Implementation**: Display years in Roman numeral format for traditional styling
- **Business Application**: Movie credits, building cornerstones, legal copyright notices
- **Technical Details**: `="Copyright "&ROMAN(YEAR(TODAY()))` produces "Copyright MMXXIV" for 2024; note updates automatically

### [[Outline and List Generation]]
- **Implementation**: Create Roman numeral outline levels for hierarchical documents
- **Business Application**: Legal briefs, academic outlines, formal meeting agendas
- **Technical Details**: `=ROMAN(RowNumber)&". "&ItemText` creates "IV. Methodology" style outlines; combine with ARABIC for roundtrip conversion

### [[Event and Edition Numbering]]
- **Implementation**: Generate Roman numerals for sequential events or editions
- **Business Application**: Super Bowl numbering, Olympics (XXX Games), sequel numbering, anniversary events
- **Technical Details**: `=EventName&" "&ROMAN(EditionNumber)` creates "Super Bowl LVIII" or "Annual Report Volume XII"

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| ROMAN function | Yes (all versions) | Yes |
| Form parameter | 5 options (0-4) | Ignored (classic only) |
| Boolean form values | TRUE=0, FALSE=4 | Ignored |
| Maximum value | 3999 | 3999 |
| Output case | Uppercase only | Uppercase only |
| Array formula support | Yes | Yes |

## Tips and Best Practices

1. **Remember the range limit** - ROMAN only works for 1-3999; larger values require custom solutions or concatenation
2. **Use ARABIC for reverse conversion** - ARABIC(ROMAN(n)) = n; useful for validation
3. **Form parameter is Excel-only** - Google Sheets always uses classic form regardless of parameter
4. **Combine with text functions** - ROMAN returns text, so use & or CONCATENATE for headings
5. **Classic form is standard** - For formal documents, use form 0 (classic) for universally recognized notation
6. **Create outline formulas** - Use ROW()-based formulas for automatic Roman numeral sequences
7. **Consider readability** - Non-classic forms (1-4) may not be recognized by all readers

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[ARABIC]] | Converts Roman to Arabic | Reverse operation; ARABIC("IV") = 4 |
| [[TEXT]] | Formats numbers as text | General formatting; no Roman numeral support |
| [[BASE]] | Converts to other bases | Different numeral systems (2-36) |
| [[DECIMAL]] | Converts from other bases | Reverse of BASE function |
| [[VALUE]] | Converts text to number | Can't convert Roman numerals |

### Commonly Used Together

- **[[ARABIC]]** - Convert back to Arabic: ARABIC(ROMAN(n)) = n for validation
- **[[CONCATENATE]]** / **&** - Combine Roman numerals with text for headings
- **[[ROW]]** - Generate sequential Roman numerals based on row position
- **[[YEAR]]** - Extract year for Roman numeral copyright notices
- **[[TEXT]]** - Additional formatting when combining with Roman output
- **[[IF]]** - Conditional formatting between Roman and Arabic numerals

## Official Documentation

- [Microsoft: ROMAN function](https://support.microsoft.com/en-us/office/roman-function-d6b0b99e-de46-4704-a518-b45a0f8b56f5)
- [Google: ROMAN function](https://support.google.com/docs/answer/3093318)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 5.0 | 1993 | Function introduced |
| Excel 2007 | 2006-11 | Form parameter behavior standardized |
| Excel 365 | Ongoing | Full support maintained |
| Google Sheets | 2006 | Full support from launch (classic form only) |
