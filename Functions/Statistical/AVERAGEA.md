---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- average
- mean
- text-handling
- logical-values
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Average A
- Average Including Text
- Average All
- Mean with Text
tags:
- statistical
- average
- mean
- text-values
- logical-values
---

# AVERAGEA

## Description

**AVERAGEA** calculates the arithmetic mean of values in a range, with the distinctive behavior of including text and logical values in the calculation. Unlike the standard AVERAGE function which ignores text and logical values, AVERAGEA treats text strings as 0 (zero) and logical TRUE as 1, FALSE as 0. This makes AVERAGEA essential when your data contains mixed types and you want every cell to contribute to both the sum and the count, rather than being silently excluded.

The mathematical formula is identical to AVERAGE (sum of values divided by count), but the key difference lies in how values are interpreted. When AVERAGEA encounters a cell containing "N/A" or any other text, it includes that cell as 0 in both the numerator (sum) and denominator (count). This means text-containing cells dilute the average rather than being ignored. Similarly, TRUE becomes 1 and FALSE becomes 0. This behavior is crucial for survey data, attendance tracking, and any scenario where empty responses or non-numeric entries should explicitly count as zero rather than being excluded.

**Critical distinction from AVERAGE:** Consider a range with values {10, 20, "Absent", 30}. AVERAGE returns 20 (sum of 60 divided by 3 numeric values). AVERAGEA returns 15 (sum of 60 divided by 4 values, since "Absent" counts as 0). This difference can be dramatic. If half your data is text, AVERAGE might show 100 while AVERAGEA shows 50. Neither is "wrong" - they answer different questions. AVERAGE asks "what's the average of the numbers?" while AVERAGEA asks "what's the average if missing/text entries count as zero?"

**Platform compatibility:** AVERAGEA behaves identically in Excel and Google Sheets, with one subtle exception in how direct arguments are handled versus cell references. Both platforms treat text in cells as 0, but text passed directly as an argument (not common practice) may generate an error. Empty cells are always ignored by both platforms - they do not count as 0. Only cells containing text strings are converted to 0. This distinction between empty and text-containing cells is often misunderstood.

## Syntax

> [!f(x)] AVERAGEA Syntax
>
> ```
> =AVERAGEA(value1, [value2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | The first value, cell reference, or range to average. Can include numbers, text, logical values, or empty cells. |
| `value2, ...` | No | Additional values, cell references, or ranges to include. Up to 255 arguments in Excel, virtually unlimited cells through ranges in Google Sheets. |

### Return Value

Returns a numeric value representing the arithmetic mean. Text values in referenced cells are counted as 0, TRUE as 1, FALSE as 0. Empty cells are ignored. Returns #DIV/0! if no values qualify (all cells empty or referenced range is empty).

## Examples

> [!f(x)] AVERAGEA Examples

### Example 1: Basic Numeric Average (Same as AVERAGE)
```
=AVERAGEA(10, 20, 30, 40, 50)
```
**Result:** 30

**Explanation:** When all values are numeric, AVERAGEA behaves identically to AVERAGE. Sum is 150, count is 5, average is 30.

---

### Example 2: Text Values Treated as Zero
```
=AVERAGEA(A1:A5)
```
Where A1:A5 contains: 10, 20, "N/A", 40, 50

**Result:** 24

**Explanation:** The text "N/A" is treated as 0. Calculation: (10 + 20 + 0 + 40 + 50) / 5 = 120 / 5 = 24. Compare to AVERAGE which would return 30 (ignoring the text cell entirely).

---

### Example 3: Logical TRUE and FALSE Values
```
=AVERAGEA(A1:A5)
```
Where A1:A5 contains: 10, TRUE, FALSE, 20, 30

**Result:** 12.2

**Explanation:** TRUE becomes 1, FALSE becomes 0. Calculation: (10 + 1 + 0 + 20 + 30) / 5 = 61 / 5 = 12.2. AVERAGE would return 20, only counting the three numbers.

---

### Example 4: Attendance Tracking with Text
```
=AVERAGEA(Attendance)
```
Where Attendance range contains: 1, 1, "Absent", 1, 1, "Absent", 1

**Result:** 0.714 (approximately)

**Explanation:** Using 1 for present and text for absent, AVERAGEA calculates attendance rate: (1+1+0+1+1+0+1)/7 = 5/7 = 0.714 or 71.4% attendance. The "Absent" text entries count as zeros.

---

### Example 5: Survey Responses with Non-Responders
```
=AVERAGEA(SurveyScores)
```
Where SurveyScores contains: 5, 4, "No Response", 5, 3, "Declined", 4

**Result:** 3

**Explanation:** Two text entries count as 0. Calculation: (5+4+0+5+3+0+4)/7 = 21/7 = 3. This treats non-responders as giving the lowest possible response (0), which may or may not be appropriate for your analysis.

---

### Example 6: Comparing AVERAGE vs AVERAGEA
```
=AVERAGE(A1:A10)    returns 75
=AVERAGEA(A1:A10)   returns 60
```
Where A1:A10 contains: 80, 70, "Error", 75, "Missing", 80, 70, 75, "N/A", 70

**Result:** AVERAGE = 74.29 (7 numbers), AVERAGEA = 52 (10 values with 3 zeros)

**Explanation:** AVERAGE ignores 3 text cells, averaging 7 numbers. AVERAGEA includes all 10 cells, treating text as 0, significantly lowering the result. Choose based on whether non-numeric entries should count.

---

### Example 7: Empty Cells vs Text Cells
```
=AVERAGEA(A1:A5)
```
Where A1:A5 contains: 10, 20, [empty cell], "Text", 30

**Result:** 15

**Explanation:** Empty cell is IGNORED (not counted at all). Text cell counts as 0. Calculation: (10 + 20 + 0 + 30) / 4 = 60 / 4 = 15. Note: count is 4, not 5, because empty cells don't count.

---

### Example 8: All Text Values
```
=AVERAGEA(A1:A4)
```
Where A1:A4 contains: "Red", "Blue", "Green", "Yellow"

**Result:** 0

**Explanation:** All text values become 0. Average of (0+0+0+0)/4 = 0. This is mathematically correct but probably indicates a data problem.

---

### Example 9: Boolean Checkboxes (Google Sheets)
```
=AVERAGEA(Checkboxes)
```
Where Checkboxes is a range of checkbox cells (TRUE/FALSE)

**Result:** 0.6 (example with 6 TRUE out of 10)

**Explanation:** Checkboxes in Google Sheets contain TRUE or FALSE. AVERAGEA converts TRUE to 1 and FALSE to 0, effectively calculating the percentage of checked boxes.

---

### Example 10: Error Values
```
=AVERAGEA(A1:A5)
```
Where A1:A5 contains: 10, 20, #DIV/0!, 30, 40

**Result:** #DIV/0!

**Explanation:** Unlike text which becomes 0, error values propagate through AVERAGEA. Any error in the range causes the function to return that error.

---

### Example 11: Multiple Ranges with Mixed Types
```
=AVERAGEA(A1:A5, B1:B5, C1:C5)
```
**Result:** Combined average across all ranges, with text as 0

**Explanation:** AVERAGEA processes all ranges together, treating any text cells as 0 and logical values as 1 (TRUE) or 0 (FALSE) across the entire combined dataset.

---

### Example 12: Percentage Calculation with Non-Responses
```
=AVERAGEA(Responses)*100 & "%"
```
Where Responses contains TRUE for Yes, FALSE for No, and text for non-responses

**Result:** "45%" (example)

**Explanation:** TRUE=1, FALSE=0, text=0. AVERAGEA gives the proportion of TRUE responses among ALL entries (including non-responses as negatives). Multiply by 100 for percentage.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | All cells in range are empty | Ensure at least one non-empty cell exists in the range. |
| `#DIV/0!` | Referenced range has no cells | Check range reference is valid and contains cells. |
| `#VALUE!` | Text passed directly as argument (not in cell) | Use cell references for text values, or use only numeric arguments directly. |
| `Error propagation` | Error value (#N/A, #REF!, etc.) in range | Remove or handle error values with IFERROR before averaging. |
| `Unexpected low average` | Text values being converted to 0 | Verify this is intended behavior; use AVERAGE if text should be ignored. |
| `Result differs from AVERAGE` | This is expected when text/logical values present | Understand the difference: AVERAGEA includes text as 0, AVERAGE ignores it. |

## Use Cases

### [[Survey Response Analysis]]

**Scenario:** A market researcher analyzes survey responses where some participants left questions blank or entered text comments instead of numeric ratings.

**Implementation:**
```
AVERAGEA approach: =AVERAGEA(Q1_Responses)
AVERAGE approach: =AVERAGE(Q1_Responses)
```
Choose based on how non-responses should be treated.

**Business Application:** Survey analysis requires decisions about non-response handling. If a customer satisfaction survey asks for 1-5 ratings and some respond "N/A", using AVERAGEA treats "N/A" as 0 (below lowest rating), penalizing non-response. This might be appropriate if non-response indicates dissatisfaction, or inappropriate if "N/A" means "not applicable to me." The choice between AVERAGE and AVERAGEA is a methodological decision affecting results interpretation.

**Technical Details:** Document which function was used and why. Consider creating two metrics: "Average among respondents" (AVERAGE) and "Average including non-responses as zero" (AVERAGEA). The gap between these reveals non-response impact.

---

### [[Employee Attendance Tracking]]

**Scenario:** An HR manager tracks daily attendance using a spreadsheet where "Present" is marked as 1, and absences are marked with reason text like "Sick", "Vacation", or "No-Show".

**Implementation:**
```
=AVERAGEA(EmployeeAttendance)*100
```
Returns attendance percentage treating any text as absence (0).

**Business Application:** This approach automatically calculates attendance rate without needing to convert all absence reasons to 0. Any non-numeric entry (whatever the reason) counts as absent. For 20 working days, if 15 are marked 1 and 5 contain text, AVERAGEA returns 0.75 (75% attendance). This simplifies data entry - staff can enter descriptive absence reasons while the formula handles the math.

**Technical Details:** Ensure "Present" entries are numeric 1, not text "1". Use ISNUMBER() to verify. Empty cells are ignored (not counted as either present or absent), so ensure all days have an entry.

---

### [[Boolean Field Analysis]]

**Scenario:** A database administrator analyzes export data where boolean fields appear as TRUE/FALSE values, calculating the percentage of TRUE values across records.

**Implementation:**
```
=AVERAGEA(BooleanField)
```
Returns proportion of TRUE values (0 to 1).

**Business Application:** When exporting data from databases, boolean fields often export as TRUE/FALSE rather than 1/0. AVERAGEA handles this natively, returning the proportion of TRUE values. For an "IsActive" field, AVERAGEA directly gives the percentage of active records. For an "HasError" field, it gives the error rate. This eliminates need for conversion formulas.

**Technical Details:** Multiply by 100 for percentage display. Note that error values (#N/A, #REF!) are not converted to 0 - they cause the function to return an error. Clean error values first with IFERROR if needed.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions since Excel 97
- **Maximum arguments:** 255 separate arguments
- **Text handling:** Text in cells becomes 0; text as direct argument causes #VALUE!
- **Logical handling:** TRUE = 1, FALSE = 0
- **Empty cells:** Ignored (not counted in average)
- **Error handling:** Errors propagate (return the error)
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Maximum arguments:** 30 separate arguments (but ranges can be very large)
- **Text handling:** Text in cells becomes 0; text as direct argument causes #VALUE!
- **Logical handling:** TRUE = 1, FALSE = 0
- **Empty cells:** Ignored (not counted in average)
- **Error handling:** Errors propagate (return the error)
- **Precision:** 15 significant decimal digits

### Key Difference Alert

AVERAGEA behaves identically between Excel and Google Sheets for all practical purposes. Both platforms:
- Treat text in referenced cells as 0
- Treat TRUE as 1, FALSE as 0
- Ignore empty cells completely
- Propagate error values
- Return #VALUE! for text passed directly as argument

The only notable difference is argument count limits (255 in Excel vs. 30 in Sheets), which rarely matters when using range references.

## Tips and Best Practices

1. **Understand when to use AVERAGEA vs AVERAGE.** Use AVERAGEA when non-numeric entries should count as 0. Use AVERAGE when non-numeric entries should be ignored. This is a methodological choice, not a technical one.

2. **Document your choice.** When sharing analysis, note which function was used and why. "Average score: 72 (non-responses counted as zero)" is clearer than just "Average score: 72."

3. **Verify text conversion is appropriate.** AVERAGEA treats ALL text as 0, regardless of content. "100" as text becomes 0, not 100. Check for numbers formatted as text with ISNUMBER().

4. **Remember empty cells are ignored.** Empty cells are NOT treated as 0 - they're completely excluded. Only cells containing text strings are converted to 0. Use "" (empty string formula result) if you need blanks to count as 0.

5. **Handle errors separately.** AVERAGEA propagates errors. Use IFERROR to convert errors to 0 or another value if errors should be included: =AVERAGEA(IFERROR(A1:A10, 0)).

6. **Consider AVERAGEIF for selective averaging.** If you want to average only numeric values but have more control than AVERAGE, consider AVERAGEIF with ISNUMBER criterion.

7. **Test with known data.** Before applying to important analysis, test AVERAGEA with sample data containing numbers, text, TRUE/FALSE, empty cells, and errors to verify behavior matches expectations.

8. **Use for checkbox percentages.** Google Sheets checkboxes contain TRUE/FALSE. AVERAGEA directly calculates the checked percentage without any conversion needed.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AVERAGE]] | Average of numbers, ignoring text and logical values | When non-numeric entries should be excluded from calculation |
| [[AVERAGEIF]] | Average values meeting a criterion | When you need conditional averaging with more control |
| [[AVERAGEIFS]] | Average with multiple criteria | For complex conditional averaging scenarios |
| [[COUNTA]] | Count non-empty cells | To count entries including text (companion to AVERAGEA) |
| [[MINA]] / [[MAXA]] | Min/Max including text and logical | For consistency when using AVERAGEA for mean |

### Commonly Used Together

**[[COUNTA]]** - Count with text

*Understanding your data composition:*
```
Total entries: =COUNTA(A2:A100)
Numeric entries: =COUNT(A2:A100)
Text/Logical entries: =COUNTA(A2:A100)-COUNT(A2:A100)
Average with text as 0: =AVERAGEA(A2:A100)
Average numbers only: =AVERAGE(A2:A100)
```
COUNTA counts all non-empty cells (like AVERAGEA includes them), while COUNT counts only numbers (like AVERAGE).

---

**[[IFERROR]]** - Error handling

*Include error cells as zero:*
```
=AVERAGEA(IFERROR(A2:A100, 0))
```
This converts any error values to 0 before averaging, so errors are treated like text rather than propagating.

---

**[[SUMPRODUCT]] with [[ISNUMBER]]** - Selective averaging

*Custom averaging with full control:*
```
=SUMPRODUCT(A2:A100*ISNUMBER(A2:A100))/SUMPRODUCT(--ISNUMBER(A2:A100))
```
Achieves same result as AVERAGE but shows the underlying logic explicitly.

---

**[[AVERAGE]]** - Comparison

*Show both metrics:*
```
Average (numbers only): =AVERAGE(A2:A100)
Average (text as 0): =AVERAGEA(A2:A100)
Difference: =AVERAGE(A2:A100)-AVERAGEA(A2:A100)
```
The difference reveals the impact of text entries on your data.

## Official Documentation

- **Microsoft Excel:** [AVERAGEA function](https://support.microsoft.com/en-us/office/averagea-function-f5f84098-d453-4f4c-bbba-3d2c66356091)
- **Google Sheets:** [AVERAGEA function](https://support.google.com/docs/answer/3093615)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original function |
| Excel 2003 | Continued support | No changes |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
