---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - text-manipulation
  - character-encoding
subTopics:
  - unicode-codepoint
  - character-code
  - text-analysis
  - encoding-detection
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - unicode-codepoint
  - char-to-code
  - character-number
  - codepoint-lookup
tags:
  - text
  - unicode
  - characters
  - encoding
  - analysis
  - excel
  - google-sheets
---

# UNICODE

## Description

UNICODE is a text function that returns the Unicode code point number for the first character of a given text string. Every character in the Unicode standard has a unique numeric identifier called a code point, and UNICODE reveals this number. For example, UNICODE("A") returns 65, UNICODE returns 937, and UNICODE of an emoji returns its corresponding high code point number. This function is the inverse of UNICHAR: while UNICHAR converts a number to a character, UNICODE converts a character to its number.

UNICODE is valuable for character analysis, encoding verification, text processing, and any situation where you need to work with characters at the numeric level. Common use cases include validating input character ranges, detecting special characters or control characters in text, sorting text by character code, comparing characters across different encodings, building character code reference tables, identifying unknown or problematic characters, and implementing character-based logic in formulas. It's particularly useful for data cleaning and validation workflows where specific character types need to be identified.

There are several important gotchas with UNICODE. First, the function only returns the code point of the FIRST character; if you pass a multi-character string, subsequent characters are ignored. Second, empty strings return a #VALUE! error since there's no character to evaluate. Third, some characters that appear as single glyphs (like some emojis) are actually composed of multiple code points (emoji sequences); UNICODE only returns the first. Fourth, the code point returned is the raw Unicode number, which may be different from what you see in some encoding contexts. Fifth, comparing characters by their Unicode values may not produce intuitive alphabetical sorting for all languages.

Regarding platform availability, UNICODE is available in Microsoft Excel (2013 and later) and Google Sheets. The function behaves identically on both platforms, returning the same code point values for the same characters. Earlier Excel versions only had the CODE function, which is limited to code points 1-255 (extended ASCII). For modern international text handling, UNICODE is essential as it handles the full range of Unicode characters including emoji and characters from all world languages.

## Syntax

> [!f(x)] UNICODE Syntax
>
> ```excel
> =UNICODE(text)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string from which to return the Unicode code point of the first character. Can be a literal string in quotes, a cell reference, or an expression returning text. | Yes |

## Examples

### Example 1: Basic ASCII Letter
```excel
=UNICODE("A")
```
**Result:** `65`

**Explanation:** Capital "A" has code point 65 in Unicode (same as ASCII). This is the most basic UNICODE usage.

---

### Example 2: Lowercase Letter
```excel
=UNICODE("a")
```
**Result:** `97`

**Explanation:** Lowercase "a" is code point 97. Lowercase letters are 32 positions after their uppercase equivalents.

---

### Example 3: First Character Only
```excel
=UNICODE("Hello")
```
**Result:** `72`

**Explanation:** UNICODE returns only the first character's code point. "H" is code point 72; the rest of "Hello" is ignored.

---

### Example 4: Cell Reference
```excel
=UNICODE(A1)
```
Where A1 contains "Excel"

**Result:** `69`

**Explanation:** Returns code point for "E" (69). Works with cell references just like literal strings.

---

### Example 5: Greek Letter
```excel
=UNICODE("Ω")
```
**Result:** `937`

**Explanation:** Capital Omega has code point 937. Greek letters have code points in the 900s range.

---

### Example 6: Check Mark Symbol
```excel
=UNICODE("✓")
```
**Result:** `10003`

**Explanation:** The check mark symbol has code point 10003. Higher numbers indicate characters outside basic ASCII.

---

### Example 7: Euro Symbol
```excel
=UNICODE("€")
```
**Result:** `8364`

**Explanation:** The Euro sign has code point 8364. Currency symbols have various code points.

---

### Example 8: Space Character
```excel
=UNICODE(" ")
```
**Result:** `32`

**Explanation:** Space has code point 32. Useful for identifying or validating whitespace characters.

---

### Example 9: Newline Character
```excel
=UNICODE(CHAR(10))
```
**Result:** `10`

**Explanation:** Line feed (newline) has code point 10. UNICODE of CHAR(n) returns n for valid code points.

---

### Example 10: Emoji Character
```excel
=UNICODE("(grinning face emoji)")
```
**Result:** `128512`

**Explanation:** Emojis have high code points in supplementary planes. Simple emojis return single code points.

---

### Example 11: Digit Character
```excel
=UNICODE("5")
```
**Result:** `53`

**Explanation:** The character "5" (not the number) has code point 53. Digit characters are in the 48-57 range.

---

### Example 12: Japanese Kanji
```excel
=UNICODE("東")
```
**Result:** `26481`

**Explanation:** CJK (Chinese/Japanese/Korean) characters have code points in higher ranges. This is the kanji for "east."

---

### Example 13: Round-Trip with UNICHAR
```excel
=UNICHAR(UNICODE("X"))
```
**Result:** `X`

**Explanation:** UNICODE and UNICHAR are inverses. This demonstrates the round-trip: character to code back to character.

---

### Example 14: Character Range Check
```excel
=IF(AND(UNICODE(A1)>=65, UNICODE(A1)<=90), "Uppercase", "Not uppercase")
```
**Result:** "Uppercase" or "Not uppercase"

**Explanation:** Check if first character is uppercase letter (A-Z is 65-90). Useful for validation rules.

---

### Example 15: Comparing Characters
```excel
=UNICODE(A1) = UNICODE(B1)
```
**Result:** TRUE or FALSE

**Explanation:** Compare first characters of two cells at the code point level. Useful for case-sensitive matching.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Empty string "" provided | Check for empty cells; use IF(A1="", "", UNICODE(A1)) |
| `#VALUE!` | Cell is truly empty | Verify cell contains text; handle empty cells conditionally |
| `Only first char returned` | Multi-character string | Expected behavior; use MID to check specific positions |
| `Unexpected code point` | Composite character (emoji sequence) | Some glyphs are multiple code points; only first is returned |
| `#NAME?` | Excel version too old | UNICODE requires Excel 2013+; use CODE for ASCII range |

## Use Cases

### [[Character Validation]]
- **Implementation**: Verify that input characters fall within acceptable ranges. Check for uppercase, lowercase, digits, or specific character sets.
- **Business Application**: Form validation, data quality checks, password policy enforcement, filename validation, and input sanitization.
- **Technical Details**: ASCII ranges: uppercase A-Z (65-90), lowercase a-z (97-122), digits 0-9 (48-57). Use AND/OR with UNICODE checks: =AND(UNICODE(A1)>=65, UNICODE(A1)<=90).

### [[Special Character Detection]]
- **Implementation**: Identify non-standard characters, control characters, or unexpected Unicode content in text data.
- **Business Application**: Data cleaning, import validation, text processing pipelines, malformed data detection, and security scanning for unusual characters.
- **Technical Details**: Control characters: 0-31 and 127. Non-ASCII: >127. High Unicode: >65535 (supplementary planes). Create formulas to flag unexpected ranges.

### [[Character Set Analysis]]
- **Implementation**: Analyze text content to determine character composition, detect language scripts, or categorize text types.
- **Business Application**: Content classification, language detection, encoding analysis, multilingual data processing, and character frequency analysis.
- **Technical Details**: Create lookup tables mapping code point ranges to character categories. Greek: 880-1023. Cyrillic: 1024-1279. CJK: 19968-40959 (basic). Build category detection formulas.

### [[Custom Sorting Logic]]
- **Implementation**: Use UNICODE values to create custom sort orders or to sort text in ways not supported by standard sorting.
- **Business Application**: Phonetic sorting, custom alphabetization, prioritized list ordering, and language-specific sort orders.
- **Technical Details**: UNICODE values determine default code point sort order. Create helper columns with UNICODE-based transformations for custom sorts. May need mapping tables for non-standard orders.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | Excel 2013+ | All versions |
| **Function Syntax** | Identical | Identical |
| **Valid Input** | Any text string | Any text string |
| **Empty String** | #VALUE! error | #VALUE! error |
| **Return Range** | 0 to 1,114,111 | 0 to 1,114,111 |
| **CODE Function** | 1-255 only | 1-255 for CODE |
| **Emoji Handling** | Returns first code point | Returns first code point |

**Key Notes:**

- UNICODE works identically on both platforms
- Both platforms return the same code points for the same characters
- CODE function is limited to ASCII; UNICODE handles full Unicode
- Composite characters (emoji sequences) only return first code point

**Excel Pre-2013:**

UNICODE is not available in Excel 2010 or earlier:
- Use CODE for ASCII characters (1-255)
- No direct equivalent for higher code points
- Consider VBA UDF for full Unicode support in older versions

## Tips and Best Practices

1. **Only First Character**: Remember UNICODE only evaluates the first character. Use MID to check specific positions: `=UNICODE(MID(A1, 3, 1))` for third character.

2. **Handle Empty Strings**: Always check for empty cells to avoid errors: `=IF(LEN(A1)>0, UNICODE(A1), "Empty")`.

3. **Character Range Validation**: Use UNICODE with AND/OR for input validation: `=AND(UNICODE(A1)>=48, UNICODE(A1)<=57)` checks for digit.

4. **Create Reference Tables**: Build lookup tables of commonly needed code points for your specific use cases.

5. **Compare with UNICHAR**: Test your understanding with round-trips: UNICHAR(UNICODE(char)) should return the original character.

6. **Unicode Chart Reference**: Keep Unicode.org charts accessible for looking up code point ranges and character categories.

7. **Composite Character Awareness**: Modern emojis and some scripts use multiple code points. Test thoroughly with actual data.

8. **Performance with Large Data**: UNICODE is fast, but apply to large datasets judiciously. Consider sampling for analysis.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from UNICODE |
|----------|-------------|----------------------------|
| [[CODE]] | Returns ASCII code (1-255) | Limited to extended ASCII range |
| [[UNICHAR]] | Returns character from code point | Inverse of UNICODE |
| [[CHAR]] | Returns character from code 1-255 | Limited range; inverse of CODE |

### Commonly Used Together

**[[UNICHAR]]** - Convert code to character

*Round-trip test:*
```excel
=UNICHAR(UNICODE("A"))
```
Returns "A". UNICHAR is the inverse of UNICODE.

**[[IF]]/[[AND]]** - Character validation

*Check character range:*
```excel
=IF(AND(UNICODE(A1)>=65, UNICODE(A1)<=90), "OK", "Invalid")
```
Validate that first character is uppercase.

**[[MID]]** - Check specific position

*Get code of nth character:*
```excel
=UNICODE(MID(A1, 5, 1))
```
Return code point of the 5th character.

**[[LEN]]** - Check before processing

*Handle empty strings:*
```excel
=IF(LEN(A1)>0, UNICODE(A1), "Empty")
```
Avoid errors on empty cells.

**[[SUBSTITUTE]]** - Find/replace by code

*Replace specific character:*
```excel
=SUBSTITUTE(A1, UNICHAR(160), " ")
```
Replace non-breaking space (160) with regular space (32).

**[[TEXTJOIN]]** - Analyze all characters

*List all code points:*
```excel
=TEXTJOIN(", ", TRUE, UNICODE(MID(A1, ROW(INDIRECT("1:"&LEN(A1))), 1)))
```
Array formula to list code points of all characters in a string.

## Official Documentation

- **Microsoft Excel**: [UNICODE function](https://support.microsoft.com/en-us/office/unicode-function-adb74aaa-a2a5-4dde-aff6-966e4e81f16f)
- **Google Sheets**: [UNICODE function](https://support.google.com/docs/answer/3094295)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Initial release |
| Excel 2016 | 2016 | Improved Unicode support |
| Excel 365 | Ongoing | Extended emoji handling |
| Google Sheets | Original | Available since early versions |

---

*Last updated: 2026-01-10*
