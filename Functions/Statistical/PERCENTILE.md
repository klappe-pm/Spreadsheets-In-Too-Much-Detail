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
- distribution-analysis
- data-ranking
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Percentile Value
- Kth Percentile
- Percentile Function
tags:
- statistical
- percentile
- ranking
- distribution
- data-analysis
- legacy-function
---

# PERCENTILE

## Description

**PERCENTILE** returns the k-th percentile of values in a range, where k is a value between 0 and 1 inclusive. A percentile indicates the value below which a given percentage of observations fall. For example, the 90th percentile (k=0.9) is the value below which 90% of the data points lie. This function is essential for understanding data distribution, identifying thresholds, and performing comparative analysis across datasets.

The function works by first sorting the data in ascending order, then calculating the position corresponding to the requested percentile using linear interpolation when the position falls between two data points. PERCENTILE uses inclusive interpolation, meaning k=0 returns the minimum value and k=1 returns the maximum value. The calculation formula positions the percentile at rank (k * (n-1)) + 1, where n is the count of values. If this position is not a whole number, PERCENTILE interpolates between the two adjacent values to produce the result.

**Key gotcha:** PERCENTILE is a legacy function that Microsoft retained for backward compatibility. In Excel 2010 and later, Microsoft introduced PERCENTILE.INC (which is functionally identical to PERCENTILE) and PERCENTILE.EXC (which uses exclusive interpolation). While PERCENTILE continues to work, Microsoft recommends using PERCENTILE.INC for new workbooks to ensure clarity about which interpolation method is being used. The distinction matters when working with small datasets or extreme percentiles, where the two methods can produce noticeably different results.

**Platform behavior:** Both Excel and Google Sheets implement PERCENTILE identically, using the inclusive method (k from 0 to 1). Google Sheets does not have a formal deprecation notice for PERCENTILE, and it remains fully supported. Both platforms return #NUM! if k is outside the 0-1 range, and #VALUE! if non-numeric arguments are provided. Empty cells and text within the array are ignored.

## Syntax

> [!f(x)] PERCENTILE Syntax
>
> ```
> =PERCENTILE(array, k)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset. Empty cells and text values are ignored. Must contain at least one numeric value. |
| `k` | Yes | The percentile value between 0 and 1 inclusive. 0 returns the minimum, 1 returns the maximum, 0.5 returns the median. Can be a decimal like 0.25, 0.75, 0.9, etc. |

### Return Value

Returns a numeric value representing the k-th percentile of the data. Returns #NUM! if k is less than 0 or greater than 1, or if the array is empty. Returns #VALUE! if k is non-numeric.

## Examples

> [!f(x)] PERCENTILE Examples

### Example 1: Basic Median Calculation
```
=PERCENTILE(A1:A5, 0.5)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** 30

**Explanation:** The 50th percentile (median) of an odd-numbered sorted dataset is the middle value. With five values, the third value (30) is the median.

---

### Example 2: 25th Percentile (First Quartile)
```
=PERCENTILE(A1:A10, 0.25)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result:** 3.25

**Explanation:** The 25th percentile falls at position (0.25 * 9) + 1 = 3.25. Since this is between the 3rd value (3) and 4th value (4), PERCENTILE interpolates: 3 + 0.25 * (4 - 3) = 3.25.

---

### Example 3: 90th Percentile
```
=PERCENTILE(A1:A10, 0.9)
```
Where A1:A10 contains: 45, 52, 61, 68, 72, 75, 81, 84, 90, 95

**Result:** 91.5

**Explanation:** The 90th percentile position is (0.9 * 9) + 1 = 9.1. This falls between the 9th value (90) and 10th value (95). Interpolating: 90 + 0.1 * (95 - 90) = 91.5.

---

### Example 4: Minimum Value (k=0)
```
=PERCENTILE(A1:A5, 0)
```
Where A1:A5 contains: 100, 200, 300, 400, 500

**Result:** 100

**Explanation:** When k=0, PERCENTILE returns the minimum value in the dataset. This is equivalent to =MIN(A1:A5).

---

### Example 5: Maximum Value (k=1)
```
=PERCENTILE(A1:A5, 1)
```
Where A1:A5 contains: 100, 200, 300, 400, 500

**Result:** 500

**Explanation:** When k=1, PERCENTILE returns the maximum value in the dataset. This is equivalent to =MAX(A1:A5).

---

### Example 6: Handling Unsorted Data
```
=PERCENTILE(A1:A5, 0.5)
```
Where A1:A5 contains: 50, 10, 40, 30, 20

**Result:** 30

**Explanation:** PERCENTILE internally sorts the data before calculating. The sorted order is 10, 20, 30, 40, 50. The median is 30 regardless of the original order in the cells.

---

### Example 7: Ignoring Text Values
```
=PERCENTILE(A1:A6, 0.5)
```
Where A1:A6 contains: 10, "N/A", 20, 30, "Missing", 40

**Result:** 25

**Explanation:** Only numeric values (10, 20, 30, 40) are considered. The median of these four values falls between 20 and 30, calculated as 20 + 0.5 * (30 - 20) = 25.

---

### Example 8: Variable Percentile from Cell Reference
```
=PERCENTILE(A1:A10, B1)
```
Where A1:A10 contains test scores and B1 contains 0.75

**Result:** The 75th percentile of the test scores

**Explanation:** The percentile value can come from a cell reference, allowing dynamic analysis. Changing B1 to different values (0.1, 0.5, 0.9) instantly recalculates different percentiles.

---

### Example 9: Using with Named Range
```
=PERCENTILE(SalesData, 0.95)
```
Where SalesData is a named range containing monthly sales figures

**Result:** The 95th percentile of sales

**Explanation:** Named ranges work seamlessly with PERCENTILE. This is useful for finding exceptional performance thresholds (e.g., "top 5% of sales days").

---

### Example 10: Comparing Value to Percentile
```
=IF(B2 >= PERCENTILE(A:A, 0.9), "Top 10%", "Below Top 10%")
```

**Result:** "Top 10%" or "Below Top 10%"

**Explanation:** Combine PERCENTILE with IF to classify values. This formula checks if a specific value meets or exceeds the 90th percentile threshold of a dataset.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | k is less than 0 or greater than 1 | Ensure k is between 0 and 1 inclusive. Use MIN(1, MAX(0, k)) to constrain variable percentile inputs. |
| `#NUM!` | Array contains no numeric values | Verify the array contains at least one number. Check for text-formatted numbers. |
| `#VALUE!` | k argument is text or non-numeric | Ensure k is a number or a cell reference containing a number, not text like "0.5". |
| `#VALUE!` | Array reference is invalid | Check that the range reference is valid and the sheet/workbook exists. |
| `Unexpected result` | Data contains hidden or filtered rows | PERCENTILE includes hidden/filtered values. Use AGGREGATE for percentiles that ignore hidden rows. |
| `Unexpected result` | Text-formatted numbers in array | Numbers stored as text are ignored. Convert using VALUE() or multiply by 1 to convert to actual numbers. |

## Use Cases

### [[Performance Benchmarking]]

**Scenario:** An HR department needs to establish salary bands based on market data, using percentiles to define competitive compensation ranges.

**Implementation:**
```
Minimum (Entry): =PERCENTILE(MarketSalaries, 0.25)
Midpoint (Target): =PERCENTILE(MarketSalaries, 0.50)
Maximum (Premium): =PERCENTILE(MarketSalaries, 0.75)
```

**Business Application:** Using 25th, 50th, and 75th percentiles creates defensible salary ranges aligned with market data. Employees can be positioned within bands based on experience, performance, or skills. This approach is industry-standard for compensation analysis.

**Technical Details:** Consider segmenting data by job level, geography, or industry before calculating percentiles. Update market data annually. Store percentile cutoffs in reference cells for easy updates across multiple formulas.

---

### [[Student Performance Classification]]

**Scenario:** A school needs to identify students in the top 10% for honors recognition and those below the 25th percentile for additional support programs.

**Implementation:**
```
=IF(StudentScore >= PERCENTILE(AllScores, 0.9), "Honors",
   IF(StudentScore < PERCENTILE(AllScores, 0.25), "Support Needed", "Standard"))
```

**Business Application:** Percentile-based classification ensures consistent standards regardless of whether a particular test was easy or difficult. A student scoring 85% might be "Honors" in a tough year but "Standard" in an easier year, based on relative performance.

**Technical Details:** Calculate percentile thresholds once in reference cells to avoid recalculating for each student. For large datasets, this improves performance. Consider using PERCENTRANK to show each student their exact percentile standing.

---

### [[Quality Control Thresholds]]

**Scenario:** A manufacturing plant needs to establish control limits based on historical process data, flagging products that fall outside normal variation.

**Implementation:**
```
Upper Control Limit: =PERCENTILE(HistoricalMeasurements, 0.995)
Lower Control Limit: =PERCENTILE(HistoricalMeasurements, 0.005)
Alert: =IF(OR(CurrentMeasurement > UCL, CurrentMeasurement < LCL), "ALERT", "OK")
```

**Business Application:** Using 99.5th and 0.5th percentiles captures 99% of normal variation, with only 1% of measurements triggering false alerts. This is less sensitive to outliers than standard deviation-based limits and works well for non-normal distributions.

**Technical Details:** Percentile-based control limits are distribution-free, making them robust for skewed or non-normal data. For normally distributed data, 99.5th percentile approximately equals mean + 2.58 standard deviations.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 2000 onward)
- **Deprecation:** Marked as legacy function since Excel 2010; PERCENTILE.INC recommended for new work
- **Maximum array size:** Limited by worksheet size (1,048,576 rows)
- **Method:** Inclusive interpolation (k from 0 to 1 inclusive)

### Google Sheets
- **Availability:** All versions since launch
- **Deprecation:** No deprecation notice; fully supported
- **Maximum array size:** Limited by worksheet size (10,000,000 cells)
- **Method:** Identical to Excel - inclusive interpolation

### Key Difference Alert
There is no functional difference between Excel and Google Sheets for PERCENTILE. Both use identical inclusive interpolation. However, Excel officially recommends PERCENTILE.INC for new workbooks, while Google Sheets treats PERCENTILE as a current function. For cross-platform compatibility and clarity, consider using PERCENTILE.INC in Excel or when workbooks may be shared between platforms.

## Tips and Best Practices

1. **Prefer PERCENTILE.INC for new work:** While PERCENTILE works identically to PERCENTILE.INC, using the newer function makes your intention explicit and aligns with Microsoft's recommendation. This is especially important in collaborative environments.

2. **Understand inclusive vs. exclusive:** PERCENTILE (and PERCENTILE.INC) use inclusive endpoints, meaning k=0 gives the minimum and k=1 gives the maximum. For statistical applications requiring exclusive endpoints, use PERCENTILE.EXC where available.

3. **Use cell references for flexible analysis:** Instead of hardcoding percentile values, reference a cell (e.g., =PERCENTILE(Data, B1)). This allows users to explore different percentiles without editing formulas.

4. **Cache percentile thresholds for performance:** When comparing many values against the same percentile threshold, calculate the threshold once in a separate cell rather than recalculating in every comparison formula.

5. **Handle errors gracefully:** Wrap PERCENTILE in IFERROR for empty datasets: `=IFERROR(PERCENTILE(A1:A10, 0.5), "No data")`. This prevents #NUM! errors from breaking dependent calculations.

6. **Remember that data order does not matter:** PERCENTILE internally sorts the data. You do not need to sort your data range first, and the function will return the same result regardless of the original order.

7. **Consider TRIMMEAN for outlier resistance:** If extreme values are skewing your percentile analysis, TRIMMEAN automatically excludes a percentage of outliers from both ends before averaging.

8. **Verify text-formatted numbers:** Numbers stored as text are silently ignored by PERCENTILE. If your results seem off, check for the green triangle error indicator in Excel or left-aligned numbers (which typically indicates text).

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERCENTILE.INC]] | Identical to PERCENTILE with inclusive interpolation | For new workbooks in Excel 2010+; makes methodology explicit |
| [[PERCENTILE.EXC]] | Percentile with exclusive interpolation (k must be >0 and <1) | When statistical standards require exclusive endpoints or for extreme percentiles |
| [[QUARTILE]] | Returns quartile values (0, 0.25, 0.5, 0.75, 1) | When you only need standard quartile breakpoints, not arbitrary percentiles |
| [[QUARTILE.INC]] | Identical to QUARTILE with inclusive method | For new workbooks; explicit methodology |
| [[QUARTILE.EXC]] | Quartiles using exclusive method | When statistical standards require exclusive interpolation |
| [[MEDIAN]] | Returns the 50th percentile | When you specifically need the median; more readable |
| [[SMALL]] / [[LARGE]] | Returns the k-th smallest/largest value | When you need exact position rather than percentile threshold |

### Commonly Used Together

**[[PERCENTRANK]]** - Reverse of PERCENTILE

*Use PERCENTRANK to find where a value falls in the distribution:*
```
Threshold: =PERCENTILE(Scores, 0.9)
Student Rank: =PERCENTRANK(Scores, StudentScore)
```
PERCENTILE finds the value at a percentile; PERCENTRANK finds the percentile of a value. Together they provide complete distribution analysis.

---

**[[MIN]] / [[MAX]]** - Boundary values

*Verify percentile endpoints:*
```
=PERCENTILE(A1:A10, 0) = MIN(A1:A10)  → TRUE
=PERCENTILE(A1:A10, 1) = MAX(A1:A10)  → TRUE
```
Understanding that PERCENTILE(data, 0) equals MIN and PERCENTILE(data, 1) equals MAX helps validate your understanding of the function.

---

**[[IF]] / [[IFS]]** - Classification logic

*Create percentile-based categories:*
```
=IFS(Value >= PERCENTILE(Data, 0.9), "Excellent",
     Value >= PERCENTILE(Data, 0.75), "Good",
     Value >= PERCENTILE(Data, 0.5), "Average",
     TRUE, "Below Average")
```
Combine PERCENTILE with conditional logic for automated classification.

---

**[[COUNT]]** - Sample size context

*Report percentile with context:*
```
="90th percentile: " & PERCENTILE(Data, 0.9) & " (n=" & COUNT(Data) & ")"
```
Always report sample size alongside percentile values for statistical validity context.

## Official Documentation

- **Microsoft Excel:** [PERCENTILE function](https://support.microsoft.com/en-us/office/percentile-function-91b43a53-543c-4708-93de-d626debdcbb7)
- **Google Sheets:** [PERCENTILE function](https://support.google.com/docs/answer/3094083)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original function with inclusive interpolation |
| Excel 2010 | PERCENTILE.INC, PERCENTILE.EXC added | PERCENTILE marked as legacy; INC/EXC provide explicit methodology |
| Excel 2013+ | All subsequent versions | PERCENTILE retained for backward compatibility |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation; no deprecation |

---

*Last updated: 2026-01-10*
