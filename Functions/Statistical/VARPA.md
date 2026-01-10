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
- population-statistics
- text-handling
- dispersion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Population Variance Including Text
- Variance Population All Values
- VAR with Logical Population
tags:
- variance
- population
- statistics
- dispersion
- text-conversion
- logical
---

# VARPA

## Description

**VARPA** calculates the population variance while treating text and logical values differently than VAR.P. While VAR.P ignores text and logical values in ranges, VARPA converts them: TRUE becomes 1, FALSE becomes 0, and text strings become 0. The function uses the population formula (dividing by n).

Use VARPA when your data represents a **complete population** AND contains logical values (TRUE/FALSE) or text that should be treated as zeros. This is a specialized function combining population statistics with text/logical conversion—relatively rare in practice but useful for specific scenarios with mixed-type population data.

VARPA is the variance counterpart to STDEVPA—both use population formulas and the same text/logical conversion rules. The conversion rules also match other "A" functions: TRUE = 1, FALSE = 0, text = 0.

**VARPA requires at least 1 data point** (after conversions). Note that text converting to zero can significantly affect variance when your data is mostly positive—it introduces additional variation that might not be meaningful.

## Syntax

> [!f(x)] VARPA Syntax
>
> ```
> =VARPA(value1, [value2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | First value, cell reference, or range. Numbers used as-is; TRUE = 1, FALSE = 0, text = 0. |
| `value2, ...` | No | Additional values, references, or ranges. Up to 255 arguments. |

### Return Value

Returns population variance after converting logical values to 1/0 and text to 0. Always non-negative. Returns 0 for single value or all identical converted values.

## Examples

> [!f(x)] VARPA Examples

### Example 1: Numeric Data Only
```
=VARPA(2, 4, 6, 8, 10)
```
**Result:** 8 (same as VAR.P)

**Explanation:** With only numbers, VARPA equals VAR.P.

---

### Example 2: Including TRUE/FALSE
```
=VARPA(5, TRUE, FALSE, 5)
```
**Result:** Different from VAR.P({5, 5})

**Explanation:** TRUE=1, FALSE=0 included. Dataset becomes {5, 1, 0, 5} for calculation.

---

### Example 3: Text as Zero
```
=VARPA(10, "N/A", 10)
```
**Result:** Larger than VAR.P(10, 10)

**Explanation:** "N/A" becomes 0, increasing variation. Dataset becomes {10, 0, 10}.

---

### Example 4: Comparing All Four Variance Functions
```
Data: {5, "text", TRUE, 10}
VAR.S:  ignores text/TRUE → {5, 10} sample variance
VARA:   {5, 0, 1, 10} sample variance
VAR.P:  ignores text/TRUE → {5, 10} population variance
VARPA:  {5, 0, 1, 10} population variance
```
**Result:** Four different results

**Explanation:** Sample/population choice AND text handling both affect results.

---

### Example 5: All Boolean Population
```
=VARPA(TRUE, TRUE, FALSE, TRUE, FALSE)
```
**Result:** 0.24

**Explanation:** Data becomes {1, 1, 0, 1, 0}. Population variance with n=5 divisor.

---

### Example 6: Relationship to STDEVPA
```
=VARPA(Data) = STDEVPA(Data)^2
=SQRT(VARPA(Data)) = STDEVPA(Data)
```
**Result:** Always true

**Explanation:** Variance is SD squared. VARPA and STDEVPA are mathematically related.

---

### Example 7: Complete Survey Data
```
=VARPA(AllResponses)
```
**Result:** Population variance including text as 0

**Explanation:** If you have ALL responses and want text treated as 0, use VARPA.

---

### Example 8: Comparing VARA vs VARPA
```
=VARA({1, 2, FALSE, 4, 5})   → Sample variance of {1, 2, 0, 4, 5}
=VARPA({1, 2, FALSE, 4, 5})  → Population variance of {1, 2, 0, 4, 5}
```
**Result:** VARPA is slightly smaller

**Explanation:** VARPA uses n divisor (population) vs n-1 (sample).

---

### Example 9: Cell Range
```
=VARPA(A1:A100)
```
**Result:** Population variance with text/logical conversion

**Explanation:** Scans range, converts as needed, calculates population variance.

---

### Example 10: Effect of Text on Positive Data
```
Data: 100, 100, "Missing", 100
VAR.P:  Population variance of {100, 100, 100} = 0
VARPA:  Population variance of {100, 100, 0, 100} = 1875
```
**Result:** Text can dramatically increase variance

**Explanation:** Text converting to 0 creates variation where none existed.

---

### Example 11: Function Selection Decision Tree
```
Sample + ignore text   → VAR.S
Sample + convert text  → VARA
Population + ignore    → VAR.P
Population + convert   → VARPA
```
**Result:** Choose based on two criteria

**Explanation:** First determine sample vs population, then decide on text handling.

---

### Example 12: Binary Population Variance
```
For binary (0/1) population data:
Variance = p*(1-p) where p = proportion of 1s
=VARPA(BooleanPopulation) gives this result
```
**Result:** Bernoulli population variance

**Explanation:** VARPA correctly calculates population variance for TRUE/FALSE data.

---

### Example 13: Consistency Check
```
=VARPA(Data)        → Variance
=STDEVPA(Data)^2    → Should equal VARPA(Data)
=SQRT(VARPA(Data))  → Should equal STDEVPA(Data)
```
**Result:** Verify relationships

**Explanation:** VARPA and STDEVPA are mathematically related.

---

### Example 14: When to Use VARPA
```
Complete organizational data + TRUE/FALSE status
Example: All products' active status
=VARPA(ProductActive)  → Variance of activity across entire catalog
```
**Result:** Appropriate VARPA usage

**Explanation:** Complete population + meaningful boolean data = valid VARPA scenario.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Error values in data | Check for #N/A, #REF! etc. in range. |
| `#NAME?` | Misspelled function | Check spelling: VARPA, not VARPAA. |
| `Unexpected large variance` | Text converted to 0 | Text becomes 0, increasing spread. May not be desired. |
| `Result equals VAR.P` | No text/logical in data | With purely numeric data, functions are identical. |
| `Wrong function` | Confusion about function choice | Use VARPA only for population data where text/logical conversion is meaningful. |

## Use Cases

### [[Complete Boolean Population]]

**Scenario:** Calculate variance of TRUE/FALSE data for entire population.

**Implementation:**
```
=VARPA(AllEmployeeStatus)
=VARPA(AllProductActive)
=AVERAGEA(Flags)*(1-AVERAGEA(Flags))  → Equivalent for binary
```

**Business Application:** Complete status analysis, binary population metrics.

**Technical Details:** For binary population data, variance = p*(1-p). VARPA calculates this.

---

### [[Complete Dataset with Text Codes]]

**Scenario:** Analyze entire population where some values are text.

**Implementation:**
```
=VARPA(EntireDataset)
=VARPA(AllObservations)
If "N/A" should be 0: =VARPA(Data)
```

**Business Application:** Census data, complete inventories, exhaustive records.

**Technical Details:** If text codes represent zero (not missing), VARPA's conversion is meaningful.

---

### [[Organizational Complete Analysis]]

**Scenario:** Analyze full organizational data with mixed types.

**Implementation:**
```
=VARPA(AllDepartmentData)
=VARPA(CompleteStatusColumn)
Statistics: Mean=AVERAGEA, Variance=VARPA
```

**Business Application:** Complete organizational metrics.

**Technical Details:** Organizations often have exhaustive data. VARPA appropriate when text/logical should convert.

---

### [[Quality Records Analysis]]

**Scenario:** Analyze all quality records with pass/fail.

**Implementation:**
```
=VARPA(QualityResults)
=VARPA(InspectionOutcomes)  → TRUE=pass=1, FALSE=fail=0
=AVERAGEA(Results)*(1-AVERAGEA(Results))  → Binary variance
```

**Business Application:** 100% inspection records, complete quality data.

**Technical Details:** For binary pass/fail, VARPA = p*(1-p) where p = pass rate.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Conversion rules:** TRUE=1, FALSE=0, text=0
- **Empty cells:** Typically ignored in ranges
- **Population formula:** Uses n denominator

### Google Sheets
- **Availability:** All versions
- **Conversion rules:** Identical to Excel
- **Behavior:** Same calculation
- **Performance:** Efficient for large datasets

### Both Platforms
- Identical mathematical calculation
- Same conversion rules
- Same error conditions
- Minimum 1 data point (after conversions)

## Tips and Best Practices

1. **Know when to use VARPA:** Very specific: population data + text/logical that should convert to 0/1. Otherwise, simpler functions suffice.

2. **Consider if conversion is meaningful:** Does FALSE really mean 0? Does "N/A" mean 0? If not, VAR.P may be better.

3. **Watch text in positive data:** Text becoming 0 can dramatically increase variance.

4. **Match with other functions:** If using VARPA, use AVERAGEA and STDEVPA for consistency.

5. **Rare in practice:** VARPA is one of the least common functions. Most scenarios call for VAR.S or VAR.P.

6. **Document usage:** If using VARPA, document why—it's unusual.

7. **Compare all four:** Test with VAR.S, VARA, VAR.P, and VARPA to understand differences.

8. **Relationship to STDEVPA:** VARPA = STDEVPA^2. Use whichever your analysis needs.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[VAR.P]] | Population variance, ignores text/logical | When text/logical should be excluded |
| [[VARA]] | Sample variance with text/logical conversion | For sample data with text/logical |
| [[VAR.S]] | Sample variance, ignores text/logical | For sample data without text/logical |
| [[STDEVPA]] | Population SD with text/logical conversion | When you need SD, not variance |

### Commonly Used Together

**[[STDEVPA]]** - Standard deviation

*SD-variance relationship:*
```
=VARPA(Data)        → Variance
=STDEVPA(Data)      → Standard deviation
=STDEVPA(Data)^2    → Should equal VARPA(Data)
```
Related by squaring.

---

**[[AVERAGEA]]** - Consistent mean

*Population statistics with conversions:*
```
Mean:     =AVERAGEA(Data)
Variance: =VARPA(Data)
```
Both use same text/logical conversion.

---

**[[VAR.P]]** - Compare for data quality

*Detect non-numeric content:*
```
=IF(VARPA(Data)=VAR.P(Data), "Numeric only", "Has text/logical")
```
Different results indicate text/logical presence.

---

**[[MAXA]]** and **[[MINA]]** - Complete statistics

*Full descriptive statistics:*
```
Mean:     =AVERAGEA(Data)
Variance: =VARPA(Data)
Max:      =MAXA(Data)
Min:      =MINA(Data)
```
All "A" functions for consistency.

---

**[[COUNTA]]** - Understand data composition

*Data profiling:*
```
=COUNTA(Range) vs =COUNT(Range)
```
Difference shows text/logical count.

## Official Documentation

- **Microsoft Excel:** [VARPA function](https://support.microsoft.com/en-us/office/varpa-function-59a62b4e-1d5c-4c04-8d2d-2eb17dd20afe)
- **Google Sheets:** [VARPA function](https://support.google.com/docs/answer/3094063)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original implementation |
| Excel 2007+ | All subsequent | No changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
