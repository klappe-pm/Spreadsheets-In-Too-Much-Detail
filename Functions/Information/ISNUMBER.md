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
  - number-validation
  - data-type-detection
  - numeric-test
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-number
  - number-check
  - numeric-test
  - number-validation
tags:
  - information
  - data-validation
  - type-checking
  - numeric-data
  - data-quality
  - excel
  - google-sheets
---

# ISNUMBER

## Description

The **ISNUMBER function** tests whether a value is a numeric value and returns TRUE if the value is a number, or FALSE if it is any other data type including text, logical values, errors, or blank cells. This function is essential for data validation, ensuring that cells expected to contain numbers actually do before performing calculations on them. ISNUMBER recognizes all numeric types including integers, decimals, negative numbers, dates (which are stored as numbers), and times. However, it returns FALSE for numbers stored as text (like "123"), even though they may look like numbers visually. This distinction makes ISNUMBER critical for identifying data quality issues where numeric values have been inadvertently converted to text.

The primary use cases for ISNUMBER include input validation in data entry forms, pre-calculation checks to avoid errors, and data cleaning workflows where numeric columns might contain text values. A common scenario is validating user input: =IF(ISNUMBER(A1), A1*2, "Please enter a number") ensures calculations only proceed with valid numeric data. ISNUMBER is also frequently used with SEARCH or FIND functions to test if a substring was found: since SEARCH returns a number when successful and #VALUE! when not found, =ISNUMBER(SEARCH("text", A1)) returns TRUE if "text" exists in A1. This pattern is extremely popular for text searching without error handling.

Understanding what ISNUMBER considers a "number" is crucial for correct usage. TRUE cases include: integers (5, -10), decimals (3.14, -0.5), dates (dates are stored as serial numbers), times (stored as decimal portions of days), currency-formatted numbers, and percentage-formatted numbers. FALSE cases include: text that looks like numbers ("123", "5.0"), logical values TRUE/FALSE (even though they can be coerced to 1/0 in calculations), error values, blank cells, and formulas that return empty strings. A particularly common gotcha is numbers imported from external sources that appear numeric but are actually text—ISNUMBER correctly identifies these as FALSE, which can be surprising but is accurate.

Platform behavior for ISNUMBER is identical between Excel and Google Sheets. Both treat the same value types as numbers and both return FALSE for the same edge cases like text-formatted numbers. The function has been available since early versions of both platforms. In array contexts, ISNUMBER works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365. One subtle point: both platforms treat dates and times as numbers (since they're stored as serial numbers), so ISNUMBER returns TRUE for date/time values. If you specifically need to test for date formatting, you'll need additional checks.

## Syntax

> [!f(x)] ISNUMBER Syntax
>
> ```
> =ISNUMBER(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value, cell reference, or formula result to test. Returns TRUE if the value is numeric (integer, decimal, date, time). Returns FALSE for text (including text-formatted numbers), logical values (TRUE/FALSE), error values, blank cells, and empty strings. |

### Return Value

Returns **TRUE** if the value is a numeric data type. Returns **FALSE** for all non-numeric data types. ISNUMBER never produces an error—it always returns TRUE or FALSE.

## Examples

> [!f(x)] ISNUMBER Examples

### Example 1: Testing an Integer
```
=ISNUMBER(42)
```
**Result:** TRUE

**Explanation:** Integers are numbers. Any whole number, positive or negative, returns TRUE.

---

### Example 2: Testing a Decimal
```
=ISNUMBER(3.14159)
```
**Result:** TRUE

**Explanation:** Decimal numbers are numbers. ISNUMBER recognizes all numeric formats.

---

### Example 3: Testing Text
```
=ISNUMBER("Hello")
```
**Result:** FALSE

**Explanation:** Text strings are not numbers. This is straightforward—letters and words are clearly not numeric.

---

### Example 4: Testing Text That Looks Like a Number
```
=ISNUMBER("123")
```
**Result:** FALSE

**Explanation:** This is a critical distinction. The value "123" (with quotes) is text, not a number. ISNUMBER correctly identifies this as FALSE. This is common with imported data where numbers become text.

---

### Example 5: Testing a Date
```
=ISNUMBER(DATE(2024, 1, 15))
```
**Result:** TRUE

**Explanation:** Dates in Excel/Sheets are stored as serial numbers (days since a reference date). ISNUMBER returns TRUE for dates because internally they ARE numbers.

---

### Example 6: Testing a Logical Value
```
=ISNUMBER(TRUE)
=ISNUMBER(FALSE)
```
**Result:** FALSE (both)

**Explanation:** Logical values are a distinct data type from numbers. Even though TRUE can be coerced to 1 in calculations (TRUE*5=5), ISNUMBER returns FALSE because the data type is logical, not numeric.

---

### Example 7: Testing a Blank Cell
```
=ISNUMBER(A1)
```
**Scenario:** A1 is completely empty
**Result:** FALSE

**Explanation:** Blank cells are not numbers. Use ISBLANK to test for empty cells specifically.

---

### Example 8: Using with SEARCH for Text Detection
```
=ISNUMBER(SEARCH("apple", A1))
```
**Scenario:** Test if "apple" exists within the text in A1
**Result:** TRUE if "apple" is found, FALSE if not found

**Explanation:** SEARCH returns a number (the position) when found, or #VALUE! when not found. ISNUMBER elegantly converts this to TRUE/FALSE without error handling. This is one of the most common ISNUMBER patterns.

---

### Example 9: Validating Numeric Input with IF
```
=IF(ISNUMBER(A1), A1 * 2, "Invalid: Enter a number")
```
**Scenario:** Double the value if it's a number, otherwise show error message
**Result:** Doubled value or validation message

**Explanation:** This pattern prevents #VALUE! errors from non-numeric input by checking first. Essential for spreadsheets that accept user input.

---

### Example 10: Counting Numeric Values in a Range
```
=SUMPRODUCT(ISNUMBER(A1:A100)*1)
```
**Scenario:** Count cells containing actual numbers (not text-formatted numbers)
**Result:** Count of numeric cells

**Explanation:** This counts cells with true numeric values. Unlike COUNT (which counts anything that looks numeric), this excludes text-formatted numbers, providing accurate data quality metrics.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `FALSE for visual numbers` | Numbers stored as text ("123") | Convert to numbers: multiply by 1, use VALUE(), or use Text to Columns |
| `TRUE for dates when checking for integers` | Dates are stored as numbers | Add date check: =AND(ISNUMBER(A1), NOT(ISNUMBER(DAY(A1)))) for non-date numbers |
| `FALSE for TRUE/FALSE` | Logical values are not numeric type | This is correct; use ISLOGICAL to test for TRUE/FALSE |
| `FALSE for blank cells` | Blank is not a numeric value | This is correct; use ISBLANK to test for empty cells |
| `Different results than COUNT` | COUNT counts text-numbers; ISNUMBER doesn't | Use ISNUMBER for strict type checking; COUNT for "anything countable" |
| `FALSE for cell showing number` | Cell might contain formula returning text | Check =TYPE(cell) to see actual data type |
| `Inconsistent results in column` | Mixed data types from different sources | Standardize data types using VALUE() for numbers or TEXT() for text |

## Use Cases

### [[Data Entry Validation System]]

**Scenario:** A financial data entry form requires users to enter various numeric values: quantities, prices, percentages, and amounts. Invalid entries (text, blanks, or errors) should be flagged immediately rather than causing calculation errors downstream. The form needs real-time validation feedback.

**Implementation:**
```
Cell Validation Status:
=IF(ISNUMBER(B2), "Valid", "ERROR: Must be numeric")

Comprehensive Field Validation:
=IF(ISBLANK(B2), "Required field",
 IF(ISNUMBER(B2),
    IF(B2>=0, "Valid", "ERROR: Must be positive"),
    "ERROR: Enter numbers only"))

Form Completion Check:
=IF(AND(ISNUMBER(Price), ISNUMBER(Quantity), ISNUMBER(Discount)),
    "Ready to submit",
    "Fix validation errors first")

Invalid Entry Count:
=ROWS(B2:B100)-SUMPRODUCT(ISNUMBER(B2:B100)*1)

Data Quality Dashboard:
Numeric Cells: =SUMPRODUCT(ISNUMBER(AllInputs)*1)
Text Cells: =SUMPRODUCT(ISTEXT(AllInputs)*1)
Blank Cells: =COUNTBLANK(AllInputs)
Error Cells: =SUMPRODUCT(ISERROR(AllInputs)*1)
```

**Business Application:** Financial data entry errors cascade into reports, budgets, and forecasts. Real-time validation prevents bad data from entering the system. The comprehensive validation provides specific feedback: "Required field" versus "Enter numbers only" helps users correct issues quickly. The completion check prevents submission until all required numeric fields are valid.

**Technical Details:** Layer ISNUMBER with ISBLANK for required fields—a blank field might be "not yet entered" rather than "invalid." Add range validation (>=0, <=100 for percentages) after confirming the value is numeric. Use conditional formatting with ISNUMBER to highlight invalid cells visually. Consider using Data Validation with custom formulas based on ISNUMBER for in-cell validation.

---

### [[Text Search Without Error Handling]]

**Scenario:** A product catalog needs to categorize items based on keywords in their descriptions. The categorization formula must test for multiple keywords without generating #VALUE! errors when keywords aren't found. Traditional SEARCH/FIND functions error when text isn't found, requiring complex IFERROR wrapping.

**Implementation:**
```
Keyword Detection:
Contains "Premium": =ISNUMBER(SEARCH("premium", Description))
Contains "Sale": =ISNUMBER(SEARCH("sale", Description))
Contains "New": =ISNUMBER(SEARCH("new", Description))

Category Assignment:
=IF(ISNUMBER(SEARCH("premium", A1)), "Premium",
 IF(ISNUMBER(SEARCH("sale", A1)), "Clearance",
 IF(ISNUMBER(SEARCH("new", A1)), "New Arrival",
    "Standard")))

Multi-Keyword Match Count:
=SUMPRODUCT(
    ISNUMBER(SEARCH("premium", A1))*1,
    ISNUMBER(SEARCH("exclusive", A1))*1,
    ISNUMBER(SEARCH("limited", A1))*1)

Filter Matching Products (Excel 365):
=FILTER(Products, ISNUMBER(SEARCH("premium", Products[Description])))
```

**Business Application:** Product categorization drives website displays, pricing rules, and inventory management. Using ISNUMBER(SEARCH(...)) provides clean TRUE/FALSE results without error handling complexity. The multi-keyword counter helps identify "premium premium exclusive" products (multiple luxury indicators). The FILTER formula creates dynamic lists of matching products for reporting.

**Technical Details:** SEARCH is case-insensitive; use FIND for case-sensitive matching. The ISNUMBER wrapper converts SEARCH's position number (when found) to TRUE and its #VALUE! error (when not found) to FALSE—no IFERROR needed. For multiple keyword searches, each ISNUMBER(SEARCH()) returns TRUE/FALSE, which can be combined with AND/OR for complex logic.

---

### [[Import Data Quality Assessment]]

**Scenario:** A data team receives CSV exports from various vendor systems. Columns that should contain numbers often have text-formatted numbers, empty strings, or error values due to export issues. Before processing, the team needs to assess and report on data quality by column.

**Implementation:**
```
Column Data Type Analysis:
Numeric Count: =SUMPRODUCT(ISNUMBER(A:A)*1)
Text Count: =SUMPRODUCT(ISTEXT(A:A)*1)
Blank Count: =COUNTBLANK(A:A)
Error Count: =SUMPRODUCT(ISERROR(A:A)*1)
Total Records: =COUNTA(A:A) + COUNTBLANK(A:A)

Expected Numeric Column Validation:
Clean Rate: =TEXT(SUMPRODUCT(ISNUMBER(NumericColumn)*1)/COUNTA(NumericColumn), "0.0%")

Problem Record Identification:
=IF(ISNUMBER(A2), "OK",
 IF(ISTEXT(A2), "TEXT: " & A2,
 IF(ISBLANK(A2), "BLANK",
 IF(ISERROR(A2), "ERROR",
    "UNKNOWN"))))

Text-to-Number Conversion Assessment:
Can Convert: =ISNUMBER(VALUE(A2))
Conversion Formula: =IF(ISNUMBER(A2), A2, IF(ISNUMBER(VALUE(A2)), VALUE(A2), "Cannot convert"))

Quality Report Summary:
="Column " & ColumnName & ": " &
 SUMPRODUCT(ISNUMBER(Column)*1) & " numeric, " &
 SUMPRODUCT(ISTEXT(Column)*1) & " text, " &
 COUNTBLANK(Column) & " blank"
```

**Business Application:** Data import quality directly impacts reporting accuracy. This assessment identifies systematic issues: if a "Price" column has 30% text values, there's an export configuration problem with the vendor. The problem record identification helps data stewards find and fix specific issues. The conversion assessment shows which text values CAN be converted to numbers (like "123.45") versus truly invalid data (like "N/A").

**Technical Details:** ISNUMBER distinguishes true numeric values from text-formatted numbers—important because text-numbers can cause subtle calculation errors (SUM ignores them, for example). Use VALUE() to attempt conversion, wrapped in ISNUMBER(VALUE()) to test if conversion is possible. For large datasets, create a separate quality assessment sheet rather than adding columns to the data itself.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 and later)
- **Date handling:** Returns TRUE for dates (they're serial numbers)
- **Array behavior:** Native dynamic arrays in Excel 365; CSE required in older versions
- **Performance:** Highly optimized
- **Related function:** TYPE() returns 1 for numbers

### Google Sheets

- **Availability:** All versions since launch
- **Date handling:** Returns TRUE for dates (stored as serial numbers)
- **Array behavior:** Requires ARRAYFORMULA wrapper for array operations
- **Performance:** Efficient for single cells and arrays
- **Query integration:** ISNUMBER can be used in QUERY WHERE clauses

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Behavior | Identical | Identical |
| Date recognition | TRUE | TRUE |
| Text number ("123") | FALSE | FALSE |
| TRUE/FALSE values | FALSE | FALSE |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |
| Performance | Excellent | Excellent |

## Tips and Best Practices

1. **Remember text-numbers return FALSE:** Numbers stored as text (common from CSV imports) return FALSE. If a column should be numeric but has many FALSE results, convert the data using VALUE(), multiplying by 1, or Text to Columns.

2. **Use ISNUMBER(SEARCH()) for text detection:** This pattern cleanly tests if a substring exists without error handling: =ISNUMBER(SEARCH("text", cell)) returns TRUE if found, FALSE if not. Much cleaner than IFERROR(SEARCH(...), FALSE).

3. **Dates are numbers:** ISNUMBER returns TRUE for dates because they're stored as serial numbers. If you need to distinguish dates from "regular" numbers, combine with additional tests or use TYPE() function.

4. **Combine with IF for validation:** =IF(ISNUMBER(A1), calculation, "Enter a number") prevents #VALUE! errors from non-numeric input and provides user-friendly feedback.

5. **Use for data quality metrics:** =SUMPRODUCT(ISNUMBER(range)*1)/COUNTA(range) gives the percentage of cells that are truly numeric—valuable for assessing imported data quality.

6. **Don't confuse with COUNT:** COUNT includes text-formatted numbers in its count if they can be interpreted as numbers. ISNUMBER is stricter and only returns TRUE for actual numeric data types.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISTEXT]] | Tests if value is text | When you need to check for text data type |
| [[ISLOGICAL]] | Tests if value is TRUE/FALSE | When checking for boolean values |
| [[ISBLANK]] | Tests if cell is empty | When checking for blank cells |
| [[TYPE]] | Returns number indicating data type | When you need to know the specific type (1=number, 2=text, etc.) |
| [[COUNT]] | Counts numeric values | When counting (includes text-numbers that look numeric) |

### Commonly Used Together

**[[IF]]** - Conditional logic based on numeric status

*Validate before calculating:*
```
=IF(ISNUMBER(A1), A1 * TaxRate, "Invalid input")
```
Prevents calculation errors from non-numeric values.

---

**[[SEARCH]]** - Text substring detection

*Elegant substring check:*
```
=ISNUMBER(SEARCH("keyword", A1))
```
Returns TRUE if "keyword" found, FALSE if not—no error handling needed.

---

**[[SUMPRODUCT]]** - Count numeric cells

*Count actual numbers in range:*
```
=SUMPRODUCT(ISNUMBER(A1:A100)*1)
```
More precise than COUNT for identifying true numeric values.

---

**[[VALUE]]** - Convert text to number

*Test if text can become number:*
```
=ISNUMBER(VALUE(A1))
```
Tests whether a text value can be converted to a number.

---

**[[FILTER]]** - Extract numeric values

*Get only numeric values from mixed data:*
```
=FILTER(A1:A100, ISNUMBER(A1:A100))
```
Creates array of only the numeric values from a mixed-type column.

## Official Documentation

- **Microsoft Excel:** [ISNUMBER function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISNUMBER function](https://support.google.com/docs/answer/3093296)

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
