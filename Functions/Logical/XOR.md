---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- logical
subTopics:
- boolean-operations
- exclusive-or
- conditional-logic
- parity-checking
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Exclusive OR
- Exclusive Or Function
- XOR Logic
- Odd Count True
tags:
- logical
- boolean
- xor
- exclusive-or
- conditional
---

# XOR

## Description

**XOR** (Exclusive OR) is a logical function that returns TRUE when an odd number of its arguments evaluate to TRUE. In its simplest two-argument form, XOR returns TRUE when exactly one argument is TRUE and the other is FALSE, returning FALSE when both are TRUE or both are FALSE. This "one or the other but not both" logic is fundamentally different from OR (which returns TRUE if any argument is TRUE) and is essential for scenarios requiring mutual exclusivity, toggle states, or parity checking.

The classic XOR use case is the exclusive choice: "You can have soup OR salad, but not both." In spreadsheets, this translates to validation rules, toggle switches, and scenarios where exactly one option should be selected from a pair. For example, an employee can be either full-time OR part-time (XOR returns TRUE when exactly one is checked). A payment can be cash OR credit but not both. A cell should be filled OR marked N/A but not both or neither.

**XOR with multiple arguments:** Unlike the common two-input XOR of programming languages, spreadsheet XOR accepts multiple arguments and returns TRUE when an odd number are TRUE. With three inputs: TRUE, TRUE, TRUE returns TRUE (3 is odd). TRUE, TRUE, FALSE returns FALSE (2 is even). This extended behavior is useful for parity checks and complex validation but can be counterintuitive if you expect strict "exactly one" behavior. For exactly-one validation with many inputs, use `=COUNTIF(range, TRUE)=1` instead.

**XOR in Boolean algebra and computing:** XOR is fundamental in computer science for operations like encryption (XOR ciphers), checksums (parity bits), binary addition, and toggle operations. While these specific applications are rare in typical spreadsheet work, understanding XOR enriches your logical toolkit. In spreadsheets, XOR most often appears in data validation, form logic, and scenarios requiring mutually exclusive conditions.

## Syntax

> [!f(x)] XOR Syntax
>
> ```
> =XOR(logical1, [logical2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `logical1` | Yes | First logical value or expression. Can be TRUE, FALSE, a cell reference, or any expression that evaluates to a logical value. Numbers are interpreted as logical: 0 = FALSE, any non-zero = TRUE. |
| `logical2, ...` | No | Additional logical values or expressions. Excel supports up to 254 arguments; Google Sheets supports up to 30. Can also be arrays or ranges of logical values. |

### Return Value

Returns TRUE if an odd number of arguments evaluate to TRUE. Returns FALSE if an even number of arguments (including zero) evaluate to TRUE. If no arguments are TRUE, returns FALSE (zero is even). Text values that cannot be interpreted as logical cause #VALUE! error.

### XOR Truth Table (Two Arguments)

| A | B | XOR(A, B) | Meaning |
|---|---|-----------|---------|
| FALSE | FALSE | FALSE | Neither is true |
| TRUE | FALSE | TRUE | Exactly one is true |
| FALSE | TRUE | TRUE | Exactly one is true |
| TRUE | TRUE | FALSE | Both are true (not exclusive) |

### XOR with Multiple Arguments

| TRUE Count | XOR Result | Reason |
|------------|------------|--------|
| 0 | FALSE | 0 is even |
| 1 | TRUE | 1 is odd |
| 2 | FALSE | 2 is even |
| 3 | TRUE | 3 is odd |
| 4 | FALSE | 4 is even |
| n | TRUE if n is odd | Parity determines result |

## Examples

> [!f(x)] XOR Examples

### Example 1: Basic Two-Value XOR
```
=XOR(TRUE, FALSE)
```
**Result:** TRUE

**Explanation:** One value is TRUE, one is FALSE - exactly one TRUE means XOR returns TRUE. This is the classic "exclusive or" scenario.

---

### Example 2: Both TRUE (Not Exclusive)
```
=XOR(TRUE, TRUE)
```
**Result:** FALSE

**Explanation:** Both arguments are TRUE. XOR returns FALSE because "exclusive" means only one should be true, not both. This distinguishes XOR from OR, which would return TRUE here.

---

### Example 3: Both FALSE
```
=XOR(FALSE, FALSE)
```
**Result:** FALSE

**Explanation:** Neither argument is TRUE. XOR returns FALSE because zero TRUEs is an even count. Neither option selected fails the "exactly one" test.

---

### Example 4: Cell Comparison XOR
```
=XOR(A1>100, B1>100)
```
**Result:** TRUE if exactly one cell exceeds 100

**Explanation:** Returns TRUE if A1 is over 100 but B1 isn't, or if B1 is over 100 but A1 isn't. Returns FALSE if both exceed 100 or neither does. Useful for detecting divergent conditions.

---

### Example 5: Form Validation - One Required Field
```
=XOR(A1<>"", B1<>"")
```
**Result:** TRUE if exactly one field is filled

**Explanation:** Validates that exactly one of two optional fields has data. Both filled or both empty returns FALSE. Useful for "provide phone OR email (not both)" type validation rules.

---

### Example 6: Multiple Argument XOR (Odd Count)
```
=XOR(TRUE, TRUE, TRUE)
```
**Result:** TRUE

**Explanation:** Three TRUEs is an odd number, so XOR returns TRUE. This is where XOR differs from the common "exactly one" expectation - it's actually "odd number of TRUEs."

---

### Example 7: Multiple Argument XOR (Even Count)
```
=XOR(TRUE, TRUE, FALSE, FALSE)
```
**Result:** FALSE

**Explanation:** Two TRUEs out of four arguments. Two is even, so XOR returns FALSE. The FALSE values don't change the count of TRUEs.

---

### Example 8: Checkbox Exclusive Selection
```
=XOR(C2=TRUE, D2=TRUE, E2=TRUE)
```
**Result:** TRUE if 1 or 3 checkboxes are checked

**Explanation:** If you expect "exactly one" behavior but have three checkboxes, be aware XOR(TRUE,FALSE,FALSE) returns TRUE, but so does XOR(TRUE,TRUE,TRUE). For strictly "exactly one", use `=COUNTIF(C2:E2,TRUE)=1` instead.

---

### Example 9: Toggle State Detection
```
=XOR(CurrentState="On", Toggle=TRUE)
```
**Result:** New state after applying toggle

**Explanation:** XOR simulates toggle logic: if CurrentState is "On" (TRUE) and Toggle is TRUE, result is FALSE (turn off). If CurrentState is "Off" (FALSE) and Toggle is TRUE, result is TRUE (turn on). Toggle=FALSE means no change.

---

### Example 10: Range-Based XOR
```
=XOR(A1:A10>50)
```
**Result:** TRUE if an odd count of cells in A1:A10 exceed 50

**Explanation:** XOR can accept a range. The comparison A1:A10>50 produces an array of TRUE/FALSE. XOR counts the TRUEs and returns TRUE if the count is odd. Useful for parity checking across multiple cells.

---

### Example 11: Combining XOR with IF
```
=IF(XOR(A1="Manager", B1="Manager"), "One Manager", "Review Needed")
```
**Result:** "One Manager" if exactly one person is a Manager

**Explanation:** A project team needs exactly one manager assigned. XOR detects when exactly one of the two role fields equals "Manager". Both managers or no managers triggers the "Review Needed" message.

---

### Example 12: Parity Check for Data Validation
```
=IF(XOR(B2:B10), "Odd parity", "Even parity")
```
**Result:** Indicates whether the count of TRUE values is odd or even

**Explanation:** Parity checking is a classic XOR application from computer science. If you have a series of yes/no values and need to verify data integrity through parity, XOR provides the check directly.

---

### Example 13: XOR vs OR vs AND Comparison
```
A1=TRUE, B1=FALSE:
=AND(A1,B1)  → FALSE (not all true)
=OR(A1,B1)   → TRUE  (at least one true)
=XOR(A1,B1)  → TRUE  (exactly one true)

A1=TRUE, B1=TRUE:
=AND(A1,B1)  → TRUE  (all true)
=OR(A1,B1)   → TRUE  (at least one true)
=XOR(A1,B1)  → FALSE (both true, not exclusive)
```
**Result:** Shows the difference between logical operators

**Explanation:** AND requires all TRUE. OR requires at least one TRUE. XOR requires an odd number of TRUEs (for two arguments, this means exactly one). Understanding these distinctions is fundamental to logical formula design.

---

### Example 14: Simulating Exactly-One Check (More Than 2 Options)
```
=COUNTIF(A1:A5, TRUE)=1
```
**Result:** TRUE if exactly one cell in range is TRUE

**Explanation:** Since XOR with multiple arguments checks for odd count (not exactly one), use COUNTIF when you truly need "exactly one out of many" validation. This formula returns TRUE only when precisely one cell is TRUE.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Text argument that cannot be interpreted as logical | Ensure all arguments are TRUE, FALSE, numbers, or expressions returning Boolean. Text like "Yes" causes errors; use comparison: `A1="Yes"`. |
| `Unexpected TRUE with 3+ arguments` | XOR returns TRUE for odd count, not exactly-one | Use `=COUNTIF(range,TRUE)=1` if you need exactly one TRUE out of many options. |
| `Wrong result with numbers` | Non-zero numbers treated as TRUE | Be aware that XOR(1,2,3) = TRUE (three TRUEs). Use explicit comparisons: XOR(A1=1, B1=2). |
| `FALSE when expecting TRUE` | Even number of TRUE values | Remember XOR returns TRUE for ODD count of TRUEs. Two TRUEs returns FALSE. |

## Use Cases

### [[Mutually Exclusive Option Validation]]

**Scenario:** A form has two payment options: "Pay Now" or "Invoice Later." Users must select exactly one.

**Implementation:**
```
=IF(XOR(PayNow=TRUE, InvoiceLater=TRUE), "Valid Selection", "Select exactly one payment option")
```

**Business Application:** Order forms, registration systems, and application forms often have mutually exclusive options. Using XOR in data validation ensures users make a clear, valid choice. This prevents processing errors from conflicting selections or missing choices.

**Technical Details:** If you have more than two options, XOR might not work as expected (returns TRUE for odd count, not exactly one). For three+ options, use `=COUNTIF(OptionRange, TRUE)=1` for strict exactly-one validation.

---

### [[Change Detection Between States]]

**Scenario:** Compare two snapshots of data to identify rows where status changed from active to inactive OR from inactive to active, but not rows that stayed the same.

**Implementation:**
```
=XOR(PreviousStatus="Active", CurrentStatus="Active")
```
Returns TRUE for rows where status changed (either direction)

**Business Application:** Audit trails and change logs need to identify what actually changed. XOR elegantly detects transitions in either direction. A subscription that was active and now isn't, or wasn't active and now is, both represent meaningful changes worth flagging.

**Technical Details:** XOR returns TRUE when the two states differ. This is equivalent to `Previous<>Current` for Boolean states but reads more clearly as "one or the other but not both." For non-Boolean states, stick with the inequality comparison.

---

### [[Toggle Switch Logic]]

**Scenario:** A dashboard control toggles a setting. Clicking the toggle should flip the current state: On becomes Off, Off becomes On.

**Implementation:**
```
New State: =XOR(CurrentState, TogglePressed)
```
If Current=TRUE and Toggle=TRUE, result is FALSE (turn off)
If Current=FALSE and Toggle=TRUE, result is TRUE (turn on)
If Toggle=FALSE, state unchanged

**Business Application:** Interactive dashboards with toggle controls need state management. XOR provides the mathematical basis for flip-flop behavior without complex IF statements. Each toggle press flips the state.

**Technical Details:** This is the fundamental XOR operation used in computing for binary state changes. In spreadsheets, combine with Named Ranges or structured references for readable toggle implementations.

---

### [[Divergence Alert in Paired Metrics]]

**Scenario:** Monitor two related metrics that should typically move together. Alert when one exceeds threshold but the other doesn't.

**Implementation:**
```
=IF(XOR(MetricA>Threshold, MetricB>Threshold), "ALERT: Divergence Detected", "Normal")
```

**Business Application:** Financial analysts monitor correlated metrics (revenue vs. orders, leads vs. conversions). When typically-paired metrics diverge (one up, one down), it signals anomalies worth investigating. XOR detects this divergence directly.

**Technical Details:** This pattern works for any paired threshold checks. Extend with AND to require divergence in a specific direction: `AND(XOR(...), MetricA>MetricB)` detects divergence where A exceeds but B doesn't (not vice versa).

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2013 and later (including Excel 365, Excel 2016, 2019, 2021)
- **NOT available:** Excel 2010 and earlier
- **Maximum arguments:** 254 logical values
- **Array behavior:** Accepts arrays/ranges; counts TRUEs across all values
- **Number handling:** Any non-zero number treated as TRUE; 0 as FALSE

### Google Sheets

- **Availability:** All versions
- **Maximum arguments:** 30 logical values
- **Array behavior:** Accepts arrays/ranges; works with ARRAYFORMULA for row-by-row evaluation
- **Number handling:** Same as Excel (non-zero = TRUE)

### Key Difference Alert

The **argument limit** is the main practical difference: Excel allows 254, Sheets allows 30. For typical exclusive-or logic (2 arguments), this doesn't matter. For parity checking across large ranges, you may need alternative approaches in Sheets. Also note that XOR wasn't available in Excel until 2013, so legacy spreadsheets may use workarounds like `=(A1+B1=1)` for the two-argument case.

## Tips and Best Practices

1. **XOR ≠ "exactly one" for 3+ arguments:** The most common XOR mistake is expecting XOR(A,B,C)=TRUE to mean exactly one is true. It actually means an odd count is true. Use COUNTIF for exactly-one validation with multiple options.

2. **XOR is commutative:** XOR(A,B) = XOR(B,A). Order doesn't matter, which simplifies formula writing.

3. **Two-argument XOR = inequality for Booleans:** XOR(A,B) is equivalent to A<>B when both are TRUE/FALSE. Use whichever reads more clearly in your context.

4. **Use for toggle logic:** XOR is perfect for implementing toggle/flip-flop behavior. Current XOR Toggle = New State.

5. **Combine with IF for user-friendly output:** XOR returns TRUE/FALSE; wrap in IF for custom messages:
   ```
   =IF(XOR(A1,B1), "Select only one", "OK")
   ```

6. **Alternative for missing XOR (old Excel):** In Excel 2010 and earlier, simulate two-argument XOR with `=(A1<>B1)` for Boolean values or `=((A1=TRUE)+(B1=TRUE)=1)` for explicit logic.

7. **Test with edge cases:** When using XOR in validation, test: both true, both false, one each direction, all combinations for 3+ inputs.

8. **XOR for simple encryption isn't secure:** While XOR is used in real encryption, spreadsheet XOR for "encrypting" data provides no real security and shouldn't be used for sensitive data protection.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AND]] | Returns TRUE if ALL arguments are TRUE | When all conditions must be met simultaneously |
| [[OR]] | Returns TRUE if ANY argument is TRUE | When at least one condition being true is sufficient |
| [[NOT]] | Reverses TRUE to FALSE and vice versa | When inverting a single condition |
| [[IF]] | Returns different values based on condition | When you need to act on XOR result with custom outputs |

### Commonly Used Together

**[[IF]]** - Provides custom output based on XOR result

*Actionable validation message:*
```
=IF(XOR(A1<>"", B1<>""), "Valid", "Fill exactly one field")
```
XOR tests the condition; IF converts to user-friendly message.

---

**[[AND]] / [[OR]]** - Other logical operators for combined conditions

*Complex validation combining operators:*
```
=IF(AND(XOR(TypeA, TypeB), StatusActive), "Process", "Hold")
```
XOR ensures exactly one type; AND requires that plus active status. Combining logical operators enables sophisticated rule sets.

---

**[[COUNTIF]]** - Alternative for "exactly one of many"

*Strictly exactly-one validation:*
```
=COUNTIF(A1:A10, TRUE) = 1
```
When you have many options and need exactly one TRUE (not XOR's odd count), COUNTIF provides the direct solution.

---

**[[NOT]]** - Inverting conditions before XOR

*XOR with inverted inputs:*
```
=XOR(NOT(A1), B1)
```
Sometimes you need XOR between an inverted condition and a direct condition.

---

**[[SWITCH]] / [[IFS]]** - Multi-way decisions after XOR

*Handling XOR outcomes:*
```
=IFS(AND(A1,B1), "Both", XOR(A1,B1), "One", TRUE, "Neither")
```
IFS can handle both-true, exactly-one, and neither cases with appropriate messages.

## Official Documentation

- **Microsoft Excel:** [XOR function](https://support.microsoft.com/en-us/office/xor-function-1548d4c2-5e47-4f77-9a92-0533bba14f37)
- **Google Sheets:** [XOR function](https://support.google.com/docs/answer/3093306)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New logical function; not in earlier versions |
| Excel 2016+ | All subsequent | No changes |
| Excel 365 | Current | Full support including dynamic arrays |
| Google Sheets | Original launch | Available from start; 30 argument limit |

---

*Last updated: 2026-01-10*
