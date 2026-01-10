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
- inverse-transformation
- hyperbolic-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Inverse Fisher Transformation
- Fisher Inverse
- Tanh
- Z-prime to Correlation
tags:
- statistical
- correlation
- transformation
- inverse
- hypothesis-testing
---

# FISHERINV

## Description

**FISHERINV** applies the inverse Fisher transformation, converting a Fisher-transformed value (z') back to a correlation coefficient (r). This function is the inverse of FISHER, taking any real number as input and returning a value strictly between -1 and 1. The formula is FISHERINV(z') = (e^(2z') - 1) / (e^(2z') + 1), which is mathematically equivalent to the hyperbolic tangent function tanh(z').

The inverse Fisher transformation is essential for interpreting results after performing statistical operations on transformed correlations. When you calculate confidence intervals, averaged correlations, or test statistics using Fisher-transformed values, FISHERINV converts these results back to the interpretable correlation scale. Without this back-transformation, statistical conclusions would be on an abstract z' scale that practitioners cannot easily interpret.

**Mathematical properties:** FISHERINV maps z' = 0 to r = 0, positive z' to positive r, and negative z' to negative r. As z' approaches positive infinity, r approaches +1. As z' approaches negative infinity, r approaches -1. The function is bounded between -1 and 1, regardless of how extreme the input value. This boundedness ensures that back-transformed values are always valid correlations.

**Completing the analysis cycle:** Statistical inference on correlations follows a transform-analyze-backtransform pattern. (1) Transform sample correlation(s) using FISHER. (2) Perform calculations on the approximately normal z' scale (confidence intervals, tests, averaging). (3) Back-transform results using FISHERINV to obtain answers on the correlation scale. This workflow is fundamental in meta-analysis and correlation-based research.

## Syntax

> [!f(x)] FISHERINV Syntax
>
> ```
> =FISHERINV(y)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `y` | Yes | The Fisher-transformed value (z') to convert back to a correlation. Can be any real number from negative to positive infinity. |

### Return Value

Returns a correlation coefficient strictly between -1 and 1 (exclusive). Returns #VALUE! if input is non-numeric. Never returns values outside (-1, 1).

## Examples

> [!f(x)] FISHERINV Examples

### Example 1: Zero Transforms to Zero
```
=FISHERINV(0)
```
**Result:** 0

**Explanation:** A z' of 0 corresponds to r = 0. The transformation preserves zero exactly.

---

### Example 2: Moderate z' Value
```
=FISHERINV(0.5)
```
**Result:** 0.462 (approximately)

**Explanation:** A z' of 0.5 back-transforms to r = 0.462. The inverse transformation compresses values toward the center.

---

### Example 3: Large z' Value
```
=FISHERINV(1.5)
```
**Result:** 0.905 (approximately)

**Explanation:** A z' of 1.5 corresponds to a correlation of about 0.91. Large z' values map to correlations near +1.

---

### Example 4: Negative z' Value
```
=FISHERINV(-0.7)
```
**Result:** -0.604 (approximately)

**Explanation:** Negative z' values transform to negative correlations. FISHERINV(-y) = -FISHERINV(y).

---

### Example 5: Verify Inverse Relationship
```
Original r: 0.6
z' = FISHER(0.6):  0.693
Back to r = FISHERINV(0.693): 0.6
```
**Result:** Original value recovered

**Explanation:** FISHERINV(FISHER(r)) = r for any valid correlation. This verifies the inverse relationship.

---

### Example 6: Very Large z' Value
```
=FISHERINV(3)
```
**Result:** 0.995 (approximately)

**Explanation:** Even large z' values produce correlations strictly less than 1. The function is bounded.

---

### Example 7: Very Negative z' Value
```
=FISHERINV(-3)
```
**Result:** -0.995 (approximately)

**Explanation:** Very negative z' values approach -1 but never reach it.

---

### Example 8: Confidence Interval Back-Transformation
```
z' = 0.867 (from sample r = 0.7, n = 30)
SE = 1/SQRT(30-3) = 0.192
CI for z': [0.867 - 1.96*0.192, 0.867 + 1.96*0.192] = [0.490, 1.244]
CI for r: [FISHERINV(0.490), FISHERINV(1.244)] = [0.454, 0.846]
```
**Result:** 95% CI for correlation is [0.454, 0.846]

**Explanation:** After computing CI on z' scale, back-transform both endpoints to get CI on correlation scale. Note the asymmetry.

---

### Example 9: Averaging Correlations
```
r1 = 0.5, r2 = 0.7, r3 = 0.8
z1 = FISHER(0.5) = 0.549
z2 = FISHER(0.7) = 0.867
z3 = FISHER(0.8) = 1.099
z_avg = (0.549 + 0.867 + 1.099)/3 = 0.838
r_avg = FISHERINV(0.838) = 0.685
```
**Result:** Properly averaged correlation is 0.685

**Explanation:** Simple average of 0.5, 0.7, 0.8 is 0.667. The Fisher-transformed average is 0.685, which correctly accounts for the bounded scale.

---

### Example 10: Manual Calculation Verification
```
Using FISHERINV: =FISHERINV(1.0)
Using formula: =(EXP(2*1)-1)/(EXP(2*1)+1)
```
**Result:** Both methods give 0.762 (approximately)

**Explanation:** FISHERINV equals the hyperbolic tangent function. These formulas verify the implementation.

---

### Example 11: Meta-Analysis Combined Estimate
```
Weighted z_avg = 0.95 (from meta-analysis)
Combined r = FISHERINV(0.95) = 0.740
```
**Result:** Meta-analytic correlation estimate

**Explanation:** After weighted averaging of z' values across studies, back-transform to get the combined correlation coefficient for interpretation and reporting.

---

### Example 12: Extreme Value Handling
```
=FISHERINV(10)
```
**Result:** 0.99999999... (very close to 1)

**Explanation:** No matter how large the input, the output never reaches exactly 1. The function is bounded.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure input is a number, not text. |
| `#NAME?` | Function name misspelled | Verify spelling is FISHERINV, not FISHINV or FISHERINVERSE. |
| `Unexpected result` | Applying to test statistic instead of z' | FISHERINV is for back-transforming z', not for p-values or z-test statistics. |
| `Result near but not at +/-1` | Expected behavior for large inputs | FISHERINV approaches but never reaches +/-1, even for very large inputs. |

## Use Cases

### [[Interpreting Confidence Intervals for Correlations]]

**Scenario:** A researcher reports a sample correlation with confidence interval, needing to back-transform bounds from the Fisher z' scale.

**Implementation:**
```
z_lower: =FISHER(r) - 1.96/SQRT(n-3)
z_upper: =FISHER(r) + 1.96/SQRT(n-3)
r_lower: =FISHERINV(z_lower)
r_upper: =FISHERINV(z_upper)
```

**Business Application:** Published correlations should include confidence intervals for proper interpretation. "The correlation between engagement and retention is 0.65 (95% CI: 0.52, 0.75)" conveys both the estimate and its precision. FISHERINV provides the back-transformed bounds for reporting.

**Technical Details:** Note that correlation CIs are asymmetric on the r scale. A CI of [0.52, 0.75] has unequal distances from 0.65 (0.13 below, 0.10 above). This asymmetry correctly reflects the bounded nature of correlations.

---

### [[Reporting Meta-Analysis Results]]

**Scenario:** A systematic reviewer reports the pooled correlation from multiple studies after computing the weighted average on the z' scale.

**Implementation:**
```
Each study: z'_i = FISHER(r_i), weight_i = n_i - 3
Pooled z': =SUMPRODUCT(weights, z_values)/SUM(weights)
Pooled r: =FISHERINV(pooled_z)
```

**Business Application:** Meta-analyses synthesize evidence across studies. The final pooled correlation must be on the interpretable r scale, not the abstract z' scale. FISHERINV converts the properly-computed weighted average to a reportable correlation.

**Technical Details:** For meta-analysis, also compute heterogeneity statistics (Q, I-squared) to assess consistency across studies. Consider random-effects models if heterogeneity is substantial.

---

### [[Converting Published z' Values]]

**Scenario:** A practitioner reads a research paper that reports z' values and needs to interpret them as standard correlations.

**Implementation:**
```
Published z' = 1.15
Equivalent r: =FISHERINV(1.15) = 0.818
```

**Business Application:** Some academic literature reports Fisher-transformed correlations. Practitioners need to convert these to intuitive correlations. A z' of 1.15 might seem unintuitive, but r = 0.82 communicates "strong positive relationship" clearly.

**Technical Details:** When reading papers, check whether reported values are r or z'. Context clues: values bounded by +/-1 are likely r; unbounded values (especially > 1) are likely z'.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Input range:** Any real number (unbounded)
- **Output range:** Strictly between -1 and 1
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Input range:** Any real number
- **Output range:** Strictly between -1 and 1
- **Precision:** 15 significant decimal digits
- **Behavior:** Identical to Excel

### Key Difference Alert

FISHERINV behaves identically between Excel and Google Sheets. Both platforms:
- Accept any real number as input
- Return values strictly between -1 and 1
- Use the same mathematical formula
- Return identical results

## Tips and Best Practices

1. **Always pair with FISHER.** The transform-analyze-backtransform workflow requires both functions.

2. **Back-transform endpoints, not ranges.** For confidence intervals, apply FISHERINV to each endpoint separately.

3. **Understand asymmetry.** Confidence intervals on r are asymmetric. This is correct and expected.

4. **Don't apply to test statistics.** Only back-transform z' values (transformed correlations), not z-test statistics.

5. **Verify with round-trip.** Check that FISHERINV(FISHER(r)) = r.

6. **Report on correlation scale.** Always report final results as correlations, not z'.

7. **Use for weighted averaging.** When averaging correlations, transform, average, back-transform.

8. **Note bounded output.** No matter how large the input, output is always between -1 and 1.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[FISHER]] | Fisher transformation (forward) | To convert correlation to z' before analysis |
| [[CORREL]] | Calculate correlation | To compute r values initially |
| [[TANH]] | Hyperbolic tangent | Mathematically equivalent to FISHERINV |
| [[EXP]] | Exponential function | For manual calculation |

### Commonly Used Together

**[[FISHER]]** - Forward transformation

*Complete workflow:*
```
Forward: z' = FISHER(r)
Analysis: (operations on z')
Back: r = FISHERINV(z')
```

---

**[[NORM.S.INV]]** - Critical values for CI

*Confidence interval:*
```
z_crit = NORM.S.INV(1-alpha/2)
CI endpoints for z': z' +/- z_crit/SQRT(n-3)
CI endpoints for r: FISHERINV(each z' endpoint)
```

## Official Documentation

- **Microsoft Excel:** [FISHERINV function](https://support.microsoft.com/en-us/office/fisherinv-function-62504b39-415a-4284-a285-19c8e82f86bb)
- **Google Sheets:** [FISHERINV function](https://support.google.com/docs/answer/3093958)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2003+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Identical to Excel |

---

*Last updated: 2026-01-10*
