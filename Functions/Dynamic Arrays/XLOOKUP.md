---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - dynamic-arrays
  - lookup-reference
subTopics:
  - data-retrieval
  - table-lookup
  - vertical-lookup
  - horizontal-lookup
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - extended-lookup
  - modern-lookup
  - flexible-lookup
  - vlookup-replacement
tags:
  - dynamic-arrays
  - lookup
  - reference
  - data-retrieval
  - excel
  - google-sheets
---

# XLOOKUP

## Description

XLOOKUP is the modern, versatile lookup function that Microsoft introduced to replace and extend the capabilities of VLOOKUP, HLOOKUP, and even many INDEX/MATCH combinations. The function searches for a value in a lookup range and returns the corresponding value from a return range, with the key advantage that these ranges can be positioned anywhere relative to each other, the lookup can proceed in any direction, and a custom "not found" value can be specified directly within the function. XLOOKUP works equally well for vertical lookups (searching down a column) and horizontal lookups (searching across a row), eliminating the need for separate functions. It returns arrays when the return range spans multiple columns or rows, making it far more powerful than its predecessors.

XLOOKUP is the recommended choice for virtually all lookup scenarios in modern Excel. Use it when you need to find a value based on an identifier, retrieve data from tables, connect related datasets, or perform any "find and fetch" operation. Common applications include looking up product prices by product code, finding employee details by ID, retrieving customer information by account number, matching invoice data to orders, and populating form fields based on selections. XLOOKUP handles one-to-one lookups, can return entire rows or multiple columns at once, supports approximate matching for range-based lookups (like tax brackets), and even enables binary search for large sorted datasets.

There are several important gotchas to understand with XLOOKUP. First, while it can look left (unlike VLOOKUP), the lookup and return ranges must have the same dimensions, both being either single columns or single rows. Second, XLOOKUP performs exact matching by default, unlike VLOOKUP which defaults to approximate matching, a change that prevents one of the most common lookup errors. Third, when multiple matches exist, XLOOKUP returns only the first match (or last, if configured), not all matches. Fourth, the function is case-insensitive by default. Fifth, wildcard matching requires explicitly setting match_mode to 2, unlike VLOOKUP where wildcards work with exact match mode. Sixth, for approximate matches, XLOOKUP has different modes for "next larger," "next smaller," and "exact or next," which can be confusing.

Regarding platform availability, XLOOKUP is available in Microsoft Excel 365, Excel 2021, and Excel for web. It was introduced in late 2019 and has become the standard lookup recommendation. **XLOOKUP is NOT available in Google Sheets**, which is a significant limitation for cross-platform work. Google Sheets users must continue using VLOOKUP, HLOOKUP, INDEX/MATCH, or the QUERY function for lookup operations. When building workbooks that will be used across both platforms, consider whether XLOOKUP formulas will need alternative implementations for Sheets users. In Excel, XLOOKUP integrates fully with dynamic arrays and can return spilling results.

## Syntax

> [!f(x)] XLOOKUP Syntax
>
> ```excel
> =XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_mode], [search_mode])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `lookup_value` | The value to search for in the lookup_array. Can be text, number, date, cell reference, or formula result. | Yes |
> | `lookup_array` | The range or array to search. Must be a single row or single column. | Yes |
> | `return_array` | The range or array from which to return values. Must have the same number of rows (if lookup_array is a column) or columns (if lookup_array is a row) as lookup_array. Can span multiple columns/rows for returning multiple values. | Yes |
> | `if_not_found` | The value to return if no match is found. If omitted, returns #N/A. | No |
> | `match_mode` | How to match: 0 = exact match (default), -1 = exact or next smaller, 1 = exact or next larger, 2 = wildcard match. | No |
> | `search_mode` | Search direction: 1 = first to last (default), -1 = last to first, 2 = binary search ascending, -2 = binary search descending. | No |

### Match Mode Values

| Value | Description |
|-------|-------------|
| 0 | Exact match (default). Returns #N/A or if_not_found if no exact match exists. |
| -1 | Exact match or next smaller item. Useful for tax brackets, price tiers. |
| 1 | Exact match or next larger item. Useful for finding available inventory levels. |
| 2 | Wildcard match. Supports *, ?, and ~ wildcards in lookup_value. |

### Search Mode Values

| Value | Description |
|-------|-------------|
| 1 | Search first to last (default). Finds first matching value. |
| -1 | Search last to first. Finds last matching value (useful for duplicates). |
| 2 | Binary search (ascending). Data must be sorted ascending. Much faster for large datasets. |
| -2 | Binary search (descending). Data must be sorted descending. Much faster for large datasets. |

## Examples

### Example 1: Basic Exact Match Lookup
```excel
=XLOOKUP("P001", A2:A100, C2:C100)
```
**Result:** Returns the value from column C where "P001" is found in column A

**Explanation:** The simplest XLOOKUP finds "P001" in the A column and returns the corresponding value from column C. Unlike VLOOKUP, there's no column number to count, the return range can be anywhere.

---

### Example 2: Lookup with "Not Found" Message
```excel
=XLOOKUP(E2, A2:A100, B2:B100, "Customer not found")
```
**Result:** Returns customer name from B, or "Customer not found" if E2 value isn't in A

**Explanation:** The fourth parameter provides a custom not-found value instead of #N/A. This is cleaner than wrapping VLOOKUP in IFERROR and shows a meaningful message to users.

---

### Example 3: Looking Up to the Left
```excel
=XLOOKUP("Smith", C2:C100, A2:A100)
```
**Result:** Returns the ID from column A where "Smith" is found in column C

**Explanation:** Unlike VLOOKUP which only looks right, XLOOKUP can return values from columns to the left of the lookup column. Here we find a name in C and return the ID from A.

---

### Example 4: Returning Multiple Columns
```excel
=XLOOKUP(A2, Products!A:A, Products!B:D)
```
**Result:** Returns three columns (B, C, D) for the matched row, spilling across three cells

**Explanation:** When the return_array spans multiple columns, XLOOKUP returns all of them as a spill array. This eliminates the need for multiple VLOOKUP formulas to get different columns.

---

### Example 5: Horizontal Lookup
```excel
=XLOOKUP("Q3", A1:D1, A2:D2)
```
**Result:** Returns the value from row 2 in the column labeled "Q3"

**Explanation:** XLOOKUP works horizontally too, replacing HLOOKUP. Search across row 1 for "Q3" and return the corresponding value from row 2.

---

### Example 6: Approximate Match - Next Smaller (Tax Brackets)
```excel
=XLOOKUP(B2, TaxBrackets!A:A, TaxBrackets!B:B, , -1)
```
Where TaxBrackets has income thresholds in A and rates in B

**Result:** Returns tax rate for income in B2, matching the bracket threshold at or below that income

**Explanation:** Match mode -1 finds the largest value that is less than or equal to the lookup value. Perfect for tax brackets, price tiers, or grade scales where you want "which bracket am I in?"

---

### Example 7: Approximate Match - Next Larger
```excel
=XLOOKUP(OrderQty, InventoryLevels, WarehouseIDs, "Insufficient stock", 1)
```
**Result:** Finds the first warehouse with inventory >= OrderQty

**Explanation:** Match mode 1 finds the smallest value that is greater than or equal to the lookup value. Useful when you need "at least this much" matching.

---

### Example 8: Wildcard Lookup
```excel
=XLOOKUP("*Smith*", A2:A100, B2:B100, "No match", 2)
```
**Result:** Returns value from B where A contains "Smith" anywhere

**Explanation:** Match mode 2 enables wildcard matching. Use * for multiple characters, ? for single character. Here we find any cell containing "Smith".

---

### Example 9: Last Match (Reverse Search)
```excel
=XLOOKUP("Order", A2:A1000, B2:B1000, , 0, -1)
```
**Result:** Returns the last occurrence of "Order" in column A, rather than the first

**Explanation:** Search mode -1 searches from last to first, returning the last match when duplicates exist. Useful for finding the most recent entry when data is chronologically ordered.

---

### Example 10: Binary Search for Large Datasets
```excel
=XLOOKUP(G2, A2:A100000, C2:C100000, "Not found", 0, 2)
```
**Result:** Same as exact match, but much faster for 100,000 rows

**Explanation:** Search mode 2 uses binary search, which is dramatically faster for large sorted datasets. The lookup column MUST be sorted ascending. Use -2 for descending sorted data.

---

### Example 11: Two-Way Lookup (Row and Column)
```excel
=XLOOKUP(A2, B1:G1, XLOOKUP(E2, A3:A20, B3:G20))
```
**Result:** Returns value at intersection of row matching E2 and column matching A2

**Explanation:** Nested XLOOKUP creates two-dimensional lookup. The inner XLOOKUP finds the row and returns the entire row, then the outer XLOOKUP finds the column within that row.

---

### Example 12: Returning Entire Row
```excel
=XLOOKUP(B2, IDs, DataTable)
```
Where DataTable is the entire data range including all columns

**Result:** Returns all columns for the matching row, spilling horizontally

**Explanation:** When return_array is a multi-column range, you get all columns back. Perfect for populating forms or displaying complete records.

---

### Example 13: Case-Insensitive by Default
```excel
=XLOOKUP("SMITH", A2:A100, B2:B100)
```
**Result:** Matches "smith", "Smith", "SMITH", or any case variation

**Explanation:** XLOOKUP is case-insensitive. "SMITH" matches "Smith" or "smith". For case-sensitive matching, you would need an array formula with EXACT.

---

### Example 14: Lookup with Date Values
```excel
=XLOOKUP(DATE(2024,6,15), A2:A100, B2:B100, "Date not found", -1)
```
**Result:** Finds the exact date or nearest earlier date

**Explanation:** XLOOKUP works with dates as numeric values. Match mode -1 is useful for "as of" date lookups where you want the most recent record on or before the specified date.

---

### Example 15: Nested in Other Functions
```excel
=SUM(XLOOKUP(A2, Products!A:A, Products!D:F))
```
**Result:** Sums the three values returned from columns D, E, and F

**Explanation:** Since XLOOKUP returns arrays, you can wrap it in aggregation functions. This sums three columns of data for the matched row without separate lookups.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | No exact match found and no if_not_found specified | Provide if_not_found parameter or verify the lookup value exists |
| `#VALUE!` | Lookup_array and return_array have different dimensions | Ensure both have same row count (for column lookup) or column count (for row lookup) |
| `#NAME?` | XLOOKUP not available in this Excel version or in Google Sheets | Use Excel 365/2021; for Sheets use VLOOKUP/INDEX-MATCH |
| `#REF!` | Return array references deleted cells | Update return_array to valid range |
| `#SPILL!` | Cells where multi-column result should spill are not empty | Clear cells to the right of the formula |
| `Wrong result with approximate match` | Data not sorted when using approximate match | Sort data appropriately, or use exact match mode 0 |
| `Wrong result` | Unexpected duplicates in lookup_array | Use search_mode -1 to get last match, or clean up duplicates |
| `Wildcards not working` | Match mode not set to 2 | Explicitly set match_mode to 2 for wildcard searches |

## Use Cases

### [[Employee Information System]]
- **Implementation**: Use XLOOKUP to retrieve employee details from an HR database based on employee ID, badge number, or email address. Return multiple columns for complete employee profiles, or specific fields for targeted information needs.
- **Business Application**: HR portals displaying employee information, payroll systems looking up salary data, access control systems validating employee credentials, organizational charts populating from master employee list, performance review systems pulling historical data.
- **Technical Details**: Store employee master data in a structured table. Use XLOOKUP with employee ID as primary lookup. Consider multiple XLOOKUP formulas to allow searching by different identifiers (ID, email, name). Use if_not_found for "Employee not found" messages. For large organizations (10,000+ employees), consider binary search mode.

### [[Dynamic Pricing and Tier Lookup]]
- **Implementation**: Use XLOOKUP with approximate matching (match_mode -1) to determine pricing tiers, discount levels, or tax brackets based on quantity, purchase amount, or income levels.
- **Business Application**: E-commerce systems calculating volume discounts, tax calculators determining applicable rates, insurance premium calculations based on age/risk brackets, loyalty program tier assignment, shipping cost determination by weight/zone.
- **Technical Details**: Structure tier tables with threshold values in ascending order in the lookup column and corresponding rates/values in the return column. Use match_mode -1 to find "the largest value less than or equal to" the lookup value. Ensure tier boundaries are clearly defined (e.g., is 100 in the 100+ tier or 99-and-under tier?).

### [[Inventory and Order Management]]
- **Implementation**: Look up product information (price, description, stock levels) by SKU or product code. Validate order items against product catalog, pull current pricing into orders, check availability.
- **Business Application**: Order entry systems populating product details, inventory checks before order confirmation, invoice generation with current pricing, product catalog displays, stock replenishment triggers.
- **Technical Details**: Maintain a master product table with unique SKUs. XLOOKUP by SKU to retrieve price, description, category, and other attributes. Return multiple columns to populate entire order line details. Use if_not_found to flag discontinued or invalid products. Consider table references for auto-expanding ranges as products are added.

### [[Cross-Reference and Data Matching]]
- **Implementation**: Match records between different data sources, validate entries against master lists, or enrich datasets by pulling in related information from lookup tables.
- **Business Application**: Reconciling bank transactions with internal records, matching customer records across systems, validating imported data against known values, merging data from acquisitions, creating unified customer views from multiple sources.
- **Technical Details**: Use exact match mode for precise reconciliation. Consider wildcard matching for partial matches. Chain multiple XLOOKUPs for complex matching scenarios. Use if_not_found to identify unmatched records requiring manual review. When matching text, consider TRIM and consistent casing.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | Excel 365, Excel 2021, Excel for Web | NOT AVAILABLE |
| **Alternative in Sheets** | N/A | VLOOKUP, INDEX/MATCH, QUERY |
| **Look Left Capability** | Yes | Use INDEX/MATCH in Sheets |
| **Custom Not Found Value** | Yes, built-in parameter | Use IFERROR(VLOOKUP(...), "message") |
| **Return Multiple Columns** | Yes, with spill | Use multiple formulas or QUERY |
| **Approximate Matching** | Match modes -1, 1 | VLOOKUP TRUE or MATCH |
| **Wildcard Matching** | Match mode 2 | VLOOKUP FALSE with wildcards |
| **Binary Search** | Search modes 2, -2 | Not available, VLOOKUP sorted TRUE |
| **Reverse Search** | Search mode -1 | Not directly available |
| **Dynamic Arrays** | Full support | ARRAYFORMULA patterns |

**Google Sheets Alternatives:**

Since XLOOKUP is not available in Google Sheets, use these approaches:

1. **Basic lookup (VLOOKUP equivalent):**
```
=VLOOKUP(A2, DataRange, 3, FALSE)
```

2. **Look left (INDEX/MATCH):**
```
=INDEX(B:B, MATCH(A2, C:C, 0))
```

3. **With error handling:**
```
=IFERROR(VLOOKUP(A2, Data, 3, FALSE), "Not found")
```

4. **Multiple columns with QUERY:**
```
=QUERY(DataRange, "SELECT B, C, D WHERE A = '"&A2&"'")
```

**Key Recommendations by Platform:**

For Microsoft Excel:
- Use XLOOKUP as the default for all new lookup formulas
- Take advantage of if_not_found for cleaner error handling
- Use multi-column returns to reduce formula count
- Apply binary search for large datasets

For Google Sheets:
- Use INDEX/MATCH for flexibility matching XLOOKUP's capabilities
- Use VLOOKUP for simple right-facing lookups
- Use QUERY for complex data retrieval
- Always wrap in IFERROR for error handling

## Tips and Best Practices

1. **Always Use the if_not_found Parameter**: Instead of wrapping in IFERROR, specify the fourth parameter. `=XLOOKUP(A2, IDs, Names, "Not found")` is cleaner and more intentional than IFERROR wrappers.

2. **Default to Exact Match**: Unlike VLOOKUP, XLOOKUP defaults to exact match (mode 0), which is correct 95% of the time. Only change match_mode when you specifically need approximate or wildcard matching.

3. **Return Multiple Columns at Once**: When you need several values from the same row, make return_array span those columns. One XLOOKUP returning three columns is better than three separate XLOOKUP formulas.

4. **Use Binary Search for Large Data**: For datasets over 10,000 rows, if the lookup column is sorted, use search_mode 2 (or -2 for descending). This transforms O(n) to O(log n) performance.

5. **Remember the Dimension Match Rule**: Lookup_array and return_array must have equal dimensions (same row count for column searches, same column count for row searches). Mismatched dimensions cause #VALUE! errors.

6. **Use Last-to-First for Recent Records**: When data has duplicates and you want the most recent (last) entry, use search_mode -1 instead of writing complex array formulas to find the last occurrence.

7. **Nest for Two-Dimensional Lookups**: For matrix-style lookups (row AND column), nest one XLOOKUP inside another. Inner finds the row, outer finds the column within that row.

8. **Validate Lookup Data Quality**: XLOOKUP is case-insensitive and matches on cell values. Extra spaces, leading zeros stored as text, or inconsistent formatting will cause match failures. Use TRIM and consistent data types.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from XLOOKUP |
|----------|-------------|----------------------------|
| [[VLOOKUP]] | Vertical lookup (legacy) | Only searches left column, returns columns to right; no built-in error handling |
| [[HLOOKUP]] | Horizontal lookup (legacy) | Only searches top row, returns rows below |
| [[INDEX]]+[[MATCH]] | Flexible lookup combination | More versatile in some edge cases; still valid approach |
| [[LOOKUP]] | Simplified lookup (legacy) | Limited functionality; primarily for backward compatibility |
| [[XMATCH]] | Modern MATCH replacement | Returns position only, not value; complements XLOOKUP |

### Commonly Used Together

**[[XMATCH]]** - Find position in range

*Use XMATCH when you need position rather than value:*
```excel
=XMATCH("Product A", A2:A100)
```
Returns the row position of "Product A". XMATCH is to XLOOKUP what MATCH is to INDEX.

**[[LET]]** - Define named variables

*Use LET with XLOOKUP for complex formulas:*
```excel
=LET(found, XLOOKUP(A2, IDs, Data), IF(ISNA(found), "Not found", found))
```
Name the lookup result for reuse, improving readability for complex conditional logic.

**[[FILTER]]** - Dynamic array filtering

*Use FILTER when XLOOKUP's single-match isn't enough:*
```excel
=FILTER(A2:D100, B2:B100=E2)
```
When you need ALL matching rows (not just first), FILTER is the right choice.

**[[SUM/AVERAGE]]** - Aggregate returned values

*Combine with aggregation for multi-column returns:*
```excel
=SUM(XLOOKUP(A2, Products!A:A, Products!C:E))
```
Sum the three columns returned by XLOOKUP for the matched row.

**[[IFERROR]]** - Error handling (legacy approach)

*When if_not_found isn't sufficient:*
```excel
=IFERROR(XLOOKUP(A2, Data, Returns), "Error occurred")
```
Still useful if you need to catch all error types, not just "not found".

**[[INDIRECT]]** - Dynamic range references

*Build dynamic table references:*
```excel
=XLOOKUP(A2, INDIRECT(Sheet&"!A:A"), INDIRECT(Sheet&"!C:C"))
```
Look up in different sheets based on a cell value.

## Official Documentation

- **Microsoft Excel**: [XLOOKUP function](https://support.microsoft.com/en-us/office/xlookup-function-b7fd680e-6d10-43e6-84f9-88eae8bf5929)
- **Google Sheets**: Not available - Use [VLOOKUP](https://support.google.com/docs/answer/3093318) or [INDEX/MATCH](https://support.google.com/docs/answer/3098242) instead

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 365 | August 2019 | Initial preview release to Insiders |
| Excel 365 | January 2020 | General availability for Microsoft 365 subscribers |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for Web | 2020 | Full support added to browser version |
| Excel 365 | 2022 | Performance optimizations for large datasets |
| Google Sheets | - | Not available; no announced plans for implementation |

---

*Last updated: 2026-01-10*
