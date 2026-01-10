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
- complex-subtraction
- electrical-engineering
- signal-processing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- complex-subtraction
- subtract-complex-numbers
- difference-complex
tags:
- engineering-function
- complex-numbers
- arithmetic
- subtraction
---

# IMSUB

## Description

IMSUB calculates the difference between two complex numbers by subtracting the second from the first. For complex numbers z1 = a1 + b1*i and z2 = a2 + b2*i, the difference is (a1 - a2) + (b1 - b2)*i. Like addition, subtraction operates component-wise on the real and imaginary parts independently. IMSUB is essential for calculating voltage drops across components, determining error signals in control systems, and computing differences between complex measurements.

Geometrically, complex subtraction in the complex plane represents vector subtraction: z1 - z2 points from the tip of z2 to the tip of z1 when both vectors start at the origin. Alternatively, subtracting z2 is equivalent to adding its negation: z1 - z2 = z1 + (-z2). This geometric interpretation is valuable for understanding phase differences, computing relative positions in signal space, and analyzing distance between complex points.

In electrical engineering, IMSUB is fundamental for calculating potential differences and voltage drops. When current flows through an impedance, the voltage drop V = I * Z must be subtracted from the source voltage to determine the remaining potential. In control systems, error signals (reference minus measured) often involve complex representations of setpoints and feedback. Signal processing uses subtraction for noise cancellation, differential measurements, and computing transfer function deviations.

Both Excel and Google Sheets implement IMSUB identically, accepting exactly two complex number arguments and returning their difference as a text string. Unlike IMSUM which accepts multiple arguments, IMSUB takes only two operands (minuend and subtrahend). The function accepts "i" or "j" notation and returns results in the notation of the first argument. To chain multiple subtractions, nest IMSUB calls or use IMSUM with negated values.

## Syntax

> [!NOTE] Syntax
> ```
> IMSUB(inumber1, inumber2)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **inumber1** | The complex number to subtract from (minuend). | Yes |
| **inumber2** | The complex number to subtract (subtrahend). | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=IMSUB("5+3i", "2+1i")` | "3+2i" | (5+3i) - (2+1i) = 3+2i |
| `=IMSUB("10+5j", "3-2j")` | "7+7j" | (10+5j) - (3-2j) = 7+7j (note: 5-(-2)=7) |
| `=IMSUB("8", "3+4i")` | "5-4i" | Real minus complex: (8+0i) - (3+4i) = 5-4i |
| `=IMSUB("6i", "2i")` | "4i" | Purely imaginary: 6i - 2i = 4i |
| `=IMSUB("3+4i", "3+4i")` | "0" | Any number minus itself = 0 |
| `=IMSUB("0", "3+4i")` | "-3-4i" | Negation: 0 - z = -z |
| `=IMSUB("5+3i", "0")` | "5+3i" | Subtracting zero: identity |
| `=IMSUB("-2+3i", "-5+1i")` | "3+2i" | (-2)-(-5)=3, (3)-(1)=2 |
| `=IMSUB(COMPLEX(10,20), COMPLEX(3,5))` | "7+15i" | Works with COMPLEX output |
| `=IMSUB("120+0j", "30+40j")` | "90-40j" | Voltage drop: V_source - V_drop |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid complex number format | Ensure both inputs follow "a+bi" or "a+bj" format |
| `#VALUE!` | Plain number input without text format | Convert numbers to complex strings using COMPLEX |
| `#VALUE!` | Empty cell reference | Ensure referenced cells contain valid complex strings |
| `#NUM!` | Spaces within complex number strings | Remove all spaces from input text |
| `#VALUE!` | More than two arguments provided | IMSUB takes exactly 2 arguments; use IMSUM for multiple |

## Use Cases

### 1. Voltage Drop Calculation in AC Circuits

**Scenario**: An electrical engineer calculates the voltage available at a load after accounting for voltage drops across transmission line impedance.

**Implementation**: Subtract the voltage drop (I*Z_line) from the source voltage to find load voltage.

```
| Parameter | Value | Formula |
|-----------|-------|---------|
| Source Voltage (V_s) | 480+0j V | Given (reference angle 0) |
| Line Current (I) | 20-10j A | Measured |
| Line Impedance (Z_line) | 2+5j ohm | From line specs |
| Voltage Drop (V_drop) | =IMPRODUCT("20-10j","2+5j") | 90+80j V |
| Load Voltage (V_load) | =IMSUB("480+0j", "90+80j") | 390-80j V |

|V_load| = IMABS("390-80j") = 398.1 V
Voltage regulation = (|V_s| - |V_load|) / |V_load| = (480-398.1)/398.1 = 20.6%
Phase shift at load: DEGREES(IMARGUMENT("390-80j")) = -11.6 degrees
```

### 2. Error Signal Computation in Feedback Control

**Scenario**: A control systems engineer computes the error signal by subtracting the measured feedback from the reference setpoint, both represented as complex phasors.

**Implementation**: Use IMSUB to calculate complex error that drives the controller.

```
| Signal | Value | Description |
|--------|-------|-------------|
| Reference (R) | 10+0j | Desired output phasor |
| Feedback (Y) | 8+3j | Measured output phasor |
| Error (E) | =IMSUB("10+0j", "8+3j") | 2-3j |

Error magnitude: |E| = IMABS("2-3j") = 3.61
Error phase: DEGREES(IMARGUMENT("2-3j")) = -56.3 degrees

The controller must provide gain and phase compensation
to drive the error toward zero.
Negative imaginary error indicates feedback is phase-leading reference.
```

### 3. Differential Signal Measurement

**Scenario**: A communications engineer measures the differential signal between two antenna elements for direction-finding or interference cancellation.

**Implementation**: Subtract signals from two sensors to extract differential information.

```
| Element | Received Signal | Formula |
|---------|-----------------|---------|
| Antenna A | 5.0+3.0i | Measured complex amplitude |
| Antenna B | 4.5+2.8i | Measured complex amplitude |
| Differential | =IMSUB(B2, B3) | 0.5+0.2i |

Differential magnitude: IMABS("0.5+0.2i") = 0.54
Differential phase: DEGREES(IMARGUMENT("0.5+0.2i")) = 21.8 degrees

Small differential indicates signals are similar (far-field source)
Large differential indicates signals differ (near-field or multipath)
Phase difference used for angle-of-arrival estimation
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Number of arguments | Exactly 2 | Exactly 2 |
| Input format | Text string | Text string |
| Output format | Text string | Text string |
| Accepts "i" suffix | Yes | Yes |
| Accepts "j" suffix | Yes | Yes |
| Zero result | "0" | "0" |
| Precision | 15 significant digits | 15 significant digits |

Both platforms implement IMSUB identically with no functional differences.

## Tips and Best Practices

1. **Order matters**: Unlike addition, subtraction is not commutative. IMSUB(a, b) ≠ IMSUB(b, a). The result is negated if you swap arguments.

2. **Chain subtractions carefully**: For a - b - c, use `=IMSUB(IMSUB(a, b), c)` or equivalently `=IMSUM(a, COMPLEX(-IMREAL(b),-IMAGINARY(b)), COMPLEX(-IMREAL(c),-IMAGINARY(c)))`.

3. **Create negation**: To negate a complex number, subtract it from zero: `=IMSUB("0", z)` returns -z.

4. **Verify with components**: Check: IMREAL(IMSUB(a,b)) = IMREAL(a) - IMREAL(b), and similarly for IMAGINARY.

5. **Magnitude of difference**: |z1 - z2| gives the "distance" between two complex numbers in the complex plane. Useful for error analysis.

6. **Relate to addition**: IMSUB(a, b) = IMSUM(a, -b). If you need to subtract multiple numbers, negate them and use IMSUM.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[IMSUM]] | Adds complex numbers (inverse operation) |
| [[IMPRODUCT]] | Multiplies complex numbers |
| [[IMDIV]] | Divides complex numbers |
| [[COMPLEX]] | Creates complex numbers for subtraction |

### Commonly Used With

- **[[IMSUM]]** - Addition: inverse operation of subtraction
- **[[IMABS]]** - Find magnitude of the difference (distance)
- **[[IMARGUMENT]]** - Find phase of the difference
- **[[IMPRODUCT]]** - Often used to compute values before subtraction (e.g., V = IZ)
- **[[COMPLEX]]** - Create complex numbers from components
- **[[IMCONJUGATE]]** - Sometimes needed in differential calculations

## Official Documentation

- [Microsoft Excel IMSUB Documentation](https://support.microsoft.com/en-us/office/imsub-function-2e404b4d-4935-4e85-9f52-cb08b9a45054)
- [Google Sheets IMSUB Documentation](https://support.google.com/docs/answer/3093217)

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
