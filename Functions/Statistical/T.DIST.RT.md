---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- t-distribution
- right-tail-probability
- one-tailed-test
- hypothesis-testing
- p-value
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- T Distribution Right Tail
- Right-Tailed T Probability
- One-Tailed T Test
- Student T Right
tags:
- statistical
- t-distribution
- probability
- hypothesis-testing
- one-tailed
- p-value
---

# T.DIST.RT

## Description

**T.DIST.RT** returns the right-tailed probability of the Student's t-distribution, representing the probability that a t-distributed random variable is greater than or equal to the specified value. This function is essential for one-tailed hypothesis tests where you have a directional hypothesis, such as "The treatment mean is greater than the control mean" rather than simply "The means are different."

The function calculates P(T >= x), which is the area under the t-distribution curve to the right of x. This directly gives you the p-value for a one-tailed (upper-tail) hypothesis test. For lower-tail tests (testing if something is significantly smaller), you would use T.DIST with cumulative=TRUE, or equivalently 1 - T.DIST.RT.

**Relationship to other t functions:** T.DIST.RT(x, df) = 1 - T.DIST(x, df, TRUE) and T.DIST.2T(x, df) = 2 * T.DIST.RT(x, df) when x >= 0. Understanding these relationships helps you navigate between different testing scenarios and verify your results.

**Platform behavior:** Excel and Google Sheets implement T.DIST.RT identically. Both accept any real number for x (positive or negative), both require degrees of freedom >= 1, and both return probabilities between 0 and 1. The function was introduced in Excel 2010 as part of the ".RT" naming convention for right-tailed distributions.

## Syntax

> [!f(x)] T.DIST.RT Syntax
>
> ```
> =T.DIST.RT(x, deg_freedom)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `x` | Yes | The t-statistic value at which to evaluate the right-tail probability. Can be any real number (positive or negative). Positive values give probabilities < 0.5; negative values give probabilities > 0.5. |
| `deg_freedom` | Yes | The degrees of freedom. Must be a positive integer (decimals are truncated). For one-sample t-test: n - 1. For two-sample tests: varies by method. |

### Return Value

Returns a decimal value between 0 and 1 representing the probability that a t-distributed random variable exceeds the specified x value. This is the p-value for a right-tailed (upper-tail) hypothesis test. Returns #NUM! if degrees of freedom < 1. Returns #VALUE! if arguments are non-numeric.

## Examples

> [!f(x)] T.DIST.RT Examples

### Example 1: Basic Right-Tail P-Value
```
=T.DIST.RT(2.0, 20)
```
**Result:** 0.0298 (approximately)

**Explanation:** With t = 2.0 and df = 20, the probability of getting a t-value this large or larger is about 2.98%. This is significant at the 5% level for a one-tailed test.

---

### Example 2: Comparing to Two-Tailed Result
```
Right-tailed: =T.DIST.RT(2.5, 15)
Two-tailed: =T.DIST.2T(2.5, 15)
```
**Result:** 0.0122 and 0.0244 (approximately)

**Explanation:** The two-tailed p-value (0.0244) is exactly twice the one-tailed (0.0122). This relationship always holds for positive t-values, confirming you are using the right approach.

---

### Example 3: Negative T-Value (Opposite Direction)
```
=T.DIST.RT(-2.0, 20)
```
**Result:** 0.9702 (approximately)

**Explanation:** When t is negative, the right-tail probability is high (> 0.5) because most of the distribution lies to the right of a negative value. This is NOT significant for an upper-tail test.

---

### Example 4: Large Degrees of Freedom
```
=T.DIST.RT(1.645, 1000)
```
**Result:** 0.0501 (approximately)

**Explanation:** With large df, the t-distribution approaches normal. The famous 1.645 z-score gives approximately 5% in the upper tail, and here we get nearly the same for t with df=1000.

---

### Example 5: Small Degrees of Freedom (Wide Distribution)
```
=T.DIST.RT(2.5, 5)
```
**Result:** 0.0274 (approximately)

**Explanation:** Even with small df (heavy tails), a t-value of 2.5 is still significant at 5% for a one-tailed test. Compare to df=20 where the same t gives p=0.011.

---

### Example 6: One-Sample Upper-Tail Test
```
Mean: =AVERAGE(A1:A30)
T-Stat: =(AVERAGE(A1:A30)-100)/(STDEV.S(A1:A30)/SQRT(30))
P-Value: =T.DIST.RT(T_Stat, 29)
```
**Result:** P-value for testing if population mean > 100

**Explanation:** Tests the directional hypothesis that the population mean exceeds 100. Use this when you specifically hypothesize "greater than," not just "different from."

---

### Example 7: Critical Value Verification
```
=T.DIST.RT(T.INV.RT(0.05, 25), 25)
```
**Result:** 0.05

**Explanation:** Confirms T.INV.RT and T.DIST.RT are inverses. The critical value at alpha = 0.05 produces exactly 5% in the right tail.

---

### Example 8: Converting from Two-Tailed P-Value
```
Two-tailed p-value: 0.08
One-tailed p-value: =0.08/2
Equivalent to: =T.DIST.RT(T.INV.2T(0.08, 20), 20)
```
**Result:** 0.04

**Explanation:** If you have a two-tailed p-value and need one-tailed (with the correct sign), divide by 2. This assumes your observed effect is in the hypothesized direction.

---

### Example 9: Highly Significant One-Tailed Result
```
=T.DIST.RT(4.5, 30)
```
**Result:** 0.0000446 (approximately)

**Explanation:** T = 4.5 with df = 30 gives an extremely small p-value (< 0.001). Strong evidence that the population parameter exceeds the null hypothesis value.

---

### Example 10: Not Significant (Wrong Direction)
```
Hypothesis: Mean > 100
Observed: Mean = 95
T-Stat: =-1.8 (negative because observed < hypothesized)
P-Value: =T.DIST.RT(-1.8, 24)
```
**Result:** 0.958 (approximately)

**Explanation:** The negative t-statistic means the observed mean is below the hypothesized value. The right-tail p-value is high, indicating no support for the "greater than" hypothesis.

---

### Example 11: Automatic One-Tailed Test Decision
```
=IF(T.DIST.RT(B2, C2) < 0.05, "Significantly greater", "Not significantly greater")
```
Where B2 = t-statistic, C2 = df

**Result:** Decision for upper-tail test

**Explanation:** Automates the one-tailed significance decision. Only concludes "significantly greater" when the t-statistic is positive and large enough.

---

### Example 12: Power Analysis Component
```
Critical t: =T.INV.RT(0.05, 29)
Probability of exceeding under H1: =T.DIST.RT(Critical_t - Effect_Size*SQRT(30), 29)
```
**Result:** Approximate power for detecting effect

**Explanation:** Power analysis requires finding the probability of rejection under the alternative hypothesis, which uses T.DIST.RT with a shifted distribution.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Degrees of freedom less than 1 | df must be at least 1. Check that sample size >= 2. |
| `#VALUE!` | Non-numeric arguments | Ensure both parameters are numbers. Check for text or empty cells. |
| `P-value > 0.5` | T-statistic is negative | A negative t-stat gives high right-tail probability. Check if you should use left-tail (T.DIST) instead. |
| `Wrong interpretation` | Used right-tail for two-tailed test | T.DIST.RT is for one-tailed tests. For two-tailed, use T.DIST.2T or multiply by 2. |
| `P-value very small but not significant` | Comparing to wrong alpha | For one-tailed tests, compare directly to alpha (e.g., 0.05). Do not halve alpha like you would for two-tailed. |

## Use Cases

### [[One-Sided Hypothesis Testing]]

**Scenario:** A pharmaceutical company tests whether a new drug increases average patient recovery rate compared to the standard treatment, with the hypothesis specifically that the new drug is better (not just different).

**Implementation:**
```
Recovery Rate Difference: =AVERAGE(NewDrug) - AVERAGE(StandardDrug)
Pooled SE: =Pooled standard error calculation
T-Statistic: =Recovery_Rate_Difference / Pooled_SE
P-Value: =T.DIST.RT(T_Statistic, df)
Conclusion: =IF(AND(T_Statistic > 0, P_Value < 0.05), "New drug significantly better", "Insufficient evidence")
```

**Business Application:** Supports regulatory submission by demonstrating the drug improves outcomes, not just changes them. FDA often requires directional evidence of improvement for new drug approval.

**Technical Details:** Only use one-tailed tests when there is a strong prior reason to expect a specific direction AND you would not care about or act on results in the opposite direction.

---

### [[A/B Testing Web Optimization]]

**Scenario:** A marketing team tests whether a new website design increases conversion rate, hypothesizing that the new design will perform better, not just differently.

**Implementation:**
```
Conversion Lift: =(New_Rate - Old_Rate) / Old_Rate
T-Statistic: Calculated from proportions test
P-Value: =T.DIST.RT(T_Statistic, Large_df_for_proportions)
Decision: =IF(AND(T_Stat>0, T.DIST.RT(T_Stat, df)<0.05), "Launch new design", "Keep current")
```

**Business Application:** Provides statistical confidence before rolling out changes that involve development investment. Ensures changes genuinely improve metrics rather than just showing random variation.

**Technical Details:** For proportion comparisons, z-tests are often used instead of t-tests, but with large samples, results are nearly identical. T.DIST.RT approximates the normal right tail when df is large.

---

### [[Quality Improvement Verification]]

**Scenario:** A process engineer tests whether a new manufacturing process reduces defect rates, with the specific hypothesis that defects will decrease (not just change).

**Implementation:**
```
Defect Reduction: =Old_Defect_Rate - New_Defect_Rate
T-Statistic: =Defect_Reduction / SE_of_Reduction
P-Value: =T.DIST.RT(T_Statistic, N_batches - 1)
Report: =IF(T.DIST.RT(T_Stat, df) < 0.05, "Process improvement confirmed", "Improvement not statistically confirmed")
```

**Business Application:** Validates process changes before full implementation, ensuring continuous improvement claims are substantiated by data. Supports quality certification requirements.

**Technical Details:** Note that if testing "defects decrease," a positive t-statistic (old > new) supports the hypothesis. Ensure your t-statistic sign aligns with your hypothesis direction.

---

### [[Financial Return Exceeds Benchmark]]

**Scenario:** A portfolio manager tests whether their active management strategy produces returns significantly greater than a passive benchmark index.

**Implementation:**
```
Excess Return: =Portfolio_Return - Benchmark_Return (for each period)
Mean Excess: =AVERAGE(Excess_Returns)
T-Statistic: =Mean_Excess / (STDEV.S(Excess_Returns) / SQRT(N_periods))
P-Value: =T.DIST.RT(T_Statistic, N_periods - 1)
Alpha: =IF(P_Value < 0.05, "Significant alpha generated", "No significant alpha")
```

**Business Application:** Justifies active management fees by demonstrating skill rather than luck. Regulatory and client reporting often requires statistical evidence of outperformance.

**Technical Details:** Financial returns often have non-normal distributions with fat tails. Consider bootstrap methods or larger sample sizes for robust inference.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Replaces:** Part of TDIST functionality (TDIST with tails=1)
- **Precision:** 15 significant digits
- **Behavior:** Accepts any real number for x; returns values 0-1

### Google Sheets
- **Availability:** All versions
- **Function:** Identical to Excel implementation
- **Precision:** 15 significant digits
- **Behavior:** Same handling of inputs and outputs

### Key Difference Alert
There are no functional differences between Excel and Google Sheets for T.DIST.RT. Both platforms compute identical right-tail probabilities for all valid inputs. The legacy TDIST function (with a tails parameter) is available in both but T.DIST.RT is clearer for new work.

## Tips and Best Practices

1. **Use one-tailed tests only with justification:** One-tailed tests are more powerful but only valid when you genuinely would not care about opposite-direction results. Journals often require pre-registration of directional hypotheses.

2. **Check your t-statistic sign:** A negative t-statistic with T.DIST.RT gives p > 0.5, which is never significant. If your hypothesis is "greater than" but your statistic is negative, the data contradicts your hypothesis.

3. **Relationship to two-tailed:** T.DIST.2T(|x|, df) = 2 * T.DIST.RT(|x|, df). You can verify calculations by checking this relationship.

4. **For "less than" hypotheses:** Use T.DIST(x, df, TRUE) or equivalently 1 - T.DIST.RT(x, df). The right-tail probability of a negative value equals the left-tail probability of its positive counterpart.

5. **Large samples approach z-distribution:** When df > 100, t and z distributions are nearly identical. T.DIST.RT(1.645, 100+) gives approximately 0.05.

6. **Report as one-tailed:** When presenting results, clearly state "one-tailed p-value = X" to distinguish from two-tailed tests. This is essential for reproducibility.

7. **Consider practical significance:** A one-tailed p-value of 0.04 might be significant, but is the effect size meaningful? Always pair with effect size measures.

8. **Avoid switching tails post-hoc:** Deciding to use one-tailed test after seeing results in a particular direction is p-hacking. Pre-specify your hypothesis direction.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[T.DIST.2T]] | Two-tailed t probability | For non-directional hypotheses (different, not greater/less) |
| [[T.DIST]] | Cumulative (left-tail) t distribution | For left-tailed tests (less than) |
| [[T.INV.RT]] | Inverse of T.DIST.RT | To find critical t-value for given alpha |
| [[T.TEST]] | Direct t-test on arrays | When comparing two samples without manual calculation |
| [[NORM.DIST]] | Normal distribution | For z-tests with known population variance |

### Commonly Used Together

**[[T.INV.RT]]** - Critical value for right-tail test

*Complete one-tailed test:*
```
Critical Value: =T.INV.RT(0.05, df)
P-Value: =T.DIST.RT(t_stat, df)
Decision: =IF(t_stat > T.INV.RT(0.05, df), "Reject H0", "Fail to reject")
```
Both approaches (critical value and p-value) give the same conclusion.

---

**[[AVERAGE]] and [[STDEV.S]]** - Descriptive statistics

*Compute t-statistic:*
```
T = (AVERAGE(Data) - Null_Value) / (STDEV.S(Data) / SQRT(COUNT(Data)))
P-Value = T.DIST.RT(T, COUNT(Data) - 1)
```
These functions provide the components for t-test calculation.

---

**[[T.DIST]]** - Left-tailed t distribution

*For lower-tail tests:*
```
Right-tail: =T.DIST.RT(x, df)
Left-tail: =T.DIST(x, df, TRUE)
Relationship: Left + Right = 1
```
Use T.DIST for "less than" hypotheses.

---

**[[T.DIST.2T]]** - Two-tailed t probability

*Convert between test types:*
```
One-tailed: =T.DIST.RT(x, df)
Two-tailed: =2 * T.DIST.RT(x, df)  (when x >= 0)
```
Understand the relationship for correct test selection.

---

**[[IF]]** - Conditional logic

*Automated test conclusions:*
```
=IF(AND(t_stat > 0, T.DIST.RT(t_stat, df) < 0.05),
    "Significantly greater",
    "Not significantly greater")
```
Combines direction check with significance test.

## Official Documentation

- **Microsoft Excel:** [T.DIST.RT function](https://support.microsoft.com/en-us/office/t-dist-rt-function-20a30020-86f9-4b35-af1f-7ef6ae683edd)
- **Google Sheets:** [T.DIST.RT function](https://support.google.com/docs/answer/9116319)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Part of statistical function naming standardization |
| Excel 2013+ | All subsequent versions | No changes to functionality |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Available since 2010 | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
