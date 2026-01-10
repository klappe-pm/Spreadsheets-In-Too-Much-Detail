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
- non-empty-cells
- data-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-count-all
- conditional-count-non-empty
tags:
- database-functions
- counting
- criteria-based
- data-completeness
---

# DCOUNTA

## Description

DCOUNTA counts all non-empty cells in a database column that match specified criteria, regardless of whether those cells contain numbers, text, or other data types. This distinguishes it from DCOUNT, which only counts numeric values. DCOUNTA is essential for counting records in text columns like names, categories, or status fields, and for assessing data completeness across any field type.

The **criteria range** architecture is central to all database functions including DCOUNTA. This separate range contains column headers that must exactly match your database headers, with criteria values in rows below. Same-row criteria combine with AND logic (all must be true), while different-row criteria create OR logic (any can be true). This powerful structure lets you build complex filters like "count all Completed orders from the East region OR any orders from the West region" without complicated nested formulas.

Deciding between DCOUNTA and COUNTIFS depends on your specific situation. DCOUNTA excels when you need OR logic across different columns, when criteria change frequently (just update the criteria range), or when maintaining legacy spreadsheets using the database function paradigm. COUNTIFS is more intuitive for straightforward AND conditions and does not require setting up a separate criteria range.

In the database function model, your data is structured with column headers in the first row and data records below. The database parameter must include this header row. The field parameter specifies which column to count and accepts a quoted header name, a column number (1 being the first column), or a cell reference. Unlike DCOUNT, DCOUNTA counts any non-empty cell including text strings, numbers, dates, logical values, and error values. Only truly empty cells are excluded from the count.

## Syntax

> [!NOTE] Syntax
> ```
> DCOUNTA(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column to count, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Counting Text Values

**Database (A1:D6):**
| Product | Category | Price | Status |
|---------|----------|-------|--------|
| Widget A | Electronics | 25 | Active |
| Widget B | Electronics | 35 | Discontinued |
| Gadget X | Home | 45 | Active |
| Gadget Y | Home | 55 | Active |
| Device Z | Electronics | 65 | Pending |

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**Formula:**
```
=DCOUNTA(A1:D6, "Status", F1:F2)
```

**Result:** `3`

**Explanation:** Counts non-empty Status values for Electronics products. All three Electronics items (Widget A, Widget B, Device Z) have Status values, even though they are text.

---

### Example 2: DCOUNTA vs DCOUNT Comparison

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**DCOUNTA Formula:**
```
=DCOUNTA(A1:D6, "Product", F1:F2)
```

**DCOUNT Formula:**
```
=DCOUNT(A1:D6, "Product", F1:F2)
```

**DCOUNTA Result:** `3`
**DCOUNT Result:** `0`

**Explanation:** DCOUNTA counts all three Product names (text values). DCOUNT returns 0 because Product contains text, not numbers.

---

### Example 3: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Status |
|--------|
| Active |

**Formula:**
```
=DCOUNTA(A1:D6, 1, F1:F2)
```

**Result:** `3`

**Explanation:** Column 1 is Product. Counts non-empty Product values for Active status items. Widget A, Gadget X, and Gadget Y are Active.

---

### Example 4: Multiple AND Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Status |
|----------|--------|
| Electronics | Active |

**Formula:**
```
=DCOUNTA(A1:D6, "Product", F1:G2)
```

**Result:** `1`

**Explanation:** Only Widget A is both Electronics AND Active. The Product field is counted (non-empty).

---

### Example 5: OR Criteria Across Rows

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F3):**
| Status |
|--------|
| Active |
| Pending |

**Formula:**
```
=DCOUNTA(A1:D6, "Product", F1:F3)
```

**Result:** `4`

**Explanation:** Counts products that are Active OR Pending. Active: Widget A, Gadget X, Gadget Y. Pending: Device Z. Total: 4.

---

### Example 6: Complex AND/OR Combination

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G3):**
| Category | Price |
|----------|-------|
| Electronics | <40 |
| Home | >50 |

**Formula:**
```
=DCOUNTA(A1:D6, "Product", F1:G3)
```

**Result:** `3`

**Explanation:** Counts (Electronics AND Price<40) OR (Home AND Price>50). Matches: Widget A, Widget B (Electronics under $40), Gadget Y (Home over $50).

---

### Example 7: Counting with Missing Data

**Database (A1:D7):**
| Product | Category | Price | Status |
|---------|----------|-------|--------|
| Widget A | Electronics | 25 | Active |
| Widget B | Electronics | 35 |  |
| Gadget X | Home | 45 | Active |
| Gadget Y |  | 55 | Active |
| Device Z | Electronics | 65 | Pending |
| Item Q | Home |  |  |

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**Formula:**
```
=DCOUNTA(A1:D7, "Status", F1:F2)
```

**Result:** `2`

**Explanation:** Electronics items are Widget A, Widget B, and Device Z. Only Widget A (Active) and Device Z (Pending) have non-empty Status values. Widget B has empty Status.

---

### Example 8: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Widget* |

**Formula:**
```
=DCOUNTA(A1:D6, "Category", F1:F2)
```

**Result:** `2`

**Explanation:** Products starting with "Widget" are Widget A and Widget B. Both have non-empty Category values (Electronics).

---

### Example 9: Employee Data Completeness

**Database (A1:E6):**
| Employee | Department | Email | Phone | Emergency |
|----------|------------|-------|-------|-----------|
| Alice | Sales | alice@co.com | 555-0101 | Yes |
| Bob | Marketing | bob@co.com |  |  |
| Carol | Sales | carol@co.com | 555-0103 | Yes |
| David | IT |  | 555-0104 |  |
| Eve | Sales | eve@co.com | 555-0105 | Yes |

**Criteria (G1:G2):**
| Department |
|------------|
| Sales |

**Formula:**
```
=DCOUNTA(A1:E6, "Emergency", G1:G2)
```

**Result:** `3`

**Explanation:** Counts Sales employees with non-empty Emergency contact info. Alice, Carol, and Eve all have "Yes" in the Emergency column.

---

### Example 10: Data Quality Assessment

**Database (A1:E6):** Same as Example 9

**Criteria (G1:G2):**
| Department |
|------------|
| Sales |

**Complete Records Formula:**
```
=DCOUNTA(A1:E6, "Phone", G1:G2)
```

**Total Records Formula:**
```
=DCOUNTA(A1:E6, "Employee", G1:G2)
```

**Complete Result:** `3`
**Total Result:** `3`

**Explanation:** All 3 Sales employees have Phone numbers. Comparing DCOUNTA on different fields reveals data completeness across your database.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Field parameter does not match any column header | Verify field name spelling and spacing match exactly |
| 0 (unexpected) | All cells in the field column are empty for matching records | Verify data exists in the specified column |
| Higher than expected | Criteria range includes blank rows interpreted as "match all" | Trim criteria range to include only header and criteria rows |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Category" not Category |
| #NUM! | Column number outside valid range | Ensure column number is between 1 and total columns |
| Incorrect count | Headers in criteria range do not match database headers | Copy headers directly from database |
| #REF! | Invalid range references | Check that database and criteria ranges exist |
| Count includes errors | Cells contain #N/A or other errors | DCOUNTA counts error cells as non-empty; filter first if needed |

## Use Cases

### Customer Data Completeness Audit

**Scenario:** A CRM administrator needs to assess data completeness across customer records, identifying which segments have complete contact information versus gaps in key fields like email, phone, or address.

**Implementation:** Build a customer database with demographic fields and contact information columns. Set up a criteria range to filter by customer segment, region, or acquisition date. Use DCOUNTA on each contact field to count records with data present.

**Technical Details:** Create a data completeness dashboard comparing DCOUNTA results across fields. For example, `=DCOUNTA(Customers, "Email", criteria)` versus `=DCOUNTA(Customers, "Phone", criteria)` reveals whether email or phone data is more complete for a segment. Use criteria rows for different segments to build a completeness matrix. The difference between total records and field-specific counts identifies data gaps requiring attention.

---

### Project Task Tracking

**Scenario:** A project manager needs to count tasks by status, owner, and phase to generate progress reports and identify workload distribution across team members.

**Implementation:** Maintain a task database with columns for Task Name, Owner, Phase, Status, Priority, and Due Date. The criteria range allows filtering by any combination of these attributes.

**Technical Details:** Count tasks assigned to each owner: `=DCOUNTA(Tasks, "Task Name", OwnerCriteria)`. Use OR criteria rows to count tasks that are "In Progress" OR "Blocked" to identify active work. Compare with completed task counts to track progress. Because task names are text, DCOUNTA is essential (DCOUNT would return 0).

---

### Survey Response Tracking

**Scenario:** A research team needs to count survey respondents who answered specific questions across different demographic segments to ensure adequate sample sizes for analysis.

**Implementation:** Structure survey data with respondent info and response columns for each question. The criteria range enables demographic filtering.

**Technical Details:** DCOUNTA counts respondents who provided any answer to a question (including text responses like "N/A" or "Other"). This is more accurate than DCOUNT for open-ended questions. Compare `=DCOUNTA(Survey, "Q5", criteria)` against total respondents to calculate response rates. Use multiple criteria rows to analyze combined demographics (e.g., "Female 25-34" OR "Female 35-44").

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Counts text cells | Yes | Yes |
| Counts numeric cells | Yes | Yes |
| Counts error cells | Yes (as non-empty) | Yes (as non-empty) |
| Counts cells with formulas returning "" | No (treated as empty) | No (treated as empty) |
| Wildcard support (*, ?) | Yes | Yes |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Structured table references | Supported | Not supported |
| Named ranges | Fully supported | Fully supported |
| Maximum rows | 1,048,576 | ~10,000,000 cells total |
| Date criteria handling | Uses system locale | Prefers DATE() function |

**Key Difference:** Both platforms treat formula results of empty strings ("") as empty, not counted by DCOUNTA. Error values like #N/A are counted as non-empty on both platforms.

## Tips and Best Practices

1. **Use DCOUNTA for text columns**: Whenever your target field contains text (names, categories, status values), always use DCOUNTA. DCOUNT only counts numeric cells and returns 0 for text columns, which is a common source of confusion.

2. **Assess data completeness by comparing fields**: Run DCOUNTA on multiple fields with the same criteria to identify which fields have missing data. For example, if DCOUNTA on "Email" returns 80 but DCOUNTA on "Employee" returns 100, you have 20 records with missing email addresses.

3. **Watch for cells that appear empty but are not**: Cells containing spaces, formulas returning empty strings, or hidden characters will be counted by DCOUNTA. Use TRIM and CLEAN functions on source data to prevent phantom counts.

4. **Include error cells intentionally or not**: DCOUNTA counts cells with #N/A, #VALUE!, and other errors as non-empty. If you need to exclude errors, add a helper column with error checking or use additional criteria.

5. **Validate your criteria range boundaries**: A common mistake is including extra rows in the criteria range. If row 3 appears empty but contains a space, it creates an unintended "match all" condition. Double-check criteria range size.

6. **Create data validation reports**: Combine DCOUNTA with DCOUNT to identify data type issues. If DCOUNTA returns 100 but DCOUNT returns 95 on a numeric field, you have 5 records with text values where numbers are expected.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[DCOUNT]] | Counts numeric cells matching criteria | Only counts numbers; returns 0 for text fields |
| [[COUNTA]] | Counts non-empty cells in a range | No filtering capability |
| [[COUNTIF]] | Counts cells matching a single criterion | Simpler but limited to one condition |
| [[COUNTIFS]] | Counts cells matching multiple criteria | Inline criteria; AND logic only |
| [[DSUM]] | Sums values matching criteria | Aggregates instead of counting |
| [[ROWS]] | Counts rows in a range | Counts all rows regardless of content |

### Commonly Used With

**[[DCOUNT]]** - Compare numeric vs total entries:
```
Total Entries: =DCOUNTA(A1:E100, "Amount", criteria)
Numeric Entries: =DCOUNT(A1:E100, "Amount", criteria)
Non-Numeric: =DCOUNTA(...) - DCOUNT(...)
```
Identifies records with non-numeric data in supposedly numeric fields.

**[[DAVERAGE]]** - Calculate average with sample size:
```
=DAVERAGE(database, "Score", criteria) & " (n=" & DCOUNTA(database, "Score", criteria) & ")"
```
Shows average alongside the number of records included.

**[[IF]]** - Conditional reporting based on counts:
```
=IF(DCOUNTA(database, "Response", criteria) >= 30, "Sufficient", "Need more")
```
Validates sample size before analysis.

## Official Documentation

- [Microsoft Excel DCOUNTA Documentation](https://support.microsoft.com/en-us/office/dcounta-function-00232a6d-5a66-4a01-a25b-c1653fda1244)
- [Google Sheets DCOUNTA Documentation](https://support.google.com/docs/answer/3094223)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved handling of different data types |
| Excel 2007 | Excel | Extended row limits to 1,048,576 |
| Excel 2010 | Excel | Performance improvements for large datasets |
| Excel 365 | Excel | Enhanced compatibility with tables |
| Initial release | Google Sheets | Full compatibility with Excel behavior |
| 2019 | Google Sheets | Performance optimizations |
