---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- hyperbolic
- tanh
- sigmoid
- neural-networks
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Hyperbolic Tangent
- Hyperbolic Tan
tags:
- hyperbolic
- tanh
- sigmoid
- activation-function
- neural-networks
- machine-learning
---

# TANH

## Description

**TANH** returns the hyperbolic tangent of a number. The formula is: tanh(x) = sinh(x)/cosh(x) = (e^x - e^(-x))/(e^x + e^(-x)). Unlike regular tangent which is unbounded with asymptotes, hyperbolic tangent is **bounded between -1 and 1**, creating a smooth S-shaped curve. This property makes TANH essential in modern applications, especially machine learning and neural networks.

**The S-curve behavior:** TANH smoothly transitions from -1 (for large negative x) through 0 (at x=0) to +1 (for large positive x). The transition happens mostly in the range -3 to +3, with the steepest part near zero. This "squashing" behavior—mapping any real number to the range (-1, 1)—is why TANH is used as an activation function in neural networks.

**Key properties:** TANH(0) = 0 (passes through origin). TANH is an odd function: tanh(-x) = -tanh(x). For small x, tanh(x) is approximately x. As x approaches positive infinity, tanh(x) approaches 1. As x approaches negative infinity, tanh(x) approaches -1. TANH saturates (flattens out) for |x| > 3.

**Unlike regular TAN:** Circular tangent has vertical asymptotes every 180 degrees and is unbounded. Hyperbolic tangent is smooth, continuous everywhere, and bounded. They're mathematically quite different despite the similar names.

## Syntax

> [!f(x)] TANH Syntax
>
> ```
> =TANH(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | Any real number. Not an angle—just a plain number. Can be positive, negative, or zero. Works for any value without overflow issues. |

### Return Value

Returns the hyperbolic tangent: (e^x - e^(-x))/(e^x + e^(-x)). The result is always between -1 and 1 (exclusive), approaching but never reaching these limits. TANH(0) = 0. Returns #VALUE! if input is non-numeric. Never overflows for finite inputs.

## Examples

> [!f(x)] TANH Examples

### Example 1: Hyperbolic Tangent of Zero
```
=TANH(0)
```
**Result:** 0

**Explanation:** tanh(0) = (1 - 1)/(1 + 1) = 0. The function passes through the origin, just like regular tangent and sine.

---

### Example 2: Hyperbolic Tangent of 1
```
=TANH(1)
```
**Result:** 0.7615941560...

**Explanation:** tanh(1) is about 0.76—already more than halfway to the upper bound of 1.

---

### Example 3: Negative Input (Odd Function)
```
=TANH(-1)
```
**Result:** -0.7615941560...

**Explanation:** TANH is odd: tanh(-x) = -tanh(x). So TANH(-1) = -TANH(1).

---

### Example 4: Approaching the Upper Bound
```
=TANH(3)
```
**Result:** 0.9950547537...

**Explanation:** At x = 3, TANH is already 0.995—very close to 1. The function saturates quickly.

---

### Example 5: Very Large Value
```
=TANH(10)
```
**Result:** 0.9999999959...

**Explanation:** TANH(10) is essentially 1 (differs by about 4 * 10^-9). For practical purposes, tanh(x) equals 1 for x > 5.

---

### Example 6: Very Large Negative Value
```
=TANH(-10)
```
**Result:** -0.9999999959...

**Explanation:** For large negative x, TANH approaches -1. The function is symmetric about the origin.

---

### Example 7: Small Value Approximation
```
=TANH(0.1)
```
**Result:** 0.0996679946...

**Explanation:** For small x, tanh(x) is approximately x. TANH(0.1) is approximately 0.1.

---

### Example 8: Using the Definition
```
=(EXP(2) - EXP(-2)) / (EXP(2) + EXP(-2))
=TANH(2)
```
**Result:** Both return 0.9640275801...

**Explanation:** TANH is this exponential expression. The function provides a convenient shorthand.

---

### Example 9: Relationship to SINH and COSH
```
=SINH(2) / COSH(2)
=TANH(2)
```
**Result:** Both return 0.9640275801...

**Explanation:** tanh(x) = sinh(x)/cosh(x). This is the defining relationship with the other hyperbolic functions.

---

### Example 10: Neural Network Activation Comparison
```
=TANH(A1)       → Output: -1 to 1 (zero-centered)
=1/(1+EXP(-A1)) → Sigmoid output: 0 to 1
```
**Result:** Both are S-curves but with different ranges

**Explanation:** TANH is often preferred over sigmoid in neural networks because its output is zero-centered, which can help with gradient descent.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. |
| `#NAME?` | Misspelled function name | Check spelling: TANH, not TANHX or HYPERTAN. |
| `Confused with TAN` | Wrong function | TANH is hyperbolic (bounded -1 to 1). TAN is circular (unbounded with asymptotes). |
| `Expected angle input` | TANH doesn't use angles | Unlike TAN, TANH takes a plain number. No RADIANS() needed. |
| `Result outside -1 to 1` | Impossible | TANH always returns between -1 and 1. If you see otherwise, check your formula. |

## Use Cases

### [[Machine Learning: Neural Network Activation]]

**Scenario:** Apply non-linear activation in neural network layers.

**Implementation:**
```
=TANH(WeightedSum)                        → Activation output
=TANH(SUMPRODUCT(Inputs, Weights) + Bias) → Neuron output
=1 - TANH(A1)^2                           → Derivative for backpropagation
```

**Business Application:** Deep learning models, pattern recognition, natural language processing, recommendation systems.

**Technical Details:** TANH is a popular activation function because: (1) it's zero-centered (output averages around 0), (2) it has stronger gradients than sigmoid for values near zero, (3) the derivative is simple: 1 - tanh^2(x). However, it suffers from vanishing gradients for large |x|.

---

### [[Data Normalization and Scaling]]

**Scenario:** Compress unbounded data into a fixed range for analysis or visualization.

**Implementation:**
```
=TANH(Value / ScalingFactor)              → Map to (-1, 1)
=(TANH(A1) + 1) / 2                        → Map to (0, 1)
=TANH((Value - Mean) / StdDev)            → Normalize then compress
```

**Business Application:** Data preprocessing, feature engineering, handling outliers, creating bounded indicators.

**Technical Details:** TANH's S-curve gradually compresses extreme values toward the bounds. This is softer than hard clipping. The scaling factor controls how quickly saturation occurs.

---

### [[Physics: Relativistic Velocity]]

**Scenario:** Calculate velocity from rapidity in special relativity.

**Implementation:**
```
=c * TANH(Rapidity)                       → Velocity from rapidity
=ATANH(Velocity / c)                      → Rapidity from velocity
```

**Business Application:** Particle physics, relativistic calculations, educational physics software.

**Technical Details:** In special relativity, velocity v = c * tanh(rapidity), where c is the speed of light. This relationship keeps velocities below c (since tanh is bounded by 1), naturally enforcing the speed limit.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Formula:** (e^x - e^(-x))/(e^x + e^(-x))
- **Range:** Always returns between -1 and 1
- **No overflow:** Handles any finite input
- **Precision:** 15 significant digits

### Google Sheets
- **Availability:** All versions
- **Formula:** Same as Excel
- **Range:** Same bounds
- **Precision:** 15 significant digits

### Both Platforms
- Identical mathematical behavior
- No overflow concerns (unlike SINH and COSH)
- Results always strictly between -1 and 1

## Tips and Best Practices

1. **TANH is bounded:** Output is always between -1 and 1. This makes it useful for constraining values.

2. **Zero-centered output:** Unlike sigmoid (0 to 1), TANH outputs -1 to 1. This is often better for machine learning because the mean is 0.

3. **No angle conversion:** TANH takes a plain number. Don't use RADIANS()—that's for circular trig functions.

4. **Saturation happens fast:** By |x| = 3, TANH is already at 0.995. The "interesting" region is roughly -3 to +3.

5. **For small x, TANH(x) is approximately x:** Linear approximation works well near zero.

6. **The derivative is 1 - TANH^2:** Useful for machine learning backpropagation: derivative = 1 - output^2.

7. **Use for soft clipping:** Instead of hard-clipping values to a range, TANH provides smooth compression.

8. **Compare with sigmoid:** TANH(x) = 2*sigmoid(2x) - 1. They're related but have different ranges and properties.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TAN]] | Circular tangent (angle-based) | For geometric angles, slopes (unbounded) |
| [[SINH]] | Hyperbolic sine | When you need unbounded exponential-like growth |
| [[COSH]] | Hyperbolic cosine | For catenary curves, always positive |
| [[ATANH]] | Inverse hyperbolic tangent | To recover input from TANH output |

### Commonly Used Together

**[[SINH]] and [[COSH]]** - Related hyperbolic functions

*Definition relationship:*
```
=SINH(x) / COSH(x)    → Same as TANH(x)
```
TANH is the ratio of SINH to COSH.

---

**[[EXP]]** - For explicit formula

*Direct calculation:*
```
=(EXP(x) - EXP(-x)) / (EXP(x) + EXP(-x))    → Same as TANH(x)
```
The exponential definition of TANH.

---

**[[ATANH]]** - Inverse hyperbolic tangent

*Reverse the transformation:*
```
=ATANH(TANH(x))    → Returns x
```
Recover the original value from TANH output.

---

**[[SUMPRODUCT]]** - For neural network calculations

*Weighted sum before activation:*
```
=TANH(SUMPRODUCT(Inputs, Weights) + Bias)
```
Calculate neuron activation.

---

**[[ABS]]** - For saturation region detection

*Check if saturated:*
```
=IF(ABS(TANH(x)) > 0.99, "Saturated", "Active")
```
Identify when TANH has effectively reached its limits.

## Official Documentation

- **Microsoft Excel:** [TANH function](https://support.microsoft.com/en-us/office/tanh-function-017222f0-a0c3-4f7b-8f01-73a3e08b9e15)
- **Google Sheets:** [TANH function](https://support.google.com/docs/answer/3093588)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
