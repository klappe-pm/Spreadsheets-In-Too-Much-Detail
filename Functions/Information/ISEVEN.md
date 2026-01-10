---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - number-testing
  - parity-check
  - even-numbers
  - mathematical-validation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-even
  - even-check
  - even-number-test
  - parity-test
tags:
  - information
  - math-functions
  - number-testing
  - even-odd
  - data-validation
  - excel
  - google-sheets
---

# ISEVEN

## Description

The **ISEVEN function** tests whether a number is even and returns TRUE if the number is divisible by 2 with no remainder, or FALSE if the number is odd. This function evaluates the mathematical parity of numeric values, providing a simple way to categorize numbers into even or odd groups without manually calculating modulo operations. ISEVEN is part of Excel's mathematical testing functions and is particularly useful for conditional formatting, data categorization, and alternating row logic. The function truncates decimal values to integers before testing, so 4.9 is treated as 4 (even) and 5.1 is treated as 5 (odd).

The primary use cases for ISEVEN include creating alternating row formatting (every other row), categorizing data by parity, implementing business rules that depend on odd/even status (like alternating schedules), and validating numeric inputs that must be even. A common pattern is using ISEVEN with ROW() to create zebra striping: =ISEVEN(ROW()) returns TRUE for rows 2, 4, 6... and FALSE for rows 1, 3, 5... This works elegantly in conditional formatting. ISEVEN also helps with inventory systems (case packs often come in even quantities), scheduling (alternating week assignments), and mathematical applications.

Important technical details about ISEVEN's behavior: First, ISEVEN truncates decimals, so 4.9 is evaluated as 4 (even), and 3.1 is evaluated as 3 (odd). Second, ISEVEN(0) returns TRUE because 0 is mathematically even (0 = 2 * 0). Third, negative numbers work correctly: -4 returns TRUE (even), -3 returns FALSE (odd). Fourth, ISEVEN returns a #VALUE! error for non-numeric input like text—it doesn't return FALSE for text. Fifth, blank cells are treated as 0, so ISEVEN on a blank cell returns TRUE. These behaviors mean you often need to combine ISEVEN with ISNUMBER for robust validation: =IF(ISNUMBER(A1), ISEVEN(A1), "Not a number").

Platform behavior for ISEVEN is identical between Excel and Google Sheets. Both truncate decimals the same way, both treat 0 as even, and both error on non-numeric input. The function has been available in Excel since Excel 2003 through the Analysis ToolPak, and natively since Excel 2007. Google Sheets has supported ISEVEN since launch. In array contexts, ISEVEN works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365.

## Syntax

> [!f(x)] ISEVEN Syntax
>
> ```
> =ISEVEN(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The numeric value to test for evenness. Decimals are truncated to integers before testing. Returns TRUE if the truncated value is even (divisible by 2). Returns #VALUE! error for non-numeric values. Blank cells are treated as 0 (even). |

### Return Value

Returns **TRUE** if the number (truncated to integer) is even. Returns **FALSE** if the number is odd. Returns **#VALUE!** error for non-numeric input. Blank cells return TRUE (treated as 0).

## Examples

> [!f(x)] ISEVEN Examples

### Example 1: Testing an Even Number
```
=ISEVEN(4)
```
**Result:** TRUE

**Explanation:** 4 is divisible by 2 with no remainder, so it's even.

---

### Example 2: Testing an Odd Number
```
=ISEVEN(7)
```
**Result:** FALSE

**Explanation:** 7 divided by 2 has a remainder of 1, so it's odd.

---

### Example 3: Testing Zero
```
=ISEVEN(0)
```
**Result:** TRUE

**Explanation:** Zero is mathematically even (0 = 2 * 0). This is correct mathematical behavior.

---

### Example 4: Testing a Negative Even Number
```
=ISEVEN(-8)
```
**Result:** TRUE

**Explanation:** -8 is divisible by 2 (-8 = 2 * -4), so negative even numbers return TRUE.

---

### Example 5: Testing a Decimal (Truncation)
```
=ISEVEN(5.9)
```
**Result:** FALSE

**Explanation:** 5.9 is truncated to 5 before testing. 5 is odd, so the result is FALSE. The .9 is ignored.

---

### Example 6: Testing Another Decimal
```
=ISEVEN(4.1)
```
**Result:** TRUE

**Explanation:** 4.1 is truncated to 4. 4 is even, so the result is TRUE.

---

### Example 7: Testing Text (Error)
```
=ISEVEN("four")
```
**Result:** #VALUE! error

**Explanation:** ISEVEN requires numeric input. Text causes an error—it doesn't return FALSE. Wrap with IFERROR if needed.

---

### Example 8: Alternating Row Detection with ROW()
```
=ISEVEN(ROW())
```
**Scenario:** Used in conditional formatting to highlight even rows
**Result:** TRUE for rows 2, 4, 6...; FALSE for rows 1, 3, 5...

**Explanation:** ROW() returns the current row number. Combined with ISEVEN, this creates alternating TRUE/FALSE patterns for zebra striping.

---

### Example 9: Conditional Logic with IF
```
=IF(ISEVEN(A1), "Even", "Odd")
```
**Scenario:** Categorize numbers as "Even" or "Odd"
**Result:** "Even" or "Odd" based on the value

**Explanation:** This pattern is useful for categorization, conditional processing, or display purposes.

---

### Example 10: Safe ISEVEN with Validation
```
=IF(ISNUMBER(A1), ISEVEN(A1), "Not a number")
```
**Scenario:** Test for even but handle non-numeric input gracefully
**Result:** TRUE/FALSE for numbers, message for non-numbers

**Explanation:** ISNUMBER check prevents #VALUE! errors from text input. Essential for data validation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input (text, error value) | Use IFERROR or check ISNUMBER first |
| `TRUE for decimal like 5.9` | ISEVEN truncates decimals | This is correct; use ROUND first if you want standard rounding |
| `TRUE for blank cell` | Blanks are treated as 0 | Check ISBLANK first if blanks should not be treated as even |
| `Unexpected TRUE for 0` | 0 is mathematically even | This is correct; add explicit check for 0 if needed |
| `ISEVEN not found` | Excel 2003 or earlier without ToolPak | Enable Analysis ToolPak add-in or upgrade Excel |

## Use Cases

### [[Alternating Row Formatting]]

**Scenario:** A report needs zebra striping (alternating row colors) for readability. Rather than manually formatting, the analyst wants automatic alternating based on row position. The formatting should persist when rows are added or deleted.

**Implementation:**
```
Conditional Formatting Formula for Even Rows:
=ISEVEN(ROW())
Format: Light gray background

Conditional Formatting Formula for Odd Rows:
=ISODD(ROW())
Format: White background

Data Row Only (skip header):
=AND(ROW()>1, ISEVEN(ROW()))

Alternating by Data Row Number:
=ISEVEN(ROW()-1)  -- Makes row 2 (first data row) "Row 1" for alternating purposes
```

**Business Application:** Zebra striping significantly improves readability of large tables. Users can track across rows more easily. Using ISEVEN(ROW()) in conditional formatting creates automatic alternating that adapts when rows are inserted or deleted—no manual reformatting needed.

**Technical Details:** The formula =ISEVEN(ROW()) in conditional formatting is evaluated for each cell, with ROW() returning that cell's row number. For data starting in row 2 with a header in row 1, you might want =ISEVEN(ROW()-1) so the first data row is treated as "row 1" (odd). Consider whether you want data rows or absolute rows to alternate.

---

### [[Inventory Packaging Validation]]

**Scenario:** A warehouse system tracks items that must be shipped in pairs or case packs (even quantities). Individual items cannot be shipped—only full pairs. The system needs to validate order quantities and flag invalid amounts.

**Implementation:**
```
Quantity Validation:
=IF(ISBLANK(Qty), "Enter quantity",
 IF(NOT(ISNUMBER(Qty)), "Invalid input",
 IF(Qty<=0, "Must be positive",
 IF(ISEVEN(Qty), "Valid: " & Qty/2 & " pairs",
    "Invalid: Cannot ship odd quantity"))))

Order Form Readiness:
=AND(ISNUMBER(Qty), Qty>0, ISEVEN(Qty))

Rounding Suggestion:
=IF(ISODD(Qty), "Round to " & Qty+1 & " or " & Qty-1, "Quantity OK")

Batch Summary:
Valid Orders: =SUMPRODUCT((ISNUMBER(AllQtys)*ISEVEN(AllQtys)*(AllQtys>0))*1)
Invalid (Odd): =SUMPRODUCT((ISNUMBER(AllQtys)*ISODD(AllQtys))*1)
```

**Business Application:** Many products ship in pairs (shoes, gloves), case packs, or must be sold in even quantities for other business reasons. ISEVEN validates that order quantities meet these requirements before processing. The rounding suggestion helps customers correct invalid orders quickly.

**Technical Details:** Layer validations in priority order: blank check, type check, positive check, then evenness check. The batch summary uses SUMPRODUCT with multiple conditions to count valid versus invalid orders. Consider adding MIN/MAX quantity validations after the even check.

---

### [[Alternating Schedule Assignment]]

**Scenario:** A staffing system assigns employees to "Team A" or "Team B" based on their employee number. Even-numbered employees go to Team A; odd-numbered employees go to Team B. This creates automatic balanced distribution for rotating assignments.

**Implementation:**
```
Team Assignment:
=IF(ISEVEN(EmployeeID), "Team A", "Team B")

Schedule Week Assignment:
=IF(ISEVEN(WeekNumber), "Morning Shift", "Afternoon Shift")

Alternating Coverage:
=IF(ISEVEN(DAY(TODAY())), "Primary: Team A", "Primary: Team B")

Fair Distribution Check:
Team A Count: =SUMPRODUCT(ISEVEN(AllEmployeeIDs)*1)
Team B Count: =SUMPRODUCT(ISODD(AllEmployeeIDs)*1)
Balance: =ABS(SUMPRODUCT(ISEVEN(AllEmployeeIDs)*1) - SUMPRODUCT(ISODD(AllEmployeeIDs)*1))
```

**Business Application:** Using employee numbers for team/schedule assignment creates automatic, predictable, and fair distribution. Employees always know their assignment based on their ID. When new employees join, they're automatically assigned based on their ID's parity. No manual assignment tracking needed.

**Technical Details:** Employee IDs should be numeric for this to work. If IDs are alphanumeric, extract numeric portions or use a different assignment method. The balance check helps identify if the distribution is roughly equal—large imbalances might indicate ID numbering issues.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2003 (Analysis ToolPak), Excel 2007+ (native)
- **Decimal handling:** Truncates to integer before testing
- **Zero handling:** Returns TRUE (0 is even)
- **Array behavior:** Native dynamic arrays in Excel 365
- **Complementary function:** ISODD for opposite test

### Google Sheets

- **Availability:** All versions since launch
- **Decimal handling:** Truncates to integer before testing
- **Zero handling:** Returns TRUE (0 is even)
- **Array behavior:** Requires ARRAYFORMULA wrapper
- **Complementary function:** ISODD for opposite test

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Behavior | Identical | Identical |
| Decimal truncation | Yes | Yes |
| Zero = even | TRUE | TRUE |
| Non-numeric error | #VALUE! | #VALUE! |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |

## Tips and Best Practices

1. **Remember decimals are truncated:** ISEVEN(5.9) returns FALSE because 5.9 becomes 5. If you need rounding instead, use ISEVEN(ROUND(A1, 0)).

2. **Zero is even:** ISEVEN(0) returns TRUE. If you need to treat 0 specially, add an explicit check: =IF(A1=0, "Zero", IF(ISEVEN(A1), "Even", "Odd")).

3. **Validate numeric input:** ISEVEN errors on text. Use =IF(ISNUMBER(A1), ISEVEN(A1), FALSE) or wrap in IFERROR for safe handling.

4. **Use with ROW() for alternating:** =ISEVEN(ROW()) in conditional formatting creates instant zebra striping. No helper columns needed.

5. **Blank cells are 0:** A blank cell returns TRUE from ISEVEN (treated as 0). Check ISBLANK first if blanks should be handled differently.

6. **Pair with ISODD for clarity:** While NOT(ISEVEN(x)) works, ISODD(x) is clearer and more readable in formulas.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISODD]] | Tests if number is odd | When checking for odd numbers |
| [[MOD]] | Returns remainder after division | When you need more control: =MOD(A1,2)=0 is equivalent to ISEVEN |
| [[TRUNC]] | Truncates to integer | When you need to see what ISEVEN will test |
| [[INT]] | Rounds down to integer | Similar truncation but different for negatives |
| [[ISNUMBER]] | Tests if value is numeric | Combine with ISEVEN for input validation |

### Commonly Used Together

**[[IF]]** - Conditional logic based on even/odd

*Categorize as even or odd:*
```
=IF(ISEVEN(A1), "Even", "Odd")
```
Basic pattern for acting on parity.

---

**[[ROW]]** - Alternating row patterns

*Zebra striping formula:*
```
=ISEVEN(ROW())
```
Essential for conditional formatting alternation.

---

**[[ISNUMBER]]** - Validate before testing

*Safe even check:*
```
=IF(ISNUMBER(A1), ISEVEN(A1), "Not numeric")
```
Prevents #VALUE! errors from non-numeric input.

---

**[[ISODD]]** - Complementary function

*Explicit odd check:*
```
=ISODD(A1)  -- Clearer than NOT(ISEVEN(A1))
```
More readable than negating ISEVEN.

---

**[[SUMPRODUCT]]** - Count even values

*Count even numbers in range:*
```
=SUMPRODUCT(ISNUMBER(A1:A100)*ISEVEN(A1:A100)*1)
```
Counts only numeric even values.

## Official Documentation

- **Microsoft Excel:** [ISEVEN function](https://support.microsoft.com/en-us/office/iseven-function-aa15929a-d77b-4fbb-92f4-2f479af55356)
- **Google Sheets:** [ISEVEN function](https://support.google.com/docs/answer/3093419)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2003 | 2003 | Analysis ToolPak add-in required |
| Excel 2007 | 2007 | Built-in natively |
| Excel 2010 | 2010 | No changes |
| Excel 2013 | 2013 | No changes |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | 2018+ | Native dynamic array support |
| Excel for Mac | 2008+ | Full support |
| Excel Online | 2010+ | Full support |
| Google Sheets | 2006 | Available since launch |
| LibreOffice Calc | 1.0 | Full compatibility |
| Apple Numbers | 2007 | Full support |

---

*Last updated: 2026-01-10*
