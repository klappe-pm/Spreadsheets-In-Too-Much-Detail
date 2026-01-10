---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- aggregation
- filtering
- subtotals
- hidden-rows
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Subtotal Function
- Filter-Aware Sum
- Aggregate With Filters
tags:
- aggregation
- filtering
- subtotals
- sum
- average
- count
- hidden-rows
- data-analysis
---

# SUBTOTAL

## Description

**SUBTOTAL** performs aggregate calculations (sum, average, count, max, min, and more) while respecting filtered rows in your data. Unlike SUM, AVERAGE, and other standard functions that always calculate across all cells in a range regardless of visibility, SUBTOTAL automatically excludes rows hidden by AutoFilter. This makes it essential for data analysis where you need totals that update dynamically as you filter your data.

The function takes a function_num argument that specifies which calculation to perform (1-11 for various functions) and one or more ranges to calculate. Critically, function numbers 1-11 ignore only rows hidden by filtering, while function numbers 101-111 ignore ALL hidden rows, including those manually hidden. This distinction matters enormously—use 1-11 for filter-aware calculations in filtered tables, use 101-111 when you want to exclude any hidden rows regardless of how they were hidden.

**The 11 available functions:** SUBTOTAL provides access to 11 different aggregate functions through the function_num parameter: 1/101=AVERAGE, 2/102=COUNT, 3/103=COUNTA, 4/104=MAX, 5/105=MIN, 6/106=PRODUCT, 7/107=STDEV, 8/108=STDEVP, 9/109=SUM, 10/110=VAR, 11/111=VARP. The most commonly used are 9/109 (sum) and 1/101 (average), but all 11 functions are valuable for comprehensive data analysis.

**Another critical feature:** SUBTOTAL ignores other SUBTOTAL results within its range. If you have a subtotal in row 10 and the data continues in rows 11-20, a SUBTOTAL spanning rows 1-20 won't double-count by including the row 10 subtotal. This nesting capability allows for hierarchical subtotals without manual exclusion logic.

## Syntax

> [!f(x)] SUBTOTAL Syntax
>
> ```
> =SUBTOTAL(function_num, ref1, [ref2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `function_num` | Yes | A number 1-11 or 101-111 specifying which function to use. Numbers 1-11 ignore filtered rows only. Numbers 101-111 ignore all hidden rows (filtered or manually hidden). See function number reference table below. |
| `ref1` | Yes | The first range or reference for the calculation. Can be a cell range, named range, or individual cells. |
| `ref2, ...` | No | Additional ranges or references. Up to 254 additional arguments. Ranges can be non-contiguous. |

### Function Number Reference

| Function | 1-11 (Ignore filtered) | 101-111 (Ignore all hidden) | Description |
|----------|------------------------|----------------------------|-------------|
| AVERAGE | 1 | 101 | Average of visible values |
| COUNT | 2 | 102 | Count of visible numbers |
| COUNTA | 3 | 103 | Count of visible non-empty cells |
| MAX | 4 | 104 | Maximum visible value |
| MIN | 5 | 105 | Minimum visible value |
| PRODUCT | 6 | 106 | Product of visible values |
| STDEV | 7 | 107 | Sample standard deviation (visible) |
| STDEVP | 8 | 108 | Population standard deviation (visible) |
| SUM | 9 | 109 | Sum of visible values |
| VAR | 10 | 110 | Sample variance (visible) |
| VARP | 11 | 111 | Population variance (visible) |

### Return Value

Returns a numeric value based on the specified function. Returns 0 for SUM/PRODUCT of empty visible ranges. Returns #DIV/0! for AVERAGE/STDEV/VAR when no visible numeric values exist. Returns #VALUE! if function_num is invalid.

## Examples

> [!f(x)] SUBTOTAL Examples

### Example 1: Sum of Visible Cells (Filtered)
```
=SUBTOTAL(9, A1:A100)
```
**Result:** Sum of values in A1:A100, excluding rows hidden by filters

**Explanation:** Function 9 means SUM. Only rows visible after filtering are included. This is the most common SUBTOTAL use case.

---

### Example 2: Sum Ignoring All Hidden Rows
```
=SUBTOTAL(109, A1:A100)
```
**Result:** Sum of values, excluding any hidden rows (filtered or manually hidden)

**Explanation:** Function 109 extends the behavior to exclude manually hidden rows too, not just filtered ones.

---

### Example 3: Average of Visible Values
```
=SUBTOTAL(1, B2:B500)
```
**Result:** Average of visible numeric values in the range

**Explanation:** Function 1 calculates AVERAGE. Only visible rows contribute to both the sum and the count.

---

### Example 4: Count of Visible Numbers
```
=SUBTOTAL(2, C1:C100)
```
**Result:** Count of visible cells containing numbers

**Explanation:** Function 2 is COUNT—counts only numeric values in visible rows. Text and blanks are ignored.

---

### Example 5: Count of Visible Non-Empty Cells
```
=SUBTOTAL(3, D1:D100)
```
**Result:** Count of visible cells that aren't empty

**Explanation:** Function 3 is COUNTA—counts all visible non-empty cells, including text, numbers, and errors.

---

### Example 6: Maximum Visible Value
```
=SUBTOTAL(4, E1:E50)
```
**Result:** The largest value among visible cells

**Explanation:** Function 4 finds the MAX. Hidden rows' values don't participate in the comparison.

---

### Example 7: Minimum Visible Value
```
=SUBTOTAL(5, E1:E50)
```
**Result:** The smallest value among visible cells

**Explanation:** Function 5 finds the MIN. Useful for seeing the current lowest value in a filtered dataset.

---

### Example 8: Product of Visible Values
```
=SUBTOTAL(6, F1:F10)
```
**Result:** Product of all visible values multiplied together

**Explanation:** Function 6 is PRODUCT. Less common but useful for compound calculations in filtered data.

---

### Example 9: Standard Deviation (Sample)
```
=SUBTOTAL(7, G2:G100)
```
**Result:** Sample standard deviation of visible values

**Explanation:** Function 7 calculates STDEV using the sample formula (n-1 denominator). For filtered statistical analysis.

---

### Example 10: Standard Deviation (Population)
```
=SUBTOTAL(8, G2:G100)
```
**Result:** Population standard deviation of visible values

**Explanation:** Function 8 calculates STDEVP using the population formula (n denominator). Use when data represents entire population.

---

### Example 11: Multiple Ranges
```
=SUBTOTAL(9, A1:A50, C1:C50, E1:E50)
```
**Result:** Sum of visible values across three separate ranges

**Explanation:** SUBTOTAL accepts multiple ranges. All contribute to the single result, each respecting visibility rules.

---

### Example 12: Nesting Subtotals (Hierarchical Data)
```
' Row 10 contains: =SUBTOTAL(9, A2:A9)
' Row 20 contains: =SUBTOTAL(9, A11:A19)
' Grand total: =SUBTOTAL(9, A2:A20)
```
**Result:** Grand total equals sum of raw data, not counting intermediate subtotals twice

**Explanation:** SUBTOTAL automatically ignores other SUBTOTAL results within its range, enabling hierarchical subtotaling without double-counting.

---

### Example 13: Variance Calculations
```
=SUBTOTAL(10, H1:H100)  ' Sample variance
=SUBTOTAL(11, H1:H100)  ' Population variance
```
**Result:** Variance of visible values

**Explanation:** Functions 10 (VAR) and 11 (VARP) provide variance calculations with filter awareness.

---

### Example 14: Comparing 9 vs 109 with Manual Hidden Rows
```
' With rows 5-10 manually hidden (not filtered):
=SUBTOTAL(9, A1:A20)    ' Includes rows 5-10
=SUBTOTAL(109, A1:A20)  ' Excludes rows 5-10
```
**Result:** Different totals when manual hiding is involved

**Explanation:** Function 9 only ignores filter-hidden rows. Function 109 ignores any hidden row. This distinction is critical for correct results.

---

### Example 15: Used in Tables
```
' In an Excel Table, below the data:
=SUBTOTAL(9, [SalesAmount])
```
**Result:** Sum of visible SalesAmount values

**Explanation:** SUBTOTAL works seamlessly with structured table references. When the table is filtered, the subtotal automatically updates.

---

### Example 16: Checking Filter Status
```
=IF(SUBTOTAL(3, A:A) < COUNTA(A:A), "Data is filtered", "No filter active")
```
**Result:** Indicates whether filtering has reduced visible rows

**Explanation:** Comparing SUBTOTAL(3,...) to COUNTA reveals if any rows are hidden by filters. This trick helps users know their data state.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid function_num (not 1-11 or 101-111) | Use a valid function number. Check spelling if using named constant. |
| `#DIV/0!` | AVERAGE, STDEV, or VAR with no visible numeric values | Wrap in IFERROR, or ensure range has visible data before calculating. |
| `#REF!` | Reference to deleted cells | Update range references. |
| `Wrong total` | Using 1-11 when manual hiding exists | Switch to 101-111 if you want to exclude all hidden rows. |
| `Total includes filtered rows` | Formula not using SUBTOTAL | Replace SUM, AVERAGE, etc. with appropriate SUBTOTAL function. |
| `Unexpected zero` | Range contains only hidden rows | This is correct behavior—no visible values means result is 0 (for SUM). |

## Use Cases

### [[Filtered Data Analysis]]

**Scenario:** Create totals and statistics that update automatically when users filter data.

**Implementation:**
```
' At the bottom of a filtered data table:
=SUBTOTAL(9, B2:B1000)     ' Sum of visible sales
=SUBTOTAL(1, C2:C1000)     ' Average of visible prices
=SUBTOTAL(3, A2:A1000)     ' Count of visible records
=SUBTOTAL(4, D2:D1000)     ' Max of visible values
=SUBTOTAL(5, D2:D1000)     ' Min of visible values
```

**Business Application:** Sales reports, inventory lists, customer databases—any filtered dataset where users need real-time summary statistics that reflect current filter settings.

**Technical Details:** Place SUBTOTAL formulas outside the filtered range (below the data) or in a separate summary area. They'll update automatically as filters change.

---

### [[Hierarchical Subtotals and Grand Totals]]

**Scenario:** Create multi-level subtotals (by region, by product category) without double-counting.

**Implementation:**
```
' Regional subtotals in scattered rows:
Row 10: =SUBTOTAL(9, B2:B9)    ' Region 1 subtotal
Row 20: =SUBTOTAL(9, B11:B19)  ' Region 2 subtotal
Row 30: =SUBTOTAL(9, B21:B29)  ' Region 3 subtotal

' Grand total spanning all rows:
Row 31: =SUBTOTAL(9, B2:B30)   ' Automatically excludes subtotal rows
```

**Business Application:** Financial reports with department subtotals and company-wide totals, inventory by warehouse and total, or any grouped reporting structure.

**Technical Details:** SUBTOTAL's automatic exclusion of nested SUBTOTAL results eliminates complex range adjustments. The grand total only counts raw data once.

---

### [[Dashboard Metrics with Hidden Sections]]

**Scenario:** Display summary statistics in a dashboard where some detail rows may be hidden manually.

**Implementation:**
```
=SUBTOTAL(109, DataRange)     ' Sum excluding all hidden
=SUBTOTAL(101, DataRange)     ' Average excluding all hidden
=SUBTOTAL(103, DataRange)     ' Count excluding all hidden
```

**Business Application:** Executive dashboards where detail sections can be collapsed, financial models with optional scenario rows, or templates where users hide irrelevant sections.

**Technical Details:** Using 101-111 functions ensures hidden detail rows don't affect summary statistics, regardless of whether rows were hidden via filter or manual hiding.

---

### [[Data Validation and Quality Checking]]

**Scenario:** Verify data completeness and detect filter status across worksheets.

**Implementation:**
```
' Check if any filtering is active:
=SUBTOTAL(3, A:A) < COUNTA(A:A)   ' TRUE if filtered

' Count visible errors:
=SUBTOTAL(3, A1:A100) - SUBTOTAL(2, A1:A100)  ' Non-numeric visible cells

' Percentage of data visible:
=SUBTOTAL(3, A1:A100) / COUNTA(A1:A100)
```

**Business Application:** Data quality reports, audit checks confirming all data is visible, and user interfaces that warn about active filters.

**Technical Details:** The difference between SUBTOTAL results and regular function results reveals hidden data presence.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Function numbers:** Full support for 1-11 and 101-111
- **AutoFilter integration:** Perfect integration with filter arrows
- **Manual hiding:** 1-11 include manually hidden; 101-111 exclude
- **Pivot Tables:** SUBTOTAL typically not used (pivots have own subtotals)
- **AGGREGATE alternative:** Excel 2010+ offers AGGREGATE with more options

### Google Sheets
- **Availability:** All versions
- **Function numbers:** Supports 1-11 and 101-111
- **Filter integration:** Works with Google Sheets filters
- **Manual hiding:** Same behavior as Excel
- **Syntax:** Identical to Excel
- **Performance:** Comparable to Excel

### Key Differences
- Both platforms implement SUBTOTAL identically
- Excel's AGGREGATE function (not available in Sheets) provides additional options like ignoring errors
- Function numbers and behavior are consistent across platforms
- No localization differences—function name is SUBTOTAL everywhere

## Tips and Best Practices

1. **Memorize key function numbers:** 9=SUM, 1=AVERAGE, 3=COUNTA are the most common. Write them on a sticky note until memorized.

2. **Use 101-111 for print layouts:** When hiding rows for printing, use 101-111 to get accurate totals of visible rows only.

3. **Add 100 to ignore all hidden:** If you know 9 is SUM, then 109 is SUM-ignoring-all-hidden. Same pattern for all functions.

4. **Place totals outside data range:** Put SUBTOTAL formulas below your data or in a separate summary area to avoid them being hidden by filters.

5. **Combine with conditional formatting:** Highlight when SUBTOTAL differs from regular SUM to alert users that data is filtered.

6. **Use for data validation:** Compare `SUBTOTAL(3, range)` to `COUNTA(range)` to detect if any rows are hidden.

7. **Leverage automatic SUBTOTAL exclusion:** When building hierarchical reports, trust that grand totals won't double-count intermediate subtotals.

8. **Consider AGGREGATE in Excel 2010+:** AGGREGATE offers additional options (ignore errors, nested functions) that SUBTOTAL doesn't provide.

9. **Document your function numbers:** In complex workbooks, add a legend or comments explaining which function numbers are used and why.

10. **Test with filters active:** Always verify SUBTOTAL behavior with actual filters applied, not just with all data visible.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AGGREGATE]] | Extended version with error handling options (Excel only) | When you need to ignore errors or use more complex aggregation |
| [[SUM]] | Simple sum without filter awareness | When you always want the total regardless of visibility |
| [[AVERAGE]] | Simple average without filter awareness | When filter state shouldn't affect the average |
| [[SUMIF]] / [[SUMIFS]] | Conditional sum based on criteria | When you need criteria-based summing rather than visibility-based |

### Commonly Used Together

**[[FILTER]] (Dynamic Array)** - Create filtered views

*Works alongside SUBTOTAL:*
```
=SUBTOTAL(9, FILTER(Data, Criteria))
```
In dynamic array Excel, FILTER creates a filtered subset that SUBTOTAL can aggregate.

---

**[[IFERROR]]** - Handle potential errors

*Wrap SUBTOTAL for robustness:*
```
=IFERROR(SUBTOTAL(1, A1:A100), 0)
```
Handles #DIV/0! errors when all rows are hidden and AVERAGE is requested.

---

**[[COUNTA]]** - Count non-empty cells

*Compare to detect filtering:*
```
=IF(SUBTOTAL(3, A1:A100) < COUNTA(A1:A100), "Filtered", "All visible")
```
Reveals whether any rows are hidden by filters.

---

**[[OFFSET]]** - Create dynamic ranges

*Build dynamic SUBTOTAL ranges:*
```
=SUBTOTAL(9, OFFSET(A1, 0, 0, COUNTA(A:A), 1))
```
Creates a range that adjusts to data size, then applies SUBTOTAL.

---

**[[INDEX]]** - Reference specific cells

*Target specific columns in SUBTOTAL:*
```
=SUBTOTAL(9, INDEX(DataTable, 0, 3))
```
Selects a specific column from a table for the SUBTOTAL calculation.

## Official Documentation

- **Microsoft Excel:** [SUBTOTAL function](https://support.microsoft.com/en-us/office/subtotal-function-7b027003-f060-4ade-9040-e478765b9939)
- **Google Sheets:** [SUBTOTAL function](https://support.google.com/docs/answer/3093649)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original function with 1-11 |
| Excel 2003 | Added 101-111 | Extended to ignore all hidden rows |
| Excel 2010 | AGGREGATE added | More powerful alternative introduced |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
