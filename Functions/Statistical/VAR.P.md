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
- population-variance
- varp
tags:
- statistical
- dispersion
- variability
- population
---

# VAR.P

## Description

The VAR.P function calculates the variance of an entire population. Unlike VAR.S (sample variance), which uses n-1 in its denominator to provide an unbiased estimate, VAR.P divides by n (the total count of values) because you are measuring the actual variability of the complete population rather than estimating it from a sample. This distinction is fundamental to accurate statistical analysis, and selecting the correct function directly impacts the validity of your conclusions.

Population variance quantifies how spread out values are from the mean by computing the average of the squared deviations from the mean. When your dataset encompasses every member of the group you are analyzing, VAR.P provides the true variance without any adjustment for sampling uncertainty. Common scenarios include calculating variance for all employees' salaries in a company, all students' test scores in a specific class, or all transactions during a complete fiscal period.

Variance and standard deviation share a direct mathematical relationship: standard deviation equals the square root of variance, and variance equals standard deviation squared. While standard deviation uses the same units as the original data, variance uses squared units, making it less intuitive for direct interpretation but essential for many statistical procedures. Variance is additive for independent random variables, a property that underpins portfolio theory, analysis of variance (ANOVA), and many regression techniques.

VAR.P was introduced in Excel 2010 as part of Microsoft's initiative to improve statistical function clarity, replacing the older VARP function. The ".P" suffix explicitly indicates population-based calculation. Both platforms support this function identically, ignoring text, logical values, and empty cells within ranges while processing only numeric values.

## Syntax

> [!info] Function Syntax
> ```
> =VAR.P(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range representing the entire population | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, names, arrays, or references containing numbers. Logical values and text representations of numbers typed directly as arguments are counted. Within arrays or references, only numeric values are processed; empty cells, logical values, text, and error values are ignored.

## Examples

### Example 1: Basic Population Variance
Calculate the variance for all employee performance scores.
```
=VAR.P(B2:B25)
```
**Result:** Returns the population variance of all 24 employee scores.

### Example 2: Explicit Values
Calculate population variance of known values.
```
=VAR.P(85, 90, 78, 92, 88)
```
**Result:** 22.0 - the true variance of these five test scores.

### Example 3: Relationship to Standard Deviation
Demonstrate the connection between VAR.P and STDEV.P.
```
=VAR.P(A1:A10)          → 100
=STDEV.P(A1:A10)        → 10
=STDEV.P(A1:A10)^2      → 100  (equals VAR.P)
=SQRT(VAR.P(A1:A10))    → 10   (equals STDEV.P)
```
**Result:** Population variance is the square of population standard deviation.

### Example 4: Comparing Sample vs Population
Demonstrating the difference between VAR.S and VAR.P.
```
Sample (n-1):     =VAR.S(A1:A10) → 111.11
Population (n):   =VAR.P(A1:A10) → 100.00
```
**Result:** Sample variance is approximately 11% higher for 10 data points.

### Example 5: Complete Inventory Analysis
Measure variation in stock levels across all locations.
```
=VAR.P(Inventory!C:C)
```
**Result:** Population variance of inventory levels across all warehouse locations.

### Example 6: All Student Grades in a Class
Analyze grade distribution for a complete roster.
```
=VAR.P(Gradebook!D2:D35)
```
**Result:** The actual variance of all 34 student grades in the class.

### Example 7: Multiple Ranges from Complete Dataset
Combine all product categories.
```
=VAR.P(Electronics!B:B, Clothing!B:B, Food!B:B)
```
**Result:** Single population variance across all product category sales.

### Example 8: Named Range for All Transactions
Using defined names for complete datasets.
```
=VAR.P(AllQuarterlyTransactions)
```
**Result:** Population variance of every transaction in the quarter.

### Example 9: Array Calculation with Filter
Calculate population variance for specific subset.
```
=VAR.P(FILTER(B2:B1000, A2:A1000="Approved"))
```
**Result:** Population variance for all approved records.

### Example 10: Pooled Variance Calculation
Combine variances from multiple complete populations.
```
=((n1-1)*VAR.P(Group1)+(n2-1)*VAR.P(Group2))/(n1+n2-2)
```
**Result:** Pooled variance estimate for comparing two populations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | No numeric values in the range (n=0) | Ensure at least 1 numeric data point exists |
| #VALUE! | Non-numeric text entered directly as argument | Use cell references or ensure arguments are numeric |
| #NAME? | Function name misspelled or using old VARP name | Use VAR.P with the period; verify Excel version |
| #NUM! | Calculation overflow with extremely large numbers | Check for data entry errors; consider scaling data |
| Zero result | All values are identical | Correct behavior - no variation exists |
| Unexpected result | Using population formula when sample formula is needed | Verify data represents complete population |

## Use Cases

### 1. Complete Workforce Analysis
**Scenario:** An HR department analyzes salary variance across all 500 employees to understand pay equity and compensation structure.

**Implementation:** Since the analysis covers every employee (complete population), VAR.P provides the actual variance without sample adjustment.

**Formula:**
```
=VAR.P(Salaries!C2:C501)
```

**Analysis:** High population variance indicates significant salary disparity, potentially signaling issues with pay equity. The HR team can segment by department or tenure to identify where variance is concentrated. Using VAR.P is correct because this encompasses all employees, not a sample.

### 2. Academic Grade Distribution
**Scenario:** A professor analyzes the final exam scores for all students in a course to evaluate test design and grading fairness.

**Implementation:** The complete class represents the full population for this analysis, making population variance appropriate.

**Formula:**
```
=VAR.P(FinalExam!B2:B85)
```

**Analysis:** A variance that's too low might indicate a test that doesn't differentiate student abilities, while very high variance could suggest inconsistent instruction or an unfair exam. Comparing variance across multiple sections taught by different instructors reveals teaching consistency.

### 3. Machine Output Consistency
**Scenario:** A plant manager compares variance in output across all 8 production machines to identify equipment requiring maintenance or replacement.

**Implementation:** Annual output data for each machine represents that machine's complete production population.

**Formula:**
```
Machine 1: =VAR.P(M1_Output)
Machine 2: =VAR.P(M2_Output)
...
```

**Analysis:** Machines with higher variance produce less consistent output. Ranking machines by variance helps prioritize maintenance schedules. A sudden increase in a machine's variance over time indicates developing problems before they cause failures.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Legacy alternative | VARP (still works) | VARP also available |
| Maximum arguments | 255 | 255 |
| Array formula support | Yes | Yes |
| Minimum data points | 1 (returns 0) | 1 (returns 0) |
| Text in range | Ignored | Ignored |
| Logical values in range | Ignored | Ignored |
| Direct logical arguments | TRUE=1, FALSE=0 | TRUE=1, FALSE=0 |
| Precision | 15 significant digits | 15 significant digits |

**Note:** VAR.P and VARP produce identical results. The ".P" naming was introduced in Excel 2010 for clarity.

## Tips and Best Practices

1. **Confirm population status:** Only use VAR.P when you have data for the entire population. If working with a sample, use VAR.S to avoid underestimating true population variance.

2. **Understand squared units:** Variance is expressed in squared units of the original data. For salary variance, the result is in "dollars squared," which is difficult to interpret. Use STDEV.P when you need results in original units.

3. **Use for statistical calculations:** Many statistical formulas require variance directly, including ANOVA, F-tests, and chi-square tests. Using VAR.P avoids the round-trip of squaring standard deviation.

4. **Consider relative measures:** For comparing variability across datasets with different scales, calculate coefficient of variation: STDEV.P/AVERAGE rather than using raw variance.

5. **Document your methodology:** Note in your analysis why the data represents a complete population. This prevents future analysts from incorrectly switching to sample variance.

6. **Be consistent:** If using population variance, ensure all related statistics (standard deviation, confidence intervals) also use population formulas.

## Related Functions

| Function | Description |
|----------|-------------|
| [[VAR.S]] | Calculates sample variance (uses n-1 denominator) |
| [[VAR]] | Legacy function identical to VAR.S |
| [[VARP]] | Legacy function identical to VAR.P |
| [[VARPA]] | Population variance including text and logical values |
| [[STDEV.P]] | Population standard deviation (square root of VAR.P) |
| [[STDEV.S]] | Sample standard deviation |
| [[COVARIANCE.P]] | Population covariance between two datasets |
| [[AVERAGE]] | Arithmetic mean, used in variance calculation |

## Official Documentation

- [Microsoft Excel VAR.P function](https://support.microsoft.com/en-us/office/var-p-function-73d1285c-108c-4843-ba5d-a51f90656f3a)
- [Google Sheets VAR.P function](https://support.google.com/docs/answer/9413171)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | VAR.P introduced with improved naming convention |
| Excel 2013 | Continued support with performance improvements |
| Excel 2016 | Dynamic array compatibility added |
| Excel 2019/365 | Full integration with modern array features |
| Google Sheets | Supported since launch; identical behavior to Excel |

---

## Sample vs. Population Variance Explained

**Use VAR.P (Population) when:**
- You have data for every member of the defined group
- Examples: All employees in a company, all products in inventory
- You are describing the actual variation, not estimating it

**Use VAR.S (Sample) when:**
- Your data represents a subset of a larger population
- Examples: Survey responses from selected customers
- You need to estimate population variation from limited data

**Mathematical formulas:**
```
Population Variance: sum((x - mean)^2) / n
Sample Variance:     sum((x - mean)^2) / (n-1)
```

**Impact on results:**
For 10 data points, sample variance is 11% higher than population variance. For 100 data points, the difference is approximately 1%. The (n-1) denominator in sample variance (Bessel's correction) compensates for the tendency of samples to underestimate population variability.
