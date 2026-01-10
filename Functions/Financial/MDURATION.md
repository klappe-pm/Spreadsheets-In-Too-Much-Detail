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
- risk-measurement
- securities
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Modified Duration
- Price Sensitivity
- Interest Rate Risk
tags:
- financial-modeling
- bond-analysis
- fixed-income
- risk-management
---

# MDURATION

## Description

**MDURATION** calculates the modified duration of a security that pays periodic interest. Modified duration is the practical, hands-on measure of interest rate risk--it directly tells you the percentage price change of a bond for a 1% (100 basis point) change in yield. If a bond has modified duration of 6.5, its price will fall approximately 6.5% if yields rise by 1%, or rise approximately 6.5% if yields fall by 1%. This direct interpretability makes modified duration the standard risk measure used by traders, portfolio managers, and risk officers.

Modified duration is derived from Macaulay duration by adjusting for the yield level: Modified Duration = Macaulay Duration / (1 + yield/frequency). This adjustment accounts for the fact that higher yields compress future cash flows more heavily in present value terms. The relationship between price and yield is not linear but curved (convex), so modified duration provides a first-order approximation that works well for small yield changes but becomes less accurate for large moves. For precise risk measurement on large yield changes, convexity adjustments are added.

The practical importance of modified duration cannot be overstated in fixed-income markets. Portfolio managers use it to: (1) measure and limit interest rate risk, (2) construct duration-neutral hedges, (3) implement tactical duration bets when forecasting rate movements, and (4) match asset duration to liability duration for immunization. Risk managers set duration limits and monitor exposures. Traders calculate dollar duration (modified duration times market value times 0.01) to know the dollar P&L impact of a 1 basis point yield move.

MDURATION has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It uses the same parameters and day count conventions as DURATION. For most practical applications in trading and risk management, MDURATION is preferred over DURATION because it gives direct price sensitivity without requiring additional calculations.

## Syntax

> [!f(x)] MDURATION Syntax
>
> ```
> =MDURATION(settlement, maturity, coupon, yld, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date. Enter as date value, date string, or cell reference. |
| `maturity` | Yes | The security's maturity date. Must be after settlement. |
| `coupon` | Yes | The security's annual coupon rate as a decimal. For 5% coupon, enter 0.05. |
| `yld` | Yes | The security's annual yield to maturity as a decimal. For 5.5% yield, enter 0.055. |
| `frequency` | Yes | Number of coupon payments per year: 1 = annual, 2 = semi-annual, 4 = quarterly. |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days per month, 360 per year. | US corporate bonds |
| 1 | Actual/Actual | Actual days in each period. | US Treasury bonds |
| 2 | Actual/360 | Actual days, divide by 360. | Money market |
| 3 | Actual/365 | Actual days, divide by 365. | Japanese bonds |
| 4 | European 30/360 | European month-end rules. | Eurobonds |

### Return Value

Returns a numeric value representing modified duration in years. This value directly indicates the percentage price change per 1% yield change.

## Examples

> [!f(x)] MDURATION Examples

### Example 1: Basic Modified Duration
```
=MDURATION("2024-06-15", "2034-06-15", 0.05, 0.05, 2, 0)
```
**Result:** `7.79 years`

**Explanation:** A 10-year bond at par (5% coupon, 5% yield) has modified duration of 7.79. This means a 1% yield increase (from 5% to 6%) would cause approximately 7.79% price decline. Conversely, a 1% yield decrease would cause about 7.79% price increase.

---

### Example 2: Comparing to Macaulay Duration
```
Macaulay: =DURATION("2024-06-15", "2034-06-15", 0.05, 0.05, 2, 0) = 7.99
Modified: =MDURATION("2024-06-15", "2034-06-15", 0.05, 0.05, 2, 0) = 7.79
Relationship: 7.99 / (1 + 0.05/2) = 7.99 / 1.025 = 7.79 ✓
```
**Result:** Modified = Macaulay / (1 + yield/frequency)

**Explanation:** Modified duration is always less than Macaulay duration. The adjustment factor (1 + yield/frequency) reflects the compounding effect. For semi-annual bonds at 5% yield, divide by 1.025.

---

### Example 3: Zero-Coupon Bond (Maximum Duration)
```
=MDURATION("2024-06-15", "2034-06-15", 0, 0.05, 2, 0)
```
**Result:** `9.76 years`

**Explanation:** A 10-year zero-coupon bond has modified duration of 9.76 years. Zero-coupon bonds have the highest duration for their maturity because all cash flow occurs at maturity. This bond's price would move almost 10% for each 1% yield change.

---

### Example 4: High-Coupon Bond (Lower Duration)
```
=MDURATION("2024-06-15", "2034-06-15", 0.10, 0.06, 2, 0)
```
**Result:** `6.40 years`

**Explanation:** A 10% coupon bond has modified duration of only 6.40 years despite 10 years to maturity. High coupons mean more cash arrives early, reducing sensitivity to distant yield changes. Premium bonds have lower duration than discount bonds of same maturity.

---

### Example 5: Dollar Duration Calculation
```
Modified Duration: =MDURATION("2024-06-15", "2034-06-15", 0.05, 0.055, 2, 0) = 7.67
Portfolio Value: $10,000,000
Dollar Duration (DV01): = $10,000,000 × 7.67 × 0.0001 = $7,670

Interpretation: Portfolio gains/loses $7,670 per basis point yield change
```
**Result:** $7,670 per basis point

**Explanation:** Dollar duration (also called DV01 - dollar value of a basis point) converts percentage risk to dollar risk. For a $10M portfolio with 7.67 modified duration, each 1bp yield move creates $7,670 P&L. This is how trading desks monitor risk.

---

### Example 6: Duration-Based Hedge Ratio
```
Bond to hedge: Mod Duration = 8.5, Value = $5M
Hedging instrument: Mod Duration = 4.2, Value = ?

Hedge Ratio = (8.5 × $5M) / 4.2 = $10.12M
Need to short $10.12M of the hedging instrument
```
**Result:** Hedge requires $10.12M position

**Explanation:** To duration-hedge a long position, short enough of another instrument so dollar durations match. The 8.5-duration bond needs a larger position in the 4.2-duration hedge to offset its rate sensitivity.

---

### Example 7: Short-Term Bond
```
=MDURATION("2024-06-15", "2025-06-15", 0.05, 0.05, 2, 0)
```
**Result:** `0.98 years`

**Explanation:** A 1-year bond has modified duration under 1 year. Short-term bonds have minimal rate sensitivity. Money market funds target near-zero duration to maintain stable NAV.

---

### Example 8: Long-Term Bond (High Duration)
```
=MDURATION("2024-06-15", "2054-06-15", 0.04, 0.045, 2, 0)
```
**Result:** `15.78 years`

**Explanation:** A 30-year bond has very high duration. A 1% yield move would change this bond's price by approximately 15.78%. Long bonds are highly sensitive to interest rates--favored when rates are expected to fall, avoided when rates might rise.

---

### Example 9: Effect of Yield Level
```
Low yield:  =MDURATION("2024-06-15", "2034-06-15", 0.05, 0.02, 2, 0) = 8.56
High yield: =MDURATION("2024-06-15", "2034-06-15", 0.05, 0.08, 2, 0) = 7.17
```
**Result:** Higher yields reduce duration

**Explanation:** At low yields, bonds have higher duration because distant cash flows are discounted less. In low-rate environments, duration risk is amplified--exactly when rising rates are most likely.

---

### Example 10: Portfolio Modified Duration
```
Bond A: Value=$4M, ModDur=5.2 → Contribution = 4M × 5.2
Bond B: Value=$3M, ModDur=8.1 → Contribution = 3M × 8.1
Bond C: Value=$3M, ModDur=12.5 → Contribution = 3M × 12.5

Portfolio ModDur = (4×5.2 + 3×8.1 + 3×12.5) / 10 = 8.26 years
```
**Result:** Portfolio duration = 8.26 years

**Explanation:** Portfolio modified duration is the market-value-weighted average. This $10M portfolio will gain or lose approximately 8.26% for every 1% yield change, or $8,260 per basis point.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or text in numeric fields | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement on or after maturity | Settlement must precede maturity. |
| `#NUM!` | Negative coupon or yield | Values must be non-negative. |
| `#NUM!` | Invalid frequency | Must be 1, 2, or 4. |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4. |
| `Result seems wrong` | Mismatched basis for security type | Use basis=1 for Treasuries, basis=0 for corporates. |

## Use Cases

### [[Value at Risk (VaR) Calculation]]

**Scenario:** A risk manager needs to calculate the 1-day 95% VaR for a bond portfolio, quantifying the potential loss under normal market conditions.

**Implementation:**
```
Portfolio Value: $50,000,000
Portfolio Modified Duration: =Weighted average = 6.8 years
Daily Yield Volatility: 5 basis points (historical)
95% Confidence Factor: 1.645

Dollar Duration: $50M × 6.8 × 0.0001 = $34,000 per bp
1-Day 95% VaR: $34,000 × 5bp × 1.645 = $279,650
```

**Business Application:** Financial institutions must report VaR to regulators and use it internally for risk limits. Modified duration translates yield volatility into price volatility, enabling bond VaR calculation. The $279,650 VaR means there's a 5% chance of losing more than this amount on any given day.

**Technical Details:** This is parametric VaR assuming normal yield changes. For fat-tailed distributions, use historical simulation or Monte Carlo. Convexity adjustment improves accuracy for large yield moves. Consider separate calculations for parallel and non-parallel curve shifts.

---

### [[Duration-Matched Hedging]]

**Scenario:** A corporate treasurer needs to hedge the interest rate risk of a bond portfolio using Treasury futures, ensuring the hedge neutralizes rate exposure.

**Implementation:**
```
Portfolio: $20M corporate bonds, Modified Duration = 7.2
Hedge: Treasury 10-year futures, Modified Duration = 8.5, Contract Value = $100,000

Dollar Duration of Portfolio: $20M × 7.2 / 100 = $1,440,000
Dollar Duration per Future: $100,000 × 8.5 / 100 = $8,500

Number of Contracts = $1,440,000 / $8,500 = 169 contracts (short)
```

**Business Application:** Companies holding bond portfolios can neutralize interest rate risk by shorting Treasury futures. If rates rise, losses on the bond portfolio are offset by gains on the short futures position. This isolates credit spread exposure while removing rate risk.

**Technical Details:** The hedge must be rebalanced as durations change. Basis risk exists between corporate bonds and Treasury futures (spreads can widen/tighten). Consider using credit derivatives for a more complete hedge that also addresses credit spread risk.

---

### [[Active Duration Management]]

**Scenario:** A portfolio manager believes the Federal Reserve will cut rates more than the market expects and wants to extend duration to profit from falling yields.

**Implementation:**
```
Current Portfolio: Duration = 5.5 years, Value = $100M
Target Duration: 8.0 years (bullish on rates)

Required Extension: +2.5 years
Method: Swap 20% of portfolio from 2-year bonds (dur=1.9) to 30-year bonds (dur=15.0)

Before: 100% × 5.5 = 5.5 years
After: 80% × 4.5 + 20% × 15.0 = 3.6 + 3.0 = 6.6 years

Or use interest rate swaps: Receive fixed to add duration
```

**Business Application:** Active managers generate alpha by successfully forecasting rate movements and positioning duration accordingly. Extending duration before rate cuts amplifies returns; shortening before rate increases limits losses.

**Technical Details:** Duration extensions can be achieved through cash bonds, futures, or swaps. Swaps are most capital-efficient. Consider the yield curve shape--in a steep curve, extending duration via long bonds also picks up yield. Monitor duration drift and rebalance periodically.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** MDURATION = DURATION / (1 + yield/frequency)
- **Precision:** Full floating-point precision

### Google Sheets
- **Availability:** All versions since launch
- **Identical Methodology:** Same calculation as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Returns modified duration directly
- Same six parameters
- Same five day count conventions
- Result always positive and less than maturity

## Tips and Best Practices

1. **Use modified duration for risk, Macaulay for immunization:** MDURATION directly gives price sensitivity. DURATION (Macaulay) is the proper measure for cash flow timing in liability matching.

2. **Duration is a linear approximation:** For large yield changes (>50bp), include convexity: Price Change ≈ -Duration × Yield Change + 0.5 × Convexity × (Yield Change)^2

3. **Dollar duration = Modified Duration × Market Value × 0.01:** This converts to dollars per basis point, the standard risk measure in trading.

4. **Duration changes with yields and time:** Duration is not static. As yields change or time passes, duration shifts. Monitor and rebalance hedges accordingly.

5. **For callable bonds, use effective duration:** Modified duration assumes cash flows are fixed. Callable bonds have uncertain cash flows; calculate effective duration numerically by shocking yields.

6. **Lower yield = higher duration = more risk:** In low-rate environments, bonds are more sensitive to rate changes. This is precisely when rates are most likely to rise.

7. **Negative duration exists:** Some instruments (like IO mortgage strips) have negative duration--they rise in value when rates rise. Useful for hedging.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DURATION]] | Macaulay duration | For immunization calculations or theoretical understanding |
| [[PRICE]] | Bond price | When you need price, not duration |
| [[YIELD]] | Bond yield | When you need yield, not duration |

### Commonly Used Together

**[[DURATION]]** - Macaulay Duration

*Conversion verification:*
```
=MDURATION(...) should equal =DURATION(...)/(1+yield/frequency)
```
Verify calculations are correct.

---

**[[PRICE]]** - Bond Price

*Duration-based price estimation:*
```
New Price ≈ Old Price × (1 - MDURATION × Yield Change)
Example: $100 × (1 - 7.0 × 0.01) = $93 if yields rise 1%
```
Quick price impact estimation.

---

**[[YIELD]]** - Bond Yield

*Combined analysis:*
```
Yield for return expectation
Modified duration for risk
Yield/Duration = risk-adjusted return concept
```
Both needed for complete bond analysis.

---

**[[SUMPRODUCT]]** - Portfolio Duration

*Weighted average:*
```
=SUMPRODUCT(ModDurationRange, MarketValueRange)/SUM(MarketValueRange)
```
Calculate portfolio-level modified duration.

## Official Documentation

- **Microsoft Excel:** [MDURATION function](https://support.microsoft.com/en-us/office/mduration-function-b3786a69-4f20-469a-94ad-33e5b90a763c)
- **Google Sheets:** [MDURATION function](https://support.google.com/docs/answer/3093218)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
