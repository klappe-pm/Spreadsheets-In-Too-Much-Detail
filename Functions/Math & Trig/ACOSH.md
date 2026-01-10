---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- hyperbolic-functions
- inverse-hyperbolic
- trigonometry
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Inverse Hyperbolic Cosine
- Arc Hyperbolic Cosine
- ARCCOSH
tags:
- hyperbolic
- trigonometry
- inverse-function
- engineering
- scientific
---

# ACOSH

## Description

**ACOSH (Inverse Hyperbolic Cosine)** returns the inverse hyperbolic cosine of a number. Mathematically, if `y = COSH(x)`, then `x = ACOSH(y)`. The function calculates the value whose hyperbolic cosine equals the input number. For any input value `x`, ACOSH returns `ln(x + sqrt(x^2 - 1))`, where `ln` is the natural logarithm.

Hyperbolic functions appear throughout engineering and physics, particularly in calculations involving catenary curves (hanging cables and chains), relativistic physics, heat transfer, and signal processing. ACOSH specifically is used when you need to "reverse" a hyperbolic cosine calculation—for instance, determining the parameter of a catenary curve from its shape, or solving certain differential equations that arise in electrical engineering.

**The critical constraint:** ACOSH only accepts values greater than or equal to 1. This is because the hyperbolic cosine function (COSH) only produces outputs >= 1. Attempting to calculate ACOSH of a value less than 1 returns a #NUM! error. This is mathematically correct—there's simply no real number whose hyperbolic cosine is less than 1. The function returns values >= 0, with ACOSH(1) = 0.

**Practical applications** are primarily in STEM fields. Unless you're working with physics simulations, engineering calculations, or advanced mathematical modeling, you may never encounter this function. However, for those specific applications, it's indispensable. Both Excel and Google Sheets implement ACOSH identically, making cross-platform work seamless for scientific calculations.

## Syntax

> [!f(x)] ACOSH Syntax
>
> ```
> =ACOSH(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value for which you want the inverse hyperbolic cosine. Must be a real number greater than or equal to 1. |

### Return Value

Returns the inverse hyperbolic cosine of the input number. The result is always a non-negative real number (>= 0). Returns #NUM! if the input is less than 1, and #VALUE! if the input is non-numeric.

## Examples

> [!f(x)] ACOSH Examples

### Example 1: Basic Calculation
```
=ACOSH(1)
```
**Result:** 0

**Explanation:** The hyperbolic cosine of 0 is 1, so the inverse hyperbolic cosine of 1 is 0. This is the minimum valid input and produces the minimum output.

---

### Example 2: Common Value
```
=ACOSH(2)
```
**Result:** 1.3169578969248166

**Explanation:** This is approximately ln(2 + sqrt(3)). The hyperbolic cosine of approximately 1.317 equals 2.

---

### Example 3: Larger Input
```
=ACOSH(10)
```
**Result:** 2.993222846126381

**Explanation:** For large values, ACOSH(x) approaches ln(2x). Here, ln(20) is about 2.996, which is close to our result.

---

### Example 4: Cell Reference
```
=ACOSH(A1)
```
Where A1 contains 5

**Result:** 2.2924316695611777

**Explanation:** References the value in cell A1 and returns its inverse hyperbolic cosine. Useful for calculations in larger worksheets.

---

### Example 5: Formula as Input
```
=ACOSH(1 + B1^2)
```
Where B1 contains 2

**Result:** 2.2924316695611777

**Explanation:** Since 1 + 4 = 5, this returns ACOSH(5). The expression guarantees the input is always >= 1 (since B1^2 >= 0).

---

### Example 6: Error Case - Value Less Than 1
```
=ACOSH(0.5)
```
**Result:** #NUM!

**Explanation:** No real number has a hyperbolic cosine of 0.5 (COSH always returns values >= 1), so ACOSH(0.5) is undefined in real numbers.

---

### Example 7: Error Handling with IFERROR
```
=IFERROR(ACOSH(A1), "Invalid input")
```
Where A1 contains 0.5

**Result:** "Invalid input"

**Explanation:** When A1 < 1, ACOSH returns #NUM!, which IFERROR catches and replaces with a friendly message.

---

### Example 8: Verifying with COSH
```
=COSH(ACOSH(5))
```
**Result:** 5

**Explanation:** Applying COSH to ACOSH reverses the operation, returning the original input. This identity can verify calculations.

---

### Example 9: Engineering Application - Catenary Parameter
```
=ACOSH(A1/B1)
```
Where A1 = 150 (cable height at support), B1 = 100 (minimum cable height)

**Result:** 0.9624236501192069

**Explanation:** In catenary calculations, this ratio helps determine the cable's shape parameter. Used in suspension bridge and power line analysis.

---

### Example 10: Array of Values (Excel 365/Sheets)
```
=ACOSH({1, 2, 5, 10})
```
**Result:** {0, 1.317, 2.292, 2.993} (approximately)

**Explanation:** Modern spreadsheet versions can apply ACOSH to an array, returning corresponding results for each input value.

---

### Example 11: Combined with Other Hyperbolic Functions
```
=ASINH(SQRT(ACOSH(A1)^2 - 1))
```
Where A1 = 3

**Result:** 1.7627471740390859

**Explanation:** Complex hyperbolic identity calculations used in advanced engineering. Demonstrates ACOSH integration with other functions.

---

### Example 12: Conditional Calculation
```
=IF(A1 >= 1, ACOSH(A1), "Value must be >= 1")
```
Where A1 contains 1.5

**Result:** 0.9624236501192069

**Explanation:** Pre-validates input before calling ACOSH, providing a user-friendly message instead of #NUM! for invalid inputs.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input value is less than 1 | Ensure input is >= 1. Use MAX(1, value) or IF to handle edge cases. |
| `#VALUE!` | Input is non-numeric (text or empty cell) | Verify input is a number. Use ISNUMBER() to validate before calculation. |
| `#NAME?` | Function name misspelled | Check spelling. Ensure it's ACOSH, not ARCOSH or ARCCOSH. |
| `Unexpected result` | Confusing ACOSH with ACOS | ACOSH is inverse hyperbolic cosine; ACOS is inverse circular cosine. They're different functions. |
| `#REF!` | Referenced cell was deleted | Restore the cell or update the reference. |

## Use Cases

### [[Catenary Curve Analysis]]

**Scenario:** Calculate the shape parameter of a hanging cable or chain given its sag and span measurements.

**Implementation:**
```
=ACOSH(1 + (sag / half_span)^2 / 2)
```

**Business Application:** Power line engineering, suspension bridge design, cable-stayed structures. Determines cable tension and optimal support placement for infrastructure projects.

**Technical Details:** The catenary equation uses COSH, so finding cable parameters from observed shapes requires ACOSH. Critical for ensuring structural safety margins.

---

### [[Relativistic Physics Calculations]]

**Scenario:** Calculate rapidity from velocity in special relativity, where rapidity is the inverse hyperbolic tangent of velocity ratio.

**Implementation:**
```
=ACOSH(1/SQRT(1 - (v/c)^2))
```
Where v = velocity, c = speed of light

**Business Application:** Particle physics research, aerospace engineering for high-speed vehicles, GPS satellite calculations requiring relativistic corrections.

**Technical Details:** Rapidity is additive for collinear velocities (unlike velocity itself), making ACOSH useful for combining relativistic effects.

---

### [[Signal Processing - Filter Design]]

**Scenario:** Calculate parameters for Chebyshev filters used in electronic signal processing.

**Implementation:**
```
=ACOSH(1/ripple_factor) / filter_order
```

**Business Application:** Audio equipment design, telecommunications, medical device signal filtering. Determines filter characteristics for desired frequency response.

**Technical Details:** Chebyshev polynomials use hyperbolic functions for passband calculations. ACOSH helps determine the order needed for specific attenuation.

---

### [[Heat Transfer Calculations]]

**Scenario:** Solve heat conduction problems in extended surfaces (fins) where temperature distribution follows hyperbolic functions.

**Implementation:**
```
=ACOSH(temperature_ratio) * fin_parameter
```

**Business Application:** HVAC system design, electronic cooling solutions, industrial heat exchanger optimization.

**Technical Details:** Fin efficiency calculations often involve hyperbolic functions. ACOSH helps determine fin length needed for target heat dissipation.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later (part of Math and Trig functions)
- **Precision:** 15 significant digits
- **Array support:** Excel 365 supports dynamic arrays; older versions require Ctrl+Shift+Enter for array formulas
- **Calculation:** Uses IEEE 754 double-precision floating-point

### Google Sheets
- **Availability:** All versions
- **Precision:** 15 significant digits
- **Array support:** Native array support without special entry
- **Calculation:** Identical to Excel's implementation

### Both Platforms
- Function name and syntax are identical
- Same domain restriction (input >= 1)
- Same return value range (>= 0)
- Identical error behaviors

## Tips and Best Practices

1. **Always validate inputs:** Since ACOSH requires values >= 1, use `=IF(A1>=1, ACOSH(A1), "Error")` or `=IFERROR(ACOSH(A1), NA())` to handle edge cases gracefully.

2. **Remember the domain:** ACOSH is only defined for x >= 1. This isn't a bug—it's mathematically correct. If your calculation produces values < 1, revisit your formula logic.

3. **Use for reversing COSH:** The primary use of ACOSH is "undoing" COSH. If you need the input that would produce a known COSH output, ACOSH is your function.

4. **Don't confuse with ACOS:** ACOS is the inverse of COS (circular cosine), while ACOSH is the inverse of COSH (hyperbolic cosine). They're fundamentally different functions with different domains and ranges.

5. **Verify with roundtrip:** Test your calculations with `=COSH(ACOSH(x))`, which should return x. If it doesn't, investigate floating-point precision issues.

6. **Combine with error handling:** For user-facing spreadsheets, always wrap ACOSH in error handling to prevent confusing #NUM! errors from appearing.

7. **Consider numerical stability:** For very large inputs, ACOSH(x) approaches ln(2x). If precision issues arise with huge values, consider using this approximation.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COSH]] | Returns the hyperbolic cosine | When you have the parameter and need the ratio |
| [[ASINH]] | Returns the inverse hyperbolic sine | For SINH reversal calculations |
| [[ATANH]] | Returns the inverse hyperbolic tangent | For TANH reversal calculations |
| [[ACOS]] | Returns the inverse circular cosine | For regular trigonometry (different function!) |

### Commonly Used Together

**[[COSH]]** - Hyperbolic cosine

*Verify ACOSH calculations:*
```
=COSH(ACOSH(A1))  // Should return A1
```
COSH and ACOSH are inverse functions—using both verifies your result.

---

**[[SQRT]]** - Square root

*In catenary and physics formulas:*
```
=ACOSH(SQRT(A1^2 + 1))
```
Many hyperbolic identities involve square roots combined with ACOSH.

---

**[[LN]]** - Natural logarithm

*Alternative calculation method:*
```
=LN(A1 + SQRT(A1^2 - 1))  // Equivalent to ACOSH(A1)
```
For educational purposes or when ACOSH isn't available.

---

**[[IFERROR]]** - Error handling

*Graceful error management:*
```
=IFERROR(ACOSH(A1), "Input must be >= 1")
```
Essential for user-facing spreadsheets where invalid inputs might occur.

## Official Documentation

- **Microsoft Excel:** [ACOSH function](https://support.microsoft.com/en-us/office/acosh-function-e3992cc1-103f-4e72-9f04-624b9ef5ebfe)
- **Google Sheets:** [ACOSH function](https://support.google.com/docs/answer/3093391)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Added as part of expanded Math and Trig library |
| Excel 365 | Ongoing | Dynamic array support added |
| Google Sheets | Original launch | Available since Sheets inception |

---

*Last updated: 2026-01-10*
