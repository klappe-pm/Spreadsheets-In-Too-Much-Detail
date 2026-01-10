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
- text-analysis
- character-identification
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Character Code
- ASCII Code
- Character To Number
- Get Character Code
tags:
- text
- character
- ascii
- unicode
- code-conversion
- analysis
---

# CODE

## Description

**CODE** returns the numeric code for the first character in a text string. This function is the inverse of CHAR: while CHAR converts numbers to characters, CODE converts characters to numbers. CODE is essential for identifying invisible or problematic characters in data, understanding character sequences for sorting and comparison, and diagnosing data quality issues that other functions cannot reveal.

The function examines only the first character of the provided text, returning its numeric code based on the character set (typically ASCII/ANSI for standard characters). For example, CODE("A") returns 65, CODE("a") returns 97, and CODE(" ") returns 32 for a standard space. This enables you to detect the difference between a regular space (32) and a non-breaking space (160), or between a standard hyphen (45) and other dash characters.

**Key diagnostic applications:** When data appears identical but formulas fail to match, CODE helps identify invisible character differences. Text that "looks right but doesn't work" often contains wrong space types (32 vs 160), wrong quote types (34 vs curly quotes), or hidden control characters (codes below 32).

**Platform consideration:** CODE returns different values on Windows and Mac for characters 128-255 due to different character encodings (Windows-1252 vs Mac Roman). For characters 1-127 (standard ASCII), all platforms return identical values. Google Sheets uses UTF-8 encoding which may differ for extended characters. For consistent results with non-ASCII characters, consider UNICODE.

## Syntax

> [!f(x)] CODE Syntax
>
> ```
> =CODE(text)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string for which you want the code of the first character. Only the first character is examined. |

### Return Value

Returns a number representing the character code of the first character in the text string. Returns #VALUE! if text is empty.

## Examples

> [!f(x)] CODE Examples

### Example 1: Uppercase Letter
```
=CODE("A")
```
**Result:** 65

**Explanation:** ASCII code for uppercase A is 65. Codes 65-90 are A-Z.

---

### Example 2: Lowercase Letter
```
=CODE("a")
```
**Result:** 97

**Explanation:** ASCII code for lowercase a is 97. Codes 97-122 are a-z. Difference of 32 between upper and lower.

---

### Example 3: Space Character
```
=CODE(" ")
```
**Result:** 32

**Explanation:** Standard space is ASCII 32. Use this to distinguish from non-breaking space (160).

---

### Example 4: Only First Character Matters
```
=CODE("Hello")
```
**Result:** 72

**Explanation:** Only examines the first character "H" (code 72). Remaining characters are ignored.

---

### Example 5: Digit Character
```
=CODE("7")
```
**Result:** 55

**Explanation:** Digit characters have codes 48-57 for 0-9. The character "7" has code 55.

---

### Example 6: Cell Reference
```
=CODE(A1)
```
Where A1 contains "Test"

**Result:** 84

**Explanation:** Returns code for "T" (first character of "Test"). Works with cell references same as literal text.

---

### Example 7: Detect Non-Breaking Space
```
=CODE(MID(A1, 5, 1))
```
Where position 5 contains a suspicious space

**Result:** 160 (if non-breaking space) or 32 (if regular space)

**Explanation:** Diagnostic technique: extract specific character with MID, then check its code to identify invisible character type.

---

### Example 8: Special Character Identification
```
=CODE("-")
```
**Result:** 45

**Explanation:** Standard hyphen-minus is 45. Different from en-dash (8211) or em-dash (8212) in Unicode.

---

### Example 9: Line Break Detection
```
=CODE(MID(A1, FIND(CHAR(10), A1), 1))
```
**Result:** 10 (if line feed found)

**Explanation:** Confirms a line break character exists at the found position. Useful for validating multi-line content.

---

### Example 10: Quote Character
```
=CODE("""")
```
**Result:** 34

**Explanation:** Double quote character has code 34. Note the escaping: four quotes to represent one quote in formula.

---

### Example 11: Tab Character Detection
```
=IF(CODE(A1)=9, "Starts with Tab", "No leading Tab")
```
**Result:** Identifies cells starting with tab character

**Explanation:** Tab character has code 9. Invisible but can cause data issues.

---

### Example 12: Control Character Detection
```
=IF(CODE(A1)<32, "Has Control Character", "Clean")
```
**Result:** Flags text starting with non-printable control character

**Explanation:** ASCII codes 0-31 are control characters. Presence indicates data quality issues.

---

### Example 13: Character Code Table Generation
```
=CHAR(ROW(A1)) & " = " & ROW(A1)
```
Copied down from row 32 to 126

**Result:** " = 32", "! = 33", etc. (printable character table)

**Explanation:** Generates reference table of characters and their codes.

---

### Example 14: Identify Invisible Difference
```
=IF(CODE(A1)=CODE(B1), "Same first char", "Different: " & CODE(A1) & " vs " & CODE(B1))
```
**Result:** Reveals character code differences even when text looks identical

**Explanation:** When text "looks the same" but doesn't match, this reveals invisible differences.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Empty text string provided | Check for empty cells; use IF to handle empty cases. |
| `#VALUE!` | Numeric value provided instead of text | Ensure input is text; use TEXT() to convert numbers if needed. |
| `Wrong code on different platform` | Windows/Mac encoding differences for codes 128-255 | Use UNICODE for extended characters; stick to ASCII 1-127 for consistency. |
| `Unexpected code value` | Character looks like one thing but is another | Legitimate finding: character isn't what you assumed. Common with spaces and dashes. |

## Use Cases

### [[Invisible Character Diagnosis]]

**Scenario:** Data imported from external systems fails to match in VLOOKUPs even though values appear identical. Need to identify hidden character differences.

**Implementation:**
```
First Char Code: =CODE(A2)
All Character Codes: =CODE(MID(A2, ROW(INDIRECT("1:"&LEN(A2))), 1))
Comparison: =IF(CODE(A2)=CODE(B2), "Match", CODE(A2) & " vs " & CODE(B2))
```

**Business Application:** Data quality troubleshooting, import validation, and matching failure diagnosis. When "identical" text doesn't match, CODE reveals the hidden differences.

**Technical Details:** Regular space is 32, non-breaking space is 160. Standard hyphen is 45, en-dash is 8211. Standard quotes are 34/39, curly quotes are 8216-8221. Compare expected codes to diagnose issues.

---

### [[Data Validation and Quality Control]]

**Scenario:** Incoming data needs validation to ensure it contains only expected characters. Flag entries with control characters, wrong character types, or unexpected symbols.

**Implementation:**
```
Has Control Char: =CODE(A2)<32
Starts with Letter: =OR(AND(CODE(A2)>=65, CODE(A2)<=90), AND(CODE(A2)>=97, CODE(A2)<=122))
Starts with Digit: =AND(CODE(A2)>=48, CODE(A2)<=57)
```

**Business Application:** Import validation, form data checking, and data quality dashboards. Prevent bad data from entering systems by validating character types.

**Technical Details:** Letters: 65-90 (A-Z), 97-122 (a-z). Digits: 48-57 (0-9). Control characters: 0-31. Space: 32. Use ranges to validate character categories.

---

### [[Character-Based Sorting and Comparison]]

**Scenario:** Need to understand or manipulate how text sorts. Custom sorting logic based on character values rather than default text comparison.

**Implementation:**
```
Sort Key: =CODE(LEFT(A2, 1)) * 1000 + CODE(MID(A2, 2, 1))
Alphabetic Only: =IF(CODE(A2)<65, "Symbol/Number First", "Letter First")
Case Check: =IF(CODE(A2)<=90, "UPPERCASE", "lowercase or other")
```

**Business Application:** Custom sort orders, data categorization, and encoding analysis. Understand character-level data structure for complex processing.

**Technical Details:** Upper/lower case differ by 32 (A=65, a=97). This enables case conversion: CHAR(CODE(A2)-32) converts lowercase to uppercase (for a-z range).

---

### [[Cross-Platform Character Verification]]

**Scenario:** Spreadsheet will be used on both Windows and Mac, or data comes from multiple platforms. Need to verify character consistency.

**Implementation:**
```
Standard ASCII Check: =IF(CODE(A2)<=127, "Safe", "Extended character - may differ")
Identify Problematic: =IF(AND(CODE(A2)>127, CODE(A2)<256), "Platform-specific range", "OK")
```

**Business Application:** Cross-platform spreadsheet deployment, data exchange between systems, and internationalization quality control.

**Technical Details:** ASCII 1-127 is identical everywhere. Codes 128-255 may differ between Windows-1252, Mac Roman, and UTF-8. For maximum compatibility, restrict to ASCII range or use UNICODE for extended characters.

## Platform Differences

### Microsoft Excel (Windows)
- **Availability:** All versions
- **Character set:** Windows-1252 for codes 128-255
- **Extended support:** UNICODE function for full Unicode analysis
- **Empty string:** Returns #VALUE!

### Microsoft Excel (Mac)
- **Character set:** Mac Roman for codes 128-255 (different from Windows)
- **ASCII 1-127:** Identical to Windows
- **Recommendation:** Use UNICODE for extended characters

### Google Sheets
- **Availability:** All versions
- **Character set:** UTF-8 encoding
- **Extended characters:** May return different values for codes 128-255
- **Empty string:** Returns #VALUE!

### Key Difference Alert
For ASCII codes 1-127, all platforms return identical values. This includes all English letters, digits, common punctuation, and control characters. Codes 128-255 may differ between platforms. To ensure consistent results for extended characters, use the UNICODE function when available, or restrict analysis to ASCII range.

## Tips and Best Practices

1. **Memorize key codes:** Space=32, 0=48, A=65, a=97. These reference points help interpret any code quickly (add 1 for each subsequent character).

2. **Use CODE to diagnose matching failures:** When VLOOKUP or comparison fails on "identical" text, use CODE on both values to reveal hidden character differences.

3. **Check for non-breaking spaces:** If CODE(space_character) returns 160 instead of 32, you found a non-breaking space. Use SUBSTITUTE(A1, CHAR(160), " ") to fix.

4. **Detect control characters:** Any CODE value below 32 indicates a control character (tab, line break, etc.). Use CLEAN to remove them.

5. **Case conversion math:** Uppercase and lowercase differ by 32. A=65, a=97. You can convert case by adding/subtracting 32 (within proper ranges).

6. **Extract and check:** Use `=CODE(MID(A1, position, 1))` to check specific character positions within a string.

7. **CODE only checks first character:** For multi-character analysis, loop through using MID or use array formulas to examine each position.

8. **Combine with CHAR:** CODE and CHAR are inverses. If CODE(A1)=X, then CHAR(X)=A1 (first character). Use both for character manipulation.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[UNICODE]] | Returns Unicode code point | For characters outside ASCII range or cross-platform consistency |
| [[CHAR]] | Returns character from code | To convert code numbers back to characters |

### Commonly Used Together

**[[CHAR]]** - Returns character from code

*Inverse of CODE:*
```
=CHAR(CODE("A")) returns "A"
=CODE(CHAR(65)) returns 65
```
CODE and CHAR are inverse operations for character/code conversion.

---

**[[MID]]** - Extract specific character

*Check character at specific position:*
```
=CODE(MID(A1, 5, 1))
```
MID extracts character at position, CODE identifies what it is.

---

**[[CLEAN]]** - Remove non-printable characters

*Validate against CLEAN:*
```
=IF(CODE(A1)<32, "Would be removed by CLEAN", "Printable")
```
Understand what CLEAN will remove by checking character codes.

---

**[[SUBSTITUTE]]** - Replace specific characters

*Replace based on code identification:*
```
=IF(CODE(MID(A1, 1, 1))=160, SUBSTITUTE(A1, CHAR(160), " "), A1)
```
Use CODE to identify problem characters, SUBSTITUTE to fix them.

---

**[[UNICODE]]** - Full Unicode code point

*For extended character analysis:*
```
=UNICODE(A1) for Unicode point
=CODE(A1) for ANSI/ASCII
```
UNICODE handles characters beyond 255 consistently across platforms.

---

**[[LEN]]** - String length

*Analyze all characters:*
```
=CODE(MID(A1, 1, 1)) through =CODE(MID(A1, LEN(A1), 1))
```
Combine LEN with MID and CODE to analyze every character in a string.

---

**[[IFERROR]]** - Error handling

*Handle empty strings:*
```
=IFERROR(CODE(A1), 0)
```
CODE returns #VALUE! for empty strings; IFERROR provides graceful handling.

## Official Documentation

- **Microsoft Excel:** [CODE function](https://support.microsoft.com/en-us/office/code-function-c32b692b-2ed0-4a04-bdd9-75640144b928)
- **Google Sheets:** [CODE function](https://support.google.com/docs/answer/3094121)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function |
| Excel 2013+ | UNICODE added | Extended Unicode support via separate function |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | UTF-8 encoding |

---

*Last updated: 2026-01-10*
