---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- f-distribution
- inverse-distribution
- critical-values
- hypothesis-testing
- left-tail-probability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- F Inverse
- Inverse F Distribution
- F Critical Value Left
- FINV Left
tags:
- statistical
- f-distribution
- inverse
- critical-value
- probability
- hypothesis-testing
---

# F.INV

## Description

**F.INV** returns the inverse of the left-tailed F probability distribution, giving you the F-value below which a specified cumulative probability lies. In simpler terms, if you ask "what F-value has 90% of the distribution below it?", F.INV provides the answer. This function is the mathematical inverse of F.DIST, meaning F.DIST(F.INV(p, df1, df2), df1, df2, TRUE) = p for any valid probability p.

The function is primarily used to find F critical values for statistical testing when you need left-tail thresholds. However, since most F-tests use right-tail probabilities (testing whether variances are significantly larger), F.INV is less commonly used than its counterpart F.INV.RT. The left-tailed F critical value is mainly useful for testing whether a variance is significantly smaller than another, which is a less common scenario in practice.

**Key relationship to understand:** F.INV finds F-values based on left-tail (cumulative) probability, while F.INV.RT finds F-values based on right-tail probability. For the same significance level alpha, F.INV(1-alpha, df1, df2) equals F.INV.RT(alpha, df1, df2). This relationship is crucial because most statistical tables and software report right-tail critical values, so you need to convert when using F.INV.

**Platform behavior:** Excel and Google Sheets implement F.INV identically. Both require probability between 0 and 1 (exclusive of 0, some implementations allow 1), and both require positive integer degrees of freedom. The function returns #NUM! for out-of-range probabilities and #VALUE! for non-numeric inputs. This function replaced the legacy FINV function in Excel 2010, though the legacy version had the same functionality.

## Syntax

> [!f(x)] F.INV Syntax
>
> ```
> =F.INV(probability, deg_freedom1, deg_freedom2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `probability` | Yes | The left-tail cumulative probability associated with the F-distribution. Must be between 0 and 1. Represents the area under the curve to the LEFT of the returned F-value. |
| `deg_freedom1` | Yes | The numerator degrees of freedom. Must be a positive integer (decimals are truncated). Typically (n1 - 1) or (groups - 1) depending on the test. |
| `deg_freedom2` | Yes | The denominator degrees of freedom. Must be a positive integer (decimals are truncated). Typically (n2 - 1) or (N - groups) depending on the test. |

### Return Value

Returns a non-negative numeric value representing the F-value at which the cumulative (left-tail) probability equals the specified probability. Returns #NUM! if probability is less than or equal to 0 or greater than or equal to 1, or if degrees of freedom are less than 1. Returns #VALUE! if arguments cannot be converted to numbers.

## Examples

> [!f(x)] F.INV Examples

### Example 1: Basic Left-Tail Critical Value
```
=F.INV(0.95, 5, 20)
```
**Result:** 2.711 (approximately)

**Explanation:** Returns the F-value below which 95% of the F-distribution lies with df1=5 and df2=20. This is the left-tail critical value at the 95th percentile.

---

### Example 2: 90th Percentile F-Value
```
=F.INV(0.90, 3, 30)
```
**Result:** 2.276 (approximately)

**Explanation:** 90% of F-values with these degrees of freedom fall below 2.276. Only 10% exceed this threshold, making it useful for understanding the distribution shape.

---

### Example 3: Median of F-Distribution
```
=F.INV(0.50, 10, 10)
```
**Result:** 1.000 (approximately)

**Explanation:** The median (50th percentile) of an F-distribution with equal degrees of freedom is very close to 1. This makes intuitive sense: when comparing two variance estimates from equal samples, you expect the ratio to be around 1.

---

### Example 4: Finding Lower Confidence Bound
```
=F.INV(0.025, 9, 14)
```
**Result:** 0.273 (approximately)

**Explanation:** For a 95% two-sided confidence interval for variance ratio, you need both lower (2.5th percentile) and upper bounds. This gives the lower bound F-value.

---

### Example 5: Converting to Right-Tail Equivalent
```
=F.INV(0.95, 4, 25)
```
**Result:** 2.759 (approximately)

**Explanation:** F.INV(0.95, 4, 25) gives the same result as F.INV.RT(0.05, 4, 25). The 95th percentile from the left equals the 5% right-tail critical value.

---

### Example 6: Very Small Probability
```
=F.INV(0.01, 5, 30)
```
**Result:** 0.178 (approximately)

**Explanation:** Only 1% of F-values with df1=5, df2=30 fall below 0.178. This is an unusually small variance ratio, potentially indicating the denominator variance is significantly larger.

---

### Example 7: Large Degrees of Freedom
```
=F.INV(0.975, 100, 100)
```
**Result:** 1.394 (approximately)

**Explanation:** With large, equal degrees of freedom, the F-distribution becomes tighter around 1. The 97.5th percentile is only 1.394, compared to much larger values with small df.

---

### Example 8: Verifying Inverse Relationship with F.DIST
```
=F.DIST(F.INV(0.80, 6, 15), 6, 15, TRUE)
```
**Result:** 0.80

**Explanation:** This confirms F.INV and F.DIST are mathematical inverses. The cumulative probability of the F-value returned by F.INV equals the original probability.

---

### Example 9: Comparing Different df Combinations
```
=F.INV(0.95, 2, 50)
```
**Result:** 3.183 (approximately)

```
=F.INV(0.95, 50, 2)
```
**Result:** 19.471 (approximately)

**Explanation:** Swapping degrees of freedom dramatically changes the result. With small numerator df, the 95th percentile is much lower than with small denominator df. Order matters significantly.

---

### Example 10: Regression F-Test Lower Bound
```
=F.INV(0.10, 3, 46)
```
**Result:** 0.471 (approximately)

**Explanation:** For a regression model with 3 predictors and 50 observations, this gives the F-value below which only 10% of random F-statistics would fall. Useful for understanding just how small an F-value could be by chance.

---

### Example 11: Building Confidence Interval for Variance Ratio
```
Lower: =VAR.S(A:A)/VAR.S(B:B) / F.INV(0.975, COUNT(A:A)-1, COUNT(B:B)-1)
Upper: =VAR.S(A:A)/VAR.S(B:B) * F.INV(0.975, COUNT(B:B)-1, COUNT(A:A)-1)
```
**Result:** 95% confidence interval for true variance ratio

**Explanation:** Uses F.INV to construct confidence bounds for the ratio of two population variances. Note the swapped df in the upper bound calculation.

---

### Example 12: Dynamic Percentile Lookup
```
=F.INV(B2, C2, D2)
```
Where B2 = desired percentile (e.g., 0.95), C2 = df1, D2 = df2

**Result:** F critical value for specified percentile

**Explanation:** Creates a flexible formula where you can change the probability and degrees of freedom to get different critical values without modifying the formula.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Probability is 0, negative, or >= 1 | Ensure probability is strictly between 0 and 1. Use 0.001 instead of 0, or 0.999 instead of 1. |
| `#NUM!` | Degrees of freedom less than 1 | Both df1 and df2 must be at least 1. Verify your sample size calculations. |
| `#VALUE!` | Non-numeric arguments | Ensure all parameters are numbers or cell references to numbers. Check for text or empty cells. |
| `Wrong critical value` | Used F.INV instead of F.INV.RT | For right-tail hypothesis tests (most common), use F.INV.RT or convert using F.INV(1-alpha, df1, df2). |
| `Unexpected result` | Degrees of freedom reversed | The order of df1 and df2 significantly affects results. Verify which sample corresponds to numerator vs. denominator. |
| `Result seems too small` | Misunderstanding left-tail interpretation | F.INV returns values where left-tail probability equals p. For p=0.05, it returns a small F-value (5th percentile), not the 95th. |

## Use Cases

### [[Confidence Intervals for Variance Ratios]]

**Scenario:** A quality control engineer needs to construct a confidence interval for the ratio of variances between two production processes to determine if their precision is statistically different.

**Implementation:**
```
Sample Variance Ratio: =VAR.S(Process1)/VAR.S(Process2)
Lower CI Bound: =Variance_Ratio / F.INV(0.975, n1-1, n2-1)
Upper CI Bound: =Variance_Ratio * F.INV(0.975, n2-1, n1-1)
Interpretation: =IF(AND(Lower_CI<1, Upper_CI>1), "Variances not significantly different", "Variances significantly different")
```

**Business Application:** Provides a range of plausible values for the true variance ratio, enabling informed decisions about process equivalence. If the confidence interval includes 1, the processes have statistically similar precision.

**Technical Details:** Note the swapped degrees of freedom between lower and upper bounds. This arises from the reciprocal relationship: if F is F-distributed with (df1, df2), then 1/F is F-distributed with (df2, df1).

---

### [[Understanding F-Distribution Quantiles]]

**Scenario:** A statistics instructor needs to demonstrate the shape and properties of the F-distribution to students, showing how it changes with different degrees of freedom.

**Implementation:**
```
1st Percentile: =F.INV(0.01, df1, df2)
5th Percentile: =F.INV(0.05, df1, df2)
25th Percentile: =F.INV(0.25, df1, df2)
50th Percentile: =F.INV(0.50, df1, df2)
75th Percentile: =F.INV(0.75, df1, df2)
95th Percentile: =F.INV(0.95, df1, df2)
99th Percentile: =F.INV(0.99, df1, df2)
```

**Business Application:** Educational tool for understanding probability distributions. Shows how the F-distribution is right-skewed (median < mean) and how the spread decreases with larger degrees of freedom.

**Technical Details:** Create a table with varying df combinations to show how the distribution shape changes. Students can see that with small df, the distribution has a long right tail, while large df produces a tighter, more symmetric shape.

---

### [[Testing Whether Variance Is Smaller]]

**Scenario:** A pharmaceutical company needs to verify that a new manufacturing process has LOWER variability than the old process (not just different, but specifically smaller).

**Implementation:**
```
Variance Ratio: =VAR.S(NewProcess)/VAR.S(OldProcess)
Critical Value: =F.INV(Alpha, n_new-1, n_old-1)
Conclusion: =IF(Variance_Ratio < Critical_Value, "New process has significantly lower variance", "Cannot conclude lower variance")
```

**Business Application:** When the goal is specifically to show reduced variability (one-sided test), you test in the left tail. If the new process variance is significantly smaller, you can claim improved consistency.

**Technical Details:** This is one of the few scenarios where F.INV is used directly rather than F.INV.RT. You are asking: "Is the variance ratio so small that it would rarely occur if variances were equal?"

---

### [[Generating F-Distribution Quantile Tables]]

**Scenario:** A researcher needs to create custom F-distribution tables for reference in reports where electronic calculation is not available.

**Implementation:**
```
For each combination of df1, df2, and alpha:
=F.INV(1-Alpha, df1, df2)
```

**Business Application:** Creates printed reference materials for field work or environments without spreadsheet access. Useful for audit documentation or exam preparation materials.

**Technical Details:** Note that traditional F-tables show right-tail critical values, so use F.INV(1-alpha) or F.INV.RT(alpha) to match the conventional table format.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Replaces:** Legacy FINV function (which had same functionality)
- **Precision:** 15 significant digits
- **Behavior:** Returns #NUM! for probability outside (0,1) exclusive

### Google Sheets
- **Availability:** All versions
- **Function:** Identical to Excel implementation
- **Precision:** 15 significant digits
- **Behavior:** Same error handling as Excel

### Key Difference Alert
There are no functional differences between Excel and Google Sheets for F.INV. Both platforms produce identical results for valid inputs and identical errors for invalid inputs. The function behavior matches exactly across platforms.

## Tips and Best Practices

1. **Know when to use F.INV vs F.INV.RT:** Most statistical tests use right-tail probabilities. F.INV is for left-tail probabilities. If you want the F-value that cuts off the top 5%, use F.INV.RT(0.05,...) or equivalently F.INV(0.95,...).

2. **Relationship formula:** F.INV(p, df1, df2) = F.INV.RT(1-p, df1, df2). Memorize this to convert between the two functions.

3. **For confidence intervals, use both bounds:** Variance ratio confidence intervals require both F.INV for the lower bound and attention to swapping df for the upper bound due to the reciprocal relationship.

4. **Check that probability is strictly between 0 and 1:** F.INV(0,...) and F.INV(1,...) return errors. Use values like 0.001 or 0.999 to approximate the extremes.

5. **Verify results with F.DIST:** Always sanity-check by confirming F.DIST(F.INV(p, df1, df2), df1, df2, TRUE) returns your original p.

6. **Document which critical value you need:** In reports, clearly state whether you used left-tail or right-tail critical values to avoid confusion during review.

7. **Remember df order matters:** Unlike symmetric distributions, swapping df1 and df2 in F-distributions gives completely different results. Double-check which variance goes in the numerator.

8. **Use named ranges for clarity:** In complex formulas, use named ranges like df_between and df_within rather than raw numbers to make the statistical logic clear.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[F.INV.RT]] | Inverse of right-tailed F distribution | For most hypothesis tests where you need upper critical values |
| [[F.DIST]] | Left-tailed cumulative F distribution | When you have an F-value and need the cumulative probability |
| [[F.DIST.RT]] | Right-tailed F distribution | When you have an F-value and need the p-value (right-tail) |
| [[FINV]] | Legacy inverse F function | Only for backward compatibility; use F.INV in new work |

### Commonly Used Together

**[[F.DIST]]** - Cumulative F distribution

*Verify inverse relationship:*
```
=F.DIST(F.INV(0.90, 5, 20), 5, 20, TRUE)
```
Returns 0.90, confirming F.INV and F.DIST are inverses.

---

**[[VAR.S]]** - Sample variance

*Calculate variance ratio for confidence interval:*
```
Ratio: =VAR.S(Sample1)/VAR.S(Sample2)
CI calculation uses this ratio with F.INV bounds
```
VAR.S provides the point estimate that gets bounded by F.INV-derived critical values.

---

**[[F.INV.RT]]** - Right-tailed inverse F distribution

*Compare left and right tail approaches:*
```
Left approach: =F.INV(0.95, 5, 20)
Right approach: =F.INV.RT(0.05, 5, 20)
```
Both return the same value, demonstrating the complementary relationship.

---

**[[COUNT]]** - Count numeric values

*Dynamic degrees of freedom:*
```
=F.INV(0.975, COUNT(A:A)-1, COUNT(B:B)-1)
```
COUNT determines sample sizes for automatic df calculation.

---

**[[CONFIDENCE.T]]** - Confidence interval for mean

*Complete statistical inference:*
```
Mean CI: Uses CONFIDENCE.T
Variance CI: Uses F.INV for bounds
```
F.INV complements t-based confidence intervals for complete inference.

## Official Documentation

- **Microsoft Excel:** [F.INV function](https://support.microsoft.com/en-us/office/f-inv-function-0dda0cf9-4ea0-42fd-8c3c-417a1ff30dbe)
- **Google Sheets:** [F.INV function](https://support.google.com/docs/answer/9116168)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Introduced to replace FINV with consistent naming |
| Excel 2013+ | All subsequent versions | No changes to functionality |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Available since 2010 | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
