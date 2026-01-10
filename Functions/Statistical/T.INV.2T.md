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
- inverse-distribution
- critical-values
- two-tailed-test
- confidence-intervals
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- T Inverse Two-Tailed
- T Critical Value
- Two-Tailed T Critical
- Student T Inverse
tags:
- statistical
- t-distribution
- inverse
- critical-value
- hypothesis-testing
- confidence-interval
---

# T.INV.2T

## Description

**T.INV.2T** returns the inverse of the two-tailed Student's t-distribution, giving you the critical t-value for a specified probability and degrees of freedom. This is the essential function for finding t critical values used in confidence interval calculations and two-tailed hypothesis tests. When you need "the t-value that cuts off 5% in both tails combined," T.INV.2T provides that answer.

The function returns a positive value representing the critical threshold. Due to the t-distribution's symmetry around zero, if T.INV.2T(0.05, 20) returns 2.086, then values less than -2.086 or greater than +2.086 would together comprise 5% of the distribution. This makes it perfect for two-tailed significance testing and for calculating margin of error in confidence intervals.

**Relationship to other functions:** T.INV.2T is the mathematical inverse of T.DIST.2T, meaning T.DIST.2T(T.INV.2T(p, df), df) = p. Also, T.INV.2T(alpha, df) returns a value whose right-tail probability is alpha/2, so T.INV.2T(0.05, df) = T.INV.RT(0.025, df). This relationship is crucial for understanding the connection between confidence levels and significance testing.

**Platform behavior:** Excel and Google Sheets implement T.INV.2T identically. Both require probability strictly between 0 and 1, both require degrees of freedom >= 1, and both return positive values. The function was introduced in Excel 2010 as part of the effort to provide clearer statistical function names.

## Syntax

> [!f(x)] T.INV.2T Syntax
>
> ```
> =T.INV.2T(probability, deg_freedom)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `probability` | Yes | The two-tailed probability (alpha level) for which to find the critical t-value. Must be strictly between 0 and 1. Common values: 0.05 (95% confidence), 0.01 (99% confidence), 0.10 (90% confidence). |
| `deg_freedom` | Yes | The degrees of freedom. Must be a positive integer (decimals are truncated). For one-sample t-test: n - 1. For confidence intervals: n - 1 where n is sample size. |

### Return Value

Returns a positive numeric value representing the critical t-value. Values of |t| greater than this threshold have two-tailed probability less than the specified alpha. Returns #NUM! if probability is outside (0, 1) or degrees of freedom < 1. Returns #VALUE! if arguments are non-numeric.

## Examples

> [!f(x)] T.INV.2T Examples

### Example 1: Standard 95% Confidence Level
```
=T.INV.2T(0.05, 20)
```
**Result:** 2.086 (approximately)

**Explanation:** For df = 20, the critical t-value for a 95% confidence interval (alpha = 0.05) is 2.086. Your t-statistic must exceed this in absolute value for two-tailed significance.

---

### Example 2: 99% Confidence Level
```
=T.INV.2T(0.01, 20)
```
**Result:** 2.845 (approximately)

**Explanation:** More stringent confidence (99%) requires a larger critical value. The t-statistic must exceed 2.845 (vs 2.086 at 95%) for significance at the 1% level.

---

### Example 3: 90% Confidence Level
```
=T.INV.2T(0.10, 20)
```
**Result:** 1.725 (approximately)

**Explanation:** Less stringent confidence (90%) has a smaller critical value. The 90% confidence interval is narrower than 95%, but you have more risk of not capturing the true parameter.

---

### Example 4: Small Sample (Large Critical Value)
```
=T.INV.2T(0.05, 5)
```
**Result:** 2.571 (approximately)

**Explanation:** With only 6 data points (df = 5), the critical t-value is 2.571, much larger than 1.96 (normal distribution). Small samples require more extreme values for significance.

---

### Example 5: Large Sample (Approaching Normal)
```
=T.INV.2T(0.05, 1000)
```
**Result:** 1.962 (approximately)

**Explanation:** With large df, the t-distribution approaches normal. The critical value of 1.962 is very close to the famous z = 1.96 for normal distributions.

---

### Example 6: Confidence Interval Calculation
```
=AVERAGE(A1:A30) - T.INV.2T(0.05, 29) * STDEV.S(A1:A30)/SQRT(30)
=AVERAGE(A1:A30) + T.INV.2T(0.05, 29) * STDEV.S(A1:A30)/SQRT(30)
```
**Result:** Lower and upper bounds of 95% confidence interval

**Explanation:** The margin of error is t-critical times the standard error. Adding and subtracting from the mean gives the confidence interval bounds.

---

### Example 7: Verifying Inverse Relationship
```
=T.DIST.2T(T.INV.2T(0.05, 25), 25)
```
**Result:** 0.05

**Explanation:** Confirms T.INV.2T and T.DIST.2T are mathematical inverses. The two-tailed probability of the critical value equals the input alpha.

---

### Example 8: Comparing Different df Values
```
df=10: =T.INV.2T(0.05, 10)
df=30: =T.INV.2T(0.05, 30)
df=100: =T.INV.2T(0.05, 100)
```
**Result:** 2.228, 2.042, 1.984 (approximately)

**Explanation:** As degrees of freedom increase, critical values decrease toward the normal distribution limit (1.96). More data means less uncertainty about the population variance.

---

### Example 9: Relationship to One-Tailed Critical Value
```
Two-tailed: =T.INV.2T(0.05, 20)
One-tailed: =T.INV.RT(0.025, 20)
```
**Result:** Both return 2.086 (approximately)

**Explanation:** The two-tailed 5% critical value equals the one-tailed 2.5% critical value. This reflects that 2.5% is in each tail for the two-tailed test.

---

### Example 10: Dynamic Critical Value Lookup
```
=T.INV.2T(B2, C2)
```
Where B2 = significance level, C2 = sample size - 1

**Result:** Critical t-value adjusts with inputs

**Explanation:** Creates a flexible calculator where changing confidence level or sample size automatically updates the critical value.

---

### Example 11: Sample Size Planning
```
For achieving margin of error E with std dev s:
Required n approximately: =(T.INV.2T(0.05, 30) * s / E)^2
```
**Result:** Approximate required sample size

**Explanation:** Rearranging the margin of error formula helps determine sample size needed for desired precision. Note: requires iteration since df depends on n.

---

### Example 12: Building Critical Value Table
```
For each alpha (0.10, 0.05, 0.01) and df (5, 10, 20, 30, 60, 120):
=T.INV.2T(alpha, df)
```
**Result:** Custom t-table

**Explanation:** Generate your own reference table for situations where electronic calculation is not available or for educational purposes.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Probability is 0, negative, or >= 1 | Probability must be strictly between 0 and 1. Use 0.05, not 0.95 for 95% confidence. |
| `#NUM!` | Degrees of freedom less than 1 | df must be at least 1 (sample size at least 2). Check your sample size. |
| `#VALUE!` | Non-numeric arguments | Ensure both parameters are numbers or cell references to numbers. |
| `Wrong interpretation` | Confusing alpha with confidence level | For 95% confidence, use alpha = 0.05, not 0.95. Alpha = 1 - confidence level. |
| `Critical value too large` | Very small degrees of freedom | This is correct behavior for small samples. Consider increasing sample size. |
| `Unexpected relationship to T.INV` | Functions have different interpretations | T.INV.2T gives the positive critical value for two-tailed tests; T.INV gives left-tail quantiles. |

## Use Cases

### [[Confidence Interval Construction]]

**Scenario:** A market research team needs to construct a 95% confidence interval for the mean customer satisfaction score from a sample of 50 respondents.

**Implementation:**
```
Sample Mean: =AVERAGE(Scores)
Sample StdDev: =STDEV.S(Scores)
Sample Size: =COUNT(Scores)
T Critical: =T.INV.2T(0.05, COUNT(Scores)-1)
Margin of Error: =T.INV.2T(0.05, COUNT(Scores)-1) * STDEV.S(Scores) / SQRT(COUNT(Scores))
Lower Bound: =AVERAGE(Scores) - Margin_of_Error
Upper Bound: =AVERAGE(Scores) + Margin_of_Error
Interpretation: ="95% CI: [" & ROUND(Lower, 2) & ", " & ROUND(Upper, 2) & "]"
```

**Business Application:** Provides a range within which the true population satisfaction score likely falls, enabling informed decisions about whether current satisfaction levels meet targets.

**Technical Details:** The confidence interval width depends on three factors: confidence level (t critical), sample variability (std dev), and sample size (sqrt n). Increasing n narrows the interval.

---

### [[Two-Tailed Hypothesis Testing]]

**Scenario:** A quality control engineer tests whether the mean product weight differs from the target specification of 500 grams, requiring evidence of any deviation (higher or lower).

**Implementation:**
```
Hypothesized Mean: 500
T-Statistic: =(AVERAGE(Weights) - 500) / (STDEV.S(Weights) / SQRT(COUNT(Weights)))
Critical Value: =T.INV.2T(0.05, COUNT(Weights)-1)
Significant: =IF(ABS(T_Statistic) > T_Critical, "Reject H0: Mean differs from 500", "Fail to reject: Mean may equal 500")
```

**Business Application:** Determines whether the production process requires adjustment for being off-target in either direction. Two-tailed test is appropriate when both overweight and underweight products are problematic.

**Technical Details:** Compare the absolute value of your t-statistic to T.INV.2T. If |t| > critical value, reject the null hypothesis that the mean equals the target.

---

### [[Statistical Power and Sample Size Planning]]

**Scenario:** A researcher plans an experiment and needs to determine the sample size required to detect a meaningful difference with 80% power at the 5% significance level.

**Implementation:**
```
Alpha: 0.05
Estimated effect size: d
Critical t for alpha: =T.INV.2T(0.05, n-1)  [iterate]
Required t for power: Using noncentral t distribution
Minimum n: Solve iteratively
```

**Business Application:** Ensures research investment is appropriately sized - not too small (underpowered, wasting resources on inconclusive studies) nor too large (over-investing when smaller samples would suffice).

**Technical Details:** Power analysis is iterative because the critical value depends on df which depends on n. Start with an estimate and refine, or use specialized power analysis software.

---

### [[Creating Statistical Reference Tables]]

**Scenario:** A statistics instructor needs to create custom t-distribution critical value tables for exams where electronic devices are not permitted.

**Implementation:**
```
For rows of df (1, 2, 5, 10, 15, 20, 25, 30, 40, 60, 120):
For columns of alpha (0.10, 0.05, 0.02, 0.01):
=T.INV.2T(alpha, df)
```

**Business Application:** Creates educational materials that help students understand the relationship between confidence level, sample size, and critical values without relying on standard tables.

**Technical Details:** Include a row for "infinity" df showing the normal distribution z-values (1.645, 1.960, 2.326, 2.576) for comparison.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Replaces:** Part of TINV functionality (TINV was one-tailed)
- **Precision:** 15 significant digits
- **Behavior:** Returns positive values only; probability must be in (0, 1)

### Google Sheets
- **Availability:** All versions
- **Function:** Identical to Excel implementation
- **Precision:** 15 significant digits
- **Behavior:** Same input requirements and outputs

### Key Difference Alert
There are no functional differences between Excel and Google Sheets for T.INV.2T. Both return identical critical values for all valid inputs. The legacy TINV function (which was two-tailed, confusingly) is available in both platforms but T.INV.2T is clearer.

## Tips and Best Practices

1. **Remember alpha, not confidence:** T.INV.2T takes alpha (significance level), not confidence level. For 95% confidence, use 0.05. For 99% confidence, use 0.01.

2. **Use for symmetric intervals:** T.INV.2T returns a positive value. For confidence intervals, both add and subtract this value times the standard error from the mean.

3. **Check the inverse relationship:** Verify your critical value is correct by checking T.DIST.2T(critical, df) returns your original alpha.

4. **Know the relationship to T.INV.RT:** T.INV.2T(alpha, df) = T.INV.RT(alpha/2, df). The two-tailed 5% critical value equals the one-tailed 2.5% critical value.

5. **Consider sample size impact:** Small samples (df < 30) have critical values notably larger than 1.96. With df = 5, the 95% critical value is 2.57 - a substantial difference.

6. **Report degrees of freedom:** Always report df alongside critical values for reproducibility. "t(df=29) critical = 2.045 at alpha = 0.05"

7. **Use in formula for confidence intervals:** CI = mean +/- T.INV.2T(alpha, n-1) * s / sqrt(n). This is the standard formula worth memorizing.

8. **Understand the symmetry:** Because the t-distribution is symmetric, +T.INV.2T and -T.INV.2T define the rejection regions for two-tailed tests.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[T.INV]] | Left-tailed inverse t distribution | When you need a specific quantile (percentile) of the t-distribution |
| [[T.INV.RT]] | Right-tailed inverse t distribution | For one-tailed critical values (upper tail) |
| [[T.DIST.2T]] | Two-tailed t probability | When you have a t-value and need the p-value |
| [[CONFIDENCE.T]] | Direct confidence interval calculation | When you want the margin of error directly |
| [[TINV]] | Legacy inverse t function | Only for backward compatibility |

### Commonly Used Together

**[[T.DIST.2T]]** - Two-tailed t probability

*Complete hypothesis test:*
```
Critical Value: =T.INV.2T(0.05, df)
P-Value: =T.DIST.2T(ABS(t_stat), df)
Both methods: Critical value method (compare to t_stat) and p-value method (compare to alpha) give same conclusion
```
These functions are mathematical inverses.

---

**[[STDEV.S]]** and **[[AVERAGE]]** - Descriptive statistics

*Confidence interval formula:*
```
Lower: =AVERAGE(Data) - T.INV.2T(0.05, COUNT(Data)-1) * STDEV.S(Data)/SQRT(COUNT(Data))
Upper: =AVERAGE(Data) + T.INV.2T(0.05, COUNT(Data)-1) * STDEV.S(Data)/SQRT(COUNT(Data))
```
Together these create the complete confidence interval.

---

**[[CONFIDENCE.T]]** - T-based confidence interval

*Compare approaches:*
```
Margin using T.INV.2T: =T.INV.2T(0.05, n-1) * s / SQRT(n)
Margin using CONFIDENCE.T: =CONFIDENCE.T(0.05, s, n)
```
CONFIDENCE.T is more direct; T.INV.2T offers more understanding.

---

**[[COUNT]]** - Count numeric values

*Automatic degrees of freedom:*
```
=T.INV.2T(0.05, COUNT(Data)-1)
```
COUNT provides sample size; subtract 1 for degrees of freedom.

---

**[[IF]]** - Conditional logic

*Automated test conclusion:*
```
=IF(ABS(t_stat) > T.INV.2T(0.05, df), "Significant", "Not significant")
```
Creates immediate interpretation of test results.

## Official Documentation

- **Microsoft Excel:** [T.INV.2T function](https://support.microsoft.com/en-us/office/t-inv-2t-function-ce72ea19-ec6c-4be7-bed2-b9baf2264f17)
- **Google Sheets:** [T.INV.2T function](https://support.google.com/docs/answer/9116176)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Introduced for clearer two-tailed t critical value calculation |
| Excel 2013+ | All subsequent versions | No changes to functionality |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Available since 2010 | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
