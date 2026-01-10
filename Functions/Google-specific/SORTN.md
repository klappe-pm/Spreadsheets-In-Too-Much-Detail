---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - sorting
  - data-manipulation
subTopics:
  - top-n-values
  - sorting-limiting
  - leaderboards
  - ranking
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - sort-top-n
  - top-values
  - sort-limit
  - best-worst
tags:
  - google-sheets-only
  - sorting
  - top-n
  - ranking
  - data-analysis
  - leaderboards
---

# SORTN

## Description

The **SORTN function** is a Google Sheets-exclusive function that returns the first n items from a dataset after sorting, making it perfect for creating "Top 10" lists, identifying worst performers, finding recent entries, and any scenario where you need a limited, sorted subset of data. SORTN combines sorting and limiting in a single efficient operation, avoiding the need to sort an entire dataset just to extract a few values. The function is particularly valuable for dashboards, leaderboards, exception reports, and performance summaries.

The function accepts a range to sort, the number of items to return, a tie-handling mode, and then sort column/order pairs for multi-level sorting. The tie-handling mode is a unique feature that controls what happens when multiple rows have the same sort value—you can keep all ties (potentially returning more than n items), display ties within the n limit, or remove duplicates entirely. This flexibility makes SORTN suitable for various ranking scenarios, from strict "exactly 10 items" requirements to "all items tied for 10th place" needs.

SORTN supports multi-column sorting by specifying multiple sort_column and is_ascending pairs. For example, you can sort by sales descending first, then by name ascending for ties. This creates sophisticated ranking logic in a single formula without intermediate sort operations. The function works with any sortable data type—numbers, text, or dates—and handles mixed data appropriately.

SORTN is exclusively available in Google Sheets and has no direct equivalent in Microsoft Excel. Excel users must combine SORT (or LARGE/SMALL) with INDEX or other functions to achieve similar results, often requiring complex array formulas or helper columns. The closest Excel approach is SORT combined with array slicing or TAKE (in Excel 365), but the tie-handling modes and syntax differ significantly. This makes SORTN one of Google Sheets' most distinctive and powerful data analysis functions.

## Syntax

> [!f(x)] SORTN Syntax
>
> ```
> =SORTN(range, [n], [display_ties_mode], [sort_column1, is_ascending1], [sort_column2, is_ascending2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `range` | Yes | The data range to sort and limit. Can be a multi-column range; all columns are returned in the output. |
| `n` | No | The number of items to return. Default is 1. When ties exist, actual count may vary based on display_ties_mode. |
| `display_ties_mode` | No | How to handle tied values: 0 = show first n, ignore ties beyond n (default). 1 = show all tied values (may return >n). 2 = show first n, removing duplicate sort values. 3 = show first n after removing all duplicates from data. |
| `sort_column1` | No | The column index to sort by (1 for first column, 2 for second, etc.). Default is 1. |
| `is_ascending1` | No | TRUE for ascending (A-Z, low-high), FALSE for descending (Z-A, high-low). Default is TRUE. |
| `sort_column2, is_ascending2, ...` | No | Additional sort column and direction pairs for multi-level sorting. Applied after previous sort levels. |

### Return Value

Returns an array containing n rows (or more/fewer depending on ties and display_ties_mode) from the sorted input range. All columns from the input range are included in the output. Rows are ordered according to the specified sort criteria.

## Examples

> [!f(x)] SORTN Examples

### Example 1: Top 5 Highest Values
```
=SORTN(A2:B100, 5, 0, 2, FALSE)
```
**Scenario:** Column A has names, Column B has sales figures; get top 5 sellers
**Result:** 5 rows with the highest values in column B, sorted descending

**Explanation:** SORTN sorts by column 2 (sales) in descending order (FALSE) and returns the first 5 rows. This is the classic "Top N" use case.

---

### Example 2: Bottom 3 Performers
```
=SORTN(A2:B100, 3, 0, 2, TRUE)
```
**Scenario:** Find the 3 employees with lowest sales
**Result:** 3 rows with the smallest values in column B

**Explanation:** Using TRUE for is_ascending sorts low-to-high, so the "first" 3 items are the lowest values—ideal for identifying underperformers.

---

### Example 3: Most Recent 10 Entries
```
=SORTN(A2:D100, 10, 0, 1, FALSE)
```
**Scenario:** Column A contains dates; get 10 most recent records
**Result:** 10 rows with the most recent dates

**Explanation:** Sorting dates descending puts newest first. SORTN then takes the first 10, giving you the most recent entries.

---

### Example 4: Top 5 Including All Ties
```
=SORTN(A2:B100, 5, 1, 2, FALSE)
```
**Scenario:** Get top 5 sales, but include everyone tied at the 5th position
**Result:** 5+ rows if multiple people share the 5th-highest value

**Explanation:** display_ties_mode=1 ensures fair representation when ties exist at the cutoff. You might get 7 rows if 3 people are tied for 5th.

---

### Example 5: Top 5 Unique Values Only
```
=SORTN(A2:B100, 5, 2, 2, FALSE)
```
**Scenario:** Get top 5 distinct sales values, one row per unique value
**Result:** 5 rows with different values in the sort column

**Explanation:** display_ties_mode=2 removes duplicates from the sort column, ensuring each returned row has a unique value. Useful for finding the actual top 5 different amounts.

---

### Example 6: Multi-Column Sort - Sales then Name
```
=SORTN(A2:C100, 10, 0, 2, FALSE, 1, TRUE)
```
**Scenario:** Top 10 by sales (descending), then alphabetically by name for ties
**Result:** 10 rows sorted by sales, with same-sales rows ordered by name

**Explanation:** Multiple sort pairs create secondary ordering. After sorting by column 2 (sales) descending, ties are broken by column 1 (name) ascending.

---

### Example 7: First 3 Alphabetically
```
=SORTN(A2:A100, 3)
```
**Scenario:** Get the first 3 names alphabetically
**Result:** The 3 names that sort first A-Z

**Explanation:** Default values (n=1, sort ascending by column 1) make basic alphabetical sorting simple. Adjusting n gets first N items.

---

### Example 8: Top N from FILTER Results
```
=SORTN(FILTER(A2:C100, B2:B100="Active"), 5, 0, 3, FALSE)
```
**Scenario:** Top 5 active users by score (column C)
**Result:** 5 rows from the Active subset, highest scores first

**Explanation:** FILTER first narrows the data, then SORTN ranks and limits the filtered subset. This pattern is very common.

---

### Example 9: Top Performers Per Category (with QUERY)
```
=QUERY(A2:C100, "SELECT A, MAX(C) GROUP BY A ORDER BY MAX(C) DESC LIMIT 5")
```
**Scenario:** Alternative approach for top N with grouping
**Result:** Top 5 categories by their maximum value

**Explanation:** While not SORTN, this shows that QUERY can achieve similar results with grouping. SORTN works on row-level data without aggregation.

---

### Example 10: Bottom 5 with All Data Columns
```
=SORTN(A2:F100, 5, 0, 3, TRUE)
```
**Scenario:** Get the 5 records with lowest values in column C, keeping all 6 columns
**Result:** 5 rows with all original columns, sorted by column C ascending

**Explanation:** SORTN preserves all columns from the input—you get complete records, not just the sort column.

---

### Example 11: Top 3 Completely Unique Rows
```
=SORTN(A2:C100, 3, 3, 2, FALSE)
```
**Scenario:** Get 3 highest values from rows with no duplicates across any column
**Result:** 3 completely unique rows by sort value

**Explanation:** display_ties_mode=3 removes entire duplicate rows (not just sort column duplicates) before returning top N.

---

### Example 12: Dynamic Top N from Cell Reference
```
=SORTN(A2:B100, E1, 0, 2, FALSE)
```
**Scenario:** E1 contains a number (user-specified); return that many top items
**Result:** Variable number of rows based on E1's value

**Explanation:** The n parameter can be a cell reference, making the limit user-controllable without editing the formula.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid display_ties_mode (not 0, 1, 2, or 3) | Use only 0, 1, 2, or 3 for the third parameter |
| `#REF!` | Sort column index exceeds range columns | Ensure sort_column numbers match actual column positions |
| `#NUM!` | Negative or zero n value | Use a positive integer for n |
| `Fewer rows than expected` | Data has fewer rows than n | Verify data range has sufficient rows |
| `More rows than expected` | display_ties_mode=1 expanding for ties | Use mode=0 for strict n limit |
| `Wrong column sorting` | Column index confusion | Column indices are 1-based within the range, not sheet columns |
| `Not sorting correctly` | Data type mismatch in sort column | Ensure sort column has consistent data types |
| `#N/A or errors in output` | Source data contains errors | Filter out errors before applying SORTN |

## Use Cases

### [[Sales Leaderboard Dashboard]]

**Scenario:** A sales organization needs a real-time leaderboard showing top performers. The dashboard should display the top 10 salespeople by revenue, update automatically as new sales are logged, and handle ties fairly by showing all people tied at the 10th position.

**Implementation:**
```
Leaderboard structure:
Source data: Sales!A:C (Name, Region, Revenue)

Top 10 leaderboard (with ties):
=SORTN(Sales!A2:C, 10, 1, 3, FALSE)

Top 10 strict (exactly 10):
=SORTN(Sales!A2:C, 10, 0, 3, FALSE)

Top performers by region:
Region filter in E1, then:
=SORTN(FILTER(Sales!A2:C, Sales!B2:B=E1), 10, 0, 3, FALSE)

Weekly comparison (two leaderboards side by side):
This week: =SORTN(FILTER(A:D, A:A>=ThisWeekStart), 10, 0, 4, FALSE)
Last week: =SORTN(FILTER(A:D, A:A>=LastWeekStart, A:A<ThisWeekStart), 10, 0, 4, FALSE)

Add ranking numbers:
=ARRAYFORMULA({SEQUENCE(ROWS(LeaderboardResults)), LeaderboardResults})
```

**Business Application:** Sales leaderboards drive competition and recognition. SORTN provides a clean, automatic solution that requires no manual updates. The tie-handling options ensure fairness—if three people are tied at 10th, you can show all of them rather than arbitrarily excluding some.

**Technical Details:** Use display_ties_mode=1 for fair tie handling, mode=0 for strict limits. SEQUENCE adds rank numbers. Combine with FILTER for regional or time-based leaderboards. Consider adding trend indicators by comparing to historical rankings.

---

### [[Inventory Alert System]]

**Scenario:** A warehouse management system needs to identify items requiring immediate attention: the 10 items with lowest stock (for reordering), the 5 items closest to expiration (for sale or disposal), and the top 10 slowest-moving items (for clearance consideration).

**Implementation:**
```
Inventory alert structure:
Source: Inventory!A:F (SKU, Name, Stock, ReorderPoint, ExpirationDate, LastSold)

Low stock alert (below reorder point, show worst 10):
=SORTN(FILTER(Inventory!A2:F, Inventory!C2:C<Inventory!D2:D), 10, 0, 3, TRUE)

Expiration alert (soonest 5 expiring):
=SORTN(FILTER(Inventory!A2:F, Inventory!E2:E<>"")), 5, 0, 5, TRUE)

Slow movers (longest since last sale):
=SORTN(Inventory!A2:F, 10, 0, 6, TRUE)

Critical items (low stock AND expiring):
=SORTN(FILTER(Inventory!A2:F, (Inventory!C2:C<Inventory!D2:D)*(Inventory!E2:E<TODAY()+30)), 10, 0, 3, TRUE)

Stock-to-reorder ratio:
Add calculated column, then:
=SORTN(WITH_RATIO_COLUMN, 10, 0, RATIO_COLUMN, TRUE)
```

**Business Application:** Inventory management requires focusing on exceptions—items that need action. SORTN efficiently surfaces these exceptions without requiring full inventory review. The alerts enable proactive management rather than reactive firefighting.

**Technical Details:** Combine FILTER to narrow to at-risk items, then SORTN to prioritize within that subset. Multiple alerts can be created on different tabs or areas of a dashboard. Consider adding conditional formatting to highlight the SORTN results.

---

### [[Academic Grade Reporting]]

**Scenario:** A teacher needs to generate reports showing: top 5 students overall, bottom 5 students (for intervention), and top 3 students per subject. The reports should update as grades are entered throughout the term.

**Implementation:**
```
Gradebook structure:
Source: Grades!A:E (Student, Math, Science, English, Average)

Top 5 overall:
=SORTN(Grades!A2:E, 5, 0, 5, FALSE)

Bottom 5 (needs support):
=SORTN(Grades!A2:E, 5, 0, 5, TRUE)

Top 3 in Math:
=SORTN(Grades!A2:E, 3, 0, 2, FALSE)

Top 3 in each subject (requires separate formulas):
Math: =SORTN(Grades!A2:E, 3, 0, 2, FALSE)
Science: =SORTN(Grades!A2:E, 3, 0, 3, FALSE)
English: =SORTN(Grades!A2:E, 3, 0, 4, FALSE)

Most improved (if change column exists):
=SORTN(Grades!A2:F, 5, 0, 6, FALSE)

Honor roll (top 10% - dynamic n):
=SORTN(Grades!A2:E, ROUNDUP(COUNTA(Grades!A2:A)*0.1,0), 0, 5, FALSE)
```

**Business Application:** Education reporting often focuses on recognition (top performers) and intervention (struggling students). SORTN automates these reports, ensuring they're always current. The dynamic n calculation enables percentage-based selections like "top 10%."

**Technical Details:** The dynamic n using COUNTA and percentage creates flexible top-N-percent reports. Multiple SORTN calls with different sort columns create subject-specific leaderboards. Consider privacy implications when displaying these reports.

---

### [[Recent Activity Feed]]

**Scenario:** An application logs user activity to a Google Sheet. The dashboard needs to display the 20 most recent activities, with the newest first, and allow filtering by activity type or user.

**Implementation:**
```
Activity log structure:
Source: ActivityLog!A:E (Timestamp, User, ActivityType, Description, Duration)

Most recent 20 activities:
=SORTN(ActivityLog!A2:E, 20, 0, 1, FALSE)

Recent activities by specific user:
=SORTN(FILTER(ActivityLog!A2:E, ActivityLog!B2:B=SelectedUser), 20, 0, 1, FALSE)

Recent activities by type:
=SORTN(FILTER(ActivityLog!A2:E, ActivityLog!C2:C=SelectedType), 20, 0, 1, FALSE)

Today's activities only:
=SORTN(FILTER(ActivityLog!A2:E, ActivityLog!A2:A>=TODAY()), 50, 0, 1, FALSE)

Longest recent activities:
=SORTN(ActivityLog!A2:E, 10, 0, 5, FALSE)

Most active users (top 10 by activity count):
Use QUERY with GROUP BY and COUNT, then LIMIT
```

**Business Application:** Activity feeds require recency-based sorting with limits. Full activity logs may have thousands of entries, but dashboards need only recent items. SORTN efficiently extracts these without sorting the entire log.

**Technical Details:** Timestamp sorting with FALSE (descending) puts newest first. Combine with FILTER for user/type filtering. For very large logs, consider archiving old entries to maintain performance.

## Platform Differences

SORTN is **exclusively available in Google Sheets** and has no direct equivalent in Microsoft Excel.

### Google Sheets
- **Availability:** All versions since SORTN introduction
- **Tie Handling:** Four built-in modes (0, 1, 2, 3)
- **Multi-Column Sort:** Supports unlimited sort column pairs
- **Efficiency:** Single function combines sort and limit

### Microsoft Excel
- **Native SORTN:** No equivalent function exists
- **Alternatives:**
  - **SORT + TAKE:** Excel 365: `=TAKE(SORT(range, col, order), n)`
  - **SORT + INDEX:** `=INDEX(SORT(range,col,order),SEQUENCE(n),,)`
  - **LARGE/SMALL:** For single-column numeric top/bottom n
  - **Power Query:** Sort and Keep Top Rows transformation
- **Tie Handling:** No built-in tie modes; requires custom logic

### Key Differences Summary

| Feature | Google Sheets (SORTN) | Microsoft Excel |
|---------|----------------------|-----------------|
| Single Function | Yes | No (requires combining functions) |
| Tie Handling Modes | 4 modes built-in | Manual implementation required |
| Multi-Column Sort | Built-in pairs | Nested SORT or separate operations |
| Syntax Simplicity | One function | Multiple functions combined |
| Learning Curve | Lower | Higher |

## Tips and Best Practices

1. **Use display_ties_mode=1 for fair rankings:** When creating competitive leaderboards, mode 1 ensures everyone tied at the cutoff is included. This prevents complaints about arbitrary exclusion.

2. **Remember column indices are relative:** sort_column=2 means the second column in your range, not column B. If your range is D:F, sort_column=2 refers to column E.

3. **Combine with FILTER for conditional Top N:** `SORTN(FILTER(range, condition), n, ...)` is more efficient than filtering SORTN output, and enables category-specific rankings.

4. **Make n dynamic with cell references:** Using a cell for n (`=SORTN(range, A1, ...)`) allows users to adjust how many items appear without editing formulas.

5. **Use mode=2 for unique-value rankings:** When you want the top 5 different values (not 5 rows that might have the same value), display_ties_mode=2 removes sort-column duplicates.

6. **Multi-column sorting breaks ties predictably:** Add secondary sort columns to ensure consistent ordering when primary values tie. This makes results reproducible.

7. **Consider performance with large datasets:** SORTN is efficient, but sorting massive datasets still takes resources. Limit the input range when possible rather than using full-column references.

8. **Test tie-handling behavior:** Different modes produce significantly different results with tied data. Test with sample data containing ties to confirm the behavior matches expectations.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SORT]] | Sorts entire range without limiting | When you need all sorted data, not top N |
| [[LARGE]] | Returns the nth largest value | For single values, not rows of data |
| [[SMALL]] | Returns the nth smallest value | For single bottom values |
| [[FILTER]] | Filters rows based on conditions | For conditional selection before SORTN |

### Commonly Used Together

**[[FILTER]]** - Narrow data before ranking

*Top 5 from a filtered subset:*
```
=SORTN(FILTER(A:C, B:B="Category1"), 5, 0, 3, FALSE)
```
Filter first, then rank the subset.

---

**[[QUERY]]** - Alternative for grouped rankings

*Top values with grouping:*
```
=QUERY(A:C, "SELECT A, MAX(C) GROUP BY A ORDER BY MAX(C) DESC LIMIT 5")
```
When you need aggregation before ranking.

---

**[[SEQUENCE]]** - Add rank numbers

*Create numbered leaderboard:*
```
=ARRAYFORMULA({SEQUENCE(5), SORTN(A:B, 5, 0, 2, FALSE)})
```
Prepends rank numbers 1-5 to the results.

---

**[[ARRAYFORMULA]]** - Combine with calculations

*Rank by calculated value:*
```
=SORTN({A:A, ARRAYFORMULA(B:B*C:C)}, 5, 0, 2, FALSE)
```
Create calculated columns then rank by them.

---

**[[INDEX]]** - Extract specific columns from result

*Get only names from top 5:*
```
=INDEX(SORTN(A:C, 5, 0, 3, FALSE), , 1)
```
Extracts specific columns from SORTN output.

## Official Documentation

- **Google Sheets:** [SORTN function](https://support.google.com/docs/answer/7354624)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2017 | Original implementation |
| Google Sheets | 2019 | Enhanced tie-handling modes |
| Google Sheets | 2021 | Performance improvements |
| Google Sheets | 2023 | Continued refinements |

---

*Last updated: 2026-01-10*
