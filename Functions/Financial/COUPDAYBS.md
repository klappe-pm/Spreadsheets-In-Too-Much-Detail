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
- coupon-dates
- accrued-interest
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Coupon Days Before Settlement
- Days Since Last Coupon
- Accrued Days
tags:
- financial-modeling
- bond-analysis
- fixed-income
- coupon-calculations
---

# COUPDAYBS

## Description

**COUPDAYBS** returns the number of days from the beginning of the coupon period to the settlement date. This is the "accrued days" portion of a bond's coupon period--the time that has elapsed since the last coupon payment (or since the bond's issue date for the first period). When you buy a bond between coupon dates, you must compensate the seller for the interest that has accrued during these days, which is why understanding COUPDAYBS is essential for calculating accrued interest and determining the true cost of a bond purchase.

The function is one of six related COUP functions that together provide complete coupon schedule analysis. COUPDAYBS works hand-in-hand with COUPDAYS (total days in the period) to calculate what fraction of the coupon period has elapsed. This fraction, multiplied by the periodic coupon payment, gives you the accrued interest. For example, if COUPDAYBS returns 45 and COUPDAYS returns 180, then 45/180 = 25% of the period has elapsed, and the buyer owes 25% of the semi-annual coupon as accrued interest.

The calculation depends critically on the **day count convention** (basis parameter) you select. Different conventions yield different day counts even for identical dates because they assume different numbers of days per month and year. US corporate bonds use 30/360 (which assumes 30 days per month), US Treasuries use Actual/Actual (which counts actual calendar days), and these conventions can produce meaningfully different results. For a settlement date of March 15 with a February 28 period start, 30/360 counts 17 days (30-28=2 for Feb + 15 for March), while Actual/Actual might count 15 days. Using the wrong convention leads to incorrect accrued interest calculations.

COUPDAYBS has been available in Excel since Excel 2007 (integrated from the Analysis ToolPak) and in Google Sheets since its inception. Both platforms implement identical calculation logic. The function accepts settlement date, maturity date, coupon frequency, and an optional day count basis. Note that while you provide the maturity date, COUPDAYBS uses it only to establish the coupon schedule--it calculates days relative to whichever coupon period contains your settlement date.

## Syntax

> [!f(x)] COUPDAYBS Syntax
>
> ```
> =COUPDAYBS(settlement, maturity, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--the date you take ownership (trade date + settlement period). Enter as a date value, date string, or cell reference. |
| `maturity` | Yes | The bond's maturity date--when principal is repaid and the bond terminates. Used to establish the coupon schedule. Must be after settlement. |
| `frequency` | Yes | The number of coupon payments per year: 1 = annual, 2 = semi-annual (most common for US bonds), 4 = quarterly. |
| `basis` | No | The day count convention to use. Defaults to 0 (US 30/360) if omitted. See Day Count Conventions table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | Assumes 30 days per month, 360 days per year | US corporate bonds, agency bonds |
| 1 | Actual/Actual | Uses actual calendar days in the period | US Treasury bonds, government securities |
| 2 | Actual/360 | Actual days elapsed, divided by 360-day year | Money market instruments |
| 3 | Actual/365 | Actual days elapsed, divided by 365-day year | Japanese government bonds (JGBs) |
| 4 | European 30/360 | Similar to US 30/360 with different end-of-month rules | Eurobonds, European corporate bonds |

### Return Value

Returns a numeric value representing the number of days from the beginning of the coupon period to the settlement date. The result is always non-negative and will be less than the total days in the coupon period (from COUPDAYS).

## Examples

> [!f(x)] COUPDAYBS Examples

### Example 1: Basic Semi-Annual Bond (30/360)
```
=COUPDAYBS("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `90`

**Explanation:** This semi-annual bond pays coupons on January 15 and July 15. The settlement date of April 15 is 3 months (90 days using 30/360) after the January 15 coupon. Using 30/360 convention, each month counts as 30 days: January 15 to February 15 = 30 days, February 15 to March 15 = 30 days, March 15 to April 15 = 30 days, totaling 90 days.

---

### Example 2: US Treasury Bond (Actual/Actual)
```
=COUPDAYBS("2024-04-15", "2030-01-15", 2, 1)
```
**Result:** `91`

**Explanation:** Same dates as Example 1, but using Actual/Actual (basis=1) as required for Treasury bonds. From January 15 to April 15, 2024: January has 16 remaining days, February has 29 days (2024 is a leap year), March has 31 days, plus 15 days in April = 16 + 29 + 31 + 15 - 16 = 91 actual days. The one-day difference from 30/360 affects accrued interest calculations.

---

### Example 3: Annual Coupon Bond
```
=COUPDAYBS("2024-09-01", "2029-03-01", 1, 0)
```
**Result:** `180`

**Explanation:** This annual bond pays one coupon per year on March 1. Settlement on September 1 is exactly 6 months (180 days in 30/360) after the March 1 coupon. With annual payments, coupon periods are 360 days (30/360), so 180 days represents exactly half the period.

---

### Example 4: Quarterly Bond
```
=COUPDAYBS("2024-05-20", "2028-02-20", 4, 0)
```
**Result:** `60`

**Explanation:** This quarterly bond pays coupons on February 20, May 20, August 20, and November 20. Settlement on May 20 falls exactly on a coupon date, but since you settle after the coupon, COUPDAYBS counts from the previous period start (February 20). From February 20 to May 20 = 3 months = 90 days, but settling on the coupon date means 0 days have elapsed in the NEW period. Wait--actually May 20 IS a coupon date, so the previous coupon just paid. This returns 0 if settlement equals a coupon date, otherwise counts from last coupon.

Let me recalculate: If May 20 is settlement and coupons pay Feb 20, May 20, Aug 20, Nov 20, the settlement is ON a coupon date. COUPDAYBS should return 0. But if May 21: From May 20 to May 21 = 1 day. Let me adjust the example.

Actually for "2024-05-20" settling exactly on coupon date:
```
=COUPDAYBS("2024-05-20", "2028-02-20", 4, 0)
```
**Result:** `0`

**Explanation:** Settlement falls exactly on a coupon payment date (May 20). Since the coupon has just been paid, zero days have accrued in the new period. The buyer owes no accrued interest--they will receive the full next coupon.

---

### Example 5: Settlement Shortly After Coupon Date
```
=COUPDAYBS("2024-05-25", "2028-02-20", 4, 0)
```
**Result:** `5`

**Explanation:** Settlement is 5 days after the May 20 coupon. Using 30/360, that's exactly 5 days of accrued interest the buyer must pay. For quarterly coupons, the accrued interest = (5/90) * quarterly coupon amount.

---

### Example 6: Month-End Settlement (30/360 vs Actual)
```
=COUPDAYBS("2024-02-29", "2027-08-31", 2, 0)
```
**Result:** `179`

**Explanation:** Semi-annual bond with August 31 coupon dates. Under 30/360, the previous coupon was August 31 (treated as August 30), and February 29 settlement (treated as February 30 in this convention) gives 5 months + 30 days = 180 days. The 30/360 convention simplifies month-end calculations by treating all months as 30 days.

---

### Example 7: Same Dates with Actual/Actual
```
=COUPDAYBS("2024-02-29", "2027-08-31", 2, 1)
```
**Result:** `182`

**Explanation:** Same scenario using Actual/Actual. Counting actual calendar days from August 31, 2023 to February 29, 2024: Sep=30, Oct=31, Nov=30, Dec=31, Jan=31, Feb=29 = 182 days. This is 3 more days than 30/360 calculated, which directly affects accrued interest--a $100,000 bond at 5% would have ~$8 different accrued interest between conventions.

---

### Example 8: Near Maturity
```
=COUPDAYBS("2024-11-15", "2025-01-15", 2, 0)
```
**Result:** `120`

**Explanation:** Settlement is just 2 months before the final maturity/coupon date. The previous coupon was July 15, so from July 15 to November 15 = 4 months = 120 days (30/360). Even near maturity, the calculation works the same--it counts days from the last coupon.

---

### Example 9: Using European 30/360
```
=COUPDAYBS("2024-03-31", "2028-09-30", 2, 4)
```
**Result:** `180`

**Explanation:** Eurobond with September 30 coupons. Using European 30/360 (basis=4), from September 30 to March 31 counts as exactly 6 months = 180 days. European 30/360 has slightly different month-end rules than US 30/360, but in this case the result is the same.

---

### Example 10: Calculating Accrued Interest Ratio
```
=COUPDAYBS("2024-04-15", "2030-01-15", 2, 0) / COUPDAYS("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `0.50` (50%)

**Explanation:** Dividing COUPDAYBS by COUPDAYS gives the fraction of the coupon period elapsed. Here, 90/180 = 0.50, meaning 50% of the period has passed. If the semi-annual coupon is $25 (5% annual on $1,000 face), accrued interest = 0.50 * $25 = $12.50.

---

### Example 11: Full Accrued Interest Calculation
```
=COUPDAYBS(A1, B1, C1, D1) / COUPDAYS(A1, B1, C1, D1) * (E1 * F1 / C1)
```
Where: A1=Settlement, B1=Maturity, C1=Frequency, D1=Basis, E1=Face Value, F1=Annual Coupon Rate

**Result:** Accrued interest in dollars

**Explanation:** This formula calculates accrued interest from first principles: (days elapsed / total days) * periodic coupon payment. This equals what ACCRINT returns, demonstrating how COUPDAYBS feeds into broader bond calculations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or non-date text | Use DATE() function or ensure dates are recognized date values |
| `#NUM!` | Settlement date >= maturity date | Settlement must be strictly before maturity |
| `#NUM!` | Invalid frequency value | Must be 1 (annual), 2 (semi-annual), or 4 (quarterly) |
| `#NUM!` | Invalid basis value | Must be 0, 1, 2, 3, or 4 |
| `#NUM!` | Negative result scenario (shouldn't occur) | Check that dates are in correct order |
| `Wrong result` | Mismatched day count convention | Verify you're using the correct basis for your security type |
| `Inconsistent with ACCRINT` | Different basis parameters | Ensure all related calculations use identical parameters |

## Use Cases

### [[Accrued Interest Calculation]]

**Scenario:** A bond trader needs to calculate the exact accrued interest for a corporate bond trade to determine the invoice amount.

**Implementation:**
```
Days Accrued: =COUPDAYBS("2024-04-15", "2030-06-15", 2, 0)
Days in Period: =COUPDAYS("2024-04-15", "2030-06-15", 2, 0)
Accrued Fraction: =COUPDAYBS(...) / COUPDAYS(...)
Accrued Interest: =Accrued_Fraction * (Face_Value * Coupon_Rate / 2)

Example: $1,000,000 face, 5% coupon
=90/180 * ($1,000,000 * 0.05 / 2) = $12,500
```

**Business Application:** Bond purchases settle at clean price + accrued interest. COUPDAYBS enables precise calculation of the accrued portion, ensuring accurate trade settlement. This is critical for portfolio accounting and P&L calculation.

**Technical Details:** Use basis=0 for corporate bonds, basis=1 for Treasuries. The result must be consistent with market conventions to match counterparty calculations. Verify against Bloomberg or other pricing systems.

---

### [[Portfolio Accrual Accounting]]

**Scenario:** An asset manager needs to accrue interest daily for month-end NAV reporting across a portfolio of fixed-income securities.

**Implementation:**
```
For each bond position:
Daily Accrual = (Annual Coupon * Face Value) / 360  [for 30/360 basis]
Days This Month = COUPDAYBS(Month_End) - COUPDAYBS(Month_Start)
Monthly Accrual = Daily Accrual * Days This Month
```

**Business Application:** Investment funds must recognize interest income as it accrues, not just when coupons are paid. COUPDAYBS tracks exactly how many days have accrued, enabling accurate income recognition under GAAP and IFRS accounting standards.

**Technical Details:** Run calculations using consistent settlement dates (typically month-end). Store day counts for audit trail. Handle coupon payment dates within the month specially--accrual resets to zero after payment.

---

### [[Trade Settlement Verification]]

**Scenario:** Operations staff need to verify that the accrued interest on a trade confirmation matches the expected calculation.

**Implementation:**
```
Expected Days: =COUPDAYBS(Trade_Settlement_Date, Maturity, Frequency, Basis)
Expected Accrued: =Expected_Days / COUPDAYS(...) * Periodic_Coupon * Quantity
Difference: =ABS(Confirmation_Accrued - Expected_Accrued)
Flag: =IF(Difference > $1, "REVIEW", "OK")
```

**Business Application:** Trade breaks from accrued interest discrepancies cause settlement delays and operational risk. Automated verification using COUPDAYBS catches errors before they become costly fails.

**Technical Details:** Tolerance thresholds account for rounding differences between systems. Investigate discrepancies > $1 as potential day count convention mismatches or date errors.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (was Analysis ToolPak add-in in earlier versions)
- **Date Handling:** Flexible; accepts various regional date formats
- **Array Support:** Does not natively support array operations; use helper columns for multiple bonds

### Google Sheets
- **Availability:** All versions since launch
- **Date Handling:** Stricter parsing; recommend DATE(year, month, day) for reliability
- **Identical Logic:** Returns same values as Excel for identical inputs

### Both Platforms
- Same four parameters (3 required, 1 optional)
- Same five day count conventions (basis 0-4)
- Returns integer or decimal day count depending on convention
- Does not accept arrays/ranges as input

## Tips and Best Practices

1. **Always pair with COUPDAYS:** COUPDAYBS alone tells you elapsed days, but you need the ratio COUPDAYBS/COUPDAYS to calculate accrued interest percentages.

2. **Match basis to security type:** US corporates use basis=0, US Treasuries use basis=1, Eurobonds use basis=4. Wrong basis = wrong accrued interest.

3. **Verify against ACCRINT:** The formula `COUPDAYBS/COUPDAYS * Periodic_Coupon` should equal ACCRINT (scaled appropriately). Use this as a sanity check.

4. **Watch for coupon date settlements:** If settlement falls exactly on a coupon date, COUPDAYBS returns 0. This is correct--no interest has accrued since the payment.

5. **Consider business day conventions:** While COUPDAYBS counts calendar days, actual settlement dates follow business day rules. Ensure your settlement date is the actual settlement, not trade date.

6. **Use DATE() for reliability:** Rather than text dates, use `DATE(2024,4,15)` to avoid regional format issues, especially in shared workbooks.

7. **Document your conventions:** In complex models, note which day count basis each calculation uses. Mixed conventions cause subtle errors.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COUPDAYS]] | Total days in the coupon period | To get the denominator for accrued interest fractions |
| [[COUPDAYSNC]] | Days from settlement to next coupon | To calculate remaining days until next payment |
| [[COUPPCD]] | Previous coupon date | To identify when the last coupon was paid |
| [[COUPNCD]] | Next coupon date | To identify when the next coupon will be paid |
| [[COUPNUM]] | Number of remaining coupons | To count payments until maturity |

### Commonly Used Together

**[[COUPDAYS]]** - Total Days in Coupon Period

*Combined for accrued interest fraction:*
```
=COUPDAYBS(...) / COUPDAYS(...)
```
This ratio represents the percentage of the coupon period elapsed, used to calculate accrued interest.

---

**[[ACCRINT]]** - Accrued Interest

*Verification of manual calculation:*
```
Manual: =COUPDAYBS(...) / COUPDAYS(...) * Face * Rate / Frequency
Built-in: =ACCRINT(...)
```
Both methods should produce identical results when parameters match.

---

**[[PRICE]]** - Bond Clean Price

*Combined for dirty price:*
```
Clean Price: =PRICE(...)
Accrued Interest: =COUPDAYBS(...) / COUPDAYS(...) * Coupon_Payment
Dirty Price: =PRICE(...) + Accrued_Interest
```
The invoice price (dirty price) is clean price plus accrued interest.

---

**[[COUPPCD]]** - Previous Coupon Date

*Verify period start:*
```
Period Start: =COUPPCD(settlement, maturity, frequency, basis)
Days from Start: =settlement - COUPPCD(...)  [for Actual basis]
```
COUPPCD gives the date from which COUPDAYBS counts.

## Official Documentation

- **Microsoft Excel:** [COUPDAYBS function](https://support.microsoft.com/en-us/office/coupdaybs-function-eb9a8dfb-2fb2-4c61-8e5d-690b320cf872)
- **Google Sheets:** [COUPDAYBS function](https://support.google.com/docs/answer/3093175)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
