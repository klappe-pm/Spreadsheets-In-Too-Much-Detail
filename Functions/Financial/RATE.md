---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- interest-rate-calculation
- time-value-of-money
- loan-analysis
- investment-returns
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Interest Rate
- Rate of Return
- APR Calculator
tags:
- financial-modeling
- loan-analysis
- investment-analysis
- interest-calculation
---

# RATE

## Description

**RATE** calculates the interest rate per period required to pay off a loan or achieve an investment goal, given regular payments, a present value, and optionally a future value. This function is essential when you know the payment amount, loan principal, and term but need to determine what interest rate makes those numbers work together. It answers questions like "What rate am I actually paying on this loan?" or "What return do I need to reach my savings goal?"

The function uses an iterative calculation (Newton-Raphson method) to find the rate that balances the time value of money equation. Unlike most financial functions that have direct formulas, RATE must solve the equation through successive approximations because the rate appears in an exponential term that cannot be isolated algebraically. This is why RATE includes an optional `guess` parameter--the algorithm needs a starting point, and a good initial guess helps it converge faster and more reliably.

**Understanding sign conventions is critical:** Cash flowing OUT of your pocket is negative, and cash flowing IN is positive. For a loan: the principal you receive is positive (pv), but the payments you make are negative (pmt). For savings: deposits are negative (money leaving you), and the future value you accumulate is positive. If you get signs wrong, RATE may return #NUM! errors or impossible rates. The returned rate is per period, so if you used monthly payments, multiply by 12 to get the annual rate.

RATE is available in all versions of Excel and Google Sheets with identical behavior. The function may fail to converge (returning #NUM!) if the payment is too low to ever pay off the loan or if the combination of values is mathematically impossible. In such cases, try providing a reasonable guess parameter. The iterative calculation runs up to 20 times, and if convergence is not achieved within a tolerance of 0.0000001, the function returns an error.

## Syntax

> [!f(x)] RATE Syntax
>
> ```
> =RATE(nper, pmt, pv, [fv], [type], [guess])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `nper` | Yes | The total number of payment periods. For a 5-year loan with monthly payments, use 60. Must match the period of your payment amount. |
| `pmt` | Yes | The payment made each period. Must be constant throughout. Enter as negative for payments you make (loan payments, savings deposits). |
| `pv` | Yes | The present value--the principal amount of a loan (positive) or current balance of savings (negative if you have already invested). |
| `fv` | No | The future value--the balance after the last payment. For loans, usually 0 (fully paid). For savings, your target amount. Defaults to 0. |
| `type` | No | When payments are due: 0 (or omitted) = end of period; 1 = beginning of period. |
| `guess` | No | Your estimate of the rate. Defaults to 0.1 (10%). Provide a guess closer to the expected answer if the function returns #NUM! or takes too long. |

### Return Value

Returns the interest rate per period as a decimal. For monthly payments, this is the monthly rate--multiply by 12 for annual rate. For quarterly payments, multiply by 4. The rate can be positive (you pay interest), zero (interest-free), or negative (rare, but possible if you receive a subsidy).

## Examples

> [!f(x)] RATE Examples

### Example 1: Find Loan Interest Rate from Payment
```
=RATE(60, -400, 20000)
```
**Result:** `0.00768` (0.768% per month, or 9.22% annually)

**Explanation:** Given a $20,000 loan paid off with $400 monthly payments over 60 months, finds the implied interest rate. The monthly rate of 0.768% multiplied by 12 equals 9.22% APR. Payment is negative because it is money you pay out. This is how you verify whether a dealer's quoted rate matches their payment amount.

---

### Example 2: Convert Monthly Rate to Annual Rate
```
=RATE(60, -400, 20000) * 12
```
**Result:** `0.0922` (9.22% annual rate)

**Explanation:** RATE returns the per-period rate. For monthly payments, multiply by 12 to get the annual percentage rate (APR). This annual rate is what lenders are required to disclose. Note: this is not the same as APY (Annual Percentage Yield), which accounts for compounding.

---

### Example 3: Mortgage Rate Verification
```
=RATE(360, -1500, 250000) * 12
```
**Result:** `0.0549` (5.49% annual rate)

**Explanation:** Verifies the interest rate on a 30-year mortgage where payments are $1,500/month on a $250,000 loan. This helps borrowers check whether they are getting the rate they were promised. If the calculated rate differs from the quoted rate, ask the lender about fees rolled into the loan.

---

### Example 4: Required Investment Return Rate
```
=RATE(360, -500, 0, 1000000) * 12
```
**Result:** `0.0791` (7.91% annual return needed)

**Explanation:** What annual return do you need to accumulate $1,000,000 by saving $500/month for 30 years? The answer is 7.91%. If this seems unrealistic for your investment strategy, you need to save more or extend your timeline. The pv is 0 (starting from nothing) and fv is your goal.

---

### Example 5: Rate with Starting Balance
```
=RATE(240, -600, -100000, 800000) * 12
```
**Result:** `0.0523` (5.23% annual return needed)

**Explanation:** Starting with $100,000 (negative because it is money already invested), contributing $600/month for 20 years to reach $800,000. What return is needed? About 5.23% annually. Both pv and pmt are negative because they represent money you have put or will put in.

---

### Example 6: Zero-Interest Loan Verification
```
=RATE(48, -250, 12000)
```
**Result:** `0.0000` (0% interest)

**Explanation:** If $12,000 is paid off with exactly $250/month for 48 months ($250 x 48 = $12,000), the rate is zero. This verifies a true 0% financing offer. If the rate comes back positive despite a "0% APR" advertisement, hidden fees may have been added to the principal.

---

### Example 7: Car Lease Implicit Rate
```
=RATE(36, -350, 25000, -10000) * 12
```
**Result:** `0.0612` (6.12% implicit rate)

**Explanation:** A car worth $25,000 is leased for $350/month for 3 years with a $10,000 residual value. What is the implicit interest rate? About 6.12% annually. The residual value is negative because you would need to pay it to own the car (or it is a liability from your perspective). This helps compare lease offers.

---

### Example 8: Beginning-of-Period Payments
```
=RATE(60, -400, 20000, 0, 1) * 12
```
**Result:** `0.0892` (8.92% annual rate)

**Explanation:** Same as Example 1, but with beginning-of-period payments (type=1). The rate is slightly lower because payments made earlier reduce principal faster. The difference is about 0.3% annually. Lease payments typically use beginning-of-period timing.

---

### Example 9: Using Guess Parameter
```
=RATE(120, -100, 15000, 0, 0, 0.005)
```
**Result:** `0.00599` (0.599% per month, 7.19% annually)

**Explanation:** When the default guess of 10% is far from the actual rate, providing a closer guess helps the function converge. Here, $100/month barely covers a high-rate loan--the guess of 0.5% monthly helps find the 0.599% answer faster.

---

### Example 10: Annuity Payout Rate
```
=RATE(300, 3500, -750000, 0, 1) * 12
```
**Result:** `0.0378` (3.78% annual return assumption)

**Explanation:** An insurance company offers $3,500/month for 25 years (300 months) for a $750,000 premium (negative because you pay it). What return does the annuity assume? About 3.78%. This helps compare annuity offers--a higher implied rate means better value. Type=1 because annuity payments typically start immediately.

---

### Example 11: Effective Rate on Installment Purchase
```
=RATE(12, -95, 999) * 12
```
**Result:** `0.1677` (16.77% annual rate)

**Explanation:** A store offers a $999 item for "just $95/month for 12 months." What is the real interest rate? A steep 16.77% annually! Total paid: $1,140. This calculation exposes the true cost of "easy payment plans" that do not disclose APR prominently.

---

### Example 12: Rate Function Returns Error
```
=RATE(60, -100, 20000)
```
**Result:** `#NUM!`

**Explanation:** $100/month for 60 months = $6,000 total, but the loan is $20,000. The payment is too small to ever pay off this loan at any positive interest rate, so RATE cannot find a solution. This error indicates an impossible scenario--either the payment, term, or principal is wrong.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | No rate can balance the equation--payment too low, impossible scenario | Verify that payments over nper can cover the principal. Check sign conventions. Try providing a guess parameter closer to expected rate. |
| `#NUM!` | Convergence failure after 20 iterations | Provide a guess closer to the expected answer. Very high or very low rates may need specific guesses. |
| `#VALUE!` | Non-numeric inputs | Ensure all parameters are numbers. Check for text, hidden characters, or empty cells. |
| `Rate is negative` | Sign convention confusion or unusual scenario | Double-check signs: loan principal positive, payments negative. Negative rates can be valid for subsidized scenarios. |
| `Rate seems wrong` | Period mismatch | If payments are monthly, RATE returns monthly rate--multiply by 12 for annual. |
| `Different from quoted APR` | Fees or different compounding | Lenders may quote rates differently. True APR includes fees; nominal vs. effective rate differences apply. |

## Use Cases

### [[Loan Shopping and Rate Comparison]]

**Scenario:** A car buyer receives three financing offers and wants to compare them fairly. Dealer A offers $25,000 financed at "$475/month for 60 months." Dealer B says "5.9% APR." Dealer C offers "$22,500 after $2,500 cash back at $425/month for 60 months."

**Implementation:**
```
Dealer A: =RATE(60, -475, 25000) * 12 = 6.24% APR
Dealer B: Quoted 5.9% APR
Dealer C: =RATE(60, -425, 22500) * 12 = 6.91% APR
```

**Business Application:** Car dealerships and finance managers often quote monthly payments rather than rates because lower payments sound attractive. RATE reveals the true cost of financing, enabling apples-to-apples comparison. In this case, Dealer B's explicit 5.9% is the best deal despite potentially higher monthly payments.

**Technical Details:** The calculation assumes simple interest amortization. Some dealers use "add-on" interest or other methods that produce different effective rates. Always verify by comparing total interest paid (payment x nper - principal) across offers.

---

### [[Investment Return Analysis]]

**Scenario:** An investment fund reports that a $50,000 investment made 10 years ago is now worth $125,000, with the investor having added $500/month throughout. What was the actual annual return?

**Implementation:**
```
=RATE(120, -500, -50000, 125000) * 12
```
**Result:** `6.47%` annual return

**Business Application:** Portfolio managers and financial advisors use RATE to calculate the dollar-weighted return (also called IRR or money-weighted return) on investments where additional contributions were made over time. This differs from time-weighted return and often provides a more meaningful measure of individual investor experience.

**Technical Details:** The calculation accounts for the timing of contributions--money added later has less time to grow. Compare this to stated fund returns, which are time-weighted and do not account for when your specific dollars were invested.

---

### [[Retirement Planning - Required Return]]

**Scenario:** A 45-year-old has $200,000 saved for retirement, plans to add $1,000/month for 20 years, and wants to retire with $1,500,000. What return rate is needed?

**Implementation:**
```
=RATE(240, -1000, -200000, 1500000) * 12
```
**Result:** `6.89%` annual return needed

**Business Application:** Financial planners use this calculation to reality-check client expectations. If the required return exceeds historical norms for the client's risk tolerance (e.g., needing 10% but only willing to accept bond-level risk), the plan needs adjustment. RATE provides the specific return target for investment allocation decisions.

**Technical Details:** Historical equity returns average 7-10% long-term, but with significant volatility. Required returns above 8-9% suggest the client may need to save more, work longer, or adjust their retirement lifestyle expectations. Use conservative estimates for planning safety margins.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Uses Newton-Raphson iteration with up to 20 attempts. Convergence tolerance is 0.0000001. Default guess is 0.1.
- **Precision:** IEEE 754 double-precision floating point.

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Same iterative algorithm and tolerance.
- **Precision:** Same as Excel.

### Both Platforms
- RATE returns per-period rate--multiply by periods per year for annual rate
- Negative pmt for payments you make, positive pv for loan principal
- May require guess parameter for extreme rate scenarios
- Returns #NUM! for mathematically impossible inputs

## Tips and Best Practices

1. **Always multiply by periods per year for annual rate:** RATE returns the per-period rate. For monthly payments, multiply by 12 (not compound, just multiply). This gives you the APR, not APY.

2. **Check sign conventions carefully:** The most common error. Loan principal is positive (you receive it), payments are negative (you pay them). For savings, deposits are negative, and future value is positive.

3. **Use guess parameter when needed:** If RATE returns #NUM! or seems slow, provide a guess. For typical loans (3-15% annually), try 0.01 (1% monthly) as a guess. For very high or low rates, adjust accordingly.

4. **Verify results with PMT:** Cross-check by using the rate you found in PMT to see if it produces the same payment: `=PMT(found_rate, nper, pv)` should equal your original payment.

5. **Distinguish APR from APY:** RATE gives you the basis for APR (simple multiplication by periods). APY (Annual Percentage Yield) accounts for compounding: APY = (1 + RATE)^12 - 1 for monthly compounding.

6. **Watch for impossible scenarios:** If the total of all payments is less than the principal, no positive rate exists. RATE will return #NUM! because the math is impossible.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[IRR]] | Internal rate of return for irregular cash flows | When cash flows vary in amount (not constant payments) |
| [[XIRR]] | IRR with specific dates | When cash flows occur on irregular dates |
| [[EFFECT]] | Effective annual rate from nominal rate | To convert APR to APY |
| [[NOMINAL]] | Nominal rate from effective rate | To convert APY to APR |

### Commonly Used Together

**[[PMT]]** - Verify Rate Calculation

*Cross-check RATE result:*
```
Found rate: =RATE(60, -400, 20000) = 0.768%
Verify: =PMT(0.00768, 60, 20000) = -$400.00
```
If PMT returns your original payment, RATE is correct.

---

**[[EFFECT]]** - Convert to Effective Rate

*Get APY from RATE:*
```
Monthly rate: =RATE(60, -400, 20000) = 0.768%
APY: =EFFECT(0.00768*12, 12) = 9.62%
```
This shows the true annual yield considering monthly compounding.

---

**[[NPER]]** - Find Term for Target Rate

*Combined for loan planning:*
```
At 6% APR, how long to pay off $20,000 with $400/month?
=NPER(0.06/12, -400, 20000) = 58.1 months
```
Once you know rates, NPER finds the time dimension.

## Official Documentation

- **Microsoft Excel:** [RATE function](https://support.microsoft.com/en-us/office/rate-function-9f665657-4a7e-4bb7-a030-83fc59e748ce)
- **Google Sheets:** [RATE function](https://support.google.com/docs/answer/3093222)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 (1997) | Original implementation with all 6 parameters |
| Excel 2007+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
