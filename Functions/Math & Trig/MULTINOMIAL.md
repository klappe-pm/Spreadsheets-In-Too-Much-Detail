---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
- combinatorics
subTopics:
- factorials
- probability
- counting-problems
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- multinomial-coefficient
- multinomial-distribution
tags:
- factorial
- combinatorics
- probability
- mathematical
- statistics
---

# MULTINOMIAL

## Description

MULTINOMIAL returns the multinomial coefficient of a set of numbers, calculated as the factorial of the sum of the arguments divided by the product of the factorials of each argument. For values n1, n2, ..., nk, MULTINOMIAL returns (n1 + n2 + ... + nk)! / (n1! * n2! * ... * nk!). This coefficient represents the number of ways to partition a set of (n1+n2+...+nk) distinct items into k groups of sizes n1, n2, ..., nk.

This function is essential for combinatorial mathematics and probability calculations. The multinomial coefficient appears in probability distributions (the multinomial distribution generalizes the binomial distribution to more than two outcomes), counting arrangements of objects where order matters within groups, and calculating coefficients in polynomial expansions. In business contexts, it supports allocation problems, quality control sampling, and market research analysis.

A critical gotcha is that all arguments must be non-negative integers (or values that truncate to non-negative integers). The function will return #NUM! for negative values. The factorial calculations can produce very large numbers quickly - MULTINOMIAL(10,10,10) produces over 5.5 trillion - so be aware of numeric overflow potential with large inputs. Also note that MULTINOMIAL of a single value always equals 1 (n!/n! = 1).

Both Excel and Google Sheets support MULTINOMIAL with consistent behavior. The function accepts multiple individual arguments or a range of values. All values are truncated to integers before calculation. The function is available in all modern versions of both platforms.

## Syntax

> [!f(x)] MULTINOMIAL Syntax
>
> ```excel
> MULTINOMIAL(number1, [number2], ...)
> ```
>
> Returns the multinomial coefficient of a set of numbers.

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| number1 | First number or range of numbers | Yes | - |
| number2, ... | Additional numbers or ranges (up to 255 arguments) | No | - |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=MULTINOMIAL(2,3)` | 10 | (2+3)!/(2!*3!) = 120/(2*6) = 10 ways to choose 2 from 5 |
| `=MULTINOMIAL(2,2,2)` | 90 | 6!/(2!*2!*2!) = 720/8 = 90 arrangements |
| `=MULTINOMIAL(1,1,1)` | 6 | 3!/(1!*1!*1!) = 6/1 = 6 permutations |
| `=MULTINOMIAL(5)` | 1 | Single value: n!/n! = 1 |
| `=MULTINOMIAL(3,0)` | 1 | Zero in arguments: 3!/(3!*0!) = 6/6 = 1 |
| `=MULTINOMIAL(1,2,3,4)` | 12600 | 10!/(1!*2!*3!*4!) = 3628800/288 |
| `=MULTINOMIAL(10,10)` | 184756 | 20!/(10!*10!) = binomial coefficient C(20,10) |
| `=MULTINOMIAL(A1:A5)` | Varies | Calculates from range of values |
| `=MULTINOMIAL(2,3,5)` | 2520 | 10!/(2!*3!*5!) = 3628800/1440 |
| `=MULTINOMIAL(4,4,4,4)` | 63063000 | 16!/(4!^4) - large symmetric grouping |
| `=MULTINOMIAL(6,6)` | 924 | Equivalent to COMBIN(12,6) |
| `=MULTINOMIAL(1,1,1,1,1,1)` | 720 | 6!/(1!^6) = 720 - all permutations of 6 items |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #NUM! | One or more arguments is negative | Ensure all values are non-negative |
| #VALUE! | Non-numeric arguments provided | Convert text values to numbers |
| #NUM! | Result exceeds numeric limits | Use smaller input values; result too large |

## Use Cases

### [[Probability - Multinomial Distribution]]
- **Implementation**: Calculate probabilities in multi-outcome experiments
- **Business Application**: Market share analysis, customer preference distribution, and quality sampling
- **Technical Details**: For a multinomial probability, use `=MULTINOMIAL(outcomes) * PRODUCT(probability^outcomes)` where outcomes are counts and probabilities are per-category probabilities

### [[Arrangement Counting - Word Permutations]]
- **Implementation**: Count distinct arrangements of letters in words with repeated letters
- **Business Application**: Password complexity analysis, code generation possibilities, and serialization schemes
- **Technical Details**: `=MULTINOMIAL(LetterCounts)` gives total arrangements; for "MISSISSIPPI", `=MULTINOMIAL(1,4,4,2)` = 34650 distinct arrangements (1 M, 4 I, 4 S, 2 P)

### [[Resource Allocation - Committee Selection]]
- **Implementation**: Count ways to partition people into groups of specified sizes
- **Business Application**: Team formation, shift scheduling, and project assignment optimization
- **Technical Details**: `=MULTINOMIAL(GroupSizes)` gives ways to form committees of given sizes from total pool; order within groups doesn't matter but group identity does

### [[Statistical Sampling - Quality Control]]
- **Implementation**: Calculate combinations in acceptance sampling schemes
- **Business Application**: Determining sample distributions for multi-category defect analysis
- **Technical Details**: When sampling n items with k defect types, `=MULTINOMIAL(CountsByType)` calculates number of ways to observe that specific outcome combination

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| MULTINOMIAL function | Yes (all versions) | Yes |
| Range arguments | Yes | Yes |
| Multiple separate arguments | Yes (up to 255) | Yes |
| Truncation of decimals | Yes | Yes |
| Maximum practical size | Limited by numeric overflow | Limited by numeric overflow |
| Zero handling | Returns valid result | Returns valid result |

## Tips and Best Practices

1. **Understand the formula** - MULTINOMIAL(a,b,c) = (a+b+c)!/(a!*b!*c!); knowing this helps verify results
2. **Single argument always returns 1** - MULTINOMIAL(n) = n!/n! = 1; useful for validation
3. **Connection to COMBIN** - MULTINOMIAL(a,b) equals COMBIN(a+b, a) equals COMBIN(a+b, b)
4. **Watch for overflow** - Large inputs quickly exceed Excel's numeric limits (around 10^308)
5. **Use ranges for many values** - Instead of many arguments, use a range reference for cleaner formulas
6. **Zero is valid** - MULTINOMIAL handles zeros correctly; 0! = 1 by convention
7. **Truncation applies** - Non-integer values are truncated to integers before calculation

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[COMBIN]] | Calculates combinations C(n,k) | Two-outcome case only; MULTINOMIAL(a,b) = COMBIN(a+b,a) |
| [[COMBINA]] | Combinations with repetition | Different counting problem |
| [[PERMUT]] | Permutations without repetition | Orders all items, doesn't group |
| [[FACT]] | Single factorial | Building block: MULTINOMIAL uses FACT internally |
| [[FACTDOUBLE]] | Double factorial | Different factorial variant |

### Commonly Used Together

- **[[FACT]]** - Verify MULTINOMIAL by calculating (sum)! / product of factorials manually
- **[[PRODUCT]]** - Calculate product of individual factorials or probabilities
- **[[POWER]]** - Raise probabilities to outcome counts in distribution calculations
- **[[SUM]]** - Compute total count (sum of arguments) for context
- **[[COMBIN]]** - Alternative for two-group partitioning problems
- **[[LOG]]** - Calculate log-multinomial for numerical stability with large values

## Official Documentation

- [Microsoft: MULTINOMIAL function](https://support.microsoft.com/en-us/office/multinomial-function-6fa6c1f8-5e54-4bce-a1d6-30e1f6f2a3f1)
- [Google: MULTINOMIAL function](https://support.google.com/docs/answer/3093406)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2000 | 1999-06 | Function introduced with Analysis ToolPak |
| Excel 2007 | 2006-11 | Integrated into standard function library |
| Excel 365 | Ongoing | Full support |
| Google Sheets | 2006 | Full support from launch |
