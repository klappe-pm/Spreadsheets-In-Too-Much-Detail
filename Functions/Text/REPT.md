---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- text-generation
- string-repetition
- visual-formatting
- data-padding
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Repeat Text
- Text Repetition
- String Multiply
- Duplicate Text
tags:
- text
- repetition
- generation
- formatting
- padding
- visualization
---

# REPT

## Description

**REPT** repeats a text string a specified number of times. This seemingly simple function has surprisingly diverse applications: creating visual elements like progress bars and dividers, padding strings to fixed lengths, generating test data, building repeated patterns, and creating text-based data visualizations. When you need to produce the same text multiple times, REPT is more elegant and flexible than copying and pasting.

The function takes two parameters: the text to repeat and the number of repetitions. The result is a single string containing the text concatenated the specified number of times. REPT handles edge cases gracefully: repeating 0 times returns an empty string, and fractional repetition counts are truncated to integers. Maximum result length is typically 32,767 characters (Excel's cell limit).

**Important gotcha:** REPT can create very long strings quickly. Repeating a 10-character string 10,000 times produces 100,000 characters, which exceeds the 32,767 character cell limit and returns a #VALUE! error. Also, REPT returns text even when repeating numbers, which affects downstream calculations. Zero or negative repetition counts return an empty string, not an error.

**Platform consideration:** Excel and Google Sheets implement REPT identically. Both truncate fractional repetition counts, both return empty strings for zero/negative counts, and both have similar character limits. Formulas are fully portable between platforms.

## Syntax

> [!f(x)] REPT Syntax
>
> ```
> =REPT(text, number_times)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string to repeat. Can include any characters, including special characters and numbers. |
| `number_times` | Yes | The number of times to repeat the text. Truncated to integer. 0 or negative returns empty string. |

### Return Value

Returns a text string consisting of the original text repeated the specified number of times. Returns empty string "" if number_times is 0 or negative. Returns #VALUE! if result exceeds maximum cell length.

## Examples

> [!f(x)] REPT Examples

### Example 1: Basic Repetition
```
=REPT("Hello", 3)
```
**Result:** "HelloHelloHello"

**Explanation:** The text "Hello" is repeated 3 times, concatenated into a single string.

---

### Example 2: Single Character Repetition
```
=REPT("*", 10)
```
**Result:** "**********"

**Explanation:** Creates a string of 10 asterisks. Common for creating dividers or visual elements.

---

### Example 3: Zero Repetitions
```
=REPT("Text", 0)
```
**Result:** "" (empty string)

**Explanation:** Zero repetitions returns an empty string, not an error. Useful in conditional scenarios.

---

### Example 4: Fractional Count (Truncated)
```
=REPT("X", 3.7)
```
**Result:** "XXX"

**Explanation:** The count 3.7 is truncated to 3 (not rounded). Only 3 repetitions occur.

---

### Example 5: Creating a Divider Line
```
=REPT("-", 50)
```
**Result:** "--------------------------------------------------"

**Explanation:** Creates a 50-character divider line. Useful for separating sections in reports.

---

### Example 6: Cell Reference for Count
```
=REPT("=", A1)
```
Where A1 contains 5

**Result:** "====="

**Explanation:** The repetition count can be a cell reference, enabling dynamic repetition based on data.

---

### Example 7: Simple Progress Bar
```
=REPT("|", A1/10) & REPT("-", (100-A1)/10)
```
Where A1 contains 40

**Result:** "||||------"

**Explanation:** Creates a 10-character progress bar where filled portion uses | and empty uses -. A1 is percentage.

---

### Example 8: Left Padding for Fixed Width
```
=REPT("0", 10-LEN(A1)) & A1
```
Where A1 contains "123"

**Result:** "0000000123"

**Explanation:** Pads a number with leading zeros to create a 10-character string. Classic zero-padding technique.

---

### Example 9: Right Padding for Fixed Width
```
=A1 & REPT(" ", 20-LEN(A1))
```
Where A1 contains "Name"

**Result:** "Name                " (16 trailing spaces)

**Explanation:** Pads text with trailing spaces to fixed width. Useful for fixed-width file generation.

---

### Example 10: Creating Visual Rating
```
=REPT("*", A1) & REPT("-", 5-A1)
```
Where A1 contains 3

**Result:** "***--"

**Explanation:** Visual star rating where A1 is the rating (1-5). Stars show rating, dashes show remaining.

---

### Example 11: Text-Based Bar Chart
```
=REPT("|", A1/100)
```
Where A1 contains 750

**Result:** "|||||||" (7.5 truncated to 7)

**Explanation:** Each | represents 100 units. Creates simple in-cell bar chart based on values.

---

### Example 12: Indentation Generator
```
=REPT("  ", A1) & B1
```
Where A1 contains 3 and B1 contains "Child Item"

**Result:** "      Child Item"

**Explanation:** Creates indentation (6 spaces for level 3). Useful for hierarchy display in text.

---

### Example 13: Pattern Generation
```
=REPT("AB", 5)
```
**Result:** "ABABABABAB"

**Explanation:** Repeats a multi-character pattern. Works with any text length.

---

### Example 14: Conditional Repetition
```
=IF(A1>0, REPT("#", MIN(A1, 20)), "")
```
Where A1 contains 15

**Result:** "###############"

**Explanation:** Only creates repetition for positive values, with a maximum of 20 characters. Prevents errors from excessive repetition.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Result exceeds 32,767 characters | Reduce repetition count or text length. Add MIN() to cap repetitions. |
| `#VALUE!` | number_times is text instead of number | Ensure repetition count is numeric. Use VALUE() to convert if needed. |
| `Unexpected empty result` | number_times is 0, negative, or <1 after truncation | Check that count is at least 1. Use MAX(count, 0) for graceful handling. |
| `Result is text, not number` | REPT always returns text | Use VALUE() if you need numeric result: VALUE(REPT("1", 3)) = 111. |
| `Formulas not calculating` | Long REPT results slow down spreadsheet | Minimize use in large datasets; consider alternative approaches. |

## Use Cases

### [[In-Cell Data Visualization]]

**Scenario:** Sales data needs quick visual comparison without creating separate charts. Each row should show a bar representing its value.

**Implementation:**
```
Bar: =REPT("|", A2/100)
Scaled Bar: =REPT("=", MIN(A2/MAX(A:A)*20, 20))
Comparison: =REPT("+", A2/100) & REPT("-", B2/100)
```

**Business Application:** Quick visual analysis in reports, dashboards without charts, and data exploration. Enables immediate visual comparison within data tables.

**Technical Details:** Scale values to appropriate bar length (typically 10-50 characters). Use MIN() to cap maximum length. Different characters can represent different categories or comparisons.

---

### [[Fixed-Width Data Export]]

**Scenario:** Legacy systems require fixed-width text files. Each field must be padded to a specific character width for proper alignment.

**Implementation:**
```
Left-Aligned (20 chars): =LEFT(A2 & REPT(" ", 20), 20)
Right-Aligned (10 chars): =RIGHT(REPT(" ", 10) & A2, 10)
Zero-Padded (8 digits): =RIGHT(REPT("0", 8) & A2, 8)
```

**Business Application:** EDI file generation, mainframe data feeds, and legacy system integration. Fixed-width formatting is essential for older data exchange standards.

**Technical Details:** Combine REPT with LEFT or RIGHT to ensure exact field width. For numeric fields, zero-pad for proper sorting and alignment. Calculate padding as (field_width - LEN(value)).

---

### [[Rating and Progress Display]]

**Scenario:** Customer satisfaction scores or task completion rates need visual representation directly in spreadsheet cells without charts.

**Implementation:**
```
Star Rating (1-5): =REPT("*", A2) & REPT(".", 5-A2)
Progress Bar: =REPT("[=]", INT(A2/10)) & REPT("[ ]", 10-INT(A2/10))
Percentage Bar: ="[" & REPT("#", A2/5) & REPT("-", 20-A2/5) & "]" & A2 & "%"
```

**Business Application:** Survey results display, project status tracking, and KPI dashboards. Visual indicators make data immediately interpretable without reading numbers.

**Technical Details:** Design consistent character sets for filled/empty states. Scale to appropriate maximum (5 stars, 10 segments, 20 character width). Include numeric value alongside visual for precision.

---

### [[Text Generation and Testing]]

**Scenario:** Development requires test data of specific sizes, or reports need placeholder text of known lengths for layout testing.

**Implementation:**
```
100-char test string: =REPT("X", 100)
1000-char lorem placeholder: =REPT("Lorem ipsum dolor sit amet. ", 34)
Sample data pattern: =REPT("ABC123", 10)
```

**Business Application:** Application testing, report template development, and data volume testing. Generate consistent test data without manual entry.

**Technical Details:** REPT quickly generates large strings for testing. Remember 32,767 character limit. Combine patterns for realistic test data structure.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 onward)
- **Maximum result:** 32,767 characters (cell limit)
- **Truncation:** Fractional counts truncated to integer
- **Zero/negative:** Returns empty string, not error
- **Performance:** Very fast for reasonable repetition counts

### Google Sheets
- **Availability:** All versions since launch
- **Behavior:** Identical to Excel
- **Maximum result:** Similar cell limits apply
- **Array support:** Works with ARRAYFORMULA for range processing
- **Performance:** Comparable to Excel

### Key Difference Alert
Both platforms implement REPT identically. No functional differences exist for standard use cases. The only consideration is cell character limits, which are similar across platforms. Formulas are fully portable.

## Tips and Best Practices

1. **Cap repetitions to prevent errors:** `=REPT("X", MIN(A1, 100))` ensures result never exceeds 100 characters regardless of input. Prevents #VALUE! from excessive repetition.

2. **Create progress bars with scaling:** Scale values to bar length: `=REPT("|", value/MAX*20)` creates bars that fit within 20 characters regardless of data range.

3. **Combine with LEN for padding:** `=REPT("0", 10-LEN(A1)) & A1` creates left-padded strings. Calculate padding based on desired width minus current length.

4. **Use for visual ratings:** `=REPT("*", rating)` creates intuitive star displays. Add empty indicators for context: `=REPT("*", A1) & REPT(".", 5-A1)`.

5. **Remember result is always text:** Even `=REPT("1", 3)` returns "111" as text, not number 111. Use VALUE() if numeric result needed.

6. **Consider performance:** Many REPT formulas with large repetition counts can slow spreadsheets. Use sparingly in large datasets.

7. **Build dividers and separators:** `=REPT("-", 40)` creates consistent visual separators. Adjust count to fit your report width.

8. **Create fixed-width exports:** Combine `=LEFT(A1 & REPT(" ", width), width)` for exact-width text fields required by legacy systems.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CONCATENATE]] | Joins multiple texts | When combining different texts, not repeating same text |
| [[TEXT]] | Formats numbers as text | When you need formatted number patterns |

### Commonly Used Together

**[[LEN]]** - String length

*Calculate padding needed:*
```
=REPT("0", 10-LEN(A1)) & A1
```
LEN determines how much padding is needed to reach target width.

---

**[[LEFT]]** / **[[RIGHT]]** - Fixed-width extraction

*Ensure exact output width:*
```
=LEFT(A1 & REPT(" ", 20), 20)
```
REPT creates padding, LEFT/RIGHT enforces exact width.

---

**[[MIN]]** / **[[MAX]]** - Value capping

*Prevent excessive repetition:*
```
=REPT("|", MIN(A1, 50))
```
MIN caps repetition count to prevent errors or excessive length.

---

**[[IF]]** - Conditional repetition

*Repeat only when appropriate:*
```
=IF(A1>0, REPT("*", A1), "N/A")
```
IF controls whether repetition occurs based on conditions.

---

**[[INT]]** / **[[ROUND]]** - Number adjustment

*Create integer repetition counts:*
```
=REPT("|", INT(A1/100))
```
INT converts values to whole number repetition counts.

---

**[[SUBSTITUTE]]** - Character replacement

*Modify repeated patterns:*
```
=SUBSTITUTE(REPT("X-", 5), "-", "")
```
Clean up patterns created with REPT.

---

**[[CHAR]]** - Special characters

*Create repeated special characters:*
```
=REPT(CHAR(9), 3)
```
Repeat tab characters, line breaks, or other special characters by code.

## Official Documentation

- **Microsoft Excel:** [REPT function](https://support.microsoft.com/en-us/office/rept-function-04c4d778-e712-43b4-9c15-d656582bb061)
- **Google Sheets:** [REPT function](https://support.google.com/docs/answer/3094139)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | Identical implementation to Excel |

---

*Last updated: 2026-01-10*
