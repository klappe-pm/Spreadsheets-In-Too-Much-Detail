---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- constant
- circle
- trigonometry
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Pi Constant
- Circle Constant
- 3.14159
tags:
- pi
- constant
- circle
- circumference
- trigonometry
---

# PI

## Description

**PI** returns the mathematical constant pi (approximately 3.14159265358979). This is the ratio of a circle's circumference to its diameter—a fundamental constant in mathematics that appears throughout geometry, trigonometry, and physics. PI() takes no arguments; it simply returns the most accurate value of pi that the spreadsheet can represent.

Pi is **irrational and transcendental**—its decimal representation never ends and never settles into a repeating pattern. Spreadsheets store approximately 15 significant digits: 3.14159265358979. For nearly all practical calculations, this precision far exceeds what's needed. Even NASA uses only 15 digits of pi for interplanetary navigation; for a circle the size of the observable universe, 39 digits would give accuracy to the width of a hydrogen atom.

**Why use PI() instead of typing 3.14159?** Using the function ensures maximum available precision and makes your formulas self-documenting. PI()*r^2 is immediately recognizable as a circle area formula. Furthermore, if you type 3.14, you lose significant precision; PI() always gives the best value the system can provide.

PI appears in formulas far beyond basic geometry. The normal distribution uses PI in its probability density formula. Trigonometric functions use radians (multiples of PI) for angles. Physics constants like h-bar (reduced Planck constant) involve PI. Anywhere circles, waves, oscillations, or rotations appear, PI is nearby.

## Syntax

> [!f(x)] PI Syntax
>
> ```
> =PI()
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| *(none)* | N/A | PI takes no arguments. The empty parentheses are required to call the function. |

### Return Value

Returns the value of pi to 15 significant digits: 3.14159265358979. This is the most precise value that can be represented in spreadsheet floating-point arithmetic.

## Examples

> [!f(x)] PI Examples

### Example 1: Get the Value of Pi
```
=PI()
```
**Result:** 3.14159265358979

**Explanation:** Returns pi to maximum available precision. No arguments needed.

---

### Example 2: Circle Area
```
=PI() * Radius^2
```
**Result:** Area of circle

**Explanation:** The formula A = pi*r^2. For radius 5: PI() * 25 = 78.54 square units.

---

### Example 3: Circle Circumference
```
=2 * PI() * Radius
```
**Result:** Circumference of circle

**Explanation:** The formula C = 2*pi*r. For radius 5: 2 * PI() * 5 = 31.42 units.

---

### Example 4: Sphere Volume
```
=(4/3) * PI() * Radius^3
```
**Result:** Volume of sphere

**Explanation:** V = (4/3)*pi*r^3. For radius 3: (4/3) * PI() * 27 = 113.1 cubic units.

---

### Example 5: Sphere Surface Area
```
=4 * PI() * Radius^2
```
**Result:** Surface area of sphere

**Explanation:** A = 4*pi*r^2. For radius 3: 4 * PI() * 9 = 113.1 square units.

---

### Example 6: Convert Degrees to Radians
```
=Degrees * PI() / 180
```
**Result:** Angle in radians

**Explanation:** Radians = degrees * pi/180. 90 degrees = 90 * PI() / 180 = 1.5708 radians (which is pi/2).

---

### Example 7: Convert Radians to Degrees
```
=Radians * 180 / PI()
```
**Result:** Angle in degrees

**Explanation:** Degrees = radians * 180/pi. PI()/2 radians = (PI()/2) * 180 / PI() = 90 degrees.

---

### Example 8: Normal Distribution Formula
```
=EXP(-POWER(x-Mean,2)/(2*Variance)) / SQRT(2*PI()*Variance)
```
**Result:** Normal probability density at x

**Explanation:** The bell curve formula includes PI in the denominator. This is the PDF of the normal distribution.

---

### Example 9: Cylinder Volume
```
=PI() * Radius^2 * Height
```
**Result:** Volume of cylinder

**Explanation:** V = pi*r^2*h. Base circle area times height. Radius 2, height 5: PI() * 4 * 5 = 62.83 cubic units.

---

### Example 10: Sine Wave Calculations
```
=Amplitude * SIN(2 * PI() * Frequency * Time)
```
**Result:** Sine wave value at given time

**Explanation:** Standard sine wave formula. 2*PI() gives one complete cycle per unit of Time when Frequency is 1.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NAME?` | Misspelled function or missing parentheses | Use PI() with parentheses. PI without () is not recognized. |
| `Low precision` | Typed approximation instead of function | Use PI() instead of 3.14 or even 3.14159 for maximum precision. |
| `Unexpected result` | Mixing radians and degrees | Trigonometric functions use radians. Convert with * PI()/180 or use RADIANS(). |
| `#VALUE!` | Arguments provided | PI() takes no arguments. Remove any values inside the parentheses. |

## Use Cases

### [[Geometry and Area/Volume Calculations]]

**Scenario:** Calculate dimensions of circles, spheres, cylinders, and other curved shapes.

**Implementation:**
```
=PI() * Radius^2                                   → Circle area
=2 * PI() * Radius                                 → Circle circumference
=(4/3) * PI() * Radius^3                           → Sphere volume
=PI() * Radius^2 * Height                          → Cylinder volume
=PI() * (OuterR^2 - InnerR^2)                      → Ring/annulus area
```

**Business Application:** Engineering, manufacturing, construction, product design. Any calculation involving circular or spherical shapes.

**Technical Details:** All circle-related formulas involve PI. Area scales with radius squared; volume scales with radius cubed. This is why small changes in radius have large effects on volume.

---

### [[Angle Conversion and Trigonometry]]

**Scenario:** Convert between degrees and radians or set up trigonometric calculations.

**Implementation:**
```
=Degrees * PI() / 180                              → To radians
=Radians * 180 / PI()                              → To degrees
=SIN(Angle * PI() / 180)                           → Sine of angle in degrees
=2 * PI()                                          → Full circle in radians
=PI() / 2                                          → Right angle in radians
```

**Business Application:** Navigation, surveying, physics simulations, graphics programming, any application using angles.

**Technical Details:** Spreadsheet trig functions (SIN, COS, TAN) expect radians. There are 2*PI radians in a full circle. RADIANS() and DEGREES() functions are shortcuts for these conversions.

---

### [[Statistical and Scientific Formulas]]

**Scenario:** Implement statistical distributions and physics formulas involving pi.

**Implementation:**
```
=1 / SQRT(2 * PI())                                → Standard normal coefficient
=EXP(-x^2/2) / SQRT(2*PI())                        → Standard normal PDF
=SQRT(2 * PI() * N) * POWER(N/EXP(1), N)          → Stirling approximation
=SQRT(PI())                                        → Gamma(0.5) = sqrt(pi)
```

**Business Application:** Quality control, risk analysis, scientific research, actuarial calculations.

**Technical Details:** PI appears in the normal distribution, gamma function, and many physics formulas. The Gaussian integral equals sqrt(PI), which is why PI appears in statistics.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Precision:** 15 significant digits
- **Value:** 3.14159265358979
- **Array support:** Returns single value (not array function)

### Google Sheets
- **Availability:** All versions
- **Precision:** Same 15 significant digits
- **Value:** Identical to Excel
- **Array support:** Same behavior

### Both Platforms
- Identical value and precision
- Same syntax (empty parentheses required)
- No platform-specific quirks
- Consistent with IEEE 754 floating-point standard

## Tips and Best Practices

1. **Always use PI() over typed values:** PI() gives 15 digits of precision. Typing 3.14159 gives only 6 digits. The function is always more precise.

2. **Remember the empty parentheses:** PI() requires () even though there are no arguments. PI alone returns #NAME! error.

3. **Know common multiples:** PI()/2 is 90 degrees (right angle), PI() is 180 degrees (straight line), 2*PI() is 360 degrees (full circle) in radians.

4. **Use RADIANS() and DEGREES() for clarity:** While Deg*PI()/180 works, RADIANS(Deg) is more readable. Same for DEGREES().

5. **PI is exact for spreadsheet purposes:** While pi is irrational, the 15-digit approximation is far more precise than any real-world measurement could possibly require.

6. **Recognize PI in formulas:** When you see denominators like SQRT(2*PI()), you're likely dealing with probability/statistics. This helps debug and verify formulas.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[EXP]](1) | Returns e (Euler's number) | When you need the base of natural logarithm |
| [[RADIANS]] | Converts degrees to radians | More readable than * PI()/180 |
| [[DEGREES]] | Converts radians to degrees | More readable than * 180/PI() |

### Commonly Used Together

**[[RADIANS]]** - Degree to radian conversion

*Equivalent operations:*
```
=RADIANS(90)      → 1.5708 (PI/2)
=90 * PI() / 180  → 1.5708 (PI/2)
```
RADIANS is cleaner for angle conversion.

---

**[[SIN]]/[[COS]]/[[TAN]]** - Trigonometric functions

*Use with PI for common angles:*
```
=SIN(PI()/2)  → 1 (sin 90 degrees)
=COS(PI())    → -1 (cos 180 degrees)
=TAN(PI()/4)  → 1 (tan 45 degrees)
```
Trig functions take radians, so PI() defines key angles.

---

**[[SQRT]]** - Square root

*Common PI combinations:*
```
=SQRT(PI())      → 1.7725 (gamma of 0.5)
=SQRT(2*PI())    → 2.5066 (normal distribution coefficient)
```
PI under square root appears in statistics.

---

**[[POWER]]** - Powers of radius

*Circle and sphere formulas:*
```
=PI() * POWER(Radius, 2)  → Circle area
=PI() * POWER(Radius, 3) * 4/3  → Sphere volume
```
Combine PI with radius powers for geometry.

---

**[[EXP]]** - Exponential function

*Normal distribution:*
```
=EXP(-x^2/(2*Var)) / SQRT(2*PI()*Var)
```
EXP and PI both appear in the normal distribution PDF.

## Official Documentation

- **Microsoft Excel:** [PI function](https://support.microsoft.com/en-us/office/pi-function-264199d0-a3ba-46b8-975a-c4a04608989b)
- **Google Sheets:** [PI function](https://support.google.com/docs/answer/3093432)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
