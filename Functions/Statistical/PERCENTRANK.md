---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- percentile-rank
- distribution-analysis
- relative-standing
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Percentile Rank
- Percent Rank
- Relative Position
tags:
- statistical
- percentile
- ranking
- distribution
- data-analysis
- legacy-function
---

# PERCENTRANK

## Description

**PERCENTRANK** returns the rank of a value in a dataset as a percentage of the dataset, telling you what fraction of values fall below the specified value. This is the inverse of the PERCENTILE function: while PERCENTILE answers "what value is at the 75th percentile?", PERCENTRANK answers "what percentile does this value represent?" The result ranges from 0 to 1 (or 0% to 100%), indicating the relative standing of the value within the distribution.

The function uses inclusive interpolation to calculate the percentile rank. If the specified value exists in the dataset, PERCENTRANK returns its exact percentile position. If the value falls between two data points, the function performs linear interpolation to estimate the percentile rank. The minimum value in the dataset returns 0 (0th percentile), and the maximum returns a value approaching 1 (but the exact value depends on dataset size and the optional significance parameter).

**Key gotcha:** PERCENTRANK is a legacy function that Microsoft retained for backward compatibility. In Excel 2010 and later, Microsoft introduced PERCENTRANK.INC (which is functionally identical to PERCENTRANK) and PERCENTRANK.EXC (which uses exclusive interpolation). The legacy PERCENTRANK truncates results to the specified significance (default 3 decimal places), which can cause unexpected precision loss. For example, a true rank of 0.12345 becomes 0.123 by default. Use PERCENTRANK.INC with appropriate significance for more precision.

**Platform behavior:** Both Excel and Google Sheets implement PERCENTRANK identically using the inclusive method. The optional third parameter (significance) controls decimal precision, defaulting to 3 decimal places. Both platforms return #N/A if the value is outside the data range (less than the minimum or greater than the maximum). Empty cells and text within the array are ignored.

## Syntax

> [!f(x)] PERCENTRANK Syntax
>
> ```
> =PERCENTRANK(array, x, [significance])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset. Empty cells, text values, and logical values are ignored. Must contain at least one numeric value. |
| `x` | Yes | The value for which you want to find the percentile rank. Must be within the range of values in the array (between MIN and MAX inclusive). |
| `significance` | No | The number of significant digits for the returned percentage. Defaults to 3. Higher values provide more precision but may not be meaningful for small datasets. |

### Return Value

Returns a decimal value between 0 and 1 representing the percentile rank of x in the dataset, truncated to the specified significance. Returns #N/A if x is outside the data range. Returns #VALUE! if arguments are non-numeric. Returns #NUM! if significance is less than 1.

## Examples

> [!f(x)] PERCENTRANK Examples

### Example 1: Basic Percentile Rank
```
=PERCENTRANK(A1:A10, 5)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result:** 0.444

**Explanation:** The value 5 is the 5th of 10 values. Its percentile rank is (5-1)/(10-1) = 4/9 = 0.444 (truncated to 3 decimal places). This means approximately 44.4% of values are below 5.

---

### Example 2: Minimum Value Rank
```
=PERCENTRANK(A1:A5, 10)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** 0

**Explanation:** The minimum value in a dataset always has a percentile rank of 0, meaning 0% of values are below it.

---

### Example 3: Maximum Value Rank
```
=PERCENTRANK(A1:A5, 50)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** 1

**Explanation:** The maximum value approaches rank 1. With the inclusive method and 5 values, the 5th value has rank (5-1)/(5-1) = 1.

---

### Example 4: Interpolated Value Not in Dataset
```
=PERCENTRANK(A1:A5, 35)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** 0.625

**Explanation:** 35 is not in the dataset but falls between 30 (rank 0.5) and 40 (rank 0.75). Linear interpolation: 0.5 + 0.5 * (35-30)/(40-30) = 0.5 + 0.5 * 0.5 = 0.625.

---

### Example 5: Controlling Decimal Precision
```
=PERCENTRANK(A1:A10, 5, 5)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result:** 0.44444

**Explanation:** With significance set to 5, the result shows 5 decimal places instead of the default 3. The true value is 4/9 = 0.44444...

---

### Example 6: Error for Value Outside Range
```
=PERCENTRANK(A1:A5, 100)
```
Where A1:A5 contains: 10, 20, 30, 40, 50

**Result:** #N/A

**Explanation:** The value 100 exceeds the maximum (50) in the dataset. PERCENTRANK cannot extrapolate beyond observed data. Use IFERROR to handle this gracefully.

---

### Example 7: Unsorted Data
```
=PERCENTRANK(A1:A5, 30)
```
Where A1:A5 contains: 40, 10, 50, 20, 30

**Result:** 0.5

**Explanation:** PERCENTRANK internally sorts the data. The value 30 is the median of {10, 20, 30, 40, 50}, so its percentile rank is 0.5 (50th percentile).

---

### Example 8: Duplicate Values
```
=PERCENTRANK(A1:A5, 30)
```
Where A1:A5 contains: 10, 30, 30, 30, 50

**Result:** 0.25

**Explanation:** When there are duplicates, PERCENTRANK uses the first occurrence for positioning. With 5 values, the first 30 is in position 2, giving rank (2-1)/(5-1) = 0.25.

---

### Example 9: Single Value Dataset
```
=PERCENTRANK(A1, 5)
```
Where A1 contains: 5

**Result:** 0 or 1 (implementation dependent)

**Explanation:** With a single value, the percentile rank calculation is trivial. If x equals that value, the result is typically 0 or 1. Avoid using PERCENTRANK with very small datasets.

---

### Example 10: Practical Student Ranking
```
=TEXT(PERCENTRANK(AllScores, StudentScore), "0%") & " percentile"
```

**Result:** "75% percentile" (example)

**Explanation:** Convert the decimal result to a percentage for user-friendly display. A student at the 75th percentile scored higher than 75% of all students.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | x is less than the minimum value in the array | Ensure x is within the data range. Use IFERROR to handle out-of-range values. |
| `#N/A` | x is greater than the maximum value in the array | Ensure x is within the data range. Consider using MIN/MAX to validate before calling PERCENTRANK. |
| `#VALUE!` | x or array contains non-numeric values as direct arguments | Ensure all arguments are numeric. Cell references to text will cause errors. |
| `#NUM!` | significance is less than 1 | Use significance of 1 or greater. Default (omitted) uses 3 decimal places. |
| `Unexpected precision loss` | Default significance of 3 truncates results | Increase significance parameter for more decimal places if needed. |
| `Unexpected result with duplicates` | Duplicate values affect rank calculation | Understand that PERCENTRANK uses the position of the first occurrence of duplicate values. |

## Use Cases

### [[Student Performance Reporting]]

**Scenario:** A school district needs to report each student's standardized test score as a percentile rank, showing parents where their child stands relative to all test-takers.

**Implementation:**
```
=PERCENTRANK(AllScores, StudentScore, 2)

Display: =TEXT(PERCENTRANK(AllScores, StudentScore), "0") & "th percentile"

Classification: =IF(PERCENTRANK(AllScores, StudentScore) >= 0.9, "Advanced",
                   IF(PERCENTRANK(AllScores, StudentScore) >= 0.5, "Proficient", "Needs Improvement"))
```

**Business Application:** Percentile ranks are the standard way to report standardized test results because they are intuitive for parents and consistent across different tests with varying difficulty levels. A student at the 75th percentile performed better than 75% of test-takers.

**Technical Details:** Use a significance of 0 or 1 for student reports (whole percentiles are clearer). Calculate percentile ranks against the appropriate norm group (grade level, school, district, or national). Document which dataset was used for ranking.

---

### [[Sales Representative Performance Ranking]]

**Scenario:** A sales manager needs to rank each representative's quarterly performance against the team, identifying top performers for recognition and those needing coaching.

**Implementation:**
```
Performance Rank: =PERCENTRANK(TeamSales, RepSales)
Performance Label: =IF(PERCENTRANK(TeamSales, RepSales) >= 0.9, "Star Performer",
                      IF(PERCENTRANK(TeamSales, RepSales) < 0.25, "Needs Support", "On Track"))

Dashboard: =REPT("*", ROUND(PERCENTRANK(TeamSales, RepSales) * 5, 0))
```

**Business Application:** Percentile ranking enables fair comparison across territories with different market sizes. A rep in a small territory can still rank highly if they outperform expectations relative to peers. This is more motivating than raw sales rankings that favor large territories.

**Technical Details:** Update the comparison dataset quarterly to reflect current team composition. Consider separate rankings for new hires (first 6 months) versus experienced reps. Use PERCENTRANK.INC for modern workbooks.

---

### [[Quality Measurement Benchmarking]]

**Scenario:** A manufacturing plant compares each batch's quality metrics against historical data to identify whether current production meets standards.

**Implementation:**
```
Quality Percentile: =PERCENTRANK(HistoricalQuality, CurrentBatch)
Status: =IF(PERCENTRANK(HistoricalQuality, CurrentBatch) < 0.05, "ALERT: Below 5th Percentile",
           IF(PERCENTRANK(HistoricalQuality, CurrentBatch) > 0.95, "Exceptional Quality", "Normal"))

Trend: =AVERAGE(PERCENTRANK(HistoricalQuality, LastBatch),
                PERCENTRANK(HistoricalQuality, CurrentBatch))
```

**Business Application:** Percentile-based quality monitoring automatically adjusts for process improvements over time. As the historical dataset improves, a batch that was once at the 50th percentile might drop to the 30th percentile, flagging potential issues relative to improved capabilities.

**Technical Details:** Maintain a rolling historical dataset (e.g., last 100 batches) to keep rankings relevant. Very old data may not reflect current process capabilities. Consider separate rankings for different product lines or equipment.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 2000 onward)
- **Deprecation:** Marked as legacy function since Excel 2010; PERCENTRANK.INC recommended
- **Default significance:** 3 decimal places
- **Maximum array size:** Limited by worksheet size

### Google Sheets
- **Availability:** All versions since launch
- **Deprecation:** No deprecation notice; fully supported
- **Default significance:** 3 decimal places (identical to Excel)
- **Maximum array size:** Limited by worksheet size

### Key Difference Alert
PERCENTRANK behaves identically in Excel and Google Sheets. The only consideration is that Excel officially recommends PERCENTRANK.INC for new workbooks, while Google Sheets has not deprecated PERCENTRANK. For cross-platform compatibility and clarity about methodology, consider using PERCENTRANK.INC.

## Tips and Best Practices

1. **Prefer PERCENTRANK.INC for new work:** Although functionally identical, PERCENTRANK.INC explicitly signals the inclusive methodology. This is important for documentation and collaboration.

2. **Increase significance for precision:** The default 3 decimal places may not be sufficient for precise calculations. Use significance of 6-10 when percentile ranks feed into further calculations.

3. **Handle out-of-range values:** PERCENTRANK returns #N/A for values outside the data range. Wrap in IFERROR: `=IFERROR(PERCENTRANK(Data, x), IF(x < MIN(Data), 0, 1))` to assign 0 or 1 to out-of-range values.

4. **Combine with PERCENTILE for validation:** PERCENTRANK and PERCENTILE are inverses. Verify: `PERCENTILE(Data, PERCENTRANK(Data, x)) should approximately equal x`.

5. **Understand duplicate handling:** When values repeat, PERCENTRANK uses the position of the first occurrence. All identical values receive the same rank, which may or may not match your expectations.

6. **Consider RANK for ordinal ranking:** If you need ordinal ranks (1st, 2nd, 3rd) rather than percentile ranks, use RANK.AVG or RANK.EQ instead.

7. **Use meaningful datasets:** Very small datasets (fewer than 10 values) produce coarse percentile ranks. Ensure your dataset is large enough for percentile analysis to be meaningful.

8. **Document your norm group:** When reporting percentile ranks, always document what dataset was used for comparison. "90th percentile of your class" means something different than "90th percentile nationally."

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERCENTRANK.INC]] | Identical to PERCENTRANK with inclusive interpolation | For new workbooks in Excel 2010+; makes methodology explicit |
| [[PERCENTRANK.EXC]] | Percentile rank with exclusive interpolation | When statistical standards require exclusive endpoints |
| [[RANK.EQ]] | Returns ordinal rank (1, 2, 3...) | When you need position rather than percentile |
| [[RANK.AVG]] | Returns ordinal rank, averaging ties | When you need position with tie handling |
| [[PERCENTILE]] | Returns value at a given percentile | The inverse operation: finding the value at a percentile |

### Commonly Used Together

**[[PERCENTILE]]** - Inverse of PERCENTRANK

*Validate the relationship between these functions:*
```
Rank: =PERCENTRANK(Data, 45)  ' Returns 0.75
Verify: =PERCENTILE(Data, 0.75)  ' Should return approximately 45
```
PERCENTRANK finds the percentile of a value; PERCENTILE finds the value at a percentile.

---

**[[TEXT]]** - Format for display

*Create user-friendly percentile labels:*
```
=TEXT(PERCENTRANK(Scores, MyScore), "0%") & " percentile"
' Returns "75% percentile"

=TEXT(PERCENTRANK(Scores, MyScore) * 100, "0") & "th percentile"
' Returns "75th percentile"
```
Format decimal percentile ranks as readable percentages.

---

**[[IF]] / [[IFS]]** - Create tier classifications

*Classify values by percentile standing:*
```
=IFS(PERCENTRANK(Data, Value) >= 0.9, "Exceptional",
     PERCENTRANK(Data, Value) >= 0.75, "Above Average",
     PERCENTRANK(Data, Value) >= 0.25, "Average",
     TRUE, "Below Average")
```
Use percentile ranks to drive classification logic.

---

**[[IFERROR]]** - Handle out-of-range values

*Prevent #N/A errors for extreme values:*
```
=IFERROR(PERCENTRANK(Data, Value),
         IF(Value > MAX(Data), 1, 0))
```
Assign meaningful ranks to values outside the dataset.

## Official Documentation

- **Microsoft Excel:** [PERCENTRANK function](https://support.microsoft.com/en-us/office/percentrank-function-f1b5836c-9619-4847-9fc9-080ec9024442)
- **Google Sheets:** [PERCENTRANK function](https://support.google.com/docs/answer/3094087)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original function with inclusive interpolation |
| Excel 2010 | PERCENTRANK.INC, PERCENTRANK.EXC added | PERCENTRANK marked as legacy; INC/EXC provide explicit methodology |
| Excel 2013+ | All subsequent versions | PERCENTRANK retained for backward compatibility |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation; no deprecation |

---

*Last updated: 2026-01-10*
