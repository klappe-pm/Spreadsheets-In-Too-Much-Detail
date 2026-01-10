---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- fisher-transformation
- correlation
- hyperbolic-functions
- normalization
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Fisher Transformation
- Fisher Z Transformation
- Arctanh
- Z-prime
tags:
- statistical
- correlation
- transformation
- normalization
- hypothesis-testing
---

# FISHER

## Description

**FISHER** applies the Fisher transformation (also called Fisher's z-transformation) to a correlation coefficient, converting it to a value that is approximately normally distributed. The function takes a correlation coefficient r (between -1 and 1) and returns z' = 0.5 * ln((1+r)/(1-r)), which is the inverse hyperbolic tangent (arctanh) of r. This transformation is essential for statistical inference involving correlation coefficients, enabling hypothesis tests, confidence intervals, and comparisons between correlations.

The Fisher transformation addresses a fundamental problem with correlation coefficients: their sampling distribution is not normal, especially when the true correlation is far from zero. A sample correlation of 0.9 cannot increase by much (limited at 1.0) but can decrease substantially, creating skewed sampling distributions. The Fisher transformation "stretches" the scale near the boundaries, converting the bounded, skewed distribution into an approximately normal, unbounded one. After performing statistical calculations on the transformed values, use FISHERINV to convert back to the correlation scale.

**Mathematical formulation:** FISHER(r) = 0.5 * ln((1+r)/(1-r)) = arctanh(r). This is the inverse hyperbolic tangent function. The transformation maps r = 0 to z' = 0, and it preserves the sign of the correlation. As r approaches +1 or -1, z' approaches positive or negative infinity, respectively. The standard error of z' is approximately 1/sqrt(n-3), which is nearly independent of the true correlation - a crucial property enabling simple inference.

**Applications in statistical inference:** The Fisher transformation enables: (1) constructing confidence intervals for correlation coefficients, (2) testing whether a correlation differs from a specified value, (3) testing whether two correlations from different samples are equal, and (4) averaging correlations across studies (meta-analysis). Without this transformation, these procedures would require complex distribution theory for each different correlation value.

## Syntax

> [!f(x)] FISHER Syntax
>
> ```
> =FISHER(x)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The correlation coefficient to transform. Must be strictly between -1 and 1 (exclusive). Values of exactly -1 or +1 return errors because the transformation approaches infinity at these boundaries. |

### Return Value

Returns the Fisher transformation (z') of the input correlation coefficient. The result is an unbounded real number: 0 for r=0, positive for positive correlations, negative for negative correlations. Returns #NUM! if x <= -1 or x >= 1.

## Examples

> [!f(x)] FISHER Examples

### Example 1: Zero Correlation
```
=FISHER(0)
```
**Result:** 0

**Explanation:** A correlation of 0 transforms to 0. The transformation preserves zero exactly.

---

### Example 2: Moderate Positive Correlation
```
=FISHER(0.5)
```
**Result:** 0.549 (approximately)

**Explanation:** A correlation of 0.5 transforms to z' = 0.549. The transformation slightly stretches values away from zero.

---

### Example 3: Strong Positive Correlation
```
=FISHER(0.9)
```
**Result:** 1.472 (approximately)

**Explanation:** A correlation of 0.9 transforms to z' = 1.472. High correlations are stretched more, compensating for the compressed sampling distribution near +1.

---

### Example 4: Negative Correlation
```
=FISHER(-0.6)
```
**Result:** -0.693 (approximately)

**Explanation:** Negative correlations transform to negative z' values. The transformation is antisymmetric: FISHER(-r) = -FISHER(r).

---

### Example 5: Very High Correlation
```
=FISHER(0.99)
```
**Result:** 2.647 (approximately)

**Explanation:** As r approaches 1, z' increases rapidly. This stretching is necessary because correlations near 1 have highly skewed sampling distributions.

---

### Example 6: Near-Zero Correlation
```
=FISHER(0.1)
```
**Result:** 0.100 (approximately)

**Explanation:** For small correlations, the transformation is nearly linear (z' approximately equals r). The function diverges from identity only as |r| increases.

---

### Example 7: Confidence Interval for Correlation
```
Step 1 - Transform: z' = =FISHER(r)
Step 2 - SE of z': SE = 1/SQRT(n-3)
Step 3 - CI for z': z' +/- 1.96*SE
Step 4 - Back-transform: =FISHERINV(z'_lower), =FISHERINV(z'_upper)
```
**Result:** 95% confidence interval for the correlation

**Explanation:** The standard error of z' is approximately 1/sqrt(n-3), enabling simple normal-based confidence intervals.

---

### Example 8: Testing if Correlation Differs from 0.5
```
z'_observed: =FISHER(r)
z'_null: =FISHER(0.5)
SE: =1/SQRT(n-3)
z_test: =(z'_observed - z'_null)/SE
p_value: =2*(1-NORM.S.DIST(ABS(z_test), TRUE))
```
**Result:** P-value for testing H0: rho = 0.5

**Explanation:** The Fisher transformation enables testing correlations against any hypothesized value, not just zero.

---

### Example 9: Comparing Two Independent Correlations
```
z'_1: =FISHER(r1)
z'_2: =FISHER(r2)
SE: =SQRT(1/(n1-3) + 1/(n2-3))
z_test: =(z'_1 - z'_2)/SE
```
**Result:** Test statistic for comparing two correlations

**Explanation:** With Fisher transformation, comparing correlations from different samples follows standard normal distribution under the null hypothesis.

---

### Example 10: Boundary Behavior
```
=FISHER(0.999)   Result: 3.800 (approximately)
=FISHER(0.9999)  Result: 4.952 (approximately)
=FISHER(1)       Result: #NUM! (error)
```
**Result:** Transformation approaches infinity at boundaries

**Explanation:** The transformation is undefined at r = +/-1 because the natural logarithm of infinity or zero is undefined. Use values strictly between -1 and 1.

---

### Example 11: Averaging Correlations (Meta-Analysis)
```
z'_1: =FISHER(r1)
z'_2: =FISHER(r2)
z'_3: =FISHER(r3)
Average z': =AVERAGE(z'_1, z'_2, z'_3)
Average r: =FISHERINV(Average_z')
```
**Result:** Properly averaged correlation

**Explanation:** Simply averaging correlations is inappropriate due to their bounded, skewed distribution. Transform, average the z' values, then back-transform.

---

### Example 12: Verification with Manual Formula
```
Using FISHER: =FISHER(0.7)
Using LN: =0.5*LN((1+0.7)/(1-0.7))
```
**Result:** Both methods give 0.867 (approximately)

**Explanation:** FISHER is mathematically equivalent to the manual formula. This verification confirms the implementation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | x <= -1 | Correlation must be strictly greater than -1. Perfect negative correlation cannot be transformed. |
| `#NUM!` | x >= 1 | Correlation must be strictly less than +1. Perfect positive correlation cannot be transformed. |
| `#VALUE!` | Non-numeric input | Ensure input is a number, not text. |
| `#NAME?` | Function name misspelled | Verify spelling is FISHER, not FISCHER or FISHERS. |
| `Unexpected result` | Input not a correlation | FISHER expects values between -1 and 1. Check that you're providing a correlation coefficient. |

## Use Cases

### [[Confidence Interval for Pearson Correlation]]

**Scenario:** A researcher calculates a confidence interval for a sample correlation coefficient to report the precision of the estimate.

**Implementation:**
```
z_prime: =FISHER(sample_r)
SE: =1/SQRT(n-3)
CI_z_lower: =z_prime - 1.96*SE
CI_z_upper: =z_prime + 1.96*SE
CI_r_lower: =FISHERINV(CI_z_lower)
CI_r_upper: =FISHERINV(CI_z_upper)
```

**Business Application:** When reporting correlations (e.g., between advertising spend and sales), confidence intervals convey uncertainty. A correlation of 0.65 with 95% CI [0.55, 0.73] tells stakeholders the relationship is reliably positive. Without CI, readers can't assess whether the correlation might plausibly be weak or very strong.

**Technical Details:** The standard error 1/sqrt(n-3) is an approximation that works well for n >= 10. For very small samples, exact methods exist but are rarely needed. Note that CIs on the correlation scale are asymmetric (longer tail toward zero).

---

### [[Meta-Analysis: Combining Correlations Across Studies]]

**Scenario:** A research synthesizer combines correlation coefficients from multiple studies to estimate an overall effect size.

**Implementation:**
```
Transform each: z'_i = FISHER(r_i)
Weight by sample size: w_i = n_i - 3
Weighted average: z'_bar = SUM(w_i * z'_i) / SUM(w_i)
Back-transform: r_bar = FISHERINV(z'_bar)
```

**Business Application:** In systematic reviews (e.g., effectiveness of training programs), combining results across studies increases precision. Simply averaging correlations gives incorrect results because the scale is bounded. Fisher transformation enables proper averaging with appropriate weighting.

**Technical Details:** Use n-3 as weight (inverse of variance). Fixed-effects meta-analysis assumes one true effect; random-effects models account for heterogeneity. Report heterogeneity statistics (Q, I-squared) alongside pooled estimates.

---

### [[Testing Whether Two Correlations Differ]]

**Scenario:** A psychologist tests whether the correlation between two variables is stronger in one population than another.

**Implementation:**
```
z'_1: =FISHER(r1)
z'_2: =FISHER(r2)
SE_diff: =SQRT(1/(n1-3) + 1/(n2-3))
z_stat: =(z'_1 - z'_2)/SE_diff
p_value: =2*(1-NORM.S.DIST(ABS(z_stat), TRUE))
```

**Business Application:** Comparing correlations answers questions like: "Is the relationship between customer satisfaction and loyalty stronger online than in-store?" or "Has the correlation between education and income changed over decades?" The Fisher transformation makes such comparisons statistically tractable.

**Technical Details:** This test assumes independent samples. For comparing correlations from the same sample (e.g., r_xy vs r_xz), different methods apply because the correlations share data. Look up Steiger's or Meng's test for dependent correlations.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Input range:** -1 < x < 1 (exclusive bounds)
- **Precision:** 15 significant decimal digits
- **Alternative:** =0.5*LN((1+x)/(1-x)) gives same result

### Google Sheets

- **Availability:** All versions since launch
- **Input range:** -1 < x < 1 (exclusive bounds)
- **Precision:** 15 significant decimal digits
- **Behavior:** Identical to Excel

### Key Difference Alert

FISHER behaves identically between Excel and Google Sheets. Both platforms:
- Require input strictly between -1 and 1
- Return #NUM! at the boundaries
- Use the same mathematical formula
- Return identical results for valid inputs

Some programming languages provide arctanh directly (equivalent to FISHER). In R, use atanh(). In Python/NumPy, use numpy.arctanh().

## Tips and Best Practices

1. **Always use FISHER and FISHERINV together.** Transform correlations with FISHER, perform statistical operations, then back-transform with FISHERINV. Don't mix transformed and untransformed values.

2. **Verify input is a valid correlation.** FISHER expects values between -1 and 1. Sample correlations rarely equal exactly +/-1, but data errors or perfect linear relationships could produce boundary values.

3. **Use for confidence intervals.** Direct confidence intervals on r are complicated. Transform to z', add/subtract 1.96/sqrt(n-3), then back-transform for 95% CI.

4. **Average correlations correctly.** Don't average raw correlations. Transform, average (possibly weighted), then back-transform. Simple averaging underestimates the true average correlation.

5. **Remember the standard error.** SE of z' = 1/sqrt(n-3), nearly independent of the true correlation. This simplifies inference but requires n >= 4.

6. **Consider small sample sizes.** For n < 10, the normal approximation for z' may be imprecise. Consider exact methods or bootstrap approaches.

7. **Understand the transformation scale.** z' = 0 corresponds to r = 0. z' = 1 corresponds to r = 0.76. z' = 2 corresponds to r = 0.96. The transformation stretches high correlations substantially.

8. **Use for testing any null hypothesis.** While simple t-tests exist for testing r = 0, FISHER enables testing r = 0.5, r = 0.8, or comparing two correlations - much more general.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FISHERINV]] | Inverse Fisher transformation | To convert z' back to correlation coefficient |
| [[CORREL]] | Calculate correlation coefficient | To compute r before transforming |
| [[TANH]] | Hyperbolic tangent | Mathematically related but different purpose |
| [[LN]] | Natural logarithm | For manual FISHER calculation |

### Commonly Used Together

**[[FISHERINV]]** - Back-transformation

*Complete CI workflow:*
```
z' = FISHER(r)
CI_z = z' +/- 1.96/SQRT(n-3)
CI_r = FISHERINV(CI_z_lower) to FISHERINV(CI_z_upper)
```

---

**[[CORREL]]** - Calculate correlation first

*Full analysis:*
```
r = CORREL(X, Y)
z' = FISHER(r)
Further inference on z'...
```

---

**[[NORM.S.DIST]]** - P-values for hypothesis tests

*Hypothesis test:*
```
z_test = (FISHER(r) - FISHER(r0)) / (1/SQRT(n-3))
p_value = 2*(1 - NORM.S.DIST(ABS(z_test), TRUE))
```

## Official Documentation

- **Microsoft Excel:** [FISHER function](https://support.microsoft.com/en-us/office/fisher-function-d656523c-5076-4f95-b87b-7741bf236c69)
- **Google Sheets:** [FISHER function](https://support.google.com/docs/answer/3093956)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2003+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Identical to Excel |

---

*Last updated: 2026-01-10*
