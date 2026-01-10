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
- inclusive-interpolation
- distribution-analysis
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Quartile Inclusive
- QUARTILE Inclusive
- Inclusive Quartile
tags:
- statistical
- quartile
- percentile
- distribution
- data-analysis
- inclusive
---

# QUARTILE.INC

## Description

**QUARTILE.INC** returns the quartile of a dataset using inclusive interpolation, dividing the distribution at the 0th, 25th, 50th, 75th, and 100th percentiles. The "INC" suffix stands for "inclusive," indicating that the function includes both endpoints: quart=0 returns the minimum and quart=4 returns the maximum. This function is functionally identical to the legacy QUARTILE function but explicitly documents the interpolation method being used.

The inclusive interpolation method calculates quartile positions using the formula: position = (quart/4) * (n-1) + 1, where n is the count of values. When the position falls between two data points, QUARTILE.INC performs linear interpolation. For example, with 10 values, Q1 (quart=1) falls at position (0.25 * 9) + 1 = 3.25, so the function returns 75% of the 3rd value plus 25% of the 4th value. QUARTILE.INC(data, 1) is equivalent to PERCENTILE.INC(data, 0.25).

**Key gotcha:** The distinction between QUARTILE.INC and QUARTILE.EXC matters for small datasets and when precise statistical methodology is required. With INC, the minimum and maximum are always achievable (quart=0 and quart=4). With EXC, the quart parameter cannot be 0 or 4, and the function requires sufficient data points. For most business applications, INC is preferred because users intuitively expect quartile 0 to mean minimum and quartile 4 to mean maximum.

**Platform behavior:** Excel introduced QUARTILE.INC in Excel 2010 to replace the ambiguous legacy QUARTILE function. Google Sheets fully supports QUARTILE.INC with identical behavior. Both platforms accept quart values 0-4, use linear interpolation for non-integer positions, and ignore text and empty cells in the array.

## Syntax

> [!f(x)] QUARTILE.INC Syntax
>
> ```
> =QUARTILE.INC(array, quart)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset. Empty cells, text values, and logical values are ignored. Must contain at least one numeric value. |
| `quart` | Yes | An integer from 0 to 4 specifying which quartile to return: 0=minimum, 1=Q1 (25th percentile), 2=median (50th percentile), 3=Q3 (75th percentile), 4=maximum. |

### Return Value

Returns a numeric value representing the specified quartile using inclusive interpolation. Returns #NUM! if quart is less than 0 or greater than 4, or if the array contains no numeric values. Returns #VALUE! if quart is non-numeric.

## Examples

> [!f(x)] QUARTILE.INC Examples

### Example 1: First Quartile (Q1)
```
=QUARTILE.INC(A1:A8, 1)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 27.5

**Explanation:** With 8 values, Q1 position = (0.25 * 7) + 1 = 2.75, between 20 and 30. Interpolating: 20 + 0.75 * (30 - 20) = 27.5. This is identical to PERCENTILE.INC(data, 0.25).

---

### Example 2: Median (Q2)
```
=QUARTILE.INC(A1:A8, 2)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 45

**Explanation:** Q2 position = (0.5 * 7) + 1 = 4.5, between 40 and 50. Interpolating: 40 + 0.5 * (50 - 40) = 45. This equals MEDIAN(A1:A8).

---

### Example 3: Third Quartile (Q3)
```
=QUARTILE.INC(A1:A8, 3)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 62.5

**Explanation:** Q3 position = (0.75 * 7) + 1 = 6.25, between 60 and 70. Interpolating: 60 + 0.25 * (70 - 60) = 62.5.

---

### Example 4: Minimum (Quartile 0)
```
=QUARTILE.INC(A1:A5, 0)
```
Where A1:A5 contains: 15, 25, 35, 45, 55

**Result:** 15

**Explanation:** With inclusive interpolation, quart=0 always returns the minimum value. This is equivalent to MIN(A1:A5).

---

### Example 5: Maximum (Quartile 4)
```
=QUARTILE.INC(A1:A5, 4)
```
Where A1:A5 contains: 15, 25, 35, 45, 55

**Result:** 55

**Explanation:** With inclusive interpolation, quart=4 always returns the maximum value. This is equivalent to MAX(A1:A5).

---

### Example 6: Comparing INC vs EXC Results
```
=QUARTILE.INC(A1:A8, 1)
=QUARTILE.EXC(A1:A8, 1)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result (INC):** 27.5
**Result (EXC):** 22.5

**Explanation:** INC and EXC use different formulas, producing different results. INC position = 2.75; EXC position = 2.25. The difference is most noticeable with smaller datasets.

---

### Example 7: Interquartile Range Calculation
```
=QUARTILE.INC(A1:A10, 3) - QUARTILE.INC(A1:A10, 1)
```
Where A1:A10 contains: 2, 4, 6, 8, 10, 12, 14, 16, 18, 20

**Result:** 9

**Explanation:** IQR = Q3 - Q1 measures the spread of the middle 50% of data. This robust spread measure is used for outlier detection and box plots.

---

### Example 8: All Quartiles at Once (Excel 365 / Google Sheets)
```
=QUARTILE.INC(A1:A10, {0; 1; 2; 3; 4})
```

**Result:** Vertical array of 5 values (min, Q1, median, Q3, max)

**Explanation:** In Excel 365 or Google Sheets, pass an array of quart values to calculate all quartiles simultaneously. This spills the five-number summary.

---

### Example 9: Outlier Detection Using Quartiles
```
=IF(OR(Value < QUARTILE.INC(Data, 1) - 1.5 * IQR,
       Value > QUARTILE.INC(Data, 3) + 1.5 * IQR), "Outlier", "Normal")
```

**Result:** "Outlier" or "Normal"

**Explanation:** The 1.5*IQR rule identifies potential outliers. Values below Q1 - 1.5*IQR or above Q3 + 1.5*IQR are flagged.

---

### Example 10: Relationship to PERCENTILE.INC
```
=QUARTILE.INC(A1:A10, 1) = PERCENTILE.INC(A1:A10, 0.25)
=QUARTILE.INC(A1:A10, 2) = PERCENTILE.INC(A1:A10, 0.5)
=QUARTILE.INC(A1:A10, 3) = PERCENTILE.INC(A1:A10, 0.75)
```

**Result:** All return TRUE

**Explanation:** QUARTILE.INC is a convenience wrapper. Q1 = 25th percentile, Q2 = 50th, Q3 = 75th. Both use the same inclusive interpolation method.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | quart is less than 0 or greater than 4 | Use only values 0, 1, 2, 3, or 4. |
| `#NUM!` | Array contains no numeric values | Verify at least one number exists. Check for text-formatted numbers. |
| `#VALUE!` | quart is text or non-numeric | Ensure quart is a number or valid cell reference. |
| `Function not found` | Using in Excel 2007 or earlier | Use legacy QUARTILE function, which is functionally identical. |
| `Unexpected result` | Confusing quart scale with percentile scale | Remember: quart uses 0-4, not 0-1 or 0-100. Q1 is quart=1, not 25. |

## Use Cases

### [[Statistical Process Control]]

**Scenario:** A quality engineer monitors manufacturing process measurements, using quartile-based control limits that are robust to non-normal distributions.

**Implementation:**
```
Q1: =QUARTILE.INC(Measurements, 1)
Q3: =QUARTILE.INC(Measurements, 3)
IQR: =QUARTILE.INC(Measurements, 3) - QUARTILE.INC(Measurements, 1)

Lower Control: =Q1 - 1.5 * IQR
Upper Control: =Q3 + 1.5 * IQR

Status: =IF(OR(Current < LowerControl, Current > UpperControl), "Out of Control", "In Control")
```

**Business Application:** Quartile-based control limits work well for non-normal processes where standard deviation-based limits fail. Many manufacturing measurements are skewed, making IQR-based limits more appropriate than mean-based limits.

**Technical Details:** Update control limits regularly as process capability improves. Consider using 2*IQR or 3*IQR for less sensitive limits. Document whether you use INC or EXC method for audit purposes.

---

### [[Compensation Band Development]]

**Scenario:** An HR team creates salary bands using market data quartiles to ensure competitive positioning while controlling costs.

**Implementation:**
```
Band Minimum: =QUARTILE.INC(MarketData, 1)
Band Midpoint: =QUARTILE.INC(MarketData, 2)
Band Maximum: =QUARTILE.INC(MarketData, 3)

Position: =IF(Salary >= Maximum, "Above Band",
             IF(Salary >= Midpoint, "Upper Half",
                IF(Salary >= Minimum, "Lower Half", "Below Band")))
```

**Business Application:** Using Q1-Q3 as salary band boundaries means targeting the middle 50% of market rates. Employees below Q1 may be underpaid relative to market; those above Q3 may be premium-paid.

**Technical Details:** Use QUARTILE.INC because it provides intuitive minimum and maximum. Segment market data by job level, geography, and company size before calculating quartiles. Update annually.

---

### [[Portfolio Risk Analysis]]

**Scenario:** A financial analyst evaluates investment returns using quartile distributions to understand risk profiles and compare fund performance.

**Implementation:**
```
Worst Quarter: =QUARTILE.INC(Returns, 0)
Q1 Return: =QUARTILE.INC(Returns, 1)
Median Return: =QUARTILE.INC(Returns, 2)
Q3 Return: =QUARTILE.INC(Returns, 3)
Best Quarter: =QUARTILE.INC(Returns, 4)

Downside Risk: =QUARTILE.INC(Returns, 1) - QUARTILE.INC(Returns, 0)
Upside Potential: =QUARTILE.INC(Returns, 4) - QUARTILE.INC(Returns, 3)
```

**Business Application:** Quartile analysis shows return distribution without assuming normality. Comparing bottom quartile (downside risk) to top quartile (upside potential) reveals asymmetric return profiles.

**Technical Details:** Use at least 20 return observations for stable quartile estimates. Consider rolling windows (36-month) for time-varying analysis. Compare fund quartiles to benchmark quartiles.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Recommended over:** QUARTILE (legacy function)
- **Method:** Inclusive interpolation using position = (k/4) * (n-1) + 1
- **Valid quart values:** 0, 1, 2, 3, 4

### Google Sheets
- **Availability:** All versions
- **Equivalence:** Functionally identical to Excel
- **Method:** Same inclusive interpolation formula
- **Valid quart values:** 0, 1, 2, 3, 4

### Key Difference Alert
QUARTILE.INC behaves identically in Excel and Google Sheets. The main consideration is choosing between INC and EXC methods:

- Use **QUARTILE.INC** when: Business users expect quartile 0 to mean minimum and quartile 4 to mean maximum, which covers most practical applications.
- Use **QUARTILE.EXC** when: Statistical rigor requires exclusive interpolation, or when interfacing with software using exclusive methods.

## Tips and Best Practices

1. **Use QUARTILE.INC for business applications:** The inclusive method is intuitive: Q0=min, Q4=max. This matches user expectations and simplifies explanations.

2. **Pair with PERCENTILE.INC for consistency:** If using QUARTILE.INC, use PERCENTILE.INC for non-standard percentiles. Mixing INC and EXC methods creates inconsistencies.

3. **Calculate IQR for robust spread:** IQR = Q3 - Q1 measures spread without being affected by outliers. This is preferred over standard deviation for skewed data.

4. **Use for box plot components:** QUARTILE.INC(data, {0,1,2,3,4}) provides all five values needed for box plot visualization in one array formula.

5. **Cache quartile values:** When classifying many values, calculate Q1, Q2, Q3 once in reference cells. This improves performance and ensures consistency.

6. **Remember the scale difference:** QUARTILE uses 0-4 (integer quartile numbers); PERCENTILE uses 0-1 (decimal percentages). Q1 corresponds to the 25th percentile (0.25).

7. **Use for non-parametric analysis:** When data is not normally distributed, quartile-based analysis is more appropriate than mean/standard deviation.

8. **Document your method:** When sharing analysis, note that you used QUARTILE.INC (inclusive method) to ensure reproducibility and avoid confusion.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[QUARTILE]] | Legacy function identical to QUARTILE.INC | Only for Excel 2007 compatibility |
| [[QUARTILE.EXC]] | Quartile with exclusive interpolation (quart 1-3 only) | When statistical standards require exclusive method |
| [[PERCENTILE.INC]] | Returns arbitrary percentile values (0-1) | When you need percentiles other than 0, 25, 50, 75, 100 |
| [[MEDIAN]] | Returns the 50th percentile | More readable for specifically the median |
| [[MIN]] / [[MAX]] | Returns minimum/maximum | Equivalent to quart=0 and quart=4 |

### Commonly Used Together

**[[PERCENTILE.INC]]** - Arbitrary percentiles

*Use matching INC methods for consistency:*
```
5th percentile: =PERCENTILE.INC(Data, 0.05)
Q1: =QUARTILE.INC(Data, 1)  ' Same as PERCENTILE.INC(Data, 0.25)
95th percentile: =PERCENTILE.INC(Data, 0.95)
```
Both functions use the same inclusive interpolation method.

---

**[[PERCENTRANK.INC]]** - Find percentile of a value

*Complete percentile analysis:*
```
Q3 Value: =QUARTILE.INC(Data, 3)
My Percentile: =PERCENTRANK.INC(Data, MyValue)
```
QUARTILE finds the value at a percentile; PERCENTRANK finds the percentile of a value.

---

**[[STDEV.S]]** - Compare spread measures

*Robust vs. parametric spread:*
```
IQR: =QUARTILE.INC(Data, 3) - QUARTILE.INC(Data, 1)
StdDev: =STDEV.S(Data)
Normalized IQR: =IQR / 1.35  ' Approx StdDev for normal data
```
Compare IQR to standard deviation to detect non-normality.

---

**[[AGGREGATE]]** - Filtered quartiles

*Calculate quartiles ignoring hidden rows:*
```
=AGGREGATE(17, 5, Data, 1)  ' Quartile 1 ignoring hidden rows
=AGGREGATE(17, 5, Data, 3)  ' Quartile 3 ignoring hidden rows
```
AGGREGATE function 17 is QUARTILE.INC; option 5 ignores hidden rows.

## Official Documentation

- **Microsoft Excel:** [QUARTILE.INC function](https://support.microsoft.com/en-us/office/quartile-inc-function-1bef2e5c-5c23-48c0-a405-56cfa67d5e35)
- **Google Sheets:** [QUARTILE function](https://support.google.com/docs/answer/3094082) (Google documents all QUARTILE variants together)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | Initial release | Introduced alongside QUARTILE.EXC to replace ambiguous QUARTILE |
| Excel 2013 | Continued support | No functional changes |
| Excel 2016 | Continued support | No functional changes |
| Excel 2019 | Continued support | No functional changes |
| Excel 365 | Continuous updates | Native dynamic array support for multiple quartiles |
| Google Sheets | Full support | Available with identical behavior to Excel |

---

*Last updated: 2026-01-10*
