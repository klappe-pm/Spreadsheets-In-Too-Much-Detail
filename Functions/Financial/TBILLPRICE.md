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
- pricing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Treasury Bill Price
- T-Bill Price
- T-Bill Pricing
tags:
- financial-modeling
- bond-analysis
- fixed-income
- treasury-bills
---

# TBILLPRICE

## Description

**TBILLPRICE** calculates the price per $100 face value of a Treasury bill, given its discount rate. This is the specialized T-bill version of PRICEDISC, automatically using T-bill market conventions. When you see a T-bill quoted at "5.25% discount," TBILLPRICE tells you exactly how much you pay: for a 90-day bill at 5.25%, you would pay approximately $98.69 per $100 face value.

Treasury bills are the most liquid and safest short-term investments in the world, backed by the full faith and credit of the US government. They trade on a discount basis--investors buy below face value and receive face value at maturity. The discount rate determines the price, and TBILLPRICE makes this conversion instantly. Understanding T-bill pricing is fundamental to money market operations, treasury management, and yield curve analysis.

The formula is: Price = 100 x (1 - Discount x Days/360). TBILLPRICE automatically uses the Actual/360 day count convention standard for T-bills--you don't need to specify a basis parameter. This simplicity makes it the preferred function for T-bill-specific calculations. The days are actual calendar days between settlement and maturity, divided by 360 for the discount calculation.

TBILLPRICE has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is the go-to function for T-bill traders, money market desks, and anyone working with short-term Treasury securities. For the reverse calculation (price to discount rate), use TBILLYIELD or the general DISC function with basis=2.

## Syntax

> [!f(x)] TBILLPRICE Syntax
>
> ```
> =TBILLPRICE(settlement, maturity, discount)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The T-bill's settlement date--when you acquire the security. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The T-bill's maturity date--when it matures and pays $100 face value. Must be after settlement and within one year. |
| `discount` | Yes | The T-bill's discount rate as a decimal. For 5.25% discount, enter 0.0525. |

### Return Value

Returns a numeric value representing the price per $100 face value. A result of 98.69 means you pay $98.69 to buy $100 face value.

## Examples

> [!f(x)] TBILLPRICE Examples

### Example 1: Standard 90-Day T-Bill
```
=TBILLPRICE("2024-06-01", "2024-08-30", 0.0525)
```
**Result:** `$98.69`

**Explanation:** A 90-day T-bill at 5.25% discount. Price = 100 x (1 - 0.0525 x 90/360) = 100 x 0.986875 = 98.69. The investor pays $98.69 and receives $100 at maturity, earning $1.31 over 90 days.

---

### Example 2: Calculating Purchase Cost
```
Face Value: $1,000,000
Discount: 5.30%
Days: 91

Per-100 Price: =TBILLPRICE("2024-06-01", "2024-08-31", 0.053) = 98.66
Purchase Cost: = $1,000,000 × (98.66/100) = $986,600
Profit at Maturity: = $1,000,000 - $986,600 = $13,400
```
**Result:** $986,600 investment, $13,400 return

**Explanation:** TBILLPRICE gives price per $100. Scale by face value for actual dollars. The $13,400 profit represents the 5.30% discount annualized over 91 days.

---

### Example 3: 4-Week T-Bill
```
=TBILLPRICE("2024-06-06", "2024-07-04", 0.0535)
```
**Result:** `$99.58`

**Explanation:** A 28-day (4-week) T-bill at 5.35% discount. Price = 100 x (1 - 0.0535 x 28/360) = 99.58. Short-term bills have prices very close to 100.

---

### Example 4: 13-Week (3-Month) T-Bill
```
=TBILLPRICE("2024-06-06", "2024-09-05", 0.0518)
```
**Result:** `$98.69`

**Explanation:** A 91-day T-bill at 5.18% discount. 13-week bills are a standard Treasury auction product, typically settling on Thursdays.

---

### Example 5: 26-Week (6-Month) T-Bill
```
=TBILLPRICE("2024-06-06", "2024-12-05", 0.0505)
```
**Result:** `$97.45`

**Explanation:** A 182-day T-bill at 5.05% discount. Longer bills show more discount because there are more days for interest to accumulate.

---

### Example 6: 52-Week T-Bill
```
=TBILLPRICE("2024-06-06", "2025-06-05", 0.0495)
```
**Result:** `$95.04`

**Explanation:** A 364-day (1-year) T-bill at 4.95% discount. Price = 100 x (1 - 0.0495 x 364/360) = 95.04. One-year bills have the largest discounts.

---

### Example 7: High Discount Rate
```
=TBILLPRICE("2024-06-01", "2024-08-30", 0.08)
```
**Result:** `$98.00`

**Explanation:** A 90-day T-bill at 8% discount. In high-rate environments (like 2022-2023), T-bill rates exceeded 5% for the first time in years.

---

### Example 8: Comparing to PRICEDISC
```
TBILLPRICE: =TBILLPRICE("2024-06-01", "2024-08-30", 0.0525) = 98.69
PRICEDISC:  =PRICEDISC("2024-06-01", "2024-08-30", 0.0525, 100, 2) = 98.69
```
**Result:** Identical results

**Explanation:** TBILLPRICE is equivalent to PRICEDISC with redemption=100 and basis=2 (Actual/360). TBILLPRICE is simpler for T-bill-specific work.

---

### Example 9: Verifying with TBILLYIELD
```
Price: =TBILLPRICE("2024-06-01", "2024-08-30", 0.0525) = 98.69
Check: =TBILLYIELD("2024-06-01", "2024-08-30", 98.69) = 5.25% ✓
```
**Result:** TBILLPRICE and TBILLYIELD are inverses

**Explanation:** TBILLPRICE calculates price from discount; TBILLYIELD calculates discount from price. They should be consistent.

---

### Example 10: Using Cell References
```
=TBILLPRICE(A2, B2, C2)
```
Where A2=Settlement, B2=Maturity, C2=Discount Rate

**Result:** Varies by inputs

**Explanation:** Building a T-bill pricing model with cell references enables quick calculation when rates change. Link to market data for real-time pricing.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric discount | Use valid dates and decimal discount rate. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Maturity more than one year from settlement | T-bills must mature within one year. |
| `#NUM!` | Discount rate <= 0 or >= 1 | Discount must be positive and less than 100%. |
| `Price > 100` | Calculation error or wrong input | For positive discount, price should be below 100. Check inputs. |

## Use Cases

### [[T-Bill Auction Settlement]]

**Scenario:** After winning a Treasury auction, an investor needs to calculate the exact wire amount for settlement.

**Implementation:**
```
Auction Result: 13-week T-bill at 5.285% discount
Face Value Awarded: $5,000,000
Settlement: Thursday (T+1)
Maturity: 91 days from settlement

Price per $100: =TBILLPRICE(settlement, maturity, 0.05285) = 98.66
Settlement Amount: = $5,000,000 × (98.66/100) = $4,933,000

Wire $4,933,000 to Treasury by settlement date
```

**Business Application:** Auction participants must wire exact settlement amounts. TBILLPRICE converts the discount rate result to dollars. Errors cause failed settlements and potential penalties.

**Technical Details:** T-bill auctions are held weekly (4-week and 8-week on Tuesdays, 13-week and 26-week on Mondays). Settlement is next business day. Wire via Fedwire to Treasury's account.

---

### [[Portfolio Mark-to-Market]]

**Scenario:** A money market fund needs to mark T-bill holdings to current market values for daily NAV calculation.

**Implementation:**
```
Holdings:
T-bill 1: $10M face, maturity 30 days, market discount 5.15%
  Price: =TBILLPRICE(today+1, maturity1, 0.0515) = 99.57
  Value: $10M × 0.9957 = $9,957,000

T-bill 2: $15M face, maturity 75 days, market discount 5.20%
  Price: =TBILLPRICE(today+1, maturity2, 0.052) = 98.92
  Value: $15M × 0.9892 = $14,838,000

Total T-bill Value: $24,795,000
```

**Business Application:** Investment funds must value holdings daily at market prices. TBILLPRICE converts current market discount rates to prices, which are then applied to position sizes for NAV calculation.

**Technical Details:** Use T+1 settlement date for most accurate current value. Obtain market discount rates from Bloomberg, Reuters, or Treasury Direct. Large discrepancies from prior day may indicate stale data.

---

### [[Trading P&L Calculation]]

**Scenario:** A trader needs to calculate profit on a T-bill position after rates have moved.

**Implementation:**
```
Trade Entry (yesterday):
Bought $50M face, 90-day, at 5.25% discount
Entry Price: =TBILLPRICE(..., 0.0525) = 98.69
Cost: $50M × 0.9869 = $49,345,000

Today's Mark (rates fell):
Current rate: 5.10% discount
Current Price: =TBILLPRICE(..., 0.051) = 98.73
Value: $50M × 0.9873 = $49,365,000

P&L: $49,365,000 - $49,345,000 = $20,000 profit
```

**Business Application:** Traders mark positions daily to track P&L. Falling rates increase T-bill prices, creating profits for long positions. Rising rates decrease prices, creating losses.

**Technical Details:** T-bill prices move inversely to rates. A 15bp rate decrease on $50M for 90 days creates approximately $19,000 profit. Use dollar duration for quick P&L estimates: DV01 ≈ FaceValue × Days/36000.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** 100 × (1 - Discount × Days/360)
- **Convention:** Automatically uses Actual/360

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Only three parameters (all required)
- No day count basis parameter (uses T-bill Actual/360 automatically)
- Maximum one-year maturity
- Returns price per $100 face value

## Tips and Best Practices

1. **TBILLPRICE uses T-bill conventions automatically:** No need to specify day count basis--it uses Actual/360, the T-bill standard.

2. **Result is price per $100:** Scale by face value for actual dollars: Actual Price = Face Value × (TBILLPRICE/100).

3. **Price is always below 100 for positive discount:** If your result is >= 100, check for input errors (negative discount or calculation issue).

4. **Equivalent to PRICEDISC with basis=2:** TBILLPRICE(s,m,d) = PRICEDISC(s,m,d,100,2). Use TBILLPRICE for simplicity with T-bills.

5. **Maximum one-year maturity:** T-bills by definition mature within one year. Longer securities are Treasury notes or bonds.

6. **Use T+1 settlement for current pricing:** Standard T-bill settlement is next business day. Use tomorrow's date for current market value.

7. **Higher discount = lower price:** As rates rise, prices fall. This inverse relationship is fundamental to fixed income.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TBILLYIELD]] | Discount rate from price | When you have price and need discount rate |
| [[TBILLEQ]] | Bond-equivalent yield | When you need yield for bond comparison |
| [[PRICEDISC]] | General discount security price | For non-T-bill securities or when basis needed |

### Commonly Used Together

**[[TBILLYIELD]]** - T-Bill Yield (Discount)

*Inverse relationship:*
```
=TBILLPRICE(..., discount) → price
=TBILLYIELD(..., price) → discount
They are inverses
```
Use to verify calculations.

---

**[[TBILLEQ]]** - Bond Equivalent Yield

*Complete T-bill analysis:*
```
Price: =TBILLPRICE(..., 0.0525) = 98.69
Bond-Equivalent Yield: =TBILLEQ(..., 0.0525) = 5.41%
```
Both price and comparable yield from discount.

---

**[[PRICEDISC]]** - General Discount Price

*More flexible version:*
```
=TBILLPRICE(s, m, d) = =PRICEDISC(s, m, d, 100, 2)
PRICEDISC allows non-100 redemption and different basis
TBILLPRICE is simpler for T-bills
```
Use PRICEDISC for non-T-bill discount securities.

---

**[[DISC]]** - Discount Rate

*Reverse calculation:*
```
If you have price and need discount:
=DISC(settlement, maturity, price, 100, 2)
Or use =TBILLYIELD for T-bill specific
```
Convert from price to discount rate.

## Official Documentation

- **Microsoft Excel:** [TBILLPRICE function](https://support.microsoft.com/en-us/office/tbillprice-function-eacca992-c29d-425a-9eb8-0513fe6035a2)
- **Google Sheets:** [TBILLPRICE function](https://support.google.com/docs/answer/3093226)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
