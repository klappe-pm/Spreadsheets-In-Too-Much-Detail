---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- lambda-functions
- array-functions
subTopics:
- array-generation
- functional-programming
- dynamic-arrays
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Make Array
- Array Generator
- Create Array
tags:
- excel-only
- lambda
- arrays
- dynamic-arrays
- functional
---

# MAKEARRAY

## Description

**MAKEARRAY** creates an array of specified dimensions by calling a LAMBDA function for each element. You specify the number of rows and columns, and provide a lambda that receives the current row and column indices and returns the value for that position. This is array generation through functional programming - you define the rule, and MAKEARRAY builds the array.

Think of MAKEARRAY as a programmable grid generator. Need a 5x5 multiplication table? A 10-row sequence? A triangular matrix? An identity matrix? Rather than typing out values or using complex array formulas, MAKEARRAY lets you express the pattern mathematically and generates the entire array dynamically.

**Important understanding:** The lambda receives row and column numbers as 1-based indices (first element is row 1, column 1, not 0, 0). Your lambda must accept exactly two parameters (row number, column number) and return the value for that cell. The generated array spills automatically in Excel 365/2021.

**Platform note:** MAKEARRAY is exclusively available in Microsoft 365 Excel and Excel 2024+. It requires the LAMBDA function, also Excel-exclusive. Google Sheets has added MAKEARRAY with similar syntax in 2022. This function will not work in Excel 2019 or earlier.

## Syntax

> [!f(x)] MAKEARRAY Syntax
>
> ```
> =MAKEARRAY(rows, cols, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rows` | Yes | The number of rows in the returned array. Must be a positive integer. |
| `cols` | Yes | The number of columns in the returned array. Must be a positive integer. |
| `lambda` | Yes | A LAMBDA function with two parameters (row_index, col_index) that returns the value for each array position. |

### Return Value

Returns an array with the specified dimensions, where each element is the result of calling the lambda with that element's row and column indices.

## Examples

> [!f(x)] MAKEARRAY Examples

### Example 1: Simple Counting Grid
```
=MAKEARRAY(3, 4, LAMBDA(r, c, r*10+c))
```
**Result:**
```
11  12  13  14
21  22  23  24
31  32  33  34
```

**Explanation:** Each cell contains row*10 + column, creating a grid where you can read position from the value. Row 2, column 3 contains 23.

---

### Example 2: Multiplication Table
```
=MAKEARRAY(10, 10, LAMBDA(r, c, r*c))
```
**Result:** 10x10 multiplication table (1x1 through 10x10)

**Explanation:** The classic multiplication table. Row represents one factor, column the other, cell contains the product.

---

### Example 3: Identity Matrix
```
=MAKEARRAY(5, 5, LAMBDA(r, c, IF(r=c, 1, 0)))
```
**Result:**
```
1  0  0  0  0
0  1  0  0  0
0  0  1  0  0
0  0  0  1  0
0  0  0  0  1
```

**Explanation:** Creates an identity matrix with 1s on the diagonal. The IF checks if row equals column to place the 1s.

---

### Example 4: Sequential Numbers
```
=MAKEARRAY(5, 3, LAMBDA(r, c, (r-1)*3+c))
```
**Result:**
```
1   2   3
4   5   6
7   8   9
10  11  12
13  14  15
```

**Explanation:** Creates sequential numbering across rows, like filling left-to-right, top-to-bottom. The formula converts 2D position to sequential number.

---

### Example 5: Column-wise Sequential
```
=MAKEARRAY(3, 5, LAMBDA(r, c, r+(c-1)*3))
```
**Result:**
```
1   4   7   10  13
2   5   8   11  14
3   6   9   12  15
```

**Explanation:** Sequential numbers going down columns instead of across rows. Changes the calculation to column-major ordering.

---

### Example 6: Checkerboard Pattern
```
=MAKEARRAY(8, 8, LAMBDA(r, c, IF(MOD(r+c, 2)=0, "X", "O")))
```
**Result:** 8x8 checkerboard with X and O alternating

**Explanation:** MOD(r+c, 2) alternates between 0 and 1 as you move across the grid, creating the checkerboard pattern.

---

### Example 7: Pascal's Triangle Values
```
=MAKEARRAY(6, 6, LAMBDA(r, c, IF(c>r, "", COMBIN(r-1, c-1))))
```
**Result:**
```
1
1  1
1  2  1
1  3  3  1
1  4  6  4  1
1  5  10 10 5  1
```

**Explanation:** Pascal's triangle using combinations. COMBIN(n,k) gives binomial coefficients. Blanks fill upper-right where c>r.

---

### Example 8: Random Array with Seed Pattern
```
=MAKEARRAY(5, 5, LAMBDA(r, c, ROUND(SIN(r*c)*100, 0)))
```
**Result:** Reproducible pseudo-random looking numbers

**Explanation:** Using SIN creates varied but reproducible values. Unlike RANDARRAY, this gives the same result every time.

---

### Example 9: Lower Triangular Matrix
```
=MAKEARRAY(5, 5, LAMBDA(r, c, IF(c<=r, r*c, 0)))
```
**Result:**
```
1   0   0   0   0
2   4   0   0   0
3   6   9   0   0
4   8   12  16  0
5   10  15  20  25
```

**Explanation:** Values only below and on the diagonal. Commonly needed in linear algebra operations.

---

### Example 10: Distance Matrix
```
=MAKEARRAY(5, 5, LAMBDA(r, c, ABS(r-c)))
```
**Result:**
```
0  1  2  3  4
1  0  1  2  3
2  1  0  1  2
3  2  1  0  1
4  3  2  1  0
```

**Explanation:** Shows the "distance" (absolute difference) between row and column indices. Forms a symmetric matrix.

---

### Example 11: Date Series Grid
```
=MAKEARRAY(4, 7, LAMBDA(r, c, DATE(2024, 1, 1)+(r-1)*7+(c-1)))
```
**Result:** Calendar grid starting January 1, 2024 (28 days in 4 weeks)

**Explanation:** Generates a calendar month view. Each row is a week, each column a day. Format cells as dates.

---

### Example 12: Lookup Reference Grid
```
=MAKEARRAY(ROWS(DataRange), COLUMNS(HeaderRange), LAMBDA(r, c, INDEX(DataRange, r, c)))
```
**Result:** Copies the structure of DataRange dynamically

**Explanation:** Creates a dynamic copy of an existing range. The array dimensions match the source, and INDEX retrieves each value.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Lambda doesn't accept exactly 2 parameters | Lambda must have form `LAMBDA(row, col, expression)` |
| `#VALUE!` | Rows or cols is zero or negative | Both dimensions must be positive integers |
| `#NAME?` | MAKEARRAY or LAMBDA not recognized | Requires Excel 365/2024+; not available in earlier versions |
| `#CALC!` | Array too large | Excel has limits on array size; reduce dimensions |
| `#SPILL!` | Not enough room for result | Clear cells in the spill range |

## Use Cases

### [[Dynamic Matrix Generation]]

**Scenario:** Generate transformation matrices for mathematical modeling based on parameters.

**Implementation:**
```
=MAKEARRAY(3, 3, LAMBDA(r, c,
    IF(r=c, ScaleFactor,
        IF(AND(r=1,c=2), ShearX,
            IF(AND(r=2,c=1), ShearY, 0)))
))
```

**Business Application:** Engineering calculations, 3D graphics transformations, and statistical models often require matrices. MAKEARRAY creates them from parameters without manual entry.

**Technical Details:** Conditional logic within the lambda places specific values at specific positions. Named ranges (ScaleFactor, ShearX, ShearY) make the formula parameterized.

---

### [[Test Data Generation]]

**Scenario:** Create structured test data for formula development or demonstrations.

**Implementation:**
```
=MAKEARRAY(100, 5, LAMBDA(r, c,
    SWITCH(c,
        1, "ID-"&TEXT(r,"0000"),
        2, CHOOSE(MOD(r,4)+1, "Alpha", "Beta", "Gamma", "Delta"),
        3, MOD(r*17, 1000),
        4, DATE(2024, MOD(r,12)+1, MOD(r,28)+1),
        5, IF(MOD(r,3)=0, "Active", "Inactive")
    )
))
```

**Business Application:** Generate realistic-looking test datasets for developing and testing Excel solutions. Ensures consistent test data that can be regenerated.

**Technical Details:** SWITCH handles different column types. Modular arithmetic creates varied but reproducible patterns.

---

### [[Amortization Schedule Skeleton]]

**Scenario:** Create the structure of a loan amortization schedule.

**Implementation:**
```
=MAKEARRAY(NumPayments, 5, LAMBDA(r, c,
    SWITCH(c,
        1, r,
        2, IF(r=1, Principal, ""),
        3, IF(r=1, MonthlyPayment, ""),
        4, IF(r=1, InterestRate/12, ""),
        5, IF(r=1, "=B"&r&"-D"&r, "")
    )
))
```

**Business Application:** Financial models can programmatically generate schedule structures. MAKEARRAY creates the skeleton; subsequent formulas fill calculations.

**Technical Details:** This creates a template structure. Real implementation would calculate actual values using financial formulas.

## Platform Differences

### Microsoft Excel (365/2024+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Complex lambda logic | Full support |
| Large arrays | Up to calculation limits |
| Dynamic spilling | Full support |

### Google Sheets

| Feature | Support |
|---------|---------|
| MAKEARRAY | Available (added 2022) |
| Syntax | Same as Excel |
| Lambda support | Full support |
| Performance | May differ from Excel |

Google Sheets added MAKEARRAY in 2022. Formulas should be portable between platforms, though testing is recommended.

### Other Platforms

MAKEARRAY is available in:
- Microsoft Excel 365: Full support
- Microsoft Excel 2024: Full support
- Google Sheets: Full support (added 2022)
- LibreOffice Calc: Not available
- Apple Numbers: Not available
- Excel 2019 and earlier: Not available

## Tips and Best Practices

1. **Use meaningful parameter names:** `LAMBDA(r, c, ...)` or `LAMBDA(row, col, ...)` makes formulas more readable than `LAMBDA(a, b, ...)`.

2. **Test lambda logic independently:** Verify your calculation works for individual positions before generating large arrays.

3. **Consider SEQUENCE for simple cases:** If you just need sequential numbers, SEQUENCE is simpler than MAKEARRAY.

4. **Combine with INDEX for dynamic patterns:** `LAMBDA(r, c, INDEX(PatternRange, MOD(r-1, Rows)+1, MOD(c-1, Cols)+1))` tiles a pattern.

5. **Use for reproducibility:** Unlike RANDARRAY, MAKEARRAY gives the same result every time (unless your lambda includes volatile functions).

6. **Watch array size limits:** Very large arrays can cause performance issues or errors. Test with smaller dimensions first.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SEQUENCE]] | Generate sequential numbers | For simple sequential arrays without custom logic |
| [[RANDARRAY]] | Generate random numbers | For random data without deterministic patterns |
| [[EXPAND]] | Expand array to larger dimensions | When padding existing arrays, not generating from scratch |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the generation rule

*Every MAKEARRAY needs a LAMBDA:*
```
=MAKEARRAY(5, 5, LAMBDA(r, c, r+c))
```
The lambda defines the value at each position.

---

**[[IF]] / [[SWITCH]]** - Conditional value generation

*Different values by position:*
```
=MAKEARRAY(3, 3, LAMBDA(r, c, IF(r=c, "X", "-")))
```
Conditional logic creates patterns within the generated array.

---

**[[SEQUENCE]]** - Alternative for sequential data

*Compare approaches:*
```
SEQUENCE(5, 5) - sequential 1-25 (simpler)
MAKEARRAY(5, 5, LAMBDA(r, c, (r-1)*5+c)) - same result (more flexible)
```
Use SEQUENCE when it suffices; MAKEARRAY when you need custom logic.

---

**[[MOD]]** - Create repeating patterns

*Checkerboard or striped patterns:*
```
=MAKEARRAY(10, 10, LAMBDA(r, c, MOD(r+c, 2)))
```
MOD creates cyclic patterns essential for many MAKEARRAY applications.

---

**[[INDEX]]** - Reference external data in generation

*Generate based on lookup data:*
```
=MAKEARRAY(5, 5, LAMBDA(r, c, INDEX(LookupTable, r, c)))
```
INDEX lets the generated array draw values from existing ranges.

## Official Documentation

- **Microsoft Excel:** [MAKEARRAY function](https://support.microsoft.com/en-us/office/makearray-function-b80da5ad-b338-4149-a523-5b221da09f36)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | March 2022 | Released with LAMBDA helper functions |
| Excel 2024 | 2024 | Included in perpetual license version |
| Excel Online | 2022 | Full support in web version |
| Google Sheets | 2022 | Added with similar syntax |
| Earlier versions | Not available | Requires LAMBDA support |

---

*Last updated: 2026-01-10*
