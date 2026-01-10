---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- standard-error
- regression-analysis
- prediction-accuracy
- residual-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Standard Error of Estimate
- Standard Error of Y Estimate
- SEE
- Root Mean Square Error
tags:
- statistical
- regression
- standard-error
- prediction
- residuals
---

# STEYX

## Description

**STEYX** calculates the standard error of the predicted Y values for each X in a linear regression, measuring the typical distance between actual data points and the regression line. While R-squared tells you what proportion of variance is explained, STEYX tells you the magnitude of prediction errors in the original units of Y. If Y is sales in dollars and STEYX returns 500, your predictions are typically off by about $500. This practical, interpretable measure of model accuracy is essential for understanding how reliable your regression predictions actually are.

The standard error of the estimate (also called standard error of the regression or root mean square error) is calculated from the residuals, which are the differences between actual Y values and predicted Y values from the regression line. STEYX computes the root mean square of these residuals, adjusted for degrees of freedom: STEYX = sqrt(sum((y_actual - y_predicted)^2) / (n-2)). The n-2 adjustment accounts for the two parameters estimated (slope and intercept), leaving valid degrees of freedom for inference.

STEYX relates to R-squared through the variance decomposition: STEYX^2 * (n-2) = (1 - RSQ) * sum((y - mean_y)^2). When R-squared is high, STEYX is low (predictions are close to actual values). When R-squared is low, STEYX is high (predictions deviate substantially). However, STEYX provides additional information: it is in the same units as Y, making it directly interpretable for decision-making. R-squared of 0.90 sounds good, but is the typical error of $50 or $50,000? STEYX answers this critical question.

This statistic is fundamental for constructing prediction intervals and assessing forecast reliability. A point prediction without uncertainty bounds is incomplete for many business decisions. STEYX forms the basis for confidence intervals around predictions, helping distinguish between situations where predictions should be trusted versus those requiring caution due to large uncertainty.

## Syntax

> [!f(x)] STEYX Syntax
>
> ```
> =STEYX(known_y's, known_x's)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `known_y's` | Yes | The dependent variable data (actual Y values). Can be a cell range, named range, or array of numeric values. |
| `known_x's` | Yes | The independent variable data (the predictor). Must have the same number of data points as known_y's. |

### Return Value

Returns a non-negative numeric value representing the standard error of the estimated Y values. Returns #N/A if arrays have different numbers of numeric data points. Returns #DIV/0! if fewer than 3 data points (need n>2 for degrees of freedom).

## Examples

> [!f(x)] STEYX Examples

### Example 1: Basic Standard Error Calculation
```
=STEYX(B2:B11, A2:A11)
```
Where A2:A11 (X - Advertising $K): 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
And B2:B11 (Y - Sales $K): 25, 32, 35, 42, 48, 51, 55, 62, 68, 75

**Result:** 2.3 (approximately)

**Explanation:** Predictions from this regression model are typically off by about $2,300. This represents the "noise" around the regression line.

---

### Example 2: Comparing Models by Prediction Error
```
Model 1: =STEYX(Sales, Price)
Model 2: =STEYX(Sales, Temperature)
```
**Result:** Lower STEYX indicates better predictions

**Explanation:** If Price-based predictions have STEYX of 500 and Temperature-based have STEYX of 800, the Price model predicts more accurately.

---

### Example 3: Relationship to R-Squared
```
STEYX: =STEYX(Y, X)
R-squared: =RSQ(Y, X)
Y variance: =VAR.S(Y)
Verification: =SQRT((1-RSQ(Y,X)) * VAR.S(Y) * (COUNT(Y)-1) / (COUNT(Y)-2))
```
**Result:** Verification formula equals STEYX

**Explanation:** STEYX measures unexplained standard deviation. Higher R-squared means lower STEYX and vice versa.

---

### Example 4: Creating Prediction Interval
```
Prediction: =INTERCEPT(Y, X) + SLOPE(Y, X) * NewX
Lower bound: =Prediction - 2 * STEYX(Y, X)
Upper bound: =Prediction + 2 * STEYX(Y, X)
```
**Result:** Approximate 95% prediction interval

**Explanation:** For rough prediction intervals, use +/- 2 standard errors. For precise intervals, more complex calculations are needed that account for distance from the mean.

---

### Example 5: Assessing Forecast Reliability
```
=STEYX(HistoricalSales, TimeIndex)
```
**Result:** 15000 (example value)

**Explanation:** Historical predictions deviate from actual sales by about $15,000 typically. Use this to set realistic expectations for future forecast accuracy.

---

### Example 6: Coefficient of Variation for Predictions
```
=STEYX(Y, X) / AVERAGE(Y) * 100
```
**Result:** Prediction error as percentage of average Y

**Explanation:** If average Y is 100 and STEYX is 5, predictions are typically off by about 5%. This relative measure allows comparison across different scales.

---

### Example 7: Minimum Data Requirement
```
=STEYX(A2:A3, B2:B3)  // Only 2 points
```
**Result:** #DIV/0!

**Explanation:** STEYX requires at least 3 data points (divides by n-2). With only 2 points, the line passes exactly through both, so there is no residual variance to measure.

---

### Example 8: Perfect Fit
```
=STEYX(B2:B6, A2:A6)
```
Where Y = 2X exactly (no scatter)

**Result:** 0

**Explanation:** When all points fall exactly on the regression line, there are no prediction errors, so STEYX equals zero. This is rare in real data.

---

### Example 9: Impact of Outliers
```
Normal: =STEYX(Y, X)
With Outlier: =STEYX(Y_with_outlier, X)
```
**Result:** STEYX increases substantially with outliers

**Explanation:** Outliers create large residuals that inflate STEYX. One extreme observation can double the standard error, making the model appear less accurate.

---

### Example 10: Quality Control Application
```
=STEYX(MeasuredValues, TargetValues)
```
**Result:** Typical deviation from target

**Explanation:** In quality control, STEYX quantifies typical deviation from specifications. Compare to tolerance limits to assess process capability.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Arrays have different numbers of numeric data points | Ensure both ranges have equal length and matching observations |
| `#DIV/0!` | Fewer than 3 data points | STEYX needs at least 3 points (n-2 degrees of freedom) |
| `#VALUE!` | Non-numeric text provided directly as argument | Use cell references; direct text causes errors |
| `Zero result` | Perfect linear relationship (all points on line) | This is correct; no prediction error exists |
| `Unexpectedly large` | Outliers or weak relationship | Plot data; investigate extreme values; check R-squared |

## Use Cases

### [[Forecast Confidence Assessment]]

**Scenario:** A sales manager builds confidence intervals around revenue forecasts to communicate uncertainty to leadership.

**Implementation:**
```
Point Forecast: =INTERCEPT(HistoricalRevenue, Quarter) + SLOPE(HistoricalRevenue, Quarter) * FutureQuarter
Standard Error: =STEYX(HistoricalRevenue, Quarter)
Confidence Range: =FORECAST +/- 1.96 * STEYX for 95% interval
```

**Business Application:** Point forecasts without uncertainty bounds create false precision. STEYX enables honest communication: "We forecast $5M revenue, but historical accuracy suggests actual results typically fall within +/- $400K." This supports better decision-making and prevents overcommitment based on uncertain projections.

**Technical Details:** The simple +/- STEYX interval is approximate. Proper prediction intervals are wider for X values far from the mean and require accounting for estimation uncertainty in slope and intercept. For formal forecasting, use more sophisticated interval calculations.

---

### [[Model Selection and Validation]]

**Scenario:** An analyst compares different predictive models to identify which provides the most accurate forecasts.

**Implementation:**
```
Model A: =STEYX(Actual, Predicted_ModelA)
Model B: =STEYX(Actual, Predicted_ModelB)
Model C: =STEYX(Actual, Predicted_ModelC)
```
Select model with lowest STEYX.

**Business Application:** When multiple models are candidates (different predictors, different time windows, different methodologies), STEYX provides an objective comparison in meaningful units. Lower STEYX means better predictions. Combine with cross-validation to prevent overfitting.

**Technical Details:** Compare STEYX on held-out test data, not training data. Models can have low STEYX on training data but high STEYX on new data (overfitting). The improvement from adding complexity should justify the added STEYX reduction.

---

### [[Process Capability Analysis]]

**Scenario:** A quality engineer assesses whether manufacturing process variation falls within acceptable specification limits.

**Implementation:**
```
Process Standard Error: =STEYX(MeasuredOutput, ProcessInput)
Specification Width: =UpperSpec - LowerSpec
Capability Ratio: =SpecificationWidth / (6 * STEYX)
```

**Business Application:** STEYX represents the typical deviation of output from predicted values given input settings. Comparing this to specification limits indicates whether the process is capable of meeting quality requirements. Ratio > 1 suggests the process is capable; ratio < 1 indicates excessive variation.

**Technical Details:** This simplified analysis assumes residuals are normally distributed. Formal capability analysis (Cp, Cpk) requires additional considerations including process centering. Monitor STEYX over time to detect process drift.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Minimum data points:** Requires at least 3 (n-2 > 0)
- **Maximum data points:** Limited by worksheet size
- **Precision:** 15 significant decimal digits
- **Empty cell handling:** Pairs with non-numeric values excluded

### Google Sheets

- **Availability:** All versions since launch
- **Minimum data points:** Requires at least 3
- **Maximum data points:** Limited by sheet size
- **Precision:** 15 significant decimal digits
- **Empty cell handling:** Same as Excel

### Key Difference Alert

STEYX behaves identically between Excel and Google Sheets. Both use the same formula and produce identical results. No practical differences exist for standard use cases.

## Tips and Best Practices

1. **Interpret in Y's units:** STEYX is measured in the same units as Y. If Y is dollars, STEYX is dollars. If Y is percentage points, STEYX is percentage points. This makes interpretation intuitive.

2. **Compare to Y magnitude:** A STEYX of 10 is small if Y averages 1000 but large if Y averages 20. Calculate relative error (STEYX/mean_Y) for context.

3. **Use for rough prediction intervals:** For approximate 95% intervals, use prediction +/- 2*STEYX. For 68% intervals, use +/- 1*STEYX. These assume normal residuals.

4. **Lower STEYX is better:** When comparing models, lower STEYX indicates more accurate predictions. Combine with R-squared for complete assessment.

5. **Watch for outliers:** A single extreme observation can substantially inflate STEYX. Plot residuals to identify influential points and decide whether they are errors or valid data.

6. **Requires at least 3 points:** STEYX divides by n-2, so you need at least 3 data points. More data provides more reliable estimates.

7. **Complements R-squared:** R-squared tells you proportion explained; STEYX tells you absolute error magnitude. Both are needed for complete model assessment.

8. **Consider for sample size planning:** Target a maximum acceptable STEYX and work backward to determine required sample size. Larger samples generally reduce STEYX.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[RSQ]] | Coefficient of determination (proportion explained) | When you need proportion rather than absolute error |
| [[LINEST]] | Complete regression statistics | When you need all statistics including STEYX |
| [[STDEV.S]] | Standard deviation of Y | For overall Y variability, not residual variability |

### Commonly Used Together

**[[FORECAST.LINEAR]]** - Predicted Y value

*Prediction with uncertainty:*
```
Point: =FORECAST.LINEAR(NewX, Y, X)
Error: =STEYX(Y, X)
Range: =FORECAST.LINEAR(NewX, Y, X) & " +/- " & STEYX(Y, X)
```
STEYX quantifies uncertainty around the point forecast.

---

**[[RSQ]]** - Coefficient of determination

*Complementary fit measures:*
```
Proportion: =RSQ(Y, X)
Magnitude: =STEYX(Y, X)
```
RSQ tells you "how much explained"; STEYX tells you "how far off."

---

**[[SLOPE]] and [[INTERCEPT]]** - Regression parameters

*Complete regression output:*
```
Slope: =SLOPE(Y, X)
Intercept: =INTERCEPT(Y, X)
Standard Error: =STEYX(Y, X)
```
These three values summarize the regression model.

---

**[[LINEST]]** - Full regression statistics

*Get standard error from array function:*
```
=LINEST(Y, X, TRUE, TRUE)
```
Returns STEYX equivalent in position [3,2] of the output array.

## Official Documentation

- **Microsoft Excel:** [STEYX function](https://support.microsoft.com/en-us/office/steyx-function-6ce74b2c-449d-4a6e-b9ac-f9cef5ba48ab)
- **Google Sheets:** [STEYX function](https://support.google.com/docs/answer/3094094)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2007 | Continued support | Increased data capacity |
| Excel 2010+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
