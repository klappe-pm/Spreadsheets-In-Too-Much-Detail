---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- internal-rate-of-return
- investment-analysis
- irregular-cash-flows
- portfolio-performance
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Extended IRR
- IRR with Dates
- Irregular IRR
tags:
- financial-modeling
- investment-analysis
- portfolio-management
- return-calculation
---

# XIRR

## Description

**XIRR (Extended Internal Rate of Return)** calculates the internal rate of return for a series of cash flows occurring on specific, irregular dates. Unlike the standard IRR function which assumes equal periods between cash flows, XIRR handles real-world investment scenarios where contributions, withdrawals, and returns happen at any time. This makes XIRR essential for calculating true investment performance when timing varies.

The function finds the annual rate of return that makes the net present value (NPV) of all cash flows equal to zero when discounted to the first date. XIRR uses an iterative calculation (Newton-Raphson method) because the equation cannot be solved algebraically. The result is always annualized, representing a yearly rate regardless of the actual investment duration. A 6-month investment returning 5% would show approximately 10.25% XIRR (annualized with compounding).

**Understanding sign conventions is critical:** Cash flowing OUT (investments, purchases, deposits) is **negative**. Cash flowing IN (withdrawals, sales, distributions) is **positive**. The first cash flow is typically negative (initial investment), and subsequent cash flows can be positive or negative. At least one cash flow must be negative and one positive for a valid IRR to exist. Getting signs wrong produces errors or misleading results.

XIRR is available in all versions of Excel and Google Sheets with identical behavior. The function is the industry standard for calculating time-weighted returns on portfolios, private equity investments, and any scenario where cash flows occur on irregular dates. For regular periodic cash flows, the standard IRR function is computationally simpler and produces equivalent results.

## Syntax

> [!f(x)] XIRR Syntax
>
> ```
> =XIRR(values, dates, [guess])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `values` | Yes | Array or range of cash flows. Must contain at least one negative and one positive value. First value corresponds to first date. |
| `dates` | Yes | Array or range of dates corresponding to each cash flow. Must be the same size as values. Dates can be in any order, but the earliest date is used as the base. |
| `guess` | No | Your estimate of the IRR. Defaults to 0.1 (10%). Provide a closer guess if the function fails to converge. |

### Return Value

Returns the annualized internal rate of return as a decimal. Multiply by 100 for percentage. The rate is always expressed as an annual rate, regardless of the actual time span of the cash flows.

## Examples

> [!f(x)] XIRR Examples

### Example 1: Basic Investment Return
```
Values: -10000, 12000
Dates: 1/1/2024, 1/1/2025
=XIRR({-10000, 12000}, {DATE(2024,1,1), DATE(2025,1,1)})
```
**Result:** `0.20` (20% annual return)

**Explanation:** Invest $10,000 on Jan 1, 2024, receive $12,000 one year later. The $2,000 gain on $10,000 = 20% return. Since this is exactly one year, the result is straightforward.

---

### Example 2: Partial Year Investment
```
Values: -10000, 10500
Dates: 1/1/2024, 7/1/2024
=XIRR({-10000, 10500}, {DATE(2024,1,1), DATE(2024,7,1)})
```
**Result:** `0.1038` (10.38% annualized)

**Explanation:** $500 profit over 6 months is 5% for the period. XIRR annualizes this: (1.05)^2 - 1 = 10.25%. The slight difference (10.38%) is due to XIRR's actual-day calculation rather than exact half-year.

---

### Example 3: Multiple Cash Flows
```
Values: -50000, 10000, 10000, 10000, 10000, 15000
Dates: 1/1/2024, 4/1/2024, 7/1/2024, 10/1/2024, 1/1/2025, 4/1/2025
=XIRR(values, dates)
```
**Result:** `0.1247` (12.47% annual return)

**Explanation:** A $50,000 investment with quarterly distributions. XIRR calculates the annualized return that makes the NPV of all these cash flows zero, accounting for the timing of each.

---

### Example 4: Private Equity Investment
```
Values: -100000, -50000, 25000, 0, 175000
Dates: 1/15/2020, 6/1/2020, 12/15/2021, 6/1/2022, 3/15/2024
=XIRR(values, dates)
```
**Result:** `0.1532` (15.32% annual return)

**Explanation:** A typical private equity pattern: initial investment, capital call, distribution, no activity, and final exit. XIRR handles the irregular timing and multiple cash flow directions.

---

### Example 5: Real Estate Investment
```
Values: -300000, 2000, 2000, 2000, ...(monthly rent for 5 years)..., 380000
Dates: Purchase date, monthly dates, sale date
=XIRR(all_values, all_dates)
```
**Result:** Calculated annualized return including rental income and appreciation

**Explanation:** Real estate returns include purchase, rental income (positive monthly cash flows), and eventual sale. XIRR captures the full investment performance including timing of all cash flows.

---

### Example 6: Dollar-Cost Averaging
```
Values: -1000, -1000, -1000, ...(12 monthly investments)..., 13500
Dates: Monthly investment dates, final value date
=XIRR(values, dates)
```
**Result:** Annualized return on systematic investment plan

**Explanation:** When investing monthly, later contributions have less time to grow. XIRR properly weights each contribution by its time in the market, giving a true money-weighted return.

---

### Example 7: Portfolio with Withdrawals
```
Values: -100000, 5000, -20000, 130000
Dates: 1/1/2023, 4/1/2023, 7/1/2023, 12/31/2024
=XIRR(values, dates)
```
**Result:** `0.0842` (8.42% annual return)

**Explanation:** Initial investment of $100,000, $5,000 withdrawal, $20,000 additional investment, final value of $130,000. Note: withdrawal is positive (money coming to you), additional investment is negative.

---

### Example 8: Negative Return
```
Values: -50000, 42000
Dates: 1/1/2024, 1/1/2025
=XIRR({-50000, 42000}, {DATE(2024,1,1), DATE(2025,1,1)})
```
**Result:** `-0.16` (-16% annual return)

**Explanation:** Lost $8,000 on $50,000 = -16% return. XIRR correctly handles losses, returning negative rates.

---

### Example 9: Using Guess Parameter
```
=XIRR(values, dates, 0.5)
```
**Result:** Helps convergence for unusual scenarios

**Explanation:** When XIRR returns #NUM! or #VALUE!, try different guesses. High-return scenarios may need guesses above 0.1; low/negative returns may need guesses below 0.1 or even negative.

---

### Example 10: Very Short Investment Period
```
Values: -10000, 10100
Dates: 1/1/2024, 1/15/2024
=XIRR({-10000, 10100}, {DATE(2024,1,1), DATE(2024,1,15)})
```
**Result:** `3.1797` (317.97% annualized!)

**Explanation:** 1% return over 14 days, annualized, produces an enormous rate. This illustrates why annualized returns can be misleading for very short periods--no one actually earns 318% over a year.

---

### Example 11: Compare to Time-Weighted Return
```
XIRR: =XIRR(cash_flows, dates) - Money-weighted return
TWR: Product of sub-period returns - Time-weighted return
```
**Result:** XIRR and TWR can differ significantly

**Explanation:** XIRR (money-weighted) measures actual investor experience, affected by timing of contributions. TWR measures manager performance independent of cash flow timing. Use XIRR for personal returns, TWR for manager evaluation.

---

### Example 12: #NUM! Error Resolution
```
=XIRR({-1000, 500, 400}, dates)
```
**Result:** `#NUM!` (may not converge)

**Explanation:** When total returns don't exceed total investments, or cash flows don't support a valid IRR, XIRR returns #NUM!. Verify signs are correct, at least one positive and one negative value exist, and consider providing a guess.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | No convergence after 100 iterations | Try different guess values. Check that signs are correct. Verify mathematical solution exists. |
| `#NUM!` | All positive or all negative values | Must have at least one positive and one negative cash flow. |
| `#VALUE!` | Dates and values arrays different sizes | Ensure each cash flow has a corresponding date. |
| `#VALUE!` | Invalid dates | Dates must be valid Excel date values, not text. |
| `Result seems too high/low` | Sign convention error | Investments (out) should be negative; returns (in) should be positive. |
| `Very different from expected` | Annualization effect | XIRR annualizes all returns. Short periods produce extreme annualized rates. |

## Use Cases

### [[Portfolio Performance Measurement]]

**Scenario:** An investor wants to calculate their true return on a portfolio where they made multiple contributions and withdrawals throughout the year.

**Implementation:**
```
Cash flows:
1/1/2024: -$100,000 (initial investment)
3/15/2024: -$25,000 (additional investment)
6/1/2024: +$10,000 (withdrawal)
9/1/2024: -$15,000 (additional investment)
12/31/2024: +$155,000 (ending value)

=XIRR({-100000, -25000, 10000, -15000, 155000},
      {DATE(2024,1,1), DATE(2024,3,15), DATE(2024,6,1),
       DATE(2024,9,1), DATE(2024,12,31)})
```
**Result:** `0.1523` (15.23% money-weighted annual return)

**Business Application:** Financial advisors use XIRR to report client portfolio performance. The money-weighted return reflects the investor's actual experience, including the impact of their contribution/withdrawal timing decisions.

**Technical Details:** XIRR is the standard for personal investment performance because it accounts for cash flow timing. An investor who added money before a rally earns a higher XIRR than one who added after, even if the underlying portfolio return was identical.

---

### [[Private Equity Fund Returns]]

**Scenario:** A private equity fund manager needs to calculate the internal rate of return for a fund with irregular capital calls and distributions over a 7-year life.

**Implementation:**
```
Fund cash flows:
Year 0: -$10M (initial commitment drawn)
Year 1: -$5M (capital call)
Year 2: -$3M (capital call)
Year 3: +$4M (distribution)
Year 4: +$2M (distribution)
Year 5: +$8M (distribution)
Year 6: +$6M (distribution)
Year 7: +$15M (final distribution)

=XIRR(all_cash_flows, all_dates)
```
**Result:** Fund IRR for investor reporting

**Business Application:** Private equity funds report IRR as a primary performance metric. Limited partners use XIRR-calculated IRR to compare fund performance and make allocation decisions.

**Technical Details:** PE returns are typically reported as "Net IRR" (after fees and carry) and "Gross IRR" (before fees). The J-curve pattern (early negative cash flows, later positive) is captured accurately by XIRR.

---

### [[Real Estate Investment Analysis]]

**Scenario:** A real estate investor wants to calculate the total return on a rental property including purchase, operating cash flows, and sale.

**Implementation:**
```
Property investment:
1/1/2019: -$400,000 (purchase with 20% down + closing costs)
Monthly: +$2,500 net rent (60 months)
1/1/2024: +$550,000 (sale proceeds after costs)

Total cash flows: Initial purchase + 60 monthly rents + sale

=XIRR(all_cash_flows, all_dates)
```
**Result:** `0.1847` (18.47% annualized return)

**Business Application:** Real estate investors use XIRR to compare properties and evaluate investment decisions. The calculation captures both cash flow yield (rent) and appreciation (sale price), properly weighting by timing.

**Technical Details:** For leveraged investments, the cash flows should reflect actual money invested/received, not property value. The $400,000 represents the down payment plus costs, not the full property price. This gives the levered return.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Uses Newton-Raphson iteration. Maximum 100 iterations. Tolerance of 0.000001%.
- **Precision:** IEEE 754 double-precision floating point.

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Same iterative algorithm.
- **Precision:** Same as Excel.

### Both Platforms
- Returns annualized rate regardless of actual time span
- Requires at least one positive and one negative cash flow
- Dates can be in any order (function sorts internally)
- Uses 365-day year for calculation

## Tips and Best Practices

1. **Get signs right:** This is the most common error. Money OUT of your pocket is negative. Money IN to your pocket is positive. Initial investment = negative. Final value or sale = positive.

2. **Include the final value:** For ongoing investments, include the current value as the final positive cash flow with today's date. This represents what you could receive if you sold.

3. **Be cautious with short periods:** XIRR annualizes everything. A 2% return over 1 month becomes ~27% annualized. State the actual time period alongside annualized returns for context.

4. **Use guess for stubborn calculations:** If XIRR returns #NUM!, try different guesses. Start with 0, then try 0.5, -0.5, 1, etc. The guess helps the algorithm find the solution.

5. **Match dates and values exactly:** Every cash flow needs a date. Use consistent date formatting. The arrays must be the same size.

6. **Understand money-weighted vs time-weighted:** XIRR gives money-weighted return (affected by cash flow timing). For comparing investment managers, time-weighted return may be more appropriate.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[IRR]] | IRR for regular periodic cash flows | When cash flows occur at regular intervals |
| [[MIRR]] | Modified IRR with financing/reinvestment rates | When you want to specify reinvestment rate |
| [[XNPV]] | NPV for irregular dates | To find NPV at a specific discount rate |
| [[RATE]] | Interest rate for annuity | For regular payment/present/future value scenarios |

### Commonly Used Together

**[[XNPV]]** - Verify XIRR Calculation

*NPV at XIRR should be zero:*
```
XIRR: =XIRR(values, dates) = 0.15
Verify: =XNPV(0.15, values, dates) ≈ 0
```
XNPV at the XIRR rate should equal approximately zero (within rounding).

---

**[[NPV]]** - Compare to Regular NPV

*For regular periods:*
```
XIRR: =XIRR(values, dates) - handles actual dates
IRR: =IRR(values) - assumes equal periods
```
Use IRR when cash flows are exactly periodic; XIRR for real-world irregular timing.

---

**[[DATE]]** - Create Date Values

*Proper date formatting:*
```
=XIRR(values, {DATE(2024,1,1), DATE(2024,6,1), DATE(2025,1,1)})
```
Use DATE function to ensure proper date values, avoiding text-date issues.

---

**[[IFERROR]]** - Handle Calculation Failures

*Graceful error handling:*
```
=IFERROR(XIRR(values, dates), "Cannot calculate IRR")
```
Catch #NUM! errors when XIRR cannot converge.

## Official Documentation

- **Microsoft Excel:** [XIRR function](https://support.microsoft.com/en-us/office/xirr-function-de1242ec-6477-445b-b11b-a303ad9adc9d)
- **Google Sheets:** [XIRR function](https://support.google.com/docs/answer/3093266)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 (Analysis ToolPak) | Originally required add-in |
| Excel 2007+ | Built-in function | No longer requires add-in |
| Google Sheets | Original launch | Built-in function from start |

---

*Last updated: 2026-01-10*
