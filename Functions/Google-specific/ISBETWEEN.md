---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - logical-functions
subTopics:
  - range-checking
  - boundary-testing
  - value-validation
  - conditional-logic
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-between
  - range-check
  - within-range
  - bound-check
tags:
  - google-sheets-only
  - logical-functions
  - validation
  - conditional
  - range-checking
---

# ISBETWEEN

## Description

The **ISBETWEEN function** is a Google Sheets-exclusive function that checks whether a value falls within a specified range, returning TRUE if the value is between the lower and upper bounds and FALSE otherwise. This logical function simplifies range comparisons that would otherwise require compound AND statements, making formulas more readable and maintainable. ISBETWEEN is perfect for grade assignments, data validation, tiered pricing, commission calculations, and any scenario requiring boundary checking.

The function accepts a value to test, lower and upper bounds, and optional parameters controlling whether the bounds themselves are inclusive or exclusive. By default, both bounds are inclusive, meaning a value equal to either bound returns TRUE. However, you can specify exclusive bounds where values must be strictly between the limits—not equal to them. This flexibility handles both "between 1 and 10 inclusive" and "greater than 1 and less than 10" scenarios in a single function.

ISBETWEEN works with numbers, dates, times, and text (using alphabetical comparison for text). For dates, this enables elegant date range checking: `ISBETWEEN(A1, DATE(2024,1,1), DATE(2024,12,31))` checks if a date falls within 2024. For text, alphabetical ordering determines the result, so "B" is between "A" and "C". The function properly handles cases where bounds are reversed (lower > upper), returning appropriate results.

ISBETWEEN is exclusively available in Google Sheets and has no direct equivalent in Microsoft Excel. Excel users must use compound AND statements: `=AND(A1>=lower, A1<=upper)` to achieve the same result. While functional, this approach is more verbose and doesn't offer the inclusive/exclusive bound options without additional complexity. ISBETWEEN provides a cleaner, more intuitive syntax for range checking operations.

## Syntax

> [!f(x)] ISBETWEEN Syntax
>
> ```
> =ISBETWEEN(value_to_compare, lower_value, upper_value, [lower_value_is_inclusive], [upper_value_is_inclusive])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value_to_compare` | Yes | The value to test against the range. Can be a number, date, time, text, or cell reference. This value will be compared to determine if it falls within bounds. |
| `lower_value` | Yes | The lower bound of the range. The value will be compared to this minimum. Can be a number, date, time, text, or cell reference. |
| `upper_value` | Yes | The upper bound of the range. The value will be compared to this maximum. Can be a number, date, time, text, or cell reference. |
| `lower_value_is_inclusive` | No | TRUE (default) means lower_value <= value is checked. FALSE means lower_value < value (exclusive). Controls whether values exactly equal to lower bound are included. |
| `upper_value_is_inclusive` | No | TRUE (default) means value <= upper_value is checked. FALSE means value < upper_value (exclusive). Controls whether values exactly equal to upper bound are included. |

### Return Value

Returns TRUE if the value falls within the specified range (considering inclusivity settings), FALSE otherwise. If any parameter is an error value, returns that error. If bounds are reversed (lower > upper), the function still works correctly by internally swapping them.

## Examples

> [!f(x)] ISBETWEEN Examples

### Example 1: Basic Numeric Range Check
```
=ISBETWEEN(50, 1, 100)
```
**Scenario:** Check if 50 is between 1 and 100
**Result:** TRUE

**Explanation:** 50 falls within the range 1-100 inclusive, so the function returns TRUE. Both bounds are inclusive by default.

---

### Example 2: Value Outside Range
```
=ISBETWEEN(150, 1, 100)
```
**Scenario:** Check if 150 is between 1 and 100
**Result:** FALSE

**Explanation:** 150 exceeds the upper bound of 100, so the function returns FALSE.

---

### Example 3: Value Equal to Bound (Inclusive)
```
=ISBETWEEN(100, 1, 100)
```
**Scenario:** Check if 100 is between 1 and 100 (inclusive)
**Result:** TRUE

**Explanation:** With default inclusive bounds, a value equal to either bound returns TRUE. 100 equals the upper bound.

---

### Example 4: Value Equal to Bound (Exclusive)
```
=ISBETWEEN(100, 1, 100, TRUE, FALSE)
```
**Scenario:** Check if 100 is between 1 and 100, with exclusive upper bound
**Result:** FALSE

**Explanation:** With upper_value_is_inclusive=FALSE, 100 is NOT considered between 1 and 100 because the upper bound is exclusive.

---

### Example 5: Date Range Check
```
=ISBETWEEN(A1, DATE(2024,1,1), DATE(2024,12,31))
```
**Scenario:** A1 contains 2024-06-15; check if it's within 2024
**Result:** TRUE

**Explanation:** Dates work naturally with ISBETWEEN. June 15, 2024 falls within January 1 to December 31, 2024.

---

### Example 6: Cell Reference for All Parameters
```
=ISBETWEEN(A1, B1, C1)
```
**Scenario:** A1=75, B1=60, C1=90; check if A1 is between B1 and C1
**Result:** TRUE

**Explanation:** All parameters can be cell references, enabling dynamic range checking where bounds change based on other data.

---

### Example 7: Text Alphabetical Range
```
=ISBETWEEN("M", "A", "Z")
```
**Scenario:** Check if "M" is alphabetically between "A" and "Z"
**Result:** TRUE

**Explanation:** Text comparison uses alphabetical order. "M" comes after "A" and before "Z" alphabetically.

---

### Example 8: Time Range Check
```
=ISBETWEEN(A1, TIME(9,0,0), TIME(17,0,0))
```
**Scenario:** A1 contains 10:30 AM; check if it's within business hours (9 AM - 5 PM)
**Result:** TRUE

**Explanation:** Time values work with ISBETWEEN for scheduling and time-based logic.

---

### Example 9: Exclusive Both Bounds
```
=ISBETWEEN(5, 5, 10, FALSE, FALSE)
```
**Scenario:** Check if 5 is strictly between 5 and 10 (exclusive both ends)
**Result:** FALSE

**Explanation:** With both bounds exclusive, 5 is NOT between 5 and 10 because it equals the lower bound.

---

### Example 10: Grade Assignment
```
=IF(ISBETWEEN(A1, 90, 100), "A", IF(ISBETWEEN(A1, 80, 89), "B", IF(ISBETWEEN(A1, 70, 79), "C", "Below C")))
```
**Scenario:** Assign letter grades based on numeric score in A1
**Result:** "A" for 90-100, "B" for 80-89, etc.

**Explanation:** ISBETWEEN within IF creates tiered classifications. Note: for continuous scales, adjust bounds to avoid gaps or overlaps.

---

### Example 11: With FILTER for Range Filtering
```
=FILTER(A:B, ISBETWEEN(B:B, 100, 500))
```
**Scenario:** Filter rows where column B values are between 100 and 500
**Result:** Only rows where column B is within range

**Explanation:** ISBETWEEN works excellently as a FILTER condition, replacing compound AND statements.

---

### Example 12: Reversed Bounds (Function Handles It)
```
=ISBETWEEN(50, 100, 1)
```
**Scenario:** Bounds provided in wrong order (100 before 1)
**Result:** TRUE (function internally corrects)

**Explanation:** ISBETWEEN intelligently handles reversed bounds, treating them as if correctly ordered.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-comparable data types | Ensure all parameters are compatible types (all numbers, all dates, etc.) |
| `#REF!` | Referenced cell was deleted | Restore cell or update reference |
| `Unexpected FALSE` | Forgetting about exclusive bounds | Check inclusivity parameters; default is inclusive |
| `Type mismatch` | Mixing dates with text representations | Use DATE() function for date bounds; ensure consistent types |
| `FALSE for boundary value` | Exclusive bound setting | Change inclusive parameter to TRUE if boundary values should match |
| `Error propagation` | Input cell contains error | Handle errors with IFERROR before ISBETWEEN |
| `Text comparison issues` | Case sensitivity in text | Text comparison is case-insensitive; "a" and "A" are equivalent |
| `Time comparison confusion` | Time zone or format issues | Use TIME() function for consistent time bounds |

## Use Cases

### [[Grade Assignment System]]

**Scenario:** A teacher needs to convert numeric scores to letter grades automatically. The grading scale is: A (90-100), B (80-89), C (70-79), D (60-69), F (below 60). The system should handle edge cases correctly and be easy to modify if grading scales change.

**Implementation:**
```
Grading structure:
Column A: Student Name
Column B: Numeric Score
Column C: Letter Grade

Letter Grade formula (C2):
=IF(ISBETWEEN(B2, 90, 100), "A",
  IF(ISBETWEEN(B2, 80, 89.99), "B",
    IF(ISBETWEEN(B2, 70, 79.99), "C",
      IF(ISBETWEEN(B2, 60, 69.99), "D", "F"))))

Alternative with IFS:
=IFS(
  ISBETWEEN(B2, 90, 100), "A",
  ISBETWEEN(B2, 80, 89.99), "B",
  ISBETWEEN(B2, 70, 79.99), "C",
  ISBETWEEN(B2, 60, 69.99), "D",
  TRUE, "F"
)

Grade scale from reference table:
=INDEX(Grades!B:B, MATCH(TRUE, ISBETWEEN(B2, Grades!C:C, Grades!D:D), 0))

Pass/Fail check:
=IF(ISBETWEEN(B2, 60, 100), "Pass", "Fail")
```

**Business Application:** Grade conversion is a common educational task. ISBETWEEN makes the logic clearer than compound IF statements with AND conditions. Changes to grading scales require updating only the boundary values, not the formula structure.

**Technical Details:** Use 89.99 instead of 89 if scores can be decimals and you want strict boundaries. The IFS function (available in Sheets) provides cleaner syntax than nested IFs. For complex grading scales, a lookup table approach may be more maintainable.

---

### [[Tiered Pricing Calculations]]

**Scenario:** An e-commerce system uses tiered pricing: orders under $50 pay $10 shipping, orders $50-$99 pay $5 shipping, and orders $100+ get free shipping. Additionally, volume discounts apply: 1-9 items at list price, 10-24 items get 10% off, 25+ items get 20% off.

**Implementation:**
```
Pricing structure:
Column A: Order Total
Column B: Item Quantity
Column C: Shipping Cost
Column D: Volume Discount

Shipping Cost (C2):
=IF(ISBETWEEN(A2, 0, 49.99), 10,
  IF(ISBETWEEN(A2, 50, 99.99), 5, 0))

Volume Discount (D2):
=IF(ISBETWEEN(B2, 1, 9), 0,
  IF(ISBETWEEN(B2, 10, 24), 0.10, 0.20))

Combined final price:
=A2 * (1 - D2) + C2

Using exclusive bounds for clarity:
=IF(A2<50, 10, IF(A2<100, 5, 0))

Commission tiers (for sales team):
=IF(ISBETWEEN(Sales, 0, 9999), Sales*0.05,
  IF(ISBETWEEN(Sales, 10000, 49999), Sales*0.08, Sales*0.10))
```

**Business Application:** Tiered pricing is fundamental to e-commerce, subscriptions, and sales compensation. ISBETWEEN clearly expresses the range logic, making it easy to audit and modify tiers. The readable syntax reduces errors in financial calculations.

**Technical Details:** Be careful with decimal boundaries (49.99 vs 50) to prevent gaps or overlaps. For complex tiering, consider a lookup table approach. Document tier boundaries clearly for business users who may need to update them.

---

### [[Date-Based Report Filtering]]

**Scenario:** A financial reporting system needs to filter transactions by custom date ranges. Reports include: current month transactions, current quarter, fiscal year, and custom date ranges entered by users. The filtering should be dynamic and handle edge cases.

**Implementation:**
```
Transaction filtering:
Source data: Transactions!A:E (Date, Description, Amount, Category, Account)

Current month transactions:
=FILTER(Transactions!A:E,
  ISBETWEEN(Transactions!A:A,
    EOMONTH(TODAY(),-1)+1,
    EOMONTH(TODAY(),0)))

Current quarter:
=FILTER(Transactions!A:E,
  ISBETWEEN(Transactions!A:A,
    DATE(YEAR(TODAY()), CEILING(MONTH(TODAY())/3,1)*3-2, 1),
    EOMONTH(DATE(YEAR(TODAY()), CEILING(MONTH(TODAY())/3,1)*3, 1), 0)))

Custom date range (user enters in G1:H1):
=FILTER(Transactions!A:E,
  ISBETWEEN(Transactions!A:A, G1, H1))

Fiscal year (July-June):
=FILTER(Transactions!A:E,
  ISBETWEEN(Transactions!A:A,
    DATE(IF(MONTH(TODAY())<7, YEAR(TODAY())-1, YEAR(TODAY())), 7, 1),
    DATE(IF(MONTH(TODAY())<7, YEAR(TODAY()), YEAR(TODAY())+1), 6, 30)))

Last 30 days:
=FILTER(Transactions!A:E,
  ISBETWEEN(Transactions!A:A, TODAY()-30, TODAY()))
```

**Business Application:** Financial reporting requires flexible date filtering. ISBETWEEN with date functions creates dynamic filters that automatically adjust. The same formula works next month without modification, unlike hardcoded date ranges.

**Technical Details:** EOMONTH finds month boundaries. CEILING calculates quarter starts. Fiscal year logic requires conditional year calculation. All dates are inclusive by default, which is usually correct for reporting ranges.

---

### [[Data Validation and Quality Checks]]

**Scenario:** A data entry form needs to validate user inputs before submission. Age must be 18-120, salary must be $10,000-$10,000,000, dates must be within the last year, and percentages must be 0-100. Invalid entries should be flagged for correction.

**Implementation:**
```
Validation structure:
Column A: Field Name
Column B: Entered Value
Column C: Validation Status

Age validation (C2, where A2="Age"):
=IF(ISBETWEEN(B2, 18, 120), "Valid", "Invalid: Age must be 18-120")

Salary validation:
=IF(ISBETWEEN(B3, 10000, 10000000), "Valid", "Invalid: Salary out of range")

Date validation (within last year):
=IF(ISBETWEEN(B4, TODAY()-365, TODAY()), "Valid", "Invalid: Date must be within last year")

Percentage validation:
=IF(ISBETWEEN(B5, 0, 100), "Valid", "Invalid: Percentage must be 0-100")

Combined validation (all fields):
=IF(AND(
  ISBETWEEN(AgeCell, 18, 120),
  ISBETWEEN(SalaryCell, 10000, 10000000),
  ISBETWEEN(DateCell, TODAY()-365, TODAY()),
  ISBETWEEN(PctCell, 0, 100)
), "All Valid", "Errors Present")

Conditional formatting rule:
Apply red highlighting where: =NOT(ISBETWEEN(B2, ValidMin, ValidMax))
```

**Business Application:** Data quality depends on validation at entry. ISBETWEEN provides clear range checking that's easy to configure and audit. The validation messages help users understand and correct errors immediately.

**Technical Details:** Combine with conditional formatting for visual feedback. Use data validation dropdown lists for enumerated values. Consider creating a validation rules table that ISBETWEEN references for maintainability.

## Platform Differences

ISBETWEEN is **exclusively available in Google Sheets** and has no direct equivalent in Microsoft Excel.

### Google Sheets
- **Availability:** All versions since introduction
- **Inclusive/Exclusive Options:** Built-in parameters
- **Data Types:** Works with numbers, dates, times, text
- **Syntax:** Single, readable function

### Microsoft Excel
- **Native ISBETWEEN:** Does not exist
- **Alternative:** `=AND(value>=lower, value<=upper)`
- **Exclusive Bounds:** `=AND(value>lower, value<upper)` (requires different formula)
- **Mixed Bounds:** Requires combining > and >= operators manually

### Key Differences Summary

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| Single Function | Yes (ISBETWEEN) | No |
| Inclusive/Exclusive Options | Built-in parameters | Manual operator selection |
| Syntax Clarity | High | Moderate |
| Code Maintenance | Easy (one function) | Harder (compound AND) |
| Error Handling | Automatic | Manual |

### Excel Equivalent Formulas

```
Google Sheets:
=ISBETWEEN(A1, 1, 100)

Excel equivalent:
=AND(A1>=1, A1<=100)

Google Sheets (exclusive upper):
=ISBETWEEN(A1, 1, 100, TRUE, FALSE)

Excel equivalent:
=AND(A1>=1, A1<100)
```

## Tips and Best Practices

1. **Use default inclusive bounds for most cases:** Unless you specifically need exclusive bounds, the default inclusive behavior matches most business requirements and avoids gaps in ranges.

2. **Be explicit with boundary values:** For continuous data like money or percentages, consider whether 99.99 and 100.00 should both be in the same tier. Use 99.99 as upper bound or 100.01 as next tier's lower bound as needed.

3. **Combine with FILTER for powerful queries:** `=FILTER(data, ISBETWEEN(column, min, max))` is cleaner and more readable than FILTER with compound AND conditions.

4. **Document tier boundaries:** When using ISBETWEEN for business rules (pricing, commissions, grades), document the boundaries clearly for auditing and future modifications.

5. **Handle edge cases explicitly:** Test your formulas with values exactly at boundaries, slightly above, and slightly below to ensure the inclusive/exclusive settings are correct.

6. **Use cell references for dynamic bounds:** `=ISBETWEEN(A1, Settings!B1, Settings!C1)` allows changing bounds without editing formulas—useful for configurable business rules.

7. **Consider lookup tables for complex tiering:** For many tiers, a reference table with VLOOKUP or INDEX/MATCH may be cleaner than nested ISBETWEEN statements.

8. **Remember text comparison is alphabetical:** When using ISBETWEEN with text, understand that comparison is case-insensitive alphabetical ordering, not string length or other metrics.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AND]] | Logical AND of multiple conditions | When you need compound conditions beyond range checking |
| [[OR]] | Logical OR of conditions | When value can be in one of multiple ranges |
| [[IF]] | Conditional logic | For actions based on ISBETWEEN results |
| [[IFS]] | Multiple condition checking | For tiered classifications |

### Commonly Used Together

**[[IF]]** - Take action based on range check

*Assign category based on value range:*
```
=IF(ISBETWEEN(A1, 0, 50), "Low", "High")
```
Use ISBETWEEN result to determine outcome.

---

**[[FILTER]]** - Filter data by value range

*Get rows where value is in range:*
```
=FILTER(A:C, ISBETWEEN(B:B, 100, 500))
```
Clean syntax for range-based filtering.

---

**[[IFS]]** - Multiple range classifications

*Assign tiers:*
```
=IFS(ISBETWEEN(A1,0,49),"Tier 1", ISBETWEEN(A1,50,99),"Tier 2", TRUE,"Tier 3")
```
Multiple ISBETWEEN for tiered logic.

---

**[[AND]]** - Combine with other conditions

*Range check plus additional criteria:*
```
=AND(ISBETWEEN(A1, 50, 100), B1="Active")
```
Combines range check with other logical tests.

---

**[[SUMIF]] / [[COUNTIF]]** - Aggregate within range

*Count values in range:*
```
=COUNTIF(A:A, ">=50") - COUNTIF(A:A, ">100")
```
Alternative for counting/summing (ISBETWEEN can't be used directly in SUMIF criteria).

## Official Documentation

- **Google Sheets:** [ISBETWEEN function](https://support.google.com/docs/answer/10538337)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2021 | Original implementation |
| Google Sheets | 2022 | Performance optimizations |
| Google Sheets | 2023 | Enhanced error handling |

---

*Last updated: 2026-01-10*
