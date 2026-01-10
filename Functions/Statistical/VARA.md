---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- variance
- sample-statistics
- text-handling
- dispersion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sample Variance Including Text
- Variance All Values
- VAR with Logical
tags:
- variance
- sample
- statistics
- dispersion
- text-conversion
- logical
---

# VARA

## Description

**VARA** calculates the sample variance while treating text and logical values differently than VAR.S. While VAR.S ignores text and logical values in ranges, VARA converts them: TRUE becomes 1, FALSE becomes 0, and text strings become 0. The function uses the sample formula (dividing by n-1).

Use VARA when your data contains **logical values (TRUE/FALSE) or text that should be treated as zeros** rather than ignored. Variance measures how spread out values are from the mean—it's the square of the standard deviation. VARA is the variance counterpart to STDEVA, using the same text/logical conversion rules.

The conversion rules match other "A" functions: TRUE = 1, FALSE = 0, text = 0. This consistency allows coherent variance calculations across mixed-type datasets. If your data is purely numeric, VARA and VAR.S produce identical results.

**VARA requires at least 2 data points** (after conversions) to calculate sample variance. With only one value, division by n-1 = 0 causes an error. Note that text converting to zero can significantly affect variance when your data is mostly positive.

## Syntax

> [!f(x)] VARA Syntax
>
> ```
> =VARA(value1, [value2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | First value, cell reference, or range. Numbers used as-is; TRUE = 1, FALSE = 0, text = 0. |
| `value2, ...` | No | Additional values, references, or ranges. Up to 255 arguments. Minimum 2 data points required. |

### Return Value

Returns sample variance after converting logical values to 1/0 and text to 0. Always non-negative. Returns #DIV/0! if fewer than 2 data points. Returns #VALUE! for direct argument errors.

## Examples

> [!f(x)] VARA Examples

### Example 1: Numeric Data Only
```
=VARA(2, 4, 6, 8, 10)
```
**Result:** 10 (same as VAR.S)

**Explanation:** With only numbers, VARA equals VAR.S.

---

### Example 2: Including TRUE/FALSE
```
=VARA(5, TRUE, FALSE, 5)
```
**Result:** Different from VAR.S({5, 5})

**Explanation:** TRUE=1, FALSE=0 included. Dataset becomes {5, 1, 0, 5} for variance calculation.

---

### Example 3: Text as Zero
```
=VARA(10, "N/A", 10)
```
**Result:** Larger than VAR.S(10, 10)

**Explanation:** "N/A" becomes 0, increasing variation. Dataset becomes {10, 0, 10}.

---

### Example 4: Comparing VAR.S vs VARA
```
Data: {5, "text", TRUE, 10}
VAR.S: ignores text/TRUE → VAR.S(5, 10) = 12.5
VARA:  {5, 0, 1, 10} → approximately 18.25
```
**Result:** Different results due to inclusion of converted values

**Explanation:** VARA includes converted values, changing both mean and variance.

---

### Example 5: All Boolean Data
```
=VARA(TRUE, FALSE, TRUE, TRUE, FALSE)
```
**Result:** 0.3

**Explanation:** Data becomes {1, 0, 1, 1, 0}. Sample variance of binary data.

---

### Example 6: Relationship to STDEVA
```
=VARA(Data) = STDEVA(Data)^2
=SQRT(VARA(Data)) = STDEVA(Data)
```
**Result:** True for all data

**Explanation:** Variance is standard deviation squared. These functions are mathematically related.

---

### Example 7: Cell Range
```
=VARA(A1:A100)
```
**Result:** Sample variance with text/logical conversion

**Explanation:** Standard usage—scans range, converts as needed, calculates variance.

---

### Example 8: Multiple Ranges
```
=VARA(A1:A50, B1:B50)
```
**Result:** Combined variance across both ranges

**Explanation:** Both ranges treated as single dataset with text/logical conversions.

---

### Example 9: Effect of Text on Positive Data
```
Data: 100, 100, "Missing", 100
VAR.S: Variance of {100, 100, 100} = 0
VARA:  Variance of {100, 100, 0, 100} = 2500
```
**Result:** Text can dramatically increase variance

**Explanation:** Text converting to 0 creates variation where none existed.

---

### Example 10: Survey Response Analysis
```
=VARA(ResponseColumn)
```
**Result:** Variance including text responses as 0

**Explanation:** Text placeholders become 0, included in variability calculation.

---

### Example 11: Binary Variance Formula
```
For binary (0/1) data:
Variance = n*p*(1-p)/(n-1)
where p = proportion of 1s
=VARA(BinaryData) gives this result
```
**Result:** Bernoulli variance estimate

**Explanation:** VARA correctly calculates sample variance for TRUE/FALSE data.

---

### Example 12: Detecting Data Issues
```
=IF(VARA(Data)>VAR.S(Data)*2, "Significant text/logical impact", "Minimal impact")
```
**Result:** Flag unusual discrepancies

**Explanation:** Large difference suggests text/logical content affecting calculations.

---

### Example 13: Minimum Data Points
```
=VARA(TRUE, FALSE)
```
**Result:** 0.5 (variance of {1, 0})

**Explanation:** Minimum 2 points required. Mean=0.5, variance=0.5.

---

### Example 14: Complete Descriptive Statistics
```
Mean:     =AVERAGEA(Data)
Variance: =VARA(Data)
StdDev:   =STDEVA(Data)  or  =SQRT(VARA(Data))
```
**Result:** Consistent statistics for mixed-type data

**Explanation:** All "A" functions use same conversion rules.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Fewer than 2 data points | Need minimum 2 values for sample variance. |
| `#VALUE!` | Error values in data | Check for #N/A, #REF! etc. in range. |
| `#NAME?` | Misspelled function | Check spelling: VARA, not VARAA or VARIA. |
| `Unexpected large variance` | Text converted to 0 | Text becomes 0, increasing spread. May not be desired. |
| `Result equals VAR.S` | No text/logical in data | With purely numeric data, functions are identical. |

## Use Cases

### [[Survey Data with Missing Responses]]

**Scenario:** Calculate variance when some responses are text codes.

**Implementation:**
```
=VARA(ResponseColumn)
=VARA(Ratings)                    → Text responses = 0
=IF(VARA(Data)>VAR(Data)*2, "High missing impact", "Manageable")
```

**Business Application:** Survey analysis, questionnaire processing.

**Technical Details:** Consider whether treating missing as zero is appropriate for variance calculation.

---

### [[Boolean Data Variance]]

**Scenario:** Calculate variance of TRUE/FALSE data.

**Implementation:**
```
=VARA(FlagColumn)
=VARA(CompletionStatus)           → TRUE=1, FALSE=0
=AVERAGEA(Flags)*(1-AVERAGEA(Flags))*COUNT(Flags)/(COUNT(Flags)-1)  → Equivalent
```

**Business Application:** Completion rate analysis, success/failure variance, binary outcome spread.

**Technical Details:** For binary data, variance = n*p*(1-p)/(n-1). VARA calculates this correctly.

---

### [[Mixed Data Type Processing]]

**Scenario:** Process data with numbers, text, and logical values.

**Implementation:**
```
=VARA(MixedColumn)
=IF(VARA(Data)=VAR.S(Data), "Pure numeric", "Mixed types")
```

**Business Application:** Legacy data processing, imported data, mixed-format fields.

**Technical Details:** VARA provides predictable conversion: numbers as-is, TRUE=1, FALSE=0, text=0.

---

### [[Data Quality Assessment]]

**Scenario:** Identify data columns with unexpected non-numeric content.

**Implementation:**
```
=VARA(Column) - VAR.S(Column)
=IF(VARA(A:A)<>VAR.S(A:A), "Contains text/logical", "Clean")
```

**Business Application:** Data validation, ETL quality checks.

**Technical Details:** Discrepancy reveals non-numeric content affecting statistics.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Conversion rules:** TRUE=1, FALSE=0, text=0
- **Empty cells:** In ranges, typically ignored
- **Sample formula:** Uses n-1 denominator

### Google Sheets
- **Availability:** All versions
- **Conversion rules:** Identical to Excel
- **Behavior:** Same calculation
- **Performance:** Efficient for large datasets

### Both Platforms
- Identical mathematical calculation
- Same conversion rules
- Same error conditions
- Empty cells in ranges generally ignored

## Tips and Best Practices

1. **Know when to use VARA:** Use when TRUE=1, FALSE=0, text=0 conversion is meaningful. Otherwise, VAR.S is usually appropriate.

2. **Watch text in positive data:** Text becoming 0 can dramatically increase variance when other values are positive.

3. **Use for binary data:** VARA correctly handles TRUE/FALSE columns as 1/0 for variance.

4. **Compare with VAR.S:** If VARA >> VAR.S, significant text/logical content is affecting results.

5. **Consistent function usage:** If using AVERAGEA, also use VARA for consistent text/logical handling.

6. **Relationship to STDEVA:** VARA = STDEVA^2. Both use identical conversion rules.

7. **Consider data cleaning:** Often, cleaning text to numbers or blanks is better than relying on conversions.

8. **Document your choice:** VARA is uncommon; document why you chose it.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VAR.S]] | Sample variance, ignores text/logical | When text/logical should be excluded |
| [[VAR.P]] | Population variance, ignores text/logical | For population data |
| [[VARPA]] | Population variance with text/logical conversion | For population data with conversion |
| [[STDEVA]] | Sample SD with text/logical conversion | When you need SD, not variance |

### Commonly Used Together

**[[STDEVA]]** - Standard deviation

*SD-variance relationship:*
```
=VARA(Data)       → Variance
=STDEVA(Data)     → Standard deviation
=STDEVA(Data)^2   → Should equal VARA(Data)
```
Related by squaring.

---

**[[AVERAGEA]]** - Consistent mean

*Paired statistics:*
```
Mean:     =AVERAGEA(Data)
Variance: =VARA(Data)
```
Both use same conversion rules.

---

**[[VAR.S]]** - Compare for data quality

*Detect non-numeric content:*
```
=IF(VARA(Data)=VAR.S(Data), "Clean", "Has text/logical")
```
Different results indicate text/logical presence.

---

**[[MAXA]]** and **[[MINA]]** - Complete statistics

*Full descriptive statistics:*
```
Mean:     =AVERAGEA(Data)
Variance: =VARA(Data)
Max:      =MAXA(Data)
Min:      =MINA(Data)
```
All "A" functions for consistency.

---

**[[COUNTA]]** - Understand data composition

*Data profiling:*
```
All non-empty: =COUNTA(Range)
Numeric only:  =COUNT(Range)
```
Difference shows text/logical count.

## Official Documentation

- **Microsoft Excel:** [VARA function](https://support.microsoft.com/en-us/office/vara-function-3de77469-fa3a-47b4-85fd-81758a1e1d07)
- **Google Sheets:** [VARA function](https://support.google.com/docs/answer/3094038)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original implementation |
| Excel 2007+ | All subsequent | No changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
