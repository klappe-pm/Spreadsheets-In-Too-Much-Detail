---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
- complex-numbers
- trigonometric-functions
subTopics:
- complex-trigonometry
- signal-processing
- wave-analysis
- electrical-engineering
- phasor-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-cosine
- imaginary-cosine
tags:
- engineering
- mathematics
- complex-numbers
- trigonometry
- phasors
---

# IMCOS

## IMCOS Description

**IMCOS** returns the cosine of a complex number in the form "x+yi" or "x+yj". For a complex number z = a + bi, the complex cosine is defined as cos(z) = cos(a)cosh(b) - i*sin(a)sinh(b), derived from Euler's formula and the relationship between trigonometric and hyperbolic functions. This function extends the familiar real cosine into the complex plane, where it exhibits fascinating behavior including the ability to exceed the bounds of -1 to 1 that constrain the real cosine.

The IMCOS function is essential in electrical engineering, signal processing, and physics applications involving oscillations with damping or growth. When analyzing AC circuits with complex impedances, computing wave propagation in lossy media, or evaluating transfer functions in control systems, IMCOS provides the complex cosine values needed for accurate calculations. The function is particularly important in phasor analysis, where signals are represented as complex exponentials, and their projections onto real axes involve complex trigonometric functions.

Key considerations when using IMCOS include understanding that the complex cosine is unbounded (unlike the real cosine which is restricted to [-1, 1]). As the imaginary part of the input grows, the magnitude of the result grows exponentially due to the cosh factor. The input must be provided as a complex number string in the format "a+bi" or "a+bj", or as a cell reference containing such a string. The function also accepts output from COMPLEX() or other IM functions. IMCOS is an entire function (analytic everywhere in the complex plane with no singularities).

Both Excel and Google Sheets implement IMCOS with identical syntax and results. The function accepts complex numbers in either "i" or "j" suffix notation and returns results using the "i" suffix by default. Both platforms handle edge cases such as purely real inputs (returning real cosine values as complex strings) and purely imaginary inputs (returning real values since cos(bi) = cosh(b)). Precision is approximately 15 significant digits on both platforms.

## Syntax

```
IMCOS(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the cosine. Can be in "x+yi" or "x+yj" format, or a cell reference. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMCOS("0")` | 1 | cos(0) = 1. Real input gives real result (as complex string "1"). |
| `=IMCOS("1")` | 0.540302305868... | cos(1) approximately equals 0.5403, the familiar real cosine. |
| `=IMCOS("1i")` | 1.54308063481524 | cos(i) = cosh(1) approximately equals 1.543. Purely imaginary input gives real result. |
| `=IMCOS("2i")` | 3.76219569108363 | cos(2i) = cosh(2) approximately equals 3.762. Grows exponentially with imaginary part. |
| `=IMCOS("1+1i")` | 0.833730025131149-0.988897705762865i | Complex cosine with both real and imaginary parts. |
| `=IMCOS("3.14159")` | -0.999999999999... | cos(pi) approximately equals -1. |
| `=IMCOS("1.5708")` | ~0 | cos(pi/2) approximately equals 0. |
| `=IMCOS("-1+2i")` | 2.03272300701967+3.0518977991518i | cos(-z) = cos(z), but this shows the full complex result. |
| `=IMCOS("0+3i")` | 10.0676619957778 | cos(3i) = cosh(3) approximately equals 10.068. Rapid exponential growth. |
| `=IMCOS(COMPLEX(2, 3))` | -4.18962569096881-9.10922789375534i | Using COMPLEX() to create input; large magnitude due to sinh(3). |
| `=IMCOS("0.5+0.5i")` | 0.989816905227429-0.24982639750382i | Moderate complex argument with small imaginary part. |
| `=IMCOS("2+0i")` | -0.416146836547142 | Purely real input in explicit complex format. |
| `=IMCOS("0-1i")` | 1.54308063481524 | cos(-i) = cosh(1) since cosine is an even function. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#VALUE!` | Empty string or non-numeric characters | Ensure input contains only numbers, +, -, and i or j |
| `#NUM!` | Complex number with extremely large imaginary part | cosh grows exponentially; limit imaginary parts to reasonable values |
| `#NAME?` | Function not recognized | Check spelling; ensure Analysis ToolPak is enabled in older Excel versions |
| `#VALUE!` | Multiple i or j suffixes | Use only one imaginary unit suffix per complex number |

## Use Cases

### [[AC Circuit Analysis]]
- **Implementation**: Calculate voltage/current relationships using `=IMCOS(COMPLEX(omega*t, -alpha*t))` for damped oscillations in RLC circuits
- **Business Application**: Design and analyze power systems, filters, and electronic circuits with accurate phase and magnitude calculations
- **Technical Details**: In lossy circuits, complex frequency s = sigma + j*omega leads to complex trigonometric arguments; IMCOS captures both oscillation and decay

### [[Signal Processing - Analytic Signal Construction]]
- **Implementation**: Compute components of analytic signals using `=IMCOS(COMPLEX(2*PI()*f*t, bandwidth_factor))` for frequency analysis
- **Business Application**: Develop audio processing algorithms, radar signal analysis, and communication system design
- **Technical Details**: Complex trigonometric functions relate to Fourier transforms of windowed signals; imaginary components represent bandwidth effects

### [[Wave Propagation in Lossy Media]]
- **Implementation**: Model electromagnetic waves using `=IMCOS(COMPLEX(beta*x, -alpha*x))` where beta is phase constant and alpha is attenuation
- **Business Application**: Design optical fibers, microwave transmission systems, and acoustic dampening materials
- **Technical Details**: Complex wavenumber k = beta - j*alpha produces exponentially decaying oscillations captured by IMCOS

### [[Conformal Mapping]]
- **Implementation**: Apply conformal transformations using `=IMCOS(z)` to map regions in complex analysis problems
- **Business Application**: Solve potential flow problems in aerodynamics, heat transfer, and electrostatics with complex geometries
- **Technical Details**: cos(z) maps the complex plane with period 2*pi in real direction; useful for periodic domain problems

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Real number input | Returns error | Returns error (must use "a+0i" format) |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |
| Overflow handling | #NUM! for extremely large results | #NUM! for extremely large results |

## Tips and Best Practices

1. **Format inputs correctly**: Always use "a+bi" format or COMPLEX(a, b). Real numbers alone will cause errors; use "3+0i" for real inputs.

2. **Understand magnitude growth**: |cos(a+bi)| grows approximately as cosh(b)/2 * e^|b| for large |b|. Be aware of potential overflow.

3. **Use Euler's formula**: cos(z) = (e^(iz) + e^(-iz))/2. This can be computed as `=IMDIV(IMSUM(IMEXP(IMPRODUCT("i",z)), IMEXP(IMPRODUCT("-i",z))), 2)` for verification.

4. **Even function property**: cos(-z) = cos(z). Use this to simplify calculations when appropriate.

5. **Periodicity**: cos(z + 2*pi) = cos(z). The function has real period 2*pi but no imaginary period.

6. **Zeros**: cos(z) = 0 when z = pi/2 + n*pi for any integer n. These are all on the real axis.

7. **Chain with other IM functions**: Build complex expressions like `=IMSUM(IMCOS(z), IMPRODUCT("i", IMSIN(z)))` for e^(iz) using Euler's formula.

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMSIN]] | Complex sine function - sin(z) |
| [[IMTAN]] | Complex tangent function - sin(z)/cos(z) |
| [[IMCOSH]] | Complex hyperbolic cosine - related by cosh(z) = cos(iz) |
| [[IMSEC]] | Complex secant function - 1/cos(z) |
| [[IMEXP]] | Complex exponential - fundamental to complex trig via Euler |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |
| [[IMREAL]] | Extracts real part of complex number |
| [[IMAGINARY]] | Extracts imaginary part of complex number |

## Official Documentation

- **Microsoft Excel**: [IMCOS function](https://support.microsoft.com/en-us/office/imcos-function-dad75277-f592-4a6b-ad6c-be93a808a53c)
- **Google Sheets**: [IMCOS function](https://support.google.com/docs/answer/9000388)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013-01 | Added as native function |
| Excel 2010 | 2010-06 | Not available |
| Google Sheets | 2018-01 | Added support for complex trigonometric functions |
| Excel 365 | 2020-01 | Dynamic array support added |
| Excel Online | 2013-06 | Full support in web version |

---

**Mathematical Background**: The complex cosine is defined via Euler's formula:

cos(z) = (e^(iz) + e^(-iz)) / 2

For z = a + bi, this expands to:
cos(a + bi) = cos(a)cosh(b) - i*sin(a)sinh(b)

Key properties:
- Even function: cos(-z) = cos(z)
- Periodic: cos(z + 2*pi*n) = cos(z) for integer n
- Derivative: d/dz cos(z) = -sin(z)
- Identity: cos^2(z) + sin^2(z) = 1 (Pythagorean identity extends to complex)
- Relationship: cos(iz) = cosh(z) (connects trig and hyperbolic functions)

The magnitude |cos(a+bi)| = sqrt(cos^2(a)cosh^2(b) + sin^2(a)sinh^2(b))
                          = sqrt((cos(2a) + cosh(2b))/2)
