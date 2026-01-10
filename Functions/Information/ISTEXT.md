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
  - text-validation
  - data-type-detection
  - string-test
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-text
  - text-check
  - string-test
  - text-validation
tags:
  - information
  - data-validation
  - type-checking
  - text-data
  - data-quality
  - excel
  - google-sheets
---

# ISTEXT

## Description

The **ISTEXT function** tests whether a value is a text string and returns TRUE if the value is text, or FALSE if it is any other data type including numbers, logical values, errors, or blank cells. This function is essential for data validation and type checking in spreadsheets where you need to confirm that values are text before applying text functions like LEFT, RIGHT, MID, or CONCATENATE. ISTEXT recognizes any string value, including empty strings (""), single characters, and multi-line text. Importantly, it returns FALSE for numbers that display with text formatting—the underlying data type matters, not the display format.

The primary use cases for ISTEXT include validating text input fields, identifying data type issues in imported data, and conditional logic that branches based on whether a value is text. A common pattern is using IF with ISTEXT: =IF(ISTEXT(A1), UPPER(A1), "Enter text") to ensure text functions only apply to actual text values. ISTEXT is also valuable for data cleaning, helping identify columns where numeric data has been inadvertently stored as text (which can cause subtle problems with SUM, sorting, and other operations). When you expect a column to contain numbers but ISTEXT returns TRUE, it's a signal that data conversion is needed.

Understanding what ISTEXT considers "text" is important for correct usage. TRUE cases include: any string including letters ("Hello"), numbers stored as text ("123"), symbols ("@#$"), empty strings (""), and spaces (" "). FALSE cases include: actual numeric values (even if formatted to look like text), logical values TRUE/FALSE, error values, dates/times (stored as numbers), and completely blank cells (no content at all). A subtle distinction: a cell containing an empty string ("") returns TRUE for ISTEXT but FALSE for ISBLANK, because "" is a text value while a truly blank cell has no value at all. This distinction matters for data validation.

Platform behavior for ISTEXT is identical between Excel and Google Sheets. Both platforms use the same criteria for determining text type, and both handle edge cases like empty strings and text-formatted numbers consistently. The function has been available since early versions of both platforms. In array contexts, ISTEXT works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365. The function pairs naturally with ISNUMBER and ISNONTEXT for comprehensive type checking.

## Syntax

> [!f(x)] ISTEXT Syntax
>
> ```
> =ISTEXT(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value, cell reference, or formula result to test. Returns TRUE if the value is a text string (including empty string ""). Returns FALSE for numbers, logical values (TRUE/FALSE), error values, dates/times, and blank cells. |

### Return Value

Returns **TRUE** if the value is a text string data type. Returns **FALSE** for all non-text data types. ISTEXT never produces an error—it always returns TRUE or FALSE.

## Examples

> [!f(x)] ISTEXT Examples

### Example 1: Testing a Text String
```
=ISTEXT("Hello World")
```
**Result:** TRUE

**Explanation:** String literals and text values return TRUE. This is the basic case ISTEXT is designed to detect.

---

### Example 2: Testing a Number
```
=ISTEXT(42)
```
**Result:** FALSE

**Explanation:** Numeric values are not text. Even if formatted to display with text characteristics, numbers are a different data type.

---

### Example 3: Testing a Number Stored as Text
```
=ISTEXT("123")
```
**Result:** TRUE

**Explanation:** This is critical—"123" (with quotes) is text, not a number. ISTEXT correctly identifies this as TRUE. This is common in imported data and can cause calculation issues.

---

### Example 4: Testing an Empty String
```
=ISTEXT("")
```
**Result:** TRUE

**Explanation:** An empty string is still a text value—it's a string with zero characters. This differs from a blank cell, which has no value at all.

---

### Example 5: Testing a Blank Cell
```
=ISTEXT(A1)
```
**Scenario:** A1 is completely empty (never had content)
**Result:** FALSE

**Explanation:** Blank cells are not text—they're empty. Use ISBLANK to specifically test for blank cells. A cell must contain something (even "") to be considered text.

---

### Example 6: Testing a Logical Value
```
=ISTEXT(TRUE)
=ISTEXT(FALSE)
```
**Result:** FALSE (both)

**Explanation:** Logical values are a distinct data type. Even though they display as text, TRUE and FALSE are not text strings.

---

### Example 7: Testing a Date
```
=ISTEXT(DATE(2024, 1, 15))
```
**Result:** FALSE

**Explanation:** Dates are stored as numbers internally. Even though they display as text like "1/15/2024", the underlying data type is numeric.

---

### Example 8: Validating Text Input with IF
```
=IF(ISTEXT(A1), LEN(A1) & " characters", "Not text - enter name")
```
**Scenario:** Check if a name field contains text before processing
**Result:** Character count if text, validation message if not

**Explanation:** This pattern ensures text functions only run on actual text values, preventing #VALUE! errors from numeric input.

---

### Example 9: Counting Text Values in a Range
```
=SUMPRODUCT(ISTEXT(A1:A100)*1)
```
**Scenario:** Count how many cells contain text in a data column
**Result:** Number of text cells

**Explanation:** ISTEXT evaluates each cell. Multiplying by 1 converts TRUE/FALSE to 1/0. SUMPRODUCT sums them. Useful for data quality assessment.

---

### Example 10: Finding Text in a Numeric Column (Data Quality)
```
=IF(ISTEXT(A1), "WARNING: Text in numeric column", A1)
```
**Scenario:** Flag cells where numbers should be but text exists
**Result:** Warning message if text, value if number

**Explanation:** In columns that should be numeric (prices, quantities), any ISTEXT=TRUE result indicates a data quality issue requiring attention.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `TRUE for "123" when expecting FALSE` | Numbers stored as text are text | This is correct; convert to numbers with VALUE() or *1 |
| `FALSE for blank cell when expecting TRUE` | Blank cells are not text (they're empty) | This is correct; empty string "" is text, blank is not |
| `FALSE for dates` | Dates are stored as numbers | This is correct; format is visual only, data type is numeric |
| `TRUE for formatted numbers` | Cell contains number formatted as text | Check if data was imported or entered with apostrophe prefix |
| `Inconsistent results across column` | Mixed data types from different sources | Standardize data types using VALUE() or TEXT() |
| `Different from LEN>0 check` | LEN() returns 0 for both blank and "" | ISTEXT distinguishes "" (TRUE) from blank (FALSE) |

## Use Cases

### [[Form Field Validation]]

**Scenario:** A contact information form requires text fields (names, addresses, comments) to contain actual text, not numbers or blanks. The form needs to validate that users entered meaningful text content before submission.

**Implementation:**
```
Name Field Validation:
=IF(ISBLANK(NameCell), "Required",
 IF(NOT(ISTEXT(NameCell)), "Must be text",
 IF(LEN(TRIM(NameCell))<2, "Name too short",
    "Valid")))

Address Field Validation:
=IF(ISBLANK(AddressCell), "Required",
 IF(NOT(ISTEXT(AddressCell)), "Invalid format",
 IF(LEN(TRIM(AddressCell))<5, "Address incomplete",
    "Valid")))

Comments Field (Optional Text):
=IF(ISBLANK(CommentsCell), "Optional - not provided",
 IF(ISTEXT(CommentsCell), "Provided: " & LEN(CommentsCell) & " characters",
    "Invalid format"))

Form Readiness:
=IF(AND(
    ISTEXT(NameCell), LEN(TRIM(NameCell))>=2,
    ISTEXT(AddressCell), LEN(TRIM(AddressCell))>=5),
    "Ready to submit",
    "Complete required fields")
```

**Business Application:** Contact forms must capture valid text data. Names that are just numbers (like someone entering their employee ID instead) need to be flagged. Addresses that are truly blank versus containing just spaces need different handling. The graduated validation (Required -> Must be text -> Too short -> Valid) guides users to correct specific issues.

**Technical Details:** Combine ISTEXT with LEN and TRIM for comprehensive validation. ISTEXT confirms the data type is text; LEN(TRIM()) checks for meaningful content after removing extra spaces. The distinction between ISBLANK (no value) and ISTEXT("") (empty string) matters—some imports create empty strings that aren't truly blank.

---

### [[Data Type Inconsistency Detection]]

**Scenario:** A data analyst receives a dataset that should contain numeric values in certain columns (prices, quantities, IDs). Due to export issues, some numeric values have been converted to text. The analyst needs to identify and quantify these issues before processing.

**Implementation:**
```
Column Type Analysis:
Text Values in Price Column: =SUMPRODUCT(ISTEXT(PriceColumn)*1)
Numeric Values: =SUMPRODUCT(ISNUMBER(PriceColumn)*1)
Blank Values: =COUNTBLANK(PriceColumn)
Total Cells: =COUNTA(PriceColumn)+COUNTBLANK(PriceColumn)

Data Type Issue Flagging:
=IF(ISNUMBER(A2), "OK",
 IF(ISTEXT(A2), "TEXT: '" & A2 & "'",
 IF(ISBLANK(A2), "BLANK",
    "OTHER")))

Convertibility Assessment:
=IF(AND(ISTEXT(A2), ISNUMBER(VALUE(A2))), "Convertible",
 IF(AND(ISTEXT(A2), ISERROR(VALUE(A2))), "Not convertible",
    "Already numeric"))

Data Quality Report:
="Column Analysis: " &
 SUMPRODUCT(ISNUMBER(Column)*1) & " numbers, " &
 SUMPRODUCT(AND(ISTEXT(Column), ISNUMBER(VALUE(Column)))*1) & " text-numbers, " &
 SUMPRODUCT(AND(ISTEXT(Column), ISERROR(VALUE(Column)))*1) & " true text, " &
 COUNTBLANK(Column) & " blanks"
```

**Business Application:** Data type issues cause silent failures—SUM ignores text cells, AVERAGE skips them, and sorting places "2" after "19" when stored as text. This analysis identifies the scope of the problem. The convertibility assessment shows how much data can be automatically fixed versus requiring manual review.

**Technical Details:** The convertibility check uses ISNUMBER(VALUE()) to test if text can become a number. "123" can; "N/A" cannot. Generate a cleanup plan based on the analysis: fully convertible data can be fixed with formulas; non-convertible text needs review. Track these metrics over time to identify systematic issues with source systems.

---

### [[Conditional Text Processing]]

**Scenario:** A product catalog contains mixed data—some cells have text descriptions, others have numeric codes, and some are blank. Processing logic needs to handle each type appropriately: format text descriptions, convert codes to descriptions via lookup, and mark blanks for follow-up.

**Implementation:**
```
Smart Cell Processing:
=IF(ISBLANK(A1), "PENDING",
 IF(ISTEXT(A1), PROPER(TRIM(A1)),
 IF(ISNUMBER(A1), "Code: " & A1,
    "Unknown")))

Text-Specific Formatting (only if text):
=IF(ISTEXT(ProductName),
    LEFT(PROPER(TRIM(ProductName)), 50),
    ProductName)

Category Assignment:
=IF(ISTEXT(A1),
    IF(LEN(A1)>20, "Long Description",
    IF(LEN(A1)>5, "Short Description",
        "Code/Abbreviation")),
    IF(ISNUMBER(A1), "Numeric Code",
        "Empty/Unknown"))

Batch Processing Counts:
Text Descriptions: =SUMPRODUCT(ISTEXT(AllProducts)*1)
Numeric Codes: =SUMPRODUCT(ISNUMBER(AllProducts)*1)
Needs Attention: =COUNTBLANK(AllProducts)
```

**Business Application:** Product data often comes from multiple sources with inconsistent formats. Some suppliers provide text descriptions; others provide numeric product codes. Processing must handle both appropriately. ISTEXT enables conditional logic that applies text functions only to text values, avoiding errors from numeric data.

**Technical Details:** The PROPER(TRIM()) combination standardizes text: TRIM removes extra spaces, PROPER capitalizes first letters. Apply only when ISTEXT is TRUE to avoid errors with numeric values. The category assignment helps route different data types to appropriate processing workflows.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 and later)
- **Empty string handling:** Returns TRUE for ""
- **Array behavior:** Native dynamic arrays in Excel 365; CSE required in older versions
- **Performance:** Highly optimized
- **Related function:** TYPE() returns 2 for text

### Google Sheets

- **Availability:** All versions since launch
- **Empty string handling:** Returns TRUE for ""
- **Array behavior:** Requires ARRAYFORMULA wrapper for array operations
- **Performance:** Efficient for single cells and arrays
- **Query integration:** ISTEXT can be used in QUERY WHERE clauses

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Behavior | Identical | Identical |
| Empty string ("") | TRUE | TRUE |
| Blank cell | FALSE | FALSE |
| Text-formatted number ("123") | TRUE | TRUE |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |
| Performance | Excellent | Excellent |

## Tips and Best Practices

1. **Distinguish ISTEXT from ISBLANK:** ISTEXT("") returns TRUE; ISTEXT(blank) returns FALSE. An empty string is a text value; a blank cell is empty. Use the appropriate test for your scenario.

2. **Identify text-numbers in numeric columns:** If ISTEXT returns TRUE for cells that should be numeric, the data needs conversion. Use VALUE() or multiply by 1 to convert text-numbers to actual numbers.

3. **Combine with text functions safely:** =IF(ISTEXT(A1), UPPER(A1), A1) ensures text functions only apply to text values. This prevents #VALUE! errors when data types are mixed.

4. **Use for data quality assessment:** =SUMPRODUCT(ISTEXT(range)*1)/COUNTA(range) gives the percentage of text values in a range—useful for validating that text columns contain text.

5. **Remember dates are not text:** Even though dates display as text like "1/15/2024", ISTEXT returns FALSE because dates are stored as numbers. Use TEXT() to convert dates to text if needed.

6. **Pair with ISNONTEXT for coverage:** ISTEXT and ISNONTEXT are complements. If ISTEXT returns FALSE, ISNONTEXT returns TRUE (for non-text values) or the cell is blank.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISNUMBER]] | Tests if value is numeric | When checking for numeric data type |
| [[ISNONTEXT]] | Tests if value is NOT text | When you need to identify non-text (numbers, blanks, errors) |
| [[ISBLANK]] | Tests if cell is empty | When checking for truly empty cells |
| [[TYPE]] | Returns number indicating data type | When you need to know the specific type (1=number, 2=text, etc.) |
| [[T]] | Returns text or empty string | When you need to extract text value or get "" for non-text |

### Commonly Used Together

**[[IF]]** - Conditional logic based on text status

*Apply text function only to text:*
```
=IF(ISTEXT(A1), UPPER(A1), "Not text")
```
Prevents errors when applying text functions to mixed data.

---

**[[LEN]]** - Measure text length after confirming it's text

*Character count for text only:*
```
=IF(ISTEXT(A1), LEN(A1), "N/A")
```
LEN works on text; combining with ISTEXT ensures valid input.

---

**[[SUMPRODUCT]]** - Count text cells

*Count text values in range:*
```
=SUMPRODUCT(ISTEXT(A1:A100)*1)
```
Counts cells containing text data type.

---

**[[TRIM]]** - Clean text after confirming type

*Safe text cleaning:*
```
=IF(ISTEXT(A1), TRIM(A1), A1)
```
Applies TRIM only to text values, passing non-text through unchanged.

---

**[[VALUE]]** - Convert text-numbers

*Fix text-formatted numbers:*
```
=IF(AND(ISTEXT(A1), ISNUMBER(VALUE(A1))), VALUE(A1), A1)
```
Converts text-numbers to numbers; leaves everything else unchanged.

## Official Documentation

- **Microsoft Excel:** [ISTEXT function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISTEXT function](https://support.google.com/docs/answer/3093297)

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
