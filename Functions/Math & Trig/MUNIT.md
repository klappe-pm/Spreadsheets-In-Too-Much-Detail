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
- identity-matrix
- array-formulas
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- identity-matrix
- unit-matrix
tags:
- matrix
- identity
- linear-algebra
- mathematical
- engineering
---

# MUNIT

## Description

MUNIT returns the identity matrix (also called unit matrix) for a specified dimension n. The identity matrix is a square matrix with 1s on the main diagonal and 0s everywhere else. It serves as the multiplicative identity in matrix algebra - multiplying any matrix by the identity matrix returns the original matrix unchanged, just as multiplying any number by 1 returns the same number.

This function is essential for linear algebra operations in spreadsheets, providing the foundation for matrix verification, initialization, and calculations. The identity matrix is used to verify MINVERSE results (A * A^(-1) should equal I), create baseline matrices for transformations, initialize iterative algorithms, and represent "no change" transformations in graphics and engineering applications.

A critical gotcha is that MUNIT is only available in Excel 2013 and later versions, and in Google Sheets. Older Excel versions require manually constructing identity matrices. The function returns a dynamic array, so in modern Excel 365 and Google Sheets it automatically spills to adjacent cells. In legacy Excel (2013-2019), you must select an n x n range and enter with Ctrl+Shift+Enter. The dimension must be a positive integer.

Both Excel (2013+) and Google Sheets fully support MUNIT with dynamic array output. The function creates a square matrix of the specified dimension. Maximum practical size is limited by spreadsheet capacity and performance considerations, though matrices up to approximately 73 x 73 work reliably.

## Syntax

> [!f(x)] MUNIT Syntax
>
> ```excel
> MUNIT(dimension)
> ```
>
> Returns the identity matrix for the specified dimension.

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| dimension | The size n of the n x n identity matrix to create | Yes | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=MUNIT(1)` | 1 | 1x1 identity matrix is just the value 1 |
| `=MUNIT(2)` | {1,0;0,1} | 2x2 identity matrix |
| `=MUNIT(3)` | {1,0,0;0,1,0;0,0,1} | 3x3 identity matrix |
| `=MUNIT(4)` | 4x4 array | 4x4 identity with 1s on diagonal |
| `=INDEX(MUNIT(5),3,3)` | 1 | Diagonal element is always 1 |
| `=INDEX(MUNIT(5),2,4)` | 0 | Off-diagonal element is always 0 |
| `=MMULT(A1:C3,MUNIT(3))` | A1:C3 values | Any matrix times identity = original |
| `=MMULT(MUNIT(3),A1:C3)` | A1:C3 values | Identity times any matrix = original |
| `=MDETERM(MUNIT(5))` | 1 | Determinant of identity is always 1 |
| `=MINVERSE(MUNIT(4))` | 4x4 identity | Identity is its own inverse |
| `=SUM(MUNIT(10))` | 10 | Sum of identity matrix = dimension (count of 1s) |
| `=SUMPRODUCT((MUNIT(3)={1,0,0;0,1,0;0,0,1})*1)` | 9 | All 9 elements match |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Dimension is not a valid number | Provide a positive integer for dimension |
| #VALUE! | Dimension is less than 1 | Use a positive integer (1 or greater) |
| #VALUE! | Dimension is non-integer (some versions) | Use INT() to ensure integer input |
| #NAME? | Function not available (Excel 2010 or earlier) | Upgrade Excel or manually construct identity matrix |

## Use Cases

### [[Matrix Verification - Inverse Checking]]
- **Implementation**: Verify that matrix inversion was computed correctly
- **Business Application**: Validating financial models, regression calculations, and optimization solutions
- **Technical Details**: `=SUMPRODUCT((MMULT(A1:C3,MINVERSE(A1:C3))-MUNIT(3))^2)` should be near 0; large values indicate numerical problems

### [[Transformation Initialization]]
- **Implementation**: Start with identity matrix and build up transformations incrementally
- **Business Application**: Graphics programming, coordinate system conversions, and engineering transformations
- **Technical Details**: Begin with MUNIT(n), then use MMULT to chain rotation, scaling, and translation matrices; identity represents "no transformation"

### [[Regression and Statistics]]
- **Implementation**: Use identity in statistical calculations like ridge regression regularization
- **Business Application**: Building predictive models with regularization to prevent overfitting
- **Technical Details**: Ridge regression adds lambda*I to X'X before inversion: `=MMULT(MINVERSE(MMULT(TRANSPOSE(X),X)+lambda*MUNIT(p)), MMULT(TRANSPOSE(X),Y))`

### [[Kronecker Operations]]
- **Implementation**: Construct larger matrices from smaller ones using identity blocks
- **Business Application**: Multi-level models, repeated measures analysis, and block-diagonal structures
- **Technical Details**: Identity matrices form the basis for constructing block diagonal matrices and Kronecker products in advanced statistical models

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| MUNIT function | Yes (2013+) | Yes |
| Dynamic array output | Excel 365+ automatic | Yes |
| CSE entry required | Excel 2013-2019 | No |
| Maximum dimension | ~73 (practical) | ~73 (practical) |
| Decimal dimension | Truncated to integer | Truncated to integer |
| Legacy Excel support | Not available pre-2013 | N/A |

## Tips and Best Practices

1. **Use for verification** - After MINVERSE, multiply by original and compare to MUNIT to verify accuracy
2. **Remember the properties** - det(I) = 1, I^(-1) = I, A * I = I * A = A for any compatible matrix A
3. **Dynamic array in modern Excel** - In Excel 365, just enter MUNIT(n) in one cell; it spills automatically
4. **Sum equals dimension** - The sum of an identity matrix equals its dimension (easy validation)
5. **Build manual identity for old Excel** - Use `=IF(ROW(A1)=COLUMN(A1),1,0)` and copy to create identity in pre-2013 Excel
6. **Trace equals dimension** - The trace (sum of diagonal) of I(n) is n
7. **Eigenvalues are all 1** - Identity matrix has all eigenvalues equal to 1

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[MINVERSE]] | Calculates matrix inverse | MINVERSE(MUNIT(n)) = MUNIT(n) |
| [[MDETERM]] | Calculates matrix determinant | MDETERM(MUNIT(n)) = 1 always |
| [[MMULT]] | Multiplies two matrices | MMULT(A, MUNIT(n)) = A |
| [[MAKEARRAY]] | Creates custom array with formula | More flexible but more complex |
| [[SEQUENCE]] | Creates sequential array | Different purpose; generates sequences |

### Commonly Used Together

- **[[MMULT]]** - Multiply matrices; identity is neutral element (A*I = I*A = A)
- **[[MINVERSE]]** - Verify inverse calculations by checking A*A^(-1) = MUNIT(n)
- **[[MDETERM]]** - Determinant of identity is always 1
- **[[INDEX]]** - Extract specific elements from the identity matrix
- **[[SUMPRODUCT]]** - Calculate comparison metrics between matrices
- **[[IF]]** - Build custom matrices based on identity structure

## Official Documentation

- [Microsoft: MUNIT function](https://support.microsoft.com/en-us/office/munit-function-c9fe916a-dc26-4105-997d-ba22799853a0)
- [Google: MUNIT function](https://support.google.com/docs/answer/9368178)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2012-10 | Function introduced |
| Excel 365 | Ongoing | Dynamic array support, automatic spilling |
| Google Sheets | 2019 | Full support added |
