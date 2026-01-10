---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- cube
- olap
subTopics:
- ranking
- iteration
- business-intelligence
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ranked-member
- nth-member
---

# CUBERANKEDMEMBER

## CUBERANKEDMEMBER Description

The CUBERANKEDMEMBER function returns the nth member from a set created by CUBESET, enabling extraction of individual members for display in ranked lists, leaderboards, and iterative reports. This function is essential for building Top N reports, sequential member listings, and any scenario where specific members from a calculated set need to be displayed individually in separate cells.

CUBERANKEDMEMBER works in conjunction with CUBESET, where CUBESET defines the complete set of members (potentially sorted) and CUBERANKEDMEMBER extracts members by their position within that set. The rank parameter specifies which member to return: 1 returns the first member, 2 returns the second, and so on. This positional access allows report designers to create structured layouts with each rank in its own row or column.

The function returns the member's caption by default, similar to CUBEMEMBER. The returned value also carries the member's underlying MDX reference, making it usable as input for CUBEVALUE formulas. This enables complete report construction where CUBERANKEDMEMBER provides row/column headers and CUBEVALUE provides the corresponding data values.

CUBERANKEDMEMBER is exclusive to Excel and requires an OLAP connection. When the requested rank exceeds the number of members in the set, the function returns #N/A, which can be used to control the visible extent of dynamic reports. Combined with CUBESETCOUNT, developers can create robust reports that handle variable-length result sets gracefully.

## Syntax

```
=CUBERANKEDMEMBER(connection, set_expression, rank, [caption])
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| connection | Yes | Text string specifying the OLAP connection name. |
| set_expression | Yes | A cell reference to a CUBESET formula, or an MDX set expression (cell reference preferred). |
| rank | Yes | The position of the member to return from the set. 1 = first member, 2 = second member, etc. |
| caption | No | Optional text to display instead of the member's default caption. |

## Examples

```
=CUBERANKEDMEMBER("SalesCube", A1, 1)
// Returns: First member of the set in A1
// Extract top-ranked member

=CUBERANKEDMEMBER("SalesCube", TopProducts, 1)
// Returns: First member using named range
// Named range pointing to CUBESET

=CUBERANKEDMEMBER("SalesCube", A1, ROW()-ROW($A$1)+1)
// Returns: Sequential members as formula is copied down
// Dynamic rank based on row position

=CUBERANKEDMEMBER("SalesCube", $B$1, ROW()-1, "Rank "&(ROW()-1))
// Returns: "Rank 1", "Rank 2", etc.
// Custom caption showing rank number

=CUBERANKEDMEMBER("SalesCube", "[Product].[Category].Members", 3)
// Returns: Third product category
// Direct MDX set expression (less common)

=IFERROR(CUBERANKEDMEMBER("SalesCube", A1, B1), "")
// Returns: Member at rank specified in B1, or empty if beyond set
// Error handling for dynamic rank values

=CUBERANKEDMEMBER("SalesCube", CUBESET("SalesCube", "TopCount([Product].Members, 5, [Measures].[Sales])"), 1)
// Returns: Top-selling product
// Nested CUBESET (cell reference preferred for reuse)

=IF(ROW()<=CUBESETCOUNT($A$1), CUBERANKEDMEMBER("SalesCube", $A$1, ROW()), "")
// Returns: Members until set exhausted, then empty
// Controlled iteration with count check

=CUBERANKEDMEMBER("SalesCube", TopRegions, MATCH(TRUE, ISBLANK($A$2:$A$20), 0))
// Returns: Next empty position's member
// Finding position for new entry

=CUBERANKEDMEMBER("SalesCube", A1, RANDBETWEEN(1, CUBESETCOUNT(A1)))
// Returns: Random member from set
// Random selection from CUBESET
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | Rank exceeds number of members in set | Use IFERROR or check against CUBESETCOUNT before calling |
| #N/A | Referenced CUBESET is empty | Verify CUBESET logic returns members; handle empty sets explicitly |
| #VALUE! | Rank is not a positive integer | Ensure rank is a whole number >= 1 |
| #VALUE! | Invalid set_expression | Use cell reference to CUBESET or valid MDX set expression |
| #NAME? | Connection name not found | Verify connection name matches Data Connections |
| #GETTING_DATA | Set still calculating | Wait for CUBESET to complete before ranked member can be determined |

## Use Cases

### Top N Leaderboard Display
**Business Context**: Creating sales leaderboards, top customer lists, or best-performing product rankings that display individual members with their corresponding metrics.

**Implementation**: Use CUBESET to define and sort the ranked list, then CUBERANKEDMEMBER to extract each position for display in a formatted table.

**Example Formula**:
```
=CUBERANKEDMEMBER("SalesCube", $A$1, ROW()-1)
```

**Technical Details**: With a sorted CUBESET in A1 containing top 10 salespeople, this formula in rows 2-11 displays each salesperson in rank order. Combine with CUBEVALUE in adjacent columns to show their sales figures, quotas, and achievements.

### Dynamic Report Row Population
**Business Context**: Building reports that automatically adjust to show all qualifying members, whether that is 5 items or 50, without manual row management.

**Implementation**: Create a report template with enough rows for the maximum expected results, using CUBERANKEDMEMBER with row-based rank calculations and conditional hiding of empty rows.

**Example Formula**:
```
=IF(ROW()-ROW($B$2)+1<=CUBESETCOUNT($A$1), CUBERANKEDMEMBER("SalesCube", $A$1, ROW()-ROW($B$2)+1), "")
```

**Technical Details**: This populates members sequentially and returns empty strings beyond the set size. Apply conditional formatting to hide or gray out rows where the formula returns empty, creating clean variable-length reports.

### Comparative Period Analysis
**Business Context**: Displaying the same ranked position across multiple time periods to track how rankings change over time (e.g., "Who was #1 in Q1 vs Q2 vs Q3?").

**Implementation**: Create separate CUBESETs for each period, then extract the same rank position from each for side-by-side comparison.

**Example Formula**:
```
=CUBERANKEDMEMBER("SalesCube", B$1, $A2)
```

**Technical Details**: With period CUBESETs across row 1 (Q1, Q2, Q3, Q4) and rank numbers in column A (1, 2, 3...), this formula creates a matrix showing who held each rank in each period. Conditional formatting can highlight rank changes.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2007 and later | Not available |
| Set expression type | Cell reference or MDX set | Not applicable |
| Custom caption | Supported via fourth parameter | Not applicable |
| Maximum rank | Limited by set size from OLAP server | Not applicable |
| Error on exceed | Returns #N/A when rank > set size | Not applicable |
| Performance | Fast extraction from cached set | Not applicable |
| Combined with CUBEVALUE | Full support for member values | Not applicable |
| Excel Online | Limited functionality | Not applicable |

## Tips

1. **Row-based rank pattern**: The formula `ROW()-ROW($StartCell)+1` creates sequential rank numbers as the formula copies down. This is the standard pattern for ranked lists.

2. **Empty row handling**: Always wrap in `=IF(ROW()<=CUBESETCOUNT(...)+StartRow, CUBERANKEDMEMBER(...), "")` to prevent #N/A errors and enable clean formatting.

3. **Caption for display**: While the returned caption is usually fine, custom captions can add context: `=CUBERANKEDMEMBER(..., 1, "Gold Medal")` for first place.

4. **Reuse sets**: Always reference CUBESET cells rather than embedding CUBESET expressions in CUBERANKEDMEMBER. This prevents redundant server queries and improves maintainability.

5. **Descending rank**: CUBESET's sort_order parameter controls order. For "bottom 10," use ascending sort in CUBESET; CUBERANKEDMEMBER rank 1 will then be the smallest.

## Related Functions

- [[CUBESET]] - Creates the set from which ranked members are extracted
- [[CUBESETCOUNT]] - Returns the count of members (use to limit rank values)
- [[CUBEVALUE]] - Retrieves values for ranked members
- [[CUBEMEMBER]] - Returns single specified members (vs. positional retrieval)
- [[CUBEKPIMEMBER]] - Retrieves KPI members (different use case)

## Official Documentation

- [Microsoft Excel CUBERANKEDMEMBER Function](https://support.microsoft.com/en-us/office/cuberankedmember-function-07efecde-e669-4075-b9bf-6b40df2dc4b3)
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
