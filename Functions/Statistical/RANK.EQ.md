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
- equal-rank
- competition-rank
tags:
- statistical
- ranking
- position
- modern
---

# RANK.EQ

## Description

The RANK.EQ function returns the position of a value within a dataset, using competition ranking where tied values receive the same rank and subsequent ranks are skipped. This is the modern, clearly-named replacement for the legacy RANK function, introduced in Excel 2010 as part of Microsoft's initiative to improve statistical function clarity. The ".EQ" suffix indicates "equal" ranking, meaning tied values receive equal (identical) ranks, distinguishing it from RANK.AVG which averages positions for ties.

When values in the dataset are identical, RANK.EQ assigns the same rank to all tied values, creating gaps in the ranking sequence. For example, if three values tie for 2nd place, all receive rank 2, and the next value receives rank 5 (skipping 3 and 4). This competition ranking system matches standard practices in sports standings, academic rankings, and most competitive scenarios where ties share the higher position.

RANK.EQ is essential for creating leaderboards, determining competitive standings, establishing cutoff points, and comparing individual values against a peer group. The function supports both descending order (highest value = rank 1, typical for scores and sales) and ascending order (lowest value = rank 1, typical for times and costs), making it versatile across different ranking contexts.

RANK.EQ produces identical results to the legacy RANK function but with clearer naming that explicitly describes its tie-handling behavior. Microsoft recommends RANK.EQ for new projects. The function ignores text values, logical values, and empty cells in the reference range, counting only numeric values when determining positions.

## Syntax

> [!info] Function Syntax
> ```
> =RANK.EQ(number, ref, [order])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number | The value whose rank you want to find | Yes |
| ref | The range or array of numbers against which to rank | Yes |
| order | How to rank: 0 or omitted = descending (largest is 1); non-zero = ascending (smallest is 1) | No |

**Note:** If number is not found in ref, RANK.EQ returns #N/A. The ref can include the number being ranked. Empty cells, text, and logical values in ref are ignored.

## Examples

### Example 1: Basic Descending Rank
Find ranking with highest value first.
```
=RANK.EQ(B2, $B$2:$B$20)
```
**Result:** Returns B2's position with highest value ranked 1st.

### Example 2: Ascending Rank
Rank where lowest is best.
```
=RANK.EQ(B2, $B$2:$B$20, 1)
```
**Result:** Returns rank with lowest value ranked 1st.

### Example 3: Comparison with RANK
Demonstrate identical results.
```
=RANK(A2, $A$2:$A$50, 0)     → 3
=RANK.EQ(A2, $A$2:$A$50, 0)  → 3
```
**Result:** Both functions return the same rank.

### Example 4: Competition Ranking with Ties
Multiple identical values.
```
Data: 100, 90, 90, 90, 80
=RANK.EQ(90, data, 0)
```
**Result:** Returns 2 for all three 90s; the 80 receives rank 5.

### Example 5: Sales Leaderboard
Create dynamic rankings for sales team.
```
=RANK.EQ([@Revenue], [Revenue], 0)
```
**Result:** Each salesperson's position using table references.

### Example 6: Race Time Ranking
Rank times where fastest (lowest) wins.
```
=RANK.EQ(B2, $B$2:$B$100, 1)
```
**Result:** Fastest time receives rank 1.

### Example 7: Rank-Based Categorization
Create tier labels based on rank.
```
=SWITCH(TRUE,
  RANK.EQ(B2,$B:$B)=1, "Champion",
  RANK.EQ(B2,$B:$B)<=3, "Medalist",
  RANK.EQ(B2,$B:$B)<=10, "Top 10",
  "Participant")
```
**Result:** Descriptive category based on ranking position.

### Example 8: Percentile from Rank
Convert rank to percentile.
```
=(COUNT($B$2:$B$100)-RANK.EQ(B2,$B$2:$B$100)+1)/COUNT($B$2:$B$100)*100
```
**Result:** Percentile position (0-100) based on rank.

### Example 9: Combined with COUNTIF for Unique Ranks
Create ranks without gaps (dense ranking).
```
=SUMPRODUCT(($B$2:$B$100>B2)*1)+1
```
**Result:** Dense rank where ties don't create gaps.

### Example 10: Filtered Ranking
Rank within a specific category.
```
=SUMPRODUCT((Region=$D$2:$D$100="East")*(Sales>$C$2:$C$100)*1)+1
```
**Result:** Rank within only the East region.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | number not found in ref | Verify value exists in reference range |
| #VALUE! | Non-numeric arguments | Ensure number and ref contain numeric values |
| #NAME? | Function not recognized (pre-2010 Excel) | Use RANK in older versions; upgrade to Excel 2010+ |
| Incorrect rank | Relative references shifting | Use absolute references ($) for ref parameter |
| Unexpected ties | Competition ranking creates gaps | Expected behavior; use RANK.AVG for averaged ranks |

## Use Cases

### 1. Employee Performance Ranking
**Scenario:** HR needs to rank employees by performance score for annual reviews, with clear standings for promotion decisions.

**Implementation:** Apply competition ranking where tied scores share the same position.

**Formula:**
```
=RANK.EQ([@PerformanceScore], [PerformanceScore], 0)
```

**Analysis:** Competition ranking (RANK.EQ) is appropriate here because if two employees tie for 2nd place, they both deserve recognition as "2nd place" rather than being artificially separated. The gap in rankings (jumping to 4th) accurately reflects that three positions are filled.

### 2. Investment Performance Comparison
**Scenario:** A portfolio manager needs to rank fund returns within a peer group to assess relative performance.

**Implementation:** Rank each fund's return against all funds in the category.

**Formula:**
```
=RANK.EQ(B2, $B$2:$B$50, 0) & " of " & COUNT($B$2:$B$50)
```

**Analysis:** Displaying rank as "5 of 50" provides context. A fund ranked in the top quartile (ranks 1-12 of 50) demonstrates competitive performance. Ties are handled appropriately since two funds with identical returns should share the same ranking.

### 3. Customer Segmentation by Purchase Volume
**Scenario:** A marketing team needs to identify VIP customers by ranking them based on total purchase value.

**Implementation:** Assign ranks to create customer tiers.

**Formula:**
```
=IF(RANK.EQ([@TotalSpend], [TotalSpend])<=ROUND(COUNT([TotalSpend])*0.1,0), "VIP", "Standard")
```

**Analysis:** The top 10% of customers by spend are labeled "VIP" for special treatment. RANK.EQ ensures that if multiple customers tie at the cutoff, they all receive the higher ranking (and thus VIP status if they tie at position 10 in a 100-customer list).

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Relationship to RANK | Identical results | Identical results |
| Default order | Descending (0) | Descending (0) |
| Tie handling | Competition ranking | Competition ranking |
| Maximum range size | Limited by memory | Limited by memory |
| Text in range | Ignored | Ignored |
| Error for missing value | #N/A | #N/A |

**Note:** RANK.EQ behaves identically across platforms and produces the same results as the legacy RANK function.

## Tips and Best Practices

1. **Use RANK.EQ over RANK:** Although results are identical, RANK.EQ's naming explicitly describes its behavior and distinguishes it from RANK.AVG.

2. **Understand competition ranking:** RANK.EQ gives all tied values the highest applicable rank. If three items tie for 2nd, all get rank 2 and the next is rank 5. This is standard for sports and academic standings.

3. **Use RANK.AVG for statistical purposes:** When you need ranks for correlation or other statistical calculations, RANK.AVG often provides better results by avoiding rank gaps.

4. **Apply absolute references:** When copying formulas, use absolute references ($B$2:$B$100) for the ref parameter to prevent range shifting.

5. **Consider dense ranking:** If you need consecutive ranks without gaps (1, 2, 2, 3, 4), use SUMPRODUCT to count values greater than current: =SUMPRODUCT((ref>number)*1)+1

6. **Validate with COUNT:** Ensure ranks fall within expected range (1 to COUNT(ref)) to verify formula correctness.

## Related Functions

| Function | Description |
|----------|-------------|
| [[RANK]] | Legacy function identical to RANK.EQ |
| [[RANK.AVG]] | Averages rank positions for tied values |
| [[LARGE]] | Returns k-th largest value |
| [[SMALL]] | Returns k-th smallest value |
| [[PERCENTRANK.INC]] | Returns rank as a percentile (inclusive) |
| [[PERCENTRANK.EXC]] | Returns rank as a percentile (exclusive) |

## Official Documentation

- [Microsoft Excel RANK.EQ function](https://support.microsoft.com/en-us/office/rank-eq-function-284858ce-8ef6-450e-b662-26245be04a40)
- [Google Sheets RANK function](https://support.google.com/docs/answer/3094091)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | RANK.EQ introduced alongside RANK.AVG |
| Excel 2013 | Performance optimizations |
| Excel 2016 | Improved with dynamic array compatibility |
| Excel 2019/365 | Full integration with modern Excel features |
| Google Sheets | Available since RANK.EQ was added |

---

## RANK.EQ vs. RANK.AVG: Choosing the Right Function

**RANK.EQ (Competition/Standard Ranking):**
```
Values: 100, 90, 90, 80
Ranks:    1,  2,  2,  4
```
- Tied values get the SAME rank (both 90s = rank 2)
- Creates GAPS in sequence (no rank 3)
- Standard for sports, academic standings
- Use when: Displaying public rankings, leaderboards

**RANK.AVG (Fractional Ranking):**
```
Values: 100, 90, 90, 80
Ranks:    1, 2.5, 2.5,  4
```
- Tied values get AVERAGE of positions they span
- NO gaps in the sequence
- Better for statistical calculations
- Use when: Calculating rank correlations, percentiles

**Decision Guide:**
- Displaying standings to users? Use RANK.EQ
- Statistical analysis with ranks? Use RANK.AVG
- Need consecutive ranks (1,2,2,3)? Use dense ranking formula
