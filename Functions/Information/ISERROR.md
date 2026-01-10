---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - error-detection
  - error-checking
  - formula-validation
  - data-quality
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-error
  - error-check
  - error-test
  - any-error
tags:
  - information
  - error-handling
  - error-detection
  - data-validation
  - formula-debugging
  - excel
  - google-sheets
---

# ISERROR

## Description

The **ISERROR function** is a fundamental error-detection function that tests whether a value is any error type and returns TRUE if an error is detected, or FALSE if the value is valid. It recognizes all Excel/Sheets error types including #N/A, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, and in Excel 365, #CALC! and #SPILL!. This function is the broadest error-catching function available—if something is an error, ISERROR will detect it. The function takes a single argument and performs a binary test: either the value is an error (TRUE) or it is not (FALSE). ISERROR is essential for building robust formulas that can handle problematic data gracefully.

The primary use cases for ISERROR include conditional logic that branches based on error status, data validation to identify problematic records, and formula auditing to find calculation issues. When combined with IF, ISERROR creates patterns like IF(ISERROR(formula), alternative_value, formula) which was the standard error-handling approach before IFERROR was introduced. This pattern evaluates the formula twice (once in the test, once for the result), but provides more flexibility than IFERROR when you need different logic for error versus non-error cases. ISERROR is also valuable for counting errors in datasets using SUMPRODUCT(ISERROR(range)*1) and for conditional formatting rules that highlight error cells.

There are important considerations when using ISERROR that affect formula design. First, ISERROR catches ALL errors indiscriminately, which can mask important problems. If your VLOOKUP fails because of a #REF! error (deleted column) rather than #N/A (value not found), ISERROR treats both the same way. For lookup functions specifically, ISNA is often more appropriate as it only catches #N/A. Second, the IF+ISERROR pattern evaluates the formula twice, which impacts performance for complex calculations. IFERROR evaluates once and is more efficient. Third, ISERROR cannot tell you WHICH error occurred—use ERROR.TYPE for that. Fourth, ISERROR returns FALSE for blank cells, text, numbers, and logical values—it only returns TRUE for actual error values.

Platform behavior for ISERROR is essentially identical between Excel and Google Sheets, making it highly portable. Both platforms recognize the same error types (with Excel 365 having additional #CALC! and #SPILL! that don't exist in Sheets). The function has been available since early versions of both platforms. In array contexts, ISERROR evaluates each cell in a range when used with ARRAYFORMULA (Sheets) or in dynamic array formulas (Excel 365). Performance characteristics are similar across platforms, though the IF+ISERROR pattern's double evaluation impacts both equally.

## Syntax

> [!f(x)] ISERROR Syntax
>
> ```
> =ISERROR(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value, cell reference, or formula result to test for errors. Can be any Excel/Sheets expression. If this evaluates to any error type (#N/A, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, #CALC!, #SPILL!), returns TRUE. For any non-error value (numbers, text, blanks, logical values), returns FALSE. |

### Return Value

Returns **TRUE** if the value is any error type. Returns **FALSE** if the value is not an error (including numbers, text, logical values, blank cells, and empty strings). ISERROR itself never produces an error—it always returns TRUE or FALSE.

### Error Types Detected by ISERROR

| Error | Meaning | Common Cause |
|-------|---------|--------------|
| `#N/A` | Not Available | Lookup function can't find value |
| `#VALUE!` | Value Error | Wrong argument type for function |
| `#REF!` | Reference Error | Invalid cell reference (deleted cells/sheets) |
| `#DIV/0!` | Division by Zero | Dividing by zero or empty cell |
| `#NUM!` | Number Error | Invalid numeric operation |
| `#NAME?` | Name Error | Unrecognized function or name |
| `#NULL!` | Null Error | Incorrect range operator |
| `#CALC!` | Calculation Error | Array engine error (Excel 365 only) |
| `#SPILL!` | Spill Error | Blocked spill range (Excel 365 only) |

## Examples

> [!f(x)] ISERROR Examples

### Example 1: Basic Error Detection
```
=ISERROR(#DIV/0!)
```
**Result:** TRUE

**Explanation:** Directly testing an error value returns TRUE. While you wouldn't typically write this literal form, it demonstrates that ISERROR recognizes error values.

---

### Example 2: Testing a Valid Cell
```
=ISERROR(A1)
```
**Scenario:** A1 contains the number 100
**Result:** FALSE

**Explanation:** Numbers are not errors, so ISERROR returns FALSE. This applies to any valid value: numbers, text, blanks, TRUE/FALSE.

---

### Example 3: Detecting Division by Zero
```
=ISERROR(A1/B1)
```
**Scenario:** A1 contains 100, B1 contains 0
**Result:** TRUE

**Explanation:** Division by zero produces #DIV/0!, which ISERROR detects. This is a common pattern for testing whether a calculation will produce an error before using the result.

---

### Example 4: Classic Error Handling Pattern with IF
```
=IF(ISERROR(A1/B1), 0, A1/B1)
```
**Scenario:** Safely perform division, returning 0 if error occurs
**Result:** Returns 0 if B1 is zero, otherwise returns the division result

**Explanation:** This is the traditional error-handling pattern that preceded IFERROR. Note that A1/B1 is evaluated twice—once in ISERROR and once for the result. For better performance, use =IFERROR(A1/B1, 0).

---

### Example 5: Testing VLOOKUP Results
```
=ISERROR(VLOOKUP(A1, B:C, 2, FALSE))
```
**Scenario:** Check if a lookup will succeed before using the result
**Result:** TRUE if value not found, FALSE if found

**Explanation:** VLOOKUP returns #N/A when the lookup value isn't found. ISERROR catches this (and any other error). However, ISNA is more precise for this use case since it only catches #N/A.

---

### Example 6: Conditional Display Based on Error Status
```
=IF(ISERROR(VLOOKUP(A1, Prices, 2, FALSE)), "Price not found", VLOOKUP(A1, Prices, 2, FALSE))
```
**Scenario:** Display a message when product price lookup fails
**Result:** Returns price if found, or "Price not found" message

**Explanation:** Provides user-friendly feedback instead of showing #N/A error. Note the formula repetition—this is where IFERROR is more elegant: =IFERROR(VLOOKUP(...), "Price not found").

---

### Example 7: Counting Errors in a Range
```
=SUMPRODUCT(ISERROR(A1:A100)*1)
```
**Scenario:** Count how many cells in a range contain errors
**Result:** Returns the count of error values

**Explanation:** ISERROR evaluates each cell, returning TRUE/FALSE. Multiplying by 1 converts to 1/0. SUMPRODUCT totals these, giving the error count. Essential for data quality monitoring.

---

### Example 8: Array Error Detection (Google Sheets)
```
=ARRAYFORMULA(IF(ISERROR(A1:A10/B1:B10), "Error", A1:A10/B1:B10))
```
**Scenario:** Process entire columns, handling errors row by row
**Result:** Returns calculation results or "Error" for each row

**Explanation:** ARRAYFORMULA allows ISERROR to work on ranges. Each cell pair is evaluated independently, with errors caught and replaced.

---

### Example 9: Nested Error Checking
```
=IF(ISERROR(A1), "A1 Error", IF(ISERROR(B1), "B1 Error", A1+B1))
```
**Scenario:** Identify which input caused an error
**Result:** Reports which cell has an error, or performs calculation

**Explanation:** Sequential ISERROR tests can pinpoint error sources. This is useful for debugging complex calculations where multiple inputs could cause failures.

---

### Example 10: Testing Complex Formulas
```
=ISERROR(INDEX(C:C, MATCH(A1, B:B, 0)))
```
**Scenario:** Validate that an INDEX+MATCH lookup will succeed
**Result:** TRUE if any error occurs in the nested formula

**Explanation:** ISERROR wraps the entire expression, catching errors from either MATCH (not found) or INDEX (invalid result). One test covers multiple potential failure points.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `ISERROR returns FALSE for empty cells` | Empty cells are not errors | This is correct behavior; use ISBLANK to test for empty cells |
| `Performance issues with IF+ISERROR` | Formula evaluated twice | Use IFERROR instead, which evaluates once |
| `Can't distinguish error types` | ISERROR catches all errors equally | Use ERROR.TYPE for error type identification, or ISNA for #N/A specifically |
| `Masking real formula problems` | ISERROR hides #REF!, #NAME? errors | Use ISNA for lookups; review source formulas periodically |
| `ISERROR returns FALSE for error-like text` | Text "#N/A" is not an error value | This is correct; only actual error values return TRUE |
| `Array formula not expanding` | Older Excel requires CSE | Press Ctrl+Shift+Enter, or use Excel 365 for native arrays |
| `Unexpected TRUE results` | Cell contains formula producing error | Check the cell's formula, not just displayed value |
| `ISERROR not catching imported errors` | Text that looks like error but isn't | Imported "#N/A" as text isn't a real error; check with =TYPE(cell) |

## Use Cases

### [[Data Quality Monitoring Dashboard]]

**Scenario:** A data analyst receives daily feeds from multiple source systems. The feeds sometimes contain calculation errors, invalid references, or data that causes downstream formula failures. The analyst needs a dashboard that quantifies data quality issues and identifies records requiring attention.

**Implementation:**
```
Error Count by Column:
Column A Errors: =SUMPRODUCT(ISERROR(A:A)*1)
Column B Errors: =SUMPRODUCT(ISERROR(B:B)*1)
Column C Errors: =SUMPRODUCT(ISERROR(C:C)*1)

Total Errors:
=SUMPRODUCT(ISERROR(A:C)*1)

Error Rate:
=TEXT(SUMPRODUCT(ISERROR(A2:C1000)*1)/(COUNTA(A2:C1000)+0.0001), "0.00%")

Error Location Finder:
=IF(ISERROR(A2), ROW(A2), "") -- Applied to helper column, then filter non-blank

Conditional Format Formula (highlight error cells):
=ISERROR(A1)

Data Quality Score:
=100 - (SUMPRODUCT(ISERROR(DataRange)*1) / COUNT(DataRange) * 100) & "%"
```

**Business Application:** Data quality directly impacts business decisions. A dashboard showing error counts and locations enables proactive data cleanup. If a particular column consistently has errors, it indicates a systemic issue with the source system. Tracking error rates over time shows whether data quality is improving or degrading. The error location finder helps analysts quickly navigate to problem records rather than scrolling through thousands of rows.

**Technical Details:** SUMPRODUCT with ISERROR is more efficient than COUNTIF for counting errors because COUNTIF can't directly count error values. Use helper columns to flag error rows for filtering and review. Consider adding ERROR.TYPE analysis to categorize errors: are they mostly #N/A (missing data) or #VALUE! (type mismatches)? This distinction guides different remediation approaches.

---

### [[Protected Calculation System]]

**Scenario:** A financial model contains interconnected calculations where one error can cascade through dozens of dependent formulas, making the entire model unusable. The model needs circuit breakers that contain errors and allow unaffected calculations to proceed normally.

**Implementation:**
```
Individual Calculation Protection:
Revenue: =IF(ISERROR(UnitsSold * PricePerUnit), "[Calc Error]", UnitsSold * PricePerUnit)

Dependent Calculation with Error Propagation Stop:
Gross Margin: =IF(OR(ISERROR(Revenue), ISERROR(COGS)),
                  "[Upstream Error]",
                  Revenue - COGS)

Percentage with Division Protection:
Margin %: =IF(ISERROR(GrossMargin/Revenue),
              IF(Revenue=0, "[No Revenue]", "[Calc Error]"),
              GrossMargin/Revenue)

Model Health Check:
=SUMPRODUCT(ISERROR(AllCalculations)*1) & " calculations in error state"

Error Source Identification:
=IF(ISERROR(A1), "ERROR SOURCE: " & CELL("address",A1), "OK")
```

**Business Application:** Financial models are often reviewed in meetings where immediate debugging isn't possible. Protected calculations ensure that a single data issue doesn't render the entire model useless. The distinction between "[Calc Error]" and "[Upstream Error]" helps users understand whether an issue originated in a particular calculation or was inherited from a dependency. This maintains model utility even during data problems and makes troubleshooting more efficient.

**Technical Details:** Layer your error protection strategically—protect fundamental calculations first, then add cascade detection for dependent formulas. Use consistent error messages so users can distinguish between error types. Consider a "Model Integrity" cell that tests critical calculations and shows a summary status. For very complex models, create an error log sheet that captures which cells are in error state each time the model calculates.

---

### [[Automated Data Validation Pipeline]]

**Scenario:** An operations team imports CSV data weekly from vendors. The data goes through transformation formulas before loading into a database. Any errors in the transformation would cause the database import to fail. The pipeline needs to validate all transformations, quarantine error records, and only pass clean records to the database.

**Implementation:**
```
Row-Level Error Check:
=IF(OR(ISERROR(B2), ISERROR(C2), ISERROR(D2), ISERROR(E2)), "QUARANTINE", "PASS")

Comprehensive Row Validator:
=IF(SUMPRODUCT(ISERROR(B2:E2)*1)>0,
    "ERROR: " & SUMPRODUCT(ISERROR(B2:E2)*1) & " fields failed",
    "VALID")

Clean Data Filter:
=FILTER(A2:E100, NOT(ISERROR(F2:F100)))  -- Where F is the validation column

Error Summary:
Total Records: =COUNTA(A2:A100)
Clean Records: =COUNTIF(F2:F100, "PASS")
Error Records: =COUNTIF(F2:F100, "QUARANTINE")
Success Rate: =TEXT(COUNTIF(F2:F100,"PASS")/COUNTA(A2:A100), "0.0%")

Error Detail Extraction:
=FILTER(A2:F100, G2:G100="QUARANTINE")  -- Extract all quarantined records
```

**Business Application:** Database imports must be reliable—partial or corrupted imports cause worse problems than failed imports. The validation pipeline ensures that only fully-transformed records proceed to the database. Quarantined records can be manually reviewed and corrected without blocking the entire batch. The success rate metric helps track vendor data quality over time, providing leverage for improvement discussions.

**Technical Details:** Use a helper column to consolidate all field-level error checks into a single row status. This simplifies the FILTER formula that separates clean and error records. Consider adding timestamp and batch ID columns to track when errors occurred. For recurring imports, maintain an error log that accumulates over time to identify systematic issues with specific vendors or data fields.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 and later)
- **Error types detected:** All Excel error types including #CALC! and #SPILL! in Excel 365
- **Array behavior:** Native dynamic arrays in Excel 365; CSE required in older versions
- **Performance:** Single cell evaluation is highly optimized
- **Alternative:** IFERROR function available from Excel 2007 for simpler error handling

### Google Sheets

- **Availability:** All versions since launch
- **Error types detected:** All Sheets error types (#N/A, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!)
- **Array behavior:** Requires ARRAYFORMULA wrapper for array operations
- **Performance:** Efficient for single cells and arrays
- **Alternative:** IFERROR function available for streamlined error handling

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Error types covered | All 9 including #CALC!, #SPILL! | All 7 standard error types |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |
| #CALC! detection | Yes (365) | N/A (no #CALC! in Sheets) |
| #SPILL! detection | Yes (365) | N/A (no #SPILL! in Sheets) |
| Performance | Excellent | Excellent |
| IFERROR alternative | 2007+ | All versions |
| In conditional formatting | Yes | Yes |

## Tips and Best Practices

1. **Prefer IFERROR for simple error handling:** If you just need to return an alternative value when an error occurs, use =IFERROR(formula, alternative) instead of =IF(ISERROR(formula), alternative, formula). IFERROR is cleaner and more efficient since it only evaluates the formula once.

2. **Use ISNA for lookup functions:** When handling VLOOKUP, INDEX+MATCH, or similar functions, ISNA is more appropriate than ISERROR. ISNA only catches #N/A (value not found), while ISERROR catches ALL errors including #REF! (deleted columns) that indicate formula problems you should know about.

3. **Combine with ERROR.TYPE for categorization:** When you need to handle different errors differently, use ERROR.TYPE after ISERROR: =IF(ISERROR(formula), IF(ERROR.TYPE(formula)=7, "Not found", "Other error"), formula). This provides precise error handling.

4. **Count errors efficiently with SUMPRODUCT:** =SUMPRODUCT(ISERROR(range)*1) is the standard pattern for counting errors in a range. The *1 converts TRUE/FALSE to 1/0 for summing.

5. **Don't suppress all errors blindly:** While ISERROR enables graceful error handling, suppressing all errors can hide real problems. Periodically review your data for cells using error alternatives. Consider logging when errors are caught.

6. **Test with actual error conditions:** Verify your ISERROR logic by intentionally creating error conditions. Delete a lookup value, enter zero for a divisor, or break a reference to confirm your error handling works correctly.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISNA]] | Tests for #N/A error only | When you specifically want to catch "not found" errors from lookups |
| [[ISERR]] | Tests for all errors EXCEPT #N/A | When #N/A is expected and acceptable but other errors aren't |
| [[ERROR.TYPE]] | Returns number identifying error type | When you need to distinguish between different error types |
| [[IFERROR]] | Returns alternative value if error | For simple error handling without conditional logic |
| [[IFNA]] | Returns alternative value if #N/A | For lookup error handling specifically |

### Commonly Used Together

**[[IF]]** - Conditional logic based on error status

*Classic error handling pattern:*
```
=IF(ISERROR(VLOOKUP(A1, Table, 2, FALSE)), "Not Found", VLOOKUP(A1, Table, 2, FALSE))
```
IF+ISERROR was the standard before IFERROR. Still useful when you need complex logic for error cases.

---

**[[SUMPRODUCT]]** - Counting errors in ranges

*Count error cells in a dataset:*
```
=SUMPRODUCT(ISERROR(A1:Z100)*1)
```
SUMPRODUCT evaluates ISERROR on each cell, converts to numbers, and sums them for a total error count.

---

**[[OR]]** - Testing multiple cells for errors

*Check if any cell in a set has an error:*
```
=IF(OR(ISERROR(A1), ISERROR(B1), ISERROR(C1)), "Error found", "All OK")
```
OR combines multiple ISERROR tests to check if any input has problems.

---

**[[NOT]]** - Inverting error test

*Find cells that are NOT errors:*
```
=NOT(ISERROR(A1))
```
NOT inverts the test, returning TRUE for valid cells and FALSE for errors.

---

**[[IFERROR]]** - Streamlined error handling

*Replace ISERROR+IF pattern:*
```
Instead of: =IF(ISERROR(A1/B1), 0, A1/B1)
Use: =IFERROR(A1/B1, 0)
```
IFERROR is more concise and efficient for simple error replacement scenarios.

## Official Documentation

- **Microsoft Excel:** [ISERROR function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISERROR function](https://support.google.com/docs/answer/3093349)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 97 | 1997 | Initial release |
| Excel 2003 | 2003 | No changes |
| Excel 2007 | 2007 | IFERROR introduced as complement |
| Excel 2010 | 2010 | No changes |
| Excel 2013 | 2013 | IFNA introduced |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | 2018+ | Detects #CALC! and #SPILL! errors |
| Excel for Mac | 2001+ | Full support |
| Excel Online | 2010+ | Full support |
| Google Sheets | 2006 | Available since Sheets launch |
| LibreOffice Calc | 1.0 | Full compatibility |
| Apple Numbers | 2007 | Full support |

---

*Last updated: 2026-01-10*
