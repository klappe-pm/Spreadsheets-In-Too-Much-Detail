---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
- mathematical-functions
- bessel-functions
subTopics:
- cylindrical-bessel-functions
- special-functions
- wave-equations
- acoustics
- oscillatory-solutions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- bessel-first-kind
- cylindrical-bessel
- bessel-j-function
tags:
- engineering
- mathematics
- scientific
- bessel
- waves
---

# BESSELJ

## BESSELJ Description

**BESSELJ** returns the Bessel function of the first kind, denoted as J_n(x), which is the canonical solution to Bessel's differential equation that remains finite at the origin. This function was first defined by mathematician Daniel Bernoulli and later generalized by Friedrich Bessel in his study of planetary motion. The Bessel function J_n(x) is characterized by oscillatory behavior that resembles damped sinusoidal waves, with amplitude decreasing as 1/sqrt(x) for large arguments while zeros become approximately equally spaced.

The BESSELJ function is indispensable in physics and engineering for problems with cylindrical or spherical symmetry, including acoustic wave propagation, electromagnetic radiation patterns, heat conduction in cylindrical objects, and vibration modes of circular membranes. J_0(x) describes the radial wave pattern created by a pebble dropped in still water, while higher-order functions J_n(x) describe more complex angular patterns. In antenna theory, the radiation pattern of a circular aperture is directly proportional to J_1(x)/x, and in FM radio, the spectral content of frequency-modulated signals is determined by Bessel functions.

Key considerations when using BESSELJ include understanding that the function oscillates with decreasing amplitude and has infinitely many zeros (roots). For large arguments, the function behaves approximately as sqrt(2/(pi*x))*cos(x - n*pi/2 - pi/4). The order n must be a non-negative integer; non-integer orders are not supported in spreadsheet implementations. Near x=0, J_0(0)=1 while J_n(0)=0 for all n>0. The function can return both positive and negative values due to its oscillatory nature.

Both Excel and Google Sheets implement BESSELJ with identical syntax and numerical precision. The function is native to Excel 2007 and later, while earlier versions require the Analysis ToolPak add-in. Google Sheets has supported BESSELJ since launch. Both platforms provide approximately 15 significant digits of precision, and the function handles a wide range of arguments without overflow issues (unlike BESSELI, which grows exponentially).

## Syntax

```
BESSELJ(x, n)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **x** | The value at which to evaluate the function. Can be any real number. | Yes | - |
| **n** | The order of the Bessel function. Must be a non-negative integer (0, 1, 2, ...). | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=BESSELJ(0, 0)` | 1 | J_0(0) = 1. The zeroth-order Bessel function equals 1 at the origin. |
| `=BESSELJ(0, 1)` | 0 | J_n(0) = 0 for all n > 0. Higher-order functions are zero at the origin. |
| `=BESSELJ(1, 0)` | 0.7651976866 | J_0(1), showing the initial decrease from J_0(0)=1. |
| `=BESSELJ(1, 1)` | 0.4400505857 | J_1(1), the first-order Bessel function starts at 0, rises to a maximum, then oscillates. |
| `=BESSELJ(2.4048, 0)` | ~0 | First zero of J_0(x). This is crucial for drum vibration frequencies. |
| `=BESSELJ(3.8317, 1)` | ~0 | First zero of J_1(x). Used in circular aperture diffraction calculations. |
| `=BESSELJ(5, 0)` | -0.1775967713 | J_0(5) is negative, demonstrating the oscillatory nature of Bessel functions. |
| `=BESSELJ(5, 2)` | 0.0465651163 | J_2(5), a second-order Bessel function value. |
| `=BESSELJ(10, 0)` | -0.2459357645 | J_0(10) continues oscillating with gradually decreasing amplitude. |
| `=BESSELJ(10, 5)` | -0.2340615282 | J_5(10), showing that higher orders can have comparable magnitudes for large x. |
| `=BESSELJ(3.14159, 0)` | -0.3042421776 | J_0(pi), useful for periodic boundary condition problems. |
| `=BESSELJ(2, 3)` | 0.1289432494 | J_3(2), a third-order Bessel function at x=2. |
| `=BESSELJ(0.5, 0)` | 0.9384698072 | J_0(0.5), close to 1 for small arguments. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input for x or n | Ensure both arguments are numbers or cell references containing numbers |
| `#NUM!` | n is negative | Use only non-negative integers for the order parameter |
| `#NUM!` | n is not an integer | Round or truncate n to the nearest integer using INT() or ROUND() |
| `#NAME?` | Function not recognized (older Excel) | Enable the Analysis ToolPak add-in in Excel Options |
| `#REF!` | Invalid cell reference | Check that referenced cells exist and contain valid data |

## Use Cases

### [[Acoustic Engineering - Drum Vibrations]]
- **Implementation**: Calculate drum membrane resonant frequencies using `=BESSELJ(2.4048*r/R, 0)` where r is radial position and R is drum radius
- **Business Application**: Design musical instruments, speaker cones, and industrial acoustic dampening systems with precise frequency control
- **Technical Details**: The zeros of J_0 determine the fundamental modes (2.4048, 5.5201, 8.6537...); higher modes involve J_n with angular dependence

### [[Antenna Design]]
- **Implementation**: Model circular aperture antenna patterns using `=2*BESSELJ(k*a*SIN(theta), 1)/(k*a*SIN(theta))` for the Airy pattern
- **Business Application**: Design satellite dishes, radar systems, and wireless communication antennas with optimal gain patterns
- **Technical Details**: The first null occurs at J_1's first zero (3.8317), defining the main lobe width; sidelobes are determined by subsequent zeros

### [[FM Signal Analysis]]
- **Implementation**: Calculate FM spectral components using `=BESSELJ(beta, n)` where beta is modulation index and n is harmonic number
- **Business Application**: Design and analyze frequency modulation systems for broadcasting, telecommunications, and signal processing
- **Technical Details**: Carrier amplitude is J_0(beta); sidebands at harmonics have amplitudes J_n(beta); Carson bandwidth approximation relies on Bessel zeros

### [[Heat Conduction in Cylinders]]
- **Implementation**: Model radial temperature distribution using `=BESSELJ(lambda_m*r/R, 0)*EXP(-alpha*lambda_m^2*t/R^2)` summed over eigenvalues
- **Business Application**: Design heat exchangers, cooling systems, and thermal processing equipment for manufacturing applications
- **Technical Details**: Eigenvalues lambda_m are zeros of J_0 or J_1 depending on boundary conditions; series converges rapidly for large time

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Maximum x value | Effectively unlimited (oscillatory, bounded) | Effectively unlimited |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |
| LAMBDA compatibility | Yes (Excel 365) | Yes |
| Non-integer n handling | Truncates to integer | Truncates to integer |
| Negative x handling | J_n(-x) = (-1)^n * J_n(x) | Same behavior |

## Tips and Best Practices

1. **Know the zeros**: The first few zeros of J_0 are 2.4048, 5.5201, 8.6537, 11.7915; for J_1: 3.8317, 7.0156, 10.1735. These are crucial for eigenvalue problems.

2. **Use symmetry properties**: J_n(-x) = (-1)^n * J_n(x), so J_0, J_2, J_4... are even functions while J_1, J_3, J_5... are odd.

3. **Recurrence relations**: Compute multiple orders efficiently using J_{n-1}(x) + J_{n+1}(x) = (2n/x)*J_n(x).

4. **Asymptotic behavior**: For large x, J_n(x) approximately equals sqrt(2/(pi*x))*cos(x - n*pi/2 - pi/4). Useful for estimation without computation.

5. **Small argument approximation**: For x << 1, J_n(x) approximately equals (x/2)^n / n!. This helps understand behavior near zero.

6. **Combine with BESSELY**: For complete solutions in bounded regions, you need both J_n (finite at origin) and Y_n (singular at origin).

7. **Root finding**: To find zeros, use iterative methods like Newton-Raphson, starting from approximate values based on asymptotic spacing of pi.

## Related Functions

| Function | Description |
|----------|-------------|
| [[BESSELI]] | Modified Bessel function of the first kind I_n(x) - exponentially growing |
| [[BESSELK]] | Modified Bessel function of the second kind K_n(x) - exponentially decaying |
| [[BESSELY]] | Bessel function of the second kind Y_n(x) - oscillatory, singular at origin |
| [[SIN]] | Sine function - asymptotic behavior of Bessel functions involves sinusoids |
| [[COS]] | Cosine function - used in asymptotic Bessel approximations |
| [[SQRT]] | Square root - amplitude decay of Bessel functions goes as 1/sqrt(x) |

## Official Documentation

- **Microsoft Excel**: [BESSELJ function](https://support.microsoft.com/en-us/office/besselj-function-839cb181-48de-408b-9d80-bd02982d94f7)
- **Google Sheets**: [BESSELJ function](https://support.google.com/docs/answer/3093409)

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

**Mathematical Background**: The Bessel function of the first kind is defined by the series:

J_n(x) = SUM[k=0 to infinity] { (-1)^k * (x/2)^(n+2k) / (k! * (n+k)!) }

The integral representation is:
J_n(x) = (1/pi) * INTEGRAL[0 to pi] { cos(n*t - x*sin(t)) dt }

For large x, the asymptotic expansion is:
J_n(x) approximately equals sqrt(2/(pi*x)) * cos(x - n*pi/2 - pi/4)

**Bessel's Differential Equation**: x^2*y'' + x*y' + (x^2 - n^2)*y = 0
