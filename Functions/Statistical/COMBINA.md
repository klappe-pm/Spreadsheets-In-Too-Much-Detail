---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- combinations
- combinatorics
- counting
- multiset
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Combinations with Repetition
- Multiset Coefficient
- Stars and Bars
- k-combinations with repetition
tags:
- statistical
- combinatorics
- combinations
- counting
- probability
---

# COMBINA

## Description

**COMBINA** calculates the number of combinations with repetition (also called multiset coefficients or k-multicombinations), answering the question: "How many ways can I choose k items from n types when I can choose the same type multiple times and order doesn't matter?" This differs from regular combinations (COMBIN) where each item can only be selected once. COMBINA is used when you're selecting from categories that can be repeated, such as choosing 5 donuts from 10 flavors where you can pick the same flavor multiple times.

The mathematical formula is: COMBINA(n, k) = C(n + k - 1, k) = (n + k - 1)! / (k! * (n - 1)!). This elegant result comes from a technique called "stars and bars" in combinatorics, where we imagine placing k identical items into n distinct bins. The number of ways to do this equals the number of ways to arrange k stars and n-1 bars (dividers), which is choosing k positions from n+k-1 total positions.

**Distinction from COMBIN:** COMBIN counts combinations without repetition - selecting k distinct items from n different items. COMBINA allows repetition - selecting k items from n types where each type can be chosen multiple times. For example, choosing 3 people from a group of 10 uses COMBIN(10,3) = 120. Choosing 3 scoops of ice cream from 10 flavors (allowing repeats) uses COMBINA(10,3) = 220. The repetition-allowed version always yields larger or equal counts.

**Practical applications:** COMBINA appears in probability calculations involving sampling with replacement, distribution problems (how many ways to distribute identical items into distinct categories), and polynomial coefficient calculations. It's also fundamental in partition theory and generating function methods. Despite its mathematical elegance, COMBINA is less commonly used than COMBIN because many real-world selection problems don't allow repetition.

## Syntax

> [!f(x)] COMBINA Syntax
>
> ```
> =COMBINA(number, number_chosen)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The number of types or categories to choose from. Must be a non-negative integer. Represents n in the formula. |
| `number_chosen` | Yes | The number of items to select (with possible repetitions). Must be a non-negative integer. Represents k in the formula. |

### Return Value

Returns the number of combinations with repetition, always a positive integer for valid inputs. Returns #NUM! if either parameter is negative or if values exceed computational limits. Returns 1 if number_chosen is 0 (one way to choose nothing).

## Examples

> [!f(x)] COMBINA Examples

### Example 1: Basic Ice Cream Selection
```
=COMBINA(10, 3)
```
**Result:** 220

**Explanation:** Choosing 3 scoops from 10 flavors (repetition allowed). You could get 3 vanilla, or 2 chocolate + 1 strawberry, etc. There are 220 distinct combinations.

---

### Example 2: Comparison with COMBIN
```
=COMBINA(5, 3)  Result: 35
=COMBIN(5, 3)   Result: 10
```
**Result:** COMBINA gives more combinations than COMBIN

**Explanation:** With 5 types and 3 choices, allowing repetition gives 35 combinations versus 10 without repetition. The difference grows larger with more selections.

---

### Example 3: Choosing Nothing
```
=COMBINA(10, 0)
```
**Result:** 1

**Explanation:** There's exactly one way to choose zero items: choose nothing. This matches the mathematical convention that C(n-1+0, 0) = 1.

---

### Example 4: One Type Available
```
=COMBINA(1, 5)
```
**Result:** 1

**Explanation:** With only one type available, there's only one way to choose 5 items: pick that type 5 times. No variety possible.

---

### Example 5: Equal n and k
```
=COMBINA(4, 4)
```
**Result:** 35

**Explanation:** Choosing 4 items from 4 types with repetition. Includes possibilities like (1,1,1,1), (2,1,1,0), (2,2,0,0), (3,1,0,0), (4,0,0,0), and all other distribution patterns.

---

### Example 6: Large Numbers
```
=COMBINA(20, 10)
```
**Result:** 20,030,010

**Explanation:** Selecting 10 items from 20 categories with repetition produces over 20 million combinations. The formula C(29,10) = 29!/(10!*19!) yields this large count.

---

### Example 7: Distributing Identical Objects
```
=COMBINA(5, 8)
```
**Result:** 495

**Explanation:** How many ways can 8 identical balls be placed into 5 distinct boxes? This is the classic "stars and bars" problem: 495 distributions.

---

### Example 8: Dice Probability Calculation
```
=COMBINA(6, 3)
```
**Result:** 56

**Explanation:** Number of distinct unordered outcomes when rolling 3 dice (considering only which faces appear, not which die shows which). There are 56 distinct multisets of 3 values from {1,2,3,4,5,6}.

---

### Example 9: Polynomial Coefficient
```
=COMBINA(3, 4)
```
**Result:** 15

**Explanation:** The number of terms in the expansion of (x + y + z)^4 equals COMBINA(3, 4) = 15. Each term corresponds to a way of distributing 4 "powers" among 3 variables.

---

### Example 10: Verifying the Formula
```
=COMBINA(7, 4)
Manual: =COMBIN(7+4-1, 4) = =COMBIN(10, 4)
```
**Result:** Both return 210

**Explanation:** COMBINA(n, k) = COMBIN(n+k-1, k). This verifies the mathematical relationship and can be used to calculate COMBINA in older software lacking the function.

---

### Example 11: Menu Selection Problem
```
=COMBINA(8, 5)
```
**Result:** 792

**Explanation:** A restaurant offers 8 side dishes. How many ways can a customer order 5 sides (with repeats allowed)? 792 different combinations, from "5 of the same" to "5 different."

---

### Example 12: Sampling with Replacement
```
=COMBINA(52, 5)
```
**Result:** 3,819,816

**Explanation:** Drawing 5 cards from a standard deck with replacement, counting only the unordered set of cards (not which draw produced which card). Much larger than COMBIN(52,5) = 2,598,960 without replacement.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | number is negative | The number of types must be >= 0. Check your input. |
| `#NUM!` | number_chosen is negative | Cannot choose negative items. Use 0 for "choose nothing." |
| `#NUM!` | Result exceeds maximum value | Very large combinations overflow. Approximate using logarithms: =EXP(GAMMALN(n+k)-GAMMALN(k+1)-GAMMALN(n)). |
| `#VALUE!` | Non-numeric arguments | Both parameters must be numbers. Remove text or convert. |
| `Unexpected large result` | Confused COMBINA with COMBIN | COMBINA allows repetition and gives larger counts. Verify which function is appropriate. |

## Use Cases

### [[Inventory Variety Planning]]

**Scenario:** A retail manager determines how many distinct product mix combinations are possible when ordering from suppliers.

**Implementation:**
```
=COMBINA(number_of_products, units_to_order)
```

**Business Application:** Understanding combination counts helps assess variety. If ordering 20 units from 15 product types (with repeats allowed), COMBINA(15, 20) = 817,190 distinct mixes are possible. This informs inventory system complexity and helps justify using optimization algorithms rather than manual selection.

**Technical Details:** This model assumes products are interchangeable in terms of order quantity (any product can be ordered 0 to 20 times). Constraints like minimum order quantities or inventory limits reduce actual feasible combinations below the COMBINA count.

---

### [[Probability Theory: Sampling with Replacement]]

**Scenario:** A statistician calculates the probability distribution of outcomes when sampling with replacement from a finite population.

**Implementation:**
```
Number of distinct unordered samples: =COMBINA(population_size, sample_size)
```

**Business Application:** When simulating draws with replacement (e.g., bootstrap sampling), understanding the number of distinct samples informs computational requirements. For 30 items sampled 30 times with replacement, COMBINA(30, 30) = 5.9 trillion distinct multisets, explaining why bootstrap methods randomly sample rather than enumerate all possibilities.

**Technical Details:** The probability of a specific multiset is not uniform. Multisets with more repetition are less likely than those with all distinct values. Exact probability requires counting permutations: P(multiset) = (k! / product of factorials of multiplicities) / n^k.

---

### [[Polynomial Expansion Term Counting]]

**Scenario:** A mathematician or programmer needs to know how many terms appear in a multinomial expansion.

**Implementation:**
```
Number of terms in (x1 + x2 + ... + xn)^k: =COMBINA(n, k)
```

**Business Application:** In computational algebra and optimization, knowing the number of terms helps estimate computational complexity. Machine learning algorithms involving polynomial features need COMBINA to calculate feature space dimensionality. For 10 features with degree-3 polynomials, COMBINA(10, 3) = 220 terms.

**Technical Details:** Each term corresponds to a way of distributing k powers among n variables. The coefficients are multinomial coefficients. For generating all terms programmatically, use recursive algorithms or integer partition methods.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2013 and later
- **Maximum values:** Limited by double-precision floating point (~10^308)
- **Truncation:** Non-integer arguments are truncated toward zero
- **Precision:** 15 significant digits for result
- **Alternative for older versions:** =COMBIN(number + number_chosen - 1, number_chosen)

### Google Sheets

- **Availability:** All versions since function introduction
- **Maximum values:** Similar limits to Excel
- **Truncation:** Non-integer arguments truncated
- **Precision:** 15 significant digits
- **Behavior:** Identical to Excel

### Key Difference Alert

COMBINA behaves identically between Excel (2013+) and Google Sheets. The primary compatibility issue is Excel 2010 and earlier, where COMBINA does not exist.

**Workaround for older Excel:**
```
=COMBIN(number + number_chosen - 1, number_chosen)
```
This is mathematically equivalent to COMBINA.

Both platforms:
- Truncate decimal inputs to integers
- Return #NUM! for negative inputs
- Handle large values until overflow occurs

## Tips and Best Practices

1. **Understand the problem type.** Use COMBINA when items can be selected multiple times and order doesn't matter (choosing flavors, distributing objects). Use COMBIN when each item can only be selected once.

2. **Remember the formula relationship.** COMBINA(n, k) = COMBIN(n+k-1, k). This helps in older software and for understanding the mathematics.

3. **Watch for large results.** Combinations grow rapidly. COMBINA(30, 30) exceeds 5 trillion. For very large values, use logarithmic approximations: ln(COMBINA) = GAMMALN(n+k) - GAMMALN(k+1) - GAMMALN(n).

4. **Verify with small examples.** For COMBINA(3, 2): the combinations are {1,1}, {1,2}, {1,3}, {2,2}, {2,3}, {3,3} = 6. Verify this matches the formula before applying to larger problems.

5. **Consider the "stars and bars" interpretation.** COMBINA(n, k) counts ways to place k identical items into n distinct bins. This visualization helps understand what you're counting.

6. **Use for polynomial complexity analysis.** When creating polynomial features in machine learning, COMBINA(features, degree) gives the number of additional terms, helping assess model complexity.

7. **Don't confuse with permutations.** COMBINA counts unordered selections with repetition. For ordered selections with repetition, use number^number_chosen instead.

8. **Test edge cases.** COMBINA(n, 0) = 1 for all n >= 0. COMBINA(1, k) = 1 for all k >= 0. COMBINA(0, 0) = 1. Verify these in your applications.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[COMBIN]] | Combinations without repetition | When each item can be selected at most once |
| [[PERMUT]] | Permutations without repetition | When order matters and no repetition |
| [[PERMUTATIONA]] | Permutations with repetition | When order matters and repetition allowed |
| [[FACT]] | Factorial | For custom combinatorial calculations |

### Commonly Used Together

**[[COMBIN]]** - Standard combinations

*Compare with and without repetition:*
```
Without repetition: =COMBIN(n, k)
With repetition: =COMBINA(n, k)
Ratio: =COMBINA(n, k) / COMBIN(n, k)
```
The ratio shows how much repetition expands the possibilities.

---

**[[GAMMALN]]** - For large value approximations

*Handle overflow with logarithms:*
```
Log of COMBINA: =GAMMALN(n+k) - GAMMALN(k+1) - GAMMALN(n)
Actual value: =EXP(GAMMALN(n+k) - GAMMALN(k+1) - GAMMALN(n))
```
Use logarithmic form for very large combinations that would overflow.

---

**[[FACT]]** - Manual calculation verification

*Verify COMBINA using factorials:*
```
=FACT(n+k-1) / (FACT(k) * FACT(n-1))
```
Should equal COMBINA(n, k) for small values (factorials overflow quickly).

## Official Documentation

- **Microsoft Excel:** [COMBINA function](https://support.microsoft.com/en-us/office/combina-function-efb49efc-4a91-4a7e-8ac3-ca18f0e0cf0c)
- **Google Sheets:** [COMBINA function](https://support.google.com/docs/answer/9366549)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New function |
| Excel 2016+ | Continued support | No changes |
| Excel 365 | Continuous updates | No functional changes |
| Google Sheets | 2019 (approximately) | Added for Excel compatibility |

---

*Last updated: 2026-01-10*
