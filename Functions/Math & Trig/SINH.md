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
- sinh
- exponential
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Hyperbolic Sine
- Hyperbolic Sin
tags:
- hyperbolic
- sinh
- exponential
- engineering
- physics
---

# SINH

## Description

**SINH** returns the hyperbolic sine of a number. Despite the similar name, hyperbolic functions are fundamentally different from regular (circular) trigonometric functions. While SIN relates to circles and rotation, SINH relates to hyperbolas and exponential growth. The formula is: sinh(x) = (e^x - e^(-x)) / 2, where e is Euler's number (approximately 2.71828).

**Where do hyperbolic functions come from?** Regular trig functions describe the unit circle (x = cos(t), y = sin(t)). Hyperbolic functions describe the unit hyperbola (x = cosh(t), y = sinh(t)). They arise naturally in physics, engineering, and mathematics—particularly in problems involving cables hanging under their own weight (catenary curves), relativistic physics, and solutions to certain differential equations.

**Key properties of SINH:** Unlike SIN which is bounded between -1 and 1, SINH is unbounded—it grows exponentially for large inputs. SINH(0) = 0, just like SIN(0). SINH is an odd function: sinh(-x) = -sinh(x). For small values of x, sinh(x) is approximately equal to x. For large positive x, sinh(x) approaches e^x/2. The hyperbolic identity cosh^2(x) - sinh^2(x) = 1 mirrors the circular identity cos^2 + sin^2 = 1.

**Important:** SINH takes a plain number as input, NOT an angle in radians or degrees. The input x in sinh(x) is a dimensionless "hyperbolic angle" parameter, not a geometric angle. There's no need for RADIANS() conversion—just pass the number directly.

## Syntax

> [!f(x)] SINH Syntax
>
> ```
> =SINH(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | Any real number. This is not an angle—it's a parameter for the hyperbolic function. Typically small to moderate values (-700 to 700 to avoid overflow). |

### Return Value

Returns the hyperbolic sine of the number: (e^x - e^(-x)) / 2. For small x, result is approximately x. For large positive x, result approaches e^x/2 (exponential growth). For large negative x, result approaches -e^(-x)/2 (negative exponential). Returns #VALUE! if input is non-numeric. May return #NUM! for extremely large inputs due to overflow.

## Examples

> [!f(x)] SINH Examples

### Example 1: Hyperbolic Sine of Zero
```
=SINH(0)
```
**Result:** 0

**Explanation:** sinh(0) = (e^0 - e^0) / 2 = (1 - 1) / 2 = 0. Like regular sine, hyperbolic sine passes through the origin.

---

### Example 2: Hyperbolic Sine of 1
```
=SINH(1)
```
**Result:** 1.1752011936...

**Explanation:** sinh(1) = (e^1 - e^(-1)) / 2 = (2.718 - 0.368) / 2 = approximately 1.175. This is slightly larger than 1.

---

### Example 3: Small Value Approximation
```
=SINH(0.1)
```
**Result:** 0.10016675002...

**Explanation:** For small x, sinh(x) is approximately equal to x. SINH(0.1) is approximately 0.1, with small deviation.

---

### Example 4: Negative Input
```
=SINH(-1)
```
**Result:** -1.1752011936...

**Explanation:** SINH is an odd function: sinh(-x) = -sinh(x). So SINH(-1) = -SINH(1).

---

### Example 5: Larger Value
```
=SINH(5)
```
**Result:** 74.203210578...

**Explanation:** For larger inputs, hyperbolic sine grows exponentially. SINH(5) is approximately e^5/2 = 148.4/2 = 74.2.

---

### Example 6: Very Large Value
```
=SINH(10)
```
**Result:** 11013.232874...

**Explanation:** SINH grows very rapidly. SINH(10) is already over 11,000. Compare to SIN(10) which is only about -0.544.

---

### Example 7: Comparing to Exponential
```
=SINH(20)
=EXP(20)/2
```
**Result:** Both approximately 242582598

**Explanation:** For large x, sinh(x) approaches e^x/2. At x=20, the approximation is very accurate.

---

### Example 8: The Hyperbolic Identity
```
=COSH(3)^2 - SINH(3)^2
```
**Result:** 1

**Explanation:** cosh^2(x) - sinh^2(x) = 1, analogous to the Pythagorean identity for circular trig. This holds for any value of x.

---

### Example 9: Using the Exponential Definition
```
=(EXP(2) - EXP(-2)) / 2
=SINH(2)
```
**Result:** Both return 3.6268604078...

**Explanation:** The formula definition. SINH is a convenient shorthand for this exponential expression.

---

### Example 10: Catenary Calculation
```
=A * SINH(X / A)
```
**Result:** Y-coordinate of catenary curve

**Explanation:** The catenary curve (shape of a hanging chain) uses hyperbolic functions. Given parameter A and x-position, SINH helps calculate the curve.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. |
| `#NUM!` | Input too large (overflow) | Keep input roughly between -710 and 710. Beyond this, e^x overflows. |
| `#NAME?` | Misspelled function name | Check spelling: SINH, not SINHX or HYPERSIN. |
| `Confused with SIN` | Wrong function entirely | SINH is hyperbolic, SIN is circular. They're mathematically different. |
| `Expected radians input` | SINH doesn't use angles | Unlike SIN, SINH takes a plain number, not an angle. No RADIANS() needed. |

## Use Cases

### [[Engineering: Catenary and Cable Calculations]]

**Scenario:** Calculate the shape of cables, chains, or arches hanging under their own weight.

**Implementation:**
```
=CatenaryConstant * COSH(X / CatenaryConstant)    → Y height of catenary
=A * SINH(SpanHalf / A)                            → Sag calculation
=SQRT(1 + SINH(X/A)^2)                             → Arc length factor
```

**Business Application:** Suspension bridge design, power line sag calculation, architectural catenary arches (like the Gateway Arch), cable car engineering.

**Technical Details:** The catenary curve y = a*cosh(x/a) describes ideal flexible cables. The tension and sag involve both SINH and COSH. These calculations are essential for structural engineering.

---

### [[Physics: Special Relativity]]

**Scenario:** Calculate relativistic velocities, rapidities, and Lorentz transformations.

**Implementation:**
```
=c * TANH(Rapidity)                       → Velocity from rapidity
=SINH(Rapidity) / SQRT(1 + SINH(Rapidity)^2)  → Beta factor
=Energy * SINH(Rapidity)                   → Momentum calculation
```

**Business Application:** Particle physics calculations, aerospace relativistic corrections, GPS satellite relativistic adjustments.

**Technical Details:** Rapidity (hyperbolic angle) adds linearly for collinear velocities, unlike regular velocities. Hyperbolic trig simplifies relativistic formulas significantly.

---

### [[Electrical Engineering: Transmission Lines]]

**Scenario:** Model signal propagation, impedance, and reflections in transmission lines.

**Implementation:**
```
=Zo * SINH(Gamma * Length)                 → Input impedance component
=SINH(PropagationConstant * Distance)      → Voltage distribution factor
```

**Business Application:** RF circuit design, telecommunications, power transmission analysis, antenna feed calculations.

**Technical Details:** Transmission line equations naturally involve hyperbolic functions. SINH and COSH describe how voltage and current vary along the line based on impedance and propagation characteristics.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Formula:** (e^x - e^(-x)) / 2
- **Overflow:** Returns #NUM! for inputs beyond approximately 710
- **Precision:** 15 significant digits

### Google Sheets
- **Availability:** All versions
- **Formula:** Same as Excel
- **Overflow:** Same behavior
- **Precision:** 15 significant digits

### Both Platforms
- Identical mathematical behavior
- Same overflow limits
- No functional differences

## Tips and Best Practices

1. **Don't confuse with SIN:** SINH is hyperbolic, SIN is circular. They have completely different behaviors and applications.

2. **No angle conversion needed:** Unlike SIN, SINH takes a plain number, not an angle. Don't use RADIANS() with SINH.

3. **Watch for overflow:** SINH grows exponentially. For x > 710, e^x overflows IEEE 754 doubles. Keep inputs reasonable.

4. **For small x, sinh(x) is approximately x:** Useful for approximations and understanding behavior near zero.

5. **Use the identity for verification:** cosh^2(x) - sinh^2(x) should always equal 1. This tests your calculations.

6. **SINH is odd:** sinh(-x) = -sinh(x). This can simplify formulas or catch sign errors.

7. **For large x, sinh(x) is approximately e^x/2:** Useful for understanding asymptotic behavior and making approximations.

8. **Pair with COSH and TANH:** Hyperbolic functions typically appear together in formulas, just like sin, cos, and tan.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SIN]] | Circular sine (angle-based) | For circular/rotational problems, wave analysis, triangles |
| [[COSH]] | Hyperbolic cosine | The partner to SINH for hyperbolic calculations |
| [[TANH]] | Hyperbolic tangent | For bounded transformations, neural networks |
| [[ASINH]] | Inverse hyperbolic sine | To find the parameter from a sinh value |
| [[EXP]] | Exponential function | SINH is defined in terms of EXP |

### Commonly Used Together

**[[COSH]]** - Hyperbolic cosine

*Complete hyperbolic calculations:*
```
Catenary: =A * COSH(X / A)
Identity: =COSH(x)^2 - SINH(x)^2    → Always equals 1
```
SINH and COSH typically appear together.

---

**[[TANH]]** - Hyperbolic tangent

*Alternative formulation:*
```
=SINH(x) / COSH(x)    → Same as TANH(x)
```
The ratio of SINH to COSH.

---

**[[EXP]]** - Exponential function

*The defining relationship:*
```
=(EXP(x) - EXP(-x)) / 2    → Same as SINH(x)
```
SINH is built from exponentials.

---

**[[ASINH]]** - Inverse hyperbolic sine

*Reverse calculation:*
```
=ASINH(SINH(x))    → Returns x
```
To recover the input from a SINH value.

---

**[[SQRT]]** - For related calculations

*Arc length factor:*
```
=SQRT(1 + SINH(x)^2)    → Same as COSH(x)
```
Another expression of the hyperbolic identity.

## Official Documentation

- **Microsoft Excel:** [SINH function](https://support.microsoft.com/en-us/office/sinh-function-1e4e8b9f-2b65-43fc-ab8a-0a37f4c1427d)
- **Google Sheets:** [SINH function](https://support.google.com/docs/answer/3093519)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
