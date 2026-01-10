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
- sample-standard-deviation
- statistical-analysis
- data-variability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-stdev
- sample-standard-deviation-database
tags:
- database-functions
- statistics
- standard-deviation
- criteria-based
---

# DSTDEV

## Description

DSTDEV calculates the sample standard deviation of values in a database column among records that match specified criteria. Standard deviation measures how spread out values are from their average. DSTDEV treats the matching records as a sample from a larger population, using the (n-1) denominator in its calculation. This is the appropriate choice when your data represents a sample rather than the complete population.

The **criteria range** enables DSTDEV to filter records before calculating variability. Create a separate area with column headers matching your database headers exactly. Place filter conditions below these headers. Same-row criteria combine with AND logic, while different-row criteria create OR logic. This allows you to measure variability in specific subsets, such as "standard deviation of sales for (Electronics in Q1) OR (Home products in Q2)."

Choosing between DSTDEV (sample) and DSTDEVP (population) is statistically important. Use DSTDEV when your data is a sample from a larger group you want to make inferences about. For example, if you have sales data from 50 randomly selected stores and want to estimate variability across all 500 stores, use DSTDEV. If your data includes the entire population (all 500 stores), use DSTDEVP. DSTDEV produces slightly larger values because it compensates for sample uncertainty.

Database functions structure data with column headers in the first row and records below. The database range must include headers. The field parameter specifies which column to analyze, accepting a quoted header name, column number (starting at 1), or cell reference. DSTDEV requires at least two numeric values to calculate; with fewer, it returns #DIV/0!. Text and empty cells are ignored.

## Syntax

> [!NOTE] Syntax
> ```
> DSTDEV(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column for which to calculate sample standard deviation, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
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
=DSTDEV(A1:D6, "Price", F1:F2)
```

**Result:** `20`

**Explanation:** Calculates sample standard deviation of Electronics prices (25, 35, 65). Mean is 41.67, and the sample standard deviation is 20.

---

### Example 2: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Home |

**Formula:**
```
=DSTDEV(A1:D6, 3, F1:F2)
```

**Result:** `7.07`

**Explanation:** Column 3 is Price. Sample standard deviation of Home prices (45, 55) is approximately 7.07.

---

### Example 3: Multiple AND Criteria

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
=DSTDEV(A1:E7, "Salary", G1:H2)
```

**Result:** `6506.41`

**Explanation:** Calculates sample standard deviation of salaries for Sales employees in North region: Alice ($55,000), Bob ($62,000), David ($68,000). Standard deviation is approximately $6,506.41.

---

### Example 4: OR Criteria

**Database (A1:E7):** Same as Example 3

**Criteria (G1:G3):**
| Department |
|------------|
| Sales |
| IT |

**Formula:**
```
=DSTDEV(A1:E7, "Salary", G1:G3)
```

**Result:** `6797.06`

**Explanation:** Sample standard deviation of salaries for Sales OR IT employees (all employees). With salaries of 55000, 62000, 58000, 68000, 72000, 54000.

---

### Example 5: Comparison Operators

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Price |
|-------|
| >=35 |

**Formula:**
```
=DSTDEV(A1:D6, "Rating", F1:F2)
```

**Result:** `0.50`

**Explanation:** Sample standard deviation of Ratings for products priced >= $35 (Widget B: 4.5, Gadget X: 3.8, Gadget Y: 4.1, Device Z: 4.8). Approximately 0.50.

---

### Example 6: Analyzing Variability by Group

**Database (A1:D8):**
| Month | Store | Sales | Returns |
|-------|-------|-------|---------|
| Jan | A | 45000 | 1200 |
| Jan | B | 52000 | 1800 |
| Jan | C | 38000 | 900 |
| Feb | A | 48000 | 1100 |
| Feb | B | 55000 | 2000 |
| Feb | C | 41000 | 950 |
| Mar | A | 51000 | 1300 |

**Criteria (F1:F2):**
| Store |
|-------|
| A |

**Formula:**
```
=DSTDEV(A1:D8, "Sales", F1:F2)
```

**Result:** `3000`

**Explanation:** Sample standard deviation of Store A sales (45000, 48000, 51000). Shows how much Store A's monthly sales vary.

---

### Example 7: Quality Control Measurements

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
=DSTDEV(A1:D9, "Measurement", F1:F2)
```

**Result:** `0.0435`

**Explanation:** Sample standard deviation of Batch A measurements (10.02, 9.98, 10.05, 9.95). Low deviation indicates consistent quality.

---

### Example 8: Wildcard Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Widget* |

**Formula:**
```
=DSTDEV(A1:D6, "Price", F1:F2)
```

**Result:** `7.07`

**Explanation:** Sample standard deviation of prices for products starting with "Widget" (Widget A: $25, Widget B: $35).

---

### Example 9: Insufficient Data Handling

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Device Z |

**Formula:**
```
=DSTDEV(A1:D6, "Price", F1:F2)
```

**Result:** `#DIV/0!`

**Explanation:** Only one record matches (Device Z). Sample standard deviation requires at least 2 values. Single values cannot have meaningful variability.

---

### Example 10: Comparing Variability Across Groups

**Database (A1:E8):**
| Product | Region | Q1 | Q2 | Q3 |
|---------|--------|-----|-----|-----|
| Widget | East | 100 | 120 | 115 |
| Widget | West | 95 | 105 | 98 |
| Gadget | East | 80 | 95 | 88 |
| Gadget | West | 75 | 82 | 79 |
| Device | East | 150 | 180 | 165 |
| Device | West | 140 | 155 | 148 |
| Tool | East | 60 | 70 | 65 |

**East Criteria (G1:G2):**
| Region |
|--------|
| East |

**Formula:**
```
=DSTDEV(A1:E8, "Q1", G1:G2)
```

**Result:** `37.42`

**Explanation:** Sample standard deviation of Q1 sales for East region products (100, 80, 150, 60). Shows high variability in East regional performance.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | Fewer than 2 numeric values match criteria | Ensure at least 2 records match; use DCOUNT to verify |
| #VALUE! | Field parameter does not match any column header | Verify field name matches exactly |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Salary" not Salary |
| #NUM! | Column number outside valid range | Use number between 1 and total columns |
| 0 (unexpected) | All matching values are identical | Identical values have zero deviation |
| Wrong result | Criteria headers do not match database headers | Copy headers from database |
| #REF! | Invalid range reference | Verify all ranges exist |

## Use Cases

### Quality Control Process Monitoring

**Scenario:** A manufacturing quality team needs to monitor process variability by measuring the standard deviation of product dimensions across different production lines, shifts, and material batches.

**Implementation:** Create a measurements database with Sample ID, Line, Shift, Batch, and Dimension columns. Use DSTDEV to calculate variability for specific combinations of factors.

**Technical Details:** Calculate `=DSTDEV(Measurements, "Dimension", criteria)` for each production line. Compare results: Line A with deviation 0.05mm vs Line B with 0.12mm indicates Line B has more variability. Use criteria to isolate "Day Shift AND Batch X" to identify specific sources of variation. Results feed control charts and trigger investigations when deviation exceeds thresholds.

---

### Investment Portfolio Risk Analysis

**Scenario:** A financial analyst needs to measure the volatility of returns across different asset classes, sectors, and time periods to assess portfolio risk.

**Implementation:** Build a returns database with Date, Asset, Class, Sector, and Return columns. DSTDEV calculates return volatility for filtered subsets.

**Technical Details:** Standard deviation of returns measures investment risk. `=DSTDEV(Returns, "Return", criteria)` with criteria for "Class = Equity AND Sector = Technology" shows tech stock volatility. Compare across sectors: higher deviation means higher risk. Use sample standard deviation because historical returns are a sample of possible future outcomes.

---

### Employee Performance Consistency

**Scenario:** An HR team needs to identify departments or roles with high performance variability to target training and standardization efforts.

**Implementation:** Structure performance data with Employee, Department, Role, Period, and Score columns. DSTDEV reveals how consistently employees in each group perform.

**Technical Details:** Low standard deviation indicates consistent performance across the group. High deviation suggests some employees significantly outperform or underperform peers. Formula `=DSTDEV(Performance, "Score", DeptCriteria)` per department identifies where variability is highest. These departments may benefit from better training, clearer processes, or performance management attention.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Calculation method | Sample standard deviation (n-1) | Sample standard deviation (n-1) |
| Minimum values required | 2 | 2 |
| Result with <2 values | #DIV/0! | #DIV/0! |
| Wildcard support | Yes | Yes |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Structured table references | Supported | Not supported |
| Named ranges | Fully supported | Fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Maximum rows | 1,048,576 | ~10,000,000 cells |

**Key Difference:** Both platforms use identical formulas for sample standard deviation. Results should match precisely for the same data.

## Tips and Best Practices

1. **Choose sample vs population appropriately**: Use DSTDEV when your data is a sample from a larger population. Use DSTDEVP when you have the complete population. The difference matters more with small datasets.

2. **Ensure sufficient data**: DSTDEV requires at least 2 values. Use DCOUNT first to verify: `=IF(DCOUNT(...)>=2, DSTDEV(...), "Insufficient data")`.

3. **Interpret with context**: A "high" or "low" standard deviation depends on context. $500 deviation is small for salaries but large for hourly wages. Compare deviations relative to the mean using coefficient of variation: `=DSTDEV(...)/DAVERAGE(...)`.

4. **Use for quality control**: In manufacturing, standard deviation indicates process consistency. Track DSTDEV over time; increasing values may signal process drift requiring attention.

5. **Combine with DAVERAGE**: Standard deviation alone is less meaningful. Pair with average: "Average sale is $100 with standard deviation $15" tells a complete story.

6. **Watch for outliers**: Standard deviation is sensitive to extreme values. One outlier can dramatically inflate the result. Consider identifying outliers before analysis.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[STDEV]] / [[STDEV.S]] | Sample standard deviation of range | No filtering capability |
| [[DSTDEVP]] | Population standard deviation with criteria | Uses n denominator instead of n-1 |
| [[DVAR]] | Sample variance with criteria | Returns variance (deviation squared) |
| [[DVARP]] | Population variance with criteria | Variance with n denominator |
| [[DAVERAGE]] | Average matching criteria | Returns mean, not spread |
| [[DCOUNT]] | Count matching criteria | Helps verify sample size |

### Commonly Used With

**[[DAVERAGE]]** - Report mean with standard deviation:
```
Average: =DAVERAGE(database, "Score", criteria)
Std Dev: =DSTDEV(database, "Score", criteria)
Report: =DAVERAGE(...) & " (" & ROUND(DSTDEV(...),2) & ")"
```
Standard statistical reporting format.

**[[DCOUNT]]** - Verify sample size before calculating:
```
=IF(DCOUNT(database, "Score", criteria)>=2, DSTDEV(database, "Score", criteria), "Need more data")
```
Prevents #DIV/0! errors.

**[[DSTDEVP]]** - Compare sample vs population deviation:
```
Sample: =DSTDEV(database, "Value", criteria)
Population: =DSTDEVP(database, "Value", criteria)
```
Understand the difference for your specific use case.

## Official Documentation

- [Microsoft Excel DSTDEV Documentation](https://support.microsoft.com/en-us/office/dstdev-function-026b8c73-616d-4b5e-b072-241871c4ab96)
- [Google Sheets DSTDEV Documentation](https://support.google.com/docs/answer/3094086)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved numerical precision |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | STDEV.S introduced as non-database alternative |
| Excel 365 | Excel | Continued support |
| Initial release | Google Sheets | Full compatibility with Excel |
| 2018 | Google Sheets | Performance improvements |
