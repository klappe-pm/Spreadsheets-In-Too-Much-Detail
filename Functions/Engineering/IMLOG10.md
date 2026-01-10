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
- decibels
- signal-processing
- bode-plots
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-log-base-10
- imaginary-log10
- complex-common-log
tags:
- engineering
- mathematics
- complex-numbers
- logarithm
- decibels
---

# IMLOG10

## IMLOG10 Description

**IMLOG10** returns the base-10 (common) logarithm of a complex number in the form "x+yi" or "x+yj". The complex base-10 logarithm is defined as log10(z) = ln(z)/ln(10), where ln(z) is the complex natural logarithm. For z = r*e^(i*theta) in polar form, this gives log10(z) = log10(r) + i*theta/ln(10) = log10|z| + i*arg(z)/ln(10).

The IMLOG10 function is particularly important in engineering applications where decibel scales are standard, including acoustics, electronics, and telecommunications. Since decibels are defined using base-10 logarithms (dB = 10*log10(power ratio) or 20*log10(amplitude ratio)), IMLOG10 provides direct access to log-magnitude values commonly used in Bode plots and frequency response analysis. The function also appears in scientific applications using order-of-magnitude calculations with complex quantities.

Like all complex logarithms, IMLOG10 inherits the multi-valued nature of IMLN, with the principal branch having imaginary part in the range (-pi/ln(10), pi/ln(10)] approximately equal to (-1.364, 1.364]. The function has a branch cut along the negative real axis and is undefined at z = 0. For negative real numbers, the imaginary part of the result equals pi/ln(10) approximately equal to 1.364. The real part equals the familiar base-10 log of the magnitude.

Both Excel and Google Sheets implement IMLOG10 using the principal branch, with identical syntax and precision. The function accepts complex numbers in "i" or "j" notation and returns results in "i" format. Both platforms return errors when input is zero. The conversion factor 1/ln(10) approximately equals 0.4343 relates IMLOG10 to IMLN.

## Syntax

```
IMLOG10(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the base-10 logarithm. Can be in "x+yi" or "x+yj" format, or a cell reference. Must not be zero. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMLOG10("10")` | 1 | log10(10) = 1. Standard base-10 log. |
| `=IMLOG10("100")` | 2 | log10(100) = 2. Power of 10. |
| `=IMLOG10("1")` | 0 | log10(1) = 0. |
| `=IMLOG10("1i")` | 0.682188176920921i | log10(i) = i*pi/(2*ln(10)) approximately equals 0.682i. |
| `=IMLOG10("-1")` | 1.36437635384184i | log10(-1) = i*pi/ln(10). Purely imaginary. |
| `=IMLOG10("10i")` | 1+0.682188176920921i | log10(10i) = 1 + i*pi/(2*ln(10)). |
| `=IMLOG10("1+1i")` | 0.150514997831991+0.34109408846046i | log10(1+i) = log10(sqrt(2)) + i*pi/(4*ln(10)). |
| `=IMLOG10("2+3i")` | 0.556971676153418+0.42682189085546i | Full complex log base 10. |
| `=IMLOG10("0.1")` | -1 | log10(0.1) = -1. |
| `=IMLOG10("0.01")` | -2 | log10(0.01) = -2. |
| `=IMLOG10("-10")` | 1+1.36437635384184i | log10(-10) = 1 + i*pi/ln(10). |
| `=IMLOG10(COMPLEX(100, 0))` | 2 | Using COMPLEX() for input. |
| `=IMLOG10("1000+0i")` | 3 | log10(1000) = 3. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input is zero | IMLOG10(0) is undefined; check for zero before computing |
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#NAME?` | Function not recognized | Check spelling; ensure Analysis ToolPak is enabled in older Excel |
| `#VALUE!` | Empty string or malformed input | Ensure proper complex number notation |

## Use Cases

### [[Decibel Calculations]]
- **Implementation**: Calculate dB magnitude using `=20*IMREAL(IMLOG10(H))` for voltage/amplitude ratios
- **Business Application**: Analyze audio systems, RF circuits, and telecommunications signal levels
- **Technical Details**: 20*log10|H| gives dB directly; for power ratios use 10*log10; IMLOG10 handles complex transfer functions

### [[Bode Plot Magnitude]]
- **Implementation**: Compute log-magnitude axis using `=IMREAL(IMLOG10(COMPLEX(Re_H, Im_H)))` for frequency response
- **Business Application**: Design and analyze filters, amplifiers, and control systems in electrical engineering
- **Technical Details**: Bode magnitude plot uses 20*log10|H(jw)|; phase plot uses arg(H(jw)); IMLOG10 provides both via real and imaginary parts

### [[Order of Magnitude Analysis]]
- **Implementation**: Estimate scales using `=ROUND(IMREAL(IMLOG10(z)), 0)` for complex quantities
- **Business Application**: Scientific computing, astronomical calculations, and engineering estimates
- **Technical Details**: Integer part of log10|z| gives order of magnitude; complex extension handles oscillatory quantities

### [[Acoustic Engineering]]
- **Implementation**: Calculate sound pressure level using `=20*IMREAL(IMLOG10(IMDIV(p, p_ref)))` for complex pressure
- **Business Application**: Design concert halls, noise barriers, and audio equipment with accurate SPL measurements
- **Technical Details**: SPL = 20*log10(|p|/p_ref) dB; complex extension handles phase in acoustic transfer functions

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Branch cut | Negative real axis | Negative real axis |
| Argument range | (-pi/ln(10), pi/ln(10)] | (-pi/ln(10), pi/ln(10)] |
| log10(0) handling | #NUM! error | #NUM! error |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |

## Tips and Best Practices

1. **Direct dB conversion**: For amplitude in dB, use `=20*IMREAL(IMLOG10(z))`. This is cleaner than using IMABS and LOG10 separately.

2. **Check for zero input**: Always validate: `=IF(IMABS(z)=0, "Undefined", IMLOG10(z))`.

3. **Relation to IMLN**: IMLOG10(z) = IMLN(z)/LN(10). Use this for verification or alternative computation.

4. **Phase in degrees**: The imaginary part gives phase/ln(10). For degrees: `=IMAGINARY(IMLOG10(z))*LN(10)*180/PI()`.

5. **Order of magnitude**: FLOOR(IMREAL(IMLOG10(z))) gives the power of 10 in the magnitude.

6. **Inverse operation**: 10^(IMLOG10(z)) = z. Compute with `=IMPOWER("10", IMLOG10(z))`.

7. **Product property**: log10(z1*z2) = log10(z1) + log10(z2) (with branch cut considerations).

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMLN]] | Complex natural logarithm - IMLOG10(z) = IMLN(z)/LN(10) |
| [[IMLOG2]] | Complex base-2 logarithm |
| [[IMEXP]] | Complex exponential - related through inverse |
| [[IMABS]] | Complex magnitude - 10^(Re(IMLOG10(z))) = |z| |
| [[IMARGUMENT]] | Complex argument - Im(IMLOG10(z))*ln(10) = arg(z) |
| [[LOG10]] | Real base-10 logarithm for comparison |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |

## Official Documentation

- **Microsoft Excel**: [IMLOG10 function](https://support.microsoft.com/en-us/office/imlog10-function-58200fca-e2a2-4271-8a98-ccd4360213a5)
- **Google Sheets**: [IMLOG10 function](https://support.google.com/docs/answer/3093424)

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

**Mathematical Background**: The complex base-10 logarithm is defined as:

log10(z) = ln(z)/ln(10) = log10|z| + i*arg(z)/ln(10)

For z = a + bi, this expands to:
log10(a + bi) = (1/2)*log10(a^2 + b^2) + i*atan2(b, a)/ln(10)

Key properties:
- Relationship: log10(z) = ln(z)/ln(10) approximately equals 0.4343*ln(z)
- Multi-valued: Full log10 has branches differing by 2*pi*i/ln(10) approximately equals 2.729i
- Principal branch: Imaginary part in (-pi/ln(10), pi/ln(10)] approximately equals (-1.364, 1.364]
- Branch cut: Along negative real axis
- Undefined at origin: log10(0) does not exist
- Derivative: d/dz log10(z) = 1/(z*ln(10))
- Product rule: log10(z1*z2) = log10(z1) + log10(z2) + 2*pi*i*k/ln(10)

Conversion factors:
- 1/ln(10) approximately equals 0.434294481903252
- pi/ln(10) approximately equals 1.36437635384184
- ln(10) approximately equals 2.302585092994046
