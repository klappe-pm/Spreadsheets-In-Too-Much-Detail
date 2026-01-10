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
- argument-angle
- electrical-engineering
- signal-processing
- polar-form
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-argument
- complex-angle
- phase-angle
tags:
- engineering-function
- complex-numbers
- polar-form
- argument
- phase
---

# IMARGUMENT

## Description

IMARGUMENT calculates and returns the argument (angle or phase) of a complex number in radians. For a complex number z = a + bi, the argument theta is the angle measured counterclockwise from the positive real axis to the line connecting the origin to the point (a, b) in the complex plane. Mathematically, theta = atan2(b, a), where atan2 is the two-argument arctangent that correctly handles all four quadrants. The result ranges from -pi to +pi radians (-180 to +180 degrees).

The argument of a complex number represents phase information across numerous engineering applications. In electrical engineering, the argument of complex impedance indicates the phase difference between voltage and current: positive arguments mean current lags voltage (inductive), negative arguments mean current leads voltage (capacitive). In signal processing, the argument of Fourier coefficients encodes the phase shift of each frequency component relative to a reference. In control systems, phase margins and phase plots derive directly from transfer function arguments.

Understanding the polar form connection is essential: any complex number z can be expressed as z = |z| * e^(i*theta) = |z| * (cos(theta) + i*sin(theta)), where |z| is the magnitude (from IMABS) and theta is the argument (from IMARGUMENT). This polar representation simplifies multiplication (add arguments), division (subtract arguments), and exponentiation (multiply arguments) compared to rectangular form. Converting between rectangular and polar forms is fundamental to AC circuit analysis, phasor mathematics, and frequency domain techniques.

Both Excel and Google Sheets implement IMARGUMENT identically, returning the angle in radians as a real numeric value. The function handles all quadrant cases correctly, returning positive angles for the upper half-plane (positive imaginary) and negative angles for the lower half-plane (negative imaginary). For purely real positive numbers, the argument is 0; for purely real negative numbers, it is pi (or -pi). The special case of zero (0+0i) returns an error since the argument is undefined for the origin. To convert to degrees, multiply the result by 180/PI() or use the DEGREES function.

## Syntax

> [!NOTE] Syntax
> ```
> IMARGUMENT(inumber)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber** | A complex number in text format (e.g., "3+4i", "5-2j", "7i", "-3"). Can be a text string, cell reference containing a complex number string, or a formula that returns a complex number. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMARGUMENT("1+i")` | 0.7854 (pi/4) | 45 degrees: equal real and imaginary parts in quadrant I |
| `=IMARGUMENT("-1+i")` | 2.3562 (3*pi/4) | 135 degrees: quadrant II |
| `=IMARGUMENT("-1-i")` | -2.3562 (-3*pi/4) | -135 degrees (or 225): quadrant III |
| `=IMARGUMENT("1-i")` | -0.7854 (-pi/4) | -45 degrees (or 315): quadrant IV |
| `=IMARGUMENT("1")` | 0 | Positive real axis: angle is 0 |
| `=IMARGUMENT("-1")` | 3.1416 (pi) | Negative real axis: angle is pi (180 degrees) |
| `=IMARGUMENT("i")` | 1.5708 (pi/2) | Positive imaginary axis: 90 degrees |
| `=IMARGUMENT("-i")` | -1.5708 (-pi/2) | Negative imaginary axis: -90 degrees |
| `=IMARGUMENT("3+4i")` | 0.9273 | atan(4/3) ≈ 53.13 degrees |
| `=DEGREES(IMARGUMENT("3+4i"))` | 53.13 | Converted to degrees for readability |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input is zero ("0" or "0+0i") | Argument is undefined for zero; handle this case separately |
| `#NUM!` | Invalid complex number format | Ensure input follows "a+bi" or "a+bj" format with no spaces |
| `#VALUE!` | Input is a plain number, not text | Convert to text format or use COMPLEX function first |
| `#VALUE!` | Empty cell reference | Ensure referenced cell contains a valid complex number string |
| `#DIV/0!` | Division by zero in subsequent calculations using argument | Check for zero magnitude before using argument |

## Use Cases

### 1. AC Circuit Phase Angle Analysis

**Scenario**: An electrical engineer needs to determine the phase relationships between voltage and current in various circuit components to analyze power factor and reactive power flow.

**Implementation**: Use IMARGUMENT to extract phase angles from complex impedance values, indicating whether loads are inductive or capacitive.

```
| Component | Impedance (Z) | Formula | Phase (rad) | Phase (deg) | Type |
|-----------|---------------|---------|-------------|-------------|------|
| Resistor | 100+0j | =IMARGUMENT(B2) | 0 | 0 | Resistive |
| Inductor | 0+50j | =IMARGUMENT(B3) | 1.571 | 90 | Inductive |
| Capacitor | 0-75j | =IMARGUMENT(B4) | -1.571 | -90 | Capacitive |
| RLC Series | 30+40j | =IMARGUMENT(B5) | 0.927 | 53.1 | Inductive |

Phase > 0: Current lags voltage (inductive)
Phase < 0: Current leads voltage (capacitive)
Phase = 0: Current in phase (resistive)
Power factor = COS(phase angle)
```

### 2. Signal Phase Spectrum Analysis

**Scenario**: A communications engineer analyzes the phase response of a filter by extracting phase information from complex frequency response measurements at various test frequencies.

**Implementation**: Compute argument of frequency response to create phase plots and identify phase distortion.

```
| Frequency | H(jw) | Formula | Phase (rad) | Phase (deg) |
|-----------|-------|---------|-------------|-------------|
| 100 Hz | 0.98-0.2i | =IMARGUMENT(B2) | -0.202 | -11.6 |
| 1 kHz | 0.707-0.707i | =IMARGUMENT(B3) | -0.785 | -45 |
| 10 kHz | 0.1-0.995i | =IMARGUMENT(B4) | -1.471 | -84.3 |

Phase response plot: IMARGUMENT(H(jw)) vs frequency
-45 degrees at cutoff indicates first-order filter
Group delay = -d(phase)/d(frequency)
```

### 3. Phasor Angle Extraction for Power Systems

**Scenario**: A power systems engineer converts phasor measurements from synchrophasor (PMU) devices to determine angular differences between buses for stability monitoring.

**Implementation**: Extract phase angles from voltage and current phasors to compute power flow and detect oscillations.

```
| Bus | Voltage Phasor | Formula | Angle (rad) | Angle (deg) |
|-----|----------------|---------|-------------|-------------|
| 1 | 1.02+0.05i | =IMARGUMENT(B2) | 0.049 | 2.8 |
| 2 | 0.98-0.12i | =IMARGUMENT(B3) | -0.122 | -7.0 |
| 3 | 0.95+0.31i | =IMARGUMENT(B4) | 0.315 | 18.1 |

Angular difference Bus 1 to Bus 2: 2.8 - (-7.0) = 9.8 degrees
Power transfer: P = (|V1|*|V2|/X)*sin(delta)
Large angular differences indicate transmission stress
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Input format | Text string | Text string |
| Output format | Number (radians) | Number (radians) |
| Output range | -pi to +pi | -pi to +pi |
| Accepts "i" suffix | Yes | Yes |
| Accepts "j" suffix | Yes | Yes |
| Zero input handling | Returns #DIV/0! | Returns #DIV/0! |
| Purely real positive | Returns 0 | Returns 0 |
| Purely real negative | Returns pi | Returns pi |

Both platforms implement IMARGUMENT identically. The only notable behavior is that zero input produces an error since the argument of zero is mathematically undefined.

## Tips and Best Practices

1. **Convert to degrees when needed**: IMARGUMENT returns radians. Use `=DEGREES(IMARGUMENT(z))` or `=IMARGUMENT(z)*180/PI()` to convert to degrees for human readability.

2. **Handle the zero case**: Before calling IMARGUMENT, check if the complex number is zero: `=IF(z="0", "undefined", IMARGUMENT(z))` prevents errors.

3. **Understand the range**: Results are in (-pi, pi], not [0, 2*pi). To get positive angles only: `=IF(IMARGUMENT(z)<0, IMARGUMENT(z)+2*PI(), IMARGUMENT(z))`.

4. **Use for polar form conversion**: Combine IMABS and IMARGUMENT for complete rectangular-to-polar conversion: magnitude = IMABS(z), angle = IMARGUMENT(z).

5. **Verify quadrant correctness**: IMARGUMENT correctly handles all four quadrants. For "1+i", expect +45 degrees; for "-1-i", expect -135 degrees (not +225).

6. **Phase wrapping awareness**: When tracking phase changes over frequency, be aware of discontinuities at +/-pi where the angle wraps around.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMABS]] | Returns the magnitude (modulus) - the other polar component |
| [[IMREAL]] | Extracts the real coefficient (for atan2 calculation) |
| [[IMAGINARY]] | Extracts the imaginary coefficient (for atan2 calculation) |
| [[ATAN2]] | Two-argument arctangent for real numbers |

### Commonly Used With

- **[[IMABS]]** - Complete polar form: (IMABS, IMARGUMENT) = (r, theta)
- **[[DEGREES]]** - Convert radians to degrees for readability
- **[[COS]]** - Calculate power factor: COS(IMARGUMENT(Z))
- **[[SIN]]** - Calculate reactive component ratios
- **[[COMPLEX]]** - Create complex numbers from rectangular form
- **[[PI]]** - Reference value for angle comparisons

## Official Documentation

- [Microsoft Excel IMARGUMENT Documentation](https://support.microsoft.com/en-us/office/imargument-function-eed37ec1-23b3-4f59-b9f3-d340358a034a)
- [Google Sheets IMARGUMENT Documentation](https://support.google.com/docs/answer/3093201)

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
