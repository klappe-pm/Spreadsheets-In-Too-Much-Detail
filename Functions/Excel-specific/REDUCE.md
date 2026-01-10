---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- lambda-functions
- array-functions
subTopics:
- accumulation
- functional-programming
- aggregation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Reduce Function
- Array Reduce
- Accumulator
tags:
- excel-only
- lambda
- arrays
- dynamic-arrays
- functional
---

# REDUCE

## Description

**REDUCE** accumulates a result by applying a LAMBDA function sequentially to each element in an array, carrying forward an accumulator value. It starts with an initial value, processes each array element while updating the accumulator, and returns the final accumulated result. This is the classic "fold" or "reduce" operation from functional programming.

Think of REDUCE as a running calculation where each step builds on the previous. Unlike SUM which simply adds all values, REDUCE lets you define any accumulation logic: building a product, concatenating text, finding conditional maximums, or implementing custom aggregations that don't have built-in Excel functions.

**Key concept:** REDUCE processes elements one at a time, left-to-right (for row arrays) or top-to-bottom (for column arrays). The lambda receives two parameters: the current accumulator value and the current array element. Whatever the lambda returns becomes the new accumulator for the next element. The final accumulator value is REDUCE's result.

**Platform note:** REDUCE is exclusively available in Microsoft 365 Excel and Excel 2024+. It requires the LAMBDA function. Google Sheets added REDUCE with similar syntax in 2022. This function will not work in Excel 2019 or earlier.

## Syntax

> [!f(x)] REDUCE Syntax
>
> ```
> =REDUCE(initial_value, array, lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `initial_value` | Yes | The starting value for the accumulator. This is the value before processing any array elements. Can be a number, text, or other value type. |
| `array` | Yes | The array to process. Elements are processed sequentially. |
| `lambda` | Yes | A LAMBDA function with exactly two parameters: (accumulator, current_value). Returns the new accumulator value after processing current_value. |

### Return Value

Returns the final accumulator value after processing all elements in the array.

## Examples

> [!f(x)] REDUCE Examples

### Example 1: Sum (Reimplemented)
```
=REDUCE(0, A1:A10, LAMBDA(acc, val, acc + val))
```
**Result:** Sum of A1:A10

**Explanation:** Starting at 0, each value is added to the accumulator. This replicates SUM but demonstrates REDUCE's pattern. Initial value 0 ensures empty arrays return 0.

---

### Example 2: Product of All Values
```
=REDUCE(1, A1:A10, LAMBDA(acc, val, acc * val))
```
**Result:** Product of all values (e.g., 1*2*3*4*... = PRODUCT equivalent)

**Explanation:** Starting at 1, each value multiplies the accumulator. Initial value 1 is critical - starting at 0 would make everything 0.

---

### Example 3: Concatenate Text with Separator
```
=REDUCE("", A1:A5, LAMBDA(acc, val, acc & IF(acc="", "", ", ") & val))
```
**Result:** `Apple, Banana, Cherry, Date, Elderberry`

**Explanation:** Builds a comma-separated string. The IF handles the first element (no leading comma). This is like TEXTJOIN but with custom control.

---

### Example 4: Find Maximum Value
```
=REDUCE(-9999999, Scores, LAMBDA(max, val, IF(val>max, val, max)))
```
**Result:** Maximum value in Scores

**Explanation:** Starting with very low value, each element is compared. If larger, it becomes the new max. Replicates MAX but shows conditional accumulation.

---

### Example 5: Count Values Meeting Criteria
```
=REDUCE(0, Data, LAMBDA(count, val, count + IF(val>100, 1, 0)))
```
**Result:** Count of values greater than 100

**Explanation:** Starts at 0, adds 1 for each value exceeding 100. Like COUNTIF but built with REDUCE.

---

### Example 6: Conditional Sum
```
=REDUCE(0, Values, LAMBDA(total, val, total + IF(val>0, val, 0)))
```
**Result:** Sum of positive values only

**Explanation:** Adds to total only if value is positive. Like SUMIF but with custom condition logic.

---

### Example 7: Running Factorial
```
=REDUCE(1, SEQUENCE(5), LAMBDA(fact, n, fact * n))
```
**Result:** `120` (5! = 5*4*3*2*1)

**Explanation:** SEQUENCE(5) produces {1,2,3,4,5}. REDUCE multiplies them cumulatively: 1*1=1, 1*2=2, 2*3=6, 6*4=24, 24*5=120.

---

### Example 8: String Reversal
```
=REDUCE("", MID(A1, SEQUENCE(LEN(A1)), 1), LAMBDA(rev, char, char & rev))
```
**Result:** Reversed text string

**Explanation:** Splits text into characters, then prepends each to the accumulator, building the string in reverse.

---

### Example 9: Find Last Non-Empty Value
```
=REDUCE("", A1:A100, LAMBDA(last, val, IF(val<>"", val, last)))
```
**Result:** The last non-empty value in the range

**Explanation:** Each non-empty value replaces the accumulator. After processing all elements, the accumulator holds the last non-empty one.

---

### Example 10: Build Array of Running Totals
```
=REDUCE(0, A1:A5, LAMBDA(acc, val, acc + val))
```
**Result:** Final running total (use SCAN for all intermediates)

**Explanation:** REDUCE gives only the final total. For all running totals, use SCAN instead (see Related Functions).

---

### Example 11: Nested Array Flattening Sum
```
=REDUCE(0, BYROW(A1:C10, LAMBDA(r, SUM(r))), LAMBDA(acc, rowsum, acc + rowsum))
```
**Result:** Grand total of entire range

**Explanation:** BYROW sums each row first, then REDUCE sums those row sums. Demonstrates combining LAMBDA functions.

---

### Example 12: Complex Accumulator Object (Text)
```
=REDUCE("Count:0,Sum:0", Values, LAMBDA(acc, val,
    "Count:"&(VALUE(MID(acc,7,FIND(",",acc)-7))+1)&
    ",Sum:"&(VALUE(MID(acc,FIND(",Sum:",acc)+5,100))+val)
))
```
**Result:** `Count:5,Sum:150` (example)

**Explanation:** The accumulator can be structured text holding multiple values. Parse and update multiple metrics in one pass. Complex but powerful.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Lambda doesn't have exactly 2 parameters | Lambda must be `LAMBDA(acc, val, expression)` |
| `#VALUE!` | Lambda returns wrong type for next iteration | Ensure lambda return type is compatible with initial_value |
| `#NAME?` | REDUCE or LAMBDA not recognized | Requires Excel 365/2024+; not available in earlier versions |
| `#CALC!` | Calculation error within lambda | Debug lambda with simple values first |
| `#NUM!` | Numeric overflow during accumulation | Check for runaway multiplication or very large sums |

## Use Cases

### [[Custom Aggregation Logic]]

**Scenario:** Calculate weighted average where weights and values are in separate arrays.

**Implementation:**
```
=LET(
    values, B2:B10,
    weights, C2:C10,
    sumProduct, REDUCE(0, SEQUENCE(ROWS(values)), LAMBDA(acc, i,
        acc + INDEX(values,i) * INDEX(weights,i)
    )),
    sumWeights, SUM(weights),
    sumProduct / sumWeights
)
```

**Business Application:** Financial portfolio returns, academic grade calculations, and survey weighted averages all need weighted calculations that SUMPRODUCT can handle, but REDUCE allows more complex weighting logic.

**Technical Details:** LET stores intermediate values. REDUCE iterates through indices, allowing access to both arrays. Final division produces the weighted average.

---

### [[Cumulative Validation]]

**Scenario:** Check that a sequence of transactions maintains positive balance.

**Implementation:**
```
=REDUCE(InitialBalance, Transactions, LAMBDA(balance, txn,
    IF(balance + txn < 0, "OVERDRAFT", balance + txn)
))
```

**Business Application:** Bank account simulations, inventory tracking, and resource allocation models need to verify running balances never go negative.

**Technical Details:** Once "OVERDRAFT" appears, subsequent additions produce errors. For full tracking, use SCAN to see where the overdraft occurred.

---

### [[Text Building]]

**Scenario:** Create formatted output from array data with custom separators and formatting.

**Implementation:**
```
=REDUCE("Items: ", ProductNames, LAMBDA(txt, name,
    txt & CHR(10) & "- " & name
))
```

**Business Application:** Generate formatted lists, build email bodies from data, or create structured text reports directly from array data.

**Technical Details:** CHR(10) is line break. The lambda adds bullet formatting to each item. Final text can be used in cells or passed to other functions.

## Platform Differences

### Microsoft Excel (365/2024+)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| Complex lambda logic | Full support |
| Large arrays | Up to calculation limits |
| Any accumulator type | Full support |

### Google Sheets

| Feature | Support |
|---------|---------|
| REDUCE | Available (added 2022) |
| Syntax | Same as Excel |
| Lambda support | Full support |
| Performance | May differ from Excel |

Google Sheets added REDUCE in 2022. Formulas should be portable between platforms.

### Other Platforms

REDUCE is available in:
- Microsoft Excel 365: Full support
- Microsoft Excel 2024: Full support
- Google Sheets: Full support (added 2022)
- LibreOffice Calc: Not available
- Apple Numbers: Not available
- Excel 2019 and earlier: Not available

## Tips and Best Practices

1. **Choose initial value carefully:** For sum, use 0. For product, use 1. For text concatenation, use "". Wrong initial values produce wrong results.

2. **Name lambda parameters meaningfully:** `LAMBDA(acc, val, ...)` or `LAMBDA(total, item, ...)` is clearer than `LAMBDA(a, b, ...)`.

3. **Use SCAN for intermediate values:** REDUCE only returns the final result. If you need all intermediate accumulator values, use SCAN instead.

4. **Test with small arrays first:** Debug your lambda on 2-3 elements before applying to large arrays.

5. **Consider built-in functions first:** SUM, PRODUCT, TEXTJOIN, MAX, MIN are simpler for standard aggregations. Use REDUCE for custom logic.

6. **Combine with LET for clarity:** `=LET(data, A1:A10, REDUCE(0, data, LAMBDA(a,v, a+v)))` improves readability.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SCAN]] | Returns all intermediate accumulator values | When you need running totals, not just final result |
| [[BYROW]] | Process each row, return array | When reducing rows, not entire array |
| [[BYCOL]] | Process each column, return array | When reducing columns, not entire array |
| [[MAP]] | Transform each element | When transforming, not accumulating |

### Commonly Used Together

**[[LAMBDA]]** - Required to define accumulation logic

*Every REDUCE needs a LAMBDA:*
```
=REDUCE(0, Data, LAMBDA(acc, val, acc + val))
```
The lambda defines how each element affects the accumulator.

---

**[[SCAN]]** - Get intermediate values

*Compare REDUCE vs SCAN:*
```
REDUCE(0, {1,2,3,4,5}, LAMBDA(a,v,a+v)) returns 15
SCAN(0, {1,2,3,4,5}, LAMBDA(a,v,a+v)) returns {1,3,6,10,15}
```
REDUCE: final total. SCAN: running totals.

---

**[[SEQUENCE]]** - Generate arrays to reduce

*Factorial calculation:*
```
=REDUCE(1, SEQUENCE(n), LAMBDA(f, i, f*i))
```
SEQUENCE provides the array; REDUCE multiplies through it.

---

**[[LET]]** - Store intermediate values

*Cleaner complex formulas:*
```
=LET(
    data, A1:A100,
    initial, 0,
    REDUCE(initial, data, LAMBDA(acc, val, acc + val^2))
)
```
LET makes REDUCE formulas more readable and maintainable.

---

**[[IF]]** - Conditional accumulation

*Conditional sum:*
```
=REDUCE(0, Data, LAMBDA(acc, val, acc + IF(condition, val, 0)))
```
IF inside lambda enables selective accumulation.

## Official Documentation

- **Microsoft Excel:** [REDUCE function](https://support.microsoft.com/en-us/office/reduce-function-42e39910-b345-45f3-84b8-0642b568b7cb)

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
