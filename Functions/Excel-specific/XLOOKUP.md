---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
- dynamic-arrays
subTopics:
- vertical-lookup
- horizontal-lookup
- bidirectional-search
- array-formulas
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- X Lookup
- Extended Lookup
- Modern VLOOKUP
- Bidirectional Lookup
tags:
- lookup
- reference
- dynamic-arrays
- data-retrieval
- excel-365
- modern-excel
---

# XLOOKUP

## Description

**XLOOKUP** is the modern, more powerful replacement for VLOOKUP, HLOOKUP, and even INDEX/MATCH combinations. Introduced in Excel 365 and Excel 2021, it searches a range or array for a match and returns the corresponding item from a second range or array. Unlike its predecessors, XLOOKUP can search both vertically and horizontally, look left or right, return multiple columns, and handle errors gracefully with built-in default values. It represents Microsoft's answer to decades of VLOOKUP limitations that frustrated Excel users worldwide.

The function eliminates the most common VLOOKUP pain points. You no longer need to count column numbers that break when columns are inserted. You no longer need separate functions for vertical (VLOOKUP) versus horizontal (HLOOKUP) lookups. The default behavior is exact match rather than approximate, preventing the silent errors that plagued countless spreadsheets. And the ability to search from last to first enables powerful last-occurrence matching that previously required complex workarounds.

**XLOOKUP in Google Sheets:** Google Sheets added XLOOKUP support in August 2022, making this function available across both major platforms. The implementations are nearly identical, with Google Sheets supporting all core functionality including match modes, search modes, and array returns. This cross-platform availability means you can now use XLOOKUP confidently in collaborative environments without worrying about compatibility. Some minor differences exist in error handling edge cases, but for practical purposes, formulas are interchangeable.

**When to use XLOOKUP over alternatives:** Use XLOOKUP for virtually all new lookup needs. It replaces VLOOKUP for looking up values in tables, replaces HLOOKUP for horizontal data, and often replaces INDEX/MATCH for flexibility. The only cases where INDEX/MATCH might still be preferred are extremely complex multi-dimensional lookups or when you need functions like SMALL/LARGE in the matching logic. For anything else, XLOOKUP's cleaner syntax and built-in error handling make it the superior choice.

## Syntax

> [!f(x)] XLOOKUP Syntax
>
> ```
> =XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_mode], [search_mode])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `lookup_value` | Yes | The value to search for. Can be a value, cell reference, or expression. When searching for text, wildcards (*,?) are supported when match_mode is set to 2 or -2. |
| `lookup_array` | Yes | The array or range to search. Must be a single row or column (one-dimensional for the search). |
| `return_array` | Yes | The array or range from which to return values. Can be multiple columns/rows to return multiple values. Must have the same number of rows (for column lookup) or columns (for row lookup) as lookup_array. |
| `if_not_found` | No | Value to return if no match is found. If omitted and no match exists, returns #N/A error. This parameter eliminates the need for IFERROR wrappers. |
| `match_mode` | No | Match type: 0 = exact match (default), -1 = exact or next smaller, 1 = exact or next larger, 2 = wildcard match with *, ?, ~ as special characters. |
| `search_mode` | No | Search direction: 1 = first to last (default), -1 = last to first, 2 = binary search ascending (data must be sorted), -2 = binary search descending (data must be sorted). |

### Return Value

Returns the value(s) from return_array at the position where the match was found in lookup_array. If return_array contains multiple columns/rows, returns all corresponding values as a spill array. Returns if_not_found value or #N/A if no match exists.

## Examples

> [!f(x)] XLOOKUP Examples

### Example 1: Basic Exact Match Lookup
```
=XLOOKUP(F2, A2:A100, C2:C100)
```
**Result:** Returns the value from column C where column A matches F2

**Explanation:** The simplest XLOOKUP pattern. Search for F2 in the range A2:A100, and return the corresponding value from C2:C100. Unlike VLOOKUP, no column number is needed, and the return column can be anywhere relative to the lookup column.

---

### Example 2: Lookup with Custom Error Message
```
=XLOOKUP(G2, A2:A100, B2:B100, "Employee not found")
```
**Result:** Returns name from column B or "Employee not found" if no match

**Explanation:** The fourth parameter specifies what to return when the lookup value doesn't exist. This eliminates the need for IFERROR or IFNA wrappers, producing cleaner formulas.

---

### Example 3: Returning Multiple Columns
```
=XLOOKUP(A10, A2:A8, B2:D8)
```
**Result:** Returns three values (from columns B, C, D) as a spill array

**Explanation:** When return_array spans multiple columns, XLOOKUP returns all of them. This single formula replaces what would require three separate VLOOKUPs or a complex INDEX/MATCH with multiple column references.

---

### Example 4: Horizontal Lookup (Replacing HLOOKUP)
```
=XLOOKUP("Q3", B1:E1, B5:E5)
```
**Result:** Returns value from row 5 where row 1 contains "Q3"

**Explanation:** XLOOKUP handles horizontal data naturally. No separate function needed. Search across row 1 for "Q3" and return the corresponding value from row 5.

---

### Example 5: Looking Left (Impossible with VLOOKUP)
```
=XLOOKUP(F2, C2:C100, A2:A100)
```
**Result:** Returns value from column A based on matching column C

**Explanation:** VLOOKUP can only return values to the right of the lookup column. XLOOKUP has no such limitation. Here we search in column C and return from column A, which is to its left.

---

### Example 6: Last Match (Search from End)
```
=XLOOKUP(G2, A2:A100, B2:B100, , 0, -1)
```
**Result:** Returns the LAST occurrence's value instead of the first

**Explanation:** Search_mode of -1 searches from last to first. When multiple rows match, this returns the most recent entry. Perfect for getting the latest transaction, most recent status, or last occurrence.

---

### Example 7: Approximate Match - Next Smaller
```
=XLOOKUP(C10, A2:A8, B2:B8, , -1)
```
**Result:** If exact match not found, returns value for next smaller lookup item

**Explanation:** Match_mode of -1 finds exact match or, if none exists, the next smaller value. Useful for tax brackets, commission tiers, or any graduated scale where you need to find which bracket applies.

---

### Example 8: Approximate Match - Next Larger
```
=XLOOKUP(150, A2:A8, B2:B8, , 1)
```
**Result:** If exact 150 not found, returns value for next larger item

**Explanation:** Match_mode of 1 finds the next larger value when no exact match exists. Useful for shipping weight tiers, pricing thresholds, or rounding up to standard sizes.

---

### Example 9: Wildcard Search
```
=XLOOKUP("*Smith*", A2:A100, B2:B100, "Not found", 2)
```
**Result:** Returns first match containing "Smith" anywhere in the name

**Explanation:** Match_mode of 2 enables wildcard matching. Asterisk (*) matches any characters, question mark (?) matches single character. The tilde (~) escapes wildcards if you need to search for literal * or ?.

---

### Example 10: Binary Search for Large Datasets
```
=XLOOKUP(F2, A2:A100000, B2:B100000, , 0, 2)
```
**Result:** Same as exact match but much faster on sorted data

**Explanation:** Search_mode of 2 uses binary search algorithm, dramatically faster on large sorted datasets. Data MUST be sorted ascending. Use -2 for descending sorted data. Only use when performance matters and data is guaranteed sorted.

---

### Example 11: Nested XLOOKUP for Two-Dimensional Lookup
```
=XLOOKUP(B10, B1:E1, XLOOKUP(A10, A2:A8, B2:E8))
```
**Result:** Looks up both row and column to return intersection value

**Explanation:** First XLOOKUP finds the row based on A10, returning the entire row B2:E8. Second XLOOKUP searches that row using B10 to find the column. This replaces INDEX/MATCH/MATCH combinations.

---

### Example 12: Using with FILTER for Conditional Lookup
```
=XLOOKUP(MAX(FILTER(B2:B100, C2:C100="Active")), B2:B100, A2:A100)
```
**Result:** Returns name of active employee with highest value in column B

**Explanation:** FILTER extracts only active employees' B values, MAX finds the highest, then XLOOKUP finds who it belongs to. Shows XLOOKUP's power combined with other dynamic array functions.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Lookup value not found and no if_not_found specified | Add fourth parameter with custom "not found" message or default value |
| `#VALUE!` | Lookup_array and return_array have different sizes | Ensure both arrays have the same number of rows (vertical) or columns (horizontal) |
| `#REF!` | Return array doesn't exist or reference is invalid | Verify return_array references valid cells and hasn't been deleted |
| `#SPILL!` | Multiple values returned but adjacent cells aren't empty | Clear the cells where the array needs to spill, or move the formula |
| `Wrong result with binary search` | Data not properly sorted for search_mode 2 or -2 | Only use binary search on pre-sorted data; use default search_mode for unsorted data |
| `Wildcard not working` | Match_mode not set to 2 or -2 | Wildcards only work when match_mode explicitly set to 2 (or -2 for wildcard + binary) |

## Use Cases

### [[Employee Information Retrieval]]

**Scenario:** HR needs a dashboard that retrieves complete employee records (name, department, hire date, salary) from a database table using employee ID.

**Implementation:**
```
=XLOOKUP(G2, A:A, B:E, "ID Not Found")
```
Where G2 contains the employee ID to search, column A has all IDs, and columns B:E contain employee details.

**Business Application:** Single lookup returns four columns of data simultaneously. No need for four separate VLOOKUP formulas. When columns are added to the database, the formula doesn't break. The custom error message prevents confusing #N/A errors for non-technical users.

**Technical Details:** Returning multiple columns creates a spill array. Ensure enough empty columns exist to the right for the results. The array auto-updates if source data changes.

---

### [[Dynamic Price Tier Lookup]]

**Scenario:** E-commerce site needs to apply discount tiers based on order quantity: 1-9 units at full price, 10-24 at 10% off, 25-99 at 20% off, 100+ at 30% off.

**Implementation:**
```
=XLOOKUP(OrderQty, {1;10;25;100}, {0;0.1;0.2;0.3}, 0, -1)
```
Returns the discount rate for the order quantity's tier.

**Business Application:** Pricing rules are embedded in the formula without maintaining a separate lookup table. Match_mode -1 (next smaller) finds which tier the quantity falls into. A quantity of 50 matches the 25 tier (next smaller than 50).

**Technical Details:** Curly braces create inline arrays. This pattern works for tax brackets, shipping zones, commission rates, or any tiered structure. Consider using named ranges for the arrays for maintainability.

---

### [[Latest Transaction Lookup]]

**Scenario:** Financial system tracks all transactions by customer. When looking up a customer, need to find their most recent transaction, not the first one.

**Implementation:**
```
=XLOOKUP(CustomerID, A:A, C:C, "No transactions", 0, -1)
```
Where transactions are listed chronologically with newest at the bottom.

**Business Application:** Customer service can instantly see the latest interaction. Accounting can verify the most recent payment. The reverse search eliminates complex array formulas previously needed to find last occurrences.

**Technical Details:** Search_mode -1 searches from last to first, returning the last match. Data should be in chronological order. For reverse chronological (newest first), use search_mode 1.

---

### [[Cross-Reference Two Data Sources]]

**Scenario:** Merge data from two systems where the key field exists in both but column positions differ. Old system has ID in column D, new system expects it to return column A values.

**Implementation:**
```
=XLOOKUP(NewSystem!A2, OldSystem!D:D, OldSystem!A:A, "Not migrated")
```

**Business Application:** Data migration and system integration projects frequently need to match records between sources with different structures. XLOOKUP's ability to look anywhere removes restructuring requirements.

**Technical Details:** Cross-sheet references work identically. For cross-workbook references, include the workbook name. Consider using structured table references for self-documenting formulas.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Array behavior:** Returns spill arrays automatically
- **Structured references:** Full support for Table[Column] syntax
- **Performance:** Binary search modes available for sorted data
- **Not available:** Excel 2019 and earlier versions

### Google Sheets
- **Availability:** Added August 2022, available to all users
- **Array behavior:** Identical spill behavior to Excel
- **Structured references:** Works with named ranges but no table column syntax
- **Performance:** Binary search supported
- **Compatibility:** Core functionality matches Excel

### Key Differences
Both platforms now fully support XLOOKUP with equivalent functionality. The main historical difference was availability, which is no longer an issue. Some edge cases in error handling may differ slightly between platforms, but for all practical purposes, XLOOKUP formulas are fully portable between Excel 365/2021 and Google Sheets.

### Migration Note
When migrating from older Excel versions that don't support XLOOKUP, you'll need to convert to INDEX/MATCH combinations or upgrade to Excel 365/2021.

## Tips and Best Practices

1. **Use the if_not_found parameter:** Always specify the fourth parameter to avoid #N/A errors. Return empty string "", zero, or a descriptive message. This eliminates IFERROR wrappers and makes formulas cleaner.

2. **Let return_array span multiple columns:** Instead of multiple XLOOKUP formulas retrieving different columns, make return_array span all needed columns. One formula, multiple results.

3. **Default to exact match:** Unlike VLOOKUP, XLOOKUP defaults to exact match (match_mode 0). Don't add unnecessary parameters if exact match is what you need.

4. **Use binary search for large datasets:** When searching sorted data with thousands of rows, search_mode 2 (ascending) or -2 (descending) dramatically improves performance.

5. **Remember last-match capability:** Search_mode -1 finds the last occurrence. This is invaluable for time-series data where you want the most recent entry.

6. **Combine with other dynamic array functions:** XLOOKUP works beautifully with FILTER, SORT, UNIQUE, and SEQUENCE. Use XLOOKUP inside FILTER criteria or on FILTER results.

7. **Use structured references in Excel tables:** `=XLOOKUP(A1, Table1[ID], Table1[Name])` is self-documenting and automatically adjusts as tables grow.

8. **Consider nested XLOOKUP for 2D lookups:** Instead of INDEX with two MATCH functions, nest XLOOKUP inside XLOOKUP for matrix-style lookups. Often cleaner and more readable.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VLOOKUP]] | Vertical lookup (legacy) | When compatibility with older Excel versions is required |
| [[HLOOKUP]] | Horizontal lookup (legacy) | When compatibility with older Excel versions is required |
| [[INDEX]] | Returns value at specific position | For complex multi-dimensional lookups with dynamic row/column selection |
| [[MATCH]] | Returns position of value | When you need position, not value; often paired with INDEX |
| [[LOOKUP]] | Simplified approximate lookup | Rarely; XLOOKUP supersedes this |

### Commonly Used Together

**[[FILTER]]** - Returns filtered array based on criteria

*Combined with XLOOKUP for conditional lookups:*
```
=XLOOKUP(MAX(FILTER(Sales, Region="West")), Sales, ProductName)
```
FILTER extracts West region sales, MAX finds highest, XLOOKUP returns the product name.

---

**[[SORT]]** - Sorts array by specified columns

*Sorted data feeding XLOOKUP:*
```
=XLOOKUP(A1, SORT(Data,2,-1), SORT(Names,2,-1))
```
When you need to search dynamically sorted data. Both arrays must be sorted identically.

---

**[[UNIQUE]]** - Returns unique values from range

*XLOOKUP on unique list:*
```
=XLOOKUP(G1, UNIQUE(Categories), CategoryTotals)
```
Lookup against deduplicated list ensures first/only match.

---

**[[IFERROR]]** - Catches formula errors (legacy approach)

*For backwards compatibility:*
```
=IFERROR(XLOOKUP(A1,B:B,C:C), "Not found")
```
Not needed with XLOOKUP's built-in if_not_found, but useful when nesting XLOOKUP in other functions that might error.

---

**[[XMATCH]]** - Returns position of value in array

*XLOOKUP and XMATCH share syntax concepts:*
```
=XMATCH(A1, B:B, 0, -1)
```
When you need just the position, not the returned value. XMATCH is to XLOOKUP what MATCH is to INDEX.

## Official Documentation

- **Microsoft Excel:** [XLOOKUP function](https://support.microsoft.com/en-us/office/xlookup-function-b7fd680e-6d10-43e6-84f9-88eae8bf5929)
- **Google Sheets:** [XLOOKUP function](https://support.google.com/docs/answer/12405947)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2019 (Insider), 2020 (General) | First introduction as part of dynamic arrays |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for web | 2020 | Full feature parity with desktop |
| Google Sheets | August 2022 | Added to match Excel functionality |
| Excel 2019 | Not available | Must use VLOOKUP/INDEX/MATCH instead |
| Excel 2016 | Not available | Must use VLOOKUP/INDEX/MATCH instead |

---

*Last updated: 2026-01-10*
