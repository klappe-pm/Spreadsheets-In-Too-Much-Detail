---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
- excel-specific
subTopics:
- loan-analysis
- principal-calculations
- amortization
- equity-building
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cumulative Principal Payment
- Accumulated Principal
- Total Principal Between Periods
tags:
- financial-modeling
- loan-analysis
- mortgage
- amortization
- principal
- equity
---

# CUMPRINC

## Description

**CUMPRINC (Cumulative Principal)** calculates the total principal paid on a loan between two specified payment periods. While CUMIPMT tells you how much you're paying in interest, CUMPRINC tells you how much you're actually paying down the loan balance. This is the equity-building portion of your payments—the part that reduces what you owe. For homeowners, this directly translates to home equity accumulation; for businesses, it represents the reduction in debt liabilities.

Understanding principal accumulation is crucial for several financial planning scenarios. In the early years of an amortized loan, most of your payment goes to interest with only a small amount reducing principal. CUMPRINC quantifies this precisely: you might discover that in year one of a 30-year mortgage, only 20% of your payments reduce the balance while 80% is interest. This ratio improves dramatically over time, and CUMPRINC lets you calculate exactly how much principal you'll pay down during any window of the loan's life.

**Important nuances:** Like CUMIPMT, CUMPRINC returns a **negative number** by default, representing money flowing out from your perspective. The type parameter (0 or 1) specifies whether payments occur at the end or beginning of each period—most conventional loans use end-of-period (type=0). For accurate results, the rate must match your payment frequency: divide an annual rate by 12 for monthly payments, by 4 for quarterly payments, etc.

CUMPRINC is available in Excel (built-in since 2007, Analysis ToolPak in earlier versions) and Google Sheets (native support). Both platforms produce identical results. The function assumes a constant interest rate and payment amount throughout the loan—for adjustable-rate mortgages or loans with varying payments, you'll need to build a custom amortization schedule. CUMPRINC and CUMIPMT are complementary: their sum for any period range equals the total payments made during that range.

## Syntax

> [!f(x)] CUMPRINC Syntax
>
> ```
> =CUMPRINC(rate, nper, pv, start_period, end_period, type)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rate` | Yes | Interest rate per period. For annual rate with monthly payments, divide by 12. Must match payment frequency. |
| `nper` | Yes | Total number of payment periods for the loan. A 30-year monthly mortgage = 360 periods. Must be a positive integer. |
| `pv` | Yes | Present value (principal amount) of the loan. Enter as a positive number. |
| `start_period` | Yes | First payment period in the calculation. Must be between 1 and nper. Period 1 is the first payment. |
| `end_period` | Yes | Last payment period in the calculation. Must be between start_period and nper. |
| `type` | Yes | When payments are due: 0 = end of period (standard for most loans), 1 = beginning of period. |

### Return Value

Returns a negative number representing the cumulative principal paid between start_period and end_period (inclusive). The negative sign indicates cash outflow. Use ABS() or multiply by -1 to display as positive.

## Examples

> [!f(x)] CUMPRINC Examples

### Example 1: Principal Paid in First Year of Mortgage
```
=CUMPRINC(0.06/12, 360, 250000, 1, 12, 0)
```
**Result:** `-$3,127.49`

**Explanation:** In the first 12 months of a $250,000 mortgage at 6% over 30 years, only $3,127 goes toward principal. Compare this to CUMIPMT for the same period (-$14,849 in interest)—in year one, 83% of your payments are interest, only 17% reduces the loan.

---

### Example 2: Principal Over Entire Loan Life
```
=CUMPRINC(0.05/12, 360, 300000, 1, 360, 0)
```
**Result:** `-$300,000.00`

**Explanation:** Total principal paid over the full life of any loan always equals the original loan amount. This serves as a validation check—if you get a different number, something's wrong with your inputs.

---

### Example 3: Comparing Early vs. Late Years
```
Year 1:  =CUMPRINC(0.06/12, 360, 250000, 1, 12, 0)   → -$3,127.49
Year 10: =CUMPRINC(0.06/12, 360, 250000, 109, 120, 0) → -$5,541.51
Year 25: =CUMPRINC(0.06/12, 360, 250000, 289, 300, 0) → -$12,685.22
```
**Result:** Principal increases over time

**Explanation:** The inverse of interest—as the loan matures, more of each payment reduces principal. In year 1, $3,127 goes to principal; by year 25, it's $12,685. This is why the middle and later years of a mortgage build equity much faster.

---

### Example 4: Equity Accumulation by Year
```
=ABS(CUMPRINC(0.065/12, 360, 400000, 1, 60, 0))
```
**Result:** `$25,834.17`

**Explanation:** After 5 years (60 months) of a $400,000 mortgage at 6.5%, you've paid down $25,834 of principal. This is your forced savings through the mortgage—your home equity from principal payments alone (not including appreciation).

---

### Example 5: Car Loan Equity Tracking
```
Year 1: =ABS(CUMPRINC(0.07/12, 60, 35000, 1, 12, 0))  → $5,854
Year 2: =ABS(CUMPRINC(0.07/12, 60, 35000, 13, 24, 0)) → $6,295
Year 3: =ABS(CUMPRINC(0.07/12, 60, 35000, 25, 36, 0)) → $6,769
```
**Result:** Principal acceleration visible

**Explanation:** Track how quickly you're paying down an auto loan. By comparing principal paid to vehicle depreciation, you can estimate when you're no longer "underwater" on the loan.

---

### Example 6: 15-Year vs 30-Year Principal Comparison
```
30-year, Year 1: =ABS(CUMPRINC(0.06/12, 360, 250000, 1, 12, 0)) → $3,127
15-year, Year 1: =ABS(CUMPRINC(0.06/12, 180, 250000, 1, 12, 0)) → $13,063
```
**Result:** 15-year pays 4x more principal in year 1

**Explanation:** A 15-year mortgage at the same rate pays down principal dramatically faster. In year one, $13,063 goes to principal vs. only $3,127 for the 30-year. This explains why 15-year mortgages build equity so much faster.

---

### Example 7: Business Equipment Loan
```
=CUMPRINC(0.08/4, 20, 100000, 1, 4, 0)
```
**Result:** `-$17,458.45`

**Explanation:** Quarterly payments on a $100,000 equipment loan at 8% over 5 years (20 quarters). First year (4 quarters) pays down $17,458 of principal—useful for balance sheet forecasting and debt reduction planning.

---

### Example 8: Beginning-of-Period Payments
```
=CUMPRINC(0.06/12, 360, 250000, 1, 12, 1)
```
**Result:** `-$3,143.10`

**Explanation:** Same mortgage but with payments at the beginning of each month (type=1). Slightly more principal paid ($3,143 vs $3,127) because each payment reduces principal earlier, so less interest accrues, leaving more room for principal in subsequent payments.

---

### Example 9: Validating Total Payments
```
Total Principal: =ABS(CUMPRINC(0.06/12, 360, 250000, 1, 12, 0)) → $3,127.49
Total Interest:  =ABS(CUMIPMT(0.06/12, 360, 250000, 1, 12, 0))  → $14,848.90
Total Payments:  =PMT(0.06/12, 360, 250000) * -12              → $17,976.39
```
**Result:** $3,127.49 + $14,848.90 = $17,976.39

**Explanation:** Principal plus interest must equal total payments. This validation formula helps catch input errors. If the numbers don't match, check your parameters.

---

### Example 10: Loan Payoff Analysis
```
Remaining balance after 10 years:
=250000 + CUMPRINC(0.06/12, 360, 250000, 1, 120, 0)
```
**Result:** `$211,004.76`

**Explanation:** The remaining balance equals original principal plus cumulative principal paid (which is negative). After 120 payments on a $250,000 mortgage, you still owe $211,005—less than $40,000 paid down in 10 years due to front-loaded interest.

---

### Example 11: Extra Payment Impact Estimation
```
Standard payments, 5 years: =CUMPRINC(0.06/12, 360, 250000, 1, 60, 0) → -$12,976.21
If loan were 25-year from start: =CUMPRINC(0.06/12, 300, 250000, 1, 60, 0) → -$17,276.45
```
**Result:** Shorter term = faster principal reduction

**Explanation:** By comparing different loan terms, you can estimate the impact of making extra payments. The difference ($4,300) approximates additional principal reduction from higher payments.

---

### Example 12: Home Sale Equity Calculation
```
Original loan: $350,000
Years owned: 7 (84 months)
Principal paid: =ABS(CUMPRINC(0.055/12, 360, 350000, 1, 84, 0)) → $40,221.77
Down payment: $70,000
Total equity (excluding appreciation): $70,000 + $40,221.77 = $110,221.77
```
**Result:** Equity calculation for sale planning

**Explanation:** When planning to sell a home, calculate equity from down payment plus principal paid. Add (or subtract) home value change to estimate net proceeds from sale.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Rate is negative, or nper/pv is zero or negative | Ensure rate >= 0, nper > 0, and pv > 0. |
| `#NUM!` | start_period < 1 or > nper | start_period must be at least 1 and at most equal to nper. |
| `#NUM!` | end_period < start_period or > nper | end_period must be >= start_period and <= nper. |
| `#NUM!` | type is not 0 or 1 | Type must be exactly 0 (end of period) or 1 (beginning of period). |
| `#VALUE!` | Non-numeric values in parameters | Check that all inputs are numbers. Watch for cells formatted as text. |
| Wrong magnitude | Rate not adjusted for payment frequency | Divide annual rate by 12 for monthly, by 4 for quarterly, etc. |
| Unexpected negative | Function returns negative by default | Use ABS() or multiply by -1 for positive display. |
| Total != loan amount | Errors in rate or period specification | Verify that CUMPRINC for entire loan (1 to nper) equals -pv. |

## Use Cases

### [[Home Equity Tracking Dashboard]]

**Scenario:** A homeowner wants to track equity buildup through mortgage payments alongside home appreciation to monitor net worth.

**Implementation:**
```
Monthly Principal Payment (this month): =ABS(CUMPRINC(rate/12, term*12, original_loan, current_month, current_month, 0))
YTD Principal Paid: =ABS(CUMPRINC(rate/12, term*12, original_loan, first_month_of_year, current_month, 0))
Lifetime Principal Paid: =ABS(CUMPRINC(rate/12, term*12, original_loan, 1, current_month, 0))
Current Loan Balance: =original_loan + CUMPRINC(rate/12, term*12, original_loan, 1, current_month, 0)
```

**Business Application:** Personal finance dashboards use CUMPRINC to show homeowners how mortgage payments translate to equity. Combined with home value tracking (Zillow, Redfin), it provides complete home equity visualization.

**Technical Details:** Store the original loan parameters (amount, rate, term) in fixed cells. Calculate the current period number based on payment start date. Refresh monthly to track progress against amortization schedule.

---

### [[Business Debt Reduction Forecasting]]

**Scenario:** A CFO needs to project balance sheet changes from debt reduction across multiple loans, forecasting year-by-year liability decreases.

**Implementation:**
```
Loan A Principal (Year 1): =ABS(CUMPRINC(0.065/12, 84, 500000, 1, 12, 0))
Loan B Principal (Year 1): =ABS(CUMPRINC(0.07/4, 20, 250000, 1, 4, 0))
Total Debt Reduction Year 1: =SUM of above

5-Year Projection:
Year 1: =ABS(CUMPRINC(..., 1, 12, 0)) + =ABS(CUMPRINC(..., 1, 4, 0))
Year 2: =ABS(CUMPRINC(..., 13, 24, 0)) + =ABS(CUMPRINC(..., 5, 8, 0))
...and so on
```

**Business Application:** Financial planning and analysis (FP&A) teams use this for balance sheet forecasting. The reduction in long-term liabilities flows directly into cash flow projections and covenant compliance monitoring.

**Technical Details:** Handle loans with different payment frequencies by building separate CUMPRINC calculations for each. Sum them for total principal reduction. Account for any balloon payments or refinancing events separately.

---

### [[Refinancing Equity Preservation Analysis]]

**Scenario:** A homeowner considering refinancing wants to understand how much equity they've built and ensure it's preserved in the new loan structure.

**Implementation:**
```
Current Loan Principal Paid: =ABS(CUMPRINC(old_rate/12, old_term*12, original_loan, 1, months_paid, 0))
Current Balance: =original_loan + CUMPRINC(old_rate/12, old_term*12, original_loan, 1, months_paid, 0)
New Loan Amount: =Current Balance + closing_costs (if rolled in)
Equity Preserved: =home_value - new_loan_amount
```

**Business Application:** Mortgage brokers and financial advisors use this analysis to show clients their equity position before and after refinancing. It helps ensure clients don't inadvertently cash out equity or increase their debt inappropriately.

**Technical Details:** Consider whether closing costs are paid out of pocket or rolled into the new loan. If rolled in, equity decreases by closing cost amount. Also factor in any cash-out portion if applicable.

## Platform Differences

### Microsoft Excel
- **Availability:** Built-in since Excel 2007. Earlier versions require Analysis ToolPak add-in (Tools > Add-ins > Analysis ToolPak).
- **Specific Behavior:** All six arguments are required. Returns #NUM! error for invalid parameter combinations.

### Google Sheets
- **Availability:** Native support since original launch. No add-in required.
- **Specific Behavior:** Identical to Excel in calculation and error handling. Same parameter requirements.

### Both Platforms
- Return negative values by default (cash outflow perspective)
- Require the type parameter explicitly (0 or 1)
- Assume fixed interest rate and consistent payment amounts
- Period numbering is 1-based (first payment = period 1)
- Combined with CUMIPMT, total equals PMT * number of periods

## Tips and Best Practices

1. **Verify with the full-life check:** CUMPRINC from period 1 to nper should exactly equal -pv (the negative of the loan amount). If it doesn't, your parameters are wrong.

2. **Use with CUMIPMT for complete analysis:** Principal plus interest equals total payment. Build dashboards that show both components to give the complete picture of loan payments.

3. **Calculate remaining balance easily:** Original_pv + CUMPRINC(rate, nper, pv, 1, current_period, type) gives you the outstanding balance. The sum is positive because CUMPRINC is negative.

4. **Document period mapping:** When analyzing specific calendar years, clearly document which payment periods map to which months/years. A mortgage starting in June has different period-to-year mapping than one starting in January.

5. **Consider for net worth tracking:** Principal paid is forced savings. Track cumulative principal paid as a component of net worth alongside other investments.

6. **Use ABS() in dashboards:** For user-facing reports, convert to positive numbers with ABS() to avoid confusion about the negative convention.

7. **Validate against amortization schedules:** For critical decisions, build a full amortization table and verify that CUMPRINC results match the sum of the principal column.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CUMIPMT]] | Cumulative interest paid between two periods | When you need the interest portion, not principal |
| [[PPMT]] | Principal portion of a single payment | When you need principal for one specific period only |
| [[IPMT]] | Interest portion of a single payment | When you need interest for one specific period |
| [[PMT]] | Calculate the total periodic payment | When you need the total payment amount |

### Commonly Used Together

**[[CUMIPMT]]** - Cumulative Interest

*Combined for complete payment decomposition:*
```
Year 1 Principal: =ABS(CUMPRINC(0.06/12, 360, 250000, 1, 12, 0))  → $3,127
Year 1 Interest: =ABS(CUMIPMT(0.06/12, 360, 250000, 1, 12, 0))   → $14,849
Year 1 Total: =PMT(0.06/12, 360, 250000) * -12                   → $17,976
Verification: $3,127 + $14,849 = $17,976
```
Always use together for complete loan analysis.

---

**[[PMT]]** - Payment Amount

*Combined for loan balance calculations:*
```
Monthly Payment: =PMT(0.06/12, 360, 250000) → -$1,498.88
Remaining Balance After 5 Years: =250000 + CUMPRINC(0.06/12, 360, 250000, 1, 60, 0) → $237,024
```
PMT determines the payment; CUMPRINC shows how much reduces the balance.

---

**[[FV]]** - Future Value

*Combined for loan-vs-invest analysis:*
```
Principal paid if keeping loan: =CUMPRINC(0.06/12, 360, 250000, 1, 60, 0)
Investment growth if paying cash: =FV(0.07/12, 60, 0, -250000)
```
Compare forced savings through mortgage vs. investment returns to inform cash vs. finance decisions.

---

**[[IF]]** - Conditional Logic

*Combined for milestone tracking:*
```
=IF(ABS(CUMPRINC(rate, nper, pv, 1, period, 0)) >= pv * 0.2, "20% Equity Milestone Reached", "Below 20%")
```
Track when principal payments reach certain thresholds (like the 20% equity point that eliminates PMI).

## Official Documentation

- **Microsoft Excel:** [CUMPRINC function](https://support.microsoft.com/en-us/office/cumprinc-function-94a4516d-bd65-41a1-bc16-053a6646aa1a)
- **Google Sheets:** [CUMPRINC function](https://support.google.com/docs/answer/3093436)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2003 & earlier | Analysis ToolPak add-in | Required manual add-in installation |
| Excel 2007+ | Built-in function | No add-in required |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Native support, identical to Excel |

---

*Last updated: 2026-01-10*
