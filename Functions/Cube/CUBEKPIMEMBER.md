---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- cube
- olap
subTopics:
- kpi
- performance-metrics
- business-intelligence
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- kpi-member
- key-performance-indicator
---

# CUBEKPIMEMBER

## CUBEKPIMEMBER Description

The CUBEKPIMEMBER function returns a Key Performance Indicator (KPI) property from an OLAP cube, enabling access to pre-defined business metrics with their associated targets, status indicators, and trend information. KPIs in OLAP cubes encapsulate complete performance measurement definitions including actual values, goal values, status calculations, and trend analysis, which CUBEKPIMEMBER exposes for display in Excel reports and dashboards.

CUBEKPIMEMBER retrieves specific components of a KPI definition rather than raw measure values. A single KPI typically includes multiple properties: the actual value (current performance), goal/target value, status (comparison of actual to goal, often shown as traffic light indicators), trend (direction of change over time), and weight (relative importance). Each property can be extracted individually using the kpi_property parameter.

The function is particularly valuable for executive dashboards and balanced scorecard implementations where KPIs are centrally defined in the cube with business logic for status and trend calculations. Rather than recreating complex conditional logic in Excel, CUBEKPIMEMBER retrieves server-calculated status indicators that reflect consistent business rules across all reporting tools.

CUBEKPIMEMBER is exclusive to Excel and requires an OLAP cube that contains defined KPI objects. Not all cubes include KPIs; this is a design-time decision by cube developers. When KPIs are available, CUBEKPIMEMBER provides a clean interface to their properties without requiring MDX expertise. The function returns member references that can be used with CUBEVALUE to retrieve actual numeric values.

## Syntax

```
=CUBEKPIMEMBER(connection, kpi_name, kpi_property, [caption])
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| connection | Yes | Text string specifying the OLAP connection name. |
| kpi_name | Yes | The name of the KPI in the cube, as a text string. |
| kpi_property | Yes | Integer specifying which KPI property to return: 1 = Value, 2 = Goal, 3 = Status, 4 = Trend, 5 = Weight, 6 = Current Time Member. |
| caption | No | Optional text to display instead of the default KPI property caption. |

## Examples

```
=CUBEKPIMEMBER("SalesCube", "Revenue Growth", 1)
// Returns: KPI value member for Revenue Growth
// Retrieve the actual value property

=CUBEKPIMEMBER("SalesCube", "Revenue Growth", 2)
// Returns: KPI goal member for Revenue Growth
// Retrieve the target/goal property

=CUBEKPIMEMBER("SalesCube", "Revenue Growth", 3)
// Returns: KPI status member
// Retrieve status indicator (-1, 0, or 1 typically)

=CUBEKPIMEMBER("SalesCube", "Revenue Growth", 4)
// Returns: KPI trend member
// Retrieve trend indicator

=CUBEKPIMEMBER("SalesCube", "Customer Satisfaction", 5)
// Returns: KPI weight member
// Retrieve relative importance weight

=CUBEKPIMEMBER("SalesCube", "Profit Margin", 6)
// Returns: Current time member for the KPI
// Retrieve the time context

=CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Revenue", 1))
// Returns: Actual revenue value
// Get numeric value from KPI value member

=CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Revenue", 2))
// Returns: Revenue target value
// Get numeric goal value

=CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Sales Growth", 3))
// Returns: -1, 0, or 1 (status indicator)
// Get numeric status for conditional formatting

=CUBEKPIMEMBER("FinanceCube", A1, B1)
// Returns: KPI property based on cell values
// Dynamic KPI name and property selection
```

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | KPI name does not exist in the cube | Verify KPI name matches exactly; check cube design for available KPIs |
| #VALUE! | Invalid kpi_property number | Use integers 1-6 only; verify property is defined for this KPI |
| #NAME? | Connection name not found | Confirm connection name matches Data Connections |
| #REF! | KPI was deleted from cube | Update formulas to reference current KPI definitions |
| #GETTING_DATA | Query processing | Wait for server response |
| No KPIs available | Cube does not contain KPI definitions | KPIs must be defined in cube design; contact cube administrator |

## Use Cases

### Executive KPI Dashboard
**Business Context**: Creating a unified dashboard displaying multiple KPIs with their actual values, targets, and status indicators for executive review and decision-making.

**Implementation**: Use CUBEKPIMEMBER to retrieve each KPI component and CUBEVALUE to get the numeric values, with conditional formatting based on status.

**Example Formula**:
```
=CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Revenue Growth", 1), "[Date].[Calendar].CurrentMember")
```

**Technical Details**: This retrieves the current revenue growth actual value. Pair with property 2 (goal) in an adjacent cell, and use property 3 (status) to drive traffic light conditional formatting, creating comprehensive KPI cards.

### Balanced Scorecard Implementation
**Business Context**: Building balanced scorecards with Financial, Customer, Process, and Learning perspectives, each containing multiple weighted KPIs that roll up to overall scores.

**Implementation**: Extract KPI values and weights using CUBEKPIMEMBER, calculate weighted scores in Excel, and display perspective and overall performance.

**Example Formula**:
```
=CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Market Share", 1)) * CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Market Share", 5))
```

**Technical Details**: This multiplies the KPI actual value by its weight for weighted contribution. Sum weighted values across all KPIs in a perspective for perspective scores, then across perspectives for overall organizational performance.

### Trend Analysis Display
**Business Context**: Showing not just current KPI values but also their trend direction (improving, stable, declining) to provide context for performance assessment.

**Implementation**: Retrieve KPI trend property and map to visual indicators (arrows, icons) using conditional formatting or symbol fonts.

**Example Formula**:
```
=IF(CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Revenue", 4))=1, "Improving", IF(CUBEVALUE("SalesCube", CUBEKPIMEMBER("SalesCube", "Revenue", 4))=-1, "Declining", "Stable"))
```

**Technical Details**: KPI trend typically returns -1 (declining), 0 (stable), or 1 (improving). This formula converts numeric codes to text; alternatively, use Wingdings font characters or conditional formatting icons for visual trend arrows.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Availability | Excel 2007 and later | Not available |
| KPI support | Requires cube with KPI definitions | Not applicable |
| Property types | 6 properties (1-6) | Not applicable |
| Custom captions | Supported via fourth parameter | Not applicable |
| Integration with CUBEVALUE | Full support | Not applicable |
| Status/Trend indicators | Typically -1, 0, 1 numeric values | Not applicable |
| Excel Online | Limited functionality | Not applicable |
| PowerPivot KPIs | Supported | Not applicable |

## Tips

1. **Property reference**: Memorize or document the property numbers: 1=Value, 2=Goal, 3=Status, 4=Trend, 5=Weight, 6=Time. Create a named reference table for clarity.

2. **Status interpretation**: Status values are typically -1 (bad/red), 0 (warning/yellow), 1 (good/green), but this depends on cube KPI definition. Verify with cube documentation.

3. **CUBEVALUE required**: CUBEKPIMEMBER returns a member reference, not a value. Always wrap in CUBEVALUE to get numeric results: `=CUBEVALUE(conn, CUBEKPIMEMBER(...))`.

4. **KPI availability**: Not all cubes have KPIs defined. Check with your BI team or use cube browser tools to see available KPIs before building reports.

5. **Conditional formatting**: Map status values (-1, 0, 1) to icon sets or color scales for intuitive traffic-light displays in dashboards.

## Related Functions

- [[CUBEVALUE]] - Required to retrieve numeric values from CUBEKPIMEMBER results
- [[CUBEMEMBER]] - Returns dimension members (CUBEKPIMEMBER is KPI-specific)
- [[CUBESET]] - Creates sets (not applicable to KPIs)
- [[CUBESETCOUNT]] - Counts set members (not applicable to KPIs)
- [[CUBERANKEDMEMBER]] - Returns ranked members (not applicable to KPIs)

## Official Documentation

- [Microsoft Excel CUBEKPIMEMBER Function](https://support.microsoft.com/en-us/office/cubekpimember-function-744608bf-2c62-42cd-b67a-a56109f4b03b)
- [Analysis Services KPI Overview](https://learn.microsoft.com/en-us/analysis-services/multidimensional-models/key-performance-indicators-kpis-in-multidimensional-models)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2007 | 2007 | Initial release |
| Excel 2010 | 2010 | PowerPivot KPI support |
| Excel 2013 | 2013 | Data Model KPI integration |
| Excel 2016 | 2016 | Power BI KPI connectivity |
| Excel 2019 | 2019 | No significant changes |
| Excel 365 | 2020+ | Continuous cloud service improvements |
| Google Sheets | N/A | Not available - Excel-only function |
