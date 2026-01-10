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
- payment-schedule
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Coupon Next Date
- Next Coupon Date
- Next Payment Date
tags:
- financial-modeling
- bond-analysis
- fixed-income
- coupon-schedule
---

# COUPNCD

## Description

**COUPNCD** returns the next coupon date after the settlement date. This function answers the straightforward but essential question: "When is the next interest payment?" For bond investors and portfolio managers, knowing the next coupon date is critical for cash flow planning, record-keeping, and understanding when accrued interest will be paid. The function returns an actual date value (Excel serial number) that can be formatted and used in further date calculations.

The function determines coupon dates by working backward from the maturity date. Bonds pay coupons at regular intervals (semi-annually, annually, or quarterly), and the final coupon coincides with maturity. COUPNCD calculates the coupon schedule based on this maturity date and frequency, then identifies which coupon date falls immediately after your settlement date. For a bond maturing on June 15 with semi-annual payments, coupons fall on June 15 and December 15 each year. If you settle on March 1, COUPNCD returns June 15 of that year.

Unlike the COUPDAYBS and COUPDAYSNC functions that count days, COUPNCD returns an actual date. This makes it directly usable for payment schedules, calendar integrations, and date-based filtering. The function still requires the day count basis parameter, though this primarily affects edge cases involving month-end dates and does not change the fundamental coupon schedule derived from maturity. The basis ensures consistency when COUPNCD is used alongside other COUP functions.

COUPNCD has been available in Excel since Excel 2007 and in Google Sheets since launch. Both platforms return identical dates for the same inputs. The function accepts settlement date, maturity date, payment frequency, and optional day count basis. The returned value is a date serial number that will display as a date when the cell is formatted appropriately. In Excel, dates are stored as numbers (days since January 1, 1900); format the result as a date to see the calendar representation.

## Syntax

> [!f(x)] COUPNCD Syntax
>
> ```
> =COUPNCD(settlement, maturity, frequency, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The bond's settlement date--when you take ownership. Enter as date value, DATE() function, or cell reference. |
| `maturity` | Yes | The bond's maturity date--defines the coupon payment schedule. Must be after settlement. |
| `frequency` | Yes | Number of coupon payments per year: 1 = annual, 2 = semi-annual, 4 = quarterly. |
| `basis` | No | Day count convention. Defaults to 0 (US 30/360). Primarily affects month-end handling. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Effect on COUPNCD |
|-------|------------|-------------------|
| 0 (or omitted) | US (NASD) 30/360 | Standard month-end handling |
| 1 | Actual/Actual | Actual calendar date calculation |
| 2 | Actual/360 | Same date calculation as basis 1 |
| 3 | Actual/365 | Same date calculation as basis 1 |
| 4 | European 30/360 | European month-end handling |

### Return Value

Returns a date serial number representing the next coupon payment date after settlement. Format the cell as a date to display in calendar format (e.g., "2024-07-15" or "July 15, 2024"). If settlement falls exactly on a coupon date, returns the following coupon date (not the current one).

## Examples

> [!f(x)] COUPNCD Examples

### Example 1: Basic Semi-Annual Bond
```
=COUPNCD("2024-04-15", "2030-06-15", 2, 0)
```
**Result:** `2024-06-15` (displayed as date)

**Explanation:** This bond matures June 15, 2030 with semi-annual payments (June 15 and December 15). Settlement on April 15, 2024 is between the December 2023 and June 2024 coupons. The next coupon after April 15 is June 15, 2024.

---

### Example 2: Settlement Exactly on Coupon Date
```
=COUPNCD("2024-06-15", "2030-06-15", 2, 0)
```
**Result:** `2024-12-15`

**Explanation:** Settlement falls exactly on a coupon date (June 15). COUPNCD returns the NEXT coupon date after settlement, which is December 15, 2024. The June 15 coupon is paid to the seller or has just been received.

---

### Example 3: One Day Before Coupon
```
=COUPNCD("2024-06-14", "2030-06-15", 2, 0)
```
**Result:** `2024-06-15`

**Explanation:** Settlement is one day before the coupon. The next coupon date is tomorrow, June 15, 2024. The buyer will receive this coupon but owes nearly a full period of accrued interest.

---

### Example 4: Annual Coupon Bond
```
=COUPNCD("2024-03-01", "2029-09-15", 1, 0)
```
**Result:** `2024-09-15`

**Explanation:** This annual bond pays one coupon per year on September 15. Settlement on March 1 is about 6 months before the next annual payment. The next coupon is September 15, 2024.

---

### Example 5: Quarterly Bond
```
=COUPNCD("2024-05-01", "2028-08-20", 4, 0)
```
**Result:** `2024-05-20`

**Explanation:** This quarterly bond pays on February 20, May 20, August 20, and November 20. Settlement on May 1 is before the May 20 coupon, so May 20, 2024 is returned as the next coupon date.

---

### Example 6: Near Maturity
```
=COUPNCD("2024-12-01", "2025-01-15", 2, 0)
```
**Result:** `2025-01-15`

**Explanation:** Settlement is near maturity. The next (and final) coupon is the maturity date itself, January 15, 2025, when the bond pays its last coupon plus returns principal.

---

### Example 7: Creating a Coupon Schedule
```
First Coupon: =COUPNCD(Settlement, Maturity, 2, 0)
Second Coupon: =COUPNCD(First_Coupon, Maturity, 2, 0)
Third Coupon: =COUPNCD(Second_Coupon, Maturity, 2, 0)
... continue until maturity
```
**Result:** Complete coupon payment schedule

**Explanation:** By using each coupon date as the new "settlement" for the next COUPNCD call, you can build a complete coupon schedule from purchase to maturity.

---

### Example 8: Calculating Days to Next Coupon (Verification)
```
Next Coupon: =COUPNCD("2024-04-15", "2030-06-15", 2, 0)
Days to NC: =COUPNCD(...) - DATE(2024,4,15)
COUPDAYSNC: =COUPDAYSNC("2024-04-15", "2030-06-15", 2, 0)
```
**Result:** The date difference and COUPDAYSNC should be similar (exact match for Actual conventions, may differ for 30/360)

**Explanation:** For Actual/Actual, the calendar day difference equals COUPDAYSNC. For 30/360, they may differ because 30/360 uses standardized month lengths.

---

### Example 9: Month-End Bond
```
=COUPNCD("2024-02-15", "2030-08-31", 2, 0)
```
**Result:** `2024-02-29` (in leap year) or `2024-02-28` (non-leap year)

**Explanation:** This bond matures on August 31, paying coupons on February 28/29 and August 31. Month-end bonds have special handling rules. In 2024 (leap year), the February coupon falls on February 29.

---

### Example 10: Formatting the Result
```
=TEXT(COUPNCD("2024-04-15", "2030-06-15", 2, 0), "MMMM D, YYYY")
```
**Result:** `June 15, 2024`

**Explanation:** COUPNCD returns a date serial number. Use TEXT() or cell formatting to display it in your preferred date format. This is useful for reports and presentations.

---

### Example 11: Days Until Next Coupon (Calendar Days)
```
=COUPNCD("2024-04-15", "2030-06-15", 2, 0) - DATE(2024,4,15)
```
**Result:** `61` (actual calendar days)

**Explanation:** Subtracting settlement from COUPNCD gives actual calendar days until the next coupon. This differs from COUPDAYSNC when using 30/360 convention, which would show 60 days.

---

### Example 12: Conditional Cash Flow Analysis
```
=IF(COUPNCD(TODAY(), Maturity, 2, 0) <= TODAY() + 30, "Coupon Imminent", "Normal")
```
**Result:** Flags bonds with coupons due within 30 days

**Explanation:** Combining COUPNCD with conditional logic identifies bonds approaching payment dates. Useful for portfolio monitoring and cash flow alerts.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or non-date text | Use DATE() function or verify date formats |
| `#NUM!` | Settlement >= maturity | Settlement must be strictly before maturity |
| `#NUM!` | Invalid frequency | Must be 1, 2, or 4 only |
| `#NUM!` | Invalid basis | Must be 0, 1, 2, 3, or 4 |
| `Number displayed` | Cell not formatted as date | Format cell as date to see calendar date |
| `Wrong date` | Month-end handling differs by basis | Verify basis matches your security's conventions |

## Use Cases

### [[Payment Schedule Generation]]

**Scenario:** A portfolio accountant needs to generate a complete coupon payment schedule for a bond from purchase to maturity.

**Implementation:**
```
Settlement: A1 (2024-04-15)
Maturity: B1 (2030-06-15)
Frequency: 2 (semi-annual)

Coupon 1: =COUPNCD(A1, B1, 2, 0)                    → 2024-06-15
Coupon 2: =COUPNCD(A2, $B$1, 2, 0)                  → 2024-12-15
Coupon 3: =COUPNCD(A3, $B$1, 2, 0)                  → 2025-06-15
... drag down until result = Maturity
```

**Business Application:** Complete payment schedules are required for accounting accruals, cash flow projections, and audit documentation. COUPNCD iteratively builds the schedule by using each coupon date as the reference for finding the next.

**Technical Details:** The schedule will contain COUPNUM() entries. Verify the final coupon equals the maturity date. Add columns for payment amounts (Face x Rate / Frequency) and accrued interest received.

---

### [[Ex-Dividend Date Calculation]]

**Scenario:** A trader needs to identify bonds going ex-dividend (ex-coupon) soon to understand who will receive the upcoming payment.

**Implementation:**
```
Next Coupon: =COUPNCD(Today, Maturity, Frequency, Basis)
Record Date: =COUPNCD(...) - 1  [or per security's rules]
Ex-Date: =Record_Date - Business_Days_to_Settle
Flag: =IF(TODAY() >= Ex_Date, "Ex-Coupon", "Cum-Coupon")
```

**Business Application:** Bonds trade "cum-coupon" (buyer gets coupon) or "ex-coupon" (seller keeps coupon). Knowing the ex-date is critical for trading decisions and explaining price changes around coupon dates.

**Technical Details:** Record date rules vary by security and market. Corporate bonds typically have record dates a few days before payment. Government bonds may differ. Verify specific security terms.

---

### [[Cash Flow Forecasting Dashboard]]

**Scenario:** A portfolio manager needs a dashboard showing expected cash inflows from bond coupons over the next 90 days.

**Implementation:**
```
For each bond:
Next Coupon Date: =COUPNCD(Today, Maturity, Frequency, Basis)
In Window: =IF(AND(COUPNCD(...) >= TODAY(), COUPNCD(...) <= TODAY()+90), "Yes", "No")
Payment Amount: =IF(In_Window="Yes", Face*Rate/Frequency, 0)

Dashboard Summary:
Total Expected Inflows: =SUMIF(In_Window_Range, "Yes", Payment_Range)
```

**Business Application:** Investment funds need to forecast cash inflows for liquidity management, redemption planning, and reinvestment strategies. COUPNCD identifies which payments fall within the planning window.

**Technical Details:** Consider settlement timing (T+1 or T+2) when forecasting actual cash receipt dates. Aggregate by week or month for summary views.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (Analysis ToolPak in earlier versions)
- **Return Value:** Date serial number (number of days since Jan 1, 1900)
- **Date System:** Uses 1900 date system by default; Mac Excel uses 1904 system optionally

### Google Sheets
- **Availability:** All versions since launch
- **Return Value:** Date serial number (compatible with Excel)
- **Identical Results:** Returns same dates as Excel for identical inputs

### Both Platforms
- Same four parameters (3 required, 1 optional)
- Returns date serial number requiring date formatting
- Handles month-end dates according to basis parameter
- Settlement ON coupon date returns the following coupon date

## Tips and Best Practices

1. **Format the result as a date:** COUPNCD returns a serial number. Format the cell as a date or use TEXT() to display the calendar date.

2. **Use for schedule building:** Chain COUPNCD calls to build complete coupon schedules: Next = COUPNCD(Previous, Maturity, Freq, Basis).

3. **Pair with COUPPCD:** COUPPCD gives the previous coupon; COUPNCD gives the next. Together they define the current coupon period.

4. **Settlement on coupon date:** COUPNCD returns the NEXT coupon, not the current one, when you settle exactly on a payment date.

5. **Verify with COUPDAYSNC:** For Actual basis, COUPNCD - Settlement should equal COUPDAYSNC. For 30/360, they may differ due to standardized month lengths.

6. **Month-end securities:** Bonds with month-end coupon dates (e.g., Feb 28/29, Aug 31) have special handling. Test with your specific dates.

7. **Ex-dividend considerations:** The next coupon date is when payment occurs, but ownership for payment purposes is determined earlier (record date). Consider this for trading near coupon dates.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COUPPCD]] | Previous coupon payment date | To find the start of the current period |
| [[COUPDAYSNC]] | Days from settlement to next coupon | When you need the day count, not the date |
| [[COUPNUM]] | Number of remaining coupons | To count how many payments remain |
| [[COUPDAYBS]] | Days from last coupon to settlement | To calculate elapsed time in current period |
| [[COUPDAYS]] | Total days in coupon period | To get the full period length |

### Commonly Used Together

**[[COUPPCD]]** - Previous Coupon Date

*Combined to define the current coupon period:*
```
Period Start: =COUPPCD(settlement, maturity, frequency, basis)
Period End: =COUPNCD(settlement, maturity, frequency, basis)
Current Period: COUPPCD to COUPNCD
```
These two functions bracket the current coupon period.

---

**[[COUPDAYSNC]]** - Days to Next Coupon

*Day count vs calendar date:*
```
Next Date: =COUPNCD(...)                        → Returns date
Days Until: =COUPDAYSNC(...)                    → Returns day count
Calendar Days: =COUPNCD(...) - Settlement       → Actual calendar days
```
COUPDAYSNC uses day count convention; direct subtraction gives calendar days.

---

**[[COUPNUM]]** - Coupon Count

*Combined for schedule sizing:*
```
Remaining Coupons: =COUPNUM(settlement, maturity, frequency, basis)
Build schedule with COUPNUM rows using COUPNCD chain
```
COUPNUM tells you how many rows your schedule needs; COUPNCD fills in the dates.

---

**[[PRICE]]** - Bond Price

*Combined for pricing and cash flow analysis:*
```
Price: =PRICE(settlement, maturity, rate, yield, 100, frequency, basis)
Next Coupon: =COUPNCD(settlement, maturity, frequency, basis)
```
Price valuation and coupon timing together provide complete bond analytics.

## Official Documentation

- **Microsoft Excel:** [COUPNCD function](https://support.microsoft.com/en-us/office/coupncd-function-fd962fef-506b-4d9d-8590-16df5393691f)
- **Google Sheets:** [COUPNCD function](https://support.google.com/docs/answer/3093178)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
