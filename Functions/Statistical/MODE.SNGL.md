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
- single-mode
- modal-value
tags:
- statistical
- central-tendency
- mode
- modern
---

# MODE.SNGL

## Description

The MODE.SNGL function returns the most frequently occurring value in a dataset. This is the modern, clearly-named replacement for the legacy MODE function, introduced in Excel 2010 as part of Microsoft's initiative to improve statistical function clarity. The ".SNGL" suffix explicitly indicates that this function returns a single mode value, distinguishing it from MODE.MULT, which returns all modes when multiple values share the highest frequency. Both MODE.SNGL and MODE produce identical results.

When multiple values share the highest frequency (a multimodal distribution), MODE.SNGL returns only the first mode encountered in the data. This behavior is identical to the legacy MODE function. If you need to identify all modes in a multimodal distribution, use MODE.MULT instead. For datasets with no repeated values, where every value is equally frequent, MODE.SNGL returns a #N/A error since no single value is more common than others.

The mode is particularly valuable for categorical data, discrete measurements, and identifying the most common or popular item in a dataset. Unlike the mean and median, which provide central values that may not actually exist in the dataset, the mode always returns an actual value from the data (when it exists). Common applications include identifying popular product sizes, peak business hours, most common ratings, and typical order quantities.

MODE.SNGL ignores text values, logical values (TRUE/FALSE), and empty cells within referenced ranges, counting only numeric values. However, if you type logical values or text representations of numbers directly as function arguments, they will be included in the calculation. This behavior mirrors other statistical functions and provides consistency across the Excel function library.

## Syntax

> [!info] Function Syntax
> ```
> =MODE.SNGL(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range for which to find the mode | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Note:** Arguments can be numbers, named ranges, arrays, or cell references. Within references, only numeric values are counted; empty cells, logical values, text, and errors are ignored. Direct arguments typed as numbers or logical values are included.

## Examples

### Example 1: Basic Mode of a Range
Find the most common value in a dataset.
```
=MODE.SNGL(B2:B100)
```
**Result:** Returns the value that appears most frequently.

### Example 2: Simple Dataset
Find the mode of explicit values.
```
=MODE.SNGL(5, 10, 10, 15, 15, 15, 20)
```
**Result:** 15 - appears three times, more than any other value.

### Example 3: Comparison with MODE
Demonstrate identical results.
```
=MODE(A1:A50)      → 25
=MODE.SNGL(A1:A50) → 25
```
**Result:** Both functions return the same result.

### Example 4: Tie Between Modes
When multiple values have equal highest frequency.
```
=MODE.SNGL(1, 1, 2, 2, 3)
```
**Result:** 1 - returns the first mode encountered.

### Example 5: Survey Rating Analysis
Find the most common customer rating.
```
=MODE.SNGL(Feedback!Rating)
```
**Result:** The rating value selected most often (e.g., 4 stars).

### Example 6: Popular Price Point
Identify the most common purchase price.
```
=MODE.SNGL(ROUND(Sales!Amount, -1))
```
**Result:** The price rounded to nearest $10 that occurs most frequently.

### Example 7: Error Handling
Handle the #N/A error when no mode exists.
```
=IFERROR(MODE.SNGL(A1:A10), "All values unique")
```
**Result:** Returns the mode if exists, otherwise displays message.

### Example 8: Comparing MODE.SNGL and MODE.MULT
When multimodal data exists.
```
Data: 1, 1, 2, 2, 3
=MODE.SNGL(A1:A5) → 1 (first mode only)
=MODE.MULT(A1:A5) → {1; 2} (both modes)
```
**Result:** MODE.SNGL returns only the first mode.

### Example 9: Multiple Ranges
Combine data from multiple sources.
```
=MODE.SNGL(Store1_Sizes, Store2_Sizes, Store3_Sizes)
```
**Result:** The most common size across all stores.

### Example 10: Filtered Data Mode
Calculate mode for subset of data.
```
=MODE.SNGL(FILTER(B2:B500, A2:A500="Category A"))
```
**Result:** Mode for Category A data only.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | No value appears more than once | Use IFERROR to handle datasets without repeated values |
| #VALUE! | Non-numeric text directly in arguments | Use numeric values or cell references |
| #NAME? | Function not recognized (Excel 2007 or earlier) | Use MODE in older versions; verify Excel 2010+ |
| Returns single mode | Multiple modes exist | Use MODE.MULT to capture all modes |
| Unexpected result | Numeric precision issues | Round values appropriately before finding mode |

## Use Cases

### 1. E-commerce Product Size Analysis
**Scenario:** An online clothing retailer analyzes order data to optimize inventory by determining the most popular size for each product category.

**Implementation:** Calculate the modal size for each category to prioritize stocking.

**Formula:**
```
=MODE.SNGL(FILTER(Orders!Size, Orders!Category="T-Shirts"))
```

**Analysis:** If Medium is the mode for T-shirts, ensure adequate Medium stock. Compare with MODE.MULT to check if other sizes are equally popular. Track mode changes seasonally to adjust inventory strategy.

### 2. Restaurant Peak Hour Scheduling
**Scenario:** A restaurant manager needs to optimize staff scheduling by identifying the most common hour for customer arrivals.

**Implementation:** Find the modal hour from reservation and walk-in data.

**Formula:**
```
=MODE.SNGL(HOUR(Arrivals!Timestamp))
```

**Analysis:** If the mode is 19 (7 PM), schedule maximum staff for this hour. Use MODE.MULT to identify if there's a secondary peak (e.g., lunch rush). The mode provides actionable scheduling guidance.

### 3. IT Support Ticket Classification
**Scenario:** An IT help desk wants to identify the most common issue type to create targeted documentation and training.

**Implementation:** Calculate the mode of issue category codes.

**Formula:**
```
=MODE.SNGL(Tickets!CategoryCode)
```

**Analysis:** If password reset (category 3) is the mode, prioritizing self-service password tools would have the greatest impact on ticket volume. Track the mode over time to identify emerging common issues.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Relationship to MODE | Identical results | Identical results |
| Maximum arguments | 255 | 255 |
| Array formula support | Yes | Yes |
| Handles text in range | Ignores | Ignores |
| Handles logical in range | Ignores | Ignores |
| Returns for ties | First mode encountered | First mode encountered |
| #N/A for no mode | Yes | Yes |

**Note:** MODE.SNGL behaves identically across platforms and produces the same results as the legacy MODE function.

## Tips and Best Practices

1. **Use MODE.SNGL over MODE:** Although results are identical, MODE.SNGL's naming clearly indicates single-mode behavior, improving formula readability and distinguishing from MODE.MULT.

2. **Always handle #N/A:** Continuous data rarely has exact duplicates, so wrap MODE.SNGL in IFERROR. Consider whether no mode is meaningful for your analysis.

3. **Round continuous data:** Before finding the mode of continuous measurements, round to meaningful precision. For prices, round to the nearest dollar or ten dollars; for measurements, round to the measurement precision.

4. **Verify with MODE.MULT:** If your analysis could involve multimodal data, check MODE.MULT results. If it returns multiple values, MODE.SNGL's single result may be misleading.

5. **Check modal frequency:** Use COUNTIF(range, MODE.SNGL(range)) to verify how often the mode appears. A mode appearing only slightly more often than others may not be practically significant.

6. **Consider the data type:** Mode is most meaningful for discrete and categorical data. For continuous data, consider whether binning or using median/mean is more appropriate.

## Related Functions

| Function | Description |
|----------|-------------|
| [[MODE]] | Legacy function identical to MODE.SNGL |
| [[MODE.MULT]] | Returns all modes as a vertical array |
| [[MEDIAN]] | Returns middle value; robust to outliers |
| [[AVERAGE]] | Calculates arithmetic mean |
| [[COUNTIF]] | Counts occurrences of specific values |
| [[FREQUENCY]] | Creates frequency distribution |

## Official Documentation

- [Microsoft Excel MODE.SNGL function](https://support.microsoft.com/en-us/office/mode-sngl-function-f1267c16-66c6-4386-959f-8fba5f8bb7f8)
- [Google Sheets MODE function](https://support.google.com/docs/answer/3094029)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | MODE.SNGL introduced alongside MODE.MULT |
| Excel 2013 | Performance optimizations |
| Excel 2016 | Dynamic array compatibility |
| Excel 2019/365 | Full integration with array formulas |
| Google Sheets | Available since MODE.SNGL was added |

---

## Choosing Between MODE.SNGL and MODE.MULT

**Use MODE.SNGL when:**
- You need exactly one representative value
- You're certain data is unimodal
- You need backward compatibility with MODE
- The first mode is acceptable for multimodal data

**Use MODE.MULT when:**
- Multiple common values may exist
- You need to identify all peaks in distribution
- Bimodal or multimodal patterns are expected
- Complete analysis of frequency distribution is needed

**Example comparison:**
For data: 10, 10, 20, 20, 30
- MODE.SNGL returns: 10 (first mode only)
- MODE.MULT returns: {10; 20} (both modes)

If knowing about both peaks is important for your analysis, MODE.MULT provides the complete picture.
