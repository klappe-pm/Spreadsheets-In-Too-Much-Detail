---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- position-matching
- dynamic-arrays
- modern-functions
- search-modes
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Modern Match
- Advanced Match
- Position Finder
- XMatch
tags:
- lookup
- reference
- xmatch
- match
- position
- dynamic-arrays
- excel-365
---

# XMATCH

## Description

**XMATCH** returns the relative position of a value within an array, serving as the modern replacement for the classic MATCH function. `=XMATCH("Apple", A1:A100)` searches for "Apple" in the range and returns its position (row number within that range). If "Apple" is in A5, and A1:A100 is the lookup range, XMATCH returns 5—the fifth position in the array. This position value is essential for INDEX functions, array manipulations, and any scenario requiring you to know WHERE something is located rather than retrieving its associated value.

The function offers the same powerful matching options as XLOOKUP: exact match (0), exact or next smaller (-1), exact or next larger (1), and wildcard matching (2). Combined with four search modes—first-to-last (1), last-to-first (-1), binary search ascending (2), and binary search descending (-2)—XMATCH handles position-finding scenarios that were difficult or impossible with the original MATCH function. Unlike MATCH, XMATCH defaults to exact matching, eliminating a common source of errors.

**XMATCH can search from the end of a range, finding the LAST occurrence of a value.** Classic MATCH always returns the first match. With `=XMATCH("Complete", StatusColumn, 0, -1)`, you find the position of the most recent "Complete" entry in chronological data. This capability is invaluable for logs, transaction histories, and any dataset where duplicates exist and you need the latest entry.

**XMATCH pairs perfectly with INDEX for dynamic data retrieval.** The combination `=INDEX(DataRange, XMATCH(LookupValue, LookupRange))` provides the same flexibility as XLOOKUP but with explicit control over the position. This is particularly useful when you need the position for multiple purposes: retrieving data, calculating offsets, or determining relative locations within complex data structures.

## Syntax

> [!f(x)] XMATCH Syntax
>
> ```
> =XMATCH(lookup_value, lookup_array, [match_mode], [search_mode])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `lookup_value` | Yes | The value to search for. Can be a cell reference, text, number, date, or formula result. Supports wildcards (* and ?) when match_mode is 2. |
| `lookup_array` | Yes | The range or array to search. Must be a single row or column—cannot be a multi-column, multi-row range. This is where XMATCH looks for the lookup_value. |
| `match_mode` | No | Specifies the match type. 0 = exact match (default), -1 = exact or next smaller, 1 = exact or next larger, 2 = wildcard match using * and ?. |
| `search_mode` | No | Specifies the search direction. 1 = first to last (default), -1 = last to first, 2 = binary search ascending (data must be sorted A-Z), -2 = binary search descending (data must be sorted Z-A). |

### Match Mode Values (Detailed)

| Value | Name | Behavior | Use Case |
|-------|------|----------|----------|
| `0` | Exact Match | Returns position only if lookup_value exactly matches a value. Default behavior. | Finding specific items: IDs, product codes, exact names. |
| `-1` | Exact or Next Smaller | Returns position of exact match or the largest value less than lookup_value. | Finding applicable tier position in tax brackets, commission tables. |
| `1` | Exact or Next Larger | Returns position of exact match or the smallest value greater than lookup_value. | Finding next available slot position, minimum qualifying tier. |
| `2` | Wildcard Match | Enables * (any characters) and ? (single character) wildcards in lookup_value. | Finding partial matches: "Jo*" matches John, Joseph, Joanne. |

### Search Mode Values (Detailed)

| Value | Name | Behavior | Use Case |
|-------|------|----------|----------|
| `1` | First to Last | Searches from first item to last. Returns position of first match. Default. | Standard searches where first occurrence matters. |
| `-1` | Last to First | Searches from last item to first. Returns position of last match. | Finding most recent entry in chronological data. |
| `2` | Binary Search (Ascending) | Uses binary search. Data MUST be sorted A-Z / smallest to largest. | Large sorted datasets—dramatically faster. |
| `-2` | Binary Search (Descending) | Uses binary search. Data MUST be sorted Z-A / largest to smallest. | Large reverse-sorted datasets. |

### Return Value

Returns an integer representing the relative position of the found value within the lookup_array. Position 1 is the first item, position 2 is the second, and so on. Returns #N/A if no match is found. Unlike MATCH, XMATCH uses 1-based indexing consistently—the first item is always position 1.

## Examples

> [!f(x)] XMATCH Examples

### Example 1: Basic Position Lookup
```
=XMATCH("Apple", A1:A100)
```
**Result:** Returns the row position within A1:A100 where "Apple" is found (e.g., 5 if "Apple" is in A5)

**Explanation:** The simplest XMATCH form. Searches A1:A100 for "Apple" and returns its position. If "Apple" is the fifth item in the range, returns 5.

---

### Example 2: Case-Insensitive Matching (Default)
```
=XMATCH("apple", A1:A100)
```
**Result:** Matches "Apple", "APPLE", "apple", or any case variation

**Explanation:** XMATCH is case-insensitive by default. "apple" matches "Apple" in the array. This matches standard spreadsheet lookup behavior.

---

### Example 3: Find Position of a Number
```
=XMATCH(500, B1:B50)
```
**Result:** Returns position of 500 in the range (e.g., 12 if 500 is in B12)

**Explanation:** XMATCH works with numbers, text, dates, and logical values. For numeric lookups, exact match finds the precise value.

---

### Example 4: Exact or Next Smaller (Tier Position)
```
=XMATCH(75000, TierBreaks, -1)
```
**Result:** If TierBreaks = {0, 25000, 50000, 100000}, returns 3 (position of 50000, the largest value <= 75000)

**Explanation:** Match mode -1 finds the position of the exact value if it exists, otherwise the position of the largest value less than the lookup value. Essential for tier-based calculations.

---

### Example 5: Exact or Next Larger (Next Slot)
```
=XMATCH(TODAY(), AvailableDates, 1)
```
**Result:** Returns position of today's date if available, or the next future date's position

**Explanation:** Match mode 1 finds the position of the smallest value greater than or equal to the lookup value. Ideal for scheduling and availability systems.

---

### Example 6: Wildcard Matching Position
```
=XMATCH("John*", Names, 2)
```
**Result:** Returns position of first name starting with "John" (matches John, Johnson, Johnny)

**Explanation:** Match mode 2 enables wildcards. Asterisk (*) matches any sequence. Returns the position of the first matching entry.

---

### Example 7: Single Character Wildcard
```
=XMATCH("A?C", ProductCodes, 2)
```
**Result:** Returns position of code matching pattern (ABC, A1C, AXC, etc.)

**Explanation:** Question mark (?) matches exactly one character. Useful for finding codes or patterns with variable single characters.

---

### Example 8: Find Last Occurrence
```
=XMATCH("Complete", StatusColumn, 0, -1)
```
**Result:** Returns position of the LAST "Complete" in the column

**Explanation:** Search mode -1 searches from bottom to top. When duplicates exist, this returns the position of the last (most recent) occurrence. Critical for chronological data.

---

### Example 9: Binary Search for Performance
```
=XMATCH(SearchValue, SortedColumn, 0, 2)
```
**Result:** Returns position using high-speed binary search (data must be sorted ascending)

**Explanation:** Search mode 2 uses binary search—exponentially faster for large datasets. For 1 million rows, binary search checks ~20 items vs. linear search's up to 1 million.

---

### Example 10: Binary Search Descending
```
=XMATCH(SearchValue, DescendingSortedRange, 0, -2)
```
**Result:** Returns position using binary search on descending-sorted data

**Explanation:** Search mode -2 uses binary search optimized for Z-A or largest-to-smallest sorted data. Data MUST be sorted in descending order.

---

### Example 11: Combined with INDEX (VLOOKUP Replacement)
```
=INDEX(ReturnColumn, XMATCH(LookupValue, LookupColumn))
```
**Result:** Returns value from ReturnColumn at the position found by XMATCH

**Explanation:** INDEX/XMATCH is the modern replacement for INDEX/MATCH. XMATCH finds the position, INDEX retrieves the value. More intuitive than MATCH's confusing match_type parameter.

---

### Example 12: Two-Dimensional Position Lookup
```
=INDEX(DataTable, XMATCH(RowValue, RowHeaders), XMATCH(ColValue, ColHeaders))
```
**Result:** Returns value at intersection of row and column matches

**Explanation:** Using two XMATCH functions with INDEX enables true two-dimensional lookups. Each XMATCH finds a position—one for row, one for column.

---

### Example 13: Horizontal Array Match
```
=XMATCH("Q3", A1:D1)
```
**Result:** Returns column position of "Q3" in the header row (e.g., 3 if Q3 is in C1)

**Explanation:** XMATCH works with horizontal arrays just as easily as vertical. The position returned is relative to the array—column position within A1:D1.

---

### Example 14: Check If Value Exists (Using ISNUMBER)
```
=ISNUMBER(XMATCH("SearchValue", DataRange))
```
**Result:** TRUE if value exists in range, FALSE if not

**Explanation:** XMATCH returns #N/A when not found. Wrapping in ISNUMBER converts this to a simple TRUE/FALSE existence check.

---

### Example 15: Position for Conditional Formatting
```
=XMATCH(B1, HighlightList)>0
```
**Result:** Used in conditional formatting to highlight cells whose values appear in a list

**Explanation:** When used in conditional formatting formulas, XMATCH > 0 (or wrapped in ISNUMBER) determines if a cell's value exists in a reference list.

---

### Example 16: Find Position in Unsorted Approximate Match
```
=XMATCH(85, Scores, -1, 1)
```
**Result:** Position of the largest score <= 85, even in unsorted data

**Explanation:** Unlike classic MATCH which required sorted data for approximate matches, XMATCH's match_mode -1 or 1 works on unsorted data when using default search_mode (1) or reverse search (-1).

---

### Example 17: Dynamic Column Selection
```
=INDEX(A:Z, 2, XMATCH(ColumnHeader, A1:Z1))
```
**Result:** Returns value from row 2 of whichever column has the specified header

**Explanation:** XMATCH determines which column contains the desired header, then INDEX retrieves from that column. Enables header-based column references.

---

### Example 18: Offset from Matched Position
```
=INDEX(DataColumn, XMATCH(SearchValue, LookupColumn) + 1)
```
**Result:** Returns value ONE ROW BELOW the matched position

**Explanation:** Since XMATCH returns a number, you can add or subtract to get offset positions. Useful for getting "next" or "previous" values relative to a match.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Lookup value not found in the array | Verify the value exists. Use IFERROR to handle gracefully: `=IFERROR(XMATCH(...), 0)` |
| `#VALUE!` | Lookup_array is multi-row AND multi-column | XMATCH requires a single row OR single column, not a range like A1:C10. |
| `#NAME?` | XMATCH not available in your Excel version | XMATCH requires Excel 365 or Excel 2021. Use MATCH for older versions. |
| `Wrong position returned` | Binary search on unsorted data | Binary search (modes 2 and -2) REQUIRES properly sorted data. Use mode 1 or -1 for unsorted. |
| `First match instead of last` | Forgot search_mode -1 for duplicates | Use `=XMATCH(value, range, 0, -1)` to find the last occurrence. |
| `Wildcards not working` | Not using match_mode 2 | Wildcards require explicit match_mode 2: `=XMATCH("*text*", range, 2)` |
| `Position seems off by 1` | Confusion with array start | XMATCH returns position within the array. If array starts at row 5, position 1 corresponds to row 5. |
| `#SPILL!` | XMATCH used in array context with blocked spill | Clear the cells where results need to spill. |

## Use Cases

### [[Dynamic INDEX-Based Data Retrieval]]

**Scenario:** Build flexible lookup systems using INDEX/XMATCH that can retrieve data from any column direction and handle complex multi-criteria scenarios.

**Implementation:**
```
Basic INDEX/XMATCH lookup:
=INDEX(PriceColumn, XMATCH(ProductID, IDColumn))

Reverse lookup (return from left of search column):
=INDEX(A:A, XMATCH(SearchValue, C:C))

Two-dimensional lookup:
=INDEX(SalesTable, XMATCH(ProductName, Products), XMATCH(MonthName, Months))

Multiple column return (array formula):
=INDEX(A:D, XMATCH(ID, A:A), {1,2,3,4})
```

**Business Application:** Customer service systems retrieving complete customer records by ID. Financial reports pulling values from intersection of account and period. Inventory systems looking up products with pricing from any column arrangement.

**Technical Details:** INDEX/XMATCH provides the same capability as XLOOKUP but with explicit position control. The position from XMATCH can be reused in multiple INDEX calls for efficiency. Unlike MATCH's confusing 1/0/-1 parameters, XMATCH's match_mode values are intuitive (0=exact, -1=smaller, 1=larger).

---

### [[Array Position Analysis]]

**Scenario:** Determine where values fall within sorted lists for ranking, percentile calculations, tier assignments, and relative positioning analysis.

**Implementation:**
```
Find rank position in sorted list:
=XMATCH(Value, SortedScores, -1)

Determine tier number:
=XMATCH(Amount, TierThresholds, -1)

Calculate percentile position:
=XMATCH(Score, SORT(AllScores), -1) / COUNT(AllScores)

Find insertion point for new value:
=XMATCH(NewValue, SortedList, 1)
```

**Business Application:** Sales leaderboard showing rep rankings. Commission tier determination for payroll. Academic grading determining which percentile a score falls into. Inventory systems determining where to insert new items in sorted catalogs.

**Technical Details:** Match mode -1 (next smaller) finds the position where a value fits in a tier structure. This position can directly index into a parallel array of tier labels, rates, or descriptions. For percentiles, dividing the position by total count gives the approximate percentile.

---

### [[Last Entry Detection in Logs]]

**Scenario:** Find the position of the most recent occurrence in chronological logs, transaction histories, and audit trails where the same key appears multiple times.

**Implementation:**
```
Position of last status update:
=XMATCH(ItemID, LogItemColumn, 0, -1)

Get last status:
=INDEX(StatusColumn, XMATCH(ItemID, LogItemColumn, 0, -1))

Count entries before last occurrence:
=XMATCH(SearchValue, DataRange, 0, -1) - 1

Find row number of last entry:
=ROW(DataRange) + XMATCH(SearchValue, DataRange, 0, -1) - 1
```

**Business Application:** Order tracking showing latest status from order history logs. Audit systems retrieving the most recent change to any record. Version control finding the latest version of a document. Customer service seeing the last interaction with a customer.

**Technical Details:** Search mode -1 searches from the end of the array forward, returning the last match position. Combined with INDEX, this retrieves the actual value. The position can be used to calculate how many previous entries exist (position - 1) or converted to actual row numbers by adding the range's starting row.

---

### [[Existence and Validation Checking]]

**Scenario:** Validate that values exist in reference lists, check for duplicates, and implement data validation logic without complex formulas.

**Implementation:**
```
Check if value exists in list:
=ISNUMBER(XMATCH(TestValue, ValidList))

Count occurrences (existence as 1 or 0):
=--ISNUMBER(XMATCH(Value, Range))

Validate multiple values at once:
=MAP(TestRange, LAMBDA(x, ISNUMBER(XMATCH(x, ValidList))))

Conditional formatting formula:
=ISNUMBER(XMATCH($A1, HighlightList))
```

**Business Application:** Data entry validation ensuring product codes exist in master list. Invoice validation checking that vendor IDs are active. Duplicate detection in imports. Conditional formatting to highlight items that appear in exception lists.

**Technical Details:** XMATCH returns #N/A when not found, which converts to FALSE with ISNUMBER. The double-negative (--) converts TRUE/FALSE to 1/0 for summing. In conditional formatting, the ISNUMBER(XMATCH()) pattern efficiently checks list membership.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365 (subscription) and Excel 2021 (perpetual license) only
- **Not available in:** Excel 2019, Excel 2016, Excel 2013, or earlier
- **Dynamic arrays:** Full support—works within array formulas
- **Performance:** Optimized for large datasets, especially with binary search modes
- **VBA compatibility:** Can be called from VBA using Application.WorksheetFunction.XMatch

### Google Sheets

- **Availability:** Added to Google Sheets in 2022
- **Syntax:** Identical to Excel
- **Array behavior:** Native array support
- **Performance:** Comparable to Excel for most datasets
- **Collaboration:** Real-time updates when data changes

### Key Differences

| Feature | Excel 365 | Excel 2021 | Google Sheets | Excel 2019 and earlier |
|---------|-----------|------------|---------------|------------------------|
| XMATCH available | Yes | Yes | Yes | No |
| Binary search modes | Yes | Yes | Yes | N/A |
| Alternative function | N/A | N/A | N/A | MATCH |
| Default match behavior | Exact (0) | Exact (0) | Exact (0) | Approximate (1) |
| Search direction control | Yes | Yes | Yes | No |

### Version Compatibility Workaround

For files that may be opened in older Excel versions:
```
XMATCH equivalent:
=XMATCH(A1, B:B, 0)

Classic MATCH equivalent:
=MATCH(A1, B:B, 0)

XMATCH with next smaller:
=XMATCH(A1, B:B, -1)

Classic MATCH (requires sorted ascending):
=MATCH(A1, B:B, 1)

XMATCH last match (no direct equivalent):
=XMATCH(A1, B:B, 0, -1)

Classic workaround for last match:
=MAX(IF(B:B=A1, ROW(B:B)-ROW(B1)+1))  (array formula with Ctrl+Shift+Enter)
```

## Tips and Best Practices

1. **XMATCH defaults to exact match, unlike MATCH:** Classic MATCH defaulted to approximate match (sorted ascending). XMATCH defaults to exact match (0), eliminating accidental wrong matches.

2. **Use search_mode -1 to find the last occurrence:** When you have duplicates and need the most recent/last entry, `=XMATCH(value, range, 0, -1)` searches from the end.

3. **Binary search requires sorted data:** Search modes 2 and -2 are dramatically faster but ONLY work if data is properly sorted. Unsorted data returns wrong results with no error.

4. **Combine with INDEX for XLOOKUP-like functionality:** `=INDEX(ReturnRange, XMATCH(Value, SearchRange))` is equivalent to XLOOKUP but gives you the position number for other uses.

5. **Position is relative to the array, not the worksheet:** If your lookup_array is B5:B100, position 1 is B5, position 2 is B6, etc. To get actual row numbers, add ROW(B5)-1 to the result.

6. **Use ISNUMBER(XMATCH()) for existence checks:** This pattern returns TRUE if the value exists, FALSE if not. Cleaner than checking for #N/A errors.

7. **Wildcards require explicit match_mode 2:** Unlike some functions where wildcards work automatically, XMATCH only interprets * and ? as wildcards when match_mode is set to 2.

8. **Match_mode works on unsorted data with default search:** Unlike MATCH's approximate match that required sorted data, XMATCH's -1 (next smaller) and 1 (next larger) work on unsorted data when search_mode is 1 or -1.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[MATCH]] | Classic position finder | Legacy compatibility or when XMATCH unavailable |
| [[XLOOKUP]] | Returns value instead of position | When you need the value, not the position |
| [[SEARCH]] | Text position within a string | Finding character position within text, not array position |
| [[FIND]] | Case-sensitive text position | Finding exact text position within strings |

### Commonly Used Together

**[[INDEX]]** - Retrieve value by position

*Combined with XMATCH for flexible lookups:*
```
=INDEX(DataRange, XMATCH(SearchValue, SearchRange))
```
XMATCH finds position, INDEX retrieves the value at that position.

---

**[[XLOOKUP]]** - Modern value lookup

*XMATCH as alternative when position needed:*
```
Position: =XMATCH(Value, Range)
Value: =XLOOKUP(Value, Range, ReturnRange)
```
Use XMATCH when you need the position number itself; use XLOOKUP when you just need the value.

---

**[[FILTER]]** - Filter array by criteria

*Combined with XMATCH for filtered position:*
```
=XMATCH(Value, FILTER(Range, Criteria))
```
Find position within a filtered subset of data.

---

**[[SORT]]** - Sort arrays

*Combined with XMATCH for sorted position:*
```
=XMATCH(Value, SORT(Range), 0, 2)
```
Sort dynamically then use binary search for performance.

---

**[[IFERROR]]** - Handle not-found errors

*Combined with XMATCH for default position:*
```
=IFERROR(XMATCH(Value, Range), 0)
```
Returns 0 instead of #N/A when value not found.

---

**[[ISNUMBER]]** - Existence checking

*Combined with XMATCH for boolean existence:*
```
=ISNUMBER(XMATCH(Value, Range))
```
Returns TRUE if value exists in range, FALSE otherwise.

## Official Documentation

- **Microsoft Excel:** [XMATCH function](https://support.microsoft.com/en-us/office/xmatch-function-d966da31-7a6b-4a13-a1c6-5a33ed6a0312)
- **Google Sheets:** [XMATCH function](https://support.google.com/docs/answer/12405947)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2019 (rolling release) | Introduced alongside XLOOKUP as part of dynamic arrays |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Google Sheets | 2022 | Added for Excel compatibility |
| Excel Web | 2019 | Full feature parity with desktop |
| Excel Mobile | 2020 | iOS and Android support |

---

*Last updated: 2026-01-10*
