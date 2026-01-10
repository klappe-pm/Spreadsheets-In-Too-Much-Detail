---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
- linear-algebra
subTopics:
- matrix-operations
- matrix-multiplication
- array-formulas
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- matrix-multiply
- matrix-multiplication
tags:
- matrix
- multiplication
- linear-algebra
- mathematical
- engineering
---

# MMULT

## Description

MMULT returns the matrix product of two arrays. Matrix multiplication is a fundamental linear algebra operation where each element of the result is computed as the dot product of a row from the first matrix and a column from the second matrix. The result is a new matrix with dimensions determined by the outer dimensions of the input matrices: if A is m x n and B is n x p, then A * B is m x p.

This function is essential for linear algebra computations in spreadsheets, enabling transformations, solving systems of equations, and weighted calculations. In business contexts, MMULT powers portfolio value calculations (weights times returns), input-output economic models, transition probability calculations in Markov chains, and multi-dimensional weighted scoring systems.

A critical gotcha is the dimensional compatibility requirement: the number of columns in the first array must exactly equal the number of rows in the second array. Matrix multiplication is NOT commutative - A * B does not equal B * A in general, and the dimensions may not even allow B * A. Additionally, the function requires all cells to contain numeric values; any text, logical values, or empty cells cause errors.

Both Excel and Google Sheets support MMULT with identical behavior. In legacy Excel versions, you must select an output range matching the expected dimensions (rows of first matrix by columns of second matrix) and enter with Ctrl+Shift+Enter. Modern Excel 365 and Google Sheets support dynamic array output, automatically sizing the result. The function handles the maximum practical matrix size of approximately 73 x 73.

## Syntax

> [!f(x)] MMULT Syntax
>
> ```excel
> MMULT(array1, array2)
> ```
>
> Returns the matrix product of two arrays.

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| array1 | The first array (m x n), providing rows for the result | Yes | - |
| array2 | The second array (n x p), providing columns for the result | Yes | - |

**Dimensional Requirement**: Columns of array1 must equal rows of array2.

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=MMULT({1,2;3,4},{5,6;7,8})` | {19,22;43,50} | 2x2 times 2x2 gives 2x2; (1*5+2*7=19, etc.) |
| `=MMULT({1,2,3},{4;5;6})` | 32 | 1x3 row times 3x1 column = scalar (dot product) |
| `=MMULT({4;5;6},{1,2,3})` | 3x3 array | 3x1 times 1x3 = 3x3 outer product |
| `=MMULT({1,0;0,1},{5,6;7,8})` | {5,6;7,8} | Identity matrix times any matrix = that matrix |
| `=MMULT({2,0;0,2},{1,2;3,4})` | {2,4;6,8} | Scalar matrix multiplication (scales all elements) |
| `=MMULT(A1:C2,D1:D3)` | 2x1 array | 2x3 times 3x1 gives 2x1 |
| `=MMULT(A1:B3,C1:D2)` | 3x2 array | 3x2 times 2x2 gives 3x2 |
| `=MMULT(A1:A3,TRANSPOSE(A1:A3))` | 3x3 array | Column times its transpose = symmetric matrix |
| `=MMULT(TRANSPOSE(X),X)` | Varies | X'X gram matrix (used in regression) |
| `=MMULT(MINVERSE(A1:B2),A1:B2)` | {1,0;0,1} | Inverse times original = identity |
| `=INDEX(MMULT(A1:C2,D1:E3),1,2)` | Varies | Extract specific element from result |
| `=MMULT({0.3,0.4,0.3},{100;200;300})` | 210 | Weighted sum: portfolio value calculation |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Columns of array1 != rows of array2 | Verify dimensional compatibility; use COLUMNS() and ROWS() to check |
| #VALUE! | Arrays contain non-numeric values | Remove text, blank cells, or logical values |
| #VALUE! | Invalid array reference | Verify range references are correct |
| #N/A | Result doesn't fit selected range (legacy Excel) | Select proper output dimensions before Ctrl+Shift+Enter |

## Use Cases

### [[Portfolio Value Calculation]]
- **Implementation**: Calculate portfolio value from weights and asset values
- **Business Application**: Investment portfolio management, fund valuation, and position tracking
- **Technical Details**: `=MMULT(Weights, AssetValues)` where Weights is 1 x n (holdings per asset) and AssetValues is n x 1 (price per asset); result is total portfolio value

### [[Transition Probability - Markov Chains]]
- **Implementation**: Calculate future state probabilities through matrix exponentiation
- **Business Application**: Customer churn modeling, credit rating transitions, and inventory state forecasting
- **Technical Details**: `=MMULT(MMULT(TransitionMatrix, TransitionMatrix), TransitionMatrix)` gives 3-step transition probabilities; MMULT(CurrentState, TransitionMatrix) gives next period state

### [[Multi-Criteria Weighted Scoring]]
- **Implementation**: Calculate weighted scores across multiple dimensions and alternatives
- **Business Application**: Vendor selection, project prioritization, and employee performance evaluation
- **Technical Details**: `=MMULT(ScoresMatrix, CriteriaWeights)` where ScoresMatrix is alternatives x criteria and CriteriaWeights is criteria x 1; result ranks alternatives

### [[Coordinate Transformations]]
- **Implementation**: Apply rotation, scaling, and translation transformations to coordinate data
- **Business Application**: Graphics applications, mapping systems, engineering CAD transformations
- **Technical Details**: `=MMULT(TransformMatrix, Coordinates)` applies transformation; chain transformations with nested MMULT calls

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| MMULT function | Yes (all versions) | Yes |
| Maximum matrix size | ~73x73 (varies) | ~73x73 (varies) |
| Array entry method | CSE in legacy; auto in 365 | Automatic |
| Dynamic array output | Excel 365+ | Yes |
| Inline array syntax | {1,2;3,4} | {1,2;3,4} |
| Precision | 15 significant digits | 15 significant digits |

## Tips and Best Practices

1. **Verify dimensions before MMULT** - Use `=IF(COLUMNS(A)=ROWS(B),"Compatible","Incompatible")` to check before multiplying
2. **Remember: order matters** - Matrix multiplication is not commutative; A*B != B*A in general
3. **Use TRANSPOSE for dot products** - `=MMULT(TRANSPOSE(A),B)` computes A'B (common in statistics)
4. **Chain multiplications left-to-right** - MMULT(MMULT(A,B),C) computes (AB)C; associative but not commutative
5. **Identity matrix check** - Multiplying by MUNIT(n) should return the original matrix unchanged
6. **Single row/column = dot product** - MMULT of 1xn and nx1 returns a scalar (sum of element-wise products)
7. **Select output range in legacy Excel** - In pre-365 Excel, select m x p range and use Ctrl+Shift+Enter

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[SUMPRODUCT]] | Multiplies arrays element-wise and sums | Element-wise, not matrix multiplication |
| [[MINVERSE]] | Calculates matrix inverse | Different operation; MMULT(A,MINVERSE(A))=I |
| [[MDETERM]] | Calculates matrix determinant | Returns scalar, not matrix |
| [[MUNIT]] | Creates identity matrix | Returns identity for MMULT verification |
| [[TRANSPOSE]] | Transposes matrix | Switches rows/columns; often used with MMULT |

### Commonly Used Together

- **[[TRANSPOSE]]** - Transpose matrices for proper dimensional alignment or statistical calculations (X'X)
- **[[MINVERSE]]** - Solve linear systems: x = A^(-1)*b via MMULT(MINVERSE(A),b)
- **[[MUNIT]]** - Verify calculations: MMULT(A, MUNIT(n)) should equal A
- **[[INDEX]]** - Extract specific elements from MMULT result arrays
- **[[SUMPRODUCT]]** - Alternative for element-wise operations when matrix multiplication isn't needed
- **[[SEQUENCE]]** - Generate sequential arrays for matrix construction

## Official Documentation

- [Microsoft: MMULT function](https://support.microsoft.com/en-us/office/mmult-function-40593ed7-a3cd-4b6b-b9a3-e4ad3c7245eb)
- [Google: MMULT function](https://support.google.com/docs/answer/3094292)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 1.0 | 1985 | Function introduced |
| Excel 365 | Ongoing | Dynamic array support, no CSE required |
| Google Sheets | 2006 | Full support from launch |
