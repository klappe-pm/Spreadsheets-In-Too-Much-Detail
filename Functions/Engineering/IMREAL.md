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
- real-part-extraction
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- real-part
- complex-real-component
- extract-real
tags:
- engineering-function
- complex-numbers
- real-coefficient
- rectangular-form
---

# IMREAL

## Description

IMREAL extracts and returns the real coefficient from a complex number represented as a text string. Given a complex number in the form a + bi (or a + bj), IMREAL returns the value "a" as a real number. This function is essential for decomposing complex results back into their constituent parts, enabling further calculations that require only the real component. In the complex plane, the real part corresponds to the horizontal (x-axis) coordinate of the complex number.

The real part of a complex number carries distinct physical meaning depending on the application context. In electrical engineering, the real part of complex impedance represents resistance (measured in ohms), which dissipates power as heat. In signal processing, the real part of a Fourier transform coefficient represents the cosine component of a signal. In control systems, the real part of a pole determines the exponential growth or decay rate of system response. IMREAL allows engineers to isolate and analyze these physically meaningful quantities.

Understanding the relationship between rectangular and polar forms is crucial when working with IMREAL. A complex number z = a + bi can also be expressed in polar form as z = r(cos(theta) + i*sin(theta)), where r is the magnitude and theta is the argument. The real part "a" equals r*cos(theta). This relationship means that for a given magnitude, the real part is maximized when theta = 0 (positive real axis) and minimized when theta = pi (negative real axis). The real part equals zero when theta = pi/2 or -pi/2 (purely imaginary numbers).

Both Excel and Google Sheets implement IMREAL identically, accepting complex number strings with either "i" or "j" as the imaginary suffix. The function returns a numeric value (not text), enabling direct use in subsequent arithmetic operations. IMREAL correctly handles purely real numbers (returning the number itself), purely imaginary numbers (returning 0), and complex numbers with zero, positive, or negative components. The function is the inverse partner of IMAGINARY, and together they decompose any complex number into its rectangular coordinates.

## Syntax

> [!NOTE] Syntax
> ```
> IMREAL(inumber)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber** | A complex number in text format (e.g., "3+4i", "5-2j", "7i", "-3"). Can be a text string, cell reference containing a complex number string, or a formula that returns a complex number. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMREAL("3+4i")` | 3 | Extracts real part from 3 + 4i |
| `=IMREAL("5-2j")` | 5 | Works with "j" notation; returns real part 5 |
| `=IMREAL("7i")` | 0 | Purely imaginary number has zero real part |
| `=IMREAL("-3")` | -3 | Purely real number returns itself |
| `=IMREAL("0")` | 0 | Zero is both real and has zero real part |
| `=IMREAL("-2.5+3.7i")` | -2.5 | Handles negative and decimal real parts |
| `=IMREAL("i")` | 0 | Imaginary unit has zero real part |
| `=IMREAL(COMPLEX(6, 8))` | 6 | Works with COMPLEX function output |
| `=IMREAL("1E3+2E2i")` | 1000 | Scientific notation supported |
| `=IMREAL(IMSUM("1+2i", "3+4i"))` | 4 | Extracts real part after complex addition |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure input follows "a+bi" or "a+bj" format with no spaces |
| `#VALUE!` | Input is a plain number, not text | Convert to text format or use COMPLEX function first |
| `#VALUE!` | Empty cell reference | Ensure referenced cell contains a valid complex number string |
| `#NUM!` | Spaces within the complex number string | Remove all spaces from the complex number text |
| `#NUM!` | Mixed suffix notation (e.g., "3+4ij") | Use only "i" or only "j", not both |

## Use Cases

### 1. AC Power Factor Calculation

**Scenario**: An electrical engineer needs to calculate the power factor of an AC circuit by extracting the resistance component from measured complex impedance values.

**Implementation**: Use IMREAL to extract resistance from complex impedance, then calculate power factor as R/|Z|.

```
| Load | Complex Impedance (Z) | Formula for R | Resistance | |Z| | Power Factor |
|------|----------------------|---------------|------------|-----|--------------|
| Motor | 30+40j | =IMREAL(B2) | 30 | 50 | 0.6 |
| Heater | 100+0j | =IMREAL(B3) | 100 | 100 | 1.0 |
| Capacitor Bank | 0-50j | =IMREAL(B4) | 0 | 50 | 0.0 |

Power Factor = R / |Z| = IMREAL(Z) / IMABS(Z)
Active Power = |I|^2 * R = |I|^2 * IMREAL(Z)
```

### 2. Signal In-Phase Component Analysis

**Scenario**: A communications engineer analyzes the output of an IQ demodulator to extract the in-phase (I) component of a received signal for constellation diagram plotting.

**Implementation**: Extract the real part of complex baseband samples to isolate the in-phase component.

```
| Sample | Complex Value | Formula | I (Real) | Q (Imaginary) |
|--------|---------------|---------|----------|---------------|
| 1 | 0.707+0.707i | =IMREAL(B2) | 0.707 | 0.707 |
| 2 | -0.707+0.707i | =IMREAL(B3) | -0.707 | 0.707 |
| 3 | -0.707-0.707i | =IMREAL(B4) | -0.707 | -0.707 |
| 4 | 0.707-0.707i | =IMREAL(B5) | 0.707 | -0.707 |

QPSK constellation points at 45, 135, 225, 315 degrees
Each symbol encodes 2 bits based on quadrant
```

### 3. Stability Analysis of Control Systems

**Scenario**: A control systems engineer evaluates system stability by examining whether the real parts of all poles are negative (indicating stable, decaying responses).

**Implementation**: Extract real parts from complex pole locations to determine stability margins and decay rates.

```
| Pole | Complex Location | Formula | Real Part | Stability |
|------|------------------|---------|-----------|-----------|
| p1 | -2+5i | =IMREAL(B2) | -2 | Stable (decaying) |
| p2 | -2-5i | =IMREAL(B3) | -2 | Stable (decaying) |
| p3 | 0.5+3i | =IMREAL(B4) | 0.5 | UNSTABLE (growing) |

Stability criterion: All poles must have Real Part < 0
Time constant: tau = -1/Real(pole)
For p1: tau = -1/(-2) = 0.5 seconds
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Input format | Text string | Text string |
| Output format | Number | Number |
| Accepts "i" suffix | Yes | Yes |
| Accepts "j" suffix | Yes | Yes |
| Scientific notation in input | Supported | Supported |
| Maximum precision | 15 significant digits | 15 significant digits |
| Purely imaginary input | Returns 0 | Returns 0 |
| Purely real input | Returns the number | Returns the number |

Both platforms implement IMREAL identically with no functional differences. The function correctly handles all valid complex number formats and returns consistent numeric results.

## Tips and Best Practices

1. **Verify input format**: IMREAL expects a text string representation of a complex number. If working with separate real and imaginary values, first combine them using COMPLEX before extraction (though this is redundant for IMREAL).

2. **Chain with arithmetic operations**: Since IMREAL returns a number (not text), you can directly use it in calculations: `=IMREAL("3+4i") * 2` returns 6.

3. **Use for polar-to-rectangular verification**: After converting from polar form, verify: `Real part = r * COS(theta)` should match `IMREAL(complex_number)`.

4. **Handle arrays efficiently**: Apply IMREAL across a column of complex numbers to extract all real parts simultaneously for statistical analysis.

5. **Combine with IMAGINARY for complete decomposition**: Use both functions together to fully decompose complex results: Real = IMREAL(z), Imaginary = IMAGINARY(z).

6. **Check for stability in control systems**: Create a conditional formula: `=IF(IMREAL(pole)<0, "Stable", "Unstable")` to quickly assess pole stability.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMAGINARY]] | Extracts the imaginary coefficient from a complex number |
| [[IMABS]] | Returns the magnitude (modulus): sqrt(real^2 + imag^2) |
| [[IMARGUMENT]] | Returns the argument (angle) in radians |
| [[COMPLEX]] | Creates a complex number from real and imaginary parts |

### Commonly Used With

- **[[IMAGINARY]]** - Extract both components for complete decomposition
- **[[COMPLEX]]** - Create complex numbers to then decompose
- **[[IMABS]]** - Calculate magnitude alongside real part extraction
- **[[IMCONJUGATE]]** - The conjugate has the same real part
- **[[POWER]]** - Square the real part for power calculations
- **[[IF]]** - Conditional logic based on real part value (stability checks)

## Official Documentation

- [Microsoft Excel IMREAL Documentation](https://support.microsoft.com/en-us/office/imreal-function-d12bc4c0-25d0-4bb3-a25f-ece1938bf366)
- [Google Sheets IMREAL Documentation](https://support.google.com/docs/answer/3093214)

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
