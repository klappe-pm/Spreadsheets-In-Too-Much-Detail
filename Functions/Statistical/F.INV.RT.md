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
- right-tail-probability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- F Inverse Right Tail
- F Critical Value
- Inverse F Distribution Right
- FINV
tags:
- statistical
- f-distribution
- inverse
- critical-value
- probability
- hypothesis-testing
- anova
---

# F.INV.RT

## Description

**F.INV.RT** returns the inverse of the right-tailed F probability distribution, giving you the critical F-value above which a specified probability lies. This is the most commonly used F inverse function because most statistical tests involving the F-distribution (ANOVA, regression F-tests, variance comparisons) use right-tail probabilities. When you need to find "the F critical value at alpha = 0.05," F.INV.RT is your function.

The function answers the question: "What F-value must I exceed to be in the top X% of the distribution?" For example, F.INV.RT(0.05, 3, 36) tells you the F-value that only 5% of random samples would exceed if the null hypothesis (equal variances or means) were true. This is the threshold against which you compare your calculated F-statistic to determine statistical significance.

**Critical relationship:** F.INV.RT is the mathematical inverse of F.DIST.RT, meaning F.DIST.RT(F.INV.RT(p, df1, df2), df1, df2) = p. Additionally, F.INV.RT(alpha, df1, df2) = F.INV(1-alpha, df1, df2), so you can always convert between right-tail and left-tail formulations. Most practitioners prefer F.INV.RT because it directly accepts the significance level (alpha) as input.

**Platform behavior:** Excel and Google Sheets implement F.INV.RT identically with the same parameters and precision. Both require probability strictly between 0 and 1, both require positive integer degrees of freedom, and both return #NUM! for invalid inputs. The function was introduced in Excel 2010 to replace the older FINV function with a more consistently named alternative.

## Syntax

> [!f(x)] F.INV.RT Syntax
>
> ```
> =F.INV.RT(probability, deg_freedom1, deg_freedom2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `probability` | Yes | The right-tail probability (alpha level) for which to find the critical F-value. Must be between 0 and 1. Common values are 0.05, 0.01, and 0.10 for 5%, 1%, and 10% significance levels. |
| `deg_freedom1` | Yes | The numerator degrees of freedom. For ANOVA, this is (number of groups - 1). For regression, this is the number of predictors. Must be a positive integer. |
| `deg_freedom2` | Yes | The denominator degrees of freedom. For ANOVA, this is (total N - number of groups). For regression, this is (n - k - 1). Must be a positive integer. |

### Return Value

Returns a positive numeric value representing the F critical value. Values of F greater than this threshold have probability less than the specified alpha of occurring by chance. Returns #NUM! if probability is outside (0, 1) or if degrees of freedom are less than 1. Returns #VALUE! if arguments are non-numeric.

## Examples

> [!f(x)] F.INV.RT Examples

### Example 1: Standard ANOVA Critical Value (alpha = 0.05)
```
=F.INV.RT(0.05, 3, 36)
```
**Result:** 2.866 (approximately)

**Explanation:** For a one-way ANOVA with 4 groups and 40 total observations, the critical F-value at the 5% significance level is 2.866. If your calculated F-statistic exceeds this, reject the null hypothesis.

---

### Example 2: More Stringent Test (alpha = 0.01)
```
=F.INV.RT(0.01, 3, 36)
```
**Result:** 4.377 (approximately)

**Explanation:** The same ANOVA setup with alpha = 0.01 requires F > 4.377 for significance. The more stringent criterion requires a larger F-value to reject the null hypothesis.

---

### Example 3: Regression F-Test Critical Value
```
=F.INV.RT(0.05, 4, 45)
```
**Result:** 2.579 (approximately)

**Explanation:** For multiple regression with 4 predictors and 50 observations (df2 = 50 - 4 - 1 = 45), the critical F at alpha = 0.05 is 2.579. The overall model is significant if the regression F exceeds this value.

---

### Example 4: Two-Sample Variance Test
```
=F.INV.RT(0.05, 14, 19)
```
**Result:** 2.256 (approximately)

**Explanation:** When comparing variances of two samples with n1=15 and n2=20 (df1=14, df2=19), F > 2.256 indicates the first sample has significantly higher variance at the 5% level.

---

### Example 5: Large Degrees of Freedom
```
=F.INV.RT(0.05, 50, 100)
```
**Result:** 1.444 (approximately)

**Explanation:** With large samples, the F critical value at 5% is much smaller than with small samples. This reflects the increased precision of variance estimates with larger n.

---

### Example 6: Equal Degrees of Freedom
```
=F.INV.RT(0.05, 20, 20)
```
**Result:** 2.124 (approximately)

**Explanation:** When both samples have 21 observations (df = 20 each), an F-value exceeding 2.124 indicates significant variance difference. Note that F critical values are not symmetric around 1 due to the distribution's skewness.

---

### Example 7: Small Sample Critical Value
```
=F.INV.RT(0.05, 2, 5)
```
**Result:** 5.786 (approximately)

**Explanation:** With very small samples, critical values are much larger. You need much stronger evidence to claim significance with limited data.

---

### Example 8: 10% Significance Level
```
=F.INV.RT(0.10, 4, 30)
```
**Result:** 2.142 (approximately)

**Explanation:** At 10% significance (more lenient), the critical F is 2.142. Compare with 2.690 at 5% significance for the same df. Choosing alpha affects how easily you reject the null.

---

### Example 9: Verifying with F.DIST.RT
```
=F.DIST.RT(F.INV.RT(0.05, 5, 25), 5, 25)
```
**Result:** 0.05

**Explanation:** This confirms F.INV.RT and F.DIST.RT are mathematical inverses. The right-tail probability of the critical value equals the input alpha.

---

### Example 10: Dynamic Significance Test
```
=IF(F_calc > F.INV.RT(0.05, df1, df2), "Significant", "Not Significant")
```
**Result:** "Significant" or "Not Significant"

**Explanation:** Compares a calculated F-statistic directly against the critical value to automate the hypothesis test conclusion.

---

### Example 11: Creating Critical Value Lookup
```
=F.INV.RT(Alpha_Cell, Groups_Cell - 1, N_Cell - Groups_Cell)
```
**Result:** Critical F-value based on input parameters

**Explanation:** Creates a flexible tool where changing alpha, number of groups, or sample size automatically updates the critical value. Useful for power analysis and study planning.

---

### Example 12: Multiple Alpha Levels for Reporting
```
Critical at 10%: =F.INV.RT(0.10, 3, 36)
Critical at 5%: =F.INV.RT(0.05, 3, 36)
Critical at 1%: =F.INV.RT(0.01, 3, 36)
```
**Result:** 2.243, 2.866, 4.377 (approximately)

**Explanation:** Displays critical values at multiple significance levels for comprehensive reporting. If F = 3.5, significant at 5% but not at 1%.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Probability is 0, negative, or >= 1 | Use probability strictly between 0 and 1. For extreme values, use 0.001 or 0.999. |
| `#NUM!` | Degrees of freedom less than 1 | Both df1 and df2 must be at least 1. Verify sample sizes are adequate. |
| `#VALUE!` | Non-numeric arguments | Ensure all parameters are numbers. Check for text, empty cells, or formulas returning errors. |
| `Critical value too high` | Degrees of freedom are too small | Small samples produce large critical values. Consider increasing sample size for more power. |
| `Unexpected result` | df1 and df2 swapped | Order matters significantly. df1 is numerator (between-groups), df2 is denominator (within-groups). |
| `Test never significant` | Using wrong alpha | Verify you are using the intended significance level. Alpha = 0.05 is common, not 0.95. |

## Use Cases

### [[ANOVA Critical Value Determination]]

**Scenario:** A market researcher conducts a one-way ANOVA comparing customer satisfaction scores across five different store locations with a total of 150 customers surveyed.

**Implementation:**
```
df1 (between groups): =5 - 1 = 4
df2 (within groups): =150 - 5 = 145
Critical F at 5%: =F.INV.RT(0.05, 4, 145)
Test Result: =IF(Calculated_F > F.INV.RT(0.05, 4, 145), "Locations differ significantly", "No significant difference")
```

**Business Application:** Determines whether observed differences in satisfaction scores reflect true location effects or random variation. Significant results warrant investigation of what makes high-performing locations different.

**Technical Details:** Always report both the calculated F and critical F in publications. Include degrees of freedom for reproducibility: F(4, 145) = calculated value, p < 0.05 (or not).

---

### [[Regression Model Overall Significance]]

**Scenario:** A financial analyst builds a model predicting quarterly revenue using 6 economic indicators with 80 quarters of historical data.

**Implementation:**
```
df1 (regression): =6 (number of predictors)
df2 (residual): =80 - 6 - 1 = 73
Critical F: =F.INV.RT(0.05, 6, 73)
Model Test: =IF(Model_F > F.INV.RT(0.05, 6, 73), "Model is significant", "Model not significant")
```

**Business Application:** Validates that the model has genuine predictive power before using it for forecasting. A non-significant F-test means the model explains no more variance than a constant-only model.

**Technical Details:** Even if overall F is significant, individual predictors may not be. Use t-tests on individual coefficients after confirming overall model significance.

---

### [[Quality Control Variance Comparison]]

**Scenario:** A manufacturing engineer needs to determine if a process change has reduced product variability. The old process had variance data from 30 samples; the new process has 25 samples.

**Implementation:**
```
Old Variance: =VAR.S(OldData)  (assume = 2.5)
New Variance: =VAR.S(NewData)  (assume = 1.8)
F-Statistic: =2.5/1.8 = 1.389 (larger/smaller)
Critical F: =F.INV.RT(0.05, 29, 24) = 1.966
Conclusion: Since 1.389 < 1.966, cannot conclude variances differ at 5% level
```

**Business Application:** Determines whether process improvements have genuinely reduced variability or whether observed differences could be due to sampling variation. Guides resource allocation for process improvements.

**Technical Details:** For one-sided test (new variance is smaller), compare F to critical value. For two-sided (variances simply differ), use alpha/2 for each tail or use F.TEST function directly.

---

### [[Experimental Design Power Analysis]]

**Scenario:** Before conducting an expensive clinical trial, a researcher needs to determine whether the planned sample size provides adequate power to detect expected treatment effects.

**Implementation:**
```
For various sample sizes N and expected effect size:
Critical F: =F.INV.RT(0.05, k-1, N-k)
Noncentrality parameter: =N * effect_size^2 / k
Power: Calculated using noncentral F distribution
```

**Business Application:** Ensures research investments have reasonable probability of detecting real effects if they exist. Prevents underpowered studies that waste resources or overpowered studies that delay results.

**Technical Details:** Power analysis requires both the critical F (from F.INV.RT) and the expected non-central F distribution. Spreadsheets cannot directly calculate power but can provide critical values for external power calculators.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Replaces:** Legacy FINV function (with same right-tail functionality)
- **Precision:** 15 significant digits
- **Behavior:** Returns #NUM! for probability outside (0,1) exclusive

### Google Sheets
- **Availability:** All versions
- **Function:** Identical to Excel implementation
- **Precision:** 15 significant digits
- **Behavior:** Same error handling and results as Excel

### Key Difference Alert
There are no functional differences between Excel and Google Sheets for F.INV.RT. Both platforms produce identical results for all valid inputs. The legacy FINV function (same functionality, different naming convention) is available in both for backward compatibility but F.INV.RT is preferred for new work.

## Tips and Best Practices

1. **F.INV.RT is your primary F inverse function:** For most statistical tests (ANOVA, regression, variance comparisons), you need right-tail critical values. F.INV.RT directly gives you what you need with alpha as the input.

2. **Match alpha to your significance level:** If you want a 5% significance test, use F.INV.RT(0.05, ...). Do not use 0.95 - that would be testing at 95% significance (extremely lenient).

3. **Always report df with critical values:** F critical values are meaningless without their degrees of freedom. Report as "F(df1, df2) = critical value" for reproducibility.

4. **Consider multiple comparison corrections:** For ANOVA with many groups, the family-wise error rate inflates. Consider Bonferroni correction (use alpha/number of comparisons) when making multiple tests.

5. **Check both calculated F and critical F:** Before concluding significance, verify your calculated F-statistic is correct. A common error is dividing smaller variance by larger, giving F < 1.

6. **Use for two-tailed tests carefully:** F.INV.RT gives one-tail critical values. For two-tailed variance tests, use alpha/2 for each tail or simply use F.TEST for the two-tailed p-value.

7. **Understand the power implications:** With small df (small samples), critical F values are high, requiring very large effects to detect. Consider this when designing studies.

8. **Combine with IFERROR for robustness:** In automated analyses, wrap F.INV.RT in IFERROR to handle edge cases where df might be invalid.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[F.INV]] | Inverse of left-tailed F distribution | When you specifically need left-tail quantiles (rare) |
| [[F.DIST.RT]] | Right-tailed F probability | When you have an F-value and need the p-value |
| [[F.DIST]] | Left-tailed cumulative F distribution | When you need cumulative probability from left |
| [[F.TEST]] | Two-tailed F-test p-value | When comparing two sample variances directly |
| [[FINV]] | Legacy right-tailed F inverse | Only for backward compatibility |

### Commonly Used Together

**[[F.DIST.RT]]** - Right-tailed F probability

*Complete hypothesis test workflow:*
```
Critical Value: =F.INV.RT(0.05, df1, df2)
P-Value: =F.DIST.RT(F_calculated, df1, df2)
Decision by critical value: =IF(F_calculated > F.INV.RT(0.05, df1, df2), "Reject H0", "Fail to reject")
Decision by p-value: =IF(F.DIST.RT(F_calculated, df1, df2) < 0.05, "Reject H0", "Fail to reject")
```
Both methods give identical conclusions; use whichever is more intuitive.

---

**[[VAR.S]]** - Sample variance

*Calculate F-statistic for variance comparison:*
```
F-Statistic: =VAR.S(Sample1)/VAR.S(Sample2)
Critical: =F.INV.RT(0.05, COUNT(Sample1)-1, COUNT(Sample2)-1)
Significant: =VAR.S(Sample1)/VAR.S(Sample2) > F.INV.RT(0.05, COUNT(Sample1)-1, COUNT(Sample2)-1)
```
Always put larger variance in numerator to test if significantly larger.

---

**[[COUNT]]** - Count numeric values

*Dynamic degrees of freedom:*
```
=F.INV.RT(0.05, Num_Groups - 1, COUNT(AllData) - Num_Groups)
```
COUNT determines total N for automatic df2 calculation.

---

**[[IF]]** - Conditional logic

*Automatic significance determination:*
```
=IF(F_stat > F.INV.RT(Alpha, df1, df2), "Significant at " & TEXT(Alpha, "0%"), "Not significant")
```
Creates clear, readable test conclusions.

---

**[[AVERAGE]] and [[ANOVA Analysis]]** - Mean comparisons

*Complete ANOVA workflow:*
```
Group Means: =AVERAGE(Group1), =AVERAGE(Group2), etc.
Overall Mean: =AVERAGE(AllData)
F-Statistic: [calculated from between/within MS]
Critical: =F.INV.RT(0.05, k-1, N-k)
```
F.INV.RT provides the decision threshold for comparing group means.

## Official Documentation

- **Microsoft Excel:** [F.INV.RT function](https://support.microsoft.com/en-us/office/f-inv-rt-function-d371aa8f-b0c1-4f4e-bcdb-3cd3d22ffe1a)
- **Google Sheets:** [F.INV.RT function](https://support.google.com/docs/answer/9116267)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Introduced with consistent .RT naming convention |
| Excel 2013+ | All subsequent versions | No changes to functionality |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Available since 2010 | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
