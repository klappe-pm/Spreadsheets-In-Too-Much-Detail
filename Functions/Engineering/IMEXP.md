---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
- complex-numbers
- exponential-functions
subTopics:
- euler-formula
- signal-processing
- phasor-analysis
- wave-analysis
- rotation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-exponential
- imaginary-exponential
- euler-formula-function
tags:
- engineering
- mathematics
- complex-numbers
- exponential
- phasors
- euler
---

# IMEXP

## IMEXP Description

**IMEXP** returns the exponential of a complex number in the form "x+yi" or "x+yj". For a complex number z = a + bi, the complex exponential is defined as e^z = e^a * (cos(b) + i*sin(b)), which is the celebrated Euler's formula when a = 0. This function is arguably the most important in complex analysis, connecting exponential growth/decay with rotation in the complex plane and forming the foundation for understanding waves, oscillations, and periodic phenomena.

The IMEXP function is fundamental across virtually all engineering and scientific disciplines. In electrical engineering, it represents phasors for AC circuit analysis where e^(jwt) encodes both frequency and phase. In signal processing, it forms the basis of the Fourier transform and spectral analysis. In quantum mechanics, complex exponentials describe wave functions and probability amplitudes. In control systems, system responses are characterized by poles that correspond to complex exponentials in the time domain.

Key considerations when using IMEXP include understanding that e^(a+bi) separates into a magnitude factor e^a and a rotation factor e^(ib) = cos(b) + i*sin(b). The magnitude |e^z| = e^(Re(z)), which can overflow for large positive real parts (approximately Re(z) > 709) and underflow for large negative real parts. The argument (phase angle) of e^z equals the imaginary part b (modulo 2*pi). The function is entire (analytic everywhere) with no zeros, though it approaches 0 as Re(z) approaches negative infinity.

Both Excel and Google Sheets implement IMEXP with identical behavior and precision. The function accepts complex numbers in "i" or "j" notation and returns results in "i" format. For purely imaginary inputs, IMEXP directly computes Euler's formula, returning values on the unit circle. For purely real inputs, it returns the standard exponential as a complex string. Precision is approximately 15 significant digits on both platforms.

## Syntax

```
IMEXP(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the exponential. Can be in "x+yi" or "x+yj" format, or a cell reference. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMEXP("0")` | 1 | e^0 = 1. The exponential of zero is one. |
| `=IMEXP("1")` | 2.71828182845905 | e^1 approximately equals 2.718, Euler's number. |
| `=IMEXP("1i")` | 0.540302305868+0.841470984808i | e^i = cos(1) + i*sin(1). Euler's formula. |
| `=IMEXP("3.14159i")` | -1 | e^(i*pi) = -1. Euler's identity! |
| `=IMEXP("1.5708i")` | i | e^(i*pi/2) = i. Quarter rotation in complex plane. |
| `=IMEXP("6.2832i")` | 1 | e^(i*2*pi) = 1. Full rotation returns to 1. |
| `=IMEXP("1+1i")` | 1.46869393991589+2.28735528717884i | e^(1+i) = e * e^i. Combination of growth and rotation. |
| `=IMEXP("-1+0i")` | 0.367879441171442 | e^(-1) approximately equals 0.368. Exponential decay. |
| `=IMEXP("0-1.5708i")` | -i | e^(-i*pi/2) = -i. Clockwise quarter rotation. |
| `=IMEXP(COMPLEX(2, PI()))` | -7.38905609893065 | e^(2+i*pi) = -e^2 approximately equals -7.389. |
| `=IMEXP("0.5+0.5i")` | 1.44656987871598+0.790730557144555i | Moderate complex argument. |
| `=IMEXP("-2-2i")` | -0.056266034946379-0.0809447424053698i | Decay with rotation. |
| `=IMEXP("10")` | 22026.4657948067 | e^10 approximately equals 22026. Large growth. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#NUM!` | Real part too large causing overflow | e^709 overflows; limit Re(z) to reasonable values |
| `#NAME?` | Function not recognized | Check spelling; ensure Analysis ToolPak is enabled in older Excel |
| `#VALUE!` | Empty string or malformed input | Ensure proper complex number notation |

## Use Cases

### [[Phasor Analysis in AC Circuits]]
- **Implementation**: Represent AC signals using `=V_peak * IMEXP(COMPLEX(0, omega*t + phi))` where omega is angular frequency and phi is phase
- **Business Application**: Analyze power systems, design filters, and compute AC circuit responses in electrical engineering
- **Technical Details**: Phasor V = V_m * e^(j*phi) encodes magnitude and phase; impedance calculations use Z = R + j*X

### [[Fourier Transform Computation]]
- **Implementation**: Calculate DFT components using `=IMEXP(COMPLEX(0, -2*PI()*k*n/N))` for frequency index k
- **Business Application**: Perform spectral analysis for audio processing, image compression, and communication systems
- **Technical Details**: DFT kernel W_N^(kn) = e^(-j*2*pi*kn/N) is the twiddle factor; IMEXP computes these directly

### [[Control System Analysis]]
- **Implementation**: Evaluate system responses using `=IMEXP(COMPLEX(sigma*t, omega*t))` for pole contributions
- **Business Application**: Design stable control systems, analyze transient responses, and tune PID controllers
- **Technical Details**: Poles at s = sigma + j*omega produce time responses e^(st); stability requires sigma < 0

### [[Rotation and Scaling Transformations]]
- **Implementation**: Apply rotation by angle theta with scaling r using `=r * IMEXP(COMPLEX(0, theta))`
- **Business Application**: Implement 2D graphics transformations, analyze rotating machinery, and model orbital mechanics
- **Technical Details**: Multiplication by e^(i*theta) rotates counterclockwise by theta radians; |z| scales the magnitude

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Overflow threshold | Re(z) approximately 709 | Re(z) approximately 709 |
| Underflow behavior | Returns 0 for very negative Re(z) | Returns 0 for very negative Re(z) |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |

## Tips and Best Practices

1. **Master Euler's formula**: e^(ib) = cos(b) + i*sin(b). This is the key insight connecting exponentials to rotations.

2. **Separate magnitude and phase**: e^(a+bi) = e^a * e^(ib). The real part controls magnitude, imaginary controls phase.

3. **Unit circle values**: |e^(ib)| = 1 for all real b. Purely imaginary exponents give unit magnitude.

4. **Key values to remember**: e^(i*pi) = -1, e^(i*pi/2) = i, e^(i*pi/4) = (1+i)/sqrt(2).

5. **Periodicity in imaginary**: e^(z + 2*pi*i) = e^z. Adding 2*pi*i to exponent doesn't change result.

6. **Build trig functions**: cos(z) = (e^(iz) + e^(-iz))/2, sin(z) = (e^(iz) - e^(-iz))/(2i). Use IMEXP to construct these.

7. **Watch for overflow**: For Re(z) > 700, consider working with logarithms or scaling to avoid numerical issues.

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMLN]] | Complex natural logarithm - inverse of IMEXP |
| [[IMCOS]] | Complex cosine - (e^(iz) + e^(-iz))/2 |
| [[IMSIN]] | Complex sine - (e^(iz) - e^(-iz))/(2i) |
| [[IMPOWER]] | Complex power function for z^w |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |
| [[IMABS]] | Complex magnitude - |e^z| = e^(Re(z)) |
| [[IMARGUMENT]] | Complex argument - arg(e^z) = Im(z) |

## Official Documentation

- **Microsoft Excel**: [IMEXP function](https://support.microsoft.com/en-us/office/imexp-function-c6f8da1f-e024-4c0c-b802-a60e7147a95f)
- **Google Sheets**: [IMEXP function](https://support.google.com/docs/answer/3093421)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2007 | 2007-01 | Added as native function |
| Excel 2003 | 2003-10 | Available via Analysis ToolPak add-in |
| Excel 97 | 1997-01 | Available via Analysis ToolPak add-in |
| Google Sheets | 2006-06 | Supported since launch |
| Excel Online | 2010-06 | Full support in web version |
| Excel 365 | 2020-01 | Dynamic array support added |

---

**Mathematical Background**: The complex exponential is defined as:

e^z = e^(a+bi) = e^a * (cos(b) + i*sin(b))

This is Euler's formula: e^(ib) = cos(b) + i*sin(b)

Key properties:
- No zeros: e^z is never zero for any complex z
- Entire function: Analytic everywhere in the complex plane
- Essential singularity: At infinity (highly oscillatory behavior)
- Magnitude: |e^z| = e^(Re(z))
- Argument: arg(e^z) = Im(z) + 2*pi*k for integer k
- Derivative: d/dz e^z = e^z (unchanged by differentiation)
- Period: e^(z + 2*pi*i) = e^z (periodic in imaginary direction)
- Functional equation: e^(z1 + z2) = e^(z1) * e^(z2)

Special values:
- e^0 = 1
- e^(i*pi) = -1 (Euler's identity)
- e^(i*pi/2) = i
- e^(i*pi/4) = (sqrt(2) + i*sqrt(2))/2 = (1+i)/sqrt(2)
