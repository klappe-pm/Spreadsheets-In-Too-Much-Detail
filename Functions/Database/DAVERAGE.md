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
- conditional-average
- structured-data
- data-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-average
- conditional-database-average
tags:
- database-functions
- averaging
- criteria-based
- data-filtering
---

# DAVERAGE

## Description

DAVERAGE calculates the average of values in a database column that match specified criteria. Unlike simple AVERAGE which operates on all values in a range, DAVERAGE enables you to filter records based on complex conditions before computing the average. This function is part of the legacy database function family that predates modern functions like AVERAGEIFS, but it remains valuable for scenarios requiring sophisticated multi-criteria filtering through a dedicated criteria range.

The power of DAVERAGE lies in its use of a **criteria range**, a separate area of your spreadsheet where you define filtering conditions. This criteria range must include column headers that exactly match those in your database, with criteria values placed in rows beneath these headers. When multiple criteria appear in the same row, they function as AND conditions (all must be true). When criteria appear in different rows, they function as OR conditions (any can be true). This approach allows for extremely complex filtering logic that would be cumbersome to express in a single formula.

Understanding when to use DAVERAGE versus AVERAGEIFS is crucial for efficient spreadsheet design. DAVERAGE excels when you have numerous criteria combinations that change frequently, since you can modify the criteria range without editing the formula. It also shines when you need OR logic across multiple columns or when working with legacy spreadsheets built around the database function paradigm. AVERAGEIFS, conversely, is more intuitive for simple conditions and integrates better with dynamic array formulas.

Database functions treat your data as a structured database with field names (column headers) in the first row and records (data entries) in subsequent rows. The database range must include headers and all data rows you want to analyze. The field parameter specifies which column to average, and it can be provided as a text string matching the column header, a number indicating the column position (1 for first column), or a cell reference containing either. This flexibility accommodates various workflow preferences.

## Syntax

> [!NOTE] Syntax
> ```
> DAVERAGE(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column to average, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Basic Single Criterion

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
=DAVERAGE(A1:D6, "Price", F1:F2)
```

**Result:** `41.67`

**Explanation:** The function averages the Price column only for records where Category equals "Electronics". The three matching products (Widget A at $25, Widget B at $35, and Device Z at $65) average to $41.67.

---

### Example 2: Using Column Number Instead of Name

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DAVERAGE(A1:D6, 3, F1:F2)
```

**Result:** `50`

**Explanation:** Using `3` instead of "Price" references the third column. This averages prices for Home category products (Gadget X at $45 and Gadget Y at $55), resulting in $50.

---

### Example 3: Multiple AND Criteria (Same Row)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Quantity |
|----------|----------|
| Electronics | >100 |

**Formula:**
```
=DAVERAGE(A1:D6, "Price", F1:G2)
```

**Result:** `50`

**Explanation:** Both conditions must be met (AND logic). Only Widget B ($35, 150 units) and Device Z ($65, 200 units) are Electronics with Quantity > 100. Their average price is $50.

---

### Example 4: Multiple OR Criteria (Different Rows)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F3):**
| Category |
|----------|
| Electronics |
| Home |

**Formula:**
```
=DAVERAGE(A1:D6, "Price", F1:F3)
```

**Result:** `45`

**Explanation:** Criteria in different rows create OR logic. All products match either Electronics OR Home, so all five prices are averaged: (25+35+45+55+65)/5 = 45.

---

### Example 5: Combining AND and OR Logic

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G3):**
| Category | Price |
|----------|-------|
| Electronics | <40 |
| Home | >50 |

**Formula:**
```
=DAVERAGE(A1:D6, "Quantity", F1:G3)
```

**Result:** `103.33`

**Explanation:** This finds (Electronics AND Price<40) OR (Home AND Price>50). Matches: Widget A (100), Widget B (150), Gadget Y (60). Average quantity: (100+150+60)/3 = 103.33.

---

### Example 6: Using Comparison Operators

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Price | Quantity |
|-------|----------|
| >=35 | <=150 |

**Formula:**
```
=DAVERAGE(A1:D6, "Price", F1:G2)
```

**Result:** `45`

**Explanation:** Finds products with Price >= 35 AND Quantity <= 150. Matches: Widget B ($35), Gadget X ($45), Gadget Y ($55). Average: (35+45+55)/3 = 45.

---

### Example 7: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Widget* |

**Formula:**
```
=DAVERAGE(A1:D6, "Price", F1:F2)
```

**Result:** `30`

**Explanation:** The asterisk wildcard matches any characters. Products starting with "Widget" (Widget A at $25 and Widget B at $35) are averaged: (25+35)/2 = 30.

---

### Example 8: Empty Criteria (All Records)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
|  |

**Formula:**
```
=DAVERAGE(A1:D6, "Price", F1:F2)
```

**Result:** `45`

**Explanation:** An empty criteria cell matches all records. This is equivalent to a simple AVERAGE of the entire Price column: (25+35+45+55+65)/5 = 45.

---

### Example 9: Text Criteria with Exact Match

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
=DAVERAGE(A1:E6, "Salary", G1:H2)
```

**Result:** `56500`

**Explanation:** Averages salaries for Sales employees in the North region. Alice ($55,000) and Eve ($58,000) match both criteria. Average: (55000+58000)/2 = 56,500.

---

### Example 10: Calculated Field Reference

**Database (A1:E6):** Same as Example 9

**Criteria (G1:G2):**
| Years |
|-------|
| >3 |

**Formula:**
```
=DAVERAGE(A1:E6, 4, G1:G2)
```

**Result:** `61667`

**Explanation:** Using column number 4 (Salary), averages salaries for employees with more than 3 years tenure. Bob ($48,000, 5 years), Carol ($62,000, 7 years), and David ($75,000, 4 years) qualify. Average: (48000+62000+75000)/3 = 61,666.67.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Field parameter does not match any column header or is an invalid column number | Verify the field name matches exactly (including spaces and case in some contexts) or use a valid column number within range |
| #DIV/0! | No records match the criteria, resulting in division by zero | Verify criteria values are correct and exist in the database; use IFERROR to handle cases with no matches |
| #NAME? | Field name is not enclosed in quotes when using text | Wrap field names in quotation marks: "Price" not Price |
| #NUM! | Column number is less than 1 or greater than the number of columns | Ensure column number is between 1 and the total number of columns in the database range |
| Incorrect results | Criteria range headers do not exactly match database headers | Copy headers directly from the database to the criteria range to ensure exact matches |
| Incorrect results | Criteria range includes extra rows with unintended values | Ensure criteria range only includes header row and rows with actual criteria (no blank rows with spaces) |
| #REF! | Database or criteria range reference is invalid | Check that all range references point to existing cells and the database includes headers |

## Use Cases

### Sales Performance Analysis

**Scenario:** A retail company needs to analyze average transaction values across different store locations, product categories, and time periods to identify high-performing segments and optimize inventory allocation.

**Implementation:** Create a database with columns for Transaction ID, Store, Category, Date, and Amount. Set up a criteria range where managers can specify any combination of store location, product category, and date ranges. The DAVERAGE formula calculates average transaction values for the filtered subset, enabling quick comparisons like "What's the average Electronics purchase at our Downtown store during Q4?"

**Technical Details:** Use named ranges for the database to accommodate growing data. Include date criteria using operators like ">=2024-01-01" and "<2024-04-01" for quarterly analysis. The criteria range can span multiple rows to compare OR conditions (e.g., average across Downtown OR Suburban stores). This approach allows non-technical users to modify analysis parameters without touching formulas.

---

### Manufacturing Quality Control

**Scenario:** A manufacturing plant tracks product measurements and needs to calculate average dimensions for specific machine configurations, shifts, and material batches to identify sources of variation.

**Implementation:** Maintain a quality database with columns for Part ID, Machine, Shift, Material Batch, Measurement Type, and Value. The criteria range allows engineers to filter by any combination of factors. DAVERAGE calculates the mean measurement for matching parts, which can be compared against specification targets.

**Technical Details:** Use multiple criteria rows to analyze "Machine A on Day Shift" OR "Machine B on Night Shift" simultaneously. Wildcard criteria like "Batch-2024*" can match all batches from a specific year. Combine with DSTDEV calculations to assess both central tendency and variation. Results feed into control charts for ongoing process monitoring.

---

### Academic Performance Tracking

**Scenario:** A university registrar needs to calculate average grades across various dimensions including department, course level, semester, and instructor to support curriculum review and faculty evaluation.

**Implementation:** Build a grade database with Student ID, Department, Course Number, Level, Semester, Instructor, and Grade columns. The criteria range enables flexible filtering such as "all 300-level courses in the Math department during Fall 2024" or comparisons across instructors teaching the same course.

**Technical Details:** Use comparison operators in criteria like ">=300" and "<400" for course level filtering. Text wildcards can match course prefixes (e.g., "MATH*" for all math courses). Create separate DAVERAGE formulas for different metrics (overall average, by level, by instructor) that all reference the same criteria range, enabling synchronized filtering across multiple calculations.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Wildcard characters (*, ?) | Supported in criteria | Supported in criteria |
| Case sensitivity | Case-insensitive matching | Case-insensitive matching |
| Array formula context | Works in array formulas | Works in array formulas |
| Structured table references | Supported (Table[Column]) | Not supported (use range references) |
| Dynamic array spill | Not applicable (returns single value) | Not applicable (returns single value) |
| Named ranges in criteria | Fully supported | Fully supported |
| Maximum database rows | Limited by worksheet size (1,048,576 rows) | Limited by worksheet size (10,000,000 cells total) |
| Performance with large datasets | Generally faster | May slow with very large criteria ranges |
| Date criteria format | Follows system locale | Requires DATE function or ISO format |
| Criteria range location | Can be on any sheet | Can be on any sheet |

**Key Difference:** In Excel, structured table references like `Table1[Amount]` can be used in the database parameter, making formulas more readable and automatically adjusting as the table grows. Google Sheets requires traditional range references and does not support this syntax.

## Tips and Best Practices

1. **Place criteria ranges strategically**: Position your criteria range away from your database (often to the right or on a separate sheet) to prevent accidental interference when sorting or filtering the main data. Use named ranges for both database and criteria to make formulas more readable and maintainable.

2. **Copy headers exactly**: Instead of retyping column headers in your criteria range, copy them directly from the database header row. Even invisible differences like trailing spaces or different dash characters will cause the function to fail silently by matching no records.

3. **Leave criteria cells truly empty for "match all"**: If you want a criteria column to match all values, leave the cell completely empty. A space character or formula returning an empty string may not work the same way. Use ISBLANK to verify cells are truly empty.

4. **Use helper columns for complex criteria**: When your criteria involve calculations (like "above average price"), create a helper column in your database with the calculated result (TRUE/FALSE), then use that column in your criteria range. This keeps the criteria range simple and maintainable.

5. **Combine with IFERROR for robustness**: Wrap DAVERAGE in IFERROR to handle cases where no records match the criteria. For example: `=IFERROR(DAVERAGE(database, "Price", criteria), "No matching records")` prevents #DIV/0! errors from disrupting your reports.

6. **Consider AVERAGEIFS for simpler cases**: If you only need basic AND conditions without complex OR logic, AVERAGEIFS offers a more intuitive syntax without requiring a separate criteria range. Reserve DAVERAGE for scenarios with frequently changing criteria or complex AND/OR combinations.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[AVERAGEIF]] | Averages cells matching a single criterion | Simpler syntax but limited to one condition on one range |
| [[AVERAGEIFS]] | Averages cells matching multiple criteria | Criteria specified inline, all conditions are AND logic |
| [[DSUM]] | Sums database values matching criteria | Returns sum instead of average |
| [[DCOUNT]] | Counts database records with numbers matching criteria | Counts numeric entries instead of averaging |
| [[DGET]] | Retrieves a single value matching criteria | Returns one value; errors if multiple matches |
| [[AVERAGE]] | Simple average of a range | No filtering capability |

### Commonly Used With

**[[DCOUNT]]** - Count matching records to understand sample size:
```
=DAVERAGE(A1:D100, "Sales", criteria) & " (n=" & DCOUNT(A1:D100, "Sales", criteria) & ")"
```
Shows average along with count of records used in calculation.

**[[DSUM]]** - Calculate total alongside average for context:
```
Total: =DSUM(A1:D100, "Sales", criteria)
Average: =DAVERAGE(A1:D100, "Sales", criteria)
```
Provides both aggregate and average for filtered data.

**[[IFERROR]]** - Handle cases with no matching records:
```
=IFERROR(DAVERAGE(A1:D100, "Sales", criteria), 0)
```
Returns 0 instead of #DIV/0! when no records match.

## Official Documentation

- [Microsoft Excel DAVERAGE Documentation](https://support.microsoft.com/en-us/office/daverage-function-a6a2d5ac-4b4b-48cd-a1d8-7b37834e5aee)
- [Google Sheets DAVERAGE Documentation](https://support.google.com/docs/answer/3094148)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of original database functions |
| Excel 97 | Excel | Improved handling of date criteria |
| Excel 2007 | Excel | Increased row limit from 65,536 to 1,048,576 |
| Excel 2010 | Excel | Improved performance with large datasets |
| Excel 365 | Excel | Enhanced compatibility with structured table references |
| Initial release | Google Sheets | Full compatibility with Excel syntax and behavior |
| 2019 | Google Sheets | Performance improvements for large datasets |
