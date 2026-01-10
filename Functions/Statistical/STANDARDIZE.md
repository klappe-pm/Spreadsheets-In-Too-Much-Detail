---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- z-score
- normalization
- standard-score
- data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Z-Score
- Standard Score
- Normalization
tags:
- z-score
- standardize
- normalize
- statistics
- transformation
- distribution
---

# STANDARDIZE

## Description

**STANDARDIZE** converts a value to a z-score (standard score) by calculating how many standard deviations the value lies from the mean. The formula is z = (x - mean) / standard_deviation. A z-score of 0 means the value equals the mean; positive z-scores indicate above-average values; negative z-scores indicate below-average values.

Use STANDARDIZE to **compare values from different distributions** on a common scale. A score of 85 in one test and 720 in another are difficult to compare directly, but their z-scores reveal which represents better relative performance. Z-scores are fundamental for statistical analysis, probability calculations, quality control, and data normalization.

Z-scores transform any normal distribution to the standard normal distribution (mean = 0, standard deviation = 1). This enables using standard normal tables and functions like NORM.S.DIST for probability calculations. For non-normal distributions, z-scores still measure relative position but probability interpretations require caution.

**STANDARDIZE requires non-zero standard deviation.** A standard deviation of 0 means all values are identical, and z-scores become undefined (division by zero). The function returns #NUM! if standard deviation is 0 or negative, and #VALUE! for non-numeric inputs.

## Syntax

> [!f(x)] STANDARDIZE Syntax
>
> ```
> =STANDARDIZE(x, mean, standard_dev)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The value to standardize. Any numeric value. |
| `mean` | Yes | The arithmetic mean of the distribution. |
| `standard_dev` | Yes | The standard deviation of the distribution. Must be positive (> 0). |

### Return Value

Returns the z-score (standardized value) of x. Can be any real number. Positive if x > mean. Negative if x < mean. Zero if x = mean. Returns #NUM! if standard_dev <= 0. Returns #VALUE! for non-numeric inputs.

## Examples

> [!f(x)] STANDARDIZE Examples

### Example 1: Basic Z-Score Calculation
```
=STANDARDIZE(75, 70, 10)
```
**Result:** 0.5

**Explanation:** 75 is half a standard deviation above the mean of 70. z = (75-70)/10 = 0.5.

---

### Example 2: Value at Mean
```
=STANDARDIZE(100, 100, 15)
```
**Result:** 0

**Explanation:** When x equals the mean, z-score is always 0.

---

### Example 3: Below Average Value
```
=STANDARDIZE(85, 100, 15)
```
**Result:** -1

**Explanation:** 85 is exactly one standard deviation below the mean of 100.

---

### Example 4: Extreme Value
```
=STANDARDIZE(145, 100, 15)
```
**Result:** 3

**Explanation:** 145 is three standard deviations above the mean—an unusually high value.

---

### Example 5: IQ Score Standardization
```
=STANDARDIZE(130, 100, 15)
```
**Result:** 2

**Explanation:** An IQ of 130 is 2 standard deviations above average (top ~2.3% of population).

---

### Example 6: Test Score Comparison
```
Test A: =STANDARDIZE(85, 75, 10) → 1.0
Test B: =STANDARDIZE(720, 650, 100) → 0.7
```
**Result:** Test A performance was relatively better

**Explanation:** Despite raw scores being incomparable, z-scores reveal Test A score is further above its mean.

---

### Example 7: Using Cell References
```
=STANDARDIZE(A1, B1, C1)
```
**Result:** Z-score using values from cells

**Explanation:** Standard usage with cell references for dynamic calculations.

---

### Example 8: Quality Control
```
=STANDARDIZE(Measurement, TargetMean, ProcessStdDev)
```
**Result:** How far measurement is from target in SD units

**Explanation:** Z-scores >3 or <-3 typically flag out-of-control conditions.

---

### Example 9: Financial Analysis
```
=STANDARDIZE(CurrentReturn, HistoricalMean, HistoricalStdDev)
```
**Result:** How unusual is today's return?

**Explanation:** Standardized returns help identify unusual market movements.

---

### Example 10: Combining with Normal Distribution
```
=NORM.S.DIST(STANDARDIZE(85, 100, 15), TRUE)
```
**Result:** 0.1587 (approximately)

**Explanation:** Standardize then use standard normal CDF to find probability.

---

### Example 11: Percentile Interpretation
```
Z-score of 1.645 → ~95th percentile
Z-score of 0     → ~50th percentile
Z-score of -1    → ~16th percentile
```
**Result:** Z-scores map to percentiles (for normal distributions)

**Explanation:** Standard normal tables convert z-scores to percentiles.

---

### Example 12: Multiple Values
```
Array: {60, 70, 80, 90, 100} with mean=80, SD=10
Z-scores: {-2, -1, 0, 1, 2}
```
**Result:** Each value's position relative to mean

**Explanation:** Evenly spaced values produce evenly spaced z-scores.

---

### Example 13: Decimal Standard Deviation
```
=STANDARDIZE(0.055, 0.05, 0.01)
```
**Result:** 0.5

**Explanation:** Works with any scale. 0.055 is half an SD above mean of 0.05.

---

### Example 14: Reverse Calculation
```
To find x from z: x = mean + z * standard_dev
=100 + 2*15 → 130
```
**Result:** Converting z-score back to original scale

**Explanation:** STANDARDIZE's inverse is: mean + z * SD.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Standard deviation is 0 | All values identical means z-scores undefined. Check for data variation. |
| `#NUM!` | Negative standard deviation | Standard deviation must be positive. Use ABS or check calculation. |
| `#VALUE!` | Non-numeric input | Ensure x, mean, and standard_dev are all numbers. |
| `#NAME?` | Misspelled function | Check spelling: STANDARDIZE (American spelling, with Z). |
| `Unexpected result` | Wrong mean/SD parameters | Verify mean and SD are from the correct distribution for x. |

## Use Cases

### [[Test Score Comparison]]

**Scenario:** Compare performance across different tests or scales.

**Implementation:**
```
=STANDARDIZE(Score, TestMean, TestSD)
=STANDARDIZE(SAT, 1050, 200) vs =STANDARDIZE(ACT, 21, 5)
=IF(STANDARDIZE(ScoreA,MeanA,SDA)>STANDARDIZE(ScoreB,MeanB,SDB), "A better", "B better")
```

**Business Application:** Educational assessment, hiring decisions, performance evaluation. Compare apples to oranges using z-scores.

**Technical Details:** Z-scores enable fair comparison regardless of original scale. Someone with z=1.5 on any test is at approximately the same percentile.

---

### [[Quality Control (Control Charts)]]

**Scenario:** Detect unusual measurements in manufacturing processes.

**Implementation:**
```
=STANDARDIZE(Measurement, ProcessMean, ProcessSD)
=IF(ABS(STANDARDIZE(X, Mean, SD))>3, "Out of control", "In control")
=STANDARDIZE(BatchWeight, TargetWeight, HistoricalSD)
```

**Business Application:** Manufacturing QC, process monitoring, Six Sigma. Identify when measurements exceed normal variation.

**Technical Details:** The 3-sigma rule: z-scores beyond +/-3 occur only 0.3% of the time by chance. Flag these for investigation.

---

### [[Financial Risk Assessment]]

**Scenario:** Measure how unusual returns or prices are compared to historical norms.

**Implementation:**
```
=STANDARDIZE(TodayReturn, HistoricalMean, HistoricalVolatility)
=IF(STANDARDIZE(Return, Mean, StdDev)<-2, "Significant loss", "Normal")
=ABS(STANDARDIZE(Price, MovingAvg, RollingStdDev))  → Distance from trend
```

**Business Application:** Risk management, anomaly detection, trading signals. Quantify market surprises.

**Technical Details:** In finance, z-scores help identify statistically unusual moves. Returns with |z| > 2 warrant attention; |z| > 3 is highly unusual.

---

### [[Data Normalization for Analysis]]

**Scenario:** Prepare data for analysis methods requiring standardized inputs.

**Implementation:**
```
=STANDARDIZE(Value, AVERAGE(Range), STDEV(Range))
Apply to each value in dataset for standardized version
Use for regression, clustering, or machine learning
```

**Business Application:** Statistical modeling, machine learning preprocessing, multivariate analysis.

**Technical Details:** Many algorithms work better with standardized inputs. Variables on different scales get equal weight when standardized.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Spelling:** STANDARDIZE (American English, with Z)
- **Error handling:** Returns #NUM! for SD <= 0
- **Precision:** Full floating-point precision

### Google Sheets
- **Availability:** All versions
- **Spelling:** Same as Excel (STANDARDIZE)
- **Behavior:** Identical calculation
- **Performance:** Efficient for all inputs

### Both Platforms
- Identical mathematical formula: (x - mean) / standard_dev
- Same error conditions
- Same parameter requirements
- No array functionality (single value only)

## Tips and Best Practices

1. **Use correct population vs sample SD:** If calculating SD yourself, be consistent. STDEV for sample, STDEV.P for population.

2. **Interpret magnitude, not just sign:** |z| < 1 is within one SD (common). |z| > 2 is unusual. |z| > 3 is very rare.

3. **Normal distribution context:** Z-score to probability conversion (via NORM.S.DIST) assumes normality. Non-normal data gives approximate interpretations.

4. **Compare fairly:** Only compare z-scores from the same or similar distributions. Z-score of 2 means different things for normal vs heavy-tailed distributions.

5. **Reverse the calculation:** To go from z-score back to original scale: x = mean + z * SD.

6. **Verify your mean and SD:** Ensure mean and standard deviation are calculated from the same reference distribution your value x belongs to.

7. **Handle outliers:** Extreme z-scores (|z| > 3) may indicate data errors or genuine anomalies—investigate before dismissing.

8. **Alternative formula:** STANDARDIZE(x, mean, sd) = (x - mean) / sd. You can write this directly if preferred.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[NORM.S.DIST]] | Standard normal CDF | When you need probability from z-score |
| [[NORM.S.INV]] | Inverse standard normal | When you need z-score from probability |
| [[AVERAGE]] | Calculate mean | When you need to compute mean for STANDARDIZE |
| [[STDEV]] | Calculate standard deviation | When you need to compute SD for STANDARDIZE |

### Commonly Used Together

**[[NORM.S.DIST]]** - Convert z-score to probability

*Full standardization workflow:*
```
z = STANDARDIZE(x, mean, sd)
P(X < x) = NORM.S.DIST(z, TRUE)
```
First standardize, then find cumulative probability.

---

**[[NORM.S.INV]]** - Reverse process

*From probability to original scale:*
```
z = NORM.S.INV(probability)
x = mean + z * sd
```
Find what value corresponds to a given percentile.

---

**[[AVERAGE]]** and **[[STDEV]]** - Calculate parameters

*Dynamic standardization:*
```
=STANDARDIZE(A1, AVERAGE(DataRange), STDEV(DataRange))
```
Compute mean and SD from data rather than specifying manually.

---

**[[PHI]]** - Probability density at z-score

*Density function:*
```
z = STANDARDIZE(x, mean, sd)
density = PHI(z)
```
Find relative likelihood of standardized value.

---

**[[ABS]]** - Distance from mean

*Absolute deviation in SD units:*
```
=ABS(STANDARDIZE(x, mean, sd))
```
How far from mean, regardless of direction.

## Official Documentation

- **Microsoft Excel:** [STANDARDIZE function](https://support.microsoft.com/en-us/office/standardize-function-81d66554-2d54-40ec-ba83-6437108ee775)
- **Google Sheets:** [STANDARDIZE function](https://support.google.com/docs/answer/3094090)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original implementation |
| Excel 2007+ | All subsequent | No changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
