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
- imaginary-part-extraction
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- imaginary-part
- complex-imaginary-component
- extract-imaginary
tags:
- engineering-function
- complex-numbers
- imaginary-coefficient
- rectangular-form
---

# IMAGINARY

## Description

IMAGINARY extracts and returns the imaginary coefficient from a complex number represented as a text string. Given a complex number in the form a + bi (or a + bj), IMAGINARY returns the value "b" as a real number (without the "i" or "j" suffix). This function is essential for decomposing complex numbers into their constituent parts, enabling analysis of the component that exists perpendicular to the real number line. In the complex plane, the imaginary part corresponds to the vertical (y-axis) coordinate.

The imaginary part of a complex number represents physically meaningful quantities across engineering disciplines. In electrical engineering, the imaginary part of complex impedance represents reactance (measured in ohms), which stores and releases energy without dissipating it. Positive reactance indicates inductive behavior (current lags voltage), while negative reactance indicates capacitive behavior (current leads voltage). In signal processing, the imaginary part of a Fourier coefficient represents the sine component of a signal, capturing phase-shifted information.

The relationship between rectangular form (a + bi) and polar form (r * e^(i*theta)) provides insight into the imaginary component's role. For a complex number with magnitude r and argument theta, the imaginary part equals r * sin(theta). This means the imaginary part is maximized at theta = pi/2 (positive imaginary axis), minimized at theta = -pi/2 (negative imaginary axis), and equals zero when theta = 0 or pi (purely real numbers). The imaginary part thus encodes the "vertical" or "orthogonal" component of any complex quantity.

Both Excel and Google Sheets implement IMAGINARY identically, accepting complex number strings with either "i" or "j" as the imaginary unit suffix. The function returns a numeric value (not text), allowing direct use in subsequent calculations. IMAGINARY correctly handles purely real numbers (returning 0), purely imaginary numbers (returning the coefficient), and all combinations with positive, negative, or zero components. Together with IMREAL, IMAGINARY enables complete decomposition of any complex number into its rectangular coordinates.

## Syntax

> [!NOTE] Syntax
> ```
> IMAGINARY(inumber)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber** | A complex number in text format (e.g., "3+4i", "5-2j", "7i", "-3"). Can be a text string, cell reference containing a complex number string, or a formula that returns a complex number. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMAGINARY("3+4i")` | 4 | Extracts imaginary coefficient from 3 + 4i |
| `=IMAGINARY("5-2j")` | -2 | Works with "j" notation; returns -2 (negative) |
| `=IMAGINARY("7i")` | 7 | Purely imaginary number returns its coefficient |
| `=IMAGINARY("-3")` | 0 | Purely real number has zero imaginary part |
| `=IMAGINARY("0")` | 0 | Zero has no imaginary component |
| `=IMAGINARY("-2.5+3.7i")` | 3.7 | Handles decimal imaginary coefficients |
| `=IMAGINARY("i")` | 1 | Imaginary unit has coefficient 1 |
| `=IMAGINARY("-i")` | -1 | Negative imaginary unit has coefficient -1 |
| `=IMAGINARY(COMPLEX(6, 8))` | 8 | Works with COMPLEX function output |
| `=IMAGINARY(IMPRODUCT("1+2i", "3+4i"))` | 10 | Extracts imaginary part after multiplication: (1+2i)(3+4i) = -5+10i |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure input follows "a+bi" or "a+bj" format with no spaces |
| `#VALUE!` | Input is a plain number, not text | Convert to text format or use COMPLEX function first |
| `#VALUE!` | Empty cell reference | Ensure referenced cell contains a valid complex number string |
| `#NUM!` | Spaces within the complex number string | Remove all spaces from the complex number text |
| `#NUM!` | Invalid suffix (not "i" or "j") | Use standard "i" or "j" suffix only |

## Use Cases

### 1. Reactive Power Analysis in AC Circuits

**Scenario**: An electrical engineer needs to analyze reactive power consumption by extracting the reactance component from complex impedance measurements across various loads.

**Implementation**: Use IMAGINARY to extract reactance from complex impedance to calculate reactive power Q = I^2 * X.

```
| Load | Complex Impedance (Z) | Formula for X | Reactance (X) | Type |
|------|----------------------|---------------|---------------|------|
| Motor | 30+40j | =IMAGINARY(B2) | 40 | Inductive (lagging) |
| LED Driver | 50-25j | =IMAGINARY(B3) | -25 | Capacitive (leading) |
| Transformer | 10+75j | =IMAGINARY(B4) | 75 | Inductive (lagging) |

Reactive Power: Q = |I|^2 * X = |I|^2 * IMAGINARY(Z)
Positive X → Inductive load (absorbs VARs)
Negative X → Capacitive load (supplies VARs)
```

### 2. Quadrature Signal Component Extraction

**Scenario**: A communications engineer extracts the quadrature (Q) component from complex baseband signal samples for demodulation and symbol detection in a digital receiver.

**Implementation**: Extract imaginary parts to isolate the quadrature channel for QPSK/QAM demodulation.

```
| Sample | Complex Sample | Formula | Q Component | Symbol Decision |
|--------|----------------|---------|-------------|-----------------|
| 1 | 0.8+0.6i | =IMAGINARY(B2) | 0.6 | Upper half-plane |
| 2 | -0.9+0.4i | =IMAGINARY(B3) | 0.4 | Upper half-plane |
| 3 | -0.7-0.7i | =IMAGINARY(B4) | -0.7 | Lower half-plane |
| 4 | 0.5-0.85i | =IMAGINARY(B5) | -0.85 | Lower half-plane |

BPSK on Q channel: Sign of IMAGINARY determines bit value
Q > 0 → bit = 1; Q < 0 → bit = 0
```

### 3. Oscillation Frequency from Complex Poles

**Scenario**: A control systems engineer determines the natural oscillation frequency of a system by examining the imaginary parts of complex conjugate poles.

**Implementation**: Extract imaginary parts from pole locations to calculate damped oscillation frequency.

```
| Pole | Complex Location | Formula | Imaginary Part | Oscillation |
|------|------------------|---------|----------------|-------------|
| p1 | -2+10i | =IMAGINARY(B2) | 10 | 10 rad/s |
| p2 | -2-10i | =IMAGINARY(B3) | -10 | 10 rad/s |
| p3 | -5+0i | =IMAGINARY(B4) | 0 | No oscillation |

Damped frequency: wd = |Imaginary(pole)| = 10 rad/s
Frequency in Hz: f = wd / (2*PI) = 10 / 6.28 = 1.59 Hz
Period: T = 1/f = 0.63 seconds
Complex conjugate pairs always have equal magnitude imaginary parts
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
| Purely real input | Returns 0 | Returns 0 |
| Input "i" alone | Returns 1 | Returns 1 |
| Input "-i" alone | Returns -1 | Returns -1 |

Both platforms implement IMAGINARY identically with no functional differences. The function correctly parses all valid complex number formats and returns consistent numeric results.

## Tips and Best Practices

1. **Remember the coefficient, not the unit**: IMAGINARY returns only the numeric coefficient (e.g., 4 from "3+4i"), not "4i". The result is always a real number.

2. **Handle implicit coefficients**: For inputs like "i" or "-i", the function correctly returns 1 or -1 respectively, recognizing the implicit coefficient.

3. **Use for phase calculations**: Combine with IMREAL: `=ATAN2(IMREAL(z), IMAGINARY(z))` gives the same result as IMARGUMENT (with argument order matching ATAN2 convention).

4. **Verify polar conversions**: After converting from polar form, verify: `Imaginary part = r * SIN(theta)` should match `IMAGINARY(complex_number)`.

5. **Check reactance signs for circuit analysis**: Positive imaginary = inductive (XL = wL), negative imaginary = capacitive (XC = -1/(wC)). Use this to quickly identify load types.

6. **Combine with IMREAL for magnitude**: Calculate magnitude manually: `=SQRT(IMREAL(z)^2 + IMAGINARY(z)^2)` equals IMABS(z).

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMREAL]] | Extracts the real coefficient from a complex number |
| [[IMABS]] | Returns the magnitude (modulus): sqrt(real^2 + imag^2) |
| [[IMARGUMENT]] | Returns the argument (angle) in radians |
| [[COMPLEX]] | Creates a complex number from real and imaginary parts |

### Commonly Used With

- **[[IMREAL]]** - Extract both components for complete rectangular decomposition
- **[[COMPLEX]]** - Create complex numbers that can then be decomposed
- **[[IMABS]]** - Calculate magnitude from extracted components
- **[[IMARGUMENT]]** - Calculate angle using imaginary component
- **[[IMCONJUGATE]]** - Conjugate negates the imaginary part
- **[[ATAN2]]** - Calculate angle from real and imaginary parts

## Official Documentation

- [Microsoft Excel IMAGINARY Documentation](https://support.microsoft.com/en-us/office/imaginary-function-dd5952fd-473d-44d9-95a1-9a17b23e428a)
- [Google Sheets IMAGINARY Documentation](https://support.google.com/docs/answer/3093202)

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
