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
- arctangent
- angles
- slope
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Arctangent
- Inverse Tangent
- Arc Tangent
tags:
- trigonometry
- inverse-trig
- arctangent
- angles
- slope
- radians
---

# ATAN

## Description

**ATAN** returns the arctangent (inverse tangent) of a number—the angle whose tangent equals that number. Since tangent = opposite/adjacent = rise/run = slope, ATAN is fundamentally the "slope to angle" converter. Given any slope value, ATAN tells you what angle would produce that slope. If TAN converts angles to slopes, ATAN converts slopes back to angles.

**ATAN returns radians, not degrees.** ATAN(1) returns approximately 0.7854 radians, not 45 degrees (even though tan(45 degrees) = 1). To get degrees, wrap with DEGREES(): `=DEGREES(ATAN(1))` returns 45. This is consistent with all inverse trig functions in Excel and Sheets.

**Unlike ASIN and ACOS, ATAN accepts any real number.** Since tangent can produce any value from negative infinity to positive infinity, ATAN can accept any input—there's no restriction like the -1 to 1 limit on ASIN/ACOS. ATAN(-1000) works fine (returning an angle very close to -90 degrees). This makes ATAN more forgiving to use than the other inverse trig functions.

**ATAN returns angles in the range -PI/2 to PI/2 (-90 to 90 degrees).** This covers only two quadrants: the right half of the coordinate plane. ATAN cannot distinguish between a slope of 1 in the first quadrant versus the third quadrant—both return 45 degrees. If you need to determine angles in all four quadrants based on x and y coordinates, use ATAN2 instead, which handles quadrant detection automatically.

## Syntax

> [!f(x)] ATAN Syntax
>
> ```
> =ATAN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The tangent value (slope) for which you want the angle. Can be any real number—positive, negative, or zero. There is no range restriction. This represents rise/run or opposite/adjacent. |

### Return Value

Returns the angle in **radians** whose tangent equals the input number. The result is always between -PI/2 and PI/2 (approximately -1.5708 to 1.5708), equivalent to -90 to 90 degrees. Use DEGREES() to convert the result to degrees. Never returns an error for numeric input (unlike ASIN/ACOS). Returns #VALUE! only if input is non-numeric.

## Examples

> [!f(x)] ATAN Examples

### Example 1: Arctangent of 0
```
=ATAN(0)
```
**Result:** 0

**Explanation:** What angle has a slope of 0? Zero radians (0 degrees)—a horizontal line. tan(0) = 0, so ATAN(0) = 0.

---

### Example 2: Arctangent of 1
```
=ATAN(1)
```
**Result:** 0.7853981634...

**Explanation:** This is PI/4 radians = 45 degrees. A slope of 1 means rise equals run—a 45-degree angle. This is the most common ATAN calculation.

---

### Example 3: Converting to Degrees
```
=DEGREES(ATAN(1))
```
**Result:** 45

**Explanation:** The essential pattern for readable output. A slope of 1 corresponds to 45 degrees. ATAN returns radians; DEGREES converts to degrees.

---

### Example 4: Steep Slope
```
=DEGREES(ATAN(10))
```
**Result:** 84.2894...

**Explanation:** A slope of 10 (rising 10 units for every 1 horizontal unit) is very steep—about 84 degrees. As slope approaches infinity, the angle approaches 90 degrees.

---

### Example 5: Very Steep Slope
```
=DEGREES(ATAN(1000))
```
**Result:** 89.9427...

**Explanation:** A slope of 1000 is almost vertical, giving an angle just shy of 90 degrees. ATAN approaches but never reaches 90 degrees.

---

### Example 6: Negative Slope
```
=DEGREES(ATAN(-1))
```
**Result:** -45

**Explanation:** A slope of -1 represents a line falling at 45 degrees. ATAN returns negative angles for negative slopes (downward slopes).

---

### Example 7: Small Slope (Road Grade)
```
=DEGREES(ATAN(0.05))
```
**Result:** 2.862...

**Explanation:** A 5% grade (0.05 slope) corresponds to about 2.86 degrees. Most road grades are small angles—a 10% grade is only about 5.7 degrees.

---

### Example 8: Verifying the Inverse Relationship
```
=TAN(ATAN(2.5))
```
**Result:** 2.5

**Explanation:** ATAN and TAN are inverses: TAN(ATAN(x)) = x for any x. This confirms the inverse relationship.

---

### Example 9: Rise Over Run Calculation
```
=DEGREES(ATAN(Rise / Run))
```
**Result:** Angle of the slope

**Explanation:** The fundamental use case. If Rise = 3 and Run = 4, ATAN(3/4) = ATAN(0.75) = approximately 36.87 degrees.

---

### Example 10: Calculating Roof Pitch Angle
```
=DEGREES(ATAN(6/12))
```
**Result:** 26.565...

**Explanation:** A 6:12 roof pitch (common in residential construction) rises 6 inches for every 12 inches horizontal, giving approximately 26.6 degrees.

---

### Example 11: Converting Percent Grade to Angle
```
=DEGREES(ATAN(PercentGrade / 100))
```
**Result:** Grade angle in degrees

**Explanation:** A percent grade is slope * 100. To get the angle, divide by 100 and take ATAN. A 15% grade = ATAN(0.15) = about 8.5 degrees.

---

### Example 12: Direction Angle from Velocity Components
```
=DEGREES(ATAN(Vy / Vx))
```
**Result:** Angle of motion (limited to -90 to 90 degrees)

**Explanation:** Given vertical velocity Vy and horizontal velocity Vx, ATAN gives the angle. BUT this only works correctly in quadrants I and IV (when Vx > 0). For full 360-degree angles, use ATAN2 instead.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number, not text. |
| `#DIV/0!` | Division by zero in the slope calculation | When Run = 0, the angle is 90 or -90 degrees. Use ATAN2(x, y) instead to handle this case. |
| `Result is decimal, expected degrees` | ATAN returns radians | Wrap with DEGREES(): `=DEGREES(ATAN(value))` |
| `#NAME?` | Misspelled function name | Check spelling: ATAN, not ARCTAN or ATANG. |
| `Wrong quadrant` | ATAN only returns -90 to 90 degrees | ATAN cannot determine quadrant from slope alone. Use ATAN2(x, y) for quadrant-aware angles. |
| `Angle is negative when expected positive` | Negative slope gives negative angle | Add 180 degrees if you know the angle should be in quadrant II or III. |

## Use Cases

### [[Slope and Grade Angle Conversion]]

**Scenario:** Convert between slope ratios (rise/run) and angles for construction, roads, and ramps.

**Implementation:**
```
=DEGREES(ATAN(Rise / Run))              → Angle from rise and run
=DEGREES(ATAN(PercentGrade / 100))      → Angle from percent grade
=DEGREES(ATAN(PitchRise / 12))          → Roof pitch to angle (assuming 12" run)
```

**Business Application:** Construction (roof pitch), civil engineering (road grades), ADA compliance (ramp angles), ski slope design, drainage system design.

**Technical Details:** Rise/run is the tangent of the angle by definition. ATAN reverses this to get the angle. Percent grade = slope * 100, so divide by 100 before ATAN.

---

### [[Projectile and Motion Angle]]

**Scenario:** Calculate the direction angle of motion from horizontal and vertical velocity components.

**Implementation:**
```
=DEGREES(ATAN(VerticalVelocity / HorizontalVelocity))
=DEGREES(ATAN(Vy / Vx))
=DEGREES(ATAN(DeltaY / DeltaX))
```

**Business Application:** Sports analytics (ball trajectory), ballistics, flight path analysis, animation motion paths.

**Technical Details:** Works correctly when horizontal velocity is positive (motion to the right). For full 360-degree motion direction, use ATAN2(Vx, Vy) instead. Remember: this gives angle from horizontal, not from vertical.

---

### [[Camera and Viewing Angle Calculations]]

**Scenario:** Determine viewing angles, camera tilt, and perspective calculations.

**Implementation:**
```
=DEGREES(ATAN(ObjectHeight / Distance))        → Elevation angle to top of object
=DEGREES(ATAN(ScreenHeight / ViewDistance))    → Vertical viewing angle
=2 * DEGREES(ATAN(SensorSize / (2 * FocalLength)))  → Field of view
```

**Business Application:** Photography focal length calculations, surveillance camera positioning, VR/AR display design, architectural sight-line analysis.

**Technical Details:** The tangent of the viewing angle equals the object size divided by distance. ATAN recovers the angle. For symmetric angles (like total FOV), double the half-angle result.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Output:** Radians only (use DEGREES() for conversion)
- **Input range:** All real numbers (no restriction)
- **Precision:** 15 significant digits
- **Output range:** -PI/2 to PI/2 (-90 to 90 degrees)

### Google Sheets
- **Availability:** All versions
- **Output:** Radians only (identical to Excel)
- **Input range:** Same—all real numbers
- **Precision:** 15 significant digits
- **Output range:** Same as Excel

### Both Platforms
- Both accept any numeric input (unlike ASIN/ACOS)
- Both return radians—use DEGREES() for degree output
- Both return principal values (-PI/2 to PI/2)
- No functional differences between platforms

## Tips and Best Practices

1. **ATAN is the slope-to-angle function:** If you have rise/run, ATAN gives the angle. This is its most intuitive use.

2. **Use ATAN2 for quadrant-aware angles:** ATAN alone cannot distinguish between quadrants I and III (or II and IV). If you have x and y coordinates, ATAN2 is almost always better.

3. **No input validation needed:** Unlike ASIN/ACOS, ATAN accepts any number. You don't need to check for valid range.

4. **Division by zero issue:** ATAN(Rise/0) causes #DIV/0!, not an angle of 90 degrees. Handle zero run with IF or use ATAN2.

5. **Percent grade to angle:** `=DEGREES(ATAN(Grade%/100))`. A 100% grade = 45 degrees.

6. **The 45-degree reference:** ATAN(1) = 45 degrees. Use this as a sanity check—if slope = 1, angle should be 45.

7. **Very large slopes approach 90 degrees:** As slope approaches infinity, ATAN approaches 90 degrees but never quite reaches it.

8. **Combine with trigonometry:** If you have angle from ATAN, you can then use SIN and COS with that angle for further calculations.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ATAN2]] | Returns angle from x and y coordinates | When you have separate x and y values and need quadrant-aware results—almost always prefer this |
| [[ASIN]] | Returns the arcsine of a number | When you have opposite/hypotenuse ratio |
| [[ACOS]] | Returns the arccosine of a number | When you have adjacent/hypotenuse ratio |
| [[TAN]] | Returns the tangent of an angle | The forward function—angle to slope |
| [[ATANH]] | Returns the inverse hyperbolic tangent | For hyperbolic inverse, not circular trigonometry |

### Commonly Used Together

**[[DEGREES]]** - Convert radians to degrees

*Essential for human-readable output:*
```
=DEGREES(ATAN(1))    → Returns 45
```
ATAN returns radians; DEGREES makes it understandable.

---

**[[TAN]]** - Forward tangent function

*Verify inverse relationship:*
```
=TAN(ATAN(2.5))    → Returns 2.5
```
TAN and ATAN are inverses of each other.

---

**[[ATAN2]]** - Quadrant-aware arctangent

*Full 360-degree angle calculation:*
```
=DEGREES(ATAN2(X, Y))    → Returns angle in proper quadrant
```
Use ATAN2 instead of ATAN when you have separate x and y coordinates.

---

**[[IF]]** - Handle vertical case

*Avoid division by zero:*
```
=IF(Run=0, 90*SIGN(Rise), DEGREES(ATAN(Rise/Run)))
```
When run is zero, the angle is 90 or -90 degrees.

---

**[[SQRT]]** - Calculate magnitude from components

*Complete vector analysis:*
```
Angle: =DEGREES(ATAN(Y/X))
Magnitude: =SQRT(X^2 + Y^2)
```
Angle and magnitude together fully describe a vector.

## Official Documentation

- **Microsoft Excel:** [ATAN function](https://support.microsoft.com/en-us/office/atan-function-50746fa8-630a-406b-81d0-4a2aed395543)
- **Google Sheets:** [ATAN function](https://support.google.com/docs/answer/3093468)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
