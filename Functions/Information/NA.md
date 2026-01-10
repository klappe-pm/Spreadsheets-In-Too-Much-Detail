---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - error-generation
  - missing-data
  - data-placeholder
  - chart-handling
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - na-function
  - not-available
  - missing-value
tags:
  - information
  - error-generation
  - missing-data
  - placeholder
  - chart-handling
  - excel
  - google-sheets
---

# NA

## Description

NA is an information function that returns the #N/A error value, which stands for "Not Available" or "Not Applicable." Unlike most functions, NA takes no arguments - you simply call `=NA()` and it returns the #N/A error. This function is essential for explicitly marking data as unavailable or creating intentional gaps in datasets, particularly useful when working with charts where you want to show breaks in data series rather than connecting points across missing values.

The function serves a fundamentally different purpose than other error-generating scenarios. While #N/A errors often occur unintentionally (like when VLOOKUP can't find a value), using NA() intentionally signals that data is purposely absent rather than missing due to an error. This distinction is important for data analysis and documentation - it tells anyone reviewing the spreadsheet that the blank isn't an oversight but a deliberate indication that no data exists or applies.

One of the most valuable uses of NA() is in charting. When Excel or Google Sheets encounters #N/A in chart data, it creates a gap in the line or series rather than plotting zero or connecting adjacent points. This behavior makes NA() perfect for scenarios like plotting stock prices where trading didn't occur on certain days, or showing survey responses where certain questions weren't asked of all participants. The chart accurately represents that no data exists rather than misleadingly showing zero.

Both Excel and Google Sheets support NA() with identical behavior. The function has been part of spreadsheet applications since their earliest versions, providing a standardized way to indicate missing or inapplicable data. When combined with ISNA() for testing and IFNA() for handling, NA() forms part of a complete toolkit for managing unavailable data in professional spreadsheet applications.

## Syntax

> [!f(x)]
> `NA()`

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| *(none)* | N/A | NA takes no arguments. The parentheses are required but must be empty. |

## Examples

### Example 1: Basic NA Generation

| Formula | Result | Notes |
|---------|--------|-------|
| `=NA()` | #N/A | Returns #N/A error |
| `=NA()` | #N/A | Always returns same result |

NA() always returns the #N/A error value. It takes no arguments and produces consistent output.

### Example 2: Testing with ISNA

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=NA()` | `=ISNA(A1)` | TRUE | Detects #N/A |
| A2 | `100` | `=ISNA(A2)` | FALSE | Not an #N/A |
| A3 | `=1/0` | `=ISNA(A3)` | FALSE | Different error type |

ISNA specifically tests for #N/A errors, distinguishing them from other error types.

### Example 3: Using with IFNA

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=NA()` | `=IFNA(A1,"No data")` | No data | Handles #N/A |
| A2 | `100` | `=IFNA(A2,"No data")` | 100 | Returns value |
| A3 | `=1/0` | `=IFNA(A3,"No data")` | #DIV/0! | Only handles #N/A |

IFNA provides a clean way to replace #N/A with a custom value while letting other errors through.

### Example 4: Conditional NA Generation

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | *(empty)* | `=IF(A1="",NA(),A1)` | #N/A | Empty becomes N/A |
| A2 | `100` | `=IF(A2="",NA(),A2)` | 100 | Value passes through |
| A3 | `0` | `=IF(A3="",NA(),A3)` | 0 | Zero is not empty |

This pattern converts empty cells to #N/A, useful for chart preparation.

### Example 5: Chart Data Preparation

| Month | Sales | Formula for Chart | Result |
|-------|-------|-------------------|--------|
| Jan | 100 | `=IF(B2>0,B2,NA())` | 100 |
| Feb | 0 | `=IF(B3>0,B3,NA())` | #N/A |
| Mar | 150 | `=IF(B4>0,B4,NA())` | 150 |
| Apr | 0 | `=IF(B5>0,B5,NA())` | #N/A |
| May | 200 | `=IF(B6>0,B6,NA())` | 200 |

Converting zeros to #N/A creates gaps in line charts rather than dips to zero.

### Example 6: Placeholder in Data Entry

| Cell | Purpose | Formula | Result | Notes |
|------|---------|---------|--------|-------|
| B2 | Pending data | `=NA()` | #N/A | Marks data as pending |
| B3 | Actual data | `125` | 125 | Real value entered |
| B4 | Not applicable | `=NA()` | #N/A | Question doesn't apply |

Use NA() to clearly indicate cells awaiting data entry or where data doesn't apply.

### Example 7: Error Type Identification

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=NA()` | `=ERROR.TYPE(A1)` | 7 | #N/A is error type 7 |
| A2 | `=1/0` | `=ERROR.TYPE(A2)` | 2 | #DIV/0! is type 2 |

ERROR.TYPE returns 7 for #N/A errors, allowing you to distinguish intentional NA from other errors.

### Example 8: Lookup Failure Simulation

| Formula | Result | Notes |
|---------|--------|-------|
| `=IF(A1="Unknown",NA(),VLOOKUP(A1,Table,2,0))` | #N/A or value | Pre-check before lookup |
| `=IFERROR(VLOOKUP(A1,Table,2,0),NA())` | #N/A for any error | Convert all errors to N/A |

These patterns help standardize error handling in lookup operations.

### Example 9: Array with NA Values

| Formula | Result | Notes |
|---------|--------|-------|
| `={1,2,NA(),4,5}` | Array with gap | NA in array constant |
| `=SUM({1,2,NA(),4,5})` | #N/A | NA propagates through SUM |
| `=SUMIF({1,2,NA(),4,5},">0")` | 12 | SUMIF can work around NA |

When #N/A appears in array calculations, it typically propagates unless handled.

### Example 10: Distinguishing Intentional vs Accidental NA

| Cell | Contents | Validation Formula | Result |
|------|----------|-------------------|--------|
| A1 | `=NA()` | `=IF(ISNA(A1),"Intentionally N/A","Has value")` | Intentionally N/A |
| A2 | `=VLOOKUP(99,B:C,2,0)` | Same formula | Intentionally N/A |

Both intentional NA() and lookup failures produce identical #N/A errors. Document your data to distinguish between them.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #NAME? | Forgot parentheses: `=NA` | Use `=NA()` with parentheses |
| Propagating #N/A | NA in calculations | Use IFNA or IFERROR to handle |
| Chart still connects points | Chart settings override NA | Check chart options for "Show empty cells as: Gaps" |
| SUM returns #N/A | Range contains NA | Use SUMIF or AGGREGATE to skip errors |
| Can't distinguish NA sources | Both intentional and lookup NA look same | Add documentation or helper columns |

## Use Cases

### Creating Gaps in Line Charts

**Scenario**: You're creating a line chart showing daily stock prices, but the market was closed on weekends and holidays. You want the chart to show gaps on those days rather than connecting Friday to Monday with a straight line.

**Solution**: Use NA() to replace missing data points, creating visual gaps in the chart.

```excel
// Convert missing dates to NA for charting
=IF(ISBLANK(B2),NA(),B2)

// Or check if date is a weekend
=IF(OR(WEEKDAY(A2)=1,WEEKDAY(A2)=7),NA(),B2)

// More comprehensive: handle both missing and weekend
=IF(OR(ISBLANK(B2),WEEKDAY(A2)=1,WEEKDAY(A2)=7),NA(),B2)

// For irregular time series, check if date exists in data
=IF(COUNTIF(DataDates,A2)=0,NA(),VLOOKUP(A2,DataTable,2,FALSE))
```

**Why this works**: Charts interpret #N/A as "no data point" and create a gap rather than plotting zero or interpolating between adjacent values. This accurately represents the absence of data.

### Marking Incomplete Survey Responses

**Scenario**: You're analyzing survey data where some questions were only asked to certain respondents based on skip logic. You need to distinguish between "answered no" (0), "skipped intentionally" (N/A), and "missing data" (blank).

**Solution**: Use NA() to explicitly mark questions that weren't asked.

```excel
// Apply skip logic: if Q1 is "No", Q2-Q5 don't apply
=IF(Q1="No",NA(),Q2_Response)

// Mark all follow-up questions based on qualifier
=IF(QualifyingQuestion<>"Yes",NA(),FollowUpResponse)

// Create a summary that handles N/A appropriately
=IF(ISNA(B2),"Not Asked",IF(ISBLANK(B2),"No Response",B2))

// Calculate response rate excluding N/A (not applicable)
=COUNTIF(B:B,"<>"&"")-SUMPRODUCT(--ISNA(B:B))
```

**Why this works**: Using NA() creates a clear three-way distinction: actual responses, intentionally skipped questions (N/A), and missing responses (blank). This preserves data integrity for analysis.

### Data Validation and Placeholder System

**Scenario**: You're building a data entry template where certain cells should be filled in later, and you want to clearly indicate which data is pending while preventing accidental calculations with incomplete data.

**Solution**: Use NA() as a placeholder that will cause dependent formulas to show errors until real data is entered.

```excel
// Placeholder for pending data
=NA()  // Enter this in cells awaiting data

// Dependent calculation that shows clear error until complete
=IF(OR(ISNA(Revenue),ISNA(Expenses)),NA(),Revenue-Expenses)

// Summary that counts pending items
="Pending items: "&SUMPRODUCT(--ISNA(DataRange))

// Validation that checks if all data is complete
=IF(SUMPRODUCT(--ISNA(RequiredData))>0,
    "Data incomplete - "&SUMPRODUCT(--ISNA(RequiredData))&" items pending",
    "All data entered")

// Replace NA with default only when explicitly approved
=IF(AND(ISNA(A2),ApprovalCell="Approved"),DefaultValue,A2)
```

**Why this works**: NA() causes formulas to fail loudly rather than silently calculating with incomplete data. This prevents errors from going unnoticed and makes the state of data completion immediately visible.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic syntax | `=NA()` | `=NA()` |
| Return value | #N/A | #N/A |
| Chart gap behavior | Creates gap | Creates gap |
| ISNA detection | Works | Works |
| IFNA handling | Works (2013+) | Works |
| ERROR.TYPE code | 7 | 7 |
| Can type #N/A directly | Yes (as text) | Yes (as text) |

NA() works identically in Excel and Google Sheets. Both platforms treat #N/A the same way in charts and calculations.

## Tips and Best Practices

1. **Use for Intentional Missing Data**: Reserve NA() for data that is intentionally unavailable or not applicable. This distinguishes it from data that is missing due to errors or oversight.

2. **Document NA Usage**: Since lookup failures and intentional NA() both produce #N/A, add documentation or helper columns to explain why specific cells contain #N/A.

3. **Choose NA Over Blank for Charts**: When preparing chart data, use NA() instead of blank cells for more predictable gap behavior across different chart types and platforms.

4. **Combine with IFNA for Graceful Handling**: Use IFNA() to provide default values or messages when encountering #N/A, making reports more user-friendly.

5. **Consider Impact on Aggregations**: Remember that NA propagates through most functions like SUM and AVERAGE. Use AGGREGATE function or SUMIF/AVERAGEIF to work around #N/A values.

6. **Use for Data Validation Placeholders**: NA() is ideal for marking cells that require data entry because it will cause visible errors in dependent cells, preventing silent calculation with incomplete data.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|---------------|
| ISNA | Tests for #N/A error | Tests for NA, doesn't create it |
| IFNA | Handles #N/A errors | Returns alternative value for #N/A |
| ISERROR | Tests for any error | Tests all errors, not just #N/A |
| IFERROR | Handles any error | Handles all errors, not just #N/A |
| ERROR.TYPE | Returns error code | Returns 7 for #N/A |
| #N/A | Direct error entry | Can type as text but not as true error |

### Commonly Used Together

- **IF + NA**: Conditional generation of #N/A
- **ISNA + NA**: Test and generate #N/A errors
- **IFNA + NA**: Handle NA values with defaults
- **VLOOKUP + NA**: NA as fallback for missing lookups
- **Charts + NA**: Create gaps in data visualization

## Official Documentation

- [Microsoft Excel NA Function](https://support.microsoft.com/en-us/office/na-function-5469c2d1-a90c-4fb5-9bbc-64bd9bb6b47c)
- [Google Sheets NA Function](https://support.google.com/docs/answer/3093359)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2003 | Function available |
| Excel 2007 | No changes |
| Excel 2010 | No changes |
| Excel 2013 | IFNA function added for NA handling |
| Excel 2016 | No changes |
| Excel 365 | No changes |
| Google Sheets | Supported since launch |
