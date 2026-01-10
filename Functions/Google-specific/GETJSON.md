---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - web-functions
  - data-import
subTopics:
  - json-parsing
  - api-integration
  - external-data
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - get json
  - json parser
  - json extract
tags:
  - google-sheets-only
  - json
  - web-data
  - api
  - data-extraction
---

# GETJSON

## Description

**GETJSON** is a Google Sheets function designed to extract and parse data from JSON (JavaScript Object Notation) strings, enabling you to work with structured data that is commonly returned by web APIs and data services. JSON has become the de facto standard for data interchange on the web, and GETJSON bridges the gap between raw JSON data and spreadsheet-friendly tabular format. Given a JSON string and a path expression, GETJSON navigates the JSON structure and returns the value at the specified location. This makes it possible to pull specific data points from complex, nested JSON structures directly into your spreadsheet cells without requiring external scripts or add-ons.

The primary use cases for GETJSON involve working with data imported from web services, APIs, or other sources that deliver JSON-formatted responses. For example, if you use IMPORTDATA or a custom Apps Script to fetch JSON from a REST API, GETJSON allows you to extract specific fields from that response. You might extract the "temperature" value from a weather API response, pull "price" from a product data feed, or navigate nested structures to retrieve deeply embedded information. GETJSON uses JSONPath-like syntax to specify the location of data within the JSON structure, supporting both dot notation (object.property) and bracket notation (array[0]) for navigating objects and arrays.

There are several important gotchas to understand when working with GETJSON. First, GETJSON operates on JSON strings, not live web connections; you must first obtain the JSON data as text (via IMPORTDATA, custom function, or direct paste) before GETJSON can parse it. Second, the path syntax must exactly match the JSON structure, including case sensitivity; a typo in property names will return errors or empty results. Third, JSON arrays are zero-indexed, meaning the first element is accessed with [0], not [1]. Fourth, complex nested structures require careful path construction, and deeply nested data may require multiple GETJSON calls or creative path expressions. Fifth, large JSON responses can cause performance issues or exceed cell content limits; consider extracting only the specific data you need rather than entire response bodies.

From a platform perspective, GETJSON is exclusive to Google Sheets and is not available in Microsoft Excel. Excel users working with JSON data typically use Power Query (Get & Transform Data) to parse JSON files, or they use VBA scripts or custom add-ins. The GETJSON function in Sheets provides a formula-based alternative that integrates naturally with other spreadsheet functions. If you are building spreadsheets that must work in both platforms, you will need to implement Excel-specific solutions for JSON handling, as there is no direct equivalent function.

## Syntax

> [!f(x)] GETJSON Syntax
>
> ```
> =GETJSON(json_string, path)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `json_string` | Yes | A text string containing valid JSON data, or a cell reference to a cell containing JSON. The JSON must be properly formatted with correct syntax (matching braces, quoted property names, etc.). Can also be the output of functions like IMPORTDATA that return JSON text. |
| `path` | Yes | A text string specifying the location of the data to extract within the JSON structure. Uses dot notation for object properties (e.g., "data.items") and bracket notation for array indices (e.g., "items[0]"). The path is case-sensitive and must match the JSON property names exactly. |

### Return Value

Returns the value at the specified path within the JSON structure. The return type depends on what exists at that path:
- String values are returned as text
- Numeric values are returned as numbers
- Boolean values are returned as TRUE or FALSE
- Null values are returned as empty
- Nested objects or arrays may be returned as JSON strings for further parsing
- If the path does not exist, returns an error

## Examples

> [!f(x)] GETJSON Examples

### Example 1: Extract Simple Property
```
=GETJSON(A1, "name")
```
Where A1 contains: `{"name": "John", "age": 30}`

**Result:** John

**Explanation:** Extracts the top-level "name" property from a simple JSON object. The path "name" directly references the property key.

---

### Example 2: Extract Numeric Value
```
=GETJSON(A1, "age")
```
Where A1 contains: `{"name": "John", "age": 30}`

**Result:** 30

**Explanation:** Numeric values are returned as numbers that can be used in calculations. GETJSON automatically handles type conversion.

---

### Example 3: Navigate Nested Objects
```
=GETJSON(A1, "address.city")
```
Where A1 contains: `{"name": "John", "address": {"city": "New York", "zip": "10001"}}`

**Result:** New York

**Explanation:** Dot notation navigates through nested objects. The path "address.city" first accesses the "address" object, then the "city" property within it.

---

### Example 4: Access Array Element by Index
```
=GETJSON(A1, "items[0]")
```
Where A1 contains: `{"items": ["apple", "banana", "cherry"]}`

**Result:** apple

**Explanation:** Bracket notation with zero-based indexing accesses array elements. [0] is the first element, [1] is the second, etc.

---

### Example 5: Access Property of Array Element
```
=GETJSON(A1, "products[0].name")
```
Where A1 contains: `{"products": [{"name": "Widget", "price": 9.99}, {"name": "Gadget", "price": 19.99}]}`

**Result:** Widget

**Explanation:** Combines bracket and dot notation to navigate an array of objects. This gets the "name" property of the first product in the array.

---

### Example 6: Access Last Array Element
```
=GETJSON(A1, "items[2]")
```
Where A1 contains: `{"items": ["first", "second", "third"]}`

**Result:** third

**Explanation:** To access the last element, you need to know the array length. Arrays are zero-indexed, so index 2 accesses the third (last) element in a 3-item array.

---

### Example 7: Extract Deeply Nested Value
```
=GETJSON(A1, "response.data.results[0].value")
```
Where A1 contains: `{"response": {"data": {"results": [{"value": 42}]}}}`

**Result:** 42

**Explanation:** Multiple levels of nesting are navigated by chaining dot and bracket notation. This path goes four levels deep into the JSON structure.

---

### Example 8: Extract Boolean Value
```
=GETJSON(A1, "active")
```
Where A1 contains: `{"name": "Account", "active": true}`

**Result:** TRUE

**Explanation:** JSON boolean values (true/false) are converted to spreadsheet TRUE/FALSE values that can be used in logical formulas.

---

### Example 9: Combined with IMPORTDATA
```
=GETJSON(IMPORTDATA("https://api.example.com/data.json"), "temperature")
```

**Result:** (The temperature value from the API response)

**Explanation:** GETJSON can parse the output of IMPORTDATA when the imported data is a JSON string. This enables direct API integration without intermediate steps.

---

### Example 10: Extract Multiple Values with Array Formula
```
={GETJSON(A1, "products[0].name"), GETJSON(A1, "products[0].price")}
```
Where A1 contains product JSON

**Result:** Two-column output with product name and price

**Explanation:** Use array formulas or HSTACK to extract multiple values from the same JSON into adjacent cells. Each GETJSON call extracts one value.

---

### Example 11: Handle Missing Properties with IFERROR
```
=IFERROR(GETJSON(A1, "optional.field"), "Not found")
```

**Result:** Returns the field value if it exists, or "Not found" if the path is invalid

**Explanation:** IFERROR wrapping handles cases where the JSON structure varies or properties may be missing. Essential for robust data extraction from inconsistent sources.

---

### Example 12: Iterate Through Array with ROW
```
=GETJSON($A$1, "items["&ROW()-2&"]")
```
Placed in rows starting from row 2

**Result:** Each row extracts the corresponding array element (row 2 gets items[0], row 3 gets items[1], etc.)

**Explanation:** Dynamic path construction using ROW() enables extracting array elements across multiple rows. Adjust the offset (ROW()-2) based on your starting row.

---

### Example 13: Extract Nested Array
```
=GETJSON(A1, "matrix[1][2]")
```
Where A1 contains: `{"matrix": [[1,2,3], [4,5,6], [7,8,9]]}`

**Result:** 6

**Explanation:** Multi-dimensional arrays use chained bracket notation. This accesses the second row (index 1), third column (index 2) of a matrix.

---

### Example 14: Property Names with Special Characters
```
=GETJSON(A1, "['first-name']")
```
Where A1 contains: `{"first-name": "John"}`

**Result:** John

**Explanation:** Property names containing special characters (hyphens, spaces) require bracket notation with quotes. Standard dot notation only works for simple alphanumeric names.

---

### Example 15: Combined with QUERY for Filtering
```
=QUERY({GETJSON(A1,"data[0].id"), GETJSON(A1,"data[0].value"); GETJSON(A1,"data[1].id"), GETJSON(A1,"data[1].value")}, "SELECT * WHERE Col2 > 10")
```

**Result:** Filtered results from extracted JSON data

**Explanation:** Extract multiple values into an array structure, then use QUERY to filter or manipulate the results. This enables SQL-like operations on JSON data.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid JSON syntax in the input string | Verify JSON is properly formatted: matching braces/brackets, quoted property names, correct comma usage. Use a JSON validator to check syntax. |
| `#ERROR!` | Path does not exist in the JSON structure | Check path spelling and case sensitivity. Verify the JSON structure matches your path. Use console or JSON viewer to explore structure. |
| `#N/A` | Property is null or undefined in JSON | Handle with IFERROR for graceful fallback. Check if the property exists before accessing. |
| `#REF!` | Array index out of bounds | Verify array length before accessing specific indices. Use negative indices or length-based access if supported. |
| `Formula parse error` | Path string contains unescaped special characters | Ensure path string is properly quoted. Escape internal quotes if needed. Use bracket notation for special property names. |
| Empty result | Property exists but has empty/null value | This is expected behavior for null JSON values. Check if you need to handle nulls differently in your logic. |
| Performance issues | Very large JSON string exceeds cell limits | Extract only needed portions. Consider splitting large JSON across multiple cells or using Apps Script for preprocessing. |
| Type mismatch | Expected number but got string (or vice versa) | JSON may store numbers as strings. Use VALUE() to convert text numbers. Check source data formatting. |

## Use Cases

### [[API Data Integration: Weather Dashboard]]

**Scenario:** A facilities team wants to display current weather conditions for multiple office locations by pulling data from a weather API that returns JSON responses.

**Implementation:**
```
=GETJSON(IMPORTDATA(WeatherAPIURL), "current.temp_c")
=GETJSON(IMPORTDATA(WeatherAPIURL), "current.condition.text")
=GETJSON(IMPORTDATA(WeatherAPIURL), "location.name")
```

**Business Application:** Creates a live weather dashboard that updates automatically. Each location's weather API URL is stored in a cell, and GETJSON extracts temperature, conditions, and location name. Facilities can monitor conditions across sites without leaving their spreadsheet, useful for HVAC planning and weather-sensitive operations.

**Technical Details:** IMPORTDATA fetches the JSON response from the API URL. GETJSON then navigates the response structure to extract specific values. API rate limits and refresh intervals should be considered; use IMPORTDATA's caching behavior or schedule refreshes appropriately.

---

### [[E-commerce: Product Data Feed Processing]]

**Scenario:** An e-commerce team receives product data from suppliers in JSON format and needs to extract product details into a structured spreadsheet for inventory management.

**Implementation:**
```
=GETJSON(A2, "sku")
=GETJSON(A2, "title")
=GETJSON(A2, "price.regular")
=GETJSON(A2, "price.sale")
=GETJSON(A2, "inventory.quantity")
```
Where column A contains JSON product records

**Business Application:** Transforms JSON product feeds into tabular data for analysis, pricing decisions, and inventory tracking. Teams can apply spreadsheet functions (SUMIF, VLOOKUP, filters) to the extracted data. Changes in the JSON feed are automatically reflected in the spreadsheet.

**Technical Details:** Each row processes one JSON product record. If the JSON contains an array of products, first extract individual records or use array indexing. Consider using named ranges for the JSON column and helper functions for complex nested extractions.

---

### [[Financial Services: Stock Quote Parsing]]

**Scenario:** A financial analyst fetches stock quote data from a financial data API and needs to extract key metrics (price, change, volume, market cap) for portfolio analysis.

**Implementation:**
```
=GETJSON(A2, "quote.latestPrice")
=GETJSON(A2, "quote.change")
=GETJSON(A2, "quote.changePercent")
=GETJSON(A2, "quote.volume")
=GETJSON(A2, "quote.marketCap")
```

**Business Application:** Creates a real-time stock tracking sheet that pulls live data from financial APIs. Analysts can calculate portfolio value, track daily changes, and monitor position sizes. The spreadsheet becomes a dynamic dashboard updated with each API refresh.

**Technical Details:** Financial APIs often have usage limits; implement caching strategies or use scheduled refreshes. Consider using IFERROR to handle market-closed periods or data unavailability. Format extracted values appropriately (currency, percentages, large numbers).

---

### [[IoT/Sensor Data: Device Status Monitoring]]

**Scenario:** An operations team monitors IoT sensors that report status via JSON-formatted messages. They need to extract sensor readings, status flags, and timestamps into a monitoring dashboard.

**Implementation:**
```
=GETJSON(SensorData, "deviceId")
=GETJSON(SensorData, "readings.temperature")
=GETJSON(SensorData, "readings.humidity")
=GETJSON(SensorData, "status.online")
=GETJSON(SensorData, "lastUpdated")
```

**Business Application:** Centralizes sensor data from multiple devices into a single monitoring view. Operations can quickly identify offline sensors, abnormal readings, and update delays. Combined with conditional formatting, this creates an effective status dashboard.

**Technical Details:** Timestamps may be in Unix epoch or ISO format; use additional functions to convert to readable dates. Boolean status values can drive conditional formatting. Consider using Apps Script for continuous data collection if Sheets refresh intervals are insufficient.

## Platform Differences

### Microsoft Excel

- **Availability:** GETJSON does not exist in Excel
- **Alternatives:**
  - **Power Query:** Use Get Data > From JSON to import and transform JSON files or web API responses
  - **VBA:** Write custom functions using the ScriptControl object or third-party JSON parsers
  - **Office Scripts:** Excel Online supports JavaScript-based scripts that can parse JSON
  - **Add-ins:** Third-party add-ins like Power Tools or custom solutions can provide JSON parsing
- **Power Query Approach:** Power Query is generally the recommended approach for JSON in Excel, offering a visual interface for navigating and extracting JSON data

### Google Sheets

- **Availability:** GETJSON is available in Google Sheets (newer function, may not be in all documentation)
- **Note:** This function may be relatively new and documentation may be limited
- **Alternative:** If GETJSON is unavailable, use Apps Script with JSON.parse() for custom JSON parsing
- **Integration:** Works with IMPORTDATA, IMPORTXML, and custom functions that return JSON strings

### Key Differences

| Feature | Google Sheets | Excel |
|---------|---------------|-------|
| Native JSON function | GETJSON (if available) | None |
| Recommended approach | GETJSON or Apps Script | Power Query |
| Learning curve | Formula-based, familiar | Requires Power Query knowledge |
| Dynamic refresh | Via IMPORTDATA functions | Via Power Query refresh |
| Complex transformations | Limited, use Apps Script | Power Query M language |

## Tips and Best Practices

1. **Validate JSON Before Parsing:** Before building complex GETJSON formulas, paste your JSON into a validator (like jsonlint.com) to ensure proper syntax. Many extraction errors stem from malformed JSON.

2. **Use a JSON Viewer to Explore Structure:** Before writing paths, visualize the JSON structure using a JSON viewer or formatter. This helps you understand nesting levels and correct property names.

3. **Build Paths Incrementally:** Start with simple paths (top-level properties) and gradually add depth. Test each level before proceeding deeper into the structure.

4. **Wrap in IFERROR for Robustness:** JSON from external sources may vary in structure. Always use IFERROR to handle missing properties gracefully: `=IFERROR(GETJSON(...), "N/A")`.

5. **Use Named Ranges for JSON Cells:** When a JSON string is used in multiple GETJSON calls, store it in a named range. This makes formulas more readable and centralizes the data source.

6. **Consider Performance with Large JSON:** Very large JSON strings can slow calculations. If possible, preprocess or filter the JSON to include only needed data before importing into Sheets.

7. **Document Your Paths:** Keep a reference of the JSON structure and paths you are using. When APIs change, this documentation helps identify what needs updating.

8. **Handle Arrays Systematically:** When extracting from arrays, consider using helper columns with row numbers to dynamically construct paths, or use ARRAYFORMULA patterns for bulk extraction.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[IMPORTDATA]] | Imports data from a URL as text | Use to fetch JSON from web APIs before parsing with GETJSON |
| [[IMPORTXML]] | Imports data from XML sources using XPath | When data source is XML rather than JSON |
| [[REGEXEXTRACT]] | Extracts text using regular expressions | For simple JSON patterns where GETJSON is unavailable or overkill |
| [[SPLIT]] | Splits text by delimiter | For very simple delimited data that happens to be in JSON format |

### Commonly Used Together

**[[IMPORTDATA]]** - Fetch JSON from web URLs

*Import JSON from API, then parse with GETJSON:*
```
=GETJSON(IMPORTDATA("https://api.example.com/data.json"), "result.value")
```
IMPORTDATA retrieves the raw JSON text, GETJSON extracts specific values.

---

**[[IFERROR]]** - Handle missing or invalid paths

*Graceful error handling for variable JSON structures:*
```
=IFERROR(GETJSON(A1, "optional.field"), "Not available")
```
Returns a default value when the path does not exist in the JSON.

---

**[[CONCATENATE]] / Ampersand** - Build dynamic paths

*Construct paths using cell values:*
```
=GETJSON(A1, "items["&B1&"].name")
```
Where B1 contains the index number. Enables dynamic array element access.

---

**[[ARRAYFORMULA]]** - Extract multiple array elements

*Extract array elements across rows:*
```
=ARRAYFORMULA(GETJSON($A$1, "items["&ROW(A1:A10)-1&"].value"))
```
Extracts values from first 10 array elements into 10 rows.

---

**[[QUERY]]** - Filter and analyze extracted data

*SQL-like operations on extracted JSON data:*
```
=QUERY({GETJSON(A1,"[0].id"), GETJSON(A1,"[0].value")}, "SELECT * WHERE Col2 > 100")
```
Extract data into an array, then filter with QUERY.

## Official Documentation

- **Google Sheets:** GETJSON documentation may be limited as this is a newer function. Check [Google Sheets function list](https://support.google.com/docs/table/25273) for current availability.
- **Apps Script Alternative:** [JSON.parse() in Apps Script](https://developers.google.com/apps-script/guides/services/advanced) for custom JSON parsing

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | 2024-2025 (estimated) | Newer function for JSON parsing; availability may vary |
| Excel | N/A | Not available; use Power Query for JSON handling |

---

*Last updated: 2026-01-10*
