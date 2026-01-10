---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- day-counting
- financial-calculation
- 30-360-convention
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Days 360
- 30-360 Day Count
- Financial Day Count
tags:
- days360
- 30-360
- financial-days
- interest-calculation
- bond-math
---

# DAYS360

## Description

**DAYS360** calculates the number of days between two dates using a 360-day year (12 months of 30 days each). This convention, known as 30/360, is widely used in financial calculations for bonds, loans, mortgages, and other interest-bearing instruments. Rather than counting actual calendar days, DAYS360 treats every month as having exactly 30 days, simplifying interest calculations and providing consistency across different months.

The function offers two calculation methods controlled by the optional `method` parameter: US (NASD) method and European method. The key difference is how they handle month-end dates. The US method has specific rules for dates on the 31st and for February end dates. The European method simply converts any date on the 31st to the 30th. These differences can produce different results for certain date combinations.

**Why 30/360 matters:** Actual calendar months have 28-31 days, which complicates interest calculations. In a 30/360 system, one month always equals 30 days and one year always equals 360 days. Interest for one month is exactly 1/12 of annual interest. This standardization simplifies calculations, makes monthly payments uniform, and is mandated by many financial instrument conventions.

DAYS360 is essential for financial professionals working with bonds, mortgages, and commercial paper. It pairs naturally with YEARFRAC (which uses DAYS360 internally for basis 0 and 4) and is used in accrued interest calculations, loan amortization, and any scenario requiring the 30/360 day count convention. Understanding the difference between US and European methods is crucial for accurate financial calculations.

## Syntax

> [!f(x)] DAYS360 Syntax
>
> ```
> =DAYS360(start_date, end_date, [method])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `start_date` | Yes | The starting date of the period. |
| `end_date` | Yes | The ending date of the period. |
| `method` | No | A logical value specifying the calculation method. FALSE or omitted = US (NASD) method; TRUE = European method. |

### Method Differences (US vs European)

| Scenario | US (NASD) Method | European Method |
|----------|------------------|-----------------|
| Start date is 31st | Changed to 30th | Changed to 30th |
| End date is 31st | Changed to 30th only if start date is 30th or 31st | Always changed to 30th |
| February end (28th/29th) | If start date is last day of Feb, changed to 30 | No adjustment |
| General rule | More complex month-end adjustments | Simpler: any 31st becomes 30th |

### Return Value

Returns an integer representing the number of days between the dates calculated using the 30/360 convention. Can return negative values if end_date is before start_date.

## Examples

> [!f(x)] DAYS360 Examples

### Example 1: Basic 30/360 Calculation
```
=DAYS360(DATE(2026, 1, 1), DATE(2026, 7, 1))
```
**Result:** 180

**Explanation:** 6 months in 30/360 equals exactly 180 days (6 x 30). January 1 to July 1 is half a year.

---

### Example 2: Full Year
```
=DAYS360(DATE(2026, 1, 1), DATE(2027, 1, 1))
```
**Result:** 360

**Explanation:** One complete year always equals 360 days in the 30/360 convention, regardless of leap year status.

---

### Example 3: One Month
```
=DAYS360(DATE(2026, 1, 15), DATE(2026, 2, 15))
```
**Result:** 30

**Explanation:** Any single month equals 30 days—whether it's January (31 actual days) or February (28 actual).

---

### Example 4: February Handling
```
=DAYS360(DATE(2026, 2, 1), DATE(2026, 3, 1))
```
**Result:** 30

**Explanation:** February has 30 days in 30/360, not its actual 28 (or 29 in leap years).

---

### Example 5: End of Month to End of Month
```
=DAYS360(DATE(2026, 1, 31), DATE(2026, 2, 28))
```
**Result:** 30 (US) or 28 (European may vary)

**Explanation:** Month-end to month-end calculations can differ between methods. Always verify which method your context requires.

---

### Example 6: European Method
```
=DAYS360(DATE(2026, 1, 31), DATE(2026, 4, 30), TRUE)
```
**Result:** 89 (European method)

**Explanation:** With TRUE for European method, the 31st is simply treated as 30th without complex month-end logic.

---

### Example 7: Compare US vs European
```
=DAYS360(A1, B1, FALSE) & " US vs " & DAYS360(A1, B1, TRUE) & " EU"
```
**Result:** Shows difference between methods for given dates

**Explanation:** For most date ranges, results are identical. Differences appear at month-end boundaries.

---

### Example 8: Interest Accrual
```
=principal * annual_rate * (DAYS360(last_payment, today) / 360)
```
**Result:** Accrued interest using 30/360 convention

**Explanation:** Standard formula for bond and loan interest accrual. Days360 divided by 360 gives the fraction of year.

---

### Example 9: Quarterly Calculation
```
=DAYS360(DATE(2026, 1, 1), DATE(2026, 4, 1))
```
**Result:** 90

**Explanation:** One quarter equals exactly 90 days in 30/360 (3 months x 30 days).

---

### Example 10: Verify YEARFRAC Relationship
```
=YEARFRAC(A1, B1, 0)
=DAYS360(A1, B1) / 360
```
**Result:** Both formulas return the same value

**Explanation:** YEARFRAC with basis 0 uses DAYS360 internally. DAYS360/360 equals YEARFRAC basis 0.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date in start_date or end_date | Ensure both parameters are valid dates |
| `#VALUE!` | Non-logical value for method | Use TRUE, FALSE, 1, 0, or omit the method parameter |
| Unexpected result | Wrong method for your context | Verify whether US (FALSE) or European (TRUE) method applies |
| Different from actual days | Expecting actual calendar days | Use DAYS or date subtraction for actual days; DAYS360 is for 30/360 convention only |
| Inconsistent results | Mixing methods | Use the same method consistently across related calculations |

## Use Cases

### [[Bond Interest Calculations]]

**Scenario:** Calculate accrued interest on corporate bonds using the 30/360 day count convention specified in the bond indenture.

**Implementation:**
```
=face_value * coupon_rate * (DAYS360(last_coupon_date, settlement_date) / 360)
=par * annual_rate * DAYS360(issue_date, first_coupon) / 360
=bond_price + (coupon * DAYS360(last_payment, settlement) / DAYS360(last_payment, next_payment))
```

**Business Application:** Bond traders calculate dirty prices including accrued interest. Portfolio managers track interest income accrual. Settlement calculations determine buyer payment to seller.

**Technical Details:** US corporate bonds typically use 30/360 (NASD), while some European bonds use European 30/360. Verify the bond's specific convention in the offering documents before calculating.

---

### [[Loan and Mortgage Calculations]]

**Scenario:** Calculate interest charges, payment allocations, and payoff amounts for loans and mortgages using 30/360 conventions.

**Implementation:**
```
=loan_balance * annual_rate * DAYS360(last_payment, current_date) / 360
=principal * rate * DAYS360(start, end) / 360                         (Interest portion)
=monthly_payment - (balance * rate * 30 / 360)                        (Principal portion)
```

**Business Application:** Mortgage servicers calculate interest due. Banks determine payoff amounts for early repayment. Loan officers quote interest charges to customers.

**Technical Details:** Most US mortgages use actual/360 or 30/360. Verify the loan documents. The difference between conventions can be significant for large balances or long periods.

---

### [[Financial Statement Standardization]]

**Scenario:** Normalize time periods across different instruments and report consistent day counts for financial analysis and reporting.

**Implementation:**
```
=DAYS360(period_start, period_end) / 360                              (Period as year fraction)
=interest_income * (360 / DAYS360(start, end))                        (Annualized interest)
=SUM(DAYS360(starts, ends)) / 360                                     (Total time in years)
```

**Business Application:** Controllers standardize interest income reporting. Analysts compare returns across instruments with different actual periods. Auditors verify interest calculations.

**Technical Details:** Using 30/360 for standardization ensures that monthly figures always represent 1/12 of annual figures, making period-over-period comparisons straightforward.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 97
- **Method parameter:** FALSE = US (NASD), TRUE = European
- **Default:** US method when method is omitted
- **February handling:** US method has specific February end-of-month rules

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel
- **Method parameter:** Same TRUE/FALSE convention
- **Compatibility:** Full cross-platform compatibility

### Key Difference Alert
DAYS360 behaves identically across Excel and Sheets. The critical consideration is understanding which method (US or European) applies to your specific financial calculation. Using the wrong method produces incorrect interest calculations that may have legal or financial consequences.

## Tips and Best Practices

1. **Know which method applies:** US corporate bonds use NASD (FALSE); some European instruments use European (TRUE). Check your instrument's documentation.

2. **Understand the relationship to YEARFRAC:** YEARFRAC with basis 0 equals DAYS360/360. Use YEARFRAC directly when you need the year fraction rather than calculating manually.

3. **Test month-end dates carefully:** The US and European methods produce different results primarily at month-end boundaries. Test with your specific date patterns.

4. **Don't use for calendar day counting:** DAYS360 is specifically for the 30/360 financial convention. Use DAYS or date subtraction for actual calendar days.

5. **Document your method choice:** When building financial models, clearly note whether you're using US or European 30/360. This is critical for audit and verification.

6. **Verify against source documents:** Financial instruments specify their day count convention. Always verify your DAYS360 calculations match the contract's requirements.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DAYS]] | Counts actual calendar days | When you need actual elapsed days, not 30/360 |
| [[YEARFRAC]] | Returns year fraction with multiple bases | When you need year fraction rather than day count |
| [[NETWORKDAYS]] | Counts business days only | When you need working days excluding weekends |

### Commonly Used Together

**[[YEARFRAC]]** - Calculate year fractions

*Relationship between functions:*
```
=YEARFRAC(A1, B1, 0)              // US 30/360 year fraction
=DAYS360(A1, B1) / 360            // Equivalent calculation
=YEARFRAC(A1, B1, 4)              // European 30/360
=DAYS360(A1, B1, TRUE) / 360      // Equivalent
```
YEARFRAC with appropriate basis uses DAYS360 logic internally.

---

**[[DAYS]]** - Compare conventions

*Understand 30/360 vs actual:*
```
=DAYS(B1, A1)                     // Actual calendar days
=DAYS360(A1, B1)                  // 30/360 days
=DAYS(B1, A1) - DAYS360(A1, B1)   // Difference between methods
```
See how 30/360 differs from actual day counting.

---

**[[PMT]] / [[PV]] / [[FV]]** - Financial functions

*Use in financial calculations:*
```
=principal * annual_rate * DAYS360(start, end) / 360
=PV(annual_rate * DAYS360(start, end) / 360, 1, -payment)
```
DAYS360 provides time periods for financial function inputs.

---

**[[IF]]** - Conditional method selection

*Select method based on instrument type:*
```
=DAYS360(A1, B1, IF(instrument_type = "European", TRUE, FALSE))
=DAYS360(A1, B1, VLOOKUP(bond_type, methods, 2, FALSE))
```
Dynamically choose US or European method.

## Official Documentation

- **Microsoft Excel:** [DAYS360 function](https://support.microsoft.com/en-us/office/days360-function-b9a509fd-49ef-407e-94df-0cbda5718c2a)
- **Google Sheets:** [DAYS360 function](https://support.google.com/docs/answer/3093042)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 97 | 1997 | Original implementation |
| Excel 2007+ | 2007 | No changes |
| Excel 365 | Current | No changes |
| Google Sheets | Original launch (2006) | Full support |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
