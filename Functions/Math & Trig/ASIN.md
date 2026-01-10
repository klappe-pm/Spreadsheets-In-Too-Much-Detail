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
- arcsine
- angles
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Arcsine
- Inverse Sine
- Arc Sine
tags:
- trigonometry
- inverse-trig
- arcsine
- angles
- radians
---

# ASIN

## Description

**ASIN** returns the arcsine (inverse sine) of a number—the angle whose sine equals that number. Given a value between -1 and 1, ASIN tells you what angle (in radians) would produce that sine value. If SIN answers "what's the sine of this angle?", ASIN answers the reverse: "what angle has this sine?" This makes ASIN essential for reverse-engineering angles from trigonometric ratios.

**Critical detail: ASIN returns radians, not degrees.** Just as SIN requires input in radians, ASIN outputs in radians. ASIN(0.5) returns approximately 0.5236 radians, not 30 degrees (even though sin(30 degrees) = 0.5). To get degrees, wrap with DEGREES(): `=DEGREES(ASIN(0.5))` returns 30. This radians output surprises many users—if your result looks like a small decimal when you expected something like "45", you forgot to convert to degrees.

The input to ASIN **must be between -1 and 1** (inclusive). This is because sine values can only range from -1 to 1. If you try ASIN(2) or ASIN(-1.5), you'll get a #NUM! error—there's no angle whose sine is 2 because that's mathematically impossible. Before calling ASIN, validate that your input is within this range, or use IFERROR to handle the error case gracefully.

**ASIN returns angles in the "principal value" range of -PI/2 to PI/2 (-90 to 90 degrees).** Mathematically, infinitely many angles have the same sine (sin(30) = sin(150) = 0.5), but ASIN always returns the one between -90 and 90 degrees. This is the right-side half of the unit circle. If you need angles in other quadrants, you'll have to calculate them: for example, if you know the angle should be obtuse, use PI - ASIN(value) to get the second-quadrant angle.

## Syntax

> [!f(x)] ASIN Syntax
>
> ```
> =ASIN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The sine value for which you want the angle. Must be between -1 and 1 (inclusive). Values outside this range return #NUM! error because no real angle has a sine outside this range. |

### Return Value

Returns the angle in **radians** whose sine equals the input number. The result is always between -PI/2 and PI/2 (approximately -1.5708 to 1.5708), equivalent to -90 to 90 degrees. Use DEGREES() to convert the result to degrees. Returns #NUM! if input is outside the range -1 to 1. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] ASIN Examples

### Example 1: Arcsine of 0
```
=ASIN(0)
```
**Result:** 0

**Explanation:** What angle has sine = 0? Zero radians (0 degrees). sin(0) = 0, so ASIN(0) = 0.

---

### Example 2: Arcsine of 0.5
```
=ASIN(0.5)
```
**Result:** 0.5235987756...

**Explanation:** This is PI/6 radians, which equals 30 degrees. sin(30 degrees) = 0.5. The result is in radians—approximately 0.524, not 30.

---

### Example 3: Converting to Degrees
```
=DEGREES(ASIN(0.5))
```
**Result:** 30

**Explanation:** The essential pattern for human-readable output. ASIN returns radians; DEGREES converts to the familiar degree measure. DEGREES(ASIN(0.5)) = 30 degrees.

---

### Example 4: Arcsine of 1
```
=ASIN(1)
```
**Result:** 1.5707963268...

**Explanation:** This is PI/2 radians = 90 degrees. sin(90 degrees) = 1 (the maximum sine value), so ASIN(1) returns 90 degrees in radians.

---

### Example 5: Arcsine of -1
```
=ASIN(-1)
```
**Result:** -1.5707963268...

**Explanation:** This is -PI/2 radians = -90 degrees. sin(-90 degrees) = -1 (the minimum sine value). ASIN ranges from -90 to +90 degrees.

---

### Example 6: Arcsine of sqrt(2)/2 (45 Degrees)
```
=ASIN(SQRT(2)/2)
```
**Result:** 0.7853981634...

**Explanation:** sqrt(2)/2 is approximately 0.707, the sine of 45 degrees. ASIN returns PI/4 radians. DEGREES(ASIN(SQRT(2)/2)) = 45.

---

### Example 7: Error - Input Out of Range
```
=ASIN(1.5)
```
**Result:** #NUM!

**Explanation:** No angle has a sine of 1.5—sine values are bounded between -1 and 1. ASIN returns #NUM! for any input outside this range. This is a mathematical impossibility, not a software bug.

---

### Example 8: Negative Input
```
=DEGREES(ASIN(-0.5))
```
**Result:** -30

**Explanation:** sin(-30 degrees) = -0.5. ASIN returns negative angles for negative inputs (in the fourth quadrant, from the perspective of degrees).

---

### Example 9: Verifying the Inverse Relationship
```
=SIN(ASIN(0.7))
```
**Result:** 0.7

**Explanation:** ASIN and SIN are inverses: SIN(ASIN(x)) = x for any x between -1 and 1. This is a useful verification that your formulas are working correctly.

---

### Example 10: Finding an Angle in a Right Triangle
```
=DEGREES(ASIN(Opposite / Hypotenuse))
```
**Result:** The angle opposite to the known side

**Explanation:** In a right triangle, sin(angle) = opposite/hypotenuse. If you know the side lengths, ASIN gives you the angle. For example, if opposite = 3 and hypotenuse = 5, ASIN(3/5) = ASIN(0.6) = approximately 36.87 degrees.

---

### Example 11: Edge Case - Nearly 1
```
=DEGREES(ASIN(0.99999))
```
**Result:** 89.7437...

**Explanation:** As the input approaches 1, the angle approaches 90 degrees. The function handles values very close to the boundaries correctly.

---

### Example 12: Safe Handling with IFERROR
```
=IFERROR(DEGREES(ASIN(A1)), "Invalid sine value")
```
**Result:** Angle in degrees, or error message if A1 is outside -1 to 1

**Explanation:** Wrap ASIN in IFERROR when input values might be invalid. This prevents #NUM! errors from propagating through your spreadsheet.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input outside range -1 to 1 | Ensure input is a valid sine value (between -1 and 1). Use MIN(1, MAX(-1, value)) to clamp if needed. |
| `#VALUE!` | Non-numeric input | Ensure the argument is a number, not text. |
| `Result is decimal, expected degrees` | ASIN returns radians | Wrap with DEGREES(): `=DEGREES(ASIN(value))` |
| `#NAME?` | Misspelled function name | Check spelling: ASIN, not ARCSIN or ASINE. |
| `Unexpected angle` | Only returns -90 to 90 degree range | ASIN returns principal values only. For obtuse angles, use PI - ASIN(value). |
| `Result seems wrong` | Input calculation error | Verify your opposite/hypotenuse ratio is correct. Common mistake: using adjacent instead of opposite. |

## Use Cases

### [[Right Triangle Angle Calculations]]

**Scenario:** Find angles in a right triangle when you know the opposite side and hypotenuse.

**Implementation:**
```
=DEGREES(ASIN(Opposite / Hypotenuse))
=DEGREES(ASIN(Height / SlopeLength))
=DEGREES(ASIN(Rise / RampLength))
```

**Business Application:** Construction angle calculations, surveying, structural engineering, determining slope angles when you have vertical rise and total distance.

**Technical Details:** sin(angle) = opposite/hypotenuse. Make sure you're using the correct sides. If you have adjacent and hypotenuse, use ACOS instead. If you have opposite and adjacent, use ATAN.

---

### [[Physics: Launch Angle and Trajectory]]

**Scenario:** Determine the angle required for projectile motion or the angle of incidence/reflection.

**Implementation:**
```
=DEGREES(ASIN(VerticalVelocity / TotalVelocity))
=DEGREES(ASIN(WaveHeight / WaveLength * 2 * PI()))
=ASIN(n1 * SIN(RADIANS(Angle1)) / n2)    → Snell's law
```

**Business Application:** Ballistics calculations, optics and lens design, satellite trajectory planning, sports analytics (optimal throw angles).

**Technical Details:** In physics, many phenomena involve known ratios that need conversion to angles. The vertical component of velocity divided by total velocity gives the sine of the launch angle. Snell's law for refraction uses arcsine to find the refracted angle.

---

### [[Audio and Signal Processing]]

**Scenario:** Convert normalized amplitude values back to phase angles in signal analysis.

**Implementation:**
```
=ASIN(NormalizedAmplitude)
=DEGREES(ASIN(SampleValue / MaxAmplitude))
=ASIN(2 * (Value - 0.5))    → Convert 0-1 range to angle
```

**Business Application:** Audio synthesis, waveform reconstruction, signal phase analysis, animation easing functions.

**Technical Details:** When working with signals represented as normalized values (-1 to 1), ASIN can recover the phase angle. This is used in some audio processing algorithms and for creating smooth animation curves (easeInSine, easeOutSine patterns).

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Output:** Radians only (use DEGREES() for conversion)
- **Range:** Returns #NUM! for inputs outside -1 to 1
- **Precision:** 15 significant digits
- **Edge values:** ASIN(1) and ASIN(-1) return exactly PI/2 and -PI/2

### Google Sheets
- **Availability:** All versions
- **Output:** Radians only (identical to Excel)
- **Range:** Same #NUM! behavior for out-of-range inputs
- **Precision:** 15 significant digits
- **Edge values:** Same behavior as Excel

### Both Platforms
- Both return radians—always use DEGREES() for degree output
- Both handle the range -1 to 1 identically
- Both return principal values (-PI/2 to PI/2)
- No functional differences between platforms

## Tips and Best Practices

1. **Always wrap with DEGREES() for readable output:** `=DEGREES(ASIN(value))` is the standard pattern. Raw ASIN output in radians is rarely useful for presentation.

2. **Validate input range:** Before calling ASIN, ensure values are between -1 and 1. Use `=IF(ABS(A1)<=1, DEGREES(ASIN(A1)), "Error")` for safe calculations.

3. **Know the opposite-hypotenuse relationship:** ASIN answers: "given opposite/hypotenuse, what's the angle?" For other side combinations, use ACOS or ATAN.

4. **Remember the restricted output range:** ASIN only returns -90 to 90 degrees. For angles in other quadrants, calculate manually: second quadrant = 180 - angle, etc.

5. **Handle floating-point edge cases:** Due to rounding, calculations might produce values like 1.0000000001. Clamp with MIN(1, MAX(-1, value)) before calling ASIN.

6. **Verify with SIN:** To check your work, SIN(ASIN(x)) should equal x. If it doesn't, investigate your formula.

7. **Use ASIN for "height to angle" problems:** Whenever you have a height and total distance (like ramp rise and ramp length), ASIN gives the angle.

8. **Pair with IFERROR for robustness:** `=IFERROR(DEGREES(ASIN(A1)), "N/A")` prevents errors from cascading through your spreadsheet.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ACOS]] | Returns the arccosine of a number | When you have adjacent/hypotenuse ratio instead of opposite/hypotenuse |
| [[ATAN]] | Returns the arctangent of a number | When you have opposite/adjacent ratio (slope) |
| [[ATAN2]] | Returns angle from x and y coordinates | When you have x,y coordinates and need quadrant-aware angle |
| [[SIN]] | Returns the sine of an angle | The forward function—use when you have the angle and want the ratio |
| [[ASINH]] | Returns the inverse hyperbolic sine | For hyperbolic inverse, not circular trigonometry |

### Commonly Used Together

**[[DEGREES]]** - Convert radians to degrees

*Essential for human-readable output:*
```
=DEGREES(ASIN(0.5))    → Returns 30
```
ASIN returns radians; DEGREES makes it understandable.

---

**[[SIN]]** - Forward sine function

*Verify inverse relationship:*
```
=SIN(ASIN(0.7))    → Returns 0.7
```
SIN and ASIN are inverses of each other.

---

**[[IFERROR]]** - Handle invalid inputs

*Graceful error handling:*
```
=IFERROR(DEGREES(ASIN(A1)), "Out of range")
```
Prevents #NUM! errors when input might exceed -1 to 1 range.

---

**[[MIN]] and [[MAX]]** - Clamp values to valid range

*Prevent out-of-range errors:*
```
=ASIN(MIN(1, MAX(-1, A1)))
```
Forces input into valid range, useful when floating-point arithmetic might produce values like 1.0000001.

---

**[[SQRT]]** - Calculate hypotenuse or other sides

*Complete triangle calculations:*
```
=DEGREES(ASIN(Height / SQRT(Height^2 + Base^2)))
```
Combine with Pythagorean theorem for full triangle solutions.

## Official Documentation

- **Microsoft Excel:** [ASIN function](https://support.microsoft.com/en-us/office/asin-function-81fb95e5-6d6f-48c4-bc45-58f955c6d347)
- **Google Sheets:** [ASIN function](https://support.google.com/docs/answer/3093464)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
