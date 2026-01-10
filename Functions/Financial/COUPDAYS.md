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
- day-count-conventions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Coupon Days
- Days in Coupon Period
- Coupon Period Length
tags:
- financial-modeling
- bond-analysis
- fixed-income
- coupon-calculations
---

# COUPDAYS

## Description

**COUPDAYS** returns the total number of days in the coupon period that contains the settlement date. This is the denominator you need when calculating accrued interest as a fraction of the coupon payment. Every bond has regular coupon periods--semi-annual bonds have 6-month periods, annual bonds have 12-month periods, quarterly bonds have 3-month periods--but the actual day count in these periods varies depending on the day count convention used. COUPDAYS tells you exactly how many days the market convention assigns to the current period.

Understanding COUPDAYS is essential because the day count convention fundamentally affects bond pricing and accrued interest calculations. Under 30/360 (basis=0), a semi-annual period is always exactly 180 days regardless of which months it spans. Under Actual/Actual (basis=1), the same period might be 181, 182, 183, or 184 days depending on the specific months and whether a leap year is involved. A period from January 15 to July 15 in a leap year contains 182 actual days, while February 15 to August 15 contains 181 days. These differences directly impact accrued interest calculations and must match market conventions.

The function works with COUPDAYBS (days from period start to settlement) to create the accrued interest fraction. The formula `COUPDAYBS / COUPDAYS` gives you the percentage of the coupon period that has elapsed, which when multiplied by the periodic coupon payment yields accrued interest. For example, if COUPDAYBS returns 60 and COUPDAYS returns 180, then 60/180 = 33.33% of the period has elapsed. This ratio is identical to what the ACCRINT function uses internally, but COUPDAYS gives you direct access to the underlying day count.

COUPDAYS has been available in Excel since Excel 2007 (when Analysis ToolPak functions were integrated into the core product) and in Google Sheets since launch. Both platforms return identical results for the same inputs. The function requires settlement date, maturity date, and coupon frequency, with an optional day count basis parameter. While you provide the maturity date, the function uses it only to establish the coupon schedule--it returns the day count for whichever period contains your settlement date, not the final period.

## Syntax

> [!f(x)] COUPDAYS Syntax
>
> ```
> =COUPDAYS(settlement, maturity, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when ownership transfers. Enter as a date value, DATE() formula, or cell reference. |
| `maturity` | Yes | The bond's maturity date--used to establish the coupon payment schedule. Must be after settlement date. |
| `frequency` | Yes | Number of coupon payments per year: 1 = annual (360 days for 30/360), 2 = semi-annual (180 days), 4 = quarterly (90 days). |
| `basis` | No | Day count convention. Defaults to 0 (US 30/360) if omitted. See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Semi-Annual Period Days | Description |
|-------|------------|------------------------|-------------|
| 0 (or omitted) | US (NASD) 30/360 | Always 180 | Each month = 30 days; predictable results |
| 1 | Actual/Actual | 181-184 | Actual calendar days; varies by period |
| 2 | Actual/360 | 181-184 (actual count) | Counts actual days but year basis differs |
| 3 | Actual/365 | 181-184 (actual count) | Counts actual days but year basis differs |
| 4 | European 30/360 | Always 180 | Similar to US 30/360, different month-end rules |

### Return Value

Returns a numeric value representing the total number of days in the coupon period containing the settlement date. For 30/360 conventions, this is always a predictable value (180 for semi-annual, 90 for quarterly, 360 for annual). For Actual conventions, the result varies based on the specific months in the period.

## Examples

> [!f(x)] COUPDAYS Examples

### Example 1: Semi-Annual Bond with 30/360
```
=COUPDAYS("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `180`

**Explanation:** Using 30/360 convention (basis=0), semi-annual coupon periods are always exactly 180 days (6 months x 30 days). This predictability is why 30/360 is popular for corporate bonds--calculations are simpler and results are consistent regardless of which months fall in the period.

---

### Example 2: Semi-Annual Bond with Actual/Actual
```
=COUPDAYS("2024-04-15", "2030-01-15", 2, 1)
```
**Result:** `182`

**Explanation:** This bond pays coupons on January 15 and July 15. The settlement falls in the January 15 - July 15 period. Counting actual days: January has 16 remaining days, February has 29 (2024 is leap year), March has 31, April has 30, May has 31, June has 30, plus 15 days in July = 182 total days. US Treasury bonds use Actual/Actual.

---

### Example 3: Different Period, Same Bond (Actual/Actual)
```
=COUPDAYS("2024-09-15", "2030-01-15", 2, 1)
```
**Result:** `184`

**Explanation:** Now settlement falls in the July 15 - January 15 period. Counting actual days: July has 16 remaining days, August has 31, September has 30, October has 31, November has 30, December has 31, plus 15 days in January = 184 days. This demonstrates why Actual/Actual results vary by period--different months have different day counts.

---

### Example 4: Annual Coupon Bond
```
=COUPDAYS("2024-06-01", "2029-12-01", 1, 0)
```
**Result:** `360`

**Explanation:** Annual coupon bonds have one payment per year. Using 30/360, the period is 12 months x 30 days = 360 days. Annual bonds are common in European markets and for certain types of structured securities.

---

### Example 5: Quarterly Bond
```
=COUPDAYS("2024-04-01", "2028-07-01", 4, 0)
```
**Result:** `90`

**Explanation:** Quarterly bonds pay 4 times per year. Using 30/360, each period is 3 months x 30 days = 90 days. Quarterly payment bonds are less common but exist in certain markets, particularly for floating-rate notes.

---

### Example 6: Comparing 30/360 vs Actual for Same Period
```
30/360:   =COUPDAYS("2024-02-15", "2027-08-15", 2, 0)  → 180
Actual:   =COUPDAYS("2024-02-15", "2027-08-15", 2, 1)  → 182
```
**Result:** `180` vs `182`

**Explanation:** The period from August 15, 2023 to February 15, 2024 contains exactly 180 days under 30/360 (6 x 30), but 184 actual calendar days. This 4-day difference affects accrued interest: if COUPDAYBS shows 90 days elapsed, the accrued fraction is 90/180 = 50% under 30/360, but 90/184 = 48.9% under Actual. For a $25 semi-annual coupon, that is $12.50 vs $12.23 accrued interest--a meaningful difference at scale.

---

### Example 7: Leap Year Impact
```
Non-leap:  =COUPDAYS("2023-04-15", "2030-01-15", 2, 1)  → 181
Leap year: =COUPDAYS("2024-04-15", "2030-01-15", 2, 1)  → 182
```
**Result:** `181` vs `182`

**Explanation:** The January-July period in a non-leap year (2023) has 181 days (28 days in February). In a leap year (2024), the same period has 182 days (29 days in February). This is why Actual/Actual is called "actual"--it reflects real calendar days including leap years.

---

### Example 8: European 30/360
```
=COUPDAYS("2024-03-31", "2028-09-30", 2, 4)
```
**Result:** `180`

**Explanation:** European 30/360 (basis=4), like US 30/360, always returns 180 for semi-annual periods. The difference between US and European 30/360 lies in how they handle month-end dates, but the period length is the same.

---

### Example 9: Actual/360 Convention
```
=COUPDAYS("2024-04-15", "2030-01-15", 2, 2)
```
**Result:** `182`

**Explanation:** Actual/360 (basis=2) counts actual calendar days for the period length, returning 182 days for this January-July period. The "/360" refers to annualization, not the period count. This convention is used for some money market instruments.

---

### Example 10: Actual/365 Convention
```
=COUPDAYS("2024-04-15", "2030-01-15", 2, 3)
```
**Result:** `182`

**Explanation:** Actual/365 (basis=3) also counts actual calendar days, returning 182 days. Like Actual/360, the denominator (365) refers to annualization for yield calculations, not the period day count. Used for Japanese government bonds.

---

### Example 11: Accrued Interest Calculation
```
Accrued Fraction: =COUPDAYBS("2024-04-15", "2030-01-15", 2, 0) / COUPDAYS("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `0.50` (90/180)

**Explanation:** Combining COUPDAYBS and COUPDAYS yields the accrued interest fraction. Here, 90 days have elapsed out of 180 total = 50% of the period. Multiply by the semi-annual coupon to get accrued interest: 0.50 x $25 = $12.50 per $1,000 face value.

---

### Example 12: Verifying Period Consistency
```
=COUPDAYBS("2024-04-15", "2030-01-15", 2, 0) + COUPDAYSNC("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `180` (equals COUPDAYS)

**Explanation:** COUPDAYBS (days elapsed) plus COUPDAYSNC (days remaining) always equals COUPDAYS (total days). This identity is useful for verifying calculations: 90 + 90 = 180.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or text in date parameters | Use DATE() function or ensure dates are valid |
| `#NUM!` | Settlement date >= maturity date | Settlement must be strictly before maturity |
| `#NUM!` | Invalid frequency value | Must be 1, 2, or 4 only |
| `#NUM!` | Invalid basis value | Must be 0, 1, 2, 3, or 4 |
| `Unexpected value` | Using wrong basis for security type | Match basis to market convention for your security |
| `Inconsistent results` | Comparing bonds with different day count conventions | Ensure consistent basis across related calculations |

## Use Cases

### [[Accrued Interest Denominator]]

**Scenario:** A fixed-income trader needs to calculate accrued interest for a corporate bond trade using proper day count conventions.

**Implementation:**
```
Period Days: =COUPDAYS(Settlement, Maturity, 2, 0)
Elapsed Days: =COUPDAYBS(Settlement, Maturity, 2, 0)
Accrued Fraction: =Elapsed_Days / Period_Days
Accrued Interest: =Face_Value * Coupon_Rate / 2 * Accrued_Fraction

Example with $1,000,000 face, 5% coupon, 90 days elapsed:
=90 / 180 * ($1,000,000 * 0.05 / 2) = $12,500
```

**Business Application:** The COUPDAYS value is critical for calculating the exact accrued interest a buyer must pay. Using the wrong denominator leads to incorrect settlement amounts, causing trade breaks and operational issues.

**Technical Details:** Corporate bonds use 30/360 (basis=0), so COUPDAYS always returns 180 for semi-annual bonds. This simplifies calculations but may differ from actual calendar days.

---

### [[Treasury Bond Pricing]]

**Scenario:** A portfolio manager needs to price US Treasury bonds using the correct Actual/Actual convention.

**Implementation:**
```
Total Period Days: =COUPDAYS(Settlement, Maturity, 2, 1)
Clean Price: =PRICE(Settlement, Maturity, Coupon, Yield, 100, 2, 1)
Accrued: =COUPDAYBS(...) / COUPDAYS(...) * Coupon/2 * 100
Dirty Price: =Clean_Price + Accrued
```

**Business Application:** Treasury bonds require Actual/Actual day counting. COUPDAYS returns the actual number of days in the period (181-184), which is essential for matching official Treasury pricing and accrued interest calculations published by FINRA.

**Technical Details:** Treasury prices are quoted in 32nds. The accrued interest calculation must use Actual/Actual to match Bloomberg, FINRA TRACE, and other industry sources.

---

### [[Day Count Convention Analysis]]

**Scenario:** A financial analyst needs to demonstrate the impact of different day count conventions on accrued interest for a client presentation.

**Implementation:**
```
Create comparison table:
Convention    Period Days    90 Days Elapsed    Accrued on $1M @ 5%
30/360:       =COUPDAYS(...,0)  → 180    90/180 = 50.00%    $12,500.00
Actual/Actual: =COUPDAYS(...,1) → 182    90/182 = 49.45%    $12,362.64
Actual/360:   =COUPDAYS(...,2)  → 182    90/182 = 49.45%    $12,362.64
```

**Business Application:** Different day count conventions produce different accrued interest amounts. This analysis helps clients understand why bond pricing varies by security type and market convention.

**Technical Details:** The difference between conventions grows larger for longer elapsed periods and higher coupon rates. A $100 million portfolio could see thousands of dollars difference in accrued interest calculations.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Date Parsing:** Flexible; accepts regional date formats
- **Consistency:** Results identical across Excel versions

### Google Sheets
- **Availability:** All versions since launch
- **Date Parsing:** Stricter; prefer DATE() function for reliability
- **Identical Results:** Returns same values as Excel for identical inputs

### Both Platforms
- Same four parameters (3 required, 1 optional)
- Same five day count conventions
- Returns whole numbers for 30/360 bases; may return decimals for Actual bases in edge cases
- No array formula support--calculate each bond individually

## Tips and Best Practices

1. **Know your security type:** Corporate bonds use 30/360 (always 180 days semi-annually), Treasuries use Actual/Actual (varies), Eurobonds use European 30/360. Always match the convention to your security.

2. **Use COUPDAYS with COUPDAYBS:** The ratio COUPDAYBS/COUPDAYS gives you the accrued fraction. This is the building block for accrued interest calculations.

3. **Verify with identity:** COUPDAYBS + COUPDAYSNC should equal COUPDAYS. If not, check your inputs.

4. **30/360 is simpler but less precise:** If you need maximum precision (e.g., for Treasuries), use Actual/Actual. If you need simplicity and speed, 30/360 is appropriate for corporate bonds.

5. **Watch for leap years:** Actual/Actual results change in leap years. February 29 adds one day to any period that includes February.

6. **Cross-check with ACCRINT:** The built-in ACCRINT function uses COUPDAYS internally. Your manual calculation should match ACCRINT when using the same parameters.

7. **Document basis consistently:** In models with multiple bonds, ensure all calculations use the correct basis for each security type. A common error is applying corporate basis to Treasury holdings.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COUPDAYBS]] | Days from period start to settlement | To get the numerator for accrued interest fraction |
| [[COUPDAYSNC]] | Days from settlement to next coupon | To calculate remaining days until payment |
| [[COUPPCD]] | Previous coupon date | To find the start of the current period |
| [[COUPNCD]] | Next coupon date | To find the end of the current period |
| [[COUPNUM]] | Number of remaining coupons | To count payments until maturity |

### Commonly Used Together

**[[COUPDAYBS]]** - Days Since Last Coupon

*Combined for accrued interest fraction:*
```
Accrued Fraction: =COUPDAYBS(settle, mat, freq, basis) / COUPDAYS(settle, mat, freq, basis)
```
This ratio is the percentage of coupon earned but not yet paid.

---

**[[ACCRINT]]** - Accrued Interest

*Verification of built-in calculation:*
```
Manual: =(COUPDAYBS(...) / COUPDAYS(...)) * Face * Rate / Frequency
Built-in: =ACCRINT(...)
```
Both should return identical results when using the same day count basis.

---

**[[PRICE]]** - Bond Clean Price

*Combined for complete bond valuation:*
```
Clean Price: =PRICE(..., basis)
Period Days: =COUPDAYS(..., basis)
Accrued: =COUPDAYBS(...) / COUPDAYS(...) * Coupon_Payment
Invoice Price: =PRICE(...) + Accrued
```
The dirty price (invoice price) is what you actually pay.

---

**[[YEARFRAC]]** - Fraction of Year

*Alternative approach for time calculations:*
```
YEARFRAC-based: =YEARFRAC(COUPPCD(...), settlement, basis)
COUPDAYS-based: =COUPDAYBS(...) / COUPDAYS(...)
```
Both methods calculate elapsed time fraction using the same day count conventions.

## Official Documentation

- **Microsoft Excel:** [COUPDAYS function](https://support.microsoft.com/en-us/office/coupdays-function-cc64380b-315b-4e8b-9f7d-6aea0b2bc4ff)
- **Google Sheets:** [COUPDAYS function](https://support.google.com/docs/answer/3093176)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
