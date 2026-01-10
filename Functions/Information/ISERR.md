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
  - na-exclusion
  - selective-errors
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-err
  - error-except-na
  - non-na-error
  - error-check-no-na
tags:
  - information
  - error-handling
  - error-detection
  - na-exclusion
  - formula-debugging
  - excel
  - google-sheets
---

# ISERR

## Description

The **ISERR function** is a specialized error-detection function that tests whether a value is an error EXCEPT for #N/A, returning TRUE for all error types like #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, and #NULL!, but returning FALSE for #N/A errors. This distinction is critical because #N/A typically indicates "value not found" in lookup operations—an expected and often acceptable condition—while other errors usually indicate genuine problems with formulas, data types, or references. By excluding #N/A from its detection, ISERR allows you to handle "real" errors differently from the common "not found" scenario that occurs naturally in lookup-driven spreadsheets.

The primary use case for ISERR is in formulas where #N/A errors are expected and acceptable, but other errors are not. Consider a price list lookup: if a product isn't in the list, #N/A is a normal outcome that you might display as "Price TBD." However, if the formula produces #REF! because someone deleted the price column, that's a serious problem requiring immediate attention. Using =IF(ISERR(formula), "FORMULA ERROR", IF(ISNA(formula), "Not Listed", formula)) lets you distinguish between these scenarios. ISERR is also valuable in data validation contexts where you want to flag calculation problems while allowing lookup failures to show their standard #N/A result.

The key distinction between ISERR and ISERROR is subtle but important. ISERROR catches ALL errors including #N/A; ISERR catches all errors EXCEPT #N/A. This means: ISERROR(#N/A) returns TRUE, while ISERR(#N/A) returns FALSE. For lookup functions like VLOOKUP, INDEX+MATCH, and XLOOKUP, the #N/A error is semantically meaningful—it tells users "this value doesn't exist in the data." Suppressing #N/A with ISERROR treats "not found" the same as "broken formula," which loses valuable information. ISERR preserves #N/A visibility while catching genuine errors. However, if you want to handle #N/A specifically, pair ISERR with ISNA for complete control.

Platform behavior for ISERR is identical between Excel and Google Sheets, with both treating #N/A as the only excluded error type. The function has been available since early versions of both platforms. In Excel 365, ISERR recognizes the newer #CALC! and #SPILL! errors (returning TRUE for both). In array contexts, ISERR works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365. The function never produces an error itself—it always returns TRUE or FALSE. This makes ISERR safe to use in any formula context without additional error protection.

## Syntax

> [!f(x)] ISERR Syntax
>
> ```
> =ISERR(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value, cell reference, or formula result to test for errors (excluding #N/A). Can be any Excel/Sheets expression. Returns TRUE for #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME?, #NULL!, #CALC!, #SPILL!. Returns FALSE for #N/A and all non-error values. |

### Return Value

Returns **TRUE** if the value is any error type EXCEPT #N/A. Returns **FALSE** if the value is #N/A or any non-error value (numbers, text, logical values, blank cells). ISERR never produces an error—it always returns TRUE or FALSE.

### Error Types Detected by ISERR

| Error | Detected? | Meaning |
|-------|-----------|---------|
| `#VALUE!` | **TRUE** | Wrong argument type |
| `#REF!` | **TRUE** | Invalid cell reference |
| `#DIV/0!` | **TRUE** | Division by zero |
| `#NUM!` | **TRUE** | Invalid numeric operation |
| `#NAME?` | **TRUE** | Unrecognized name/function |
| `#NULL!` | **TRUE** | Incorrect range operator |
| `#CALC!` | **TRUE** | Calculation engine error (Excel 365) |
| `#SPILL!` | **TRUE** | Blocked spill range (Excel 365) |
| `#N/A` | **FALSE** | Not available (lookup not found) |

## Examples

> [!f(x)] ISERR Examples

### Example 1: Testing #DIV/0! Error
```
=ISERR(1/0)
```
**Result:** TRUE

**Explanation:** Division by zero produces #DIV/0!, which ISERR detects. This is a "real" error that ISERR is designed to catch.

---

### Example 2: Testing #N/A Error
```
=ISERR(NA())
```
**Result:** FALSE

**Explanation:** #N/A is specifically excluded from ISERR detection. This is the key difference from ISERROR—ISERR returns FALSE for #N/A.

---

### Example 3: Comparing ISERR and ISERROR
```
ISERR(#N/A):    FALSE
ISERROR(#N/A):  TRUE
```
**Explanation:** This demonstrates the fundamental difference. ISERROR catches ALL errors; ISERR catches all EXCEPT #N/A. Choose based on whether #N/A should be treated as an error in your context.

---

### Example 4: VLOOKUP with ISERR for Formula Protection
```
=IF(ISERR(VLOOKUP(A1, Prices, 2, FALSE)), "FORMULA ERROR", VLOOKUP(A1, Prices, 2, FALSE))
```
**Scenario:** Look up product price, catching formula errors but not "not found"
**Result:** Returns price if found, #N/A if not found, "FORMULA ERROR" for other errors

**Explanation:** If the lookup produces #VALUE!, #REF!, etc., you see "FORMULA ERROR." If the product isn't found (#N/A), the #N/A displays as-is. This preserves the meaningful distinction between "not in list" and "broken formula."

---

### Example 5: Dual Error Handling with IF
```
=IF(ISERR(formula), "Formula Error", IF(ISNA(formula), "Not Found", formula))
```
**Scenario:** Handle #N/A and other errors differently
**Result:** Three possible outcomes: the result, "Not Found", or "Formula Error"

**Explanation:** This pattern gives complete control over error handling. ISERR catches real errors, ISNA catches the specific "not found" case, and valid results pass through unchanged.

---

### Example 6: Division with Selective Error Handling
```
=IF(ISERR(Revenue/Expenses), "Calculation Error", Revenue/Expenses)
```
**Scenario:** Calculate ratio but flag division problems
**Result:** Returns ratio or "Calculation Error" for #DIV/0!, #VALUE!, etc.

**Explanation:** If Expenses is zero, you get #DIV/0! caught by ISERR. If either cell contains text, #VALUE! is caught. The formula handles genuine calculation problems.

---

### Example 7: Counting Non-NA Errors
```
=SUMPRODUCT(ISERR(A1:A100)*1)
```
**Scenario:** Count errors in a range, excluding #N/A values
**Result:** Number of cells with errors other than #N/A

**Explanation:** This counts "real" errors while ignoring #N/A. Useful when #N/A is expected in lookup columns but other errors indicate data problems.

---

### Example 8: Conditional Format to Highlight Problem Errors
```
Conditional Formatting Formula: =ISERR(A1)
```
**Scenario:** Highlight cells with errors, but not #N/A
**Result:** Cells with #VALUE!, #REF!, etc. are highlighted; #N/A cells are not

**Explanation:** Perfect for lookup results where #N/A is acceptable but other errors need attention. Visual distinction helps users focus on real problems.

---

### Example 9: Combined with ISNA for Complete Control
```
=SWITCH(TRUE, ISERR(A1), "ERROR: Check formula", ISNA(A1), "Not Available", A1)
```
**Scenario:** Provide specific feedback for different conditions
**Result:** Different messages for errors vs #N/A vs valid values

**Explanation:** SWITCH with TRUE evaluates conditions in order. ISERR catches real errors first, then ISNA handles #N/A, and valid values fall through to display normally.

---

### Example 10: Array Formula Error Analysis (Google Sheets)
```
=ARRAYFORMULA(IF(ISERR(A1:A10/B1:B10), "Calc Error", IF(ISNA(A1:A10/B1:B10), "NA", A1:A10/B1:B10)))
```
**Scenario:** Process arrays with differentiated error handling
**Result:** Array of results with appropriate error messages per cell

**Explanation:** Each row is evaluated independently. Division errors show "Calc Error," NA values show "NA," and successful calculations show results.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `Expected TRUE for #N/A but got FALSE` | ISERR excludes #N/A by design | Use ISERROR if you want to catch #N/A, or use ISNA specifically |
| `Can't distinguish error types` | ISERR only knows "error or not" | Use ERROR.TYPE after ISERR to identify specific error types |
| `Formula shows #N/A when I wanted to handle it` | ISERR doesn't catch #N/A | Add ISNA check: =IF(ISERR(x), "err", IF(ISNA(x), "na", x)) |
| `ISERR returning FALSE for error-looking text` | Text "#N/A" is not an error value | Only actual error values are detected; text is just text |
| `Double evaluation performance` | IF+ISERR pattern evaluates twice | No IFERR function exists; consider helper cells or LET function |
| `Confusion with ISERROR` | Names are similar, behavior differs | Remember: ISERR = ISERRor minus #N/A |
| `Array formula not working` | Older Excel requires CSE | Use Ctrl+Shift+Enter, or upgrade to Excel 365 |

## Use Cases

### [[Lookup System with Formula Integrity Monitoring]]

**Scenario:** A inventory system uses VLOOKUP to pull product information from a master list. When products aren't in the master list, #N/A is expected and acceptable—it simply means the product is new. However, if someone accidentally deletes the master list columns or the formula references break, those errors must be immediately visible and not confused with "product not found."

**Implementation:**
```
Standard Lookup with ISERR Protection:
=IF(ISERR(VLOOKUP(ProductID, MasterList, 2, FALSE)),
    "FORMULA ERROR - Contact Admin",
    VLOOKUP(ProductID, MasterList, 2, FALSE))

Enhanced Three-Way Handler:
=IF(ISERR(VLOOKUP(ProductID, MasterList, 2, FALSE)),
    "SYSTEM ERROR",
    IF(ISNA(VLOOKUP(ProductID, MasterList, 2, FALSE)),
        "New Product - Add to Master",
        VLOOKUP(ProductID, MasterList, 2, FALSE)))

Error Monitoring Dashboard:
Formula Errors: =SUMPRODUCT(ISERR(LookupColumn)*1)
Missing Products: =SUMPRODUCT(ISNA(LookupColumn)*1)
Clean Records: =COUNTA(LookupColumn)-SUMPRODUCT((ISERROR(LookupColumn))*1)

Formula Integrity Check:
=IF(SUMPRODUCT(ISERR(AllLookups)*1)>0,
    "ALERT: " & SUMPRODUCT(ISERR(AllLookups)*1) & " formula errors detected!",
    "All formulas functioning normally")
```

**Business Application:** Inventory systems must distinguish between "product doesn't exist yet" and "the lookup is broken." If a #REF! error is treated the same as #N/A, users might think products simply aren't in the system when actually the entire lookup mechanism is broken. ISERR ensures formula integrity issues are immediately visible while allowing normal "not found" results to display. The integrity check provides early warning of systemic problems.

**Technical Details:** The three-way handler uses nested IFs, testing ISERR first (it's more critical), then ISNA, then success. The dashboard separates error types for monitoring. Consider adding logging: when ISERR returns TRUE, log the cell address and timestamp to a separate sheet for admin review. This creates an audit trail of formula issues.

---

### [[Financial Model Validation]]

**Scenario:** A financial model contains complex calculations with dependencies on multiple inputs. Some calculations legitimately result in #N/A when data isn't applicable (e.g., year-over-year growth in year 1), but other errors indicate model problems. The model needs validation that distinguishes between acceptable #N/A scenarios and genuine calculation errors.

**Implementation:**
```
Year-over-Year Growth (N/A acceptable in year 1):
=IF(ISERR((CurrentYear-PriorYear)/ABS(PriorYear)),
    "MODEL ERROR",
    (CurrentYear-PriorYear)/ABS(PriorYear))

Model Health Summary:
Calculation Errors: =SUMPRODUCT(ISERR(ModelOutputs)*1)
N/A Values (expected): =SUMPRODUCT(ISNA(ModelOutputs)*1)
Valid Calculations: =COUNTA(ModelOutputs)-SUMPRODUCT(ISERROR(ModelOutputs)*1)

Model Validation Status:
=IF(SUMPRODUCT(ISERR(ModelOutputs)*1)>0,
    "MODEL HAS ERRORS - DO NOT DISTRIBUTE",
    IF(SUMPRODUCT(ISNA(ModelOutputs)*1)>ExpectedNAs,
        "Review: More N/A values than expected",
        "Model validates successfully"))

Calculation Chain Validator:
=IF(ISERR(Step1), "Error in Step 1",
 IF(ISERR(Step2), "Error in Step 2",
 IF(ISERR(Step3), "Error in Step 3",
    "Chain complete")))
```

**Business Application:** Financial models distributed to executives or used for decisions must be error-free. But "error" has nuance—#N/A for first-year growth rate is mathematically correct, not an error. ISERR allows models to flag genuine problems (#DIV/0! from zero revenue, #REF! from broken references) while accepting legitimate #N/A values. The validation status provides a single go/no-go indicator for model distribution.

**Technical Details:** The calculation chain validator tests each step in sequence, identifying exactly where errors originate. This is more useful than wrapping the entire chain in ISERR because it pinpoints the failure point. For complex models, maintain an "expected N/A count" that reflects legitimate N/A scenarios, and alert if actual count differs significantly.

---

### [[Data Import Quality Assurance]]

**Scenario:** A data pipeline imports CSV files and transforms values using formulas. Some source values legitimately don't map to references (#N/A from lookups), but transformation errors like type mismatches (#VALUE!) or missing lookup tables (#REF!) indicate import problems. QA needs to distinguish between "source data gaps" and "import process failures."

**Implementation:**
```
Transformation with Quality Classification:
=IF(ISERR(TransformFormula),
    "TRANSFORM_ERROR",
    IF(ISNA(TransformFormula),
        "SOURCE_GAP",
        TransformFormula))

Record Quality Status:
=IF(SUMPRODUCT(ISERR(B2:Z2)*1)>0,
    "FAILED: Contains transformation errors",
    IF(SUMPRODUCT(ISNA(B2:Z2)*1)>0,
        "PARTIAL: Has unmapped values",
        "COMPLETE: All fields transformed"))

Import Batch Summary:
Total Records: =COUNTA(A:A)-1
Failed (ISERR): =COUNTIF(StatusColumn, "FAILED*")
Partial (ISNA only): =COUNTIF(StatusColumn, "PARTIAL*")
Complete: =COUNTIF(StatusColumn, "COMPLETE*")
Success Rate: =TEXT(COUNTIF(StatusColumn,"COMPLETE*")/(COUNTA(A:A)-1), "0.0%")

Process Health Alert:
=IF(COUNTIF(StatusColumn,"FAILED*")>0,
    "IMPORT FAILURE: " & COUNTIF(StatusColumn,"FAILED*") & " records have transformation errors. Pipeline may be misconfigured.",
    IF(COUNTIF(StatusColumn,"PARTIAL*")/(COUNTA(A:A)-1)>0.1,
        "DATA QUALITY: " & TEXT(COUNTIF(StatusColumn,"PARTIAL*")/(COUNTA(A:A)-1),"0%") & " of records have unmapped values",
        "Import successful"))
```

**Business Application:** Data imports fail for different reasons requiring different responses. ISERR errors (transformation failures) mean the import pipeline has problems—maybe a lookup table is missing or formula references are wrong. ISNA results (unmapped values) mean the source data has values that don't match reference tables—a data quality issue, not a pipeline issue. The health alert directs attention appropriately: pipeline issues to IT, data quality issues to data stewards.

**Technical Details:** Status column formulas should run ISERR first since those errors are more critical. The success rate metric uses complete records only—partial records might be usable but need review. Consider adding a "quarantine" workflow where ISERR records go to one sheet (for IT), ISNA-only records go to another (for data cleanup), and complete records proceed to the database.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 and later)
- **Error types detected:** All Excel error types EXCEPT #N/A (includes #CALC!, #SPILL! in Excel 365)
- **Array behavior:** Native dynamic arrays in Excel 365; CSE required in older versions
- **Performance:** Highly optimized single-cell evaluation
- **Complement:** Use ISNA to specifically test for #N/A

### Google Sheets

- **Availability:** All versions since launch
- **Error types detected:** All Sheets error types EXCEPT #N/A
- **Array behavior:** Requires ARRAYFORMULA wrapper for array operations
- **Performance:** Efficient for single cells and arrays
- **Complement:** Use ISNA to specifically test for #N/A

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| #N/A handling | Returns FALSE | Returns FALSE |
| #CALC! detection | TRUE (365) | N/A (no #CALC! in Sheets) |
| #SPILL! detection | TRUE (365) | N/A (no #SPILL! in Sheets) |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |
| Companion function | ISNA, ISERROR | ISNA, ISERROR |
| Performance | Excellent | Excellent |

## Tips and Best Practices

1. **Remember the mnemonic:** ISERR = ISERROR minus NA. This helps you remember that ISERR catches everything EXCEPT #N/A.

2. **Use for lookups where #N/A is meaningful:** When your lookup returning #N/A conveys useful information ("not in list"), use ISERR to catch only problematic errors while preserving the #N/A display.

3. **Combine with ISNA for complete control:** The pattern =IF(ISERR(x), "Error", IF(ISNA(x), "Not Found", x)) gives you three distinct outcomes. This is more informative than ISERROR, which lumps everything together.

4. **Use in conditional formatting:** Apply conditional formatting with =ISERR(A1) to highlight cells with "real" errors while leaving #N/A cells unmarked. This visual distinction helps users focus on problems.

5. **No IFERR function exists:** Unlike IFERROR, there's no IFERR function. Use the IF+ISERR pattern, or consider using LET to avoid formula repetition: =LET(result, formula, IF(ISERR(result), "error", result)).

6. **Count errors separately:** Use SUMPRODUCT(ISERR(range)*1) for non-NA errors and SUMPRODUCT(ISNA(range)*1) for NA values. This separation provides better insight into data quality issues.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISERROR]] | Tests for ALL errors including #N/A | When #N/A should be treated as an error |
| [[ISNA]] | Tests ONLY for #N/A | When you specifically want to handle lookup not-found |
| [[ERROR.TYPE]] | Returns number identifying error type | When you need to distinguish between specific error types |
| [[IFERROR]] | Returns alternative if any error | For simple error handling (note: catches #N/A too) |
| [[IFNA]] | Returns alternative if #N/A only | For handling lookup not-found specifically |

### Commonly Used Together

**[[IF]]** - Conditional logic based on error status

*Handle errors while preserving #N/A:*
```
=IF(ISERR(VLOOKUP(A1,B:C,2,FALSE)), "Error!", VLOOKUP(A1,B:C,2,FALSE))
```
Returns the lookup result or #N/A if not found, but "Error!" for formula problems.

---

**[[ISNA]]** - Complete error categorization

*Distinguish between error types:*
```
=IF(ISERR(A1), "Formula Error", IF(ISNA(A1), "Not Found", A1))
```
Three-way handling: real errors, #N/A, and valid values each handled differently.

---

**[[SUMPRODUCT]]** - Count errors excluding #N/A

*Count only "real" errors:*
```
=SUMPRODUCT(ISERR(A1:A100)*1)
```
Counts #VALUE!, #REF!, #DIV/0!, etc., but not #N/A values.

---

**[[SWITCH]]** - Multi-way error handling

*Elegant error categorization:*
```
=SWITCH(TRUE, ISERR(A1), "Error", ISNA(A1), "N/A", A1)
```
SWITCH with TRUE evaluates conditions in order for clean multi-way logic.

---

**[[LET]]** - Avoid formula repetition

*Efficient error checking:*
```
=LET(result, VLOOKUP(A1,B:C,2,FALSE), IF(ISERR(result), "Error", result))
```
LET stores the lookup result, eliminating the need to write it twice.

## Official Documentation

- **Microsoft Excel:** [ISERR function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISERR function](https://support.google.com/docs/answer/3093348)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 97 | 1997 | Initial release |
| Excel 2003 | 2003 | No changes |
| Excel 2007 | 2007 | No changes |
| Excel 2010 | 2010 | No changes |
| Excel 2013 | 2013 | No changes |
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
