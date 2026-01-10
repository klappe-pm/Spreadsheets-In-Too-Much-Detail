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
- cash-flow-timing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Coupon Days Next Coupon
- Days to Next Coupon
- Days Until Payment
tags:
- financial-modeling
- bond-analysis
- fixed-income
- coupon-calculations
---

# COUPDAYSNC

## Description

**COUPDAYSNC** returns the number of days from the settlement date to the next coupon date. This is the complement of COUPDAYBS--while COUPDAYBS tells you how many days have elapsed since the last coupon, COUPDAYSNC tells you how many days remain until the next coupon payment. For bond investors, this answers a critical question: "How long until I receive my next interest payment?" The function is essential for cash flow timing, reinvestment planning, and understanding a bond's accrual position.

The function follows a simple identity: COUPDAYBS + COUPDAYSNC = COUPDAYS. The total days in the coupon period equal the days elapsed plus the days remaining. This relationship is useful for verification--if your calculations do not satisfy this identity, something is wrong with your inputs. For example, in a 180-day period (30/360 semi-annual), if 60 days have elapsed (COUPDAYBS = 60), then 120 days remain (COUPDAYSNC = 120). This mathematical consistency ensures accurate bond analytics.

Like all COUP functions, COUPDAYSNC respects the day count convention specified in the basis parameter. For 30/360 conventions, results are predictable and sum neatly; for Actual conventions, the results depend on the specific calendar days in the period. A bond purchased on February 15 with a June 15 coupon might have 120 days remaining under 30/360 (4 months x 30 days) but 121 days remaining under Actual/Actual (counting actual calendar days through a 31-day March). Using the correct convention for your security type is essential.

COUPDAYSNC has been available in Excel since Excel 2007 and in Google Sheets since launch. The function requires settlement date, maturity date, and payment frequency, with an optional day count basis. The maturity date establishes the coupon schedule, and the function returns the days remaining in whichever coupon period contains the settlement date. If settlement falls exactly on a coupon date, COUPDAYSNC returns the full period length (e.g., 180 for semi-annual 30/360), representing the days until the next coupon after the one just received.

## Syntax

> [!f(x)] COUPDAYSNC Syntax
>
> ```
> =COUPDAYSNC(settlement, maturity, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when you take ownership. Enter as date value, DATE() formula, or cell reference. |
| `maturity` | Yes | The bond's maturity date--used to establish the coupon payment schedule. Must be after settlement. |
| `frequency` | Yes | Number of coupon payments per year: 1 = annual, 2 = semi-annual, 4 = quarterly. |
| `basis` | No | Day count convention. Defaults to 0 (US 30/360) if omitted. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Result Characteristics |
|-------|------------|-------------|----------------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days per month, 360 per year | Predictable, clean numbers |
| 1 | Actual/Actual | Actual calendar days | Varies by specific dates |
| 2 | Actual/360 | Actual days, 360-day year | Variable period days |
| 3 | Actual/365 | Actual days, 365-day year | Variable period days |
| 4 | European 30/360 | European month-end rules | Predictable, clean numbers |

### Return Value

Returns a numeric value representing the number of days from settlement to the next coupon payment date. The result is always positive and represents the remaining portion of the current coupon period. If settlement is exactly on a coupon date, returns the full length of the next period.

## Examples

> [!f(x)] COUPDAYSNC Examples

### Example 1: Basic Semi-Annual Bond (30/360)
```
=COUPDAYSNC("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `90`

**Explanation:** This semi-annual bond pays coupons on January 15 and July 15. Settlement on April 15 is 3 months (90 days) before the July 15 coupon. Under 30/360, April 15 to July 15 = 3 months x 30 days = 90 days remaining until the next coupon payment.

---

### Example 2: US Treasury Bond (Actual/Actual)
```
=COUPDAYSNC("2024-04-15", "2030-01-15", 2, 1)
```
**Result:** `91`

**Explanation:** Using Actual/Actual for Treasury bonds. From April 15 to July 15, 2024: remaining April days (15), May (31), June (30), July 1-15 (15) = 15 + 31 + 30 + 15 = 91 actual days. The one extra day compared to 30/360 reflects the 31-day May.

---

### Example 3: Settlement Exactly on Coupon Date
```
=COUPDAYSNC("2024-01-15", "2030-01-15", 2, 0)
```
**Result:** `180`

**Explanation:** Settlement falls exactly on a coupon payment date (January 15). The investor receives (or the seller retains) the coupon being paid, and COUPDAYSNC returns the full 180 days until the NEXT coupon (July 15). The next period starts fresh.

---

### Example 4: One Day After Coupon
```
=COUPDAYSNC("2024-01-16", "2030-01-15", 2, 0)
```
**Result:** `179`

**Explanation:** Settlement is one day after the January 15 coupon. Under 30/360, there are 179 days remaining until the July 15 coupon (180 - 1 = 179). One day has accrued; 179 remain.

---

### Example 5: One Day Before Coupon
```
=COUPDAYSNC("2024-01-14", "2030-01-15", 2, 0)
```
**Result:** `1`

**Explanation:** Settlement is just one day before the coupon date. Only 1 day remains until the January 15 payment. The buyer will receive the coupon but must pay 179 days of accrued interest to the seller.

---

### Example 6: Annual Coupon Bond
```
=COUPDAYSNC("2024-06-01", "2029-12-01", 1, 0)
```
**Result:** `180`

**Explanation:** This annual bond pays coupons on December 1. Settlement on June 1 is exactly 6 months before the December coupon. Under 30/360, that is 180 days remaining (6 months x 30 days). Annual periods are 360 days total.

---

### Example 7: Quarterly Bond
```
=COUPDAYSNC("2024-04-01", "2028-07-01", 4, 0)
```
**Result:** `90`

**Explanation:** This quarterly bond pays on January 1, April 1, July 1, and October 1. Settlement on April 1 falls on a coupon date, so COUPDAYSNC returns the full quarterly period of 90 days until July 1.

---

### Example 8: Verifying the Identity Relationship
```
COUPDAYBS: =COUPDAYBS("2024-04-15", "2030-01-15", 2, 0)  → 90
COUPDAYSNC: =COUPDAYSNC("2024-04-15", "2030-01-15", 2, 0) → 90
COUPDAYS: =COUPDAYS("2024-04-15", "2030-01-15", 2, 0)    → 180
Verify: 90 + 90 = 180 ✓
```
**Result:** Identity confirmed

**Explanation:** The fundamental identity holds: days elapsed (90) + days remaining (90) = total days in period (180). This relationship is a useful sanity check for your calculations.

---

### Example 9: Leap Year Impact (Actual/Actual)
```
=COUPDAYSNC("2024-02-15", "2030-08-15", 2, 1)
```
**Result:** `181`

**Explanation:** From February 15 to August 15, 2024 (leap year): remaining Feb (14), Mar (31), Apr (30), May (31), Jun (30), Jul (31), Aug 1-15 (15) = 14 + 31 + 30 + 31 + 30 + 31 + 15 - 1 = 181 days. Leap year February affects the count.

---

### Example 10: Near Maturity
```
=COUPDAYSNC("2024-12-01", "2025-01-15", 2, 0)
```
**Result:** `44`

**Explanation:** Settlement is 44 days (30/360) before the final maturity and coupon on January 15, 2025. From December 1 to January 15 = 1 month + 14 days = 30 + 14 = 44 days remaining until the final payment.

---

### Example 11: Cash Flow Timing Analysis
```
Next Coupon Date: =COUPNCD("2024-04-15", "2030-01-15", 2, 0)
Days Until Payment: =COUPDAYSNC("2024-04-15", "2030-01-15", 2, 0)
Actual Date Calculation: =Settlement + COUPDAYSNC(...)
```
**Result:** Schedule analysis for reinvestment planning

**Explanation:** COUPDAYSNC provides the day count, COUPNCD provides the actual date. Together they enable precise cash flow timing for portfolio management and reinvestment strategies.

---

### Example 12: Discount Factor Calculation
```
Days to Next Coupon: =COUPDAYSNC("2024-04-15", "2030-01-15", 2, 0)
Days in Period: =COUPDAYS("2024-04-15", "2030-01-15", 2, 0)
Fractional Period: =COUPDAYSNC(...) / COUPDAYS(...)
```
**Result:** `0.50` (90/180)

**Explanation:** The ratio COUPDAYSNC/COUPDAYS gives the fractional period remaining until the next coupon. This is used in bond pricing formulas to discount the first cash flow appropriately. Here, 50% of the period remains.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or non-date values | Use DATE() function or verify date cell formats |
| `#NUM!` | Settlement >= maturity | Settlement must be strictly before maturity date |
| `#NUM!` | Invalid frequency | Must be 1 (annual), 2 (semi-annual), or 4 (quarterly) |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4 |
| `Unexpected result` | Wrong day count convention | Match basis to your security type |
| `Identity failure` | COUPDAYBS + COUPDAYSNC != COUPDAYS | Check all parameters are identical across functions |

## Use Cases

### [[Cash Flow Timing]]

**Scenario:** A portfolio manager needs to forecast when bond coupon payments will arrive for liquidity planning.

**Implementation:**
```
For each bond in portfolio:
Days to Next Coupon: =COUPDAYSNC(Today, Maturity, Frequency, Basis)
Next Payment Date: =Today + COUPDAYSNC(...)  [approximate]
Actual Payment Date: =COUPNCD(Today, Maturity, Frequency, Basis)
Payment Amount: =Face_Value * Coupon_Rate / Frequency
```

**Business Application:** Investment funds need to forecast cash inflows for redemption management, expense payments, and reinvestment planning. COUPDAYSNC provides the time component of this analysis, enabling accurate cash flow projections.

**Technical Details:** Use COUPNCD for the exact date; COUPDAYSNC provides the day count which may differ from calendar day difference for 30/360 conventions. Aggregate across portfolio for total expected inflows by date.

---

### [[Bond Pricing First Cash Flow Discount]]

**Scenario:** A quantitative analyst needs to implement bond pricing from first principles, requiring precise discounting of the first coupon payment.

**Implementation:**
```
Days to First Coupon: =COUPDAYSNC(Settlement, Maturity, 2, 1)
Days in Period: =COUPDAYS(Settlement, Maturity, 2, 1)
Fractional Period: =COUPDAYSNC(...) / COUPDAYS(...)
Discount Factor: =1 / (1 + Yield/2)^Fractional_Period
First Coupon PV: =Coupon_Payment * Discount_Factor
```

**Business Application:** The first coupon is typically less than a full period away, requiring fractional discounting. COUPDAYSNC provides the numerator for this fraction, enabling accurate present value calculations that match market prices.

**Technical Details:** Bond pricing discounts all cash flows to settlement. The first coupon uses fractional period discounting; subsequent coupons add whole periods. This methodology matches PRICE function internals.

---

### [[Reinvestment Strategy Timing]]

**Scenario:** A fixed-income trader needs to identify bonds with imminent coupon payments for a reinvestment strategy that captures coupons for reinvestment in higher-yielding securities.

**Implementation:**
```
Filter criteria: COUPDAYSNC < 30 (coupons due within 30 days)
For each qualifying bond:
  Days Remaining: =COUPDAYSNC(Today, Maturity, Frequency, Basis)
  Coupon Amount: =Face_Value * Coupon_Rate / Frequency
  Flag: =IF(COUPDAYSNC(...) < 30, "IMMINENT", "NORMAL")
```

**Business Application:** Coupon reinvestment strategies benefit from knowing exactly when cash will be available. Bonds with imminent coupons can be identified and the incoming cash can be pre-allocated to new investment opportunities.

**Technical Details:** Consider ex-coupon dates and settlement timing. The coupon accrues to whoever owns the bond on the record date, typically a few days before payment.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Date Handling:** Accepts various regional date formats
- **Calculation:** Identical logic across all Excel versions

### Google Sheets
- **Availability:** All versions since launch
- **Date Handling:** More strict; prefer DATE() function
- **Identical Results:** Returns same values as Excel for same inputs

### Both Platforms
- Same four parameters (3 required, 1 optional)
- Same five day count conventions
- Satisfies the identity: COUPDAYBS + COUPDAYSNC = COUPDAYS
- Returns integer values for 30/360; may return decimals for Actual in edge cases

## Tips and Best Practices

1. **Use the identity for verification:** Always check that COUPDAYBS + COUPDAYSNC = COUPDAYS. If this fails, you have a parameter mismatch.

2. **Combine with COUPNCD for complete analysis:** COUPDAYSNC gives you the day count; COUPNCD gives you the actual date. Together they provide complete next-coupon information.

3. **Match day count to security:** Treasuries use Actual/Actual (basis=1), corporates use 30/360 (basis=0). Wrong basis = wrong answer.

4. **Settlement on coupon date:** If you settle exactly on a coupon date, COUPDAYSNC returns the full period length (next period), not zero.

5. **For pricing calculations:** The ratio COUPDAYSNC/COUPDAYS gives the fractional period for discounting the first cash flow in bond pricing models.

6. **Cash flow forecasting:** Add COUPDAYSNC to settlement date for an approximate next payment date, but use COUPNCD for the exact date (30/360 day counts don't match calendar days).

7. **Near-maturity bonds:** COUPDAYSNC works correctly even for the final coupon period, returning days until the final coupon and principal repayment at maturity.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COUPDAYBS]] | Days from last coupon to settlement | To find elapsed days (the complement) |
| [[COUPDAYS]] | Total days in the coupon period | To get the denominator for fractional periods |
| [[COUPNCD]] | Next coupon payment date | To get the actual date, not just day count |
| [[COUPPCD]] | Previous coupon payment date | To find when the last coupon was paid |
| [[COUPNUM]] | Number of remaining coupons | To count total payments until maturity |

### Commonly Used Together

**[[COUPNCD]]** - Next Coupon Date

*Combined for complete next-coupon analysis:*
```
Days Until: =COUPDAYSNC(settlement, maturity, frequency, basis)
Actual Date: =COUPNCD(settlement, maturity, frequency, basis)
```
COUPDAYSNC gives the count; COUPNCD gives the calendar date.

---

**[[COUPDAYS]]** - Total Period Days

*Combined for fractional period calculation:*
```
Fractional Period Remaining: =COUPDAYSNC(...) / COUPDAYS(...)
```
Used in bond pricing to discount the first cash flow.

---

**[[COUPDAYBS]]** - Days Since Last Coupon

*Identity verification:*
```
=COUPDAYBS(...) + COUPDAYSNC(...)  should equal  =COUPDAYS(...)
```
Use this to verify calculation consistency.

---

**[[PRICE]]** - Bond Clean Price

*Understanding PRICE internals:*
```
First coupon discounted by: (1 + yield/freq)^(COUPDAYSNC/COUPDAYS)
```
PRICE uses COUPDAYSNC internally for fractional period discounting.

## Official Documentation

- **Microsoft Excel:** [COUPDAYSNC function](https://support.microsoft.com/en-us/office/coupdaysnc-function-5ab3f0b2-029f-4a8b-bb65-47d525eea547)
- **Google Sheets:** [COUPDAYSNC function](https://support.google.com/docs/answer/3093177)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
