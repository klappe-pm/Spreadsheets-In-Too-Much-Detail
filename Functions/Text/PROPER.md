---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- case-conversion
- text-formatting
- name-formatting
- title-case
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Title Case
- Proper Case
- Capitalize Words
- Initial Caps
tags:
- text
- case-conversion
- title-case
- proper-case
- formatting
- names
---

# PROPER

## Description

**PROPER** converts text to title case, capitalizing the first letter of each word while converting all other letters to lowercase. This function is invaluable for formatting names, titles, addresses, and any text where professional presentation matters. When you have data entered in ALL CAPS, all lowercase, or inconsistent mixtures, PROPER transforms it into readable, standardized title case.

The function identifies word boundaries by spaces, punctuation, and numbers. Each time it encounters a character that follows one of these delimiters, it capitalizes that character. This means "JOHN SMITH" becomes "John Smith", "mcdonald" becomes "Mcdonald" (not "McDonald"), and "123main" becomes "123Main". The algorithm is simple and consistent, which is both its strength and its limitation.

**Critical gotcha:** PROPER does not understand proper nouns, surnames with internal capitals (McDonald, O'Brien, DeVito), or context-appropriate exceptions. It capitalizes after any non-letter character, so "o'brien" becomes "O'Brien" (correct), but "mcdonald" becomes "Mcdonald" (incorrect). Acronyms like "USA" become "Usa". Names like "van der Berg" become "Van Der Berg". You will often need post-processing with SUBSTITUTE or manual review for these edge cases.

**Platform consideration:** Excel and Google Sheets implement PROPER identically. Both capitalize after any non-letter character and convert all other letters to lowercase. Neither platform includes smart handling for common exceptions like "Jr.", "III", or "LLC". For professional applications, consider PROPER as a first-pass tool that may require refinement through additional formulas or manual review.

## Syntax

> [!f(x)] PROPER Syntax
>
> ```
> =PROPER(text)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string you want to convert to title case. Can be a literal string in quotes, a cell reference, or a formula that returns text. Numbers are converted to their text representation. |

### Return Value

Returns the text string with the first letter of each word capitalized and all other letters converted to lowercase. Numbers and symbols remain unchanged but trigger capitalization of following letters. Empty strings return empty strings.

## Examples

> [!f(x)] PROPER Examples

### Example 1: Basic Name Formatting
```
=PROPER("john smith")
```
**Result:** "John Smith"

**Explanation:** Each word's first letter is capitalized; remaining letters become lowercase. Perfect for names entered in all lowercase.

---

### Example 2: All Caps to Title Case
```
=PROPER("JOHN SMITH")
```
**Result:** "John Smith"

**Explanation:** All letters after the first in each word are converted to lowercase. Great for data entry that was accidentally in caps lock.

---

### Example 3: Mixed Case Input
```
=PROPER("jOHN sMITH")
```
**Result:** "John Smith"

**Explanation:** Regardless of original case mixture, PROPER normalizes to title case. Inconsistent data entry is standardized.

---

### Example 4: Cell Reference
```
=PROPER(A1)
```
Where A1 contains "MARY JANE WATSON"

**Result:** "Mary Jane Watson"

**Explanation:** Cell references work identically to literal strings. This is the standard usage for data cleaning.

---

### Example 5: Hyphenated Names
```
=PROPER("mary-jane watson")
```
**Result:** "Mary-Jane Watson"

**Explanation:** Hyphens trigger capitalization, so both parts of hyphenated names are capitalized correctly.

---

### Example 6: Apostrophe in Names
```
=PROPER("o'brien")
```
**Result:** "O'Brien"

**Explanation:** Apostrophes trigger capitalization, correctly handling Irish names like O'Brien, O'Connor, O'Malley.

---

### Example 7: McDonald Problem
```
=PROPER("mcdonald")
```
**Result:** "Mcdonald"

**Explanation:** PROPER cannot detect that the "d" should be capitalized. This is a known limitation requiring SUBSTITUTE correction.

---

### Example 8: Address Formatting
```
=PROPER("123 MAIN STREET, ANYTOWN, NY 12345")
```
**Result:** "123 Main Street, Anytown, Ny 12345"

**Explanation:** Numbers pass through, letters are title-cased. Note that "NY" becomes "Ny" - abbreviations may need separate handling.

---

### Example 9: Titles with Articles
```
=PROPER("THE LORD OF THE RINGS")
```
**Result:** "The Lord Of The Rings"

**Explanation:** PROPER capitalizes all words including articles (the, of, a). Formal title case rules would lowercase articles, but PROPER does not make this distinction.

---

### Example 10: Numbers and Letters Mixed
```
=PROPER("product123name")
```
**Result:** "Product123Name"

**Explanation:** Numbers trigger capitalization of following letters. "123" causes "n" to become "N".

---

### Example 11: Empty String
```
=PROPER("")
```
**Result:** "" (empty string)

**Explanation:** Empty strings return empty strings. Safe for optional fields.

---

### Example 12: Combining with TRIM
```
=PROPER(TRIM(A1))
```
Where A1 contains "  JOHN   SMITH  "

**Result:** "John Smith"

**Explanation:** TRIM normalizes spaces first, then PROPER applies title case. Recommended combination for data cleaning.

---

### Example 13: Fixing McDonald with SUBSTITUTE
```
=SUBSTITUTE(PROPER(A1), "Mcdonald", "McDonald")
```
Where A1 contains "mcdonald"

**Result:** "McDonald"

**Explanation:** Use SUBSTITUTE to correct known exceptions after PROPER does the initial formatting.

---

### Example 14: Multiple Exception Handling
```
=SUBSTITUTE(SUBSTITUTE(PROPER(A1), "Mcdonald", "McDonald"), "Macdonald", "MacDonald")
```
**Result:** Correct handling of both McDonald and MacDonald variations

**Explanation:** Chain SUBSTITUTE functions for multiple known exceptions. For many exceptions, consider a lookup table approach.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Rare; typically from nested formula errors | Check formulas feeding into PROPER for errors. |
| `"Mcdonald" not "McDonald"` | PROPER doesn't recognize internal capitals | Use SUBSTITUTE to correct specific surnames: SUBSTITUTE(PROPER(A1), "Mcdonald", "McDonald"). |
| `"Usa" not "USA"` | Acronyms are treated as words | Keep a list of acronyms and use SUBSTITUTE to restore uppercase. |
| `"Van Der Berg" not "van der Berg"` | PROPER capitalizes all words | Dutch/German name particles need manual correction or SUBSTITUTE. |
| `"Jr." becomes "Jr."` | Numbers after periods capitalize | "Jr." is correct, but "2nd" becomes "2Nd" - check for unwanted capitals after numbers. |

## Use Cases

### [[Customer Name Standardization]]

**Scenario:** Customer names imported from legacy systems or web forms have inconsistent capitalization. Need uniform presentation for CRM, mailings, and reports.

**Implementation:**
```
=PROPER(TRIM(A2))
```
With common exceptions:
```
=SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(PROPER(TRIM(A2)), "Mcdonald", "McDonald"), "Macarthur", "MacArthur"), "O'brien", "O'Brien")
```

**Business Application:** Professional customer communications, mail merge documents, and CRM data quality. Prevents embarrassing name misspellings on invoices and correspondence.

**Technical Details:** Apply PROPER as first pass, then use SUBSTITUTE chain for known exceptions. Maintain a lookup table of corrections for large datasets. Consider flagging unusual names for manual review.

---

### [[Address Data Cleanup]]

**Scenario:** Address data from various sources has inconsistent formatting. Street names, cities, and company names need standardization for mailings and geocoding.

**Implementation:**
```
Street: =PROPER(TRIM(A2))
City: =PROPER(TRIM(B2))
State: =UPPER(TRIM(C2))  // States should stay uppercase
```

**Business Application:** Address verification, shipping labels, and location-based analytics. Consistent address formatting improves deliverability and reduces returned mail.

**Technical Details:** Apply PROPER to street and city, but use UPPER for state abbreviations. Be aware that street suffixes like "NE", "SW" become "Ne", "Sw" and may need correction.

---

### [[Book and Article Title Formatting]]

**Scenario:** A publishing database contains titles entered with inconsistent capitalization. Need to display titles in standard title case for catalogs and websites.

**Implementation:**
```
=PROPER(LOWER(A2))
```
Note: This creates simple title case where all words are capitalized, not formal title case with lowercase articles.

**Business Application:** Library catalogs, bookstore inventory, and content management systems. Consistent title presentation improves professional appearance and searchability.

**Technical Details:** PROPER capitalizes ALL words, including articles and prepositions that formal title case would lowercase. For true title case, use PROPER then SUBSTITUTE to lowercase specific words: "Of", "The", "A", "An", "And", etc.

---

### [[Employee Directory Creation]]

**Scenario:** HR data exported from payroll has names in all caps. Need to create a professional employee directory with properly formatted names.

**Implementation:**
```
Full Name: =PROPER(A2) & " " & PROPER(B2)
With Title: =PROPER(A2) & " " & PROPER(B2) & ", " & C2
```

**Business Application:** Internal directories, org charts, and employee communications. Professional name formatting reflects organizational quality standards.

**Technical Details:** Combine with CONCATENATE for full name assembly. Consider special handling for suffixes (Jr., III, PhD) that should retain their formatting.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 onward)
- **Behavior:** Capitalizes first letter after any non-letter character
- **Word boundaries:** Spaces, punctuation, numbers all trigger capitalization
- **Return type:** Always returns text, even for numeric input

### Google Sheets
- **Availability:** All versions since launch
- **Behavior:** Identical to Excel in all tested scenarios
- **Array support:** Works with ARRAYFORMULA for range processing
- **Unicode handling:** Handles accented characters appropriately

### Key Difference Alert
Both platforms implement PROPER identically. The main challenge is not platform differences but the function's inherent limitations:
1. Does not recognize surnames with internal capitals (McDonald, DeVito)
2. Does not recognize acronyms that should stay uppercase (USA, PhD)
3. Does not implement formal title case rules for articles/prepositions
4. Both platforms require SUBSTITUTE workarounds for these cases

## Tips and Best Practices

1. **Always combine with TRIM:** `=PROPER(TRIM(A1))` handles both spacing and capitalization issues. Extra spaces between names can cause formatting problems.

2. **Create an exceptions list:** Maintain a reference list of names/words that need special handling (McDonald, O'Brien, III, PhD). Use VLOOKUP or nested SUBSTITUTE to correct these.

3. **PROPER before SUBSTITUTE:** Apply PROPER first to standardize everything, then use SUBSTITUTE to fix known exceptions. This ensures consistent handling.

4. **Handle state abbreviations separately:** Use UPPER for state codes (CA, NY, TX) since PROPER would convert them to "Ca", "Ny", "Tx".

5. **Flag unusual results:** Use conditional formatting to highlight results containing unusual patterns (consecutive capitals, very short words) for manual review.

6. **Consider context:** PROPER is ideal for names and addresses but not for acronyms, technical terms, or formal titles that have specific capitalization rules.

7. **Test with your data:** Before applying PROPER to a large dataset, test with sample data that includes edge cases (hyphenated names, apostrophes, numbers).

8. **Document exceptions:** Keep a record of SUBSTITUTE corrections applied, making it easier to maintain and extend the exception handling over time.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[UPPER]] | Converts text to all uppercase | For codes, identifiers, or data requiring all caps |
| [[LOWER]] | Converts text to all lowercase | For emails, URLs, or comparison keys |

### Commonly Used Together

**[[TRIM]]** - Removes extra spaces

*Combine with PROPER for complete name cleaning:*
```
=PROPER(TRIM(A1))
```
Essential combination: TRIM removes irregular spaces that could affect word boundaries, then PROPER applies title case.

---

**[[SUBSTITUTE]]** - Replaces specific text

*Correct known exceptions after PROPER:*
```
=SUBSTITUTE(PROPER(A1), "Mcdonald", "McDonald")
```
Handle surnames, acronyms, and other words that need non-standard capitalization.

---

**[[UPPER]]** - Converts to all uppercase

*Use for portions that should remain uppercase:*
```
=PROPER(A1) & " " & UPPER(B1)
```
Combine PROPER for names with UPPER for state codes or acronyms.

---

**[[LOWER]]** - Converts to all lowercase

*Use LOWER before PROPER for consistency:*
```
=PROPER(LOWER(A1))
```
Ensures starting point is all lowercase before title case is applied.

---

**[[CONCATENATE]]** / **[[&]]** - Joins text strings

*Build formatted names from components:*
```
=PROPER(TRIM(A1)) & " " & PROPER(TRIM(B1))
```
Assemble full names from separate first/last name fields.

---

**[[LEFT]]** / **[[RIGHT]]** / **[[MID]]** - Text extraction functions

*Extract and format portions of text:*
```
=PROPER(LEFT(A1, FIND(" ", A1)-1))
```
Extract and title-case the first name from a full name string.

---

**[[IF]]** - Conditional logic

*Conditional formatting based on context:*
```
=IF(LEN(A1)<=2, UPPER(A1), PROPER(A1))
```
Keep short strings (likely abbreviations) uppercase; apply PROPER to longer text.

## Official Documentation

- **Microsoft Excel:** [PROPER function](https://support.microsoft.com/en-us/office/proper-function-52a5a283-e8b2-49be-8506-b2887b889f94)
- **Google Sheets:** [PROPER function](https://support.google.com/docs/answer/3094133)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function, one of the earliest text functions |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | Identical implementation to Excel |

---

*Last updated: 2026-01-10*
