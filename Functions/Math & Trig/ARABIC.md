---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- number-conversion
- roman-numerals
- text-to-number
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Roman to Arabic
- Roman Numeral Converter
- Convert Roman Numerals
tags:
- conversion
- roman-numerals
- text-processing
- number-systems
---

# ARABIC

## Description

**ARABIC** converts a Roman numeral text string into its equivalent Arabic (standard decimal) number. Give it "XIV" and get back 14. Give it "MCMXCIX" and get back 1999. It's the reverse of the ROMAN function—together they form a conversion pair for working with Roman numerals in spreadsheets.

Roman numerals appear in surprisingly many contexts: copyright years (MMXXV = 2025), book chapters (Chapter IX), outlines (Section III.A.2), movie sequels (Rocky IV), watch faces, and legal documents. When you need to perform calculations on Roman numerals—sorting, comparing, or doing math—you first need to convert them to regular numbers. That's where ARABIC comes in.

**Validation and error handling:** ARABIC validates its input and returns #VALUE! for invalid Roman numeral strings. It accepts both uppercase and lowercase input (case-insensitive). The function handles the "subtractive notation" rules properly: IV = 4 (not IIII), XC = 90 (not LXXXX). However, it's fairly permissive—"IIII" (used on some clock faces) is accepted as 4, even though it's not standard notation.

**Range limitations:** ARABIC handles Roman numerals up to 3999 (MMMCMXCIX)—the traditional limit since there's no standard symbol for 5000. For values beyond this, Roman numeral notation becomes unwieldy or requires non-standard extensions. Empty strings return 0, which is mathematically questionable (Romans had no zero) but practically useful.

## Syntax

> [!f(x)] ARABIC Syntax
>
> ```
> =ARABIC(text)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | A Roman numeral as a text string, or a reference to a cell containing Roman numeral text. Case-insensitive. |

### Return Value

Returns the Arabic numeral equivalent as an integer. Returns 0 for empty strings. Returns #VALUE! for invalid Roman numeral strings or non-text inputs.

## Examples

> [!f(x)] ARABIC Examples

### Example 1: Basic Small Number
```
=ARABIC("V")
```
**Result:** 5

**Explanation:** V is the Roman numeral for 5. The simplest conversion case.

---

### Example 2: Subtractive Notation
```
=ARABIC("IV")
```
**Result:** 4

**Explanation:** IV uses subtractive notation (5 - 1 = 4). ARABIC correctly handles this Roman numeral convention.

---

### Example 3: Larger Number
```
=ARABIC("XLII")
```
**Result:** 42

**Explanation:** XL = 40 (50 - 10), II = 2. Combined: 42. The answer to life, the universe, and everything.

---

### Example 4: Complex Number
```
=ARABIC("MCMXCIX")
```
**Result:** 1999

**Explanation:** M=1000, CM=900 (1000-100), XC=90 (100-10), IX=9 (10-1). Total: 1999. Prince's favorite year.

---

### Example 5: Year in Roman Numerals
```
=ARABIC("MMXXV")
```
**Result:** 2025

**Explanation:** MM = 2000, XX = 20, V = 5. Common in copyright notices and movie credits.

---

### Example 6: Cell Reference
```
=ARABIC(A1)
```
Where A1 contains "CDXLIV"

**Result:** 444

**Explanation:** CD = 400, XL = 40, IV = 4. References allow processing Roman numerals stored in cells.

---

### Example 7: Lowercase Input
```
=ARABIC("xvii")
```
**Result:** 17

**Explanation:** Function is case-insensitive. Lowercase "xvii" equals uppercase "XVII" equals 17.

---

### Example 8: Mixed Case
```
=ARABIC("McmLxxxIV")
```
**Result:** 1984

**Explanation:** Mixed case is accepted. M=1000, CM=900, LXXX=80, IV=4. Total: 1984.

---

### Example 9: Maximum Standard Value
```
=ARABIC("MMMCMXCIX")
```
**Result:** 3999

**Explanation:** This is the largest number representable in standard Roman numerals (without extended notation).

---

### Example 10: Empty String
```
=ARABIC("")
```
**Result:** 0

**Explanation:** Empty strings return 0. Historically questionable (Romans had no zero) but practically convenient for calculations.

---

### Example 11: Non-Standard but Accepted Notation
```
=ARABIC("IIII")
```
**Result:** 4

**Explanation:** While "IV" is standard, "IIII" (seen on clock faces) is also accepted. The function is permissive about notation variations.

---

### Example 12: Error Case - Invalid Input
```
=ARABIC("ABC")
```
**Result:** #VALUE!

**Explanation:** "ABC" contains invalid Roman numeral characters (A and B). Function returns error for unrecognized text.

---

### Example 13: Using with ROMAN for Verification
```
=ROMAN(ARABIC("CXXIII"))
```
**Result:** "CXXIII"

**Explanation:** Converting ARABIC and back to ROMAN should return the original (assuming standard notation). Useful for validation.

---

### Example 14: Arithmetic with Roman Numerals
```
=ARABIC("X") + ARABIC("V")
```
**Result:** 15

**Explanation:** Convert both numerals to Arabic, then add. X=10, V=5, sum=15.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid Roman numeral character | Check input for non-Roman characters (A, B, E, F, G, H, J, K, etc.). Valid characters: I, V, X, L, C, D, M. |
| `#VALUE!` | Number input instead of text | ARABIC expects text. If cell contains 14, use ROMAN(14) first, or ensure source is text format. |
| `#VALUE!` | Non-text input type | Ensure input is a text string. Numbers and errors as input cause #VALUE!. |
| `0` | Empty string | Empty input returns 0. May need to handle with IF to distinguish from intentional zero. |
| `Wrong result` | Malformed Roman numeral | While ARABIC is permissive, severely malformed input may give unexpected results. Validate source data. |

## Use Cases

### [[Processing Copyright Years]]

**Scenario:** Convert Roman numeral copyright years from legal documents or media files to standard years for sorting and filtering.

**Implementation:**
```
=ARABIC(B2)
```
Where B2 contains copyright year in Roman format (e.g., "MMXX")

**Business Application:** Media cataloging, legal document processing, film database management. Copyright years are often written in Roman numerals on films and broadcasts.

**Technical Details:** Useful for sorting films by release year, identifying expired copyrights, or building searchable archives of media.

---

### [[Outline and Chapter Processing]]

**Scenario:** Convert Roman numeral chapter numbers to Arabic for sorting, calculations, or generating page numbers.

**Implementation:**
```
Chapter Number: =ARABIC(LEFT(A2, FIND(" ", A2)-1))
```
Where A2 contains "XIV Introduction to Calculus"

**Business Application:** Book indexing, document management, academic publishing. Many formal documents use Roman numerals for chapter or section numbers.

**Technical Details:** Combined with text functions (LEFT, FIND, MID) to extract Roman numerals from mixed content strings.

---

### [[Historical Date Conversion]]

**Scenario:** Convert dates from historical documents or inscriptions that use Roman numerals.

**Implementation:**
```
Year: =ARABIC(YearCell)
Full Date: =DATE(ARABIC(YearCell), ARABIC(MonthCell), ARABIC(DayCell))
```

**Business Application:** Historical research, museum cataloging, archaeology data processing. Many historical records use Roman numerals for dates.

**Technical Details:** May need to handle various date formats. Roman numerals for days and months were common in medieval documents.

---

### [[Competition and Sports Rankings]]

**Scenario:** Process Super Bowl numbers, Olympics numbers (XXIII Winter Olympics), or other sporting event designations.

**Implementation:**
```
Event Number: =ARABIC(SUBSTITUTE(A2, "Super Bowl ", ""))
```
Where A2 contains "Super Bowl LVIII"

**Business Application:** Sports statistics databases, event tracking, historical analysis of championship games.

**Technical Details:** Extract the Roman numeral portion from event names, convert to number for mathematical operations and sorting.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Case handling:** Case-insensitive (accepts "iv", "IV", "Iv")
- **Validation:** Returns #VALUE! for invalid Roman numerals
- **Empty string:** Returns 0

### Google Sheets
- **Availability:** All versions
- **Case handling:** Case-insensitive (same as Excel)
- **Validation:** Returns #VALUE! for invalid Roman numerals
- **Empty string:** Returns 0
- **Behavior:** Identical to Excel

### Both Platforms
- Maximum valid input: MMMCMXCIX (3999)
- Minimum valid input: empty string (returns 0)
- Accepts non-standard notations like "IIII" for 4
- Same error conditions and messages

## Tips and Best Practices

1. **Validate before converting:** Use `=IF(ISERROR(ARABIC(A1)), "Invalid", ARABIC(A1))` to catch and handle invalid Roman numerals gracefully.

2. **Remember case insensitivity:** You don't need to worry about uppercase/lowercase conversion before using ARABIC. "mcmxcix" works just as well as "MCMXCIX".

3. **Handle empty cells:** ARABIC returns 0 for empty strings. If you need to distinguish between "missing data" and "actual zero," wrap in IF: `=IF(A1="", "No value", ARABIC(A1))`.

4. **Pair with ROMAN for roundtrip:** Use `=ROMAN(ARABIC(A1))` to standardize Roman numeral notation (e.g., "IIII" becomes "IV").

5. **Extract from mixed text:** When Roman numerals are embedded in text, use LEFT, MID, or REGEXEXTRACT (Sheets) to isolate them before conversion.

6. **Consider the 3999 limit:** Standard Roman numerals max out at 3999. If you're working with larger numbers, you'll need custom solutions or extended notation systems.

7. **Sort Roman numerals numerically:** Convert to Arabic for sorting: `=ARABIC(RomanColumn)` creates a sort key that orders Roman numerals correctly (IX before X, X before XI).

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ROMAN]] | Converts Arabic number to Roman numeral | When you need the reverse conversion |
| [[VALUE]] | Converts text to number | For converting numeric strings (not Roman numerals) |
| [[NUMBERVALUE]] | Locale-aware text to number | For formatted numbers with locale-specific separators |

### Commonly Used Together

**[[ROMAN]]** - Arabic to Roman conversion

*Roundtrip validation:*
```
=ROMAN(ARABIC(A1))  // Standardizes Roman numeral notation
```
Convert back and forth to verify accuracy or normalize format.

---

**[[IF]]** - Conditional logic

*Handle invalid inputs:*
```
=IF(ISERROR(ARABIC(A1)), "Invalid Roman", ARABIC(A1))
```
Provide user-friendly messages for invalid Roman numerals.

---

**[[TEXT]]** - Text formatting

*Format result:*
```
=TEXT(ARABIC(A1), "0000")  // Pad with leading zeros
```
Format converted numbers for specific display requirements.

---

**[[CONCATENATE]] / [[&]]** - Text joining

*Create output strings:*
```
=ARABIC(A1) & " (" & A1 & ")"  // Results in "1999 (MCMXCIX)"
```
Combine Arabic result with original Roman numeral for clarity.

---

**[[SORT]]** - Sorting (Google Sheets)

*Sort Roman numerals correctly:*
```
=SORT(A2:B10, ARABIC(A2:A10), TRUE)
```
Sort a list by Roman numeral column using Arabic values for correct ordering.

## Official Documentation

- **Microsoft Excel:** [ARABIC function](https://support.microsoft.com/en-us/office/arabic-function-9a8da418-c17b-4ef9-a657-9370a30a674f)
- **Google Sheets:** [ARABIC function](https://support.google.com/docs/answer/9061514)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Initial implementation |
| Excel 2016+ | Enhanced | Improved error handling |
| Google Sheets | 2019 | Added to match Excel functionality |

---

*Last updated: 2026-01-10*
