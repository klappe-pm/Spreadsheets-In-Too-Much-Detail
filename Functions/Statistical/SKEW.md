---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- skewness
- distribution-shape
- asymmetry
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Skewness
- Distribution Asymmetry
- Sample Skewness
tags:
- skewness
- distribution
- asymmetry
- statistics
- shape
- descriptive
---

# SKEW

## Description

**SKEW** calculates the skewness of a distribution—a measure of asymmetry around the mean. A symmetric distribution (like the normal distribution) has skewness of 0. Positive skewness indicates a longer right tail (data extends further above the mean), while negative skewness indicates a longer left tail (data extends further below the mean).

Use SKEW to understand the **shape and asymmetry of your data distribution**. In finance, positive skewness in returns means occasional large gains; negative skewness means occasional large losses. In operations, skewed process data suggests systematic factors pushing values in one direction. Skewness helps determine if standard statistical methods (which often assume symmetry) are appropriate.

SKEW returns sample skewness using a bias-corrected formula. This is the third standardized moment: the average of cubed deviations from the mean, divided by the cubed standard deviation, with a correction factor for sample size. The formula produces unbiased estimates from sample data.

**SKEW requires at least 3 data points.** With fewer values, the calculation is undefined and returns #DIV/0!. Also note that skewness is sensitive to outliers—a single extreme value can dramatically affect the result. Always examine your data visually alongside the skewness calculation.

## Syntax

> [!f(x)] SKEW Syntax
>
> ```
> =SKEW(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | First number, cell reference, or range for which to calculate skewness. |
| `number2, ...` | No | Additional numbers, references, or ranges. Up to 255 arguments. Minimum 3 total data points required. |

### Return Value

Returns sample skewness as a single number. Zero indicates symmetry. Positive indicates right skew (right tail longer). Negative indicates left skew (left tail longer). Returns #DIV/0! if fewer than 3 data points or zero variance.

## Examples

> [!f(x)] SKEW Examples

### Example 1: Symmetric Data
```
=SKEW(1, 2, 3, 4, 5)
```
**Result:** 0

**Explanation:** Perfectly symmetric data around mean of 3. No skewness—distribution is balanced.

---

### Example 2: Right-Skewed Data
```
=SKEW(1, 2, 2, 3, 3, 3, 10)
```
**Result:** Positive (approximately 1.5)

**Explanation:** Most values clustered low, one high outlier. Right tail extends further—positive skew.

---

### Example 3: Left-Skewed Data
```
=SKEW(1, 8, 8, 9, 9, 9, 10)
```
**Result:** Negative (approximately -1.5)

**Explanation:** Most values clustered high, one low outlier. Left tail extends further—negative skew.

---

### Example 4: Range of Data
```
=SKEW(A1:A100)
```
**Result:** Skewness of dataset in range

**Explanation:** Standard usage with cell range. Common for analyzing columns of data.

---

### Example 5: Income Distribution (Typical)
```
=SKEW(IncomeData)
```
**Result:** Typically positive

**Explanation:** Income distributions usually show positive skew—many moderate incomes, few very high incomes.

---

### Example 6: Normal Distribution Sample
```
=SKEW(NormalRandomSample)
```
**Result:** Near 0

**Explanation:** Data from normal distribution should have skewness near 0. Large deviation suggests non-normality.

---

### Example 7: Multiple Ranges
```
=SKEW(A1:A50, B1:B50, C1:C50)
```
**Result:** Combined skewness of all data

**Explanation:** Multiple ranges treated as single dataset for skewness calculation.

---

### Example 8: Test Scores (Floor Effect)
```
=SKEW(TestScores) with easy test
```
**Result:** Negative (ceiling effect)

**Explanation:** Easy tests where most score high show negative skew. Difficult tests show positive skew.

---

### Example 9: Manufacturing Process
```
=SKEW(MeasurementData)
```
**Result:** Indicates process bias

**Explanation:** Skewed manufacturing data suggests systematic drift toward one specification limit.

---

### Example 10: Comparing Distributions
```
Distribution A: =SKEW(DataA) → 0.5
Distribution B: =SKEW(DataB) → -0.3
```
**Result:** A is right-skewed, B is left-skewed

**Explanation:** Compare skewness to understand relative asymmetry of different datasets.

---

### Example 11: Financial Returns
```
=SKEW(DailyReturns)
```
**Result:** Varies by asset

**Explanation:** Stock returns often show slight negative skew (occasional large drops). Some assets show positive skew.

---

### Example 12: Minimum Data Points
```
=SKEW(1, 2, 3)
```
**Result:** 0 (for symmetric set)

**Explanation:** Minimum 3 points required. Small samples produce unreliable estimates.

---

### Example 13: Combined Statistics
```
Mean:     =AVERAGE(Data)
StdDev:   =STDEV(Data)
Skewness: =SKEW(Data)
Kurtosis: =KURT(Data)
```
**Result:** Complete distribution profile

**Explanation:** Four statistics characterize distribution: location, spread, asymmetry, and tail behavior.

---

### Example 14: Normality Assessment
```
=IF(ABS(SKEW(Data))<0.5, "Approximately symmetric", "Significantly skewed")
```
**Result:** Quick symmetry check

**Explanation:** Rule of thumb: |skewness| < 0.5 suggests reasonable symmetry for many purposes.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Fewer than 3 data points | SKEW requires minimum 3 values. Add more data or use different measure. |
| `#DIV/0!` | Zero variance (all values identical) | If all values equal, skewness undefined. Check data for variation. |
| `#VALUE!` | Non-numeric data | Ensure all values are numbers. Text causes errors. |
| `#NAME?` | Misspelled function | Check spelling: SKEW, not SKEWNESS. |
| `Unreliable result` | Small sample size | Skewness estimates unstable with n < 30. Use larger samples when possible. |
| `Outlier influence` | Single extreme value | One outlier can dominate skewness. Investigate outliers before interpreting. |

## Use Cases

### [[Distribution Analysis and Normality Testing]]

**Scenario:** Assess whether data is approximately normal or significantly skewed.

**Implementation:**
```
=SKEW(Data)
=IF(AND(ABS(SKEW(Data))<1, ABS(KURT(Data))<1), "Near normal", "Non-normal")
=IF(ABS(SKEW(Data))>2, "Highly skewed", "Moderate or symmetric")
```

**Business Application:** Statistical testing, model validation, distribution fitting. Many statistical tests assume symmetry or normality.

**Technical Details:** For normal distribution, skewness = 0. Values between -0.5 and 0.5 often considered approximately symmetric. Beyond 1 or -1 suggests significant skewness.

---

### [[Financial Risk Analysis]]

**Scenario:** Understand asymmetry in returns and risk.

**Implementation:**
```
=SKEW(Returns)
=IF(SKEW(Returns)<-1, "High downside risk", "Moderate risk profile")
=SKEW(PortfolioReturns) vs SKEW(BenchmarkReturns)
```

**Business Application:** Portfolio management, risk assessment, investment selection. Negative skew indicates tail risk (occasional large losses).

**Technical Details:** Investors generally prefer positive skew (occasional large gains). Negative skew in returns indicates "fat left tail"—crash risk.

---

### [[Quality Control and Process Analysis]]

**Scenario:** Detect systematic process drift or bias.

**Implementation:**
```
=SKEW(ProcessMeasurements)
=IF(ABS(SKEW(Data))>1, "Process may be biased", "Process centered")
=SKEW(BatchWeights)  → Detect overfill or underfill tendency
```

**Business Application:** Manufacturing QC, process improvement, specification compliance. Skewed process data suggests correctable bias.

**Technical Details:** Symmetric process variation is typically random. Skewness suggests systematic cause—investigate and correct.

---

### [[Survey and Response Analysis]]

**Scenario:** Understand response patterns and ceiling/floor effects.

**Implementation:**
```
=SKEW(SatisfactionScores)
=IF(SKEW(Ratings)<-1, "Ceiling effect present", "")
=IF(SKEW(Ratings)>1, "Floor effect present", "")
```

**Business Application:** Customer feedback, employee surveys, rating analysis. Skewness reveals response patterns.

**Technical Details:** Negative skew in satisfaction surveys indicates ceiling effect (most responses at maximum). May indicate need for refined scale.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Formula:** Uses bias-corrected sample skewness
- **Minimum data:** Requires 3+ points
- **Precision:** Full floating-point precision

### Google Sheets
- **Availability:** All versions
- **Formula:** Identical to Excel
- **Behavior:** Same calculation and requirements
- **Performance:** Efficient for large datasets

### Both Platforms
- Identical mathematical formula
- Same error conditions
- Same minimum data requirement (n >= 3)
- Text and logical values in ranges ignored

### Formula Used
Both platforms calculate:
```
n/((n-1)(n-2)) * Sum[(xi-mean)^3]/s^3
```
Where n is sample size and s is sample standard deviation.

## Tips and Best Practices

1. **Ensure adequate sample size:** Skewness estimates are unreliable for small samples. Use 30+ data points for reasonable estimates.

2. **Interpret sign correctly:** Positive = right-skewed (long right tail). Negative = left-skewed (long left tail).

3. **Watch for outliers:** Single extreme values dramatically affect skewness. Investigate outliers before drawing conclusions.

4. **Combine with kurtosis:** Use both SKEW and KURT for complete distribution shape analysis.

5. **Consider context:** Some distributions are naturally skewed (income, insurance claims). Skewness isn't always "bad."

6. **Use visual verification:** Always plot histograms or box plots alongside numerical skewness. Visual inspection complements statistics.

7. **Know rule-of-thumb thresholds:** |Skew| < 0.5: approximately symmetric. |Skew| 0.5-1: moderately skewed. |Skew| > 1: highly skewed.

8. **For populations, use SKEW.P:** SKEW is for samples. If data represents entire population, SKEW.P provides population skewness.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SKEW.P]] | Population skewness | When data is entire population, not sample |
| [[KURT]] | Kurtosis (tail weight) | When concerned about tail behavior, not asymmetry |
| [[STDEV]] | Standard deviation | When concerned about spread, not shape |
| [[AVERAGE]] | Arithmetic mean | When concerned about central tendency |

### Commonly Used Together

**[[KURT]]** - Complete shape analysis

*Distribution shape profile:*
```
Skewness: =SKEW(Data)   → Asymmetry
Kurtosis: =KURT(Data)   → Tail weight
```
Together characterize distribution shape beyond mean and variance.

---

**[[AVERAGE]]** and **[[STDEV]]** - Full descriptive statistics

*Four moments:*
```
Mean:     =AVERAGE(Data)  → Location
Std Dev:  =STDEV(Data)    → Spread
Skewness: =SKEW(Data)     → Asymmetry
Kurtosis: =KURT(Data)     → Tails
```
Complete distribution characterization.

---

**[[SKEW.P]]** - Population version

*Sample vs population:*
```
Sample:     =SKEW(Data)
Population: =SKEW.P(Data)
```
Choose based on whether data is sample or complete population.

---

**[[PERCENTILE]]** - Verify skewness direction

*Compare median to mean:*
```
Mean > Median → Positive skew
Mean < Median → Negative skew
=AVERAGE(Data) - MEDIAN(Data)  → Positive if right-skewed
```
Mean-median relationship reflects skewness direction.

---

**[[COUNT]]** - Verify sample size

*Check before interpreting:*
```
=IF(COUNT(Data)>=30, SKEW(Data), "Insufficient data")
```
Ensure adequate sample size for reliable skewness.

## Official Documentation

- **Microsoft Excel:** [SKEW function](https://support.microsoft.com/en-us/office/skew-function-bdf49d86-b1ef-4804-a046-28eaea69c9fa)
- **Google Sheets:** [SKEW function](https://support.google.com/docs/answer/3094085)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2003 | Original implementation |
| Excel 2013 | Added SKEW.P | Population version added |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
