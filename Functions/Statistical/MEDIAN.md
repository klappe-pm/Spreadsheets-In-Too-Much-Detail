---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- central-tendency
- descriptive-statistics
- robust-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- middle-value
- 50th-percentile
tags:
- statistical
- central-tendency
- median
- robust
---

# MEDIAN

## Description

The MEDIAN function returns the middle value in a dataset when values are arranged in ascending or descending order. As a measure of central tendency, the median represents the point that divides a distribution exactly in half, with 50% of values falling below it and 50% above. Unlike the arithmetic mean (AVERAGE), the median is resistant to extreme values, making it a robust statistic for describing typical values in skewed distributions or datasets containing outliers.

When a dataset contains an odd number of values, the median is simply the middle value after sorting. For datasets with an even number of values, the median is calculated as the average of the two middle values. This ensures a defined median exists for any dataset, regardless of whether a single middle value can be identified. The MEDIAN function handles this calculation automatically, sorting values internally and returning the appropriate result.

The median is particularly valuable in real estate (median home prices), income analysis (median household income), and any scenario where a few extreme values would distort the mean. For example, in a neighborhood where most homes cost $300,000 but one mansion sells for $5,000,000, the mean price would be misleadingly high while the median accurately reflects typical home values. Similarly, median income better represents what a typical worker earns than mean income, which is pulled upward by high earners.

MEDIAN ignores text values, logical values, and empty cells within the reference range, processing only numeric values. This behavior makes it robust for use with real-world data that may contain missing values or text annotations. The function accepts up to 255 arguments, which can be individual values, cell references, or ranges.

## Syntax

> [!info] Function Syntax
> ```
> =MEDIAN(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range for which to find the median | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, names, arrays, or references containing numbers. Logical values and text representations of numbers typed directly as arguments are counted. Within arrays or references, only numeric values are processed; empty cells, logical values, text, and error values are ignored.

## Examples

### Example 1: Basic Median of a Range
Find the median of sales figures.
```
=MEDIAN(B2:B13)
```
**Result:** Returns the median of the 12 monthly sales values.

### Example 2: Odd Number of Values
Find the median of 5 values.
```
=MEDIAN(10, 20, 30, 40, 50)
```
**Result:** 30 - the middle value when sorted.

### Example 3: Even Number of Values
Find the median of 6 values.
```
=MEDIAN(10, 20, 30, 40, 50, 60)
```
**Result:** 35 - the average of the two middle values (30 and 40).

### Example 4: Comparing Median to Mean
Demonstrate the difference with skewed data.
```
Data: 10, 20, 30, 40, 500
=MEDIAN(A1:A5)  → 30
=AVERAGE(A1:A5) → 120
```
**Result:** Median is unaffected by the outlier; mean is heavily influenced.

### Example 5: Real Estate Prices
Calculate median home price.
```
=MEDIAN(HomePrices!B:B)
```
**Result:** The middle home price, unaffected by luxury outliers.

### Example 6: Income Analysis
Find median household income.
```
=MEDIAN(IncomeData!C2:C10000)
```
**Result:** The income level that separates the top 50% from bottom 50%.

### Example 7: Multiple Ranges
Combine data from multiple sources.
```
=MEDIAN(Q1_Sales, Q2_Sales, Q3_Sales, Q4_Sales)
```
**Result:** Single median calculated across all quarterly data.

### Example 8: Named Range Usage
Using a defined name for clarity.
```
=MEDIAN(SurveyResponses)
```
**Result:** Median of all survey response values.

### Example 9: Mixed Data Types
Median with text and numbers mixed.
```
=MEDIAN(A1:A10)
```
Where A1:A10 contains: 5, 15, "N/A", 25, "", 35, TRUE, 45, 55, 65
**Result:** 35 - only numeric values (5, 15, 25, 35, 45, 55, 65) are considered.

### Example 10: Filtered Data Median
Calculate median for subset of data.
```
=MEDIAN(FILTER(B2:B500, A2:A500="Region A"))
```
**Result:** Median for Region A data only.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #NUM! | No numeric values in the specified range | Ensure at least 1 numeric data point exists |
| #VALUE! | Non-numeric text entered directly as argument | Use cell references or convert text to numbers |
| #NAME? | Function name misspelled | Verify spelling of MEDIAN |
| Unexpected result | Empty cells or text being ignored | Verify data range contains only intended numeric values |
| Different from expected | Confusion with mean (average) | Remember median is the middle value, not the average |

## Use Cases

### 1. Real Estate Market Analysis
**Scenario:** A real estate analyst reports on the housing market, where a few luxury properties could distort the average home price.

**Implementation:** Use median instead of mean to report typical home prices that prospective buyers can expect.

**Formula:**
```
=MEDIAN(Listings!D2:D5000)
```

**Analysis:** If the mean home price is $450,000 but the median is $320,000, this indicates a right-skewed distribution with some expensive outliers pulling the mean up. The median of $320,000 better represents what a typical buyer will spend. Reporting both measures provides insight into market distribution.

### 2. Employee Salary Benchmarking
**Scenario:** HR needs to determine typical compensation for a role to ensure competitive pay, but executive salaries would skew the average.

**Implementation:** Calculate median salary by job level to establish fair pay ranges.

**Formula:**
```
=MEDIAN(FILTER(Salaries!C:C, Salaries!B:B="Software Engineer"))
```

**Analysis:** The median salary for Software Engineers represents what a typical employee in that role earns. If a company's offer is below the median, it may struggle to attract talent. Unlike the mean, the median isn't inflated by a few highly compensated individuals.

### 3. Response Time Performance Analysis
**Scenario:** IT operations tracks server response times, but occasional spikes (during maintenance or incidents) would make mean response time misleading.

**Implementation:** Use median response time as the primary performance metric to represent normal operating conditions.

**Formula:**
```
=MEDIAN(ResponseTimes!B2:B10000)
```

**Analysis:** If mean response time is 250ms but median is 45ms, this indicates that while most requests are fast, some extreme outliers are pulling up the mean. The median of 45ms accurately represents the typical user experience. Tracking both metrics helps identify when outliers become problematic.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | All versions | Fully supported |
| Maximum arguments | 255 | 255 |
| Array formula support | Yes | Yes |
| Handles text in range | Ignores | Ignores |
| Handles logical values in range | Ignores | Ignores |
| Direct logical arguments | TRUE=1, FALSE=0 | TRUE=1, FALSE=0 |
| Empty cell handling | Ignored | Ignored |
| Precision | 15 significant digits | 15 significant digits |

**Note:** MEDIAN behaves identically across Excel and Google Sheets with no significant differences.

## Tips and Best Practices

1. **Choose median for skewed data:** When data contains outliers or is noticeably skewed (income, home prices, response times), the median provides a more representative measure of central tendency than the mean.

2. **Report both measures:** Consider presenting both median and mean together. The difference between them indicates the degree of skewness in your data. Large differences signal significant outliers.

3. **Use with percentiles:** The median is the 50th percentile. For more complete distribution analysis, combine with QUARTILE or PERCENTILE functions to understand the full spread.

4. **Consider median absolute deviation:** For robust spread measurement, pair MEDIAN with a median-based measure of dispersion rather than standard deviation, which is sensitive to outliers.

5. **Validate data first:** Since MEDIAN ignores non-numeric values silently, verify your range contains the expected number of values using COUNT or similar functions.

6. **Understand limitations:** The median provides less information about overall distribution shape than the mean combined with standard deviation. For normally distributed data, mean and median should be nearly equal.

## Related Functions

| Function | Description |
|----------|-------------|
| [[AVERAGE]] | Calculates arithmetic mean (sensitive to outliers) |
| [[MODE]] | Returns most frequently occurring value |
| [[MODE.SNGL]] | Modern function for single mode |
| [[PERCENTILE]] | Returns value at a specified percentile |
| [[QUARTILE]] | Returns quartile values (including median as Q2) |
| [[TRIMMEAN]] | Calculates mean after trimming extreme values |
| [[GEOMEAN]] | Calculates geometric mean |
| [[HARMEAN]] | Calculates harmonic mean |

## Official Documentation

- [Microsoft Excel MEDIAN function](https://support.microsoft.com/en-us/office/median-function-d0916313-4753-414c-8537-ce85bdd967d2)
- [Google Sheets MEDIAN function](https://support.google.com/docs/answer/3094025)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available in initial modern Excel releases |
| Excel 2007 | Increased argument limit from 30 to 255 |
| Excel 2010 | Performance improvements |
| Excel 2019/365 | Dynamic array compatibility |
| Google Sheets | Supported since launch with identical behavior |

---

## Median vs. Mean: When to Use Each

**Use Median when:**
- Data is skewed (income, home prices, wait times)
- Outliers are present and should not influence the result
- You want to describe the "typical" value
- Comparing groups with different outlier patterns

**Use Mean when:**
- Data is approximately normally distributed
- All values should contribute equally to the result
- You need to calculate totals (mean * count = sum)
- Performing statistical tests that assume normality

**Practical example:**
For the values: 10, 20, 30, 40, 1000
- Mean = 220 (heavily influenced by 1000)
- Median = 30 (unaffected by 1000)

If 1000 is a data entry error, the median correctly represents the data. If 1000 is legitimate, you should investigate why this outlier exists before deciding which measure to report.
