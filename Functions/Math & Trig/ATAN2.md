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
- coordinates
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Two-Argument Arctangent
- Quadrant-Aware Arctangent
- Atan2
tags:
- trigonometry
- inverse-trig
- arctangent
- angles
- coordinates
- quadrant
---

# ATAN2

## Description

**ATAN2** returns the arctangent of y/x coordinates—the angle from the positive x-axis to the point (x, y). Unlike regular ATAN, which only takes a single slope value, ATAN2 takes x and y as separate arguments and **automatically determines the correct quadrant**. This makes ATAN2 the go-to function for converting Cartesian coordinates (x, y) to polar coordinates (angle, radius), and it correctly handles all four quadrants of the coordinate plane.

**Critical syntax difference: Excel uses ATAN2(x, y) while most programming languages use atan2(y, x).** This is one of Excel's most confusing design decisions. In Excel and Google Sheets, the x-coordinate comes first: `=ATAN2(x_num, y_num)`. In almost every programming language (Python, JavaScript, C, etc.), it's `atan2(y, x)` with y first. Double-check your argument order when translating formulas from code to spreadsheets.

**ATAN2 returns angles from -PI to PI (-180 to 180 degrees).** The angle is measured counterclockwise from the positive x-axis. Points in quadrant I (positive x, positive y) give angles 0 to 90 degrees. Quadrant II gives 90 to 180 degrees. Quadrant III gives -180 to -90 degrees. Quadrant IV gives -90 to 0 degrees. This full range makes ATAN2 far more useful than ATAN for coordinate-based calculations.

**ATAN2 gracefully handles the cases that break ATAN.** When x=0 (vertical direction), ATAN(y/x) fails with #DIV/0!, but ATAN2(0, y) correctly returns 90 or -90 degrees. When both x and y are 0, ATAN2 returns an error (there's no angle from the origin to itself). This robustness makes ATAN2 the safer choice for coordinate calculations.

## Syntax

> [!f(x)] ATAN2 Syntax
>
> ```
> =ATAN2(x_num, y_num)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x_num` | Yes | The x-coordinate of the point. Can be any real number including zero (unless y is also zero). Represents the horizontal distance from the origin. |
| `y_num` | Yes | The y-coordinate of the point. Can be any real number including zero (unless x is also zero). Represents the vertical distance from the origin. |

### Return Value

Returns the angle in **radians** from the positive x-axis to the point (x, y), measured counterclockwise. The result is between -PI and PI (approximately -3.1416 to 3.1416), equivalent to -180 to 180 degrees. Use DEGREES() to convert. Returns #DIV/0! error if both x and y are 0. Returns #VALUE! if either argument is non-numeric.

## Examples

> [!f(x)] ATAN2 Examples

### Example 1: First Quadrant (45 Degrees)
```
=DEGREES(ATAN2(1, 1))
```
**Result:** 45

**Explanation:** The point (1, 1) is at 45 degrees from the positive x-axis. This is the same as DEGREES(ATAN(1/1)), but ATAN2 also gives correct results in other quadrants.

---

### Example 2: Second Quadrant (135 Degrees)
```
=DEGREES(ATAN2(-1, 1))
```
**Result:** 135

**Explanation:** The point (-1, 1) is in the second quadrant, 135 degrees from the positive x-axis. ATAN alone could not distinguish this from the fourth quadrant.

---

### Example 3: Third Quadrant (-135 Degrees)
```
=DEGREES(ATAN2(-1, -1))
```
**Result:** -135

**Explanation:** The point (-1, -1) is in the third quadrant. ATAN2 returns -135 degrees (or equivalently, 225 degrees if you add 360).

---

### Example 4: Fourth Quadrant (-45 Degrees)
```
=DEGREES(ATAN2(1, -1))
```
**Result:** -45

**Explanation:** The point (1, -1) is in the fourth quadrant, -45 degrees from the positive x-axis. ATAN(y/x) = ATAN(-1) would give the same result, but ATAN2 is clearer and safer.

---

### Example 5: Positive Y-Axis (90 Degrees)
```
=DEGREES(ATAN2(0, 1))
```
**Result:** 90

**Explanation:** The point (0, 1) is straight up on the positive y-axis. ATAN(1/0) would fail with #DIV/0!, but ATAN2(0, 1) correctly returns 90 degrees.

---

### Example 6: Negative Y-Axis (-90 Degrees)
```
=DEGREES(ATAN2(0, -1))
```
**Result:** -90

**Explanation:** The point (0, -1) is straight down on the negative y-axis. ATAN2 handles this case that would break ATAN.

---

### Example 7: Positive X-Axis (0 Degrees)
```
=DEGREES(ATAN2(5, 0))
```
**Result:** 0

**Explanation:** The point (5, 0) is on the positive x-axis, angle = 0. Any point with y=0 and positive x gives 0 degrees.

---

### Example 8: Negative X-Axis (180 Degrees)
```
=DEGREES(ATAN2(-5, 0))
```
**Result:** 180

**Explanation:** The point (-5, 0) is on the negative x-axis, angle = 180 degrees (PI radians).

---

### Example 9: Error Case - Origin
```
=ATAN2(0, 0)
```
**Result:** #DIV/0!

**Explanation:** There's no angle from the origin to itself. Both coordinates being zero is the only error case for ATAN2.

---

### Example 10: Converting to 0-360 Degree Range
```
=MOD(DEGREES(ATAN2(x, y)) + 360, 360)
```
**Result:** Angle from 0 to 360 degrees

**Explanation:** ATAN2 returns -180 to 180. To get 0 to 360, add 360 and take MOD 360. This converts -45 degrees to 315 degrees, etc.

---

### Example 11: Compass Bearing from Coordinates
```
=MOD(90 - DEGREES(ATAN2(East, North)), 360)
```
**Result:** Compass bearing (0 = North, 90 = East, etc.)

**Explanation:** Compass bearings measure clockwise from north. This formula converts from standard mathematical angles (counterclockwise from east) to compass bearings.

---

### Example 12: Angle Between Two Points
```
=DEGREES(ATAN2(X2-X1, Y2-Y1))
```
**Result:** Direction angle from point 1 to point 2

**Explanation:** To find the direction from (X1, Y1) to (X2, Y2), use the differences. This gives the angle of the line connecting the points.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Both x and y are 0 | Check for origin case before calling ATAN2. There's no angle to the origin. |
| `#VALUE!` | Non-numeric input | Ensure both arguments are numbers. |
| `Wrong argument order` | Used (y, x) instead of (x, y) | Excel uses ATAN2(x, y), not (y, x) like programming languages. |
| `Result is decimal, expected degrees` | ATAN2 returns radians | Wrap with DEGREES(): `=DEGREES(ATAN2(x, y))` |
| `#NAME?` | Misspelled function name | Check spelling: ATAN2, not ATAN_2 or ARCTAN2. |
| `Negative angle when expected positive` | ATAN2 returns -180 to 180 | Use `=MOD(DEGREES(ATAN2(x,y))+360, 360)` for 0-360 range. |

## Use Cases

### [[Coordinate System Conversion (Cartesian to Polar)]]

**Scenario:** Convert (x, y) coordinates to polar form (angle, radius).

**Implementation:**
```
Angle: =DEGREES(ATAN2(X, Y))
Radius: =SQRT(X^2 + Y^2)
0-360 Angle: =MOD(DEGREES(ATAN2(X, Y)) + 360, 360)
```

**Business Application:** Data visualization (polar charts), physics simulations, mapping applications, radar displays, wind rose diagrams.

**Technical Details:** Polar coordinates represent points as (radius, angle) instead of (x, y). ATAN2 gives the angle; SQRT(x^2+y^2) gives the radius. Together they provide complete polar conversion.

---

### [[Navigation and Heading Calculations]]

**Scenario:** Calculate compass bearings, navigation headings, and direction between points.

**Implementation:**
```
=MOD(90 - DEGREES(ATAN2(EastOffset, NorthOffset)), 360)      → Compass bearing
=DEGREES(ATAN2(Longitude2-Longitude1, Latitude2-Latitude1))  → Initial bearing (simplified)
=MOD(DEGREES(ATAN2(DeltaX, DeltaY)) + 360, 360)              → Direction 0-360
```

**Business Application:** GPS navigation, maritime routing, aviation heading calculations, delivery route optimization, drone flight path planning.

**Technical Details:** Standard math angles measure counterclockwise from east. Compass bearings measure clockwise from north. The formula `90 - angle` converts between them, with MOD handling the wrap-around.

---

### [[Game Development and Graphics]]

**Scenario:** Calculate sprite rotation, aiming direction, and camera angles from coordinate differences.

**Implementation:**
```
=DEGREES(ATAN2(TargetX - PlayerX, TargetY - PlayerY))  → Aim angle
=ATAN2(MouseX - CenterX, MouseY - CenterY)            → Pointer direction (radians)
=MOD(DEGREES(ATAN2(Vx, Vy)) + 360, 360)               → Movement direction
```

**Business Application:** Video game development, interactive data visualizations, animation systems, UI compass/direction indicators.

**Technical Details:** In games and graphics, ATAN2 is constantly used to point objects at targets, rotate sprites to face movement direction, and calculate angles for collision detection. Most game engines use atan2(y, x), so adapt the argument order.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 5.0
- **Syntax:** ATAN2(x_num, y_num) — x first, then y
- **Output:** Radians only (use DEGREES() for conversion)
- **Range:** -PI to PI (-180 to 180 degrees)
- **Error:** Returns #DIV/0! when both arguments are 0

### Google Sheets
- **Availability:** All versions
- **Syntax:** ATAN2(x, y) — same as Excel, x first
- **Output:** Radians only
- **Range:** Same as Excel
- **Error:** Same behavior as Excel

### Both Platforms vs Programming Languages
- **Excel/Sheets:** ATAN2(x, y) — x comes first
- **Python, JavaScript, C, Java, etc.:** atan2(y, x) — y comes first
- This is a major source of bugs when porting code. Always verify argument order!

## Tips and Best Practices

1. **ALWAYS prefer ATAN2 over ATAN for coordinates:** ATAN2 handles all quadrants and the vertical case (x=0). There's rarely a reason to use ATAN when you have x and y separately.

2. **Remember: Excel uses ATAN2(x, y), not (y, x):** This is opposite to most programming languages. Triple-check argument order.

3. **Convert to 0-360 range when needed:** Use `=MOD(DEGREES(ATAN2(x,y))+360, 360)` to get positive angles from 0 to 360 degrees.

4. **Handle the origin case:** ATAN2(0, 0) returns #DIV/0!. Use `=IF(AND(x=0, y=0), 0, ATAN2(x, y))` if you need a default value.

5. **For compass bearings, adjust the formula:** Standard angles measure counterclockwise from east; compass bearings measure clockwise from north. Use `=MOD(90-DEGREES(ATAN2(x,y))+360, 360)`.

6. **Use with SQRT for complete polar conversion:** Angle = ATAN2, Radius = SQRT(x^2+y^2). This pair converts any (x, y) to polar coordinates.

7. **Direction between points:** To find direction from point A to point B, use ATAN2(Bx-Ax, By-Ay).

8. **Combine with IF for special cases:** For navigation where "no movement" should return a specific heading, check for zero movement before calling ATAN2.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ATAN]] | Returns arctangent of a single value | Only when you have pre-calculated slope and don't need quadrant detection |
| [[ASIN]] | Returns arcsine | When you have opposite/hypotenuse ratio, not coordinates |
| [[ACOS]] | Returns arccosine | When you have adjacent/hypotenuse ratio, not coordinates |

### Commonly Used Together

**[[DEGREES]]** - Convert radians to degrees

*Essential for human-readable output:*
```
=DEGREES(ATAN2(1, 1))    → Returns 45
```
ATAN2 returns radians; DEGREES makes it understandable.

---

**[[SQRT]]** - Calculate radius (magnitude)

*Complete polar conversion:*
```
Angle: =DEGREES(ATAN2(X, Y))
Radius: =SQRT(X^2 + Y^2)
```
Together these convert Cartesian to polar coordinates.

---

**[[MOD]]** - Convert to 0-360 degree range

*Normalize angle range:*
```
=MOD(DEGREES(ATAN2(X, Y)) + 360, 360)
```
Changes -180 to 180 range into 0 to 360 range.

---

**[[SIN]] and [[COS]]** - Convert back to coordinates

*Reverse conversion:*
```
X: =Radius * COS(ATAN2(X, Y))
Y: =Radius * SIN(ATAN2(X, Y))
```
Reconstruct coordinates from polar form.

---

**[[IF]]** - Handle origin case

*Avoid error at origin:*
```
=IF(AND(X=0, Y=0), 0, DEGREES(ATAN2(X, Y)))
```
Provide default when both coordinates are zero.

## Official Documentation

- **Microsoft Excel:** [ATAN2 function](https://support.microsoft.com/en-us/office/atan2-function-c04592ab-b9e3-4908-b428-c96b3a565033)
- **Google Sheets:** [ATAN2 function](https://support.google.com/docs/answer/3093469)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original function |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
