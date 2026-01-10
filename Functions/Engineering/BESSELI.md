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
- scientific-computing
- heat-transfer
- cylindrical-coordinates
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- modified-bessel-first-kind
- bessel-i-function
tags:
- engineering
- mathematics
- scientific
- bessel
---

# BESSELI

## BESSELI Description

**BESSELI** returns the modified Bessel function of the first kind, denoted as I_n(x), which is a solution to the modified Bessel differential equation. This function is fundamental in mathematical physics and engineering, particularly in problems involving cylindrical symmetry with exponential rather than oscillatory behavior. The modified Bessel function I_n(x) can be defined as I_n(x) = i^(-n) * J_n(ix), where J_n is the Bessel function of the first kind and i is the imaginary unit.

The BESSELI function is essential for engineers and scientists working on heat conduction problems in cylindrical or spherical geometries, electromagnetic wave propagation in circular waveguides, vibration analysis of circular membranes, and statistical distributions such as the von Mises distribution. Unlike the standard Bessel function J_n, the modified version I_n(x) exhibits exponential growth for large arguments rather than oscillatory decay, making it suitable for problems where solutions grow or decay exponentially with distance from an axis or center point.

A critical consideration when using BESSELI is that the function grows rapidly for large positive arguments, which can lead to overflow errors when x exceeds approximately 700 (depending on system precision). The order n must be a non-negative integer, and negative values will cause errors. For small arguments close to zero, the function approaches 1 for n=0 and approaches 0 for n>0. Be aware that the function requires the Analysis ToolPak add-in to be enabled in older versions of Excel.

Both Excel and Google Sheets support BESSELI with identical syntax and behavior, making cross-platform spreadsheet development straightforward. The function is available in Excel 2007 and later versions natively, while earlier versions require the Analysis ToolPak add-in. Google Sheets has supported BESSELI since its inception with full compatibility. Results are accurate to approximately 15 significant digits on both platforms.

## Syntax

```
BESSELI(x, n)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **x** | The value at which to evaluate the function. Must be a numeric value. | Yes | - |
| **n** | The order of the Bessel function. Must be a non-negative integer (0, 1, 2, ...). | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=BESSELI(1, 0)` | 1.2660658778 | Modified Bessel function I_0(1). For order 0, I_0(x) equals the average value of e^(x*cos(theta)) over all angles. |
| `=BESSELI(1, 1)` | 0.5651591040 | Modified Bessel function I_1(1). The first-order function starts at 0 and increases with x. |
| `=BESSELI(0, 0)` | 1 | I_0(0) = 1. The zeroth-order modified Bessel function always equals 1 at x=0. |
| `=BESSELI(0, 1)` | 0 | I_n(0) = 0 for all n > 0. Higher-order functions are zero at the origin. |
| `=BESSELI(2.5, 0)` | 3.2898391440 | I_0(2.5) showing the exponential growth characteristic of modified Bessel functions. |
| `=BESSELI(2.5, 1)` | 2.5167162453 | I_1(2.5) is smaller than I_0(2.5) but still shows significant growth. |
| `=BESSELI(2.5, 2)` | 1.2764661479 | I_2(2.5). Higher orders produce smaller values for the same argument. |
| `=BESSELI(5, 0)` | 27.2398718236 | I_0(5) demonstrates rapid exponential growth for larger arguments. |
| `=BESSELI(5, 3)` | 10.3311501008 | I_3(5). Even high-order functions grow rapidly as x increases. |
| `=BESSELI(10, 0)` | 2815.7166284663 | I_0(10) shows the explosive growth characteristic of modified Bessel functions. |
| `=BESSELI(10, 5)` | 777.1882140053 | I_5(10). The ratio I_n(x)/I_0(x) approaches 1 as x becomes very large. |
| `=BESSELI(0.5, 2)` | 0.0319062164 | Small argument with higher order produces a very small result. |
| `=BESSELI(3.14159, 0)` | 6.1611932989 | Using pi as the argument; useful in periodic boundary problems. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input for x or n | Ensure both arguments are numbers or cell references containing numbers |
| `#NUM!` | n is negative | Use only non-negative integers for the order parameter |
| `#NUM!` | n is not an integer | Round or truncate n to the nearest integer using INT() or ROUND() |
| `#NUM!` | x is too large (overflow) | For x > ~700, results overflow; consider using logarithmic forms or scaling |
| `#NAME?` | Function not recognized (older Excel) | Enable the Analysis ToolPak add-in in Excel Options |
| `#REF!` | Invalid cell reference | Check that referenced cells exist and contain valid data |

## Use Cases

### [[Heat Transfer Analysis]]
- **Implementation**: Calculate temperature distributions in cylindrical systems using `=BESSELI(r*sqrt(s/alpha), 0)` where r is radial distance, s is Laplace variable, and alpha is thermal diffusivity
- **Business Application**: Design heat exchangers, insulation systems, and thermal management solutions for manufacturing and HVAC applications
- **Technical Details**: The modified Bessel function appears in Laplace-transformed heat equations; I_0 handles axisymmetric problems while higher orders handle angular variations

### [[Electromagnetic Engineering]]
- **Implementation**: Analyze field distributions in coaxial cables and circular waveguides using `=BESSELI(gamma*a, n)` where gamma is propagation constant and a is radius
- **Business Application**: Design telecommunications equipment, radar systems, and microwave components with optimal signal transmission characteristics
- **Technical Details**: TM and TE modes in circular waveguides involve modified Bessel functions; cutoff frequencies depend on zeros of these functions

### [[Statistical Analysis - Von Mises Distribution]]
- **Implementation**: Normalize the von Mises distribution using `=EXP(kappa*COS(theta))/(2*PI()*BESSELI(kappa, 0))` for circular statistics
- **Business Application**: Analyze directional data in biology (animal migration), meteorology (wind directions), and geology (fault orientations)
- **Technical Details**: I_0(kappa) serves as the normalization constant; higher concentration parameter kappa requires accurate Bessel computation

### [[Vibration Analysis]]
- **Implementation**: Model natural frequencies of circular membranes with radial tension using `=BESSELI(omega*r*SQRT(rho/T), n)`
- **Business Application**: Design drums, loudspeaker cones, and pressure vessel membranes with desired acoustic properties
- **Technical Details**: Combined with BESSELK for bounded domains; eigenfrequencies determined by boundary condition matching

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Maximum x value | ~709 before overflow | ~709 before overflow |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |
| LAMBDA compatibility | Yes (Excel 365) | Yes |
| Non-integer n handling | Truncates to integer | Truncates to integer |
| Negative x handling | Returns I_n(|x|) for even n | Returns I_n(|x|) for even n |

## Tips and Best Practices

1. **Check for overflow**: For large x values, consider using the asymptotic approximation I_n(x) ≈ e^x / sqrt(2*pi*x) or work with log(BESSELI()) when possible.

2. **Validate input ranges**: Use data validation or IF statements to ensure x and n are within acceptable ranges before calling BESSELI.

3. **Integer orders only**: Always ensure n is a non-negative integer; use `=BESSELI(x, INT(n))` if n might be a decimal.

4. **Combine with BESSELK**: Many boundary value problems require both I_n (finite at origin) and K_n (finite at infinity) for complete solutions.

5. **Recurrence relations**: For computing multiple orders efficiently, use I_{n+1}(x) = I_{n-1}(x) - (2n/x)*I_n(x).

6. **Numerical stability**: For ratio computations I_n(x)/I_m(x), consider computing both values and dividing rather than using asymptotic ratios.

7. **Physical interpretation**: Remember I_0 has a peak at the origin and I_n (n>0) are zero at the origin—important for boundary condition selection.

## Related Functions

| Function | Description |
|----------|-------------|
| [[BESSELJ]] | Bessel function of the first kind J_n(x) - oscillatory solution |
| [[BESSELK]] | Modified Bessel function of the second kind K_n(x) - decays exponentially |
| [[BESSELY]] | Bessel function of the second kind Y_n(x) - oscillatory, singular at origin |
| [[COMPLEX]] | Creates complex numbers for advanced engineering calculations |
| [[IMEXP]] | Complex exponential function for wave analysis |
| [[EXP]] | Standard exponential function related through I_0(x) = (1/pi)*integral of e^(x*cos(t)) |

## Official Documentation

- **Microsoft Excel**: [BESSELI function](https://support.microsoft.com/en-us/office/besseli-function-8d33855c-9a8d-444b-98e0-852267b1c0df)
- **Google Sheets**: [BESSELI function](https://support.google.com/docs/answer/3093408)

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

**Mathematical Background**: The modified Bessel function of the first kind is defined by the series:

I_n(x) = (x/2)^n * SUM[k=0 to infinity] { (x^2/4)^k / (k! * (n+k)!) }

For large x, the asymptotic expansion is:
I_n(x) ≈ e^x / sqrt(2*pi*x) * [1 - (4n^2-1)/(8x) + ...]
