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
- securities
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Price at Maturity
- Maturity Interest Price
- Add-on Interest Security Price
tags:
- financial-modeling
- bond-analysis
- fixed-income
- money-market
---

# PRICEMAT

## Description

**PRICEMAT** calculates the price per $100 face value of a security that pays interest at maturity, given a specified yield. These securities--sometimes called "add-on interest" or "simple interest" securities--differ from both coupon bonds (which pay periodic interest) and discount securities (which pay no stated interest). Instead, they have a stated interest rate that accrues from issue to maturity, with all accumulated interest paid along with principal at maturity. PRICEMAT determines what price to pay today to achieve a target yield.

The key distinction is between the security's stated **coupon rate** (which determines how much interest accrues) and the **yield** (your required return, which determines the price you are willing to pay). If your yield equals the coupon rate, the price equals par (100). If your yield is higher than the coupon rate, you pay less than par (discount). If your yield is lower than the coupon rate, you pay more than par (premium). This price-yield relationship is the same as for coupon bonds but the cash flow structure is simpler.

The formula accounts for interest that has already accrued from the issue date to the settlement date. When you buy a security that pays at maturity, you must compensate the seller for interest accrued since issuance--similar to buying a coupon bond between coupon dates. PRICEMAT incorporates this accrued interest into the price calculation using the day count convention (basis parameter). The result is a "dirty price" that includes accrued interest, unlike PRICE which returns a clean price.

PRICEMAT has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. It is used for certificates of deposit, certain corporate notes, and other instruments structured with interest at maturity. The companion function YIELDMAT calculates yield when price is known.

## Syntax

> [!f(x)] PRICEMAT Syntax
>
> ```
> =PRICEMAT(settlement, maturity, issue, rate, yld, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The settlement date--when you acquire the security. Must be on or after issue date. |
| `maturity` | Yes | The maturity date--when principal and all interest are paid. Must be after settlement. |
| `issue` | Yes | The issue date--when the security was originally issued and interest began accruing. |
| `rate` | Yes | The security's annual interest (coupon) rate as a decimal. For 5% rate, enter 0.05. |
| `yld` | Yes | The required yield (discount rate) as a decimal. For 5.5% yield, enter 0.055. |
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

Returns a numeric value representing the price per $100 face value, including accrued interest from issue to settlement.

## Examples

> [!f(x)] PRICEMAT Examples

### Example 1: Par Pricing (Rate = Yield)
```
=PRICEMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 0.05, 0)
```
**Result:** `$100.00`

**Explanation:** When settlement equals issue date and yield equals rate, price equals par (100). The security is newly issued, no interest has accrued, and the return matches the stated rate.

---

### Example 2: Premium Pricing (Yield < Rate)
```
=PRICEMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.06, 0.05, 0)
```
**Result:** `$100.48`

**Explanation:** This security pays 6% but the market only requires 5% yield. Investors bid up the price above par to compete for the above-market return. The premium of $0.48 will be offset by lower yield than the coupon.

---

### Example 3: Discount Pricing (Yield > Rate)
```
=PRICEMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.04, 0.05, 0)
```
**Result:** `$99.52`

**Explanation:** This security pays only 4% but the market requires 5%. The price must be discounted so that the return (including capital gain at maturity) equals the required 5% yield.

---

### Example 4: With Accrued Interest
```
=PRICEMAT("2024-09-15", "2024-12-15", "2024-06-15", 0.05, 0.05, 0)
```
**Result:** `$101.25`

**Explanation:** Settlement is 3 months after issue. Interest has accrued for 3 months at 5% = $1.25 per $100. The buyer pays $101.25 (par plus accrued) and receives $102.50 at maturity (par plus 6 months interest). The effective yield is 5%.

---

### Example 5: CD Pricing
```
=PRICEMAT("2024-06-01", "2024-12-01", "2024-06-01", 0.0525, 0.055, 0)
```
**Result:** `$99.88`

**Explanation:** A 6-month CD with 5.25% stated rate but 5.5% required yield. The CD trades at a small discount because market rates (5.5%) exceed the stated rate (5.25%). The buyer pays $99.88 and receives $102.625 at maturity.

---

### Example 6: Money Market Convention (Actual/360)
```
=PRICEMAT("2024-06-01", "2024-09-01", "2024-06-01", 0.05, 0.055, 2)
```
**Result:** `$99.87`

**Explanation:** Using Actual/360 (basis=2) for a money market instrument. The day count affects both accrued interest and discounting calculations. Money market instruments typically use this convention.

---

### Example 7: Long Accrual Period
```
=PRICEMAT("2024-10-01", "2024-12-15", "2024-03-15", 0.05, 0.05, 0)
```
**Result:** `$102.71`

**Explanation:** Settlement is 6.5 months after issue with 2.5 months to maturity. Significant interest has accrued ($2.71). The buyer pays for accrued interest but will receive full interest at maturity ($3.75), netting $1.04 plus principal.

---

### Example 8: UK Style (Actual/365)
```
=PRICEMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 0.055, 3)
```
**Result:** `$99.76`

**Explanation:** Using Actual/365 for a sterling instrument. The different day count convention produces a slightly different price than 30/360 or Actual/360.

---

### Example 9: Verifying with YIELDMAT
```
Price: =PRICEMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 0.055, 0) = 99.76
Check: =YIELDMAT("2024-06-15", "2024-12-15", "2024-06-15", 0.05, 99.76, 0) = 5.50% ✓
```
**Result:** PRICEMAT and YIELDMAT are inverses

**Explanation:** PRICEMAT calculates price from yield; YIELDMAT calculates yield from price. They should be mathematically consistent.

---

### Example 10: Using Cell References
```
=PRICEMAT(A2, B2, C2, D2, E2, F2)
```
Where A2=Settlement, B2=Maturity, C2=Issue, D2=Rate, E2=Yield, F2=Basis

**Result:** Varies by inputs

**Explanation:** Building a pricing model with cell references allows scenario analysis--how does price change as yields move? Link to market data for real-time pricing.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric parameters | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement before issue date | Settlement must be on or after issue. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Negative rate or yield | Both must be non-negative. |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Price seems wrong` | Wrong day count convention | Match basis to instrument type. |

## Use Cases

### [[Certificate of Deposit Pricing]]

**Scenario:** A money market trader needs to price a CD in the secondary market, given current market yields that differ from the CD's stated rate.

**Implementation:**
```
CD Terms:
Issue Date: 2024-03-15
Maturity: 2024-09-15
Stated Rate: 5.00%
Current Market Yield: 5.25%

Price: =PRICEMAT("2024-06-15", "2024-09-15", "2024-03-15", 0.05, 0.0525, 0)
Result: $100.63 per $100 face

Purchase of $1M face: $1,006,300
Maturity Proceeds: $1,025,000 (principal + 6 months interest)
Profit: $18,700 over 3 months = 7.5% annualized
```

**Business Application:** CDs trade in secondary markets at prices reflecting current yields. When rates rise after issuance, CDs trade below the sum of principal plus accrued (effective discount). When rates fall, they trade at a premium. PRICEMAT provides fair value.

**Technical Details:** The price includes accrued interest--this is a "dirty price" concept. Compare to current yield quotes to determine if the CD is fairly priced. Consider early withdrawal penalties if applicable.

---

### [[Corporate Note Valuation]]

**Scenario:** A portfolio manager needs to mark-to-market a corporate note that pays interest at maturity, using current market yields for the issuer.

**Implementation:**
```
Note Details:
Issuer: Investment-grade corporate
Issue: 2024-01-15
Maturity: 2025-01-15
Rate: 5.50%
Current comparable yield: 5.25%

Fair Value: =PRICEMAT("2024-07-15", "2025-01-15", "2024-01-15", 0.055, 0.0525, 0)
Result: $103.12

Position: $5M face value
Market Value: $5,156,000
Book Value: $5,000,000
Unrealized Gain: $156,000
```

**Business Application:** Notes paying at maturity must be valued at current market rates for portfolio reporting. When rates fall (as in this example), notes appreciate. PRICEMAT provides the theoretical fair value based on yield.

**Technical Details:** Credit spreads affect required yield. If issuer credit deteriorates, required yield increases and price falls. PRICEMAT calculates mechanical price; credit analysis determines appropriate yield input.

---

### [[Yield Sensitivity Analysis]]

**Scenario:** A risk analyst needs to show how a security's price changes across a range of yield scenarios for stress testing.

**Implementation:**
```
Security: 1-year note, 5% coupon, issued today
Settlement = Issue = Today, Maturity = Today + 365

Yield Scenarios:
4.0%: =PRICEMAT(..., 0.04, ...) = $100.95
4.5%: =PRICEMAT(..., 0.045, ...) = $100.47
5.0%: =PRICEMAT(..., 0.05, ...) = $100.00
5.5%: =PRICEMAT(..., 0.055, ...) = $99.53
6.0%: =PRICEMAT(..., 0.06, ...) = $99.07

Sensitivity: ~$0.47 per 50bp yield move
```

**Business Application:** Risk managers create price-yield tables to understand exposure. Regulatory stress tests require showing portfolio value under various rate scenarios. PRICEMAT enables systematic scenario analysis.

**Technical Details:** The price-yield relationship is approximately linear for small yield changes but curves for large moves. For more precise risk measurement, calculate dollar duration (price change per basis point).

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** Includes accrued interest from issue to settlement
- **Returns:** Dirty price (includes accrued interest)

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Same six parameters (5 required, 1 optional)
- Same five day count conventions
- Returns price including accrued interest
- Requires issue date (unlike PRICE which doesn't)

## Tips and Best Practices

1. **PRICEMAT returns dirty price:** The result includes accrued interest from issue to settlement. This is different from PRICE, which returns clean price.

2. **Issue date is required:** Unlike PRICE (which only needs settlement and maturity), PRICEMAT needs the issue date to calculate accrued interest.

3. **Rate vs Yield distinction:** Rate is the stated coupon; Yield is your required return. Rate determines cash flow; Yield determines price.

4. **Settlement can equal issue date:** For new issues, settlement = issue. In this case, no interest has accrued and PRICEMAT behaves simply.

5. **Verify with YIELDMAT:** Calculate YIELDMAT using your PRICEMAT result. You should get back the original yield.

6. **Match basis to instrument:** CDs often use 30/360; money market uses Actual/360. Wrong basis gives wrong price.

7. **Price converges to principal+interest at maturity:** As settlement approaches maturity, price approaches face value plus total interest.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[YIELDMAT]] | Yield from price | When you know price and need yield |
| [[PRICE]] | Coupon bond price | For bonds with periodic interest payments |
| [[PRICEDISC]] | Discount security price | For zero-coupon discount instruments |

### Commonly Used Together

**[[YIELDMAT]]** - Yield at Maturity

*Inverse relationship:*
```
=PRICEMAT(..., yield, ...) → price
=YIELDMAT(..., price, ...) → yield
They are inverses
```
Use to verify calculations.

---

**[[ACCRINTM]]** - Accrued Interest at Maturity

*Interest component:*
```
PRICEMAT returns price including accrued interest
ACCRINTM returns just the accrued interest portion
Clean value = PRICEMAT - (ACCRINTM adjusted for settlement)
```
Separate interest from principal for accounting.

---

**[[RECEIVED]]** - Amount Received at Maturity

*Maturity value:*
```
PRICEMAT: What you pay at settlement
RECEIVED: What you receive at maturity
Return = RECEIVED - PRICEMAT
```
Both needed for full cash flow picture.

---

**[[INTRATE]]** - Interest Rate

*Rate from amounts:*
```
If you know investment and maturity amounts:
=INTRATE gives the implied rate
=PRICEMAT gives price from rate
```
Different approaches to same problem.

## Official Documentation

- **Microsoft Excel:** [PRICEMAT function](https://support.microsoft.com/en-us/office/pricemat-function-52c3b4da-bc7e-476a-989f-a95f675cae77)
- **Google Sheets:** [PRICEMAT function](https://support.google.com/docs/answer/3093223)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
