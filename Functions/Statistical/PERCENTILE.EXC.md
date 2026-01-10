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
- exclusive-interpolation
- distribution-analysis
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Percentile Exclusive
- PERCENTILE Exclusive
- Exclusive Percentile
tags:
- statistical
- percentile
- ranking
- distribution
- data-analysis
- exclusive
---

# PERCENTILE.EXC

## Description

**PERCENTILE.EXC** returns the k-th percentile of values in a range using exclusive interpolation, where k must be strictly greater than 0 and strictly less than 1. The "EXC" suffix stands for "exclusive," meaning the function excludes both endpoints: k=0 and k=1 are not valid inputs. This method aligns with statistical conventions used in many scientific and academic contexts, where the 0th and 100th percentiles are considered undefined for sample data.

The exclusive interpolation method calculates the percentile position using the formula: rank = k * (n+1), where n is the count of values. This formula treats the observed data as a sample from a larger population, reserving theoretical positions for values that might exist beyond the observed minimum and maximum. For a dataset with n values, valid k values range from 1/(n+1) to n/(n+1). Values outside this range return #NUM! because they would require extrapolation beyond observed data.

**Key gotcha:** PERCENTILE.EXC has stricter requirements than PERCENTILE.INC. Not only must k be strictly between 0 and 1, but the dataset must have enough values to support the requested percentile. For example, with only 3 data points, the valid k range is 0.25 to 0.75 (or 1/4 to 3/4). Requesting the 90th percentile (k=0.9) from a 3-element dataset returns #NUM! because there is insufficient data to estimate that extreme percentile using the exclusive method.

**Platform behavior:** Excel introduced PERCENTILE.EXC in Excel 2010 alongside PERCENTILE.INC to provide statistical rigor for users who need exclusive endpoints. Google Sheets fully supports PERCENTILE.EXC with identical behavior. Both platforms return #NUM! when k is at or outside the valid range for the given dataset size. The exclusive method is preferred in academic research, scientific publications, and when interfacing with statistical software that uses exclusive conventions by default (such as certain R functions).

## Syntax

> [!f(x)] PERCENTILE.EXC Syntax
>
> ```
> =PERCENTILE.EXC(array, k)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset. Empty cells, text values, and logical values are ignored. Must contain at least one numeric value. |
| `k` | Yes | The percentile value, which must be strictly greater than 0 and strictly less than 1. Additionally, k must be in the range 1/(n+1) to n/(n+1) where n is the count of numeric values. |

### Return Value

Returns a numeric value representing the k-th percentile using exclusive interpolation. Returns #NUM! if k is less than or equal to 0, greater than or equal to 1, or outside the valid range for the dataset size. Returns #VALUE! if k is non-numeric.

## Examples

> [!f(x)] PERCENTILE.EXC Examples

### Example 1: Median Calculation (50th Percentile)
```
=PERCENTILE.EXC(A1:A9, 0.5)
```
Where A1:A9 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9

**Result:** 5

**Explanation:** With 9 values, position = 0.5 * 10 = 5, which corresponds exactly to the 5th value. For the median, INC and EXC methods often produce identical results.

---

### Example 2: First Quartile (25th Percentile)
```
=PERCENTILE.EXC(A1:A8, 0.25)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 22.5

**Explanation:** Position = 0.25 * 9 = 2.25. This falls between the 2nd value (20) and 3rd value (30). Interpolating: 20 + 0.25 * (30 - 20) = 22.5. Note this differs from PERCENTILE.INC which would return 27.5.

---

### Example 3: Third Quartile (75th Percentile)
```
=PERCENTILE.EXC(A1:A8, 0.75)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 67.5

**Explanation:** Position = 0.75 * 9 = 6.75. This falls between the 6th value (60) and 7th value (70). Interpolating: 60 + 0.75 * (70 - 60) = 67.5. Compare to PERCENTILE.INC which returns 62.5.

---

### Example 4: Error When k=0
```
=PERCENTILE.EXC(A1:A5, 0)
```
Where A1:A5 contains: 15, 25, 35, 45, 55

**Result:** #NUM!

**Explanation:** PERCENTILE.EXC requires k > 0. The 0th percentile is undefined in the exclusive method because it would require extrapolation below the observed minimum.

---

### Example 5: Error When k=1
```
=PERCENTILE.EXC(A1:A5, 1)
```
Where A1:A5 contains: 15, 25, 35, 45, 55

**Result:** #NUM!

**Explanation:** PERCENTILE.EXC requires k < 1. The 100th percentile is undefined because it would require extrapolation above the observed maximum.

---

### Example 6: Minimum Valid k for Small Dataset
```
=PERCENTILE.EXC(A1:A4, 0.2)
```
Where A1:A4 contains: 10, 20, 30, 40

**Result:** 10

**Explanation:** With 4 values, the minimum valid k is 1/(4+1) = 0.2. Position = 0.2 * 5 = 1, returning the first value. Any k less than 0.2 returns #NUM!.

---

### Example 7: Maximum Valid k for Small Dataset
```
=PERCENTILE.EXC(A1:A4, 0.8)
```
Where A1:A4 contains: 10, 20, 30, 40

**Result:** 40

**Explanation:** With 4 values, the maximum valid k is 4/(4+1) = 0.8. Position = 0.8 * 5 = 4, returning the fourth value. Any k greater than 0.8 returns #NUM!.

---

### Example 8: Comparing INC vs EXC on Same Data
```
=PERCENTILE.INC(A1:A10, 0.1)
=PERCENTILE.EXC(A1:A10, 0.1)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result (INC):** 1.9
**Result (EXC):** 1.1

**Explanation:** For the same 10th percentile request, INC and EXC return different values. INC position = (0.1 * 9) + 1 = 1.9. EXC position = 0.1 * 11 = 1.1. The difference is more pronounced at extreme percentiles.

---

### Example 9: Error Due to Insufficient Data
```
=PERCENTILE.EXC(A1:A3, 0.9)
```
Where A1:A3 contains: 10, 20, 30

**Result:** #NUM!

**Explanation:** With only 3 values, valid k ranges from 1/4 = 0.25 to 3/4 = 0.75. The 90th percentile (k=0.9) exceeds the maximum valid k (0.75), so the function returns #NUM!. You need at least 9 values to calculate the 90th percentile with PERCENTILE.EXC.

---

### Example 10: Academic Grading with Exclusive Percentiles
```
=IF(PERCENTILE.EXC(AllScores, 0.9) <= StudentScore, "Top Decile", "Below Top 10%")
```

**Result:** Classification based on exclusive percentile threshold

**Explanation:** Academic contexts often prefer the exclusive method because it aligns with statistical conventions used in research. The exclusive approach is more conservative, reserving extreme percentiles for truly exceptional observations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | k equals 0 | Use k slightly greater than 0 (e.g., 0.01) or switch to PERCENTILE.INC if you need the minimum. |
| `#NUM!` | k equals 1 | Use k slightly less than 1 (e.g., 0.99) or switch to PERCENTILE.INC if you need the maximum. |
| `#NUM!` | k is less than 1/(n+1) | Increase dataset size or use a less extreme percentile. Formula: minimum valid k = 1/(COUNT(array)+1). |
| `#NUM!` | k is greater than n/(n+1) | Increase dataset size or use a less extreme percentile. Formula: maximum valid k = COUNT(array)/(COUNT(array)+1). |
| `#NUM!` | Array contains no numeric values | Verify at least one numeric value exists. Check for text-formatted numbers. |
| `#VALUE!` | k is text or non-numeric | Ensure k is a numeric value or cell reference to a number. |
| `Function not found` | Using PERCENTILE.EXC in Excel 2007 or earlier | No direct equivalent exists; consider PERCENTILE with manual adjustment. |

## Use Cases

### [[Scientific Research Data Analysis]]

**Scenario:** A research team analyzing clinical trial data needs to identify outliers and establish reference ranges using methodology consistent with academic publications and statistical software.

**Implementation:**
```
Lower 2.5%: =PERCENTILE.EXC(MeasurementData, 0.025)
Upper 97.5%: =PERCENTILE.EXC(MeasurementData, 0.975)
Reference Range: Values between lower and upper thresholds

Outlier Detection: =IF(OR(Value < Lower2.5, Value > Upper97.5), "Outlier", "Normal")
```

**Business Application:** Clinical and research contexts require reproducibility. Using PERCENTILE.EXC ensures results match statistical software like SAS, SPSS, or R (when using type 6 or type 7 quantile methods), making peer review and replication straightforward.

**Technical Details:** The exclusive method requires sufficient sample size for extreme percentiles. For 2.5th and 97.5th percentiles (95% reference range), you need at least 39 data points (1/0.025 = 40, so n+1 >= 40, meaning n >= 39). Plan sample sizes accordingly.

---

### [[Risk Modeling and Value at Risk (VaR)]]

**Scenario:** A financial analyst calculates Value at Risk using historical simulation, requiring the 1st or 5th percentile of portfolio returns to estimate potential losses.

**Implementation:**
```
VaR 95%: =ABS(PERCENTILE.EXC(DailyReturns, 0.05))
VaR 99%: =ABS(PERCENTILE.EXC(DailyReturns, 0.01))

Dollar VaR: =PortfolioValue * VaR_Percentage
```

**Business Application:** Regulatory frameworks (Basel III) and risk management best practices often specify exclusive percentile methods. Using PERCENTILE.EXC aligns spreadsheet calculations with regulatory and auditing expectations.

**Technical Details:** VaR calculations require sufficient historical data. For 1% VaR (99th percentile), you need at least 99 data points. Most financial applications use 250+ trading days. Ensure your dataset meets minimum requirements before using extreme percentiles.

---

### [[Educational Assessment Norming]]

**Scenario:** A testing organization develops standardized test norms, requiring percentile ranks that align with psychometric conventions used in educational measurement.

**Implementation:**
```
Stanine Cutoffs:
Stanine 1 (Bottom 4%): Below =PERCENTILE.EXC(NormingSample, 0.04)
Stanine 2 (Next 7%): =PERCENTILE.EXC(NormingSample, 0.04) to =PERCENTILE.EXC(NormingSample, 0.11)
Stanine 5 (Middle 20%): =PERCENTILE.EXC(NormingSample, 0.40) to =PERCENTILE.EXC(NormingSample, 0.60)
Stanine 9 (Top 4%): Above =PERCENTILE.EXC(NormingSample, 0.96)
```

**Business Application:** Psychometric standards typically use exclusive percentiles to avoid the theoretical issues with assigning 0th or 100th percentile ranks to observed scores. This ensures consistency with published norms and professional guidelines.

**Technical Details:** Norming samples should be large (typically 1,000+ examinees) to support extreme percentile calculations. Document the percentile method (INC vs EXC) in technical manuals to ensure proper interpretation.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Method:** Exclusive interpolation using rank = k * (n+1)
- **Valid k range:** 1/(n+1) < k < n/(n+1)
- **Array support:** Native in Excel 365; requires Ctrl+Shift+Enter in earlier versions

### Google Sheets
- **Availability:** All versions
- **Equivalence:** Functionally identical to Excel implementation
- **Method:** Same exclusive interpolation formula
- **Array support:** Native array formula support

### Key Difference Alert
PERCENTILE.EXC behaves identically in Excel and Google Sheets. The primary consideration is understanding when to use EXC versus INC:

- Use **PERCENTILE.EXC** when: Working with statistical software that uses exclusive methods, following academic conventions, or when theoretical correctness requires that 0th and 100th percentiles remain undefined.
- Use **PERCENTILE.INC** when: You need the full 0-1 range, working with small datasets, or when business requirements specify minimum/maximum as valid percentiles.

## Tips and Best Practices

1. **Understand the data size requirements:** PERCENTILE.EXC has minimum data size requirements for extreme percentiles. For the k-th percentile, you need at least CEILING(1/k) - 1 data points. For the 99th percentile, you need at least 99 values.

2. **Calculate valid k range programmatically:** Before using PERCENTILE.EXC, calculate the valid range: `Min k = 1/(COUNT(data)+1)` and `Max k = COUNT(data)/(COUNT(data)+1)`. Use IFERROR to handle cases where the requested percentile is invalid.

3. **Choose EXC for academic and scientific work:** When publishing research or interfacing with statistical software, PERCENTILE.EXC aligns with common conventions. Document your method choice for reproducibility.

4. **Use INC for business applications:** Most business contexts (compensation analysis, SLA monitoring, performance management) work better with PERCENTILE.INC because users expect 0th percentile to mean "minimum" and 100th percentile to mean "maximum."

5. **Handle errors gracefully:** Since PERCENTILE.EXC returns #NUM! for invalid k values, wrap in IFERROR: `=IFERROR(PERCENTILE.EXC(Data, 0.95), "Insufficient data for 95th percentile")`.

6. **Consider the interpolation difference:** For the same k value, EXC and INC return different results. The difference is most pronounced at extreme percentiles (below 0.1 or above 0.9) and with small datasets. Test both methods if unsure which to use.

7. **Document your methodology:** When sharing spreadsheets, add comments or a methodology section explaining why you chose EXC over INC. This prevents confusion when others review your work.

8. **Match your statistical software:** If replicating analysis from R, Python, SAS, or SPSS, verify which percentile method the software uses. R's quantile() function has 9 different type options; PERCENTILE.EXC typically aligns with type 6.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERCENTILE.INC]] | Percentile with inclusive interpolation (k from 0 to 1) | When you need the full range or for business applications |
| [[PERCENTILE]] | Legacy function identical to PERCENTILE.INC | For backward compatibility with older Excel versions |
| [[QUARTILE.EXC]] | Returns quartile values using exclusive method | When you need only standard quartile positions with exclusive method |
| [[QUARTILE.INC]] | Returns quartile values using inclusive method | When you need standard quartiles with inclusive method |
| [[MEDIAN]] | Returns the 50th percentile | When you specifically need the median; produces same result as both INC and EXC |

### Commonly Used Together

**[[PERCENTRANK.EXC]]** - Inverse of PERCENTILE.EXC

*Find where a value falls using exclusive method:*
```
Threshold: =PERCENTILE.EXC(Data, 0.95)
Value Rank: =PERCENTRANK.EXC(Data, SpecificValue)
```
Use matching INC/EXC pairs for consistency.

---

**[[COUNT]]** - Validate data size

*Check if dataset supports the requested percentile:*
```
=IF(0.95 <= COUNT(Data)/(COUNT(Data)+1),
    PERCENTILE.EXC(Data, 0.95),
    "Need more data")
```
Verify data size before calculating extreme percentiles.

---

**[[MIN]] / [[MAX]]** - Boundary alternatives

*Get boundaries when PERCENTILE.EXC cannot:*
```
Lower Bound: =IF(k < 1/(COUNT(Data)+1), MIN(Data), PERCENTILE.EXC(Data, k))
Upper Bound: =IF(k > COUNT(Data)/(COUNT(Data)+1), MAX(Data), PERCENTILE.EXC(Data, k))
```
Fall back to MIN/MAX when the exclusive method returns errors.

---

**[[NORM.INV]]** - Theoretical percentiles

*Compare empirical to theoretical percentiles:*
```
Empirical 95th: =PERCENTILE.EXC(Data, 0.95)
Theoretical 95th: =AVERAGE(Data) + NORM.INV(0.95, 0, 1) * STDEV.S(Data)
```
Compare observed percentiles to what would be expected under normality.

## Official Documentation

- **Microsoft Excel:** [PERCENTILE.EXC function](https://support.microsoft.com/en-us/office/percentile-exc-function-bbaa7204-e9e1-4010-85bf-c31dc5dce4ba)
- **Google Sheets:** [PERCENTILE function](https://support.google.com/docs/answer/3094083) (Google documents all PERCENTILE variants together)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | Initial release | Introduced alongside PERCENTILE.INC for explicit exclusive method |
| Excel 2013 | Continued support | No functional changes |
| Excel 2016 | Continued support | No functional changes |
| Excel 2019 | Continued support | No functional changes |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Full support | Available with identical behavior to Excel |

---

*Last updated: 2026-01-10*
