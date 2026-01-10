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
- Odd Last Price
- Odd Last Period Price
- Short Last Coupon Price
- Long Last Coupon Price
tags:
- financial-modeling
- bond-analysis
- fixed-income
- odd-coupon
- securities-pricing
---

# ODDLPRICE

## Description

**ODDLPRICE** calculates the price per $100 face value of a security with an odd (irregular) last coupon period. While most bonds have regular coupon periods throughout their life, the final period before maturity sometimes differs from standard. This can happen when a bond's maturity date does not fall exactly on a scheduled coupon date, when bonds are restructured, or when call provisions are exercised at non-standard dates. A bond with semi-annual coupons on June 30 and December 31 that matures on October 15 has a short last period (3.5 months instead of 6).

The standard PRICE function assumes all coupon periods, including the final one, are regular length. When the last period is irregular, PRICE produces incorrect results because it miscalculates the final cash flow's timing and, for short periods, the final coupon amount itself. The error magnitude depends on how different the last period is from standard. ODDLPRICE correctly handles these situations by accepting the last coupon date as a separate parameter, enabling accurate calculation of the final period's cash flows.

Unlike ODDFPRICE (which handles odd first periods at bond issuance), ODDLPRICE is relevant when you're settling into a bond near maturity and the remaining period is irregular. This scenario is common for seasoned bonds where the original maturity date did not align with coupon schedules, for bonds called early, or for certain structured products. The function calculates the present value of the remaining cash flows--typically just one more coupon (possibly fractional) plus principal--discounted from maturity back to settlement.

ODDLPRICE has been available in Excel since Excel 2007 and in Google Sheets since launch. It requires eight parameters: settlement, maturity, last coupon date, coupon rate, yield, redemption value, frequency, and day count basis. The "last coupon" parameter refers to the most recent coupon payment date before settlement--this defines the start of the odd last period.

## Syntax

> [!f(x)] ODDLPRICE Syntax
>
> ```
> =ODDLPRICE(settlement, maturity, last_interest, rate, yld, redemption, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when you acquire the bond. Must be after last interest date. |
| `maturity` | Yes | The bond's maturity date--when principal is repaid. This is the end of the odd last period. |
| `last_interest` | Yes | The date of the last (most recent) coupon payment before settlement. Defines the start of the odd last period. |
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

Returns the clean price per $100 face value. A result of 99.50 means the bond trades at $99.50 per $100 face (99.5% of par). Add accrued interest to get the dirty (invoice) price.

## Examples

> [!f(x)] ODDLPRICE Examples

### Example 1: Short Last Period at Par
```
=ODDLPRICE("2024-10-01", "2024-12-15", "2024-06-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** `100.00` (approximately)

**Explanation:** Last coupon was June 15; maturity is December 15. Settlement October 1 is in the final period. At par yield (5% coupon = 5% yield), price is approximately 100. The last period is a normal 6 months, but we're using ODDLPRICE because we're in the final coupon period with only one payment remaining.

---

### Example 2: Very Short Last Period
```
=ODDLPRICE("2024-11-01", "2024-11-15", "2024-09-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** `100.00` (approximately)

**Explanation:** Last coupon September 15; maturity November 15--only 2 months, not the normal 6. Settlement November 1 is just 14 days before maturity. The final coupon is pro-rated (2/6 of a normal coupon). At par yield, price remains near 100.

---

### Example 3: Short Last Period at Discount
```
=ODDLPRICE("2024-10-01", "2024-11-15", "2024-06-15", 0.05, 0.06, 100, 2, 0)
```
**Result:** `99.55` (approximately)

**Explanation:** Last coupon June 15; maturity November 15 (5 months later, not 6). Yield (6%) exceeds coupon (5%), creating a discount. With only about 6 weeks to maturity, the discount is small because there's little time for the yield difference to compound.

---

### Example 4: Short Last Period at Premium
```
=ODDLPRICE("2024-10-01", "2024-11-15", "2024-06-15", 0.06, 0.05, 100, 2, 0)
```
**Result:** `100.46` (approximately)

**Explanation:** 6% coupon with 5% yield creates a premium. The short last period (5 months vs 6) means slightly less final coupon, but the above-market rate still produces a premium price.

---

### Example 5: Long Last Period
```
=ODDLPRICE("2024-02-01", "2024-09-30", "2024-01-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** `100.00` (approximately)

**Explanation:** Last coupon January 15; maturity September 30--about 8.5 months, longer than the normal 6. This creates a "long" last period where the final coupon is larger than normal. At par yield, price is still approximately 100.

---

### Example 6: Near Maturity (Days Away)
```
=ODDLPRICE("2024-12-10", "2024-12-15", "2024-06-15", 0.05, 0.04, 100, 2, 0)
```
**Result:** `100.05` (approximately)

**Explanation:** Settlement is just 5 days before maturity. At this point, price is almost entirely driven by the imminent principal repayment plus the small remaining coupon. The premium reflects the higher coupon vs yield, but with only days remaining, the effect is minimal.

---

### Example 7: Treasury Bond (Actual/Actual)
```
=ODDLPRICE("2024-10-15", "2024-12-31", "2024-06-30", 0.04, 0.045, 100, 2, 1)
```
**Result:** `99.75` (approximately)

**Explanation:** Using Actual/Actual (basis=1) for Treasury bonds. Last coupon June 30; maturity December 31 is exactly 6 months (normal). Settlement October 15 puts you in the final period. Yield > coupon creates a small discount.

---

### Example 8: Callable Bond Called at Non-Standard Date
```
=ODDLPRICE("2024-08-01", "2024-10-15", "2024-06-15", 0.055, 0.04, 100, 2, 0)
```
**Result:** `100.60` (approximately)

**Explanation:** Bond called for October 15 redemption (not a regular coupon date). Last coupon was June 15, so the final period is 4 months, not 6. The premium coupon (5.5% vs 4% yield) creates a price above par.

---

### Example 9: Annual Bond with Odd Last
```
=ODDLPRICE("2024-09-01", "2025-03-15", "2024-06-15", 0.05, 0.055, 100, 1, 0)
```
**Result:** `99.68` (approximately)

**Explanation:** Annual coupon bond (frequency=1) with last coupon June 15, maturing March 15--9 months, less than a full year. The discount reflects the higher yield vs coupon rate.

---

### Example 10: Quarterly Bond with Odd Last
```
=ODDLPRICE("2024-11-01", "2024-12-10", "2024-10-15", 0.05, 0.05, 100, 4, 0)
```
**Result:** `100.00` (approximately)

**Explanation:** Quarterly bond with last coupon October 15; maturity December 10 (about 2 months, less than the normal 3-month quarter). At par yield, price is approximately 100.

---

### Example 11: Comparing ODDLPRICE to PRICE
```
Regular PRICE: =PRICE("2024-10-01", "2024-12-15", 0.05, 0.05, 100, 2, 0)
Odd Last PRICE: =ODDLPRICE("2024-10-01", "2024-12-15", "2024-06-15", 0.05, 0.05, 100, 2, 0)
```
**Result:** Results should be similar when the last period is normal length

**Explanation:** When the last period equals a standard period (6 months for semi-annual), both functions should produce similar results. The difference becomes significant when the last period is truly irregular.

---

### Example 12: Using Cell References
```
=ODDLPRICE(A2, B2, C2, D2, E2, F2, G2, H2)
```
Where cells contain: Settlement, Maturity, Last Interest, Rate, Yield, Redemption, Frequency, Basis

**Result:** Price based on cell values

**Explanation:** For portfolio analysis, cell references enable pricing multiple bonds with odd last periods by copying the formula.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format | Use DATE() function or verify date formats |
| `#NUM!` | Dates in wrong order | Must be: last_interest < settlement < maturity |
| `#NUM!` | Invalid frequency | Must be 1, 2, or 4 |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4 |
| `#NUM!` | Negative rate, yield, or redemption | All must be non-negative |
| `Wrong result` | Using PRICE for odd last period | Must use ODDLPRICE when last period is irregular |
| `Settlement before last interest` | Dates reversed | Verify date order; last_interest is before settlement |

## Use Cases

### [[Near-Maturity Bond Pricing]]

**Scenario:** A trader needs to price a bond in its final coupon period where the period length is non-standard.

**Implementation:**
```
Bond Details:
Last Coupon: June 15, 2024
Maturity: October 31, 2024 (4.5 months, not 6)
Settlement: August 1, 2024
Coupon: 5%
Yield: 5.25%

Price: =ODDLPRICE("2024-08-01", "2024-10-31", "2024-06-15", 0.05, 0.0525, 100, 2, 0)
```

**Business Application:** Bonds approaching non-standard maturity dates require ODDLPRICE for accurate pricing. The final coupon is pro-rated based on the actual last period length, affecting the fair value.

**Technical Details:** Settlement must be after the last coupon date. If settlement is before the last coupon, the bond is not yet in its "last" period and standard PRICE or other functions apply.

---

### [[Called Bond Pricing]]

**Scenario:** A portfolio manager needs to price a callable bond that has been called for redemption on a non-standard date.

**Implementation:**
```
Call Details:
Call Date (effective maturity): September 15, 2024
Call Price: 102 (premium call)
Last Coupon: June 30, 2024
Settlement: August 1, 2024

Price: =ODDLPRICE("2024-08-01", "2024-09-15", "2024-06-30", Rate, Yield, 102, 2, 0)
```

**Business Application:** When bonds are called, the redemption date often does not match the regular coupon schedule. ODDLPRICE handles this by treating the call date as maturity with a potentially irregular final period.

**Technical Details:** Use the call price (102 in example) as the redemption value, not 100. The yield should reflect yield-to-call, not yield-to-maturity.

---

### [[Restructured Bond Pricing]]

**Scenario:** A distressed debt analyst needs to price a bond that has been restructured with a new maturity date that does not match the original coupon schedule.

**Implementation:**
```
Restructured Terms:
New Maturity: March 15, 2025 (does not match June/December schedule)
Original Frequency: Semi-annual (June/December)
Last Coupon: December 15, 2024
Settlement: January 15, 2025

Price: =ODDLPRICE("2025-01-15", "2025-03-15", "2024-12-15", New_Rate, Yield, Redemption, 2, 0)
```

**Business Application:** Bond restructurings often result in new maturity dates that create odd final periods. ODDLPRICE enables accurate valuation post-restructuring.

**Technical Details:** Restructured bonds may also have different coupon rates or redemption values. Ensure all parameters reflect the restructured terms.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Precision:** IEEE 754 double precision
- **Date Handling:** Accepts various regional date formats

### Google Sheets
- **Availability:** All versions since launch
- **Identical Results:** Returns same values as Excel
- **Date Handling:** Stricter; recommend DATE() function

### Both Platforms
- Same eight parameters (7 required, 1 optional)
- Same five day count conventions
- Returns clean price per $100 face value
- Requires: last_interest < settlement < maturity

## Tips and Best Practices

1. **Identify when to use ODDLPRICE:** Use when settlement is in the final coupon period AND that period's length differs from standard (e.g., not exactly 6 months for semi-annual bonds).

2. **Last interest parameter:** This is the most recent coupon payment date BEFORE settlement, not the final coupon. The final coupon pays at maturity.

3. **Date order is critical:** Excel requires last_interest < settlement < maturity. Violating this produces #NUM! errors.

4. **Short vs long last periods:** ODDLPRICE handles both automatically based on the dates provided. No need to specify which type.

5. **Compare to PRICE:** When the last period is exactly standard length, ODDLPRICE and PRICE should give similar results. Large differences indicate either an error or a significantly odd period.

6. **For called bonds:** Use the call date as maturity and call price as redemption value. The yield should be yield-to-call.

7. **Combine with ODDLYIELD:** Use ODDLPRICE to find price from yield, or ODDLYIELD to find yield from price. They are inverse functions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PRICE]] | Bond price with regular periods | When last period is standard length |
| [[ODDFPRICE]] | Price with odd first period | When the first period is irregular, not last |
| [[ODDLYIELD]] | Yield with odd last period | When you know price and want yield |
| [[PRICEDISC]] | Discounted security price | For zero-coupon or discount instruments |

### Commonly Used Together

**[[ODDLYIELD]]** - Odd Last Period Yield

*Inverse relationship:*
```
Price to Yield: =ODDLYIELD(settle, mat, last, rate, PRICE, redemption, freq, basis)
Yield to Price: =ODDLPRICE(settle, mat, last, rate, YIELD, redemption, freq, basis)
```
These are inverse functions for odd last period bonds.

---

**[[PRICE]]** - Standard Bond Price

*Comparison and validation:*
```
PRICE (regular): =PRICE(settle, mat, rate, yld, redemption, freq, basis)
ODDLPRICE (odd): =ODDLPRICE(settle, mat, last, rate, yld, redemption, freq, basis)
```
Compare to verify when ODDLPRICE is needed.

---

**[[ACCRINT]]** - Accrued Interest

*Combined for dirty price:*
```
Clean Price: =ODDLPRICE(...)
Accrued: =ACCRINT(last_interest, first_interest, settlement, rate, par, frequency, basis)
Dirty Price: =Clean_Price + Accrued
```
Accrued interest runs from last coupon to settlement.

---

**[[COUPNCD]] / [[COUPPCD]]** - Coupon Dates

*Identify if period is truly odd:*
```
Normal Next Coupon: =COUPNCD(settle, mat, freq, basis)
If Normal_Next != Maturity → may need ODDLPRICE
```
Use coupon date functions to determine if the final period is irregular.

## Official Documentation

- **Microsoft Excel:** [ODDLPRICE function](https://support.microsoft.com/en-us/office/oddlprice-function-fb657749-d200-4902-afaf-ed5445027fc4)
- **Google Sheets:** [ODDLPRICE function](https://support.google.com/docs/answer/3093188)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
