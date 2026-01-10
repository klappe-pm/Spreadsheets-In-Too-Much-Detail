---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- text-replacement
- positional-replacement
- string-manipulation
- data-transformation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Replace By Position
- Positional Replace
- Character Replace
- String Replace
tags:
- text
- replacement
- position
- manipulation
- transformation
- substring
---

# REPLACE

## Description

**REPLACE** substitutes part of a text string with different text based on character position. Unlike SUBSTITUTE, which replaces specific text wherever it appears, REPLACE operates by location: you specify exactly where to start replacing and how many characters to replace. This positional approach is ideal for fixed-format data like codes, phone numbers, and identifiers where certain character positions always contain specific information.

The function requires four parameters: the original text, the starting position for replacement, the number of characters to remove, and the replacement text. This precision allows you to insert, delete, or replace portions of text with surgical accuracy. For example, you can replace characters 5-8 of a string while leaving everything else intact, regardless of what those characters actually contain.

**Key gotcha:** REPLACE uses 1-based positioning, where the first character is position 1, not 0. This differs from many programming languages. Also, the replacement text does not need to be the same length as the removed section. Replacing 3 characters with 5 characters simply lengthens the result; replacing 3 with 1 shortens it. To insert text without removing anything, use 0 as the num_chars parameter.

**Platform consideration:** Excel and Google Sheets implement REPLACE identically. Both use 1-based positioning, both support variable-length replacements, and both allow insertion by specifying 0 characters to replace. The function is fully compatible across platforms with no behavioral differences.

## Syntax

> [!f(x)] REPLACE Syntax
>
> ```
> =REPLACE(old_text, start_num, num_chars, new_text)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `old_text` | Yes | The original text string in which you want to replace characters. |
| `start_num` | Yes | The position of the first character to replace. Position 1 is the first character. |
| `num_chars` | Yes | The number of characters to replace. Use 0 to insert without removing characters. |
| `new_text` | Yes | The text to insert. Can be any length, including empty string "" to delete characters. |

### Return Value

Returns a new text string with the specified characters replaced. The original text is not modified. If start_num exceeds the text length, new_text is appended. If num_chars is 0, new_text is inserted at the position without removing any characters.

## Examples

> [!f(x)] REPLACE Examples

### Example 1: Basic Character Replacement
```
=REPLACE("ABCDEFGH", 3, 2, "XX")
```
**Result:** "ABXXEFGH"

**Explanation:** Starting at position 3, replace 2 characters ("CD") with "XX". The result maintains the same length because replacement equals removed length.

---

### Example 2: Replace More with Fewer Characters
```
=REPLACE("ABCDEFGH", 3, 4, "X")
```
**Result:** "ABXGH"

**Explanation:** Removes 4 characters ("CDEF") and replaces with 1 character ("X"). The result is shorter than the original.

---

### Example 3: Replace Fewer with More Characters
```
=REPLACE("ABCDEFGH", 3, 1, "XXXXX")
```
**Result:** "ABXXXXXDEFGH"

**Explanation:** Removes 1 character ("C") and inserts 5 characters. The result is longer than the original.

---

### Example 4: Insert Text Without Replacing (num_chars = 0)
```
=REPLACE("ABCDEFGH", 3, 0, "INSERT")
```
**Result:** "ABINSERTCDEFGH"

**Explanation:** With num_chars = 0, no characters are removed. "INSERT" is placed before position 3 ("C"). Powerful technique for inserting text at specific positions.

---

### Example 5: Delete Characters (empty new_text)
```
=REPLACE("ABCDEFGH", 3, 3, "")
```
**Result:** "ABFGH"

**Explanation:** Removes 3 characters ("CDE") and replaces with nothing. Effectively deletes characters at the specified position.

---

### Example 6: Cell Reference
```
=REPLACE(A1, 5, 4, "XXXX")
```
Where A1 contains "123-456-7890"

**Result:** "123-XXXX-7890"

**Explanation:** Replaces the area code with "XXXX". Useful for data masking in fixed-format strings.

---

### Example 7: Replace First Characters
```
=REPLACE("OldPrefix-12345", 1, 9, "NewPrefix")
```
**Result:** "NewPrefix-12345"

**Explanation:** Starting at position 1, replace the first 9 characters. Useful for prefix replacement in structured codes.

---

### Example 8: Replace Last Characters
```
=REPLACE(A1, LEN(A1)-3, 4, "XXXX")
```
Where A1 contains "1234-5678"

**Result:** "1234-XXXX"

**Explanation:** Uses LEN to calculate position from the end. Replaces the last 4 characters with "XXXX".

---

### Example 9: Mask Credit Card Number
```
=REPLACE(A1, 5, 8, "XXXX-XXXX")
```
Where A1 contains "1234-5678-9012-3456"

**Explanation:** Masks the middle 8 characters of a credit card number, leaving first and last 4 visible.

---

### Example 10: Format Phone Number
```
=REPLACE(REPLACE(A1, 7, 0, "-"), 4, 0, "-")
```
Where A1 contains "1234567890"

**Result:** "123-456-7890"

**Explanation:** Nested REPLACE inserts hyphens at positions without removing any digits. First insert at position 7, then at position 4.

---

### Example 11: Remove Middle Section
```
=REPLACE(A1, 4, 5, "")
```
Where A1 contains "ABC12345XYZ"

**Result:** "ABCXYZ"

**Explanation:** Removes characters from position 4 through 8 (5 characters). Useful for stripping unwanted middle portions.

---

### Example 12: Dynamic Start Position with FIND
```
=REPLACE(A1, FIND("-", A1), 1, "/")
```
Where A1 contains "2024-01-15"

**Result:** "2024/01-15"

**Explanation:** FIND locates the first hyphen, then REPLACE changes it to a slash. Combines positional and content-based logic.

---

### Example 13: Replace at End Using LEN
```
=REPLACE(A1, LEN(A1)-2, 3, "NEW")
```
Where A1 contains "FileOLD"

**Result:** "FileNEW"

**Explanation:** Calculates start position relative to string end using LEN. Replaces the last 3 characters.

---

### Example 14: Append Text (start beyond length)
```
=REPLACE(A1, LEN(A1)+1, 0, "-SUFFIX")
```
Where A1 contains "PREFIX"

**Result:** "PREFIX-SUFFIX"

**Explanation:** When start_num is beyond the string length, new_text is appended. Equivalent to concatenation but uses REPLACE syntax.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | start_num is less than 1 | Position must be 1 or greater. Use 1 to start at the beginning. |
| `#VALUE!` | num_chars is negative | num_chars must be 0 or positive. Use 0 to insert without removing. |
| `#VALUE!` | Non-numeric start_num or num_chars | Ensure position and length parameters are numbers, not text. |
| `Unexpected result length` | num_chars doesn't match new_text length | This is normal behavior; results can be any length. |
| `Wrong characters replaced` | Off-by-one counting error | Remember position 1 is the first character, not position 0. |

## Use Cases

### [[Data Masking for Privacy]]

**Scenario:** Customer data containing SSNs, credit card numbers, or phone numbers needs to be partially masked for display in reports or shared documents.

**Implementation:**
```
SSN: =REPLACE(A2, 1, 5, "XXX-XX")
Credit Card: =REPLACE(A2, 5, 8, "XXXX-XXXX")
Phone: =REPLACE(A2, 1, 6, "(XXX) XXX")
```

**Business Application:** Compliance with data protection regulations (GDPR, HIPAA, PCI-DSS). Allows data to be usable for analysis while protecting sensitive portions.

**Technical Details:** Identify fixed positions in formatted data. Replace sensitive portions with mask characters while preserving visible portions for identification. Combine with RIGHT to show last digits.

---

### [[Fixed-Format Code Transformation]]

**Scenario:** Product codes need reformatting from old system format "AAA-12345" to new format "AAA/12345/US". The hyphen becomes a slash and a suffix is added.

**Implementation:**
```
=REPLACE(A2, 4, 1, "/") & "/US"
```
Or for complete transformation:
```
=REPLACE(REPLACE(A2, 4, 1, "/"), LEN(REPLACE(A2, 4, 1, "/"))+1, 0, "/US")
```

**Business Application:** System migrations, inventory management, and data standardization. Ensures codes match new system requirements without manual reformatting.

**Technical Details:** REPLACE handles the positional substitution, concatenation or nested REPLACE adds suffixes. For bulk transformations, create helper columns then paste values.

---

### [[Inserting Formatting Characters]]

**Scenario:** Raw numeric data needs formatting characters inserted at specific positions: phone numbers need hyphens, dates need slashes, codes need delimiters.

**Implementation:**
```
Phone (10 digits to XXX-XXX-XXXX):
=REPLACE(REPLACE(A2, 7, 0, "-"), 4, 0, "-")

Date (MMDDYYYY to MM/DD/YYYY):
=REPLACE(REPLACE(A2, 5, 0, "/"), 3, 0, "/")
```

**Business Application:** Data presentation, report formatting, and export preparation. Transforms machine-readable data into human-readable formats.

**Technical Details:** Use num_chars = 0 for insertion without deletion. Work from right to left when inserting multiple characters to avoid position shifts affecting subsequent operations.

---

### [[Updating Version Strings]]

**Scenario:** Software version strings need to be updated in documentation or configuration files. Version "v2.1.3" needs to become "v2.2.0" by replacing specific portion.

**Implementation:**
```
=REPLACE(A2, 4, 3, "2.0")
```
Or dynamically:
```
=REPLACE(A2, FIND(".", A2)+1, LEN(A2)-FIND(".", A2), "2.0")
```

**Business Application:** Documentation updates, configuration management, and release automation. Ensures version strings are consistently updated across files.

**Technical Details:** For fixed formats, use static positions. For variable formats, combine FIND with REPLACE to locate the portion to replace dynamically.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 onward)
- **Positioning:** 1-based (first character is position 1)
- **Zero chars:** num_chars = 0 inserts without deleting
- **Beyond length:** start_num beyond string length appends text
- **Unicode:** Counts each character as 1, including multi-byte characters

### Google Sheets
- **Availability:** All versions since launch
- **Behavior:** Identical to Excel in all parameters
- **Array support:** Works with ARRAYFORMULA for range processing
- **Unicode:** Same handling as Excel
- **REPLACEB:** Not available (Excel only for byte-based replacement)

### Key Difference Alert
Both platforms implement REPLACE identically. The only difference is that Excel offers REPLACEB for byte-based positioning in double-byte character sets (Asian languages), which Google Sheets lacks. For standard Western text, this is not a concern. All formulas using REPLACE are fully portable between platforms.

## Tips and Best Practices

1. **Remember 1-based indexing:** The first character is position 1, not 0. This catches programmers coming from other languages. Position 1 is always the start.

2. **Use 0 for insertion:** `=REPLACE(A1, 5, 0, "INSERT")` places text before position 5 without removing anything. Powerful for adding formatting characters.

3. **Empty string for deletion:** `=REPLACE(A1, 3, 4, "")` removes 4 characters starting at position 3. More precise than SUBSTITUTE for positional deletion.

4. **Combine with LEN for end-relative positioning:** `=REPLACE(A1, LEN(A1)-2, 3, "NEW")` targets the last 3 characters. Essential for suffix replacement.

5. **Combine with FIND for content-based positioning:** `=REPLACE(A1, FIND("@", A1), 1, "")` uses content to determine position. Bridges REPLACE and SUBSTITUTE approaches.

6. **Work right-to-left for multiple insertions:** When inserting multiple strings, start from the rightmost position to avoid affecting positions of earlier insertions.

7. **Use for data masking:** REPLACE is ideal for partially masking sensitive data. Replace middle portions of SSNs, credit cards, and phone numbers with X's.

8. **Nest for complex transformations:** Multiple REPLACE calls can handle complex reformatting tasks. Each REPLACE operates on the result of the previous one.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SUBSTITUTE]] | Replaces text by content match | When you know what text to replace but not its position |
| [[MID]] | Extracts characters by position | When you need to extract rather than replace |

### Commonly Used Together

**[[LEN]]** - Returns string length

*Calculate positions relative to end:*
```
=REPLACE(A1, LEN(A1)-3, 4, "NEW")
```
Essential for replacing characters at the end of variable-length strings.

---

**[[FIND]]** / **[[SEARCH]]** - Locate text position

*Dynamic positioning based on content:*
```
=REPLACE(A1, FIND("-", A1), 1, "/")
```
Find the position of a character, then use REPLACE to modify it.

---

**[[LEFT]]** / **[[RIGHT]]** / **[[MID]]** - Text extraction

*Combine extraction with replacement:*
```
=LEFT(A1, 3) & REPLACE(MID(A1, 4, LEN(A1)), 1, 4, "XXXX")
```
Extract portions to preserve, replace others.

---

**[[SUBSTITUTE]]** - Content-based replacement

*Choose based on need:*
```
Position known: =REPLACE(A1, 5, 3, "NEW")
Content known: =SUBSTITUTE(A1, "OLD", "NEW")
```
REPLACE for positional, SUBSTITUTE for content-based replacement.

---

**[[CONCATENATE]]** / **[[&]]** - Text joining

*Alternative to insertion:*
```
=LEFT(A1, 4) & "-" & MID(A1, 5, LEN(A1))
Same as: =REPLACE(A1, 5, 0, "-")
```
Concatenation can achieve similar results; REPLACE is often more concise.

---

**[[TEXT]]** - Format numbers as text

*Prepare numeric data for REPLACE:*
```
=REPLACE(TEXT(A1, "0000000000"), 4, 0, "-")
```
Convert numbers to fixed-length text before positional replacement.

---

**[[REPT]]** - Repeat text

*Create mask characters:*
```
=REPLACE(A1, 5, 8, REPT("X", 8))
```
Generate consistent mask patterns for data privacy.

## Official Documentation

- **Microsoft Excel:** [REPLACE function](https://support.microsoft.com/en-us/office/replace-replaceb-functions-8d799074-2425-4a8a-84bc-82472868878a)
- **Google Sheets:** [REPLACE function](https://support.google.com/docs/answer/3094142)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | Identical implementation to Excel |

---

*Last updated: 2026-01-10*
