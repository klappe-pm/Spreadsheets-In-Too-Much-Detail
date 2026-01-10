---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- financial-functions
- investment-analysis
subTopics:
- rate-of-return
- growth-rate
- investment-performance
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Rate of Return
- Investment Rate
- Growth Rate
tags:
- excel-only
- financial
- investment
- growth-rate
---

# RRI

## Description

**RRI** calculates the equivalent interest rate (rate of return) for the growth of an investment over a given number of periods. If an investment grew from $1,000 to $2,000 over 10 years, what annual rate of return does that represent? RRI answers: approximately 7.18%. It reverse-engineers the compound growth rate from observed start and end values.

RRI stands for "Rate of Return on Investment" and is mathematically equivalent to CAGR (Compound Annual Growth Rate), a widely-used metric in finance. The function assumes growth occurs through compounding, making it perfect for evaluating investments, comparing performance, and setting growth targets.

**Key understanding:** RRI calculates the constant rate that would produce the observed growth if applied each period. Real-world returns vary year to year, but RRI gives the smoothed equivalent rate. It is the inverse of the compound interest formula: given PV, FV, and n, solve for rate.

**Platform note:** RRI is an Excel-exclusive function introduced in Excel 2013. Google Sheets does not have this function, though the calculation can be replicated using: `=(fv/pv)^(1/nper)-1`. This produces identical results to RRI.

## Syntax

> [!f(x)] RRI Syntax
>
> ```
> =RRI(nper, pv, fv)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `nper` | Yes | The number of periods over which the investment grew. Must be positive. |
| `pv` | Yes | The present value (starting value) of the investment. |
| `fv` | Yes | The future value (ending value) of the investment. |

### Return Value

Returns the equivalent interest rate per period as a decimal. Multiply by 100 for percentage. Returns negative rate if the investment declined in value.

## Examples

> [!f(x)] RRI Examples

### Example 1: Basic CAGR Calculation
```
=RRI(10, 1000, 2000)
```
**Result:** `0.0718` (7.18% annual rate)

**Explanation:** $1,000 growing to $2,000 over 10 years represents a 7.18% compound annual growth rate. This is the standard CAGR calculation.

---

### Example 2: Investment Performance
```
=RRI(5, 50000, 73000)
```
**Result:** `0.0787` (7.87% annually)

**Explanation:** A portfolio that grew from $50,000 to $73,000 over 5 years achieved 7.87% annualized return.

---

### Example 3: Negative Return (Loss)
```
=RRI(3, 10000, 8500)
```
**Result:** `-0.0525` (-5.25% annually)

**Explanation:** An investment that fell from $10,000 to $8,500 over 3 years has a -5.25% annual rate - a loss.

---

### Example 4: Monthly Rate from Annual Data
```
=RRI(5*12, 1000, 1500)
```
**Result:** `0.00676` (0.676% monthly)

**Explanation:** 5 years = 60 months. The monthly rate that grows $1,000 to $1,500 is 0.676% per month, or about 8.4% annually.

---

### Example 5: Business Revenue Growth
```
=RRI(4, 2500000, 4000000)
```
**Result:** `0.1247` (12.47% annually)

**Explanation:** Company revenue growing from $2.5M to $4M over 4 years represents 12.47% annual growth - useful for business valuation.

---

### Example 6: Population Growth Rate
```
=RRI(50, 150000000, 330000000)
```
**Result:** `0.0158` (1.58% annually)

**Explanation:** US population growth from 150M to 330M over 50 years averages 1.58% annual growth. RRI works for any growth scenario.

---

### Example 7: Inflation Rate from Price Changes
```
=RRI(20, 100, 180)
```
**Result:** `0.0298` (2.98% annually)

**Explanation:** If prices rose 80% over 20 years (from index 100 to 180), the average annual inflation was 2.98%.

---

### Example 8: Compare Two Investments
```
Investment A: =RRI(5, 10000, 18000)
Investment B: =RRI(5, 15000, 22000)
```
**Result:** A: 12.47%, B: 7.96%

**Explanation:** Despite Investment B ending higher, Investment A had better relative performance. RRI enables fair comparison of different starting amounts.

---

### Example 9: Required Return for Goal
```
=RRI(15, 100000, 500000)
```
**Result:** `0.1119` (11.19% annually)

**Explanation:** To grow $100,000 to $500,000 in 15 years requires 11.19% annual return. This helps set realistic investment expectations.

---

### Example 10: Quarterly Rate
```
=RRI(20, 10000, 25000)
```
**Result:** `0.0469` (4.69% per quarter if nper=20 quarters)

**Explanation:** If 20 represents quarters, the quarterly rate is 4.69%. Always ensure nper units match your intended period.

---

### Example 11: Real vs Nominal Return
```
Nominal: =RRI(10, 10000, 25000)
Inflation: =RRI(10, 100, 130)
Real: Nominal - Inflation (approximately)
```
**Result:** Nominal: 9.6%, Inflation: 2.66%, Real: ~6.9%

**Explanation:** Real return adjusts for inflation. Subtract inflation rate from nominal rate for approximate real return.

---

### Example 12: Breakeven Rate After Loss
```
=RRI(5, 8000, 10000)
```
**Result:** `0.0456` (4.56% annually)

**Explanation:** After a 20% loss ($10,000 to $8,000), you need 4.56% annually for 5 years to recover. Recovery rate is always higher than loss rate.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | NPER is zero or negative | Number of periods must be positive |
| `#NUM!` | PV and FV have opposite signs and result in invalid calculation | Ensure PV and FV are both positive or handle sign conventions |
| `#VALUE!` | Non-numeric arguments | Ensure all parameters are numbers |
| `#NAME?` | Function not recognized | Requires Excel 2013+; use formula `=(fv/pv)^(1/nper)-1` in older versions |
| `#DIV/0!` | PV is zero | Present value cannot be zero |

## Use Cases

### [[Investment Performance Evaluation]]

**Scenario:** Calculate the actual return achieved by an investment portfolio.

**Implementation:**
```
=RRI(YEARFRAC(StartDate, EndDate), StartingValue, CurrentValue)
```

**Business Application:** Portfolio managers report CAGR to clients. RRI with YEARFRAC handles partial years accurately, giving precise performance metrics.

**Technical Details:** YEARFRAC calculates exact fractional years between dates. This is more accurate than counting whole years for mid-year calculations.

---

### [[Business Growth Analysis]]

**Scenario:** Analyze company revenue or profit growth over multiple years.

**Implementation:**
```
=RRI(ROWS(Revenue)-1, FIRST(Revenue), LAST(Revenue))
```
Where Revenue is a column of annual values.

**Business Application:** Due diligence, company valuations, and investor presentations all use CAGR metrics. RRI provides the standard calculation.

**Technical Details:** ROWS counts periods (minus 1 because n values span n-1 growth periods). FIRST and LAST extract endpoints.

---

### [[Goal-Based Planning]]

**Scenario:** Determine what return rate is needed to reach a financial goal.

**Implementation:**
```
=RRI(YearsToGoal, CurrentSavings, GoalAmount)
```

**Business Application:** Financial planning requires understanding required returns. "You need 8% annually to retire with $1M" is more actionable than abstract growth discussions.

**Technical Details:** Compare the required rate to realistic expected returns. If RRI produces 15%, the goal may need adjustment or timeline extension.

## Platform Differences

### Microsoft Excel (2013+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Negative returns | Full support |
| Large value ranges | Full support |

### Google Sheets

| Feature | Support |
|---------|---------|
| RRI | NOT AVAILABLE |
| Alternative | Use formula: `=(fv/pv)^(1/nper)-1` |

**Workaround for Google Sheets:**
```
=(B2/A2)^(1/C2)-1
```
Where A2=PV, B2=FV, C2=NPER. This produces identical results to RRI.

### Other Platforms

RRI availability:
- Microsoft Excel 2013+: Full support
- Microsoft Excel 2010 and earlier: Not available
- Google Sheets: Not available (use formula workaround)
- LibreOffice Calc: Not available (use formula workaround)
- Apple Numbers: Not available

## Tips and Best Practices

1. **Use RRI for CAGR:** RRI is the standard Excel function for Compound Annual Growth Rate calculations. CAGR = RRI.

2. **Match period units:** If nper is in months, the result is monthly rate. For annual rate with monthly data, divide nper by 12.

3. **Handle negative growth correctly:** RRI returns negative rates for declining values - this is correct behavior, not an error.

4. **Convert to percentage:** RRI returns decimals. Multiply by 100 or format as percentage for display.

5. **Combine with PDURATION:** RRI and PDURATION are inverses. RRI finds rate given periods; PDURATION finds periods given rate.

6. **Verify with FV:** `=FV(RRI(n,pv,fv), n, 0, -pv)` should equal fv. Use for validation.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PDURATION]] | Calculates periods given rate and values | When you know the rate and want to find time |
| [[RATE]] | Calculates rate with regular payments | When there are periodic payments, not just endpoints |
| [[IRR]] | Internal rate of return for cash flow series | For irregular cash flows, not just start/end values |

### Commonly Used Together

**[[PDURATION]]** - Find how long at known rate

*Inverse relationship:*
```
RRI: "What rate grew $10K to $20K in 10 years?" = 7.18%
PDURATION: "How long at 7.18% to double?" = 10 years
```
These functions are mathematical inverses.

---

**[[FV]]** - Project future value at calculated rate

*Verify and project:*
```
=FV(RRI(10,1000,2000), 10, 0, -1000) = 2000 (verification)
=FV(RRI(10,1000,2000), 20, 0, -1000) = 4000 (projection)
```
Once you know the rate, project future values.

---

**[[YEARFRAC]]** - Calculate exact period length

*Precise partial-year calculations:*
```
=RRI(YEARFRAC(StartDate, EndDate), StartValue, EndValue)
```
YEARFRAC provides exact fractional years for accurate rates.

---

**[[XIRR]]** - For irregular cash flows

*When RRI isn't enough:*
```
RRI: Start and end values only
XIRR: Multiple investments/withdrawals at different dates
```
Use XIRR when cash flows occur throughout the period.

## Official Documentation

- **Microsoft Excel:** [RRI function](https://support.microsoft.com/en-us/office/rri-function-6f5822d8-7ef1-4233-944c-79e8172c1aa6)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 | 2013 | Initial release |
| Excel 2016+ | 2016 | Continued support |
| Excel 365 | Current | Full support |
| Earlier versions | Not available | Use `=(fv/pv)^(1/nper)-1` instead |

---

*Last updated: 2026-01-10*
