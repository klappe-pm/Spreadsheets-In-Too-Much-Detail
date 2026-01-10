---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - google-specific
  - text-manipulation
subTopics:
  - text-splitting
  - delimiter-parsing
  - data-extraction
  - text-to-columns
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - split-text
  - text-to-columns
  - delimiter-split
  - parse-text
tags:
  - text-functions
  - data-parsing
  - delimiters
  - text-manipulation
  - data-cleaning
---

# SPLIT

## Description

The **SPLIT function** divides text around a specified delimiter or delimiters, outputting each segment into separate cells in a horizontal array. This powerful text manipulation function is essential for parsing structured data like comma-separated values, breaking apart full names into first and last components, extracting data from formatted strings, and any situation where you need to separate a single text value into multiple parts. SPLIT takes a text string and a delimiter (or multiple delimiters) and returns an array of values, with each segment becoming a separate cell.

The function is particularly valuable when dealing with imported data that combines multiple pieces of information in single cells. For example, addresses like "123 Main St, Springfield, IL, 62701" can be split on commas to place each component in its own column for separate processing. Similarly, full names like "John Smith" can be split on spaces to separate first and last names. SPLIT handles the complexity of parsing automatically, eliminating the need for complex LEFT/RIGHT/FIND formulas that would otherwise be required to extract substrings.

SPLIT offers two optional parameters that provide control over its behavior: split_by_each, which determines whether each character in the delimiter string should be treated as a separate delimiter, and remove_empty_text, which controls whether empty strings from consecutive delimiters are included in the output. These options enable handling of complex parsing scenarios like splitting on multiple delimiter types simultaneously (commas and semicolons) or preserving empty values when data fields are missing between delimiters.

Originally a Google Sheets-exclusive function, SPLIT has now been introduced to Microsoft Excel as part of the TEXTSPLIT function in Excel 365 (2022). However, the syntax differs between platforms—Excel's TEXTSPLIT uses different parameter names and ordering. The Google Sheets SPLIT function remains widely used and is the standard approach for text splitting in Sheets. For cross-platform compatibility, be aware that formulas using SPLIT will need adjustment when moving between Sheets and Excel.

## Syntax

> [!f(x)] SPLIT Syntax
>
> ```
> =SPLIT(text, delimiter, [split_by_each], [remove_empty_text])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string to split. Can be a literal string in quotes, a cell reference, or an expression returning text. This is the input that will be divided into parts. |
| `delimiter` | Yes | The character or string to split around. Each occurrence of this delimiter marks where the text should be divided. Can be a single character (","), multiple characters ("::"), or a string of delimiters when split_by_each is TRUE. |
| `split_by_each` | No | If TRUE, each character in the delimiter string is treated as a separate delimiter. If FALSE (default), the entire delimiter string must match to trigger a split. For example, with delimiter ", " and split_by_each=TRUE, both commas and spaces cause splits. |
| `remove_empty_text` | No | If TRUE (default), empty text values between consecutive delimiters are removed from the result. If FALSE, empty strings are preserved as empty cells in the output. Useful when position matters or missing values are significant. |

### Return Value

Returns a horizontal array of text values, with one cell for each segment between delimiters. The original text (before the first delimiter), text between each pair of delimiters, and text after the final delimiter are all included as separate cells. The number of cells in the result depends on how many times the delimiter appears in the input.

## Examples

> [!f(x)] SPLIT Examples

### Example 1: Basic Comma Split
```
=SPLIT("apple,banana,cherry", ",")
```
**Scenario:** Splitting a comma-separated list into individual items
**Result:** | apple | banana | cherry | (three cells)

**Explanation:** The function finds each comma and creates separate cells for the text between them. This is the most common SPLIT use case for parsing CSV-style data.

---

### Example 2: Split on Space for Names
```
=SPLIT("John Michael Smith", " ")
```
**Scenario:** Breaking a full name into components
**Result:** | John | Michael | Smith | (three cells)

**Explanation:** Splitting on space separates each word. For names with multiple parts, this creates multiple outputs. For first/last name extraction, you may need INDEX to select specific parts.

---

### Example 3: Split on Multi-Character Delimiter
```
=SPLIT("item1::item2::item3", "::")
```
**Scenario:** Data using double-colon as separator
**Result:** | item1 | item2 | item3 |

**Explanation:** When split_by_each is FALSE (default), the entire delimiter string "::" must appear. Single colons within text would not cause splits.

---

### Example 4: Split by Each Character
```
=SPLIT("a,b;c:d", ",;:", TRUE)
```
**Scenario:** Splitting on multiple different delimiter types
**Result:** | a | b | c | d |

**Explanation:** With split_by_each=TRUE, each character in ",;:" is treated as a separate delimiter. The text splits on commas, semicolons, and colons.

---

### Example 5: Preserving Empty Values
```
=SPLIT("a,,b,,c", ",", FALSE, FALSE)
```
**Scenario:** Data with missing values between delimiters
**Result:** | a | (empty) | b | (empty) | c |

**Explanation:** With remove_empty_text=FALSE, consecutive delimiters produce empty cells. This preserves positional information when some fields are empty.

---

### Example 6: Removing Empty Values (Default)
```
=SPLIT("a,,b,,c", ",")
```
**Scenario:** Same data, default behavior
**Result:** | a | b | c |

**Explanation:** By default (remove_empty_text=TRUE), empty values are removed. The output contains only non-empty segments.

---

### Example 7: Split Cell Reference
```
=SPLIT(A1, ",")
```
**Scenario:** A1 contains "red, green, blue" (comma-space separated)
**Result:** | red |  green |  blue | (note leading spaces)

**Explanation:** The function splits only on the specified delimiter. If your data has "comma-space" separators, split on ", " (with space) to avoid leading spaces in results.

---

### Example 8: Extract First Element with INDEX
```
=INDEX(SPLIT(A1, ","), 1, 1)
```
**Scenario:** Extract only the first item from "apple,banana,cherry"
**Result:** apple

**Explanation:** INDEX(array, 1, 1) returns the first cell from SPLIT's output. This pattern extracts specific positions without displaying all split values.

---

### Example 9: Extract Last Element
```
=INDEX(SPLIT(A1, ","), 1, COUNTA(SPLIT(A1, ",")))
```
**Scenario:** Get the last item from a variable-length list
**Result:** cherry (from "apple,banana,cherry")

**Explanation:** COUNTA counts how many elements SPLIT produces. INDEX then retrieves that position, giving the last element regardless of list length.

---

### Example 10: Split and Join in Different Order
```
=JOIN("-", SORT(TRANSPOSE(SPLIT("c,a,b", ",")), 1, TRUE))
```
**Scenario:** Split a list, sort it, and rejoin with different delimiter
**Result:** "a-b-c"

**Explanation:** This pattern demonstrates SPLIT combined with other functions for data transformation—split into parts, process parts, rejoin.

---

### Example 11: Split with ARRAYFORMULA for Multiple Rows
```
=ARRAYFORMULA(SPLIT(A2:A10, ","))
```
**Scenario:** Split values in multiple cells at once
**Result:** Multiple rows, each with their split values

**Explanation:** ARRAYFORMULA enables SPLIT to process multiple cells. Each row gets its values split into columns. Rows with different numbers of values may create uneven results.

---

### Example 12: Split URL into Components
```
=SPLIT("https://www.example.com/path/to/page", "/")
```
**Scenario:** Parse URL components
**Result:** | https: | (empty) | www.example.com | path | to | page |

**Explanation:** Splitting URLs on "/" separates protocol and path components. Note the empty cell from "//" in "https://". Use SPLIT with other functions for complete URL parsing.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Empty text or delimiter | Ensure both text and delimiter have values; check for empty cell references |
| `#REF!` | Output would overwrite existing data | Clear cells to the right of the formula or move the formula |
| `Unexpected empty cells in output` | Consecutive delimiters with remove_empty_text=FALSE | Use TRUE for remove_empty_text if empty values aren't needed |
| `Results have leading/trailing spaces` | Delimiter doesn't include spaces | Include space in delimiter (", " not ",") or use TRIM on results |
| `Delimiter not found, returns original text` | Delimiter doesn't exist in text | Verify delimiter matches exactly (case-sensitive for some characters) |
| `Split creates too many columns` | Many delimiters in source text | Consider using INDEX to extract specific positions |
| `ARRAYFORMULA not working as expected` | Rows have different numbers of delimiters | Each row produces different column counts; consider alternative approaches |
| `Splitting on wrong characters` | split_by_each mixing up intention | Set split_by_each explicitly to TRUE or FALSE based on need |

## Use Cases

### [[Parsing Full Names into Components]]

**Scenario:** An HR database has full names in a single column, but the payroll system requires separate first name, middle name, and last name fields. Names may have 2, 3, or more parts, and the system needs to handle variations gracefully.

**Implementation:**
```
Name parsing structure:
Column A: Full Name (input)
Column B: First Name
Column C: Middle Name(s)
Column D: Last Name

First Name (B2):
=IFERROR(INDEX(SPLIT(A2, " "), 1, 1), A2)

Last Name (D2):
=IFERROR(INDEX(SPLIT(A2, " "), 1, COUNTA(SPLIT(A2, " "))), "")

Middle Name - everything between first and last (C2):
=IF(COUNTA(SPLIT(A2, " "))>2,
  JOIN(" ", ARRAY_CONSTRAIN(OFFSET(SPLIT(A2, " "), 0, 1), 1, COUNTA(SPLIT(A2, " "))-2)),
  "")

Alternative for simple first/last only:
=IFERROR(REGEXEXTRACT(A2, "^(\S+)"), A2)  -- First word
=IFERROR(REGEXEXTRACT(A2, "(\S+)$"), "")  -- Last word
```

**Business Application:** Name parsing is one of the most common data cleaning tasks. HR, CRM, and mailing systems often require name components separately. SPLIT provides the foundation for this parsing, though edge cases (Jr., III, von, de la) may require additional logic or manual review.

**Technical Details:** The INDEX/SPLIT combination extracts specific positions. COUNTA(SPLIT(...)) counts how many parts exist. For robust name parsing, consider handling prefixes (Dr., Mr.), suffixes (Jr., III), and compound surnames. REGEXEXTRACT offers alternative pattern-based extraction.

---

### [[Processing CSV Data Within Cells]]

**Scenario:** Survey responses are exported with multi-select answers in single cells, like "Option A, Option B, Option D". The analysis team needs to count how often each option appears across all responses and create individual records for each selection.

**Implementation:**
```
Survey analysis structure:
Column A: Respondent ID
Column B: Multi-select Response (e.g., "A, B, D")
Columns C onwards: Split responses

Split Responses (C2):
=SPLIT(B2, ", ")

Count specific option across all responses:
=SUMPRODUCT(--REGEXMATCH(B:B, "Option A"))
or
=COUNTIF(FLATTEN(SPLIT(B:B, ", ")), "Option A")

Create normalized table (one row per selection):
Use FLATTEN and SPLIT combination:
=QUERY(FLATTEN(ARRAYFORMULA(IF(B2:B100="", "", A2:A100 & "|" & SPLIT(B2:B100, ", ")))),
  "WHERE Col1 <> '' ")
Then SPLIT the results on "|" to separate ID from option
```

**Business Application:** Survey tools often export multi-select responses as delimited lists. Analysis requires either counting option frequencies or "exploding" the data into normalized form (one row per selection). SPLIT enables both approaches, transforming compact data into analyzable structure.

**Technical Details:** FLATTEN collapses the split results into a single column. For counting, REGEXMATCH can check if a value exists without full splitting. The normalized table approach creates ID|Option pairs that can be further processed. Consider memory limits with very large datasets.

---

### [[Extracting Domain from Email Addresses]]

**Scenario:** A marketing team has a list of email addresses and needs to analyze which email domains are most common, separate personal from business emails, and group contacts by company domain.

**Implementation:**
```
Email analysis structure:
Column A: Email Address
Column B: Domain
Column C: Domain Type
Column D: Company Name

Extract Domain (B2):
=INDEX(SPLIT(A2, "@"), 1, 2)

Classify Domain Type (C2):
=IF(REGEXMATCH(B2, "gmail|yahoo|hotmail|outlook|aol"),
  "Personal",
  "Business")

Extract Company from Domain (D2):
=IFERROR(INDEX(SPLIT(B2, "."), 1, 1), B2)

Domain frequency analysis:
=QUERY(B:B, "SELECT B, COUNT(B) WHERE B IS NOT NULL GROUP BY B ORDER BY COUNT(B) DESC")

Top business domains:
=QUERY({B:B, C:C}, "SELECT Col1, COUNT(Col1) WHERE Col2 = 'Business' GROUP BY Col1 ORDER BY COUNT(Col1) DESC LIMIT 10")
```

**Business Application:** Email domain analysis helps marketing teams understand their audience composition, identify B2B leads (business domains), segment personal vs. professional contacts, and find concentrations of contacts at specific companies. SPLIT on "@" cleanly separates the domain portion.

**Technical Details:** The @ symbol always separates user from domain in email addresses, making SPLIT reliable here. Further splitting on "." extracts subdomains or the primary domain name. REGEXMATCH against common personal email providers enables classification.

---

### [[Parsing Addresses into Components]]

**Scenario:** A real estate database has property addresses in a single field like "123 Main St, Apt 4, Springfield, IL, 62701". For analysis and mail merge, the system needs separate fields for street, unit, city, state, and ZIP code.

**Implementation:**
```
Address parsing structure:
Column A: Full Address
Column B: Street
Column C: Unit/Apt
Column D: City
Column E: State
Column F: ZIP

Basic comma split approach:
Street (B2): =TRIM(INDEX(SPLIT(A2, ","), 1, 1))
City (D2): =TRIM(INDEX(SPLIT(A2, ","), 1, COUNTA(SPLIT(A2, ","))-2))
State (E2): =TRIM(INDEX(SPLIT(A2, ","), 1, COUNTA(SPLIT(A2, ","))-1))
ZIP (F2): =TRIM(INDEX(SPLIT(A2, ","), 1, COUNTA(SPLIT(A2, ","))))

Handling unit numbers (more complex):
=IF(REGEXMATCH(INDEX(SPLIT(A2, ","), 1, 2), "(?i)apt|unit|suite|#"),
  INDEX(SPLIT(A2, ","), 1, 2),
  "")

State/ZIP combined then split:
=TRIM(INDEX(SPLIT(INDEX(SPLIT(A2, ","), 1, COUNTA(SPLIT(A2, ","))), " "), 1, 1)) -- State
=TRIM(INDEX(SPLIT(INDEX(SPLIT(A2, ","), 1, COUNTA(SPLIT(A2, ","))), " "), 1, 2)) -- ZIP
```

**Business Application:** Address parsing is notoriously complex due to varying formats. SPLIT on comma provides a starting point, but addresses don't follow strict standards. This approach works for consistently formatted addresses but may require manual review for variations.

**Technical Details:** TRIM removes leading/trailing spaces from split results. Nested SPLIT calls handle compound fields (State ZIP). REGEXMATCH helps identify optional components like apartment numbers. For production systems, consider using address validation APIs.

## Platform Differences

SPLIT originated in Google Sheets. Excel 365 introduced the TEXTSPLIT function with similar but different syntax.

### Google Sheets
- **Function Name:** SPLIT
- **Syntax:** SPLIT(text, delimiter, [split_by_each], [remove_empty_text])
- **Default Behavior:** Splits each delimiter character by default (split_by_each=TRUE effective)
- **Array Output:** Horizontal (single row)
- **Availability:** All versions

### Microsoft Excel
- **Function Name:** TEXTSPLIT (introduced 2022)
- **Syntax:** TEXTSPLIT(text, col_delimiter, [row_delimiter], [ignore_empty], [match_mode], [pad_with])
- **Default Behavior:** Uses full delimiter string by default
- **Additional Features:** Row delimiters for 2D splits, padding for uneven results
- **Availability:** Excel 365 only (not in Excel 2019 or earlier)

### Key Differences Summary

| Feature | Google Sheets (SPLIT) | Microsoft Excel (TEXTSPLIT) |
|---------|----------------------|---------------------------|
| Function Name | SPLIT | TEXTSPLIT |
| Each Character Splitting | Built-in option | Requires multiple calls |
| 2D Splitting (rows and cols) | Not supported | Supported |
| Padding Uneven Results | Not supported | Supported |
| Empty Value Handling | remove_empty_text | ignore_empty |
| Legacy Support | All versions | 365 only |

## Tips and Best Practices

1. **Include spaces in delimiters when appropriate:** If your data uses "comma-space" separation (", "), split on ", " rather than just ",". Otherwise, results will have leading spaces that need to be trimmed.

2. **Use TRIM on results for clean data:** Even with correct delimiters, source data may have extra spaces. Wrap INDEX(SPLIT(...)) in TRIM to ensure clean output.

3. **Set split_by_each explicitly:** Don't rely on default behavior—explicitly set TRUE or FALSE based on your needs. This makes formulas clearer and prevents surprises with multi-character delimiter strings.

4. **Extract specific positions with INDEX:** When you only need one part of the split (like the first name or domain), use INDEX(SPLIT(...), 1, position) rather than displaying the full array.

5. **Handle variable-length splits gracefully:** Not all inputs have the same number of parts. Use IFERROR to handle cases where INDEX references a position that doesn't exist.

6. **Preserve empty values when position matters:** Use remove_empty_text=FALSE when empty fields between delimiters are meaningful, such as in fixed-format data where field position indicates meaning.

7. **Consider REGEXEXTRACT for complex patterns:** When delimiters vary or patterns are complex, REGEXEXTRACT may be more appropriate than SPLIT. Use SPLIT for consistent delimiters, REGEX for patterns.

8. **Test with edge cases:** Test your SPLIT formulas with empty strings, strings with no delimiters, strings with only delimiters, and strings with consecutive delimiters to ensure robust handling.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[JOIN]] | Combines values with a delimiter | Reverse of SPLIT—joining array elements into one string |
| [[REGEXEXTRACT]] | Extracts text matching a pattern | When extraction follows a pattern rather than delimiter-based |
| [[LEFT]] / [[RIGHT]] / [[MID]] | Extract substring by position | When you know exact character positions |
| [[TEXTSPLIT]] | Excel's text splitting function | In Excel 365 environments |

### Commonly Used Together

**[[INDEX]]** - Extract specific position from split results

*Get the first element from a split:*
```
=INDEX(SPLIT(A1, ","), 1, 1)
```
Retrieves a specific position from the array output.

---

**[[TRIM]]** - Remove extra whitespace from results

*Clean up split values:*
```
=TRIM(INDEX(SPLIT(A1, ","), 1, 2))
```
Removes leading/trailing spaces from extracted text.

---

**[[JOIN]]** - Recombine after processing

*Split, process, and rejoin:*
```
=JOIN("-", SPLIT(A1, ","))
```
The inverse operation—join splits a string, JOIN combines array elements.

---

**[[ARRAYFORMULA]]** - Process multiple cells

*Split values in a column:*
```
=ARRAYFORMULA(SPLIT(A2:A10, ","))
```
Enables SPLIT to process multiple cells at once.

---

**[[IFERROR]]** - Handle missing positions gracefully

*Safely extract optional fields:*
```
=IFERROR(INDEX(SPLIT(A1, ","), 1, 3), "")
```
Returns empty string if the third position doesn't exist.

---

**[[COUNTA]]** - Count split elements

*Determine how many parts exist:*
```
=COUNTA(SPLIT(A1, ","))
```
Returns the number of elements in the split result.

## Official Documentation

- **Google Sheets:** [SPLIT function](https://support.google.com/docs/answer/3094136)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | ~2010 | Original implementation |
| Google Sheets | 2015 | Added split_by_each and remove_empty_text parameters |
| Google Sheets | 2018 | Performance improvements for large splits |
| Google Sheets | 2020 | Better ARRAYFORMULA compatibility |
| Microsoft Excel | 2022 (365) | TEXTSPLIT function introduced with different syntax |

---

*Last updated: 2026-01-10*
