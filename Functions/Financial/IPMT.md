---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- interest-calculation
- loan-amortization
- debt-analysis
- payment-breakdown
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Interest Payment
- Interest Portion
- Loan Interest
tags:
- financial-modeling
- loan-analysis
- amortization
- interest-calculation
---

# IPMT

## Description

**IPMT (Interest Payment)** calculates the interest portion of a specific payment within a loan or investment with constant periodic payments. While PMT gives you the total payment amount, IPMT reveals exactly how much of each payment goes toward interest versus principal. This distinction is crucial for understanding the true cost of borrowing, projecting tax deductions (mortgage interest is often deductible), and building amortization schedules.

The function reveals a fundamental truth about amortizing loans: early payments are interest-heavy, while later payments are principal-heavy. On a 30-year mortgage, the first payment might be 80% interest and 20% principal. By payment 300, those proportions reverse. IPMT lets you examine any specific payment to see exactly where your money goes, which is essential for tax planning (interest deductions) and for understanding why loans feel like they "go nowhere" in the early years.

**Understanding sign conventions is essential:** Cash you pay OUT is negative, and cash you receive IN is positive. IPMT returns a **negative value** for loans because interest is money leaving your pocket to the lender. For investments where you receive interest (like an annuity), IPMT would return positive values. The period parameter (per) is 1-indexed: period 1 is the first payment, period 2 is the second, and so on up to nper.

IPMT is available in all versions of Excel and Google Sheets with identical behavior. The function requires that you specify the exact period you want to examine. For cumulative interest over a range of periods (like "total interest in year 5"), use CUMIPMT instead. Remember: IPMT + PPMT for any period equals PMT for that same loan--interest plus principal equals total payment.

## Syntax

> [!f(x)] IPMT Syntax
>
> ```
> =IPMT(rate, per, nper, pv, [fv], [type])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rate` | Yes | The interest rate per period. For 6% annual with monthly payments, use 0.06/12. Must match the payment period. |
| `per` | Yes | The specific period for which you want the interest. Must be between 1 and nper inclusive. Period 1 is the first payment. |
| `nper` | Yes | The total number of payment periods. For a 30-year mortgage with monthly payments, use 360. |
| `pv` | Yes | The present value--the loan principal (positive) or current investment value. |
| `fv` | No | The future value after the last payment. Usually 0 for loans (fully paid). Defaults to 0. |
| `type` | No | When payments are due: 0 (or omitted) = end of period; 1 = beginning of period. |

### Return Value

Returns the interest portion of the specified payment as a number. For loans, this is negative (interest you pay out). The absolute value represents dollars of interest. IPMT + PPMT for any period equals PMT for that loan.

## Examples

> [!f(x)] IPMT Examples

### Example 1: First Month's Interest on Mortgage
```
=IPMT(0.06/12, 1, 360, 200000)
```
**Result:** `-$1,000.00`

**Explanation:** The first month's interest on a $200,000 mortgage at 6% APR. Interest is calculated on the full principal: $200,000 x 0.06 / 12 = $1,000. This is the maximum interest payment--subsequent months have less interest as principal decreases. The total payment (PMT) would be about $1,199, so only $199 goes to principal in month 1.

---

### Example 2: Last Month's Interest
```
=IPMT(0.06/12, 360, 360, 200000)
```
**Result:** `-$5.95`

**Explanation:** The final payment's interest portion is tiny--only $5.95--because almost all principal has been paid. The remaining balance before this payment is about $1,193, and one month's interest on that is $5.95. The rest of the ~$1,199 payment goes entirely to paying off the final principal.

---

### Example 3: Compare Early vs. Late Payment Interest
```
Month 1: =IPMT(0.06/12, 1, 360, 200000) = -$1,000.00
Month 180 (halfway): =IPMT(0.06/12, 180, 360, 200000) = -$614.81
Month 300 (year 25): =IPMT(0.06/12, 300, 360, 200000) = -$256.72
```
**Result:** Interest decreases dramatically over loan life

**Explanation:** This comparison reveals loan amortization dynamics. At month 1, interest is $1,000; at month 180, it is $615; by month 300, only $257. The crossover point where principal exceeds interest occurs around month 221 on this loan. This is why early extra payments have such dramatic long-term effects.

---

### Example 4: Display as Positive Number
```
=-IPMT(0.06/12, 1, 360, 200000)
```
**Result:** `$1,000.00`

**Explanation:** Negating the result gives a positive number for display purposes. Amortization tables often show interest as positive for readability. The sign convention (negative = money out) is mathematically correct, but positive display is often preferred.

---

### Example 5: Annual Interest (Year 1 Total)
```
=SUMPRODUCT(--IPMT(0.06/12, ROW(1:12), 360, 200000))
```
**Result:** `-$11,933.19`

**Explanation:** Sums interest for periods 1-12 (first year). Alternatively, use CUMIPMT: `=CUMIPMT(0.06/12, 360, 200000, 1, 12, 0)` = -$11,933.19. This first-year interest total is typically tax-deductible for primary residences, making it crucial for tax planning.

---

### Example 6: Car Loan Interest Breakdown
```
=IPMT(0.049/12, 1, 60, 25000)
```
**Result:** `-$102.08`

**Explanation:** First month's interest on a $25,000 car loan at 4.9% APR over 5 years. The total payment would be about $471, meaning $369 goes to principal from day one. Car loans with shorter terms and lower rates have much better principal-to-interest ratios than mortgages.

---

### Example 7: Beginning-of-Period Payment
```
=IPMT(0.06/12, 1, 360, 200000, 0, 1)
```
**Result:** `-$995.02`

**Explanation:** With beginning-of-period payments (type=1, like rent), the first payment includes slightly less interest because the payment is made before a full period's interest accrues. The difference is small (about $5) but mathematically correct. Leases often use this timing.

---

### Example 8: Investment/Annuity Interest Received
```
=IPMT(0.05/12, 1, 240, -500000, 0)
```
**Result:** `$2,083.33`

**Explanation:** If you invest $500,000 (negative pv, money you put in) and receive periodic payments, IPMT returns positive interest--money coming to you. The first month's interest earned on a $500,000 annuity at 5% is $2,083. This models retirement income from investments.

---

### Example 9: Midpoint of Loan Analysis
```
Period 180 of 360:
Interest: =IPMT(0.06/12, 180, 360, 200000) = -$614.81
Principal: =PPMT(0.06/12, 180, 360, 200000) = -$584.29
Total: =PMT(0.06/12, 360, 200000) = -$1,199.10
```
**Result:** At halfway point, interest and principal are roughly equal

**Explanation:** Midway through a loan, the payment splits roughly equally between interest and principal. Before this point, interest dominates; after, principal dominates. This "crossover point" varies with rate--higher rates push it later in the loan.

---

### Example 10: Balloon Payment Loan Interest
```
=IPMT(0.07/12, 60, 60, 300000, -100000)
```
**Result:** `-$629.92`

**Explanation:** A loan with a $100,000 balloon payment at the end (fv = -100000) has different amortization. The 60th payment's interest is calculated on the remaining balance before the balloon. Monthly payments are lower because you are not fully amortizing.

---

### Example 11: Verify PMT = IPMT + PPMT
```
PMT: =PMT(0.06/12, 360, 200000) = -$1,199.10
IPMT: =IPMT(0.06/12, 25, 360, 200000) = -$964.88
PPMT: =PPMT(0.06/12, 25, 360, 200000) = -$234.22
Sum: -$964.88 + -$234.22 = -$1,199.10
```
**Result:** IPMT + PPMT always equals PMT

**Explanation:** This identity holds for every period and serves as a validation check. If IPMT + PPMT does not equal PMT, you have an error in your parameters. Use this relationship to verify amortization table accuracy.

---

### Example 12: Period Out of Range Error
```
=IPMT(0.06/12, 0, 360, 200000)
=IPMT(0.06/12, 361, 360, 200000)
```
**Result:** Both return `#NUM!`

**Explanation:** The period (per) must be between 1 and nper inclusive. Period 0 does not exist (no payment before the first payment), and period 361 exceeds the 360-month term. These are input validation errors.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Period (per) is 0, negative, or greater than nper | Ensure 1 <= per <= nper. First payment is period 1. |
| `#VALUE!` | Non-numeric inputs | Verify all parameters are numbers. Check for text in cells. |
| `Positive when expecting negative` | Sign confusion or annuity scenario | For loans with positive pv, IPMT is negative. Positive results occur with negative pv (investments). |
| `Interest seems too high` | Rate not divided for period | Monthly payments require monthly rate: annual rate / 12. |
| `Doesn't match statement` | Rounding or actual vs. scheduled | Loan servicers round payments to cents, causing minor cumulative differences. |
| `Interest is $0` | Zero interest rate | At 0% rate, there is no interest--all payment goes to principal. |

## Use Cases

### [[Annual Tax Deduction Calculation]]

**Scenario:** A homeowner needs to calculate total mortgage interest paid in the current tax year (January through December payments) for Schedule A itemized deductions.

**Implementation:**
```
Loan details: $350,000 at 5.5%, 30-year, currently in year 3

Using CUMIPMT for efficiency:
Year 3 interest: =CUMIPMT(0.055/12, 360, 350000, 25, 36, 0)
```
**Result:** `-$18,456.72` (deductible interest)

**Business Application:** Tax professionals and homeowners use IPMT/CUMIPMT to project and verify mortgage interest deductions. This is especially important after life events like home purchases, refinances, or when deciding between standard and itemized deductions.

**Technical Details:** The period range (25-36) represents months 25 through 36 of the loan if the loan started in January two years prior. Adjust period numbers based on actual loan origination date. Form 1098 from your lender should match this calculation.

---

### [[Amortization Schedule Construction]]

**Scenario:** A financial analyst needs to build a complete amortization schedule showing payment breakdown, running balance, and cumulative interest for a business loan.

**Implementation:**
```
For each row (period 1 to nper):
Payment: =PMT($B$1/12, $B$2, $B$3)
Interest: =IPMT($B$1/12, A5, $B$2, $B$3)  where A5 = period number
Principal: =PPMT($B$1/12, A5, $B$2, $B$3)
Balance: =Previous_Balance + PPMT(...)
Cumulative Interest: =Previous_Cumulative + IPMT(...)
```

**Business Application:** Banks, loan servicers, and accounting departments use amortization schedules for payment processing, customer statements, and financial reporting. IPMT is essential because GAAP requires separating interest expense from principal reduction in financial statements.

**Technical Details:** For efficiency with large schedules, consider computing the opening balance first, then calculating each period's interest as `Balance * Rate` rather than calling IPMT repeatedly. However, IPMT provides built-in validation that your schedule matches standard amortization.

---

### [[Loan Comparison Analysis]]

**Scenario:** A borrower is comparing a 15-year mortgage at 5.0% versus a 30-year mortgage at 5.5% and wants to understand the interest cost difference, especially in early years.

**Implementation:**
```
15-year at 5.0% on $300,000:
Year 1 interest: =CUMIPMT(0.05/12, 180, 300000, 1, 12, 0) = -$14,482.65
Total interest: =CUMIPMT(0.05/12, 180, 300000, 1, 180, 0) = -$127,028.46

30-year at 5.5% on $300,000:
Year 1 interest: =CUMIPMT(0.055/12, 360, 300000, 1, 12, 0) = -$16,232.17
Total interest: =CUMIPMT(0.055/12, 360, 300000, 1, 360, 0) = -$313,212.29

Difference in total interest: $186,183.83 more for 30-year
```

**Business Application:** Mortgage brokers and financial advisors use interest breakdowns to help clients make informed decisions. The dramatic difference in total interest ($186K) often convinces clients that the higher monthly payment of a 15-year term is worthwhile if affordable.

**Technical Details:** Note that year 1 interest for the 30-year loan is higher ($16,232 vs $14,482) despite similar rates because more principal remains outstanding. The 15-year loan forces faster principal paydown, reducing the base on which interest is calculated.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Standard TVM calculation. Period must be integer between 1 and nper.
- **Precision:** IEEE 754 double-precision floating point.

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Same parameter order and return value.
- **Precision:** Same as Excel.

### Both Platforms
- IPMT returns negative for loans (interest you pay out)
- IPMT returns positive for investments (interest you receive)
- IPMT + PPMT = PMT for any period (fundamental identity)
- Period parameter is 1-indexed (first payment = period 1)

## Tips and Best Practices

1. **Remember IPMT + PPMT = PMT:** This identity is your validation check. For any period, interest plus principal must equal the total payment. If your amortization table fails this test, you have a formula error.

2. **Use CUMIPMT for totals:** When you need interest across multiple periods (annual, quarterly, loan-to-date), CUMIPMT is cleaner than summing individual IPMT calls. It is also faster computationally.

3. **Early payments have maximum interest impact:** Because early payments are interest-heavy, extra payments early in the loan have outsized effects on total interest. Use IPMT to illustrate this to clients considering prepayment.

4. **Match rate to period frequency:** For monthly payments, use rate/12. This is the most common error and produces dramatically wrong results if overlooked.

5. **Negate for positive display:** Financial reports often show interest as positive numbers. Use `-IPMT(...)` or `ABS(IPMT(...))` for display, but remember the conceptual direction--interest is money leaving you.

6. **Consider type parameter for leases:** Lease payments at the beginning of each period (type=1) calculate interest slightly differently. Most loans use end-of-period (type=0), but verify your specific situation.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PPMT]] | Principal portion of a payment | When you need the principal portion rather than interest |
| [[CUMIPMT]] | Cumulative interest over period range | When you need total interest across multiple periods |
| [[PMT]] | Total payment amount | When you need the complete payment, not just the interest portion |
| [[ISPMT]] | Simple interest for period | For simple interest calculations (not standard amortization) |

### Commonly Used Together

**[[PPMT]]** - Principal Portion

*Complete payment breakdown:*
```
Interest: =IPMT(0.06/12, 1, 360, 200000) = -$1,000.00
Principal: =PPMT(0.06/12, 1, 360, 200000) = -$199.10
Total: =PMT(0.06/12, 360, 200000) = -$1,199.10
```
IPMT and PPMT together provide the complete payment split.

---

**[[CUMIPMT]]** - Cumulative Interest

*For period range totals:*
```
Single period: =IPMT(0.06/12, 36, 360, 200000) = -$949.25
Year 3 total: =CUMIPMT(0.06/12, 360, 200000, 25, 36, 0) = -$11,347.58
```
CUMIPMT is more efficient than summing 12 IPMT calls.

---

**[[FV]]** - Remaining Balance

*Calculate balance at any point:*
```
Balance after 60 payments: =FV(0.06/12, 60, -PMT(0.06/12,360,200000), 200000)
Interest on period 61: =IPMT(0.06/12, 61, 360, 200000)
Alternative: Balance_60 * 0.06/12 = Interest_61
```
FV can verify IPMT calculations by computing the outstanding balance.

## Official Documentation

- **Microsoft Excel:** [IPMT function](https://support.microsoft.com/en-us/office/ipmt-function-5cce0ad6-8402-4a41-8d29-61a0b054cb6f)
- **Google Sheets:** [IPMT function](https://support.google.com/docs/answer/3093175)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 (1997) | Original implementation with all 6 parameters |
| Excel 2007+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
