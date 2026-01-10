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
  - boolean-validation
  - logical-values
  - true-false-test
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-logical
  - boolean-check
  - true-false-test
  - logical-validation
tags:
  - information
  - data-validation
  - type-checking
  - boolean-data
  - logical-values
  - excel
  - google-sheets
---

# ISLOGICAL

## Description

The **ISLOGICAL function** tests whether a value is a logical (Boolean) value and returns TRUE if the value is TRUE or FALSE, or returns FALSE for any other data type including numbers, text, errors, or blank cells. This function is essential for type checking when you need to confirm that a cell contains an actual Boolean value rather than something that merely looks like one or can be coerced to one. ISLOGICAL specifically recognizes only two values: the logical TRUE and the logical FALSE. It does not consider the text "TRUE" or "FALSE", the numbers 1 or 0, or any other representation as logical values.

The primary use cases for ISLOGICAL include validating checkbox or toggle inputs, ensuring proper data types in Boolean columns, and distinguishing logical values from their text or numeric equivalents. A checkbox in Excel or Google Sheets produces an actual logical value when checked (TRUE) or unchecked (FALSE)—ISLOGICAL can verify this. Similarly, some formulas return logical results (like comparison operators A1>B1), and ISLOGICAL confirms the result is truly Boolean. This matters because logical TRUE and text "TRUE" behave differently in some contexts: logical TRUE equals 1 in arithmetic but text "TRUE" causes errors.

Understanding what ISLOGICAL considers "logical" requires precision. TRUE cases: the logical value TRUE, the logical value FALSE, any formula result that produces TRUE or FALSE (like =A1>5, =AND(...), =OR(...), =NOT(...)). FALSE cases: the text strings "TRUE" or "FALSE" (text, not logical), the numbers 1 or 0 (numeric, not logical), cells showing TRUE/FALSE due to formatting (the underlying value might be 1/0), blank cells, and error values. This strictness is ISLOGICAL's strength—it won't be fooled by text or numbers that look like Boolean values but aren't.

Platform behavior for ISLOGICAL is identical between Excel and Google Sheets. Both recognize only the actual logical TRUE and FALSE values, not text or numeric representations. The function has been available since early versions of both platforms. In array contexts, ISLOGICAL works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365. A practical note: when copying data between systems, logical values sometimes convert to text or numbers—ISLOGICAL helps identify these conversions.

## Syntax

> [!f(x)] ISLOGICAL Syntax
>
> ```
> =ISLOGICAL(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value, cell reference, or formula result to test. Returns TRUE only if the value is the logical TRUE or logical FALSE. Returns FALSE for numbers (including 1 and 0), text (including "TRUE" and "FALSE"), errors, and blank cells. |

### Return Value

Returns **TRUE** if the value is a logical TRUE or FALSE. Returns **FALSE** for all other data types. ISLOGICAL never produces an error—it always returns TRUE or FALSE.

## Examples

> [!f(x)] ISLOGICAL Examples

### Example 1: Testing Logical TRUE
```
=ISLOGICAL(TRUE)
```
**Result:** TRUE

**Explanation:** The logical value TRUE is recognized as a logical type. This is one of only two values for which ISLOGICAL returns TRUE.

---

### Example 2: Testing Logical FALSE
```
=ISLOGICAL(FALSE)
```
**Result:** TRUE

**Explanation:** The logical value FALSE is also a logical type. TRUE and FALSE are the only two logical values.

---

### Example 3: Testing a Comparison Result
```
=ISLOGICAL(5>3)
```
**Result:** TRUE

**Explanation:** Comparison operators return logical values. 5>3 evaluates to TRUE, which is a logical value.

---

### Example 4: Testing the Number 1
```
=ISLOGICAL(1)
```
**Result:** FALSE

**Explanation:** The number 1 is numeric, not logical. Even though TRUE coerces to 1 in arithmetic, 1 itself is not a logical value.

---

### Example 5: Testing the Number 0
```
=ISLOGICAL(0)
```
**Result:** FALSE

**Explanation:** The number 0 is numeric, not logical. FALSE coerces to 0, but 0 is not inherently logical.

---

### Example 6: Testing Text "TRUE"
```
=ISLOGICAL("TRUE")
```
**Result:** FALSE

**Explanation:** The text string "TRUE" is text, not a logical value. This distinction is important—text "TRUE" and logical TRUE behave differently in formulas.

---

### Example 7: Testing a Blank Cell
```
=ISLOGICAL(A1)
```
**Scenario:** A1 is empty
**Result:** FALSE

**Explanation:** Blank cells are not logical values. They're empty—no data type at all.

---

### Example 8: Testing AND Function Result
```
=ISLOGICAL(AND(A1>0, B1>0))
```
**Scenario:** Test the result type of a logical function
**Result:** TRUE

**Explanation:** AND() returns a logical TRUE or FALSE, which ISLOGICAL recognizes as logical values.

---

### Example 9: Checkbox Validation
```
=IF(ISLOGICAL(CheckboxCell), "Valid checkbox", "Invalid")
```
**Scenario:** Verify that a checkbox cell contains an actual logical value
**Result:** "Valid checkbox" if TRUE/FALSE, "Invalid" otherwise

**Explanation:** Excel/Sheets checkboxes produce logical TRUE/FALSE. This validates that the cell contains a proper checkbox value, not text or a number.

---

### Example 10: Counting Logical Values
```
=SUMPRODUCT(ISLOGICAL(A1:A100)*1)
```
**Scenario:** Count how many cells contain actual logical values
**Result:** Number of cells with TRUE or FALSE

**Explanation:** Counts only cells with actual logical values, not cells with 1, 0, "TRUE", or "FALSE".

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `FALSE for 1 or 0` | Numbers aren't logical values | This is correct; use =value=TRUE OR value=FALSE if you want to accept 1/0 |
| `FALSE for text "TRUE"` | Text isn't a logical value | This is correct; convert with =value="TRUE" or use proper logical input |
| `FALSE for formatted checkbox` | Cell might show TRUE but contain 1 | Verify checkbox setup; recreate if it contains number instead of logical |
| `Unexpected FALSE for formula result` | Formula might return text or number | Check the formula's return type with TYPE() |
| `TRUE in other language Excel` | Excel translates TRUE/FALSE to local language | Logical values are recognized regardless of display language |

## Use Cases

### [[Checkbox Data Validation]]

**Scenario:** A project tracking spreadsheet uses checkboxes for task completion status. Some cells might have been manually edited to contain "TRUE", "Yes", 1, or other non-checkbox values. The project manager needs to ensure all status cells contain proper checkbox values for accurate filtering and formulas.

**Implementation:**
```
Checkbox Validation:
=IF(ISLOGICAL(StatusCell), "Valid", "Invalid - reset checkbox")

Task Status Display:
=IF(ISLOGICAL(StatusCell),
    IF(StatusCell, "Complete", "Pending"),
    "ERROR: Invalid status")

Data Integrity Check:
Valid Checkboxes: =SUMPRODUCT(ISLOGICAL(AllStatuses)*1)
Invalid Entries: =ROWS(AllStatuses)-SUMPRODUCT(ISLOGICAL(AllStatuses)*1)

Conditional Completion Count:
=SUMPRODUCT((ISLOGICAL(AllStatuses)*AllStatuses)*1)

Reset Invalid Entries Identifier:
=IF(NOT(ISLOGICAL(A2)), ROW(A2), "") -- Use to find rows needing correction
```

**Business Application:** Checkboxes should contain logical TRUE/FALSE values. Manual edits or copy-paste operations can replace checkbox values with text or numbers that look the same but behave differently. COUNTIF(Status, TRUE) only counts logical TRUE values—text "TRUE" is not counted. This validation ensures formulas work correctly and filtering by TRUE/FALSE operates as expected.

**Technical Details:** Recreate invalid checkboxes by: 1) Clear the cell, 2) Insert checkbox via menu. The integrity check quantifies the problem. For large datasets, consider using conditional formatting with =NOT(ISLOGICAL(A1)) to highlight invalid entries for manual correction.

---

### [[Boolean Column Type Enforcement]]

**Scenario:** A data validation system imports Boolean flags from external sources. Due to varying export formats, some flags arrive as logical TRUE/FALSE, others as 1/0, and others as text "true"/"false". The system needs to normalize all values to actual logical type while tracking what needs conversion.

**Implementation:**
```
Type Analysis:
Logical Values: =SUMPRODUCT(ISLOGICAL(FlagColumn)*1)
Numeric (1/0): =SUMPRODUCT((ISNUMBER(FlagColumn)*(FlagColumn=1)+ISNUMBER(FlagColumn)*(FlagColumn=0))*1)
Text: =SUMPRODUCT(ISTEXT(FlagColumn)*1)
Other/Blank: =ROWS(FlagColumn)-SUMPRODUCT((ISLOGICAL(FlagColumn)+ISNUMBER(FlagColumn)+ISTEXT(FlagColumn))*1)

Normalization Formula:
=IF(ISLOGICAL(A2), A2,
 IF(AND(ISNUMBER(A2), OR(A2=1, A2=0)), A2=1,
 IF(AND(ISTEXT(A2), OR(UPPER(A2)="TRUE", UPPER(A2)="FALSE")), UPPER(A2)="TRUE",
    "INVALID")))

Conversion Status:
=IF(ISLOGICAL(A2), "Already logical",
 IF(AND(ISNUMBER(A2), OR(A2=1, A2=0)), "Convert from number",
 IF(AND(ISTEXT(A2), OR(UPPER(A2)="TRUE", UPPER(A2)="FALSE")), "Convert from text",
    "Cannot convert")))
```

**Business Application:** Boolean data arriving in different formats causes inconsistencies. A column mixing logical TRUE, number 1, and text "True" will have unpredictable behavior in formulas. This analysis quantifies the issue by format type. The normalization formula standardizes everything to logical values while flagging unconvertible data.

**Technical Details:** The normalization formula checks ISLOGICAL first (already correct), then tests for valid numeric (must be exactly 1 or 0), then tests for valid text (case-insensitive "true" or "false"). The conversion uses =1 and ="TRUE" comparisons which return logical values. Unconvertible data (like 2, "yes", or blank) is flagged for manual review.

---

### [[Formula Result Type Verification]]

**Scenario:** A template designer creates complex formulas that should return TRUE/FALSE for conditional formatting and other logic. Before releasing the template, they need to verify that all Boolean-intended formulas actually return logical values, not text or numbers that coincidentally match.

**Implementation:**
```
Formula Result Type Check:
=IF(ISLOGICAL(FormulaCell), "Returns logical",
 IF(AND(FormulaCell=TRUE, TYPE(FormulaCell)<>4), "Returns 1 (not logical)",
 IF(AND(FormulaCell=FALSE, TYPE(FormulaCell)<>4), "Returns 0 (not logical)",
    "Returns other type")))

Template Validation Summary:
Correct Logical Returns: =SUMPRODUCT(ISLOGICAL(AllBooleanCells)*1)
Needs Fix: =COUNTA(AllBooleanCells)-SUMPRODUCT(ISLOGICAL(AllBooleanCells)*1)

Detailed Diagnosis:
=IF(ISLOGICAL(A2), "OK",
    "Cell " & ADDRESS(ROW(A2),COLUMN(A2)) & " returns " & TYPE(A2) & " not logical")
```

**Business Application:** Templates distributed to others must work reliably. A formula that returns 1 instead of TRUE might work in most cases but fail in specific conditional formatting scenarios or COUNTIF operations. Pre-release validation catches these subtle issues before users encounter them.

**Technical Details:** TYPE() returns 4 for logical values. Combined with ISLOGICAL, this provides comprehensive type checking. Common causes of non-logical returns: using 1 and 0 directly instead of TRUE and FALSE in IF statements, mathematical operations that convert logical to numeric, or string concatenation that converts to text.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 and later)
- **Checkbox integration:** Returns TRUE for valid checkbox TRUE/FALSE
- **Array behavior:** Native dynamic arrays in Excel 365; CSE required in older versions
- **Performance:** Highly optimized
- **Related function:** TYPE() returns 4 for logical values

### Google Sheets

- **Availability:** All versions since launch
- **Checkbox integration:** Returns TRUE for valid checkbox TRUE/FALSE
- **Array behavior:** Requires ARRAYFORMULA wrapper for array operations
- **Performance:** Efficient for single cells and arrays
- **Query integration:** ISLOGICAL can be used in QUERY WHERE clauses

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Behavior | Identical | Identical |
| TRUE recognition | TRUE only | TRUE only |
| FALSE recognition | FALSE only | FALSE only |
| Text "TRUE"/"FALSE" | FALSE | FALSE |
| Numbers 1/0 | FALSE | FALSE |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |

## Tips and Best Practices

1. **Use for checkbox validation:** ISLOGICAL verifies that checkbox cells contain proper logical values rather than text or numbers that might have been pasted in.

2. **Remember only TRUE and FALSE qualify:** ISLOGICAL is strict—only logical TRUE and logical FALSE return TRUE. Numbers (1, 0), text ("TRUE", "FALSE"), and everything else return FALSE.

3. **Distinguish from coercion:** Logical TRUE equals 1 in arithmetic, but ISLOGICAL(1) returns FALSE. The data TYPE matters, not mathematical equivalence.

4. **Combine with comparison for flexibility:** If you want to accept 1/0 as Boolean-like values, use =OR(ISLOGICAL(A1), AND(ISNUMBER(A1), OR(A1=0, A1=1))) instead of just ISLOGICAL.

5. **Use TYPE()==4 for equivalent check:** TYPE() returning 4 indicates a logical value. =TYPE(A1)=4 is equivalent to ISLOGICAL(A1).

6. **Count logical values carefully:** =COUNTIF(range, TRUE) counts only logical TRUE, not text "TRUE" or number 1. Use ISLOGICAL to understand why counts might seem wrong.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISNUMBER]] | Tests if value is numeric | When checking for numeric data type |
| [[ISTEXT]] | Tests if value is text | When checking for text data type |
| [[TYPE]] | Returns number indicating data type | When you need to know the specific type (4=logical) |
| [[NOT]] | Logical negation | When you need to invert a logical value |
| [[AND]] / [[OR]] | Logical operations | When combining logical tests |

### Commonly Used Together

**[[IF]]** - Conditional logic based on logical status

*Handle logical values specifically:*
```
=IF(ISLOGICAL(A1), IF(A1, "Yes", "No"), "Not a checkbox")
```
Ensures only logical TRUE/FALSE are processed as Boolean values.

---

**[[SUMPRODUCT]]** - Count logical values

*Count TRUE/FALSE cells:*
```
=SUMPRODUCT(ISLOGICAL(A1:A100)*1)
```
Counts cells containing actual logical values.

---

**[[TYPE]]** - Detailed type checking

*Verify logical type:*
```
=TYPE(A1)=4  -- Equivalent to ISLOGICAL(A1)
```
TYPE returns 4 for logical values.

---

**[[AND]]** / **[[OR]]** - Results to verify

*Check logical function output:*
```
=ISLOGICAL(AND(A1>0, B1>0))  -- Returns TRUE
```
Confirms logical functions return logical values.

---

**[[NOT]]** - Invert logical values

*Invert after confirming type:*
```
=IF(ISLOGICAL(A1), NOT(A1), "Invalid")
```
Safely inverts only actual logical values.

## Official Documentation

- **Microsoft Excel:** [ISLOGICAL function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISLOGICAL function](https://support.google.com/docs/answer/3093307)

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
