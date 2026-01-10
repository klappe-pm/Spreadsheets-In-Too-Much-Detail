---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- ranking
- order-statistics
- statistical-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- average-rank
- fractional-rank
tags:
- statistical
- ranking
- position
- fractional
---

# RANK.AVG

## Description

The RANK.AVG function returns the position of a value within a dataset, using average ranking where tied values receive the mean of the positions they would occupy. Unlike RANK.EQ (competition ranking) which gives all tied values the same rank and creates gaps, RANK.AVG assigns each tied value the average of their shared positions, eliminating gaps in the ranking sequence. This approach is particularly valuable for statistical calculations where fractional ranks provide more meaningful results.

When values in the dataset are identical, RANK.AVG calculates the average of the positions those values would occupy if they were slightly different. For example, if three values tie for positions 2, 3, and 4, each receives rank 3 (the average of 2+3+4=9/3=3). The next value then receives rank 5, maintaining sequence integrity. This fractional ranking system is preferred for statistical tests like Spearman's rank correlation because it preserves information about the data structure.

RANK.AVG was introduced in Excel 2010 alongside RANK.EQ to provide explicit control over tie handling. While RANK.EQ matches the behavior of the legacy RANK function (competition ranking), RANK.AVG offers an alternative appropriate for statistical analysis, research applications, and scenarios where fractional ranks are meaningful. The function is particularly useful in non-parametric statistics and when converting rank data for further analysis.

The function supports both descending order (highest value = rank 1) and ascending order (lowest value = rank 1), making it versatile across different contexts. RANK.AVG ignores text values, logical values, and empty cells in the reference range, counting only numeric values when determining positions.

## Syntax

> [!info] Function Syntax
> ```
> =RANK.AVG(number, ref, [order])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number | The value whose rank you want to find | Yes |
| ref | The range or array of numbers against which to rank | Yes |
| order | How to rank: 0 or omitted = descending (largest is 1); non-zero = ascending (smallest is 1) | No |

**Note:** If number is not found in ref, RANK.AVG returns #N/A. The ref can include the number being ranked. Empty cells, text, and logical values in ref are ignored.

## Examples

### Example 1: Basic Average Ranking
Find position with averaged ties.
```
=RANK.AVG(B2, $B$2:$B$20)
```
**Result:** Returns B2's position with fractional rank for ties.

### Example 2: Comparing RANK.EQ and RANK.AVG
Demonstrate the difference in tie handling.
```
Data: 100, 90, 90, 80
=RANK.EQ(90, data)  → 2 (competition ranking)
=RANK.AVG(90, data) → 2.5 (average of positions 2 and 3)
```
**Result:** RANK.AVG returns fractional rank for tied values.

### Example 3: Three-Way Tie
When three values are identical.
```
Data: 100, 85, 85, 85, 70
=RANK.AVG(85, data, 0)
```
**Result:** 3 (average of positions 2, 3, and 4).

### Example 4: Ascending Order for Times
Rank race times where lowest is best.
```
=RANK.AVG(B2, $B$2:$B$50, 1)
```
**Result:** Fractional rank with fastest time as rank 1.

### Example 5: Spearman Correlation Preparation
Calculate ranks for correlation analysis.
```
Column C: =RANK.AVG(A2, $A$2:$A$50)
Column D: =RANK.AVG(B2, $B$2:$B$50)
Then: =CORREL(C:C, D:D)
```
**Result:** Proper fractional ranks for Spearman correlation.

### Example 6: No Ties - Same as RANK.EQ
When all values are unique.
```
Data: 100, 90, 85, 80
=RANK.EQ(90, data)  → 2
=RANK.AVG(90, data) → 2
```
**Result:** Without ties, both functions return identical integer ranks.

### Example 7: Named Range Usage
Using defined names for clarity.
```
=RANK.AVG(A2, SurveyScores, 0)
```
**Result:** Fractional rank within the named range.

### Example 8: Converting to Percentile
Calculate percentile from average rank.
```
=(COUNT($B$2:$B$100)-RANK.AVG(B2,$B$2:$B$100)+0.5)/COUNT($B$2:$B$100)
```
**Result:** Percentile position based on average rank.

### Example 9: Large Dataset with Many Ties
Handle datasets with frequent duplicates.
```
Survey ratings (1-5 scale):
=RANK.AVG(B2, $B$2:$B$1000, 0)
```
**Result:** Fractional ranks appropriately handle numerous tied responses.

### Example 10: Error Handling
Handle missing values gracefully.
```
=IFERROR(RANK.AVG(A2, $B$2:$B$100), "Not found")
```
**Result:** Returns message if value doesn't exist in range.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | number not found in ref | Verify value exists in reference range |
| #VALUE! | Non-numeric arguments | Ensure number and ref contain numeric values |
| #NAME? | Function not recognized (pre-2010 Excel) | Upgrade to Excel 2010+; function not available in earlier versions |
| Incorrect rank | Relative references shifting | Use absolute references ($) for ref parameter |
| Non-integer result | This is expected for ties | RANK.AVG intentionally returns fractional ranks |

## Use Cases

### 1. Spearman Rank Correlation Analysis
**Scenario:** A researcher needs to calculate the correlation between two ordinal variables (like education level and income bracket) using Spearman's rank correlation coefficient.

**Implementation:** Use RANK.AVG to assign ranks that properly handle ties, then calculate correlation.

**Formula:**
```
X Ranks: =RANK.AVG(A2, $A$2:$A$100)
Y Ranks: =RANK.AVG(B2, $B$2:$B$100)
Correlation: =CORREL(XRanks, YRanks)
```

**Analysis:** RANK.AVG is required for accurate Spearman correlation because fractional ranks ensure that the sum of ranks equals the expected value (n(n+1)/2). Using RANK.EQ with its gaps would produce incorrect correlation coefficients for data with ties.

### 2. Non-Parametric Statistical Tests
**Scenario:** A statistician performs a Mann-Whitney U test to compare two groups when data isn't normally distributed.

**Implementation:** Assign average ranks to combined data for proper test statistic calculation.

**Formula:**
```
=RANK.AVG(B2, CombinedData, 1)
```

**Analysis:** Many non-parametric tests require average ranking for ties to maintain test validity. The fractional ranks ensure that the sum of ranks in each group is calculated correctly, which directly affects the U statistic and p-value.

### 3. Fair Allocation Based on Ranking
**Scenario:** A committee allocates resources based on application scores, where applicants with identical scores should receive equal consideration (fractional share).

**Implementation:** Use average ranks to determine proportional allocation.

**Formula:**
```
=RANK.AVG([@Score], [Score], 0)
Share: =1/COUNTIF([Score], [@Score])
```

**Analysis:** When three applicants tie for positions 2-4, each receives rank 3 rather than all getting rank 2 (which would overstate their position) or arbitrary differentiation. This provides a fair basis for proportional resource allocation.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Legacy alternative | No equivalent before 2010 | N/A |
| Default order | Descending (0) | Descending (0) |
| Tie handling | Average of positions | Average of positions |
| Fractional results | Yes (e.g., 2.5, 3.5) | Yes |
| Maximum range size | Limited by memory | Limited by memory |
| Text in range | Ignored | Ignored |

**Note:** RANK.AVG behaves identically across Excel and Google Sheets. There is no legacy equivalent with averaged tie handling before Excel 2010.

## Tips and Best Practices

1. **Use for statistical calculations:** RANK.AVG is preferred for Spearman correlation, Mann-Whitney U test, Kruskal-Wallis test, and other non-parametric statistics where proper tie handling affects results.

2. **Expect fractional results:** RANK.AVG intentionally returns decimal values (2.5, 3.5) when ties exist. Format cells appropriately and don't assume integer results.

3. **Choose based on purpose:** Use RANK.AVG for statistical analysis; use RANK.EQ for public-facing leaderboards where fractional ranks would be confusing.

4. **Verify with unique data:** When no ties exist, RANK.AVG and RANK.EQ return identical results. Use test data with ties to verify your formulas behave as expected.

5. **Apply absolute references:** When copying formulas, use absolute references ($B$2:$B$100) for the ref parameter to prevent range shifting.

6. **Consider the audience:** Fractional ranks like "2.5" are meaningful in statistical contexts but may confuse end users in reports. Choose the appropriate function based on how results will be used.

## Related Functions

| Function | Description |
|----------|-------------|
| [[RANK.EQ]] | Competition ranking (gives same rank to ties, creates gaps) |
| [[RANK]] | Legacy function identical to RANK.EQ |
| [[LARGE]] | Returns k-th largest value |
| [[SMALL]] | Returns k-th smallest value |
| [[PERCENTRANK.INC]] | Returns rank as a percentile (inclusive) |
| [[PERCENTRANK.EXC]] | Returns rank as a percentile (exclusive) |
| [[CORREL]] | Calculates correlation coefficient |

## Official Documentation

- [Microsoft Excel RANK.AVG function](https://support.microsoft.com/en-us/office/rank-avg-function-bd406a6f-eb38-4d73-aa8e-6d1c3c72e83a)
- [Google Sheets RANK function](https://support.google.com/docs/answer/3094091)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | RANK.AVG introduced alongside RANK.EQ |
| Excel 2013 | Performance optimizations |
| Excel 2016 | Improved dynamic array compatibility |
| Excel 2019/365 | Full integration with modern Excel features |
| Google Sheets | Available since RANK.AVG was added |

---

## Comparing Ranking Methods

**RANK.EQ (Competition Ranking - 1224):**
```
Values: 100, 90, 90, 90, 80
Ranks:    1,  2,  2,  2,  5
Sum of ranks: 1+2+2+2+5 = 12 (not equal to 1+2+3+4+5=15)
```
- Same rank for ties, gaps in sequence
- Standard for displayed standings
- Sum of ranks less than expected

**RANK.AVG (Fractional Ranking - 1333):**
```
Values: 100, 90, 90, 90, 80
Ranks:    1,  3,  3,  3,  5
Sum of ranks: 1+3+3+3+5 = 15 (equals 1+2+3+4+5=15)
```
- Average of tied positions
- Preserves sum of ranks
- Required for statistical tests

**Dense Ranking (1223 - not native):**
```
Values: 100, 90, 90, 90, 80
Ranks:    1,  2,  2,  2,  3
```
- Same rank for ties, no gaps
- Requires custom formula

**When to use each:**
- Public leaderboards: RANK.EQ
- Statistical analysis: RANK.AVG
- Sequential numbering: Dense ranking formula
