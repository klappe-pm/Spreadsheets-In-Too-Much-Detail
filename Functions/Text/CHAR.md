---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- character-conversion
- ascii-codes
- special-characters
- text-generation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Character From Code
- ASCII Character
- Code To Character
- Special Character
tags:
- text
- character
- ascii
- unicode
- code-conversion
- special-characters
---

# CHAR

## Description

**CHAR** returns the character specified by a numeric code. This function translates numbers into characters based on your computer's character set, enabling you to work with characters that cannot be typed directly on a keyboard. Common uses include inserting line breaks within cells, adding tab characters for formatting, working with non-breaking spaces, and generating special symbols or characters from different languages.

The function accepts a number between 1 and 255 (for standard ASCII/ANSI characters) or higher Unicode values in modern versions. It returns the corresponding character from the character set. For example, CHAR(65) returns "A", CHAR(10) returns a line feed (line break), and CHAR(9) returns a tab character. This makes CHAR essential for data cleaning (targeting invisible characters), text formatting (inserting line breaks), and generating special characters.

**Key applications:** The most commonly used CHAR codes are: CHAR(10) for line feed (new line within cell), CHAR(13) for carriage return, CHAR(9) for tab, CHAR(32) for space, CHAR(160) for non-breaking space, and CHAR(34) for double quote (useful when building strings containing quotes).

**Platform consideration:** Excel and Google Sheets handle CHAR slightly differently for extended character sets. Both support standard ASCII (1-127) identically. For codes 128-255, Windows Excel uses Windows-1252 encoding while Mac Excel uses Mac Roman, which can produce different characters. Google Sheets uses UTF-8. For standard ASCII characters (1-127), all platforms are identical.

## Syntax

> [!f(x)] CHAR Syntax
>
> ```
> =CHAR(number)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | A number between 1 and 255 specifying the character. Numbers outside this range may work in modern versions for Unicode but standard range is 1-255. |

### Return Value

Returns a single character corresponding to the specified code number. Returns #VALUE! if the number is outside valid range or cannot be converted.

## Examples

> [!f(x)] CHAR Examples

### Example 1: Basic Letter (A)
```
=CHAR(65)
```
**Result:** "A"

**Explanation:** ASCII code 65 is uppercase A. Codes 65-90 are A-Z.

---

### Example 2: Lowercase Letter (a)
```
=CHAR(97)
```
**Result:** "a"

**Explanation:** ASCII code 97 is lowercase a. Codes 97-122 are a-z.

---

### Example 3: Line Break in Cell (Line Feed)
```
="Line 1" & CHAR(10) & "Line 2"
```
**Result:**
Line 1
Line 2

**Explanation:** CHAR(10) inserts a line break. Text appears on two lines within the same cell (requires Wrap Text enabled to display).

---

### Example 4: Tab Character
```
="Column1" & CHAR(9) & "Column2"
```
**Result:** "Column1	Column2" (with tab between)

**Explanation:** CHAR(9) inserts a tab character. Useful for creating tab-delimited output.

---

### Example 5: Non-Breaking Space
```
="Word" & CHAR(160) & "Word"
```
**Result:** "Word Word" (with non-breaking space)

**Explanation:** CHAR(160) is the non-breaking space, often found in web data. Looks like a space but TRIM doesn't remove it.

---

### Example 6: Double Quote in String
```
="He said " & CHAR(34) & "Hello" & CHAR(34)
```
**Result:** He said "Hello"

**Explanation:** CHAR(34) is the double quote character. Essential for building strings that contain quotes.

---

### Example 7: Carriage Return + Line Feed (Windows Line Break)
```
="Line 1" & CHAR(13) & CHAR(10) & "Line 2"
```
**Result:** Two-line text with Windows-style line ending

**Explanation:** Windows typically uses CR+LF (CHAR(13) + CHAR(10)) for line breaks. Mac/Linux use just LF (CHAR(10)).

---

### Example 8: Number to Character Range
```
=CHAR(ROW()+64)
```
In rows 1-26

**Result:** A, B, C, D... Z (one letter per row)

**Explanation:** Generates sequential letters based on row number. ROW()+64 gives 65, 66, 67... which are A, B, C...

---

### Example 9: Bullet Point Character
```
=CHAR(149) & " Item 1"
```
**Result:** "o Item 1" (with bullet character)

**Explanation:** CHAR(149) is the bullet point in Windows-1252. Creates visual lists without manual formatting.

---

### Example 10: Copyright Symbol
```
=CHAR(169)
```
**Result:** "c" (copyright symbol)

**Explanation:** CHAR(169) produces the copyright symbol. Similar: CHAR(174) for registered trademark.

---

### Example 11: Degree Symbol
```
="Temperature: 72" & CHAR(176) & "F"
```
**Result:** "Temperature: 72 degrees F"

**Explanation:** CHAR(176) is the degree symbol. Useful for temperature and angular measurements.

---

### Example 12: Build Alphabet String
```
=CONCAT(CHAR(ROW(INDIRECT("A65:A90"))))
```
**Result:** "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

**Explanation:** Generates complete alphabet by converting codes 65-90 to letters.

---

### Example 13: Remove Non-Breaking Space
```
=SUBSTITUTE(A1, CHAR(160), " ")
```
**Result:** Text with non-breaking spaces converted to regular spaces

**Explanation:** Common cleaning pattern: use CHAR to reference the invisible non-breaking space for replacement.

---

### Example 14: Create Multi-Line Cell Content
```
="Name: " & A1 & CHAR(10) & "Email: " & B1 & CHAR(10) & "Phone: " & C1
```
**Result:** Three-line formatted text in a single cell

**Explanation:** CHAR(10) creates line breaks for structured, multi-line display in cells.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Number outside valid range (1-255 or Unicode range) | Ensure number is within valid range for your platform. |
| `#VALUE!` | Non-numeric input | Ensure parameter is a number, not text. Use VALUE() if needed. |
| `Wrong character appears` | Platform encoding differences | Codes 128-255 differ between Windows, Mac, and web. Use Unicode functions (UNICHAR) for consistency. |
| `Line break not showing` | Cell wrap text not enabled | Enable Wrap Text in cell formatting to display line breaks. |
| `Tab not visible` | Display shows nothing | Tab characters are invisible; they affect spacing but show as blank. |

## Use Cases

### [[Inserting Line Breaks in Formulas]]

**Scenario:** Need to create multi-line cell content dynamically using formulas, such as formatted address blocks or structured data displays.

**Implementation:**
```
Address Block: =A2 & CHAR(10) & B2 & ", " & C2 & " " & D2 & CHAR(10) & E2
Multi-Line Label: ="Product: " & A2 & CHAR(10) & "SKU: " & B2 & CHAR(10) & "Price: $" & C2
```

**Business Application:** Report formatting, label generation, and dynamic content creation. Multi-line cells enable rich formatting within spreadsheet constraints.

**Technical Details:** Cell must have Wrap Text enabled to display line breaks. CHAR(10) is line feed (works everywhere); some Windows apps expect CHAR(13) & CHAR(10). Row height may need adjustment.

---

### [[Cleaning Non-Breaking Spaces]]

**Scenario:** Data from web sources or documents contains non-breaking spaces (CHAR 160) that look like regular spaces but cause matching failures and resist TRIM.

**Implementation:**
```
Convert NBSP: =SUBSTITUTE(A2, CHAR(160), " ")
Full Clean: =TRIM(CLEAN(SUBSTITUTE(A2, CHAR(160), " ")))
Multiple Invisible: =SUBSTITUTE(SUBSTITUTE(A2, CHAR(160), " "), CHAR(173), "")
```

**Business Application:** Data import cleaning, web scraping processing, and document data extraction. Non-breaking spaces are the most common invisible problem character.

**Technical Details:** CHAR(160) is non-breaking space; CHAR(173) is soft hyphen. Neither is removed by CLEAN or TRIM. Use SUBSTITUTE with CHAR to target them specifically.

---

### [[Generating Special Characters]]

**Scenario:** Need to include special characters (degree, currency, copyright, registered trademark) in calculated text without relying on manual entry.

**Implementation:**
```
Temperature: =A2 & CHAR(176) & "C"
Copyright: =CHAR(169) & " 2024 Company Name"
Currency: =CHAR(163) & FORMAT(A2, "#,##0.00")
Registered: ="Product Name" & CHAR(174)
```

**Business Application:** Document generation, report headers, and dynamic text creation. Ensures special characters appear correctly regardless of keyboard or locale.

**Technical Details:** Character codes differ between Windows-1252 and other encodings. For cross-platform compatibility, consider UNICHAR for extended characters. Common codes: 169=c, 174=(R), 176=degree, 163=pounds.

---

### [[Creating Tab-Delimited Output]]

**Scenario:** Need to generate tab-delimited text for export to other systems or for proper alignment in non-spreadsheet contexts.

**Implementation:**
```
Tab-Separated: =A2 & CHAR(9) & B2 & CHAR(9) & C2
With Header: ="Name" & CHAR(9) & "Email" & CHAR(9) & "Phone"
```

**Business Application:** Data export, clipboard operations, and text file generation. Tab-delimited format is widely compatible with text editors and import tools.

**Technical Details:** CHAR(9) is the tab character. When copied to clipboard, tab-delimited cells paste correctly into many applications. Use TEXTJOIN with CHAR(9) delimiter for multiple columns.

## Platform Differences

### Microsoft Excel (Windows)
- **Availability:** All versions
- **Character set:** Windows-1252 for codes 128-255
- **Extended support:** UNICHAR function for Unicode above 255
- **Line break display:** Requires Wrap Text enabled

### Microsoft Excel (Mac)
- **Character set:** Mac Roman for codes 128-255 (may differ from Windows)
- **Recommendation:** Use ASCII 1-127 or UNICHAR for cross-platform compatibility

### Google Sheets
- **Availability:** All versions
- **Character set:** UTF-8 encoding
- **Extended support:** Higher Unicode values may work directly
- **Line break display:** Wrap text enabled by default in many cases

### Key Difference Alert
For ASCII codes 1-127, all platforms are identical. Codes 128-255 may produce different characters on Windows Excel, Mac Excel, and Google Sheets due to different character encodings. For reliability:
- Use ASCII 1-127 only for guaranteed consistency
- Use UNICHAR for extended characters when available
- Test specific characters on your target platform

## Tips and Best Practices

1. **Memorize key codes:** CHAR(10)=line break, CHAR(9)=tab, CHAR(32)=space, CHAR(160)=non-breaking space, CHAR(34)=double quote. These are used constantly.

2. **Enable Wrap Text for line breaks:** CHAR(10) line breaks only display when cell formatting has Wrap Text enabled. Otherwise, breaks are invisible.

3. **Use CHAR for cleaning web data:** Non-breaking spaces (CHAR 160) from web pages are invisible troublemakers. Always include `SUBSTITUTE(A1, CHAR(160), " ")` when cleaning web data.

4. **Build quotes into strings:** Use `CHAR(34)` to include double quotes in formula-generated text: `="He said " & CHAR(34) & "Hello" & CHAR(34)`.

5. **Generate sequential letters:** `=CHAR(ROW()+64)` creates A, B, C... based on row number. Useful for auto-generated labels.

6. **Test extended characters:** Codes 128-255 may display differently on Windows, Mac, and web. Test on your target platform or use UNICHAR for reliability.

7. **Combine with SUBSTITUTE for cleaning:** `=SUBSTITUTE(A1, CHAR(x), "")` removes specific invisible characters. Essential for data cleaning workflows.

8. **Use CODE as inverse:** CODE returns the number for a character, CHAR returns the character for a number. They are inverse functions.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[UNICHAR]] | Returns Unicode character from code | For characters above 255 or cross-platform Unicode consistency |
| [[CODE]] | Returns numeric code for character | To identify what character code a specific character has |

### Commonly Used Together

**[[CODE]]** - Returns character code

*Inverse of CHAR:*
```
=CODE("A") returns 65
=CHAR(65) returns "A"
```
CODE and CHAR are inverse operations for character/code conversion.

---

**[[SUBSTITUTE]]** - Text replacement

*Target specific characters by code:*
```
=SUBSTITUTE(A1, CHAR(160), " ")
```
Use CHAR to reference invisible characters for removal or replacement.

---

**[[CONCATENATE]]** / **[[&]]** - Text joining

*Insert special characters in strings:*
```
="Line 1" & CHAR(10) & "Line 2"
```
CHAR provides special characters to insert between text elements.

---

**[[CLEAN]]** - Remove non-printable characters

*Complement CLEAN with CHAR-based substitution:*
```
=CLEAN(SUBSTITUTE(A1, CHAR(160), " "))
```
CLEAN removes ASCII 0-31; SUBSTITUTE with CHAR handles what CLEAN misses.

---

**[[UNICHAR]]** - Unicode character

*For extended character support:*
```
=UNICHAR(8364) for Euro symbol
```
UNICHAR handles characters beyond the 255 limit of CHAR.

---

**[[ROW]]** / **[[COLUMN]]** - Position functions

*Generate sequential characters:*
```
=CHAR(ROW()+64)
```
Combine with ROW/COLUMN for dynamic character generation.

---

**[[TEXT]]** - Number formatting

*Combine with special character output:*
```
=TEXT(A1, "#,##0") & CHAR(176) & "C"
```
Format numbers and append special characters.

## Official Documentation

- **Microsoft Excel:** [CHAR function](https://support.microsoft.com/en-us/office/char-function-bbd249c8-b36e-4a91-8017-1c133f9b837a)
- **Google Sheets:** [CHAR function](https://support.google.com/docs/answer/3094120)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function |
| Excel 2013+ | UNICHAR added | Extended Unicode support via separate function |
| Excel 365 | Continuous updates | Full Unicode support |
| Google Sheets | Original launch (2006) | UTF-8 encoding support |

---

*Last updated: 2026-01-10*
