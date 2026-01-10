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
- Dollar Decimal
- Fractional to Decimal Price
- Bond Price Decimal
tags:
- bond-pricing
- securities
- fixed-income
- price-conversion
- trading
---

# DOLLARDE

## Description

**DOLLARDE (Dollar Decimal)** converts a dollar price expressed as a fraction into a decimal number. This function is essential for fixed-income securities trading where bond prices are traditionally quoted in fractional format (like 99-16 meaning 99 and 16/32nds) but calculations require decimal values. DOLLARDE takes a fractional price where the integer part is the whole dollar amount and the decimal part represents the numerator of the fraction, then converts it to a true decimal price.

The fractional pricing convention dates back centuries in bond markets and remains the standard for U.S. Treasury securities, municipal bonds, and some corporate bonds. A price of 101.16 in fractional notation (with 32nds as the fraction denominator) means 101 + 16/32 = 101.5 in decimal. The challenge is that this "decimal" representation (101.16) is NOT a true decimal--it is a compact way to write a fraction. DOLLARDE performs the conversion: `DOLLARDE(101.16, 32) = 101.5`. Without this conversion, arithmetic operations on bond prices would produce incorrect results.

**Understanding the fraction parameter is critical:** The second parameter specifies what the fractional part represents. For 32nds (standard for Treasuries), use 32. For 8ths (some municipal bonds), use 8. For 100ths (already decimal), use 100. If your fractional price is 99.04 and fraction is 8, this means 99 and 4/8ths = 99.5. If fraction is 32, the same 99.04 means 99 and 4/32nds = 99.125. Getting this parameter wrong completely changes your result.

DOLLARDE is available in Excel (built-in since Excel 2007; Analysis ToolPak required in earlier versions) and Google Sheets. The function is primarily used by fixed-income traders, portfolio managers, and anyone working with bond pricing data imported from trading systems that use fractional notation. Note that DOLLARFR performs the reverse conversion (decimal to fractional).

## Syntax

> [!f(x)] DOLLARDE Syntax
>
> ```
> =DOLLARDE(fractional_dollar, fraction)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `fractional_dollar` | Yes | The number to convert, expressed as an integer followed by a decimal. The integer part is the whole dollars; the decimal part is the numerator of the fraction. For example, 101.16 represents 101 and 16/fraction. |
| `fraction` | Yes | The integer denominator of the fraction. Common values: 2 (halves), 4 (quarters), 8 (eighths), 16 (sixteenths), 32 (thirty-seconds), 64 (sixty-fourths). Must be a positive integer. |

### Return Value

Returns a numeric value representing the decimal equivalent of the fractional dollar price. The result is a true decimal number suitable for arithmetic operations.

## Examples

> [!f(x)] DOLLARDE Examples

### Example 1: Basic Treasury Price Conversion (32nds)
```
=DOLLARDE(101.16, 32)
```
**Result:** `101.5`

**Explanation:** Converts a Treasury bond price quoted as 101-16 (101 and 16/32nds) to decimal. The calculation: 101 + (16/32) = 101 + 0.5 = 101.5. This is the standard conversion for U.S. Treasury securities.

---

### Example 2: Below Par Bond Price
```
=DOLLARDE(98.24, 32)
```
**Result:** `98.75`

**Explanation:** A bond trading at 98-24 (below par value of 100) converts to 98.75 in decimal. Calculation: 98 + (24/32) = 98 + 0.75 = 98.75. This bond costs $987.50 per $1,000 face value.

---

### Example 3: Municipal Bond (8ths)
```
=DOLLARDE(102.6, 8)
```
**Result:** `102.75`

**Explanation:** Some municipal bonds quote in eighths. A price of 102-6 (102 and 6/8ths) converts to: 102 + (6/8) = 102 + 0.75 = 102.75. Know your market's quoting convention before using DOLLARDE.

---

### Example 4: Half-Point Increments
```
=DOLLARDE(100.1, 2)
```
**Result:** `100.5`

**Explanation:** With fraction = 2, the decimal part represents halves. 100.1 means 100 and 1/2 = 100.5. This might be used for securities quoted in half-point increments.

---

### Example 5: Quarter-Point Pricing
```
=DOLLARDE(99.3, 4)
```
**Result:** `99.75`

**Explanation:** A price quoted in quarters. 99.3 means 99 and 3/4 = 99.75. Stock prices historically used this convention before decimalization in 2001.

---

### Example 6: 64ths for Fine Pricing
```
=DOLLARDE(100.32, 64)
```
**Result:** `100.5`

**Explanation:** Some markets use 64ths for finer price resolution. 100.32 with fraction=64 means 100 + (32/64) = 100.5. Note: 32/64 simplifies to 1/2.

---

### Example 7: Price at Par
```
=DOLLARDE(100.00, 32)
```
**Result:** `100`

**Explanation:** A bond at par (100-00) has no fractional component. 100 + (0/32) = 100 exactly. The DOLLARDE function correctly returns 100.

---

### Example 8: Maximum Fraction Value
```
=DOLLARDE(99.31, 32)
```
**Result:** `99.96875`

**Explanation:** 99-31 is just below 100 (since 32/32 = 1). Calculation: 99 + (31/32) = 99 + 0.96875 = 99.96875. This shows the precision possible with 32nds.

---

### Example 9: Converting Price for Yield Calculation
```
=YIELD(settlement, maturity, coupon, DOLLARDE(98.16, 32), redemption, frequency)
```
**Result:** Depends on other parameters

**Explanation:** When importing bond prices from a trading system that uses fractional notation, convert to decimal before using in yield calculations. The YIELD function requires decimal price input.

---

### Example 10: Price Difference Calculation
```
=DOLLARDE(101.24, 32) - DOLLARDE(101.16, 32)
```
**Result:** `0.25`

**Explanation:** Calculate the price change between two Treasury quotes. From 101-16 to 101-24 is an increase of 8/32nds = 0.25 points. On a $1 million position, this represents $2,500.

---

### Example 11: Dollar Value Calculation
```
=DOLLARDE(102.08, 32) * 1000000 / 100
```
**Result:** `$1,022,500`

**Explanation:** Calculate the dollar value of $1 million face value of bonds at 102-08. First convert to decimal (102.25), then multiply by face value and divide by 100 (since bonds are quoted per $100 face).

---

### Example 12: Handling Fractional Numerators (Plus Notation)
```
=DOLLARDE(99.164, 32)
```
**Result:** `99.5125`

**Explanation:** Some systems use extra decimal places to indicate half-32nds. 99.164 could represent 99-16+ (99 and 16.4/32 = 99 and 16.5/32). DOLLARDE treats .164 as 16.4/32 = 0.5125. Always verify your data source's notation convention.

---

### Example 13: Comparing Fractional and Decimal Prices
```
Fractional: 100.16 (meaning 100-16)
=DOLLARDE(100.16, 32) = 100.5
=DOLLARFR(100.5, 32) = 100.16 (converts back)
```
**Result:** Bidirectional conversion verified

**Explanation:** DOLLARDE and DOLLARFR are inverse functions. Converting fractional to decimal and back should return the original value (subject to rounding for non-standard inputs).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Fraction is 0 or negative | Fraction must be a positive integer. You cannot divide by zero or negative numbers. |
| `#VALUE!` | Non-numeric inputs | Ensure both parameters are numbers, not text. Check for hidden characters or formatting. |
| `#DIV/0!` | Fraction is 0 | Fraction cannot be 0. Use at least 1 (though 1 would just return the input unchanged). |
| `Wrong result` | Incorrect fraction denominator | Verify you are using the correct fraction for your market (32 for Treasuries, 8 for some munis, etc.). |
| `Unexpected decimals` | Fractional numerator exceeds denominator | If fractional_dollar has a decimal part >= fraction, the result may be unexpected. 100.35 with fraction 32 means 35/32 = 1.09375 extra, so result is 101.09375. |
| `#NAME?` | Function not recognized (older Excel) | Enable Analysis ToolPak add-in in Excel 2003 and earlier via Tools > Add-Ins. |

## Use Cases

### [[Treasury Bond Trading Desk Operations]]

**Scenario:** A fixed-income trader receives price quotes in traditional 32nds format from multiple dealers and needs to compare them accurately and calculate trade values.

**Implementation:**
```
Dealer A Price: =DOLLARDE(99.28, 32) = 99.875
Dealer B Price: =DOLLARDE(99.24, 32) = 99.75
Price Difference: =DOLLARDE(99.28, 32) - DOLLARDE(99.24, 32) = 0.125 (4/32nds)
Trade Value ($10M face): =DOLLARDE(99.28, 32) * 10000000 / 100 = $9,987,500
```

**Business Application:** Treasury bond traders work with fractional prices throughout the day. Converting to decimal enables proper comparison, weighted average calculations, and trade value computations. Many trading systems store data in fractional format, but analysis requires decimal precision.

**Technical Details:** Treasury prices can be quoted to half-32nds (64ths) or even quarter-32nds (128ths). A price of 99-16+ means 99 and 16.5/32nds. To handle this in DOLLARDE, input 99.165 (treating the trailing digit as tenths of a 32nd). Some systems use 99.16 for 99-16 and 99.165 for 99-16+.

---

### [[Bond Portfolio Valuation]]

**Scenario:** A portfolio manager needs to value a portfolio of municipal and Treasury bonds where some positions are quoted in 8ths and others in 32nds.

**Implementation:**
```
Position 1 (Treasury): =DOLLARDE(B2, 32) * C2 / 100
Position 2 (Muni): =DOLLARDE(B3, 8) * C3 / 100
Position 3 (Treasury): =DOLLARDE(B4, 32) * C4 / 100
Total Portfolio Value: =SUM(D2:D4)
```
Where B = fractional price, C = face value, D = calculated market value

**Business Application:** End-of-day portfolio valuation requires consistent decimal pricing across all holdings. Different bond sectors use different fractional conventions. DOLLARDE normalizes everything to decimal for accurate Net Asset Value (NAV) calculations, performance reporting, and risk analysis.

**Technical Details:** Build a lookup table or VLOOKUP that maps security types to their fractional convention (Treasury = 32, Muni = 8, etc.). Use this in the DOLLARDE formula: `=DOLLARDE(price, VLOOKUP(type, convention_table, 2, FALSE))` for automated processing.

---

### [[Historical Price Analysis and Backtesting]]

**Scenario:** A quantitative analyst is backtesting a trading strategy using historical Treasury prices stored in fractional format.

**Implementation:**
```
Array formula to convert entire price column:
=DOLLARDE(A:A, 32)

Calculate returns:
=(DOLLARDE(A2, 32) - DOLLARDE(A1, 32)) / DOLLARDE(A1, 32)
```

**Business Application:** Historical databases often store prices in their original quoted format (fractional). Before running statistical analysis, regression, or strategy backtests, all prices must be converted to decimal. DOLLARDE enables batch conversion of historical datasets.

**Technical Details:** When processing large datasets, consider converting fractional prices to decimal once and storing the results in a separate column rather than recalculating repeatedly. This improves spreadsheet performance. For very large datasets, database-level conversion (using equivalent SQL functions) may be more efficient.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2007 and later (built-in); Excel 2003 and earlier requires Analysis ToolPak add-in
- **Specific Behavior:** Accepts non-integer fraction values but rounds to integer before calculation
- **Add-in Requirement:** In older Excel versions, enable via Tools > Add-Ins > Analysis ToolPak

### Google Sheets

- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Built-in, no add-in required.
- **Precision:** Same calculation precision as Excel

### Both Platforms

- The fractional_dollar parameter is interpreted as: integer part = whole dollars, decimal part = fraction numerator
- Fraction must be a positive integer (or is truncated to integer)
- Result is a true decimal number
- DOLLARFR performs the inverse conversion

## Tips and Best Practices

1. **Know your market convention:** Different bond markets use different fractional denominators. Treasuries use 32nds, some munis use 8ths, and stocks (pre-2001) used 8ths or 16ths. Using the wrong denominator produces incorrect prices.

2. **Handle plus notation carefully:** A price like 99-16+ (meaning 99-16.5/32) might be entered as 99.165. Verify how your data source represents half-32nds before processing.

3. **Validate conversions with DOLLARFR:** For critical calculations, convert back using DOLLARFR to verify your decimal result is correct: `DOLLARFR(DOLLARDE(99.16, 32), 32)` should return 99.16.

4. **Watch for data format issues:** Imported price data may be stored as text. Convert to numbers first using VALUE() or formatting before using DOLLARDE.

5. **Remember: this is NOT regular decimal math:** 100.16 in bond fractional notation is NOT equal to 100.16 in decimal. The .16 means 16/32 = 0.5, so the true decimal is 100.5.

6. **Use for price calculations, then convert back for display:** Calculate portfolio values, yields, and durations in decimal. If you need to report prices in traditional fractional format, convert back using DOLLARFR.

7. **Handle edge cases:** A fractional part of .32 with fraction=32 means 32/32 = 1, so 99.32 converts to 100. This can occur with data entry errors or rounding.

8. **Create named ranges for clarity:** Instead of hardcoding 32 throughout your spreadsheet, create a named range like "TREASURY_FRACTION = 32" for clearer, more maintainable formulas.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DOLLARFR]] | Converts decimal dollar to fractional format | When you need to convert decimal prices back to fractional notation for trading or display |
| [[ROUND]] | Rounds to specified decimal places | When you need to round the decimal result to specific precision |
| [[TRUNC]] | Truncates to integer | If you need only the whole dollar amount from the result |

### Commonly Used Together

**[[DOLLARFR]]** - Dollar Fractional

*Round-trip conversion to verify accuracy:*
```
Original: 101.16 (meaning 101-16/32)
To Decimal: =DOLLARDE(101.16, 32) = 101.5
Back to Fractional: =DOLLARFR(101.5, 32) = 101.16
```
Use DOLLARFR to convert calculated decimal prices back to market convention for order entry or reporting.

---

**[[YIELD]]** - Bond Yield

*Calculate yield using converted price:*
```
=YIELD(settlement, maturity, coupon, DOLLARDE(price, 32), redemption, frequency)
```
Bond yield calculations require decimal price input. Convert fractional prices before using in YIELD.

---

**[[PRICE]]** - Bond Price

*Compare calculated vs. market price:*
```
Market Price (decimal): =DOLLARDE(quoted_price, 32)
Theoretical Price: =PRICE(settlement, maturity, coupon, yield, redemption, frequency)
Rich/Cheap: =Market_Price - Theoretical_Price
```
Determine if a bond is trading rich (above theoretical) or cheap (below theoretical).

---

**[[SUM]]** / **[[SUMPRODUCT]]** - Aggregation

*Calculate total portfolio value:*
```
=SUMPRODUCT(DOLLARDE(prices, 32), face_values) / 100
```
Aggregate portfolio market values after converting all prices to decimal.

## Official Documentation

- **Microsoft Excel:** [DOLLARDE function](https://support.microsoft.com/en-us/office/dollarde-function-db85aab0-1677-428a-9dfd-a38476693427)
- **Google Sheets:** [DOLLARDE function](https://support.google.com/docs/answer/3093219)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 (2007) | Built-in function; previously required Analysis ToolPak |
| Excel 2003 and earlier | Analysis ToolPak add-in | Required manual activation of add-in |
| Google Sheets | Original launch | Built-in support from the beginning |

---

*Last updated: 2026-01-10*
