---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- minimum
- logical-values
- text-conversion
- extreme-values
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Minimum Including Text
- Min All Values
- Minimum with Logical
tags:
- minimum
- min
- logical
- text
- statistics
- extreme
---

# MINA

## Description

**MINA** returns the smallest value in a dataset, treating text and logical values differently than MIN. While MIN ignores text and logical values in ranges, MINA converts them: TRUE becomes 1, FALSE becomes 0, and text strings become 0. This makes MINA useful when you want logical and text values to participate in minimum calculations.

Use MINA when your data contains **logical values (TRUE/FALSE) or text that should be treated as zeros** rather than ignored. In datasets where FALSE represents a floor value, or where text placeholders should count as zero, MINA ensures these non-numeric values influence the result. If your range contains only numbers, MINA and MIN return identical results.

The key distinction is handling of non-numeric data: MIN(5, FALSE, "text", 1) returns 1 (ignores FALSE and "text"), while MINA(5, FALSE, "text", 1) returns 0 (FALSE=0 and "text"=0, both smaller than 1). The difference matters when FALSE or text-as-zero might be your minimum value.

**Important behavior note:** Text converting to 0 frequently affects MINA results with positive data. If your data is {5, 3, "N/A", 1}, MIN returns 1 (ignoring "N/A"), but MINA returns 0 (converting "N/A" to 0). For positive numeric data, any text presence pulls MINA down to zero or below.

## Syntax

> [!f(x)] MINA Syntax
>
> ```
> =MINA(value1, [value2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | First value, cell reference, or range. Numbers used as-is; TRUE = 1, FALSE = 0, text = 0. |
| `value2, ...` | No | Additional values, references, or ranges. Up to 255 arguments total. |

### Return Value

Returns the smallest value after converting logical values to 1/0 and text to 0. If all values convert to 0 or higher, text presence can make 0 the minimum. Returns #VALUE! if arguments cannot be processed. Unlike MIN, empty cells in ranges are treated as 0 when referenced directly.

## Examples

> [!f(x)] MINA Examples

### Example 1: Basic Numeric Minimum
```
=MINA(5, 3, 9, 1, 7)
```
**Result:** 1

**Explanation:** With only numbers, MINA works exactly like MIN, returning the smallest value.

---

### Example 2: Including FALSE Values
```
=MINA(5, FALSE, TRUE, 3)
```
**Result:** 0

**Explanation:** FALSE converts to 0, which is smaller than the numeric values. MINA returns 0.

---

### Example 3: Comparing MIN vs MINA with Logical
```
MIN:  =MIN(5, FALSE, 3)  → 3
MINA: =MINA(5, FALSE, 3) → 0
```
**Result:** Different results due to FALSE handling

**Explanation:** MIN ignores FALSE; MINA converts it to 0 and includes it in comparison.

---

### Example 4: Text Converts to Zero
```
=MINA(5, "text", 3)
```
**Result:** 0

**Explanation:** "text" converts to 0, which is smaller than 3 and 5. Text presence creates a zero minimum.

---

### Example 5: Comparing MIN vs MINA with Text
```
MIN:  =MIN(5, "N/A", 3)  → 3
MINA: =MINA(5, "N/A", 3) → 0
```
**Result:** Dramatically different results

**Explanation:** MIN ignores "N/A" and returns 3. MINA converts "N/A" to 0, making it the minimum.

---

### Example 6: Negative Numbers with Text
```
=MINA(-5, "text", -3)
```
**Result:** -5

**Explanation:** "text" becomes 0, but -5 is still smaller. With negatives, text-to-zero may not affect minimum.

---

### Example 7: Range with Mixed Data
```
=MINA(A1:A10)
```
**Result:** Minimum including logical/text conversions

**Explanation:** Scans entire range, converting any TRUE to 1, FALSE to 0, text to 0, then finds minimum.

---

### Example 8: All Logical Values
```
=MINA(TRUE, TRUE, FALSE, TRUE)
```
**Result:** 0

**Explanation:** TRUE=1, FALSE=0. Minimum is 0 from the FALSE value.

---

### Example 9: Detecting FALSE Presence
```
=MINA(BooleanRange)=0
```
**Result:** TRUE if any FALSE (or text) exists

**Explanation:** If minimum is 0 in a TRUE/FALSE range, at least one FALSE exists.

---

### Example 10: Empty Cells Behavior
```
=MINA(A1:A5) where A1=5, A2=empty, A3=3
```
**Result:** 3

**Explanation:** Empty cells in ranges are typically ignored in MINA (same as MIN for empty cells).

---

### Example 11: Direct Empty Reference
```
=MINA(A1) where A1 is empty
```
**Result:** 0

**Explanation:** Direct reference to empty cell treats it as 0, not ignored.

---

### Example 12: Multiple Ranges
```
=MINA(A1:A10, B1:B10, C1:C10)
```
**Result:** Minimum across all three ranges

**Explanation:** MINA accepts multiple ranges and finds the overall minimum with conversions applied.

---

### Example 13: Score Analysis with Missing Data
```
=MINA(TestScores) where some scores are "Absent"
```
**Result:** 0 (treating "Absent" as 0)

**Explanation:** Text placeholders become 0, which may or may not be the desired behavior.

---

### Example 14: Practical Difference Check
```
=IF(MIN(Range)=MINA(Range), "Same", "Different")
```
**Result:** Indicates if logical/text values affected result

**Explanation:** Quick test to see if your data contains logical/text values that MINA treats differently.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Error values in data | MINA propagates errors like #N/A, #REF!. Clean data or use IFERROR wrapper. |
| `Unexpected 0` | Text converting to 0 with positive data | Any text in positive data makes 0 the minimum. Consider MIN instead. |
| `#NAME?` | Misspelled function | Check spelling: MINA, not MINNA or MIINA. |
| `Result equals MIN` | No logical/text in data | If data is purely numeric, MINA and MIN are identical. This is expected. |
| `FALSE not counted` | Used MIN instead of MINA | MIN ignores FALSE. Use MINA if FALSE should count as 0. |

## Use Cases

### [[Survey and Response Analysis]]

**Scenario:** Find minimum response including FALSE/zero responses.

**Implementation:**
```
=MINA(Responses)
=IF(MINA(BooleanResponses)=0, "Has FALSE responses", "All TRUE")
=MINA(RatingColumn)                 → Includes FALSE=0
```

**Business Application:** Survey analysis, form validation, response tracking. When FALSE should count as a valid zero response.

**Technical Details:** In satisfaction surveys where FALSE means "not satisfied," MINA correctly treats these as minimum (zero) scores.

---

### [[Data Validation and Quality]]

**Scenario:** Detect presence of text placeholders or FALSE values in data.

**Implementation:**
```
=MINA(DataColumn)
=IF(MINA(Data)<MIN(Data), "Text/FALSE present", "Clean numeric")
=IF(AND(MIN(Data)>0, MINA(Data)=0), "Has text or FALSE", "OK")
```

**Business Application:** Data cleaning, ETL validation, report preparation. Identify non-numeric data in supposedly numeric columns.

**Technical Details:** When MIN > MINA, the data contains text or FALSE values that MINA converts to 0.

---

### [[Boolean Floor Detection]]

**Scenario:** Determine if any FALSE values exist in a boolean column.

**Implementation:**
```
=MINA(FlagColumn)=0                 → TRUE if any FALSE exists
=IF(MINA(Flags)=0, "Has FALSE", "All TRUE")
=NOT(MINA(BooleanRange))            → TRUE if any FALSE
```

**Business Application:** Status checking, validation rules, conditional processing. Quick detection of FALSE values.

**Technical Details:** Since FALSE=0 and TRUE=1, MINA returning 0 from a boolean column means at least one FALSE exists.

---

### [[Threshold and Floor Calculations]]

**Scenario:** Find minimum value where text represents "below threshold" as zero.

**Implementation:**
```
=MINA(Measurements)                 → Text "BDL" (below detection limit) = 0
=MINA(Concentrations)               → "<LOQ" values treated as 0
=MAX(MINA(Data), 0.01)              → Floor at 0.01 if text present
```

**Business Application:** Laboratory data, environmental monitoring, quality testing. Text codes for below-threshold readings should be treated as zero.

**Technical Details:** Scientific data often uses text codes like "ND" (not detected) or "<LOQ" (below limit of quantification). MINA treats these as zero, appropriate for minimum calculations.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Array handling:** Works with ranges and arrays
- **Text in direct arguments:** Text values as direct arguments (not in ranges) may cause #VALUE!
- **Empty cells:** Empty cells in ranges typically ignored; direct empty reference = 0

### Google Sheets
- **Availability:** All versions
- **Array handling:** Works with ARRAYFORMULA for array operations
- **Text handling:** Same conversion rules as Excel
- **Performance:** Efficient for large ranges

### Both Platforms
- TRUE converts to 1
- FALSE converts to 0
- Text strings convert to 0
- Error values propagate (cause #VALUE! or similar)
- Purely numeric data gives same result as MIN

## Tips and Best Practices

1. **Know when to use MINA vs MIN:** Use MINA only when you specifically want TRUE=1, FALSE=0, text=0 behavior. For typical numeric minimums, MIN is appropriate.

2. **Watch positive data with text:** Any text in positive-only data will make 0 the minimum. This is often unexpected—test with sample data.

3. **Use for FALSE detection:** MINA(boolean_range)=0 is a quick way to check if any FALSE exists in a range.

4. **Compare MIN and MINA for data quality:** If MIN(range) <> MINA(range), your data contains logical values or text affecting calculations.

5. **Consider MINIFS for conditional minimum:** If you need minimum with conditions, MINIFS may be more appropriate than MINA.

6. **Document when using MINA:** Since it's less common than MIN, comment your formulas to explain why MINA was chosen.

7. **Test with edge cases:** Before deploying, test with: all TRUE, all FALSE, all text, mixed types, and positive vs negative numbers.

8. **Remember direct vs range reference:** =MINA(A1) with empty A1 returns 0. =MINA(A1:A10) ignores empty cells. Behavior differs.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[MIN]] | Minimum ignoring text/logical in ranges | When you want to ignore non-numeric values |
| [[MAXA]] | Maximum with text/logical conversion | When finding maximum with same conversion rules |
| [[SMALL]] | Kth smallest value | When you need 2nd, 3rd, etc. smallest |
| [[MINIFS]] | Conditional minimum | When you need minimum with criteria |

### Commonly Used Together

**[[MAXA]]** - Paired maximum function

*Range statistics with text handling:*
```
Maximum: =MAXA(Range)
Minimum: =MINA(Range)
```
Both handle text/logical values consistently.

---

**[[MIN]]** - Compare for data quality

*Detect non-numeric values:*
```
=IF(MIN(Data)=MINA(Data), "Numeric only", "Has text/logical")
```
Different results indicate presence of text or logical values.

---

**[[AVERAGEA]]** - Consistent text handling

*Statistics with same conversion rules:*
```
Max:     =MAXA(Data)
Min:     =MINA(Data)
Average: =AVERAGEA(Data)
```
All three A-functions use same text/logical conversion.

---

**[[COUNTA]]** - Count all non-empty cells

*Understand data composition:*
```
=COUNTA(Range)    → Total non-empty cells
=COUNT(Range)     → Numeric cells only
```
Difference indicates text/logical cells present.

---

**[[IF]]** - Conditional logic with MINA

*Detection formula:*
```
=IF(MINA(BooleanRange)=0, "Has FALSE", "All TRUE")
```
MINA enables FALSE detection without COUNTIF.

## Official Documentation

- **Microsoft Excel:** [MINA function](https://support.microsoft.com/en-us/office/mina-function-245a6f46-7ca5-4dc7-ab49-805341bc31d3)
- **Google Sheets:** [MINA function](https://support.google.com/docs/answer/3094086)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original implementation |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
