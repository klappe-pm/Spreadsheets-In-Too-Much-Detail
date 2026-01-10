---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- lambda-functions
- array-functions
subTopics:
- element-wise-operations
- functional-programming
- array-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Map Function
- Array Map
- Element-wise Apply
tags:
- excel-only
- lambda
- arrays
- dynamic-arrays
- functional
---

# MAP

## Description

**MAP** applies a LAMBDA function to each element in one or more arrays, returning an array of results. It is the element-wise transformation function - whatever calculation your lambda performs on individual values gets applied to every element in the input array(s), producing an output array of the same dimensions.

MAP brings the functional programming concept of "mapping" to Excel. In programming terms, it transforms each element through a function without explicit loops. Need to apply a custom calculation to every cell in a range? Rather than copying a formula down and across, MAP does it in a single formula that maintains array structure.

**Key capability:** MAP can accept multiple arrays of the same dimensions. When you provide multiple arrays, your lambda receives one value from each array at corresponding positions. This enables element-wise operations between arrays, like adding corresponding elements or comparing values at the same positions.

**Platform note:** MAP is exclusively available in Microsoft 365 Excel and Excel 2024+. It requires the LAMBDA function. Google Sheets added MAP with similar syntax in 2022. This function will not work in Excel 2019 or earlier.

## Syntax

> [!f(x)] MAP Syntax
>
> ```
> =MAP(array1, [array2, ...], lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first array to process. Each element is passed to the lambda function. |
| `array2, ...` | No | Additional arrays (must have same dimensions as array1). Elements from corresponding positions are passed together to the lambda. |
| `lambda` | Yes | A LAMBDA function with one parameter per input array. Returns the transformed value for each position. |

### Return Value

Returns an array with the same dimensions as the input array(s), where each element is the result of applying the lambda to the corresponding element(s) from the input array(s).

## Examples

> [!f(x)] MAP Examples

### Example 1: Square Each Element
```
=MAP(A1:A10, LAMBDA(x, x^2))
```
**Result:** Array of squared values

**Explanation:** The simplest MAP pattern - apply a calculation to each element. Each value in A1:A10 is squared in the result.

---

### Example 2: Apply Custom Text Transformation
```
=MAP(Names, LAMBDA(name, PROPER(TRIM(name))))
```
**Result:** Cleaned and properly capitalized names

**Explanation:** TRIM removes extra spaces, PROPER capitalizes. This cleans an entire name column in one formula.

---

### Example 3: Conditional Transformation
```
=MAP(Scores, LAMBDA(s, IF(s>=60, "Pass", "Fail")))
```
**Result:** Array of "Pass" or "Fail" based on each score

**Explanation:** The lambda can contain any logic. Here, each score is converted to a pass/fail label.

---

### Example 4: Element-wise Addition of Two Arrays
```
=MAP(Array1, Array2, LAMBDA(a, b, a+b))
```
**Result:** Array where each element is sum of corresponding elements

**Explanation:** With two arrays, lambda receives pairs of values from matching positions. This adds them element-wise.

---

### Example 5: Compare Arrays Element-wise
```
=MAP(Actual, Budget, LAMBDA(a, b, a-b))
```
**Result:** Variance array (actual minus budget for each position)

**Explanation:** Calculate differences between two aligned arrays. Perfect for variance analysis where each position represents a category.

---

### Example 6: Percentage Calculation from Two Arrays
```
=MAP(Values, Totals, LAMBDA(v, t, ROUND(v/t*100, 1)))
```
**Result:** Percentage each value represents of its corresponding total

**Explanation:** When values and totals are parallel arrays, MAP calculates percentages position-by-position.

---

### Example 7: Apply Custom Business Logic
```
=MAP(Quantities, Prices, LAMBDA(qty, price,
    IF(qty>100, qty*price*0.9, qty*price)
))
```
**Result:** Extended prices with bulk discount applied

**Explanation:** Complex business logic applied element-wise. Orders over 100 units get 10% discount.

---

### Example 8: Nested Functions
```
=MAP(Dates, LAMBDA(d, TEXT(d, "mmmm yyyy")))
```
**Result:** Array of formatted date strings

**Explanation:** Each date is formatted to show month name and year. FORMAT/TEXT functions work within MAP.

---

### Example 9: Error Handling Per Element
```
=MAP(Data, LAMBDA(x, IFERROR(1/x, "Div/0")))
```
**Result:** Reciprocals with division by zero handled

**Explanation:** IFERROR inside the lambda handles errors element-by-element, not for the whole array.

---

### Example 10: Three-Array Operation
```
=MAP(Width, Height, Depth, LAMBDA(w, h, d, w*h*d))
```
**Result:** Volume calculations from three dimension arrays

**Explanation:** MAP scales to any number of arrays. Here, three arrays of measurements produce volume calculations.

---

### Example 11: Lookup Within MAP
```
=MAP(ProductCodes, LAMBDA(code,
    IFERROR(VLOOKUP(code, PriceTable, 2, FALSE), "Not Found")
))
```
**Result:** Prices looked up for each product code

**Explanation:** VLOOKUP inside MAP performs individual lookups for each element. Handles missing codes gracefully.

---

### Example 12: Dynamic Grade Assignment
```
=MAP(Scores, LAMBDA(s,
    IFS(s>=90,"A", s>=80,"B", s>=70,"C", s>=60,"D", TRUE,"F")
))
```
**Result:** Letter grades for each score

**Explanation:** IFS provides multi-level classification. Each score is independently converted to its letter grade.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Arrays have different dimensions | All arrays must have same rows and columns |
| `#VALUE!` | Lambda parameter count doesn't match array count | One parameter per input array |
| `#NAME?` | MAP or LAMBDA not recognized | Requires Excel 365/2024+; not available in earlier versions |
| `#CALC!` | Lambda produces calculation errors | Debug lambda with a single value first |
| `#SPILL!` | No room for result array | Clear cells in the spill range |

## Use Cases

### [[Bulk Data Transformation]]

**Scenario:** Clean and standardize an imported dataset with multiple data quality issues.

**Implementation:**
```
=MAP(RawData, LAMBDA(cell,
    PROPER(
        TRIM(
            SUBSTITUTE(
                SUBSTITUTE(cell, CHAR(160), " "),
                "  ", " "
            )
        )
    )
))
```

**Business Application:** Imported data often has inconsistent capitalization, extra spaces, and non-breaking spaces. MAP applies comprehensive cleaning to every cell in one formula.

**Technical Details:** The nested functions clean progressively: substitute non-breaking spaces, remove double spaces, trim edges, proper case. Each cell is processed independently.

---

### [[Parallel Array Calculations]]

**Scenario:** Calculate weighted scores from separate component and weight arrays.

**Implementation:**
```
=MAP(ComponentScores, ComponentWeights, LAMBDA(score, weight,
    score * weight
))
```
Then sum the result for final weighted score.

**Business Application:** Performance evaluations, investment portfolio calculations, and grade computations often use weighted components. MAP performs the multiplication position-by-position.

**Technical Details:** Both arrays must align - first component with first weight, etc. The result array has the same structure as the inputs.

---

### [[Custom Formula Application]]

**Scenario:** Apply a specialized calculation that doesn't have a built-in function.

**Implementation:**
```
=MAP(Values, LAMBDA(x,
    (x - MIN(AllValues)) / (MAX(AllValues) - MIN(AllValues))
))
```

**Business Application:** Min-max normalization scales all values to 0-1 range. MAP applies this statistical normalization to each value using the overall min/max.

**Technical Details:** The lambda can reference external ranges (AllValues) for context while processing elements. This enables calculations that need both element-level and aggregate data.

## Platform Differences

### Microsoft Excel (365/2024+)

| Feature | Support |
|---------|---------|
| Single array MAP | Full support |
| Multi-array MAP | Full support |
| Complex lambdas | Full support |
| Large arrays | Up to calculation limits |

### Google Sheets

| Feature | Support |
|---------|---------|
| MAP | Available (added 2022) |
| Syntax | Same as Excel |
| Multi-array support | Full support |
| Performance | May differ from Excel |

Google Sheets added MAP in 2022. Formulas should be portable between platforms, though behavior with edge cases may differ slightly.

### Other Platforms

MAP is available in:
- Microsoft Excel 365: Full support
- Microsoft Excel 2024: Full support
- Google Sheets: Full support (added 2022)
- LibreOffice Calc: Not available
- Apple Numbers: Not available
- Excel 2019 and earlier: Not available

## Tips and Best Practices

1. **Use meaningful parameter names:** `LAMBDA(price, qty, price*qty)` is clearer than `LAMBDA(a, b, a*b)`.

2. **Test lambda on single values first:** Before applying to arrays, verify your lambda works: `=LAMBDA(x, your_formula)(test_value)`.

3. **Ensure array dimensions match:** When using multiple arrays, they must have identical dimensions or you'll get #VALUE!.

4. **Consider native array math:** For simple operations, `=A1:A10 * B1:B10` may be simpler than MAP. Use MAP for complex transformations.

5. **Handle errors within the lambda:** `LAMBDA(x, IFERROR(calculation, fallback))` handles errors per-element.

6. **Combine with other lambda functions:** MAP can be nested inside BYROW/BYCOL for complex multi-level processing.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BYROW]] | Applies lambda to each row | When you need to reduce rows to single values |
| [[BYCOL]] | Applies lambda to each column | When you need to reduce columns to single values |
| [[SCAN]] | Returns intermediate accumulation results | When you need running calculations |
| [[REDUCE]] | Reduces array to single value | When you need one final result from array |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the transformation

*Every MAP needs a LAMBDA:*
```
=MAP(Range, LAMBDA(x, SQRT(x)))
```
The lambda defines what happens to each element.

---

**[[BYROW]] / [[BYCOL]]** - Combine transformations

*Transform then aggregate:*
```
=BYROW(MAP(Data, LAMBDA(x, x^2)), LAMBDA(row, SUM(row)))
```
MAP transforms elements, BYROW aggregates rows.

---

**[[FILTER]]** - Select then transform

*Clean targeted data:*
```
=MAP(FILTER(Data, Criteria), LAMBDA(x, UPPER(x)))
```
FILTER selects rows, MAP transforms the filtered results.

---

**[[IFERROR]]** - Error handling per element

*Robust transformations:*
```
=MAP(Data, LAMBDA(x, IFERROR(100/x, 0)))
```
IFERROR inside lambda catches errors at element level.

---

**[[IFS]] / [[SWITCH]]** - Multi-condition transformations

*Classification mapping:*
```
=MAP(Values, LAMBDA(v, SWITCH(TRUE, v<0,"Negative", v=0,"Zero", "Positive")))
```
Complex classification logic applied to each element.

## Official Documentation

- **Microsoft Excel:** [MAP function](https://support.microsoft.com/en-us/office/map-function-48006093-f97c-47c1-bfcc-749263bb1f01)

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
