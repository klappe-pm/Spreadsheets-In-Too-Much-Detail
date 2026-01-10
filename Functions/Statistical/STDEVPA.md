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
- population-statistics
- text-handling
- variability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Population StdDev Including Text
- StdDev Population All Values
- Population SD with Logical
tags:
- standard-deviation
- population
- statistics
- variability
- text-conversion
- logical
---

# STDEVPA

## Description

**STDEVPA** calculates the population standard deviation while treating text and logical values differently than STDEV.P. While STDEV.P ignores text and logical values in ranges, STDEVPA converts them: TRUE becomes 1, FALSE becomes 0, and text strings become 0. The function uses the population formula (dividing by n).

Use STDEVPA when your data represents a **complete population** AND contains logical values (TRUE/FALSE) or text that should be treated as zeros. This is a specialized function combining population statistics with text/logical conversion—relatively rare in practice but useful for specific scenarios with mixed-type population data.

The conversion rules match other "A" functions (AVERAGEA, MAXA, MINA, STDEVA): TRUE = 1, FALSE = 0, text = 0. This ensures consistent statistics across mixed-type datasets. The population formula (dividing by n instead of n-1) makes it appropriate when you have all data, not a sample.

**STDEVPA requires at least 1 data point** (after conversions). Note that text converting to zero can significantly affect standard deviation when your data is mostly positive—it introduces additional variation that might not be meaningful.

## Syntax

> [!f(x)] STDEVPA Syntax
>
> ```
> =STDEVPA(value1, [value2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value1` | Yes | First value, cell reference, or range. Numbers used as-is; TRUE = 1, FALSE = 0, text = 0. |
| `value2, ...` | No | Additional values, references, or ranges. Up to 255 arguments. |

### Return Value

Returns population standard deviation after converting logical values to 1/0 and text to 0. Always non-negative. Returns 0 for single value or all identical converted values.

## Examples

> [!f(x)] STDEVPA Examples

### Example 1: Numeric Data Only
```
=STDEVPA(2, 4, 6, 8, 10)
```
**Result:** 2.828... (same as STDEV.P)

**Explanation:** With only numbers, STDEVPA equals STDEV.P.

---

### Example 2: Including TRUE/FALSE
```
=STDEVPA(5, TRUE, FALSE, 5)
```
**Result:** Different from STDEV.P({5, 5})

**Explanation:** TRUE=1, FALSE=0 are included. Dataset becomes {5, 1, 0, 5} for calculation.

---

### Example 3: Text as Zero
```
=STDEVPA(10, "N/A", 10)
```
**Result:** Larger than STDEV.P(10, 10)

**Explanation:** "N/A" becomes 0, increasing variation. Dataset becomes {10, 0, 10}.

---

### Example 4: Comparing All Four SD Functions
```
Data: {5, "text", TRUE, 10}
STDEV.S:  ignores text/TRUE → {5, 10} sample SD
STDEVA:   {5, 0, 1, 10} sample SD
STDEV.P:  ignores text/TRUE → {5, 10} population SD
STDEVPA:  {5, 0, 1, 10} population SD
```
**Result:** Four different results

**Explanation:** Sample/population choice AND text handling both affect results.

---

### Example 5: All Boolean Population
```
=STDEVPA(TRUE, TRUE, FALSE, TRUE, FALSE)
```
**Result:** Approximately 0.490

**Explanation:** Data becomes {1, 1, 0, 1, 0}. Population SD with n=5 divisor.

---

### Example 6: Complete Survey Data
```
=STDEVPA(AllResponses)
```
**Result:** Population SD including text responses as 0

**Explanation:** If you have ALL survey responses and want text treated as 0, use STDEVPA.

---

### Example 7: Comparing STDEVA vs STDEVPA
```
=STDEVA({1, 2, FALSE, 4, 5})   → Sample SD of {1, 2, 0, 4, 5}
=STDEVPA({1, 2, FALSE, 4, 5})  → Population SD of {1, 2, 0, 4, 5}
```
**Result:** STDEVPA is slightly smaller

**Explanation:** Same data, but STDEVPA uses n divisor (population) vs n-1 (sample).

---

### Example 8: Cell Range
```
=STDEVPA(A1:A100)
```
**Result:** Population SD with text/logical conversion

**Explanation:** Scans range, converts as needed, calculates population SD.

---

### Example 9: Multiple Ranges
```
=STDEVPA(A1:A50, B1:B50)
```
**Result:** Combined population SD

**Explanation:** Both ranges treated as single population with text/logical conversions.

---

### Example 10: Effect of Text on Positive Data
```
Data: 100, 100, "Missing", 100
STDEV.P:  Population SD of {100, 100, 100} = 0
STDEVPA:  Population SD of {100, 100, 0, 100} = 43.3
```
**Result:** Text can dramatically increase SD

**Explanation:** Text converting to 0 creates variation where none existed before.

---

### Example 11: Function Selection Decision Tree
```
Sample + ignore text   → STDEV.S
Sample + convert text  → STDEVA
Population + ignore    → STDEV.P
Population + convert   → STDEVPA
```
**Result:** Choose based on two criteria

**Explanation:** First determine sample vs population, then decide on text handling.

---

### Example 12: Complete Product Status
```
=STDEVPA(ProductStatus)
```
**Result:** Population SD of all products

**Explanation:** All products in catalog with TRUE/FALSE status = population data with logical values.

---

### Example 13: Consistency Check
```
=STDEVPA(Data)^2  → Should equal VARPA(Data)
=SQRT(VARPA(Data)) → Should equal STDEVPA(Data)
```
**Result:** Verify SD-variance relationship

**Explanation:** STDEVPA and VARPA are mathematically related.

---

### Example 14: When to Actually Use STDEVPA
```
Scenario: Complete organizational data with TRUE/FALSE fields
Example: All employees' completion status for training
=STDEVPA(TrainingCompleted)
```
**Result:** Appropriate STDEVPA usage

**Explanation:** Complete population + meaningful boolean data = rare but valid STDEVPA use case.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Error values in data | Check for #N/A, #REF! etc. in range. |
| `#NAME?` | Misspelled function | Check spelling: STDEVPA, not STDDEVPA. |
| `Unexpected large SD` | Text converted to 0 | Text becomes 0, increasing spread. May not be desired. |
| `Result equals STDEV.P` | No text/logical in data | With purely numeric data, functions are identical. |
| `Wrong function` | Confusion about which function | Use STDEVPA only for population data where text/logical conversion is meaningful. |

## Use Cases

### [[Complete Boolean Population Data]]

**Scenario:** Calculate variability in TRUE/FALSE data for entire population.

**Implementation:**
```
=STDEVPA(AllEmployeeStatus)
=STDEVPA(AllProductActive)
=STDEVPA(AllCompletionFlags)
```

**Business Application:** HR status analysis, product catalog analysis, compliance tracking.

**Technical Details:** When you have TRUE/FALSE for every member of a population, STDEVPA treats TRUE=1, FALSE=0 and calculates population SD.

---

### [[Complete Dataset with Missing Codes]]

**Scenario:** Analyze entire population where missing values are coded as text.

**Implementation:**
```
=STDEVPA(EntireDataset)
=STDEVPA(AllObservations)
If missing="N/A" should be 0: =STDEVPA(Data)
```

**Business Application:** Census data, complete inventories, exhaustive surveys.

**Technical Details:** If text codes represent zero values (not missing data), STDEVPA's conversion is meaningful. If text means "unknown," excluding via STDEV.P may be better.

---

### [[Organizational Complete Analysis]]

**Scenario:** Analyze full organizational data with mixed types.

**Implementation:**
```
=STDEVPA(AllDepartmentData)
=STDEVPA(CompleteStatusColumn)
Statistics: Mean=AVERAGEA, SD=STDEVPA
```

**Business Application:** Complete organizational metrics where all records are analyzed.

**Technical Details:** Organizations often have complete data (all employees, all products). STDEVPA is appropriate when that data includes text/logical values that should convert to 0/1.

---

### [[Complete Quality Records]]

**Scenario:** Analyze all quality records with pass/fail status.

**Implementation:**
```
=STDEVPA(QualityResults)
=STDEVPA(InspectionOutcomes)  → TRUE=pass=1, FALSE=fail=0
=SQRT(AVERAGEA(Results)*(1-AVERAGEA(Results)))  → Binary SD formula
```

**Business Application:** Complete quality inspection records, 100% inspection scenarios.

**Technical Details:** For binary pass/fail data, STDEVPA = sqrt(p*(1-p)) where p is proportion passing. Population formula since all items inspected.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions
- **Conversion rules:** TRUE=1, FALSE=0, text=0
- **Empty cells:** Typically ignored in ranges
- **Population formula:** Uses n denominator

### Google Sheets
- **Availability:** All versions
- **Conversion rules:** Identical to Excel
- **Behavior:** Same calculation and conversions
- **Performance:** Efficient for large datasets

### Both Platforms
- Identical mathematical calculation
- Same conversion rules for text/logical
- Same error conditions
- Minimum 1 data point (after conversions)

## Tips and Best Practices

1. **Know when to use STDEVPA:** Very specific: population data (all items) + text/logical that should convert to 0/1. Otherwise, simpler functions suffice.

2. **Consider if conversion is meaningful:** Does FALSE really mean 0? Does "N/A" mean 0? If not, STDEV.P (ignoring non-numeric) may be more appropriate.

3. **Watch text in positive data:** Text becoming 0 can dramatically increase SD when other values are positive. Verify this is desired behavior.

4. **Match with other functions:** If using STDEVPA, consider using AVERAGEA and VARPA for consistent handling.

5. **Rare in practice:** STDEVPA is one of the least common statistical functions. Most scenarios call for STDEV.S or STDEV.P.

6. **Document usage:** If using STDEVPA, document why—it's unusual enough that others may question the choice.

7. **Compare all four:** Test your data with STDEV.S, STDEVA, STDEV.P, and STDEVPA to understand which is appropriate.

8. **Relationship to VARPA:** STDEVPA = sqrt(VARPA). Use whichever measure your analysis requires.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[STDEV.P]] | Population SD, ignores text/logical | When text/logical should be excluded |
| [[STDEVA]] | Sample SD with text/logical conversion | For sample data with text/logical |
| [[STDEV.S]] | Sample SD, ignores text/logical | For sample data without text/logical |
| [[VARPA]] | Population variance with text/logical | When you need variance, not SD |

### Commonly Used Together

**[[AVERAGEA]]** - Consistent mean

*Population statistics with conversions:*
```
Mean: =AVERAGEA(Data)
SD:   =STDEVPA(Data)
```
Both use same text/logical conversion.

---

**[[VARPA]]** - Population variance

*SD-variance relationship:*
```
=STDEVPA(Data)^2  → Should equal VARPA(Data)
=SQRT(VARPA(Data)) → Should equal STDEVPA(Data)
```
Related by squaring/square root.

---

**[[STDEV.P]]** - Compare for data quality

*Detect non-numeric content:*
```
=IF(STDEVPA(Data)=STDEV.P(Data), "Numeric only", "Has text/logical")
```
Different results indicate text/logical in data.

---

**[[MAXA]]** and **[[MINA]]** - Complete statistics

*Full descriptive statistics:*
```
Mean: =AVERAGEA(Data)
SD:   =STDEVPA(Data)
Max:  =MAXA(Data)
Min:  =MINA(Data)
```
All "A" functions for consistency.

---

**[[COUNTA]]** - Understand data composition

*Count all non-empty cells:*
```
=COUNTA(Range) vs =COUNT(Range)
```
Difference reveals text/logical count.

## Official Documentation

- **Microsoft Excel:** [STDEVPA function](https://support.microsoft.com/en-us/office/stdevpa-function-5578d4d6-d3db-4e5b-bb42-b8503651c36a)
- **Google Sheets:** [STDEVPA function](https://support.google.com/docs/answer/3094088)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original implementation |
| Excel 2007+ | All subsequent | No changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
