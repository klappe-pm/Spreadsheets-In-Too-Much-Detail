---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- bond-pricing
- securities
- price-conversion
- fixed-income
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Dollar Fractional
- Decimal to Fractional Price
- Bond Price Fractional
tags:
- bond-pricing
- securities
- fixed-income
- price-conversion
- trading
---

# DOLLARFR

## Description

**DOLLARFR (Dollar Fractional)** converts a decimal dollar price into a fractional representation as used in securities markets. This function is essential for fixed-income trading where calculated prices must be converted back to the traditional fractional notation that traders, order systems, and price displays expect. DOLLARFR takes a true decimal price (like 101.5) and converts it to the compact fractional format (101.16, meaning 101 and 16/32nds) used for quoting bonds.

The fractional pricing convention is deeply embedded in fixed-income markets, particularly for U.S. Treasury securities, government agency bonds, and municipal bonds. While all calculations should be performed in decimal (for mathematical accuracy), final prices for trading, display, or reporting must often be converted back to fractional format. A trader who calculates a fair value of 99.75 needs to quote this as 99-24 (99 and 24/32nds) to match market convention.

**Understanding the fraction parameter is essential:** The second parameter tells DOLLARFR what denominator to use for the fractional representation. For Treasuries (32nds), use 32. The decimal result encodes the numerator in an unusual way: 99.24 means 99 and 24/32nds, NOT 99.24 in regular decimal notation. This is the same format used by DOLLARDE but in reverse. The function performs: `DOLLARFR(99.75, 32) = 99.24` because 0.75 = 24/32.

DOLLARFR is available in Excel (built-in since Excel 2007; Analysis ToolPak required in earlier versions) and Google Sheets. The function is the inverse of DOLLARDE and is primarily used by fixed-income trading systems, price reporting applications, and anyone who needs to display calculated prices in traditional bond market notation.

## Syntax

> [!f(x)] DOLLARFR Syntax
>
> ```
> =DOLLARFR(decimal_dollar, fraction)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `decimal_dollar` | Yes | The decimal number to convert. This is a true decimal price like 101.5 or 99.75. The decimal part will be converted to a fraction of the specified denominator. |
| `fraction` | Yes | The integer denominator for the fractional representation. Common values: 2 (halves), 4 (quarters), 8 (eighths), 16 (sixteenths), 32 (thirty-seconds), 64 (sixty-fourths). Must be a positive integer. |

### Return Value

Returns a numeric value where the integer part is the whole dollars and the decimal part represents the numerator of the fraction. For example, 101.16 with fraction=32 means 101 and 16/32nds. This is NOT a true decimal but a compact fractional notation.

## Examples

> [!f(x)] DOLLARFR Examples

### Example 1: Basic Treasury Price Conversion
```
=DOLLARFR(101.5, 32)
```
**Result:** `101.16`

**Explanation:** Converts 101.5 decimal to fractional 32nds format. Since 0.5 = 16/32, the result is 101.16 (meaning 101-16 in bond notation). This is the inverse of DOLLARDE(101.16, 32) = 101.5.

---

### Example 2: Quarter Point in 32nds
```
=DOLLARFR(99.75, 32)
```
**Result:** `99.24`

**Explanation:** Decimal 0.75 equals 24/32 (since 0.75 x 32 = 24). The result 99.24 means 99-24 in trader notation. This is a common Treasury price just below par.

---

### Example 3: At Par
```
=DOLLARFR(100.0, 32)
```
**Result:** `100`

**Explanation:** A price exactly at par (100.0 decimal) has no fractional component. The result is 100 (or 100.00), meaning 100-00 in bond notation.

---

### Example 4: Eighths for Municipal Bonds
```
=DOLLARFR(102.375, 8)
```
**Result:** `102.3`

**Explanation:** For markets using eighths, 0.375 = 3/8. The result 102.3 means 102 and 3/8ths, or 102-3 in muni bond notation. Different markets require different fraction denominators.

---

### Example 5: Quarters (Historical Stock Pricing)
```
=DOLLARFR(45.25, 4)
```
**Result:** `45.1`

**Explanation:** Before stock decimalization (2001), US stocks traded in fractions. A price of $45.25 would be 45-1/4 or 45-1. The result 45.1 encodes "45 and 1/4."

---

### Example 6: Fine Pricing in 64ths
```
=DOLLARFR(100.515625, 64)
```
**Result:** `100.33`

**Explanation:** Some markets use 64ths for finer resolution. 0.515625 = 33/64, so the result is 100.33. This might display as 100-33 or 100-16+ (since 33/64 = 16.5/32).

---

### Example 7: Below Par Premium
```
=DOLLARFR(98.125, 32)
```
**Result:** `98.04`

**Explanation:** Decimal 0.125 = 4/32 (0.125 x 32 = 4). A bond at 98-04 is trading at a discount to par, common when market yields are higher than the bond's coupon.

---

### Example 8: Converting Calculated Yield Price
```
Calculated price: =PRICE(...) returns 103.71875
Display format: =DOLLARFR(103.71875, 32)
```
**Result:** `103.23`

**Explanation:** After calculating a theoretical bond price (103.71875), convert to market format. 0.71875 = 23/32, so display as 103-23. This enables apples-to-apples comparison with dealer quotes.

---

### Example 9: Non-Exact Conversion (Rounding Required)
```
=DOLLARFR(99.80, 32)
```
**Result:** `99.256`

**Explanation:** 0.80 decimal doesn't convert exactly to 32nds. 0.80 x 32 = 25.6, so the result encodes 25.6/32. In practice, you might round: 99-26 (rounded up) or 99-25+ (99-25.5).

---

### Example 10: Halves
```
=DOLLARFR(50.5, 2)
```
**Result:** `50.1`

**Explanation:** With fraction=2, the function converts to halves. 0.5 = 1/2, so result is 50.1 meaning "50 and 1/2."

---

### Example 11: Round-Trip Verification
```
Original Decimal: 101.625
=DOLLARFR(101.625, 32) = 101.20
=DOLLARDE(101.20, 32) = 101.625
```
**Result:** Original value recovered

**Explanation:** DOLLARFR and DOLLARDE are inverse functions. Converting decimal to fractional and back should return the original value (for values that convert exactly to the given fraction denominator).

---

### Example 12: Price Spread in Fractional Format
```
Calculated Spread: =PRICE(A) - PRICE(B) = 0.25 (decimal)
Spread in 32nds: =DOLLARFR(0.25, 32) = 0.08
```
**Result:** `0.08` (meaning 8/32nds spread)

**Explanation:** Price spreads and changes can also be converted. A 0.25 decimal spread equals 8/32nds, which traders might express as "8 ticks" or "8/32."

---

### Example 13: Formatting for Trade Tickets
```
=INT(DOLLARFR(price, 32)) & "-" & RIGHT(TEXT(DOLLARFR(price, 32)-INT(DOLLARFR(price, 32)), "00"), 2)
```
**Result:** `"101-16"` (formatted string)

**Explanation:** This formula converts the DOLLARFR result into the traditional "101-16" text format seen on trade tickets. The integer part and fractional numerator are separated by a hyphen.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Fraction is 0 or negative | Fraction must be a positive integer. Cannot create fractions with zero or negative denominator. |
| `#VALUE!` | Non-numeric inputs | Ensure both parameters are numbers. Check for text formatting or hidden characters. |
| `Unexpected result` | Decimal doesn't convert evenly | Some decimals don't convert exactly to the chosen fraction. 0.80 with 32nds = 25.6/32. Decide whether to round or use finer fractions (64ths). |
| `Wrong fraction used` | Market convention mismatch | Verify you are using the correct denominator for your market (32 for Treasuries, 8 for some munis). |
| `#NAME?` | Function not recognized (older Excel) | Enable Analysis ToolPak add-in in Excel 2003 and earlier via Tools > Add-Ins. |

## Use Cases

### [[Trade Order Entry and Execution]]

**Scenario:** A trader calculates a fair value or limit price in decimal and needs to enter it into a trading system that requires fractional notation.

**Implementation:**
```
Calculated Fair Value: 99.8125 (decimal)
Order Price in 32nds: =DOLLARFR(99.8125, 32) = 99.26
```
Interpretation: Enter order as 99-26 (99 and 26/32nds)

**Business Application:** Trading platforms for Treasury bonds typically accept prices in fractional format. Traders use DOLLARFR to convert their analytical calculations to the format required for order entry. This ensures accurate execution at intended price levels.

**Technical Details:** Some trading systems accept prices with half-32nds (like 99-26+ for 99-26.5/32). To handle this, use fraction=64 and interpret the result accordingly, or use custom formatting that adds "+" for half ticks. Verify your platform's notation requirements.

---

### [[Price Display and Reporting]]

**Scenario:** A portfolio management system calculates NAV and bond prices in decimal but needs to display them in traditional fractional format for client reports.

**Implementation:**
```
Calculated Price: =cell containing 101.75
Display Price: =INT(DOLLARFR(A1, 32)) & "-" & TEXT((DOLLARFR(A1, 32) - INT(DOLLARFR(A1, 32))) * 100, "00")
```
Result: "101-24"

**Business Application:** Institutional investors and asset managers often require reports showing bond prices in traditional market notation. While internal calculations use decimals, client-facing documents should match the format traders and investors expect.

**Technical Details:** Create a custom formatting function or VBA macro for consistent display formatting. Handle edge cases like 32/32 (which should display as +1 to the integer) and half-32nds. Consider creating a lookup table for common fractional values.

---

### [[Bid-Ask Spread Analysis]]

**Scenario:** An analyst needs to express calculated bid-ask spreads in traditional tick notation for market microstructure analysis.

**Implementation:**
```
Bid (decimal): 99.75
Ask (decimal): 99.8125
Spread (decimal): =B2-B1 = 0.0625
Spread in ticks: =DOLLARFR(0.0625, 32) * 100 = 2
```
Or: 0.0625 x 32 = 2 ticks

**Business Application:** Fixed-income analysts often discuss spreads in terms of "ticks" (32nds). A 2-tick spread is 2/32nds or $0.0625 per $100 face value. Converting spreads to fractional notation facilitates communication with traders and fits market convention.

**Technical Details:** For spreads (which are differences, not prices), the DOLLARFR result's decimal part directly represents ticks. Multiply by 100 or just extract the decimal portion. Very tight spreads may require 64ths (half-tick precision).

---

### [[Regulatory and Compliance Reporting]]

**Scenario:** A broker-dealer must report trade prices to regulators in the format they were executed, which for Treasury bonds is fractional.

**Implementation:**
```
Execution Price (internal): 100.5
Reported Price: =DOLLARFR(100.5, 32) = 100.16
Reported as: "100-16"
```

**Business Application:** TRACE (Trade Reporting and Compliance Engine) and other regulatory reporting systems may require prices in their originally quoted format. DOLLARFR enables accurate conversion from internal decimal records to the required fractional reporting format.

**Technical Details:** Maintain audit trails showing both the decimal and fractional representations. Document the conversion methodology. For prices that don't convert exactly, establish rounding policies and ensure they are consistently applied and documented.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2007 and later (built-in); Excel 2003 and earlier requires Analysis ToolPak add-in
- **Specific Behavior:** Non-integer fraction parameters are truncated to integers before calculation
- **Add-in Requirement:** In older versions, enable via Tools > Add-Ins > Analysis ToolPak

### Google Sheets

- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Built-in, no add-in required.
- **Precision:** Same calculation precision as Excel

### Both Platforms

- The result is NOT a true decimal but a compact fractional notation
- The integer part of the result is the whole dollar amount
- The decimal part of the result is the numerator divided by 100 (for display)
- DOLLARDE performs the inverse conversion
- Non-exact conversions produce fractional numerators (like 25.6)

## Tips and Best Practices

1. **Understand the output format:** DOLLARFR output like 99.24 means 99 and 24/32nds, NOT 99.24 in regular decimal. The decimal part encodes the fraction numerator, not a true decimal value.

2. **Match your market convention:** Use the correct fraction denominator for your market. Treasuries use 32, some munis use 8, mortgage-backed securities may use 32 or 64. Using the wrong denominator produces meaningless results.

3. **Handle non-exact conversions:** When the decimal doesn't convert exactly (e.g., 0.80 to 32nds = 25.6), decide whether to round (to 26) or use finer fractions (64ths). Document your rounding policy.

4. **Validate with DOLLARDE:** For critical applications, verify conversions by round-tripping: `DOLLARDE(DOLLARFR(value, 32), 32)` should return the original value for exact conversions.

5. **Create display formatting formulas:** Build helper formulas or functions to convert DOLLARFR output to traditional display format (101-16) for reports and trade tickets.

6. **Handle edge cases:** A decimal of exactly 1.0 for the fractional part (like 100.0 to 32nds from 101.0) should yield 101-00, not 100-32. Test your formulas with edge cases.

7. **Document the fraction parameter:** In spreadsheets with multiple bond types, clearly document which fraction denominator applies to each security type to avoid errors.

8. **Consider half-tick notation:** For half-32nds (common in active Treasury trading), either use fraction=64 or develop custom handling that shows "+" for half ticks (99-16+ for 99-16.5).

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DOLLARDE]] | Converts fractional dollar to decimal | When you have fractional prices and need true decimal values for calculations |
| [[ROUND]] | Rounds to specified decimal places | When you need to round the fractional numerator to a whole number |
| [[INT]] | Returns integer portion | When you need only the whole dollar amount from the result |
| [[MOD]] | Returns remainder | Useful for extracting just the fractional portion of the result |

### Commonly Used Together

**[[DOLLARDE]]** - Dollar Decimal

*Inverse function for verification:*
```
Decimal: 99.75
Fractional: =DOLLARFR(99.75, 32) = 99.24
Back to Decimal: =DOLLARDE(99.24, 32) = 99.75
```
Use DOLLARDE to verify conversions and for calculations that require decimal precision.

---

**[[PRICE]]** - Bond Price Calculation

*Convert calculated prices to market format:*
```
Theoretical Price: =PRICE(settlement, maturity, coupon, yield, redemption, frequency) = 101.625
Market Format: =DOLLARFR(101.625, 32) = 101.20
Display: "101-20"
```
Convert calculated theoretical prices to quotable market format.

---

**[[ROUND]]** - Rounding

*Round fractional numerator to nearest whole tick:*
```
Raw Conversion: =DOLLARFR(99.80, 32) = 99.256
Whole Tick: =INT(DOLLARFR(99.80, 32)) + ROUND((DOLLARFR(99.80, 32) - INT(DOLLARFR(99.80, 32))) * 100, 0) / 100
```
Create formulas to round to the nearest tradable tick.

---

**[[TEXT]]** - Formatting

*Format for display:*
```
=TEXT(INT(DOLLARFR(A1, 32)), "0") & "-" & TEXT(ROUND((DOLLARFR(A1, 32) - INT(DOLLARFR(A1, 32))) * 100, 0), "00")
```
Convert DOLLARFR output to traditional "101-16" text format for reports.

---

**[[YIELD]]** - Bond Yield

*Work backward from market price:*
```
Market Quote: 99.16 (fractional)
Decimal Price: =DOLLARDE(99.16, 32) = 99.5
Calculate Yield: =YIELD(..., 99.5, ...)
```
Convert market quotes to decimal for yield calculations, use DOLLARFR to convert back for display.

## Official Documentation

- **Microsoft Excel:** [DOLLARFR function](https://support.microsoft.com/en-us/office/dollarfr-function-0835d163-3023-4a33-9824-3042c5d4f495)
- **Google Sheets:** [DOLLARFR function](https://support.google.com/docs/answer/3093220)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 (2007) | Built-in function; previously required Analysis ToolPak |
| Excel 2003 and earlier | Analysis ToolPak add-in | Required manual activation of add-in |
| Google Sheets | Original launch | Built-in support from the beginning |

---

*Last updated: 2026-01-10*
