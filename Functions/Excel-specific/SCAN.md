---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- lambda-functions
- array-functions
subTopics:
- running-calculations
- functional-programming
- cumulative-operations
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Scan Function
- Running Total
- Cumulative Array
tags:
- excel-only
- lambda
- arrays
- dynamic-arrays
- functional
---

# SCAN

## Description

**SCAN** creates an array of intermediate accumulation values by applying a LAMBDA function sequentially to each element, like REDUCE but returning all intermediate results instead of just the final value. If REDUCE is a running total that only shows the final sum, SCAN is the same running total showing every step along the way.

SCAN is essential for cumulative calculations: running totals, running averages, running maximums, or any scenario where you need to see how an accumulated value changes with each successive element. Financial running balances, cumulative sums, and progressive calculations all benefit from SCAN.

**Key distinction from REDUCE:** Both SCAN and REDUCE process arrays with an accumulator, but they return different things. REDUCE returns one value (the final accumulator). SCAN returns an array where each element is the accumulator's state after processing that many elements. For an array of 10 elements, REDUCE returns 1 value; SCAN returns 10 values.

**Platform note:** SCAN is exclusively available in Microsoft 365 Excel and Excel 2024+. It requires the LAMBDA function. Google Sheets added SCAN with similar syntax in 2022. This function will not work in Excel 2019 or earlier.

## Syntax

> [!f(x)] SCAN Syntax
>
> ```
> =SCAN(initial_value, array, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `initial_value` | Yes | The starting value for the accumulator. This value is used before processing the first element but is NOT included in the output array. |
| `array` | Yes | The array to process. Each element is processed sequentially. |
| `lambda` | Yes | A LAMBDA function with exactly two parameters: (accumulator, current_value). Returns the new accumulator value after processing current_value. |

### Return Value

Returns an array with the same dimensions as the input array, where each element is the accumulator's value after processing that element and all preceding elements.

## Examples

> [!f(x)] SCAN Examples

### Example 1: Running Total (Cumulative Sum)
```
=SCAN(0, {10,20,30,40,50}, LAMBDA(acc, val, acc + val))
```
**Result:** `{10, 30, 60, 100, 150}`

**Explanation:** Starting at 0: 0+10=10, 10+20=30, 30+30=60, 60+40=100, 100+50=150. Each result shows the running total at that point.

---

### Example 2: Running Product
```
=SCAN(1, {2,3,4,5}, LAMBDA(acc, val, acc * val))
```
**Result:** `{2, 6, 24, 120}`

**Explanation:** Starting at 1: 1*2=2, 2*3=6, 6*4=24, 24*5=120. Shows factorial-like progression.

---

### Example 3: Bank Balance with Transactions
```
=SCAN(1000, Transactions, LAMBDA(balance, txn, balance + txn))
```
Where Transactions = {-50, 200, -75, -100, 500}

**Result:** `{950, 1150, 1075, 975, 1475}`

**Explanation:** Starting balance $1000. Each result shows the balance after that transaction. Perfect for account reconciliation.

---

### Example 4: Running Maximum
```
=SCAN(-9999999, Values, LAMBDA(max, val, IF(val>max, val, max)))
```
**Result:** Running maximum - each position shows highest value seen so far

**Explanation:** At each position, the result is the maximum of all values up to and including that position.

---

### Example 5: Running Minimum
```
=SCAN(9999999, Values, LAMBDA(min, val, IF(val<min, val, min)))
```
**Result:** Running minimum - each position shows lowest value seen so far

**Explanation:** The inverse of running maximum. Each element shows the minimum encountered up to that point.

---

### Example 6: Running Average (using count)
```
=SCAN(0, A1:A10, LAMBDA(sum, val, sum + val)) / SEQUENCE(ROWS(A1:A10))
```
**Result:** Running average at each position

**Explanation:** Running sum divided by count (via SEQUENCE). Position 3 shows average of first 3 values.

---

### Example 7: String Accumulation
```
=SCAN("", {"A","B","C","D"}, LAMBDA(str, char, str & char))
```
**Result:** `{"A", "AB", "ABC", "ABCD"}`

**Explanation:** Each position shows all characters concatenated up to that point. Demonstrates text accumulation.

---

### Example 8: Detect First Negative Balance
```
=SCAN(1000, Transactions, LAMBDA(bal, txn,
    IF(bal<0, "OVERDRAFT", IF(bal+txn<0, "OVERDRAFT", bal+txn))
))
```
**Result:** Running balances until overdraft, then "OVERDRAFT" for remaining positions

**Explanation:** Once balance goes negative, it stays flagged. Shows exactly where the overdraft occurred.

---

### Example 9: Running Count of Positives
```
=SCAN(0, Data, LAMBDA(count, val, count + IF(val>0, 1, 0)))
```
**Result:** Running count of positive values

**Explanation:** At each position, shows how many positive values have been encountered so far.

---

### Example 10: Fibonacci Sequence Generation
```
=DROP(SCAN({0,1}, SEQUENCE(10), LAMBDA(pair, n,
    HSTACK(INDEX(pair,1,2), SUM(pair))
)), , 1)
```
**Result:** First 11 Fibonacci numbers

**Explanation:** Complex example using array as accumulator. Each step shifts the pair and adds new sum.

---

### Example 11: Running Percentage of Total
```
=SCAN(0, Values, LAMBDA(sum, val, sum + val)) / SUM(Values) * 100
```
**Result:** Cumulative percentage at each position

**Explanation:** Shows what percentage of the total has been accumulated at each point. Useful for Pareto analysis.

---

### Example 12: Compound Interest Over Time
```
=SCAN(Principal, SEQUENCE(Years), LAMBDA(balance, year, balance * (1 + Rate)))
```
**Result:** Account balance at end of each year

**Explanation:** Shows compound growth year by year. Each position is the balance after that year's interest.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Lambda doesn't have exactly 2 parameters | Lambda must be `LAMBDA(acc, val, expression)` |
| `#VALUE!` | Lambda returns incompatible type | Ensure return type is consistent across all iterations |
| `#NAME?` | SCAN or LAMBDA not recognized | Requires Excel 365/2024+; not available in earlier versions |
| `#CALC!` | Calculation error within lambda | Debug with smaller array first |
| `#SPILL!` | No room for result array | Clear cells in the spill range; result array matches input size |

## Use Cases

### [[Running Bank Balance]]

**Scenario:** Show account balance after each transaction for reconciliation.

**Implementation:**
```
=SCAN(OpeningBalance, Transactions, LAMBDA(bal, txn, bal + txn))
```

**Business Application:** Bank statements, credit card reconciliation, and cash flow tracking all need running balances. SCAN creates them dynamically without helper columns.

**Technical Details:** The opening balance is the initial_value, so it's not in the output. The first output is the balance after the first transaction.

---

### [[Cumulative Sales Targets]]

**Scenario:** Track year-to-date sales against cumulative targets.

**Implementation:**
```
=LET(
    ytdSales, SCAN(0, MonthlySales, LAMBDA(a,v, a+v)),
    ytdTargets, SCAN(0, MonthlyTargets, LAMBDA(a,v, a+v)),
    ytdSales - ytdTargets
)
```

**Business Application:** Sales dashboards showing YTD performance versus YTD targets. Each month shows cumulative variance.

**Technical Details:** Run SCAN on both sales and targets, then subtract. LET keeps the formula readable. Result shows cumulative variance month by month.

---

### [[Running Statistical Measures]]

**Scenario:** Calculate running averages for time series smoothing.

**Implementation:**
```
=LET(
    runningSum, SCAN(0, Data, LAMBDA(s,v, s+v)),
    count, SEQUENCE(ROWS(Data)),
    runningSum / count
)
```

**Business Application:** Stock price smoothing, trend analysis, and moving average calculations. Shows how the average evolves as data accumulates.

**Technical Details:** Divide running sum by position count. For weighted running averages, modify the accumulator to track both sum and weighted sum.

## Platform Differences

### Microsoft Excel (365/2024+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Complex lambda logic | Full support |
| Array accumulators | Full support |
| Large arrays | Up to calculation limits |

### Google Sheets

| Feature | Support |
|---------|---------|
| SCAN | Available (added 2022) |
| Syntax | Same as Excel |
| Lambda support | Full support |
| Performance | May differ from Excel |

Google Sheets added SCAN in 2022. Formulas should be portable between platforms.

### Other Platforms

SCAN is available in:
- Microsoft Excel 365: Full support
- Microsoft Excel 2024: Full support
- Google Sheets: Full support (added 2022)
- LibreOffice Calc: Not available
- Apple Numbers: Not available
- Excel 2019 and earlier: Not available

## Tips and Best Practices

1. **Remember initial_value isn't in output:** SCAN returns an array the same size as input. The initial_value affects calculations but isn't shown.

2. **Use REDUCE for final-value-only:** If you only need the end result, REDUCE is more appropriate and clearer in intent.

3. **Combine with other array operations:** `MAX(SCAN(...))` finds the peak running value; `MIN(SCAN(...))` finds the trough.

4. **Match array dimensions:** For 2D arrays, SCAN processes in row-major order (left-to-right, top-to-bottom).

5. **Debug with REDUCE:** Test your lambda logic with REDUCE first (simpler output), then switch to SCAN for full results.

6. **Consider memory for large arrays:** SCAN outputs an array equal in size to the input. Very large arrays may impact performance.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[REDUCE]] | Returns only final accumulated value | When you only need the end result |
| [[MAP]] | Transform each element independently | When no accumulation is needed |
| [[BYROW]] | Process each row independently | When accumulating within rows, not across entire array |

### Commonly Used Together

**[[LAMBDA]]** - Required to define accumulation logic

*Every SCAN needs a LAMBDA:*
```
=SCAN(0, Data, LAMBDA(acc, val, acc + val))
```
The lambda defines how each element affects the accumulator.

---

**[[REDUCE]]** - Final value counterpart

*Compare SCAN vs REDUCE:*
```
SCAN(0, {1,2,3,4,5}, LAMBDA(a,v,a+v)) returns {1,3,6,10,15}
REDUCE(0, {1,2,3,4,5}, LAMBDA(a,v,a+v)) returns 15
```
Same logic, different outputs.

---

**[[SEQUENCE]]** - Generate position numbers

*Running average calculation:*
```
=SCAN(0, Values, LAMBDA(s,v,s+v)) / SEQUENCE(ROWS(Values))
```
SEQUENCE provides denominators for running average.

---

**[[MAX]] / [[MIN]]** - Find extremes in running values

*Find peak running balance:*
```
=MAX(SCAN(StartBalance, Transactions, LAMBDA(b,t, b+t)))
```
Wrapping SCAN in MAX finds the highest balance achieved.

---

**[[LET]]** - Named intermediate calculations

*Complex running calculations:*
```
=LET(
    running, SCAN(0, Data, LAMBDA(a,v,a+v)),
    total, REDUCE(0, Data, LAMBDA(a,v,a+v)),
    running / total * 100
)
```
LET makes SCAN formulas more readable.

## Official Documentation

- **Microsoft Excel:** [SCAN function](https://support.microsoft.com/en-us/office/scan-function-d58dfd11-9969-4439-b2dc-e7062724de29)

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
