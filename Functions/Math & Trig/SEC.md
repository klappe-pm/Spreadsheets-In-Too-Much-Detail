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
- secant
- reciprocal-functions
- angles
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Secant
- Secant Function
- Reciprocal Cosine
tags:
- trigonometry
- secant
- angles
- radians
- engineering
- physics
---

# SEC

## Description

**SEC** returns the secant of an angle specified in radians. The secant is defined as the reciprocal of the cosine function: SEC(x) = 1/COS(x). While less commonly used than sine and cosine in everyday spreadsheet work, secant appears frequently in advanced mathematics, engineering calculations, physics problems, and certain financial models that involve trigonometric relationships.

The input angle must be in radians, not degrees. This trips up many users who think in degrees. To convert degrees to radians, multiply by PI()/180 or use the RADIANS function. For example, the secant of 60 degrees would be `=SEC(RADIANS(60))` or `=SEC(60*PI()/180)`, not `=SEC(60)`. Since SEC(60 radians) is a completely different value than SEC(60 degrees), getting this wrong produces results that look plausible but are entirely incorrect.

**Critical constraint:** SEC is undefined when COS equals zero, which occurs at odd multiples of pi/2 (90 degrees, 270 degrees, etc.). At these points, the function approaches positive or negative infinity. Excel returns a #DIV/0! error when you attempt to calculate SEC at these angles. In practice, due to floating-point precision, you might get extremely large numbers near these points rather than exact errors, so be careful when working near the asymptotes.

**Platform note:** SEC was introduced in Excel 2013 as part of Microsoft's effort to add more comprehensive trigonometric functions. Earlier Excel versions required the workaround `=1/COS(angle)`. Google Sheets has supported SEC since its inception, matching Excel 2013+ behavior exactly. If you're sharing workbooks with users on older Excel versions, consider using the 1/COS equivalent for backward compatibility.

## Syntax

> [!f(x)] SEC Syntax
>
> ```
> =SEC(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The angle in radians for which you want the secant. Can be a literal number, cell reference, or formula result. Must not be an odd multiple of PI()/2 where cosine equals zero. |

### Return Value

Returns a numeric value representing the secant of the input angle. The result is always greater than or equal to 1, or less than or equal to -1 (there are no secant values between -1 and 1). Returns #DIV/0! error when the angle is an odd multiple of PI()/2. Returns #VALUE! if the input is non-numeric.

## Examples

> [!f(x)] SEC Examples

### Example 1: Secant of Zero
```
=SEC(0)
```
**Result:** 1

**Explanation:** The secant of 0 radians equals 1 because COS(0) = 1, and 1/1 = 1. This is one of the most fundamental trigonometric identities and a good sanity check for the function.

---

### Example 2: Secant of PI/4 (45 Degrees)
```
=SEC(PI()/4)
```
**Result:** 1.414213562... (approximately square root of 2)

**Explanation:** At 45 degrees (PI/4 radians), COS = 1/sqrt(2), so SEC = sqrt(2). This is the well-known value at the 45-degree angle where sine and cosine are equal.

---

### Example 3: Secant of PI/3 (60 Degrees)
```
=SEC(PI()/3)
```
**Result:** 2

**Explanation:** The cosine of 60 degrees (PI/3 radians) is 0.5, so the secant is 1/0.5 = 2. This exact integer result is useful for verifying the function works correctly.

---

### Example 4: Converting from Degrees
```
=SEC(RADIANS(60))
```
**Result:** 2

**Explanation:** When you have an angle in degrees, wrap it in RADIANS() before passing to SEC. This is equivalent to SEC(PI()/3) and returns 2.

---

### Example 5: Secant of Negative Angle
```
=SEC(-PI()/3)
```
**Result:** 2

**Explanation:** Secant is an even function, meaning SEC(-x) = SEC(x). The secant of -60 degrees equals the secant of 60 degrees. This symmetry property is useful in calculations involving oscillating or symmetric phenomena.

---

### Example 6: Near the Asymptote (Approaching 90 Degrees)
```
=SEC(PI()/2 - 0.0001)
```
**Result:** Approximately 10000

**Explanation:** As the angle approaches PI/2 (90 degrees), secant approaches infinity. This very large result demonstrates the asymptotic behavior. At exactly PI()/2, the function would return #DIV/0!.

---

### Example 7: At the Asymptote (Exactly 90 Degrees)
```
=SEC(PI()/2)
```
**Result:** #DIV/0!

**Explanation:** SEC is undefined at 90 degrees because COS(90 degrees) = 0, and division by zero is undefined. This is a critical edge case to handle in your formulas.

---

### Example 8: Secant of PI (180 Degrees)
```
=SEC(PI())
```
**Result:** -1

**Explanation:** The cosine of 180 degrees is -1, so the secant is 1/(-1) = -1. This is another exact value useful for verification.

---

### Example 9: Full Rotation (2*PI)
```
=SEC(2*PI())
```
**Result:** 1

**Explanation:** After a full rotation (360 degrees or 2*PI radians), secant returns to its starting value. SEC(2*PI) = SEC(0) = 1, demonstrating the periodic nature of the function.

---

### Example 10: Using Cell Reference
```
=SEC(A1)
```
Where A1 contains 0.785398 (approximately PI/4)

**Result:** Approximately 1.414

**Explanation:** SEC works with cell references just like any other function. This allows you to calculate secant for values generated by other calculations or entered by users.

---

### Example 11: Secant with Manual Degree Conversion
```
=SEC(45*PI()/180)
```
**Result:** 1.414213562...

**Explanation:** An alternative to RADIANS() is multiplying degrees by PI()/180. Both methods work; RADIANS() is more readable, while this explicit formula shows the mathematics clearly.

---

### Example 12: Array of Angles (Modern Excel/Sheets)
```
=SEC({0, PI()/6, PI()/4, PI()/3})
```
**Result:** {1, 1.1547, 1.4142, 2}

**Explanation:** SEC can operate on arrays, returning secant values for multiple angles at once. This is useful for generating lookup tables or analyzing multiple angles simultaneously.

---

### Example 13: Combining with Other Trig Functions
```
=SEC(A1)^2 - TAN(A1)^2
```
**Result:** 1 (for any valid angle)

**Explanation:** This demonstrates the Pythagorean identity: sec^2(x) - tan^2(x) = 1. This identity holds for all angles where both functions are defined.

---

### Example 14: Engineering Wave Calculation
```
=SEC(2*PI()*frequency*time + phase)
```
**Result:** Secant of the wave phase angle

**Explanation:** In wave mechanics and signal processing, secant can appear in expressions describing wave behavior, particularly in contexts involving hyperbolic functions and wave impedance.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Angle is an odd multiple of PI/2 (90, 270 degrees, etc.) | Check for these values before calculation. Use IFERROR to handle gracefully: `=IFERROR(SEC(A1), "Undefined")` |
| `#VALUE!` | Non-numeric input | Ensure the input is a number. Text values like "45" won't work unless converted. |
| `#NAME?` | Function not recognized (Excel 2010 or earlier) | Use `=1/COS(angle)` as equivalent formula. Or upgrade to Excel 2013+. |
| `Wrong result` | Input in degrees instead of radians | Convert using RADIANS(): `=SEC(RADIANS(degrees))` |
| `Unexpectedly large result` | Angle very close to PI/2 | This is correct behavior near asymptotes. Consider whether your angle calculation has rounding issues. |
| `Unexpected negative result` | Angle in second or third quadrant | Secant is negative when cosine is negative (between 90-270 degrees). This is mathematically correct. |

## Use Cases

### [[Electrical Engineering - Impedance Calculations]]

**Scenario:** Calculate the impedance magnitude in AC circuits where phase angle relationships involve secant functions.

**Implementation:**
```
=Voltage * SEC(RADIANS(PhaseAngle))
=Z0 * SEC(ElectricalLength * PI() / 180)
```

**Business Application:** Electrical engineers designing transmission lines, antenna systems, and power distribution networks need secant calculations for impedance matching and power factor correction.

**Technical Details:** In transmission line theory, the input impedance of a lossless line involves secant functions of the electrical length. The secant arises naturally from the ratio of voltage to current in standing wave analysis.

---

### [[Physics - Optics and Refraction]]

**Scenario:** Model light behavior in optical systems where angle-dependent effects involve secant relationships.

**Implementation:**
```
=PathLength * SEC(RADIANS(IncidenceAngle))
=BasePower / SEC(RADIANS(TiltAngle))
```

**Business Application:** Optical system design, solar panel efficiency modeling, camera lens calculations, and atmospheric correction in satellite imagery.

**Technical Details:** When light travels through a medium at an angle, the actual path length is the thickness times the secant of the angle. Solar panel output decreases by the secant of the angle from normal incidence.

---

### [[Navigation and Cartography]]

**Scenario:** Perform map projection calculations using the Mercator projection or similar conformal mappings.

**Implementation:**
```
=EarthRadius * LN(TAN(PI()/4 + Latitude/2))
=Scale * SEC(RADIANS(Latitude))
```

**Business Application:** GIS systems, navigation software, maritime charts, and any application involving map projections that preserve angles.

**Technical Details:** The Mercator projection's scale factor at any latitude is the secant of that latitude. This is why Greenland appears so large on Mercator maps - the secant of high latitudes is very large.

---

### [[Structural Engineering - Load Analysis]]

**Scenario:** Calculate force components in structural members at various angles.

**Implementation:**
```
=VerticalLoad * SEC(RADIANS(RoofAngle))
=NormalForce / SEC(RADIANS(InclineAngle))
```

**Business Application:** Roof truss design, bridge engineering, scaffolding calculations, and any structure where forces act at angles to the horizontal.

**Technical Details:** When a load acts on an inclined surface, the force along the surface member equals the vertical load times the secant of the incline angle from vertical.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later only
- **Legacy alternative:** Use `=1/COS(angle)` in Excel 2010 and earlier
- **Precision:** 15 significant digits
- **Array support:** Yes, with Ctrl+Shift+Enter in older versions, automatic in Excel 365
- **Error at asymptotes:** Returns #DIV/0!

### Google Sheets
- **Availability:** All versions since launch
- **Syntax:** Identical to Excel
- **Precision:** 15 significant digits
- **Array support:** Native support without special entry
- **Error at asymptotes:** Returns #DIV/0!

### Key Differences
- Excel 2010 and earlier users need the 1/COS workaround
- Both platforms handle very large results near asymptotes similarly (may show extremely large numbers rather than infinity)
- Both return #DIV/0! at exact asymptote points
- No localization differences - function name is SEC in all language versions

## Tips and Best Practices

1. **Always convert degrees to radians:** This is the most common error. Use `=SEC(RADIANS(degrees))` when your source data is in degrees. Consider adding a helper column that converts degrees to radians first.

2. **Handle asymptotes explicitly:** Wrap SEC in IFERROR when processing data that might include 90-degree or 270-degree angles: `=IFERROR(SEC(A1), "")` prevents #DIV/0! errors from breaking your spreadsheet.

3. **Use 1/COS for backward compatibility:** If your workbook might be opened in Excel 2010 or earlier, use `=1/COS(angle)` instead of SEC. It's mathematically identical and works everywhere.

4. **Verify with known values:** Test your formula with SEC(0)=1, SEC(PI/3)=2, SEC(PI)=-1 to confirm your angles are in the correct units.

5. **Remember the range:** SEC never returns values between -1 and 1. If you get such a value, something is wrong with your formula or input.

6. **Consider the periodicity:** SEC has period 2*PI, meaning SEC(x) = SEC(x + 2*PI). This can simplify calculations by reducing angles to the principal range.

7. **Use ROUND for display:** SEC often produces long decimal results. Use ROUND(SEC(angle), 4) for cleaner display while keeping full precision in underlying calculations.

8. **Combine with error checking for robustness:**
   ```
   =IF(MOD(angle, PI()/2) = 0, "Undefined", SEC(angle))
   ```
   This pre-checks for asymptote conditions before calculating.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COS]] | Returns the cosine of an angle | When you need cosine directly, or for SEC equivalent: 1/COS(x) |
| [[CSC]] | Returns the cosecant (1/SIN) | When you need the reciprocal of sine instead of cosine |
| [[COT]] | Returns the cotangent (1/TAN) | When you need the reciprocal of tangent |
| [[SECH]] | Returns the hyperbolic secant | When working with hyperbolic rather than circular functions |
| [[ACOS]] | Returns the arccosine (inverse cosine) | When you have a cosine value and need the angle |

### Commonly Used Together

**[[RADIANS]]** - Convert degrees to radians

*Essential companion for degree inputs:*
```
=SEC(RADIANS(A1))
```
Converts degree value in A1 to radians before calculating secant.

---

**[[COS]]** - Cosine function

*Related through reciprocal identity:*
```
=1/COS(A1)  (equivalent to SEC(A1))
```
Useful for backward compatibility or understanding the relationship.

---

**[[TAN]]** - Tangent function

*Combined in trigonometric identities:*
```
=SEC(A1)^2 - TAN(A1)^2  (always equals 1)
```
The Pythagorean identity relates secant and tangent.

---

**[[IFERROR]]** - Error handling

*Handle asymptote errors gracefully:*
```
=IFERROR(SEC(A1), "Undefined")
```
Returns custom text instead of #DIV/0! at undefined angles.

---

**[[PI]]** - Pi constant

*Define angles as fractions of pi:*
```
=SEC(PI()/4)  (secant of 45 degrees)
```
More precise than decimal approximations for special angles.

## Official Documentation

- **Microsoft Excel:** [SEC function](https://support.microsoft.com/en-us/office/sec-function-ff224717-9c87-4170-9b58-d069ced6d5f7)
- **Google Sheets:** [SEC function](https://support.google.com/docs/answer/9116510)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Added as part of expanded trigonometric function set |
| Excel Online | Launch | Supported from initial release |
| Google Sheets | Original launch | Available from the beginning |
| LibreOffice Calc | 4.0+ | Compatible implementation |

---

*Last updated: 2026-01-10*
