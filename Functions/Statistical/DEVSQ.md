---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- sum-of-squares
- deviation
- variance-component
- dispersion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Deviation Squared
- Sum of Squared Deviations
- SS Total
- Total Sum of Squares
tags:
- statistical
- variance
- deviation
- sum-of-squares
- dispersion
---

# DEVSQ

## Description

**DEVSQ** calculates the sum of squared deviations from the sample mean, which is a fundamental building block for variance, standard deviation, and many other statistical measures. For each value in your data, DEVSQ computes how far it is from the mean, squares this deviation, and sums all squared deviations together. The result represents the total variability in your dataset and forms the numerator when calculating sample variance (which divides by n-1) or population variance (which divides by n).

The mathematical formula is: DEVSQ = SUM((xi - mean)^2), where mean is the arithmetic average of all values. Squaring the deviations serves two purposes: it eliminates negative signs (ensuring deviations don't cancel out) and it gives more weight to larger deviations (a point 10 units from the mean contributes 100 to DEVSQ, while a point 5 units away contributes only 25). This quadratic weighting is foundational to least-squares methods throughout statistics.

**Relationship to variance and standard deviation:** DEVSQ is the numerator in variance calculations. Sample variance = DEVSQ / (n-1), and population variance = DEVSQ / n. Therefore: DEVSQ = VAR.S * (n-1) = VAR.P * n. Similarly, standard deviation = SQRT(DEVSQ / (n-1)) for samples. If you have DEVSQ and count, you can derive any variance measure.

**Why DEVSQ matters:** Beyond variance calculations, DEVSQ appears in ANOVA (Analysis of Variance), regression analysis (total sum of squares), coefficient of determination (R-squared), and various hypothesis tests. Understanding DEVSQ provides insight into how these statistical methods decompose and explain variability. It's also useful when you need to combine statistics from multiple groups or perform weighted variance calculations.

## Syntax

> [!f(x)] DEVSQ Syntax
>
> ```
> =DEVSQ(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first number, cell reference, or range for which you want the sum of squared deviations. |
| `number2, ...` | No | Additional numbers, cell references, or ranges. Up to 255 arguments in Excel, effectively unlimited through ranges in Google Sheets. |

### Return Value

Returns a non-negative number representing the sum of squared deviations from the mean. Returns 0 if all values are identical (no deviation from mean). Returns #DIV/0! or equivalent error if no numeric values are provided.

## Examples

> [!f(x)] DEVSQ Examples

### Example 1: Basic Sum of Squared Deviations
```
=DEVSQ(2, 4, 6, 8, 10)
```
**Result:** 40

**Explanation:** Mean is 6. Deviations: -4, -2, 0, 2, 4. Squared: 16, 4, 0, 4, 16. Sum = 40. This is the total squared variability around the mean.

---

### Example 2: Uniform Data (No Variation)
```
=DEVSQ(5, 5, 5, 5, 5)
```
**Result:** 0

**Explanation:** When all values equal the mean, every deviation is zero. Zero squared is still zero, and the sum is zero. No variability exists.

---

### Example 3: Cell Range Reference
```
=DEVSQ(A2:A11)
```
Where A2:A11 contains 10 test scores

**Result:** Sum of squared deviations from the mean score

**Explanation:** DEVSQ calculates the mean of A2:A11, then sums the squared deviations from that mean.

---

### Example 4: Calculating Sample Variance from DEVSQ
```
DEVSQ: =DEVSQ(A2:A11)
Count: =COUNT(A2:A11)
Sample Variance: =DEVSQ(A2:A11)/(COUNT(A2:A11)-1)
Verify: =VAR.S(A2:A11)
```
**Result:** Both variance calculations match

**Explanation:** Sample variance divides DEVSQ by (n-1) for Bessel's correction. This demonstrates the relationship between DEVSQ and VAR.S.

---

### Example 5: Calculating Population Variance from DEVSQ
```
Population Variance: =DEVSQ(A2:A11)/COUNT(A2:A11)
Verify: =VAR.P(A2:A11)
```
**Result:** Both calculations match

**Explanation:** Population variance divides DEVSQ by n (no correction needed when you have the entire population).

---

### Example 6: Multiple Ranges Combined
```
=DEVSQ(Group1, Group2, Group3)
```
**Result:** Combined sum of squared deviations

**Explanation:** DEVSQ calculates the overall mean across all groups, then sums squared deviations from that combined mean. Useful for pooled variance calculations.

---

### Example 7: Single Value
```
=DEVSQ(42)
```
**Result:** 0

**Explanation:** With one value, the mean equals that value, so deviation is zero and DEVSQ is zero.

---

### Example 8: Ignoring Text Values
```
=DEVSQ(A1:A10)
```
Where A1:A10 contains: 10, 20, "N/A", 30, 40, "Missing", 50, 60, 70, 80

**Result:** DEVSQ of numeric values only (10, 20, 30, 40, 50, 60, 70, 80)

**Explanation:** Text values are ignored. The mean and DEVSQ are calculated from the 8 numeric values.

---

### Example 9: Relationship to Standard Deviation
```
DEVSQ: =DEVSQ(A2:A101)       Result: 2500
n: =COUNT(A2:A101)          Result: 100
STDEV.S: =SQRT(DEVSQ(A2:A101)/(COUNT(A2:A101)-1))
Verify: =STDEV.S(A2:A101)
```
**Result:** Both standard deviation calculations match

**Explanation:** Standard deviation is the square root of variance, which is DEVSQ divided by (n-1) for samples.

---

### Example 10: ANOVA Total Sum of Squares
```
SS_Total: =DEVSQ(AllData)
```
**Result:** Total sum of squares for ANOVA decomposition

**Explanation:** In ANOVA, total variability (SS_Total) is partitioned into between-group (SS_Between) and within-group (SS_Within). DEVSQ provides SS_Total directly.

---

### Example 11: Large Dataset
```
=DEVSQ(SalesData)
```
Where SalesData contains 10,000 daily sales figures

**Result:** Total squared deviation from mean sales

**Explanation:** DEVSQ efficiently handles large datasets, providing the total variability measure for further analysis.

---

### Example 12: Coefficient of Variation Components
```
DEVSQ: =DEVSQ(A2:A101)
Mean: =AVERAGE(A2:A101)
CV components for analysis
```
**Result:** Building blocks for standardized variability measures

**Explanation:** DEVSQ and mean together enable calculation of coefficient of variation and other standardized dispersion measures.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | No numeric values in range | Ensure at least one numeric value exists. Check for all-text or all-empty ranges. |
| `#VALUE!` | Non-numeric value passed directly as argument | Use cell references for mixed data. Only numeric constants as direct arguments. |
| `#NAME?` | Function name misspelled | Check spelling: DEVSQ, not DEVSSQ or DEVSQUARE. |
| `0` (unexpected) | All values identical | Correct behavior. Identical values have zero deviation. |
| `Very large result` | Outliers or large scale | Expected for variable data. Consider data transformation if needed. |
| `Negative result` | Never happens | DEVSQ cannot be negative (squared terms). If you see negative, check your formula. |

## Use Cases

### [[ANOVA: Total Sum of Squares Calculation]]

**Scenario:** A statistician performs Analysis of Variance (ANOVA) and needs to decompose total variability into between-group and within-group components.

**Implementation:**
```
SS_Total: =DEVSQ(AllObservations)
SS_Between: (calculated from group means and sizes)
SS_Within: =SS_Total - SS_Between
```

**Business Application:** ANOVA determines whether differences among group means are statistically significant. In marketing, this might compare campaign effectiveness across regions. In manufacturing, it could compare output quality across machines. DEVSQ provides the total variability that ANOVA partitions and explains.

**Technical Details:** SS_Total from DEVSQ represents variability if we ignored group membership. The ratio SS_Between/SS_Total indicates how much variability is explained by group membership. This forms the basis for the F-statistic and p-value in ANOVA.

---

### [[Regression Analysis: R-Squared Calculation]]

**Scenario:** A data analyst calculates the coefficient of determination (R-squared) to evaluate how well a regression model explains variability in the dependent variable.

**Implementation:**
```
SS_Total: =DEVSQ(Y_actual)
SS_Residual: =SUMXMY2(Y_actual, Y_predicted)
R_squared: =1 - SS_Residual/SS_Total
```

**Business Application:** R-squared tells stakeholders what percentage of outcome variability the model explains. For a sales prediction model, R-squared of 0.85 means the model explains 85% of sales variability. DEVSQ provides the baseline total variability against which model performance is measured.

**Technical Details:** SS_Total (DEVSQ of actual values) represents variability without any predictive model. SS_Residual represents unexplained variability after modeling. R-squared = 1 - (SS_Residual/SS_Total) = SS_Explained/SS_Total.

---

### [[Pooled Variance Calculation]]

**Scenario:** A researcher combines standard deviations from multiple samples to create a pooled estimate for use in two-sample t-tests or meta-analysis.

**Implementation:**
```
DEVSQ_1: =VAR.S(Group1)*(COUNT(Group1)-1)
DEVSQ_2: =VAR.S(Group2)*(COUNT(Group2)-1)
Pooled_Variance: =(DEVSQ_1+DEVSQ_2)/(n1+n2-2)
```

**Business Application:** When comparing two groups with potentially different variances, pooled variance provides a weighted average. This is used in independent-samples t-tests assuming equal variances. Correct pooling requires sum of squared deviations (DEVSQ), not simple averaging of variances.

**Technical Details:** Direct use of DEVSQ (rather than recovering from variance) is more accurate. The formula weights each group's sum of squares equally, not by sample size. Degrees of freedom are additive: (n1-1) + (n2-1) = n1+n2-2.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Maximum arguments:** 255 separate arguments
- **Text handling:** Text values in ranges are ignored
- **Logical handling:** TRUE/FALSE in ranges are ignored
- **Precision:** 15 significant decimal digits
- **Empty cells:** Ignored in calculations

### Google Sheets

- **Availability:** All versions since launch
- **Maximum arguments:** 30 separate arguments (ranges can contain more cells)
- **Text handling:** Text values in ranges are ignored
- **Logical handling:** TRUE/FALSE in ranges are ignored
- **Precision:** 15 significant decimal digits
- **Empty cells:** Ignored in calculations

### Key Difference Alert

DEVSQ behaves identically between Excel and Google Sheets. Both platforms:
- Calculate mean from numeric values only
- Ignore text, logical values, and empty cells
- Return 0 for single values or uniform data
- Use the same mathematical algorithm

The only practical difference is argument count limits, which rarely matter when using range references.

## Tips and Best Practices

1. **Understand the relationship to variance.** DEVSQ / (n-1) = sample variance. DEVSQ / n = population variance. This relationship is fundamental and helps verify calculations.

2. **Use for ANOVA and regression.** DEVSQ provides total sum of squares (SS_Total) directly, which is essential for F-tests in ANOVA and R-squared in regression.

3. **Verify with known data.** For the dataset {2, 4, 6}, mean=4, deviations are {-2, 0, 2}, squared deviations are {4, 0, 4}, DEVSQ=8. Use simple examples to verify your understanding.

4. **DEVSQ is always non-negative.** Squared values cannot be negative. If you ever get a negative result, there's an error in your formula or reference.

5. **Combine groups correctly.** For pooled variance, add DEVSQ values (not variances). Variances aren't additive; sums of squares are.

6. **Consider computational stability.** For very large datasets with values far from zero, the direct DEVSQ calculation is numerically stable. Alternative formulas using SUM(X^2) - n*MEAN^2 can have precision issues.

7. **Use for quality control.** Track DEVSQ over time to monitor process variability. Increasing DEVSQ indicates growing inconsistency.

8. **Remember what DEVSQ ignores.** Text and logical values are silently excluded. Verify your data contains the expected numeric values.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VAR.S]] | Sample variance | When you want variance directly (DEVSQ/(n-1)) |
| [[VAR.P]] | Population variance | When you have complete population (DEVSQ/n) |
| [[STDEV.S]] | Sample standard deviation | When you want standard deviation (sqrt of variance) |
| [[AVEDEV]] | Average absolute deviation | For robust dispersion measure less sensitive to outliers |
| [[SUMXMY2]] | Sum of squared differences between two arrays | For residual sum of squares in regression |

### Commonly Used Together

**[[COUNT]]** - To calculate variance from DEVSQ

*Derive variance:*
```
DEVSQ: =DEVSQ(data)
n: =COUNT(data)
Sample Variance: =DEVSQ(data)/(COUNT(data)-1)
```

---

**[[VAR.S]]** - Verification and comparison

*Verify relationship:*
```
From DEVSQ: =DEVSQ(data)/(COUNT(data)-1)
Direct: =VAR.S(data)
```
Both should match.

---

**[[SUMXMY2]]** - Regression residual sum of squares

*R-squared calculation:*
```
SS_Total: =DEVSQ(Y_actual)
SS_Residual: =SUMXMY2(Y_actual, Y_predicted)
R_squared: =1 - SS_Residual/SS_Total
```

---

**[[AVERAGE]]** - Mean for context

*Report both:*
```
Mean: =AVERAGE(data)
DEVSQ: =DEVSQ(data)
```
The mean gives location; DEVSQ gives spread.

## Official Documentation

- **Microsoft Excel:** [DEVSQ function](https://support.microsoft.com/en-us/office/devsq-function-8b739616-8376-4df5-8bd0-cfe0a6caf444)
- **Google Sheets:** [DEVSQ function](https://support.google.com/docs/answer/3093849)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2003 | Continued support | No changes |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
