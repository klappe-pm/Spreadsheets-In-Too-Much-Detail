---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- order-statistics
- ranking
- top-n-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- kth-largest
- nth-largest
- top-value
tags:
- statistical
- ranking
- order-statistics
---

# LARGE

## Description

The LARGE function returns the k-th largest value from a dataset, enabling flexible extraction of top values without sorting the entire range. This order statistic function is invaluable for identifying top performers, highest values, or analyzing the upper tail of a distribution. By specifying different values of k, you can retrieve the largest (k=1), second largest (k=2), third largest (k=3), and so on, making it essential for leaderboards, top-n analysis, and performance reporting.

Unlike MAX, which returns only the single largest value, LARGE provides access to any position in the descending sorted order. This flexibility enables powerful applications such as summing the top 5 sales, averaging the highest 10 scores, identifying the 3rd highest bid, or creating dynamic rankings that update automatically as data changes. The function pairs naturally with SMALL, which performs the complementary operation of finding k-th smallest values.

LARGE is particularly useful in competitive scenarios like sales leaderboards, sports rankings, academic honor rolls, and financial analysis where understanding the top tier of values matters more than the full distribution. By varying the k parameter, you can analyze how values are distributed among top performers and identify gaps between ranking positions.

The function ignores text values, logical values, and empty cells in the range, counting only numeric values when determining positions. If k exceeds the count of numeric values or is less than 1, LARGE returns a #NUM! error. The k parameter must be a positive integer; decimal values are truncated.

## Syntax

> [!info] Function Syntax
> ```
> =LARGE(array, k)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| array | The range or array of numeric values from which to extract the k-th largest | Yes |
| k | The position from the largest (1 = largest, 2 = second largest, etc.) | Yes |

**Note:** The array can be a range, named range, or array of numbers. Text, logical values, and empty cells are ignored. The k value must be between 1 and the count of numeric values in the array.

## Examples

### Example 1: Largest Value
Find the maximum value (equivalent to MAX).
```
=LARGE(B2:B100, 1)
```
**Result:** Returns the largest value in the range (same as MAX).

### Example 2: Second Largest Value
Find the runner-up value.
```
=LARGE(B2:B100, 2)
```
**Result:** Returns the second highest value.

### Example 3: Top 3 Sales Values
List the top 3 sales figures.
```
=LARGE(Sales!B:B, 1)  → Highest
=LARGE(Sales!B:B, 2)  → Second highest
=LARGE(Sales!B:B, 3)  → Third highest
```
**Result:** The three highest sales values.

### Example 4: Sum of Top 5 Values
Calculate the total of the five highest values.
```
=LARGE(B2:B50, 1) + LARGE(B2:B50, 2) + LARGE(B2:B50, 3) + LARGE(B2:B50, 4) + LARGE(B2:B50, 5)
```
**Result:** Sum of the five largest values.

### Example 5: Sum Top N Using SUMPRODUCT
More elegant formula for summing top values.
```
=SUM(LARGE(B2:B100, {1,2,3,4,5}))
```
**Result:** Sum of the 5 largest values using array constant.

### Example 6: Dynamic Top N with Sequence
Sum top N where N is variable.
```
=SUM(LARGE(B2:B100, SEQUENCE(5)))
```
**Result:** Sum of top 5 using SEQUENCE (Excel 365).

### Example 7: Average of Top 10
Calculate mean of highest 10 scores.
```
=AVERAGE(LARGE(Scores!B:B, {1,2,3,4,5,6,7,8,9,10}))
```
**Result:** Average of the 10 highest scores.

### Example 8: Creating a Leaderboard
Build a top 10 leaderboard.
```
In cells B2:B11, use:
=LARGE(AllScores, ROW()-1)
```
**Result:** Dynamic list of top 10 scores that updates automatically.

### Example 9: Finding Position Gap
Calculate the difference between 1st and 2nd place.
```
=LARGE(Results, 1) - LARGE(Results, 2)
```
**Result:** The margin between winner and runner-up.

### Example 10: Nth Largest with Error Handling
Handle cases where k exceeds data count.
```
=IFERROR(LARGE(A1:A20, 25), "Not enough data")
```
**Result:** Returns error message if fewer than 25 values exist.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #NUM! | k is larger than the count of numeric values | Verify k is between 1 and COUNT(array) |
| #NUM! | k is less than 1 or zero | Ensure k is a positive integer |
| #VALUE! | k is not a number | Provide a numeric value for k |
| #NAME? | Function name misspelled | Verify spelling of LARGE |
| Unexpected result | Text or empty cells in range | These are ignored; verify intended values are included |
| Decimal k value | k is not an integer | k is truncated to integer (2.9 becomes 2) |

## Use Cases

### 1. Sales Performance Top Performers
**Scenario:** A sales manager needs to identify and reward the top 5 sales representatives each month based on revenue generated.

**Implementation:** Use LARGE to extract the top 5 sales values, then match to find the corresponding salespeople.

**Formula:**
```
Top 5 Values: =LARGE(SalesData!Revenue, SEQUENCE(5))
To get names: =INDEX(Names, MATCH(LARGE(Revenue,1), Revenue, 0))
```

**Analysis:** The top 5 values identify the threshold for recognition. Combining LARGE with INDEX/MATCH retrieves the corresponding names. This approach updates automatically as new sales data is added.

### 2. Academic Honor Roll Calculation
**Scenario:** A school needs to identify students eligible for honor roll (top 10% of class) and calculate the minimum GPA required for inclusion.

**Implementation:** Use LARGE to find the cutoff value for the top 10%.

**Formula:**
```
=LARGE(GPAs, ROUNDUP(COUNT(GPAs)*0.1, 0))
```

**Analysis:** This formula finds the GPA at the top 10% cutoff position. Students at or above this value qualify for honor roll. Using ROUNDUP ensures the cutoff is inclusive when the 10% calculation results in a fraction.

### 3. Financial Portfolio Analysis
**Scenario:** An investment analyst wants to analyze portfolio concentration by calculating what percentage of total value is held in the top 3 holdings.

**Implementation:** Sum the top 3 values and calculate as percentage of total.

**Formula:**
```
=SUM(LARGE(Holdings!Value, {1,2,3})) / SUM(Holdings!Value)
```

**Analysis:** If the top 3 holdings represent 60% of portfolio value, this indicates high concentration risk. Comparing top-3 concentration over time reveals whether the portfolio is becoming more or less diversified.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | All versions | Fully supported |
| Array constant support | Yes ({1,2,3}) | Yes |
| SEQUENCE integration | Excel 365/2019 | Yes |
| Maximum array size | Limited by memory | Limited by memory |
| Handles text in range | Ignores | Ignores |
| Handles logical in range | Ignores | Ignores |
| Decimal k handling | Truncates to integer | Truncates to integer |
| Precision | 15 significant digits | 15 significant digits |

**Note:** LARGE behaves consistently across platforms. Excel 365's SEQUENCE function simplifies dynamic top-n calculations.

## Tips and Best Practices

1. **Combine with SMALL for range analysis:** Use LARGE for top values and SMALL for bottom values to analyze both ends of your distribution simultaneously.

2. **Use array constants for multiple positions:** Instead of separate formulas, use =LARGE(range, {1,2,3,4,5}) to extract multiple positions at once.

3. **Leverage SEQUENCE for dynamic top-n:** In Excel 365, =LARGE(range, SEQUENCE(n)) creates a dynamic list of top n values that spills automatically.

4. **Handle ties appropriately:** LARGE returns duplicate values if they exist at different k positions. Two identical top values both appear when using k=1 and k=2.

5. **Validate k parameter:** Wrap LARGE in IFERROR or validate that k does not exceed COUNT(array) to prevent #NUM! errors.

6. **Consider MAXIFS for conditional:** If you need the largest value meeting specific criteria, MAXIFS may be more direct than filtering before LARGE.

## Related Functions

| Function | Description |
|----------|-------------|
| [[SMALL]] | Returns k-th smallest value (opposite of LARGE) |
| [[MAX]] | Returns only the largest value (equivalent to LARGE(array, 1)) |
| [[MIN]] | Returns only the smallest value |
| [[RANK]] | Returns the position/rank of a value within a dataset |
| [[RANK.EQ]] | Modern function for ranking with ties |
| [[PERCENTILE]] | Returns value at a specified percentile position |
| [[QUARTILE]] | Returns quartile values |

## Official Documentation

- [Microsoft Excel LARGE function](https://support.microsoft.com/en-us/office/large-function-3af0af19-cf62-4d45-b58a-4e5a0c2d9ab8)
- [Google Sheets LARGE function](https://support.google.com/docs/answer/3094015)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available in initial modern Excel releases |
| Excel 2007 | Increased range support |
| Excel 2010 | Performance improvements |
| Excel 2019/365 | Dynamic array support; works with SEQUENCE |
| Google Sheets | Supported since launch with identical behavior |

---

## LARGE vs. Sorting

**When to use LARGE:**
- You need only specific positions (top 3, top 10)
- Data should remain in original order
- Building dynamic leaderboards
- Combining with other functions like SUM or AVERAGE

**When to sort instead:**
- You need all values in order
- You're building a sorted table or report
- You need to see the full ranking

**Performance note:**
LARGE is efficient for extracting specific positions because it doesn't sort the entire dataset. For a few top values from a large dataset, LARGE outperforms sorting the entire range.
