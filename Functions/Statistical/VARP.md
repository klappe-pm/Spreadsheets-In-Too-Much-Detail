---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- variance
- population-statistics
- dispersion
- variability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Population Variance
- Variance Population
- VAR.P
tags:
- variance
- population
- statistics
- dispersion
- variability
---

# VARP

## Description

**VARP** (or VAR.P in newer Excel) calculates the population variance—a measure of how spread out values are from the mean when your data represents the entire population. Unlike sample variance (VAR.S), which divides by n-1, VARP divides by n, making it appropriate when you have complete population data rather than a sample.

Use VARP when your data represents a **complete population**, not a sample drawn from a larger group. Examples include: all employees in a department, all products in inventory, all transactions in a specific period, or any dataset where you have exhaustive information about the group of interest. If your data is a sample intended to estimate population variability, use VAR.S instead.

Variance is the square of the standard deviation—it measures average squared deviation from the mean. While less intuitive than SD (variance has squared units), it has useful mathematical properties: variances add for independent variables, and it's used directly in many statistical formulas.

**VARP requires at least 1 data point** (unlike sample variance which requires 2). It ignores text and logical values in ranges—if you need those converted to 0/1, use VARPA instead.

## Syntax

> [!f(x)] VARP Syntax
>
> ```
> =VARP(number1, [number2], ...)
> ```
> Or in Excel 2010+:
> ```
> =VAR.P(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | First number, cell reference, or range representing population data. |
| `number2, ...` | No | Additional numbers, references, or ranges. Up to 255 arguments total. |

### Return Value

Returns population variance as a non-negative number. Returns 0 if all values are identical (no variation). Returns #VALUE! for non-numeric direct arguments. Text and logical values in ranges are ignored.

## Examples

> [!f(x)] VARP Examples

### Example 1: Basic Population Variance
```
=VARP(2, 4, 6, 8, 10)
```
**Result:** 8

**Explanation:** Population variance with mean 6. Sum of squared deviations = 40, divided by n=5.

---

### Example 2: Comparing VARP vs VAR.S
```
=VARP(2, 4, 6, 8, 10)  → 8
=VAR.S(2, 4, 6, 8, 10) → 10
```
**Result:** Sample variance is larger

**Explanation:** Sample variance divides by n-1=4 instead of n=5, producing larger result.

---

### Example 3: Relationship to STDEVP
```
=VARP(Data) = STDEVP(Data)^2
=SQRT(VARP(Data)) = STDEVP(Data)
```
**Result:** Always true

**Explanation:** Variance is standard deviation squared. VARP and STDEVP are mathematically related.

---

### Example 4: All Department Salaries
```
=VARP(DepartmentSalaries)
```
**Result:** True population variance

**Explanation:** If you have ALL employees in department, use VARP for exact variability measure.

---

### Example 5: Cell Range
```
=VARP(A1:A100)
```
**Result:** Population variance of values

**Explanation:** Standard usage with cell range. Text and logical values ignored.

---

### Example 6: Single Value
```
=VARP(5)
```
**Result:** 0

**Explanation:** Single value has no variation. Unlike VAR.S, VARP works with n=1.

---

### Example 7: Two Values
```
=VARP(10, 20)
```
**Result:** 25

**Explanation:** Mean=15, each value is 5 away. Variance = (25+25)/2 = 25.

---

### Example 8: Identical Values
```
=VARP(5, 5, 5, 5, 5)
```
**Result:** 0

**Explanation:** No variation means zero variance.

---

### Example 9: Multiple Ranges
```
=VARP(A1:A50, B1:B50, C1:C50)
```
**Result:** Combined population variance

**Explanation:** All values treated as single population.

---

### Example 10: Complete Inventory Values
```
=VARP(InventoryPrices)
```
**Result:** Price variance across entire inventory

**Explanation:** All products = complete population. VARP gives exact measure.

---

### Example 11: Large vs Small n Effect
```
n=5:  VARP/VAR.S ratio = 4/5 = 0.8
n=30: VARP/VAR.S ratio = 29/30 = 0.967
n=100: VARP/VAR.S ratio = 0.99
```
**Result:** Difference diminishes with larger n

**Explanation:** For large populations, choice matters less. For small populations, be precise.

---

### Example 12: VAR.P Syntax (Excel 2010+)
```
=VAR.P(A1:A100)
```
**Result:** Same as VARP

**Explanation:** VAR.P is the newer name for VARP; both work identically.

---

### Example 13: Variance Additivity
```
For independent variables:
VAR(X + Y) = VAR(X) + VAR(Y)
=VARP(X) + VARP(Y) approximates VARP(X+Y) for independent data
```
**Result:** Variances add for independent data

**Explanation:** Key mathematical property making variance useful in statistics.

---

### Example 14: Portfolio Variance Calculation
```
For two-asset portfolio:
Portfolio_Var = w1^2*VARP(R1) + w2^2*VARP(R2) + 2*w1*w2*Cov(R1,R2)
```
**Result:** Portfolio risk calculation

**Explanation:** Variance is fundamental to portfolio theory.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric direct argument | Text as direct arguments causes error. Use cell references. |
| `#DIV/0!` | No numeric values | Range contains no numbers. Check data. |
| `#NAME?` | Misspelled function | Check spelling: VARP or VAR.P (with dot for Excel 2010+). |
| `Wrong function choice` | Sample vs population confusion | Use VARP for populations, VAR.S for samples. |
| `Unexpected 0` | All values identical | Zero variation produces variance of 0. This is correct. |

## Use Cases

### [[Complete Population Analysis]]

**Scenario:** Calculate variance for exhaustive datasets.

**Implementation:**
```
=VARP(AllEmployeeTenure)
=VARP(EntireInventoryValues)
=VARP(AllTransactionAmounts)
```

**Business Application:** HR analytics, inventory management, complete financial records.

**Technical Details:** When data is exhaustive, VARP gives the true population parameter, not an estimate.

---

### [[Portfolio Risk Calculation]]

**Scenario:** Calculate variance components for portfolio analysis.

**Implementation:**
```
=VARP(AssetReturns)
Portfolio_Var = Sum(wi^2 * VARPi) + 2*Sum(wi*wj*COVij)
=SQRT(Portfolio_Var)  → Portfolio volatility
```

**Business Application:** Investment management, risk analysis, asset allocation.

**Technical Details:** Variance is central to Modern Portfolio Theory. Use VARP for historical return analysis.

---

### [[Quality Control]]

**Scenario:** Measure process variation for entire production.

**Implementation:**
```
=VARP(BatchMeasurements)
=VARP(ProductDimensions)
Process_Capability = (USL-LSL)^2 / (36*VARP(Data))
```

**Business Application:** Manufacturing quality, process capability, Six Sigma.

**Technical Details:** Cp and Cpk calculations use variance. For 100% inspection, use VARP.

---

### [[Academic Analysis]]

**Scenario:** Analyze complete class performance.

**Implementation:**
```
=VARP(AllGrades)
=VARP(ExamScores)
Z-score = (Score - AVERAGE(Scores))/SQRT(VARP(Scores))
```

**Business Application:** Grade analysis, assessment evaluation.

**Technical Details:** Entire class is the population; VARP gives exact variance.

## Platform Differences

### Microsoft Excel
- **Availability:** VARP in all versions; VAR.P added in Excel 2010
- **Recommendation:** Use VAR.P for new workbooks (clearer naming)
- **Legacy:** VARP maintained for backward compatibility
- **Behavior:** Both identical mathematically

### Google Sheets
- **Availability:** Both VARP and VAR.P supported
- **Performance:** Efficient for large datasets
- **Compatibility:** Full Excel compatibility

### Both Platforms
- Identical mathematical formula: sum((x-mean)^2)/n
- Same handling of text/logical in ranges (ignored)
- Same error conditions
- Minimum 1 data point required

## Tips and Best Practices

1. **Know your data type:** Population (complete data) = VARP. Sample (partial data) = VAR.S. This choice matters for small n.

2. **Use VAR.P for clarity:** The dot notation makes it clearer you're using population formula.

3. **Large n approximation:** For n > 30, difference between VARP and VAR.S becomes small.

4. **Don't mix functions:** If using VARP, also use STDEV.P for SD. Be consistent.

5. **Relationship with SD:** VARP = STDEVP^2. Choose whichever your analysis needs.

6. **Variance adds for independent data:** This property makes variance fundamental for portfolio theory and ANOVA.

7. **Text handling:** VARP ignores text. If text should count as 0, use VARPA.

8. **Zero variance interpretation:** If VARP returns 0, all values are identical.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VAR.S]] | Sample variance | When data is a sample, not complete population |
| [[VARA]] | Sample variance with text/logical conversion | For sample data with text/logical |
| [[VARPA]] | Population variance with text/logical conversion | For population data with text/logical |
| [[STDEV.P]] | Population standard deviation | When you need SD, not variance |

### Commonly Used Together

**[[STDEV.P]]** - Standard deviation

*SD-variance relationship:*
```
=VARP(Data)        → Variance
=STDEVP(Data)      → Standard deviation
=STDEVP(Data)^2    → Should equal VARP(Data)
```
Related by squaring.

---

**[[AVERAGE]]** - Calculate mean

*Basic population statistics:*
```
Mean:     =AVERAGE(Data)
Variance: =VARP(Data)
SD:       =STDEVP(Data)
```
Together describe population distribution.

---

**[[VAR.S]]** - Compare sample vs population

*Understand the difference:*
```
Population: =VARP(Data)
Sample:     =VAR.S(Data)
Ratio:      =VARP(Data)/VAR.S(Data) = (n-1)/n
```
Ratio approaches 1 as n increases.

---

**[[COVARIANCE.P]]** - Portfolio calculations

*Combined variability:*
```
=VARP(X) + VARP(Y) + 2*COVARIANCE.P(X,Y)
```
For sum of two variables.

---

**[[DEVSQ]]** - Sum of squared deviations

*Components:*
```
=DEVSQ(Data)/COUNT(Data)  → Should equal VARP(Data)
```
DEVSQ gives numerator of variance calculation.

## Official Documentation

- **Microsoft Excel:** [VAR.P function](https://support.microsoft.com/en-us/office/var-p-function-73d1285c-108c-4843-ba5d-a51f90656f3a)
- **Google Sheets:** [VARP function](https://support.google.com/docs/answer/3094113)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original VARP |
| Excel 2010 | Added | VAR.P as newer name |
| Excel 2013+ | Both supported | VARP for compatibility |
| Google Sheets | Original launch | Both syntaxes supported |

---

*Last updated: 2026-01-10*
