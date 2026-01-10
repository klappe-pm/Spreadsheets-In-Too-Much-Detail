---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- average-deviation
- variability
- dispersion
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Average Deviation
- Mean Deviation
- Average Absolute Deviation
- AAD
tags:
- statistical
- variability
- dispersion
- deviation
- descriptive-statistics
---

# AVEDEV

## Description

**AVEDEV** calculates the average of the absolute deviations of data points from their arithmetic mean, providing a measure of variability or dispersion in a dataset. For each value in your data, the function computes how far it is from the mean (ignoring whether it is above or below), then averages all these distances. This produces a single number that tells you, on average, how much your data points deviate from the center. Unlike variance or standard deviation, AVEDEV uses absolute deviations rather than squared deviations, making it more intuitive to interpret in the original units of measurement.

The mathematical formula for AVEDEV is straightforward: first calculate the arithmetic mean of all values, then for each value compute the absolute difference from this mean, and finally average all these absolute differences. Mathematically: AVEDEV = (1/n) * SUM(|xi - mean|). This approach avoids the squaring operation used in variance calculations, which means outliers have less exaggerated influence on the result. A dataset with AVEDEV = 5 means that, on average, each data point is 5 units away from the mean, which is directly interpretable in whatever units your data uses (dollars, seconds, meters, etc.).

**Important caveat:** While AVEDEV is intuitive, it is less commonly used in formal statistical analysis than standard deviation. Standard deviation has better mathematical properties for inference, hypothesis testing, and confidence intervals. AVEDEV does not follow the rules of normal distribution theory as elegantly. However, for descriptive purposes and communicating variability to non-statistical audiences, AVEDEV's direct interpretability makes it valuable. If someone asks "how spread out is my data?" the answer "on average, 12 dollars from the mean" is immediately understandable without explaining squared deviations.

**Platform behavior:** AVEDEV behaves identically in Excel and Google Sheets. Both platforms ignore text values, logical values (TRUE/FALSE), and empty cells within ranges. Only numeric values contribute to the calculation. If you need to include text representations of numbers or logical values, you must convert them explicitly before passing to AVEDEV. The function requires at least one numeric value to return a result; otherwise, it returns a #DIV/0! error.

## Syntax

> [!f(x)] AVEDEV Syntax
>
> ```
> =AVEDEV(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first number, cell reference, or range for which you want the average deviation. Can be a single cell, a range like A1:A100, or a numeric constant. |
| `number2, ...` | No | Additional numbers, cell references, or ranges to include. Up to 255 arguments in Excel, virtually unlimited in Google Sheets when using ranges. |

### Return Value

Returns a non-negative numeric value representing the average absolute deviation from the mean. The result is always zero or positive since absolute values are used. Returns #DIV/0! if no numeric values are provided.

## Examples

> [!f(x)] AVEDEV Examples

### Example 1: Basic Calculation with Uniform Data
```
=AVEDEV(10, 10, 10, 10, 10)
```
**Result:** 0

**Explanation:** When all values are identical, the mean equals each value, so every deviation is zero. The average of all zeros is zero. Zero AVEDEV indicates no variability whatsoever.

---

### Example 2: Simple Dataset with Variability
```
=AVEDEV(2, 4, 6, 8, 10)
```
**Result:** 2.4

**Explanation:** Mean is 6. Deviations are |2-6|=4, |4-6|=2, |6-6|=0, |8-6|=2, |10-6|=4. Average of (4+2+0+2+4)/5 = 12/5 = 2.4. On average, values are 2.4 units from the mean.

---

### Example 3: Cell Range Reference
```
=AVEDEV(A2:A11)
```
Where A2:A11 contains: 85, 92, 78, 95, 88, 76, 91, 84, 79, 90

**Result:** 5.48

**Explanation:** The test scores have a mean of 85.8. On average, each score deviates about 5.48 points from this mean, indicating moderate spread in performance.

---

### Example 4: Multiple Separate Ranges
```
=AVEDEV(A2:A10, C2:C10, E2:E10)
```
**Result:** Combined average deviation across all three ranges

**Explanation:** AVEDEV combines all values from multiple ranges into one calculation, computing a single mean and then averaging all absolute deviations from that combined mean.

---

### Example 5: Mixed Numbers and Ranges
```
=AVEDEV(A1:A5, 100, B1:B3)
```
**Result:** Average deviation including the constant 100

**Explanation:** You can mix cell ranges with explicit numeric values. The constant 100 is included in both the mean calculation and the deviation calculation.

---

### Example 6: Data with Text Values (Ignored)
```
=AVEDEV(A2:A10)
```
Where A2:A10 contains: 10, 20, "N/A", 30, 40, "Missing", 50, 60, 70

**Result:** 17.14 (approximately)

**Explanation:** Text values "N/A" and "Missing" are silently ignored. Only the 7 numeric values are used: mean is ~40, and the average absolute deviation from 40 is calculated.

---

### Example 7: Financial Price Volatility
```
=AVEDEV(DailyPrices)
```
Where DailyPrices is a named range of 30 daily stock prices

**Result:** 2.35 (example)

**Explanation:** If the result is $2.35, this means the stock price typically varies $2.35 from its average price over the period, providing an intuitive measure of day-to-day volatility.

---

### Example 8: Comparing Two Datasets' Variability
```
Dataset A: =AVEDEV(A2:A101)  Result: 12.5
Dataset B: =AVEDEV(B2:B101)  Result: 3.2
```
**Result:** Dataset A is more variable

**Explanation:** By comparing AVEDEV values, you can quickly determine which dataset has more spread. Dataset A's values deviate from its mean by 12.5 on average, nearly four times the variability of Dataset B.

---

### Example 9: Quality Control Measurements
```
=AVEDEV(Measurements)
```
Where Measurements contains: 10.02, 10.01, 9.98, 10.03, 9.97, 10.00, 9.99, 10.01, 10.02, 9.98

**Result:** 0.0164

**Explanation:** Manufacturing measurements targeting 10.00 show very low average deviation (0.0164 units), indicating precise, consistent production with minimal variation.

---

### Example 10: Temperature Variation Analysis
```
=AVEDEV(JanuaryTemps)
```
Where JanuaryTemps contains 31 daily high temperatures

**Result:** 4.7 (example)

**Explanation:** An AVEDEV of 4.7 degrees means daily temperatures typically vary about 4.7 degrees from the monthly average, useful for understanding weather consistency.

---

### Example 11: Single Value Edge Case
```
=AVEDEV(42)
```
**Result:** 0

**Explanation:** A single value is both the mean and the only data point. Its deviation from itself is zero. This confirms the function handles edge cases correctly.

---

### Example 12: Relationship to Standard Deviation
```
AVEDEV: =AVEDEV(A2:A100)     Result: 8.5
STDEV:  =STDEV.S(A2:A100)   Result: 10.2
```
**Result:** STDEV is typically larger than AVEDEV

**Explanation:** For normally distributed data, standard deviation is approximately 1.25 times the average deviation. The squared deviations in STDEV give more weight to outliers than the absolute deviations in AVEDEV.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | No numeric values in the arguments | Ensure at least one numeric value is included. Check for all-text or all-empty ranges. |
| `#VALUE!` | Non-numeric text passed directly as argument | Use cell references containing numbers, not text strings like "ten". |
| `#NAME?` | Function name misspelled | Verify spelling is AVEDEV, not AVGDEV or AVERAGEDEV. |
| `0` when unexpected | All values are identical | This is correct behavior. Identical values have zero deviation from their mean. |
| `Result seems too low` | Text or blank cells being ignored | Verify which cells contain actual numbers vs. text-formatted numbers. Use VALUE() to convert. |
| `Unexpected large result` | Outliers in data | Investigate outliers. One extreme value can significantly increase average deviation. |

## Use Cases

### [[Manufacturing Quality Control]]

**Scenario:** A quality engineer monitors the dimensions of manufactured parts to ensure consistency and detect production problems.

**Implementation:**
```
=AVEDEV(PartDimensions)
```
Calculate AVEDEV for each batch of measurements to track production consistency.

**Business Application:** Parts must meet tight tolerances. An AVEDEV of 0.02mm indicates consistent production, while a sudden increase to 0.08mm signals machine drift or material problems. Unlike range (max-min), AVEDEV considers all measurements, not just extremes. Daily AVEDEV tracking creates an early warning system for quality issues before defects reach customers.

**Technical Details:** Set control limits based on historical AVEDEV values. If baseline AVEDEV is 0.02mm, an upper control limit of 0.04mm (2x baseline) triggers investigation. AVEDEV is more stable than range for small samples and easier to explain to production staff than standard deviation.

---

### [[Customer Service Response Time Analysis]]

**Scenario:** A support manager analyzes call center response times to understand service consistency and identify training needs.

**Implementation:**
```
=AVEDEV(ResponseTimes)
```
Compare AVEDEV across agents, shifts, and time periods.

**Business Application:** Two agents might have identical average response times of 3 minutes, but different AVEDEVs. Agent A with AVEDEV of 0.5 minutes is consistently near 3 minutes, while Agent B with AVEDEV of 2 minutes swings between 1 and 5 minutes. The manager can target coaching to improve Agent B's consistency. Customers value predictable service; high variability frustrates them even if averages are acceptable.

**Technical Details:** Calculate AVEDEV by day-of-week to identify patterns. Monday AVEDEV of 3 minutes versus Friday AVEDEV of 1 minute suggests Monday staffing or complexity issues. Combine with percentile analysis for complete picture.

---

### [[Investment Portfolio Volatility Assessment]]

**Scenario:** An investor compares the historical volatility of different assets to make allocation decisions and assess risk.

**Implementation:**
```
Asset A Volatility: =AVEDEV(AssetA_Returns)
Asset B Volatility: =AVEDEV(AssetB_Returns)
```
Calculate on monthly or daily returns.

**Business Application:** AVEDEV provides an intuitive volatility measure. If Asset A has AVEDEV of 2% and Asset B has 5%, Asset A's returns typically stay within 2% of its average return while Asset B swings 5%. For risk-averse investors, lower AVEDEV assets provide more predictable performance. The "average deviation in percentage terms" is easier to communicate than standard deviation to many clients.

**Technical Details:** Use percentage returns, not prices, for meaningful comparisons across different price levels. Calculate rolling AVEDEV (e.g., 60-day) to track changing volatility over time. Remember AVEDEV underweights extreme events compared to standard deviation.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Maximum arguments:** 255 separate arguments
- **Maximum data points:** Limited by worksheet size (over 1 million per range)
- **Empty cell handling:** Ignored in calculations
- **Text handling:** Text values in ranges are ignored
- **Logical handling:** TRUE/FALSE in ranges are ignored
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Maximum arguments:** 30 separate arguments, but ranges can contain unlimited cells
- **Maximum data points:** Limited by sheet size (10 million cells total)
- **Empty cell handling:** Ignored in calculations
- **Text handling:** Text values in ranges are ignored
- **Logical handling:** TRUE/FALSE in ranges are ignored
- **Precision:** 15 significant decimal digits

### Key Difference Alert

AVEDEV behaves identically between Excel and Google Sheets. Both platforms:
- Ignore text, logical values, and empty cells within ranges
- Include numeric values passed directly as arguments
- Return #DIV/0! when no numeric values exist
- Use the same mathematical algorithm

The only practical difference is argument limits (255 in Excel vs. 30 in Sheets for separate arguments), but this rarely matters since ranges are typically used.

## Tips and Best Practices

1. **Use AVEDEV for communication, STDEV for inference.** When explaining variability to non-technical audiences, AVEDEV's "average distance from the mean" is immediately understandable. For statistical tests and confidence intervals, use standard deviation instead.

2. **Compare AVEDEV to the mean for context.** An AVEDEV of 10 means nothing without context. If the mean is 1000, AVEDEV of 10 (1%) indicates tight clustering. If the mean is 20, AVEDEV of 10 (50%) indicates high variability. Consider calculating AVEDEV/AVERAGE as a relative measure.

3. **Verify data types before calculating.** Numbers stored as text are ignored by AVEDEV. Use ISNUMBER() to check values or VALUE() to convert. A sudden drop in AVEDEV might just mean data imported as text.

4. **AVEDEV of zero is meaningful.** Zero indicates all values are identical to the mean, meaning no variability at all. This might indicate data problems (duplicate records) or perfect consistency (rare in real data).

5. **Consider sample size.** AVEDEV from 5 data points is less reliable than from 500. Small samples may not represent true variability. Report sample size alongside AVEDEV for honest communication.

6. **Use for trend analysis.** Track AVEDEV over time to detect changes in consistency. Increasing AVEDEV often signals process degradation before averages shift noticeably.

7. **Combine with AVERAGE for complete picture.** Always report both central tendency (AVERAGE) and dispersion (AVEDEV) together. Knowing "average is 50" is incomplete without knowing whether values typically range from 48-52 or 20-80.

8. **Be aware of outlier sensitivity.** While AVEDEV is less sensitive to outliers than variance, extreme values still influence the result. One value of 1000 in a dataset averaging 100 will significantly increase AVEDEV.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[STDEV.S]] | Sample standard deviation | For statistical inference, hypothesis testing, and confidence intervals |
| [[STDEV.P]] | Population standard deviation | When data represents entire population, not a sample |
| [[VAR.S]] | Sample variance | When you need squared deviation measure for further calculations |
| [[DEVSQ]] | Sum of squared deviations | For ANOVA calculations or custom variance formulas |
| [[AVERAGE]] | Arithmetic mean | For central tendency, not dispersion |

### Commonly Used Together

**[[AVERAGE]]** - Central tendency

*Calculate both mean and spread:*
```
Mean: =AVERAGE(A2:A100)
Spread: =AVEDEV(A2:A100)
Coefficient of variation: =AVEDEV(A2:A100)/AVERAGE(A2:A100)
```
The coefficient of variation (AVEDEV/AVERAGE) provides a relative measure of dispersion that enables comparison across datasets with different scales.

---

**[[MIN]] and [[MAX]]** - Range boundaries

*Understand the full distribution:*
```
Average: =AVERAGE(A2:A100)
Average Deviation: =AVEDEV(A2:A100)
Minimum: =MIN(A2:A100)
Maximum: =MAX(A2:A100)
Range: =MAX(A2:A100)-MIN(A2:A100)
```
Combining AVEDEV with range gives both typical variation and extreme bounds.

---

**[[STDEV.S]]** - Standard deviation comparison

*Compare deviation measures:*
```
Average Deviation: =AVEDEV(A2:A100)
Standard Deviation: =STDEV.S(A2:A100)
Ratio: =STDEV.S(A2:A100)/AVEDEV(A2:A100)
```
For normal distributions, the ratio is approximately 1.25. Significant deviation from this ratio suggests non-normal distribution.

## Official Documentation

- **Microsoft Excel:** [AVEDEV function](https://support.microsoft.com/en-us/office/avedev-function-58fe8d65-2a84-4dc7-8052-f3f87b5c6639)
- **Google Sheets:** [AVEDEV function](https://support.google.com/docs/answer/3093617)

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
