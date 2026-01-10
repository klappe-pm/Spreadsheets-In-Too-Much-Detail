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
- treasury-bills
- yield-calculation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- T-Bill Bond Equivalent Yield
- Treasury Bill Equivalent
- BEY
tags:
- financial-modeling
- bond-analysis
- fixed-income
- treasury-bills
---

# TBILLEQ

## Description

**TBILLEQ** calculates the bond-equivalent yield (BEY) for a Treasury bill, converting the T-bill's discount rate into a yield that can be directly compared to semi-annual coupon bonds. This is essential because T-bills are quoted using discount rates (bank discount basis), while bonds are quoted using yields. Without this conversion, comparing T-bill returns to bond returns would be like comparing apples to oranges--the numbers would not be on the same basis.

The bond-equivalent yield accounts for two key differences between T-bill and bond conventions: (1) T-bill discount rates are based on face value while bond yields are based on purchase price, and (2) T-bill rates use a 360-day year while bond yields use actual/actual day count. TBILLEQ adjusts for both factors, producing a yield that represents what a T-bill's return would be if expressed using bond conventions. For T-bills with maturities over six months, the calculation also incorporates semi-annual compounding to match bond mathematics.

The conversion matters significantly in practice. A T-bill with a 5.00% discount rate might have a bond-equivalent yield of 5.15% or higher, depending on maturity. This spread represents real money for large investors--on a $100 million position, 15 basis points is $150,000 annually. Portfolio managers and traders use TBILLEQ constantly to ensure they are making valid comparisons when choosing between T-bills and bonds.

TBILLEQ has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is the standard function for T-bill yield conversion in money market analysis. Note that TBILLEQ takes the discount rate as input (not price), making it a direct conversion function rather than a price-to-yield calculation like YIELDDISC.

## Syntax

> [!f(x)] TBILLEQ Syntax
>
> ```
> =TBILLEQ(settlement, maturity, discount)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The T-bill's settlement date--when you acquire the security. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The T-bill's maturity date--when it matures and pays face value. Must be after settlement and within one year. |
| `discount` | Yes | The T-bill's discount rate as a decimal. For 5% discount, enter 0.05. This is the market-quoted discount rate. |

### Return Value

Returns a decimal value representing the bond-equivalent yield. A result of 0.0515 means 5.15% bond-equivalent yield.

## Examples

> [!f(x)] TBILLEQ Examples

### Example 1: Basic 90-Day T-Bill Conversion
```
=TBILLEQ("2024-06-01", "2024-08-30", 0.05)
```
**Result:** `0.0516` (5.16%)

**Explanation:** A 90-day T-bill at 5.00% discount has a bond-equivalent yield of 5.16%. The 16 basis point increase reflects the conversion from discount (face-value based) to yield (purchase-price based) methodology.

---

### Example 2: Comparing Discount vs Bond-Equivalent Yield
```
Discount Rate: 5.25%
Bond-Equivalent Yield: =TBILLEQ("2024-06-01", "2024-08-30", 0.0525) = 5.42%
Spread: 17 basis points
```
**Result:** BEY is always higher than discount rate

**Explanation:** The spread between discount and bond-equivalent yield widens at higher rate levels. This matters when comparing T-bills to bonds yielding 5.40%--the T-bill at 5.25% discount is actually competitive.

---

### Example 3: 6-Month T-Bill
```
=TBILLEQ("2024-01-15", "2024-07-15", 0.052)
```
**Result:** `0.0541` (5.41%)

**Explanation:** A 6-month T-bill at 5.20% discount yields 5.41% on bond-equivalent basis. Longer maturities show larger conversion spreads because the discount accumulates over more time.

---

### Example 4: 4-Week T-Bill
```
=TBILLEQ("2024-06-06", "2024-07-04", 0.0535)
```
**Result:** `0.0545` (5.45%)

**Explanation:** A 28-day T-bill at 5.35% discount yields 5.45%. Even short-term bills show meaningful conversion differences. 4-week T-bills are popular for liquidity management.

---

### Example 5: High Rate Environment
```
=TBILLEQ("2024-06-01", "2024-08-30", 0.08)
```
**Result:** `0.0831` (8.31%)

**Explanation:** At 8% discount, the bond-equivalent yield is 8.31%--a 31 basis point spread. The discount-to-yield conversion effect increases at higher rate levels.

---

### Example 6: Near-Maturity T-Bill
```
=TBILLEQ("2024-06-25", "2024-07-04", 0.053)
```
**Result:** `0.0539` (5.39%)

**Explanation:** A T-bill with only 9 days to maturity. Even very short maturities show conversion differences, though they are smaller in absolute terms.

---

### Example 7: 13-Week T-Bill (Standard Auction)
```
=TBILLEQ("2024-06-06", "2024-09-05", 0.0518)
```
**Result:** `0.0535` (5.35%)

**Explanation:** 13-week (91-day) T-bills are a standard Treasury auction product. At 5.18% discount, the bond-equivalent yield is 5.35%, making it comparable to 3-month bond yields.

---

### Example 8: 26-Week T-Bill (Standard Auction)
```
=TBILLEQ("2024-06-06", "2024-12-05", 0.0505)
```
**Result:** `0.0527` (5.27%)

**Explanation:** 26-week (182-day) T-bills are another standard auction product. The 5.05% discount converts to 5.27% BEY, directly comparable to 6-month bond rates.

---

### Example 9: T-Bill vs Bond Comparison
```
T-Bill: 91-day at 5.20% discount
  BEY: =TBILLEQ("2024-06-01", "2024-08-31", 0.052) = 5.37%

3-Month Corporate Bond: Yielding 5.35%

Comparison: T-bill BEY 5.37% > Bond 5.35% → T-bill is better
(Plus T-bill has no credit risk and better liquidity)
```
**Result:** T-bill is the better investment

**Explanation:** TBILLEQ enables valid comparison. Without conversion, the T-bill at 5.20% appears to lose to the bond at 5.35%. With conversion, the T-bill at 5.37% BEY wins.

---

### Example 10: Using Cell References
```
=TBILLEQ(A2, B2, C2)
```
Where A2=Settlement, B2=Maturity, C2=Discount Rate

**Result:** Varies by inputs

**Explanation:** Building a T-bill analysis model with cell references allows quick comparison across multiple securities. Link to auction results for automated yield conversion.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric discount | Use valid dates and decimal discount rate. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Maturity more than one year from settlement | T-bills must mature within one year. For longer, use bonds. |
| `#NUM!` | Discount rate <= 0 or >= 1 | Discount rate must be positive and less than 100%. |
| `Result seems wrong` | Entered yield instead of discount | TBILLEQ takes discount rate input, not yield. Use the market-quoted discount. |

## Use Cases

### [[T-Bill Auction Analysis]]

**Scenario:** An investor participating in a Treasury auction needs to convert the auction's discount rate results to bond-equivalent yield for portfolio comparison.

**Implementation:**
```
Auction Results: 13-week T-bill awarded at 5.285% discount
Settlement: Auction date + 1 (T+1)
Maturity: 91 days from settlement

BEY: =TBILLEQ(settlement, maturity, 0.05285) = 5.46%

Portfolio Comparison:
3-month CD: 5.40%
3-month Commercial Paper: 5.50%
T-bill BEY: 5.46%

Decision: T-bill competitive with CD, slightly below CP (but safer)
```

**Business Application:** Treasury auctions announce results as discount rates. TBILLEQ converts to bond-equivalent yield for comparison to other portfolio holdings. This drives allocation decisions between T-bills and other short-term investments.

**Technical Details:** Auction results are published by Treasury on auction day. Settlement is next business day (T+1). Primary dealers bid competitively; individuals can submit non-competitive bids guaranteed to receive the auction average.

---

### [[Money Market Fund Yield Calculation]]

**Scenario:** A money market fund manager needs to report the weighted average yield of T-bill holdings on a bond-equivalent basis for SEC yield reporting.

**Implementation:**
```
T-bill Portfolio:
Bill 1: $50M, discount 5.20%, maturity 30 days → BEY = 5.30%
Bill 2: $75M, discount 5.15%, maturity 60 days → BEY = 5.29%
Bill 3: $100M, discount 5.10%, maturity 90 days → BEY = 5.26%

Weighted Average BEY:
= (50×5.30 + 75×5.29 + 100×5.26) / 225 = 5.27%
```

**Business Application:** SEC regulations require money market funds to report yields in a standardized way. Converting T-bill discounts to bond-equivalent yields ensures consistency with bond and CD holdings in the same fund.

**Technical Details:** Weight by market value, not face value. Include all T-bill holdings in the calculation. Combine with bond yields (already in BEY format) and CD yields for total portfolio yield.

---

### [[Yield Curve Construction]]

**Scenario:** A fixed-income analyst needs to build a short-end yield curve using T-bill rates converted to bond-equivalent yields.

**Implementation:**
```
Maturity   Discount   Bond-Equivalent Yield
1-month    5.30%      =TBILLEQ(...) = 5.41%
3-month    5.25%      =TBILLEQ(...) = 5.41%
6-month    5.15%      =TBILLEQ(...) = 5.36%

Plot curve: Flat to slightly inverted short end
Compare to Fed Funds: 5.25-5.50% target range
Analysis: Short rates at/below Fed Funds → market expects cuts
```

**Business Application:** Yield curves show the term structure of interest rates and inform monetary policy analysis. Using bond-equivalent yields ensures the T-bill portion of the curve is consistent with the bond portion.

**Technical Details:** The T-bill curve (0-12 months) connects to the Treasury bond curve (2-30 years). Interpolation between points fills gaps. An inverted curve (short rates > long rates) historically signals recession risk.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** Converts discount to BEY accounting for day count and compounding
- **Limitation:** Maturity must be within one year of settlement

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Only three parameters (all required)
- No day count basis parameter (uses T-bill conventions automatically)
- Maximum one-year maturity
- Always returns higher yield than input discount rate

## Tips and Best Practices

1. **Input is discount rate, not yield:** TBILLEQ converts FROM discount rate TO bond-equivalent yield. Enter the market-quoted discount rate.

2. **Result is always higher than input:** Because discount is based on face value and BEY is based on purchase price, BEY > discount always.

3. **Use for T-bills up to 1 year:** TBILLEQ is designed for T-bills, which have maximum 52-week maturities. Longer securities are not T-bills.

4. **No basis parameter needed:** TBILLEQ automatically uses T-bill conventions (Actual/360 for discount, adjusted for BEY).

5. **Compare BEY to bond yields:** After conversion, T-bill BEY can be directly compared to semi-annual bond yields. This is the whole point of the function.

6. **For price from discount, use TBILLPRICE:** TBILLEQ gives yield; TBILLPRICE gives price. Use both for complete T-bill analysis.

7. **Higher rates = larger conversion spread:** At 5% discount, BEY might be 5.15%. At 8% discount, BEY might be 8.30%. The spread is not constant.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TBILLPRICE]] | T-bill price from discount | When you need price, not yield |
| [[TBILLYIELD]] | T-bill yield from price | When you have price and need yield (not discount) |
| [[YIELDDISC]] | General discount security yield | For non-T-bill discount securities |
| [[DISC]] | Discount rate from price | For the reverse conversion (price to discount) |

### Commonly Used Together

**[[TBILLPRICE]]** - T-Bill Price

*Complete T-bill analysis:*
```
Discount Rate: 5.25% (market quote)
Price: =TBILLPRICE(..., 0.0525) = 98.69
BEY: =TBILLEQ(..., 0.0525) = 5.41%
```
Both price and yield from discount rate.

---

**[[TBILLYIELD]]** - T-Bill Yield

*Alternative yield calculation:*
```
TBILLEQ: Discount Rate → Bond Equivalent Yield
TBILLYIELD: Price → Yield (similar to BEY)
Both give yield output, different inputs
```
Use TBILLYIELD when you have price, TBILLEQ when you have discount.

---

**[[DISC]]** - Discount Rate

*Reverse conversion:*
```
Price → Discount: =DISC(...)
Discount → Price: =TBILLPRICE(...)
Discount → BEY: =TBILLEQ(...)
```
Complete price-discount-yield toolkit.

---

**[[YIELDDISC]]** - Discount Security Yield

*General version:*
```
TBILLEQ: Specific to T-bills, from discount rate
YIELDDISC: General discount securities, from price
```
YIELDDISC is more flexible but requires price input.

## Official Documentation

- **Microsoft Excel:** [TBILLEQ function](https://support.microsoft.com/en-us/office/tbilleq-function-2ab72d90-9b4d-4efe-9fc2-0f81f2c19c8c)
- **Google Sheets:** [TBILLEQ function](https://support.google.com/docs/answer/3093225)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
