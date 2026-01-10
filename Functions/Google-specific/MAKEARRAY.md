---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - lambda-functions
  - array-generation
subTopics:
  - programmatic-arrays
  - matrix-creation
  - dynamic-data
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - make array
  - array generator
  - matrix builder
tags:
  - google-sheets-only
  - lambda
  - arrays
  - matrix
  - generation
---

# MAKEARRAY

## Description

**MAKEARRAY** is a powerful lambda-based function in Google Sheets that generates an array of specified dimensions by applying a custom function to each cell position. Unlike functions that transform existing data, MAKEARRAY creates arrays from scratch based on row and column indices. You specify the number of rows, number of columns, and a LAMBDA function that receives the current row index and column index as parameters, then returns the value that should appear at that position. This enables programmatic generation of grids, matrices, lookup tables, patterns, and any other structured data where cell values depend on their position within the array.

The primary use cases for MAKEARRAY include creating multiplication tables, distance matrices, pattern-based data (like chessboard patterns), identity matrices, and dynamically sized grids that adapt to changing requirements. For example, creating a 12x12 multiplication table is as simple as `=MAKEARRAY(12, 12, LAMBDA(row, col, row*col))`. MAKEARRAY is particularly valuable when you need to generate test data, create mathematical matrices for calculations, build reference tables programmatically, or produce any grid where the value at each position can be computed from its coordinates. The function eliminates the need to manually enter repetitive patterns or create complex nested formulas to build structured arrays.

There are several important gotchas when working with MAKEARRAY. First, the LAMBDA function receives 1-based indices, not 0-based; the first row is index 1, not 0. Second, the LAMBDA must return a single value for each cell position; returning arrays or leaving values undefined will cause errors. Third, MAKEARRAY requires empty cells below and to the right of the formula to spill its output; existing data in those cells causes #REF! errors. Fourth, performance degrades with very large dimensions; generating a 1000x1000 array means the LAMBDA is called one million times, which can be slow. Fifth, the generated array is static for a given recalculation; values are computed once and do not inherently update unless the formula recalculates.

From a platform perspective, MAKEARRAY is available in both Google Sheets and Microsoft Excel 365. The syntax and functionality are identical between platforms. MAKEARRAY was introduced as part of the LAMBDA function family in 2022. For users of older Excel versions without LAMBDA support, MAKEARRAY is not available, and generating arrays programmatically would require helper columns, manual data entry, or VBA macros. When building spreadsheets for cross-platform use, MAKEARRAY formulas will work in both modern Excel and Google Sheets without modification.

## Syntax

> [!f(x)] MAKEARRAY Syntax
>
> ```
> =MAKEARRAY(rows, columns, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rows` | Yes | The number of rows in the generated array. Must be a positive integer. Can be a literal number, cell reference, or formula that evaluates to a positive integer. |
| `columns` | Yes | The number of columns in the generated array. Must be a positive integer. Can be a literal number, cell reference, or formula that evaluates to a positive integer. |
| `lambda` | Yes | A LAMBDA function with exactly two parameters that define what value to place at each position. The first parameter receives the row index (1-based), the second receives the column index (1-based). The LAMBDA must return a single value for each cell. Syntax: `LAMBDA(row_index, col_index, expression_returning_cell_value)`. |

### Return Value

Returns an array with the specified number of rows and columns. Each cell in the array contains the value returned by the LAMBDA function when called with that cell's row and column indices. The array spills automatically into adjacent cells.

## Examples

> [!f(x)] MAKEARRAY Examples

### Example 1: Simple Multiplication Table
```
=MAKEARRAY(10, 10, LAMBDA(r, c, r * c))
```
**Result:** A 10x10 multiplication table where each cell contains the product of its row and column numbers

**Explanation:** The LAMBDA receives row index `r` and column index `c`, returning their product. Cell at row 3, column 4 contains 12 (3 x 4). This is the classic MAKEARRAY demonstration.

---

### Example 2: Addition Table
```
=MAKEARRAY(10, 10, LAMBDA(row, col, row + col))
```
**Result:** A 10x10 addition table where each cell is the sum of row and column indices

**Explanation:** Simple arithmetic on indices. Cell at position (5, 7) contains 12. Useful for teaching or reference tables.

---

### Example 3: Identity Matrix
```
=MAKEARRAY(5, 5, LAMBDA(r, c, IF(r = c, 1, 0)))
```
**Result:** A 5x5 identity matrix with 1s on the diagonal and 0s elsewhere

**Explanation:** The IF condition checks if row equals column (diagonal). Identity matrices are fundamental in linear algebra and used in matrix operations.

---

### Example 4: Sequential Numbers (Row-Major)
```
=MAKEARRAY(5, 4, LAMBDA(r, c, (r - 1) * 4 + c))
```
**Result:** Numbers 1-20 arranged in a 5x4 grid, reading left-to-right, top-to-bottom

**Explanation:** Calculates sequential position based on row and column. Row 1 has 1-4, row 2 has 5-8, etc. Useful for creating numbered grids.

---

### Example 5: Chessboard Pattern
```
=MAKEARRAY(8, 8, LAMBDA(r, c, IF(MOD(r + c, 2) = 0, "W", "B")))
```
**Result:** An 8x8 grid with alternating "W" and "B" values like a chessboard

**Explanation:** MOD of the sum of indices determines alternating pattern. Even sum = "W" (white), odd sum = "B" (black).

---

### Example 6: Distance Matrix
```
=MAKEARRAY(5, 5, LAMBDA(r, c, ABS(r - c)))
```
**Result:** A 5x5 matrix showing "distance" from the diagonal

**Explanation:** Cells on the diagonal are 0, adjacent cells are 1, etc. Useful for creating distance or proximity matrices.

---

### Example 7: Powers Table
```
=MAKEARRAY(5, 6, LAMBDA(r, c, r^c))
```
**Result:** A table where each cell is row number raised to column number power

**Explanation:** Row 2 shows 2^1, 2^2, 2^3, etc. Row 3 shows 3^1, 3^2, 3^3, etc. Useful for exponential reference tables.

---

### Example 8: Triangular Matrix (Lower)
```
=MAKEARRAY(5, 5, LAMBDA(r, c, IF(c <= r, 1, 0)))
```
**Result:** A lower triangular matrix with 1s on and below the diagonal

**Explanation:** Values appear where column index is less than or equal to row index. Upper portion (where c > r) is zeros.

---

### Example 9: Triangular Matrix (Upper)
```
=MAKEARRAY(5, 5, LAMBDA(r, c, IF(r <= c, 1, 0)))
```
**Result:** An upper triangular matrix with 1s on and above the diagonal

**Explanation:** Values appear where row index is less than or equal to column index. Lower portion is zeros.

---

### Example 10: Row Labels and Column Values Combined
```
=MAKEARRAY(4, 5, LAMBDA(r, c, IF(c = 1, "Row " & r, r * 10 + c)))
```
**Result:** A grid where column 1 contains row labels, other columns contain calculated values

**Explanation:** First column is treated as labels ("Row 1", "Row 2", etc.), remaining columns have computed values. Demonstrates conditional logic within MAKEARRAY.

---

### Example 11: Random Number Grid
```
=MAKEARRAY(5, 5, LAMBDA(r, c, RANDBETWEEN(1, 100)))
```
**Result:** A 5x5 grid of random numbers between 1 and 100

**Explanation:** Each cell gets a random value. Note that these will recalculate whenever the sheet recalculates. The row and column parameters are not used but are required by MAKEARRAY.

---

### Example 12: Coordinate Pairs
```
=MAKEARRAY(3, 3, LAMBDA(r, c, "(" & r & "," & c & ")"))
```
**Result:** A 3x3 grid where each cell shows its coordinates like "(1,1)", "(1,2)", etc.

**Explanation:** Concatenates row and column indices into a coordinate string. Useful for understanding array positions or creating labeled grids.

---

### Example 13: Dynamic Size Based on Data
```
=MAKEARRAY(COUNTA(A:A), COUNTA(1:1), LAMBDA(r, c, r + c))
```
**Result:** Array sized based on data extent in column A (rows) and row 1 (columns)

**Explanation:** Uses COUNTA to dynamically determine dimensions. The array automatically resizes as data is added or removed.

---

### Example 14: Calendar Month Grid
```
=MAKEARRAY(6, 7, LAMBDA(r, c, LET(day, (r-1)*7+c-3, IF(AND(day>=1, day<=31), day, ""))))
```
**Result:** A 6x7 calendar grid layout (adjusting -3 for the starting weekday)

**Explanation:** Creates a calendar-style grid where valid day numbers appear in their positions. Empty strings for invalid days. Adjust offset based on month's starting day.

---

### Example 15: Color Gradient Values
```
=MAKEARRAY(5, 10, LAMBDA(r, c, ROUND(c/10*255)))
```
**Result:** A gradient of values from 0 to 255 across columns, repeated for each row

**Explanation:** Creates values that could represent color intensity in a gradient. Column position determines the value, creating a horizontal gradient effect.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Rows or columns argument is zero, negative, or non-integer | Ensure both dimension arguments are positive integers. Use INT() if your formula might return decimals. |
| `#NAME?` | LAMBDA syntax is incorrect or parameter names are invalid | Check LAMBDA structure: must have exactly two parameters for row and column indices, followed by the expression. |
| `#REF!` | Output array would spill into cells with existing data | Clear cells below and to the right of the formula. MAKEARRAY needs empty space for the generated array. |
| `#VALUE!` | LAMBDA returns an array instead of a single value | The LAMBDA must return one value per cell. If your expression returns multiple values, wrap in INDEX or adjust logic. |
| `Circular reference` | LAMBDA expression references the cell containing MAKEARRAY | The generating formula cannot reference its own output cells. Restructure to avoid self-reference. |
| `Performance issues` | Very large dimensions cause slow calculation | Reduce array size if possible. Each cell requires a LAMBDA evaluation; 1000x1000 = 1 million evaluations. |
| `#ERROR!` | Missing required parameter in LAMBDA or MAKEARRAY | Verify all three MAKEARRAY parameters are provided and LAMBDA has exactly two parameters plus expression. |

## Use Cases

### [[Mathematical Modeling: Matrix Operations]]

**Scenario:** A data scientist needs to create various mathematical matrices (identity, diagonal, triangular, symmetric) for linear algebra calculations without manually entering values.

**Implementation:**
```
=MAKEARRAY(N, N, LAMBDA(r, c, IF(r = c, 1, 0)))
=MAKEARRAY(N, N, LAMBDA(r, c, IF(c <= r, r - c + 1, 0)))
=MAKEARRAY(N, N, LAMBDA(r, c, MIN(r, c)))
```
Where N is the matrix dimension

**Business Application:** Enables programmatic creation of matrices for statistical analysis, optimization problems, and machine learning preprocessing. Matrices can be dynamically sized and automatically regenerated when dimensions change, supporting iterative model development.

**Technical Details:** Identity matrices use equality check. Lower triangular matrices check column <= row. Symmetric matrices use MIN or MAX of indices. These patterns can be combined with MMULT for matrix operations or FILTER for conditional selection.

---

### [[Education: Multiplication and Reference Tables]]

**Scenario:** A math teacher wants to create various reference tables (multiplication, addition, powers) for students that automatically adjust size based on the lesson requirements.

**Implementation:**
```
=MAKEARRAY(A1, A1, LAMBDA(r, c, r * c))
=MAKEARRAY(12, 12, LAMBDA(r, c, r^c))
=MAKEARRAY(10, 10, LAMBDA(r, c, GCD(r, c)))
```
Where A1 contains the desired table size

**Business Application:** Creates customizable educational materials. Tables can be resized by changing a single cell, enabling different complexity levels for different student groups. The programmatic approach ensures accuracy and saves preparation time.

**Technical Details:** Use LAMBDA parameters directly for multiplication tables. Add headers with VSTACK and HSTACK if needed. GCD (Greatest Common Divisor) tables demonstrate number theory concepts. Consider adding conditional formatting to highlight patterns.

---

### [[Data Analysis: Distance and Similarity Matrices]]

**Scenario:** An analyst needs to create distance matrices showing relationships between items, such as geographic distances between locations or similarity scores between products.

**Implementation:**
```
=MAKEARRAY(ROWS(Locations), ROWS(Locations), LAMBDA(r, c,
  SQRT((INDEX(Locations, r, 1) - INDEX(Locations, c, 1))^2 +
       (INDEX(Locations, r, 2) - INDEX(Locations, c, 2))^2)))
```
Where Locations is a range with X,Y coordinates

**Business Application:** Generates distance matrices for logistics optimization, clustering analysis, or recommendation systems. The matrix can drive route planning, identify nearby locations, or support similarity-based grouping algorithms.

**Technical Details:** The LAMBDA accesses external data (Locations) while using indices to determine which pair to compare. Euclidean distance formula calculates distance. For geographic coordinates, consider using the Haversine formula for accuracy.

---

### [[Testing: Sample Data Generation]]

**Scenario:** A developer needs to generate test datasets with specific patterns, sizes, and characteristics for validating spreadsheet formulas or demonstrating functionality.

**Implementation:**
```
=MAKEARRAY(100, 5, LAMBDA(r, c,
  CHOOSE(c,
    "ID-" & TEXT(r, "000"),
    RANDBETWEEN(18, 65),
    CHOOSE(RANDBETWEEN(1,3), "A", "B", "C"),
    ROUND(RAND() * 10000, 2),
    DATE(2024, RANDBETWEEN(1,12), RANDBETWEEN(1,28))
  )))
```

**Business Application:** Creates realistic test data with IDs, ages, categories, amounts, and dates. Useful for testing formulas, demonstrating dashboards, or populating prototype applications. Data can be customized for different testing scenarios.

**Technical Details:** CHOOSE selects different generation logic based on column index. Column 1 creates sequential IDs, column 2 random ages, etc. RANDBETWEEN and RAND create variability. Adjust parameters to match real data characteristics.

## Platform Differences

### Microsoft Excel

- **Availability:** MAKEARRAY is available in Excel 365 (Microsoft 365 subscription versions)
- **Syntax:** Identical to Google Sheets
- **Behavior:** Same functionality; generates arrays based on LAMBDA function
- **Not available in:** Excel 2019, Excel 2016, Excel 2021, or earlier versions
- **Alternative for older Excel:** Use INDEX/MATCH with helper columns, VBA, or manual data entry

### Google Sheets

- **Availability:** Available in all Google Sheets (introduced 2022)
- **Specific Behavior:** This documentation focuses on Google Sheets behavior
- **Performance:** May vary compared to Excel for very large arrays
- **Integration:** Works with all Google Sheets functions including QUERY and other LAMBDA functions

### Key Differences

| Feature | Google Sheets | Excel 365 |
|---------|---------------|-----------|
| Availability | All users | Microsoft 365 subscribers only |
| Syntax | Identical | Identical |
| Index base | 1-based | 1-based |
| Maximum dimensions | Limited by cell limits | Limited by cell limits |
| Performance | Good | Generally better for large arrays |
| Spill behavior | Automatic | Automatic |

## Tips and Best Practices

1. **Remember 1-Based Indexing:** Row and column indices start at 1, not 0. The first cell is (1, 1). Adjust calculations accordingly, especially when translating from 0-based programming logic.

2. **Unused Parameters Are Still Required:** Even if your LAMBDA does not use row or column index (like generating random values), you must still include both parameters. Use underscore or descriptive names: `LAMBDA(_, _, RAND())`.

3. **Test with Small Dimensions First:** Start with a 3x3 or 5x5 array to verify your LAMBDA logic before scaling to larger dimensions. Debugging is much easier with small outputs.

4. **Use LET for Complex Logic:** When your LAMBDA expression is complex, use LET within the LAMBDA to name intermediate calculations: `LAMBDA(r, c, LET(x, r+c, y, r*c, x/y))`.

5. **Combine with External Data:** Your LAMBDA can reference ranges outside the generated array using INDEX: `LAMBDA(r, c, INDEX(DataRange, r, c) * 2)` to transform existing data.

6. **Add Headers with VSTACK/HSTACK:** MAKEARRAY creates data only; add headers by stacking: `=VSTACK(HeaderRow, MAKEARRAY(...))`.

7. **Consider Performance:** Each cell requires a LAMBDA evaluation. A 100x100 array means 10,000 evaluations. Keep LAMBDA logic efficient and dimensions reasonable.

8. **Use for Dynamic Sizing:** Combine with ROWS(), COLUMNS(), or COUNTA() to create arrays that automatically resize based on data extent.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SEQUENCE]] | Generates sequential numbers | For simple linear sequences; faster than MAKEARRAY for sequences |
| [[BYROW]] | Applies function to each row | When processing existing data by row, not generating new arrays |
| [[BYCOL]] | Applies function to each column | When processing existing data by column |
| [[MAP]] | Applies function to each element | When transforming existing arrays element-by-element |
| Array literals | Create arrays with {} syntax | For small, static arrays with known values |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the value generation function

*MAKEARRAY requires LAMBDA to define cell values:*
```
=MAKEARRAY(5, 5, LAMBDA(r, c, expression))
```
The LAMBDA takes row and column indices and returns each cell's value.

---

**[[LET]]** - Simplify complex LAMBDA expressions

*Use LET within LAMBDA for clearer calculations:*
```
=MAKEARRAY(10, 10, LAMBDA(r, c,
  LET(sum, r+c, diff, ABS(r-c), sum * diff)))
```
Names intermediate values for readability within the cell generator.

---

**[[INDEX]]** - Access external data within LAMBDA

*Reference existing ranges based on position:*
```
=MAKEARRAY(ROWS(Data), COLUMNS(Data), LAMBDA(r, c,
  INDEX(Data, r, c) * 2))
```
Creates a transformed version of an existing range.

---

**[[VSTACK]] / [[HSTACK]]** - Add headers to generated arrays

*Combine MAKEARRAY output with labels:*
```
=VSTACK({"A","B","C","D","E"}, MAKEARRAY(5, 5, LAMBDA(r, c, r*c)))
```
Adds a header row above the generated multiplication table.

---

**[[MMULT]]** - Perform matrix multiplication

*Use MAKEARRAY to create matrices, then multiply:*
```
=MMULT(MAKEARRAY(3,3,LAMBDA(r,c,IF(r=c,1,0))), DataMatrix)
```
Identity matrix multiplied by data matrix returns the data matrix.

---

**[[SEQUENCE]]** - Generate simple sequences

*Use SEQUENCE for linear sequences, MAKEARRAY for complex patterns:*
```
=SEQUENCE(10, 1, 1, 1)
```
SEQUENCE is simpler and faster when you just need sequential numbers.

## Official Documentation

- **Google Sheets:** [MAKEARRAY function](https://support.google.com/docs/answer/12570928)
- **Microsoft Excel:** [MAKEARRAY function](https://support.microsoft.com/en-us/office/makearray-function-b80da5ad-b338-4149-a523-5b221da09097)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2022 | Introduced as part of the LAMBDA helper function family |
| Excel 365 | 2022 | Added with LAMBDA function ecosystem |
| Excel 2021 | N/A | Not available in perpetual license version |
| Excel Online | 2022 | Available in web version |

---

*Last updated: 2026-01-10*
