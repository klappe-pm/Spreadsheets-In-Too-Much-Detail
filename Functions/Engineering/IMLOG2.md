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
- binary-logarithm
- information-theory
- digital-signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-log-base-2
- imaginary-log2
- complex-binary-log
tags:
- engineering
- mathematics
- complex-numbers
- logarithm
- binary
- information-theory
---

# IMLOG2

## IMLOG2 Description

**IMLOG2** returns the base-2 (binary) logarithm of a complex number in the form "x+yi" or "x+yj". The complex base-2 logarithm is defined as log2(z) = ln(z)/ln(2), where ln(z) is the complex natural logarithm. For z = r*e^(i*theta) in polar form, this gives log2(z) = log2(r) + i*theta/ln(2) = log2|z| + i*arg(z)/ln(2).

The IMLOG2 function is essential in digital signal processing, information theory, and computer science applications where binary representations are fundamental. The base-2 logarithm measures information content in bits, determines the number of binary digits needed to represent a number, and appears in entropy calculations. When extended to complex numbers, IMLOG2 enables analysis of complex-valued signals in terms of binary scales, useful in FFT analysis and digital communication systems.

Like all complex logarithms, IMLOG2 inherits the multi-valued nature of IMLN, with the principal branch having imaginary part in the range (-pi/ln(2), pi/ln(2)] approximately equal to (-4.532, 4.532]. The function has a branch cut along the negative real axis and is undefined at z = 0. For negative real numbers, the imaginary part equals pi/ln(2) approximately equal to 4.532. The real part equals the familiar base-2 log of the magnitude.

Both Excel and Google Sheets implement IMLOG2 using the principal branch, with identical behavior and precision. The function accepts complex numbers in "i" or "j" notation and returns results in "i" format. Both platforms return errors when input is zero. The conversion factor 1/ln(2) approximately equal to 1.4427 relates IMLOG2 to IMLN.

## Syntax

```
IMLOG2(inumber)
```

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| **inumber** | A complex number for which you want the base-2 logarithm. Can be in "x+yi" or "x+yj" format, or a cell reference. Must not be zero. | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMLOG2("2")` | 1 | log2(2) = 1. Standard base-2 log. |
| `=IMLOG2("8")` | 3 | log2(8) = log2(2^3) = 3. |
| `=IMLOG2("1")` | 0 | log2(1) = 0. |
| `=IMLOG2("1i")` | 2.26618007091361i | log2(i) = i*pi/(2*ln(2)) approximately equals 2.266i. |
| `=IMLOG2("-1")` | 4.53236014182719i | log2(-1) = i*pi/ln(2). Purely imaginary. |
| `=IMLOG2("2i")` | 1+2.26618007091361i | log2(2i) = 1 + i*pi/(2*ln(2)). |
| `=IMLOG2("1+1i")` | 0.5+1.1330900354568i | log2(1+i) = log2(sqrt(2)) + i*pi/(4*ln(2)) = 0.5 + 1.133i. |
| `=IMLOG2("4+4i")` | 2.5+1.1330900354568i | log2(4+4i) = log2(4*sqrt(2)) + i*pi/4/ln(2). |
| `=IMLOG2("0.5")` | -1 | log2(0.5) = log2(2^-1) = -1. |
| `=IMLOG2("0.25")` | -2 | log2(0.25) = log2(2^-2) = -2. |
| `=IMLOG2("-2")` | 1+4.53236014182719i | log2(-2) = 1 + i*pi/ln(2). |
| `=IMLOG2("1024")` | 10 | log2(1024) = log2(2^10) = 10. |
| `=IMLOG2(COMPLEX(2, 2))` | 1.5+1.1330900354568i | Using COMPLEX() for input. |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input is zero | IMLOG2(0) is undefined; check for zero before computing |
| `#VALUE!` | Input is not a valid complex number format | Use proper format "a+bi" or "a+bj", or use COMPLEX(a, b) |
| `#NAME?` | Function not recognized | Check spelling; ensure Analysis ToolPak is enabled in older Excel |
| `#VALUE!` | Empty string or malformed input | Ensure proper complex number notation |

## Use Cases

### [[FFT Scaling Analysis]]
- **Implementation**: Determine FFT bin scaling using `=IMLOG2(COMPLEX(N, 0))` for transform size N
- **Business Application**: Optimize digital signal processing algorithms for audio, image, and communication systems
- **Technical Details**: FFT complexity is O(N*log2(N)); log2 determines number of butterfly stages; complex extension handles windowed transforms

### [[Information Theory Calculations]]
- **Implementation**: Compute entropy-related quantities using `=IMREAL(IMLOG2(probability_amplitude))` for complex-valued probabilities
- **Business Application**: Analyze communication channels, design coding schemes, and optimize data compression
- **Technical Details**: Information content in bits = -log2(probability); complex extension useful for quantum information contexts

### [[Binary Representation Analysis]]
- **Implementation**: Determine bit requirements using `=CEILING(IMREAL(IMLOG2(z)), 1)` for complex magnitudes
- **Business Application**: Design digital hardware, memory allocation, and data type selection for numerical systems
- **Technical Details**: Bits needed = ceiling(log2|z|) for representing magnitude; phase requires additional bits

### [[Octave Calculations in Audio]]
- **Implementation**: Compute frequency octaves using `=IMREAL(IMLOG2(IMDIV(f2, f1)))` for complex frequency ratios
- **Business Application**: Design audio equalizers, musical instruments, and acoustic analysis systems
- **Technical Details**: Each octave is a doubling of frequency (log2 ratio = 1); complex ratios encode phase differences

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2007+ native; 2003 requires Analysis ToolPak | Always available |
| Input format | "a+bi" or "a+bj" | "a+bi" or "a+bj" |
| Output format | Uses "i" suffix | Uses "i" suffix |
| Branch cut | Negative real axis | Negative real axis |
| Argument range | (-pi/ln(2), pi/ln(2)] | (-pi/ln(2), pi/ln(2)] |
| log2(0) handling | #NUM! error | #NUM! error |
| Precision | 15 significant digits | 15 significant digits |
| Array formula support | Yes (with CSE in older versions) | Yes (native) |

## Tips and Best Practices

1. **Check for zero input**: Always validate: `=IF(IMABS(z)=0, "Undefined", IMLOG2(z))`.

2. **Relation to IMLN**: IMLOG2(z) = IMLN(z)/LN(2). Use this for verification or manual computation.

3. **Relation to IMLOG10**: IMLOG2(z) = IMLOG10(z)/LOG10(2) = IMLOG10(z)*3.32193...

4. **Integer powers of 2**: For z = 2^n, IMLOG2(z) = n exactly. Useful for verification.

5. **Bits of precision**: IMREAL(IMLOG2(z)) tells you how many binary digits represent |z|.

6. **Inverse operation**: 2^(IMLOG2(z)) = z. Compute with `=IMPOWER("2", IMLOG2(z))`.

7. **Octave intervals**: Musical octave = frequency ratio of 2, so log2(f2/f1) gives octaves difference.

## Related Functions

| Function | Description |
|----------|-------------|
| [[IMLN]] | Complex natural logarithm - IMLOG2(z) = IMLN(z)/LN(2) |
| [[IMLOG10]] | Complex base-10 logarithm |
| [[IMEXP]] | Complex exponential - related through inverse |
| [[IMPOWER]] | Complex power function - 2^(IMLOG2(z)) = z |
| [[IMABS]] | Complex magnitude - 2^(Re(IMLOG2(z))) = |z| |
| [[LOG]] | Real logarithm with arbitrary base |
| [[COMPLEX]] | Creates complex numbers from real and imaginary parts |

## Official Documentation

- **Microsoft Excel**: [IMLOG2 function](https://support.microsoft.com/en-us/office/imlog2-function-152e13b4-bc79-486c-a243-e6a676878c51)
- **Google Sheets**: [IMLOG2 function](https://support.google.com/docs/answer/3093423)

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

**Mathematical Background**: The complex base-2 logarithm is defined as:

log2(z) = ln(z)/ln(2) = log2|z| + i*arg(z)/ln(2)

For z = a + bi, this expands to:
log2(a + bi) = (1/2)*log2(a^2 + b^2) + i*atan2(b, a)/ln(2)

Key properties:
- Relationship: log2(z) = ln(z)/ln(2) approximately equals 1.4427*ln(z)
- Multi-valued: Full log2 has branches differing by 2*pi*i/ln(2) approximately equals 9.0647i
- Principal branch: Imaginary part in (-pi/ln(2), pi/ln(2)] approximately equals (-4.532, 4.532]
- Branch cut: Along negative real axis
- Undefined at origin: log2(0) does not exist
- Derivative: d/dz log2(z) = 1/(z*ln(2))
- Product rule: log2(z1*z2) = log2(z1) + log2(z2) + 2*pi*i*k/ln(2)

Conversion factors:
- 1/ln(2) approximately equals 1.44269504088896
- pi/ln(2) approximately equals 4.53236014182719
- ln(2) approximately equals 0.693147180559945
- log2(10) approximately equals 3.32192809488736
