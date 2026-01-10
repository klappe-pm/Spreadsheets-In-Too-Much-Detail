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
- repetition
- counting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Permutations with Repetition
- Permutations Allowing Repeats
- nPr with Replacement
tags:
- permutations
- repetition
- combinatorics
- counting
- arrangements
- probability
---

# PERMUTATIONA

## Description

**PERMUTATIONA** calculates the number of permutations—ordered arrangements—when repetition is allowed. The formula is simply n^k: you can choose any of n items for each of k positions, and items can be reused. This differs from PERMUT, which assumes each item can only be used once.

Use PERMUTATIONA when **order matters AND items can be repeated**. Classic examples include: PIN codes (each digit can appear multiple times), combination locks (numbers can repeat), passwords where characters can repeat, or multiple-choice tests where the same answer can be used repeatedly. Any situation where you're selecting with replacement from a set.

The calculation is elegantly simple: n^k. For a 4-digit PIN using digits 0-9, you have 10 choices for each of 4 positions: 10^4 = 10,000 possibilities. Compare this to PERMUT(10,4) = 5,040 when digits cannot repeat—allowing repetition nearly doubles the possibilities.

**Key distinction from PERMUT:** PERMUTATIONA allows repetition (n^k), while PERMUT prohibits it (n!/(n-k)!). With PERMUTATIONA, k can exceed n—you can arrange more positions than available items because items are reusable. A 10-digit code from 4 symbols is valid with PERMUTATIONA but impossible with PERMUT.

## Syntax

> [!f(x)] PERMUTATIONA Syntax
>
> ```
> =PERMUTATIONA(number, number_chosen)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The number of distinct items to choose from (n). Must be a non-negative integer. |
| `number_chosen` | Yes | The number of positions to fill (k). Must be a non-negative integer. Can be greater than number. |

### Return Value

Returns an integer equal to n^k representing permutations with repetition allowed. Returns #NUM! if either parameter is negative. Returns #VALUE! if parameters are non-numeric. For large values, returns scientific notation.

## Examples

> [!f(x)] PERMUTATIONA Examples

### Example 1: Basic PIN Code
```
=PERMUTATIONA(10, 4)
```
**Result:** 10000

**Explanation:** 4-digit PIN using digits 0-9, digits can repeat: 10^4 = 10,000 possible PINs.

---

### Example 2: Binary Sequences
```
=PERMUTATIONA(2, 8)
```
**Result:** 256

**Explanation:** 8-bit binary number: 2^8 = 256 possible values (0-255).

---

### Example 3: Combination Lock
```
=PERMUTATIONA(40, 3)
```
**Result:** 64000

**Explanation:** 3-number combination from 40 numbers (0-39), numbers can repeat: 40^3 = 64,000 combinations.

---

### Example 4: Comparing with PERMUT
```
Without repetition: =PERMUT(10, 4)      → 5040
With repetition:    =PERMUTATIONA(10, 4) → 10000
```
**Result:** Repetition nearly doubles possibilities

**Explanation:** Allowing repetition increases permutation count significantly.

---

### Example 5: Multiple Choice Test
```
=PERMUTATIONA(4, 20)
```
**Result:** 1.10E+12 (1,099,511,627,776)

**Explanation:** 20 questions with 4 choices each: 4^20 possible answer combinations.

---

### Example 6: DNA Sequences
```
=PERMUTATIONA(4, 10)
```
**Result:** 1048576

**Explanation:** 10-base DNA sequence using 4 nucleotides (A, T, G, C): 4^10 = 1,048,576 possible sequences.

---

### Example 7: Password with Repetition
```
=PERMUTATIONA(26, 6)
```
**Result:** 308915776

**Explanation:** 6-letter lowercase password, letters can repeat: 26^6 = 308,915,776 possibilities.

---

### Example 8: More Positions Than Items
```
=PERMUTATIONA(3, 5)
```
**Result:** 243

**Explanation:** 5 positions filled from 3 items with repetition: 3^5 = 243. Valid because items can repeat.

---

### Example 9: Single Item Multiple Positions
```
=PERMUTATIONA(1, 10)
```
**Result:** 1

**Explanation:** Only one item available, repeated 10 times: 1^10 = 1 way (all positions identical).

---

### Example 10: Zero Positions
```
=PERMUTATIONA(5, 0)
```
**Result:** 1

**Explanation:** Filling 0 positions has exactly 1 way: do nothing. Any number to power of 0 equals 1.

---

### Example 11: License Plates
```
=PERMUTATIONA(26, 3) * PERMUTATIONA(10, 4)
```
**Result:** 175760000

**Explanation:** 3 letters followed by 4 digits, all can repeat: 26^3 * 10^4 = 17,576 * 10,000.

---

### Example 12: Color Combinations
```
=PERMUTATIONA(8, 3)
```
**Result:** 512

**Explanation:** Selecting 3 colors from 8, order matters, colors can repeat: 8^3 = 512 arrangements.

---

### Example 13: Dice Rolling Outcomes
```
=PERMUTATIONA(6, 2)
```
**Result:** 36

**Explanation:** Rolling 2 dice: each die has 6 outcomes, total = 6^2 = 36 ordered pairs.

---

### Example 14: ASCII Character Strings
```
=PERMUTATIONA(128, 8)
```
**Result:** 7.21E+16

**Explanation:** 8-character ASCII strings: 128^8 = 72,057,594,037,927,936 possibilities.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Negative parameter | Both n and k must be non-negative. Check input values. |
| `#VALUE!` | Non-numeric argument | Ensure parameters are numbers or numeric cell references. |
| `#NAME?` | Misspelled function | Check spelling: PERMUTATIONA, not PERMUTATIONSA or PERMUTAWITHA. |
| `Overflow` | Result too large | n^k grows extremely fast. Very large k causes overflow. Consider logarithms. |
| `Wrong function` | Confusion with PERMUT | Use PERMUTATIONA when repetition allowed, PERMUT when prohibited. |

## Use Cases

### [[Password and Security Analysis]]

**Scenario:** Calculate password space when characters can repeat.

**Implementation:**
```
=PERMUTATIONA(26, 8)                → 8 lowercase letters (209 billion)
=PERMUTATIONA(62, 8)                → Alphanumeric (218 trillion)
=PERMUTATIONA(95, 12)               → All printable ASCII, 12 chars
=LOG10(PERMUTATIONA(62, n))         → Password strength in digits
```

**Business Application:** Security policy design, password strength estimation, brute-force attack analysis. Most real passwords allow repetition.

**Technical Details:** Password spaces grow exponentially with length. Adding characters to the set (a-z vs a-zA-Z0-9) has less impact than adding length.

---

### [[Genetic and Molecular Analysis]]

**Scenario:** Calculate possible sequences in molecular biology.

**Implementation:**
```
=PERMUTATIONA(4, Sequence_Length)   → DNA/RNA sequences
=PERMUTATIONA(20, Peptide_Length)   → Protein sequences (20 amino acids)
=PERMUTATIONA(4, 100)               → Possible 100-base DNA sequences
```

**Business Application:** Genomics research, drug design, bioinformatics. Understanding sequence space for research and analysis.

**Technical Details:** Biological sequences are ordered with repetition (same base/amino acid can appear multiple times), making PERMUTATIONA appropriate.

---

### [[Access Code Generation]]

**Scenario:** Design and analyze access codes and PINs.

**Implementation:**
```
=PERMUTATIONA(10, 4)                → Standard 4-digit PIN (10,000)
=PERMUTATIONA(10, 6)                → 6-digit PIN (1,000,000)
=1/PERMUTATIONA(10, 4)              → Probability of guessing PIN
```

**Business Application:** Banking, security systems, physical access control. Balance security (more combinations) with usability (shorter codes).

**Technical Details:** Each additional digit multiplies possibilities by the base (10 for numeric). 6-digit PINs are 100x more secure than 4-digit.

---

### [[Probability Calculations]]

**Scenario:** Calculate probabilities for events with replacement.

**Implementation:**
```
=PERMUTATIONA(6, 3)                 → Possible dice outcomes (216)
=1/PERMUTATIONA(6, 3)               → Probability of specific sequence
=PERMUTATIONA(2, n)                 → Possible n-flip coin sequences
```

**Business Application:** Gaming, statistics education, risk analysis. Probability = favorable outcomes / PERMUTATIONA.

**Technical Details:** For "with replacement" probability problems, PERMUTATIONA gives the sample space size. Divide favorable outcomes by this for probability.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later (not available in Excel 2010 and earlier)
- **Calculation:** Simple n^k implementation
- **Range:** Limited by floating-point precision (about 15 significant digits)
- **Performance:** Highly optimized

### Google Sheets
- **Availability:** All versions
- **Calculation:** Identical to Excel
- **Range:** Same practical limits
- **Performance:** Efficient for all inputs

### Both Platforms
- Identical mathematical result (n^k)
- Same error handling
- Same handling of edge cases
- No platform-specific quirks

### Compatibility Note
For Excel 2010 and earlier, PERMUTATIONA isn't available. Use `=number^number_chosen` instead, which produces identical results.

## Tips and Best Practices

1. **Choose correctly between PERMUT and PERMUTATIONA:** With repetition = PERMUTATIONA (n^k). Without repetition = PERMUT (n!/(n-k)!).

2. **Use for most password calculations:** Real passwords typically allow character repetition, making PERMUTATIONA correct.

3. **Remember it's just n^k:** PERMUTATIONA(n, k) equals n^k. You can use POWER(n, k) interchangeably.

4. **k can exceed n:** Unlike PERMUT, PERMUTATIONA allows more positions than items (because repetition is allowed).

5. **Calculate security margins:** Password security often measured in bits: LOG(PERMUTATIONA(chars, length), 2).

6. **Combine for complex scenarios:** License plates with 3 letters and 4 numbers: PERMUTATIONA(26,3) * PERMUTATIONA(10,4).

7. **Compare scenarios:** Always compare with and without repetition to understand the impact.

8. **Watch for overflow:** n^k grows extremely fast. PERMUTATIONA(100, 100) is astronomically large.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PERMUT]] | Permutations without repetition | When items cannot be reused |
| [[COMBIN]] | Combinations without repetition | When order doesn't matter, no repetition |
| [[COMBINA]] | Combinations with repetition | When order doesn't matter, with repetition |
| [[POWER]] | Exponentiation | Equivalent calculation (n^k) |

### Commonly Used Together

**[[PERMUT]]** - Compare with/without repetition

*Understand repetition impact:*
```
Without: =PERMUT(10, 4)       → 5040
With:    =PERMUTATIONA(10, 4) → 10000
```
Repetition nearly doubles possibilities with these parameters.

---

**[[POWER]]** - Equivalent calculation

*Alternative formula:*
```
=PERMUTATIONA(n, k)
=POWER(n, k)
=n^k
```
All three produce identical results.

---

**[[LOG]]** - Strength measurement

*Password strength in bits:*
```
=LOG(PERMUTATIONA(62, 8), 2)  → About 47.6 bits of entropy
=LOG(PERMUTATIONA(95, 12), 2) → About 78.8 bits of entropy
```
More bits = stronger password.

---

**[[PRODUCT]]** - Complex scenarios

*Multi-part codes:*
```
=PERMUTATIONA(26, 3) * PERMUTATIONA(10, 3)
```
Combine different sections with different character sets.

---

**[[COMBIN]]** and **[[COMBINA]]** - Order doesn't matter

*When arrangement is irrelevant:*
```
Order matters, with rep:    PERMUTATIONA(n, k)
Order matters, no rep:      PERMUT(n, k)
Order doesn't matter, no:   COMBIN(n, k)
Order doesn't matter, with: COMBINA(n, k)
```
Complete decision tree for counting problems.

## Official Documentation

- **Microsoft Excel:** [PERMUTATIONA function](https://support.microsoft.com/en-us/office/permutationa-function-6c7d7fdc-d657-44e6-aa19-2857b25cae4e)
- **Google Sheets:** [PERMUTATIONA function](https://support.google.com/docs/answer/9116331)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | New function (not in Excel 2010) |
| Excel 2013+ | All subsequent | No changes |
| Google Sheets | Original launch | Available from beginning |
| LibreOffice Calc | Recent versions | Compatible implementation |

---

*Last updated: 2026-01-10*
