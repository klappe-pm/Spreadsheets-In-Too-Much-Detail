---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
- random-numbers
subTopics:
- array-formulas
- simulation
- dynamic-arrays
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- random-array
- random-number-array
tags:
- random
- array
- simulation
- mathematical
- dynamic-array
---

# RANDARRAY

## Description

RANDARRAY generates an array of random numbers with specified dimensions and value range. Unlike the basic RAND() function that returns a single random value, RANDARRAY creates an entire matrix of random values in one formula, with control over the number of rows, columns, minimum and maximum values, and whether to generate integers or decimals. This is a dynamic array function that automatically spills to adjacent cells.

This function is essential for simulation modeling, Monte Carlo analysis, sample data generation, and statistical testing. In business contexts, RANDARRAY powers scenario analysis (generating thousands of random scenarios), bootstrapping for statistical inference, creating test datasets, random assignment in experiments, and building stochastic models for risk analysis.

A critical gotcha is that RANDARRAY recalculates every time the worksheet changes, producing new random values each time. If you need stable random values, copy the results and use Paste Special > Values. The function generates uniformly distributed random numbers; for other distributions, you'll need to transform the output (e.g., using NORM.INV for normal distribution). Also note that the integer parameter only affects boundary handling - TRUE generates integers from min to max inclusive.

RANDARRAY is available in Excel 365 and Google Sheets, but NOT in Excel 2019 or earlier perpetual license versions. Both platforms support all five parameters. In Google Sheets, the function works identically to Excel 365. The function leverages dynamic array behavior, automatically expanding to fill the specified dimensions.

## Syntax

> [!f(x)] RANDARRAY Syntax
>
> ```excel
> RANDARRAY([rows], [columns], [min], [max], [integer])
> ```
>
> Returns an array of random numbers with specified dimensions and range.

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| rows | Number of rows in the output array | No | 1 |
| columns | Number of columns in the output array | No | 1 |
| min | Minimum value in the range | No | 0 |
| max | Maximum value in the range | No | 1 |
| integer | TRUE for whole numbers, FALSE for decimals | No | FALSE |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=RANDARRAY()` | Single decimal 0-1 | Default: 1 row, 1 column, range 0-1, decimal |
| `=RANDARRAY(5)` | 5x1 array | Five random decimals between 0 and 1 |
| `=RANDARRAY(3,4)` | 3x4 array | 12 random decimals in 3 rows, 4 columns |
| `=RANDARRAY(5,1,1,100)` | 5x1 array | Five decimals between 1 and 100 |
| `=RANDARRAY(5,1,1,100,TRUE)` | 5x1 array | Five integers between 1 and 100 inclusive |
| `=RANDARRAY(10,1,1,6,TRUE)` | 10x1 array | Simulates 10 dice rolls (1-6) |
| `=RANDARRAY(52,1,1,52,TRUE)` | May have repeats! | Does NOT guarantee unique values |
| `=RANDARRAY(3,3,-1,1)` | 3x3 array | Random decimals between -1 and 1 |
| `=RANDARRAY(100,1,0,1)*10` | 100x1 array | Random 0-10 with multiplication |
| `=ROUND(RANDARRAY(5,5,0,100),2)` | 5x5 array | Rounded to 2 decimal places |
| `=SORT(RANDARRAY(10,1,1,100,TRUE))` | Sorted 10x1 | Sorted random integers |
| `=RANDARRAY(1,12,DATE(2024,1,1),DATE(2024,12,31),TRUE)` | 12 random dates | Random dates in 2024 |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Rows or columns is not a valid number | Provide positive integers for dimensions |
| #VALUE! | Min is greater than max | Ensure min <= max |
| #CALC! | Result would exceed worksheet limits | Reduce rows or columns |
| #NAME? | Function not available (Excel 2019 or earlier) | Upgrade to Excel 365 or use alternative |
| #SPILL! | Output range contains data | Clear cells where array needs to spill |

## Use Cases

### [[Monte Carlo Simulation]]
- **Implementation**: Generate thousands of random scenarios for financial modeling
- **Business Application**: Risk analysis, option pricing, portfolio optimization, and capital budgeting
- **Technical Details**: `=RANDARRAY(10000,1,MinReturn,MaxReturn)` generates 10,000 scenarios; combine with model formulas to simulate outcomes distribution

### [[Random Sampling - Survey Selection]]
- **Implementation**: Randomly select samples from a population for surveys or audits
- **Business Application**: Customer surveys, quality audits, compliance sampling, and A/B test assignments
- **Technical Details**: `=SORTBY(DataRange,RANDARRAY(ROWS(DataRange)))` shuffles data; take top N rows for random sample

### [[Test Data Generation]]
- **Implementation**: Create realistic test datasets for application development and testing
- **Business Application**: Software testing, database population, and training environment setup
- **Technical Details**: `=RANDARRAY(1000,1,1000,9999,TRUE)` generates 1000 random 4-digit IDs; combine with other functions for complete test records

### [[Game and Simulation Development]]
- **Implementation**: Generate random events, dice rolls, or card draws for games
- **Business Application**: Training simulations, educational games, and gamification elements
- **Technical Details**: `=RANDARRAY(1,2,1,6,TRUE)` simulates rolling two dice; `=RANDARRAY(1,5,1,52,TRUE)` draws 5 cards (with possible repeats)

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| RANDARRAY function | Excel 365 only | Yes |
| Excel 2019/earlier | Not available | N/A |
| All 5 parameters | Yes | Yes |
| Dynamic array output | Yes | Yes |
| Recalculation | On every change | On every change |
| Maximum array size | Limited by worksheet | Limited by worksheet |

## Tips and Best Practices

1. **Freeze values when needed** - Copy and Paste Special > Values to stop random values from changing
2. **Combine with SORT for shuffling** - `=SORTBY(data, RANDARRAY(ROWS(data)))` randomly shuffles any list
3. **Generate unique randoms carefully** - RANDARRAY can produce duplicates; use UNIQUE(RANDARRAY(...)) for dedupe, or SORTBY with INDEX for sampling without replacement
4. **Transform for other distributions** - Use NORM.INV(RANDARRAY(),...) for normal distribution, LOG for lognormal, etc.
5. **Integer mode includes both bounds** - RANDARRAY(1,1,1,6,TRUE) can return 1 OR 6 (inclusive range)
6. **Date generation works** - Min and max can be dates; use TRUE for integer to get valid date serial numbers
7. **Consider volatility impact** - RANDARRAY recalculates on any change; large arrays slow down workbooks

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[RAND]] | Single random decimal 0-1 | No parameters; one value only |
| [[RANDBETWEEN]] | Single random integer in range | Integers only; one value only |
| [[SEQUENCE]] | Generates sequential array | Deterministic, not random |
| [[SORTBY]] | Sorts by another array | Often combined with RANDARRAY for shuffling |
| [[UNIQUE]] | Removes duplicates | Used to ensure RANDARRAY produces unique values |

### Commonly Used Together

- **[[SORTBY]]** - Shuffle data randomly: SORTBY(data, RANDARRAY(ROWS(data)))
- **[[INDEX]]** - Random selection from list: INDEX(list, RANDARRAY(1,1,1,ROWS(list),TRUE))
- **[[NORM.INV]]** - Generate normally distributed values: NORM.INV(RANDARRAY(), mean, stdev)
- **[[ROUND]]** - Control decimal precision in random decimals
- **[[UNIQUE]]** - Remove duplicate random values when uniqueness needed
- **[[FILTER]]** - Filter data based on random assignment (A/B testing)

## Official Documentation

- [Microsoft: RANDARRAY function](https://support.microsoft.com/en-us/office/randarray-function-21261e55-3bec-4f72-9ac6-e4e1bce40b14)
- [Google: RANDARRAY function](https://support.google.com/docs/answer/9368169)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 365 | 2018-09 | Function introduced as part of dynamic arrays |
| Excel 365 | Ongoing | Continued support and optimization |
| Google Sheets | 2020 | Full support added |
