---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- end-of-month
- month-calculation
- date-arithmetic
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- End of Month
- Last Day of Month
- Month End Date
tags:
- eomonth
- end-of-month
- month-end
- date-calculation
- financial-dates
---

# EOMONTH

## Description

**EOMONTH** returns the last day of a month that is a specified number of months before or after a starting date. This function is essential for financial reporting, period-end calculations, contract expirations, and any scenario requiring the final day of a month. It automatically handles the variable lengths of months (28, 29, 30, or 31 days) and correctly adjusts for leap years.

The function takes a start date and a months offset. An offset of 0 returns the last day of the start date's month; positive offsets move forward that many months; negative offsets move backward. EOMONTH(January 15, 0) returns January 31; EOMONTH(January 15, 1) returns February 28 (or 29 in leap years); EOMONTH(January 15, -1) returns December 31 of the previous year.

**Why EOMONTH matters:** Without EOMONTH, finding the last day of a month requires understanding that months have different lengths. You'd need complex logic: "If month is 1, 3, 5, 7, 8, 10, or 12, it's 31; if 4, 6, 9, or 11, it's 30; if 2, check for leap year..." EOMONTH eliminates all of this. It simply returns the correct last day, period. This is particularly valuable for February where the last day depends on whether the year is a leap year.

EOMONTH is heavily used in financial contexts: calculating payment due dates at month end, determining reporting period boundaries, scheduling recurring month-end activities, and computing date ranges for SUMIFS and other filtering functions. It pairs naturally with EOMONTH(date, -1)+1 to find the first day of a month, and with EDATE for month-to-month date calculations.

## Syntax

> [!f(x)] EOMONTH Syntax
>
> ```
> =EOMONTH(start_date, months)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `start_date` | Yes | The starting date. The specific day within the month doesn't affect the result—only the month and year matter. |
| `months` | Yes | The number of months to move forward (positive) or backward (negative) from start_date. Use 0 to get the end of the start_date's month. |

### Return Value

Returns a serial number representing the last day of the target month. The result is always a valid date that is the final calendar day of that month (28, 29, 30, or 31).

## Examples

> [!f(x)] EOMONTH Examples

### Example 1: End of Current Month
```
=EOMONTH(DATE(2026, 1, 15), 0)
```
**Result:** 1/31/2026

**Explanation:** With months=0, returns the last day of the same month as the start date. January always has 31 days.

---

### Example 2: End of Next Month
```
=EOMONTH(DATE(2026, 1, 15), 1)
```
**Result:** 2/28/2026

**Explanation:** One month forward from January gives February. 2026 is not a leap year, so February has 28 days.

---

### Example 3: End of Previous Month
```
=EOMONTH(DATE(2026, 3, 20), -1)
```
**Result:** 2/28/2026

**Explanation:** One month back from March gives February. Useful for finding the end of the prior period.

---

### Example 4: Leap Year February
```
=EOMONTH(DATE(2024, 1, 1), 1)
```
**Result:** 2/29/2024

**Explanation:** 2024 is a leap year, so February has 29 days. EOMONTH automatically handles this.

---

### Example 5: First Day of Month (Combined Pattern)
```
=EOMONTH(A1, -1) + 1
```
**Result:** First day of the month containing A1

**Explanation:** Get the last day of the previous month, then add 1. This always gives the first of the current month.

---

### Example 6: First Day of Next Month
```
=EOMONTH(A1, 0) + 1
```
**Result:** First day of the month after A1

**Explanation:** End of current month plus 1 equals the start of next month.

---

### Example 7: End of Quarter
```
=EOMONTH(DATE(2026, 1, 1), 3 - MOD(MONTH(A1) - 1, 3) - 1)
```
**Result:** Last day of the quarter containing the date

**Explanation:** Calculates how many months until quarter end, then uses EOMONTH to find that date. Q1 ends March 31, Q2 ends June 30, etc.

---

### Example 8: Payment Due at Month End
```
=EOMONTH(invoice_date, 1)
```
**Result:** End of the month following the invoice

**Explanation:** Common "net 30" alternative: payment due at the end of the following month regardless of invoice date.

---

### Example 9: Six Months Forward
```
=EOMONTH(TODAY(), 6)
```
**Result:** Last day of the month 6 months from now

**Explanation:** For semi-annual planning, contracts, or forecasts extending 6 months out.

---

### Example 10: Fiscal Year End (June 30)
```
=EOMONTH(DATE(YEAR(A1) + IF(MONTH(A1) > 6, 1, 0), 6, 1), 0)
```
**Result:** June 30 of the fiscal year containing the date

**Explanation:** For fiscal years ending June 30, finds the appropriate fiscal year-end date.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid start_date (not a date) | Ensure start_date is a valid date value |
| `#VALUE!` | Non-numeric months parameter | Months must be a number (can be decimal, will be truncated) |
| `#NUM!` | Result date is out of range | Check that the result falls within Excel's valid date range |
| Unexpected date | Confusion about which month | Remember months=0 is the current month, not next month |
| Date displays as number | Cell not formatted as date | Apply date formatting to the result cell |

## Use Cases

### [[Financial Period Boundaries]]

**Scenario:** Define month-end dates for financial reporting, closing periods, and period comparisons across fiscal calendars.

**Implementation:**
```
=EOMONTH(TODAY(), 0)                                      (Current month end)
=EOMONTH(TODAY(), -1) + 1 & " to " & EOMONTH(TODAY(), 0)  (Current month range)
=SUMIFS(Revenue, Dates, ">=" & EOMONTH(A1, -1) + 1, Dates, "<=" & EOMONTH(A1, 0))
```

**Business Application:** Controllers close books at month end. Financial analysts create period-over-period comparisons. Treasury teams forecast cash positions at month boundaries.

**Technical Details:** EOMONTH handles fiscal year-end calculations elegantly. For fiscal years ending in June: =EOMONTH(DATE(year, 6, 1), 0). For quarters: calculate months to quarter end and use EOMONTH.

---

### [[Contract and Subscription Management]]

**Scenario:** Calculate contract end dates, subscription renewals, and billing period boundaries that align with month ends.

**Implementation:**
```
=EOMONTH(contract_start, contract_months - 1)             (Contract end date)
=IF(TODAY() > EOMONTH(start, term), "Expired", "Active")  (Expiration check)
=EOMONTH(start, 12) - EOMONTH(start, 0)                   (Days in contract year)
```

**Business Application:** SaaS platforms determine subscription end dates. Insurance companies calculate policy expiration. Landlords track lease boundaries.

**Technical Details:** For month-end billing, EOMONTH ensures bills always fall on the last day regardless of how many days are in the month. A subscription starting January 31 and billed monthly would next bill on February 28/29, March 31, April 30, etc.

---

### [[Data Aggregation and Reporting]]

**Scenario:** Create dynamic date ranges for data filtering, enabling automatic month-to-date and period comparisons in dashboards and reports.

**Implementation:**
```
=EOMONTH(TODAY(), -1) + 1                                 (MTD start)
=SUMIFS(Sales, Dates, ">=" & EOMONTH(B$1, -1) + 1, Dates, "<=" & EOMONTH(B$1, 0))
=EOMONTH(EOMONTH(TODAY(), 0), -12) + 1                    (Same month last year start)
```

**Business Application:** Dashboard designers create self-updating period boundaries. Report builders construct month-over-month analyses. Data analysts filter to complete months only.

**Technical Details:** Combining EOMONTH with SUMIFS/COUNTIFS creates powerful date-based aggregations. The pattern EOMONTH(date, -1)+1 to EOMONTH(date, 0) defines any month's complete range.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (native); Analysis ToolPak in earlier versions
- **Decimal handling:** Months parameter is truncated to integer
- **Date system:** Works with 1900 date system (Mac may differ)
- **Maximum range:** Standard Excel date limits apply

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel
- **Decimal handling:** Same truncation behavior
- **Compatibility:** Full cross-platform compatibility

### Key Difference Alert
EOMONTH behaves identically across Excel and Sheets. The only consideration is the date system (1900 vs 1904 on older Mac versions), which affects the underlying serial number but not the displayed date result.

## Tips and Best Practices

1. **Use EOMONTH(date, -1)+1 for first of month:** This pattern is cleaner than DATE(YEAR(date), MONTH(date), 1) and handles year boundaries automatically.

2. **Remember months=0 is current month:** A common mistake is using 1 to get the current month's end. Months=0 gives the end of the start_date's own month.

3. **Combine with TEXT for formatted periods:** =TEXT(EOMONTH(date, 0), "mmmm yyyy") gives "January 2026" style period labels.

4. **Use for quarter-end calculations:** Q1=EOMONTH(DATE(year, 3, 1), 0), Q2=EOMONTH(DATE(year, 6, 1), 0), etc. Or build dynamic formulas based on month.

5. **Don't worry about the day:** EOMONTH(DATE(2026, 1, 1), 0) equals EOMONTH(DATE(2026, 1, 31), 0). The day parameter is ignored—only month and year matter.

6. **Chain for complex calculations:** EOMONTH(EOMONTH(date, 0), 1) gives end of the month after next. Build chains for multi-step date navigation.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[EDATE]] | Returns date that is N months away | When you want the same day, not month end |
| [[DATE]] | Constructs date from year, month, day | When you need specific day of month |
| [[DAY]] | Extracts day from date | When you need to check the day number |

### Commonly Used Together

**[[EDATE]]** - Add months preserving day

*Compare EOMONTH vs EDATE:*
```
=EOMONTH(DATE(2026, 1, 15), 1)    // Returns 2/28/2026 (month end)
=EDATE(DATE(2026, 1, 15), 1)      // Returns 2/15/2026 (same day)
```
EOMONTH always gives month end; EDATE preserves the day number.

---

**[[SUMIFS]] / [[COUNTIFS]]** - Period filtering

*Aggregate data by month:*
```
=SUMIFS(Sales, Dates, ">=" & EOMONTH(A1, -1) + 1, Dates, "<=" & EOMONTH(A1, 0))
=COUNTIFS(Orders, ">=" & EOMONTH(TODAY(), -1) + 1, Orders, "<=" & EOMONTH(TODAY(), 0))
```
Define complete month boundaries for data aggregation.

---

**[[TODAY]]** - Dynamic current month

*Self-updating period calculations:*
```
=EOMONTH(TODAY(), 0)              // Current month end
=EOMONTH(TODAY(), -1) + 1         // Current month start
=EOMONTH(TODAY(), -1)             // Last month end
```
Create formulas that automatically update to current period.

---

**[[TEXT]]** - Format period labels

*Display month-end as formatted text:*
```
=TEXT(EOMONTH(A1, 0), "mmmm yyyy")      // "January 2026"
=TEXT(EOMONTH(A1, 0), "mmm-yy")         // "Jan-26"
```
Convert EOMONTH results to readable period identifiers.

## Official Documentation

- **Microsoft Excel:** [EOMONTH function](https://support.microsoft.com/en-us/office/eomonth-function-7314ffa1-2bc9-4005-9d66-f49db127d628)
- **Google Sheets:** [EOMONTH function](https://support.google.com/docs/answer/3093044)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2007 | 2007 | Native function (previously Analysis ToolPak) |
| Excel 2010+ | 2010 | No changes |
| Excel 365 | Current | No changes |
| Google Sheets | Original launch (2006) | Full support |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
