---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- database
subTopics:
- criteria-range
- conditional-product
- multiplication
- data-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-product
- conditional-multiplication
tags:
- database-functions
- product
- criteria-based
- multiplication
---

# DPRODUCT

## Description

DPRODUCT multiplies all values in a database column among records that match specified criteria. While most database functions aggregate by summing or averaging, DPRODUCT performs multiplication, which is useful for compound calculations like interest factors, probability products, or scaling factors. For records matching the criteria, all numeric values in the specified field are multiplied together.

The **criteria range** functions identically to other database functions. Create a separate range with column headers matching your database exactly, then place filter conditions below those headers. Same-row criteria combine with AND logic (both must be true), while different-row criteria use OR logic (either can be true). This enables filtered multiplication like "multiply all conversion rates for (Marketing campaigns in Q1) OR (Sales campaigns in Q2)."

DPRODUCT is less commonly used than DSUM or DAVERAGE because multiplication aggregation has fewer business applications. However, it is essential for compound growth calculations, probability computations, and scenarios where sequential factors must be combined. For example, calculating a final price after multiple percentage adjustments, or computing the probability of independent events all occurring.

Database functions organize data with column headers in the first row and records below. The database range must include headers. The field parameter specifies which column to multiply, accepting a quoted header name, column number (starting at 1), or cell reference. DPRODUCT only uses numeric values; text and empty cells in the field column are ignored. Be aware that including zeros in your data will result in a product of zero.

## Syntax

> [!NOTE] Syntax
> ```
> DPRODUCT(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column to multiply, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which records to include | Yes |

## Examples

### Example 1: Basic Multiplication with Single Criterion

**Database (A1:D6):**
| Stage | Category | Factor | Description |
|-------|----------|--------|-------------|
| 1 | Process A | 1.05 | Initial markup |
| 2 | Process A | 1.10 | Quality adjustment |
| 3 | Process A | 0.95 | Efficiency factor |
| 4 | Process B | 1.08 | Initial markup |
| 5 | Process B | 1.12 | Quality adjustment |

**Criteria (F1:F2):**
| Category |
|----------|
| Process A |

**Formula:**
```
=DPRODUCT(A1:D6, "Factor", F1:F2)
```

**Result:** `1.09725`

**Explanation:** Multiplies all factors for Process A: 1.05 * 1.10 * 0.95 = 1.09725. This represents a cumulative 9.725% increase.

---

### Example 2: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Process B |

**Formula:**
```
=DPRODUCT(A1:D6, 3, F1:F2)
```

**Result:** `1.2096`

**Explanation:** Column 3 is Factor. Multiplies Process B factors: 1.08 * 1.12 = 1.2096, representing a 20.96% cumulative increase.

---

### Example 3: Compound Interest Factors

**Database (A1:D6):**
| Year | Account | Growth Factor | Balance |
|------|---------|---------------|---------|
| 2020 | Savings | 1.02 | 10000 |
| 2021 | Savings | 1.025 | 10200 |
| 2022 | Savings | 1.03 | 10455 |
| 2023 | Savings | 1.028 | 10769 |
| 2024 | Savings | 1.032 | 11070 |

**Criteria (F1:F2):**
| Account |
|---------|
| Savings |

**Formula:**
```
=DPRODUCT(A1:D6, "Growth Factor", F1:F2)
```

**Result:** `1.1406`

**Explanation:** Multiplies all annual growth factors: 1.02 * 1.025 * 1.03 * 1.028 * 1.032 = 1.1406. Total growth over 5 years is 14.06%.

---

### Example 4: Multiple AND Criteria

**Database (A1:E7):**
| Step | Process | Type | Factor | Sequence |
|------|---------|------|--------|----------|
| 1 | Assembly | Quality | 1.05 | 1 |
| 2 | Assembly | Quality | 1.02 | 2 |
| 3 | Assembly | Speed | 0.98 | 1 |
| 4 | Finishing | Quality | 1.08 | 1 |
| 5 | Finishing | Quality | 1.03 | 2 |
| 6 | Finishing | Speed | 0.95 | 1 |

**Criteria (G1:H2):**
| Process | Type |
|---------|------|
| Assembly | Quality |

**Formula:**
```
=DPRODUCT(A1:E7, "Factor", G1:H2)
```

**Result:** `1.071`

**Explanation:** Multiplies factors for Assembly AND Quality: 1.05 * 1.02 = 1.071.

---

### Example 5: OR Criteria (Multiple Rows)

**Database (A1:E7):** Same as Example 4

**Criteria (G1:G3):**
| Type |
|------|
| Quality |
| Speed |

**Formula:**
```
=DPRODUCT(A1:E7, "Factor", G1:G3)
```

**Result:** `1.0745`

**Explanation:** Multiplies all Quality OR Speed factors: 1.05 * 1.02 * 0.98 * 1.08 * 1.03 * 0.95 = 1.0745.

---

### Example 6: Probability Calculation

**Database (A1:D6):**
| Event | Category | Probability | Description |
|-------|----------|-------------|-------------|
| A | Required | 0.95 | Component A works |
| B | Required | 0.92 | Component B works |
| C | Required | 0.98 | Component C works |
| D | Optional | 0.85 | Component D works |
| E | Optional | 0.90 | Component E works |

**Criteria (F1:F2):**
| Category |
|----------|
| Required |

**Formula:**
```
=DPRODUCT(A1:D6, "Probability", F1:F2)
```

**Result:** `0.85652`

**Explanation:** Multiplies probabilities for all required components: 0.95 * 0.92 * 0.98 = 0.85652 (85.65% chance all required components work).

---

### Example 7: Discount Chain Calculation

**Database (A1:D6):**
| Order | Discount Type | Multiplier | Applied |
|-------|---------------|------------|---------|
| 1 | Volume | 0.90 | Yes |
| 2 | Loyalty | 0.95 | Yes |
| 3 | Seasonal | 0.85 | No |
| 4 | Promo | 0.92 | Yes |
| 5 | Early Bird | 0.97 | No |

**Criteria (F1:F2):**
| Applied |
|---------|
| Yes |

**Formula:**
```
=DPRODUCT(A1:D6, "Multiplier", F1:F2)
```

**Result:** `0.7866`

**Explanation:** Multiplies applied discount multipliers: 0.90 * 0.95 * 0.92 = 0.7866. Final price is 78.66% of original (21.34% total discount).

---

### Example 8: Manufacturing Yield Calculation

**Database (A1:D7):**
| Stage | Line | Yield Rate | Units In |
|-------|------|------------|----------|
| Cutting | A | 0.98 | 1000 |
| Forming | A | 0.95 | 980 |
| Assembly | A | 0.92 | 931 |
| Cutting | B | 0.97 | 1000 |
| Forming | B | 0.93 | 970 |
| Assembly | B | 0.90 | 902 |

**Criteria (F1:F2):**
| Line |
|------|
| A |

**Formula:**
```
=DPRODUCT(A1:D7, "Yield Rate", F1:F2)
```

**Result:** `0.8567`

**Explanation:** Multiplies yield rates for Line A: 0.98 * 0.95 * 0.92 = 0.8567. Starting with 1000 units, expect 857 to complete all stages.

---

### Example 9: Currency Conversion Chain

**Database (A1:D5):**
| Step | From | Rate | To |
|------|------|------|-----|
| 1 | USD | 0.85 | EUR |
| 2 | EUR | 1.08 | GBP |
| 3 | GBP | 156 | JPY |
| 4 | JPY | 0.0064 | USD |

**Criteria (F1:F2):**
| Step |
|------|
|  |

**Formula:**
```
=DPRODUCT(A1:D5, "Rate", F1:F2)
```

**Result:** `0.9157`

**Explanation:** Empty criteria matches all records. Multiplies all conversion rates: 0.85 * 1.08 * 156 * 0.0064 = 0.9157. This shows the round-trip conversion factor.

---

### Example 10: Comparison with Criteria

**Database (A1:D7):**
| Period | Type | Growth | Revenue |
|--------|------|--------|---------|
| Q1 | Organic | 1.05 | 50000 |
| Q2 | Organic | 1.08 | 52500 |
| Q3 | Organic | 1.12 | 56700 |
| Q4 | Organic | 1.06 | 63504 |
| Q1 | Paid | 1.15 | 30000 |
| Q2 | Paid | 1.20 | 34500 |

**Criteria (F1:G2):**
| Type | Growth |
|------|--------|
| Organic | >1.05 |

**Formula:**
```
=DPRODUCT(A1:D7, "Growth", F1:G2)
```

**Result:** `1.2902`

**Explanation:** Multiplies Organic growth factors greater than 1.05: 1.08 * 1.12 * 1.06 = 1.2902 (29.02% cumulative growth).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Field parameter does not match any column header | Verify field name matches exactly |
| 0 | One or more matching values is 0 | Check data for zero values; zeros make entire product 0 |
| 1 | No records match criteria | Verify criteria; empty match set returns 1 (identity for multiplication) |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Factor" not Factor |
| #NUM! | Column number outside valid range | Use number between 1 and total columns |
| Unexpected result | Negative values causing sign changes | Review data for negative numbers affecting the product |
| #REF! | Invalid range reference | Verify all ranges point to existing cells |

## Use Cases

### Compound Growth Analysis

**Scenario:** An investment analyst needs to calculate cumulative returns over multiple periods, where each period's growth factor multiplies the previous balance.

**Implementation:** Build a returns database with Period, Investment, Growth Factor, and Value columns. Use DPRODUCT to calculate total compound growth for specific investments or time ranges.

**Technical Details:** For an investment with annual growth factors of 1.08, 1.05, and 1.10, the formula `=DPRODUCT(Returns, "Growth Factor", criteria)` returns 1.2474 (24.74% total growth). This is more accurate than adding percentages. Criteria can filter specific investment types or date ranges.

---

### Reliability Engineering

**Scenario:** A systems engineer needs to calculate the overall reliability of a system based on the individual reliability of components, where all components must function for the system to work.

**Implementation:** Create a component database with Component ID, System, Reliability, and Category columns. DPRODUCT calculates the probability that all components in a filtered set function correctly.

**Technical Details:** For series systems, overall reliability equals the product of component reliabilities. `=DPRODUCT(Components, "Reliability", criteria)` gives the system reliability. Use criteria to analyze subsystems: "Power components" OR "Control components." Results guide maintenance priorities and redundancy planning.

---

### Pricing Chain Calculation

**Scenario:** A pricing analyst needs to calculate final prices after multiple sequential adjustments (markups, discounts, taxes) are applied to a base price.

**Implementation:** Structure an adjustments database with Sequence, Type, Factor, and Status columns. DPRODUCT multiplies all applicable factors.

**Technical Details:** A base price of $100 with factors 1.20 (markup), 0.90 (discount), and 1.08 (tax) yields `=100 * DPRODUCT(Adjustments, "Factor", ActiveCriteria)` = $116.64. The criteria range filters for active adjustments only. Change criteria to compare scenarios with different adjustment combinations.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Wildcard characters (*, ?) | Supported in text criteria | Supported in text criteria |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Handles zeros | Returns 0 | Returns 0 |
| Empty match result | Returns 1 | Returns 1 |
| Negative number handling | Follows multiplication rules | Follows multiplication rules |
| Structured table references | Supported | Not supported |
| Named ranges | Fully supported | Fully supported |
| Maximum rows | 1,048,576 | ~10,000,000 cells |
| Precision | Up to 15 significant digits | Up to 15 significant digits |

**Key Difference:** Both platforms return 1 when no records match (the multiplicative identity), unlike DSUM which returns 0. This can indicate criteria issues if unexpected.

## Tips and Best Practices

1. **Watch for zeros**: A single zero in your data makes the entire product zero. Before using DPRODUCT, verify your data does not contain zeros unless that is the intended behavior. Use criteria to exclude zero values if needed.

2. **Understand the identity return**: DPRODUCT returns 1 when no records match (not 0 or error). This is mathematically correct (1 is the identity for multiplication) but can mask criteria problems. Validate with DCOUNT.

3. **Use for compound calculations**: DPRODUCT excels at compound growth, sequential factors, and probability products. If you are adding values, use DSUM instead.

4. **Mind negative numbers**: Negative values follow normal multiplication rules. An odd count of negative numbers produces a negative product; even count produces positive. Review data for unexpected negatives.

5. **Express percentages as factors**: For calculations like compound interest or sequential discounts, express rates as factors (1.05 for 5% increase, 0.95 for 5% decrease) rather than percentages.

6. **Verify with manual calculation**: Because DPRODUCT is less intuitive than DSUM, verify results by manually multiplying a few values from matching records to ensure the formula works as expected.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[PRODUCT]] | Multiplies values in a range | No filtering capability |
| [[DSUM]] | Sums values matching criteria | Adds instead of multiplies |
| [[DAVERAGE]] | Averages values matching criteria | Returns mean, not product |
| [[DMAX]] | Maximum matching criteria | Returns single largest value |
| [[DMIN]] | Minimum matching criteria | Returns single smallest value |
| [[POWER]] | Raises number to a power | Different mathematical operation |

### Commonly Used With

**[[DCOUNT]]** - Verify records match before multiplication:
```
=IF(DCOUNT(database, "Factor", criteria)>0, DPRODUCT(database, "Factor", criteria), "No factors")
```
Distinguishes "no matches" (returns 1) from actual products.

**[[LOG]]** - Convert multiplicative to additive analysis:
```
Sum of logs: =DSUM(database, "LogFactor", criteria)
Equivalent product: =10^DSUM(...)
```
Useful for very large products that might overflow.

**[[DSUM]]** - Compare additive vs multiplicative aggregation:
```
Product of factors: =DPRODUCT(database, "Factor", criteria)
Sum of percentages: =DSUM(database, "Percent", criteria)
```
Choose based on mathematical relationship in your data.

## Official Documentation

- [Microsoft Excel DPRODUCT Documentation](https://support.microsoft.com/en-us/office/dproduct-function-4f96b13e-d49c-47a7-b769-22f6d017cb31)
- [Google Sheets DPRODUCT Documentation](https://support.google.com/docs/answer/3094147)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Improved numeric precision |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | Performance optimizations |
| Excel 365 | Excel | Continued support without changes |
| Initial release | Google Sheets | Full compatibility with Excel behavior |
| 2019 | Google Sheets | Performance improvements |
