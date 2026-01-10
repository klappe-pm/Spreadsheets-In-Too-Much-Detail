---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- pivot-tables
- data-extraction
- report-automation
- business-intelligence
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Pivot Data
- Extract Pivot Value
- Pivot Table Lookup
tags:
- lookup
- reference
- pivot
- data-extraction
- reporting
---

# GETPIVOTDATA

## Description

**GETPIVOTDATA** extracts specific values from a PivotTable using field names and criteria, rather than cell references. While you can click on a PivotTable cell to get its value, those references break when the PivotTable layout changes. GETPIVOTDATA creates stable references that continue working even when rows and columns are rearranged, filtered, or when the PivotTable refreshes with new data. It's the only reliable way to build formulas referencing PivotTable data.

The function uses a unique syntax with field name/value pairs to identify exactly which data point you want. Instead of `=B5` (which might move), you write `=GETPIVOTDATA("Sales", $A$3, "Region", "North", "Year", 2024)` which always retrieves North region's 2024 sales regardless of where that cell ends up in the PivotTable. This semantic referencing is what makes GETPIVOTDATA invaluable.

**GETPIVOTDATA is automatically generated when you click PivotTable cells.** If you type = then click a PivotTable cell, Excel generates a GETPIVOTDATA formula instead of a simple cell reference. Some users find this annoying for quick references, but it's essential for stable reporting. You can disable this behavior in PivotTable Options, but for serious reporting work, embrace it.

**The function works with multiple field criteria.** Complex PivotTables with multiple row/column fields require multiple field/value pairs to identify unique data points. `=GETPIVOTDATA("Revenue", PivotRef, "Year", 2024, "Quarter", "Q3", "Product", "Widget", "Region", "West")` drills down through multiple dimensions to find exactly the value needed.

## Syntax

> [!f(x)] GETPIVOTDATA Syntax
>
> ```
> =GETPIVOTDATA(data_field, pivot_table, [field1, item1], [field2, item2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `data_field` | Yes | The name of the value field to retrieve, as text. Must match a field name in the Values area of the PivotTable (e.g., "Sum of Sales", "Average of Price"). |
| `pivot_table` | Yes | A reference to any cell within the PivotTable. Typically use the top-left cell with absolute reference ($A$3). |
| `field1, item1` | No | Field name and item value pair specifying criteria. Field is text (e.g., "Region"), item is the value to match (e.g., "North"). |
| `field2, item2...` | No | Additional field/item pairs for multi-dimensional lookups. Add as many pairs as needed to uniquely identify the data point. |

### Return Value

Returns the numeric or text value from the specified data field that matches all field/item criteria. Returns #REF! if the PivotTable doesn't contain data matching the criteria, or if field names don't match.

## Examples

> [!f(x)] GETPIVOTDATA Examples

### Example 1: Basic Single-Field Lookup
```
=GETPIVOTDATA("Sum of Sales", $A$3, "Region", "North")
```
**Result:** Returns total sales for North region from the PivotTable

**Explanation:** Retrieves the "Sum of Sales" value where Region equals "North". $A$3 anchors to a cell in the PivotTable. The field name "Region" must match the PivotTable field exactly.

---

### Example 2: Multi-Dimensional Lookup
```
=GETPIVOTDATA("Sum of Revenue", $A$3, "Year", 2024, "Product", "Widget A")
```
**Result:** Returns 2024 revenue for Widget A

**Explanation:** Multiple field/item pairs narrow down to a specific intersection. Year=2024 AND Product="Widget A" identifies the exact cell value needed.

---

### Example 3: Grand Total Retrieval
```
=GETPIVOTDATA("Sum of Sales", $A$3)
```
**Result:** Returns the grand total of Sales from the PivotTable

**Explanation:** With no field/item criteria, GETPIVOTDATA returns the grand total for the specified data field. Useful for summary reporting.

---

### Example 4: Using Cell References for Criteria
```
=GETPIVOTDATA("Sum of Sales", $A$3, "Region", E2, "Year", F2)
```
**Result:** Returns sales for the region in E2 and year in F2

**Explanation:** Criteria values can come from cells, enabling dynamic lookups. Change E2 and F2 to retrieve different data points. Perfect for dashboard interactivity.

---

### Example 5: Calculated Field Extraction
```
=GETPIVOTDATA("Profit Margin", $A$3, "Product Category", "Electronics")
```
**Result:** Returns the calculated profit margin for Electronics

**Explanation:** Calculated fields in PivotTables work with GETPIVOTDATA. Use the calculated field name as data_field. The field must exist in the PivotTable's Values area.

---

### Example 6: Date-Based Criteria
```
=GETPIVOTDATA("Count of Orders", $A$3, "Order Date", DATE(2024,6,15))
```
**Result:** Returns order count for June 15, 2024

**Explanation:** Date criteria should use DATE function or formatted date values matching the PivotTable's date format. Ensure date grouping in PivotTable matches your criteria format.

---

### Example 7: Grouped Date Lookup (Month)
```
=GETPIVOTDATA("Sum of Sales", $A$3, "Date", "Jun")
```
**Result:** Returns June sales when dates are grouped by month

**Explanation:** When PivotTable dates are grouped (by month, quarter, year), use the group label as the item value. "Jun" matches the month group, not individual dates.

---

### Example 8: With Row and Column Fields
```
=GETPIVOTDATA("Average of Price", $A$3, "Category", "Food", "Region", "East", "Quarter", "Q2")
```
**Result:** Returns average price for Food category, East region, Q2

**Explanation:** Complex PivotTables with multiple row and column fields require all relevant field/item pairs to uniquely identify the intersection.

---

### Example 9: Handling Special Characters in Field Names
```
=GETPIVOTDATA("Sum of Sales ($)", $A$3, "Region", "North")
```
**Result:** Works with field names containing special characters

**Explanation:** If your data field is named "Sum of Sales ($)", include the exact name with special characters. Field names are case-insensitive but must otherwise match exactly.

---

### Example 10: Multiple Data Fields
```
=GETPIVOTDATA("Sum of Revenue", $A$3, "Year", 2024) - GETPIVOTDATA("Sum of Costs", $A$3, "Year", 2024)
```
**Result:** Calculates 2024 profit from two PivotTable fields

**Explanation:** Use multiple GETPIVOTDATA calls to perform calculations across different value fields. Here we subtract costs from revenue for 2024.

---

### Example 11: Error Handling with IFERROR
```
=IFERROR(GETPIVOTDATA("Sum of Sales", $A$3, "Region", A10), 0)
```
**Result:** Returns 0 if the region in A10 doesn't exist in PivotTable

**Explanation:** GETPIVOTDATA returns #REF! when criteria don't match. Wrap in IFERROR for graceful handling—useful when criteria come from user input or variable data.

---

### Example 12: Building Reports from PivotTable Data
```
=GETPIVOTDATA("Sum of Units", PivotAnchor, "Store", StoreList, "Month", MONTH(ReportDate))
```
**Result:** Extracts units for specified store and month

**Explanation:** Use named references for cleaner formulas. PivotAnchor points to the PivotTable, StoreList is a named cell with store name, and ReportDate provides the month.

---

### Example 13: Percentage of Total Extraction
```
=GETPIVOTDATA("% of Total", $A$3, "Product", "Widget")
```
**Result:** Returns Widget's percentage of total (if that calculation exists in PivotTable)

**Explanation:** If your PivotTable includes "Show Values As" calculations like % of Total, use that exact field name. The PivotTable must have this calculation configured.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Specified criteria don't exist in PivotTable | Verify field names match exactly. Check that item values exist (case-insensitive but spelling-sensitive). |
| `#REF!` | pivot_table reference doesn't point to a PivotTable | Ensure the reference points to a cell within a valid PivotTable. |
| `#REF!` | data_field name doesn't match | Use exact field name as shown in PivotTable (e.g., "Sum of Sales" not just "Sales"). Check Values area field names. |
| `#VALUE!` | Odd number of field/item arguments | Field/item must come in pairs. Every field name needs a corresponding item value. |
| `Wrong value` | Criteria don't uniquely identify a cell | Add more field/item pairs to narrow down to the specific data point needed. |
| `#NAME?` | Field name not in quotes | Field names must be text strings in quotes: "Region" not Region. |

## Use Cases

### [[Stable Report Building]]

**Scenario:** Create reports that pull specific values from PivotTables without breaking when the PivotTable layout changes, is filtered differently, or refreshes with new data.

**Implementation:**
```
Executive summary cells:
Total Revenue: =GETPIVOTDATA("Sum of Revenue", PivotSheet!$A$3)
YTD Sales: =GETPIVOTDATA("Sum of Sales", $A$3, "Year", YEAR(TODAY()))
Top Region: =GETPIVOTDATA("Sum of Sales", $A$3, "Region", TopRegionCell)

Formatted report:
="Q"&QuarterNum&" Results: "&TEXT(GETPIVOTDATA("Sum of Sales", $A$3, "Quarter", "Q"&QuarterNum), "$#,##0")
```

**Business Application:** Financial reports, sales summaries, KPI dashboards, and any reporting that relies on PivotTable aggregations but needs stable cell references for presentation.

**Technical Details:** Use absolute references ($A$3) for the pivot_table parameter so formulas can be copied without breaking. Named ranges for the PivotTable anchor improve readability and maintenance.

---

### [[Interactive Dashboards]]

**Scenario:** Build dashboards where users select dimensions (region, product, time period) and the display updates to show the relevant PivotTable data.

**Implementation:**
```
Dashboard with dropdowns:
Selected Region: Data Validation dropdown in D2
Selected Year: Data Validation dropdown in D3

Display cells:
Total Sales: =GETPIVOTDATA("Sum of Sales", PivotData!$A$3, "Region", $D$2, "Year", $D$3)
Unit Count: =GETPIVOTDATA("Sum of Units", PivotData!$A$3, "Region", $D$2, "Year", $D$3)
Avg Price: =GETPIVOTDATA("Average of Price", PivotData!$A$3, "Region", $D$2, "Year", $D$3)
```

**Business Application:** Self-service reporting tools, manager dashboards with drill-down capability, and any interactive analysis where users explore PivotTable data through a cleaner interface.

**Technical Details:** Data Validation dropdowns populate with unique values from the source data. GETPIVOTDATA formulas reference these cells. Add IFERROR wrappers for graceful handling when users select combinations without data.

---

### [[Cross-PivotTable Analysis]]

**Scenario:** Compare or combine data from multiple PivotTables, or integrate PivotTable data with non-PivotTable calculations and data sources.

**Implementation:**
```
Compare regions across two PivotTables:
=GETPIVOTDATA("Sum of Sales", SalesPivot!$A$3, "Region", "North") - GETPIVOTDATA("Sum of Target", TargetPivot!$A$3, "Region", "North")

Variance analysis:
Actual: =GETPIVOTDATA("Sum of Actual", $A$3, "Month", MonthName)
Budget: =GETPIVOTDATA("Sum of Budget", $A$3, "Month", MonthName)
Variance: =(Actual-Budget)/Budget

Integration with non-pivot data:
=GETPIVOTDATA("Sum of Sales", $A$3, "Product", A2) / VLOOKUP(A2, CostTable, 2, FALSE)
```

**Business Application:** Budget vs actual analysis, multi-source data consolidation, cross-functional reporting combining sales data (PivotTable) with cost data (regular tables).

**Technical Details:** GETPIVOTDATA returns values that work in normal Excel calculations. You can add, subtract, multiply, divide, and combine with other functions freely. This bridges PivotTable aggregations with broader spreadsheet analysis.

---

### [[Automated Reporting Refresh]]

**Scenario:** Build reports that automatically show correct values after PivotTable data refreshes, without manual adjustment.

**Implementation:**
```
Monthly report template:
Current Month: =TEXT(EOMONTH(TODAY(),-1)+1,"mmmm yyyy")
Sales: =GETPIVOTDATA("Sum of Sales", $A$3, "Month", TEXT(EOMONTH(TODAY(),-1)+1,"mmm"))
Prior Month: =GETPIVOTDATA("Sum of Sales", $A$3, "Month", TEXT(EOMONTH(TODAY(),-2)+1,"mmm"))
YoY Change: =(CurrentMonth-PriorYear)/PriorYear
```

**Business Application:** Automated monthly/quarterly reports, rolling time period analysis, and any reporting that should update automatically when underlying data changes.

**Technical Details:** Combine GETPIVOTDATA with date functions to create time-aware reports. Ensure date criteria match PivotTable grouping format (abbreviated month, full month name, etc.). Test thoroughly after initial setup.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2002 (XP) and later
- **Auto-generation:** Enabled by default when clicking PivotTable cells
- **Disable auto-generation:** PivotTable Tools > Options > Generate GetPivotData
- **Multiple PivotTables:** Full support for referencing any PivotTable in workbook
- **OLAP PivotTables:** Works with OLAP-based PivotTables with MDX expressions

### Google Sheets

- **Availability:** Supported for Sheets Pivot Tables
- **Syntax:** Identical to Excel
- **Auto-generation:** Not automatic; must type formula manually
- **Limitations:** Only works with native Sheets Pivot Tables, not connected data sources
- **Performance:** Works well for standard Pivot Tables

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Auto-generated formulas | Yes (configurable) | No |
| OLAP PivotTable support | Yes | No |
| External data PivotTables | Yes | Limited |
| Syntax | Identical | Identical |
| Field name matching | Case-insensitive | Case-insensitive |

## Tips and Best Practices

1. **Use absolute references for pivot_table parameter:** `$A$3` not `A3`. This allows copying formulas to other cells without breaking the PivotTable reference.

2. **Match field names exactly:** Copy field names directly from the PivotTable to avoid typos. The data_field must match "Sum of Sales" not "Sales" if that's how it appears.

3. **Embrace auto-generation initially:** Let Excel generate GETPIVOTDATA when you click PivotTable cells. Study the generated formulas to learn the syntax, then modify as needed.

4. **Use cell references for dynamic criteria:** `=GETPIVOTDATA("Sales", $A$3, "Region", E2)` lets you change E2 to look up different regions without editing the formula.

5. **Add IFERROR for user-facing reports:** `=IFERROR(GETPIVOTDATA(...), "N/A")` prevents #REF! errors from appearing when criteria don't match PivotTable data.

6. **Create a PivotAnchor named range:** Define a name pointing to a stable PivotTable cell. Use `=GETPIVOTDATA("Field", PivotAnchor, ...)` for cleaner, more maintainable formulas.

7. **Test after PivotTable changes:** When you modify PivotTable structure (add fields, change grouping), verify GETPIVOTDATA formulas still work. Field names may change.

8. **For grand totals, omit field/item pairs:** `=GETPIVOTDATA("Sum of Sales", $A$3)` with no criteria returns the grand total.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VLOOKUP]]/[[XLOOKUP]] | General-purpose lookups | For non-PivotTable data or when simple cell references work |
| [[INDEX]]/[[MATCH]] | Flexible position-based lookups | For regular data tables; not effective for PivotTable dynamic layouts |
| [[SUMIFS]] | Sum with multiple criteria | For raw data calculations; GETPIVOTDATA uses pre-aggregated PivotTable data |
| [[CUBEVALUE]] | Extract OLAP cube data | For OLAP-based PivotTables with complex cube expressions |

### Commonly Used Together

**[[IFERROR]]** - Handle lookup failures

*Combined with GETPIVOTDATA for graceful error handling:*
```
=IFERROR(GETPIVOTDATA("Sum of Sales", $A$3, "Region", A10), 0)
```
Returns 0 (or custom message) when criteria don't match PivotTable data.

---

**[[TEXT]]** - Format values

*Combined with GETPIVOTDATA for formatted output:*
```
=TEXT(GETPIVOTDATA("Sum of Sales", $A$3, "Year", 2024), "$#,##0")
```
Formats the extracted value for display.

---

**[[IF]]** - Conditional logic

*Combined with GETPIVOTDATA for conditional extraction:*
```
=IF(ShowDetail, GETPIVOTDATA("Sum of Sales", $A$3, "Region", SelectedRegion), GETPIVOTDATA("Sum of Sales", $A$3))
```
Switches between detail and summary based on a toggle.

---

**[[DATE]]/[[YEAR]]/[[MONTH]]** - Date handling

*Combined with GETPIVOTDATA for date-based criteria:*
```
=GETPIVOTDATA("Sum of Sales", $A$3, "Year", YEAR(TODAY()), "Month", TEXT(TODAY(),"mmm"))
```
Dynamically references current year/month data.

---

**[[INDIRECT]]** - Dynamic references

*Combined with GETPIVOTDATA for dynamic field selection:*
```
=GETPIVOTDATA(INDIRECT("FieldName"), $A$3, "Region", "North")
```
Field name comes from a cell, enabling user-selectable metrics.

## Official Documentation

- **Microsoft Excel:** [GETPIVOTDATA function](https://support.microsoft.com/en-us/office/getpivotdata-function-8c083b99-a922-4ca0-af5e-3af55960761f)
- **Google Sheets:** [GETPIVOTDATA function](https://support.google.com/docs/answer/6167528)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2002 (XP) | Original implementation |
| Excel 2007 | Enhanced | Better OLAP PivotTable support |
| Excel 2010 | Enhanced | Improved auto-generation |
| Excel 365 | Current | Full feature set |
| Google Sheets | 2014 | Added support for Sheets Pivot Tables |

---

*Last updated: 2026-01-10*
