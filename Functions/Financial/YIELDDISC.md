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
- yield-calculation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Discount Security Yield
- Discounted Yield
- Money Market Yield
tags:
- financial-modeling
- bond-analysis
- fixed-income
- treasury-bills
---

# YIELDDISC

## Description

**YIELDDISC** calculates the annual yield for a discounted security, given its price and redemption value. This function converts a discount security's price into an equivalent yield, allowing comparison with other investments like bonds and CDs that are quoted in yield terms. Unlike the discount rate (DISC), which is based on face value and understates true return, YIELDDISC calculates the actual rate of return based on the purchase price--what you actually invested.

Discounted securities such as Treasury bills, commercial paper, and banker's acceptances are quoted in the market using discount rates, not yields. However, yields are more meaningful for investment comparison because they show return on invested capital. A T-bill with a 5.25% discount rate has a higher yield because you invest less than face value but receive the full face value at maturity. YIELDDISC makes this conversion: it tells you the true annual return on your investment.

The formula is: Yield = (Redemption - Price) / Price x (Days in Year / Days to Maturity). Unlike DISC (which divides by Redemption), YIELDDISC divides by Price, giving a higher rate. The day count convention (basis parameter) affects the annualization. For most money market instruments, use Actual/360 (basis=2), though the result may differ slightly from the "bond-equivalent yield" convention used in some markets. YIELDDISC uses simple interest; for compound interest equivalents, additional conversion is needed.

YIELDDISC has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is essential for money market analysis, portfolio yield calculation, and investment comparison. The function is the inverse of PRICEDISC--given a yield from YIELDDISC, PRICEDISC will return the original price (though the conversion is not exactly symmetric due to the discount vs yield distinction).

## Syntax

> [!f(x)] YIELDDISC Syntax
>
> ```
> =YIELDDISC(settlement, maturity, pr, redemption, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you acquire the instrument. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The security's maturity date--when it matures and pays redemption. Must be after settlement. |
| `pr` | Yes | The security's price per $100 face value. For a security at $98.50, enter 98.5. |
| `redemption` | Yes | The redemption value per $100 face value. Usually 100 for standard instruments. |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year. | Some commercial paper |
| 1 | Actual/Actual | Actual days in period and year. | Some government securities |
| 2 | Actual/360 | Actual days, divide by 360. **Money market standard.** | US Treasury bills |
| 3 | Actual/365 | Actual days, divide by 365. | UK sterling instruments |
| 4 | European 30/360 | European month-end rules. | European commercial paper |

### Return Value

Returns a decimal value representing the annual yield. A result of 0.0540 means 5.40% annual yield.

## Examples

> [!f(x)] YIELDDISC Examples

### Example 1: Basic T-Bill Yield
```
=YIELDDISC("2024-03-15", "2024-06-13", 98.68, 100, 2)
```
**Result:** `0.0540` (5.40%)

**Explanation:** A 90-day T-bill at price 98.68 has a yield of 5.40%. Compare to DISC for the same security which gives 5.28%. The 12 basis point difference arises because yield is based on investment (98.68) while discount is based on face value (100).

---

### Example 2: Comparing YIELDDISC vs DISC
```
DISC:      =DISC("2024-03-15", "2024-06-13", 98.68, 100, 2) = 5.28%
YIELDDISC: =YIELDDISC("2024-03-15", "2024-06-13", 98.68, 100, 2) = 5.40%
Difference: 12 basis points
```
**Result:** YIELDDISC > DISC always

**Explanation:** For the same security, yield always exceeds discount rate. The higher the discount (lower price), the bigger the difference. This matters when comparing T-bills to bonds or CDs quoted in yield terms.

---

### Example 3: Commercial Paper
```
=YIELDDISC("2024-06-01", "2024-09-01", 98.75, 100, 0)
```
**Result:** `0.0506` (5.06%)

**Explanation:** 90-day commercial paper at $98.75 using 30/360. The yield is 5.06%, slightly higher than the discount rate. Commercial paper may use different day count conventions than T-bills.

---

### Example 4: Short-Term (30 Days)
```
=YIELDDISC("2024-06-01", "2024-07-01", 99.55, 100, 2)
```
**Result:** `0.0542` (5.42%)

**Explanation:** A 30-day instrument at 99.55 yields 5.42%. Short-term securities have prices close to par, but the yield annualization still gives meaningful percentage returns.

---

### Example 5: UK Style (Actual/365)
```
=YIELDDISC("2024-03-15", "2024-06-13", 98.68, 100, 3)
```
**Result:** `0.0548` (5.48%)

**Explanation:** Using Actual/365 produces a higher yield than Actual/360 because you multiply by a larger year. The same price shows different yields under different conventions--match to market standard.

---

### Example 6: 6-Month T-Bill
```
=YIELDDISC("2024-01-15", "2024-07-15", 97.40, 100, 2)
```
**Result:** `0.0528` (5.28%)

**Explanation:** A 6-month T-bill at 97.40 yields 5.28%. Longer-term discount securities accumulate more discount but the annualized yield may be similar to shorter-term instruments in a flat rate environment.

---

### Example 7: High Discount Rate Environment
```
=YIELDDISC("2024-06-01", "2024-09-01", 97.00, 100, 2)
```
**Result:** `0.1237` (12.37%)

**Explanation:** A 90-day instrument at 97.00 yields over 12%. In high-rate or crisis environments, money market yields can spike significantly. The yield-discount spread widens at higher rate levels.

---

### Example 8: Verifying with PRICEDISC
```
Step 1: Get yield from price
  =YIELDDISC("2024-03-15", "2024-06-13", 98.68, 100, 2) = 5.40%

Step 2: Note that PRICEDISC uses discount rate, not yield
  To verify: Calculate discount rate first using DISC
  =DISC("2024-03-15", "2024-06-13", 98.68, 100, 2) = 5.28%
  =PRICEDISC("2024-03-15", "2024-06-13", 0.0528, 100, 2) = 98.68 ✓
```
**Result:** Relationship verified through DISC

**Explanation:** YIELDDISC and PRICEDISC are not direct inverses (they use different rate concepts). To round-trip, use DISC with PRICEDISC or YIELDDISC with INTRATE.

---

### Example 9: Converting Discount Quote to Yield
```
Market Quote: T-bill at 5.25% discount
What is the equivalent yield?

First, get the price:
=PRICEDISC("2024-06-01", "2024-09-01", 0.0525, 100, 2) = 98.6875

Then get the yield:
=YIELDDISC("2024-06-01", "2024-09-01", 98.6875, 100, 2) = 5.39%
```
**Result:** 5.25% discount = 5.39% yield

**Explanation:** This two-step process converts discount quotes to yields for comparison. The 14 basis point spread allows proper comparison to bonds and CDs.

---

### Example 10: Using Cell References
```
=YIELDDISC(A2, B2, C2, D2, E2)
```
Where A2=Settlement, B2=Maturity, C2=Price, D2=Redemption, E2=Basis

**Result:** Varies by inputs

**Explanation:** Building a yield calculator with cell references enables rapid analysis of multiple securities. Link to market data for real-time yield monitoring.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric parameters | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Price >= Redemption | For discount securities, price must be below redemption. |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Yield seems wrong` | Wrong day count convention | Match basis to instrument type. T-bills use basis=2. |
| `Yield lower than expected` | Used discount rate instead of yield calculation | Ensure using YIELDDISC, not DISC, for yield output. |

## Use Cases

### [[Money Market Investment Comparison]]

**Scenario:** A portfolio manager needs to compare T-bill returns to a bank CD offering to determine the best short-term investment option.

**Implementation:**
```
T-bill Quote: 91-day at 5.22% discount rate
Convert to price: =PRICEDISC("2024-06-03", "2024-09-02", 0.0522, 100, 2) = 98.68
Convert to yield: =YIELDDISC("2024-06-03", "2024-09-02", 98.68, 100, 2) = 5.35%

Bank CD: 5.30% APY

Comparison: T-bill 5.35% > CD 5.30% → T-bill is better
```

**Business Application:** Investment decisions require comparing apples to apples. T-bills are quoted as discount rates; CDs are quoted as yields. YIELDDISC converts T-bill quotes to yields, enabling direct comparison.

**Technical Details:** Note that CD rates are usually quoted as APY (compound) while YIELDDISC gives simple yield. For precise comparison, convert to the same basis. The difference is usually small for short-term instruments.

---

### [[Portfolio Yield Reporting]]

**Scenario:** A money market fund needs to calculate the weighted average yield of its discount security holdings for daily NAV and SEC yield reporting.

**Implementation:**
```
For each discount security:
Position 1: =YIELDDISC(settlement, maturity, price, 100, 2) = 5.40%
Position 2: =YIELDDISC(settlement, maturity, price, 100, 2) = 5.25%
Position 3: =YIELDDISC(settlement, maturity, price, 100, 2) = 5.50%

Portfolio Yield = SUMPRODUCT(yields, market_values) / SUM(market_values)
```

**Business Application:** SEC regulations require money market funds to report 7-day yields in a specific format. Portfolio yield is the market-value-weighted average of individual security yields. YIELDDISC provides the yield input for each discount security.

**Technical Details:** Weight by market value, not face value. Include both discount securities (use YIELDDISC) and interest-bearing securities (use YIELD or direct rate). Ensure consistent day count treatment across portfolio.

---

### [[Relative Value Analysis]]

**Scenario:** A fixed-income analyst needs to identify mispriced T-bills by comparing actual yields to theoretical values based on the yield curve.

**Implementation:**
```
Actual: 91-day T-bill price = 98.65
=YIELDDISC("2024-06-01", "2024-08-31", 98.65, 100, 2) = 5.48%

Theoretical: Based on interpolated yield curve = 5.40%

Rich/Cheap: Actual 5.48% > Theoretical 5.40% → 8bp cheap (attractive)

Trade: Buy the cheap T-bill vs sell the curve (futures or other T-bills)
```

**Business Application:** Relative value traders profit from temporary mispricings. YIELDDISC converts prices to yields for curve comparison. Cheap securities (high yield relative to curve) are buys; rich securities (low yield) are sells.

**Technical Details:** Use consistent day count for all yield calculations. The yield curve is typically expressed in constant-maturity terms. Small mispricings (a few basis points) can be profitable at large sizes. Consider transaction costs and financing.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** (Redemption - Price) / Price × (YearDays / DaysToMaturity)
- **Precision:** Full floating-point precision

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Same five parameters (4 required, 1 optional)
- Same five day count conventions
- Returns annualized simple yield
- Always gives higher result than DISC for same inputs

## Tips and Best Practices

1. **YIELDDISC gives true return, DISC gives market quote:** For investment analysis, YIELDDISC is more meaningful. For trading at market rates, DISC matches market convention.

2. **Use basis=2 (Actual/360) for T-bills:** This is the money market standard. Different conventions give different yields for the same price.

3. **Yield is always greater than discount rate:** Because yield is based on investment (less than 100) while discount is based on face value (100). The spread widens at higher rates.

4. **YIELDDISC is simple yield, not bond-equivalent yield:** For exact comparison to semi-annual coupon bonds, use TBILLEQ which adjusts for bond pricing conventions.

5. **To convert discount quote to yield:** First use PRICEDISC to get price, then YIELDDISC to get yield. This two-step process is common in practice.

6. **Price must be less than redemption:** YIELDDISC requires a discount security. If price >= redemption, there is no positive yield from the discount.

7. **Functionally similar to INTRATE:** YIELDDISC and INTRATE produce identical results for the same inputs. Use either based on parameter name preference.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DISC]] | Discount rate from price | When you need market-convention discount rate |
| [[INTRATE]] | Interest rate on investment | Same calculation as YIELDDISC, different parameter name |
| [[TBILLYIELD]] | T-bill yield specifically | Purpose-built for T-bill analysis |
| [[TBILLEQ]] | T-bill bond-equivalent yield | For comparing to semi-annual bonds |

### Commonly Used Together

**[[DISC]]** - Discount Rate

*Comparing yield vs discount:*
```
DISC: Market quote convention, based on face value
YIELDDISC: True return, based on investment
YIELDDISC > DISC always
```
Both used in money market analysis.

---

**[[PRICEDISC]]** - Discount Security Price

*Conversion workflow:*
```
Discount Rate → Price: =PRICEDISC(...)
Price → Yield: =YIELDDISC(...)
```
Complete the discount-to-yield conversion.

---

**[[TBILLEQ]]** - Bond Equivalent Yield

*For bond comparison:*
```
Simple Yield: =YIELDDISC(...)
Bond-Equivalent Yield: =TBILLEQ(...)
```
Use TBILLEQ for semi-annual bond comparison.

---

**[[INTRATE]]** - Interest Rate

*Identical calculation:*
```
=YIELDDISC(settlement, maturity, price, redemption, basis)
=INTRATE(settlement, maturity, price, redemption, basis)
Same result, different parameter names
```
Use either based on context.

## Official Documentation

- **Microsoft Excel:** [YIELDDISC function](https://support.microsoft.com/en-us/office/yielddisc-function-a9dbdbae-7dae-46de-b995-615faffaaed7)
- **Google Sheets:** [YIELDDISC function](https://support.google.com/docs/answer/3093231)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
