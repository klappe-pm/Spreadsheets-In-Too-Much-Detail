---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
- complex-numbers
- hyperbolic-functions
subTopics:
- complex-hyperbolic
- signal-processing
- solitons
- pulse-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-hyperbolic-secant
- imaginary-sech
tags:
- engineering
- mathematics
- complex-numbers
- hyperbolic
- solitons
---

# IMSECH

## IMSECH Description

**IMSECH** returns the hyperbolic secant of a complex number in the form "x+yi" or "x+yj". The complex hyperbolic secant is defined as sech(z) = 1/cosh(z) = 2/(e^z + e^(-z)), the reciprocal of the complex hyperbolic cosine. For a complex number z = a + bi, sech can be expressed as sech(z) = [cosh(a)cos(b) - i*sinh(a)sin(b)] / [cosh^2(a)cos^2(b) + sinh^2(a)sin^2(b)].

The IMSECH function appears prominently in physics applications, particularly in the study of solitons (wave packets that maintain their shape) and pulse propagation in nonlinear media. The sech function describes the shape of optical solitons in fiber optics, the envelope of certain wave packets, and appears in solutions to the Korteweg-de Vries equation. In quantum mechanics, sech-shaped potentials (Poschl-Teller potentials) have exactly solvable bound states.

Critical considerations when using IMSECH include understanding that it has poles where cosh(z) = 0, which occurs at z = i*(pi/2 + n*pi) for all integers n. These poles are all on the imaginary axis. The function is even (sech(-z) = sech(z)) and decays exponentially for large real parts, with |sech(a+bi)| approaching 2*e^(-|a|) as |a| becomes large. For purely real inputs, sech is always positive and bounded by 0 < sech(x) <= 1.

Both Excel and Google Sheets implement IMSECH with consistent behavior. The function accepts complex numbers in "i" or "j" notation and returns results in "i" format. Poles are handled by returning errors when input is exactly at a singularity. Numerical precision is approximately 15 significant digits.

## Syntax

```
IMSECH(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the hyperbolic secant. Can be in "x+yi" or "x+yj" format, or a cell reference. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMSECH("0")` | 1 | sech(0) = 1/cosh(0) = 1. Maximum value for real arguments. |
| `=IMSECH("1")` | 0.648054273663885 | sech(1) = 1/cosh(1) approximately equals 0.648. |
| `=IMSECH("2")` | 0.265802228834079 | sech(2) = 1/cosh(2). Rapid decay. |
| `=IMSECH("1i")` | 1.85081571768093 | sech(i) = 1/cos(1) approximately equals 1.851. Note: sech(ib) = sec(b). |
| `=IMSECH("1+1i")` | 0.498337030555187-0.591083841721045i | Full complex result. |
| `=IMSECH("-1")` | 0.648054273663885 | sech(-1) = sech(1). Even function. |
| `=IMSECH("3")` | 0.0993279274194332 | sech(3) approximately equals 0.099. Small for large arguments. |
| `=IMSECH("0+1.5708i")` | ~large | sech(pi*i/2) = sec(pi/2) approaches pole. |
| `=IMSECH(COMPLEX(2, 1))` | -0.41314934426694-0.687527438655479i | Using COMPLEX() for input. |
| `=IMSECH("0.5+0.5i")` | 0.950439328502208-0.240566966567109i | Moderate complex argument. |
| `=IMSECH("5")` | 0.0134765058305891 | sech(5) very small due to exponential decay. |
| `=IMSECH("2+3i")` | -0.0416749644111443+0.0906111371962376i | Complex argument. |
| `=IMSECH("0+0i")` | 1 | sech(0) = 1. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#NUM!` | Input is at a pole (z = i*(pi/2 + n*pi)) | Avoid purely imaginary odd multiples of pi/2 |
| `#DIV/0!` | Division by zero when cosh(z) = 0 | Check for poles before computing |
| `#NAME?` | Function not recognized | Verify spelling; ensure Analysis ToolPak enabled in older Excel |
| `#VALUE!` | Non-complex input format | Format real numbers as "a+0i" |

## Use Cases

### [[Soliton Analysis]]
- **Implementation**: Model optical soliton pulses using `=A*IMSECH(COMPLEX(t/T0, chirp))` where T0 is pulse width
- **Business Application**: Design fiber optic communication systems, ultrafast lasers, and nonlinear optical devices
- **Technical Details**: Fundamental soliton has sech(t/T0) envelope; complex extension handles chirped pulses and gain/loss

### [[Pulse Shaping]]
- **Implementation**: Design sech-shaped pulses using `=IMSECH(COMPLEX(x/sigma, 0))` for pulse envelope design
- **Business Application**: Create radar pulses, ultrasound signals, and communication waveforms with specific spectral properties
- **Technical Details**: sech pulse has unique property that Fourier transform is also sech-shaped; ideal for minimum time-bandwidth product

### [[Quantum Well Potentials]]
- **Implementation**: Calculate bound states in Poschl-Teller potential using `=V0*IMSECH(COMPLEX(x/a, 0))^2`
- **Business Application**: Design quantum devices, semiconductor heterostructures, and model molecular potentials
- **Technical Details**: sech^2 potential has exactly solvable Schrodinger equation; eigenvalues depend on well depth

### [[Statistical Distributions]]
- **Implementation**: Evaluate hyperbolic secant distribution using `=IMSECH(COMPLEX(pi*x/(2*s), 0))/(2*s)` for scale s
- **Business Application**: Model data with heavier tails than normal distribution in financial and scientific applications
- **Technical Details**: Hyperbolic secant distribution is symmetric with kurtosis 2; complex extension for characteristic functions

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2013+ native | Added in 2018 |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Pole handling | #NUM! or #DIV/0! at poles | #NUM! or #DIV/0! at poles |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |

## Tips and Best Practices

1. **Poles on imaginary axis**: sech(z) has poles at z = i*(pi/2 + n*pi). Avoid these purely imaginary values.

2. **Even function property**: sech(-z) = sech(z). Exploit symmetry in calculations.

3. **Exponential decay**: For large |Re(z)|, |sech(z)| approximately equals 2*e^(-|Re(z)|). Useful for asymptotic analysis.

4. **Build from IMCOSH**: Verify results with `=IMDIV("1", IMCOSH(z))` for identical output.

5. **Relation to sec**: sech(z) = sec(iz). Convert between hyperbolic and trigonometric as needed.

6. **Maximum at origin**: For real arguments, sech(x) has maximum value 1 at x = 0.

7. **Soliton applications**: The fundamental soliton shape is sech(x); many nonlinear wave phenomena use this profile.

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMCOSH]] | Complex hyperbolic cosine - sech(z) = 1/cosh(z) |
| [[IMCSCH]] | Complex hyperbolic cosecant - 1/sinh(z) |
| [[IMSEC]] | Complex secant - related by sec(z) = sech(iz) |
| [[IMSINH]] | Complex hyperbolic sine - often paired with cosh |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |
| [[IMDIV]] | Complex division for building sech from cosh |

## Official Documentation

- **Microsoft Excel**: [IMSECH function](https://support.microsoft.com/en-us/office/imsech-function-f250304f-788b-4505-954e-eb01fa50903b)
- **Google Sheets**: [IMSECH function](https://support.google.com/docs/answer/9000353)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013-01 | Added as native function |
| Excel 2010 | 2010-06 | Not available |
| Google Sheets | 2018-01 | Added support for complex hyperbolic functions |
| Excel 365 | 2020-01 | Dynamic array support added |
| Excel Online | 2013-06 | Full support in web version |

---

**Mathematical Background**: The complex hyperbolic secant is defined as:

sech(z) = 1/cosh(z) = 2/(e^z + e^(-z))

For z = a + bi, the components are:
sech(a + bi) = [cosh(a)cos(b) - i*sinh(a)sin(b)] / |cosh(a + bi)|^2

where |cosh(a + bi)|^2 = sinh^2(a) + cos^2(b) = (cosh(2a) + cos(2b))/2

Key properties:
- Even function: sech(-z) = sech(z)
- Poles: z = i*(pi/2 + n*pi) for all integers n (on imaginary axis)
- No zeros (cosh never reaches infinity)
- Period in imaginary: 2*pi*i (sech(z + 2*pi*i) = sech(z))
- Maximum: sech(0) = 1 (for real arguments)
- Asymptotic: |sech(a + bi)| approximately equals 2*e^(-|a|) for large |a|
- Derivative: d/dz sech(z) = -sech(z)tanh(z)
- Relation: sech(iz) = sec(z)
- Fourier transform: F{sech(at)} = (pi/a)*sech(pi*omega/(2a))
