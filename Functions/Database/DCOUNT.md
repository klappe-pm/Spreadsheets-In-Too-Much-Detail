---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- database
subTopics:
- criteria-range
- conditional-count
- numeric-counting
- data-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-count
- conditional-database-count
tags:
- database-functions
- counting
- criteria-based
- numeric-data
---

# DCOUNT

## Description

DCOUNT counts the cells containing numeric values in a database column that match specified criteria. This function is distinct from DCOUNTA in that it only counts cells with numbers, ignoring text, blanks, and errors. As part of the database function family, DCOUNT uses a separate criteria range to filter records before performing the count, enabling complex conditional counting scenarios.

The **criteria range** is the key concept that differentiates database functions from their simpler counterparts like COUNTIF. Your criteria range must contain column headers that exactly match the headers in your database. Below each header, you place the conditions that records must meet. Conditions in the same row are combined with AND logic (all must be true), while conditions in different rows create OR logic (any can be true). This structure allows you to express criteria like "count Electronics items priced over $50 OR any Home items" without nested formulas.

When choosing between DCOUNT and COUNTIFS, consider your specific needs. DCOUNT is advantageous when criteria change frequently since users can modify the criteria range without touching the formula. It handles complex OR conditions across multiple fields more elegantly than nested COUNTIFS. However, COUNTIFS is typically faster for simple AND-only conditions and requires no separate criteria range setup.

Database functions view your spreadsheet data as a traditional database table: column headers serve as field names, and each row below represents a record. The database range must include the header row plus all data rows. The field parameter identifies which column to count and can be specified as a quoted text string matching the header, a column number (1 for the first column), or a cell reference. Note that DCOUNT specifically counts numeric values; use DCOUNTA if you need to count all non-empty cells regardless of content type.

## Syntax

> [!NOTE] Syntax
> ```
> DCOUNT(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column to count, specified as a column header name in quotes, a column number (1-based), or a cell reference; can be omitted to count records matching criteria | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Basic Single Criterion Count

**Database (A1:D6):**
| Product | Category | Price | Quantity |
|---------|----------|-------|----------|
| Widget A | Electronics | 25 | 100 |
| Widget B | Electronics | 35 | 150 |
| Gadget X | Home | 45 | 80 |
| Gadget Y | Home | 55 | 60 |
| Device Z | Electronics | 65 | 200 |

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**Formula:**
```
=DCOUNT(A1:D6, "Price", F1:F2)
```

**Result:** `3`

**Explanation:** Counts how many numeric Price values exist for Electronics products. Three products (Widget A, Widget B, Device Z) match, and all have numeric prices.

---

### Example 2: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DCOUNT(A1:D6, 4, F1:F2)
```

**Result:** `2`

**Explanation:** Column 4 is Quantity. Counts numeric Quantity values for Home category products. Two Home products exist with numeric quantities.

---

### Example 3: Multiple AND Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Price |
|----------|-------|
| Electronics | >30 |

**Formula:**
```
=DCOUNT(A1:D6, "Quantity", F1:G2)
```

**Result:** `2`

**Explanation:** Counts Electronics products with Price > 30. Widget B (price 35) and Device Z (price 65) match. Both have numeric Quantity values, so the count is 2.

---

### Example 4: OR Criteria (Multiple Rows)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F3):**
| Price |
|-------|
| <30 |
| >60 |

**Formula:**
```
=DCOUNT(A1:D6, "Product", F1:F3)
```

**Result:** `0`

**Explanation:** Counts products where Price < 30 OR Price > 60. Widget A (25) and Device Z (65) match. However, the Product column contains text, not numbers, so DCOUNT returns 0. Use DCOUNTA for text values.

---

### Example 5: Counting with Numeric Field

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F3):**
| Price |
|-------|
| <30 |
| >60 |

**Formula:**
```
=DCOUNT(A1:D6, "Price", F1:F3)
```

**Result:** `2`

**Explanation:** Same criteria as Example 4, but now counting the numeric Price field. Both matching records (Widget A at $25 and Device Z at $65) have numeric prices.

---

### Example 6: Complex AND/OR Combination

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G3):**
| Category | Quantity |
|----------|----------|
| Electronics | >150 |
| Home | <70 |

**Formula:**
```
=DCOUNT(A1:D6, "Price", F1:G3)
```

**Result:** `2`

**Explanation:** Counts (Electronics AND Quantity>150) OR (Home AND Quantity<70). Device Z (Electronics, 200) and Gadget Y (Home, 60) match. Both have numeric prices.

---

### Example 7: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| *get* |

**Formula:**
```
=DCOUNT(A1:D6, "Price", F1:F2)
```

**Result:** `2`

**Explanation:** The asterisks match any characters before and after "get". Gadget X and Gadget Y match. Both have numeric Price values.

---

### Example 8: Counting Records with Missing Data

**Database (A1:D7):**
| Product | Category | Price | Quantity |
|---------|----------|-------|----------|
| Widget A | Electronics | 25 | 100 |
| Widget B | Electronics | N/A | 150 |
| Gadget X | Home | 45 | 80 |
| Gadget Y | Home | 55 |  |
| Device Z | Electronics | 65 | 200 |
| Item Q | Home | | 50 |

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DCOUNT(A1:D7, "Price", F1:F2)
```

**Result:** `2`

**Explanation:** Counts Home products with numeric Price values. Gadget X ($45) and Gadget Y ($55) have numbers. Item Q has a blank price (not counted). Only numeric cells are counted.

---

### Example 9: Employee Database Count

**Database (A1:E6):**
| Employee | Department | Region | Salary | Years |
|----------|------------|--------|--------|-------|
| Alice | Sales | North | 55000 | 3 |
| Bob | Marketing | South | 48000 | 5 |
| Carol | Sales | South | 62000 | 7 |
| David | IT | North | 75000 | 4 |
| Eve | Sales | North | 58000 | 2 |

**Criteria (G1:H2):**
| Department | Years |
|------------|-------|
| Sales | >=3 |

**Formula:**
```
=DCOUNT(A1:E6, "Salary", G1:H2)
```

**Result:** `2`

**Explanation:** Counts Sales employees with 3+ years tenure who have numeric salaries. Alice (3 years) and Carol (7 years) match.

---

### Example 10: Using Cell Reference for Field

**Database (A1:E6):** Same as Example 9

**Criteria (G1:G2):**
| Region |
|--------|
| North |

**Cell J1:** `Salary`

**Formula:**
```
=DCOUNT(A1:E6, J1, G1:G2)
```

**Result:** `3`

**Explanation:** The field is specified by cell reference J1 containing "Salary". Counts North region employees with numeric salaries: Alice, David, and Eve.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Field parameter does not match any column header | Verify field name matches exactly including spelling and spacing |
| 0 (unexpected) | Field column contains text values, not numbers | DCOUNT only counts numeric cells; use DCOUNTA for text or mixed data |
| 0 (unexpected) | No records match the criteria | Check criteria values against actual data; look for typos or case mismatches |
| #NAME? | Field name not in quotes | Enclose text field names in quotation marks: "Sales" |
| #NUM! | Column number outside valid range | Use column number between 1 and total columns in database |
| Incorrect count | Criteria headers do not match database headers exactly | Copy headers from database to criteria range |
| Incorrect count | Criteria range includes extra blank rows | Ensure criteria range only covers header plus criteria rows |
| #REF! | Invalid range reference | Verify database and criteria ranges point to existing cells |

## Use Cases

### Inventory Tracking

**Scenario:** A warehouse manager needs to count how many products in specific categories have stock levels recorded in the inventory system to identify data gaps and ensure accurate reporting.

**Implementation:** Create an inventory database with columns for SKU, Category, Location, Stock Level, and Last Counted date. The criteria range allows filtering by category, location, or date range. DCOUNT on the Stock Level column reveals how many items have numeric inventory counts recorded versus missing data.

**Technical Details:** Compare DCOUNT (numeric entries) with DCOUNTA (all non-empty entries) to identify cells containing text errors like "N/A" or "Pending". Use criteria rows for different warehouses to generate a data completeness report. This identifies products needing physical counts before month-end reporting.

---

### Survey Response Analysis

**Scenario:** A market research team needs to count valid numeric responses to survey questions across different demographic segments to determine statistical validity of findings.

**Implementation:** Build a survey database with respondent demographics (Age Group, Region, Income Bracket) and numeric response columns (Satisfaction Score, Likelihood to Recommend, etc.). The criteria range enables segmentation analysis.

**Technical Details:** DCOUNT helps identify sample sizes for each segment. For example, "How many respondents aged 25-34 in the Northeast provided a numeric satisfaction score?" This validates whether segment-level analysis has sufficient data. Use multiple criteria rows to count responses from "25-34 OR 35-44" age groups for combined analysis.

---

### Financial Transaction Auditing

**Scenario:** An audit team needs to count transactions with valid numeric amounts across different accounts, time periods, and transaction types to assess data quality and identify anomalies.

**Implementation:** Structure transaction data with Account Number, Date, Type, Amount, and Status columns. The criteria range allows auditors to specify any combination of filters without modifying formulas.

**Technical Details:** DCOUNT on the Amount column counts transactions with valid numeric values. Compare with total record counts (using DCOUNTA or ROWS) to identify transactions with missing or text amounts requiring investigation. Use date criteria like ">2024-01-01" combined with account criteria to focus on specific audit periods.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Wildcard characters (*, ?) | Supported in text criteria | Supported in text criteria |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Counting behavior | Counts numeric cells only | Counts numeric cells only |
| Structured table references | Supported (Table[Column]) | Not supported |
| Named ranges | Fully supported | Fully supported |
| Maximum rows | 1,048,576 | Limited by total cell count (10M) |
| Performance | Optimized for large datasets | May slow with complex criteria |
| Date handling in criteria | Accepts serial numbers or formatted dates | Requires DATE() or ISO format |
| Omitting field parameter | Returns count of matching records | Returns count of matching records |

**Key Difference:** Both platforms handle DCOUNT identically for numeric data. The main differences relate to table references (Excel-only feature) and date formatting in criteria ranges.

## Tips and Best Practices

1. **Understand DCOUNT vs DCOUNTA**: DCOUNT only counts cells containing numbers. If your target column has text data, DCOUNT returns 0 even when records match. Use DCOUNTA to count all non-empty cells regardless of content type.

2. **Validate criteria range size**: Your criteria range should include exactly the header row plus rows containing criteria values. Extra blank rows may be interpreted as "match all" conditions, producing unexpected counts.

3. **Use comparison operators correctly**: Place operators directly in the criteria cell with the value: ">50" not "> 50" (no space). Supported operators include =, <>, <, <=, >, and >=. Omitting an operator implies equals (=).

4. **Create reusable criteria templates**: Set up criteria ranges with data validation dropdowns for common filter values. Users can select criteria from lists rather than typing, reducing errors and improving consistency.

5. **Combine DCOUNT with DCOUNTA for data quality**: The formula `=DCOUNTA(...) - DCOUNT(...)` reveals how many matching records have non-numeric data in a field that should be numeric, helping identify data quality issues.

6. **Name your ranges for clarity**: Instead of `=DCOUNT(A1:E1000, "Amount", G1:H2)`, use named ranges: `=DCOUNT(Transactions, "Amount", TransactionCriteria)`. This makes formulas self-documenting and easier to maintain.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[DCOUNTA]] | Counts non-empty cells matching criteria | Counts all non-empty cells including text; DCOUNT counts only numbers |
| [[COUNTIF]] | Counts cells matching a single criterion | Simpler syntax but limited to one condition |
| [[COUNTIFS]] | Counts cells matching multiple criteria | Criteria inline in formula; AND logic only |
| [[COUNT]] | Counts numeric cells in a range | No filtering capability |
| [[DSUM]] | Sums values matching criteria | Aggregates values instead of counting |
| [[DGET]] | Retrieves single value matching criteria | Returns a value, not a count |

### Commonly Used With

**[[DCOUNTA]]** - Compare total records vs numeric records:
```
Numeric: =DCOUNT(A1:E100, "Amount", criteria)
Total: =DCOUNTA(A1:E100, "Amount", criteria)
Missing Numbers: =DCOUNTA(...) - DCOUNT(...)
```
Identifies records with non-numeric data in numeric fields.

**[[DAVERAGE]]** - Combine count with average for context:
```
Average: =DAVERAGE(A1:E100, "Sales", criteria)
Sample Size: =DCOUNT(A1:E100, "Sales", criteria)
```
Provides statistical context for averages.

**[[IFERROR]]** - Handle edge cases gracefully:
```
=IFERROR(DCOUNT(database, "Field", criteria), 0)
```
Returns 0 if criteria range or field is invalid.

## Official Documentation

- [Microsoft Excel DCOUNT Documentation](https://support.microsoft.com/en-us/office/dcount-function-c1fc7b93-fb0d-4d8d-97db-8d5f076eaeb1)
- [Google Sheets DCOUNT Documentation](https://support.google.com/docs/answer/3094222)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release with database functions |
| Excel 97 | Excel | Enhanced criteria handling |
| Excel 2007 | Excel | Expanded row limits to 1,048,576 |
| Excel 2010 | Excel | Performance optimizations |
| Excel 365 | Excel | Compatibility with dynamic arrays |
| Initial release | Google Sheets | Full Excel compatibility |
| 2018 | Google Sheets | Improved performance with large datasets |
