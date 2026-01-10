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
- securities
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Bond Yield
- Yield to Maturity
- YTM
tags:
- financial-modeling
- bond-analysis
- fixed-income
- securities-trading
---

# YIELD

## Description

**YIELD** calculates the yield to maturity (YTM) of a security that pays periodic interest, given its current market price. This is the inverse of the PRICE function--where PRICE tells you what a bond is worth given a yield, YIELD tells you what return you will earn given the price you pay. Yield to maturity represents the total annualized return an investor will receive if they buy the bond at the current price and hold it until maturity, assuming all coupon payments are reinvested at the same yield.

Yield to maturity is the most important metric for comparing bond investments because it captures all sources of return: coupon income, plus any capital gain (if bought at a discount) or minus any capital loss (if bought at a premium). A bond trading at $95 with a 5% coupon will have a YTM higher than 5% because the investor also gains $5 at maturity when the bond redeems at $100. Conversely, a bond trading at $105 will have a YTM lower than its coupon rate. YTM is the single number that summarizes the total return profile of a bond investment.

The YIELD function uses an iterative calculation (Newton-Raphson method) to find the discount rate that makes the present value of all future cash flows equal to the current price. This is mathematically complex because there is no closed-form solution--the function must try different yield values until it finds one that works. The calculation depends on the **day count convention** (basis parameter), which affects how time fractions are calculated between coupon dates. Different bond markets use different conventions: US Treasuries use Actual/Actual, corporate bonds use 30/360, and Eurobonds use European 30/360.

YIELD has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. The function requires settlement date, maturity date, coupon rate, price, redemption value, and payment frequency. It is essential for bond trading, portfolio management, and fixed-income analysis. For securities with non-standard first or last coupon periods, use ODDFYIELD or ODDLYIELD instead.

## Syntax

> [!f(x)] YIELD Syntax
>
> ```
> =YIELD(settlement, maturity, rate, pr, redemption, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you acquire the bond. Enter as date value, date string, or cell reference. |
| `maturity` | Yes | The security's maturity date--when the bond matures and principal is repaid. Must be after settlement. |
| `rate` | Yes | The security's annual coupon rate as a decimal. For a 5% coupon, enter 0.05 or 5%. |
| `pr` | Yes | The security's clean price per $100 face value. For a bond trading at 98.50, enter 98.5. |
| `redemption` | Yes | The redemption value per $100 face value at maturity. Usually 100 for standard bonds. |
| `frequency` | Yes | The number of coupon payments per year: 1 = annual, 2 = semi-annual (most common), 4 = quarterly. |
| `basis` | No | The day count basis (convention) to use. See Day Count Conventions table below. Defaults to 0 (US 30/360) if omitted. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | Assumes 30 days per month, 360 days per year. | US corporate bonds, agency bonds |
| 1 | Actual/Actual | Uses actual days in each period. Most precise for government bonds. | US Treasury bonds |
| 2 | Actual/360 | Uses actual days but divides by 360. | Money market instruments |
| 3 | Actual/365 | Uses actual days but divides by 365. | Japanese government bonds |
| 4 | European 30/360 | European variant of 30/360 with different month-end rules. | Eurobonds |

### Return Value

Returns a decimal value representing the annual yield to maturity. A result of 0.0525 means 5.25% annual yield.

## Examples

> [!f(x)] YIELD Examples

### Example 1: Par Bond (Yield Equals Coupon)
```
=YIELD("2024-06-15", "2034-06-15", 0.05, 100, 100, 2, 0)
```
**Result:** `0.05` (5.00%)

**Explanation:** When a bond trades exactly at par (100), its yield equals its coupon rate. This 5% coupon bond trading at 100 yields exactly 5%. This is the reference point--any price deviation from 100 creates a yield different from the coupon rate.

---

### Example 2: Discount Bond (Price < 100)
```
=YIELD("2024-06-15", "2034-06-15", 0.05, 92, 100, 2, 0)
```
**Result:** `0.0594` (5.94%)

**Explanation:** This 5% coupon bond trades at $92 per $100 face. The yield is higher than the coupon because the investor gains $8 at maturity (buys at 92, receives 100). The 5.94% yield includes both the 5% coupon income and the capital gain over 10 years.

---

### Example 3: Premium Bond (Price > 100)
```
=YIELD("2024-06-15", "2034-06-15", 0.05, 108, 100, 2, 0)
```
**Result:** `0.0417` (4.17%)

**Explanation:** This bond trades at $108, an $8 premium. The yield is below the 5% coupon because the investor loses $8 at maturity. The 4.17% yield reflects coupon income minus the amortized capital loss over the holding period.

---

### Example 4: US Treasury Bond (Actual/Actual)
```
=YIELD("2024-03-15", "2034-03-15", 0.0425, 97.5, 100, 2, 1)
```
**Result:** `0.0460` (4.60%)

**Explanation:** A 10-year Treasury with 4.25% coupon trading at 97.50. Using Actual/Actual (basis=1) as required for Treasury bonds. The 35 basis point pickup over the coupon comes from the discount. Treasury yields are the benchmark against which all other bond yields are compared.

---

### Example 5: High-Yield Corporate Bond
```
=YIELD("2024-06-01", "2029-06-01", 0.075, 95, 100, 2, 0)
```
**Result:** `0.0865` (8.65%)

**Explanation:** A 5-year high-yield bond with 7.5% coupon at 95. The 8.65% yield reflects both the high coupon and the discount. High-yield bonds offer higher yields to compensate for credit risk--the possibility of default before maturity.

---

### Example 6: Near-Par Bond
```
=YIELD("2024-06-15", "2034-06-15", 0.0475, 99.25, 100, 2, 0)
```
**Result:** `0.0484` (4.84%)

**Explanation:** A bond trading just slightly below par at 99.25 with a 4.75% coupon. The yield is 4.84%--only 9 basis points higher than the coupon. Small price deviations create small yield deviations. This illustrates the yield-price relationship sensitivity.

---

### Example 7: Annual Coupon Eurobond
```
=YIELD("2024-04-01", "2030-04-01", 0.035, 96, 100, 1, 4)
```
**Result:** `0.0426` (4.26%)

**Explanation:** A 6-year Eurobond with annual payments (frequency=1) and European 30/360 (basis=4). The 3.5% coupon at price 96 yields 4.26%. Eurobonds typically have annual coupons, unlike US bonds which pay semi-annually.

---

### Example 8: Short-Term Bond
```
=YIELD("2024-06-01", "2025-06-01", 0.05, 99, 100, 2, 0)
```
**Result:** `0.0603` (6.03%)

**Explanation:** A 1-year bond at 99. The $1 gain over just one year significantly boosts yield. Short-term bonds are more sensitive to price changes in yield terms because the gain/loss is realized quickly.

---

### Example 9: Zero-Coupon Bond
```
=YIELD("2024-06-15", "2029-06-15", 0, 75, 100, 2, 0)
```
**Result:** `0.0585` (5.85%)

**Explanation:** A zero-coupon bond pays no interest but trades at a deep discount (75) to face value (100). The entire return comes from capital appreciation. The 5.85% yield represents the annualized return from buying at 75 and receiving 100 in 5 years.

---

### Example 10: Verifying PRICE/YIELD Inverse Relationship
```
Step 1: =PRICE("2024-06-15", "2034-06-15", 0.05, 0.055, 100, 2, 0) → 96.14
Step 2: =YIELD("2024-06-15", "2034-06-15", 0.05, 96.14, 100, 2, 0) → 0.055
```
**Result:** Confirms inverse relationship

**Explanation:** If you price a bond at 5.5% yield (getting 96.14), then calculate the yield at price 96.14, you should get back 5.5%. This circular verification ensures your inputs are correct and the functions are working properly.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or text in numeric parameters | Ensure dates are valid Excel/Sheets dates. Use DATE() if needed. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. Check date order. |
| `#NUM!` | Price is negative or zero | Price must be positive. Check for data entry errors. |
| `#NUM!` | Invalid frequency | Frequency must be 1, 2, or 4. |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `#NUM!` | Convergence failure | Unusual inputs may prevent the iterative solver from converging. Check for extreme values. |
| `Unexpected result` | Wrong day count convention | Match basis to security type. Treasuries=1, Corporates=0, Eurobonds=4. |

## Use Cases

### [[Bond Purchase Decision Analysis]]

**Scenario:** An investor is considering purchasing a corporate bond and needs to determine if the yield meets their required return threshold of 5.5%.

**Implementation:**
```
Market Price: 97.50
=YIELD("2024-06-15", "2029-06-15", 0.05, 97.50, 100, 2, 0)
Result: 5.58%

Decision: 5.58% > 5.5% required return → Consider purchasing
```

**Business Application:** Individual and institutional investors set minimum yield thresholds based on their opportunity cost and risk tolerance. YIELD allows quick screening of bonds to identify those meeting investment criteria. This is the first step in bond selection before deeper credit analysis.

**Technical Details:** The required yield should account for credit risk. A BBB corporate at 5.58% might be attractive if comparable BBBs yield 5.4%. But if the issuer is deteriorating, that spread may be insufficient compensation. YIELD provides the math; credit analysis provides the judgment.

---

### [[Yield Spread Analysis]]

**Scenario:** A fixed-income analyst needs to calculate the yield spread between a corporate bond and a benchmark Treasury to assess relative value.

**Implementation:**
```
Corporate Bond: =YIELD("2024-06-15", "2034-06-15", 0.055, 98, 100, 2, 0) = 5.75%
Treasury Benchmark: =YIELD("2024-06-15", "2034-06-15", 0.0425, 99, 100, 2, 1) = 4.38%
Spread: 5.75% - 4.38% = 137 basis points
```

**Business Application:** Credit spreads measure the additional yield investors demand for credit risk. A 137 bp spread might be typical for investment-grade corporates, but if it widened to 200 bp while fundamentals stayed stable, the bond might be undervalued. Spread analysis drives relative value trading.

**Technical Details:** Use the correct day count for each bond (corporates=0, Treasuries=1). Compare spreads to historical ranges and peer group averages. Spread widening often precedes credit downgrades; spread tightening suggests improving credit perception.

---

### [[Portfolio Yield Calculation]]

**Scenario:** A portfolio manager needs to calculate the weighted average yield of a bond portfolio for performance reporting.

**Implementation:**
```
Bond 1 (40% weight): =YIELD(...) = 4.50%
Bond 2 (35% weight): =YIELD(...) = 5.25%
Bond 3 (25% weight): =YIELD(...) = 5.75%

Portfolio Yield: =(0.40*4.50%)+(0.35*5.25%)+(0.25*5.75%) = 5.08%
```

**Business Application:** Portfolio managers report weighted average yield to show overall portfolio positioning. Higher yield indicates more aggressive positioning (longer duration or lower credit). Clients use this metric to understand their expected return profile.

**Technical Details:** Weight by market value, not face value. A bond at premium has more market value per unit of face value. Also consider yield-to-worst for callable bonds, not yield-to-maturity, if calls are likely.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (was Analysis ToolPak add-in in earlier versions)
- **Iterative Solver:** Uses Newton-Raphson iteration to solve for yield
- **Precision:** Generally accurate to 7+ decimal places

### Google Sheets
- **Availability:** All versions since launch
- **Identical Behavior:** Uses same calculation methodology as Excel
- **Same Precision:** Results match Excel for identical inputs

### Both Platforms
- Same seven parameters (6 required, 1 optional)
- Same five day count conventions
- Returns annual yield as decimal
- Maximum 100 iterations for convergence (rarely reached)

## Tips and Best Practices

1. **Yield is quoted annually, not per period:** A 5% yield on a semi-annual bond means 2.5% per six-month period, but YIELD returns the annualized 5% directly. No adjustment needed.

2. **Use clean price, not dirty price:** YIELD expects the clean price (without accrued interest). If you have the dirty price, subtract accrued interest first.

3. **Match day count to security type:** US Treasuries use Actual/Actual (basis=1), corporates use 30/360 (basis=0). Wrong basis gives wrong yield.

4. **Verify with PRICE:** Calculate PRICE using your YIELD result. You should get back the original price. If not, check your inputs.

5. **For callable bonds, consider yield-to-call:** YIELD calculates yield-to-maturity. If the bond is likely to be called early, separately calculate yield-to-call using the call date and call price.

6. **Convert percentage prices correctly:** If a bond is quoted as "98-16" (Treasury convention), convert to decimal: 98 + 16/32 = 98.5 before using YIELD.

7. **Yield is not the same as current yield:** Current yield = Annual coupon / Price. YTM includes capital gains/losses. YIELD gives YTM, not current yield.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PRICE]] | Calculate price given yield | When you know yield and want price |
| [[YIELDDISC]] | Yield of discounted security | For T-bills and other discount instruments |
| [[YIELDMAT]] | Yield of security paying at maturity | For securities with single interest payment at maturity |
| [[ODDFYIELD]] | Yield with odd first period | When first coupon period is non-standard |
| [[ODDLYIELD]] | Yield with odd last period | When last coupon period is non-standard |

### Commonly Used Together

**[[PRICE]]** - Bond Price

*Inverse verification:*
```
If YIELD = 5.5%, then PRICE at 5.5% should return original price
=PRICE(..., YIELD(...), ...) should equal original price input
```
Use to verify calculations are correct.

---

**[[DURATION]]** - Macaulay Duration

*Combined for risk analysis:*
```
Yield: =YIELD(...)
Duration: =DURATION(...)
Higher yield = shorter duration (other things equal)
```
Duration and yield together characterize a bond's risk profile.

---

**[[MDURATION]]** - Modified Duration

*Combined for price sensitivity:*
```
%Price Change ≈ -MDURATION * Yield Change
```
Modified duration tells you how much price will change per 1% yield change.

---

**[[ACCRINT]]** - Accrued Interest

*Used when you have dirty price:*
```
Clean Price = Dirty Price - ACCRINT(...)
Then: =YIELD(..., Clean_Price, ...)
```
Extract clean price from dirty price before calculating yield.

## Official Documentation

- **Microsoft Excel:** [YIELD function](https://support.microsoft.com/en-us/office/yield-function-f5f5ca43-c4bd-434f-8bd2-ed3c9727a4fe)
- **Google Sheets:** [YIELD function](https://support.google.com/docs/answer/3093230)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
