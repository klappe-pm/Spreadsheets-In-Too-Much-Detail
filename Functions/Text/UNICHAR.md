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
  - unicode-character
  - character-generation
  - special-characters
  - symbol-insertion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - unicode-character
  - codepoint-to-char
  - unicode-symbol
  - special-character
tags:
  - text
  - unicode
  - characters
  - encoding
  - symbols
  - excel
  - google-sheets
---

# UNICHAR

## Description

UNICHAR is a text function that returns the character represented by a given Unicode code point number. Every character in the Unicode standard, from basic ASCII letters to emojis, mathematical symbols, and characters from virtually every writing system in the world, has a unique numeric code point. UNICHAR takes this number and returns the corresponding character. For example, UNICHAR(65) returns "A" (the standard ASCII capital A), UNICHAR(937) returns the Greek letter "Omega", and UNICHAR(128512) returns a grinning face emoji. This function is essential for working with special characters, symbols, and international text.

UNICHAR is valuable whenever you need to insert characters that aren't easily typed on a keyboard or when you need programmatic control over character generation. Common use cases include inserting special symbols (bullets, arrows, check marks), working with international characters (accented letters, Greek letters, mathematical symbols), generating emojis in formulas, creating custom bullets or markers, inserting line breaks or other control characters, and building character reference tables. It's particularly useful in templates and reports where specific symbols need to appear consistently.

There are several important gotchas with UNICHAR. First, the function requires a valid Unicode code point (0 to 1,114,111); values outside this range return errors. Second, not all code points have assigned characters; some return blank or special replacement characters. Third, character display depends on font support; if the font doesn't include a glyph for that code point, a placeholder (often a box or question mark) appears. Fourth, some characters (like emojis) use supplementary planes and require larger code point numbers. Fifth, the character may display differently across operating systems, applications, and devices. Sixth, control characters (code points 0-31) may not display visibly but can affect text processing.

Regarding platform availability, UNICHAR is available in both Microsoft Excel (2013 and later) and Google Sheets. The function behaves identically on both platforms for standard Unicode characters. Earlier Excel versions had only the CHAR function, which is limited to code points 1-255 (extended ASCII). UNICHAR's ability to handle the full Unicode range makes it essential for modern international text handling. Both platforms support emoji and international character rendering, though display quality depends on the fonts installed on the system viewing the spreadsheet.

## Syntax

> [!f(x)] UNICHAR Syntax
>
> ```excel
> =UNICHAR(number)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `number` | The Unicode code point number for the character to return. Must be an integer between 0 and 1,114,111 (the maximum Unicode code point). | Yes |

## Examples

### Example 1: Basic ASCII Letter
```excel
=UNICHAR(65)
```
**Result:** `A`

**Explanation:** Code point 65 is capital letter "A" in both ASCII and Unicode. Basic letters have low code point numbers.

---

### Example 2: Lowercase Letter
```excel
=UNICHAR(97)
```
**Result:** `a`

**Explanation:** Code point 97 is lowercase "a". Lowercase letters are 32 positions after their uppercase equivalents.

---

### Example 3: Check Mark Symbol
```excel
=UNICHAR(10003)
```
**Result:** `✓`

**Explanation:** Code point 10003 is the check mark symbol. Commonly used in status columns or completion indicators.

---

### Example 4: Heavy Check Mark
```excel
=UNICHAR(10004)
```
**Result:** `✔`

**Explanation:** Code point 10004 is a heavier check mark variant. Unicode often has multiple versions of similar symbols.

---

### Example 5: X Mark (Cross)
```excel
=UNICHAR(10007)
```
**Result:** `✗`

**Explanation:** Code point 10007 is a ballot X. Use with check marks for yes/no status displays.

---

### Example 6: Bullet Point
```excel
=UNICHAR(8226)
```
**Result:** `•`

**Explanation:** Code point 8226 is the standard bullet point. Useful for creating bulleted lists in cells.

---

### Example 7: Arrow Symbols
```excel
=UNICHAR(8594)
```
**Result:** `→`

**Explanation:** Code point 8594 is a rightward arrow. Unicode has many arrow variants in different directions and styles.

---

### Example 8: Greek Letter (Omega)
```excel
=UNICHAR(937)
```
**Result:** `Ω`

**Explanation:** Code point 937 is capital Omega. Greek letters are common in scientific and mathematical contexts.

---

### Example 9: Pi Symbol
```excel
=UNICHAR(960)
```
**Result:** `π`

**Explanation:** Code point 960 is lowercase pi. Essential for mathematical formulas and documentation.

---

### Example 10: Copyright Symbol
```excel
=UNICHAR(169)
```
**Result:** `©`

**Explanation:** Code point 169 is the copyright symbol. Common in legal and attribution text.

---

### Example 11: Degree Symbol
```excel
=UNICHAR(176)
```
**Result:** `°`

**Explanation:** Code point 176 is the degree symbol for temperatures and angles. Often concatenated with numbers.

---

### Example 12: Euro Currency Symbol
```excel
=UNICHAR(8364)
```
**Result:** `€`

**Explanation:** Code point 8364 is the Euro sign. Currency symbols have various code points in Unicode.

---

### Example 13: Line Break Character
```excel
=UNICHAR(10)
```
**Result:** Line feed character (creates new line)

**Explanation:** Code point 10 is the line feed (newline) character. Same as CHAR(10), used for multi-line cell content.

---

### Example 14: Simple Emoji
```excel
=UNICHAR(128512)
```
**Result:** (grinning face emoji)

**Explanation:** Emojis have high code points in the supplementary planes. Display depends on font support.

---

### Example 15: Building Status Text
```excel
=IF(A1="Complete", UNICHAR(10003), UNICHAR(10007))
```
**Result:** Check mark or X based on status

**Explanation:** UNICHAR enables conditional symbols. Returns visual indicators based on cell values.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Code point number is not a valid integer | Ensure number is between 0 and 1,114,111 |
| `#VALUE!` | Negative number provided | Unicode code points must be positive |
| `#VALUE!` | Number exceeds 1,114,111 | Maximum valid Unicode code point is 1,114,111 |
| `#VALUE!` | Text value instead of number | Ensure input is numeric; use VALUE() if needed |
| `Blank or box displayed` | Code point is unassigned or font lacks glyph | Verify code point is assigned; try different font |
| `Different character than expected` | Code point confusion | Verify correct code point from Unicode charts |

## Use Cases

### [[Status Indicators and Symbols]]
- **Implementation**: Use UNICHAR to display visual status indicators like check marks, X marks, circles, stars, or warning symbols based on data conditions.
- **Business Application**: Project tracking dashboards, task completion status, quality inspection results, approval workflows, and any display requiring clear visual indicators.
- **Technical Details**: Common symbols: check (10003, 10004), X (10005, 10007, 10008), circle (9679, 9675), star (9733, 9734), warning (9888). Use with IF for conditional display.

### [[Mathematical and Scientific Notation]]
- **Implementation**: Insert Greek letters, mathematical operators, subscripts, superscripts, and scientific symbols into formulas and documentation.
- **Business Application**: Engineering calculations, scientific reports, statistical formulas, chemical notations, and academic documents requiring proper mathematical typography.
- **Technical Details**: Greek: alpha (945), beta (946), gamma (947), delta (948), pi (960), omega (969). Operators: plus-minus (177), approximately (8776), not equal (8800), less-or-equal (8804).

### [[International Character Support]]
- **Implementation**: Generate characters from various writing systems, accented letters, and special characters not available on standard keyboards.
- **Business Application**: International product catalogs, multilingual documents, proper name spelling with diacritics, and localized content generation.
- **Technical Details**: Look up code points in Unicode charts for specific scripts. Common accented letters: a-acute (225), e-acute (233), n-tilde (241). Currency symbols in various positions.

### [[Custom Bullet and List Formatting]]
- **Implementation**: Create custom bullet styles and list markers using various Unicode symbols instead of standard bullets.
- **Business Application**: Branded documents, styled reports, differentiated list levels, and visual variety in presentations and reports.
- **Technical Details**: Bullet options: bullet (8226), triangle (9654, 9658), arrow (8594, 8658), dash (8211), diamond (9670). Combine with text concatenation.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | Excel 2013+ | All versions |
| **Function Syntax** | Identical | Identical |
| **Valid Range** | 0 to 1,114,111 | 0 to 1,114,111 |
| **Emoji Support** | Yes (high code points) | Yes (high code points) |
| **Font Rendering** | System fonts | Browser/system fonts |
| **CHAR Function** | 1-255 only | 1-255 for CHAR |
| **Error Handling** | #VALUE! | #VALUE! |

**Key Notes:**

- UNICHAR works identically on both platforms for valid code points
- Character display quality depends on available fonts
- Emojis may render differently across platforms and versions
- CHAR function is limited to ASCII (1-255); UNICHAR handles full Unicode

**Excel Pre-2013:**

UNICHAR is not available in Excel 2010 or earlier. Workarounds:
- Use CHAR for code points 1-255
- Copy/paste special characters from Character Map
- Use font-specific symbols (Wingdings, etc.)

## Tips and Best Practices

1. **Reference Unicode Charts**: Keep a reference of commonly used code points. The official Unicode.org charts provide comprehensive listings.

2. **Use CHAR for Low Values**: For code points 1-255 (ASCII and Latin-1), CHAR and UNICHAR work identically. CHAR is more widely compatible.

3. **Test Font Support**: Before using exotic characters, test that target fonts support them. Fallback to simpler alternatives if needed.

4. **Create Named Constants**: For frequently used symbols, create named ranges: CheckMark = UNICHAR(10003). This improves readability.

5. **Combine with Conditional Logic**: `=IF(A1>=target, UNICHAR(10003), UNICHAR(10007))` creates dynamic status displays.

6. **Document Code Points**: Comment or document which code points you're using, as raw numbers aren't self-explanatory.

7. **Consider Accessibility**: Visual symbols may not be accessible to screen readers. Include text alternatives when needed.

8. **Handle Display Variations**: The same code point may render differently across devices. Test on target platforms.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from UNICHAR |
|----------|-------------|----------------------------|
| [[CHAR]] | Returns character from code 1-255 | Limited to extended ASCII range |
| [[UNICODE]] | Returns code point of first character | Inverse of UNICHAR |
| [[CODE]] | Returns ASCII code (1-255) | Limited range; legacy function |

### Commonly Used Together

**[[UNICODE]]** - Get code point from character

*Round-trip test:*
```excel
=UNICODE(UNICHAR(65))
```
Returns 65. UNICODE is the inverse of UNICHAR.

**[[IF]]** - Conditional symbols

*Display status indicators:*
```excel
=IF(A1="Yes", UNICHAR(10003), UNICHAR(10007))
```
Show check or X based on value.

**[[CONCATENATE]]/[[&]]** - Combine with text

*Add symbols to text:*
```excel
="Temperature: " & A1 & UNICHAR(176) & "C"
```
Append degree symbol to temperature value.

**[[SUBSTITUTE]]** - Replace with symbols

*Replace text markers with symbols:*
```excel
=SUBSTITUTE(A1, "[check]", UNICHAR(10003))
```
Replace placeholder text with actual symbol.

**[[REPT]]** - Repeat characters

*Create star ratings:*
```excel
=REPT(UNICHAR(9733), A1) & REPT(UNICHAR(9734), 5-A1)
```
Display filled and empty stars based on rating value.

**[[TEXTJOIN]]** - Build symbol strings

*Create visual bars:*
```excel
=TEXTJOIN("", TRUE, REPT(UNICHAR(9608), A1))
```
Generate progress bars using block characters.

## Official Documentation

- **Microsoft Excel**: [UNICHAR function](https://support.microsoft.com/en-us/office/unichar-function-ffeb64f5-f131-44c6-b332-5cd72f0659b8)
- **Google Sheets**: [UNICHAR function](https://support.google.com/docs/answer/3094294)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Initial release |
| Excel 2016 | 2016 | Improved Unicode support |
| Excel 365 | Ongoing | Extended emoji support |
| Google Sheets | Original | Available since early versions |

---

*Last updated: 2026-01-10*
