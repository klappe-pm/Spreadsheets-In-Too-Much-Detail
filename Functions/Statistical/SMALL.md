---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- order-statistics
- ranking
- bottom-n-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- kth-smallest
- nth-smallest
- bottom-value
tags:
- statistical
- ranking
- order-statistics
---

# SMALL

## Description

The SMALL function returns the k-th smallest value from a dataset, enabling flexible extraction of bottom values without sorting the entire range. This order statistic function is invaluable for identifying minimum values, lowest performers, analyzing the lower tail of a distribution, or finding specific percentile boundaries. By specifying different values of k, you can retrieve the smallest (k=1), second smallest (k=2), third smallest (k=3), and so on.

Unlike MIN, which returns only the single smallest value, SMALL provides access to any position in the ascending sorted order. This flexibility enables powerful applications such as finding the 3rd lowest bid, identifying the bottom 5 performing products, calculating medians (the middle value), or removing outliers by excluding extreme low values. The function pairs naturally with LARGE, which performs the complementary operation of finding k-th largest values.

SMALL is particularly useful in financial analysis for finding minimum thresholds, in quality control for identifying the lowest measurements, in competitive bidding for analyzing price distributions, and in data cleaning for detecting and handling outliers at the low end of the distribution. By varying the k parameter, you can analyze how values are distributed among the lowest observations.

The function ignores text values, logical values, and empty cells in the range, counting only numeric values when determining positions. If k exceeds the count of numeric values or is less than 1, SMALL returns a #NUM! error. The k parameter must be a positive integer; decimal values are truncated.

## Syntax

> [!info] Function Syntax
> ```
> =SMALL(array, k)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| array | The range or array of numeric values from which to extract the k-th smallest | Yes |
| k | The position from the smallest (1 = smallest, 2 = second smallest, etc.) | Yes |

**Note:** The array can be a range, named range, or array of numbers. Text, logical values, and empty cells are ignored. The k value must be between 1 and the count of numeric values in the array.

## Examples

### Example 1: Smallest Value
Find the minimum value (equivalent to MIN).
```
=SMALL(B2:B100, 1)
```
**Result:** Returns the smallest value in the range (same as MIN).

### Example 2: Second Smallest Value
Find the second lowest value.
```
=SMALL(B2:B100, 2)
```
**Result:** Returns the second smallest value.

### Example 3: Bottom 3 Prices
List the 3 lowest prices.
```
=SMALL(Prices!B:B, 1)  → Lowest
=SMALL(Prices!B:B, 2)  → Second lowest
=SMALL(Prices!B:B, 3)  → Third lowest
```
**Result:** The three lowest prices in the dataset.

### Example 4: Calculate Median Using SMALL
Find the median for odd number of values.
```
=SMALL(A1:A9, (COUNT(A1:A9)+1)/2)
```
**Result:** For 9 values, returns the 5th smallest (the median).

### Example 5: Sum of Bottom 5 Values
Calculate total of five lowest values.
```
=SUM(SMALL(B2:B100, {1,2,3,4,5}))
```
**Result:** Sum of the 5 smallest values using array constant.

### Example 6: Dynamic Bottom N with Sequence
Sum bottom N where N is variable.
```
=SUM(SMALL(B2:B100, SEQUENCE(5)))
```
**Result:** Sum of bottom 5 using SEQUENCE (Excel 365).

### Example 7: Average Excluding Lowest Outliers
Calculate mean after removing 3 lowest values.
```
=(SUM(B2:B50)-SUM(SMALL(B2:B50,{1,2,3})))/(COUNT(B2:B50)-3)
```
**Result:** Average excluding the 3 lowest outliers.

### Example 8: Finding Quartile Boundaries
Calculate Q1 (25th percentile) manually.
```
=SMALL(Data, ROUND(COUNT(Data)*0.25, 0))
```
**Result:** Approximate first quartile value.

### Example 9: Creating Ascending Ranked List
Build a sorted list in ascending order.
```
In cells B2:B11, use:
=SMALL(AllValues, ROW()-1)
```
**Result:** Dynamic ascending sorted list of first 10 values.

### Example 10: Handle Missing Positions
Handle cases where k exceeds data count.
```
=IFERROR(SMALL(A1:A20, 25), "Not enough data")
```
**Result:** Returns error message if fewer than 25 values exist.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #NUM! | k is larger than the count of numeric values | Verify k is between 1 and COUNT(array) |
| #NUM! | k is less than 1 or zero | Ensure k is a positive integer |
| #VALUE! | k is not a number | Provide a numeric value for k |
| #NAME? | Function name misspelled | Verify spelling of SMALL |
| Unexpected result | Text or empty cells in range | These are ignored; verify intended values are included |
| Decimal k value | k is not an integer | k is truncated to integer (2.9 becomes 2) |

## Use Cases

### 1. Competitive Bid Analysis
**Scenario:** A procurement manager analyzes vendor bids to understand pricing distribution and identify the lowest competitive offers.

**Implementation:** Use SMALL to extract and analyze the lowest bids submitted.

**Formula:**
```
Lowest 3: =SMALL(Bids!Amount, {1,2,3})
Gap between 1st and 2nd: =SMALL(Bids!Amount, 2) - SMALL(Bids!Amount, 1)
```

**Analysis:** The lowest bid wins, but analyzing the gap between lowest bids reveals how competitive the bidding was. A small gap suggests intense competition; a large gap might indicate the lowest bidder made an error or has unsustainable pricing.

### 2. Outlier Detection and Removal
**Scenario:** A data analyst needs to calculate summary statistics after removing extreme low values that may be data entry errors.

**Implementation:** Identify the lowest values to investigate, then calculate statistics excluding them.

**Formula:**
```
Bottom 3: =SMALL(Data, {1,2,3})
Mean without bottom 3: =(SUM(Data)-SUM(SMALL(Data,{1,2,3})))/(COUNT(Data)-3)
```

**Analysis:** If the bottom 3 values are significantly lower than expected (e.g., 0 or negative when values should be positive), they may be errors. After investigation, excluding confirmed errors provides more accurate summary statistics.

### 3. Response Time SLA Compliance
**Scenario:** An IT operations team needs to identify the fastest response times to understand best-case performance and set realistic targets.

**Implementation:** Analyze the distribution of fastest responses.

**Formula:**
```
=SMALL(ResponseTimes, SEQUENCE(10))
```

**Analysis:** The 10 fastest response times show what's achievable under ideal conditions. If the fastest times are dramatically better than the median, investigate what conditions enabled those responses to replicate them more consistently.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | All versions | Fully supported |
| Array constant support | Yes ({1,2,3}) | Yes |
| SEQUENCE integration | Excel 365/2019 | Yes |
| Maximum array size | Limited by memory | Limited by memory |
| Handles text in range | Ignores | Ignores |
| Handles logical in range | Ignores | Ignores |
| Decimal k handling | Truncates to integer | Truncates to integer |
| Precision | 15 significant digits | 15 significant digits |

**Note:** SMALL behaves consistently across platforms with no significant differences.

## Tips and Best Practices

1. **Pair with LARGE for complete analysis:** Use SMALL for bottom values and LARGE for top values to analyze both tails of your distribution.

2. **Use for median calculation:** For n values, SMALL(array, (n+1)/2) gives the median when n is odd. For even n, average SMALL at positions n/2 and n/2+1.

3. **Create sorted lists without sorting:** Use SMALL with ROW() or SEQUENCE to create ascending sorted lists that update automatically.

4. **Handle negative values correctly:** SMALL correctly orders negative numbers (e.g., -10 is smaller than -5).

5. **Validate k parameter:** Wrap SMALL in IFERROR or check that k does not exceed COUNT(array) to prevent #NUM! errors.

6. **Consider MINIFS for conditional:** If you need the smallest value meeting specific criteria, MINIFS may be more direct than filtering before SMALL.

## Related Functions

| Function | Description |
|----------|-------------|
| [[LARGE]] | Returns k-th largest value (opposite of SMALL) |
| [[MIN]] | Returns only the smallest value (equivalent to SMALL(array, 1)) |
| [[MAX]] | Returns only the largest value |
| [[RANK]] | Returns the position/rank of a value within a dataset |
| [[PERCENTILE]] | Returns value at a specified percentile position |
| [[QUARTILE]] | Returns quartile values |
| [[MEDIAN]] | Returns the middle value (50th percentile) |

## Official Documentation

- [Microsoft Excel SMALL function](https://support.microsoft.com/en-us/office/small-function-17da8222-7c82-42b2-961b-14c45384df07)
- [Google Sheets SMALL function](https://support.google.com/docs/answer/3094050)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available in initial modern Excel releases |
| Excel 2007 | Increased range support |
| Excel 2010 | Performance improvements |
| Excel 2019/365 | Dynamic array support; works with SEQUENCE |
| Google Sheets | Supported since launch with identical behavior |

---

## SMALL vs. LARGE: Complementary Functions

**SMALL(array, k):**
- Returns k-th value counting from smallest
- SMALL(array, 1) = MIN(array)
- Ascending order perspective

**LARGE(array, k):**
- Returns k-th value counting from largest
- LARGE(array, 1) = MAX(array)
- Descending order perspective

**Relationship:**
For n values in array:
- SMALL(array, k) = LARGE(array, n-k+1)
- SMALL(array, 1) = LARGE(array, n)

**Use both for trimmed statistics:**
```
Trimmed mean (exclude top and bottom 3):
=(SUM(array)-SUM(SMALL(array,{1,2,3}))-SUM(LARGE(array,{1,2,3})))/(COUNT(array)-6)
```
