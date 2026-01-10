---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- chi-square-distribution
- inverse-distribution
- quantile-function
- left-tail
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Chi-Square Inverse
- Chi-Square Quantile
- Chi-Square Left Tail Inverse
tags:
- statistical
- chi-square
- inverse
- quantile
- probability
---

# CHISQ.INV

## Description

**CHISQ.INV** returns the inverse of the left-tailed cumulative chi-square distribution. Given a probability p and degrees of freedom, it finds the chi-square value x such that the cumulative probability from 0 to x equals p. In other words, it answers: "What chi-square value has p probability of being exceeded from below?" This function is essential for constructing confidence intervals for variance and for certain statistical procedures that require chi-square quantiles.

The chi-square distribution is asymmetric and bounded below by zero, making its inverse function particularly useful for variance estimation. Unlike the normal distribution which is symmetric, confidence intervals for variance require both CHISQ.INV (for lower bounds) and CHISQ.INV.RT (for upper bounds). When you need to find the chi-square value where a specified proportion of the distribution lies below it, CHISQ.INV provides the answer directly.

**Mathematical relationship:** CHISQ.INV is the inverse of CHISQ.DIST with cumulative=TRUE. If CHISQ.DIST(x, df, TRUE) = p, then CHISQ.INV(p, df) = x. This inverse relationship is fundamental: CHISQ.INV "undoes" what CHISQ.DIST does. Given a probability, it returns the corresponding chi-square value. This is analogous to NORM.INV for normal distributions or T.INV for t-distributions.

**Important distinction from CHISQ.INV.RT:** CHISQ.INV works with left-tail probabilities (area from 0 to x), while CHISQ.INV.RT works with right-tail probabilities (area from x to infinity). For a given probability p: CHISQ.INV(p, df) gives a smaller chi-square value than CHISQ.INV.RT(p, df). The relationship is: CHISQ.INV(p, df) = CHISQ.INV.RT(1-p, df). In hypothesis testing, CHISQ.INV.RT is more commonly used because p-values are right-tail probabilities.

## Syntax

> [!f(x)] CHISQ.INV Syntax
>
> ```
> =CHISQ.INV(probability, deg_freedom)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `probability` | Yes | The left-tail cumulative probability. Must be between 0 and 1 (exclusive of 0, inclusive of values approaching but not equal to 1). |
| `deg_freedom` | Yes | The degrees of freedom. Must be a positive integer (1, 2, 3, ...). Determines the shape of the chi-square distribution. |

### Return Value

Returns the chi-square value x such that P(X <= x) = probability for a chi-square distribution with the specified degrees of freedom. Returns #NUM! if probability <= 0 or >= 1, or if deg_freedom < 1.

## Examples

> [!f(x)] CHISQ.INV Examples

### Example 1: Basic Quantile Calculation
```
=CHISQ.INV(0.95, 10)
```
**Result:** 18.307 (approximately)

**Explanation:** The chi-square value where 95% of the distribution lies below it, with 10 degrees of freedom. 95% of chi-square(10) values are less than 18.307.

---

### Example 2: Median of Chi-Square Distribution
```
=CHISQ.INV(0.5, 5)
```
**Result:** 4.351 (approximately)

**Explanation:** The median (50th percentile) of a chi-square distribution with 5 degrees of freedom. Half of values fall below 4.351, half above.

---

### Example 3: Lower Confidence Bound for Variance
```
=CHISQ.INV(0.025, 29)
```
**Result:** 16.047 (approximately)

**Explanation:** For a 95% confidence interval with n=30 (df=29), this is the lower chi-square quantile. Used in the denominator when calculating the upper bound of variance CI.

---

### Example 4: Very Low Probability
```
=CHISQ.INV(0.01, 8)
```
**Result:** 1.646 (approximately)

**Explanation:** Only 1% of chi-square(8) values fall below 1.646. This represents an unusually small chi-square value for this distribution.

---

### Example 5: High Probability (Near Maximum)
```
=CHISQ.INV(0.99, 8)
```
**Result:** 20.090 (approximately)

**Explanation:** 99% of chi-square(8) values are below 20.090. Compare to CHISQ.INV(0.01, 8) to see the distribution's asymmetry.

---

### Example 6: Single Degree of Freedom
```
=CHISQ.INV(0.95, 1)
```
**Result:** 3.841 (approximately)

**Explanation:** With 1 degree of freedom, the chi-square distribution is highly skewed. The 95th percentile is 3.841, matching the critical value for significance testing.

---

### Example 7: Large Degrees of Freedom
```
=CHISQ.INV(0.95, 100)
```
**Result:** 124.342 (approximately)

**Explanation:** With high degrees of freedom, the chi-square distribution approaches normal. The 95th percentile of chi-square(100) is about 124.

---

### Example 8: Quartiles of Chi-Square
```
Q1: =CHISQ.INV(0.25, 6)   Result: 3.455
Q2: =CHISQ.INV(0.50, 6)   Result: 5.348
Q3: =CHISQ.INV(0.75, 6)   Result: 7.841
```
**Result:** Quartile values for chi-square with 6 degrees of freedom

**Explanation:** These three values divide the chi-square(6) distribution into four equal probability regions. The interquartile range is 7.841 - 3.455 = 4.386.

---

### Example 9: Verifying Inverse Relationship
```
Step 1: =CHISQ.INV(0.9, 5)        Result: 9.236
Step 2: =CHISQ.DIST(9.236, 5, TRUE)  Result: 0.9
```
**Result:** CHISQ.DIST returns the original probability

**Explanation:** This demonstrates the inverse relationship. CHISQ.INV finds the value; CHISQ.DIST confirms that 90% of probability lies below it.

---

### Example 10: Relationship to CHISQ.INV.RT
```
Left tail:  =CHISQ.INV(0.95, 7)     Result: 14.067
Right tail: =CHISQ.INV.RT(0.05, 7)  Result: 14.067
```
**Result:** Same chi-square value

**Explanation:** CHISQ.INV(p, df) = CHISQ.INV.RT(1-p, df). The value where 95% lies below equals the value where 5% lies above.

---

### Example 11: Confidence Interval Components
```
Lower chi-sq: =CHISQ.INV(0.025, 24)   Result: 12.401
Upper chi-sq: =CHISQ.INV(0.975, 24)  Result: 39.364
```
**Result:** Both bounds for 95% CI with 24 degrees of freedom

**Explanation:** For a two-sided 95% confidence interval, you need both the 2.5th and 97.5th percentiles. These define the range containing 95% of chi-square(24) values.

---

### Example 12: Percentile Table Generation
```
For df=10:
=CHISQ.INV(0.10, 10)  Result: 4.865
=CHISQ.INV(0.50, 10)  Result: 9.342
=CHISQ.INV(0.90, 10)  Result: 15.987
```
**Result:** 10th, 50th, and 90th percentiles

**Explanation:** Building percentile tables for reference. The 90th percentile (15.987) is much farther from the median than the 10th percentile (4.865), reflecting right-skewness.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | probability <= 0 | Probability must be greater than 0. Use a very small positive value like 0.0001 if needed. |
| `#NUM!` | probability >= 1 | Probability must be less than 1. Use a value like 0.9999 for near-maximum quantiles. |
| `#NUM!` | deg_freedom < 1 | Degrees of freedom must be at least 1. Check your df calculation. |
| `#VALUE!` | Non-numeric arguments | Ensure both parameters are numbers or cell references to numbers. |
| `Unexpected result` | Confused left/right tail | CHISQ.INV uses left-tail probability. For right-tail, use CHISQ.INV.RT or compute 1-p. |

## Use Cases

### [[Confidence Interval for Population Variance]]

**Scenario:** A quality engineer calculates a confidence interval for the variance of a manufacturing process based on sample data.

**Implementation:**
```
Sample variance: s² (calculated from data)
Lower bound: (n-1)*s² / CHISQ.INV(1-alpha/2, n-1)
Upper bound: (n-1)*s² / CHISQ.INV(alpha/2, n-1)
```

**Business Application:** Process capability analysis requires understanding variance, not just the mean. A 95% confidence interval for variance tells management the likely range of true process variability. If the upper bound exceeds specifications, process improvement is warranted even if the point estimate looks acceptable.

**Technical Details:** The confidence interval formula uses the property that (n-1)s²/sigma² follows a chi-square distribution with n-1 degrees of freedom. Note that variance CIs use CHISQ.INV differently than mean CIs use T.INV - the chi-square values appear in denominators, causing the inverse relationship between probability and bound.

---

### [[Statistical Process Control Limit Calculation]]

**Scenario:** A statistician establishes control limits for monitoring the variability of a process, using chi-square-based limits for variance charts.

**Implementation:**
```
Lower control limit factor: =CHISQ.INV(0.00135, n-1)/(n-1)
Upper control limit factor: =CHISQ.INV(0.99865, n-1)/(n-1)
```

**Business Application:** S-charts and R-charts monitor process variability. Control limits set at 3-sigma equivalent probabilities (0.00135 and 0.99865 for normal distribution) identify when variance has shifted. Early detection of variance increases prevents quality problems before they affect customers.

**Technical Details:** For subgroup size n, the sample variance follows a scaled chi-square distribution. Control limits multiply the chi-square quantiles by scaling factors. Verify limits match standard control chart tables for validation.

---

### [[Simulation and Random Number Generation]]

**Scenario:** A financial analyst generates chi-square distributed random variables for Monte Carlo simulation of option pricing models.

**Implementation:**
```
=CHISQ.INV(RAND(), degrees_of_freedom)
```
Generates one chi-square random value; copy for multiple simulations.

**Business Application:** Options and derivatives pricing models often require chi-square distributions for modeling variance processes. The inverse transform method uses CHISQ.INV with uniform random inputs to generate chi-square samples for simulation-based pricing and risk analysis.

**Technical Details:** This inverse transform method works because applying any distribution's inverse CDF to uniform(0,1) random values produces random values from that distribution. For better simulation efficiency, consider specialized algorithms for chi-square generation with small degrees of freedom.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2010 and later
- **Probability range:** 0 < probability < 1 (exclusive bounds)
- **Precision:** 15 significant decimal digits
- **Maximum deg_freedom:** 10^10
- **Numerical method:** Iterative algorithm for inverse calculation

### Google Sheets

- **Availability:** All versions since launch
- **Probability range:** 0 < probability < 1 (exclusive bounds)
- **Precision:** 15 significant decimal digits
- **Maximum deg_freedom:** Very large (practical limit)
- **Numerical method:** Similar iterative approach

### Key Difference Alert

CHISQ.INV behaves identically between Excel (2010+) and Google Sheets. Both platforms:
- Require probability strictly between 0 and 1
- Return the left-tail inverse (cumulative probability)
- Use equivalent numerical algorithms
- Return #NUM! for out-of-range inputs

No legacy function exists for CHISQ.INV (unlike CHISQ.DIST.RT which replaces CHIDIST). Older Excel versions lack this function entirely.

## Tips and Best Practices

1. **Remember left-tail vs right-tail.** CHISQ.INV finds the value where probability p lies BELOW it. For hypothesis testing critical values, you typically want CHISQ.INV.RT (right-tail). Confusion between these is a common error.

2. **Use for confidence intervals on variance.** The main application of CHISQ.INV is constructing confidence intervals for population variance. Remember that chi-square values appear in denominators, so lower chi-square gives higher variance bound.

3. **Check the inverse relationship.** Verify your CHISQ.INV result by plugging it back into CHISQ.DIST. If CHISQ.DIST(CHISQ.INV(p, df), df, TRUE) doesn't return approximately p, something is wrong.

4. **Handle edge probabilities carefully.** Probability 0 or 1 returns #NUM!. For extreme quantiles, use values like 0.0001 or 0.9999 instead.

5. **Understand degrees of freedom.** The df parameter dramatically affects results. Double-check your df calculation for the specific statistical procedure you're implementing.

6. **Consider symmetry carefully.** Unlike normal distribution, chi-square is not symmetric. The distance from median to 5th percentile is much smaller than median to 95th percentile. Don't assume symmetric intervals.

7. **Use for random number generation.** CHISQ.INV(RAND(), df) generates chi-square random values via inverse transform method. This is useful for Monte Carlo simulations.

8. **Cross-reference with tables.** Verify key values against published chi-square distribution tables. Common reference values include CHISQ.INV(0.95, 10) = 18.307 and CHISQ.INV(0.99, 5) = 15.086.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CHISQ.INV.RT]] | Inverse of right-tailed chi-square | For critical values in hypothesis testing (most common use) |
| [[CHISQ.DIST]] | Chi-square CDF or PDF | To find probability for a given chi-square value |
| [[CHISQ.DIST.RT]] | Right-tail probability | For p-values in hypothesis testing |
| [[NORM.INV]] | Normal distribution inverse | For symmetric distributions or large df approximation |
| [[T.INV]] | Student's t distribution inverse | For confidence intervals on means |

### Commonly Used Together

**[[CHISQ.INV.RT]]** - Upper tail quantiles

*Both bounds for confidence interval:*
```
Lower chi-sq: =CHISQ.INV(alpha/2, df)
Upper chi-sq: =CHISQ.INV.RT(alpha/2, df)
or equivalently: =CHISQ.INV(1-alpha/2, df)
```
For two-sided intervals, you need quantiles from both tails.

---

**[[CHISQ.DIST]]** - Verification

*Verify inverse calculation:*
```
Quantile: =CHISQ.INV(0.90, 12)          Result: 18.549
Check: =CHISQ.DIST(18.549, 12, TRUE)    Result: 0.90
```
The cumulative distribution of the quantile should return the original probability.

---

**[[VAR.S]]** or [[STDEV.S]] - Sample statistics

*Confidence interval for variance:*
```
Sample variance: =VAR.S(data_range)
Lower bound: =(COUNT(data_range)-1)*VAR.S(data_range)/CHISQ.INV(0.975, COUNT(data_range)-1)
Upper bound: =(COUNT(data_range)-1)*VAR.S(data_range)/CHISQ.INV(0.025, COUNT(data_range)-1)
```

## Official Documentation

- **Microsoft Excel:** [CHISQ.INV function](https://support.microsoft.com/en-us/office/chisq-inv-function-400db556-62b3-472d-80b3-254723e7092f)
- **Google Sheets:** [CHISQ.INV function](https://support.google.com/docs/answer/9116232)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | New function (no legacy equivalent) |
| Excel 2013+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | All versions | Available since launch |

---

*Last updated: 2026-01-10*
