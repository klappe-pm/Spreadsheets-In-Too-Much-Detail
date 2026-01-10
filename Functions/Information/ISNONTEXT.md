---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - type-checking
  - non-text-validation
  - data-type-detection
  - numeric-or-blank-test
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-non-text
  - nontext-check
  - not-text-test
  - numeric-blank-check
tags:
  - information
  - data-validation
  - type-checking
  - non-text-data
  - data-quality
  - excel
  - google-sheets
---

# ISNONTEXT

## Description

The **ISNONTEXT function** tests whether a value is NOT a text string and returns TRUE if the value is anything other than text, including numbers, logical values, errors, and blank cells. This function is the logical complement of ISTEXT—where ISTEXT returns TRUE, ISNONTEXT returns FALSE, and vice versa (with one exception: blank cells return TRUE for ISNONTEXT and FALSE for ISTEXT). ISNONTEXT is useful when you need to identify cells that contain non-textual data or verify that a value is suitable for numeric processing.

The primary use cases for ISNONTEXT include data validation for numeric columns, identifying cells safe for mathematical operations, and filtering out text from mixed-type data. While ISNUMBER specifically tests for numeric values, ISNONTEXT has a broader scope—it returns TRUE for numbers, blanks, errors, and logical values (anything that isn't text). This makes ISNONTEXT useful for "exclude text" logic rather than "include numbers only" logic. For example, in a column that should contain numbers or be empty, =ISNONTEXT(A1) catches both valid states, whereas ISNUMBER would flag blank cells as invalid.

Understanding ISNONTEXT's behavior with different data types is essential. TRUE cases: numbers (123), dates (dates are numbers internally), logical values (TRUE, FALSE), error values (#N/A, #VALUE!, etc.), and blank cells. FALSE cases: any text string, including "123" (numbers stored as text), "", empty strings from formulas, and single characters. The critical distinction from ISNUMBER: ISNONTEXT returns TRUE for blanks and errors, while ISNUMBER returns FALSE for both. This means ISNONTEXT is a looser "not text" check, not a strict "is number" check.

Platform behavior for ISNONTEXT is identical between Excel and Google Sheets. Both treat the same value types as non-text, and both handle blank cells consistently (returning TRUE). The function has been available since early versions of both platforms. In array contexts, ISNONTEXT works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365. The function pairs naturally with ISTEXT for complete type coverage.

## Syntax

> [!f(x)] ISNONTEXT Syntax
>
> ```
> =ISNONTEXT(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value, cell reference, or formula result to test. Returns TRUE for numbers, logical values (TRUE/FALSE), error values, dates, times, and blank cells. Returns FALSE for text strings including empty strings ("") and numbers stored as text ("123"). |

### Return Value

Returns **TRUE** if the value is anything other than text (numbers, dates, logicals, errors, blanks). Returns **FALSE** if the value is a text string. ISNONTEXT never produces an error—it always returns TRUE or FALSE.

## Examples

> [!f(x)] ISNONTEXT Examples

### Example 1: Testing a Number
```
=ISNONTEXT(42)
```
**Result:** TRUE

**Explanation:** Numbers are not text, so ISNONTEXT returns TRUE.

---

### Example 2: Testing Text
```
=ISNONTEXT("Hello")
```
**Result:** FALSE

**Explanation:** Text strings return FALSE—they ARE text, so they're not "non-text."

---

### Example 3: Testing a Blank Cell
```
=ISNONTEXT(A1)
```
**Scenario:** A1 is completely empty
**Result:** TRUE

**Explanation:** Blank cells are not text, so ISNONTEXT returns TRUE. This differs from ISNUMBER, which returns FALSE for blanks.

---

### Example 4: Testing a Number Stored as Text
```
=ISNONTEXT("123")
```
**Result:** FALSE

**Explanation:** "123" is text (it's a string that looks like a number). ISNONTEXT returns FALSE because the data type is text.

---

### Example 5: Testing an Empty String
```
=ISNONTEXT("")
```
**Result:** FALSE

**Explanation:** An empty string is still text—it's a string with zero characters. This differs from a blank cell.

---

### Example 6: Testing a Logical Value
```
=ISNONTEXT(TRUE)
=ISNONTEXT(FALSE)
```
**Result:** TRUE (both)

**Explanation:** Logical values are not text, so ISNONTEXT returns TRUE.

---

### Example 7: Testing an Error Value
```
=ISNONTEXT(#N/A)
```
**Result:** TRUE

**Explanation:** Error values are not text, so ISNONTEXT returns TRUE. This is useful because errors might appear in otherwise numeric columns.

---

### Example 8: Testing a Date
```
=ISNONTEXT(DATE(2024, 1, 15))
```
**Result:** TRUE

**Explanation:** Dates are stored as numbers internally, so they're not text. ISNONTEXT returns TRUE.

---

### Example 9: Comparing ISNONTEXT and ISTEXT
```
=ISNONTEXT(A1) = NOT(ISTEXT(A1))
```
**Scenario:** Test if ISNONTEXT is simply the opposite of ISTEXT
**Result:** TRUE for non-blank cells; FALSE for blank cells

**Explanation:** For most values, ISNONTEXT equals NOT(ISTEXT). However, blank cells return TRUE for ISNONTEXT and FALSE for ISTEXT, so NOT(ISTEXT(blank)) = TRUE, matching ISNONTEXT. The functions are complements including blank handling.

---

### Example 10: Validating Numeric/Blank Column
```
=IF(ISNONTEXT(A1), "OK", "Text found - check data")
```
**Scenario:** Validate that a column contains numbers or blanks, not text
**Result:** "OK" for numbers/blanks, warning for text

**Explanation:** ISNONTEXT accepts numbers AND blanks, making it suitable for columns where empty cells are acceptable but text is not.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `TRUE for blank cells` | Blanks are non-text | This is correct; add ISBLANK check if blanks need different handling |
| `TRUE for errors` | Errors are non-text | This is correct; add ISERROR check if errors should be flagged |
| `FALSE for "123"` | Numbers stored as text are text | This is correct; the data type matters, not appearance |
| `FALSE for empty string ""` | Empty string is text (zero-length string) | Distinguish from blank cells which return TRUE |
| `Confusion with ISNUMBER` | ISNONTEXT includes blanks/errors; ISNUMBER doesn't | Use ISNUMBER for strict numeric check; ISNONTEXT for "exclude text" |

## Use Cases

### [[Numeric Column Validation (Allowing Blanks)]]

**Scenario:** A data entry form has numeric fields where users must enter numbers or leave cells blank (for optional fields). Text entries are errors that need flagging. ISNUMBER would flag blanks as invalid, but blanks are acceptable in this case.

**Implementation:**
```
Field Validation (accepts numbers or blank):
=IF(ISNONTEXT(A1), "Valid", "ERROR: Text found")

Required Field (must be number):
=IF(ISBLANK(A1), "Required",
 IF(ISNUMBER(A1), "Valid",
    "ERROR: Must be numeric"))

Optional Field (number or blank OK, text invalid):
=IF(ISNONTEXT(A1),
    IF(ISBLANK(A1), "Optional - empty", "Valid: " & A1),
    "ERROR: Text not allowed")

Data Quality Summary:
Valid Entries: =SUMPRODUCT((ISNONTEXT(Column)-ISERROR(Column))*ISNUMBER(Column)*1)
Blank (Optional): =COUNTBLANK(Column)
Text Errors: =SUMPRODUCT(ISTEXT(Column)*1)
Other Errors: =SUMPRODUCT(ISERROR(Column)*1)
```

**Business Application:** Optional numeric fields are common in forms. Users might not have data for every field. ISNONTEXT validates that if something IS entered, it's not text. Blank is acceptable; "N/A" or "none" is not. This prevents data quality issues where text entries break downstream calculations.

**Technical Details:** The validation hierarchy is important: check required first, then type. For the summary, ISNONTEXT includes errors (which might be acceptable or not depending on context), so separate error counting helps. Consider whether formula results returning "" should be treated as blank (they're not—they're empty strings).

---

### [[Filter Non-Text Values]]

**Scenario:** A column contains mixed data—numbers, text labels, and blanks. An analyst needs to extract only the non-text values (numbers and blanks) for calculation while preserving their row positions.

**Implementation:**
```
Extract Non-Text Values:
=IF(ISNONTEXT(A1), A1, "")

Count Numeric Entries:
=SUMPRODUCT(ISNUMBER(DataColumn)*1)

Sum Non-Text Values (numbers only):
=SUMPRODUCT((ISNONTEXT(DataColumn)*ISNUMBER(DataColumn))*DataColumn)

Flag Text for Removal:
=IF(ISNONTEXT(A1), "Keep", "Remove")

Create Clean Array (Excel 365):
=FILTER(A1:A100, ISNONTEXT(A1:A100))
```

**Business Application:** Imported data often mixes numbers with text annotations. "N/A", "TBD", or notes might appear in numeric columns. ISNONTEXT helps identify which entries can be used in calculations. The extraction formula preserves row alignment for side-by-side comparison.

**Technical Details:** ISNONTEXT returns TRUE for blanks and errors. If you need strictly numbers, use ISNUMBER instead. The FILTER formula creates a new array of only non-text values—useful for calculations but loses row alignment. For row-preserving cleanup, use the IF(ISNONTEXT...) pattern.

---

### [[Data Type Audit]]

**Scenario:** A data governance team audits spreadsheets to ensure columns contain expected data types. They need to identify columns where text has crept into numeric fields and quantify the extent of type violations.

**Implementation:**
```
Cell Type Classification:
=IF(ISNUMBER(A1), "Number",
 IF(ISTEXT(A1), "Text",
 IF(ISBLANK(A1), "Blank",
 IF(ISLOGICAL(A1), "Boolean",
 IF(ISERROR(A1), "Error",
    "Unknown")))))

Column Type Purity (for expected numeric):
Text Contamination: =SUMPRODUCT(ISTEXT(Column)*1)
Pure (non-text): =SUMPRODUCT(ISNONTEXT(Column)*1)
Purity Rate: =TEXT(SUMPRODUCT(ISNONTEXT(Column)*1)/COUNTA(Column), "0.0%")

Multi-Column Audit Dashboard:
Column A: ="Text cells: " & SUMPRODUCT(ISTEXT(A:A)*1)
Column B: ="Text cells: " & SUMPRODUCT(ISTEXT(B:B)*1)
...

Contamination Location Finder:
=IF(AND(ISTEXT(A2), Expected_Type="Numeric"), ROW(A2), "")
```

**Business Application:** Data type violations cause calculation errors, sorting issues, and report problems. Regular audits identify columns with text contamination. The purity rate metric tracks data quality over time. Location finder helps data stewards correct specific issues.

**Technical Details:** The classification formula uses nested IFs to categorize every possible type. For large audits, use SUMPRODUCT to count by type without helper columns. Track purity rates over time to identify trends—increasing text contamination might indicate a source system issue or process problem.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 and later)
- **Blank handling:** Returns TRUE (blanks are not text)
- **Error handling:** Returns TRUE (errors are not text)
- **Array behavior:** Native dynamic arrays in Excel 365
- **Complementary function:** ISTEXT for opposite test

### Google Sheets

- **Availability:** All versions since launch
- **Blank handling:** Returns TRUE (blanks are not text)
- **Error handling:** Returns TRUE (errors are not text)
- **Array behavior:** Requires ARRAYFORMULA wrapper
- **Complementary function:** ISTEXT for opposite test

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Behavior | Identical | Identical |
| Blank cells | TRUE | TRUE |
| Error values | TRUE | TRUE |
| Empty string "" | FALSE | FALSE |
| Text numbers | FALSE | FALSE |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |

## Tips and Best Practices

1. **ISNONTEXT includes blanks:** Unlike ISNUMBER, ISNONTEXT returns TRUE for blank cells. This is useful for "number or empty" validation but might not be what you want for required fields.

2. **ISNONTEXT includes errors:** Error values like #N/A return TRUE because they're not text. Add ISERROR check if errors should be flagged separately.

3. **Not the same as ISNUMBER:** ISNUMBER is stricter—only TRUE for actual numbers. ISNONTEXT is broader—TRUE for anything that ISN'T text.

4. **Empty string vs blank:** ISNONTEXT("") returns FALSE (empty string is text); ISNONTEXT(blank) returns TRUE. This distinction matters for formula results.

5. **Use for "exclude text" logic:** When you need to accept multiple non-text types (numbers, blanks, dates), ISNONTEXT is simpler than combining ISNUMBER with OR(ISBLANK...).

6. **Pairs with ISTEXT:** For complete type coverage, use ISTEXT and ISNONTEXT together. Every non-error, non-blank value is either text or non-text.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISTEXT]] | Tests if value is text | When you need to identify text specifically |
| [[ISNUMBER]] | Tests if value is numeric | When you need strict numeric check (excludes blanks/errors) |
| [[ISBLANK]] | Tests if cell is empty | When you need to identify blank cells specifically |
| [[TYPE]] | Returns number indicating data type | When you need specific type identification |
| [[ISERROR]] | Tests if value is any error | When you need to handle errors separately from ISNONTEXT |

### Commonly Used Together

**[[IF]]** - Conditional logic based on text status

*Validate non-text entry:*
```
=IF(ISNONTEXT(A1), "Valid", "Text not allowed")
```
Accepts numbers, blanks, dates—rejects text.

---

**[[ISBLANK]]** - Separate blank handling

*Check for number specifically:*
```
=IF(ISBLANK(A1), "Empty", IF(ISNONTEXT(A1), "Non-text value", "Text"))
```
Distinguishes blank from other non-text values.

---

**[[ISTEXT]]** - Complementary function

*Complete classification:*
```
=IF(ISTEXT(A1), "Is Text", "Is Not Text")
```
ISTEXT and ISNONTEXT cover all possibilities.

---

**[[ISERROR]]** - Separate error handling

*Non-text excluding errors:*
```
=IF(AND(ISNONTEXT(A1), NOT(ISERROR(A1))), "Clean", "Problem")
```
Identifies non-text values that aren't errors.

---

**[[SUMPRODUCT]]** - Count non-text values

*Count non-text entries:*
```
=SUMPRODUCT(ISNONTEXT(A1:A100)*1)
```
Counts cells containing anything other than text.

## Official Documentation

- **Microsoft Excel:** [ISNONTEXT function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISNONTEXT function](https://support.google.com/docs/answer/3093295)

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
| Excel 365 | 2018+ | Native dynamic array support |
| Excel for Mac | 2001+ | Full support |
| Excel Online | 2010+ | Full support |
| Google Sheets | 2006 | Available since launch |
| LibreOffice Calc | 1.0 | Full compatibility |
| Apple Numbers | 2007 | Full support |

---

*Last updated: 2026-01-10*
