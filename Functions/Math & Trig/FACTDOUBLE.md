---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- factorial
- combinatorics
- special-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Double Factorial
- Semi-Factorial
- n!!
tags:
- factorial
- combinatorics
- statistics
- physics
- mathematics
---

# FACTDOUBLE

## Description

**FACTDOUBLE** returns the double factorial of a number, written mathematically as n!!. Despite the name, it's NOT the factorial of a factorial—that would be (n!)!. Instead, the double factorial multiplies every other number from n down to 1 or 2. For odd n: n!! = n * (n-2) * (n-4) * ... * 3 * 1. For even n: n!! = n * (n-2) * (n-4) * ... * 4 * 2.

The double factorial appears less frequently than the regular factorial but has important applications in combinatorics, probability theory, and physics. You'll encounter it when counting certain arrangements, in formulas for the surface area and volume of hyperspheres, and in quantum mechanics calculations involving angular momentum.

**Understanding the pattern:** FACTDOUBLE(7) = 7 * 5 * 3 * 1 = 105. FACTDOUBLE(8) = 8 * 6 * 4 * 2 = 384. Each step skips a number, multiplying only the odd or only the even integers down to 1 or 2 respectively. FACTDOUBLE(0) = 1 and FACTDOUBLE(-1) = 1 by convention.

**Relationship to regular factorial:** For any non-negative integer n, n! = n!! * (n-1)!! (when n > 0). Also, (2n)!! = 2^n * n! and (2n-1)!! = (2n)!/(2^n * n!). These identities can help verify calculations or simplify expressions.

## Syntax

> [!f(x)] FACTDOUBLE Syntax
>
> ```
> =FACTDOUBLE(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The non-negative integer for which you want the double factorial. Decimal values are truncated to integers. Must be >= -1. |

### Return Value

Returns the double factorial (n!!) as a number. Returns #NUM! if number is less than -1. Returns #VALUE! if number is non-numeric.

## Examples

> [!f(x)] FACTDOUBLE Examples

### Example 1: Small Odd Number
```
=FACTDOUBLE(5)
```
**Result:** 15

**Explanation:** 5!! = 5 * 3 * 1 = 15. Multiplies only odd numbers from 5 down.

---

### Example 2: Small Even Number
```
=FACTDOUBLE(6)
```
**Result:** 48

**Explanation:** 6!! = 6 * 4 * 2 = 48. Multiplies only even numbers from 6 down.

---

### Example 3: Larger Odd Number
```
=FACTDOUBLE(9)
```
**Result:** 945

**Explanation:** 9!! = 9 * 7 * 5 * 3 * 1 = 945. Five terms in the product.

---

### Example 4: Larger Even Number
```
=FACTDOUBLE(10)
```
**Result:** 3840

**Explanation:** 10!! = 10 * 8 * 6 * 4 * 2 = 3840. Five terms in the product.

---

### Example 5: Zero
```
=FACTDOUBLE(0)
```
**Result:** 1

**Explanation:** By convention, 0!! = 1 (empty product). Similar to how 0! = 1.

---

### Example 6: Negative One
```
=FACTDOUBLE(-1)
```
**Result:** 1

**Explanation:** By convention, (-1)!! = 1. This makes certain recursive formulas work correctly.

---

### Example 7: One
```
=FACTDOUBLE(1)
```
**Result:** 1

**Explanation:** 1!! = 1. The product of just the number 1.

---

### Example 8: Two
```
=FACTDOUBLE(2)
```
**Result:** 2

**Explanation:** 2!! = 2. Only one term in the product.

---

### Example 9: Cell Reference
```
=FACTDOUBLE(A1)
```
Where A1 = 7

**Result:** 105

**Explanation:** 7!! = 7 * 5 * 3 * 1 = 105. Cell reference allows dynamic calculation.

---

### Example 10: Decimal Input (Truncated)
```
=FACTDOUBLE(5.9)
```
**Result:** 15

**Explanation:** Decimals are truncated to integers. 5.9 becomes 5, so 5!! = 15.

---

### Example 11: Relationship to Regular Factorial
```
=FACT(6) = FACTDOUBLE(6) * FACTDOUBLE(5)
```
Where FACT(6) = 720, FACTDOUBLE(6) = 48, FACTDOUBLE(5) = 15

**Result:** TRUE (720 = 48 * 15)

**Explanation:** n! = n!! * (n-1)!! for positive integers. Useful identity for verification.

---

### Example 12: Error Case
```
=FACTDOUBLE(-2)
```
**Result:** #NUM!

**Explanation:** FACTDOUBLE is only defined for n >= -1. Values less than -1 return an error.

---

### Example 13: Comparing Odd and Even
```
=FACTDOUBLE(8) / FACTDOUBLE(7)
```
**Result:** 3.657142857...

**Explanation:** Ratio shows how even and odd double factorials grow at similar rates but with different values.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number is less than -1 | FACTDOUBLE requires n >= -1. Check your input value. |
| `#VALUE!` | Non-numeric input | Ensure input is a number. Text values cause errors. |
| `#NUM!` | Result too large | Double factorials grow quickly. For n > 300 or so, results exceed Excel's limits. |
| `Unexpected result` | Confusing with (n!)! | FACTDOUBLE(5) = 15, not FACT(FACT(5)). Double factorial is NOT factorial of factorial. |
| `Decimal truncation` | Decimal input | FACTDOUBLE truncates decimals. 5.9 becomes 5. Use ROUND if you need different behavior. |

## Use Cases

### [[Combinatorics - Counting Arrangements]]

**Scenario:** Count the number of ways to pair up 2n people, or count certain permutation patterns.

**Implementation:**
```
Pairings of 2n people: =(2*n - 1)!!
// Or equivalently:
=FACTDOUBLE(2*n - 1)
```

**Business Application:** Tournament scheduling, match-making algorithms, experimental design where subjects are paired.

**Technical Details:** The number of ways to partition 2n items into n pairs is (2n-1)!! = (2n)!/(2^n * n!). This appears frequently in combinatorics.

---

### [[Physics - Quantum Mechanics]]

**Scenario:** Calculate coefficients in angular momentum and spherical harmonic formulas.

**Implementation:**
```
Coefficient: =FACTDOUBLE(2*l) / (2^l * FACT(l))
// Related to associated Legendre polynomials
```

**Business Application:** Scientific research, physics simulations, quantum computing calculations.

**Technical Details:** Double factorials appear in normalization constants for spherical harmonics and in Clebsch-Gordan coefficients used in angular momentum coupling.

---

### [[Statistics - Moments of Normal Distribution]]

**Scenario:** Calculate the moments of the standard normal distribution.

**Implementation:**
```
Even moment E[X^(2k)] for standard normal: =FACTDOUBLE(2*k - 1)
// E[X^2] = 1, E[X^4] = 3, E[X^6] = 15, etc.
```

**Business Application:** Risk analysis, financial modeling, quality control statistics.

**Technical Details:** For the standard normal distribution N(0,1), the even moments are (2k-1)!! = 1, 3, 15, 105, etc. Odd moments are all zero.

---

### [[Geometry - Hypersphere Volumes]]

**Scenario:** Calculate volumes and surface areas of n-dimensional spheres.

**Implementation:**
```
// Volume of unit n-sphere involves:
For even n: =PI()^(n/2) / FACT(n/2)
For odd n: =2^((n+1)/2) * PI()^((n-1)/2) / FACTDOUBLE(n)
```

**Business Application:** Scientific computing, dimensional analysis, theoretical physics research.

**Technical Details:** Hypersphere formulas use double factorials in denominators. The n-ball volume is pi^(n/2) / Gamma(n/2 + 1), which relates to double factorials for integer dimensions.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (legacy function)
- **Domain:** Integer n >= -1 (decimals truncated)
- **Maximum value:** Limited by Excel's numeric range (about 10^308)
- **Precision:** 15 significant digits

### Google Sheets
- **Availability:** All versions
- **Domain:** Same as Excel
- **Maximum value:** Same limits
- **Precision:** 15 significant digits

### Both Platforms
- Identical function name and syntax
- Same truncation behavior for decimals
- Same error conditions
- FACTDOUBLE(0) = FACTDOUBLE(-1) = 1

## Tips and Best Practices

1. **Remember the definition:** n!! is NOT (n!)!. It's the product of every other integer from n down. 7!! = 7*5*3*1, not 5040!.

2. **Know the base cases:** FACTDOUBLE(0) = FACTDOUBLE(-1) = FACTDOUBLE(1) = 1. These are defined by convention.

3. **Use the factorial relationship:** n! = n!! * (n-1)!! for verification. If unsure, check this identity.

4. **Watch for overflow:** Double factorials grow quickly (though slower than regular factorials). FACTDOUBLE(300) is near Excel's limit.

5. **Truncation warning:** Decimal inputs are truncated, not rounded. FACTDOUBLE(5.9) = FACTDOUBLE(5) = 15.

6. **Alternative calculation:** For even n: n!! = 2^(n/2) * (n/2)!. For odd n: n!! = n! / (n-1)!!.

7. **Combine with FACT for advanced formulas:** Many mathematical formulas use both FACT and FACTDOUBLE together.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FACT]] | Returns the regular factorial (n!) | For standard factorial calculations |
| [[COMBIN]] | Returns combinations (n choose k) | For combination counting |
| [[PERMUT]] | Returns permutations | For permutation counting |
| [[GAMMA]] | Returns the gamma function | For non-integer factorial-like calculations |

### Commonly Used Together

**[[FACT]]** - Regular factorial

*Verify using relationship:*
```
=FACT(n) = FACTDOUBLE(n) * FACTDOUBLE(n-1)  // For n > 0
```
Factorial and double factorial are related.

---

**[[POWER]]** - Exponentiation

*In double factorial identities:*
```
=(2^n) * FACT(n)  // Equals FACTDOUBLE(2*n)
```
Even double factorial has exponential relationship.

---

**[[PI]]** - Mathematical constant

*In sphere volume formulas:*
```
=PI()^(n/2) / FACTDOUBLE(n) * radius^n  // For certain n-ball calculations
```
Double factorial appears in geometric formulas.

---

**[[SQRT]]** - Square root

*In statistical formulas:*
```
=SQRT(2/PI()) * FACTDOUBLE(2*k-1)  // Related to normal distribution
```
Combined in moment calculations.

---

**[[IF]]** - Conditional logic

*Handle edge cases:*
```
=IF(A1 < -1, "Invalid", FACTDOUBLE(A1))
```
Pre-validate inputs to avoid errors.

## Official Documentation

- **Microsoft Excel:** [FACTDOUBLE function](https://support.microsoft.com/en-us/office/factdouble-function-e67697ac-d214-48eb-b7b7-cce2589ecac8)
- **Google Sheets:** [FACTDOUBLE function](https://support.google.com/docs/answer/3093431)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2003 | Part of Analysis ToolPak, later built-in |
| Excel 2007+ | Built-in | No longer requires add-in |
| Google Sheets | Original launch | Available since Sheets inception |

---

*Last updated: 2026-01-10*
