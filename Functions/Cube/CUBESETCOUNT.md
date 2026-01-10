---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- cube
- olap
subTopics:
- counting
- sets
- business-intelligence
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- count-set-members
- olap-count
---

# CUBESETCOUNT

## CUBESETCOUNT Description

The CUBESETCOUNT function returns the number of items in a set created by the CUBESET function, enabling dynamic counting of dimension members, filtered results, or custom groupings from OLAP cubes. This function is essential for building dynamic reports where the size of data sets may vary based on filters, time periods, or business conditions, and where knowing the count is necessary for display, validation, or loop control.

CUBESETCOUNT operates by taking a reference to a cell containing a CUBESET result and returning an integer representing the number of members in that set. The function performs a simple count operation that executes quickly regardless of the set's complexity, as the set has already been evaluated by the CUBESET function. This separation of concerns allows report designers to reference set counts multiple times without recalculating the underlying set.

The function is particularly valuable for creating dynamic user interfaces that adapt to data. For example, when filtering to show "Products with Sales > $1M," CUBESETCOUNT reveals how many products qualify, which can drive conditional formatting, display messages like "Showing 15 of 200 products," or control the number of rows displayed in a ranked list.

CUBESETCOUNT returns 0 if the referenced CUBESET evaluates to an empty set and returns an error if the referenced cell does not contain a valid CUBESET result. The function is exclusive to Excel and requires an existing CUBESET formula to count. It cannot directly evaluate MDX set expressions without a CUBESET intermediary.

## Syntax

```
=CUBESETCOUNT(set)
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| set | Yes | A cell reference to a CUBESET formula result. The referenced cell must contain a valid CUBESET; text strings or MDX expressions are not accepted. |

## Examples

```
=CUBESETCOUNT(A1)
// Returns: Number of members in the set defined in A1
// Basic count of CUBESET result

=CUBESETCOUNT(TopProducts)
// Returns: Count using named range "TopProducts"
// Named range pointing to CUBESET cell

="Showing "&CUBESETCOUNT(A1)&" products"
// Returns: "Showing 10 products"
// Building display text with count

=IF(CUBESETCOUNT(A1)=0, "No data found", CUBERANKEDMEMBER($B$1, A1, 1))
// Returns: First member or "No data found"
// Conditional logic based on set size

=IF(CUBESETCOUNT(A1)>100, "Too many results - please filter", "")
// Returns: Warning message or empty
// Validation for set size limits

=CUBESETCOUNT(CUBESET("SalesCube", "[Product].[Category].Members"))
// Returns: Count of categories
// Nested CUBESET (though cell reference is preferred)

=ROW()-ROW($A$1)+1<=CUBESETCOUNT($B$1)
// Returns: TRUE/FALSE for controlling visible rows
// Dynamic row visibility control

="Page "&A1&" of "&ROUNDUP(CUBESETCOUNT(B1)/10,0)
// Returns: "Page 1 of 5" (example)
// Pagination calculation

=CUBESETCOUNT(A1)/CUBESETCOUNT(B1)*100&"% of total"
// Returns: "25% of total" (example)
// Percentage calculations

=MIN(CUBESETCOUNT(A1), 10)
// Returns: Smaller of set count or 10
// Capping display to maximum rows
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | Referenced cell does not contain a CUBESET | Ensure the reference points to a cell with CUBESET formula |
| #VALUE! | Parameter is text or number instead of cell reference | Use cell reference to CUBESET, not text strings |
| #REF! | Referenced cell was deleted or reference is invalid | Update reference to valid CUBESET cell |
| 0 (unexpected) | CUBESET returned empty set | Verify CUBESET filter logic; empty results return count of 0 |
| #GETTING_DATA | Underlying CUBESET still calculating | Wait for CUBESET to complete; count cannot proceed until set is ready |
| #NAME? | Referenced named range not found | Check spelling of named range |

## Use Cases

### Dynamic Report Row Control
**Business Context**: Creating reports that automatically expand or contract based on the data, showing exactly the number of rows needed for the current dataset without manual adjustment.

**Implementation**: Use CUBESETCOUNT to determine how many CUBERANKEDMEMBER formulas to populate, or to control conditional visibility of rows.

**Example Formula**:
```
=IF(ROW()-ROW($B$2)+1<=CUBESETCOUNT($A$1), CUBERANKEDMEMBER("SalesCube", $A$1, ROW()-ROW($B$2)+1), "")
```

**Technical Details**: This formula displays members from the set sequentially, row by row, and returns empty for rows beyond the set size. Combined with conditional formatting to hide empty rows, this creates dynamic-length reports.

### Filter Result Feedback
**Business Context**: Providing users with feedback about their filter selections, showing how many items match their criteria before displaying the full results.

**Implementation**: Display the count from filtered CUBESETs in dashboard summary cells, enabling users to understand the scope of their analysis.

**Example Formula**:
```
="Your filters match "&CUBESETCOUNT(A2)&" of "&CUBESETCOUNT(A1)&" total customers"
```

**Technical Details**: A1 contains a CUBESET of all customers; A2 contains a filtered subset. This shows "Your filters match 45 of 500 total customers," helping users understand filter effectiveness.

### Pagination and Batch Processing
**Business Context**: Implementing paginated displays for large datasets or controlling batch processing logic in complex reports with many potential results.

**Implementation**: Calculate total pages, current page range, and next/previous page logic using CUBESETCOUNT as the denominator.

**Example Formula**:
```
=ROUNDUP(CUBESETCOUNT(ProductSet)/PageSize, 0)
```

**Technical Details**: With ProductSet containing 237 items and PageSize defined as 25, this returns 10 (total pages). Combined with page navigation controls, this enables paginated OLAP reporting in Excel.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2007 and later | Not available |
| Parameter type | Cell reference to CUBESET required | Not applicable |
| Nested CUBESET | Technically supported but not recommended | Not applicable |
| Maximum count | Limited by OLAP server set size limits | Not applicable |
| Performance | Fast (counts already-calculated set) | Not applicable |
| Empty set handling | Returns 0 | Not applicable |
| Error propagation | Returns error if CUBESET errors | Not applicable |
| Excel Online | Limited functionality | Not applicable |

## Tips

1. **Cell reference required**: CUBESETCOUNT requires a cell reference to a CUBESET, not an MDX expression. Store your CUBESET in a cell first, then reference it.

2. **Performance**: Since CUBESETCOUNT only counts an already-calculated set, it is extremely fast. Multiple CUBESETCOUNT calls to the same CUBESET add minimal overhead.

3. **Empty set detection**: Use `=IF(CUBESETCOUNT(A1)=0, "No results", ...)` to handle empty filter results gracefully in user-facing reports.

4. **Named ranges**: Consider naming CUBESET cells (e.g., "TopProducts") for clearer CUBESETCOUNT formulas: `=CUBESETCOUNT(TopProducts)`.

5. **Dynamic ranges**: Combine with INDEX to create dynamic named ranges that adjust to set size: `=INDEX($B$2:$B$1000, CUBESETCOUNT($A$1))` for the last row.

## Related Functions

- [[CUBESET]] - Creates the set that CUBESETCOUNT counts
- [[CUBERANKEDMEMBER]] - Returns individual members from the counted set
- [[CUBEVALUE]] - Retrieves values; can aggregate across sets
- [[CUBEMEMBER]] - Returns single members (use CUBESETCOUNT for multiple)
- [[COUNT]] - Standard Excel count (does not work with CUBESET)

## Official Documentation

- [Microsoft Excel CUBESETCOUNT Function](https://support.microsoft.com/en-us/office/cubesetcount-function-c4c2a438-c1ff-4061-80fe-982f2d705286)
- [Excel Cube Functions Overview](https://support.microsoft.com/en-us/office/cube-functions-reference-2378132b-d3f2-4af1-896d-48a9ee840eb2)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2007 | 2007 | Initial release |
| Excel 2010 | 2010 | PowerPivot integration |
| Excel 2013 | 2013 | Data Model support |
| Excel 2016 | 2016 | Power BI connectivity |
| Excel 2019 | 2019 | No significant changes |
| Excel 365 | 2020+ | Continuous improvements |
| Google Sheets | N/A | Not available - Excel-only function |
