---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- dispersion
- variability
- sample-statistics
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- standard-deviation
- sample-standard-deviation
tags:
- statistical
- dispersion
- variability
- sample
- legacy
---

# STDEV

## Description

The STDEV function calculates the standard deviation of a dataset based on a sample of the population. Standard deviation is one of the most fundamental measures of dispersion in statistics, quantifying how spread out values are from the arithmetic mean. When you have a representative sample of data rather than the complete population, STDEV applies the appropriate correction factor (using n-1 in the denominator, known as Bessel's correction) to provide an unbiased estimate of the true population standard deviation.

Understanding the difference between sample and population standard deviation is crucial for accurate statistical analysis. When you measure every single member of a population (such as test scores for all students in a specific class), you would use STDEV.P (population standard deviation). However, when working with a sample drawn from a larger population (such as surveying 100 customers from a customer base of 10,000), you should use STDEV or its modern equivalent STDEV.S. The sample standard deviation formula divides by (n-1) rather than n, which corrects for the tendency of samples to underestimate population variability.

STDEV is particularly useful in quality control, financial analysis, scientific research, and any field requiring measurement of data consistency. A low standard deviation indicates that data points cluster tightly around the mean, while a high standard deviation signals that values are spread across a wider range. In practice, standard deviation helps establish control limits, assess investment risk, evaluate measurement precision, and compare variability across different datasets or time periods.

This function is a legacy function maintained for backward compatibility. Microsoft recommends using STDEV.S for new projects, as it provides identical functionality with clearer naming that explicitly indicates sample-based calculation. STDEV ignores text values, logical values, and empty cells within the reference range, processing only numeric values in its calculation.

## Syntax

> [!info] Function Syntax
> ```
> =STDEV(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range containing sample data | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, names, arrays, or references containing numbers. Logical values and text representations of numbers typed directly into the argument list are counted. If an argument is an array or reference, only numbers in that array or reference are counted; empty cells, logical values, text, and error values in the array or reference are ignored.

## Examples

### Example 1: Basic Standard Deviation of a Range
Calculate the standard deviation of sales figures.
```
=STDEV(B2:B13)
```
**Result:** Returns the sample standard deviation of monthly sales values in B2:B13.

### Example 2: Standard Deviation of Individual Values
Calculate standard deviation of explicitly listed values.
```
=STDEV(10, 15, 20, 25, 30)
```
**Result:** 7.91 (approximately) - measures spread of the five values around their mean of 20.

### Example 3: Multiple Range Arguments
Combine data from multiple ranges.
```
=STDEV(A2:A10, C2:C10, E2:E10)
```
**Result:** Calculates a single standard deviation across all three non-contiguous ranges.

### Example 4: Quality Control - Process Variation
Measure manufacturing consistency.
```
=STDEV(Measurements!B:B)
```
**Result:** Returns the standard deviation of all measurement values, indicating process stability.

### Example 5: Comparing Variability with Mean
Calculate coefficient of variation for relative comparison.
```
=STDEV(B2:B100)/AVERAGE(B2:B100)*100
```
**Result:** Returns the coefficient of variation as a percentage, allowing comparison of variability across datasets with different means.

### Example 6: Investment Risk Assessment
Calculate volatility of monthly returns.
```
=STDEV(Returns!C2:C25)
```
**Result:** Returns the standard deviation of 24 monthly returns, commonly used as a measure of investment volatility.

### Example 7: With Named Range
Using a defined name for cleaner formulas.
```
=STDEV(SampleData)
```
**Result:** Calculates standard deviation using the named range "SampleData".

### Example 8: Handling Mixed Data Types
Standard deviation with text and numbers mixed.
```
=STDEV(A1:A10)
```
Where A1:A10 contains: 5, 10, "N/A", 15, "", 20, TRUE, 25, 30, 35
**Result:** 10.80 (approximately) - only numeric values (5, 10, 15, 20, 25, 30, 35) are included.

### Example 9: Control Chart Limits
Calculate control limits using standard deviation.
```
Upper Limit: =AVERAGE(B2:B50)+3*STDEV(B2:B50)
Lower Limit: =AVERAGE(B2:B50)-3*STDEV(B2:B50)
```
**Result:** Establishes 3-sigma control limits for quality monitoring.

### Example 10: Conditional Standard Deviation with Array
Calculate standard deviation for values meeting criteria.
```
=STDEV(IF(A2:A100="East", B2:B100))
```
**Note:** Enter as array formula (Ctrl+Shift+Enter) in older Excel versions. Returns standard deviation only for the "East" region.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #DIV/0! | Fewer than 2 numeric values provided | Ensure at least 2 numeric data points exist in the range |
| #VALUE! | Text values entered directly as arguments | Use cell references or convert text to numbers; direct text arguments cause errors |
| #NAME? | Function name misspelled or not recognized | Verify spelling; check if using correct Excel version |
| #NUM! | Calculation overflow with extremely large values | Check data for unreasonable values; consider scaling data |
| Unexpected result | Empty cells or text being ignored | Verify data range contains only intended numeric values |
| Zero result | All values are identical | This is correct behavior - no variation exists in the data |

## Use Cases

### 1. Manufacturing Quality Control
**Scenario:** A factory produces metal rods that must be 10mm in diameter with a tolerance of plus or minus 0.5mm.

**Implementation:** Quality inspectors measure samples from each production batch and calculate the standard deviation to monitor process consistency.

**Formula:**
```
=STDEV(Measurements!B2:B51)
```

**Analysis:** If the standard deviation exceeds 0.17mm (one-third of the tolerance), the process capability is insufficient. A rising standard deviation trend indicates machinery wear or process drift requiring attention. Control charts using mean plus/minus 3 standard deviations capture 99.7% of expected variation, helping identify out-of-control conditions immediately.

### 2. Portfolio Risk Management
**Scenario:** An investment analyst needs to assess the risk profile of a stock portfolio by measuring the volatility of historical returns.

**Implementation:** Calculate the standard deviation of monthly returns to quantify investment risk and compare across different securities.

**Formula:**
```
=STDEV(MonthlyReturns)*SQRT(12)
```

**Analysis:** The monthly standard deviation is annualized by multiplying by the square root of 12. This annualized volatility figure is industry-standard for comparing risk across investments. A portfolio with 15% annualized standard deviation is considered moderately volatile, while 25%+ indicates high risk. Combining this with Sharpe ratio calculations helps evaluate risk-adjusted returns.

### 3. Educational Assessment Analysis
**Scenario:** A school administrator analyzes standardized test scores to evaluate consistency across classrooms and identify students who may need additional support.

**Implementation:** Calculate standard deviation of test scores by class to understand score distribution and identify outliers.

**Formula:**
```
=STDEV(ClassA_Scores)
```

**Analysis:** A class with a mean score of 75 and standard deviation of 10 indicates most students score between 65-85. Classes with unusually high standard deviations may have significant achievement gaps requiring differentiated instruction. Comparing standard deviations across classes helps identify teaching effectiveness and curriculum consistency.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | All versions (legacy since Excel 2010) | Fully supported |
| Maximum arguments | 255 | 255 |
| Array formula support | Yes (CSE or dynamic) | Yes (native) |
| Handles text in range | Ignores | Ignores |
| Handles logical values in range | Ignores | Ignores |
| Direct logical arguments | Counts TRUE=1, FALSE=0 | Counts TRUE=1, FALSE=0 |
| Empty cell handling | Ignored | Ignored |
| Precision | 15 significant digits | 15 significant digits |
| Recommended replacement | STDEV.S | STDEV.S (also available) |

**Note:** Both platforms treat this function identically. The main difference is that Excel's documentation explicitly marks STDEV as a legacy function, recommending STDEV.S for new work.

## Tips and Best Practices

1. **Use STDEV.S for new projects:** While STDEV works perfectly, STDEV.S provides clearer intent and is the recommended modern function. Both produce identical results for sample standard deviation.

2. **Verify sample vs. population:** Before calculating, determine whether your data represents a complete population (use STDEV.P) or a sample from a larger population (use STDEV/STDEV.S). Using the wrong function introduces systematic bias.

3. **Check for sufficient data:** Standard deviation requires at least 2 data points. For meaningful statistical analysis, ensure your sample size is adequate for your application - generally 30+ observations for normal distribution assumptions.

4. **Watch for outliers:** Standard deviation is sensitive to extreme values. A single outlier can dramatically inflate the result. Consider using TRIMMEAN with manual standard deviation calculation or investigate outliers before removing them.

5. **Consider data distribution:** Standard deviation assumes roughly symmetric data distribution. For highly skewed data, consider using interquartile range (IQR) or median absolute deviation as alternative measures of spread.

6. **Document your choice:** When sharing spreadsheets, include notes explaining whether you used sample or population standard deviation and why. This prevents confusion when others interpret your analysis.

## Related Functions

| Function | Description |
|----------|-------------|
| [[STDEV.S]] | Modern replacement for STDEV; calculates sample standard deviation (identical results) |
| [[STDEV.P]] | Calculates population standard deviation (divides by n, not n-1) |
| [[STDEVA]] | Calculates sample standard deviation including text and logical values |
| [[STDEVPA]] | Calculates population standard deviation including text and logical values |
| [[VAR]] | Calculates sample variance (standard deviation squared) |
| [[VAR.S]] | Modern replacement for VAR; calculates sample variance |
| [[AVERAGE]] | Calculates the arithmetic mean, used with STDEV for complete descriptive statistics |
| [[AVEDEV]] | Calculates average absolute deviation from the mean |

## Official Documentation

- [Microsoft Excel STDEV function](https://support.microsoft.com/en-us/office/stdev-function-51fecaaa-231e-4bbb-9230-33f0e7b1e2d1)
- [Google Sheets STDEV function](https://support.google.com/docs/answer/3094054)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available in initial modern Excel releases |
| Excel 2007 | Increased argument limit from 30 to 255 |
| Excel 2010 | Marked as legacy; STDEV.S introduced as replacement |
| Excel 2019/365 | Continued support for backward compatibility |
| Google Sheets | Supported since launch with identical behavior to Excel |

---

## Sample vs. Population: Understanding the Difference

When calculating standard deviation (or variance), the choice between sample and population formulas has significant mathematical and practical implications:

**Population Standard Deviation (STDEV.P):**
- Formula: sqrt(sum((x - mean)^2) / n)
- Use when: You have measured the entire population of interest
- Example: Test scores of all 30 students in a specific class
- The divisor is n (the count of values)

**Sample Standard Deviation (STDEV/STDEV.S):**
- Formula: sqrt(sum((x - mean)^2) / (n-1))
- Use when: You have a sample representing a larger population
- Example: Survey responses from 100 customers out of 10,000 total
- The divisor is (n-1), known as Bessel's correction

**Why the difference matters:**
Samples tend to be less variable than the populations they represent because extreme values are less likely to be captured. Dividing by (n-1) instead of n corrects this bias, providing an unbiased estimate of population standard deviation. This correction becomes more significant with smaller sample sizes - for a sample of 5, the difference is 25%, while for a sample of 100, it's only about 1%.
