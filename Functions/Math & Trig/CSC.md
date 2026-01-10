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
- cosecant
- reciprocal-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cosecant
- Cosec
- 1/SIN
tags:
- trigonometry
- cosecant
- reciprocal
- engineering
- scientific
---

# CSC

## Description

**CSC (Cosecant)** returns the cosecant of an angle specified in radians. The cosecant is the reciprocal of the sine function: CSC(x) = 1/SIN(x). Geometrically, in a right triangle, the cosecant represents the ratio of the hypotenuse to the opposite side for a given angle.

While sine is ubiquitous in mathematics and engineering, cosecant has specific applications where it provides more natural or direct formulations. You'll encounter cosecant in certain physics problems, signal processing calculations, and mathematical analyses where working with the reciprocal is more convenient than dealing with 1/SIN(x) repeatedly.

**The domain restriction:** CSC is undefined wherever SIN equals zero—that is, at 0, pi, 2*pi, 3*pi, and all integer multiples of pi radians (0, 180, 360 degrees, etc.). At these points, CSC returns a #DIV/0! error because you'd be dividing by zero. The function has vertical asymptotes at each of these points.

**Range characteristics:** CSC(x) is never between -1 and 1. It's always >= 1 or <= -1. When SIN(x) is close to 1 or -1, CSC(x) is also close to 1 or -1. When SIN(x) is close to 0, CSC(x) becomes very large in magnitude.

## Syntax

> [!f(x)] CSC Syntax
>
> ```
> =CSC(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The angle in radians for which you want the cosecant. Must not be a multiple of pi (0, pi, 2*pi, etc.). |

### Return Value

Returns the cosecant of the angle. Result is always >= 1 or <= -1 (never between -1 and 1). Returns #DIV/0! if angle is a multiple of pi. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] CSC Examples

### Example 1: Cosecant of pi/2 (90 degrees)
```
=CSC(PI()/2)
```
**Result:** 1

**Explanation:** At 90 degrees (pi/2 radians), SIN = 1, so CSC = 1/1 = 1. This is the minimum positive value CSC can return.

---

### Example 2: Cosecant of pi/6 (30 degrees)
```
=CSC(PI()/6)
```
**Result:** 2

**Explanation:** SIN(30 degrees) = 0.5, so CSC(30 degrees) = 1/0.5 = 2. A standard trigonometric value.

---

### Example 3: Cosecant of pi/4 (45 degrees)
```
=CSC(PI()/4)
```
**Result:** 1.41421356237309 (sqrt(2))

**Explanation:** SIN(45 degrees) = 1/sqrt(2), so CSC(45 degrees) = sqrt(2), approximately 1.414.

---

### Example 4: Cosecant of pi/3 (60 degrees)
```
=CSC(PI()/3)
```
**Result:** 1.15470053837925 (2/sqrt(3))

**Explanation:** SIN(60 degrees) = sqrt(3)/2, so CSC(60 degrees) = 2/sqrt(3), approximately 1.155.

---

### Example 5: Converting from Degrees
```
=CSC(RADIANS(30))
```
**Result:** 2

**Explanation:** Use RADIANS() to convert degree input. 30 degrees converted to radians, then cosecant calculated.

---

### Example 6: Negative Angle
```
=CSC(-PI()/2)
```
**Result:** -1

**Explanation:** Cosecant is an odd function: CSC(-x) = -CSC(x). CSC of -90 degrees is -1.

---

### Example 7: Error Case - Zero
```
=CSC(0)
```
**Result:** #DIV/0!

**Explanation:** At 0 radians, SIN(0) = 0, so CSC(0) = 1/0, which is undefined.

---

### Example 8: Error Case - Pi
```
=CSC(PI())
```
**Result:** #DIV/0! (or very large number due to floating-point)

**Explanation:** At pi radians (180 degrees), SIN(pi) = 0, making CSC undefined.

---

### Example 9: Near Undefined Point
```
=CSC(0.01)
```
**Result:** 100.001666683334

**Explanation:** Very close to 0, sine is approximately equal to the angle, so CSC is approximately 1/angle. At 0.01, CSC is about 100.

---

### Example 10: Verifying Reciprocal Relationship
```
=CSC(1) * SIN(1)
```
**Result:** 1

**Explanation:** Since CSC = 1/SIN, multiplying them gives 1. Useful for verification.

---

### Example 11: Cell Reference
```
=CSC(B2)
```
Where B2 contains PI()/6

**Result:** 2

**Explanation:** Reference cells containing radian values for dynamic calculations.

---

### Example 12: Using Alternative Formula
```
=1/SIN(A1)
```
Where A1 = PI()/4

**Result:** 1.41421356237309 (same as CSC(PI()/4))

**Explanation:** CSC(x) = 1/SIN(x). Use when CSC function isn't available.

---

### Example 13: In Third Quadrant
```
=CSC(4*PI()/3)
```
**Result:** -1.15470053837925

**Explanation:** 4*PI()/3 is 240 degrees (third quadrant). Sine is negative there, so cosecant is also negative.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Angle is a multiple of pi (0, pi, 2*pi, etc.) | Check if SIN of angle is zero. Handle edge cases with IF or IFERROR. |
| `#VALUE!` | Non-numeric input | Ensure input is a number. Check for text or empty cells. |
| `#NAME?` | Function not recognized | Ensure Excel 2013+ or Google Sheets. Use 1/SIN(x) in older versions. |
| `Unexpected result` | Angle in degrees instead of radians | Use RADIANS(degrees) to convert. 30 degrees = RADIANS(30), not just 30. |
| `Very large result` | Angle very close to multiple of pi | Expected behavior—CSC approaches infinity near undefined points. |

## Use Cases

### [[Physics - Wave Calculations]]

**Scenario:** Calculate wave amplitude relationships and standing wave patterns.

**Implementation:**
```
Amplitude Factor: =CSC(phase_angle)
Standing Wave Ratio: =CSC(k * position)
```
Where k = wave number, position = location along wave

**Business Application:** Acoustics engineering, radio frequency design, optics. Wave interference and resonance calculations.

**Technical Details:** Standing waves in pipes, strings, and electromagnetic cavities involve cosecant functions in their amplitude descriptions at certain points.

---

### [[Electrical Engineering - Impedance]]

**Scenario:** Calculate reactive impedance components in AC circuits at specific phase angles.

**Implementation:**
```
Impedance Magnitude: =base_impedance * CSC(phase_angle)
```

**Business Application:** Power system design, filter design, transformer analysis. Phase-dependent calculations.

**Technical Details:** Certain circuit topologies have impedance expressions involving cosecant of the phase angle. Useful in resonant circuit analysis.

---

### [[Optics - Refraction Calculations]]

**Scenario:** Calculate critical angles and total internal reflection parameters.

**Implementation:**
```
// Using cosecant in Snell's law rearrangements
Refraction Factor: =n2/n1 * CSC(incident_angle)
```

**Business Application:** Lens design, fiber optics, optical sensor development. Light behavior at material interfaces.

**Technical Details:** Some optical formulas are more naturally expressed using cosecant. Critical angle calculations sometimes use CSC form.

---

### [[Navigation and Surveying]]

**Scenario:** Calculate distances in triangulation where cosecant provides direct results.

**Implementation:**
```
Distance: =baseline * CSC(angle) * SIN(other_angle)
```

**Business Application:** Land surveying, marine navigation, geodesy. Triangle-based position determination.

**Technical Details:** Law of sines can be rearranged to use cosecant for direct calculation of unknown sides.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Domain:** All real numbers except multiples of pi
- **Error handling:** Returns #DIV/0! at singularities
- **Precision:** 15 significant digits

### Google Sheets
- **Availability:** All versions
- **Domain:** Same as Excel
- **Error handling:** Same #DIV/0! behavior
- **Precision:** 15 significant digits

### Both Platforms
- Input must be in radians
- Odd function: CSC(-x) = -CSC(x)
- Equivalent to 1/SIN(x)
- Same floating-point precision limitations

### For Older Excel Versions (pre-2013)
CSC function not available. Use:
```
=1/SIN(angle)
```

## Tips and Best Practices

1. **Always use radians:** CSC expects radians. Use RADIANS(degrees) to convert: `=CSC(RADIANS(30))` not `=CSC(30)`.

2. **Handle undefined points:** CSC is undefined at 0, pi, 2*pi, etc. Use IFERROR or check if MOD(angle, PI()) is near zero.

3. **Remember the range:** CSC never returns values between -1 and 1. If you get such a result, check your formula.

4. **It's an odd function:** CSC(-x) = -CSC(x). This symmetry can simplify formulas involving signed angles.

5. **Verify with reciprocal:** CSC(x) * SIN(x) should equal 1. Use this to verify calculations.

6. **For older Excel:** If CSC isn't available, use `=1/SIN(x)`. Mathematically equivalent.

7. **Near-singularity caution:** For angles near 0 or pi, cosecant values become extremely large. Consider whether your application needs special handling.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SIN]] | Returns the sine | Most common trig function; CSC = 1/SIN |
| [[CSCH]] | Returns the hyperbolic cosecant | For hyperbolic (not circular) cosecant |
| [[SEC]] | Returns the secant | Reciprocal of cosine, not sine |
| [[COT]] | Returns the cotangent | Reciprocal of tangent |

### Commonly Used Together

**[[SIN]]** - Sine (reciprocal relationship)

*Verify cosecant calculations:*
```
=CSC(A1) * SIN(A1)  // Should equal 1
```
Sine and cosecant are reciprocals.

---

**[[RADIANS]]** - Convert degrees to radians

*Use degree inputs:*
```
=CSC(RADIANS(45))  // Cosecant of 45 degrees
```
Essential for working with degree measurements.

---

**[[PI]]** - Mathematical constant pi

*Standard angles:*
```
=CSC(PI()/6)   // 30 degrees
=CSC(PI()/4)   // 45 degrees
=CSC(PI()/3)   // 60 degrees
=CSC(PI()/2)   // 90 degrees
```
Construct exact radian values for standard angles.

---

**[[IFERROR]]** - Handle undefined points

*Graceful error handling:*
```
=IFERROR(CSC(A1), "Undefined")
```
Catch division by zero at singularities.

---

**[[MOD]]** - Check for singularities

*Pre-validate angles:*
```
=IF(ABS(MOD(A1, PI()))<0.0001, "Undefined", CSC(A1))
```
Check if angle is near a multiple of pi before calculating.

## Official Documentation

- **Microsoft Excel:** [CSC function](https://support.microsoft.com/en-us/office/csc-function-07379361-219a-4398-8675-07ddc4f135c1)
- **Google Sheets:** [CSC function](https://support.google.com/docs/answer/9084178)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Added as part of new trig function set |
| Excel 2016+ | Maintained | No changes |
| Google Sheets | 2019 | Added to match Excel functionality |

---

*Last updated: 2026-01-10*
