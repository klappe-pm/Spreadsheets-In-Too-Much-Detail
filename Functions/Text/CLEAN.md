---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- text
subTopics:
- data-cleaning
- text-sanitization
- character-removal
- data-import
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Clean Text
- Remove Non-Printable
- Sanitize Text
- Strip Control Characters
tags:
- text
- cleaning
- non-printable
- control-characters
- sanitization
- import
---

# CLEAN

## Description

**CLEAN** removes all non-printable characters from text, including control characters that are invisible but can cause problems in data processing. When data is imported from external systems, copied from the web, or transferred between applications, it often picks up hidden characters that break formulas, prevent matching, and cause display issues. CLEAN strips these problematic characters (ASCII codes 0-31) and returns clean, usable text.

The function operates silently and completely: it scans every character in your text and removes any that fall within the non-printable range. This includes null characters, tabs, line feeds, carriage returns, and other control characters that are typically invisible but affect how text behaves. The remaining visible characters are left intact and concatenated into the result.

**Critical gotcha:** CLEAN only removes characters with ASCII codes 0-31. It does NOT remove the non-breaking space (ASCII 160, often from web pages), which is one of the most common problematic characters in imported data. For complete cleaning, combine CLEAN with `SUBSTITUTE(text, CHAR(160), " ")` to handle non-breaking spaces. Also note that CLEAN removes line breaks (CHAR(10) and CHAR(13)), which may not always be desired.

**Platform consideration:** Excel and Google Sheets implement CLEAN identically. Both target the same ASCII range (0-31), both preserve all other characters, and both leave non-breaking spaces untouched. For comprehensive data cleaning across both platforms, use the same combined formula: `TRIM(CLEAN(SUBSTITUTE(text, CHAR(160), " ")))`.

## Syntax

> [!f(x)] CLEAN Syntax
>
> ```
> =CLEAN(text)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string from which you want to remove non-printable characters. Can be a cell reference, literal text, or formula result. |

### Return Value

Returns the text string with all non-printable characters (ASCII 0-31) removed. Other characters, including spaces and non-breaking spaces, remain. Empty strings return empty strings.

## Examples

> [!f(x)] CLEAN Examples

### Example 1: Basic Control Character Removal
```
=CLEAN(A1)
```
Where A1 contains text with hidden control characters

**Result:** Text with control characters removed

**Explanation:** Any non-printable characters (ASCII 0-31) are silently removed from the text.

---

### Example 2: Removing Line Breaks
```
=CLEAN("Line1" & CHAR(10) & "Line2")
```
**Result:** "Line1Line2"

**Explanation:** CHAR(10) is a line feed. CLEAN removes it, joining the lines into a single line.

---

### Example 3: Removing Carriage Return
```
=CLEAN("Text" & CHAR(13) & "More Text")
```
**Result:** "TextMore Text"

**Explanation:** CHAR(13) is a carriage return. CLEAN removes it along with all other control characters.

---

### Example 4: Removing Tab Characters
```
=CLEAN("Column1" & CHAR(9) & "Column2")
```
**Result:** "Column1Column2"

**Explanation:** CHAR(9) is a tab. CLEAN removes it, but note that no space is left behind. Use SUBSTITUTE if you want to replace tabs with spaces.

---

### Example 5: Text Already Clean
```
=CLEAN("Hello World")
```
**Result:** "Hello World"

**Explanation:** Clean text passes through unchanged. CLEAN is safe to apply even when unnecessary.

---

### Example 6: Combination with TRIM
```
=TRIM(CLEAN(A1))
```
**Result:** Text with control characters and excess spaces removed

**Explanation:** The most common cleaning combination: CLEAN removes non-printable characters, TRIM normalizes spacing.

---

### Example 7: Comprehensive Cleaning Formula
```
=TRIM(CLEAN(SUBSTITUTE(A1, CHAR(160), " ")))
```
**Result:** Text with non-breaking spaces, control characters, and excess spaces all cleaned

**Explanation:** The gold standard cleaning formula: converts non-breaking spaces to regular spaces, removes control characters, then normalizes spacing.

---

### Example 8: Preserving Line Breaks (Alternative Approach)
```
=SUBSTITUTE(SUBSTITUTE(CLEAN(SUBSTITUTE(SUBSTITUTE(A1, CHAR(10), "||LF||"), CHAR(13), "||CR||")), "||LF||", CHAR(10)), "||CR||", CHAR(13))
```
**Result:** Text with control characters removed except line breaks

**Explanation:** Complex technique to preserve line breaks: replace them with placeholders, clean, then restore. Use when multi-line text must be preserved.

---

### Example 9: Detecting Non-Printable Characters
```
=LEN(A1) - LEN(CLEAN(A1))
```
**Result:** Count of non-printable characters in the text

**Explanation:** Difference between original and cleaned length reveals how many control characters were present. Non-zero indicates the data had hidden characters.

---

### Example 10: Conditional Cleaning Flag
```
=IF(LEN(A1) <> LEN(CLEAN(A1)), "Contains Control Characters", "Clean")
```
**Result:** Flags cells with non-printable characters

**Explanation:** Audit formula to identify which cells have hidden characters before bulk cleaning.

---

### Example 11: Clean for VLOOKUP
```
=VLOOKUP(TRIM(CLEAN(A1)), Table, 2, FALSE)
```
**Result:** Lookup with cleaned search value

**Explanation:** Cleans the lookup value to prevent matching failures caused by hidden characters in the search term.

---

### Example 12: Cleaning Imported Data
```
=TRIM(CLEAN(SUBSTITUTE(SUBSTITUTE(A1, CHAR(160), " "), CHAR(127), "")))
```
**Result:** Thoroughly cleaned imported text

**Explanation:** Extended cleaning that also removes DEL character (127) which CLEAN doesn't catch. Common for problematic data imports.

---

### Example 13: Clean Empty Check
```
=IF(CLEAN(A1)="", "Empty or Only Control Chars", CLEAN(A1))
```
**Result:** Identifies cells that become empty after cleaning

**Explanation:** Some cells may contain only control characters; cleaning makes them effectively empty.

---

### Example 14: Batch Cleaning Diagnostic
```
=SUMPRODUCT((LEN(A1:A100)-LEN(CLEAN(A1:A100))))
```
**Result:** Total control characters across a range

**Explanation:** Counts all non-printable characters across a dataset. Useful for data quality assessment before import.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Rare; usually from nested formula errors | Check formulas feeding into CLEAN for errors. |
| `Non-breaking spaces still present` | CLEAN doesn't remove CHAR(160) | Add SUBSTITUTE(text, CHAR(160), " ") before CLEAN. |
| `Text still has line breaks` | Line breaks ARE removed by CLEAN | If you want them, use SUBSTITUTE to preserve specific characters. |
| `Matching still fails` | Other invisible characters above ASCII 31 | Use CODE function to identify remaining problem characters. |
| `Unexpected text joining` | Tabs and breaks removed without replacement | Use SUBSTITUTE to replace with spaces before CLEAN if separation needed. |

## Use Cases

### [[Data Import Sanitization]]

**Scenario:** Data imported from external systems, databases, or file transfers contains hidden control characters that cause formula errors, matching failures, and display issues.

**Implementation:**
```
Standard Clean: =TRIM(CLEAN(A2))
Full Clean: =TRIM(CLEAN(SUBSTITUTE(A2, CHAR(160), " ")))
Ultra Clean: =TRIM(CLEAN(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A2, CHAR(160), " "), CHAR(127), ""), CHAR(0), "")))
```

**Business Application:** ETL pipelines, data migration, and system integration. Clean data is essential for reliable processing and prevents mysterious failures.

**Technical Details:** Apply cleaning formulas to a helper column during import. After validation, paste values to replace original. For large imports, consider cleaning in stages to identify which characters cause issues.

---

### [[Web Data Cleaning]]

**Scenario:** Data copied from web pages or received via web forms often contains non-breaking spaces (CHAR 160) and other hidden formatting characters that look like spaces but behave differently.

**Implementation:**
```
Web Clean: =TRIM(CLEAN(SUBSTITUTE(A2, CHAR(160), " ")))
With Additional Web Characters: =TRIM(CLEAN(SUBSTITUTE(SUBSTITUTE(A2, CHAR(160), " "), CHAR(173), "")))
```

**Business Application:** Web scraping data processing, form submission handling, and content management. Web content requires specific cleaning for proper handling.

**Technical Details:** CHAR(160) is the non-breaking space, extremely common in web content. CHAR(173) is the soft hyphen, used for word-break hints. Both pass through CLEAN unchanged and need explicit SUBSTITUTE treatment.

---

### [[Multi-Line Text Flattening]]

**Scenario:** Data entered in multi-line format needs to be flattened to single lines for database import, reporting, or processing systems that don't handle line breaks.

**Implementation:**
```
Flatten: =CLEAN(A2)
Flatten with Space: =TRIM(SUBSTITUTE(SUBSTITUTE(A2, CHAR(10), " "), CHAR(13), " "))
Keep Limited Structure: =SUBSTITUTE(CLEAN(A2), "  ", " ")
```

**Business Application:** Database normalization, report generation, and data export to line-based systems. Multi-line content must be flattened for many downstream uses.

**Technical Details:** CLEAN removes CHAR(10) and CHAR(13) entirely. If you want spaces where line breaks were, use SUBSTITUTE to convert them before using CLEAN for other characters.

---

### [[Data Quality Auditing]]

**Scenario:** Before processing a large dataset, need to identify which fields contain non-printable characters and quantify the data quality issues.

**Implementation:**
```
Has Hidden Chars: =LEN(A2)<>LEN(CLEAN(A2))
Hidden Char Count: =LEN(A2)-LEN(CLEAN(A2))
Quality Score: =1-(LEN(A2)-LEN(CLEAN(A2)))/LEN(A2)
```

**Business Application:** Data quality assessment, pre-migration auditing, and compliance verification. Understanding data quality before processing prevents downstream issues.

**Technical Details:** Comparing LEN before and after CLEAN reveals hidden characters. Aggregate across columns and rows for overall quality metrics. Flag specific cells for manual review when automatic cleaning isn't appropriate.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 onward)
- **Target range:** Removes ASCII characters 0-31 only
- **Does NOT remove:** Non-breaking space (160), DEL (127), other Unicode control characters
- **Return type:** Text string

### Google Sheets
- **Availability:** All versions since launch
- **Behavior:** Identical to Excel
- **Target range:** Same ASCII 0-31 range
- **Array support:** Works with ARRAYFORMULA for range processing

### Key Difference Alert
Both platforms implement CLEAN identically. The critical limitation is consistent across both: CLEAN only removes ASCII 0-31. Non-breaking spaces (CHAR 160) and other problematic characters above 31 require SUBSTITUTE. The "gold standard" cleaning formula works identically on both platforms: `=TRIM(CLEAN(SUBSTITUTE(A1, CHAR(160), " ")))`.

## Tips and Best Practices

1. **Always combine with TRIM:** `=TRIM(CLEAN(A1))` handles both non-printable characters and spacing issues. This is the minimum cleaning formula for most use cases.

2. **Include non-breaking space handling:** `=TRIM(CLEAN(SUBSTITUTE(A1, CHAR(160), " ")))` is the gold standard for cleaning imported data. Non-breaking spaces are extremely common and CLEAN doesn't remove them.

3. **Audit before bulk cleaning:** Use `=LEN(A1)-LEN(CLEAN(A1))` to identify which cells have hidden characters. This helps understand your data quality before applying fixes.

4. **CLEAN is safe to apply universally:** Clean text passes through unchanged. Apply CLEAN broadly without worrying about affecting already-clean data.

5. **Be aware of line break removal:** CLEAN removes CHAR(10) and CHAR(13). If you need multi-line text preserved, use SUBSTITUTE with placeholders.

6. **Check for CHAR(127):** The DEL character (127) is above CLEAN's range and can cause issues. Add `SUBSTITUTE(text, CHAR(127), "")` for thorough cleaning.

7. **Use CODE to diagnose:** When cleaning doesn't solve problems, use `=CODE(MID(A1, position, 1))` to identify specific problematic character codes.

8. **Create a master cleaning formula:** For consistently problematic data sources, build a comprehensive formula addressing all known issues and reuse it.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[TRIM]] | Removes extra spaces | When only spacing issues exist, no control characters |
| [[SUBSTITUTE]] | Replaces specific characters | When you need to target specific characters CLEAN misses |

### Commonly Used Together

**[[TRIM]]** - Removes extra spaces

*Combine for complete text normalization:*
```
=TRIM(CLEAN(A1))
```
CLEAN handles control characters, TRIM handles spacing. The essential duo.

---

**[[SUBSTITUTE]]** - Replaces specific text

*Handle non-breaking spaces:*
```
=CLEAN(SUBSTITUTE(A1, CHAR(160), " "))
```
SUBSTITUTE converts non-breaking spaces before CLEAN removes other control characters.

---

**[[CHAR]]** - Returns character from code

*Target specific control characters:*
```
=SUBSTITUTE(A1, CHAR(9), " ")
```
Convert tabs to spaces rather than removing them entirely with CLEAN.

---

**[[CODE]]** - Returns code for character

*Diagnose problematic characters:*
```
=CODE(MID(A1, 1, 1))
```
When CLEAN doesn't solve the problem, CODE helps identify what characters remain.

---

**[[LEN]]** - String length

*Detect hidden characters:*
```
=LEN(A1) - LEN(CLEAN(A1))
```
Non-zero result indicates non-printable characters were present.

---

**[[VLOOKUP]]** / **[[INDEX]]** / **[[MATCH]]** - Lookup functions

*Clean before matching:*
```
=VLOOKUP(TRIM(CLEAN(A1)), Table, 2, FALSE)
```
Clean lookup values to prevent matching failures from hidden characters.

---

**[[MID]]** - Extract specific character

*Isolate problematic characters:*
```
=CODE(MID(A1, FIND(problem_position, 1), 1))
```
Extract and identify specific characters causing issues.

## Official Documentation

- **Microsoft Excel:** [CLEAN function](https://support.microsoft.com/en-us/office/clean-function-26f3d7c5-475f-4a9c-90e5-4b8ba987ba41)
- **Google Sheets:** [CLEAN function](https://support.google.com/docs/answer/3267340)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original function |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Works with dynamic arrays |
| Google Sheets | Original launch (2006) | Identical implementation to Excel |

---

*Last updated: 2026-01-10*
