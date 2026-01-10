---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- kurtosis
- distribution-shape
- tail-analysis
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Kurtosis
- Excess Kurtosis
- Distribution Peakedness
tags:
- kurtosis
- distribution
- statistics
- tail-risk
- outliers
- shape
---

# KURT

## Description

**KURT** calculates the kurtosis of a dataset—a measure of the "tailedness" or extremity of a probability distribution compared to a normal distribution. KURT returns excess kurtosis, meaning a normal distribution has kurtosis of 0. Positive kurtosis indicates heavier tails (more extreme outliers), while negative kurtosis indicates lighter tails (fewer extreme values).

Use KURT to understand the **risk of extreme values in your data**. In finance, high kurtosis ("leptokurtic" distributions) indicates more frequent extreme gains and losses than a normal distribution would predict. In quality control, kurtosis helps identify whether process variation comes from occasional extreme events or consistent moderate variation. It's a key metric for risk assessment and distribution fitting.

Kurtosis is often misunderstood as measuring "peakedness," but modern statistics emphasizes its role in measuring tail behavior. A distribution with high kurtosis has heavier tails—more probability mass far from the mean. This matters for risk management: if returns have high kurtosis, extreme events (crashes, windfalls) occur more often than normal distribution models predict.

**KURT requires at least 4 data points.** With fewer values, the calculation is mathematically undefined and returns #DIV/0!. Also note that KURT calculates sample excess kurtosis using a bias-corrected formula, not population kurtosis. For small samples, kurtosis estimates can be unreliable—interpret with caution when n < 30.

## Syntax

> [!f(x)] KURT Syntax
>
> ```
> =KURT(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first number, cell reference, or range of values for which to calculate kurtosis. |
| `number2, ...` | No | Additional numbers, cell references, or ranges. Up to 255 arguments total. Must have at least 4 total data points. |

### Return Value

Returns a single numeric value representing the excess kurtosis of the dataset. Value of 0 indicates normal distribution tail behavior. Positive values indicate heavier tails (leptokurtic). Negative values indicate lighter tails (platykurtic). Returns #DIV/0! if fewer than 4 data points. Returns #VALUE! for non-numeric data.

## Examples

> [!f(x)] KURT Examples

### Example 1: Normal Distribution Data
```
=KURT(2.5, 3.5, 4, 4.5, 5, 5.5, 6, 6.5, 7.5)
```
**Result:** Approximately 0

**Explanation:** Data approximating a normal distribution yields kurtosis near 0, indicating typical tail behavior.

---

### Example 2: Range of Values
```
=KURT(A1:A100)
```
**Result:** Kurtosis of the dataset in A1:A100

**Explanation:** Standard usage with a cell range. Ensure at least 4 numeric values exist in the range.

---

### Example 3: High Kurtosis (Heavy Tails)
```
=KURT(0, 0, 0, 10, 0, 0, 0, -10)
```
**Result:** Positive value (approximately 2.0)

**Explanation:** Data with extreme values surrounded by many similar values produces positive kurtosis—heavy tails, indicating outlier risk.

---

### Example 4: Low Kurtosis (Light Tails)
```
=KURT(1, 2, 3, 4, 5, 6, 7, 8)
```
**Result:** Negative value (approximately -1.2)

**Explanation:** Uniform-like distributions have negative excess kurtosis—lighter tails than normal, fewer extreme values.

---

### Example 5: Stock Returns Analysis
```
=KURT(DailyReturns)
```
**Result:** Typically positive for financial data

**Explanation:** Stock returns famously exhibit positive kurtosis—extreme moves happen more often than normal distribution predicts.

---

### Example 6: Multiple Arguments
```
=KURT(A1:A50, B1:B50, C1:C50)
```
**Result:** Combined kurtosis of all three ranges

**Explanation:** Multiple ranges are combined into a single dataset for kurtosis calculation.

---

### Example 7: Quality Control Measurement
```
=KURT(Measurements)
```
**Result:** Indicates process stability

**Explanation:** High kurtosis in manufacturing data suggests occasional extreme deviations—possible equipment issues or special cause variation.

---

### Example 8: Comparing Distribution Shapes
```
=KURT(Dataset1) vs =KURT(Dataset2)
```
**Result:** Relative tail heaviness comparison

**Explanation:** Dataset with higher kurtosis has more extreme values. Useful for comparing risk profiles.

---

### Example 9: Minimum Data Points (4)
```
=KURT(1, 2, 3, 4)
```
**Result:** -1.2 (exactly)

**Explanation:** Minimum required data points. With only 4 values, estimates are highly variable—use larger samples for reliable results.

---

### Example 10: Insurance Claim Analysis
```
=KURT(ClaimAmounts)
```
**Result:** Typically high positive kurtosis

**Explanation:** Insurance claims often show extreme kurtosis—many small claims, occasional catastrophic ones. High kurtosis signals tail risk.

---

### Example 11: Survey Score Distribution
```
=KURT(SurveyScores)
```
**Result:** Varies by response pattern

**Explanation:** Survey data with extreme responses (many 1s and 5s, few 3s) shows positive kurtosis. Moderate responses show negative kurtosis.

---

### Example 12: Error Distribution Analysis
```
=KURT(PredictionErrors)
```
**Result:** Indicates error behavior

**Explanation:** Positive kurtosis in model errors suggests occasional large misses—model may underestimate tail risk.

---

### Example 13: Combined with Other Statistics
```
Mean:     =AVERAGE(Data)
StdDev:   =STDEV(Data)
Skewness: =SKEW(Data)
Kurtosis: =KURT(Data)
```
**Result:** Complete distribution profile

**Explanation:** The four moments together fully characterize a distribution: location (mean), spread (std dev), asymmetry (skewness), and tail behavior (kurtosis).

---

### Example 14: Detecting Non-Normality
```
=IF(ABS(KURT(Data))>1, "Non-normal tails", "Approximately normal")
```
**Result:** Quick normality assessment

**Explanation:** Kurtosis significantly different from 0 (>1 or <-1 as rule of thumb) suggests non-normal distribution.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Fewer than 4 data points | KURT requires at least 4 values. Add more data or use a different measure. |
| `#DIV/0!` | Zero variance (all values identical) | If all values are the same, kurtosis is undefined. Check data for variation. |
| `#VALUE!` | Non-numeric data | Ensure all values are numbers. Text and errors in ranges cause calculation failure. |
| `#NAME?` | Misspelled function | Check spelling: KURT, not KURTOSIS or KURTOSIS. |
| `Unreliable result` | Small sample size | Kurtosis estimates are unstable with small n. Use 30+ data points for reliable results. |
| `Unexpected sign` | Confusion about excess vs raw kurtosis | KURT returns excess kurtosis (normal = 0). Add 3 if you need raw kurtosis (normal = 3). |

## Use Cases

### [[Financial Risk Assessment]]

**Scenario:** Evaluate tail risk in investment returns.

**Implementation:**
```
=KURT(DailyReturns)
=KURT(MonthlyPortfolioReturns)
=IF(KURT(Returns)>3, "Extreme tail risk", "Moderate tail risk")
```

**Business Application:** Portfolio risk management, VaR modeling, derivative pricing. High kurtosis means normal distribution underestimates extreme event probability.

**Technical Details:** Financial returns typically show kurtosis of 3-10 or higher. This "fat tail" phenomenon invalidates many models assuming normal distributions. Black-Scholes, basic VaR models, and other tools systematically underestimate tail risk.

---

### [[Quality Control and Process Monitoring]]

**Scenario:** Assess whether process variation includes unusual extreme values.

**Implementation:**
```
=KURT(ProcessMeasurements)
=KURT(DefectSizes)
=KURT(BatchWeights)
```

**Business Application:** Manufacturing quality, pharmaceutical production, food processing. High kurtosis signals special cause variation—investigate root causes.

**Technical Details:** Control charts assume normal distribution. High kurtosis data produces more out-of-control signals than expected. Consider kurtosis when setting control limits.

---

### [[Insurance and Actuarial Analysis]]

**Scenario:** Model claim severity distributions for pricing and reserving.

**Implementation:**
```
=KURT(ClaimSeverities)
=KURT(HistoricalLosses)
=IF(KURT(Claims)>5, "Heavy-tailed", "Moderate-tailed")
```

**Business Application:** Insurance pricing, reinsurance treaties, catastrophe modeling. High kurtosis indicates potential for extreme losses.

**Technical Details:** Actuaries often use distributions explicitly designed for high kurtosis (Pareto, log-normal, Weibull) rather than assuming normality. KURT helps identify appropriate distribution families.

---

### [[Scientific Data Analysis]]

**Scenario:** Test distributional assumptions for statistical procedures.

**Implementation:**
```
=KURT(ExperimentalData)
=IF(AND(ABS(SKEW(Data))<1, ABS(KURT(Data))<1), "Approximately normal", "Non-normal")
=KURT(ResidualErrors)
```

**Business Application:** Research data analysis, regression diagnostics, hypothesis testing. Many statistical tests assume normality—kurtosis helps verify this.

**Technical Details:** Combined tests using both skewness and kurtosis (like Jarque-Bera) provide formal normality tests. Significant non-normality may require data transformation or non-parametric methods.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 2003
- **Formula:** Uses bias-corrected sample excess kurtosis formula
- **Array handling:** Accepts multiple ranges; ignores text, logical values, and empty cells in ranges
- **Precision:** Full floating-point precision

### Google Sheets
- **Availability:** All versions
- **Formula:** Identical to Excel—sample excess kurtosis
- **Array handling:** Same behavior as Excel
- **Performance:** Efficient for typical dataset sizes

### Both Platforms
- Return excess kurtosis (normal distribution = 0)
- Use identical bias-corrected sample formula
- Require minimum 4 data points
- Ignore non-numeric values in ranges
- Return #DIV/0! for insufficient data or zero variance

### Formula Used
Both platforms calculate:
```
n(n+1)/((n-1)(n-2)(n-3)) * Sum[(xi-mean)^4]/s^4 - 3(n-1)^2/((n-2)(n-3))
```
Where n is sample size and s is sample standard deviation.

## Tips and Best Practices

1. **Ensure adequate sample size:** Kurtosis estimates are notoriously unstable for small samples. Use at least 30 data points; 100+ is better for reliable estimates.

2. **Interpret excess kurtosis correctly:** KURT returns excess kurtosis. Normal = 0. Positive = heavier tails. Negative = lighter tails. Don't confuse with raw kurtosis where normal = 3.

3. **Combine with skewness:** Use both SKEW and KURT together for complete distribution shape assessment. A distribution can have normal kurtosis but extreme skewness, or vice versa.

4. **Watch for outlier sensitivity:** A single extreme value can dramatically increase kurtosis. Investigate outliers before drawing conclusions about underlying distribution.

5. **Consider robust alternatives:** For exploratory analysis, consider comparing distributions visually (histograms, Q-Q plots) alongside kurtosis calculation.

6. **Use for model selection:** High kurtosis suggests using Student's t, Laplace, or other heavy-tailed distributions rather than normal distribution in modeling.

7. **Financial rule of thumb:** Stock returns typically have kurtosis of 3-10. Much higher suggests extreme tail risk or possible data errors.

8. **Don't over-interpret:** Kurtosis is one of many distribution characteristics. High kurtosis doesn't mean data is "bad"—it's information about tail behavior.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SKEW]] | Measures distribution asymmetry | When concerned about lopsided distributions |
| [[SKEW.P]] | Population skewness | When data is entire population |
| [[STDEV]] | Standard deviation | When concerned about overall spread, not tail behavior |
| [[VAR]] | Variance | When computing spread as squared deviation |

### Commonly Used Together

**[[SKEW]]** - Complete shape analysis

*Distribution shape profile:*
```
Skewness: =SKEW(Data)
Kurtosis: =KURT(Data)
```
Together, skewness and kurtosis characterize distribution shape beyond mean and variance.

---

**[[AVERAGE]]** and **[[STDEV]]** - Complete descriptive statistics

*Four moments of distribution:*
```
Mean:       =AVERAGE(Data)
Std Dev:    =STDEV(Data)
Skewness:   =SKEW(Data)
Kurtosis:   =KURT(Data)
```
The first four moments fully characterize most distributions.

---

**[[NORM.DIST]]** - Compare to normal assumption

*Test normality impact:*
```
If KURT is high, normal distribution underestimates:
- P(extreme loss) is higher than NORM.DIST predicts
- Tail probabilities need adjustment
```

---

**[[PERCENTILE]]** - Examine actual tail values

*Compare percentiles:*
```
=PERCENTILE(Data, 0.01)  → Actual 1st percentile
=PERCENTILE(Data, 0.99)  → Actual 99th percentile
```
High kurtosis means extreme percentiles are further from mean than normal predicts.

---

**[[COUNT]]** - Verify sample size

*Ensure adequate data:*
```
=IF(COUNT(Data)>=30, KURT(Data), "Insufficient data")
```
Check sample size before trusting kurtosis estimate.

## Official Documentation

- **Microsoft Excel:** [KURT function](https://support.microsoft.com/en-us/office/kurt-function-bc3a265c-5da4-4dcb-b7fd-c237789095ab)
- **Google Sheets:** [KURT function](https://support.google.com/docs/answer/3094009)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2003 | Original implementation |
| Excel 2007+ | All subsequent | No formula changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
