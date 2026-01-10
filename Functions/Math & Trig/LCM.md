---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- multiple
- common-multiple
- synchronization
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Least Common Multiple
- Lowest Common Multiple
- Smallest Common Multiple
tags:
- lcm
- multiple
- synchronization
- scheduling
- number-theory
---

# LCM

## Description

**LCM** returns the least common multiple of two or more integers—the smallest positive integer that is divisible by all the given numbers. LCM(4, 6) returns 12 because 12 is the smallest number divisible by both 4 (12/4=3) and 6 (12/6=2). LCM answers "when will these cycles align?" questions.

This function is essential for **synchronization problems, scheduling, and fraction arithmetic**. When will two maintenance schedules coincide? LCM of their intervals. What's the common denominator for 1/4 + 1/6? LCM(4,6) = 12. LCM finds the smallest container that holds complete multiples of all input quantities.

**LCM works with multiple numbers.** LCM(4, 6, 9) returns 36—the smallest number divisible by all three. Like GCD, LCM accepts up to 255 arguments and works with ranges. The mathematical relationship is: GCD(a,b) * LCM(a,b) = a * b. You can calculate LCM from GCD: LCM(a,b) = (a*b)/GCD(a,b).

LCM grows quickly with more arguments. While GCD shrinks (or stays the same) as you add numbers, LCM typically grows. LCM(2,3,4,5,6,7,8,9,10) = 2520. For many numbers, LCM can become very large, potentially exceeding spreadsheet numeric limits.

## Syntax

> [!f(x)] LCM Syntax
>
> ```
> =LCM(number1, [number2], ...)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number1` | Yes | The first positive integer (or range). Decimals are truncated. Must be non-negative. |
| `number2, ...` | No | Additional integers to include. Up to 255 arguments. All must be non-negative after truncation. |

### Return Value

Returns the least common multiple—the smallest positive integer divisible by all arguments. Returns 0 if any argument is 0. Returns #VALUE! for non-numeric input. Returns #NUM! for negative numbers.

## Examples

> [!f(x)] LCM Examples

### Example 1: Two Numbers
```
=LCM(4, 6)
```
**Result:** 12

**Explanation:** 12 is divisible by both 4 and 6. No smaller positive number is.

---

### Example 2: Coprime Numbers
```
=LCM(8, 15)
```
**Result:** 120

**Explanation:** Since 8 and 15 share no common factors, LCM = 8 * 15 = 120.

---

### Example 3: One Divides the Other
```
=LCM(4, 12)
```
**Result:** 12

**Explanation:** When one number divides the other evenly, LCM is the larger number.

---

### Example 4: Three Numbers
```
=LCM(4, 6, 9)
```
**Result:** 36

**Explanation:** 36 is the smallest number divisible by 4 (9 times), 6 (6 times), and 9 (4 times).

---

### Example 5: Finding Common Denominator
```
=LCM(Denom1, Denom2)
```
**Result:** Common denominator for fractions

**Explanation:** For 1/4 + 1/6: LCM(4,6)=12. Convert to 3/12 + 2/12 = 5/12.

---

### Example 6: Same Number Twice
```
=LCM(7, 7)
```
**Result:** 7

**Explanation:** Any number's LCM with itself is that number.

---

### Example 7: Including 1
```
=LCM(12, 1)
```
**Result:** 12

**Explanation:** LCM(n, 1) = n. Every number is a multiple of 1.

---

### Example 8: Including Zero
```
=LCM(12, 0)
```
**Result:** 0

**Explanation:** LCM with 0 is 0 by convention (some consider it undefined).

---

### Example 9: Using a Range
```
=LCM(A1:A5)
```
**Result:** LCM of all values in range

**Explanation:** If A1:A5 contains {2, 3, 4, 5, 6}, LCM returns 60.

---

### Example 10: Schedule Synchronization
```
=LCM(DaysInterval1, DaysInterval2)
```
**Result:** Days until both schedules align

**Explanation:** Maintenance every 12 days and 15 days: LCM(12,15)=60 days until same-day maintenance.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input | Ensure all arguments are numbers or numeric cell references. |
| `#NUM!` | Negative number | LCM requires non-negative integers. Use ABS() for potentially negative values. |
| `#NUM!` | Result too large | LCM can grow very large; may exceed spreadsheet limits. Consider if you really need all those numbers. |
| `#NAME?` | Misspelled function name | Verify spelling: LCM, not LCF or LOWEST. |
| `Result of 0` | Zero in arguments | LCM with 0 returns 0. Check for zeros if unexpected. |

## Use Cases

### [[Scheduling and Synchronization]]

**Scenario:** Determine when periodic events will coincide.

**Implementation:**
```
=LCM(CycleA, CycleB)                               → Days until both events occur together
=LCM(ShiftHours, TaskHours)                        → Hours until shift/task alignment
=LCM(A2:A10)                                       → Common cycle for multiple schedules
```

**Business Application:** Maintenance scheduling, production planning, meeting scheduling, project coordination.

**Technical Details:** If events occur every A and B units, they coincide every LCM(A,B) units. For events that started at different times, add appropriate offset.

---

### [[Fraction Arithmetic]]

**Scenario:** Find common denominators for adding or comparing fractions.

**Implementation:**
```
=LCM(Denom1, Denom2)                               → Common denominator
=Num1 * (LCM/Denom1) + Num2 * (LCM/Denom2)         → Numerator sum
=(A1/A2) = (A1*(LCM/A2))/(A2*(LCM/A2))             → Convert to common denom
```

**Business Application:** Financial calculations requiring exact fractions, recipe scaling, measurement conversions.

**Technical Details:** To add a/b + c/d: common denominator is LCM(b,d). Convert each fraction, then add numerators.

---

### [[Pattern and Cycle Analysis]]

**Scenario:** Find repeating patterns in data or determine cycle lengths.

**Implementation:**
```
=LCM(PeriodA, PeriodB)                             → Combined period
=LCM(WavelengthA, WavelengthB)                     → Beat frequency period
=LCM(GearTeethA, GearTeethB)                       → Full rotation alignment
```

**Business Application:** Signal processing, mechanical engineering, music (beat frequencies), astronomy (planetary alignments).

**Technical Details:** When two waves or cycles combine, the combined pattern repeats after LCM of individual periods. Used in Moire pattern analysis and interference calculations.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2000+ (originally Analysis ToolPak), built-in since Excel 2007
- **Arguments:** Up to 255 numbers/ranges
- **Decimal handling:** Truncated to integers
- **Overflow:** Returns #NUM! if result exceeds limits

### Google Sheets
- **Availability:** All versions
- **Arguments:** Similar limit
- **Decimal handling:** Same truncation
- **Overflow:** Same behavior

### Both Platforms
- Identical results for same inputs
- Same LCM(n, 0) = 0 convention
- Same truncation of decimals
- Same numeric limits

## Tips and Best Practices

1. **LCM for common denominators:** To add fractions with denominators a and b, use LCM(a,b) as the common denominator.

2. **Relationship with GCD:** LCM(a,b) = (a*b)/GCD(a,b). Use this if you need to verify results or if one function isn't available.

3. **LCM grows quickly:** Unlike GCD which shrinks, LCM typically grows as you add more numbers. Watch for overflow with many inputs.

4. **LCM(a,b) >= max(a,b):** The LCM is always at least as large as the larger input. When one divides the other, LCM equals the larger.

5. **Zero handling:** LCM with 0 returns 0. If this isn't desired behavior, filter out zeros first.

6. **Use for scheduling:** "When will schedules A and B next coincide?" is answered by LCM(A,B) time units from when they last aligned.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[GCD]] | Greatest common divisor | For finding largest common factor instead of smallest common multiple |
| [[MOD]] | Remainder after division | For checking divisibility or cycling |
| [[PRODUCT]] | Multiply all values | When you need product, not LCM (different for non-coprime numbers) |

### Commonly Used Together

**[[GCD]]** - Partner function

*Related calculations:*
```
=GCD(A1, B1)                     → Greatest common divisor
=A1 * B1 / GCD(A1, B1)           → Same as LCM (identity)
```
GCD and LCM are mathematically linked.

---

**[[MOD]]** - Verify divisibility

*Check LCM correctness:*
```
=AND(MOD(LCM(A1,B1),A1)=0, MOD(LCM(A1,B1),B1)=0)
```
True if LCM is indeed divisible by both.

---

**[[PRODUCT]]** - Upper bound for LCM

*LCM never exceeds product:*
```
=PRODUCT(Range) >= LCM(Range)    → Always true
=PRODUCT(A1,B1) / GCD(A1,B1)     → Equals LCM(A1,B1)
```
For coprime numbers, LCM equals product.

---

**[[ABS]]** - Handle negatives

*Safe LCM calculation:*
```
=LCM(ABS(A1), ABS(B1))
```
Convert negatives to positives before LCM.

---

**[[QUOTIENT]]** - How many cycles

*Cycles to reach LCM:*
```
=LCM(A1,B1)/A1    → Cycles of A until sync
=LCM(A1,B1)/B1    → Cycles of B until sync
```
Shows how many complete cycles each period goes through.

## Official Documentation

- **Microsoft Excel:** [LCM function](https://support.microsoft.com/en-us/office/lcm-function-7152b67a-8bb5-4075-ae5c-06ede5563c94)
- **Google Sheets:** [LCM function](https://support.google.com/docs/answer/3093421)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Originally in Analysis ToolPak |
| Excel 2007+ | Built-in | No longer requires add-in |
| Google Sheets | Original launch | Always built-in |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
