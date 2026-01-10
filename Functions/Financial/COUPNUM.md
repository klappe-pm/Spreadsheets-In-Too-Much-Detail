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
- payment-count
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Coupon Number
- Number of Coupons
- Remaining Payments
tags:
- financial-modeling
- bond-analysis
- fixed-income
- coupon-schedule
---

# COUPNUM

## Description

**COUPNUM** returns the number of coupon payments remaining between the settlement date and maturity date. This function answers the fundamental question: "How many more interest payments will this bond make?" For a bond purchased today with 5 years to maturity and semi-annual payments, COUPNUM returns 10 (5 years x 2 payments per year). This count is essential for building payment schedules, sizing cash flow arrays, and understanding a bond's remaining life.

The function counts only FUTURE coupons from the settlement date forward. If you settle exactly on a coupon date, that coupon is counted as having just been paid (or being paid to the seller), so COUPNUM counts from the next payment onward. For the final coupon period, COUPNUM returns 1--there is one payment remaining at maturity. The count is always a whole number representing complete coupon payments; there are no fractional coupons.

COUPNUM is critical for bond pricing calculations because it determines how many cash flows must be discounted. The PRICE function internally uses COUPNUM to set up its discounting loop: the first coupon is discounted by a fractional period (COUPDAYSNC/COUPDAYS), and subsequent coupons are discounted by additional full periods up to COUPNUM total payments. Understanding this relationship helps you build custom pricing models or verify Excel's built-in calculations.

COUPNUM has been available in Excel since Excel 2007 and in Google Sheets since launch. The function requires settlement date, maturity date, and coupon frequency, with an optional day count basis. While the basis parameter is accepted for consistency with other COUP functions, it has minimal impact on COUPNUM since the count depends primarily on the time span and frequency, not on specific day counting conventions.

## Syntax

> [!f(x)] COUPNUM Syntax
>
> ```
> =COUPNUM(settlement, maturity, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when you take ownership. Payments after this date are counted. |
| `maturity` | Yes | The bond's maturity date--the final coupon and principal payment. Defines the endpoint. |
| `frequency` | Yes | Number of coupon payments per year: 1 = annual, 2 = semi-annual, 4 = quarterly. |
| `basis` | No | Day count convention. Defaults to 0. Has minimal effect on count but maintains parameter consistency with other COUP functions. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Notes for COUPNUM |
|-------|------------|-------------------|
| 0 (or omitted) | US (NASD) 30/360 | Standard calculation |
| 1 | Actual/Actual | May differ slightly at edge cases |
| 2 | Actual/360 | Same as basis 1 for counting |
| 3 | Actual/365 | Same as basis 1 for counting |
| 4 | European 30/360 | Same as basis 0 for counting |

### Return Value

Returns a whole number representing the count of remaining coupon payments from settlement to maturity (inclusive of the maturity payment). The minimum value is 1 (in the final coupon period). The maximum depends on the time to maturity and payment frequency.

## Examples

> [!f(x)] COUPNUM Examples

### Example 1: 10-Year Semi-Annual Bond
```
=COUPNUM("2024-01-15", "2034-01-15", 2, 0)
```
**Result:** `20`

**Explanation:** A 10-year bond with semi-annual payments has 20 coupons remaining (10 years x 2 payments/year = 20). The count includes all payments from after settlement through maturity.

---

### Example 2: 5-Year Annual Bond
```
=COUPNUM("2024-06-01", "2029-06-01", 1, 0)
```
**Result:** `5`

**Explanation:** A 5-year bond with annual payments has 5 coupons remaining. Annual bonds pay once per year, so 5 years = 5 payments.

---

### Example 3: 3-Year Quarterly Bond
```
=COUPNUM("2024-04-01", "2027-04-01", 4, 0)
```
**Result:** `12`

**Explanation:** A 3-year bond with quarterly payments has 12 coupons remaining (3 years x 4 payments/year = 12). Quarterly bonds are common for floating-rate securities.

---

### Example 4: Mid-Period Settlement
```
=COUPNUM("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `12`

**Explanation:** Settlement on April 15 is mid-period between the January 15 and July 15 coupons. There are still 12 coupons remaining: July 2024, Jan 2025, July 2025, Jan 2026, July 2026, Jan 2027, July 2027, Jan 2028, July 2028, Jan 2029, July 2029, and Jan 2030 (maturity).

---

### Example 5: Settlement on Coupon Date
```
=COUPNUM("2024-01-15", "2030-01-15", 2, 0)
```
**Result:** `12`

**Explanation:** Settlement falls exactly on a coupon date (January 15). The coupon paid that day goes to the seller. COUPNUM counts the remaining payments starting from the next coupon (July 15). Six years with 2 payments/year = 12 coupons.

---

### Example 6: Near Maturity (Final Period)
```
=COUPNUM("2024-10-01", "2025-01-15", 2, 0)
```
**Result:** `1`

**Explanation:** Settlement is in the final coupon period before maturity. Only one payment remains--the final coupon and principal at maturity on January 15, 2025.

---

### Example 7: Very Long Bond (30-Year Treasury)
```
=COUPNUM("2024-06-15", "2054-06-15", 2, 0)
```
**Result:** `60`

**Explanation:** A 30-year bond with semi-annual payments has 60 coupons remaining. Long-duration bonds have many cash flows to discount, making COUPNUM essential for pricing model setup.

---

### Example 8: Short-Term Bond (1 Year)
```
=COUPNUM("2024-06-01", "2025-06-01", 2, 0)
```
**Result:** `2`

**Explanation:** A 1-year bond with semi-annual payments has 2 coupons: one in 6 months (December) and one at maturity (June). Short-term bonds have few payments.

---

### Example 9: Using COUPNUM for Schedule Array Sizing
```
Create COUPNUM rows for a payment schedule:
Number of rows needed: =COUPNUM("2024-04-15", "2030-01-15", 2, 0)  → 12
Then populate each row with:
  Date: =COUPNCD chain
  Payment: =Face * Rate / Frequency
```
**Result:** Properly sized payment schedule

**Explanation:** COUPNUM tells you exactly how many rows to create for a complete coupon schedule. Each row represents one future payment from settlement to maturity.

---

### Example 10: Verification Formula
```
Settlement: 2024-04-15
Maturity: 2030-01-15
Expected: Approximately (2030-2024) * 2 = 12 coupons

=COUPNUM("2024-04-15", "2030-01-15", 2, 0)
```
**Result:** `12`

**Explanation:** Quick verification: years to maturity times frequency should approximately equal COUPNUM. The exact count depends on where in the coupon period you settle.

---

### Example 11: Comparing Different Frequencies
```
Same 5-year bond with different frequencies:
Annual:      =COUPNUM("2024-06-01", "2029-06-01", 1, 0)  → 5
Semi-annual: =COUPNUM("2024-06-01", "2029-06-01", 2, 0)  → 10
Quarterly:   =COUPNUM("2024-06-01", "2029-06-01", 4, 0)  → 20
```
**Result:** `5`, `10`, `20`

**Explanation:** The same maturity yields different coupon counts depending on payment frequency. Higher frequency = more payments = more cash flows to manage and discount.

---

### Example 12: Bond Duration Approximation
```
Duration rough estimate: Years to maturity approximates duration for par bonds
Years: =(Maturity - Settlement) / 365
Coupons: =COUPNUM(Settlement, Maturity, Frequency, Basis)
Average Life: =Coupons / Frequency  [in years]
```
**Result:** Quick duration approximation

**Explanation:** COUPNUM divided by frequency gives approximate years to maturity. For par bonds, this roughly approximates duration. (Actual duration requires DURATION or MDURATION for precision.)

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or non-date text | Use DATE() function or verify date values |
| `#NUM!` | Settlement >= maturity | Settlement must be strictly before maturity |
| `#NUM!` | Invalid frequency | Must be 1 (annual), 2 (semi-annual), or 4 (quarterly) |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4 |
| `Unexpected count` | Misunderstanding of coupon date handling | Settlement ON coupon date counts from next payment |
| `Off by one` | Edge case near coupon dates | Verify exact settlement vs coupon date alignment |

## Use Cases

### [[Payment Schedule Initialization]]

**Scenario:** A portfolio systems developer needs to create a properly sized array for bond cash flows in a pricing model.

**Implementation:**
```
Number of Cash Flows: =COUPNUM(Settlement, Maturity, Frequency, Basis)
Array Size: =COUPNUM(...) + 1  [if including settlement row]

For each payment i from 1 to COUPNUM:
  Payment Date: =COUPNCD(Previous_Date, Maturity, Frequency, Basis)
  Payment Amount: Face * Rate / Frequency
  Principal: IF(i = COUPNUM, Face, 0)  [principal only at maturity]
```

**Business Application:** Fixed-income pricing systems require knowing the exact number of future cash flows to allocate arrays, loop through discount factors, and ensure all payments are captured. COUPNUM provides this count directly.

**Technical Details:** Include error handling for bonds with no remaining coupons (already matured). Consider callable bonds where effective maturity may differ from stated maturity.

---

### [[Duration Calculation Setup]]

**Scenario:** A risk analyst needs to implement Macaulay duration from first principles, requiring the count of cash flows to weight.

**Implementation:**
```
Number of Coupons: =COUPNUM(Settlement, Maturity, Frequency, Basis)

For each coupon i:
  Time to Payment: =COUPDAYSNC(...)/COUPDAYS(...) + (i-1)  [in periods]
  PV of Payment: =Payment / (1 + Yield/Frequency)^Time
  Weighted PV: =Time * PV of Payment

Duration: =SUM(Weighted_PVs) / Total_PV
```

**Business Application:** Duration measures interest rate sensitivity. The calculation requires summing time-weighted present values of all cash flows. COUPNUM determines how many terms to include in this summation.

**Technical Details:** The first cash flow has fractional time (COUPDAYSNC/COUPDAYS periods); subsequent coupons add full periods. The final cash flow includes both coupon and principal.

---

### [[Bond Amortization Schedule]]

**Scenario:** An accountant needs to create an amortization schedule for a premium or discount bond, showing the carrying value over time.

**Implementation:**
```
Number of Periods: =COUPNUM(Settlement, Maturity, Frequency, Basis)

Starting Carrying Value: =PRICE(...) / 100 * Face
For each period:
  Interest Income: =Carrying_Value * Yield / Frequency
  Cash Received: =Face * Coupon / Frequency
  Amortization: =Interest_Income - Cash_Received
  Ending CV: =Beginning_CV + Amortization
Final CV: Should equal Face at maturity
```

**Business Application:** GAAP requires premium/discount amortization using the effective interest method. COUPNUM determines how many periods to include in the amortization schedule, ensuring it runs exactly from purchase to maturity.

**Technical Details:** Premium bonds have negative amortization (CV decreases to par); discount bonds have positive amortization (CV increases to par). Verify ending CV equals face value as a check.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Return Value:** Integer representing coupon count
- **Calculation:** Consistent across all Excel versions

### Google Sheets
- **Availability:** All versions since launch
- **Return Value:** Integer (same as Excel)
- **Identical Results:** Returns same counts as Excel for identical inputs

### Both Platforms
- Same four parameters (3 required, 1 optional)
- Returns whole number (no fractional coupons)
- Minimum return value is 1 (in final period)
- Basis parameter has minimal effect on count

## Tips and Best Practices

1. **Use for array sizing:** COUPNUM tells you exactly how many rows/elements you need for a complete payment schedule. No guessing required.

2. **Settlement on coupon date:** If you settle exactly on a coupon date, that coupon is NOT included in the count--it goes to the seller. COUPNUM counts from the next payment.

3. **Quick verification:** Years to maturity times frequency should approximately equal COUPNUM. Large discrepancies indicate input errors.

4. **Final period = 1:** In the final coupon period, COUPNUM always returns 1 (one payment remains at maturity).

5. **Pair with COUPNCD for schedules:** Use COUPNUM to know how many dates you need, then chain COUPNCD calls to fill them in.

6. **Pricing model loop:** For i = 1 to COUPNUM, discount each payment. The first payment uses COUPDAYSNC/COUPDAYS fractional period; subsequent payments add full periods.

7. **Includes maturity payment:** The count includes the final coupon paid at maturity. Principal repayment occurs at the same time but is not a separate "coupon."

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COUPNCD]] | Next coupon payment date | To find specific dates, not just count |
| [[COUPPCD]] | Previous coupon payment date | To find when the last coupon was paid |
| [[COUPDAYBS]] | Days from last coupon to settlement | For time elapsed in current period |
| [[COUPDAYSNC]] | Days from settlement to next coupon | For time remaining to next payment |
| [[COUPDAYS]] | Total days in coupon period | For period length |

### Commonly Used Together

**[[COUPNCD]]** - Next Coupon Date

*Combined for schedule building:*
```
Count: =COUPNUM(settlement, maturity, frequency, basis)
Dates: Chain COUPNCD calls for COUPNUM iterations
```
COUPNUM tells you how many; COUPNCD gives you each date.

---

**[[PRICE]]** - Bond Clean Price

*Understanding PRICE internals:*
```
PRICE discounts COUPNUM cash flows:
First coupon at fractional period
Remaining coupons at full period increments
Principal at final coupon period
```
PRICE uses COUPNUM internally to set up its cash flow loop.

---

**[[DURATION]]** - Macaulay Duration

*Combined for analysis:*
```
Coupon Count: =COUPNUM(...)
Duration: =DURATION(...)
Average period per coupon: =DURATION(...) * Frequency / COUPNUM(...)
```
Both functions analyze time characteristics of bond cash flows.

---

**[[YIELD]]** - Bond Yield to Maturity

*Combined for analytics:*
```
Yield: =YIELD(settlement, maturity, rate, price, 100, frequency, basis)
Remaining Coupons: =COUPNUM(settlement, maturity, frequency, basis)
Total Interest to Receive: =COUPNUM(...) * Face * Rate / Frequency
```
Understanding both return and payment count provides complete bond analysis.

## Official Documentation

- **Microsoft Excel:** [COUPNUM function](https://support.microsoft.com/en-us/office/coupnum-function-a90af57b-de53-4969-9c99-dd6139db2522)
- **Google Sheets:** [COUPNUM function](https://support.google.com/docs/answer/3093179)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
