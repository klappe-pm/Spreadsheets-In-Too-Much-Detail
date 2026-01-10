---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- geometric-mean
- average
- growth-rate
- multiplicative-data
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Geometric Mean
- Geometric Average
- Multiplicative Mean
- Growth Rate Average
tags:
- statistical
- average
- geometric
- growth-rate
- financial
- investment
---

# GEOMEAN

## Description

**GEOMEAN** calculates the geometric mean of a set of positive numbers, which is the nth root of the product of n values. Unlike the arithmetic mean (simple average), the geometric mean is appropriate for data that is multiplicative in nature, such as growth rates, ratios, percentages, and financial returns. For n values, the geometric mean equals (x1 * x2 * ... * xn)^(1/n), which can also be computed as EXP(AVERAGE(LN(values))).

The geometric mean is always less than or equal to the arithmetic mean (equality only when all values are identical), making it a more conservative measure. This property is crucial for averaging rates of change: if an investment grows by 100% one year (doubles) and shrinks by 50% the next (halves), the arithmetic mean suggests 25% average growth, but the geometric mean correctly shows 0% (you end up where you started). GEOMEAN requires all values to be positive - any zero or negative value causes an error.

**Why geometric mean matters:** Compound growth is multiplicative, not additive. When averaging annual returns like +20%, -10%, +15%, converting to growth factors (1.20, 0.90, 1.15), taking their geometric mean, and subtracting 1 gives the true average annual return that, if compounded, produces the same final value. This is fundamental in finance (investment returns), biology (population growth rates), and any field involving proportional change.

**Important constraints:** GEOMEAN requires strictly positive values. Zero values produce #NUM! because you cannot take the nth root of zero in this context. Negative values also produce #NUM! because they would require complex number roots. For data with zeros or negatives, consider transformations (adding a constant) or using the arithmetic mean with appropriate interpretation.

## Syntax

> [!f(x)] GEOMEAN Syntax
>
> ```
> =GEOMEAN(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first positive number or range of positive numbers. |
| `number2, ...` | No | Additional positive numbers or ranges. Up to 255 arguments total. |

### Return Value

Returns the geometric mean of all positive numeric values. Empty cells and text are ignored. Returns #NUM! if any argument is zero or negative. Returns #VALUE! if non-numeric text is passed directly as an argument.

## Examples

> [!f(x)] GEOMEAN Examples

### Example 1: Simple Geometric Mean
```
=GEOMEAN(2, 8)
```
**Result:** 4

**Explanation:** Geometric mean of 2 and 8 = sqrt(2 * 8) = sqrt(16) = 4. The geometric mean of two numbers is their square root of product.

---

### Example 2: Three Values
```
=GEOMEAN(1, 3, 9)
```
**Result:** 3

**Explanation:** Geometric mean = (1 * 3 * 9)^(1/3) = 27^(1/3) = 3. For a geometric sequence (1, 3, 9), the geometric mean equals the middle value.

---

### Example 3: Investment Returns (Growth Factors)
```
=GEOMEAN(1.10, 1.05, 0.95, 1.20)
```
**Result:** 1.071 (approximately)

**Explanation:** Four years with returns +10%, +5%, -5%, +20%. Growth factors: 1.10, 1.05, 0.95, 1.20. Geometric mean = 1.071, so average annual return is about 7.1%.

---

### Example 4: Comparing with Arithmetic Mean
```
Geometric: =GEOMEAN(2, 8)          Result: 4
Arithmetic: =AVERAGE(2, 8)         Result: 5
```
**Result:** Geometric mean (4) < Arithmetic mean (5)

**Explanation:** The geometric mean is always less than or equal to the arithmetic mean. This is the AM-GM inequality.

---

### Example 5: Range of Values
```
=GEOMEAN(A1:A10)
```
Where A1:A10 contains ten positive numbers

**Result:** Geometric mean of all values in range

**Explanation:** GEOMEAN accepts ranges, making it easy to calculate geometric mean of a column of data.

---

### Example 6: Value with Zero (Error)
```
=GEOMEAN(5, 0, 10)
```
**Result:** #NUM!

**Explanation:** Zero values cause an error. The geometric mean is undefined when any value is zero (product becomes zero, nth root is zero, but interpretation fails).

---

### Example 7: Negative Value (Error)
```
=GEOMEAN(4, -2, 8)
```
**Result:** #NUM!

**Explanation:** Negative values cause an error. Even roots of negative numbers aren't real, so geometric mean is undefined for negative data.

---

### Example 8: All Equal Values
```
=GEOMEAN(5, 5, 5, 5)
```
**Result:** 5

**Explanation:** When all values are identical, geometric mean equals that value. This is the only case where geometric mean equals arithmetic mean.

---

### Example 9: Equivalent Calculation with LN and EXP
```
=GEOMEAN(A1:A10)
=EXP(AVERAGE(LN(A1:A10)))
```
**Result:** Both return the same value

**Explanation:** Geometric mean can be computed as exp of the arithmetic mean of logarithms. This is the mathematical foundation.

---

### Example 10: Population Growth Rates
```
Growth rates: 5%, 10%, 8%, 12%
Factors: 1.05, 1.10, 1.08, 1.12
=GEOMEAN(1.05, 1.10, 1.08, 1.12)
```
**Result:** 1.0873 (approximately 8.73% average growth)

**Explanation:** To find average compound growth rate, take geometric mean of growth factors and subtract 1.

---

### Example 11: Index Numbers
```
Index values (base 100): 100, 105, 112, 108, 115
=GEOMEAN(100, 105, 112, 108, 115)
```
**Result:** 107.8 (approximately)

**Explanation:** Geometric mean of index numbers gives the central tendency that preserves multiplicative relationships.

---

### Example 12: Financial Portfolio CAGR
```
Year-end values: 10000, 11500, 10350, 12420, 14283
Growth factors: 1.15, 0.90, 1.20, 1.15
=GEOMEAN(1.15, 0.90, 1.20, 1.15) - 1
```
**Result:** 0.093 or 9.3% (CAGR - Compound Annual Growth Rate)

**Explanation:** The geometric mean of year-over-year growth factors, minus 1, gives the CAGR. This is the constant annual return that would produce the same final value.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | One or more values is zero | Remove zeros or add a small constant to all values. |
| `#NUM!` | One or more values is negative | Geometric mean requires positive values. Transform data or use different measure. |
| `#VALUE!` | Non-numeric text argument | Ensure all direct arguments are numbers. Text in ranges is ignored. |
| `#DIV/0!` | Empty range | Provide at least one positive number. |
| `Result seems too low` | Outliers near zero | Geometric mean is heavily influenced by small values. Check data for errors. |

## Use Cases

### [[Investment Performance: CAGR Calculation]]

**Scenario:** A financial analyst calculates the Compound Annual Growth Rate of an investment over multiple years with varying returns.

**Implementation:**
```
Annual returns: +20%, -15%, +25%, +10%, -5%
Growth factors: 1.20, 0.85, 1.25, 1.10, 0.95
CAGR: =GEOMEAN(1.20, 0.85, 1.25, 1.10, 0.95) - 1
```

**Business Application:** CAGR is the standard metric for reporting investment performance. "The fund achieved 7.2% CAGR over 5 years" is meaningful because it's the constant rate that would achieve the same total return. Simple averaging of returns (+20-15+25+10-5)/5 = 7% overstates performance because it ignores compounding losses.

**Technical Details:** Convert percentage returns to growth factors by adding 1 (or dividing by 100 and adding 1). The geometric mean of factors minus 1 gives the true compound rate. This equals (Final/Initial)^(1/years) - 1.

---

### [[Scientific Data: Ratio and Index Analysis]]

**Scenario:** A researcher analyzes data expressed as ratios, fold-changes, or multiplicative factors where arithmetic averaging would be inappropriate.

**Implementation:**
```
Fold-changes in gene expression: 2.5, 0.8, 3.2, 1.5
Geometric mean: =GEOMEAN(2.5, 0.8, 3.2, 1.5)
```

**Business Application:** In biology, chemistry, and other sciences, data often represents multiplicative changes. A gene expressed at 2x and 0.5x the baseline (up then down) should average to 1x (no net change), which geometric mean correctly gives: sqrt(2 * 0.5) = 1. Arithmetic mean would incorrectly suggest 1.25x average.

**Technical Details:** Log-transform data for statistical tests (t-tests assume additive effects). Report the geometric mean and geometric standard deviation for symmetric confidence intervals on the ratio scale.

---

### [[Economic Indicators: Average Growth Rates]]

**Scenario:** An economist calculates average annual GDP growth across countries or time periods.

**Implementation:**
```
Country GDP growth rates (as factors): 1.03, 1.05, 1.02, 1.04, 1.06
Average growth factor: =GEOMEAN(1.03, 1.05, 1.02, 1.04, 1.06)
Average growth rate: =GEOMEAN(1.03, 1.05, 1.02, 1.04, 1.06) - 1
```

**Business Application:** Reporting "average GDP growth of 4%" should mean that applying 4% growth each year produces equivalent cumulative growth. Geometric mean provides this interpretation. The World Bank, IMF, and economic publications use geometric means for growth rate comparisons.

**Technical Details:** When growth rates vary significantly, the difference between arithmetic and geometric means can be substantial. High volatility in growth rates increases this gap.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Argument limit:** Up to 255 arguments
- **Requirement:** All values must be strictly positive (> 0)
- **Empty cells:** Ignored in ranges
- **Text in ranges:** Ignored

### Google Sheets

- **Availability:** All versions since launch
- **Argument limit:** Very large (comparable to Excel)
- **Requirement:** All values must be strictly positive (> 0)
- **Empty cells:** Ignored in ranges
- **Behavior:** Identical to Excel

### Key Difference Alert

GEOMEAN behaves identically between Excel and Google Sheets. Both platforms:
- Require all values to be strictly positive
- Return #NUM! for zero or negative values
- Ignore empty cells and text in ranges
- Return identical results for valid inputs

No significant platform differences exist for this function.

## Tips and Best Practices

1. **Use for multiplicative data.** Growth rates, ratios, percentages, and fold-changes are multiplicative - use geometric mean.

2. **Convert percentages to factors.** For returns like +10%, -5%, convert to 1.10, 0.95 before applying GEOMEAN. Subtract 1 from result for average percentage.

3. **Handle zeros carefully.** Zeros make geometric mean undefined. Options: remove zeros, add small constant, or recognize that zero growth makes cumulative product zero.

4. **Understand the AM-GM inequality.** Geometric mean <= Arithmetic mean always. Greater data spread means larger difference.

5. **Use for CAGR calculations.** CAGR = GEOMEAN(growth_factors) - 1. This is the standard way to report compound growth.

6. **Verify with log transformation.** GEOMEAN(values) = EXP(AVERAGE(LN(values))). This equivalence helps verify calculations.

7. **Watch for outliers near zero.** Small values heavily influence geometric mean. A single 0.01 in your data drastically reduces the result.

8. **Choose the right mean for your data.** Additive data (heights, weights, temperatures): arithmetic mean. Multiplicative data (returns, ratios): geometric mean.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AVERAGE]] | Arithmetic mean | For additive data (most general-purpose averaging) |
| [[HARMEAN]] | Harmonic mean | For averaging rates (speed, efficiency) when time is constant |
| [[MEDIAN]] | Middle value | For robust central tendency with outliers |
| [[TRIMMEAN]] | Trimmed mean | For average excluding extreme values |

### Commonly Used Together

**[[AVERAGE]]** - Compare means

*Verify AM-GM inequality:*
```
Geometric: =GEOMEAN(values)
Arithmetic: =AVERAGE(values)
Difference: =AVERAGE(values) - GEOMEAN(values)
```
Arithmetic >= Geometric always; difference indicates data spread.

---

**[[LN]] and [[EXP]]** - Equivalent calculation

*Manual geometric mean:*
```
=EXP(AVERAGE(LN(A1:A10)))
```
Equals GEOMEAN(A1:A10). Useful for understanding the math or working around limitations.

---

**[[PRODUCT]]** - Direct calculation

*Explicit formula:*
```
=PRODUCT(A1:A10)^(1/COUNT(A1:A10))
```
Equals GEOMEAN(A1:A10). Shows the underlying nth-root-of-product formula.

## Official Documentation

- **Microsoft Excel:** [GEOMEAN function](https://support.microsoft.com/en-us/office/geomean-function-db1ac48d-25a5-40a0-ab83-0b38980e40d5)
- **Google Sheets:** [GEOMEAN function](https://support.google.com/docs/answer/3094018)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2003+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Available since inception |

---

*Last updated: 2026-01-10*
