---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- percentile
- inclusive-interpolation
- distribution-analysis
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Percentile Inclusive
- PERCENTILE Inclusive
- Inclusive Percentile
tags:
- statistical
- percentile
- ranking
- distribution
- data-analysis
- inclusive
---

# PERCENTILE.INC

## Description

**PERCENTILE.INC** returns the k-th percentile of values in a range using inclusive interpolation, where k ranges from 0 to 1 inclusive. The "INC" suffix stands for "inclusive," meaning the function includes both endpoints: k=0 returns the minimum value and k=1 returns the maximum value. This function is functionally identical to the legacy PERCENTILE function but provides explicit clarity about the interpolation method being used.

The inclusive interpolation method calculates the percentile position using the formula: rank = k * (n-1) + 1, where n is the count of values. When this position falls between two data points, PERCENTILE.INC performs linear interpolation between the adjacent values. For example, with 10 data points, the 25th percentile (k=0.25) falls at position 3.25, so the function returns 75% of the 3rd value plus 25% of the 4th value.

**Key gotcha:** The distinction between PERCENTILE.INC and PERCENTILE.EXC matters most with small datasets and extreme percentiles. With the inclusive method, you can always calculate k=0 (minimum) and k=1 (maximum), but for a 10-element dataset, the 95th percentile (k=0.95) uses actual data points. With the exclusive method, certain extreme percentiles become undefined (returning #NUM!) because they would require extrapolation beyond the observed data. Choose INC when you need the full 0-to-1 range; choose EXC when statistical rigor requires exclusive endpoints.

**Platform behavior:** Excel introduced PERCENTILE.INC in Excel 2010 to replace the ambiguous PERCENTILE function. Google Sheets fully supports PERCENTILE.INC with identical behavior. Both platforms accept k values from 0 to 1 inclusive, ignore text and empty cells in the array, and return #NUM! for invalid k values. The function is recommended by Microsoft for all new workbooks over the legacy PERCENTILE function.

## Syntax

> [!f(x)] PERCENTILE.INC Syntax
>
> ```
> =PERCENTILE.INC(array, k)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset. Empty cells, text values, and logical values are ignored. Must contain at least one numeric value. |
| `k` | Yes | The percentile value between 0 and 1 inclusive. k=0 returns the minimum, k=1 returns the maximum, k=0.5 returns the median. Accepts any decimal value in this range. |

### Return Value

Returns a numeric value representing the k-th percentile using inclusive interpolation. Returns #NUM! if k is less than 0 or greater than 1, or if the array contains no numeric values. Returns #VALUE! if k is non-numeric.

## Examples

> [!f(x)] PERCENTILE.INC Examples

### Example 1: Median Calculation (50th Percentile)
```
=PERCENTILE.INC(A1:A9, 0.5)
```
Where A1:A9 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9

**Result:** 5

**Explanation:** With 9 values, the median position is (0.5 * 8) + 1 = 5, which is exactly the 5th value. No interpolation is needed when the position is a whole number.

---

### Example 2: First Quartile (25th Percentile)
```
=PERCENTILE.INC(A1:A8, 0.25)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 27.5

**Explanation:** Position = (0.25 * 7) + 1 = 2.75. This falls between the 2nd value (20) and 3rd value (30). Interpolating: 20 + 0.75 * (30 - 20) = 27.5.

---

### Example 3: Third Quartile (75th Percentile)
```
=PERCENTILE.INC(A1:A8, 0.75)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 62.5

**Explanation:** Position = (0.75 * 7) + 1 = 6.25. This falls between the 6th value (60) and 7th value (70). Interpolating: 60 + 0.25 * (70 - 60) = 62.5.

---

### Example 4: Minimum Value (k=0)
```
=PERCENTILE.INC(A1:A5, 0)
```
Where A1:A5 contains: 15, 25, 35, 45, 55

**Result:** 15

**Explanation:** With inclusive interpolation, k=0 always returns the minimum value. Position = (0 * 4) + 1 = 1, the first element.

---

### Example 5: Maximum Value (k=1)
```
=PERCENTILE.INC(A1:A5, 1)
```
Where A1:A5 contains: 15, 25, 35, 45, 55

**Result:** 55

**Explanation:** With inclusive interpolation, k=1 always returns the maximum value. Position = (1 * 4) + 1 = 5, the last element.

---

### Example 6: 90th Percentile for Performance Analysis
```
=PERCENTILE.INC(SalesData, 0.9)
```
Where SalesData contains: 1200, 1450, 1800, 2100, 2400, 2750, 3100, 3500, 4000, 4500

**Result:** 4250

**Explanation:** Position = (0.9 * 9) + 1 = 9.1. This falls between the 9th value (4000) and 10th value (4500). Interpolating: 4000 + 0.1 * (4500 - 4000) = 4250. Sales reps exceeding this threshold are in the top 10%.

---

### Example 7: Comparing INC vs EXC Results
```
=PERCENTILE.INC(A1:A5, 0.25)
=PERCENTILE.EXC(A1:A5, 0.25)
```
Where A1:A5 contains: 1, 2, 3, 4, 5

**Result (INC):** 2
**Result (EXC):** 1.5

**Explanation:** For the same data and percentile, INC and EXC can produce different results. INC uses position (0.25 * 4) + 1 = 2, returning the 2nd value exactly. EXC uses position 0.25 * 6 = 1.5, interpolating between the 1st and 2nd values.

---

### Example 8: Handling Empty Cells
```
=PERCENTILE.INC(A1:A10, 0.5)
```
Where A1:A10 contains: 10, [empty], 20, [empty], 30, [empty], 40, [empty], 50, [empty]

**Result:** 30

**Explanation:** Empty cells are ignored. The effective dataset is {10, 20, 30, 40, 50}. The median of these 5 values is 30.

---

### Example 9: Array Formula with Multiple Percentiles
```
=PERCENTILE.INC(A1:A100, {0.25, 0.5, 0.75})
```

**Result:** Array of three values representing Q1, median, and Q3

**Explanation:** In Excel 365 or Google Sheets, you can pass an array of k values to get multiple percentiles at once. This spills three results showing the quartile distribution.

---

### Example 10: Nested in Conditional Logic
```
=IF(B2 > PERCENTILE.INC($A$1:$A$100, 0.95), "Exceptional",
   IF(B2 > PERCENTILE.INC($A$1:$A$100, 0.75), "Above Average", "Standard"))
```

**Result:** Classification label based on percentile standing

**Explanation:** Use PERCENTILE.INC within IF statements to create dynamic classification tiers. The absolute references ensure the same dataset is used when copying the formula.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | k is less than 0 or greater than 1 | Ensure k is in the range [0, 1]. For INC, both endpoints are valid. |
| `#NUM!` | Array contains no numeric values | Verify at least one numeric value exists. Convert text-formatted numbers if needed. |
| `#VALUE!` | k is text or a non-numeric value | Ensure k is numeric. Cell references to text will cause this error. |
| `#VALUE!` | Invalid range reference | Check that the array reference points to valid cells. |
| `Function not found` | Using PERCENTILE.INC in Excel 2007 or earlier | Use legacy PERCENTILE function, which is functionally identical. |
| `Unexpected result` | Confusing INC with EXC methodology | Verify you are using the correct function for your statistical requirements. |

## Use Cases

### [[Salary Benchmarking and Compensation Analysis]]

**Scenario:** A compensation analyst needs to position company salaries against industry benchmarks, establishing competitive ranges based on market percentiles.

**Implementation:**
```
Below Market: =PERCENTILE.INC(IndustryData, 0.25)
At Market: =PERCENTILE.INC(IndustryData, 0.50)
Above Market: =PERCENTILE.INC(IndustryData, 0.75)
Premium: =PERCENTILE.INC(IndustryData, 0.90)

Position Rating: =IF(Salary >= Premium, "Premium",
                    IF(Salary >= AtMarket, "Competitive", "Below Market"))
```

**Business Application:** The inclusive method is preferred for compensation because it provides clear minimum (0th percentile) and maximum (100th percentile) anchors. HR can confidently state that "our target is the 50th percentile of market" knowing this represents a meaningful middle position.

**Technical Details:** Use PERCENTILE.INC for compensation analysis because the inclusive range aligns with how salary surveys present data. Always document the data source, effective date, and any adjustments (geography, company size) applied to the benchmark data.

---

### [[Website Response Time SLA Monitoring]]

**Scenario:** A DevOps team monitors web application performance, using the 95th percentile response time as the SLA metric rather than average or maximum.

**Implementation:**
```
P95 Response Time: =PERCENTILE.INC(ResponseTimes, 0.95)
P99 Response Time: =PERCENTILE.INC(ResponseTimes, 0.99)
SLA Status: =IF(PERCENTILE.INC(ResponseTimes, 0.95) <= 200, "Meeting SLA", "SLA Breach")
```

**Business Application:** P95 and P99 percentiles are industry-standard metrics for performance SLAs because they capture typical user experience while being robust to occasional outliers. A P95 of 200ms means 95% of requests complete in under 200 milliseconds.

**Technical Details:** The inclusive method works well for SLA monitoring because it provides defined behavior at the extremes. For large datasets (millions of requests), consider sampling or pre-aggregating data before applying PERCENTILE.INC to maintain calculation performance.

---

### [[Test Score Norming and Grade Curves]]

**Scenario:** An educational institution needs to establish grade cutoffs based on the actual distribution of scores, creating a fair curve regardless of test difficulty.

**Implementation:**
```
A Threshold (Top 10%): =PERCENTILE.INC(AllScores, 0.90)
B Threshold (Top 25%): =PERCENTILE.INC(AllScores, 0.75)
C Threshold (Top 50%): =PERCENTILE.INC(AllScores, 0.50)
D Threshold (Top 75%): =PERCENTILE.INC(AllScores, 0.25)
F: Below D Threshold

Grade: =IFS(Score >= AThreshold, "A",
            Score >= BThreshold, "B",
            Score >= CThreshold, "C",
            Score >= DThreshold, "D",
            TRUE, "F")
```

**Business Application:** Percentile-based grading ensures consistent grade distribution across different test administrations. If one exam was harder than another, the percentile approach automatically adjusts thresholds based on actual performance.

**Technical Details:** Store threshold values in reference cells rather than recalculating in each grade formula. This improves performance and ensures consistency. Consider whether ties at threshold boundaries should round up or down for grade assignment.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Recommended over:** PERCENTILE (legacy function)
- **Array support:** Native in Excel 365; requires Ctrl+Shift+Enter in earlier versions
- **Maximum array size:** 1,048,576 rows per column

### Google Sheets
- **Availability:** All versions
- **Equivalence:** Functionally identical to Excel implementation
- **Array support:** Native array formula support
- **Maximum array size:** Limited by total cell count (10,000,000 cells per spreadsheet)

### Key Difference Alert
PERCENTILE.INC behaves identically in Excel and Google Sheets. The primary consideration is Excel version compatibility: if your workbook may be opened in Excel 2007 or earlier, use PERCENTILE instead (which is functionally identical). For Excel 2010+ and Google Sheets, PERCENTILE.INC is preferred because it explicitly documents the inclusive interpolation method being used.

## Tips and Best Practices

1. **Use PERCENTILE.INC over PERCENTILE:** Even though they are functionally identical, PERCENTILE.INC explicitly communicates your methodology. This is especially important in regulated industries or academic contexts where statistical methods must be documented.

2. **Understand when to use EXC instead:** Use PERCENTILE.EXC when your statistical framework requires exclusive endpoints, or when you are replicating analysis from software that uses the exclusive method by default (like some statistical packages). The exclusive method is mathematically preferred for probability distribution fitting.

3. **Create a percentile lookup table:** For repeated analysis, calculate all needed percentiles once in a reference area (e.g., 10th, 25th, 50th, 75th, 90th, 95th, 99th) rather than embedding PERCENTILE.INC calls in every formula.

4. **Combine with PERCENTRANK.INC:** Use PERCENTILE.INC to find thresholds and PERCENTRANK.INC to classify values. These are complementary functions: PERCENTILE gives you the value at a percentile, PERCENTRANK gives you the percentile of a value.

5. **Handle small datasets carefully:** With very small datasets (fewer than 10 values), percentile results can be unstable and sensitive to the interpolation method. Consider whether percentile analysis is appropriate or if simple ranking might be clearer.

6. **Document your percentile choices:** When sharing analysis, note which percentile function you used (INC or EXC) and why. Different stakeholders may have different expectations based on their statistical backgrounds.

7. **Use absolute references for thresholds:** When comparing values to a percentile threshold, use absolute references ($A$1:$A$100) for the data range so the formula can be copied without shifting the reference.

8. **Consider AGGREGATE for filtered data:** PERCENTILE.INC includes hidden and filtered rows. Use AGGREGATE(16, 5, array, k) if you need a percentile that respects filters (16 = PERCENTILE.INC equivalent, 5 = ignore hidden rows).

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERCENTILE]] | Legacy function identical to PERCENTILE.INC | Only for Excel 2007 compatibility; otherwise use PERCENTILE.INC |
| [[PERCENTILE.EXC]] | Percentile with exclusive interpolation (k must be >0 and <1) | When statistical standards require exclusive endpoints |
| [[QUARTILE.INC]] | Returns specific quartile values (0, 1, 2, 3, 4) using inclusive method | When you only need standard quartile positions |
| [[MEDIAN]] | Returns the 50th percentile | More readable when you specifically need the median |
| [[SMALL]] | Returns the k-th smallest value by position | When you need a specific ranked position, not a percentile |
| [[LARGE]] | Returns the k-th largest value by position | When you need a specific ranked position from the top |

### Commonly Used Together

**[[PERCENTRANK.INC]]** - Inverse of PERCENTILE.INC

*Find where a specific value falls in the distribution:*
```
Threshold: =PERCENTILE.INC(Scores, 0.9)
Student Standing: =PERCENTRANK.INC(Scores, StudentScore)
```
Together, these functions provide complete percentile-based analysis.

---

**[[QUARTILE.INC]]** - Simplified quartile access

*Get standard quartile breakpoints:*
```
Q1: =QUARTILE.INC(Data, 1)  ' Same as PERCENTILE.INC(Data, 0.25)
Q2: =QUARTILE.INC(Data, 2)  ' Same as PERCENTILE.INC(Data, 0.5)
Q3: =QUARTILE.INC(Data, 3)  ' Same as PERCENTILE.INC(Data, 0.75)
```
QUARTILE.INC is more readable when you need exactly the quartile positions.

---

**[[STDEV.S]] / [[STDEV.P]]** - Spread measurement

*Combine percentiles with standard deviation for complete distribution summary:*
```
="Median: " & PERCENTILE.INC(Data, 0.5) & ", IQR: " &
  (PERCENTILE.INC(Data, 0.75) - PERCENTILE.INC(Data, 0.25)) &
  ", StdDev: " & STDEV.S(Data)
```
Percentiles describe distribution shape; standard deviation describes spread.

---

**[[AGGREGATE]]** - Filtered percentiles

*Calculate percentiles ignoring hidden rows:*
```
=AGGREGATE(16, 5, A1:A100, 0.9)
```
AGGREGATE function 16 is PERCENTILE.INC; option 5 ignores hidden rows. Use when working with filtered data.

## Official Documentation

- **Microsoft Excel:** [PERCENTILE.INC function](https://support.microsoft.com/en-us/office/percentile-inc-function-680f9539-45eb-410b-9a5e-c1355e5fe2ed)
- **Google Sheets:** [PERCENTILE function](https://support.google.com/docs/answer/3094083) (Google documents PERCENTILE and PERCENTILE.INC together)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | Initial release | Introduced alongside PERCENTILE.EXC to replace ambiguous PERCENTILE |
| Excel 2013 | Continued support | No functional changes |
| Excel 2016 | Continued support | No functional changes |
| Excel 2019 | Continued support | No functional changes |
| Excel 365 | Continuous updates | Native dynamic array support for multiple percentile calculation |
| Google Sheets | Full support | Available since feature parity updates; identical to Excel |

---

*Last updated: 2026-01-10*
