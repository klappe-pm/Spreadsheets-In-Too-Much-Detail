---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- truncation
- precision
- toward-zero
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Truncate
- Remove Decimals
- Toward Zero
tags:
- truncate
- precision
- toward-zero
- decimals
- integer
---

# TRUNC

## Description

**TRUNC** truncates a number toward zero by removing digits—it simply chops off decimal places without rounding. TRUNC(3.7) = 3, TRUNC(-3.7) = -3. Unlike INT which always goes toward negative infinity, TRUNC always goes toward zero. It's like removing digits with scissors rather than rounding mathematically.

The function optionally takes a second argument specifying how many decimal places to keep. TRUNC(3.456, 2) = 3.45 (keeps 2 decimals), TRUNC(3.456, 1) = 3.4 (keeps 1 decimal), TRUNC(3.456, 0) = 3 (keeps no decimals, same as default). You can even use negative values: TRUNC(1234, -2) = 1200 (truncates to hundreds place).

**TRUNC vs INT: The critical difference explained.** For positive numbers, TRUNC and INT are identical—both give 3 for 3.7. For negative numbers, they diverge. TRUNC(-3.7) = -3 (toward zero), INT(-3.7) = -4 (toward negative infinity). Think of TRUNC as "remove decimals" and INT as "floor function." The mathematical floor always goes down on the number line; truncation always goes toward zero.

**When to use which?** Use INT when you need true floor behavior (largest integer less than or equal to), especially in mathematical contexts. Use TRUNC when you want to simply remove decimal places regardless of sign—like displaying whole dollars without cents, or limiting precision without rounding. TRUNC is more intuitive for "just cut off the decimals" scenarios.

## Syntax

> [!f(x)] TRUNC Syntax
>
> ```
> =TRUNC(number, [num_digits])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The numeric value to truncate. Can be positive or negative. The function removes digits in the direction of zero (positive becomes smaller, negative becomes larger/closer to zero). |
| `num_digits` | No | The number of decimal places to keep. Default is 0 (truncate to integer). Positive values specify decimals to keep; negative values truncate to the left of the decimal point (tens, hundreds, etc.). |

### Return Value

Returns the truncated number with the specified precision. Always moves toward zero—positive numbers become smaller or stay same, negative numbers become larger (closer to zero) or stay same.

## Examples

> [!f(x)] TRUNC Examples

### Example 1: Basic Truncation (No Decimals)
```
=TRUNC(3.7)
```
**Result:** 3

**Explanation:** Removes the decimal portion, leaving just 3. Default num_digits is 0.

---

### Example 2: Negative Number Truncation - KEY DIFFERENCE
```
=TRUNC(-3.7)
```
**Result:** -3

**Explanation:** Truncates toward zero. -3 is closer to zero than -3.7. This is where TRUNC differs from INT, which would give -4.

---

### Example 3: Keep Two Decimal Places
```
=TRUNC(3.14159, 2)
```
**Result:** 3.14

**Explanation:** Keeps 2 decimal places, discarding the rest. No rounding—just truncation. ROUND(3.14159, 2) would give 3.14, but TRUNC(3.14559, 2) = 3.14 while ROUND would give 3.15.

---

### Example 4: Keep One Decimal Place
```
=TRUNC(2.789, 1)
```
**Result:** 2.7

**Explanation:** Keeps 1 decimal place. The .089 is simply discarded, not rounded. ROUND(2.789, 1) = 2.8, but TRUNC gives 2.7.

---

### Example 5: Truncate to Tens
```
=TRUNC(1234, -1)
```
**Result:** 1230

**Explanation:** Negative num_digits truncates left of the decimal. -1 removes the ones digit. 1234 becomes 1230.

---

### Example 6: Truncate to Hundreds
```
=TRUNC(5678, -2)
```
**Result:** 5600

**Explanation:** -2 removes tens and ones digits. 5678 becomes 5600. Useful for displaying approximate values.

---

### Example 7: Truncate to Thousands
```
=TRUNC(12345, -3)
```
**Result:** 12000

**Explanation:** -3 removes hundreds, tens, and ones. Common for budget or financial summary displays.

---

### Example 8: Compare with ROUND
```
=TRUNC(2.5, 0) vs =ROUND(2.5, 0)
```
**Result:** TRUNC = 2, ROUND = 3

**Explanation:** TRUNC just removes decimals (2.5 → 2). ROUND applies rounding rules (2.5 rounds up to 3).

---

### Example 9: Negative with Decimals
```
=TRUNC(-5.789, 1)
```
**Result:** -5.7

**Explanation:** Truncates toward zero, keeping 1 decimal. -5.7 is closer to zero than -5.789.

---

### Example 10: Compare TRUNC vs INT for Negative
```
=TRUNC(-2.3) vs =INT(-2.3)
```
**Result:** TRUNC = -2, INT = -3

**Explanation:** TRUNC goes toward zero (-2.3 → -2). INT floors toward negative infinity (-2.3 → -3). Critical distinction for negative numbers.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure arguments are numbers. Text causes errors. |
| `#NAME?` | Misspelled function name | Verify spelling: TRUNC, not TRUNCATE or TRUN. |
| `Expected INT behavior` | Confusion between functions | For negative numbers, TRUNC goes toward zero, INT floors toward -infinity. Use the one matching your need. |
| `Unexpected with negative num_digits` | Unfamiliar syntax | TRUNC(1234, -2) = 1200 is correct. Negative num_digits truncates to left of decimal. |
| `Result seems rounded` | Cell formatting | TRUNC doesn't round, but cell formatting might display fewer decimals. Check actual value. |

## Use Cases

### [[Display Formatting Without Rounding]]

**Scenario:** Show values with limited decimals without the bias of rounding.

**Implementation:**
```
=TRUNC(Price, 2)                             → Price to cents, no rounding
=TRUNC(Percentage * 100, 1) & "%"            → Percentage with 1 decimal
=TRUNC(LargeNumber, -6)                      → Display in millions
```

**Business Application:** Financial displays where rounding bias is undesirable, inventory quantities, report formatting. When you want to show partial values without artificially inflating them.

**Technical Details:** TRUNC always reduces magnitude (or keeps same), never increases. For financial reporting where rounding up might overstate values, TRUNC is conservative.

---

### [[Extracting Integer Parts from Calculations]]

**Scenario:** Get whole number results from division or other calculations, truncating rather than rounding.

**Implementation:**
```
=TRUNC(Total / ItemPrice)                    → Whole items affordable
=TRUNC(Score / 10)                           → Decade grouping
=TRUNC(ElapsedTime * 24)                     → Complete hours
```

**Business Application:** Inventory allocation, grouping calculations, time decomposition. Any scenario where you need the integer part and the decimal is just noise.

**Technical Details:** For positive numbers, TRUNC and INT are equivalent. For negative numbers, choose based on whether you want toward-zero (TRUNC) or floor (INT) behavior.

---

### [[Precision Control in Calculations]]

**Scenario:** Limit decimal places at intermediate steps or for specific precision requirements.

**Implementation:**
```
=TRUNC(Rate, 4)                              → Limit rate to 4 decimals
=TRUNC(A1 * 1000) / 1000                     → Alternative 3-decimal truncation
=TRUNC(Measurement, DesiredPrecision)        → Dynamic precision control
```

**Business Application:** Currency calculations, measurement precision, API requirements with specific decimal limits.

**Technical Details:** TRUNC doesn't round, which may be important for certain financial or scientific calculations where rounding could introduce systematic bias.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 1.0
- **Behavior:** Truncates toward zero for all numbers
- **num_digits default:** 0 (truncate to integer)
- **Negative num_digits:** Supported, truncates left of decimal

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel—toward zero
- **num_digits default:** Same as Excel
- **Negative num_digits:** Same support

### Both Platforms
- Identical behavior for all inputs
- Same handling of positive and negative numbers
- Same support for negative num_digits
- Same #VALUE! error for non-numeric input

### Critical Comparison: TRUNC vs INT

| Input | TRUNC Result | INT Result | Direction |
|-------|-------------|-----------|-----------|
| 3.7 | 3 | 3 | Same (both go to 3) |
| 3.2 | 3 | 3 | Same (both go to 3) |
| -3.2 | -3 | -4 | TRUNC → 0, INT → -infinity |
| -3.7 | -3 | -4 | TRUNC → 0, INT → -infinity |
| -0.5 | 0 | -1 | TRUNC → 0, INT → -infinity |
| 0.5 | 0 | 0 | Same (both go to 0) |

**Remember:** TRUNC always toward zero. INT always toward negative infinity.

## Tips and Best Practices

1. **TRUNC for "remove decimals" intent:** When you want to simply cut off decimal places without mathematical rounding, TRUNC expresses that intent clearly. It's intuitive for display truncation.

2. **Know the INT difference:** TRUNC(-3.7) = -3, INT(-3.7) = -4. For positive numbers they're the same, but for negative numbers, TRUNC goes toward zero while INT floors toward negative infinity.

3. **Use num_digits for precision control:** TRUNC(value, 2) keeps 2 decimals. TRUNC(value, -2) truncates to hundreds. More flexible than INT which only gives integers.

4. **Combine with multiplication for percentage display:** TRUNC(Rate * 100, 2) gives percentage with 2 decimals without rounding bias.

5. **Conservative financial calculations:** When rounding might overstate values (inventory, conservative estimates), TRUNC ensures you never exceed the true value.

6. **Alternative to ROUNDDOWN:** TRUNC and ROUNDDOWN are identical for positive numbers. For negative numbers, TRUNC goes toward zero while ROUNDDOWN goes away from zero (like floor).

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[INT]] | Floor toward negative infinity | When you need true floor function behavior |
| [[ROUND]] | Standard rounding | When you want to round to nearest, not truncate |
| [[ROUNDDOWN]] | Round toward zero | Same as TRUNC, but some prefer this name |
| [[FLOOR]] | Floor to multiple | When truncating to arbitrary multiples |

### Commonly Used Together

**[[INT]]** - Compare truncation approaches

*Choose the right behavior:*
```
=TRUNC(-2.5)  → -2 (toward zero)
=INT(-2.5)    → -3 (floor)
```
Pick based on whether you want toward-zero or floor behavior.

---

**[[ROUND]]** - When rounding is appropriate

*Compare truncation vs rounding:*
```
=TRUNC(2.789, 1)  → 2.7
=ROUND(2.789, 1)  → 2.8
```
TRUNC cuts off, ROUND rounds to nearest.

---

**[[MOD]]** - Get the truncated portion

*Decompose a number:*
```
Integer part: =TRUNC(A1)
Decimal part: =A1 - TRUNC(A1)   or   =MOD(A1, 1)
```
Separate whole and fractional parts.

---

**[[TEXT]]** - Format truncated values

*Display with fixed decimals:*
```
=TEXT(TRUNC(A1, 2), "0.00")
```
Ensures display shows exactly 2 decimal places.

---

**[[ABS]]** - Magnitude truncation

*Truncate magnitude, preserve sign:*
```
=SIGN(A1) * TRUNC(ABS(A1), 2)
```
Sometimes clearer for complex precision requirements.

## Official Documentation

- **Microsoft Excel:** [TRUNC function](https://support.microsoft.com/en-us/office/trunc-function-8b86a64c-3127-43db-ba14-aa5ceb292721)
- **Google Sheets:** [TRUNC function](https://support.google.com/docs/answer/3093588)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 (1985) | Original function |
| Excel 2007+ | All subsequent | No changes to behavior |
| Google Sheets | Original launch | Identical to Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
