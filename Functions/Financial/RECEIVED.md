---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- bond-analysis
- fixed-income
- maturity-calculation
- securities
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Amount Received at Maturity
- Maturity Value
- Redemption Amount
tags:
- financial-modeling
- bond-analysis
- fixed-income
- money-market
---

# RECEIVED

## Description

**RECEIVED** calculates the amount received at maturity for a fully invested security--one where you invest a principal amount and receive a larger amount at maturity based on a stated interest rate. This function answers the question: "If I invest this amount at this rate for this period, how much will I get back?" It is the inverse of INTRATE: while INTRATE calculates the rate from amounts, RECEIVED calculates the amount from a rate.

RECEIVED is used for securities that pay all interest at maturity rather than periodically, such as certificates of deposit (CDs), short-term notes, banker's acceptances, and certain money market instruments. The calculation is simple interest: Maturity Amount = Investment x (1 + Rate x Days/Year). Unlike compound interest, where interest earns interest, simple interest applies the rate only to the original principal.

The day count convention (basis parameter) critically affects the result because it determines how "days" and "year" are counted. A 5% rate over 90 days produces different results under Actual/360 (90/360 = 0.25 of a year) versus Actual/365 (90/365 = 0.247 of a year). Money market instruments typically use Actual/360, which results in slightly higher interest payments because you are dividing by a smaller year. Always match the basis to the security's market convention.

RECEIVED has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is commonly used in treasury operations to calculate expected cash flows, project maturity proceeds for reinvestment planning, and verify that instruments are paying the expected amounts. The function pairs naturally with INTRATE for validation.

## Syntax

> [!f(x)] RECEIVED Syntax
>
> ```
> =RECEIVED(settlement, maturity, investment, discount, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you make the investment. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The security's maturity date--when you receive the maturity amount. Must be after settlement. |
| `investment` | Yes | The amount invested (purchase price/principal). Enter as positive number. |
| `discount` | Yes | The security's annual interest rate as a decimal. For 5% rate, enter 0.05. (Note: parameter named "discount" but represents interest rate.) |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year. | CDs, corporate instruments |
| 1 | Actual/Actual | Actual days in period and year. | Government securities |
| 2 | Actual/360 | Actual days, divide by 360. | Money market, T-bills |
| 3 | Actual/365 | Actual days, divide by 365. | UK sterling instruments |
| 4 | European 30/360 | European month-end rules. | European markets |

### Return Value

Returns a numeric value representing the amount received at maturity (principal plus interest).

## Examples

> [!f(x)] RECEIVED Examples

### Example 1: Basic CD Maturity Calculation
```
=RECEIVED("2024-01-15", "2024-07-15", 100000, 0.05, 0)
```
**Result:** `$102,500.00`

**Explanation:** A $100,000 CD at 5% for 6 months (180 days under 30/360). Interest = $100,000 x 0.05 x (180/360) = $2,500. Maturity amount = $102,500. The investor receives principal plus interest at maturity.

---

### Example 2: Money Market (Actual/360)
```
=RECEIVED("2024-06-01", "2024-09-01", 1000000, 0.055, 2)
```
**Result:** `$1,014,027.78`

**Explanation:** $1 million invested for 92 days at 5.5% using Actual/360. Interest = $1M x 0.055 x (92/360) = $14,055.56. Actual/360 is standard for money market instruments and results in slightly more interest than 30/360.

---

### Example 3: Short-Term Investment (30 Days)
```
=RECEIVED("2024-06-01", "2024-07-01", 500000, 0.0525, 2)
```
**Result:** `$502,187.50`

**Explanation:** $500,000 for 30 days at 5.25%. Interest = $500K x 0.0525 x (30/360) = $2,187.50. Short-term investments are common in corporate treasury for managing daily cash positions.

---

### Example 4: UK Style (Actual/365)
```
=RECEIVED("2024-03-01", "2024-09-01", 100000, 0.05, 3)
```
**Result:** `$102,520.55`

**Explanation:** Using Actual/365 (184 days from Mar 1 to Sep 1). Interest = $100K x 0.05 x (184/365) = $2,520.55. UK sterling instruments typically use this convention.

---

### Example 5: Comparing Day Count Impact
```
30/360:      =RECEIVED("2024-01-15", "2024-07-15", 100000, 0.05, 0) = $102,500.00
Actual/360:  =RECEIVED("2024-01-15", "2024-07-15", 100000, 0.05, 2) = $102,527.78
Actual/365:  =RECEIVED("2024-01-15", "2024-07-15", 100000, 0.05, 3) = $102,493.15
```
**Result:** Different conventions give different amounts

**Explanation:** The same investment at the same rate produces different maturity amounts depending on day count convention. Actual/360 gives the most interest (divides by 360), Actual/365 gives the least (divides by 365). The differences are $27.78 and -$6.85 from baseline.

---

### Example 6: Overnight Investment
```
=RECEIVED("2024-06-14", "2024-06-15", 10000000, 0.0535, 2)
```
**Result:** `$10,001,486.11`

**Explanation:** $10 million overnight at 5.35% fed funds rate. Interest = $10M x 0.0535 x (1/360) = $1,486.11. Overnight rates are calculated using Actual/360. Banks use this for daily cash management.

---

### Example 7: One-Year Zero-Coupon
```
=RECEIVED("2024-01-01", "2025-01-01", 95000, 0.0526, 1)
```
**Result:** `$100,000.26`

**Explanation:** Investing $95,000 at 5.26% for one year using Actual/Actual. The $5,000 interest equals 5.26% return on the $95,000 investment. This matches what a zero-coupon bond would pay.

---

### Example 8: Verifying with INTRATE
```
Maturity Amount: =RECEIVED("2024-06-01", "2024-09-01", 98000, 0.0525, 2) = $99,314.17
Rate Check: =INTRATE("2024-06-01", "2024-09-01", 98000, 99314.17, 2) = 5.25% ✓
```
**Result:** RECEIVED and INTRATE are consistent

**Explanation:** RECEIVED calculates maturity amount from rate; INTRATE calculates rate from amounts. They should be mathematically consistent. Use this cross-check to verify calculations.

---

### Example 9: Large Corporate Investment
```
=RECEIVED("2024-03-15", "2024-06-15", 50000000, 0.0575, 2)
```
**Result:** `$50,718,750.00`

**Explanation:** $50 million corporate investment for 92 days at 5.75%. Interest = $718,750. Large corporations earn significant interest on short-term cash reserves. Treasury operations optimize placement across multiple instruments.

---

### Example 10: Using Cell References
```
=RECEIVED(A2, B2, C2, D2, E2)
```
Where A2=Settlement, B2=Maturity, C2=Investment, D2=Rate, E2=Basis

**Result:** Varies by inputs

**Explanation:** Building a cash flow model with cell references allows projecting maturity proceeds across multiple investments. Link to position and rate data for automated cash forecasting.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric parameters | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. Check date order. |
| `#NUM!` | Negative investment or discount rate | Investment must be positive. Rate should be positive for normal investments. |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Amount too high/low` | Wrong day count convention | Match basis to instrument type. Money market uses basis=2 (Actual/360). |

## Use Cases

### [[Treasury Cash Flow Forecasting]]

**Scenario:** A corporate treasurer needs to project cash inflows from maturing money market investments over the next 30 days for cash planning.

**Implementation:**
```
Investment Schedule:
Week 1: =RECEIVED("2024-01-01", "2024-06-07", 5000000, 0.055, 2) = $5,076,458
Week 2: =RECEIVED("2024-01-15", "2024-06-14", 3000000, 0.0525, 2) = $3,043,750
Week 3: =RECEIVED("2024-02-01", "2024-06-21", 4000000, 0.05, 2) = $4,058,333
Week 4: =RECEIVED("2024-02-15", "2024-06-28", 2000000, 0.0575, 2) = $2,034,028

Total 30-day inflows: $14,212,569
```

**Business Application:** Corporate treasurers must ensure sufficient cash for payroll, vendor payments, and capital expenditures. RECEIVED projects exactly how much cash will become available from maturing investments, enabling precise liquidity planning.

**Technical Details:** Match the basis parameter to each instrument's actual convention. Build a rolling 30/60/90-day forecast by updating settlement and maturity dates. Integrate with banking systems for automated updates.

---

### [[Investment Decision: Rate Comparison]]

**Scenario:** A money market fund manager must choose between two investments with different terms and rates, comparing total returns.

**Implementation:**
```
Option A: 60-day CP at 5.50%
  =RECEIVED("2024-06-01", "2024-07-31", 10000000, 0.055, 2) = $10,091,667
  Return: $91,667

Option B: 90-day T-bill at 5.35%
  =RECEIVED("2024-06-01", "2024-08-30", 10000000, 0.0535, 2) = $10,133,750
  Return: $133,750

Analysis: T-bill pays more total but ties up capital longer
Annualized: CP = 5.50%, T-bill = 5.35%
Decision depends on reinvestment assumptions
```

**Business Application:** Money market managers optimize returns within risk and liquidity constraints. Total dollar return matters for performance, but annualized rates matter for comparison. RECEIVED shows absolute returns to complement rate analysis.

**Technical Details:** Consider reinvestment risk--what rate will be available when the CP matures? If rates are expected to fall, locking in the 90-day T-bill might be better despite lower rate. Build scenario analysis using RECEIVED.

---

### [[Bank Statement Reconciliation]]

**Scenario:** An accountant needs to verify that a CD matured at the correct amount by comparing the bank's payment to the expected calculation.

**Implementation:**
```
CD Terms: $250,000 principal, 5.00% rate, 6-month term
Expected: =RECEIVED("2023-12-15", "2024-06-15", 250000, 0.05, 0) = $256,250.00

Bank Statement Shows: $256,250.00

Reconciliation: Expected matches actual ✓
```

**Business Application:** Finance teams must reconcile bank statements to internal records. Maturity amounts should match calculations. Discrepancies indicate either calculation errors, rate misunderstandings, or bank errors requiring investigation.

**Technical Details:** Verify the day count convention used by the bank. Some banks use Actual/365, others use 30/360. Small differences (a few dollars on large amounts) often result from day count variances. Large differences indicate problems.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Parameter Name:** Fourth parameter is called "discount" but represents interest rate
- **Calculation:** Investment × (1 + Rate × Days/YearDays)

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Same five parameters (4 required, 1 optional)
- Same five day count conventions
- Returns simple interest maturity amount
- Parameter "discount" is actually the interest rate

## Tips and Best Practices

1. **The "discount" parameter is really the interest rate:** Despite its name, enter the annual interest rate, not a discount rate. This naming inconsistency is a legacy issue in Excel.

2. **Use Actual/360 (basis=2) for money market:** Most US money market instruments use this convention. It produces slightly higher interest than other conventions.

3. **RECEIVED uses simple interest:** For multi-year investments, simple interest understates the true compound return. Use compound interest formulas for long-term projections.

4. **Verify with INTRATE:** Calculate INTRATE using your RECEIVED result. It should return the original rate. This validates your inputs.

5. **Match day count to instrument:** CDs might use 30/360, T-bills use Actual/360, UK instruments use Actual/365. Wrong basis = wrong maturity amount.

6. **Build cash flow models with cell references:** Link settlement, maturity, principal, and rate to data tables for dynamic forecasting across multiple investments.

7. **Account for weekends and holidays:** RECEIVED uses calendar days. If maturity falls on a non-business day, actual payment might be the next business day.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[INTRATE]] | Interest rate from amounts | When you know maturity amount and want the rate |
| [[ACCRINTM]] | Accrued interest at maturity | When you need just the interest portion, not total |
| [[PRICEMAT]] | Price of security paying at maturity | For pricing given yield (different use case) |

### Commonly Used Together

**[[INTRATE]]** - Interest Rate

*Inverse relationship:*
```
=RECEIVED(settlement, maturity, investment, rate, basis) → maturity amount
=INTRATE(settlement, maturity, investment, maturity_amount, basis) → rate
```
Use to verify calculations.

---

**[[ACCRINTM]]** - Accrued Interest at Maturity

*Interest portion:*
```
Total Received: =RECEIVED(...)
Interest Only: =ACCRINTM(...)
Principal = Total - Interest
```
Separate principal and interest for accounting.

---

**[[FV]]** - Future Value

*Compound interest alternative:*
```
Simple: =RECEIVED(...) for single period, no compounding
Compound: =FV(...) for multiple periods with compounding
```
Use FV for compound interest scenarios.

---

**[[PRICEDISC]]** - Discount Security Price

*Related pricing:*
```
=PRICEDISC gives purchase price from discount rate
=RECEIVED gives maturity amount from interest rate
Different perspectives on same instrument
```
Both useful for money market analysis.

## Official Documentation

- **Microsoft Excel:** [RECEIVED function](https://support.microsoft.com/en-us/office/received-function-7a3f8b93-6611-4f81-8576-828312c9b5e5)
- **Google Sheets:** [RECEIVED function](https://support.google.com/docs/answer/3093224)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
