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
- bond-pricing
- odd-periods
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Odd First Price
- Odd First Period Price
- Short First Coupon Price
- Long First Coupon Price
tags:
- financial-modeling
- bond-analysis
- fixed-income
- odd-coupon
- securities-pricing
---

# ODDFPRICE

## Description

**ODDFPRICE** calculates the price per $100 face value of a security with an odd (irregular) first coupon period. Most bonds are designed to pay their first coupon exactly one standard period (6 months for semi-annual, 3 months for quarterly) after issue. However, newly issued bonds frequently have odd first periods because issuers choose issue dates for market timing reasons without regard to coupon schedules. A bond issued on March 15 with June 30/December 30 coupon dates has a "short" first period (3.5 months instead of 6). A bond issued January 15 with June 30/December 30 dates has a "long" first period (5.5 months instead of 6). ODDFPRICE handles both scenarios.

The standard PRICE function assumes regular coupon periods and will produce incorrect results for bonds with odd first periods. The error can be significant--potentially several price points--because the first coupon cash flow is different from standard periodic coupons. For a short first period, the bondholder receives less than a full coupon at the first payment; for a long first period, they receive more than a full coupon (accrued from issue to first payment). ODDFPRICE correctly handles these irregular cash flows by accepting both the issue date and first coupon date as separate parameters.

The function determines whether the first period is short or long by comparing the issue date to where a "normal" first coupon date would fall. It then calculates the correct first coupon amount (pro-rated for short periods, extended for long periods) and discounts all cash flows appropriately to arrive at the clean price. The mathematics is more complex than standard bond pricing because the first cash flow requires special treatment, but the underlying concept is the same: price equals the present value of all future cash flows.

ODDFPRICE has been available in Excel since Excel 2007 and in Google Sheets since launch. It requires eight parameters: settlement, maturity, issue date, first coupon date, coupon rate, yield, redemption value, and frequency, plus an optional day count basis. This is more parameters than the standard PRICE function because the odd first period requires specifying both when the bond was issued and when the first coupon actually pays.

## Syntax

> [!f(x)] ODDFPRICE Syntax
>
> ```
> =ODDFPRICE(settlement, maturity, issue, first_coupon, rate, yld, redemption, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when you acquire the bond. Must be after issue date. |
| `maturity` | Yes | The bond's maturity date--when principal is repaid. Must be after first coupon. |
| `issue` | Yes | The bond's original issue date--when the bond was first sold. Defines the start of the odd first period. |
| `first_coupon` | Yes | The date of the first coupon payment. This may be shorter or longer than a normal period from issue. |
| `rate` | Yes | The annual coupon rate as a decimal (e.g., 0.05 for 5%). |
| `yld` | Yes | The annual yield to maturity as a decimal (e.g., 0.06 for 6%). |
| `redemption` | Yes | The redemption value per $100 face value at maturity. Typically 100. |
| `frequency` | Yes | Coupon payments per year: 1 = annual, 2 = semi-annual, 4 = quarterly. |
| `basis` | No | Day count convention. Defaults to 0 (US 30/360). |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year | US corporate bonds |
| 1 | Actual/Actual | Actual calendar days | US Treasury bonds |
| 2 | Actual/360 | Actual days, 360-day year | Money market |
| 3 | Actual/365 | Actual days, 365-day year | Japanese bonds |
| 4 | European 30/360 | European month-end rules | Eurobonds |

### Return Value

Returns the clean price per $100 face value. A result of 98.50 means the bond trades at $98.50 per $100 face (98.5% of par). Add accrued interest to get the dirty (invoice) price.

## Examples

> [!f(x)] ODDFPRICE Examples

### Example 1: Short First Period (Issue Close to First Coupon)
```
=ODDFPRICE("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** `100.00` (approximately)

**Explanation:** Bond issued March 15, first coupon June 15--only 3 months instead of the normal 6. At par yield (5% coupon, 5% yield), the price is approximately 100. The short first period means the first coupon is pro-rated (approximately half of a normal coupon), but since yield equals coupon rate, the price is still near par.

---

### Example 2: Long First Period (Issue Well Before First Coupon)
```
=ODDFPRICE("2024-02-01", "2034-06-15", "2023-09-15", "2024-06-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** `100.00` (approximately)

**Explanation:** Bond issued September 15, first coupon June 15--9 months instead of the normal 6. This is a "long" first period. The first coupon will be larger than normal (1.5x for 9 months vs 6 months). At par yield, price remains near 100.

---

### Example 3: Short First with Premium (Coupon > Yield)
```
=ODDFPRICE("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.06, 0.05, 100, 2, 0)
```
**Result:** `107.84` (approximately)

**Explanation:** 6% coupon with 5% yield creates a premium bond. Even with the short first period, the above-market coupon makes this bond worth more than par. The short first coupon is smaller, but all subsequent coupons are full 6% semi-annual payments.

---

### Example 4: Short First with Discount (Coupon < Yield)
```
=ODDFPRICE("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.04, 0.05, 100, 2, 0)
```
**Result:** `92.23` (approximately)

**Explanation:** 4% coupon with 5% yield creates a discount bond. The short first period means slightly less interest initially, but the main driver of the discount is the below-market coupon rate sustained over the bond's life.

---

### Example 5: Very Short First Period (Days Before First Coupon)
```
=ODDFPRICE("2024-06-10", "2034-06-15", "2024-06-01", "2024-06-15", 0.05, 0.055, 100, 2, 0)
```
**Result:** `96.21` (approximately)

**Explanation:** Bond issued June 1, first coupon June 15--only 14 days! This extremely short first period means the first coupon is tiny. Settlement on June 10 is just 5 days before this small first payment. The discount mainly reflects the yield being higher than the coupon.

---

### Example 6: Long First Period with Premium
```
=ODDFPRICE("2024-02-01", "2034-06-15", "2023-06-15", "2024-06-15", 0.06, 0.05, 100, 2, 0)
```
**Result:** `108.50` (approximately)

**Explanation:** Bond issued June 15, 2023 with first coupon June 15, 2024--a full year, which is a very long first period (12 months vs normal 6). The first coupon is double-sized. Combined with a premium coupon rate, this produces a significant price above par.

---

### Example 7: Treasury-Style Bond (Actual/Actual)
```
=ODDFPRICE("2024-04-15", "2034-05-15", "2024-02-15", "2024-05-15", 0.0425, 0.045, 100, 2, 1)
```
**Result:** `97.75` (approximately)

**Explanation:** Using Actual/Actual (basis=1) for Treasury securities. Issue February 15, first coupon May 15 is a 3-month short first period. The higher yield vs coupon creates a discount.

---

### Example 8: Annual Coupon with Odd First
```
=ODDFPRICE("2024-03-01", "2034-12-15", "2023-09-15", "2024-12-15", 0.05, 0.055, 100, 1, 0)
```
**Result:** `96.02` (approximately)

**Explanation:** Annual coupon bond (frequency=1) with a long first period of 15 months (September to December of the following year). Annual bonds are less common but exist in European markets.

---

### Example 9: Quarterly Bond with Short First
```
=ODDFPRICE("2024-04-01", "2029-07-01", "2024-03-15", "2024-04-01", 0.05, 0.05, 100, 4, 0)
```
**Result:** `100.00` (approximately)

**Explanation:** Quarterly bond with only 17 days in the first period (March 15 to April 1). At par yield, price is approximately 100. Settlement exactly on first coupon date.

---

### Example 10: Comparing ODDFPRICE to PRICE
```
Regular (PRICE):   =PRICE("2024-06-15", "2034-06-15", 0.05, 0.055, 100, 2, 0)
Odd First:         =ODDFPRICE("2024-06-15", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 0.055, 100, 2, 0)
```
**Result:** Different prices (PRICE assumes regular periods; ODDFPRICE accounts for the short first)

**Explanation:** For a bond with an odd first period, PRICE gives incorrect results because it assumes the first period was standard. ODDFPRICE correctly handles the irregular first coupon. The difference depends on how "odd" the first period is.

---

### Example 11: Using Cell References
```
=ODDFPRICE(A2, B2, C2, D2, E2, F2, G2, H2, I2)
```
Where cells contain: Settlement, Maturity, Issue, First Coupon, Rate, Yield, Redemption, Frequency, Basis

**Result:** Price based on cell values

**Explanation:** In practice, parameters are typically in cells for portfolio pricing models. This allows pricing many odd-first bonds by copying the formula.

---

### Example 12: Sensitivity to First Coupon Date
```
First Coupon June 15: =ODDFPRICE("2024-05-01", "2034-06-15", "2024-03-15", "2024-06-15", 0.05, 0.05, 100, 2, 0)
First Coupon Sept 15: =ODDFPRICE("2024-05-01", "2034-09-15", "2024-03-15", "2024-09-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** Prices differ due to different first period lengths

**Explanation:** Moving the first coupon from June to September changes it from a short first period to a long first period. This affects the first coupon amount and all subsequent cash flow timing.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format | Use DATE() function or verify date formats |
| `#NUM!` | Dates in wrong order | Must be: issue <= settlement < first_coupon <= maturity |
| `#NUM!` | Invalid frequency | Must be 1, 2, or 4 |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4 |
| `#NUM!` | Negative rate, yield, or redemption | All must be non-negative |
| `Incorrect result` | Using PRICE instead of ODDFPRICE | For odd first periods, must use ODDFPRICE |
| `Large price difference` | Wrong first coupon date | Verify the actual first coupon payment date from prospectus |

## Use Cases

### [[New Issue Bond Pricing]]

**Scenario:** An underwriter needs to price a new corporate bond issue where the issue date does not align with the standard coupon schedule.

**Implementation:**
```
Issue Parameters:
Issue Date: March 15, 2024
First Coupon: June 30, 2024 (3.5 month short first)
Maturity: June 30, 2034
Coupon Rate: 5.25%
Target Yield: 5.50%

Price: =ODDFPRICE(Settlement, "2034-06-30", "2024-03-15", "2024-06-30", 0.0525, 0.055, 100, 2, 0)
```

**Business Application:** New bond issues frequently have odd first periods because issuers choose launch dates based on market conditions, not coupon schedules. ODDFPRICE ensures accurate pricing that accounts for the irregular first cash flow.

**Technical Details:** The settlement date for new issues is typically issue date + a few days. Verify the exact first coupon date from the offering documents.

---

### [[Secondary Market Odd-First Trading]]

**Scenario:** A portfolio manager needs to price a bond that was originally issued with an odd first period for secondary market trading.

**Implementation:**
```
Trade settlement during first period:
Settlement: 2024-05-15 (after issue, before first coupon)
Issue: 2024-03-15
First Coupon: 2024-06-30
Maturity: 2034-06-30

Clean Price: =ODDFPRICE("2024-05-15", "2034-06-30", "2024-03-15", "2024-06-30", Rate, Yield, 100, 2, 0)
Accrued Interest: Calculate from issue date to settlement (special for first period)
Dirty Price: Clean + Accrued
```

**Business Application:** When trading bonds during their odd first period, the standard accrued interest calculation may not apply. ODDFPRICE handles the pricing; accrued interest from issue to settlement must be calculated separately.

**Technical Details:** After the first coupon pays, the bond has regular periods and PRICE can be used (settlement > first_coupon). ODDFPRICE is specifically needed when settlement falls before or around the odd first coupon.

---

### [[Odd-First Bond Model Validation]]

**Scenario:** A quant developer needs to validate their bond pricing model against Excel for odd first period bonds.

**Implementation:**
```
Test Cases:
Short First: =ODDFPRICE(settle, mat, issue, first, 0.05, 0.05, 100, 2, 0)
Long First: =ODDFPRICE(settle, mat, issue, first, 0.05, 0.05, 100, 2, 0)
Various Yields: Range of yields from 3% to 7%

Compare model output to ODDFPRICE results
Tolerance: < 0.01 price points
```

**Business Application:** Custom pricing systems must match market conventions. ODDFPRICE provides benchmark values for validating internal models handling odd first periods.

**Technical Details:** Test edge cases: settlement on issue date, settlement on first coupon date, very short periods, very long periods. Document any discrepancies from rounding differences.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Precision:** IEEE 754 double precision
- **Date Handling:** Flexible regional formats

### Google Sheets
- **Availability:** All versions since launch
- **Identical Results:** Returns same values as Excel
- **Date Handling:** Stricter; prefer DATE() function

### Both Platforms
- Same nine parameters (8 required, 1 optional)
- Same five day count conventions
- Returns clean price per $100 face value
- Requires: issue <= settlement < first_coupon <= maturity

## Tips and Best Practices

1. **Use for new issues:** Most newly issued bonds have odd first periods. Always check if ODDFPRICE is needed rather than assuming PRICE works.

2. **Verify dates from prospectus:** The issue date and first coupon date must be exact. These come from the bond's official documentation, not estimates.

3. **Date order matters:** Excel requires issue <= settlement < first_coupon <= maturity. Violating this order produces #NUM! errors.

4. **After first coupon, use PRICE:** Once the first coupon has paid and settlement is in a regular period, standard PRICE is appropriate. ODDFPRICE is specifically for the odd first period.

5. **Short vs long first:** ODDFPRICE handles both automatically based on the issue and first coupon dates. You do not need to specify which type of odd period it is.

6. **Accrued interest is separate:** ODDFPRICE returns clean price. For the odd first period, accrued interest calculation requires special handling (from issue date, not previous coupon date).

7. **Combine with ODDFYIELD:** Use ODDFPRICE to find price given yield, or ODDFYIELD to find yield given price. They are inverse functions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PRICE]] | Bond price with regular periods | When first period is standard length |
| [[ODDLPRICE]] | Price with odd last period | When the last period is irregular, not first |
| [[ODDFYIELD]] | Yield with odd first period | When you know price and want yield |
| [[PRICEDISC]] | Discounted security price | For zero-coupon or discount instruments |

### Commonly Used Together

**[[ODDFYIELD]]** - Odd First Period Yield

*Inverse relationship:*
```
Price to Yield: =ODDFYIELD(settle, mat, issue, first, rate, PRICE, redemption, freq, basis)
Yield to Price: =ODDFPRICE(settle, mat, issue, first, rate, YIELD, redemption, freq, basis)
```
These are inverse functions for odd first period bonds.

---

**[[ACCRINT]]** - Accrued Interest

*Note: May need special handling for odd first period:*
```
Regular ACCRINT uses previous coupon; for odd first, accrued runs from issue date.
Manual: =(Settlement - Issue) / Period_Days * First_Coupon_Amount
```
Accrued interest in the first period is from issue date, not a previous coupon.

---

**[[PRICE]]** - Standard Bond Price

*Comparison for validation:*
```
PRICE (regular): =PRICE(settle, mat, rate, yld, redemption, freq, basis)
ODDFPRICE (odd): =ODDFPRICE(settle, mat, issue, first, rate, yld, redemption, freq, basis)
```
Compare to understand the impact of the odd first period on price.

---

**[[YIELD]]** - Standard Bond Yield

*After first coupon, revert to standard:*
```
During odd first period: Use ODDFYIELD
After first coupon paid: Can use standard YIELD
```
Once past the odd first period, standard functions apply.

## Official Documentation

- **Microsoft Excel:** [ODDFPRICE function](https://support.microsoft.com/en-us/office/oddfprice-function-d7d664a8-34df-4233-8d2b-922bcf6a69e1)
- **Google Sheets:** [ODDFPRICE function](https://support.google.com/docs/answer/3093186)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
