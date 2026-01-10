---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- angle-conversion
- radians
- degrees
- trigonometry
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Convert Degrees to Radians
- Degree to Radian Conversion
tags:
- angle-conversion
- radians
- degrees
- trigonometry
- unit-conversion
---

# RADIANS

## Description

**RADIANS** converts an angle from degrees to radians. This is the **essential preprocessing function** for all trigonometric calculations in spreadsheets because SIN, COS, TAN, and their variants all require input in radians, not degrees. If you have angles in degrees (the format most humans use), you must convert them with RADIANS before passing to trig functions.

**Why radians?** In mathematics, radians are the "natural" unit for angles. A radian is defined as the angle where the arc length equals the radius—approximately 57.3 degrees. A full circle is 2*PI radians. Calculus works elegantly with radians: the derivative of sin(x) is exactly cos(x) only when x is in radians. Spreadsheets follow this mathematical convention, which unfortunately creates friction for users who think in degrees.

**The conversion formula:** radians = degrees * (PI / 180). RADIANS simply multiplies by PI/180 (approximately 0.01745329). You could write `=A1 * PI() / 180` instead of `=RADIANS(A1)`, but RADIANS is cleaner and communicates intent. The function accepts any numeric value—there's no restriction to 0-360 or any "valid" range.

**The critical pattern:** When using SIN, COS, or TAN with degree values, always use `=SIN(RADIANS(degrees))`, `=COS(RADIANS(degrees))`, or `=TAN(RADIANS(degrees))`. This is not optional—forgetting RADIANS is the #1 source of errors in spreadsheet trigonometry. SIN(30) does NOT give you sin(30 degrees); it gives sin(30 radians), which is completely wrong for typical use cases.

## Syntax

> [!f(x)] RADIANS Syntax
>
> ```
> =RADIANS(angle)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `angle` | Yes | The angle in degrees that you want to convert to radians. Can be any numeric value—positive, negative, or zero. Typically human-entered angles like 30, 45, 60, 90, 180, 360, etc. |

### Return Value

Returns a numeric value representing the angle in radians. The conversion multiplies by PI/180 (approximately 0.01745329). There are no range restrictions—any number converts mathematically. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] RADIANS Examples

### Example 1: Convert 180 Degrees to Radians
```
=RADIANS(180)
```
**Result:** 3.14159265358979... (PI)

**Explanation:** 180 degrees = PI radians. This is the fundamental relationship—a straight angle is PI radians.

---

### Example 2: Convert 90 Degrees to Radians
```
=RADIANS(90)
```
**Result:** 1.5707963267949... (PI/2)

**Explanation:** 90 degrees = PI/2 radians (a right angle, quarter circle). One of the most common conversions.

---

### Example 3: Convert 360 Degrees to Radians
```
=RADIANS(360)
```
**Result:** 6.28318530717959... (2*PI)

**Explanation:** 360 degrees = 2*PI radians (full circle). This confirms the full rotation relationship.

---

### Example 4: Using with SIN
```
=SIN(RADIANS(30))
```
**Result:** 0.5

**Explanation:** The correct way to calculate sin(30 degrees). RADIANS(30) converts to PI/6 radians, then SIN returns 0.5 exactly.

---

### Example 5: The WRONG Way (Common Mistake)
```
=SIN(30)
```
**Result:** -0.9880316241...

**Explanation:** This is SIN of 30 RADIANS, not 30 degrees! 30 radians is about 4.77 full circles, giving a seemingly random result. This is the most common trigonometry error in spreadsheets.

---

### Example 6: Using with COS
```
=COS(RADIANS(60))
```
**Result:** 0.5

**Explanation:** cos(60 degrees) = 0.5 exactly. Without RADIANS, COS(60) would give approximately -0.952—completely wrong.

---

### Example 7: Using with TAN
```
=TAN(RADIANS(45))
```
**Result:** 1

**Explanation:** tan(45 degrees) = 1 exactly (rise equals run). Without RADIANS, TAN(45) gives approximately 1.62—wrong.

---

### Example 8: Converting Small Angles
```
=RADIANS(1)
```
**Result:** 0.0174532925...

**Explanation:** 1 degree = approximately 0.01745 radians. This shows the scale difference: degrees are roughly 57 times larger than radians.

---

### Example 9: Negative Angles
```
=RADIANS(-45)
```
**Result:** -0.7853981634...

**Explanation:** Negative degrees convert to negative radians. -45 degrees = -PI/4 radians.

---

### Example 10: Cell Reference
```
=SIN(RADIANS(A1))
```
**Result:** Sine of angle in A1 (assuming A1 contains degrees)

**Explanation:** Standard pattern for working with degree values stored in cells. If A1 contains 30, this returns 0.5.

---

### Example 11: Multiple Common Angles
```
=RADIANS(30)   → 0.5236 (PI/6)
=RADIANS(45)   → 0.7854 (PI/4)
=RADIANS(60)   → 1.0472 (PI/3)
=RADIANS(90)   → 1.5708 (PI/2)
```
**Result:** Special angles converted to radians

**Explanation:** These are the "special angles" from trigonometry classes. Memorizing that 30° = PI/6, 45° = PI/4, etc., helps verify calculations.

---

### Example 12: Round-Trip Conversion
```
=DEGREES(RADIANS(45))
```
**Result:** 45

**Explanation:** RADIANS and DEGREES are inverses. Converting degrees to radians and back gives the original value (within floating-point precision).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text like "45 degrees" doesn't work. |
| `#NAME?` | Misspelled function name | Check spelling: RADIANS, not RADIAN or RAD. |
| `Wrong trig result` | Forgot to use RADIANS | Always use SIN(RADIANS(x)), COS(RADIANS(x)), TAN(RADIANS(x)) for degree inputs. |
| `Result seems very small` | Expected degrees, got radians | Remember that radians are smaller than degrees. 90 degrees = 1.57 radians. |
| `Double conversion` | Applied RADIANS to already-radian values | Don't convert twice. RADIANS(RADIANS(90)) gives 0.0274, which is wrong. |

## Use Cases

### [[Trigonometric Calculations with Degree Input]]

**Scenario:** Calculate sine, cosine, or tangent when you have angles in degrees.

**Implementation:**
```
=SIN(RADIANS(AngleDegrees))
=COS(RADIANS(AngleDegrees))
=TAN(RADIANS(AngleDegrees))
=SIN(RADIANS(A1)) * Amplitude        → Wave calculation
```

**Business Application:** Any trigonometric calculation with human-entered or standard-format angles—construction, surveying, physics, engineering.

**Technical Details:** This is the primary use of RADIANS. Human data is almost always in degrees; trig functions need radians. RADIANS bridges this gap.

---

### [[Circular Motion and Rotation]]

**Scenario:** Calculate positions or coordinates for circular paths defined in degrees.

**Implementation:**
```
X: =Radius * COS(RADIANS(AngleDegrees))
Y: =Radius * SIN(RADIANS(AngleDegrees))
=COS(RADIANS(RotationAngle)) * X - SIN(RADIANS(RotationAngle)) * Y  → Rotated X
```

**Business Application:** Animation, game development, robotics, CAD rotation, orbital mechanics, radar displays.

**Technical Details:** Circular paths and rotation matrices use trig functions internally. Convert degree specifications to radians before calculation, then the math follows standard formulas.

---

### [[Navigation and Bearing Calculations]]

**Scenario:** Calculate distances and directions using latitude/longitude or compass bearings.

**Implementation:**
```
=COS(RADIANS(Latitude1)) * COS(RADIANS(Latitude2))
=SIN(RADIANS(Bearing)) * Distance    → East/West component
=COS(RADIANS(Bearing)) * Distance    → North/South component
```

**Business Application:** GPS applications, maritime navigation, aviation, logistics route planning, geodesy.

**Technical Details:** Geographic coordinates are specified in degrees. All spherical trigonometry formulas require radians. Convert latitude, longitude, and bearings with RADIANS before calculations.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Conversion factor:** PI/180 (exact mathematical relationship)
- **Precision:** 15 significant digits
- **Input range:** Any numeric value
- **Performance:** Negligible overhead

### Google Sheets
- **Availability:** All versions
- **Conversion factor:** Same as Excel
- **Precision:** 15 significant digits
- **Input range:** Same as Excel
- **Performance:** Comparable

### Both Platforms
- Identical mathematical behavior
- Both accept any numeric input (no range restrictions)
- Both return #VALUE! for non-numeric input
- No functional differences between platforms

## Tips and Best Practices

1. **Always use RADIANS with SIN, COS, TAN:** Make `=SIN(RADIANS(x))` your automatic pattern. Never use SIN(x) directly when x is in degrees.

2. **Create a mental checklist:** Before using any trig function, ask: "Is my input in radians?" If not, add RADIANS().

3. **Document your units:** Add a comment or label indicating whether angles are in degrees or radians. This prevents confusion later.

4. **Use named ranges for clarity:** Name a cell "DegToRad" containing `=PI()/180`, then use `=SIN(angle * DegToRad)` for clearer formulas.

5. **Remember the scale:** Degrees are about 57 times larger than radians. If your result seems off by that factor, you forgot the conversion.

6. **Verify with known values:** SIN(RADIANS(30)) should equal 0.5. SIN(RADIANS(90)) should equal 1. Use these for sanity checks.

7. **Don't double-convert:** If values are already in radians (rare outside scientific contexts), don't apply RADIANS again.

8. **Combine with DEGREES for round-trip:** DEGREES(RADIANS(x)) should equal x. Use this to verify your formula structure.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DEGREES]] | Converts radians to degrees | When you need to display radian results in degrees (opposite direction) |
| [[PI]] | Returns the value of PI | For manual conversion: degrees * PI() / 180 |

### Commonly Used Together

**[[SIN]]** - Sine function requiring radian input

*Standard pattern:*
```
=SIN(RADIANS(45))    → Returns 0.707...
```
Convert degrees to radians for SIN input.

---

**[[COS]]** - Cosine function requiring radian input

*Standard pattern:*
```
=COS(RADIANS(60))    → Returns 0.5
```
Convert degrees to radians for COS input.

---

**[[TAN]]** - Tangent function requiring radian input

*Standard pattern:*
```
=TAN(RADIANS(45))    → Returns 1
```
Convert degrees to radians for TAN input.

---

**[[DEGREES]]** - Opposite conversion

*Round-trip verification:*
```
=DEGREES(RADIANS(90))    → Returns 90
```
RADIANS and DEGREES are inverse functions.

---

**[[PI]]** - For manual conversion

*Explicit formula:*
```
=SIN(45 * PI() / 180)    → Same as SIN(RADIANS(45))
```
Manual conversion when you want to show the math explicitly.

## Official Documentation

- **Microsoft Excel:** [RADIANS function](https://support.microsoft.com/en-us/office/radians-function-ac409508-3d48-45f5-ac02-1497c92de5bf)
- **Google Sheets:** [RADIANS function](https://support.google.com/docs/answer/3093437)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
