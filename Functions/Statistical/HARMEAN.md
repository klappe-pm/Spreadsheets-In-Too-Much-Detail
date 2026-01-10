---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- harmonic-mean
- average
- rates
- central-tendency
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Harmonic Mean
- Harmonic Average
- Reciprocal Mean
tags:
- harmonic-mean
- average
- rates
- speed
- central-tendency
- statistics
---

# HARMEAN

## Description

**HARMEAN** calculates the harmonic mean of a set of positive numbers. The harmonic mean is the reciprocal of the arithmetic mean of the reciprocals: n / (1/x1 + 1/x2 + ... + 1/xn). For two numbers, it equals 2ab/(a+b). The harmonic mean is always less than or equal to the geometric mean, which is always less than or equal to the arithmetic mean.

Use HARMEAN when averaging **rates, ratios, or speeds**—situations where the denominator matters. If you drive 60 mph for one leg and 40 mph for the same distance on return, your average speed is the harmonic mean (48 mph), not the arithmetic mean (50 mph). Similarly, averaging P/E ratios, fuel efficiency (mpg), or any rate expressed as "per unit" benefits from harmonic averaging.

The harmonic mean gives less weight to extreme high values than the arithmetic mean does. This property makes it useful in certain financial calculations, particularly when dealing with ratios where outliers could skew results. When averaging price-to-earnings ratios across stocks of different sizes, the harmonic mean prevents high-P/E stocks from dominating the average.

**Critical limitation: HARMEAN requires strictly positive values.** Any zero in your data causes a division-by-zero error (#NUM!), and negative values produce mathematically undefined results. If your data might contain zeros, you cannot use HARMEAN directly—consider filtering zeros or using a different averaging method.

## Syntax

> [!f(x)] HARMEAN Syntax
>
> ```
> =HARMEAN(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first positive number, cell reference, or range for which to calculate the harmonic mean. Must be greater than zero. |
| `number2, ...` | No | Additional positive numbers, cell references, or ranges. Up to 255 arguments total in Excel, variable in Sheets. |

### Return Value

Returns a single numeric value representing the harmonic mean of all provided positive numbers. The result is always positive and always less than or equal to the arithmetic mean of the same data. Returns #NUM! if any value is zero or negative. Returns #VALUE! if any argument cannot be converted to a number.

## Examples

> [!f(x)] HARMEAN Examples

### Example 1: Basic Two-Number Harmonic Mean
```
=HARMEAN(40, 60)
```
**Result:** 48

**Explanation:** The harmonic mean of 40 and 60 is 2*40*60/(40+60) = 4800/100 = 48. This is less than the arithmetic mean of 50.

---

### Example 2: Average Speed Calculation
```
=HARMEAN(60, 30)
```
**Result:** 40

**Explanation:** If you travel equal distances at 60 mph and 30 mph, your average speed is 40 mph (not 45). The slower speed affects total time more significantly.

---

### Example 3: Range of Values
```
=HARMEAN(A1:A10)
```
**Result:** Harmonic mean of all values in range

**Explanation:** Works with cell ranges. All values must be positive—any zero or negative value causes #NUM! error.

---

### Example 4: Multiple Arguments
```
=HARMEAN(10, 20, 30, 40, 50)
```
**Result:** 21.74 (approximately)

**Explanation:** The harmonic mean of 5 values: 5 / (1/10 + 1/20 + 1/30 + 1/40 + 1/50) = 5 / 0.2283... = 21.74

---

### Example 5: Equal Values
```
=HARMEAN(25, 25, 25, 25)
```
**Result:** 25

**Explanation:** When all values are identical, harmonic, geometric, and arithmetic means all equal that value. This is a useful sanity check.

---

### Example 6: Investment Rate Averaging
```
=HARMEAN({0.05, 0.08, 0.06, 0.07})
```
**Result:** 0.0635 (approximately 6.35%)

**Explanation:** Harmonic mean of rates of return. Useful when analyzing average rates over equal investment amounts.

---

### Example 7: Price-to-Earnings Ratios
```
=HARMEAN(15, 20, 25, 18, 22)
```
**Result:** 19.35 (approximately)

**Explanation:** Averaging P/E ratios with harmonic mean prevents high-P/E outliers from skewing the result upward, giving a more representative "typical" P/E.

---

### Example 8: Fuel Efficiency Average
```
=HARMEAN(35, 28, 32, 30)
```
**Result:** 31.05 (approximately)

**Explanation:** When averaging mpg figures for equal distance trips, harmonic mean gives the correct combined efficiency.

---

### Example 9: Mixed Cell References and Numbers
```
=HARMEAN(A1:A5, 100, B1:B3)
```
**Result:** Harmonic mean of all specified values

**Explanation:** HARMEAN accepts mixed arguments—ranges, individual cells, and direct numbers can all be combined.

---

### Example 10: F-Number Averaging in Photography
```
=HARMEAN(2.8, 4, 5.6)
```
**Result:** 3.82 (approximately)

**Explanation:** Camera f-stops follow a geometric progression. Harmonic mean helps average exposure values correctly.

---

### Example 11: Comparing with Arithmetic Mean
```
Arithmetic: =AVERAGE(2, 8) → 5
Harmonic:   =HARMEAN(2, 8) → 3.2
```
**Result:** Harmonic mean is significantly lower

**Explanation:** The more spread out the values, the greater the difference between arithmetic and harmonic means. This demonstrates harmonic mean's sensitivity to small values.

---

### Example 12: Parallel Resistance Calculation
```
=1/HARMEAN(1/100, 1/200, 1/300)
```
**Result:** 54.55 (approximately)

**Explanation:** Parallel resistance formula relates to harmonic mean. For resistors R1, R2, R3 in parallel: 1/Rtotal = 1/R1 + 1/R2 + 1/R3.

---

### Example 13: Time-Weighted Performance
```
=HARMEAN(1.10, 1.15, 0.95, 1.08)
```
**Result:** 1.068 (approximately)

**Explanation:** Growth factors (1 + rate) averaged harmonically for certain time-weighted calculations.

---

### Example 14: Large Dataset
```
=HARMEAN(DataRange)
```
**Result:** Harmonic mean of entire dataset

**Explanation:** For large datasets, ensure no zeros exist. Use =COUNTIF(DataRange, 0) to check before applying HARMEAN.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Zero value in data | Remove zeros or use alternative method. HARMEAN cannot handle zeros due to division by zero. |
| `#NUM!` | Negative value in data | Harmonic mean is undefined for negative numbers. Filter to positive values only. |
| `#VALUE!` | Text in data | Ensure all values are numeric. Text cells cause conversion errors. |
| `#DIV/0!` | All values are blank | HARMEAN needs at least one positive number. Check your range for data. |
| `#NAME?` | Misspelled function | Check spelling: HARMEAN, not HARMONICMEAN or HARMMEAN. |
| `Unexpected result` | Used when arithmetic mean appropriate | Harmonic mean is lower than arithmetic mean. Verify harmonic mean is the correct choice for your application. |

## Use Cases

### [[Speed and Rate Averaging]]

**Scenario:** Calculate true average speed when traveling equal distances at different speeds.

**Implementation:**
```
=HARMEAN(Speed1, Speed2, Speed3)
=HARMEAN(OutboundSpeed, ReturnSpeed)           → Round trip average
=HARMEAN(Leg1_mph, Leg2_mph, Leg3_mph)         → Multi-leg trip average
```

**Business Application:** Logistics, transportation planning, delivery route optimization. When distances are equal but speeds vary, arithmetic mean overstates true average speed.

**Technical Details:** For equal time at different speeds, use arithmetic mean. For equal distance at different speeds, use harmonic mean. The choice depends on what's held constant.

---

### [[Financial Ratio Analysis]]

**Scenario:** Average price-to-earnings or other financial ratios across a portfolio.

**Implementation:**
```
=HARMEAN(PE_Ratios)
=HARMEAN(PriceToBook_Range)
=HARMEAN(EV_EBITDA_Multiples)
```

**Business Application:** Portfolio analysis, index construction, comparable company analysis. Harmonic mean prevents high-ratio outliers from dominating the average.

**Technical Details:** The S&P 500's P/E is often calculated as harmonic mean to avoid distortion from a few high-P/E stocks. This gives a value closer to the "typical" stock's ratio.

---

### [[Fuel Efficiency Calculations]]

**Scenario:** Calculate combined fuel efficiency for trips of equal distance at different efficiencies.

**Implementation:**
```
=HARMEAN(MPG_City, MPG_Highway)                → Combined rating estimate
=HARMEAN(Vehicle1_MPG, Vehicle2_MPG, Vehicle3_MPG)
=HARMEAN(Trip1_Efficiency, Trip2_Efficiency)
```

**Business Application:** Fleet management, vehicle comparison, EPA fuel economy ratings. Government fuel economy ratings use weighted harmonic means.

**Technical Details:** The EPA's combined mpg rating uses a weighted harmonic mean (55% city, 45% highway weighting) rather than simple arithmetic average.

---

### [[Machine Learning: F1 Score]]

**Scenario:** Calculate F1 score as harmonic mean of precision and recall.

**Implementation:**
```
=HARMEAN(Precision, Recall)                     → F1 Score (without the 2x factor)
=2*Precision*Recall/(Precision+Recall)          → Standard F1 formula
=2/(1/Precision + 1/Recall)                     → Equivalent using harmonic mean concept
```

**Business Application:** Model evaluation in classification problems. F1 score balances precision and recall, penalizing extreme imbalances.

**Technical Details:** True F1 score is 2 * harmonic mean of precision and recall. HARMEAN gives half of F1; multiply by 2 for the standard metric.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 2000
- **Argument limit:** Up to 255 arguments
- **Array handling:** Accepts arrays and ranges; ignores text and logical values in ranges
- **Performance:** Highly optimized for large datasets

### Google Sheets
- **Availability:** All versions
- **Argument limit:** Variable, generally supports extensive arguments
- **Array handling:** Works with arrays via ARRAYFORMULA for element-wise operations
- **Performance:** Efficient for typical use cases

### Both Platforms
- Identical mathematical calculation
- Same error behavior for zeros and negatives
- Text in ranges is ignored (not counted, doesn't cause error)
- Logical TRUE/FALSE in ranges are ignored
- Empty cells are ignored

## Tips and Best Practices

1. **Verify positive data:** Before using HARMEAN, check with `=COUNTIF(range, "<=0")`. Any result greater than 0 means HARMEAN will fail.

2. **Understand when to use it:** Harmonic mean is correct for averaging rates where the denominator varies. "Miles per gallon" averages correctly with harmonic mean when distances are equal.

3. **Compare with other means:** For important analyses, calculate arithmetic, geometric, and harmonic means. They should satisfy: Harmonic <= Geometric <= Arithmetic (equality only when all values are identical).

4. **Handle zeros with workarounds:** If zeros represent missing data, filter them out. If zeros are real values, consider adding a small constant or using a different metric.

5. **Use for resistance/capacitance:** Parallel electrical components combine using harmonic relationships. Total parallel resistance = HARMEAN * n / 1.

6. **Remember the relationship:** For two numbers a and b: Harmonic mean = (Geometric mean)^2 / (Arithmetic mean). This helps verify calculations.

7. **Consider weighted harmonic mean:** For unequal weights, use: =1/SUMPRODUCT(weights, 1/values) * SUM(weights). This extends HARMEAN's concept.

8. **Don't confuse with geometric mean:** Use GEOMEAN for multiplicative processes (growth rates). Use HARMEAN for rate averaging (speed, efficiency).

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AVERAGE]] | Arithmetic mean | When averaging absolute values, not rates |
| [[GEOMEAN]] | Geometric mean | When averaging growth rates or multiplicative factors |
| [[MEDIAN]] | Middle value | When outliers could distort any mean |
| [[TRIMMEAN]] | Mean excluding outliers | When you want to reduce outlier impact |

### Commonly Used Together

**[[AVERAGE]]** - Compare means

*Verify relationship between means:*
```
Arithmetic: =AVERAGE(range)
Harmonic:   =HARMEAN(range)
```
Harmonic should always be less than or equal to arithmetic mean.

---

**[[GEOMEAN]]** - Complete mean comparison

*All three Pythagorean means:*
```
Arithmetic: =AVERAGE(range)
Geometric:  =GEOMEAN(range)
Harmonic:   =HARMEAN(range)
```
Should satisfy: Harmonic <= Geometric <= Arithmetic.

---

**[[COUNTIF]]** - Validate data before HARMEAN

*Check for zeros:*
```
=COUNTIF(range, 0)
=IF(COUNTIF(range, "<=0")=0, HARMEAN(range), "Data contains non-positive values")
```
Prevents #NUM! errors by checking data validity.

---

**[[FILTER]]** - Remove zeros before calculation

*Clean data:*
```
=HARMEAN(FILTER(range, range>0))
```
Filters out zeros and negatives before harmonic mean calculation.

---

**[[SUMPRODUCT]]** - Weighted harmonic mean

*Custom weighted calculation:*
```
=SUM(weights)/SUMPRODUCT(weights, 1/values)
```
Extends HARMEAN to handle unequal weights.

## Official Documentation

- **Microsoft Excel:** [HARMEAN function](https://support.microsoft.com/en-us/office/harmean-function-5efd9184-fab5-42f9-b1d3-57883a1d3bc6)
- **Google Sheets:** [HARMEAN function](https://support.google.com/docs/answer/3094010)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original implementation |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
