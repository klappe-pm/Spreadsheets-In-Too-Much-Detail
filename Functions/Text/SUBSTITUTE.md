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
- data-cleaning
- string-manipulation
- find-replace
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Find and Replace
- Text Replace
- String Replace
- Replace Text
tags:
- text
- replacement
- cleaning
- substitution
- find-replace
- manipulation
---

# SUBSTITUTE

## Description

**SUBSTITUTE** replaces occurrences of a specified text string with a new text string. Unlike REPLACE, which operates by position, SUBSTITUTE works by matching the actual text content you want to replace. This makes SUBSTITUTE the go-to function for targeted text replacement when you know what you want to change but not necessarily where it appears in the string. Use SUBSTITUTE for data cleaning, text standardization, character removal, and any scenario where you need to find and replace specific text patterns.

The function searches for all occurrences of the old text within your string and replaces them with the new text. By default, it replaces every occurrence, but an optional fourth parameter allows you to target only the first, second, third, or nth occurrence. This selective replacement is valuable when text appears multiple times but you only want to change a specific instance, such as changing only the first comma in an address.

**Critical gotcha:** SUBSTITUTE is case-sensitive. "Apple" and "apple" are treated as different strings. If you need case-insensitive replacement, you must either normalize the case first with UPPER or LOWER, or use multiple SUBSTITUTE calls to handle variations. Additionally, SUBSTITUTE searches for exact matches; it cannot use wildcards or patterns. For pattern matching, consider REGEXREPLACE in Google Sheets or a VBA solution in Excel.

**Platform consideration:** Excel and Google Sheets implement SUBSTITUTE identically. Both are case-sensitive, both support the optional instance_num parameter, and both allow nesting for multiple replacements. Google Sheets additionally offers REGEXREPLACE for pattern-based replacements, which Excel lacks (without VBA).

## Syntax

> [!f(x)] SUBSTITUTE Syntax
>
> ```
> =SUBSTITUTE(text, old_text, new_text, [instance_num])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string or cell reference containing text in which you want to make substitutions. |
| `old_text` | Yes | The text you want to replace. Case-sensitive exact match required. |
| `new_text` | Yes | The replacement text. Use "" (empty string) to delete the old_text. |
| `instance_num` | No | Specifies which occurrence of old_text to replace. If omitted, all occurrences are replaced. 1 = first occurrence, 2 = second, etc. |

### Return Value

Returns the text string with specified substitutions made. If old_text is not found, the original text is returned unchanged. If new_text is empty (""), the old_text is effectively deleted from the string.

## Examples

> [!f(x)] SUBSTITUTE Examples

### Example 1: Basic Text Replacement
```
=SUBSTITUTE("Hello World", "World", "Universe")
```
**Result:** "Hello Universe"

**Explanation:** Replaces "World" with "Universe". Simple, direct text substitution.

---

### Example 2: Replace All Occurrences
```
=SUBSTITUTE("one-two-three-four", "-", " ")
```
**Result:** "one two three four"

**Explanation:** Replaces all hyphens with spaces. Default behavior replaces every occurrence.

---

### Example 3: Replace Specific Occurrence
```
=SUBSTITUTE("one-two-three-four", "-", " ", 2)
```
**Result:** "one-two three-four"

**Explanation:** Only the second hyphen is replaced with a space. The fourth parameter (2) targets the specific occurrence.

---

### Example 4: Delete Text (Replace with Nothing)
```
=SUBSTITUTE("Phone: (123) 456-7890", "(", "")
```
**Result:** "Phone: 123) 456-7890"

**Explanation:** Using empty string "" as new_text effectively deletes the old_text. Combine multiple calls to remove all formatting characters.

---

### Example 5: Remove All Parentheses and Hyphens
```
=SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A1, "(", ""), ")", ""), "-", "")
```
Where A1 contains "(123) 456-7890"

**Result:** "123 4567890"

**Explanation:** Nested SUBSTITUTE removes multiple different characters. Each SUBSTITUTE handles one character type.

---

### Example 6: Case Sensitivity Demonstration
```
=SUBSTITUTE("Apple apple APPLE", "apple", "orange")
```
**Result:** "Apple orange APPLE"

**Explanation:** Only "apple" (lowercase) is replaced. "Apple" and "APPLE" remain unchanged due to case sensitivity.

---

### Example 7: Replace Non-Breaking Spaces
```
=SUBSTITUTE(A1, CHAR(160), " ")
```
Where A1 contains text copied from web with non-breaking spaces

**Result:** Text with regular spaces

**Explanation:** Web content often has non-breaking spaces (CHAR 160) that TRIM cannot remove. SUBSTITUTE converts them to regular spaces.

---

### Example 8: Line Break Removal
```
=SUBSTITUTE(SUBSTITUTE(A1, CHAR(10), " "), CHAR(13), " ")
```
**Result:** Multi-line text converted to single line with spaces

**Explanation:** CHAR(10) is line feed, CHAR(13) is carriage return. Replace both to flatten multi-line text.

---

### Example 9: Cell Reference for Replacement
```
=SUBSTITUTE(A1, B1, C1)
```
Where A1 = "Hello World", B1 = "World", C1 = "Everyone"

**Result:** "Hello Everyone"

**Explanation:** All parameters can be cell references, enabling dynamic replacement rules.

---

### Example 10: Count Occurrences Using SUBSTITUTE
```
=(LEN(A1)-LEN(SUBSTITUTE(A1, "a", "")))/LEN("a")
```
Where A1 contains "banana"

**Result:** 3

**Explanation:** Classic technique: compare length before and after removing a character to count its occurrences.

---

### Example 11: Replace First Occurrence Only
```
=SUBSTITUTE("Mr. Smith and Mrs. Smith", "Smith", "Jones", 1)
```
**Result:** "Mr. Jones and Mrs. Smith"

**Explanation:** Only the first "Smith" is replaced. Useful when you need surgical precision in replacements.

---

### Example 12: Creating Slug/URL from Title
```
=LOWER(SUBSTITUTE(SUBSTITUTE(TRIM(A1), " ", "-"), "'", ""))
```
Where A1 contains "John's Article Title"

**Result:** "johns-article-title"

**Explanation:** Replaces spaces with hyphens, removes apostrophes, and lowercases. Common pattern for URL slug generation.

---

### Example 13: Standardize Delimiters
```
=SUBSTITUTE(SUBSTITUTE(A1, ";", ","), "|", ",")
```
**Result:** All semicolons and pipes converted to commas

**Explanation:** Normalize different delimiter styles to a standard format for consistent data processing.

---

### Example 14: Extract Domain from Email
```
=SUBSTITUTE(A1, LEFT(A1, FIND("@", A1)), "")
```
Where A1 contains "user@example.com"

**Result:** "example.com"

**Explanation:** Removes everything up to and including @ symbol by substituting the left portion with nothing.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | One or more parameters is not text | Ensure all inputs are text or cell references containing text. |
| `Text unchanged` | old_text not found (case mismatch) | Check for case sensitivity. Use UPPER/LOWER for case-insensitive replacement. |
| `Only partial replacement` | Multiple variations of text (different cases) | Use multiple SUBSTITUTE calls or normalize case first. |
| `Wrong occurrence replaced` | instance_num counting issue | Occurrences are counted left to right. Verify which instance you need. |
| `Unexpected results` | Hidden characters in data | Use CLEAN first or check for non-printable characters with CODE. |

## Use Cases

### [[Phone Number Standardization]]

**Scenario:** Phone numbers collected from various sources have inconsistent formatting: "(123) 456-7890", "123-456-7890", "123.456.7890", "1234567890". Need to standardize to a single format.

**Implementation:**
```
Strip to digits: =SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A2, "(", ""), ")", ""), "-", ""), ".", ""), " ", "")

Format as (XXX) XXX-XXXX:
=IF(LEN(B2)=10, "("&LEFT(B2,3)&") "&MID(B2,4,3)&"-"&RIGHT(B2,4), "Invalid")
```

**Business Application:** CRM data quality, telemarketing lists, and customer communication systems. Standardized phone numbers enable duplicate detection and proper dialing.

**Technical Details:** Chain SUBSTITUTE to remove all formatting characters first, leaving only digits. Then use LEFT, MID, RIGHT to reformat to your standard. Validate length to catch data errors.

---

### [[Data Import Character Cleanup]]

**Scenario:** Data imported from external systems contains unwanted characters: line breaks, tabs, non-breaking spaces, and special symbols that break downstream processing.

**Implementation:**
```
=TRIM(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A2, CHAR(10), " "), CHAR(13), " "), CHAR(9), " "), CHAR(160), " "))
```

**Business Application:** ETL processes, data migration, and integration projects. Clean data reduces errors in processing and improves data quality in target systems.

**Technical Details:** CHAR(10)=line feed, CHAR(13)=carriage return, CHAR(9)=tab, CHAR(160)=non-breaking space. Replace all with regular spaces, then TRIM normalizes the spacing.

---

### [[SKU/Product Code Transformation]]

**Scenario:** A new inventory system requires SKU format change from "CAT-12345-A" to "CAT_12345_A". Need to transform thousands of existing codes.

**Implementation:**
```
=SUBSTITUTE(A2, "-", "_")
```
For more complex transformations:
```
=UPPER(SUBSTITUTE(SUBSTITUTE(A2, "-", "_"), " ", "_"))
```

**Business Application:** System migrations, inventory management, and product data synchronization between platforms. Consistent formatting prevents lookup failures.

**Technical Details:** SUBSTITUTE handles pattern replacement; combine with UPPER/LOWER for case standardization. Test with sample data before applying to full dataset.

---

### [[Name Prefix/Suffix Removal]]

**Scenario:** Contact list includes titles and suffixes that should be removed for certain uses: "Dr. John Smith, Jr." should become "John Smith".

**Implementation:**
```
=TRIM(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A2, "Dr. ", ""), "Mr. ", ""), "Mrs. ", ""), ", Jr.", ""))
```

**Business Application:** Mail merge personalization, data matching, and CRM deduplication. Clean names without titles enable better matching and more flexible addressing.

**Technical Details:** Order matters when titles might overlap. Include the space after titles to prevent partial matches. Consider a more comprehensive approach with a lookup table of titles to remove.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 onward)
- **Case sensitivity:** Always case-sensitive, no option to change
- **Instance targeting:** Supports instance_num parameter for selective replacement
- **No wildcards:** Cannot use wildcards or patterns; exact match only
- **Nesting:** Supports multiple nested SUBSTITUTE calls

### Google Sheets
- **Availability:** All versions since launch
- **Behavior:** Identical to Excel for all parameters
- **Array support:** Works with ARRAYFORMULA for range processing
- **Additional option:** REGEXREPLACE available for pattern-based replacement
- **Instance targeting:** Same as Excel

### Key Difference Alert
Functionally identical between platforms. The key difference is that Google Sheets offers REGEXREPLACE as a complement for pattern-based replacements, while Excel requires VBA or Power Query for regex capabilities. For standard SUBSTITUTE usage, formulas are fully compatible between platforms.

## Tips and Best Practices

1. **Nest SUBSTITUTE for multiple replacements:** `=SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A1, "x", "y"), "a", "b"), "1", "2")` processes multiple character substitutions in one formula.

2. **Use for character counting:** `=(LEN(A1)-LEN(SUBSTITUTE(A1,"x","")))/LEN("x")` counts occurrences of any character or string.

3. **Remember case sensitivity:** "Apple" and "apple" are different. For case-insensitive replacement, use `SUBSTITUTE(UPPER(A1), "APPLE", "ORANGE")` or chain multiple SUBSTITUTE calls for each case variation.

4. **Delete by replacing with empty string:** `=SUBSTITUTE(A1, "unwanted", "")` removes text. Combine with TRIM to clean up any extra spaces.

5. **Clean special characters first:** Before analysis, remove formatting characters from phone numbers, SSNs, and other formatted data using SUBSTITUTE chains.

6. **Target specific occurrences wisely:** The instance_num parameter is powerful for surgical changes. Remember occurrences are counted left-to-right.

7. **Combine with TRIM for spacing:** After substitutions, use TRIM to normalize any double spaces or edge spacing: `=TRIM(SUBSTITUTE(...))`.

8. **Handle non-breaking spaces:** Web data often contains CHAR(160). Always include `SUBSTITUTE(A1, CHAR(160), " ")` in web data cleaning formulas.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[REPLACE]] | Replaces characters by position | When you know the position but not the exact text to replace |
| [[REGEXREPLACE]] | Pattern-based replacement (Sheets only) | When you need wildcard or pattern matching for replacements |

### Commonly Used Together

**[[TRIM]]** - Removes extra spaces

*Clean up spacing after substitutions:*
```
=TRIM(SUBSTITUTE(A1, "-", " "))
```
When replacing characters with spaces, TRIM normalizes the result.

---

**[[UPPER]]** / **[[LOWER]]** - Case conversion

*Enable case-insensitive replacement:*
```
=SUBSTITUTE(UPPER(A1), "APPLE", "ORANGE")
```
Normalize case before substitution when case variations exist.

---

**[[LEN]]** - String length

*Count character occurrences:*
```
=(LEN(A1)-LEN(SUBSTITUTE(A1, "x", "")))/LEN("x")
```
Classic technique for counting how many times a character appears.

---

**[[CHAR]]** - Returns character from code

*Replace special characters:*
```
=SUBSTITUTE(A1, CHAR(160), " ")
```
Target non-printable or special characters by their ASCII/Unicode code.

---

**[[CLEAN]]** - Removes non-printable characters

*Combine for thorough data cleaning:*
```
=TRIM(SUBSTITUTE(CLEAN(A1), CHAR(160), " "))
```
CLEAN handles control characters, SUBSTITUTE targets non-breaking spaces.

---

**[[LEFT]]** / **[[RIGHT]]** / **[[MID]]** - Text extraction

*Build replacement text dynamically:*
```
=SUBSTITUTE(A1, LEFT(A1, 3), "NEW")
```
Replace dynamic portions of text based on extraction functions.

---

**[[FIND]]** / **[[SEARCH]]** - Locate text position

*Validate before substitution:*
```
=IF(ISNUMBER(FIND("old", A1)), SUBSTITUTE(A1, "old", "new"), A1)
```
Check if text exists before attempting substitution.

## Official Documentation

- **Microsoft Excel:** [SUBSTITUTE function](https://support.microsoft.com/en-us/office/substitute-function-6434944e-a904-4336-a9b0-1e58df3bc332)
- **Google Sheets:** [SUBSTITUTE function](https://support.google.com/docs/answer/3094215)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function, one of the earliest text functions |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | Identical implementation to Excel |

---

*Last updated: 2026-01-10*
