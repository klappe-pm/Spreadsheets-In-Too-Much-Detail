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
- investment-analysis
- compound-interest
- variable-rates
- portfolio-growth
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Future Value Schedule
- Variable Rate Future Value
- Compound Growth Schedule
tags:
- financial-modeling
- investment-analysis
- compound-interest
- variable-rates
- portfolio-growth
---

# FVSCHEDULE

## Description

**FVSCHEDULE (Future Value Schedule)** calculates the future value of an initial investment after applying a series of compound interest rates. Unlike the standard FV function that assumes a constant interest rate, FVSCHEDULE handles variable rates—perfect for real-world scenarios where returns change year to year. If you invested $10,000 and earned 8% in year 1, lost 3% in year 2, and gained 12% in year 3, FVSCHEDULE computes the final value by applying each rate sequentially.

This function is essential for modeling realistic investment performance. In practice, investment returns are never constant. Stock portfolios might gain 25% one year and lose 10% the next. Real estate investments fluctuate with market conditions. FVSCHEDULE captures this variability by compounding each period's rate onto the previous balance. The mathematical formula is: Principal x (1 + rate1) x (1 + rate2) x (1 + rate3) x ... and so on for each rate in the schedule.

**Critical understanding of rate formatting:** FVSCHEDULE expects rates as decimals (0.08 for 8%), not percentages (8). A 5% loss should be entered as -0.05. The function multiplies (1 + rate) for each period, so zero return is 0 (not 1), and a complete 100% loss would be -1. Be especially careful with negative returns—they compound just like gains but in reverse, and the order matters more than you might expect.

FVSCHEDULE is available in Excel (built-in since Excel 2007, Analysis ToolPak in earlier versions) and Google Sheets (native support). Both platforms produce identical results. The function is particularly useful for back-testing investment strategies, modeling portfolio performance with historical returns, and projecting future values under varying economic scenarios.

## Syntax

> [!f(x)] FVSCHEDULE Syntax
>
> ```
> =FVSCHEDULE(principal, schedule)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `principal` | Yes | The initial investment or present value. Enter as a positive number. This is the starting amount before any returns are applied. |
| `schedule` | Yes | A range or array of interest rates to apply. Each rate represents one compounding period. Enter as decimals (0.05 for 5%, -0.03 for -3%). |

### Return Value

Returns the future value after all rates in the schedule have been applied to the principal through compound multiplication. Always returns a positive number unless principal or rates are structured to produce negative (which would require rates below -100%).

## Examples

> [!f(x)] FVSCHEDULE Examples

### Example 1: Basic Variable Rate Investment
```
=FVSCHEDULE(10000, {0.05, 0.08, 0.06})
```
**Result:** `$12,020.40`

**Explanation:** Starting with $10,000: Year 1 grows 5% to $10,500; Year 2 grows 8% to $11,340; Year 3 grows 6% to $12,020.40. The function compounds each rate sequentially.

---

### Example 2: Historical Stock Market Returns
```
=FVSCHEDULE(100000, {0.2889, -0.1103, 0.2687, 0.1940, -0.1019})
```
**Result:** `$148,983.02`

**Explanation:** Simulating 5 years of S&P 500 returns with volatility. Despite two negative years, the $100,000 grows to nearly $149,000. This is more realistic than assuming a constant average return.

---

### Example 3: Using Cell Range for Rates
```
=FVSCHEDULE(50000, B2:B11)
```
Where B2:B11 contains 10 years of historical or projected rates

**Result:** Depends on rates in range

**Explanation:** Reference a column of rates for cleaner formulas. This approach makes it easy to update projections—just change the rate values, and the result recalculates automatically.

---

### Example 4: Including Zero Return Year
```
=FVSCHEDULE(25000, {0.07, 0, 0.04, 0.09})
```
**Result:** `$30,098.10`

**Explanation:** Year 2 has 0% return (flat performance). FVSCHEDULE handles zeros correctly—it multiplies by (1 + 0) = 1, leaving the balance unchanged for that period.

---

### Example 5: All Negative Returns (Bear Market)
```
=FVSCHEDULE(100000, {-0.05, -0.10, -0.08})
```
**Result:** `$78,660.00`

**Explanation:** Three consecutive losing years: -5%, -10%, -8%. The $100,000 drops to $78,660. This models bear market scenarios for stress-testing portfolios.

---

### Example 6: Mixed Returns with Large Swing
```
=FVSCHEDULE(50000, {0.35, -0.25, 0.20})
```
**Result:** `$60,750.00`

**Explanation:** Volatile performance: +35%, -25%, +20%. Note that +35% then -25% does NOT net to +10%. After gaining 35% ($67,500), losing 25% drops to $50,625. Then +20% reaches $60,750. Sequence matters.

---

### Example 7: Order Matters Demonstration
```
Rising then falling: =FVSCHEDULE(10000, {0.20, 0.20, -0.30})  → $10,080
Falling then rising: =FVSCHEDULE(10000, {-0.30, 0.20, 0.20})  → $10,080
```
**Result:** Same final value ($10,080)

**Explanation:** Interestingly, for simple multiplication, the order doesn't affect the final value—but it dramatically affects the path and psychological experience. The peak before the crash is $14,400 vs. starting with the loss at $7,000.

---

### Example 8: Single Rate Comparison with FV
```
FVSCHEDULE: =FVSCHEDULE(1000, {0.05, 0.05, 0.05, 0.05, 0.05})  → $1,276.28
FV function: =FV(0.05, 5, 0, -1000)                            → $1,276.28
```
**Result:** Both return $1,276.28

**Explanation:** When all rates are equal, FVSCHEDULE produces the same result as FV. This validates your formula and demonstrates that FVSCHEDULE is a generalization of FV.

---

### Example 9: Bond Ladder Returns
```
=FVSCHEDULE(200000, {0.04, 0.042, 0.045, 0.048, 0.05})
```
**Result:** `$252,101.71`

**Explanation:** A bond ladder with rising rates as bonds mature and are reinvested at higher rates. The $200,000 grows to $252,102 over 5 years with gradually increasing yields.

---

### Example 10: Real Estate Investment with Varying Returns
```
=FVSCHEDULE(500000, {0.12, 0.08, 0.15, -0.05, 0.10, 0.07, 0.09})
```
**Result:** `$883,442.94`

**Explanation:** A 7-year real estate investment with variable appreciation (including one down year). Property value grows from $500,000 to $883,443, demonstrating long-term growth despite volatility.

---

### Example 11: Calculating Average Return from Final Value
```
Investment: =FVSCHEDULE(10000, {0.08, -0.05, 0.12, 0.06, -0.02}) → $12,027.01
CAGR: =((12027.01/10000)^(1/5))-1 → 3.77%
```
**Result:** 3.77% compound annual growth rate

**Explanation:** After finding the final value, calculate the CAGR (compound annual growth rate) that would produce the same result with constant returns. This is the "equivalent" constant rate.

---

### Example 12: Inflation-Adjusted Returns
```
Nominal returns: {0.10, 0.08, 0.12}
Inflation rates: {0.03, 0.025, 0.04}
Real return calculation: =FVSCHEDULE(100000, {(1.10/1.03)-1, (1.08/1.025)-1, (1.12/1.04)-1})
```
**Result:** `$120,898.62`

**Explanation:** To calculate real (inflation-adjusted) growth, convert each nominal return to real return using: (1 + nominal)/(1 + inflation) - 1. This shows purchasing power growth, not just nominal value.

---

### Example 13: Multiple Investment Comparison
```
Conservative: =FVSCHEDULE(50000, {0.04, 0.05, 0.03, 0.04, 0.05}) → $61,693
Aggressive:   =FVSCHEDULE(50000, {0.15, -0.08, 0.22, -0.05, 0.18}) → $69,673
```
**Result:** Aggressive wins despite volatility

**Explanation:** Comparing two investment strategies over 5 years. The aggressive portfolio has higher returns despite two negative years. Use this approach to back-test different allocation strategies.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Text values in rate schedule | Ensure all rates are numeric. Check for hidden text or special characters. |
| `#VALUE!` | Non-numeric principal | Principal must be a number. Check for text formatting. |
| `#NUM!` | Rate equals exactly -1 (100% loss) | A -1 rate means complete loss. Subsequent calculations may error. Use -0.99 for near-total loss. |
| Wrong result | Rates entered as percentages instead of decimals | Enter 5% as 0.05, not 5. The formula multiplies (1 + rate). |
| Unexpected zero | One rate in schedule is -1 | Any rate of -1 zeros out the entire investment permanently. |
| Result too large/small | Rates not matching intended periods | Verify that rates correspond to intended time periods. Annual rates for annual periods, etc. |

## Use Cases

### [[Portfolio Performance Analysis]]

**Scenario:** An investment advisor needs to calculate actual portfolio value based on year-by-year returns for client reporting and performance evaluation.

**Implementation:**
```
Annual returns stored in C2:C11 (10 years of data)
Initial investment: $250,000

Current Value: =FVSCHEDULE(250000, C2:C11)
Total Return: =FVSCHEDULE(250000, C2:C11)/250000 - 1
CAGR: =((FVSCHEDULE(250000, C2:C11)/250000)^(1/10))-1
```

**Business Application:** Wealth managers use FVSCHEDULE to report accurate portfolio values and performance metrics. Unlike simple average returns, this captures the actual compound effect of variable returns, which is required for accurate client statements.

**Technical Details:** Store historical returns in a dedicated column. For multiple accounts, use FVSCHEDULE with each account's starting balance. The CAGR calculation provides the "equivalent constant rate" for comparing against benchmarks.

---

### [[Retirement Projection with Variable Returns]]

**Scenario:** A financial planner needs to model retirement portfolio growth under different market scenarios rather than assuming unrealistic constant returns.

**Implementation:**
```
Optimistic scenario: =FVSCHEDULE(portfolio, optimistic_rates)
Base scenario: =FVSCHEDULE(portfolio, base_rates)
Pessimistic scenario: =FVSCHEDULE(portfolio, pessimistic_rates)

Where each rates range contains 20+ years of projected returns
```

**Business Application:** Monte Carlo simulations and scenario analysis rely on variable return modeling. FVSCHEDULE allows planners to show clients a range of outcomes rather than a single "average return" projection that may be misleading.

**Technical Details:** Build rate schedules that reflect different market environments. The optimistic might average 9% with low volatility, base case 7% with moderate volatility, and pessimistic 4% with high volatility. Use historical return distributions to create realistic sequences.

---

### [[Educational Savings Calculator]]

**Scenario:** Parents want to project college savings growth using historically-based variable returns rather than optimistic constant assumptions.

**Implementation:**
```
Starting balance: $10,000
Annual contributions: Additional investments handled separately
Projected 18-year returns (from historical averages):
=FVSCHEDULE(10000, historical_18yr_return_sequence)

For ongoing contributions, calculate each year's contribution separately:
Year 1 contribution FV: =FVSCHEDULE(contribution, OFFSET(rates, 1, 0, remaining_years))
Year 2 contribution FV: =FVSCHEDULE(contribution, OFFSET(rates, 2, 0, remaining_years))
...and so on
```

**Business Application:** 529 plan administrators and financial advisors use realistic projections to help parents understand the range of possible outcomes and the importance of consistent contributions.

**Technical Details:** FVSCHEDULE doesn't directly handle periodic contributions—only single lump sums. For regular contributions, calculate each year's contribution separately with its own FVSCHEDULE (using remaining years' rates), then sum all results.

## Platform Differences

### Microsoft Excel
- **Availability:** Built-in since Excel 2007. Earlier versions require Analysis ToolPak add-in.
- **Specific Behavior:** Accepts arrays using curly braces {0.05, 0.08, 0.06} or cell ranges. Maximum array size follows Excel's standard limits.

### Google Sheets
- **Availability:** Native support since original launch. No add-in required.
- **Specific Behavior:** Identical calculation. Array syntax same as Excel: {0.05, 0.08, 0.06} or range references.

### Both Platforms
- Process rates in order from first to last in the schedule
- Ignore empty cells in rate ranges (treated as 0% return)
- Return #VALUE! if any rate is non-numeric
- Can use horizontal or vertical ranges for schedule

## Tips and Best Practices

1. **Always use decimals for rates:** Enter 8% as 0.08, not 8. This is the most common error and produces wildly incorrect results.

2. **Validate with known scenarios:** Test with constant rates and compare to FV function. They should match: `=FVSCHEDULE(1000, {0.05, 0.05, 0.05})` equals `=FV(0.05, 3, 0, -1000)`.

3. **Store rates in a range:** Rather than hardcoding rates in arrays, store them in a column. This makes updates easier and keeps formulas readable.

4. **Calculate CAGR for comparison:** After finding the final value, calculate CAGR: `=((final/initial)^(1/periods))-1`. This gives the equivalent constant rate for benchmarking.

5. **Handle contributions separately:** FVSCHEDULE only works with a single initial principal. For ongoing contributions, calculate each contribution's future value separately and sum them.

6. **Use for sensitivity analysis:** Create multiple rate schedules (optimistic, base, pessimistic) to show the range of possible outcomes.

7. **Remember that order technically doesn't matter:** For pure multiplication, final value is the same regardless of rate order. But the path matters for understanding risk and psychology.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FV]] | Future value with constant rate | When rate is the same for all periods—simpler and faster |
| [[XNPV]] | Net present value with specific dates | When you need present value of variable cash flows |
| [[IRR]] | Internal rate of return | When you need to find the equivalent constant rate that produces a given outcome |

### Commonly Used Together

**[[FV]]** - Future Value with Constant Rate

*Combined for comparison:*
```
Variable returns: =FVSCHEDULE(10000, historical_rates)
Assumed constant: =FV(average_rate, periods, 0, -10000)
Difference shows: Impact of variability vs. averages
```
Compare FVSCHEDULE results against FV with average rate to quantify the cost of volatility.

---

**[[RATE]]** - Calculate Equivalent Rate

*Combined to find CAGR:*
```
Final Value: =FVSCHEDULE(10000, A1:A10)
Equivalent Rate: =RATE(10, 0, -10000, FVSCHEDULE(10000, A1:A10))
```
Find the constant rate that would produce the same final value—this is the Compound Annual Growth Rate (CAGR).

---

**[[PRODUCT]]** - Alternative Calculation Method

*Alternative approach to FVSCHEDULE:*
```
Using FVSCHEDULE: =FVSCHEDULE(10000, {0.05, 0.08, 0.06})
Using PRODUCT: =10000 * PRODUCT(1+{0.05, 0.08, 0.06})
```
Both return the same result. PRODUCT approach may be more flexible for certain advanced scenarios.

---

**[[AVERAGE]]** - Calculate Average Return

*Combined for analysis:*
```
Average annual return: =AVERAGE(A1:A10)
But actual growth: =FVSCHEDULE(10000, A1:A10)
CAGR (real average): =((FVSCHEDULE(10000, A1:A10)/10000)^(1/10))-1
```
The arithmetic average of returns is always higher than the geometric (compound) average. FVSCHEDULE reveals the true compound growth.

## Official Documentation

- **Microsoft Excel:** [FVSCHEDULE function](https://support.microsoft.com/en-us/office/fvschedule-function-bec29522-bd87-4082-bab9-a241f3fb251d)
- **Google Sheets:** [FVSCHEDULE function](https://support.google.com/docs/answer/3093444)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2003 & earlier | Analysis ToolPak add-in | Required manual add-in installation |
| Excel 2007+ | Built-in function | No add-in required |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Native support, identical to Excel |

---

*Last updated: 2026-01-10*
