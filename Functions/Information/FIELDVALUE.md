---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- information
subTopics:
- linked-data-types
- data-extraction
- stocks-geography
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Field Value
- Data Type Field
- Linked Data Extraction
tags:
- information
- linked-data
- stocks
- geography
- data-types
- excel-365
---

# FIELDVALUE

## Description

**FIELDVALUE** extracts specific properties from Linked Data Types in Excel, such as stock tickers or geographic locations. When you convert a cell containing "MSFT" to a Stock data type or "France" to a Geography data type, that cell becomes a rich data object with dozens of underlying properties (price, market cap, population, capital city, etc.). FIELDVALUE is the programmatic way to pull out any of those properties.

Think of Linked Data Types as smart cells that know things. A cell showing "Microsoft Corporation" after being converted to a Stock type secretly contains price, CEO name, employees, revenue, and much more. You can click the cell icon to see a card of data, but FIELDVALUE lets you extract any single property into a formula. This is essential for building dynamic dashboards, comparison tables, and automated reports based on live data.

**Important context:** FIELDVALUE is specifically designed for Linked Data Types, which are an Excel 365 (and Excel for web) feature requiring internet connectivity. The data comes from Microsoft's data providers (Refinitiv for stocks, Bing for geography, etc.) and updates periodically. Without a Microsoft 365 subscription and internet access, Linked Data Types don't work, and neither does FIELDVALUE.

**The dot notation alternative:** Excel provides a shorthand syntax that often replaces FIELDVALUE. If A1 contains a Stock data type, you can write `=A1.Price` instead of `=FIELDVALUE(A1,"Price")`. Both work identically, but FIELDVALUE is more explicit and can be easier to troubleshoot. The dot notation is quicker for simple cases; FIELDVALUE is better when field names come from other cells or when building dynamic formulas.

## Syntax

> [!f(x)] FIELDVALUE Syntax
>
> ```
> =FIELDVALUE(value, field_name)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | A cell reference containing a Linked Data Type (Stock, Geography, Currency, etc.), or an expression that returns a Linked Data Type. Cannot be plain text—the cell must have been converted to a data type. |
| `field_name` | Yes | A text string specifying which property to extract. Must exactly match an available field name for that data type. Case-insensitive but spelling-sensitive. Examples: "Price", "Market cap", "Population", "Capital". |

### Available Field Names (Common Examples)

**Stock Data Type:**
| Field Name | Description |
|------------|-------------|
| `"Price"` | Current stock price |
| `"Price change"` | Change since previous close |
| `"Change (%)"` | Percentage change |
| `"Market cap"` | Total market capitalization |
| `"52 week high"` | Highest price in past year |
| `"52 week low"` | Lowest price in past year |
| `"Volume"` | Trading volume |
| `"PE ratio"` | Price to earnings ratio |
| `"Employees"` | Number of employees |
| `"Headquarters"` | Company headquarters location |
| `"CEO"` | Chief Executive Officer name |
| `"Description"` | Company description |

**Geography Data Type:**
| Field Name | Description |
|------------|-------------|
| `"Population"` | Total population |
| `"Capital"` | Capital city |
| `"Area"` | Land area (sq km) |
| `"Largest city"` | Largest city by population |
| `"Official language"` | Primary language |
| `"Currency code"` | Currency code (USD, EUR, etc.) |
| `"Time zone"` | Primary time zone |
| `"Leader"` | Head of state/government |
| `"Latitude"` | Latitude coordinate |
| `"Longitude"` | Longitude coordinate |

### Return Value

Returns the value of the specified field. The type depends on the field: numeric for Price, Market cap, Population; text for Name, CEO, Capital; date for certain historical fields. Returns #FIELD! error if the field name doesn't exist or data isn't available.

## Examples

> [!f(x)] FIELDVALUE Examples

### Example 1: Get Current Stock Price
```
=FIELDVALUE(A1, "Price")
```
Where A1 contains a Stock data type for "AAPL"
**Result:** 185.42 (current Apple stock price)

**Explanation:** The most common FIELDVALUE use case. Extracts the current price from a stock data type. This value updates when Excel refreshes the linked data (manually or automatically based on settings).

---

### Example 2: Get Market Capitalization
```
=FIELDVALUE(A1, "Market cap")
```
Where A1 contains a Stock data type for "MSFT"
**Result:** 2850000000000 (approximately 2.85 trillion)

**Explanation:** Returns the total market capitalization as a raw number. Format the cell as currency with custom formatting to display "2.85T" or similar. Market cap = share price x shares outstanding.

---

### Example 3: Get Country Population
```
=FIELDVALUE(B2, "Population")
```
Where B2 contains a Geography data type for "Japan"
**Result:** 125700000 (approximately 125.7 million)

**Explanation:** Geography data types contain demographic and geographic data. Population returns the most recent census or estimate available in Microsoft's data sources.

---

### Example 4: Get Country Capital City
```
=FIELDVALUE(B2, "Capital")
```
Where B2 contains a Geography data type for "Australia"
**Result:** "Canberra"

**Explanation:** Text fields return strings directly. Note that some countries have multiple capitals (South Africa, for example)—Excel typically returns the primary or administrative capital.

---

### Example 5: Using Dot Notation Shorthand
```
=A1.Price
```
Equivalent to: `=FIELDVALUE(A1, "Price")`
**Result:** Same as FIELDVALUE—current stock price

**Explanation:** Excel's dot notation provides a cleaner syntax for simple field extraction. Type the cell reference, a period, and the field name. Autocomplete shows available fields as you type.

---

### Example 6: Dynamic Field Name from Cell
```
=FIELDVALUE(A1, B1)
```
Where A1 = Stock data type, B1 = "PE ratio"
**Result:** 32.5 (or whatever the P/E ratio is)

**Explanation:** Unlike dot notation, FIELDVALUE accepts cell references for the field name. This enables dynamic dashboards where users select which metric to display from a dropdown list.

---

### Example 7: Build a Stock Comparison Table
```
=FIELDVALUE($A2, B$1)
```
Where column A has stock tickers as data types, row 1 has field names
**Result:** Comparison matrix of multiple stocks and metrics

**Explanation:** The classic FIELDVALUE pattern. Lock the row reference for field names (B$1), lock the column reference for stocks ($A2), then drag the formula to fill a grid. Instantly compares Price, PE ratio, Market cap across stocks.

---

### Example 8: Calculate Stock Price Change
```
=FIELDVALUE(A1, "Price") - FIELDVALUE(A1, "Previous close")
```
**Result:** Today's price change in dollars

**Explanation:** Combine multiple FIELDVALUE calls in calculations. This calculates the same result as the "Price change" field but shows how to build custom calculations from raw data.

---

### Example 9: Get Company Headquarters with Error Handling
```
=IFERROR(FIELDVALUE(A1, "Headquarters"), "Not available")
```
**Result:** Headquarters location or "Not available" if field doesn't exist

**Explanation:** Not all data types have all fields. Wrap FIELDVALUE in IFERROR to handle missing data gracefully. Essential for robust dashboards that might encounter incomplete records.

---

### Example 10: Extract Geographic Coordinates
```
=FIELDVALUE(A1, "Latitude") & ", " & FIELDVALUE(A1, "Longitude")
```
Where A1 contains a Geography data type
**Result:** "35.6762, 139.6503" (for Tokyo)

**Explanation:** Combine latitude and longitude into a single coordinate string. Useful for mapping applications or calculating distances between locations.

---

### Example 11: Array Formula for Multiple Fields
```
=FIELDVALUE(A1, {"Price", "Volume", "Market cap"})
```
**Result:** Horizontal array with three values

**Explanation:** In Excel 365, FIELDVALUE can accept an array of field names and return multiple values at once. Spills across multiple cells automatically. Efficient way to extract several fields simultaneously.

---

### Example 12: Nested with VLOOKUP for Ticker Lookup
```
=FIELDVALUE(VLOOKUP(E1, A:B, 2, FALSE), "Price")
```
Where E1 contains company name, A:B maps names to stock data types
**Result:** Price for the looked-up stock

**Explanation:** FIELDVALUE works on any expression returning a Linked Data Type, not just direct cell references. Here, VLOOKUP finds the right stock data type, then FIELDVALUE extracts its price.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#FIELD!` | Field name doesn't exist for this data type | Check available fields by clicking the data type icon. Verify spelling exactly matches. |
| `#VALUE!` | Reference isn't a Linked Data Type | Ensure the cell has been converted to a Stock/Geography/etc. data type. Plain text won't work. |
| `#CALC!` | Data refresh failed or connectivity issue | Check internet connection. Try Data > Refresh All to force data update. |
| `#N/A` | Data not available for this entity | Some companies or locations have incomplete data. Check the data card for available fields. |
| `#NAME?` | Using dot notation with spaces in field name | Use FIELDVALUE for multi-word field names: `=FIELDVALUE(A1,"Market cap")` not `=A1.Market cap`. |
| `#BUSY!` | Data is still loading | Wait for data refresh to complete. Excel shows spinning icon while loading. |

## Use Cases

### [[Stock Portfolio Dashboard]]

**Scenario:** Create a live dashboard showing current prices, changes, and key metrics for a portfolio of stocks.

**Implementation:**
```
Column A: Stock symbols converted to Stock data type
=FIELDVALUE($A2, "Name") in column B
=FIELDVALUE($A2, "Price") in column C
=FIELDVALUE($A2, "Change (%)") in column D
=FIELDVALUE($A2, "Market cap") in column E
```

**Business Application:** Real-time portfolio monitoring without leaving Excel. Investment analysts, financial advisors, and personal investors can track holdings with automatically updating data.

**Technical Details:** Set up Data > Queries & Connections > Properties to control refresh frequency. Consider using conditional formatting on Change (%) to highlight gains (green) and losses (red).

---

### [[Country Comparison Analysis]]

**Scenario:** Compare economic and demographic indicators across multiple countries for research or reporting.

**Implementation:**
```
Column A: Country names converted to Geography data type
=FIELDVALUE($A2, "Population")
=FIELDVALUE($A2, "GDP")
=FIELDVALUE($A2, "Capital")
=FIELDVALUE($A2, "Area")
```

**Business Application:** Market research, academic studies, policy analysis, and business expansion planning. Quickly gather standardized data across countries without manual research.

**Technical Details:** Data accuracy varies by country and metric. Major nations have comprehensive data; smaller territories may have gaps. Always verify critical data from primary sources.

---

### [[Dynamic Stock Metric Selector]]

**Scenario:** Let users choose which stock metric to display from a dropdown, then show that metric for all stocks in a list.

**Implementation:**
```
Cell E1: Data validation dropdown with: Price, PE ratio, Market cap, Volume
Formula: =FIELDVALUE(A2, $E$1)
```
Drag formula down for all stocks in column A.

**Business Application:** Interactive dashboards where users explore different metrics without needing multiple static columns. Keeps dashboards clean and focused.

**Technical Details:** Create the dropdown using Data > Data Validation > List. Field names must match exactly (case-insensitive but spelling-sensitive). Consider adding a list of valid field names in a hidden sheet for reference.

---

### [[Currency Exchange Integration]]

**Scenario:** Combine stock prices with currency data types for international portfolio valuation.

**Implementation:**
```
A1: Stock data type (trades in foreign currency)
B1: Currency data type (e.g., EUR/USD)
=FIELDVALUE(A1, "Price") * FIELDVALUE(B1, "Price")
```

**Business Application:** Accurate portfolio valuation for international holdings. Financial reporting that needs consistent currency translation.

**Technical Details:** Currency data types provide exchange rates. Note that timing differences may occur—stock price might be from last close while currency is more recent. Verify your valuation timing requirements.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365 (Microsoft 365 subscription), Excel for the web
- **Requires:** Internet connectivity for Linked Data Types
- **Data sources:** Refinitiv for stocks, Bing for geography, Wolfram Alpha for other data
- **Refresh options:** Manual, on file open, or automatic intervals
- **Dot notation:** Fully supported as shorthand syntax
- **Array support:** Can accept array of field names in Excel 365

### Google Sheets

- **Not available:** Google Sheets does not have Linked Data Types or FIELDVALUE
- **Alternative:** Use GOOGLEFINANCE for stock data (different syntax and capabilities)
- **Geography data:** No equivalent; requires manual data entry or third-party add-ons

### Key Difference Alert

FIELDVALUE is entirely an Excel feature with no Google Sheets equivalent. Google Sheets users can use GOOGLEFINANCE for basic stock data (`=GOOGLEFINANCE("AAPL","price")`) but it lacks the rich entity model and extensive field options of Excel's Linked Data Types. For geography data, Google Sheets has no built-in solution.

## Tips and Best Practices

1. **Convert to data types first:** FIELDVALUE only works on Linked Data Types. Select cells with stock symbols or location names, then Data > Stocks or Data > Geography to convert them.

2. **Use dot notation for simple cases:** `=A1.Price` is cleaner than `=FIELDVALUE(A1,"Price")`. Save FIELDVALUE for when field names come from cells or contain spaces.

3. **Check available fields via data card:** Click the icon that appears when you select a data type cell to see all available fields. Not all entities have all fields.

4. **Handle errors gracefully:** Wrap FIELDVALUE in IFERROR for production dashboards. Data might be missing, connectivity might fail, or symbols might be unrecognized.

5. **Control refresh frequency:** Right-click a data type and select Data Type > Refresh Settings to control how often data updates. Frequent refreshes can slow large workbooks.

6. **Use absolute references in comparison tables:** Lock row references for headers ($A2) and column references for field names (B$1) to enable dragging formulas across grids.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CELL]] | Returns cell properties (formatting, location) | When you need cell metadata, not external data type fields |
| [[TYPE]] | Returns numeric code for value type | When checking basic data types (number, text, etc.) |
| [[INDIRECT]] | Returns reference from text string | When building dynamic references, not extracting data type fields |
| [[GOOGLEFINANCE]] | Google Sheets stock data function | When working in Google Sheets (different syntax/capabilities) |

### Commonly Used Together

**[[VLOOKUP]] / [[XLOOKUP]]** - Find data type cells dynamically

*Combined with FIELDVALUE for lookup-based extraction:*
```
=FIELDVALUE(XLOOKUP(E1, CompanyNames, StockDataTypes), "Price")
```
Look up a company name, get its data type, extract the price.

---

**[[IFERROR]]** - Handle missing data

*Combined with FIELDVALUE for robust formulas:*
```
=IFERROR(FIELDVALUE(A1, "Employees"), "N/A")
```
Provides fallback when field data isn't available.

---

**[[IF]]** - Conditional field extraction

*Combined with FIELDVALUE for conditional logic:*
```
=IF(FIELDVALUE(A1,"Change (%)")>0, "Up", "Down")
```
Classify stocks based on field values.

---

**[[TEXT]]** - Format extracted values

*Combined with FIELDVALUE for display formatting:*
```
=TEXT(FIELDVALUE(A1,"Market cap"), "$#,##0,,\"M\"")
```
Format market cap as "$2,850M" for readability.

---

**[[STOCKHISTORY]]** - Historical stock data

*Complement to FIELDVALUE for time-series analysis:*
```
=STOCKHISTORY(A1, DATE(2024,1,1), DATE(2024,12,31))
```
Get historical prices while FIELDVALUE provides current snapshot.

## Official Documentation

- **Microsoft Excel:** [FIELDVALUE function](https://support.microsoft.com/en-us/office/fieldvalue-function-ce3d8741-71d3-4b8e-a86f-25a8a5d4c9e1)
- **Microsoft Data Types:** [Linked data types overview](https://support.microsoft.com/en-us/office/excel-data-types-stocks-and-geography-61a33056-9935-484f-8ac8-f1a89e210877)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2018 (with Linked Data Types) | Introduced alongside Stock and Geography data types |
| Excel 2021 | Included | Available in perpetual license version |
| Excel for web | 2018+ | Full support in browser version |
| Google Sheets | Not available | No equivalent function or feature |

---

*Last updated: 2026-01-10*
