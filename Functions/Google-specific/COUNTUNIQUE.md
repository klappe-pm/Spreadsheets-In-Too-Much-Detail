---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - counting
  - aggregation
subTopics:
  - distinct-count
  - unique-counting
  - data-analysis
  - cardinality
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - count-unique
  - count-distinct
  - distinct-count
  - unique-count
tags:
  - google-sheets-only
  - counting-functions
  - aggregation
  - data-analysis
  - distinct-values
---

# COUNTUNIQUE

## Description

The **COUNTUNIQUE function** is a Google Sheets-exclusive function that counts the number of unique values in a specified range or across multiple ranges, returning a single number representing the count of distinct entries. This aggregation function is essential for data analysis tasks such as determining how many different customers placed orders, counting unique products in inventory, identifying the number of distinct categories in a dataset, and any scenario requiring a count of non-repeating values rather than total occurrences.

The function examines all values in the specified ranges and counts each unique value exactly once, regardless of how many times it appears. For example, if a column contains "Apple", "Banana", "Apple", "Cherry", "Banana", "Apple", COUNTUNIQUE returns 3 because there are only three distinct values. The function treats different data types separately—the number 1 and the text "1" are counted as distinct values. Empty cells are typically not counted, though this behavior can vary based on context.

COUNTUNIQUE accepts multiple ranges as arguments, enabling counting of unique values across non-contiguous areas or combining multiple columns into a single unique count. This is more powerful than simply applying UNIQUE and then COUNTA, as it handles multiple ranges directly. The function also works with values returned by other functions, enabling complex calculations like counting unique values that meet certain criteria when combined with FILTER.

COUNTUNIQUE is exclusively available in Google Sheets. Microsoft Excel does not have a direct equivalent single function. Excel users must use workarounds such as SUMPRODUCT with COUNTIF (`=SUMPRODUCT(1/COUNTIF(range,range))`), combining UNIQUE with COUNTA in Excel 365, or using pivot tables. The simplicity of COUNTUNIQUE makes it one of Google Sheets' most practical exclusive functions for data analysis.

## Syntax

> [!f(x)] COUNTUNIQUE Syntax
>
> ```
> =COUNTUNIQUE(value1, [value2, ...])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | The first value, cell reference, or range to include in the unique count. Can be a single cell (A1), a range (A1:A100), a value, or an expression returning values. |
| `value2, ...` | No | Additional values, cell references, or ranges to include in the unique count. Multiple ranges are treated as a combined pool of values for uniqueness determination. Up to 30 arguments supported. |

### Return Value

Returns a single number representing the count of unique (distinct) values across all provided ranges. Empty cells are generally not counted. Error values in the input may cause errors in the output depending on context.

## Examples

> [!f(x)] COUNTUNIQUE Examples

### Example 1: Basic Column Count
```
=COUNTUNIQUE(A1:A10)
```
**Scenario:** A1:A10 contains: Apple, Banana, Apple, Cherry, Banana, Date, Apple, Cherry, Date, Date
**Result:** 4

**Explanation:** There are 4 distinct values (Apple, Banana, Cherry, Date) regardless of how many times each appears.

---

### Example 2: Count Unique with Empty Cells
```
=COUNTUNIQUE(A1:A10)
```
**Scenario:** A1:A10 contains: Apple, (empty), Banana, (empty), Apple, (empty)
**Result:** 2

**Explanation:** Empty cells are not counted as values. Only "Apple" and "Banana" are counted as unique.

---

### Example 3: Multiple Ranges
```
=COUNTUNIQUE(A1:A10, B1:B10, C1:C10)
```
**Scenario:** Count unique values across three columns
**Result:** Count of distinct values appearing in any of the three columns

**Explanation:** All three ranges are combined, and values are counted once each across the entire set.

---

### Example 4: Count Unique Numbers
```
=COUNTUNIQUE(1, 2, 3, 1, 2, 1)
```
**Scenario:** Direct values as arguments
**Result:** 3

**Explanation:** Only 1, 2, and 3 are unique; repetitions don't increase the count.

---

### Example 5: With FILTER for Conditional Count
```
=COUNTUNIQUE(FILTER(A:A, B:B="Active"))
```
**Scenario:** Count unique values in column A, but only for rows where column B is "Active"
**Result:** Count of distinct values from the filtered subset

**Explanation:** FILTER returns only qualifying values, then COUNTUNIQUE counts distinct among those.

---

### Example 6: Count Unique Customers
```
=COUNTUNIQUE(Orders!B:B)
```
**Scenario:** Column B contains customer names/IDs from an orders table
**Result:** Total number of distinct customers

**Explanation:** Even if a customer has 50 orders, they're counted once. This gives total unique customer count.

---

### Example 7: Compare to Total Count
```
="Unique: " & COUNTUNIQUE(A:A) & " of " & COUNTA(A:A) & " total"
```
**Scenario:** Show unique count vs. total count
**Result:** "Unique: 45 of 100 total"

**Explanation:** Comparing COUNTUNIQUE to COUNTA shows the ratio of unique to total—useful for duplication analysis.

---

### Example 8: Count Unique Across Sheets
```
=COUNTUNIQUE(Sheet1!A:A, Sheet2!A:A, Sheet3!A:A)
```
**Scenario:** Count unique values across the same column in three sheets
**Result:** Distinct values appearing in any of the three sheets

**Explanation:** COUNTUNIQUE works across sheet references, enabling cross-sheet deduplication counting.

---

### Example 9: With FLATTEN for Multi-Column
```
=COUNTUNIQUE(FLATTEN(A:C))
```
**Scenario:** Count unique values across multiple columns treated as one pool
**Result:** Distinct values from all three columns combined

**Explanation:** FLATTEN converts the multi-column range to a single column, then COUNTUNIQUE counts unique values from that combined set.

---

### Example 10: Count Unique by Criteria (Array)
```
=COUNTUNIQUE(IF(B:B="Sales", A:A, ""))
```
**Scenario:** Count unique values in A where B="Sales" (legacy array approach)
**Result:** Count of unique A values for Sales rows

**Explanation:** This array formula approach works but `COUNTUNIQUE(FILTER(...))` is preferred for clarity.

---

### Example 11: Year-Over-Year Unique Comparison
```
=COUNTUNIQUE(FILTER(A:A, YEAR(B:B)=2024))
```
**Scenario:** Count unique customers (column A) for orders in 2024 (column B has dates)
**Result:** Number of distinct customers in 2024

**Explanation:** Filter by year, then count unique—useful for year-over-year analysis.

---

### Example 12: Data Quality Check
```
=IF(COUNTUNIQUE(A:A)=COUNTA(A:A), "All Unique", "Duplicates Exist")
```
**Scenario:** Check if a column has any duplicate values
**Result:** "All Unique" or "Duplicates Exist"

**Explanation:** When unique count equals total count, no duplicates exist. This is a quick data quality check.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid argument type | Ensure arguments are valid ranges, values, or expressions |
| `#REF!` | Referenced range was deleted | Restore range or update formula reference |
| `Count includes blanks` | Blank cells being counted in some contexts | Use FILTER to exclude blanks: COUNTUNIQUE(FILTER(A:A, A:A<>"")) |
| `Higher count than expected` | Different data types counted separately | "1" (text) and 1 (number) are different; standardize data types |
| `Lower count than expected` | Case-insensitive comparison | "Apple" and "APPLE" are the same; counts as one unique |
| `Performance issues` | Very large ranges | Limit range size; avoid full column references when possible |
| `Error values in result` | Errors in source range | Filter out errors or use IFERROR in source data |
| `Unexpected result with dates` | Date formatting inconsistencies | Ensure dates are actual date values, not text |

## Use Cases

### [[Customer Analytics and Retention]]

**Scenario:** An e-commerce company needs to analyze customer behavior: total unique customers, unique customers per month, repeat customer rate, and customer acquisition trends. These metrics drive marketing and retention strategies.

**Implementation:**
```
Customer analytics structure:
Orders table: A (OrderID), B (Date), C (CustomerID), D (Amount)

Total unique customers all-time:
=COUNTUNIQUE(Orders!C:C)

Unique customers this month:
=COUNTUNIQUE(FILTER(Orders!C:C, MONTH(Orders!B:B)=MONTH(TODAY()), YEAR(Orders!B:B)=YEAR(TODAY())))

Unique customers by specific month (dynamic):
=COUNTUNIQUE(FILTER(Orders!C:C, EOMONTH(Orders!B:B, 0)=SelectedMonth))

New vs returning customers:
New customers = customers appearing only once historically
=COUNTUNIQUE(FILTER(Orders!C:C, COUNTIF(Orders!C:C, Orders!C:C)=1))

Unique customers per product category:
=COUNTUNIQUE(FILTER(Orders!C:C, Orders!E:E="Electronics"))

Monthly unique customer trend (for each month):
Create table with months, each cell:
=COUNTUNIQUE(FILTER(Orders!C:C, EOMONTH(Orders!B:B,0)=$A2))
```

**Business Application:** Customer count metrics are fundamental to business health assessment. Total unique customers shows market reach, monthly unique tracks engagement, and comparing to order count reveals purchase frequency. These metrics inform marketing spend, inventory planning, and growth projections.

**Technical Details:** FILTER by date components enables time-based unique counting. Comparing COUNTUNIQUE to total orders gives orders-per-customer ratio. For cohort analysis, filter by customer first-order date range.

---

### [[Product and Inventory Analysis]]

**Scenario:** A retail inventory system needs to track: how many unique SKUs are in stock, unique products per category, unique suppliers, and identify categories with limited product variety for expansion opportunities.

**Implementation:**
```
Inventory analysis structure:
Inventory: A (SKU), B (ProductName), C (Category), D (Supplier), E (Quantity)

Total unique SKUs:
=COUNTUNIQUE(Inventory!A:A)

Unique SKUs in stock (quantity > 0):
=COUNTUNIQUE(FILTER(Inventory!A:A, Inventory!E:E>0))

Unique products per category:
=QUERY(Inventory!A:C, "SELECT C, COUNT(A) GROUP BY C LABEL COUNT(A) 'Unique Products'")

Alternative per-category count (for specific category):
=COUNTUNIQUE(FILTER(Inventory!A:A, Inventory!C:C="Electronics"))

Unique suppliers:
=COUNTUNIQUE(Inventory!D:D)

Suppliers per category:
=COUNTUNIQUE(FILTER(Inventory!D:D, Inventory!C:C="Electronics"))

Categories with <10 unique products (expansion opportunities):
=FILTER(CategoryList, COUNTUNIQUE(FILTER(Inventory!A:A, Inventory!C:C=CategoryList))<10)
```

**Business Application:** Inventory diversity metrics guide purchasing and merchandising decisions. Categories with few unique products may need expansion. Supplier concentration (few unique suppliers) indicates supply chain risk. SKU counts help with warehouse planning.

**Technical Details:** QUERY provides grouped unique counts efficiently. For threshold-based analysis, combine COUNTUNIQUE in FILTER conditions. Consider caching expensive COUNTUNIQUE calculations in helper columns for complex dashboards.

---

### [[Survey Response Analysis]]

**Scenario:** A research team collects survey responses with multiple-choice and open-ended questions. Analysis requires counting unique respondents, unique response options selected, and identifying questions with high vs. low response variety.

**Implementation:**
```
Survey analysis structure:
Responses: A (ResponseID), B (RespondentID), C-Z (Question responses)

Unique respondents:
=COUNTUNIQUE(Responses!B:B)

Response rate (unique respondents / invites sent):
=COUNTUNIQUE(Responses!B:B) / InviteCount

Unique answers per question:
Question 1 variety: =COUNTUNIQUE(Responses!C:C)
Question 2 variety: =COUNTUNIQUE(Responses!D:D)

High-variety questions (many unique responses - likely open-ended):
Questions where COUNTUNIQUE > 20 indicate free-text responses

Low-variety questions (few unique responses):
Questions where COUNTUNIQUE < 5 indicate limited options or consensus

Multi-select questions (responses in single cell, comma-separated):
=COUNTUNIQUE(FLATTEN(SPLIT(TEXTJOIN(",", TRUE, Responses!C:C), ",")))

Respondents who answered all questions:
=COUNTUNIQUE(FILTER(Responses!B:B, AND(Responses!C:C<>"", Responses!D:D<>"")))
```

**Business Application:** Survey analysis requires understanding both response volume and variety. COUNTUNIQUE on respondent IDs gives participation rate. COUNTUNIQUE on answers shows response diversity—high diversity suggests open-ended questions or disagreement; low diversity indicates consensus or limited options.

**Technical Details:** For multi-select responses stored as comma-separated values, split and flatten before counting unique. Compare unique response count to total possible options to assess distribution. Use conditional COUNTUNIQUE for segment analysis (e.g., unique responses from specific demographics).

---

### [[Data Quality Monitoring]]

**Scenario:** A data engineering team needs to monitor data quality by tracking: unique primary keys (should equal row count), unique values per column for cardinality analysis, and identifying columns with unexpectedly low or high uniqueness.

**Implementation:**
```
Data quality monitoring:
Data table: A-Z columns with various data

Primary key uniqueness check:
=IF(COUNTUNIQUE(A:A)=COUNTA(A:A), "PK Valid", "Duplicates Found")

Duplicate count:
=COUNTA(A:A) - COUNTUNIQUE(A:A)

Cardinality per column (create for each column):
Column A: =COUNTUNIQUE(A:A)
Column B: =COUNTUNIQUE(B:B)
...

Cardinality ratio (unique/total):
=COUNTUNIQUE(A:A) / COUNTA(A:A)

High cardinality columns (likely IDs):
Columns where COUNTUNIQUE = COUNTA

Low cardinality columns (likely categories):
Columns where COUNTUNIQUE < 20

Expected vs actual unique count:
=IF(COUNTUNIQUE(A:A) >= ExpectedMin, "OK", "Below expected unique count")

Identify actual duplicates:
=UNIQUE(FILTER(A:A, COUNTIF(A:A, A:A)>1))
```

**Business Application:** Data quality depends on understanding uniqueness. Primary keys must be fully unique. Columns with unexpected cardinality (high or low) may indicate data issues. Regular monitoring catches problems before they affect downstream systems.

**Technical Details:** Cardinality ratio (unique/total) classifies column types: ~1.0 = identifier, ~0 = constant, between = category/attribute. Set up automated alerts when COUNTUNIQUE deviates from expected values. Create data quality dashboards with COUNTUNIQUE metrics per table/column.

## Platform Differences

COUNTUNIQUE is **exclusively available in Google Sheets** with no single-function equivalent in Microsoft Excel.

### Google Sheets
- **Availability:** All versions since introduction
- **Multiple Ranges:** Supports multiple range arguments
- **Integration:** Works with FILTER, FLATTEN, and other array functions
- **Simplicity:** Single function for distinct counting

### Microsoft Excel
- **Native COUNTUNIQUE:** Does not exist
- **Alternatives:**
  - **SUMPRODUCT method:** `=SUMPRODUCT(1/COUNTIF(range, range))` (has issues with blanks/errors)
  - **UNIQUE + COUNTA (365):** `=COUNTA(UNIQUE(range))`
  - **Pivot Tables:** Use Count Distinct in data model
  - **Power Query:** Group By with distinct count
- **Complexity:** All alternatives require workarounds or multiple functions

### Key Differences Summary

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| Single Function | Yes (COUNTUNIQUE) | No |
| Multiple Range Support | Built-in | Requires UNION or combining |
| Handles Blanks Cleanly | Yes | SUMPRODUCT method has issues |
| Learning Curve | Simple | Moderate to Complex |
| Performance | Good | Varies by method |

### Excel Equivalent Formulas

```
Google Sheets:
=COUNTUNIQUE(A1:A100)

Excel 365:
=COUNTA(UNIQUE(A1:A100))

Excel (older):
=SUMPRODUCT((A1:A100<>"")/COUNTIF(A1:A100, A1:A100&""))

Google Sheets with filter:
=COUNTUNIQUE(FILTER(A:A, B:B="Yes"))

Excel 365:
=COUNTA(UNIQUE(FILTER(A:A, B:B="Yes")))
```

## Tips and Best Practices

1. **Use FILTER for conditional unique counts:** `COUNTUNIQUE(FILTER(A:A, B:B="Criteria"))` is the cleanest way to count unique values meeting conditions.

2. **Combine with FLATTEN for multi-column counting:** When counting unique values across multiple columns as a single pool, use `COUNTUNIQUE(FLATTEN(A:C))`.

3. **Compare to COUNTA for duplication analysis:** If `COUNTUNIQUE(A:A) < COUNTA(A:A)`, duplicates exist. The difference is the number of duplicate entries.

4. **Be aware of data type sensitivity:** Text "1" and number 1 count as different values. Standardize data types before analysis for accurate counts.

5. **Case insensitivity:** "Apple" and "APPLE" are considered the same value. This is usually desired but be aware if case matters.

6. **Handle empty cells appropriately:** COUNTUNIQUE typically doesn't count empty cells, but verify with your specific use case. Use FILTER to explicitly exclude blanks if needed.

7. **Consider performance with large ranges:** Avoid full column references (A:A) when possible. Use bounded ranges (A2:A10000) for better performance.

8. **Use for data quality checks:** Regular COUNTUNIQUE monitoring on key columns can catch data issues early (unexpected duplicates or missing uniqueness).

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[UNIQUE]] | Returns array of unique values | When you need the actual values, not just count |
| [[COUNTA]] | Counts non-empty cells | When counting all occurrences, not unique |
| [[COUNT]] | Counts numeric values | For numeric-only counting |
| [[COUNTIF]] | Counts cells matching criteria | For counting specific value occurrences |

### Commonly Used Together

**[[FILTER]]** - Conditional unique counting

*Count unique values meeting criteria:*
```
=COUNTUNIQUE(FILTER(A:A, B:B="Active"))
```
The primary pattern for conditional unique counts.

---

**[[FLATTEN]]** - Multi-column unique counting

*Count unique across multiple columns:*
```
=COUNTUNIQUE(FLATTEN(A:C))
```
Combines columns before counting unique.

---

**[[COUNTA]]** - Compare for duplicate detection

*Check for duplicates:*
```
=IF(COUNTUNIQUE(A:A)=COUNTA(A:A), "No dupes", "Has dupes")
```
Comparing unique count to total reveals duplicates.

---

**[[UNIQUE]]** - Get the values, not just count

*List unique values alongside count:*
```
Values: =UNIQUE(A:A)
Count: =COUNTUNIQUE(A:A)
```
UNIQUE for list, COUNTUNIQUE for number.

---

**[[QUERY]]** - Group-wise unique counting

*Unique count per category:*
```
=QUERY(A:B, "SELECT A, COUNT(B) GROUP BY A")
```
QUERY's COUNT on groups provides per-category unique counts.

## Official Documentation

- **Google Sheets:** [COUNTUNIQUE function](https://support.google.com/docs/answer/3093405)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | ~2010 | Original implementation |
| Google Sheets | 2015 | Multiple range support enhanced |
| Google Sheets | 2018 | Performance improvements |
| Google Sheets | 2020 | Better integration with array functions |

---

*Last updated: 2026-01-10*
