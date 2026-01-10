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
- inverse-trig
- arccosine
- angles
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Arccosine
- Inverse Cosine
- Arc Cosine
tags:
- trigonometry
- inverse-trig
- arccosine
- angles
- radians
---

# ACOS

## Description

**ACOS** returns the arccosine (inverse cosine) of a number—the angle whose cosine equals that number. Given a value between -1 and 1, ACOS tells you what angle (in radians) would produce that cosine value. If COS answers "what's the cosine of this angle?", ACOS answers the reverse: "what angle has this cosine?" This is essential for finding angles when you know the adjacent side and hypotenuse of a right triangle.

**ACOS returns radians, not degrees.** ACOS(0.5) returns approximately 1.047 radians, not 60 degrees (even though cos(60 degrees) = 0.5). To convert to degrees, wrap with DEGREES(): `=DEGREES(ACOS(0.5))` returns 60. This is consistent with all inverse trig functions in Excel and Sheets—they always return radians.

The input to ACOS **must be between -1 and 1** (inclusive), just like ASIN. Cosine values are bounded to this range, so any input outside it produces a #NUM! error. There's no angle whose cosine is 2 or -1.5. Always validate input or wrap in IFERROR when the input might be out of range.

**ACOS returns angles in the "principal value" range of 0 to PI (0 to 180 degrees).** This is different from ASIN, which returns -90 to 90 degrees. ACOS gives angles in the upper half of the unit circle. ACOS(1) = 0 degrees, ACOS(0) = 90 degrees, ACOS(-1) = 180 degrees. Note that ACOS never returns negative angles—if you need angles in the third or fourth quadrant, you'll need to calculate them manually (typically 360 degrees minus the ACOS result).

## Syntax

> [!f(x)] ACOS Syntax
>
> ```
> =ACOS(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The cosine value for which you want the angle. Must be between -1 and 1 (inclusive). Values outside this range return #NUM! error because no real angle has a cosine outside this range. |

### Return Value

Returns the angle in **radians** whose cosine equals the input number. The result is always between 0 and PI (approximately 0 to 3.1416), equivalent to 0 to 180 degrees. Use DEGREES() to convert the result to degrees. Returns #NUM! if input is outside the range -1 to 1. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] ACOS Examples

### Example 1: Arccosine of 1
```
=ACOS(1)
```
**Result:** 0

**Explanation:** What angle has cosine = 1? Zero radians (0 degrees). cos(0) = 1, so ACOS(1) = 0. This is the starting point of the cosine curve.

---

### Example 2: Arccosine of 0.5
```
=ACOS(0.5)
```
**Result:** 1.0471975512...

**Explanation:** This is PI/3 radians, which equals 60 degrees. cos(60 degrees) = 0.5. The result is in radians—approximately 1.047, not 60.

---

### Example 3: Converting to Degrees
```
=DEGREES(ACOS(0.5))
```
**Result:** 60

**Explanation:** The essential pattern for readable output. ACOS returns radians; DEGREES converts to degrees. cos(60 degrees) = 0.5, so ACOS(0.5) = 60 degrees.

---

### Example 4: Arccosine of 0
```
=ACOS(0)
```
**Result:** 1.5707963268...

**Explanation:** This is PI/2 radians = 90 degrees. cos(90 degrees) = 0. ACOS(0) returns 90 degrees (in radians).

---

### Example 5: Arccosine of -1
```
=ACOS(-1)
```
**Result:** 3.1415926536...

**Explanation:** This is PI radians = 180 degrees. cos(180 degrees) = -1 (the minimum cosine value). ACOS ranges from 0 to 180 degrees, and -1 gives the maximum angle.

---

### Example 6: Arccosine of sqrt(2)/2 (45 Degrees)
```
=DEGREES(ACOS(SQRT(2)/2))
```
**Result:** 45

**Explanation:** sqrt(2)/2 is approximately 0.707, the cosine of 45 degrees. At this angle, sine and cosine are equal.

---

### Example 7: Error - Input Out of Range
```
=ACOS(1.5)
```
**Result:** #NUM!

**Explanation:** No angle has a cosine of 1.5. Cosine values range from -1 to 1, so inputs outside this range are mathematically impossible.

---

### Example 8: Negative Input
```
=DEGREES(ACOS(-0.5))
```
**Result:** 120

**Explanation:** cos(120 degrees) = -0.5. Unlike ASIN (which returns negative angles for negative inputs), ACOS returns angles between 90 and 180 degrees for negative inputs.

---

### Example 9: Verifying the Inverse Relationship
```
=COS(ACOS(0.7))
```
**Result:** 0.7

**Explanation:** ACOS and COS are inverses: COS(ACOS(x)) = x for any x between -1 and 1. This confirms the inverse relationship.

---

### Example 10: Finding Angle in a Right Triangle
```
=DEGREES(ACOS(Adjacent / Hypotenuse))
```
**Result:** The angle adjacent to the known side

**Explanation:** In a right triangle, cos(angle) = adjacent/hypotenuse. If adjacent = 4 and hypotenuse = 5, ACOS(4/5) = ACOS(0.8) = approximately 36.87 degrees.

---

### Example 11: Angle Between Two Vectors (Dot Product)
```
=DEGREES(ACOS(DotProduct / (Magnitude1 * Magnitude2)))
```
**Result:** The angle between two vectors

**Explanation:** The dot product formula: A.B = |A||B|cos(theta). Rearranging: theta = ACOS(A.B / (|A||B|)). This is fundamental in physics and 3D graphics.

---

### Example 12: Safe Handling with IFERROR
```
=IFERROR(DEGREES(ACOS(A1)), "Invalid cosine value")
```
**Result:** Angle in degrees, or error message if A1 is outside -1 to 1

**Explanation:** Protects against #NUM! errors when input might be invalid.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input outside range -1 to 1 | Ensure input is a valid cosine value. Use MIN(1, MAX(-1, value)) to clamp. |
| `#VALUE!` | Non-numeric input | Ensure the argument is a number, not text. |
| `Result is decimal, expected degrees` | ACOS returns radians | Wrap with DEGREES(): `=DEGREES(ACOS(value))` |
| `#NAME?` | Misspelled function name | Check spelling: ACOS, not ARCCOS or ACOSINE. |
| `Expected negative angle` | ACOS only returns 0-180 degrees | ACOS never returns negative angles. For fourth quadrant, use -ACOS(value) or 360-DEGREES(ACOS(value)). |
| `Wrong triangle side used` | Used opposite instead of adjacent | ACOS uses adjacent/hypotenuse. For opposite/hypotenuse, use ASIN. |

## Use Cases

### [[Right Triangle Calculations with Adjacent Side]]

**Scenario:** Find angles in a right triangle when you know the adjacent side and hypotenuse.

**Implementation:**
```
=DEGREES(ACOS(Adjacent / Hypotenuse))
=DEGREES(ACOS(HorizontalDistance / SlopeLength))
=DEGREES(ACOS(BaseLength / CableLength))
```

**Business Application:** Engineering angle calculations, cable tension analysis, structural design, determining viewing angles when you know horizontal distance and line-of-sight distance.

**Technical Details:** cos(angle) = adjacent/hypotenuse. Use ACOS when you know the side "adjacent" to the angle (not the opposite side). If you have the opposite side, use ASIN instead.

---

### [[Vector Angle Calculations]]

**Scenario:** Calculate the angle between two vectors using the dot product formula.

**Implementation:**
```
=DEGREES(ACOS((A1*B1 + A2*B2) / (SQRT(A1^2+A2^2) * SQRT(B1^2+B2^2))))
=ACOS(SUMPRODUCT(Vector1, Vector2) / (SQRT(SUMSQ(Vector1)) * SQRT(SUMSQ(Vector2))))
```

**Business Application:** 3D graphics and game development, physics simulations, machine learning (cosine similarity), portfolio diversification analysis, market correlation studies.

**Technical Details:** The angle between vectors A and B is arccos(A.B / |A||B|). This is one of the most important applications of ACOS in technical computing. Note that this always returns angles between 0 and 180 degrees.

---

### [[Navigation and Bearing Calculations]]

**Scenario:** Calculate angles and bearings for navigation purposes.

**Implementation:**
```
=DEGREES(ACOS(SIN(lat1)*SIN(lat2) + COS(lat1)*COS(lat2)*COS(lon2-lon1)))
=ACOS(DotProduct / Distance)  → Direction angle
```

**Business Application:** GPS navigation, shipping route optimization, aviation flight planning, satellite positioning.

**Technical Details:** The great-circle distance formula and bearing calculations use ACOS to convert from cosine values back to angles. Be careful with units—latitude and longitude must be in radians for trig functions.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Output:** Radians only (use DEGREES() for conversion)
- **Range:** Returns #NUM! for inputs outside -1 to 1
- **Precision:** 15 significant digits
- **Output range:** 0 to PI (0 to 180 degrees)

### Google Sheets
- **Availability:** All versions
- **Output:** Radians only (identical to Excel)
- **Range:** Same #NUM! behavior for out-of-range inputs
- **Precision:** 15 significant digits
- **Output range:** Same as Excel

### Both Platforms
- Both return radians—always use DEGREES() for degree output
- Both return principal values (0 to PI)
- Both have identical handling of edge cases
- No functional differences between platforms

## Tips and Best Practices

1. **Remember ACOS vs ASIN output ranges:** ACOS returns 0 to 180 degrees (never negative). ASIN returns -90 to 90 degrees. Choose based on which range fits your problem.

2. **Use for adjacent/hypotenuse problems:** ACOS answers: "given adjacent/hypotenuse, what's the angle?" For opposite/hypotenuse, use ASIN. For opposite/adjacent, use ATAN.

3. **Dot product angle formula:** theta = ACOS(A.B / |A||B|) is fundamental. This is how you find the angle between any two vectors.

4. **Handle floating-point edge cases:** Calculations might produce 1.0000000001 due to rounding. Use MIN(1, MAX(-1, value)) to clamp before calling ACOS.

5. **ACOS(0) = 90 degrees:** This is a quick reference point. If something has zero x-component (cosine), it's pointing straight up (90 degrees).

6. **Convert to fourth quadrant if needed:** For angles between 180 and 360 degrees, use 360 - DEGREES(ACOS(value)) with appropriate logic.

7. **Cosine similarity uses ACOS:** In data science, cosine similarity = A.B/(|A||B|). To get the actual angle, apply ACOS.

8. **Verify with COS:** COS(ACOS(x)) should equal x. Use this to check your formulas.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ASIN]] | Returns the arcsine of a number | When you have opposite/hypotenuse ratio |
| [[ATAN]] | Returns the arctangent of a number | When you have opposite/adjacent ratio (slope) |
| [[ATAN2]] | Returns angle from x and y coordinates | When you have coordinates and need full 360-degree range |
| [[COS]] | Returns the cosine of an angle | The forward function—angle to ratio |
| [[ACOSH]] | Returns the inverse hyperbolic cosine | For hyperbolic inverse, not circular trigonometry |

### Commonly Used Together

**[[DEGREES]]** - Convert radians to degrees

*Essential for human-readable output:*
```
=DEGREES(ACOS(0.5))    → Returns 60
```
ACOS returns radians; DEGREES makes it understandable.

---

**[[COS]]** - Forward cosine function

*Verify inverse relationship:*
```
=COS(ACOS(0.7))    → Returns 0.7
```
COS and ACOS are inverses of each other.

---

**[[SQRT]]** - For magnitude calculations

*Calculate vector magnitudes:*
```
=ACOS(DotProduct / (SQRT(SUMSQ(A)) * SQRT(SUMSQ(B))))
```
Vector angle calculation requires magnitudes via square root.

---

**[[SUMPRODUCT]]** - Calculate dot products

*Dot product for vector angles:*
```
=ACOS(SUMPRODUCT(Vector1, Vector2) / ...)
```
SUMPRODUCT computes the numerator of the angle formula.

---

**[[IFERROR]]** - Handle out-of-range inputs

*Graceful error handling:*
```
=IFERROR(DEGREES(ACOS(A1)), "Invalid")
```
Catches #NUM! errors from values outside -1 to 1.

## Official Documentation

- **Microsoft Excel:** [ACOS function](https://support.microsoft.com/en-us/office/acos-function-cb73173f-d089-4582-afe5-7965ba77f665)
- **Google Sheets:** [ACOS function](https://support.google.com/docs/answer/3093461)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
