---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - lambda-functions
  - running-calculations
subTopics:
  - cumulative-operations
  - functional-programming
  - intermediate-results
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - scan function
  - running total
  - cumulative accumulator
tags:
  - google-sheets-only
  - lambda
  - arrays
  - running-totals
  - functional
---

# SCAN

## Description

**SCAN** is a powerful lambda-based function in Google Sheets that processes an array by applying a custom accumulator function to each element, just like REDUCE, but instead of returning only the final result, SCAN returns an array showing the accumulated value at every step. Think of it as a "running accumulation" function: while REDUCE collapses an array into a single final value, SCAN preserves the entire history of intermediate results. You provide an initial value, an array to process, and a LAMBDA that specifies how to update the accumulator with each new element. SCAN returns an array of the same size as the input, where each cell shows what the accumulator was after processing up to that point.

The primary use cases for SCAN include calculating running totals, cumulative sums, running averages, progressive products, and any scenario where you need to see how a value evolves as data is processed. For example, `=SCAN(0, A1:A10, LAMBDA(total, value, total + value))` produces a running sum where each cell shows the cumulative total up to that point. SCAN is indispensable for financial analysis (running balances), sports statistics (running scores), inventory tracking (progressive stock levels), and debugging REDUCE formulas (see intermediate values to identify problems). It brings the functional programming "scan" or "prefix sum" pattern to spreadsheets.

There are several important gotchas when working with SCAN. First, the initial value is NOT included in the output array; the output has the same number of elements as the input array, with each element showing the accumulator AFTER processing that input element. Second, like REDUCE, the LAMBDA must accept exactly two parameters: the current accumulator and the current element. Third, SCAN processes elements in row-major order (left to right, top to bottom), and the output maintains the same shape as the input. Fourth, empty cells in the input are passed to your LAMBDA, so handle them explicitly if needed. Fifth, the output array needs empty cells to spill into, just like other array functions.

From a platform perspective, SCAN is available in both Google Sheets and Microsoft Excel 365. The syntax and functionality are identical between platforms. SCAN was introduced as part of the LAMBDA function family in 2022. For users of older Excel versions without LAMBDA support, SCAN is not available, and running calculations must be done using helper columns with formulas referencing previous rows. When building cross-platform spreadsheets, SCAN formulas work seamlessly in both modern Excel and Google Sheets.

## Syntax

> [!f(x)] SCAN Syntax
>
> ```
> =SCAN(initial_value, array, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `initial_value` | Yes | The starting value for the accumulator before processing the first array element. This value is used to compute the first output but is not included in the output array itself. Type depends on your operation: 0 for running sums, 1 for running products, "" for string building, etc. |
| `array` | Yes | The array or range to process. Elements are processed in row-major order (left to right, top to bottom). Can be a range reference, named range, or array from another function. The output will have the same dimensions. |
| `lambda` | Yes | A LAMBDA function with exactly two parameters: the first receives the current accumulator value, the second receives the current array element. The LAMBDA returns the new accumulator value that will appear in the output for this position. Syntax: `LAMBDA(accumulator, element, expression)`. |

### Return Value

Returns an array with the same dimensions as the input array. Each cell contains the accumulator value AFTER processing the corresponding input element (and all prior elements). The first output cell is the result of applying the LAMBDA to the initial_value and the first element.

## Examples

> [!f(x)] SCAN Examples

### Example 1: Running Sum (Cumulative Total)
```
=SCAN(0, A1:A10, LAMBDA(total, value, total + value))
```
**Result:** An array showing running totals: if input is {1,2,3}, output is {1,3,6}

**Explanation:** Starting from 0, each value is added. First cell shows 0+1=1, second shows 1+2=3, third shows 3+3=6. This is the classic running total pattern.

---

### Example 2: Running Product
```
=SCAN(1, A1:A10, LAMBDA(product, num, product * num))
```
**Result:** Cumulative products: if input is {2,3,4}, output is {2,6,24}

**Explanation:** Starting from 1, each value is multiplied in. First: 1*2=2, second: 2*3=6, third: 6*4=24. Useful for compound growth calculations.

---

### Example 3: Running Maximum
```
=SCAN(-9999999, B2:B100, LAMBDA(max, val, IF(val > max, val, max)))
```
**Result:** Each cell shows the maximum value seen so far

**Explanation:** Starting with a very small number, the accumulator updates whenever a larger value is found. Each output cell represents the max up to that point.

---

### Example 4: Running Minimum
```
=SCAN(9999999, B2:B100, LAMBDA(min, val, IF(val < min, val, min)))
```
**Result:** Each cell shows the minimum value seen so far

**Explanation:** Starting with a very large number, the accumulator updates whenever a smaller value is found. Useful for tracking "low watermarks."

---

### Example 5: Running Count
```
=SCAN(0, A1:A50, LAMBDA(count, val, count + 1))
```
**Result:** Sequential numbers: {1, 2, 3, 4, ...}

**Explanation:** Simply increments by 1 for each element, regardless of value. Creates a row counter. The input values are ignored; only the iteration matters.

---

### Example 6: Running Count of Non-Empty Cells
```
=SCAN(0, A1:A50, LAMBDA(count, val, count + IF(val <> "", 1, 0)))
```
**Result:** Running count that only increases for non-empty cells

**Explanation:** Adds 1 only when cell is not empty. Shows cumulative count of populated cells at each position.

---

### Example 7: Running Average
```
=SCAN(0, A1:A20, LAMBDA(acc, val, acc + val)) / SCAN(0, A1:A20, LAMBDA(c, v, c + 1))
```
**Result:** Running average at each position

**Explanation:** Divides running sum by running count. Each cell shows the average of all values up to and including that point.

---

### Example 8: Bank Account Balance
```
=SCAN(1000, Transactions, LAMBDA(balance, txn, balance + txn))
```
**Result:** Running balance after each transaction, starting with 1000

**Explanation:** Starting balance of 1000. Positive transactions add, negative subtract. Each cell shows account balance at that point in time.

---

### Example 9: Cumulative Percentage of Total
```
=SCAN(0, A1:A20, LAMBDA(sum, val, sum + val)) / SUM(A1:A20)
```
**Result:** Each cell shows what fraction of the total has been accumulated so far

**Explanation:** Running sum divided by grand total. Shows progressive completion percentage. Useful for Pareto analysis.

---

### Example 10: String Building (Running Concatenation)
```
=SCAN("", A1:A5, LAMBDA(acc, val, IF(acc = "", val, acc & ", " & val)))
```
**Result:** Progressive string: {"Apple", "Apple, Banana", "Apple, Banana, Cherry", ...}

**Explanation:** Builds comma-separated string progressively. Each cell shows all values concatenated up to that point.

---

### Example 11: Fibonacci-Style Sequence
```
=SCAN({0,1}, SEQUENCE(10), LAMBDA(prev, n, {INDEX(prev,2), INDEX(prev,1)+INDEX(prev,2)}))
```
**Result:** Pairs of Fibonacci numbers at each step

**Explanation:** Accumulator is a 2-element array holding previous two values. Each step shifts and adds. Demonstrates array accumulators.

---

### Example 12: Running Difference from First Value
```
=SCAN(INDEX(A1:A20, 1), A1:A20, LAMBDA(first, val, val - first))
```
**Result:** Difference of each value from the first value

**Explanation:** First cell is 0 (first minus itself), subsequent cells show how each value differs from the first. The accumulator (first value) never changes.

---

### Example 13: Cumulative Days Between Dates
```
=SCAN(0, B2:B50, LAMBDA(acc, date, IF(acc = 0, 0, date - INDEX($B$1:B1, ROWS($B$1:B1)))))
```
**Result:** Days elapsed since previous date entry

**Explanation:** Calculates gaps between consecutive dates. First cell is 0, subsequent cells show days since prior entry.

---

### Example 14: Running Streak Counter
```
=SCAN(0, A1:A30, LAMBDA(streak, val, IF(val > 0, streak + 1, 0)))
```
**Result:** Counts consecutive positive values, resetting to 0 on non-positive

**Explanation:** Increments streak for positive values, resets to 0 otherwise. Useful for tracking winning streaks, consecutive successes, etc.

---

### Example 15: Debug a REDUCE Formula
```
=SCAN(1, A1:A10, LAMBDA(acc, val, acc * (1 + val/100)))
```
**Result:** Shows compound growth at each step (for debugging/verification)

**Explanation:** When a REDUCE formula produces unexpected results, replace REDUCE with SCAN to see all intermediate values and identify where the logic fails.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | LAMBDA does not have exactly two parameters | SCAN LAMBDA must accept accumulator and element parameters. Check parameter count. |
| `#NAME?` | LAMBDA syntax is incorrect or parameter names are invalid | Verify LAMBDA structure. Parameter names cannot be cell references or reserved words. |
| `#REF!` | Output would spill into cells with existing data | Clear cells below and to the right of the formula. SCAN output matches input array dimensions. |
| `#VALUE!` | Type mismatch in accumulation (e.g., arithmetic on text) | Ensure LAMBDA handles all possible value types, including empty cells. |
| `#ERROR!` | Array is empty | Provide a valid array with at least one element. |
| Unexpected pattern | Empty cells processed differently than expected | Add explicit handling: `IF(val="", acc, acc + val)` |
| Wrong number of results | Expecting N+1 results (including initial value) | SCAN output has same count as input; initial value is not in output. |

## Use Cases

### [[Financial Analysis: Running Account Balance]]

**Scenario:** A finance manager needs to track bank account balance over time, showing the balance after each transaction to identify when the account was at risk of overdraft.

**Implementation:**
```
=SCAN(StartingBalance, Transactions, LAMBDA(balance, txn, balance + txn))
```
Where StartingBalance is the opening balance and Transactions is the list of deposits (positive) and withdrawals (negative)

**Business Application:** Creates a complete balance history showing the account state after every transaction. Finance can identify low-balance periods, track cash flow patterns, and detect when overdraft protection would be needed. Combined with conditional formatting, balances below threshold can be highlighted.

**Technical Details:** Each transaction adjusts the running balance. Negative balances indicate overdraft. The pattern extends to multiple accounts by applying SCAN to each account's transaction column. Consider wrapping in MAX(0, balance+txn) for accounts with overdraft prevention.

---

### [[Sales Analytics: Cumulative Sales Tracking]]

**Scenario:** A sales manager needs to track cumulative sales toward monthly targets, showing progress after each deal to motivate the team and identify when targets are reached.

**Implementation:**
```
=SCAN(0, DailySales, LAMBDA(total, sale, total + sale))
=SCAN(0, DailySales, LAMBDA(total, sale, total + sale)) / MonthlyTarget
```

**Business Application:** First formula shows cumulative sales amounts. Second shows percentage toward target. The team can see real-time progress, identify when momentum builds or stalls, and celebrate hitting milestones. Visualized as a chart, it shows the sales trajectory.

**Technical Details:** The running total builds naturally. Dividing by target converts to percentage. Consider filtering to include only closed deals. For multi-rep tracking, apply SCAN to each rep's column.

---

### [[Inventory Management: Progressive Stock Levels]]

**Scenario:** A warehouse manager needs to track inventory levels over time, showing stock after each receipt and shipment to identify potential stockouts before they occur.

**Implementation:**
```
=SCAN(InitialStock, StockChanges, LAMBDA(stock, change,
  LET(newStock, stock + change,
    IF(newStock < 0, "STOCKOUT", newStock))))
```

**Business Application:** Shows inventory level after each movement. Identifies when stock would go negative, allowing proactive reordering. Historical view reveals consumption patterns and seasonal variations. Can be linked to alerts when stock falls below reorder point.

**Technical Details:** Positive changes are receipts, negative are shipments. The LET structure handles stockout detection. Consider adding MIN(newStock, MaxCapacity) for storage-limited scenarios.

---

### [[Sports Statistics: Running Scores and Stats]]

**Scenario:** A sports analyst tracks game scores quarter by quarter or play by play, showing cumulative score at each point to analyze momentum shifts and scoring patterns.

**Implementation:**
```
=SCAN(0, TeamAPoints, LAMBDA(score, pts, score + pts))
=SCAN(0, TeamAPoints, LAMBDA(score, pts, score + pts)) - SCAN(0, TeamBPoints, LAMBDA(score, pts, score + pts))
```

**Business Application:** First formula shows Team A's running score. Second shows running point differential (lead/deficit). Analysts can identify when teams pull ahead, momentum swings, and clutch performances. Charted, it tells the story of the game.

**Technical Details:** Each scoring event is processed chronologically. The differential formula shows who was winning at each point. Extend to track running shooting percentages, possession time, or other cumulative stats.

## Platform Differences

### Microsoft Excel

- **Availability:** SCAN is available in Excel 365 (Microsoft 365 subscription versions)
- **Syntax:** Identical to Google Sheets
- **Behavior:** Same functionality; returns all intermediate accumulator values
- **Not available in:** Excel 2019, Excel 2016, Excel 2021, or earlier versions
- **Alternative for older Excel:** Use helper column with formulas referencing previous row (like =B1+A2)

### Google Sheets

- **Availability:** Available in all Google Sheets (introduced 2022)
- **Specific Behavior:** This documentation focuses on Google Sheets behavior
- **Processing order:** Row-major order (left to right, top to bottom)
- **Integration:** Works with all Google Sheets functions including QUERY and other LAMBDA functions

### Key Differences

| Feature | Google Sheets | Excel 365 |
|---------|---------------|-----------|
| Availability | All users | Microsoft 365 subscribers only |
| Syntax | Identical | Identical |
| Output shape | Matches input array | Matches input array |
| Processing order | Row-major | Row-major |
| Initial value in output | No | No |
| Performance | Good | Generally slightly better |

## Tips and Best Practices

1. **Initial Value Is Not in Output:** Remember that SCAN output has the same size as input. The initial value affects the first calculation but is not itself in the output array.

2. **Use SCAN to Debug REDUCE:** If a REDUCE formula gives unexpected results, change REDUCE to SCAN (with same parameters) to see all intermediate values and identify the problem.

3. **Choose Initial Value Carefully:** Use 0 for running sums, 1 for running products, "" for string building, very large numbers for running minimums, very small numbers for running maximums.

4. **Handle Empty Cells Explicitly:** SCAN passes empty cells to your LAMBDA. Use `IF(val="", acc, ...)` to skip empties or handle them appropriately.

5. **Name Parameters Descriptively:** Use meaningful names like `runningTotal`, `cumulative`, `balance` for the accumulator to make formulas self-documenting.

6. **Consider Output Location:** SCAN produces an array the same size as input. Plan your sheet layout to accommodate the spilled results without overwriting data.

7. **Combine with Charts:** SCAN output is perfect for charting running totals, cumulative progress, or evolving metrics over time.

8. **Multi-Value Accumulators:** The accumulator can be an array to track multiple running values simultaneously. Use INDEX to extract components.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[REDUCE]] | Like SCAN but returns only the final result | When you need just the end result, not intermediate values |
| [[BYROW]] | Applies LAMBDA to each row | When aggregating within rows, not cumulative across rows |
| Running total formula | Traditional `=A1+B2` pattern | In older Excel versions without SCAN support |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the accumulation function

*SCAN requires LAMBDA to define how accumulation works:*
```
=SCAN(0, A1:A100, LAMBDA(acc, val, acc + val))
```
The LAMBDA specifies how to combine accumulator with each element.

---

**[[REDUCE]]** - Final value counterpart

*Use both for verification:*
```
=REDUCE(0, A1:A10, LAMBDA(a, v, a + v))
=INDEX(SCAN(0, A1:A10, LAMBDA(a, v, a + v)), 10)
```
Both should return the same value; useful for validation.

---

**[[LET]]** - Structure complex accumulation logic

*Use LET within LAMBDA for clarity:*
```
=SCAN(0, A1:A50, LAMBDA(acc, val,
  LET(cleaned, IF(ISNUMBER(val), val, 0), acc + cleaned)))
```
Names intermediate values for readability.

---

**[[IF]]** - Conditional accumulation

*Different logic based on conditions:*
```
=SCAN(0, A1:A50, LAMBDA(streak, val,
  IF(val > 0, streak + 1, 0)))
```
Resets streak on non-positive values.

---

**[[MAX]] / [[MIN]]** - Running extremes

*Track running maximum:*
```
=SCAN(INDEX(A1:A100,1), A1:A100, LAMBDA(max, val, MAX(max, val)))
```
Each cell shows the highest value seen so far.

---

**[[SPARKLINE]]** - Visualize SCAN results

*Mini chart of running totals:*
```
=SPARKLINE(SCAN(0, A1:A12, LAMBDA(t, v, t + v)))
```
Creates an in-cell chart of the cumulative trend.

## Official Documentation

- **Google Sheets:** [SCAN function](https://support.google.com/docs/answer/12570921)
- **Microsoft Excel:** [SCAN function](https://support.microsoft.com/en-us/office/scan-function-d58dfd11-9969-4439-b2dc-e7062724de29)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2022 | Introduced as part of the LAMBDA helper function family |
| Excel 365 | 2022 | Added with LAMBDA function ecosystem |
| Excel 2021 | N/A | Not available in perpetual license version |
| Excel Online | 2022 | Available in web version |

---

*Last updated: 2026-01-10*
