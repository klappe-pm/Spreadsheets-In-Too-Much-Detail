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
- interest-calculations
- amortization
- mortgage-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cumulative Interest Payment
- Accumulated Interest
- Total Interest Between Periods
tags:
- financial-modeling
- loan-analysis
- mortgage
- amortization
- interest
---

# CUMIPMT

## Description

**CUMIPMT (Cumulative Interest Payment)** calculates the total interest paid on a loan between two specified payment periods. Instead of calculating interest for a single payment, CUMIPMT aggregates all interest payments across a range of periods—giving you the total interest portion paid from, say, month 13 through month 24 of a mortgage. This is essential for loan analysis, tax planning (mortgage interest is often deductible), and understanding the true cost of borrowing over time.

The function is particularly valuable because loan payments are not created equal. In the early years of a mortgage, most of each payment goes toward interest with only a small portion reducing principal. Over time, this ratio shifts—later payments are mostly principal. CUMIPMT lets you quantify exactly how much interest you'll pay during any window of your loan's life. Want to know how much interest you'll pay in year 3 of a 30-year mortgage? CUMIPMT answers that precisely.

**Critical parameter: the type argument.** CUMIPMT requires you to specify whether payments occur at the beginning of each period (type=1, annuity due) or the end of each period (type=0, ordinary annuity). Most loans use end-of-period payments (type=0). Getting this wrong shifts all your calculations by one period, which particularly matters for short-term loans or when calculating interest for specific calendar years. Also note that CUMIPMT returns a **negative number** by default, representing money flowing out from your perspective—multiply by -1 or use ABS() if you want positive values.

CUMIPMT is available in Excel (all versions with the Analysis ToolPak add-in, built-in since Excel 2007) and Google Sheets (native support). Both platforms produce identical results. The function complements CUMPRINC (cumulative principal) and together they allow complete loan decomposition. For irregular payment schedules, consider building a full amortization table instead, as CUMIPMT assumes constant payments and consistent interest rates.

## Syntax

> [!f(x)] CUMIPMT Syntax
>
> ```
> =CUMIPMT(rate, nper, pv, start_period, end_period, type)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rate` | Yes | Interest rate per period. For annual rate with monthly payments, divide by 12 (e.g., 6%/12 = 0.5% per month). Must be consistent with payment frequency. |
| `nper` | Yes | Total number of payment periods for the loan. For a 30-year monthly mortgage, nper = 360. Must be positive integer. |
| `pv` | Yes | Present value (principal amount) of the loan. Enter as a positive number. |
| `start_period` | Yes | First payment period in the calculation. Must be between 1 and nper. Period 1 is the first payment. |
| `end_period` | Yes | Last payment period in the calculation. Must be between start_period and nper. |
| `type` | Yes | When payments are due: 0 = end of period (most loans), 1 = beginning of period. |

### Return Value

Returns a negative number representing the cumulative interest paid between start_period and end_period (inclusive). The negative sign indicates cash outflow. Use ABS() or multiply by -1 for positive values.

## Examples

> [!f(x)] CUMIPMT Examples

### Example 1: Total Interest in First Year of Mortgage
```
=CUMIPMT(0.06/12, 360, 250000, 1, 12, 0)
```
**Result:** `-$14,848.90`

**Explanation:** Calculates total interest paid during the first 12 months of a $250,000 mortgage at 6% annual rate over 30 years (360 months). Payments occur at end of month (type=0). Nearly $15,000 in interest alone during year one—that's almost the entire annual payment amount going to interest.

---

### Example 2: Interest Over Entire Loan Life
```
=CUMIPMT(0.05/12, 360, 300000, 1, 360, 0)
```
**Result:** `-$279,767.35`

**Explanation:** Total interest paid over the full life of a $300,000 mortgage at 5% over 30 years. You'll pay almost as much in interest ($279,767) as you borrowed! This illustrates why understanding the true cost of borrowing matters.

---

### Example 3: Comparing Early vs. Late Years
```
Year 1:  =CUMIPMT(0.06/12, 360, 250000, 1, 12, 0)   → -$14,848.90
Year 10: =CUMIPMT(0.06/12, 360, 250000, 109, 120, 0) → -$12,434.88
Year 25: =CUMIPMT(0.06/12, 360, 250000, 289, 300, 0) → -$5,291.17
```
**Result:** Interest decreases over time

**Explanation:** This demonstrates amortization dynamics. In year 1, you pay nearly $15,000 in interest. By year 10, it's dropped to about $12,400. By year 25, it's down to just over $5,200. As principal decreases, so does the interest charged on it.

---

### Example 4: Car Loan Interest Analysis
```
=CUMIPMT(0.07/12, 60, 35000, 1, 60, 0)
```
**Result:** `-$6,523.09`

**Explanation:** Total interest on a $35,000 car loan at 7% APR over 5 years (60 months). The total cost of financing the car is $6,523—important to factor in when negotiating purchase price or comparing to paying cash.

---

### Example 5: Interest for Tax Year Planning
```
=CUMIPMT(0.055/12, 360, 400000, 25, 36, 0)
```
**Result:** `-$21,348.24`

**Explanation:** Interest paid during months 25-36 of a mortgage (calendar year 3, if you closed in January). Useful for projecting mortgage interest deduction for tax planning. This becomes the basis for your 1098 form estimate.

---

### Example 6: Quarterly Business Loan
```
=CUMIPMT(0.08/4, 20, 100000, 1, 4, 0)
```
**Result:** `-$7,516.87`

**Explanation:** First year's interest on a $100,000 business loan at 8% annual rate, paid quarterly over 5 years (20 quarters). Rate is divided by 4 for quarterly periods. First four payments (one year) accumulate $7,517 in interest.

---

### Example 7: Beginning-of-Period Payments (Annuity Due)
```
=CUMIPMT(0.06/12, 360, 250000, 1, 12, 1)
```
**Result:** `-$14,775.05`

**Explanation:** Same mortgage as Example 1, but with payments at the beginning of each month (type=1). Interest is slightly lower ($14,775 vs $14,849) because each payment reduces principal one month earlier, reducing interest charged.

---

### Example 8: Impact of Extra Payments Analysis
```
Before extra payments: =CUMIPMT(0.06/12, 360, 250000, 1, 360, 0) → -$289,595.47
After refinancing to 15-year: =CUMIPMT(0.06/12, 180, 250000, 1, 180, 0) → -$129,443.63
```
**Result:** Savings of $160,151.84

**Explanation:** Comparing total interest on 30-year vs 15-year mortgage at same rate. The 15-year mortgage saves over $160,000 in interest! Monthly payment is higher, but total cost is dramatically lower.

---

### Example 9: Partial Year Calculation
```
=CUMIPMT(0.065/12, 360, 325000, 7, 12, 0)
```
**Result:** `-$10,431.76`

**Explanation:** Interest for months 7-12 (July through December if loan closed in January). Useful when you need interest for a partial year—perhaps for tax deductions when you bought a home mid-year.

---

### Example 10: Using Cell References for Sensitivity Analysis
```
=CUMIPMT(B1/12, B2*12, B3, B4, B5, 0)
```
Where B1=0.055 (rate), B2=30 (years), B3=200000 (principal), B4=1, B5=12

**Result:** Depends on inputs

**Explanation:** Using cell references allows easy what-if analysis. Change the interest rate and instantly see how first-year interest changes. Essential for comparing loan offers from different lenders.

---

### Example 11: Comparing Loan Offers
```
Lender A (6.0%): =ABS(CUMIPMT(0.06/12, 360, 300000, 1, 360, 0)) → $347,514.57
Lender B (5.75%): =ABS(CUMIPMT(0.0575/12, 360, 300000, 1, 360, 0)) → $330,120.24
```
**Result:** Lender B saves $17,394.33 over loan life

**Explanation:** A 0.25% rate difference on a $300,000 mortgage saves over $17,000 in total interest. ABS() converts the negative result to positive for clearer comparison. Small rate differences compound significantly over 30 years.

---

### Example 12: Student Loan Analysis
```
=CUMIPMT(0.045/12, 120, 50000, 1, 120, 0)
```
**Result:** `-$12,372.36`

**Explanation:** Total interest on a $50,000 student loan at 4.5% over 10 years. The loan will cost $12,372 in interest over its life. Comparing this to income-driven repayment plans helps students make informed decisions.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Rate is negative, nper is zero or negative, pv is zero or negative | Ensure rate >= 0, nper > 0, and pv > 0. All must be positive (or rate can be zero). |
| `#NUM!` | start_period < 1 or > nper | start_period must be at least 1 and at most nper. |
| `#NUM!` | end_period < start_period or > nper | end_period must be >= start_period and <= nper. |
| `#NUM!` | type is not 0 or 1 | Type must be exactly 0 (end of period) or 1 (beginning of period). |
| `#VALUE!` | Text values in parameters | Ensure all parameters are numeric. Check for hidden text formatting. |
| Wrong result | Annual rate used with monthly periods without dividing by 12 | Always match rate period to payment period. Divide annual rate by 12 for monthly payments. |
| Wrong sign | Expected positive, got negative | CUMIPMT returns negative by default. Use ABS() or multiply by -1 for positive results. |

## Use Cases

### [[Mortgage Interest Deduction Planning]]

**Scenario:** A homeowner needs to estimate their mortgage interest deduction for tax planning, projecting interest by calendar year over the next 5 years.

**Implementation:**
```
Year 1: =ABS(CUMIPMT(0.065/12, 360, 450000, 1, 12, 0))    → $28,943
Year 2: =ABS(CUMIPMT(0.065/12, 360, 450000, 13, 24, 0))   → $28,283
Year 3: =ABS(CUMIPMT(0.065/12, 360, 450000, 25, 36, 0))   → $27,585
Year 4: =ABS(CUMIPMT(0.065/12, 360, 450000, 37, 48, 0))   → $26,847
Year 5: =ABS(CUMIPMT(0.065/12, 360, 450000, 49, 60, 0))   → $26,066
```

**Business Application:** Tax professionals and financial planners use these projections to estimate itemized deductions, determine whether to itemize vs. standard deduction, and plan for changes in tax liability as interest deduction decreases each year.

**Technical Details:** For mid-year purchases, adjust start_period to match the first full month of payments. If closing occurs in March, period 1 is April, so the first calendar year would be periods 1-9 (April-December). Account for any prepaid interest at closing separately.

---

### [[Loan Comparison Dashboard]]

**Scenario:** A corporate treasury team needs to compare financing options for a $5 million equipment purchase, evaluating total interest cost under different loan structures.

**Implementation:**
```
3-Year Term:    =ABS(CUMIPMT(0.075/12, 36, 5000000, 1, 36, 0))    → $591,584
5-Year Term:    =ABS(CUMIPMT(0.08/12, 60, 5000000, 1, 60, 0))     → $1,066,597
7-Year Term:    =ABS(CUMIPMT(0.085/12, 84, 5000000, 1, 84, 0))    → $1,587,425
```

**Business Application:** CFOs and treasury managers use this analysis to balance cash flow constraints (lower monthly payments with longer terms) against total cost of capital (lower interest with shorter terms). The dashboard informs capital budgeting decisions.

**Technical Details:** Include all costs: origination fees should be added to total cost, and any prepayment penalties should factor into early payoff scenarios. Use CUMPRINC alongside CUMIPMT to verify that principal plus interest equals total payments.

---

### [[Refinancing Break-Even Analysis]]

**Scenario:** A homeowner is considering refinancing a $350,000 mortgage from 6.5% to 5.5%, but needs to pay $8,000 in closing costs. When does the interest savings offset the refinancing cost?

**Implementation:**
```
Current loan interest (next 60 months):
=ABS(CUMIPMT(0.065/12, 360, 350000, 37, 96, 0)) → $104,429

New loan interest (same 60 months):
=ABS(CUMIPMT(0.055/12, 360, 350000, 1, 60, 0)) → $91,284

Monthly savings: ($104,429 - $91,284) / 60 = $219/month
Break-even: $8,000 / $219 = 37 months
```

**Business Application:** Mortgage advisors use this analysis to counsel clients on refinancing decisions. The break-even point determines whether refinancing makes sense given how long the client plans to stay in the home.

**Technical Details:** This simplified analysis assumes restarting a 30-year term. For more accurate comparison, compare remaining interest on old loan vs. full interest on new loan, and factor in the different payoff timelines.

## Platform Differences

### Microsoft Excel
- **Availability:** Built-in since Excel 2007. In earlier versions, requires Analysis ToolPak add-in (Tools > Add-ins > Analysis ToolPak).
- **Specific Behavior:** All arguments are required. Returns #NUM! for invalid inputs. Maximum precision follows Excel's 15-digit limit.

### Google Sheets
- **Availability:** Native support, no add-in required.
- **Specific Behavior:** Identical calculation method and results as Excel. Same parameter requirements and error handling.

### Both Platforms
- Return negative values by default (representing cash outflow)
- Require type parameter (0 or 1) explicitly
- Assume constant interest rate and payment amounts throughout loan life
- Period numbers are 1-indexed (first payment is period 1, not period 0)

## Tips and Best Practices

1. **Match rate and period frequency:** The most common error is using annual rate with monthly periods. Always divide annual rate by 12 for monthly payments, by 4 for quarterly, etc.

2. **Use ABS() for readability:** Since CUMIPMT returns negative values, wrap in ABS() for reports and dashboards where positive numbers are expected: `=ABS(CUMIPMT(...))`.

3. **Validate with PMT:** Cross-check your total payments: `PMT * nper` should equal `ABS(CUMIPMT(...)) + ABS(CUMPRINC(...))` for the full loan life.

4. **Build an amortization schedule for verification:** For critical financial decisions, build a full amortization table to verify CUMIPMT results and to see month-by-month breakdowns.

5. **Account for partial periods:** If analyzing a specific calendar year and the loan started mid-year, carefully calculate which periods correspond to which months.

6. **Consider prepayment scenarios:** CUMIPMT assumes the standard payment schedule. If planning extra payments, build a custom amortization table instead.

7. **Document assumptions:** When sharing spreadsheets, clearly note whether rates are annual or periodic, and whether type is 0 or 1.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CUMPRINC]] | Cumulative principal paid between two periods | When you need the principal portion of payments, not interest |
| [[IPMT]] | Interest portion of a single payment | When you need interest for one specific period, not a range |
| [[PPMT]] | Principal portion of a single payment | When you need principal for one specific period |
| [[PMT]] | Calculate the periodic payment amount | When you need the total payment amount, not interest/principal split |

### Commonly Used Together

**[[CUMPRINC]]** - Cumulative Principal Paid

*Combined to decompose total payments:*
```
Total Interest: =ABS(CUMIPMT(0.06/12, 360, 250000, 1, 12, 0))
Total Principal: =ABS(CUMPRINC(0.06/12, 360, 250000, 1, 12, 0))
Total Payments: =PMT(0.06/12, 360, 250000) * 12 * -1
```
Interest plus principal equals total payments. Use together to understand payment composition.

---

**[[PMT]]** - Payment Amount

*Combined for loan analysis:*
```
Monthly Payment: =PMT(0.06/12, 360, 250000)
First Year Interest: =CUMIPMT(0.06/12, 360, 250000, 1, 12, 0)
First Year Principal: =CUMPRINC(0.06/12, 360, 250000, 1, 12, 0)
```
PMT gives the fixed payment amount; CUMIPMT and CUMPRINC break it into components.

---

**[[PV]]** - Present Value

*Combined for loan sizing:*
```
=PV(0.06/12, 360, -1500)  → Maximum loan at $1,500/month payment
Then: =CUMIPMT(0.06/12, 360, PV_result, 1, 360, 0)  → Total interest on that loan
```
Use PV to determine affordable loan amount, then CUMIPMT to understand total interest cost.

---

**[[IF]]** - Conditional Logic

*Combined for payment milestone tracking:*
```
=IF(ABS(CUMIPMT(rate, nper, pv, 1, period, 0)) > target, "Interest Target Reached", "Below Target")
```
Track when cumulative interest crosses thresholds for reporting or alerts.

## Official Documentation

- **Microsoft Excel:** [CUMIPMT function](https://support.microsoft.com/en-us/office/cumipmt-function-61067bb0-9016-427d-b95b-1a752af0e606)
- **Google Sheets:** [CUMIPMT function](https://support.google.com/docs/answer/3093435)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2003 & earlier | Analysis ToolPak add-in | Required manual add-in installation |
| Excel 2007+ | Built-in | No add-in required |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Native support, identical to Excel |

---

*Last updated: 2026-01-10*
