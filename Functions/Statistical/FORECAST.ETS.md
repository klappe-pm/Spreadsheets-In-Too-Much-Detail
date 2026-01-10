---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- statistical
subTopics:
- forecasting
- time-series
- exponential-smoothing
- seasonality
- predictive-analytics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ETS Forecast
- Exponential Triple Smoothing
- Seasonal Forecast
- AAA Forecast
tags:
- statistical
- forecasting
- time-series
- exponential-smoothing
- seasonality
- prediction
---

# FORECAST.ETS

## Description

**FORECAST.ETS** calculates predicted values using the Exponential Triple Smoothing (ETS) algorithm, also known as Holt-Winters forecasting. This sophisticated function automatically detects and models seasonality, trends, and level components in your time-series data to produce forecasts. Unlike simple linear regression, FORECAST.ETS excels at handling data with repeating patterns (weekly, monthly, quarterly cycles) and non-linear trends.

The function employs the AAA variant of exponential smoothing by default: Additive error, Additive trend, and Additive seasonality. It analyzes historical data to determine optimal smoothing parameters (alpha, beta, gamma) and seasonal patterns, then applies these to project future values. The algorithm is particularly powerful for business forecasting where regular patterns (holiday sales spikes, seasonal demand cycles) significantly influence predictions.

**Key capabilities:** FORECAST.ETS automatically detects the seasonality period from your data if not specified, handles missing data points through interpolation, and can work with or without seasonal components. The function processes data with irregular timestamps and can aggregate multiple values at the same time point.

**Platform differences:** This is an **Excel-only function** - Google Sheets does not support FORECAST.ETS or its related functions (FORECAST.ETS.CONFINT, FORECAST.ETS.SEASONALITY, FORECAST.ETS.STAT). Excel introduced this function in Excel 2016. For Google Sheets users, alternative approaches include using FORECAST.LINEAR combined with manual seasonal adjustments.

## Syntax

> [!f(x)] FORECAST.ETS Syntax
>
> ```
> =FORECAST.ETS(target_date, values, timeline, [seasonality], [data_completion], [aggregation])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `target_date` | Yes | The date/time point for which to forecast. Can be a single value or an array for multiple forecasts. Must be in the same format/units as timeline. |
| `values` | Yes | Historical values (dependent variable). Must be a continuous range or array of numeric values representing your time-series data. |
| `timeline` | Yes | Historical dates/times (independent variable). Must be same length as values. Should be equally spaced. |
| `seasonality` | No | The seasonal period length. 0 = no seasonality, 1 = auto-detect (default), or specify number of periods (e.g., 12 for monthly data with yearly cycle). |
| `data_completion` | No | How to handle missing timeline values. 0 = treat as zeros, 1 = interpolate (default). |
| `aggregation` | No | How to aggregate multiple values at same time point. 1=AVERAGE (default), 2=COUNT, 3=COUNTA, 4=MAX, 5=MEDIAN, 6=MIN, 7=SUM. |

### Return Value

Returns a numeric value representing the forecasted value at the target date. Can return an array of values if target_date is an array. Returns #VALUE! if timeline and values have different lengths. Returns #NUM! if less than 2 complete seasons of data are available.

## Examples

> [!f(x)] FORECAST.ETS Examples

### Example 1: Basic Forecast with Auto-Detected Seasonality
```
=FORECAST.ETS(DATE(2026,1,1), B2:B25, A2:A25)
```
**Result:** Forecasted value for January 1, 2026

**Explanation:** Uses 24 months of historical data to forecast January 2026. Seasonality is auto-detected from data patterns.

---

### Example 2: Specifying Monthly Seasonality
```
=FORECAST.ETS(DATE(2026,3,1), Sales, Months, 12)
```
**Result:** Forecast with explicit 12-month seasonal pattern

**Explanation:** Forces the model to use 12-period (annual) seasonality, appropriate for monthly data with yearly cycles.

---

### Example 3: No Seasonality (Trend Only)
```
=FORECAST.ETS(A26, B2:B25, A2:A25, 0)
```
**Result:** Trend-based forecast without seasonal adjustment

**Explanation:** Setting seasonality to 0 disables seasonal modeling, producing a forecast based only on trend and level.

---

### Example 4: Quarterly Data with Annual Pattern
```
=FORECAST.ETS(DATE(2026,4,1), QuarterlySales, QuarterDates, 4)
```
**Result:** Q2 2026 forecast

**Explanation:** For quarterly data with annual seasonality, specify 4 as the seasonal period.

---

### Example 5: Weekly Data with Annual Seasonality
```
=FORECAST.ETS(DATE(2026,1,15), WeeklySales, WeekDates, 52)
```
**Result:** Forecast incorporating 52-week seasonal pattern

**Explanation:** Weekly data with annual patterns requires seasonality of 52. Needs at least 104 weeks (2 years) of data.

---

### Example 6: Daily Data with Weekly Pattern
```
=FORECAST.ETS(DATE(2026,1,10), DailyTraffic, Days, 7)
```
**Result:** Forecast accounting for day-of-week patterns

**Explanation:** For daily data with clear weekly patterns, use 7-period seasonality.

---

### Example 7: Multiple Future Dates (Array Formula)
```
=FORECAST.ETS(FutureDates, HistoricalValues, HistoricalDates, 12)
```
**Result:** Array of forecasted values for each date

**Explanation:** In Excel 365, this spills results for all target dates.

---

### Example 8: Handling Missing Data with Interpolation
```
=FORECAST.ETS(TARGET, Values, Timeline, 1, 1)
```
**Result:** Forecast with interpolated missing values

**Explanation:** The fifth parameter (1) specifies that missing timeline points should be filled via interpolation.

---

### Example 9: Forecast Next 6 Months
```
=FORECAST.ETS(EDATE(MAX(Dates),ROW(A1)), Values, Dates, 12)
```
**Result:** Rolling monthly forecast (drag down for 6 rows)

**Explanation:** EDATE generates future months relative to the last historical date.

---

### Example 10: Aggregating Duplicate Timestamps
```
=FORECAST.ETS(TargetDate, Values, Timeline, 1, 1, 7)
```
**Result:** Forecast using SUM aggregation for duplicates

**Explanation:** The sixth parameter specifies how to aggregate duplicate dates (7=SUM).

---

### Example 11: Complete Forecasting Template
```
Historical Data: A2:A37 (3 years monthly dates), B2:B37 (values)
Forecast: =FORECAST.ETS(C2:C13, B2:B37, A2:A37, 12, 1, 1)
```
**Result:** Full year forecast array

**Explanation:** With 36 months of history and explicit 12-month seasonality, this produces reliable forecasts.

---

### Example 12: Comparing Auto vs. Specified Seasonality
```
Auto: =FORECAST.ETS(Target, Values, Dates, 1)
Manual 12: =FORECAST.ETS(Target, Values, Dates, 12)
```
**Result:** Different forecasts based on seasonality assumption

**Explanation:** Comparing forecasts with different seasonality settings helps validate which pattern best fits your data.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Values and timeline arrays have different lengths | Ensure both arrays have exactly the same number of elements. |
| `#NUM!` | Insufficient data for seasonal model | Need at least 2 complete seasonal cycles. For seasonality=12, need 24+ data points. |
| `#NUM!` | Timeline not properly ordered or spaced | Timeline should be chronologically ordered with consistent spacing. |
| `#NAME?` | Function not recognized | FORECAST.ETS requires Excel 2016 or later. Not available in Google Sheets. |
| `Unreasonable forecast` | Auto-detected wrong seasonality | Specify seasonality explicitly. Verify with FORECAST.ETS.SEASONALITY. |

## Use Cases

### [[Retail Sales Forecasting]]

**Scenario:** A retail manager needs to forecast monthly sales for the next year to plan inventory and staffing.

**Implementation:**
```
Historical Sales: 3+ years of monthly data
Forecast: =FORECAST.ETS(FutureMonth, MonthlySales, MonthDates, 12, 1, 1)
Confidence: =FORECAST.ETS.CONFINT(FutureMonth, MonthlySales, MonthDates, 0.95, 12)
```

**Business Application:** Enables inventory optimization by anticipating peak demand periods. Reduces stockouts during high-season.

**Technical Details:** Retail data often benefits from explicit 12-month seasonality due to strong holiday effects.

---

### [[Budget and Financial Planning]]

**Scenario:** A finance team needs to project quarterly revenue for the next fiscal year.

**Implementation:**
```
Revenue History: 12+ quarters
Quarterly Forecast: =FORECAST.ETS(FutureQuarter, QuarterlyRevenue, QuarterDates, 4)
```

**Business Application:** Creates data-driven budget projections that account for seasonal revenue patterns.

**Technical Details:** For quarterly data, ensure at least 8 quarters (2 years) for seasonal model.

---

### [[Demand Planning]]

**Scenario:** A supply chain analyst forecasts product demand to optimize manufacturing schedules.

**Implementation:**
```
Weekly Demand: 104+ weeks of data
Forecast: =FORECAST.ETS(FutureWeek, DemandHistory, WeekDates, 52)
```

**Business Application:** Synchronizes supply with anticipated demand, reducing inventory costs.

**Technical Details:** Weekly data with annual seasonality (52 periods) requires substantial history.

---

### [[Website Traffic Prediction]]

**Scenario:** A marketing team forecasts website traffic to plan server capacity.

**Implementation:**
```
Daily Traffic: Last 2+ years
Forecast: =FORECAST.ETS(FutureDate, DailyVisits, Dates, 7)
```

**Business Application:** Ensures adequate server capacity during predicted traffic spikes.

**Technical Details:** Daily data may have weekly patterns (7-period seasonality).

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2016 and later
- **Full support:** All parameters functional
- **Array output:** Excel 365 supports dynamic arrays

### Google Sheets
- **Availability:** NOT SUPPORTED
- **Alternative:** Use FORECAST.LINEAR for simple forecasting
- **Workaround:** Manual seasonal adjustments or add-ons

### Key Difference Alert
**CRITICAL: FORECAST.ETS is Excel-only.** Google Sheets does not support this function.

## Tips and Best Practices

1. **Ensure sufficient historical data:** Provide at least 2 complete seasonal cycles for reliable forecasts.

2. **Validate auto-detected seasonality:** Use FORECAST.ETS.SEASONALITY to verify the detected pattern.

3. **Check forecast reasonableness:** Sanity-check forecasts against business knowledge.

4. **Use confidence intervals:** Combine with FORECAST.ETS.CONFINT to understand uncertainty.

5. **Maintain consistent timeline spacing:** Works best with regular intervals.

6. **Test with holdout data:** Validate by forecasting recent known data.

7. **Document your model:** Record seasonality settings for reproducibility.

8. **Consider model statistics:** Use FORECAST.ETS.STAT to evaluate model quality.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FORECAST.LINEAR]] | Linear trend forecasting | When data has no seasonality |
| [[FORECAST]] | Legacy linear forecast | Same as FORECAST.LINEAR |
| [[TREND]] | Multiple linear regression | For multiple predictors |
| [[GROWTH]] | Exponential growth projection | For multiplicative trends |

### Commonly Used Together

**[[FORECAST.ETS.CONFINT]]** - Confidence intervals
```
Margin: =FORECAST.ETS.CONFINT(Target, Values, Timeline, 0.95, 12)
```

---

**[[FORECAST.ETS.SEASONALITY]]** - Detected seasonality
```
Period: =FORECAST.ETS.SEASONALITY(Values, Timeline)
```

---

**[[FORECAST.ETS.STAT]]** - Model statistics
```
MASE: =FORECAST.ETS.STAT(Values, Timeline, 4, 12)
```

---

**[[EDATE]]** - Future date generation
```
=FORECAST.ETS(EDATE(LastDate, 1), Values, Dates, 12)
```

## Official Documentation

- **Microsoft Excel:** [FORECAST.ETS function](https://support.microsoft.com/en-us/office/forecast-ets-function-15389b8b-677e-4fbd-bd95-21d464333f41)
- **Google Sheets:** Not available

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2016 | Introduced with enhanced forecasting |
| Excel 365 | Continuous updates | Dynamic array support |
| Google Sheets | Not available | No plans announced |

---

*Last updated: 2026-01-10*
