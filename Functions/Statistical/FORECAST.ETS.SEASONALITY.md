---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- statistical
subTopics:
- forecasting
- seasonality-detection
- time-series
- pattern-recognition
- cycle-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ETS Seasonality
- Seasonal Period Detection
- Cycle Length
- Pattern Detection
tags:
- statistical
- forecasting
- seasonality
- time-series
- pattern-detection
---

# FORECAST.ETS.SEASONALITY

## Description

**FORECAST.ETS.SEASONALITY** returns the length of the seasonal pattern that the ETS algorithm detects in your time-series data. This function reveals what repeating cycle the FORECAST.ETS function identifies when you use auto-detection (seasonality parameter = 1), helping you verify that the algorithm found the expected pattern.

The function analyzes historical data to identify the strongest repeating pattern, returning the number of periods in one complete seasonal cycle. For monthly data with annual patterns, it returns 12. For daily data with weekly patterns, it returns 7. If no significant pattern is found, it returns 1.

**Primary use case:** This function is diagnostic - use it to verify that FORECAST.ETS auto-detection found the seasonality you expect. If the result differs from expectations, you should override by explicitly setting the seasonality parameter in FORECAST.ETS.

**Platform availability:** This is an **Excel-only function** introduced in Excel 2016. Google Sheets does not support this function.

## Syntax

> [!f(x)] FORECAST.ETS.SEASONALITY Syntax
>
> ```
> =FORECAST.ETS.SEASONALITY(values, timeline, [data_completion], [aggregation])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `values` | Yes | Historical values. The same time-series data used in FORECAST.ETS. |
| `timeline` | Yes | Historical dates/times. Must be same length as values. |
| `data_completion` | No | How to handle missing values. 0=zeros, 1=interpolate (default). |
| `aggregation` | No | How to aggregate duplicates. 1=AVERAGE (default), 2-7 for other methods. |

### Return Value

Returns a positive integer representing the detected seasonal period. Returns 1 if no seasonality is detected. Returns #VALUE! if arrays have different lengths.

## Examples

> [!f(x)] FORECAST.ETS.SEASONALITY Examples

### Example 1: Basic Seasonality Detection
```
=FORECAST.ETS.SEASONALITY(B2:B37, A2:A37)
```
**Result:** 12 (for monthly data with annual pattern)

**Explanation:** Analyzes 36 months and detects annual seasonality.

---

### Example 2: Quarterly Data
```
=FORECAST.ETS.SEASONALITY(QuarterlySales, QuarterDates)
```
**Result:** 4

**Explanation:** Detects 4-period annual cycle in quarterly data.

---

### Example 3: No Seasonality Detected
```
=FORECAST.ETS.SEASONALITY(StockPrices, TradingDays)
```
**Result:** 1

**Explanation:** Stock prices typically lack predictable patterns; returns 1.

---

### Example 4: Weekly Pattern in Daily Data
```
=FORECAST.ETS.SEASONALITY(DailyTraffic, Days)
```
**Result:** 7

**Explanation:** Detects weekday/weekend pattern in daily data.

---

### Example 5: Unexpected Result
```
=FORECAST.ETS.SEASONALITY(MonthlySales, Months)
```
Expected: 12, Result: 6

**Explanation:** Algorithm found semi-annual pattern. Override with explicit seasonality=12 if annual is correct.

---

### Example 6: Validation Check
```
Detected: =FORECAST.ETS.SEASONALITY(Values, Dates)
Valid: =IF(Detected=12, "Correct", "Review needed")
```
**Result:** Automatic validation

**Explanation:** Compares detected vs. expected seasonality.

---

### Example 7: Use in FORECAST.ETS
```
Period: =FORECAST.ETS.SEASONALITY(Values, Dates)
Forecast: =FORECAST.ETS(Target, Values, Dates, Period)
```
**Result:** Forecast using detected period

**Explanation:** Feeds detected seasonality into forecast.

---

### Example 8: Documentation String
```
="Model uses " & FORECAST.ETS.SEASONALITY(Values, Dates) & "-period seasonality"
```
**Result:** "Model uses 12-period seasonality"

**Explanation:** Documents model assumptions for reporting.

---

### Example 9: Compare Multiple Series
```
Series A: =FORECAST.ETS.SEASONALITY(A_Values, Dates)
Series B: =FORECAST.ETS.SEASONALITY(B_Values, Dates)
```
**Result:** Different patterns per series

**Explanation:** Different products may have different seasonal patterns.

---

### Example 10: Conditional Forecast Method
```
=IF(FORECAST.ETS.SEASONALITY(Values, Dates)>1,
    FORECAST.ETS(Target, Values, Dates),
    FORECAST.LINEAR(Target, Values, Dates))
```
**Result:** ETS if seasonal, linear if not

**Explanation:** Automatic method selection based on detected pattern.

---

### Example 11: With Data Completion
```
=FORECAST.ETS.SEASONALITY(Values, Timeline, 1)
```
**Result:** Seasonality with interpolated gaps

**Explanation:** Interpolates missing values before detection.

---

### Example 12: Named Ranges
```
=FORECAST.ETS.SEASONALITY(Sales_History, Date_History)
```
**Result:** Detected seasonal period

**Explanation:** Using named ranges improves formula readability.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Arrays have different lengths | Ensure values and timeline match. |
| `#NUM!` | Insufficient data | Need enough history for pattern detection. |
| `#NAME?` | Function not recognized | Requires Excel 2016+. Not in Google Sheets. |
| `Returns 1` | No pattern detected | Data may lack seasonality or pattern is too weak. |

## Use Cases

### [[Validating Forecast Assumptions]]

**Scenario:** Demand planner verifies ETS model is using expected seasonality.

**Implementation:**
```
Detected: =FORECAST.ETS.SEASONALITY(Demand, Dates)
Expected: 12
Match: =IF(Detected=12, "Verified", "WARNING")
Forecast: =FORECAST.ETS(Target, Demand, Dates, 12)  // Override if needed
```

**Business Application:** Prevents forecast errors from incorrect seasonality assumptions.

**Technical Details:** Always verify auto-detection against business knowledge.

---

### [[Discovering Hidden Patterns]]

**Scenario:** Data analyst explores whether data contains seasonal patterns.

**Implementation:**
```
Monthly: =FORECAST.ETS.SEASONALITY(MonthlyData, MonthDates)
Weekly: =FORECAST.ETS.SEASONALITY(WeeklyData, WeekDates)
Daily: =FORECAST.ETS.SEASONALITY(DailyData, DayDates)
```

**Business Application:** Discovers cyclical patterns for business strategy.

**Technical Details:** Different aggregation levels may reveal different patterns.

---

### [[Choosing Forecast Method]]

**Scenario:** Analyst decides between seasonal and non-seasonal forecasting.

**Implementation:**
```
Check: =FORECAST.ETS.SEASONALITY(Data, Timeline)
Method: =IF(Check > 1, "Use FORECAST.ETS", "Use FORECAST.LINEAR")
```

**Business Application:** Automatically selects appropriate forecasting method.

**Technical Details:** Seasonality=1 indicates no detected pattern.

---

### [[Cross-Product Comparison]]

**Scenario:** Portfolio manager compares seasonal patterns across products.

**Implementation:**
```
Product_A: =FORECAST.ETS.SEASONALITY(A_Sales, Dates)
Product_B: =FORECAST.ETS.SEASONALITY(B_Sales, Dates)
Same: =IF(A=B, "Same pattern", "Different")
```

**Business Application:** Informs portfolio strategy and resource allocation.

**Technical Details:** Products with different patterns may smooth combined demand.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2016 and later
- **Full support:** All parameters functional

### Google Sheets
- **Availability:** NOT SUPPORTED
- **Alternative:** Manual autocorrelation analysis or visual inspection

### Key Difference Alert
**CRITICAL: FORECAST.ETS.SEASONALITY is Excel-only.**

## Tips and Best Practices

1. **Always verify auto-detected seasonality:** Do not blindly trust auto-detection.

2. **Override when you know better:** If business knowledge contradicts detection, override.

3. **Ensure sufficient data:** Need enough history to observe multiple cycles.

4. **Use for data exploration:** Run on new datasets to discover patterns.

5. **Document detected seasonality:** Record for model reproducibility.

6. **Compare across series:** Check consistency of patterns across related data.

7. **Re-evaluate periodically:** Seasonality can change over time.

8. **Consider multiple patterns:** Data may have weekly AND annual patterns.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FORECAST.ETS]] | Seasonal forecast | For actual predictions |
| [[FORECAST.ETS.STAT]] | Model statistics | For smoothing parameters |
| [[CORREL]] | Correlation | For manual autocorrelation |

### Commonly Used Together

**[[FORECAST.ETS]]** - Forecasting
```
Period: =FORECAST.ETS.SEASONALITY(Values, Dates)
Forecast: =FORECAST.ETS(Target, Values, Dates, Period)
```

---

**[[FORECAST.ETS.STAT]]** - Model parameters
```
Seasonality: =FORECAST.ETS.SEASONALITY(Values, Dates)
Gamma: =FORECAST.ETS.STAT(Values, Dates, 3, Seasonality)
```

---

**[[IF]]** - Conditional logic
```
=IF(FORECAST.ETS.SEASONALITY(Data, Dates) > 1, "Seasonal", "Non-seasonal")
```

## Official Documentation

- **Microsoft Excel:** [FORECAST.ETS.SEASONALITY function](https://support.microsoft.com/en-us/office/forecast-ets-seasonality-function-c80eff86-5e66-4dbf-8af1-ef0f0db77695)
- **Google Sheets:** Not available

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2016 | Introduced with FORECAST.ETS family |
| Excel 365 | Continuous updates | Ongoing improvements |
| Google Sheets | Not available | No plans announced |

---

*Last updated: 2026-01-10*
