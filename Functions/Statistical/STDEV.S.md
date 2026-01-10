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
- sample-standard-deviation
- stdevs
tags:
- statistical
- dispersion
- variability
- sample
- modern
---

# STDEV.S

## Description

The STDEV.S function calculates the standard deviation based on a sample of a population. This is the modern, clearly-named replacement for the legacy STDEV function, introduced in Excel 2010 as part of Microsoft's initiative to improve statistical function clarity. The ".S" suffix explicitly indicates sample-based calculation, distinguishing it from STDEV.P (population standard deviation). Both STDEV.S and STDEV produce identical results, but Microsoft recommends STDEV.S for new projects due to its unambiguous naming.

Sample standard deviation uses Bessel's correction, dividing by (n-1) rather than n in its formula. This adjustment compensates for the inherent tendency of samples to underestimate population variability. When you collect data from a subset of a larger population, you are statistically less likely to capture extreme values, leading to an artificially compressed view of the true spread. The (n-1) denominator corrects this bias, providing an unbiased estimate of the population standard deviation from sample data.

This function is essential for virtually all statistical analysis involving sampled data, which encompasses the vast majority of real-world scenarios. Quality control samples from production runs, customer survey responses, experimental measurements, and financial market samples all require STDEV.S for accurate variability assessment. The result helps establish confidence intervals, determine required sample sizes, assess measurement precision, and compare consistency across different datasets or time periods.

STDEV.S ignores text values, logical values (TRUE/FALSE), and empty cells within referenced ranges, counting only numeric values. However, if you type logical values or text representations of numbers directly as function arguments, they will be included in the calculation. This behavior mirrors other statistical functions and provides flexibility while protecting against common data quality issues.

## Syntax

> [!info] Function Syntax
> ```
> =STDEV.S(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range containing sample data | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, named ranges, arrays, or cell references. Within references, only numeric values are counted; empty cells, logical values, text, and errors are ignored. Direct arguments typed as numbers or logical values are included.

## Examples

### Example 1: Basic Sample Standard Deviation
Calculate the sample standard deviation of monthly sales.
```
=STDEV.S(B2:B13)
```
**Result:** Returns the sample standard deviation of 12 monthly sales values.

### Example 2: Explicit Numeric Values
Calculate standard deviation of directly entered values.
```
=STDEV.S(45, 52, 48, 55, 50)
```
**Result:** 3.81 (approximately) - the sample standard deviation of these five values.

### Example 3: Quality Control Sample
Measure variation in product weight samples.
```
=STDEV.S(Samples!C2:C31)
```
**Result:** Sample standard deviation of 30 weight measurements used to estimate production consistency.

### Example 4: Multiple Disconnected Ranges
Combine samples from different sources.
```
=STDEV.S(Q1_Sales, Q2_Sales, Q3_Sales, Q4_Sales)
```
**Result:** Single sample standard deviation calculated across all quarterly named ranges.

### Example 5: Financial Volatility Calculation
Calculate investment return volatility.
```
=STDEV.S(Returns!D2:D25)*SQRT(12)
```
**Result:** Annualized volatility based on 24 months of sample returns.

### Example 6: Comparing with Population SD
Demonstrating the difference in formulas.
```
Sample:     =STDEV.S(A1:A5)  → 2.74
Population: =STDEV.P(A1:A5)  → 2.45
```
**Result:** Sample SD is higher due to (n-1) correction for 5 data points.

### Example 7: With AVERAGE for Complete Statistics
Calculate descriptive statistics together.
```
Mean: =AVERAGE(B2:B100)
SD:   =STDEV.S(B2:B100)
CV:   =STDEV.S(B2:B100)/AVERAGE(B2:B100)
```
**Result:** Complete basic statistical summary of the sample.

### Example 8: Control Chart Upper Limit
Establish statistical control limits.
```
=AVERAGE(Samples)+3*STDEV.S(Samples)
```
**Result:** Upper control limit at 3 standard deviations above mean.

### Example 9: Filtered Data Standard Deviation
Calculate SD for subset of data.
```
=STDEV.S(FILTER(B2:B500, A2:A500="Region A"))
```
**Result:** Sample standard deviation for Region A data only.

### Example 10: Confidence Interval Width
Calculate margin of error for confidence interval.
```
=STDEV.S(B2:B50)/SQRT(COUNT(B2:B50))*1.96
```
**Result:** 95% confidence interval margin (approximately) for the sample mean.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | Fewer than 2 numeric values in range | STDEV.S requires at least 2 data points (n-1 cannot be 0) |
| #VALUE! | Non-numeric text directly in arguments | Use numeric values or cell references to data |
| #NAME? | Function not recognized or misspelled | Verify spelling; ensure Excel 2010 or later |
| #NUM! | Numeric overflow in calculation | Check for extremely large values or data errors |
| Zero result | All values are identical | Correct behavior - no variation in identical values |
| Different from expected | Sample vs population confusion | Verify STDEV.S is appropriate for your data type |

## Use Cases

### 1. Survey Response Analysis
**Scenario:** A market research team surveys 500 customers about product satisfaction on a 1-10 scale to understand the variability in customer experience.

**Implementation:** The survey responses represent a sample from the total customer population of 50,000, making STDEV.S the appropriate choice.

**Formula:**
```
=STDEV.S(SurveyResponses!C2:C501)
```

**Analysis:** A standard deviation of 2.5 on a 10-point scale indicates moderate variability in satisfaction. Combined with the mean, this helps segment customers: those more than 1 SD below the mean may be at risk of churn. The sample standard deviation properly estimates what the true population variability would be if all 50,000 customers were surveyed.

### 2. A/B Testing Statistical Significance
**Scenario:** A website runs an A/B test comparing conversion rates between two page designs, with each variant receiving 1,000 visitors.

**Implementation:** Calculate the standard deviation of individual user behaviors to determine if the difference between variants is statistically significant.

**Formula:**
```
=STDEV.S(VariantA_Conversions)
SE_A: =STDEV.S(VariantA)/SQRT(1000)
```

**Analysis:** The sample standard deviation feeds into standard error and t-test calculations. With 1,000 observations per variant, you have sufficient power to detect meaningful differences. The sample formula correctly accounts for the fact that these visitors represent a sample of potential future visitors.

### 3. Laboratory Measurement Precision
**Scenario:** A laboratory conducts 20 replicate measurements of a chemical compound to assess measurement precision and establish acceptable variation ranges.

**Implementation:** Sample standard deviation quantifies measurement uncertainty, which is essential for quality certification and method validation.

**Formula:**
```
=STDEV.S(ReplicateMeasurements!B2:B21)
```

**Analysis:** The standard deviation represents measurement precision under repeatability conditions. Regulatory standards often require that precision (expressed as relative standard deviation) falls below specified thresholds. Using sample standard deviation is correct because these 20 measurements sample the infinite possible measurements that could be made.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Relationship to STDEV | Identical results | Identical results |
| Maximum arguments | 255 | 255 |
| Minimum data points | 2 | 2 |
| Array formula support | Yes (CSE and dynamic) | Yes (native) |
| Text in range | Ignored | Ignored |
| Logical in range | Ignored | Ignored |
| Direct logical arguments | TRUE=1, FALSE=0 | TRUE=1, FALSE=0 |
| Precision | 15 significant digits | 15 significant digits |

**Note:** STDEV.S behaves identically across Excel and Google Sheets. The function produces the same results as STDEV but with clearer naming convention.

## Tips and Best Practices

1. **Prefer STDEV.S over STDEV:** Although both produce identical results, STDEV.S clearly communicates that you are calculating sample standard deviation. This improves formula readability and reduces confusion.

2. **Ensure adequate sample size:** While STDEV.S technically works with just 2 data points, meaningful statistical inference typically requires larger samples. Aim for at least 30 observations when making population inferences.

3. **Validate sample representativeness:** The accuracy of your sample standard deviation as a population estimate depends entirely on having a representative sample. Random or stratified sampling produces better estimates than convenience samples.

4. **Use with confidence intervals:** Combine STDEV.S with CONFIDENCE.T or manual calculations to establish confidence intervals around your mean estimates.

5. **Consider robust alternatives:** Standard deviation is sensitive to outliers. For datasets with extreme values, consider using interquartile range or median absolute deviation as complementary measures.

6. **Document your methodology:** When sharing analysis, note that you used sample standard deviation and explain your sampling approach. This supports reproducibility and peer review.

## Related Functions

| Function | Description |
|----------|-------------|
| [[STDEV]] | Legacy function identical to STDEV.S; use STDEV.S for new work |
| [[STDEV.P]] | Population standard deviation (divides by n) |
| [[STDEVA]] | Sample standard deviation including text and logical values in arrays |
| [[VAR.S]] | Sample variance (STDEV.S squared) |
| [[VAR.P]] | Population variance |
| [[AVERAGE]] | Arithmetic mean, typically paired with STDEV.S |
| [[CONFIDENCE.T]] | Confidence interval using t-distribution and sample statistics |
| [[T.TEST]] | T-test using sample statistics |

## Official Documentation

- [Microsoft Excel STDEV.S function](https://support.microsoft.com/en-us/office/stdev-s-function-7d69cf97-0c1f-4acf-be27-f3e83904cc23)
- [Google Sheets STDEV.S function](https://support.google.com/docs/answer/9413087)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | STDEV.S introduced as modern replacement for STDEV |
| Excel 2013 | Performance optimizations |
| Excel 2016 | Integration with dynamic arrays |
| Excel 2019/365 | Full compatibility with array formulas and spill ranges |
| Google Sheets | Supported since STDEV.S was added to Google Sheets |

---

## Sample vs. Population: Practical Guidelines

**When to use STDEV.S (Sample):**
- Survey data representing a larger customer base
- Quality samples from ongoing production
- Experimental measurements from a study
- Financial data used to predict future performance
- Any data subset intended to represent a larger group

**When to use STDEV.P (Population):**
- Complete census of all members
- All transactions in a closed period
- Every student's grade in a specific class
- All employees in a company department

**The Mathematical Difference:**
- STDEV.S: sqrt(sum((x-mean)^2)/(n-1))
- STDEV.P: sqrt(sum((x-mean)^2)/n)

The (n-1) denominator in STDEV.S is Bessel's correction, which produces an unbiased estimate of population standard deviation. Without this correction, sample standard deviations would systematically underestimate the true population value.
