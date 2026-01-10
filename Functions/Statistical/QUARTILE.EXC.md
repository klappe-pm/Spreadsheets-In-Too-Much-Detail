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
- exclusive-interpolation
- distribution-analysis
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Quartile Exclusive
- QUARTILE Exclusive
- Exclusive Quartile
tags:
- statistical
- quartile
- percentile
- distribution
- data-analysis
- exclusive
---

# QUARTILE.EXC

## Description

**QUARTILE.EXC** returns the quartile of a dataset using exclusive interpolation, where the quart parameter must be 1, 2, or 3 (not 0 or 4). The "EXC" suffix stands for "exclusive," indicating that the function excludes the theoretical 0th and 100th percentiles from valid quartile positions. This method treats the observed data as a sample from a larger population, reserving the extreme positions for values that might exist beyond the observed range.

The exclusive interpolation method calculates quartile positions using the formula: position = (quart/4) * (n+1), where n is the count of values. This differs from QUARTILE.INC which uses (quart/4) * (n-1) + 1. For a dataset with n values, the exclusive method spreads quartile positions differently, typically producing results further from the extremes. QUARTILE.EXC requires at least 4 data points for Q1 and Q3; with fewer values, certain quartiles may return #NUM!.

**Key gotcha:** QUARTILE.EXC does not accept quart=0 (minimum) or quart=4 (maximum). These values return #NUM! because the exclusive method considers the 0th and 100th percentiles undefined for sample data. If you need the minimum or maximum, use MIN/MAX or switch to QUARTILE.INC. Additionally, the exclusive method requires sufficient data points: with only 3 values, Q1 and Q3 are undefined because the exclusive positions would fall outside the data range.

**Platform behavior:** Excel introduced QUARTILE.EXC in Excel 2010 alongside QUARTILE.INC to provide statisticians with the exclusive methodology used in academic contexts. Google Sheets supports QUARTILE.EXC with identical behavior. Both platforms return #NUM! for quart=0, quart=4, or when insufficient data exists for the requested quartile.

## Syntax

> [!f(x)] QUARTILE.EXC Syntax
>
> ```
> =QUARTILE.EXC(array, quart)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset. Empty cells, text values, and logical values are ignored. Must contain sufficient values for the requested quartile. |
| `quart` | Yes | An integer 1, 2, or 3 specifying which quartile to return: 1=Q1 (25th percentile), 2=median (50th percentile), 3=Q3 (75th percentile). Values 0 and 4 return #NUM!. |

### Return Value

Returns a numeric value representing the specified quartile using exclusive interpolation. Returns #NUM! if quart is 0, 4, or outside 1-3. Returns #NUM! if the array has insufficient values for the requested quartile. Returns #VALUE! if quart is non-numeric.

## Examples

> [!f(x)] QUARTILE.EXC Examples

### Example 1: First Quartile (Q1)
```
=QUARTILE.EXC(A1:A8, 1)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 22.5

**Explanation:** With 8 values, Q1 position = (1/4) * 9 = 2.25, between 20 and 30. Interpolating: 20 + 0.25 * (30 - 20) = 22.5. Compare to QUARTILE.INC which returns 27.5.

---

### Example 2: Median (Q2)
```
=QUARTILE.EXC(A1:A8, 2)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 45

**Explanation:** Q2 position = (2/4) * 9 = 4.5, between 40 and 50. Interpolating: 40 + 0.5 * (50 - 40) = 45. For the median, INC and EXC often produce identical results.

---

### Example 3: Third Quartile (Q3)
```
=QUARTILE.EXC(A1:A8, 3)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** 67.5

**Explanation:** Q3 position = (3/4) * 9 = 6.75, between 60 and 70. Interpolating: 60 + 0.75 * (70 - 60) = 67.5. Compare to QUARTILE.INC which returns 62.5.

---

### Example 4: Error for Minimum (Quartile 0)
```
=QUARTILE.EXC(A1:A8, 0)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** #NUM!

**Explanation:** QUARTILE.EXC does not support quart=0. The 0th percentile is undefined in the exclusive method. Use MIN(A1:A8) or QUARTILE.INC(A1:A8, 0) instead.

---

### Example 5: Error for Maximum (Quartile 4)
```
=QUARTILE.EXC(A1:A8, 4)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result:** #NUM!

**Explanation:** QUARTILE.EXC does not support quart=4. The 100th percentile is undefined. Use MAX(A1:A8) or QUARTILE.INC(A1:A8, 4) instead.

---

### Example 6: Comparing INC vs EXC
```
=QUARTILE.INC(A1:A8, 1) vs =QUARTILE.EXC(A1:A8, 1)
```
Where A1:A8 contains: 10, 20, 30, 40, 50, 60, 70, 80

**Result (INC):** 27.5
**Result (EXC):** 22.5

**Explanation:** The exclusive method places Q1 closer to the minimum (22.5 vs 27.5) because it uses a different position formula. The difference is most noticeable with small to medium datasets.

---

### Example 7: Minimum Data for Q1
```
=QUARTILE.EXC(A1:A4, 1)
```
Where A1:A4 contains: 10, 20, 30, 40

**Result:** 15

**Explanation:** With 4 values, Q1 position = (1/4) * 5 = 1.25, between 10 and 20. This is the minimum dataset size where Q1 is computable with QUARTILE.EXC.

---

### Example 8: Insufficient Data Error
```
=QUARTILE.EXC(A1:A3, 1)
```
Where A1:A3 contains: 10, 20, 30

**Result:** #NUM!

**Explanation:** With only 3 values, Q1 position = (1/4) * 4 = 1, which falls exactly on the first value. However, the exclusive method considers this undefined because it would represent the boundary. At least 4 values are needed for Q1/Q3.

---

### Example 9: IQR Using Exclusive Method
```
=QUARTILE.EXC(A1:A10, 3) - QUARTILE.EXC(A1:A10, 1)
```
Where A1:A10 contains: 5, 10, 15, 20, 25, 30, 35, 40, 45, 50

**Result:** Exclusive IQR

**Explanation:** The IQR calculated with exclusive quartiles will differ from the IQR calculated with inclusive quartiles. Be consistent in your choice throughout your analysis.

---

### Example 10: Academic Research Application
```
Reference Range Low: =QUARTILE.EXC(NormalValues, 1)
Reference Range High: =QUARTILE.EXC(NormalValues, 3)
Classification: =IF(AND(Value >= Low, Value <= High), "Normal", "Abnormal")
```

**Result:** Clinical classification based on exclusive quartile reference range

**Explanation:** Medical and academic research often uses exclusive quartiles to align with statistical software conventions. The exclusive method provides more conservative bounds.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | quart equals 0 | Use MIN() for minimum, or switch to QUARTILE.INC(data, 0). |
| `#NUM!` | quart equals 4 | Use MAX() for maximum, or switch to QUARTILE.INC(data, 4). |
| `#NUM!` | quart is less than 1 or greater than 3 | QUARTILE.EXC only accepts 1, 2, or 3. |
| `#NUM!` | Insufficient data for requested quartile | Need at least 4 values for Q1/Q3. Add more data or use QUARTILE.INC. |
| `#VALUE!` | quart is text or non-numeric | Ensure quart is a number or valid cell reference. |
| `Unexpected result` | Confused EXC with INC methodology | Verify which method is appropriate. EXC produces more extreme quartile values. |

## Use Cases

### [[Scientific Research Data Analysis]]

**Scenario:** A researcher analyzing clinical trial data needs quartile values that align with statistical software conventions used in peer-reviewed publications.

**Implementation:**
```
Q1 (Lower Quartile): =QUARTILE.EXC(Measurements, 1)
Median: =QUARTILE.EXC(Measurements, 2)
Q3 (Upper Quartile): =QUARTILE.EXC(Measurements, 3)
IQR: =QUARTILE.EXC(Measurements, 3) - QUARTILE.EXC(Measurements, 1)

Reference Range: Q1 to Q3 (containing central 50% of observations)
```

**Business Application:** Academic journals and regulatory submissions require consistent statistical methods. Using QUARTILE.EXC ensures results match those from SAS, SPSS, or R (with appropriate quantile type), facilitating peer review and replication.

**Technical Details:** The exclusive method is mathematically preferred for probability distribution fitting. Document your quartile method in methodology sections. Ensure sufficient sample size (minimum 4 values for Q1/Q3).

---

### [[Financial Risk Quantile Analysis]]

**Scenario:** A risk analyst calculates portfolio return quartiles for risk reporting, using methodology consistent with industry-standard risk software.

**Implementation:**
```
Downside Quartile: =QUARTILE.EXC(Returns, 1)
Median Return: =QUARTILE.EXC(Returns, 2)
Upside Quartile: =QUARTILE.EXC(Returns, 3)

Risk Report:
"25% of returns were below " & FORMAT(Q1, "0.00%")
"Median return was " & FORMAT(Q2, "0.00%")
"25% of returns exceeded " & FORMAT(Q3, "0.00%")
```

**Business Application:** Financial risk frameworks often specify exclusive quantile methods to align with regulatory and audit expectations. Using QUARTILE.EXC ensures consistency with risk management software.

**Technical Details:** Use at least 20-50 return observations for stable quartile estimates. The exclusive method avoids the theoretical issue of assigning the minimum/maximum return to the 0th/100th percentile.

---

### [[Educational Testing Norming]]

**Scenario:** A testing organization develops standardized test norms using quartile methods that conform to psychometric conventions.

**Implementation:**
```
Lower Quartile Score: =QUARTILE.EXC(NormingSample, 1)
Median Score: =QUARTILE.EXC(NormingSample, 2)
Upper Quartile Score: =QUARTILE.EXC(NormingSample, 3)

Quartile Classification:
=IF(Score >= Q3, "Top Quartile",
   IF(Score >= Q2, "Upper Middle",
      IF(Score >= Q1, "Lower Middle", "Bottom Quartile")))
```

**Business Application:** Educational testing standards often use exclusive quartiles to ensure no student receives exactly the minimum or maximum quartile position based on sample data. This is more statistically conservative.

**Technical Details:** Norming samples should be large (1,000+ examinees). The exclusive method aligns with statistical software used in psychometric analysis. Document the quartile method in technical manuals.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Method:** Exclusive interpolation using position = (quart/4) * (n+1)
- **Valid quart values:** 1, 2, 3 only (0 and 4 return #NUM!)
- **Minimum data:** 4 values for Q1/Q3, 2 values for Q2

### Google Sheets
- **Availability:** All versions
- **Equivalence:** Functionally identical to Excel
- **Method:** Same exclusive interpolation formula
- **Valid quart values:** 1, 2, 3 only

### Key Difference Alert
QUARTILE.EXC behaves identically in Excel and Google Sheets. The critical consideration is choosing between EXC and INC:

- Use **QUARTILE.EXC** when: Interfacing with statistical software, publishing academic research, or when theoretical correctness requires that min/max remain undefined as quartile endpoints.
- Use **QUARTILE.INC** when: Business users expect Q0=min and Q4=max, or when the data range is small and exclusive quartiles become undefined.

## Tips and Best Practices

1. **Understand the restrictions:** QUARTILE.EXC only accepts quart values 1, 2, and 3. For minimum (Q0) and maximum (Q4), use MIN(), MAX(), or QUARTILE.INC.

2. **Check data size:** QUARTILE.EXC requires at least 4 data points for Q1 and Q3. With 3 or fewer values, use QUARTILE.INC or accept that quartile analysis may not be meaningful.

3. **Match your statistical software:** If replicating analysis from R, SAS, or SPSS, QUARTILE.EXC typically produces matching results. R's quantile() with type=6 corresponds to this method.

4. **Pair with PERCENTILE.EXC:** For consistency, use QUARTILE.EXC with PERCENTILE.EXC. Mixing exclusive and inclusive methods creates inconsistencies.

5. **Handle errors gracefully:** Since QUARTILE.EXC returns #NUM! for invalid inputs, use IFERROR: `=IFERROR(QUARTILE.EXC(Data, 1), "Insufficient data")`.

6. **Document your methodology:** When sharing analysis, explicitly state that you used the exclusive quartile method. This prevents confusion when others try to replicate.

7. **Consider the IQR difference:** IQR calculated with QUARTILE.EXC differs from QUARTILE.INC. Use consistent methods when comparing across datasets.

8. **Use for robust statistics:** The exclusive method is preferred for probability-based analysis because it avoids boundary issues at the data extremes.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[QUARTILE.INC]] | Quartile with inclusive interpolation (quart 0-4) | When you need Q0 (min) or Q4 (max), or for business applications |
| [[QUARTILE]] | Legacy function identical to QUARTILE.INC | Only for Excel 2007 compatibility |
| [[PERCENTILE.EXC]] | Returns arbitrary percentile values (exclusive) | When you need percentiles other than 25, 50, 75 |
| [[MEDIAN]] | Returns the 50th percentile | Produces same result as QUARTILE.EXC(data, 2) |
| [[MIN]] / [[MAX]] | Returns minimum/maximum | Use when QUARTILE.EXC cannot provide Q0/Q4 |

### Commonly Used Together

**[[PERCENTILE.EXC]]** - Arbitrary exclusive percentiles

*Use matching EXC methods:*
```
5th percentile: =PERCENTILE.EXC(Data, 0.05)
Q1: =QUARTILE.EXC(Data, 1)  ' Same as PERCENTILE.EXC(Data, 0.25)
Q3: =QUARTILE.EXC(Data, 3)  ' Same as PERCENTILE.EXC(Data, 0.75)
95th percentile: =PERCENTILE.EXC(Data, 0.95)
```
Both functions use the same exclusive interpolation method.

---

**[[MIN]] / [[MAX]]** - Boundary values

*Get boundaries when QUARTILE.EXC cannot:*
```
Minimum: =MIN(Data)  ' QUARTILE.EXC(Data, 0) returns #NUM!
Q1: =QUARTILE.EXC(Data, 1)
Q3: =QUARTILE.EXC(Data, 3)
Maximum: =MAX(Data)  ' QUARTILE.EXC(Data, 4) returns #NUM!
```
Combine MIN/MAX with QUARTILE.EXC for complete five-number summary.

---

**[[IFERROR]]** - Handle insufficient data

*Provide fallback for small datasets:*
```
=IFERROR(QUARTILE.EXC(Data, 1), QUARTILE.INC(Data, 1))
```
Fall back to inclusive method when exclusive method fails.

---

**[[COUNT]]** - Verify data size

*Check if dataset supports exclusive quartiles:*
```
=IF(COUNT(Data) >= 4, QUARTILE.EXC(Data, 1), "Need 4+ values")
```
Validate data size before attempting exclusive quartile calculation.

## Official Documentation

- **Microsoft Excel:** [QUARTILE.EXC function](https://support.microsoft.com/en-us/office/quartile-exc-function-5a355b7a-840b-4a01-b0f1-f538c2864cad)
- **Google Sheets:** [QUARTILE function](https://support.google.com/docs/answer/3094082) (Google documents all QUARTILE variants together)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | Initial release | Introduced alongside QUARTILE.INC for explicit methodology choice |
| Excel 2013 | Continued support | No functional changes |
| Excel 2016 | Continued support | No functional changes |
| Excel 2019 | Continued support | No functional changes |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Full support | Available with identical behavior to Excel |

---

*Last updated: 2026-01-10*
