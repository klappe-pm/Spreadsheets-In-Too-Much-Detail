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
- tangent
- radians
- angles
- slope
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Tangent Function
- Tan
- Trigonometric Tangent
tags:
- trigonometry
- tangent
- radians
- angles
- slope
- rise-over-run
---

# TAN

## Description

**TAN** returns the tangent of an angle. Like SIN and COS, the input must be in **radians, not degrees**—forgetting this conversion is the most common error. Tangent is defined as sine divided by cosine: tan(x) = sin(x)/cos(x). In a right triangle, tangent represents the ratio of the opposite side to the adjacent side—geometrically, this is the "rise over run" or slope of the line at that angle.

Unlike sine and cosine, **tangent is unbounded**—it can return any real number from negative infinity to positive infinity. At 0 degrees, tan = 0. As the angle approaches 90 degrees, tangent approaches infinity. At exactly 90 degrees (PI/2 radians), tangent is **undefined** because you're dividing by cos(90) = 0. The function then continues from negative infinity just past 90 degrees, rising back through 0 at 180 degrees. This creates vertical asymptotes every 180 degrees (PI radians), which is critical to understand when using TAN.

**TAN is the "slope function" of trigonometry.** If you draw a line from the origin at angle theta to the positive x-axis, the tangent of theta equals the slope of that line. This makes TAN invaluable for converting between angles and slopes in engineering, surveying, and graphics. A 45-degree angle has a slope of 1 (tan(45) = 1, meaning rise equals run). Steeper angles have larger tangents; a 60-degree angle has tan = 1.732, while an 89-degree angle has tan = 57.3.

**Key values and behaviors:** TAN(0) = 0, TAN(PI()/4) = 1 (45 degrees), TAN(PI()/3) = 1.732... (60 degrees, equal to square root of 3). TAN is periodic with period PI (180 degrees), not 2*PI like sine and cosine. TAN is an odd function: tan(-x) = -tan(x). Be very careful near 90 and 270 degrees—while Excel won't crash, you'll get extremely large numbers that may cause overflow in subsequent calculations.

## Syntax

> [!f(x)] TAN Syntax
>
> ```
> =TAN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The angle in **radians** for which you want the tangent. Can be any real number except odd multiples of PI/2 (90 degrees, 270 degrees, etc.) where tangent is undefined. For angles in degrees, use RADIANS(degrees) or multiply by PI()/180. |

### Return Value

Returns a numeric value that can be any real number. Unlike SIN and COS (bounded between -1 and 1), TAN has no bounds—it can return very large positive or negative numbers. Near odd multiples of 90 degrees, returns extremely large values. If input is non-numeric, returns #VALUE! error. Excel handles the mathematical undefined case (at exactly PI/2) by returning a very large number due to floating-point precision.

## Examples

> [!f(x)] TAN Examples

### Example 1: Tangent of Zero
```
=TAN(0)
```
**Result:** 0

**Explanation:** tan(0) = sin(0)/cos(0) = 0/1 = 0. At 0 degrees, the line is horizontal with zero slope.

---

### Example 2: Tangent of 45 Degrees
```
=TAN(RADIANS(45))
```
**Result:** 1

**Explanation:** tan(45 degrees) = 1 exactly. This is the angle where rise equals run—a slope of 1. At 45 degrees, sine and cosine are equal, so their ratio is 1.

---

### Example 3: Common Mistake - Forgetting Radians
```
=TAN(45)
```
**Result:** 1.6197751905...

**Explanation:** This is NOT tan(45 degrees), which equals 1. This is tan(45 radians)—a meaningless value for most practical purposes. **Always use =TAN(RADIANS(45))** when working with degrees.

---

### Example 4: Tangent of 60 Degrees
```
=TAN(RADIANS(60))
```
**Result:** 1.7320508076...

**Explanation:** tan(60 degrees) = square root of 3, approximately 1.732. This is one of the special angles from trigonometry. A 60-degree slope is quite steep—rising 1.732 units for every 1 unit of horizontal travel.

---

### Example 5: Approaching the Asymptote (89 Degrees)
```
=TAN(RADIANS(89))
```
**Result:** 57.28996163...

**Explanation:** As angles approach 90 degrees, tangent grows very large. At 89 degrees, the slope is about 57—meaning a rise of 57 for every 1 unit horizontal. One more degree and it goes to infinity.

---

### Example 6: Near-Vertical (89.9 Degrees)
```
=TAN(RADIANS(89.9))
```
**Result:** 572.9572134...

**Explanation:** At 89.9 degrees, tangent exceeds 572. The closer to 90 degrees, the larger the tangent. This illustrates why very steep angles require careful handling.

---

### Example 7: The Undefined Case (90 Degrees)
```
=TAN(RADIANS(90))
```
**Result:** 1.63312393531954E+16 (a very large number)

**Explanation:** Mathematically, tan(90 degrees) is undefined (division by zero). Due to floating-point precision, RADIANS(90) isn't exactly PI/2, so Excel returns an astronomically large number instead of an error. Don't rely on this behavior—treat 90-degree tangent as undefined.

---

### Example 8: Negative Angles
```
=TAN(RADIANS(-45))
```
**Result:** -1

**Explanation:** Tangent is an odd function: tan(-x) = -tan(x). The tangent of -45 degrees is -1, representing a downward slope (falling 1 unit for every 1 unit right).

---

### Example 9: Beyond 90 Degrees
```
=TAN(RADIANS(120))
```
**Result:** -1.7320508076...

**Explanation:** tan(120 degrees) = -sqrt(3), approximately -1.732. In the second quadrant (90-180 degrees), tangent is negative. 120 degrees is 30 degrees past vertical, giving the same magnitude as 60 degrees but negative.

---

### Example 10: Using PI Directly
```
=TAN(PI()/4)
```
**Result:** 1

**Explanation:** PI/4 radians = 45 degrees. Using PI() directly is mathematically cleaner and avoids the degree conversion step. TAN(PI()/4) = 1 exactly.

---

### Example 11: Calculating Slope from Grade Angle
```
=TAN(RADIANS(A1))
```
**Result:** Slope (rise over run) for grade angle in A1

**Explanation:** If A1 contains a road grade angle of 5 degrees, this returns 0.0875, meaning 0.0875 feet of rise per foot of horizontal run (or 8.75% grade).

---

### Example 12: Periodicity (180 Degrees Apart)
```
=TAN(RADIANS(30))
=TAN(RADIANS(210))
```
**Result:** Both return 0.5773502692...

**Explanation:** Tangent has period 180 degrees (PI radians), not 360 degrees like sine and cosine. tan(30) = tan(210) = tan(390), etc.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text values cause this error. |
| `Unexpected result` | Forgot radians conversion | Use RADIANS(degrees) or multiply by PI()/180. |
| `Very large number` | Angle near 90 or 270 degrees | Tangent approaches infinity at these angles. This is mathematically correct but may cause overflow in subsequent calculations. |
| `#NUM!` | Result too large for Excel | At angles extremely close to 90 degrees, the result may exceed Excel's number range. Avoid angles within ~1E-15 radians of PI/2. |
| `#NAME?` | Misspelled function name | Check spelling: TAN, not TANG or TANGENT. |
| `Wrong sign` | Quadrant confusion | Tangent is positive in quadrants I and III (0-90, 180-270 degrees), negative in II and IV. |

## Use Cases

### [[Slope and Grade Calculations]]

**Scenario:** Convert between angles and slopes for roads, roofs, ramps, or any inclined surface.

**Implementation:**
```
=TAN(RADIANS(GradeAngle))                    → Slope from angle
=TAN(RADIANS(A1)) * 100                      → Percent grade
=Rise / TAN(RADIANS(Angle))                  → Horizontal run from rise and angle
```

**Business Application:** Civil engineering road design, architecture roof pitch, ADA ramp compliance (max 8.33% grade = 4.76 degrees), ski slope classification.

**Technical Details:** Grade percentages are slope * 100. A 45-degree angle = 100% grade. Most road grades are under 10% (5.7 degrees). The tangent function directly converts between angular measurement and rise-over-run ratios.

---

### [[Height and Distance Calculations]]

**Scenario:** Calculate heights of objects using angle measurements (surveying, navigation).

**Implementation:**
```
=Distance * TAN(RADIANS(ElevationAngle))     → Height of object
=Height / TAN(RADIANS(Angle))                → Distance to object
=TAN(RADIANS(A1)) * BaseDistance + EyeHeight → Total height including observer
```

**Business Application:** Surveying and land measurement, determining building heights, artillery and ballistics, astronomy telescope pointing.

**Technical Details:** This is classic trigonometric surveying. Measure the angle to the top of an object and your distance from it, then height = distance * tan(angle). Remember to add the height of the measuring instrument.

---

### [[Computer Graphics and Game Development]]

**Scenario:** Calculate field of view, perspective projections, and camera angles.

**Implementation:**
```
=2 * TAN(RADIANS(FOV/2)) * Distance          → View width at distance
=Height / (2 * TAN(RADIANS(VerticalFOV/2)))  → Distance for desired height in frame
=DEGREES(ATAN(ViewWidth / (2*Distance))) * 2 → FOV from view width
```

**Business Application:** 3D rendering systems, game camera setup, VR/AR headset calibration, photography focal length calculations.

**Technical Details:** Field of view (FOV) determines how much of a scene is visible. The tangent of half the FOV angle, multiplied by distance, gives half the visible width. This is fundamental to perspective projection in computer graphics.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Input:** Radians only
- **Undefined handling:** Returns very large number at exact PI/2 (floating-point prevents true undefined)
- **Precision:** 15 significant digits
- **Overflow:** Returns #NUM! if result exceeds approximately 1.8E+308

### Google Sheets
- **Availability:** All versions
- **Input:** Radians only (same as Excel)
- **Undefined handling:** Same behavior as Excel
- **Precision:** 15 significant digits
- **Overflow:** Similar handling to Excel

### Both Platforms
- Neither returns an error at exactly 90 degrees due to floating-point imprecision
- Both have the same period (PI radians, not 2*PI)
- Both handle negative angles identically (odd function)
- Large results may display in scientific notation

## Tips and Best Practices

1. **Watch for asymptotes:** TAN is undefined at 90, 270, 450, ... degrees. Check that your input angles never hit these values, or add error handling for large results.

2. **Use for slope conversions:** TAN directly converts angles to slopes (rise/run). This is its most practical application—slope = TAN(angle), angle = ATAN(slope).

3. **Remember the smaller period:** TAN repeats every 180 degrees, not 360 like SIN and COS. TAN(x) = TAN(x + 180 degrees).

4. **TAN is odd:** TAN(-x) = -TAN(x). Negative angles give negative tangents, representing downward slopes.

5. **Key values for verification:** TAN(0)=0, TAN(45 deg)=1, TAN(60 deg)=sqrt(3). If these don't work, check your radian conversion.

6. **Avoid very steep angles:** Near 90 degrees, tiny angle changes cause huge tangent changes. Consider using ATAN2 to work with coordinates instead.

7. **For inverse operations use ATAN:** To find an angle from a slope (tangent value), use ATAN. For quadrant-aware angle finding from coordinates, use ATAN2.

8. **Handle #NUM! errors:** For robust code, wrap TAN in IFERROR when input angles might approach 90 degrees.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SIN]] | Returns the sine of an angle | When you need the opposite/hypotenuse ratio or y-coordinate |
| [[COS]] | Returns the cosine of an angle | When you need the adjacent/hypotenuse ratio or x-coordinate |
| [[ATAN]] | Returns the arctangent (inverse tangent) | When you have a slope and need to find the angle |
| [[ATAN2]] | Returns angle from x and y coordinates | When you have coordinates and need angle (handles all quadrants) |
| [[TANH]] | Returns the hyperbolic tangent | For hyperbolic applications, machine learning activation functions |

### Commonly Used Together

**[[RADIANS]]** - Convert degrees to radians

*Essential for angle input:*
```
=TAN(RADIANS(30))
```
Standard pattern for working with degree measurements.

---

**[[ATAN]]** - Inverse tangent

*Convert slope back to angle:*
```
Slope: =TAN(RADIANS(45))         → 1
Angle: =DEGREES(ATAN(1))         → 45
```
These are inverse operations: ATAN(TAN(x)) = x.

---

**[[SIN]] and [[COS]]** - Related trig functions

*Tangent definition:*
```
=SIN(A1) / COS(A1)    → Equals TAN(A1)
```
Tangent is the ratio of sine to cosine.

---

**[[DEGREES]]** - Convert radians to degrees

*Display angles in familiar units:*
```
=DEGREES(ATAN(slope))
```
After ATAN returns radians, convert to degrees for readability.

---

**[[IFERROR]]** - Handle near-asymptote cases

*Protect against very large results:*
```
=IFERROR(TAN(RADIANS(A1)), "Undefined")
```
Gracefully handle angles near 90 degrees.

## Official Documentation

- **Microsoft Excel:** [TAN function](https://support.microsoft.com/en-us/office/tan-function-08851a40-179f-4052-b789-d7f699447401)
- **Google Sheets:** [TAN function](https://support.google.com/docs/answer/3093586)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
