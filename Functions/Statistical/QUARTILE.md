---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- quartile
- distribution-analysis
- descriptive-statistics
- data-segmentation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Quartile Value
- Quarter Percentile
- Q1 Q2 Q3
tags:
- statistical
- quartile
- percentile
- distribution
- data-analysis
- legacy-function
---

# QUARTILE

## Description

**QUARTILE** returns the quartile of a dataset, dividing the distribution into four equal parts at the 0th, 25th, 50th, 75th, and 100th percentiles. Quartiles are fundamental to descriptive statistics, providing key breakpoints that summarize data distribution. The function accepts a quart parameter (0-4) specifying which quartile to return: 0 for minimum, 1 for first quartile (Q1/25th percentile), 2 for median (Q2/50th percentile), 3 for third quartile (Q3/75th percentile), and 4 for maximum.

The function uses inclusive interpolation (identical to PERCENTILE.INC) to calculate quartile values. When the quartile position falls between two data points, QUARTILE performs linear interpolation. For example, with 10 data points, Q1 falls at position (0.25 * 9) + 1 = 3.25, so the function interpolates between the 3rd and 4th values. QUARTILE is essentially a convenience wrapper around PERCENTILE, where QUARTILE(data, 1) equals PERCENTILE(data, 0.25).

**Key gotcha:** QUARTILE is a legacy function that Microsoft retained for backward compatibility. In Excel 2010 and later, Microsoft introduced QUARTILE.INC (functionally identical to QUARTILE) and QUARTILE.EXC (which uses exclusive interpolation). The quart parameter must be an integer from 0 to 4; non-integer values are truncated, and values outside this range return #NUM!. Additionally, the interquartile range (IQR = Q3 - Q1) is a common measure of spread that you must calculate manually as QUARTILE(data, 3) - QUARTILE(data, 1).

**Platform behavior:** Both Excel and Google Sheets implement QUARTILE identically using the inclusive method. Both platforms accept quart values 0-4, return #NUM! for invalid quart values, and ignore text and empty cells in the array. Google Sheets has not deprecated QUARTILE, while Excel recommends QUARTILE.INC for new workbooks.

## Syntax

> [!f(x)] QUARTILE Syntax
>
> ```
> =QUARTILE(array, quart)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset. Empty cells, text values, and logical values are ignored. Must contain at least one numeric value. |
| `quart` | Yes | An integer from 0 to 4 specifying which quartile to return: 0=minimum, 1=Q1 (25th percentile), 2=median (50th percentile), 3=Q3 (75th percentile), 4=maximum. |

### Return Value

Returns a numeric value representing the specified quartile of the dataset using inclusive interpolation. Returns #NUM! if quart is less than 0 or greater than 4. Returns #VALUE! if quart is non-numeric. Returns #NUM! if the array contains no numeric values.

## Examples

> [!f(x)] QUARTILE Examples

### Example 1: Minimum Value (Quartile 0)
```
=QUARTILE(A1:A10, 0)
```
Where A1:A10 contains: 5, 10, 15, 20, 25, 30, 35, 40, 45, 50

**Result:** 5

**Explanation:** Quartile 0 returns the minimum value in the dataset. This is equivalent to =MIN(A1:A10).

---

### Example 2: First Quartile (Q1)
```
=QUARTILE(A1:A10, 1)
```
Where A1:A10 contains: 5, 10, 15, 20, 25, 30, 35, 40, 45, 50

**Result:** 16.25

**Explanation:** Q1 is at position (0.25 * 9) + 1 = 3.25, between 15 and 20. Interpolating: 15 + 0.25 * (20 - 15) = 16.25. 25% of values are below this threshold.

---

### Example 3: Median (Q2)
```
=QUARTILE(A1:A10, 2)
```
Where A1:A10 contains: 5, 10, 15, 20, 25, 30, 35, 40, 45, 50

**Result:** 27.5

**Explanation:** Q2 (median) is at position (0.5 * 9) + 1 = 5.5, between 25 and 30. Interpolating: 25 + 0.5 * (30 - 25) = 27.5. This is equivalent to =MEDIAN(A1:A10).

---

### Example 4: Third Quartile (Q3)
```
=QUARTILE(A1:A10, 3)
```
Where A1:A10 contains: 5, 10, 15, 20, 25, 30, 35, 40, 45, 50

**Result:** 38.75

**Explanation:** Q3 is at position (0.75 * 9) + 1 = 7.75, between 35 and 40. Interpolating: 35 + 0.75 * (40 - 35) = 38.75. 75% of values are below this threshold.

---

### Example 5: Maximum Value (Quartile 4)
```
=QUARTILE(A1:A10, 4)
```
Where A1:A10 contains: 5, 10, 15, 20, 25, 30, 35, 40, 45, 50

**Result:** 50

**Explanation:** Quartile 4 returns the maximum value in the dataset. This is equivalent to =MAX(A1:A10).

---

### Example 6: Calculating Interquartile Range (IQR)
```
=QUARTILE(A1:A10, 3) - QUARTILE(A1:A10, 1)
```
Where A1:A10 contains: 5, 10, 15, 20, 25, 30, 35, 40, 45, 50

**Result:** 22.5

**Explanation:** The IQR (38.75 - 16.25 = 22.5) measures the spread of the middle 50% of data. IQR is used for outlier detection: values below Q1 - 1.5*IQR or above Q3 + 1.5*IQR are often considered outliers.

---

### Example 7: Unsorted Data
```
=QUARTILE(A1:A5, 2)
```
Where A1:A5 contains: 40, 10, 30, 50, 20

**Result:** 30

**Explanation:** QUARTILE internally sorts the data. The sorted order is {10, 20, 30, 40, 50}, and the median is 30 regardless of the original order.

---

### Example 8: Invalid Quartile Value
```
=QUARTILE(A1:A5, 5)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** #NUM!

**Explanation:** The quart parameter must be 0, 1, 2, 3, or 4. Values outside this range return #NUM! error.

---

### Example 9: Relationship to PERCENTILE
```
=QUARTILE(A1:A10, 1) = PERCENTILE(A1:A10, 0.25)
```

**Result:** TRUE

**Explanation:** QUARTILE is a convenience function for specific percentiles. Q1 = 25th percentile, Q2 = 50th, Q3 = 75th. QUARTILE(data, n) equals PERCENTILE(data, n*0.25) for n=1,2,3.

---

### Example 10: Five-Number Summary
```
Min: =QUARTILE(Data, 0)
Q1:  =QUARTILE(Data, 1)
Q2:  =QUARTILE(Data, 2)
Q3:  =QUARTILE(Data, 3)
Max: =QUARTILE(Data, 4)
```

**Result:** Complete five-number summary

**Explanation:** The five-number summary (min, Q1, median, Q3, max) provides a comprehensive overview of data distribution. This is the foundation for box plots and outlier analysis.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | quart is less than 0 or greater than 4 | Use only integer values 0, 1, 2, 3, or 4 for the quart parameter. |
| `#NUM!` | Array contains no numeric values | Verify the array contains at least one number. Check for text-formatted numbers. |
| `#VALUE!` | quart is text or non-numeric | Ensure quart is a number or a cell reference containing a number. |
| `Unexpected result` | Non-integer quart value used | QUARTILE truncates non-integers. QUARTILE(data, 1.9) returns Q1, not Q2. Use integer values explicitly. |
| `Unexpected result` | Confusing with PERCENTILE scale | Remember: QUARTILE uses 0-4, not 0-1 or 0-100. Q1 is quart=1, not quart=25. |

## Use Cases

### [[Box Plot Visualization Data]]

**Scenario:** A data analyst needs to calculate the five-number summary for creating box plot visualizations of sales performance across regions.

**Implementation:**
```
Min:    =QUARTILE(RegionSales, 0)
Q1:     =QUARTILE(RegionSales, 1)
Median: =QUARTILE(RegionSales, 2)
Q3:     =QUARTILE(RegionSales, 3)
Max:    =QUARTILE(RegionSales, 4)
IQR:    =QUARTILE(RegionSales, 3) - QUARTILE(RegionSales, 1)

Lower Fence: =QUARTILE(RegionSales, 1) - 1.5 * IQR
Upper Fence: =QUARTILE(RegionSales, 3) + 1.5 * IQR
```

**Business Application:** Box plots provide instant visual comparison of distributions across groups. The quartiles show spread, skewness, and outliers at a glance. Marketing can quickly identify which regions have consistent performance versus high variability.

**Technical Details:** The "1.5 * IQR" rule for outliers is a common convention but not absolute. Some analyses use 2 * IQR or 3 * IQR for more extreme outliers. Consider whether outliers represent errors or genuine extreme values.

---

### [[Customer Segmentation by Value]]

**Scenario:** A retail company segments customers into quartiles based on annual spending to create targeted marketing campaigns.

**Implementation:**
```
Q1 Threshold: =QUARTILE(AllCustomerSpending, 1)
Q2 Threshold: =QUARTILE(AllCustomerSpending, 2)
Q3 Threshold: =QUARTILE(AllCustomerSpending, 3)

Segment: =IF(Spending >= Q3, "Platinum",
            IF(Spending >= Q2, "Gold",
               IF(Spending >= Q1, "Silver", "Bronze")))
```

**Business Application:** Quartile-based segmentation ensures exactly 25% of customers fall into each tier (approximately, due to ties). This provides balanced segment sizes for campaign planning and resource allocation.

**Technical Details:** Store quartile thresholds in reference cells to avoid recalculation. Update quarterly or annually as the customer base changes. Consider whether new customers should be evaluated against historical or current quartiles.

---

### [[Grade Distribution Analysis]]

**Scenario:** An educator analyzes test score distributions to understand class performance and identify students needing additional support.

**Implementation:**
```
Class Statistics:
Lowest: =QUARTILE(TestScores, 0)
Q1:     =QUARTILE(TestScores, 1)
Median: =QUARTILE(TestScores, 2)
Q3:     =QUARTILE(TestScores, 3)
Highest: =QUARTILE(TestScores, 4)

Student Classification:
=IF(Score >= QUARTILE(TestScores, 3), "Exceeds Expectations",
   IF(Score >= QUARTILE(TestScores, 2), "Meets Expectations",
      IF(Score >= QUARTILE(TestScores, 1), "Approaching", "Needs Support")))
```

**Business Application:** Quartile-based analysis provides relative standing that adjusts for test difficulty. A student at Q3 performed better than 75% of classmates, regardless of whether the class average was 70% or 85%.

**Technical Details:** For small classes (fewer than 20 students), quartile boundaries may not be meaningful. Consider combining classes for more stable reference groups. Report quartile standing alongside absolute scores for complete context.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 2000 onward)
- **Deprecation:** Marked as legacy function since Excel 2010; QUARTILE.INC recommended
- **Method:** Inclusive interpolation (same as PERCENTILE)
- **Valid quart values:** 0, 1, 2, 3, 4

### Google Sheets
- **Availability:** All versions since launch
- **Deprecation:** No deprecation notice; fully supported
- **Method:** Identical to Excel - inclusive interpolation
- **Valid quart values:** 0, 1, 2, 3, 4

### Key Difference Alert
QUARTILE behaves identically in Excel and Google Sheets. The only consideration is that Excel officially recommends QUARTILE.INC for new workbooks for explicitness, while Google Sheets treats QUARTILE as a current function. For cross-platform work, either function produces identical results.

## Tips and Best Practices

1. **Use QUARTILE.INC for new work:** Although functionally identical, QUARTILE.INC explicitly communicates the inclusive methodology. This is important for documentation and when working with statisticians who may ask which method was used.

2. **Understand the INC vs EXC difference:** QUARTILE (and QUARTILE.INC) use inclusive interpolation. QUARTILE.EXC uses exclusive interpolation and has different requirements. For most business applications, INC is preferred.

3. **Calculate IQR for spread measurement:** The interquartile range (Q3 - Q1) is a robust measure of spread that is not affected by outliers like standard deviation. Use IQR for skewed distributions.

4. **Use for outlier detection:** The standard rule is that values below Q1 - 1.5*IQR or above Q3 + 1.5*IQR are potential outliers. This is more robust than standard deviation-based detection for non-normal data.

5. **Remember the scale:** QUARTILE uses 0-4, not percentages. Q1 is quart=1, not quart=25. This is different from PERCENTILE which uses 0-1.

6. **Cache quartile values for efficiency:** When classifying many values against quartile thresholds, calculate the thresholds once in reference cells rather than recalculating in every formula.

7. **Consider PERCENTILE for flexibility:** If you need arbitrary percentiles (10th, 90th, 95th), use PERCENTILE directly. QUARTILE only provides the standard quartile positions.

8. **Use for box plot preparation:** The five-number summary from QUARTILE (min, Q1, median, Q3, max) provides all data needed for box plot visualization.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[QUARTILE.INC]] | Identical to QUARTILE with inclusive interpolation | For new workbooks; explicit methodology |
| [[QUARTILE.EXC]] | Quartile with exclusive interpolation | When statistical standards require exclusive method |
| [[PERCENTILE]] | Returns arbitrary percentile values | When you need percentiles other than 0, 25, 50, 75, 100 |
| [[MEDIAN]] | Returns the 50th percentile | More readable for specifically the median (equivalent to QUARTILE(data, 2)) |
| [[MIN]] / [[MAX]] | Returns minimum/maximum | Equivalent to QUARTILE(data, 0) and QUARTILE(data, 4) respectively |

### Commonly Used Together

**[[PERCENTILE]]** - Arbitrary percentiles

*Use for percentiles beyond standard quartiles:*
```
10th percentile: =PERCENTILE(Data, 0.1)
Q1 (25th):       =QUARTILE(Data, 1)   ' or PERCENTILE(Data, 0.25)
Median (50th):   =QUARTILE(Data, 2)   ' or PERCENTILE(Data, 0.5)
90th percentile: =PERCENTILE(Data, 0.9)
```
QUARTILE is a convenience for common percentiles; PERCENTILE provides flexibility.

---

**[[STDEV.S]] / [[STDEV.P]]** - Spread comparison

*Compare quartile-based and standard deviation-based spread:*
```
IQR: =QUARTILE(Data, 3) - QUARTILE(Data, 1)
StdDev: =STDEV.S(Data)
```
IQR is robust to outliers; standard deviation is affected by extremes.

---

**[[IF]] / [[IFS]]** - Quartile-based classification

*Segment data by quartile:*
```
=IFS(Value >= QUARTILE(Data, 3), "Top 25%",
     Value >= QUARTILE(Data, 2), "Upper Middle",
     Value >= QUARTILE(Data, 1), "Lower Middle",
     TRUE, "Bottom 25%")
```
Create four equal-sized segments based on distribution.

---

**[[AVERAGE]]** - Compare mean to median

*Assess distribution skewness:*
```
Mean: =AVERAGE(Data)
Median: =QUARTILE(Data, 2)
Skew Indicator: =IF(Mean > Median, "Right-skewed", IF(Mean < Median, "Left-skewed", "Symmetric"))
```
Comparing mean and median reveals distribution shape.

## Official Documentation

- **Microsoft Excel:** [QUARTILE function](https://support.microsoft.com/en-us/office/quartile-function-93cf8f62-60cd-4fdb-8a92-8451041e1a2a)
- **Google Sheets:** [QUARTILE function](https://support.google.com/docs/answer/3094082)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original function with inclusive interpolation |
| Excel 2010 | QUARTILE.INC, QUARTILE.EXC added | QUARTILE marked as legacy; INC/EXC provide explicit methodology |
| Excel 2013+ | All subsequent versions | QUARTILE retained for backward compatibility |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation; no deprecation |

---

*Last updated: 2026-01-10*
