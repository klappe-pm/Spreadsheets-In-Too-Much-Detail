---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - text-manipulation
  - japanese-text
subTopics:
  - furigana-extraction
  - reading-guide
  - kanji-pronunciation
  - ruby-text
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - furigana
  - reading-text
  - pronunciation-guide
  - kanji-reading
tags:
  - text
  - japanese
  - furigana
  - phonetic
  - kanji
  - excel
  - google-sheets
---

# PHONETIC

## Description

PHONETIC is a specialized text function that extracts the phonetic reading (furigana) from Japanese text stored in cells. In Japanese, kanji characters can have multiple possible readings, and Excel and other applications allow users to specify the intended reading when entering text. This reading information, known as furigana, is stored as metadata alongside the visible text. The PHONETIC function retrieves this hidden phonetic information, returning the reading guide in hiragana or katakana. This is particularly important because the same kanji can be pronounced differently depending on context, and the furigana captures the intended pronunciation.

PHONETIC is essential in Japanese language applications where pronunciation matters. Common use cases include generating furigana for documents and publications, creating sorted lists by pronunciation (Japanese sorting is often phonetic rather than by character code), extracting readings for text-to-speech applications, verifying correct kanji readings in educational materials, and supporting accessibility features for Japanese content. The function is also useful for data validation and quality control when working with Japanese names, which often have non-standard readings that need to be explicitly stored and retrieved.

There are several important gotchas when using PHONETIC. First, the function only returns phonetic data that was explicitly entered when the original text was input; if no furigana was specified during input, PHONETIC returns the original text unchanged. Second, phonetic information is only available when text was entered through Japanese input methods that support furigana or when explicitly added in Excel's phonetic guide feature. Third, the function returns hiragana by default in Excel, but the format can depend on how the data was entered. Fourth, phonetic information may be lost when data is copied between applications or imported from external sources. Fifth, the function has limited use outside Japanese text processing.

Regarding platform availability, PHONETIC is available in Microsoft Excel across all versions with Japanese language support. In Google Sheets, PHONETIC has limited functionality and may not work as expected with all Japanese text because Google Sheets handles phonetic information differently than Excel. For comprehensive Japanese text processing, Excel typically provides more robust support. When working with Japanese data that requires phonetic extraction, verify that the phonetic information is preserved when moving data between applications.

## Syntax

> [!f(x)] PHONETIC Syntax
>
> ```excel
> =PHONETIC(reference)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `reference` | A cell reference containing text with phonetic (furigana) information. Must be a single cell reference or a range of contiguous cells. | Yes |

## Examples

### Example 1: Basic Phonetic Extraction
```excel
=PHONETIC(A1)
```
Where A1 contains "東京" with furigana "とうきょう"

**Result:** `とうきょう`

**Explanation:** Extracts the hiragana reading "toukyou" from the kanji "Tokyo". The furigana must have been entered when typing the text.

---

### Example 2: Name Reading Extraction
```excel
=PHONETIC(B2)
```
Where B2 contains "山田太郎" with furigana "やまだたろう"

**Result:** `やまだたろう`

**Explanation:** Japanese names often have important furigana because the same kanji can have different readings. PHONETIC extracts the intended reading.

---

### Example 3: No Phonetic Information
```excel
=PHONETIC(A1)
```
Where A1 contains "東京" without furigana

**Result:** `東京`

**Explanation:** When no phonetic information exists, PHONETIC returns the original text unchanged. This happens when text was pasted or imported without furigana data.

---

### Example 4: Katakana Phonetic Return
```excel
=PHONETIC(A1)
```
Where A1 contains "日本" with katakana furigana "ニホン"

**Result:** `ニホン`

**Explanation:** PHONETIC returns the furigana in whatever form it was entered, whether hiragana or katakana.

---

### Example 5: Range of Cells
```excel
=PHONETIC(A1:A3)
```
Where A1:A3 contains names with furigana

**Result:** Combined phonetic readings from all cells

**Explanation:** When given a range, PHONETIC concatenates the phonetic readings from all cells in the range.

---

### Example 6: Company Name Reading
```excel
=PHONETIC(C5)
```
Where C5 contains "株式会社" with furigana "かぶしきがいしゃ"

**Result:** `かぶしきがいしゃ`

**Explanation:** Business terms in Japanese often have standard readings extracted via PHONETIC.

---

### Example 7: Mixed Content
```excel
=PHONETIC(A1)
```
Where A1 contains "ABC東京123" with partial furigana "とうきょう"

**Result:** `ABCとうきょう123`

**Explanation:** Non-Japanese characters (Latin letters, numbers) pass through unchanged; only kanji with furigana are converted.

---

### Example 8: Product Name Sorting Key
```excel
=PHONETIC(ProductName)
```
**Result:** Phonetic reading suitable for Japanese alphabetical sorting

**Explanation:** Japanese is often sorted by phonetic reading (50-on order) rather than by kanji. PHONETIC provides the sort key.

---

### Example 9: Hiragana Already
```excel
=PHONETIC(A1)
```
Where A1 contains "ひらがな" (all hiragana)

**Result:** `ひらがな`

**Explanation:** Text already in hiragana or katakana returns as-is since it needs no phonetic guide.

---

### Example 10: Address Component
```excel
=PHONETIC(Prefecture)
```
Where Prefecture contains "北海道" with furigana "ほっかいどう"

**Result:** `ほっかいどう`

**Explanation:** Geographic names like prefectures have standard readings that PHONETIC extracts.

---

### Example 11: Concatenated with Original
```excel
=A1 & "(" & PHONETIC(A1) & ")"
```
Where A1 contains "漢字" with furigana "かんじ"

**Result:** `漢字(かんじ)`

**Explanation:** Common pattern to display kanji with reading in parentheses for accessibility or learning materials.

---

### Example 12: Sorting Helper Column
```excel
=PHONETIC(B2)
```
Used as a helper column for SORT function

**Result:** Phonetic text used as sort key

**Explanation:** Create a helper column with PHONETIC values, then sort by that column to achieve Japanese phonetic ordering.

---

### Example 13: Name with Unusual Reading
```excel
=PHONETIC(A1)
```
Where A1 contains "小鳥遊" with furigana "たかなし"

**Result:** `たかなし`

**Explanation:** Some Japanese names have unusual readings that can't be guessed from the kanji. PHONETIC captures the intended reading.

---

### Example 14: Checking for Phonetic Data
```excel
=IF(A1=PHONETIC(A1), "No furigana", "Has furigana")
```
**Result:** "No furigana" or "Has furigana"

**Explanation:** If PHONETIC returns the same as the original, no phonetic information exists. This formula detects whether furigana is present.

---

### Example 15: Text-to-Speech Preparation
```excel
=PHONETIC(A1)
```
**Result:** Reading suitable for TTS systems

**Explanation:** Text-to-speech systems for Japanese often need phonetic readings to pronounce kanji correctly. PHONETIC provides this.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Reference is not a valid cell reference | Ensure the argument is a cell reference (like A1), not a text string |
| `Returns original text` | No phonetic information stored with the text | Re-enter text using Japanese IME with furigana, or add phonetic guide in Excel |
| `#NAME?` | Function not available (rare) | Verify Excel has Japanese language support installed |
| `Incomplete phonetic` | Only some characters have furigana | Add phonetic information to remaining characters in Excel |
| `Wrong reading returned` | Furigana was entered incorrectly | Edit the cell's phonetic guide in Excel to correct the reading |

## Use Cases

### [[Japanese Name Processing]]
- **Implementation**: Extract phonetic readings from Japanese names for sorting, searching, and display purposes. Store both kanji and phonetic versions in databases.
- **Business Application**: Customer databases, employee directories, contact management systems, and any application handling Japanese personal names need phonetic readings for proper sorting and searching.
- **Technical Details**: Japanese names particularly require phonetic storage because the same kanji can have many readings. Always capture furigana during data entry. Use PHONETIC to create sort keys and search indices.

### [[Document Ruby Text Generation]]
- **Implementation**: Generate furigana (ruby text) for documents, educational materials, and publications. Extract readings for annotation above kanji.
- **Business Application**: Educational publishing, children's materials, language learning content, and accessibility-compliant documents all need kanji with reading guides.
- **Technical Details**: PHONETIC extracts the reading; formatting for ruby display requires document features (Word, HTML ruby tags, etc.). Ensure source data has phonetic information before extraction.

### [[Japanese Phonetic Sorting]]
- **Implementation**: Create sort keys based on phonetic readings rather than character codes. Japanese is typically sorted in gojuon (50-sound) order by pronunciation.
- **Business Application**: Directory listings, index generation, alphabetized lists in Japanese documents, and any sorted Japanese content need phonetic sort keys.
- **Technical Details**: Create a helper column with =PHONETIC(name_column), then sort by that column. Katakana and hiragana sort differently; normalize to one form if needed.

### [[Text-to-Speech Preparation]]
- **Implementation**: Provide phonetic readings to text-to-speech engines that need pronunciation guidance for kanji characters.
- **Business Application**: Accessibility features, automated announcements, voice synthesis applications, and audio content generation from Japanese text.
- **Technical Details**: TTS engines need phonetic readings to pronounce kanji correctly. PHONETIC extracts the intended reading. Verify TTS system's expected input format (hiragana vs. katakana vs. romaji).

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | All versions with Japanese support | Limited functionality |
| **Function Syntax** | =PHONETIC(reference) | =PHONETIC(reference) |
| **Furigana Storage** | Full support | Limited/different handling |
| **Return Format** | As entered (hiragana/katakana) | May vary |
| **Range Support** | Concatenates readings | May differ |
| **Phonetic Guide Feature** | Built-in editing | Not available |
| **IME Integration** | Full support | Browser-dependent |

**Key Notes:**

- Excel has superior Japanese text support with full phonetic guide features
- Google Sheets may not preserve or properly handle furigana from all sources
- When working with Japanese text requiring phonetic extraction, Excel is recommended
- Test PHONETIC behavior when moving data between platforms

**Excel Phonetic Guide Feature:**

Excel provides a "Phonetic Guide" feature to add or edit furigana:
1. Select the cell with Japanese text
2. Format > Phonetic Guide > Show/Edit
3. Add or modify the reading above the kanji

This feature is not available in Google Sheets.

## Tips and Best Practices

1. **Enter Furigana During Input**: The best way to have phonetic information is to enter it when typing using a Japanese IME. The reading used during conversion is captured automatically.

2. **Check for Phonetic Data**: Use `=IF(A1=PHONETIC(A1), "No", "Yes")` to check whether cells contain phonetic information before relying on it.

3. **Use for Sort Keys**: When sorting Japanese text, create a helper column with PHONETIC values and sort by that column for proper gojuon order.

4. **Preserve When Copying**: Be aware that copying Japanese text may lose phonetic information depending on the method and destination. Use Paste Special > Values and Formats when possible.

5. **Edit with Phonetic Guide**: Use Excel's Format > Phonetic Guide feature to add or correct readings for existing text.

6. **Normalize Script**: If consistent output is needed, consider converting PHONETIC results to all hiragana or all katakana using SUBSTITUТЕ patterns or other functions.

7. **Handle Missing Data Gracefully**: Not all Japanese text will have furigana. Design formulas to handle cases where PHONETIC returns the original text.

8. **Document Japanese-Specific Logic**: When building Japanese-language applications, document where and how phonetic information is captured, stored, and used.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from PHONETIC |
|----------|-------------|------------------------------|
| [[ASC]] | Full-width to half-width conversion | Character width, not reading |
| [[JIS]] | Half-width to full-width conversion | Character width, not reading |
| [[KATAKANA]] | Convert to katakana (custom) | Not built-in; would change script type |

### Commonly Used Together

**[[SORT]]** - Sort by phonetic reading

*Use PHONETIC as sort key:*
```excel
=SORT(A2:B100, PHONETIC(A2:A100), 1)
```
Sort Japanese names by their phonetic reading in gojuon order.

**[[CONCATENATE]]/[[&]]** - Combine with original

*Display kanji with reading:*
```excel
=A1 & "(" & PHONETIC(A1) & ")"
```
Create display format showing kanji followed by reading in parentheses.

**[[IF]]** - Check for phonetic data

*Conditional based on furigana presence:*
```excel
=IF(A1=PHONETIC(A1), "Add reading", A1)
```
Flag entries missing phonetic information.

**[[SEARCH]]/[[FIND]]** - Search phonetic strings

*Search by reading:*
```excel
=IF(ISNUMBER(SEARCH("やま", PHONETIC(A1))), "Match", "No match")
```
Search for partial readings within phonetic text.

**[[LEN]]** - Count phonetic characters

*Check reading length:*
```excel
=LEN(PHONETIC(A1))
```
Count characters in the phonetic reading.

## Official Documentation

- **Microsoft Excel**: [PHONETIC function](https://support.microsoft.com/en-us/office/phonetic-function-0a9804e7-beda-4fc1-928c-18b0c95fb0fb)
- **Google Sheets**: [PHONETIC function](https://support.google.com/docs/answer/3094402) (limited functionality)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 97 | 1997 | Original implementation for Japanese Excel |
| Excel 2007 | 2007 | Unicode improvements |
| Excel 2013 | 2013 | Enhanced phonetic guide editing |
| Google Sheets | Varies | Limited support for phonetic extraction |
| Excel 365 | Ongoing | Continued Japanese language support |

---

*Last updated: 2026-01-10*
