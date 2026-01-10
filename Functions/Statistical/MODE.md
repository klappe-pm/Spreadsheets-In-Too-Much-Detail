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
- frequency-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- modal-value
- most-frequent
tags:
- statistical
- central-tendency
- mode
- legacy
---

# MODE

## Description

The MODE function returns the most frequently occurring value in a dataset. As a measure of central tendency, the mode identifies the value that appears more often than any other, making it particularly useful for categorical data, discrete measurements, and identifying common patterns. Unlike the mean and median, which require numeric data on an interval or ratio scale, the mode can be meaningfully applied to nominal data where values are simply categories.

When multiple values share the highest frequency (multimodal distribution), the legacy MODE function returns only the first mode encountered in the data. This limitation led to the introduction of MODE.SNGL (which behaves identically to MODE) and MODE.MULT (which returns all modes) in Excel 2010. For datasets with no repeated values, every value is equally frequent, and MODE returns a #N/A error since no single value is more common than others.

The mode is especially valuable in business contexts for identifying popular product sizes, common price points, peak activity hours, and preferred options. In quality control, the mode helps identify the most common defect type or the most frequent measurement value. Unlike the mean, the mode is not affected by outliers, and unlike the median, it identifies an actual value from the dataset rather than a potentially interpolated result.

MODE ignores text values, logical values, and empty cells within the reference range, processing only numeric values. This function is maintained for backward compatibility; Microsoft recommends using MODE.SNGL for new projects, as it provides identical functionality with clearer naming that distinguishes it from MODE.MULT.

## Syntax

> [!info] Function Syntax
> ```
> =MODE(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range for which to find the mode | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, names, arrays, or references containing numbers. Logical values and text representations of numbers typed directly as arguments are counted. Within arrays or references, only numeric values are processed; empty cells, logical values, text, and error values are ignored.

## Examples

### Example 1: Basic Mode of a Range
Find the most common value in sales data.
```
=MODE(B2:B100)
```
**Result:** Returns the most frequently occurring sales amount.

### Example 2: Simple Dataset
Find the mode of explicit values.
```
=MODE(1, 2, 2, 3, 3, 3, 4, 4, 5)
```
**Result:** 3 - appears three times, more than any other value.

### Example 3: Tie Between Modes
When multiple values have the same highest frequency.
```
=MODE(1, 1, 2, 2, 3, 4, 5)
```
**Result:** 1 - returns the first mode encountered when tied.

### Example 4: No Mode Exists
When all values are unique.
```
=MODE(1, 2, 3, 4, 5)
```
**Result:** #N/A - no value appears more than once.

### Example 5: Survey Response Analysis
Find the most common rating.
```
=MODE(SurveyResponses!C:C)
```
**Result:** The rating value given most frequently by respondents.

### Example 6: Popular Product Size
Identify the most ordered size.
```
=MODE(Orders!F2:F5000)
```
**Result:** The size code that appears most often in orders.

### Example 7: Peak Hour Identification
Find the most common hour for transactions.
```
=MODE(HOUR(Transactions!A:A))
```
**Result:** The hour (0-23) when most transactions occur.

### Example 8: Error Handling for No Mode
Handle the #N/A error gracefully.
```
=IFERROR(MODE(A1:A10), "No mode found")
```
**Result:** Returns the mode if one exists, otherwise displays "No mode found".

### Example 9: Named Range Usage
Using a defined name.
```
=MODE(TestScores)
```
**Result:** The most common test score in the named range.

### Example 10: Mixed Data Types
Mode with text and numbers mixed.
```
=MODE(A1:A10)
```
Where A1:A10 contains: 5, 5, 10, "N/A", 10, 10, TRUE, 15, 15, 20
**Result:** 10 - appears three times (text and logical values ignored).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | No value appears more than once (no mode exists) | Use IFERROR to handle datasets without repeated values |
| #VALUE! | Non-numeric text entered directly as argument | Use cell references or convert text to numbers |
| #NAME? | Function name misspelled | Verify spelling of MODE |
| Returns first mode only | Multiple modes exist but function returns only one | Use MODE.MULT to get all modes |
| Unexpected result | Empty cells or text being ignored | Verify data range contains expected numeric values |

## Use Cases

### 1. Retail Size Optimization
**Scenario:** A clothing retailer analyzes sales data to optimize inventory by stocking more of the most popular sizes.

**Implementation:** Calculate the mode of size purchases to identify which sizes to prioritize.

**Formula:**
```
=MODE(Sales!SizeCode)
```

**Analysis:** If Medium (size code 3) is the mode, the retailer should ensure ample Medium stock. However, using MODE.MULT reveals if multiple sizes are equally popular, requiring a more nuanced stocking strategy. Comparing mode to mean helps understand if the distribution is symmetric or skewed toward certain sizes.

### 2. Customer Service Peak Hours
**Scenario:** A call center needs to staff appropriately by identifying when most customer calls occur.

**Implementation:** Find the mode of call arrival hours to determine peak staffing needs.

**Formula:**
```
=MODE(HOUR(CallLog!Timestamp))
```

**Analysis:** If the mode is 14 (2 PM), this is when most calls arrive. However, examining the full distribution is important since calls may be spread across several hours. The mode identifies the single peak, while a histogram would reveal the overall pattern.

### 3. Quality Control Defect Analysis
**Scenario:** A manufacturing plant categorizes defects by type code and wants to identify the most common defect for targeted improvement.

**Implementation:** Calculate the mode of defect type codes.

**Formula:**
```
=MODE(DefectLog!TypeCode)
```

**Analysis:** The modal defect type should receive priority attention for process improvement. If defect type 7 (e.g., surface scratches) is the mode, investigating the root cause of scratches provides the greatest quality improvement opportunity.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | All versions (legacy since Excel 2010) | Fully supported |
| Maximum arguments | 255 | 255 |
| Array formula support | Yes | Yes |
| Handles text in range | Ignores | Ignores |
| Handles logical values in range | Ignores | Ignores |
| Direct logical arguments | TRUE=1, FALSE=0 | TRUE=1, FALSE=0 |
| Empty cell handling | Ignored | Ignored |
| Returns for ties | First mode encountered | First mode encountered |
| Recommended replacement | MODE.SNGL | MODE.SNGL available |

**Note:** Both platforms return #N/A when no mode exists. Excel marks MODE as legacy, recommending MODE.SNGL.

## Tips and Best Practices

1. **Use MODE.SNGL for new work:** While MODE functions identically, MODE.SNGL has clearer naming that distinguishes it from MODE.MULT. Use MODE.SNGL for single-mode needs and MODE.MULT when multiple modes may exist.

2. **Handle #N/A gracefully:** Wrap MODE in IFERROR since datasets with unique values return #N/A. This is common with continuous data that rarely has exact duplicates.

3. **Consider rounding continuous data:** Continuous measurements rarely have exact repeats. Round values to meaningful precision (e.g., nearest 10) before finding the mode to identify the most common range.

4. **Verify mode significance:** A mode might appear only slightly more often than other values. Check the frequency to ensure the mode is meaningfully more common, not just marginally.

5. **Use with COUNT functions:** Combine with COUNTIF to verify how often the mode appears: =COUNTIF(range, MODE(range)) confirms the modal frequency.

6. **Check for multimodal distributions:** If your analysis suggests multiple common values, switch to MODE.MULT to capture all modes.

## Related Functions

| Function | Description |
|----------|-------------|
| [[MODE.SNGL]] | Modern replacement for MODE; returns single mode |
| [[MODE.MULT]] | Returns all modes as a vertical array |
| [[MEDIAN]] | Returns middle value (50th percentile) |
| [[AVERAGE]] | Calculates arithmetic mean |
| [[COUNTIF]] | Counts frequency of specific values |
| [[FREQUENCY]] | Returns frequency distribution as array |

## Official Documentation

- [Microsoft Excel MODE function](https://support.microsoft.com/en-us/office/mode-function-e45192ce-9122-4f46-8c97-9a3db002ee3e)
- [Google Sheets MODE function](https://support.google.com/docs/answer/3094029)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available in initial modern Excel releases |
| Excel 2007 | Increased argument limit from 30 to 255 |
| Excel 2010 | Marked as legacy; MODE.SNGL and MODE.MULT introduced |
| Excel 2019/365 | Continued support for backward compatibility |
| Google Sheets | Supported since launch with identical behavior |

---

## Understanding Mode in Different Distributions

**Unimodal distribution:**
- One clear peak
- MODE returns the single most common value
- Example: Test scores clustered around 75

**Bimodal distribution:**
- Two distinct peaks
- MODE returns only the first one encountered
- Use MODE.MULT to identify both peaks
- Example: Height distribution in a mixed-gender group

**Multimodal distribution:**
- Multiple peaks
- MODE returns only the first encountered
- MODE.MULT returns all modes
- Example: Restaurant peak hours (lunch and dinner)

**Uniform distribution:**
- No value more common than others
- MODE returns #N/A
- Example: Roll of a fair die over many trials
