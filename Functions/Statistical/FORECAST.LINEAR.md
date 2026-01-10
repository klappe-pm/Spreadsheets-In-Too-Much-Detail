---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- linear-regression
- prediction
- forecasting
- trend-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Linear Forecast
- Point Prediction
- Trend Forecast
tags:
- statistical
- regression
- forecast
- prediction
- linear
---

# FORECAST.LINEAR

## Description

**FORECAST.LINEAR** predicts a Y value for a specified X value using simple linear regression, returning the point on the best-fit line at that X position. Introduced in Excel 2016, this function is the modern replacement for the legacy FORECAST function, with identical behavior but a name that clearly indicates linear projection is being used. The ".LINEAR" suffix distinguishes it from FORECAST.ETS which handles time series with trends and seasonality.

The function implements the fundamental regression prediction: Y_predicted = intercept + slope * X. It fits a line to your historical data using least squares (minimizing the sum of squared distances between points and line) and evaluates that line at your specified X value. This is mathematically equivalent to INTERCEPT(known_y's, known_x's) + SLOPE(known_y's, known_x's) * x, just more convenient.

FORECAST.LINEAR is designed for simple, single-point predictions. When you need to predict Y for multiple X values, TREND is more efficient. When your data shows seasonal patterns or non-linear trends, FORECAST.ETS may be more appropriate. When you need regression statistics (R-squared, standard errors) in addition to predictions, use LINEST. FORECAST.LINEAR excels at answering the simple question: "What does the linear trend predict for this specific X value?"

Like all prediction functions, FORECAST.LINEAR returns a point estimate without uncertainty bounds. Responsible forecasting requires acknowledging that predictions are uncertain. Supplement with STEYX for approximate prediction intervals: FORECAST.LINEAR +/- 2*STEYX gives rough 95% bounds around the prediction.

## Syntax

> [!f(x)] FORECAST.LINEAR Syntax
>
> ```
> =FORECAST.LINEAR(x, known_y's, known_x's)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The X value for which to predict Y. Numeric value or cell reference. |
| `known_y's` | Yes | The dependent variable data (historical Y values). Array or range. |
| `known_x's` | Yes | The independent variable data (historical X values). Must have same count as known_y's. |

### Return Value

Returns a single numeric value representing the predicted Y at the specified X. Returns #N/A if arrays have different lengths. Returns #DIV/0! if X values have no variance.

## Examples

> [!f(x)] FORECAST.LINEAR Examples

### Example 1: Basic Future Prediction
```
=FORECAST.LINEAR(15, B2:B11, A2:A11)
```
Where A2:A11 contains values 1-10 and B2:B11 contains corresponding Y values

**Result:** Predicted Y at X = 15

**Explanation:** Extends the linear trend from X=1-10 to predict what Y would be at X=15.

---

### Example 2: Sales Forecast
```
=FORECAST.LINEAR(DATE(2027,1,1), MonthlySales, MonthDates)
```
**Result:** Projected sales for January 2027

**Explanation:** Uses date values as X for time-based forecasting. Linear trend extrapolates to future dates.

---

### Example 3: Temperature-Dependent Prediction
```
=FORECAST.LINEAR(90, EnergyUsage, Temperature)
```
**Result:** Predicted energy usage when temperature is 90 degrees

**Explanation:** Uses historical temperature-energy relationship to predict usage at a specific temperature.

---

### Example 4: Verify Against Legacy Function
```
Modern: =FORECAST.LINEAR(10, Y, X)
Legacy: =FORECAST(10, Y, X)
```
**Result:** Both return identical values

**Explanation:** FORECAST.LINEAR is exactly equivalent to FORECAST. Use the modern version for new work.

---

### Example 5: Verify Against Components
```
FORECAST.LINEAR: =FORECAST.LINEAR(10, Y, X)
Components: =INTERCEPT(Y, X) + SLOPE(Y, X) * 10
```
**Result:** Both return identical values

**Explanation:** FORECAST.LINEAR applies the regression equation directly.

---

### Example 6: Cell Reference for Dynamic Prediction
```
=FORECAST.LINEAR(D2, Sales, Month)
```
Where D2 contains the month number to forecast

**Result:** Predicted sales for the month in D2

**Explanation:** Using a cell reference enables easy sensitivity analysis and what-if scenarios.

---

### Example 7: Interpolation (Within Data Range)
```
=FORECAST.LINEAR(5.5, Y, X)
```
Where X values are 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**Result:** Estimated Y at X = 5.5

**Explanation:** FORECAST.LINEAR interpolates between observed points, not just extrapolates beyond them.

---

### Example 8: Finding Intercept
```
=FORECAST.LINEAR(0, Y, X)
```
**Result:** Equals INTERCEPT(Y, X)

**Explanation:** The predicted Y at X=0 is by definition the Y-intercept of the regression line.

---

### Example 9: With Prediction Interval
```
Point: =FORECAST.LINEAR(NewX, Y, X)
Lower: =FORECAST.LINEAR(NewX, Y, X) - 2*STEYX(Y, X)
Upper: =FORECAST.LINEAR(NewX, Y, X) + 2*STEYX(Y, X)
```
**Result:** Forecast with approximate 95% confidence bounds

**Explanation:** STEYX provides the typical prediction error, enabling uncertainty quantification.

---

### Example 10: Multiple Predictions Comparison
```
Single: =FORECAST.LINEAR(11, Y, X)
Array: =INDEX(TREND(Y, X, 11), 1)
```
**Result:** Both return identical values

**Explanation:** For single predictions, FORECAST.LINEAR and TREND are equivalent. TREND is better for multiple predictions.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | known_y's and known_x's have different lengths | Ensure both ranges have equal number of values |
| `#DIV/0!` | X values have no variance (all identical) | Need different X values to calculate slope |
| `#VALUE!` | X argument is text or non-numeric | Ensure x is a numeric value |
| `#NAME?` | Function not recognized | Excel 2016+ or Google Sheets required; use FORECAST in older versions |
| `Unreliable prediction` | Extrapolating far from data | Keep predictions reasonably close to observed X range |

## Use Cases

### [[Quarterly Business Planning]]

**Scenario:** A business analyst forecasts next quarter's metrics based on historical trends to support quarterly planning.

**Implementation:**
```
Revenue Forecast: =FORECAST.LINEAR(NextQuarter, QuarterlyRevenue, QuarterNumber)
Expense Forecast: =FORECAST.LINEAR(NextQuarter, QuarterlyExpense, QuarterNumber)
Profit Forecast: =Revenue_Forecast - Expense_Forecast
```

**Business Application:** Linear trend projection provides a baseline expectation for planning purposes. The forecast informs budget allocation, staffing decisions, and goal setting. Track forecast accuracy over time to improve future projections.

**Technical Details:** Quarterly data often has seasonal patterns that linear trend ignores. Consider FORECAST.ETS for seasonal data, or include quarter-of-year as a separate factor using multiple regression. Compare forecasts to actuals and investigate systematic errors.

---

### [[Goal Tracking and Projections]]

**Scenario:** A project manager tracks progress toward goals and projects completion dates based on current trajectory.

**Implementation:**
```
Current Trajectory: =FORECAST.LINEAR(TargetDate, CompletedUnits, DateIndex)
Time to Goal: =FORECAST.LINEAR(TargetUnits, DateIndex, CompletedUnits)
```

**Business Application:** Understanding the current pace helps identify whether projects are on track. If FORECAST.LINEAR at target date falls short of goals, early intervention is possible. Projecting when goals will be achieved based on current progress informs stakeholder expectations.

**Technical Details:** Linear projection assumes constant productivity. Actual projects often have acceleration (learning curve) or deceleration (difficult final tasks). Update projections frequently as new data becomes available.

---

### [[Pricing Strategy Support]]

**Scenario:** A pricing analyst estimates demand at different price points to support pricing decisions.

**Implementation:**
```
=FORECAST.LINEAR(ProposedPrice, HistoricalDemand, HistoricalPrice)
```

**Business Application:** Historical price-demand data reveals how customers respond to pricing. FORECAST.LINEAR estimates demand at price points not previously tested, supporting price optimization. Combine with margin analysis to identify profit-maximizing prices.

**Technical Details:** Price-demand relationships are often non-linear (price elasticity varies by price level). Validate linearity with scatter plots. Log-log transformations may better capture demand response. Avoid extrapolating to prices far from tested range.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2016 and later
- **Replaces:** FORECAST (legacy, still works)
- **Related functions:** Part of FORECAST.* family including FORECAST.ETS
- **Behavior:** Identical to FORECAST
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** Supported in all versions
- **Alternative:** FORECAST also available
- **Behavior:** Identical to Excel
- **Precision:** 15 significant decimal digits

### Key Difference Alert

FORECAST.LINEAR behaves identically between Excel and Google Sheets. In Excel, it was introduced in 2016 as part of a forecasting function update. For earlier Excel versions (2013 and before), use FORECAST instead (identical functionality).

## Tips and Best Practices

1. **Prefer FORECAST.LINEAR over FORECAST:** The modern function name is clearer and indicates the linear method being used. It helps distinguish from FORECAST.ETS for seasonal data.

2. **Remember argument order:** The x to predict comes first: FORECAST.LINEAR(x, Y, X). This is consistent with FORECAST but different from SLOPE(Y, X).

3. **Always add uncertainty bounds:** A point prediction without error range can be misleading. Use +/- 2*STEYX for approximate 95% prediction intervals.

4. **Validate the linear assumption:** Before forecasting, check that the relationship is approximately linear using scatter plots or CORREL. Non-linear relationships require different approaches.

5. **Stay close to your data:** Predictions are most reliable within or near the observed X range. Extrapolating far beyond historical data increases uncertainty dramatically.

6. **Track forecast accuracy:** Compare predictions to actual outcomes. Persistent over- or under-prediction indicates the model needs adjustment.

7. **Use TREND for multiple predictions:** When you need predictions for several X values, TREND is more efficient than multiple FORECAST.LINEAR calls.

8. **Consider FORECAST.ETS for time series:** If your data has seasonal patterns, trends, or complex time dynamics, FORECAST.ETS may provide better forecasts than simple linear projection.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FORECAST]] | Legacy version (identical) | Only for backward compatibility |
| [[TREND]] | Array of predictions | When you need multiple predictions |
| [[FORECAST.ETS]] | Time series with seasonality | For data with trends and seasonal patterns |
| [[GROWTH]] | Exponential predictions | For percentage-based growth patterns |

### Commonly Used Together

**[[STEYX]]** - Standard error of estimate

*Add prediction uncertainty:*
```
=FORECAST.LINEAR(NewX, Y, X) & " +/- " & ROUND(2*STEYX(Y, X), 2)
```
Communicates both prediction and uncertainty.

---

**[[RSQ]]** - Coefficient of determination

*Assess prediction reliability:*
```
Prediction: =FORECAST.LINEAR(NewX, Y, X)
Reliability: =RSQ(Y, X)
```
Higher R-squared suggests more reliable forecasts.

---

**[[LINEST]]** - Full regression output

*Complete model with statistics:*
```
Prediction: =FORECAST.LINEAR(NewX, Y, X)
Full Stats: =LINEST(Y, X, TRUE, TRUE)
```
LINEST provides R-squared, standard errors, and significance tests.

---

**[[SLOPE]] and [[INTERCEPT]]** - Regression components

*Understand the model:*
```
Equation: Y = INTERCEPT(Y,X) + SLOPE(Y,X) * X
Applied: =FORECAST.LINEAR(NewX, Y, X)
```
See what equation is being applied.

## Official Documentation

- **Microsoft Excel:** [FORECAST.LINEAR function](https://support.microsoft.com/en-us/office/forecast-linear-function-a39f4c55-af55-4f75-ab70-e9cb5b1c2e15)
- **Google Sheets:** [FORECAST function](https://support.google.com/docs/answer/3094000)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2016 | Initial release | Part of forecasting function update |
| Excel 2019 | Continued support | No changes |
| Excel 365 | Current | Fully supported |
| Google Sheets | Supported | Full compatibility with Excel |

---

*Last updated: 2026-01-10*
