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
- interest-rate-calculation
- securities
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Interest Rate
- Security Interest Rate
- Fully Invested Rate
tags:
- financial-modeling
- bond-analysis
- fixed-income
- money-market
---

# INTRATE

## Description

**INTRATE** calculates the interest rate for a fully invested security--a security where you pay an amount at settlement and receive a larger amount at maturity, with the difference representing your interest. This function is designed for instruments that do not pay periodic interest but instead accrue interest that is paid in full at maturity, such as certificates of deposit, zero-coupon securities, and certain money market instruments. INTRATE answers: "What annual interest rate am I earning on this investment?"

Unlike DISC (which calculates the discount rate based on face value) or YIELD (which handles periodic coupon bonds), INTRATE computes the simple interest rate based on the purchase price and maturity proceeds. The formula is: Interest Rate = (Amount Received - Investment) / Investment x (Days in Year / Days Held). This is a true yield calculation because it uses your actual investment as the denominator, giving you the real return on the money you put in.

The distinction between INTRATE and DISC is important for money market practitioners. DISC calculates the bank discount rate, which divides the discount by face value and is the market convention for quoting T-bills. INTRATE calculates the investment rate (also called the "money market yield" or "CD-equivalent yield"), which divides the earnings by the purchase price. INTRATE always produces a higher rate than DISC for the same security because it uses a smaller denominator. This is why T-bill yields (INTRATE concept) are always higher than T-bill discount rates (DISC concept).

INTRATE has been available in Excel since Excel 2007 (integrated from Analysis ToolPak) and in Google Sheets since launch. The function is essential for comparing money market instruments to bank deposits and other investments where returns are expressed as yields rather than discount rates. The day count convention (basis parameter) affects the annualization and must match market convention.

## Syntax

> [!f(x)] INTRATE Syntax
>
> ```
> =INTRATE(settlement, maturity, investment, redemption, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `settlement` | Yes | The security's settlement date--when you acquire (invest in) the instrument. Enter as date value, string, or cell reference. |
| `maturity` | Yes | The security's maturity date--when it matures and you receive the redemption amount. Must be after settlement. |
| `investment` | Yes | The amount invested (purchase price). This is what you pay at settlement. |
| `redemption` | Yes | The amount received at maturity. This must be greater than investment for a positive interest rate. |
| `basis` | No | Day count basis. Defaults to 0 (US 30/360). See table below. |

### Day Count Conventions (Basis Parameter)

| Value | Convention | Description | Common Use |
|-------|------------|-------------|------------|
| 0 (or omitted) | US (NASD) 30/360 | 30 days/month, 360 days/year. | Some CDs, corporate instruments |
| 1 | Actual/Actual | Actual days in period and year. | Some government securities |
| 2 | Actual/360 | Actual days, divide by 360. | Money market instruments, T-bills |
| 3 | Actual/365 | Actual days, divide by 365. | UK instruments, some money markets |
| 4 | European 30/360 | European month-end rules. | European money market |

### Return Value

Returns a decimal value representing the annualized interest rate (yield). A result of 0.0545 means 5.45% annual interest rate.

## Examples

> [!f(x)] INTRATE Examples

### Example 1: Basic CD Interest Rate
```
=INTRATE("2024-01-15", "2024-07-15", 98000, 100000, 0)
```
**Result:** `0.0408` (4.08%)

**Explanation:** Invested $98,000, received $100,000 after 6 months. The $2,000 profit on a $98,000 investment over 180 days annualizes to 4.08%. This is the true yield on your invested capital.

---

### Example 2: Treasury Bill Investment Rate
```
=INTRATE("2024-03-15", "2024-06-13", 98.68, 100, 2)
```
**Result:** `0.0540` (5.40%)

**Explanation:** A T-bill purchased at 98.68 with 100 redemption. Using Actual/360 (basis=2). Note this is higher than the DISC rate (5.28%) for the same security because INTRATE uses the purchase price as denominator. This is the CD-equivalent or bond-equivalent yield.

---

### Example 3: Comparing INTRATE vs DISC
```
DISC:    =DISC("2024-03-15", "2024-06-13", 98.68, 100, 2) = 5.28%
INTRATE: =INTRATE("2024-03-15", "2024-06-13", 98.68, 100, 2) = 5.40%
```
**Result:** INTRATE > DISC always

**Explanation:** For the same security, INTRATE gives a higher rate than DISC. DISC uses face value (100) as denominator; INTRATE uses investment (98.68). The difference (12 basis points here) matters when comparing T-bills to other investments.

---

### Example 4: Short-Term Money Market
```
=INTRATE("2024-06-01", "2024-07-01", 995000, 1000000, 2)
```
**Result:** `0.0603` (6.03%)

**Explanation:** A 30-day money market placement: invest $995,000, receive $1,000,000. The $5,000 return on $995,000 over 30 days equals 6.03% annualized. This is how bank treasury desks calculate returns on short-term placements.

---

### Example 5: UK Style (Actual/365)
```
=INTRATE("2024-03-15", "2024-06-13", 98.70, 100, 3)
```
**Result:** `0.0548` (5.48%)

**Explanation:** Using Actual/365 instead of Actual/360 produces a slightly higher rate because the year is longer (365 vs 360 days). UK sterling instruments often use this convention.

---

### Example 6: Long-Term Zero-Coupon
```
=INTRATE("2024-01-01", "2025-01-01", 95, 100, 1)
```
**Result:** `0.0526` (5.26%)

**Explanation:** A 1-year zero-coupon instrument: invest 95, receive 100. The 5-point gain on 95 investment equals 5.26% return. For longer periods, simple interest (INTRATE) differs from compound interest--use YIELD functions for multi-year instruments.

---

### Example 7: Overnight Rate
```
=INTRATE("2024-06-14", "2024-06-15", 10000000, 10001389, 2)
```
**Result:** `0.05` (5.00%)

**Explanation:** An overnight investment of $10 million earning $1,389. Using Actual/360: ($1,389 / $10M) x (360/1) = 5.00%. This is how federal funds rate returns are calculated for overnight lending between banks.

---

### Example 8: Commercial Paper Return
```
=INTRATE("2024-06-01", "2024-09-01", 98750, 100000, 0)
```
**Result:** `0.0506` (5.06%)

**Explanation:** 90-day commercial paper purchased at $98,750, matures at $100,000. The investor earns 5.06% on the invested capital. This can be compared directly to bank CD rates.

---

### Example 9: Verifying with RECEIVED
```
Investment: $10,000 at 5% for 180 days (Actual/360)
Expected Maturity Amount: =RECEIVED("2024-01-15", "2024-07-15", 10000, 0.05, 2) = $10,250

Verification: =INTRATE("2024-01-15", "2024-07-15", 10000, 10250, 2) = 5.00% ✓
```
**Result:** INTRATE and RECEIVED are inverses

**Explanation:** RECEIVED calculates maturity amount from rate; INTRATE calculates rate from amounts. They should be consistent for the same inputs.

---

### Example 10: Portfolio Return Calculation
```
=INTRATE(A2, B2, C2, D2, E2)
```
Where A2=Settlement, B2=Maturity, C2=Investment, D2=Redemption, E2=Basis

**Result:** Varies by inputs

**Explanation:** Building a money market portfolio model with cell references allows calculating returns across multiple positions. Weight by investment amount for portfolio average return.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date or non-numeric parameters | Use valid dates and numbers. DATE() ensures correct format. |
| `#NUM!` | Settlement on or after maturity | Settlement must be before maturity. |
| `#NUM!` | Redemption less than or equal to investment | For positive interest, redemption must exceed investment. |
| `#NUM!` | Investment or redemption is zero or negative | Both must be positive numbers. |
| `#NUM!` | Invalid basis | Basis must be 0, 1, 2, 3, or 4. |
| `Rate seems wrong` | Wrong day count convention | Match basis to instrument type. T-bills use basis=2. |

## Use Cases

### [[Money Market Investment Analysis]]

**Scenario:** A corporate treasurer needs to compare returns on various short-term investment options: T-bills, commercial paper, and bank CDs, all with different quote conventions.

**Implementation:**
```
T-bill (quoted as discount):
  First convert: =INTRATE("2024-06-01", "2024-09-01", 98.68, 100, 2) = 5.40%

Commercial Paper (quoted as discount):
  Convert: =INTRATE("2024-06-01", "2024-09-01", 98.75, 100, 0) = 5.06%

Bank CD (quoted as yield): 5.25% (already in INTRATE format)

Comparison: T-bill 5.40% > CD 5.25% > CP 5.06%
Best return: T-bill
```

**Business Application:** Treasury departments must maximize returns on short-term cash while maintaining safety and liquidity. Different instruments use different conventions, making direct comparison impossible without conversion. INTRATE puts everything on the same basis.

**Technical Details:** T-bills are quoted as discount rates but should be converted to INTRATE (bond-equivalent yield) for comparison. CDs are typically already quoted as yields. Some CP is quoted as discount, some as yield--verify the convention before comparing.

---

### [[Bank Deposit Rate Verification]]

**Scenario:** A retail investor wants to verify that a bank is paying the advertised rate on a CD by calculating the actual return from the purchase and maturity amounts.

**Implementation:**
```
Advertised Rate: 5.00% APY on 6-month CD
Investment: $50,000
Expected Maturity Value (simple interest): $51,250
Actual Maturity Statement: $51,230

Calculated Rate: =INTRATE("2024-01-15", "2024-07-15", 50000, 51230, 0) = 4.92%

Discrepancy: 5.00% advertised vs 4.92% actual = 8 basis points short
```

**Business Application:** Consumers can verify that banks pay advertised rates. Small differences might be due to day count conventions or compounding methods. Large differences warrant investigation. INTRATE provides the simple interest equivalent.

**Technical Details:** Banks often quote APY (Annual Percentage Yield), which includes compounding. INTRATE calculates simple interest rate. For exact comparison, convert APY to simple rate or vice versa. Leap years and exact day counts can create small differences.

---

### [[Fixed-Income Arbitrage Pricing]]

**Scenario:** A trader identifies a potential arbitrage between a T-bill and a bank CD with identical maturities and needs to calculate exact returns to determine if the trade is profitable after costs.

**Implementation:**
```
T-bill: Buy at 98.50, receive 100 in 90 days
  Return: =INTRATE("2024-06-01", "2024-08-30", 98.50, 100, 2) = 6.15%

CD: 6.00% stated rate
  Verify: =RECEIVED("2024-06-01", "2024-08-30", 100, 0.06, 2) = 101.50
  But CD requires 100 investment, T-bill requires 98.50

Net arbitrage: Borrow at CD rate, invest in T-bill
  Profit = 6.15% - 6.00% = 15 basis points (before costs)
  Transaction costs: ~5 basis points
  Net profit: 10 basis points ✓
```

**Business Application:** Arbitrage traders look for mispricings between equivalent securities. INTRATE converts all returns to comparable terms, revealing opportunities. Even small spreads are profitable at large sizes with leverage.

**Technical Details:** Real arbitrage requires considering bid/ask spreads, settlement timing, counterparty risk, and financing costs. INTRATE provides the theoretical return; transaction costs determine if the trade is executable.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later
- **Calculation:** (Redemption - Investment) / Investment × (YearDays / DaysHeld)
- **Date Handling:** Accepts various formats based on locale

### Google Sheets
- **Availability:** All versions since launch
- **Identical Calculation:** Same methodology as Excel
- **Same Results:** Matches Excel for identical inputs

### Both Platforms
- Same five parameters (4 required, 1 optional)
- Same five day count conventions
- Returns simple interest rate (not compound)
- Redemption must exceed investment for positive rate

## Tips and Best Practices

1. **INTRATE gives true yield, DISC gives discount rate:** For comparing investments, INTRATE is usually more meaningful because it shows return on your actual investment, not on face value.

2. **Use basis=2 (Actual/360) for T-bill comparisons:** This is the "bond-equivalent yield" or "CD-equivalent yield" convention for money market instruments.

3. **INTRATE is simple interest, not compound:** For multi-year investments, simple interest understates true return. Use YIELD-type functions for longer-term securities.

4. **Verify with inverse function:** RECEIVED(INTRATE(...)) should return original redemption. Use this cross-check.

5. **Investment and redemption are amounts, not rates:** Enter the actual dollar amounts (or per-100 amounts for bonds), not percentage rates.

6. **Watch the day count on overnight rates:** Overnight rates annualize dramatically. A 1-day return multiplied by 360 gives the annual rate.

7. **INTRATE works for any fully-invested security:** Any instrument where you pay X and receive Y at maturity can use INTRATE, regardless of the instrument type.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DISC]] | Discount rate | When calculating bank discount rate (T-bill quotes) |
| [[YIELDDISC]] | Yield of discount security | Similar to INTRATE but different parameter order |
| [[RECEIVED]] | Amount received at maturity | When you know the rate and need the maturity amount |

### Commonly Used Together

**[[RECEIVED]]** - Maturity Amount

*Inverse relationship:*
```
=INTRATE(...) gives rate from amounts
=RECEIVED(...) gives amount from rate
They are inverses
```
Use to verify calculations.

---

**[[DISC]]** - Discount Rate

*Comparing conventions:*
```
DISC rate: Based on face value (bank discount)
INTRATE: Based on investment (true yield)
INTRATE > DISC always for discount securities
```
Understand both to work with money markets.

---

**[[TBILLEQ]]** - T-Bill Equivalent Yield

*T-bill specific:*
```
=TBILLEQ gives bond-equivalent yield from discount rate
=INTRATE gives investment rate from amounts
Similar concepts, different inputs
```
Both convert T-bills to yield basis.

---

**[[YIELDDISC]]** - Discount Security Yield

*Similar function:*
```
=YIELDDISC(settlement, maturity, pr, redemption, basis)
=INTRATE(settlement, maturity, investment, redemption, basis)
Same calculation, different parameter names
```
Functionally equivalent--use either.

## Official Documentation

- **Microsoft Excel:** [INTRATE function](https://support.microsoft.com/en-us/office/intrate-function-5cb34dde-a221-4cb6-b3eb-0b9e55e1316f)
- **Google Sheets:** [INTRATE function](https://support.google.com/docs/answer/3093214)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 | Integrated into core Excel from Analysis ToolPak |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
