---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
subTopics:
- complex-numbers
- imaginary-numbers
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- create-complex-number
- complex-number-constructor
tags:
- engineering-function
- complex-numbers
- imaginary-unit
- rectangular-form
---

# COMPLEX

## Description

COMPLEX constructs a complex number from its real and imaginary components, returning the result as a text string in rectangular form. Complex numbers are mathematical entities that extend the real number system by incorporating an imaginary unit, denoted as either "i" or "j", where i^2 = -1 (or equivalently j^2 = -1). This function is foundational for all complex number operations in spreadsheets, enabling engineers, scientists, and mathematicians to work with quantities that have both magnitude and phase components.

The rectangular form of a complex number is expressed as a + bi (or a + bj), where "a" is the real part and "b" is the imaginary coefficient. The choice between "i" and "j" notation is purely conventional: mathematicians and physicists typically use "i", while electrical engineers prefer "j" to avoid confusion with current (also denoted "i"). COMPLEX allows you to specify which suffix to use, ensuring compatibility with your field's conventions. When the imaginary coefficient is zero, the result is a purely real number; when the real part is zero, the result is purely imaginary.

Complex numbers are essential in numerous engineering and scientific applications because they elegantly represent quantities with two independent components. In electrical engineering, AC circuit analysis uses complex impedance (Z = R + jX) to combine resistance and reactance. In signal processing, complex exponentials and phasors simplify the analysis of sinusoidal signals. Control systems use complex poles and zeros to characterize system behavior. The COMPLEX function serves as the gateway to this rich mathematical framework within spreadsheet applications.

Both Excel and Google Sheets implement COMPLEX identically, returning a text string representation that can be used with other IM* functions. The function accepts positive, negative, or zero values for both components. When the imaginary part is 1 or -1, the function optimizes the output by omitting the "1" (returning "1+i" instead of "1+1i"). Understanding that COMPLEX returns text is crucial: you cannot perform standard arithmetic on the results, but must use dedicated functions like IMSUM, IMPRODUCT, and IMDIV.

## Syntax

> [!NOTE] Syntax
> ```
> COMPLEX(real_num, i_num, [suffix])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **real_num** | The real coefficient of the complex number. Can be any real number (positive, negative, or zero). | Yes |
| **i_num** | The imaginary coefficient of the complex number. Can be any real number (positive, negative, or zero). | Yes |
| **suffix** | The suffix for the imaginary component. Use "i" or "j" (case-sensitive). If omitted, defaults to "i". | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=COMPLEX(3, 4)` | "3+4i" | Creates 3 + 4i using default "i" suffix |
| `=COMPLEX(3, 4, "j")` | "3+4j" | Creates 3 + 4j for electrical engineering notation |
| `=COMPLEX(5, 0)` | "5" | Purely real number (imaginary part omitted when zero) |
| `=COMPLEX(0, 7)` | "7i" | Purely imaginary number (real part omitted when zero) |
| `=COMPLEX(-2, -3)` | "-2-3i" | Complex number in third quadrant |
| `=COMPLEX(0, 1)` | "i" | The imaginary unit itself |
| `=COMPLEX(0, -1)` | "-i" | Negative imaginary unit |
| `=COMPLEX(1.5, 2.75)` | "1.5+2.75i" | Decimal values fully supported |
| `=COMPLEX(0, 0)` | "0" | Zero is represented as real zero |
| `=COMPLEX(100, -50, "j")` | "100-50j" | Impedance notation: 100 ohms resistance, -50 ohms reactance |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric real_num or i_num argument | Ensure both components are numeric values, not text |
| `#VALUE!` | Invalid suffix (not "i" or "j") | Use exactly "i" or "j" (lowercase, case-sensitive) |
| `#VALUE!` | Suffix is uppercase "I" or "J" | Change to lowercase "i" or "j" |
| `#VALUE!` | Empty cell reference for required parameter | Ensure referenced cells contain numeric values |
| `#VALUE!` | Text that looks like a number (e.g., "3") | Remove quotes or use VALUE() to convert to number |

## Use Cases

### 1. AC Circuit Impedance Representation

**Scenario**: An electrical engineer needs to represent and calculate complex impedances for an RLC circuit analysis, where resistance is measured in ohms and reactance combines inductive and capacitive effects.

**Implementation**: Use COMPLEX to create impedance values from measured resistance and calculated reactance (XL - XC). The "j" suffix follows electrical engineering convention.

```
| Component | Resistance (R) | Reactance (X) | Formula | Impedance (Z) |
|-----------|----------------|---------------|---------|---------------|
| Resistor  | 100            | 0             | =COMPLEX(B2, C2, "j") | 100 |
| Inductor  | 0              | 314.16        | =COMPLEX(B3, C3, "j") | 314.16j |
| Capacitor | 0              | -106.1        | =COMPLEX(B4, C4, "j") | -106.1j |
| Series    | 100            | 208.06        | =COMPLEX(B5, C5, "j") | 100+208.06j |
```

### 2. Signal Processing Phasor Representation

**Scenario**: A communications engineer needs to represent sinusoidal signals as phasors for analyzing signal interference and combining multiple signal sources.

**Implementation**: Convert amplitude and phase measurements to rectangular form using trigonometric conversion, then use COMPLEX to create phasor notation.

```
Given: Amplitude = 5V, Phase = 30 degrees
Real part: =5*COS(RADIANS(30)) → 4.33
Imaginary part: =5*SIN(RADIANS(30)) → 2.5
Phasor: =COMPLEX(5*COS(RADIANS(30)), 5*SIN(RADIANS(30))) → "4.33+2.5i"

This represents the signal: v(t) = 5*cos(wt + 30 degrees)
```

### 3. Control Systems Pole-Zero Analysis

**Scenario**: A control systems engineer needs to define the poles and zeros of a transfer function to analyze system stability and frequency response.

**Implementation**: Create complex conjugate pole pairs that define second-order system dynamics. Poles with negative real parts indicate stable systems.

```
| Pole/Zero | Real Part | Imaginary Part | Formula | Complex Value |
|-----------|-----------|----------------|---------|---------------|
| Pole 1    | -2        | 5              | =COMPLEX(B2, C2) | -2+5i |
| Pole 2    | -2        | -5             | =COMPLEX(B3, C3) | -2-5i |
| Zero 1    | -1        | 0              | =COMPLEX(B4, C4) | -1 |

Conjugate pairs: Poles at -2 +/- 5i indicate a stable oscillatory system
Natural frequency: wn = IMABS(pole) = sqrt(4+25) = 5.39 rad/s
Damping ratio: zeta = -Re(pole)/wn = 2/5.39 = 0.37
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Default suffix | "i" | "i" |
| Accepted suffixes | "i", "j" (lowercase only) | "i", "j" (lowercase only) |
| Output format | Text string | Text string |
| Decimal precision | 15 significant digits | 15 significant digits |
| Zero imaginary handling | Omitted from output | Omitted from output |
| Zero real handling | Omitted from output | Omitted from output |
| Coefficient of 1 handling | "1" omitted (shows "i" not "1i") | "1" omitted (shows "i" not "1i") |

Both platforms implement COMPLEX identically with no functional differences. The function was part of the Analysis ToolPak in older Excel versions but is now built into the standard function library in modern versions.

## Tips and Best Practices

1. **Choose suffix consistently**: Pick either "i" or "j" and use it throughout your workbook. Mixing suffixes can cause errors in subsequent IM* function calculations.

2. **Convert from polar form**: To create a complex number from magnitude (r) and angle (theta in radians), use: `=COMPLEX(r*COS(theta), r*SIN(theta))`. For degrees, wrap theta in RADIANS().

3. **Understand the text output**: COMPLEX returns text, not a numeric type. Use IM* functions (IMSUM, IMPRODUCT, etc.) for arithmetic, not standard +, -, *, / operators.

4. **Handle special cases**: When both arguments are zero, the result is "0" (text). When creating arrays of complex numbers, be aware that empty cells will cause errors.

5. **Validate inputs before calculation**: Use IFERROR to gracefully handle cases where input cells might contain errors or non-numeric values.

6. **Link real and imaginary sources**: Often the real and imaginary parts come from separate measurements or calculations. Use cell references to create dynamic complex numbers that update when source data changes.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMREAL]] | Extracts the real coefficient from a complex number |
| [[IMAGINARY]] | Extracts the imaginary coefficient from a complex number |
| [[IMABS]] | Returns the magnitude (modulus) of a complex number |
| [[IMARGUMENT]] | Returns the argument (angle) of a complex number in radians |

### Commonly Used With

- **[[IMSUM]]** - Add multiple complex numbers created with COMPLEX
- **[[IMPRODUCT]]** - Multiply complex numbers for impedance calculations
- **[[IMDIV]]** - Divide complex numbers for transfer function analysis
- **[[IMCONJUGATE]]** - Find the complex conjugate for power calculations
- **[[IMABS]]** - Calculate magnitude from rectangular form created by COMPLEX
- **[[IMARGUMENT]]** - Calculate phase angle from rectangular form

## Official Documentation

- [Microsoft Excel COMPLEX Documentation](https://support.microsoft.com/en-us/office/complex-function-f0b8f3a9-51cc-4d6d-86fb-3a9362fa4b8c)
- [Google Sheets COMPLEX Documentation](https://support.google.com/docs/answer/3093192)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Available via Analysis ToolPak add-in |
| Excel 2007 | 2007 | Integrated into standard function library |
| Excel 2010 | 2010 | No changes |
| Excel 2013 | 2013 | No changes |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2006 | Available since launch |
