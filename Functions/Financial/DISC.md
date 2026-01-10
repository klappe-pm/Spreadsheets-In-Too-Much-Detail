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
- Discount Rate
- Security Discount
- Bank Discount Rate
tags:
- financial-modeling
- bond-analysis
- fixed-income
- treasury-bills
---

# DISC

## Description

**DISC** calculates the discount rate for a security, which is the percentage by which a security's price is below its redemption (face) value, annualized according to the specified day count convention. This function is primarily used for discount securities--instruments like Treasury bills, commercial paper, and banker's acceptances that do not pay periodic interest but instead are sold below face value and redeemed at par. The "discount" is the investor's return.

The discount rate is different from yield, and understanding this distinction is crucial for money market professionals. The discount rate is calculated based on the face value (what you will receive), while yield is calculated based on the purchase price (what you actually invest). For example, a $100 T-bill selling at $98 has a discount rate based on $100, but the yield is based on $98. Because of this, yield is always higher than discount rate for the same security. The formula is: Discount Rate = (Redemption - Price) / Redemption × (Year Days / Days to Maturity).

Treasury bills are quoted in the market using discount rates, not yields, which is why DISC is essential for T-bill trading and analysis. When you see a T-bill quoted at "5.25%", that's the discount rate, not the yield. To compare T-bills to other investments like bonds or CDs (which are quoted in yield terms), you need to convert the discount rate to bond-equivalent yield using the TBILLEQ function. This historical convention dates back to when T-bills were the primary money market instrument and discount rates were simpler to calculate by hand.

DISC has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. The function requires settlement date, maturity date, current price, and redemption value, with an optional day count basis parameter. Note that DISC returns the annualized discount rate, not the actual discount amount--use PRICEDISC to work backward from discount rate to price.

## Syntax

> [!f(x)] DISC Syntax
>
> ```
> =DISC(settlement, maturity, pr, redemption, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you acquire the instrument. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The security's maturity date--when it matures and pays the redemption value. Must be after settlement. |
| `pr` | Yes | The security's price per $100 face value. For a security trading at $97.50, enter 97.5. |
| `redemption` | Yes | The redemption value per $100 face value. Usually 100 for standard instruments. |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year. | Some commercial paper |
| 1 | Actual/Actual | Actual days in period and year. | Some government securities |
| 2 | Actual/360 | Actual days, divide by 360. **Standard for T-bills.** | US Treasury bills, bank deposits |
| 3 | Actual/365 | Actual days, divide by 365. | UK instruments |
| 4 | European 30/360 | European month-end rules. | European commercial paper |

### Return Value

Returns a decimal value representing the annualized discount rate. A result of 0.0525 means 5.25% discount rate.

## Examples

> [!f(x)] DISC Examples

### Example 1: Treasury Bill Discount Rate
```
=DISC("2024-03-15", "2024-06-13", 98.68, 100, 2)
```
**Result:** `0.0528` (5.28%)

**Explanation:** A 90-day T-bill (using Actual/360, basis=2) priced at 98.68 has a discount rate of 5.28%. This is how T-bills are quoted in the market. The investor pays $98.68 and receives $100 at maturity, earning $1.32 over 90 days.

---

### Example 2: Commercial Paper
```
=DISC("2024-06-01", "2024-09-01", 98.75, 100, 0)
```
**Result:** `0.05` (5.00%)

**Explanation:** Commercial paper with 90-day maturity (using 30/360) at $98.75 price has 5.00% discount rate. The $1.25 discount on a 90-day instrument annualizes to 5% under the 30/360 convention.

---

### Example 3: Short-Term Instrument (30 Days)
```
=DISC("2024-06-01", "2024-07-01", 99.56, 100, 2)
```
**Result:** `0.0528` (5.28%)

**Explanation:** A 30-day instrument at 99.56 has a 5.28% discount rate. The $0.44 discount over 30 days annualizes to 5.28% using Actual/360 (30/360 × 0.44/100 × 360/30 = 5.28%).

---

### Example 4: 6-Month Discount Security
```
=DISC("2024-01-15", "2024-07-15", 97.35, 100, 2)
```
**Result:** `0.0529` (5.29%)

**Explanation:** A 6-month (182-day) discount security at 97.35 has a 5.29% discount rate. Longer-term discount securities show how the discount compounds over time--$2.65 over 182 days equals about 5.29% annualized.

---

### Example 5: Comparing Price Levels
```
Price 98.00: =DISC("2024-03-15", "2024-06-13", 98.00, 100, 2) = 8.00%
Price 99.00: =DISC("2024-03-15", "2024-06-13", 99.00, 100, 2) = 4.00%
Price 97.00: =DISC("2024-03-15", "2024-06-13", 97.00, 100, 2) = 12.00%
```
**Result:** Lower price = higher discount rate

**Explanation:** The relationship between price and discount rate is linear (unlike price-yield for coupon bonds). Each $1 price drop increases the annualized discount rate by approximately 4% for a 90-day instrument.

---

### Example 6: UK Style (Actual/365)
```
=DISC("2024-03-15", "2024-06-13", 98.70, 100, 3)
```
**Result:** `0.0536` (5.36%)

**Explanation:** Using Actual/365 (UK convention) instead of Actual/360 produces a higher discount rate because you divide by a larger year. The same price and term shows 5.36% vs 5.28% under different conventions--conventions matter!

---

### Example 7: Verifying with PRICEDISC
```
Discount Rate: =DISC("2024-03-15", "2024-06-13", 98.68, 100, 2) = 5.28%
Verification: =PRICEDISC("2024-03-15", "2024-06-13", 0.0528, 100, 2) = 98.68 ✓
```
**Result:** DISC and PRICEDISC are inverses

**Explanation:** DISC calculates rate from price; PRICEDISC calculates price from rate. Using one function's output in the other should return the original input. This verification ensures your inputs are correct.

---

### Example 8: Near-Par Pricing
```
=DISC("2024-06-01", "2024-06-08", 99.90, 100, 2)
```
**Result:** `0.0514` (5.14%)

**Explanation:** A 7-day instrument at 99.90 shows 5.14% discount. Very short-term instruments trade near par because there's little time to accrue discount. Even small discounts annualize to significant rates.

---

### Example 9: Non-Standard Redemption
```
=DISC("2024-03-15", "2024-06-13", 99.18, 100.5, 2)
```
**Result:** `0.0526` (5.26%)

**Explanation:** If a security redeems at 100.5 instead of 100 (perhaps a premium redemption or different convention), the discount rate changes accordingly. Most instruments use 100 redemption, but always verify.

---

### Example 10: Using Cell References
```
=DISC(A2, B2, C2, D2, E2)
```
Where A2=Settlement, B2=Maturity, C2=Price, D2=Redemption, E2=Basis

**Result:** Varies by inputs

**Explanation:** Building a money market trading model with cell references allows you to input market prices and instantly see discount rates. Link to real-time price feeds for live rate calculations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric parameters | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Price equals or exceeds redemption | For discount securities, price must be below redemption (else no discount). |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Unexpected result` | Wrong day count convention | T-bills use basis=2 (Actual/360). Using basis=0 gives wrong rate. |
| `Rate seems too high/low` | Wrong price input | Ensure price is per $100, not total dollar amount or percentage. |

## Use Cases

### [[Treasury Bill Trading]]

**Scenario:** A money market trader needs to calculate the discount rate for a T-bill quote to verify it matches market indications.

**Implementation:**
```
Market T-bill price: 98.6875 per $100 face
Days to maturity: 91 days
Settlement: Today, Maturity: +91 days

=DISC(TODAY(), TODAY()+91, 98.6875, 100, 2)
Result: 5.22% discount rate

Market quote: 5.22% → Matches ✓
```

**Business Application:** T-bill traders constantly convert between prices and discount rates. DISC provides quick verification that prices and rates align. Discrepancies indicate potential arbitrage opportunities or data errors that need investigation before trading.

**Technical Details:** T-bills are quoted in discount rate with two decimal places (e.g., 5.22%). Price precision is typically 1/32 or finer. Always use Actual/360 (basis=2) for T-bills. Settlement is T+1 for T-bills (next business day).

---

### [[Money Market Fund Yield Calculation]]

**Scenario:** A money market fund manager needs to calculate the book yield contribution from T-bill holdings for daily NAV and yield reporting.

**Implementation:**
```
For each T-bill holding:
Discount Rate: =DISC(settlement, maturity, purchase_price, 100, 2)
Bond Equivalent Yield: =TBILLEQ(settlement, maturity, discount_rate)

Report the bond equivalent yield for comparison to other holdings
```

**Business Application:** Money market funds hold various instruments--T-bills, commercial paper, CDs--with different yield conventions. Converting all to bond-equivalent yield allows apples-to-apples comparison and accurate portfolio yield calculation.

**Technical Details:** SEC requires 7-day yields to be reported in a specific format. The discount rate must be converted to bond-equivalent yield before averaging with other holdings. Weight by market value for portfolio yield.

---

### [[Commercial Paper Issuance Pricing]]

**Scenario:** A corporate treasurer issuing commercial paper needs to determine the appropriate price given target market discount rates.

**Implementation:**
```
Target discount rate: 5.50%
Term: 90 days
Face value: $50,000,000

Price calculation: =PRICEDISC(issue_date, maturity, 0.055, 100, 0)
Result: 98.625 per $100

Proceeds: $50M × 0.98625 = $49,312,500
Discount (interest cost): $50M - $49,312,500 = $687,500

Verify: =DISC(issue_date, maturity, 98.625, 100, 0) = 5.50% ✓
```

**Business Application:** Corporations issue commercial paper for short-term funding. The discount rate determines issuance proceeds and effective borrowing cost. Treasurers compare CP rates to bank lines of credit to choose the cheapest funding source.

**Technical Details:** Commercial paper typically uses 30/360 convention. Dealers quote rates; issuers determine proceeds using PRICEDISC. The effective annual rate (bank-equivalent) is higher than the discount rate--use TBILLEQ or YIELDDISC for comparison to bank loans.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** (Redemption - Price) / Redemption × (DayCountYear / DaysToMaturity)
- **Date Handling:** Accepts various formats based on locale

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Same five parameters (4 required, 1 optional)
- Same five day count conventions
- Returns annualized discount rate as decimal
- Price must be less than redemption for positive discount rate

## Tips and Best Practices

1. **Use basis=2 (Actual/360) for Treasury bills:** This is the market standard for T-bills. Using any other basis will give rates that don't match market quotes.

2. **Discount rate is NOT yield:** Discount rate is based on face value; yield is based on purchase price. Use TBILLEQ or YIELDDISC to convert to yield for comparison with other investments.

3. **Price must be below redemption:** For discount securities, price < redemption. If price >= redemption, there's no discount and DISC returns an error or zero.

4. **Verify with inverse function:** PRICEDISC(DISC(...)) should return original price. Use this cross-check to validate inputs.

5. **Watch day count conventions:** The same security shows different discount rates under different conventions. Always match the market convention for that instrument type.

6. **Convert quotes to consistent basis:** When comparing T-bills to commercial paper to CDs, convert all to the same yield basis before comparing.

7. **Short-term securities have high annualized rates:** A $0.10 discount on a 7-day $100 instrument annualizes to 5.2%. Don't be surprised by seemingly high rates on very short paper.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PRICEDISC]] | Price from discount rate | When you know the rate and need the price |
| [[YIELDDISC]] | Yield of discount security | When you need yield instead of discount rate |
| [[TBILLYIELD]] | T-bill yield specifically | For T-bill yield calculation |
| [[INTRATE]] | Interest rate | For securities paying interest at maturity |

### Commonly Used Together

**[[PRICEDISC]]** - Discount Security Price

*Inverse relationship:*
```
=DISC(...) gives rate from price
=PRICEDISC(...) gives price from rate
They are inverses of each other
```
Use to convert between price and rate.

---

**[[TBILLEQ]]** - T-Bill Equivalent Yield

*Converting discount to bond-equivalent yield:*
```
Discount Rate: =DISC(...) = 5.25%
Bond-Equivalent Yield: =TBILLEQ(...) = 5.42%
```
For comparing T-bills to bonds and other instruments.

---

**[[YIELDDISC]]** - Discount Security Yield

*Alternative yield calculation:*
```
Discount Rate: =DISC(...) based on face value
Yield: =YIELDDISC(...) based on purchase price
```
YIELDDISC gives true economic return on investment.

---

**[[TBILLPRICE]]** - T-Bill Price

*T-bill specific pricing:*
```
=TBILLPRICE(settlement, maturity, discount_rate)
```
Purpose-built for T-bill calculations.

## Official Documentation

- **Microsoft Excel:** [DISC function](https://support.microsoft.com/en-us/office/disc-function-71fce9f3-3f05-4acf-a5a3-eac6ef4daa53)
- **Google Sheets:** [DISC function](https://support.google.com/docs/answer/3093211)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
