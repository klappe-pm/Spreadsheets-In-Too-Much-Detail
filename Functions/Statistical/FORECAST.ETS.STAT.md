---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- statistical
subTopics:
- forecasting
- model-statistics
- exponential-smoothing
- model-diagnostics
- accuracy-metrics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ETS Statistics
- Forecast Model Stats
- Smoothing Parameters
- ETS Diagnostics
tags:
- statistical
- forecasting
- model-statistics
- accuracy
- diagnostics
---

# FORECAST.ETS.STAT

## Description

**FORECAST.ETS.STAT** returns statistical metrics and model parameters from the Exponential Triple Smoothing (ETS) algorithm, providing diagnostic information about the forecast model. This function reveals the internal workings of FORECAST.ETS, including smoothing parameters (alpha, beta, gamma) and accuracy metrics (MASE, SMAPE, MAE, RMSE).

The function returns different statistics based on the stat_type parameter. These include smoothing parameters that control how the model weights recent vs. historical data, and error metrics that quantify how well the model fits the historical data.

**Key statistics:** Alpha (level smoothing), Beta (trend smoothing), Gamma (seasonal smoothing), MASE (Mean Absolute Scaled Error), SMAPE (Symmetric Mean Absolute Percentage Error), MAE (Mean Absolute Error), and RMSE (Root Mean Square Error).

**Platform availability:** This is an **Excel-only function** introduced in Excel 2016. Google Sheets does not support this function.

## Syntax

> [!f(x)] FORECAST.ETS.STAT Syntax
>
> ```
> =FORECAST.ETS.STAT(values, timeline, stat_type, [seasonality], [data_completion], [aggregation])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `values` | Yes | Historical values. Same data used in FORECAST.ETS. |
| `timeline` | Yes | Historical dates/times. Must match values length. |
| `stat_type` | Yes | Integer 1-8 specifying which statistic to return (see table below). |
| `seasonality` | No | Seasonal period. Should match FORECAST.ETS. 0=none, 1=auto, or specify. |
| `data_completion` | No | Missing value handling. 0=zeros, 1=interpolate (default). |
| `aggregation` | No | Duplicate aggregation. 1=AVERAGE (default), 2-7 for others. |

### Stat Type Values

| stat_type | Statistic | Description | Interpretation |
|-----------|-----------|-------------|----------------|
| 1 | Alpha | Level smoothing (0-1) | Higher = more responsive to level changes |
| 2 | Beta | Trend smoothing (0-1) | Higher = more responsive to trend changes |
| 3 | Gamma | Seasonal smoothing (0-1) | Higher = more responsive to seasonal changes |
| 4 | MASE | Mean Absolute Scaled Error | <1 = better than naive forecast |
| 5 | SMAPE | Symmetric MAPE (0-200%) | Lower = better fit |
| 6 | MAE | Mean Absolute Error | Average error in original units |
| 7 | RMSE | Root Mean Square Error | Penalizes large errors |
| 8 | Step size | Timeline step | Detected time interval |

### Return Value

Returns a numeric value for the requested statistic. For smoothing parameters (1-3), returns 0-1. For error metrics (4-7), returns non-negative values where lower is better.

## Examples

> [!f(x)] FORECAST.ETS.STAT Examples

### Example 1: Get Alpha (Level Smoothing)
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 1, 12)
```
**Result:** 0.35 (example)

**Explanation:** Alpha of 0.35 means moderate responsiveness to recent level changes.

---

### Example 2: Get Beta (Trend Smoothing)
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 2, 12)
```
**Result:** 0.10 (example)

**Explanation:** Low beta indicates stable trend over time.

---

### Example 3: Get Gamma (Seasonal Smoothing)
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 3, 12)
```
**Result:** 0.25 (example)

**Explanation:** Gamma of 0.25 indicates moderate seasonal responsiveness.

---

### Example 4: Get MASE
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 4, 12)
```
**Result:** 0.78 (example)

**Explanation:** MASE < 1 means model outperforms naive forecast by 22%.

---

### Example 5: Get SMAPE
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 5, 12)
```
**Result:** 8.5 (representing 8.5%)

**Explanation:** Average percentage error is about 8.5%.

---

### Example 6: Get MAE
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 6, 12)
```
**Result:** 1250 (in original units)

**Explanation:** On average, predictions are off by 1,250 units.

---

### Example 7: Get RMSE
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 7, 12)
```
**Result:** 1580

**Explanation:** RMSE penalizes large errors more than MAE.

---

### Example 8: Complete Model Dashboard
```
Alpha: =FORECAST.ETS.STAT(Values, Dates, 1, 12)
Beta: =FORECAST.ETS.STAT(Values, Dates, 2, 12)
Gamma: =FORECAST.ETS.STAT(Values, Dates, 3, 12)
MASE: =FORECAST.ETS.STAT(Values, Dates, 4, 12)
```
**Result:** Complete diagnostic report

**Explanation:** Creates comprehensive view of model parameters and accuracy.

---

### Example 9: Compare Models
```
With_Season: =FORECAST.ETS.STAT(Values, Dates, 4, 12)
No_Season: =FORECAST.ETS.STAT(Values, Dates, 4, 0)
Better: =IF(With < No, "Seasonal", "Non-seasonal")
```
**Result:** Identifies better model

**Explanation:** Compare MASE across model specifications.

---

### Example 10: Quality Assessment
```
MASE: =FORECAST.ETS.STAT(Values, Dates, 4, 12)
Rating: =IF(MASE<0.5, "Excellent", IF(MASE<1, "Good", "Poor"))
```
**Result:** Quality rating

**Explanation:** Classifies model quality based on MASE thresholds.

---

### Example 11: Model Validation Report
```
="MASE=" & TEXT(FORECAST.ETS.STAT(Values, Dates, 4, 12), "0.00")
```
**Result:** "MASE=0.78"

**Explanation:** Formatted string for reporting.

---

### Example 12: Get Step Size
```
=FORECAST.ETS.STAT(B2:B37, A2:A37, 8)
```
**Result:** 30 (approximately, for monthly)

**Explanation:** Returns detected time interval between points.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid stat_type | Must be 1-8. |
| `#NUM!` | Insufficient data | Need 2+ seasonal cycles. |
| `#VALUE!` | Arrays different lengths | Ensure values and timeline match. |
| `#NAME?` | Function not recognized | Requires Excel 2016+. Not in Google Sheets. |

## Use Cases

### [[Model Performance Evaluation]]

**Scenario:** Demand planner evaluates forecast model before using it for inventory planning.

**Implementation:**
```
MASE: =FORECAST.ETS.STAT(Demand, Dates, 4, 12)
MAE: =FORECAST.ETS.STAT(Demand, Dates, 6, 12)
Verdict: =IF(MASE<1, "Acceptable", "Needs improvement")
```

**Business Application:** Provides objective evidence of forecast quality.

**Technical Details:** MASE < 1 indicates model beats naive forecast.

---

### [[Model Comparison]]

**Scenario:** Analyst chooses between different model configurations.

**Implementation:**
```
Model_A: =FORECAST.ETS.STAT(Values, Dates, 4, 12)
Model_B: =FORECAST.ETS.STAT(Values, Dates, 4, 0)
Best: =IF(A<=B, "12-period", "No seasonality")
```

**Business Application:** Ensures optimal model configuration.

**Technical Details:** Compare same metric across models; lower is better.

---

### [[Model Monitoring]]

**Scenario:** Data science team monitors forecast quality over time.

**Implementation:**
```
Current_MASE: =FORECAST.ETS.STAT(RecentData, RecentDates, 4, 12)
Baseline: 0.75
Alert: =IF(Current > Baseline*1.2, "DEGRADATION ALERT", "Stable")
```

**Business Application:** Detects when model accuracy degrades.

**Technical Details:** Monitor for significant increases in error metrics.

---

### [[Understanding Model Behavior]]

**Scenario:** Statistician explains model to stakeholders.

**Implementation:**
```
Alpha: =FORECAST.ETS.STAT(Values, Dates, 1, 12)
Beta: =FORECAST.ETS.STAT(Values, Dates, 2, 12)
Gamma: =FORECAST.ETS.STAT(Values, Dates, 3, 12)
```

**Business Application:** Enables clear communication about model behavior.

**Technical Details:** High parameters mean more responsive to recent data.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2016 and later
- **Full support:** All stat_type values (1-8)

### Google Sheets
- **Availability:** NOT SUPPORTED
- **Alternative:** Manual error metric calculations

### Key Difference Alert
**CRITICAL: FORECAST.ETS.STAT is Excel-only.**

## Tips and Best Practices

1. **Use MASE as primary accuracy metric:** Scale-independent and compares against naive forecast.

2. **Check all smoothing parameters:** Very high alpha suggests random walk behavior.

3. **Compare models systematically:** Use same error metric across different settings.

4. **Monitor over time:** Set up regular checks to catch model degradation.

5. **Use MAE for business context:** In original units for stakeholder communication.

6. **Consider RMSE for outlier sensitivity:** Use when large errors are especially costly.

7. **Document baseline metrics:** Record when model is deployed for future comparison.

8. **Match parameters across functions:** Use same seasonality in all ETS functions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FORECAST.ETS]] | Point forecast | For actual predictions |
| [[FORECAST.ETS.CONFINT]] | Confidence intervals | For uncertainty |
| [[FORECAST.ETS.SEASONALITY]] | Detected seasonality | For pattern verification |

### Commonly Used Together

**[[FORECAST.ETS]]** - Point forecasting
```
Quality: =FORECAST.ETS.STAT(Values, Dates, 4, 12)
Forecast: =IF(Quality < 1, FORECAST.ETS(...), "Model unreliable")
```

---

**[[FORECAST.ETS.CONFINT]]** - Uncertainty
```
MASE: =FORECAST.ETS.STAT(Values, Dates, 4, 12)
Confidence: =FORECAST.ETS.CONFINT(...)
```

---

**[[IF]]** - Conditional logic
```
=IF(FORECAST.ETS.STAT(Values, Dates, 4, 12) < 0.5, "Excellent", "Review")
```

## Official Documentation

- **Microsoft Excel:** [FORECAST.ETS.STAT function](https://support.microsoft.com/en-us/office/forecast-ets-stat-function-9cb87575-a4de-45fc-a4b7-3a28a0e1c7d6)
- **Google Sheets:** Not available

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2016 | Introduced with FORECAST.ETS family |
| Excel 365 | Continuous updates | Ongoing improvements |
| Google Sheets | Not available | No plans announced |

---

*Last updated: 2026-01-10*
