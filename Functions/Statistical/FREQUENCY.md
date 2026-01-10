---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- histogram
- frequency-distribution
- data-binning
- array-function
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Frequency Distribution
- Histogram Data
- Data Binning
- Frequency Count
tags:
- statistical
- frequency
- histogram
- distribution
- array-function
---

# FREQUENCY

## Description

**FREQUENCY** calculates how many values in a dataset fall into specified intervals (bins), returning a vertical array of frequency counts. This function is essential for creating histograms, analyzing distributions, and understanding how data spreads across ranges. Given a data array and an array of bin boundaries (thresholds), FREQUENCY counts how many data values fall at or below each threshold but above the previous one, plus a final count for values exceeding the last threshold.

The function returns an array with one more element than the bins array. If you specify n bin boundaries, you get n+1 frequency counts: values <= first bin, values > first bin and <= second bin, ..., values > last bin. This structure ensures all data values are counted exactly once, regardless of their range. The resulting frequencies sum to the total count of numeric values in your data.

**Array function behavior:** FREQUENCY is an array function that returns multiple values. In older Excel versions, you must select a range of cells larger than the bins array, enter the formula, and press Ctrl+Shift+Enter (CSE). In Excel 365, Google Sheets, and modern Excel with dynamic arrays, the formula automatically spills into adjacent cells. The output array is always vertical (column), regardless of whether your bins are horizontal or vertical.

**Handling edge cases:** Values exactly equal to a bin boundary count in that bin (not the next one). Empty cells and text in the data array are ignored (not counted). If the bins array contains text or is empty, FREQUENCY returns an array of zeros. Duplicate bin values are handled but may produce confusing results - use sorted, unique bin boundaries for clarity.

## Syntax

> [!f(x)] FREQUENCY Syntax
>
> ```
> =FREQUENCY(data_array, bins_array)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `data_array` | Yes | The array or range of values to count. Non-numeric values and empty cells are ignored. |
| `bins_array` | Yes | The array or range of intervals (thresholds) defining the bins. Should be sorted in ascending order for meaningful results. |

### Return Value

Returns a vertical array of frequency counts with one more element than bins_array. Each element shows the count of data values in that interval. Sum of all frequencies equals the count of numeric values in data_array.

## Examples

> [!f(x)] FREQUENCY Examples

### Example 1: Basic Frequency Distribution
```
=FREQUENCY({1,2,3,4,5,6,7,8,9,10}, {3,6,9})
```
**Result:** {3; 3; 3; 1} (vertical array)

**Explanation:** Four bins: <=3 (values 1,2,3 = 3 items), 4-6 (values 4,5,6 = 3 items), 7-9 (values 7,8,9 = 3 items), >9 (value 10 = 1 item).

---

### Example 2: Test Score Distribution
```
=FREQUENCY(Scores, {59, 69, 79, 89})
```
Where Scores contains student test scores (0-100)

**Result:** Frequency for F (<=59), D (60-69), C (70-79), B (80-89), A (90+)

**Explanation:** Five grade categories created by four bin boundaries. The last bin captures all scores 90 and above.

---

### Example 3: Age Group Analysis
```
=FREQUENCY(Ages, {17, 24, 34, 44, 54, 64})
```
**Result:** Counts for under 18, 18-24, 25-34, 35-44, 45-54, 55-64, 65+

**Explanation:** Seven age groups from six bin boundaries. Common demographic breakdown for surveys.

---

### Example 4: Single Bin (Two Categories)
```
=FREQUENCY(Data, {50})
```
**Result:** {count_at_or_below_50; count_above_50}

**Explanation:** Simplest frequency distribution: values at/below threshold and values above.

---

### Example 5: Empty Bins Array
```
=FREQUENCY({1,2,3,4,5}, {})
```
**Result:** {5}

**Explanation:** With no bins specified, all values count as "> empty" which includes everything. Rarely useful but shows edge case behavior.

---

### Example 6: Values Exactly at Bin Boundaries
```
=FREQUENCY({10, 20, 30, 40, 50}, {20, 40})
```
**Result:** {2; 2; 1}

**Explanation:** 10 and 20 in first bin (<=20), 30 and 40 in second bin (21-40), 50 in third bin (>40). Values exactly at boundaries go in that bin.

---

### Example 7: Histogram Data Preparation
```
Data in A1:A100 (100 values)
Bins in B1:B10 (10 bin edges)
Frequencies: =FREQUENCY(A1:A100, B1:B10)
```
**Result:** 11 frequency values for creating histogram chart

**Explanation:** Standard workflow for histogram preparation. Use the frequencies with a column chart for visualization.

---

### Example 8: Handling Non-Numeric Values
```
=FREQUENCY({"A", 1, "B", 2, "C", 3}, {1.5})
```
**Result:** {1; 2}

**Explanation:** Text values are ignored. Only 1, 2, 3 are counted: 1 value <=1.5 (the value 1), 2 values >1.5 (values 2 and 3).

---

### Example 9: Price Range Distribution
```
=FREQUENCY(ProductPrices, {10, 25, 50, 100, 200})
```
**Result:** Distribution across price tiers

**Explanation:** Six price tiers: $0-10, $11-25, $26-50, $51-100, $101-200, $201+. Useful for pricing analysis.

---

### Example 10: Response Time Analysis
```
=FREQUENCY(ResponseTimes, {1, 2, 5, 10, 30})
```
Where ResponseTimes is in seconds

**Result:** Counts for <=1s, 2s, 3-5s, 6-10s, 11-30s, >30s

**Explanation:** Service level analysis: how many responses meet various SLA thresholds.

---

### Example 11: Using with Dynamic Arrays (Excel 365/Sheets)
```
=FREQUENCY(A:A, B1:B5)
```
**Result:** Automatically spills to 6 cells

**Explanation:** Modern spreadsheets don't require CSE entry. Formula automatically returns all frequencies to adjacent cells.

---

### Example 12: Combining with SUM for Verification
```
=SUM(FREQUENCY(Data, Bins))
```
**Result:** Should equal COUNT(Data)

**Explanation:** The sum of all frequencies must equal the total count of numeric values. Use this to verify your frequency distribution is complete.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid array reference | Check that data_array and bins_array are valid ranges or arrays. |
| `Single value instead of array` | Not entered as array formula (older Excel) | Select output range, enter formula, press Ctrl+Shift+Enter. |
| `Frequencies don't sum to data count` | Non-numeric values in data | Check for text or errors in data_array. These are silently ignored. |
| `All frequencies are zero` | Bins array is text or invalid | Ensure bins_array contains numeric values. |
| `Unexpected distribution` | Unsorted bins | Sort bins in ascending order for meaningful results. |
| `Missing counts` | Output range too small | Output needs one more cell than bins. For n bins, need n+1 cells. |

## Use Cases

### [[Histogram Creation for Data Visualization]]

**Scenario:** A data analyst creates a histogram to visualize the distribution of customer order values.

**Implementation:**
```
Order data in A2:A1001 (1000 orders)
Bins: {0, 25, 50, 75, 100, 150, 200, 300, 500}
Frequencies: =FREQUENCY(A2:A1001, bins)
Use frequencies with bin labels for column chart
```

**Business Application:** Histograms reveal distribution shape that summary statistics miss. Seeing that most orders are $25-75 with a long tail of high-value orders informs pricing strategy, inventory planning, and customer segmentation. The visual immediately shows whether distribution is normal, skewed, bimodal, etc.

**Technical Details:** Choose bins to create 10-20 bars for clarity. Equal-width bins are standard but consider using meaningful business thresholds (price tiers, performance bands). Label each bin with its range for interpretation.

---

### [[Survey Response Analysis]]

**Scenario:** A market researcher analyzes Likert scale survey responses to understand satisfaction distribution.

**Implementation:**
```
Responses: 1-5 scale in A2:A501 (500 responses)
Bins: {1, 2, 3, 4}
Frequencies: =FREQUENCY(A2:A501, {1, 2, 3, 4})
```
Result shows count for each rating: 1, 2, 3, 4, 5

**Business Application:** Distribution of satisfaction scores reveals more than the average. Two products might both average 3.5 stars, but one has mostly 3s and 4s (consistent mediocrity) while another splits between 1s and 5s (polarizing). FREQUENCY reveals these patterns for strategic decisions.

**Technical Details:** For integer scales, set bins at each integer to get exact counts per value. For continuous scales, choose meaningful intervals. Consider normalizing frequencies to percentages for comparison across different sample sizes.

---

### [[Performance Metric Categorization]]

**Scenario:** An HR analyst categorizes employee performance scores into rating bands for compensation decisions.

**Implementation:**
```
Performance scores: 0-100 in A2:A201
Bins: {59, 69, 79, 89} for grade boundaries
=FREQUENCY(A2:A201, {59, 69, 79, 89})
```
Result: counts for "Needs Improvement", "Meets Expectations", "Exceeds", "Outstanding", "Exceptional"

**Business Application:** Performance distributions inform calibration discussions. If 80% score "Exceptional," either the team is genuinely excellent or ratings are inflated. FREQUENCY enables objective analysis of rating distributions across departments, managers, or time periods.

**Technical Details:** Use consistent bin boundaries across analyses for valid comparisons. Document boundary meanings (e.g., "79 means score 70-79 inclusive"). Consider whether boundaries should be 70 or 69.999 depending on tie-breaking rules.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Array entry:** Requires Ctrl+Shift+Enter in Excel 2019 and earlier
- **Dynamic arrays:** Excel 365 supports automatic spilling
- **Output direction:** Always vertical array
- **Maximum size:** Limited by worksheet dimensions

### Google Sheets

- **Availability:** All versions since launch
- **Array entry:** Automatic (no CSE required)
- **Spilling:** Automatically expands to needed cells
- **Output direction:** Always vertical array
- **Behavior:** Very similar to Excel

### Key Difference Alert

The main platform difference is array entry behavior:
- **Excel 2019 and earlier:** Requires selecting output range and Ctrl+Shift+Enter
- **Excel 365:** Automatic spilling with dynamic arrays
- **Google Sheets:** Always automatic, no special entry needed

Both platforms produce identical frequency counts for the same inputs. The output is always a vertical array with n+1 elements for n bins.

## Tips and Best Practices

1. **Plan output size.** FREQUENCY returns one more value than the number of bins. For 10 bins, you need space for 11 values.

2. **Sort bins in ascending order.** Unsorted bins produce confusing results. Always verify bins are ordered correctly.

3. **Verify with SUM.** The sum of frequencies should equal COUNT of numeric data values. If not, check for text or errors in data.

4. **Use meaningful bin boundaries.** Don't use arbitrary intervals. Choose bins that represent meaningful categories (grade boundaries, price tiers, time thresholds).

5. **Consider equal-width vs. custom bins.** Equal-width bins are standard for general histograms. Custom bins are better when specific thresholds matter (pass/fail at 60, SLA at 5 seconds).

6. **Remember boundary behavior.** Values exactly at a bin edge count in that bin, not the next. This matters for integer data.

7. **Label bins clearly.** When presenting results, label each bin with its range (e.g., "25-50" not just "50").

8. **Use for data quality checks.** Unexpected frequency distributions (all zeros, extreme outliers) often reveal data problems.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COUNTIF]] | Count values meeting one criterion | For counting single categories, not distributions |
| [[COUNTIFS]] | Count with multiple criteria | For complex counting conditions |
| [[SUMPRODUCT]] | Flexible array calculations | For custom frequency logic |
| [[HISTOGRAM]] (chart) | Visual histogram | For direct visualization without frequency data |

### Commonly Used Together

**[[COUNT]]** - Verify total

*Validation check:*
```
Total data: =COUNT(data_array)
Total frequencies: =SUM(FREQUENCY(data_array, bins_array))
```
These should be equal.

---

**[[MIN]] and [[MAX]]** - Determine bin range

*Set bin boundaries:*
```
Minimum: =MIN(data_array)
Maximum: =MAX(data_array)
Bins: Create evenly spaced values between min and max
```

---

**[[PERCENTILE]]** - Create quantile-based bins

*Equal-frequency bins:*
```
=PERCENTILE(data_array, 0.25) for quartile boundaries
=PERCENTILE(data_array, 0.2) for quintile boundaries
```
Use percentiles as bins for equal-sized groups.

## Official Documentation

- **Microsoft Excel:** [FREQUENCY function](https://support.microsoft.com/en-us/office/frequency-function-44e3be2b-edd9-4aba-bf42-1fcc51f8f6f7)
- **Google Sheets:** [FREQUENCY function](https://support.google.com/docs/answer/3094286)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function (CSE array) |
| Excel 2007 | Continued support | No changes |
| Excel 365 | 2019+ | Dynamic array support (automatic spilling) |
| Google Sheets | Original launch (2006) | Always supported automatic expansion |

---

*Last updated: 2026-01-10*
