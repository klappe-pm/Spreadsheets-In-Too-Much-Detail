---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- modern-lookup
- dynamic-arrays
- bidirectional-search
- approximate-matching
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Modern Lookup
- Advanced Lookup
- Flexible Lookup
- XL Lookup
tags:
- lookup
- reference
- xlookup
- search
- match-modes
- dynamic-arrays
- excel-365
---

# XLOOKUP

## Description

**XLOOKUP** is Microsoft's modern replacement for VLOOKUP, HLOOKUP, and INDEX/MATCH combinations, offering unprecedented flexibility in data retrieval. `=XLOOKUP(lookup_value, lookup_array, return_array)` searches for a value in one array and returns a corresponding value from another array. Unlike its predecessors, XLOOKUP can search left-to-right or right-to-left, handles vertical and horizontal data equally, returns arrays of multiple columns/rows, and includes built-in error handling. Introduced in Excel 365 and Excel 2021, XLOOKUP represents the most significant evolution in spreadsheet lookup capabilities in decades.

The function's power lies in its six distinct matching behaviors controlled by the `match_mode` parameter: exact match (0), exact or next smaller (-1), exact or next larger (1), and wildcard matching (2). Combined with four `search_mode` options—first-to-last (1), last-to-first (-1), binary search ascending (2), and binary search descending (-2)—XLOOKUP handles virtually any lookup scenario. This flexibility eliminates the need to sort data for approximate matches or use workarounds for reverse lookups.

**XLOOKUP defaults to exact match, unlike VLOOKUP's approximate match default.** This alone prevents countless lookup errors where VLOOKUP returned wrong values because users forgot to specify FALSE. XLOOKUP also includes an `if_not_found` parameter for custom error messages, replacing the need for IFERROR wrappers. When the lookup value isn't found, you can return a meaningful message like "Product not in catalog" instead of cryptic #N/A errors.

**XLOOKUP returns entire rows or columns, not just single cells.** The return_array can span multiple columns, and XLOOKUP returns all of them as a spilling array. This enables powerful multi-column lookups in a single formula: `=XLOOKUP(ID, A:A, B:E)` returns columns B through E for the matching row. Combined with dynamic arrays, XLOOKUP fundamentally changes how lookup formulas are constructed, making complex data retrieval operations simpler and more intuitive.

## Syntax

> [!f(x)] XLOOKUP Syntax
>
> ```
> =XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_mode], [search_mode])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `lookup_value` | Yes | The value to search for. Can be a cell reference, text, number, or formula result. Supports wildcards (* and ?) when match_mode is 2. |
| `lookup_array` | Yes | The array or range to search. Must be a single row or column. This is where XLOOKUP looks for the lookup_value. |
| `return_array` | Yes | The array or range from which to return values. Can be a single column, single row, or multiple columns/rows. Must have same number of rows (for vertical) or columns (for horizontal) as lookup_array. |
| `if_not_found` | No | Value to return if no match is found. If omitted and no match exists, returns #N/A. Can be text, number, formula, or empty string "". |
| `match_mode` | No | Specifies the match type. 0 = exact match (default), -1 = exact or next smaller, 1 = exact or next larger, 2 = wildcard match using * and ?. |
| `search_mode` | No | Specifies the search direction. 1 = first to last (default), -1 = last to first, 2 = binary search ascending (data must be sorted A-Z), -2 = binary search descending (data must be sorted Z-A). |

### Match Mode Values (Detailed)

| Value | Name | Behavior | Use Case |
|-------|------|----------|----------|
| `0` | Exact Match | Returns only if lookup_value exactly matches a value in lookup_array. Default behavior. | Most common lookups: finding specific IDs, names, codes. |
| `-1` | Exact or Next Smaller | Returns exact match if found; otherwise returns the largest value less than lookup_value. Data need not be sorted. | Tax brackets, commission tiers, finding applicable rate for a value. |
| `1` | Exact or Next Larger | Returns exact match if found; otherwise returns the smallest value greater than lookup_value. Data need not be sorted. | Finding next available slot, minimum qualifying tier. |
| `2` | Wildcard Match | Enables * (any characters) and ? (single character) wildcards in lookup_value. | Partial text matching, pattern searches like "John*" or "???-1234". |

### Search Mode Values (Detailed)

| Value | Name | Behavior | Use Case |
|-------|------|----------|----------|
| `1` | First to Last | Searches from first item to last. Default behavior. Returns first match found. | Standard lookups where you want the first occurrence. |
| `-1` | Last to First | Searches from last item to first. Returns last match when duplicates exist. | Finding most recent entry in chronological data. |
| `2` | Binary Search (Ascending) | Uses binary search algorithm. Data MUST be sorted A-Z or smallest to largest. Much faster for large datasets. | Large sorted datasets where performance matters. |
| `-2` | Binary Search (Descending) | Uses binary search algorithm. Data MUST be sorted Z-A or largest to smallest. Much faster for large datasets. | Large reverse-sorted datasets. |

### Return Value

Returns the value(s) from the return_array corresponding to the position of the match in lookup_array. If return_array contains multiple columns or rows, returns an array that spills automatically (Excel 365) or fills array formula range (older versions). Returns #N/A if no match found and if_not_found is not specified.

## Examples

> [!f(x)] XLOOKUP Examples

### Example 1: Basic Exact Match Lookup
```
=XLOOKUP("Apple", A2:A100, B2:B100)
```
**Result:** Returns the value from column B where column A equals "Apple"

**Explanation:** The simplest XLOOKUP form. Searches A2:A100 for "Apple" and returns the corresponding value from B2:B100. Equivalent to VLOOKUP but searches the exact column specified, not by column index.

---

### Example 2: Lookup with Custom Error Message
```
=XLOOKUP(D1, A:A, B:B, "Product not found")
```
**Result:** Returns matching value from B, or "Product not found" if D1 doesn't exist in column A

**Explanation:** The fourth parameter replaces #N/A errors with a user-friendly message. Eliminates the need for IFERROR(VLOOKUP(...), "message") patterns.

---

### Example 3: Return Multiple Columns
```
=XLOOKUP(E1, A:A, B:D)
```
**Result:** Returns a 1-row by 3-column array containing values from columns B, C, and D for the matching row

**Explanation:** Unlike VLOOKUP which returns only one column, XLOOKUP can return entire sections of a row. The result spills across three cells in Excel 365.

---

### Example 4: Left-to-Right Lookup (Reverse Lookup)
```
=XLOOKUP("Sales Manager", C:C, A:A)
```
**Result:** Returns the value from column A where column C equals "Sales Manager"

**Explanation:** XLOOKUP has no column index—it directly references the return column. This enables "reverse" lookups where the return column is to the LEFT of the search column, impossible with VLOOKUP.

---

### Example 5: Exact or Next Smaller (Tax Bracket Example)
```
=XLOOKUP(75000, TaxBrackets, TaxRates, , -1)
```
**Result:** If brackets are {0, 10000, 40000, 85000}, returns the rate for 40000 (the largest bracket <= 75000)

**Explanation:** Match mode -1 finds exact match or the next smaller value. Perfect for tiered pricing, tax brackets, or commission structures where you need the applicable tier for a given amount.

---

### Example 6: Exact or Next Larger (Finding Next Available)
```
=XLOOKUP(TODAY(), AvailableDates, Slots, "None available", 1)
```
**Result:** Returns the slot for today's date if available, otherwise the next future available date

**Explanation:** Match mode 1 finds exact match or the next larger value. Ideal for appointment systems, finding the next available inventory, or scheduling.

---

### Example 7: Wildcard Matching
```
=XLOOKUP("John*", A:A, B:B, "Not found", 2)
```
**Result:** Returns value from B for first name starting with "John" (matches John, Johnson, Johnny, etc.)

**Explanation:** Match mode 2 enables wildcards. The asterisk (*) matches any sequence of characters. Use question mark (?) to match exactly one character.

---

### Example 8: Last Match with Duplicates
```
=XLOOKUP("Completed", StatusColumn, DateColumn, , 0, -1)
```
**Result:** Returns the date from the LAST row where status equals "Completed"

**Explanation:** Search mode -1 searches from bottom to top, returning the last occurrence. Critical for finding the most recent entry in chronological data with duplicate keys.

---

### Example 9: Binary Search for Performance
```
=XLOOKUP(SearchID, SortedIDs, Values, "Not found", 0, 2)
```
**Result:** Returns value using high-speed binary search (data must be sorted ascending)

**Explanation:** Search mode 2 uses binary search algorithm—dramatically faster for large sorted datasets. For 1 million rows, binary search checks ~20 items vs. potentially 1 million with linear search.

---

### Example 10: Horizontal Lookup
```
=XLOOKUP("Q3", A1:D1, A2:D2)
```
**Result:** Returns the value from row 2 under the "Q3" column header

**Explanation:** XLOOKUP handles horizontal data naturally—no need for a separate HLOOKUP function. The lookup_array and return_array are simply horizontal instead of vertical.

---

### Example 11: Two-Way Lookup (Row and Column)
```
=XLOOKUP(RowHeader, A:A, XLOOKUP(ColHeader, 1:1, 2:100))
```
**Result:** Returns the intersection of a specific row and column

**Explanation:** Nested XLOOKUP replaces INDEX/MATCH/MATCH. The inner XLOOKUP finds the correct column based on ColHeader, and the outer XLOOKUP finds the row.

---

### Example 12: Return Entire Row
```
=XLOOKUP(ID, A:A, A:Z)
```
**Result:** Returns all values from columns A through Z for the matching row

**Explanation:** Setting return_array to a wide range returns the entire row. Useful for retrieving complete records from a database table.

---

### Example 13: Case-Sensitive Lookup (Using EXACT)
```
=XLOOKUP(TRUE, EXACT(A:A, SearchValue), B:B)
```
**Result:** Returns value from B where A exactly matches SearchValue including case

**Explanation:** XLOOKUP is case-insensitive by default. Wrapping the comparison in EXACT creates an array of TRUE/FALSE values, then XLOOKUP finds TRUE.

---

### Example 14: Multiple Criteria Lookup
```
=XLOOKUP(1, (A:A=Criteria1)*(B:B=Criteria2), C:C)
```
**Result:** Returns value from C where both A matches Criteria1 AND B matches Criteria2

**Explanation:** Multiplying conditions creates an array of 1s (both TRUE) and 0s (any FALSE). XLOOKUP finds the 1, effectively creating an AND condition across multiple columns.

---

### Example 15: Lookup with Calculated Return
```
=XLOOKUP(Product, A:A, C:C*D:D)
```
**Result:** Returns Price * Quantity for the matching product row

**Explanation:** The return_array can be a calculation. This returns the product of columns C and D for the matching row, eliminating the need for a helper column.

---

### Example 16: Approximate Match on Unsorted Data
```
=XLOOKUP(100, Scores, Grades, , -1, 1)
```
**Result:** Finds the largest score <= 100 even in unsorted data, returns corresponding grade

**Explanation:** Unlike VLOOKUP's approximate match which requires sorted data, XLOOKUP's match_mode -1 or 1 works on unsorted data when search_mode is 1 (default) or -1.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Lookup value not found and no if_not_found specified | Add the if_not_found parameter: `=XLOOKUP(val, search, return, "Not found")` |
| `#VALUE!` | Lookup_array and return_array have different numbers of rows (or columns for horizontal) | Ensure both arrays have matching dimensions—same row count for vertical lookups. |
| `#REF!` | Return_array reference is invalid or deleted | Verify the return_array range exists and is correctly referenced. |
| `#NAME?` | XLOOKUP not available in your Excel version | XLOOKUP requires Excel 365 or Excel 2021. Use INDEX/MATCH for older versions. |
| `#SPILL!` | Multi-column return blocked by existing data | Clear cells where the result needs to spill, or use single-column return. |
| `Wrong result with binary search` | Data not properly sorted for search_mode 2 or -2 | Binary search REQUIRES sorted data. Sort ascending for mode 2, descending for -2. |
| `Returns first instead of last match` | Using default search_mode with duplicates | Use search_mode -1 to find last occurrence: `=XLOOKUP(..., , , 0, -1)` |
| `Wildcards not working` | Using wildcards without match_mode 2 | Explicitly set match_mode to 2: `=XLOOKUP("*text*", ..., , , 2)` |

## Use Cases

### [[Customer Data Retrieval System]]

**Scenario:** Build a comprehensive customer lookup system that retrieves multiple fields of customer data from a master database based on Customer ID, with graceful handling of unknown customers.

**Implementation:**
```
Customer name and email:
=XLOOKUP(CustomerID, Database[ID], Database[Name]&" - "&Database[Email], "Customer not in system")

Full customer record (multiple columns):
=XLOOKUP(CustomerID, $A:$A, $B:$F, {"","","","",""})

Most recent order for customer (last match):
=XLOOKUP(CustomerID, OrderLog[CustomerID], OrderLog[OrderDate], "No orders", 0, -1)
```

**Business Application:** Customer service representatives enter a customer ID and instantly retrieve contact information, account status, and order history. The if_not_found parameter shows clear messages when IDs are mistyped. Multi-column returns eliminate the need for multiple lookup formulas.

**Technical Details:** Using structured table references (Table[Column]) makes formulas more readable and automatically adjusts when rows are added. The last-match search mode (-1) is essential for finding the most recent transaction among potentially thousands of orders for the same customer.

---

### [[Dynamic Pricing and Commission Tiers]]

**Scenario:** Implement tiered pricing structures, commission rates, or tax brackets where the applicable rate depends on which bracket a value falls into.

**Implementation:**
```
Commission rate based on sales amount:
=XLOOKUP(SalesAmount, TierThresholds, CommissionRates, 0%, -1)

Tax calculation with brackets:
=XLOOKUP(TaxableIncome, TaxBrackets[MinIncome], TaxBrackets[Rate], 0, -1)

Volume discount tier:
=XLOOKUP(OrderQuantity, DiscountBreaks, DiscountPercent, 0%, -1) * UnitPrice * OrderQuantity
```

**Business Application:** Sales compensation automatically calculates correct commission rates as reps hit different tiers. Finance applies correct tax rates without manual lookup. Pricing systems automatically apply volume discounts based on order size.

**Technical Details:** Match mode -1 (exact or next smaller) is the key—it finds the highest threshold that doesn't exceed the lookup value. Unlike VLOOKUP's approximate match, this works on unsorted data (though sorted is cleaner). The if_not_found value of 0% handles cases below the minimum threshold.

---

### [[Bidirectional Data Integration]]

**Scenario:** Merge data from multiple sources where the key columns aren't in the leftmost position, requiring lookups in multiple directions and across different table structures.

**Implementation:**
```
Lookup where key is in column C, return from column A:
=XLOOKUP(SearchValue, Table1[MiddleColumn], Table1[FirstColumn])

Cross-reference between systems with different ID formats:
=XLOOKUP("*"&PartialID&"*", LegacySystem[FullID], LegacySystem[Description], "Not found", 2)

Two-way lookup (intersection of row and column):
=XLOOKUP(RowProduct, Products, XLOOKUP(ColMonth, Months, SalesData))
```

**Business Application:** Data integration between legacy systems with different structures. Reporting that pulls data from CRM, ERP, and financial systems regardless of how each organizes its columns. Matrix-style reports where values are looked up by both row and column headers.

**Technical Details:** XLOOKUP's bidirectional capability eliminates the INDEX/MATCH workaround previously required for leftward lookups. Wildcard matching (mode 2) enables fuzzy matching between systems with slightly different ID formats. Nested XLOOKUP provides true two-dimensional lookups previously requiring INDEX with two MATCH functions.

---

### [[Time-Series and Version Tracking]]

**Scenario:** Work with historical data where you need to find the most recent entry, the next available date, or values as of a specific point in time.

**Implementation:**
```
Most recent status update:
=XLOOKUP(ItemID, Log[Item], Log[Status], "No history", 0, -1)

Next available appointment after requested date:
=XLOOKUP(RequestedDate, AvailableSlots[Date], AvailableSlots[Time], "None available", 1)

Value as of specific date (last entry on or before):
=XLOOKUP(AsOfDate, PriceHistory[EffectiveDate], PriceHistory[Price], , -1)
```

**Business Application:** Audit trails showing current status from chronological logs. Appointment scheduling finding next available slots. Financial reporting pulling prices or rates effective as of specific dates. Version control showing which configuration was active at any point in time.

**Technical Details:** Search mode -1 (last to first) combined with chronological data finds the most recent entry. Match mode 1 (next larger) finds the next available date. Match mode -1 (next smaller) on dates finds the most recent entry on or before a target date—essential for "as of" reporting.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365 (subscription) and Excel 2021 (perpetual license) only
- **Not available in:** Excel 2019, Excel 2016, Excel 2013, or earlier
- **Dynamic arrays:** Full support—multi-column returns spill automatically
- **Performance:** Optimized for large datasets, especially with binary search modes
- **VBA compatibility:** Can be called from VBA using Application.WorksheetFunction.XLookup

### Google Sheets

- **Availability:** Added to Google Sheets in 2022
- **Syntax:** Identical to Excel
- **Array behavior:** Native array support—spills automatically
- **Performance:** Comparable to Excel for most datasets
- **Collaboration:** Real-time updates when source data changes

### Key Differences

| Feature | Excel 365 | Excel 2021 | Google Sheets | Excel 2019 and earlier |
|---------|-----------|------------|---------------|------------------------|
| XLOOKUP available | Yes | Yes | Yes | No |
| Dynamic array spilling | Yes | Yes | Yes | N/A |
| Binary search modes | Yes | Yes | Yes | N/A |
| Alternative function | N/A | N/A | N/A | INDEX/MATCH |
| Maximum search_mode values | -2 to 2 | -2 to 2 | -2 to 2 | N/A |

### Version Compatibility Workaround

For files that may be opened in older Excel versions, consider INDEX/MATCH alternatives:
```
XLOOKUP equivalent:
=XLOOKUP(A1, B:B, C:C)

INDEX/MATCH equivalent:
=INDEX(C:C, MATCH(A1, B:B, 0))

XLOOKUP with if_not_found:
=XLOOKUP(A1, B:B, C:C, "Not found")

INDEX/MATCH with IFERROR:
=IFERROR(INDEX(C:C, MATCH(A1, B:B, 0)), "Not found")
```

## Tips and Best Practices

1. **Use if_not_found to replace IFERROR wrappers:** Instead of `=IFERROR(XLOOKUP(...), "Error")`, use `=XLOOKUP(..., "Error")` as the fourth parameter. Cleaner and more efficient.

2. **Leverage multi-column returns:** XLOOKUP can return entire row sections. `=XLOOKUP(ID, A:A, B:F)` returns 5 columns at once, eliminating the need for 5 separate lookups.

3. **Remember the match_mode defaults differ from VLOOKUP:** XLOOKUP defaults to exact match (0). VLOOKUP defaulted to approximate match (TRUE). This prevents accidental wrong matches.

4. **Use binary search for large sorted datasets:** For datasets over 10,000 rows that are already sorted, search_mode 2 or -2 dramatically improves performance. But data MUST be sorted correctly.

5. **Search_mode -1 finds the last match:** When you have duplicate keys and need the most recent or last entry, use search_mode -1. This is invaluable for chronological data.

6. **Combine match modes thoughtfully:** Match_mode -1 (next smaller) + search_mode 1 (first to last) on unsorted data finds the correct tier bracket. Match_mode 0 + search_mode -1 finds the last exact match.

7. **Use wildcards intentionally:** Wildcard matching (match_mode 2) only activates when explicitly specified. This prevents accidental pattern matching when your data contains * or ? characters.

8. **Nest XLOOKUP for two-dimensional lookups:** `=XLOOKUP(RowValue, RowHeaders, XLOOKUP(ColValue, ColHeaders, DataRange))` replaces complex INDEX/MATCH/MATCH formulas.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VLOOKUP]] | Vertical lookup by column index | Legacy compatibility or when XLOOKUP unavailable |
| [[HLOOKUP]] | Horizontal lookup by row index | Legacy compatibility for horizontal data |
| [[INDEX]] + [[MATCH]] | Classic combination for flexible lookups | When XLOOKUP unavailable or for specific array manipulations |
| [[LOOKUP]] | Simple vector/array lookup | Rarely—XLOOKUP is superior in almost all cases |
| [[XMATCH]] | Returns position instead of value | When you need the row/column number, not the value |

### Commonly Used Together

**[[XMATCH]]** - Return position of a match

*Combined with XLOOKUP for complex position-based operations:*
```
=XLOOKUP(XMATCH(Criteria1, Range1) & XMATCH(Criteria2, Range2), KeyColumn, ValueColumn)
```
Uses XMATCH positions as composite keys for multi-criteria lookups.

---

**[[FILTER]]** - Filter arrays by criteria

*Combined with XLOOKUP for filtered results lookup:*
```
=XLOOKUP(SearchVal, FILTER(A:A, B:B>100), FILTER(C:C, B:B>100))
```
Searches only within filtered subsets of data.

---

**[[SORT]]** - Sort arrays

*Combined with XLOOKUP for sorted result lookups:*
```
=XLOOKUP(SearchVal, SORT(SourceRange), SORT(ReturnRange, , , BY(SourceRange)))
```
Enables binary search by sorting data dynamically.

---

**[[UNIQUE]]** - Extract unique values

*Combined with XLOOKUP for unique value lookups:*
```
=XLOOKUP(SearchVal, UNIQUE(Categories), UNIQUE(CategoryValues))
```
Looks up in deduplicated lists.

---

**[[INDIRECT]]** - Create dynamic references

*Combined with XLOOKUP for dynamic table references:*
```
=XLOOKUP(SearchVal, INDIRECT(TableName&"[ID]"), INDIRECT(TableName&"[Value]"))
```
Enables lookups across dynamically selected tables.

---

**[[IFERROR]]** - Legacy error handling

*Alternative to XLOOKUP's built-in if_not_found:*
```
=IFERROR(XLOOKUP(Val, Search, Return), "Custom error")
```
Rarely needed since XLOOKUP has if_not_found parameter, but useful for catching other error types.

## Official Documentation

- **Microsoft Excel:** [XLOOKUP function](https://support.microsoft.com/en-us/office/xlookup-function-b7fd680e-6d10-43e6-84f9-88eae8bf5929)
- **Google Sheets:** [XLOOKUP function](https://support.google.com/docs/answer/12405947)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2019 (rolling release) | First availability as part of dynamic arrays initiative |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added for Excel compatibility |
| Excel Web | 2019 | Full feature parity with desktop |
| Excel Mobile | 2020 | iOS and Android support |

---

*Last updated: 2026-01-10*
