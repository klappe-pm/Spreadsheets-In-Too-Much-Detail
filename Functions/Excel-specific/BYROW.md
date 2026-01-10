---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- lambda-functions
- array-functions
subTopics:
- row-processing
- functional-programming
- array-iteration
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- By Row
- Row Iterator
- Row Lambda
tags:
- excel-only
- lambda
- arrays
- dynamic-arrays
- functional
---

# BYROW

## Description

**BYROW** applies a LAMBDA function to each row in an array and returns a vertical array of results. It is the row-oriented counterpart to BYCOL. You provide a multi-row range and a function, and BYROW runs that function on each row independently, stacking the results into a single column. This eliminates the need for helper columns or dragging formulas down.

BYROW revolutionizes row-by-row calculations by making them dynamic. Instead of writing a formula and copying it down hundreds of rows, one BYROW formula handles all rows and automatically expands when data is added. Need to calculate the sum of columns B through F for each row? `=BYROW(B:F, LAMBDA(row, SUM(row)))` does it in one formula.

**Critical limitation:** The LAMBDA function must return a single value per row. If your lambda returns an array (like a filtered subset), BYROW only takes the first element. This is the most common point of confusion. Also, BYROW cannot skip rows or process them conditionally - it processes every row in the input array sequentially.

**Platform note:** BYROW is exclusively available in Microsoft 365 Excel and Excel 2024+. It depends on LAMBDA, also an Excel-exclusive feature. Google Sheets has its own BYROW with slightly different behavior. This function does not work in Excel 2019 or earlier, LibreOffice Calc, or Apple Numbers.

## Syntax

> [!f(x)] BYROW Syntax
>
> ```
> =BYROW(array, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The array or range to process row by row. Can be a cell range (e.g., A1:D10), an array constant, or a formula that returns an array. |
| `lambda` | Yes | A LAMBDA function that takes one parameter (a row array) and returns a single value. The lambda is called once for each row in the array. |

### Return Value

Returns a vertical (single-column) array where each element is the result of applying the lambda function to the corresponding row from the input array. The number of elements equals the number of rows in the input array.

## Examples

> [!f(x)] BYROW Examples

### Example 1: Sum Each Row
```
=BYROW(B2:F100, LAMBDA(row, SUM(row)))
```
**Result:** Vertical array of 99 row sums

**Explanation:** The foundational BYROW pattern. Each row is passed to the lambda as an array, summed, and the result placed in the output column. This replaces having a SUM formula in every row.

---

### Example 2: Maximum Value Per Row
```
=BYROW(A1:E50, LAMBDA(r, MAX(r)))
```
**Result:** `{95; 88; 100; 92; ...}` (maximum from each row, vertical array)

**Explanation:** Finds the highest value across columns A through E for each of 50 rows. The result spills downward, one max value per source row.

---

### Example 3: Count Values Meeting Criteria
```
=BYROW(Scores, LAMBDA(r, COUNTIF(r, ">=60")))
```
**Result:** Count of passing scores (>=60) in each row

**Explanation:** COUNTIF within the lambda counts how many columns in each row meet the criteria. Perfect for gradebooks counting passed subjects.

---

### Example 4: Concatenate Row Values
```
=BYROW(A1:C10, LAMBDA(r, TEXTJOIN(" - ", TRUE, r)))
```
**Result:** `{"Apple - Red - Fruit"; "Carrot - Orange - Vegetable"; ...}`

**Explanation:** TEXTJOIN inside lambda concatenates each row's cells with a delimiter. Creates readable combined fields without helper columns.

---

### Example 5: Check for Complete Rows
```
=BYROW(DataRange, LAMBDA(row, COUNTA(row)=COLUMNS(DataRange)))
```
**Result:** `{TRUE; FALSE; TRUE; TRUE; FALSE; ...}` (which rows are fully populated)

**Explanation:** Returns TRUE if a row has no empty cells. COUNTA counts non-empty cells; comparing to column count identifies complete records.

---

### Example 6: Calculate Weighted Average Per Row
```
=BYROW(Values, LAMBDA(v, SUMPRODUCT(v, Weights)/SUM(Weights)))
```
**Result:** Weighted average for each row using shared weights

**Explanation:** Each row of values is multiplied by a weight array (which could be a row or named range) and averaged. This enables portfolio-style weighted calculations.

---

### Example 7: Find Position of Maximum
```
=BYROW(A2:E100, LAMBDA(r, MATCH(MAX(r), r, 0)))
```
**Result:** Column position of the maximum value in each row

**Explanation:** MATCH finds where the MAX value appears within each row. Returns 1-5 indicating which column has the highest value.

---

### Example 8: Conditional Row Calculation
```
=BYROW(A1:D50, LAMBDA(r, IF(INDEX(r,1)="Sales", SUM(r), AVERAGE(r))))
```
**Result:** SUM if first column is "Sales", otherwise AVERAGE

**Explanation:** The lambda can include conditional logic. INDEX extracts the first element to determine which calculation to apply.

---

### Example 9: Row-wise Standard Deviation
```
=BYROW(Measurements, LAMBDA(m, STDEV.S(m)))
```
**Result:** Sample standard deviation across columns for each row

**Explanation:** Calculates variability within each row. Useful for quality control where each row represents repeated measurements of one item.

---

### Example 10: Rank Within Row
```
=BYROW(Scores, LAMBDA(s, RANK(INDEX(s,1), s)))
```
**Result:** Rank of first column's value within each row

**Explanation:** Shows how the first column ranks among all columns in that row. Useful for competitive comparisons.

---

### Example 11: Create Row Hash/Identifier
```
=BYROW(A2:D100, LAMBDA(r, TEXTJOIN("|", TRUE, r)))
```
**Result:** `{"John|Sales|50000|Active"; "Jane|Marketing|55000|Active"; ...}`

**Explanation:** Creates a unique text identifier for each row by joining all values. Useful for duplicate detection or creating lookup keys.

---

### Example 12: Nested Row and Column Processing
```
=BYROW(A1:D10, LAMBDA(r, SUM(MAP(r, LAMBDA(x, IF(x>0, x, 0))))))
```
**Result:** Sum of positive values only in each row

**Explanation:** MAP processes each cell within the row (zeroing negatives), then SUM totals the row. Demonstrates nesting lambda functions.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Lambda doesn't return a single value per row | Ensure your lambda reduces each row to one value |
| `#NAME?` | LAMBDA or BYROW not recognized | Requires Excel 365/2024+; not available in earlier versions |
| `#CALC!` | Calculation error within the lambda | Test lambda on a single row first: `=LAMBDA(r, SUM(r))(A1:E1)` |
| `#SPILL!` | No room for the vertical result array | Ensure enough empty cells below the formula cell |
| `#REF!` | Lambda references invalid cells | Check that any fixed references inside lambda are valid |

## Use Cases

### [[Dynamic Row Totals Without Helper Columns]]

**Scenario:** Calculate row totals for a data table that frequently gains new rows, without maintaining a formula column.

**Implementation:**
```
=BYROW(Table1[Jan]:Table1[Dec], LAMBDA(r, SUM(r)))
```

**Business Application:** Financial reports with monthly columns need row totals showing annual amounts. BYROW handles new rows automatically as they're added to the table.

**Technical Details:** When the source is a structured table, results stay synchronized even as rows are inserted. The spill range expands automatically.

---

### [[Row-Level Data Validation]]

**Scenario:** Validate each row of imported data has complete required fields.

**Implementation:**
```
=BYROW(ImportData, LAMBDA(row,
    IF(AND(
        INDEX(row,1)<>"",
        INDEX(row,2)<>"",
        ISNUMBER(INDEX(row,3))
    ), "Valid", "Missing Required Data")
))
```

**Business Application:** Before processing imports, identify which rows have validation issues. This creates a status column that flags problematic records for review.

**Technical Details:** INDEX with position numbers extracts specific columns from each row for validation. The AND function combines multiple checks.

---

### [[Custom Aggregation Logic]]

**Scenario:** Calculate a custom metric that combines multiple columns with business-specific logic.

**Implementation:**
```
=BYROW(Metrics, LAMBDA(r,
    INDEX(r,1) * 0.4 +
    INDEX(r,2) * 0.3 +
    INDEX(r,3) * 0.2 +
    INDEX(r,4) * 0.1
))
```

**Business Application:** Weighted scoring models (employee reviews, lead scoring, risk assessment) require custom calculations that weight different factors. BYROW applies the formula consistently to all rows.

**Technical Details:** INDEX extracts each column value by position. The weights can be hardcoded or referenced from cells for adjustability.

## Platform Differences

### Microsoft Excel (365/2024+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Complex lambdas | Full support |
| Nested with BYCOL | Full support |
| Named LAMBDA functions | Full support |

### Google Sheets

| Feature | Support |
|---------|---------|
| BYROW | Available (different syntax) |
| Syntax | `=BYROW(array, LAMBDA(row, expression))` |
| Behavior | Similar but may have performance differences |

Google Sheets added BYROW in 2022. The syntax is nearly identical to Excel, making formulas relatively portable between platforms. Test thoroughly when migrating.

### Other Platforms

BYROW in the Excel form is exclusive to Microsoft 365 and Excel 2024+. Availability elsewhere:
- Google Sheets: Available with similar syntax
- LibreOffice Calc: Not available
- Apple Numbers: Not available
- Excel 2019 and earlier: Not available

## Tips and Best Practices

1. **Return single values from lambda:** The most common mistake is expecting BYROW to preserve arrays. Your lambda should reduce each row to one result.

2. **Use INDEX to access specific columns:** Inside the lambda, `INDEX(row, 3)` gets the third column. This is how you work with individual cells within the row.

3. **Test lambda separately:** Before using BYROW, verify your lambda works: `=LAMBDA(r, your_expression)(A1:E1)`. Fix issues before scaling up.

4. **Consider performance for large datasets:** BYROW calls the lambda once per row. For 10,000 rows, that's 10,000 lambda executions. Complex lambdas may slow down large workbooks.

5. **Combine with FILTER for conditional results:** To only process certain rows, FILTER first, then BYROW: `=BYROW(FILTER(Data, Criteria), LAMBDA(r, ...))`.

6. **Use BYCOL for column-wise operations:** If you need to aggregate across all rows of each column (like column totals), BYCOL is the counterpart function.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BYCOL]] | Applies lambda to each column | When you need column-by-column processing |
| [[MAP]] | Applies lambda to each element | When processing individual cells, not entire rows |
| [[REDUCE]] | Accumulates values across array | When you need a single result from all rows |
| [[SCAN]] | Like REDUCE but returns intermediate results | When you need running calculations row by row |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the row processing logic

*Every BYROW needs a LAMBDA:*
```
=BYROW(Data, LAMBDA(r, PRODUCT(r)))
```
The lambda defines what happens to each row.

---

**[[INDEX]]** - Access specific columns within each row

*Extract and calculate from specific positions:*
```
=BYROW(Sales, LAMBDA(r, INDEX(r,3) * INDEX(r,4)))
```
INDEX retrieves column values by position number.

---

**[[FILTER]]** - Pre-filter rows before processing

*Process only active records:*
```
=BYROW(FILTER(Data, Status="Active"), LAMBDA(r, SUM(r)))
```
FILTER selects which rows to process; BYROW handles the calculation.

---

**[[VSTACK]]** - Combine BYROW results with other arrays

*Add header to BYROW results:*
```
=VSTACK("Total", BYROW(A2:E10, LAMBDA(r, SUM(r))))
```
Creates a complete column with header and calculated values.

## Official Documentation

- **Microsoft Excel:** [BYROW function](https://support.microsoft.com/en-us/office/byrow-function-2e04c677-78c8-4e6b-8c10-a4602f2602bb)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | March 2022 | Released with LAMBDA helper functions |
| Excel 2024 | 2024 | Included in perpetual license version |
| Excel Online | 2022 | Full support in web version |
| Google Sheets | 2022 | Similar implementation added |
| Earlier Excel versions | Not available | Requires LAMBDA support |

---

*Last updated: 2026-01-10*
