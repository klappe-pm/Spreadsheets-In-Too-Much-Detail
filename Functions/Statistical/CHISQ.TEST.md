---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- chi-square-test
- hypothesis-testing
- independence-test
- goodness-of-fit
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Chi-Square Test
- Chi-Square Independence Test
- CHITEST
- Pearson Chi-Square
tags:
- statistical
- chi-square
- hypothesis-testing
- independence
- categorical-data
---

# CHISQ.TEST

## Description

**CHISQ.TEST** performs a chi-square test of independence by comparing observed frequencies to expected frequencies, returning the p-value directly. This function streamlines hypothesis testing by handling both the chi-square statistic calculation and the probability lookup in one step. You simply provide two ranges of the same dimensions - one containing observed frequencies and one containing expected frequencies - and CHISQ.TEST returns the probability that differences this large or larger could occur by chance alone.

The function calculates the chi-square statistic internally using the formula: chi-square = SUM((Observed - Expected)^2 / Expected), summing across all cells. It then returns the right-tail probability from the chi-square distribution with degrees of freedom equal to (rows-1) * (columns-1) for contingency tables. A small p-value (typically < 0.05) indicates that the observed frequencies differ significantly from expected, suggesting the null hypothesis of independence or good fit should be rejected.

**Proper expected frequency calculation is critical.** For tests of independence in contingency tables, expected frequency for each cell = (row total * column total) / grand total. For goodness-of-fit tests, expected frequencies come from your hypothesized distribution (e.g., equal frequencies for uniform distribution, or theoretically derived proportions). CHISQ.TEST assumes you've calculated expected values correctly - it does not calculate them for you.

**Assumptions and limitations:** The chi-square approximation requires sufficiently large expected frequencies, typically at least 5 in each cell. With small expected values, the approximation is poor and results are unreliable. For 2x2 tables with small samples, consider Fisher's exact test instead. CHISQ.TEST also assumes independent observations - each subject or item should appear in only one cell. Violations of these assumptions invalidate the test results.

## Syntax

> [!f(x)] CHISQ.TEST Syntax
>
> ```
> =CHISQ.TEST(actual_range, expected_range)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `actual_range` | Yes | The range containing observed frequencies. Must contain non-negative numbers. Dimensions must match expected_range exactly. |
| `expected_range` | Yes | The range containing expected frequencies under the null hypothesis. Must have same dimensions as actual_range. Values should be positive (expected frequencies cannot be zero or negative). |

### Return Value

Returns a probability value between 0 and 1 representing the p-value of the chi-square test. Small values (typically < 0.05) indicate statistically significant differences between observed and expected frequencies. Returns #N/A if ranges have different dimensions.

## Examples

> [!f(x)] CHISQ.TEST Examples

### Example 1: Basic 2x2 Contingency Table
```
=CHISQ.TEST(A1:B2, D1:E2)
```
Where A1:B2 contains observed: {{30, 20}, {25, 25}}
And D1:E2 contains expected: {{27.5, 22.5}, {27.5, 22.5}}

**Result:** 0.3247 (approximately)

**Explanation:** P-value of 32% indicates observed frequencies are consistent with expected. No significant association between the row and column variables.

---

### Example 2: Significant Association Found
```
=CHISQ.TEST(Observed, Expected)
```
Where Observed contains: {{50, 10}, {20, 40}}
And Expected contains: {{30, 30}, {30, 30}}

**Result:** 0.00001 (approximately)

**Explanation:** Very small p-value indicates strong evidence against independence. The variables are significantly associated - knowing one helps predict the other.

---

### Example 3: Dice Fairness Test
```
=CHISQ.TEST(A1:F1, G1:L1)
```
Where A1:F1 contains observed counts from 60 rolls: {8, 12, 9, 11, 10, 10}
And G1:L1 contains expected under fair die: {10, 10, 10, 10, 10, 10}

**Result:** 0.9215 (approximately)

**Explanation:** P-value of 92% strongly suggests the die is fair. Observed frequencies are well within normal random variation from expected even distribution.

---

### Example 4: Customer Preference Survey
```
=CHISQ.TEST(SurveyResults, UniformExpectation)
```
Where SurveyResults contains product preference counts: {45, 32, 28, 35}
And UniformExpectation contains: {35, 35, 35, 35}

**Result:** 0.1876 (approximately)

**Explanation:** P-value of 19% indicates no significant preference difference. Customer preferences are statistically consistent with equal popularity, though Product A leads numerically.

---

### Example 5: 3x3 Contingency Table
```
=CHISQ.TEST(A1:C3, E1:G3)
```
Testing independence between education level (3 levels) and job category (3 categories)

**Result:** P-value for independence test with df = (3-1)*(3-1) = 4

**Explanation:** Larger tables have more degrees of freedom. The p-value indicates whether education and job category are independent or associated.

---

### Example 6: Comparing to Critical Value Approach
```
P-value approach: =CHISQ.TEST(Observed, Expected)
Critical value approach: =SUMPRODUCT((Observed-Expected)^2/Expected) compared to =CHISQ.INV.RT(0.05, df)
```
**Result:** Both approaches give identical conclusions

**Explanation:** CHISQ.TEST provides p-value directly; alternatively, calculate chi-square statistic and compare to critical value. Both methods are equivalent.

---

### Example 7: Marketing Campaign A/B Test
```
=CHISQ.TEST(A1:B1, C1:D1)
```
Where A1:B1 contains: {85 conversions, 915 non-conversions} (Treatment)
And row 2: {70 conversions, 930 non-conversions} (Control)
Expected calculated from combined rates

**Result:** 0.0312 (approximately)

**Explanation:** P-value below 0.05 indicates significant difference in conversion rates between treatment and control groups.

---

### Example 8: Genetic Distribution Test
```
=CHISQ.TEST(ObservedPhenotypes, MendelianExpected)
```
Testing if observed phenotype ratios match expected 9:3:3:1 Mendelian ratio

**Result:** P-value for goodness-of-fit to genetic model

**Explanation:** Biologists use chi-square tests to verify genetic inheritance patterns. Non-significant p-value supports the genetic model.

---

### Example 9: Quality Control Defect Analysis
```
=CHISQ.TEST(DefectsByShift, ExpectedByProduction)
```
Where DefectsByShift contains defect counts per shift
And ExpectedByProduction contains expected defects proportional to shift production volume

**Result:** P-value indicates whether defects are distributed proportionally

**Explanation:** Significant result suggests certain shifts have systematic quality issues beyond what production volume explains.

---

### Example 10: Single Row (Goodness-of-Fit)
```
=CHISQ.TEST(A1:E1, F1:J1)
```
Where A1:E1 contains observed frequencies for 5 categories
And F1:J1 contains expected frequencies

**Result:** P-value with df = 5 - 1 = 4

**Explanation:** For single-row goodness-of-fit tests, degrees of freedom equals categories minus 1. CHISQ.TEST handles this automatically.

---

### Example 11: Verifying CHISQ.TEST Calculation
```
P-value: =CHISQ.TEST(Observed, Expected)
Manual: =CHISQ.DIST.RT(SUMPRODUCT((Observed-Expected)^2/Expected), df)
```
**Result:** Both formulas return identical p-values

**Explanation:** This verification shows CHISQ.TEST is equivalent to calculating chi-square statistic with SUMPRODUCT, then finding right-tail probability with CHISQ.DIST.RT.

---

### Example 12: Borderline Result Interpretation
```
=CHISQ.TEST(A1:C2, D1:F2)
```
**Result:** 0.0523

**Explanation:** P-value of 5.23% is just above the conventional 0.05 threshold. Technically "not significant" at 5% level, but warrants further investigation. Report the exact p-value rather than just "significant/not significant."

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Observed and expected ranges have different dimensions | Ensure both ranges have identical row and column counts. |
| `#VALUE!` | Non-numeric values in ranges | All cells in both ranges must contain numbers. Remove or convert text. |
| `#NUM!` | Zero or negative expected frequency | Expected frequencies must be positive. Check expected value calculations. |
| `#DIV/0!` | Zero expected frequency | Expected frequencies cannot be zero (division by zero in formula). Combine categories if needed. |
| `Very small p-value (e.g., 1E-50)` | Possible data error or massive sample | Verify data accuracy. Very large samples detect trivially small effects. |
| `P-value = 1` | Perfect match between observed and expected | Suspiciously perfect data may indicate fabrication or calculation error. Real data rarely matches expectations exactly. |

## Use Cases

### [[Market Research Survey Analysis]]

**Scenario:** A market researcher analyzes whether customer preferences for product features vary by demographic segment.

**Implementation:**
```
Observed: =matrix of actual preference counts by segment
Expected: =matrix of expected counts if preferences were independent of segment
P-value: =CHISQ.TEST(Observed, Expected)
```

**Business Application:** Understanding whether different customer segments have distinct preferences guides targeted marketing. If CHISQ.TEST shows significant association (low p-value), the company can develop segment-specific product variants or messaging. If p-value is high, a one-size-fits-all approach may suffice, reducing complexity.

**Technical Details:** Calculate expected frequencies carefully: Expected[i,j] = (row_i_total * column_j_total) / grand_total. Ensure each cell has expected frequency >= 5. For sparse data, combine small categories. Report effect size (Cramer's V) alongside p-value for practical significance.

---

### [[A/B Testing Significance Evaluation]]

**Scenario:** A product team evaluates whether a website change significantly affected conversion rates compared to the control version.

**Implementation:**
```
Observed: {{control_conversions, control_nonconversions}, {test_conversions, test_nonconversions}}
Expected: calculated from pooled conversion rate
=CHISQ.TEST(Observed, Expected)
```

**Business Application:** Before implementing changes permanently, statistical significance must be established. CHISQ.TEST determines whether observed conversion differences could reasonably occur by chance. A p-value below 0.05 gives confidence that the change has a real effect, justifying implementation.

**Technical Details:** For 2x2 tables with small samples, consider Yates' continuity correction or Fisher's exact test. The chi-square test is equivalent to a two-proportion z-test squared. For one-tailed hypotheses ("new version is better"), halve the two-tailed p-value from CHISQ.TEST.

---

### [[Genetic Inheritance Pattern Testing]]

**Scenario:** A geneticist tests whether observed phenotype frequencies in offspring match predicted Mendelian ratios.

**Implementation:**
```
Observed: {dominant-dominant, dominant-recessive, recessive-dominant, recessive-recessive}
Expected: {9/16, 3/16, 3/16, 1/16} * total_offspring
=CHISQ.TEST(Observed, Expected)
```

**Business Application:** Verifying genetic models supports breeding programs, drug development, and understanding disease inheritance. A non-significant chi-square test suggests observations match the genetic model, while significance indicates unexpected factors (linkage, selection, or incorrect model).

**Technical Details:** Degrees of freedom = number of categories - 1 - number of estimated parameters. If parameters are estimated from the same data being tested, subtract additional degrees of freedom. For simple ratios with no estimated parameters, df = categories - 1.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2010 and later
- **Legacy function:** CHITEST (still supported for backward compatibility)
- **Range handling:** Accepts any rectangular ranges of equal dimensions
- **Precision:** 15 significant decimal digits
- **Empty cells:** Treated as zero (may cause issues with expected frequencies)

### Google Sheets

- **Availability:** All versions since launch
- **Legacy function:** CHITEST also supported
- **Range handling:** Same as Excel
- **Precision:** 15 significant decimal digits
- **Empty cells:** Treated as zero

### Key Difference Alert

CHISQ.TEST behaves identically between Excel (2010+) and Google Sheets. Both platforms:
- Require ranges of identical dimensions
- Calculate chi-square statistic and p-value automatically
- Determine degrees of freedom from range dimensions
- Support the legacy CHITEST function name

The only compatibility issue is Excel 2007 and earlier, where only CHITEST exists (identical functionality).

## Tips and Best Practices

1. **Verify expected frequency assumptions.** All expected frequencies should ideally be 5 or greater. With smaller expected values, consider combining categories or using exact tests. Report any cells with low expected frequencies.

2. **Calculate expected values correctly.** For independence tests: Expected = (row total * column total) / grand total. For goodness-of-fit: Expected = hypothesized proportion * total count. CHISQ.TEST assumes correct expected values.

3. **Check range dimensions.** Observed and expected ranges must have identical dimensions. A mismatch returns #N/A. Double-check range selections before relying on results.

4. **Report exact p-values, not just significance.** Instead of just "p < 0.05," report "p = 0.023." This provides more information and allows readers to apply their own significance thresholds.

5. **Consider effect sizes.** Statistical significance doesn't imply practical significance. Calculate Cramer's V or phi coefficient to quantify association strength. Large samples can show significant but trivial effects.

6. **Ensure independence of observations.** Each observation should contribute to only one cell. Repeated measures on the same subjects violate independence and require different methods.

7. **Use for categorical data only.** Chi-square tests compare frequency counts in categories. For continuous data, use t-tests, ANOVA, or correlation analysis instead.

8. **Validate with manual calculation.** Occasionally verify CHISQ.TEST results by manually calculating the chi-square statistic and comparing to CHISQ.DIST.RT. This catches range selection errors.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CHISQ.DIST.RT]] | Right-tail chi-square probability | When you've calculated chi-square statistic separately |
| [[CHISQ.INV.RT]] | Critical value for significance level | When using critical value approach instead of p-value |
| [[CHITEST]] | Legacy function (same as CHISQ.TEST) | For compatibility with older workbooks |
| [[T.TEST]] | T-test for means | When comparing continuous data means, not categorical frequencies |
| [[F.TEST]] | F-test for variances | When comparing variances of continuous data |

### Commonly Used Together

**[[SUMPRODUCT]]** - Calculate chi-square statistic

*Verify CHISQ.TEST calculation:*
```
Chi-square statistic: =SUMPRODUCT((Observed-Expected)^2/Expected)
P-value check: =CHISQ.DIST.RT(chi_square, df)
Direct: =CHISQ.TEST(Observed, Expected)
```
Manual calculation helps verify CHISQ.TEST and provides the test statistic for reporting.

---

**[[MMULT]] and array formulas** - Expected frequency calculation

*Calculate expected frequencies for independence test:*
```
Row totals: Use SUM across rows
Column totals: Use SUM down columns
Expected[i,j]: =row_total[i] * col_total[j] / grand_total
```
Then apply CHISQ.TEST to observed and calculated expected ranges.

---

**[[IF]]** - Significance determination

*Automated significance decision:*
```
=IF(CHISQ.TEST(Observed, Expected) < 0.05, "Significant", "Not significant")
```
Provides immediate interpretation, though reporting exact p-values is preferred.

## Official Documentation

- **Microsoft Excel:** [CHISQ.TEST function](https://support.microsoft.com/en-us/office/chisq-test-function-2e8a7861-b14a-4985-aa93-fb88de3f260f)
- **Google Sheets:** [CHISQ.TEST function](https://support.google.com/docs/answer/9116295)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2010 | Replaced CHITEST with clearer naming |
| Excel 2013+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | Original launch (2006) | Available as CHITEST initially, CHISQ.TEST added later |

---

*Last updated: 2026-01-10*
