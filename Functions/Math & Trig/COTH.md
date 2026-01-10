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
- cotangent
- reciprocal-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Hyperbolic Cotangent
- Coth Function
- COTANH
tags:
- hyperbolic
- cotangent
- engineering
- scientific
- physics
---

# COTH

## Description

**COTH (Hyperbolic Cotangent)** returns the hyperbolic cotangent of a number. The hyperbolic cotangent is the reciprocal of the hyperbolic tangent: COTH(x) = 1/TANH(x) = COSH(x)/SINH(x). While circular trigonometric functions relate to the unit circle, hyperbolic functions relate to the unit hyperbola and appear throughout physics and engineering.

Hyperbolic functions describe naturally occurring phenomena that circular functions don't capture well: the shape of hanging cables (catenary), heat distribution, signal attenuation in transmission lines, and special relativistic velocity addition. COTH specifically appears in certain heat transfer equations, some electric field calculations, and specialized physics problems.

**The domain restriction:** COTH is undefined at x = 0 because SINH(0) = 0, making the ratio undefined. For all other real numbers, COTH is well-defined. The function approaches 1 as x approaches positive infinity, approaches -1 as x approaches negative infinity, and has a vertical asymptote at x = 0.

**Behavior characteristics:** Unlike the circular cotangent (COT) which oscillates infinitely, COTH approaches finite limits. For large positive x, COTH(x) is approximately 1; for large negative x, it's approximately -1. Near zero, the function grows without bound (positive from the right, negative from the left).

## Syntax

> [!f(x)] COTH Syntax
>
> ```
> =COTH(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | Any real number except 0. The value for which you want the hyperbolic cotangent. |

### Return Value

Returns the hyperbolic cotangent of the input number. For x > 0, result is > 1; for x < 0, result is < -1. Returns #DIV/0! if input is 0. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] COTH Examples

### Example 1: Positive Value
```
=COTH(1)
```
**Result:** 1.31303528549933

**Explanation:** COTH(1) = COSH(1)/SINH(1). The hyperbolic cotangent of 1 is approximately 1.313.

---

### Example 2: Larger Positive Value
```
=COTH(2)
```
**Result:** 1.03731472072755

**Explanation:** As x increases, COTH(x) approaches 1. At x=2, we're already quite close at 1.037.

---

### Example 3: Very Large Value
```
=COTH(10)
```
**Result:** 1.00000000041 (essentially 1)

**Explanation:** For large x, COTH(x) is virtually indistinguishable from 1. The hyperbolic tangent approaches 1, so its reciprocal also approaches 1.

---

### Example 4: Negative Value
```
=COTH(-1)
```
**Result:** -1.31303528549933

**Explanation:** COTH is an odd function: COTH(-x) = -COTH(x). The result is the negative of COTH(1).

---

### Example 5: Very Large Negative Value
```
=COTH(-10)
```
**Result:** -1.00000000041 (essentially -1)

**Explanation:** For large negative x, COTH(x) approaches -1, mirroring the positive case.

---

### Example 6: Small Positive Value
```
=COTH(0.1)
```
**Result:** 10.0333111322541

**Explanation:** Near zero, COTH grows rapidly. At 0.1, it's already over 10. For very small x, COTH(x) approaches 1/x.

---

### Example 7: Very Small Value
```
=COTH(0.01)
```
**Result:** 100.003333333556

**Explanation:** Approaching zero, COTH(x) approaches 1/x. At x=0.01, COTH is approximately 100.

---

### Example 8: Error Case - Zero
```
=COTH(0)
```
**Result:** #DIV/0!

**Explanation:** COTH(0) = COSH(0)/SINH(0) = 1/0, which is undefined. Zero is the only value where COTH is undefined.

---

### Example 9: Verifying Reciprocal Relationship
```
=COTH(2) * TANH(2)
```
**Result:** 1

**Explanation:** Since COTH = 1/TANH, their product is 1. Useful for verifying calculations.

---

### Example 10: Using Alternative Formula
```
=COSH(A1)/SINH(A1)
```
Where A1 = 1.5

**Result:** 1.10479293494073 (same as COTH(1.5))

**Explanation:** COTH(x) = COSH(x)/SINH(x). This formula works when COTH isn't available.

---

### Example 11: Cell Reference
```
=COTH(B2)
```
Where B2 contains 0.5

**Result:** 2.16395341373865

**Explanation:** References cell value for calculation. Useful in larger engineering models.

---

### Example 12: Near-Zero Behavior Demonstration
```
=COTH(0.001)
```
**Result:** 1000.00033333334

**Explanation:** Very close to zero, COTH(x) is approximately 1/x. At 0.001, result is approximately 1000.

---

### Example 13: Asymptotic Behavior Check
```
=COTH(5) - 1
```
**Result:** 0.000181583231 (very close to 0)

**Explanation:** At x=5, COTH is essentially 1. The difference from 1 is less than 0.0002.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Input is zero | Zero is the only undefined point. Check for zero input before calculating. |
| `#VALUE!` | Non-numeric input | Ensure input is a number. Check for text or empty cells. |
| `#NAME?` | Function not recognized | Ensure Excel 2013+ or Google Sheets. Use COSH(x)/SINH(x) in older versions. |
| `Result very large` | Input very close to zero | Expected behavior—COTH approaches infinity near zero. May indicate input error. |
| `Result near 1 or -1` | Large input magnitude | Expected behavior—COTH approaches +-1 for large |x|. Not an error. |

## Use Cases

### [[Heat Transfer - Fin Efficiency]]

**Scenario:** Calculate extended surface (fin) efficiency in heat exchanger design.

**Implementation:**
```
Fin Efficiency: =TANH(m * L) / (m * L)
// Or using COTH in alternative forms:
Parameter: =COTH(m * L) * (m * L)
```
Where m = fin parameter, L = fin length

**Business Application:** HVAC system design, electronics cooling, industrial heat exchanger optimization. Fin efficiency determines actual vs. theoretical heat transfer.

**Technical Details:** Hyperbolic functions arise naturally in the differential equations governing heat conduction in extended surfaces. COTH appears in certain boundary condition formulations.

---

### [[Electrical Engineering - Transmission Lines]]

**Scenario:** Calculate transmission line parameters and wave propagation characteristics.

**Implementation:**
```
Input Impedance: =Z0 * COTH(gamma * length)
// For open-circuited line
```
Where Z0 = characteristic impedance, gamma = propagation constant

**Business Application:** Power transmission design, RF engineering, telecommunications. Determine impedance matching and signal behavior.

**Technical Details:** Transmission line equations naturally involve hyperbolic functions. COTH appears for certain termination conditions (open circuit).

---

### [[Physics - Statistical Mechanics]]

**Scenario:** Calculate magnetic moment and susceptibility using quantum mechanical models.

**Implementation:**
```
Brillouin Function: =COTH(J*x) - (1/(2*J))*COTH(x/(2*J))
// Where J is angular momentum quantum number
```

**Business Application:** Materials science, magnetic material development, quantum computing research. Understanding magnetic properties at atomic scale.

**Technical Details:** The Brillouin function, which describes magnetic moment, contains COTH terms. Used in analyzing paramagnetic and ferromagnetic materials.

---

### [[Special Relativity]]

**Scenario:** Calculate relativistic velocity relationships using rapidity formulation.

**Implementation:**
```
Velocity Ratio: =TANH(rapidity)
Inverse Relation: =COTH(rapidity) - CSCH(rapidity)
```

**Business Application:** Particle physics, space mission planning, GPS satellite corrections. Relativistic effects require hyperbolic function formulation.

**Technical Details:** Rapidity (hyperbolic angle) provides additive velocity composition in relativity, unlike regular velocity. COTH appears in certain relativistic formulas.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Domain:** All real numbers except 0
- **Error handling:** Returns #DIV/0! at x=0
- **Precision:** 15 significant digits

### Google Sheets
- **Availability:** All versions
- **Domain:** Same as Excel
- **Error handling:** Same #DIV/0! behavior
- **Precision:** 15 significant digits

### Both Platforms
- Identical function name and syntax
- Same mathematical behavior
- Odd function: COTH(-x) = -COTH(x)
- Approaches +-1 for large |x|

### For Older Excel Versions (pre-2013)
COTH function not available. Use:
```
=COSH(x)/SINH(x)
=1/TANH(x)
=(EXP(x)+EXP(-x))/(EXP(x)-EXP(-x))
```

## Tips and Best Practices

1. **Check for zero input:** COTH is only undefined at zero. Use `=IF(A1=0, "Undefined", COTH(A1))` to handle this case.

2. **Remember it's odd:** COTH(-x) = -COTH(x). This symmetry can simplify formulas with signed inputs.

3. **Know the limits:** For large |x|, COTH approaches sign(x). This can help verify results and provide intuition.

4. **Small x approximation:** For |x| < 0.5, COTH(x) is approximately 1/x + x/3. Near zero, it behaves like 1/x.

5. **Verify with reciprocal:** COTH(x) * TANH(x) should equal 1. Use for calculation verification.

6. **For older Excel:** Use `=COSH(x)/SINH(x)` or `=1/TANH(x)` when COTH isn't available.

7. **Don't confuse with COT:** COT is circular cotangent (periodic, infinite singularities). COTH is hyperbolic (one singularity, bounded limits). Completely different functions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TANH]] | Returns the hyperbolic tangent | Most common hyperbolic trig function; COTH = 1/TANH |
| [[COT]] | Returns the circular cotangent | For regular (circular) trigonometry |
| [[COSH]] | Returns the hyperbolic cosine | When you need cosh, not coth |
| [[SINH]] | Returns the hyperbolic sine | When you need sinh, not coth |

### Commonly Used Together

**[[TANH]]** - Hyperbolic tangent (reciprocal)

*Verify calculations:*
```
=COTH(A1) * TANH(A1)  // Should equal 1
```
TANH and COTH are reciprocals.

---

**[[SINH]] / [[COSH]]** - Component functions

*Alternative calculation:*
```
=COSH(A1)/SINH(A1)  // Equivalent to COTH(A1)
```
Use when COTH isn't available or for clarity.

---

**[[CSCH]]** - Hyperbolic cosecant

*Related reciprocal function:*
```
CSCH(x) = 1/SINH(x)
// Often appears with COTH in physics formulas
```
Both are reciprocal hyperbolic functions.

---

**[[IFERROR]]** - Handle zero input

*Graceful error handling:*
```
=IFERROR(COTH(A1), "Undefined at zero")
```
Catch division by zero at x=0.

---

**[[EXP]]** - Fundamental definition

*Explicit exponential form:*
```
=(EXP(A1)+EXP(-A1))/(EXP(A1)-EXP(-A1))  // = COTH(A1)
```
Useful for understanding the function's definition.

## Official Documentation

- **Microsoft Excel:** [COTH function](https://support.microsoft.com/en-us/office/coth-function-2d373a66-a97b-4e77-8b93-7eea47315c55)
- **Google Sheets:** [COTH function](https://support.google.com/docs/answer/9084227)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Added as part of new hyperbolic function set |
| Excel 2016+ | Maintained | No changes |
| Google Sheets | 2019 | Added to match Excel functionality |

---

*Last updated: 2026-01-10*
