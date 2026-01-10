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
- probability
- standard-normal
- cumulative-distribution
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Gauss Function
- Standard Normal Probability
- Normal CDF from Zero
- Area Under Normal Curve
tags:
- statistical
- normal-distribution
- probability
- standard-normal
- gaussian
---

# GAUSS

## Description

**GAUSS** returns the probability that a random variable from the standard normal distribution falls between 0 and a specified value z. This is the area under the standard normal curve from 0 to z. For positive z, GAUSS returns a positive probability; for negative z, it returns a negative probability (indicating area to the left of 0). The function is mathematically equivalent to NORM.S.DIST(z, TRUE) - 0.5, representing the "one-sided" probability from the center of the distribution.

The standard normal distribution has mean 0 and standard deviation 1. GAUSS(z) calculates the integral of the normal PDF from 0 to z, which equals Phi(z) - 0.5 where Phi is the standard normal CDF. For z = 0, GAUSS returns 0 (no area from 0 to 0). For z = 1, GAUSS returns approximately 0.3413, meaning about 34.13% of values fall between 0 and 1 standard deviation. For z = 1.96, GAUSS returns approximately 0.475, corresponding to the familiar 95% confidence interval (0.475 on each side of the mean).

**Why GAUSS matters:** The function provides a direct answer to questions like "What fraction of a normal population falls between the mean and a given z-score?" This is particularly useful in quality control (what percentage of products fall within specification from the target?), academic grading (what proportion of students score between average and one standard deviation above?), and statistical process control (area within control limits from center).

**Relationship to other normal functions:** GAUSS(z) = NORM.S.DIST(z, TRUE) - 0.5. Equivalently, NORM.S.DIST(z, TRUE) = GAUSS(z) + 0.5. While NORM.S.DIST gives the total area from negative infinity to z, GAUSS gives only the area from 0 to z. For symmetric intervals around the mean, use 2 * GAUSS(z) to get the total probability within that range.

## Syntax

> [!f(x)] GAUSS Syntax
>
> ```
> =GAUSS(z)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `z` | Yes | The z-score (number of standard deviations from the mean) for which to calculate the probability. Can be any real number, positive or negative. |

### Return Value

Returns the probability that a standard normal random variable falls between 0 and z. Returns values between -0.5 and 0.5. Positive z gives positive results; negative z gives negative results. Returns #VALUE! for non-numeric input.

## Examples

> [!f(x)] GAUSS Examples

### Example 1: Probability from 0 to 1
```
=GAUSS(1)
```
**Result:** 0.3413 (approximately)

**Explanation:** About 34.13% of a standard normal distribution falls between z = 0 and z = 1.

---

### Example 2: GAUSS(0) = 0
```
=GAUSS(0)
```
**Result:** 0

**Explanation:** The area from 0 to 0 is zero. This is the function's center point.

---

### Example 3: Negative z-Score
```
=GAUSS(-1)
```
**Result:** -0.3413 (approximately)

**Explanation:** GAUSS returns negative values for negative z. This represents area to the left of 0, expressed as negative.

---

### Example 4: 95% Confidence Interval Value
```
=GAUSS(1.96)
```
**Result:** 0.475 (approximately)

**Explanation:** About 47.5% of the distribution falls between 0 and 1.96. Since 2 * 0.475 = 0.95, this corresponds to the 95% confidence interval.

---

### Example 5: 99% Confidence Interval Value
```
=GAUSS(2.576)
```
**Result:** 0.495 (approximately)

**Explanation:** About 49.5% falls between 0 and 2.576. Since 2 * 0.495 = 0.99, this gives the 99% confidence interval.

---

### Example 6: Two Standard Deviations
```
=GAUSS(2)
```
**Result:** 0.4772 (approximately)

**Explanation:** About 47.72% falls between 0 and 2. The famous "95% within 2 standard deviations" comes from 2 * 0.4772 = 0.9544.

---

### Example 7: Three Standard Deviations
```
=GAUSS(3)
```
**Result:** 0.4987 (approximately)

**Explanation:** About 49.87% falls between 0 and 3. Two-sided: 99.73% falls within 3 standard deviations.

---

### Example 8: Symmetric Interval Probability
```
=2*GAUSS(1.645)
```
**Result:** 0.90 (approximately)

**Explanation:** Doubling GAUSS gives the probability for a symmetric interval. 2 * GAUSS(1.645) = 0.90 for the 90% confidence interval.

---

### Example 9: Relationship with NORM.S.DIST
```
=GAUSS(1.5)
=NORM.S.DIST(1.5, TRUE) - 0.5
```
**Result:** Both return 0.4332 (approximately)

**Explanation:** GAUSS(z) equals NORM.S.DIST(z, TRUE) minus 0.5. These formulas are mathematically equivalent.

---

### Example 10: Very Large z-Score
```
=GAUSS(5)
```
**Result:** 0.4999997 (approximately)

**Explanation:** For large z, GAUSS approaches 0.5 asymptotically. Nearly all the right half of the distribution is captured.

---

### Example 11: Antisymmetric Property
```
=GAUSS(1.5) + GAUSS(-1.5)
```
**Result:** 0

**Explanation:** GAUSS(-z) = -GAUSS(z). The function is antisymmetric about 0.

---

### Example 12: Six Sigma Calculation
```
=2*GAUSS(6)
```
**Result:** 0.9999998 (approximately, or 99.99998%)

**Explanation:** The famous "Six Sigma" quality level: 99.99998% of values fall within 6 standard deviations of the mean (ignoring process shift).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure input is a number, not text or an error value. |
| `#NAME?` | Function not available | GAUSS requires Excel 2013 or later. Use NORM.S.DIST(z, TRUE) - 0.5 in older versions. |
| `Unexpected negative result` | Negative z input | GAUSS returns negative values for negative z. This is correct behavior, not an error. |
| `Result not matching expectations` | Confusion with NORM.S.DIST | GAUSS gives area from 0 to z, not from -infinity to z. Add 0.5 for the cumulative probability. |

## Use Cases

### [[Quality Control: Specification Limits]]

**Scenario:** A quality engineer calculates the proportion of products falling between the target value and upper specification limit.

**Implementation:**
```
z_upper = (USL - target) / standard_deviation
Proportion within spec (one side): =GAUSS(z_upper)
Proportion within both specs: =2*GAUSS(z_upper) if symmetric
```

**Business Application:** Manufacturing processes aim for products near the target. GAUSS tells you what proportion falls between target and spec limit. If GAUSS(3) = 0.4987, then 49.87% of production is between target and 3-sigma limit. Combined with the symmetric lower half, 99.73% is within specification.

**Technical Details:** This assumes the process is centered on target. For off-center processes, use NORM.S.DIST directly. Six Sigma methodology uses GAUSS to calculate process capability indices and defect rates.

---

### [[Academic Grading: Score Distributions]]

**Scenario:** An educator determines what percentage of students score between the mean and various grade boundaries on a normally distributed test.

**Implementation:**
```
z_grade = (grade_cutoff - mean) / stdev
Percentage between mean and cutoff: =GAUSS(z_grade) * 100 & "%"
```

**Business Application:** If the B/A cutoff is 1.5 standard deviations above mean, GAUSS(1.5) = 0.433 tells us 43.3% of students fall between average and this boundary. Understanding these proportions helps set fair grade cutoffs and predict grade distributions.

**Technical Details:** Real test scores may not be perfectly normal. Verify normality assumption before applying. For highly skewed distributions, consider non-parametric percentile approaches.

---

### [[Statistical Process Control: Control Charts]]

**Scenario:** A process analyst calculates the expected proportion of points falling within control limits from the center line.

**Implementation:**
```
Standard control limits at 3-sigma: =2*GAUSS(3)
Warning limits at 2-sigma: =2*GAUSS(2)
```

**Business Application:** Control charts flag out-of-control processes. With 3-sigma limits, GAUSS shows that 99.73% of in-control data should fall within limits. This means about 0.27% (roughly 1 in 370) of points should randomly exceed limits even when the process is stable, setting expectations for false alarms.

**Technical Details:** Shewhart control charts assume normality. For non-normal processes (counts, proportions, ranges), different control chart types apply. The 3-sigma convention balances sensitivity against false alarm rate.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2013 and later (NOT in Excel 2010 or earlier)
- **Input range:** Any real number
- **Output range:** -0.5 to 0.5
- **Precision:** 15 significant digits
- **Alternative for older versions:** =NORM.S.DIST(z, TRUE) - 0.5

### Google Sheets

- **Availability:** All versions since launch
- **Input range:** Any real number
- **Output range:** -0.5 to 0.5
- **Precision:** 15 significant digits
- **Behavior:** Identical to Excel

### Key Difference Alert

GAUSS behaves identically between Excel 2013+ and Google Sheets. Both platforms:
- Accept any real number as input
- Return values between -0.5 and 0.5
- Use the same mathematical formula
- Return identical results

**Excel 2010 and earlier compatibility:** GAUSS is not available. Use the equivalent formula:
```
=NORM.S.DIST(z, TRUE) - 0.5
```
Or in very old Excel (pre-2010):
```
=NORMSDIST(z) - 0.5
```

## Tips and Best Practices

1. **Understand what GAUSS returns.** It gives area from 0 to z, not total cumulative probability. For cumulative, add 0.5.

2. **Double for symmetric intervals.** To find probability within z standard deviations of the mean (both sides), use 2 * GAUSS(z).

3. **Remember the sign convention.** GAUSS returns negative for negative z. This is mathematically correct (negative area to the left of center).

4. **Know key values.** GAUSS(1) = 0.3413, GAUSS(1.96) = 0.475, GAUSS(2) = 0.4772, GAUSS(3) = 0.4987.

5. **Use for control chart calculations.** GAUSS directly answers "what proportion is within k sigma of center?"

6. **Consider NORM.S.DIST for cumulative.** If you need P(Z <= z), use NORM.S.DIST(z, TRUE) instead of adding 0.5 to GAUSS.

7. **Check version compatibility.** GAUSS requires Excel 2013+. For backward compatibility, use the NORM.S.DIST formula.

8. **Convert from raw data.** First standardize: z = (x - mean) / stdev, then apply GAUSS(z).

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[NORM.S.DIST]] | Full standard normal CDF | When you need P(Z <= z), not just area from 0 |
| [[NORM.DIST]] | Normal CDF with custom mean/stdev | For non-standard normal distributions |
| [[NORM.S.INV]] | Inverse standard normal | To find z given probability |
| [[PHI]] | Standard normal PDF | For probability density, not cumulative |

### Commonly Used Together

**[[NORM.S.DIST]]** - Full cumulative probability

*Relationship:*
```
GAUSS(z) = NORM.S.DIST(z, TRUE) - 0.5
NORM.S.DIST(z, TRUE) = GAUSS(z) + 0.5
```
Use whichever form is more intuitive for your problem.

---

**[[NORM.S.INV]]** - Find z from probability

*Inverse operations:*
```
If GAUSS(z) = p, then z = NORM.S.INV(p + 0.5)
Example: GAUSS(1.96) approximately equals 0.475
Inverse: NORM.S.INV(0.975) approximately equals 1.96
```

---

**[[STANDARDIZE]]** - Convert raw value to z-score

*Standard workflow:*
```
z = STANDARDIZE(x, mean, stdev)
Probability from mean to x: =GAUSS(z)
```

## Official Documentation

- **Microsoft Excel:** [GAUSS function](https://support.microsoft.com/en-us/office/gauss-function-069f1b4e-7dee-4d6a-a71f-4b69044a6b33)
- **Google Sheets:** [GAUSS function](https://support.google.com/docs/answer/9116269)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New function |
| Excel 2016+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Available since inception |

---

*Last updated: 2026-01-10*
