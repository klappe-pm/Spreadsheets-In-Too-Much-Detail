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
- bond-pricing
- securities
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Bond Price
- Security Price
- Clean Price
tags:
- financial-modeling
- bond-analysis
- fixed-income
- securities-trading
---

# PRICE

## Description

**PRICE** calculates the price per $100 face value of a security that pays periodic interest, commonly known as a coupon bond. This is the "clean price"--the price without accrued interest--that you will see quoted in bond markets and on trading screens. When you actually buy a bond, you pay the clean price plus accrued interest (the "dirty price" or "invoice price"), but PRICE returns only the clean price component. Understanding this distinction is critical for accurate bond trading and portfolio valuation.

The bond pricing formula implemented by PRICE discounts all future cash flows (coupon payments and principal repayment at maturity) back to the settlement date using the specified yield to maturity. This is the fundamental relationship in fixed-income investing: price and yield move inversely. When yields rise, prices fall; when yields fall, prices rise. A bond trading at 100 ("par") has a yield equal to its coupon rate. A bond trading below 100 ("discount") has a yield higher than its coupon rate. A bond trading above 100 ("premium") has a yield lower than its coupon rate.

The calculation depends heavily on the **day count convention** (basis parameter), which affects both the accrued interest adjustment and the present value calculation of future cash flows. The choice of day count convention is not arbitrary--it is determined by market convention for each security type. US Treasury bonds use Actual/Actual, US corporate bonds use 30/360, and Eurobonds use European 30/360. Using the wrong convention will produce incorrect prices that do not match market quotes. The settlement date is also critical--bond prices change daily as time passes and yields fluctuate.

PRICE has been available in Excel since Excel 2007 (when Analysis ToolPak functions were integrated) and in Google Sheets since launch. The function requires settlement date, maturity date, annual coupon rate, yield to maturity, redemption value, and payment frequency, with an optional day count basis. For odd first or last coupon periods (when the bond doesn't fit neatly into standard coupon periods), consider using ODDFPRICE or ODDLPRICE instead.

## Syntax

> [!f(x)] PRICE Syntax
>
> ```
> =PRICE(settlement, maturity, rate, yld, redemption, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you acquire the bond (typically trade date + 1 or 2 business days). Enter as date value, date string, or cell reference. |
| `maturity` | Yes | The security's maturity date--when the bond matures and principal is repaid. Must be after settlement date. |
| `rate` | Yes | The security's annual coupon rate as a decimal. For a 5% coupon bond, enter 0.05 or 5%. |
| `yld` | Yes | The security's annual yield to maturity as a decimal. This is the discount rate used to value the bond. For 6% yield, enter 0.06 or 6%. |
| `redemption` | Yes | The redemption value per $100 face value at maturity. Typically 100 for standard bonds. Enter 100 for par redemption. |
| `frequency` | Yes | The number of coupon payments per year: 1 = annual, 2 = semi-annual (most common for US bonds), 4 = quarterly. |
| `basis` | No | The day count basis (convention) to use. See Day Count Conventions table below. Defaults to 0 (US 30/360) if omitted. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | Assumes 30 days per month, 360 days per year. Standard for US corporate bond pricing. | US corporate bonds, agency bonds |
| 1 | Actual/Actual | Uses actual days in each month and actual days in the coupon period. Most precise method. | US Treasury bonds, many government securities |
| 2 | Actual/360 | Uses actual number of days but divides by 360. Slightly inflates time fractions. | Some money market instruments |
| 3 | Actual/365 | Uses actual number of days but always divides by 365. | Japanese government bonds (JGBs) |
| 4 | European 30/360 | Similar to US 30/360 with different end-of-month rules. | Eurobonds, European corporate bonds |

### Return Value

Returns a numeric value representing the clean price per $100 face value. A result of 98.50 means the bond is trading at $98.50 per $100 of face value, or 98.5% of par.

## Examples

> [!f(x)] PRICE Examples

### Example 1: Par Bond (Price Equals 100)
```
=PRICE("2024-06-15", "2034-06-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** `100.00`

**Explanation:** When a bond's yield equals its coupon rate (both 5%), the bond trades at par (100). This 10-year bond pays 5% semi-annually and is priced to yield 5%. This is the baseline scenario--any difference between coupon and yield creates a premium or discount.

---

### Example 2: Premium Bond (Coupon > Yield)
```
=PRICE("2024-06-15", "2034-06-15", 0.06, 0.05, 100, 2, 0)
```
**Result:** `107.79`

**Explanation:** This bond pays a 6% coupon but yields only 5%. Because the coupon is higher than market yields, investors are willing to pay more than face value--a premium of $7.79 per $100 face. Premium bonds are common when interest rates have fallen since the bond was issued.

---

### Example 3: Discount Bond (Coupon < Yield)
```
=PRICE("2024-06-15", "2034-06-15", 0.04, 0.05, 100, 2, 0)
```
**Result:** `92.21`

**Explanation:** This bond pays only 4% but the market requires 5% yield. Investors will only pay $92.21 for $100 face value, compensating for the below-market coupon with a discount. At maturity, the investor receives $100 for what they paid $92.21 for--this capital gain, plus coupons, delivers the 5% yield.

---

### Example 4: US Treasury Bond (Actual/Actual)
```
=PRICE("2024-03-15", "2034-03-15", 0.0425, 0.045, 100, 2, 1)
```
**Result:** `97.93`

**Explanation:** A 10-year Treasury with 4.25% coupon, priced to yield 4.5%. Using Actual/Actual (basis=1) as required for Treasury bonds. The 25 basis point yield pickup over the coupon creates a discount. Treasury prices are always quoted per $100 face value in 32nds (e.g., 97-30 = 97 + 30/32 = 97.9375).

---

### Example 5: High-Yield Corporate Bond
```
=PRICE("2024-06-01", "2029-06-01", 0.085, 0.095, 100, 2, 0)
```
**Result:** `96.18`

**Explanation:** A 5-year high-yield (junk) bond with 8.5% coupon priced to yield 9.5%. The higher yield reflects credit risk--the possibility the issuer might default. High-yield bonds typically have larger yield spreads over Treasuries and more price volatility.

---

### Example 6: Annual Coupon Eurobond
```
=PRICE("2024-04-01", "2030-04-01", 0.04, 0.045, 100, 1, 4)
```
**Result:** `97.29`

**Explanation:** A 6-year Eurobond with annual coupon payments (frequency=1) and European 30/360 day count (basis=4). Eurobonds typically pay annually rather than semi-annually. The 4% coupon vs 4.5% yield creates a discount.

---

### Example 7: Quarterly Payment Bond
```
=PRICE("2024-01-15", "2027-01-15", 0.06, 0.055, 100, 4, 0)
```
**Result:** `101.40`

**Explanation:** A 3-year bond with quarterly coupon payments (frequency=4). The 6% coupon exceeds the 5.5% yield, creating a premium. Quarterly bonds are less common but exist in some markets, particularly for floating-rate notes that reset quarterly.

---

### Example 8: Near-Maturity Bond
```
=PRICE("2024-06-01", "2024-12-01", 0.05, 0.06, 100, 2, 0)
```
**Result:** `99.51`

**Explanation:** A bond maturing in just 6 months. Short-term bonds have prices very close to 100 regardless of coupon because there is little time for the coupon/yield difference to compound. Only one more coupon payment remains.

---

### Example 9: Long-Duration Bond (30 Years)
```
=PRICE("2024-06-15", "2054-06-15", 0.045, 0.05, 100, 2, 0)
```
**Result:** `91.36`

**Explanation:** A 30-year bond is highly sensitive to yield changes. The 0.5% difference between coupon (4.5%) and yield (5%) creates an $8.64 discount. For long bonds, small yield changes create large price changes--this is duration risk.

---

### Example 10: Using Cell References for Portfolio Pricing
```
=PRICE(A2, B2, C2, D2, E2, F2, G2)
```
Where A2=Settlement, B2=Maturity, C2=Coupon, D2=Yield, E2=Redemption, F2=Frequency, G2=Basis

**Result:** Varies by inputs

**Explanation:** Building a bond portfolio model with cell references allows pricing multiple bonds by copying the formula. Link yield inputs to market data feeds for real-time pricing. This is the foundation of bond portfolio management systems.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or text in numeric parameters | Ensure dates are valid Excel/Sheets date values. Use DATE() function if necessary. |
| `#NUM!` | Settlement date is on or after maturity date | Settlement must be before maturity. Check date order. |
| `#NUM!` | Invalid frequency value | Frequency must be 1 (annual), 2 (semi-annual), or 4 (quarterly). |
| `#NUM!` | Invalid basis value | Basis must be 0, 1, 2, 3, or 4. |
| `#NUM!` | Negative rate, yield, or redemption | All must be positive values (zero coupon allowed for rate). |
| `Unexpected result` | Wrong day count convention | Different basis values give different results. Match to security type. |
| `Doesn't match market` | Using wrong yield | Ensure yield matches the market quote methodology (typically yield to maturity). |

## Use Cases

### [[Bond Fair Value Calculation]]

**Scenario:** A portfolio manager needs to mark-to-market a corporate bond portfolio at month-end, using current market yields to calculate fair values.

**Implementation:**
```
For each bond:
Clean Price: =PRICE(Today, MaturityDate, CouponRate, MarketYield, 100, 2, 0)
Market Value: =Clean_Price/100 * FaceValue
Plus Accrued: =Market_Value + ACCRINT(...)
```

**Business Application:** Investment funds must report NAV daily, which requires marking all holdings to market. PRICE converts current market yields into dollar values. For bonds without active trading, yield spreads over benchmarks are used to estimate appropriate yields.

**Technical Details:** Corporate bonds use 30/360 (basis=0), settle T+2, and typically pay semi-annually. Compare calculated prices to dealer quotes as a sanity check. Large discrepancies may indicate data errors or illiquid markets.

---

### [[Yield-Price Sensitivity Analysis]]

**Scenario:** A fixed-income analyst needs to show how a bond's price changes across different yield scenarios for risk presentation.

**Implementation:**
```
Create yield scenarios: 4%, 4.5%, 5%, 5.5%, 6%
For each: =PRICE("2024-06-15", "2034-06-15", 0.05, YieldScenario, 100, 2, 0)

Results:
4.0%: 108.18
4.5%: 103.96
5.0%: 100.00
5.5%: 96.28
6.0%: 92.78
```

**Business Application:** Risk managers use yield-price tables to understand portfolio exposure to interest rate movements. A 1% yield increase drops this bond's price by roughly 7-8%. This informs hedging decisions and risk limit setting.

**Technical Details:** The price-yield relationship is convex (curved), meaning prices rise more for yield decreases than they fall for equal yield increases. This convexity benefits bond holders. Use DURATION and MDURATION for linear approximations.

---

### [[Bond Relative Value Analysis]]

**Scenario:** A trader needs to compare two similar bonds to identify which offers better value given current market yields.

**Implementation:**
```
Bond A (5-year, 4.5% coupon): =PRICE("2024-06-01", "2029-06-01", 0.045, 0.0475, 100, 2, 0)
Bond B (5-year, 5.0% coupon): =PRICE("2024-06-01", "2029-06-01", 0.05, 0.0475, 100, 2, 0)

Result: Both should trade at prices reflecting 4.75% yield, but their dollar prices differ
```

**Business Application:** Relative value analysis identifies bonds that are cheap or rich compared to similar securities. If Bond A trades below its fair value while Bond B trades at fair value, Bond A may be a better purchase. This drives trading decisions in fixed-income markets.

**Technical Details:** Beyond yield, consider credit rating, liquidity, call features, and sector. Two bonds with identical yields may have different risk profiles. PRICE provides the mathematical fair value; judgment determines if the market price represents opportunity.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (was Analysis ToolPak add-in in earlier versions)
- **Date Handling:** Accepts various date formats based on system locale. Both DATE() function and text dates work.
- **Precision:** IEEE 754 double precision, approximately 15 significant digits.

### Google Sheets
- **Availability:** All versions since launch
- **Date Handling:** More strict about date formats; use DATE(year, month, day) for reliability.
- **Identical Results:** Returns same values as Excel for identical inputs.

### Both Platforms
- Same seven parameters (6 required, 1 optional)
- Same five day count conventions
- Returns clean price per $100 face value
- Handles all standard coupon frequencies (1, 2, 4)

## Tips and Best Practices

1. **Remember PRICE returns clean price, not dirty price:** Add accrued interest (from ACCRINT) to get the actual invoice amount for a trade: Dirty Price = Clean Price + Accrued Interest.

2. **Match day count convention to security type:** Treasuries use Actual/Actual (basis=1), corporates use 30/360 (basis=0), Eurobonds use European 30/360 (basis=4). Wrong basis = wrong price.

3. **Verify the inverse relationship:** PRICE(yield) and YIELD(price) should be consistent. If you calculate a price from a yield, YIELD on that price should return your original yield.

4. **Use settlement date, not trade date:** Bonds settle T+1 (Treasuries) or T+2 (corporates) after trade date. Use the actual settlement date for accurate pricing.

5. **Redemption is almost always 100:** Unless the bond has unusual redemption terms (callable at premium, sinking fund provisions), use 100 for standard par redemption.

6. **Scale correctly for position size:** PRICE returns price per $100 face. For a $1 million position, multiply: (PRICE/100) * 1,000,000 = market value.

7. **For odd periods, use specialized functions:** ODDFPRICE handles odd first coupon periods; ODDLPRICE handles odd last coupon periods.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[YIELD]] | Calculate yield given price | When you know the price and want to find the yield |
| [[PRICEDISC]] | Price of a discounted security | For securities that don't pay coupons (T-bills, commercial paper) |
| [[PRICEMAT]] | Price of security paying interest at maturity | For securities that pay all interest at maturity |
| [[ODDFPRICE]] | Price with odd first period | When first coupon period is shorter or longer than normal |
| [[ODDLPRICE]] | Price with odd last period | When last coupon period is shorter or longer than normal |

### Commonly Used Together

**[[YIELD]]** - Bond Yield to Maturity

*Inverse relationship with PRICE:*
```
=PRICE(..., 0.05, ...) → returns price
=YIELD(..., calculated_price, ...) → should return 0.05
```
Use YIELD to verify PRICE calculations or to find yield when price is known.

---

**[[ACCRINT]]** - Accrued Interest

*Combined for dirty price calculation:*
```
Clean Price: =PRICE(...)
Accrued Interest: =ACCRINT(...)
Dirty Price: =PRICE(...) + ACCRINT(...) / 100
```
The actual settlement amount is clean price plus accrued interest.

---

**[[DURATION]]** - Macaulay Duration

*Combined for risk analysis:*
```
Price: =PRICE(...)
Duration: =DURATION(...)
Price Change ≈ -Duration * Yield Change * Price
```
Duration estimates how much price will change for a given yield change.

---

**[[MDURATION]]** - Modified Duration

*Combined for hedging:*
```
Modified Duration: =MDURATION(...)
Dollar Duration: =MDURATION(...) * PRICE(...) / 100
```
Dollar duration measures price sensitivity in dollar terms per basis point of yield change.

## Official Documentation

- **Microsoft Excel:** [PRICE function](https://support.microsoft.com/en-us/office/price-function-3ea9deac-8dfa-436f-a7c8-17ea02c21b0a)
- **Google Sheets:** [PRICE function](https://support.google.com/docs/answer/3093222)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
