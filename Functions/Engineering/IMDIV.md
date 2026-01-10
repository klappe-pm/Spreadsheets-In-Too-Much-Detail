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
- complex-division
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-division
- divide-complex-numbers
- quotient-complex
tags:
- engineering-function
- complex-numbers
- arithmetic
- division
---

# IMDIV

## Description

IMDIV calculates the quotient of two complex numbers by dividing the first by the second. For complex numbers z1 = a1 + b1*i and z2 = a2 + b2*i, division is computed by multiplying both numerator and denominator by the conjugate of the denominator: z1/z2 = (z1 * conj(z2)) / |z2|^2. This rationalization technique eliminates the complex denominator, yielding a complex result with real and imaginary parts. Division is essential for calculating admittance from impedance, transfer functions, reflection coefficients, and normalized quantities.

In polar form, complex division has an elegant interpretation: magnitudes divide and arguments subtract. If z1 = r1*e^(i*theta1) and z2 = r2*e^(i*theta2), then z1/z2 = (r1/r2)*e^(i*(theta1-theta2)). This means division scales inversely and rotates in the opposite direction compared to the divisor. A complex number divided by itself equals 1 (unity), and dividing by a unit complex number produces pure rotation in the negative direction.

In electrical engineering, IMDIV is fundamental for calculating admittance (Y = 1/Z), current from voltage and impedance (I = V/Z), and transfer functions (H = Vout/Vin). RF engineers use it for computing reflection coefficients and S-parameters. In signal processing, division normalizes signals, computes frequency responses, and implements deconvolution. Control systems use it for calculating inverse transfer functions and loop gains.

Both Excel and Google Sheets implement IMDIV identically, accepting exactly two complex number arguments and returning the quotient as a text string. Division by zero (including "0", "0+0i") produces an error. The function accepts "i" or "j" notation and returns results in the first argument's notation style. For chained divisions, nest IMDIV calls: a/b/c = IMDIV(IMDIV(a,b),c).

## Syntax

> [!NOTE] Syntax
> ```
> IMDIV(inumber1, inumber2)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber1** | The complex number to be divided (dividend or numerator). | Yes |
| **inumber2** | The complex number to divide by (divisor or denominator). Must not be zero. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMDIV("10+5i", "2+i")` | "5+0i" or "5" | (10+5i)/(2+i) = (10+5i)(2-i)/5 = (25)/5 = 5 |
| `=IMDIV("4+3j", "1-2j")` | "-0.4+2.2j" | Rationalize: multiply by (1+2j)/(1+2j) |
| `=IMDIV("1", "i")` | "-i" | 1/i = -i (since i*(-i) = 1) |
| `=IMDIV("3+4i", "3+4i")` | "1" | Any nonzero number divided by itself = 1 |
| `=IMDIV("6+8i", "2")` | "3+4i" | Dividing by real: scales both parts |
| `=IMDIV("10", "2+i")` | "4-2i" | 10/(2+i) = 10(2-i)/5 = 4-2i |
| `=IMDIV("3+4i", "5")` | "0.6+0.8i" | (3+4i)/5 = 0.6+0.8i |
| `=IMDIV("0", "3+4i")` | "0" | Zero divided by nonzero = 0 |
| `=IMDIV("i", "-i")` | "-1" | i/(-i) = -i^2/(-i*i) = -(-1)/1 = -1 |
| `=IMDIV(COMPLEX(100,0,"j"), COMPLEX(50,50,"j"))` | "1-1j" | 100/(50+50j) = 1-1j |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Division by zero ("0" or "0+0i") | Check divisor is nonzero before dividing |
| `#NUM!` | Invalid complex number format | Ensure both inputs follow "a+bi" or "a+bj" format |
| `#VALUE!` | Plain number input without text format | Convert to complex string format |
| `#VALUE!` | Empty cell reference | Ensure referenced cells contain valid complex strings |
| `#DIV/0!` | Divisor evaluates to zero in some contexts | Verify divisor has nonzero magnitude |

## Use Cases

### 1. Impedance to Admittance Conversion

**Scenario**: An electrical engineer converts complex impedance values to admittance for parallel circuit analysis, where admittances add directly.

**Implementation**: Use IMDIV to compute Y = 1/Z for each element.

```
| Component | Impedance Z (ohms) | Formula | Admittance Y (siemens) |
|-----------|-------------------|---------|------------------------|
| Resistor | 100+0j | =IMDIV("1", B2) | 0.01+0j |
| Inductor | 0+50j | =IMDIV("1", B3) | 0-0.02j |
| RC Series | 30+40j | =IMDIV("1", B4) | 0.012-0.016j |

Y = 1/Z = G + jB
G = conductance (real part)
B = susceptance (imaginary part)

For RC: |Z| = 50 ohms, |Y| = 0.02 siemens
Y = (30-40j)/(30^2+40^2) = (30-40j)/2500 = 0.012-0.016j
Negative susceptance indicates capacitive behavior
```

### 2. Transfer Function Calculation

**Scenario**: A control systems engineer calculates the closed-loop transfer function from open-loop components, requiring division of complex polynomial evaluations.

**Implementation**: Evaluate and divide complex transfer function values at specific frequencies.

```
| Frequency | Numerator N(jw) | Denominator D(jw) | Formula | H(jw) = N/D |
|-----------|-----------------|-------------------|---------|-------------|
| 1 rad/s | 10+0i | 1+10i | =IMDIV(B2,C2) | 0.099-0.99i |
| 10 rad/s | 10+0i | 1+100i | =IMDIV(B3,C3) | 0.001-0.1i |
| 100 rad/s | 10+0i | 1+1000i | =IMDIV(B4,C4) | 0.00001-0.01i |

For H(s) = 10/(1+s/1) at s = jw:
At w=1: H = 10/(1+j) = 10(1-j)/2 = 5-5j ≈ magnitude 7.07, phase -45 degrees
Magnitude decreases with frequency (low-pass behavior)
Phase approaches -90 degrees at high frequency
```

### 3. Reflection Coefficient from Impedance

**Scenario**: An RF engineer calculates the reflection coefficient (Gamma) at a load to assess impedance matching quality and determine VSWR.

**Implementation**: Use IMDIV for the reflection coefficient formula Gamma = (Z_L - Z_0)/(Z_L + Z_0).

```
| Load | Z_L (ohms) | Z_0 (ohms) | Formula for Gamma | Gamma | |Gamma| | VSWR |
|------|------------|------------|-------------------|-------|--------|------|
| 1 | 75+0j | 50 | =IMDIV(IMSUB(B2,"50"),IMSUM(B2,"50")) | 0.2+0j | 0.2 | 1.5 |
| 2 | 50+50j | 50 | =IMDIV(IMSUB(B3,"50"),IMSUM(B3,"50")) | 0.2+0.4i | 0.447 | 2.62 |
| 3 | 25-25j | 50 | =IMDIV(IMSUB(B4,"50"),IMSUM(B4,"50")) | -0.2-0.4i | 0.447 | 2.62 |

Gamma = (Z_L - Z_0)/(Z_L + Z_0)
|Gamma| = 0 means perfect match
|Gamma| = 1 means total reflection
VSWR = (1 + |Gamma|)/(1 - |Gamma|)

For matched load (Z_L = Z_0): Gamma = 0, VSWR = 1
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Number of arguments | Exactly 2 | Exactly 2 |
| Division by zero | #NUM! error | #NUM! error |
| Input format | Text string | Text string |
| Output format | Text string | Text string |
| Precision | 15 significant digits | 15 significant digits |
| Accepts "i"/"j" | Yes | Yes |

Both platforms implement IMDIV identically with no functional differences.

## Tips and Best Practices

1. **Check for zero divisor**: Before dividing, verify the divisor is nonzero: `=IF(IMABS(divisor)=0, "Error", IMDIV(dividend, divisor))`.

2. **Use polar form for understanding**: |z1/z2| = |z1|/|z2| and arg(z1/z2) = arg(z1) - arg(z2). This helps verify results.

3. **Division by real is scaling**: IMDIV("a+bi", "r") = "a/r + (b/r)i". Simpler than general complex division.

4. **Invert using IMDIV**: The multiplicative inverse of z is IMDIV("1", z). Useful for converting impedance to admittance.

5. **Chain divisions correctly**: For a/b/c, use IMDIV(IMDIV(a,b),c). Note that (a/b)/c ≠ a/(b/c) in general.

6. **Verify with multiplication**: IMPRODUCT(IMDIV(a,b), b) should return a (within rounding precision).

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMPRODUCT]] | Multiplies complex numbers (inverse operation) |
| [[IMSUM]] | Adds complex numbers |
| [[IMSUB]] | Subtracts complex numbers |
| [[IMPOWER]] | Raises complex number to a power (including -1 for inverse) |

### Commonly Used With

- **[[IMPRODUCT]]** - Multiplication: inverse of division; verify a = (a/b)*b
- **[[IMABS]]** - Verify magnitude: |a/b| = |a|/|b|
- **[[IMARGUMENT]]** - Verify phase: arg(a/b) = arg(a) - arg(b)
- **[[IMCONJUGATE]]** - Used internally for rationalization
- **[[IMSUM]]** / **[[IMSUB]]** - Often needed for numerator/denominator formation
- **[[COMPLEX]]** - Create operands for division

## Official Documentation

- [Microsoft Excel IMDIV Documentation](https://support.microsoft.com/en-us/office/imdiv-function-a505aff7-af8a-4451-8142-77ec3d74d83f)
- [Google Sheets IMDIV Documentation](https://support.google.com/docs/answer/3093205)

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
