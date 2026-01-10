---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- future-value
- compound-interest
- variable-rates
- investment-growth
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Future Value Schedule
- Variable Rate Future Value
- Compound Growth with Varying Rates
tags:
- future-value
- compound-interest
- variable-rates
- investment-planning
- time-value-of-money
---

# FVSCHEDULE

## Description

**FVSCHEDULE (Future Value Schedule)** calculates the future value of an initial principal after applying a series of compound interest rates. Unlike the standard FV function that assumes a constant interest rate throughout the investment period, FVSCHEDULE handles situations where interest rates vary from period to period. This makes it essential for modeling investments in variable-rate environments, stepped interest rate products, or any scenario where returns differ across time periods.

The function works by sequentially applying each rate in your schedule to compound the principal. If you invest $1,000 and have rates of 5%, 8%, and 6% over three years, FVSCHEDULE calculates: $1,000 x (1.05) x (1.08) x (1.06) = $1,200.74. Each rate is applied to the cumulative balance from the previous period, creating true compound growth with variable rates. This is fundamentally different from simply averaging the rates and using FV.

**Key distinction from FV:** The standard FV function calculates future value assuming the same interest rate applies to every period. FVSCHEDULE allows different rates for each period. Use FV when you have a fixed-rate investment (like a CD with guaranteed rate) and FVSCHEDULE when rates change over time (like variable-rate bonds, projected stock returns, or stepped-rate CDs).

FVSCHEDULE is available in both Excel (built-in since Excel 2007; Analysis ToolPak required in earlier versions) and Google Sheets. The function accepts rates as an array, making it flexible for various schedule lengths. Note that FVSCHEDULE only handles compounding--it does not accommodate additional deposits or withdrawals during the investment period. For investments with periodic contributions at varying rates, you need a more complex model.

## Syntax

> [!f(x)] FVSCHEDULE Syntax
>
> ```
> =FVSCHEDULE(principal, schedule)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `principal` | Yes | The present value or initial investment amount. This is the starting balance that will be compounded by the rates in the schedule. Must be a positive number (negative values are accepted but represent debt). |
| `schedule` | Yes | An array of interest rates to apply sequentially. Can be a range reference (A1:A5), an array constant ({0.05, 0.08, 0.06}), or a named range. Each rate should be expressed as a decimal (0.05 for 5%, not 5). |

### Return Value

Returns the future value of the principal after all scheduled rates have been applied in sequence. The result represents the compound growth of the original investment across all periods.

## Examples

> [!f(x)] FVSCHEDULE Examples

### Example 1: Basic Three-Year Variable Rate
```
=FVSCHEDULE(1000, {0.05, 0.08, 0.06})
```
**Result:** `$1,200.74`

**Explanation:** A $1,000 investment earns 5% in year 1, 8% in year 2, and 6% in year 3. Calculation: $1,000 x 1.05 = $1,050; $1,050 x 1.08 = $1,134; $1,134 x 1.06 = $1,200.74. Each year's rate compounds on the accumulated balance.

---

### Example 2: Using Cell Range for Rates
```
=FVSCHEDULE(5000, A1:A5)
```
Where A1:A5 contains: 0.03, 0.04, 0.045, 0.05, 0.055

**Result:** `$6,198.36`

**Explanation:** A $5,000 investment over 5 years with rates from cells A1:A5. The rates increase each year (3%, 4%, 4.5%, 5%, 5.5%), perhaps reflecting improving market conditions or a stepped-rate CD.

---

### Example 3: Negative Returns (Market Decline)
```
=FVSCHEDULE(10000, {0.15, -0.20, 0.10})
```
**Result:** `$10,120.00`

**Explanation:** An investment gains 15% in year 1, loses 20% in year 2, then gains 10% in year 3. This demonstrates that FVSCHEDULE handles negative rates (losses): $10,000 x 1.15 = $11,500; $11,500 x 0.80 = $9,200; $9,200 x 1.10 = $10,120.

---

### Example 4: Stepped Interest Rate CD
```
=FVSCHEDULE(25000, {0.02, 0.025, 0.03, 0.035, 0.04})
```
**Result:** `$29,047.90`

**Explanation:** A 5-year stepped-rate CD starts at 2% and increases by 0.5% each year. This product structure rewards holding to maturity. Total growth: 16.2% over 5 years.

---

### Example 5: Constant Rate Comparison
```
Variable: =FVSCHEDULE(1000, {0.04, 0.05, 0.06, 0.07, 0.08}) = $1,338.23
Constant: =FV(0.06, 5, 0, -1000) = $1,338.23 (if average rate were 6%)
```
**Result:** Different results

**Explanation:** FVSCHEDULE with rates 4%, 5%, 6%, 7%, 8% (averaging 6%) gives a different result than FV at constant 6% because compounding order matters. Lower rates early compound less than higher rates early.

---

### Example 6: Single Period (Equivalent to Simple Interest)
```
=FVSCHEDULE(1000, {0.10})
```
**Result:** `$1,100.00`

**Explanation:** With just one rate, FVSCHEDULE simply applies that rate once: $1,000 x 1.10 = $1,100. This is equivalent to one period of FV or simple interest for a single year.

---

### Example 7: Monthly Rates Over a Year
```
=FVSCHEDULE(1000, B1:B12)
```
Where B1:B12 contains monthly rates: 0.005, 0.006, 0.004, ...

**Result:** Depends on monthly rates

**Explanation:** FVSCHEDULE can handle any period length. For monthly compounding over a year, provide 12 monthly rates. Ensure rates match the period length (monthly rates, not annual rates).

---

### Example 8: Historical Stock Market Returns
```
=FVSCHEDULE(10000, C1:C10)
```
Where C1:C10 contains actual S&P 500 annual returns: 0.316, 0.184, -0.043, ...

**Result:** Actual growth based on historical returns

**Explanation:** Model how an investment would have grown using actual historical returns. This is more accurate than using average historical returns because it captures the sequence of returns effect.

---

### Example 9: Inflation Adjustment (Negative Rates)
```
Real Value: =FVSCHEDULE(50000, {-0.03, -0.025, -0.02, -0.02, -0.015})
```
**Result:** `$44,589.14`

**Explanation:** Calculate the real (inflation-adjusted) value of money over time by using negative rates representing purchasing power loss. $50,000 today might only buy $44,589 worth of goods in 5 years after 3%, 2.5%, 2%, 2%, 1.5% inflation.

---

### Example 10: Bond Reinvestment at Different Rates
```
=FVSCHEDULE(1000, {0.045, 0.04, 0.035, 0.03, 0.025})
```
**Result:** `$1,188.48`

**Explanation:** Model a bond ladder or series where coupon payments are reinvested at prevailing rates that decline over time. The declining rate scenario shows lower future value than if rates had remained constant.

---

### Example 11: Zero Rate Periods
```
=FVSCHEDULE(5000, {0.05, 0, 0, 0.06, 0.07})
```
**Result:** `$5,944.95`

**Explanation:** Zero rates in the schedule simply multiply by 1 (no growth for those periods). Years 2 and 3 have zero growth, but the investment continues earning 6% and 7% in years 4 and 5.

---

### Example 12: Very High Returns (Venture Capital)
```
=FVSCHEDULE(100000, {-0.50, -0.30, 0.50, 2.00, 3.00})
```
**Result:** `$1,575,000`

**Explanation:** Models a venture capital investment with early losses (-50%, -30%), then explosive growth (50%, 200%, 300%). Despite early losses, the compounded return is substantial due to later gains.

---

### Example 13: Comparing Different Rate Sequences
```
Early High: =FVSCHEDULE(1000, {0.20, 0.10, 0.05}) = $1,386.00
Early Low:  =FVSCHEDULE(1000, {0.05, 0.10, 0.20}) = $1,386.00
```
**Result:** Same final value

**Explanation:** Interestingly, the order of rates does not affect the final result for pure FVSCHEDULE calculations (multiplication is commutative). However, this changes if you add periodic deposits or withdrawals, which FVSCHEDULE does not handle.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Text values in the schedule array | Ensure all rates are numeric values. Check for hidden text or improper formatting in cells. |
| `#VALUE!` | Empty cells in the schedule range | Remove empty cells or replace with 0 if you want no growth for that period. |
| `#NUM!` | Rate less than -1 (below -100%) | A rate of -1 or below would mean losing more than 100% of value, which is invalid. Minimum practical rate is -1 (total loss). |
| `Wrong result` | Rates entered as percentages instead of decimals | Enter 0.05 for 5%, not 5. Using 5 means 500% growth per period. |
| `Lower than expected` | Negative rates in schedule | Verify negative rates are intentional (representing losses or inflation). |
| `#NAME?` | Function not recognized (older Excel) | Enable Analysis ToolPak add-in in Excel 2003 and earlier via Tools > Add-Ins. |

## Use Cases

### [[Retirement Portfolio Growth Projection]]

**Scenario:** A financial planner is projecting a client's retirement portfolio value over 20 years, using different expected returns for different phases of the investment lifecycle (aggressive early, conservative later).

**Implementation:**
```
=FVSCHEDULE(500000, A1:A20)
```
Where A1:A20 contains:
- Years 1-10 (A1:A10): 0.08 (8% aggressive growth phase)
- Years 11-15 (A11:A15): 0.06 (6% balanced phase)
- Years 16-20 (A16:A20): 0.04 (4% conservative phase)

**Business Application:** Retirement projections should reflect changing asset allocation as clients age. FVSCHEDULE models the glide path from aggressive to conservative investing, providing more realistic projections than assuming a constant average return.

**Technical Details:** Create scenarios with different rate schedules (optimistic, expected, pessimistic) to show the range of possible outcomes. Remember that FVSCHEDULE does not include ongoing contributions--for models with regular deposits, combine with annual FV calculations or use a more complex model.

---

### [[Variable-Rate Investment Product Analysis]]

**Scenario:** A bank is pricing a 5-year CD with a stepped interest rate structure that increases each year to incentivize customers to hold to maturity.

**Implementation:**
```
Face Value: $10,000
Rate Schedule: {0.015, 0.020, 0.025, 0.030, 0.035}
Maturity Value: =FVSCHEDULE(10000, {0.015, 0.020, 0.025, 0.030, 0.035}) = $11,320.77
Effective Annual Rate: =(11320.77/10000)^(1/5)-1 = 2.51%
```

**Business Application:** Banks design stepped-rate products to compete on advertised rates while managing interest rate risk. FVSCHEDULE helps determine the maturity value and calculate the true effective annual rate for comparison with simple fixed-rate alternatives.

**Technical Details:** When marketing these products, clearly disclose the effective annual rate (calculated from FVSCHEDULE result) alongside the scheduled rates. Compare stepped-rate products to fixed-rate alternatives using the same FVSCHEDULE-derived effective rate.

---

### [[Historical Investment Performance Analysis]]

**Scenario:** An analyst is calculating the actual growth of a $10,000 investment in the S&P 500 over a specific historical period to compare with other asset classes.

**Implementation:**
```
=FVSCHEDULE(10000, historical_returns)
```
Where historical_returns contains actual annual S&P 500 returns from historical data

**Business Application:** Historical analysis using actual year-by-year returns captures the sequence of returns effect that average returns miss. An investor who experienced 2008's -37% return followed by 2009's +26% had a different outcome than one with those returns reversed (though FVSCHEDULE actually produces the same endpoint, the psychological and behavioral impacts differ).

**Technical Details:** Source historical return data from reliable financial databases. Include dividends for total return analysis. For comparison across different start dates, normalize to a common starting value ($1 or $10,000). Create rolling FVSCHEDULE calculations to analyze different entry points.

---

### [[Real (Inflation-Adjusted) Value Projection]]

**Scenario:** An economist is projecting the real purchasing power of a fixed pension benefit over 20 years, accounting for varying inflation expectations.

**Implementation:**
```
Nominal Pension: $50,000/year
Inflation Schedule: {-0.03, -0.028, -0.025, ...} (negative because inflation reduces purchasing power)
Real Value Year 20: =FVSCHEDULE(50000, inflation_schedule)
```

**Business Application:** Fixed-income recipients (pensioners, annuitants) need to understand how inflation erodes their purchasing power. FVSCHEDULE with negative rates representing inflation shows the real value of fixed payments over time.

**Technical Details:** Use economic forecasts for near-term inflation and long-term averages for later years. Create multiple scenarios (low inflation, expected inflation, high inflation) to show the range of outcomes. Consider combining with nominal growth projections to show real returns on investments.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2007 and later (built-in); Excel 2003 and earlier requires Analysis ToolPak add-in
- **Specific Behavior:** Accepts array constants {}, cell ranges, and named ranges for the schedule parameter
- **Array Handling:** Schedule must be a single row or column of values

### Google Sheets

- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Built-in, no add-in required.
- **Array Handling:** Same behavior as Excel for array inputs

### Both Platforms

- Rates in schedule are applied sequentially (multiplicatively)
- Negative rates are allowed (represent losses or inflation)
- A rate of 0 means no growth for that period (multiplies by 1)
- Empty cells in the schedule range cause errors
- Order of rates does not affect final result (but matters for period-by-period analysis)

## Tips and Best Practices

1. **Express rates as decimals:** Use 0.05 for 5%, not 5. Using whole numbers will produce wildly incorrect results (500% growth per period instead of 5%).

2. **Match rate periods to schedule length:** If you have monthly rates, your schedule should contain 12 values per year. Annual rates need one value per year. Mixing periods produces incorrect results.

3. **Use named ranges for clarity:** Instead of referencing A1:A20, create a named range like "Return_Schedule" for self-documenting formulas.

4. **Handle negative returns explicitly:** FVSCHEDULE correctly handles negative rates (losses). A -20% return should be entered as -0.20, which multiplies by 0.80 (keeping 80% of value).

5. **Remember: no periodic deposits:** FVSCHEDULE only compounds an initial principal. It does not handle periodic contributions like FV does. For investments with regular deposits at varying rates, you need a more complex model.

6. **Calculate effective annual rates:** To compare a variable-rate schedule to a fixed rate, calculate the effective annual rate: `=(FVSCHEDULE(1, schedule)^(1/n))-1` where n is the number of periods.

7. **Validate with manual calculation:** For simple cases, verify FVSCHEDULE by manually multiplying: Principal x (1+rate1) x (1+rate2) x ... should equal the FVSCHEDULE result.

8. **Use for scenario analysis:** Create multiple rate schedules (optimistic, base case, pessimistic) and calculate FVSCHEDULE for each to understand the range of possible outcomes.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FV]] | Future value with constant interest rate | When the interest rate is the same for all periods |
| [[XNPV]] | Net present value with specific dates | When cash flows occur at irregular dates with varying rates (requires more complex setup) |
| [[PRODUCT]] | Multiplies all values in a range | Could manually implement FVSCHEDULE using Principal * PRODUCT(1+rates) |

### Commonly Used Together

**[[FV]]** - Future Value (Constant Rate)

*Compare variable vs. constant rate outcomes:*
```
Variable: =FVSCHEDULE(10000, rate_schedule)
Constant (at average rate): =FV(AVERAGE(rate_schedule), COUNT(rate_schedule), 0, -10000)
```
Compare FVSCHEDULE result with FV using the average rate to see the impact of rate variability.

---

**[[AVERAGE]]** - Calculate Average Rate

*Determine average rate from schedule:*
```
Rate Schedule: A1:A10
Average Rate: =AVERAGE(A1:A10)
Effective Rate: =(FVSCHEDULE(1, A1:A10)^(1/10))-1
```
The effective rate from FVSCHEDULE may differ from the arithmetic average of rates.

---

**[[GEOMEAN]]** - Geometric Mean

*Calculate geometric mean return:*
```
=(FVSCHEDULE(1, rates))^(1/n) - 1
```
This is equivalent to the geometric mean of (1+rate) values, which represents the compound annual growth rate.

---

**[[PRODUCT]]** - Manual Calculation

*Replicate FVSCHEDULE manually:*
```
=1000 * PRODUCT(1 + A1:A5)
```
This produces the same result as FVSCHEDULE(1000, A1:A5). Useful when you need to modify the calculation or work in platforms without FVSCHEDULE.

---

**[[NPV]]** / **[[IRR]]** - Investment Analysis

*Combine with cash flow analysis:*
```
Project A Ending Value: =FVSCHEDULE(initial_investment, projected_returns)
Compare to: =NPV(discount_rate, projected_cash_flows)
```
Use FVSCHEDULE for growth projections and NPV/IRR for cash flow valuations.

## Official Documentation

- **Microsoft Excel:** [FVSCHEDULE function](https://support.microsoft.com/en-us/office/fvschedule-function-bec29522-bd87-4082-bab9-a241f3fb251d)
- **Google Sheets:** [FVSCHEDULE function](https://support.google.com/docs/answer/3093228)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 (2007) | Built-in function; previously required Analysis ToolPak |
| Excel 2003 and earlier | Analysis ToolPak add-in | Required manual activation of add-in |
| Google Sheets | Original launch | Built-in support from the beginning |

---

*Last updated: 2026-01-10*
