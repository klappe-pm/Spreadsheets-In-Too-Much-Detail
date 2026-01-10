---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- z-test
- hypothesis-testing
- normal-distribution
- one-sample-test
- standard-normal
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Z Test
- One-Sample Z Test
- Normal Test
- ZTEST
tags:
- statistical
- z-test
- hypothesis-testing
- normal-distribution
- mean-comparison
- p-value
---

# Z.TEST

## Description

**Z.TEST** returns the one-tailed probability (p-value) of a z-test, testing whether a sample mean is significantly greater than a hypothesized population mean. This function uses the standard normal (z) distribution rather than the t-distribution, which is appropriate when you have a large sample or know the population standard deviation. It answers: "What is the probability of observing a sample mean this high or higher if the true population mean equals the hypothesized value?"

The function calculates the z-statistic as (sample mean - hypothesized mean) / (standard deviation / sqrt(n)), then returns the upper-tail probability P(Z >= z). By default, it uses the sample standard deviation if you do not provide the population standard deviation, though technically the z-test assumes known population variance.

**Critical interpretation note:** Z.TEST returns the ONE-TAILED (right/upper) probability. If your sample mean is above the hypothesized value, this gives you the p-value for testing "mean > hypothesis." For a two-tailed test (testing "mean is different"), double the result. If your sample mean is below the hypothesis, Z.TEST returns a value > 0.5, and you should use 1 - Z.TEST for the left-tail p-value.

**Platform behavior:** Excel and Google Sheets implement Z.TEST identically. Both accept an array, a hypothesized mean, and an optional standard deviation. If sigma is omitted, the sample standard deviation is used (making it technically a large-sample approximation). Both ignore non-numeric values in the array.

## Syntax

> [!f(x)] Z.TEST Syntax
>
> ```
> =Z.TEST(array, x, [sigma])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The sample data (range or array of numeric values) to test. Non-numeric values are ignored. Must contain at least one numeric value. |
| `x` | Yes | The hypothesized population mean to test against. Also referred to as mu-zero (the null hypothesis value). |
| `sigma` | No | The known population standard deviation. If omitted, the sample standard deviation is calculated and used (appropriate for large samples). |

### Return Value

Returns a decimal value between 0 and 1 representing the one-tailed (upper) probability that the sample mean is greater than or equal to the observed value, given the hypothesized mean. Small values (< 0.05) indicate the sample mean is significantly HIGHER than hypothesized. Returns #N/A if array is empty. Returns #DIV/0! if sigma is zero or sample has no variance.

## Examples

> [!f(x)] Z.TEST Examples

### Example 1: Basic Z-Test (Sample Mean vs. Hypothesis)
```
=Z.TEST({3,6,7,8,6,5,4,2,1,9}, 4)
```
**Result:** 0.0907 (approximately)

**Explanation:** Tests whether the sample mean (5.1) is significantly greater than 4. The p-value of 0.09 is not significant at 5%, meaning we cannot conclude the population mean exceeds 4.

---

### Example 2: With Known Population Standard Deviation
```
=Z.TEST(A1:A100, 50, 10)
```
**Result:** One-tailed p-value

**Explanation:** Tests if the sample mean exceeds 50 when the population standard deviation is known to be 10. Using known sigma is the classic z-test assumption.

---

### Example 3: Sample Mean Below Hypothesis
```
=Z.TEST({1,2,3,4,5}, 10)
```
**Result:** 0.9999+ (very close to 1)

**Explanation:** The sample mean (3) is far below the hypothesis (10). Z.TEST returns nearly 1 because the probability of exceeding the observed mean is very high when it's below the hypothesis.

---

### Example 4: Converting to Two-Tailed Test
```
One-tailed: =Z.TEST(Data, 100, 15)
Two-tailed: =2*MIN(Z.TEST(Data, 100, 15), 1-Z.TEST(Data, 100, 15))
```
**Result:** Two-tailed p-value

**Explanation:** For a two-tailed test, double the smaller tail probability. MIN selects the appropriate tail based on whether the sample mean is above or below the hypothesis.

---

### Example 5: Converting to Left-Tailed Test
```
Right-tailed (default): =Z.TEST(Data, 100)
Left-tailed: =1 - Z.TEST(Data, 100)
```
**Result:** P-value for mean < hypothesis

**Explanation:** If testing whether the mean is LESS than hypothesized, subtract Z.TEST from 1. This gives the lower-tail probability.

---

### Example 6: Quality Control Application
```
=Z.TEST(ProductWeights, 500, 5)
```
Where ProductWeights contains measured weights, target is 500g, known sigma is 5g

**Result:** P-value for testing if mean weight > 500

**Explanation:** Tests whether products are significantly heavier than target. Small p-value indicates systematic overweight requiring process adjustment.

---

### Example 7: Large Sample Without Known Sigma
```
=Z.TEST(LargeSample, 75)
```
Where LargeSample has n > 100 observations

**Result:** Approximate z-test p-value

**Explanation:** With large samples (n > 30-100), the sample standard deviation approximates the population value well, and z-test approximates the t-test.

---

### Example 8: Using Range Reference
```
=Z.TEST(A1:A500, B1, C1)
```
Where A1:A500 = data, B1 = hypothesized mean, C1 = known sigma

**Result:** Dynamic z-test with changeable parameters

**Explanation:** Creates a flexible test where hypothesis and sigma can be changed without editing the formula.

---

### Example 9: Significance Test with Decision
```
=IF(Z.TEST(Measurements, Target, Sigma) < 0.05, "Mean significantly above target", "Mean not significantly above target")
```
**Result:** Automated conclusion

**Explanation:** Provides immediate interpretation of z-test result at 5% significance level for one-tailed upper test.

---

### Example 10: Complete Two-Tailed Analysis
```
Sample Mean: =AVERAGE(Data)
Z-Stat: =(AVERAGE(Data)-Hypothesis)/(Sigma/SQRT(COUNT(Data)))
One-Tail P: =Z.TEST(Data, Hypothesis, Sigma)
Two-Tail P: =2*MIN(Z.TEST(Data, Hypothesis, Sigma), 1-Z.TEST(Data, Hypothesis, Sigma))
Significant: =IF(Two_Tail_P < 0.05, "Yes", "No")
```
**Result:** Complete z-test summary

**Explanation:** Full analysis showing all components. Two-tailed p-value is appropriate when testing for any difference, not specifically higher or lower.

---

### Example 11: Standardized Test Score Analysis
```
=Z.TEST(ClassScores, 100, 15)
```
Where 100 is national mean, 15 is national standard deviation

**Result:** P-value for class exceeding national average

**Explanation:** Tests whether this class performs significantly above the national norm on a standardized test (e.g., IQ test with mean 100, SD 15).

---

### Example 12: Comparing Against Industry Standard
```
Industry Mean: 85%
Industry SD: 8%
Our Performance: {90, 88, 92, 87, 89, 91, 86, 93, 88, 90}
Test: =Z.TEST(Our_Performance, 85, 8)
```
**Result:** P-value for exceeding industry standard

**Explanation:** Tests whether our performance significantly exceeds the industry benchmark, using industry-known standard deviation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Array contains no numeric values | Verify data range contains numbers. Check for numbers stored as text. |
| `#DIV/0!` | Sigma is zero or sample has zero variance | Standard deviation must be positive. Check for identical values or data entry errors. |
| `#VALUE!` | Non-numeric hypothesis or sigma | Ensure x and sigma parameters are numeric values. |
| `Misinterpretation` | Forgetting this is one-tailed | Z.TEST is upper-tail. For two-tailed, double the result. For lower-tail, use 1-Z.TEST. |
| `P-value > 0.5` | Sample mean below hypothesis | This is correct behavior. High p-value for upper tail when mean is below hypothesis. |
| `Using with small samples` | T-test more appropriate | Z.TEST assumes known variance. With small samples and unknown sigma, use T.TEST instead. |

## Use Cases

### [[Standardized Test Performance Analysis]]

**Scenario:** A school administrator tests whether their district's average standardized test scores exceed the national norm, using the national mean and standard deviation as known parameters.

**Implementation:**
```
District Scores: Student scores from recent test
National Mean: 100 (for many standardized tests)
National SD: 15 (for many standardized tests)
Z-Test: =Z.TEST(DistrictScores, 100, 15)
Interpretation: =IF(Z.TEST(...)<0.05, "District significantly above national average", "No significant difference from national norm")
```

**Business Application:** Supports grant applications, demonstrates program effectiveness, and guides resource allocation by showing whether district performance meaningfully exceeds expectations.

**Technical Details:** The z-test is appropriate here because the population parameters (national mean and SD) are known from test norming studies. This is one of the few cases where z-test assumptions are truly met.

---

### [[Manufacturing Process Monitoring]]

**Scenario:** A quality engineer tests whether the mean fill volume of bottles exceeds the target of 500mL, using historical process data that established the standard deviation.

**Implementation:**
```
Sample Volumes: Measurements from production run
Target Volume: 500 mL
Process SD: 2.5 mL (from historical data)
Test: =Z.TEST(SampleVolumes, 500, 2.5)
Alert: =IF(Z.TEST(...)<0.05, "ALERT: Overfilling detected", "Process on target")
```

**Business Application:** Prevents product giveaway (overfilling) while ensuring compliance with minimum fill requirements. Enables real-time process adjustment based on statistical evidence.

**Technical Details:** In stable manufacturing processes, the standard deviation is often well-characterized from extensive historical data, making z-test appropriate despite not knowing the true population value.

---

### [[Customer Satisfaction Benchmarking]]

**Scenario:** A company tests whether their customer satisfaction scores exceed an industry benchmark, using publicly available industry statistics.

**Implementation:**
```
Company Scores: Customer satisfaction ratings
Industry Mean: 7.2 (from industry report)
Industry SD: 1.5 (from industry report)
Test: =Z.TEST(CompanyScores, 7.2, 1.5)
Two-Tailed: =2*MIN(Z.TEST(...), 1-Z.TEST(...))
Report: =IF(Z.TEST(...)<0.05, "Significantly above industry average", IF(1-Z.TEST(...)<0.05, "Significantly below industry average", "Consistent with industry average"))
```

**Business Application:** Provides competitive positioning evidence for marketing and investor communications. Identifies whether satisfaction investments produce measurably superior results.

**Technical Details:** Using industry SD assumes your company's variance is similar to industry average. If your company has different variability, results should be interpreted cautiously.

---

### [[Scientific Measurement Validation]]

**Scenario:** A laboratory validates that their measurement method produces results consistent with a known standard value, using established measurement precision.

**Implementation:**
```
Measurements: Repeated measurements of standard reference material
True Value: Certified value from standard
Measurement Precision: Known from validation studies
Accuracy Test: =2*MIN(Z.TEST(Measurements, TrueValue, Precision), 1-Z.TEST(Measurements, TrueValue, Precision))
Valid: =IF(Accuracy_Test > 0.05, "Method accurate - no significant bias", "WARNING: Significant bias detected")
```

**Business Application:** Validates analytical methods for regulatory compliance (FDA, EPA). Demonstrates measurement traceability and accuracy for accreditation requirements.

**Technical Details:** Two-tailed test is appropriate because bias in either direction is problematic. Method validation typically uses well-characterized precision from repeatability studies.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later (ZTEST available earlier)
- **Replaces:** Legacy ZTEST function (identical functionality)
- **Precision:** 15 significant digits
- **Behavior:** Returns one-tailed upper probability; ignores non-numeric values

### Google Sheets
- **Availability:** All versions
- **Function:** Identical to Excel implementation
- **Precision:** 15 significant digits
- **Behavior:** Same handling of parameters and missing sigma

### Key Difference Alert
There are no functional differences between Excel and Google Sheets for Z.TEST. Both platforms return identical one-tailed probabilities. The legacy ZTEST function is available in both for backward compatibility.

## Tips and Best Practices

1. **Remember it is ONE-TAILED:** Z.TEST returns upper-tail (right) probability. For two-tailed tests, double the result. For lower-tail, use 1-Z.TEST(array, x, sigma).

2. **Know when to use z vs. t:** Z.TEST is technically for known population variance. With unknown variance, T.TEST is more appropriate. However, for large samples (n > 100), they give nearly identical results.

3. **Provide sigma when truly known:** If you have historical data establishing the population standard deviation (e.g., from stable manufacturing processes), provide it. Otherwise, the sample SD is used.

4. **Interpret high p-values correctly:** If Z.TEST returns > 0.5, your sample mean is below the hypothesis. This is not "highly significant" - it means no evidence for mean being higher.

5. **Two-tailed formula:** =2*MIN(Z.TEST(data, x, sigma), 1-Z.TEST(data, x, sigma)). This correctly handles both directions.

6. **Consider practical significance:** A statistically significant difference from a target may not be practically important. Combine z-test with effect size or confidence interval.

7. **Check sample size:** Z.TEST assumes large samples when sigma is omitted. With n < 30 and unknown sigma, prefer T.TEST or manual t-test calculation.

8. **Validate assumptions:** Z-test assumes approximately normal data or large sample (Central Limit Theorem). With small samples from non-normal distributions, consider non-parametric alternatives.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[T.TEST]] | T-test for comparing means | When population variance is unknown (most cases) |
| [[T.DIST.RT]] | Right-tailed t probability | For manual t-test with small samples |
| [[NORM.S.DIST]] | Standard normal cumulative | When you have already calculated z-statistic |
| [[CONFIDENCE.NORM]] | Z-based confidence interval | When constructing confidence intervals with known sigma |
| [[ZTEST]] | Legacy z-test function | Only for backward compatibility |

### Commonly Used Together

**[[AVERAGE]]** - Calculate sample mean

*Understand test components:*
```
Sample Mean: =AVERAGE(Data)
Hypothesized Mean: 100
Difference: =AVERAGE(Data) - 100
P-Value: =Z.TEST(Data, 100, Sigma)
```
The mean shows direction; Z.TEST provides statistical significance.

---

**[[STDEV.S]] or [[STDEV.P]]** - Standard deviation

*Calculate z-statistic manually:*
```
Z-Stat: =(AVERAGE(Data) - Hypothesis) / (STDEV.S(Data) / SQRT(COUNT(Data)))
Compare to: =Z.TEST(Data, Hypothesis)
```
Understanding the z-statistic helps interpret the p-value.

---

**[[COUNT]]** - Sample size

*Report complete test statistics:*
```
n: =COUNT(Data)
Mean: =AVERAGE(Data)
SE: =Sigma / SQRT(COUNT(Data))
Z-Test: =Z.TEST(Data, Hypothesis, Sigma)
```
Sample size affects statistical power and standard error.

---

**[[MIN]]** - Minimum value

*Two-tailed p-value calculation:*
```
=2 * MIN(Z.TEST(Data, x, sigma), 1 - Z.TEST(Data, x, sigma))
```
MIN selects the correct tail for the two-tailed calculation.

---

**[[IF]]** - Conditional logic

*Automated interpretation:*
```
=IF(Z.TEST(Data, Target, Sigma) < 0.05,
    "Sample mean significantly exceeds target",
    IF(1-Z.TEST(Data, Target, Sigma) < 0.05,
        "Sample mean significantly below target",
        "No significant difference from target"))
```
Provides complete three-way interpretation.

## Official Documentation

- **Microsoft Excel:** [Z.TEST function](https://support.microsoft.com/en-us/office/z-test-function-d633d5a3-2031-4614-a016-92180ad82bee)
- **Google Sheets:** [Z.TEST function](https://support.google.com/docs/answer/9367190)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Replaced ZTEST with identical functionality |
| Excel 2013+ | All subsequent versions | No changes to functionality |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Original launch | Named ZTEST initially; Z.TEST added for compatibility |

---

*Last updated: 2026-01-10*
