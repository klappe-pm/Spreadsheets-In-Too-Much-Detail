---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- f-test
- variance-comparison
- hypothesis-testing
- two-sample-test
- homogeneity-of-variance
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- F Test
- Variance Test
- Two-Sample F-Test
- FTEST
- Homogeneity Test
tags:
- statistical
- f-test
- variance
- hypothesis-testing
- two-sample
- comparison
---

# F.TEST

## Description

**F.TEST** returns the two-tailed probability (p-value) from an F-test comparing the variances of two datasets. It directly answers the question: "What is the probability of observing variance ratios this extreme or more extreme if the two populations have equal variances?" This is the definitive function for testing homogeneity of variance (homoscedasticity), a key assumption in many statistical procedures including t-tests and ANOVA.

The function calculates the F-statistic automatically by dividing the variance of the first array by the variance of the second array, then returns the two-tailed probability. Unlike F.DIST.RT which requires you to calculate the F-statistic yourself, F.TEST handles everything in one step. The two-tailed nature means it tests whether variances are different in either direction (one larger or smaller than the other), not specifically which one is larger.

**Critical behavior note:** F.TEST returns a two-tailed p-value regardless of which array has larger variance. It automatically handles the calculation correctly by using the larger variance in the numerator internally. If you need a one-tailed test (specifically testing if one variance is larger), divide the F.TEST result by 2 or use F.DIST.RT with your own F-statistic calculation.

**Platform behavior:** Excel and Google Sheets implement F.TEST identically. Both accept arrays or ranges of equal or unequal length, both return two-tailed p-values, and both handle the F-statistic calculation internally. The function replaced the legacy FTEST function in Excel 2010 but provides identical results. Both platforms require at least 2 data points in each array and ignore non-numeric values within the arrays.

## Syntax

> [!f(x)] F.TEST Syntax
>
> ```
> =F.TEST(array1, array2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first dataset, range, or array of numeric values. Must contain at least 2 numeric values. Non-numeric values (text, blanks, logical values) are ignored. |
| `array2` | Yes | The second dataset, range, or array of numeric values. Must contain at least 2 numeric values. Arrays can be different sizes. Non-numeric values are ignored. |

### Return Value

Returns a decimal value between 0 and 1 representing the two-tailed probability that the variances of the two populations are not significantly different. Small values (typically < 0.05) indicate significant variance differences. Returns #DIV/0! if either array has fewer than 2 numeric values or zero variance. Returns #VALUE! if arrays contain no numeric data.

## Examples

> [!f(x)] F.TEST Examples

### Example 1: Basic Variance Comparison
```
=F.TEST({1,2,3,4,5}, {2,4,6,8,10})
```
**Result:** 0.0252 (approximately)

**Explanation:** The second array has larger variance (spread). The low p-value (0.0252 < 0.05) indicates the variances are significantly different at the 5% level.

---

### Example 2: Similar Variances (High P-Value)
```
=F.TEST({10,12,11,13,12}, {20,22,21,23,22})
```
**Result:** 1.000

**Explanation:** Both arrays have the same spread (just shifted by 10). The p-value of 1.0 indicates no evidence of different variances - the variance ratio is exactly 1.

---

### Example 3: Range References
```
=F.TEST(A1:A20, B1:B20)
```
**Result:** P-value for variance comparison

**Explanation:** Compares variances of data in column A (rows 1-20) vs column B. This is the typical usage for comparing two groups of measurements.

---

### Example 4: Different Sample Sizes
```
=F.TEST(A1:A30, B1:B15)
```
**Result:** P-value accounting for different sample sizes

**Explanation:** F.TEST correctly handles arrays of different lengths. Degrees of freedom are calculated as (n1-1, n2-1) automatically. Unequal sample sizes are common and acceptable.

---

### Example 5: Manufacturing Quality Comparison
```
=F.TEST(MachineA_Data, MachineB_Data)
```
Where MachineA_Data and MachineB_Data are named ranges

**Result:** P-value for variance homogeneity test

**Explanation:** Tests whether two machines produce parts with equal variability. A significant result (p < 0.05) suggests one machine is more consistent than the other.

---

### Example 6: Pre-Requisite for T-Test
```
=IF(F.TEST(Group1, Group2) > 0.05, "Equal variances - use pooled t-test", "Unequal variances - use Welch's t-test")
```
**Result:** Recommendation for t-test type

**Explanation:** Before comparing means with a t-test, check variance equality with F.TEST. This determines whether to use the pooled variance t-test (equal variances) or Welch's t-test (unequal variances).

---

### Example 7: Nearly Significant Result
```
=F.TEST({1,3,5,7,9,11}, {2,3,4,5,6,7})
```
**Result:** 0.0665 (approximately)

**Explanation:** P-value of 0.0665 is not significant at 5% but would be at 10%. The first array has more spread. Report exact p-values to let readers apply their own thresholds.

---

### Example 8: Arrays with Missing Values (Blanks)
```
=F.TEST(A1:A10, B1:B10)
```
Where A1:A10 contains: 5, 10, [blank], 15, 20, 25, [blank], 30, 35, 40
Where B1:B10 contains: 8, 12, 16, 20, [blank], 24, 28, 32, 36, 40

**Result:** P-value based on 8 values from A and 9 values from B

**Explanation:** Blank cells are automatically excluded. F.TEST uses only the numeric values, adjusting degrees of freedom accordingly.

---

### Example 9: Extremely Different Variances
```
=F.TEST({100,100,100,100,100}, {10,50,90,130,170})
```
**Result:** Very small number (near 0)

**Explanation:** First array has zero variance (all identical), but F.TEST may return #DIV/0! for exact zero variance. With minimal variance vs. large variance, p-values become extremely small.

---

### Example 10: Converting to One-Tailed Test
```
One-tailed p-value: =F.TEST(LargerVarianceData, SmallerVarianceData)/2
```
**Result:** Half of the two-tailed p-value

**Explanation:** If you have a directional hypothesis (e.g., "Treatment increases variability"), divide F.TEST by 2 for the one-tailed p-value. Ensure you put the hypothesized larger variance first.

---

### Example 11: Multiple Group Comparisons
```
Group1 vs Group2: =F.TEST(A:A, B:B)
Group1 vs Group3: =F.TEST(A:A, C:C)
Group2 vs Group3: =F.TEST(B:B, C:C)
```
**Result:** Pairwise variance comparisons

**Explanation:** When comparing more than two groups, perform pairwise F-tests. Consider Bonferroni correction for multiple comparisons (use alpha/number of tests).

---

### Example 12: Conditional Formatting Trigger
```
=F.TEST(Baseline, Treatment) < 0.05
```
**Result:** TRUE or FALSE

**Explanation:** Use in conditional formatting to highlight significant variance differences. Cells turn red (or other format) when treatment variance differs significantly from baseline.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | One or both arrays have fewer than 2 numeric values | Ensure each array contains at least 2 numbers. Check for text-formatted numbers or all-blank ranges. |
| `#DIV/0!` | One array has zero variance (all identical values) | Cannot compare when one group has no variability. This may indicate data entry errors or insufficient variation. |
| `#VALUE!` | Arrays contain no interpretable numeric data | Verify ranges contain numbers. Check for numbers stored as text. |
| `P-value always 1` | Arrays have identical variance | When variance ratio is exactly 1, p-value is 1. This is correct behavior, not an error. |
| `Unexpected high p-value` | Small sample sizes | F-tests have low power with small samples. Even different variances may not be detected. Increase sample size. |
| `Interpretation error` | Forgetting this is two-tailed | F.TEST gives two-tailed p-value. For one-tailed hypothesis, divide by 2. |

## Use Cases

### [[Pre-Analysis Variance Check for T-Tests]]

**Scenario:** A researcher needs to compare mean response times between two software interfaces but first must verify the homogeneity of variance assumption before selecting the appropriate t-test.

**Implementation:**
```
Variance Test: =F.TEST(InterfaceA_Times, InterfaceB_Times)
T-Test Type: =IF(F.TEST(InterfaceA_Times, InterfaceB_Times)>0.05, "Pooled", "Welch")
Mean Comparison: =T.TEST(InterfaceA_Times, InterfaceB_Times, 2, IF(F.TEST(...)>0.05, 2, 3))
```

**Business Application:** Ensures the correct statistical test is used, preventing invalid conclusions. Using the wrong t-test type when variances differ can inflate Type I error rates.

**Technical Details:** Many statisticians now recommend always using Welch's t-test regardless of F.TEST results, as Welch's test is robust to unequal variances with minimal power loss when variances are equal.

---

### [[Quality Control Process Comparison]]

**Scenario:** A manufacturing plant needs to determine if two production shifts produce parts with equal consistency before combining their data for control chart calculations.

**Implementation:**
```
Variance Test: =F.TEST(DayShift, NightShift)
Decision: =IF(F.TEST(DayShift, NightShift)>0.05,
    "Combine data for single control chart",
    "Use separate control charts for each shift")
Control Limit Base: =IF(F.TEST(...)>0.05, STDEV.S(AllData), "See separate charts")
```

**Business Application:** Determines appropriate quality monitoring strategy. Combining data with different variances leads to inappropriate control limits and missed defects or false alarms.

**Technical Details:** Even if p > 0.05, consider practical significance. If one shift's standard deviation is 50% larger, separate monitoring may be warranted regardless of statistical significance.

---

### [[Investment Portfolio Risk Comparison]]

**Scenario:** A financial analyst needs to compare the volatility (variance of returns) between two investment strategies to advise clients on risk profiles.

**Implementation:**
```
Returns_StrategyA: Monthly percentage returns for Strategy A
Returns_StrategyB: Monthly percentage returns for Strategy B
Volatility Test: =F.TEST(Returns_StrategyA, Returns_StrategyB)
Risk Assessment: =IF(F.TEST(...)<0.05,
    "Strategies have significantly different risk levels",
    "Similar risk profiles")
```

**Business Application:** Helps investors understand whether two portfolios have meaningfully different risk characteristics. Significant differences warrant different investor suitability classifications.

**Technical Details:** Financial returns often have fat tails (non-normal distributions). The F-test assumes normality; for financial data, consider supplementing with non-parametric tests like Levene's test.

---

### [[Clinical Trial Group Comparability]]

**Scenario:** Before analyzing treatment effects in a clinical trial, statisticians must verify that the treatment and control groups have comparable variability in baseline measurements.

**Implementation:**
```
Baseline Variability: =F.TEST(Treatment_Baseline, Control_Baseline)
Groups Comparable: =IF(F.TEST(...) > 0.05, "Yes", "No - investigate randomization")
Report: ="Baseline variance comparison: F-test p = " & TEXT(F.TEST(...), "0.0000")
```

**Business Application:** Validates randomization success and supports regulatory submission by demonstrating group comparability. Significant baseline variance differences may indicate randomization failures.

**Technical Details:** Document F.TEST results in statistical analysis plans. If baseline variances differ significantly, analysis may need to account for this through variance-weighted methods.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Replaces:** Legacy FTEST function (identical functionality)
- **Precision:** 15 significant digits
- **Behavior:** Ignores text, logical values, and empty cells within arrays

### Google Sheets
- **Availability:** All versions
- **Function:** Identical to Excel implementation
- **Precision:** 15 significant digits
- **Behavior:** Same handling of non-numeric values as Excel

### Key Difference Alert
There are no functional differences between Excel and Google Sheets for F.TEST. Both platforms calculate identical two-tailed p-values for variance comparison. The legacy FTEST function is available in both for backward compatibility but F.TEST is preferred for consistency with other modern function naming.

## Tips and Best Practices

1. **Remember F.TEST is two-tailed:** The returned p-value tests whether variances differ in either direction. For one-tailed tests (one variance specifically larger), divide by 2.

2. **Use F.TEST before T.TEST:** Check variance equality before choosing between pooled t-test (equal variances) and Welch's t-test (unequal variances). This ensures valid mean comparisons.

3. **Consider practical vs. statistical significance:** Even with p > 0.05, if one variance is much larger (e.g., 2x), consider treating them as different for practical purposes, especially in quality control.

4. **Check the normality assumption:** F.TEST assumes normally distributed data. With non-normal data, especially with small samples, consider Levene's test or Brown-Forsythe test instead.

5. **Report exact p-values:** Rather than just "p < 0.05" or "not significant," report the actual F.TEST result. This allows readers to apply their own significance thresholds.

6. **Handle multiple comparisons:** When comparing variances across many groups pairwise, apply Bonferroni or similar corrections to maintain family-wise error rate.

7. **Sample size matters for power:** F.TEST has low power with small samples. Non-significant results with n < 20 per group should be interpreted cautiously.

8. **Verify data quality:** Before interpreting F.TEST results, check that both datasets are complete and free of outliers. A single extreme value can dramatically affect variance.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[F.DIST.RT]] | Right-tailed F probability from F-statistic | When you need one-tailed test or want to control F-statistic calculation |
| [[F.INV.RT]] | Critical F-value for given alpha | When you prefer critical value approach over p-value |
| [[VAR.S]] | Sample variance | To examine raw variances before formal testing |
| [[FTEST]] | Legacy two-tailed F-test | Only for backward compatibility |

### Commonly Used Together

**[[T.TEST]]** - Two-sample t-test

*Complete mean comparison workflow:*
```
Step 1 - Check variances: =F.TEST(Group1, Group2)
Step 2 - Select t-test type: =IF(F.TEST(Group1, Group2)>0.05, 2, 3)
Step 3 - Run t-test: =T.TEST(Group1, Group2, 2, type_from_step2)
```
F.TEST determines which t-test is appropriate before comparing means.

---

**[[VAR.S]]** - Sample variance

*Understand the raw values behind F.TEST:*
```
Variance 1: =VAR.S(Group1)
Variance 2: =VAR.S(Group2)
Variance Ratio: =MAX(VAR.S(Group1), VAR.S(Group2))/MIN(VAR.S(Group1), VAR.S(Group2))
P-Value: =F.TEST(Group1, Group2)
```
Seeing the actual variances helps interpret statistical significance.

---

**[[STDEV.S]]** - Sample standard deviation

*Report in original units:*
```
StdDev 1: =STDEV.S(Group1)
StdDev 2: =STDEV.S(Group2)
Ratio: =MAX(STDEV.S(Group1), STDEV.S(Group2))/MIN(STDEV.S(Group1), STDEV.S(Group2))
```
Standard deviations are often more interpretable than variances.

---

**[[IF]]** - Conditional logic

*Automate analysis decisions:*
```
=IF(F.TEST(A:A, B:B)<0.05, "Use Welch's t-test", "Use pooled t-test")
```
Creates automatic workflow recommendations based on variance test results.

---

**[[COUNT]]** - Count numeric values

*Verify sample sizes:*
```
n1: =COUNT(Group1)
n2: =COUNT(Group2)
df1: =COUNT(Group1)-1
df2: =COUNT(Group2)-1
```
Understanding degrees of freedom helps interpret F.TEST results.

## Official Documentation

- **Microsoft Excel:** [F.TEST function](https://support.microsoft.com/en-us/office/f-test-function-100a59e7-4108-46f8-8443-78ffacb6c0a7)
- **Google Sheets:** [F.TEST function](https://support.google.com/docs/answer/9116267)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Replaced FTEST with consistent naming convention |
| Excel 2013+ | All subsequent versions | No changes to functionality |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Available since 2010 | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
