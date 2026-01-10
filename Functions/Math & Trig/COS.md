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
- cosine
- radians
- angles
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Cosine Function
- Cos
- Trigonometric Cosine
tags:
- trigonometry
- cosine
- radians
- angles
- circular-functions
- wave
---

# COS

## Description

**COS** returns the cosine of an angle. Like all spreadsheet trigonometric functions, the input must be in **radians, not degrees**. If your angle is in degrees, convert it first with RADIANS() or multiply by PI()/180. COS(60) gives you the cosine of 60 radians (approximately -0.952), not the cosine of 60 degrees (which is 0.5). This radians-versus-degrees confusion is the #1 source of errors in trigonometric calculations.

Cosine is the "sibling" of sine in trigonometry. In a right triangle, cosine is the ratio of the adjacent side to the hypotenuse. On the unit circle, cosine represents the **x-coordinate** of a point at a given angle from the positive x-axis (while sine gives the y-coordinate). COS oscillates between -1 and 1, completing one full cycle every 2*PI radians (360 degrees). While sine starts at 0 and rises, cosine starts at its maximum (1) and decreases—they're the same wave shape, just shifted by 90 degrees (PI/2 radians).

**The relationship between sine and cosine is fundamental:** cos(x) = sin(x + PI/2), and sin(x)^2 + cos(x)^2 = 1 (the Pythagorean identity). Together, SIN and COS define circular motion, rotation, and the transformation between polar and Cartesian coordinates. In physics and engineering, they represent orthogonal components of vectors, oscillations that are 90 degrees out of phase, and the real/imaginary parts of complex exponentials. You rarely use cosine in isolation—it almost always appears alongside sine.

**Key values to know:** COS(0) = 1 (cosine starts at maximum), COS(PI()/3) = 0.5 (60 degrees), COS(PI()/4) = 0.707... (45 degrees—equal to sine at this angle), COS(PI()/2) = 0 (90 degrees), COS(PI()) = -1 (180 degrees—the minimum). If you're calculating coordinates on a circle, cosine gives the horizontal component: x = radius * COS(angle).

## Syntax

> [!f(x)] COS Syntax
>
> ```
> =COS(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The angle in **radians** for which you want the cosine. Can be any real number—positive, negative, or zero. For angles in degrees, use RADIANS(degrees) or multiply by PI()/180. Cell references and formula results are accepted. |

### Return Value

Returns a numeric value between -1 and 1 (inclusive). The cosine function is bounded and never returns values outside this range. Returns the cosine of the input angle measured in radians. If input is non-numeric, returns #VALUE! error.

## Examples

> [!f(x)] COS Examples

### Example 1: Cosine of Zero
```
=COS(0)
```
**Result:** 1

**Explanation:** The cosine of 0 radians is 1—the maximum value. This is where cosine "starts" on the unit circle: at angle 0, the point is at (1, 0), so the x-coordinate (cosine) is 1.

---

### Example 2: Cosine of 60 Degrees (Converting Properly)
```
=COS(RADIANS(60))
```
**Result:** 0.5

**Explanation:** COS(60 degrees) = 0.5 exactly. This is one of the "special angles" from trigonometry. Note: =COS(60) without RADIANS would give approximately -0.952, which is the cosine of 60 radians—completely wrong.

---

### Example 3: Common Mistake - Forgetting Radians
```
=COS(45)
```
**Result:** 0.5253219888...

**Explanation:** This is NOT cos(45 degrees), which equals approximately 0.707. This is cos(45 radians). Since most people work with degrees, forgetting to convert is extremely common. **Use =COS(RADIANS(45)) to get the expected 0.707...**

---

### Example 4: Cosine of 90 Degrees
```
=COS(RADIANS(90))
```
**Result:** 6.12323399573677E-17 (essentially 0)

**Explanation:** Mathematically, cos(90 degrees) = 0 exactly. The tiny number (10^-17) is a floating-point artifact. For practical purposes, treat this as zero. At 90 degrees on the unit circle, the x-coordinate is 0.

---

### Example 5: Cosine of 180 Degrees
```
=COS(RADIANS(180))
```
**Result:** -1

**Explanation:** cos(180 degrees) = cos(PI) = -1, the minimum value. At 180 degrees, the point on the unit circle is at (-1, 0), so the x-coordinate (cosine) is -1.

---

### Example 6: Alternative Conversion Method
```
=COS(60 * PI() / 180)
```
**Result:** 0.5

**Explanation:** Multiplying degrees by PI()/180 converts to radians. This is equivalent to using RADIANS() but shows the math explicitly. Either method works.

---

### Example 7: Negative Angles
```
=COS(RADIANS(-45))
```
**Result:** 0.7071067812...

**Explanation:** Cosine is an **even function**: cos(-x) = cos(x). The cosine of -45 degrees equals the cosine of +45 degrees. This is different from sine, which is odd. On the unit circle, reflecting across the x-axis doesn't change the x-coordinate.

---

### Example 8: Angles Beyond 360 Degrees
```
=COS(RADIANS(450))
```
**Result:** 6.12323399573677E-17 (essentially 0)

**Explanation:** 450 degrees = 360 + 90 = one full rotation plus 90 degrees. Since cosine is periodic with period 360 degrees, cos(450) = cos(90) = 0 (with floating-point approximation).

---

### Example 9: Using PI Directly
```
=COS(PI()/3)
```
**Result:** 0.5

**Explanation:** PI/3 radians = 60 degrees. Using PI() directly avoids the degree conversion and is mathematically cleaner for standard angles. COS(PI()/3) = 0.5 exactly.

---

### Example 10: X-Coordinate on a Circle
```
=5 * COS(RADIANS(30))
```
**Result:** 4.330127019...

**Explanation:** For a point at 30 degrees on a circle of radius 5, the x-coordinate is 5 * cos(30 degrees) = 5 * 0.866... = 4.33. This is the fundamental use of cosine in coordinate calculations.

---

### Example 11: Creating a Cosine Wave
```
=COS(A1 * 2 * PI() / 100)
```
**Result:** Cosine wave value for position A1

**Explanation:** If A1 contains 0, 1, 2, ..., 100, this traces one complete cosine wave. At A1=0, result is 1 (cosine starts at maximum). At A1=25, result is 0. At A1=50, result is -1.

---

### Example 12: Phase Relationship with Sine
```
=COS(RADIANS(A1))
=SIN(RADIANS(A1 + 90))
```
**Result:** Both formulas give identical results

**Explanation:** Cosine is just sine shifted by 90 degrees: cos(x) = sin(x + 90 degrees). This relationship is fundamental in signal processing and physics.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is numeric. Text like "45" needs VALUE() conversion. |
| `Unexpected result` | Forgot radians conversion | Use RADIANS(degrees) or multiply by PI()/180. This is the most common mistake. |
| `Very small number instead of 0` | Floating-point precision | Values like 6.12E-17 are effectively 0. Use ROUND() if needed for clean display. |
| `#NAME?` | Misspelled function name | Check spelling: COS, not COSINE. |
| `Result outside -1 to 1` | Formula error, not the COS function | COS always returns between -1 and 1. Check your formula structure. |
| `Same result for positive and negative` | Correct—cosine is an even function | cos(-x) = cos(x). This is mathematically correct, unlike sine. |

## Use Cases

### [[Circular Motion and Coordinate Calculation]]

**Scenario:** Calculate x-coordinates for circular paths, rotations, and polar-to-Cartesian conversion.

**Implementation:**
```
=Radius * COS(RADIANS(Angle))           → X-coordinate
=CenterX + Radius * COS(RADIANS(Angle)) → Absolute X-position
=COS(RADIANS(Heading)) * Distance       → East/West component
```

**Business Application:** Animation systems, game development, robotic arm positioning, radar displays, navigation calculations, satellite orbit modeling.

**Technical Details:** Combined with SIN for y-coordinates, COS completes the parametric equations for a circle: x = r*cos(theta), y = r*sin(theta). For navigation, COS gives the east-west component when angle is measured from north.

---

### [[Vector Decomposition and Physics]]

**Scenario:** Break vectors into horizontal (x) components for force analysis, velocity, or displacement.

**Implementation:**
```
=Force * COS(RADIANS(AngleFromHorizontal))  → Horizontal force component
=Velocity * COS(RADIANS(LaunchAngle))       → Horizontal velocity
=SUM(Force1*COS(RADIANS(Angle1)), Force2*COS(RADIANS(Angle2)))  → Net horizontal force
```

**Business Application:** Engineering stress analysis, projectile motion, structural load calculations, wind force analysis, athletic performance analysis.

**Technical Details:** Any vector can be decomposed into orthogonal components. The horizontal component uses COS (adjacent over hypotenuse), while the vertical uses SIN. The angle must be measured consistently from the horizontal axis.

---

### [[3D Graphics and Rotation]]

**Scenario:** Calculate rotation matrices, transform coordinates, create circular or spiral patterns.

**Implementation:**
```
NewX: =X*COS(RADIANS(Theta)) - Y*SIN(RADIANS(Theta))
NewY: =X*SIN(RADIANS(Theta)) + Y*COS(RADIANS(Theta))
Spiral: =A1*COS(RADIANS(A1*10))  → X with expanding radius
```

**Business Application:** Computer graphics, CAD systems, data visualization, creating artistic patterns in charts, modeling orbital mechanics.

**Technical Details:** The 2D rotation matrix uses both COS and SIN. To rotate point (x, y) by angle theta counterclockwise: x' = x*cos(theta) - y*sin(theta), y' = x*sin(theta) + y*cos(theta). This extends to 3D with rotation matrices around each axis.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Input:** Radians only (use RADIANS() for conversion)
- **Precision:** 15 significant digits
- **Array support:** Works with dynamic arrays; use CSE (Ctrl+Shift+Enter) in older versions
- **Performance:** Highly optimized, negligible overhead

### Google Sheets
- **Availability:** All versions
- **Input:** Radians only (same as Excel)
- **Precision:** 15 significant digits
- **Array support:** Works with ARRAYFORMULA
- **Performance:** Comparable to Excel

### Both Platforms
- Both require radians input—no degree option
- Both have identical floating-point precision characteristics
- Both return the same small non-zero values for theoretical zeros (like COS(PI/2))
- No functional differences between platforms for this function

## Tips and Best Practices

1. **Always pair COS with SIN for coordinates:** x = r*COS(angle), y = r*SIN(angle). They're rarely used alone in practical applications.

2. **Remember COS is even:** COS(-x) = COS(x), which means the cosine curve is symmetric about the y-axis. This can simplify calculations involving reflected angles.

3. **Use cosine for horizontal, sine for vertical:** In standard coordinate systems, COS gives the x-component (horizontal) and SIN gives the y-component (vertical) when the angle is measured from the positive x-axis.

4. **Know the phase relationship:** COS(x) = SIN(x + PI/2). Cosine leads sine by 90 degrees. This is crucial in signal processing and physics.

5. **Key values for quick checks:** COS(0)=1, COS(PI/3)=0.5, COS(PI/2)=0, COS(PI)=-1. Use these to verify your formulas are working correctly.

6. **Clean up floating-point artifacts:** Use ROUND(COS(RADIANS(90)), 10) to get exactly 0 instead of 6.12E-17 when you need clean output.

7. **Cosine rule for triangles:** For non-right triangles, c^2 = a^2 + b^2 - 2ab*cos(C). This extends trigonometry beyond right triangles.

8. **Create rotation formulas as a pair:** When rotating coordinates, always implement both x' and y' transformations together to avoid partial rotations.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SIN]] | Returns the sine of an angle | When you need the y-component or opposite/hypotenuse ratio |
| [[TAN]] | Returns the tangent of an angle | When you need the slope (rise over run) or opposite/adjacent ratio |
| [[ACOS]] | Returns the arccosine (inverse cosine) | When you have a cosine value and need to find the angle |
| [[COSH]] | Returns the hyperbolic cosine | For catenary curves and hyperbolic geometry, not circular |
| [[RADIANS]] | Converts degrees to radians | To prepare degree values for COS |

### Commonly Used Together

**[[SIN]]** - Sine function for y-coordinates

*Complete coordinate calculations:*
```
X: =Radius * COS(RADIANS(Angle))
Y: =Radius * SIN(RADIANS(Angle))
```
COS and SIN together define any point on a circle.

---

**[[RADIANS]]** - Convert degrees to radians

*Essential conversion:*
```
=COS(RADIANS(45))
```
The standard pattern for using COS with degree inputs.

---

**[[SQRT]]** - Square root for distance and verification

*Verify the Pythagorean identity:*
```
=SQRT(COS(A1)^2 + SIN(A1)^2)    → Always equals 1
```
sin^2 + cos^2 = 1 for any angle.

---

**[[ATAN2]]** - Calculate angle from coordinates

*Reverse the coordinate calculation:*
```
=ATAN2(X, Y)    → Returns angle in radians
```
To find the angle when you have x and y coordinates.

---

**[[PI]]** - The constant PI for radian calculations

*Direct angle specification:*
```
=COS(PI()/4)    → Returns 0.707... (45 degrees)
=COS(PI())      → Returns -1 (180 degrees)
```
For mathematically precise standard angles.

## Official Documentation

- **Microsoft Excel:** [COS function](https://support.microsoft.com/en-us/office/cos-function-0fb808a5-95d6-4553-8148-22aebdce5f05)
- **Google Sheets:** [COS function](https://support.google.com/docs/answer/3093476)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
