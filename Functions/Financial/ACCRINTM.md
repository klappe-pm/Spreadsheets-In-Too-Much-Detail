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
- Accrued Interest at Maturity
- Maturity Accrued Interest
- Zero Coupon Interest
tags:
- financial-modeling
- bond-analysis
- fixed-income
- securities-trading
---

# ACCRINTM

## Description

**ACCRINTM (Accrued Interest at Maturity)** calculates the accrued interest for a security that pays interest only at maturity, rather than through periodic coupon payments. This function applies to discount securities, zero-coupon bonds, and other instruments where all interest is paid as a lump sum when the security matures. Unlike ACCRINT which handles periodic interest payments, ACCRINTM deals with the simpler case where interest accrues from issue date to maturity date and is paid all at once.

Securities that pay at maturity are common in money markets and short-term financing. Treasury bills, commercial paper, and certificates of deposit (CDs) often work this way--you buy the security at a discount to face value, and at maturity you receive the full face value. The difference between what you paid and what you receive represents your interest income. ACCRINTM calculates this interest based on the stated annual rate, issue date, maturity date, par value, and day count convention.

The **day count convention** (basis parameter) is critical because it determines how many days are assumed in the accrual period and in a year. Different markets use different conventions: US Treasury bills use Actual/360, commercial paper often uses 30/360, and certificates of deposit may use Actual/365. The formula is: Accrued Interest = Par x Rate x (Days from Issue to Maturity / Days per Year under convention). A security with a 5% annual rate held for 90 days under Actual/360 would accrue interest of Par x 0.05 x (90/360) = Par x 1.25%.

ACCRINTM has been available in Excel since Excel 2007 (when Analysis ToolPak functions were integrated into core Excel) and in Google Sheets since launch. The function is simpler than ACCRINT because it does not require frequency or first interest date parameters--all interest is assumed to accrue linearly from issue to maturity. This function is essential for pricing and trading short-term securities, calculating interest income on discount instruments, and reconciling money market fund holdings.

## Syntax

> [!f(x)] ACCRINTM Syntax
>
> ```
> =ACCRINTM(issue, settlement, rate, par, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `issue` | Yes | The security's issue date--when the security was originally issued or the start of the interest accrual period. Enter as date value, date string, or cell reference. |
| `settlement` | Yes | The maturity (settlement) date--when the security matures and interest is paid. Despite the parameter name, this represents the maturity date in context of this function. Must be after issue date. |
| `rate` | Yes | The security's annual interest rate as a decimal. For a 4% rate, enter 0.04 or 4%. |
| `par` | Yes | The security's par (face) value--the amount received at maturity. Enter as a positive number. |
| `basis` | No | The day count basis (convention) to use. See Day Count Conventions table below. Defaults to 0 (US 30/360) if omitted. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | Assumes 30 days per month, 360 days per year. If a date falls on the 31st, it is treated as the 30th. | US commercial paper, some CDs |
| 1 | Actual/Actual | Uses actual days in the period and actual days in the year (365 or 366). Most accurate but less common for short-term instruments. | Some government securities |
| 2 | Actual/360 | Uses actual number of days but divides by 360. Standard for money market instruments. | US Treasury bills, bank deposits, Eurodollar deposits |
| 3 | Actual/365 | Uses actual number of days but always divides by 365 (ignores leap years). | UK Treasury bills, some sterling instruments |
| 4 | European 30/360 | Similar to US 30/360 with different end-of-month handling. | European commercial paper |

### Return Value

Returns a numeric value representing the total accrued interest from issue to maturity (settlement), in the same currency unit as the par value.

## Examples

> [!f(x)] ACCRINTM Examples

### Example 1: Basic 90-Day Commercial Paper
```
=ACCRINTM("2024-01-15", "2024-04-15", 0.05, 100000, 0)
```
**Result:** `$1,250.00`

**Explanation:** Commercial paper issued January 15, maturing April 15 (90 days under 30/360), with 5% annual rate on $100,000 par. Interest = $100,000 x 0.05 x (90/360) = $1,250. This is the interest that accrues over the 90-day term. The investor buys at $98,750 and receives $100,000 at maturity.

---

### Example 2: Treasury Bill (Actual/360)
```
=ACCRINTM("2024-03-01", "2024-06-01", 0.0525, 10000, 2)
```
**Result:** `$134.17`

**Explanation:** A T-bill with 5.25% rate, $10,000 face value, 92 actual days from March 1 to June 1. Using Actual/360 (basis=2, standard for T-bills): Interest = $10,000 x 0.0525 x (92/360) = $134.17. T-bills are quoted on a discount basis, but ACCRINTM shows the equivalent interest earned.

---

### Example 3: 6-Month Certificate of Deposit
```
=ACCRINTM("2024-01-01", "2024-07-01", 0.0475, 50000, 0)
```
**Result:** `$1,187.50`

**Explanation:** A 6-month CD at 4.75% APY on $50,000. Using 30/360, Jan 1 to Jul 1 = 180 days. Interest = $50,000 x 0.0475 x (180/360) = $1,187.50. At maturity, the depositor receives $51,187.50. CDs are a common ACCRINTM use case.

---

### Example 4: Short-Term Note (30 Days)
```
=ACCRINTM("2024-05-01", "2024-05-31", 0.055, 1000000, 2)
```
**Result:** `$4,583.33`

**Explanation:** A 30-day corporate note at 5.5% on $1 million par. Using Actual/360: Interest = $1,000,000 x 0.055 x (30/360) = $4,583.33. Short-term notes are used by corporations for working capital financing. The lender earns this interest for a one-month loan.

---

### Example 5: UK-Style Calculation (Actual/365)
```
=ACCRINTM("2024-02-01", "2024-05-01", 0.045, 100000, 3)
```
**Result:** `$1,097.26`

**Explanation:** A sterling money market instrument with 4.5% rate, 89 actual days (Feb has 29 in 2024, Mar has 31, so Feb 1 to May 1 = 29+31+30 = 89 days in a leap year). Using Actual/365 (basis=3): Interest = 100,000 x 0.045 x (89/365) = $1,097.26. UK instruments often use Actual/365 convention.

---

### Example 6: European Commercial Paper
```
=ACCRINTM("2024-04-15", "2024-07-15", 0.04, 500000, 4)
```
**Result:** `$5,000.00`

**Explanation:** Euro commercial paper with 4% rate, EUR 500,000 par, using European 30/360 (basis=4). From Apr 15 to Jul 15 = 90 days. Interest = 500,000 x 0.04 x (90/360) = 5,000. European money market instruments typically use European 30/360 convention.

---

### Example 7: Comparing Day Count Conventions
```
=ACCRINTM("2024-01-31", "2024-04-30", 0.05, 100000, 0) → 30/360
=ACCRINTM("2024-01-31", "2024-04-30", 0.05, 100000, 1) → Actual/Actual
=ACCRINTM("2024-01-31", "2024-04-30", 0.05, 100000, 2) → Actual/360
```
**Result:** Different values for each basis

**Explanation:** The same security produces different accrued interest depending on basis. 30/360 treats it as exactly 90 days (3 months x 30). Actual/Actual counts 90 real days (29+31+30=90) divided by 366 (leap year). Actual/360 counts 90 real days divided by 360. Understanding these differences is critical for accurate pricing.

---

### Example 8: Zero-Coupon Bond to Maturity
```
=ACCRINTM("2020-06-15", "2024-06-15", 0.035, 10000, 1)
```
**Result:** `$1,400.00`

**Explanation:** A 4-year zero-coupon bond with implied 3.5% rate. Interest = $10,000 x 0.035 x 4 = $1,400. For longer-term zeros, ACCRINTM gives simple interest, but actual zero-coupon bond pricing uses compound interest. ACCRINTM is more accurate for short-term instruments.

---

### Example 9: Overnight Deposit
```
=ACCRINTM("2024-06-14", "2024-06-15", 0.0535, 10000000, 2)
```
**Result:** `$1,486.11`

**Explanation:** An overnight deposit of $10 million at 5.35% federal funds rate. Using Actual/360 for 1 day: Interest = $10,000,000 x 0.0535 x (1/360) = $1,486.11. Banks earn this amount on each overnight lending transaction. Small per-day amounts add up significantly over time.

---

### Example 10: Using Cell References for Portfolio
```
=ACCRINTM(A2, B2, C2, D2, E2)
```
Where A2=Issue, B2=Maturity, C2=Rate, D2=Par, E2=Basis

**Result:** Varies by inputs

**Explanation:** Building a money market portfolio model with cell references allows calculating interest income across multiple positions. Copy the formula down for each security, then SUM the results for total expected interest income.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or text in numeric parameters | Ensure dates are valid Excel/Sheets date values. Use DATE() function if needed. |
| `#NUM!` | Settlement (maturity) date is before or equal to issue date | Maturity must be after issue date. Check date order. |
| `#NUM!` | Invalid basis value | Basis must be 0, 1, 2, 3, or 4. Check for typos. |
| `#NUM!` | Negative rate or par value | Rate and par must be positive. Zero rate is allowed but unusual. |
| `Unexpected result` | Wrong day count convention | Different basis values give different results. Verify which convention applies to your instrument. |
| `Precision differences` | Leap year handling | Actual/Actual (basis=1) and Actual/365 (basis=3) handle leap years differently. Verify which is appropriate. |

## Use Cases

### [[Money Market Fund Interest Calculation]]

**Scenario:** A money market fund manager needs to calculate expected interest income from a portfolio of commercial paper and Treasury bills maturing over the next 30 days.

**Implementation:**
```
For each security:
=ACCRINTM(IssueDate, MaturityDate, Rate, ParValue, Basis)

Portfolio total:
=SUM(ACCRINTM(A2,B2,C2,D2,E2), ACCRINTM(A3,B3,C3,D3,E3), ...)
```

**Business Application:** Money market funds must project interest income for NAV calculations and yield reporting. ACCRINTM provides the expected interest from each holding through its maturity date. Aggregating across all holdings gives total expected income, which is distributed to fund shareholders as dividends.

**Technical Details:** Treasury bills use Actual/360 (basis=2), commercial paper typically uses 30/360 (basis=0), and CDs may vary. Ensure each security uses its appropriate convention. Match the par value to your actual holding size, not the standard trading unit.

---

### [[Certificate of Deposit Comparison Shopping]]

**Scenario:** An individual investor wants to compare CDs from different banks with varying rates, terms, and compounding methods to maximize interest income on a $100,000 deposit.

**Implementation:**
```
Bank A (6-month, 4.50%): =ACCRINTM("2024-01-01", "2024-07-01", 0.045, 100000, 0)
Bank B (9-month, 4.75%): =ACCRINTM("2024-01-01", "2024-10-01", 0.0475, 100000, 0)
Bank C (12-month, 5.00%): =ACCRINTM("2024-01-01", "2025-01-01", 0.05, 100000, 0)
```

**Business Application:** When comparing CDs, simple rate comparison is insufficient--term matters. A higher rate for a shorter term might yield less total interest than a lower rate for a longer term. ACCRINTM shows absolute dollar returns for direct comparison, helping investors choose optimally.

**Technical Details:** ACCRINTM calculates simple interest. Most CDs quote APY (Annual Percentage Yield) which includes compounding effects. For accurate comparison, either convert ACCRINTM results to APY or use stated APR (not APY) as the rate input. Longer CDs may offer higher rates but sacrifice liquidity.

---

### [[Treasury Bill Pricing Verification]]

**Scenario:** A bond trader needs to verify that a Treasury bill quote is correctly priced based on the stated discount rate and days to maturity.

**Implementation:**
```
Given: 91-day T-bill, 5.25% discount rate, $1,000,000 face value
Discount Amount: =ACCRINTM("2024-03-14", "2024-06-13", 0.0525, 1000000, 2)
                = $13,270.83
Purchase Price: = $1,000,000 - $13,270.83 = $986,729.17
```

**Business Application:** T-bill prices are quoted as discount rates, not prices. Traders use ACCRINTM to convert between discount rates and dollar prices. This calculation verifies broker quotes, ensures correct wire amounts, and helps identify mispriced securities for arbitrage opportunities.

**Technical Details:** T-bills use Actual/360 (basis=2) and are quoted per $100 face value in the market. The discount rate is not the same as yield--use TBILLYIELD to convert discount to equivalent bond yield for comparison to other investments.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (was Analysis ToolPak add-in in earlier versions)
- **Date Handling:** Accepts various date formats based on system locale. DATE(), DATEVALUE(), and direct entry all work.
- **Parameter name:** The second parameter is called "settlement" in documentation but represents maturity date in context.

### Google Sheets
- **Availability:** All versions since launch
- **Date Handling:** More strict about date formats; use DATE(year, month, day) for reliability.
- **Identical Behavior:** Results match Excel for same inputs.

### Both Platforms
- Same five basis (day count) options
- Same calculation methodology
- No frequency parameter needed (all interest at maturity)
- Results identical for equivalent inputs

## Tips and Best Practices

1. **Use Actual/360 (basis=2) for Treasury bills and bank deposits:** This is the market convention for most US money market instruments. Using 30/360 will give incorrect results.

2. **Remember that "settlement" means maturity date:** The parameter name is confusing, but for ACCRINTM, the settlement date is when the security matures and pays out.

3. **Match day count to market convention:** Commercial paper typically uses 30/360, T-bills use Actual/360, UK instruments use Actual/365. Using wrong conventions causes pricing errors.

4. **For compound interest instruments, ACCRINTM understates returns:** This function calculates simple interest. CDs and longer instruments with compounding will actually pay more than ACCRINTM shows.

5. **Verify with the basic formula:** Interest = Par x Rate x (Days/Year). Calculate manually to validate your ACCRINTM inputs are correct.

6. **Scale for position size:** If you enter par as $1,000 but own $100,000 face value, multiply the result by 100.

7. **Consider using TBILLPRICE for T-bill specific calculations:** While ACCRINTM works for T-bills, the TBILLPRICE function is purpose-built and may be more appropriate.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ACCRINT]] | Accrued interest for periodic coupon payments | When the security pays interest on a regular schedule (semi-annual, quarterly, etc.) |
| [[TBILLPRICE]] | Treasury bill price | When specifically working with T-bills and need price from discount rate |
| [[RECEIVED]] | Amount received at maturity for discounted securities | When you know the purchase price and want to find maturity proceeds |

### Commonly Used Together

**[[PRICEDISC]]** - Discounted Security Price

*Combined to understand full transaction:*
```
Purchase Price: =PRICEDISC(settlement, maturity, discount, redemption, basis)
Interest Earned: =ACCRINTM(settlement, maturity, rate, redemption, basis)
```
PRICEDISC shows what you pay; ACCRINTM shows what you earn in interest.

---

**[[YIELDDISC]]** - Discounted Security Yield

*Combined for rate analysis:*
```
Interest in dollars: =ACCRINTM(...)
Yield as percentage: =YIELDDISC(...)
```
ACCRINTM gives absolute return; YIELDDISC gives annualized percentage return for comparison.

---

**[[DISC]]** - Discount Rate

*Combined for pricing:*
```
Discount Rate: =DISC(settlement, maturity, price, redemption, basis)
Interest: =ACCRINTM(settlement, maturity, rate, redemption, basis)
```
Convert between discount rates and dollar amounts.

---

**[[SUM]]** - Portfolio Total Interest

*Combined for portfolio management:*
```
=SUM(ACCRINTM(A2:A10, B2:B10, C2:C10, D2:D10, E2:E10))
```
Total expected interest income from all money market holdings.

## Official Documentation

- **Microsoft Excel:** [ACCRINTM function](https://support.microsoft.com/en-us/office/accrintm-function-f62f01f9-5754-4cc4-805b-0e70199328a7)
- **Google Sheets:** [ACCRINTM function](https://support.google.com/docs/answer/3093201)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
