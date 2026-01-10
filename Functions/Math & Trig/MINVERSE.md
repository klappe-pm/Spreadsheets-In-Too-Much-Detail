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
- inverse-matrix
- square-matrices
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- matrix-inverse
- inverse-matrix
tags:
- matrix
- inverse
- linear-algebra
- mathematical
- engineering
---

# MINVERSE

## Description

MINVERSE returns the inverse matrix for a square matrix stored in an array. The inverse of matrix A is a matrix A^(-1) such that when multiplied together (A * A^(-1)), they produce the identity matrix. Matrix inversion is fundamental to solving systems of linear equations, as the solution to Ax = b can be found as x = A^(-1) * b. Not all matrices have inverses - only those with non-zero determinants are invertible.

This function is crucial for linear algebra applications in spreadsheets, enabling solutions to systems of equations, least-squares regression calculations, and coordinate transformations. In business contexts, MINVERSE supports portfolio optimization (solving for optimal weights), input-output economic analysis, and solving simultaneous equations in forecasting models.

A critical gotcha is that MINVERSE only works with square matrices (equal rows and columns) that have non-zero determinants. Attempting to invert a singular matrix (determinant = 0) returns a #NUM! error. Numerical precision can be problematic - nearly singular matrices may produce inverted results with significant errors. The resulting inverse matrix has the same dimensions as the input, and you should verify accuracy by confirming that MMULT(original, inverse) approximately equals MUNIT(n).

Both Excel and Google Sheets support MINVERSE with consistent behavior. Legacy Excel versions require Ctrl+Shift+Enter to enter as an array formula, while Excel 365 and Google Sheets support dynamic array output automatically. Results are limited by floating-point precision (approximately 15 significant digits), so extremely large or ill-conditioned matrices may yield unreliable results.

## Syntax

> [!f(x)] MINVERSE Syntax
>
> ```excel
> MINVERSE(array)
> ```
>
> Returns the inverse matrix for a square matrix array.

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| array | A square numeric array (n x n) for which you want the inverse | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=MINVERSE({4,7;2,6})` | {0.6,-0.7;-0.2,0.4} | 2x2 inverse: det=10, formula applies |
| `=MINVERSE({1,0;0,1})` | {1,0;0,1} | Identity matrix is its own inverse |
| `=MINVERSE({2,0;0,2})` | {0.5,0;0,0.5} | Scalar matrix inverse: reciprocal on diagonal |
| `=MINVERSE({3})` | 0.333... | 1x1 matrix: inverse is 1/value |
| `=MINVERSE({1,2;2,4})` | #NUM! | Singular matrix (det=0), no inverse exists |
| `=MINVERSE(A1:B2)` | 2x2 array | Inverts matrix from cell range |
| `=MINVERSE({1,0,0;0,1,0;0,0,1})` | {1,0,0;0,1,0;0,0,1} | 3x3 identity is its own inverse |
| `=MINVERSE({1,2,3;0,1,4;5,6,0})` | Varies | General 3x3 matrix inversion |
| `=INDEX(MINVERSE(A1:C3),2,1)` | Varies | Extract specific element from inverse |
| `=MMULT(A1:B2,MINVERSE(A1:B2))` | {1,0;0,1} | Verify: original * inverse = identity |
| `=SUMPRODUCT((MMULT(A1:B2,MINVERSE(A1:B2))-MUNIT(2))^2)` | ~0 | Numerical verification of inversion accuracy |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Array is not square (rows != columns) | Ensure the matrix has equal rows and columns |
| #VALUE! | Array contains non-numeric values | Remove text, blank cells, or logical values from array |
| #NUM! | Matrix is singular (determinant = 0) | Check MDETERM first; singular matrices have no inverse |
| #NUM! | Matrix is nearly singular | Ill-conditioned matrix; consider regularization or alternative approach |

## Use Cases

### [[Solving Linear Equations]]
- **Implementation**: Solve system Ax = b by computing x = A^(-1) * b
- **Business Application**: Determining break-even quantities, allocation problems, and economic equilibrium calculations
- **Technical Details**: `=MMULT(MINVERSE(CoefficientMatrix), Constants)` returns solution vector; verify MDETERM(CoefficientMatrix)<>0 first

### [[Portfolio Optimization]]
- **Implementation**: Calculate optimal portfolio weights using covariance matrix inverse
- **Business Application**: Mean-variance optimization, risk parity, and minimum variance portfolio construction
- **Technical Details**: `=MMULT(MINVERSE(CovarianceMatrix), ExpectedReturns) / SUM(MMULT(MINVERSE(CovarianceMatrix), ExpectedReturns))` gives optimal weights

### [[Regression Coefficient Calculation]]
- **Implementation**: Compute OLS regression coefficients using normal equations
- **Business Application**: Building predictive models for sales forecasting, pricing analysis, and trend estimation
- **Technical Details**: `=MMULT(MINVERSE(MMULT(TRANSPOSE(X),X)), MMULT(TRANSPOSE(X),Y))` calculates beta coefficients where X is predictor matrix, Y is response

### [[Coordinate Transformation]]
- **Implementation**: Reverse coordinate transformations by inverting transformation matrices
- **Business Application**: Graphics applications, mapping systems, and engineering coordinate conversions
- **Technical Details**: `=MMULT(MINVERSE(TransformMatrix), TransformedCoords)` recovers original coordinates from transformed values

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| MINVERSE function | Yes (all versions) | Yes |
| Maximum matrix size | ~73x73 (varies) | ~73x73 (varies) |
| Array entry method | CSE in legacy; auto in 365 | Automatic |
| Dynamic array output | Excel 365+ | Yes |
| Precision | 15 significant digits | 15 significant digits |
| Singular matrix handling | #NUM! error | #NUM! error |

## Tips and Best Practices

1. **Always check determinant first** - Use `=IF(MDETERM(array)=0,"Singular","Invertible")` before attempting MINVERSE to avoid errors
2. **Verify inversion accuracy** - Multiply original by inverse with MMULT and compare to MUNIT; significant deviation indicates numerical problems
3. **Use condition number for stability** - Matrices with very high condition numbers (ratio of largest to smallest singular value) produce unreliable inverses
4. **Consider alternatives for large systems** - For systems larger than ~20x20, consider specialized linear algebra tools or iterative methods
5. **Watch for near-zero determinants** - A determinant of 1E-12 is mathematically non-zero but practically indicates ill-conditioning
6. **Diagonal matrices are simple** - Inverse of diagonal matrix has reciprocals on the diagonal; useful for validation
7. **Select sufficient output range** - In legacy Excel (CSE mode), select n x n range before entering formula with Ctrl+Shift+Enter

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[MDETERM]] | Calculates matrix determinant | Returns scalar; tests if inverse exists |
| [[MMULT]] | Multiplies two matrices | Used to verify inverse: A * A^(-1) = I |
| [[MUNIT]] | Creates identity matrix | Expected result of A * A^(-1) |
| [[TRANSPOSE]] | Transposes matrix | Flips rows/columns; not inverse |
| [[LINEST]] | Linear regression array | Built-in regression, may be simpler than manual MINVERSE |

### Commonly Used Together

- **[[MDETERM]]** - Verify non-zero determinant before attempting inversion
- **[[MMULT]]** - Multiply inverse by original to verify accuracy; solve Ax=b via A^(-1)*b
- **[[MUNIT]]** - Create identity matrix for comparison
- **[[TRANSPOSE]]** - Used in normal equations: (X'X)^(-1)X'Y
- **[[SUMPRODUCT]]** - Verify inversion error: sum of squared differences from identity
- **[[INDEX]]** - Extract specific elements from the returned inverse array

## Official Documentation

- [Microsoft: MINVERSE function](https://support.microsoft.com/en-us/office/minverse-function-11f55086-adde-4c9f-8eb9-59da2d72efc6)
- [Google: MINVERSE function](https://support.google.com/docs/answer/3094263)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 1.0 | 1985 | Function introduced |
| Excel 365 | Ongoing | Dynamic array support, no CSE required |
| Google Sheets | 2006 | Full support from launch |
