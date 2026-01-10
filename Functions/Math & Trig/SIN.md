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
- sine
- radians
- angles
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sine Function
- Sin
- Trigonometric Sine
tags:
- trigonometry
- sine
- radians
- angles
- circular-functions
- wave
---

# SIN

## Description

**SIN** returns the sine of an angle. The input must be in **radians, not degrees**—this is the single most common source of confusion with all spreadsheet trigonometric functions. If you have an angle in degrees (like 30 or 45), you must convert it first using RADIANS() or multiply by PI()/180. SIN(30) does not give you the sine of 30 degrees; it gives you the sine of 30 radians (approximately -0.988), which is almost certainly not what you want.

The sine function is one of the fundamental trigonometric functions, arising from the ratio of the opposite side to the hypotenuse in a right triangle. On the unit circle (a circle with radius 1 centered at the origin), sine represents the y-coordinate of a point at a given angle from the positive x-axis. SIN oscillates smoothly between -1 and 1, completing one full cycle every 2*PI radians (360 degrees). This periodic behavior makes sine essential for modeling waves, oscillations, circular motion, and any phenomenon that repeats regularly.

**Understanding radians vs degrees is crucial.** Degrees are the familiar system where a full circle is 360 degrees—convenient for everyday use because it divides evenly into many fractions. Radians are the mathematician's choice: a full circle is 2*PI radians (approximately 6.283). One radian is the angle where the arc length equals the radius—about 57.3 degrees. Excel and Sheets use radians because calculus and advanced mathematics work more elegantly with radians (the derivative of sin(x) is exactly cos(x) only when x is in radians).

**Key values to memorize:** SIN(0) = 0, SIN(PI()/6) = 0.5 (30 degrees), SIN(PI()/4) = 0.707... (45 degrees), SIN(PI()/2) = 1 (90 degrees), SIN(PI()) = 0 (180 degrees). If you're getting results that look wrong, the first thing to check is whether you forgot to convert degrees to radians. The formula pattern `=SIN(RADIANS(angle_in_degrees))` should become second nature.

## Syntax

> [!f(x)] SIN Syntax
>
> ```
> =SIN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The angle in **radians** for which you want the sine. Can be any real number—positive, negative, or zero. For angles in degrees, use RADIANS(degrees) or multiply by PI()/180. Cell references and formula results are accepted. |

### Return Value

Returns a numeric value between -1 and 1 (inclusive). The sine function is bounded: it never returns a value less than -1 or greater than 1. Returns the sine of the input angle measured in radians. If input is non-numeric, returns #VALUE! error.

## Examples

> [!f(x)] SIN Examples

### Example 1: Sine of Zero
```
=SIN(0)
```
**Result:** 0

**Explanation:** The sine of 0 radians (0 degrees) is exactly 0. This is the starting point of the sine wave. On the unit circle, an angle of 0 corresponds to the point (1, 0), where the y-coordinate (sine) is 0.

---

### Example 2: Sine of 90 Degrees (The Radians Trap)
```
=SIN(RADIANS(90))
```
**Result:** 1

**Explanation:** 90 degrees = PI/2 radians. The sine of 90 degrees is 1—the maximum value. **Note:** =SIN(90) would give approximately 0.894, which is the sine of 90 RADIANS—a completely different value. Always convert degrees to radians.

---

### Example 3: Common Mistake - Forgetting Radians
```
=SIN(30)
```
**Result:** -0.9880316241...

**Explanation:** This is NOT the sine of 30 degrees (which is 0.5). This is the sine of 30 radians. Since 30 radians is about 4.77 full rotations (30/(2*PI)), the result is some value between -1 and 1, but not what most users expect. **Use =SIN(RADIANS(30)) to get 0.5.**

---

### Example 4: Sine of 30 Degrees (Correct)
```
=SIN(RADIANS(30))
```
**Result:** 0.5

**Explanation:** Converting 30 degrees to radians first (PI/6), then taking the sine. This is one of the "special angles" from trigonometry: sin(30 degrees) = 1/2 exactly. The result is 0.5.

---

### Example 5: Sine of 45 Degrees
```
=SIN(RADIANS(45))
```
**Result:** 0.7071067812...

**Explanation:** sin(45 degrees) = sin(PI/4) = square root of 2 divided by 2, approximately 0.707. This is another special angle. At 45 degrees, sine and cosine are equal.

---

### Example 6: Alternative Radians Conversion
```
=SIN(45 * PI() / 180)
```
**Result:** 0.7071067812...

**Explanation:** Instead of RADIANS(45), you can multiply degrees by PI()/180. Both methods work identically. RADIANS() is cleaner; PI()/180 is more explicit about the math.

---

### Example 7: Sine of PI
```
=SIN(PI())
```
**Result:** 1.22464679914735E-16 (essentially 0)

**Explanation:** Mathematically, sin(PI) = 0 exactly. Due to floating-point representation, you get a tiny number very close to zero (10^-16). For practical purposes, this is zero. This illustrates that trig functions can have tiny rounding artifacts.

---

### Example 8: Negative Angles
```
=SIN(RADIANS(-30))
```
**Result:** -0.5

**Explanation:** Sine is an odd function: sin(-x) = -sin(x). The sine of -30 degrees is -0.5. Negative angles go clockwise on the unit circle, giving negative y-coordinates in the fourth quadrant.

---

### Example 9: Angles Beyond 360 Degrees
```
=SIN(RADIANS(390))
```
**Result:** 0.5

**Explanation:** 390 degrees = 360 + 30 = one full rotation plus 30 degrees. Since sine is periodic with period 360 degrees (2*PI radians), sin(390 degrees) = sin(30 degrees) = 0.5. The function wraps around.

---

### Example 10: Using Cell Reference
```
=SIN(RADIANS(A1))
```
**Result:** Sine of angle in A1 (assuming A1 contains degrees)

**Explanation:** Practical usage with cell references. If A1 contains 60, this returns sin(60 degrees) = approximately 0.866. Always clarify in your spreadsheet whether angle columns contain degrees or radians.

---

### Example 11: Creating a Sine Wave
```
=SIN(A1 * 2 * PI() / 100)
```
**Result:** Sine wave value for position A1

**Explanation:** If A1 contains values 0, 1, 2, ..., 100, this creates one complete sine wave cycle. Multiply by an amplitude factor for waves of different heights: `=5*SIN(A1*2*PI()/100)` creates a wave oscillating between -5 and 5.

---

### Example 12: Phase-Shifted Sine Wave
```
=SIN(RADIANS(A1) + PI()/4)
```
**Result:** Sine wave shifted by 45 degrees

**Explanation:** Adding PI/4 (45 degrees) to the angle shifts the wave. Phase shifts are common in signal processing and physics. The wave peaks earlier by 45 degrees compared to the standard sine.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text strings like "45" may need conversion with VALUE(). |
| `Unexpected result` | Forgot to convert degrees to radians | Use RADIANS(degrees) or multiply by PI()/180. This is the #1 mistake with trig functions. |
| `Result is E-16 instead of 0` | Floating-point precision | Values like 1.22E-16 are effectively zero. Round if needed: =ROUND(SIN(PI()), 10) returns 0. |
| `#NAME?` | Misspelled function name | Check spelling: SIN, not SINE. |
| `Result outside -1 to 1` | Impossible—check your formula | SIN always returns between -1 and 1. If you see values outside this range, the formula is wrong. |
| `Wrong sign` | Angle in wrong quadrant | Remember: sine is positive in quadrants I and II (0-180 degrees), negative in III and IV (180-360 degrees). |

## Use Cases

### [[Wave and Signal Generation]]

**Scenario:** Create sine waves for modeling oscillations, signals, or periodic data.

**Implementation:**
```
=Amplitude * SIN(2 * PI() * Frequency * Time + Phase)
=SIN(RADIANS(360 * A1 / Period))
=SIN(2 * PI() * A1 / WaveLength)
```

**Business Application:** Audio synthesis, signal processing prototypes, financial cycle modeling, seasonal pattern simulation, engineering vibration analysis.

**Technical Details:** The general sine wave formula is A*sin(2*PI*f*t + phi) where A is amplitude, f is frequency, t is time, and phi is phase shift. In spreadsheets, create a time column and calculate corresponding sine values to visualize or analyze periodic phenomena.

---

### [[Circular Motion and Position Calculations]]

**Scenario:** Calculate y-coordinates for points moving in circles or arcs.

**Implementation:**
```
=Radius * SIN(RADIANS(Angle))           → Y-coordinate from center
=CenterY + Radius * SIN(RADIANS(Angle)) → Absolute Y-position
=SIN(RADIANS(Heading)) * Distance       → North/South component of movement
```

**Business Application:** Animation path calculations, radar displays, navigation systems, robotics positioning, game development.

**Technical Details:** For circular motion, combine SIN (y-component) with COS (x-component). The parametric equations x = r*cos(theta), y = r*sin(theta) trace a circle of radius r. Heading calculations in navigation use sine for the north-south component.

---

### [[Right Triangle Calculations]]

**Scenario:** Solve for unknown sides or verify triangle measurements.

**Implementation:**
```
=Hypotenuse * SIN(RADIANS(Angle))       → Opposite side length
=Opposite / SIN(RADIANS(Angle))          → Hypotenuse length
=SIN(RADIANS(A)) / a = SIN(RADIANS(B)) / b  → Law of Sines
```

**Business Application:** Construction calculations, surveying, architecture, structural engineering, shadow length calculations.

**Technical Details:** In a right triangle, sin(angle) = opposite/hypotenuse. For non-right triangles, use the Law of Sines: a/sin(A) = b/sin(B) = c/sin(C). Always ensure consistent units and degree-to-radian conversion.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Input:** Radians only (use RADIANS() for degree conversion)
- **Precision:** 15 significant digits
- **Array support:** Works with arrays in dynamic array versions; use CSE in older versions
- **Special values:** Returns #VALUE! for non-numeric input

### Google Sheets
- **Availability:** All versions
- **Input:** Radians only (identical to Excel)
- **Precision:** 15 significant digits (same as Excel)
- **Array support:** Works naturally with ARRAYFORMULA
- **Special values:** Same error handling as Excel

### Both Platforms
- Both require radians—there is no optional parameter for degrees
- Both return identical results for the same inputs
- Both handle very large radian values correctly (wrapping around)
- Both have the same floating-point precision quirks (e.g., SIN(PI()) is not exactly 0)

## Tips and Best Practices

1. **Always convert degrees to radians:** Make `=SIN(RADIANS(angle))` your default pattern. If your data is already in radians (rare outside scientific contexts), you can use SIN directly.

2. **Create a conversion helper:** Put `=PI()/180` in a named cell (e.g., "DegToRad") and use `=SIN(A1*DegToRad)` for cleaner formulas when working with many degree values.

3. **Remember key values for sanity checks:** SIN(0)=0, SIN(RADIANS(30))=0.5, SIN(RADIANS(90))=1. If your results don't match these for known inputs, check your radian conversion.

4. **Use with COS for coordinates:** For circular patterns, you almost always need both: x = r*COS(angle), y = r*SIN(angle). These work together as a pair.

5. **Handle floating-point artifacts:** `=ROUND(SIN(PI()), 10)` cleans up tiny values that should be zero. Important when comparing results or displaying clean output.

6. **Understand the period:** SIN repeats every 2*PI radians (360 degrees). SIN(RADIANS(x)) = SIN(RADIANS(x + 360)) = SIN(RADIANS(x + 720)), etc.

7. **Leverage the odd function property:** SIN(-x) = -SIN(x). This can simplify formulas involving reflected angles.

8. **For inverse operations use ASIN:** To find the angle from a sine value, use ASIN. Remember ASIN returns radians; convert with DEGREES() if needed.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COS]] | Returns the cosine of an angle | When you need the x-component of circular motion or the adjacent/hypotenuse ratio |
| [[TAN]] | Returns the tangent of an angle | When you need the ratio of opposite to adjacent (slope), or SIN/COS |
| [[ASIN]] | Returns the arcsine (inverse sine) | When you have a sine value and need to find the angle |
| [[SINH]] | Returns the hyperbolic sine | For hyperbolic (not circular) trigonometry—catenary curves, special relativity |
| [[RADIANS]] | Converts degrees to radians | When you need to convert before using SIN (very common) |

### Commonly Used Together

**[[RADIANS]]** - Convert degrees to radians

*The essential partner function:*
```
=SIN(RADIANS(45))
```
Converts 45 degrees to radians, then calculates sine. This pattern is used in virtually every practical SIN formula.

---

**[[COS]]** - Cosine function for x-coordinates

*Circular motion coordinates:*
```
X: =Radius * COS(RADIANS(Angle))
Y: =Radius * SIN(RADIANS(Angle))
```
SIN and COS together define points on a circle. Used in rotation, animation, and coordinate calculations.

---

**[[DEGREES]]** - Convert radians back to degrees

*When displaying angles:*
```
=DEGREES(ASIN(0.5))    → Returns 30
```
After inverse trig functions return radians, DEGREES converts for human-readable output.

---

**[[SQRT]]** - Square root for distance calculations

*Combining trig with Pythagorean theorem:*
```
=SQRT(SIN(angle)^2 + COS(angle)^2)    → Always equals 1
```
The Pythagorean identity: sin squared plus cos squared equals 1.

---

**[[PI]]** - The constant PI for radian calculations

*Direct radian specification:*
```
=SIN(PI()/2)    → Returns 1 (sine of 90 degrees)
=SIN(PI()/6)    → Returns 0.5 (sine of 30 degrees)
```
For mathematically precise angles, specify directly in terms of PI.

## Official Documentation

- **Microsoft Excel:** [SIN function](https://support.microsoft.com/en-us/office/sin-function-cf0e3432-8b9e-483c-bc55-a76651c95602)
- **Google Sheets:** [SIN function](https://support.google.com/docs/answer/3093447)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
