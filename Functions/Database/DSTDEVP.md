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
- population-standard-deviation
- statistical-analysis
- data-variability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-stdevp
- population-standard-deviation-database
tags:
- database-functions
- statistics
- standard-deviation
- criteria-based
---

# DSTDEVP

## Description

DSTDEVP calculates the population standard deviation of values in a database column among records that match specified criteria. Standard deviation quantifies how spread out values are from their mean. DSTDEVP treats the matching records as the entire population, using n (the count of values) as the denominator. This is the appropriate choice when your data represents the complete set of values you care about, not a sample from a larger group.

The **criteria range** allows DSTDEVP to filter records before calculating variability. Create a separate area with column headers that exactly match your database headers. Place conditions below these headers. Same-row criteria combine with AND logic (all must be true), while different-row criteria create OR logic (any can be true). This enables targeted variability analysis like "population standard deviation of scores for all students in (Grade 10 Math) OR (Grade 11 Math)."

The distinction between DSTDEVP (population) and DSTDEV (sample) is statistically crucial. Use DSTDEVP when your data includes every member of the group you are analyzing. For instance, if you have test scores for all students in a class and want to know the variability of that specific class, use DSTDEVP. If you sampled 30 students to estimate variability across the entire school, use DSTDEV. DSTDEVP produces slightly smaller values because it does not add the correction factor for sample uncertainty.

Database functions require data structured with column headers in the first row and records below. The database range must include headers. The field parameter specifies which column to analyze, accepting a quoted header name, column number (starting at 1), or cell reference. DSTDEVP requires at least one numeric value; with no matching values, it returns #DIV/0!. Text and empty cells are ignored in the calculation.

## Syntax

> [!NOTE] Syntax
> ```
> DSTDEVP(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column for which to calculate population standard deviation, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Basic Single Criterion

**Database (A1:D6):**
| Product | Category | Price | Rating |
|---------|----------|-------|--------|
| Widget A | Electronics | 25 | 4.2 |
| Widget B | Electronics | 35 | 4.5 |
| Gadget X | Home | 45 | 3.8 |
| Gadget Y | Home | 55 | 4.1 |
| Device Z | Electronics | 65 | 4.8 |

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**Formula:**
```
=DSTDEVP(A1:D6, "Price", F1:F2)
```

**Result:** `16.33`

**Explanation:** Calculates population standard deviation of all Electronics prices (25, 35, 65). Mean is 41.67, and population standard deviation is approximately 16.33 (using n=3 as denominator).

---

### Example 2: Comparing DSTDEVP vs DSTDEV

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**DSTDEVP Formula:**
```
=DSTDEVP(A1:D6, "Price", F1:F2)
```

**DSTDEV Formula:**
```
=DSTDEV(A1:D6, "Price", F1:F2)
```

**DSTDEVP Result:** `16.33`
**DSTDEV Result:** `20`

**Explanation:** Population standard deviation (16.33) is smaller than sample standard deviation (20). DSTDEV uses n-1=2 as denominator; DSTDEVP uses n=3.

---

### Example 3: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DSTDEVP(A1:D6, 4, F1:F2)
```

**Result:** `0.15`

**Explanation:** Column 4 is Rating. Population standard deviation of Home product ratings (3.8, 4.1) is 0.15.

---

### Example 4: Multiple AND Criteria

**Database (A1:E7):**
| Student | Grade | Subject | Score | Term |
|---------|-------|---------|-------|------|
| Alice | 10 | Math | 85 | Fall |
| Bob | 10 | Math | 92 | Fall |
| Carol | 10 | Math | 78 | Fall |
| David | 10 | Science | 88 | Fall |
| Eve | 11 | Math | 95 | Fall |
| Frank | 10 | Math | 81 | Fall |

**Criteria (G1:H2):**
| Grade | Subject |
|-------|---------|
| 10 | Math |

**Formula:**
```
=DSTDEVP(A1:E7, "Score", G1:H2)
```

**Result:** `5.12`

**Explanation:** Population standard deviation of scores for all Grade 10 Math students (Alice: 85, Bob: 92, Carol: 78, Frank: 81). This represents the complete population of Grade 10 Math students in this dataset.

---

### Example 5: OR Criteria (Multiple Rows)

**Database (A1:E7):** Same as Example 4

**Criteria (G1:G3):**
| Subject |
|---------|
| Math |
| Science |

**Formula:**
```
=DSTDEVP(A1:E7, "Score", G1:G3)
```

**Result:** `5.56`

**Explanation:** Population standard deviation of all Math OR Science scores (85, 92, 78, 88, 95, 81). Includes all records.

---

### Example 6: Complete Class Analysis

**Database (A1:D11):**
| Student | Class | Test1 | Test2 |
|---------|-------|-------|-------|
| Amy | A | 88 | 92 |
| Ben | A | 76 | 80 |
| Cal | A | 95 | 91 |
| Dan | A | 82 | 85 |
| Eve | A | 90 | 88 |
| Fay | B | 72 | 78 |
| Guy | B | 85 | 82 |
| Hal | B | 79 | 81 |
| Ivy | B | 91 | 89 |
| Joy | B | 68 | 74 |

**Criteria (F1:F2):**
| Class |
|-------|
| A |

**Formula:**
```
=DSTDEVP(A1:D11, "Test1", F1:F2)
```

**Result:** `6.36`

**Explanation:** Population standard deviation of Test1 scores for entire Class A (88, 76, 95, 82, 90). Since this is the complete class, not a sample, DSTDEVP is appropriate.

---

### Example 7: Manufacturing Process Analysis

**Database (A1:D9):**
| Batch | Line | Weight | Time |
|-------|------|--------|------|
| 001 | A | 100.2 | 45 |
| 002 | A | 99.8 | 47 |
| 003 | A | 100.5 | 44 |
| 004 | A | 99.5 | 46 |
| 005 | B | 100.8 | 42 |
| 006 | B | 99.2 | 48 |
| 007 | B | 101.2 | 43 |
| 008 | B | 98.8 | 49 |

**Criteria (F1:F2):**
| Line |
|------|
| A |

**Formula:**
```
=DSTDEVP(A1:D9, "Weight", F1:F2)
```

**Result:** `0.37`

**Explanation:** Population standard deviation of weights from Line A production (100.2, 99.8, 100.5, 99.5). This includes all Line A batches in the period.

---

### Example 8: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| *get* |

**Formula:**
```
=DSTDEVP(A1:D6, "Price", F1:F2)
```

**Result:** `5`

**Explanation:** Population standard deviation of prices for products containing "get" (Gadget X: $45, Gadget Y: $55). Result is exactly 5.

---

### Example 9: Single Value Returns Zero

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Device Z |

**Formula:**
```
=DSTDEVP(A1:D6, "Price", F1:F2)
```

**Result:** `0`

**Explanation:** Only one record matches (Device Z). A single value has no variability from itself, so population standard deviation is 0. (Note: DSTDEV would return #DIV/0! for this case.)

---

### Example 10: Department Performance Analysis

**Database (A1:E8):**
| Employee | Dept | Q1 | Q2 | Q3 |
|----------|------|-----|-----|-----|
| Alice | Sales | 105 | 110 | 108 |
| Bob | Sales | 95 | 102 | 98 |
| Carol | Sales | 112 | 118 | 115 |
| David | IT | 88 | 92 | 90 |
| Eve | IT | 95 | 98 | 96 |
| Frank | IT | 82 | 85 | 84 |
| Grace | Sales | 100 | 105 | 103 |

**Criteria (G1:G2):**
| Dept |
|------|
| Sales |

**Formula:**
```
=DSTDEVP(A1:E8, "Q1", G1:G2)
```

**Result:** `6.24`

**Explanation:** Population standard deviation of Q1 performance for all Sales employees (105, 95, 112, 100). This is the entire Sales department.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | No numeric values match criteria | Verify criteria values; ensure matching records have numeric data |
| #VALUE! | Field parameter does not match any column header | Check field name spelling and spacing |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Score" not Score |
| #NUM! | Column number outside valid range | Use number between 1 and total columns |
| 0 (when expected otherwise) | Only one value matches | Single values have zero population deviation |
| Wrong result | Criteria headers do not match database headers | Copy headers directly from database |
| #REF! | Invalid range reference | Verify all ranges point to existing cells |

## Use Cases

### Complete Census Analysis

**Scenario:** A school administrator needs to analyze test score variability across the entire student body to understand achievement gaps and set appropriate targets.

**Implementation:** Create a student database with ID, Grade, Subject, Teacher, and Score columns. Since the data includes all students (not a sample), DSTDEVP calculates the true population variability.

**Technical Details:** Formula `=DSTDEVP(Students, "Score", criteria)` with criteria for "Grade = 10 AND Subject = Math" shows variability among all 10th grade math students. Lower deviation indicates more consistent performance. Compare across subjects: Math deviation of 8 vs English deviation of 15 suggests more variable English performance. Results guide curriculum adjustments.

---

### Complete Transaction Analysis

**Scenario:** A retail store needs to analyze the variability of all transaction amounts during a specific period to understand purchasing patterns and optimize pricing strategies.

**Implementation:** Build a transaction database with Date, Time, Category, Amount, and Payment Method columns. The criteria range filters by any combination of attributes.

**Technical Details:** Since the data includes every transaction (the complete population), DSTDEVP is correct. Formula `=DSTDEVP(Transactions, "Amount", criteria)` calculates variability. High deviation in Electronics category suggests wide price range; low deviation in Grocery suggests more uniform purchases. Use multiple criteria rows for "Weekday OR Weekend" comparisons.

---

### Equipment Performance Monitoring

**Scenario:** A facility manager needs to measure variability in equipment performance metrics across all machines to identify units needing maintenance or replacement.

**Implementation:** Structure performance data with Machine ID, Type, Location, Efficiency, and Output columns. DSTDEVP analyzes variability across the complete equipment inventory.

**Technical Details:** For a fleet of 20 machines, the data represents the entire population. `=DSTDEVP(Machines, "Efficiency", TypeCriteria)` shows how much efficiency varies among machines of the same type. High deviation suggests some machines underperform. Pair with DAVERAGE to see both average efficiency and its variability. Results prioritize maintenance schedules.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Calculation method | Population standard deviation (n) | Population standard deviation (n) |
| Minimum values required | 1 | 1 |
| Result with 1 value | 0 | 0 |
| Result with 0 values | #DIV/0! | #DIV/0! |
| Wildcard support | Yes | Yes |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Structured table references | Supported | Not supported |
| Named ranges | Fully supported | Fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Maximum rows | 1,048,576 | ~10,000,000 cells |

**Key Difference:** Both platforms calculate population standard deviation identically. The only notable difference is the single-value case returns 0 (unlike DSTDEV which returns #DIV/0! for a single value).

## Tips and Best Practices

1. **Use for complete populations only**: DSTDEVP is correct when your data includes every member of the group you are analyzing. If your data is a sample intended to represent a larger population, use DSTDEV instead.

2. **Single values return zero**: Unlike DSTDEV, DSTDEVP returns 0 when only one value matches (not #DIV/0!). This is mathematically correct but may indicate criteria are too restrictive.

3. **Understand the mathematical difference**: DSTDEVP divides by n; DSTDEV divides by n-1. For large datasets (n > 30), the difference is negligible. For small datasets, it can be significant.

4. **Combine with DAVERAGE for context**: Standard deviation is most meaningful alongside the mean. Report as "Average: 85, SD: 5.2" to give readers complete context.

5. **Use for quality control of known populations**: In manufacturing, when analyzing all products from a specific batch (the complete population of that batch), DSTDEVP gives the true variability.

6. **Compare across groups**: Calculate DSTDEVP for different filtered groups using the same criteria columns but different criteria values. Higher deviation indicates less consistency within that group.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[STDEV.P]] / [[STDEVP]] | Population standard deviation of range | No filtering capability |
| [[DSTDEV]] | Sample standard deviation with criteria | Uses n-1 denominator |
| [[DVARP]] | Population variance with criteria | Returns variance (deviation squared) |
| [[DVAR]] | Sample variance with criteria | Variance with n-1 denominator |
| [[DAVERAGE]] | Average matching criteria | Returns mean, not spread |
| [[DCOUNT]] | Count matching criteria | Helps verify data count |

### Commonly Used With

**[[DAVERAGE]]** - Complete statistical summary:
```
Mean: =DAVERAGE(database, "Score", criteria)
SD: =DSTDEVP(database, "Score", criteria)
Report: ="Mean: " & ROUND(DAVERAGE(...),1) & ", SD: " & ROUND(DSTDEVP(...),2)
```
Standard format for descriptive statistics.

**[[DCOUNT]]** - Verify population size:
```
=DCOUNT(database, "Score", criteria) & " values, SD: " & ROUND(DSTDEVP(...),2)
```
Reports sample size alongside variability.

**[[DSTDEV]]** - Compare population vs sample:
```
Population SD: =DSTDEVP(database, "Value", criteria)
Sample SD: =DSTDEV(database, "Value", criteria)
```
Understand which is appropriate for your analysis.

## Official Documentation

- [Microsoft Excel DSTDEVP Documentation](https://support.microsoft.com/en-us/office/dstdevp-function-04b78995-da03-4813-bbd9-d74fd0f5d94b)
- [Google Sheets DSTDEVP Documentation](https://support.google.com/docs/answer/3094087)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved numerical precision |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | STDEV.P introduced as non-database alternative |
| Excel 365 | Excel | Continued support |
| Initial release | Google Sheets | Full compatibility with Excel |
| 2018 | Google Sheets | Performance improvements |
