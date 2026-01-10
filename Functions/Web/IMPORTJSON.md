---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - web-data
  - api-integration
subTopics:
  - json-parsing
  - web-scraping
  - data-import
  - rest-api
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - import-json
  - json-import
  - api-import
  - fetch-json
tags:
  - web
  - json
  - api
  - import
  - custom-function
  - google-sheets
  - sheets-only
  - apps-script
---

# IMPORTJSON

## Description

IMPORTJSON is a custom Google Sheets function (not built-in) that fetches JSON data from a URL and imports it into spreadsheet cells, enabling direct integration with REST APIs, web services, and JSON data sources. Unlike Google Sheets' built-in IMPORTDATA function which only handles CSV/TSV formats, IMPORTJSON parses the hierarchical JSON structure and flattens it into a tabular format suitable for spreadsheet analysis. This function must be added to your spreadsheet via Google Apps Script before it can be used, making it a community-developed solution that has become widely adopted for API data integration in Google Sheets.

The most popular implementation of IMPORTJSON (originally created by Brad Jasper and further developed by Trevor Lohrbeer) supports fetching JSON from any publicly accessible URL, extracting specific data using JSONPath-like query syntax, customizing output with various options for headers and transformations, and handling both arrays and nested objects. Common use cases include importing cryptocurrency prices and market data, pulling weather information from APIs, fetching stock quotes and financial data, retrieving social media metrics, accessing CRM or business application data via APIs, importing IoT sensor data, and any scenario where JSON-formatted data needs to flow into spreadsheet analysis.

Several critical aspects require careful attention when using IMPORTJSON. First, this is NOT a built-in function—you must install the Apps Script code in your spreadsheet's Script Editor (Extensions > Apps Script). Second, the function makes HTTP requests from Google's servers, so the target URL must be publicly accessible (authenticated APIs require additional handling). Third, Google Sheets has execution time limits (approximately 30 seconds for custom functions) and URL fetch quotas that may affect reliability with slow or rate-limited APIs. Fourth, IMPORTJSON results are static snapshots; they don't auto-refresh unless you use triggers or manual recalculation. Fifth, deeply nested JSON structures may require JSONPath queries to extract specific data. Sixth, some APIs require headers (API keys, authentication) which the basic IMPORTJSON may not support without modification.

IMPORTJSON is a custom function that works exclusively in Google Sheets via Apps Script. It has no equivalent in Microsoft Excel without VBA or Power Query. Excel users seeking similar functionality should use Power Query (Get & Transform Data) which can natively connect to JSON web sources, or write VBA macros using the MSXML2 or WinHttp libraries. For cross-platform work, consider using an intermediary service or pre-processing JSON data into CSV format, which both platforms can import easily.

## Installation

Before using IMPORTJSON, you must add the script to your Google Sheets:

1. Open your Google Sheets document
2. Go to **Extensions > Apps Script**
3. Delete any existing code in the editor
4. Copy and paste the IMPORTJSON script (see Resources section for source)
5. Save the project (Ctrl+S or File > Save)
6. Return to your spreadsheet and use the function

**Note:** Each spreadsheet requires its own installation. The function is not globally available.

## Syntax

> [!f(x)] IMPORTJSON Syntax
>
> ```
> =IMPORTJSON(url, [query], [options])
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `url` | The URL of the JSON data source. Must return valid JSON. Can be a direct URL string in quotes or a cell reference containing the URL. The endpoint must be publicly accessible from Google's servers. | Yes |
> | `query` | JSONPath-like query to extract specific data from the JSON structure. Use "/" for root, "/path/to/data" for nested objects, or leave empty to import all data. Multiple queries can be comma-separated. | No |
> | `options` | Comma-separated list of processing options: "noHeaders" (omit headers), "noTruncate" (don't truncate values), "noInherit" (don't inherit values in arrays), "rawHeaders" (use raw JSON keys as headers), "allHeaders" (include all nested headers). | No |

## JSONPath Query Syntax

| Query | Description | Example |
|-------|-------------|---------|
| `/` | Root of JSON | Returns entire JSON structure |
| `/key` | Direct child key | `/data` gets the "data" object |
| `/key1/key2` | Nested path | `/data/items` navigates to nested items |
| `/key/0` | Array index | `/items/0` gets first array element |
| `/key1,/key2` | Multiple queries | `/name,/price` extracts both fields |
| `//key` | Recursive search | `//price` finds all "price" keys at any level |

## Examples

### Example 1: Import Simple JSON Object
```
=IMPORTJSON("https://api.example.com/user/1")
```
Where the URL returns `{"id": 1, "name": "John", "email": "john@example.com"}`

**Result:**
```
id    | name | email
1     | John | john@example.com
```

**Explanation:** For simple JSON objects, IMPORTJSON creates headers from the keys and values in a row below. This is the most basic use case.

---

### Example 2: Import JSON Array
```
=IMPORTJSON("https://api.example.com/users")
```
Where the URL returns `[{"id": 1, "name": "John"}, {"id": 2, "name": "Jane"}]`

**Result:**
```
id | name
1  | John
2  | Jane
```

**Explanation:** JSON arrays are converted to rows, with each object becoming a row and keys becoming column headers. This is ideal for list-based API responses.

---

### Example 3: Extract Nested Data with Query
```
=IMPORTJSON("https://api.example.com/response", "/data/users")
```
Where the URL returns `{"status": "ok", "data": {"users": [{"name": "John"}, {"name": "Jane"}]}}`

**Result:**
```
name
John
Jane
```

**Explanation:** The query `/data/users` navigates into the nested structure, extracting only the users array. Without the query, the entire response would be flattened.

---

### Example 4: Cryptocurrency Price Data
```
=IMPORTJSON("https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd,eur")
```

**Result:**
```
bitcoin/usd | bitcoin/eur
45000       | 38000
```

**Explanation:** This fetches live Bitcoin prices from CoinGecko's free API. The nested structure (bitcoin.usd, bitcoin.eur) is flattened into column headers.

---

### Example 5: Weather Data Import
```
=IMPORTJSON("https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_API_KEY", "/main")
```

**Result:**
```
temp   | feels_like | humidity | pressure
283.15 | 280.5      | 76       | 1015
```

**Explanation:** The query `/main` extracts only the main weather metrics, excluding other response sections like wind, clouds, and system data.

---

### Example 6: Extract Multiple Specific Fields
```
=IMPORTJSON("https://api.example.com/product/123", "/name,/price,/stock")
```

**Result:**
```
name         | price | stock
Widget Pro   | 29.99 | 150
```

**Explanation:** Multiple comma-separated queries extract specific fields from the JSON. Only the requested fields appear in the output.

---

### Example 7: Import Without Headers
```
=IMPORTJSON("https://api.example.com/data", "", "noHeaders")
```

**Result:**
```
John | john@example.com | 30
```

**Explanation:** The "noHeaders" option omits the header row, useful when you want data only or are appending to an existing table.

---

### Example 8: Access Array Element by Index
```
=IMPORTJSON("https://api.example.com/items", "/0/name")
```
Where the URL returns `[{"name": "First"}, {"name": "Second"}]`

**Result:**
```
First
```

**Explanation:** Numeric indices access specific array elements. `/0` is the first element, `/1` the second, etc.

---

### Example 9: URL from Cell Reference
```
=IMPORTJSON(A1)
```
Where A1 contains "https://api.example.com/data"

**Result:** (JSON data from the URL in A1)

**Explanation:** The URL can be a cell reference, enabling dynamic API calls based on cell values. Combine with CONCATENATE to build dynamic URLs.

---

### Example 10: Dynamic URL with Parameters
```
=IMPORTJSON("https://api.example.com/search?q=" & A1)
```
Where A1 contains "widgets"

**Result:** Search results for "widgets"

**Explanation:** Concatenate cell values into the URL for dynamic queries. The API receives the search term from the cell.

---

### Example 11: GitHub API - Repository Info
```
=IMPORTJSON("https://api.github.com/repos/microsoft/vscode", "/name,/stargazers_count,/forks_count,/open_issues_count")
```

**Result:**
```
name   | stargazers_count | forks_count | open_issues_count
vscode | 150000           | 25000       | 5000
```

**Explanation:** GitHub's public API returns repository statistics. Specific fields are extracted using the query parameter.

---

### Example 12: Exchange Rates Import
```
=IMPORTJSON("https://api.exchangerate-api.com/v4/latest/USD", "/rates")
```

**Result:**
```
EUR   | GBP   | JPY    | CAD   | ...
0.92  | 0.79  | 149.5  | 1.35  | ...
```

**Explanation:** Currency exchange rate APIs typically nest rates under a "rates" key. The query extracts just the rates object.

---

### Example 13: Handle Nested Arrays
```
=IMPORTJSON("https://api.example.com/orders", "/orders/0/items")
```
Where the response has nested arrays: `{"orders": [{"id": 1, "items": [{"sku": "A1"}, {"sku": "A2"}]}]}`

**Result:**
```
sku
A1
A2
```

**Explanation:** Navigate through nested arrays using path syntax. Here we get items from the first order.

---

### Example 14: Combine with Other Functions
```
=INDEX(IMPORTJSON("https://api.example.com/data"), 2, 1)
```

**Result:** The value in row 2, column 1 of the imported data

**Explanation:** IMPORTJSON returns an array that works with other array functions like INDEX, FILTER, QUERY, etc.

---

### Example 15: Error Handling with IFERROR
```
=IFERROR(IMPORTJSON("https://api.example.com/data"), "API unavailable")
```

**Result:** Imported data, or "API unavailable" if request fails

**Explanation:** Wrap IMPORTJSON in IFERROR to handle network errors, invalid JSON, or API downtime gracefully.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#ERROR!` or `Unknown function` | IMPORTJSON not installed | Add the Apps Script to your spreadsheet via Extensions > Apps Script |
| `#REF!` | Invalid URL or network error | Verify URL is accessible and returns valid JSON; check for typos |
| `Exception: Request failed` | URL not accessible from Google servers | Ensure URL is public; check for firewall/authentication requirements |
| `Timeout exceeded` | API too slow or response too large | Use query parameter to limit data; consider a faster API endpoint |
| `#VALUE!` | Invalid JSON response | Check that URL returns valid JSON, not HTML or error messages |
| `Results don't refresh` | Custom functions cache results | Add a dummy parameter that changes, or use time-based trigger |
| `Authorization required` | API requires authentication | Modify script to include headers, or use authenticated Apps Script |
| `Quota exceeded` | Too many URL fetches | Google limits URL fetch calls; reduce frequency of refreshes |
| `Array result was not expanded` | Single cell can't contain array | Ensure destination cells are empty for array spill |
| `Missing data/columns` | Query path doesn't match JSON structure | Verify JSON structure with browser; adjust query path |

## Use Cases

### [[Financial Data and Market Prices]]
- **Implementation**: Use IMPORTJSON with cryptocurrency APIs (CoinGecko, CoinMarketCap), stock APIs (Alpha Vantage, Yahoo Finance), or forex APIs to import live financial data. Create dashboards with `=IMPORTJSON("api_url", "/price,/volume,/change")`.
- **Business Application**: Build portfolio trackers, price monitoring sheets, and financial dashboards. Create alerts based on price thresholds. Track investment performance over time.
- **Technical Details**: Financial APIs often require API keys; modify script to include authentication headers. Rate limits may restrict refresh frequency. Cache data locally for analysis to avoid quota issues.

### [[Business Intelligence and CRM Integration]]
- **Implementation**: Connect to CRM APIs (HubSpot, Salesforce), project management tools (Asana, Monday.com), or custom business systems via their REST APIs. Extract customer data, sales metrics, or project status.
- **Business Application**: Create real-time sales dashboards, customer analytics reports, and project status overviews without manual data export. Enable cross-system reporting by combining data sources.
- **Technical Details**: Most business APIs require OAuth or API key authentication—modify IMPORTJSON to include authorization headers. Handle pagination for large datasets. Consider data refresh scheduling with triggers.

### [[Weather and Environmental Data]]
- **Implementation**: Fetch weather data from OpenWeatherMap, Climate APIs, or environmental monitoring services. Import forecasts, historical data, or sensor readings using `=IMPORTJSON(weather_api_url, "/main/temp,/weather/0/description")`.
- **Business Application**: Create weather dashboards for logistics planning, agricultural monitoring, or event management. Track environmental conditions for compliance reporting.
- **Technical Details**: Weather APIs typically offer free tiers with rate limits. Structure queries to extract only needed fields to minimize response size. Consider timezone handling for forecast data.

### [[Social Media and Analytics]]
- **Implementation**: Connect to social media APIs (Twitter/X, Instagram Graph API, YouTube Data API) to import engagement metrics, follower counts, and content performance data.
- **Business Application**: Build social media dashboards, track campaign performance, monitor brand mentions, and analyze content engagement across platforms.
- **Technical Details**: Social APIs require OAuth authentication—significant script modification needed. Respect rate limits and terms of service. Consider using aggregator services that provide unified social data.

## Platform Differences

| Feature | Google Sheets (IMPORTJSON) | Microsoft Excel |
|---------|---------------------------|-----------------|
| **Availability** | Custom function via Apps Script | NOT AVAILABLE (use alternatives) |
| **Built-in JSON Support** | No native function; requires script | Power Query has native JSON support |
| **Installation** | Copy script to Apps Script | N/A |
| **Authentication** | Requires script modification | Power Query handles OAuth natively |
| **Refresh Method** | Manual or trigger-based | Power Query refresh button |
| **URL Fetch Limits** | Google quotas apply | No specific limits |
| **Processing Location** | Google's servers | Local machine or Power BI service |

**Excel Alternatives:**

**Power Query (Recommended):**
1. Go to **Data > Get Data > From Other Sources > From Web**
2. Enter the JSON URL
3. Use Power Query Editor to transform JSON structure
4. Load to worksheet

Power Query advantages:
- Native JSON parsing
- Visual transformation editor
- Authentication built-in
- Scheduled refresh in Power BI

**VBA Alternative:**
```vba
Function IMPORTJSON(url As String) As Variant
    ' Requires Microsoft XML library reference
    Dim http As Object, json As Object
    Set http = CreateObject("MSXML2.XMLHTTP")
    http.Open "GET", url, False
    http.Send
    ' Parse JSON and return (requires JSON parser library)
End Function
```

**Cross-Platform Strategy:**
- For simple use cases, pre-convert JSON to CSV using external tools
- Maintain separate data import methods per platform
- Consider cloud-based ETL tools that feed both platforms
- Document platform-specific requirements clearly

## Tips and Best Practices

1. **Install Script First**: IMPORTJSON requires Apps Script installation. Go to Extensions > Apps Script, paste the code, and save before using the function.

2. **Test URLs in Browser First**: Before using in the formula, open the URL in a browser to verify it returns valid JSON and understand the structure.

3. **Use Query Parameters for Large Responses**: Extract only what you need with query paths. Full JSON imports can be slow and hit size limits.

4. **Handle Refresh Carefully**: Custom functions don't auto-refresh. Add a timestamp parameter that changes to force refresh: `=IMPORTJSON(url, query, NOW())` (use sparingly—affects performance).

5. **Wrap in IFERROR**: APIs can fail. Always use `=IFERROR(IMPORTJSON(...), "Error message")` for production sheets.

6. **Respect API Rate Limits**: Don't over-refresh or create many cells calling the same API. You may get blocked or exceed Google quotas.

7. **Build URLs Dynamically**: Concatenate parameters from cells: `=IMPORTJSON("https://api.com/data?id=" & A1)`. This enables flexible, user-controlled queries.

8. **Consider Authentication Needs**: Most useful APIs require keys. Modify the script to include headers, or use services that provide authenticated data proxies.

9. **Cache Data Locally**: For analysis, import data once and paste as values, rather than constantly fetching. This improves performance and reduces API calls.

10. **Document Your APIs**: Add comments noting which APIs are used, any rate limits, required parameters, and expected response structure for future maintenance.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from IMPORTJSON |
|----------|-------------|--------------------------------|
| [[IMPORTDATA]] | Imports CSV/TSV from URL | Built-in but only handles comma/tab-separated data, not JSON |
| [[IMPORTXML]] | Imports XML data with XPath | Built-in, for XML format only, uses XPath queries |
| [[IMPORTHTML]] | Imports tables/lists from HTML | Built-in, for HTML tables, not JSON API data |
| [[IMPORTRANGE]] | Imports from other Sheets | Imports from other spreadsheets, not external APIs |
| [[IMPORTFEED]] | Imports RSS/Atom feeds | Built-in, for RSS/Atom feeds only |

### Commonly Used Together

**[[IFERROR]]** - Handle API failures gracefully

*Provide fallback for failed imports:*
```
=IFERROR(IMPORTJSON(url), "API unavailable")
```
Prevents error display when API is down or URL is invalid.

---

**[[CONCATENATE]]** or **&** - Build dynamic URLs

*Create URLs with cell-based parameters:*
```
=IMPORTJSON("https://api.com/search?q=" & ENCODEURL(A1))
```
Enables dynamic API queries based on user input.

---

**[[INDEX]]** - Extract specific values from imported data

*Get a single value from the result array:*
```
=INDEX(IMPORTJSON(url), 2, 3)
```
Retrieves the value at row 2, column 3 of the imported data.

---

**[[QUERY]]** - Filter and transform imported data

*SQL-like filtering of imported results:*
```
=QUERY(IMPORTJSON(url), "SELECT Col1, Col2 WHERE Col3 > 100")
```
Enables powerful data manipulation on imported JSON.

---

**[[FILTER]]** - Filter rows by condition

*Keep only rows meeting criteria:*
```
=FILTER(IMPORTJSON(url), IMPORTJSON(url, "/active") = TRUE)
```
Filters imported data to rows where active is TRUE.

---

**[[ENCODEURL]]** - Encode URL parameters

*Safely include special characters in URLs:*
```
=IMPORTJSON("https://api.com/search?q=" & ENCODEURL(A1))
```
Properly encodes spaces and special characters in query parameters.

## Resources and Installation

### Popular IMPORTJSON Scripts

**Brad Jasper / Trevor Lohrbeer Version (Most Common):**
- GitHub: https://github.com/bradjasper/ImportJSON
- GitHub (Lohrbeer Fork): https://github.com/fastfedora/google-sheets-importjson

**Installation Steps:**
1. Open your Google Sheets document
2. Go to **Extensions > Apps Script**
3. Delete the default `myFunction` code
4. Visit the GitHub repository above
5. Copy the entire ImportJSON.gs file content
6. Paste into the Apps Script editor
7. **File > Save** (or Ctrl+S)
8. Close the Apps Script editor
9. Wait a few seconds, then use `=IMPORTJSON()` in your sheet

**First-Time Authorization:**
- The first use may prompt for authorization
- Review and accept the permissions (URL fetch capability)
- The function will then work for all users of that specific document

### Alternative Approaches

**Google Sheets' Built-in IMPORTDATA (for CSV):**
If your API can return CSV format, use the built-in function:
```
=IMPORTDATA("https://api.example.com/data?format=csv")
```

**Apps Script for Complex Scenarios:**
For authenticated APIs or complex processing, consider writing a custom Apps Script that runs on a schedule and writes data directly to sheets rather than using a custom function.

## Official Documentation

- **GitHub (Original)**: [bradjasper/ImportJSON](https://github.com/bradjasper/ImportJSON)
- **GitHub (Enhanced Fork)**: [fastfedora/google-sheets-importjson](https://github.com/fastfedora/google-sheets-importjson)
- **Google Apps Script**: [UrlFetchApp Documentation](https://developers.google.com/apps-script/reference/url-fetch/url-fetch-app)
- **Google Sheets Custom Functions**: [Custom Functions in Google Sheets](https://developers.google.com/apps-script/guides/sheets/functions)

## Version History

| Version/Source | Date | Features |
|----------------|------|----------|
| Brad Jasper Original | 2012 | Basic JSON import functionality |
| Trevor Lohrbeer Fork | 2013 | Added query, options, improved parsing |
| Community Updates | 2015-2020 | Bug fixes, performance improvements |
| Various Forks | 2020+ | Authentication support, advanced features |
| Google Sheets (Native) | N/A | No native IMPORTJSON; IMPORTDATA for CSV only |
| Microsoft Excel | N/A | No equivalent; use Power Query |

**Note:** As a community-developed script, IMPORTJSON has many variants. The behavior may differ between versions. Always verify the script source and test thoroughly before production use.

---

*Last updated: 2026-01-10*
