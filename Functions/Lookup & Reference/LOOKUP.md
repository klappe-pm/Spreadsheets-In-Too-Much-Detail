---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- data-retrieval
- legacy-functions
- approximate-match
- vector-lookup
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Vector Lookup
- Array Lookup
- Legacy Lookup
tags:
- lookup
- reference
- data-retrieval
- legacy
- approximate-match
---

# LOOKUP

## Description

**LOOKUP** is Excel and Google Sheets' original lookup function, predating VLOOKUP and HLOOKUP. It comes in two forms: the **vector form** (look up in one row or column, return from another) and the **array form** (look up in a table's first row or column). While VLOOKUP and HLOOKUP have largely replaced LOOKUP for most use cases, LOOKUP retains one significant advantage: it can return the *last* match in a sorted list, making it valuable for specific scenarios like finding the most recent entry.

**The vector form** is most common: `=LOOKUP(lookup_value, lookup_vector, result_vector)`. You specify what to search for, where to search, and where to get the result. Unlike VLOOKUP, the lookup and result ranges can be in any position relative to each other—they don't need to be in the same table, and the result can be to the left of the lookup column.

**Critical limitation: LOOKUP only does approximate matching.** It always assumes the lookup vector is sorted in ascending order and finds the largest value that is less than or equal to the lookup value. There is no exact match option. If you need exact matching, use VLOOKUP with FALSE, INDEX/MATCH with 0, or XLOOKUP. This approximate-match-only behavior makes LOOKUP unsuitable for most ID lookups but perfect for range-based lookups like tax brackets.

**The "last match" trick:** Because LOOKUP finds the largest value ≤ the search value, you can use it with binary arrays to find the last TRUE or last match: `=LOOKUP(2,1/(A:A="Target"),B:B)` returns the value from column B corresponding to the last "Target" in column A. This pattern is widely used but somewhat arcane—understand it before deploying it.

## Syntax

> [!f(x)] LOOKUP Syntax
>
> **Vector Form (most common):**
> ```
> =LOOKUP(lookup_value, lookup_vector, [result_vector])
> ```
>
> **Array Form (legacy):**
> ```
> =LOOKUP(lookup_value, array)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `lookup_value` | Yes | The value to search for. Can be a number, text, logical value, or cell reference. |
| `lookup_vector` | Yes | A single row or column range to search. Must be sorted in ascending order for correct results. |
| `result_vector` | No | A single row or column range from which to return a value. Must be the same size as lookup_vector. If omitted, returns from lookup_vector itself. |
| `array` | Yes (array form) | A range of cells containing values to compare and return. If the array is wider than tall, LOOKUP searches the first row; if taller than wide, it searches the first column. |

### Return Value

Returns the value from result_vector (or lookup_vector if result_vector is omitted) at the position where the largest value less than or equal to lookup_value was found. Returns #N/A if lookup_value is smaller than all values in lookup_vector.

## Examples

> [!f(x)] LOOKUP Examples

### Example 1: Basic Vector Lookup
```
=LOOKUP(50, A2:A10, B2:B10)
```
**Result:** Returns value from B column at position where 50 (or largest value ≤50) was found in A column

**Explanation:** Searches A2:A10 for the largest value ≤50, then returns the corresponding value from B2:B10. If A2:A10 contains {10,20,30,40,50,60}, it finds 50 at position 5 and returns B6.

---

### Example 2: Approximate Match for Grade Bands
```
=LOOKUP(B2, {0,60,70,80,90}, {"F","D","C","B","A"})
```
**Result:** Returns letter grade based on score in B2

**Explanation:** If B2 is 85, LOOKUP finds 80 (largest ≤85) and returns "B". This is LOOKUP's strength—range-based lookups without explicit upper bounds. The cutoffs {0,60,70,80,90} must be in ascending order.

---

### Example 3: Tax Bracket Lookup
```
=LOOKUP(Income, TaxBrackets, TaxRates)
```
**Result:** Returns the tax rate for the income's bracket

**Explanation:** Given brackets {0, 10000, 40000, 85000} with rates {10%, 12%, 22%, 24%}, an income of $50,000 matches the $40,000 bracket (largest ≤ income) and returns 22%.

---

### Example 4: Finding the Last Match (Classic Pattern)
```
=LOOKUP(2, 1/(A:A="Target"), B:B)
```
**Result:** Returns value from column B for the LAST occurrence of "Target" in column A

**Explanation:** `1/(A:A="Target")` creates an array of 1s (where TRUE) and #DIV/0! errors (where FALSE). LOOKUP(2,...) finds the largest value ≤2, which is the last 1. This pattern finds the last match instead of the first.

---

### Example 5: Last Non-Empty Cell in a Column
```
=LOOKUP(2, 1/(A:A<>""), A:A)
```
**Result:** Returns the last non-empty value in column A

**Explanation:** Another application of the "last match" pattern. `1/(A:A<>"")` returns 1 for non-empty cells. LOOKUP finds the last 1 and returns the corresponding value from column A.

---

### Example 6: Single Vector (No Result Vector)
```
=LOOKUP(100, A2:A50)
```
**Result:** Returns the value from A2:A50 that is ≤100 and largest

**Explanation:** When result_vector is omitted, LOOKUP returns from the lookup_vector itself. If A2:A50 contains {10,25,50,75,100,125}, and you search for 100, it returns 100. Search for 110, and you still get 100 (largest ≤110).

---

### Example 7: Looking Up with Wildcards (Not Supported)
```
=LOOKUP("App*", A2:A10, B2:B10)
```
**Result:** #N/A (likely)

**Explanation:** Unlike VLOOKUP or MATCH, LOOKUP does NOT support wildcards. The asterisk is treated literally. For wildcard lookups, use VLOOKUP, MATCH, or XLOOKUP instead.

---

### Example 8: Array Form (Horizontal)
```
=LOOKUP("Q3", A1:D1, A2:D2)
```
**Result:** Returns value from row 2 at position where "Q3" was found in row 1

**Explanation:** When lookup and result vectors are rows, LOOKUP works horizontally. If A1:D1 is {"Q1","Q2","Q3","Q4"} and A2:D2 contains revenue figures, this returns Q3's revenue.

---

### Example 9: Finding Position of Last TRUE in Array
```
=LOOKUP(TRUE, {FALSE;TRUE;FALSE;TRUE;FALSE}, {1;2;3;4;5})
```
**Result:** 4

**Explanation:** TRUE > FALSE in Excel's sort order, so LOOKUP finds the last TRUE (position 4) and returns 4. Useful for array formula patterns finding the last occurrence of a condition.

---

### Example 10: Cumulative Total Lookup
```
=LOOKUP(TargetAmount, CumulativeSums, ItemNames)
```
**Result:** Returns the item where cumulative sum first exceeds target

**Explanation:** For inventory picking or budget allocation, cumulative sums in ascending order let LOOKUP find which item corresponds to a running total target. If cumulative sums are {100,200,350,500} and target is 275, returns the item at position 3 (sum=350).

---

### Example 11: Reverse Lookup (Right to Left)
```
=LOOKUP(TargetValue, C2:C100, A2:A100)
```
**Result:** Returns value from column A based on match in column C

**Explanation:** Unlike VLOOKUP which only looks left-to-right, LOOKUP's vector form allows any column arrangement. Here we search column C and return from column A (which is to the left).

---

### Example 12: Nested LOOKUP for Two-Dimensional Selection
```
=LOOKUP(ColHeader, HeaderRow, LOOKUP(RowHeader, RowLabels, DataRange))
```
**Result:** Two-dimensional lookup using nested LOOKUPs

**Explanation:** While INDEX/MATCH is cleaner for 2D lookups, nested LOOKUPs can achieve similar results when data is sorted. The inner LOOKUP returns a row, and the outer LOOKUP finds the column within it.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | lookup_value is smaller than all values in lookup_vector | Ensure lookup_vector contains values ≤ lookup_value, or handle with IFERROR. |
| `#N/A` | lookup_vector and result_vector have different sizes | Both vectors must have the same number of elements. |
| `Wrong result` | lookup_vector is not sorted in ascending order | LOOKUP requires ascending sort. Sort the data or use VLOOKUP/XLOOKUP with exact match. |
| `#VALUE!` | lookup_vector contains errors | Clean error values from the lookup range before using LOOKUP. |
| `#REF!` | Referenced ranges have been deleted | Update references to valid ranges. |
| `Unexpected match` | Trying to do exact match but LOOKUP found approximate | LOOKUP only does approximate matching. Use VLOOKUP(FALSE), INDEX/MATCH(0), or XLOOKUP for exact. |

## Use Cases

### [[Range-Based Classification]]

**Scenario:** Assign categories, grades, or tiers based on numeric values falling within ranges—without needing to define upper bounds explicitly.

**Implementation:**
```
Grade assignment:
=LOOKUP(Score, {0,60,70,80,90}, {"F","D","C","B","A"})

Commission tier:
=LOOKUP(SalesAmount, {0,10000,25000,50000,100000}, {5%,7%,10%,12%,15%})

Age group:
=LOOKUP(Age, {0,13,20,30,40,50,65}, {"Child","Teen","20s","30s","40s","50s","Senior"})
```

**Business Application:** Educational grading systems, sales commission structures, insurance risk tiers, tax bracket calculations, and any classification system based on numeric ranges.

**Technical Details:** LOOKUP's approximate matching is ideal here because you define lower bounds of each range, and LOOKUP finds which range the value falls into. This is cleaner than nested IFs or VLOOKUP with TRUE.

---

### [[Finding Last Occurrence]]

**Scenario:** Retrieve data associated with the most recent or last occurrence of a value—like the last transaction for a customer, the last date an event occurred, or the most recent entry in a log.

**Implementation:**
```
Last value for a customer:
=LOOKUP(2, 1/(CustomerColumn=CustomerID), ValueColumn)

Last date of occurrence:
=LOOKUP(2, 1/(EventColumn="Error"), DateColumn)

Last non-empty entry:
=LOOKUP(2, 1/(A:A<>""), A:A)
```

**Business Application:** Finding last transaction amount, most recent login date, latest status update, or final entry in a chronological log. Anywhere "last" matters more than "first."

**Technical Details:** The `LOOKUP(2, 1/(condition), result)` pattern works because `1/TRUE=1` and `1/FALSE=#DIV/0!`. LOOKUP ignores errors and finds the last 1 (which is always <2). This is an implicit way to find the last match.

---

### [[Reverse Direction Lookups]]

**Scenario:** Look up a value in one column and return from a column to its left—something VLOOKUP cannot do without helper columns.

**Implementation:**
```
Look right, return left:
=LOOKUP(EmployeeID, EmployeeIDs_Column, Names_Column_ToLeft)

Look in column C, return from column A:
=LOOKUP(SearchValue, C2:C100, A2:A100)
```

**Business Application:** Any data structure where the lookup column isn't the leftmost column. Legacy databases, imported data with inconvenient column ordering, or normalized data structures.

**Technical Details:** LOOKUP's vector form is direction-agnostic—specify any column as lookup_vector and any other as result_vector. This flexibility predates INDEX/MATCH and remains useful for simple cases.

---

### [[Cumulative Threshold Matching]]

**Scenario:** Find which item in a list corresponds to a cumulative total reaching a certain threshold—useful for inventory allocation, budget distribution, or progressive pricing.

**Implementation:**
```
Inventory picking (FIFO):
=LOOKUP(QuantityNeeded, CumulativeStock, BinLocations)

Budget allocation:
=LOOKUP(RequestedAmount, CumulativeBudget, DepartmentNames)
```

**Business Application:** Warehouse picking lists determining which bins to pull from, budget systems finding which allocation covers a request, or lottery-style selections based on weighted probabilities.

**Technical Details:** Pre-calculate cumulative sums in ascending order. LOOKUP finds where the cumulative total first exceeds (or equals) the target. Combine with arithmetic to determine exact quantities from multiple sources.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (one of Excel's original functions)
- **Sorting requirement:** Absolute—unsorted data produces wrong results silently
- **Wildcards:** Not supported in LOOKUP
- **Performance:** Efficient on sorted data due to binary search algorithm
- **Array form:** Supported but rarely used

### Google Sheets

- **Availability:** All versions
- **Sorting requirement:** Same as Excel—must be sorted ascending
- **Syntax:** Identical to Excel
- **Performance:** Comparable to Excel
- **Array form:** Supported

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic behavior | Identical | Identical |
| Sorting requirement | Must be ascending | Must be ascending |
| Wildcard support | No | No |
| Error handling | Standard | Standard |
| Vector form | Full support | Full support |
| Array form | Full support | Full support |

## Tips and Best Practices

1. **Always ensure data is sorted ascending:** LOOKUP assumes ascending sort order. Unsorted data produces incorrect results without any error message—a silent failure.

2. **Use for range lookups, not exact matches:** LOOKUP shines for tax brackets, grade scales, and tiered systems. For exact ID matching, use VLOOKUP(FALSE), INDEX/MATCH(0), or XLOOKUP.

3. **Master the "last match" pattern:** `=LOOKUP(2, 1/(condition), result)` finds the last occurrence. Memorize this—it's one of LOOKUP's unique capabilities.

4. **LOOKUP vectors can be anywhere:** Unlike VLOOKUP's left-to-right restriction, LOOKUP's lookup_vector and result_vector can be in any columns. This enables reverse lookups without INDEX/MATCH.

5. **Keep vectors the same size:** lookup_vector and result_vector must have identical dimensions. Mismatched sizes cause #N/A errors.

6. **Consider XLOOKUP instead:** In modern Excel, XLOOKUP offers LOOKUP's flexibility plus exact matching, error handling, and wildcard support. Use LOOKUP mainly for backward compatibility or the "last match" trick.

7. **Avoid wildcards with LOOKUP:** Unlike VLOOKUP and MATCH, LOOKUP doesn't support * and ? wildcards. Characters are matched literally.

8. **LOOKUP ignores errors in the "last match" pattern:** The `1/(condition)` trick works precisely because LOOKUP skips over #DIV/0! errors when searching.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VLOOKUP]] | Vertical lookup with exact/approximate modes | When you need exact matching or data is in standard table format |
| [[HLOOKUP]] | Horizontal lookup for row-based data | When data is organized horizontally |
| [[XLOOKUP]] | Modern lookup with all capabilities | Available in Excel 365/2019 for cleaner syntax and more features |
| [[INDEX]]+[[MATCH]] | Flexible lookup combination | For complex lookups, multiple criteria, or when XLOOKUP unavailable |

### Commonly Used Together

**[[IFERROR]]** - Handle #N/A errors

*Combined with LOOKUP for graceful failure handling:*
```
=IFERROR(LOOKUP(value, lookup_vector, result_vector), "Not found")
```
Catches #N/A when lookup_value is smaller than all values in lookup_vector.

---

**[[MATCH]]** - Find position in a range

*Alternative to LOOKUP for finding positions:*
```
=MATCH(value, range, 1)  vs  =LOOKUP(value, range, {1;2;3;...})
```
MATCH with match_type=1 is similar to LOOKUP but returns position rather than value.

---

**[[INDEX]]** - Return value at position

*Combined with MATCH as LOOKUP alternative:*
```
=INDEX(result_range, MATCH(value, lookup_range, 1))
```
INDEX/MATCH replicates LOOKUP behavior with more control.

---

**[[IF]]** - Conditional logic

*Combined with LOOKUP for conditional lookups:*
```
=IF(condition, LOOKUP(val1, range1, result1), LOOKUP(val2, range2, result2))
```
Selects between different lookup operations based on conditions.

---

**[[ISNA]]** - Test for #N/A error

*Combined with LOOKUP for error checking:*
```
=IF(ISNA(LOOKUP(value, range)), "Below minimum", LOOKUP(value, range, result))
```
Tests whether LOOKUP will return #N/A before executing the main lookup.

## Official Documentation

- **Microsoft Excel:** [LOOKUP function](https://support.microsoft.com/en-us/office/lookup-function-446d94af-663b-451d-8251-369d5e3864cb)
- **Google Sheets:** [LOOKUP function](https://support.google.com/docs/answer/3256570)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original lookup function; predates VLOOKUP/HLOOKUP |
| Excel 5.0 | 1993 | Array form added |
| Excel 2007 | Unchanged | No significant changes |
| Excel 365 | 2019 | XLOOKUP introduced as modern alternative |
| Google Sheets | Original launch (2006) | Full compatibility with Excel behavior |

---

*Last updated: 2026-01-10*
