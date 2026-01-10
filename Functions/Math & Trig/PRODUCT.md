---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- multiplication
- aggregate
- product
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Multiply Range
- Product Function
- Multiply All
tags:
- multiplication
- product
- aggregate
- multiply
- range-calculation
---

# PRODUCT

## Description

**PRODUCT** multiplies all the numbers given as arguments and returns the result. While you can multiply numbers with the asterisk operator (A1*A2*A3), PRODUCT shines when you need to multiply entire ranges or variable numbers of values. PRODUCT(A1:A10) multiplies all ten values together in a single elegant formula, something that would require nine asterisks otherwise.

This function is essential for **compound calculations, factorial-like operations, growth rate analysis, and any scenario where multiple values must be multiplied together**. Unlike SUM which adds values, PRODUCT compounds them multiplicatively. A 10% increase followed by a 20% increase is not a 30% increase—it's PRODUCT(1.1, 1.2) = 1.32 or 32% total. This multiplicative compounding is fundamental to finance, probability, and growth modeling.

**PRODUCT handles mixed input gracefully.** It accepts individual numbers, cell references, ranges, and arrays. Critically, PRODUCT ignores text and logical values (TRUE/FALSE) when they appear in ranges, but converts them to numbers when entered directly as arguments. Empty cells are ignored, not treated as zeros—this is important because a single zero in PRODUCT makes the entire result zero.

**Watch out for zeros and very small/large numbers.** Any zero in the arguments makes PRODUCT return zero. Very small numbers can produce underflow (result becomes zero), while very large numbers can overflow to #NUM! error. For products of many small probabilities, consider working with logarithms (summing logs, then exponentiating) to avoid precision loss.

## Syntax

> [!f(x)] PRODUCT Syntax
>
> ```
> =PRODUCT(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first number, cell reference, or range to multiply. This is the starting value for the product calculation. |
| `number2, ...` | No | Additional numbers, cell references, or ranges to include in the product. Up to 255 arguments can be provided. Each value is multiplied with the others. |

### Return Value

Returns the product (multiplication result) of all numeric arguments. Returns 0 if any argument is zero. Returns #VALUE! if any directly-specified argument cannot be interpreted as a number. Text and logical values in cell references are ignored.

## Examples

> [!f(x)] PRODUCT Examples

### Example 1: Basic Multiplication
```
=PRODUCT(2, 3, 4)
```
**Result:** 24

**Explanation:** Multiplies 2 x 3 x 4 = 24. Equivalent to =2*3*4 but more readable when you have many values.

---

### Example 2: Multiply a Range
```
=PRODUCT(A1:A5)
```
**Result:** Product of all values in A1 through A5

**Explanation:** If A1:A5 contains {2, 3, 4, 5, 6}, the result is 2 x 3 x 4 x 5 x 6 = 720. Much cleaner than =A1*A2*A3*A4*A5.

---

### Example 3: Compound Growth Calculation
```
=PRODUCT(1.05, 1.08, 1.03, 1.06)
```
**Result:** 1.2388... (approximately 23.88% total growth)

**Explanation:** Calculates cumulative growth from four periods with 5%, 8%, 3%, and 6% growth rates. This demonstrates that growth rates compound, not add—total is 23.88%, not 22%.

---

### Example 4: Mixed Arguments
```
=PRODUCT(A1:A3, 10, B1:B2)
```
**Result:** Product of A1:A3 multiplied by 10 multiplied by B1:B2

**Explanation:** PRODUCT accepts any combination of individual values and ranges. All numeric values are multiplied together.

---

### Example 5: Effect of Zero
```
=PRODUCT(5, 10, 0, 15)
```
**Result:** 0

**Explanation:** Any zero in the arguments makes the entire product zero. This is mathematically correct but can be surprising if you have data with missing values coded as zeros.

---

### Example 6: Empty Cells Are Ignored
```
=PRODUCT(A1:A5)
```
Where A3 is empty:
**Result:** Product of A1, A2, A4, A5 (A3 is skipped)

**Explanation:** Empty cells don't contribute to the product—they're not treated as zero. This behavior differs from explicit zeros and is often what you want.

---

### Example 7: Calculate Factorial Manually
```
=PRODUCT(1, 2, 3, 4, 5)
```
**Result:** 120 (which is 5!)

**Explanation:** While Excel has FACT for factorials, this shows how PRODUCT can compute 5! = 1 x 2 x 3 x 4 x 5 = 120.

---

### Example 8: Investment Return Calculation
```
=PRODUCT(1 + A1:A12) - 1
```
**Result:** Total return from monthly percentage returns

**Explanation:** If A1:A12 contains monthly returns as decimals (0.02 for 2%), this calculates the compound annual return. Subtract 1 to get just the return portion.

---

### Example 9: Probability of Independent Events
```
=PRODUCT(0.9, 0.85, 0.95, 0.92)
```
**Result:** 0.6699... (about 67%)

**Explanation:** Probability of four independent events all occurring (90%, 85%, 95%, 92% chance each) is their product: 67% combined probability.

---

### Example 10: Unit Conversion Chain
```
=PRODUCT(Meters, 3.28084, 12)
```
**Result:** Meters converted to inches

**Explanation:** Chain conversions by multiplying factors: meters to feet (3.28084), feet to inches (12). PRODUCT makes multi-step conversions readable.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric direct argument | Ensure all explicitly typed arguments are numbers. Text in direct arguments causes errors; text in ranges is simply ignored. |
| `#NUM!` | Result too large | Product exceeds Excel's numeric limit (~1.8 x 10^308). Work with logarithms for very large products or break into smaller calculations. |
| `0` (unexpected) | Zero in arguments | Check for zeros in your data. A single zero makes the entire product zero. Consider IF logic to handle zeros. |
| `#NAME?` | Misspelled function name | Verify spelling: PRODUCT, not PROD or MULTIPLY. |
| `Result of 1` | Empty range or all non-numeric | If range contains only text/blanks, PRODUCT returns 1 (empty product). Verify range contains numeric data. |
| `Different from manual` | Floating-point precision | Very large or very small products may lose precision. PRODUCT(0.1, 0.1, 0.1) might not exactly equal 0.001. |

## Use Cases

### [[Compound Interest and Growth Calculations]]

**Scenario:** Calculate cumulative investment returns or growth over multiple periods.

**Implementation:**
```
=PRODUCT(1 + MonthlyReturns) - 1           → Total return from monthly data
=InitialValue * PRODUCT(1 + GrowthRates)   → Final value after growth
=PRODUCT(1 + A1:A12)^(1/12) - 1            → Average monthly return
```

**Business Application:** Investment performance analysis, portfolio returns, revenue growth tracking, population modeling. Any scenario where percentage changes compound over time.

**Technical Details:** Always add 1 to percentage returns before multiplying, then subtract 1 from the result. A 10% return is multiplied as 1.10, not 0.10. This represents the total value multiplier, not just the gain.

---

### [[Probability Calculations]]

**Scenario:** Calculate the probability of multiple independent events all occurring.

**Implementation:**
```
=PRODUCT(EventProbabilities)               → Combined probability
=1 - PRODUCT(1 - FailureRates)             → System reliability
=PRODUCT(0.99^ComponentCounts)             → Multi-component reliability
```

**Business Application:** Risk assessment, quality control, reliability engineering, game design, insurance calculations. Essential for any probability model with independent events.

**Technical Details:** This applies only to independent events. For dependent events, you need conditional probabilities. Probabilities must be between 0 and 1. Very small probabilities multiplied together can underflow to zero—use log sums for extreme cases.

---

### [[Dimensional Analysis and Unit Conversion]]

**Scenario:** Convert units through chains of conversion factors.

**Implementation:**
```
=PRODUCT(Value, 0.453592, 1000)            → Pounds to grams (lb → kg → g)
=PRODUCT(Gallons, 3.78541, 1000, 1000)     → Gallons to milliliters
=PRODUCT(A1, ConversionFactors)            → Apply multiple conversion factors
```

**Business Application:** Scientific calculations, engineering conversions, international commerce, recipe scaling. Any multi-step conversion benefits from PRODUCT's clarity.

**Technical Details:** List conversion factors in order of application. Each factor multiplies the previous result. This approach is self-documenting and easy to verify compared to one complex multiplied expression.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Argument limit:** Up to 255 arguments
- **Array support:** Works with arrays and dynamic arrays in Excel 365
- **Text handling:** Text in ranges is ignored; text as direct argument causes #VALUE!
- **Empty cells:** Ignored (not treated as zero)

### Google Sheets
- **Availability:** All versions
- **Argument limit:** Similar to Excel, effectively unlimited for practical purposes
- **Array support:** Works with ARRAYFORMULA context
- **Text handling:** Same as Excel
- **Empty cells:** Same behavior—ignored

### Both Platforms
- Identical numerical results for same inputs
- Same handling of empty cells, text, and logical values
- Same error conditions and error types
- Both return 1 for empty product (no valid numeric arguments)

## Tips and Best Practices

1. **Watch for zeros:** PRODUCT returns 0 if any value is zero. If zeros represent missing data, filter them out or use a formula like `=PRODUCT(IF(Range<>0, Range, 1))` to treat zeros as ones.

2. **Add 1 for percentage calculations:** When compounding returns or growth rates, remember to add 1 before multiplying. PRODUCT(0.05, 0.08) gives 0.004, but PRODUCT(1.05, 1.08) gives 1.134 (13.4% total growth).

3. **Use PRODUCT for readability:** Even when you could use asterisks, PRODUCT(A:A) is clearer than A1*A2*A3*...*A100. Self-documenting code is worth the extra characters.

4. **Empty product equals 1:** PRODUCT with no valid numeric arguments returns 1, the multiplicative identity. This is mathematically correct but can be confusing if your range is unexpectedly empty.

5. **Consider logarithms for extreme values:** For products of many small probabilities or very large numbers, sum the logarithms instead: `=EXP(SUM(LN(Range)))` is mathematically equivalent but avoids overflow/underflow.

6. **Combine with SEQUENCE for factorials:** `=PRODUCT(SEQUENCE(n))` calculates n! in modern Excel/Sheets with dynamic arrays, though FACT(n) is more direct.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SUM]] | Adds values together | When you need addition, not multiplication |
| [[SUMPRODUCT]] | Multiplies corresponding elements then sums | When you need to multiply arrays element-wise and sum the results |
| [[POWER]] | Raises to a power | When multiplying the same value repeatedly (x^n vs PRODUCT of n copies of x) |

### Commonly Used Together

**[[SUM]]** - Combine additive and multiplicative aggregation

*Compare sum vs product behavior:*
```
=SUM(A1:A10)      → Additive total
=PRODUCT(A1:A10)  → Multiplicative total
```
Different aggregation methods for different analyses.

---

**[[LN]]** - Convert product to sum for large calculations

*Avoid overflow with logarithms:*
```
=EXP(SUM(LN(A1:A100)))
```
Mathematically equivalent to PRODUCT but handles extreme values better.

---

**[[IF]]** - Conditional product to handle zeros

*Exclude zeros from product:*
```
=PRODUCT(IF(A1:A10<>0, A1:A10, 1))
```
Replaces zeros with 1 so they don't zero out the product.

---

**[[SEQUENCE]]** - Generate values for product

*Calculate factorial:*
```
=PRODUCT(SEQUENCE(5))  → 120 (5!)
```
Dynamic array approach to factorial calculation.

---

**[[GEOMEAN]]** - Geometric mean from products

*Average multiplicative factor:*
```
=PRODUCT(GrowthFactors)^(1/COUNT(GrowthFactors))
```
GEOMEAN is equivalent but more direct for this purpose.

## Official Documentation

- **Microsoft Excel:** [PRODUCT function](https://support.microsoft.com/en-us/office/product-function-8e6b5b24-90ee-4650-aeec-80982a0512ce)
- **Google Sheets:** [PRODUCT function](https://support.google.com/docs/answer/3093502)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Excel 365 | Dynamic arrays | Works with spilled ranges |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
