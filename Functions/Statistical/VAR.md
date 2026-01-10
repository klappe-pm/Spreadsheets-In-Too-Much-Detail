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
- sample-statistics
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- variance
- sample-variance
tags:
- statistical
- dispersion
- variability
- sample
- legacy
---

# VAR

## Description

The VAR function calculates the variance of a dataset based on a sample of the population. Variance is a fundamental measure of dispersion that quantifies how spread out values are from the arithmetic mean by computing the average of the squared deviations. As a sample-based function, VAR uses (n-1) in its denominator (Bessel's correction) to provide an unbiased estimate of the true population variance when working with sample data rather than a complete population.

Variance and standard deviation are closely related: standard deviation is simply the square root of variance. While standard deviation is often preferred for interpretation because it uses the same units as the original data, variance has important mathematical properties that make it essential for statistical analysis. Variance is additive for independent random variables, making it crucial for portfolio theory, analysis of variance (ANOVA), and regression analysis. Many statistical tests and formulas work directly with variance rather than standard deviation.

The VAR function is particularly useful in financial analysis for calculating portfolio risk, in quality control for measuring process variation, and in scientific research for analyzing experimental data. A low variance indicates that data points cluster tightly around the mean, while a high variance signals that values are spread across a wider range. Since variance uses squared units, comparing it directly to the original data scale requires taking the square root to obtain standard deviation.

This function is a legacy function maintained for backward compatibility. Microsoft recommends using VAR.S for new projects, as it provides identical functionality with clearer naming that explicitly indicates sample-based calculation. VAR ignores text values, logical values, and empty cells within the reference range, processing only numeric values in its calculation.

## Syntax

> [!info] Function Syntax
> ```
> =VAR(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range containing sample data | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, names, arrays, or references containing numbers. Logical values and text representations of numbers typed directly into the argument list are counted. If an argument is an array or reference, only numbers in that array or reference are counted; empty cells, logical values, text, and error values are ignored.

## Examples

### Example 1: Basic Variance of a Range
Calculate the variance of sales figures.
```
=VAR(B2:B13)
```
**Result:** Returns the sample variance of monthly sales values in B2:B13.

### Example 2: Variance of Individual Values
Calculate variance of explicitly listed values.
```
=VAR(10, 15, 20, 25, 30)
```
**Result:** 62.5 - the sample variance of these five values around their mean of 20.

### Example 3: Relationship to Standard Deviation
Demonstrate the connection between VAR and STDEV.
```
=VAR(A1:A10)        → 100
=STDEV(A1:A10)      → 10
=STDEV(A1:A10)^2    → 100  (equals VAR)
=SQRT(VAR(A1:A10))  → 10   (equals STDEV)
```
**Result:** Variance is the square of standard deviation.

### Example 4: Multiple Range Arguments
Combine data from multiple ranges.
```
=VAR(A2:A10, C2:C10, E2:E10)
```
**Result:** Calculates a single variance across all three non-contiguous ranges.

### Example 5: Portfolio Risk Calculation
Calculate variance of investment returns.
```
=VAR(Returns!C2:C25)
```
**Result:** Returns the sample variance of 24 monthly returns for risk analysis.

### Example 6: Named Range Usage
Using a defined name for cleaner formulas.
```
=VAR(SampleData)
```
**Result:** Calculates variance using the named range "SampleData".

### Example 7: Comparing Sample vs Population Variance
Demonstrating the difference.
```
Sample (n-1):     =VAR(A1:A10)    → 111.11
Population (n):   =VAR.P(A1:A10)  → 100.00
```
**Result:** Sample variance is higher due to Bessel's correction.

### Example 8: Quality Control - Process Variation
Measure manufacturing process stability.
```
=VAR(Measurements!B2:B51)
```
**Result:** Sample variance of 50 measurements indicates process consistency.

### Example 9: Mixed Data Types
Variance with text and numbers mixed.
```
=VAR(A1:A10)
```
Where A1:A10 contains: 5, 10, "N/A", 15, "", 20, TRUE, 25, 30, 35
**Result:** 116.67 (approximately) - only numeric values are included.

### Example 10: ANOVA Preparation
Calculate within-group variance for analysis.
```
Group A: =VAR(GroupA_Values)
Group B: =VAR(GroupB_Values)
```
**Result:** Individual group variances for analysis of variance calculations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | Fewer than 2 numeric values provided | Ensure at least 2 numeric data points exist in the range |
| #VALUE! | Text values entered directly as arguments | Use cell references or convert text to numbers |
| #NAME? | Function name misspelled or not recognized | Verify spelling; check if using correct Excel version |
| #NUM! | Calculation overflow with extremely large values | Check data for unreasonable values; consider scaling data |
| Unexpected result | Empty cells or text being ignored | Verify data range contains only intended numeric values |
| Zero result | All values are identical | This is correct behavior - no variation exists in the data |

## Use Cases

### 1. Portfolio Variance and Diversification
**Scenario:** An investment analyst calculates portfolio variance to assess overall risk and evaluate the benefits of diversification across multiple assets.

**Implementation:** Portfolio variance considers not just individual asset variances but also the covariances between assets, making VAR essential for modern portfolio theory.

**Formula:**
```
Asset Variance: =VAR(AssetReturns)
Combined: Portfolio variance = w1^2*VAR1 + w2^2*VAR2 + 2*w1*w2*COV12
```

**Analysis:** A well-diversified portfolio has lower variance than the weighted average of individual asset variances due to imperfect correlation between assets. Comparing the sum of weighted individual variances to actual portfolio variance quantifies the diversification benefit.

### 2. Analysis of Variance (ANOVA) Preparation
**Scenario:** A researcher compares test scores across three teaching methods to determine if the differences are statistically significant.

**Implementation:** Calculate within-group variance for each teaching method as input to F-test calculations.

**Formula:**
```
=VAR(Method1_Scores)
=VAR(Method2_Scores)
=VAR(Method3_Scores)
```

**Analysis:** ANOVA compares between-group variance to within-group variance. If teaching method significantly affects scores, between-group variance will be large relative to within-group variance. Individual group variances help assess homogeneity of variance assumption required for valid ANOVA.

### 3. Process Capability Analysis
**Scenario:** A manufacturing engineer evaluates whether a production process can consistently meet specifications by analyzing variance in product dimensions.

**Implementation:** Calculate sample variance from quality samples to estimate process capability indices.

**Formula:**
```
=VAR(SampleMeasurements)
Cp = (USL - LSL) / (6 * SQRT(VAR(SampleMeasurements)))
```

**Analysis:** Lower variance indicates a more capable process. The process capability index (Cp) uses standard deviation (square root of variance) to determine how many specification widths fit within the process spread. A Cp greater than 1.33 typically indicates adequate capability.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | All versions (legacy since Excel 2010) | Fully supported |
| Maximum arguments | 255 | 255 |
| Array formula support | Yes (CSE or dynamic) | Yes (native) |
| Handles text in range | Ignores | Ignores |
| Handles logical values in range | Ignores | Ignores |
| Direct logical arguments | Counts TRUE=1, FALSE=0 | Counts TRUE=1, FALSE=0 |
| Empty cell handling | Ignored | Ignored |
| Precision | 15 significant digits | 15 significant digits |
| Recommended replacement | VAR.S | VAR.S (also available) |

**Note:** Both platforms treat this function identically. Excel marks VAR as legacy, recommending VAR.S for new work.

## Tips and Best Practices

1. **Use VAR.S for new projects:** While VAR works perfectly, VAR.S provides clearer intent and is the recommended modern function. Both produce identical results for sample variance.

2. **Consider standard deviation for interpretation:** Since variance uses squared units, it can be difficult to interpret directly. Take the square root (or use STDEV) when you need results in the original measurement units.

3. **Verify sample vs. population:** Before calculating, determine whether your data represents a complete population (use VAR.P) or a sample from a larger population (use VAR/VAR.S). Using the wrong function introduces systematic bias.

4. **Understand the mathematical properties:** Variance is additive for independent variables. The variance of a sum equals the sum of variances (plus covariance terms for correlated variables). This property is essential for risk aggregation.

5. **Watch for outliers:** Variance is extremely sensitive to outliers because deviations are squared. A single extreme value can dramatically inflate the result. Consider using robust alternatives for datasets with outliers.

6. **Check sample size adequacy:** Variance estimates are unstable with small samples. For reliable estimates, use at least 30 observations when possible.

## Related Functions

| Function | Description |
|----------|-------------|
| [[VAR.S]] | Modern replacement for VAR; calculates sample variance (identical results) |
| [[VAR.P]] | Calculates population variance (divides by n, not n-1) |
| [[VARA]] | Calculates sample variance including text and logical values |
| [[VARPA]] | Calculates population variance including text and logical values |
| [[STDEV]] | Calculates sample standard deviation (square root of variance) |
| [[STDEV.S]] | Modern sample standard deviation function |
| [[COVAR]] | Calculates covariance between two datasets |
| [[AVERAGE]] | Calculates arithmetic mean, central to variance calculation |

## Official Documentation

- [Microsoft Excel VAR function](https://support.microsoft.com/en-us/office/var-function-73b47e82-cbca-483d-a99f-0b6b3815f7f4)
- [Google Sheets VAR function](https://support.google.com/docs/answer/3094063)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available in initial modern Excel releases |
| Excel 2007 | Increased argument limit from 30 to 255 |
| Excel 2010 | Marked as legacy; VAR.S introduced as replacement |
| Excel 2019/365 | Continued support for backward compatibility |
| Google Sheets | Supported since launch with identical behavior to Excel |

---

## Sample vs. Population Variance

**Population Variance (VAR.P):**
- Formula: sum((x - mean)^2) / n
- Use when: You have measured the entire population of interest
- The divisor is n (the count of values)

**Sample Variance (VAR/VAR.S):**
- Formula: sum((x - mean)^2) / (n-1)
- Use when: You have a sample representing a larger population
- The divisor is (n-1), known as Bessel's correction

**Why variance matters:**
Variance captures the average squared distance from the mean. While less intuitive than standard deviation, variance has mathematical properties that make it fundamental to statistical theory:
- Variances are additive for independent variables
- Many statistical tests (F-test, ANOVA) are based on variance ratios
- Portfolio risk calculations use variance and covariance
- Linear regression minimizes the variance of residuals
