---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- concatenation
- joining
- strings
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- concatenate-modern
- join-strings
---

# CONCAT

## CONCAT Description

The CONCAT function joins multiple text strings, cell references, or ranges into a single continuous text string without any delimiter between values. Introduced in Excel 2016 as a modern replacement for CONCATENATE, CONCAT offers significant improvements including the ability to accept range references directly, making it far more practical for combining data from multiple cells without listing each cell individually.

CONCAT processes its arguments in order, appending each value to the result string. When a range is provided, CONCAT reads the cells from left to right, top to bottom (row by row), joining all values into one continuous string. Numbers, dates, and other non-text values are automatically converted to their text representations. Empty cells are included as empty strings, meaning they contribute nothing to the output but also do not cause errors.

The key advantage of CONCAT over the legacy CONCATENATE function is range support. While CONCATENATE requires explicit listing of each cell (=CONCATENATE(A1, A2, A3, A4, A5)), CONCAT accepts entire ranges (=CONCAT(A1:A5)), dramatically simplifying formulas that combine many cells. This makes CONCAT particularly valuable for tasks like joining columns of data, combining text fragments, or building identifiers from multiple components.

CONCAT does not insert any separator between joined values - for delimiter-separated output, use TEXTJOIN instead. CONCAT is supported in Excel 2016 and later, Excel 365, Excel Online, and Google Sheets. Users of Excel 2013 or earlier must use CONCATENATE or the ampersand (&) operator as alternatives, though these lack the range reference capability.

## Syntax

```
=CONCAT(text1, [text2], ...)
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| text1 | Yes | The first text string, cell reference, or range to concatenate. |
| text2, ... | No | Additional text strings, cell references, or ranges to concatenate. Up to 253 arguments in Excel, fewer in some versions. |

## Examples

```
=CONCAT("Hello", " ", "World")
// Returns: "Hello World"
// Basic text concatenation

=CONCAT(A1:A5)
// Returns: All values in A1:A5 joined without separator
// Range concatenation - CONCAT's key feature

=CONCAT(A1, B1, C1)
// Returns: Values of A1, B1, C1 joined
// Individual cell references

=CONCAT("ID-", A1, "-", B1)
// Returns: "ID-123-ABC" (example)
// Building identifiers with mixed text and cells

=CONCAT(A1:C1, "-", D1:F1)
// Returns: Values from both ranges with hyphen between
// Multiple ranges with separator

=CONCAT(A1:A3, B1:B3)
// Returns: A1A2A3B1B2B3 (values without separators)
// Multiple ranges processed in order

=CONCAT(TEXT(A1, "0000"), TEXT(B1, "00"))
// Returns: "0123" + "05" = "012305"
// Formatted numbers concatenated

=CONCAT(UPPER(A1), " - ", LOWER(B1))
// Returns: "HELLO - world" (example)
// Nested functions within CONCAT

=CONCAT(IF(A1>0, A1, "N/A"), " units")
// Returns: "500 units" or "N/A units"
// Conditional values in concatenation

=CONCAT(A1:A100)
// Returns: All 100 values joined
// Large range concatenation
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #NAME? | Function not available in Excel 2013 or earlier | Use CONCATENATE or upgrade Excel version |
| #VALUE! | Result exceeds 32,767 characters | Split concatenation or use shorter input |
| Unexpected spacing | Empty cells contributing nothing visible | This is correct behavior; use TEXTJOIN for delimited output |
| Numbers display oddly | Number formatting lost in conversion | Use TEXT() to preserve formatting before CONCAT |
| Missing values | Empty cells included as empty strings | This is expected; no visible contribution from empty cells |
| #CALC! | Circular reference | Check for self-referencing formulas |

## Use Cases

### Building Compound Identifiers
**Business Context**: Creating unique identifiers by combining multiple data fields like department codes, year, and sequence numbers into standardized ID formats.

**Implementation**: Use CONCAT to assemble identifier components with proper formatting, ensuring consistent ID structure across records.

**Example Formula**:
```
=CONCAT(A2, "-", TEXT(YEAR(B2), "0000"), "-", TEXT(C2, "00000"))
```

**Technical Details**: Given department "SALES", date in B2, and sequence 42 in C2, this produces "SALES-2024-00042". The TEXT functions ensure consistent digit lengths for sortable, standardized identifiers.

### Full Name Assembly
**Business Context**: Combining first name, middle name, and last name fields into complete name strings for reports, labels, or communication.

**Implementation**: Use CONCAT with space separators to build complete names, handling cases where middle names may be absent.

**Example Formula**:
```
=TRIM(CONCAT(A2, " ", B2, " ", C2))
```

**Technical Details**: TRIM removes extra spaces if middle name (B2) is empty. This produces "John Smith" when middle name is blank, or "John Michael Smith" when present. The TRIM handles the double-space that would otherwise occur.

### Address Line Construction
**Business Context**: Creating single-line or formatted address strings from separate address component fields for mailing labels or display purposes.

**Implementation**: Combine address fields with appropriate separators and line breaks for complete address formatting.

**Example Formula**:
```
=CONCAT(A2, CHAR(10), B2, ", ", C2, " ", D2)
```

**Technical Details**: Creates a two-line address: "123 Main Street" on line one, "New York, NY 10001" on line two. The CHAR(10) inserts a line break; enable "Wrap text" cell formatting to display properly.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2016 and later | All versions |
| Range support | Full range support | Full range support |
| Maximum arguments | 253 text arguments | Large number supported |
| Maximum result length | 32,767 characters | 50,000 characters |
| Array formula support | Native support | Automatic array expansion |
| Empty cell handling | Treated as empty string | Treated as empty string |
| Number conversion | Automatic to text | Automatic to text |
| Date conversion | Converts to serial number text | Converts to serial number text |

## Tips

1. **Versus ampersand (&)**: `=A1&B1&C1` is equivalent to `=CONCAT(A1,B1,C1)` for individual cells, but CONCAT handles ranges while & does not.

2. **Preserve number formats**: Wrap numbers in TEXT() to preserve formatting: `=CONCAT(TEXT(A1,"$#,##0.00"))` keeps currency format.

3. **Add separators manually**: For delimited output, either add separators explicitly `=CONCAT(A1,", ",B1,", ",C1)` or use TEXTJOIN instead.

4. **Date handling**: Dates concatenate as serial numbers. Use `=CONCAT(TEXT(A1,"MM/DD/YYYY"))` to preserve readable date format.

5. **Performance**: CONCAT with large ranges is efficient, but extremely long results may slow recalculation. Consider limiting range size if performance is an issue.

## Related Functions

- [[TEXTJOIN]] - Concatenates with delimiter and empty-cell control
- [[CONCATENATE]] - Legacy version without range support
- [[&]] - Ampersand operator for simple two-value joining
- [[TEXT]] - Format numbers/dates before concatenation
- [[REPT]] - Repeat text multiple times
- [[TRIM]] - Clean up extra spaces after concatenation

## Official Documentation

- [Microsoft Excel CONCAT Function](https://support.microsoft.com/en-us/office/concat-function-9b1a9a3f-94ff-41af-9736-694cbd6b4ca2)
- [Google Sheets CONCAT Function](https://support.google.com/docs/answer/3094123)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Not available (use CONCATENATE) |
| Excel 2016 | 2016 | Initial release with range support |
| Excel 2019 | 2019 | No significant changes |
| Excel 365 | 2019+ | Dynamic array integration |
| Excel Online | 2016 | Full support |
| Excel for Mac | 2016 | Full support |
| Google Sheets | 2006 | Available since launch |
