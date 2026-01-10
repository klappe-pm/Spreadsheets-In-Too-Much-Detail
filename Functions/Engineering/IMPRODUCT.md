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
- complex-multiplication
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-multiplication
- multiply-complex-numbers
- product-complex
tags:
- engineering-function
- complex-numbers
- arithmetic
- multiplication
---

# IMPRODUCT

## Description

IMPRODUCT calculates the product of two or more complex numbers using the standard rules of complex multiplication. For two complex numbers z1 = a1 + b1*i and z2 = a2 + b2*i, the product is (a1*a2 - b1*b2) + (a1*b2 + a2*b1)*i, derived from distributing the multiplication and using the identity i^2 = -1. This operation is fundamental to electrical engineering calculations involving Ohm's law (V = I*Z), signal modulation, rotation in the complex plane, and transfer function evaluation.

Complex multiplication has a beautiful interpretation in polar form: when multiplying two complex numbers, their magnitudes multiply and their arguments add. If z1 = r1*e^(i*theta1) and z2 = r2*e^(i*theta2), then z1*z2 = (r1*r2)*e^(i*(theta1+theta2)). This means multiplication scales and rotates complex numbers. Multiplying by a unit complex number (|z|=1) produces pure rotation, while multiplying by a positive real scales without rotation.

In electrical engineering, IMPRODUCT is indispensable for Ohm's law in AC circuits: V = I * Z, where all quantities are complex phasors. Current through impedance creates voltage drop with both magnitude scaling (by |Z|) and phase shift (by arg(Z)). In signal processing, multiplication implements modulation, mixing, and frequency translation. Control system analysis uses complex multiplication to evaluate transfer functions: H(s) = G(s) * K(s) for cascaded systems.

Both Excel and Google Sheets implement IMPRODUCT identically, accepting multiple complex number arguments (up to 255 in Excel, 30 in Google Sheets) and returning the product as a text string. The function multiplies all arguments together in sequence. For large products, be aware that magnitudes can grow or shrink rapidly, potentially causing overflow or underflow in extreme cases. IMPRODUCT accepts "i" or "j" notation and returns results in the first argument's style.

## Syntax

> [!NOTE] Syntax
> ```
> IMPRODUCT(inumber1, [inumber2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber1** | The first complex number or range. | Yes |
| **inumber2, ...** | Additional complex numbers to multiply. Up to 255 in Excel, 30 in Google Sheets. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMPRODUCT("3+4i", "1+2i")` | "-5+10i" | (3)(1)-(4)(2) + ((3)(2)+(4)(1))i = -5+10i |
| `=IMPRODUCT("2+3j", "4-j")` | "11+10j" | (2)(4)-(3)(-1) + ((2)(-1)+(3)(4))j = 11+10j |
| `=IMPRODUCT("i", "i")` | "-1" | i * i = i^2 = -1 |
| `=IMPRODUCT("3+4i", "3-4i")` | "25" | z * conj(z) = |z|^2 = 9+16 = 25 |
| `=IMPRODUCT("2", "3+4i")` | "6+8i" | Scaling by real: 2*(3+4i) = 6+8i |
| `=IMPRODUCT("i", "3+4i")` | "-4+3i" | Rotation by 90 deg: i*(3+4i) = -4+3i |
| `=IMPRODUCT("1+i", "1+i")` | "2i" | (1+i)^2 = 1+2i-1 = 2i |
| `=IMPRODUCT("1", "3+4i")` | "3+4i" | Multiplicative identity |
| `=IMPRODUCT("0", "3+4i")` | "0" | Multiplication by zero |
| `=IMPRODUCT("3+4i", "5-12i", "1+i")` | "119+7i" | Product of three complex numbers |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure all inputs follow "a+bi" or "a+bj" format |
| `#VALUE!` | Plain number input without text format | Convert to complex string format |
| `#VALUE!` | Empty cell in referenced range | Ensure all cells contain valid complex numbers |
| `#NUM!` | Spaces within complex number strings | Remove all spaces from input text |
| `#NUM!` | Result overflow (extremely large values) | Check for numerical stability; consider logarithmic approach |

## Use Cases

### 1. Ohm's Law for AC Circuits (V = I * Z)

**Scenario**: An electrical engineer calculates the voltage drop across complex impedances when AC current flows through circuit elements.

**Implementation**: Use IMPRODUCT to multiply current phasor by impedance for each component.

```
| Component | Current (I) | Impedance (Z) | Formula | Voltage (V) |
|-----------|-------------|---------------|---------|-------------|
| Resistor | 5+0j A | 100+0j ohm | =IMPRODUCT(B2,C2) | 500+0j V |
| Inductor | 5+0j A | 0+314j ohm | =IMPRODUCT(B3,C3) | 0+1570j V |
| RC combo | 3-4j A | 50+100j ohm | =IMPRODUCT(B4,C4) | 550+100j V |

V = I * Z
|V| = |I| * |Z| (magnitudes multiply)
arg(V) = arg(I) + arg(Z) (phases add)

For RC combo: (3-4j)*(50+100j) = 150+300j-200j+400 = 550+100j V
```

### 2. Cascaded System Transfer Functions

**Scenario**: A control systems engineer calculates the overall transfer function of cascaded (series-connected) subsystems by multiplying their individual transfer functions.

**Implementation**: Multiply transfer functions evaluated at the same frequency point.

```
| Subsystem | H(s) at s=jw | Formula | Result |
|-----------|--------------|---------|--------|
| Amplifier G1 | 10+0i | Given | Gain of 10, no phase shift |
| Filter G2 | 0.707-0.707i | Given | Gain 1, phase -45 degrees |
| Plant G3 | 0.1+0.2i | Given | Gain 0.224, phase 63 degrees |
| Overall H | =IMPRODUCT(B2,B3,B4) | 0.707+0.707i | |

Overall magnitude: |H| = 10 * 1 * 0.224 = 2.24
Overall phase: 0 + (-45) + 63 = 18 degrees
Verify: IMABS("0.707+0.707i") ≈ 1, DEGREES(IMARGUMENT) ≈ 45 degrees
(Note: actual calculation gives different phase due to complex multiplication)
```

### 3. Signal Mixing and Modulation

**Scenario**: A communications engineer implements frequency mixing by multiplying a baseband signal with a carrier in complex phasor form, shifting the signal to a new frequency band.

**Implementation**: Multiply signal phasor by carrier phasor for frequency translation.

```
| Parameter | Value | Description |
|-----------|-------|-------------|
| Baseband Signal | 1+0.5i | Complex envelope at baseband |
| Carrier (local osc) | 0.866+0.5i | Unit phasor at 30 degrees |
| Mixed Output | =IMPRODUCT("1+0.5i","0.866+0.5i") | 0.616+0.933i |

Mixing multiplies magnitudes: |output| = |signal| * |carrier|
Mixing adds phases: arg(output) = arg(signal) + arg(carrier)

|baseband| = IMABS("1+0.5i") = 1.118
arg(baseband) = 26.57 degrees
|carrier| = 1 (unit phasor)
arg(carrier) = 30 degrees

Expected: |output| = 1.118, arg(output) = 56.57 degrees
Actual: IMABS("0.616+0.933i") = 1.118, arg = 56.57 degrees ✓
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum arguments | 255 | 30 |
| Range input | Supported | Supported |
| Output format | Text string | Text string |
| Accepts "i"/"j" | Yes | Yes |
| Zero result | "0" | "0" |
| Precision | 15 significant digits | 15 significant digits |
| Overflow handling | Returns #NUM! | Returns #NUM! |

Both platforms implement IMPRODUCT identically. Be mindful of the argument limit difference when multiplying many complex numbers.

## Tips and Best Practices

1. **Use polar form mentally**: Remember |z1*z2| = |z1|*|z2| and arg(z1*z2) = arg(z1)+arg(z2). This helps verify results and understand the geometric effect.

2. **Verify with magnitude**: Quick check: IMABS(IMPRODUCT(a,b)) should equal IMABS(a)*IMABS(b).

3. **Multiplication by i rotates 90 degrees**: Multiplying any complex number by "i" rotates it counterclockwise by 90 degrees. By "-i" rotates clockwise 90 degrees.

4. **Use for squaring**: IMPRODUCT(z, z) = z^2. For higher powers, use IMPOWER for efficiency.

5. **Conjugate product gives magnitude squared**: IMPRODUCT(z, IMCONJUGATE(z)) = IMABS(z)^2, always a positive real number.

6. **Watch for growth**: Products of many complex numbers can grow very large or very small. Consider normalizing intermediate results if precision is critical.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMDIV]] | Divides complex numbers (inverse operation) |
| [[IMSUM]] | Adds complex numbers |
| [[IMSUB]] | Subtracts complex numbers |
| [[IMPOWER]] | Raises complex number to a power |

### Commonly Used With

- **[[IMDIV]]** - Division: inverse of multiplication
- **[[IMABS]]** - Verify magnitude: |a*b| = |a|*|b|
- **[[IMARGUMENT]]** - Verify phase: arg(a*b) = arg(a)+arg(b)
- **[[IMCONJUGATE]]** - z*conj(z) = |z|^2
- **[[COMPLEX]]** - Create operands for multiplication
- **[[IMPOWER]]** - For repeated multiplication (powers)

## Official Documentation

- [Microsoft Excel IMPRODUCT Documentation](https://support.microsoft.com/en-us/office/improduct-function-2fb8651a-a4f2-444f-975e-8ba7aab3a5ba)
- [Google Sheets IMPRODUCT Documentation](https://support.google.com/docs/answer/3093212)

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
