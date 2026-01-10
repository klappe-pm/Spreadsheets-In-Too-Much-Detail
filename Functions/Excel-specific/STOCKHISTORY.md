---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- financial-functions
- data-types
subTopics:
- stock-data
- market-data
- historical-prices
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Stock History
- Historical Stock Prices
- Market Data
tags:
- excel-only
- financial
- stocks
- market-data
- data-types
---

# STOCKHISTORY

## Description

**STOCKHISTORY** retrieves historical stock price data from Microsoft's financial data service, returning an array of prices (and optionally volumes) for a specified date range. This enables direct access to market data within Excel without external data connections, web scraping, or third-party add-ins. Want 30 days of Apple stock prices? STOCKHISTORY delivers them in a dynamic array.

The function connects to Microsoft's financial data providers (typically Refinitiv) and pulls historical data including open, high, low, close prices, and trading volume. Results update when the workbook recalculates, though there's typically a 15-20 minute delay from real-time (not suitable for live trading).

**Important limitations:** STOCKHISTORY requires a Microsoft 365 subscription with connected experiences enabled. Data availability depends on Microsoft's data providers - not all securities are available, and international stocks may have limited history. The function returns #VALUE! for unrecognized symbols or unavailable data. Currency exchange data is available for some currency pairs.

**Platform note:** STOCKHISTORY is exclusively available in Microsoft 365 Excel with connected experiences enabled. It is NOT available in Excel 2019, Excel 2021 (perpetual license), Google Sheets, or any other spreadsheet application. This is a cloud-connected feature requiring active Microsoft 365 subscription.

## Syntax

> [!f(x)] STOCKHISTORY Syntax
>
> ```
> =STOCKHISTORY(stock, start_date, [end_date], [interval], [headers], [properties])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `stock` | Yes | The stock ticker symbol or a cell containing a Stock data type. Examples: "MSFT", "AAPL", "GOOGL". |
| `start_date` | Yes | The start date for historical data. Can be a date value or text in date format. |
| `end_date` | No | The end date for historical data. Defaults to current date if omitted. |
| `interval` | No | Time interval: 0 = Daily (default), 1 = Weekly, 2 = Monthly. |
| `headers` | No | Include headers: 0 = No headers (default), 1 = Include column headers, 2 = Include instrument identifier and headers. |
| `properties` | No | Which data to return: 0 = Date and Close (default), 1 = Date, Open, High, Low, Close, Volume. Can also be an array of specific property codes. |

### Property Codes

| Code | Property |
|------|----------|
| 0 | Date |
| 1 | Close |
| 2 | Open |
| 3 | High |
| 4 | Low |
| 5 | Volume |

### Return Value

Returns a dynamic array of historical stock data. The array structure depends on parameters: typically dates in the first column and price/volume data in subsequent columns. Most recent data appears first (descending date order).

## Examples

> [!f(x)] STOCKHISTORY Examples

### Example 1: Basic 30-Day History
```
=STOCKHISTORY("MSFT", TODAY()-30)
```
**Result:** 30 days of Microsoft closing prices (2 columns: Date, Close)

**Explanation:** Simplest form - gets daily closing prices for the last 30 days. Dates and close prices only.

---

### Example 2: Specific Date Range
```
=STOCKHISTORY("AAPL", "2024-01-01", "2024-06-30")
```
**Result:** Apple stock data from January 1 to June 30, 2024

**Explanation:** Explicit start and end dates. Text dates work, or use DATE() function.

---

### Example 3: Full OHLCV Data
```
=STOCKHISTORY("GOOGL", TODAY()-90, , 0, 0, 1)
```
**Result:** 90 days of Google data with Date, Open, High, Low, Close, Volume

**Explanation:** Property code 1 returns full trading data. Six columns instead of two.

---

### Example 4: Weekly Data
```
=STOCKHISTORY("TSLA", DATE(2024,1,1), DATE(2024,12,31), 1)
```
**Result:** Weekly Tesla prices for 2024

**Explanation:** Interval 1 returns weekly aggregated data - useful for longer-term analysis without daily noise.

---

### Example 5: Monthly Data with Headers
```
=STOCKHISTORY("AMZN", DATE(2020,1,1), , 2, 1)
```
**Result:** Monthly Amazon prices from 2020 with column headers

**Explanation:** Interval 2 for monthly, headers 1 to include column names in first row.

---

### Example 6: Custom Property Selection
```
=STOCKHISTORY("META", TODAY()-60, , 0, 0, {0,1,5})
```
**Result:** Date, Close, and Volume only (no Open/High/Low)

**Explanation:** Array of property codes selects specific columns. Here: Date (0), Close (1), Volume (5).

---

### Example 7: Currency Exchange Rates
```
=STOCKHISTORY("USD/EUR", DATE(2024,1,1), TODAY())
```
**Result:** Historical USD to EUR exchange rates

**Explanation:** STOCKHISTORY works for currency pairs. Format: "BASE/QUOTE".

---

### Example 8: With Full Identifier Header
```
=STOCKHISTORY("NFLX", TODAY()-30, , 0, 2, 1)
```
**Result:** Netflix data with stock identifier and column headers

**Explanation:** Headers=2 adds the stock symbol above the data in addition to column headers.

---

### Example 9: Dynamic Stock Reference
```
=STOCKHISTORY(A1, B1, B2)
```
Where A1="MSFT", B1=start date, B2=end date

**Result:** Historical data for whatever stock/dates are in the referenced cells

**Explanation:** Cell references enable user-controlled stock selection. Change A1 to see different stocks.

---

### Example 10: Year-to-Date Performance
```
=STOCKHISTORY("NVDA", DATE(YEAR(TODAY()),1,1), TODAY(), 0, 1, {0,1})
```
**Result:** NVIDIA daily closing prices from January 1 to today with headers

**Explanation:** Dynamic start date using YEAR(TODAY()) always gets YTD data.

---

### Example 11: Calculate Returns from History
```
=LET(
    history, STOCKHISTORY("DIS", TODAY()-365, , 0, 0, {0,1}),
    prices, DROP(history, , 1),
    (INDEX(prices,1) - INDEX(prices, ROWS(prices))) / INDEX(prices, ROWS(prices))
)
```
**Result:** One-year return for Disney stock

**Explanation:** Fetch history, extract prices, calculate return from oldest to newest price.

---

### Example 12: Index Fund Data
```
=STOCKHISTORY("SPY", DATE(2024,1,1), , 1, 1, 1)
```
**Result:** Weekly SPY (S&P 500 ETF) data with full OHLCV and headers

**Explanation:** ETFs like SPY, QQQ, and IWM work like individual stocks.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Unrecognized stock symbol | Verify ticker symbol; try adding exchange (e.g., "MSFT" not "Microsoft") |
| `#VALUE!` | Invalid date format or range | Use valid dates; start_date must be before end_date |
| `#VALUE!` | Data not available for date range | Some stocks have limited history; try different dates |
| `#BLOCKED!` | Connected experiences disabled | Enable connected experiences in Excel Trust Center |
| `#BUSY!` | Data is loading | Wait for data to load; large requests take time |
| `#CONNECT!` | No internet connection | Verify network connectivity |
| `#SPILL!` | Spill range blocked | Clear cells where data needs to appear |

## Use Cases

### [[Investment Portfolio Tracking]]

**Scenario:** Track historical performance of portfolio holdings.

**Implementation:**
```
=STOCKHISTORY(HoldingSymbol, PurchaseDate, TODAY(), 0, 1, {0,1})
```

**Business Application:** Investors can track their holdings' price history from purchase date, calculate unrealized gains, and visualize performance over time.

**Technical Details:** Use PurchaseDate from transaction records. Calculate returns by comparing latest price to purchase price.

---

### [[Financial Analysis Charts]]

**Scenario:** Create dynamic stock charts that update automatically.

**Implementation:**
```
=STOCKHISTORY("Stock", TODAY()-180, , 0, 0, {0,2,3,4,1})
```
Columns arranged for candlestick charts: Date, Open, High, Low, Close.

**Business Application:** Financial analysts can create live-updating charts for presentations, reports, or monitoring dashboards.

**Technical Details:** Arrange property codes in OHLC order for candlestick chart compatibility. Excel's chart tools work directly with STOCKHISTORY output.

---

### [[Volatility Calculation]]

**Scenario:** Calculate historical volatility for options analysis.

**Implementation:**
```
=LET(
    history, STOCKHISTORY("STOCK", TODAY()-365, , 0, 0, {0,1}),
    prices, DROP(history, , 1),
    returns, (DROP(prices,1)-DROP(prices,-1))/DROP(prices,-1),
    STDEV(returns)*SQRT(252)
)
```

**Business Application:** Options traders and risk managers need volatility metrics. Historical volatility informs options pricing and risk assessment.

**Technical Details:** Calculate daily returns, then annualize standard deviation (multiply by sqrt of trading days).

## Platform Differences

### Microsoft Excel 365 (with Connected Experiences)

| Feature | Support |
|---------|---------|
| Basic functionality | Full support |
| All intervals | Full support |
| OHLCV data | Full support |
| Currency pairs | Partial support |
| Requires internet | Yes |

### Microsoft Excel 2021 (Perpetual)

| Feature | Support |
|---------|---------|
| STOCKHISTORY | NOT AVAILABLE |
| Reason | Cloud-connected feature requires 365 subscription |

### Google Sheets

| Feature | Support |
|---------|---------|
| STOCKHISTORY | NOT AVAILABLE |
| Alternative | Use GOOGLEFINANCE function |

**Google Sheets alternative:**
```
=GOOGLEFINANCE("MSFT", "close", DATE(2024,1,1), DATE(2024,6,30), "DAILY")
```
GOOGLEFINANCE provides similar functionality with different syntax.

### Other Platforms

STOCKHISTORY is exclusively available in Microsoft 365 Excel:
- Microsoft 365 (subscription): Full support
- Excel 2021/2019 (perpetual): Not available
- Google Sheets: Not available (use GOOGLEFINANCE)
- LibreOffice Calc: Not available
- Apple Numbers: Not available

## Tips and Best Practices

1. **Use well-known tickers:** "MSFT", "AAPL", "GOOGL" work reliably. Lesser-known or international stocks may not be available.

2. **Handle errors gracefully:** `=IFERROR(STOCKHISTORY(...), "Data unavailable")` prevents #VALUE! from breaking your workbook.

3. **Limit date ranges for performance:** Very long histories create large arrays. Use weekly/monthly intervals for multi-year analysis.

4. **Enable connected experiences:** File > Options > Trust Center > Privacy Options > Connected Experiences must be enabled.

5. **Data has delays:** STOCKHISTORY is not real-time. Expect 15-20 minute delays for recent data. Not suitable for trading decisions.

6. **Combine with charts:** STOCKHISTORY output works directly with Excel charts for dynamic visualizations.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| Stock data type | Live current stock info | For current price, not historical |
| [[GOOGLEFINANCE]] | Google's stock data (Sheets) | When using Google Sheets |
| External data connections | Custom data sources | When you need specific data providers |

### Commonly Used Together

**[[LET]]** - Structure complex calculations

*Calculate returns from history:*
```
=LET(
    data, STOCKHISTORY("MSFT", TODAY()-30),
    firstPrice, INDEX(data, ROWS(data), 2),
    lastPrice, INDEX(data, 1, 2),
    (lastPrice - firstPrice) / firstPrice
)
```
LET organizes historical data analysis.

---

**[[INDEX]]** - Extract specific values from history

*Get latest closing price:*
```
=INDEX(STOCKHISTORY("AAPL", TODAY()-7), 1, 2)
```
INDEX pulls specific values from the returned array.

---

**[[SPARKLINE]]** - Mini charts from historical data

*Price sparkline:*
```
Requires selecting price column from STOCKHISTORY output
```
Create inline visualizations from historical data.

---

**[[TAKE]] / [[DROP]]** - Manipulate returned arrays

*Get last 10 days from longer history:*
```
=TAKE(STOCKHISTORY("GOOGL", TODAY()-30), 10)
```
TAKE limits rows in the result.

## Official Documentation

- **Microsoft Excel:** [STOCKHISTORY function](https://support.microsoft.com/en-us/office/stockhistory-function-1ac8b5b3-5f62-4d94-8ab8-7504ec7239a8)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2020 | Initial release with Stock data types |
| Excel 365 | Ongoing | Data coverage expands over time |
| Excel 2021 | Not available | Perpetual license doesn't include cloud features |
| Earlier versions | Not available | Requires 365 subscription |

---

*Last updated: 2026-01-10*
