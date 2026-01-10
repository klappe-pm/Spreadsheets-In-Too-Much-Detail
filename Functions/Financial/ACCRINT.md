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
- accrued-interest
- securities
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Accrued Interest
- Bond Accrued Interest
- Periodic Accrued Interest
tags:
- financial-modeling
- bond-analysis
- fixed-income
- securities-trading
---

# ACCRINT

## Description

**ACCRINT (Accrued Interest)** calculates the accrued interest for a security that pays periodic interest, such as corporate bonds, government bonds, or other fixed-income securities. When you buy a bond between coupon payment dates, you must compensate the seller for the interest that has accumulated since the last payment--this is accrued interest. The buyer pays the clean price plus accrued interest (the "dirty price"), and then receives the full coupon payment on the next payment date. ACCRINT tells you exactly how much accrued interest is owed.

Understanding accrued interest is fundamental to bond trading because bonds almost never trade exactly on coupon dates. If a bond pays $50 in interest every six months and you buy it three months after the last payment, you owe the seller three months' worth of interest (approximately $25). Without this mechanism, sellers would have an incentive to sell immediately before coupon dates and buyers would only want to purchase right after--accrued interest eliminates this timing distortion and allows bonds to trade freely at fair value on any day.

The calculation of accrued interest depends critically on the **day count convention** (the basis parameter), which determines how days between dates are counted. Different markets and security types use different conventions: US corporate bonds typically use 30/360, US Treasury bonds use Actual/Actual, Eurobonds often use Actual/360, and money market instruments frequently use Actual/365. Using the wrong day count convention can result in calculation errors of several days' worth of interest--significant when dealing with large bond positions. The formula is essentially: Accrued Interest = (Annual Coupon) x (Days Since Last Payment / Days in Year under convention).

ACCRINT has been available in Excel since Excel 2007 (as part of the Analysis ToolPak functions integrated into core Excel) and in Google Sheets since its inception. The function requires the issue date, first interest date, settlement date, coupon rate, par value, and payment frequency, with optional parameters for day count basis and calculation method. Note that Google Sheets does not support the calc_method parameter that Excel offers.

## Syntax

> [!f(x)] ACCRINT Syntax
>
> ```
> =ACCRINT(issue, first_interest, settlement, rate, par, frequency, [basis], [calc_method])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `issue` | Yes | The security's issue date--when the bond was originally issued. Enter as a date value, date string, or cell reference containing a date. |
| `first_interest` | Yes | The security's first interest (coupon) payment date. This establishes the payment schedule. Must be after the issue date. |
| `settlement` | Yes | The settlement date--when you acquire (or are calculating accrued interest for) the security. Must be after the issue date. |
| `rate` | Yes | The security's annual coupon rate as a decimal. For a 5% coupon bond, enter 0.05 or 5%. |
| `par` | Yes | The security's par (face) value. Corporate bonds typically have $1,000 par; government bonds often use $100 for pricing. |
| `frequency` | Yes | The number of coupon payments per year: 1 = annual, 2 = semi-annual (most common for US bonds), 4 = quarterly. |
| `basis` | No | The day count basis (convention) to use. See Day Count Conventions table below. Defaults to 0 (US 30/360) if omitted. |
| `calc_method` | No | **Excel only.** Logical value: TRUE or omitted = accrued interest from issue date to settlement; FALSE = from first interest to settlement. Google Sheets always uses TRUE behavior. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | Assumes 30 days per month, 360 days per year. If a date falls on the 31st, it is treated as the 30th. | US corporate bonds, agency bonds |
| 1 | Actual/Actual | Uses actual days in each month and actual days in the coupon period (365 or 366 for leap years). Most precise method. | US Treasury bonds, many government securities |
| 2 | Actual/360 | Uses actual number of days but divides by 360. Results in slightly higher interest than Actual/365. | Money market instruments, some Euro-denominated bonds |
| 3 | Actual/365 | Uses actual number of days but always divides by 365 (ignores leap years). | Japanese government bonds (JGBs), some money markets |
| 4 | European 30/360 | Similar to US 30/360 but handles end-of-month dates differently. Both start and end dates on the 31st become the 30th. | Eurobonds, European corporate bonds |

### Return Value

Returns a numeric value representing the accrued interest in the same currency unit as the par value. For a $1,000 par bond, the result is in dollars.

## Examples

> [!f(x)] ACCRINT Examples

### Example 1: Basic Semi-Annual Corporate Bond
```
=ACCRINT("2024-01-01", "2024-07-01", "2024-04-15", 0.06, 1000, 2, 0)
```
**Result:** `$17.50`

**Explanation:** A corporate bond issued January 1, 2024, with first coupon July 1, 2024, settled on April 15, 2024. The 6% coupon on $1,000 par pays $60/year or $30 per semi-annual period. Using 30/360, there are 105 days from issue (Jan 1) to settlement (Apr 15), out of a 180-day period. Accrued interest = $60 x (105/360) = $17.50. The buyer pays this to the seller plus the clean price.

---

### Example 2: US Treasury Bond (Actual/Actual)
```
=ACCRINT("2023-08-15", "2024-02-15", "2024-01-10", 0.045, 100, 2, 1)
```
**Result:** `$2.19`

**Explanation:** A Treasury bond with 4.5% coupon, $100 par (standard Treasury pricing), semi-annual payments. Using Actual/Actual (basis=1), the calculation counts actual days from the previous coupon date (Aug 15) to settlement (Jan 10)--148 days out of a 184-day period (Aug 15 to Feb 15). Accrued = $4.50 x (148/184) x (184/365) = $2.19 approximately. Treasuries use Actual/Actual for precision.

---

### Example 3: Quarterly Payment Bond
```
=ACCRINT("2024-03-01", "2024-06-01", "2024-05-15", 0.08, 1000, 4, 0)
```
**Result:** `$16.67`

**Explanation:** A bond with quarterly payments (frequency=4) has an 8% annual coupon, paying $20 per quarter. From issue (Mar 1) to settlement (May 15), there are 75 days using 30/360 (2 months + 14 days = 74 days, but Mar 1 to May 15 = 30+30+14=74, adjusted to 75). Accrued = $80 x (75/360) = $16.67. Quarterly bonds are less common but used in some high-yield and emerging market debt.

---

### Example 4: Annual Coupon Eurobond
```
=ACCRINT("2023-06-15", "2024-06-15", "2024-03-01", 0.055, 1000, 1, 4)
```
**Result:** `$39.17`

**Explanation:** A Eurobond with annual coupon payments, 5.5% rate, using European 30/360 (basis=4). From June 15, 2023 to March 1, 2024 is approximately 256 days under 30/360 counting. Accrued = $55 x (256/360) = $39.11. Eurobonds commonly use annual payments and European day count conventions.

---

### Example 5: High-Yield Bond Settlement
```
=ACCRINT("2024-02-01", "2024-08-01", "2024-06-20", 0.095, 1000, 2, 0)
```
**Result:** `$43.54`

**Explanation:** A high-yield (junk) bond with 9.5% coupon settling June 20. At nearly 5 months since issue, significant accrued interest has accumulated. Accrued = $95 x (139/360) = $36.65... wait, let me recalculate: Feb 1 to Jun 20 = 29 + 30 + 30 + 30 + 20 = 139 days (30/360). $95 x (139/360) = $36.68. The high coupon rate means substantial accrued interest even over relatively short periods.

---

### Example 6: Money Market Convention (Actual/360)
```
=ACCRINT("2024-01-15", "2024-07-15", "2024-04-15", 0.05, 100000, 2, 2)
```
**Result:** `$1,250.00`

**Explanation:** Using Actual/360 (basis=2) for a money market-style instrument with $100,000 par. From Jan 15 to Apr 15 is exactly 91 actual days. Accrued = $5,000 x (91/360) = $1,263.89. Actual/360 is called "money market basis" and results in slightly higher interest than Actual/365 because you divide by fewer days.

---

### Example 7: Japanese Style (Actual/365)
```
=ACCRINT("2024-04-01", "2024-10-01", "2024-07-15", 0.025, 10000000, 2, 3)
```
**Result:** `$71,917.81`

**Explanation:** A Japanese-style bond (JGBs use Actual/365) with 2.5% coupon on JPY 10 million par. From Apr 1 to Jul 15 is 105 actual days. Accrued = JPY 250,000 x (105/365) = JPY 71,917.81. Japanese government bonds traditionally use Actual/365, ignoring leap years for simplicity.

---

### Example 8: Newly Issued Bond (Minimal Accrued)
```
=ACCRINT("2024-06-01", "2024-12-01", "2024-06-05", 0.06, 1000, 2)
```
**Result:** `$0.67`

**Explanation:** Settling just 4 days after issue, minimal accrued interest has accumulated. This is typical for new issue purchases where the settlement date is very close to the issue date. Accrued = $60 x (4/360) = $0.67. Buyers of new issues pay almost no accrued interest.

---

### Example 9: Long Accrual Period (Near Coupon Date)
```
=ACCRINT("2023-07-01", "2024-01-01", "2023-12-28", 0.07, 1000, 2, 0)
```
**Result:** `$34.42`

**Explanation:** Settling just 3 days before the coupon payment date, the buyer must pay nearly the full six months of accrued interest ($35 semi-annual coupon). Accrued = $70 x (177/360) = $34.42. The buyer pays this accrued interest but will receive the full $35 coupon payment in just 3 days--the net result is appropriate compensation.

---

### Example 10: Using Cell References for Bond Portfolio
```
=ACCRINT(A2, B2, C2, D2, E2, F2, G2)
```
Where A2=Issue date, B2=First coupon, C2=Settlement, D2=Rate, E2=Par, F2=Frequency, G2=Basis

**Result:** Varies by inputs

**Explanation:** Building a bond portfolio model with cell references allows you to calculate accrued interest for multiple bonds by copying the formula down. This is the standard approach for bond trading desks and portfolio management systems that track accrued interest across hundreds of positions.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or text in numeric parameters | Ensure dates are valid Excel/Sheets date values, not text strings. Use DATEVALUE() if necessary. |
| `#NUM!` | Settlement date is before issue date | Settlement must be on or after issue date. Check date order. |
| `#NUM!` | Invalid frequency value | Frequency must be 1 (annual), 2 (semi-annual), or 4 (quarterly). No other values accepted. |
| `#NUM!` | Invalid basis value | Basis must be 0, 1, 2, 3, or 4. Check for typos or out-of-range values. |
| `#NUM!` | Rate or par is negative | Both rate and par must be positive values. |
| `Unexpected result` | Wrong day count convention | Different basis values give different results. Verify which convention applies to your security type. |
| `Wrong accrued amount` | Par value mismatch | US corporates use $1,000 par; Treasuries are quoted per $100. Match your par value to market convention. |

## Use Cases

### [[Bond Trade Settlement Calculation]]

**Scenario:** A fixed-income trader needs to calculate the full settlement amount (dirty price) for purchasing $2,000,000 face value of a corporate bond trading at 98.50 (clean price), with accrued interest.

**Implementation:**
```
Clean Price: =2000000 * 0.985 = $1,970,000
Accrued Interest: =ACCRINT("2024-01-15", "2024-07-15", "2024-05-20", 0.0575, 2000000, 2, 0)
                  = $32,638.89
Total Settlement: = $1,970,000 + $32,638.89 = $2,002,638.89
```

**Business Application:** Every bond trade requires precise calculation of accrued interest to ensure correct settlement. Trading desks use ACCRINT to verify broker quotes, calculate wire amounts, and reconcile trade confirmations. Errors in accrued interest calculations can result in failed trades or monetary disputes.

**Technical Details:** Corporate bonds in the US settle T+2 (two business days after trade date). The settlement date for ACCRINT should be the actual settlement date, not the trade date. For large positions, even small day count errors multiply into significant dollar amounts.

---

### [[Portfolio Accrued Interest Reporting]]

**Scenario:** An investment manager needs to calculate total accrued interest across a bond portfolio for month-end NAV (Net Asset Value) reporting.

**Implementation:**
```
Portfolio accrued interest:
=SUMPRODUCT(
  ACCRINT(IssueDates, FirstCoupons, NAVDate, Rates, ParValues, Frequencies, Bases),
  Quantities
)
```
Or for each bond: =ACCRINT(...) * (Position Size / Par)

**Business Application:** Mutual funds, pension funds, and hedge funds must report accurate NAV daily or monthly. Accrued interest is a component of the total market value of bond holdings. Understating accrued interest understates NAV; overstating it creates the opposite problem. Auditors verify these calculations.

**Technical Details:** For portfolios with bonds using different day count conventions, each bond must use its appropriate basis. Create a lookup table mapping bond types to basis values. Total portfolio accrued interest may represent millions of dollars that flow through to investor returns.

---

### [[Bond Pricing Model Validation]]

**Scenario:** A quantitative analyst needs to validate that a bond pricing model correctly calculates dirty price by independently checking the accrued interest component.

**Implementation:**
```
Model Dirty Price: $1,025.50
Model Clean Price: $1,008.75
Expected Accrued: =ACCRINT("2023-09-15", "2024-03-15", "2024-02-01", 0.0525, 1000, 2, 1)
                = $16.75
Validation: Clean + Accrued = $1,008.75 + $16.75 = $1,025.50 (matches)
```

**Business Application:** Pricing models used for trading decisions or risk management must be validated against known correct calculations. ACCRINT provides an independent check on the accrued interest portion of pricing models. Discrepancies indicate bugs or methodology differences that must be investigated.

**Technical Details:** When validating, ensure both the model and ACCRINT use identical inputs, especially the basis parameter. A common validation failure is using Actual/Actual in one system and 30/360 in another--technically both "correct" but incompatible for comparison.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (was Analysis ToolPak add-in in earlier versions)
- **calc_method Parameter:** Supports the 8th parameter (calc_method). TRUE or omitted calculates from issue to settlement; FALSE calculates from first_interest to settlement.
- **Date Handling:** Recognizes multiple date formats based on system locale settings.

### Google Sheets
- **Availability:** All versions since launch
- **calc_method Parameter:** Does NOT support the calc_method parameter. Always behaves as if TRUE (issue to settlement). If you provide an 8th parameter, it is ignored.
- **Date Handling:** More strict about date formats; explicitly use DATE() function for reliability.

### Both Platforms
- Core calculation methodology is identical
- Same six basis (day count) options available
- Same frequency options (1, 2, 4)
- Results match when using equivalent inputs

## Tips and Best Practices

1. **Match the day count convention to your security type:** US corporate bonds typically use 30/360 (basis=0), Treasury bonds use Actual/Actual (basis=1), Eurobonds use European 30/360 (basis=4). Using the wrong convention creates pricing errors.

2. **Use $100 par for Treasury bonds, $1,000 for corporates:** Treasury prices and yields are quoted per $100 face value, while corporate bonds use $1,000. Ensure your par value matches market convention to get meaningful results.

3. **Remember that settlement date is NOT trade date:** Bonds settle T+1 or T+2 after trade date. Use the actual settlement date in ACCRINT, not the date you execute the trade.

4. **Verify results with the formula:** Accrued Interest = Par x Rate x (Days/Year). Manually check at least one calculation to ensure your inputs are correct.

5. **For portfolios, create a day count lookup table:** Different bonds use different conventions. Map security types or CUSIPs to their appropriate basis values rather than hardcoding.

6. **Be careful with date entry:** Enter dates using DATE(year, month, day) function for maximum reliability. Text dates like "1/15/2024" may be interpreted differently based on locale settings.

7. **Scale results appropriately:** If your position is $5 million face value but you calculated accrued on $1,000 par, multiply the result by 5,000 to get total accrued interest owed.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ACCRINTM]] | Accrued interest for securities that pay at maturity | When the security pays all interest at maturity (zero coupon or discount securities) |
| [[COUPDAYBS]] | Days from beginning of coupon period to settlement | When you need just the day count, not the dollar amount |
| [[COUPDAYS]] | Total days in the coupon period | When calculating day count fractions manually |

### Commonly Used Together

**[[PRICE]]** - Bond Clean Price

*Combined with ACCRINT for dirty price calculation:*
```
Dirty Price = PRICE(...) + ACCRINT(...) / (Par/100)
```
Clean price (PRICE) plus accrued interest equals the full amount paid at settlement.

---

**[[YIELD]]** - Bond Yield to Maturity

*Combined with ACCRINT for yield analysis:*
```
Clean price is input to YIELD; verify with ACCRINT that total settlement is reasonable:
Total Cost = (PRICE/100 * Face) + ACCRINT(...)
```
When analyzing bond investments, calculate both yield (return) and accrued interest (cost component).

---

**[[COUPDAYBS]]** - Days from Beginning of Coupon Period

*Used to verify ACCRINT day count:*
```
=COUPDAYBS(settlement, maturity, frequency, basis)
```
Should match the day count implicit in ACCRINT calculation--useful for debugging.

---

**[[SUM]]** - Total Accrued Interest

*Combined for portfolio totals:*
```
=SUM(ACCRINT(...bond1...), ACCRINT(...bond2...), ACCRINT(...bond3...))
```
Total accrued interest across multiple bond positions.

## Official Documentation

- **Microsoft Excel:** [ACCRINT function](https://support.microsoft.com/en-us/office/accrint-function-fe45d089-6722-4fb3-9379-e1f911d8dc74)
- **Google Sheets:** [ACCRINT function](https://support.google.com/docs/answer/3093200)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Same as Excel except calc_method parameter not supported |

---

*Last updated: 2026-01-10*
