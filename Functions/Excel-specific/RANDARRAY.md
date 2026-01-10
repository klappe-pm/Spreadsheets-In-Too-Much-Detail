---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- dynamic-arrays
- generation
subTopics:
- random-numbers
- array-generation
- simulation
- testing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Random Array
- Random Number Generator
- Random Matrix
- RAND Array
tags:
- random
- dynamic-arrays
- generation
- simulation
- excel-365
- testing
---

# RANDARRAY

## Description

**RANDARRAY** generates an array of random numbers with customizable dimensions, range, and type (decimal or integer). One formula fills an entire grid with random values, from a simple 10-row column of decimals to a 100x100 matrix of integers between 1 and 1000. The function provides far more control than RAND() or RANDBETWEEN(), which only fill single cells and require copying to create arrays.

The function is essential for Monte Carlo simulations, test data generation, statistical sampling, and any scenario requiring multiple random values. Specify the exact dimensions, minimum value, maximum value, and whether you need whole numbers or decimals. The array automatically spills to fill the required cells, recalculating with new random values whenever the workbook recalculates.

**RANDARRAY in Google Sheets:** Google Sheets added RANDARRAY in 2022, providing full compatibility with Excel's implementation. The syntax, parameters, and behavior are identical. Formulas work interchangeably between platforms. Both platforms recalculate random values on sheet changes, though you can wrap RANDARRAY in special techniques to "freeze" values if needed.

**Volatility consideration:** Like RAND() and RANDBETWEEN(), RANDARRAY is volatile, meaning it recalculates whenever any cell in the workbook changes. For large arrays or performance-sensitive workbooks, consider copying and pasting values to freeze the random numbers, or use calculation settings strategically. In simulations requiring reproducible results, you may need to capture results before they change.

## Syntax

> [!f(x)] RANDARRAY Syntax
>
> ```
> =RANDARRAY([rows], [columns], [min], [max], [integer])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `rows` | No | Number of rows to return. Default is 1. Must be positive integer. |
| `columns` | No | Number of columns to return. Default is 1. Must be positive integer. |
| `min` | No | Minimum value in the random range. Default is 0. Can be any number. |
| `max` | No | Maximum value in the random range. Default is 1. Must be greater than min. |
| `integer` | No | TRUE for whole numbers, FALSE for decimals. Default is FALSE (decimals). |

### Return Value

Returns an array of random numbers with specified dimensions. Values are uniformly distributed between min and max (inclusive for integers, nearly inclusive for decimals). Decimals have approximately 15 digits of precision.

## Examples

> [!f(x)] RANDARRAY Examples

### Example 1: Single Random Decimal (Default)
```
=RANDARRAY()
```
**Result:** Single random decimal between 0 and 1

**Explanation:** All parameters default: 1 row, 1 column, minimum 0, maximum 1, decimal. Equivalent to RAND() but using new syntax.

---

### Example 2: Column of Random Decimals
```
=RANDARRAY(10)
```
**Result:** 10 random decimals between 0 and 1 in a column

**Explanation:** Only rows specified; other parameters default. Creates vertical array of 10 values. Each recalculation produces new values.

---

### Example 3: Grid of Random Decimals
```
=RANDARRAY(5, 4)
```
**Result:** 5x4 grid (20 cells) of random decimals between 0 and 1

**Explanation:** Two-dimensional random array. Each cell contains independent random value.

---

### Example 4: Custom Range Decimals
```
=RANDARRAY(10, 1, 50, 100)
```
**Result:** 10 random decimals between 50 and 100

**Explanation:** Numbers like 72.384, 91.256, 56.829. Useful for test data in specific ranges.

---

### Example 5: Random Integers
```
=RANDARRAY(10, 1, 1, 100, TRUE)
```
**Result:** 10 random whole numbers between 1 and 100

**Explanation:** Fifth parameter TRUE forces integers. Replaces multiple RANDBETWEEN() calls with single formula.

---

### Example 6: Random Integers (Dice Simulation)
```
=RANDARRAY(1, 6, 1, 6, TRUE)
```
**Result:** Six random dice rolls (values 1-6)

**Explanation:** Simulates rolling six dice. Each cell is an independent die roll. Common for game simulations or probability demonstrations.

---

### Example 7: Random Percentages
```
=RANDARRAY(5, 3, 0, 1)
```
**Result:** 5x3 array of random decimals between 0 and 1 (format as %)

**Explanation:** Random percentages for testing or simulation. Format cells as percentage to display as 0%-100%.

---

### Example 8: Large Integer Range
```
=RANDARRAY(100, 1, 10000, 99999, TRUE)
```
**Result:** 100 random 5-digit numbers

**Explanation:** Generate test invoice numbers, order IDs, or other large integers. Useful for creating realistic test datasets.

---

### Example 9: Random Negative Numbers
```
=RANDARRAY(10, 1, -100, 100, TRUE)
```
**Result:** 10 integers between -100 and 100

**Explanation:** Min can be negative for ranges that span zero. Useful for financial simulations with gains and losses.

---

### Example 10: Random Dates (Current Year)
```
=RANDARRAY(10, 1, DATE(2024,1,1), DATE(2024,12,31), TRUE)
```
**Result:** 10 random dates in 2024

**Explanation:** Excel dates are integers. RANDARRAY with integer mode generates random dates. Format cells as dates for display.

---

### Example 11: Random Times
```
=RANDARRAY(10, 1, 0.375, 0.75)
```
**Result:** 10 random times between 9:00 AM and 6:00 PM

**Explanation:** Time in Excel is decimal portion of day. 0.375 = 9:00 AM, 0.75 = 6:00 PM. Format as time for readable display.

---

### Example 12: Weighted Simulation (Coin Flips)
```
=IF(RANDARRAY(100, 1)>0.5, "Heads", "Tails")
```
**Result:** 100 simulated coin flips

**Explanation:** Random values compared to 0.5 create 50/50 split. Adjust threshold for weighted outcomes (0.7 gives 70% Tails).

---

### Example 13: Normal Distribution Approximation
```
=AVERAGE(RANDARRAY(12, 1, 0, 1))*10
```
**Result:** Value approximating normal distribution (by Central Limit Theorem)

**Explanation:** Average of multiple uniform random values approaches normal distribution. Repeat formula for many samples. For true normal, use NORM.INV(RAND()).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Min is greater than max | Ensure max parameter is larger than min |
| `#VALUE!` | Rows or columns is zero or negative | Both dimensions must be positive integers |
| `#SPILL!` | Cells in output range aren't empty | Clear the cells where results need to spill |
| `#CALC!` | Requested array is too large | Reduce dimensions; Excel has array size limits |
| `Non-integer despite TRUE` | Rounding display issue | Values are integers; adjust cell number format |
| `Same values repeatedly` | Recalculation paused | Press F9 or enable automatic calculation |

## Use Cases

### [[Monte Carlo Simulation]]

**Scenario:** Estimate project completion time given uncertain task durations. Each task has optimistic, likely, and pessimistic estimates.

**Implementation:**
```
=LET(
  scenarios, 1000,
  task1, RANDARRAY(scenarios, 1, 5, 15, TRUE),
  task2, RANDARRAY(scenarios, 1, 10, 30, TRUE),
  task3, RANDARRAY(scenarios, 1, 8, 20, TRUE),
  totals, task1 + task2 + task3,
  VSTACK(
    AVERAGE(totals),
    PERCENTILE.INC(totals, 0.95)
  )
)
```

**Business Application:** Project managers see expected duration and 95th percentile worst case. Informs buffer planning and deadline setting.

**Technical Details:** Run thousands of scenarios with random durations. Aggregate results for probability-based estimates. Recalculates with new random values; save results if reproducibility needed.

---

### [[Test Data Generation]]

**Scenario:** QA team needs 500 realistic test records with random but valid order IDs, quantities, prices, and dates.

**Implementation:**
```
Order IDs: ="ORD-"&TEXT(RANDARRAY(500,1,10000,99999,TRUE),"00000")
Quantities: =RANDARRAY(500, 1, 1, 100, TRUE)
Prices: =ROUND(RANDARRAY(500, 1, 9.99, 999.99), 2)
Order Dates: =RANDARRAY(500, 1, TODAY()-365, TODAY(), TRUE)
```

**Business Application:** Instantly generate test datasets without manually creating or importing data. Test formulas, reports, and systems with realistic volumes.

**Technical Details:** Combine RANDARRAY with TEXT for formatted IDs. Use ROUND for realistic currency values. Date range ensures recent dates.

---

### [[Random Sampling]]

**Scenario:** Select 50 random customers from a 10,000-customer database for survey participation.

**Implementation:**
```
=LET(
  data, CustomerTable[Name],
  n, ROWS(data),
  sample_size, 50,
  random_ranks, RANDARRAY(n),
  TAKE(SORTBY(data, random_ranks), sample_size)
)
```

**Business Application:** Unbiased random selection for surveys, audits, quality checks, or prize drawings. Every customer has equal selection probability.

**Technical Details:** Assign random value to each row, sort by that value, take top N. True random sampling without replacement. Recalculates to new sample on sheet change.

---

### [[Statistical Distribution Simulation]]

**Scenario:** Teaching statistics, demonstrate how sample means approach normal distribution regardless of underlying distribution (Central Limit Theorem).

**Implementation:**
```
=LET(
  sample_size, 30,
  num_samples, 100,
  uniform_samples, RANDARRAY(sample_size, num_samples, 0, 100),
  sample_means, BYCOL(uniform_samples, LAMBDA(col, AVERAGE(col))),
  sample_means
)
```

**Business Application:** Educational demonstrations, statistical training, or validating analytical methods through simulation.

**Technical Details:** 100 columns each with 30 random values. BYCOL calculates mean of each column. Resulting 100 means approximate normal distribution.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Volatility:** Recalculates on any workbook change
- **Maximum size:** Limited by calculation engine memory
- **Random quality:** High-quality pseudorandom generator

### Google Sheets
- **Availability:** Added in 2022
- **Volatility:** Same recalculation behavior
- **Maximum size:** Similar practical limits
- **Random quality:** Comparable pseudorandom quality

### Key Differences
RANDARRAY works identically across both platforms. The main difference is in older Excel versions (2019 and earlier) where the function doesn't exist.

### Alternatives for Older Excel
Before RANDARRAY, generating random arrays required:
- RAND() or RANDBETWEEN() in each cell, copied to fill range
- Array formulas with INDEX and ROWS for random selection
- VBA for custom random generation

## Tips and Best Practices

1. **Remember volatility:** RANDARRAY recalculates constantly. Copy/Paste Values if you need to freeze results for analysis or reporting.

2. **Integers for categories:** Use TRUE for the integer parameter when generating categorical codes, IDs, or any discrete values.

3. **Date generation:** Excel dates are integers. Set min/max as DATE() values with integer=TRUE for random dates.

4. **Reproducibility:** For reproducible simulations, immediately copy results to values before any recalculation, or consider VBA with seed values.

5. **Combine with SORTBY for shuffling:** `=SORTBY(data, RANDARRAY(ROWS(data)))` randomly reorders any data.

6. **Performance awareness:** Large RANDARRAY formulas can slow workbooks. Limit size or convert to values after generation.

7. **Weighted distributions:** For non-uniform distributions, use RANDARRAY with NORM.INV, LOGNORM.INV, or other inverse distribution functions.

8. **Testing edge cases:** Generate test data at boundaries by setting min=max for constants or narrow ranges for specific value testing.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[RAND]] | Single random decimal 0-1 | When you need just one random value in legacy format |
| [[RANDBETWEEN]] | Single random integer in range | When you need one integer and prefer simpler syntax |
| [[SEQUENCE]] | Sequential number array | When you need ordered numbers, not random |
| [[MAKEARRAY]] | Custom array via LAMBDA | When random logic is complex or non-uniform |

### Commonly Used Together

**[[SORTBY]]** - Randomize existing data order

*Shuffle a list randomly:*
```
=SORTBY(A1:A100, RANDARRAY(100))
```
Sort by random values to randomize order.

---

**[[IF]]** - Convert to categories

*Random category assignment:*
```
=IF(RANDARRAY(100,1)>0.7, "Group A", "Group B")
```
30% Group A, 70% Group B random assignment.

---

**[[ROUND]]** - Control decimal precision

*Random prices with 2 decimals:*
```
=ROUND(RANDARRAY(50, 1, 10, 100), 2)
```
Random values between 10.00 and 100.00.

---

**[[INDEX]]** - Random selection from list

*Pick random items from list:*
```
=INDEX(ProductList, RANDARRAY(5, 1, 1, ROWS(ProductList), TRUE))
```
Randomly select 5 products from a list.

---

**[[NORM.INV]]** - Normal distribution

*Normally distributed random values:*
```
=NORM.INV(RANDARRAY(100, 1), 100, 15)
```
100 values with mean 100, std dev 15.

---

**[[TAKE]]** - Limit random results

*Top N random values:*
```
=TAKE(SORTBY(Data, RANDARRAY(ROWS(Data))), 10)
```
Random sample of 10 from Data.

## Official Documentation

- **Microsoft Excel:** [RANDARRAY function](https://support.microsoft.com/en-us/office/randarray-function-21261e55-3bec-4885-86a6-8b0a47fd4d33)
- **Google Sheets:** [RANDARRAY function](https://support.google.com/docs/answer/10534375)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2019 (Insider), 2020 (General) | Part of dynamic array functions release |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for web | 2020 | Full feature parity |
| Google Sheets | 2022 | Added to match Excel functionality |
| Excel 2019 | Not available | Must use RAND/RANDBETWEEN with copy/paste |

---

*Last updated: 2026-01-10*
