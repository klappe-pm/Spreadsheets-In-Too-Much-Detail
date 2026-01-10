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
- Odd Last Yield
- Odd Last Period Yield
- Short Last Coupon Yield
- Long Last Coupon Yield
tags:
- financial-modeling
- bond-analysis
- fixed-income
- odd-coupon
- yield-to-maturity
---

# ODDLYIELD

## Description

**ODDLYIELD** calculates the yield to maturity of a security with an odd (irregular) last coupon period. This is the inverse function of ODDLPRICE: while ODDLPRICE calculates price given yield, ODDLYIELD calculates yield given price. When you know the market price of a bond approaching maturity with an irregular final period, ODDLYIELD tells you what annualized return that price implies if held to maturity.

The standard YIELD function assumes all coupon periods are regular length. For bonds with irregular final periods--where the time from the last coupon to maturity is shorter or longer than a standard period--YIELD produces incorrect results. The error becomes more significant as the irregular period becomes more pronounced. A bond with a 2-month final period instead of a 6-month period will have a meaningfully different yield calculation when done correctly versus assuming a standard period.

ODDLYIELD is particularly relevant for bonds near maturity where the remaining time does not fit neatly into the bond's coupon schedule. This includes bonds originally issued with non-standard maturity dates, callable bonds exercised at irregular dates, or bonds after restructuring. With only one cash flow remaining (final coupon plus principal), the calculation is simpler than for bonds with many periods, but correctness still requires accounting for the actual time remaining.

ODDLYIELD has been available in Excel since Excel 2007 and in Google Sheets since launch. It requires eight parameters: settlement, maturity, last coupon date, coupon rate, price, redemption value, frequency, and optional day count basis. Like other yield functions, ODDLYIELD uses iterative numerical methods internally to solve for the yield that makes the present value of future cash flows equal the given price.

## Syntax

> [!f(x)] ODDLYIELD Syntax
>
> ```
> =ODDLYIELD(settlement, maturity, last_interest, rate, pr, redemption, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when you acquire the bond. Must be after last interest date. |
| `maturity` | Yes | The bond's maturity date--when principal is repaid. End of the odd last period. |
| `last_interest` | Yes | The date of the last (most recent) coupon payment before settlement. Start of the odd last period. |
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

> [!f(x)] ODDLYIELD Examples

### Example 1: Short Last Period at Par
```
=ODDLYIELD("2024-10-01", "2024-12-15", "2024-06-15", 0.05, 100, 100, 2, 0)
```
**Result:** `0.05` (5.0%)

**Explanation:** Bond trading at par (price=100) with 5% coupon yields 5%. Settlement in the final period, maturity December 15 (normal 6 months from last coupon). At par, yield equals coupon rate regardless of period length.

---

### Example 2: Short Last Period Below Par
```
=ODDLYIELD("2024-10-01", "2024-11-15", "2024-06-15", 0.05, 99.5, 100, 2, 0)
```
**Result:** `0.0560` (5.60%)

**Explanation:** Last coupon June 15; maturity November 15 (5 months, not 6). Trading at 99.5, the yield exceeds the coupon rate. The short remaining time amplifies the discount's effect on yield.

---

### Example 3: Short Last Period Above Par
```
=ODDLYIELD("2024-10-01", "2024-11-15", "2024-06-15", 0.06, 100.5, 100, 2, 0)
```
**Result:** `0.0446` (4.46%)

**Explanation:** Trading at 100.5 premium with 6% coupon. The premium reduces yield below the coupon rate. With only about 6 weeks to maturity, the yield is very sensitive to small price changes.

---

### Example 4: Very Short Last Period
```
=ODDLYIELD("2024-12-10", "2024-12-15", "2024-06-15", 0.05, 100.1, 100, 2, 0)
```
**Result:** `0.0279` (2.79%)

**Explanation:** Only 5 days to maturity. The price of 100.1 implies a low yield because you're paying 0.1 above par to receive only the final prorated coupon plus 100. With such short time remaining, even small premiums significantly compress yield.

---

### Example 5: Long Last Period
```
=ODDLYIELD("2024-02-01", "2024-09-30", "2024-01-15", 0.05, 99, 100, 2, 0)
```
**Result:** `0.0566` (5.66%)

**Explanation:** Last coupon January 15; maturity September 30 (8.5 months, longer than normal 6). Trading at 99 produces a yield above the coupon rate. The longer last period means more final coupon accrual.

---

### Example 6: Treasury Bond (Actual/Actual)
```
=ODDLYIELD("2024-10-15", "2024-12-31", "2024-06-30", 0.04, 99.75, 100, 2, 1)
```
**Result:** `0.0451` (4.51%)

**Explanation:** Using Actual/Actual (basis=1) for Treasury bonds. Last coupon June 30; maturity December 31 (normal 6 months). Trading at 99.75 (small discount), yield is slightly above the 4% coupon.

---

### Example 7: Verifying ODDLPRICE/ODDLYIELD Inverse
```
Price: =ODDLPRICE("2024-10-01", "2024-11-15", "2024-06-15", 0.05, 0.055, 100, 2, 0) → ~99.58
Yield: =ODDLYIELD("2024-10-01", "2024-11-15", "2024-06-15", 0.05, 99.58, 100, 2, 0) → ~0.055
```
**Result:** Round-trip verification

**Explanation:** ODDLPRICE at 5.5% yield gives a price; ODDLYIELD at that price returns 5.5%. This inverse relationship confirms correct usage.

---

### Example 8: Called Bond Yield
```
=ODDLYIELD("2024-08-01", "2024-10-15", "2024-06-15", 0.055, 102.5, 102, 2, 0)
```
**Result:** `0.0375` (3.75%)

**Explanation:** Bond called at 102 (call premium) on October 15. Trading at 102.5, yielding 3.75%. The yield-to-call is lower because you're paying premium and receiving the call price soon.

---

### Example 9: Annual Bond with Odd Last
```
=ODDLYIELD("2024-09-01", "2025-03-15", "2024-06-15", 0.05, 99.5, 100, 1, 0)
```
**Result:** `0.0565` (5.65%)

**Explanation:** Annual coupon bond (frequency=1) with a 9-month final period (less than the normal 12). Trading at 99.5, yield exceeds the 5% coupon.

---

### Example 10: Quarterly Bond with Odd Last
```
=ODDLYIELD("2024-11-01", "2024-12-10", "2024-10-15", 0.05, 100.2, 100, 4, 0)
```
**Result:** `0.0342` (3.42%)

**Explanation:** Quarterly bond with last coupon October 15; maturity December 10 (about 2 months, less than the normal 3). Trading at a slight premium compresses yield below the coupon rate.

---

### Example 11: High-Yield Bond Near Maturity
```
=ODDLYIELD("2024-10-01", "2024-11-30", "2024-08-15", 0.09, 99, 100, 2, 0)
```
**Result:** `0.1125` (11.25%)

**Explanation:** High-yield bond with 9% coupon, trading at 99 with short time to maturity. The discount combined with the high coupon produces an 11.25% yield.

---

### Example 12: Using Cell References for Portfolio Analysis
```
=ODDLYIELD(A2, B2, C2, D2, E2, F2, G2, H2)
```
Where cells contain: Settlement, Maturity, Last Interest, Rate, Price, Redemption, Frequency, Basis

**Result:** Yield based on cell values

**Explanation:** Portfolio analytics require calculating yields for many bonds. Cell references enable copying the formula across a bond list.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format | Use DATE() function or verify date formats |
| `#NUM!` | Dates in wrong order | Must be: last_interest < settlement < maturity |
| `#NUM!` | Invalid frequency | Must be 1, 2, or 4 |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4 |
| `#NUM!` | Price too extreme | Very high or low prices may not converge |
| `#NUM!` | Negative rate or price | All must be positive |
| `Wrong yield` | Using YIELD for odd last period | Must use ODDLYIELD when last period is irregular |

## Use Cases

### [[Near-Maturity Yield Analysis]]

**Scenario:** A portfolio manager needs to calculate the yield for a bond in its final irregular period to compare against reinvestment alternatives.

**Implementation:**
```
Bond Details:
Last Coupon: June 30, 2024
Maturity: October 15, 2024 (3.5 months, not 6)
Settlement: August 15, 2024
Market Price: 100.25
Coupon: 4.5%

Yield: =ODDLYIELD("2024-08-15", "2024-10-15", "2024-06-30", 0.045, 100.25, 100, 2, 0)
```

**Business Application:** As bonds approach maturity, the yield calculation must reflect the actual remaining time. ODDLYIELD provides accurate yields for comparison against alternative investments or reinvestment decisions.

**Technical Details:** With short remaining time, yields are very sensitive to price. Small price changes create large yield swings. Consider bid/ask spreads.

---

### [[Yield-to-Call Calculation]]

**Scenario:** An analyst needs to calculate the yield-to-call for a bond that has been called at a non-standard date.

**Implementation:**
```
Call Notice Received:
Call Date: November 1, 2024 (not a regular coupon date)
Call Price: 101.50
Last Coupon: September 15, 2024
Settlement: October 1, 2024
Current Price: 101.75

Yield-to-Call: =ODDLYIELD("2024-10-01", "2024-11-01", "2024-09-15", Rate, 101.75, 101.50, 2, 0)
```

**Business Application:** Callable bonds exercised at non-standard dates create odd final periods. ODDLYIELD calculates the accurate yield-to-call using the call date as maturity and call price as redemption.

**Technical Details:** Use the call price as redemption value (101.50 in example). The "maturity" parameter is actually the call date when calculating yield-to-call.

---

### [[Maturing Portfolio Analysis]]

**Scenario:** A fixed-income analyst needs to calculate yields for a portfolio of bonds approaching maturity, several of which have irregular final periods.

**Implementation:**
```
For each bond approaching maturity:
If (Maturity != Next_Regular_Coupon_Date):
  Yield: =ODDLYIELD(Today, Maturity, Last_Coupon, Rate, Price, 100, Freq, Basis)
Else:
  Yield: =YIELD(Today, Maturity, Rate, Price, 100, Freq, Basis)

Portfolio Yield: Weighted average of individual yields
```

**Business Application:** Portfolios often contain bonds with various maturity characteristics. Accurate yield calculation for each bond enables proper portfolio yield aggregation and comparison.

**Technical Details:** Identify odd last period bonds by checking if maturity date matches the regular coupon schedule. Automate this check in portfolio systems.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Calculation:** Iterative numerical method (Newton-Raphson or similar)
- **Precision:** Converges to high precision

### Google Sheets
- **Availability:** All versions since launch
- **Identical Results:** Returns same yields as Excel
- **Calculation:** Same iterative approach

### Both Platforms
- Same eight parameters (7 required, 1 optional)
- Same five day count conventions
- Returns annualized yield as decimal
- May fail to converge for extreme prices

## Tips and Best Practices

1. **Verify inverse relationship:** Calculate ODDLPRICE with your result to confirm you get back the original price. This validates correct usage.

2. **Use clean price:** ODDLYIELD expects clean price (excluding accrued interest). Verify market quotes are clean, not dirty.

3. **Date order matters:** Ensure last_interest < settlement < maturity. The function returns #NUM! if this order is violated.

4. **Short time = high sensitivity:** Near maturity, yields are extremely sensitive to price. A $0.10 price change can move yield significantly.

5. **Compare to YIELD:** Calculate both YIELD and ODDLYIELD to understand the impact of the irregular period. Large differences indicate the period is significantly non-standard.

6. **For called bonds:** Use call date as maturity, call price as redemption. The yield is yield-to-call, not yield-to-maturity.

7. **Convergence issues:** Very extreme prices (far above or below par) may cause convergence failures. The function returns #NUM! if it cannot solve.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[YIELD]] | Yield with regular periods | When last period is standard length |
| [[ODDFYIELD]] | Yield with odd first period | When the first period is irregular, not last |
| [[ODDLPRICE]] | Price with odd last period | When you know yield and want price |
| [[YIELDDISC]] | Yield on discounted security | For zero-coupon or discount instruments |

### Commonly Used Together

**[[ODDLPRICE]]** - Odd Last Period Price

*Inverse relationship:*
```
Yield → Price: =ODDLPRICE(settle, mat, last, rate, YIELD, redemption, freq, basis)
Price → Yield: =ODDLYIELD(settle, mat, last, rate, PRICE, redemption, freq, basis)
```
These are inverse functions; use together for bid/offer analysis.

---

**[[YIELD]]** - Standard Bond Yield

*Comparison:*
```
YIELD (regular): =YIELD(settle, mat, rate, price, redemption, freq, basis)
ODDLYIELD (odd): =ODDLYIELD(settle, mat, last, rate, price, redemption, freq, basis)
Difference: Shows impact of irregular period
```
Compare to verify when ODDLYIELD is needed.

---

**[[COUPPCD]]** - Previous Coupon Date

*Identify last interest date:*
```
Last Interest: =COUPPCD(settlement, maturity, frequency, basis)
Then use in: =ODDLYIELD(settle, mat, Last_Interest, rate, price, redemption, freq, basis)
```
COUPPCD can identify the last_interest parameter for ODDLYIELD.

---

**[[ACCRINT]]** - Accrued Interest

*Combined for clean/dirty price analysis:*
```
Yield: =ODDLYIELD(..., Clean_Price, ...)
Accrued: =ACCRINT(last_interest, next_coupon, settlement, rate, par, frequency, basis)
Dirty Price: =Clean_Price + Accrued
```
Understand the clean price input by calculating accrued interest.

## Official Documentation

- **Microsoft Excel:** [ODDLYIELD function](https://support.microsoft.com/en-us/office/oddlyield-function-c873d088-cf40-435f-8d41-c8232fee9238)
- **Google Sheets:** [ODDLYIELD function](https://support.google.com/docs/answer/3093189)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
