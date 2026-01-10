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
- complex-square-root
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-square-root
- sqrt-complex
- imaginary-square-root
tags:
- engineering-function
- complex-numbers
- square-root
- principal-value
- polar-form
---

# IMSQRT

## Description

IMSQRT calculates the principal square root of a complex number. For a complex number z = r*e^(i*theta) in polar form, the square root is sqrt(r)*e^(i*theta/2), which means the magnitude is square-rooted and the argument is halved. Every nonzero complex number has exactly two square roots (differing by 180 degrees), and IMSQRT returns the principal value, defined as the root with argument in the range (-pi/2, pi/2], ensuring the real part is non-negative.

The complex square root extends the familiar real square root to handle negative numbers and complex inputs. Unlike real square roots, which are undefined for negative numbers, IMSQRT returns well-defined complex results: sqrt(-1) = i, sqrt(-4) = 2i, and so on. This capability is essential for solving quadratic equations with negative discriminants, analyzing electrical circuits with reactive components, and processing signals in the complex frequency domain.

In electrical engineering, IMSQRT appears in calculating characteristic impedance sqrt(Z*Y), propagation constants, and geometric means of complex quantities. Signal processing uses complex square roots for computing RMS values of complex signals, spectral analysis, and filter design. Control systems use it for finding poles and zeros of transfer functions from their squares, such as when analyzing Butterworth filter prototypes or solving Riccati equations.

Both Excel and Google Sheets implement IMSQRT identically, accepting a complex number string and returning the principal square root as a text string. The function handles purely real positive numbers (returning the positive real root), negative real numbers (returning a purely positive imaginary result), purely imaginary numbers, and general complex numbers. For the second square root (the negative of the principal root), multiply the IMSQRT result by -1 using IMSUB("0", IMSQRT(z)).

## Syntax

> [!NOTE] Syntax
> ```
> IMSQRT(inumber)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber** | The complex number whose square root to compute. Text string format (e.g., "3+4i", "-1", "4i"). | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMSQRT("3+4i")` | "2+i" | Verify: (2+i)^2 = 4+4i-1 = 3+4i |
| `=IMSQRT("-1")` | "i" | sqrt(-1) = i by definition |
| `=IMSQRT("-4")` | "2i" | sqrt(-4) = 2*sqrt(-1) = 2i |
| `=IMSQRT("4")` | "2" | Positive real: returns positive real root |
| `=IMSQRT("i")` | "0.707+0.707i" | sqrt(i) = (1+i)/sqrt(2), angle halved from 90 to 45 degrees |
| `=IMSQRT("-i")` | "0.707-0.707i" | sqrt(-i), angle halved from -90 to -45 degrees |
| `=IMSQRT("0")` | "0" | sqrt(0) = 0 |
| `=IMSQRT("8-6j")` | "3-j" | Verify: (3-j)^2 = 9-6j-1 = 8-6j |
| `=IMSQRT("-3+4i")` | "1+2i" | Verify: (1+2i)^2 = 1+4i-4 = -3+4i |
| `=IMSQRT("5+12i")` | "3+2i" | Verify: (3+2i)^2 = 9+12i-4 = 5+12i |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure input follows "a+bi" or "a+bj" format |
| `#VALUE!` | Input is a plain number, not text | Convert to text format or use COMPLEX function |
| `#VALUE!` | Empty cell reference | Ensure referenced cell contains a valid complex string |
| `#NUM!` | Spaces within complex number string | Remove all spaces from input text |
| `#NUM!` | Invalid characters in input | Use only digits, decimal points, +/-, and i or j |

## Use Cases

### 1. Characteristic Impedance of Transmission Lines

**Scenario**: An RF engineer calculates the characteristic impedance Z0 = sqrt(Z/Y) of a transmission line from its series impedance Z and shunt admittance Y per unit length.

**Implementation**: Use IMSQRT to compute the square root of the impedance-admittance ratio.

```
| Parameter | Value | Unit |
|-----------|-------|------|
| Series Impedance Z | 100+50j | ohm/m |
| Shunt Admittance Y | 0.001+0.002j | S/m |
| Z/Y | =IMDIV("100+50j", "0.001+0.002j") | 60000-80000j |
| Z0 = sqrt(Z/Y) | =IMSQRT("60000-80000j") | 223.6-178.9j ohms |

Characteristic impedance Z0 = sqrt(Z/Y)
Propagation constant gamma = sqrt(Z*Y)
For lossless line (Z purely reactive, Y purely susceptive):
Z0 is purely real, gamma is purely imaginary (no attenuation)
```

### 2. Quadratic Formula with Complex Discriminant

**Scenario**: A mathematician solves a quadratic equation ax^2 + bx + c = 0 where the discriminant b^2 - 4ac is negative or complex, requiring complex square root.

**Implementation**: Use IMSQRT to compute sqrt(discriminant) for the quadratic formula.

```
Quadratic: x^2 + 2x + 5 = 0
a = 1, b = 2, c = 5

| Step | Formula | Result |
|------|---------|--------|
| Discriminant | =IMSUB(IMPOWER("2",2), IMPRODUCT("4","1","5")) | -16 |
| sqrt(Discriminant) | =IMSQRT("-16") | 4i |
| x1 = (-b + sqrt)/2a | =IMDIV(IMSUM("-2", "4i"), "2") | -1+2i |
| x2 = (-b - sqrt)/2a | =IMDIV(IMSUB("-2", "4i"), "2") | -1-2i |

Roots: x = -1 +/- 2i (complex conjugate pair)
Verification: (-1+2i)^2 + 2(-1+2i) + 5 = (1-4-4i) + (-2+4i) + 5 = 0 ✓
```

### 3. RMS Calculation for Complex Signals

**Scenario**: A signal processing engineer computes the RMS (root-mean-square) amplitude of a complex-valued signal by taking the square root of the mean squared magnitude.

**Implementation**: While typically using IMABS for magnitude, demonstrate IMSQRT for complex intermediate calculations.

```
| Sample | Signal x[n] | |x|^2 | Formula |
|--------|-------------|-------|---------|
| 1 | 3+4i | 25 | =IMPRODUCT(x, IMCONJUGATE(x)) |
| 2 | 1+i | 2 | =IMPRODUCT(x, IMCONJUGATE(x)) |
| 3 | -2+3i | 13 | =IMPRODUCT(x, IMCONJUGATE(x)) |
| Mean |x|^2 | | 13.33 | =(25+2+13)/3 |
| RMS = sqrt(mean) | | 3.65 | =SQRT(13.33) |

For complex signal: RMS = sqrt(mean(|x|^2))
Alternative with IMSQRT for complex intermediate:
If computing sqrt(sum of complex powers), use IMSQRT
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Input format | Text string | Text string |
| Output format | Text string | Text string |
| Returns principal value | Yes | Yes |
| Negative real input | Returns positive imaginary | Returns positive imaginary |
| Zero input | Returns "0" | Returns "0" |
| Accepts "i"/"j" | Yes | Yes |
| Precision | 15 significant digits | 15 significant digits |

Both platforms implement IMSQRT identically, returning the principal square root with non-negative real part.

## Tips and Best Practices

1. **Verify by squaring**: Always check: IMPRODUCT(IMSQRT(z), IMSQRT(z)) should return z (within floating-point precision).

2. **Understand the principal value**: IMSQRT returns the root with argument in (-pi/2, pi/2], meaning non-negative real part. The other root is its negative.

3. **Get both roots if needed**: The two square roots of z are IMSQRT(z) and IMSUB("0", IMSQRT(z)). For quadratic formulas, you typically need both.

4. **Use polar form understanding**: |sqrt(z)| = sqrt(|z|) and arg(sqrt(z)) = arg(z)/2. A 90-degree angle halves to 45 degrees.

5. **Handle negative reals correctly**: sqrt(-9) = 3i, not -3i. The principal value always has non-negative real part.

6. **Compare with IMPOWER**: IMSQRT(z) is equivalent to IMPOWER(z, 0.5), but IMSQRT may be more efficient and clearer for square roots.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMPOWER]] | Raises complex number to any power (use 0.5 for square root) |
| [[SQRT]] | Square root for real numbers only |
| [[IMPRODUCT]] | Multiply to verify: sqrt(z)*sqrt(z) = z |
| [[IMABS]] | Magnitude: |sqrt(z)| = sqrt(|z|) |

### Commonly Used With

- **[[IMPRODUCT]]** - Verify: IMSQRT(z)^2 = z
- **[[IMABS]]** - Check magnitude: |sqrt(z)| = sqrt(|z|)
- **[[IMARGUMENT]]** - Check angle: arg(sqrt(z)) = arg(z)/2
- **[[IMDIV]]** - Often used before IMSQRT (e.g., sqrt(Z/Y))
- **[[IMSUB]]** - Get second root: -sqrt(z)
- **[[COMPLEX]]** - Create complex numbers to take roots of

## Official Documentation

- [Microsoft Excel IMSQRT Documentation](https://support.microsoft.com/en-us/office/imsqrt-function-e1753f80-ba11-4664-a10e-e17368396b70)
- [Google Sheets IMSQRT Documentation](https://support.google.com/docs/answer/3093215)

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
