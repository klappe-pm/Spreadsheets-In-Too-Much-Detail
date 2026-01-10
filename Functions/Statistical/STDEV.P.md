---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- dispersion
- variability
- population-statistics
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- population-standard-deviation
- stdevp
tags:
- statistical
- dispersion
- variability
- population
---

# STDEV.P

## Description

The STDEV.P function calculates the standard deviation of an entire population. Unlike STDEV.S (sample standard deviation), which uses n-1 in its denominator to provide an unbiased estimate, STDEV.P divides by n (the total count of values) because you are measuring the actual variability of the complete population rather than estimating it from a sample. This distinction is fundamental to accurate statistical analysis and choosing the correct function impacts the validity of your conclusions.

Population standard deviation is appropriate when your dataset encompasses every member of the group you are analyzing. Common scenarios include calculating the variation in test scores for all students who took a specific exam, measuring the spread of all transactions in a fiscal quarter, or analyzing the performance of every machine in a factory. If you are working with a subset of data intended to represent a larger group, you should use STDEV.S instead to account for sampling variability.

The mathematical formula for population standard deviation takes the square root of the average squared deviation from the mean. Each value's distance from the mean is squared (eliminating negative values), these squared deviations are averaged, and the square root transforms the result back to the original unit of measurement. STDEV.P is less commonly used than STDEV.S because most real-world analysis involves samples rather than complete populations, but it is essential for census data, complete inventory analysis, and other exhaustive datasets.

STDEV.P was introduced in Excel 2010 as part of the consistency improvement initiative for statistical functions, replacing the older STDEVP function. The naming convention clearly indicates population-based calculation with the ".P" suffix. Both platforms support this function identically, ignoring text, logical values, and empty cells within ranges while processing only numeric values.

## Syntax

> [!info] Function Syntax
> ```
> =STDEV.P(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range representing the entire population | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, names, arrays, or references containing numbers. Logical values and text representations of numbers typed directly as arguments are counted. Within arrays or references, only numeric values are processed; empty cells, logical values, text, and error values are ignored.

## Examples

### Example 1: Basic Population Standard Deviation
Calculate the standard deviation for all employee salaries in a small company.
```
=STDEV.P(B2:B25)
```
**Result:** Returns the population standard deviation of all 24 employee salaries.

### Example 2: Explicit Values
Calculate population standard deviation of known values.
```
=STDEV.P(85, 90, 78, 92, 88)
```
**Result:** 4.69 (approximately) - the true standard deviation of these five test scores.

### Example 3: Complete Inventory Analysis
Measure variation in stock quantities across all warehouse locations.
```
=STDEV.P(Inventory!C:C)
```
**Result:** Population standard deviation of inventory levels across all locations.

### Example 4: Comparing Sample vs Population
Demonstrating the difference between STDEV.S and STDEV.P.
```
Sample (n-1): =STDEV.S(A1:A10)  → 10.54
Population (n): =STDEV.P(A1:A10) → 10.00
```
**Result:** For 10 values, the sample standard deviation is approximately 5% higher than population.

### Example 5: All Student Grades in a Class
Analyze grade distribution for a complete class roster.
```
=STDEV.P(Gradebook!D2:D35)
```
**Result:** The actual standard deviation of all 34 student grades in the class.

### Example 6: Multiple Ranges from Complete Dataset
Combine all product categories.
```
=STDEV.P(Electronics!B:B, Clothing!B:B, Food!B:B)
```
**Result:** Single population standard deviation across all product category sales.

### Example 7: Named Range for All Transactions
Using defined names for complete datasets.
```
=STDEV.P(AllQuarterlyTransactions)
```
**Result:** Population standard deviation of every transaction in the quarter.

### Example 8: Array Calculation with Filter
Calculate population SD for specific subset of complete data.
```
=STDEV.P(FILTER(B2:B1000, A2:A1000="Complete"))
```
**Result:** Population standard deviation for all records marked as "Complete".

### Example 9: Population Coefficient of Variation
Calculate relative variability for a complete population.
```
=STDEV.P(B2:B100)/AVERAGE(B2:B100)*100
```
**Result:** Population coefficient of variation as a percentage.

### Example 10: Z-Score Calculation
Calculate z-scores using population parameters.
```
=(A2-AVERAGE($A$2:$A$100))/STDEV.P($A$2:$A$100)
```
**Result:** The z-score for cell A2 based on population mean and standard deviation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | No numeric values in the range (n=0) | Ensure at least 1 numeric data point exists; unlike STDEV.S, STDEV.P can work with single value (returns 0) |
| #VALUE! | Non-numeric text entered directly as argument | Use cell references or ensure arguments are numeric |
| #NAME? | Function name misspelled or using old STDEVP name | Use STDEV.P with the period; verify Excel version supports it |
| #NUM! | Calculation overflow with extremely large numbers | Check for data entry errors; consider scaling data |
| Zero result | All values are identical | Correct behavior - no variation exists in identical values |
| Unexpected result | Using population formula when sample formula is appropriate | Verify your data represents complete population, not a sample |

## Use Cases

### 1. Census Data Analysis
**Scenario:** A city planning department analyzes property values for all 15,000 residential properties to understand neighborhood variation for tax assessment purposes.

**Implementation:** Since the data includes every property in the city (complete population), STDEV.P provides the actual standard deviation without needing to estimate from a sample.

**Formula:**
```
=STDEV.P(PropertyValues!B:B)
```

**Analysis:** The population standard deviation reveals true variation in property values. A high standard deviation indicates significant disparity between property values, suggesting the need for neighborhood-specific assessment approaches. Since this is census data covering all properties, using STDEV.S would overestimate the variation.

### 2. Complete Transaction Audit
**Scenario:** An auditor examines all transactions from the previous fiscal year to identify unusual patterns and assess process consistency.

**Implementation:** Because the audit covers every single transaction (not a sample), population standard deviation accurately describes the actual variation in transaction amounts.

**Formula:**
```
=STDEV.P(Transactions!E2:E50000)
```

**Analysis:** The auditor uses this value to establish thresholds for flagging outliers. Transactions beyond 3 population standard deviations from the mean warrant investigation. Using population statistics is appropriate because the analysis encompasses all transactions, not a sample.

### 3. Machine Performance Monitoring
**Scenario:** A manufacturing plant monitors all 12 production machines and needs to compare their output consistency using daily production data for the entire year.

**Implementation:** Each machine's annual production data represents a complete population of that machine's performance, making STDEV.P appropriate for comparing consistency.

**Formula:**
```
=STDEV.P(Machine1_Production)
```

**Analysis:** Machines with lower population standard deviations demonstrate more consistent output. Ranking machines by their standard deviation helps prioritize maintenance and identify which equipment may need calibration or replacement. The complete annual dataset justifies population statistics.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Legacy alternative | STDEVP (still works) | STDEVP also available |
| Maximum arguments | 255 | 255 |
| Array formula support | Yes | Yes |
| Minimum data points | 1 (returns 0) | 1 (returns 0) |
| Text in range | Ignored | Ignored |
| Logical values in range | Ignored | Ignored |
| Direct logical arguments | TRUE=1, FALSE=0 | TRUE=1, FALSE=0 |
| Precision | 15 significant digits | 15 significant digits |

**Note:** The ".P" naming convention was introduced in Excel 2010 for clarity. Both STDEV.P and the legacy STDEVP produce identical results.

## Tips and Best Practices

1. **Confirm population status:** Only use STDEV.P when you have measured the entire population. If your data is a sample from a larger group, use STDEV.S instead. This is the most common source of statistical error.

2. **Understand the impact:** For small datasets, the difference between sample and population standard deviation is significant. With 5 data points, STDEV.S is about 12% higher than STDEV.P. With 100 points, the difference is approximately 0.5%.

3. **Document your reasoning:** Include notes explaining why your data qualifies as a complete population. This helps reviewers understand your methodology and prevents incorrect modifications.

4. **Use for derived statistics:** When calculating z-scores, percentiles, or other statistics for a complete dataset, use STDEV.P to avoid overestimating variation.

5. **Consider the context:** Complete populations are more common in business contexts than scientific research. Annual sales, complete inventory counts, and all employees in a department are typical population scenarios.

6. **Avoid mixing methodologies:** If using population standard deviation, ensure all related calculations (variance, confidence intervals, etc.) also use population formulas for consistency.

## Related Functions

| Function | Description |
|----------|-------------|
| [[STDEV.S]] | Calculates sample standard deviation (uses n-1 denominator) |
| [[STDEV]] | Legacy function identical to STDEV.S |
| [[STDEVP]] | Legacy function identical to STDEV.P |
| [[STDEVPA]] | Population standard deviation including text (TRUE=1, FALSE=0) and logical values |
| [[VAR.P]] | Calculates population variance (standard deviation squared) |
| [[VAR.S]] | Calculates sample variance |
| [[AVERAGE]] | Arithmetic mean, typically used alongside standard deviation |
| [[STANDARDIZE]] | Converts values to z-scores using mean and standard deviation |

## Official Documentation

- [Microsoft Excel STDEV.P function](https://support.microsoft.com/en-us/office/stdev-p-function-6e917c05-31a0-496f-ade7-4f4e7462f285)
- [Google Sheets STDEV.P function](https://support.google.com/docs/answer/9413086)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | STDEV.P introduced with improved naming convention |
| Excel 2013 | Continued support with performance improvements |
| Excel 2016 | Dynamic array compatibility added |
| Excel 2019/365 | Full integration with modern array features |
| Google Sheets | Supported since launch; identical behavior to Excel |

---

## Sample vs. Population: Key Decision Points

**Use STDEV.P (Population) when:**
- You have data for every member of the defined group
- Examples: All employees in a company, all products in inventory, all transactions in a period
- You are describing the actual variation, not estimating it

**Use STDEV.S (Sample) when:**
- Your data represents a subset of a larger population
- Examples: Survey responses from selected customers, quality samples from production
- You need to estimate population variation from limited data

**Mathematical Impact:**
```
Population: sqrt(sum((x - mean)^2) / n)
Sample:     sqrt(sum((x - mean)^2) / (n-1))
```

The sample formula's (n-1) denominator, called Bessel's correction, compensates for the fact that samples tend to underestimate population variation because they are less likely to include extreme values.
