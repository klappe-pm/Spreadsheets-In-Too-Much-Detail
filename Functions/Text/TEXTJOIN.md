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
- arrays
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- join-text
- concatenate-delimiter
---

# TEXTJOIN

## TEXTJOIN Description

The TEXTJOIN function concatenates text strings from multiple cells or ranges, inserting a specified delimiter between each value. This powerful function solves the common problem of combining lists of values into a single delimited string, such as creating comma-separated lists, building file paths, or generating formatted output from range data. TEXTJOIN offers significant advantages over older functions like CONCATENATE by accepting ranges directly and providing built-in handling for empty cells.

TEXTJOIN's second parameter determines how empty cells are handled during concatenation. When set to TRUE, empty cells are ignored and do not contribute delimiters to the output. When FALSE, empty cells are included, resulting in consecutive delimiters where blanks exist. This control eliminates the need for complex IF statements or FILTER functions to clean data before concatenation, streamlining formula development considerably.

The function accepts multiple arguments after the first two parameters, each of which can be individual cell references, ranges, arrays, or text strings. This flexibility allows TEXTJOIN to combine data from non-contiguous ranges, mix hardcoded text with cell references, and process entire columns or rows in a single formula. The maximum result length is limited by cell capacity (32,767 characters in Excel, 50,000 in Google Sheets).

TEXTJOIN was introduced in Excel 2019 and Excel 365, and has been available in Google Sheets since its early days. For organizations using older Excel versions, TEXTJOIN is not available natively, requiring workarounds using CONCATENATE, ampersand operators, or VBA. When available, TEXTJOIN dramatically simplifies formulas that previously required complex nested structures or array formulas to achieve the same result.

## Syntax

```
=TEXTJOIN(delimiter, ignore_empty, text1, [text2], ...)
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| delimiter | Yes | The text string to insert between each joined value. Can be a single character like "," or multiple characters like ", " or even an empty string "". |
| ignore_empty | Yes | Logical value: TRUE ignores empty cells; FALSE includes empty cells (causing consecutive delimiters). |
| text1 | Yes | The first text item, cell reference, or range to join. |
| text2, ... | No | Additional text items, cell references, or ranges to join. Up to 252 arguments supported. |

## Examples

```
=TEXTJOIN(", ", TRUE, A1:A5)
// Returns: "Apple, Banana, Cherry" (if range contains these values)
// Comma-separated list ignoring empty cells

=TEXTJOIN(", ", FALSE, A1:A5)
// Returns: "Apple, , Banana, , Cherry" (if cells 2 and 4 are empty)
// Includes empty cells as consecutive delimiters

=TEXTJOIN("-", TRUE, "2024", "01", "15")
// Returns: "2024-01-15"
// Building a date string from components

=TEXTJOIN(CHAR(10), TRUE, A1:A10)
// Returns: Multi-line text with each value on new line
// Using line break as delimiter

=TEXTJOIN("; ", TRUE, A1:A5, B1:B5)
// Returns: Combined values from both ranges
// Joining multiple ranges

=TEXTJOIN("/", TRUE, "C:", "Users", A1, "Documents")
// Returns: "C:/Users/John/Documents" (if A1="John")
// Building file paths with mixed inputs

=TEXTJOIN(", ", TRUE, FILTER(A1:A10, B1:B10="Yes"))
// Returns: Filtered values joined
// Combined with FILTER for conditional joining

=TEXTJOIN(" and ", TRUE, A1:A3)
// Returns: "Red and Blue and Green"
// Using word delimiters

=TEXTJOIN("", TRUE, A1:D1)
// Returns: "ABCD" (if cells contain A, B, C, D)
// Empty delimiter for direct concatenation

=TEXTJOIN(", ", TRUE, IF(B1:B10>100, A1:A10, ""))
// Returns: Names where corresponding value exceeds 100
// Conditional text joining with IF
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Result exceeds maximum cell length (32,767 characters) | Reduce input range or split into multiple TEXTJOIN calls |
| #NAME? | Function not available (Excel 2016 or earlier) | Upgrade Excel or use CONCATENATE workaround |
| #VALUE! | Invalid delimiter type | Ensure delimiter is text; numbers are auto-converted but other types may fail |
| Missing values | ignore_empty=TRUE filtering unexpectedly | Verify empty cells are truly empty, not containing spaces or formulas returning "" |
| Unexpected delimiters | ignore_empty=FALSE with empty cells | Change to TRUE to skip empty cells |
| #CALC! | Circular reference or calculation issue | Check for formulas referencing themselves |

## Use Cases

### Creating Comma-Separated Lists
**Business Context**: Generating comma-separated lists from columns of data for reports, exports, email lists, or display purposes is a common spreadsheet task.

**Implementation**: Use TEXTJOIN to convert a column of values into a single cell containing all values separated by commas.

**Example Formula**:
```
=TEXTJOIN(", ", TRUE, A2:A100)
```

**Technical Details**: This creates a list like "John, Jane, Bob, Alice" from a column of names. The TRUE parameter ensures blank cells do not create double commas. Useful for creating email recipient lists, tag lists, or summary displays.

### Building Dynamic File Paths or URLs
**Business Context**: Constructing file paths, URLs, or reference codes from multiple components stored in separate cells requires reliable concatenation with proper separators.

**Implementation**: Use TEXTJOIN with path separators to build complete paths from directory and filename components.

**Example Formula**:
```
=TEXTJOIN("/", TRUE, "https://example.com", A2, B2, C2&".html")
```

**Technical Details**: Given values "products", "category", "item", this produces "https://example.com/products/category/item.html". The TRUE parameter handles any missing path segments gracefully without double slashes.

### Conditional Data Aggregation
**Business Context**: Creating summary lists that only include values meeting certain criteria, such as listing only overdue items or high-priority tasks.

**Implementation**: Combine TEXTJOIN with FILTER to create conditional lists without helper columns.

**Example Formula**:
```
=TEXTJOIN(CHAR(10), TRUE, FILTER(A2:A50, B2:B50="Urgent"))
```

**Technical Details**: This creates a line-break-separated list of items from column A where column B equals "Urgent". The CHAR(10) creates line breaks for vertical display within a single cell. Requires Excel 365 or Google Sheets for FILTER function.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2019, 365, and later | Available since early versions |
| Maximum result length | 32,767 characters | 50,000 characters |
| Maximum arguments | 252 | 30 (but ranges can contain unlimited cells) |
| Array handling | Full array support | Full array support |
| FILTER integration | Supported (365) | Fully supported |
| Empty string delimiter | Supported | Supported |
| Multi-character delimiter | Supported | Supported |
| Performance with large ranges | Optimized | Generally fast |

## Tips

1. **Multi-line output**: Use `CHAR(10)` as delimiter for line breaks within a cell. Enable "Wrap text" formatting to see each value on its own line.

2. **Versus CONCAT**: TEXTJOIN offers delimiter and empty-handling features that CONCAT lacks. Use CONCAT only for simple no-delimiter joining.

3. **Remove duplicates before joining**: Combine with UNIQUE: `=TEXTJOIN(", ", TRUE, UNIQUE(A1:A100))` creates a list without duplicate values.

4. **Empty string delimiter**: `TEXTJOIN("", TRUE, A1:D1)` is equivalent to CONCAT but with empty-cell handling.

5. **Backward compatibility**: For Excel 2016 and earlier, this pattern approximates TEXTJOIN: `=SUBSTITUTE(TRIM(SUBSTITUTE(SUBSTITUTE(A1&" "&A2&" "&A3,"  "," "),"  "," "))," ",", ")` though it is complex and limited.

## Related Functions

- [[CONCAT]] - Simple concatenation without delimiter (Excel 2016+)
- [[CONCATENATE]] - Legacy concatenation function (all versions)
- [[JOIN]] - Google Sheets function similar to TEXTJOIN
- [[FILTER]] - Filter data before joining
- [[UNIQUE]] - Remove duplicates before joining
- [[SPLIT]] - Opposite operation: splits text by delimiter
- [[CHAR]] - Generate delimiter characters (CHAR(10) for line break)

## Official Documentation

- [Microsoft Excel TEXTJOIN Function](https://support.microsoft.com/en-us/office/textjoin-function-357b449a-ec91-49d0-80c9-c15a0a00bf82)
- [Google Sheets TEXTJOIN Function](https://support.google.com/docs/answer/7013992)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2016 | 2016 | Not available |
| Excel 2019 | 2019 | Initial release |
| Excel 365 | 2019+ | Full support with dynamic arrays |
| Excel for Mac | 2019 | Added support |
| Excel Online | 2019 | Added support |
| Google Sheets | 2016 | Available with full functionality |
