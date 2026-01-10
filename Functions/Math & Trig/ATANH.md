---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- hyperbolic-functions
- inverse-hyperbolic
- trigonometry
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Inverse Hyperbolic Tangent
- Arc Hyperbolic Tangent
- ARCTANH
tags:
- hyperbolic
- trigonometry
- inverse-function
- statistics
- fisher-transform
---

# ATANH

## Description

**ATANH (Inverse Hyperbolic Tangent)** returns the inverse hyperbolic tangent of a number. Mathematically, if `y = TANH(x)`, then `x = ATANH(y)`. The function calculates the value whose hyperbolic tangent equals the input number. For any input value `x`, ATANH returns `0.5 * ln((1+x)/(1-x))`, where `ln` is the natural logarithm.

The most famous use of ATANH is the **Fisher transformation** in statistics, which converts correlation coefficients (bounded between -1 and 1) into unbounded z-scores for analysis. If you've ever compared correlation coefficients or calculated confidence intervals for correlations, you've implicitly used ATANH. This single application makes ATANH one of the most practically important hyperbolic functions.

**The domain restriction:** ATANH only accepts values strictly between -1 and 1 (exclusive). This makes mathematical sense: TANH outputs only values in (-1, 1), so ATANH can only accept inputs in that range. Attempting to calculate ATANH(1), ATANH(-1), or any value outside this range returns #NUM!. The function approaches infinity as input approaches 1, and negative infinity as input approaches -1.

**Beyond statistics:** ATANH appears in physics (special relativity velocity addition), control systems (transfer function analysis), and anywhere transformation between bounded and unbounded scales is needed. Its role in converting between "percentage-like" values and unbounded values makes it a bridge function between different mathematical domains.

## Syntax

> [!f(x)] ATANH Syntax
>
> ```
> =ATANH(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | A real number strictly between -1 and 1 (exclusive). The value for which you want the inverse hyperbolic tangent. |

### Return Value

Returns the inverse hyperbolic tangent of the input number. The result can be any real number (positive, negative, or zero). Returns #NUM! if input is <= -1 or >= 1. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] ATANH Examples

### Example 1: Zero Input
```
=ATANH(0)
```
**Result:** 0

**Explanation:** The hyperbolic tangent of 0 is 0, so the inverse hyperbolic tangent of 0 is 0. This is the function's fixed point.

---

### Example 2: Positive Value
```
=ATANH(0.5)
```
**Result:** 0.549306144334055

**Explanation:** This is 0.5 * ln(3), since (1+0.5)/(1-0.5) = 3. The hyperbolic tangent of approximately 0.549 equals 0.5.

---

### Example 3: Negative Value
```
=ATANH(-0.5)
```
**Result:** -0.549306144334055

**Explanation:** ATANH is an odd function, so ATANH(-0.5) = -ATANH(0.5). The negative of the previous result.

---

### Example 4: Near Boundary
```
=ATANH(0.99)
```
**Result:** 2.64665241236225

**Explanation:** As input approaches 1, output grows rapidly toward infinity. At 0.99, we're already at about 2.65.

---

### Example 5: Very Near Boundary
```
=ATANH(0.999)
```
**Result:** 3.80023072066063

**Explanation:** Just 0.009 closer to 1, but output jumps by over 1. The function accelerates dramatically near boundaries.

---

### Example 6: Fisher Transformation of Correlation
```
=ATANH(0.75)
```
**Result:** 0.972955074527657

**Explanation:** In statistics, this is the Fisher z-transformation of correlation r=0.75. The z-score can now be used in standard statistical tests.

---

### Example 7: Cell Reference
```
=ATANH(A1)
```
Where A1 contains 0.3

**Result:** 0.309519604203112

**Explanation:** References the correlation or value in cell A1 and returns its inverse hyperbolic tangent.

---

### Example 8: Error Case - Boundary Value
```
=ATANH(1)
```
**Result:** #NUM!

**Explanation:** ATANH(1) would be infinity, which isn't representable. The function is undefined at exactly 1 and -1.

---

### Example 9: Error Case - Out of Range
```
=ATANH(1.5)
```
**Result:** #NUM!

**Explanation:** Values outside (-1, 1) have no real inverse hyperbolic tangent. The TANH function never outputs 1.5.

---

### Example 10: Verifying with TANH
```
=TANH(ATANH(0.8))
```
**Result:** 0.8

**Explanation:** Applying TANH to ATANH reverses the operation, returning the original input. This identity can verify calculations.

---

### Example 11: Fisher Transform for Multiple Correlations
```
=ATANH(B2:B10)
```
Where B2:B10 contains correlation coefficients

**Result:** Array of Fisher z-values

**Explanation:** In Excel 365 or Google Sheets, applies Fisher transformation to all correlations simultaneously for statistical comparison.

---

### Example 12: Correlation Confidence Interval
```
Standard Error: =1/SQRT(N-3)
Lower Z: =ATANH(r) - 1.96*SE
Upper Z: =ATANH(r) + 1.96*SE
Lower r: =TANH(Lower_Z)
Upper r: =TANH(Upper_Z)
```
**Result:** 95% confidence interval for correlation

**Explanation:** Classic statistical procedure for correlation confidence intervals uses ATANH to transform, calculate interval, then TANH to back-transform.

---

### Example 13: Average of Correlations
```
=TANH(AVERAGE(ATANH(r1), ATANH(r2), ATANH(r3)))
```
**Result:** Properly averaged correlation

**Explanation:** You can't simply average correlations directly. Transform with ATANH, average the z-values, then back-transform with TANH.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input value is >= 1 or <= -1 | Ensure input is strictly between -1 and 1. Check data validity. |
| `#VALUE!` | Input is non-numeric (text, empty cell, error) | Verify input is a number. Use ISNUMBER() to validate. |
| `#NAME?` | Function name misspelled | Check spelling. Ensure it's ATANH, not ARCTANH or ATAN (which is inverse tangent, different function). |
| `Very large result` | Input very close to 1 or -1 | This is mathematically correct. As input approaches boundary, output approaches infinity. |
| `Unexpected result` | Confusing ATANH with ATAN | ATANH is inverse hyperbolic tangent; ATAN is inverse circular tangent. Very different domains and applications. |

## Use Cases

### [[Fisher Z-Transformation for Correlations]]

**Scenario:** Convert correlation coefficients to z-scores for statistical hypothesis testing or averaging.

**Implementation:**
```
Z-score: =ATANH(correlation)
Standard Error: =1/SQRT(sample_size - 3)
Z-test: =(ATANH(r1) - ATANH(r2)) / SQRT(1/(n1-3) + 1/(n2-3))
```

**Business Application:** Comparing correlations between groups, calculating confidence intervals for correlations, meta-analysis of correlation studies in psychology, medicine, and social sciences.

**Technical Details:** The Fisher transformation converts the bounded, non-normal distribution of r to an approximately normal distribution with known standard error. Essential for valid statistical inference on correlations.

---

### [[Averaging Correlation Coefficients]]

**Scenario:** Calculate the average of multiple correlation coefficients from different studies or samples.

**Implementation:**
```
=TANH(AVERAGE(ATANH(A2:A10)))
```

**Business Application:** Meta-analysis, combining research results, averaging effect sizes across multiple studies or time periods.

**Technical Details:** Simple arithmetic mean of correlations is biased. Fisher-transform first, average the z-values, then back-transform. This gives an unbiased estimate.

---

### [[Relativistic Velocity Addition]]

**Scenario:** Calculate combined velocity when two velocities are added relativistically (approaching speed of light).

**Implementation:**
```
Combined_rapidity: =ATANH(v1/c) + ATANH(v2/c)
Combined_velocity: =c * TANH(Combined_rapidity)
```

**Business Application:** Particle physics simulations, space mission planning at relativistic speeds, high-energy physics calculations.

**Technical Details:** In special relativity, velocities don't add linearly. ATANH converts velocity ratios to rapidities, which do add linearly, then TANH converts back.

---

### [[Logit-Like Transformations]]

**Scenario:** Transform probability-like values (0-1 range) to unbounded scale for regression or analysis.

**Implementation:**
```
=2 * ATANH(2*p - 1)
```
Where p is probability (0 to 1)

**Business Application:** Bounded data regression, risk scoring transformations, converting percentages to unbounded scores for modeling.

**Technical Details:** This transformation maps [0,1] to (-infinity, infinity), similar to logit but with different curvature. Useful when logit transformation is too aggressive.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later (part of Math and Trig functions)
- **Precision:** 15 significant digits
- **Array support:** Excel 365 supports dynamic arrays; older versions require Ctrl+Shift+Enter
- **FISHER function:** Excel also has FISHER() which is identical to ATANH()
- **FISHERINV function:** Excel also has FISHERINV() which is identical to TANH()

### Google Sheets
- **Availability:** All versions
- **Precision:** 15 significant digits
- **Array support:** Native array support without special entry
- **No FISHER alias:** Google Sheets doesn't have FISHER(), only ATANH()
- **Behavior:** Mathematically identical to Excel

### Both Platforms
- Function name and syntax are identical
- Same domain restriction (-1 < x < 1, exclusive)
- Same return value range (all real numbers)
- Identical error behaviors
- Odd function property: ATANH(-x) = -ATANH(x)

## Tips and Best Practices

1. **Know your domain:** ATANH requires input strictly between -1 and 1. Values of exactly -1 or 1 will error. Pre-check with `=IF(ABS(A1)<1, ATANH(A1), "Out of range")`.

2. **Use FISHER() in Excel for clarity:** When doing statistical correlation work, `=FISHER(r)` is identical to `=ATANH(r)` but communicates intent more clearly to statisticians.

3. **Remember it's odd:** ATANH(-x) = -ATANH(x). This symmetry can simplify formulas and verify results.

4. **Zero is zero:** ATANH(0) = 0. No special handling needed for zero inputs.

5. **Beware boundary behavior:** Near +-1, ATANH changes very rapidly. Small changes in input cause large changes in output. This is mathematically correct but can amplify measurement errors.

6. **Always back-transform:** After Fisher z-transformation, remember to use TANH() to convert results back to correlation scale for interpretation.

7. **Don't confuse with ATAN:** ATAN is inverse tangent for circular trigonometry, domain all real numbers. ATANH is inverse hyperbolic tangent with restricted domain. Completely different functions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TANH]] | Returns the hyperbolic tangent | When you have the parameter and need the bounded result |
| [[FISHER]] | Fisher transformation (Excel only) | Identical to ATANH, clearer for statistics contexts |
| [[ACOSH]] | Returns the inverse hyperbolic cosine | For COSH reversal calculations |
| [[ASINH]] | Returns the inverse hyperbolic sine | For SINH reversal (no domain restrictions) |
| [[ATAN]] | Returns the inverse circular tangent | For regular trigonometry (different function!) |

### Commonly Used Together

**[[TANH]]** / **[[FISHERINV]]** - Hyperbolic tangent (back-transformation)

*Convert z-score back to correlation:*
```
=TANH(z_score)  // Convert Fisher z back to r
=FISHERINV(z_score)  // Same thing, Excel only
```
Essential for completing the Fisher transformation round-trip.

---

**[[AVERAGE]]** - Average z-scores

*Average correlations properly:*
```
=TANH(AVERAGE(ATANH(A1:A10)))  // Average of correlations
```
Transform, average, back-transform for unbiased correlation average.

---

**[[SQRT]]** - Standard error calculation

*Fisher transformation standard error:*
```
=1/SQRT(n-3)  // Standard error of Fisher z
```
Used for confidence intervals and hypothesis tests on correlations.

---

**[[IFERROR]]** - Error handling

*Handle out-of-range values:*
```
=IFERROR(ATANH(A1), "Correlation must be between -1 and 1")
```
Catches boundary violations gracefully.

---

**[[ABS]]** - Check domain

*Validate before calculation:*
```
=IF(ABS(A1)<1, ATANH(A1), NA())
```
Pre-validate input to avoid #NUM! errors.

## Official Documentation

- **Microsoft Excel:** [ATANH function](https://support.microsoft.com/en-us/office/atanh-function-3cd65768-0de7-4f1d-b312-d01c8c930d90)
- **Google Sheets:** [ATANH function](https://support.google.com/docs/answer/3093395)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Added as part of expanded Math and Trig library |
| Excel 365 | Ongoing | Dynamic array support added |
| Google Sheets | Original launch | Available since Sheets inception |

---

*Last updated: 2026-01-10*
