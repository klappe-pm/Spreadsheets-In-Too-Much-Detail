---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- trigonometry
- cotangent
- reciprocal-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cotangent
- Cotan
- CTG
tags:
- trigonometry
- cotangent
- reciprocal
- engineering
- scientific
---

# COT

## Description

**COT (Cotangent)** returns the cotangent of an angle specified in radians. The cotangent is the reciprocal of the tangent function: COT(x) = 1/TAN(x) = COS(x)/SIN(x). Geometrically, in a right triangle, the cotangent represents the ratio of the adjacent side to the opposite side for a given angle.

While tangent is the more commonly used function, cotangent has its own important applications, particularly in engineering, physics, and certain mathematical analyses. You'll encounter cotangent in antenna design, AC circuit analysis, signal processing, and various geometric calculations where it provides more natural formulations than using tangent.

**The domain restriction:** COT is undefined where SIN equals zero—that is, at 0, pi, 2*pi, 3*pi, and so on (all integer multiples of pi radians, or 0, 180, 360 degrees, etc.). At these points, COT returns a #DIV/0! error because you'd be dividing by zero. The function exists for all other real numbers.

**Radians, not degrees:** Like all Excel and Sheets trigonometric functions, COT expects angles in radians. To convert degrees to radians, use RADIANS() or multiply by PI()/180. A common mistake is passing degree values directly, which produces unexpected results (COT(45) is cotangent of 45 radians, not 45 degrees).

## Syntax

> [!f(x)] COT Syntax
>
> ```
> =COT(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The angle in radians for which you want the cotangent. Must not be a multiple of pi (0, pi, 2*pi, etc.). |

### Return Value

Returns the cotangent of the angle. Can be any real number (positive, negative, or very large in magnitude near undefined points). Returns #DIV/0! if angle is a multiple of pi. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] COT Examples

### Example 1: Cotangent of pi/4 (45 degrees)
```
=COT(PI()/4)
```
**Result:** 1

**Explanation:** At 45 degrees (pi/4 radians), sine and cosine are equal, so their ratio is 1. COT(pi/4) = COS(pi/4)/SIN(pi/4) = 1.

---

### Example 2: Cotangent of pi/6 (30 degrees)
```
=COT(PI()/6)
```
**Result:** 1.73205080756888 (approximately sqrt(3))

**Explanation:** COT(30 degrees) = sqrt(3), a standard trigonometric value. This is the adjacent/opposite ratio in a 30-60-90 triangle.

---

### Example 3: Cotangent of pi/3 (60 degrees)
```
=COT(PI()/3)
```
**Result:** 0.577350269189626 (approximately 1/sqrt(3))

**Explanation:** COT(60 degrees) = 1/sqrt(3). Notice COT(30) * COT(60) = sqrt(3) * 1/sqrt(3) = 1.

---

### Example 4: Converting from Degrees
```
=COT(RADIANS(45))
```
**Result:** 1

**Explanation:** Use RADIANS() to convert degree input. 45 degrees converted to radians, then cotangent calculated.

---

### Example 5: Cotangent of 1 Radian
```
=COT(1)
```
**Result:** 0.642092615934331

**Explanation:** 1 radian is about 57.3 degrees. The cotangent at this angle is approximately 0.642.

---

### Example 6: Negative Angle
```
=COT(-PI()/4)
```
**Result:** -1

**Explanation:** Cotangent is an odd function: COT(-x) = -COT(x). The cotangent of -45 degrees is -1.

---

### Example 7: Error Case - Zero
```
=COT(0)
```
**Result:** #DIV/0!

**Explanation:** At 0 radians, SIN(0) = 0, so COT(0) = COS(0)/SIN(0) = 1/0, which is undefined.

---

### Example 8: Error Case - Pi
```
=COT(PI())
```
**Result:** #DIV/0! (or very large number due to floating-point)

**Explanation:** At pi radians (180 degrees), SIN(pi) should be 0, making COT undefined. Due to floating-point, result may be very large instead of error.

---

### Example 9: Near Undefined Point
```
=COT(0.001)
```
**Result:** 999.999666666889 (approximately 1000)

**Explanation:** Very close to 0, cotangent approaches infinity. At small angles, COT(x) is approximately 1/x.

---

### Example 10: Verifying Reciprocal Relationship
```
=COT(1) * TAN(1)
```
**Result:** 1

**Explanation:** Since COT = 1/TAN, multiplying them gives 1 (within floating-point precision). Useful for verification.

---

### Example 11: Using Alternative Formula
```
=COS(A1)/SIN(A1)
```
Where A1 = 0.5

**Result:** 1.83048772171245 (same as COT(0.5))

**Explanation:** COT(x) = COS(x)/SIN(x). This formula works when COT function isn't available or for clarity.

---

### Example 12: Cell Reference
```
=COT(B2)
```
Where B2 contains angle in radians

**Result:** Cotangent of the angle in B2

**Explanation:** Reference cells containing radian values for dynamic calculations.

---

### Example 13: Cotangent in Engineering Context
```
=impedance * COT(2 * PI() * frequency * time)
```
**Result:** Phase-dependent component calculation

**Explanation:** Cotangent appears in AC circuit analysis and wave calculations. Combined with frequency and time.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Angle is a multiple of pi (0, pi, 2*pi, etc.) | Check if SIN of angle is zero. Handle edge cases with IF or IFERROR. |
| `#VALUE!` | Non-numeric input | Ensure input is a number. Check for text or empty cells. |
| `#NAME?` | Function not recognized | Ensure Excel 2013+ or Google Sheets. Use 1/TAN(x) in older versions. |
| `Unexpected result` | Angle in degrees instead of radians | Use RADIANS(degrees) to convert. 45 degrees = RADIANS(45), not just 45. |
| `Very large result` | Angle very close to multiple of pi | Expected behavior—COT approaches infinity near undefined points. |

## Use Cases

### [[Electrical Engineering - AC Circuits]]

**Scenario:** Calculate reactive power factor and phase relationships in alternating current circuits.

**Implementation:**
```
Power Factor Angle: =COT(power_factor_angle)
Reactance Ratio: =COT(2 * PI() * frequency * time_constant)
```

**Business Application:** Power system design, motor analysis, transformer calculations. Cotangent naturally expresses certain phase relationships.

**Technical Details:** In AC analysis, cotangent of phase angle relates reactive and resistive components. Sometimes more natural than tangent for specific circuit topologies.

---

### [[Antenna Design]]

**Scenario:** Calculate antenna element spacing and radiation pattern parameters.

**Implementation:**
```
Element Spacing: =wavelength / 2 * COT(radiation_angle)
Pattern Factor: =COT(PI() * element_length / wavelength * SIN(angle))
```

**Business Application:** Telecommunications, broadcast engineering, radar systems. Antenna gain calculations often involve cotangent functions.

**Technical Details:** Dipole and array antenna calculations use cotangent for element interactions. Critical for optimizing signal strength and directionality.

---

### [[Surveying and Navigation]]

**Scenario:** Calculate distances and positions using triangulation where cotangent provides direct results.

**Implementation:**
```
Distance: =height * COT(RADIANS(elevation_angle))
```

**Business Application:** Land surveying, civil engineering, GPS-independent navigation. Calculate horizontal distance from height and angle.

**Technical Details:** Given vertical height and angle of elevation, cotangent directly gives horizontal distance without needing to calculate tangent first.

---

### [[Mathematical Analysis]]

**Scenario:** Evaluate mathematical expressions and series involving cotangent.

**Implementation:**
```
Cotangent Series Term: =COT(n * PI() / points)
Partial Fraction: =1/2 * COT(x/2)
```

**Business Application:** Scientific computing, numerical analysis, mathematical research. Certain infinite series and integrals have cotangent in closed forms.

**Technical Details:** Cotangent appears in Fourier analysis, complex analysis residue calculations, and various summation formulas.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Domain:** All real numbers except multiples of pi
- **Error handling:** Returns #DIV/0! at singularities (or very large numbers due to floating-point)
- **Precision:** 15 significant digits

### Google Sheets
- **Availability:** All versions
- **Domain:** Same as Excel
- **Error handling:** Same #DIV/0! behavior
- **Precision:** 15 significant digits

### Both Platforms
- Input must be in radians
- Odd function: COT(-x) = -COT(x)
- Equivalent to 1/TAN(x) or COS(x)/SIN(x)
- Same floating-point precision limitations near singularities

### For Older Excel Versions (pre-2013)
COT function not available. Use:
```
=1/TAN(angle)
=COS(angle)/SIN(angle)
```

## Tips and Best Practices

1. **Always use radians:** COT expects radians. Use RADIANS(degrees) to convert: `=COT(RADIANS(45))` not `=COT(45)`.

2. **Handle undefined points:** COT is undefined at 0, pi, 2*pi, etc. Use IFERROR or check if MOD(angle, PI()) is near zero.

3. **Use for direct calculations:** When a formula naturally involves cotangent, use COT directly rather than converting from tangent. It's clearer and avoids division errors.

4. **Remember it's odd:** COT(-x) = -COT(x). This symmetry can simplify formulas involving signed angles.

5. **Verify with reciprocal:** COT(x) * TAN(x) should equal 1. Use this to verify calculations.

6. **For older Excel:** If COT isn't available, use `=1/TAN(x)` or `=COS(x)/SIN(x)`. Both are mathematically equivalent.

7. **Near-zero caution:** For angles near 0 or pi, cotangent values become extremely large. Consider whether your application needs special handling.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TAN]] | Returns the tangent | Most common trig function; COT = 1/TAN |
| [[COTH]] | Returns the hyperbolic cotangent | For hyperbolic (not circular) cotangent |
| [[COS]] | Returns the cosine | When you need cosine, not cotangent |
| [[SIN]] | Returns the sine | When you need sine, not cotangent |

### Commonly Used Together

**[[TAN]]** - Tangent (reciprocal relationship)

*Verify cotangent calculations:*
```
=COT(A1) * TAN(A1)  // Should equal 1
```
Tangent and cotangent are reciprocals.

---

**[[RADIANS]]** - Convert degrees to radians

*Use degree inputs:*
```
=COT(RADIANS(60))  // Cotangent of 60 degrees
```
Essential for working with degree measurements.

---

**[[PI]]** - Mathematical constant pi

*Standard angles:*
```
=COT(PI()/6)   // 30 degrees
=COT(PI()/4)   // 45 degrees
=COT(PI()/3)   // 60 degrees
```
Construct exact radian values for standard angles.

---

**[[IFERROR]]** - Handle undefined points

*Graceful error handling:*
```
=IFERROR(COT(A1), "Undefined")
```
Catch division by zero at singularities.

---

**[[COS]] / [[SIN]]** - Alternative calculation

*Explicit ratio calculation:*
```
=COS(A1)/SIN(A1)  // Equivalent to COT(A1)
```
Use when COT function unavailable or for clarity.

## Official Documentation

- **Microsoft Excel:** [COT function](https://support.microsoft.com/en-us/office/cot-function-c446f34d-6fe4-40dc-84f8-cf59e5f5e31a)
- **Google Sheets:** [COT function](https://support.google.com/docs/answer/9084169)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Added as part of new trig function set |
| Excel 2016+ | Maintained | No changes |
| Google Sheets | 2019 | Added to match Excel functionality |

---

*Last updated: 2026-01-10*
