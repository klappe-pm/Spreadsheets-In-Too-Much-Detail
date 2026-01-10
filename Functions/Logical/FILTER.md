---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- logical
- dynamic-arrays
subTopics:
- data-filtering
- conditional-extraction
- array-formulas
- criteria-matching
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Filter Data
- Array Filter
- Dynamic Filter
- Conditional Extraction
tags:
- filtering
- dynamic-arrays
- data-extraction
- conditional
- spill-functions
---

# FILTER

## Description

**FILTER** is a game-changing dynamic array function that extracts rows or columns from a range based on one or more criteria. Unlike legacy filtering methods that required helper columns, VLOOKUP tricks, or manual filter operations, FILTER returns matching results directly in your formula, automatically spilling across as many cells as needed. Think of it as a programmable, formula-based version of Excel's AutoFilter or Google Sheets' filter view, but one that updates instantly and can be embedded in other calculations.

The power of FILTER lies in its elegance: define what data you want to extract (the array) and what conditions must be true (the include criteria). FILTER evaluates each row against your conditions and returns only the rows where all conditions are TRUE. Need all sales over $10,000? `=FILTER(Sales, Amounts>10000)`. Need January transactions for customer "Acme"? `=FILTER(Data, (Month=1)*(Customer="Acme"))`. The results appear instantly and update automatically when source data changes, eliminating the need to reapply manual filters.

**Critical gotcha for multi-criteria filtering:** To combine multiple conditions, you must use array multiplication (*) for AND logic or addition (+) for OR logic, not the AND() and OR() functions directly. AND() and OR() collapse arrays to single values, breaking FILTER's row-by-row evaluation. The pattern `(condition1)*(condition2)` means "both must be true" (TRUE*TRUE=1, anything else=0). The pattern `(condition1)+(condition2)` means "either can be true" (any sum >0 is truthy). This syntax trips up nearly everyone at first.

**Platform availability:** FILTER is available in Excel 365, Excel 2021, Excel for the web, and Google Sheets. It is NOT available in Excel 2019 or earlier desktop versions. Google Sheets has had FILTER since 2006, making it one of the earliest implementations. The function behaves nearly identically across platforms, with minor differences in the if_empty parameter handling and error behavior when no matches are found.

## Syntax

> [!f(x)] FILTER Syntax
>
> ```
> =FILTER(array, include, [if_empty])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array to filter. This is the data you want to extract from. Can be a single column, single row, or multi-column/multi-row range. FILTER returns entire rows (or columns) from this array based on the include criteria. |
| `include` | Yes | A Boolean array (TRUE/FALSE values) that defines which rows/columns to include. Must have the same height (for row filtering) or width (for column filtering) as the array. Typically created by a comparison like `A1:A100>50` which produces an array of TRUE/FALSE values. |
| `if_empty` | No | The value to return if no rows match the filter criteria. If omitted and no matches exist, Excel returns #CALC! error, Google Sheets returns #N/A. Common values: "" (empty string), "No results", 0, or NA(). |

### Return Value

Returns an array (spilling results) containing all rows from the array where the corresponding include value is TRUE. If filtering columns, returns all columns where include is TRUE. The result dynamically resizes based on how many items match the criteria. If no items match and if_empty is not specified, returns an error (#CALC! in Excel, #N/A in Google Sheets).

## Examples

> [!f(x)] FILTER Examples

### Example 1: Basic Single-Condition Filter
```
=FILTER(A2:D20, B2:B20="Active")
```
**Result:** All rows where column B equals "Active"

**Explanation:** The simplest FILTER pattern. B2:B20="Active" creates an array of TRUE/FALSE values matching the height of your data. FILTER returns complete rows (columns A through D) only where the corresponding value is TRUE. If rows 3, 7, and 15 have "Active" in column B, you get those three complete rows in your result.

---

### Example 2: Numeric Comparison Filter
```
=FILTER(A2:E100, E2:E100>1000)
```
**Result:** All rows where column E (e.g., sales amount) exceeds 1000

**Explanation:** FILTER works with any comparison operator: >, <, >=, <=, =, <>. Here we extract all high-value transactions. The result automatically includes all columns (A through E) for qualifying rows. The spill range adjusts dynamically as data changes.

---

### Example 3: Multiple Conditions with AND Logic
```
=FILTER(A2:F50, (B2:B50="West")*(D2:D50>500))
```
**Result:** Rows where region is "West" AND amount exceeds 500

**Explanation:** Multiply conditions for AND logic. Each condition produces TRUE (1) or FALSE (0). When multiplied: TRUE*TRUE=1 (included), TRUE*FALSE=0 (excluded), FALSE*anything=0 (excluded). Only rows passing BOTH tests make it through. Parentheses around each condition are required.

---

### Example 4: Multiple Conditions with OR Logic
```
=FILTER(A2:D30, (C2:C30="Urgent")+(C2:C30="Critical"))
```
**Result:** Rows where priority is "Urgent" OR "Critical"

**Explanation:** Add conditions for OR logic. TRUE+TRUE=2, TRUE+FALSE=1, FALSE+TRUE=1, FALSE+FALSE=0. Any result >0 is truthy, so rows matching either condition are included. This is useful for extracting records that match any of several criteria.

---

### Example 5: Combining AND and OR Logic
```
=FILTER(A2:E100, (B2:B100="Active")*((C2:C100="Gold")+(C2:C100="Platinum")))
```
**Result:** Active customers who are either Gold OR Platinum tier

**Explanation:** Complex logic uses nested parentheses. The inner addition handles OR (Gold or Platinum), then multiplication applies AND (must be Active AND one of those tiers). Read it as: "Active AND (Gold OR Platinum)". Structure carefully with parentheses.

---

### Example 6: Handling No Results with if_empty
```
=FILTER(A2:C20, B2:B20>9999, "No qualifying records")
```
**Result:** Matching rows, or "No qualifying records" if none match

**Explanation:** Without if_empty, no matches cause an error. The third parameter provides a fallback value. Common choices: "" for blank, "No results" for clear messaging, 0 for numeric contexts, or NA() if you want to explicitly show no data. Essential for user-facing dashboards.

---

### Example 7: Filter with Date Criteria
```
=FILTER(A2:F100, (D2:D100>=DATE(2024,1,1))*(D2:D100<=DATE(2024,3,31)))
```
**Result:** Records from Q1 2024 (January through March)

**Explanation:** Date comparisons work like numeric comparisons since dates are stored as serial numbers. This extracts all records where column D (a date column) falls within Q1 2024. Use DATE() function to create unambiguous date values for comparison.

---

### Example 8: Filtering and Sorting Combined (Nested Functions)
```
=SORT(FILTER(A2:D50, C2:C50="Electronics"), 4, -1)
```
**Result:** Electronics products sorted by column 4 (e.g., price) in descending order

**Explanation:** FILTER results can feed directly into other dynamic array functions. Here SORT takes the filtered results and orders them. This combination replaces complex manual workflows: filter data, copy to new location, sort. All happens automatically in one formula.

---

### Example 9: Filter with Wildcards Using SEARCH
```
=FILTER(A2:D100, ISNUMBER(SEARCH("Corp", A2:A100)))
```
**Result:** All rows where column A contains "Corp" anywhere in the text

**Explanation:** FILTER's include parameter doesn't directly support wildcards, but wrapping SEARCH in ISNUMBER creates a TRUE/FALSE array. SEARCH returns a position number if found (truthy via ISNUMBER) or #VALUE! if not found (falsy via ISNUMBER returning FALSE). Use FIND for case-sensitive matching.

---

### Example 10: Returning Specific Columns from Filtered Data
```
=FILTER(CHOOSECOLS(A2:F100, 1, 3, 5), D2:D100="Complete")
```
**Result:** Only columns 1, 3, and 5 from rows where status is "Complete"

**Explanation:** If you don't need all columns, use CHOOSECOLS (or INDEX) to select specific columns before or after filtering. Here we get columns A, C, and E for completed records. Alternatively: `=CHOOSECOLS(FILTER(A2:F100, D2:D100="Complete"), 1, 3, 5)` - order doesn't matter here.

---

### Example 11: Counting Filtered Results
```
=ROWS(FILTER(A2:D100, B2:B100>500, ""))
```
**Result:** Number of rows matching the criteria

**Explanation:** Wrap FILTER in ROWS to count matches. The if_empty of "" prevents errors when no matches exist (ROWS of "" returns 1, so subtract 1 or use IFERROR for zero). Alternative: COUNTIF is simpler for just counting, but this pattern is useful when you need both count and data.

---

### Example 12: Filter Using Cell Reference for Criteria
```
=FILTER(A2:E100, B2:B100=G1, "No matches for "&G1)
```
**Result:** Rows matching the value in G1, with dynamic error message

**Explanation:** Referencing a cell for criteria creates interactive filters. Type a value in G1, and the filtered list updates instantly. The if_empty message includes the search term for clarity. Build dropdown lists in G1 for user-friendly filtering interfaces.

---

### Example 13: Filter Top N Results
```
=FILTER(A2:D50, B2:B50>=LARGE(B2:B50, 5))
```
**Result:** All rows where column B is among the top 5 values

**Explanation:** LARGE returns the Nth largest value. Comparing each row to that threshold creates a dynamic "top N" filter. Note: If there are ties at the threshold, you may get more than N results. For exactly N results regardless of ties, nest in TAKE: `=TAKE(SORT(FILTER(...), col, -1), 5)`.

---

### Example 14: Cross-Sheet Filtering
```
=FILTER(Data!A2:F100, Data!C2:C100=Summary!A1)
```
**Result:** Rows from Data sheet where column C matches the value on Summary sheet

**Explanation:** FILTER works across sheets seamlessly. The array and include references can point to any sheet. This enables centralized summary sheets that pull filtered data from multiple source sheets. Ensure both ranges reference the same sheet for the include criteria.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#CALC!` (Excel) or `#N/A` (Sheets) | No rows match the filter criteria | Use the if_empty parameter: `=FILTER(array, include, "No matches")` to provide a fallback value. |
| `#VALUE!` | The include array has different dimensions than the source array | Ensure the include range has exactly the same number of rows (for row filtering) as the array. Check for extra or missing rows. |
| `#SPILL!` | The spill range is blocked by existing cell content | Clear the cells where FILTER needs to output results. FILTER requires empty cells below/to the right of the formula cell. |
| `#REF!` | Array reference is invalid or deleted | Check that the referenced range still exists and hasn't been deleted or moved. |
| `Unexpected results with AND()` | Using AND() or OR() functions instead of array operators | Replace `AND(A:A>5, B:B="X")` with `(A:A>5)*(B:B="X")`. AND/OR collapse arrays and don't work row-by-row. |
| `Formula returns all rows` | Include condition always evaluates to TRUE | Check your criteria expression. Common mistake: `=FILTER(A:D, B:B="Active")` when no cells contain exactly "Active" (check for spaces, case issues). |
| `#NAME?` error | Using FILTER in unsupported Excel version | FILTER requires Excel 365, Excel 2021, or Excel Online. Not available in Excel 2019 or earlier. |

## Use Cases

### [[Dynamic Sales Dashboard Filtering]]

**Scenario:** A sales manager needs an interactive dashboard where selecting a sales rep, region, or date range instantly shows relevant transactions without manual filtering.

**Implementation:**
```
=FILTER(SalesData, (SalesData[Rep]=RepDropdown)*(SalesData[Region]=RegionDropdown)*(SalesData[Date]>=StartDate)*(SalesData[Date]<=EndDate), "No transactions match criteria")
```

**Business Application:** Interactive dashboards empower managers to self-serve their data needs without requesting IT reports. Each combination of dropdown selections yields instant results, enabling rapid analysis during sales meetings or forecast discussions. The if_empty message prevents confusion when filter combinations return no data.

**Technical Details:** Use Data Validation dropdowns for RepDropdown and RegionDropdown cells. Name the date input cells (StartDate, EndDate) for formula clarity. The Table reference syntax (SalesData[Rep]) auto-expands as new data is added. Consider UNIQUE() to populate dropdown options dynamically from the data itself.

---

### [[Real-Time Exception Reporting]]

**Scenario:** Operations team needs a constantly-updated list of orders that are past due, over budget, or flagged for review.

**Implementation:**
```
=FILTER(Orders, (Orders[DueDate]<TODAY())+(Orders[Actual]>Orders[Budget])+(Orders[Flag]="Review"), "No exceptions - all orders on track")
```

**Business Application:** Exception-based management lets teams focus attention where it's needed rather than reviewing every order. This living exception report updates automatically each day (TODAY() recalculates), ensuring past-due items surface immediately. Operations meetings can start with this list rather than hunting for problems.

**Technical Details:** The OR logic (using +) surfaces orders meeting ANY exception criteria. Each condition represents a different type of exception. Consider adding SORT to prioritize by severity or due date: `=SORT(FILTER(...), date_col, 1)` to show most urgent first.

---

### [[Customer Segmentation Analysis]]

**Scenario:** Marketing needs to extract customer lists for targeted campaigns based on purchase history, account status, and engagement metrics.

**Implementation:**
```
=FILTER(Customers, (Customers[TotalPurchases]>=1000)*(Customers[Status]="Active")*(Customers[LastContact]<TODAY()-90), "No customers match segmentation criteria")
```

**Business Application:** Precise customer targeting improves campaign ROI. This formula identifies high-value active customers who haven't been contacted in 90+ days - ideal for re-engagement campaigns. Marketing can export this dynamic list directly to email platforms, always getting current data.

**Technical Details:** The 90-day calculation (TODAY()-90) creates a rolling window that updates daily. For more segments, create multiple FILTER formulas with different criteria, or use VSTACK to combine multiple segments: `=VSTACK(FILTER(segment1), FILTER(segment2))`.

---

### [[Inventory Reorder Report]]

**Scenario:** Warehouse manager needs a current list of products below minimum stock levels, excluding discontinued items, for reorder processing.

**Implementation:**
```
=FILTER(Inventory, (Inventory[QtyOnHand]<=Inventory[ReorderPoint])*(Inventory[Status]<>"Discontinued"), "All products sufficiently stocked")
```

**Business Application:** Automated reorder triggers prevent stockouts without requiring manual inventory review. This list feeds directly into purchase order creation, with columns for product ID, current quantity, reorder quantity, and vendor information all included. Just-in-time inventory management becomes possible.

**Technical Details:** Using <= instead of < catches items exactly at the reorder point, not just below. The <>"Discontinued" filter prevents ordering obsolete items. Extend with supplier info: wrap in another formula to VLOOKUP vendor contacts for immediate PO creation.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365 (subscription), Excel 2021 (perpetual), Excel Online
- **NOT available:** Excel 2019, Excel 2016, or earlier versions
- **Error when no matches:** Returns #CALC! error if no rows match and if_empty is omitted
- **Spill behavior:** Results spill automatically; #SPILL! error if blocked
- **Array size limits:** Maximum 1,048,576 rows, though practical limits depend on performance
- **if_empty handling:** Can return any value type including arrays

### Google Sheets

- **Availability:** All versions since 2006 (one of the earliest implementations)
- **Error when no matches:** Returns #N/A error if no rows match and if_empty is omitted
- **Spill behavior:** Native array formula support; results expand automatically
- **Array size limits:** 10 million cells maximum per spreadsheet
- **if_empty handling:** Equivalent functionality to Excel
- **Performance:** Generally handles large FILTER operations efficiently

### Key Difference Alert

The primary cross-platform concern is **error handling when no matches exist**: Excel returns #CALC!, Sheets returns #N/A. Always use the if_empty parameter for consistent behavior. Also note that FILTER doesn't exist in older Excel versions (2019 and earlier). For backwards compatibility with older Excel, use legacy approaches like INDEX/MATCH with array formulas or helper columns with AGGREGATE.

## Tips and Best Practices

1. **Always use if_empty parameter:** Don't let your formulas error when no data matches. Provide meaningful fallback text like "No data matches criteria" or an empty string "" for clean reports.

2. **Memorize the AND/OR pattern:** Conditions multiplied (*) = AND logic, conditions added (+) = OR logic. Parentheses around each condition are essential: `(A1:A10>5)*(B1:B10="Yes")`.

3. **Use named ranges for readability:** `=FILTER(Orders, (Region=SelectedRegion)*(Status="Open"))` is much clearer than `=FILTER(A2:F100, (B2:B100=$H$1)*(D2:D100="Open"))`.

4. **Combine with SORT for ordered results:** FILTER returns data in source order. Wrap in SORT for custom ordering: `=SORT(FILTER(...), column_index, order)`.

5. **Watch for text matching issues:** Excel's FILTER is case-insensitive for text, but Google Sheets is case-sensitive. Use UPPER() or LOWER() on both sides for consistent behavior.

6. **Keep criteria cells visible:** If filtering based on cell references (dropdown selections), keep those cells visible near the output so users understand what's being filtered.

7. **Handle spill blocking proactively:** Keep cells below/to the right of FILTER formulas empty. If your filtered list might grow, ensure adequate space. Consider placing FILTER output in its own area.

8. **Use ROWS() to count matches:** `=ROWS(FILTER(..., ""))` gives you the count of matching rows. Subtract 1 if using empty string for if_empty.

9. **Filter then aggregate for conditional calculations:** `=SUM(FILTER(Sales, Region="West"))` sums only West region sales. Sometimes clearer than SUMIF for complex conditions.

10. **Test with edge cases:** What happens when one row matches? Zero rows match? All rows match? The criteria cell is blank? Test these scenarios before deploying.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SORT]] | Sorts an array by one or more columns | Use with FILTER to order filtered results: `=SORT(FILTER(...))` |
| [[UNIQUE]] | Returns unique values from a range | When you need distinct values rather than filtered rows |
| [[QUERY]] | Google Sheets SQL-like data manipulation | Sheets only; when you need SQL-style filtering, grouping, and aggregation in one formula |
| [[SUMIF]] / [[COUNTIF]] | Conditional sum/count | When you only need aggregated results, not the actual rows |
| [[XLOOKUP]] | Lookup with flexible matching | When finding single values rather than filtering multiple rows |

### Commonly Used Together

**[[SORT]]** - Orders the filtered results

*Sorting filtered data by date descending:*
```
=SORT(FILTER(Orders, Orders[Status]="Open"), 4, -1)
```
FILTER extracts open orders; SORT orders them by column 4 (e.g., date) in descending order. Most recent orders appear first.

---

**[[UNIQUE]]** - Extracts distinct values from filtered data

*Getting unique customers from filtered transactions:*
```
=UNIQUE(FILTER(INDEX(Transactions,,3), Transactions[Region]="West"))
```
Filters to West region, extracts the customer column (3), then returns only unique customer names.

---

**[[SORTBY]]** - Sorts by a column not in the output

*Filtering and sorting by an external ranking:*
```
=SORTBY(FILTER(Products, Products[Category]="Electronics"), FILTER(Prices, Categories="Electronics"), -1)
```
Sorts filtered products by corresponding prices without including prices in the output.

---

**[[IFERROR]]** - Provides alternative for errors

*Graceful handling beyond if_empty:*
```
=IFERROR(FILTER(Data, Criteria), "Error in filter criteria")
```
Catches errors beyond just "no matches" - useful when criteria references might be broken.

---

**[[VSTACK]]** / [[HSTACK]] - Combines multiple filter results

*Merging filtered results from multiple criteria:*
```
=VSTACK(FILTER(Data, Region="East"), FILTER(Data, Region="West"))
```
Returns both East and West regions stacked vertically. Alternative to OR logic when you want distinct sections.

## Official Documentation

- **Microsoft Excel:** [FILTER function](https://support.microsoft.com/en-us/office/filter-function-f4f7cb66-82eb-4767-8f7c-4877ad80c759)
- **Google Sheets:** [FILTER function](https://support.google.com/docs/answer/3093197)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2006 | Original launch - one of the earliest FILTER implementations |
| Excel 365 | September 2018 | Introduced with dynamic array functions |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel Online | 2018+ | Same functionality as desktop Excel 365 |
| Excel 2019 | Not supported | Requires 365 or 2021 for FILTER function |

---

*Last updated: 2026-01-10*
