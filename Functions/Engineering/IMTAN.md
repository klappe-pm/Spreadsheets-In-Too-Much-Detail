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
- conformal-mapping
- phase-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-tangent
- imaginary-tangent
tags:
- engineering
- mathematics
- complex-numbers
- trigonometry
- phase
---

# IMTAN

## IMTAN Description

**IMTAN** returns the tangent of a complex number in the form "x+yi" or "x+yj". The complex tangent is defined as tan(z) = sin(z)/cos(z), extending the familiar real tangent function into the complex plane. For a complex number z = a + bi, the tangent can be expressed as tan(z) = [sin(2a) + i*sinh(2b)] / [cos(2a) + cosh(2b)], which shows how both real and imaginary parts of z affect the result.

The IMTAN function is essential in signal processing, control systems, and physics applications involving phase relationships and periodic phenomena. In electrical engineering, tangent functions appear in transmission line calculations, antenna impedance matching, and filter design. In physics, the complex tangent arises in quantum mechanical scattering problems and optical reflection/transmission coefficients. The function is also fundamental in conformal mapping applications.

Critical considerations when using IMTAN include its poles (singularities) at z = pi/2 + n*pi for all integers n, where cos(z) = 0. Near these points, the function approaches infinity. Unlike real tangent which is unbounded and oscillates, the complex tangent has controlled behavior away from the real axis: as |Im(z)| grows large, |tan(z)| approaches 1 (specifically, tan(a+bi) approaches i*sgn(b) as |b| approaches infinity). IMTAN is an odd function with period pi.

Both Excel and Google Sheets implement IMTAN with identical behavior. The function accepts complex numbers in "i" or "j" notation and returns results in "i" format. Pole behavior is handled similarly on both platforms, returning errors when input is at a singularity. Numerical precision is approximately 15 significant digits.

## Syntax

```
IMTAN(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the tangent. Can be in "x+yi" or "x+yj" format, or a cell reference. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMTAN("0")` | 0 | tan(0) = 0. |
| `=IMTAN("0.7854")` | ~1 | tan(pi/4) approximately equals 1. |
| `=IMTAN("1")` | 1.55740772465490 | tan(1 radian) approximately equals 1.557. |
| `=IMTAN("1i")` | 0.761594155955764i | tan(i) = i*tanh(1) approximately equals 0.762i. Purely imaginary. |
| `=IMTAN("2i")` | 0.964027580075817i | tan(2i) = i*tanh(2). Approaches i as imaginary part grows. |
| `=IMTAN("1+1i")` | 0.271752585319512+1.08392332733869i | Full complex tangent. |
| `=IMTAN("3.14159")` | ~0 | tan(pi) approximately equals 0. |
| `=IMTAN("0+5i")` | 0.999909204262595i | For large imaginary, approaches i. |
| `=IMTAN(COMPLEX(2, 1))` | -0.0243458201185727+1.01479361614663i | Using COMPLEX() for input. |
| `=IMTAN("0.5+0.5i")` | 0.400588963320936+0.564083245812168i | Moderate complex argument. |
| `=IMTAN("-1")` | -1.55740772465490 | tan(-1) = -tan(1). Odd function. |
| `=IMTAN("2+3i")` | -0.00376402564150425+1.00323862735361i | Large imaginary dominates. |
| `=IMTAN("-0.7854")` | ~-1 | tan(-pi/4) approximately equals -1. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#NUM!` | Input is at or very near a pole (z = pi/2 + n*pi) | Avoid odd multiples of pi/2 on the real axis |
| `#DIV/0!` | Division by zero when cos(z) = 0 | Check for poles before computing; add small imaginary part |
| `#NAME?` | Function not recognized | Verify spelling; ensure Analysis ToolPak enabled in older Excel |
| `#VALUE!` | Empty string or invalid characters | Ensure proper complex number format |

## Use Cases

### [[Transmission Line Impedance]]
- **Implementation**: Calculate normalized impedance using `=IMTAN(COMPLEX(beta*l, alpha*l))` for lossy line analysis
- **Business Application**: Design RF matching networks, antenna feeds, and high-frequency transmission systems
- **Technical Details**: For lossless lines, tan(beta*l) gives normalized reactance; complex extension handles lossy lines with attenuation alpha

### [[Filter Design]]
- **Implementation**: Compute bilinear transform using `=IMTAN(COMPLEX(omega_d*T/2, 0))` for frequency mapping
- **Business Application**: Design digital filters by mapping analog prototypes to discrete-time implementations
- **Technical Details**: Bilinear transform uses omega_a = (2/T)*tan(omega_d*T/2); complex extension useful for broadband analysis

### [[Conformal Mapping]]
- **Implementation**: Apply tan(z) transformation to map infinite strips to bounded regions
- **Business Application**: Solve potential flow problems, heat conduction, and electrostatic field distributions
- **Technical Details**: tan(z) maps vertical strips of width pi to the entire complex plane; useful for periodic domain problems

### [[Scattering and Reflection Analysis]]
- **Implementation**: Calculate phase shifts using `=IMTAN(COMPLEX(delta_real, delta_imag))` for quantum scattering
- **Business Application**: Analyze wave scattering, design anti-reflection coatings, and model resonances
- **Technical Details**: S-matrix elements involve tan of complex phase shifts; poles correspond to bound states or resonances

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Pole handling | #NUM! near poles | #NUM! near poles |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |

## Tips and Best Practices

1. **Avoid poles**: tan(z) has poles at z = pi/2 + n*pi for all integers n. Add a small imaginary part when working near these values.

2. **Odd function property**: tan(-z) = -tan(z). Use this to simplify calculations on symmetric domains.

3. **Period pi**: tan(z + pi) = tan(z). The function has period pi, not 2*pi like sine and cosine.

4. **Large imaginary limit**: As |Im(z)| grows, tan(z) approaches i*sgn(Im(z)). This bounded behavior is useful for asymptotic analysis.

5. **Build from sin/cos**: Verify with `=IMDIV(IMSIN(z), IMCOS(z))` for the same result.

6. **Relation to tanh**: tan(iz) = i*tanh(z). Convert between trigonometric and hyperbolic as needed.

7. **Phase unwrapping**: Complex tangent naturally handles 2*pi phase ambiguity issues in some phase recovery algorithms.

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMSIN]] | Complex sine - numerator of tan(z) |
| [[IMCOS]] | Complex cosine - denominator of tan(z) |
| [[IMCOT]] | Complex cotangent - 1/tan(z) |
| [[IMSEC]] | Complex secant - 1/cos(z) |
| [[IMCSC]] | Complex cosecant - 1/sin(z) |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |

## Official Documentation

- **Microsoft Excel**: [IMTAN function](https://support.microsoft.com/en-us/office/imtan-function-8478f45d-610a-43cf-8544-9fc0b553a132)
- **Google Sheets**: [IMTAN function](https://support.google.com/docs/answer/3093428)

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

**Mathematical Background**: The complex tangent is defined as:

tan(z) = sin(z)/cos(z) = -i * (e^(iz) - e^(-iz))/(e^(iz) + e^(-iz))

For z = a + bi, the explicit formula is:
tan(a + bi) = [sin(2a) + i*sinh(2b)] / [cos(2a) + cosh(2b)]

Key properties:
- Odd function: tan(-z) = -tan(z)
- Period: pi (tan(z + pi) = tan(z))
- Poles: z = pi/2 + n*pi for all integers n (where cos(z) = 0)
- Zeros: z = n*pi for all integers n (where sin(z) = 0)
- Asymptotic: tan(a + bi) approaches i*sgn(b) as |b| approaches infinity
- Derivative: d/dz tan(z) = sec^2(z) = 1 + tan^2(z)
- Relation: tan(iz) = i*tanh(z)

The magnitude behaves as:
|tan(a+bi)|^2 = [sin^2(2a) + sinh^2(2b)] / [cos(2a) + cosh(2b)]^2

For large |b|:
tan(a + bi) approximately equals i*sgn(b) * [1 - 2*e^(-2|b|)*cos(2a)]
