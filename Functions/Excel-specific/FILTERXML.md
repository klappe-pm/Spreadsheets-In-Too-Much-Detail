---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- web-functions
- xml-processing
subTopics:
- xpath-queries
- web-data-extraction
- api-integration
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Filter XML
- XML XPath
- Parse XML
tags:
- excel-only
- web
- xml
- xpath
- data-extraction
---

# FILTERXML

## Description

**FILTERXML** extracts specific content from XML text using XPath queries. It takes an XML string (typically from WEBSERVICE) and an XPath expression, returning the matching elements. This function is Excel's native XML parser, enabling you to consume web APIs, parse XML files, and extract structured data directly in formulas without VBA.

XPath is a query language for selecting nodes from XML documents. FILTERXML supports a subset of XPath 1.0 syntax, including path expressions, predicates (filters), and basic functions. You can navigate XML hierarchies, filter by attribute values, select specific positions in node lists, and extract text content.

**Critical limitations:** FILTERXML only works on the Windows version of Excel - it is NOT available on Mac Excel. It also has XPath limitations compared to full XPath processors: no support for axes like "ancestor::" or "following-sibling::", limited function support, and it cannot handle namespaced XML properly. Additionally, FILTERXML returns one result per cell; for multiple matches, you need array formulas or helper columns.

**Platform note:** FILTERXML is exclusive to Windows versions of Excel 2013 and later. Mac Excel does not support this function at all (returns #VALUE!). Google Sheets has no equivalent function. For cross-platform XML parsing, you'll need Power Query or external tools.

## Syntax

> [!f(x)] FILTERXML Syntax
>
> ```
> =FILTERXML(xml, xpath)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `xml` | Yes | A valid XML string. Can be literal text, a cell reference containing XML, or typically the result of WEBSERVICE function. |
| `xpath` | Yes | An XPath 1.0 expression specifying which nodes to extract. Must be valid XPath syntax. |

### Return Value

Returns the text content of the first matching XML node. If the XPath matches multiple nodes, only the first match is returned (unless used as an array formula). Returns #VALUE! if the XML is invalid or XPath finds no matches.

## Examples

> [!f(x)] FILTERXML Examples

### Example 1: Basic Element Extraction
```
=FILTERXML("<root><item>Apple</item><item>Banana</item></root>", "//item")
```
**Result:** `Apple`

**Explanation:** The XPath "//item" selects all <item> elements anywhere in the document. FILTERXML returns the text content of the first match.

---

### Example 2: Specific Element by Position
```
=FILTERXML("<root><item>Apple</item><item>Banana</item></root>", "//item[2]")
```
**Result:** `Banana`

**Explanation:** The predicate [2] selects the second <item> element. XPath uses 1-based indexing, so [2] gets the second match.

---

### Example 3: Extract Attribute Value
```
=FILTERXML("<products><product id='123' name='Widget'/></products>", "//product/@name")
```
**Result:** `Widget`

**Explanation:** The @ symbol selects attributes. "//product/@name" gets the name attribute from the first product element.

---

### Example 4: Filter by Attribute Value
```
=FILTERXML("<items><item type='fruit'>Apple</item><item type='veg'>Carrot</item></items>", "//item[@type='fruit']")
```
**Result:** `Apple`

**Explanation:** The predicate [@type='fruit'] filters to only elements where the type attribute equals 'fruit'.

---

### Example 5: Nested Element Navigation
```
=FILTERXML("<order><customer><name>John Smith</name><email>john@test.com</email></customer></order>", "/order/customer/name")
```
**Result:** `John Smith`

**Explanation:** Full path from root navigates the hierarchy. Leading / means absolute path from document root.

---

### Example 6: Count Elements
```
=FILTERXML("<data><row>1</row><row>2</row><row>3</row></data>", "count(//row)")
```
**Result:** `3`

**Explanation:** XPath count() function returns the number of matching nodes. Useful for knowing how many records exist.

---

### Example 7: Get All Items with Array Formula (Legacy)
```
=FILTERXML("<root><item>A</item><item>B</item><item>C</item></root>", "//item["&ROW(A1:A3)&"]")
```
Entered as array formula (Ctrl+Shift+Enter) in A1:A3

**Result:** A, B, C in cells A1, A2, A3

**Explanation:** By concatenating row numbers into the XPath, each cell gets a different position. This pattern extracts all items into separate cells.

---

### Example 8: Combine with WEBSERVICE
```
=FILTERXML(WEBSERVICE("http://api.example.com/data.xml"), "//price")
```
**Result:** Extracts the price element from the fetched XML

**Explanation:** The classic pattern: WEBSERVICE fetches XML from the web, FILTERXML extracts the needed data.

---

### Example 9: Numeric Comparison in Predicate
```
=FILTERXML("<items><item><name>A</name><qty>5</qty></item><item><name>B</name><qty>15</qty></item></items>", "//item[qty>10]/name")
```
**Result:** `B`

**Explanation:** The predicate [qty>10] filters to items where the qty child element exceeds 10, then /name extracts the name.

---

### Example 10: Using XPath Contains Function
```
=FILTERXML("<products><product>Blue Widget</product><product>Red Gadget</product></products>", "//product[contains(.,'Widget')]")
```
**Result:** `Blue Widget`

**Explanation:** The contains() function performs substring matching. The dot (.) refers to the current node's text content.

---

### Example 11: String Splitting Trick
```
=FILTERXML("<t><s>"&SUBSTITUTE(A1,",","</s><s>")&"</s></t>", "//s")
```
Where A1 = "Apple,Banana,Cherry"

**Result:** First word ("Apple") - use with array formula for all words

**Explanation:** Creative use: construct XML from delimited text, then use FILTERXML to split it. This was a popular pre-TEXTSPLIT technique.

---

### Example 12: Last Element
```
=FILTERXML("<data><value>10</value><value>20</value><value>30</value></data>", "//value[last()]")
```
**Result:** `30`

**Explanation:** The last() function returns the position of the final element. Useful when you need the most recent or final entry.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid XML structure | Check for unclosed tags, special characters, or malformed XML |
| `#VALUE!` | XPath finds no matches | Verify path exists; check element names are exact (case-sensitive) |
| `#VALUE!` | Running on Mac Excel | FILTERXML is Windows-only; no Mac support |
| `#VALUE!` | XML contains namespaces | FILTERXML doesn't handle namespaces well; use local-name() function |
| `#NAME?` | Excel version doesn't support FILTERXML | Requires Excel 2013+; no workaround in older versions |

## Use Cases

### [[Web API Data Extraction]]

**Scenario:** Retrieve and parse stock quote data from a financial API that returns XML.

**Implementation:**
```
=FILTERXML(
    WEBSERVICE("https://api.finance.com/quote?symbol=" & ENCODEURL(A1)),
    "//quote/price"
)
```

**Business Application:** Real-time data feeds can be pulled directly into Excel. Stock prices, exchange rates, weather data - any XML API becomes accessible.

**Technical Details:** Combine ENCODEURL for safe parameter passing, WEBSERVICE for fetching, and FILTERXML for parsing. The result refreshes when the workbook calculates.

---

### [[Text Splitting Without TEXTSPLIT]]

**Scenario:** Split comma-separated values into separate cells (useful before TEXTSPLIT existed).

**Implementation:**
```
=FILTERXML("<t><s>"&SUBSTITUTE(A1,",","</s><s>")&"</s></t>", "//s["&COLUMN(A1)&"]")
```
Drag horizontally to split all values.

**Business Application:** Legacy workbooks or older Excel versions without TEXTSPLIT can use this pattern to parse delimited data.

**Technical Details:** The text is wrapped in XML tags, delimiters become tag boundaries, and FILTERXML extracts by position. COLUMN() or ROW() provides the position number.

---

### [[Configuration File Parsing]]

**Scenario:** Read configuration values from an XML config file referenced in the workbook.

**Implementation:**
```
=FILTERXML(
    ConfigXML,
    "//setting[@name='MaxRetries']/@value"
)
```
Where ConfigXML is a named range containing the XML text.

**Business Application:** Applications can store settings in XML format. Excel can read these settings to control calculation parameters, enabling external configuration without editing formulas.

**Technical Details:** Store XML in a cell or fetch via WEBSERVICE. Named ranges make formulas cleaner. Attribute selectors (@) access configuration values.

## Platform Differences

### Microsoft Excel (Windows, 2013+)

| Feature | Support |
|---------|---------|
| Basic XPath | Full support |
| XPath predicates | Full support |
| XPath functions | Partial (count, contains, last, etc.) |
| Namespaced XML | Limited (use local-name() workaround) |
| Array results | Supported with array formulas |

### Microsoft Excel (Mac)

| Feature | Support |
|---------|---------|
| FILTERXML | NOT AVAILABLE |
| Returns | #VALUE! error |
| Alternative | Use Power Query or VBA |

**This is a major limitation.** Workbooks using FILTERXML will break on Mac. There is no formula workaround.

### Google Sheets

| Feature | Support |
|---------|---------|
| FILTERXML | NOT AVAILABLE |
| Alternative | Use IMPORTXML function (different syntax) |

**Google Sheets alternative:**
```
=IMPORTXML("https://api.example.com/data.xml", "//price")
```
IMPORTXML fetches AND parses in one function (different from Excel's two-step approach).

### Other Platforms

FILTERXML is exclusive to Windows Excel 2013+:
- Mac Excel: Not available (critical limitation)
- Google Sheets: Not available (use IMPORTXML instead)
- LibreOffice Calc: Not available
- Apple Numbers: Not available
- Excel Online: Not available

## Tips and Best Practices

1. **Use named ranges for XML:** Store XML in a named range if it's long. `=FILTERXML(MyXMLData, "//value")` is cleaner than embedding or referencing long cell content.

2. **Handle namespaces with local-name():** If XML has namespaces and paths fail, try: `"//*[local-name()='elementName']"` to ignore namespace prefixes.

3. **Test XPath separately:** Use online XPath testers to verify your expressions work before embedding in Excel.

4. **Check for Mac compatibility:** If your workbook might run on Mac, avoid FILTERXML or provide alternative calculations.

5. **Use array formulas for multiple results:** Single FILTERXML returns first match only. For all matches, use array formula with position predicates.

6. **Wrap in IFERROR:** `=IFERROR(FILTERXML(xml, xpath), "Not found")` handles cases where the XPath matches nothing.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WEBSERVICE]] | Fetches text from URL | To retrieve the XML before parsing with FILTERXML |
| [[ENCODEURL]] | URL-encodes text | To build safe URLs for WEBSERVICE |
| Google's IMPORTXML | Fetches and parses XML | In Google Sheets as an alternative approach |

### Commonly Used Together

**[[WEBSERVICE]]** - Fetch XML data to parse

*Complete web data extraction:*
```
=FILTERXML(WEBSERVICE(url), xpath)
```
WEBSERVICE fetches, FILTERXML parses. This is the standard pattern.

---

**[[ENCODEURL]]** - Build safe API URLs

*API query with parameters:*
```
=FILTERXML(WEBSERVICE("https://api.com/data?id=" & ENCODEURL(A1)), "//result")
```
ENCODEURL ensures parameters are URL-safe before fetching.

---

**[[IFERROR]]** - Handle parsing failures

*Graceful error handling:*
```
=IFERROR(FILTERXML(xml, "//element"), "Element not found")
```
Provides fallback when XPath matches nothing.

---

**[[SUBSTITUTE]]** - Pre-process XML or build XML

*Text-to-XML conversion for splitting:*
```
=FILTERXML("<t><s>" & SUBSTITUTE(text, ",", "</s><s>") & "</s></t>", "//s")
```
Creates XML structure from delimited text.

## Official Documentation

- **Microsoft Excel:** [FILTERXML function](https://support.microsoft.com/en-us/office/filterxml-function-4df72efc-11ec-4f60-a221-6a8c0e8d0031)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 (Windows) | 2013 | Initial release with Web functions |
| Excel 2016+ (Windows) | 2016 | Continued support |
| Excel 365 (Windows) | Current | Full support |
| Excel Mac | Never | Not supported on any Mac version |
| Excel Online | Not available | Web version doesn't support FILTERXML |

---

*Last updated: 2026-01-10*
