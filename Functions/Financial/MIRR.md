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
- modified-irr
- investment-analysis
- reinvestment-rate
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Modified IRR
- Modified Internal Rate of Return
- MIRR Function
tags:
- investment-analysis
- capital-budgeting
- return-calculation
- financial-modeling
---

# MIRR

## Description

**MIRR (Modified Internal Rate of Return)** calculates the internal rate of return for a series of cash flows, with explicit assumptions about the reinvestment rate for positive cash flows and the financing rate for negative cash flows. Unlike standard IRR which implicitly assumes cash flows are reinvested at the IRR itself (often unrealistically high), MIRR uses more realistic rates that you specify, producing a more conservative and often more accurate measure of investment return.

The MIRR formula works by: (1) discounting all negative cash flows to present value using the finance rate, (2) compounding all positive cash flows to future value using the reinvestment rate, and (3) finding the rate that equates these two values over the investment period. The formula is: MIRR = (FV of positives / PV of negatives)^(1/n) - 1, where n is the number of periods.

**Understanding why MIRR matters:** Traditional IRR assumes you can reinvest all cash inflows at the IRR rate. For a project with 25% IRR, this assumes you can reinvest interim cash flows at 25%--often unrealistic. MIRR lets you specify a reinvestment rate (perhaps your cost of capital or a risk-free rate) and a financing rate for additional investments or costs. This produces a more realistic return estimate.

MIRR is available in all versions of Excel and Google Sheets with identical behavior. The function is particularly valuable when comparing projects with different cash flow patterns, when projects have high IRRs that would be unrealistic reinvestment rates, or when projects have multiple sign changes (which can cause IRR to have multiple solutions). MIRR always has a unique solution, avoiding the multiple IRR problem.

## Syntax

> [!f(x)] MIRR Syntax
>
> ```
> =MIRR(values, finance_rate, reinvest_rate)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `values` | Yes | Array or range of cash flows. Must contain at least one negative and one positive value. First value is Period 0, second is Period 1, etc. |
| `finance_rate` | Yes | The interest rate paid on money used for negative cash flows (cost of borrowing or cost of capital). Express as decimal. |
| `reinvest_rate` | Yes | The interest rate earned on reinvested positive cash flows. Express as decimal. |

### Return Value

Returns the modified internal rate of return per period as a decimal. Multiply by 100 for percentage. The rate represents the periodic return (usually annual).

## Examples

> [!f(x)] MIRR Examples

### Example 1: Basic MIRR Calculation
```
Cash flows: -10000, 3000, 4000, 5000, 6000
Finance rate: 8%
Reinvestment rate: 10%

=MIRR({-10000, 3000, 4000, 5000, 6000}, 0.08, 0.10)
```
**Result:** `0.1518` (15.18%)

**Explanation:** With borrowing at 8% and reinvesting at 10%, the modified return is 15.18%. Compare to standard IRR of about 20%--MIRR is more conservative because it doesn't assume 20% reinvestment.

---

### Example 2: Comparing MIRR to IRR
```
Cash flows: -100000, 30000, 40000, 50000, 60000

IRR: =IRR({-100000,30000,40000,50000,60000}) = 22.7%
MIRR: =MIRR({-100000,30000,40000,50000,60000}, 0.08, 0.10) = 15.2%
```
**Result:** MIRR is significantly lower than IRR

**Explanation:** IRR assumes reinvestment at 22.7%. MIRR uses 10% reinvestment, which is more realistic. The 7.5% difference shows how much IRR overstates returns when reinvestment rates matter.

---

### Example 3: Equal Finance and Reinvestment Rates
```
=MIRR({-50000, 20000, 20000, 20000}, 0.10, 0.10)
```
**Result:** `0.0962` (9.62%)

**Explanation:** When both rates are equal, MIRR gives a consistent rate of return based on that single rate assumption. This is useful when using cost of capital for both rates.

---

### Example 4: High Finance Rate (Expensive Borrowing)
```
Cash flows: -100000, -20000, 50000, 80000, 100000
Finance rate: 15% (high borrowing cost)
Reinvestment rate: 8%

=MIRR({-100000,-20000,50000,80000,100000}, 0.15, 0.08)
```
**Result:** Lower MIRR due to expensive early financing

**Explanation:** The year-2 additional investment (-$20,000) is financed at 15%, reducing overall returns. Multiple negative cash flows are handled by compounding them back to present value at the finance rate.

---

### Example 5: Private Equity Fund Return
```
Fund cash flows over 7 years:
Year 0: -$1,000,000 (initial investment)
Year 1: -$500,000 (capital call)
Year 2: -$300,000 (capital call)
Year 3: $200,000 (distribution)
Year 4: $400,000 (distribution)
Year 5: $600,000 (distribution)
Year 6: $1,200,000 (exit)

Finance rate: 8% (cost of capital)
Reinvestment rate: 6% (safe reinvestment)

=MIRR(cash_flows, 0.08, 0.06)
```
**Result:** MIRR reflecting realistic reinvestment of distributions

**Explanation:** PE funds make distributions that investors must reinvest. Using 6% for reinvestment (perhaps a bond rate) gives a more realistic return than IRR, which assumes reinvestment at the (often 15-25%) IRR.

---

### Example 6: Project with Multiple IRR Problem
```
Cash flows: -100, 230, -132 (oil well scenario: invest, produce, cleanup)

IRR: =IRR({-100, 230, -132}) = either 10% or 20% (multiple solutions!)
MIRR: =MIRR({-100, 230, -132}, 0.08, 0.08) = 4.6% (unique solution)
```
**Result:** MIRR provides single, unambiguous answer

**Explanation:** Sign changes in cash flows can create multiple IRR solutions. MIRR always has exactly one solution, making it reliable for projects with non-conventional cash flow patterns.

---

### Example 7: Conservative vs. Aggressive Assumptions
```
Cash flows: -50000, 15000, 20000, 25000, 30000

Conservative (low reinvestment):
=MIRR(flows, 0.10, 0.05) = 10.8%

Moderate:
=MIRR(flows, 0.10, 0.10) = 13.5%

Aggressive (high reinvestment):
=MIRR(flows, 0.10, 0.15) = 16.1%
```
**Result:** Range of returns based on assumptions

**Explanation:** MIRR lets you test sensitivity to reinvestment assumptions. Present all three scenarios to stakeholders for informed decision-making.

---

### Example 8: Short-Term Project
```
6-month project:
Month 0: -$100,000
Month 3: +$50,000
Month 6: +$70,000

Monthly rates: Finance 0.5%, Reinvest 0.6%
=MIRR({-100000, 50000, 70000}, 0.005, 0.006)
```
**Result:** Monthly MIRR (multiply by 12 for approximate annual)

**Explanation:** When cash flows are monthly, both rates should be monthly rates. The result is also a monthly rate. Annualize carefully: (1+monthly_rate)^12 - 1.

---

### Example 9: Comparing Two Projects
```
Project A: Large early returns
-100000, 80000, 40000, 20000, 10000

Project B: Large late returns
-100000, 10000, 20000, 40000, 100000

At 8% finance, 10% reinvest:
MIRR_A: =MIRR(A_flows, 0.08, 0.10) = 15.1%
MIRR_B: =MIRR(B_flows, 0.08, 0.10) = 12.5%
```
**Result:** Project A better despite equal total cash flows

**Explanation:** Project A returns cash earlier, which can be reinvested sooner. MIRR captures this timing advantage. Standard IRR would give similar results, but MIRR uses realistic reinvestment rates.

---

### Example 10: Zero or Low Reinvestment Rate
```
=MIRR({-10000, 5000, 4000, 3000, 2000}, 0.08, 0)
```
**Result:** MIRR assuming no reinvestment return

**Explanation:** Setting reinvestment rate to 0% is extremely conservative--it assumes positive cash flows earn nothing until the end. This provides a floor return estimate.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | No negative or no positive cash flow | MIRR requires at least one positive and one negative value |
| `#VALUE!` | Non-numeric values in array | Ensure all cash flows are numbers |
| `#NUM!` | Rates causing invalid calculation | Check that rates are reasonable (not extremely negative) |
| `MIRR < finance rate` | Poor investment | This can happen when investment returns are weak; the result is valid |
| `Very different from IRR` | Normal behavior when reinvestment matters | MIRR is often lower than IRR; this is the realistic adjustment |

## Use Cases

### [[Capital Budgeting Project Selection]]

**Scenario:** A company must choose between two mutually exclusive projects with different cash flow patterns and wants a realistic comparison.

**Implementation:**
```
Project Alpha (front-loaded returns):
Year 0: -$500,000
Year 1: $250,000
Year 2: $200,000
Year 3: $150,000
Year 4: $100,000

Project Beta (back-loaded returns):
Year 0: -$500,000
Year 1: $50,000
Year 2: $100,000
Year 3: $200,000
Year 4: $450,000

Company WACC: 10%
Realistic reinvestment rate: 8%

=MIRR(Alpha, 0.10, 0.08) = 14.2%
=MIRR(Beta, 0.10, 0.08) = 11.8%

IRR comparison:
=IRR(Alpha) = 20.5%
=IRR(Beta) = 18.7%
```
**Result:** Alpha preferred by both metrics, but the gap is more realistic with MIRR

**Business Application:** The CFO presents MIRR to the board because it reflects the company's actual reinvestment opportunities (8%) rather than the unrealistic assumption of reinvesting at 20%+ IRR. This leads to better capital allocation decisions.

**Technical Details:** The WACC (10%) is used as the finance rate because it represents the cost of funding new investments. The reinvestment rate (8%) reflects realistic returns on intermediate cash flows.

---

### [[Investment Analysis with Irregular Cash Patterns]]

**Scenario:** An investor is evaluating a real estate development that requires multiple capital infusions and generates irregular returns.

**Implementation:**
```
Real estate development:
Year 0: -$1,000,000 (land acquisition)
Year 1: -$2,000,000 (construction phase 1)
Year 2: -$1,500,000 (construction phase 2)
Year 3: +$500,000 (partial pre-sales)
Year 4: +$1,000,000 (rental income starts)
Year 5: +$6,000,000 (project sale)

Investor's cost of capital: 12%
Expected reinvestment return: 7% (bonds/savings)

=MIRR(all_cash_flows, 0.12, 0.07)
```
**Result:** MIRR accounting for realistic financing and reinvestment

**Business Application:** Real estate investors often can't reinvest at project-level returns. MIRR with a conservative reinvestment rate (treasury or bond rate) gives a more accurate picture of personal wealth accumulation.

**Technical Details:** The multiple negative cash flows (capital calls during construction) are all discounted at the finance rate. The positive cash flows are compounded at the reinvestment rate to the final period.

---

### [[Comparing Investments with Different Risk Levels]]

**Scenario:** A portfolio manager needs to compare investments with different risk profiles, using appropriate discount and reinvestment rates for each.

**Implementation:**
```
High-risk tech investment:
Cash flows: -100000, 20000, 30000, 50000, 80000
Finance rate: 15% (reflects higher risk)
Reinvestment rate: 8% (same pool of capital)
=MIRR(tech_flows, 0.15, 0.08) = 13.1%

Low-risk utility investment:
Cash flows: -100000, 25000, 25000, 25000, 60000
Finance rate: 8% (reflects lower risk)
Reinvestment rate: 8%
=MIRR(utility_flows, 0.08, 0.08) = 10.2%

Risk-adjusted comparison:
Tech provides 2.9% excess return for additional risk
```
**Result:** Risk-adjusted returns for proper comparison

**Business Application:** Using different finance rates to reflect risk levels creates comparable risk-adjusted returns. The tech investment must exceed the utility by enough to justify the additional risk.

**Technical Details:** The finance rate adjustment is a form of risk-adjustment. Higher-risk investments should use higher discount rates for the negative cash flows, reflecting the higher cost of capital for risky ventures.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Assumes equal periods between cash flows
- **Precision:** IEEE 754 double-precision floating point

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Identical to Excel
- **Precision:** Same as Excel

### Both Platforms
- Requires at least one positive and one negative cash flow
- Returns periodic rate (usually annual)
- First value is Period 0 (beginning), subsequent values are end of each period
- Both rates should be expressed as decimals

## Tips and Best Practices

1. **Use your actual cost of capital:** The finance rate should reflect what you actually pay for funding--your borrowing rate, WACC, or required return on equity.

2. **Be realistic about reinvestment:** The reinvestment rate should reflect actual reinvestment opportunities--often a conservative bond rate, money market rate, or company cost of capital.

3. **Use MIRR for project comparison:** When comparing projects with different cash flow timing, MIRR with consistent rates provides apples-to-apples comparison.

4. **Prefer MIRR when IRR is high:** If IRR exceeds 20-25%, the implicit reinvestment assumption becomes increasingly unrealistic. MIRR gives a more grounded estimate.

5. **Use MIRR for multiple sign changes:** When cash flows alternate between positive and negative, IRR may have multiple solutions. MIRR always has exactly one solution.

6. **Consider presenting both metrics:** Show IRR (best case with optimal reinvestment) and MIRR (realistic case) to give stakeholders a range of expected returns.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[IRR]] | Internal rate of return (assumes reinvestment at IRR) | When reinvestment at project rate is realistic |
| [[XIRR]] | IRR for irregular dates | When cash flows occur on specific dates |
| [[NPV]] | Net present value at given rate | To find value rather than return rate |
| [[XNPV]] | NPV for irregular dates | For NPV with specific cash flow dates |

### Commonly Used Together

**[[IRR]]** - Compare to Standard IRR

*Show the impact of reinvestment assumptions:*
```
IRR: =IRR(cash_flows) = 25%
MIRR: =MIRR(cash_flows, 0.10, 0.08) = 15%
Difference: 10% overstated by IRR's reinvestment assumption
```
Present both to show the range of expected returns.

---

**[[NPV]]** - Verify with Net Present Value

*Check MIRR against NPV analysis:*
```
If MIRR > WACC, then NPV at WACC > 0
=NPV(WACC, future_cash_flows) + initial_investment
```
MIRR and NPV should give consistent accept/reject decisions.

---

**[[FV]]** - Calculate Future Value of Positives

*Understand MIRR's calculation:*
```
FV of positives at reinvest rate
PV of negatives at finance rate
MIRR = (FV/PV)^(1/n) - 1
```
Breaking down MIRR's components aids understanding.

---

**[[IF]]** - Investment Decision Logic

*Automate accept/reject:*
```
=IF(MIRR(flows, fin_rate, reinv_rate) > hurdle, "Accept", "Reject")
```
Accept if MIRR exceeds the required return threshold.

## Official Documentation

- **Microsoft Excel:** [MIRR function](https://support.microsoft.com/en-us/office/mirr-function-b020f038-7f67-4f0f-9e71-9d5fd3e32d93)
- **Google Sheets:** [MIRR function](https://support.google.com/docs/answer/3093187)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original implementation |
| Excel 2007+ | Unchanged | Consistent behavior maintained |
| Google Sheets | Original launch | Built-in function from start |

---

*Last updated: 2026-01-10*
