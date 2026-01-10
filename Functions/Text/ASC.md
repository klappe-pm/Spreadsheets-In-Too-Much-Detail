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
  - fullwidth-to-halfwidth
  - katakana-conversion
  - japanese-text
  - asian-characters
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - fullwidth-to-halfwidth
  - wide-to-narrow
  - katakana-halfwidth
  - dbcs-to-sbcs
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

# ASC

## Description

ASC is a text function that converts full-width (double-byte) characters to half-width (single-byte) characters in languages that use double-byte character sets (DBCS), primarily Japanese. Full-width characters occupy two bytes and display at the same width as Japanese kanji characters, while half-width characters occupy one byte and display at half that width. The ASC function specifically targets full-width katakana, Latin letters, numbers, and symbols, converting them to their half-width equivalents while leaving other characters (like kanji and hiragana) unchanged. The function name "ASC" derives from "ASCII," reflecting its purpose of converting characters toward the ASCII-compatible half-width representation.

ASC is essential when working with Japanese text data that needs to be standardized, compared, or processed consistently. Full-width and half-width versions of the same character are treated as different characters by text comparison functions, meaning "ABC" (full-width) and "ABC" (half-width) would not match. Common use cases include normalizing imported data from various sources that may use inconsistent character widths, preparing text for database operations that expect half-width characters, cleaning up customer input in Japanese systems, standardizing product codes or identifiers, and ensuring consistent formatting for reports and exports.

There are several important gotchas when using ASC. First, the function only converts characters that have half-width equivalents; kanji and hiragana do not have half-width versions and pass through unchanged. Second, some characters change representation when converted: for example, full-width voiced katakana (like "ga") may become two half-width characters (base character plus voiced mark). Third, the function is specific to DBCS languages; it has no effect on standard Western European text. Fourth, character encoding matters: the function assumes proper encoding of the input text. Fifth, the length of the resulting string may change due to voiced/semi-voiced marks becoming separate characters.

Regarding platform availability, ASC is available in both Microsoft Excel and Google Sheets with identical syntax and functionality. Both platforms support the function across all versions where Japanese language support is available. The function is most relevant when working with Japanese text, though it technically affects any text containing full-width characters. In environments where East Asian language support is not installed or enabled, the function still works but may not be needed. The inverse function, JIS, converts half-width characters to full-width.

## Syntax

> [!f(x)] ASC Syntax
>
> ```excel
> =ASC(text)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `text` | The text string or reference to a cell containing text to convert from full-width to half-width characters. Any characters without half-width equivalents remain unchanged. | Yes |

## Examples

### Example 1: Convert Full-Width Latin Letters
```excel
=ASC("ABC")
```
**Result:** `ABC`

**Explanation:** Full-width Latin letters "ABC" (each character 2 bytes wide) are converted to standard half-width ASCII letters "ABC" (each character 1 byte). Visually, the full-width letters appear wider and more spaced.

---

### Example 2: Convert Full-Width Numbers
```excel
=ASC("12345")
```
**Result:** `12345`

**Explanation:** Full-width numbers are converted to half-width. This is common in Japanese data entry where the keyboard input method may default to full-width characters.

---

### Example 3: Convert Full-Width Katakana
```excel
=ASC("カタカナ")
```
**Result:** `カタカナ` (half-width katakana)

**Explanation:** Full-width katakana characters are converted to their half-width equivalents. Half-width katakana are commonly used in legacy systems and some display contexts.

---

### Example 4: Mixed Content Conversion
```excel
=ASC("Test123テスト")
```
**Result:** `Test123ﾃｽﾄ` (Latin and numbers half-width, katakana half-width)

**Explanation:** ASC converts all convertible characters: full-width Latin letters, numbers, and katakana. Mixed text is processed character by character.

---

### Example 5: Kanji Pass-Through
```excel
=ASC("東京ABC")
```
**Result:** `東京ABC`

**Explanation:** Kanji characters (like "Tokyo") have no half-width equivalents and pass through unchanged. Only the full-width "ABC" is converted to half-width.

---

### Example 6: Cell Reference Conversion
```excel
=ASC(A1)
```
Where A1 contains "HELLO"

**Result:** `HELLO`

**Explanation:** ASC works with cell references just as with literal text. The full-width text in A1 is converted to half-width.

---

### Example 7: Normalizing Phone Numbers
```excel
=ASC("090-1234-5678")
```
**Result:** `090-1234-5678`

**Explanation:** Japanese phone numbers entered with full-width digits are converted to standard half-width for consistent storage and comparison.

---

### Example 8: Empty String Handling
```excel
=ASC("")
```
**Result:** `` (empty string)

**Explanation:** An empty string returns an empty string. No error is generated.

---

### Example 9: Already Half-Width Text
```excel
=ASC("Already half-width")
```
**Result:** `Already half-width`

**Explanation:** Text that is already half-width passes through unchanged. ASC does not double-convert or alter half-width characters.

---

### Example 10: Voiced Katakana Conversion
```excel
=ASC("ガギグゲゴ")
```
**Result:** `ｶﾞｷﾞｸﾞｹﾞｺﾞ`

**Explanation:** Voiced katakana (ga, gi, gu, ge, go) become base character plus voiced mark (dakuten) when converted to half-width. The string length increases.

---

### Example 11: Combining with TRIM
```excel
=TRIM(ASC(A1))
```
**Result:** Cleaned, half-width text with extra spaces removed

**Explanation:** Common pattern to both normalize character width and clean up spacing. Full-width spaces become half-width, then TRIM removes extras.

---

### Example 12: Data Validation Preparation
```excel
=EXACT(ASC(A1), ASC(B1))
```
**Result:** TRUE or FALSE

**Explanation:** When comparing Japanese text, normalize both sides with ASC first. This ensures full-width "ABC" matches half-width "ABC" after conversion.

---

### Example 13: Converting Full-Width Punctuation
```excel
=ASC("（Hello）")
```
**Result:** `(Hello)`

**Explanation:** Full-width parentheses and other punctuation marks are converted to half-width equivalents.

---

### Example 14: Nested with UPPER
```excel
=UPPER(ASC(A1))
```
**Result:** Half-width, uppercase text

**Explanation:** Convert to half-width first, then apply UPPER for consistent uppercase formatting. The order can matter for some edge cases.

---

### Example 15: Building Standardized Identifiers
```excel
=SUBSTITUTE(ASC(A1), " ", "")
```
**Result:** Half-width text with spaces removed

**Explanation:** Normalize to half-width, then remove spaces to create clean identifiers suitable for database keys or file names.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Numeric value passed instead of text | Convert number to text first with TEXT() or concatenate with "" |
| `No visible change` | Input already contains half-width characters | This is expected behavior; ASC only converts full-width |
| `No visible change` | Input contains kanji or hiragana | These have no half-width equivalents; use only on appropriate content |
| `String length changed` | Voiced katakana split into base + mark | Expected behavior; voiced marks become separate characters in half-width |
| `Unexpected characters` | Encoding issues in source data | Verify source data encoding is correct (UTF-8 or appropriate) |

## Use Cases

### [[Japanese Data Entry Normalization]]
- **Implementation**: Apply ASC to all user input fields in Japanese applications to ensure consistent character width storage. Create input validation rules that normalize data before database insertion.
- **Business Application**: Customer relationship management systems, order entry forms, address databases, product catalog management, and any system accepting Japanese text input need consistent data representation for accurate searching and matching.
- **Technical Details**: Apply ASC immediately after data entry or import. Store normalized data in databases. Create lookup formulas that normalize both the search term and target data. Consider creating a UDF or macro for bulk normalization.

### [[Product Code and ID Standardization]]
- **Implementation**: Normalize product codes, SKUs, customer IDs, and other identifiers that may be entered in either full-width or half-width format. Create standardization formulas for all identifier fields.
- **Business Application**: Inventory systems, order processing, customer lookup, barcode systems, and cross-system data integration all require consistent identifier formatting to function correctly.
- **Technical Details**: Combine ASC with UPPER, TRIM, and SUBSTITUTE for complete normalization: =SUBSTITUTE(TRIM(UPPER(ASC(A1))), " ", ""). Store normalized versions alongside or instead of raw input.

### [[Text Comparison and Matching]]
- **Implementation**: Normalize text before comparison operations to ensure full-width and half-width versions of the same content are treated as matches. Use ASC on both sides of comparisons.
- **Business Application**: Duplicate detection in customer databases, fuzzy matching for data deduplication, search functionality in Japanese applications, and reconciliation of data from multiple sources.
- **Technical Details**: For VLOOKUP, MATCH, and similar: =VLOOKUP(ASC(search_term), ASC(lookup_range), col, FALSE). Note that ASC on a range requires array formula or BYROW/MAP in modern Excel.

### [[Export Data Preparation]]
- **Implementation**: Convert data to half-width format before exporting to systems that expect ASCII or half-width characters. Prepare data for legacy systems, APIs, or international integration.
- **Business Application**: EDI (Electronic Data Interchange) exports, API integrations with Western systems, legacy mainframe data feeds, international data sharing, and systems with byte-length constraints.
- **Technical Details**: Apply ASC to all text columns in export preparation. Verify that downstream systems handle the output correctly. Consider that string length may change (increase) due to voiced marks.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | All versions with East Asian language support | All versions |
| **Function Syntax** | =ASC(text) | =ASC(text) |
| **Behavior** | Identical | Identical |
| **Kanji Handling** | Passes through unchanged | Passes through unchanged |
| **Hiragana Handling** | Passes through unchanged | Passes through unchanged |
| **Voiced Katakana** | Splits to base + mark | Splits to base + mark |
| **Array Support** | With BYROW/MAP or CSE | With ARRAYFORMULA |
| **Error on Number** | #VALUE! | #VALUE! |

**Key Notes:**

- ASC is fully compatible between Excel and Google Sheets
- The function is part of the standard function set on both platforms
- No behavioral differences exist between platforms for standard use cases
- Both platforms handle Unicode text appropriately

## Tips and Best Practices

1. **Always Normalize Before Comparing**: When comparing Japanese text, apply ASC (and possibly UPPER, LOWER, or TRIM) to both values being compared. "ABC" (full-width) and "ABC" (half-width) are different strings without normalization.

2. **Understand Voiced Mark Behavior**: Voiced katakana (ga, gi, gu, etc.) become two characters (base + mark) when converted to half-width. Plan for string length changes in fixed-length fields.

3. **Combine with JIS for Round-Trip**: To convert to full-width, use JIS. Note that ASC(JIS(text)) may not equal the original if voiced marks are involved, as they separate and recombine.

4. **Apply at Data Entry**: Normalize data as early as possible in the workflow, ideally at entry or import. This prevents issues downstream and ensures consistent storage.

5. **Don't Expect Kanji/Hiragana Changes**: ASC only affects characters with half-width equivalents. Kanji and hiragana pass through unchanged.

6. **Use for Space Normalization**: Full-width spaces are converted to half-width, which then allows TRIM to work as expected for removing extra spaces.

7. **Test with Actual Japanese Text**: When developing formulas, test with actual full-width and half-width Japanese text samples to ensure expected behavior.

8. **Consider LEN Changes**: The LEN of a string may change after ASC conversion. If using LEN for validation, normalize first.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from ASC |
|----------|-------------|------------------------|
| [[JIS]] | Converts half-width to full-width | Inverse of ASC; converts katakana, Latin, numbers to full-width |
| [[DBCS]] | Converts half-width to full-width | Same as JIS; alternative name used in some contexts |
| [[PHONETIC]] | Extracts phonetic (furigana) text | Returns pronunciation guide, not width conversion |

### Commonly Used Together

**[[JIS]]** - Convert to full-width

*Convert half-width to full-width (inverse operation):*
```excel
=JIS("ABC")
```
Returns full-width "ABC". Use when full-width format is required.

**[[TRIM]]** - Remove extra spaces

*Clean up text after normalization:*
```excel
=TRIM(ASC(A1))
```
Normalize character width, then remove leading, trailing, and duplicate spaces.

**[[UPPER]]/[[LOWER]]** - Standardize case

*Complete text normalization:*
```excel
=UPPER(ASC(A1))
```
Convert to half-width and uppercase for consistent identifiers.

**[[SUBSTITUTE]]** - Remove specific characters

*Create clean identifiers:*
```excel
=SUBSTITUTE(ASC(A1), " ", "")
```
Normalize width and remove spaces.

**[[EXACT]]** - Case-sensitive comparison

*Compare normalized text:*
```excel
=EXACT(ASC(A1), ASC(B1))
```
Normalize both strings before exact comparison.

**[[LEN]]** - Get string length

*Check length after normalization:*
```excel
=LEN(ASC(A1))
```
Note that length may change due to voiced mark splitting.

## Official Documentation

- **Microsoft Excel**: [ASC function](https://support.microsoft.com/en-us/office/asc-function-0b6abf1c-c663-4004-a964-ebc00b723266)
- **Google Sheets**: [ASC function](https://support.google.com/docs/answer/3093312)

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
