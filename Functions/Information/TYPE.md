---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - data-type-detection
  - type-checking
  - data-validation
  - debugging
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - type-function
  - data-type
  - value-type
tags:
  - information
  - type-checking
  - data-validation
  - debugging
  - data-analysis
  - excel
  - google-sheets
---

# TYPE

## Description

TYPE is an information function that returns a number indicating the data type of a value. This function is essential for understanding what kind of data you're working with, enabling you to write formulas that behave differently based on whether a cell contains a number, text, logical value, error, or array. Unlike the individual IS functions (ISNUMBER, ISTEXT, etc.) that test for a specific type, TYPE tells you the exact type in a single call, making it more efficient for comprehensive type analysis.

The function accepts a single value argument and returns one of several numeric codes: 1 for numbers, 2 for text, 4 for logical values (TRUE/FALSE), 16 for error values, and 64 for arrays. Excel 365 also returns 128 for compound data types like linked stock or geography data. These specific numbers were chosen in early spreadsheet development and have remained consistent for backward compatibility, though the gaps between them (1, 2, 4, 16, 64) might seem unusual.

One important behavior to understand is that TYPE returns the type of the resulting value, not the type of the cell content. If a cell contains a formula that returns a number, TYPE returns 1 (number), not some code for "formula." Similarly, dates are stored as numbers in Excel and Google Sheets, so TYPE returns 1 for dates. Empty cells are treated as 0 by TYPE, so they return 1 (number) rather than having their own type code.

Both Excel and Google Sheets support TYPE with the same basic functionality and type codes for common data types. The function is particularly valuable in data validation, debugging complex formulas, and building flexible formulas that can handle multiple input types gracefully. When combined with SWITCH or IF statements, TYPE enables you to create formulas that adapt their behavior based on the input data type.

## Syntax

> [!f(x)]
> `TYPE(value)`

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| value | Yes | The value whose type you want to determine. Can be any value, cell reference, or formula result. |

## Type Codes

| Code | Data Type | Description |
|------|-----------|-------------|
| 1 | Number | Numeric values, including dates and times |
| 2 | Text | Text strings, including empty strings ("") |
| 4 | Logical | Boolean values (TRUE or FALSE) |
| 16 | Error | Error values (#N/A, #VALUE!, #REF!, etc.) |
| 64 | Array | Arrays (ranges entered as array formulas) |
| 128 | Compound | Linked data types (Excel 365 only - stocks, geography) |

## Examples

### Example 1: Basic Type Detection

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `100` | `=TYPE(A1)` | 1 | Number |
| A2 | `Hello` | `=TYPE(A2)` | 2 | Text |
| A3 | `TRUE` | `=TYPE(A3)` | 4 | Logical |
| A4 | `=1/0` | `=TYPE(A4)` | 16 | Error |

This example demonstrates the fundamental behavior of TYPE, returning different codes for each major data type.

### Example 2: Numbers and Dates

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `42.5` | `=TYPE(A1)` | 1 | Decimal number |
| A2 | `1/15/2024` | `=TYPE(A2)` | 1 | Date (stored as number) |
| A3 | `10:30 AM` | `=TYPE(A3)` | 1 | Time (stored as number) |
| A4 | `-100` | `=TYPE(A4)` | 1 | Negative number |

Dates and times are stored as numbers in spreadsheets, so TYPE returns 1 for them. This is why ISNUMBER returns TRUE for dates.

### Example 3: Empty Cells and Zeros

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | *(empty)* | `=TYPE(A1)` | 1 | Empty treated as 0 |
| A2 | `0` | `=TYPE(A2)` | 1 | Explicit zero |
| A3 | `=""` | `=TYPE(A3)` | 2 | Empty string is text |

Empty cells are converted to 0 before TYPE evaluates them, so they return 1 (number). However, a formula that returns an empty string is type 2 (text).

### Example 4: Text Variations

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `Hello World` | `=TYPE(A1)` | 2 | Regular text |
| A2 | `'123` | `=TYPE(A2)` | 2 | Number stored as text |
| A3 | ` ` | `=TYPE(A3)` | 2 | Space character |
| A4 | `=""` | `=TYPE(A4)` | 2 | Empty string formula |

Any text value, including numbers formatted as text (with leading apostrophe), returns type code 2.

### Example 5: Direct Value Testing

| Formula | Result | Notes |
|---------|--------|-------|
| `=TYPE(123)` | 1 | Direct number |
| `=TYPE("Hello")` | 2 | Direct text |
| `=TYPE(TRUE)` | 4 | Direct boolean |
| `=TYPE(#N/A)` | 16 | Direct error |
| `=TYPE({1,2,3})` | 64 | Direct array |

You can pass values directly to TYPE, not just cell references. This is useful for testing and validation formulas.

### Example 6: Formula Results

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=SUM(1,2,3)` | `=TYPE(A1)` | 1 | Formula returning number |
| A2 | `=CONCATENATE("A","B")` | `=TYPE(A2)` | 2 | Formula returning text |
| A3 | `=A1>0` | `=TYPE(A3)` | 4 | Formula returning logical |
| A4 | `=VLOOKUP(99,B:C,2,0)` | `=TYPE(A4)` | 16 | Formula returning error |

TYPE evaluates the result of formulas, not the formula itself. The type depends on what the formula produces.

### Example 7: Error Type Codes

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=1/0` | `=TYPE(A1)` | 16 | #DIV/0! |
| A2 | `=NA()` | `=TYPE(A2)` | 16 | #N/A |
| A3 | `="A"+1` | `=TYPE(A3)` | 16 | #VALUE! |
| A4 | `=UnknownFunc()` | `=TYPE(A4)` | 16 | #NAME? |

All error types return the same code (16). Use ERROR.TYPE if you need to distinguish between different error types.

### Example 8: Type-Based Conditional Logic

| Cell | Contents | Formula | Result |
|------|----------|---------|--------|
| A1 | `100` | `=SWITCH(TYPE(A1),1,"Number",2,"Text",4,"Boolean",16,"Error","Other")` | Number |
| A2 | `Hello` | `=SWITCH(TYPE(A2),1,"Number",2,"Text",4,"Boolean",16,"Error","Other")` | Text |

Using SWITCH with TYPE allows you to create human-readable type labels or take different actions based on data type.

### Example 9: Counting by Type

| Data Range | Formula | Result | Notes |
|------------|---------|--------|-------|
| A1:A10 (mixed) | `=SUMPRODUCT(--(TYPE(A1:A10)=1))` | 5 | Count numbers |
| A1:A10 (mixed) | `=SUMPRODUCT(--(TYPE(A1:A10)=2))` | 3 | Count text values |
| A1:A10 (mixed) | `=SUMPRODUCT(--(TYPE(A1:A10)=16))` | 2 | Count errors |

SUMPRODUCT with TYPE allows you to count how many cells contain each data type.

### Example 10: Input Validation

| Cell | Contents | Formula | Result |
|------|----------|---------|--------|
| B2 | `100` | `=IF(TYPE(B2)=1,"Valid number","Please enter a number")` | Valid number |
| B3 | `Hello` | `=IF(TYPE(B3)=1,"Valid number","Please enter a number")` | Please enter a number |

This pattern validates that user input is the expected type before processing it in calculations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Unexpected 1 | Empty cell treated as number | Check for empty cells separately with ISBLANK before TYPE |
| Unexpected 1 for date | Dates are stored as numbers | Use ISNUMBER with date range validation for date checking |
| Type 2 for number | Number stored as text | Data may have leading apostrophe; use VALUE to convert |
| No code for formula | TYPE returns result type | TYPE tests values, not cell properties; use ISFORMULA for formula detection |
| Missing type 128 | Compound data only in Excel 365 | Type 128 for linked data types only exists in modern Excel |

## Use Cases

### Dynamic Input Processing

**Scenario**: You have a form where users can enter either a product ID (number) or product name (text), and you need to look up information using the appropriate method.

**Solution**: Use TYPE to detect the input type and route to the correct lookup method.

```excel
// Determine lookup method based on input type
=IF(TYPE(A2)=1,
    VLOOKUP(A2,ProductIDs,2,FALSE),
    VLOOKUP(A2,ProductNames,2,FALSE)
)

// More comprehensive with error handling
=IF(TYPE(A2)=16,"Error in input",
    IF(TYPE(A2)=1,
        IFERROR(VLOOKUP(A2,IDTable,2,FALSE),"ID not found"),
        IFERROR(VLOOKUP(A2,NameTable,2,FALSE),"Name not found")
    )
)

// Using SWITCH for multiple types
=SWITCH(TYPE(A2),
    1, "Looking up by ID: "&VLOOKUP(A2,IDTable,2,FALSE),
    2, "Looking up by name: "&VLOOKUP(A2,NameTable,2,FALSE),
    "Invalid input type"
)
```

**Why this works**: TYPE lets you create flexible formulas that adapt to different input types, improving user experience by accepting multiple formats without requiring separate input fields.

### Data Quality Assessment

**Scenario**: You've imported data from an external source and need to analyze the data types in each column to identify potential quality issues like numbers stored as text.

**Solution**: Create a data profiling report using TYPE to categorize values.

```excel
// Create a type profile for a column
="Numbers: "&SUMPRODUCT(--(TYPE(A:A)=1))&
", Text: "&SUMPRODUCT(--(TYPE(A:A)=2))&
", Errors: "&SUMPRODUCT(--(TYPE(A:A)=16))

// Flag mixed types in a column (potential data issue)
=IF(
    AND(SUMPRODUCT(--(TYPE(A1:A100)=1))>0, SUMPRODUCT(--(TYPE(A1:A100)=2))>0),
    "WARNING: Mixed number and text in column",
    "OK: Consistent data types"
)

// Identify specific problematic cells
=IF(AND(TYPE(A2)=2, ISNUMBER(VALUE(A2))), "Number stored as text", "OK")
```

**Why this works**: TYPE provides a numeric code that's easy to aggregate and compare, making it efficient for analyzing large datasets and identifying type inconsistencies that might cause calculation errors.

### Flexible Formula Builder

**Scenario**: You're building a template where formulas need to handle various input types gracefully, performing appropriate operations or conversions based on what users enter.

**Solution**: Create type-aware formulas that process each type appropriately.

```excel
// Convert any input to a displayable string
=SWITCH(TYPE(A1),
    1, TEXT(A1,"#,##0.00"),
    2, A1,
    4, IF(A1,"Yes","No"),
    16, "Error: "&IFERROR(CHOOSE(ERROR.TYPE(A1),"NULL","DIV/0","VALUE","REF","NAME","NUM","N/A"),"Unknown"),
    "Unknown type"
)

// Process numbers, ignore text
=IF(TYPE(A1)=1, A1*1.1, A1)

// Create a type-safe SUM that reports text entries
=IF(SUMPRODUCT(--(TYPE(A1:A10)=2))>0,
    "Warning: "&SUMPRODUCT(--(TYPE(A1:A10)=2))&" text values ignored. Sum: "&SUM(A1:A10),
    SUM(A1:A10)
)
```

**Why this works**: Type-aware formulas are more robust because they handle edge cases explicitly rather than failing or producing unexpected results when encountering unexpected data types.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic syntax | `=TYPE(value)` | `=TYPE(value)` |
| Number type (1) | Supported | Supported |
| Text type (2) | Supported | Supported |
| Logical type (4) | Supported | Supported |
| Error type (16) | Supported | Supported |
| Array type (64) | Supported | Supported |
| Compound type (128) | Excel 365 only | Not supported |
| Empty cell behavior | Returns 1 (number) | Returns 1 (number) |

The main difference is Excel 365's support for type 128 (compound data types), which applies to linked data like stocks and geography. For all common data types, the function works identically across platforms.

## Tips and Best Practices

1. **Remember Empty Cell Behavior**: Empty cells return type 1 (number) because they're treated as 0. If you need to detect empty cells, use ISBLANK before or instead of TYPE.

2. **Dates Return Type 1**: Since dates and times are stored as serial numbers, TYPE returns 1 for them. Use additional validation if you need to distinguish dates from regular numbers.

3. **Use SWITCH for Readability**: When creating type-based logic, use SWITCH(TYPE(A1),1,"...",2,"...") rather than nested IFs. It's cleaner and easier to maintain.

4. **Combine with IS Functions**: For single-type checks, ISNUMBER, ISTEXT, etc. are more readable. Use TYPE when you need to check multiple types or get a numeric code for further processing.

5. **Error Values Need Special Handling**: TYPE returns 16 for all errors. If you need to know which specific error occurred, use ERROR.TYPE instead or in addition to TYPE.

6. **Numbers-as-Text Detection**: To find numbers stored as text, combine TYPE with VALUE: `=IF(AND(TYPE(A1)=2,ISNUMBER(VALUE(A1))),"Number as text","OK")`

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|---------------|
| ISNUMBER | Tests if value is number | Returns TRUE/FALSE, not type code |
| ISTEXT | Tests if value is text | Returns TRUE/FALSE, not type code |
| ISLOGICAL | Tests if value is boolean | Returns TRUE/FALSE, not type code |
| ISERROR | Tests if value is error | Returns TRUE/FALSE, not type code |
| ERROR.TYPE | Returns error type number | Only works on errors, returns error code 1-8 |
| ISBLANK | Tests if cell is empty | TYPE treats empty as number (0) |
| CELL | Returns cell information | Returns format info, not value type |

### Commonly Used Together

- **SWITCH + TYPE**: Map type codes to readable labels or actions
- **IF + TYPE**: Conditional logic based on data type
- **SUMPRODUCT + TYPE**: Count values by type
- **TYPE + ERROR.TYPE**: Complete type and error analysis
- **ISBLANK + TYPE**: Handle empty cells separately from zeros

## Official Documentation

- [Microsoft Excel TYPE Function](https://support.microsoft.com/en-us/office/type-function-45b4e688-4bc3-48b3-a105-ffa892995899)
- [Google Sheets TYPE Function](https://support.google.com/docs/answer/3093498)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available with types 1, 2, 4, 16, 64 |
| Excel 2010 | No changes |
| Excel 2016 | No changes |
| Excel 365 | Added type 128 for compound/linked data types |
| Google Sheets | Supported since launch (types 1, 2, 4, 16, 64) |
