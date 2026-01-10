---
categories:
- spreadsheet-functions
subCategories:
- sheets
topics:
- statistical
subTopics:
- counting
- unique-values
- distinct-count
- data-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Count Distinct
- Distinct Count
- Unique Count
- Count Unique Values
tags:
- statistical
- counting
- unique
- distinct
- google-sheets
---

# COUNTUNIQUE

## Description

**COUNTUNIQUE** counts the number of distinct (unique) values in one or more ranges, ignoring duplicate occurrences. If a value appears multiple times, it's counted only once. This function is essential for understanding data diversity - how many different categories, customers, products, or values exist in your dataset. Unlike COUNT (which counts all numeric entries) or COUNTA (which counts all non-empty entries), COUNTUNIQUE answers "how many different values are there?"

The function examines all provided values, compares them for equality, and returns the count of distinct entries. Text comparisons are case-sensitive in some contexts but case-insensitive in others (implementation varies). Blank cells are ignored by default - they don't contribute to the unique count. Numbers, text, dates, and logical values are all supported, and different types are considered distinct (the number 1 and the text "1" are counted as two unique values).

**Google Sheets exclusive:** COUNTUNIQUE is a Google Sheets function that does not exist natively in Excel. Excel users must use alternative approaches such as SUMPRODUCT with COUNTIF, or the newer UNIQUE function combined with COUNTA. This makes COUNTUNIQUE a significant convenience feature in Google Sheets for a common analytical need.

**Performance consideration:** COUNTUNIQUE must compare all values against each other to identify duplicates, which can be slow for very large datasets. For ranges with millions of cells, consider pivot tables or database queries instead. For typical spreadsheet sizes (thousands of rows), performance is generally acceptable.

## Syntax

> [!f(x)] COUNTUNIQUE Syntax
>
> ```
> =COUNTUNIQUE(value1, [value2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | The first value, cell reference, or range to count unique values from. Can be any data type. |
| `value2, ...` | No | Additional values, cell references, or ranges. All values are pooled together for unique counting. |

### Return Value

Returns a non-negative integer representing the number of distinct values across all provided arguments. Returns 0 if all cells are empty or no arguments contain values.

## Examples

> [!f(x)] COUNTUNIQUE Examples

### Example 1: Basic Unique Count
```
=COUNTUNIQUE(A1:A10)
```
Where A1:A10 contains: Apple, Orange, Apple, Banana, Orange, Apple, Grape, Banana, Apple, Grape

**Result:** 4

**Explanation:** Four unique values (Apple, Orange, Banana, Grape), regardless of how many times each appears.

---

### Example 2: All Unique Values
```
=COUNTUNIQUE(1, 2, 3, 4, 5)
```
**Result:** 5

**Explanation:** All values are distinct, so count equals total count.

---

### Example 3: All Duplicate Values
```
=COUNTUNIQUE(A1:A5)
```
Where A1:A5 contains: Red, Red, Red, Red, Red

**Result:** 1

**Explanation:** Only one unique value exists despite five entries.

---

### Example 4: Mixed Data Types
```
=COUNTUNIQUE(1, "1", TRUE, "TRUE")
```
**Result:** 4

**Explanation:** Number 1, text "1", boolean TRUE, and text "TRUE" are all distinct values - different types don't match.

---

### Example 5: With Empty Cells
```
=COUNTUNIQUE(A1:A10)
```
Where A1:A10 contains: Apple, [empty], Orange, [empty], Apple, [empty], Banana

**Result:** 3

**Explanation:** Empty cells are ignored. Three unique non-empty values: Apple, Orange, Banana.

---

### Example 6: Counting Unique Customers
```
=COUNTUNIQUE(CustomerID)
```
Where CustomerID is a named range of customer IDs from a transaction log

**Result:** Number of distinct customers

**Explanation:** Each customer counted once regardless of how many transactions they made.

---

### Example 7: Multiple Ranges Combined
```
=COUNTUNIQUE(A1:A100, B1:B100, C1:C100)
```
**Result:** Unique values across all three columns

**Explanation:** All values are pooled together before counting uniques. A value appearing in A and B is still counted only once.

---

### Example 8: Date Uniqueness
```
=COUNTUNIQUE(DateColumn)
```
Where DateColumn contains transaction dates

**Result:** Number of distinct transaction dates

**Explanation:** Useful for counting how many different days had activity, ignoring multiple transactions per day.

---

### Example 9: Case Sensitivity Test
```
=COUNTUNIQUE("apple", "Apple", "APPLE")
```
**Result:** 3 (in most implementations)

**Explanation:** Text comparisons are typically case-sensitive, so different capitalizations are counted as distinct values. Verify behavior in your specific version.

---

### Example 10: Numbers and Formatted Numbers
```
=COUNTUNIQUE(A1:A5)
```
Where A1:A5 contains: 100, 100.00, $100, 100.00, 100

**Result:** 1 or 2 (depending on formatting)

**Explanation:** If all cells contain the numeric value 100 (just formatted differently), count is 1. If "$100" is entered as text, it's counted separately.

---

### Example 11: Product Variety Analysis
```
=COUNTUNIQUE(ProductSKU)
```
**Result:** Number of distinct products sold

**Explanation:** From a sales log with thousands of rows, quickly determine how many different products were involved.

---

### Example 12: Conditional Unique Count (Workaround)
```
=COUNTUNIQUE(FILTER(A:A, B:B="Category1"))
```
**Result:** Unique values in column A where column B equals "Category1"

**Explanation:** Combine with FILTER to count uniques conditionally. COUNTUNIQUEIFS is the dedicated function for this in newer Google Sheets.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NAME?` in Excel | Function doesn't exist in Excel | Use SUMPRODUCT(1/COUNTIF(range,range)) or UNIQUE + COUNTA instead |
| `#VALUE!` | Invalid range reference | Check range syntax. Ensure proper cell references. |
| `0` when unexpected | All cells empty | Verify data exists in the specified range. |
| `Slow calculation` | Very large range | Consider pivot tables for large datasets. Limit range if possible. |
| `Case sensitivity issues` | Different capitalization treated differently | Standardize case with UPPER() or LOWER() before counting if needed. |
| `Unexpected high count` | Numbers stored as text | Verify data types. Convert text-numbers with VALUE() if needed. |

## Use Cases

### [[Customer Analytics: Unique Customer Count]]

**Scenario:** A marketing analyst determines how many distinct customers made purchases during a campaign period.

**Implementation:**
```
=COUNTUNIQUE(TransactionLog[CustomerID])
```

**Business Application:** Total transactions might be 10,000, but unique customers might be only 2,500 (indicating repeat purchases). This metric is crucial for understanding customer reach versus engagement. Customer acquisition cost calculations require unique customer counts, not transaction counts.

**Technical Details:** Ensure customer IDs are consistently formatted. Mixed formats (ID-001 vs 1 vs 001) create false uniqueness. Clean data first with TRIM() and consistent formatting.

---

### [[Inventory Management: SKU Diversity]]

**Scenario:** An inventory manager assesses product variety in stock or sales by counting distinct SKUs.

**Implementation:**
```
Total SKUs stocked: =COUNTUNIQUE(InventorySKU)
SKUs sold: =COUNTUNIQUE(FILTER(SalesSKU, SalesDate>=StartDate))
```

**Business Application:** Understanding product diversity helps with assortment planning. If 5,000 transactions involve only 50 unique SKUs while you stock 500, you have slow-moving inventory. High SKU diversity with low sales per SKU suggests overassortment.

**Technical Details:** Track unique counts over time to monitor assortment changes. Compare unique SKUs ordered versus unique SKUs received to catch supply issues.

---

### [[Data Quality Assessment]]

**Scenario:** A data analyst validates data by checking expected uniqueness - IDs should be unique, while category codes should have limited distinct values.

**Implementation:**
```
Should be unique: =IF(COUNTUNIQUE(IDs)=COUNTA(IDs), "Valid", "Duplicates exist")
Expected categories: =IF(COUNTUNIQUE(StatusCodes)<=5, "Normal", "Investigate new codes")
```

**Business Application:** Data quality checks prevent analytical errors. If order IDs should be unique but COUNTUNIQUE < COUNTA, duplicates exist requiring investigation. If categorical fields have unexpected new values, data entry errors or system changes may need attention.

**Technical Details:** Build automated data validation dashboards with COUNTUNIQUE checks. Flag anomalies for review rather than failing silently.

## Platform Differences

### Microsoft Excel

- **Availability:** NOT AVAILABLE natively
- **Workarounds:**
  - Excel 365/2021: `=COUNTA(UNIQUE(range))` or `=ROWS(UNIQUE(range))`
  - Older Excel: `=SUMPRODUCT(1/COUNTIF(range, range))` (excludes blanks)
  - Or: `=SUMPRODUCT((range<>"")/COUNTIF(range,range&""))`

### Google Sheets

- **Availability:** All versions since launch
- **Behavior:** Native function, counts distinct non-empty values
- **Case sensitivity:** Generally case-sensitive for text
- **Performance:** Good for typical spreadsheet sizes

### Key Difference Alert

COUNTUNIQUE is a **Google Sheets exclusive function**. Excel does not have a direct equivalent with this name.

**Excel alternatives:**
```
Excel 365+: =ROWS(UNIQUE(FILTER(range, range<>"")))
Older Excel: =SUMPRODUCT(1/COUNTIF(range, range))
```

The SUMPRODUCT workaround has limitations:
- Excludes blank cells automatically
- Can have issues with errors in range
- Slower than native COUNTUNIQUE

## Tips and Best Practices

1. **Understand case sensitivity.** Text comparisons may be case-sensitive. If "Apple" and "apple" should be the same, convert to consistent case first: =COUNTUNIQUE(ARRAYFORMULA(LOWER(range))).

2. **Handle empty cells intentionally.** Empty cells are ignored by default. If blanks should count as a unique value, you may need workarounds with IF statements.

3. **Verify data type consistency.** Numbers stored as text (due to leading zeros or import issues) are counted separately from actual numbers. Use ISNUMBER() to check.

4. **Combine with FILTER for conditional counts.** For unique counts with criteria, use =COUNTUNIQUE(FILTER(range, criteria)) or the newer COUNTUNIQUEIFS function.

5. **Monitor performance with large data.** For hundreds of thousands of rows, COUNTUNIQUE may slow recalculation. Consider pivot tables or periodic snapshots instead of live formulas.

6. **Use for data validation.** Check if ID columns are truly unique: =IF(COUNTUNIQUE(IDs)=COUNTA(IDs), "All unique", "Duplicates found").

7. **Document Excel alternatives.** When sharing spreadsheets between Sheets and Excel, document workarounds for COUNTUNIQUE so Excel users understand the logic.

8. **Combine with COUNT/COUNTA for ratios.** =COUNTA(range)/COUNTUNIQUE(range) gives average occurrences per unique value, useful for frequency analysis.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[UNIQUE]] | Returns array of unique values | When you need the actual values, not just count |
| [[COUNTA]] | Counts non-empty cells | When all values should be counted, duplicates included |
| [[COUNT]] | Counts numeric values | When counting only numbers, duplicates included |
| [[COUNTUNIQUEIFS]] | Conditional unique count | When counting unique values meeting criteria (Google Sheets) |
| [[DISTINCT]] (SQL) | Database distinct count | In database contexts |

### Commonly Used Together

**[[UNIQUE]]** - Get the actual unique values

*Count and list:*
```
How many: =COUNTUNIQUE(A:A)
What are they: =UNIQUE(A:A)
```
UNIQUE returns the array; COUNTUNIQUE counts it.

---

**[[FILTER]]** - Conditional unique count

*Unique values meeting criteria:*
```
=COUNTUNIQUE(FILTER(ProductID, Region="West"))
```
Counts unique products sold in the West region.

---

**[[COUNTA]]** - Compare for duplicates

*Detect duplicates:*
```
Total entries: =COUNTA(IDs)
Unique entries: =COUNTUNIQUE(IDs)
Duplicates exist: =COUNTA(IDs) > COUNTUNIQUE(IDs)
```

---

**[[QUERY]]** - Alternative approach

*Database-style unique count:*
```
=QUERY(data, "SELECT COUNT(A) LABEL COUNT(A) ''")
vs
=COUNTUNIQUE(A:A)
```
QUERY provides SQL-like syntax; COUNTUNIQUE is simpler for basic cases.

## Official Documentation

- **Google Sheets:** [COUNTUNIQUE function](https://support.google.com/docs/answer/3093405)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | Original launch (2006) | Available since inception |
| Google Sheets | Ongoing updates | COUNTUNIQUEIFS added later for conditional counting |
| Excel | Not available | Use UNIQUE + COUNTA in Excel 365, or SUMPRODUCT workaround |

---

*Last updated: 2026-01-10*
