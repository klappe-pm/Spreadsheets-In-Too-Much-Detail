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
- cosh
- exponential
- catenary
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Hyperbolic Cosine
- Hyperbolic Cos
tags:
- hyperbolic
- cosh
- exponential
- catenary
- engineering
---

# COSH

## Description

**COSH** returns the hyperbolic cosine of a number. The formula is: cosh(x) = (e^x + e^(-x)) / 2. Unlike regular cosine which oscillates between -1 and 1, hyperbolic cosine is always greater than or equal to 1, forming a U-shaped curve. COSH is most famous for describing the **catenary**—the curve formed by a chain or cable hanging under its own weight.

**The catenary connection:** When you hang a chain between two points and let it droop naturally, it forms a catenary curve described by y = a * cosh(x/a). This is NOT a parabola (a common misconception). The Gateway Arch in St. Louis is an inverted catenary. Power lines between poles follow catenary curves. Understanding COSH is essential for structural engineering involving cables, arches, and suspended structures.

**Key properties of COSH:** COSH is always positive and has a minimum value of 1 at x = 0. It's an even function: cosh(-x) = cosh(x), meaning the curve is symmetric about the y-axis. For large |x|, cosh(x) approaches e^|x|/2 (exponential growth). The hyperbolic identity cosh^2(x) - sinh^2(x) = 1 is fundamental. For small x, cosh(x) is approximately 1 + x^2/2.

**Important:** Like SINH, COSH takes a plain number as input, not an angle in degrees or radians. The parameter x is dimensionless. No conversion is needed—just pass the number directly.

## Syntax

> [!f(x)] COSH Syntax
>
> ```
> =COSH(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | Any real number. This is not an angle—it's a parameter for the hyperbolic function. Typically small to moderate values (-710 to 710 to avoid overflow). |

### Return Value

Returns the hyperbolic cosine of the number: (e^x + e^(-x)) / 2. The result is always >= 1, with minimum at COSH(0) = 1. For large |x|, grows exponentially. Returns #VALUE! if input is non-numeric. May return #NUM! for extremely large |x| due to overflow.

## Examples

> [!f(x)] COSH Examples

### Example 1: Hyperbolic Cosine of Zero
```
=COSH(0)
```
**Result:** 1

**Explanation:** cosh(0) = (e^0 + e^0) / 2 = (1 + 1) / 2 = 1. This is the minimum value of COSH. Unlike COS(0) = 1, both functions return 1 at zero.

---

### Example 2: Hyperbolic Cosine of 1
```
=COSH(1)
```
**Result:** 1.5430806348...

**Explanation:** cosh(1) = (e + 1/e) / 2 = (2.718 + 0.368) / 2 = approximately 1.543.

---

### Example 3: Negative Input (Even Function)
```
=COSH(-1)
```
**Result:** 1.5430806348...

**Explanation:** COSH is an even function: cosh(-x) = cosh(x). So COSH(-1) = COSH(1). The curve is symmetric about the y-axis.

---

### Example 4: Small Value Approximation
```
=COSH(0.1)
```
**Result:** 1.0050041681...

**Explanation:** For small x, cosh(x) is approximately 1 + x^2/2. COSH(0.1) is approximately 1 + 0.01/2 = 1.005.

---

### Example 5: Moderate Value
```
=COSH(3)
```
**Result:** 10.067661996...

**Explanation:** COSH grows faster than linear but slower than pure exponential near zero. By x = 3, it's about 10.

---

### Example 6: Large Value
```
=COSH(10)
```
**Result:** 11013.232920...

**Explanation:** For large x, cosh(x) approaches e^x/2. COSH(10) is approximately e^10/2 = 22026/2 = 11013.

---

### Example 7: The Hyperbolic Identity
```
=COSH(5)^2 - SINH(5)^2
```
**Result:** 1

**Explanation:** cosh^2(x) - sinh^2(x) = 1 for all x. This is the hyperbolic analog of sin^2 + cos^2 = 1.

---

### Example 8: Catenary Curve Point
```
=10 * COSH(5 / 10)
```
**Result:** 12.76259652...

**Explanation:** For a catenary with parameter a = 10, the height at x = 5 is a * cosh(x/a) = 10 * cosh(0.5) = 12.76 units.

---

### Example 9: Comparing with Exponential
```
=COSH(20)
=EXP(20)/2
```
**Result:** Both approximately 242582598

**Explanation:** For large x, cosh(x) is approximately e^x/2. The e^(-x) term becomes negligible.

---

### Example 10: Using the Definition
```
=(EXP(3) + EXP(-3)) / 2
=COSH(3)
```
**Result:** Both return 10.067661996...

**Explanation:** COSH is defined as this exponential expression. The function is a convenient shorthand.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. |
| `#NUM!` | Input too large (overflow) | Keep input roughly between -710 and 710. |
| `#NAME?` | Misspelled function name | Check spelling: COSH, not COSHX or HYPERCOS. |
| `Confused with COS` | Wrong function | COSH is hyperbolic, COS is circular. Different math. |
| `Expected angle input` | COSH doesn't use angles | Unlike COS, COSH takes a plain number. No RADIANS() needed. |
| `Result less than 1` | Impossible | COSH always returns >= 1. If you see less, check your formula. |

## Use Cases

### [[Structural Engineering: Catenary Curves]]

**Scenario:** Calculate the shape of hanging cables, suspension bridges, and catenary arches.

**Implementation:**
```
=A * COSH(X / A)                           → Catenary height at position X
=A * (COSH(Span/(2*A)) - 1)                → Maximum sag at midpoint
=A * SINH(Span/(2*A)) * 2                  → Arc length of cable
```

**Business Application:** Power line design, suspension bridge cables, catenary arches (Gateway Arch), cable car systems, tent and membrane structures.

**Technical Details:** The catenary y = a*cosh(x/a) describes ideal flexible cables under uniform gravity. The parameter 'a' relates to cable tension and weight per unit length. Real cables require adjustments for elasticity.

---

### [[Physics: Relativity and Particle Physics]]

**Scenario:** Calculate Lorentz factors, energy-momentum relations, and relativistic transformations.

**Implementation:**
```
=COSH(Rapidity)                            → Lorentz gamma factor
=RestMass * COSH(Rapidity)                 → Relativistic energy / c^2
=COSH(Rapidity)^2 - SINH(Rapidity)^2       → Verifies = 1 (invariant)
```

**Business Application:** Particle accelerator design, GPS relativistic corrections, high-energy physics calculations.

**Technical Details:** Using rapidity (hyperbolic angle) instead of velocity simplifies many relativistic formulas. Lorentz transformations become hyperbolic rotations.

---

### [[Electrical Engineering: Transmission Lines]]

**Scenario:** Analyze voltage and current distribution in long transmission lines.

**Implementation:**
```
=Vs * COSH(Gamma * Length) + Is * Zo * SINH(Gamma * Length)  → Receiving end voltage
=COSH(PropagationConstant * Distance)      → Voltage ratio factor
```

**Business Application:** Power grid design, RF transmission analysis, telecommunications, antenna systems.

**Technical Details:** Transmission line equations involve hyperbolic functions due to the wave equation solutions. COSH terms represent forward and backward wave combinations.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Formula:** (e^x + e^(-x)) / 2
- **Overflow:** Returns #NUM! for |x| beyond approximately 710
- **Precision:** 15 significant digits

### Google Sheets
- **Availability:** All versions
- **Formula:** Same as Excel
- **Overflow:** Same behavior
- **Precision:** 15 significant digits

### Both Platforms
- Identical mathematical behavior
- Same overflow limits
- Always returns values >= 1 for real inputs

## Tips and Best Practices

1. **COSH is always >= 1:** The minimum is COSH(0) = 1. If you get results less than 1, check your formula.

2. **COSH is even:** cosh(-x) = cosh(x). The function is symmetric, so you can simplify calculations involving negative values.

3. **No angle conversion needed:** COSH takes a plain number, not an angle. Don't use RADIANS() with COSH.

4. **For small x, use approximation:** cosh(x) is approximately 1 + x^2/2 for small x. Useful for quick estimates.

5. **Use the identity for checking:** cosh^2(x) - sinh^2(x) should always equal 1. Great for verifying calculations.

6. **Watch for overflow:** COSH grows exponentially. For |x| > 710, you'll get overflow errors.

7. **The catenary formula:** y = a * COSH(x/a) is the shape of a hanging chain. The parameter 'a' controls the "tightness."

8. **Pair with SINH and TANH:** Hyperbolic functions appear together in most applications, just like circular trig functions.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COS]] | Circular cosine (angle-based) | For circular/rotational problems, wave analysis |
| [[SINH]] | Hyperbolic sine | The partner to COSH; often used together |
| [[TANH]] | Hyperbolic tangent | For bounded transformations, S-curves |
| [[ACOSH]] | Inverse hyperbolic cosine | To find parameter from a cosh value |
| [[EXP]] | Exponential function | COSH is defined in terms of EXP |

### Commonly Used Together

**[[SINH]]** - Hyperbolic sine

*Complete hyperbolic calculations:*
```
Identity: =COSH(x)^2 - SINH(x)^2    → Always equals 1
Catenary slope: =SINH(x/a)          → Derivative of catenary
```
COSH and SINH are natural partners.

---

**[[TANH]]** - Hyperbolic tangent

*Related calculation:*
```
=SINH(x) / COSH(x)    → Same as TANH(x)
```
TANH is the ratio of SINH to COSH.

---

**[[EXP]]** - Exponential function

*The defining formula:*
```
=(EXP(x) + EXP(-x)) / 2    → Same as COSH(x)
```
COSH is the average of e^x and e^(-x).

---

**[[ACOSH]]** - Inverse hyperbolic cosine

*Reverse calculation:*
```
=ACOSH(COSH(x))    → Returns |x| (not x, because COSH is even)
```
To recover parameter from a COSH value.

---

**[[SQRT]]** - For identity calculations

*Express COSH from SINH:*
```
=SQRT(1 + SINH(x)^2)    → Same as COSH(x)
```
Derived from the hyperbolic identity.

## Official Documentation

- **Microsoft Excel:** [COSH function](https://support.microsoft.com/en-us/office/cosh-function-e460d426-c471-43e8-9540-a57ff3b70555)
- **Google Sheets:** [COSH function](https://support.google.com/docs/answer/3093479)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
