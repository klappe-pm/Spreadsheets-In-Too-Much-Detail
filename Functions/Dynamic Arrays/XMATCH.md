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
  - position-finding
  - index-lookup
  - array-search
  - match-modes
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - extended-match
  - modern-match
  - position-finder
  - match-replacement
tags:
  - dynamic-arrays
  - lookup
  - match
  - position
  - index
  - excel
  - google-sheets
---

# XMATCH

## Description

XMATCH is the modern replacement for the MATCH function, designed to find the relative position of a value within a range or array. Unlike MATCH, which uses confusing match_type values of 1, 0, and -1, XMATCH uses intuitive parameters for match mode and search direction. The function searches a specified range for a lookup value and returns its position as a number, which can then be used with INDEX or other functions to retrieve values. XMATCH supports exact matching, approximate matching (next larger or next smaller), and wildcard matching, along with the ability to search from first-to-last or last-to-first, or use efficient binary search for large sorted datasets.

XMATCH is essential whenever you need to know WHERE a value is located rather than retrieving the value itself. Common uses include finding the position of an item in a list for use with INDEX, determining which row or column contains a specific header, calculating rankings or ordinal positions, locating the position of a maximum or minimum value, and supporting complex lookups that require knowing the position before fetching data. XMATCH is often paired with INDEX to create flexible lookup solutions that can look in any direction, handle multiple return values, and work with irregular data structures where XLOOKUP might not be suitable.

There are several important gotchas when using XMATCH. First, XMATCH returns a 1-based position, meaning the first item in the search range is position 1, not 0. Second, when multiple matches exist, XMATCH returns the position of the first match by default (or last if using search_mode -1). Third, the function is case-insensitive for text matching. Fourth, approximate matching (match_mode -1 or 1) does NOT require sorted data when using default search mode, but DOES require sorted data when using binary search modes. Fifth, wildcard mode (match_mode 2) uses * for any characters, ? for single characters, and ~ to escape wildcards. Sixth, XMATCH returns #N/A when no match is found, which must be handled for robust formulas.

Regarding platform availability, XMATCH is available in Microsoft Excel 365 and Excel 2021, but is NOT available in Google Sheets. Google Sheets users must continue using the traditional MATCH function, which has a confusing syntax where 0 means exact match, 1 means approximate ascending, and -1 means approximate descending. The lack of XMATCH in Google Sheets is notable because it pairs naturally with XLOOKUP, which is also absent from Sheets. For cross-platform compatibility, consider using the traditional MATCH function, which is available in both platforms with identical core functionality despite the syntax differences.

## Syntax

> [!f(x)] XMATCH Syntax
>
> ```excel
> =XMATCH(lookup_value, lookup_array, [match_mode], [search_mode])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `lookup_value` | The value to search for. Can be text, number, date, cell reference, or formula result. | Yes |
> | `lookup_array` | The range or array to search within. Must be a single row or single column. | Yes |
> | `match_mode` | How to match: 0 = exact match (default), -1 = exact or next smaller, 1 = exact or next larger, 2 = wildcard match using *, ?, ~. | No |
> | `search_mode` | Search direction: 1 = first to last (default), -1 = last to first, 2 = binary search ascending, -2 = binary search descending. | No |

### Match Mode Values Explained

| Value | Name | Description | Use Case |
|-------|------|-------------|----------|
| 0 | Exact Match | Returns position only if value is found exactly. Returns #N/A otherwise. | Most lookups: IDs, codes, exact text |
| -1 | Exact or Next Smaller | If exact match not found, returns position of next smaller value. | Tax brackets, pricing tiers, grade scales |
| 1 | Exact or Next Larger | If exact match not found, returns position of next larger value. | Finding minimum threshold, inventory levels |
| 2 | Wildcard Match | Allows *, ?, and ~ wildcards in lookup_value. | Partial text matching, pattern finding |

### Search Mode Values Explained

| Value | Name | Description | Requirement |
|-------|------|-------------|-------------|
| 1 | First to Last | Searches from beginning to end, returns first match. | Default; works with any data order |
| -1 | Last to First | Searches from end to beginning, returns last match. | Use for most recent when duplicates exist |
| 2 | Binary Search Ascending | Fast search assuming data sorted ascending. | Data MUST be sorted ascending |
| -2 | Binary Search Descending | Fast search assuming data sorted descending. | Data MUST be sorted descending |

## Examples

### Example 1: Basic Exact Match Position
```excel
=XMATCH("Apple", A2:A100)
```
**Result:** Returns the row position of "Apple" within A2:A100, e.g., 5 means A6 (A2 is position 1)

**Explanation:** The simplest XMATCH finds the position of a value. If "Apple" is in A6 (the 5th cell of the range A2:A100), XMATCH returns 5. This position is 1-based relative to the search range.

---

### Example 2: Combining with INDEX for Lookup
```excel
=INDEX(B2:B100, XMATCH(E2, A2:A100))
```
**Result:** Returns the value from column B corresponding to where E2's value is found in column A

**Explanation:** XMATCH finds the position, then INDEX retrieves the value at that position from a different column. This is the INDEX/XMATCH pattern, similar to INDEX/MATCH but with cleaner syntax.

---

### Example 3: Finding Header Column Position
```excel
=XMATCH("Revenue", A1:G1)
```
**Result:** Returns the column number where "Revenue" appears in the header row

**Explanation:** Use XMATCH to dynamically find column positions by header name. The result can be used with INDEX, OFFSET, or for column references in other formulas.

---

### Example 4: Next Smaller (Tax Bracket Lookup)
```excel
=XMATCH(75000, TaxBrackets, -1)
```
Where TaxBrackets contains: 0, 10000, 40000, 85000, 165000

**Result:** 3 (the position of 40000, which is the largest value <= 75000)

**Explanation:** Match mode -1 finds the exact value or the next smaller one. Income of 75000 falls into the bracket that starts at 40000 (position 3), not 85000. Perfect for tiered lookups.

---

### Example 5: Next Larger (Minimum Threshold)
```excel
=XMATCH(50, InventoryLevels, 1)
```
Where InventoryLevels contains: 10, 25, 75, 100, 200

**Result:** 3 (the position of 75, the first value >= 50)

**Explanation:** Match mode 1 finds the exact value or the next larger one. When you need "at least 50," this returns the position of 75, the first inventory level that meets the requirement.

---

### Example 6: Wildcard Matching
```excel
=XMATCH("*Corp", A2:A100, 2)
```
**Result:** Returns position of first cell ending with "Corp" (e.g., "Acme Corp", "BigBiz Corp")

**Explanation:** Match mode 2 enables wildcards. The asterisk (*) matches any characters, so "*Corp" finds any company name ending with "Corp". Use ? for single character wildcard.

---

### Example 7: Finding Last Match (Reverse Search)
```excel
=XMATCH("Pending", Status, 0, -1)
```
**Result:** Returns position of the LAST occurrence of "Pending" in the Status range

**Explanation:** Search mode -1 searches backward. When duplicates exist and you want the most recent (last) entry, this finds the position of the last "Pending" status.

---

### Example 8: Binary Search for Performance
```excel
=XMATCH(E2, SortedIDs, 0, 2)
```
**Result:** Same as exact match, but uses binary search for speed

**Explanation:** Search mode 2 uses binary search algorithm, dramatically faster for large datasets. The SortedIDs range MUST be sorted in ascending order. For 100,000 rows, this is roughly 17 comparisons instead of 50,000 on average.

---

### Example 9: Finding Position of MAX Value
```excel
=XMATCH(MAX(A2:A100), A2:A100)
```
**Result:** Returns the position of the maximum value within the range

**Explanation:** First calculate MAX to find the highest value, then XMATCH finds where that value is located. Useful for highlighting winners or locating peak values.

---

### Example 10: Position for Dynamic Column Reference
```excel
=INDEX(A2:G2, XMATCH(H1, A1:G1))
```
**Result:** Returns value from row 2 at the column specified by H1

**Explanation:** XMATCH finds which column header matches H1, then INDEX retrieves the value from that column in row 2. This creates a dynamic column selector.

---

### Example 11: Position-Based Conditional Logic
```excel
=IF(XMATCH(A2, ApprovedList, 0) > 0, "Approved", "Not in list")
```
**Result:** "Approved" if A2 is in the ApprovedList, otherwise "Not in list"

**Explanation:** Since XMATCH returns #N/A when not found, wrapping in ISNUMBER() or using IFERROR creates a membership test. Here, any positive result means the item is in the list.

---

### Example 12: Finding Position in Dynamic Array
```excel
=XMATCH("Target", UNIQUE(A2:A100))
```
**Result:** Position of "Target" within the unique values list

**Explanation:** XMATCH works with dynamic array results, not just static ranges. Here it searches within the UNIQUE values generated from column A.

---

### Example 13: Multiple Criteria with Concatenation
```excel
=XMATCH(A2&B2, ID_Region, 0)
```
Where ID_Region is a helper column with concatenated lookup keys

**Result:** Position where both ID and Region match

**Explanation:** For multi-criteria matching, concatenate the criteria and search in a concatenated lookup range. This is a common pattern for compound key lookups.

---

### Example 14: Position of Closest Date
```excel
=XMATCH(TODAY(), DateColumn, -1)
```
**Result:** Position of today's date, or the most recent past date if today isn't in the list

**Explanation:** Match mode -1 with dates finds the exact date or the closest earlier date. Perfect for "as of" date lookups in financial or historical data.

---

### Example 15: Handling Not Found with IFERROR
```excel
=IFERROR(XMATCH(A2, Products, 0), 0)
```
**Result:** Position if found, or 0 if not found

**Explanation:** XMATCH returns #N/A when no match exists. Wrap in IFERROR to provide a default value. Returning 0 is common for further calculations, or use "Not found" for display.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Value not found in exact match mode | Verify value exists, check for data type mismatch (text vs number), use IFERROR for graceful handling |
| `#N/A` | Wildcard match found no results | Check wildcard syntax, ensure * and ? are positioned correctly |
| `#VALUE!` | Lookup_array is not a single row or column | Provide a 1-dimensional range or array |
| `#NAME?` | XMATCH not available (older Excel or Google Sheets) | Use Excel 365/2021; for Sheets use MATCH |
| `Wrong position` | Binary search used on unsorted data | Sort data appropriately, or use default search_mode 1 |
| `Wrong position` | Data type mismatch ("123" text vs 123 number) | Ensure consistent data types in lookup_value and lookup_array |
| `Position off by 1` | Confusing 1-based counting | Remember position 1 = first cell in range, not row 1 of spreadsheet |

## Use Cases

### [[Dynamic Column Selection]]
- **Implementation**: Use XMATCH to find the position of a column header, then use that position with INDEX to retrieve values from that column. Create dropdown menus that let users select which column to display.
- **Business Application**: Interactive dashboards where users select metrics to view, report templates that work across different column layouts, data validation systems that adapt to varying import formats, flexible chart data sources.
- **Technical Details**: Search the header row with XMATCH(selection, A1:Z1), use result with INDEX(A2:Z100, , position) to get entire column. Combine with INDIRECT for fully dynamic range building. Cache position in a cell if used multiple times.

### [[Ranking and Position Calculations]]
- **Implementation**: Find the position of a value within a sorted list to determine rank. Combine XMATCH with SORT to create custom ranking systems that handle ties and missing values appropriately.
- **Business Application**: Sales leaderboards, performance rankings, competitive positioning analysis, grade class rankings, tournament seedings, market share position tracking.
- **Technical Details**: For simple ranking: XMATCH(value, SORT(range, , -1)) gives descending rank. Handle ties by considering whether RANK, RANK.EQ, or RANK.AVG behavior is desired. For percentile ranking, divide position by count.

### [[Multi-Dimensional Lookups with INDEX]]
- **Implementation**: Use XMATCH twice (once for row position, once for column position) with INDEX to create flexible two-way lookups. This is more explicit than nested XLOOKUP and works well for matrix-style data.
- **Business Application**: Price lookup matrices (size x color), schedule lookups (day x time), rate tables (origin x destination), inventory grid lookups, skill assessment matrices.
- **Technical Details**: =INDEX(DataMatrix, XMATCH(RowCriteria, RowHeaders), XMATCH(ColCriteria, ColHeaders)). Ensure headers are properly aligned with data matrix dimensions. Handle not-found cases for both dimensions.

### [[Data Validation and Existence Checking]]
- **Implementation**: Use XMATCH in conditional logic to test whether values exist in reference lists. Create validation rules that check incoming data against approved lists.
- **Business Application**: Order entry validation (valid product codes), user access control (approved user list), data import validation, inventory item verification, customer account validation.
- **Technical Details**: Use ISNUMBER(XMATCH(...)) for TRUE/FALSE existence check. Combine with conditional formatting to highlight invalid entries. For faster validation of large datasets, consider binary search mode if the reference list is sorted.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | Excel 365, Excel 2021 | NOT AVAILABLE |
| **Alternative in Sheets** | N/A | MATCH function |
| **Match Mode Syntax** | 0, -1, 1, 2 (intuitive) | 0, 1, -1 (confusing) |
| **Search Mode Options** | First-to-last, last-to-first, binary search | First-to-last only (MATCH) |
| **Wildcard Support** | Match mode 2 | In MATCH with type 0 |
| **Binary Search** | Search modes 2, -2 | MATCH type 1 or -1 requires sorted |
| **Case Sensitivity** | Case-insensitive | Case-insensitive |

**Google Sheets MATCH Syntax Comparison:**

| XMATCH Syntax | Equivalent MATCH Syntax | Description |
|---------------|------------------------|-------------|
| XMATCH(val, rng, 0) | MATCH(val, rng, 0) | Exact match |
| XMATCH(val, rng, -1) | MATCH(val, rng, 1)* | Next smaller (requires sorted asc) |
| XMATCH(val, rng, 1) | MATCH(val, rng, -1)* | Next larger (requires sorted desc) |
| XMATCH(val, rng, 2) | MATCH with wildcards | Wildcard with type 0 |

*Note: MATCH's approximate modes require sorted data; XMATCH's default search does not.

**Key Recommendations by Platform:**

For Microsoft Excel:
- Use XMATCH as the modern standard for all position-finding
- Pair with INDEX for powerful, flexible lookups
- Take advantage of search_mode -1 for last-match scenarios
- Use binary search for large sorted datasets

For Google Sheets:
- Use MATCH with type 0 for exact matching
- Remember MATCH's confusing type values (1 and -1 are approximate)
- Combine with INDEX just as you would XMATCH + INDEX
- Wrap in IFERROR for error handling

## Tips and Best Practices

1. **Pair with INDEX, Not Just XLOOKUP**: While XLOOKUP is great, INDEX + XMATCH remains valuable for complex scenarios like returning entire rows, working with irregular ranges, or when you need the position for other calculations.

2. **Remember 1-Based Positioning**: XMATCH returns 1 for the first item, not 0. When using with INDEX, this works perfectly since INDEX is also 1-based.

3. **Use Match Mode 0 as Default**: Exact matching (mode 0) is correct for most scenarios. Only use approximate modes (-1, 1) when you specifically need bracket/tier behavior.

4. **Binary Search for Performance**: For sorted data with more than 10,000 rows, binary search (mode 2 or -2) dramatically improves performance. But ONLY if the data is actually sorted.

5. **Search Mode -1 for Last Occurrence**: When duplicates exist and you want the most recent (typically last) entry, use search_mode -1 instead of complex array formulas.

6. **Handle #N/A with IFERROR**: Always plan for not-found cases. `=IFERROR(XMATCH(...), "Not found")` or using the result conditionally prevents errors from propagating.

7. **Verify Data Types Match**: "123" (text) and 123 (number) are different. XMATCH will return #N/A if types don't match. Use VALUE() or TEXT() to ensure consistency.

8. **Use for Column Position in Dynamic Reports**: XMATCH(header_name, header_row) is perfect for making formulas independent of specific column letters, improving template robustness.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from XMATCH |
|----------|-------------|---------------------------|
| [[MATCH]] | Traditional position-finding function | Uses confusing match_type values; no search direction option |
| [[XLOOKUP]] | Modern value lookup | Returns values, not positions; XMATCH + INDEX = XLOOKUP alternative |
| [[SEARCH]] | Finds text position within string | Operates on text strings, not arrays/ranges |
| [[FIND]] | Finds text position (case-sensitive) | Text within string, not value in range |

### Commonly Used Together

**[[INDEX]]** - Return value at position

*The classic INDEX + XMATCH pattern:*
```excel
=INDEX(B2:B100, XMATCH(E2, A2:A100))
```
XMATCH finds position, INDEX retrieves value. Together they create flexible lookups.

**[[XLOOKUP]]** - Modern lookup alternative

*When you don't need the position separately:*
```excel
=XLOOKUP(E2, A2:A100, B2:B100)
```
XLOOKUP combines the find and fetch in one function. Use XMATCH when you need the position for other purposes.

**[[SORT]]** - Sort arrays

*Find position in sorted results:*
```excel
=XMATCH(value, SORT(data, , -1))
```
Find where a value ranks in descending sorted order.

**[[UNIQUE]]** - Get unique values

*Find position in unique list:*
```excel
=XMATCH(target, UNIQUE(A:A))
```
Find position among deduplicated values.

**[[MAX]]/[[MIN]]** - Find extreme values

*Locate maximum or minimum:*
```excel
=XMATCH(MAX(Sales), Sales)
```
Find which position contains the highest value.

**[[FILTER]]** - Filter arrays

*Find position in filtered results:*
```excel
=XMATCH(target, FILTER(A:A, B:B>100))
```
Search within a dynamically filtered subset.

## Official Documentation

- **Microsoft Excel**: [XMATCH function](https://support.microsoft.com/en-us/office/xmatch-function-d966da31-7a6b-4a13-a1c6-5a33ed6a0312)
- **Google Sheets**: Not available - Use [MATCH function](https://support.google.com/docs/answer/3093378) instead

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 365 | August 2019 | Initial preview release to Insiders |
| Excel 365 | January 2020 | General availability for Microsoft 365 subscribers |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for Web | 2020 | Full support added to browser version |
| Google Sheets | - | Not available; use MATCH as alternative |

---

*Last updated: 2026-01-10*
