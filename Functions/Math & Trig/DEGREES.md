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
- degrees
- radians
- trigonometry
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Convert Radians to Degrees
- Radian to Degree Conversion
tags:
- angle-conversion
- degrees
- radians
- trigonometry
- unit-conversion
---

# DEGREES

## Description

**DEGREES** converts an angle from radians to degrees. This is one of the most frequently needed functions when working with trigonometry in spreadsheets because all inverse trig functions (ASIN, ACOS, ATAN, ATAN2) return results in radians—a unit that most people find unintuitive. DEGREES(PI()) returns 180, DEGREES(PI()/2) returns 90, and DEGREES(1) returns approximately 57.3.

**Why do we need this function?** Radians are the "natural" unit for angles in mathematics—they make calculus work elegantly—but most people think in degrees. A full circle is 360 degrees (intuitive) or 2*PI radians (about 6.283—not intuitive). When ASIN(0.5) returns 0.5236, that's useless to most users; DEGREES(ASIN(0.5)) returns 30, which everyone understands. DEGREES bridges the gap between mathematical precision and human readability.

**The conversion formula is simple:** degrees = radians * (180 / PI). DEGREES just multiplies by the constant 180/PI (approximately 57.2957795). You could write `=A1 * 180 / PI()` instead of `=DEGREES(A1)`, but DEGREES is cleaner, more readable, and communicates intent clearly. The function accepts any numeric value—there's no restriction to "valid" angle ranges.

**Common workflow:** After using ASIN, ACOS, ATAN, or ATAN2 to find an angle, immediately wrap with DEGREES for human-readable output. `=DEGREES(ASIN(opposite/hypotenuse))` or `=DEGREES(ATAN2(x, y))` are standard patterns. This is so common that you should make it automatic habit: inverse trig function + DEGREES = readable angle.

## Syntax

> [!f(x)] DEGREES Syntax
>
> ```
> =DEGREES(angle)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `angle` | Yes | The angle in radians that you want to convert to degrees. Can be any numeric value—positive, negative, or zero. Typically the result of inverse trig functions (ASIN, ACOS, ATAN, ATAN2) or manual radian calculations. |

### Return Value

Returns a numeric value representing the angle in degrees. The conversion multiplies by 180/PI (approximately 57.2957795). There are no range restrictions on input or output—any number converts mathematically. Returns #VALUE! if input is non-numeric.

## Examples

> [!f(x)] DEGREES Examples

### Example 1: Convert PI to Degrees
```
=DEGREES(PI())
```
**Result:** 180

**Explanation:** PI radians = 180 degrees (half a circle). This is the fundamental relationship between radians and degrees, and a good sanity check for the function.

---

### Example 2: Convert PI/2 to Degrees
```
=DEGREES(PI()/2)
```
**Result:** 90

**Explanation:** PI/2 radians = 90 degrees (a quarter circle, a right angle). Another fundamental conversion to memorize.

---

### Example 3: Convert 1 Radian to Degrees
```
=DEGREES(1)
```
**Result:** 57.2957795...

**Explanation:** One radian equals approximately 57.3 degrees. A radian is the angle where the arc length equals the radius—about 1/6 of a circle.

---

### Example 4: Convert ASIN Result
```
=DEGREES(ASIN(0.5))
```
**Result:** 30

**Explanation:** ASIN(0.5) returns approximately 0.5236 radians. DEGREES converts this to 30 degrees—the angle whose sine is 0.5. This is the standard pattern for using inverse trig functions.

---

### Example 5: Convert ACOS Result
```
=DEGREES(ACOS(0.5))
```
**Result:** 60

**Explanation:** ACOS(0.5) returns approximately 1.047 radians. DEGREES converts to 60 degrees—the angle whose cosine is 0.5.

---

### Example 6: Convert ATAN Result
```
=DEGREES(ATAN(1))
```
**Result:** 45

**Explanation:** ATAN(1) finds the angle with slope 1, returning PI/4 radians. DEGREES converts to 45 degrees—where rise equals run.

---

### Example 7: Convert ATAN2 Result
```
=DEGREES(ATAN2(1, 1))
```
**Result:** 45

**Explanation:** ATAN2(1, 1) finds the angle to point (1, 1), returning PI/4 radians. DEGREES converts to the familiar 45 degrees.

---

### Example 8: Full Circle
```
=DEGREES(2*PI())
```
**Result:** 360

**Explanation:** 2*PI radians = 360 degrees = one complete rotation. This confirms the full circle relationship.

---

### Example 9: Negative Angle
```
=DEGREES(-PI()/4)
```
**Result:** -45

**Explanation:** Negative radians convert to negative degrees. -PI/4 radians = -45 degrees (45 degrees clockwise from the positive x-axis).

---

### Example 10: Small Angle
```
=DEGREES(0.1)
```
**Result:** 5.7295779513...

**Explanation:** 0.1 radians is a small angle, approximately 5.73 degrees. Useful for understanding the scale of radian measurements.

---

### Example 11: Complete Triangle Calculation
```
=DEGREES(ASIN(3/5))
```
**Result:** 36.8698976...

**Explanation:** In a 3-4-5 right triangle, the angle opposite the side of length 3 has sine = 3/5 = 0.6. DEGREES(ASIN(0.6)) gives approximately 36.87 degrees.

---

### Example 12: Multiple Conversions
```
=DEGREES(A1) + DEGREES(B1)
```
**Result:** Sum of two angles converted from radians

**Explanation:** Each radian value converts independently. You can add, subtract, or otherwise combine the degree results.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure the argument is a number. Text values cause this error. |
| `#NAME?` | Misspelled function name | Check spelling: DEGREES, not DEGREE or DEG. |
| `Result seems too large` | Input was already in degrees | DEGREES converts radians TO degrees. Don't apply to values already in degrees—you'll get nonsense (e.g., DEGREES(90) = 5157). |
| `Expected whole number` | Radians rarely convert to round degrees | Most radian values don't convert to nice round degrees. 1 radian = 57.3 degrees, not a whole number. |
| `Wrong direction conversion` | Confused with RADIANS | DEGREES converts radians to degrees. RADIANS does the opposite. Don't mix them up. |

## Use Cases

### [[Converting Inverse Trig Function Results]]

**Scenario:** Make the results of ASIN, ACOS, ATAN, and ATAN2 human-readable.

**Implementation:**
```
=DEGREES(ASIN(Opposite / Hypotenuse))
=DEGREES(ACOS(Adjacent / Hypotenuse))
=DEGREES(ATAN(Rise / Run))
=DEGREES(ATAN2(X, Y))
```

**Business Application:** Any trigonometric calculation where the final answer needs to be understood by humans—construction angles, navigation bearings, engineering specifications.

**Technical Details:** This is the primary use of DEGREES. Inverse trig functions return radians by design (for mathematical consistency), but humans need degrees. Make this conversion a habit.

---

### [[Angular Measurement Display]]

**Scenario:** Display calculated angles in degrees for reports, dashboards, or user interfaces.

**Implementation:**
```
=ROUND(DEGREES(AngleInRadians), 1) & " degrees"
=TEXT(DEGREES(A1), "0.0") & "°"
=IF(DEGREES(A1)>=0, DEGREES(A1) & "° CW", ABS(DEGREES(A1)) & "° CCW")
```

**Business Application:** Reports, dashboards, CAD applications, surveying outputs, navigation displays, physics simulations.

**Technical Details:** Combine DEGREES with TEXT or ROUND for clean presentation. Add degree symbols (°) with concatenation. Consider whether negative angles should display differently.

---

### [[Scientific and Engineering Calculations]]

**Scenario:** Convert mathematical results to standard engineering units.

**Implementation:**
```
=DEGREES(PhaseAngle)                    → Electrical phase in degrees
=DEGREES(AngularVelocity * Time)        → Rotation amount
=MOD(DEGREES(ATAN2(Vx, Vy)) + 360, 360) → Heading 0-360
```

**Business Application:** Electrical engineering (phase angles), mechanical engineering (rotation), aerospace (attitude angles), physics education.

**Technical Details:** Many engineering formulas produce radian results. DEGREES standardizes output to the conventional degree format used in specifications, standards, and communication.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Conversion factor:** 180/PI (exact mathematical relationship)
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

1. **Always wrap inverse trig functions:** Make `=DEGREES(ASIN(...))`, `=DEGREES(ACOS(...))`, `=DEGREES(ATAN(...))`, and `=DEGREES(ATAN2(...))` your default patterns.

2. **Don't double-convert:** If a value is already in degrees, applying DEGREES again gives nonsense. DEGREES(90) = 5157 degrees, which is meaningless.

3. **Combine with ROUND for clean output:** `=ROUND(DEGREES(A1), 2)` gives angles to 2 decimal places—often sufficient for practical purposes.

4. **Add degree symbol for clarity:** `=DEGREES(A1) & "°"` or `=TEXT(DEGREES(A1), "0.0°")` makes output unambiguous.

5. **Remember the conversion factor:** DEGREES multiplies by 180/PI (approximately 57.3). This helps you understand scale: 1 radian is about 57 degrees.

6. **Use for normalized angles:** To convert ATAN2 results to 0-360 range, use `=MOD(DEGREES(ATAN2(x,y))+360, 360)`.

7. **Verify with known values:** DEGREES(PI()) should equal 180. DEGREES(PI()/2) should equal 90. Use these for sanity checks.

8. **Pair with RADIANS for round-trip:** RADIANS(DEGREES(x)) should equal x (within floating-point precision). Use this to verify conversions.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[RADIANS]] | Converts degrees to radians | When you need to go the other direction—preparing input for trig functions |
| [[PI]] | Returns the value of PI | For manual conversion: radians * 180 / PI() |

### Commonly Used Together

**[[ASIN]]** - Inverse sine returning radians

*Standard conversion pattern:*
```
=DEGREES(ASIN(0.5))    → Returns 30
```
ASIN returns radians; DEGREES makes it readable.

---

**[[ACOS]]** - Inverse cosine returning radians

*Standard conversion pattern:*
```
=DEGREES(ACOS(0.5))    → Returns 60
```
ACOS returns radians; DEGREES makes it readable.

---

**[[ATAN]]** - Inverse tangent returning radians

*Standard conversion pattern:*
```
=DEGREES(ATAN(1))    → Returns 45
```
ATAN returns radians; DEGREES makes it readable.

---

**[[ATAN2]]** - Two-argument arctangent returning radians

*Standard conversion pattern:*
```
=DEGREES(ATAN2(1, 1))    → Returns 45
```
ATAN2 returns radians; DEGREES makes it readable.

---

**[[ROUND]]** - Clean up decimal places

*Presentable output:*
```
=ROUND(DEGREES(A1), 1)    → Angle to 1 decimal place
```
Most angles don't need many decimal places.

## Official Documentation

- **Microsoft Excel:** [DEGREES function](https://support.microsoft.com/en-us/office/degrees-function-4d6ec4db-e694-4b94-ace0-1cc3f61f9ba1)
- **Google Sheets:** [DEGREES function](https://support.google.com/docs/answer/3093481)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function, unchanged behavior |
| Excel 2007+ | All subsequent | No functional changes |
| Google Sheets | Original launch | Matches Excel behavior exactly |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
