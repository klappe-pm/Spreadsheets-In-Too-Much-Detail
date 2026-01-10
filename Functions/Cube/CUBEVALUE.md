---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- cube
- olap
subTopics:
- data-retrieval
- aggregation
- business-intelligence
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- cube-aggregate
- olap-value
---

# CUBEVALUE

## CUBEVALUE Description

The CUBEVALUE function retrieves an aggregated value from an OLAP (Online Analytical Processing) cube by specifying the intersection of multiple dimension members. This function is the primary method for extracting summarized data from multidimensional databases connected to Excel, enabling business intelligence reporting directly within spreadsheets. CUBEVALUE returns calculated measures such as sales totals, averages, counts, or custom calculations defined within the cube structure.

CUBEVALUE operates by accepting a connection name and one or more member expressions that define coordinates within the cube's dimensional structure. Each member expression specifies a position along a dimension hierarchy, such as a specific product category, time period, geographic region, or customer segment. The function returns the measure value at the intersection of all specified coordinates, performing server-side aggregation for optimal performance.

The function is particularly powerful because it leverages the cube's pre-calculated aggregations and optimized query engine, making it suitable for large-scale enterprise data analysis. Unlike standard Excel formulas that process data locally, CUBEVALUE sends requests to the OLAP server, which returns results from potentially billions of underlying records in milliseconds. This architecture supports real-time dashboards and ad-hoc analysis against enterprise data warehouses.

CUBEVALUE is exclusively an Excel function and requires an active connection to an OLAP data source such as SQL Server Analysis Services (SSAS), Azure Analysis Services, or Power BI datasets. It is not available in Google Sheets, LibreOffice, or other spreadsheet applications. The function forms part of Excel's comprehensive suite of Cube functions designed for enterprise BI integration.

## Syntax

```
=CUBEVALUE(connection, [member_expression1], [member_expression2], ...)
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| connection | Yes | A text string specifying the name of the OLAP connection to the cube. This is the connection name as defined in Excel's Data Connections. |
| member_expression1 | No | An MDX expression or cell reference to a CUBEMEMBER/CUBESET result that identifies a member or tuple in the cube. |
| member_expression2, ... | No | Additional member expressions to further define the intersection point. Up to 255 member expressions can be provided. |

## Examples

```
=CUBEVALUE("SalesCube", "[Measures].[Sales Amount]")
// Returns: Total sales amount across all dimensions
// Basic measure retrieval without dimension filters

=CUBEVALUE("SalesCube", "[Measures].[Sales Amount]", "[Date].[Calendar Year].[2024]")
// Returns: Sales amount for calendar year 2024
// Single dimension filter applied

=CUBEVALUE("SalesCube", "[Measures].[Sales Amount]", "[Date].[Calendar Year].[2024]", "[Product].[Category].[Electronics]")
// Returns: Electronics sales for 2024
// Multiple dimension filters

=CUBEVALUE("FinanceCube", "[Measures].[Revenue]", A2, B2)
// Returns: Revenue at intersection of members in A2 and B2
// Cell references to CUBEMEMBER results

=CUBEVALUE("SalesCube", "[Measures].[Profit Margin]", CUBEMEMBER("SalesCube", "[Geography].[Country].[United States]"))
// Returns: Profit margin for United States
// Nested CUBEMEMBER function

=CUBEVALUE("InventoryCube", "[Measures].[Stock Level]", $A$1, B$1, $A2)
// Returns: Stock level at mixed reference intersection
// Absolute and relative references for pivot-style layouts

=CUBEVALUE("SalesCube", "[Measures].[Sales Amount]", "[Date].[Calendar Year].&[2024]")
// Returns: Sales for 2024 using member key syntax
// Ampersand key-based member reference

=IFERROR(CUBEVALUE("SalesCube", "[Measures].[Sales]", A2), "No Data")
// Returns: Sales value or "No Data" if member combination invalid
// Error handling for missing intersections

=CUBEVALUE("SalesCube", "[Measures].[Sales Amount]") - CUBEVALUE("SalesCube", "[Measures].[Cost]")
// Returns: Calculated gross profit
// Multiple CUBEVALUE calls in formula

=CUBEVALUE("SalesCube", "[Measures].[YTD Sales]", "[Date].[Calendar].[Month].&[202412]")
// Returns: Year-to-date sales through December 2024
// Time intelligence measure
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | No value exists at the specified intersection | Use IFERROR to handle empty intersections; verify member expressions are valid |
| #VALUE! | Invalid member expression syntax | Check MDX syntax; ensure dimension and hierarchy names are correct |
| #NAME? | Connection name not found or invalid | Verify connection name matches exactly; check Data Connections list |
| #REF! | Referenced cell containing CUBEMEMBER returns error | Fix the source CUBEMEMBER formula first |
| #GETTING_DATA | Query still processing | Wait for calculation to complete; check network connectivity to OLAP server |
| Unexpected aggregation | Wrong measure or missing dimensions | Ensure correct measure is specified; add required dimension filters |

## Use Cases

### Dynamic Dashboard Reporting
**Business Context**: Creating interactive dashboards where users select dimension members (regions, time periods, products) and see corresponding KPIs update automatically across multiple metrics.

**Implementation**: Build a grid of CUBEVALUE formulas that reference CUBEMEMBER cells containing user-selected dimensions, allowing one selection to update an entire dashboard.

**Example Formula**:
```
=CUBEVALUE($A$1, "[Measures].[Revenue]", $B$2, $C$2)
```

**Technical Details**: Cell A1 contains the connection name, B2 contains a CUBEMEMBER for selected region, C2 for selected year. Changing the CUBEMEMBER selections updates all dependent CUBEVALUE cells, creating interactive reporting without VBA.

### Pivot-Style Cross-Tabulation
**Business Context**: Building custom pivot-table-like reports with greater formatting flexibility than standard PivotTables, allowing complete control over layout, formatting, and additional calculations.

**Implementation**: Create row and column headers using CUBEMEMBER, then populate the data area with CUBEVALUE formulas referencing the appropriate header cells.

**Example Formula**:
```
=CUBEVALUE("SalesCube", "[Measures].[Sales]", $A3, B$1)
```

**Technical Details**: With regions in column A and years across row 1 as CUBEMEMBER values, this formula retrieves sales at each intersection. Copy down and across to fill the table. Mixed references ensure correct member combinations.

### KPI Scorecards with Targets
**Business Context**: Displaying actual performance against targets with variance calculations, trends, and conditional formatting for executive scorecards.

**Implementation**: Use CUBEVALUE to retrieve actual values, target values, and calculated variances, with conditional formatting highlighting performance status.

**Example Formula**:
```
=CUBEVALUE("KPICube", "[Measures].[Actual Revenue]", "[Date].[Month].&[202412]") / CUBEVALUE("KPICube", "[Measures].[Target Revenue]", "[Date].[Month].&[202412]") - 1
```

**Technical Details**: This calculates percentage variance from target. Combining multiple CUBEVALUE calls enables complex calculations that may not be pre-defined in the cube, providing analytical flexibility while leveraging server-side aggregation.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2007 and later | Not available |
| OLAP connectivity | SQL Server Analysis Services, Azure AS, Power BI, others | Not available |
| Maximum member expressions | 255 | Not applicable |
| Asynchronous calculation | Supported (shows #GETTING_DATA) | Not applicable |
| Caching | Query results cached until refresh | Not applicable |
| MDX support | Full MDX expression support | Not applicable |
| PowerPivot integration | Fully integrated | Not applicable |
| Excel Online support | Limited (read-only display) | Not applicable |

## Tips

1. **Connection management**: Create named connections with descriptive names. Use `=CUBEVALUE("Sales_Production", ...)` rather than generic names to distinguish between test and production environments.

2. **Performance optimization**: Minimize the number of CUBEVALUE calls by designing efficient layouts. Each formula generates a server query; hundreds of cells can impact workbook performance.

3. **Error handling**: Always wrap CUBEVALUE in IFERROR for production reports: `=IFERROR(CUBEVALUE(...), "-")` handles missing data gracefully.

4. **Debugging MDX**: Test member expressions in SQL Server Management Studio's MDX query window before using in Excel to verify syntax and expected results.

5. **Refresh strategy**: CUBEVALUE results cache until workbook refresh. Use Data > Refresh All or VBA automation for scheduled updates in reporting scenarios.

## Related Functions

- [[CUBEMEMBER]] - Returns a member or tuple from the cube for use in CUBEVALUE
- [[CUBESET]] - Defines a calculated set of members for filtering CUBEVALUE
- [[CUBESETCOUNT]] - Counts items in a set created by CUBESET
- [[CUBERANKEDMEMBER]] - Returns the nth member from a set
- [[CUBEKPIMEMBER]] - Returns KPI properties from a cube
- [[CUBEMEMBERPROPERTY]] - Returns member property values

## Official Documentation

- [Microsoft Excel CUBEVALUE Function](https://support.microsoft.com/en-us/office/cubevalue-function-8733da24-26d1-4e34-9b3a-84a8f00dcbe0)
- [Excel Cube Functions Overview](https://support.microsoft.com/en-us/office/cube-functions-reference-2378132b-d3f2-4af1-896d-48a9ee840eb2)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2007 | 2007 | Initial release with OLAP connectivity |
| Excel 2010 | 2010 | Improved PowerPivot integration |
| Excel 2013 | 2013 | Enhanced Data Model support |
| Excel 2016 | 2016 | Power BI dataset connectivity |
| Excel 2019 | 2019 | Azure Analysis Services support improvements |
| Excel 365 | 2020+ | Continuous updates for cloud data source connectivity |
| Google Sheets | N/A | Not available - Excel-only function |
