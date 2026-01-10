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
- Text Before
- Get Text Before
- Left of Delimiter
- Extract Before
tags:
- textbefore
- text
- extraction
- parsing
- excel-365
---

# TEXTBEFORE

## Description

**TEXTBEFORE** extracts text that appears before a specified delimiter in a string. Need the first name from "John Smith"? `=TEXTBEFORE(A1, " ")` returns "John". This function replaces complex LEFT/FIND combinations with a single, readable function call.

The function provides advanced options for handling multiple delimiter occurrences, case sensitivity, and behavior when the delimiter isn't found. Find the text before the second comma, ignore case when matching, or return the original text when no match exists.

**TEXTBEFORE in Google Sheets:** Google Sheets supports TEXTBEFORE with identical syntax and behavior since 2022. Formulas are fully portable between platforms.

**Part of a trio:** TEXTBEFORE, TEXTAFTER, and TEXTSPLIT work together for comprehensive text parsing. TEXTBEFORE gets text before a delimiter, TEXTAFTER gets text after, and TEXTSPLIT splits into arrays.

## Syntax

> [!f(x)] TEXTBEFORE Syntax
>
> ```
> =TEXTBEFORE(text, delimiter, [instance_num], [match_mode], [match_end], [if_not_found])
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

Returns text before the specified delimiter occurrence. Returns #N/A or if_not_found value when delimiter doesn't exist.

## Examples

> [!f(x)] TEXTBEFORE Examples

### Example 1: Basic Extraction
```
=TEXTBEFORE("John Smith", " ")
```
**Result:** "John"

**Explanation:** Returns text before first space.

---

### Example 2: Email Username
```
=TEXTBEFORE("user@domain.com", "@")
```
**Result:** "user"

**Explanation:** Extract username from email address.

---

### Example 3: Before Second Delimiter
```
=TEXTBEFORE("A-B-C-D", "-", 2)
```
**Result:** "A-B"

**Explanation:** Get text before the second hyphen.

---

### Example 4: From End (Negative)
```
=TEXTBEFORE("A-B-C-D", "-", -1)
```
**Result:** "A-B-C"

**Explanation:** Text before the last hyphen.

---

### Example 5: Case Insensitive
```
=TEXTBEFORE("Hello World", "W", 1, 1)
```
**Result:** "Hello "

**Explanation:** Match_mode 1 ignores case.

---

### Example 6: Custom Not Found
```
=TEXTBEFORE("No delimiter here", "@", 1, 0, 0, "N/A")
```
**Result:** "N/A"

**Explanation:** Return custom value when delimiter missing.

---

### Example 7: Return Original if Missing
```
=TEXTBEFORE("No delimiter", "@", 1, 0, 0, A1)
```
**Result:** Original text in A1

**Explanation:** Return original when no match found.

---

### Example 8: Multiple Delimiters
```
=TEXTBEFORE("item:123;456", {":",";"})
```
**Result:** "item"

**Explanation:** Find first occurrence of any delimiter in array.

---

### Example 9: File Name Without Extension
```
=TEXTBEFORE("document.pdf", ".")
```
**Result:** "document"

**Explanation:** Extract filename without extension.

---

### Example 10: Domain from URL
```
=TEXTBEFORE(TEXTAFTER("https://www.example.com/page", "://"), "/")
```
**Result:** "www.example.com"

**Explanation:** Chain TEXTAFTER and TEXTBEFORE for complex parsing.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Delimiter not found | Use if_not_found parameter |
| `#VALUE!` | Empty text or delimiter | Verify inputs are not empty |
| `#VALUE!` | instance_num is 0 | Must be positive or negative, not zero |

## Use Cases

### [[Name Parsing]]

**Scenario:** Extract first name from full name field.

**Implementation:**
```
=TEXTBEFORE(FullName, " ", 1, 0, 0, FullName)
```

**Business Application:** Split name fields for personalization, sorting, or matching.

---

### [[Email Domain Extraction]]

**Scenario:** Get the domain from email addresses.

**Implementation:**
```
=TEXTAFTER(Email, "@")
```

**Business Application:** Email categorization, domain analysis, filtering.

---

### [[File Path Parsing]]

**Scenario:** Extract directory path from full file path.

**Implementation:**
```
=TEXTBEFORE(FilePath, "\", -1)
```

**Business Application:** File organization, path analysis, folder extraction.

## Platform Differences

Both Excel and Google Sheets support TEXTBEFORE identically since 2022.

## Tips and Best Practices

1. **Use if_not_found:** Always specify what to return when delimiter is missing.

2. **Negative instance_num:** Count from end with -1 for last occurrence.

3. **Chain with TEXTAFTER:** Parse between delimiters with nested functions.

4. **Multiple delimiters:** Use array like {",",";"} to match any of several delimiters.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TEXTAFTER]] | Text after delimiter | When you need content after the delimiter |
| [[TEXTSPLIT]] | Split into array | When you need all parts, not just before/after |
| [[LEFT]] | Left N characters | When extracting by character count, not delimiter |

## Official Documentation

- **Microsoft Excel:** [TEXTBEFORE function](https://support.microsoft.com/en-us/office/textbefore-function-d099c28a-dba8-448e-ac6c-f086d0fa1b29)
- **Google Sheets:** [TEXTBEFORE function](https://support.google.com/docs/answer/12191852)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of text parsing functions |
| Google Sheets | 2022 | Added to match Excel |

---

*Last updated: 2026-01-10*
