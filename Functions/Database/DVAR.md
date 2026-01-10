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
- sample-variance
- statistical-analysis
- data-variability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-variance
- sample-variance-database
tags:
- database-functions
- statistics
- variance
- criteria-based
---

# DVAR

## Description

DVAR calculates the sample variance of values in a database column among records that match specified criteria. Variance measures how spread out values are from their mean, expressed as the average of squared deviations. DVAR treats the matching records as a sample from a larger population, using (n-1) as the denominator (Bessel's correction). This is appropriate when your data represents a sample and you want to estimate the variance of the larger population.

The **criteria range** enables DVAR to filter records before calculating variance. Create a separate area with column headers matching your database headers exactly, then place filter conditions below. Same-row criteria combine with AND logic (all must be true), while different-row criteria create OR logic (any can be true). This allows targeted variance analysis like "variance of sales for (January Electronics) OR (February Home products)."

The relationship between variance and standard deviation is straightforward: variance is the standard deviation squared, or equivalently, standard deviation is the square root of variance. DVAR is related to DSTDEV by this relationship. Variance is useful for statistical calculations where squared units are mathematically convenient, such as in analysis of variance (ANOVA) or when combining variances from multiple sources.

Database functions structure data with column headers in the first row and records below. The database range must include headers. The field parameter specifies which column to analyze, accepting a quoted header name, column number (starting at 1), or cell reference. DVAR requires at least two numeric values to calculate; with fewer, it returns #DIV/0!. Text and empty cells are ignored.

## Syntax

> [!NOTE] Syntax
> ```
> DVAR(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column for which to calculate sample variance, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
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
=DVAR(A1:D6, "Price", F1:F2)
```

**Result:** `400`

**Explanation:** Sample variance of Electronics prices (25, 35, 65). Mean is 41.67; variance is 400 (standard deviation squared: 20^2 = 400).

---

### Example 2: Relationship to Standard Deviation

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**DVAR Formula:**
```
=DVAR(A1:D6, "Price", F1:F2)
```

**DSTDEV Formula:**
```
=DSTDEV(A1:D6, "Price", F1:F2)
```

**DVAR Result:** `400`
**DSTDEV Result:** `20`

**Explanation:** SQRT(400) = 20. Variance and standard deviation are mathematically related.

---

### Example 3: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DVAR(A1:D6, 3, F1:F2)
```

**Result:** `50`

**Explanation:** Column 3 is Price. Sample variance of Home prices (45, 55). Mean is 50; variance is 50.

---

### Example 4: Multiple AND Criteria

**Database (A1:E7):**
| Employee | Department | Region | Salary | Years |
|----------|------------|--------|--------|-------|
| Alice | Sales | North | 55000 | 3 |
| Bob | Sales | North | 62000 | 5 |
| Carol | Sales | South | 58000 | 4 |
| David | Sales | North | 68000 | 7 |
| Eve | IT | North | 72000 | 6 |
| Frank | Sales | South | 54000 | 2 |

**Criteria (G1:H2):**
| Department | Region |
|------------|--------|
| Sales | North |

**Formula:**
```
=DVAR(A1:E7, "Salary", G1:H2)
```

**Result:** `42333333`

**Explanation:** Sample variance of salaries for Sales employees in North region (55000, 62000, 68000). Variance is approximately 42,333,333.

---

### Example 5: OR Criteria

**Database (A1:E7):** Same as Example 4

**Criteria (G1:G3):**
| Department |
|------------|
| Sales |
| IT |

**Formula:**
```
=DVAR(A1:E7, "Salary", G1:G3)
```

**Result:** `46206667`

**Explanation:** Sample variance of salaries for Sales OR IT employees (all employees in database).

---

### Example 6: Quality Measurements

**Database (A1:D9):**
| Sample | Batch | Measurement | Inspector |
|--------|-------|-------------|-----------|
| 1 | A | 10.02 | Jones |
| 2 | A | 9.98 | Jones |
| 3 | A | 10.05 | Smith |
| 4 | A | 9.95 | Smith |
| 5 | B | 10.12 | Jones |
| 6 | B | 9.88 | Jones |
| 7 | B | 10.08 | Smith |
| 8 | B | 9.92 | Smith |

**Criteria (F1:F2):**
| Batch |
|-------|
| A |

**Formula:**
```
=DVAR(A1:D9, "Measurement", F1:F2)
```

**Result:** `0.00187`

**Explanation:** Sample variance of Batch A measurements (10.02, 9.98, 10.05, 9.95). Low variance indicates consistent quality.

---

### Example 7: Comparison Operators

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Price |
|-------|
| >=35 |

**Formula:**
```
=DVAR(A1:D6, "Rating", F1:F2)
```

**Result:** `0.167`

**Explanation:** Sample variance of Ratings for products priced >= $35 (4.5, 3.8, 4.1, 4.8). Variance is approximately 0.167.

---

### Example 8: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| *get* |

**Formula:**
```
=DVAR(A1:D6, "Price", F1:F2)
```

**Result:** `50`

**Explanation:** Sample variance of prices for products containing "get" (Gadget X: 45, Gadget Y: 55).

---

### Example 9: Insufficient Data

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Device Z |

**Formula:**
```
=DVAR(A1:D6, "Price", F1:F2)
```

**Result:** `#DIV/0!`

**Explanation:** Only one record matches. Sample variance requires at least 2 values.

---

### Example 10: Comparing Groups

**Database (A1:D8):**
| Team | Member | Score | Project |
|------|--------|-------|---------|
| Alpha | Alice | 92 | Web |
| Alpha | Bob | 88 | Web |
| Alpha | Carol | 95 | Mobile |
| Beta | David | 78 | Web |
| Beta | Eve | 85 | Mobile |
| Beta | Frank | 72 | Web |
| Beta | Grace | 80 | Mobile |

**Alpha Criteria (F1:F2):**
| Team |
|------|
| Alpha |

**Beta Criteria (H1:H2):**
| Team |
|------|
| Beta |

**Alpha Formula:**
```
=DVAR(A1:D8, "Score", F1:F2)
```

**Beta Formula:**
```
=DVAR(A1:D8, "Score", H1:H2)
```

**Alpha Result:** `12.33`
**Beta Result:** `28.92`

**Explanation:** Team Beta has higher variance (28.92 vs 12.33), indicating less consistent performance compared to Team Alpha.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | Fewer than 2 numeric values match criteria | Ensure at least 2 records match; verify with DCOUNT |
| #VALUE! | Field parameter does not match any column header | Check field name spelling and spacing |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Score" not Score |
| #NUM! | Column number outside valid range | Use number between 1 and total columns |
| 0 (unexpected) | All matching values are identical | Identical values have zero variance |
| Wrong result | Criteria headers do not match database headers | Copy headers from database |
| #REF! | Invalid range reference | Verify all ranges exist |

## Use Cases

### Statistical Process Control

**Scenario:** A quality engineer needs to monitor process variance across different production lines, shifts, and time periods to ensure manufacturing consistency.

**Implementation:** Create a measurements database with Sample ID, Line, Shift, Date, and Measurement columns. DVAR calculates variance for specific combinations of factors.

**Technical Details:** Calculate `=DVAR(QualityData, "Measurement", criteria)` for each production line. Compare variances: Line A with variance 0.0025 vs Line B with variance 0.0089 indicates Line B has more variability. Use variance for control chart calculations. Track variance trends over time to detect process drift.

---

### Financial Risk Analysis

**Scenario:** A portfolio manager needs to assess the variance of returns for different investment categories to quantify risk and optimize asset allocation.

**Implementation:** Build a returns database with Date, Investment, Category, Sector, and Return columns. DVAR calculates return variance for filtered investment groups.

**Technical Details:** Variance of returns is a key risk measure. `=DVAR(Returns, "Return", criteria)` calculates variance for specific categories. Higher variance means higher risk. Use sample variance because historical returns represent a sample of possible future outcomes. Portfolio variance calculations use individual asset variances as inputs.

---

### Research Data Analysis

**Scenario:** A researcher needs to calculate variance in experimental results across different treatment groups, time points, and conditions.

**Implementation:** Structure experimental data with Subject ID, Treatment, Time Point, Condition, and Measurement columns. DVAR enables analysis of variance components.

**Technical Details:** Variance is fundamental to statistical tests. `=DVAR(Experiment, "Measurement", TreatmentCriteria)` gives within-group variance. Compare treatment variances to assess whether treatments affect variability, not just averages. Results inform statistical power calculations for future studies.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Calculation method | Sample variance (n-1) | Sample variance (n-1) |
| Minimum values required | 2 | 2 |
| Result with <2 values | #DIV/0! | #DIV/0! |
| Wildcard support | Yes | Yes |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Structured table references | Supported | Not supported |
| Named ranges | Fully supported | Fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Maximum rows | 1,048,576 | ~10,000,000 cells |

**Key Difference:** Both platforms calculate sample variance identically using Bessel's correction (n-1 denominator).

## Tips and Best Practices

1. **Choose sample vs population correctly**: Use DVAR when data is a sample from a larger population. Use DVARP when data represents the complete population. The difference is significant for small datasets.

2. **Understand variance units**: Variance is in squared units of the original data. A salary variance of 1,000,000 means salaries vary significantly, but the unit is "dollars squared." Take the square root for standard deviation in original units.

3. **Ensure sufficient data**: DVAR requires at least 2 values. Use DCOUNT to verify: `=IF(DCOUNT(...)>=2, DVAR(...), "Need more data")`.

4. **Use for ANOVA calculations**: When performing Analysis of Variance, DVAR helps calculate within-group variances. Compare variances across groups to test for treatment effects.

5. **Compare variances carefully**: To test if two variances differ significantly, use an F-test rather than just comparing the values. Large samples reduce the uncertainty in variance estimates.

6. **Consider coefficient of variation**: For comparing variability across groups with different means, divide standard deviation by mean: `=SQRT(DVAR(...)) / DAVERAGE(...)` gives a dimensionless ratio.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[VAR]] / [[VAR.S]] | Sample variance of range | No filtering capability |
| [[DVARP]] | Population variance with criteria | Uses n denominator |
| [[DSTDEV]] | Sample standard deviation with criteria | Returns square root of variance |
| [[DSTDEVP]] | Population standard deviation with criteria | SD with n denominator |
| [[DAVERAGE]] | Average matching criteria | Returns mean, not spread |
| [[DCOUNT]] | Count matching criteria | Verifies sample size |

### Commonly Used With

**[[DSTDEV]]** - Convert to standard deviation:
```
Variance: =DVAR(database, "Value", criteria)
Std Dev: =SQRT(DVAR(database, "Value", criteria))
Or directly: =DSTDEV(database, "Value", criteria)
```
Standard deviation is often easier to interpret.

**[[DAVERAGE]]** - Report mean with variance:
```
Mean: =DAVERAGE(database, "Score", criteria)
Variance: =DVAR(database, "Score", criteria)
```
Complete descriptive statistics.

**[[DCOUNT]]** - Verify sample size:
```
=IF(DCOUNT(database, "Score", criteria)>=2, DVAR(database, "Score", criteria), "Insufficient data")
```
Prevents #DIV/0! errors.

## Official Documentation

- [Microsoft Excel DVAR Documentation](https://support.microsoft.com/en-us/office/dvar-function-d6747ca9-99c7-48bb-996e-9d7af00f3ed1)
- [Google Sheets DVAR Documentation](https://support.google.com/docs/answer/3094088)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved numerical precision |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | VAR.S introduced as non-database alternative |
| Excel 365 | Excel | Continued support |
| Initial release | Google Sheets | Full compatibility with Excel |
| 2018 | Google Sheets | Performance improvements |
