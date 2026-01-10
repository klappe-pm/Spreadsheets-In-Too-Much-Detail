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
- maturity-securities
- yield-calculation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Yield at Maturity
- Maturity Interest Yield
- Add-on Interest Yield
tags:
- financial-modeling
- bond-analysis
- fixed-income
- money-market
---

# YIELDMAT

## Description

**YIELDMAT** calculates the annual yield of a security that pays interest at maturity, given its current price. These securities--often called "add-on interest" or "simple interest" securities--have a stated coupon rate that accrues from issue date to maturity, with all interest paid along with principal at maturity. YIELDMAT determines what annual return you will earn if you buy the security at the given price and hold it to maturity.

Unlike discount securities (which have no stated interest rate) or coupon bonds (which pay interest periodically), maturity-interest securities accumulate interest based on a stated rate. The price you pay may differ from par if market rates have changed since issuance. YIELDMAT accounts for both the stated interest you will receive and any capital gain or loss from buying above or below the security's accrued value. This makes it the inverse of PRICEMAT--given a yield from YIELDMAT, PRICEMAT will return the original price.

The calculation is more complex than simple yield because it must account for interest that has already accrued from the issue date to the settlement date. The buyer pays for this accrued interest (included in the price from PRICEMAT) but receives all interest at maturity. YIELDMAT uses the day count convention (basis parameter) consistently for both the accrual calculation and the discounting calculation. The resulting yield represents the true annual return on your investment.

YIELDMAT has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is used for pricing CDs in secondary markets, valuing corporate notes with single interest payments, and analyzing money market securities that use add-on interest conventions.

## Syntax

> [!f(x)] YIELDMAT Syntax
>
> ```
> =YIELDMAT(settlement, maturity, issue, rate, pr, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The settlement date--when you acquire the security. Must be on or after issue date. |
| `maturity` | Yes | The maturity date--when principal and all interest are paid. Must be after settlement. |
| `issue` | Yes | The issue date--when the security was originally issued and interest began accruing. |
| `rate` | Yes | The security's annual interest (coupon) rate as a decimal. For 5% rate, enter 0.05. |
| `pr` | Yes | The security's price per $100 face value (including accrued interest). |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year. | CDs, corporate instruments |
| 1 | Actual/Actual | Actual days in period and year. | Government securities |
| 2 | Actual/360 | Actual days, divide by 360. | Money market instruments |
| 3 | Actual/365 | Actual days, divide by 365. | UK sterling instruments |
| 4 | European 30/360 | European month-end rules. | European markets |

### Return Value

Returns a decimal value representing the annual yield. A result of 0.0550 means 5.50% annual yield.

## Examples

> [!f(x)] YIELDMAT Examples

### Example 1: Par Yield (Rate = Yield)
```
=YIELDMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 100, 0)
```
**Result:** `0.05` (5.00%)

**Explanation:** When settlement equals issue date and price equals par (100), yield equals the stated rate. No accrued interest, no premium or discount--the stated rate is the true return.

---

### Example 2: Premium Security (Price > Par)
```
=YIELDMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.06, 100.50, 0)
```
**Result:** `0.0500` (5.00%)

**Explanation:** This security pays 6% but trades at 100.50. The investor pays a $0.50 premium which is offset by the above-market coupon. The resulting yield (5.00%) is what the market requires.

---

### Example 3: Discount Security (Price < Par)
```
=YIELDMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.04, 99.50, 0)
```
**Result:** `0.0501` (5.01%)

**Explanation:** A 4% security at 99.50. The below-market coupon is compensated by buying at a discount. The capital gain at maturity plus the coupon produces a 5.01% yield.

---

### Example 4: With Accrued Interest
```
=YIELDMAT("2024-09-15", "2024-12-15", "2024-06-15", 0.05, 101.25, 0)
```
**Result:** `0.05` (5.00%)

**Explanation:** Settlement is 3 months after issue. The price of 101.25 includes par (100) plus 3 months accrued interest (1.25). Paying exactly par plus accrued at the stated rate produces a yield equal to the rate.

---

### Example 5: CD Secondary Market
```
=YIELDMAT("2024-09-15", "2024-12-15", "2024-06-15", 0.05, 101.50, 0)
```
**Result:** `0.0405` (4.05%)

**Explanation:** A 5% CD trading at 101.50 (premium). The buyer pays more than par plus accrued, so the effective yield (4.05%) is below the stated rate. This happens when market rates have fallen since issuance.

---

### Example 6: Money Market Convention (Actual/360)
```
=YIELDMAT("2024-06-01", "2024-09-01", "2024-06-01", 0.05, 99.75, 2)
```
**Result:** `0.0600` (6.00%)

**Explanation:** Using Actual/360 for a money market instrument. The security trades at a small discount (99.75), boosting yield above the 5% stated rate to 6.00%.

---

### Example 7: UK Style (Actual/365)
```
=YIELDMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 99.75, 3)
```
**Result:** `0.0551` (5.51%)

**Explanation:** Using Actual/365 for a sterling instrument. Different day count conventions produce different yields from the same price. Always match to market convention.

---

### Example 8: Verifying with PRICEMAT
```
Yield: =YIELDMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 99.76, 0) = 5.49%
Check: =PRICEMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 0.0549, 0) = 99.76 ✓
```
**Result:** YIELDMAT and PRICEMAT are inverses

**Explanation:** YIELDMAT calculates yield from price; PRICEMAT calculates price from yield. They should be mathematically consistent.

---

### Example 9: Long Accrual Period
```
=YIELDMAT("2024-11-01", "2024-12-15", "2024-03-15", 0.05, 103.50, 0)
```
**Result:** `0.0447` (4.47%)

**Explanation:** Settlement is 7.5 months after issue with only 1.5 months to maturity. Most of the interest has accrued ($3.12). Paying 103.50 with only $0.62 remaining interest produces a lower yield.

---

### Example 10: Using Cell References
```
=YIELDMAT(A2, B2, C2, D2, E2, F2)
```
Where A2=Settlement, B2=Maturity, C2=Issue, D2=Rate, E2=Price, F2=Basis

**Result:** Varies by inputs

**Explanation:** Building a yield model with cell references enables analysis of multiple securities. Change prices to see yield impact instantly.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric parameters | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement before issue date | Settlement must be on or after issue. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Negative rate or price | Both must be non-negative. Price must be positive. |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Yield seems wrong` | Price includes/excludes accrued inconsistently | PRICEMAT returns price including accrued; match YIELDMAT inputs accordingly. |

## Use Cases

### [[CD Secondary Market Analysis]]

**Scenario:** A money market trader needs to determine the yield on a CD being offered in the secondary market to decide if it meets investment criteria.

**Implementation:**
```
CD Details:
Issue Date: 2024-03-15
Maturity: 2024-09-15
Stated Rate: 5.25%
Offered Price: 102.75

Yield: =YIELDMAT("2024-06-15", "2024-09-15", "2024-03-15", 0.0525, 102.75, 0)
Result: 4.42%

Required Yield: 5.00%
Decision: 4.42% < 5.00% → Reject, yield too low
```

**Business Application:** Secondary CD markets allow investors to buy and sell CDs before maturity. YIELDMAT reveals the true return at the offered price. Compare to required yield to make buy/sell decisions.

**Technical Details:** The offered price includes accrued interest from issue to settlement. If the market has rallied (rates fell), CDs trade at premium and yield below stated rate. If rates rose, CDs trade at discount and yield above stated rate.

---

### [[Corporate Note Yield Calculation]]

**Scenario:** A portfolio manager needs to calculate the yield on a corporate note that pays interest at maturity for performance attribution.

**Implementation:**
```
Note Details:
Issue: 2024-01-15
Maturity: 2025-01-15
Rate: 5.50%
Current Mark: 102.25 (including accrued)

Yield: =YIELDMAT("2024-07-15", "2025-01-15", "2024-01-15", 0.055, 102.25, 0)
Result: 4.11%

Attribution: The 4.11% yield contributes to portfolio return based on weight
```

**Business Application:** Performance attribution requires accurate yield calculation for all holdings. YIELDMAT provides the expected return for maturity-interest securities based on current marks.

**Technical Details:** For multi-period attribution, track yield changes over time. Price appreciation/depreciation flows through income and capital gains separately. Match day count to the note's market convention.

---

### [[Yield Curve Placement]]

**Scenario:** A fixed-income analyst needs to place a maturity-interest security on the yield curve to assess relative value.

**Implementation:**
```
Security: 6-month CD, 5% coupon
Current Price: 99.50

Yield: =YIELDMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 99.50, 0) = 6.03%

Curve Check:
6-month Treasury: 5.25%
6-month AA Corporate: 5.50%
This CD at 6.03%: 78bp over AA → Cheap or distressed?
```

**Business Application:** Placing securities on the yield curve reveals relative value. Unusually high yields may indicate distressed issuers or attractive buying opportunities. YIELDMAT enables this analysis for maturity-interest securities.

**Technical Details:** Compare to similar-maturity, similar-credit securities. Spread analysis (yield minus benchmark) is more stable than outright yield comparison. Consider liquidity premiums for less-traded instruments.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** Accounts for accrued interest from issue to settlement
- **Input:** Price includes accrued interest (dirty price)

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Same six parameters (5 required, 1 optional)
- Same five day count conventions
- Requires issue date (unlike YIELD for coupon bonds)
- Price input is dirty price (includes accrued)

## Tips and Best Practices

1. **Price input is dirty price:** YIELDMAT expects price including accrued interest, consistent with PRICEMAT output. Don't subtract accrued before input.

2. **Issue date is required:** Unlike YIELD (for coupon bonds), YIELDMAT needs the issue date to calculate accrued interest from issue to settlement.

3. **Verify with PRICEMAT:** PRICEMAT(YIELDMAT(...)) should return the original price. Use this cross-check to validate inputs.

4. **Match day count to instrument:** CDs often use 30/360; money market uses Actual/360. Wrong basis gives wrong yield.

5. **When settlement = issue, no accrued:** For new issues where settlement equals issue date, there's no accrued interest complication and YIELDMAT behaves simply.

6. **Yield reflects total return:** YIELDMAT captures both coupon income and capital gain/loss from premium/discount. It's the comprehensive return measure.

7. **Compare to YIELD for coupon bonds:** For maturity-interest securities, use YIELDMAT. For periodic coupon bonds, use YIELD. Don't mix them.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PRICEMAT]] | Price from yield | When you know yield and need price |
| [[YIELD]] | Coupon bond yield | For bonds with periodic interest payments |
| [[YIELDDISC]] | Discount security yield | For zero-coupon discount instruments |

### Commonly Used Together

**[[PRICEMAT]]** - Price at Maturity

*Inverse relationship:*
```
=YIELDMAT(..., price, ...) → yield
=PRICEMAT(..., yield, ...) → price
They are inverses
```
Use to verify calculations.

---

**[[ACCRINTM]]** - Accrued Interest at Maturity

*Understanding price components:*
```
Dirty Price = Clean Value + Accrued Interest
YIELDMAT uses dirty price
ACCRINTM shows accrued component
```
Decompose price for analysis.

---

**[[INTRATE]]** - Interest Rate

*Related calculation:*
```
YIELDMAT: Yield given price, rate, and dates
INTRATE: Rate given investment and redemption amounts
Different parameterizations of return calculation
```
Both measure returns on maturity instruments.

---

**[[RECEIVED]]** - Maturity Amount

*Complete cash flow picture:*
```
Pay: PRICEMAT (or given price)
Receive: RECEIVED (or calculate from rate and par)
Return: YIELDMAT
```
Use together for full analysis.

## Official Documentation

- **Microsoft Excel:** [YIELDMAT function](https://support.microsoft.com/en-us/office/yieldmat-function-ba7d33f6-0e6e-4dba-9dd8-4e1f5b42b71c)
- **Google Sheets:** [YIELDMAT function](https://support.google.com/docs/answer/3093232)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
