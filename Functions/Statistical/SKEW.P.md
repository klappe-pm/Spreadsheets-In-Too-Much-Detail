---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- skewness
- population-statistics
- distribution-shape
- asymmetry
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Population Skewness
- SKEWP
- True Skewness
tags:
- skewness
- population
- distribution
- asymmetry
- statistics
- shape
---

# SKEW.P

## Description

**SKEW.P** calculates the population skewness of a distribution—a measure of asymmetry when your data represents the entire population rather than a sample. Unlike SKEW (which uses a bias-corrected sample formula), SKEW.P computes the true population skewness using the straightforward third standardized moment without sample correction.

Use SKEW.P when your data represents a **complete population**, not a sample drawn from a larger population. Examples include: all employees in a company, all students in a class, all transactions in a specific period, or any scenario where you have exhaustive data for the group of interest. If your data is a sample intended to estimate population characteristics, use SKEW instead.

The difference between SKEW and SKEW.P is similar to the difference between STDEV.S and STDEV.P: the sample version applies a correction factor that adjusts for the fact that samples tend to underestimate population variability. For skewness, this correction involves factors of n, n-1, and n-2 in the denominator.

**SKEW.P requires at least 3 data points.** Like SKEW, population skewness is undefined with fewer than 3 values. For large datasets, SKEW and SKEW.P converge to nearly identical values—the correction factor becomes negligible as n increases.

## Syntax

> [!f(x)] SKEW.P Syntax
>
> ```
> =SKEW.P(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | First number, cell reference, or range representing population data. |
| `number2, ...` | No | Additional numbers, references, or ranges. Up to 255 arguments. Minimum 3 total data points required. |

### Return Value

Returns population skewness as a single number. Zero indicates symmetry. Positive indicates right skew (right tail longer). Negative indicates left skew (left tail longer). Returns #DIV/0! if fewer than 3 data points or zero variance.

## Examples

> [!f(x)] SKEW.P Examples

### Example 1: Symmetric Population
```
=SKEW.P(1, 2, 3, 4, 5)
```
**Result:** 0

**Explanation:** Symmetric data has zero skewness regardless of sample/population distinction.

---

### Example 2: Comparing SKEW vs SKEW.P
```
=SKEW({1, 2, 2, 3, 3, 3, 10})
=SKEW.P({1, 2, 2, 3, 3, 3, 10})
```
**Result:** SKEW returns larger absolute value than SKEW.P

**Explanation:** Sample correction inflates SKEW estimate. SKEW.P gives "raw" population value.

---

### Example 3: All Employees' Tenure
```
=SKEW.P(EmployeeTenure)
```
**Result:** Population skewness of tenure distribution

**Explanation:** If data includes ALL employees, use SKEW.P. Typically positive skew—many new employees, few long-tenured.

---

### Example 4: Complete Transaction Set
```
=SKEW.P(AllTransactions)
```
**Result:** True skewness of transaction amounts

**Explanation:** All transactions in a period = complete population. SKEW.P gives exact skewness.

---

### Example 5: Test Scores (Entire Class)
```
=SKEW.P(ClassScores)
```
**Result:** Population skewness of score distribution

**Explanation:** Entire class = population, not sample. Use SKEW.P for accurate characterization.

---

### Example 6: Small Population Effect
```
n=10: SKEW={data}  → Noticeably different from SKEW.P
n=100: SKEW≈SKEW.P → Correction factor diminishes
```
**Result:** Difference shrinks with larger n

**Explanation:** For small populations, the choice matters. For large populations, results converge.

---

### Example 7: Cell Range
```
=SKEW.P(A1:A500)
```
**Result:** Skewness of all values in range

**Explanation:** Standard usage with cell reference for population data.

---

### Example 8: Multiple Arguments
```
=SKEW.P(DeptA, DeptB, DeptC)
```
**Result:** Combined skewness of all departments

**Explanation:** Multiple ranges combined into single population for skewness calculation.

---

### Example 9: Right-Skewed Population
```
=SKEW.P(SalaryData)
```
**Result:** Positive (typically)

**Explanation:** Salary distributions typically show positive population skewness.

---

### Example 10: Left-Skewed Population
```
=SKEW.P(LifespanData)
```
**Result:** Negative (often)

**Explanation:** Human lifespan data often shows negative skew—most live to old age, few die young.

---

### Example 11: Inventory Turn Rates
```
=SKEW.P(TurnoverRates)
```
**Result:** Population skewness of all SKUs

**Explanation:** If analyzing all products, use population skewness.

---

### Example 12: When to Choose SKEW.P
```
Complete census data → SKEW.P
Survey sample → SKEW
All transactions → SKEW.P
Random transaction sample → SKEW
```
**Result:** Decision guide

**Explanation:** Choose based on whether data is exhaustive (population) or partial (sample).

---

### Example 13: Numerical Comparison
```
Data: {1, 2, 3, 4, 5, 6, 7, 8, 9, 100}
SKEW:   =SKEW(data)   → approximately 2.94
SKEW.P: =SKEW.P(data) → approximately 2.79
```
**Result:** ~5% difference with n=10

**Explanation:** Sample formula inflates estimate to compensate for sampling variability.

---

### Example 14: Verifying with Manual Formula
```
=SKEW.P(data) vs =AVERAGE(((data-AVERAGE(data))/STDEV.P(data))^3)
```
**Result:** Should match

**Explanation:** SKEW.P equals average of cubed z-scores using population standard deviation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#DIV/0!` | Fewer than 3 data points | Population skewness requires minimum 3 values. |
| `#DIV/0!` | Zero variance | All identical values produce undefined skewness. |
| `#VALUE!` | Non-numeric data | Ensure all values are numbers. |
| `#NAME?` | Excel 2010 or earlier | SKEW.P added in Excel 2013. Use manual formula for older versions. |
| `Wrong function choice` | Sample vs population confusion | Use SKEW for samples, SKEW.P for populations. |

## Use Cases

### [[Complete Dataset Analysis]]

**Scenario:** Analyze skewness when you have all data points for a population.

**Implementation:**
```
=SKEW.P(AllCustomers)
=SKEW.P(EntireInventory)
=SKEW.P(CompleteCensus)
```

**Business Application:** HR analytics (all employees), inventory analysis (all SKUs), financial reporting (all transactions).

**Technical Details:** When data is exhaustive, SKEW.P gives exact population parameter. No estimation uncertainty—this IS the population skewness.

---

### [[Organizational Metrics]]

**Scenario:** Calculate skewness for complete organizational data.

**Implementation:**
```
=SKEW.P(AllEmployeeSalaries)
=SKEW.P(AllDepartmentBudgets)
=SKEW.P(AllProductMargins)
```

**Business Application:** Compensation analysis, budget planning, product portfolio analysis.

**Technical Details:** Organizations often have complete data about their own operations. Use SKEW.P for internal analysis.

---

### [[Academic Performance Analysis]]

**Scenario:** Analyze grade distributions for entire classes or courses.

**Implementation:**
```
=SKEW.P(FinalGrades)
=SKEW.P(ExamScores)
=IF(SKEW.P(Grades)<-0.5, "Ceiling effect", IF(SKEW.P(Grades)>0.5, "Floor effect", "Balanced"))
```

**Business Application:** Educational assessment, curriculum evaluation, grading policy analysis.

**Technical Details:** A class's grades represent the complete population of that class. Skewness reveals if assessments are too easy (negative skew) or too hard (positive skew).

---

### [[Complete Period Analysis]]

**Scenario:** Analyze all events within a specific time period.

**Implementation:**
```
=SKEW.P(January_Sales)
=SKEW.P(Q4_Transactions)
=SKEW.P(Annual_Returns)
```

**Business Application:** Period-specific reporting, seasonal analysis, year-end reviews.

**Technical Details:** All transactions in a period = complete population for that period. SKEW.P is appropriate.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later only
- **Not available:** Excel 2010 and earlier
- **Formula alternative:** `=AVERAGE(((data-AVERAGE(data))/STDEV.P(data))^3)`
- **Precision:** Full floating-point precision

### Google Sheets
- **Availability:** All versions
- **Identical to Excel:** Same calculation
- **Performance:** Efficient for large populations

### Both Platforms
- Identical mathematical formula
- Same minimum data requirement (n >= 3)
- Same error conditions
- Converges to SKEW for large n

### Formula for Excel 2010 and Earlier
```
=AVERAGE(POWER((range-AVERAGE(range))/STDEV.P(range),3))
```
Enter as array formula with Ctrl+Shift+Enter.

## Tips and Best Practices

1. **Know the difference from SKEW:** SKEW.P is for populations (all data), SKEW is for samples (partial data). The mathematical formulas differ.

2. **Consider data scope:** Ask "Do I have ALL the data, or a sample?" If all, use SKEW.P.

3. **Large n makes choice less critical:** For n > 100, SKEW and SKEW.P produce nearly identical results.

4. **Small populations: SKEW.P is exact:** For complete small populations, SKEW.P gives the true skewness without estimation error.

5. **Interpret identically to SKEW:** Positive = right-skewed, Negative = left-skewed, Zero = symmetric. Interpretation doesn't change.

6. **Combine with other population measures:** Use STDEV.P and VAR.P alongside SKEW.P for consistent population statistics.

7. **Manual verification:** SKEW.P = mean of (z-scores)^3 where z-scores use population standard deviation.

8. **For inferential statistics:** If you're testing hypotheses about a larger population, use SKEW even if you have "all" your collected data.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SKEW]] | Sample skewness | When data is a sample, not complete population |
| [[KURT]] | Sample kurtosis | When concerned about tail behavior |
| [[STDEV.P]] | Population standard deviation | Paired measure of population spread |
| [[VAR.P]] | Population variance | Paired measure of population dispersion |

### Commonly Used Together

**[[SKEW]]** - Compare sample vs population

*Understand the difference:*
```
Sample:     =SKEW(Data)
Population: =SKEW.P(Data)
```
For large n, results converge. For small n, difference is meaningful.

---

**[[STDEV.P]]** - Consistent population measures

*Complete population statistics:*
```
Mean:     =AVERAGE(Data)
Std Dev:  =STDEV.P(Data)    → Population SD
Skewness: =SKEW.P(Data)     → Population skewness
```
Use all population versions together for consistency.

---

**[[VAR.P]]** - Population variance

*Population dispersion measures:*
```
Variance: =VAR.P(Data)
Std Dev:  =STDEV.P(Data)
Skewness: =SKEW.P(Data)
```
All three use population (n) denominators.

---

**[[KURT]]** - Distribution shape

*Complete shape analysis:*
```
Skewness: =SKEW.P(Data)  → Asymmetry
Kurtosis: =KURT(Data)    → Tail weight
```
Note: Excel doesn't have KURT.P; KURT uses sample formula.

---

**[[AVERAGE]]** - Central tendency

*Full descriptive statistics:*
```
=AVERAGE(Data)   → Mean
=STDEV.P(Data)   → Population SD
=SKEW.P(Data)    → Population skewness
```
Combined characterization of population.

## Official Documentation

- **Microsoft Excel:** [SKEW.P function](https://support.microsoft.com/en-us/office/skew-p-function-76530a5c-99b9-48a1-8392-26632d542fcb)
- **Google Sheets:** [SKEW function](https://support.google.com/docs/answer/3094085) (covers both SKEW and population concepts)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New function for population skewness |
| Excel 2016+ | All subsequent | No changes |
| Google Sheets | Recent versions | Added for Excel compatibility |
| LibreOffice Calc | Recent versions | Compatible implementation |

---

*Last updated: 2026-01-10*
