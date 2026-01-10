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
- determinants
- square-matrices
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- matrix-determinant
- determinant
tags:
- matrix
- determinant
- linear-algebra
- mathematical
- engineering
---

# MDETERM

## Description

MDETERM returns the matrix determinant of an array. The determinant is a single scalar value calculated from a square matrix that provides crucial information about the matrix's properties - including whether it's invertible, the volume scaling factor it represents, and whether its rows/columns are linearly independent. If the determinant is zero, the matrix is called singular and cannot be inverted.

This function is essential in advanced mathematics, engineering, and scientific computing applications. Determinants are used to solve systems of linear equations (Cramer's rule), calculate eigenvalues, determine if transformations preserve orientation, and verify matrix invertibility before attempting MINVERSE calculations. In business contexts, determinants appear in optimization problems, portfolio theory, and econometric models.

A critical gotcha is that MDETERM only works with square matrices (same number of rows and columns). The array must contain only numeric values - any text, blank cells, or logical values will cause errors. For large matrices, small numerical errors can accumulate during calculation, potentially giving very small non-zero values instead of true zero for nearly-singular matrices. The computational complexity grows rapidly with matrix size.

Both Excel and Google Sheets provide MDETERM with consistent behavior. In older Excel versions, the function requires Ctrl+Shift+Enter for array entry, though modern Excel 365 and Google Sheets support dynamic arrays. The function handles matrices up to approximately 73x73 in Excel (implementation-dependent), though practical performance degrades with very large matrices.

## Syntax

> [!f(x)] MDETERM Syntax
>
> ```excel
> MDETERM(array)
> ```
>
> Returns the matrix determinant of an array (square matrix).

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| array | A square numeric array (n x n) for which you want the determinant | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=MDETERM({1,2;3,4})` | -2 | 2x2 determinant: (1)(4)-(2)(3) = 4-6 = -2 |
| `=MDETERM({2,0;0,2})` | 4 | Diagonal matrix determinant: 2 * 2 = 4 |
| `=MDETERM({1,0;0,1})` | 1 | Identity matrix always has determinant 1 |
| `=MDETERM({1,2;2,4})` | 0 | Singular matrix (row 2 = 2 * row 1), not invertible |
| `=MDETERM({3})` | 3 | 1x1 matrix determinant equals the single value |
| `=MDETERM({1,0,0;0,1,0;0,0,1})` | 1 | 3x3 identity matrix |
| `=MDETERM({2,0,0;0,3,0;0,0,4})` | 24 | Diagonal matrix: product of diagonal = 2*3*4 |
| `=MDETERM({1,2,3;4,5,6;7,8,9})` | 0 | Singular: columns are linearly dependent |
| `=MDETERM({6,1,1;4,-2,5;2,8,7})` | -306 | General 3x3 matrix computation |
| `=MDETERM(A1:C3)` | Varies | Calculates determinant from cell range |
| `=MDETERM({1,1,1,1;1,2,3,4;1,3,6,10;1,4,10,20})` | 1 | 4x4 Pascal matrix has determinant 1 |
| `=ABS(MDETERM(A1:D4))` | Varies | Absolute value of determinant (volume scaling) |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Array is not square (rows != columns) | Ensure the matrix has equal rows and columns |
| #VALUE! | Array contains non-numeric values | Remove text, blank cells, or logical values from array |
| #VALUE! | Array reference is invalid | Verify the range reference is correct |
| #NUM! | Result exceeds numeric limits | Break down calculation or use smaller matrix |

## Use Cases

### [[Linear System Solvability - Cramer's Rule]]
- **Implementation**: Check if a system of linear equations has a unique solution
- **Business Application**: Verifying economic models, supply-demand equilibrium, and resource allocation problems have solutions
- **Technical Details**: `=IF(MDETERM(CoefficientMatrix)<>0,"Unique Solution","No Unique Solution")` tests solvability; non-zero determinant indicates exactly one solution exists

### [[Portfolio Covariance Analysis]]
- **Implementation**: Verify covariance matrices are positive definite before optimization
- **Business Application**: Ensuring portfolio risk calculations and mean-variance optimization are mathematically valid
- **Technical Details**: `=MDETERM(CovarianceMatrix)` must be positive for valid covariance; zero or negative indicates multicollinearity problems

### [[Geometric Transformations - Area/Volume Scaling]]
- **Implementation**: Calculate how much a linear transformation scales areas or volumes
- **Business Application**: Computer graphics, engineering design, and geographic information systems transformations
- **Technical Details**: `=ABS(MDETERM(TransformMatrix))` gives the absolute scaling factor; negative determinant indicates reflection

### [[Engineering - Structural Analysis]]
- **Implementation**: Analyze stability of structural systems via stiffness matrix determinants
- **Business Application**: Civil and mechanical engineering checking for structural stability and mechanism detection
- **Technical Details**: Near-zero determinant in stiffness matrix indicates unstable structure or rigid body motion; `=IF(ABS(MDETERM(StiffnessMatrix))<1E-10,"Unstable","Stable")`

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| MDETERM function | Yes (all versions) | Yes |
| Maximum matrix size | ~73x73 (varies) | ~73x73 (varies) |
| Array entry method | CSE in legacy; auto in 365 | Automatic |
| Inline array syntax | {1,2;3,4} | {1,2;3,4} |
| Precision | 15 significant digits | 15 significant digits |
| Error handling | #VALUE! for invalid input | #VALUE! for invalid input |

## Tips and Best Practices

1. **Verify square matrix requirement** - MDETERM requires exactly equal row and column counts; use ROWS() and COLUMNS() to validate before calling
2. **Check determinant before MINVERSE** - If MDETERM returns 0 (or near-zero), MINVERSE will fail or return incorrect results
3. **Consider numerical precision** - Very small determinants (like 1E-14) may be computational artifacts rather than true zeros; use a threshold for singular testing
4. **Use absolute value for scaling factors** - Negative determinant indicates orientation reversal; ABS(MDETERM(...)) gives pure scaling magnitude
5. **Diagonal matrices are easy** - Determinant of a diagonal matrix is simply the product of diagonal elements
6. **Identity matrix benchmark** - MDETERM of any identity matrix equals 1; useful for verifying correct matrix dimensions
7. **Break down large calculations** - For very large matrices, consider block matrix techniques or specialized linear algebra tools

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[MINVERSE]] | Calculates matrix inverse | Requires non-zero determinant; MDETERM tests invertibility |
| [[MMULT]] | Multiplies two matrices | No square requirement; different operation |
| [[MUNIT]] | Creates identity matrix | Returns matrix, not scalar |
| [[SUMPRODUCT]] | Multiplies and sums arrays | Element-wise operation, not matrix algebra |
| [[TRANSPOSE]] | Transposes matrix | Rearranges elements; determinant unchanged |

### Commonly Used Together

- **[[MINVERSE]]** - Calculate inverse after confirming non-zero determinant
- **[[MMULT]]** - Verify matrix multiplications (det(AB) = det(A)*det(B))
- **[[TRANSPOSE]]** - Transpose operations (determinant unchanged under transpose)
- **[[MUNIT]]** - Create identity matrices for comparison
- **[[IF]]** - Conditional logic based on determinant (singularity testing)
- **[[ABS]]** - Get absolute determinant value for volume/area calculations

## Official Documentation

- [Microsoft: MDETERM function](https://support.microsoft.com/en-us/office/mdeterm-function-e7bfa857-3834-422b-b871-0ffd03717020)
- [Google: MDETERM function](https://support.google.com/docs/answer/3094161)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 1.0 | 1985 | Function introduced |
| Excel 365 | Ongoing | Dynamic array support, no CSE required |
| Google Sheets | 2006 | Full support from launch |
