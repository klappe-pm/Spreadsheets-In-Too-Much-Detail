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
- leaderboards
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ranking
- position
tags:
- statistical
- ranking
- position
- legacy
---

# RANK

## Description

The RANK function returns the position of a value within a dataset relative to other values, indicating where a specific number falls when the data is sorted. This is essential for creating leaderboards, determining competitive standings, identifying percentile positions, and understanding where a particular value sits within a distribution. RANK can order values in descending order (highest = 1) or ascending order (lowest = 1) based on the order parameter.

When values in the dataset are tied (identical values), RANK assigns the same rank to all tied values, leaving gaps in the ranking sequence. For example, if two values tie for 2nd place, both receive rank 2, and the next rank assigned is 4 (skipping 3). This behavior is known as "competition ranking" or "1224 ranking" and matches how sports standings and academic rankings typically work.

RANK is widely used in sales performance analysis, academic grading, sports standings, competitive bidding, and any scenario requiring comparison of a value against a set of peers. Understanding a value's rank often provides more actionable insight than the raw value itself - knowing a score is "3rd highest" conveys competitive context that "score of 87" alone does not.

This function is a legacy function maintained for backward compatibility. Microsoft introduced RANK.EQ (identical to RANK) and RANK.AVG (averages ranks for ties) in Excel 2010 to provide clearer naming and alternative tie-handling. For new projects, Microsoft recommends RANK.EQ, which produces identical results to RANK with clearer naming convention.

## Syntax

> [!info] Function Syntax
> ```
> =RANK(number, ref, [order])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number | The value whose rank you want to find | Yes |
| ref | The range or array of numbers against which to rank | Yes |
| order | How to rank: 0 or omitted = descending (largest is 1); non-zero = ascending (smallest is 1) | No |

**Note:** If number is not found in ref, RANK returns #N/A. The ref can include the number being ranked. Empty cells, text, and logical values in ref are ignored.

## Examples

### Example 1: Basic Descending Rank
Find where a sales value ranks (highest = 1st).
```
=RANK(B2, $B$2:$B$20)
```
**Result:** Returns the rank of B2's value with highest value ranked 1st.

### Example 2: Ascending Rank
Rank where lowest is best (e.g., race times).
```
=RANK(B2, $B$2:$B$20, 1)
```
**Result:** Returns rank with lowest value ranked 1st.

### Example 3: Sales Team Leaderboard
Create rankings for all salespeople.
```
Cell C2: =RANK(B2, $B$2:$B$50, 0)
Copy down to C50
```
**Result:** Each row shows that person's rank among all salespeople.

### Example 4: Handling Ties
When multiple values are identical.
```
Data: 100, 95, 95, 90
=RANK(95, data)
```
**Result:** Both 95s receive rank 2; the 90 receives rank 4 (not 3).

### Example 5: Rank Within Named Range
Using a defined name for cleaner formulas.
```
=RANK(A2, AllScores, 0)
```
**Result:** Rank of A2 within the named range AllScores.

### Example 6: Percentile-Based Rank
Calculate percentile rank.
```
=1-(RANK(B2,$B$2:$B$100)-1)/(COUNT($B$2:$B$100)-1)
```
**Result:** Returns percentile (0 to 1) based on rank.

### Example 7: Combining with IF for Labels
Add descriptive labels to rankings.
```
=IF(RANK(B2,$B:$B)<=3, "Top 3", IF(RANK(B2,$B:$B)<=10, "Top 10", "Other"))
```
**Result:** Categorizes values based on their rank position.

### Example 8: Error Handling for Value Not Found
Handle cases where value isn't in reference range.
```
=IFERROR(RANK(A2, B2:B100), "Not in list")
```
**Result:** Returns message if the value doesn't exist in the range.

### Example 9: Rank Among Multiple Criteria
Rank within a filtered subset.
```
=SUMPRODUCT((C$2:C$100="East")*(B$2:B$100>=B2)*(B$2:B$100<>""))
```
**Result:** Rank of B2 among only "East" region values.

### Example 10: Dynamic Rank for Current Row
Self-referencing rank in a table.
```
=RANK([@Sales], [Sales], 0)
```
**Result:** Rank using structured table references.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | number is not found in ref | Verify the value exists in the reference range |
| #VALUE! | Invalid data types | Ensure number and ref contain numeric values |
| #NAME? | Function name misspelled | Verify spelling of RANK |
| Incorrect rank | Relative vs absolute references | Use absolute references ($) for ref when copying formula |
| Ties not handled as expected | RANK uses competition ranking | Use RANK.AVG for averaged ranks or add tiebreaker |

## Use Cases

### 1. Sales Team Leaderboard
**Scenario:** A sales manager needs to rank the sales team by monthly revenue to display standings and determine bonus eligibility.

**Implementation:** Create a rank column that updates automatically as sales figures change.

**Formula:**
```
=RANK([@Revenue], [Revenue], 0)
```

**Analysis:** Descending rank (0) makes highest revenue = rank 1. If two salespeople tie, both receive the same rank. The leaderboard updates in real-time as deals close, providing motivation and transparency.

### 2. Academic Class Ranking
**Scenario:** A registrar needs to determine class rank for all students based on cumulative GPA for transcripts and honor roll determination.

**Implementation:** Calculate each student's position within their graduating class.

**Formula:**
```
=RANK(B2, $B$2:$B$500, 0)
```

**Analysis:** Students with identical GPAs receive the same rank (appropriate for academic records). The formula uses absolute references so it can be copied for all students. Ties are handled using competition ranking (if two students tie for 2nd, the next rank is 4th).

### 3. Vendor Bid Ranking
**Scenario:** A procurement team evaluates vendor bids and needs to rank them from lowest (best) to highest price.

**Implementation:** Use ascending order ranking where lowest bid = rank 1.

**Formula:**
```
=RANK(B2, $B$2:$B$20, 1)
```

**Analysis:** The order parameter of 1 ranks lowest values first (appropriate for pricing where lower is better). This quickly identifies the winning bid and runner-ups for evaluation.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | All versions (legacy since Excel 2010) | Fully supported |
| Default order | Descending (0) | Descending (0) |
| Tie handling | Competition ranking (gaps) | Competition ranking (gaps) |
| Recommended replacement | RANK.EQ | RANK.EQ available |
| Maximum range size | Limited by memory | Limited by memory |
| Text in range | Ignored | Ignored |
| Error for missing value | #N/A | #N/A |

**Note:** Both platforms treat RANK identically. Excel marks RANK as legacy, recommending RANK.EQ for new work.

## Tips and Best Practices

1. **Use RANK.EQ for new work:** While RANK works identically, RANK.EQ has clearer naming. Use RANK.AVG when you want tied values to share the average of their positions rather than the lowest position.

2. **Use absolute references:** When copying RANK formulas down a column, make the ref parameter absolute ($B$2:$B$100) so it doesn't shift.

3. **Understand tie handling:** RANK uses competition ranking where ties receive the same rank and leave gaps. If this causes issues, consider RANK.AVG or add a secondary sort criterion.

4. **Choose order carefully:** Use 0 (descending) when higher values are better (scores, sales). Use 1 (ascending) when lower values are better (times, costs, errors).

5. **Combine with COUNTIF for percentile rank:** Use (RANK-1)/(COUNT-1) or similar to convert rank to percentile.

6. **Handle #N/A errors:** Wrap RANK in IFERROR if the value might not exist in the reference range.

## Related Functions

| Function | Description |
|----------|-------------|
| [[RANK.EQ]] | Modern replacement; identical to RANK |
| [[RANK.AVG]] | Averages rank positions for tied values |
| [[LARGE]] | Returns k-th largest value |
| [[SMALL]] | Returns k-th smallest value |
| [[PERCENTRANK]] | Returns rank as a percentile |
| [[PERCENTRANK.INC]] | Percentile rank (inclusive) |
| [[PERCENTRANK.EXC]] | Percentile rank (exclusive) |

## Official Documentation

- [Microsoft Excel RANK function](https://support.microsoft.com/en-us/office/rank-function-6a2fc49d-1831-4a03-9e6c-fc2b540a4c28)
- [Google Sheets RANK function](https://support.google.com/docs/answer/3094091)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available in initial modern Excel releases |
| Excel 2007 | Performance improvements |
| Excel 2010 | Marked as legacy; RANK.EQ and RANK.AVG introduced |
| Excel 2019/365 | Continued support for backward compatibility |
| Google Sheets | Supported since launch with identical behavior |

---

## Understanding Tie Handling

**RANK / RANK.EQ (Competition Ranking):**
```
Values: 100, 95, 95, 90, 85
Ranks:    1,  2,  2,  4,  5
```
Both 95s get rank 2; next value gets rank 4 (skips 3).

**RANK.AVG (Average Ranking):**
```
Values: 100, 95, 95, 90, 85
Ranks:    1, 2.5, 2.5, 4,  5
```
Both 95s get rank 2.5 (average of 2 and 3); no gaps.

**Dense Ranking (not native in Excel):**
```
Values: 100, 95, 95, 90, 85
Ranks:    1,  2,  2,  3,  4
```
Would require custom formula: =SUMPRODUCT((ref>number)+1)
