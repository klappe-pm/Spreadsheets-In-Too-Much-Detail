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
- modified-bessel-functions
- special-functions
- exponential-decay
- boundary-value-problems
- cylindrical-coordinates
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- modified-bessel-second-kind
- bessel-k-function
- macdonald-function
tags:
- engineering
- mathematics
- scientific
- bessel
- decay
---

# BESSELK

## BESSELK Description

**BESSELK** returns the modified Bessel function of the second kind, denoted as K_n(x), which is a solution to the modified Bessel differential equation that decays exponentially for large arguments and becomes singular at the origin. Also known as the Macdonald function or Basset function, K_n(x) is the linearly independent complement to BESSELI (I_n) and is essential for problems requiring solutions that remain finite as the radial coordinate approaches infinity.

The BESSELK function is critical in physics and engineering applications involving exponential decay in cylindrical or spherical geometries. Common applications include modeling heat dissipation from cylindrical sources, electromagnetic field attenuation in coaxial cables, gravitational and electrostatic potentials around cylindrical bodies, and quantum mechanical tunneling through cylindrical barriers. K_0(x) represents the potential due to a line charge or line mass, while higher orders K_n(x) appear in more complex boundary conditions.

Key considerations when using BESSELK include its singular behavior at x=0, where the function approaches infinity. This singularity is logarithmic for K_0 (K_0(x) approaches -ln(x/2) - gamma as x approaches 0) and algebraic for higher orders (K_n(x) approaches (n-1)!/2 * (2/x)^n for n > 0). The argument x must be strictly positive; zero or negative values will return errors. For large x, K_n(x) decays as sqrt(pi/(2x)) * e^(-x), making it suitable for problems with decay boundary conditions.

Both Excel and Google Sheets implement BESSELK with identical syntax and behavior. The function requires x > 0 and n as a non-negative integer. Excel 2007 and later include BESSELK natively, while earlier versions require the Analysis ToolPak. Google Sheets has supported BESSELK since its launch. Numerical precision is approximately 15 significant digits on both platforms, though results very close to zero or for very large x may show platform-specific rounding.

## Syntax

```
BESSELK(x, n)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **x** | The value at which to evaluate the function. Must be a positive real number (x > 0). | Yes | - |
| **n** | The order of the Bessel function. Must be a non-negative integer (0, 1, 2, ...). | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=BESSELK(1, 0)` | 0.4210244382 | K_0(1), the zeroth-order modified Bessel function of the second kind. |
| `=BESSELK(1, 1)` | 0.6019072302 | K_1(1), first-order function; note K_1 > K_0 at x=1 due to the singularity structure. |
| `=BESSELK(0.1, 0)` | 2.4270690247 | K_0(0.1), showing the growth toward infinity as x approaches 0. |
| `=BESSELK(0.01, 0)` | 4.7212358389 | K_0(0.01), demonstrating the logarithmic singularity at the origin. |
| `=BESSELK(2, 0)` | 0.1138938727 | K_0(2), showing the exponential decay for larger arguments. |
| `=BESSELK(2, 1)` | 0.1398658818 | K_1(2), slightly larger than K_0(2) at this argument. |
| `=BESSELK(5, 0)` | 0.0036910983 | K_0(5), demonstrating rapid exponential decay. |
| `=BESSELK(5, 2)` | 0.0053089438 | K_2(5), higher orders decay slightly more slowly. |
| `=BESSELK(10, 0)` | 0.0000177801 | K_0(10), extremely small due to e^(-10) factor in asymptotic form. |
| `=BESSELK(0.5, 3)` | 59.0524401143 | K_3(0.5), large value due to the (2/x)^n singularity for small x and large n. |
| `=BESSELK(3, 0)` | 0.0347395044 | K_0(3), continuing the exponential decay pattern. |
| `=BESSELK(1.5, 2)` | 0.5836559632 | K_2(1.5), intermediate argument and order value. |
| `=BESSELK(2.5, 1)` | 0.0738908163 | K_1(2.5), showing transition from singular to decaying regime. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | x is zero or negative | BESSELK requires x > 0; use a positive value |
| `#NUM!` | n is negative | Use only non-negative integers for the order parameter |
| `#NUM!` | n is not an integer | Round or truncate n to the nearest integer using INT() or ROUND() |
| `#VALUE!` | Non-numeric input for x or n | Ensure both arguments are numbers or cell references containing numbers |
| `#NAME?` | Function not recognized (older Excel) | Enable the Analysis ToolPak add-in in Excel Options |
| `#NUM!` | x is extremely small causing overflow | K_n grows without bound as x approaches 0; use larger x values |

## Use Cases

### [[Electromagnetic Field Analysis]]
- **Implementation**: Calculate field attenuation in coaxial cables using `=BESSELK(gamma*r, 0)` where gamma is the propagation constant and r is radial distance
- **Business Application**: Design telecommunications cables, RF shielding, and waveguide systems with predictable signal loss characteristics
- **Technical Details**: Fields outside cylindrical conductors decay as K_0 or K_n; combine with BESSELI for interior solutions to match boundary conditions

### [[Heat Transfer - Cylindrical Fins]]
- **Implementation**: Model temperature in extended surfaces using `=BESSELK(m*r, 0)*BESSELI(m*R, 0) - BESSELI(m*r, 0)*BESSELK(m*R, 0)` for annular fins
- **Business Application**: Optimize heat sink designs, cooling fin geometry, and thermal management systems for electronics and industrial equipment
- **Technical Details**: The fin parameter m = sqrt(h*P/(k*A)) determines decay rate; K_n ensures finite temperature at outer radius

### [[Gravitational/Electrostatic Potentials]]
- **Implementation**: Calculate potential around cylindrical mass/charge distributions using `=lambda/(2*PI()*epsilon)*BESSELK(k*r, 0)` for screened potentials
- **Business Application**: Model ion screening in electrochemistry, plasma physics simulations, and geophysical exploration applications
- **Technical Details**: K_0 is the Green's function for the 2D Helmholtz equation; K_n appears in multipole expansions

### [[Quantum Tunneling]]
- **Implementation**: Model wavefunction decay in classically forbidden regions using `=A*BESSELK(kappa*r, n)` where kappa = sqrt(2m(V-E))/hbar
- **Business Application**: Design quantum devices, tunnel diodes, and scanning tunneling microscope analysis in semiconductor research
- **Technical Details**: K_n describes exponentially decaying solutions; probability current proportional to |K_n|^2 for transmission coefficients

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Minimum x value | x > 0 strictly | x > 0 strictly |
| Maximum x value | ~700 before underflow to 0 | ~700 before underflow to 0 |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |
| LAMBDA compatibility | Yes (Excel 365) | Yes |
| Non-integer n handling | Truncates to integer | Truncates to integer |

## Tips and Best Practices

1. **Avoid x=0**: BESSELK is undefined at x=0. Always validate inputs: `=IF(x>0, BESSELK(x, n), "Error: x must be positive")`.

2. **Combine with BESSELI**: Complete solutions to modified Bessel equations in bounded annular regions require C1*I_n(x) + C2*K_n(x).

3. **Use asymptotic form**: For x >> n, K_n(x) approximately equals sqrt(pi/(2x)) * e^(-x). This helps estimate without computation for large x.

4. **Small argument behavior**: For x << 1, K_0(x) approximately equals -ln(x/2) - gamma (Euler's constant approximately equals 0.5772), and K_n(x) approximately equals (n-1)!/2 * (2/x)^n for n > 0.

5. **Recurrence relations**: Compute multiple orders using K_{n+1}(x) = K_{n-1}(x) + (2n/x)*K_n(x).

6. **Wronskian identity**: I_n(x)*K_{n+1}(x) + I_{n+1}(x)*K_n(x) = 1/x, useful for verification and normalization.

7. **Physical interpretation**: K_n represents outward-propagating/decaying solutions; use for exterior problems, I_n for interior problems.

## Related Functions

| Function | Description |
|----------|-------------|
| [[BESSELI]] | Modified Bessel function of the first kind I_n(x) - exponentially growing, finite at origin |
| [[BESSELJ]] | Bessel function of the first kind J_n(x) - oscillatory solution |
| [[BESSELY]] | Bessel function of the second kind Y_n(x) - oscillatory, singular at origin |
| [[EXP]] | Exponential function - K_n decays approximately as e^(-x) for large x |
| [[LN]] | Natural logarithm - K_0 has logarithmic singularity at origin |

## Official Documentation

- **Microsoft Excel**: [BESSELK function](https://support.microsoft.com/en-us/office/besselk-function-606d11bc-06d3-4d53-9ecb-2803e2b90b70)
- **Google Sheets**: [BESSELK function](https://support.google.com/docs/answer/3093410)

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

**Mathematical Background**: The modified Bessel function of the second kind can be defined as:

K_n(x) = (pi/2) * [I_{-n}(x) - I_n(x)] / sin(n*pi)  (for non-integer n, extended by limits)

For integer n, use the limit:
K_n(x) = lim[v->n] K_v(x)

The integral representation is:
K_n(x) = INTEGRAL[0 to infinity] { e^(-x*cosh(t)) * cosh(n*t) dt }

Asymptotic expansions:
- Small x: K_0(x) approximately equals -ln(x/2) - gamma; K_n(x) approximately equals (n-1)!/2 * (2/x)^n
- Large x: K_n(x) approximately equals sqrt(pi/(2x)) * e^(-x) * [1 + (4n^2-1)/(8x) + ...]

**Modified Bessel Equation**: x^2*y'' + x*y' - (x^2 + n^2)*y = 0
