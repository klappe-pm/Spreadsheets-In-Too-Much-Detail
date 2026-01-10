---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- normal-distribution
- probability-density
- standard-normal
- statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Standard Normal PDF
- Normal Density Function
- Phi Function
tags:
- normal-distribution
- probability
- density
- statistics
- phi
- gaussian
---

# PHI

## Description

**PHI** returns the value of the probability density function (PDF) for the standard normal distribution at a specified point. The standard normal distribution has mean 0 and standard deviation 1. PHI calculates the height of the famous "bell curve" at any given z-value, using the formula: (1/sqrt(2*pi)) * e^(-x^2/2).

Use PHI when you need the **probability density at a specific point** on the standard normal curve. This is different from cumulative probability (which NORM.S.DIST provides)—PHI gives the curve's height, not the area under it. PHI is essential for likelihood calculations, kernel density estimation, and understanding normal distribution properties.

The maximum value of PHI is approximately 0.3989 at x=0 (the peak of the bell curve). As x moves away from 0 in either direction, PHI decreases, approaching zero as x approaches positive or negative infinity. The curve is symmetric: PHI(x) = PHI(-x) for all x.

**Important distinction:** PHI gives density, not probability. For a continuous distribution, the probability of any exact point is technically zero—probabilities come from areas under the curve (integrals). PHI values are useful for comparing relative likelihoods, not for direct probability statements.

## Syntax

> [!f(x)] PHI Syntax
>
> ```
> =PHI(x)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The value (z-score) at which to evaluate the standard normal density function. Can be any real number. |

### Return Value

Returns the probability density of the standard normal distribution at x. Always positive. Maximum value approximately 0.3989 at x=0. Decreases toward 0 as |x| increases. Never exactly 0 but approaches 0 asymptotically.

## Examples

> [!f(x)] PHI Examples

### Example 1: Density at Mean (Peak)
```
=PHI(0)
```
**Result:** 0.3989422804...

**Explanation:** The maximum height of the standard normal curve occurs at x=0 (the mean). This is 1/sqrt(2*pi).

---

### Example 2: Density at z=1
```
=PHI(1)
```
**Result:** 0.2419707245...

**Explanation:** At one standard deviation from the mean, the density is about 60% of the peak value.

---

### Example 3: Density at z=-1
```
=PHI(-1)
```
**Result:** 0.2419707245...

**Explanation:** Same as PHI(1) due to symmetry. The normal curve is symmetric around x=0.

---

### Example 4: Density at z=2
```
=PHI(2)
```
**Result:** 0.0539909665...

**Explanation:** At two standard deviations, density drops to about 13.5% of peak value.

---

### Example 5: Density at z=3
```
=PHI(3)
```
**Result:** 0.0044318484...

**Explanation:** At three standard deviations, density is very small—about 1.1% of peak.

---

### Example 6: Extreme Values
```
=PHI(5)
```
**Result:** 1.48672E-06

**Explanation:** Five standard deviations from mean has negligible density—extremely unlikely region.

---

### Example 7: Negative Extreme
```
=PHI(-4)
```
**Result:** 0.0001338302...

**Explanation:** Symmetric with PHI(4). Both tails have equal density at equal distances from mean.

---

### Example 8: Verifying Symmetry
```
=PHI(1.5) - PHI(-1.5)
```
**Result:** 0

**Explanation:** Demonstrates perfect symmetry: PHI(x) = PHI(-x) for all x.

---

### Example 9: Using Cell Reference
```
=PHI(A1)
```
**Result:** Density at z-score in A1

**Explanation:** Standard usage with cell reference for dynamic calculations.

---

### Example 10: Comparing Densities
```
=PHI(0)/PHI(2)
```
**Result:** 7.389... (approximately e^2)

**Explanation:** Ratio shows how much more likely values near the mean are compared to 2 SD away.

---

### Example 11: Likelihood Ratio
```
=PHI(1.5)/PHI(2.5)
```
**Result:** 3.679...

**Explanation:** A z-score of 1.5 is about 3.7 times more likely (in density terms) than 2.5.

---

### Example 12: Building the Bell Curve
```
For x from -3 to 3 in steps of 0.1: =PHI(x)
```
**Result:** Array of density values for plotting

**Explanation:** Generate y-values for plotting the standard normal curve.

---

### Example 13: Verifying Formula
```
=PHI(1) vs =(1/SQRT(2*PI()))*EXP(-1^2/2)
```
**Result:** Both equal 0.2419707245...

**Explanation:** PHI implements the standard normal PDF formula.

---

### Example 14: Inflection Points
```
=PHI(1) vs =PHI(0)*EXP(-0.5)
```
**Result:** Equal values

**Explanation:** At z = plus/minus 1, the curve has inflection points where curvature changes sign.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric argument | Ensure x is a number or cell containing a number. |
| `#NAME?` | Function not available or misspelled | PHI was added in Excel 2013. Use manual formula in older versions. Check spelling. |
| `Confusion with cumulative` | Expected area, got density | PHI gives height (density), not area (probability). Use NORM.S.DIST for cumulative probability. |
| `Unexpected small values` | Large |x| values | PHI approaches 0 for extreme x values—this is correct, not an error. |

## Use Cases

### [[Statistical Likelihood Calculations]]

**Scenario:** Calculate relative likelihoods of different observed values under normal assumption.

**Implementation:**
```
=PHI(z_observed)                    → Likelihood of specific z-score
=PHI(z1)/PHI(z2)                    → Likelihood ratio
=PRODUCT(PHI(z_array))              → Joint likelihood (independence assumed)
```

**Business Application:** Quality control, anomaly detection, statistical inference. Compare how "expected" different observations are.

**Technical Details:** Likelihood ratios are more meaningful than individual densities. PHI(0)/PHI(2) shows values at mean are about 7.4x more likely than values 2 SD away.

---

### [[Data Visualization]]

**Scenario:** Create normal distribution curves for charts and visualizations.

**Implementation:**
```
=PHI(X_value)                       → Y-value for standard normal curve
=PHI((X-Mean)/StdDev)/StdDev        → Y-value for general normal curve
=Generate x from -4 to 4, y=PHI(x)  → Data for bell curve plot
```

**Business Application:** Reports, presentations, educational materials. Visualize probability distributions.

**Technical Details:** For non-standard normal (mean != 0, SD != 1), scale the input and output: y = PHI((x-mu)/sigma)/sigma.

---

### [[Kernel Density Estimation]]

**Scenario:** Build smooth density estimates from data points.

**Implementation:**
```
=SUMPRODUCT(PHI((x - data_points)/bandwidth))/COUNT(data_points)/bandwidth
```

**Business Application:** Data smoothing, non-parametric density estimation, distribution approximation.

**Technical Details:** Gaussian kernel uses PHI as the kernel function. Each data point contributes a small normal curve; sum creates smooth estimate.

---

### [[Normal Distribution Education]]

**Scenario:** Demonstrate and teach properties of normal distribution.

**Implementation:**
```
=PHI(0)                             → Maximum density
=PHI(1)/PHI(0)                      → Relative density at 1 SD
=PHI(x) for x=-3 to 3               → Generate curve data
```

**Business Application:** Statistics courses, training materials, documentation. Show how normal distribution works.

**Technical Details:** PHI makes abstract concepts concrete: the 68-95-99.7 rule, symmetry, and tail behavior become visible.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later only
- **Not available:** Excel 2010 and earlier—use manual formula
- **Precision:** Full floating-point precision
- **Formula alternative:** `=(1/SQRT(2*PI()))*EXP(-x^2/2)`

### Google Sheets
- **Availability:** All versions
- **Performance:** Efficient calculation
- **Alternative:** Same manual formula works if needed

### Both Platforms
- Identical mathematical result
- Same handling of extreme values
- No array function capability (single value only)

### Compatibility Formula
For Excel 2010 and earlier:
```
=(1/SQRT(2*PI()))*EXP(-x^2/2)
```

## Tips and Best Practices

1. **Don't confuse with cumulative:** PHI gives density (curve height), not probability (area). For P(Z < x), use NORM.S.DIST.

2. **Use for comparisons:** PHI values are most useful as ratios—comparing relative likelihoods of different values.

3. **Remember symmetry:** PHI(x) = PHI(-x). This can simplify calculations involving symmetric scenarios.

4. **Scale for non-standard normal:** For mean mu and std dev sigma: density = PHI((x-mu)/sigma)/sigma.

5. **Maximum at zero:** PHI(0) = 1/sqrt(2*pi) = 0.3989... is the curve's peak. No larger value is possible.

6. **Generate bell curves:** Create x values from -4 to 4 (or -3 to 3), calculate PHI(x) for each to plot standard normal.

7. **Consider NORM.DIST for general cases:** For non-standard normal with PDF output, NORM.DIST(x, mean, sd, FALSE) is more direct.

8. **Verify with formula:** PHI(x) = EXP(-x^2/2)/SQRT(2*PI()). Use this for verification or in older Excel.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[NORM.S.DIST]] | Standard normal CDF | When you need cumulative probability P(Z < x) |
| [[NORM.DIST]] | General normal PDF or CDF | For non-standard normal distributions |
| [[GAUSS]] | Area from 0 to x under standard normal | For area between 0 and z |
| [[NORM.S.INV]] | Inverse standard normal | When you have probability and need z-score |

### Commonly Used Together

**[[NORM.S.DIST]]** - Cumulative probability

*Density vs cumulative:*
```
Density:     =PHI(1)              → 0.2419 (height)
Cumulative:  =NORM.S.DIST(1,TRUE) → 0.8413 (area to left)
```
PHI gives point density; NORM.S.DIST gives accumulated probability.

---

**[[NORM.DIST]]** - General normal distribution

*Non-standard normal density:*
```
Standard:    =PHI(z)
General:     =NORM.DIST(x, mean, sd, FALSE)
Equivalent:  =PHI((x-mean)/sd)/sd
```
NORM.DIST handles any normal distribution.

---

**[[STANDARDIZE]]** - Convert to z-score

*Pre-process before PHI:*
```
z = STANDARDIZE(x, mean, sd)
density = PHI(z) / sd
```
Convert raw values to z-scores for PHI.

---

**[[EXP]]** and **[[SQRT]]** - Manual formula

*Calculate without PHI:*
```
=EXP(-x^2/2)/SQRT(2*PI())
```
Equivalent to PHI(x); use in older Excel versions.

---

**[[GAUSS]]** - Related area function

*Area from 0 to z:*
```
=GAUSS(z)                         → Area from 0 to z
=NORM.S.DIST(z,TRUE) - 0.5        → Equivalent calculation
```
GAUSS gives area, PHI gives density.

## Official Documentation

- **Microsoft Excel:** [PHI function](https://support.microsoft.com/en-us/office/phi-function-23e2c89b-8d11-4a08-a7a2-67abcb3b3a3c)
- **Google Sheets:** [PHI function](https://support.google.com/docs/answer/9116335)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New function |
| Excel 2016+ | All subsequent | No changes |
| Google Sheets | Original launch | Always available |
| LibreOffice Calc | Recent versions | Compatible implementation |

---

*Last updated: 2026-01-10*
