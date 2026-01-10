---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- gamma-function
- logarithm
- high-precision
- numerical-stability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Precise Log Gamma
- High-Precision GAMMALN
- Ln Gamma Precise
tags:
- statistical
- gamma
- logarithm
- precision
- numerical-stability
---

# GAMMALN.PRECISE

## Description

**GAMMALN.PRECISE** returns the natural logarithm of the gamma function, ln(Gamma(x)), with enhanced numerical precision compared to the standard GAMMALN function. This function is designed for applications requiring maximum accuracy, particularly for arguments near 1 and 2 where the standard GAMMALN implementation may lose precision. For most practical purposes, GAMMALN and GAMMALN.PRECISE return identical results, but the PRECISE variant uses a more sophisticated algorithm to minimize rounding errors in edge cases.

The mathematical definition is identical to GAMMALN: for any positive real number x, GAMMALN.PRECISE(x) = ln(Gamma(x)). Since Gamma(n) = (n-1)! for positive integers, GAMMALN.PRECISE(n) = ln((n-1)!). Key reference values include: GAMMALN.PRECISE(1) = 0 exactly (since Gamma(1) = 1 and ln(1) = 0), GAMMALN.PRECISE(2) = 0 exactly (since Gamma(2) = 1), and GAMMALN.PRECISE(0.5) = ln(sqrt(pi)) approximately 0.5723649429.

**Why precision matters:** The gamma function has zeros of its logarithm at x = 1 and x = 2, where Gamma equals exactly 1. Near these points, the standard GAMMALN computation involves subtracting nearly equal numbers, causing catastrophic cancellation and precision loss. GAMMALN.PRECISE uses series expansions and special algorithms near these critical points to maintain full 15-digit precision. For scientific computing, financial modeling with extreme parameters, and numerical analysis validation, this enhanced precision can be essential.

**When to use GAMMALN.PRECISE:** Use this function when (1) your calculations involve arguments near 1 or 2 and require maximum accuracy, (2) you're validating numerical algorithms and need reference values, (3) your application involves iterative calculations where small errors could compound, or (4) you're implementing statistical methods where precision near the mode matters. For typical business applications with arguments far from 1 and 2, standard GAMMALN is sufficient and identical in results.

## Syntax

> [!f(x)] GAMMALN.PRECISE Syntax
>
> ```
> =GAMMALN.PRECISE(x)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The positive value at which to evaluate the natural logarithm of the gamma function. Must be strictly positive (x > 0). |

### Return Value

Returns the natural logarithm of Gamma(x) with enhanced precision. Result can be any real number (negative for x between 1 and 2). Returns #NUM! if x <= 0. Returns #VALUE! for non-numeric input.

## Examples

> [!f(x)] GAMMALN.PRECISE Examples

### Example 1: GAMMALN.PRECISE(1) = 0 Exactly
```
=GAMMALN.PRECISE(1)
```
**Result:** 0

**Explanation:** Gamma(1) = 1, and ln(1) = 0. This is returned with full precision.

---

### Example 2: GAMMALN.PRECISE(2) = 0 Exactly
```
=GAMMALN.PRECISE(2)
```
**Result:** 0

**Explanation:** Gamma(2) = 1! = 1, and ln(1) = 0. Another critical point where precision matters.

---

### Example 3: Value Between 1 and 2 (Negative Result)
```
=GAMMALN.PRECISE(1.5)
```
**Result:** -0.120782 (approximately)

**Explanation:** For x between 1 and 2, Gamma(x) < 1, so ln(Gamma(x)) < 0. This region requires careful computation.

---

### Example 4: Factorial Relationship
```
=GAMMALN.PRECISE(6)
```
**Result:** 4.787 (approximately, = ln(5!) = ln(120))

**Explanation:** For integers, GAMMALN.PRECISE(n) = ln((n-1)!). Same as standard GAMMALN.

---

### Example 5: Recovering GAMMA from GAMMALN.PRECISE
```
=EXP(GAMMALN.PRECISE(5))
```
**Result:** 24 (= Gamma(5) = 4!)

**Explanation:** Exponentiating GAMMALN.PRECISE gives the gamma function value.

---

### Example 6: GAMMALN.PRECISE(0.5) = ln(sqrt(pi))
```
=GAMMALN.PRECISE(0.5)
```
**Result:** 0.5724 (approximately)

**Explanation:** Gamma(0.5) = sqrt(pi), so GAMMALN.PRECISE(0.5) = ln(sqrt(pi)) = 0.5 * ln(pi).

---

### Example 7: Compare with Standard GAMMALN
```
=GAMMALN.PRECISE(1.0001)
=GAMMALN(1.0001)
```
**Result:** Both return approximately -0.0000577 (may differ in last digits)

**Explanation:** Near x = 1, GAMMALN.PRECISE may provide a few more correct digits than GAMMALN.

---

### Example 8: Large Argument (No Overflow)
```
=GAMMALN.PRECISE(200)
```
**Result:** 857.93 (approximately)

**Explanation:** Same as GAMMALN - handles large arguments without overflow.

---

### Example 9: Very Small Positive Argument
```
=GAMMALN.PRECISE(0.001)
```
**Result:** 6.906 (approximately)

**Explanation:** As x approaches 0, Gamma(x) approaches infinity, so GAMMALN.PRECISE approaches infinity.

---

### Example 10: Negative Argument (Error)
```
=GAMMALN.PRECISE(-1)
=GAMMALN.PRECISE(0)
```
**Result:** #NUM! for both

**Explanation:** GAMMALN.PRECISE requires strictly positive arguments, just like GAMMALN.

---

### Example 11: Precision Test Near x = 1
```
=GAMMALN.PRECISE(1 + 10^-10)
```
**Result:** Very small negative number (approximately -5.77 * 10^-11)

**Explanation:** Near x = 1, the result should be approximately -(x-1) * gamma_constant. GAMMALN.PRECISE handles this accurately.

---

### Example 12: Binomial Coefficient Using GAMMALN.PRECISE
```
=EXP(GAMMALN.PRECISE(101) - GAMMALN.PRECISE(51) - GAMMALN.PRECISE(51))
```
**Result:** C(100, 50) = approximately 1.009 * 10^29

**Explanation:** For combinatorial calculations, GAMMALN.PRECISE can provide marginally better accuracy.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Argument is zero or negative | GAMMALN.PRECISE requires x > 0. Only positive values are valid. |
| `#VALUE!` | Non-numeric input | Ensure argument is a number, not text or an error value. |
| `#NAME?` | Function not available | GAMMALN.PRECISE requires Excel 2010 or later. Use GAMMALN in older versions. |
| `Unexpected zeros` | Result is exactly zero | Near x = 1 or x = 2, the true value may be zero or very close to zero. This is correct behavior. |

## Use Cases

### [[High-Precision Statistical Computing]]

**Scenario:** A statistician implements numerical algorithms requiring maximum precision for gamma function calculations, particularly validation of theoretical results.

**Implementation:**
```
Reference value at x = 1.5: =GAMMALN.PRECISE(1.5)
Verify: =GAMMALN.PRECISE(1.5) - (-0.1207822376352452)
```

**Business Application:** Numerical libraries and statistical software require verified reference values for testing. Research publications cite values with many decimal places. GAMMALN.PRECISE provides authoritative results for validation against published mathematical tables or software implementations.

**Technical Details:** The improvement in precision is most noticeable for x in [0.5, 2.5]. For x > 3 or x < 0.1, both GAMMALN and GAMMALN.PRECISE typically give identical 15-digit results.

---

### [[Financial Model Validation]]

**Scenario:** A quantitative analyst validates a financial model that uses gamma functions in option pricing or risk calculations.

**Implementation:**
```
Model output: some calculated log-gamma value
Validation: =GAMMALN.PRECISE(parameter)
Compare difference to ensure model accuracy
```

**Business Application:** Complex derivative pricing models (variance swaps, options on realized variance) involve gamma functions. When regulatory compliance requires model validation, GAMMALN.PRECISE provides trusted reference values for comparing against model outputs.

**Technical Details:** Financial models often iterate calculations. Small errors from imprecise gamma values could compound. Using GAMMALN.PRECISE in validation ensures you're comparing against the best available reference.

---

### [[Scientific Research Calculations]]

**Scenario:** A researcher performs calculations involving gamma functions for physics, biology, or engineering applications where precision is paramount.

**Implementation:**
```
Theoretical value involving log-gamma:
=GAMMALN.PRECISE(shape_parameter)
```

**Business Application:** Scientific publications require reproducible results. Using GAMMALN.PRECISE ensures calculations match those from high-precision mathematical software (Mathematica, Maple, etc.), supporting reproducibility and peer review.

**Technical Details:** When comparing spreadsheet results with scientific computing software, use GAMMALN.PRECISE to minimize discrepancies due to different numerical implementations.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2010 and later
- **Argument requirement:** Strictly positive (x > 0)
- **Precision:** 15 significant digits, optimized near x = 1 and x = 2
- **Algorithm:** Enhanced series expansions near critical points
- **Not in Excel 2007 or earlier:** Use GAMMALN instead

### Google Sheets

- **Availability:** All versions since launch
- **Name:** GAMMALN.PRECISE (same syntax)
- **Argument requirement:** Strictly positive (x > 0)
- **Precision:** 15 significant digits
- **Behavior:** Equivalent to Excel implementation

### Key Difference Alert

GAMMALN.PRECISE behaves identically between Excel 2010+ and Google Sheets. Both platforms:
- Require strictly positive arguments
- Return #NUM! for x <= 0
- Use enhanced precision algorithms
- Return identical results for valid inputs

**For older Excel versions (2007 and earlier):** GAMMALN.PRECISE is not available. Use GAMMALN instead - for most practical purposes the results are identical.

**Practical difference from GAMMALN:** The precision enhancement is subtle. Most users won't notice any difference. The improvement affects primarily the 14th and 15th significant digits for arguments near 1 and 2.

## Tips and Best Practices

1. **Use when precision is critical.** For scientific computing, model validation, or publishing results, GAMMALN.PRECISE provides maximum accuracy.

2. **Standard GAMMALN is usually sufficient.** For typical business applications, GAMMALN and GAMMALN.PRECISE give identical results. Don't over-engineer.

3. **Know the critical regions.** The precision advantage matters most for x in [0.5, 2.5], especially near 1 and 2.

4. **Same argument requirements as GAMMALN.** Both functions require x > 0 and return identical errors for invalid inputs.

5. **Verify with known values.** GAMMALN.PRECISE(1) = 0 exactly, GAMMALN.PRECISE(2) = 0 exactly. Use these as sanity checks.

6. **Consider backward compatibility.** GAMMALN.PRECISE requires Excel 2010+. For maximum compatibility, use GAMMALN (available since Excel 97).

7. **Combine with EXP for gamma values.** EXP(GAMMALN.PRECISE(x)) = Gamma(x) with full precision, even for large x where direct GAMMA would overflow.

8. **Document your choice.** If using GAMMALN.PRECISE for precision reasons, document why - it helps future maintainers understand your intent.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[GAMMALN]] | Natural log of gamma function (standard) | For general use; results identical except near x = 1 or 2 |
| [[GAMMA]] | Gamma function (not log) | For small arguments where overflow isn't a concern |
| [[FACT]] | Factorial function | For integer factorials of small numbers |
| [[LN]] | Natural logarithm | For general logarithm calculations |

### Commonly Used Together

**[[EXP]]** - Back-transform to gamma value

*Convert log-gamma to gamma:*
```
=EXP(GAMMALN.PRECISE(x))
```
Equivalent to GAMMA(x) but with enhanced precision and large-argument capability.

---

**[[GAMMA]]** - Direct gamma value

*Verification:*
```
=GAMMA(5) equals =EXP(GAMMALN.PRECISE(5))
```
Both give 24 (= 4!).

---

**[[GAMMALN]]** - Standard precision version

*Comparison:*
```
=GAMMALN(1.5) approximately equals =GAMMALN.PRECISE(1.5)
```
Results differ only in last few digits.

## Official Documentation

- **Microsoft Excel:** [GAMMALN.PRECISE function](https://support.microsoft.com/en-us/office/gammaln-precise-function-5cdfe601-4e1e-4189-9d74-241ef1caa599)
- **Google Sheets:** [GAMMALN.PRECISE function](https://support.google.com/docs/answer/9116171)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | New function for enhanced precision |
| Excel 2013+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Available since inception |

---

*Last updated: 2026-01-10*
