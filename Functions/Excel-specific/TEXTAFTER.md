---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text-functions
- string-manipulation
subTopics:
- text-extraction
- delimiter-parsing
- substring
- text-parsing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Text After
- Get Text After
- Right of Delimiter
- Extract After
tags:
- textafter
- text
- extraction
- parsing
- excel-365
---

# TEXTAFTER

## Description

**TEXTAFTER** extracts text that appears after a specified delimiter in a string. Need the last name from "John Smith"? `=TEXTAFTER(A1, " ")` returns "Smith". This function replaces complex RIGHT/LEN/FIND combinations with a single, readable function call.

The function provides advanced options for handling multiple delimiter occurrences, case sensitivity, and behavior when the delimiter isn't found. Find the text after the second comma, ignore case when matching, or return the original text when no match exists.

**TEXTAFTER in Google Sheets:** Google Sheets supports TEXTAFTER with identical syntax and behavior since 2022. Formulas are fully portable between platforms.

**Part of a trio:** TEXTBEFORE, TEXTAFTER, and TEXTSPLIT work together for comprehensive text parsing. TEXTAFTER gets text after a delimiter, TEXTBEFORE gets text before, and TEXTSPLIT splits into arrays.

## Syntax

> [!f(x)] TEXTAFTER Syntax
>
> ```
> =TEXTAFTER(text, delimiter, [instance_num], [match_mode], [match_end], [if_not_found])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string to search in. |
| `delimiter` | Yes | The character(s) to search for. Can be array for multiple delimiters. |
| `instance_num` | No | Which occurrence to find. Default is 1. Negative counts from end. |
| `match_mode` | No | 0 = case-sensitive (default), 1 = case-insensitive. |
| `match_end` | No | 0 = match against delimiter (default), 1 = match against end of text. |
| `if_not_found` | No | Value to return if delimiter not found. Default is #N/A error. |

### Return Value

Returns text after the specified delimiter occurrence. Returns #N/A or if_not_found value when delimiter doesn't exist.

## Examples

> [!f(x)] TEXTAFTER Examples

### Example 1: Basic Extraction
```
=TEXTAFTER("John Smith", " ")
```
**Result:** "Smith"

**Explanation:** Returns text after first space.

---

### Example 2: Email Domain
```
=TEXTAFTER("user@domain.com", "@")
```
**Result:** "domain.com"

**Explanation:** Extract domain from email address.

---

### Example 3: After Second Delimiter
```
=TEXTAFTER("A-B-C-D", "-", 2)
```
**Result:** "C-D"

**Explanation:** Get text after the second hyphen.

---

### Example 4: From End (Negative)
```
=TEXTAFTER("A-B-C-D", "-", -1)
```
**Result:** "D"

**Explanation:** Text after the last hyphen.

---

### Example 5: Case Insensitive
```
=TEXTAFTER("Hello World", "o", 1, 1)
```
**Result:** " World"

**Explanation:** Match_mode 1 ignores case, matches first "o".

---

### Example 6: Custom Not Found
```
=TEXTAFTER("No delimiter here", "@", 1, 0, 0, "N/A")
```
**Result:** "N/A"

**Explanation:** Return custom value when delimiter missing.

---

### Example 7: File Extension
```
=TEXTAFTER("document.pdf", ".")
```
**Result:** "pdf"

**Explanation:** Extract file extension.

---

### Example 8: Multiple Delimiters
```
=TEXTAFTER("item:123;456", {":",";"})
```
**Result:** "123;456"

**Explanation:** Find first occurrence of any delimiter in array.

---

### Example 9: URL Path
```
=TEXTAFTER("https://example.com/path/page", "/", 3)
```
**Result:** "path/page"

**Explanation:** Extract path after third slash.

---

### Example 10: Last Word
```
=TEXTAFTER("One Two Three Four", " ", -1)
```
**Result:** "Four"

**Explanation:** Get text after the last space (last word).

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Delimiter not found | Use if_not_found parameter |
| `#VALUE!` | Empty text or delimiter | Verify inputs are not empty |
| `#VALUE!` | instance_num is 0 | Must be positive or negative, not zero |

## Use Cases

### [[Last Name Extraction]]

**Scenario:** Extract last name from full name field.

**Implementation:**
```
=TEXTAFTER(FullName, " ", -1, 0, 0, FullName)
```

**Business Application:** Name parsing for sorting, formal addressing.

---

### [[File Extension Detection]]

**Scenario:** Get file extension for type validation.

**Implementation:**
```
=TEXTAFTER(FileName, ".", -1)
```

**Business Application:** File type filtering, upload validation.

---

### [[Domain Extraction]]

**Scenario:** Get domain from email for organizational grouping.

**Implementation:**
```
=TEXTAFTER(Email, "@", 1, 0, 0, "Unknown")
```

**Business Application:** Email categorization, company identification.

## Platform Differences

Both Excel and Google Sheets support TEXTAFTER identically since 2022.

## Tips and Best Practices

1. **Use if_not_found:** Always specify what to return when delimiter is missing.

2. **Negative instance_num:** Use -1 for "after last occurrence" patterns.

3. **Chain with TEXTBEFORE:** Extract between delimiters by nesting functions.

4. **Multiple delimiters:** Array of delimiters matches first occurrence of any.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TEXTBEFORE]] | Text before delimiter | When you need content before the delimiter |
| [[TEXTSPLIT]] | Split into array | When you need all parts, not just before/after |
| [[RIGHT]] | Right N characters | When extracting by character count |

## Official Documentation

- **Microsoft Excel:** [TEXTAFTER function](https://support.microsoft.com/en-us/office/textafter-function-c8db2546-5b51-416a-9690-c7e6722e90b4)
- **Google Sheets:** [TEXTAFTER function](https://support.google.com/docs/answer/12191881)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of text parsing functions |
| Google Sheets | 2022 | Added to match Excel |

---

*Last updated: 2026-01-10*
