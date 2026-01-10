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
- Treasury Bill Yield
- T-Bill Yield
- T-Bill Discount from Price
tags:
- financial-modeling
- bond-analysis
- fixed-income
- treasury-bills
---

# TBILLYIELD

## Description

**TBILLYIELD** calculates the yield (discount rate) for a Treasury bill based on its price. This is the inverse of TBILLPRICE--while TBILLPRICE converts a discount rate to a price, TBILLYIELD converts a price back to a discount rate. When you see a T-bill trading at $98.69 with 90 days to maturity, TBILLYIELD tells you the equivalent discount rate: approximately 5.25%.

Despite its name, TBILLYIELD returns the discount rate, not the bond-equivalent yield. This matches T-bill market convention where securities are quoted as discount rates. The discount rate is based on face value: Discount Rate = (100 - Price) / 100 x (360 / Days). If you need bond-equivalent yield for comparison to coupon bonds, use TBILLEQ with the discount rate or use YIELDDISC with the price.

Understanding the distinction between discount rate and yield is critical for money market professionals. The discount rate (TBILLYIELD output) is always lower than the true yield because it is calculated on face value ($100), not investment ($98.69). For a T-bill at $98.69, the discount rate might be 5.25%, but the true yield (return on investment) is approximately 5.40%. Use TBILLEQ to get the bond-equivalent yield from the discount rate.

TBILLYIELD has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is essential for T-bill trading, portfolio analysis, and money market operations. The function automatically uses T-bill conventions (Actual/360), simplifying calculations compared to the more general DISC function.

## Syntax

> [!f(x)] TBILLYIELD Syntax
>
> ```
> =TBILLYIELD(settlement, maturity, pr)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The T-bill's settlement date--when you acquire the security. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The T-bill's maturity date--when it matures and pays $100 face value. Must be after settlement and within one year. |
| `pr` | Yes | The T-bill's price per $100 face value. For a T-bill at $98.69, enter 98.69. |

### Return Value

Returns a decimal value representing the discount rate (not bond-equivalent yield). A result of 0.0525 means 5.25% discount rate.

## Examples

> [!f(x)] TBILLYIELD Examples

### Example 1: Standard 90-Day T-Bill
```
=TBILLYIELD("2024-06-01", "2024-08-30", 98.69)
```
**Result:** `0.0524` (5.24%)

**Explanation:** A 90-day T-bill at price 98.69 has a discount rate of 5.24%. This is the market-convention quote, not the true yield (which would be higher).

---

### Example 2: Verifying with TBILLPRICE
```
Discount: =TBILLYIELD("2024-06-01", "2024-08-30", 98.69) = 5.24%
Price: =TBILLPRICE("2024-06-01", "2024-08-30", 0.0524) = 98.69 ✓
```
**Result:** TBILLYIELD and TBILLPRICE are inverses

**Explanation:** These functions are mathematical inverses. TBILLYIELD from price gives discount; TBILLPRICE from that discount returns original price.

---

### Example 3: Converting Price to Discount for Comparison
```
T-bill A: Price 98.75, 90 days
  Discount: =TBILLYIELD("2024-06-01", "2024-08-30", 98.75) = 5.00%

T-bill B: Price 98.65, 90 days
  Discount: =TBILLYIELD("2024-06-01", "2024-08-30", 98.65) = 5.40%

Comparison: B has higher discount (better return)
```
**Result:** Lower price = higher discount rate

**Explanation:** TBILLYIELD enables comparison of T-bills at different prices by converting to a common rate basis.

---

### Example 4: 4-Week T-Bill
```
=TBILLYIELD("2024-06-06", "2024-07-04", 99.58)
```
**Result:** `0.0540` (5.40%)

**Explanation:** A 28-day T-bill at 99.58 has 5.40% discount rate. Short-term bills have prices near 100, but discount rates can still be significant.

---

### Example 5: 26-Week T-Bill
```
=TBILLYIELD("2024-06-06", "2024-12-05", 97.45)
```
**Result:** `0.0504` (5.04%)

**Explanation:** A 182-day T-bill at 97.45. The $2.55 discount over 182 days annualizes to 5.04% under the discount rate convention.

---

### Example 6: 52-Week T-Bill
```
=TBILLYIELD("2024-06-06", "2025-06-05", 95.00)
```
**Result:** `0.0495` (4.95%)

**Explanation:** A 1-year T-bill at 95.00. The $5.00 discount over 364 days equals approximately 4.95% discount rate.

---

### Example 7: Converting to Bond-Equivalent Yield
```
Step 1: Get discount from price
  =TBILLYIELD("2024-06-01", "2024-08-30", 98.69) = 5.24%

Step 2: Convert discount to bond-equivalent yield
  =TBILLEQ("2024-06-01", "2024-08-30", 0.0524) = 5.40%

Result: The T-bill at $98.69 yields 5.40% on bond-equivalent basis
```
**Result:** Full price-to-BEY conversion

**Explanation:** This two-step process converts T-bill prices to bond-equivalent yields for comparison to coupon bonds or CDs.

---

### Example 8: Comparing to DISC Function
```
TBILLYIELD: =TBILLYIELD("2024-06-01", "2024-08-30", 98.69) = 5.24%
DISC:       =DISC("2024-06-01", "2024-08-30", 98.69, 100, 2) = 5.24%
```
**Result:** Identical results

**Explanation:** TBILLYIELD is equivalent to DISC with redemption=100 and basis=2 (Actual/360). TBILLYIELD is simpler for T-bill work.

---

### Example 9: Near-Par Pricing
```
=TBILLYIELD("2024-06-28", "2024-07-04", 99.91)
```
**Result:** `0.0540` (5.40%)

**Explanation:** A 6-day T-bill at 99.91. Even the tiny $0.09 discount annualizes to 5.40% because of the short term.

---

### Example 10: Using Cell References
```
=TBILLYIELD(A2, B2, C2)
```
Where A2=Settlement, B2=Maturity, C2=Price

**Result:** Varies by inputs

**Explanation:** Building a T-bill analysis model with cell references. Link to real-time prices for live discount rate calculation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric price | Use valid dates and numeric price. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Maturity more than one year from settlement | T-bills mature within one year. |
| `#NUM!` | Price <= 0 or > 100 | Price must be positive and typically below 100 (for discount). |
| `Wrong result` | Expecting yield but getting discount | TBILLYIELD returns discount rate, not yield. Use TBILLEQ for BEY. |

## Use Cases

### [[Secondary Market Price Analysis]]

**Scenario:** A trader sees a T-bill quoted at a price and needs to determine the equivalent discount rate for comparison to auction results.

**Implementation:**
```
Market Quote: 91-day T-bill at 98.67 (price)
Convert to Discount: =TBILLYIELD("2024-06-03", "2024-09-02", 98.67) = 5.28%

Recent Auction: 5.30% discount

Analysis: Market at 5.28% vs Auction at 5.30%
The T-bill trades 2bp richer (lower rate) than auction
Possible trades: Sell existing holdings or wait for better prices
```

**Business Application:** Secondary market prices must be compared to primary auction results. TBILLYIELD converts prices to discount rates, enabling direct comparison.

**Technical Details:** Secondary market T-bills trade between dealers on a price basis. Converting to discount rate allows comparison to auction results (quoted as discount). Bid/ask spreads are typically 1-2 basis points.

---

### [[Portfolio Rate Calculation]]

**Scenario:** A money market fund holds T-bills purchased at various prices and needs to calculate the weighted average discount rate for the portfolio.

**Implementation:**
```
Holdings:
Bill 1: $20M, price 98.65, 60 days → =TBILLYIELD(...) = 5.28%
Bill 2: $30M, price 98.72, 90 days → =TBILLYIELD(...) = 5.10%
Bill 3: $50M, price 97.48, 180 days → =TBILLYIELD(...) = 5.04%

Weighted Average = (20×5.28 + 30×5.10 + 50×5.04) / 100 = 5.11%
```

**Business Application:** Portfolio managers calculate average discount rates to understand overall portfolio positioning. TBILLYIELD converts purchase prices to rates for averaging.

**Technical Details:** Weight by market value, not face value. For yield reporting (SEC requirements), convert discount rates to bond-equivalent yields using TBILLEQ. Average BEY, not average discount.

---

### [[Trading Analysis: Rich/Cheap Assessment]]

**Scenario:** A fixed-income analyst needs to identify T-bills that are rich or cheap relative to the yield curve.

**Implementation:**
```
T-bill: 75 days to maturity, price 98.90
Discount: =TBILLYIELD("2024-06-03", "2024-08-17", 98.90) = 5.28%
BEY: =TBILLEQ("2024-06-03", "2024-08-17", 0.0528) = 5.44%

Curve Interpolation:
60-day point: 5.40%
90-day point: 5.50%
75-day interpolated: 5.46%

Rich/Cheap: Actual 5.44% vs Curve 5.46% → 2bp rich

Trade Idea: Sell this T-bill, buy 90-day (on curve)
```

**Business Application:** Relative value analysis identifies mispricings. TBILLYIELD converts prices to rates for curve comparison. Rich securities should be sold; cheap securities should be bought.

**Technical Details:** Yield curve comparison requires consistent rate basis. Convert TBILLYIELD output to BEY for curve comparison. Small mispricings (a few basis points) can be profitable at large sizes.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** (100 - Price) / 100 × (360 / Days)
- **Output:** Discount rate (not bond-equivalent yield)

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Only three parameters (all required)
- No day count basis parameter (uses T-bill Actual/360 automatically)
- Maximum one-year maturity
- Returns discount rate, NOT yield

## Tips and Best Practices

1. **TBILLYIELD returns discount rate, not yield:** Despite its name, the output is the bank discount rate (market convention), not bond-equivalent yield. For yield, use TBILLEQ on the discount result.

2. **Price should be below 100:** For discount securities, price < 100. If price >= 100, there's no discount (or a premium, which is unusual for T-bills).

3. **Equivalent to DISC with basis=2:** TBILLYIELD(s,m,p) = DISC(s,m,p,100,2). Use TBILLYIELD for simplicity with T-bills.

4. **Verify with TBILLPRICE:** TBILLPRICE(TBILLYIELD(...)) should return original price. Use this cross-check.

5. **For bond comparison, add TBILLEQ step:** Price → TBILLYIELD → Discount → TBILLEQ → Bond-Equivalent Yield.

6. **Higher price = lower discount:** As prices rise (investors pay more), discount rate falls. Inverse relationship.

7. **Maximum one-year maturity:** T-bills by definition are short-term. Securities over one year are notes or bonds.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TBILLPRICE]] | Price from discount | When you have discount and need price |
| [[TBILLEQ]] | Bond-equivalent yield | When you need yield for bond comparison |
| [[DISC]] | General discount rate | For non-T-bill securities or when basis needed |
| [[YIELDDISC]] | True yield from price | When you need actual yield (not discount rate) |

### Commonly Used Together

**[[TBILLPRICE]]** - T-Bill Price

*Inverse relationship:*
```
=TBILLYIELD(..., price) → discount
=TBILLPRICE(..., discount) → price
They are inverses
```
Use to verify calculations.

---

**[[TBILLEQ]]** - Bond Equivalent Yield

*Full conversion workflow:*
```
Price → Discount: =TBILLYIELD(..., 98.69) = 5.24%
Discount → BEY: =TBILLEQ(..., 0.0524) = 5.40%
```
Complete price-to-yield conversion for bond comparison.

---

**[[DISC]]** - General Discount Rate

*More flexible version:*
```
=TBILLYIELD(s, m, p) = =DISC(s, m, p, 100, 2)
DISC allows different redemption and basis
TBILLYIELD is simpler for T-bills
```
Use DISC for non-T-bill discount securities.

---

**[[YIELDDISC]]** - True Yield

*Actual yield (not discount):*
```
Discount Rate: =TBILLYIELD(...) = 5.24%
True Yield: =YIELDDISC(..., price, 100, 2) = 5.40%
```
YIELDDISC gives actual return on investment.

## Official Documentation

- **Microsoft Excel:** [TBILLYIELD function](https://support.microsoft.com/en-us/office/tbillyield-function-6c5a89f8-4fb3-468b-b9e9-f59c09ad2ba4)
- **Google Sheets:** [TBILLYIELD function](https://support.google.com/docs/answer/3093227)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
