---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- data-filtering
- criteria-matching
- dynamic-arrays
- conditional-extraction
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Filter Data
- Filter Array
- Conditional Filter
- Data Extraction
tags:
- filter
- criteria
- array
- dynamic-arrays
- data-extraction
---

# FILTER

## Description

**FILTER** extracts rows from an array that meet specified criteria, returning a dynamic array of matching records. Think of it as a formula-based version of Excel's AutoFilter or Google Sheets' filter views—but far more powerful because it's dynamic, formula-driven, and can live anywhere in your spreadsheet. Give FILTER a dataset and a condition like "Sales > 1000" or "Region = 'East'", and it returns all rows that match. The result updates automatically as your data changes.

The power of FILTER lies in its flexibility with criteria. You can filter by a single condition, combine multiple conditions with AND logic (multiply the conditions), combine with OR logic (add the conditions), or use complex calculated criteria. Want rows where sales exceed the average? Where dates fall in the current month? Where text contains a specific substring? FILTER handles all of these. And because it returns an array, you can wrap it in SORT, UNIQUE, TAKE, or other functions to further process the results.

**Key behaviors to understand:** FILTER returns entire rows that match your criteria—not just the column you filtered on. If no rows match, FILTER returns a #CALC! error by default, but you can specify a custom if_empty value. The include parameter must be a TRUE/FALSE array with the same number of rows as the source array. When combining multiple criteria, remember: multiplication (*) means AND, addition (+) with comparison to >0 means OR.

**Platform availability:** FILTER is available in Microsoft Excel 365 and Excel 2021, as well as Google Sheets. It's not available in Excel 2019 or earlier. Google Sheets has had FILTER longer than Excel, so it's well-established on both platforms. The syntax and behavior are nearly identical, with minor differences in error handling. For older Excel, SUMPRODUCT with INDEX/MATCH or helper columns were the workarounds—far more complex than FILTER.

## Syntax

> [!f(x)] FILTER Syntax
>
> ```
> =FILTER(array, include, [if_empty])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array to filter. This is the data from which matching rows will be returned. |
| `include` | Yes | A Boolean array (TRUE/FALSE values) that indicates which rows to include. Must have the same number of rows as array. Typically created by a comparison like `Column>Value`. |
| `if_empty` | No | The value to return if no rows match the criteria. If omitted and no matches exist, returns #CALC! error. Can be a value, text, or array. |

### Return Value

Returns an array containing only the rows where the include parameter is TRUE. The result has the same number of columns as the source array and as many rows as there are TRUE values in include. Returns the if_empty value (or #CALC! if not specified) when no rows match.

## Examples

> [!f(x)] FILTER Examples

### Example 1: Basic Single-Criteria Filter
```
=FILTER(A2:D100, B2:B100="East")
```
**Result:** Returns all rows where column B equals "East"

**Explanation:** The most common FILTER pattern. The condition `B2:B100="East"` creates an array of TRUE/FALSE values. FILTER returns rows where this condition is TRUE. Note that the filter range (B2:B100) must match the data range (A2:D100) in row count.

---

### Example 2: Numeric Comparison Filter
```
=FILTER(A2:D100, C2:C100>1000)
```
**Result:** Returns all rows where column C exceeds 1000

**Explanation:** Works with any comparison operator: >, <, >=, <=, <>. This returns all records with values greater than 1000 in column C. The result includes all columns (A through D), not just column C.

---

### Example 3: Multiple Criteria with AND Logic
```
=FILTER(A2:D100, (B2:B100="East")*(C2:C100>1000))
```
**Result:** Returns rows where column B is "East" AND column C exceeds 1000

**Explanation:** Multiply conditions together for AND logic. TRUE * TRUE = 1 (truthy), TRUE * FALSE = 0 (falsy). Only rows satisfying BOTH conditions are returned. Parentheses around each condition are recommended for clarity.

---

### Example 4: Multiple Criteria with OR Logic
```
=FILTER(A2:D100, (B2:B100="East")+(B2:B100="West"))
```
**Result:** Returns rows where column B is "East" OR "West"

**Explanation:** Add conditions together for OR logic. TRUE + TRUE = 2, TRUE + FALSE = 1, FALSE + FALSE = 0. Any non-zero result is truthy. This returns rows matching either condition.

---

### Example 5: Handling Empty Results with if_empty
```
=FILTER(A2:D100, B2:B100="NonExistent", "No matches found")
```
**Result:** Returns "No matches found" if no rows match

**Explanation:** Without if_empty, no matches returns #CALC! error. Specifying if_empty provides a graceful fallback. Use "" for blank, 0 for zero, or descriptive text. This prevents error cascades in dependent formulas.

---

### Example 6: Filter with Cell Reference Criteria
```
=FILTER(A2:D100, B2:B100=F1)
```
**Result:** Returns rows where column B equals the value in cell F1

**Explanation:** Reference a cell for dynamic filtering. Users can change F1 to filter different values without editing the formula. Combine with data validation dropdowns for user-friendly filtering.

---

### Example 7: Date Range Filter
```
=FILTER(A2:E100, (D2:D100>=DATE(2024,1,1))*(D2:D100<=DATE(2024,12,31)))
```
**Result:** Returns rows where column D dates fall within 2024

**Explanation:** Filter by date ranges using AND logic with date comparisons. Ensure the date column contains actual dates (not text). This pattern is essential for time-based reporting.

---

### Example 8: Text Contains Filter (Partial Match)
```
=FILTER(A2:D100, ISNUMBER(SEARCH("widget", A2:A100)))
```
**Result:** Returns rows where column A contains "widget" (case-insensitive)

**Explanation:** SEARCH returns a number (position) if found, or #VALUE! if not. ISNUMBER converts this to TRUE/FALSE. This enables "contains" filtering. For case-sensitive, use FIND instead of SEARCH.

---

### Example 9: Filter and Sort Combined
```
=SORT(FILTER(A2:D100, B2:B100="East"), 3, -1)
```
**Result:** Returns East region rows, sorted by column 3 descending

**Explanation:** FILTER first extracts matching rows, then SORT orders them. This combination is powerful for creating sorted subsets—like "Top sales in the East region."

---

### Example 10: Filter with Calculated Criteria
```
=FILTER(A2:D100, C2:C100>AVERAGE(C2:C100))
```
**Result:** Returns rows where column C exceeds the average

**Explanation:** The criteria can include calculations. AVERAGE(C2:C100) is calculated once, then each row is compared against it. Returns all "above average" records.

---

### Example 11: Filter Top N Values
```
=FILTER(A2:D100, C2:C100>=LARGE(C2:C100, 10))
```
**Result:** Returns rows with the top 10 values in column C

**Explanation:** LARGE returns the 10th largest value. Filtering for >= that value returns all rows with top-10 values. Note: ties may return more than 10 rows. For exactly 10, combine with TAKE.

---

### Example 12: Filter Excluding Blanks
```
=FILTER(A2:D100, A2:A100<>"")
```
**Result:** Returns rows where column A is not blank

**Explanation:** Filters out rows with empty cells in the specified column. Essential for cleaning data that may have gaps or incomplete records.

---

### Example 13: Complex Multi-Column Criteria
```
=FILTER(Data, (Col1="A")*(Col2>10)+(Col1="B")*(Col2>20))
```
**Result:** Returns rows where (Col1="A" AND Col2>10) OR (Col1="B" AND Col2>20)

**Explanation:** Combine AND and OR logic for complex conditions. This pattern: each AND group is multiplied, then groups are added for OR. Returns rows meeting either compound condition.

---

### Example 14: Filter with ISBLANK for Missing Data
```
=FILTER(A2:E100, ISBLANK(C2:C100))
```
**Result:** Returns rows where column C is blank

**Explanation:** Find records with missing data using ISBLANK. Useful for data quality audits, identifying incomplete records, or finding items that need follow-up.

---

### Example 15: Filter One Column Based on Another
```
=FILTER(A2:A100, B2:B100="Active")
```
**Result:** Returns only column A values where column B is "Active"

**Explanation:** The array to return (A2:A100) can be different from the filter criteria column (B2:B100). This extracts just the IDs/names of active records, without returning all columns.

---

### Example 16: Filter with NOT Logic
```
=FILTER(A2:D100, NOT(B2:B100="Inactive"))
```
**Result:** Returns rows where column B is NOT "Inactive"

**Explanation:** Wrap conditions in NOT() to invert them. Alternatively, use `B2:B100<>"Inactive"`. NOT is clearer for complex inverted conditions.

---

### Example 17: Filter Returning Array for if_empty
```
=FILTER(A2:D100, B2:B100="Rare", {"ID","Name","Date","Value";"N/A","N/A","N/A",0})
```
**Result:** Returns headers and placeholder row if no matches

**Explanation:** if_empty can be an array, not just a single value. This returns a 2-row array with headers and placeholder data when no matches exist—useful for maintaining table structure.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#CALC!` | No rows match the criteria (and if_empty not specified) | Add an if_empty parameter: `FILTER(data, criteria, "No results")` |
| `#VALUE!` | Include array size doesn't match data array rows | Ensure both ranges have identical row counts. If data is A2:D100, criteria must reference 99 rows. |
| `#SPILL!` | Output range is blocked by existing data | Clear the cells where the result needs to spill, or move the formula. |
| `#NAME?` | Function not available in your Excel version | FILTER requires Excel 365/2021 or Google Sheets. |
| `Wrong results` | Text/number type mismatch | If column contains "100" (text) and you filter for 100 (number), no match. Check data types. |
| `All rows returned` | Criteria always TRUE | Review your condition—perhaps a typo or referencing wrong column. |

## Use Cases

### [[Dynamic Reporting Dashboard]]

**Scenario:** A sales dashboard allows users to select a region, product category, and date range via dropdown menus. FILTER extracts the relevant data for charts and summaries.

**Implementation:**
```
Cell Inputs:
  G1: Region dropdown (or "All")
  G2: Category dropdown (or "All")
  G3: Start Date
  G4: End Date

Filtered Data:
=FILTER(SalesData,
  (IF(G1="All", TRUE, Region=G1)) *
  (IF(G2="All", TRUE, Category=G2)) *
  (SaleDate>=G3) *
  (SaleDate<=G4),
  "No matching records"
)
```

**Business Application:** Interactive reporting, executive dashboards, self-service analytics. Users explore data without technical skills, and results update instantly.

**Technical Details:** The IF statements allow "All" selections to not filter that dimension. Each condition is multiplied for AND logic. Date comparisons work directly if SaleDate contains real dates.

---

### [[Customer Segmentation Analysis]]

**Scenario:** Marketing needs to extract customer lists meeting specific criteria—high-value customers, recent purchasers, or customers in specific segments.

**Implementation:**
```
High-Value Customers (>$10,000 lifetime value):
=FILTER(Customers, LifetimeValue>10000)

Recent Inactive (no purchase in 90 days, previously active):
=FILTER(Customers,
  (LastPurchase<TODAY()-90) * (TotalPurchases>0),
  "No matching customers"
)

Segment-Specific:
=FILTER(Customers, Segment=TargetSegment)
```

**Business Application:** Email campaigns, loyalty programs, churn prevention. Dynamic customer lists that update as customer data changes.

**Technical Details:** Combine FILTER with ROWS() to count matches: `=ROWS(FILTER(Customers, criteria))` gives segment size. Wrap in IFERROR for counts: `=IFERROR(ROWS(FILTER(...)), 0)`.

---

### [[Inventory Management]]

**Scenario:** Warehouse managers need to identify items requiring reorder, items with excess stock, or items matching specific criteria for picking lists.

**Implementation:**
```
Reorder Alert (stock below minimum):
=FILTER(Inventory, StockQty<ReorderPoint, "All items stocked")

Excess Stock (above maximum):
=FILTER(Inventory, StockQty>MaxStock)

Picking List (specific items needed):
=FILTER(Inventory, ISNUMBER(MATCH(SKU, OrderSKUs, 0)))
```

**Business Application:** Supply chain management, warehouse operations, procurement. Real-time visibility into inventory status.

**Technical Details:** The picking list example uses MATCH to check if each SKU appears in an order list. ISNUMBER converts match results to TRUE/FALSE for filtering.

---

### [[Data Validation and Quality Checks]]

**Scenario:** Analysts need to identify data quality issues: missing values, outliers, duplicates, or records failing validation rules.

**Implementation:**
```
Records with missing required fields:
=FILTER(Data,
  ISBLANK(RequiredCol1) + ISBLANK(RequiredCol2) + ISBLANK(RequiredCol3) > 0,
  "All records complete"
)

Statistical outliers (beyond 3 standard deviations):
=FILTER(Data,
  ABS(Values - AVERAGE(Values)) > 3*STDEV(Values),
  "No outliers detected"
)

Potential duplicates (same key value):
=FILTER(Data, COUNTIF(KeyColumn, KeyColumn)>1)
```

**Business Application:** Data quality assurance, ETL validation, audit preparation. Quickly surface records requiring attention.

**Technical Details:** The outlier check uses the empirical rule. The duplicate check uses COUNTIF—if a key appears more than once, that row is returned.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365, Excel 2021
- **Not available:** Excel 2019, Excel 2016, or earlier
- **Dynamic arrays:** Results automatically spill
- **Empty result:** Returns #CALC! error if no matches (unless if_empty specified)
- **Boolean handling:** TRUE=1, FALSE=0 for arithmetic
- **Workaround for older versions:** Complex SUMPRODUCT, INDEX/MATCH, or helper columns

### Google Sheets

- **Availability:** All versions (FILTER predates Excel's version)
- **Dynamic arrays:** Always supported
- **Empty result:** Similar behavior to Excel
- **Boolean handling:** Same as Excel
- **QUERY alternative:** Google Sheets' QUERY function offers SQL-like filtering as an alternative

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | 365/2021+ | All versions |
| QUERY alternative | No | Yes |
| Error value for empty | #CALC! | Empty or error (varies) |
| Array formula entry | Not required | Not required (modern Sheets) |

## Tips and Best Practices

1. **Always include if_empty:** Prevent #CALC! errors with a fallback: `=FILTER(data, criteria, "No results")`. Use "" for blank, 0 for zero, or descriptive text.

2. **Match row counts exactly:** If data is A2:D100 (99 rows), criteria must also reference exactly 99 rows. Off-by-one errors are common.

3. **Use parentheses for compound criteria:** `(A="X")*(B>10)` is clearer than `A="X"*B>10`. Parentheses prevent operator precedence issues.

4. **Multiply for AND, add for OR:** `(cond1)*(cond2)` means both must be true. `(cond1)+(cond2)>0` means either can be true.

5. **Reference cells for dynamic criteria:** Instead of hardcoding `="East"`, use `=G1` where G1 is a dropdown. Users can filter without editing formulas.

6. **Combine with SORT, UNIQUE, TAKE:** `=SORT(FILTER(...))` sorts results. `=UNIQUE(FILTER(...))` removes duplicates. `=TAKE(SORT(FILTER(...)), 10)` gets top 10.

7. **Use ISNUMBER(SEARCH()) for contains:** For partial text matching, `ISNUMBER(SEARCH("text", column))` returns TRUE where column contains "text".

8. **Count matches with ROWS():** `=ROWS(FILTER(data, criteria))` returns the count of matching rows. Wrap in IFERROR for safety.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[QUERY]] | SQL-like data extraction (Google Sheets only) | When you need GROUP BY, ORDER BY, or complex queries in Sheets |
| [[INDEX]]+[[MATCH]] | Return values based on lookup | When you need specific cells, not entire rows; or in older Excel |
| [[SUMPRODUCT]] | Sum products of arrays with conditions | When aggregating with conditions rather than extracting rows |
| [[SUMIFS]]/[[COUNTIFS]] | Conditional aggregation | When you need totals or counts, not raw data |

### Commonly Used Together

**[[SORT]]** - Sort array by columns

*Combined with FILTER for sorted, filtered results:*
```
=SORT(FILTER(Data, Sales>1000), 3, -1)
```
Filter first, then sort by column 3 descending.

---

**[[UNIQUE]]** - Return unique values

*Combined with FILTER to remove duplicates from filtered results:*
```
=UNIQUE(FILTER(Data, Region="East"))
```
Get unique records from the filtered subset.

---

**[[TAKE]]** - Return top/bottom N rows

*Combined with FILTER and SORT for top N:*
```
=TAKE(SORT(FILTER(Data, Active=TRUE), Sales, -1), 10)
```
Top 10 active records by sales.

---

**[[CHOOSECOLS]]** - Select specific columns

*Combined with FILTER to get specific columns from filtered rows:*
```
=CHOOSECOLS(FILTER(Data, Criteria), 1, 3, 5)
```
Return only columns 1, 3, 5 from filtered results.

---

**[[ROWS]]** - Count rows

*Combined with FILTER to count matches:*
```
=ROWS(FILTER(Data, Criteria))
```
Returns the count of matching rows (wrap in IFERROR for safety).

---

**[[IFERROR]]** - Handle errors

*Combined with FILTER for robust formulas:*
```
=IFERROR(FILTER(Data, Criteria), "No data")
```
Alternative to if_empty parameter for error handling.

## Official Documentation

- **Microsoft Excel:** [FILTER function](https://support.microsoft.com/en-us/office/filter-function-f4f7cb66-82eb-4767-8f7c-4877ad80c759)
- **Google Sheets:** [FILTER function](https://support.google.com/docs/answer/3093197)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | Original launch (2006) | FILTER has been available since Sheets' inception |
| Excel | Excel 365 (2018-2019) | Introduced with dynamic array functions |
| Excel | Excel 2021 | Included in perpetual license version |

---

*Last updated: 2026-01-10*
