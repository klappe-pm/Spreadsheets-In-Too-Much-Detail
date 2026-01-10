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
- hyperbolic-secant
- exponential-functions
- engineering-math
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Hyperbolic Secant
- Hyperbolic Sec
- sech
tags:
- hyperbolic
- trigonometry
- exponential
- engineering
- physics
- catenary
---

# SECH

## Description

**SECH** returns the hyperbolic secant of a number. It's the reciprocal of the hyperbolic cosine: SECH(x) = 1/COSH(x). Mathematically, it can also be expressed in terms of exponentials: SECH(x) = 2/(e^x + e^(-x)). Unlike circular trigonometric functions that describe relationships in circles, hyperbolic functions describe relationships in hyperbolas and arise naturally in physics, engineering, and applied mathematics.

The hyperbolic secant has a distinctive bell-shaped curve centered at zero, where it reaches its maximum value of 1. As the input moves away from zero in either direction, SECH rapidly approaches zero. This makes SECH(x) always positive and bounded between 0 and 1 (exclusive of 0, inclusive of 1). Unlike the circular secant (SEC), SECH has no asymptotes and no undefined values—it accepts any real number as input.

**Where does SECH appear?** The most famous application is the sech-squared function that describes the shape of solitary waves (solitons) in water and optical fibers. The catenary curve (shape of a hanging chain) involves COSH, and its curvature involves SECH. In quantum mechanics, the sech potential well is one of the few exactly solvable quantum problems. In statistics and machine learning, SECH squared is proportional to the derivative of the hyperbolic tangent activation function.

**Platform availability:** SECH was introduced in Excel 2013 along with other hyperbolic functions. Earlier Excel versions require the manual formula `=1/COSH(x)` or `=2/(EXP(x)+EXP(-x))`. Google Sheets has supported SECH since its launch. If backward compatibility with Excel 2010 matters, use the explicit formulas instead.

## Syntax

> [!f(x)] SECH Syntax
>
> ```
> =SECH(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | Any real number for which you want the hyperbolic secant. Can be positive, negative, or zero. Can be a literal number, cell reference, or formula result. There are no restrictions—any numeric input is valid. |

### Return Value

Returns a numeric value between 0 (exclusive) and 1 (inclusive). SECH(0) = 1, and the result approaches 0 as the absolute value of the input increases. The function is always positive. Returns #VALUE! if the input is non-numeric.

## Examples

> [!f(x)] SECH Examples

### Example 1: SECH of Zero
```
=SECH(0)
```
**Result:** 1

**Explanation:** The hyperbolic secant reaches its maximum at x=0. COSH(0) = 1, so SECH(0) = 1/1 = 1. This is the only input that produces exactly 1.

---

### Example 2: SECH of 1
```
=SECH(1)
```
**Result:** 0.648054273663885

**Explanation:** For x=1, COSH(1) is approximately 1.543, so SECH(1) is approximately 0.648. The value has already dropped significantly from the maximum of 1 at zero.

---

### Example 3: SECH of Negative Number
```
=SECH(-1)
```
**Result:** 0.648054273663885

**Explanation:** SECH is an even function: SECH(-x) = SECH(x). The result for -1 is identical to the result for 1. This symmetry reflects the symmetric bell-curve shape.

---

### Example 4: SECH of 2
```
=SECH(2)
```
**Result:** 0.265802228834079

**Explanation:** At x=2, SECH has dropped to about 26.6% of its maximum value. The rapid decay demonstrates the exponential nature of hyperbolic functions.

---

### Example 5: Large Positive Input
```
=SECH(10)
```
**Result:** 0.0000907999138660016

**Explanation:** For large inputs, SECH approaches zero rapidly. At x=10, the result is less than 0.01%. The exponential decay means SECH effectively acts as a localized pulse function.

---

### Example 6: Large Negative Input
```
=SECH(-10)
```
**Result:** 0.0000907999138660016

**Explanation:** Due to the even function property, SECH(-10) equals SECH(10). Large negative inputs also produce values very close to zero.

---

### Example 7: Small Positive Input
```
=SECH(0.1)
```
**Result:** 0.995016625083194

**Explanation:** Near zero, SECH stays close to 1. At x=0.1, the value is still about 99.5% of the maximum. The function is quite flat near its peak.

---

### Example 8: Using Exponential Form
```
=2/(EXP(A1)+EXP(-A1))
```
Where A1 = 1

**Result:** 0.648054273663885

**Explanation:** This is the explicit exponential formula equivalent to SECH(1). Useful for understanding the function mathematically or for backward compatibility with older Excel.

---

### Example 9: Relationship to COSH
```
=1/COSH(A1)
```
Where A1 = 2

**Result:** 0.265802228834079

**Explanation:** SECH(x) = 1/COSH(x) by definition. This formula produces identical results to SECH(2) and works in all Excel versions.

---

### Example 10: SECH Squared (Soliton Shape)
```
=SECH(A1)^2
```
Where A1 = 0

**Result:** 1

**Explanation:** SECH squared appears in soliton wave theory and as the derivative of TANH. At x=0, SECH(0)^2 = 1^2 = 1.

---

### Example 11: Array Calculation
```
=SECH({-2, -1, 0, 1, 2})
```
**Result:** {0.2658, 0.6481, 1, 0.6481, 0.2658}

**Explanation:** SECH operates on arrays, showing the symmetric bell-curve shape with maximum at the center (0) and symmetric decay on both sides.

---

### Example 12: Derivative of TANH Verification
```
=SECH(A1)^2
=(TANH(A1+0.0001)-TANH(A1-0.0001))/0.0002
```
**Result:** Both formulas give approximately the same result

**Explanation:** The derivative of TANH(x) is SECH(x)^2. The second formula approximates the derivative numerically, confirming the mathematical relationship.

---

### Example 13: Signal Processing Pulse
```
=Amplitude * SECH((x - Center)/Width)
```
**Result:** Sech pulse centered at specified location

**Explanation:** In signal processing and optics, SECH pulses are used because they maintain their shape as they propagate. The formula shifts and scales the standard SECH curve.

---

### Example 14: Calculating Curvature of Catenary
```
=SECH(x/a)^2 / a
```
**Result:** Curvature at point x on a catenary with parameter a

**Explanation:** The curvature of the catenary curve y = a*COSH(x/a) is given by SECH(x/a)^2/a. Maximum curvature occurs at x=0.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the input is numeric. Text strings, even if they look like numbers, will cause this error. |
| `#NAME?` | Function not recognized (Excel 2010 or earlier) | Use `=1/COSH(x)` or `=2/(EXP(x)+EXP(-x))` as equivalent formulas. |
| `Result is 0` | Input magnitude is too large | This is expected behavior—SECH approaches 0 for large |x|. For |x| > ~700, the result underflows to exactly 0. |
| `Unexpected precision loss` | Very large inputs | For inputs beyond ~700, SECH underflows to 0 due to floating-point limits. |
| `Confused with SEC` | Using SEC when SECH was intended | SEC is the circular secant (1/COS), SECH is hyperbolic secant (1/COSH). They are different functions. |

## Use Cases

### [[Fiber Optics - Soliton Pulse Modeling]]

**Scenario:** Model optical soliton pulses in fiber optic communications where pulse shape preservation is critical.

**Implementation:**
```
=PeakPower * SECH((Time - PulseCenter) / PulseWidth)^2
=SECH(2*ACOSH(SQRT(2)))  ' Width at half maximum
```

**Business Application:** Telecommunications engineers designing long-haul fiber optic systems use soliton pulses because they resist dispersion. Understanding SECH profiles helps optimize data transmission rates.

**Technical Details:** The fundamental soliton in nonlinear fiber optics has a SECH profile. The pulse maintains its shape indefinitely when nonlinear and dispersive effects balance exactly.

---

### [[Physics - Quantum Mechanics Potentials]]

**Scenario:** Solve the Schrodinger equation for a SECH-squared potential well, one of the few analytically solvable cases.

**Implementation:**
```
=V0 * SECH(x/a)^2
=EnergyLevel * (1 - SECH(x/Width)^2)
```

**Business Application:** Research institutions and educational settings studying quantum mechanics use these potentials to teach bound state concepts and verify numerical methods.

**Technical Details:** The reflectionless potential -V0*SECH(x/a)^2 has exact solutions and transmits all incoming waves perfectly, making it useful for modeling and testing numerical methods.

---

### [[Neural Networks - Activation Function Derivatives]]

**Scenario:** Calculate the derivative of the tanh activation function for backpropagation in neural networks.

**Implementation:**
```
=SECH(WeightedInput)^2
=1 - TANH(WeightedInput)^2  ' Equivalent form
```

**Business Application:** Machine learning engineers implementing neural networks need these derivatives for gradient descent. Understanding the SECH-squared form helps optimize training algorithms.

**Technical Details:** The derivative of TANH(x) is SECH(x)^2 = 1-TANH(x)^2. This derivative reaches its maximum at x=0 and decays for large |x|, causing the vanishing gradient problem in deep networks.

---

### [[Structural Engineering - Cable Sag Analysis]]

**Scenario:** Analyze the curvature and stress distribution in hanging cables and suspension bridges.

**Implementation:**
```
=TensionParameter * COSH(HorizontalDistance / TensionParameter)
=SECH(HorizontalDistance / TensionParameter)^2 / TensionParameter
```

**Business Application:** Civil engineers designing suspension bridges, power lines, and cable-stayed structures need to calculate cable geometry and stress distributions.

**Technical Details:** The catenary curve y=a*COSH(x/a) describes the shape of a uniform cable hanging under its own weight. The curvature at any point is SECH(x/a)^2/a.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later only
- **Legacy alternative:** Use `=1/COSH(x)` or `=2/(EXP(x)+EXP(-x))` in Excel 2010 and earlier
- **Precision:** 15 significant digits
- **Underflow:** Returns 0 for inputs with absolute value greater than approximately 710
- **Array support:** Yes, with dynamic arrays in Excel 365

### Google Sheets
- **Availability:** All versions since launch
- **Syntax:** Identical to Excel
- **Precision:** 15 significant digits
- **Underflow:** Similar behavior for very large inputs
- **Array support:** Native support

### Key Differences
- Excel 2010 and earlier users must use equivalent formulas
- Both platforms handle underflow similarly (return 0 for very large |x|)
- No localization differences—function name is SECH in all language versions
- Both platforms treat SECH as an even function correctly

## Tips and Best Practices

1. **Remember SECH is always positive:** Unlike SEC (circular secant), SECH never goes negative. The output range is (0, 1]. Use this for sanity checks on your calculations.

2. **Use 1/COSH for backward compatibility:** If your workbook might be opened in Excel 2010 or earlier, use `=1/COSH(x)` instead of SECH. It's mathematically identical.

3. **Handle large inputs gracefully:** For |x| > 20, SECH is effectively zero for most practical purposes. Consider setting a threshold below which you treat the result as zero to avoid unnecessary precision.

4. **Know the relationship to TANH:** The derivative of TANH is SECH^2. If you're doing calculus-related work, this identity is essential.

5. **Use SECH^2 for pulse shapes:** When modeling pulses or waves, SECH^2 often appears. The squared version maintains positivity and has better mathematical properties for physical modeling.

6. **Verify with SECH(0)=1:** This is the maximum value and a quick sanity check. If SECH(0) doesn't return exactly 1, something is wrong with your formula.

7. **Consider numerical stability:** For very large |x|, EXP(x) overflows before SECH underflows. The 2/(EXP(x)+EXP(-x)) form can have numerical issues for |x|>700. SECH handles this internally.

8. **Don't confuse with SEC:** SECH is hyperbolic (uses exponentials), SEC is circular (uses cosine). They have very different behaviors and domains.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SEC]] | Returns the circular secant (1/COS) | When working with circular trigonometry and angles |
| [[COSH]] | Returns the hyperbolic cosine | When you need the denominator of SECH: SECH = 1/COSH |
| [[TANH]] | Returns the hyperbolic tangent | When you need the function whose derivative is SECH^2 |
| [[CSCH]] | Returns the hyperbolic cosecant (1/SINH) | When you need the reciprocal of hyperbolic sine |

### Commonly Used Together

**[[COSH]]** - Hyperbolic cosine

*Reciprocal relationship:*
```
=1/COSH(A1)  (equivalent to SECH(A1))
```
SECH is defined as the reciprocal of COSH.

---

**[[TANH]]** - Hyperbolic tangent

*Derivative relationship:*
```
=SECH(A1)^2  (equals the derivative of TANH at A1)
```
Essential for neural network backpropagation and calculus applications.

---

**[[EXP]]** - Exponential function

*Explicit formula:*
```
=2/(EXP(A1)+EXP(-A1))  (equivalent to SECH(A1))
```
Understanding the exponential form helps with mathematical analysis.

---

**[[LN]]** - Natural logarithm

*Inverse relationship (for specific ranges):*
```
=LN((1+SQRT(1-x^2))/x)  (inverse SECH for 0<x<=1)
```
Finding what input produces a given SECH output.

---

**[[ACOSH]]** - Inverse hyperbolic cosine

*Finding inputs from SECH values:*
```
=ACOSH(1/SECH_value)
```
Converts a SECH result back to the original input (positive solution).

## Official Documentation

- **Microsoft Excel:** [SECH function](https://support.microsoft.com/en-us/office/sech-function-e05a789f-5ff8-4d7f-bc71-4f6dc0ebed20)
- **Google Sheets:** [SECH function](https://support.google.com/docs/answer/9116426)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Added as part of expanded math function set |
| Excel Online | Launch | Supported from initial release |
| Google Sheets | Original launch | Available from the beginning |
| LibreOffice Calc | 4.0+ | Compatible implementation |

---

*Last updated: 2026-01-10*
