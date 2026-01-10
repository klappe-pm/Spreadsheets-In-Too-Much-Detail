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
- factorial-generalization
- special-functions
- mathematical
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Gamma Function
- Factorial Extension
- Euler Gamma
tags:
- statistical
- gamma
- factorial
- mathematical
- special-functions
---

# GAMMA

## Description

**GAMMA** calculates the gamma function, which extends the factorial function to non-integer and complex numbers. For positive integers, GAMMA(n) = (n-1)!, so GAMMA(5) = 4! = 24. But unlike factorial, gamma is defined for all real numbers except non-positive integers (0, -1, -2, ...). This makes it essential for probability distributions (like gamma, chi-square, and beta distributions), combinatorics with non-integer parameters, and various mathematical applications.

The gamma function is defined by the integral: Gamma(x) = integral from 0 to infinity of t^(x-1) * e^(-t) dt. This integral converges for all positive real numbers and can be analytically continued to negative non-integers. Key properties include: Gamma(x+1) = x * Gamma(x) (recurrence relation), Gamma(1) = 1, Gamma(1/2) = sqrt(pi) approximately 1.772, and Gamma(n) = (n-1)! for positive integers.

**Why GAMMA matters:** Many probability density functions involve gamma functions in their normalizing constants. The chi-square distribution's PDF contains Gamma(df/2). The beta distribution uses gamma functions. The binomial coefficient can be expressed using gamma: C(n,k) = Gamma(n+1) / (Gamma(k+1) * Gamma(n-k+1)), which works even for non-integer n. Statistical tables often assume integer degrees of freedom, but GAMMA enables exact calculations for non-integer parameters.

**Numerical considerations:** GAMMA grows extremely rapidly for positive arguments (faster than exponential). GAMMA(171) exceeds the maximum double-precision floating-point value, causing overflow. For large arguments, use GAMMALN (logarithm of gamma) instead and exponentiate only when needed for final results. For negative non-integer arguments, gamma oscillates and can be very large positive or negative.

## Syntax

> [!f(x)] GAMMA Syntax
>
> ```
> =GAMMA(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The value at which to evaluate the gamma function. Can be any real number except non-positive integers (0, -1, -2, ...). |

### Return Value

Returns the gamma function value at the specified number. Returns #NUM! for non-positive integers (where gamma is undefined) or when the result exceeds representable values (overflow). Returns #VALUE! for non-numeric input.

## Examples

> [!f(x)] GAMMA Examples

### Example 1: Positive Integer (Factorial Relationship)
```
=GAMMA(5)
```
**Result:** 24

**Explanation:** GAMMA(5) = (5-1)! = 4! = 24. For positive integers n, GAMMA(n) = (n-1)!.

---

### Example 2: Factorial of n Using GAMMA
```
=GAMMA(n+1)
```
**Result:** n!

**Explanation:** To get n!, use GAMMA(n+1). So GAMMA(6) = 5! = 120, GAMMA(7) = 6! = 720.

---

### Example 3: Non-Integer Argument
```
=GAMMA(3.5)
```
**Result:** 3.323 (approximately)

**Explanation:** Gamma extends factorials to non-integers. GAMMA(3.5) = 2.5 * 1.5 * 0.5 * GAMMA(0.5) = 2.5 * 1.5 * 0.5 * sqrt(pi).

---

### Example 4: GAMMA(1/2) = sqrt(pi)
```
=GAMMA(0.5)
```
**Result:** 1.772 (approximately, equals sqrt(pi))

**Explanation:** Famous result: Gamma(1/2) = sqrt(pi). This arises from the Gaussian integral and connects gamma to the normal distribution.

---

### Example 5: GAMMA(1) = 1
```
=GAMMA(1)
```
**Result:** 1

**Explanation:** Gamma(1) = 0! = 1. This is one of the base cases for the gamma function.

---

### Example 6: Negative Non-Integer
```
=GAMMA(-0.5)
```
**Result:** -3.545 (approximately)

**Explanation:** Gamma is defined for negative non-integers. GAMMA(-0.5) = -2 * sqrt(pi). The function oscillates between positive and negative.

---

### Example 7: Near-Zero Positive
```
=GAMMA(0.1)
```
**Result:** 9.514 (approximately)

**Explanation:** As x approaches 0 from the right, GAMMA(x) approaches infinity. GAMMA(0.1) is large but finite.

---

### Example 8: Non-Positive Integer (Error)
```
=GAMMA(0)
=GAMMA(-1)
=GAMMA(-2)
```
**Result:** #NUM! for all

**Explanation:** Gamma is undefined at non-positive integers (poles of the function). These inputs produce errors.

---

### Example 9: Large Argument (Overflow Risk)
```
=GAMMA(170)   Result: 7.26E+306 (approximately)
=GAMMA(171)   Result: #NUM! (overflow)
```
**Result:** GAMMA grows extremely fast

**Explanation:** GAMMA exceeds double-precision maximum around argument 171. Use GAMMALN for large arguments.

---

### Example 10: Binomial Coefficient with GAMMA
```
C(5, 2) = =GAMMA(6)/(GAMMA(3)*GAMMA(4))
```
**Result:** 10

**Explanation:** Binomial coefficient using gamma: C(n,k) = Gamma(n+1)/(Gamma(k+1)*Gamma(n-k+1)). Works for non-integer n too.

---

### Example 11: Chi-Square Distribution Constant
```
Normalizing constant for chi-square(df):
=1/(2^(df/2) * GAMMA(df/2))
```
**Result:** Part of chi-square PDF

**Explanation:** Chi-square distribution's PDF uses Gamma(df/2). Even for odd df, GAMMA handles the half-integer correctly.

---

### Example 12: Verification with GAMMALN
```
=GAMMA(10)
=EXP(GAMMALN(10))
```
**Result:** Both return 362880 (= 9!)

**Explanation:** GAMMA(x) = EXP(GAMMALN(x)). Use GAMMALN for large arguments to avoid overflow.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Argument is 0 or negative integer | Gamma undefined at 0, -1, -2, ... Use non-integer values. |
| `#NUM!` | Overflow (argument >= 171) | Use GAMMALN instead and exponentiate only when necessary. |
| `#VALUE!` | Non-numeric input | Ensure argument is a number, not text. |
| `#NAME?` | Function not available (Excel 2010-) | GAMMA was added in Excel 2013. Use EXP(GAMMALN(x)) in older versions. |
| `Very large result` | Normal for positive arguments | Gamma grows rapidly. Verify this is expected for your application. |

## Use Cases

### [[Probability Distribution Calculations]]

**Scenario:** A statistician calculates probability density values for gamma-family distributions (gamma, chi-square, beta) that require gamma function in normalizing constants.

**Implementation:**
```
Chi-square PDF at x with df degrees of freedom:
=x^(df/2-1) * EXP(-x/2) / (2^(df/2) * GAMMA(df/2))
```

**Business Application:** Custom statistical calculations often require exact PDF values, not just cumulative probabilities. Financial risk models using chi-square distributions for portfolio variance, quality control applications using gamma distributions for wait times, and Bayesian analysis using beta distributions all need gamma function calculations.

**Technical Details:** For large df or x, calculate in log space: ln(PDF) = (df/2-1)*ln(x) - x/2 - (df/2)*ln(2) - GAMMALN(df/2), then exponentiate. This avoids overflow in intermediate calculations.

---

### [[Generalized Combinatorics]]

**Scenario:** A researcher calculates binomial coefficients for non-integer upper parameters, arising in negative binomial distributions or fractional calculus.

**Implementation:**
```
Generalized binomial: C(n, k) = GAMMA(n+1) / (GAMMA(k+1) * GAMMA(n-k+1))
```
Works even when n is not an integer.

**Business Application:** Negative binomial distributions model count data with overdispersion. The probability mass function involves binomial coefficients with non-integer "number of failures" parameter. GAMMA enables exact calculations rather than approximations.

**Technical Details:** For large values, use log form: ln(C(n,k)) = GAMMALN(n+1) - GAMMALN(k+1) - GAMMALN(n-k+1). Exponentiate only the final result.

---

### [[Special Function Computation]]

**Scenario:** An engineer or scientist implements mathematical formulas that include gamma functions, such as Bessel functions, hypergeometric functions, or physics equations.

**Implementation:**
```
Various formulas reference Gamma(x) directly
Example: Stirling approximation verification
=GAMMA(n+1) vs =SQRT(2*PI()*n)*(n/EXP(1))^n
```

**Business Application:** Scientific computing, actuarial science, and engineering applications frequently encounter gamma functions in formulas. Direct GAMMA calculation simplifies implementation versus custom numerical integration.

**Technical Details:** Be aware of precision limitations. For very large or very small arguments, numerical precision degrades. Consider specialized libraries for high-precision requirements.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2013 and later (NOT in Excel 2010 or earlier)
- **Overflow threshold:** ~170.6 (GAMMA(171) overflows)
- **Negative integers:** Returns #NUM!
- **Precision:** 15 significant digits
- **Alternative for older versions:** =EXP(GAMMALN(x)) for positive x

### Google Sheets

- **Availability:** All versions since launch
- **Overflow threshold:** Similar to Excel (~171)
- **Negative integers:** Returns #NUM!
- **Precision:** 15 significant digits
- **Behavior:** Identical to modern Excel

### Key Difference Alert

GAMMA behaves identically in Excel 2013+ and Google Sheets. The main compatibility issue is Excel 2010 and earlier, which lack GAMMA entirely.

**Workaround for Excel 2010 and earlier:**
- For positive arguments: =EXP(GAMMALN(x))
- For negative non-integers: No direct workaround (requires custom VBA or approximation)

## Tips and Best Practices

1. **Remember the factorial offset.** GAMMA(n) = (n-1)!, not n!. To get n!, use GAMMA(n+1).

2. **Use GAMMALN for large arguments.** GAMMA overflows around 171. For large values, work with logarithms using GAMMALN.

3. **Avoid non-positive integers.** GAMMA is undefined at 0, -1, -2, ... These produce #NUM! errors.

4. **Know key values.** GAMMA(1) = 1, GAMMA(0.5) = sqrt(pi) approximately 1.772, GAMMA(n) = (n-1)! for positive integers.

5. **Use for distribution constants.** Chi-square, gamma, beta, and F distributions all use gamma functions. GAMMA simplifies implementing their PDFs.

6. **Verify with factorial.** For integer arguments, check that GAMMA(n) equals (n-1)!. This catches formula errors.

7. **Consider precision.** For arguments very close to non-positive integers, numerical precision may suffer due to near-poles.

8. **Check version compatibility.** GAMMA requires Excel 2013+. Use EXP(GAMMALN(x)) for backward compatibility with positive arguments.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[GAMMALN]] | Natural log of gamma function | For large arguments to avoid overflow |
| [[GAMMALN.PRECISE]] | More precise log gamma | When highest precision needed |
| [[FACT]] | Factorial function | When argument is a non-negative integer |
| [[GAMMA.DIST]] | Gamma distribution | For probability calculations |

### Commonly Used Together

**[[GAMMALN]]** - Log form for large values

*Avoid overflow:*
```
Direct: =GAMMA(150) works
Direct: =GAMMA(200) overflows
Log form: =EXP(GAMMALN(200)) works
```

---

**[[EXP]]** - Back-transform from log

*Calculate from GAMMALN:*
```
=EXP(GAMMALN(x))
```
Equivalent to GAMMA(x) but handles larger arguments.

---

**[[FACT]]** - Integer factorial

*Compare:*
```
=FACT(5) gives 5! = 120
=GAMMA(6) gives (6-1)! = 120
```
Use FACT for integer factorials; use GAMMA when you need non-integer extension.

## Official Documentation

- **Microsoft Excel:** [GAMMA function](https://support.microsoft.com/en-us/office/gamma-function-ce1702b1-cf55-471d-8307-f83be0fc5297)
- **Google Sheets:** [GAMMA function](https://support.google.com/docs/answer/9116267)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New function |
| Excel 2016+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Available since inception |

---

*Last updated: 2026-01-10*
