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
- text-splitting
- delimiter-parsing
- array-generation
- data-parsing
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Text Split
- Split Text
- String Split
- Delimiter Split
tags:
- textsplit
- text
- splitting
- parsing
- excel-365
- dynamic-arrays
---

# TEXTSPLIT

## Description

**TEXTSPLIT** splits a text string into an array of substrings based on column and/or row delimiters. "A,B,C" with comma delimiter becomes a 3-column array. This function replaces complex combinations of LEFT, MID, RIGHT, and FIND with a single, powerful function call.

The function can split horizontally (into columns), vertically (into rows), or both to create a 2D array. Split "A,B;C,D" by comma (columns) and semicolon (rows) to get a 2x2 array. This makes TEXTSPLIT incredibly powerful for parsing structured text data.

**TEXTSPLIT in Google Sheets:** Google Sheets added TEXTSPLIT in 2022 with identical functionality to Excel. The syntax and behavior match exactly, making formulas portable.

**Most powerful of the trio:** TEXTBEFORE and TEXTAFTER extract single pieces; TEXTSPLIT creates complete arrays from delimited text. Use TEXTSPLIT when you need all parts, not just one.

## Syntax

> [!f(x)] TEXTSPLIT Syntax
>
> ```
> =TEXTSPLIT(text, col_delimiter, [row_delimiter], [ignore_empty], [match_mode], [pad_with])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string to split. |
| `col_delimiter` | Yes | Delimiter(s) for splitting into columns. Use array for multiple. |
| `row_delimiter` | No | Delimiter(s) for splitting into rows. Omit for single-row output. |
| `ignore_empty` | No | FALSE = keep empty strings (default), TRUE = ignore empty. |
| `match_mode` | No | 0 = case-sensitive (default), 1 = case-insensitive. |
| `pad_with` | No | Value for uneven rows. Default is #N/A. |

### Return Value

Returns an array of substrings. Single-row if only col_delimiter; multi-row if row_delimiter also specified.

## Examples

> [!f(x)] TEXTSPLIT Examples

### Example 1: Basic Comma Split
```
=TEXTSPLIT("A,B,C", ",")
```
**Result:** {"A","B","C"} (3 columns)

**Explanation:** Split by comma into horizontal array.

---

### Example 2: Row Split Only
```
=TEXTSPLIT("Line1;Line2;Line3", , ";")
```
**Result:** 3-row single column

**Explanation:** Semicolon as row delimiter creates vertical array.

---

### Example 3: Both Delimiters (2D Array)
```
=TEXTSPLIT("A,B;C,D", ",", ";")
```
**Result:** 2x2 array: A|B and C|D

**Explanation:** Comma for columns, semicolon for rows.

---

### Example 4: Ignore Empty Cells
```
=TEXTSPLIT("A,,B,,C", ",", , TRUE)
```
**Result:** {"A","B","C"}

**Explanation:** ignore_empty TRUE removes empty strings.

---

### Example 5: Multiple Column Delimiters
```
=TEXTSPLIT("A,B;C|D", {",",";","|"})
```
**Result:** {"A","B","C","D"}

**Explanation:** Array of delimiters matches any of them.

---

### Example 6: Space Split
```
=TEXTSPLIT("The quick brown fox", " ")
```
**Result:** {"The","quick","brown","fox"}

**Explanation:** Split sentence into words.

---

### Example 7: Custom Padding
```
=TEXTSPLIT("A,B;C", ",", ";", FALSE, 0, "-")
```
**Result:** 2x2 with "-" padding for short row

**Explanation:** pad_with fills uneven rows.

---

### Example 8: Tab-Separated Values
```
=TEXTSPLIT(A1, CHAR(9))
```
**Result:** Array split by tabs

**Explanation:** CHAR(9) represents tab character.

---

### Example 9: New Line Split
```
=TEXTSPLIT(A1, , CHAR(10))
```
**Result:** Each line as separate row

**Explanation:** CHAR(10) is line feed for row splitting.

---

### Example 10: Parse CSV Line
```
=TEXTSPLIT("John,Smith,30,Engineer", ",")
```
**Result:** {"John","Smith","30","Engineer"}

**Explanation:** Parse comma-separated values.

---

### Example 11: Case Insensitive Split
```
=TEXTSPLIT("OneANDtwoANDthree", "and", , , 1)
```
**Result:** {"One","two","three"}

**Explanation:** match_mode 1 ignores case.

---

### Example 12: Complex Multi-Line CSV
```
=TEXTSPLIT(CSVText, ",", CHAR(10), TRUE)
```
**Result:** 2D array of all CSV data

**Explanation:** Full CSV parsing with empty cell handling.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Empty or missing text | Verify input is not empty |
| `#SPILL!` | No room for array | Clear target cells |
| `#N/A in cells` | Uneven rows | Use pad_with parameter |

## Use Cases

### [[CSV Data Parsing]]

**Scenario:** Convert CSV text into usable array.

**Implementation:**
```
=TEXTSPLIT(CSVData, ",", CHAR(10), TRUE)
```

**Business Application:** Import external data, parse exports, data transformation.

---

### [[Address Parsing]]

**Scenario:** Split address into components.

**Implementation:**
```
=TEXTSPLIT(Address, ",")
```

**Business Application:** Address standardization, component extraction.

---

### [[Tag/Keyword Extraction]]

**Scenario:** Split comma-separated tags into array.

**Implementation:**
```
=TEXTSPLIT(Tags, ",", , TRUE)
```

**Business Application:** Tag analysis, keyword processing, category assignment.

---

### [[Form Data Restructuring]]

**Scenario:** Parse form submission with field separators.

**Implementation:**
```
=TEXTSPLIT(FormData, "=", "&")
```

**Business Application:** Query string parsing, form data extraction.

## Platform Differences

Both Excel and Google Sheets support TEXTSPLIT identically since 2022.

## Tips and Best Practices

1. **Use CHAR for special characters:** CHAR(10) for newline, CHAR(9) for tab.

2. **Combine delimiters with arrays:** {",",";"} matches either comma or semicolon.

3. **Set pad_with for clean output:** Avoids #N/A in uneven data.

4. **ignore_empty for clean data:** TRUE removes empty cells from consecutive delimiters.

5. **Row delimiter for vertical output:** Omit col_delimiter and use row_delimiter only for column output.

6. **Combine with TRANSPOSE:** TEXTSPLIT then TRANSPOSE for orientation changes.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TEXTBEFORE]] | Text before delimiter | When you need just one part |
| [[TEXTAFTER]] | Text after delimiter | When you need just the tail |
| [[FILTERXML]] | XML parsing (Excel only) | For complex parsing patterns |

### Commonly Used Together

**[[TOCOL]]** - Reshape split result
```
=TOCOL(TEXTSPLIT(Text, ","))
```

**[[UNIQUE]]** - Unique values from split
```
=UNIQUE(TEXTSPLIT(Tags, ",", , TRUE))
```

**[[TRIM]]** - Clean whitespace after split
```
=TRIM(TEXTSPLIT(Text, ","))
```

## Official Documentation

- **Microsoft Excel:** [TEXTSPLIT function](https://support.microsoft.com/en-us/office/textsplit-function-b1ca414e-4c21-4ca0-b1b7-bdecace8a36e)
- **Google Sheets:** [TEXTSPLIT function](https://support.google.com/docs/answer/12192824)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of text parsing functions |
| Google Sheets | 2022 | Added to match Excel |

---

*Last updated: 2026-01-10*
