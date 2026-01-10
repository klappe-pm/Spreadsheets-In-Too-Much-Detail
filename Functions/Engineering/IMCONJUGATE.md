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
- complex-conjugate
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-conjugate
- conjugate-complex
tags:
- engineering-function
- complex-numbers
- conjugate
- reflection
---

# IMCONJUGATE

## Description

IMCONJUGATE returns the complex conjugate of a complex number. For a complex number z = a + bi, the complex conjugate (denoted z* or z-bar) is a - bi, which reflects the number across the real axis in the complex plane. The conjugate preserves the real part while negating the imaginary part, creating a mirror image below (or above) the real axis. This fundamental operation appears throughout electrical engineering, signal processing, and mathematical analysis.

The complex conjugate has profound mathematical and physical significance. A key identity is that multiplying a complex number by its conjugate yields a purely real result: z * conj(z) = |z|^2 = a^2 + b^2. This property is essential for rationalizing complex denominators, computing power in AC circuits, and calculating autocorrelation functions. In electrical engineering, the conjugate appears in apparent power calculations: S = V * I* (voltage times current conjugate), separating real power from reactive power.

The conjugate preserves magnitude while negating the argument: |conj(z)| = |z| and arg(conj(z)) = -arg(z). In polar form, if z = r*e^(i*theta), then conj(z) = r*e^(-i*theta). This symmetry makes conjugates essential in signal processing for creating real-valued signals from complex spectra (conjugate symmetry in FFT), designing matched filters (time-reversed conjugate of the signal), and implementing correlation operations.

Both Excel and Google Sheets implement IMCONJUGATE identically, accepting complex number strings with "i" or "j" notation and returning the conjugate as a text string in the same format. The function correctly handles purely real numbers (returns the same number since the conjugate of a real is itself), purely imaginary numbers (negates the coefficient), and zero (returns "0"). The output maintains the same imaginary unit suffix ("i" or "j") as the input, ensuring consistency in subsequent calculations.

## Syntax

> [!NOTE] Syntax
> ```
> IMCONJUGATE(inumber)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber** | A complex number in text format (e.g., "3+4i", "5-2j", "7i", "-3"). Can be a text string, cell reference containing a complex number string, or a formula that returns a complex number. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMCONJUGATE("3+4i")` | "3-4i" | Negates imaginary part: (3+4i)* = 3-4i |
| `=IMCONJUGATE("5-2j")` | "5+2j" | Negates negative imaginary: (5-2j)* = 5+2j |
| `=IMCONJUGATE("7i")` | "-7i" | Purely imaginary: (7i)* = -7i |
| `=IMCONJUGATE("-7i")` | "7i" | Double conjugate returns original |
| `=IMCONJUGATE("5")` | "5" | Real numbers are their own conjugate |
| `=IMCONJUGATE("-3")` | "-3" | Negative reals are also self-conjugate |
| `=IMCONJUGATE("0")` | "0" | Zero is its own conjugate |
| `=IMCONJUGATE("i")` | "-i" | Conjugate of i is -i |
| `=IMCONJUGATE(COMPLEX(6, 8))` | "6-8i" | Works with COMPLEX function output |
| `=IMPRODUCT("3+4i", IMCONJUGATE("3+4i"))` | "25" | z * z* = |z|^2 = 25 (purely real) |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure input follows "a+bi" or "a+bj" format with no spaces |
| `#VALUE!` | Input is a plain number, not text | Convert to text format or use COMPLEX function first |
| `#VALUE!` | Empty cell reference | Ensure referenced cell contains a valid complex number string |
| `#NUM!` | Spaces within the complex number string | Remove all spaces from the complex number text |
| `#NUM!` | Invalid characters in input | Ensure only digits, decimal points, +/- signs, and i/j are present |

## Use Cases

### 1. AC Power Calculation (Apparent, Real, and Reactive Power)

**Scenario**: An electrical engineer calculates apparent power (S), real power (P), and reactive power (Q) using the complex power formula S = V * I*, where I* is the complex conjugate of current.

**Implementation**: Use IMCONJUGATE to compute current conjugate, then multiply by voltage for complex power.

```
| Load | Voltage (V) | Current (I) | I* = IMCONJUGATE(I) | S = V * I* | P (Real) | Q (Reactive) |
|------|-------------|-------------|---------------------|------------|----------|--------------|
| Motor | 120+0j | 8-6j | 8+6j | 960+720j | 960 W | 720 VAR |
| Heater | 120+0j | 10+0j | 10+0j | 1200+0j | 1200 W | 0 VAR |
| Cap Bank | 120+0j | 0+5j | 0-5j | 0-600j | 0 W | -600 VAR |

S = V * conj(I) = P + jQ
Real Power P = Re(S) = watts dissipated
Reactive Power Q = Im(S) = VARs exchanged
Positive Q = inductive (absorbing), Negative Q = capacitive (supplying)
```

### 2. Matched Filter Implementation for Signal Detection

**Scenario**: A communications engineer implements a matched filter for optimal detection of a known signal in noise by correlating received data with the time-reversed conjugate of the transmitted waveform.

**Implementation**: Use IMCONJUGATE on reference signal samples to create the matched filter coefficients.

```
| Sample | TX Signal h[n] | Matched Filter h*[-n] | Formula |
|--------|----------------|----------------------|---------|
| 0 | 1+0i | =IMCONJUGATE(h[2]) | 0.5+0.5i |
| 1 | 0.707+0.707i | =IMCONJUGATE(h[1]) | 0.707-0.707i |
| 2 | 0.5-0.5i | =IMCONJUGATE(h[0]) | 1+0i |

Matched filter maximizes SNR at output
Correlation: sum of (received[n] * matched[n])
Conjugate ensures proper phase alignment for coherent detection
```

### 3. Reflection Coefficient and VSWR Calculation

**Scenario**: An RF engineer calculates the reflection coefficient and Voltage Standing Wave Ratio (VSWR) for transmission line impedance matching analysis.

**Implementation**: Use complex impedances and conjugate matching principles to minimize reflections.

```
| Port | Z_load | Z0 | Gamma = (ZL-Z0)/(ZL+Z0) | |Gamma| | VSWR |
|------|--------|-------|------------------------|--------|------|
| Ant 1 | 50+25j | 50 | =IMDIV(IMSUB(B2,C2),IMSUM(B2,C2)) | 0.243 | 1.64 |
| Ant 2 | 75+0j | 50 | =IMDIV(IMSUB(B3,C3),IMSUM(B3,C3)) | 0.2 | 1.5 |
| Ant 3 | 50+0j | 50 | =IMDIV(IMSUB(B4,C4),IMSUM(B4,C4)) | 0 | 1.0 |

For conjugate matching (maximum power transfer):
Z_load should equal conj(Z_source)
If Z_source = 50-25j, ideal Z_load = IMCONJUGATE("50-25j") = 50+25j
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Input format | Text string | Text string |
| Output format | Text string | Text string |
| Accepts "i" suffix | Yes | Yes |
| Accepts "j" suffix | Yes | Yes |
| Preserves suffix style | Yes (returns same as input) | Yes (returns same as input) |
| Purely real input | Returns input unchanged | Returns input unchanged |
| Zero input | Returns "0" | Returns "0" |

Both platforms implement IMCONJUGATE identically with no functional differences. The output format matches the input format, maintaining consistency with the chosen imaginary unit notation.

## Tips and Best Practices

1. **Verify with z * conj(z)**: A quick sanity check: `=IMPRODUCT(z, IMCONJUGATE(z))` should always yield a purely real positive result equal to IMABS(z)^2.

2. **Use for rationalization**: To simplify a/b where both are complex, multiply numerator and denominator by conj(b): `=IMDIV(IMPRODUCT(a, IMCONJUGATE(b)), IMPRODUCT(b, IMCONJUGATE(b)))`.

3. **Double conjugate identity**: Applying IMCONJUGATE twice returns the original: `=IMCONJUGATE(IMCONJUGATE(z))` equals z.

4. **Real numbers are self-conjugate**: For purely real inputs, IMCONJUGATE returns the same value. Use this to verify input format.

5. **Conjugate symmetry in FFT**: For real signals, FFT coefficients satisfy X[-k] = conj(X[k]). Use IMCONJUGATE to verify or reconstruct symmetric spectra.

6. **Power factor correction**: In power systems, calculate the required capacitance to conjugate-match a load: ideal compensation makes total impedance purely real.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMREAL]] | Extracts the real part (unchanged by conjugation) |
| [[IMAGINARY]] | Extracts imaginary part (negated by conjugation) |
| [[IMABS]] | Returns magnitude (same for z and conj(z)) |
| [[IMARGUMENT]] | Returns argument (negated for conjugate) |

### Commonly Used With

- **[[IMPRODUCT]]** - z * conj(z) = |z|^2 for power calculations
- **[[IMDIV]]** - Rationalize denominators using conjugate
- **[[IMSUM]]** - z + conj(z) = 2*Re(z) (doubles real part)
- **[[IMSUB]]** - z - conj(z) = 2i*Im(z) (doubles imaginary part times i)
- **[[IMABS]]** - Verify |z| = |conj(z)|
- **[[COMPLEX]]** - Create complex numbers to conjugate

## Official Documentation

- [Microsoft Excel IMCONJUGATE Documentation](https://support.microsoft.com/en-us/office/imconjugate-function-2e2fc1ea-f32b-4f9b-9de6-233853bafd42)
- [Google Sheets IMCONJUGATE Documentation](https://support.google.com/docs/answer/3093203)

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
