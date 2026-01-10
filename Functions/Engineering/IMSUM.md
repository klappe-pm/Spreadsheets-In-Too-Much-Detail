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
- complex-addition
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-addition
- add-complex-numbers
- sum-complex
tags:
- engineering-function
- complex-numbers
- arithmetic
- addition
---

# IMSUM

## Description

IMSUM calculates the sum of two or more complex numbers, adding them component-wise in rectangular form. For complex numbers z1 = a1 + b1*i and z2 = a2 + b2*i, the sum is (a1 + a2) + (b1 + b2)*i. This operation combines the real parts separately and the imaginary parts separately, producing a new complex number. IMSUM is the complex number equivalent of the standard SUM function, essential for combining impedances in parallel-series circuits, summing phasors in signal analysis, and aggregating complex data.

Complex addition has a clear geometric interpretation in the complex plane: placing complex numbers tip-to-tail as vectors, the sum is the vector from the origin to the final tip (parallelogram rule). This vector addition property makes IMSUM invaluable for combining multiple signal sources, calculating total current from branch currents, and determining net effects of multiple complex quantities. Unlike multiplication, addition in rectangular form is straightforward and does not require conversion to polar form.

In electrical engineering, IMSUM directly applies to series circuit analysis where impedances add: Z_total = Z1 + Z2 + Z3. For phasor addition in signal processing, multiple sinusoidal signals of the same frequency combine through complex addition of their phasor representations. The result's magnitude and phase (obtained via IMABS and IMARGUMENT) give the amplitude and phase of the combined signal. This principle underlies interference patterns, beam forming, and signal combining networks.

Both Excel and Google Sheets implement IMSUM identically, accepting multiple complex number arguments (up to 255 in Excel, up to 30 in Google Sheets) and returning the sum as a text string. The function accepts a mix of "i" and "j" notation in inputs but returns results using the notation of the first argument. IMSUM handles purely real, purely imaginary, and mixed complex numbers seamlessly. For summing arrays of complex numbers, you can pass a range reference containing complex number strings.

## Syntax

> [!NOTE] Syntax
> ```
> IMSUM(inumber1, [inumber2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber1** | The first complex number or range of complex numbers to add. | Yes |
| **inumber2, ...** | Additional complex numbers to add. Up to 255 arguments in Excel, 30 in Google Sheets. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMSUM("3+4i", "1+2i")` | "4+6i" | (3+4i) + (1+2i) = 4+6i |
| `=IMSUM("5+3j", "-2+4j")` | "3+7j" | Using "j" notation: (5+3j) + (-2+4j) = 3+7j |
| `=IMSUM("10", "5i")` | "10+5i" | Adding real and imaginary numbers |
| `=IMSUM("3+4i", "-3-4i")` | "0" | Additive inverse: z + (-z) = 0 |
| `=IMSUM("1+i", "2+2i", "3+3i")` | "6+6i" | Sum of three complex numbers |
| `=IMSUM("5")` | "5" | Single real number passes through |
| `=IMSUM("0", "3+4i")` | "3+4i" | Adding zero: identity element |
| `=IMSUM("-2.5+1.5i", "0.5-0.5i")` | "-2+i" | Decimal values: (-2.5+0.5) + (1.5-0.5)i = -2+i |
| `=IMSUM(A1:A5)` | Sum of range | Sums all complex numbers in the range |
| `=IMSUM("100+50j", "50-25j", "-30+10j")` | "120+35j" | Series impedances: 100+50+(-30) + (50-25+10)j |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format in any argument | Ensure all inputs follow "a+bi" or "a+bj" format |
| `#VALUE!` | Plain number input without text format | Convert numbers to complex strings or use COMPLEX function |
| `#VALUE!` | Empty cell in referenced range | Ensure all cells contain valid complex number strings |
| `#NUM!` | Spaces within complex number strings | Remove all spaces from complex number text |
| `#VALUE!` | Non-numeric text in range | Verify all range cells contain valid complex numbers |

## Use Cases

### 1. Series Impedance Calculation in AC Circuits

**Scenario**: An electrical engineer needs to calculate total impedance of components connected in series, where individual impedances simply add together.

**Implementation**: Use IMSUM to add complex impedances of series-connected components.

```
| Component | Value | Impedance (Z) | Formula |
|-----------|-------|---------------|---------|
| Resistor R | 100 ohm | 100+0j | =COMPLEX(100,0,"j") |
| Inductor L | 50 mH @ 1kHz | 0+314.16j | =COMPLEX(0, 2*PI()*1000*0.05, "j") |
| Capacitor C | 10 uF @ 1kHz | 0-15.92j | =COMPLEX(0, -1/(2*PI()*1000*0.00001), "j") |
| Total Series | | =IMSUM(B2:B4) | 100+298.24j |

Z_series = Z_R + Z_L + Z_C = 100 + j(314.16 - 15.92) = 100 + 298.24j ohms
|Z| = IMABS("100+298.24j") = 314.5 ohms
Phase = IMARGUMENT("100+298.24j") = 71.5 degrees (inductive)
```

### 2. Phasor Addition for Signal Combining

**Scenario**: A communications engineer combines multiple signal sources represented as phasors to determine the resultant signal amplitude and phase at a receiving antenna.

**Implementation**: Sum phasor representations of individual signals to find combined signal characteristics.

```
| Source | Amplitude | Phase (deg) | Phasor (rect form) | Formula |
|--------|-----------|-------------|-------------------|---------|
| TX1 | 5V | 0 | 5+0i | =COMPLEX(5*COS(RADIANS(0)), 5*SIN(RADIANS(0))) |
| TX2 | 3V | 60 | 1.5+2.6i | =COMPLEX(3*COS(RADIANS(60)), 3*SIN(RADIANS(60))) |
| TX3 | 4V | -45 | 2.83-2.83i | =COMPLEX(4*COS(RADIANS(-45)), 4*SIN(RADIANS(-45))) |
| Sum | | | =IMSUM(D2:D4) | 9.33-0.23i |

Combined amplitude: IMABS("9.33-0.23i") = 9.33V
Combined phase: DEGREES(IMARGUMENT("9.33-0.23i")) = -1.4 degrees
Constructive interference increased amplitude from max individual (5V) to 9.33V
```

### 3. Kirchhoff's Current Law with Complex Currents

**Scenario**: A power systems engineer applies Kirchhoff's Current Law (KCL) at a node, verifying that the sum of all complex currents entering and leaving equals zero.

**Implementation**: Sum all branch currents at a node; the result should be zero (or very small due to rounding).

```
| Branch | Direction | Current (A) | Formula |
|--------|-----------|-------------|---------|
| I1 | Into node | 10+5j | Measured |
| I2 | Into node | 5-8j | Measured |
| I3 | Out of node | -12+2j | Measured (negative = out) |
| I4 | Out of node | -3-(-1)j | Unknown, to calculate |

KCL: I1 + I2 + I3 + I4 = 0
Sum of known: =IMSUM("10+5j", "5-8j", "-12+2j") = "3-1j"
Therefore I4 = -(3-1j) = "-3+1j" to balance

Verification: =IMSUM("10+5j", "5-8j", "-12+2j", "-3+1j") = "0"
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum arguments | 255 | 30 |
| Range input | Supported | Supported |
| Mixed "i"/"j" input | Allowed (uses first arg's style) | Allowed (uses first arg's style) |
| Output format | Text string | Text string |
| Zero result display | "0" | "0" |
| Precision | 15 significant digits | 15 significant digits |

Both platforms implement IMSUM with the same mathematical behavior. The main difference is the maximum number of arguments (255 vs 30), which rarely matters in practice since ranges can be used for large datasets.

## Tips and Best Practices

1. **Use ranges for multiple values**: Instead of listing many arguments, put complex numbers in a column and use `=IMSUM(A1:A100)` for cleaner formulas.

2. **Verify with component extraction**: Check results by verifying: Real(Sum) = Sum of Reals, Imag(Sum) = Sum of Imaginaries using IMREAL and IMAGINARY.

3. **Handle additive inverses**: To subtract, add the negative: `=IMSUM(z1, COMPLEX(-IMREAL(z2), -IMAGINARY(z2)))` or better, use IMSUB directly.

4. **Consistent notation**: While IMSUM accepts mixed "i" and "j" notation, use consistent notation throughout your workbook to avoid confusion.

5. **Check for zero results**: A sum of "0" means perfect cancellation. In physical systems, this often indicates balanced conditions or errors to investigate.

6. **Series vs parallel**: Remember: series components add directly (IMSUM), but parallel components require the reciprocal formula: 1/Z_total = 1/Z1 + 1/Z2 (use IMDIV and IMSUM).

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMSUB]] | Subtracts one complex number from another |
| [[IMPRODUCT]] | Multiplies complex numbers (different from addition) |
| [[SUM]] | Adds real numbers only |
| [[COMPLEX]] | Creates complex numbers to then add |

### Commonly Used With

- **[[IMSUB]]** - Subtraction: inverse operation of addition
- **[[IMABS]]** - Find magnitude of the sum
- **[[IMARGUMENT]]** - Find phase angle of the sum
- **[[COMPLEX]]** - Create complex numbers from real/imaginary pairs
- **[[IMDIV]]** - For parallel impedance calculations
- **[[IMREAL]]** - Extract real part of sum for verification

## Official Documentation

- [Microsoft Excel IMSUM Documentation](https://support.microsoft.com/en-us/office/imsum-function-81542999-5f1c-4da6-9bd6-cbc5c5fc4b56)
- [Google Sheets IMSUM Documentation](https://support.google.com/docs/answer/3093216)

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
