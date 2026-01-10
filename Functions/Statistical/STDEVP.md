---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- standard-deviation
- population-statistics
- variability
- dispersion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Population Standard Deviation
- StdDev Population
- STDEV.P
tags:
- standard-deviation
- population
- statistics
- variability
- dispersion
---

# STDEVP

## Description

**STDEVP** (or STDEV.P in newer Excel) calculates the population standard deviation—a measure of how spread out values are from the mean when your data represents the entire population. Unlike sample standard deviation (STDEV.S), which divides by n-1, STDEVP divides by n, making it appropriate when you have complete population data rather than a sample.

Use STDEVP when your data represents a **complete population**, not a sample drawn from a larger group. Examples include: all employees in a department, all products in inventory, all transactions in a specific period, or any dataset where you have exhaustive information about the group of interest. If your data is a sample intended to estimate population variability, use STDEV.S instead.

The difference between sample and population standard deviation matters for small datasets. Sample SD (n-1 divisor) is slightly larger to compensate for the tendency of samples to underestimate population variability. For large n, the difference becomes negligible: when n=100, sample SD is only about 0.5% larger than population SD.

**STDEVP requires at least 1 data point** (unlike sample SD which requires 2). It ignores text and logical values in ranges—if you need those converted to 0/1, use STDEVPA instead.

## Syntax

> [!f(x)] STDEVP Syntax
>
> ```
> =STDEVP(number1, [number2], ...)
> ```
> Or in Excel 2010+:
> ```
> =STDEV.P(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | First number, cell reference, or range representing population data. |
| `number2, ...` | No | Additional numbers, references, or ranges. Up to 255 arguments total. |

### Return Value

Returns population standard deviation as a non-negative number. Returns 0 if all values are identical (no variation). Returns #VALUE! for non-numeric direct arguments. Text and logical values in ranges are ignored.

## Examples

> [!f(x)] STDEVP Examples

### Example 1: Basic Population SD
```
=STDEVP(2, 4, 6, 8, 10)
```
**Result:** 2.828...

**Explanation:** Population SD of five values with mean 6. Sum of squared deviations divided by n=5.

---

### Example 2: Comparing STDEVP vs STDEV.S
```
=STDEVP(2, 4, 6, 8, 10)  → 2.828
=STDEV.S(2, 4, 6, 8, 10) → 3.162
```
**Result:** Sample SD is larger

**Explanation:** Sample SD divides by n-1=4 instead of n=5, producing larger result.

---

### Example 3: All Department Salaries
```
=STDEVP(DepartmentSalaries)
```
**Result:** True population SD of salary distribution

**Explanation:** If you have ALL employees in department, use STDEVP for exact variability measure.

---

### Example 4: Cell Range
```
=STDEVP(A1:A100)
```
**Result:** Population SD of values in range

**Explanation:** Standard usage with cell range. Text and logical values ignored.

---

### Example 5: Single Value
```
=STDEVP(5)
```
**Result:** 0

**Explanation:** Single value has no variation. Unlike STDEV.S, STDEVP works with n=1.

---

### Example 6: Two Values
```
=STDEVP(10, 20)
```
**Result:** 5

**Explanation:** Mean=15, each value is 5 away. Population SD = sqrt((25+25)/2) = 5.

---

### Example 7: Identical Values
```
=STDEVP(5, 5, 5, 5, 5)
```
**Result:** 0

**Explanation:** No variation means zero standard deviation.

---

### Example 8: Multiple Ranges
```
=STDEVP(A1:A50, B1:B50, C1:C50)
```
**Result:** Combined population SD

**Explanation:** All values treated as single population for SD calculation.

---

### Example 9: Complete Product Prices
```
=STDEVP(ProductPrices)
```
**Result:** Price variation across entire catalog

**Explanation:** All products = complete population. STDEVP gives exact measure.

---

### Example 10: Quiz Scores (Entire Class)
```
=STDEVP(QuizScores)
```
**Result:** True variation in class performance

**Explanation:** Entire class is the population; STDEVP is appropriate.

---

### Example 11: Large vs Small n Effect
```
n=5:  STDEVP/STDEV.S ratio = sqrt(4/5) = 0.894
n=30: STDEVP/STDEV.S ratio = sqrt(29/30) = 0.983
n=100: STDEVP/STDEV.S ratio = 0.995
```
**Result:** Difference diminishes with larger n

**Explanation:** For large populations, choice matters less. For small populations, be precise.

---

### Example 12: STDEV.P Syntax (Excel 2010+)
```
=STDEV.P(A1:A100)
```
**Result:** Same as STDEVP

**Explanation:** STDEV.P is the newer name for STDEVP; both work identically.

---

### Example 13: Coefficient of Variation
```
=STDEVP(Data)/AVERAGE(Data)
```
**Result:** Relative variability (CV)

**Explanation:** CV expresses SD as fraction of mean—useful for comparing different-scale data.

---

### Example 14: Process Capability
```
=STDEVP(ProcessMeasurements)
Upper Control Limit = Mean + 3*STDEVP
Lower Control Limit = Mean - 3*STDEVP
```
**Result:** Control limits for process monitoring

**Explanation:** When measuring entire production run, use STDEVP for control limits.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric direct argument | Text as direct arguments causes error. Use cell references with text to ignore it. |
| `#DIV/0!` | No numeric values | Range contains no numbers. Check data. |
| `#NAME?` | Misspelled function | Check spelling: STDEVP or STDEV.P (with dot for Excel 2010+). |
| `Wrong function choice` | Sample vs population confusion | Use STDEVP for populations, STDEV.S for samples. |
| `Unexpected 0` | All values identical | Zero variation produces SD of 0. This is correct. |

## Use Cases

### [[Complete Population Analysis]]

**Scenario:** Calculate standard deviation for exhaustive data sets.

**Implementation:**
```
=STDEVP(AllEmployeeTenure)
=STDEVP(EntireInventoryValues)
=STDEVP(AllTransactionAmounts)
```

**Business Application:** HR analytics, inventory management, complete financial records.

**Technical Details:** When data is exhaustive, STDEVP gives the true population parameter, not an estimate. No sampling error exists.

---

### [[Manufacturing Quality Control]]

**Scenario:** Measure process variation for entire production runs.

**Implementation:**
```
=STDEVP(BatchMeasurements)
=STDEVP(ProductDimensions)
UCL = AVERAGE(Data) + 3*STDEVP(Data)
LCL = AVERAGE(Data) - 3*STDEVP(Data)
```

**Business Application:** Process capability, control charts, quality assurance.

**Technical Details:** When measuring 100% of production (not sampling), STDEVP provides exact process variation.

---

### [[Academic Grading Analysis]]

**Scenario:** Analyze grade distributions for complete classes.

**Implementation:**
```
=STDEVP(ClassGrades)
=STDEVP(ExamScores)
Grade_Curve = (Score - AVERAGE(Scores))/STDEVP(Scores)
```

**Business Application:** Course analysis, grade curve adjustments, assessment evaluation.

**Technical Details:** A class is a complete population. STDEVP gives exact variation; STDEV.S would overestimate.

---

### [[Portfolio Risk (Historical)]]]

**Scenario:** Calculate historical volatility from complete return series.

**Implementation:**
```
=STDEVP(HistoricalReturns)
=STDEVP(DailyPriceChanges)
Annualized = STDEVP(DailyReturns) * SQRT(252)
```

**Business Application:** Risk reporting, performance attribution, historical analysis.

**Technical Details:** For historical analysis (the actual past returns), STDEVP is appropriate. For forecasting, sample-based estimates may be preferred.

## Platform Differences

### Microsoft Excel
- **Availability:** STDEVP in all versions; STDEV.P added in Excel 2010
- **Recommendation:** Use STDEV.P for new workbooks (clearer naming)
- **Legacy:** STDEVP maintained for backward compatibility
- **Behavior:** Both identical mathematically

### Google Sheets
- **Availability:** Both STDEVP and STDEV.P supported
- **Performance:** Efficient for large datasets
- **Compatibility:** Full Excel compatibility

### Both Platforms
- Identical mathematical formula: sqrt(sum((x-mean)^2)/n)
- Same handling of text/logical in ranges (ignored)
- Same error conditions
- Minimum 1 data point required

## Tips and Best Practices

1. **Know your data type:** Population (complete data) = STDEVP. Sample (partial data) = STDEV.S. This choice matters for small n.

2. **Use STDEV.P for clarity:** The dot notation makes it clearer you're using population formula. STDEVP and STDEV.P are identical.

3. **Large n approximation:** For n > 30, the difference between STDEVP and STDEV.S becomes small. For n > 100, often negligible.

4. **Don't mix functions:** If using STDEVP, also use VAR.P (not VAR.S) for variance. Be consistent within analysis.

5. **Relationship with variance:** STDEVP = sqrt(VAR.P). Variance is SD squared.

6. **Coefficient of variation:** STDEVP/AVERAGE gives relative variability, useful for comparing different-scale data.

7. **Text handling:** STDEVP ignores text in ranges. If text should count as 0, use STDEVPA instead.

8. **Zero SD interpretation:** If STDEVP returns 0, all values are identical—no variation exists in the data.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[STDEV.S]] | Sample standard deviation | When data is a sample, not complete population |
| [[STDEVA]] | Sample SD with text/logical conversion | When text/logical should be converted to 0/1 |
| [[STDEVPA]] | Population SD with text/logical conversion | For population data with text/logical inclusion |
| [[VAR.P]] | Population variance | When you need squared measure of spread |

### Commonly Used Together

**[[AVERAGE]]** - Calculate mean

*Basic population statistics:*
```
Mean: =AVERAGE(Data)
SD:   =STDEVP(Data)
```
Together describe center and spread of population.

---

**[[VAR.P]]** - Population variance

*SD-variance relationship:*
```
=STDEVP(Data)^2  → Should equal VAR.P(Data)
=SQRT(VAR.P(Data)) → Should equal STDEVP(Data)
```
Variance is standard deviation squared.

---

**[[STDEV.S]]** - Compare sample vs population

*Understand the difference:*
```
Population: =STDEVP(Data)
Sample:     =STDEV.S(Data)
Ratio:      =STDEVP(Data)/STDEV.S(Data) = sqrt((n-1)/n)
```
Ratio approaches 1 as n increases.

---

**[[STANDARDIZE]]** - Z-scores using population SD

*Population-based standardization:*
```
=STANDARDIZE(x, AVERAGE(Data), STDEVP(Data))
```
Use STDEVP for z-scores when data is complete population.

---

**[[COUNT]]** - Sample size

*Check n for SD choice:*
```
n = COUNT(Data)
Population SD: =STDEVP(Data)
For small n, choice between STDEVP and STDEV.S matters more.
```

## Official Documentation

- **Microsoft Excel:** [STDEV.P function](https://support.microsoft.com/en-us/office/stdev-p-function-6e917c05-31a0-496f-ade7-4f4e7462f285)
- **Google Sheets:** [STDEVP function](https://support.google.com/docs/answer/3094105)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original STDEVP |
| Excel 2010 | Added | STDEV.P as newer name |
| Excel 2013+ | Both supported | STDEVP for compatibility |
| Google Sheets | Original launch | Both syntaxes supported |

---

*Last updated: 2026-01-10*
