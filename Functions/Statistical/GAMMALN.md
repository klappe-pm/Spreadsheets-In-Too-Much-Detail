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
- factorial-extension
- numerical-stability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Log Gamma
- Natural Log of Gamma
- Ln Gamma
tags:
- statistical
- gamma
- logarithm
- factorial
- numerical-stability
---

# GAMMALN

## Description

**GAMMALN** returns the natural logarithm of the gamma function, ln(Gamma(x)). This function is crucial for numerical computations involving gamma functions because the gamma function grows extremely rapidly and often overflows standard floating-point representation. By working with logarithms, you can handle much larger values and maintain numerical precision through multiplication and division operations (which become addition and subtraction in log space).

For any positive real number x, GAMMALN(x) = ln(Gamma(x)). Since Gamma(n) = (n-1)! for positive integers, GAMMALN(n) = ln((n-1)!). The function is defined only for positive arguments (unlike GAMMA which handles negative non-integers). Key values include: GAMMALN(1) = 0 (since Gamma(1) = 1), GAMMALN(0.5) = ln(sqrt(pi)) approximately 0.572, and GAMMALN(n) = ln((n-1)!) for positive integers.

**Why logarithms matter:** Gamma(171) exceeds the maximum double-precision floating-point value and causes overflow. But GAMMALN(171) = approximately 706, well within representable range. When computing ratios like Gamma(a)/Gamma(b), calculating ln(Gamma(a)) - ln(Gamma(b)) and then exponentiating gives accurate results even when Gamma(a) and Gamma(b) would individually overflow.

**Applications:** GAMMALN is essential for computing binomial coefficients for large n, probability densities for gamma-family distributions, and any formula involving products or ratios of gamma functions. Statistical computations (beta functions, incomplete gamma, chi-square distributions) frequently use GAMMALN internally for numerical stability.

## Syntax

> [!f(x)] GAMMALN Syntax
>
> ```
> =GAMMALN(x)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The positive value at which to evaluate the natural logarithm of the gamma function. Must be strictly positive (x > 0). |

### Return Value

Returns the natural logarithm of Gamma(x). Result can be any real number. Returns #NUM! if x <= 0. Returns #VALUE! for non-numeric input.

## Examples

> [!f(x)] GAMMALN Examples

### Example 1: Log of Factorial
```
=GAMMALN(6)
```
**Result:** 4.787 (approximately)

**Explanation:** GAMMALN(6) = ln(Gamma(6)) = ln(5!) = ln(120) = 4.787. This is the natural log of 5 factorial.

---

### Example 2: Recovering GAMMA from GAMMALN
```
=EXP(GAMMALN(5))
```
**Result:** 24 (= Gamma(5) = 4!)

**Explanation:** Exponentiating GAMMALN gives GAMMA. This equivalence is exact for moderate arguments.

---

### Example 3: GAMMALN(1) = 0
```
=GAMMALN(1)
```
**Result:** 0

**Explanation:** Gamma(1) = 1, and ln(1) = 0. This is a key reference value.

---

### Example 4: Large Argument (No Overflow)
```
=GAMMALN(200)
```
**Result:** 857.93 (approximately)

**Explanation:** GAMMA(200) would overflow (it's about 10^372), but GAMMALN(200) = 857.93 is easily representable.

---

### Example 5: Non-Integer Argument
```
=GAMMALN(3.5)
```
**Result:** 1.201 (approximately)

**Explanation:** ln(Gamma(3.5)) = 1.201. GAMMALN handles non-integers just like GAMMA does.

---

### Example 6: GAMMALN(0.5) = ln(sqrt(pi))
```
=GAMMALN(0.5)
```
**Result:** 0.572 (approximately)

**Explanation:** Gamma(0.5) = sqrt(pi), so GAMMALN(0.5) = ln(sqrt(pi)) = 0.5 * ln(pi) = 0.572.

---

### Example 7: Computing Binomial Coefficient (Large n)
```
=EXP(GAMMALN(101) - GAMMALN(51) - GAMMALN(51))
```
**Result:** C(100, 50) = approximately 1.009 * 10^29

**Explanation:** ln(C(n,k)) = GAMMALN(n+1) - GAMMALN(k+1) - GAMMALN(n-k+1). Even for n=100, this works when direct calculation would overflow.

---

### Example 8: Beta Function Using GAMMALN
```
Beta(a, b) = =EXP(GAMMALN(a) + GAMMALN(b) - GAMMALN(a+b))
```
**Result:** Beta function value

**Explanation:** Beta(a,b) = Gamma(a)*Gamma(b)/Gamma(a+b). In log form: ln(Beta) = GAMMALN(a) + GAMMALN(b) - GAMMALN(a+b).

---

### Example 9: Negative Argument (Error)
```
=GAMMALN(-1)
=GAMMALN(0)
```
**Result:** #NUM! for both

**Explanation:** GAMMALN requires strictly positive arguments. Unlike GAMMA, it doesn't handle negative non-integers.

---

### Example 10: Very Small Positive Argument
```
=GAMMALN(0.001)
```
**Result:** 6.906 (approximately)

**Explanation:** As x approaches 0 from the right, Gamma(x) approaches infinity, so GAMMALN approaches infinity too. GAMMALN(0.001) is ln of a very large number.

---

### Example 11: Stirling Approximation Comparison
```
Exact: =GAMMALN(n+1)
Stirling: =(n+0.5)*LN(n) - n + 0.5*LN(2*PI())
```
**Result:** Stirling approximation is very accurate for large n

**Explanation:** Stirling's formula approximates ln(n!) for large n. Compare against GAMMALN(n+1) to verify accuracy.

---

### Example 12: Log-Likelihood for Poisson Distribution
```
Log-likelihood term: =k*LN(lambda) - lambda - GAMMALN(k+1)
```
**Result:** Contribution to Poisson log-likelihood

**Explanation:** Poisson log-likelihood includes ln(k!) = GAMMALN(k+1). Working in log space is standard for likelihood calculations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Argument is zero or negative | GAMMALN requires x > 0. Use GAMMALN.PRECISE for same behavior. |
| `#VALUE!` | Non-numeric input | Ensure argument is a number, not text. |
| `#NAME?` | Function name misspelled | Verify spelling is GAMMALN, not GAMLN or LNGAMMA. |
| `Precision issues` | Very large arguments | For extremely large x (>10^15), precision degrades. Use specialized libraries for extreme cases. |

## Use Cases

### [[Maximum Likelihood Estimation]]

**Scenario:** A statistician computes log-likelihoods for model fitting, requiring log-factorials and log-gamma values.

**Implementation:**
```
Poisson log-likelihood: =SUMPRODUCT(data*LN(lambda) - lambda - GAMMALN(data+1))
```

**Business Application:** Maximum likelihood estimation is the foundation of statistical modeling. Poisson regression for count data, negative binomial models for overdispersed counts, and Dirichlet-multinomial models for compositional data all require log-gamma in their log-likelihoods. Working in log space prevents numerical underflow that would occur multiplying many small probabilities.

**Technical Details:** Always compute log-likelihoods, not likelihoods. Use GAMMALN for log-factorials: ln(n!) = GAMMALN(n+1). Optimization algorithms (like solver) work better with log-likelihood's smoother landscape.

---

### [[Large Combinatorial Calculations]]

**Scenario:** A data scientist calculates binomial coefficients or multinomial coefficients for large n where direct calculation would overflow.

**Implementation:**
```
ln(C(n,k)) = GAMMALN(n+1) - GAMMALN(k+1) - GAMMALN(n-k+1)
C(n,k) = EXP(above formula)
```

**Business Application:** A/B testing with millions of users, genetics with thousands of loci, and text analysis with large vocabularies all require combinatorics at scale. C(1000, 500) has about 300 digits - impossible to compute directly. GAMMALN enables exact calculations for any practical size.

**Technical Details:** Compute entirely in log space when possible. Only exponentiate final results if needed. For ratios of combinatorial quantities, subtract log values rather than dividing potentially overflow-prone raw values.

---

### [[Probability Density Function Implementation]]

**Scenario:** An analyst implements custom probability distributions that require gamma function normalizing constants.

**Implementation:**
```
Chi-square log-PDF:
=(df/2-1)*LN(x) - x/2 - (df/2)*LN(2) - GAMMALN(df/2)
```

**Business Application:** While built-in functions exist for common distributions, custom distributions (mixtures, truncated versions, reparameterizations) require implementing PDFs from scratch. GAMMALN handles the normalizing constants accurately for any parameters.

**Technical Details:** Always implement log-PDF first. Exponentiate only if you specifically need the PDF value. For numerical integration (custom CDFs), work with log values to prevent underflow in the tails.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Argument requirement:** Strictly positive (x > 0)
- **Precision:** 15 significant digits
- **Maximum argument:** Very large (~10^307 before precision issues)

### Google Sheets

- **Availability:** All versions since launch
- **Argument requirement:** Strictly positive (x > 0)
- **Precision:** 15 significant digits
- **Behavior:** Identical to Excel

### Key Difference Alert

GAMMALN behaves identically between Excel and Google Sheets. Both platforms:
- Require strictly positive arguments
- Return #NUM! for x <= 0
- Use the same numerical algorithm
- Return identical results for valid inputs

Unlike GAMMA (Excel 2013+ only), GAMMALN has been available since Excel 97, making it the more portable option for backward compatibility.

## Tips and Best Practices

1. **Prefer GAMMALN over GAMMA.** When computing products or ratios of gamma functions, work in log space. Add/subtract GAMMALN values, then exponentiate only the final result.

2. **Use for large factorials.** ln(n!) = GAMMALN(n+1). For n > 170, this is the only practical approach in standard spreadsheets.

3. **Remember GAMMALN requires positive arguments.** Unlike GAMMA, GAMMALN doesn't handle negative non-integers. Use only x > 0.

4. **Implement log-likelihoods.** Statistical model fitting should always use log-likelihoods with GAMMALN, never raw likelihoods.

5. **Verify with small values.** For n <= 170, check that EXP(GAMMALN(n)) equals GAMMA(n) or (n-1)!.

6. **Know key values.** GAMMALN(1) = 0, GAMMALN(2) = 0, GAMMALN(0.5) approximately 0.572.

7. **Use for beta function.** Beta(a,b) = EXP(GAMMALN(a) + GAMMALN(b) - GAMMALN(a+b)).

8. **Consider GAMMALN.PRECISE.** For highest accuracy, especially near 1 and 2, use GAMMALN.PRECISE if available.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[GAMMA]] | Gamma function (not log) | For small arguments where overflow isn't a concern |
| [[GAMMALN.PRECISE]] | Higher precision log gamma | When maximum precision needed, especially near 1 or 2 |
| [[FACT]] | Factorial function | For integer factorials of small numbers |
| [[LN]] | Natural logarithm | For other log calculations (GAMMALN is specialized) |

### Commonly Used Together

**[[EXP]]** - Back-transform to gamma value

*Convert log to value:*
```
=EXP(GAMMALN(x))
```
Equivalent to GAMMA(x) but handles larger arguments.

---

**[[GAMMA]]** - Direct gamma value

*Verification:*
```
=GAMMA(10) equals =EXP(GAMMALN(10))
```
Both give 362880 (= 9!).

---

**[[COMBIN]]** - Binomial coefficient

*Compare approaches:*
```
Direct: =COMBIN(100, 50)
Via GAMMALN: =EXP(GAMMALN(101)-GAMMALN(51)-GAMMALN(51))
```
Both give same result; GAMMALN works for much larger n.

## Official Documentation

- **Microsoft Excel:** [GAMMALN function](https://support.microsoft.com/en-us/office/gammaln-function-b838c48b-c65f-484f-9e1d-141c55470eb9)
- **Google Sheets:** [GAMMALN function](https://support.google.com/docs/answer/3093416)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2010 | GAMMALN.PRECISE added | Higher precision variant |
| Excel 2013+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Identical to Excel |

---

*Last updated: 2026-01-10*
