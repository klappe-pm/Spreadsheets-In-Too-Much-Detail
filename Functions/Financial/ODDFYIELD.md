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
- yield-calculation
- odd-periods
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Odd First Yield
- Odd First Period Yield
- Short First Coupon Yield
- Long First Coupon Yield
tags:
- financial-modeling
- bond-analysis
- fixed-income
- odd-coupon
- yield-to-maturity
---

# ODDFYIELD

## Description

**ODDFYIELD** calculates the yield to maturity of a security with an odd (irregular) first coupon period. This is the inverse function of ODDFPRICE: while ODDFPRICE calculates price given yield, ODDFYIELD calculates yield given price. When you know what price a bond with an odd first period is trading at, ODDFYIELD tells you what annualized return (yield to maturity) that price implies if you hold the bond to maturity.

Yield to maturity is the internal rate of return that equates the present value of all future cash flows to the current price. For bonds with odd first periods, this calculation is more complex because the first coupon is not standard. A short first period means a smaller first coupon; a long first period means a larger first coupon. ODDFYIELD handles both scenarios, iteratively solving for the discount rate that makes the present value of all cash flows (including the irregular first coupon) equal to the given price plus accrued interest.

The standard YIELD function assumes regular coupon periods and will produce incorrect results for odd-first bonds. The error magnitude depends on how irregular the first period is and how far settlement is from the first coupon. Using YIELD instead of ODDFYIELD on an odd-first bond can produce yield errors of 10-50+ basis points, which is significant for trading decisions and portfolio analytics. Always verify whether a bond has an odd first period before choosing which function to use.

ODDFYIELD has been available in Excel since Excel 2007 and in Google Sheets since launch. It requires nine parameters: settlement, maturity, issue date, first coupon date, coupon rate, price, redemption value, and frequency, plus an optional day count basis. The function uses iterative numerical methods internally, solving for the yield that satisfies the bond pricing equation.

## Syntax

> [!f(x)] ODDFYIELD Syntax
>
> ```
> =ODDFYIELD(settlement, maturity, issue, first_coupon, rate, pr, redemption, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when you acquire the bond. |
| `maturity` | Yes | The bond's maturity date--when principal is repaid. |
| `issue` | Yes | The bond's original issue date--start of the odd first period. |
| `first_coupon` | Yes | The date of the first coupon payment--end of the odd first period. |
| `rate` | Yes | The annual coupon rate as a decimal (e.g., 0.05 for 5%). |
| `pr` | Yes | The bond's clean price per $100 face value. |
| `redemption` | Yes | The redemption value per $100 face at maturity. Typically 100. |
| `frequency` | Yes | Coupon payments per year: 1 = annual, 2 = semi-annual, 4 = quarterly. |
| `basis` | No | Day count convention. Defaults to 0 (US 30/360). |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year | US corporate bonds |
| 1 | Actual/Actual | Actual calendar days | US Treasury bonds |
| 2 | Actual/360 | Actual days, 360-day year | Money market |
| 3 | Actual/365 | Actual days, 365-day year | Japanese bonds |
| 4 | European 30/360 | European month-end rules | Eurobonds |

### Return Value

Returns the annual yield to maturity as a decimal. A result of 0.055 means 5.5% yield. Multiply by 100 to express as a percentage.

## Examples

> [!f(x)] ODDFYIELD Examples

### Example 1: Short First Period at Par
```
=ODDFYIELD("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 100, 100, 2, 0)
```
**Result:** `0.05` (5.0%)

**Explanation:** A bond with 5% coupon trading at par (price=100) yields 5%. The short first period (March 15 to June 15 = 3 months) does not change this relationship at par--coupon rate equals yield when price equals 100.

---

### Example 2: Short First Period at Discount
```
=ODDFYIELD("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 95, 100, 2, 0)
```
**Result:** `0.0564` (5.64%)

**Explanation:** Trading at 95 (5% below par), the yield exceeds the 5% coupon. The short first period means slightly less interest initially, but the main driver is buying at a discount and receiving 100 at maturity.

---

### Example 3: Short First Period at Premium
```
=ODDFYIELD("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 105, 100, 2, 0)
```
**Result:** `0.0442` (4.42%)

**Explanation:** Trading at 105 (5% above par), the yield is below the 5% coupon. Paying a premium means you receive less than you paid at maturity, reducing your effective return.

---

### Example 4: Long First Period at Par
```
=ODDFYIELD("2024-02-01", "2034-06-15", "2023-09-15", "2024-06-15", 0.05, 100, 100, 2, 0)
```
**Result:** `0.05` (5.0%)

**Explanation:** Long first period (September to June = 9 months) with a larger first coupon. At par, yield still equals coupon rate. The additional first-period interest is offset by the pricing at par.

---

### Example 5: Long First Period at Discount
```
=ODDFYIELD("2024-02-01", "2034-06-15", "2023-09-15", "2024-06-15", 0.05, 98, 100, 2, 0)
```
**Result:** `0.0526` (5.26%)

**Explanation:** Trading at 98 with a long first period. The discount increases yield above the coupon rate. The large first coupon plus buying at a discount creates an attractive yield.

---

### Example 6: Very Short First Period
```
=ODDFYIELD("2024-06-10", "2034-06-15", "2024-06-01", "2024-06-15", 0.05, 99.5, 100, 2, 0)
```
**Result:** `0.0507` (5.07%)

**Explanation:** Extremely short first period (June 1 to June 15 = 14 days). The first coupon is tiny. Trading slightly below par produces a yield slightly above the coupon rate.

---

### Example 7: Treasury-Style Bond (Actual/Actual)
```
=ODDFYIELD("2024-04-15", "2034-05-15", "2024-02-15", "2024-05-15", 0.0425, 97.5, 100, 2, 1)
```
**Result:** `0.0462` (4.62%)

**Explanation:** Using Actual/Actual (basis=1) for Treasury securities. Short first period (February to May = 3 months). Trading at 97.5 produces a yield above the 4.25% coupon.

---

### Example 8: Verifying ODDFPRICE/ODDFYIELD Inverse Relationship
```
Price: =ODDFPRICE("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 0.055, 100, 2, 0) → ~96.21
Yield: =ODDFYIELD("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 96.21, 100, 2, 0) → ~0.055
```
**Result:** Round-trip verification

**Explanation:** ODDFPRICE at 5.5% yield gives a price; ODDFYIELD at that price returns 5.5%. This inverse relationship confirms correct usage.

---

### Example 9: Annual Coupon Odd First
```
=ODDFYIELD("2024-03-01", "2034-12-15", "2023-09-15", "2024-12-15", 0.05, 102, 100, 1, 0)
```
**Result:** `0.0475` (4.75%)

**Explanation:** Annual coupon bond (frequency=1) with a long first period (15 months). Trading at 102 premium produces a yield below the 5% coupon.

---

### Example 10: Quarterly Bond with Short First
```
=ODDFYIELD("2024-04-01", "2029-07-01", "2024-03-15", "2024-04-01", 0.05, 99.75, 100, 4, 0)
```
**Result:** `0.0510` (5.10%)

**Explanation:** Quarterly bond with a 17-day first period. Trading slightly below par yields slightly above the coupon rate. Settlement on first coupon date.

---

### Example 11: High-Yield Odd First Bond
```
=ODDFYIELD("2024-05-01", "2029-06-15", "2024-03-15", "2024-06-15", 0.08, 95, 100, 2, 0)
```
**Result:** `0.0925` (9.25%)

**Explanation:** High-yield (junk) bond with 8% coupon trading at 95. The discount combined with the high coupon produces a substantial yield of 9.25%.

---

### Example 12: Using Cell References for Portfolio Analysis
```
=ODDFYIELD(A2, B2, C2, D2, E2, F2, G2, H2, I2)
```
Where cells contain: Settlement, Maturity, Issue, First Coupon, Rate, Price, Redemption, Frequency, Basis

**Result:** Yield based on cell values

**Explanation:** Portfolio analytics require calculating yields for many bonds. Cell references allow copying the formula across a bond list.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format | Use DATE() function or verify date formats |
| `#NUM!` | Dates in wrong order | Must be: issue <= settlement <= first_coupon <= maturity |
| `#NUM!` | Invalid frequency | Must be 1, 2, or 4 |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4 |
| `#NUM!` | Price too extreme | Very high or low prices may not converge |
| `#NUM!` | Negative price or rate | All must be positive |
| `Wrong yield` | Using YIELD instead of ODDFYIELD | For odd first periods, must use ODDFYIELD |

## Use Cases

### [[New Issue Yield Analysis]]

**Scenario:** An investor evaluating a new bond issue needs to understand what yield is implied by the offering price, accounting for the odd first period.

**Implementation:**
```
New Issue Terms:
Issue Date: March 15, 2024
First Coupon: June 30, 2024 (short first)
Maturity: June 30, 2034
Coupon: 5.25%
Offering Price: 99.50

Yield: =ODDFYIELD(Settlement, "2034-06-30", "2024-03-15", "2024-06-30", 0.0525, 99.50, 100, 2, 0)
```

**Business Application:** Investors compare yields across different bond offerings to identify value. ODDFYIELD provides accurate yield calculation that accounts for the irregular first coupon, enabling fair comparison.

**Technical Details:** The offering price should be the clean price. Verify the exact first coupon date from the prospectus--errors here significantly affect the yield calculation.

---

### [[Secondary Market Yield Calculation]]

**Scenario:** A trader needs to calculate the yield for a bond trading in the secondary market that still has its odd first period ahead.

**Implementation:**
```
Market Quote:
Settlement: 2024-05-15 (during odd first period)
Market Price: 98.25 (clean)
Issue Date: 2024-03-15
First Coupon: 2024-06-30

Yield: =ODDFYIELD("2024-05-15", Maturity, "2024-03-15", "2024-06-30", Rate, 98.25, 100, 2, 0)
```

**Business Application:** Market participants quote bonds by price or yield. ODDFYIELD converts a price quote to yield for comparison against other investment opportunities or yield curve analysis.

**Technical Details:** Ensure the market price is clean (excluding accrued). After the first coupon pays, standard YIELD function can be used.

---

### [[Yield Curve Building]]

**Scenario:** A quantitative analyst building a corporate bond yield curve needs accurate yields for newly issued bonds with odd first periods.

**Implementation:**
```
For each bond in curve construction set:
If (Settlement < First_Coupon) AND (Issue != normal_schedule):
  Yield: =ODDFYIELD(Settlement, Maturity, Issue, First_Coupon, Rate, Price, 100, 2, Basis)
Else:
  Yield: =YIELD(Settlement, Maturity, Rate, Price, 100, 2, Basis)
```

**Business Application:** Yield curves require accurate yields for all bonds used in construction. Using YIELD for odd-first bonds would introduce errors that distort the curve shape.

**Technical Details:** Identify odd-first bonds by checking if issue date aligns with the coupon schedule. Automate this check in data processing.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Calculation:** Iterative Newton-Raphson or similar method internally
- **Precision:** Converges to high precision (typically 10+ decimal places)

### Google Sheets
- **Availability:** All versions since launch
- **Identical Results:** Returns same yields as Excel
- **Calculation:** Same iterative approach

### Both Platforms
- Same nine parameters (8 required, 1 optional)
- Same five day count conventions
- Returns annualized yield as decimal
- Iterative calculation may fail for extreme inputs

## Tips and Best Practices

1. **Verify inverse relationship:** Calculate ODDFPRICE with your result, and you should get back the original price. This confirms correct usage.

2. **Use clean price:** ODDFYIELD expects clean price (excluding accrued interest). Market quotes may be dirty or clean--verify before input.

3. **Date order validation:** Ensure issue <= settlement <= first_coupon <= maturity. ODDFYIELD checks this and returns #NUM! if violated.

4. **After first coupon, use YIELD:** Once the first coupon has paid, the bond has regular periods. Standard YIELD is then appropriate.

5. **Compare to YIELD for error magnitude:** Calculate both YIELD and ODDFYIELD to understand how much the odd first period affects the result.

6. **Extreme prices may not converge:** Very high premiums or deep discounts can cause convergence issues. The function returns #NUM! if it cannot solve.

7. **Basis consistency:** Use the same basis as market convention for the security type. Corporates typically use 0, Treasuries use 1.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[YIELD]] | Yield with regular periods | When first period is standard length |
| [[ODDLYIELD]] | Yield with odd last period | When the last period is irregular, not first |
| [[ODDFPRICE]] | Price with odd first period | When you know yield and want price |
| [[YIELDDISC]] | Yield on discounted security | For zero-coupon or discount instruments |

### Commonly Used Together

**[[ODDFPRICE]]** - Odd First Period Price

*Inverse relationship:*
```
Yield → Price: =ODDFPRICE(..., yield, ...)
Price → Yield: =ODDFYIELD(..., price, ...)
```
These are inverse functions; use together for bid/offer analysis.

---

**[[YIELD]]** - Standard Bond Yield

*Comparison and transition:*
```
During odd first period: =ODDFYIELD(...)
After first coupon: =YIELD(...)
Compare: Difference shows odd-period effect
```
After the first coupon pays, transition to standard YIELD.

---

**[[DURATION]]** / [[MDURATION]] - Interest Rate Sensitivity

*Combined for risk analysis:*
```
Yield: =ODDFYIELD(...)
Duration: =DURATION(settle, mat, rate, YIELD, redemption, freq, basis)
```
Note: Standard DURATION may not perfectly handle odd first periods; verify or calculate manually.

---

**[[ACCRINT]]** - Accrued Interest

*Combined for dirty price:*
```
Yield: =ODDFYIELD(..., Clean_Price, ...)
Accrued: Special calculation for first period (from issue date)
Dirty Price: =Clean_Price + Accrued
```
First-period accrued interest runs from issue date, not previous coupon.

## Official Documentation

- **Microsoft Excel:** [ODDFYIELD function](https://support.microsoft.com/en-us/office/oddfyield-function-66bc8b7b-6501-4c93-9ce3-2fd16220fe37)
- **Google Sheets:** [ODDFYIELD function](https://support.google.com/docs/answer/3093187)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
