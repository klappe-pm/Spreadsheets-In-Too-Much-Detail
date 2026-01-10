---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- logical
- information
subTopics:
- error-checking
- error-handling
- data-validation
- is-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Is Error
- Error Check
- Error Detection
- Any Error Test
tags:
- error-checking
- error-handling
- validation
- is-functions
- boolean
---

# ISERROR

## Description

**ISERROR** is the most comprehensive error detection function in spreadsheets. It returns TRUE if a cell or expression contains any error value whatsoever: #N/A, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, #GETTING_DATA, or #CALC!. If you want to catch every possible error without exception, ISERROR is your tool. It's the sledgehammer approach to error detection when you don't need to differentiate between error types.

The primary use case for ISERROR is building error-proof formulas that gracefully handle unexpected situations. Before IFERROR was introduced, the pattern `=IF(ISERROR(formula), fallback_value, formula)` was ubiquitous in professional spreadsheets. This pattern catches any error from the formula and replaces it with a clean fallback value, preventing ugly error messages from appearing in reports. While IFERROR has largely replaced this pattern for simple cases, ISERROR remains essential when you need to detect errors without necessarily replacing them.

**ISERROR vs ISERR:** The critical distinction is that ISERROR catches #N/A errors while ISERR does not. This matters because #N/A typically means "value not found" in lookups, which is semantically different from calculation errors. If you want to treat "not found" as an error like any other, use ISERROR. If you want to preserve #N/A for special handling, use ISERR instead. Many spreadsheet mistakes stem from using ISERROR too broadly when ISERR plus ISNA would provide better control.

**Performance and modern alternatives:** ISERROR evaluates its argument once and returns a simple Boolean result, making it extremely fast. However, the IF(ISERROR(x), y, x) pattern evaluates x twice, which matters for complex formulas or external data connections. IFERROR(x, y) evaluates x only once and should be preferred for simple error replacement. Use ISERROR when you need Boolean logic (conditional formatting, SUMPRODUCT criteria, IF conditions with more than two outcomes) rather than direct value replacement.

## Syntax

> [!f(x)] ISERROR Syntax
>
> ```
> =ISERROR(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value to test for errors. Can be a cell reference, expression, formula result, or literal value. ISERROR evaluates this and returns TRUE if it is any error type, FALSE otherwise. |

### Return Value

Returns TRUE if value is any error type: #N/A, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, #GETTING_DATA, #CALC!, or platform-specific errors like #ERROR! in Google Sheets. Returns FALSE for any non-error value including numbers, text, logical values (TRUE/FALSE), and blank cells.

### Error Types Detected

| Error Type | Detected | Typical Cause |
|------------|----------|---------------|
| #N/A | Yes | Value not found in lookup |
| #VALUE! | Yes | Wrong argument type for function |
| #REF! | Yes | Invalid cell reference (deleted cells) |
| #DIV/0! | Yes | Division by zero |
| #NUM! | Yes | Invalid numeric value in calculation |
| #NAME? | Yes | Unrecognized formula or range name |
| #NULL! | Yes | Incorrect range intersection operator |
| #GETTING_DATA | Yes | External data still loading (Excel) |
| #CALC! | Yes | Array calculation error (Excel 365) |
| #ERROR! | Yes | Generic error (Google Sheets) |

## Examples

> [!f(x)] ISERROR Examples

### Example 1: Basic Division by Zero Detection
```
=ISERROR(10/0)
```
**Result:** TRUE

**Explanation:** Division by zero produces #DIV/0!, which ISERROR detects. This is the classic error scenario in spreadsheets. Any time a formula might divide by a cell that could be zero, ISERROR (or IFERROR) can prevent the ugly error message.

---

### Example 2: Detecting Lookup Failures
```
=ISERROR(VLOOKUP("XYZ", A1:B100, 2, FALSE))
```
**Result:** TRUE (if "XYZ" is not found)

**Explanation:** When VLOOKUP cannot find the lookup value, it returns #N/A. Unlike ISERR, ISERROR catches this error too. Use this when any lookup failure should be treated as an error, not as a valid "not found" condition.

---

### Example 3: Text-to-Number Error Detection
```
=ISERROR("Hello" + 5)
```
**Result:** TRUE

**Explanation:** Attempting to add text to a number produces #VALUE!. ISERROR catches this type mismatch error. Common in imported data where numbers may be stored as text with hidden characters.

---

### Example 4: Invalid Reference Detection
```
=ISERROR(#REF!)
```
**Result:** TRUE

**Explanation:** #REF! errors occur when formulas reference deleted cells or ranges. ISERROR detects these structural problems. This error often appears after rows/columns are deleted that were referenced by formulas elsewhere.

---

### Example 5: Classic IF/ISERROR Pattern (Pre-IFERROR)
```
=IF(ISERROR(A1/B1), 0, A1/B1)
```
**Result:** 0 if division causes error, otherwise the quotient

**Explanation:** The traditional error-handling pattern before IFERROR existed. Returns 0 instead of any error. Note: this evaluates A1/B1 twice, which is inefficient. Use `=IFERROR(A1/B1, 0)` for the same result with better performance.

---

### Example 6: Detecting Formula Name Errors
```
=ISERROR(MADEUPFUNCTION(A1))
```
**Result:** TRUE

**Explanation:** Using a function name that doesn't exist (typo or unsupported function) produces #NAME?. ISERROR catches this. Common when formulas are copied between Excel and Google Sheets where function names differ.

---

### Example 7: Counting All Errors in a Range
```
=SUMPRODUCT(--ISERROR(A1:A100))
```
**Result:** Count of cells containing any error

**Explanation:** ISERROR returns TRUE/FALSE for each cell. Double-dash converts to 1/0. SUMPRODUCT sums these values. This counts all error cells regardless of error type. Useful for data quality dashboards.

---

### Example 8: Conditional Formatting Error Highlight
```
=ISERROR(A1)
```
**Result:** Use as conditional formatting rule to highlight error cells

**Explanation:** In conditional formatting, use this formula to highlight any cell containing an error. Apply to a range (e.g., apply to A1:Z100 with formula =ISERROR(A1)). Cells with errors will be formatted (e.g., red background).

---

### Example 9: Filtering Out Error Rows
```
=FILTER(A1:D100, NOT(ISERROR(C1:C100)), "No valid rows")
```
**Result:** All rows where column C does not contain an error

**Explanation:** In modern Excel/Sheets, combine FILTER with ISERROR to extract only error-free rows. NOT() inverts the logic (we want non-error rows). Useful for cleaning data before analysis.

---

### Example 10: Error-Free Sum with Legacy Approach
```
=SUMPRODUCT((NOT(ISERROR(A1:A100)))*A1:A100)
```
**Result:** Sum of all non-error values in range

**Explanation:** Multiplying by NOT(ISERROR()) zeros out error cells before summing. Pre-AGGREGATE approach that still works everywhere. Modern alternative: `=AGGREGATE(9, 6, A1:A100)` ignores errors automatically.

---

### Example 11: Multi-Condition Error Check
```
=ISERROR(VLOOKUP(A1,Table1,2,0)) + ISERROR(VLOOKUP(A1,Table2,2,0))
```
**Result:** 0 if both lookups succeed, 1 if one fails, 2 if both fail

**Explanation:** Since ISERROR returns TRUE (1) or FALSE (0), you can add them to count how many operations failed. Greater than 0 means at least one error. Equals 2 means both failed.

---

### Example 12: Nested Error Checking for Complex Formulas
```
=IF(ISERROR(INDEX(Data,MATCH(A1,Keys,0),MATCH(B1,Headers,0))), "Not found", INDEX(Data,MATCH(A1,Keys,0),MATCH(B1,Headers,0)))
```
**Result:** "Not found" for any error, otherwise the looked-up value

**Explanation:** Complex INDEX/MATCH combinations can fail in multiple ways (either MATCH not finding its value, or INDEX receiving invalid coordinates). ISERROR catches all possibilities in one check.

---

### Example 13: ISERROR with Array Formula
```
=ISERROR(A1:A10/B1:B10)
```
**Result:** Array of TRUE/FALSE values for each row

**Explanation:** When applied to ranges, ISERROR returns an array of results. In Excel 365/Sheets, this spills automatically. Each position indicates whether that specific calculation produced an error.

---

### Example 14: Validating External Data
```
=IF(ISERROR(WEBSERVICE(A1)), "Invalid URL or service unavailable", WEBSERVICE(A1))
```
**Result:** Error message or the web service response

**Explanation:** External data functions can fail for many reasons: bad URLs, network issues, service downtime. ISERROR provides a clean fallback. Note: WEBSERVICE is Excel-only; Sheets uses IMPORTDATA, IMPORTXML, etc.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Formula evaluates twice | Pattern `IF(ISERROR(f),x,f)` calculates f twice | Use IFERROR(f,x) instead for simple replacement. It's more efficient. |
| Masking important errors | Using ISERROR to hide all errors indiscriminately | Be selective. Sometimes errors should be visible to indicate problems. Consider ISERR+ISNA for nuanced handling. |
| Wrong return type expectation | Expecting ISERROR to return the error type | ISERROR only returns TRUE/FALSE. Use ERROR.TYPE to identify which error occurred. |
| Catching errors too broadly | All errors handled identically | Different errors may need different responses. #N/A (not found) vs #DIV/0! (calculation error) often warrant different handling. |
| Confusion with ISERR | Expecting ISERROR and ISERR to behave the same | ISERR excludes #N/A; ISERROR includes it. Know which you need. |

## Use Cases

### [[Report Error Sanitization]]

**Scenario:** Monthly financial reports must be clean with no error values visible to executives. Any formula errors should display "N/A" or a dash instead.

**Implementation:**
```
=IF(ISERROR(RevenueFormula), "-", TEXT(RevenueFormula, "$#,##0"))
```

**Business Application:** Professional reports can't show #DIV/0! or #REF! to stakeholders. Wrapping calculated fields in ISERROR checks ensures clean presentation. The text alternative (dash or "N/A") indicates missing data without technical error messages.

**Technical Details:** For many cells, this is better handled with conditional formatting to hide error cells, combined with IFERROR in individual formulas. Consider a global error-handling approach rather than cell-by-cell implementation.

---

### [[Data Quality Scoring]]

**Scenario:** A data import process needs to calculate a quality score based on how many cells contain errors after transformation formulas are applied.

**Implementation:**
```
=1 - (SUMPRODUCT(--ISERROR(B2:B1000)) / ROWS(B2:B1000))
```
Returns quality score from 0 to 1 (1 = no errors)

**Business Application:** Data stewardship teams need metrics to track data quality over time. A formula column that attempts to validate/transform data will produce errors for invalid records. Counting these errors creates an objective quality measurement.

**Technical Details:** SUMPRODUCT with ISERROR counts errors; dividing by total rows gives error rate; subtracting from 1 converts to quality score. Track this metric over time to measure data quality improvement.

---

### [[Dynamic Error-Tolerant Aggregation]]

**Scenario:** Sum a column of calculated values where some calculations may fail due to missing input data, without using AGGREGATE function (for compatibility).

**Implementation:**
```
=SUMPRODUCT((NOT(ISERROR(PriceCalculations)))*PriceCalculations)
```

**Business Application:** Pricing models pulling from multiple data sources may have intermittent failures. The total should still calculate based on available data rather than failing entirely when one component errors.

**Technical Details:** Multiplying by NOT(ISERROR()) effectively zeros out error values before summing. Works in all Excel/Sheets versions. For Excel 2010+, AGGREGATE(9,6,range) is cleaner. For Sheets, this SUMPRODUCT pattern is reliable.

---

### [[Conditional Formatting for Error Highlighting]]

**Scenario:** Highlight all cells containing errors in red across a data entry form to help users identify and fix problems.

**Implementation:**
Conditional formatting rule: `=ISERROR(A1)`
Applied to: $A$1:$Z$100
Format: Red fill

**Business Application:** Interactive data entry forms benefit from immediate visual feedback. Red-highlighted cells tell users exactly where problems exist. Combined with data validation messages, this creates a user-friendly error correction experience.

**Technical Details:** The formula reference (A1) should be relative for the first cell in the apply range. Excel/Sheets automatically adjusts for each cell. This is more efficient than individual IFERROR wrappers when you want to show rather than hide errors.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 onward)
- **Error types detected:** All Excel error types including #GETTING_DATA, #CALC!, #SPILL!, #CONNECT!, #BLOCKED!, #UNKNOWN!, #FIELD!, #EXTERNAL!
- **Array behavior:** Works with dynamic arrays in Excel 365; Ctrl+Shift+Enter in legacy versions
- **Performance:** Extremely fast; simple type check operation
- **Note:** Some newer error types (#SPILL!, #CALC!) only exist in Excel 365

### Google Sheets

- **Availability:** All versions
- **Error types detected:** All Sheets error types including #ERROR! (generic), #N/A, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!
- **Array behavior:** Works with ARRAYFORMULA for range application
- **Sheets-specific:** Detects #ERROR! which is Sheets' generic error for various conditions
- **Performance:** Fast; equivalent to Excel implementation

### Key Difference Alert

Both platforms implement ISERROR identically for practical purposes. The main differences are in which specific error types exist on each platform. Excel 365 has introduced new error types for dynamic arrays (#SPILL!, #CALC!) that don't exist in Sheets. Sheets has #ERROR! for some conditions that would produce specific errors in Excel. ISERROR catches all errors on both platforms regardless of type.

## Tips and Best Practices

1. **Prefer IFERROR for simple replacement:** If you just want to replace errors with a fallback value, `=IFERROR(formula, fallback)` is cleaner and more efficient than `=IF(ISERROR(formula), fallback, formula)`.

2. **Be cautious about hiding all errors:** Blanket use of ISERROR can mask real problems. A #REF! error (broken reference) usually indicates something that needs fixing, not hiding.

3. **Consider ISERR + ISNA for nuanced handling:** When #N/A (lookup not found) should be treated differently from other errors, use ISERR to catch calculation errors and ISNA separately for lookup failures.

4. **Use for conditional formatting:** ISERROR excels in conditional formatting rules where you want to visually flag error cells without changing their values.

5. **Combine with LET for efficiency:** In Excel 365/Sheets, use LET to evaluate complex formulas once:
   ```
   =LET(result, ComplexFormula, IF(ISERROR(result), "Error", result))
   ```

6. **Don't use for data validation:** ISERROR detects errors after they occur. For preventing bad data entry, use Data Validation with proper criteria.

7. **Test specific error types when debugging:** While ISERROR catches everything, during debugging use ISNA, ISERR, or ERROR.TYPE to identify which specific error is occurring.

8. **Remember ISERROR returns Boolean:** The result is TRUE or FALSE, not the error value itself. Use ERROR.TYPE if you need to know which error occurred.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISERR]] | Returns TRUE for any error EXCEPT #N/A | When #N/A (lookup not found) should pass through without being treated as an error |
| [[ISNA]] | Returns TRUE only for #N/A | When you specifically want to catch only lookup failures |
| [[IFERROR]] | Returns alternate value if any error | When you want to replace errors with a fallback (more efficient than IF+ISERROR) |
| [[IFNA]] | Returns alternate value if #N/A | When you specifically want to handle only #N/A with a fallback |
| [[ERROR.TYPE]] | Returns number indicating error type | When you need to identify which specific error occurred |

### Commonly Used Together

**[[IF]]** - Conditional branching based on error status

*Classic error handling pattern:*
```
=IF(ISERROR(A1/B1), "Cannot calculate", A1/B1)
```
Before IFERROR, this was the standard approach. Still useful when you need complex branching beyond simple replacement.

---

**[[NOT]]** - Inverting error detection

*Finding cells without errors:*
```
=NOT(ISERROR(A1))
```
Returns TRUE for cells that do NOT contain errors. Useful in FILTER criteria: `=FILTER(A:D, NOT(ISERROR(C:C)))`.

---

**[[SUMPRODUCT]]** - Counting or summing with error filtering

*Counting all errors in a range:*
```
=SUMPRODUCT(--ISERROR(B2:B1000))
```
The double-dash converts TRUE/FALSE to 1/0 for counting. Essential for data quality metrics.

---

**[[FILTER]]** - Extracting non-error rows

*Getting only valid data rows:*
```
=FILTER(A1:D100, NOT(ISERROR(C1:C100)))
```
Returns complete rows where the criteria column (C) doesn't contain errors.

---

**[[AGGREGATE]]** - Error-tolerant aggregation (Excel)

*Alternative to ISERROR for sums/averages:*
```
=AGGREGATE(9, 6, A1:A100)
```
SUM (function 9) ignoring errors (option 6). More efficient than SUMPRODUCT+ISERROR for large ranges.

## Official Documentation

- **Microsoft Excel:** [ISERROR function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISERROR function](https://support.google.com/docs/answer/3093349)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | One of the original IS functions |
| Excel 2007 | IFERROR added | New function reduced need for IF+ISERROR pattern |
| Excel 2013 | IFNA added | More specific alternative for #N/A handling |
| Excel 365 | Dynamic arrays | New error types like #CALC!, #SPILL! also detected |
| Google Sheets | Original launch | Identical functionality to Excel |

---

*Last updated: 2026-01-10*
