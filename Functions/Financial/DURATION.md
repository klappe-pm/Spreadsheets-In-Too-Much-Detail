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
- Macaulay Duration
- Bond Duration
- Interest Rate Sensitivity
tags:
- financial-modeling
- bond-analysis
- fixed-income
- risk-management
---

# DURATION

## Description

**DURATION** calculates the Macaulay duration of a security that pays periodic interest. Macaulay duration, named after economist Frederick Macaulay who developed the concept in 1938, measures the weighted average time until a bond's cash flows are received. Think of it as the "center of gravity" of a bond's payments--the point in time where the present value of cash flows before that point equals the present value of cash flows after. Duration is expressed in years and is fundamental to understanding how bond prices respond to interest rate changes.

Duration serves two critical purposes in fixed-income investing. First, it measures interest rate sensitivity--bonds with higher duration experience larger price changes when yields move. A bond with 5-year duration will drop approximately 5% in price if yields rise 1%. Second, duration provides a tool for immunization strategies, where portfolio managers match the duration of assets to liabilities to protect against interest rate risk. Insurance companies and pension funds rely heavily on duration matching to ensure they can meet future obligations regardless of rate movements.

Several factors affect duration: **maturity** (longer maturity = higher duration), **coupon rate** (higher coupon = lower duration, because you receive cash sooner), **yield** (higher yield = lower duration, because far-off payments are discounted more heavily), and **payment frequency** (more frequent payments = slightly lower duration). A zero-coupon bond's duration equals its maturity because all cash flow comes at the end. Coupon bonds always have duration less than maturity because some cash flows arrive before maturity.

DURATION has been available in Excel since Excel 2007 (from Analysis ToolPak integration) and in Google Sheets since launch. The function uses the same day count conventions as other bond functions. Note that DURATION returns Macaulay duration; for risk calculations, you often need modified duration (use MDURATION), which adjusts for the yield level and provides a more direct measure of price sensitivity.

## Syntax

> [!f(x)] DURATION Syntax
>
> ```
> =DURATION(settlement, maturity, coupon, yld, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you acquire the bond. Enter as date value, date string, or cell reference. |
| `maturity` | Yes | The security's maturity date. Must be after settlement. |
| `coupon` | Yes | The security's annual coupon rate as a decimal. For 5% coupon, enter 0.05. |
| `yld` | Yes | The security's annual yield to maturity as a decimal. For 5.5% yield, enter 0.055. |
| `frequency` | Yes | Number of coupon payments per year: 1 = annual, 2 = semi-annual, 4 = quarterly. |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | Assumes 30 days per month, 360 days per year. | US corporate bonds |
| 1 | Actual/Actual | Uses actual days in each period. | US Treasury bonds |
| 2 | Actual/360 | Actual days divided by 360. | Money market instruments |
| 3 | Actual/365 | Actual days divided by 365. | Japanese government bonds |
| 4 | European 30/360 | European month-end adjustment rules. | Eurobonds |

### Return Value

Returns a numeric value representing the Macaulay duration in years. A result of 7.5 means the weighted average time to receive cash flows is 7.5 years.

## Examples

> [!f(x)] DURATION Examples

### Example 1: 10-Year Par Bond
```
=DURATION("2024-06-15", "2034-06-15", 0.05, 0.05, 2, 0)
```
**Result:** `7.99 years`

**Explanation:** A 10-year bond with 5% coupon at 5% yield (par) has duration of about 8 years. Even though maturity is 10 years, the weighted average time is shorter because coupon payments arrive throughout the life. Duration is always less than maturity for coupon bonds.

---

### Example 2: Zero-Coupon Bond
```
=DURATION("2024-06-15", "2034-06-15", 0, 0.05, 2, 0)
```
**Result:** `10.00 years`

**Explanation:** A zero-coupon bond has duration exactly equal to its maturity. With no intermediate payments, all cash flow comes at maturity, so the weighted average equals the maturity date. Zeros have maximum duration for their maturity, making them most sensitive to rates.

---

### Example 3: High-Coupon Bond (Lower Duration)
```
=DURATION("2024-06-15", "2034-06-15", 0.10, 0.05, 2, 0)
```
**Result:** `6.76 years`

**Explanation:** A 10% coupon bond (at 5% yield) has lower duration than a 5% coupon bond of same maturity. Higher coupons mean more cash flow arrives earlier, pulling the weighted average time closer to today. Premium bonds have shorter duration.

---

### Example 4: Low-Coupon Bond (Higher Duration)
```
=DURATION("2024-06-15", "2034-06-15", 0.02, 0.05, 2, 0)
```
**Result:** `8.98 years`

**Explanation:** A 2% coupon bond (at 5% yield) has higher duration. The small coupons provide little early cash flow, so most value comes from the principal repayment at maturity. Discount bonds have longer duration than par bonds.

---

### Example 5: Effect of Yield on Duration
```
Low yield:  =DURATION("2024-06-15", "2034-06-15", 0.05, 0.03, 2, 0) = 8.38 years
High yield: =DURATION("2024-06-15", "2034-06-15", 0.05, 0.08, 2, 0) = 7.45 years
```
**Result:** Higher yield = lower duration

**Explanation:** When yields are high, distant cash flows are discounted more heavily, making them contribute less to duration. Low-yield environments create longer duration and greater rate sensitivity--exactly when rates are most likely to rise.

---

### Example 6: Short-Term Bond
```
=DURATION("2024-06-15", "2026-06-15", 0.05, 0.05, 2, 0)
```
**Result:** `1.93 years`

**Explanation:** A 2-year bond has duration just under 2 years. Short-term bonds have low duration and minimal rate sensitivity. Money market funds target very short duration to maintain stable NAV.

---

### Example 7: Long-Term Bond (30 Years)
```
=DURATION("2024-06-15", "2054-06-15", 0.045, 0.05, 2, 0)
```
**Result:** `16.14 years`

**Explanation:** A 30-year bond at 4.5% coupon has duration around 16 years. Despite 30 years to maturity, the coupons pull weighted average to about half the maturity. Long bonds have high duration and significant rate risk.

---

### Example 8: US Treasury Bond (Actual/Actual)
```
=DURATION("2024-03-15", "2034-03-15", 0.0425, 0.045, 2, 1)
```
**Result:** `8.15 years`

**Explanation:** A 10-year Treasury using Actual/Actual basis (required for Treasuries). Duration helps Treasury traders manage rate exposure and construct hedges using futures contracts.

---

### Example 9: Annual Payment Bond
```
=DURATION("2024-06-15", "2034-06-15", 0.05, 0.05, 1, 0)
```
**Result:** `8.11 years`

**Explanation:** An annual-payment bond has slightly higher duration than semi-annual equivalent because payments are less frequent. Eurobonds often pay annually, resulting in marginally higher rate sensitivity.

---

### Example 10: Portfolio Duration Calculation
```
Bond A (50% weight): =DURATION(...) = 5.5 years
Bond B (30% weight): =DURATION(...) = 8.0 years
Bond C (20% weight): =DURATION(...) = 12.0 years

Portfolio Duration: =(0.50*5.5)+(0.30*8.0)+(0.20*12.0) = 7.55 years
```
**Result:** Weighted average = 7.55 years

**Explanation:** Portfolio duration is the market-value-weighted average of individual bond durations. This portfolio will move approximately 7.55% for every 1% change in yields (using modified duration approximation).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or text parameters | Ensure dates are valid Excel/Sheets dates. Use DATE() if needed. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. Check date order. |
| `#NUM!` | Negative coupon or yield | Both must be non-negative (zero is allowed for coupon). |
| `#NUM!` | Invalid frequency | Frequency must be 1, 2, or 4. |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Unexpectedly high result` | Near-zero yield | Very low yields produce very high durations. This is mathematically correct. |

## Use Cases

### [[Interest Rate Risk Management]]

**Scenario:** A bank's asset-liability manager needs to measure the interest rate sensitivity of the bond portfolio to ensure it matches the duration of deposit liabilities.

**Implementation:**
```
For each bond: =DURATION(settlement, maturity, coupon, yield, frequency, basis)
Portfolio Duration: =SUMPRODUCT(DurationArray, MarketValueWeights)

Target: Match portfolio duration to liability duration (e.g., 4.5 years)
Current: 6.2 years → Too long, need to shorten
Action: Sell long bonds, buy short bonds
```

**Business Application:** Banks face interest rate risk when asset duration differs from liability duration. If asset duration is longer, rising rates hurt assets more than liabilities, reducing equity. Duration matching ("immunization") protects against this risk.

**Technical Details:** Use modified duration for precise hedging calculations. Consider convexity for large rate moves. Rebalance periodically as duration drifts with time and rate changes. Key rate duration analysis shows sensitivity to specific points on the yield curve.

---

### [[Pension Fund Liability Matching]]

**Scenario:** A pension fund needs to construct a bond portfolio that will meet benefit payments over the next 20 years, regardless of interest rate movements.

**Implementation:**
```
Liability Duration: PV-weighted average time to benefit payments = 12.5 years
Asset Target: Build portfolio with duration = 12.5 years

Bond selection:
=DURATION(...long bond...) = 15 years × 60% weight
=DURATION(...medium bond...) = 8 years × 40% weight
Portfolio: (15×0.60) + (8×0.40) = 12.2 years ≈ target
```

**Business Application:** Pension funds have legal obligations to pay benefits regardless of market conditions. Duration matching ensures that if rates rise (lowering asset values), the present value of liabilities also falls proportionally, maintaining funded status.

**Technical Details:** Perfect immunization requires matching both duration and convexity. Cash flow matching is even more precise but constrains investment flexibility. Many funds use liability-driven investing (LDI) strategies combining duration targeting with return-seeking assets.

---

### [[Bond Trading Strategy: Duration Positioning]]

**Scenario:** A fixed-income portfolio manager believes interest rates will fall and wants to increase portfolio duration to profit from the expected rate decline.

**Implementation:**
```
Current portfolio duration: 5.5 years
Target if rates expected to fall 50bp: Increase to 8.0 years

Trade: Sell 3-year bonds (duration ≈ 2.8)
       Buy 20-year bonds (duration ≈ 13.5)

Expected profit if rates fall 50bp:
= Portfolio Value × New Duration × Rate Change
= $100M × 8.0 × 0.005 = $4M gain
```

**Business Application:** Active bond managers adjust duration based on rate forecasts. Extending duration before rate declines amplifies gains; shortening duration before rate increases minimizes losses. Duration positioning is a primary source of alpha in fixed-income.

**Technical Details:** Duration trades can be implemented with cash bonds or derivatives (futures, swaps). Futures are more capital-efficient. Consider the yield curve shape--a bull steepener vs bull flattener affects which duration extension strategy works best.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (was Analysis ToolPak add-in previously)
- **Precision:** Returns Macaulay duration to full floating-point precision
- **Behavior:** Consistent with standard bond math formulas

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Precision:** Results match Excel for identical inputs

### Both Platforms
- Returns Macaulay duration (not modified duration)
- Same six parameters (5 required, 1 optional)
- Same five day count conventions
- Duration always positive and less than or equal to maturity

## Tips and Best Practices

1. **Macaulay vs Modified Duration:** DURATION returns Macaulay duration. For price sensitivity calculations, convert to modified duration: Modified Duration = Macaulay Duration / (1 + yield/frequency). Or use MDURATION directly.

2. **Duration is an approximation:** Duration provides a linear estimate of price change. For large yield moves (>50bp), the actual price change differs due to convexity. Duration works best for small yield changes.

3. **Duration changes over time:** As a bond ages, its duration shortens (assuming constant yields). Portfolios must be rebalanced periodically to maintain target duration.

4. **Zero-coupon bonds have maximum duration:** If you want maximum rate sensitivity for a given maturity, use zeros. If you want minimum sensitivity, use high-coupon bonds.

5. **Weight by market value, not face value:** Portfolio duration is the market-value-weighted average. A $1M position in a premium bond counts for more than $1M face value.

6. **Consider key rate durations for curve exposure:** Overall duration measures parallel shift sensitivity. Key rate durations measure sensitivity to specific curve points (2-year, 5-year, 10-year, etc.).

7. **Match duration to investment horizon for immunization:** To immunize a liability due in 7 years, target 7-year duration. The portfolio will perform correctly regardless of rate movements.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[MDURATION]] | Modified duration | When calculating price sensitivity directly (more common in practice) |
| [[PRICE]] | Bond price | When you need price, not duration |
| [[YIELD]] | Bond yield | When you need yield, not duration |

### Commonly Used Together

**[[MDURATION]]** - Modified Duration

*Direct conversion from Macaulay:*
```
Macaulay: =DURATION(...)
Modified: =MDURATION(...) or =DURATION(...)/(1+yield/frequency)
```
Modified duration is more useful for risk calculations.

---

**[[PRICE]]** - Bond Price

*Combined for duration-based price estimation:*
```
Current Price: =PRICE(..., current_yield, ...)
If yield changes: New Price ≈ Current × (1 - Duration × Yield Change)
```
Duration helps estimate price impact of yield changes.

---

**[[YIELD]]** - Bond Yield

*Combined for portfolio analysis:*
```
Yield: =YIELD(...) for return measure
Duration: =DURATION(...) for risk measure
```
Yield and duration together characterize return/risk profile.

---

**[[SUMPRODUCT]]** - Weighted Average

*For portfolio duration:*
```
=SUMPRODUCT(DurationRange, WeightRange)
```
Calculate portfolio duration from individual bond durations.

## Official Documentation

- **Microsoft Excel:** [DURATION function](https://support.microsoft.com/en-us/office/duration-function-b254ea57-eadc-4602-a86a-c8e369334038)
- **Google Sheets:** [DURATION function](https://support.google.com/docs/answer/3093212)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
