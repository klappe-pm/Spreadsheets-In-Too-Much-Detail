---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- cube
- olap
subTopics:
- dimension-members
- hierarchy
- business-intelligence
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- cube-dimension
- olap-member
---

# CUBEMEMBER

## CUBEMEMBER Description

The CUBEMEMBER function returns a member or tuple from an OLAP cube, displaying the member's caption in the cell while storing the underlying member reference for use in other Cube functions. This function serves as the foundation for building dynamic OLAP reports in Excel, providing the dimension coordinates that CUBEVALUE uses to retrieve specific data points. CUBEMEMBER validates that the specified member exists in the cube and returns an error if the member cannot be found.

CUBEMEMBER accepts an MDX (Multidimensional Expressions) member expression that identifies a specific position within a cube dimension hierarchy. The expression can reference any level of the hierarchy - from the all-member root down to leaf-level detail members. When the formula calculates successfully, the cell displays the member's caption (friendly name) while internally storing the member's unique name for reference by other formulas.

The function supports tuple syntax, allowing multiple dimension members to be combined into a single CUBEMEMBER result. This enables complex intersection references where a single cell represents a specific combination of dimensions, which CUBEVALUE can then use as a single argument. Tuples are specified using parentheses containing comma-separated member expressions.

CUBEMEMBER is essential for creating flexible, maintainable OLAP reports in Excel. By isolating member references in dedicated cells, report designers can build CUBEVALUE grids that automatically update when CUBEMEMBER selections change, enabling interactive reporting without VBA programming. The function is exclusive to Excel and requires an active connection to an OLAP data source.

## Syntax

```
=CUBEMEMBER(connection, member_expression, [caption])
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| connection | Yes | A text string specifying the name of the OLAP connection to the cube. Must match an existing Data Connection name. |
| member_expression | Yes | An MDX expression identifying a member or tuple within the cube. Can include hierarchy, level, and member specifications. |
| caption | No | Optional text to display in the cell instead of the member's default caption from the cube. Useful for custom labeling. |

## Examples

```
=CUBEMEMBER("SalesCube", "[Product].[Category].[Electronics]")
// Returns: "Electronics"
// Simple member reference displaying caption

=CUBEMEMBER("SalesCube", "[Date].[Calendar Year].[2024]", "FY2024")
// Returns: "FY2024"
// Custom caption overrides default display

=CUBEMEMBER("SalesCube", "[Geography].[Country].&[US]")
// Returns: "United States"
// Member key syntax with ampersand

=CUBEMEMBER("SalesCube", "[Product].[Category].Members")
// Returns: First member of the set
// Using .Members to reference all category members

=CUBEMEMBER("SalesCube", "[Date].[Calendar].[Month].&[202401]")
// Returns: "January 2024"
// Date dimension member by key

=CUBEMEMBER("SalesCube", "([Product].[Category].[Electronics], [Geography].[Region].[West])")
// Returns: "Electronics, West" (or similar tuple display)
// Tuple combining multiple dimension members

=CUBEMEMBER("SalesCube", "[Date].[Calendar Year].[All Years]")
// Returns: "All Years"
// All-member for aggregation across entire dimension

=CUBEMEMBER("FinanceCube", "[Account].[Account Number].&["&A1&"]")
// Returns: Account name for account number in A1
// Dynamic member expression with cell reference

=CUBEMEMBER("SalesCube", "[Product].[Category].[Clothing].Children")
// Returns: First child of Clothing category
// Hierarchy navigation with .Children

=CUBEMEMBER("SalesCube", "[Date].[Calendar Year].["&YEAR(TODAY())&"]")
// Returns: Current year member
// Dynamic current year member
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | Member does not exist in the cube | Verify member name spelling; check if member was deleted from source data |
| #VALUE! | Invalid MDX syntax in member expression | Review MDX syntax; check bracket placement and hierarchy path |
| #NAME? | Connection name not found | Confirm connection name matches Data Connections exactly |
| #REF! | Cube structure changed; member path invalid | Update member expression to match current cube structure |
| #GETTING_DATA | Query processing | Wait for server response; check OLAP server connectivity |
| Displays unique name instead of caption | Caption parameter issue or cube configuration | Use explicit caption parameter; verify cube member captions are defined |

## Use Cases

### Interactive Dimension Selection
**Business Context**: Enabling users to select a product category, region, or time period from dropdown lists and have all dashboard metrics update automatically based on the selection.

**Implementation**: Create data validation dropdowns that feed into CUBEMEMBER formulas, which then drive dependent CUBEVALUE calculations.

**Example Formula**:
```
=CUBEMEMBER("SalesCube", "[Product].[Category].["&B1&"]")
```

**Technical Details**: Cell B1 contains a dropdown with category names (Electronics, Clothing, etc.). Selecting a category updates this CUBEMEMBER, which then propagates to all CUBEVALUE formulas referencing this cell, creating interactive dashboards without macros.

### Dynamic Row and Column Headers
**Business Context**: Building custom pivot-style reports where row and column headers come from cube dimensions and can be easily reorganized or filtered.

**Implementation**: Use CUBEMEMBER formulas for row/column headers, enabling layout changes by modifying member expressions rather than rebuilding the entire report.

**Example Formula**:
```
=CUBEMEMBER("SalesCube", "[Date].[Quarter].&["&TEXT(EDATE(TODAY(),-3),"YYYY")&"Q"&ROUNDUP(MONTH(EDATE(TODAY(),-3))/3,0)&"]")
```

**Technical Details**: This dynamically calculates the previous quarter's member reference. As time passes, the report automatically shifts to the correct quarter without manual updates.

### Hierarchical Navigation
**Business Context**: Displaying drill-down hierarchies where selecting a parent member reveals its children, enabling exploration of data at varying levels of detail.

**Implementation**: Use CUBEMEMBER with .Children, .Parent, or .Siblings to navigate cube hierarchies programmatically.

**Example Formula**:
```
=CUBEMEMBER("SalesCube", CUBEMEMBER("SalesCube", "[Geography].[Region].[West]")&".Children")
```

**Technical Details**: This returns the first child of the West region, typically states or countries within West. Combined with CUBERANKEDMEMBER, you can iterate through all children to display complete drill-down lists.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2007 and later | Not available |
| OLAP connectivity | Full SSAS, Azure AS, Power BI support | Not available |
| Custom captions | Supported via third parameter | Not applicable |
| Tuple support | Full tuple syntax supported | Not applicable |
| Dynamic member expressions | Supports cell references in MDX | Not applicable |
| Hierarchy navigation | .Children, .Parent, .Siblings supported | Not applicable |
| Calculated members | Can reference cube-defined calculated members | Not applicable |
| Excel Online | Limited functionality | Not applicable |

## Tips

1. **Member key vs. caption**: Use ampersand syntax `[Dimension].[Hierarchy].&[Key]` for reliable references. Captions may change, but keys remain stable.

2. **Null vs. missing**: A member that exists but has no data returns valid CUBEMEMBER; CUBEVALUE on that member returns #N/A. CUBEMEMBER itself fails only if the member does not exist.

3. **Tuple limitations**: While tuples combine dimensions, they cannot include multiple members from the same dimension. Each dimension can appear only once in a tuple.

4. **Performance**: CUBEMEMBER queries are typically fast since they retrieve metadata, not measures. Use them freely for headers without significant performance impact.

5. **Caption parameter**: The optional caption is purely for display. It does not affect how other formulas interpret the member; the underlying MDX reference is always used.

## Related Functions

- [[CUBEVALUE]] - Retrieves aggregated values using CUBEMEMBER references
- [[CUBESET]] - Creates calculated sets of multiple members
- [[CUBERANKEDMEMBER]] - Returns the nth member from a set
- [[CUBESETCOUNT]] - Counts members in a CUBESET result
- [[CUBEKPIMEMBER]] - Retrieves KPI-specific members
- [[CUBEMEMBERPROPERTY]] - Gets properties of a member

## Official Documentation

- [Microsoft Excel CUBEMEMBER Function](https://support.microsoft.com/en-us/office/cubemember-function-0f6a15b9-2c18-4819-ae89-e1b5c8b398ad)
- [MDX Member Expressions Reference](https://learn.microsoft.com/en-us/analysis-services/multidimensional-models/mdx/mdx-member-expressions)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2007 | 2007 | Initial release with OLAP support |
| Excel 2010 | 2010 | PowerPivot integration |
| Excel 2013 | 2013 | Improved Data Model connectivity |
| Excel 2016 | 2016 | Power BI dataset support |
| Excel 2019 | 2019 | Enhanced cloud data source support |
| Excel 365 | 2020+ | Continuous improvements for Analysis Services |
| Google Sheets | N/A | Not available - Excel-only function |
