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
- inclusive-interpolation
- relative-standing
- descriptive-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Percentile Rank Inclusive
- PERCENTRANK Inclusive
- Inclusive Percent Rank
tags:
- statistical
- percentile
- ranking
- distribution
- data-analysis
- inclusive
---

# PERCENTRANK.INC

## Description

**PERCENTRANK.INC** returns the rank of a value in a dataset as a percentage using inclusive interpolation, where results can range from 0 (for the minimum value) to 1 (for the maximum value). The "INC" suffix stands for "inclusive," indicating that the function includes both endpoints of the distribution. This is the inverse of PERCENTILE.INC: while PERCENTILE.INC answers "what value is at the 75th percentile?", PERCENTRANK.INC answers "what percentile does this specific value represent?"

The inclusive interpolation method calculates the percentile rank using the formula: rank = (position - 1) / (n - 1), where position is the 1-based rank of the value in the sorted dataset and n is the count of values. When the specified value falls between data points, PERCENTRANK.INC performs linear interpolation to estimate the percentile rank. The minimum value always returns 0 (0th percentile), and the maximum value always returns 1 (100th percentile).

**Key gotcha:** PERCENTRANK.INC is functionally identical to the legacy PERCENTRANK function but explicitly documents the inclusive methodology. The key difference from PERCENTRANK.EXC is in the range of possible outputs: INC returns values from 0 to 1, while EXC returns values strictly between 0 and 1 (never exactly 0 or 1). For small datasets, this distinction is significant. Additionally, PERCENTRANK.INC has a significance parameter that truncates decimal places (default 3), which can cause unexpected precision loss in calculations.

**Platform behavior:** Excel introduced PERCENTRANK.INC in Excel 2010 to replace the ambiguous legacy PERCENTRANK function. Google Sheets fully supports PERCENTRANK.INC with identical behavior. Both platforms return #N/A if the value is outside the data range (below minimum or above maximum). The optional significance parameter defaults to 3 decimal places; increase it for more precision in calculations. Values matching exactly with data points return exact positions; interpolated values use linear estimation.

## Syntax

> [!f(x)] PERCENTRANK.INC Syntax
>
> ```
> =PERCENTRANK.INC(array, x, [significance])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array of numeric values that defines the dataset for ranking. Empty cells, text values, and logical values are ignored. Must contain at least one numeric value. |
| `x` | Yes | The value for which you want to find the percentile rank. Must be within the range of values in the array (between MIN and MAX inclusive). |
| `significance` | No | The number of significant digits for the returned percentage. Defaults to 3. Use higher values (6-10) when results feed into further calculations. |

### Return Value

Returns a decimal value between 0 and 1 inclusive, representing the percentile rank of x in the dataset, truncated to the specified significance. Returns #N/A if x is outside the data range. Returns #VALUE! if arguments are non-numeric. Returns #NUM! if significance is less than 1.

## Examples

> [!f(x)] PERCENTRANK.INC Examples

### Example 1: Basic Percentile Rank
```
=PERCENTRANK.INC(A1:A10, 5)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result:** 0.444

**Explanation:** The value 5 is in position 5 of 10. Using the inclusive formula: (5-1)/(10-1) = 4/9 = 0.444 (truncated to 3 decimal places). Approximately 44.4% of values are at or below 5.

---

### Example 2: Minimum Value Returns 0
```
=PERCENTRANK.INC(A1:A5, 100)
```
Where A1:A5 contains: 100, 200, 300, 400, 500

**Result:** 0

**Explanation:** The minimum value in a dataset always returns exactly 0 with the inclusive method. This represents the 0th percentile, meaning no values are below it.

---

### Example 3: Maximum Value Returns 1
```
=PERCENTRANK.INC(A1:A5, 500)
```
Where A1:A5 contains: 100, 200, 300, 400, 500

**Result:** 1

**Explanation:** The maximum value returns exactly 1 with the inclusive method. This represents the 100th percentile, meaning all values are at or below it.

---

### Example 4: Interpolated Value Between Data Points
```
=PERCENTRANK.INC(A1:A5, 250)
```
Where A1:A5 contains: 100, 200, 300, 400, 500

**Result:** 0.375

**Explanation:** 250 falls between 200 (rank 0.25) and 300 (rank 0.5). Linear interpolation: 0.25 + (250-200)/(300-200) * (0.5-0.25) = 0.25 + 0.5 * 0.25 = 0.375.

---

### Example 5: Higher Significance for Precision
```
=PERCENTRANK.INC(A1:A10, 5, 6)
```
Where A1:A10 contains: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result:** 0.444444

**Explanation:** With significance set to 6, the result shows more decimal places. The true value is 4/9 = 0.444444... This precision is important when the result feeds into other calculations.

---

### Example 6: Comparing INC vs EXC Results
```
=PERCENTRANK.INC(A1:A5, 100)
=PERCENTRANK.EXC(A1:A5, 100)
```
Where A1:A5 contains: 100, 200, 300, 400, 500

**Result (INC):** 0
**Result (EXC):** 0.166...

**Explanation:** For the minimum value, INC returns exactly 0, while EXC returns 1/(n+1) = 1/6 = 0.166... The exclusive method never returns 0 or 1, reserving those positions for theoretical values below/above the observed range.

---

### Example 7: Value Not in Dataset
```
=PERCENTRANK.INC(A1:A4, 25)
```
Where A1:A4 contains: 10, 20, 30, 40

**Result:** 0.416

**Explanation:** 25 is not in the dataset but falls between 20 and 30. The function interpolates to find its percentile rank within the distribution.

---

### Example 8: Handling Duplicate Values
```
=PERCENTRANK.INC(A1:A5, 30)
```
Where A1:A5 contains: 10, 30, 30, 30, 50

**Result:** 0.25

**Explanation:** When duplicates exist, PERCENTRANK.INC uses the position of the first occurrence. The first 30 is in position 2, so rank = (2-1)/(5-1) = 0.25.

---

### Example 9: Out-of-Range Value Error
```
=PERCENTRANK.INC(A1:A5, 1000)
```
Where A1:A5 contains: 100, 200, 300, 400, 500

**Result:** #N/A

**Explanation:** The value 1000 exceeds the maximum (500). PERCENTRANK.INC cannot extrapolate beyond observed data. Use IFERROR to handle this case.

---

### Example 10: Practical Application with Formatting
```
=TEXT(PERCENTRANK.INC(AllSalaries, MySalary, 1) * 100, "0") & "th percentile"
```

**Result:** "75th percentile" (example)

**Explanation:** Convert the decimal rank to a percentage for display. A salary at the 75th percentile is higher than 75% of all salaries in the comparison dataset.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | x is less than the minimum value in array | Use IFERROR to handle out-of-range values: `=IFERROR(PERCENTRANK.INC(Data, x), 0)` |
| `#N/A` | x is greater than the maximum value in array | Use IFERROR to handle out-of-range values: `=IFERROR(PERCENTRANK.INC(Data, x), 1)` |
| `#VALUE!` | x or array contains non-numeric text | Ensure all arguments are numeric values or valid cell references. |
| `#NUM!` | significance is less than 1 | Use significance of 1 or greater. Omit for default of 3. |
| `Precision loss` | Default significance of 3 truncates results | Increase significance to 6-10 when results are used in further calculations. |
| `Function not found` | Using in Excel 2007 or earlier | Use legacy PERCENTRANK function, which is functionally identical. |

## Use Cases

### [[Compensation Benchmarking and Pay Equity]]

**Scenario:** An HR analyst needs to compare each employee's salary to the market, showing where they fall in the distribution of comparable roles.

**Implementation:**
```
Market Position: =PERCENTRANK.INC(MarketSalaries, EmployeeSalary, 2)

Display: =TEXT(PERCENTRANK.INC(MarketSalaries, EmployeeSalary), "0%")

Compa-Ratio Category:
=IF(PERCENTRANK.INC(MarketSalaries, EmployeeSalary) >= 0.75, "Above Market",
   IF(PERCENTRANK.INC(MarketSalaries, EmployeeSalary) >= 0.25, "At Market", "Below Market"))
```

**Business Application:** Percentile ranks provide objective, defensible positioning for compensation decisions. An employee at the 60th percentile earns more than 60% of comparable workers in the market. This is more intuitive than compa-ratios for communication with employees and executives.

**Technical Details:** Use market data from reputable salary surveys. Segment by job family, level, and geography for meaningful comparisons. Update annually. Consider using PERCENTRANK.INC rather than the legacy PERCENTRANK for modern workbooks.

---

### [[Investment Portfolio Analysis]]

**Scenario:** A financial advisor compares client portfolio returns to benchmark data, showing clients where their performance ranks among similar portfolios.

**Implementation:**
```
Performance Percentile: =PERCENTRANK.INC(BenchmarkReturns, PortfolioReturn, 4)

Report Text: =IF(PERCENTRANK.INC(BenchmarkReturns, PortfolioReturn) >= 0.5,
    "Your portfolio outperformed " & TEXT(PERCENTRANK.INC(BenchmarkReturns, PortfolioReturn), "0%") & " of comparable portfolios",
    "Your portfolio is in the " & TEXT(PERCENTRANK.INC(BenchmarkReturns, PortfolioReturn), "0%") & " of comparable portfolios")
```

**Business Application:** Clients understand percentile rankings intuitively. "Your portfolio is in the 75th percentile" is clearer than "you outperformed by 2.3 standard deviations." This framing helps set expectations and contextualize performance.

**Technical Details:** Match benchmark data to portfolio characteristics (risk level, asset allocation, time period). Use sufficient historical data for stable rankings. Higher significance (4-6) provides smoother ranking transitions quarter to quarter.

---

### [[Customer Segmentation by Purchase Behavior]]

**Scenario:** A marketing team segments customers by spending level, using percentile ranks to identify high-value customers for targeted campaigns.

**Implementation:**
```
Spending Percentile: =PERCENTRANK.INC(AllCustomerSpending, ThisCustomerSpending)

Segment: =IF(PERCENTRANK.INC(AllCustomerSpending, ThisCustomerSpending) >= 0.9, "VIP",
            IF(PERCENTRANK.INC(AllCustomerSpending, ThisCustomerSpending) >= 0.75, "High Value",
               IF(PERCENTRANK.INC(AllCustomerSpending, ThisCustomerSpending) >= 0.5, "Standard", "Casual")))

Score (0-100): =ROUND(PERCENTRANK.INC(AllCustomerSpending, ThisCustomerSpending) * 100, 0)
```

**Business Application:** Percentile-based segmentation automatically adjusts to changes in the customer base. As average spending increases, the thresholds for "High Value" adjust accordingly, maintaining consistent segment sizes. This is more robust than fixed dollar thresholds.

**Technical Details:** Recalculate percentile ranks regularly (monthly/quarterly) to reflect current spending patterns. Consider multiple factors (frequency, recency, monetary value) combined into a composite score before ranking.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Recommended over:** PERCENTRANK (legacy function)
- **Default significance:** 3 decimal places
- **Method:** Inclusive, returning 0 for minimum and 1 for maximum

### Google Sheets
- **Availability:** All versions
- **Equivalence:** Functionally identical to Excel
- **Default significance:** 3 decimal places
- **Method:** Same inclusive methodology

### Key Difference Alert
PERCENTRANK.INC behaves identically in Excel and Google Sheets. The primary consideration is choosing between INC and EXC methods:

- Use **PERCENTRANK.INC** when: You want the minimum to return 0 and maximum to return 1, which is intuitive for business applications and aligns with common user expectations.
- Use **PERCENTRANK.EXC** when: Statistical rigor requires that 0th and 100th percentiles remain undefined, or when interfacing with statistical software using exclusive methods.

## Tips and Best Practices

1. **Always use INC or EXC explicitly:** Avoid the legacy PERCENTRANK function. Using PERCENTRANK.INC makes your methodology clear to anyone reviewing your spreadsheet.

2. **Match PERCENTILE and PERCENTRANK methods:** If using PERCENTILE.INC to find thresholds, use PERCENTRANK.INC to find ranks. Mixing INC and EXC methods can produce inconsistent results.

3. **Increase significance for calculations:** Default significance of 3 decimal places (0.123) may not be sufficient. When percentile ranks feed into formulas, use significance of 6 or higher.

4. **Handle out-of-range values explicitly:** Unlike PERCENTILE functions, PERCENTRANK functions return #N/A for values outside the data range. Use: `=IFERROR(PERCENTRANK.INC(Data, x, 6), IF(x > MAX(Data), 1, 0))`.

5. **Understand duplicate handling:** All duplicate values receive the same percentile rank (the position of the first occurrence). This may not match expectations for tied rankings.

6. **Use with large datasets:** Percentile ranks are most meaningful with larger datasets (50+ values). With small datasets, the rank increments are coarse and may not provide useful differentiation.

7. **Document your norm group:** Always clarify what dataset is used for ranking. "90th percentile of your team" is very different from "90th percentile globally."

8. **Consider RANK functions for ordinal needs:** If you need ordinal rankings (1st, 2nd, 3rd) rather than percentile ranks, use RANK.EQ or RANK.AVG instead.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERCENTRANK]] | Legacy function identical to PERCENTRANK.INC | Only for Excel 2007 compatibility |
| [[PERCENTRANK.EXC]] | Percentile rank with exclusive interpolation (never returns 0 or 1) | When statistical standards require exclusive endpoints |
| [[RANK.EQ]] | Returns ordinal rank (1, 2, 3...) | When you need position numbers rather than percentiles |
| [[RANK.AVG]] | Returns ordinal rank, averaging ties | When ordinal rank with averaged ties is needed |
| [[PERCENTILE.INC]] | Returns value at a given percentile | The inverse: finding value at a percentile vs. percentile of a value |

### Commonly Used Together

**[[PERCENTILE.INC]]** - Inverse of PERCENTRANK.INC

*Verify the relationship between these functions:*
```
Rank: =PERCENTRANK.INC(Data, 75)  ' Returns 0.6
Verify: =PERCENTILE.INC(Data, 0.6)  ' Should return approximately 75
```
These functions are mathematical inverses when using matching INC/EXC methods.

---

**[[IFERROR]]** - Handle out-of-range values

*Assign meaningful ranks to values outside the dataset:*
```
=IFERROR(PERCENTRANK.INC(Data, Value, 6),
         IF(Value > MAX(Data), 1, 0))
```
Convert #N/A errors to 0 (below minimum) or 1 (above maximum).

---

**[[TEXT]]** - Format for display

*Create user-friendly percentile labels:*
```
=TEXT(PERCENTRANK.INC(Data, Value) * 100, "0") & "th percentile"
' Returns "75th percentile"
```
Format decimal ranks as readable percentages.

---

**[[IFS]]** - Create tier classifications

*Segment values by percentile standing:*
```
=IFS(PERCENTRANK.INC(Data, Value) >= 0.95, "Top 5%",
     PERCENTRANK.INC(Data, Value) >= 0.75, "Top Quartile",
     PERCENTRANK.INC(Data, Value) >= 0.25, "Middle 50%",
     TRUE, "Bottom Quartile")
```
Use percentile ranks for dynamic segmentation.

## Official Documentation

- **Microsoft Excel:** [PERCENTRANK.INC function](https://support.microsoft.com/en-us/office/percentrank-inc-function-149592c9-00c0-49ba-86c1-c1f45b80463a)
- **Google Sheets:** [PERCENTRANK function](https://support.google.com/docs/answer/3094087) (Google documents all PERCENTRANK variants together)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | Initial release | Introduced alongside PERCENTRANK.EXC to replace ambiguous PERCENTRANK |
| Excel 2013 | Continued support | No functional changes |
| Excel 2016 | Continued support | No functional changes |
| Excel 2019 | Continued support | No functional changes |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Full support | Available with identical behavior to Excel |

---

*Last updated: 2026-01-10*
