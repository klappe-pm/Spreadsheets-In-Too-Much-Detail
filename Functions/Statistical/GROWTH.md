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
- forecasting
- trend-analysis
- curve-fitting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Exponential Growth
- Exponential Trend
- Exponential Regression
tags:
- exponential
- regression
- forecasting
- trend
- growth-rate
- curve-fitting
---

# GROWTH

## Description

**GROWTH** calculates predicted y-values along an exponential growth curve based on existing x-y data pairs. It fits the equation y = b * m^x to your data (where m is the growth factor and b is the y-intercept), then uses this exponential model to predict new y-values for specified x-values. This is the exponential counterpart to TREND, which fits linear data.

Use GROWTH when your data exhibits **exponential patterns rather than linear ones**. This includes population growth, compound interest calculations, bacterial colony expansion, viral spread models, technology adoption curves, and any phenomenon where growth rate is proportional to current size. If your data doubles at regular intervals or shows percentage-based growth, GROWTH is likely more appropriate than TREND.

The function can operate in several modes: predicting y-values for new x-values, fitting a curve through existing data, forcing the curve through a specific intercept, or calculating multiple regression with several independent variables. When arrays are involved, GROWTH returns an array of values—in older Excel, enter as an array formula with Ctrl+Shift+Enter; modern Excel and Google Sheets handle this automatically.

**Critical gotcha: GROWTH requires positive y-values.** Exponential curves cannot produce zero or negative values, so any zero or negative values in your known_y's will cause a #NUM! error. If your data contains zeros, consider adding a small constant before using GROWTH, or evaluate whether exponential modeling is truly appropriate for your dataset.

## Syntax

> [!f(x)] GROWTH Syntax
>
> ```
> =GROWTH(known_y's, [known_x's], [new_x's], [const])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `known_y's` | Yes | The set of y-values you already know in the relationship y = b*m^x. Must be positive numbers. This is your dependent variable data. |
| `known_x's` | No | The set of x-values corresponding to known_y's. If omitted, assumed to be {1, 2, 3, ...} matching the count of known_y's. |
| `new_x's` | No | The x-values for which you want GROWTH to calculate corresponding y-values. If omitted, assumed to be the same as known_x's (returns fitted values). |
| `const` | No | TRUE (default) or omitted: calculates b normally. FALSE: forces b = 1, making the curve pass through y=1 when x=0. |

### Return Value

Returns an array of predicted y-values based on the exponential regression curve fitted to your data. If new_x's is a single value, returns a single number. If new_x's is an array, returns an array of the same dimensions. Returns #NUM! if known_y's contains zero or negative values, or if the calculation cannot converge.

## Examples

> [!f(x)] GROWTH Examples

### Example 1: Basic Exponential Forecast
```
=GROWTH({2,4,8,16}, {1,2,3,4}, {5})
```
**Result:** 32

**Explanation:** The y-values double for each unit increase in x, forming a perfect exponential curve y = 1 * 2^x. For x=5, the predicted value is 2^5 = 32.

---

### Example 2: Multiple Predictions at Once
```
=GROWTH({100,150,225}, {1,2,3}, {4,5,6})
```
**Result:** {337.5, 506.25, 759.375}

**Explanation:** GROWTH fits an exponential curve to the data (growth factor of approximately 1.5), then predicts values for x = 4, 5, and 6. Each prediction follows the same exponential pattern.

---

### Example 3: Using Cell Ranges
```
=GROWTH(B2:B10, A2:A10, A11:A15)
```
**Result:** Array of predicted values for future x-values

**Explanation:** Fits exponential curve to data in columns A and B, then predicts y-values for the x-values in A11:A15. Common pattern for forecasting time series data.

---

### Example 4: Omitting Known X's
```
=GROWTH({10,20,40,80})
```
**Result:** {10, 20, 40, 80}

**Explanation:** With only known_y's provided, assumes x = {1, 2, 3, 4} and returns the fitted values. Since the data is perfectly exponential, fitted values equal original values.

---

### Example 5: Population Growth Projection
```
=GROWTH({1000,1200,1440,1728}, {2020,2021,2022,2023}, {2024,2025})
```
**Result:** {2073.6, 2488.32}

**Explanation:** Population growing at 20% annually. GROWTH identifies this exponential pattern and projects 2024 and 2025 populations continuing the same growth rate.

---

### Example 6: Forcing Curve Through Origin (const=FALSE)
```
=GROWTH({1,2,4,8}, {0,1,2,3}, {4}, FALSE)
```
**Result:** 16

**Explanation:** With const=FALSE, forces b=1, so the curve passes through y=1 when x=0. The curve becomes y = 2^x, predicting 2^4 = 16 for x=4.

---

### Example 7: Bacterial Growth Model
```
=GROWTH({100,200,400,800,1600}, {0,1,2,3,4}, {5,6,7,8})
```
**Result:** {3200, 6400, 12800, 25600}

**Explanation:** Bacteria doubling every hour. GROWTH projects the count for hours 5-8 following the same exponential pattern.

---

### Example 8: Investment Growth Projection
```
=GROWTH({10000,10500,11025,11576.25}, {0,1,2,3}, {4,5,10,20})
```
**Result:** {12155.06, 12762.82, 16288.95, 26532.98}

**Explanation:** Investment growing at 5% annually. GROWTH projects future values, including long-term projections for years 10 and 20.

---

### Example 9: Fitted Values (Smoothing)
```
=GROWTH({95,210,390,820,1550}, {1,2,3,4,5})
```
**Result:** {102.4, 204.8, 409.6, 819.2, 1638.4}

**Explanation:** When new_x's is omitted, returns fitted values for existing x's. These smoothed values lie exactly on the best-fit exponential curve.

---

### Example 10: Technology Adoption Curve
```
=GROWTH({1000,3000,9000,27000}, {2018,2019,2020,2021}, {2022,2023,2024})
```
**Result:** {81000, 243000, 729000}

**Explanation:** Users tripling each year (300% growth). GROWTH captures this pattern and projects future user counts.

---

### Example 11: Interpolation Within Data Range
```
=GROWTH({100,400,1600}, {1,2,3}, {1.5,2.5})
```
**Result:** {200, 800}

**Explanation:** GROWTH can interpolate—predicting values between known data points. The curve predicts y-values at x = 1.5 and 2.5 using the fitted exponential.

---

### Example 12: Revenue Growth with Noise
```
=GROWTH({1000,1850,3800,7200,15500}, {1,2,3,4,5}, {6,7})
```
**Result:** {30047.7, 59043.8}

**Explanation:** Real-world data with some noise. GROWTH fits the best exponential curve through the imperfect data and projects forward.

---

### Example 13: Multiple Regression with Two X Variables
```
=GROWTH(C2:C10, A2:B10, A11:B15)
```
**Result:** Array of predictions based on two independent variables

**Explanation:** When known_x's has multiple columns, GROWTH performs exponential regression with multiple predictors. The model becomes y = b * m1^x1 * m2^x2.

---

### Example 14: Single Period Growth Factor
```
=GROWTH({100,150}, {1,2}, {2})/GROWTH({100,150}, {1,2}, {1})
```
**Result:** 1.5

**Explanation:** Dividing consecutive predictions reveals the growth factor (m in y = b*m^x). This data shows 50% growth per period.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Zero or negative values in known_y's | Exponential curves require positive y-values. Add a constant to shift all values positive, or use TREND for linear fit. |
| `#NUM!` | Data doesn't fit exponential model | If data is too random or not exponential, the calculation fails. Verify exponential pattern visually first. |
| `#REF!` | Mismatched array sizes | Ensure known_x's and known_y's have the same number of elements. |
| `#VALUE!` | Non-numeric data in ranges | Check for text, blanks, or errors in your data ranges. Clean data before using GROWTH. |
| `#NAME?` | Misspelled function | Check spelling: GROWTH, not GRWOTH or GROWHT. |
| `Array not expanding` | Legacy array formula issue | In older Excel, select output range first and press Ctrl+Shift+Enter. Modern Excel expands automatically. |

## Use Cases

### [[Sales and Revenue Forecasting]]

**Scenario:** Project future revenue based on historical exponential growth patterns.

**Implementation:**
```
=GROWTH(Revenue_History, Time_Periods, Future_Periods)
=GROWTH(B2:B13, A2:A13, A14:A25)              → 12-month projection
=GROWTH(Quarterly_Revenue, Quarter_Numbers, {13,14,15,16})  → Next year forecast
```

**Business Application:** Startups and high-growth companies often exhibit exponential revenue patterns. GROWTH extrapolates these patterns to forecast future performance, useful for financial planning, investor presentations, and resource allocation.

**Technical Details:** Exponential forecasts can become unrealistic over long horizons. Always apply business judgment to GROWTH projections—real-world constraints eventually limit exponential growth.

---

### [[Population and Biological Modeling]]

**Scenario:** Model population growth, bacterial cultures, or viral spread using exponential curves.

**Implementation:**
```
=GROWTH(Population_Counts, Time_Points, Future_Times)
=GROWTH({1000,2000,4000}, {0,24,48}, {72,96})     → Bacterial count at 72 and 96 hours
=GROWTH(Infection_Data, Days, {14,21,28})          → Disease spread projection
```

**Business Application:** Healthcare planning, laboratory research, environmental studies, and epidemiological modeling all use exponential growth patterns. GROWTH provides quick projections for resource planning.

**Technical Details:** Most biological systems eventually hit carrying capacity limits. GROWTH assumes unlimited exponential growth—use logistic models for long-term projections.

---

### [[Investment and Compound Interest Analysis]]

**Scenario:** Project investment values or analyze compound growth rates.

**Implementation:**
```
=GROWTH(Portfolio_Values, Years, Future_Years)
=GROWTH({10000,10500,11025}, {0,1,2}, {3,4,5,10})  → Project investment growth
=(GROWTH({Start,End}, {0,Periods}, {1})/Start)^(1/1)-1  → Calculate implied growth rate
```

**Business Application:** Financial advisors, portfolio managers, and personal finance planning use compound growth projections. GROWTH fits historical returns and projects future values.

**Technical Details:** Financial markets don't follow perfect exponential curves. Use GROWTH for quick estimates, but incorporate volatility and risk analysis for serious financial planning.

---

### [[Technology Adoption and Market Penetration]]

**Scenario:** Model S-curve or early-phase exponential adoption of new technologies.

**Implementation:**
```
=GROWTH(User_Counts, Launch_Quarters, Future_Quarters)
=GROWTH(Downloads, Week_Numbers, {52,53,54,55,56})
=GROWTH(Market_Share, Years, {2024,2025,2026})
```

**Business Application:** Tech companies track user growth, app downloads, and market penetration. Early-stage growth often follows exponential patterns before reaching saturation.

**Technical Details:** Exponential growth in technology eventually slows (S-curve). GROWTH works best for early-phase projections. Consider logistic models for mature markets.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 5.0
- **Array formula:** Legacy versions require Ctrl+Shift+Enter for array entry; Excel 365/2021+ handles dynamic arrays automatically
- **Performance:** Optimized for large datasets; millions of data points handled efficiently
- **Multiple regression:** Supports multiple independent variables in known_x's

### Google Sheets
- **Availability:** All versions
- **Array formula:** Works directly without special entry; use ARRAYFORMULA wrapper for explicit array operations
- **Performance:** Good performance; very large datasets may be slower than Excel
- **Multiple regression:** Fully supported with multi-column x ranges

### Both Platforms
- Identical mathematical algorithm (least squares exponential fit)
- Same parameter defaults and behavior
- Same error conditions (#NUM! for non-positive y-values)
- Results match to full floating-point precision

## Tips and Best Practices

1. **Verify exponential pattern first:** Plot your data before using GROWTH. If it doesn't look exponential (curved upward/downward), TREND or other models may be more appropriate.

2. **Watch for unrealistic long-term projections:** Exponential growth accelerates forever in theory. Real-world constraints make long-term GROWTH projections unreliable—apply business judgment.

3. **Handle zeros with care:** GROWTH requires positive y-values. If you must include near-zero data, add a small constant (e.g., 0.001) to all values, then subtract after prediction.

4. **Extract growth rate from GROWTH:** To find the implied growth factor: `=(GROWTH(data,x,{max_x+1})/GROWTH(data,x,{max_x}))` gives you m in y=b*m^x.

5. **Use logarithmic transformation for verification:** LN of exponential data should be linear. Check with `=TREND(LN(known_y's), known_x's)` to verify exponential fit quality.

6. **Compare with LOGEST:** LOGEST returns the parameters (m and b) while GROWTH returns predicted values. Use LOGEST when you need the actual growth equation.

7. **Consider confidence intervals:** GROWTH returns point estimates. For uncertainty bounds, use multiple scenarios or combine with statistical measures of fit quality.

8. **Avoid extrapolation beyond reasonable range:** Predicting far beyond your data range amplifies errors. Keep projections within 2-3x your data span when possible.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TREND]] | Linear regression predictions | When data follows a linear (straight line) pattern |
| [[LOGEST]] | Returns exponential regression parameters | When you need the actual growth equation coefficients |
| [[LINEST]] | Returns linear regression parameters | When you need linear equation coefficients |
| [[FORECAST]] | Single-value linear prediction | Simple single-point linear forecast |

### Commonly Used Together

**[[LOGEST]]** - Get exponential regression parameters

*Extract growth rate and intercept:*
```
=LOGEST(known_y's, known_x's, TRUE, TRUE)
```
LOGEST returns the m and b values that GROWTH uses internally.

---

**[[LN]]** - Verify exponential fit

*Transform to check linearity:*
```
=LN(known_y's)
```
Taking natural log of exponential data should produce linear pattern.

---

**[[TREND]]** - Compare linear vs exponential fit

*Test which model fits better:*
```
=SUMPRODUCT((known_y's-GROWTH(known_y's))^2)  → Exponential error
=SUMPRODUCT((known_y's-TREND(known_y's))^2)   → Linear error
```
Compare sum of squared errors to choose better model.

---

**[[EXP]]** - Manual exponential calculation

*Calculate from LOGEST parameters:*
```
=INDEX(LOGEST(y,x),1,2) * INDEX(LOGEST(y,x),1,1)^new_x
```
Equivalent to GROWTH using LOGEST output.

---

**[[FORECAST.ETS]]** - Advanced time series forecasting

*More sophisticated forecasting:*
```
=FORECAST.ETS(target_date, values, timeline)
```
For seasonal patterns and advanced time series analysis.

## Official Documentation

- **Microsoft Excel:** [GROWTH function](https://support.microsoft.com/en-us/office/growth-function-541a91dc-3d5e-437d-b156-21324e68b80d)
- **Google Sheets:** [GROWTH function](https://support.google.com/docs/answer/3094287)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original implementation |
| Excel 2007 | All subsequent | Enhanced array handling |
| Excel 365 | 2018+ | Dynamic array support |
| Google Sheets | Original launch | Full compatibility with Excel |

---

*Last updated: 2026-01-10*
