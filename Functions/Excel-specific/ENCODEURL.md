---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- web-functions
- text-functions
subTopics:
- url-encoding
- web-integration
- api-calls
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- URL Encode
- Percent Encoding
- URI Encoding
tags:
- excel-only
- web
- url-encoding
- api
---

# ENCODEURL

## Description

**ENCODEURL** converts text into URL-encoded format by replacing special characters with their percent-encoded equivalents (like spaces becoming %20). This is essential when constructing URLs programmatically, especially when passing variable data to web services, APIs, or building dynamic hyperlinks. Any character that isn't safe for URLs gets converted to its hexadecimal representation prefixed with %.

URL encoding is a fundamental requirement of web standards. Spaces, ampersands, question marks, non-ASCII characters, and many other characters have special meanings in URLs or are simply not allowed. When you need to include a product name like "Black & Decker" in a query string, it must become "Black%20%26%20Decker" to work correctly. ENCODEURL handles this conversion automatically.

**Important considerations:** ENCODEURL encodes ALL special characters, including characters that might be intentional URL syntax like "/" and "?". If you're building a complete URL, encode only the variable parts, not the entire URL structure. Also, the function uses UTF-8 encoding for non-ASCII characters, which is the modern standard but may cause issues with legacy systems expecting different encodings.

**Platform note:** ENCODEURL is an Excel-exclusive function. Google Sheets does not have a native equivalent, though similar results can be achieved using Google Apps Script or external tools. This function is only available in Excel 2013 and later versions.

## Syntax

> [!f(x)] ENCODEURL Syntax
>
> ```
> =ENCODEURL(text)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text string to be URL-encoded. Can be a cell reference, text in quotes, or any formula returning text. |

### Return Value

Returns a text string with all non-alphanumeric characters (except hyphen, underscore, period, and tilde) replaced by their percent-encoded equivalents. Returns an empty string if the input is empty.

## Examples

> [!f(x)] ENCODEURL Examples

### Example 1: Basic Space Encoding
```
=ENCODEURL("Hello World")
```
**Result:** `Hello%20World`

**Explanation:** The space character is not allowed in URLs and gets converted to %20 (the hex value of space is 20). This is the most common encoding scenario.

---

### Example 2: Special Characters
```
=ENCODEURL("Price: $100 & discount!")
```
**Result:** `Price%3A%20%24100%20%26%20discount%21`

**Explanation:** Multiple special characters are encoded: colon (:) becomes %3A, dollar ($) becomes %24, ampersand (&) becomes %26, exclamation (!) becomes %21. Each is replaced with % followed by its hexadecimal ASCII code.

---

### Example 3: Building a Search Query URL
```
="https://www.google.com/search?q=" & ENCODEURL(A1)
```
Where A1 = "Excel functions tutorial"

**Result:** `https://www.google.com/search?q=Excel%20functions%20tutorial`

**Explanation:** The base URL structure stays intact while the search term (from cell A1) is properly encoded. This creates a clickable Google search link.

---

### Example 4: API Query Parameter
```
="https://api.example.com/data?name=" & ENCODEURL(B2) & "&city=" & ENCODEURL(C2)
```
Where B2 = "John Smith", C2 = "New York"

**Result:** `https://api.example.com/data?name=John%20Smith&city=New%20York`

**Explanation:** Multiple parameters are individually encoded while preserving the URL structure (the literal & between parameters is not encoded because it's outside the ENCODEURL calls).

---

### Example 5: Non-ASCII Characters
```
=ENCODEURL("Cafe")
```
**Result:** `Caf%C3%A9`

**Explanation:** The e-acute character (e) is a multi-byte UTF-8 character. It gets encoded as %C3%A9 (its UTF-8 byte sequence). This ensures international characters work correctly in URLs.

---

### Example 6: Already Safe Characters
```
=ENCODEURL("Simple-Text_123.test~value")
```
**Result:** `Simple-Text_123.test~value`

**Explanation:** Hyphens, underscores, periods, and tildes are considered "safe" URL characters along with alphanumerics. They pass through unchanged.

---

### Example 7: Forward Slash Encoding
```
=ENCODEURL("path/to/resource")
```
**Result:** `path%2Fto%2Fresource`

**Explanation:** Forward slashes ARE encoded because ENCODEURL doesn't know if they're meant to be path separators or literal text. Encode only values, not URL structure.

---

### Example 8: Question Mark and Equals Sign
```
=ENCODEURL("What is 2+2=4?")
```
**Result:** `What%20is%202%2B2%3D4%3F`

**Explanation:** Mathematical and question symbols are all encoded: plus (+) becomes %2B, equals (=) becomes %3D, question mark (?) becomes %3F. Plus is especially important as it can mean "space" in some URL contexts.

---

### Example 9: Dynamic HYPERLINK Creation
```
=HYPERLINK("https://maps.google.com/?q=" & ENCODEURL(Address), "View Map")
```
Where Address = "123 Main St, New York, NY"

**Result:** Clickable "View Map" link pointing to the encoded address

**Explanation:** ENCODEURL makes user-provided addresses safe for Google Maps URLs. The HYPERLINK function creates a clickable link in the cell.

---

### Example 10: Email Subject Line
```
="mailto:support@example.com?subject=" & ENCODEURL(A1)
```
Where A1 = "Order #12345 - Question about delivery"

**Result:** `mailto:support@example.com?subject=Order%20%2312345%20-%20Question%20about%20delivery`

**Explanation:** Creating mailto links with pre-filled subjects requires encoding. The # symbol especially needs encoding (%23) as it has special URL meaning.

---

### Example 11: JSON-like Data in URL
```
=ENCODEURL("{""name"":""John"",""age"":30}")
```
**Result:** `%7B%22name%22%3A%22John%22%2C%22age%22%3A30%7D`

**Explanation:** Passing JSON data in query strings requires encoding curly braces, quotes, and colons. The result is verbose but URL-safe.

---

### Example 12: Combined with WEBSERVICE
```
=WEBSERVICE("https://api.exchangerate.host/latest?base=" & ENCODEURL(B1))
```
Where B1 = "USD"

**Result:** Fetches exchange rate data for the currency specified in B1

**Explanation:** ENCODEURL ensures the currency code is safe even if someone enters unusual characters. This is defensive programming for web queries.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Input is an error value | Wrap in IFERROR or check source data: `=IFERROR(ENCODEURL(A1), "")` |
| `#NAME?` | Excel version doesn't support ENCODEURL | Requires Excel 2013+; no workaround in older versions |
| Broken URL | Entire URL was encoded including structure | Only encode the variable parts, not "https://", "?", or "&" separators |
| Double encoding | Already-encoded text was re-encoded | %20 becomes %2520; ensure input is raw text, not pre-encoded |
| Unexpected encoding | Characters you expected to pass through were encoded | Only A-Z, a-z, 0-9, -, _, ., ~ pass through unencoded |

## Use Cases

### [[Dynamic Report Links]]

**Scenario:** Create clickable links to filtered dashboard views based on spreadsheet data.

**Implementation:**
```
=HYPERLINK(
    "https://dashboard.company.com/reports?dept=" & ENCODEURL(A2) &
    "&period=" & ENCODEURL(B2) &
    "&filter=" & ENCODEURL(C2),
    "View Report")
```

**Business Application:** Sales reports can include links that pre-filter the company dashboard to show specific department data. Users click directly from Excel to filtered views.

**Technical Details:** Each parameter is separately encoded. The structural parts (&, =) remain literal while values are safely encoded.

---

### [[API Integration]]

**Scenario:** Build REST API calls that include user-specified search terms.

**Implementation:**
```
=WEBSERVICE(
    "https://api.weather.com/v1/forecast?location=" &
    ENCODEURL(CityName) &
    "&apikey=" & APIKey
)
```

**Business Application:** Automated weather data retrieval, stock quotes, or other API integrations where query parameters come from spreadsheet cells.

**Technical Details:** API keys often contain special characters and should also be encoded. Some APIs have specific encoding requirements - check their documentation.

---

### [[Email Campaign Links]]

**Scenario:** Generate personalized mailto links for email campaigns.

**Implementation:**
```
="mailto:" & A2 & "?subject=" & ENCODEURL(B2) & "&body=" & ENCODEURL(C2)
```

**Business Application:** Customer service teams can generate personalized email links with pre-filled subjects and body text, ensuring consistent messaging.

**Technical Details:** Email body text often includes line breaks. Use CHAR(10) for line breaks, which ENCODEURL will convert to %0A.

## Platform Differences

### Microsoft Excel (2013+)

| Feature | Support |
|---------|---------|
| Basic encoding | Full support |
| UTF-8 encoding | Full support |
| Non-ASCII characters | Full support |
| Empty string handling | Returns empty string |

### Google Sheets

| Feature | Support |
|---------|---------|
| ENCODEURL | NOT AVAILABLE |
| Alternative | Use custom function via Apps Script |

**Workaround for Google Sheets:**
```javascript
// In Apps Script (Tools > Script editor)
function ENCODEURL(text) {
  return encodeURIComponent(text);
}
```
After adding this script, use `=ENCODEURL(A1)` in Sheets.

### Other Platforms

ENCODEURL is exclusive to Microsoft Excel 2013 and later:
- Google Sheets: Not available (requires Apps Script workaround)
- LibreOffice Calc: Not available
- Apple Numbers: Not available
- Excel 2010 and earlier: Not available

## Tips and Best Practices

1. **Encode only values, not URL structure:** Wrong: `=ENCODEURL("https://example.com?q=test")`. Right: `="https://example.com?q=" & ENCODEURL("test")`.

2. **Don't double-encode:** If data is already URL-encoded, encoding again corrupts it. %20 becomes %2520. Check your source data.

3. **Combine with WEBSERVICE for API calls:** ENCODEURL is designed to work with WEBSERVICE. Build safe API URLs by encoding all variable parameters.

4. **Handle errors gracefully:** `=IFERROR(ENCODEURL(A1), "")` prevents #VALUE! errors from propagating.

5. **Consider URL length limits:** URLs have practical length limits (around 2000 characters). Long encoded strings can exceed this.

6. **Test with special cases:** Always test with spaces, ampersands, question marks, and international characters before deploying.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WEBSERVICE]] | Fetches data from web URL | After building URL with ENCODEURL |
| [[FILTERXML]] | Parses XML returned by WEBSERVICE | To extract data from web responses |
| [[HYPERLINK]] | Creates clickable link | To make encoded URLs clickable |

### Commonly Used Together

**[[WEBSERVICE]]** - Fetch data from the encoded URL

*Complete API workflow:*
```
=WEBSERVICE("https://api.example.com/data?q=" & ENCODEURL(SearchTerm))
```
ENCODEURL makes the query safe, WEBSERVICE retrieves the data.

---

**[[HYPERLINK]]** - Create clickable links

*Clickable search link:*
```
=HYPERLINK("https://google.com/search?q=" & ENCODEURL(A1), "Search")
```
HYPERLINK displays friendly text while the URL remains properly encoded.

---

**[[FILTERXML]]** - Parse XML responses

*Full API data extraction:*
```
=FILTERXML(WEBSERVICE("https://api.example.com/xml?q=" & ENCODEURL(A1)), "//result/value")
```
Chain: encode query, fetch data, extract results.

---

**[[SUBSTITUTE]]** - Pre-process text before encoding

*Clean up data before encoding:*
```
=ENCODEURL(SUBSTITUTE(SUBSTITUTE(A1, CHAR(10), " "), CHAR(13), ""))
```
Remove line breaks before encoding if you want spaces instead.

## Official Documentation

- **Microsoft Excel:** [ENCODEURL function](https://support.microsoft.com/en-us/office/encodeurl-function-07c7fb90-7c60-4f1d-a0c3-88e6a6989be4)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 | 2013 | Initial release with Web functions |
| Excel 2016+ | 2016 | Continued support |
| Excel 365 | Current | Full support |
| Excel Online | Supported | Available in web version |
| Earlier versions | Not available | No support in Excel 2010 or earlier |

---

*Last updated: 2026-01-10*
