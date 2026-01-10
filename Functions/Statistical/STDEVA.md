---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- standard-deviation
- sample-statistics
- text-handling
- variability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Standard Deviation Including Text
- StdDev All Values
- Sample StdDev with Logical
tags:
- standard-deviation
- sample
- statistics
- variability
- text-conversion
- logical
---

# STDEVA

## Description

**STDEVA** calculates the sample standard deviation while treating text and logical values differently than STDEV.S. While STDEV.S ignores text and logical values in ranges, STDEVA converts them: TRUE becomes 1, FALSE becomes 0, and text strings become 0. The function uses the sample formula (dividing by n-1).

Use STDEVA when your data contains **logical values (TRUE/FALSE) or text that should be treated as zeros** rather than ignored. In datasets where FALSE represents a zero value or text placeholders should count as zero, STDEVA ensures these non-numeric values influence the standard deviation calculation.

The conversion rules match other "A" functions (AVERAGEA, MAXA, MINA): TRUE = 1, FALSE = 0, text = 0. This consistency allows you to calculate coherent statistics across mixed-type datasets. If your data is purely numeric, STDEVA and STDEV.S produce identical results.

**STDEVA requires at least 2 data points** (after conversions) to calculate sample standard deviation. With only one value, the formula would require division by zero (n-1 = 0). Note that text converting to zero can significantly affect standard deviation when your data is mostly positive—it introduces additional variation.

## Syntax

> [!f(x)] STDEVA Syntax
>
> ```
> =STDEVA(value1, [value2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | First value, cell reference, or range. Numbers used as-is; TRUE = 1, FALSE = 0, text = 0. |
| `value2, ...` | No | Additional values, references, or ranges. Up to 255 arguments. Minimum 2 data points required. |

### Return Value

Returns sample standard deviation after converting logical values to 1/0 and text to 0. Always non-negative. Returns #DIV/0! if fewer than 2 data points. Returns #VALUE! for direct argument errors.

## Examples

> [!f(x)] STDEVA Examples

### Example 1: Numeric Data Only
```
=STDEVA(2, 4, 6, 8, 10)
```
**Result:** 3.162... (same as STDEV.S)

**Explanation:** With only numbers, STDEVA equals STDEV.S.

---

### Example 2: Including TRUE/FALSE
```
=STDEVA(5, TRUE, FALSE, 5)
```
**Result:** Different from STDEV.S({5, 5})

**Explanation:** TRUE=1, FALSE=0 are included. Dataset becomes {5, 1, 0, 5} with SD calculated accordingly.

---

### Example 3: Text as Zero
```
=STDEVA(10, "N/A", 10)
```
**Result:** Larger than STDEV.S(10, 10)

**Explanation:** "N/A" becomes 0, increasing variation. Dataset becomes {10, 0, 10}.

---

### Example 4: Comparing STDEV.S vs STDEVA
```
Data: {5, "text", TRUE, 10}
STDEV.S: ignores text and TRUE → STDEV.S(5, 10) = 3.54
STDEVA:  {5, 0, 1, 10} → approximately 4.27
```
**Result:** Different results due to inclusion of converted values

**Explanation:** STDEVA includes converted values, changing both mean and spread.

---

### Example 5: All Boolean Data
```
=STDEVA(TRUE, FALSE, TRUE, TRUE, FALSE)
```
**Result:** Approximately 0.548

**Explanation:** Data becomes {1, 0, 1, 1, 0}. STDEVA calculates SD of these values.

---

### Example 6: Survey with Mixed Types
```
=STDEVA(SurveyColumn)
```
**Result:** SD including text responses as 0

**Explanation:** Text responses ("N/A", "Skipped") become 0, included in variability calculation.

---

### Example 7: Cell Range
```
=STDEVA(A1:A100)
```
**Result:** Sample SD with text/logical conversion

**Explanation:** Standard usage—scans range, converts as needed, calculates SD.

---

### Example 8: Multiple Ranges
```
=STDEVA(A1:A50, B1:B50)
```
**Result:** Combined SD across both ranges

**Explanation:** Both ranges treated as single dataset with text/logical conversions.

---

### Example 9: Effect of Text on Positive Data
```
Data in cells: 100, 100, "Missing", 100
STDEV.S: SD of {100, 100, 100} = 0
STDEVA:  SD of {100, 100, 0, 100} = 50
```
**Result:** Text can dramatically increase SD

**Explanation:** Text converting to 0 in otherwise uniform positive data creates large variation.

---

### Example 10: Detecting Data Quality Issues
```
=IF(STDEVA(Data)>STDEV.S(Data)*1.5, "Check for text/logical", "Data appears clean")
```
**Result:** Flag potential data quality issues

**Explanation:** Large discrepancy between STDEVA and STDEV.S suggests significant text/logical content.

---

### Example 11: Minimum Data Points
```
=STDEVA(TRUE, FALSE)
```
**Result:** 0.707... (SD of {1, 0})

**Explanation:** Minimum 2 points required. Converts and calculates.

---

### Example 12: Single Value with Text
```
=STDEVA(5, "text")
```
**Result:** 3.536... (SD of {5, 0})

**Explanation:** Text becomes 0, creating a two-value dataset.

---

### Example 13: Quality Check Formula
```
=IF(ABS(STDEVA(Data)-STDEV.S(Data))<0.001, "Numeric only", "Has text/logical")
```
**Result:** Detect non-numeric content

**Explanation:** If results differ significantly, data contains text or logical values.

---

### Example 14: Consistent Mixed-Type Statistics
```
Mean:   =AVERAGEA(Data)
StdDev: =STDEVA(Data)
Max:    =MAXA(Data)
Min:    =MINA(Data)
```
**Result:** Coherent statistics for mixed-type data

**Explanation:** All "A" functions use same conversion rules.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Fewer than 2 data points | Need minimum 2 values for sample SD. |
| `#VALUE!` | Error values in data | Check for #N/A, #REF! etc. in range. |
| `#NAME?` | Misspelled function | Check spelling: STDEVA, not STDDEVA or STDEVA. |
| `Unexpected large SD` | Text converted to 0 | Text becomes 0, increasing spread in positive data. May not be desired behavior. |
| `Result equals STDEV.S` | No text/logical in data | With purely numeric data, functions are identical. This is expected. |

## Use Cases

### [[Survey Data with Missing Responses]]

**Scenario:** Calculate variability when some responses are text codes.

**Implementation:**
```
=STDEVA(ResponseColumn)
=STDEVA(Ratings)                    → Text responses = 0
=IF(STDEVA(Data)>STDEV(Data)*2, "High missing impact", "Manageable")
```

**Business Application:** Survey analysis, questionnaire processing. When text codes ("N/A", "Refused") should be treated as zero.

**Technical Details:** Consider whether treating missing as zero is appropriate. For many analyses, excluding missing (using STDEV.S) may be preferred.

---

### [[Boolean Flag Variability]]

**Scenario:** Calculate standard deviation of TRUE/FALSE data.

**Implementation:**
```
=STDEVA(FlagColumn)
=STDEVA(CompletionFlags)            → TRUE=1, FALSE=0
=SQRT(AVERAGEA(Flags)*(1-AVERAGEA(Flags)))  → Equivalent for binary
```

**Business Application:** Completion rate variability, success/failure analysis, binary outcome spread.

**Technical Details:** For binary (0/1) data, SD = sqrt(p*(1-p)) where p is proportion of 1s. STDEVA calculates this correctly.

---

### [[Mixed Data Type Processing]]

**Scenario:** Process data that legitimately contains numbers, text, and logical values.

**Implementation:**
```
=STDEVA(MixedColumn)
=IF(STDEVA(Data)=STDEV.S(Data), "Pure numeric", "Mixed types")
Compare: STDEV.S (exclude non-numeric) vs STDEVA (include as 0/1)
```

**Business Application:** Legacy data processing, imported data, mixed-format fields.

**Technical Details:** STDEVA provides predictable behavior: numbers as-is, TRUE=1, FALSE=0, text=0. Use when this conversion makes sense for your analysis.

---

### [[Data Quality Assessment]]

**Scenario:** Identify data columns with unexpected non-numeric content.

**Implementation:**
```
=STDEVA(Column) - STDEV.S(Column)
=IF(STDEVA(A:A)<>STDEV.S(A:A), "Contains text/logical", "Clean")
=ABS(STDEVA(Data)-STDEV(Data))/STDEV(Data)  → Relative difference
```

**Business Application:** Data validation, ETL quality checks, data profiling.

**Technical Details:** Discrepancy between STDEVA and STDEV.S reveals non-numeric content that affects statistics differently.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Conversion rules:** TRUE=1, FALSE=0, text=0
- **Empty cells:** In ranges, typically ignored; direct reference = 0
- **Sample formula:** Uses n-1 denominator

### Google Sheets
- **Availability:** All versions
- **Conversion rules:** Identical to Excel
- **Behavior:** Same calculation and conversions
- **Performance:** Efficient for large datasets

### Both Platforms
- Identical mathematical calculation
- Same conversion rules for text/logical
- Same error conditions
- Empty cells in ranges generally ignored

## Tips and Best Practices

1. **Know when to use STDEVA:** Use when TRUE=1, FALSE=0, text=0 conversion is meaningful for your analysis. Otherwise, STDEV.S is usually appropriate.

2. **Watch text in positive data:** Text becoming 0 can dramatically increase SD when other values are positive. Test with sample data first.

3. **Use for binary data:** STDEVA correctly handles TRUE/FALSE columns, treating them as 1/0 for standard deviation.

4. **Compare with STDEV.S:** If STDEVA >> STDEV.S, significant text/logical content is affecting results.

5. **Consistent "A" function usage:** If using AVERAGEA, also use STDEVA for consistent text/logical handling.

6. **Consider data cleaning:** Often, cleaning text values to proper numbers or blanks is better than relying on STDEVA's conversions.

7. **Document your choice:** Since STDEVA is less common, document why you chose it over STDEV.S in your analysis.

8. **Empty cells differ from text:** Empty cells in ranges are typically ignored, unlike text which becomes 0.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[STDEV.S]] | Sample SD, ignores text/logical | When text/logical should be excluded |
| [[STDEV.P]] | Population SD, ignores text/logical | For population data, excluding text |
| [[STDEVPA]] | Population SD with text/logical conversion | For population data with text/logical inclusion |
| [[VAR.S]] | Sample variance, ignores text/logical | When you need variance, not SD |

### Commonly Used Together

**[[AVERAGEA]]** - Consistent mean calculation

*Paired statistics:*
```
Mean:   =AVERAGEA(Data)
StdDev: =STDEVA(Data)
```
Both use same text/logical conversion for consistency.

---

**[[STDEV.S]]** - Compare for data quality

*Detect non-numeric content:*
```
=IF(STDEVA(Data)=STDEV.S(Data), "Clean", "Has text/logical")
```
Different results indicate text/logical values in data.

---

**[[MAXA]]** and **[[MINA]]** - Complete statistics

*Full descriptive statistics with same rules:*
```
Mean: =AVERAGEA(Data)
SD:   =STDEVA(Data)
Max:  =MAXA(Data)
Min:  =MINA(Data)
```
All "A" functions use consistent conversion.

---

**[[VARA]]** - Variance with same conversions

*Variance-SD relationship:*
```
=STDEVA(Data)^2  → Should equal VARA(Data)
=SQRT(VARA(Data)) → Should equal STDEVA(Data)
```
Variance is squared standard deviation.

---

**[[COUNT]]** vs **[[COUNTA]]** - Understand data composition

*Data profiling:*
```
Numeric cells: =COUNT(Range)
All non-empty: =COUNTA(Range)
```
Difference shows text/logical cell count.

## Official Documentation

- **Microsoft Excel:** [STDEVA function](https://support.microsoft.com/en-us/office/stdeva-function-5ff38888-7ea5-48de-9a6d-11ed73b29e9d)
- **Google Sheets:** [STDEVA function](https://support.google.com/docs/answer/3094089)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original implementation |
| Excel 2007+ | All subsequent | No changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
