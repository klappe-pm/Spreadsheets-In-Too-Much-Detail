---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- sum-of-squares
- statistics
- euclidean-distance
- regression
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sum of Squares
- Sum Squared
- Squared Sum
tags:
- sum-of-squares
- statistics
- variance
- standard-deviation
- distance
- pythagorean
- regression
---

# SUMSQ

## Description

**SUMSQ** returns the sum of the squares of its arguments. Given a set of numbers, it squares each one and adds the squared values together. Mathematically, SUMSQ(a, b, c) = a^2 + b^2 + c^2. This fundamental calculation appears throughout statistics, physics, engineering, and geometry—anywhere you need to work with squared quantities or calculate distances, variances, or least-squares values.

The function accepts individual values, cell references, and ranges. It automatically ignores text values, logical values, and empty cells within ranges, processing only numeric values. You can mix arguments freely: `=SUMSQ(A1:A10, 5, B1:B5)` sums the squares of all numbers in both ranges plus 5 squared (25).

**Where is SUMSQ used?** The sum of squares is a building block for many statistical calculations. Variance involves sum of squared deviations from the mean. Standard deviation is the square root of variance. In regression analysis, the sum of squared residuals measures model fit. In physics, the Pythagorean theorem uses sum of squares for distance: distance = sqrt(x^2 + y^2). In quality control, sum of squares helps quantify process variation. Machine learning's mean squared error (MSE) relies on sum of squares for measuring prediction accuracy.

**Performance note:** SUMSQ is more efficient than using SUMPRODUCT(range, range) or writing out individual squared terms. It's optimized for this specific calculation and handles large datasets efficiently. For conditional sum of squares, you'd need SUMPRODUCT with a condition, as there's no SUMSQIF function.

## Syntax

> [!f(x)] SUMSQ Syntax
>
> ```
> =SUMSQ(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first number, cell reference, or range whose squared values you want to sum. |
| `number2, ...` | No | Additional numbers, cell references, or ranges. Up to 255 arguments. Text values and logical values within ranges are ignored. |

### Return Value

Returns a numeric value equal to the sum of squared values. Returns 0 if all arguments are empty or contain only non-numeric values. Returns #VALUE! if any direct argument (not within a range) is non-numeric text.

## Examples

> [!f(x)] SUMSQ Examples

### Example 1: Sum of Squares of Individual Numbers
```
=SUMSQ(3, 4)
```
**Result:** 25

**Explanation:** 3^2 + 4^2 = 9 + 16 = 25. This is the famous 3-4-5 right triangle—the hypotenuse length would be SQRT(25) = 5.

---

### Example 2: Sum of Squares of a Range
```
=SUMSQ(A1:A5)
```
Where A1:A5 contains {1, 2, 3, 4, 5}

**Result:** 55

**Explanation:** 1^2 + 2^2 + 3^2 + 4^2 + 5^2 = 1 + 4 + 9 + 16 + 25 = 55.

---

### Example 3: Single Value
```
=SUMSQ(7)
```
**Result:** 49

**Explanation:** 7^2 = 49. With a single value, SUMSQ simply returns the square.

---

### Example 4: Negative Numbers
```
=SUMSQ(-3, -4)
```
**Result:** 25

**Explanation:** (-3)^2 + (-4)^2 = 9 + 16 = 25. Squaring eliminates negative signs, so negative numbers contribute positively.

---

### Example 5: Mixed Positive and Negative
```
=SUMSQ(-2, 3, -4, 5)
```
**Result:** 54

**Explanation:** 4 + 9 + 16 + 25 = 54. All squared values are positive regardless of original sign.

---

### Example 6: Combining Ranges and Values
```
=SUMSQ(A1:A3, 10, B1:B2)
```
Where A1:A3 = {1, 2, 3} and B1:B2 = {4, 5}

**Result:** 155

**Explanation:** (1 + 4 + 9) + 100 + (16 + 25) = 14 + 100 + 41 = 155. Ranges and individual values combine seamlessly.

---

### Example 7: 3D Distance Calculation
```
=SQRT(SUMSQ(x2-x1, y2-y1, z2-z1))
```
**Result:** Euclidean distance between two 3D points

**Explanation:** The Pythagorean theorem extends to 3D. SUMSQ calculates the squared component differences, SQRT gives the distance.

---

### Example 8: Range with Text and Blanks
```
=SUMSQ(A1:A5)
```
Where A1:A5 = {1, "text", 3, "", 5}

**Result:** 35

**Explanation:** 1^2 + 3^2 + 5^2 = 1 + 9 + 25 = 35. Text and blank cells are ignored within ranges.

---

### Example 9: All Zeros
```
=SUMSQ(0, 0, 0, 0, 0)
```
**Result:** 0

**Explanation:** 0^2 = 0 for each value. The sum of zeros is zero.

---

### Example 10: Decimal Values
```
=SUMSQ(1.5, 2.5, 3.5)
```
**Result:** 20.75

**Explanation:** 2.25 + 6.25 + 12.25 = 20.75. SUMSQ handles decimals with full precision.

---

### Example 11: Large Dataset
```
=SUMSQ(A1:A10000)
```
**Result:** Sum of squares for 10,000 values

**Explanation:** SUMSQ efficiently processes large ranges. This is faster than equivalent SUMPRODUCT formulas.

---

### Example 12: Variance Calculation Component
```
=SUMSQ(A1:A10)/COUNT(A1:A10) - AVERAGE(A1:A10)^2
```
**Result:** Population variance (alternative formula)

**Explanation:** Variance can be calculated as E[X^2] - (E[X])^2. SUMSQ provides the sum of squares term.

---

### Example 13: Sum of Squared Deviations from Mean
```
=SUMSQ(A1:A10 - AVERAGE(A1:A10))
```
(Array formula in older Excel, automatic in Excel 365)

**Result:** Sum of squared deviations

**Explanation:** For variance calculation, you need squared deviations from mean. This subtracts the mean from each value, then sums squares.

---

### Example 14: Pythagorean Theorem Verification
```
=SUMSQ(3, 4) = 5^2
```
**Result:** TRUE (both equal 25)

**Explanation:** The classic Pythagorean triple: 3^2 + 4^2 = 5^2. SUMSQ verifies triangle relationships.

---

### Example 15: Root Mean Square (RMS)
```
=SQRT(SUMSQ(A1:A100)/COUNT(A1:A100))
```
**Result:** RMS value of the dataset

**Explanation:** RMS = sqrt(mean of squared values). SUMSQ divided by count gives mean of squares; SQRT gives RMS.

---

### Example 16: Comparison with SUMPRODUCT
```
=SUMSQ(A1:A10)
=SUMPRODUCT(A1:A10, A1:A10)
```
**Result:** Both return identical results

**Explanation:** SUMPRODUCT(range, range) multiplies each value by itself (squaring) and sums. SUMSQ is cleaner and often faster.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Direct text argument (not in a range) | Ensure direct arguments are numeric. Text within ranges is automatically ignored. |
| `#REF!` | Invalid cell reference | Check for deleted cells or broken references. |
| `Result unexpectedly large` | Large values square to very large numbers | This is mathematically correct. Consider using smaller scale or handling overflow. |
| `Result is 0` | All arguments are empty or non-numeric | Verify that the range contains numeric data. |
| `Unexpected positive result` | Forgot that squaring makes negatives positive | By definition, squares are always non-negative. This is correct behavior. |

## Use Cases

### [[Statistical Analysis - Variance and Standard Deviation]]

**Scenario:** Calculate variance components for statistical analysis.

**Implementation:**
```
' Sum of squared deviations (variance numerator)
=SUMSQ(Data - AVERAGE(Data))

' Population variance formula component
=SUMSQ(Data)/COUNT(Data) - AVERAGE(Data)^2

' Verify against built-in VAR.P
=VAR.P(Data) = (SUMSQ(Data)/COUNT(Data) - AVERAGE(Data)^2)
```

**Business Application:** Quality control, process capability analysis, financial risk assessment, and any statistical analysis requiring variance or standard deviation.

**Technical Details:** While VAR.P, VAR.S, STDEV.P, and STDEV.S provide direct calculations, understanding the SUMSQ components helps verify results and build custom statistical measures.

---

### [[Geometry - Distance Calculations]]

**Scenario:** Calculate Euclidean distances between points in 2D or 3D space.

**Implementation:**
```
' 2D distance between (x1,y1) and (x2,y2)
=SQRT(SUMSQ(x2-x1, y2-y1))

' 3D distance
=SQRT(SUMSQ(x2-x1, y2-y1, z2-z1))

' Distance from origin
=SQRT(SUMSQ(x, y, z))
```

**Business Application:** Logistics optimization, geographic analysis, CAD/engineering calculations, game development, and any spatial analysis.

**Technical Details:** SUMSQ naturally extends the Pythagorean theorem to any number of dimensions. Distance in n-dimensional space uses SQRT(SUMSQ(d1, d2, ..., dn)).

---

### [[Regression Analysis - Model Fit Metrics]]

**Scenario:** Calculate sum of squared residuals and R-squared values for regression models.

**Implementation:**
```
' Sum of squared residuals (SSR)
=SUMSQ(Actual - Predicted)

' Total sum of squares (SST)
=SUMSQ(Actual - AVERAGE(Actual))

' R-squared calculation
=1 - SUMSQ(Residuals)/SUMSQ(Actual - AVERAGE(Actual))
```

**Business Application:** Financial forecasting validation, sales prediction accuracy, marketing attribution models, and any predictive analytics evaluation.

**Technical Details:** R-squared = 1 - SSR/SST measures how much variance the model explains. SUMSQ provides both sum of squared residuals and total sum of squares.

---

### [[Signal Processing - Power and RMS]]

**Scenario:** Calculate signal power, RMS voltage, or energy content.

**Implementation:**
```
' RMS (Root Mean Square)
=SQRT(SUMSQ(SignalData)/COUNT(SignalData))

' Signal power (proportional to sum of squares)
=SUMSQ(SignalData)

' Normalized power
=SUMSQ(SignalData)/COUNT(SignalData)
```

**Business Application:** Audio engineering, electrical engineering, vibration analysis, and telecommunications signal quality assessment.

**Technical Details:** RMS values are fundamental in AC circuit analysis and signal processing. The sum of squares represents total energy in the signal.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Maximum arguments:** 255
- **Range handling:** Ignores text, logical values, and empty cells
- **Array support:** Works with array results in Excel 365
- **Performance:** Optimized for large ranges

### Google Sheets
- **Availability:** All versions
- **Maximum arguments:** Practically unlimited
- **Range handling:** Same as Excel—ignores non-numeric values
- **Syntax:** Identical to Excel
- **Performance:** Comparable to Excel

### Key Differences
- Both platforms implement SUMSQ identically
- Maximum argument limits differ but rarely matter in practice
- No localization differences—function name is SUMSQ everywhere
- Both handle negative numbers, decimals, and empty cells the same way

## Tips and Best Practices

1. **Use for distance calculations:** SQRT(SUMSQ(dx, dy, dz)) is cleaner than SQRT(dx^2 + dy^2 + dz^2) for Euclidean distance.

2. **Remember: squares are always positive:** SUMSQ(-3, -4) = 25, not -25. Negative inputs contribute positive squared values.

3. **Prefer SUMSQ over SUMPRODUCT for squares:** `=SUMSQ(A1:A100)` is cleaner and often faster than `=SUMPRODUCT(A1:A100, A1:A100)`.

4. **Use for variance components:** Understanding that VAR = SUMSQ(deviations)/n helps verify and customize statistical calculations.

5. **Calculate RMS values:** RMS = SQRT(SUMSQ(data)/COUNT(data)). This is essential in signal processing and electrical engineering.

6. **Check for overflow with large values:** Squaring large numbers can produce very large results. For values near 10^7, squared values approach Excel's precision limits.

7. **Combine with SQRT for magnitudes:** Vector magnitude = SQRT(SUMSQ(components)). This applies to any vector dimension.

8. **Use named ranges for clarity:** `=SUMSQ(Residuals)` is clearer than `=SUMSQ(C2:C100-D2:D100)`.

9. **Verify against known values:** Test with 3, 4 -> 25 (the 3-4-5 triangle) to confirm your formula is working.

10. **Consider DEVSQ for deviations from mean:** DEVSQ directly calculates the sum of squared deviations from the mean, which is often what you need for variance.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DEVSQ]] | Sum of squared deviations from the mean | When you specifically need sum of squared deviations, not raw sum of squares |
| [[SUMPRODUCT]] | Sum of products of arrays | For weighted sums or when you need to multiply different arrays before summing |
| [[SUM]] | Sum without squaring | When you need simple addition without squaring |
| [[POWER]] | Raise to a power | When you need powers other than 2 |

### Commonly Used Together

**[[SQRT]]** - Square root

*Calculate distances:*
```
=SQRT(SUMSQ(A1-B1, A2-B2))
```
SUMSQ provides the sum of squares; SQRT gives the distance.

---

**[[AVERAGE]]** - Mean value

*Calculate variance components:*
```
=SUMSQ(Data)/COUNT(Data) - AVERAGE(Data)^2
```
The computational formula for variance uses both AVERAGE and SUMSQ.

---

**[[COUNT]]** - Count numeric values

*Calculate RMS:*
```
=SQRT(SUMSQ(Data)/COUNT(Data))
```
Dividing by count gives mean of squares; SQRT gives RMS.

---

**[[VAR.P]] / [[VAR.S]]** - Variance functions

*Relationship to SUMSQ:*
```
VAR.P(Data) = SUMSQ(Data - AVERAGE(Data)) / COUNT(Data)
```
Understanding this helps verify variance calculations.

---

**[[STDEV.P]] / [[STDEV.S]]** - Standard deviation

*Built on SUMSQ:*
```
STDEV.P(Data) = SQRT(SUMSQ(Data - AVERAGE(Data)) / COUNT(Data))
```
Standard deviation is the square root of variance, which uses SUMSQ.

---

**[[DEVSQ]]** - Sum of squared deviations

*Related function:*
```
=DEVSQ(Data)  ' Same as SUMSQ(Data - AVERAGE(Data))
```
DEVSQ automatically calculates deviations from mean before squaring.

## Official Documentation

- **Microsoft Excel:** [SUMSQ function](https://support.microsoft.com/en-us/office/sumsq-function-e3313c02-51cc-4963-aae6-31442d9ec307)
- **Google Sheets:** [SUMSQ function](https://support.google.com/docs/answer/3093714)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| All Excel versions | Unchanged | Consistent behavior throughout |
| Google Sheets | Original launch | Available from the beginning |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
