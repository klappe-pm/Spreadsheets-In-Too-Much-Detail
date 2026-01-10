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
- conditional-minimum
- data-analysis
- floor-values
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-min
- conditional-database-minimum
tags:
- database-functions
- minimum
- criteria-based
- data-filtering
---

# DMIN

## Description

DMIN returns the smallest value in a database column among records that match specified criteria. While the simple MIN function finds the minimum across an entire range, DMIN first filters records using a criteria range and then identifies the minimum within that subset. This enables targeted analysis like "What was the lowest sale in Q4?" or "What is the minimum processing time for urgent orders?"

The **criteria range** mechanism allows DMIN to filter data before finding the minimum. You create a separate area containing column headers that exactly match your database headers, with filter conditions below. Same-row criteria combine with AND logic (all must be true), while different-row criteria use OR logic (any can be true). This enables complex queries like "find the lowest price among (Electronics with rating >= 4) OR (Home products with stock > 50)."

When deciding between DMIN and MINIFS, consider the complexity of your criteria. DMIN is preferred when criteria change frequently (just modify the criteria range), when you need OR logic across multiple columns, or when maintaining consistency with other database functions in your workbook. MINIFS is more straightforward for simple AND conditions with inline criteria. Both functions ignore text values and empty cells.

Database functions structure data with column headers in the first row and records below. The database parameter must include this header row. The field parameter specifies which column to search for the minimum, accepting a quoted header name, column number (starting at 1), or cell reference. DMIN only evaluates numeric values in the field column; text entries are ignored when determining the minimum.

## Syntax

> [!NOTE] Syntax
> ```
> DMIN(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column to search for the minimum value, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Basic Single Criterion Minimum

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
=DMIN(A1:D6, "Price", F1:F2)
```

**Result:** `25`

**Explanation:** Finds the minimum Price among Electronics products. Widget A at $25 is the lowest-priced Electronics item.

---

### Example 2: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DMIN(A1:D6, 4, F1:F2)
```

**Result:** `60`

**Explanation:** Column 4 is Quantity. Finds minimum quantity among Home products. Gadget Y has 60 units, which is less than Gadget X's 80.

---

### Example 3: Multiple AND Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Price |
|----------|-------|
| Electronics | >25 |

**Formula:**
```
=DMIN(A1:D6, "Quantity", F1:G2)
```

**Result:** `150`

**Explanation:** Finds minimum Quantity for Electronics priced over $25. Widget B (150) and Device Z (200) qualify. Minimum is 150.

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
=DMIN(A1:D6, "Quantity", F1:F3)
```

**Result:** `100`

**Explanation:** Finds minimum Quantity where Price < 30 OR Price > 60. Widget A (100) and Device Z (200) match. Minimum is 100.

---

### Example 5: Complex AND/OR Combination

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G3):**
| Category | Quantity |
|----------|----------|
| Electronics | >100 |
| Home | <80 |

**Formula:**
```
=DMIN(A1:D6, "Price", F1:G3)
```

**Result:** `35`

**Explanation:** Finds minimum Price for (Electronics AND Quantity>100) OR (Home AND Quantity<80). Matches: Widget B ($35), Device Z ($65), Gadget Y ($55). Minimum is $35.

---

### Example 6: Comparison Operators

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Price | Quantity |
|-------|----------|
| <=50 | >=80 |

**Formula:**
```
=DMIN(A1:D6, "Price", F1:G2)
```

**Result:** `25`

**Explanation:** Finds minimum Price where Price <= 50 AND Quantity >= 80. Widget A ($25, 100), Widget B ($35, 150), and Gadget X ($45, 80) match. Minimum is $25.

---

### Example 7: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Gadget* |

**Formula:**
```
=DMIN(A1:D6, "Price", F1:F2)
```

**Result:** `45`

**Explanation:** The asterisk matches any characters. Products starting with "Gadget" are Gadget X ($45) and Gadget Y ($55). Minimum is $45.

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

**Criteria (G1:G2):**
| Department |
|------------|
| Sales |

**Formula:**
```
=DMIN(A1:E6, "Salary", G1:G2)
```

**Result:** `55000`

**Explanation:** Finds minimum Salary among Sales employees. Alice ($55,000), Carol ($62,000), and Eve ($58,000) are in Sales. Minimum is $55,000.

---

### Example 9: Finding Entry-Level Metrics

**Database (A1:E7):**
| Employee | Level | Department | Salary | Start Date |
|----------|-------|------------|--------|------------|
| Alice | Junior | Engineering | 65000 | 2023-06-01 |
| Bob | Senior | Engineering | 95000 | 2019-03-15 |
| Carol | Junior | Marketing | 55000 | 2024-01-10 |
| David | Mid | Engineering | 80000 | 2021-08-20 |
| Eve | Junior | Sales | 52000 | 2023-11-05 |
| Frank | Senior | Marketing | 88000 | 2018-05-12 |

**Criteria (G1:G2):**
| Level |
|-------|
| Junior |

**Formula:**
```
=DMIN(A1:E7, "Salary", G1:G2)
```

**Result:** `52000`

**Explanation:** Finds minimum salary among Junior-level employees. Alice ($65,000), Carol ($55,000), and Eve ($52,000) are Juniors. Minimum is $52,000.

---

### Example 10: Processing Time Analysis

**Database (A1:E8):**
| Order ID | Type | Priority | Processing Days | Amount |
|----------|------|----------|-----------------|--------|
| 1001 | Standard | Normal | 5 | 250 |
| 1002 | Express | Urgent | 1 | 450 |
| 1003 | Standard | Normal | 4 | 180 |
| 1004 | Express | Normal | 2 | 320 |
| 1005 | Standard | Urgent | 2 | 560 |
| 1006 | Express | Urgent | 1 | 290 |
| 1007 | Standard | Normal | 6 | 410 |

**Criteria (G1:G2):**
| Priority |
|----------|
| Urgent |

**Formula:**
```
=DMIN(A1:E8, "Processing Days", G1:G2)
```

**Result:** `1`

**Explanation:** Finds minimum processing time for Urgent priority orders. Orders 1002 (1 day), 1005 (2 days), and 1006 (1 day) qualify. Minimum is 1 day.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Field parameter does not match any column header | Verify field name matches exactly |
| 0 (unexpected) | All matching records have 0 values or no numeric values | Check that field contains numbers |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Salary" not Salary |
| #NUM! | Column number outside valid range | Use number between 1 and total columns |
| Wrong result | Criteria headers do not match database headers | Copy headers from database to criteria range |
| Wrong result | Extra rows in criteria range | Trim criteria range to header plus criteria rows only |
| #REF! | Invalid range reference | Verify all ranges point to existing cells |
| 0 | No records match criteria | Verify criteria values exist in database |

## Use Cases

### Cost Analysis and Procurement

**Scenario:** A procurement team needs to identify the lowest prices for specific product categories across different vendors to negotiate better contracts and identify cost-saving opportunities.

**Implementation:** Create a procurement database with Vendor, Product, Category, Unit Price, and Quantity columns. The criteria range enables filtering by vendor, category, or product specifications.

**Technical Details:** Use `=DMIN(ProcurementData, "Unit Price", criteria)` to find the lowest available price. Multiple criteria rows help compare "lowest from Vendor A OR Vendor B." Combine with DMAX to see the price range and negotiate accordingly. Track results over time to monitor price trends.

---

### Service Level Agreement Monitoring

**Scenario:** An IT department needs to track minimum response times achieved for different ticket categories to ensure SLA compliance and identify best-case performance benchmarks.

**Implementation:** Build a ticket database with Ticket ID, Category, Priority, Response Time, Resolution Time, and Technician columns. The criteria range filters by any combination of attributes.

**Technical Details:** Find fastest response: `=DMIN(Tickets, "Response Time", criteria)`. Use criteria for "Priority = High AND Category = Network" to find best-case scenarios for specific combinations. This helps validate that SLAs are achievable and identifies which conditions produce optimal results.

---

### Real Estate Price Analysis

**Scenario:** A real estate analyst needs to find the lowest property prices in specific neighborhoods, property types, and square footage ranges to identify market entry points.

**Implementation:** Structure property data with Address, Neighborhood, Type, Square Feet, Bedrooms, and Price columns. The criteria range allows filtering by any combination of attributes.

**Technical Details:** Find entry price: `=DMIN(Properties, "Price", criteria)`. Use comparison operators like ">=1500" for square footage and criteria rows for "Neighborhood = Downtown OR Neighborhood = Midtown" to compare minimum prices across areas. Results guide investment recommendations.

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
| Date criteria handling | Accepts serial numbers or dates | Prefers DATE() function |
| Result when no matches | Returns 0 | Returns 0 |

**Key Difference:** Both platforms return 0 when no records match criteria. This behavior is identical to DMAX and can mask criteria errors. Always validate with DCOUNT.

## Tips and Best Practices

1. **Distinguish zero from no matches**: DMIN returns 0 both when the actual minimum is 0 and when no records match. Use `=IF(DCOUNT(...)>0, DMIN(...), "No data")` to differentiate these scenarios.

2. **Pair with DMAX for range analysis**: Using DMIN and DMAX together with the same criteria shows the full spread: `="Range: " & DMIN(...) & " to " & DMAX(...)`. This is essential for understanding data variability.

3. **Copy headers precisely**: Even minor differences between criteria headers and database headers (like trailing spaces) cause matching failures. Always copy headers directly from your database.

4. **Use MINIFS for simpler needs**: When you only need AND logic with a few conditions, MINIFS has cleaner syntax. Reserve DMIN for OR logic requirements or when criteria change frequently.

5. **Create floor-based alerts**: Combine DMIN with conditional formatting or IF statements: `=IF(CurrentValue < DMIN(...)*1.1, "Near minimum", "OK")` alerts when values approach historical lows.

6. **Track minimum over time**: Use DMIN with date criteria to find minimum values for specific periods, enabling trend analysis of best-case performance.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[MIN]] | Minimum value in a range | No filtering capability |
| [[MINIFS]] | Minimum with multiple criteria | Inline criteria; AND logic only |
| [[DMAX]] | Maximum value matching criteria | Returns largest instead of smallest |
| [[SMALL]] | Nth smallest value in range | No filtering; returns by rank |
| [[DSUM]] | Sum values matching criteria | Aggregates instead of finding minimum |
| [[DAVERAGE]] | Average values matching criteria | Returns mean, not minimum |

### Commonly Used With

**[[DMAX]]** - Show the complete range:
```
Minimum: =DMIN(A1:E100, "Sales", criteria)
Maximum: =DMAX(A1:E100, "Sales", criteria)
Spread: =DMAX(...) - DMIN(...)
```
Essential for understanding value distribution.

**[[DCOUNT]]** - Verify matching records exist:
```
=IF(DCOUNT(A1:E100, "Sales", criteria)>0, DMIN(A1:E100, "Sales", criteria), "No records")
```
Prevents misinterpreting 0 as actual minimum.

**[[DAVERAGE]]** - Compare minimum to average:
```
Min: =DMIN(database, "Amount", criteria)
Avg: =DAVERAGE(database, "Amount", criteria)
Below Average: =DAVERAGE(...) / DMIN(...)
```
Shows how far minimum is from the average.

## Official Documentation

- [Microsoft Excel DMIN Documentation](https://support.microsoft.com/en-us/office/dmin-function-4ae6f1d9-1f26-40f1-a783-6dc3680192a3)
- [Google Sheets DMIN Documentation](https://support.google.com/docs/answer/3094099)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved criteria handling |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | Performance optimizations |
| Excel 2019 | Excel | MINIFS introduced as modern alternative |
| Excel 365 | Excel | Continued support alongside MINIFS |
| Initial release | Google Sheets | Full compatibility with Excel |
| 2018 | Google Sheets | Performance improvements |
