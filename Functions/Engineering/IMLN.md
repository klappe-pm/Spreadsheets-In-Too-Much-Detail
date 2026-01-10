---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
- complex-numbers
- logarithmic-functions
subTopics:
- complex-logarithm
- signal-processing
- bode-plots
- branch-cuts
- decibels
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-natural-log
- imaginary-logarithm
- complex-ln
tags:
- engineering
- mathematics
- complex-numbers
- logarithm
- decibels
---

# IMLN

## IMLN Description

**IMLN** returns the natural logarithm of a complex number in the form "x+yi" or "x+yj". For a complex number z = r*e^(i*theta) expressed in polar form, the complex logarithm is defined as ln(z) = ln(r) + i*theta, where r = |z| is the magnitude and theta = arg(z) is the argument (angle) of z. This function is the inverse of IMEXP, such that IMEXP(IMLN(z)) = z for all non-zero z.

The IMLN function is crucial in engineering applications involving decibel calculations, phase analysis, and transfer function characterization. In control systems, the logarithm converts multiplication of transfer functions into addition, simplifying Bode plot construction. In signal processing, log-magnitude representations are standard for spectral analysis. The complex logarithm also appears in solving exponential equations with complex solutions and in conformal mapping for fluid dynamics and electrostatics.

A critical consideration with IMLN is that the complex logarithm is multi-valued because e^(z + 2*pi*i) = e^z. The principal branch is defined with the imaginary part (argument) in the range (-pi, pi], which means there is a branch cut along the negative real axis. The function is undefined at z = 0 (since ln(0) is negative infinity). For negative real numbers, IMLN returns values with imaginary part equal to pi (e.g., ln(-1) = i*pi). The real part of the result equals ln|z|, which can be very negative for small |z| and positive for large |z|.

Both Excel and Google Sheets implement IMLN using the principal branch with consistent behavior. The function accepts complex numbers in "i" or "j" notation and returns results in "i" format. The imaginary part of the result (the argument) is always in the range (-pi, pi]. Both platforms return errors when the input is zero. Precision is approximately 15 significant digits.

## Syntax

```
IMLN(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the natural logarithm. Can be in "x+yi" or "x+yj" format, or a cell reference. Must not be zero. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMLN("1")` | 0 | ln(1) = 0. The logarithm of 1 is zero. |
| `=IMLN("2.71828")` | ~1 | ln(e) approximately equals 1. |
| `=IMLN("1i")` | 1.5707963267949i | ln(i) = i*pi/2. Purely imaginary result. |
| `=IMLN("-1")` | 3.14159265358979i | ln(-1) = i*pi. Euler's identity rearranged. |
| `=IMLN("-1i")` | -1.5707963267949i | ln(-i) = -i*pi/2. |
| `=IMLN("1+1i")` | 0.346573590279973+0.785398163397448i | ln(1+i) = ln(sqrt(2)) + i*pi/4. |
| `=IMLN("10")` | 2.30258509299405 | ln(10) approximately equals 2.303. Natural log, not base 10. |
| `=IMLN("2+3i")` | 1.28247467873077+0.98279372324733i | Full complex logarithm with both parts. |
| `=IMLN(COMPLEX(-1, 1))` | 0.346573590279973+2.35619449019234i | ln(-1+i), argument in second quadrant. |
| `=IMLN("0.5")` | -0.693147180559945 | ln(0.5) = -ln(2) approximately equals -0.693. |
| `=IMLN("-2-2i")` | 1.03972077083992-2.35619449019234i | Third quadrant; argument is negative. |
| `=IMLN("100")` | 4.60517018598809 | ln(100) = 2*ln(10) approximately equals 4.605. |
| `=IMLN(IMEXP("3+4i"))` | 3+4i | Inverse property: ln(e^z) = z (for -pi < Im(z) <= pi). |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input is zero | IMLN(0) is undefined; check for zero before computing |
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#NAME?` | Function not recognized | Check spelling; ensure Analysis ToolPak is enabled in older Excel |
| `#VALUE!` | Empty string or malformed input | Ensure proper complex number notation |

## Use Cases

### [[Bode Plot Construction]]
- **Implementation**: Calculate log-magnitude using `=20*LOG10(IMABS(H)) + IMARGUMENT(H)*180/PI()` where H is complex transfer function
- **Business Application**: Design and analyze control systems, filters, and feedback loops in electrical and mechanical engineering
- **Technical Details**: IMLN provides ln|H| + i*arg(H); convert to dB with 20*ln|H|/ln(10) = 20*IMREAL(IMLN(H))/ln(10)

### [[Complex Power Calculation]]
- **Implementation**: Compute z^w using `=IMEXP(IMPRODUCT(w, IMLN(z)))` for arbitrary complex exponents
- **Business Application**: Evaluate advanced mathematical models in physics, finance, and engineering
- **Technical Details**: z^w = e^(w*ln(z)); multi-valued due to branch of logarithm; principal value uses IMLN principal branch

### [[Signal Phase Unwrapping]]
- **Implementation**: Track continuous phase using IMLN to extract phase from complex signals
- **Business Application**: Process radar, sonar, and communication signals requiring phase continuity
- **Technical Details**: Phase = IMARGUMENT(z) = Im(IMLN(z)); must handle 2*pi discontinuities at branch cut

### [[Conformal Mapping]]
- **Implementation**: Apply logarithmic conformal maps using `=IMLN(z)` to transform wedge domains to strips
- **Business Application**: Solve fluid flow around corners, heat conduction in angular domains, and electrostatic problems
- **Technical Details**: ln(z) maps sectors to horizontal strips; branch cut on negative real axis

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Branch cut | Negative real axis | Negative real axis |
| Argument range | (-pi, pi] | (-pi, pi] |
| ln(0) handling | #NUM! error | #NUM! error |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |

## Tips and Best Practices

1. **Never compute ln(0)**: Always check that z is not zero: `=IF(IMABS(z)=0, "Undefined", IMLN(z))`.

2. **Understand the branch cut**: The imaginary part of IMLN is in (-pi, pi]. For negative real numbers, the result has imaginary part = pi.

3. **Relation to IMEXP**: IMLN is the inverse of IMEXP on the principal branch: IMEXP(IMLN(z)) = z, but IMLN(IMEXP(z)) = z only if -pi < Im(z) <= pi.

4. **Polar form insight**: ln(r*e^(i*theta)) = ln(r) + i*theta. Real part is log-magnitude, imaginary is phase angle.

5. **Convert to dB**: For decibels, use `=20*IMREAL(IMLN(z))/LN(10)` or equivalently `=20*LOG10(IMABS(z))`.

6. **Multi-valued nature**: Different branches of ln(z) differ by 2*pi*i*k for integer k. IMLN gives the principal branch (k=0).

7. **Chain rule for products**: ln(z1*z2) = ln(z1) + ln(z2) (modulo 2*pi*i adjustments at branch cut).

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMEXP]] | Complex exponential - inverse of IMLN |
| [[IMLOG10]] | Complex base-10 logarithm |
| [[IMLOG2]] | Complex base-2 logarithm |
| [[IMABS]] | Complex magnitude - e^(Re(IMLN(z))) = |z| |
| [[IMARGUMENT]] | Complex argument - Im(IMLN(z)) = arg(z) |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |
| [[LN]] | Real natural logarithm for comparison |

## Official Documentation

- **Microsoft Excel**: [IMLN function](https://support.microsoft.com/en-us/office/imln-function-32b98bcf-8b81-437c-a636-6f3d1c5409d5)
- **Google Sheets**: [IMLN function](https://support.google.com/docs/answer/3093422)

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

**Mathematical Background**: The complex natural logarithm is defined for z = r*e^(i*theta) as:

ln(z) = ln(r) + i*theta = ln|z| + i*arg(z)

For z = a + bi, this can be written as:
ln(a + bi) = ln(sqrt(a^2 + b^2)) + i*atan2(b, a)
           = (1/2)*ln(a^2 + b^2) + i*atan2(b, a)

Key properties:
- Multi-valued: Full log has branches differing by 2*pi*i*k
- Principal branch: Imaginary part in (-pi, pi]
- Branch cut: Along negative real axis where argument jumps from pi to -pi
- Inverse of exp: e^(ln(z)) = z for all z except z = 0
- Undefined at origin: ln(0) does not exist
- Derivative: d/dz ln(z) = 1/z
- Product rule: ln(z1*z2) = ln(z1) + ln(z2) + 2*pi*i*k for appropriate k
- Power rule: ln(z^n) = n*ln(z) + 2*pi*i*k

Special values:
- ln(1) = 0
- ln(-1) = i*pi (Euler's identity: e^(i*pi) = -1)
- ln(i) = i*pi/2
- ln(-i) = -i*pi/2
- ln(e) = 1
