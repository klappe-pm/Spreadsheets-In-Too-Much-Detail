---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- cube
- olap
subTopics:
- sets
- filtering
- business-intelligence
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- cube-set
- olap-set
---

# CUBESET

## CUBESET Description

The CUBESET function defines a calculated set of members or tuples from an OLAP cube by sending a set expression to the server and returning the result set for use in other Cube functions. This function enables complex filtering, custom member collections, and dynamic groupings that go beyond simple single-member references, providing powerful analytical capabilities directly within Excel formulas.

CUBESET evaluates an MDX (Multidimensional Expressions) set expression on the OLAP server, which can include functions like Filter, Order, TopCount, Head, Tail, and set operations like Union, Intersect, and Except. The resulting set is stored in the cell for reference by CUBEVALUE, CUBERANKEDMEMBER, and CUBESETCOUNT functions. The cell displays a caption (customizable) representing the set, while internally maintaining the complete member list.

The function is particularly powerful for creating dynamic Top N analysis, filtered member lists, and custom aggregation groups without modifying the underlying cube structure. For example, CUBESET can define "Top 10 Products by Revenue" or "Stores with Sales > $1M" as reusable sets that update automatically when data changes.

CUBESET supports sorting through the optional sort_order and sort_by parameters, allowing sets to be returned in ascending or descending order based on a measure. This enables dynamic ranked lists directly in Excel, useful for leaderboards, exception reports, and prioritized analysis. The function is exclusive to Excel and requires an OLAP connection.

## Syntax

```
=CUBESET(connection, set_expression, [caption], [sort_order], [sort_by])
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| connection | Yes | Text string specifying the OLAP connection name. |
| set_expression | Yes | MDX set expression defining the members to include. Can use MDX set functions. |
| caption | No | Text to display in the cell. If omitted, displays a default set caption. |
| sort_order | No | Integer specifying sort order: 0 = default order, 1 = ascending, 2 = descending, 3 = ascending alphabetical, 4 = descending alphabetical, 5 = natural ascending, 6 = natural descending. |
| sort_by | No | MDX expression for the measure or value to sort by. Required if sort_order is 1 or 2. |

## Examples

```
=CUBESET("SalesCube", "[Product].[Category].Members")
// Returns: Set containing all product categories
// Basic set of all members at a hierarchy level

=CUBESET("SalesCube", "[Product].[Category].Members", "All Categories")
// Returns: "All Categories" (displayed caption)
// Custom caption for the set

=CUBESET("SalesCube", "{[Product].[Category].[Electronics], [Product].[Category].[Clothing]}")
// Returns: Set with two specific members
// Explicit member list in curly braces

=CUBESET("SalesCube", "TopCount([Product].[Product].Members, 10, [Measures].[Sales Amount])", "Top 10 Products", 2, "[Measures].[Sales Amount]")
// Returns: Top 10 products by sales, sorted descending
// Top N analysis with sorting

=CUBESET("SalesCube", "Filter([Customer].[Customer].Members, [Measures].[Total Orders] > 100)", "High-Volume Customers")
// Returns: Customers with more than 100 orders
// Filtered set based on measure condition

=CUBESET("SalesCube", "[Date].[Calendar Year].Members", "Years", 1, "[Measures].[Sales Amount]")
// Returns: Years sorted by ascending sales
// Sorted set by measure

=CUBESET("SalesCube", "Descendants([Geography].[Region].[North America], [Geography].[City])", "NA Cities")
// Returns: All cities in North America
// Hierarchical descendants

=CUBESET("SalesCube", "Except([Product].[Category].Members, {[Product].[Category].[Discontinued]})", "Active Categories")
// Returns: All categories except Discontinued
// Set difference operation

=CUBESET("SalesCube", "Union([Product].[Category].[Electronics].Children, [Product].[Category].[Appliances].Children)", "Tech Products")
// Returns: Combined children of two categories
// Set union operation

=CUBESET("SalesCube", "Head([Product].[Product].Members, 5)")
// Returns: First 5 products
// Head function for limiting set size
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | Set expression returns empty set | Verify set logic; check that members meeting criteria exist |
| #VALUE! | Invalid MDX set expression syntax | Check MDX syntax; ensure proper use of set functions and brackets |
| #NAME? | Connection name not found | Verify connection name matches Data Connections exactly |
| #GETTING_DATA | Server processing query | Wait for completion; complex set expressions may take longer |
| #REF! | Referenced dimension or hierarchy not found | Verify dimension/hierarchy names match current cube structure |
| Sort not working | sort_by parameter missing when using sort_order 1 or 2 | Provide valid measure expression for sort_by parameter |

## Use Cases

### Top N Product Analysis
**Business Context**: Identifying top-performing products, customers, or regions for executive reporting and strategic focus areas.

**Implementation**: Use CUBESET with TopCount or BottomCount MDX functions to create dynamic ranked lists that update as data changes.

**Example Formula**:
```
=CUBESET("SalesCube", "TopCount([Product].[Product].Members, 10, [Measures].[Profit])", "Top 10 Profitable Products", 2, "[Measures].[Profit]")
```

**Technical Details**: This creates a set of the 10 most profitable products, sorted by profit descending. Use with CUBERANKEDMEMBER to display individual products and CUBEVALUE to show their metrics in a formatted report.

### Exception Reporting
**Business Context**: Highlighting items that fall outside expected parameters, such as products with declining sales, regions missing targets, or customers with unusual patterns.

**Implementation**: Use CUBESET with Filter to create dynamic exception lists based on measure thresholds or calculated conditions.

**Example Formula**:
```
=CUBESET("SalesCube", "Filter([Product].[Product].Members, [Measures].[Sales YoY Growth] < 0)", "Declining Products")
```

**Technical Details**: This creates a set of products with negative year-over-year growth. The set automatically adjusts as sales data updates, keeping exception reports current without manual intervention.

### Custom Aggregation Groups
**Business Context**: Creating ad-hoc groupings of dimension members for analysis not supported by existing cube hierarchies, such as combining specific regions into custom territories or grouping products for promotional analysis.

**Implementation**: Use CUBESET with explicit member lists or Union operations to define custom groupings, then use CUBEVALUE to aggregate measures across the set.

**Example Formula**:
```
=CUBESET("SalesCube", "Union({[Geography].[Country].[USA], [Geography].[Country].[Canada]}, {[Geography].[Country].[Mexico]})", "North America Custom")
```

**Technical Details**: This creates a custom set combining three countries. CUBEVALUE referencing this set will aggregate measures across all three, enabling analysis at grouping levels not defined in the cube.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2007 and later | Not available |
| MDX set functions | Full support (Filter, TopCount, etc.) | Not applicable |
| Sorting capability | 7 sort options (0-6) | Not applicable |
| Maximum set size | Limited by OLAP server configuration | Not applicable |
| Set operations | Union, Intersect, Except supported | Not applicable |
| Dynamic sets | Fully recalculated on refresh | Not applicable |
| Nested CUBESET | Supported | Not applicable |
| Excel Online | Limited functionality | Not applicable |

## Tips

1. **Set size awareness**: Large sets can impact performance. Use TopCount, Head, or Filter to limit set size when working with high-cardinality dimensions.

2. **Caption best practices**: Always provide descriptive captions for maintainability. "Top 10 Customers by Revenue" is clearer than the default set display.

3. **Combining with CUBERANKEDMEMBER**: CUBESET defines the list; CUBERANKEDMEMBER(connection, set_cell, n) extracts individual members for display. Use them together for ranked lists.

4. **Server-side calculation**: Set expressions execute on the OLAP server. Complex Filter or TopCount operations leverage server performance rather than Excel calculations.

5. **Dynamic TopN**: Change the N value dynamically: `=CUBESET("SalesCube", "TopCount([Product].Members, "&A1&", [Measures].[Sales])")` where A1 contains the count.

## Related Functions

- [[CUBEVALUE]] - Retrieves aggregated values; can sum across a CUBESET
- [[CUBERANKEDMEMBER]] - Returns individual members from a CUBESET by position
- [[CUBESETCOUNT]] - Returns the count of members in a CUBESET
- [[CUBEMEMBER]] - Returns single members (CUBESET is for multiple members)
- [[CUBEKPIMEMBER]] - Retrieves KPI-related members

## Official Documentation

- [Microsoft Excel CUBESET Function](https://support.microsoft.com/en-us/office/cubeset-function-5b2146bd-62d6-4f57-b5c7-9f23c5d47cf7)
- [MDX Set Functions Reference](https://learn.microsoft.com/en-us/analysis-services/multidimensional-models/mdx/mdx-function-reference-mdx)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2007 | 2007 | Initial release |
| Excel 2010 | 2010 | PowerPivot integration |
| Excel 2013 | 2013 | Enhanced Data Model support |
| Excel 2016 | 2016 | Power BI connectivity |
| Excel 2019 | 2019 | Improved performance for large sets |
| Excel 365 | 2020+ | Continuous cloud connectivity improvements |
| Google Sheets | N/A | Not available - Excel-only function |
