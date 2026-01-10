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
- magnitude-modulus
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-magnitude
- complex-modulus
- absolute-value-complex
tags:
- engineering-function
- complex-numbers
- polar-form
- magnitude
---

# IMABS

## Description

IMABS calculates and returns the absolute value (magnitude or modulus) of a complex number. For a complex number z = a + bi, the magnitude |z| is calculated as sqrt(a^2 + b^2), representing the distance from the origin to the point (a, b) in the complex plane. This fundamental operation converts a complex number's rectangular coordinates into its radial distance, which is the first component of polar form representation. IMABS is essential for any calculation involving the "size" or "strength" of a complex quantity.

The magnitude of a complex number has profound physical significance across engineering disciplines. In electrical engineering, the magnitude of complex impedance |Z| determines the ratio of voltage to current amplitude in AC circuits, regardless of phase relationships. In signal processing, the magnitude of Fourier transform coefficients indicates the amplitude of each frequency component in a signal. In control systems, the magnitude of a transfer function at various frequencies defines the system's gain characteristics, forming the basis of Bode magnitude plots.

Mathematically, IMABS implements the Euclidean norm in the complex plane. The magnitude is always a non-negative real number, with |z| = 0 only when z = 0. Key properties include: |z1 * z2| = |z1| * |z2| (magnitudes multiply), |z1 / z2| = |z1| / |z2| (magnitudes divide), and |z| = |conj(z)| (conjugates have equal magnitude). The magnitude relates to the complex conjugate through the identity |z|^2 = z * conj(z), which is extensively used in power calculations and signal processing.

Both Excel and Google Sheets implement IMABS identically, accepting complex number strings with either "i" or "j" notation and returning a real numeric value. The function handles purely real numbers (returning their absolute value), purely imaginary numbers (returning the absolute coefficient), and the special case of zero (returning 0). Since IMABS returns a number rather than text, its result can be used directly in standard arithmetic operations without requiring specialized complex number functions.

## Syntax

> [!NOTE] Syntax
> ```
> IMABS(inumber)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber** | A complex number in text format (e.g., "3+4i", "5-2j", "7i", "-3"). Can be a text string, cell reference containing a complex number string, or a formula that returns a complex number. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMABS("3+4i")` | 5 | Classic 3-4-5 right triangle: sqrt(9+16) = 5 |
| `=IMABS("5-12j")` | 13 | Another Pythagorean triple: sqrt(25+144) = 13 |
| `=IMABS("1+i")` | 1.414... | sqrt(1+1) = sqrt(2) ≈ 1.414 |
| `=IMABS("5")` | 5 | Purely real number: magnitude equals absolute value |
| `=IMABS("-7")` | 7 | Negative real: magnitude is positive |
| `=IMABS("4i")` | 4 | Purely imaginary: magnitude equals |coefficient| |
| `=IMABS("-3i")` | 3 | Negative imaginary: magnitude is positive |
| `=IMABS("0")` | 0 | Zero has magnitude zero |
| `=IMABS(COMPLEX(6, 8))` | 10 | Works with COMPLEX output: sqrt(36+64) = 10 |
| `=IMABS("1E-10+1E-10i")` | 1.414E-10 | Scientific notation: sqrt(2)*1E-10 |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure input follows "a+bi" or "a+bj" format with no spaces |
| `#VALUE!` | Input is a plain number, not text | Convert to text format or use COMPLEX function first |
| `#VALUE!` | Empty cell reference | Ensure referenced cell contains a valid complex number string |
| `#NUM!` | Spaces within the complex number string | Remove all spaces from the complex number text |
| `#NUM!` | Multiple imaginary units (e.g., "3+4ii") | Use single "i" or "j" suffix |

## Use Cases

### 1. AC Circuit Impedance Magnitude and Current Flow

**Scenario**: An electrical engineer needs to calculate the current magnitude flowing through circuit elements given the source voltage and complex impedance values.

**Implementation**: Use IMABS to find impedance magnitude, then apply Ohm's law: |I| = |V| / |Z|.

```
| Component | Impedance (Z) | Formula for |Z| | |Z| (ohms) | Voltage | Current |
|-----------|---------------|-----------------|------------|---------|---------|
| Load 1 | 30+40j | =IMABS(B2) | 50 | 120V | 2.4A |
| Load 2 | 60-80j | =IMABS(B3) | 100 | 120V | 1.2A |
| Load 3 | 100+0j | =IMABS(B4) | 100 | 120V | 1.2A |

Ohm's Law for AC: |I| = |V| / |Z|
Current magnitude depends only on impedance magnitude
Phase angle affects timing, not amplitude
```

### 2. Signal Spectrum Power Analysis

**Scenario**: A communications engineer analyzes the power spectrum of a signal by computing the magnitude of FFT (Fast Fourier Transform) coefficients to identify dominant frequency components.

**Implementation**: Calculate magnitude of each complex FFT bin to create a power spectrum display.

```
| Freq Bin | FFT Coefficient | Formula | Magnitude | Power (dB) |
|----------|-----------------|---------|-----------|------------|
| 100 Hz | 0.5+0.866i | =IMABS(B2) | 1.0 | 0 dB |
| 200 Hz | 0.25+0.433i | =IMABS(B3) | 0.5 | -6 dB |
| 300 Hz | 0.1+0.173i | =IMABS(B4) | 0.2 | -14 dB |
| 400 Hz | 0.05+0.087i | =IMABS(B5) | 0.1 | -20 dB |

Power in dB: =20*LOG10(IMABS(fft_coeff))
Signal energy at frequency f is proportional to |FFT(f)|^2
```

### 3. Control System Gain Analysis

**Scenario**: A control systems engineer evaluates the frequency response of a transfer function by calculating the magnitude at various test frequencies for Bode plot construction.

**Implementation**: Evaluate transfer function at complex frequency points s = jw and compute magnitude.

```
| Frequency (rad/s) | s = jw | H(s) | Formula | |H(s)| | Gain (dB) |
|-------------------|--------|------|---------|--------|-----------|
| 1 | 1j | 0.98-0.2i | =IMABS(C2) | 1.0 | 0 dB |
| 10 | 10j | 0.5-0.5i | =IMABS(C3) | 0.707 | -3 dB |
| 100 | 100j | 0.01-0.1i | =IMABS(C4) | 0.1 | -20 dB |

Bode magnitude plot: |H(jw)| vs frequency
-3 dB point indicates cutoff frequency
Slope indicates filter order (-20 dB/decade per pole)
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Input format | Text string | Text string |
| Output format | Number | Number |
| Accepts "i" suffix | Yes | Yes |
| Accepts "j" suffix | Yes | Yes |
| Scientific notation | Supported | Supported |
| Maximum precision | 15 significant digits | 15 significant digits |
| Purely real input | Returns absolute value | Returns absolute value |
| Zero input | Returns 0 | Returns 0 |

Both platforms implement IMABS identically with no functional differences. The function uses the same Euclidean distance formula and returns consistent results across platforms.

## Tips and Best Practices

1. **Use for polar form conversion**: IMABS provides the "r" in polar form r*e^(i*theta). Combine with IMARGUMENT to get both polar components from rectangular form.

2. **Verify with manual calculation**: For simple cases, verify: IMABS("3+4i") should equal SQRT(3^2 + 4^2) = 5. This helps catch input formatting errors.

3. **Chain operations efficiently**: Since IMABS returns a number, use it directly in formulas: `=IMABS("3+4i")^2` returns 25 (the squared magnitude).

4. **Calculate power from magnitude**: In signal processing, power is proportional to magnitude squared: Power = |signal|^2 = IMABS(signal)^2.

5. **Use magnitude for comparisons**: When comparing complex number "sizes," use IMABS rather than comparing real and imaginary parts separately.

6. **Remember magnitude product rule**: |z1 * z2| = |z1| * |z2|. Use this to verify complex multiplication results or simplify calculations.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMARGUMENT]] | Returns the argument (angle) in radians - the other polar component |
| [[IMREAL]] | Extracts the real coefficient (horizontal component) |
| [[IMAGINARY]] | Extracts the imaginary coefficient (vertical component) |
| [[ABS]] | Absolute value for real numbers only |

### Commonly Used With

- **[[IMARGUMENT]]** - Complete polar form conversion with both magnitude and angle
- **[[COMPLEX]]** - Create complex numbers to then find magnitude
- **[[IMPRODUCT]]** - Magnitude of product equals product of magnitudes
- **[[IMDIV]]** - Magnitude of quotient equals quotient of magnitudes
- **[[SQRT]]** - Square root of squared magnitude equals magnitude
- **[[LOG10]]** - Convert magnitude to decibels: 20*LOG10(IMABS(z))

## Official Documentation

- [Microsoft Excel IMABS Documentation](https://support.microsoft.com/en-us/office/imabs-function-b31e73c6-d90c-4062-90bc-8eb351d765a1)
- [Google Sheets IMABS Documentation](https://support.google.com/docs/answer/3093200)

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
