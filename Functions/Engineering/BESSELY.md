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
- neumann-functions
- boundary-value-problems
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- bessel-second-kind
- neumann-function
- weber-function
- bessel-y-function
tags:
- engineering
- mathematics
- scientific
- bessel
- waves
---

# BESSELY

## BESSELY Description

**BESSELY** returns the Bessel function of the second kind, denoted as Y_n(x), also known as the Neumann function (N_n) or Weber function. This function is the second linearly independent solution to Bessel's differential equation, complementing BESSELJ (J_n). While J_n(x) remains finite at the origin, Y_n(x) has a logarithmic singularity at x=0 for n=0 and an algebraic singularity (proportional to x^(-n)) for n>0, making it essential for problems where boundary conditions exclude the origin.

The BESSELY function is fundamental in wave physics and engineering applications involving cylindrical geometry where solutions must satisfy conditions at both inner and outer boundaries. Applications include acoustic radiation from vibrating cylinders, electromagnetic field patterns in hollow waveguides, water wave diffraction around circular islands, and seismic wave propagation in layered cylindrical structures. The general solution to problems with cylindrical symmetry is A*J_n(x) + B*Y_n(x), where constants A and B are determined by boundary conditions.

Critical considerations when using BESSELY include its singular behavior at x=0, which means the function is only defined for positive x values. Like BESSELJ, the function oscillates for positive arguments with amplitude decaying as 1/sqrt(x) and zeros becoming approximately equally spaced. For large x, Y_n(x) behaves as sqrt(2/(pi*x))*sin(x - n*pi/2 - pi/4), which is 90 degrees out of phase with J_n(x). The order n must be a non-negative integer in spreadsheet implementations.

Both Excel and Google Sheets implement BESSELY with identical syntax, requiring x > 0 and n as a non-negative integer. The function is native to Excel 2007 and later, while earlier versions require the Analysis ToolPak add-in. Google Sheets has supported BESSELY since launch. Both platforms provide approximately 15 significant digits of precision. When x is very close to zero, results may overflow to very large negative numbers before returning an error.

## Syntax

```
BESSELY(x, n)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **x** | The value at which to evaluate the function. Must be a positive real number (x > 0). | Yes | - |
| **n** | The order of the Bessel function. Must be a non-negative integer (0, 1, 2, ...). | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=BESSELY(1, 0)` | 0.0882569642 | Y_0(1), the zeroth-order Neumann function at x=1. |
| `=BESSELY(1, 1)` | -0.7812128213 | Y_1(1), negative because Y_1 starts at negative infinity and rises. |
| `=BESSELY(0.1, 0)` | -1.5342386513 | Y_0(0.1), approaching negative infinity as x approaches 0. |
| `=BESSELY(0.5, 0)` | -0.4445187335 | Y_0(0.5), still negative but closer to first zero. |
| `=BESSELY(0.8936, 0)` | ~0 | First zero of Y_0(x) approximately at x = 0.8936. |
| `=BESSELY(2, 0)` | 0.5103756726 | Y_0(2), now positive after crossing zero. |
| `=BESSELY(5, 0)` | -0.3085176252 | Y_0(5), oscillating back to negative. |
| `=BESSELY(5, 2)` | 0.3676628826 | Y_2(5), second-order Neumann function. |
| `=BESSELY(10, 0)` | 0.0556711673 | Y_0(10), amplitude decreasing as 1/sqrt(x). |
| `=BESSELY(10, 5)` | 0.1354030477 | Y_5(10), higher-order function at large argument. |
| `=BESSELY(3.14159, 0)` | 0.3289375969 | Y_0(pi), value at a common mathematical constant. |
| `=BESSELY(2.1971, 1)` | ~0 | First zero of Y_1(x) approximately at x = 2.1971. |
| `=BESSELY(3, 2)` | 0.1604003762 | Y_2(3), intermediate argument and order. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | x is zero or negative | BESSELY requires x > 0; the function has a singularity at x=0 |
| `#NUM!` | n is negative | Use only non-negative integers for the order parameter |
| `#NUM!` | n is not an integer | Round or truncate n to the nearest integer using INT() or ROUND() |
| `#VALUE!` | Non-numeric input for x or n | Ensure both arguments are numbers or cell references containing numbers |
| `#NAME?` | Function not recognized (older Excel) | Enable the Analysis ToolPak add-in in Excel Options |

## Use Cases

### [[Acoustic Radiation from Cylinders]]
- **Implementation**: Model sound radiation using `=A*BESSELJ(k*r, n) + B*BESSELY(k*r, n)` where k is wavenumber and r is radial distance
- **Business Application**: Design loudspeakers, sonar transducers, and noise barriers with predictable acoustic radiation patterns
- **Technical Details**: For exterior problems (r > a), use Hankel functions H_n = J_n + i*Y_n for outgoing waves; coefficients from boundary matching

### [[Hollow Waveguide Analysis]]
- **Implementation**: Calculate TE/TM modes in coaxial waveguides using `=BESSELJ(k_c*r, n)*BESSELY(k_c*a, n) - BESSELY(k_c*r, n)*BESSELJ(k_c*a, n)`
- **Business Application**: Design microwave transmission systems, particle accelerators, and high-power RF equipment
- **Technical Details**: Cutoff conditions involve zeros of J_n*Y_n' - Y_n*J_n' combinations; mode charts essential for design

### [[Water Wave Diffraction]]
- **Implementation**: Model wave scattering around circular islands using both J_n and Y_n to form Hankel functions for radiation conditions
- **Business Application**: Design offshore structures, breakwaters, and coastal protection systems with accurate wave loading predictions
- **Technical Details**: Sommerfeld radiation condition requires outgoing waves only; use H_n^(1) = J_n + i*Y_n for this purpose

### [[Vibration of Annular Plates]]
- **Implementation**: Analyze ring-shaped membrane vibrations using `=C1*BESSELJ(k*r, n) + C2*BESSELY(k*r, n) + C3*BESSELI(k*r, n) + C4*BESSELK(k*r, n)`
- **Business Application**: Design circular diaphragms, speaker cones, and pressure vessel end caps with controlled resonance properties
- **Technical Details**: Four constants require four boundary conditions (two at inner radius, two at outer); eigenvalues from determinant condition

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Minimum x value | x > 0 strictly | x > 0 strictly |
| Maximum x value | Effectively unlimited (oscillatory, bounded) | Effectively unlimited |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |
| LAMBDA compatibility | Yes (Excel 365) | Yes |
| Non-integer n handling | Truncates to integer | Truncates to integer |
| Behavior near x=0 | Returns very large negative numbers | Same behavior |

## Tips and Best Practices

1. **Always check x > 0**: BESSELY is undefined at x=0. Use `=IF(x>0, BESSELY(x, n), "Undefined")` for safety.

2. **Know key zeros**: First zeros of Y_0: 0.8936, 3.9577, 7.0861; Y_1: 2.1971, 5.4297, 8.5960. Critical for eigenvalue problems.

3. **Combine with BESSELJ**: General solutions require both functions: y(x) = A*J_n(x) + B*Y_n(x). Use boundary conditions to find A and B.

4. **Form Hankel functions**: For wave radiation problems, H_n^(1)(x) = J_n(x) + i*Y_n(x) represents outgoing waves; H_n^(2)(x) = J_n(x) - i*Y_n(x) represents incoming waves.

5. **Asymptotic behavior**: For large x, Y_n(x) approximately equals sqrt(2/(pi*x))*sin(x - n*pi/2 - pi/4), 90 degrees out of phase with J_n.

6. **Small argument behavior**: Y_0(x) approximately equals (2/pi)*[ln(x/2) + gamma] for x << 1; Y_n(x) approximately equals -(n-1)!/pi * (2/x)^n for n > 0.

7. **Wronskian relation**: J_n(x)*Y_{n+1}(x) - J_{n+1}(x)*Y_n(x) = -2/(pi*x), useful for normalizations and verifications.

## Related Functions

| Function | Description |
|----------|-------------|
| [[BESSELJ]] | Bessel function of the first kind J_n(x) - oscillatory, finite at origin |
| [[BESSELI]] | Modified Bessel function of the first kind I_n(x) - exponentially growing |
| [[BESSELK]] | Modified Bessel function of the second kind K_n(x) - exponentially decaying |
| [[SIN]] | Sine function - Y_n asymptotically behaves as sin(...) |
| [[COS]] | Cosine function - J_n asymptotically behaves as cos(...) |
| [[LN]] | Natural logarithm - Y_0 has logarithmic singularity at origin |

## Official Documentation

- **Microsoft Excel**: [BESSELY function](https://support.microsoft.com/en-us/office/bessely-function-f3a356b3-da89-42c3-8974-2da54d6353a2)
- **Google Sheets**: [BESSELY function](https://support.google.com/docs/answer/3093411)

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

**Mathematical Background**: The Bessel function of the second kind is defined as:

Y_n(x) = [J_n(x)*cos(n*pi) - J_{-n}(x)] / sin(n*pi)  (for non-integer n)

For integer n, use the limit which gives:
Y_n(x) = (2/pi)*J_n(x)*ln(x/2) - (1/pi)*SUM[k=0 to n-1]{(n-k-1)!/k! * (x/2)^(2k-n)}
         - (1/pi)*SUM[k=0 to infinity]{(-1)^k * (psi(k+1) + psi(n+k+1)) * (x/2)^(n+2k) / (k!*(n+k)!)}

where psi is the digamma function.

Asymptotic expansion for large x:
Y_n(x) approximately equals sqrt(2/(pi*x)) * sin(x - n*pi/2 - pi/4)

**Bessel's Differential Equation**: x^2*y'' + x*y' + (x^2 - n^2)*y = 0

The Wronskian of J_n and Y_n: W{J_n, Y_n} = 2/(pi*x)
