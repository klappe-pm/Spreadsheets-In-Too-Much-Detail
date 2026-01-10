---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- year-fraction
- day-count-convention
- financial-calculations
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Year Fraction
- Fractional Year
- Day Count Fraction
tags:
- yearfrac
- year-fraction
- day-count-convention
- interest-calculation
- financial
- basis
---

# YEARFRAC

## Description

**YEARFRAC** calculates the fraction of a year represented by the number of whole days between two dates. This function is essential for financial calculations including interest accrual, bond pricing, loan amortization, and any computation requiring precise time periods expressed as decimal years. A half year might be 0.5, a quarter might be 0.25, but the exact value depends on the day count convention used.

The function's third parameter, `basis`, specifies the day count convention—a set of rules for how to count days in a year. Different financial instruments and markets use different conventions. The US (NASD) method uses 30/360, where every month has 30 days and the year has 360 days. The actual/actual method counts real calendar days and divides by actual days in the year. The choice of basis significantly affects calculations and must match the convention specified in the financial instrument's terms.

**Day count conventions explained:** The basis parameter offers five options: 0 (US 30/360 NASD), 1 (Actual/Actual), 2 (Actual/360), 3 (Actual/365), and 4 (European 30/360). Each produces different results for the same date range. For example, February's 28 days might count as 30 days (in 30/360) or 28 days (in Actual methods). Interest calculations, bond yields, and other financial metrics depend critically on using the correct convention.

YEARFRAC is commonly used with financial functions like PV, FV, PMT, and RATE when time periods don't align with simple monthly or annual intervals. It's essential for pro-rata calculations, partial period interest, and anywhere you need to express a date span as a fraction of a year. The function pairs with DAYS and DATEDIF for different perspectives on time intervals.

## Syntax

> [!f(x)] YEARFRAC Syntax
>
> ```
> =YEARFRAC(start_date, end_date, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `start_date` | Yes | The starting date of the period. |
| `end_date` | Yes | The ending date of the period. |
| `basis` | No | The day count basis to use. Default is 0 if omitted. See basis values table below. |

### Basis Values (Day Count Conventions)

| Basis | Method | Days per Month | Days per Year | Common Usage |
|-------|--------|----------------|---------------|--------------|
| 0 or omitted | US (NASD) 30/360 | 30 | 360 | US corporate bonds, mortgages |
| 1 | Actual/Actual | Actual | Actual (365 or 366) | US Treasury bonds, international |
| 2 | Actual/360 | Actual | 360 | US commercial paper, money markets |
| 3 | Actual/365 | Actual | 365 | Japanese bonds, some international |
| 4 | European 30/360 | 30 | 360 | European bonds, Eurobonds |

### Return Value

Returns a decimal number representing the fraction of a year between the two dates. For example, 0.5 represents half a year, 1.0 represents one full year, 2.5 represents two and a half years.

## Examples

> [!f(x)] YEARFRAC Examples

### Example 1: Basic Year Fraction (Default 30/360)
```
=YEARFRAC(DATE(2026, 1, 1), DATE(2026, 7, 1))
```
**Result:** 0.5

**Explanation:** January 1 to July 1 is exactly 6 months, which equals 0.5 years in 30/360 convention.

---

### Example 2: Full Year
```
=YEARFRAC(DATE(2026, 1, 1), DATE(2027, 1, 1))
```
**Result:** 1.0

**Explanation:** One complete year equals exactly 1.0 regardless of leap year status in 30/360.

---

### Example 3: Actual/Actual Method
```
=YEARFRAC(DATE(2026, 1, 1), DATE(2026, 7, 1), 1)
```
**Result:** 0.4959 (approximately)

**Explanation:** With Actual/Actual, 181 days divided by 365 gives a slightly different result than 30/360.

---

### Example 4: Leap Year with Actual/Actual
```
=YEARFRAC(DATE(2024, 1, 1), DATE(2024, 7, 1), 1)
```
**Result:** 0.4973 (approximately)

**Explanation:** In leap year 2024, 182 days (Jan-Jun including Feb 29) divided by 366 days in the year.

---

### Example 5: Quarter Calculation
```
=YEARFRAC(DATE(2026, 1, 1), DATE(2026, 4, 1))
```
**Result:** 0.25

**Explanation:** 3 months = 0.25 years in 30/360. Useful for quarterly pro-rations.

---

### Example 6: Interest Accrual Calculation
```
=principal * annual_rate * YEARFRAC(start_date, end_date, 2)
```
**Result:** Accrued interest using Actual/360

**Explanation:** Money market instruments often use Actual/360, where each day accrues 1/360 of annual interest.

---

### Example 7: Compare Basis Methods
```
=YEARFRAC(A1, B1, 0) & " | " & YEARFRAC(A1, B1, 1) & " | " & YEARFRAC(A1, B1, 2)
```
**Result:** Shows differences between 30/360, Actual/Actual, and Actual/360

**Explanation:** Demonstrates how the same date range produces different fractions depending on convention.

---

### Example 8: Pro-Rata Salary Calculation
```
=annual_salary * YEARFRAC(hire_date, end_of_year)
```
**Result:** Portion of annual salary earned from hire to year-end

**Explanation:** Employee hired mid-year earns a fractional annual salary based on time worked.

---

### Example 9: Bond Accrued Interest
```
=face_value * coupon_rate * YEARFRAC(last_coupon_date, settlement_date, 4)
```
**Result:** Accrued interest on a European bond

**Explanation:** Eurobonds typically use European 30/360 (basis 4). Buyer pays seller accrued interest.

---

### Example 10: Days to Year Conversion
```
=YEARFRAC(A1, A1 + 90, 3)
```
**Result:** 0.2466 (90/365)

**Explanation:** Converting 90 days to years using Actual/365. Useful for comparing periods of different lengths.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date in start_date or end_date | Ensure both are valid date values |
| `#NUM!` | Invalid basis value | Use only 0, 1, 2, 3, or 4 |
| `#NUM!` | Negative date range not supported | Ensure end_date >= start_date (or handle with ABS) |
| Unexpected result | Wrong basis for your use case | Verify which day count convention applies to your calculation |
| Slight precision differences | Different platforms/versions | Small rounding differences may occur; usually immaterial |

## Use Cases

### [[Interest and Loan Calculations]]

**Scenario:** Calculate accrued interest, partial period payments, and pro-rated charges for financial instruments using the appropriate day count convention.

**Implementation:**
```
=principal * annual_rate * YEARFRAC(start, end, 0)           (US corporate bond)
=loan_balance * rate * YEARFRAC(last_payment, today, 2)      (Money market)
=face_value * coupon * YEARFRAC(last_coupon, settlement, 1)  (Treasury bond)
```

**Business Application:** Banks calculate interest accruals for month-end. Trading desks compute accrued interest for bond settlements. Loan servicers prorate interest for early payoffs.

**Technical Details:** Always verify which basis applies to your specific instrument. Bond indentures and loan documents specify the day count convention. Using the wrong basis leads to incorrect interest calculations that may be legally problematic.

---

### [[Employee Compensation Proration]]

**Scenario:** Calculate partial year compensation for mid-year hires, terminations, bonuses, and benefit accruals.

**Implementation:**
```
=annual_salary * YEARFRAC(hire_date, year_end)               (Partial year salary)
=bonus_target * YEARFRAC(start_date, end_date)               (Pro-rated bonus)
=PTO_annual_days * YEARFRAC(hire_date, anniversary)          (Accrued PTO)
```

**Business Application:** HR calculates partial-year compensation for new hires. Finance prorates bonuses for mid-year promotions. Benefits administrators compute accrued vacation time.

**Technical Details:** For HR purposes, Actual/Actual (basis 1) usually makes most sense—it reflects actual calendar days. Some companies use 30/360 for simplicity in monthly calculations.

---

### [[Financial Reporting and Analysis]]

**Scenario:** Express time periods as decimal years for comparative analysis, forecasting, and financial modeling.

**Implementation:**
```
=revenue_growth / YEARFRAC(period_start, period_end, 1)      (Annualized growth rate)
=LN(end_value / start_value) / YEARFRAC(start, end, 1)       (Continuous growth rate)
=NPV(annual_rate * YEARFRAC(base, date, 1), cashflows)       (Irregular period NPV)
```

**Business Application:** Analysts compare returns across different holding periods. Modelers annualize returns for consistent comparison. Forecasters project trends based on partial-year data.

**Technical Details:** For annualized returns, YEARFRAC converts any period to a standardized annual basis. A 20% return over 3 months doesn't equal 80% annual return (compound effects apply), but YEARFRAC provides the time basis for proper annualization.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (native); Analysis ToolPak in earlier versions
- **Basis values:** Full support for 0-4
- **Precision:** Standard floating-point precision
- **Negative ranges:** Returns error for negative date ranges

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel
- **Basis values:** Same 0-4 options
- **Compatibility:** Full cross-platform compatibility

### Key Difference Alert
YEARFRAC behaves identically across platforms. The critical consideration is choosing the correct basis for your specific financial instrument or use case. Different industries and regions use different conventions, and using the wrong one produces incorrect results.

## Tips and Best Practices

1. **Verify the correct basis:** Financial instruments specify their day count convention. Corporate bonds typically use 30/360 (basis 0); Treasuries use Actual/Actual (basis 1); money markets use Actual/360 (basis 2). Using the wrong basis gives incorrect results.

2. **Document your basis choice:** When building financial models, clearly note which day count convention you're using. Future users need to understand and verify the assumptions.

3. **Use for annualizing returns:** To annualize a return over a partial period: (1 + return)^(1/YEARFRAC(start, end, 1)) - 1. This properly accounts for compounding.

4. **Combine with financial functions:** YEARFRAC provides the time periods for PV, FV, and other functions when periods aren't simple months or years.

5. **Test with known dates:** Verify your understanding by testing with simple date ranges. January 1 to July 1 should be 0.5 in 30/360, slightly less in Actual/365.

6. **Handle leap years consciously:** Actual/Actual (basis 1) handles leap years correctly. Other methods may produce slightly different results in leap years versus non-leap years.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DAYS]] | Returns integer days between dates | When you need whole days, not year fraction |
| [[DATEDIF]] | Returns days, months, or years between dates | When you need component-based differences |
| [[DAYS360]] | Returns days using 30/360 convention | When you specifically need 30/360 day count |

### Commonly Used Together

**[[DAYS]]** - Compare with day count

*Understand the relationship:*
```
=YEARFRAC(A1, B1, 3)              // Fraction of year (Actual/365)
=DAYS(B1, A1) / 365               // Manual equivalent for basis 3
```
YEARFRAC handles the convention logic; DAYS gives raw day count.

---

**[[PV]] / [[FV]] / [[PMT]]** - Financial calculations

*Use YEARFRAC for irregular periods:*
```
=FV(annual_rate * YEARFRAC(start, end, 1), 1, 0, -principal)
=PV(annual_rate * YEARFRAC(start, end), 1, 0, -future_value)
```
Convert annual rates to partial-period rates.

---

**[[RATE]]** - Calculate effective rate

*Annualize observed returns:*
```
=((end_value / start_value)^(1/YEARFRAC(start, end, 1)) - 1)
=(LN(end / start)) / YEARFRAC(start, end, 1)    // Continuous rate
```
Convert absolute returns to annualized rates.

---

**[[DAYS360]]** - Compare conventions

*Understand 30/360 day count:*
```
=YEARFRAC(A1, B1, 0)              // 30/360 year fraction
=DAYS360(A1, B1) / 360            // Manual equivalent
```
DAYS360 gives the numerator that YEARFRAC with basis 0 uses.

## Official Documentation

- **Microsoft Excel:** [YEARFRAC function](https://support.microsoft.com/en-us/office/yearfrac-function-3844141e-c76d-4143-82b6-208454ddc6a8)
- **Google Sheets:** [YEARFRAC function](https://support.google.com/docs/answer/3093305)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2007 | 2007 | Native function (previously Analysis ToolPak) |
| Excel 2010+ | 2010 | No changes |
| Excel 365 | Current | No changes |
| Google Sheets | Original launch (2006) | Full support |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
