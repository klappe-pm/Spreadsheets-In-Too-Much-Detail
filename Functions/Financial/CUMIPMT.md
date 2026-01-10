---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- cumulative-interest
- loan-amortization
- tax-deductions
- interest-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cumulative Interest Payment
- Total Interest Paid
- Lifetime Interest
tags:
- financial-modeling
- loan-analysis
- tax-planning
- mortgage-interest
---

# CUMIPMT

## Description

**CUMIPMT (Cumulative Interest Payment)** calculates the total interest paid on a loan between two specified payment periods. While IPMT shows interest for a single payment, CUMIPMT sums interest across a range of payments--perfect for answering questions like "How much interest will I pay this year?" or "What is my total interest over the life of this loan?" This function is essential for tax planning (mortgage interest deductions), loan comparisons, and understanding the true cost of borrowing.

The function provides a powerful window into loan costs that monthly statements obscure. A $300,000 mortgage at 6.5% over 30 years has monthly payments of about $1,896, but the total interest paid is over $382,000--more than the original principal. CUMIPMT reveals this cumulative burden by calculating interest across any period range, helping borrowers understand why loan terms, interest rates, and prepayment strategies matter so much.

**Understanding sign conventions is critical:** Cash you pay OUT is negative, and cash you receive IN is positive. CUMIPMT returns a **negative value** because interest represents money leaving your pocket. Additionally, the function requires a `type` parameter (0 or 1) that specifies payment timing--it is NOT optional like in PMT or IPMT. The start_period and end_period are 1-indexed: period 1 is the first payment.

CUMIPMT is available in all versions of Excel and Google Sheets with identical behavior. Unlike IPMT, which requires repeated calls for multiple periods, CUMIPMT efficiently calculates the sum in one function call. This makes it ideal for annual tax reporting, loan comparison analysis, and amortization summary tables. The companion function CUMPRINC calculates cumulative principal over the same period range.

## Syntax

> [!f(x)] CUMIPMT Syntax
>
> ```
> =CUMIPMT(rate, nper, pv, start_period, end_period, type)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rate` | Yes | The interest rate per period. For 6% annual with monthly payments, use 0.06/12. Must match the payment period. |
| `nper` | Yes | The total number of payment periods. For a 30-year mortgage with monthly payments, use 360. |
| `pv` | Yes | The present value--the loan principal. Must be positive for a standard loan. |
| `start_period` | Yes | The first period to include in the calculation. Must be >= 1. |
| `end_period` | Yes | The last period to include in the calculation. Must be >= start_period and <= nper. |
| `type` | Yes | When payments are due: 0 = end of period; 1 = beginning of period. **This parameter is required (not optional).** |

### Return Value

Returns the cumulative interest paid between start_period and end_period as a negative number (money you pay out). The absolute value represents total dollars of interest. To display as positive, use `-CUMIPMT(...)` or `ABS(CUMIPMT(...))`.

## Examples

> [!f(x)] CUMIPMT Examples

### Example 1: Total Interest Over Loan Life
```
=CUMIPMT(0.06/12, 360, 200000, 1, 360, 0)
```
**Result:** `-$231,676.38`

**Explanation:** Total interest paid over the entire 30-year mortgage: $231,676. The original loan was $200,000, so you pay back $431,676 total--more than double the borrowed amount. This stark reality check often motivates borrowers to consider shorter terms or extra payments.

---

### Example 2: First Year Interest (Tax Deduction)
```
=CUMIPMT(0.065/12, 360, 350000, 1, 12, 0)
```
**Result:** `-$22,421.18`

**Explanation:** Interest paid in year 1 of a $350,000 mortgage at 6.5%. This figure appears on Form 1098 from your lender and is potentially tax-deductible. First-year interest is highest because it is calculated on the full principal balance throughout.

---

### Example 3: Compare Year 1 vs. Year 30 Interest
```
Year 1: =CUMIPMT(0.06/12, 360, 200000, 1, 12, 0) = -$11,933.19
Year 30: =CUMIPMT(0.06/12, 360, 200000, 349, 360, 0) = -$424.80
```
**Result:** Year 1 has 28x more interest than year 30

**Explanation:** In year 1, interest dominates ($11,933 of $14,389 total payments). By year 30, only $425 of $14,389 goes to interest. This dramatic shift illustrates why early loan years feel expensive and why prepayment early in the loan is so impactful.

---

### Example 4: Display as Positive Number
```
=-CUMIPMT(0.06/12, 360, 200000, 1, 60, 0)
```
**Result:** `$58,189.49`

**Explanation:** Negating gives a positive display for reports. The first 5 years' interest is $58,189--on $200,000 borrowed, you have paid over 29% of principal in interest alone, while principal has only decreased about 7%.

---

### Example 5: Interest by 5-Year Blocks
```
Years 1-5: =CUMIPMT(0.06/12, 360, 200000, 1, 60, 0) = -$58,189.49
Years 6-10: =CUMIPMT(0.06/12, 360, 200000, 61, 120, 0) = -$53,186.73
Years 11-15: =CUMIPMT(0.06/12, 360, 200000, 121, 180, 0) = -$46,641.16
Years 16-20: =CUMIPMT(0.06/12, 360, 200000, 181, 240, 0) = -$38,102.73
Years 21-25: =CUMIPMT(0.06/12, 360, 200000, 241, 300, 0) = -$26,936.49
Years 26-30: =CUMIPMT(0.06/12, 360, 200000, 301, 360, 0) = -$12,619.78
```
**Result:** Interest decreases dramatically in later years

**Explanation:** The first 5 years' interest ($58K) exceeds the last 10 years combined ($39K). This breakdown helps borrowers understand why they should not refinance into a new 30-year term late in their loan--they would restart the high-interest years.

---

### Example 6: Car Loan Total Interest
```
=CUMIPMT(0.049/12, 60, 25000, 1, 60, 0)
```
**Result:** `-$3,230.55`

**Explanation:** Total interest on a 5-year car loan at 4.9%: $3,231. Compare to the mortgage example: shorter terms and lower rates dramatically reduce total interest. The car loan's interest is only 13% of principal versus 116% for the 30-year mortgage.

---

### Example 7: Quarterly Business Loan Interest
```
=CUMIPMT(0.08/4, 20, 100000, 1, 20, 0)
```
**Result:** `-$22,108.96`

**Explanation:** Total interest on a 5-year (20 quarters) business loan at 8%. Rate is divided by 4 for quarterly payments. This models equipment financing or lines of credit with quarterly payment terms.

---

### Example 8: Beginning-of-Period Payments
```
=CUMIPMT(0.06/12, 360, 200000, 1, 12, 1)
```
**Result:** `-$11,873.57`

**Explanation:** With beginning-of-period payments (type=1), year 1 interest is slightly less ($59.62 less) because each payment is made before a full period's interest accrues. This timing is typical for lease payments.

---

### Example 9: Interest After Lump Sum Payment
```
After paying extra $20,000 principal at month 24:
Original loan: $200,000 at 6%, 30 years
New effective balance: $177,540.07 (after 24 normal payments)
Less extra payment: $157,540.07
Remaining interest: =CUMIPMT(0.06/12, 336, 157540.07, 1, 336, 0) = -$144,678.89

Without extra payment: =CUMIPMT(0.06/12, 360, 200000, 25, 360, 0) = -$207,621.02
Interest saved: $62,942.13
```
**Result:** $20,000 extra payment saves $62,942 in interest

**Explanation:** A lump sum payment early in the loan avoids interest on that principal for the remaining term. The $20,000 extra payment saves over $62,000--a 3x return. This is why financial advisors recommend prepayment for those in high-interest debt.

---

### Example 10: Compare 15-Year vs 30-Year Total Interest
```
30-year at 6%: =CUMIPMT(0.06/12, 360, 250000, 1, 360, 0) = -$289,595.47
15-year at 5.5%: =CUMIPMT(0.055/12, 180, 250000, 1, 180, 0) = -$123,174.71

Difference: $166,420.76 saved with 15-year term
```
**Result:** 15-year saves $166K in interest

**Explanation:** The shorter term combined with typically lower rates on 15-year mortgages results in massive interest savings. The 15-year monthly payment is higher ($2,042 vs $1,499), but total cost is $166,420 less. CUMIPMT makes this comparison crystal clear.

---

### Example 11: Single Period (Matches IPMT)
```
=CUMIPMT(0.06/12, 360, 200000, 1, 1, 0)
```
**Result:** `-$1,000.00`

```
=IPMT(0.06/12, 1, 360, 200000)
```
**Result:** `-$1,000.00`

**Explanation:** When start_period = end_period, CUMIPMT returns the same value as IPMT for that period. This relationship validates that CUMIPMT is truly a sum of individual IPMT values.

---

### Example 12: Invalid Period Range
```
=CUMIPMT(0.06/12, 360, 200000, 50, 40, 0)
```
**Result:** `#NUM!`

**Explanation:** The end_period (40) cannot be less than start_period (50). Also, start_period must be >= 1 and end_period must be <= nper. Invalid ranges produce #NUM! errors.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | start_period > end_period | Ensure start <= end. Periods must be a valid ascending range. |
| `#NUM!` | start_period < 1 or end_period > nper | Periods must be within 1 to nper range. |
| `#NUM!` | pv <= 0 or nper <= 0 | Present value and number of periods must be positive. |
| `#VALUE!` | Non-numeric inputs or missing type parameter | Verify all parameters are numbers. Type is REQUIRED (0 or 1). |
| `Result is positive` | Unusual sign on pv | Standard loans have positive pv; result is negative. Negative pv (investments) produces positive cumulative interest. |
| `Doesn't match Form 1098` | Period range mismatch | Ensure your period range matches the tax year. Loan origination date affects which periods fall in which calendar year. |

## Use Cases

### [[Annual Tax Deduction Reporting]]

**Scenario:** A homeowner needs to calculate mortgage interest for Schedule A tax deduction. Their $400,000 mortgage at 6.25% originated in March 2024, so the first tax year includes only 10 months of payments.

**Implementation:**
```
First year (March-December, payments 1-10):
=CUMIPMT(0.0625/12, 360, 400000, 1, 10, 0)
```
**Result:** `-$20,585.77` (deductible interest)

```
Full second year (payments 11-22):
=CUMIPMT(0.0625/12, 360, 400000, 11, 22, 0)
```
**Result:** `-$24,333.01`

**Business Application:** CPAs and tax preparers use CUMIPMT to verify Form 1098 figures and project future deductions. The function also helps in year-end tax planning--clients can see how accelerating or delaying a payment affects the annual deduction.

**Technical Details:** The 1098 amount should closely match CUMIPMT, though minor differences may occur due to escrow adjustments, late fees, or prepaid interest at origination. Always reconcile with actual lender statements.

---

### [[Loan Comparison Analysis]]

**Scenario:** A borrower is comparing three financing options for a $300,000 home and wants to see total interest cost under each scenario over the first 10 years and over the full term.

**Implementation:**
```
Option A - 30-year at 6.5%:
10-year interest: =CUMIPMT(0.065/12, 360, 300000, 1, 120, 0) = -$181,489.88
Total interest: =CUMIPMT(0.065/12, 360, 300000, 1, 360, 0) = -$382,633.47

Option B - 15-year at 5.75%:
10-year interest: =CUMIPMT(0.0575/12, 180, 300000, 1, 120, 0) = -$133,284.66
Total interest: =CUMIPMT(0.0575/12, 180, 300000, 1, 180, 0) = -$164,606.78

Option C - 20-year at 6.0%:
10-year interest: =CUMIPMT(0.06/12, 240, 300000, 1, 120, 0) = -$156,543.08
Total interest: =CUMIPMT(0.06/12, 240, 300000, 1, 240, 0) = -$215,838.09
```

**Business Application:** Mortgage brokers and financial advisors use CUMIPMT to create compelling comparisons showing that the 15-year option saves $218,000 in interest versus the 30-year--more than the down payment many buyers struggle to save.

**Technical Details:** Include monthly payment comparison using PMT: the 15-year payment is $2,495 vs $1,896 for 30-year. For clients who cannot afford the higher payment, the 20-year option offers a middle ground with significant savings over 30-year.

---

### [[Refinance ROI Analysis]]

**Scenario:** A homeowner 5 years into a 30-year mortgage at 7% is offered a refinance to 5.5% for 25 years with $6,000 closing costs. What is the interest savings, and when is break-even?

**Implementation:**
```
Current loan: $350,000 at 7%, now at month 61 (25 years remaining)
Balance remaining: =FV(0.07/12, 60, -2328.56, 350000) = $326,414.23

Remaining interest on current loan:
=CUMIPMT(0.07/12, 360, 350000, 61, 360, 0) = -$371,175.09

New loan: $332,414 (balance + costs) at 5.5%, 25 years
Total interest: =CUMIPMT(0.055/12, 300, 332414, 1, 300, 0) = -$252,177.91

Gross savings: $371,175 - $252,178 = $118,997
Net savings after closing costs: $118,997 - $6,000 = $112,997
```

**Business Application:** Refinance specialists use CUMIPMT to show the total interest savings of refinancing. The $113K savings figure is far more compelling than "lower monthly payment," especially for clients hesitant about closing costs.

**Technical Details:** This analysis assumes the homeowner stays for the full term. For shorter holding periods, calculate interest saved only through the expected sale date. Monthly payment comparison: old = $2,329, new = $2,130 ($199/month savings).

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Requires the Analysis ToolPak in older Excel versions (pre-2007). Now built-in.
- **Precision:** IEEE 754 double-precision floating point.

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Built-in function, no add-in required. Functionally identical to Excel.
- **Precision:** Same as Excel.

### Both Platforms
- type parameter is REQUIRED (unlike in PMT/IPMT where it is optional)
- Returns negative for loans (interest paid out)
- Period parameters are 1-indexed (first payment = 1)
- Equivalent to sum of IPMT for all periods in range

## Tips and Best Practices

1. **Remember that type is required:** Unlike PMT or IPMT where type is optional, CUMIPMT requires the type parameter. Most loans use 0 (end of period). Omitting type causes an error.

2. **Use for tax planning:** CUMIPMT with period range matching your tax year gives you the deductible interest figure. This should closely match Form 1098 from your lender.

3. **Create period-by-period summaries:** Use CUMIPMT to build annual or 5-year summary tables that show how interest burden decreases over time. This helps clients understand loan amortization visually.

4. **Compare total interest across loan options:** When shopping for loans, comparing CUMIPMT for the full term reveals the true cost difference that monthly payment comparisons obscure.

5. **Calculate prepayment impact:** Use CUMIPMT on the remaining term to show total remaining interest, then compare to a scenario with extra payments (modeled with reduced principal).

6. **Verify with IPMT sum:** For any period range, CUMIPMT should equal the sum of individual IPMT calls for each period in that range. Use this to validate your calculations.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CUMPRINC]] | Cumulative principal over period range | When you need total principal paid rather than interest |
| [[IPMT]] | Interest for a single period | When you need interest for one specific payment |
| [[PMT]] | Total payment amount | When you need the complete payment per period |

### Commonly Used Together

**[[CUMPRINC]]** - Cumulative Principal

*Complete loan analysis:*
```
Year 1 Interest: =CUMIPMT(0.06/12, 360, 200000, 1, 12, 0) = -$11,933.19
Year 1 Principal: =CUMPRINC(0.06/12, 360, 200000, 1, 12, 0) = -$2,455.97
Year 1 Total Paid: $14,389.16 (matches 12 * PMT)
```
CUMIPMT + CUMPRINC = Total payments for the period range.

---

**[[FV]]** - Remaining Balance

*Track equity growth:*
```
Balance after year 5: =FV(0.06/12, 60, -PMT(0.06/12,360,200000), 200000)
Equity = Original - Balance = $200,000 - $186,109 = $13,891
Principal paid: =-CUMPRINC(0.06/12, 360, 200000, 1, 60, 0) = $13,891
```
FV calculates remaining balance; CUMPRINC calculates principal reduction (same value).

---

**[[PMT]]** - Verify Total Payments

*Validation check:*
```
Total payments over 5 years: =12 * 5 * PMT(0.06/12, 360, 200000) = -$71,945.88
CUMIPMT for 5 years: -$58,189.49
CUMPRINC for 5 years: -$13,756.39
Sum: -$71,945.88
```
PMT * periods = CUMIPMT + CUMPRINC for that range.

---

**[[IF]]** - Conditional Analysis

*Scenario switching:*
```
=IF(B1="15-year",
    CUMIPMT(0.055/12, 180, 250000, 1, 180, 0),
    CUMIPMT(0.06/12, 360, 250000, 1, 360, 0))
```
Allow users to compare total interest for different loan options dynamically.

## Official Documentation

- **Microsoft Excel:** [CUMIPMT function](https://support.microsoft.com/en-us/office/cumipmt-function-61067bb0-9016-427d-b95b-1a752af0e606)
- **Google Sheets:** [CUMIPMT function](https://support.google.com/docs/answer/3093200)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 (Analysis ToolPak) | Originally required add-in activation |
| Excel 2007+ | Built-in function | No longer requires add-in |
| Google Sheets | Original launch | Built-in function from start |

---

*Last updated: 2026-01-10*
