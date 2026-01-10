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
- trend-analysis
- forecasting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Trend Values
- Linear Trend
- Regression Prediction
tags:
- statistical
- regression
- trend
- prediction
- forecasting
---

# TREND

## Description

**TREND** returns predicted Y values based on a linear regression model, calculating multiple predictions at once using the regression equation Y = b + m*X. Unlike FORECAST.LINEAR which returns a single prediction, TREND is an array function that can return fitted values for existing data points (to assess model fit) or predictions for new X values (for forecasting). This makes TREND the go-to function for generating trendlines and projections across a range of values.

The function performs linear regression using the least squares method, identical to what SLOPE, INTERCEPT, and LINEST compute. TREND simply applies the resulting equation to generate predicted values rather than returning the coefficients themselves. For simple regression, TREND(known_y's, known_x's, new_x's) returns: INTERCEPT(known_y's, known_x's) + SLOPE(known_y's, known_x's) * new_x's for each new_x value.

What makes TREND particularly powerful is its support for **multiple regression**. When you provide multiple columns of X values, TREND fits a multiple regression model and generates predictions that account for all predictors simultaneously. This enables sophisticated forecasting that considers multiple influencing factors, not just a single variable.

TREND can also force the regression through the origin (Y=0 when X=0) using the optional const parameter. This is appropriate when theory demands zero input produces zero output, but use it cautiously since forcing through origin can substantially change the slope estimate if the true intercept is non-zero.

## Syntax

> [!f(x)] TREND Syntax
>
> ```
> =TREND(known_y's, [known_x's], [new_x's], [const])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `known_y's` | Yes | The dependent variable data used to fit the regression model. |
| `known_x's` | No | The independent variable(s) for fitting. Multiple columns for multiple regression. If omitted, uses {1,2,3,...n}. |
| `new_x's` | No | The X values for which to calculate predicted Y values. If omitted, uses the same values as known_x's (returns fitted values). |
| `const` | No | TRUE (default) to calculate intercept normally; FALSE to force intercept to zero. |

### Return Value

Returns an array of predicted Y values, one for each row in new_x's. Size matches new_x's (or known_y's if new_x's is omitted). In legacy Excel, requires Ctrl+Shift+Enter if entered in a range.

## Examples

> [!f(x)] TREND Examples

### Example 1: Fitted Values (Model Assessment)
```
=TREND(B2:B11, A2:A11)
```
Where new_x's is omitted, so uses known_x's

**Result:** Array of 10 predicted Y values

**Explanation:** Returns the Y values that fall exactly on the regression line at each original X value. Compare to actual Y values to assess fit.

---

### Example 2: Future Predictions
```
=TREND(B2:B11, A2:A11, {11;12;13})
```
**Result:** Predicted Y values for X = 11, 12, 13

**Explanation:** Extends the trend line beyond observed data to forecast future values. Use for projections and planning.

---

### Example 3: Single Point Prediction
```
=TREND(Sales, Month, 15)
```
**Result:** Predicted sales for month 15

**Explanation:** For single predictions, TREND and FORECAST.LINEAR are equivalent. FORECAST.LINEAR syntax may be clearer for this use case.

---

### Example 4: Creating a Trendline
```
In column D: =TREND($B$2:$B$11, $A$2:$A$11, A2:A11)
```
**Result:** Trendline values for plotting

**Explanation:** Generate Y values that fall on the regression line, useful for charting trendlines alongside actual data.

---

### Example 5: Multiple Regression Prediction
```
=TREND(Sales, A2:B11, A12:B14)
```
Where A2:B11 contains two predictor columns (e.g., Advertising, Price)

**Result:** Predicted sales based on both factors

**Explanation:** TREND fits a multiple regression and generates predictions using all predictors. Essential when Y depends on multiple factors.

---

### Example 6: Time Series with No X Variable
```
=TREND(Revenue,,)
```
**Result:** Fitted values using implicit {1,2,3,...} as X

**Explanation:** When known_x's is omitted, TREND uses sequential integers. Useful for simple time series trend extraction.

---

### Example 7: Force Through Origin
```
=TREND(Distance, Time, NewTime, FALSE)
```
**Result:** Predictions with intercept = 0

**Explanation:** Forces Distance = 0 when Time = 0. Appropriate when physical laws require this (e.g., distance = speed * time with zero starting position).

---

### Example 8: Residual Calculation
```
=B2:B11 - TREND(B2:B11, A2:A11)
```
**Result:** Array of residuals (actual - predicted)

**Explanation:** Residuals reveal how well the model fits each observation. Large residuals indicate outliers or model problems.

---

### Example 9: Historical Backcast
```
=TREND(Sales, Year, {2018;2019})
```
Where historical data starts from 2020

**Result:** Estimated sales for 2018 and 2019

**Explanation:** TREND can predict backward as well as forward. Useful for filling gaps or estimating pre-data values.

---

### Example 10: Seasonal Adjustment Using Multiple Regression
```
=TREND(Sales, Month:SeasonalDummy, NewMonth:NewDummy)
```
**Result:** Predictions accounting for seasonality

**Explanation:** Include seasonal dummy variables as additional predictors to deseasonalize forecasts.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Input arrays have incompatible dimensions | Ensure known_y's and known_x's have same number of rows |
| `#VALUE!` | Non-numeric data in inputs | Verify all data is numeric |
| `#REF!` | Output does not fit in selected cells (legacy Excel) | Select appropriate range before entering formula |
| `Wrong number of results` | new_x's has different dimensions than expected | Verify new_x's dimensions match your needs |
| `Unexpected predictions` | Argument order incorrect | Order is Y, X, new_X, not X, Y |

## Use Cases

### [[Sales Forecasting]]

**Scenario:** A financial analyst projects quarterly sales for the upcoming fiscal year based on historical trends.

**Implementation:**
```
Historical: =TREND(QuarterlySales, QuarterNumber)
Forecast: =TREND(QuarterlySales, QuarterNumber, {13;14;15;16})
```

**Business Application:** TREND generates projections by extending historical patterns. Use for budget planning, inventory management, and capacity planning. Compare TREND forecasts against actual results to assess forecast accuracy over time.

**Technical Details:** Simple linear trend assumes constant growth rate in absolute terms. For percentage growth, consider LOGEST/GROWTH. Add confidence intervals using STEYX for uncertainty bounds. Monitor forecast errors to detect when relationships change.

---

### [[Model Visualization]]

**Scenario:** A data analyst creates visualizations showing actual values versus fitted trend line to communicate analytical insights.

**Implementation:**
```
Actuals: B2:B100 (plot as points)
Trend: =TREND(B2:B100, A2:A100) (plot as line)
```

**Business Application:** Showing both actual data and the trend line helps stakeholders understand underlying patterns versus noise. The trend line smooths out fluctuations, revealing the core relationship. Gaps between actuals and trend (residuals) highlight unusual periods worth investigating.

**Technical Details:** For better visualization, calculate trend line at a few key points rather than all observations. Add confidence bands (trend +/- 2*STEYX) to show prediction uncertainty visually.

---

### [[What-If Analysis]]

**Scenario:** A business planner explores how different input scenarios would affect predicted outcomes.

**Implementation:**
```
=TREND(HistoricalOutput, HistoricalInput, ScenarioInputs)
```
Where ScenarioInputs contains different possible future values.

**Business Application:** TREND enables rapid scenario analysis by generating predictions for multiple input assumptions simultaneously. Compare optimistic, expected, and pessimistic scenarios to understand the range of possible outcomes and develop contingency plans.

**Technical Details:** Scenarios should stay within reasonable extrapolation range; predictions far from historical X values are unreliable. Consider sensitivity by varying one input while holding others constant (for multiple regression).

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Array formula behavior:** Legacy requires Ctrl+Shift+Enter; Excel 365 spills automatically
- **Multiple regression:** Supports multiple X columns
- **Maximum size:** Limited by worksheet dimensions
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Array formula behavior:** Native array support; spills automatically
- **Multiple regression:** Supports multiple X columns
- **Maximum size:** Limited by sheet size
- **Precision:** 15 significant decimal digits

### Key Difference Alert

TREND behaves identically between Excel and Google Sheets for calculations. The main difference is array formula handling in legacy Excel versions. Excel 365 and Google Sheets both automatically spill results across multiple cells.

## Tips and Best Practices

1. **Use for fitted values and predictions:** TREND serves two purposes: generating fitted values (new_x's omitted) to assess model fit, and predictions (new_x's provided) for forecasting.

2. **Prefer FORECAST.LINEAR for single predictions:** For predicting one Y value from one X, FORECAST.LINEAR has clearer syntax. Use TREND when you need multiple predictions.

3. **Check before extrapolating:** Predictions for X values far from your data are unreliable. The relationship may not extend beyond the observed range.

4. **Calculate residuals:** Actual - TREND gives residuals. Plot these to check for patterns that indicate model problems (non-linearity, heteroscedasticity).

5. **Combine with LINEST for full analysis:** Use LINEST to get regression statistics (R-squared, standard errors), then TREND to generate predictions.

6. **Force through origin carefully:** Only use const=FALSE when theory demands it. Forcing intercept to zero when it should not be distorts predictions.

7. **Handle multiple regression correctly:** For multiple X variables, ensure all columns are included for both fitting and prediction. Missing a predictor in new_x's causes errors.

8. **Compare to actual outcomes:** Track forecast accuracy by comparing TREND predictions to actual results as new data becomes available.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FORECAST.LINEAR]] | Single point prediction | When you need one prediction with clearer syntax |
| [[GROWTH]] | Exponential trend predictions | When growth is percentage-based, not linear |
| [[LINEST]] | Regression coefficients | When you need the equation, not just predictions |
| [[FORECAST.ETS]] | Time series forecasting | For data with seasonality and trend patterns |

### Commonly Used Together

**[[LINEST]]** - Regression statistics

*Complete analysis:*
```
Statistics: =LINEST(Y, X, TRUE, TRUE)
Predictions: =TREND(Y, X, NewX)
```
LINEST provides the model; TREND applies it.

---

**[[STEYX]]** - Standard error

*Prediction intervals:*
```
Point: =TREND(Y, X, NewX)
Lower: =TREND(Y, X, NewX) - 2*STEYX(Y, X)
Upper: =TREND(Y, X, NewX) + 2*STEYX(Y, X)
```
Approximate 95% prediction bounds.

---

**[[RSQ]]** - Model fit

*Assess trend reliability:*
```
Predictions: =TREND(Y, X)
Fit: =RSQ(Y, X)
```
Higher R-squared means trend predictions are more reliable.

## Official Documentation

- **Microsoft Excel:** [TREND function](https://support.microsoft.com/en-us/office/trend-function-e2f135f0-8827-4096-9873-9a7cf7b51ef1)
- **Google Sheets:** [TREND function](https://support.google.com/docs/answer/3094249)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2007 | Continued support | Increased capacity |
| Excel 365 | Continuous updates | Dynamic array spilling |
| Google Sheets | Original launch (2006) | Full support |

---

*Last updated: 2026-01-10*
