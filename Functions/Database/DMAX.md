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
- conditional-maximum
- data-analysis
- peak-values
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-max
- conditional-database-maximum
tags:
- database-functions
- maximum
- criteria-based
- data-filtering
---

# DMAX

## Description

DMAX returns the largest value in a database column among records that match specified criteria. Unlike the simple MAX function which finds the maximum across an entire range, DMAX allows you to filter records first using a criteria range, then find the maximum only within that filtered subset. This makes it ideal for answering questions like "What was the highest sale in the Electronics category?" or "What is the maximum salary among employees hired after 2020?"

The **criteria range** is what enables DMAX to filter data before finding the maximum. This separate area of your spreadsheet contains column headers that must exactly match those in your database. Below each header, you place the conditions records must meet. When criteria appear in the same row, they create AND logic (all must be true). When criteria appear in different rows, they create OR logic (any can be true). This powerful structure lets you find the maximum for complex scenarios like "highest price among (Electronics with stock > 100) OR (Home products)."

Choosing between DMAX and MAXIFS depends on your needs. DMAX excels when criteria change frequently (just update the criteria range without editing formulas), when you need OR logic across multiple columns, or when working with legacy spreadsheets using database functions. MAXIFS is more direct for simple AND-only conditions and does not require a separate criteria range. Both functions ignore text and empty cells when determining the maximum.

Database functions treat your data as a structured table with column headers in the first row and data records below. The database range must include these headers. The field parameter identifies which column to search for the maximum and can be specified as a quoted column header name, a column number (1 for the first column), or a cell reference. DMAX only considers numeric values; text values in the field column are ignored when finding the maximum.

## Syntax

> [!NOTE] Syntax
> ```
> DMAX(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column to search for the maximum value, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Basic Single Criterion Maximum

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
=DMAX(A1:D6, "Price", F1:F2)
```

**Result:** `65`

**Explanation:** Finds the maximum Price among Electronics products. Device Z at $65 is the highest-priced Electronics item.

---

### Example 2: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DMAX(A1:D6, 4, F1:F2)
```

**Result:** `80`

**Explanation:** Column 4 is Quantity. Finds the maximum quantity among Home products. Gadget X has 80 units, which is higher than Gadget Y's 60.

---

### Example 3: Multiple AND Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Price |
|----------|-------|
| Electronics | >30 |

**Formula:**
```
=DMAX(A1:D6, "Quantity", F1:G2)
```

**Result:** `200`

**Explanation:** Finds maximum Quantity for Electronics priced over $30. Widget B (150) and Device Z (200) qualify. Maximum is 200.

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
=DMAX(A1:D6, "Price", F1:F3)
```

**Result:** `65`

**Explanation:** Finds maximum Price across Electronics OR Home products. All products match, and Device Z at $65 is the highest.

---

### Example 5: Complex AND/OR Combination

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G3):**
| Category | Quantity |
|----------|----------|
| Electronics | >100 |
| Home | <70 |

**Formula:**
```
=DMAX(A1:D6, "Price", F1:G3)
```

**Result:** `65`

**Explanation:** Finds maximum Price for (Electronics AND Quantity>100) OR (Home AND Quantity<70). Matches: Widget B ($35), Device Z ($65), Gadget Y ($55). Maximum is $65.

---

### Example 6: Comparison Operators

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Price | Quantity |
|-------|----------|
| >=40 | <=100 |

**Formula:**
```
=DMAX(A1:D6, "Price", F1:G2)
```

**Result:** `55`

**Explanation:** Finds maximum Price where Price >= 40 AND Quantity <= 100. Gadget X ($45, 80 units) and Gadget Y ($55, 60 units) match. Maximum is $55.

---

### Example 7: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Widget* |

**Formula:**
```
=DMAX(A1:D6, "Price", F1:F2)
```

**Result:** `35`

**Explanation:** The asterisk matches any characters. Products starting with "Widget" are Widget A ($25) and Widget B ($35). Maximum is $35.

---

### Example 8: Employee Salary Analysis

**Database (A1:E6):**
| Employee | Department | Region | Salary | Years |
|----------|------------|--------|--------|-------|
| Alice | Sales | North | 55000 | 3 |
| Bob | Marketing | South | 48000 | 5 |
| Carol | Sales | South | 62000 | 7 |
| David | IT | North | 75000 | 4 |
| Eve | Sales | North | 58000 | 2 |

**Criteria (G1:H2):**
| Department | Region |
|------------|--------|
| Sales | North |

**Formula:**
```
=DMAX(A1:E6, "Salary", G1:H2)
```

**Result:** `58000`

**Explanation:** Finds maximum Salary among Sales employees in North region. Alice ($55,000) and Eve ($58,000) qualify. Maximum is $58,000.

---

### Example 9: Date-Based Criteria

**Database (A1:E6):**
| Order | Customer | Date | Amount | Status |
|-------|----------|------|--------|--------|
| 1001 | Acme | 2024-01-15 | 5000 | Complete |
| 1002 | Beta | 2024-02-20 | 7500 | Complete |
| 1003 | Acme | 2024-03-10 | 3000 | Pending |
| 1004 | Gamma | 2024-01-25 | 8500 | Complete |
| 1005 | Acme | 2024-04-05 | 6000 | Complete |

**Criteria (G1:H2):**
| Customer | Status |
|----------|--------|
| Acme | Complete |

**Formula:**
```
=DMAX(A1:E6, "Amount", G1:H2)
```

**Result:** `6000`

**Explanation:** Finds maximum Amount for completed Acme orders. Orders 1001 ($5,000) and 1005 ($6,000) qualify. Maximum is $6,000.

---

### Example 10: Finding Peak Performance

**Database (A1:E8):**
| Salesperson | Quarter | Year | Revenue | Deals |
|-------------|---------|------|---------|-------|
| Alice | Q1 | 2024 | 125000 | 15 |
| Alice | Q2 | 2024 | 145000 | 18 |
| Bob | Q1 | 2024 | 98000 | 12 |
| Bob | Q2 | 2024 | 110000 | 14 |
| Carol | Q1 | 2024 | 135000 | 16 |
| Carol | Q2 | 2024 | 155000 | 19 |
| Alice | Q3 | 2024 | 160000 | 20 |

**Criteria (G1:G2):**
| Salesperson |
|-------------|
| Alice |

**Formula:**
```
=DMAX(A1:E8, "Revenue", G1:G2)
```

**Result:** `160000`

**Explanation:** Finds Alice's maximum quarterly revenue. Q3 2024 at $160,000 is her peak performance.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Field parameter does not match any column header | Verify field name matches exactly including spelling and spacing |
| 0 (unexpected) | No numeric values in the field column for matching records | Ensure the field contains numbers, not text representations |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Price" not Price |
| #NUM! | Column number outside valid range | Use column number between 1 and total columns in database |
| Wrong result | Criteria headers do not match database headers exactly | Copy headers directly from database to criteria range |
| Wrong result | Criteria range includes extra rows | Ensure criteria range includes only header and criteria rows |
| #REF! | Invalid range reference | Verify database and criteria ranges exist |
| 0 | No records match criteria | Check criteria values against actual database content |

## Use Cases

### Sales Record Analysis

**Scenario:** A sales manager needs to identify peak transaction amounts across different regions, time periods, and product categories to recognize top performers and set realistic targets.

**Implementation:** Build a sales database with Transaction ID, Salesperson, Region, Product, Date, and Amount columns. Create a criteria range where managers can specify filters. DMAX reveals the highest sale for any combination of criteria.

**Technical Details:** Use date criteria like ">=2024-01-01" and "<2024-04-01" for quarterly analysis. Multiple criteria rows enable comparisons like "highest sale in East OR West regions." Name your ranges (SalesData, SalesCriteria) for maintainability. The results inform commission structures and performance benchmarks.

---

### Inventory Peak Tracking

**Scenario:** A warehouse manager needs to identify maximum stock levels historically held for specific product categories to optimize storage allocation and understand seasonal patterns.

**Implementation:** Maintain an inventory history database with Date, Product, Category, Location, and Stock Level columns. The criteria range enables filtering by any combination of attributes.

**Technical Details:** Find peak inventory with `=DMAX(InventoryHistory, "Stock Level", criteria)`. Use wildcard criteria like "2024*" in a Date column to find the maximum during a specific year. Multiple criteria rows help identify "highest Electronics inventory in Warehouse A OR Warehouse B." Results guide capacity planning.

---

### Performance Metrics Dashboard

**Scenario:** An HR department needs to identify the highest performance scores across different departments, job levels, and review periods for calibration discussions.

**Implementation:** Structure performance data with Employee ID, Department, Level, Review Period, and Score columns. The criteria range allows flexible analysis across dimensions.

**Technical Details:** DMAX finds the top score for any filtered group: `=DMAX(Reviews, "Score", DeptCriteria)` shows the highest rating in a department. Compare across departments using separate criteria cells. This informs calibration by showing the ceiling for each group. Combine with DMIN to show the full range of scores.

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
| Date criteria handling | Accepts serial numbers or formatted dates | Prefers DATE() function |
| Result when no matches | Returns 0 | Returns 0 |

**Key Difference:** Both platforms return 0 when no records match the criteria (unlike DAVERAGE which returns #DIV/0!). This can mask cases where criteria are incorrect, so always validate criteria with DCOUNT first.

## Tips and Best Practices

1. **Validate matches with DCOUNT**: Before relying on DMAX results, use DCOUNT with the same criteria to verify records actually match. DMAX returns 0 for both "no matches" and "all matching values are 0," which can be misleading.

2. **Use DMAX with DMIN for range analysis**: Finding both maximum and minimum with the same criteria reveals the spread of values. This is useful for understanding variability: `="Range: " & DMIN(...) & " to " & DMAX(...)`.

3. **Copy headers exactly**: Criteria range headers must match database headers precisely. Copy them directly from your database instead of retyping to avoid invisible differences like extra spaces.

4. **Consider MAXIFS for simpler criteria**: If you only need AND conditions, MAXIFS has a more intuitive inline syntax. Reserve DMAX for scenarios requiring OR logic across multiple columns or frequently changing criteria.

5. **Create dynamic criteria references**: Link criteria cells to other cells or use data validation dropdowns. Users can then change filters without touching the formula, making reports interactive.

6. **Handle the zero-for-no-matches issue**: Use a formula like `=IF(DCOUNT(...)>0, DMAX(...), "No data")` to distinguish between actual zero values and no-match scenarios.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[MAX]] | Maximum value in a range | No filtering capability |
| [[MAXIFS]] | Maximum with multiple criteria | Inline criteria; AND logic only |
| [[DMIN]] | Minimum value matching criteria | Returns smallest instead of largest |
| [[LARGE]] | Nth largest value in range | No filtering; returns by rank |
| [[DSUM]] | Sum values matching criteria | Aggregates instead of finding maximum |
| [[DAVERAGE]] | Average values matching criteria | Returns mean, not maximum |

### Commonly Used With

**[[DMIN]]** - Show the full range of values:
```
Minimum: =DMIN(A1:E100, "Sales", criteria)
Maximum: =DMAX(A1:E100, "Sales", criteria)
Range: =DMAX(...) - DMIN(...)
```
Provides context for understanding value spread.

**[[DCOUNT]]** - Validate that records match:
```
=IF(DCOUNT(A1:E100, "Sales", criteria)>0, DMAX(A1:E100, "Sales", criteria), "No records")
```
Distinguishes zero results from no-match scenarios.

**[[DAVERAGE]]** - Compare maximum to average:
```
Max: =DMAX(database, "Amount", criteria)
Avg: =DAVERAGE(database, "Amount", criteria)
Above Average: =DMAX(...) / DAVERAGE(...)
```
Shows how far the maximum exceeds the average.

## Official Documentation

- [Microsoft Excel DMAX Documentation](https://support.microsoft.com/en-us/office/dmax-function-f4e8209d-8958-4c3d-a1ee-6351665d41c2)
- [Google Sheets DMAX Documentation](https://support.google.com/docs/answer/3094098)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved criteria handling |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | Performance optimizations |
| Excel 2019 | Excel | MAXIFS introduced as modern alternative |
| Excel 365 | Excel | Continued support alongside MAXIFS |
| Initial release | Google Sheets | Full compatibility with Excel |
| 2018 | Google Sheets | Performance improvements |
