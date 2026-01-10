---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- statistical
subTopics:
- forecasting
- confidence-intervals
- uncertainty
- time-series
- prediction-intervals
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ETS Confidence Interval
- Forecast Uncertainty
- Prediction Interval
- ETS Error Bounds
tags:
- statistical
- forecasting
- confidence-interval
- uncertainty
- time-series
- prediction
---

# FORECAST.ETS.CONFINT

## Description

**FORECAST.ETS.CONFINT** returns the confidence interval width for a forecasted value at a specified target date using the Exponential Triple Smoothing (ETS) algorithm. This function quantifies the uncertainty in your forecasts, providing the margin of error that, when added to and subtracted from the point forecast, creates a prediction interval within which future values are likely to fall.

The function calculates uncertainty by analyzing the historical prediction errors from the ETS model and projecting how that uncertainty grows over the forecast horizon. Forecast uncertainty typically increases the further you project into the future, so confidence intervals widen as the target date moves further from your historical data.

**Critical interpretation:** The returned value is the HALF-WIDTH of the confidence interval (the distance from the forecast to each bound), not the full interval width. To construct the complete interval, add and subtract this value from the corresponding FORECAST.ETS point forecast.

**Platform availability:** This is an **Excel-only function** introduced in Excel 2016. Google Sheets does not support this function. For Google Sheets users, confidence intervals must be calculated manually using alternative approaches.

## Syntax

> [!f(x)] FORECAST.ETS.CONFINT Syntax
>
> ```
> =FORECAST.ETS.CONFINT(target_date, values, timeline, [confidence_level], [seasonality], [data_completion], [aggregation])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `target_date` | Yes | The date/time point for which to calculate the confidence interval. |
| `values` | Yes | Historical values. Same array used in FORECAST.ETS. |
| `timeline` | Yes | Historical dates/times. Must match FORECAST.ETS timeline. |
| `confidence_level` | No | Probability level (default 0.95 for 95%). Must be between 0 and 1. |
| `seasonality` | No | Seasonal period. Should match FORECAST.ETS. 0=none, 1=auto-detect, or specify. |
| `data_completion` | No | How to handle missing values. 0=zeros, 1=interpolate (default). |
| `aggregation` | No | How to aggregate duplicates. 1=AVERAGE (default), 2-7 for other methods. |

### Return Value

Returns a positive numeric value representing the half-width of the confidence interval. Add to FORECAST.ETS for upper bound; subtract for lower bound. Returns #NUM! if confidence level is outside (0,1) or insufficient data.

## Examples

> [!f(x)] FORECAST.ETS.CONFINT Examples

### Example 1: Basic 95% Confidence Interval
```
Forecast: =FORECAST.ETS(DATE(2026,1,1), B2:B25, A2:A25, 12)
Margin: =FORECAST.ETS.CONFINT(DATE(2026,1,1), B2:B25, A2:A25, 0.95, 12)
```
**Result:** Margin of error for 95% confidence

**Explanation:** The margin creates an interval expected to contain the true value 95% of the time.

---

### Example 2: Constructing Complete Interval
```
Forecast: =FORECAST.ETS(A26, Sales, Dates, 12)
Margin: =FORECAST.ETS.CONFINT(A26, Sales, Dates, 0.95, 12)
Lower: =Forecast - Margin
Upper: =Forecast + Margin
```
**Result:** Complete [Lower, Upper] bounds

**Explanation:** Add and subtract margin from point forecast to get full interval.

---

### Example 3: 99% vs 95% Confidence Levels
```
95% Margin: =FORECAST.ETS.CONFINT(Target, Values, Timeline, 0.95, 12)
99% Margin: =FORECAST.ETS.CONFINT(Target, Values, Timeline, 0.99, 12)
```
**Result:** 99% margin is larger

**Explanation:** Higher confidence requires wider intervals.

---

### Example 4: Uncertainty Growth Over Time
```
1 Month: =FORECAST.ETS.CONFINT(EDATE(LastDate,1), Values, Dates, 0.95, 12)
6 Months: =FORECAST.ETS.CONFINT(EDATE(LastDate,6), Values, Dates, 0.95, 12)
```
**Result:** Larger margins for further horizons

**Explanation:** Forecasts become less certain over time.

---

### Example 5: Conservative Revenue Planning
```
Revenue: =FORECAST.ETS(FutureDate, RevenueHistory, Dates, 12)
Conservative: =Revenue - FORECAST.ETS.CONFINT(FutureDate, RevenueHistory, Dates, 0.95, 12)
```
**Result:** Lower bound of expected revenue

**Explanation:** Use lower bound for conservative budget planning.

---

### Example 6: Risk-Adjusted Cost Planning
```
Cost: =FORECAST.ETS(FutureDate, CostHistory, Dates, 12)
Risk_Adjusted: =Cost + FORECAST.ETS.CONFINT(FutureDate, CostHistory, Dates, 0.95, 12)
```
**Result:** Upper bound of expected costs

**Explanation:** Budget for upper bound to protect against overruns.

---

### Example 7: Multiple Target Dates
```
=FORECAST.ETS.CONFINT(FutureDateRange, Values, Timeline, 0.95, 12)
```
**Result:** Array of confidence intervals

**Explanation:** Excel 365 spills margins for all target dates.

---

### Example 8: 90% Confidence
```
=FORECAST.ETS.CONFINT(Target, Values, Timeline, 0.90, 12)
```
**Result:** Narrower interval than 95%

**Explanation:** Lower confidence = narrower interval = less certainty.

---

### Example 9: No Seasonality
```
=FORECAST.ETS.CONFINT(Target, Values, Timeline, 0.95, 0)
```
**Result:** Interval for trend-only model

**Explanation:** Use seasonality=0 for non-seasonal data.

---

### Example 10: Complete Dashboard
```
Point: =FORECAST.ETS(A2, Values, Dates, 12)
Margin: =FORECAST.ETS.CONFINT(A2, Values, Dates, 0.95, 12)
Lower: =B2-C2
Upper: =B2+C2
Uncertainty: =C2/B2*100 & "%"
```
**Result:** Comprehensive forecast view

**Explanation:** Shows forecast with bounds and uncertainty percentage.

---

### Example 11: Comparing Models
```
With_Season: =FORECAST.ETS.CONFINT(Target, Values, Dates, 0.95, 12)
No_Season: =FORECAST.ETS.CONFINT(Target, Values, Dates, 0.95, 0)
```
**Result:** Different interval widths

**Explanation:** Better-fitting model typically has narrower intervals.

---

### Example 12: Auto-Detected Seasonality
```
=FORECAST.ETS.CONFINT(Target, Values, Timeline, 0.95, 1)
```
**Result:** Uses auto-detected seasonal period

**Explanation:** Seasonality=1 enables auto-detection.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Arrays have different lengths | Ensure values and timeline match. |
| `#NUM!` | Confidence level outside (0,1) | Use 0.95, not 95. |
| `#NUM!` | Insufficient data | Need 2+ complete seasonal cycles. |
| `#NAME?` | Function not recognized | Requires Excel 2016+. Not in Google Sheets. |

## Use Cases

### [[Budget Planning with Uncertainty]]

**Scenario:** CFO needs budget reserves accounting for forecast uncertainty.

**Implementation:**
```
Revenue_Forecast: =FORECAST.ETS(Month, RevenueHistory, Dates, 12)
Revenue_Lower: =Revenue_Forecast - FORECAST.ETS.CONFINT(Month, RevenueHistory, Dates, 0.95, 12)
Cost_Upper: =Cost_Forecast + FORECAST.ETS.CONFINT(Month, CostHistory, Dates, 0.95, 12)
```

**Business Application:** Creates realistic budget acknowledging uncertainty.

**Technical Details:** Using lower revenue bound and upper cost bound creates "worst reasonable case."

---

### [[Inventory Safety Stock]]

**Scenario:** Supply chain manager determines safety stock levels based on demand uncertainty.

**Implementation:**
```
Demand: =FORECAST.ETS(ReplenishDate, DemandHistory, Dates, 12)
Uncertainty: =FORECAST.ETS.CONFINT(ReplenishDate, DemandHistory, Dates, 0.95, 12)
Safety_Stock: =Uncertainty * Service_Level_Factor
```

**Business Application:** Optimizes inventory based on actual forecast uncertainty.

**Technical Details:** Safety stock scales with confidence interval width.

---

### [[Scenario Planning]]

**Scenario:** Executive team needs range of possible outcomes for strategic decisions.

**Implementation:**
```
Base: =FORECAST.ETS(Target, History, Timeline, 12)
Margin: =FORECAST.ETS.CONFINT(Target, History, Timeline, 0.95, 12)
Optimistic: =Base + Margin
Pessimistic: =Base - Margin
```

**Business Application:** Supports robust decision-making with multiple scenarios.

**Technical Details:** 95% interval captures most reasonable outcomes.

---

### [[Forecast Accuracy Reporting]]

**Scenario:** Demand planner reports forecast reliability to stakeholders.

**Implementation:**
```
Forecast: =FORECAST.ETS(Target, Data, Dates, 12)
Range: =FORECAST.ETS.CONFINT(Target, Data, Dates, 0.95, 12)
Report: ="Expect " & Forecast & " +/- " & Range
```

**Business Application:** Sets appropriate expectations about forecast precision.

**Technical Details:** Report intervals to prevent over-reliance on point forecasts.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2016 and later
- **Full support:** All parameters functional

### Google Sheets
- **Availability:** NOT SUPPORTED
- **Alternative:** Manual prediction interval calculations

### Key Difference Alert
**CRITICAL: FORECAST.ETS.CONFINT is Excel-only.**

## Tips and Best Practices

1. **Always report intervals with forecasts:** Point forecasts alone are incomplete.

2. **Match parameters with FORECAST.ETS:** Use identical settings in both functions.

3. **Understand interval growth:** Intervals widen for longer horizons.

4. **Choose appropriate confidence levels:** 95% is standard; adjust based on risk tolerance.

5. **Use intervals for decisions:** Plan for upper bound costs, lower bound revenues.

6. **Validate empirically:** Check if actuals fall within intervals at expected rate.

7. **Document confidence level:** Clearly state the probability used.

8. **Consider asymmetric risks:** Emphasize relevant bound based on business context.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CONFIDENCE.T]] | T-distribution interval for mean | For sample mean confidence |
| [[FORECAST.ETS]] | Point forecast | For actual predictions |
| [[FORECAST.ETS.STAT]] | Model statistics | For accuracy metrics |

### Commonly Used Together

**[[FORECAST.ETS]]** - Point forecast
```
Lower: =FORECAST.ETS(...) - FORECAST.ETS.CONFINT(...)
Upper: =FORECAST.ETS(...) + FORECAST.ETS.CONFINT(...)
```

---

**[[FORECAST.ETS.STAT]]** - Model accuracy
```
MASE: =FORECAST.ETS.STAT(Values, Dates, 4, 12)
```

## Official Documentation

- **Microsoft Excel:** [FORECAST.ETS.CONFINT function](https://support.microsoft.com/en-us/office/forecast-ets-confint-function-6a15159d-f3d6-4d08-8a62-3c23e9af8a48)
- **Google Sheets:** Not available

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2016 | Introduced with FORECAST.ETS family |
| Excel 365 | Continuous updates | Dynamic array support |
| Google Sheets | Not available | No plans announced |

---

*Last updated: 2026-01-10*
