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
- discount-securities
- money-market
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Discounted Security Price
- Discount Price
- T-Bill Price
tags:
- financial-modeling
- bond-analysis
- fixed-income
- treasury-bills
---

# PRICEDISC

## Description

**PRICEDISC** calculates the price per $100 face value of a discounted security, given its discount rate. This is the inverse of the DISC function--while DISC calculates the discount rate from a known price, PRICEDISC calculates the price from a known discount rate. This function is essential for pricing Treasury bills, commercial paper, banker's acceptances, and other money market instruments that trade on a discount basis.

Discounted securities do not pay periodic interest. Instead, they are sold below face value (at a "discount") and redeemed at par at maturity. The investor's return is the difference between the purchase price and the face value. For example, if a T-bill has a 5% discount rate for 90 days, PRICEDISC calculates how much below $100 you would pay: approximately $98.75. At maturity, you receive the full $100, and your $1.25 profit represents the 5% annualized discount.

The formula is: Price = Redemption x (1 - Discount Rate x Days/Year). The day count convention (basis parameter) determines how "days" and "year" are measured. Treasury bills use Actual/360 (basis=2), which is the money market standard. Using the wrong convention will give prices that do not match market quotes. Note that the discount rate is not the same as yield--discount rates are always lower than yields for the same security because discount is calculated on face value while yield is calculated on purchase price.

PRICEDISC has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is used by traders to convert discount rate quotes to prices, by treasury departments to calculate purchase costs, and by analysts to value money market portfolios. The function pairs with DISC, YIELDDISC, and TBILLPRICE for complete money market analysis.

## Syntax

> [!f(x)] PRICEDISC Syntax
>
> ```
> =PRICEDISC(settlement, maturity, discount, redemption, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you acquire the instrument. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The security's maturity date--when it matures and pays the redemption amount. Must be after settlement. |
| `discount` | Yes | The security's discount rate as a decimal. For 5% discount, enter 0.05. |
| `redemption` | Yes | The redemption value per $100 face value. Usually 100 for standard instruments. |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year. | Some commercial paper |
| 1 | Actual/Actual | Actual days in period and year. | Some government securities |
| 2 | Actual/360 | Actual days, divide by 360. **T-bill standard.** | US Treasury bills, money market |
| 3 | Actual/365 | Actual days, divide by 365. | UK instruments |
| 4 | European 30/360 | European month-end rules. | European commercial paper |

### Return Value

Returns a numeric value representing the price per $100 face value. A result of 98.75 means the security costs $98.75 per $100 of face value.

## Examples

> [!f(x)] PRICEDISC Examples

### Example 1: Treasury Bill Pricing
```
=PRICEDISC("2024-03-15", "2024-06-13", 0.0525, 100, 2)
```
**Result:** `$98.69`

**Explanation:** A 90-day T-bill at 5.25% discount rate using Actual/360 (basis=2). Price = 100 x (1 - 0.0525 x 90/360) = 100 x 0.986875 = 98.69. The investor pays $98.69 and receives $100 at maturity.

---

### Example 2: Commercial Paper
```
=PRICEDISC("2024-06-01", "2024-09-01", 0.05, 100, 0)
```
**Result:** `$98.75`

**Explanation:** 90-day commercial paper at 5% discount using 30/360. Price = 100 x (1 - 0.05 x 90/360) = 98.75. Commercial paper often uses 30/360 convention, unlike T-bills which use Actual/360.

---

### Example 3: Comparing Conventions
```
30/360:      =PRICEDISC("2024-03-15", "2024-06-13", 0.05, 100, 0) = $98.75
Actual/360:  =PRICEDISC("2024-03-15", "2024-06-13", 0.05, 100, 2) = $98.75
Actual/365:  =PRICEDISC("2024-03-15", "2024-06-13", 0.05, 100, 3) = $98.77
```
**Result:** Different conventions give different prices

**Explanation:** The same discount rate produces different prices under different conventions. Actual/365 gives a higher price (smaller discount) because the year is longer. For T-bills, always use basis=2.

---

### Example 4: Short-Term (30 Days)
```
=PRICEDISC("2024-06-01", "2024-07-01", 0.055, 100, 2)
```
**Result:** `$99.54`

**Explanation:** A 30-day instrument at 5.5% discount. Price = 100 x (1 - 0.055 x 30/360) = 99.54. Short-term securities trade close to par because there is little time for discount to accumulate.

---

### Example 5: 6-Month T-Bill
```
=PRICEDISC("2024-01-15", "2024-07-15", 0.0515, 100, 2)
```
**Result:** `$97.40`

**Explanation:** A 6-month (182-day) T-bill at 5.15% discount. Price = 100 x (1 - 0.0515 x 182/360) = 97.40. Longer-term T-bills show larger discounts as interest accumulates over more days.

---

### Example 6: High Discount Rate
```
=PRICEDISC("2024-06-01", "2024-09-01", 0.08, 100, 2)
```
**Result:** `$98.00`

**Explanation:** 90 days at 8% discount. Price = 100 x (1 - 0.08 x 90/360) = 98.00. Higher discount rates mean lower prices and higher returns for investors. During financial stress, money market rates can spike significantly.

---

### Example 7: Verifying with DISC
```
Price: =PRICEDISC("2024-03-15", "2024-06-13", 0.0525, 100, 2) = 98.69
Check: =DISC("2024-03-15", "2024-06-13", 98.69, 100, 2) = 5.25% ✓
```
**Result:** PRICEDISC and DISC are inverses

**Explanation:** PRICEDISC calculates price from rate; DISC calculates rate from price. Using one function's output in the other should return the original input. This verification ensures correct inputs.

---

### Example 8: Calculating Purchase Cost
```
Face Value: $1,000,000
Discount Rate: 5.30%
Days to Maturity: 91

Per-100 Price: =PRICEDISC("2024-06-01", "2024-08-31", 0.053, 100, 2) = 98.66
Purchase Cost: = $1,000,000 × (98.66/100) = $986,600
Discount (Profit at maturity): = $1,000,000 - $986,600 = $13,400
```
**Result:** $986,600 purchase cost

**Explanation:** To buy $1 million face value, multiply the per-100 price by 10,000. The $13,400 discount represents the investor's return for holding the T-bill to maturity.

---

### Example 9: UK Style (Actual/365)
```
=PRICEDISC("2024-03-15", "2024-06-13", 0.0525, 100, 3)
```
**Result:** `$98.71`

**Explanation:** Using Actual/365 produces a slightly higher price (98.71 vs 98.69) than Actual/360 because you divide by more days per year. UK sterling instruments use this convention.

---

### Example 10: Using Cell References
```
=PRICEDISC(A2, B2, C2, D2, E2)
```
Where A2=Settlement, B2=Maturity, C2=Discount Rate, D2=Redemption, E2=Basis

**Result:** Varies by inputs

**Explanation:** Building a T-bill trading model with cell references allows instant price calculations when rates change. Link to market data feeds for real-time pricing.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric parameters | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Discount rate >= 1 or negative | Discount rate must be between 0 and 1 (0% to 100%). |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Price seems wrong` | Wrong day count convention | T-bills use basis=2 (Actual/360). Using basis=0 gives wrong price. |
| `Price > Redemption` | Negative discount rate | For discount securities, price should be below redemption. |

## Use Cases

### [[Treasury Bill Purchase Calculation]]

**Scenario:** A money market fund manager needs to calculate the exact purchase cost for a T-bill auction with $10 million face value at the awarded discount rate.

**Implementation:**
```
Auction Results:
Face Value: $10,000,000
Discount Rate: 5.285%
Settlement: Auction date + 1
Maturity: 91 days from settlement

Per-100 Price: =PRICEDISC(settlement, maturity, 0.05285, 100, 2) = 98.67
Purchase Cost: = $10,000,000 × 0.9867 = $9,867,000
Wire Amount: $9,867,000 (to Federal Reserve)
```

**Business Application:** T-bill auctions announce winning discount rates, not prices. Buyers must calculate exact purchase amounts for settlement. PRICEDISC converts auction results to wire amounts. Errors cause settlement failures.

**Technical Details:** T-bill auctions settle T+1 (next business day). Use the settlement date, not auction date, in PRICEDISC. Primary dealers bid in discount rate terms; individual investors can enter non-competitive bids and receive the average auction rate.

---

### [[Money Market Portfolio Valuation]]

**Scenario:** An asset manager needs to mark-to-market a portfolio of discount securities using current market rates for daily NAV calculation.

**Implementation:**
```
For each holding:
Market Discount Rate: From Bloomberg/Reuters
Current Price: =PRICEDISC(TODAY()+1, maturity, market_rate, 100, basis)
Market Value: = Face Value × (Price/100)

Portfolio:
T-bill 1: $5M face, 5.20% rate → $4,934,000
T-bill 2: $3M face, 5.15% rate → $2,961,000
CP 1: $2M face, 5.35% rate → $1,973,000
Total Market Value: $9,868,000
```

**Business Application:** Mutual funds and ETFs must calculate NAV daily. PRICEDISC converts market discount rates to prices, which are then multiplied by holdings to get market values. Accurate pricing ensures fair treatment of investors buying and selling fund shares.

**Technical Details:** Use T+1 settlement date (tomorrow) for most accurate pricing. Match basis to each security type. Large discrepancies between calculated and quoted prices may indicate stale data or unusual market conditions.

---

### [[Arbitrage Pricing Analysis]]

**Scenario:** A trader spots a potential mispricing between two T-bills with similar maturities and wants to calculate exact prices to determine profitability.

**Implementation:**
```
T-bill A: 89-day, quoted at 5.25% discount
T-bill B: 91-day, quoted at 5.20% discount

Prices:
A: =PRICEDISC("2024-06-03", "2024-08-31", 0.0525, 100, 2) = 98.70
B: =PRICEDISC("2024-06-03", "2024-09-02", 0.052, 100, 2) = 98.69

Analysis: B is slightly cheaper but matures 2 days later
Annualized return: Need to calculate yield, not just discount
```

**Business Application:** Traders look for relative value opportunities within the T-bill curve. PRICEDISC provides precise prices for comparison. Small mispricings can be profitable at large sizes, especially when financing costs are low.

**Technical Details:** Convert discount rates to yields for proper comparison across maturities. Consider bid/ask spreads and transaction costs. Roll-down analysis shows how prices change as time passes.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** Redemption × (1 - Discount × Days/Year)
- **Precision:** Full floating-point precision

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Same five parameters (4 required, 1 optional)
- Same five day count conventions
- Returns price per 100 face value
- Discount must be less than 1 (100%)

## Tips and Best Practices

1. **Use basis=2 (Actual/360) for Treasury bills:** This is the market standard. Using other conventions gives prices that do not match market quotes.

2. **Discount rate is NOT yield:** A 5% discount rate produces a yield higher than 5% because yield is calculated on purchase price (less than 100). Use YIELDDISC or TBILLEQ for yields.

3. **Verify with DISC:** Calculate DISC using your PRICEDISC result. You should get back the original discount rate. This cross-check validates inputs.

4. **Scale for position size:** PRICEDISC returns price per $100. For $1 million face value, multiply: (PRICEDISC/100) × 1,000,000.

5. **Price is always less than redemption:** For discount securities, price < 100. If your result is >= 100, check for input errors.

6. **Short-term = near par:** Very short-term securities (days to weeks) have prices very close to 100. Don't be surprised by prices like 99.95.

7. **Higher discount = lower price:** As rates rise, prices fall. This inverse relationship is consistent across all fixed-income securities.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DISC]] | Discount rate from price | When you know price and need rate |
| [[TBILLPRICE]] | T-bill price specifically | Functionally similar, T-bill focused |
| [[PRICE]] | Coupon bond price | For securities with periodic interest payments |

### Commonly Used Together

**[[DISC]]** - Discount Rate

*Inverse relationship:*
```
=PRICEDISC(..., discount_rate, ...) → price
=DISC(..., price, ...) → discount_rate
They are inverses
```
Use to verify calculations.

---

**[[YIELDDISC]]** - Discount Security Yield

*Converting discount to yield:*
```
Price: =PRICEDISC(..., 0.0525, ...) = 98.69
Yield: =YIELDDISC(..., 98.69, ...) = 5.40%
```
Yield > Discount rate always for discount securities.

---

**[[TBILLPRICE]]** - T-Bill Price

*T-bill specific:*
```
Both calculate T-bill prices
=TBILLPRICE(settlement, maturity, discount)
=PRICEDISC(settlement, maturity, discount, 100, 2)
Results should match for T-bills
```
TBILLPRICE assumes T-bill conventions.

---

**[[TBILLEQ]]** - T-Bill Equivalent Yield

*Yield conversion:*
```
Discount Rate → Price: =PRICEDISC(...)
Discount Rate → Yield: =TBILLEQ(...)
```
Use TBILLEQ for bond-equivalent yield comparison.

## Official Documentation

- **Microsoft Excel:** [PRICEDISC function](https://support.microsoft.com/en-us/office/pricedisc-function-d06ad7c1-380e-4be7-9fd9-75e3079acfd3)
- **Google Sheets:** [PRICEDISC function](https://support.google.com/docs/answer/3093221)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
