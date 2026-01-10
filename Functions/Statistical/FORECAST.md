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
- legacy-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Forecast
- Linear Forecast
- Point Prediction
tags:
- statistical
- regression
- forecast
- prediction
- legacy
---

# FORECAST

## Description

**FORECAST** predicts a Y value for a given X value using linear regression, returning the point on the best-fit line at the specified X position. This is the most direct way to answer "what Y do we expect when X equals this value?" The function fits a linear regression to your historical data and applies the equation Y = b + m*X at the new X point, where m is the slope and b is the intercept calculated from your data.

The function performs the same calculation as combining SLOPE and INTERCEPT: FORECAST(x, known_y's, known_x's) equals INTERCEPT(known_y's, known_x's) + SLOPE(known_y's, known_x's) * x. FORECAST simply bundles this into a single, convenient function for point predictions.

**FORECAST is a legacy function** maintained for backward compatibility. Microsoft introduced FORECAST.LINEAR in Excel 2016 as part of a forecasting function family that includes FORECAST.ETS for time series with trends and seasonality. While FORECAST continues to work and produces identical results to FORECAST.LINEAR, Microsoft recommends using the newer function name for new projects. The naming convention makes it clear that simple linear projection is being used.

The fundamental limitation of FORECAST is that it returns a point estimate without uncertainty bounds. Real-world predictions always carry uncertainty, and a single number can create false confidence. For responsible forecasting, supplement FORECAST with STEYX to understand typical prediction errors, or use the full LINEST output to construct proper confidence intervals.

## Syntax

> [!f(x)] FORECAST Syntax
>
> ```
> =FORECAST(x, known_y's, known_x's)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The X value for which you want to predict Y. Can be a single numeric value or a reference to a cell containing a number. |
| `known_y's` | Yes | The dependent variable data (historical Y values) used to fit the regression model. |
| `known_x's` | Yes | The independent variable data (historical X values). Must have the same number of points as known_y's. |

### Return Value

Returns a single numeric value representing the predicted Y at the specified X value. Returns #N/A if known_y's and known_x's have different numbers of points. Returns #DIV/0! if known_x's has zero variance.

## Examples

> [!f(x)] FORECAST Examples

### Example 1: Basic Sales Forecast
```
=FORECAST(12, B2:B11, A2:A11)
```
Where A2:A11 contains months 1-10 and B2:B11 contains monthly sales

**Result:** Predicted sales for month 12

**Explanation:** The function fits a trend line to months 1-10 and extrapolates to month 12, projecting where sales would be if the trend continues.

---

### Example 2: Temperature-Based Prediction
```
=FORECAST(75, IceCreamSales, Temperature)
```
**Result:** Predicted ice cream sales when temperature is 75 degrees

**Explanation:** Uses the historical relationship between temperature and sales to predict sales at a temperature of 75.

---

### Example 3: Verification Against SLOPE/INTERCEPT
```
FORECAST: =FORECAST(15, Y, X)
Manual: =INTERCEPT(Y, X) + SLOPE(Y, X) * 15
```
**Result:** Both formulas return identical values

**Explanation:** FORECAST is shorthand for the regression prediction equation.

---

### Example 4: Budget Projection
```
=FORECAST(2027, AnnualRevenue, Year)
```
**Result:** Projected revenue for 2027

**Explanation:** Extends the historical revenue trend to project future years. Useful for long-term planning and budgeting.

---

### Example 5: Filling Missing Data
```
=FORECAST(5, ObservedValues, TimePoints)
```
Where TimePoints is 1, 2, 3, 4, 6, 7 (missing 5)

**Result:** Estimated value at the missing time point

**Explanation:** FORECAST can interpolate within data range, not just extrapolate beyond it.

---

### Example 6: Price-Demand Prediction
```
=FORECAST(29.99, DemandQuantity, Price)
```
**Result:** Predicted demand at price $29.99

**Explanation:** Uses price-demand relationship to forecast how many units might sell at a specific price point.

---

### Example 7: Production Planning
```
=FORECAST(1000, ProductionCost, OutputVolume)
```
**Result:** Predicted total cost for producing 1000 units

**Explanation:** Based on historical cost-volume relationship, estimates costs for a planned production level.

---

### Example 8: Cell Reference for X
```
=FORECAST(D2, B2:B11, A2:A11)
```
Where D2 contains the X value for prediction

**Result:** Predicted Y value for whatever X is in D2

**Explanation:** Using a cell reference allows easy sensitivity analysis by changing D2.

---

### Example 9: Backward Prediction (Backcasting)
```
=FORECAST(0, B2:B11, A2:A11)
```
Where X data ranges from 1 to 10

**Result:** Estimated Y at X = 0 (equals INTERCEPT)

**Explanation:** FORECAST at X=0 returns the intercept. Can also estimate values before your data begins.

---

### Example 10: Multiple Forecasts with TREND
```
FORECAST: =FORECAST(11, Y, X)
TREND: =TREND(Y, X, 11)
```
**Result:** Both return identical values

**Explanation:** For a single prediction, FORECAST and TREND are equivalent. TREND is better for multiple predictions at once.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | known_y's and known_x's have different numbers of points | Ensure both ranges have equal length |
| `#DIV/0!` | known_x's has zero variance (all identical values) | X values must vary to calculate slope |
| `#VALUE!` | X is text or non-numeric | Ensure x argument is a number |
| `Unexpected result` | Argument order incorrect | Order is x, known_y's, known_x's (different from SLOPE/INTERCEPT) |
| `Poor prediction` | Extrapolating too far beyond data | Stay within reasonable range of historical X values |

## Use Cases

### [[Budget and Financial Planning]]

**Scenario:** A CFO projects next year's expenses based on historical spending patterns and growth trends.

**Implementation:**
```
Next Year Projection: =FORECAST(2027, AnnualExpenses, Year)
5-Year Projection: {=FORECAST(ROW(2027:2031), Expenses, Years)}
```

**Business Application:** Linear projection provides a baseline forecast for budgeting. The trend-based estimate serves as a starting point, which analysts then adjust for known changes (new initiatives, cost-cutting, market changes). Compare FORECAST projections to actual outcomes to calibrate future expectations.

**Technical Details:** Financial data often shows patterns beyond simple linear trends (seasonal cycles, growth rates that compound). For more sophisticated forecasting, consider FORECAST.ETS or logarithmic transformations. Always supplement point forecasts with uncertainty ranges.

---

### [[Resource Capacity Planning]]

**Scenario:** An operations manager estimates staffing needs based on projected demand using historical demand-to-headcount relationships.

**Implementation:**
```
=FORECAST(ProjectedDemand, HistoricalHeadcount, HistoricalDemand)
```

**Business Application:** Understanding the relationship between demand and required resources enables proactive capacity planning. FORECAST estimates how many staff are needed for a given demand level, supporting hiring timelines and workforce planning.

**Technical Details:** The relationship may not be perfectly linear (economies of scale, minimum staffing levels). Validate the linear assumption with scatter plots and R-squared analysis. Consider step-function models for capacity that changes in discrete increments.

---

### [[Pricing and Revenue Estimation]]

**Scenario:** A product manager estimates revenue at different price points to inform pricing strategy.

**Implementation:**
```
=FORECAST(ProposedPrice, HistoricalRevenue, HistoricalPrice) * ExpectedVolume
```

**Business Application:** If historical data shows how price affects demand and revenue, FORECAST can estimate outcomes at new price points. This supports price optimization decisions. Combine with margin analysis to find profit-maximizing prices.

**Technical Details:** Price-demand relationships are often non-linear (price elasticity varies). Log-linear models may be more appropriate. Be cautious about extrapolating beyond tested price ranges; consumer behavior may change unexpectedly.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Status:** Legacy function since Excel 2016; FORECAST.LINEAR recommended
- **Behavior:** Identical to FORECAST.LINEAR
- **Maximum data:** Limited by worksheet size
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Status:** Fully supported (not marked as legacy)
- **Behavior:** Identical to Excel
- **Maximum data:** Limited by sheet size
- **Precision:** 15 significant decimal digits

### Key Difference Alert

FORECAST behaves identically between Excel and Google Sheets. The main difference is that Excel marks FORECAST as legacy since 2016, while Google Sheets treats it as a current function. Both platforms also support FORECAST.LINEAR.

## Tips and Best Practices

1. **Use FORECAST.LINEAR for new work:** While FORECAST still works, the newer name is clearer and part of a consistent forecasting function family.

2. **Remember the argument order:** FORECAST uses (x, y, x) - the prediction point comes first. This differs from SLOPE and INTERCEPT which use (y, x).

3. **Add uncertainty bounds:** A point forecast without error range creates false confidence. Use STEYX to build approximate prediction intervals: FORECAST +/- 2*STEYX for roughly 95% bounds.

4. **Do not over-extrapolate:** Predictions become less reliable as X moves further from your historical data. Stay within reasonable distance of observed values.

5. **Check the relationship first:** Before forecasting, verify the linear relationship is reasonable using CORREL or scatter plots. Non-linear relationships require different approaches.

6. **Use for single predictions:** For multiple predictions at once, TREND is more efficient. FORECAST is best for one-off point predictions.

7. **Document assumptions:** Forecasts assume the historical relationship continues unchanged. Note when this assumption may not hold (market changes, disruptions, etc.).

8. **Track forecast accuracy:** Compare predictions to actual outcomes over time. Systematic errors indicate the model needs adjustment.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FORECAST.LINEAR]] | Modern replacement (identical) | For new work; clearer naming |
| [[TREND]] | Array of predictions | When you need multiple predictions |
| [[FORECAST.ETS]] | Time series with seasonality | For seasonal or complex time series |
| [[GROWTH]] | Exponential prediction | When growth is percentage-based |

### Commonly Used Together

**[[STEYX]]** - Standard error of estimate

*Prediction interval:*
```
Point: =FORECAST(NewX, Y, X)
Lower: =FORECAST(NewX, Y, X) - 2*STEYX(Y, X)
Upper: =FORECAST(NewX, Y, X) + 2*STEYX(Y, X)
```
Adds uncertainty bounds to point forecast.

---

**[[RSQ]]** - Coefficient of determination

*Assess forecast reliability:*
```
Forecast: =FORECAST(NewX, Y, X)
Fit: =RSQ(Y, X)
```
Higher R-squared suggests more reliable forecasts.

---

**[[SLOPE]] and [[INTERCEPT]]** - Regression coefficients

*Understand the underlying model:*
```
Forecast: =FORECAST(NewX, Y, X)
Equation: Y = INTERCEPT(Y,X) + SLOPE(Y,X) * X
```
See what equation FORECAST is applying.

## Official Documentation

- **Microsoft Excel:** [FORECAST function](https://support.microsoft.com/en-us/office/forecast-function-50ca49c9-7b40-4892-94e4-7ad38bbeda99)
- **Google Sheets:** [FORECAST function](https://support.google.com/docs/answer/3094000)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2007 | Continued support | Increased capacity |
| Excel 2016 | Marked as legacy | FORECAST.LINEAR introduced |
| Excel 365 | Backward compatible | Still works, .LINEAR preferred |
| Google Sheets | Original launch (2006) | Fully supported |

---

*Last updated: 2026-01-10*
