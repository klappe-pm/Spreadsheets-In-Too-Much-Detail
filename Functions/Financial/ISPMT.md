---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- loan-calculations
- interest-payments
- equal-principal
- debt-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Interest Payment Simple
- Equal Principal Interest
- Straight-Line Loan Interest
tags:
- loan-calculations
- interest-payments
- amortization
- debt-management
- equal-principal-payment
---

# ISPMT

## Description

**ISPMT (Interest Payment - Simple)** calculates the interest portion of a loan payment for a specific period when the loan uses equal principal payments (also called straight-line amortization). This is fundamentally different from the standard IPMT function, which calculates interest for loans with equal total payments (standard amortization). In an equal principal payment loan, each payment reduces the principal by the same fixed amount, so interest charges decrease over time as the outstanding balance decreases.

In a standard amortizing loan (calculated with IPMT), the total payment remains constant while the interest portion decreases and principal portion increases over time. In an equal principal payment loan (calculated with ISPMT), the principal payment is constant, but total payments decrease over time because interest is calculated only on the remaining balance. For example, a $12,000 loan over 12 months with equal principal payments would have $1,000 principal each month, plus interest only on the remaining balance.

**Key distinction from IPMT:** IPMT assumes equal total payments (standard mortgage-style amortization). ISPMT assumes equal principal payments (straight-line principal reduction). The choice between them depends on the loan structure. Some commercial loans, international loan products, and certain consumer loans use equal principal payment structures.

ISPMT is available in both Excel and Google Sheets. The function returns a negative value because interest represents a payment flowing out from the borrower's perspective. Some users prefer to negate the result for display purposes. The function is particularly useful for loans in European and Asian markets where equal principal payment structures are more common than in the United States.

## Syntax

> [!f(x)] ISPMT Syntax
>
> ```
> =ISPMT(rate, per, nper, pv)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rate` | Yes | The interest rate per period. For a 6% annual rate with monthly payments, use 0.06/12 = 0.005. Must match the payment period frequency. |
| `per` | Yes | The specific period for which to calculate interest. Period 1 is the first period, period 2 is the second, etc. Must be between 1 and nper inclusive. |
| `nper` | Yes | The total number of payment periods for the loan. For a 3-year loan with monthly payments, nper = 36. |
| `pv` | Yes | The present value (principal amount) of the loan. This is the total amount borrowed. Typically entered as a positive number. |

### Return Value

Returns a negative numeric value representing the interest portion of the payment for the specified period. The result is negative because interest is a cash outflow from the borrower. To display as a positive number, negate the result: `-ISPMT(...)` or `ABS(ISPMT(...))`.

## Examples

> [!f(x)] ISPMT Examples

### Example 1: First Payment Interest on Equal Principal Loan
```
=ISPMT(0.10/12, 1, 24, 10000)
```
**Result:** `-$83.33`

**Explanation:** For a $10,000 loan at 10% annual rate (0.833% monthly) over 24 months with equal principal payments. In the first month, the full $10,000 is outstanding, so interest is $10,000 x (0.10/12) = $83.33 (shown as negative).

---

### Example 2: Last Payment Interest
```
=ISPMT(0.10/12, 24, 24, 10000)
```
**Result:** `-$3.47`

**Explanation:** By month 24, most principal is paid. With equal principal payments of $416.67 per month, only $416.67 remains before the final payment. Interest on $416.67 at 0.833% = $3.47.

---

### Example 3: Compare First and Last Interest Payments
```
First: =ISPMT(0.08/12, 1, 36, 50000) = -$333.33
Last: =ISPMT(0.08/12, 36, 36, 50000) = -$9.26
```
**Result:** Significant decline in interest

**Explanation:** A $50,000 loan at 8% over 36 months. First month interest ($333.33) is much higher than last month ($9.26) because the balance has been steadily reduced by equal principal payments of $1,388.89 per month.

---

### Example 4: Mid-Loan Interest Payment
```
=ISPMT(0.06/12, 30, 60, 100000)
```
**Result:** `-$258.33`

**Explanation:** For a $100,000 loan at 6% over 60 months, calculating interest at month 30 (halfway point). By month 30, half the principal ($50,000) has been paid, so remaining balance is $51,666.67, and interest is approximately $258.33.

---

### Example 5: Displaying as Positive
```
=-ISPMT(0.09/12, 6, 48, 25000)
```
**Result:** `$163.02`

**Explanation:** By negating ISPMT, the result displays as a positive number, which some prefer for reports and schedules. The interest for month 6 of this $25,000 loan is $163.02.

---

### Example 6: Annual Payments
```
=ISPMT(0.07, 1, 5, 200000)
```
**Result:** `-$14,000`

**Explanation:** A $200,000 loan with annual payments over 5 years at 7%. In year 1, the full $200,000 is outstanding, so annual interest is $200,000 x 0.07 = $14,000.

---

### Example 7: Building Interest Schedule
```
Period 1: =ISPMT($B$1/12, 1, $B$2, $B$3)
Period 2: =ISPMT($B$1/12, 2, $B$2, $B$3)
...
Period N: =ISPMT($B$1/12, N, $B$2, $B$3)
```
**Result:** Declining interest amounts

**Explanation:** Building a complete interest schedule for an equal principal loan. Each period's interest is calculated separately, and the amounts will decrease linearly because the outstanding balance decreases by a constant amount each period.

---

### Example 8: Total Interest Over Loan Life
```
=SUMPRODUCT(ISPMT(0.08/12, ROW(INDIRECT("1:36")), 36, 50000))
```
**Result:** `-$6,166.67`

**Explanation:** Sums interest for all 36 periods of a $50,000 loan. This gives total interest paid over the loan life, useful for comparing equal principal loans to standard amortizing loans.

---

### Example 9: Quarterly Payments
```
=ISPMT(0.12/4, 4, 20, 80000)
```
**Result:** `-$2,040`

**Explanation:** For a $80,000 loan at 12% with quarterly payments over 5 years (20 quarters). Quarter 4 interest calculation: Outstanding balance at quarter 4 = $68,000 (after 3 payments of $4,000 each). Interest = $68,000 x 0.03 = $2,040.

---

### Example 10: Comparing ISPMT to IPMT
```
Equal Principal (ISPMT): =ISPMT(0.06/12, 1, 60, 100000) = -$500.00
Equal Payment (IPMT):    =IPMT(0.06/12, 1, 60, 100000) = -$500.00
```
**Result:** Same for first period

**Explanation:** For the first period, ISPMT and IPMT produce the same result because the full principal is outstanding. Differences emerge in subsequent periods because the payment structures differ.

---

### Example 11: Period 2 Comparison - ISPMT vs IPMT
```
Equal Principal (ISPMT): =ISPMT(0.06/12, 2, 60, 100000) = -$491.67
Equal Payment (IPMT):    =IPMT(0.06/12, 2, 60, 100000) = -$497.93
```
**Result:** Different values

**Explanation:** By period 2, ISPMT shows lower interest ($491.67) than IPMT ($497.93) because equal principal payments reduce the balance faster initially. The difference grows in later periods.

---

### Example 12: Calculate Total Payment (Principal + Interest)
```
Principal Payment: =PV/NPER = 100000/60 = $1,666.67
Interest Payment: =-ISPMT(0.06/12, 1, 60, 100000) = $500.00
Total Payment Month 1: =$1,666.67 + $500.00 = $2,166.67
```
**Result:** `$2,166.67` (first month total payment)

**Explanation:** In an equal principal loan, the principal payment is constant (PV/NPER). Add the ISPMT result to get total payment. Note that total payments decrease each month as interest decreases.

---

### Example 13: Interest Rate Sensitivity
```
At 5%: =ISPMT(0.05/12, 1, 36, 30000) = -$125.00
At 7%: =ISPMT(0.07/12, 1, 36, 30000) = -$175.00
At 9%: =ISPMT(0.09/12, 1, 36, 30000) = -$225.00
```
**Result:** Interest scales with rate

**Explanation:** For the first period, interest is directly proportional to the rate (since principal is constant). A 2% rate increase adds $50/month to first payment interest for this $30,000 loan.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric inputs or text in parameters | Ensure all parameters (rate, per, nper, pv) are numeric values. |
| `#NUM!` | Period (per) is less than 1 or greater than nper | Period must be between 1 and total periods inclusive. |
| `#NUM!` | nper is zero or negative | Number of periods must be a positive integer. |
| `Unexpected large value` | Rate entered as percentage instead of decimal | Enter 0.06 for 6%, not 6. Using 6 means 600% annual rate. |
| `Positive when expecting negative` | Sign convention misunderstanding | ISPMT returns negative (cash outflow). Negate or use ABS() if you need positive values. |
| `Different from IPMT` | Using wrong function for loan type | ISPMT is for equal principal loans; IPMT is for equal payment loans. Verify your loan structure. |

## Use Cases

### [[Commercial Loan Interest Tracking]]

**Scenario:** A commercial lender offers loans with equal principal payments and needs to generate interest schedules for borrowers showing the declining interest amounts over the loan term.

**Implementation:**
```
Loan Amount: $500,000
Rate: 8% annual
Term: 5 years (60 monthly payments)
Monthly Principal: $500,000 / 60 = $8,333.33

Month 1 Interest: =-ISPMT(0.08/12, 1, 60, 500000) = $3,333.33
Month 1 Total: $8,333.33 + $3,333.33 = $11,666.67
...
Month 60 Interest: =-ISPMT(0.08/12, 60, 60, 500000) = $55.56
Month 60 Total: $8,333.33 + $55.56 = $8,388.89
```

**Business Application:** Commercial real estate loans and business equipment financing often use equal principal payment structures. Borrowers appreciate the declining payment amount, especially businesses with predictable expense budgets. Lenders use ISPMT to generate accurate payment schedules and interest income projections.

**Technical Details:** Create a complete amortization schedule by calculating ISPMT for each period and adding the constant principal payment. Track remaining balance (decreases by principal payment each period) for verification. Total of all ISPMT values equals total interest paid.

---

### [[International Loan Products Analysis]]

**Scenario:** A multinational bank is comparing loan products across markets. European and Asian markets often use equal principal payment structures, while US markets typically use equal total payment structures.

**Implementation:**
```
Loan: $100,000 at 6% for 5 years

US-Style (Equal Payment):
Monthly Payment: =PMT(0.06/12, 60, 100000) = -$1,933.28 (constant)
Total Interest: =$1,933.28 * 60 - $100,000 = $15,996.80

European-Style (Equal Principal):
Monthly Principal: $1,666.67
First Payment: $1,666.67 + (-ISPMT(0.06/12, 1, 60, 100000)) = $2,166.67
Last Payment: $1,666.67 + (-ISPMT(0.06/12, 60, 60, 100000)) = $1,675.00
Total Interest: =SUMPRODUCT(-ISPMT(0.06/12, ROW(INDIRECT("1:60")), 60, 100000)) = $15,250.00
```

**Business Application:** Different loan structures have different implications for borrowers and lenders. Equal principal loans have higher early payments but lower total interest. This comparison helps product managers price loans and helps borrowers choose appropriate products.

**Technical Details:** Total interest for equal principal loans is always less than equal payment loans (same rate/term) because principal is paid down faster. The difference is more pronounced for longer terms and higher rates.

---

### [[Budget Planning with Declining Payments]]

**Scenario:** A CFO is planning cash flows for a company that has borrowed using an equal principal payment structure and needs to forecast the declining payment amounts for budgeting purposes.

**Implementation:**
```
Loan Details: $1,000,000, 7%, 10-year term

Year 1 Total Interest: =12 * AVERAGE(ISPMT(0.07/12, {1,2,3,4,5,6,7,8,9,10,11,12}, 120, 1000000))
Year 2 Total Interest: Calculated similarly for months 13-24
...

Or calculate each month and aggregate:
=SUMPRODUCT(-ISPMT(0.07/12, ROW(INDIRECT("1:12")), 120, 1000000))
```

**Business Application:** Companies with equal principal payment debt can benefit from predictable declining cash outflows. This is advantageous for businesses expecting growth--higher payments early when cash flow may be tighter are offset by lower payments later when the business has grown.

**Technical Details:** Create a year-by-year interest expense projection for financial planning. Interest expense for accounting purposes should match ISPMT calculations. For tax planning, understand when interest becomes deductible (generally when paid or accrued depending on accounting method).

---

### [[Consumer Auto Loan Comparison]]

**Scenario:** A consumer is comparing two auto loan offers--one with traditional equal payments and one with equal principal payments--to understand the total cost and cash flow differences.

**Implementation:**
```
Loan: $25,000, 5% APR, 4 years

Option A (Equal Payment):
Monthly Payment: =PMT(0.05/12, 48, 25000) = -$575.73
Total Paid: $575.73 x 48 = $27,635.04
Total Interest: $2,635.04

Option B (Equal Principal):
Monthly Principal: $520.83
Month 1 Total: $520.83 + (-ISPMT(0.05/12, 1, 48, 25000)) = $625.00
Month 48 Total: $520.83 + (-ISPMT(0.05/12, 48, 48, 25000)) = $522.92
Total Interest: $2,552.08 (lower than Option A)
```

**Business Application:** While equal payment loans are standard in the US, some lenders offer equal principal options. Consumers who can afford higher early payments may prefer equal principal loans for lower total interest. ISPMT enables accurate comparison.

**Technical Details:** Equal principal loans have $83 less total interest in this example, but require $49 higher payment in month 1. The breakeven depends on opportunity cost of early payments. Create a present value analysis to determine which option is truly better for the borrower's situation.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 through Excel 365 and beyond)
- **Specific Behavior:** Returns negative value for interest payments (outflow convention)
- **Note:** ISPMT uses a different calculation method than IPMT--they are for different loan structures

### Google Sheets

- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel
- **Note:** Same sign convention (negative for outflows)

### Both Platforms

- Period parameter (per) must be between 1 and nper inclusive
- Rate must match payment period (monthly rate for monthly payments)
- Result is negative (cash outflow from borrower's perspective)
- Does not include principal portion--only calculates interest

## Tips and Best Practices

1. **Understand the loan structure:** ISPMT is specifically for equal principal payment loans. For standard amortizing loans with equal total payments, use IPMT instead. Verify your loan structure before choosing.

2. **Match rate to period:** If you have monthly payments, use monthly rate (annual rate / 12). Annual payments use annual rate. Mismatched periods produce incorrect results.

3. **Remember the sign convention:** ISPMT returns negative values representing cash outflow. Use `-ISPMT(...)` or `ABS(ISPMT(...))` if you want positive values for display.

4. **Calculate total payments correctly:** Total payment = Principal payment (PV/NPER) + Interest (ISPMT result). In equal principal loans, principal payment is constant; interest (and thus total payment) decreases over time.

5. **Compare to IPMT for analysis:** For the same loan parameters, ISPMT will show lower total interest than IPMT because principal is paid down faster. This comparison can inform borrowing decisions.

6. **Build complete schedules:** Create a period-by-period schedule showing principal, interest (ISPMT), total payment, and remaining balance. This helps borrowers and lenders track the loan accurately.

7. **Verify with manual calculation:** Interest for period N = (Beginning balance for period N) x (periodic rate). Beginning balance = Original principal - (N-1) x (Principal payment).

8. **Use for cash flow projections:** The declining payment characteristic of equal principal loans makes them suitable for businesses expecting growth or wanting to frontload debt repayment.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[IPMT]] | Interest payment for equal total payment loans | For standard amortizing loans (fixed monthly payment amount) |
| [[PPMT]] | Principal payment for equal total payment loans | To find principal portion of standard loan payments |
| [[PMT]] | Total payment for equal payment loans | To calculate constant payment for standard loans |

### Commonly Used Together

**[[IPMT]]** - Interest Payment (Equal Total Payments)

*Compare loan structures:*
```
Equal Principal: =ISPMT(0.06/12, 6, 60, 100000)
Equal Payment: =IPMT(0.06/12, 6, 60, 100000)
```
Compare interest for the same period under different loan structures.

---

**[[SUM]]** / **[[SUMPRODUCT]]** - Total Interest Calculation

*Calculate total interest over loan life:*
```
=SUMPRODUCT(-ISPMT(rate, ROW(INDIRECT("1:"&nper)), nper, pv))
```
Sum all period interest payments to find total interest cost.

---

**[[IF]]** - Conditional Payment Structure

*Choose calculation based on loan type:*
```
=IF(loan_type="Equal Principal", ISPMT(rate, per, nper, pv), IPMT(rate, per, nper, pv))
```
Dynamically select the correct interest function based on loan structure.

---

**[[ABS]]** - Display as Positive

*Convert to positive for display:*
```
=ABS(ISPMT(rate, per, nper, pv))
```
Display interest as a positive number in payment schedules.

---

**Simple Division** - Principal Payment

*Calculate principal portion:*
```
Principal each period: =pv/nper
Total Payment: =pv/nper + ABS(ISPMT(rate, per, nper, pv))
```
In equal principal loans, principal payment is constant (total loan / number of periods).

## Official Documentation

- **Microsoft Excel:** [ISPMT function](https://support.microsoft.com/en-us/office/ispmt-function-fa58adb6-9d39-4ce0-8f43-75399cea56cc)
- **Google Sheets:** [ISPMT function](https://support.google.com/docs/answer/3093192)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 (1997) | Original implementation |
| Excel 2007+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
