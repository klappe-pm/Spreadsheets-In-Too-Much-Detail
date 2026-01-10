---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - lambda-functions
  - array-transformation
subTopics:
  - element-wise-processing
  - functional-programming
  - data-mapping
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - map function
  - array map
  - element mapper
tags:
  - google-sheets-only
  - lambda
  - arrays
  - transformation
  - functional
---

# MAP

## Description

**MAP** is a powerful lambda-based function in Google Sheets that applies a custom transformation function to each element of one or more arrays, returning a new array of the same dimensions with transformed values. Think of it as a universal element-by-element transformer: you provide one or more arrays and a LAMBDA function that defines how to transform each element, and MAP returns an array where every cell has been processed through your function. Unlike BYROW or BYCOL which aggregate rows or columns into single values, MAP maintains the original array dimensions, making it ideal for cell-by-cell transformations that need to preserve structure.

The primary use cases for MAP include applying calculations to every cell in a range (like converting units, applying markup, or transforming text), processing parallel arrays together (like calculating differences between corresponding cells), and applying custom logic that would otherwise require helper columns or copying formulas down. For example, `=MAP(A1:A100, LAMBDA(x, ROUND(x*1.1, 2)))` applies a 10% markup and rounds every value in the range. MAP truly shines when you need to apply the same transformation across an entire dataset without creating auxiliary formulas. It brings functional programming's "map" operation to spreadsheets, enabling clean, declarative data transformations.

There are several important gotchas when working with MAP. First, when using multiple arrays, they must have identical dimensions; MAP will error if array sizes do not match. Second, the LAMBDA must accept the same number of parameters as there are input arrays; one array means one parameter, two arrays means two parameters. Third, unlike ARRAYFORMULA, MAP explicitly processes each element through the LAMBDA, which can be slower for very large arrays but is more flexible for complex transformations. Fourth, the LAMBDA must return a single value for each element; returning arrays within the LAMBDA causes errors. Fifth, MAP processes all elements including empty cells, so your LAMBDA should handle empty inputs appropriately if the source data has gaps.

From a platform perspective, MAP is available in both Google Sheets and Microsoft Excel 365. The syntax and functionality are identical between platforms, making formulas fully portable. MAP was introduced as part of the LAMBDA function family in 2022. For users of older Excel versions without LAMBDA support, MAP is not available, and element-wise transformations must be done using ARRAYFORMULA (in Sheets) or by copying formulas down (in Excel). When building cross-platform spreadsheets, MAP formulas work seamlessly in both modern Excel and Google Sheets.

## Syntax

> [!f(x)] MAP Syntax
>
> ```
> =MAP(array1, [array2, ...], lambda)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first array or range to process. Each element of this array is passed to the LAMBDA function. Can be a range reference, named range, or array from another function. |
| `array2, ...` | No | Additional arrays to process in parallel. Must have exactly the same dimensions as array1. Elements from corresponding positions are passed together to the LAMBDA. Up to 253 additional arrays can be specified. |
| `lambda` | Yes | A LAMBDA function that defines the transformation for each element. Must accept the same number of parameters as there are input arrays. For one array: `LAMBDA(element, expression)`. For two arrays: `LAMBDA(element1, element2, expression)`. Must return a single value. |

### Return Value

Returns an array with the same dimensions as the input arrays. Each cell contains the result of applying the LAMBDA function to the corresponding element(s) from the input array(s). The output array spills automatically into adjacent cells.

## Examples

> [!f(x)] MAP Examples

### Example 1: Simple Value Transformation
```
=MAP(A1:A10, LAMBDA(x, x * 2))
```
**Result:** A column of 10 values, each double the original

**Explanation:** The LAMBDA receives each value as `x` and returns it doubled. This is the simplest MAP usage: apply a calculation to every element.

---

### Example 2: Percentage Markup with Rounding
```
=MAP(B2:B100, LAMBDA(price, ROUND(price * 1.15, 2)))
```
**Result:** Each price increased by 15% and rounded to 2 decimal places

**Explanation:** Applies a 15% markup to all prices in one formula. ROUND within the LAMBDA ensures clean currency values.

---

### Example 3: Text Transformation
```
=MAP(A1:C10, LAMBDA(name, PROPER(TRIM(name))))
```
**Result:** All text cells cleaned (trimmed) and converted to proper case

**Explanation:** TRIM removes extra spaces, PROPER capitalizes first letters. The transformation applies to the entire 3x10 range.

---

### Example 4: Two Arrays - Calculate Differences
```
=MAP(B2:B100, C2:C100, LAMBDA(actual, budget, actual - budget))
```
**Result:** Each cell shows the difference between actual and budget values

**Explanation:** Two parallel arrays are processed together. The LAMBDA receives corresponding elements from each and returns their difference.

---

### Example 5: Two Arrays - Percentage of Target
```
=MAP(B2:B50, C2:C50, LAMBDA(val, target, IF(target=0, 0, val/target)))
```
**Result:** Each cell shows what percentage the value is of the target

**Explanation:** Divides value by target with division-by-zero protection. Returns 0 when target is zero.

---

### Example 6: Conditional Transformation
```
=MAP(A1:A100, LAMBDA(x, IF(x > 100, "High", IF(x > 50, "Medium", "Low"))))
```
**Result:** Categorizes each value as "High", "Medium", or "Low"

**Explanation:** The LAMBDA contains conditional logic. Each element is independently categorized based on its value.

---

### Example 7: Apply Custom Calculation
```
=MAP(A1:D10, LAMBDA(score, ROUND((score - 50) / 10 + 5, 1)))
```
**Result:** Converts raw scores to a standardized 1-10 scale

**Explanation:** Applies a linear transformation to normalize scores. The formula works on the entire 4x10 grid.

---

### Example 8: Extract Part of Text
```
=MAP(A2:A100, LAMBDA(email, IFERROR(LEFT(email, FIND("@", email) - 1), "")))
```
**Result:** Extracts username from each email address

**Explanation:** Uses FIND and LEFT to extract text before @. IFERROR handles cells without @ symbol.

---

### Example 9: Format Numbers as Text
```
=MAP(B2:B50, LAMBDA(num, TEXT(num, "$#,##0.00")))
```
**Result:** Each number formatted as currency text

**Explanation:** TEXT function within LAMBDA formats each number. Useful for creating display-ready values.

---

### Example 10: Three Arrays - Weighted Calculation
```
=MAP(A2:A100, B2:B100, C2:C100, LAMBDA(qty, price, discount, qty * price * (1 - discount)))
```
**Result:** Calculates line total (quantity x price x discount factor) for each row

**Explanation:** Three parallel arrays are combined. Each LAMBDA call receives corresponding values from all three ranges.

---

### Example 11: Handle Empty Cells
```
=MAP(A1:A50, LAMBDA(x, IF(x = "", "", x * 1.1)))
```
**Result:** Multiplies non-empty values by 1.1, leaves empty cells empty

**Explanation:** Checks for empty cells before transformation. Prevents empty cells from becoming zeros.

---

### Example 12: Boolean to Yes/No
```
=MAP(A1:C20, LAMBDA(val, IF(val = TRUE, "Yes", IF(val = FALSE, "No", val))))
```
**Result:** Converts TRUE to "Yes", FALSE to "No", leaves other values unchanged

**Explanation:** Conditional replacement of boolean values. Non-boolean values pass through unchanged.

---

### Example 13: Date Formatting
```
=MAP(A2:A100, LAMBDA(dt, IF(ISNUMBER(dt), TEXT(dt, "MMM DD, YYYY"), dt)))
```
**Result:** Formats dates as "Jan 15, 2024" style text

**Explanation:** ISNUMBER checks for valid dates before formatting. Non-date values pass through unchanged.

---

### Example 14: Calculate Days Until Due
```
=MAP(B2:B50, LAMBDA(dueDate, dueDate - TODAY()))
```
**Result:** Days remaining until each due date (negative if past)

**Explanation:** Simple date arithmetic applied to every element. TODAY() is constant across all evaluations.

---

### Example 15: Complex Text Parsing
```
=MAP(A1:A100, LAMBDA(code, LET(
  parts, SPLIT(code, "-"),
  region, INDEX(parts, 1),
  year, INDEX(parts, 2),
  region & " (20" & year & ")"
)))
```
**Result:** Transforms "US-24" into "US (2024)"

**Explanation:** Uses LET within LAMBDA for complex multi-step transformations. SPLIT parses the code, then components are reformatted.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input arrays have different dimensions | Ensure all arrays have exactly the same number of rows and columns. Use ROWS() and COLUMNS() to verify. |
| `#NAME?` | LAMBDA syntax is incorrect or parameter names are invalid | Check LAMBDA structure. Parameter names cannot be cell references or reserved words. |
| `#REF!` | Output would spill into cells with existing data | Clear cells where the output needs to appear. MAP needs empty space matching input dimensions. |
| `#VALUE!` | LAMBDA returns an array instead of a single value | The LAMBDA must return one value per element. If using functions that return arrays, wrap in INDEX or adjust logic. |
| `#ERROR!` | Number of LAMBDA parameters does not match number of input arrays | For 2 arrays, LAMBDA needs 2 parameters. For 3 arrays, 3 parameters. Adjust LAMBDA signature accordingly. |
| `Formula parse error` | Missing comma between arrays or LAMBDA parameters | Review syntax: arrays are comma-separated, LAMBDA parameters are comma-separated, expression follows parameters. |
| Unexpected zeros | Empty cells are processed as zeros in calculations | Add empty cell handling: `IF(x="", "", calculation)` at the start of your LAMBDA expression. |

## Use Cases

### [[Data Cleaning: Standardizing Text Data]]

**Scenario:** A data analyst receives customer data with inconsistent formatting: varying cases, extra spaces, leading/trailing whitespace, and mixed separators. They need to clean and standardize all text fields.

**Implementation:**
```
=MAP(A2:D500, LAMBDA(cell,
  LET(
    trimmed, TRIM(cell),
    cleaned, SUBSTITUTE(SUBSTITUTE(trimmed, "  ", " "), "  ", " "),
    standardized, PROPER(cleaned),
    standardized
  )))
```

**Business Application:** Cleans an entire dataset in one formula. All text fields are trimmed, extra spaces removed, and capitalized consistently. This ensures data quality for analysis, deduplication, and customer communications. The transformation is applied uniformly, eliminating inconsistencies.

**Technical Details:** LET creates named intermediate steps for clarity. Double SUBSTITUTE handles multiple consecutive spaces. PROPER standardizes capitalization. The same transformation applies to all cells in the 4-column, 500-row range.

---

### [[Financial Analysis: Currency Conversion]]

**Scenario:** A multinational company has sales data in various currencies and needs to convert all amounts to a single reporting currency using current exchange rates stored in a reference table.

**Implementation:**
```
=MAP(Amounts, Currencies, LAMBDA(amount, currency,
  amount * VLOOKUP(currency, ExchangeRates, 2, FALSE)))
```
Where Amounts contains values and Currencies contains currency codes

**Business Application:** Converts all transaction amounts to the reporting currency in one formula. When exchange rates update, all conversions recalculate automatically. Finance teams get consistent, comparable figures across all geographic regions.

**Technical Details:** Two parallel arrays are processed: amounts and their currencies. The LAMBDA performs a lookup for each currency and multiplies. VLOOKUP could be replaced with INDEX/MATCH for better performance with large rate tables.

---

### [[Data Transformation: Score Normalization]]

**Scenario:** An HR team has employee assessment scores from different evaluation systems (different scales, different meaning of high/low) and needs to normalize them to a common 0-100 scale.

**Implementation:**
```
=MAP(RawScores, ScaleTypes, LAMBDA(score, scale,
  SWITCH(scale,
    "1-5", (score - 1) / 4 * 100,
    "1-10", (score - 1) / 9 * 100,
    "0-100", score,
    "percent", score * 100,
    score
  )))
```

**Business Application:** Normalizes diverse scoring systems to enable fair comparison across departments or evaluation methods. Aggregate statistics become meaningful. The transformation handles multiple scale types in a single formula.

**Technical Details:** SWITCH handles different scale types. Each scale type has its own normalization formula. Unknown scale types pass through unchanged. This pattern works for any number of scale types.

---

### [[Reporting: Status Indicator Generation]]

**Scenario:** A project manager needs to generate status indicators (traffic light colors, risk levels, priority flags) for hundreds of tasks based on multiple factors: due date proximity, completion percentage, and blocker status.

**Implementation:**
```
=MAP(DueDates, PctComplete, Blocked, LAMBDA(due, pct, block,
  LET(
    daysLeft, due - TODAY(),
    atRisk, OR(block = TRUE, AND(daysLeft < 7, pct < 80)),
    critical, OR(daysLeft < 0, AND(daysLeft < 3, pct < 50)),
    IFS(critical, "Critical", atRisk, "At Risk", pct = 100, "Complete", TRUE, "On Track")
  )))
```

**Business Application:** Generates status indicators for all tasks in one formula. Status updates automatically as time passes and progress is recorded. Managers can quickly identify problem areas without manually reviewing each task.

**Technical Details:** Three input arrays provide due dates, completion percentage, and blocker flags. LET defines intermediate boolean conditions. IFS evaluates conditions in priority order. The output is a status label for each task.

## Platform Differences

### Microsoft Excel

- **Availability:** MAP is available in Excel 365 (Microsoft 365 subscription versions)
- **Syntax:** Identical to Google Sheets
- **Behavior:** Same functionality; processes each element through LAMBDA
- **Not available in:** Excel 2019, Excel 2016, Excel 2021, or earlier versions
- **Alternative for older Excel:** Copy formulas down/across, or use VBA for custom transformations

### Google Sheets

- **Availability:** Available in all Google Sheets (introduced 2022)
- **Specific Behavior:** This documentation focuses on Google Sheets behavior
- **Comparison to ARRAYFORMULA:** MAP is more flexible for complex transformations; ARRAYFORMULA is faster for simple operations
- **Integration:** Works with all Google Sheets functions including QUERY and other LAMBDA functions

### Key Differences

| Feature | Google Sheets | Excel 365 |
|---------|---------------|-----------|
| Availability | All users | Microsoft 365 subscribers only |
| Syntax | Identical | Identical |
| Multiple arrays | Up to 253 | Up to 253 |
| Performance | Good | Generally slightly better |
| Spill behavior | Automatic | Automatic |
| Alternative | ARRAYFORMULA for simple cases | No direct equivalent |

## Tips and Best Practices

1. **Match Array Dimensions Exactly:** When using multiple arrays, they must be the same size. Use ROWS() and COLUMNS() to verify dimensions before combining.

2. **Handle Empty Cells Explicitly:** MAP processes all cells including empties. Add `IF(x="", "", ...)` at the start of your LAMBDA to preserve empty cells.

3. **Use LET for Complex Transformations:** When your LAMBDA has multiple steps, use LET within it to name intermediate values and improve readability.

4. **Consider ARRAYFORMULA for Simple Cases:** For basic arithmetic (like `A1:A100 * 2`), ARRAYFORMULA is simpler and faster. Use MAP when you need LAMBDA's flexibility.

5. **Name LAMBDA Parameters Meaningfully:** Use descriptive names like `price`, `date`, `status` instead of `x`, `y`, `z`. This makes formulas self-documenting.

6. **Test with Small Ranges First:** Verify your LAMBDA logic on a few rows before applying to large datasets. Debugging is easier with small outputs.

7. **Use for Parallel Processing:** MAP is ideal when you need to combine values from multiple columns cell-by-cell. Each LAMBDA call receives aligned values.

8. **Combine with Other LAMBDA Functions:** MAP can feed into BYROW, REDUCE, or other functions. Use MAP to transform data, then aggregate with other functions.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ARRAYFORMULA]] | Applies formulas to arrays | For simple operations; faster than MAP for basic arithmetic |
| [[BYROW]] | Applies LAMBDA to each row | When aggregating across columns, not transforming individual cells |
| [[BYCOL]] | Applies LAMBDA to each column | When aggregating down rows, not transforming individual cells |
| [[MAKEARRAY]] | Generates arrays by position | When creating new arrays rather than transforming existing ones |

### Commonly Used Together

**[[LAMBDA]]** - Required to define the transformation function

*MAP requires LAMBDA to define element transformation:*
```
=MAP(A1:A100, LAMBDA(x, transformation_expression))
```
The LAMBDA encapsulates the logic applied to each element.

---

**[[LET]]** - Structure complex transformations

*Use LET within LAMBDA for multi-step logic:*
```
=MAP(A1:A100, LAMBDA(x, LET(step1, x*2, step2, ROUND(step1,0), step2)))
```
Names intermediate calculations for clarity.

---

**[[IFERROR]]** - Handle potential errors

*Wrap risky operations in IFERROR:*
```
=MAP(A1:A100, B1:B100, LAMBDA(a, b, IFERROR(a/b, 0)))
```
Prevents single errors from breaking the entire result.

---

**[[IF]]** - Conditional transformations

*Apply different logic based on conditions:*
```
=MAP(A1:A100, LAMBDA(x, IF(x>100, x*0.9, x)))
```
Different transformation for different value ranges.

---

**[[FILTER]]** - Pre-filter before mapping

*Filter data then transform:*
```
=MAP(FILTER(A1:A100, A1:A100>0), LAMBDA(x, SQRT(x)))
```
Apply transformation only to qualifying values.

---

**[[HSTACK]] / [[VSTACK]]** - Combine mapped results

*Stack transformed arrays:*
```
=HSTACK(A1:A10, MAP(A1:A10, LAMBDA(x, x*2)))
```
Place original and transformed values side by side.

## Official Documentation

- **Google Sheets:** [MAP function](https://support.google.com/docs/answer/12568985)
- **Microsoft Excel:** [MAP function](https://support.microsoft.com/en-us/office/map-function-48006093-f97c-47c1-bfcc-749263bb1f01)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2022 | Introduced as part of the LAMBDA helper function family |
| Excel 365 | 2022 | Added with LAMBDA function ecosystem |
| Excel 2021 | N/A | Not available in perpetual license version |
| Excel Online | 2022 | Available in web version |

---

*Last updated: 2026-01-10*
