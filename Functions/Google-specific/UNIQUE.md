---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - google-specific
  - data-manipulation
subTopics:
  - deduplication
  - unique-values
  - distinct-values
  - array-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - unique-values
  - distinct
  - remove-duplicates
  - deduplicate
tags:
  - array-functions
  - deduplication
  - data-cleaning
  - dynamic-arrays
  - lookup-lists
---

# UNIQUE

## Description

The **UNIQUE function** returns the unique rows from a range or array, removing duplicate entries to produce a list of distinct values. This essential data manipulation function is invaluable for creating deduplicated lists, building dropdown validation sources, identifying distinct categories in datasets, and any situation where you need to eliminate repetition and extract only unique records. UNIQUE examines each row in the input and includes it in the output only if it hasn't been seen before, preserving the order of first occurrence.

The function works on both single-column data (extracting unique values) and multi-column data (extracting unique row combinations). When applied to multiple columns, UNIQUE considers the entire row as a unit—two rows are considered duplicates only if ALL their values match across all columns. This enables deduplication based on compound keys, such as finding unique combinations of Customer and Product, or unique Name-Email-Phone records. The output maintains all columns from the input, just with duplicate rows removed.

UNIQUE offers optional parameters that control how uniqueness is determined. The `by_column` parameter changes comparison from row-based to column-based (useful for transposed data). The `exactly_once` parameter, when TRUE, returns only values that appear exactly once in the input—filtering out any value that has duplicates rather than keeping one copy of each. These options enable both standard deduplication and identification of truly singular entries.

Originally a Google Sheets-exclusive function, UNIQUE has now been adopted by Microsoft Excel as part of the dynamic array functions introduced in Excel 365 (2018+). The implementations are largely compatible, though Google Sheets has had UNIQUE longer and it works seamlessly with other Sheets-specific functions like QUERY. For cross-platform work, UNIQUE formulas typically work in both environments with minimal or no modification.

## Syntax

> [!f(x)] UNIQUE Syntax
>
> ```
> =UNIQUE(range, [by_column], [exactly_once])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `range` | Yes | The range or array from which to extract unique rows. Can be a cell range (A1:B100), an array constant, or an array returned by another function. For single-column uniqueness, provide one column; for row-based uniqueness, provide multiple columns. |
| `by_column` | No | If TRUE, compares columns instead of rows (for horizontally arranged data). Default is FALSE (compare rows). Use TRUE when your data is arranged in rows rather than columns. |
| `exactly_once` | No | If TRUE, returns only rows/columns that appear exactly once in the source (no duplicates at all). If FALSE (default), returns one copy of each unique value regardless of how many times it appears. |

### Return Value

Returns an array containing the unique rows (or columns if by_column is TRUE) from the input range. The output preserves the column structure of the input—if input has 3 columns, output has 3 columns. The number of rows in output depends on how many unique combinations exist. Values appear in order of first occurrence in the input.

## Examples

> [!f(x)] UNIQUE Examples

### Example 1: Basic Single-Column Unique
```
=UNIQUE(A1:A10)
```
**Scenario:** A1:A10 contains: Apple, Banana, Apple, Cherry, Banana, Date, Apple, Cherry, Date, Date
**Result:** Apple, Banana, Cherry, Date (4 rows)

**Explanation:** UNIQUE removes duplicate values, keeping only the first occurrence of each. The result is a clean list of distinct values.

---

### Example 2: Multi-Column Unique Combinations
```
=UNIQUE(A1:B10)
```
**Scenario:** Columns A and B contain Customer-Product pairs with some rows repeated
**Result:** Only unique Customer-Product combinations

**Explanation:** When applied to multiple columns, UNIQUE treats each row as a unit. Rows are only duplicates if ALL columns match.

---

### Example 3: Create Dropdown Source from Data
```
=UNIQUE(Products!A:A)
```
**Scenario:** Creating a list of products for a data validation dropdown
**Result:** Unique product names from the Products sheet

**Explanation:** UNIQUE on a data column creates perfect dropdown source lists. The list automatically updates as new products are added.

---

### Example 4: Exactly Once - Find Singular Values
```
=UNIQUE(A1:A20, FALSE, TRUE)
```
**Scenario:** A1:A20 contains various values; find those appearing only once
**Result:** Only values that have no duplicates

**Explanation:** With exactly_once=TRUE, UNIQUE filters to only values appearing exactly once—useful for finding outliers or errors.

---

### Example 5: Column-Based Unique
```
=UNIQUE(A1:J1, TRUE)
```
**Scenario:** Row 1 contains values across columns A-J with duplicates
**Result:** Unique values from the row (arranged horizontally)

**Explanation:** by_column=TRUE switches to column-wise comparison for horizontally arranged data.

---

### Example 6: Unique with SORT for Alphabetical List
```
=SORT(UNIQUE(A:A))
```
**Scenario:** Create an alphabetically sorted list of unique values
**Result:** Deduplicated and sorted list

**Explanation:** UNIQUE removes duplicates, SORT orders them. This is a common pattern for creating clean reference lists.

---

### Example 7: Unique with FILTER for Conditional Uniqueness
```
=UNIQUE(FILTER(A:B, C:C="Active"))
```
**Scenario:** Get unique combinations from columns A-B, but only for rows where column C is "Active"
**Result:** Unique combinations from filtered subset

**Explanation:** FILTER first selects qualifying rows, then UNIQUE deduplicates. Order matters—filter first, then unique.

---

### Example 8: Count Unique Values
```
=COUNTA(UNIQUE(A:A))
```
**Scenario:** Determine how many distinct values exist in a column
**Result:** A single number representing the count of unique values

**Explanation:** COUNTA counts the cells in UNIQUE's output. This is equivalent to COUNTUNIQUE(A:A).

---

### Example 9: Unique from SPLIT Results
```
=UNIQUE(FLATTEN(SPLIT(A1, ",")))
```
**Scenario:** A1 contains "red,blue,red,green,blue,yellow"
**Result:** red, blue, green, yellow

**Explanation:** SPLIT creates an array, FLATTEN makes it vertical, UNIQUE removes duplicates. Great for processing comma-separated lists.

---

### Example 10: Unique with Multiple Conditions via FILTER
```
=UNIQUE(FILTER(A:C, (B:B="Sales")*(C:C>1000)))
```
**Scenario:** Unique rows from columns A-C where B="Sales" AND C>1000
**Result:** Unique matching combinations

**Explanation:** Multiply conditions together for AND logic in FILTER, then apply UNIQUE to the result.

---

### Example 11: Cross-Reference Unique Values
```
=FILTER(UNIQUE(Sheet1!A:A), ISNA(MATCH(UNIQUE(Sheet1!A:A), Sheet2!A:A, 0)))
```
**Scenario:** Find unique values in Sheet1 that don't exist in Sheet2
**Result:** Values unique to Sheet1

**Explanation:** This pattern finds differences between two lists—values in one but not the other.

---

### Example 12: Dynamic Unique with Named Range
```
=UNIQUE(ProductList)
```
**Scenario:** ProductList is a named range referencing A2:A
**Result:** Unique products from the named range

**Explanation:** Named ranges work with UNIQUE for cleaner, more maintainable formulas.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#REF!` | Output array would overwrite existing data | Ensure sufficient empty rows below the formula |
| `#VALUE!` | Invalid parameter values | Verify by_column and exactly_once are TRUE/FALSE |
| `Empty result` | No unique values (all duplicates with exactly_once=TRUE) | Check if exactly_once should be FALSE |
| `Unexpected duplicates in output` | Multi-column comparison not matching as expected | Verify all columns are included; check for trailing spaces |
| `#N/A in source causing issues` | Error values in input range | Filter or handle errors before UNIQUE |
| `Performance issues` | Very large ranges with full column references | Limit range to actual data (A2:A1000 vs A:A) |
| `Different results than expected` | Case sensitivity or whitespace differences | Standardize data with UPPER/LOWER/TRIM first |
| `Function not working` | Excel version without dynamic arrays | Requires Excel 365 or Google Sheets |

## Use Cases

### [[Building Dynamic Dropdown Lists]]

**Scenario:** A data entry form needs dropdown lists that automatically update as new categories, products, or options are added to the source data. Rather than manually maintaining static lists, the dropdowns should reflect current data.

**Implementation:**
```
Dropdown list structure:
Source data: DataEntry!A:A (Product column with entries)
Dropdown source: =SORT(UNIQUE(FILTER(DataEntry!A:A, DataEntry!A:A<>"")))

For dependent dropdowns (Category -> Product):
Categories: =SORT(UNIQUE(DataEntry!A:A))
Products for selected category:
=SORT(UNIQUE(FILTER(DataEntry!B:B, DataEntry!A:A=SelectedCategory)))

Named range approach:
1. Create named range: UniqueCategories = SORT(UNIQUE(Data!A:A))
2. Use in Data Validation: =UniqueCategories

With error handling:
=IFERROR(SORT(UNIQUE(FILTER(A:A, A:A<>""))), "No data")
```

**Business Application:** Static dropdown lists require constant maintenance as data changes. UNIQUE-based dropdowns eliminate this overhead—when someone enters a new product in the source data, it automatically appears in all dropdown lists. This reduces data entry errors and ensures consistency.

**Technical Details:** Combine with SORT for alphabetical ordering. FILTER out blanks to prevent empty options. Named ranges make data validation setup cleaner. For dependent dropdowns, FILTER by the parent selection before applying UNIQUE.

---

### [[Customer Segmentation Analysis]]

**Scenario:** A marketing team needs to analyze customer segments by identifying unique customer-region combinations, unique product categories purchased by each customer, and customers who have made only one purchase (exactly_once use case).

**Implementation:**
```
Segmentation analysis:
Unique customer-region pairs:
=UNIQUE(FILTER({Customers!A:A, Customers!C:C}, Customers!A:A<>""))

Unique categories per customer:
=QUERY(UNIQUE({Orders!A:A, Orders!C:C}), "SELECT Col1, COUNT(Col2) GROUP BY Col1")

One-time customers (exactly_once):
=UNIQUE(Orders!A:A, FALSE, TRUE)

Customers with multiple orders (appear more than once):
=UNIQUE(FILTER(Orders!A:A, COUNTIF(Orders!A:A, Orders!A:A)>1))

Customer-Product matrix (unique combinations):
=UNIQUE({Customers!A:A, Products!B:B})

Regional customer counts:
=QUERY(UNIQUE({Customers!A:A, Customers!C:C}), "SELECT Col2, COUNT(Col1) GROUP BY Col2")
```

**Business Application:** Understanding customer segments requires identifying unique combinations and patterns. UNIQUE enables quick identification of distinct customer-attribute pairs without manual deduplication. The exactly_once option helps identify one-time customers for retention campaigns.

**Technical Details:** Array syntax {} combines columns for multi-column uniqueness. QUERY on UNIQUE results enables aggregation. COUNTIF identifies repetition counts. FILTER before UNIQUE handles conditional segmentation.

---

### [[Data Quality and Deduplication Reporting]]

**Scenario:** A data quality team needs to audit a database extract for duplicates, identify records that should be unique but aren't, and create a clean master list for import back into the system.

**Implementation:**
```
Deduplication workflow:
Original record count:
=COUNTA(A:A)-1

Unique record count:
=ROWS(UNIQUE(A:A))-1

Duplicate record count:
=COUNTA(A:A)-ROWS(UNIQUE(A:A))

Records with duplicates (values appearing >1 time):
=UNIQUE(FILTER(A:A, COUNTIF(A:A, A:A)>1))

Duplicate details (value and count):
=QUERY(A:A, "SELECT A, COUNT(A) GROUP BY A HAVING COUNT(A) > 1")

Clean master list for export:
=SORT(UNIQUE(FILTER(A:B, A:A<>"")))

Identify first occurrence of each duplicate group:
Use MATCH with UNIQUE to mark first occurrences for keeping
```

**Business Application:** Duplicate data causes problems across systems—multiple mailings, incorrect counts, synchronization errors. UNIQUE creates clean datasets for migration or reporting. The analysis formulas quantify the scope of duplication issues.

**Technical Details:** ROWS(UNIQUE(...)) counts unique values. Subtracting from total gives duplicate count. COUNTIF>1 identifies values that have duplicates. QUERY provides detailed duplicate analysis with counts.

---

### [[Building Lookup Tables from Transaction Data]]

**Scenario:** A finance system exports transaction data without separate reference tables. The team needs to extract unique vendors, cost centers, and GL accounts from transaction data to build reference tables for validation and reporting.

**Implementation:**
```
Reference table extraction:
Unique vendors:
=SORT(UNIQUE(FILTER(Transactions!B:B, Transactions!B:B<>"")))

Unique cost centers:
=SORT(UNIQUE(FILTER(Transactions!D:D, Transactions!D:D<>"")))

Unique GL accounts with descriptions (two columns):
=SORT(UNIQUE(FILTER({Transactions!E:E, Transactions!F:F}, Transactions!E:E<>"")))

Vendor-Category mapping (derive relationships):
=UNIQUE({Transactions!B:B, Transactions!C:C})

Active vendors (appeared in last 12 months):
=SORT(UNIQUE(FILTER(Transactions!B:B, Transactions!A:A>TODAY()-365)))

Complete reference table build:
Combine UNIQUE extracts into structured reference sheets
```

**Business Application:** Many systems export flat transaction data without normalized reference tables. UNIQUE enables reverse-engineering of reference data from transactions, creating validation lists and reporting hierarchies that the source system lacks.

**Technical Details:** Multiple columns in UNIQUE extract relationship pairs (vendor-category). Date filters identify active vs. historical entries. SORT ensures consistent ordering. These unique lists become sources for VLOOKUP and data validation.

## Platform Differences

UNIQUE is available in **both Google Sheets and Microsoft Excel 365**, making it one of the more portable advanced functions.

### Google Sheets
- **Availability:** All versions since early Sheets
- **Integration:** Works seamlessly with QUERY, FILTER, ARRAYFORMULA
- **exactly_once:** Fully supported third parameter
- **by_column:** Fully supported second parameter

### Microsoft Excel
- **Availability:** Excel 365 (dynamic arrays) and Excel 2021+
- **Not Available:** Excel 2019 and earlier versions
- **Parameter Compatibility:** Same parameters as Google Sheets
- **Spill Behavior:** Uses Excel's dynamic array spill feature

### Key Differences Summary

| Feature | Google Sheets | Microsoft Excel 365 |
|---------|---------------|---------------------|
| Availability | All versions | 365/2021+ only |
| by_column parameter | Yes | Yes |
| exactly_once parameter | Yes | Yes |
| Spill to multiple cells | Yes | Yes |
| Works with QUERY | Yes | No QUERY in Excel |
| Array @ operator | Not needed | Optional |

## Tips and Best Practices

1. **Combine with SORT for clean lists:** `SORT(UNIQUE(range))` is a common pattern for creating alphabetically ordered reference lists suitable for dropdowns and reports.

2. **Filter before UNIQUE for conditional uniqueness:** When you need unique values from a subset, FILTER first: `UNIQUE(FILTER(A:A, B:B="Active"))` is more efficient than filtering after.

3. **Use COUNTA to count unique values:** `COUNTA(UNIQUE(range))` returns the count of distinct values—equivalent to COUNTUNIQUE but works with the array.

4. **Handle empty cells explicitly:** UNIQUE includes empty cells if present in the input. Use `UNIQUE(FILTER(A:A, A:A<>""))` to exclude blanks.

5. **Understand multi-column behavior:** For multiple columns, ALL columns must match for rows to be considered duplicates. Include all relevant columns for proper deduplication.

6. **Use exactly_once for outlier detection:** The third parameter with TRUE finds values appearing only once—useful for identifying data entry errors or special cases.

7. **Consider case sensitivity:** UNIQUE is case-insensitive for letters. "Apple" and "APPLE" are considered the same. Use EXACT for case-sensitive deduplication.

8. **Limit range size for performance:** Full column references (A:A) work but can be slow with UNIQUE. Use bounded ranges (A2:A1000) when possible.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COUNTUNIQUE]] | Counts unique values | When you only need the count, not the list |
| [[DISTINCT]] (SQL) | Similar concept in QUERY | Within QUERY statements |
| [[FILTER]] | Filters based on conditions | For conditional selection before UNIQUE |
| [[SORT]] | Orders values | Often combined with UNIQUE for sorted lists |

### Commonly Used Together

**[[SORT]]** - Order unique results

*Create sorted unique list:*
```
=SORT(UNIQUE(A:A))
```
Standard pattern for clean, alphabetized reference lists.

---

**[[FILTER]]** - Conditional uniqueness

*Unique values from filtered subset:*
```
=UNIQUE(FILTER(A:A, B:B="Category1"))
```
Get unique values only from rows matching criteria.

---

**[[COUNTA]]** - Count unique values

*Count distinct values:*
```
=COUNTA(UNIQUE(A:A))
```
Returns the number of unique values.

---

**[[FLATTEN]]** - Combine columns before uniqueness

*Unique across multiple columns:*
```
=UNIQUE(FLATTEN(A:C))
```
Gets all unique values from multiple columns combined.

---

**[[COUNTIF]]** - Find values with duplicates

*Identify repeated values:*
```
=UNIQUE(FILTER(A:A, COUNTIF(A:A, A:A)>1))
```
Returns values that have duplicates.

---

**[[QUERY]]** - SQL-style operations

*Group and count unique values:*
```
=QUERY(A:A, "SELECT A, COUNT(A) GROUP BY A")
```
Provides frequency counts alongside unique values.

## Official Documentation

- **Google Sheets:** [UNIQUE function](https://support.google.com/docs/answer/3093198)
- **Microsoft Excel:** [UNIQUE function](https://support.microsoft.com/en-us/office/unique-function-c5ab87fd-30a3-4ce9-9d1a-40204fb85e1e)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | ~2010 | Original implementation |
| Google Sheets | 2015 | Added by_column and exactly_once parameters |
| Google Sheets | 2020 | Performance improvements |
| Microsoft Excel | 2018 (365) | Introduced as dynamic array function |
| Microsoft Excel | 2021 | Included in perpetual license version |

---

*Last updated: 2026-01-10*
