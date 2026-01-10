---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- array-manipulation
- data-transformation
- row-column-swap
- matrix-operations
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Flip Rows Columns
- Rotate Data
- Row to Column
- Column to Row
tags:
- lookup
- reference
- transpose
- array
- matrix
- transformation
---

# TRANSPOSE

## Description

**TRANSPOSE** rotates the orientation of a range or array—rows become columns and columns become rows. A vertical list becomes horizontal; a horizontal header row becomes a vertical label column. `=TRANSPOSE(A1:A5)` converts a 5-row, 1-column range into a 1-row, 5-column array. This fundamental matrix operation is essential for data transformation, report formatting, and working with functions that expect data in a specific orientation.

The transformation follows a simple rule: the element at row R, column C in the original becomes the element at row C, column R in the result. A 3-row by 5-column range becomes a 5-row by 3-column range. The first row of the original becomes the first column of the result. This predictable behavior makes TRANSPOSE reliable for systematic data restructuring.

**In modern Excel (365/2019+) with dynamic arrays, TRANSPOSE spills automatically.** Enter `=TRANSPOSE(A1:C10)` in a single cell, and the result spreads across the necessary cells. In older Excel, TRANSPOSE required array formula entry: select the output range, type the formula, and press Ctrl+Shift+Enter. Google Sheets handles TRANSPOSE natively without special entry methods.

**TRANSPOSE works with formula arrays, not just ranges.** You can transpose the result of other functions: `=TRANSPOSE(SEQUENCE(5))` turns a vertical sequence into horizontal. Combined with dynamic array functions, TRANSPOSE enables powerful data reshaping. However, TRANSPOSE creates a snapshot—it doesn't maintain a live link that updates bidirectionally. Changes to the source update the transposed result, but not vice versa.

## Syntax

> [!f(x)] TRANSPOSE Syntax
>
> ```
> =TRANSPOSE(array)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | The range or array to transpose. Can be a cell range (A1:C10), named range, array constant, or formula that returns an array. Rows and columns are swapped in the result. |

### Return Value

Returns an array with rows and columns exchanged. If the input is M rows by N columns, the output is N rows by M columns. In Excel 365, this spills automatically. In older Excel, requires array formula entry into a pre-selected range of correct dimensions.

## Examples

> [!f(x)] TRANSPOSE Examples

### Example 1: Transpose a Vertical List to Horizontal
```
=TRANSPOSE(A1:A5)
```
**Result:** If A1:A5 contains {1;2;3;4;5} (vertical), result is {1,2,3,4,5} (horizontal)

**Explanation:** A 5-row, 1-column range becomes a 1-row, 5-column range. Each row value becomes a column value. In Excel 365, this spills across 5 columns automatically.

---

### Example 2: Transpose a Horizontal List to Vertical
```
=TRANSPOSE(A1:E1)
```
**Result:** If A1:E1 contains {1,2,3,4,5} (horizontal), result is {1;2;3;4;5} (vertical)

**Explanation:** A 1-row, 5-column range becomes a 5-row, 1-column range. This is the reverse of Example 1—converting horizontal headers to vertical labels.

---

### Example 3: Transpose a Full Table
```
=TRANSPOSE(A1:C10)
```
**Result:** A 10-row by 3-column table becomes a 3-row by 10-column table

**Explanation:** Complete tables transpose with structure preserved. Row headers become column headers. Useful for reformatting data between systems that expect different orientations.

---

### Example 4: Transpose with Headers
```
=TRANSPOSE(A1:D5)
```
**Result:** First column becomes first row (headers), data rotates accordingly

**Explanation:** When your original data has row labels in column A, TRANSPOSE puts those labels in row 1 of the result. Column headers become row labels. The entire structure flips.

---

### Example 5: Transpose an Array Constant
```
=TRANSPOSE({1,2,3;4,5,6})
```
**Result:** {1,4;2,5;3,6} - original 2 rows by 3 columns becomes 3 rows by 2 columns

**Explanation:** Array constants work with TRANSPOSE. The semicolons (row separators) and commas (column separators) swap roles. Useful for creating small reference tables in formulas.

---

### Example 6: Transpose SEQUENCE Result
```
=TRANSPOSE(SEQUENCE(10))
```
**Result:** Horizontal sequence {1,2,3,4,5,6,7,8,9,10}

**Explanation:** SEQUENCE(10) creates a vertical 10x1 array. TRANSPOSE flips it horizontal. Common pattern for creating horizontal number sequences.

---

### Example 7: Transpose UNIQUE Results
```
=TRANSPOSE(UNIQUE(A1:A100))
```
**Result:** Unique values displayed horizontally instead of vertically

**Explanation:** UNIQUE returns a vertical list by default. TRANSPOSE converts it to horizontal, useful for creating horizontal category headers from unique values.

---

### Example 8: Double Transpose (Returns Original)
```
=TRANSPOSE(TRANSPOSE(A1:C5))
```
**Result:** Same as A1:C5

**Explanation:** Transposing twice returns the original orientation. This is rarely useful on its own but confirms TRANSPOSE's predictable behavior. T(T(X)) = X.

---

### Example 9: Transpose for VLOOKUP Table Reorientation
```
=VLOOKUP(SearchValue, TRANSPOSE(DataRange), 2, FALSE)
```
**Result:** Performs VLOOKUP on transposed data

**Explanation:** If your data is arranged horizontally but you need VLOOKUP (which searches vertically), TRANSPOSE flips the data first. However, HLOOKUP might be simpler.

---

### Example 10: Creating Multiplication Table
```
=SEQUENCE(10)*TRANSPOSE(SEQUENCE(10))
```
**Result:** 10x10 multiplication table

**Explanation:** SEQUENCE(10) creates {1;2;...;10} vertical. TRANSPOSE makes {1,2,...,10} horizontal. Multiplying them creates a matrix where each cell is row*column.

---

### Example 11: Transpose Named Range
```
=TRANSPOSE(MonthlyData)
```
**Result:** Transposed version of the named range

**Explanation:** Named ranges work directly with TRANSPOSE. If MonthlyData is 12 rows (months) by 5 columns (metrics), result is 5 rows by 12 columns.

---

### Example 12: Combining TRANSPOSE with FILTER
```
=TRANSPOSE(FILTER(A:B, B:B>100))
```
**Result:** Filtered data displayed horizontally

**Explanation:** FILTER returns rows matching criteria. TRANSPOSE flips the filtered results horizontal. Useful when filtered results should display across columns rather than down rows.

---

### Example 13: Legacy Array Formula Entry (Pre-365)
```
Select G1:K3, type =TRANSPOSE(A1:C5), press Ctrl+Shift+Enter
```
**Result:** Transposed data appears in G1:K3

**Explanation:** In Excel 2019 and earlier without dynamic arrays, you must pre-select the exact output range (3 rows if original had 3 columns, 5 columns if original had 5 rows), then array-enter. Curly braces {} appear around the formula.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | In legacy Excel, output range dimensions don't match transposed dimensions | Select output range matching new dimensions (original rows = new columns, vice versa). |
| `#REF!` | Source range has been deleted | Update the reference to a valid range. |
| `#SPILL!` | (Excel 365) Output range is blocked by existing data | Clear the cells where the transposed array needs to spill. |
| `Partial results` | In legacy Excel, selected output range was too small | Re-enter with correctly sized output selection. |
| `{formula} display` | Legacy array formula showing | This is normal in pre-365 Excel. The braces indicate an array formula. |
| `Data doesn't update` | Expecting bidirectional link | TRANSPOSE is one-way. Source changes update result, but not reverse. For bidirectional, you need two TRANSPOSE formulas. |

## Use Cases

### [[Data Restructuring for Analysis]]

**Scenario:** Transform data between row-oriented and column-oriented formats to match analysis tool requirements or reporting standards.

**Implementation:**
```
Convert time series from rows to columns:
=TRANSPOSE(A2:M2)  -- Monthly data row becomes column

Pivot product attributes:
=TRANSPOSE(ProductAttributes)  -- Features as rows become columns

Reformat survey responses:
=TRANSPOSE(B2:B100)  -- Vertical responses to horizontal analysis row
```

**Business Application:** Data preparation for tools expecting specific orientations, reformatting between database exports (row-based) and financial reports (column-based months), and restructuring survey data for different analysis perspectives.

**Technical Details:** TRANSPOSE is non-destructive—the source data remains unchanged. For permanent restructuring, copy-paste the transposed result as values. Remember that formulas referencing the source may need updating after transposition.

---

### [[Dynamic Report Formatting]]

**Scenario:** Create reports where data orientation is determined dynamically—showing months as columns vs rows, products horizontal vs vertical, or adapting to available space.

**Implementation:**
```
Conditional orientation:
=IF(DisplayHorizontal, TRANSPOSE(Data), Data)

Fit data to available columns:
=IF(COLUMNS(AvailableSpace)>=ROWS(Data), TRANSPOSE(Data), "Not enough space")

Dynamic headers:
=TRANSPOSE(FILTER(CategoryNames, IsSelected))
```

**Business Application:** Flexible dashboards that adapt to display preferences, reports that resize based on output medium (screen vs print), and templates that accommodate varying data volumes.

**Technical Details:** Wrap TRANSPOSE in IF for conditional orientation. Use ROWS and COLUMNS to measure dimensions before transposing. Consider that transposed formulas referencing relative cells may behave unexpectedly—test thoroughly.

---

### [[Matrix Operations]]

**Scenario:** Perform matrix algebra operations that require transposition—matrix multiplication, creating symmetric matrices, or computing matrix products.

**Implementation:**
```
Matrix multiplication A × B^T:
=MMULT(MatrixA, TRANSPOSE(MatrixB))

Create symmetric matrix:
=(Matrix + TRANSPOSE(Matrix))/2

Covariance-related calculations:
=MMULT(TRANSPOSE(DataMatrix), DataMatrix)
```

**Business Application:** Portfolio optimization calculations, statistical correlation matrices, engineering simulations, and any quantitative analysis requiring linear algebra operations.

**Technical Details:** Excel's MMULT requires compatible matrix dimensions. For A(m×n) × B^T, B must be (p×n) so B^T is (n×p), and result is (m×p). TRANSPOSE is essential for many matrix algebra operations.

---

### [[Array Formula Support]]

**Scenario:** Use TRANSPOSE within larger array formulas to reshape intermediate results, enabling complex calculations that require data in specific orientations.

**Implementation:**
```
Horizontal running totals:
=SCAN(0, TRANSPOSE(Values), LAMBDA(a,b, a+b))

Cross-product array:
=SEQUENCE(N)*TRANSPOSE(SEQUENCE(M))

Reshape SEQUENCE output:
=WRAPROWS(SEQUENCE(12), 4)  -- Alternative to TRANSPOSE for specific patterns
```

**Business Application:** Financial modeling requiring intermediate array manipulations, simulation models processing matrix data, and advanced analytics with custom calculation patterns.

**Technical Details:** In Excel 365, TRANSPOSE integrates seamlessly with LAMBDA, SCAN, REDUCE, and other modern array functions. The combination enables sophisticated data transformations previously requiring VBA.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions
- **Dynamic arrays (365/2019+):** Automatic spilling—just enter formula in one cell
- **Legacy entry:** Pre-365 requires Ctrl+Shift+Enter with pre-selected output range
- **With other arrays:** Full integration with SEQUENCE, FILTER, UNIQUE, etc.
- **Performance:** Efficient for typical ranges; very large arrays may slow calculation

### Google Sheets

- **Availability:** All versions
- **Array handling:** Native support—no special entry required
- **ARRAYFORMULA:** Works within ARRAYFORMULA context
- **With other functions:** Full compatibility with Sheets array functions
- **Performance:** Handles large arrays well

### Key Differences

| Feature | Excel (365) | Excel (Legacy) | Google Sheets |
|---------|-------------|----------------|---------------|
| Entry method | Normal | Ctrl+Shift+Enter | Normal |
| Auto-spill | Yes | No | Yes |
| Pre-select range | No | Yes (exact size) | No |
| Dynamic array functions | Full support | Limited | Full support |
| Maximum size | Large | 65,536 cells | Large |

## Tips and Best Practices

1. **In Excel 365, just enter the formula:** No need for Ctrl+Shift+Enter or pre-selecting ranges. Type `=TRANSPOSE(range)` and press Enter—results spill automatically.

2. **Clear the spill range:** In Excel 365, if you get #SPILL! error, clear any data in cells where the transposed array needs to appear.

3. **Remember the dimension swap:** If source is 5 rows × 3 columns, result is 3 rows × 5 columns. Plan your output location accordingly.

4. **TRANSPOSE is one-directional:** Changes to source update the transposed result, but you cannot edit the transposed cells directly (they show {=TRANSPOSE(...)}). For editable copies, paste as values.

5. **Use with modern array functions:** `=TRANSPOSE(UNIQUE(...))`, `=TRANSPOSE(FILTER(...))`, `=TRANSPOSE(SORT(...))` combine powerfully in Excel 365 and Sheets.

6. **For bidirectional sync, use two formulas:** If you need changes in either location to reflect in the other, set up TRANSPOSE formulas in both places pointing to each other (though this can cause circular reference issues).

7. **Legacy Excel: count dimensions carefully:** When selecting the output range for Ctrl+Shift+Enter, count source rows (becomes new columns) and source columns (becomes new rows). Wrong dimensions truncate or show errors.

8. **TRANSPOSE preserves cell formatting in paste, not in formula:** The TRANSPOSE function doesn't carry over number formats, colors, or borders. Only values and formulas transfer.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WRAPROWS]] | Wraps a row array into rows of specified width | For specific row-based reshaping patterns (Excel 365) |
| [[WRAPCOLS]] | Wraps a column array into columns of specified height | For specific column-based reshaping (Excel 365) |
| [[TOROW]] | Converts array to single row | When you need everything in one row (Excel 365) |
| [[TOCOL]] | Converts array to single column | When you need everything in one column (Excel 365) |

### Commonly Used Together

**[[SEQUENCE]]** - Generate number sequences

*Combined with TRANSPOSE for horizontal sequences:*
```
=TRANSPOSE(SEQUENCE(12))
```
Creates a horizontal month number sequence instead of vertical.

---

**[[UNIQUE]]** - Extract unique values

*Combined with TRANSPOSE for horizontal unique list:*
```
=TRANSPOSE(UNIQUE(A:A))
```
Displays unique values as a horizontal row instead of vertical column.

---

**[[FILTER]]** - Filter array by criteria

*Combined with TRANSPOSE for horizontal filtered results:*
```
=TRANSPOSE(FILTER(Data, Criteria))
```
Shows filtered rows as columns.

---

**[[SORT]]** - Sort array values

*Combined with TRANSPOSE for sorted horizontal data:*
```
=TRANSPOSE(SORT(Data, SortColumn, Order))
```
Sorted data displayed horizontally.

---

**[[MMULT]]** - Matrix multiplication

*Combined with TRANSPOSE for matrix operations:*
```
=MMULT(A, TRANSPOSE(B))
```
Transposes B before matrix multiplication with A.

---

**[[SUMPRODUCT]]** - Sum products of arrays

*Combined with TRANSPOSE for cross-array calculations:*
```
=SUMPRODUCT(RowArray * TRANSPOSE(ColArray))
```
Creates products of row elements with column elements.

## Official Documentation

- **Microsoft Excel:** [TRANSPOSE function](https://support.microsoft.com/en-us/office/transpose-function-ed039415-ed8a-4a81-93e9-4b6dfac76027)
- **Google Sheets:** [TRANSPOSE function](https://support.google.com/docs/answer/3094262)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 (1997) | Required array formula entry (Ctrl+Shift+Enter) |
| Excel 2007 | Unchanged | Same array formula behavior |
| Excel 365/2019 | Dynamic Arrays | Automatic spilling without special entry |
| Google Sheets | Original launch (2006) | Native array support from start |

---

*Last updated: 2026-01-10*
