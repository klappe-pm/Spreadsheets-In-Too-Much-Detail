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
- complex-exponentiation
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-power
- complex-exponentiation
- raise-complex-power
tags:
- engineering-function
- complex-numbers
- exponentiation
- power
- polar-form
---

# IMPOWER

## Description

IMPOWER raises a complex number to a real-valued power, computing z^n for complex z and real n. Using the polar form representation z = r*e^(i*theta), exponentiation becomes z^n = r^n * e^(i*n*theta), which means the magnitude is raised to the power n while the argument is multiplied by n. This elegant formula, derived from De Moivre's theorem, allows calculation of any real power of a complex number, including fractional and negative exponents.

Complex exponentiation is fundamental to understanding the roots of unity, polynomial solutions, and signal processing transforms. When n is a positive integer, z^n represents repeated multiplication: z^2 = z*z, z^3 = z*z*z, etc. For n = -1, IMPOWER computes the multiplicative inverse 1/z. Fractional powers like n = 0.5 compute roots (though IMSQRT is preferred for square roots). The key insight is that raising to power n multiplies the phase angle by n, which can produce rotation, phase wrapping, or even oscillation in iterative applications.

In electrical engineering, IMPOWER appears in transfer function analysis where system poles contribute terms like 1/(s-p)^n, and in computing powers of impedance ratios. Signal processing uses complex exponentiation for generating higher harmonics (z^n produces the nth harmonic of a unit circle phasor), analyzing system stability margins, and implementing polynomial evaluations. Control systems use it for analyzing repeated poles and calculating sensitivity functions.

Both Excel and Google Sheets implement IMPOWER identically, accepting a complex number string and a real number exponent. The function returns a complex number as a text string. For integer powers, results are exact (within floating-point precision). For fractional powers, the function returns the principal value (one specific root among potentially multiple roots). IMPOWER handles negative exponents correctly, computing z^(-n) = 1/(z^n). The special case z^0 returns 1 for any nonzero z, while 0^0 may vary by implementation.

## Syntax

> [!NOTE] Syntax
> ```
> IMPOWER(inumber, number)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber** | The complex number to be raised to a power. Text string format (e.g., "3+4i"). | Yes |
| **number** | The real-valued power (exponent) to which to raise the complex number. Can be positive, negative, integer, or fractional. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMPOWER("2+i", 2)` | "3+4i" | (2+i)^2 = 4+4i-1 = 3+4i |
| `=IMPOWER("1+i", 2)` | "2i" | (1+i)^2 = 1+2i-1 = 2i (magnitude 2, angle 90 degrees) |
| `=IMPOWER("3+4i", 0)` | "1" | Any nonzero number to power 0 equals 1 |
| `=IMPOWER("2+i", -1)` | "0.4-0.2i" | Reciprocal: 1/(2+i) = (2-i)/5 = 0.4-0.2i |
| `=IMPOWER("i", 2)` | "-1" | i^2 = -1 by definition |
| `=IMPOWER("i", 4)` | "1" | i^4 = (i^2)^2 = (-1)^2 = 1 (full rotation) |
| `=IMPOWER("1+i", 0.5)` | "1.099+0.455i" | Principal square root of 1+i |
| `=IMPOWER("2", 3)` | "8" | Real number: 2^3 = 8 |
| `=IMPOWER("-1", 0.5)` | "i" | sqrt(-1) = i (principal value) |
| `=IMPOWER("3+4j", 2)` | "−7+24j" | (3+4j)^2 = 9+24j-16 = -7+24j |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure inumber follows "a+bi" or "a+bj" format |
| `#NUM!` | Zero raised to zero (0^0) | This is mathematically undefined; handle specially |
| `#NUM!` | Zero raised to negative power | 0^(-n) is undefined (division by zero) |
| `#VALUE!` | Non-numeric exponent | Ensure the power is a numeric value |
| `#VALUE!` | Empty cell reference | Ensure referenced cells contain valid values |

## Use Cases

### 1. Calculating Powers of Impedance for Resonance Analysis

**Scenario**: An electrical engineer analyzes resonant circuits where the quality factor Q involves impedance ratios raised to various powers.

**Implementation**: Use IMPOWER to compute squared magnitude and other powers of complex impedances.

```
| Parameter | Value | Formula | Result |
|-----------|-------|---------|--------|
| Impedance Z | 30+40j ohm | Given | |
| Z^2 | | =IMPOWER("30+40j", 2) | -700+2400j |
| Z^(-1) (Admittance) | | =IMPOWER("30+40j", -1) | 0.012-0.016j |
| |Z|^2 | | =IMPOWER(IMABS("30+40j"), 2) | 2500 |

Quality factor Q = |Z_reactive| / R at resonance
For series RLC: Z = R + j(wL - 1/wC)
At resonance: w = 1/sqrt(LC), reactive part = 0
Power dissipation P = I^2 * |Z|^2 / R
```

### 2. Generating Harmonic Phasors for Signal Analysis

**Scenario**: A signal processing engineer generates harmonic components of a fundamental frequency by raising the unit phasor to integer powers.

**Implementation**: Use IMPOWER to create phasor representations of harmonics.

```
| Harmonic | Power n | Unit Phasor at 30 deg | Formula | Harmonic Phasor |
|----------|---------|----------------------|---------|-----------------|
| Fund (1st) | 1 | 0.866+0.5i | =IMPOWER(B2, 1) | 0.866+0.5i (30 deg) |
| 2nd | 2 | 0.866+0.5i | =IMPOWER(B3, 2) | 0.5+0.866i (60 deg) |
| 3rd | 3 | 0.866+0.5i | =IMPOWER(B4, 3) | 0+1i (90 deg) |
| 4th | 4 | 0.866+0.5i | =IMPOWER(B5, 4) | -0.5+0.866i (120 deg) |

Harmonic n has phase = n * fundamental_phase
Third harmonic of 30 degrees = 90 degrees
Useful for analyzing nonlinear distortion and harmonic content
```

### 3. Evaluating Polynomial Transfer Functions

**Scenario**: A control systems engineer evaluates a transfer function polynomial at a specific complex frequency point by computing powers of s = jw.

**Implementation**: Use IMPOWER to calculate (jw)^n terms for polynomial evaluation.

```
| Term | Power | s = j*10 | Formula | Value |
|------|-------|----------|---------|-------|
| s^0 | 0 | 10i | =IMPOWER("10i", 0) | 1 |
| s^1 | 1 | 10i | =IMPOWER("10i", 1) | 10i |
| s^2 | 2 | 10i | =IMPOWER("10i", 2) | -100 |
| s^3 | 3 | 10i | =IMPOWER("10i", 3) | -1000i |
| s^4 | 4 | 10i | =IMPOWER("10i", 4) | 10000 |

For H(s) = s^2 + 2s + 1 at s = 10i:
H(10i) = (10i)^2 + 2*(10i) + 1 = -100 + 20i + 1 = -99 + 20i
Powers of j cycle: j^1=j, j^2=-1, j^3=-j, j^4=1, j^5=j, ...
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Complex input | Text string | Text string |
| Exponent type | Real number | Real number |
| Output format | Text string | Text string |
| Fractional exponents | Supported (principal value) | Supported (principal value) |
| Negative exponents | Supported | Supported |
| 0^0 handling | May return #NUM! or 1 | May return #NUM! or 1 |
| Precision | 15 significant digits | 15 significant digits |

Both platforms implement IMPOWER identically for standard use cases. Edge cases like 0^0 may have platform-specific behavior.

## Tips and Best Practices

1. **Understand the polar form connection**: z^n means magnitude^n and argument*n. If z has magnitude 5 and angle 30 degrees, z^2 has magnitude 25 and angle 60 degrees.

2. **Use for integer powers efficiently**: For z^2, z^3, etc., IMPOWER is cleaner than nested IMPRODUCT calls: IMPOWER(z, 3) vs IMPRODUCT(z, z, z).

3. **Handle fractional powers carefully**: z^(1/n) returns one principal root. Complex numbers have n distinct nth roots, spaced 360/n degrees apart.

4. **Calculate reciprocals easily**: z^(-1) = 1/z. Use IMPOWER(z, -1) instead of IMDIV("1", z) for clarity when computing inverses.

5. **Watch for phase wrapping**: If z has angle 200 degrees, z^2 has angle 400 degrees = 40 degrees (mod 360). Large powers cause multiple rotations.

6. **Verify with magnitude**: |z^n| = |z|^n. Check that IMABS(IMPOWER(z,n)) equals IMABS(z)^n.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMSQRT]] | Square root (equivalent to IMPOWER with n=0.5) |
| [[IMPRODUCT]] | Multiplication (for small integer powers) |
| [[IMDIV]] | Division (equivalent to IMPOWER with n=-1 combined with IMPRODUCT) |
| [[IMEXP]] | Raises e to a complex power (different from IMPOWER) |

### Commonly Used With

- **[[IMABS]]** - Verify |z^n| = |z|^n
- **[[IMARGUMENT]]** - Verify arg(z^n) = n*arg(z)
- **[[IMSQRT]]** - Specifically for square roots (n=0.5)
- **[[IMPRODUCT]]** - Alternative for small integer powers
- **[[COMPLEX]]** - Create complex numbers to raise to powers
- **[[IMSUM]]** - Combine powered terms for polynomial evaluation

## Official Documentation

- [Microsoft Excel IMPOWER Documentation](https://support.microsoft.com/en-us/office/impower-function-210fd2f5-f8ff-4c6a-9d60-30e34fbdef39)
- [Google Sheets IMPOWER Documentation](https://support.google.com/docs/answer/3093211)

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
