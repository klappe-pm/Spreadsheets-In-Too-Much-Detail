---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- permutations
- combinatorics
- counting
- probability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Permutations
- Ordered Arrangements
- nPr
tags:
- permutations
- combinatorics
- counting
- arrangements
- probability
- factorial
---

# PERMUT

## Description

**PERMUT** calculates the number of permutations—ordered arrangements—of selecting k objects from a set of n objects without repetition. The formula is n!/(n-k)!, commonly written as nPr or P(n,k). Unlike combinations (COMBIN), permutations care about order: selecting A then B is different from selecting B then A.

Use PERMUT when the **order of selection matters and items cannot be reused**. Classic examples include: assigning gold, silver, and bronze medals to 10 competitors (order matters: who gets gold vs silver); arranging books on a shelf; determining possible finish orders in a race; or calculating password possibilities where characters cannot repeat.

The distinction between permutations and combinations is fundamental: PERMUT(5,3) = 60 different ordered arrangements, while COMBIN(5,3) = 10 unordered groups. Permutations always produce larger numbers because each combination can be arranged in multiple ways. The relationship is: PERMUT(n,k) = COMBIN(n,k) * FACT(k).

**Key constraint: k cannot exceed n**, since you cannot select more items than exist in the set (no repetition allowed). Both n and k must be non-negative integers. For permutations where repetition is allowed (like a 4-digit PIN where digits can repeat), use PERMUTATIONA instead.

## Syntax

> [!f(x)] PERMUT Syntax
>
> ```
> =PERMUT(number, number_chosen)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The total number of objects (n). Must be a non-negative integer. This is the size of the set you're selecting from. |
| `number_chosen` | Yes | The number of objects to arrange (k). Must be a non-negative integer and cannot exceed number. |

### Return Value

Returns an integer representing the number of permutations: n!/(n-k)!. Returns #NUM! if number_chosen > number or if either parameter is negative. Returns #VALUE! if parameters are non-numeric. For large values, may return scientific notation.

## Examples

> [!f(x)] PERMUT Examples

### Example 1: Basic Permutation
```
=PERMUT(5, 3)
```
**Result:** 60

**Explanation:** Ways to arrange 3 items from 5: 5!/(5-3)! = 5!/2! = 120/2 = 60 different ordered arrangements.

---

### Example 2: Medal Assignments
```
=PERMUT(10, 3)
```
**Result:** 720

**Explanation:** Assigning gold, silver, bronze to 10 athletes. Order matters: 10 * 9 * 8 = 720 possible podium arrangements.

---

### Example 3: All Items Selected
```
=PERMUT(4, 4)
```
**Result:** 24

**Explanation:** Arranging all 4 items: 4! = 24. When k=n, PERMUT equals FACT(n).

---

### Example 4: Single Selection
```
=PERMUT(10, 1)
```
**Result:** 10

**Explanation:** Choosing 1 item from 10: simply 10 ways. No arrangement to consider with single item.

---

### Example 5: Zero Selection
```
=PERMUT(5, 0)
```
**Result:** 1

**Explanation:** Selecting nothing has exactly one way to do it—select nothing. Mathematical convention: 0! = 1.

---

### Example 6: Race Finish Order
```
=PERMUT(8, 8)
```
**Result:** 40320

**Explanation:** Possible finish orders for 8 runners: 8! = 40,320 different race outcomes.

---

### Example 7: Phone Number Arrangements
```
=PERMUT(10, 7)
```
**Result:** 604800

**Explanation:** 7-digit arrangements using digits 0-9 without repetition. With repetition allowed, it would be 10^7 = 10 million (use PERMUTATIONA).

---

### Example 8: Password Without Repeating Letters
```
=PERMUT(26, 4)
```
**Result:** 358800

**Explanation:** 4-letter passwords from 26 letters, no letter repeated: 26 * 25 * 24 * 23 = 358,800 possibilities.

---

### Example 9: Comparing with COMBIN
```
PERMUT(5, 3)  = 60
COMBIN(5, 3)  = 10
PERMUT/COMBIN = 6 = FACT(3)
```
**Result:** Relationship between permutations and combinations

**Explanation:** Each combination of 3 items can be arranged 3! = 6 ways. PERMUT = COMBIN * FACT(k).

---

### Example 10: Seating Arrangements
```
=PERMUT(12, 5)
```
**Result:** 95040

**Explanation:** Seating 5 people in a row of 12 chairs: 12 * 11 * 10 * 9 * 8 = 95,040 arrangements.

---

### Example 11: Election Positions
```
=PERMUT(15, 4)
```
**Result:** 32760

**Explanation:** Electing president, VP, secretary, treasurer from 15 candidates. Each position is distinct: 15 * 14 * 13 * 12 = 32,760.

---

### Example 12: Maximum Equal to Total
```
=PERMUT(6, 6)
```
**Result:** 720

**Explanation:** All possible orderings of 6 items = 6! = 720. This is equivalent to FACT(6).

---

### Example 13: Large Numbers
```
=PERMUT(20, 10)
```
**Result:** 6.7E+11 (670,442,572,800)

**Explanation:** Large permutation values grow extremely fast. 20!/(20-10)! produces a 12-digit number.

---

### Example 14: Probability Calculation
```
=1/PERMUT(6, 3)
```
**Result:** 0.00833... (1 in 120)

**Explanation:** Probability of guessing exactly the top 3 finishers in order from 6 competitors: 1/120.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | number_chosen > number | Cannot select more items than available. Ensure k <= n. |
| `#NUM!` | Negative parameter | Both n and k must be non-negative. Check for negative values. |
| `#VALUE!` | Non-numeric argument | Ensure both parameters are numbers or cell references to numbers. |
| `#NAME?` | Misspelled function | Check spelling: PERMUT, not PERMUTATE or PERMUTATION. |
| `Overflow` | Result too large | Very large permutations may exceed Excel's number capacity. Consider logarithms or approximations. |

## Use Cases

### [[Sports and Competition Analysis]]

**Scenario:** Calculate possible outcomes in competitions where finishing order matters.

**Implementation:**
```
=PERMUT(Competitors, Positions)
=PERMUT(10, 3)                     → Medal possibilities (720)
=PERMUT(20, 5)                     → Top 5 finish orders
=1/PERMUT(8, 3)                    → Probability of predicting exact top 3
```

**Business Application:** Sports betting, tournament planning, award ceremonies. Understand the odds of specific outcome predictions.

**Technical Details:** For races, PERMUT gives exact finish order possibilities. For "top 3 in any order," use COMBIN instead.

---

### [[Security and Password Analysis]]

**Scenario:** Calculate password or code possibilities without character repetition.

**Implementation:**
```
=PERMUT(26, 8)                     → 8-letter password, no repeats
=PERMUT(36, 6)                     → Alphanumeric, no repeats
=PERMUT(10, 4)                     → 4-digit PIN, no repeated digits
```

**Business Application:** Security assessment, password policy design, access code generation. Estimate brute-force attack difficulty.

**Technical Details:** If repetition is allowed (most passwords), use PERMUTATIONA or simple exponentiation (26^8). PERMUT applies only to no-repeat scenarios.

---

### [[Scheduling and Assignment Problems]]

**Scenario:** Calculate possible ways to assign tasks, shifts, or positions.

**Implementation:**
```
=PERMUT(Employees, Shifts)
=PERMUT(10, 5)                     → Assign 5 shifts to 10 employees
=PERMUT(Projects, Teams)           → Unique project assignments
```

**Business Application:** Workforce scheduling, project assignment, resource allocation. Understand the solution space for optimization problems.

**Technical Details:** When assignments have no ordering (team A vs team B doesn't matter), use COMBIN. When roles are distinct (manager, lead, analyst), use PERMUT.

---

### [[Probability and Statistics Education]]

**Scenario:** Teach and demonstrate combinatorial concepts.

**Implementation:**
```
=PERMUT(n, k)                      → Direct permutation calculation
=FACT(n)/FACT(n-k)                 → Equivalent formula
=PERMUT(n, k)/FACT(k)              → Verify equals COMBIN(n, k)
=PERMUT(n, n)                      → Verify equals FACT(n)
```

**Business Application:** Academic instruction, textbook examples, exam problems. Demonstrate relationships between combinatorial formulas.

**Technical Details:** PERMUT provides quick answers for homework problems and allows verification of manual calculations.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 2003
- **Integer handling:** Truncates decimal inputs to integers
- **Range:** Handles values up to floating-point limits (approximately 170! before overflow)
- **Performance:** Highly optimized calculation

### Google Sheets
- **Availability:** All versions
- **Integer handling:** Same truncation behavior as Excel
- **Range:** Same practical limits as Excel
- **Performance:** Efficient for all reasonable inputs

### Both Platforms
- Identical mathematical calculation
- Same error conditions
- Same handling of edge cases (k=0 returns 1)
- Same truncation of non-integer inputs

## Tips and Best Practices

1. **Distinguish from combinations:** PERMUT counts ordered arrangements; COMBIN counts unordered selections. Ask "does order matter?" to choose correctly.

2. **Verify with relationship:** PERMUT(n,k) should equal COMBIN(n,k) * FACT(k). Use this to verify your choice of function.

3. **Use for sequential selection:** When selection is sequential (first place, second place), PERMUT is appropriate.

4. **Watch for repetition:** If items can repeat (like digits in a PIN), PERMUT is wrong—use PERMUTATIONA or simple powers.

5. **Calculate probabilities:** 1/PERMUT(n,k) gives probability of guessing exact ordered selection.

6. **Understand growth:** Permutations grow faster than combinations. PERMUT(100,10) is enormously larger than COMBIN(100,10).

7. **Simplify when possible:** PERMUT(n,1) = n. PERMUT(n,n) = FACT(n). PERMUT(n,0) = 1. Use these shortcuts for verification.

8. **Consider alternative formulas:** For very large n and small k, n * (n-1) * (n-2) * ... may be clearer than PERMUT.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERMUTATIONA]] | Permutations with repetition | When items can be reused (like PIN digits) |
| [[COMBIN]] | Combinations without repetition | When order doesn't matter |
| [[COMBINA]] | Combinations with repetition | When order doesn't matter and items can repeat |
| [[FACT]] | Factorial | When arranging all items (k = n) |

### Commonly Used Together

**[[COMBIN]]** - Verify relationship

*Check permutation/combination relationship:*
```
=PERMUT(10, 3)                → 720
=COMBIN(10, 3) * FACT(3)      → 120 * 6 = 720
```
PERMUT = COMBIN * FACT(k) always.

---

**[[FACT]]** - Alternative calculation

*Manual permutation formula:*
```
=FACT(n)/FACT(n-k)
=FACT(10)/FACT(7)             → Same as PERMUT(10, 3) = 720
```
Useful for understanding the underlying math.

---

**[[PERMUTATIONA]]** - With repetition

*Compare with/without repetition:*
```
Without: =PERMUT(10, 4)       → 5040 (no digit repeats)
With:    =PERMUTATIONA(10, 4) → 10000 (digits can repeat)
```
Choose based on whether repetition is allowed.

---

**[[PRODUCT]]** - Sequential multiplication

*Alternative calculation:*
```
=PRODUCT(7,8,9,10)            → Same as PERMUT(10, 4) = 5040
=PERMUT(10, 4)                → 10 * 9 * 8 * 7 = 5040
```
Sometimes clearer for small k values.

---

**[[IF]]** - Conditional permutation problems

*Problem-specific logic:*
```
=IF(Positions<=Candidates, PERMUT(Candidates, Positions), "Not enough candidates")
```
Handle edge cases in practical applications.

## Official Documentation

- **Microsoft Excel:** [PERMUT function](https://support.microsoft.com/en-us/office/permut-function-3bd1cb9a-2880-41ab-a197-f246a7a602d3)
- **Google Sheets:** [PERMUT function](https://support.google.com/docs/answer/3094016)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2003 | Original implementation |
| Excel 2007+ | All subsequent | No changes |
| Google Sheets | Original launch | Full compatibility with Excel |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
