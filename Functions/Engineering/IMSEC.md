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
- reciprocal-trig
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-secant
- imaginary-secant
tags:
- engineering
- mathematics
- complex-numbers
- trigonometry
---

# IMSEC

## IMSEC Description

**IMSEC** returns the secant of a complex number in the form "x+yi" or "x+yj". The complex secant is defined as sec(z) = 1/cos(z), the reciprocal of the complex cosine function. For a complex number z = a + bi, the secant involves both trigonometric and hyperbolic components, with sec(z) = [cos(a)cosh(b) + i*sin(a)sinh(b)] / [cos^2(a)cosh^2(b) + sin^2(a)sinh^2(b)].

The IMSEC function is useful in specialized engineering and mathematical applications where reciprocal trigonometric functions appear naturally. This includes analysis of certain filter designs, wave scattering problems, and conformal mappings. In optics and electromagnetics, secant functions can appear in calculations involving oblique incidence angles and polarization. The function also arises in evaluating certain definite integrals and series in complex analysis.

Critical considerations when using IMSEC include understanding that it has poles (singularities) wherever cos(z) = 0, which occurs at z = pi/2 + n*pi for all integers n. Near these poles, the function approaches infinity. Unlike real secant which is unbounded, the complex secant decays exponentially for large imaginary parts because |sec(a+bi)| approaches 2*e^(-|b|) as |b| approaches infinity. The function is even (sec(-z) = sec(z)) and has period 2*pi.

Both Excel and Google Sheets implement IMSEC with identical behavior. The function accepts complex numbers in "i" or "j" notation and returns results using "i" notation. Pole behavior is handled similarly on both platforms, returning errors when input is exactly at a singularity. Numerical precision is approximately 15 significant digits.

## Syntax

```
IMSEC(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the secant. Can be in "x+yi" or "x+yj" format, or a cell reference. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMSEC("0")` | 1 | sec(0) = 1/cos(0) = 1. |
| `=IMSEC("1")` | 1.85081571768093 | sec(1) = 1/cos(1) approximately equals 1.851. |
| `=IMSEC("1i")` | 0.648054273663885 | sec(i) = 1/cosh(1) approximately equals 0.648. Real result for imaginary input. |
| `=IMSEC("2i")` | 0.265802228834079 | sec(2i) = 1/cosh(2). Decreases as imaginary part grows. |
| `=IMSEC("1+1i")` | 0.498337030555187+0.591083841721045i | Full complex result. |
| `=IMSEC("3.14159")` | -1 | sec(pi) = 1/cos(pi) = -1. |
| `=IMSEC("0.7854")` | ~1.414 | sec(pi/4) = sqrt(2) approximately equals 1.414. |
| `=IMSEC("0+3i")` | 0.0993279274194332 | sec(3i) = 1/cosh(3) approximately equals 0.099. Small for large imaginary. |
| `=IMSEC(COMPLEX(2, 1))` | -0.41314934426694+0.687527438655479i | Using COMPLEX() for input. |
| `=IMSEC("0.5+0.5i")` | 0.950439328502208+0.240566966567109i | Moderate complex argument. |
| `=IMSEC("-1")` | 1.85081571768093 | sec(-1) = sec(1). Even function. |
| `=IMSEC("2+3i")` | -0.0416749644111443-0.0906111371962376i | Large imaginary; small magnitude. |
| `=IMSEC("0.5")` | 1.13949392732455 | sec(0.5) = 1/cos(0.5) approximately equals 1.139. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#NUM!` | Input is at a pole (z = pi/2 + n*pi) | Avoid odd multiples of pi/2 on the real axis |
| `#DIV/0!` | Division by zero when cos(z) = 0 | Check for poles before computing |
| `#NAME?` | Function not recognized | Verify spelling; ensure Analysis ToolPak enabled in older Excel |
| `#VALUE!` | Empty or malformed input | Ensure proper complex notation |

## Use Cases

### [[Optical Calculations]]
- **Implementation**: Calculate refraction effects using `=IMSEC(COMPLEX(theta, absorption))` for complex incidence angles
- **Business Application**: Design optical coatings, laser systems, and polarization-sensitive devices
- **Technical Details**: Oblique incidence formulas involve sec(theta); complex extension handles absorbing media with complex refractive indices

### [[Transfer Function Analysis]]
- **Implementation**: Evaluate system responses where sec appears in closed-form solutions
- **Business Application**: Design filters and control systems with specific frequency characteristics
- **Technical Details**: Certain Chebyshev and elliptic filter designs involve secant functions; complex frequency analysis requires IMSEC

### [[Conformal Mapping]]
- **Implementation**: Apply sec(z) transformation for mapping specific domains
- **Business Application**: Solve potential problems in regions with appropriate geometries
- **Technical Details**: sec(z) provides conformal maps useful for certain boundary shapes; poles create interesting mapping behaviors

### [[Series Evaluation]]
- **Implementation**: Evaluate sums involving secant using IMSEC in partial fraction expansions
- **Business Application**: Compute special functions and infinite series in scientific applications
- **Technical Details**: Secant appears in Euler numbers and certain generating functions; complex extension broadens applicability

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

1. **Avoid poles**: sec(z) has poles at z = pi/2 + n*pi for all integers n. Check inputs before computing.

2. **Even function property**: sec(-z) = sec(z). Use this symmetry to simplify calculations.

3. **Decay for large imaginary**: |sec(a+bi)| approximately equals 2*e^(-|b|) for large |b|. Useful for asymptotic analysis.

4. **Build from IMCOS**: Verify results with `=IMDIV("1", IMCOS(z))` for identical output.

5. **Period**: sec(z + 2*pi) = sec(z). The function has period 2*pi.

6. **Relation to sech**: sec(iz) = sech(z). Convert between trigonometric and hyperbolic as needed.

7. **Near poles**: sec(z) approximately equals 1/(z - pi/2 - n*pi) near each pole.

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMCOS]] | Complex cosine - sec(z) = 1/cos(z) |
| [[IMCSC]] | Complex cosecant - 1/sin(z) |
| [[IMTAN]] | Complex tangent - sin(z)/cos(z) |
| [[IMSECH]] | Complex hyperbolic secant - 1/cosh(z) |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |
| [[IMDIV]] | Complex division for building sec from cos |

## Official Documentation

- **Microsoft Excel**: [IMSEC function](https://support.microsoft.com/en-us/office/imsec-function-6df11132-4411-4df4-a3dc-1f17372571b1)
- **Google Sheets**: [IMSEC function](https://support.google.com/docs/answer/9000355)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013-01 | Added as native function |
| Excel 2010 | 2010-06 | Not available |
| Google Sheets | 2018-01 | Added support for complex trigonometric functions |
| Excel 365 | 2020-01 | Dynamic array support added |
| Excel Online | 2013-06 | Full support in web version |

---

**Mathematical Background**: The complex secant is defined as:

sec(z) = 1/cos(z) = 2/(e^(iz) + e^(-iz))

For z = a + bi, the components are:
sec(a + bi) = [cos(a)cosh(b) + i*sin(a)sinh(b)] / |cos(a + bi)|^2

where |cos(a + bi)|^2 = cos^2(a)cosh^2(b) + sin^2(a)sinh^2(b) = (cos(2a) + cosh(2b))/2

Key properties:
- Even function: sec(-z) = sec(z)
- Period: 2*pi (sec(z + 2*pi) = sec(z))
- Poles: z = pi/2 + n*pi for all integers n (where cos(z) = 0)
- No zeros (since cos(z) never equals infinity)
- Asymptotic: |sec(a + bi)| approximately equals 2*e^(-|b|) for large |b|
- Derivative: d/dz sec(z) = sec(z)tan(z)
- Relation: sec(iz) = sech(z)
