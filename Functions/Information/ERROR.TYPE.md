---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - error-handling
  - error-identification
  - debugging
  - data-validation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - error-type
  - error-number
  - identify-error
tags:
  - information
  - error-handling
  - debugging
  - error-identification
  - data-quality
  - excel
  - google-sheets
---

# ERROR.TYPE

## Description

ERROR.TYPE is an information function that returns a number corresponding to one of the error values in a cell, or #N/A if no error exists. This function is essential for advanced error handling because it allows you to identify exactly which type of error occurred, enabling you to create specific responses for different error conditions rather than treating all errors the same way. Each error type has a unique numeric code, making it possible to build sophisticated error classification and handling systems.

The function accepts a single argument, which is the value or cell reference you want to test. If the argument contains an error value, ERROR.TYPE returns a number from 1 to 8 identifying the specific error type. If the argument is not an error, the function returns #N/A (not #VALUE! or FALSE), which is important to understand when building error handling logic. This means you often need to wrap ERROR.TYPE in an ISNA or IFERROR function when checking for non-error values.

The error type codes are: 1 for #NULL!, 2 for #DIV/0!, 3 for #VALUE!, 4 for #REF!, 5 for #NAME?, 6 for #NUM!, 7 for #N/A, and in Excel 2007 and later, 8 for #GETTING_DATA. Understanding these codes is crucial for building conditional logic that responds differently based on the specific error encountered. For example, a #N/A error from a lookup function typically indicates "not found," while a #DIV/0! error indicates a calculation issue.

Both Excel and Google Sheets support ERROR.TYPE with the same syntax and error codes for the main error types. The function is particularly valuable in data validation scenarios, quality assurance processes, and debugging complex spreadsheets where understanding why an error occurred is as important as knowing that an error exists. Combined with SWITCH or nested IF statements, ERROR.TYPE enables you to create user-friendly error messages that explain the specific problem to users.

## Syntax

> [!f(x)]
> `ERROR.TYPE(error_val)`

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| error_val | Yes | The error value you want to identify. Can be a direct error value, a cell reference containing an error, or a formula that produces an error. |

## Error Type Codes

| Code | Error | Description |
|------|-------|-------------|
| 1 | #NULL! | Incorrect range reference (spaces instead of operators) |
| 2 | #DIV/0! | Division by zero |
| 3 | #VALUE! | Wrong type of argument or operand |
| 4 | #REF! | Invalid cell reference |
| 5 | #NAME? | Unrecognized formula name or text |
| 6 | #NUM! | Invalid numeric value |
| 7 | #N/A | Value not available (lookup failed, etc.) |
| 8 | #GETTING_DATA | Data retrieval in progress (Excel 2007+, async connections) |
| #N/A | *(none)* | The value is not an error |

## Examples

### Example 1: Basic Error Identification

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=1/0` | `=ERROR.TYPE(A1)` | 2 | #DIV/0! error returns code 2 |
| A2 | `=NA()` | `=ERROR.TYPE(A2)` | 7 | #N/A error returns code 7 |

This example demonstrates the fundamental behavior of ERROR.TYPE. Each error type has a specific numeric code that the function returns.

### Example 2: Testing Non-Error Values

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `100` | `=ERROR.TYPE(A1)` | #N/A | Number is not an error |
| A2 | `Hello` | `=ERROR.TYPE(A2)` | #N/A | Text is not an error |
| A3 | `TRUE` | `=ERROR.TYPE(A3)` | #N/A | Boolean is not an error |
| A4 | *(empty)* | `=ERROR.TYPE(A4)` | #N/A | Empty cell is not an error |

When the argument is not an error value, ERROR.TYPE returns #N/A, not FALSE or 0. This behavior requires careful handling in formulas.

### Example 3: All Error Types

| Cell | Contents | Formula | Result | Error Type |
|------|----------|---------|--------|------------|
| A1 | `=A1:A2 B1:B2` | `=ERROR.TYPE(A1)` | 1 | #NULL! |
| A2 | `=1/0` | `=ERROR.TYPE(A2)` | 2 | #DIV/0! |
| A3 | `="A"+1` | `=ERROR.TYPE(A3)` | 3 | #VALUE! |
| A4 | *(deleted reference)* | `=ERROR.TYPE(A4)` | 4 | #REF! |
| A5 | `=UnknownFunction()` | `=ERROR.TYPE(A5)` | 5 | #NAME? |
| A6 | `=SQRT(-1)` | `=ERROR.TYPE(A6)` | 6 | #NUM! |
| A7 | `=NA()` | `=ERROR.TYPE(A7)` | 7 | #N/A |

This comprehensive example shows the error code for each possible error type in Excel and Google Sheets.

### Example 4: Direct Error Testing

| Formula | Result | Notes |
|---------|--------|-------|
| `=ERROR.TYPE(#DIV/0!)` | 2 | Direct error value |
| `=ERROR.TYPE(#N/A)` | 7 | Direct error value |
| `=ERROR.TYPE(#VALUE!)` | 3 | Direct error value |
| `=ERROR.TYPE(1/0)` | 2 | Expression that causes error |

You can pass error values directly to ERROR.TYPE, not just cell references. This is useful for testing and validation.

### Example 5: Handling ERROR.TYPE's Return Value

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `100` | `=IFERROR(ERROR.TYPE(A1),"No error")` | No error | Handles #N/A from non-error |
| A2 | `=1/0` | `=IFERROR(ERROR.TYPE(A2),"No error")` | 2 | Returns error code |
| A3 | `100` | `=IF(ISERROR(A3),ERROR.TYPE(A3),"OK")` | OK | Better pattern |

Since ERROR.TYPE returns #N/A for non-errors, you need to handle this appropriately. The third pattern checks for error first before calling ERROR.TYPE.

### Example 6: User-Friendly Error Messages

| Cell | Contents | Formula | Result |
|------|----------|---------|--------|
| A1 | `=1/0` | `=SWITCH(ERROR.TYPE(A1),1,"Range intersection error",2,"Division by zero",3,"Wrong value type",7,"Not found","Unknown error")` | Division by zero |

Using SWITCH (or nested IFs) with ERROR.TYPE lets you create descriptive error messages that help users understand what went wrong.

### Example 7: Error Classification

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=1/0` | `=IF(ISNUMBER(ERROR.TYPE(A1)),IF(ERROR.TYPE(A1)<=3,"Calculation Error","Reference/Lookup Error"),"No Error")` | Calculation Error | Categorizes errors |

This pattern groups errors into categories: codes 1-3 are typically calculation issues, while codes 4-7 are reference or lookup issues.

### Example 8: Counting Specific Error Types

| Data Range | Formula | Result | Notes |
|------------|---------|--------|-------|
| A1:A10 (mixed) | `=SUMPRODUCT(--(ERROR.TYPE(A1:A10)=7))` | 3 | Count #N/A errors |
| A1:A10 (mixed) | `=SUMPRODUCT(--(ERROR.TYPE(A1:A10)=2))` | 2 | Count #DIV/0! errors |
| A1:A10 (mixed) | `=SUMPRODUCT(--ISNUMBER(ERROR.TYPE(A1:A10)))` | 5 | Count all errors |

Using SUMPRODUCT with ERROR.TYPE allows you to count occurrences of specific error types or all errors in a range.

### Example 9: Conditional Error Handling

| Cell | Contents | Formula | Result |
|------|----------|---------|--------|
| A1 | `=VLOOKUP(99,B:C,2,0)` | `=IF(ISERROR(A1),IF(ERROR.TYPE(A1)=7,"Item not found","Lookup error"),A1)` | Item not found |

This pattern provides specific handling for #N/A errors (item not found in lookup) versus other errors that might indicate formula problems.

### Example 10: Error Audit Report

| Cell | Contents | Formula | Result |
|------|----------|---------|--------|
| A1 | `=1/0` | `=IF(ISERROR(A1),"Error: "&CHOOSE(ERROR.TYPE(A1),"NULL","DIV/0","VALUE","REF","NAME","NUM","N/A")&" in cell A1",A1)` | Error: DIV/0 in cell A1 |

This creates an audit report showing both the error type and location, useful for debugging and quality assurance.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | Input value is not an error | This is expected behavior; wrap in IFERROR or test with ISERROR first |
| Unexpected #N/A | Testing a cell that was expected to have an error | Verify the cell actually contains an error value |
| Formula error | Using ERROR.TYPE result directly in math | Check for #N/A result before using in calculations |
| Code 8 not recognized | Using older Excel or Google Sheets | #GETTING_DATA (code 8) only exists in Excel 2007+ |
| Wrong code returned | Confusing error types | Review the error type code table to match codes correctly |

## Use Cases

### Intelligent Error Handling System

**Scenario**: You have a complex financial model with many calculations that can produce different types of errors. You want to provide specific, helpful error messages rather than generic ones.

**Solution**: Build an error handling system that translates error codes into actionable messages.

```excel
// Create a comprehensive error message function
=IF(ISERROR(A1),
    SWITCH(ERROR.TYPE(A1),
        1, "Error: Invalid range intersection. Check your cell references.",
        2, "Error: Division by zero. Verify denominator is not zero.",
        3, "Error: Invalid value type. Check input data types.",
        4, "Error: Invalid reference. A referenced cell may have been deleted.",
        5, "Error: Unknown function name. Check spelling of function names.",
        6, "Error: Invalid number. Value may be too large or negative for this operation.",
        7, "Error: Value not found. The lookup value doesn't exist in the data.",
        "Error: Unknown error type."
    ),
    A1
)

// Simpler version with grouped messages
=IF(NOT(ISERROR(A1)),A1,
    IF(OR(ERROR.TYPE(A1)=2,ERROR.TYPE(A1)=6),"Calculation problem - check inputs",
    IF(ERROR.TYPE(A1)=7,"Lookup value not found",
    "Formula error - contact support")))
```

**Why this works**: ERROR.TYPE distinguishes between different error causes, allowing your error messages to provide specific guidance. A #N/A error typically needs different user action than a #DIV/0! error.

### Data Quality Monitoring Dashboard

**Scenario**: You're managing a large dataset that pulls from multiple sources and need to monitor data quality by tracking the types and quantities of errors occurring.

**Solution**: Create a dashboard that counts and categorizes errors by type.

```excel
// Count each error type in a data range
="Null Errors: "&SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=1))
="Division Errors: "&SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=2))
="Value Errors: "&SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=3))
="Reference Errors: "&SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=4))
="Name Errors: "&SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=5))
="Number Errors: "&SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=6))
="N/A Errors: "&SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=7))

// Calculate error rate by type
=SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=7))/(ROWS(A1:Z100)*COLUMNS(A1:Z100))*100&"% N/A rate"

// Find most common error type
=SWITCH(MAX(SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=1)),SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=2)),...),
    SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=1)),"#NULL!",
    SUMPRODUCT(--(ERROR.TYPE(A1:Z100)=2)),"#DIV/0!",
    ...)
```

**Why this works**: By categorizing errors, you can identify systemic issues. A sudden increase in #REF! errors might indicate deleted data, while #N/A increases might indicate lookup table issues.

### Automatic Error Resolution Suggestions

**Scenario**: You want to create a helper column that not only identifies errors but suggests specific resolution steps based on the error type.

**Solution**: Build a resolution guide using ERROR.TYPE to match errors with their typical causes and fixes.

```excel
// Create resolution guide
=IF(NOT(ISERROR(B2)),"OK",
    CHOOSE(ERROR.TYPE(B2),
        "Check cell references - remove extra spaces between ranges",
        "Cell "&ADDRESS(ROW(),COLUMN()-1)&" divides by zero. Check cell values.",
        "Type mismatch. Ensure numeric operations have numeric inputs.",
        "Broken reference. Restore deleted cells or update formula.",
        "Unknown name. Check function spelling or define named range.",
        "Number out of bounds. Check for negative roots or overflow.",
        "Lookup failed. Value '"&A2&"' not in lookup table. Add it or check spelling."
    )
)

// Priority-based error flagging
=IF(ISERROR(B2),
    SWITCH(ERROR.TYPE(B2),
        4,"CRITICAL",
        5,"CRITICAL",
        2,"HIGH",
        3,"MEDIUM",
        7,"LOW",
        "MEDIUM"
    ),
    "OK"
)
```

**Why this works**: Different errors have different causes and require different fixes. ERROR.TYPE enables context-aware help that guides users to the specific resolution for their particular error.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic syntax | `=ERROR.TYPE(error_val)` | `=ERROR.TYPE(error_val)` |
| Error codes 1-7 | Supported | Supported |
| Error code 8 (#GETTING_DATA) | Excel 2007+ | Not supported |
| Return for non-error | #N/A | #N/A |
| Array formula support | Yes | Yes (with ARRAYFORMULA) |
| Direct error input | Yes | Yes |
| Named error constants | Supported | Supported |

The main difference is that Google Sheets does not have the #GETTING_DATA error (code 8), which only occurs in Excel when external data connections are still loading. For all practical purposes, the function works identically across both platforms.

## Tips and Best Practices

1. **Always Test for Error First**: Since ERROR.TYPE returns #N/A for non-errors, use ISERROR to check if a cell contains an error before calling ERROR.TYPE. This prevents cascading #N/A values in your formulas.

2. **Use SWITCH for Readability**: When creating error messages based on ERROR.TYPE, use SWITCH instead of nested IFs for cleaner, more maintainable code. SWITCH makes it easy to see the mapping between error codes and responses.

3. **Group Related Error Types**: Consider grouping errors by category (calculation errors: 1-3, reference errors: 4-5, data errors: 6-7) when you don't need to distinguish between every individual error type.

4. **Document Error Codes**: When building error handling systems, include comments or documentation explaining what each error code means. Not everyone memorizes that 2 means #DIV/0!.

5. **Combine with IFERROR for Graceful Handling**: Use `=IFERROR(ERROR.TYPE(A1),0)` to convert the #N/A return (for non-errors) to 0, making it easier to use in calculations and SUMPRODUCT formulas.

6. **Consider User Experience**: When displaying error information to end users, translate the numeric codes into meaningful messages. Users should never see "Error type 7" - they should see "Value not found."

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|---------------|
| ISERROR | Tests for any error | Returns TRUE/FALSE, not error code |
| ISERR | Tests for errors except #N/A | Returns TRUE/FALSE, not error code |
| ISNA | Tests specifically for #N/A | Returns TRUE/FALSE, not error code |
| IFERROR | Returns alternative for errors | Handles errors, doesn't identify them |
| IFNA | Returns alternative for #N/A | Handles #N/A specifically |
| TYPE | Returns data type code | Returns type of value, not error type |
| NA | Creates #N/A error | Generates error, doesn't test for it |

### Commonly Used Together

- **IF + ISERROR + ERROR.TYPE**: Test for error, then get error type
- **SWITCH + ERROR.TYPE**: Map error codes to messages
- **CHOOSE + ERROR.TYPE**: Another way to map codes to values
- **SUMPRODUCT + ERROR.TYPE**: Count specific error types
- **IFERROR + ERROR.TYPE**: Handle ERROR.TYPE's #N/A return for non-errors

## Official Documentation

- [Microsoft Excel ERROR.TYPE Function](https://support.microsoft.com/en-us/office/error-type-function-10958677-7c8d-44f7-ae77-b9a9ee6eefaa)
- [Google Sheets ERROR.TYPE Function](https://support.google.com/docs/answer/3093358)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available (codes 1-7) |
| Excel 2007 | Added code 8 for #GETTING_DATA |
| Excel 2010 | No changes |
| Excel 2016 | No changes |
| Excel 365 | No changes |
| Google Sheets | Supported since launch (codes 1-7) |
