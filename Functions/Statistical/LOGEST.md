---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- exponential-regression
- curve-fitting
- regression-statistics
- parameter-estimation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Logarithmic Estimation
- Exponential Regression Statistics
- Exponential Curve Parameters
tags:
- exponential
- regression
- statistics
- parameters
- curve-fitting
- logest
---

# LOGEST

## Description

**LOGEST** returns the parameters of an exponential regression curve that best fits your data, along with optional regression statistics. While GROWTH predicts y-values from the fitted curve, LOGEST gives you the actual equation parameters: the base (m) and coefficient (b) in the equation y = b * m^x. With multiple x variables, it extends to y = b * m1^x1 * m2^x2 * ... * mn^xn.

Use LOGEST when you need to **understand or report the exponential relationship itself**, not just make predictions. If you need to know the growth rate, doubling time, or decay constant, LOGEST provides the parameters. It's essential for scientific reporting where you need to state the fitted equation, and for quality assessment of the exponential fit via optional statistics.

LOGEST returns an array: in its simplest form, a single row containing m and b. With the statistics option enabled, it returns a 5-row array containing the coefficients, standard errors, R-squared, F-statistic, and sum of squared errors. This rich output makes LOGEST the analytical counterpart to GROWTH's predictive capabilities.

**Critical requirement: like GROWTH, LOGEST requires positive y-values.** Exponential curves model positive quantities only. Zero or negative y-values cause #NUM! errors. Also note that LOGEST works by taking logarithms internally—it's actually performing linear regression on log-transformed data, then converting back. This means it minimizes relative errors, not absolute errors.

## Syntax

> [!f(x)] LOGEST Syntax
>
> ```
> =LOGEST(known_y's, [known_x's], [const], [stats])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `known_y's` | Yes | The set of positive y-values from your data. Must all be greater than zero. |
| `known_x's` | No | The x-values corresponding to known_y's. If omitted, assumed to be {1, 2, 3, ...}. Can be multiple columns for multiple regression. |
| `const` | No | TRUE (default): calculate b normally. FALSE: force b = 1 (curve passes through y=1 at x=0). |
| `stats` | No | FALSE (default): return just the coefficients. TRUE: return additional regression statistics (5-row array). |

### Return Value

Returns an array. Basic output (stats=FALSE): one row with [m, b] for single variable, or [mn, mn-1, ..., m1, b] for multiple variables. Full statistics (stats=TRUE): 5-row array containing coefficients, standard errors, R-squared and standard error of y, F-statistic and degrees of freedom, regression sum of squares and residual sum of squares.

## Examples

> [!f(x)] LOGEST Examples

### Example 1: Basic Exponential Parameters
```
=LOGEST({2,4,8,16}, {1,2,3,4})
```
**Result:** {2, 1}

**Explanation:** Returns m=2 (growth factor) and b=1 (intercept). The equation is y = 1 * 2^x, meaning values double for each unit increase in x.

---

### Example 2: Extract Growth Rate Only
```
=INDEX(LOGEST(B2:B10, A2:A10), 1, 1)
```
**Result:** The m value (growth factor)

**Explanation:** INDEX extracts just the first element—the growth factor. Subtract 1 to get percentage growth: (m-1)*100%.

---

### Example 3: Extract Intercept Only
```
=INDEX(LOGEST(B2:B10, A2:A10), 1, 2)
```
**Result:** The b value (y-intercept coefficient)

**Explanation:** The second value in LOGEST output is the coefficient b in y = b * m^x.

---

### Example 4: Full Statistics Output
```
=LOGEST(B2:B20, A2:A20, TRUE, TRUE)
```
**Result:** 5x2 array of statistics

**Explanation:** Returns complete regression analysis: coefficients, standard errors, R-squared, F-statistic, and sum of squares.

---

### Example 5: Interpreting Full Statistics
```
Row 1: m, b                    → Coefficients
Row 2: se_m, se_b              → Standard errors of coefficients
Row 3: R², se_y                → R-squared, standard error of y estimate
Row 4: F, df                   → F-statistic, degrees of freedom
Row 5: SSreg, SSresid          → Regression SS, Residual SS
```
**Result:** Understanding the 5-row output

**Explanation:** Each row provides different statistical measures for assessing fit quality and coefficient reliability.

---

### Example 6: Calculate Doubling Time
```
=LN(2)/LN(INDEX(LOGEST(YData, XData), 1, 1))
```
**Result:** Time for values to double

**Explanation:** Doubling time = ln(2)/ln(m). If m=1.07 (7% growth), doubling time = 0.693/0.0677 = 10.24 periods.

---

### Example 7: Calculate Half-Life (Decay)
```
=LN(0.5)/LN(INDEX(LOGEST(DecayData, TimeData), 1, 1))
```
**Result:** Half-life for exponential decay

**Explanation:** For decay (m < 1), half-life = ln(0.5)/ln(m). Negative result indicates decay direction.

---

### Example 8: Force Through Origin
```
=LOGEST({1,2,4,8}, {0,1,2,3}, FALSE)
```
**Result:** {2} (single value when const=FALSE)

**Explanation:** With const=FALSE, b is forced to 1, so only m is returned. Curve passes through (0,1).

---

### Example 9: Multiple Independent Variables
```
=LOGEST(C2:C20, A2:B20, TRUE, FALSE)
```
**Result:** {m2, m1, b}

**Explanation:** With two x columns, fits y = b * m1^x1 * m2^x2. Returns three coefficients.

---

### Example 10: R-Squared from Full Statistics
```
=INDEX(LOGEST(B2:B100, A2:A100, TRUE, TRUE), 3, 1)
```
**Result:** R-squared value

**Explanation:** R-squared is in row 3, column 1 of the statistics output. Indicates goodness of fit (0 to 1).

---

### Example 11: Verify GROWTH Matches LOGEST
```
LOGEST output: m=2, b=100
GROWTH({100,200,400}, {1,2,3}, {4}) = 800
Manual: 100 * 2^4 = 1600... wait, GROWTH({100,200,400}, {1,2,3}, {4})
```
**Result:** GROWTH predictions match b * m^x

**Explanation:** You can verify: GROWTH result should equal INDEX(LOGEST(),1,2) * INDEX(LOGEST(),1,1)^new_x.

---

### Example 12: Annual Growth Rate Calculation
```
=INDEX(LOGEST(AnnualRevenue, Years), 1, 1) - 1
```
**Result:** Annual growth rate as decimal

**Explanation:** If LOGEST returns m=1.15, growth rate is 0.15 or 15% per year.

---

### Example 13: Comparing Linear vs Exponential Fit
```
Exponential R²: =INDEX(LOGEST(Y, X, TRUE, TRUE), 3, 1)
Linear R²:      =INDEX(LINEST(Y, X, TRUE, TRUE), 3, 1)
```
**Result:** Compare which model fits better

**Explanation:** Higher R-squared indicates better fit. Compare to decide between linear and exponential models.

---

### Example 14: Standard Error of Growth Rate
```
=INDEX(LOGEST(B2:B50, A2:A50, TRUE, TRUE), 2, 1)
```
**Result:** Standard error of m estimate

**Explanation:** Row 2 contains standard errors. Useful for confidence intervals: m plus/minus 1.96*SE for 95% CI.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Zero or negative y-values | All y-values must be positive for exponential regression. Filter or transform data. |
| `#NUM!` | Calculation cannot converge | Data may not fit exponential model. Check for data issues or try linear regression. |
| `#REF!` | Mismatched array dimensions | Ensure known_x's and known_y's have same number of rows. |
| `#VALUE!` | Non-numeric data | Check for text or errors in data ranges. |
| `Array not expanding` | Legacy Excel | In older Excel, select 5-row output area first, enter formula, press Ctrl+Shift+Enter. |
| `Only one value returned` | const=FALSE | When forcing b=1, only m values are returned. This is correct behavior. |

## Use Cases

### [[Scientific Growth Modeling]]

**Scenario:** Determine the exact growth equation for biological or chemical processes.

**Implementation:**
```
=LOGEST(BacteriaCount, Hours, TRUE, TRUE)
Growth rate: =INDEX(LOGEST(BacteriaCount, Hours), 1, 1)
Doubling time: =LN(2)/LN(INDEX(LOGEST(BacteriaCount, Hours), 1, 1))
```

**Business Application:** Laboratory research, pharmaceutical development, environmental studies. Scientists need exact parameters for papers and further calculations.

**Technical Details:** LOGEST provides publishable results: "Growth followed exponential model y = 102.3 * 1.087^t (R² = 0.994)."

---

### [[Financial Growth Analysis]]

**Scenario:** Calculate compound annual growth rate (CAGR) and assess growth consistency.

**Implementation:**
```
=INDEX(LOGEST(Revenue, Years), 1, 1) - 1                → CAGR
=INDEX(LOGEST(Revenue, Years, TRUE, TRUE), 3, 1)        → R² (consistency)
=INDEX(LOGEST(Revenue, Years, TRUE, TRUE), 2, 1)        → SE of growth rate
```

**Business Application:** Investment analysis, company valuation, financial reporting. CAGR is standard metric for multi-year growth.

**Technical Details:** LOGEST provides more robust CAGR than simple start/end calculation, using all data points and providing fit statistics.

---

### [[Radioactive Decay Analysis]]

**Scenario:** Determine decay constants and half-lives from measurement data.

**Implementation:**
```
Decay constant: =LN(INDEX(LOGEST(Activity, Time), 1, 1))
Half-life: =LN(0.5)/LN(INDEX(LOGEST(Activity, Time), 1, 1))
Initial activity: =INDEX(LOGEST(Activity, Time), 1, 2)
```

**Business Application:** Nuclear medicine, environmental monitoring, archaeological dating. Decay parameters are fundamental to these fields.

**Technical Details:** For decay, m < 1. The natural log of m gives the decay constant commonly used in physics equations.

---

### [[Depreciation Curve Fitting]]

**Scenario:** Model asset value decline over time for accounting and planning.

**Implementation:**
```
=LOGEST(AssetValues, Age)
Annual depreciation rate: =1 - INDEX(LOGEST(AssetValues, Age), 1, 1)
Projected value: =INDEX(LOGEST(...), 1, 2) * INDEX(LOGEST(...), 1, 1)^FutureAge
```

**Business Application:** Asset management, lease accounting, insurance valuation. Exponential depreciation often fits better than straight-line for technology assets.

**Technical Details:** Compare exponential R² to linear R² to determine if exponential depreciation model is appropriate for specific asset class.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 5.0
- **Array formula:** Legacy versions require Ctrl+Shift+Enter for full statistics output; Excel 365/2021+ handles dynamic arrays
- **Output dimensions:** Returns 2-column output (or more for multiple regression) with up to 5 rows if stats=TRUE
- **Precision:** Full floating-point precision in all statistics

### Google Sheets
- **Availability:** All versions
- **Array formula:** Works natively without special entry
- **Output dimensions:** Same as Excel
- **Performance:** Good performance for typical datasets

### Both Platforms
- Identical mathematical algorithm (log-transform linear regression)
- Same parameter order: mn, ..., m1, b (right-to-left for x variables)
- Same statistics output structure
- Same error conditions
- Results match to floating-point precision

## Tips and Best Practices

1. **Understand the output order:** For multiple x variables, coefficients are returned right-to-left: [mn, mn-1, ..., m1, b]. The last value is always b.

2. **Use INDEX to extract specific values:** LOGEST returns an array. Use INDEX(LOGEST(...), row, column) to get specific statistics.

3. **Check R-squared for fit quality:** Enable stats=TRUE and examine R² (row 3, col 1). Values close to 1 indicate good exponential fit.

4. **Calculate confidence intervals:** Use standard errors (row 2) to compute confidence intervals: coefficient plus/minus t-value * SE.

5. **Compare with LINEST:** Run both LOGEST and LINEST on your data. Compare R² values to determine if exponential or linear model fits better.

6. **Remember log transformation:** LOGEST minimizes errors in log space, meaning it minimizes relative (percentage) errors, not absolute errors.

7. **Calculate derived quantities:** From m, calculate doubling time (ln(2)/ln(m)), growth rate (m-1), or decay constant (ln(m)).

8. **Verify with GROWTH:** Cross-check that GROWTH predictions match b * m^x using LOGEST parameters. This validates your understanding.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[GROWTH]] | Predicts y-values from exponential fit | When you need predictions, not parameters |
| [[LINEST]] | Returns linear regression parameters | When data follows linear pattern |
| [[TREND]] | Predicts y-values from linear fit | When you need linear predictions |
| [[EXP]] | Exponential function | For manual calculations with LOGEST parameters |

### Commonly Used Together

**[[GROWTH]]** - Predictions from same model

*Verify relationship:*
```
Parameters: =LOGEST(Y, X)
Prediction: =GROWTH(Y, X, new_x)
Manual:     =INDEX(LOGEST(Y,X),1,2) * INDEX(LOGEST(Y,X),1,1)^new_x
```
GROWTH and manual calculation from LOGEST should match.

---

**[[INDEX]]** - Extract specific statistics

*Get individual values:*
```
=INDEX(LOGEST(Y, X, TRUE, TRUE), 1, 1)  → m (growth factor)
=INDEX(LOGEST(Y, X, TRUE, TRUE), 1, 2)  → b (coefficient)
=INDEX(LOGEST(Y, X, TRUE, TRUE), 3, 1)  → R-squared
```
INDEX is essential for extracting elements from LOGEST array.

---

**[[LN]]** - Convert to linear form

*Logarithmic transformation:*
```
=LN(INDEX(LOGEST(Y, X), 1, 1))  → Continuous growth rate
=LN(2)/LN(INDEX(LOGEST(Y, X), 1, 1))  → Doubling time
```
Natural log converts exponential parameters to additive form.

---

**[[LINEST]]** - Compare fit quality

*Model selection:*
```
Exponential R²: =INDEX(LOGEST(Y, X, TRUE, TRUE), 3, 1)
Linear R²:      =INDEX(LINEST(Y, X, TRUE, TRUE), 3, 1)
```
Compare R-squared values to choose better model.

---

**[[EXP]]** - Reconstruct predictions

*Manual prediction:*
```
=INDEX(LOGEST(Y,X),1,2) * INDEX(LOGEST(Y,X),1,1)^new_x
```
Equivalent to GROWTH, using LOGEST parameters directly.

## Official Documentation

- **Microsoft Excel:** [LOGEST function](https://support.microsoft.com/en-us/office/logest-function-f27462d8-3657-4030-866b-a272c1d18b4b)
- **Google Sheets:** [LOGEST function](https://support.google.com/docs/answer/3094249)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original implementation |
| Excel 2007 | Enhanced | Improved array handling |
| Excel 365 | 2018+ | Dynamic array support |
| Google Sheets | Original launch | Full compatibility with Excel |

---

*Last updated: 2026-01-10*
