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
- conditional-sum
- data-aggregation
- total-calculation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-sum
- conditional-database-sum
tags:
- database-functions
- summation
- criteria-based
- data-filtering
---

# DSUM

## Description

DSUM adds up all values in a database column among records that match specified criteria. As the foundational aggregation function in the database function family, DSUM enables you to calculate totals for filtered subsets of your data. Unlike simple SUM which totals all values in a range, DSUM first filters records using a criteria range, then sums only the values from matching records.

The **criteria range** is the defining feature that makes DSUM powerful. This separate area contains column headers that must exactly match your database headers. Below each header, you place filter conditions. When multiple criteria appear in the same row, they combine with AND logic (all conditions must be true). When criteria appear in different rows, they create OR logic (any condition can be true). This enables complex queries like "sum sales for (Electronics in January) OR (Home products in February) OR (any product in March)."

Choosing between DSUM and SUMIFS depends on your situation. DSUM excels when criteria change frequently (just update the criteria range, not the formula), when you need OR logic across multiple columns, or when maintaining legacy spreadsheets built on database functions. SUMIFS is more intuitive for straightforward AND conditions and does not require a separate criteria range. SUMIFS is also generally faster for very large datasets with simple criteria.

Database functions structure data with column headers in the first row and records below. The database range must include this header row. The field parameter identifies which column to sum, accepting a quoted header name, a column number (1 for the first column), or a cell reference. DSUM ignores text values and empty cells in the field column, summing only numeric values.

## Syntax

> [!NOTE] Syntax
> ```
> DSUM(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column to sum, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Basic Single Criterion Sum

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
=DSUM(A1:D6, "Quantity", F1:F2)
```

**Result:** `450`

**Explanation:** Sums Quantity for all Electronics products: Widget A (100) + Widget B (150) + Device Z (200) = 450 units.

---

### Example 2: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DSUM(A1:D6, 3, F1:F2)
```

**Result:** `100`

**Explanation:** Column 3 is Price. Sums prices for Home products: Gadget X ($45) + Gadget Y ($55) = $100.

---

### Example 3: Multiple AND Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Price |
|----------|-------|
| Electronics | >30 |

**Formula:**
```
=DSUM(A1:D6, "Quantity", F1:G2)
```

**Result:** `350`

**Explanation:** Sums Quantity for Electronics with Price > $30: Widget B (150) + Device Z (200) = 350 units.

---

### Example 4: OR Criteria (Multiple Rows)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F3):**
| Category |
|----------|
| Electronics |
| Home |

**Formula:**
```
=DSUM(A1:D6, "Quantity", F1:F3)
```

**Result:** `590`

**Explanation:** Sums Quantity for Electronics OR Home products (all products): 100 + 150 + 80 + 60 + 200 = 590 units.

---

### Example 5: Complex AND/OR Combination

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G3):**
| Category | Price |
|----------|-------|
| Electronics | <40 |
| Home | >50 |

**Formula:**
```
=DSUM(A1:D6, "Quantity", F1:G3)
```

**Result:** `310`

**Explanation:** Sums Quantity for (Electronics AND Price<40) OR (Home AND Price>50): Widget A (100) + Widget B (150) + Gadget Y (60) = 310 units.

---

### Example 6: Comparison Operators

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Quantity | Price |
|----------|-------|
| >=100 | <=50 |

**Formula:**
```
=DSUM(A1:D6, "Price", F1:G2)
```

**Result:** `60`

**Explanation:** Sums Price for products with Quantity >= 100 AND Price <= 50: Widget A ($25) + Widget B ($35) = $60.

---

### Example 7: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Widget* |

**Formula:**
```
=DSUM(A1:D6, "Quantity", F1:F2)
```

**Result:** `250`

**Explanation:** The asterisk matches any characters. Sums Quantity for products starting with "Widget": Widget A (100) + Widget B (150) = 250 units.

---

### Example 8: Sales Report by Region

**Database (A1:E7):**
| Order | Region | Product | Amount | Units |
|-------|--------|---------|--------|-------|
| 1001 | East | Widget | 5000 | 50 |
| 1002 | West | Gadget | 3500 | 35 |
| 1003 | East | Gadget | 4200 | 42 |
| 1004 | North | Widget | 6000 | 60 |
| 1005 | West | Widget | 4500 | 45 |
| 1006 | East | Widget | 5500 | 55 |

**Criteria (G1:H2):**
| Region | Product |
|--------|---------|
| East | Widget |

**Formula:**
```
=DSUM(A1:E7, "Amount", G1:H2)
```

**Result:** `10500`

**Explanation:** Sums Amount for East region Widget orders: Order 1001 ($5,000) + Order 1006 ($5,500) = $10,500.

---

### Example 9: Date-Based Summation

**Database (A1:E8):**
| Invoice | Customer | Date | Amount | Status |
|---------|----------|------|--------|--------|
| INV-001 | Acme | 2024-01-15 | 1500 | Paid |
| INV-002 | Beta | 2024-01-22 | 2200 | Paid |
| INV-003 | Acme | 2024-02-10 | 1800 | Pending |
| INV-004 | Gamma | 2024-02-18 | 3000 | Paid |
| INV-005 | Acme | 2024-03-05 | 2500 | Paid |
| INV-006 | Beta | 2024-03-12 | 1900 | Pending |
| INV-007 | Acme | 2024-03-20 | 2100 | Paid |

**Criteria (G1:H2):**
| Customer | Status |
|----------|--------|
| Acme | Paid |

**Formula:**
```
=DSUM(A1:E8, "Amount", G1:H2)
```

**Result:** `6100`

**Explanation:** Sums Amount for paid Acme invoices: INV-001 ($1,500) + INV-005 ($2,500) + INV-007 ($2,100) = $6,100.

---

### Example 10: Empty Criteria (All Records)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
|  |

**Formula:**
```
=DSUM(A1:D6, "Quantity", F1:F2)
```

**Result:** `590`

**Explanation:** Empty criteria cell matches all records. Sums all Quantity values: 100 + 150 + 80 + 60 + 200 = 590 (equivalent to SUM of the column).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Field parameter does not match any column header | Verify field name matches exactly including spelling and spacing |
| 0 (unexpected) | No records match criteria or field contains only text | Check criteria values; ensure field column has numeric data |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Amount" not Amount |
| #NUM! | Column number outside valid range | Use column number between 1 and total columns in database |
| Wrong result | Criteria headers do not match database headers exactly | Copy headers directly from database to criteria range |
| Wrong result | Criteria range includes extra rows | Ensure criteria range covers only header and criteria rows |
| #REF! | Invalid range reference | Verify database and criteria ranges point to existing cells |
| Unexpected sum | Blank criteria interpreted as "match all" | Remove blank rows from criteria range or use specific criteria |

## Use Cases

### Sales Reporting and Analysis

**Scenario:** A sales manager needs to generate flexible reports showing total sales by various dimensions including region, product category, salesperson, and time period.

**Implementation:** Create a sales database with Transaction ID, Date, Region, Salesperson, Category, Product, Amount, and Units columns. Set up a criteria range where users can specify any combination of filters.

**Technical Details:** Use named ranges for clarity: `=DSUM(SalesData, "Amount", SalesCriteria)`. Date ranges use operators like ">=2024-01-01" and "<2024-04-01" for Q1 totals. Multiple criteria rows enable "East OR West" regional comparisons. Users modify the criteria range to create different reports without changing formulas. Combine multiple DSUM formulas for a complete dashboard.

---

### Budget Tracking and Expense Analysis

**Scenario:** A finance team needs to track actual spending against budgeted amounts across different cost centers, expense categories, and time periods.

**Implementation:** Build an expense database with Date, Cost Center, Category, Vendor, Amount, and Budget Code columns. DSUM calculates actual spending for any filtered combination.

**Technical Details:** Create parallel DSUM formulas for actual vs budget: `=DSUM(Expenses, "Amount", criteria)` vs `=DSUM(Budget, "Planned", criteria)`. The variance is their difference. Use criteria rows for "Marketing OR Sales" to analyze combined departmental spending. Date criteria enable month-over-month or year-over-year comparisons.

---

### Inventory Valuation

**Scenario:** A warehouse manager needs to calculate total inventory value across different product categories, storage locations, and suppliers.

**Implementation:** Structure inventory data with SKU, Product, Category, Location, Supplier, Quantity, and Unit Cost columns. Add a calculated column for Total Value (Quantity * Unit Cost).

**Technical Details:** Formula `=DSUM(Inventory, "Total Value", criteria)` gives inventory value for filtered items. Use criteria like "Location = Warehouse A AND Category = Electronics" for specific valuations. Multiple rows enable "Supplier X OR Supplier Y" analysis. Results feed financial reports and insurance valuations.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Wildcard characters (*, ?) | Supported in text criteria | Supported in text criteria |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Ignores text values | Yes | Yes |
| Ignores empty cells | Yes | Yes |
| Structured table references | Supported (Table[Column]) | Not supported |
| Named ranges | Fully supported | Fully supported |
| Maximum rows | 1,048,576 | ~10,000,000 cells |
| Performance with large data | Generally faster | May slow with complex criteria |
| Date criteria handling | Accepts serial numbers or formatted dates | Prefers DATE() function |
| Result when no matches | Returns 0 | Returns 0 |

**Key Difference:** In Excel, you can use structured table references like `=DSUM(Table1, "Amount", CriteriaTable)` which automatically adjust as tables grow. Google Sheets requires traditional range references.

## Tips and Best Practices

1. **Copy headers exactly**: The most common DSUM error is mismatched headers between database and criteria range. Always copy headers directly from your database rather than retyping them.

2. **Use named ranges for readability**: Instead of `=DSUM(A1:G1000, "Amount", K1:L2)`, use `=DSUM(SalesData, "Amount", SalesCriteria)`. This makes formulas self-documenting and easier to maintain.

3. **Place criteria range strategically**: Position your criteria range away from your database (to the right or on a separate sheet) to prevent interference when sorting or filtering the main data.

4. **Leave cells truly empty for "match all"**: If you want a criteria column to accept any value, leave the cell completely empty. Spaces or formulas returning empty strings may not work correctly.

5. **Use SUMIFS for simple AND conditions**: When you only need straightforward AND logic, SUMIFS is faster and more intuitive. Reserve DSUM for OR conditions or frequently changing criteria.

6. **Validate with DCOUNT**: Before trusting DSUM results, use DCOUNT with the same criteria to verify the expected number of records are being summed. This catches criteria errors early.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[SUM]] | Sum values in a range | No filtering capability |
| [[SUMIF]] | Sum with single criterion | Limited to one condition |
| [[SUMIFS]] | Sum with multiple criteria | Inline criteria; AND logic only |
| [[DAVERAGE]] | Average matching criteria | Returns mean instead of total |
| [[DCOUNT]] | Count matching criteria | Counts records instead of summing |
| [[DMAX]] | Maximum matching criteria | Returns largest value |

### Commonly Used With

**[[DCOUNT]]** - Verify record count alongside sum:
```
Total: =DSUM(A1:E100, "Amount", criteria)
Records: =DCOUNT(A1:E100, "Amount", criteria)
Average: =DSUM(...) / DCOUNT(...)
```
Provides context for the summed values.

**[[DAVERAGE]]** - Compare sum to average:
```
Total: =DSUM(database, "Sales", criteria)
Average: =DAVERAGE(database, "Sales", criteria)
```
Shows both aggregate and central tendency.

**[[IFERROR]]** - Handle edge cases:
```
=IFERROR(DSUM(database, "Amount", criteria), 0)
```
Returns 0 if criteria or field reference is invalid.

## Official Documentation

- [Microsoft Excel DSUM Documentation](https://support.microsoft.com/en-us/office/dsum-function-53181285-0c4b-4f5a-aaa3-529a322be41b)
- [Google Sheets DSUM Documentation](https://support.google.com/docs/answer/3094151)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of original database functions |
| Excel 97 | Excel | Improved handling of date criteria |
| Excel 2007 | Excel | Row limit increased from 65,536 to 1,048,576 |
| Excel 2010 | Excel | Performance improvements for large datasets |
| Excel 2016 | Excel | Enhanced compatibility with structured tables |
| Excel 365 | Excel | Continued support alongside modern functions |
| Initial release | Google Sheets | Full compatibility with Excel syntax |
| 2019 | Google Sheets | Performance improvements for large datasets |
