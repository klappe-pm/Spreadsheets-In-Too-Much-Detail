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
- population-variance
- statistical-analysis
- data-variability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-variance-population
- population-variance-database
tags:
- database-functions
- statistics
- variance
- criteria-based
---

# DVARP

## Description

DVARP calculates the population variance of values in a database column among records that match specified criteria. Variance measures how spread out values are from their mean, expressed as the average of squared deviations. DVARP treats the matching records as the entire population, using n (the count of values) as the denominator. This is appropriate when your data represents every member of the group you are analyzing, not a sample from a larger population.

The **criteria range** allows DVARP to filter records before calculating variance. Create a separate area with column headers that exactly match your database headers. Place conditions below these headers. Same-row criteria combine with AND logic (all must be true), while different-row criteria create OR logic (any can be true). This enables targeted analysis like "population variance of all test scores for Grade 10 students."

The distinction between DVARP (population) and DVAR (sample) is statistically important. Use DVARP when your data includes every member of the group you care about. For example, if you have performance scores for all employees in a department and want to know the variance for that specific department, use DVARP. If you sampled some employees to estimate variance across all departments, use DVAR. DVARP produces slightly smaller values because it does not include Bessel's correction for sample uncertainty.

Database functions structure data with column headers in the first row and records below. The database range must include headers. The field parameter specifies which column to analyze, accepting a quoted header name, column number (starting at 1), or cell reference. DVARP requires at least one numeric value; with no matching values, it returns #DIV/0!. Text and empty cells are ignored.

## Syntax

> [!NOTE] Syntax
> ```
> DVARP(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column for which to calculate population variance, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
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
=DVARP(A1:D6, "Price", F1:F2)
```

**Result:** `266.67`

**Explanation:** Population variance of all Electronics prices (25, 35, 65). Mean is 41.67; population variance is 266.67 (using n=3 as denominator).

---

### Example 2: Comparing DVARP vs DVAR

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**DVARP Formula:**
```
=DVARP(A1:D6, "Price", F1:F2)
```

**DVAR Formula:**
```
=DVAR(A1:D6, "Price", F1:F2)
```

**DVARP Result:** `266.67`
**DVAR Result:** `400`

**Explanation:** Population variance (266.67) is smaller than sample variance (400). DVAR uses n-1=2 as denominator; DVARP uses n=3.

---

### Example 3: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DVARP(A1:D6, 3, F1:F2)
```

**Result:** `25`

**Explanation:** Column 3 is Price. Population variance of all Home prices (45, 55). Mean is 50; population variance is 25.

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
=DVARP(A1:E7, "Score", G1:H2)
```

**Result:** `26.19`

**Explanation:** Population variance of scores for all Grade 10 Math students (85, 92, 78, 81). This is the complete population of Grade 10 Math students in the dataset.

---

### Example 5: OR Criteria

**Database (A1:E7):** Same as Example 4

**Criteria (G1:G3):**
| Subject |
|---------|
| Math |
| Science |

**Formula:**
```
=DVARP(A1:E7, "Score", G1:G3)
```

**Result:** `30.92`

**Explanation:** Population variance of all Math OR Science scores (85, 92, 78, 88, 95, 81). Includes all records.

---

### Example 6: Complete Department Analysis

**Database (A1:D9):**
| Employee | Department | Performance | Tenure |
|----------|------------|-------------|--------|
| Alice | Sales | 88 | 3 |
| Bob | Sales | 92 | 5 |
| Carol | Sales | 85 | 2 |
| David | Sales | 90 | 4 |
| Eve | IT | 78 | 6 |
| Frank | IT | 85 | 3 |
| Grace | IT | 82 | 4 |
| Henry | IT | 80 | 2 |

**Criteria (F1:F2):**
| Department |
|------------|
| Sales |

**Formula:**
```
=DVARP(A1:D9, "Performance", F1:F2)
```

**Result:** `6.69`

**Explanation:** Population variance of performance scores for all Sales employees (88, 92, 85, 90). This represents the entire Sales department.

---

### Example 7: Relationship to Standard Deviation

**Database (A1:D9):** Same as Example 6

**Criteria (F1:F2):**
| Department |
|------------|
| IT |

**DVARP Formula:**
```
=DVARP(A1:D9, "Performance", F1:F2)
```

**DSTDEVP Formula:**
```
=DSTDEVP(A1:D9, "Performance", F1:F2)
```

**DVARP Result:** `6.69`
**DSTDEVP Result:** `2.59`

**Explanation:** SQRT(6.69) = 2.59. Population standard deviation is the square root of population variance.

---

### Example 8: Single Value Returns Zero

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Device Z |

**Formula:**
```
=DVARP(A1:D6, "Price", F1:F2)
```

**Result:** `0`

**Explanation:** Only one record matches. A single value has no deviation from itself, so population variance is 0. (Note: DVAR would return #DIV/0! for this case.)

---

### Example 9: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Widget* |

**Formula:**
```
=DVARP(A1:D6, "Price", F1:F2)
```

**Result:** `25`

**Explanation:** Population variance of prices for products starting with "Widget" (Widget A: 25, Widget B: 35). Mean is 30; population variance is 25.

---

### Example 10: Manufacturing Batch Analysis

**Database (A1:D11):**
| Part | Batch | Dimension | Shift |
|------|-------|-----------|-------|
| 001 | X | 50.02 | Day |
| 002 | X | 49.98 | Day |
| 003 | X | 50.05 | Night |
| 004 | X | 49.95 | Night |
| 005 | X | 50.00 | Day |
| 006 | Y | 50.15 | Day |
| 007 | Y | 49.85 | Day |
| 008 | Y | 50.10 | Night |
| 009 | Y | 49.90 | Night |
| 010 | Y | 50.00 | Day |

**Criteria (F1:F2):**
| Batch |
|-------|
| X |

**Formula:**
```
=DVARP(A1:D11, "Dimension", F1:F2)
```

**Result:** `0.00104`

**Explanation:** Population variance of all Batch X dimensions (50.02, 49.98, 50.05, 49.95, 50.00). This is the complete population of Batch X parts.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | No numeric values match criteria | Verify criteria; ensure matching records have numeric data |
| #VALUE! | Field parameter does not match any column header | Check field name spelling and spacing |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Score" not Score |
| #NUM! | Column number outside valid range | Use number between 1 and total columns |
| 0 (when unexpected) | Only one value matches | Single values have zero population variance |
| Wrong result | Criteria headers do not match database headers | Copy headers from database |
| #REF! | Invalid range reference | Verify all ranges exist |

## Use Cases

### Complete Census Data Analysis

**Scenario:** A school administrator needs to analyze the variance in student test scores across the entire student body to understand achievement disparities.

**Implementation:** Create a student database with ID, Grade, Class, Subject, and Score columns. Since data includes all students (complete census), DVARP calculates true population variance.

**Technical Details:** Formula `=DVARP(Students, "Score", criteria)` with criteria for "Grade = 10 AND Subject = Math" gives the true variance for all 10th grade math scores. Lower variance indicates more consistent performance. Compare across grades or subjects to identify areas with high variability requiring intervention.

---

### Complete Inventory Analysis

**Scenario:** A warehouse manager needs to analyze variance in stock levels across all products in specific categories to understand inventory consistency.

**Implementation:** Build an inventory database with SKU, Category, Location, Quantity, and Reorder Point columns. Since data includes all products, DVARP provides population variance.

**Technical Details:** Calculate `=DVARP(Inventory, "Quantity", criteria)` for each category. High variance in a category suggests inconsistent inventory management. Compare "Location = Warehouse A" vs "Location = Warehouse B" to identify which warehouse maintains more consistent stock levels.

---

### Complete Production Run Analysis

**Scenario:** A quality engineer needs to measure variance in product measurements across all units in a production batch to assess process capability.

**Implementation:** Structure production data with Unit ID, Batch, Line, Measurement, and Timestamp columns. DVARP analyzes the complete batch (all units produced).

**Technical Details:** Population variance is appropriate because the batch represents the complete population of interest. Formula `=DVARP(Production, "Measurement", BatchCriteria)` gives true process variance for that batch. Results feed into process capability indices (Cp, Cpk) and control chart limits.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Calculation method | Population variance (n) | Population variance (n) |
| Minimum values required | 1 | 1 |
| Result with 1 value | 0 | 0 |
| Result with 0 values | #DIV/0! | #DIV/0! |
| Wildcard support | Yes | Yes |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Structured table references | Supported | Not supported |
| Named ranges | Fully supported | Fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Maximum rows | 1,048,576 | ~10,000,000 cells |

**Key Difference:** Both platforms calculate population variance identically. Single values return 0 (not #DIV/0! like DVAR).

## Tips and Best Practices

1. **Use for complete populations only**: DVARP is correct when your data includes every member of the group you are analyzing. If your data is a sample intended to represent a larger population, use DVAR instead.

2. **Single values return zero**: Unlike DVAR, DVARP returns 0 when only one value matches (not #DIV/0!). This is mathematically correct but may indicate overly restrictive criteria.

3. **Understand the mathematical difference**: DVARP divides by n; DVAR divides by n-1. For large datasets (n > 30), the difference is small. For small datasets, it can be significant.

4. **Convert to standard deviation when needed**: Population standard deviation = SQRT(DVARP). Standard deviation is often easier to interpret because it uses original units.

5. **Use for process capability**: In quality management, population variance from complete production runs feeds into capability calculations. Smaller variance means more capable processes.

6. **Compare groups using the same function**: When comparing variance across groups, consistently use either DVAR or DVARP for all groups to ensure fair comparison.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[VAR.P]] / [[VARP]] | Population variance of range | No filtering capability |
| [[DVAR]] | Sample variance with criteria | Uses n-1 denominator |
| [[DSTDEVP]] | Population standard deviation with criteria | Returns square root of variance |
| [[DSTDEV]] | Sample standard deviation with criteria | SD with n-1 denominator |
| [[DAVERAGE]] | Average matching criteria | Returns mean, not spread |
| [[DCOUNT]] | Count matching criteria | Verifies population size |

### Commonly Used With

**[[DSTDEVP]]** - Convert to standard deviation:
```
Population Variance: =DVARP(database, "Value", criteria)
Population Std Dev: =SQRT(DVARP(...))
Or directly: =DSTDEVP(database, "Value", criteria)
```
Standard deviation uses original units.

**[[DAVERAGE]]** - Complete statistical summary:
```
Mean: =DAVERAGE(database, "Score", criteria)
Variance: =DVARP(database, "Score", criteria)
Std Dev: =DSTDEVP(database, "Score", criteria)
```
Provides full descriptive statistics.

**[[DCOUNT]]** - Report population size:
```
="N=" & DCOUNT(database, "Score", criteria) & ", Variance=" & ROUND(DVARP(...),2)
```
Shows sample size alongside variance.

## Official Documentation

- [Microsoft Excel DVARP Documentation](https://support.microsoft.com/en-us/office/dvarp-function-eb0e8929-2c3e-4d22-b76e-bc24bf8d6ead)
- [Google Sheets DVARP Documentation](https://support.google.com/docs/answer/3094089)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved numerical precision |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | VAR.P introduced as non-database alternative |
| Excel 365 | Excel | Continued support |
| Initial release | Google Sheets | Full compatibility with Excel |
| 2018 | Google Sheets | Performance improvements |
