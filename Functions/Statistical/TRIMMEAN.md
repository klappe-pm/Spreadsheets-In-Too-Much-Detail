---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- trimmed-mean
- robust-statistics
- outlier-resistant
- central-tendency
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Trimmed Mean
- Truncated Mean
- Winsorized Average
tags:
- statistical
- average
- mean
- outlier-resistant
- robust-statistics
- data-analysis
---

# TRIMMEAN

## Description

**TRIMMEAN** calculates the mean of a dataset after excluding a specified percentage of data points from both the high and low extremes. This "trimmed mean" is a robust measure of central tendency that is less sensitive to outliers than the ordinary arithmetic mean. By discarding extreme values symmetrically from both ends, TRIMMEAN provides a more stable estimate of the "typical" value when data may contain errors, anomalies, or genuine but extreme observations.

The function removes an equal number of values from the top and bottom of the sorted dataset. The percent parameter specifies the total fraction of data to exclude; this is split evenly between the two ends. For example, with 20 values and percent=0.1 (10%), TRIMMEAN excludes 2 values total (1 from each end), then averages the remaining 18 values. The number of excluded points is always rounded down to ensure an even split and that at least one value remains.

**Key gotcha:** The percent parameter represents the total percentage of data to trim from both ends combined, not the percentage to trim from each end. With percent=0.2, you trim 10% from the bottom and 10% from the top (20% total), not 20% from each end. Additionally, percent must be between 0 and 1, and after accounting for the trim, at least one data point must remain. If the resulting trim would exclude all data points, TRIMMEAN returns #NUM!.

**Platform behavior:** Both Excel and Google Sheets implement TRIMMEAN identically. The function rounds down the number of excluded data points, so TRIMMEAN(data, 0.1) on a 15-element dataset excludes 1 element from each end (not 1.5, which would be impossible). Both platforms ignore text and empty cells in the array, and return #VALUE! if percent is non-numeric or #NUM! if percent is outside the valid range.

## Syntax

> [!f(x)] TRIMMEAN Syntax
>
> ```
> =TRIMMEAN(array, percent)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values to average. Empty cells, text values, and logical values are ignored. Must contain at least one numeric value after trimming. |
| `percent` | Yes | The fraction of data points to exclude from the calculation, between 0 (exclusive) and 1 (exclusive). Half of this percentage is removed from each end. For example, 0.2 means 10% from bottom + 10% from top. |

### Return Value

Returns a numeric value representing the mean of the data after excluding the specified percentage of extreme values. Returns #NUM! if percent is less than 0 or greater than or equal to 1, or if trimming would remove all data points. Returns #VALUE! if percent is non-numeric.

## Examples

> [!f(x)] TRIMMEAN Examples

### Example 1: Basic Trimmed Mean
```
=TRIMMEAN(A1:A10, 0.2)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 100

**Result:** 5.5

**Explanation:** With 10 values and 20% trim, 2 values are excluded (1 from each end). Removing the minimum (1) and maximum (100), the mean of {2, 3, 4, 5, 6, 7, 8, 9} is 44/8 = 5.5. Compare to the regular AVERAGE of 14.5, which is heavily skewed by the outlier 100.

---

### Example 2: Zero Trim Equals Regular Average
```
=TRIMMEAN(A1:A5, 0)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** 30

**Explanation:** With percent=0, no values are trimmed. TRIMMEAN returns the same result as AVERAGE.

---

### Example 3: Small Dataset Behavior
```
=TRIMMEAN(A1:A5, 0.2)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** 30

**Explanation:** With 5 values and 20% trim, the calculation would exclude 1 value total (0.2 * 5 = 1). Since we need an even number for symmetric trimming, this rounds down to 0. No values are trimmed, and the result equals AVERAGE.

---

### Example 4: Rounding Down Behavior
```
=TRIMMEAN(A1:A7, 0.3)
```
Where A1:A7 contains: 5, 10, 15, 20, 25, 30, 35

**Result:** 20

**Explanation:** With 7 values and 30% trim, we would exclude 2.1 values. This rounds down to 2 (1 from each end). Removing 5 and 35, the mean of {10, 15, 20, 25, 30} is 100/5 = 20.

---

### Example 5: High Trim Percentage
```
=TRIMMEAN(A1:A10, 0.4)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result:** 5.5

**Explanation:** With 40% trim, 4 values are excluded (2 from each end). Removing {1, 2} and {9, 10}, the mean of {3, 4, 5, 6, 7, 8} is 33/6 = 5.5.

---

### Example 6: Survey Score Analysis
```
=TRIMMEAN(ResponseScores, 0.1)
```
Where ResponseScores contains: 1, 3, 4, 4, 5, 5, 5, 5, 6, 10

**Result:** 4.625

**Explanation:** Trimming 10% (1 value from each end) removes the extreme 1 and 10. The mean of {3, 4, 4, 5, 5, 5, 5, 6} is 37/8 = 4.625. This better represents typical responses than the regular average of 4.8.

---

### Example 7: Comparing TRIMMEAN with AVERAGE
```
=AVERAGE(A1:A10)
=TRIMMEAN(A1:A10, 0.2)
```
Where A1:A10 contains: 2, 3, 4, 5, 5, 5, 6, 7, 8, 1000

**Result (AVERAGE):** 104.5
**Result (TRIMMEAN):** 5.375

**Explanation:** The outlier (1000) dramatically skews the average to 104.5. TRIMMEAN, by excluding extremes, returns 5.375, which better represents the bulk of the data.

---

### Example 8: Error with Excessive Trim
```
=TRIMMEAN(A1:A3, 0.9)
```
Where A1:A3 contains: 10, 20, 30

**Result:** #NUM!

**Explanation:** With only 3 values and 90% trim, we would need to exclude 2.7 values (rounding down to 2, which means 1 from each end). This leaves only 1 value, which is valid. However, if percent is 1.0 or higher, all data would be trimmed, returning #NUM!.

---

### Example 9: Financial Return Analysis
```
=TRIMMEAN(MonthlyReturns, 0.1)
```
Where MonthlyReturns contains 24 months of returns including some extreme months

**Result:** More stable average return estimate

**Explanation:** Financial returns often have occasional extreme months (crashes or spikes). TRIMMEAN with 10% trim excludes the most extreme months, providing a more representative "typical" monthly return.

---

### Example 10: Comparing Different Trim Levels
```
10% trim: =TRIMMEAN(Data, 0.1)
20% trim: =TRIMMEAN(Data, 0.2)
30% trim: =TRIMMEAN(Data, 0.3)
```

**Result:** Decreasing sensitivity to outliers as trim percentage increases

**Explanation:** Higher trim percentages exclude more extreme values, producing results closer to the median. At 50% trim (25% from each end), TRIMMEAN approaches the median for large datasets.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | percent is less than 0 | Use a positive percentage between 0 and 1. |
| `#NUM!` | percent is greater than or equal to 1 | Use a percentage less than 1 (e.g., 0.5 for 50% total trim). |
| `#NUM!` | Trimming would remove all data points | Reduce the percent parameter or add more data. |
| `#VALUE!` | percent is text or non-numeric | Ensure percent is a number or a cell reference containing a number. |
| `#VALUE!` | Array contains no numeric values | Verify the array contains at least one number. |
| `Unexpected result` | Confusing total trim with per-end trim | Remember: percent=0.2 means 10% from each end (20% total), not 20% from each end. |

## Use Cases

### [[Survey Data Analysis with Response Bias]]

**Scenario:** A market researcher analyzes customer satisfaction surveys where extreme responses (1s and 10s) may represent biased or inattentive respondents rather than genuine opinions.

**Implementation:**
```
Regular Average: =AVERAGE(SurveyResponses)
Trimmed Average: =TRIMMEAN(SurveyResponses, 0.1)
Difference: =AVERAGE(SurveyResponses) - TRIMMEAN(SurveyResponses, 0.1)

Quality Flag: =IF(ABS(Difference) > 0.5, "Review for outliers", "Data looks clean")
```

**Business Application:** Survey responses often include "straight-liners" who give all 1s or all 10s without reading questions. TRIMMEAN provides a more representative satisfaction score by excluding these extreme responses, leading to more actionable insights.

**Technical Details:** A 10% trim is common for survey data (5% from each end). If the difference between AVERAGE and TRIMMEAN is large, investigate the extreme responses for patterns (bot responses, data entry errors, etc.).

---

### [[Olympic Judging Score Calculation]]

**Scenario:** A scoring system for athletic competitions needs to calculate final scores that exclude potentially biased high and low judge scores, following Olympic-style conventions.

**Implementation:**
```
Traditional Olympic Method (drop high/low): =(SUM(Scores) - MAX(Scores) - MIN(Scores)) / (COUNT(Scores) - 2)

TRIMMEAN Equivalent: =TRIMMEAN(Scores, 2/COUNT(Scores))

For 5 judges: =TRIMMEAN(Scores, 0.4)  ' Drops 2 of 5 scores (1 each end)
For 7 judges: =TRIMMEAN(Scores, 2/7)  ' Approximately 0.286
```

**Business Application:** The trimmed mean prevents a single biased judge (either overly generous or harsh) from disproportionately affecting the final score. This is the mathematical basis behind "drop the highest and lowest" scoring in many competitions.

**Technical Details:** The traditional "drop high and low" is mathematically equivalent to TRIMMEAN with percent = 2/n where n is the number of judges. For larger panels, you might trim more (e.g., top and bottom 2 judges with percent = 4/n).

---

### [[Investment Performance Reporting]]

**Scenario:** A portfolio manager reports quarterly performance using trimmed returns to avoid having exceptional quarters (either very good or very bad) distort the "typical" performance picture.

**Implementation:**
```
Regular Average Return: =AVERAGE(QuarterlyReturns)
Trimmed Average Return: =TRIMMEAN(QuarterlyReturns, 0.2)
Median Return: =MEDIAN(QuarterlyReturns)

Risk Assessment: =IF(AVERAGE(QuarterlyReturns) > TRIMMEAN(QuarterlyReturns, 0.2),
                     "Positive outliers present", "Negative outliers present")
```

**Business Application:** Investment marketing often highlights best quarters while downplaying worst quarters. TRIMMEAN provides a more honest "typical quarter" performance metric that clients can expect. If the regular average significantly exceeds the trimmed average, exceptional positive quarters may be skewing perceptions.

**Technical Details:** A 20% trim (10% from each end) is common in finance. Compare TRIMMEAN to median: if they are close, the distribution is relatively symmetric; large differences indicate skewness that might affect risk assessment.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 2000 onward)
- **Rounding:** Rounds down the number of excluded points to ensure symmetric trimming
- **Maximum array size:** Limited by worksheet size
- **Ignores:** Text, empty cells, and logical values in ranges

### Google Sheets
- **Availability:** All versions since launch
- **Equivalence:** Functionally identical to Excel
- **Rounding:** Same round-down behavior
- **Ignores:** Same treatment of text and empty cells

### Key Difference Alert
TRIMMEAN behaves identically in Excel and Google Sheets. There are no platform-specific considerations. The function is fully compatible across both platforms, making it safe for cross-platform workbooks.

## Tips and Best Practices

1. **Understand the percent parameter:** The percent value is the total fraction to trim from both ends combined. With percent=0.1, you trim 5% from the bottom and 5% from the top. This is a common source of confusion.

2. **Use appropriate trim levels:** Common choices are 10% (0.1) for mild outlier resistance, 20% (0.2) for moderate resistance, and 40% (0.4) for high resistance. Higher trims approach the median.

3. **Compare with AVERAGE and MEDIAN:** Calculate all three measures. If AVERAGE differs significantly from TRIMMEAN and MEDIAN, outliers are affecting your mean. If TRIMMEAN equals MEDIAN, your trim may be too aggressive.

4. **Account for small datasets:** With fewer than 10 values, TRIMMEAN may not remove any values due to rounding down. Ensure your dataset is large enough for meaningful trimming.

5. **Document your trim percentage:** When reporting trimmed means, always state the trim percentage used. "Mean (10% trimmed)" is clearer than just "Mean."

6. **Consider WINSORIZING as an alternative:** If you want to retain all data points but reduce outlier influence, consider Winsorizing (replacing extremes with less extreme values) rather than trimming.

7. **Use for preprocessing:** TRIMMEAN can help identify appropriate "typical" values for data cleaning or imputation, especially when you expect some data points to be erroneous.

8. **Combine with other robust measures:** Use TRIMMEAN alongside MEDIAN and interquartile range (IQR) for a complete robust statistical summary that is not distorted by outliers.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AVERAGE]] | Arithmetic mean of all values | When outliers are not a concern or are meaningful |
| [[MEDIAN]] | Middle value (50th percentile) | For maximum outlier resistance; essentially a 50% trimmed mean |
| [[GEOMEAN]] | Geometric mean | For growth rates or multiplicative data |
| [[HARMEAN]] | Harmonic mean | For rates or ratios |
| [[AGGREGATE]] | Flexible aggregate with error/hidden row handling | When you need AVERAGE ignoring errors or hidden rows |

### Commonly Used Together

**[[AVERAGE]]** - Compare trimmed to untrimmed mean

*Detect outlier influence:*
```
Regular: =AVERAGE(Data)
Trimmed: =TRIMMEAN(Data, 0.1)
Outlier Impact: =AVERAGE(Data) - TRIMMEAN(Data, 0.1)
```
Large differences indicate significant outlier influence.

---

**[[MEDIAN]]** - Complete robust statistics

*Compare central tendency measures:*
```
Mean: =AVERAGE(Data)
Trimmed Mean: =TRIMMEAN(Data, 0.2)
Median: =MEDIAN(Data)
```
Together, these three measures reveal distribution shape and outlier presence.

---

**[[STDEV.S]]** - Spread comparison

*Assess variability with and without outliers:*
```
Full StdDev: =STDEV.S(Data)
Trimmed Data StdDev: Need manual approach (no TRIMSTDEV function)
IQR: =QUARTILE(Data, 3) - QUARTILE(Data, 1)
```
Compare IQR to standard deviation to detect outlier-inflated spread.

---

**[[PERCENTILE]]** - Custom trim points

*Create custom trimmed mean:*
```
Lower Bound: =PERCENTILE(Data, 0.05)
Upper Bound: =PERCENTILE(Data, 0.95)
Custom Trimmed Mean: =AVERAGEIFS(Data, Data, ">="&LowerBound, Data, "<="&UpperBound)
```
Use PERCENTILE to define custom trim thresholds when you need more control.

---

**[[COUNT]]** - Determine effective sample size

*Calculate how many values are averaged:*
```
Total Values: =COUNT(Data)
Values Trimmed: =FLOOR(COUNT(Data) * TrimPercent, 2)
Values Used: =COUNT(Data) - Values Trimmed
```
Know how many data points contribute to your trimmed mean.

## Official Documentation

- **Microsoft Excel:** [TRIMMEAN function](https://support.microsoft.com/en-us/office/trimmean-function-d90c9878-a119-4746-88fa-63d988f511d3)
- **Google Sheets:** [TRIMMEAN function](https://support.google.com/docs/answer/3094082)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original function for robust mean calculation |
| Excel 2003 | Continued support | No functional changes |
| Excel 2007 | Continued support | No functional changes |
| Excel 2010+ | All subsequent versions | No functional changes |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
