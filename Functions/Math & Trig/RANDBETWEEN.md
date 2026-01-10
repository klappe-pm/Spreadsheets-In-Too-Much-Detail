---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- random
- integer
- range
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Random Integer
- Random Between
- Random in Range
tags:
- random
- integer
- range
- simulation
- sampling
---

# RANDBETWEEN

## Description

**RANDBETWEEN** returns a random integer between two specified values, inclusive. RANDBETWEEN(1, 6) returns 1, 2, 3, 4, 5, or 6 with equal probability—perfect for simulating a die roll. Unlike RAND() which gives decimals from 0 to 1, RANDBETWEEN gives whole numbers in exactly the range you specify. It's the go-to function for generating random integers.

This function is perfect for **discrete random values like dice, cards, random selections from numbered lists, and any scenario where you need random whole numbers**. The range is inclusive on both ends: RANDBETWEEN(1, 10) can return both 1 and 10. Each integer in the range has equal probability of being selected.

**Like RAND, RANDBETWEEN is volatile.** Every worksheet recalculation generates new random values. Edit any cell, press F9, or trigger recalculation, and all RANDBETWEEN cells get new numbers. Copy and paste values if you need to preserve the random selections.

RANDBETWEEN can work with negative numbers too: RANDBETWEEN(-10, 10) returns integers from -10 to 10 inclusive. The only requirement is that bottom must be less than or equal to top. If bottom > top, you get a #NUM! error.

## Syntax

> [!f(x)] RANDBETWEEN Syntax
>
> ```
> =RANDBETWEEN(bottom, top)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `bottom` | Yes | The smallest integer the function can return. Can be negative. Decimals are truncated to integers. |
| `top` | Yes | The largest integer the function can return. Must be greater than or equal to bottom. Decimals are truncated to integers. |

### Return Value

Returns a random integer greater than or equal to bottom and less than or equal to top. All integers in the range have equal probability. Changes on each worksheet recalculation.

## Examples

> [!f(x)] RANDBETWEEN Examples

### Example 1: Single Die Roll
```
=RANDBETWEEN(1, 6)
```
**Result:** Integer from 1 to 6

**Explanation:** Simulates rolling a standard six-sided die. Each number has 1/6 probability.

---

### Example 2: Two Dice Roll
```
=RANDBETWEEN(1, 6) + RANDBETWEEN(1, 6)
```
**Result:** Sum from 2 to 12

**Explanation:** Simulates rolling two dice and summing. Note: distribution is not uniform (7 is most likely).

---

### Example 3: Random Percentage
```
=RANDBETWEEN(0, 100)
```
**Result:** Integer from 0 to 100

**Explanation:** Random whole number percentage. Use for simulating percentage scores, completion rates, etc.

---

### Example 4: Random Year
```
=RANDBETWEEN(2020, 2025)
```
**Result:** Year from 2020 to 2025

**Explanation:** Random year in range. Useful for generating test data with dates.

---

### Example 5: Random Selection Index
```
=INDEX(Names, RANDBETWEEN(1, COUNTA(Names)))
```
**Result:** Random name from list

**Explanation:** Generates random row number, returns that name. Equal probability for each name.

---

### Example 6: Coin Flip (0 or 1)
```
=RANDBETWEEN(0, 1)
```
**Result:** 0 or 1

**Explanation:** Binary random outcome. Map to Heads/Tails with IF: =IF(RANDBETWEEN(0,1),"Heads","Tails")

---

### Example 7: Negative Range
```
=RANDBETWEEN(-50, 50)
```
**Result:** Integer from -50 to 50

**Explanation:** Works with negative numbers. Returns any integer from -50 through 50 inclusive.

---

### Example 8: Random Date in Range
```
=RANDBETWEEN(DATE(2024,1,1), DATE(2024,12,31))
```
**Result:** Random date in 2024

**Explanation:** Dates are stored as numbers, so this returns a random date serial number. Format cell as Date.

---

### Example 9: Random Time in Range
```
=RANDBETWEEN(9*60, 17*60)/60/24
```
**Result:** Random time between 9 AM and 5 PM

**Explanation:** Generate random minutes (540-1020), convert to Excel time fraction. Format as Time.

---

### Example 10: Playing Card
```
=RANDBETWEEN(1, 52)
```
**Result:** Integer from 1 to 52

**Explanation:** Random card number. Combine with INDEX to map to card names, or use MOD/INT for suit and rank.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Bottom is greater than top | Ensure first argument is smaller than or equal to second. RANDBETWEEN(10, 1) fails. |
| `#VALUE!` | Non-numeric arguments | Ensure both arguments are numbers. Text causes errors. |
| `#NAME?` | Misspelled function name | Verify spelling: RANDBETWEEN, not RAND_BETWEEN or RANDOMBETWEEN. |
| `Values keep changing` | Normal volatile behavior | RANDBETWEEN recalculates on every sheet change. Copy/paste values to preserve. |
| `Decimals truncated` | Arguments rounded down | RANDBETWEEN(1.9, 5.1) becomes RANDBETWEEN(1, 5). Use RAND for decimal ranges. |

## Use Cases

### [[Random Test Data Generation]]

**Scenario:** Create realistic random data for testing, demonstrations, or mockups.

**Implementation:**
```
=RANDBETWEEN(18, 65)                               → Random age
=RANDBETWEEN(DATE(2023,1,1), DATE(2023,12,31))    → Random date
=RANDBETWEEN(30000, 150000)                        → Random salary
=INDEX(Cities, RANDBETWEEN(1, ROWS(Cities)))       → Random city
```

**Business Application:** Database testing, UI mockups, demo data, training datasets. Quickly generate realistic-looking data.

**Technical Details:** Combine multiple RANDBETWEEN calls for related fields. For realistic distributions (most people aren't at age extremes), consider NORM.INV with RAND for bell-curved data.

---

### [[Game and Simulation Development]]

**Scenario:** Create random game elements, dice rolls, card draws, or chance events.

**Implementation:**
```
=RANDBETWEEN(1, 6)                                 → Standard die
=RANDBETWEEN(1, 20)                                → D20 die
=RANDBETWEEN(1, 4) + RANDBETWEEN(1, 4)             → 2d4 dice
=IF(RANDBETWEEN(1,100)<=25,"Critical!","Normal")   → 25% critical chance
```

**Business Application:** Game prototyping, educational probability demos, decision randomization, raffle systems.

**Technical Details:** For multiple dice, add separate RANDBETWEEN calls—don't multiply or use other shortcuts. Each die is independent. Dice sums follow specific probability distributions.

---

### [[Random Assignment and Selection]]

**Scenario:** Randomly assign people to groups, select winners, or allocate resources.

**Implementation:**
```
=RANDBETWEEN(1, NumberOfGroups)                    → Assign to random group
=INDEX(Participants, RANDBETWEEN(1, COUNT(Participants)))  → Random winner
=IF(RANDBETWEEN(1,TotalItems)<=SampleSize,"Selected","") → Random sample
```

**Business Application:** A/B test assignment, lottery selection, jury pools, random audits, team allocation.

**Technical Details:** For sampling without replacement (each item selected once), generate RANDBETWEEN for all items, sort, take top n. Or use RAND() column and sort.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2000+ (originally Analysis ToolPak), built-in since Excel 2007
- **Range:** Returns values inclusive of both endpoints
- **Decimals:** Truncated to integers before generating
- **Negative numbers:** Supported

### Google Sheets
- **Availability:** All versions
- **Range:** Same inclusive behavior
- **Decimals:** Same truncation
- **Negative numbers:** Same support

### Both Platforms
- Identical behavior for same inputs
- Same volatile recalculation behavior
- Same #NUM! error when bottom > top
- Equal probability for all integers in range

## Tips and Best Practices

1. **Remember it's inclusive:** RANDBETWEEN(1, 10) can return both 1 and 10. There are 10 possible outcomes, not 9.

2. **Paste values to preserve:** Like RAND, RANDBETWEEN recalculates constantly. Copy > Paste Special > Values locks in the current random values.

3. **Works with dates:** Dates are just numbers, so RANDBETWEEN(StartDate, EndDate) generates random dates. Format the cell as Date to display properly.

4. **Don't use for decimals:** RANDBETWEEN only returns integers. For random decimals in a range, use: =Min + RAND()*(Max-Min).

5. **Each call is independent:** RANDBETWEEN(1,6) + RANDBETWEEN(1,6) rolls two dice—different random values. It's not the same as 2*RANDBETWEEN(1,6) which would just double one roll.

6. **Arguments are truncated:** RANDBETWEEN(1.9, 5.7) is treated as RANDBETWEEN(1, 5). Be aware if you're calculating bounds dynamically.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[RAND]] | Random decimal 0-1 | When you need decimal values or uniform distribution |
| [[RANDARRAY]] | Array of random numbers | When you need multiple randoms at once (Excel 365) |

### Commonly Used Together

**[[RAND]]** - For decimal random values

*Compare approaches:*
```
=RANDBETWEEN(1, 100)      → Integer 1-100
=RAND()*100               → Decimal 0-99.999...
```
Use RAND for continuous values, RANDBETWEEN for discrete.

---

**[[INDEX]]** - Random selection from list

*Pick random item:*
```
=INDEX(List, RANDBETWEEN(1, ROWS(List)))
```
Generate random row number, return that item.

---

**[[CHOOSE]]** - Random from explicit options

*Random from short list:*
```
=CHOOSE(RANDBETWEEN(1,3), "Red", "Blue", "Green")
```
Select randomly from listed values.

---

**[[DATE]]** - Random date generation

*Random date in range:*
```
=RANDBETWEEN(DATE(2024,1,1), DATE(2024,12,31))
```
Date serial numbers work directly with RANDBETWEEN.

---

**[[IF]]** - Map to categories

*Random outcome with labels:*
```
=IF(RANDBETWEEN(1,6)=6, "You win!", "Try again")
```
Convert random integer to meaningful output.

## Official Documentation

- **Microsoft Excel:** [RANDBETWEEN function](https://support.microsoft.com/en-us/office/randbetween-function-4cc7f0d1-87dc-4eb7-987f-a469ab381685)
- **Google Sheets:** [RANDBETWEEN function](https://support.google.com/docs/answer/3093507)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Originally in Analysis ToolPak |
| Excel 2007+ | Built-in | No longer requires add-in |
| Google Sheets | Original launch | Always built-in |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
