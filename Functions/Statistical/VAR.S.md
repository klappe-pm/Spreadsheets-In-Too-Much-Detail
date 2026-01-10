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
- sample-variance
- vars
tags:
- statistical
- dispersion
- variability
- sample
- modern
---

# VAR.S

## Description

The VAR.S function calculates the variance based on a sample of a population. This is the modern, clearly-named replacement for the legacy VAR function, introduced in Excel 2010 as part of Microsoft's initiative to improve statistical function clarity. The ".S" suffix explicitly indicates sample-based calculation, distinguishing it from VAR.P (population variance). Both VAR.S and VAR produce identical results, but Microsoft recommends VAR.S for new projects due to its unambiguous naming.

Sample variance uses Bessel's correction, dividing by (n-1) rather than n in its formula. This adjustment compensates for the inherent tendency of samples to underestimate population variability. When you collect data from a subset of a larger population, you are statistically less likely to capture extreme values, leading to an artificially compressed view of the true spread. The (n-1) denominator corrects this bias, providing an unbiased estimate of the population variance from sample data.

Variance quantifies how spread out values are from the mean by computing the average of squared deviations. While standard deviation is often preferred for interpretation because it shares units with the original data, variance has crucial mathematical properties for advanced statistical analysis. Variance is additive for independent random variables, meaning the variance of a sum equals the sum of variances (plus covariance terms). This property is fundamental to portfolio theory, analysis of variance (ANOVA), regression analysis, and many other statistical techniques.

VAR.S ignores text values, logical values (TRUE/FALSE), and empty cells within referenced ranges, counting only numeric values. However, if you type logical values or text representations of numbers directly as function arguments, they will be included in the calculation. This behavior mirrors other statistical functions and provides flexibility while protecting against common data quality issues.

## Syntax

> [!info] Function Syntax
> ```
> =VAR.S(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range containing sample data | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, named ranges, arrays, or cell references. Within references, only numeric values are counted; empty cells, logical values, text, and errors are ignored. Direct arguments typed as numbers or logical values are included.

## Examples

### Example 1: Basic Sample Variance
Calculate the sample variance of monthly sales.
```
=VAR.S(B2:B13)
```
**Result:** Returns the sample variance of 12 monthly sales values.

### Example 2: Explicit Numeric Values
Calculate variance of directly entered values.
```
=VAR.S(45, 52, 48, 55, 50)
```
**Result:** 14.5 - the sample variance of these five values.

### Example 3: Relationship to Standard Deviation
Demonstrate the connection between VAR.S and STDEV.S.
```
=VAR.S(A1:A10)           → 100
=STDEV.S(A1:A10)         → 10
=STDEV.S(A1:A10)^2       → 100  (equals VAR.S)
=SQRT(VAR.S(A1:A10))     → 10   (equals STDEV.S)
```
**Result:** Sample variance is the square of sample standard deviation.

### Example 4: Quality Control Sample
Measure variation in product weight samples.
```
=VAR.S(Samples!C2:C31)
```
**Result:** Sample variance of 30 weight measurements.

### Example 5: Multiple Disconnected Ranges
Combine samples from different sources.
```
=VAR.S(Q1_Sales, Q2_Sales, Q3_Sales, Q4_Sales)
```
**Result:** Single sample variance calculated across all quarterly named ranges.

### Example 6: Comparing with Population Variance
Demonstrating the difference in formulas.
```
Sample:     =VAR.S(A1:A5)  → 10.0
Population: =VAR.P(A1:A5)  → 8.0
```
**Result:** Sample variance is 25% higher for 5 data points due to (n-1) correction.

### Example 7: Financial Return Variance
Calculate variance for investment analysis.
```
=VAR.S(MonthlyReturns)*12
```
**Result:** Annualized variance from monthly return samples.

### Example 8: Filtered Data Variance
Calculate variance for subset of data.
```
=VAR.S(FILTER(B2:B500, A2:A500="Region A"))
```
**Result:** Sample variance for Region A data only.

### Example 9: F-Test Preparation
Calculate variances for comparing two samples.
```
Sample 1: =VAR.S(GroupA)
Sample 2: =VAR.S(GroupB)
F-ratio:  =VAR.S(GroupA)/VAR.S(GroupB)
```
**Result:** Variance ratio for F-test.

### Example 10: Pooled Variance Calculation
Combine sample variances for t-test.
```
=((COUNT(A)-1)*VAR.S(A)+(COUNT(B)-1)*VAR.S(B))/(COUNT(A)+COUNT(B)-2)
```
**Result:** Pooled variance estimate for independent samples t-test.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | Fewer than 2 numeric values in range | VAR.S requires at least 2 data points (n-1 cannot be 0) |
| #VALUE! | Non-numeric text directly in arguments | Use numeric values or cell references to data |
| #NAME? | Function not recognized or misspelled | Verify spelling; ensure Excel 2010 or later |
| #NUM! | Numeric overflow in calculation | Check for extremely large values or data errors |
| Zero result | All values are identical | Correct behavior - no variation in identical values |
| Different from expected | Sample vs population confusion | Verify VAR.S is appropriate for your data type |

## Use Cases

### 1. Portfolio Risk Analysis
**Scenario:** An investment analyst calculates the variance of stock returns to assess risk and optimize portfolio allocation.

**Implementation:** Monthly returns represent a sample of possible future returns, making VAR.S appropriate for estimating true variance.

**Formula:**
```
Monthly Variance: =VAR.S(MonthlyReturns)
Annualized: =VAR.S(MonthlyReturns)*12
```

**Analysis:** Portfolio variance combines individual asset variances weighted by allocation, plus covariance terms. Assets with lower variance relative to return contribute to better risk-adjusted performance. The sample variance properly estimates what the true population variance of all possible returns would be.

### 2. A/B Test Variance Comparison
**Scenario:** A product team compares the variance of user engagement times between two app versions to assess which provides a more consistent experience.

**Implementation:** Each version's engagement data represents a sample of all possible user sessions.

**Formula:**
```
Version A: =VAR.S(EngagementA)
Version B: =VAR.S(EngagementB)
```

**Analysis:** Lower variance indicates more consistent user experience. The F-test using VAR.S(A)/VAR.S(B) determines if the difference in variances is statistically significant. Consistent experience (lower variance) often indicates better UX design.

### 3. Manufacturing Process Capability
**Scenario:** A quality engineer samples product dimensions to evaluate whether the manufacturing process meets specifications.

**Implementation:** Sample variance feeds into process capability calculations that determine if the process can consistently produce within tolerance.

**Formula:**
```
=VAR.S(DimensionSamples)
Cp = (USL - LSL)^2 / (36 * VAR.S(Samples))
```

**Analysis:** Lower variance indicates a more capable process. The variance must be small enough that 6 standard deviations (approximately 99.7% of output) fits within the specification limits. Sample variance appropriately estimates the true process variance from the sample data.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Relationship to VAR | Identical results | Identical results |
| Maximum arguments | 255 | 255 |
| Minimum data points | 2 | 2 |
| Array formula support | Yes (CSE and dynamic) | Yes (native) |
| Text in range | Ignored | Ignored |
| Logical in range | Ignored | Ignored |
| Direct logical arguments | TRUE=1, FALSE=0 | TRUE=1, FALSE=0 |
| Precision | 15 significant digits | 15 significant digits |

**Note:** VAR.S behaves identically across Excel and Google Sheets. It produces the same results as VAR but with clearer naming convention.

## Tips and Best Practices

1. **Prefer VAR.S over VAR:** Although both produce identical results, VAR.S clearly communicates that you are calculating sample variance. This improves formula readability and reduces confusion.

2. **Use STDEV.S for interpretation:** Since variance uses squared units, consider calculating STDEV.S (the square root of VAR.S) when you need results that are easier to interpret in the context of your data.

3. **Ensure adequate sample size:** While VAR.S technically works with just 2 data points, variance estimates are highly unstable with small samples. Aim for at least 30 observations for reliable estimates.

4. **Validate sampling methodology:** The accuracy of your variance estimate depends entirely on having a representative sample. Biased sampling produces biased variance estimates.

5. **Consider degrees of freedom:** When combining variances or performing statistical tests, remember that sample variance has n-1 degrees of freedom, not n.

6. **Document your methodology:** Note that you used sample variance and explain your sampling approach. This supports reproducibility and proper interpretation.

## Related Functions

| Function | Description |
|----------|-------------|
| [[VAR]] | Legacy function identical to VAR.S; use VAR.S for new work |
| [[VAR.P]] | Population variance (divides by n) |
| [[VARA]] | Sample variance including text and logical values in arrays |
| [[STDEV.S]] | Sample standard deviation (square root of VAR.S) |
| [[STDEV.P]] | Population standard deviation |
| [[AVERAGE]] | Arithmetic mean, central to variance calculation |
| [[F.TEST]] | Compares variances of two samples |
| [[COVARIANCE.S]] | Sample covariance between two datasets |

## Official Documentation

- [Microsoft Excel VAR.S function](https://support.microsoft.com/en-us/office/var-s-function-913633de-136b-449d-813e-65a00b2b990b)
- [Google Sheets VAR.S function](https://support.google.com/docs/answer/9413172)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | VAR.S introduced as modern replacement for VAR |
| Excel 2013 | Performance optimizations |
| Excel 2016 | Integration with dynamic arrays |
| Excel 2019/365 | Full compatibility with array formulas and spill ranges |
| Google Sheets | Supported since VAR.S was added to Google Sheets |

---

## Sample vs. Population: Practical Guidelines

**When to use VAR.S (Sample):**
- Survey data representing a larger customer base
- Quality samples from ongoing production
- Experimental measurements from a study
- Financial returns used to predict future performance
- Any data subset intended to represent a larger group

**When to use VAR.P (Population):**
- Complete census of all members
- All transactions in a closed period
- Every student's grade in a specific class
- All employees in a company department

**The Mathematical Difference:**
- VAR.S: sum((x-mean)^2)/(n-1)
- VAR.P: sum((x-mean)^2)/n

The (n-1) denominator in VAR.S is Bessel's correction. This adjustment produces an unbiased estimate of population variance from sample data. Without this correction, sample variance would systematically underestimate the true population variance because samples tend to be less variable than the populations they represent.
