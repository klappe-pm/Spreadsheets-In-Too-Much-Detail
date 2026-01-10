---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- dynamic-arrays
- sorting
subTopics:
- multi-level-sort
- array-sorting
- custom-sort-order
- data-organization
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sort By Function
- Multi-Column Sort
- Array Sort
- Custom Sort
tags:
- sorting
- dynamic-arrays
- organization
- data-management
- excel-365
---

# SORTBY

## Description

**SORTBY** sorts a range or array based on values in a separate corresponding range or array. Unlike the basic SORT function, SORTBY lets you sort data by columns that aren't even in the output. Sort a list of names by their sales figures, sort products by inventory counts, or sort students by test scores, all while displaying only the columns you want. The sorted results spill automatically, updating whenever the source data changes.

The function truly shines with multi-level sorting. Pass multiple sort-by arrays and their sort orders to create complex hierarchies: sort by department (ascending), then by salary (descending), then by hire date (ascending). Each level breaks ties from the previous level, creating precisely ordered results. This matches what you can do manually with Data > Sort, but as a dynamic formula that never needs re-running.

**SORTBY in Google Sheets:** Google Sheets added SORTBY in 2022, providing full compatibility with Excel's implementation. The syntax and behavior are identical, including multi-level sorting capabilities. Formulas transfer directly between platforms without modification. The only difference is the slightly different keyboard shortcuts for array entry in legacy modes, but with modern dynamic arrays, this is no longer a concern.

**SORTBY vs SORT:** Use SORT when sorting by columns that exist within the output range and you can reference them by position number. Use SORTBY when: (1) the sort column shouldn't appear in output, (2) the sort column is in a completely separate range, or (3) you need to sort by complex calculated arrays. SORTBY offers more flexibility at the cost of slightly more verbose syntax.

## Syntax

> [!f(x)] SORTBY Syntax
>
> ```
> =SORTBY(array, by_array1, [sort_order1], [by_array2, sort_order2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array to sort. This is what gets returned in the result, but the sorting is based on by_array, not this array. |
| `by_array1` | Yes | The range or array to sort by. Must have the same number of rows (or columns for horizontal arrays) as array. |
| `sort_order1` | No | 1 for ascending (A-Z, smallest to largest), -1 for descending (Z-A, largest to smallest). Default is 1 (ascending). |
| `by_array2, sort_order2, ...` | No | Additional arrays and sort orders for multi-level sorting. Each by_array must match dimensions of the first. Up to 126 pairs supported. |

### Return Value

Returns the array sorted according to the by_array values. Original array structure is maintained (same columns in same order). Array spills to adjacent cells automatically.

## Examples

> [!f(x)] SORTBY Examples

### Example 1: Basic Sort by Another Column
```
=SORTBY(A2:A20, B2:B20)
```
**Result:** Column A values sorted by corresponding column B values (ascending)

**Explanation:** Names in A sorted by scores in B. The scores don't appear in output, only the names, but they're in score order. Perfect for ranked lists where ranking criteria shouldn't be displayed.

---

### Example 2: Multiple Column Output Sorted by One Column
```
=SORTBY(A2:C20, D2:D20)
```
**Result:** Columns A, B, C sorted together based on column D values

**Explanation:** Three columns of data sorted by a fourth column. All columns maintain their row relationships. Employee name, department, title sorted by hire date.

---

### Example 3: Descending Sort
```
=SORTBY(A2:B20, C2:C20, -1)
```
**Result:** Sorted highest to lowest based on column C

**Explanation:** The third parameter (-1) sorts descending. Sort sales reps by revenue highest first, students by grades best first, or products by inventory largest first.

---

### Example 4: Multi-Level Sort (Two Levels)
```
=SORTBY(A2:D20, B2:B20, 1, C2:C20, -1)
```
**Result:** Sorted by column B ascending, then by column C descending

**Explanation:** Two-level sort: primary sort by B (ascending), secondary by C (descending). Within each B group, C values are ordered high to low. Like sorting by Department then by Salary descending.

---

### Example 5: Three-Level Sort
```
=SORTBY(A2:E50, B2:B50, 1, C2:C50, 1, D2:D50, -1)
```
**Result:** Three-level hierarchy

**Explanation:** Sorts by B (ascending), then C (ascending), then D (descending). Example: Region, then Store, then Sales (highest first). Each level breaks ties from the previous.

---

### Example 6: Sort by Calculated Values
```
=SORTBY(A2:B20, C2:C20*D2:D20, -1)
```
**Result:** Sorted by the product of columns C and D, descending

**Explanation:** The by_array can be a calculation. Here, sorting by calculated total (quantity * price). The calculated values don't need to exist in a column.

---

### Example 7: Sort by Array Formula Result
```
=SORTBY(A2:A20, LEN(A2:A20), -1)
```
**Result:** Names sorted by length, longest first

**Explanation:** Sort by the length of text. LEN applied to an array returns array of lengths. Creative use for text analysis or finding outliers.

---

### Example 8: Sort by Date with Most Recent First
```
=SORTBY(A2:C100, D2:D100, -1)
```
**Result:** All rows sorted with most recent dates first

**Explanation:** Dates sort naturally as numbers. Descending (-1) puts most recent at top. Common for activity logs, transaction history, or project timelines.

---

### Example 9: Sort Names Alphabetically by Last Name
```
=SORTBY(A2:A20, RIGHT(A2:A20, LEN(A2:A20)-FIND(" ",A2:A20)), 1)
```
**Result:** Full names sorted by last name

**Explanation:** Extracts last name from "First Last" format and sorts by that. Demonstrates sorting by derived/extracted values that don't exist as separate data.

---

### Example 10: Random Sort (Shuffle)
```
=SORTBY(A2:A20, RANDARRAY(ROWS(A2:A20)))
```
**Result:** List in random order (changes on recalculation)

**Explanation:** RANDARRAY generates random numbers for each row. Sorting by random numbers randomizes the list. Useful for random selection, shuffling card games, or randomized testing.

---

### Example 11: Sort by Lookup Result
```
=SORTBY(A2:B20, XLOOKUP(B2:B20, PriorityTable[Code], PriorityTable[Order]), 1)
```
**Result:** Sorted by priority order looked up from another table

**Explanation:** XLOOKUP converts codes to sort order numbers. Custom sort order without changing source data. High-Medium-Low priorities become 1-2-3 for proper ordering.

---

### Example 12: Sort with FILTER
```
=SORTBY(FILTER(A2:D100, C2:C100="Active"), FILTER(B2:B100, C2:C100="Active"), -1)
```
**Result:** Filtered results then sorted

**Explanation:** First filter to Active records only, then sort the filtered results by column B descending. Both FILTER expressions must use same criteria.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | by_array has different row count than array | Ensure all arrays have matching dimensions |
| `#SPILL!` | Cells in spill range aren't empty | Clear the cells where results need to appear |
| `#REF!` | Referenced range is invalid | Verify all range references exist and are accessible |
| `Inconsistent results` | by_array contains mixed data types | Ensure sort column has consistent data types (all numbers or all text) |
| `#CALC!` | Array result too large | Reduce the source data size or filter before sorting |
| `Sort order ignored` | Missing or invalid sort_order value | Use 1 for ascending or -1 for descending only |

## Use Cases

### [[Sales Leaderboard Dashboard]]

**Scenario:** Create a real-time sales leaderboard showing top performers ranked by total sales, but display only names and rankings without revealing actual sales figures.

**Implementation:**
```
=LET(
  names, SalesData[Rep_Name],
  sales, SalesData[Total_Sales],
  sorted_names, SORTBY(names, sales, -1),
  HSTACK(SEQUENCE(ROWS(sorted_names)), sorted_names)
)
```

**Business Application:** Competitive dashboards that motivate without creating uncomfortable specific comparisons. Rankings update automatically as sales data changes.

**Technical Details:** SORTBY reorders names by sales descending. HSTACK adds rank numbers. The sales figures never appear in output, maintaining confidentiality while still ranking.

---

### [[Priority Task Management]]

**Scenario:** Display tasks sorted by custom priority (Critical, High, Medium, Low), then by due date, showing only task name and due date in the output.

**Implementation:**
```
=LET(
  tasks, TaskTable[TaskName],
  dates, TaskTable[DueDate],
  priority, XLOOKUP(TaskTable[Priority], {"Critical";"High";"Medium";"Low"}, {1;2;3;4}),
  SORTBY(HSTACK(tasks, dates), priority, 1, dates, 1)
)
```

**Business Application:** Automated task prioritization that considers both urgency (priority) and time sensitivity (due date). No manual re-sorting needed.

**Technical Details:** XLOOKUP converts text priorities to numeric order. Two-level sort: first by priority number, then by date within each priority level.

---

### [[Inventory Reorder List]]

**Scenario:** Generate a reorder list showing products where stock falls below reorder point, sorted by days of stock remaining (most urgent first).

**Implementation:**
```
=LET(
  products, FILTER(Inventory[Product], Inventory[Stock]<Inventory[ReorderPoint]),
  urgency, FILTER(Inventory[Stock]/Inventory[DailyUsage], Inventory[Stock]<Inventory[ReorderPoint]),
  SORTBY(products, urgency, 1)
)
```

**Business Application:** Purchasing teams see what needs ordering most urgently without calculating days manually. Dynamic updates as inventory changes.

**Technical Details:** Days of stock = Stock/DailyUsage. Lower numbers = more urgent. Filter first, then sort the filtered results by calculated urgency.

---

### [[Student Grade Reports]]

**Scenario:** Generate class rankings by overall GPA, but within each grade level, and show students with their rank within their grade.

**Implementation:**
```
=LET(
  data, HSTACK(Students[Name], Students[Grade], Students[GPA]),
  sorted, SORTBY(data, Students[Grade], 1, Students[GPA], -1),
  sorted
)
```

**Business Application:** Academic reporting where context matters. An 11th grader with 3.5 GPA ranks among 11th graders, not the whole school.

**Technical Details:** First sort by Grade (ascending: 9,10,11,12), then by GPA (descending: highest first within each grade). Add HSTACK with calculated rank per grade for complete rankings.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Multi-level limit:** Up to 126 by_array/sort_order pairs
- **Array behavior:** Automatic spilling
- **Performance:** Optimized for large datasets

### Google Sheets
- **Availability:** Added in 2022
- **Multi-level limit:** Same 126 pair limit
- **Array behavior:** Identical spill behavior
- **Performance:** Comparable to Excel

### Key Differences
Both platforms implement SORTBY identically. Formulas are fully portable between Excel 365/2021 and Google Sheets. The main compatibility consideration is older Excel versions (2019 and earlier) where SORTBY is not available.

### Migration from Older Excel
Before SORTBY, complex multi-column sorting required:
- Manual Data > Sort operations (not formula-based)
- Helper columns with sorting indices
- Complex array formulas with SMALL/LARGE and INDEX/MATCH

SORTBY provides a clean, formula-based solution that updates automatically.

## Tips and Best Practices

1. **Keep by_array dimensions matching:** Every by_array must have the same row count as the main array. Use ROWS() to verify before building complex formulas.

2. **Multi-level sort order:** Parameters alternate: by_array1, order1, by_array2, order2. Don't group all arrays then all orders.

3. **Use calculated by_arrays:** You don't need a physical column to sort by. Create calculated arrays inline: `SORTBY(A:A, LEN(A:A), -1)`.

4. **Combine with FILTER:** Filter first, then sort. Both the main array and by_array need the same filter applied separately.

5. **Random shuffling:** `SORTBY(data, RANDARRAY(ROWS(data)))` randomly orders data. Recalculates on every sheet change.

6. **Custom sort orders:** Use XLOOKUP or SWITCH to convert categorical values to numeric sort order for proper sequencing.

7. **Remember: array is output, by_array is criteria:** What you see in results comes from the first parameter. What determines order comes from subsequent parameters.

8. **Consider SORT for simpler cases:** If sorting by a column that's in the output and you can use column position, SORT has simpler syntax.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SORT]] | Sorts array by column within it | When sort column is part of the output and you can reference by position |
| [[FILTER]] | Returns rows matching criteria | When you need to exclude rows, not order them |
| [[UNIQUE]] | Returns distinct values | When eliminating duplicates matters more than ordering |
| [[RANK]] | Returns rank of value in list | When you need rank numbers rather than reordered data |

### Commonly Used Together

**[[FILTER]]** - Subset data before sorting

*Filter then sort:*
```
=SORTBY(FILTER(Data, Criteria), FILTER(SortColumn, Criteria), -1)
```
Filter must be applied to both arrays with same criteria.

---

**[[UNIQUE]]** - Deduplicate before or after sorting

*Sort unique values:*
```
=SORTBY(UNIQUE(A:A), UNIQUE(A:A), 1)
```
Get unique values in sorted order.

---

**[[HSTACK]]** - Combine columns for output

*Merge columns then sort:*
```
=SORTBY(HSTACK(A:A, B:B, C:C), D:D, -1)
```
Create multi-column output sorted by separate column.

---

**[[XLOOKUP]]** - Create custom sort order

*Convert categories to sort numbers:*
```
=SORTBY(Data, XLOOKUP(Category, {"High";"Med";"Low"}, {1;2;3}))
```
XLOOKUP maps text values to numeric sort order.

---

**[[SEQUENCE]]** - Add row numbers to sorted results

*Numbered sorted list:*
```
=HSTACK(SEQUENCE(ROWS(SORTBY(A:A,B:B))), SORTBY(A:A,B:B))
```
Adds 1,2,3... ranking column to sorted output.

---

**[[LET]]** - Simplify complex SORTBY formulas

*Named intermediate calculations:*
```
=LET(data, A2:C100, score, D2:D100, SORTBY(data, score, -1))
```
Makes formulas more readable and avoids recalculating ranges.

## Official Documentation

- **Microsoft Excel:** [SORTBY function](https://support.microsoft.com/en-us/office/sortby-function-cd2d7a62-1b93-435c-b561-d6a35134f28f)
- **Google Sheets:** [SORTBY function](https://support.google.com/docs/answer/10534042)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2019 (Insider), 2020 (General) | Part of dynamic array functions release |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for web | 2020 | Full feature parity |
| Google Sheets | 2022 | Added to match Excel functionality |
| Excel 2019 | Not available | Must use Data > Sort or helper column methods |

---

*Last updated: 2026-01-10*
