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
- Is Error Except NA
- Error Check Without NA
- Non-NA Error Test
tags:
- error-checking
- error-handling
- validation
- is-functions
- boolean
---

# ISERR

## Description

**ISERR** checks whether a value is an error, but with one critical exception: it excludes the #N/A error from its detection. It returns TRUE for #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, and #GETTING_DATA errors, but returns FALSE for #N/A. This distinction makes ISERR specifically useful when you want to treat #N/A differently from other errors, typically because #N/A from lookup functions has a different meaning (value not found) than calculation errors.

The reason ISERR exists alongside ISERROR is rooted in how spreadsheets handle lookup failures. When VLOOKUP, MATCH, or XLOOKUP cannot find a value, they return #N/A, which is semantically different from #DIV/0! (math error) or #REF! (broken reference). In many workflows, "not found" isn't an error condition at all, it's expected data. A product not in the discount table means no discount applies, a missing employee means they're not enrolled. ISERR lets you catch genuine formula problems while allowing #N/A to propagate or be handled separately.

**The IS functions family:** ISERR is part of a family of information functions that test cell contents: ISERROR (any error), ISNA (only #N/A), ISERR (any error except #N/A), ISNUMBER, ISTEXT, ISBLANK, ISLOGICAL, ISREF, and more. Understanding which IS function to use prevents the common mistake of using ISERROR everywhere, which can mask legitimate "not found" conditions that should trigger different logic.

**Modern alternative consideration:** Since IFERROR and IFNA were introduced, many scenarios that previously required ISERR inside IF statements can now be handled more elegantly. Instead of `=IF(ISERR(formula), fallback, formula)` which evaluates the formula twice, use `=IFERROR(formula, fallback)` for any error, or combine approaches for nuanced handling. However, ISERR remains valuable for conditional logic that needs to distinguish between error types without immediately replacing the error with a value.

## Syntax

> [!f(x)] ISERR Syntax
>
> ```
> =ISERR(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value to test for errors. Can be a cell reference, formula result, or literal value. ISERR evaluates this value and returns TRUE if it is any error type except #N/A. |

### Return Value

Returns TRUE if the value is one of: #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, #GETTING_DATA, or #CALC! (Excel 365). Returns FALSE if the value is #N/A, or if the value is not an error at all (numbers, text, logical values, blanks).

### Error Detection Matrix

| Error Type | ISERR | ISERROR | ISNA | Typical Cause |
|------------|-------|---------|------|---------------|
| #DIV/0! | TRUE | TRUE | FALSE | Division by zero |
| #VALUE! | TRUE | TRUE | FALSE | Wrong argument type |
| #REF! | TRUE | TRUE | FALSE | Invalid cell reference |
| #NAME? | TRUE | TRUE | FALSE | Unrecognized formula name |
| #NUM! | TRUE | TRUE | FALSE | Invalid numeric value |
| #NULL! | TRUE | TRUE | FALSE | Incorrect range operator |
| #N/A | **FALSE** | TRUE | TRUE | Value not found (lookups) |
| #GETTING_DATA | TRUE | TRUE | FALSE | External data loading |
| #CALC! | TRUE | TRUE | FALSE | Array calculation error |

## Examples

> [!f(x)] ISERR Examples

### Example 1: Basic Division Error Detection
```
=ISERR(10/0)
```
**Result:** TRUE

**Explanation:** Division by zero produces #DIV/0!, which ISERR detects as TRUE. This is a genuine calculation error that typically indicates a formula problem or unexpected zero in a denominator. ISERR correctly identifies this as an error condition.

---

### Example 2: Testing #N/A (The Key Difference)
```
=ISERR(VLOOKUP("NotFound", A1:B10, 2, FALSE))
```
**Result:** FALSE (if value not found)

**Explanation:** When VLOOKUP cannot find the lookup value, it returns #N/A. ISERR returns FALSE for #N/A because it explicitly excludes this error type. This is the critical difference between ISERR and ISERROR. Use this behavior when "not found" should be treated differently than "calculation error."

---

### Example 3: Value Error Detection
```
=ISERR("text"+5)
```
**Result:** TRUE

**Explanation:** Adding text to a number produces #VALUE! error. ISERR correctly identifies this as an error. This typically indicates a formula is receiving the wrong type of input, such as text in a cell that should contain a number.

---

### Example 4: Reference Error Detection
```
=ISERR(#REF!)
```
**Result:** TRUE

**Explanation:** #REF! errors occur when a formula references a cell that has been deleted. ISERR catches this error type. Unlike #N/A which often means "data not yet available," #REF! indicates structural problems with the spreadsheet that need fixing.

---

### Example 5: Using ISERR with IF for Selective Error Handling
```
=IF(ISERR(A1/B1), "Calculation Error", IF(ISNA(A1/B1), "N/A", A1/B1))
```
**Result:** Shows "Calculation Error" for #DIV/0!, "N/A" for #N/A, otherwise the result

**Explanation:** This pattern distinguishes between error types. ISERR catches calculation errors (like division by zero), ISNA would catch not-found errors, and valid results pass through. Useful when you need different messaging for different error conditions.

---

### Example 6: ISERR in Conditional Formatting Logic
```
=ISERR(INDIRECT("Sheet"&A1&"!A1"))
```
**Result:** TRUE if the sheet doesn't exist, FALSE if it does

**Explanation:** INDIRECT to a non-existent sheet returns #REF!, which ISERR detects. This technique can validate sheet names or check if referenced sheets exist. Useful for building dynamic multi-sheet formulas with error checking.

---

### Example 7: Detecting Text-to-Number Conversion Errors
```
=ISERR(VALUE(A1))
```
**Result:** TRUE if A1 contains text that cannot convert to a number

**Explanation:** VALUE() tries to convert text to a number, returning #VALUE! if it fails. ISERR detects this conversion failure. Use this pattern to validate that data can be treated numerically before performing calculations.

---

### Example 8: Checking Formula Validity
```
=IF(ISERR(B2*C2/D2), "Check inputs", B2*C2/D2)
```
**Result:** "Check inputs" if calculation fails (except for #N/A), otherwise the result

**Explanation:** A simple error-checking wrapper around a calculation. Any genuine calculation error (wrong types, division by zero, bad references) displays a warning. This prevents error values from propagating into reports while allowing #N/A to flow through if desired.

---

### Example 9: Counting Calculation Errors in a Range
```
=SUMPRODUCT(--ISERR(A1:A100))
```
**Result:** Count of cells containing errors (excluding #N/A)

**Explanation:** ISERR returns TRUE/FALSE for each cell in the range. The double negative (--) converts TRUE to 1 and FALSE to 0. SUMPRODUCT sums these to count errors. This excludes #N/A from the count, useful for quality checks that consider #N/A as acceptable.

---

### Example 10: Filtering Out Calculation Errors (Modern Excel/Sheets)
```
=FILTER(A1:C100, NOT(ISERR(B1:B100)), "No valid data")
```
**Result:** All rows where column B does not have a calculation error

**Explanation:** ISERR tests each row in column B. NOT() inverts the result (we want non-error rows). FILTER returns only rows passing the test. This specifically allows #N/A rows to pass through, which ISERROR would have excluded.

---

### Example 11: ISERR vs ISERROR Comparison
```
Cell A1: =1/0 (produces #DIV/0!)
Cell A2: =VLOOKUP("x",B:C,2,0) (produces #N/A)

=ISERR(A1) returns TRUE
=ISERR(A2) returns FALSE
=ISERROR(A1) returns TRUE
=ISERROR(A2) returns TRUE
```
**Result:** Shows the difference in handling #N/A

**Explanation:** This comparison demonstrates the critical difference. Both cells contain errors, but ISERR treats them differently. #DIV/0! in A1 is detected by both functions. #N/A in A2 is only detected by ISERROR, not by ISERR. Choose your function based on whether you want #N/A treated as an error.

---

### Example 12: Nested ISERR for Error Categorization
```
=IF(ISERR(A1), "Formula Error", IF(ISNA(A1), "Not Found", A1))
```
**Result:** Categorizes error type with specific messages

**Explanation:** A categorization pattern that provides different user feedback based on error type. "Formula Error" indicates something needs fixing in the formula or data. "Not Found" indicates missing lookup data, which might be expected. Helps users understand what action to take.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Unexpected FALSE for #N/A | ISERR by design returns FALSE for #N/A | Use ISERROR if you want to catch all errors including #N/A. ISERR specifically excludes #N/A. |
| Formula evaluates twice | Using ISERR(formula) and formula in same IF | Consider IFERROR or IFNA for efficiency. Pattern `IF(ISERR(f),alt,f)` evaluates f twice. |
| Wrong function choice | Using ISERR when you meant ISERROR | Clarify your requirement: do you want to catch #N/A or not? Use ISERROR for all errors, ISERR to exclude #N/A. |
| Missing cell reference | ISERR without parentheses or argument | ISERR requires exactly one argument: `=ISERR(A1)` not `=ISERR` |
| Testing literal error | `=ISERR(#REF!)` entered directly | While this works for testing, note that literal error values in formulas may cause issues. Reference cells containing errors instead. |

## Use Cases

### [[Lookup Validation with Error Distinction]]

**Scenario:** A pricing formula uses VLOOKUP to find product prices, but you need to distinguish between "product not in catalog" (#N/A) versus "pricing formula error" (other errors).

**Implementation:**
```
=IF(ISERR(VLOOKUP(A2,PriceTable,2,0)*B2), "Pricing Error - Check Formula",
    IF(ISNA(VLOOKUP(A2,PriceTable,2,0)), "Not in Catalog - Use List Price",
    VLOOKUP(A2,PriceTable,2,0)*B2))
```

**Business Application:** Order entry personnel need different guidance for different problems. A pricing error (perhaps the lookup returns text that can't multiply with quantity) needs IT review. A product not in the catalog simply needs manual pricing. ISERR enables this distinction.

**Technical Details:** This pattern evaluates the lookup multiple times, which is inefficient. In modern Excel, consider LET to calculate once: `=LET(price, VLOOKUP(A2,PriceTable,2,0)*B2, IF(ISERR(price),"Error", IF(ISNA(price),"Not Found",price)))`. The separation of ISERR and ISNA checks creates clear, actionable categories.

---

### [[Data Import Quality Assessment]]

**Scenario:** After importing data from an external system, validate that calculations work on the imported data, but expect some lookup values to be missing.

**Implementation:**
```
=SUMPRODUCT(--ISERR(E2:E1000))
```
Count of rows with formula errors (excluding expected #N/A values)

**Business Application:** Data quality dashboards that distinguish "structural problems" (formula errors indicating data format issues) from "missing reference data" (#N/A from lookups against incomplete reference tables). IT focuses on the former while data stewards handle the latter.

**Technical Details:** Apply ISERR across entire calculation columns to identify systematic issues. A high count of ISERR suggests data format problems (dates imported as text, numbers with currency symbols, etc.). #N/A counts would be tracked separately with ISNA.

---

### [[Conditional Error Display]]

**Scenario:** A financial model should show #N/A when data isn't available yet (acceptable) but flag and explain other errors (need investigation).

**Implementation:**
```
=IF(ISERR(CalculatedField), "ERR: Check inputs", CalculatedField)
```

**Business Application:** Financial models shared with executives should be clean and interpretable. Showing "ERR: Check inputs" tells viewers to investigate, while allowing #N/A to appear indicates "we're waiting for this data." This contextual error display improves model usability.

**Technical Details:** The key insight is that #N/A in financial models often means "period not yet complete" or "forecast not entered." This is different from #VALUE! (formula receiving wrong input) or #DIV/0! (unexpected zero denominator). ISERR preserves this semantic distinction.

---

### [[Multi-Source Data Integration]]

**Scenario:** Combining data from multiple sources where #N/A indicates "not applicable" (valid business meaning) but other errors indicate integration problems.

**Implementation:**
```
=IF(ISERR(Data1!A2+Data2!A2+Data3!A2), "Integration Error",
    IF(ISNA(Data1!A2)+ISNA(Data2!A2)+ISNA(Data3!A2)>0, "Partial Data",
    Data1!A2+Data2!A2+Data3!A2))
```

**Business Application:** Consolidated reports combining regional data. If any region marks a line item as N/A, the consolidated view shows "Partial Data" (still useful). If there's an actual calculation error (wrong data types, bad references), the consolidated view shows "Integration Error" (needs fixing).

**Technical Details:** ISERR catches cross-source issues like text vs number mismatches that would cause #VALUE!. The ISNA check on individual sources identifies which specific sources have N/A values for further investigation.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 onward)
- **Error types detected:** #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, #GETTING_DATA, #CALC!
- **Error types NOT detected:** #N/A
- **Array behavior:** Can be applied to ranges in Excel 365 with array formula behavior
- **Performance:** Fast; simple comparison operation

### Google Sheets

- **Availability:** All versions
- **Error types detected:** #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, #ERROR!
- **Error types NOT detected:** #N/A
- **Array behavior:** Can be used with ARRAYFORMULA for range application
- **Sheets-specific:** Also returns TRUE for Sheets' generic #ERROR! type

### Key Difference Alert

Both platforms implement ISERR identically in terms of excluding #N/A from detection. The main difference is in which specific error types exist on each platform (#GETTING_DATA is Excel-specific, #ERROR! is Sheets-specific). For practical purposes, ISERR behaves the same: catches everything except #N/A.

## Tips and Best Practices

1. **Know when to use ISERR vs ISERROR:** Use ISERR when #N/A from lookups is expected and should be handled differently. Use ISERROR when you want to catch all errors without distinction.

2. **Consider IFERROR/IFNA for simpler patterns:** Instead of `=IF(ISERR(formula), alt, formula)`, use `=IFERROR(formula, alt)` if you don't need to distinguish #N/A. It's cleaner and only evaluates the formula once.

3. **Use ISERR for formula debugging:** When a complex formula returns errors, use ISERR on component parts to isolate which section is causing problems (excluding lookup-related #N/A).

4. **Combine with ISNA for complete error handling:**
   ```
   =IF(ISERR(x), "Calc Error", IF(ISNA(x), "Not Found", x))
   ```
   This pattern provides clear categorization of all error types.

5. **Remember ISERR returns Boolean:** The result is TRUE or FALSE, not the error type. If you need to identify which specific error occurred, you'll need additional logic.

6. **Avoid in data validation rules:** ISERR doesn't prevent errors; it only detects them after they occur. Use proper data validation to prevent errors at data entry time.

7. **Test edge cases:** Verify behavior with blank cells, text, numbers, and each error type to ensure your formula handles all scenarios correctly.

8. **Document your error handling strategy:** When building complex workbooks, note which errors you're catching with ISERR and why you're treating #N/A differently.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISERROR]] | Returns TRUE for any error including #N/A | When you want to catch ALL errors without exception |
| [[ISNA]] | Returns TRUE only for #N/A | When you specifically want to detect only #N/A errors |
| [[IFERROR]] | Returns alternate value if error | When you want to replace errors with a fallback value (simpler than IF+ISERR) |
| [[IFNA]] | Returns alternate value if #N/A | When you specifically want to handle only #N/A with a fallback |
| [[ERROR.TYPE]] | Returns a number indicating the error type | When you need to identify the specific error type for detailed handling |

### Commonly Used Together

**[[IF]]** - Conditional branching based on error status

*Using ISERR with IF for error handling:*
```
=IF(ISERR(A1/B1), "Cannot calculate", A1/B1)
```
The classic pattern for catching calculation errors while letting #N/A pass through.

---

**[[ISNA]]** - Detecting specifically #N/A errors

*Complete error categorization:*
```
=IF(ISERR(A1), "Error", IF(ISNA(A1), "N/A", A1))
```
Handle different error types with different logic. ISERR catches everything except #N/A, ISNA catches #N/A.

---

**[[VLOOKUP]] / [[XLOOKUP]]** - Lookup functions that return #N/A

*Why ISERR exists - distinguishing lookup failures:*
```
=IF(ISERR(VLOOKUP(A1,Data,2,0)*B1), "Data Error", IFERROR(VLOOKUP(A1,Data,2,0)*B1, "Not Found"))
```
First checks if there's a calculation error (the multiplication fails), then handles lookup failure separately.

---

**[[SUMPRODUCT]]** - Counting errors across ranges

*Counting calculation errors in a dataset:*
```
=SUMPRODUCT(--ISERR(B2:B1000))
```
Counts cells with errors, excluding #N/A values from the count.

---

**[[NOT]]** - Inverting the error check

*Finding cells without calculation errors:*
```
=NOT(ISERR(A1))
```
Returns TRUE when the cell does not contain a calculation error (including when it contains #N/A or valid data).

## Official Documentation

- **Microsoft Excel:** [ISERR function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISERR function](https://support.google.com/docs/answer/3093349)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Part of original IS function family |
| Excel 2007+ | All subsequent | No changes to basic function |
| Excel 365 | Dynamic arrays | Works with array formulas natively |
| Google Sheets | Original launch | Identical functionality to Excel |

---

*Last updated: 2026-01-10*
