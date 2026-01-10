---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - type-conversion
  - number-conversion
  - formula-comments
  - compatibility
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - n-function
  - convert-to-number
  - numeric-value
tags:
  - information
  - type-conversion
  - number-conversion
  - formula-comments
  - compatibility
  - excel
  - google-sheets
---

# N

## Description

N is an information function that converts a value to a number according to specific rules: numbers remain unchanged, dates return their serial number, TRUE returns 1, FALSE returns 0, error values pass through unchanged, and all other values (including text) return 0. While this function was originally designed for compatibility with other spreadsheet programs, its most popular modern use is as a clever way to add comments directly inside formulas since `N("comment text")` returns 0 and doesn't affect calculations.

The function accepts a single value argument and applies straightforward conversion rules. Numbers and dates (which are stored as numbers internally) return their numeric value unchanged. Logical values convert to 1 (TRUE) or 0 (FALSE). Error values are returned as errors, which means using N on an error cell will propagate that error. Text strings, regardless of their content, always return 0 - even if the text contains numeric characters like "123".

One of the most valuable uses of N in modern spreadsheets is embedding comments within complex formulas. Since `N("This calculates the tax rate")` evaluates to 0, you can add it to any formula without affecting the result: `=A1*B1+N("Revenue * Tax Rate")`. This technique makes complex formulas self-documenting and easier for others (or your future self) to understand. The comment is visible when editing the formula but doesn't change the calculated result.

Both Excel and Google Sheets support the N function with identical behavior. The function is rarely needed for its original purpose of type conversion since modern spreadsheets handle type coercion automatically in most contexts. However, understanding N is valuable for reading legacy formulas and for the formula commenting technique. In data validation scenarios, N can also be useful for safely converting mixed data types to numbers without causing errors.

## Syntax

> [!f(x)]
> `N(value)`

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| value | Yes | The value to convert to a number. Can be a value, cell reference, or expression. |

## Conversion Rules

| Input Type | Result | Example |
|------------|--------|---------|
| Number | Same number | N(42) = 42 |
| Date | Serial number | N(DATE(2024,1,15)) = 45306 |
| Time | Decimal fraction | N(TIME(12,0,0)) = 0.5 |
| TRUE | 1 | N(TRUE) = 1 |
| FALSE | 0 | N(FALSE) = 0 |
| Error | Same error | N(#N/A) = #N/A |
| Text | 0 | N("Hello") = 0 |
| Empty cell | 0 | N(A1) = 0 (if A1 is empty) |

## Examples

### Example 1: Basic Number Conversion

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `100` | `=N(A1)` | 100 | Number unchanged |
| A2 | `42.5` | `=N(A2)` | 42.5 | Decimal unchanged |
| A3 | `-50` | `=N(A3)` | -50 | Negative unchanged |

Numbers pass through N unchanged. The function simply returns the same value.

### Example 2: Text Conversion

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `Hello` | `=N(A1)` | 0 | Text becomes 0 |
| A2 | `123` (as text) | `=N(A2)` | 0 | Text numbers still become 0 |
| A3 | ` ` | `=N(A3)` | 0 | Space character becomes 0 |

All text values return 0, even if the text looks like a number. Use VALUE() if you need to convert numeric text to actual numbers.

### Example 3: Logical Value Conversion

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `TRUE` | `=N(A1)` | 1 | TRUE converts to 1 |
| A2 | `FALSE` | `=N(A2)` | 0 | FALSE converts to 0 |
| A3 | `=5>3` | `=N(A3)` | 1 | Formula result TRUE = 1 |

Logical values convert to their numeric equivalents: TRUE = 1, FALSE = 0.

### Example 4: Date and Time Conversion

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `1/15/2024` | `=N(A1)` | 45306 | Date serial number |
| A2 | `12:00 PM` | `=N(A2)` | 0.5 | Time as decimal (noon = 0.5) |
| A3 | `1/15/2024 12:00` | `=N(A3)` | 45306.5 | DateTime combined |

Dates and times are already stored as numbers internally, so N returns their serial number representation.

### Example 5: Error Handling

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=1/0` | `=N(A1)` | #DIV/0! | Error passes through |
| A2 | `=NA()` | `=N(A2)` | #N/A | Error passes through |
| A3 | `=VLOOKUP(99,B:C,2,0)` | `=N(A3)` | #N/A | Error from lookup passes through |

Error values are not converted to 0; they propagate through the N function. Use IFERROR if you need to handle errors.

### Example 6: Formula Comments

| Formula | Result | Notes |
|---------|--------|-------|
| `=A1*B1+N("Revenue times tax rate")` | (A1*B1) | Comment adds 0 |
| `=SUM(A:A)+N("Total sales for Q1")` | SUM(A:A) | Comment embedded |
| `=VLOOKUP(A1,Data,2,0)+N("Gets price from lookup table")` | VLOOKUP result | Self-documenting formula |

This is the most popular modern use of N - adding inline comments that don't affect calculations.

### Example 7: Multiple Comments in Formula

| Formula | Result | Notes |
|---------|--------|-------|
| `=(A1+N("Base price"))*B1+N("times quantity")+C1+N("plus shipping")` | Calculated value | Multiple comments |
| `=IF(A1>100,A1*0.9,A1)+N("10% discount over 100")` | Conditional result | Explains logic |

You can add multiple N comments throughout a formula to explain different parts.

### Example 8: Empty Cell Handling

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | *(empty)* | `=N(A1)` | 0 | Empty becomes 0 |
| A2 | `=""` | `=N(A2)` | 0 | Empty string becomes 0 |
| A3 | `0` | `=N(A3)` | 0 | Zero is zero |

Both empty cells and cells containing empty strings return 0.

### Example 9: Direct Value Testing

| Formula | Result | Notes |
|---------|--------|-------|
| `=N(123)` | 123 | Direct number |
| `=N("text")` | 0 | Direct text |
| `=N(TRUE)` | 1 | Direct boolean |
| `=N(DATE(2024,6,15))` | 45458 | Date function |

You can pass values directly to N, not just cell references.

### Example 10: Safe Numeric Conversion

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `100` | `=SUM(B1:B10)+N(A1)` | Correct sum | Safe to add |
| A2 | `Error text` | `=SUM(B1:B10)+N(A2)` | Same sum | Text ignored (0) |
| A3 | `TRUE` | `=SUM(B1:B10)+N(A3)` | Sum + 1 | Boolean converted |

N can be used to safely include values in calculations, converting non-numbers to 0 rather than causing errors.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Error propagation | N on error cell passes the error | Wrap in IFERROR: `=IFERROR(N(A1),0)` |
| 0 for numeric text | Text "123" returns 0, not 123 | Use VALUE("123") to convert text numbers |
| Unexpected 0 | Forgot that text always returns 0 | Verify cell actually contains a number, not text |
| Comment affecting formula | Incorrect placement of N() | Ensure N() is added, not multiplied |
| Array behavior | N only processes first cell of range | Use SUMPRODUCT or array formula for ranges |

## Use Cases

### Self-Documenting Complex Formulas

**Scenario**: You have complex formulas that others need to maintain, and you want to add explanations without using separate comment cells or notes.

**Solution**: Use N to embed comments directly in your formulas.

```excel
// Simple inline comment
=VLOOKUP(A2,PriceTable,3,FALSE)+N("Get unit price from column 3")

// Multi-part formula with explanations
=(B2+N("Base salary"))
*(1+C2+N("plus bonus percentage"))
*(1-D2+N("minus tax rate"))

// Complex nested formula with documentation
=IF(
    A2>1000+N("threshold for discount"),
    A2*0.9+N("apply 10% discount"),
    A2+N("no discount")
)

// Date calculation with explanation
=EDATE(A2,12)+N("Add 12 months for renewal date")
```

**Why this works**: N("text") always returns 0, so adding it to any formula doesn't change the result. The comment text is visible when editing the formula, making complex formulas self-documenting.

### Safe Type Conversion for Mixed Data

**Scenario**: You're importing data that may contain a mix of numbers, text, and empty cells, and you need to perform calculations without #VALUE! errors.

**Solution**: Use N to safely convert all values to numbers.

```excel
// Sum mixed data without errors (text becomes 0)
=N(A1)+N(A2)+N(A3)+N(A4)+N(A5)

// Average with safe conversion
=AVERAGE(N(A1),N(A2),N(A3),N(A4),N(A5))

// Use in SUMPRODUCT for ranges
=SUMPRODUCT(N(A1:A10))

// Conditional calculation that handles mixed types
=IF(ISNUMBER(A1),A1,N(A1))+B1
```

**Why this works**: N converts non-numeric values to 0 rather than causing errors, making calculations more robust when dealing with inconsistent data sources.

### Converting Boolean Results for Arithmetic

**Scenario**: You need to use TRUE/FALSE results from logical tests in arithmetic calculations, converting them explicitly to 1/0.

**Solution**: Use N to explicitly convert logical values.

```excel
// Count conditions met (TRUE=1, FALSE=0)
=N(A1>100)+N(B1>100)+N(C1>100)+N("Count of values over 100")

// Create a score based on multiple conditions
=N(Sales>Target)*10
+N(Satisfaction>4)*5
+N(Tenure>2)*3
+N("Bonus points: 10 for sales, 5 for satisfaction, 3 for tenure")

// Weighted boolean calculation
=N(Criteria1Met)*Weight1
+N(Criteria2Met)*Weight2
+N(Criteria3Met)*Weight3
```

**Why this works**: While Excel often auto-converts booleans to numbers, using N makes the conversion explicit and clear to anyone reading the formula.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic syntax | `=N(value)` | `=N(value)` |
| Number handling | Returns number unchanged | Returns number unchanged |
| Text handling | Returns 0 | Returns 0 |
| Boolean handling | TRUE=1, FALSE=0 | TRUE=1, FALSE=0 |
| Date handling | Returns serial number | Returns serial number |
| Error handling | Passes error through | Passes error through |
| Formula commenting | Works | Works |
| Array handling | Returns first cell value | Returns first cell value |

The N function works identically in Excel and Google Sheets. There are no significant platform differences for this function.

## Tips and Best Practices

1. **Use for Formula Comments**: The most valuable modern use of N is adding inline comments to formulas. `=formula+N("explanation")` makes formulas self-documenting without affecting results.

2. **Don't Use for Text-to-Number**: N returns 0 for text, even numeric text like "123". Use VALUE() to convert text containing numbers to actual numbers.

3. **Remember Error Propagation**: Unlike converting text to 0, N passes errors through unchanged. Wrap in IFERROR if you need to handle potential errors.

4. **Prefer Modern Functions**: For most type checking and conversion needs, modern functions like ISNUMBER, VALUE, and automatic type coercion are more appropriate than N.

5. **Document Complex Logic**: Add N comments at key decision points in complex formulas to explain business rules or calculation logic that might not be obvious.

6. **Be Consistent with Comment Style**: If using N for comments in a workbook, be consistent. Either use `+N("comment")` throughout or don't use it at all to avoid confusion.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|---------------|
| VALUE | Converts text to number | Converts "123" to 123; N returns 0 |
| INT | Truncates to integer | Rounds down; N doesn't change numbers |
| NUMBERVALUE | Locale-aware text to number | Handles decimal/thousand separators |
| TYPE | Returns type code | Returns type number, doesn't convert |
| T | Returns text or empty | Opposite of N - returns text or "" |
| ISNUMBER | Tests if value is number | Returns TRUE/FALSE, not converted value |

### Commonly Used Together

- **N + Complex Formulas**: Add inline documentation
- **N + SUM**: Safe addition with mixed types
- **IFERROR + N**: Handle errors before conversion
- **N + Boolean Expressions**: Explicit TRUE/FALSE to 1/0 conversion
- **N + T**: Together provide text-or-number conversion pair

## Official Documentation

- [Microsoft Excel N Function](https://support.microsoft.com/en-us/office/n-function-a624cad1-3635-4208-b54a-29b04dc8b4e7)
- [Google Sheets N Function](https://support.google.com/docs/answer/3093357)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available |
| Excel 2007 | No changes |
| Excel 2010 | No changes |
| Excel 2016 | No changes |
| Excel 365 | No changes |
| Google Sheets | Supported since launch |
