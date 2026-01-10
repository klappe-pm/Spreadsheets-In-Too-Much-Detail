---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - text-manipulation
  - character-conversion
subTopics:
  - halfwidth-to-fullwidth
  - katakana-conversion
  - japanese-text
  - asian-characters
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - halfwidth-to-fullwidth
  - narrow-to-wide
  - katakana-fullwidth
  - sbcs-to-dbcs
tags:
  - text
  - character-conversion
  - japanese
  - asian-text
  - fullwidth
  - halfwidth
  - excel
  - google-sheets
---

# JIS

## Description

JIS is a text function that converts half-width (single-byte) characters to full-width (double-byte) characters in languages that use double-byte character sets (DBCS), primarily Japanese. Half-width characters, which include ASCII letters, numbers, katakana, and certain punctuation marks, are converted to their full-width equivalents that occupy two bytes and display at the same width as kanji characters. The function name "JIS" refers to the Japanese Industrial Standards character encoding, which standardizes full-width character representations. JIS is the inverse function of ASC, which converts in the opposite direction.

JIS is essential when preparing text for contexts that require full-width character formatting, which is common in Japanese document standards, form fields with fixed character spacing, and visual alignment with kanji text. Full-width characters align neatly in vertical text layouts and proportional-width Japanese fonts, creating a more aesthetically pleasing appearance. Common use cases include preparing data for official documents, formatting text for Japanese forms that expect full-width input, creating visually aligned columns in Japanese text, standardizing data entry for Japanese systems, and converting imported data to match local formatting standards.

There are several important gotchas when using JIS. First, only characters with full-width equivalents are converted; kanji and hiragana (which are inherently full-width) pass through unchanged. Second, half-width katakana with voiced marks (dakuten) that are represented as two characters (base + mark) may combine into single full-width characters, potentially shortening the string. Third, the function is specific to DBCS languages; it has no meaningful effect on standard Western European text that has no DBCS equivalent. Fourth, the visual width of text will increase significantly as each converted character becomes twice as wide. Fifth, string length in bytes will increase, which matters for byte-limited fields.

Regarding platform availability, JIS is available in both Microsoft Excel and Google Sheets with identical syntax and functionality. Both platforms handle the conversion consistently across all versions where Japanese language support is available. In some documentation, you may see JIS referred to as DBCS (Double-Byte Character String), which is an alternative function name in certain Excel versions that performs the same operation. The function works in any spreadsheet environment but is most meaningful when working with Japanese text.

## Syntax

> [!f(x)] JIS Syntax
>
> ```excel
> =JIS(text)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string or reference to a cell containing text to convert from half-width to full-width characters. Characters without full-width equivalents remain unchanged. | Yes |

## Examples

### Example 1: Convert Half-Width Latin Letters
```excel
=JIS("ABC")
```
**Result:** `ABC` (full-width characters)

**Explanation:** Half-width ASCII letters "ABC" are converted to full-width equivalents. Each character now occupies the width of a kanji character.

---

### Example 2: Convert Half-Width Numbers
```excel
=JIS("12345")
```
**Result:** `12345` (full-width numbers)

**Explanation:** Half-width digits become full-width, aligning with Japanese text in documents requiring consistent character width.

---

### Example 3: Convert Half-Width Katakana
```excel
=JIS("ｶﾀｶﾅ")
```
**Result:** `カタカナ` (full-width katakana)

**Explanation:** Half-width katakana (common in legacy systems) are converted to standard full-width katakana.

---

### Example 4: Mixed Content Conversion
```excel
=JIS("Test123ﾃｽﾄ")
```
**Result:** `Test123テスト`

**Explanation:** All convertible half-width characters (Latin, numbers, katakana) become full-width. Mixed content is processed character by character.

---

### Example 5: Kanji Pass-Through
```excel
=JIS("東京ABC")
```
**Result:** `東京ABC`

**Explanation:** Kanji are already full-width and pass through unchanged. The half-width "ABC" becomes full-width while "東京" remains as-is.

---

### Example 6: Cell Reference Conversion
```excel
=JIS(A1)
```
Where A1 contains "HELLO"

**Result:** `HELLO` (full-width)

**Explanation:** JIS works with cell references. The half-width text in A1 is converted to full-width.

---

### Example 7: Voiced Katakana Combination
```excel
=JIS("ｶﾞｷﾞｸﾞ")
```
**Result:** `ガギグ` (full-width)

**Explanation:** Half-width katakana with separate voiced marks (base + dakuten) combine into single full-width voiced characters. The string becomes shorter.

---

### Example 8: Empty String Handling
```excel
=JIS("")
```
**Result:** `` (empty string)

**Explanation:** An empty string returns an empty string. No error is generated.

---

### Example 9: Already Full-Width Text
```excel
=JIS("すでに全角")
```
**Result:** `すでに全角`

**Explanation:** Text that is already full-width (including hiragana) passes through unchanged. JIS does not double-convert.

---

### Example 10: Converting Punctuation
```excel
=JIS("(Hello)")
```
**Result:** `（Hello）` (full-width parentheses)

**Explanation:** Half-width punctuation marks with full-width equivalents are converted along with letters.

---

### Example 11: Space Conversion
```excel
=JIS("Hello World")
```
**Result:** `Hello　World` (with full-width space)

**Explanation:** Half-width spaces become full-width spaces, which are twice as wide. This affects text alignment.

---

### Example 12: Combining with TRIM
```excel
=JIS(TRIM(A1))
```
**Result:** Full-width text with extra spaces removed first

**Explanation:** TRIM removes excess half-width spaces before conversion. Note that TRIM works on half-width spaces.

---

### Example 13: Standardizing Input for Forms
```excel
=JIS(UPPER(A1))
```
**Result:** Full-width uppercase text

**Explanation:** Convert to uppercase, then to full-width. Common pattern for Japanese form data standardization.

---

### Example 14: Handling Symbols
```excel
=JIS("@#$%")
```
**Result:** `@#$%` (full-width symbols)

**Explanation:** Common half-width symbols are converted to their full-width equivalents.

---

### Example 15: Mixed Script Document
```excel
=JIS("Name: 山田太郎 Tel: 090-1234-5678")
```
**Result:** `Name:　山田太郎　Tel:　090-1234-5678`

**Explanation:** In a mixed document, all half-width characters (including punctuation, numbers, and spaces) become full-width while kanji remain unchanged.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Numeric value passed instead of text | Convert number to text first with TEXT() or concatenate with "" |
| `No visible change` | Input already contains full-width characters | Expected behavior; JIS only converts half-width |
| `No visible change` | Input contains only kanji or hiragana | These are already full-width; function has no effect |
| `String length changed` | Voiced half-width katakana combined | Expected; separate base+mark combine to single character |
| `Layout issues` | Text became wider than expected | Full-width characters are twice as wide; adjust column widths |

## Use Cases

### [[Japanese Form Data Standardization]]
- **Implementation**: Convert user input to full-width format as required by Japanese official forms and systems. Many Japanese government and business forms expect all alphanumeric input in full-width characters.
- **Business Application**: Tax filing systems, bank account forms, official registration documents, and government submissions in Japan typically require full-width character input. Automated conversion ensures compliance.
- **Technical Details**: Apply JIS to form field data before submission or storage. Validate that output meets form field requirements. Consider combining with other normalization functions (UPPER, TRIM).

### [[Visual Text Alignment]]
- **Implementation**: Convert mixed text to full-width for consistent visual alignment in Japanese documents. Full-width characters align neatly in both horizontal and vertical layouts.
- **Business Application**: Professional documents, business cards, official correspondence, and publications benefit from consistent character width. Full-width ensures proper alignment in Japanese typography.
- **Technical Details**: Apply JIS to all text that will appear alongside kanji. Consider column width implications in spreadsheets. Test in the final output format (print, PDF, screen).

### [[Data Export for Japanese Systems]]
- **Implementation**: Convert data to full-width format before exporting to systems that expect or require full-width character encoding.
- **Business Application**: Legacy Japanese mainframe systems, certain databases, and specialized Japanese software may require full-width character input. Data integration with such systems requires conversion.
- **Technical Details**: Apply JIS during export preparation. Verify character encoding compatibility (Shift-JIS, EUC-JP, etc.). Test with actual target system to confirm acceptance.

### [[Vertical Text Preparation]]
- **Implementation**: Prepare text for vertical layout (tategaki) where full-width characters render correctly. Half-width characters may not display properly in vertical text flows.
- **Business Application**: Traditional Japanese documents, book formatting, signage, and formal invitations use vertical text. Full-width characters are essential for proper vertical rendering.
- **Technical Details**: Convert all text to full-width before applying vertical text formatting. Test in target rendering environment. Consider that some characters have different vertical forms.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | All versions with East Asian support | All versions |
| **Function Syntax** | =JIS(text) | =JIS(text) |
| **Alternative Name** | DBCS (same function) | JIS only |
| **Behavior** | Identical | Identical |
| **Kanji Handling** | Passes through unchanged | Passes through unchanged |
| **Hiragana Handling** | Passes through unchanged | Passes through unchanged |
| **Voiced Katakana** | Combines base + mark | Combines base + mark |
| **Array Support** | With BYROW/MAP or CSE | With ARRAYFORMULA |
| **Error on Number** | #VALUE! | #VALUE! |

**Key Notes:**

- JIS is fully compatible between Excel and Google Sheets
- In Excel, JIS and DBCS are synonymous (same function, two names)
- The function is standard on both platforms
- No behavioral differences exist for standard use cases

## Tips and Best Practices

1. **Know Your Target Requirements**: Verify whether full-width is actually required. Some modern Japanese systems accept half-width characters.

2. **Consider Display Width Impact**: Full-width characters are twice as wide. Plan column widths and text fields accordingly.

3. **Handle Voiced Katakana Carefully**: When converting half-width voiced katakana (with separate marks), the string length may decrease as marks combine. Test with representative data.

4. **Combine with ASC for Round-Trip Testing**: JIS(ASC(text)) should return the original if text was already full-width. ASC(JIS(text)) returns half-width. Test edge cases.

5. **Apply Before Layout Finalization**: Convert to full-width before finalizing document layouts, as character widths affect spacing and alignment.

6. **Use for Form Field Pre-Population**: When pre-filling Japanese forms programmatically, apply JIS to ensure correct formatting.

7. **Test with Actual Japanese Text**: Include variety of characters (Latin, numbers, katakana, punctuation) in test data to verify expected behavior.

8. **Consider DBCS as Alternative Name**: In some Excel documentation or code, you may see DBCS instead of JIS. They are the same function.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from JIS |
|----------|-------------|------------------------|
| [[ASC]] | Converts full-width to half-width | Inverse of JIS; converts to narrower characters |
| [[DBCS]] | Same as JIS (alternative name) | Identical function with different name |
| [[PHONETIC]] | Extracts phonetic (furigana) text | Returns pronunciation guide, not width conversion |

### Commonly Used Together

**[[ASC]]** - Convert to half-width

*Inverse operation for normalization:*
```excel
=ASC("ABC")
```
Returns half-width "ABC". Use when half-width format is required.

**[[TRIM]]** - Remove extra spaces

*Clean before conversion:*
```excel
=JIS(TRIM(A1))
```
Remove excess spaces before converting to full-width.

**[[UPPER]]/[[LOWER]]** - Standardize case

*Complete normalization:*
```excel
=JIS(UPPER(A1))
```
Convert to uppercase, then to full-width for consistent formatting.

**[[LEN]]** - Check string length

*Monitor length changes:*
```excel
=LEN(JIS(A1))
```
Length may decrease if voiced marks combine. Verify for field limits.

**[[SUBSTITUTE]]** - Replace specific characters

*Pre-process before conversion:*
```excel
=JIS(SUBSTITUTE(A1, "  ", " "))
```
Clean up data before width conversion.

**[[EXACT]]** - Compare strings precisely

*Compare after normalization:*
```excel
=EXACT(JIS(A1), JIS(B1))
```
Normalize both strings before exact comparison.

## Official Documentation

- **Microsoft Excel**: [JIS function](https://support.microsoft.com/en-us/office/jis-function-b72fb1a7-ba52-448a-b7d3-d2571f21113b)
- **Google Sheets**: [JIS function](https://support.google.com/docs/answer/10200385)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 97 | 1997 | Original implementation for Japanese localization |
| Excel 2007 | 2007 | Unicode improvements |
| Excel 2013 | 2013 | Enhanced East Asian language support |
| Google Sheets | Original | Available since launch with Japanese support |
| Excel 365 | 2020+ | Works with dynamic arrays via BYROW/MAP |

---

*Last updated: 2026-01-10*
