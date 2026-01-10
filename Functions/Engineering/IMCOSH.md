---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
- complex-numbers
- hyperbolic-functions
subTopics:
- complex-hyperbolic
- signal-processing
- catenary-curves
- electrical-engineering
- transmission-lines
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-hyperbolic-cosine
- imaginary-cosh
tags:
- engineering
- mathematics
- complex-numbers
- hyperbolic
- transmission-lines
---

# IMCOSH

## IMCOSH Description

**IMCOSH** returns the hyperbolic cosine of a complex number in the form "x+yi" or "x+yj". For a complex number z = a + bi, the complex hyperbolic cosine is defined as cosh(z) = cosh(a)cos(b) + i*sinh(a)sin(b), which is the hyperbolic analog of the complex cosine function. The hyperbolic cosine is intimately connected to the exponential function through the identity cosh(z) = (e^z + e^(-z))/2 and relates to the regular cosine through cosh(z) = cos(iz).

The IMCOSH function is critical in engineering applications involving transmission lines, wave propagation, and structural analysis. In electrical engineering, the hyperbolic cosine appears in the voltage and current equations for lossless and lossy transmission lines. The catenary curve (the shape of a hanging cable) is described by cosh, and when damping or complex impedances are involved, the complex extension becomes necessary. IMCOSH is also fundamental in solving differential equations with complex coefficients.

Key considerations when using IMCOSH include understanding that it is an even function (cosh(-z) = cosh(z)) and that its magnitude can grow exponentially with both real and imaginary components of the input. Unlike the real cosh which is always greater than or equal to 1, the complex cosh can take any complex value. The function has zeros at z = i*(pi/2 + n*pi) for integer n, all located on the imaginary axis. The input must be a properly formatted complex number string.

Both Excel and Google Sheets implement IMCOSH with consistent behavior. The function accepts complex numbers in "i" or "j" notation and returns results using "i" notation. Edge cases are handled identically: purely real inputs return real hyperbolic cosine values, and purely imaginary inputs return cos of the imaginary coefficient (since cosh(bi) = cos(b)). Numerical precision is maintained at approximately 15 significant digits.

## Syntax

```
IMCOSH(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the hyperbolic cosine. Can be in "x+yi" or "x+yj" format, or a cell reference. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMCOSH("0")` | 1 | cosh(0) = 1. The hyperbolic cosine of zero is 1. |
| `=IMCOSH("1")` | 1.54308063481524 | cosh(1) approximately equals 1.543, the familiar real hyperbolic cosine. |
| `=IMCOSH("1i")` | 0.540302305868... | cosh(i) = cos(1) approximately equals 0.5403. Purely imaginary gives cosine of coefficient. |
| `=IMCOSH("3.14159i")` | -1 | cosh(pi*i) = cos(pi) = -1. |
| `=IMCOSH("1+1i")` | 0.833730025131149+0.988897705762865i | Full complex result with both real and imaginary parts. |
| `=IMCOSH("2+0i")` | 3.76219569108363 | cosh(2) approximately equals 3.762. Purely real input. |
| `=IMCOSH("0+1.5708i")` | ~0 | cosh(pi*i/2) = cos(pi/2) approximately equals 0. A zero of complex cosh. |
| `=IMCOSH("-1-1i")` | 0.833730025131149-0.988897705762865i | cosh(-z) = cosh(z) for real part; imaginary part changes sign. |
| `=IMCOSH("3")` | 10.0676619957778 | cosh(3) approximately equals 10.068. Exponential growth for real arguments. |
| `=IMCOSH(COMPLEX(1, 2))` | -0.64214812471552+1.06860742138278i | Using COMPLEX() for input construction. |
| `=IMCOSH("0.5+0.5i")` | 0.989816905227429+0.24982639750382i | Moderate complex argument. |
| `=IMCOSH("2+3i")` | -3.72454550491532+0.511822569987385i | Larger imaginary component affects both parts. |
| `=IMCOSH("5i")` | 0.283662185463226 | cosh(5i) = cos(5) approximately equals 0.284. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#VALUE!` | Empty string or non-numeric characters | Ensure input contains only valid numeric and complex notation |
| `#NUM!` | Extremely large real part causing overflow | Limit inputs; cosh grows as e^|x|/2 for large |x| |
| `#NAME?` | Function not recognized | Verify spelling; ensure Analysis ToolPak enabled in older Excel |
| `#VALUE!` | Cell reference contains non-complex data | Verify referenced cell contains valid complex number |

## Use Cases

### [[Transmission Line Analysis]]
- **Implementation**: Calculate voltage/current using `=IMCOSH(COMPLEX(alpha*x, beta*x))` where alpha is attenuation and beta is phase constant
- **Business Application**: Design power transmission lines, communication cables, and RF feed systems with accurate impedance matching
- **Technical Details**: The ABCD matrix for transmission lines involves cosh(gamma*l) where gamma = alpha + j*beta is complex propagation constant

### [[Catenary Curve Analysis with Complex Loads]]
- **Implementation**: Model suspension cable shapes using `=IMCOSH(COMPLEX(x/a, damping_factor))` for cables with dynamic loading
- **Business Application**: Design suspension bridges, cable-stayed structures, and overhead power lines with accurate sag calculations
- **Technical Details**: Complex extension handles time-varying loads and damped oscillations around equilibrium position

### [[Laplace Transform Solutions]]
- **Implementation**: Evaluate inverse transforms using `=IMCOSH(COMPLEX(sigma, omega))` for second-order systems
- **Business Application**: Analyze control systems, mechanical vibrations, and electrical circuit transients
- **Technical Details**: cosh appears in Laplace transforms of symmetric initial conditions; complex argument handles damped oscillatory modes

### [[Heat Conduction in Fins]]
- **Implementation**: Calculate temperature using `=T_base * IMREAL(IMDIV(IMCOSH(COMPLEX(m*(L-x), 0)), IMCOSH(COMPLEX(m*L, 0))))` for fin efficiency
- **Business Application**: Design heat sinks, radiator fins, and thermal management systems for electronics cooling
- **Technical Details**: Complex extension handles oscillatory boundary conditions or time-periodic heat sources

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2013+ native | Added in 2018 |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Real number input | Returns error | Returns error (must use "a+0i" format) |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |
| Overflow threshold | Approximately e^709 | Approximately e^709 |

## Tips and Best Practices

1. **Use proper input format**: Always provide input as "a+bi" or use COMPLEX(a, b). Bare real numbers cause errors.

2. **Even function property**: cosh(-z) = cosh(z). This can simplify calculations involving symmetric domains.

3. **Relation to cos**: cosh(z) = cos(i*z). Use this to convert between trigonometric and hyperbolic problems.

4. **Exponential form**: cosh(z) = (e^z + e^(-z))/2. For verification: `=IMDIV(IMSUM(IMEXP(z), IMEXP(IMSUB("0", z))), 2)`.

5. **Zeros location**: cosh(z) = 0 when z = i*(pi/2 + n*pi). All zeros are on the imaginary axis.

6. **Magnitude estimation**: |cosh(a+bi)| approximately equals cosh(a) for small b, and grows as e^|a|/2 for large |a|.

7. **Pair with IMSINH**: Many physical problems involve both cosh and sinh, such as general solutions to linear ODEs.

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMSINH]] | Complex hyperbolic sine - sinh(z) |
| [[IMCOS]] | Complex cosine - related by cos(z) = cosh(iz) |
| [[IMSECH]] | Complex hyperbolic secant - 1/cosh(z) |
| [[IMEXP]] | Complex exponential - cosh(z) = (e^z + e^(-z))/2 |
| [[IMLN]] | Complex natural logarithm - inverse relationship |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |
| [[COSH]] | Real hyperbolic cosine for comparison |

## Official Documentation

- **Microsoft Excel**: [IMCOSH function](https://support.microsoft.com/en-us/office/imcosh-function-053e4ddb-4122-458b-be9a-457c405e90ff)
- **Google Sheets**: [IMCOSH function](https://support.google.com/docs/answer/9000351)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013-01 | Added as native function |
| Excel 2010 | 2010-06 | Not available |
| Google Sheets | 2018-01 | Added support for complex hyperbolic functions |
| Excel 365 | 2020-01 | Dynamic array support added |
| Excel Online | 2013-06 | Full support in web version |

---

**Mathematical Background**: The complex hyperbolic cosine is defined as:

cosh(z) = (e^z + e^(-z)) / 2

For z = a + bi, this expands to:
cosh(a + bi) = cosh(a)cos(b) + i*sinh(a)sin(b)

Key properties:
- Even function: cosh(-z) = cosh(z)
- Relation to cosine: cosh(z) = cos(iz) and cos(z) = cosh(iz)
- Derivative: d/dz cosh(z) = sinh(z)
- Identity: cosh^2(z) - sinh^2(z) = 1 (hyperbolic Pythagorean identity)
- At origin: cosh(0) = 1, cosh'(0) = 0 (minimum for real arguments)
- Zeros: z = i*(pi/2 + n*pi) for integer n

The magnitude is given by:
|cosh(a+bi)| = sqrt(sinh^2(a) + cos^2(b)) = sqrt((cosh(2a) + cos(2b))/2)
