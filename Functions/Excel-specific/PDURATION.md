---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- financial-functions
- investment-analysis
subTopics:
- time-value-of-money
- growth-calculations
- investment-duration
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Period Duration
- Investment Duration
- Growth Period
tags:
- excel-only
- financial
- investment
- growth
---

# PDURATION

## Description

**PDURATION** calculates the number of periods required for an investment to reach a specified future value, given a constant interest rate. If you have $10,000 invested at 8% annual return and want to know when it will become $25,000, PDURATION tells you: approximately 11.9 years. It answers the fundamental question: "How long until my money grows to my target?"

The function assumes compound growth at a constant rate, making it ideal for projection planning. Unlike PMT or FV which work with regular payment streams, PDURATION focuses on lump-sum investments growing through compounding. It is mathematically equivalent to solving for n in the compound interest formula: FV = PV * (1 + rate)^n.

**Important clarification:** PDURATION calculates periods, not necessarily years. If your rate is monthly (say, 0.5% per month), the result is in months. If your rate is quarterly, the result is in quarters. Always ensure your rate matches your desired time unit. The function returns a decimal value representing the exact duration, including fractional periods.

**Platform note:** PDURATION is an Excel-exclusive function introduced in Excel 2013. Google Sheets does not have this function, though the calculation can be replicated using logarithms: `=LOG(fv/pv)/LOG(1+rate)`. LibreOffice Calc also lacks PDURATION.

## Syntax

> [!f(x)] PDURATION Syntax
>
> ```
> =PDURATION(rate, pv, fv)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rate` | Yes | The interest rate per period, expressed as a decimal (e.g., 0.08 for 8%). Must be greater than 0. |
| `pv` | Yes | The present value (starting amount) of the investment. Must be positive. |
| `fv` | Yes | The future value (target amount) you want to reach. Must be positive. |

### Return Value

Returns the number of periods (as a decimal) required for the present value to grow to the future value at the specified interest rate.

## Examples

> [!f(x)] PDURATION Examples

### Example 1: Basic Investment Growth Time
```
=PDURATION(0.08, 10000, 20000)
```
**Result:** `9.006` (approximately 9 years)

**Explanation:** At 8% annual interest, $10,000 takes about 9 years to double. This is the classic "how long to double" calculation.

---

### Example 2: Rule of 72 Verification
```
=PDURATION(0.06, 1, 2)
```
**Result:** `11.90` years

**Explanation:** The Rule of 72 suggests 72/6 = 12 years to double at 6%. PDURATION gives the precise answer: 11.9 years. Close to the rule but more accurate.

---

### Example 3: Retirement Planning
```
=PDURATION(0.07, 50000, 1000000)
```
**Result:** `44.21` years

**Explanation:** Starting with $50,000 and earning 7% annually, it takes about 44 years to reach $1 million. Critical for long-term retirement planning.

---

### Example 4: Monthly Rate Calculation
```
=PDURATION(0.005, 5000, 10000)
```
**Result:** `138.98` months (about 11.6 years)

**Explanation:** At 0.5% monthly interest (approximately 6% annual), $5,000 takes about 139 months to double. Note: result is in months because rate is monthly.

---

### Example 5: Quarterly Compounding
```
=PDURATION(0.02, 10000, 15000)
```
**Result:** `20.47` quarters (about 5.1 years)

**Explanation:** At 2% per quarter (8% annual), $10,000 grows to $15,000 in about 20.5 quarters. Converting quarterly result to years: divide by 4.

---

### Example 6: Small Growth Target
```
=PDURATION(0.10, 1000, 1100)
```
**Result:** `0.95` years

**Explanation:** A 10% gain at 10% interest takes slightly less than 1 year. For small percentage gains, the period approximates the percentage divided by rate.

---

### Example 7: Large Multiple Growth
```
=PDURATION(0.12, 1000, 10000)
```
**Result:** `20.32` years

**Explanation:** Growing 10x at 12% annual return takes about 20 years. High returns significantly shorten the time to reach large multiples.

---

### Example 8: Comparing Different Rates
```
=PDURATION(0.05, 10000, 50000) vs =PDURATION(0.10, 10000, 50000)
```
**Result:** `32.99` years at 5% vs `16.89` years at 10%

**Explanation:** Doubling the interest rate nearly halves the time. This demonstrates the power of higher returns over long periods.

---

### Example 9: College Savings Planning
```
=PDURATION(0.06, 25000, 100000)
```
**Result:** `23.79` years

**Explanation:** Starting college savings at $25,000, how long until it reaches $100,000 at 6%? About 24 years - start when the child is born if target is college at 18.

---

### Example 10: Inflation Adjustment
```
=PDURATION(0.02, 100, 200)
```
**Result:** `35.00` years

**Explanation:** At 2% inflation, prices double in 35 years. PDURATION works for inflation calculations too - just use inflation rate instead of investment return.

---

### Example 11: Break-Even Analysis
```
=PDURATION(0.04, 80000, 100000)
```
**Result:** `5.68` years

**Explanation:** If you bought an asset at $100,000, sold at $80,000, how long at 4% growth until you would have had your original value back? About 5.7 years.

---

### Example 12: Real vs Nominal Return
```
=PDURATION((0.08-0.03), 10000, 20000)
```
**Result:** `14.21` years

**Explanation:** With 8% nominal return and 3% inflation, real return is approximately 5%. Real doubling time is 14 years, not 9 years at nominal rate.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Rate is zero or negative | Rate must be positive (greater than 0) |
| `#NUM!` | PV or FV is zero or negative | Both present and future values must be positive |
| `#NUM!` | FV is less than PV | Future value must be greater than present value for positive rate |
| `#VALUE!` | Non-numeric arguments | Ensure all parameters are numbers or cell references containing numbers |
| `#NAME?` | Function not recognized | Requires Excel 2013+; not available in earlier versions |

## Use Cases

### [[Investment Goal Planning]]

**Scenario:** Determine how long until a current investment reaches a target value at expected returns.

**Implementation:**
```
=PDURATION(ExpectedReturn, CurrentValue, TargetValue)
```

**Business Application:** Financial advisors use this to set realistic client expectations. "At 7% return, your $100,000 will become $500,000 in approximately 24 years."

**Technical Details:** Use conservative return estimates. Historical stock market returns average 7-10% nominal, but individual investments vary significantly.

---

### [[Comparing Investment Strategies]]

**Scenario:** Evaluate how different return rates affect the time to reach financial goals.

**Implementation:**
```
Conservative: =PDURATION(0.05, 50000, 200000)
Moderate: =PDURATION(0.07, 50000, 200000)
Aggressive: =PDURATION(0.09, 50000, 200000)
```

**Business Application:** Portfolio allocation discussions benefit from showing concrete time differences. The gap between 5% and 9% returns translates to years or decades of difference.

**Technical Details:** Create sensitivity tables showing duration across ranges of rates and targets to visualize trade-offs.

---

### [[Loan Payoff Acceleration]]

**Scenario:** Calculate how long until extra payments reduce debt to a target level.

**Implementation:**
```
=PDURATION(MonthlyRate, CurrentBalance, TargetBalance)
```
Note: For debt reduction, you're looking at how negative growth (payments exceeding interest) brings balance down.

**Business Application:** Help borrowers understand the impact of making extra payments by showing the accelerated payoff timeline.

**Technical Details:** For declining balances, restructure the calculation: solve for when accelerated payments reduce principal to zero versus minimum payments.

## Platform Differences

### Microsoft Excel (2013+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Decimal period results | Full support |
| Large value ranges | Full support |

### Google Sheets

| Feature | Support |
|---------|---------|
| PDURATION | NOT AVAILABLE |
| Alternative | Use logarithm formula |

**Workaround for Google Sheets:**
```
=LOG(fv/pv)/LOG(1+rate)
```
This formula produces identical results to PDURATION.

### Other Platforms

PDURATION availability:
- Microsoft Excel 2013+: Full support
- Microsoft Excel 2010 and earlier: Not available
- Google Sheets: Not available (use LOG formula)
- LibreOffice Calc: Not available (use LOG formula)
- Apple Numbers: Not available

## Tips and Best Practices

1. **Match rate to desired time units:** If you want years, use annual rate. For months, use monthly rate. Result units match rate period units.

2. **Use for "Rule of 72" verification:** PDURATION(rate, 1, 2) gives exact doubling time. Compare to 72/rate% for education.

3. **Consider real returns:** Subtract inflation from nominal return for real purchasing power analysis: `=PDURATION(nominal-inflation, pv, fv)`.

4. **Round results appropriately:** PDURATION returns decimals. For practical planning, round up (CEILING) to ensure the target is actually reached.

5. **Combine with FV for validation:** After calculating duration, verify with `=FV(rate, periods, 0, -pv)` should equal fv.

6. **Handle percentage inputs carefully:** Enter 8% as 0.08, not 8. This is the most common mistake.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[RRI]] | Calculates interest rate given periods and values | When you know duration and want to find required rate |
| [[NPER]] | Calculates periods with regular payments | When there are periodic contributions, not just lump sum |
| [[FV]] | Calculates future value | When you know periods and want to find ending value |

### Commonly Used Together

**[[FV]]** - Calculate what investment will be worth

*Validate PDURATION calculations:*
```
=FV(0.08, PDURATION(0.08, 10000, 20000), 0, -10000)
```
Should return 20000 (the target FV). Useful for verification.

---

**[[RRI]]** - Find required rate of return

*Inverse of PDURATION:*
```
PDURATION: "How long at 8% to reach $20,000?"
RRI: "What rate to reach $20,000 in 10 years?"
```
These functions solve for different unknowns in the same equation.

---

**[[PV]]** - Calculate present value needed

*Planning backwards:*
```
=PV(rate, PDURATION(rate, current, target), 0, -target)
```
Find how much you need today to reach a goal in calculated time.

---

**[[RATE]]** - Calculate effective rate

*Alternative rate calculation:*
```
=RATE(periods, 0, -pv, fv)
```
RATE handles payment streams; RRI is simpler for lump sums.

## Official Documentation

- **Microsoft Excel:** [PDURATION function](https://support.microsoft.com/en-us/office/pduration-function-44f33460-5be5-4c90-b857-22308892adaf)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 | 2013 | Initial release |
| Excel 2016+ | 2016 | Continued support |
| Excel 365 | Current | Full support |
| Earlier versions | Not available | Use LOG(fv/pv)/LOG(1+rate) instead |

---

*Last updated: 2026-01-10*
