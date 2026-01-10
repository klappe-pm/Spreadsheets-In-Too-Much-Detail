---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- cumulative-principal
- loan-amortization
- equity-building
- debt-payoff
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cumulative Principal
- Total Principal Paid
- Equity Building
tags:
- financial-modeling
- loan-analysis
- debt-management
- equity-tracking
---

# CUMPRINC

## Description

**CUMPRINC (Cumulative Principal)** calculates the total principal paid on a loan between two specified payment periods. While PPMT shows principal for a single payment, CUMPRINC sums principal across a range of payments--perfect for answering "How much of my debt have I actually paid off?" or "How much equity have I built this year?" This function is essential for tracking real debt reduction, equity building, and understanding the true progress of loan payoff.

The function reveals one of the most frustrating aspects of long-term loans: early principal paydown is painfully slow. In year 1 of a 30-year mortgage, about 83% of payments go to interest and only 17% to principal. CUMPRINC helps visualize this progression and shows how the ratio improves over time. It also quantifies the impact of extra payments--showing exactly how much additional principal was retired beyond the minimum.

**Understanding sign conventions is critical:** Cash you pay OUT is negative, and cash you receive IN is positive. CUMPRINC returns a **negative value** because principal payments represent money leaving your pocket (even though they reduce your debt). Like CUMIPMT, the function requires a `type` parameter (0 or 1) that specifies payment timing--it is NOT optional. The start_period and end_period are 1-indexed: period 1 is the first payment.

CUMPRINC is available in all versions of Excel and Google Sheets with identical behavior. It efficiently calculates the sum of principal payments in one function call, making it ideal for annual equity reports, payoff milestone tracking, and amortization summary tables. The fundamental identity holds: CUMIPMT + CUMPRINC for any period range equals the total payments made during that range.

## Syntax

> [!f(x)] CUMPRINC Syntax
>
> ```
> =CUMPRINC(rate, nper, pv, start_period, end_period, type)
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

Returns the cumulative principal paid between start_period and end_period as a negative number (money you pay out). The absolute value represents total dollars of principal reduction. To display as positive, use `-CUMPRINC(...)` or `ABS(CUMPRINC(...))`.

## Examples

> [!f(x)] CUMPRINC Examples

### Example 1: Total Principal Over Loan Life
```
=CUMPRINC(0.06/12, 360, 200000, 1, 360, 0)
```
**Result:** `-$200,000.00`

**Explanation:** Total principal paid over a 30-year mortgage equals exactly the original loan amount--the loan is fully paid off. This confirms the math works: all the borrowed principal gets returned through payments over the loan term.

---

### Example 2: First Year Principal (Equity Building)
```
=CUMPRINC(0.06/12, 360, 200000, 1, 12, 0)
```
**Result:** `-$2,455.97`

**Explanation:** In year 1 of a $200,000 mortgage at 6%, only $2,456 of your ~$14,389 in payments actually reduces the loan. That is only 17% going to principal--the rest is interest. This slow start frustrates borrowers but is mathematically correct for amortizing loans.

---

### Example 3: Compare Year 1 vs. Year 30 Principal
```
Year 1: =CUMPRINC(0.06/12, 360, 200000, 1, 12, 0) = -$2,455.97
Year 30: =CUMPRINC(0.06/12, 360, 200000, 349, 360, 0) = -$13,964.32
```
**Result:** Year 30 has 5.7x more principal reduction than year 1

**Explanation:** In year 1, only $2,456 of $14,389 paid goes to principal (17%). In year 30, $13,964 of $14,389 goes to principal (97%). The ratio reverses completely over the loan life. This is why staying in a loan long-term builds equity faster.

---

### Example 4: Display as Positive (Equity Gained)
```
=-CUMPRINC(0.06/12, 360, 200000, 1, 60, 0)
```
**Result:** `$13,891.16`

**Explanation:** Negating gives positive display for equity reports. After 5 years, you have built $13,891 in equity through principal payments alone. Add appreciation and your equity may be substantially higher.

---

### Example 5: Principal by 5-Year Blocks
```
Years 1-5: =CUMPRINC(0.06/12, 360, 200000, 1, 60, 0) = -$13,891.16
Years 6-10: =CUMPRINC(0.06/12, 360, 200000, 61, 120, 0) = -$18,759.14
Years 11-15: =CUMPRINC(0.06/12, 360, 200000, 121, 180, 0) = -$25,304.71
Years 16-20: =CUMPRINC(0.06/12, 360, 200000, 181, 240, 0) = -$33,843.14
Years 21-25: =CUMPRINC(0.06/12, 360, 200000, 241, 300, 0) = -$45,009.38
Years 26-30: =CUMPRINC(0.06/12, 360, 200000, 301, 360, 0) = -$63,192.47
```
**Result:** Principal paydown accelerates dramatically in later years

**Explanation:** Principal reduction grows each 5-year block: $13.9K, $18.8K, $25.3K, $33.8K, $45.0K, $63.2K. The last 5 years pay off 4.5x more principal than the first 5 years, even though total payments are identical.

---

### Example 6: Car Loan Equity Building
```
=CUMPRINC(0.049/12, 60, 25000, 1, 24, 0)
```
**Result:** `-$8,938.88`

**Explanation:** After 2 years (24 months) of a 5-year car loan at 4.9%, you have paid down $8,939 of the $25,000 borrowed. This is important for trade-in value calculations--if your car is worth less than $16,061, you are "underwater."

---

### Example 7: Verify CUMIPMT + CUMPRINC = Total Payments
```
Year 1 Interest: =CUMIPMT(0.06/12, 360, 200000, 1, 12, 0) = -$11,933.19
Year 1 Principal: =CUMPRINC(0.06/12, 360, 200000, 1, 12, 0) = -$2,455.97
Sum: -$14,389.16

Annual Payments: =PMT(0.06/12, 360, 200000) * 12 = -$14,389.16
```
**Result:** Identity confirmed--CUMIPMT + CUMPRINC = Total PMT

**Explanation:** This fundamental identity provides validation. For any period range, cumulative interest plus cumulative principal equals total payments made. Use this to verify your amortization models.

---

### Example 8: Beginning-of-Period Payments
```
=CUMPRINC(0.06/12, 360, 200000, 1, 12, 1)
```
**Result:** `-$2,515.59`

**Explanation:** With beginning-of-period payments (type=1), year 1 principal is slightly higher ($59.62 more) because payments made earlier reduce principal sooner, leaving less principal for interest calculation. Leases often use this timing.

---

### Example 9: Extra Payments Impact
```
Standard 5-year principal: =CUMPRINC(0.06/12, 360, 200000, 1, 60, 0) = -$13,891.16

With $200/month extra (approximate):
Extra goes directly to principal: 60 months x $200 = $12,000
Plus accelerated amortization effect
Effective 5-year principal: approximately -$27,000
```
**Result:** Extra payments roughly double equity building in early years

**Explanation:** CUMPRINC shows scheduled principal reduction. Extra payments add directly to principal AND accelerate the amortization effect. The $12,000 in extra payments builds more than $12,000 additional equity due to reduced interest on the lower balance.

---

### Example 10: Remaining Principal (Loan Balance)
```
Principal paid in 60 months: =CUMPRINC(0.06/12, 360, 200000, 1, 60, 0) = -$13,891.16
Remaining balance: $200,000 - $13,891.16 = $186,108.84

Verify with FV: =FV(0.06/12, 60, -PMT(0.06/12,360,200000), 200000) = $186,108.84
```
**Result:** Original principal minus CUMPRINC = Remaining balance

**Explanation:** This relationship helps track loan payoff progress. Original loan minus cumulative principal paid equals current balance. Use either method; they should match exactly.

---

### Example 11: Single Period (Matches PPMT)
```
=CUMPRINC(0.06/12, 360, 200000, 1, 1, 0)
```
**Result:** `-$199.10`

```
=PPMT(0.06/12, 1, 360, 200000)
```
**Result:** `-$199.10`

**Explanation:** When start_period = end_period, CUMPRINC returns the same value as PPMT for that period. This confirms CUMPRINC is truly a sum of individual PPMT values.

---

### Example 12: Invalid Period Range
```
=CUMPRINC(0.06/12, 360, 200000, 100, 50, 0)
```
**Result:** `#NUM!`

**Explanation:** The end_period (50) cannot be less than start_period (100). Period parameters must form a valid ascending range within 1 to nper. Invalid inputs produce #NUM! errors.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | start_period > end_period | Ensure start <= end. Periods must be a valid ascending range. |
| `#NUM!` | start_period < 1 or end_period > nper | Periods must be within 1 to nper range. |
| `#NUM!` | pv <= 0 or nper <= 0 | Present value and number of periods must be positive. |
| `#VALUE!` | Non-numeric inputs or missing type parameter | Verify all parameters are numbers. Type is REQUIRED (0 or 1). |
| `Result is positive` | Unusual sign on pv | Standard loans have positive pv; result is negative. Negative pv (investments) produces positive cumulative result. |
| `Doesn't equal loan paydown` | Comparing to different balance source | CUMPRINC shows scheduled paydown. Actual balance may differ due to rounding, fees, or prepayments. |

## Use Cases

### [[Home Equity Tracking]]

**Scenario:** A homeowner wants to track their equity growth over time, separating the equity built through mortgage payments from equity built through home appreciation.

**Implementation:**
```
Original purchase: $400,000, loan $320,000 at 6%, 30-year
Current month: 84 (7 years in)

Equity from down payment: $80,000
Equity from principal: =-CUMPRINC(0.06/12, 360, 320000, 1, 84, 0) = $27,429.51
Current home value: $520,000 (estimated)
Equity from appreciation: $520,000 - $400,000 = $120,000

Total equity: $80,000 + $27,429.51 + $120,000 = $227,429.51
Loan balance: $320,000 - $27,429.51 = $292,570.49
```

**Business Application:** Real estate agents, mortgage lenders, and homeowners use CUMPRINC to calculate "forced savings" equity from loan paydown. This helps clients understand that even without appreciation, they are building wealth through each mortgage payment.

**Technical Details:** Appreciation equity is unrealized until sale. Principal equity is guaranteed (assuming payments are made). Conservative financial planning may focus on principal equity as the "sure thing."

---

### [[Debt Payoff Milestone Celebration]]

**Scenario:** A family wants to celebrate debt payoff milestones by tracking when they have paid off certain percentages of their original loan principal.

**Implementation:**
```
Loan: $250,000 at 5.5%, 30-year
Monthly payment: $1,419.47

25% payoff ($62,500): Find period where CUMPRINC reaches -$62,500
=CUMPRINC(0.055/12, 360, 250000, 1, 177, 0) = -$62,376.22 (month 177, year 14.75)

50% payoff ($125,000):
=CUMPRINC(0.055/12, 360, 250000, 1, 260, 0) = -$125,045.65 (month 260, year 21.7)

75% payoff ($187,500):
=CUMPRINC(0.055/12, 360, 250000, 1, 320, 0) = -$187,609.35 (month 320, year 26.7)
```

**Business Application:** Financial coaches use CUMPRINC milestones to gamify debt payoff. Celebrating "25% paid off!" provides motivation during the slow early years. Banks could use similar milestones for customer engagement.

**Technical Details:** The milestones are not evenly spaced: 25% takes 14.75 years, but 50% only takes 7 more years. The accelerating nature of principal paydown means later milestones come faster. This is encouraging for long-term borrowers.

---

### [[Refinance Equity Comparison]]

**Scenario:** A homeowner considering refinancing wants to compare equity building between keeping their current loan versus refinancing to a new 30-year term.

**Implementation:**
```
Current: 5 years into 30-year at 7%, original $300,000
Equity built so far: =-CUMPRINC(0.07/12, 360, 300000, 1, 60, 0) = $14,861.55
Balance: $300,000 - $14,861.55 = $285,138.45

Option A: Keep current loan, next 5 years
Additional equity: =-CUMPRINC(0.07/12, 360, 300000, 61, 120, 0) = $20,665.07
Total 10-year equity: $35,526.62

Option B: Refinance $285,138 at 5.5%, new 30-year
5-year equity: =-CUMPRINC(0.055/12, 360, 285138.45, 1, 60, 0) = $22,453.80
Total 10-year equity: $14,861.55 + $22,453.80 = $37,315.35
```

**Business Application:** Mortgage advisors use CUMPRINC comparisons to show that refinancing to a lower rate can actually build MORE equity despite restarting the term, because lower interest means more of each payment goes to principal.

**Technical Details:** This analysis should also compare monthly payments and total interest. The refinance option may have lower payments AND faster equity building if the rate reduction is significant enough.

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
- type parameter is REQUIRED (unlike in PMT/PPMT where it is optional)
- Returns negative for loans (principal paid out)
- Period parameters are 1-indexed (first payment = 1)
- Equivalent to sum of PPMT for all periods in range

## Tips and Best Practices

1. **Remember that type is required:** Unlike PMT or PPMT where type is optional, CUMPRINC requires the type parameter. Most loans use 0 (end of period). Omitting type causes an error.

2. **Track equity building annually:** Use CUMPRINC to calculate yearly principal paydown. This shows clients their "forced savings" from mortgage payments--money that builds wealth rather than disappearing like rent.

3. **Compare to appreciation:** Principal equity is guaranteed (assuming payments made); appreciation equity is speculative. CUMPRINC helps separate these components for realistic financial planning.

4. **Verify with remaining balance:** Original principal minus CUMPRINC should equal remaining balance (which can be calculated with FV). Use this relationship to validate your models.

5. **Model prepayment scenarios:** Calculate CUMPRINC for the standard amortization, then show how extra payments increase the equity-building rate. The difference is compelling for prepayment motivation.

6. **Remember the identity:** CUMIPMT + CUMPRINC for any period range equals total payments made during that range. This is your validation check.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CUMIPMT]] | Cumulative interest over period range | When you need total interest paid rather than principal |
| [[PPMT]] | Principal for a single period | When you need principal for one specific payment |
| [[PMT]] | Total payment amount | When you need the complete payment per period |

### Commonly Used Together

**[[CUMIPMT]]** - Cumulative Interest

*Complete payment breakdown:*
```
Year 1 Principal: =CUMPRINC(0.06/12, 360, 200000, 1, 12, 0) = -$2,455.97
Year 1 Interest: =CUMIPMT(0.06/12, 360, 200000, 1, 12, 0) = -$11,933.19
Year 1 Total: $14,389.16 (= 12 * PMT)
```
CUMPRINC + CUMIPMT = Total payments for any period range.

---

**[[FV]]** - Calculate Remaining Balance

*Two ways to find balance:*
```
Method 1: Original - CUMPRINC
$200,000 - $13,891.16 = $186,108.84

Method 2: FV
=FV(0.06/12, 60, -PMT(0.06/12,360,200000), 200000) = $186,108.84
```
Both methods should produce identical results--use for validation.

---

**[[PMT]]** - Total Payment Verification

*Check the identity:*
```
5-year payments: =PMT(0.06/12, 360, 200000) * 60 = -$71,945.88
CUMIPMT: -$58,189.49
CUMPRINC: -$13,756.39
Sum: -$71,945.88
```
PMT * periods = CUMIPMT + CUMPRINC always.

---

**[[NPER]]** - Time to Equity Milestone

*Find when target equity is reached:*
```
When will $50,000 in principal be paid?
Test with CUMPRINC:
=CUMPRINC(0.06/12, 360, 200000, 1, 159, 0) = -$50,155.99
Answer: About month 159 (13.25 years)
```
Use iterative testing with CUMPRINC to find milestone timing.

## Official Documentation

- **Microsoft Excel:** [CUMPRINC function](https://support.microsoft.com/en-us/office/cumprinc-function-94a4516d-bd65-41a1-bc16-053a6e4d0baf)
- **Google Sheets:** [CUMPRINC function](https://support.google.com/docs/answer/3093201)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 (Analysis ToolPak) | Originally required add-in activation |
| Excel 2007+ | Built-in function | No longer requires add-in |
| Google Sheets | Original launch | Built-in function from start |

---

*Last updated: 2026-01-10*
