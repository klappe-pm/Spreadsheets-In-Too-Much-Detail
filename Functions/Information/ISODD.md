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
  - odd-numbers
  - mathematical-validation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-odd
  - odd-check
  - odd-number-test
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

# ISODD

## Description

The **ISODD function** tests whether a number is odd and returns TRUE if the number is NOT divisible by 2 (has a remainder of 1 when divided by 2), or FALSE if the number is even. This function is the complement to ISEVEN and provides a direct way to test for odd numbers without calculating modulo operations. ISODD is useful for conditional formatting, alternating patterns, data categorization, and any logic that depends on odd/even status. Like ISEVEN, the function truncates decimal values to integers before testing, so 5.9 is treated as 5 (odd) and 4.1 is treated as 4 (even).

The primary use cases for ISODD include creating alternating patterns (odd-numbered rows/items), categorizing data by parity, scheduling that uses odd/even logic, and validating inputs that must be odd numbers. In conditional formatting, =ISODD(ROW()) highlights rows 1, 3, 5... while ISEVEN highlights rows 2, 4, 6... ISODD is functionally equivalent to NOT(ISEVEN(x)), but using ISODD directly is clearer and more readable. Common applications include page numbering (odd pages on right), alternating week schedules, and mathematical categorization.

Important technical details about ISODD's behavior mirror ISEVEN: First, ISODD truncates decimals, so 4.9 becomes 4 (returns FALSE, even) and 5.1 becomes 5 (returns TRUE, odd). Second, ISODD(0) returns FALSE because 0 is mathematically even. Third, negative numbers work correctly: -5 returns TRUE (odd), -4 returns FALSE (even). Fourth, ISODD returns #VALUE! error for non-numeric input—it doesn't return TRUE for text. Fifth, blank cells are treated as 0, so ISODD on a blank cell returns FALSE. These parallel behaviors make ISEVEN and ISODD perfect complements.

Platform behavior for ISODD is identical between Excel and Google Sheets. Both truncate decimals, both treat 0 as even (so ISODD returns FALSE), and both error on non-numeric input. The function has been available in Excel since Excel 2003 through the Analysis ToolPak, and natively since Excel 2007. Google Sheets has supported ISODD since launch. In array contexts, ISODD works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365.

## Syntax

> [!f(x)] ISODD Syntax
>
> ```
> =ISODD(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The numeric value to test for oddness. Decimals are truncated to integers before testing. Returns TRUE if the truncated value is odd (has remainder 1 when divided by 2). Returns #VALUE! error for non-numeric values. Blank cells are treated as 0 (returns FALSE). |

### Return Value

Returns **TRUE** if the number (truncated to integer) is odd. Returns **FALSE** if the number is even. Returns **#VALUE!** error for non-numeric input. Blank cells return FALSE (treated as 0, which is even).

## Examples

> [!f(x)] ISODD Examples

### Example 1: Testing an Odd Number
```
=ISODD(7)
```
**Result:** TRUE

**Explanation:** 7 divided by 2 has a remainder of 1, so it's odd.

---

### Example 2: Testing an Even Number
```
=ISODD(8)
```
**Result:** FALSE

**Explanation:** 8 is divisible by 2 with no remainder, so it's even, not odd.

---

### Example 3: Testing Zero
```
=ISODD(0)
```
**Result:** FALSE

**Explanation:** Zero is mathematically even (0 = 2 * 0), so ISODD returns FALSE.

---

### Example 4: Testing a Negative Odd Number
```
=ISODD(-9)
```
**Result:** TRUE

**Explanation:** -9 is odd (not divisible by 2 evenly). Negative odd numbers return TRUE.

---

### Example 5: Testing a Decimal (Truncation)
```
=ISODD(4.9)
```
**Result:** FALSE

**Explanation:** 4.9 is truncated to 4 before testing. 4 is even, so ISODD returns FALSE.

---

### Example 6: Testing Another Decimal
```
=ISODD(5.1)
```
**Result:** TRUE

**Explanation:** 5.1 is truncated to 5. 5 is odd, so the result is TRUE.

---

### Example 7: Testing Text (Error)
```
=ISODD("five")
```
**Result:** #VALUE! error

**Explanation:** ISODD requires numeric input. Text causes an error. Use IFERROR or check ISNUMBER first.

---

### Example 8: Alternating Row Detection with ROW()
```
=ISODD(ROW())
```
**Scenario:** Used in conditional formatting to highlight odd rows
**Result:** TRUE for rows 1, 3, 5...; FALSE for rows 2, 4, 6...

**Explanation:** Combined with ROW(), ISODD creates the opposite pattern from ISEVEN for zebra striping.

---

### Example 9: Conditional Logic with IF
```
=IF(ISODD(A1), "Odd", "Even")
```
**Scenario:** Categorize numbers as "Odd" or "Even"
**Result:** "Odd" or "Even" based on the value

**Explanation:** Clear categorization for display or processing purposes.

---

### Example 10: Safe ISODD with Validation
```
=IF(ISNUMBER(A1), ISODD(A1), "Not a number")
```
**Scenario:** Test for odd but handle non-numeric input gracefully
**Result:** TRUE/FALSE for numbers, message for non-numbers

**Explanation:** ISNUMBER check prevents #VALUE! errors. Essential for data validation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input (text, error value) | Use IFERROR or check ISNUMBER first |
| `FALSE for decimal like 4.9` | ISODD truncates decimals to 4 (even) | This is correct; use ROUND first if you want standard rounding |
| `FALSE for blank cell` | Blanks are treated as 0 (even) | Check ISBLANK first if blanks should be handled differently |
| `FALSE for 0` | 0 is mathematically even | This is correct; odd check for 0 should be FALSE |
| `ISODD not found` | Excel 2003 or earlier without ToolPak | Enable Analysis ToolPak add-in or upgrade Excel |

## Use Cases

### [[Odd-Page Formatting]]

**Scenario:** A print layout needs different formatting for odd and even pages (common in book printing where odd pages appear on the right). The template uses page numbers to determine formatting.

**Implementation:**
```
Page Side Determination:
=IF(ISODD(PageNumber), "Right Side", "Left Side")

Margin Settings:
Left Margin: =IF(ISODD(PageNumber), 0.5, 1)  -- Narrower left on odd (right) pages
Right Margin: =IF(ISODD(PageNumber), 1, 0.5) -- Wider right for binding

Header Position:
=IF(ISODD(PageNumber), "Align Right", "Align Left")

Page Classification:
=CHOOSE(ISODD(PageNumber)+1, "Verso (Even/Left)", "Recto (Odd/Right)")
```

**Business Application:** Book and document printing follows conventions where odd pages are on the right (recto) and even pages on the left (verso). Headers, footers, and margins often mirror between sides. ISODD determines page placement automatically based on page number.

**Technical Details:** The CHOOSE formula uses ISODD+1 to convert FALSE/TRUE to 1/2 for CHOOSE indexing. Margin calculations assume the binding edge (spine) needs more space. For front matter (roman numerals), convert to page numbers first before testing parity.

---

### [[Alternating Work Assignments]]

**Scenario:** A call center assigns incoming tickets to two teams in strict alternation. Odd-numbered tickets go to Team Alpha, even-numbered tickets go to Team Beta. This ensures fair workload distribution.

**Implementation:**
```
Team Assignment:
=IF(ISODD(TicketNumber), "Team Alpha", "Team Beta")

Current Queue Counts:
Team Alpha Queue: =SUMPRODUCT((Status="Open")*(ISODD(TicketNumber))*1)
Team Beta Queue: =SUMPRODUCT((Status="Open")*(ISEVEN(TicketNumber))*1)

Workload Balance:
=ABS(SUMPRODUCT(ISODD(AllTickets)*1) - SUMPRODUCT(ISEVEN(AllTickets)*1))

Assignment With Overflow Handling:
=IF(Team_Alpha_Queue > Threshold,
    "Team Beta (overflow)",
    IF(ISODD(TicketNumber), "Team Alpha", "Team Beta"))
```

**Business Application:** Round-robin assignment based on ticket number creates automatic, auditable, fair distribution. Neither team can claim favoritism. When disputes arise, the assignment is mathematically determined by the ticket number. The overflow handling allows temporary load balancing during busy periods.

**Technical Details:** Ticket numbers must be sequential for true alternation. If tickets can be deleted or numbers skip, the alternation might become unbalanced. The queue counts show current workload. Consider adding time-based analysis to see if odd/even tickets have different complexity levels.

---

### [[Mathematical Sequence Detection]]

**Scenario:** An educational worksheet helps students identify odd and even numbers. The system needs to check student answers, generate practice sequences, and provide feedback on patterns.

**Implementation:**
```
Answer Checking:
=IF(ISODD(Number)=(StudentAnswer="odd"), "Correct!", "Incorrect - " & Number & " is " & IF(ISODD(Number), "odd", "even"))

Generate Next Odd Number:
=IF(ISODD(CurrentNumber), CurrentNumber+2, CurrentNumber+1)

Odd Number Sequence (from range):
=FILTER(Numbers, ISODD(Numbers))

Pattern Recognition Prompt:
=IF(AND(ISODD(A1), ISODD(B1), ISODD(C1)),
    "All odd - sum will be odd",
    IF(AND(ISEVEN(A1), ISEVEN(B1), ISEVEN(C1)),
        "All even - sum will be even",
        "Mixed - analyze the pattern"))
```

**Business Application:** Educational tools for mathematics need to verify odd/even understanding. ISODD provides clear, correct determination for answer checking. The pattern recognition teaches students about properties of odd/even numbers (sum of two odds is even, etc.).

**Technical Details:** The answer checking compares ISODD result to student's text answer. Generate sequences by adding 2 to stay within odd/even category. FILTER extracts all odd numbers from a dataset for practice problems. Consider adding explanations for why numbers are odd/even.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2003 (Analysis ToolPak), Excel 2007+ (native)
- **Decimal handling:** Truncates to integer before testing
- **Zero handling:** Returns FALSE (0 is even)
- **Array behavior:** Native dynamic arrays in Excel 365
- **Complementary function:** ISEVEN for opposite test

### Google Sheets

- **Availability:** All versions since launch
- **Decimal handling:** Truncates to integer before testing
- **Zero handling:** Returns FALSE (0 is even)
- **Array behavior:** Requires ARRAYFORMULA wrapper
- **Complementary function:** ISEVEN for opposite test

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Behavior | Identical | Identical |
| Decimal truncation | Yes | Yes |
| Zero = even | FALSE (not odd) | FALSE (not odd) |
| Non-numeric error | #VALUE! | #VALUE! |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |

## Tips and Best Practices

1. **Complement to ISEVEN:** ISODD(x) is equivalent to NOT(ISEVEN(x)), but ISODD is clearer. Use the function that matches your intent.

2. **Remember decimals are truncated:** ISODD(4.9) returns FALSE because 4.9 becomes 4 (even). Use ROUND first if you want standard rounding behavior.

3. **Zero is not odd:** ISODD(0) returns FALSE. Zero is mathematically even. If you need special handling for 0, add an explicit check.

4. **Validate numeric input:** ISODD errors on text. Use =IF(ISNUMBER(A1), ISODD(A1), FALSE) for safe handling.

5. **Use with ROW() for alternating:** =ISODD(ROW()) highlights odd rows in conditional formatting, complementing ISEVEN for zebra striping.

6. **Blank cells return FALSE:** Blanks are treated as 0 (even). Check ISBLANK first if blanks need different handling.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISEVEN]] | Tests if number is even | When checking for even numbers |
| [[MOD]] | Returns remainder after division | When you need more control: =MOD(A1,2)=1 is equivalent to ISODD |
| [[TRUNC]] | Truncates to integer | When you need to see what ISODD will test |
| [[INT]] | Rounds down to integer | Similar truncation but different for negatives |
| [[ISNUMBER]] | Tests if value is numeric | Combine with ISODD for input validation |

### Commonly Used Together

**[[IF]]** - Conditional logic based on odd/even

*Categorize as odd or even:*
```
=IF(ISODD(A1), "Odd", "Even")
```
Basic pattern for acting on parity.

---

**[[ROW]]** - Alternating row patterns

*Highlight odd rows:*
```
=ISODD(ROW())
```
Complements ISEVEN for conditional formatting.

---

**[[ISNUMBER]]** - Validate before testing

*Safe odd check:*
```
=IF(ISNUMBER(A1), ISODD(A1), "Not numeric")
```
Prevents #VALUE! errors.

---

**[[ISEVEN]]** - Complementary function

*Complete parity logic:*
```
=IF(ISODD(A1), "Odd", IF(ISEVEN(A1), "Even", "Error"))
```
Full coverage of odd/even cases.

---

**[[FILTER]]** - Extract odd values

*Get all odd numbers from range:*
```
=FILTER(A1:A100, ISODD(A1:A100))
```
Creates array of only odd values.

## Official Documentation

- **Microsoft Excel:** [ISODD function](https://support.microsoft.com/en-us/office/isodd-function-1208a56d-4f10-4f44-a5fc-648cafd6c07a)
- **Google Sheets:** [ISODD function](https://support.google.com/docs/answer/3093491)

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
