---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- financial
subTopics:
- currency-conversion
- euro
- european-monetary-union
- legacy-currencies
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Euro Conversion
- EMU Currency Converter
- Legacy Currency Conversion
tags:
- currency-conversion
- euro
- european-union
- monetary-conversion
- legacy-data
---

# EUROCONVERT

## Description

**EUROCONVERT** converts currencies between the Euro and the legacy national currencies of European Monetary Union (EMU) member countries, or directly between two legacy currencies using the fixed conversion rates established on January 1, 1999 (and later for subsequent adopters). This function is essential for working with historical financial data from before Euro adoption, converting legacy contracts and obligations, and maintaining compliance with EU regulations requiring precise conversion using official rates.

When the Euro was introduced, the European Central Bank established irrevocable fixed exchange rates between the Euro and each participating national currency. These rates were not rounded market rates but precisely calculated conversion factors that must be used for all official currency conversions. For example, 1 Euro = 6.55957 French Francs, exactly. EUROCONVERT uses these official rates and follows the triangulation method required by EU law: converting between two legacy currencies always goes through the Euro as an intermediate step, never directly.

**This function is Excel-only and not available in Google Sheets.** Google Sheets does not include EUROCONVERT, likely because its primary use case (legacy EMU currency conversion) has diminished significantly since most legacy currencies have been out of circulation for many years. If you need this functionality in Google Sheets, you must implement custom formulas using the fixed conversion rates.

EUROCONVERT requires the Euro Currency Tools add-in in older Excel versions but is built into Excel 2007 and later. The function remains relevant for historical data analysis, legal compliance for contracts denominated in legacy currencies, and archival financial record conversion. While most day-to-day transactions now use only Euros, legacy obligations, historical comparisons, and certain legal documents still require precise conversions using the official rates.

## Syntax

> [!f(x)] EUROCONVERT Syntax
>
> ```
> =EUROCONVERT(number, source, target, [full_precision], [triangulation_precision])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The currency amount to convert. Can be any positive or negative number. |
| `source` | Yes | A three-letter ISO currency code for the source currency. Must be "EUR" for Euro or a valid legacy EMU currency code (see table below). Case-insensitive. |
| `target` | Yes | A three-letter ISO currency code for the target currency. Must be "EUR" for Euro or a valid legacy EMU currency code. Case-insensitive. |
| `full_precision` | No | If TRUE (or omitted), uses all significant figures. If FALSE, rounds result to currency-specific decimal places (2 for most currencies, 0 for Italian Lira, etc.). |
| `triangulation_precision` | No | Number of significant digits for the intermediate Euro value when converting between two legacy currencies. Default is 3. Minimum is 3. |

### Supported Currency Codes

| Code | Currency | Conversion Rate (1 EUR =) |
|------|----------|---------------------------|
| EUR | Euro | 1 |
| ATS | Austrian Schilling | 13.7603 |
| BEF | Belgian Franc | 40.3399 |
| DEM | German Mark (Deutsche Mark) | 1.95583 |
| ESP | Spanish Peseta | 166.386 |
| FIM | Finnish Markka | 5.94573 |
| FRF | French Franc | 6.55957 |
| GRD | Greek Drachma | 340.750 |
| IEP | Irish Pound (Punt) | 0.787564 |
| ITL | Italian Lira | 1936.27 |
| LUF | Luxembourg Franc | 40.3399 |
| NLG | Dutch Guilder (Netherlands) | 2.20371 |
| PTE | Portuguese Escudo | 200.482 |

### Return Value

Returns the converted currency amount. If full_precision is FALSE, the result is rounded appropriately for the target currency.

## Examples

> [!f(x)] EUROCONVERT Examples

### Example 1: Euro to German Marks
```
=EUROCONVERT(100, "EUR", "DEM")
```
**Result:** `195.583`

**Explanation:** Converts 100 Euros to German Marks using the fixed rate of 1 EUR = 1.95583 DEM. Result: 100 x 1.95583 = 195.583 DEM.

---

### Example 2: French Francs to Euro
```
=EUROCONVERT(1000, "FRF", "EUR")
```
**Result:** `152.449`

**Explanation:** Converts 1,000 French Francs to Euros. Calculation: 1000 / 6.55957 = 152.4490... EUR. The fixed rate ensures precise, legally compliant conversion.

---

### Example 3: Legacy to Legacy (Triangulation)
```
=EUROCONVERT(500, "DEM", "FRF")
```
**Result:** `1676.94`

**Explanation:** Converts German Marks to French Francs. EU regulations require triangulation: DEM -> EUR -> FRF. 500 DEM / 1.95583 = 255.646 EUR, then 255.646 x 6.55957 = 1676.94 FRF.

---

### Example 4: Italian Lira with Rounding
```
=EUROCONVERT(1000000, "ITL", "EUR", FALSE)
```
**Result:** `516.46`

**Explanation:** Converts 1 million Italian Lira to Euros with currency rounding (full_precision = FALSE). Result is rounded to 2 decimal places for Euros. 1000000 / 1936.27 = 516.4569... rounds to 516.46.

---

### Example 5: Full Precision vs. Rounded
```
Full: =EUROCONVERT(1234.56, "EUR", "FRF", TRUE)  = 8097.628...
Rounded: =EUROCONVERT(1234.56, "EUR", "FRF", FALSE) = 8097.63
```
**Result:** Different precision levels

**Explanation:** With full_precision = TRUE (or omitted), all decimal places are preserved. With FALSE, the result is rounded to the target currency's standard precision (2 decimal places for French Francs).

---

### Example 6: Spanish Pesetas to Euro
```
=EUROCONVERT(50000, "ESP", "EUR")
```
**Result:** `300.51`

**Explanation:** Converts 50,000 Spanish Pesetas to Euros. 50000 / 166.386 = 300.506... EUR. The high peseta-to-euro rate reflects the peseta's lower unit value.

---

### Example 7: Austrian Schillings Round Trip
```
Step 1: =EUROCONVERT(1000, "EUR", "ATS") = 13760.3
Step 2: =EUROCONVERT(13760.3, "ATS", "EUR") = 1000
```
**Result:** Round-trip conversion returns original

**Explanation:** Converting to and from a legacy currency should return the original amount (subject to rounding). This validates the conversion rates are inverse of each other.

---

### Example 8: Greek Drachma (Late Adopter)
```
=EUROCONVERT(100000, "GRD", "EUR")
```
**Result:** `293.47`

**Explanation:** Greece joined the Euro later (2001) than the original 11 countries (1999). The Drachma rate of 340.750 per Euro was fixed when Greece qualified for EMU membership.

---

### Example 9: Irish Pound to Dutch Guilder
```
=EUROCONVERT(100, "IEP", "NLG")
```
**Result:** `279.81`

**Explanation:** Converting between two legacy currencies uses triangulation. 100 IEP / 0.787564 = 127.0... EUR x 2.20371 = 279.81 NLG. The intermediate Euro calculation ensures accuracy.

---

### Example 10: Triangulation Precision Parameter
```
=EUROCONVERT(1000, "FIM", "PTE", TRUE, 6)
```
**Result:** Higher precision intermediate

**Explanation:** When converting Finnish Markka to Portuguese Escudo, the triangulation_precision parameter (6) specifies using 6 significant digits for the intermediate Euro value instead of the default 3. This may improve accuracy for large amounts.

---

### Example 11: Belgian to Luxembourg Francs (Same Rate)
```
=EUROCONVERT(1000, "BEF", "LUF")
```
**Result:** `1000`

**Explanation:** Belgium and Luxembourg had the same conversion rate (40.3399) because their currencies were at parity before Euro adoption. Converting between them yields a 1:1 result.

---

### Example 12: Handling Large Italian Lira Amounts
```
=EUROCONVERT(1000000000, "ITL", "EUR", FALSE)
```
**Result:** `516,456.90`

**Explanation:** One billion Lira converts to approximately half a million Euros. Italian Lira had a very high exchange rate (1936.27 per Euro), so large nominal Lira amounts translate to much smaller Euro values.

---

### Example 13: Converting Historical Contract Values
```
Legacy Contract: 10000 DEM monthly
Current Value: =EUROCONVERT(10000, "DEM", "EUR") = 5112.92 EUR
```
**Result:** `5,112.92 EUR per month`

**Explanation:** Historical contracts denominated in legacy currencies must be converted using official EUROCONVERT rates for legal and accounting purposes. This ensures compliance with EU monetary conversion regulations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid or unrecognized currency code | Use valid EMU currency codes: EUR, ATS, BEF, DEM, ESP, FIM, FRF, GRD, IEP, ITL, LUF, NLG, PTE |
| `#VALUE!` | Non-EMU currency code used | EUROCONVERT only works with Euro and EMU legacy currencies. For other currencies (USD, GBP, etc.), use market exchange rates or other conversion methods |
| `#NAME?` | Function not available | EUROCONVERT is Excel-only. In older Excel versions, enable the Euro Currency Tools add-in via Tools > Add-Ins |
| `#VALUE!` | Text instead of number for amount | Ensure the number parameter is a numeric value, not text |
| `Unexpected precision` | full_precision parameter misunderstanding | TRUE = maximum precision, FALSE = currency-appropriate rounding (usually 2 decimal places) |
| `Different than expected` | Triangulation affecting results | When converting between legacy currencies, the intermediate Euro step can introduce minor rounding. Use higher triangulation_precision if needed |

## Use Cases

### [[Historical Financial Data Analysis]]

**Scenario:** A financial analyst is studying European monetary integration and needs to compare pre-1999 economic data denominated in various national currencies to create consistent time series.

**Implementation:**
```
German GDP (1998): 3,867.4 billion DEM
Converted: =EUROCONVERT(3867400000000, "DEM", "EUR") = 1,977.4 billion EUR

French GDP (1998): 8,485.7 billion FRF
Converted: =EUROCONVERT(8485700000000, "FRF", "EUR") = 1,293.5 billion EUR
```

**Business Application:** Academic researchers, central banks, and policy analysts need to analyze historical economic data across EMU countries. Converting all historical figures to Euros enables meaningful cross-country and cross-time comparisons without distortion from exchange rate fluctuations.

**Technical Details:** For large-scale data conversion, create a lookup table mapping countries to currency codes. Use VLOOKUP or INDEX/MATCH with EUROCONVERT for automated batch processing. Always document that conversions use official fixed rates, not historical market rates.

---

### [[Legacy Contract Conversion for Legal Compliance]]

**Scenario:** A law firm is handling a dispute involving a 1997 contract denominated in Italian Lira. The current Euro-equivalent value must be calculated using legally mandated conversion rates.

**Implementation:**
```
Contract Amount: 500,000,000 ITL
Legal EUR Value: =EUROCONVERT(500000000, "ITL", "EUR", FALSE) = 258,228.48 EUR
```

**Business Application:** EU regulations require that all conversions from legacy currencies to Euros use the irrevocable fixed rates. EUROCONVERT ensures legal compliance for contract valuations, inheritance calculations, pension conversions, and any other legal matters involving pre-Euro monetary amounts.

**Technical Details:** For legal documents, always use full_precision = FALSE to get properly rounded currency values. Document the conversion method and reference the relevant EU regulation (Council Regulation (EC) No 1103/97). Maintain audit trails of all conversions.

---

### [[Archival Record Conversion for Accounting]]

**Scenario:** A German company is digitizing historical accounting records from before 2002 (when Deutsche Marks were physically replaced by Euros) and needs to convert all amounts to Euros for the modern accounting system.

**Implementation:**
```
Historical Amount: =EUROCONVERT(B2, "DEM", "EUR")
```
Applied to entire column of historical DEM values

**Business Application:** Companies with pre-Euro historical records often need to integrate this data with modern Euro-denominated systems. EUROCONVERT enables systematic conversion of archival data while maintaining the precision required for accounting reconciliation.

**Technical Details:** Create a conversion template with the source currency as a parameter. For large datasets, consider converting and storing Euro values rather than recalculating repeatedly. Maintain original legacy currency values alongside converted values for audit purposes.

---

### [[Pension and Long-Term Obligation Calculations]]

**Scenario:** A pension fund is calculating the current value of legacy pension obligations that were originally defined in French Francs before France adopted the Euro.

**Implementation:**
```
Original Pension: 10,000 FRF per month
Current EUR Value: =EUROCONVERT(10000, "FRF", "EUR") = 1,524.49 EUR
Annual Value: =EUROCONVERT(10000, "FRF", "EUR") * 12 = 18,293.88 EUR
```

**Business Application:** Many pension obligations, insurance policies, and long-term contracts originating before Euro adoption were denominated in legacy currencies. These must be converted to Euros using official rates for benefit calculations, reserve valuations, and regulatory reporting.

**Technical Details:** For recurring obligations (monthly pensions), convert the periodic amount and apply to all future periods. Document the conversion basis. Be aware that some contracts may have specific conversion clauses that supersede standard EUROCONVERT rates (though this is rare).

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2007 and later (built-in); Excel 2003 and earlier requires Euro Currency Tools add-in
- **Specific Behavior:** Supports all 12 original EMU currencies plus Euro (13 codes total)
- **Add-in Requirement:** In older versions, enable via Tools > Add-Ins > Euro Currency Tools

### Google Sheets

- **Availability:** NOT AVAILABLE
- **Alternative:** Implement custom conversion using fixed rates in formulas or a lookup table

### Implementing EUROCONVERT in Google Sheets

```
// Custom formula approach
=IF(source="EUR", number * VLOOKUP(target, rates, 2),
    IF(target="EUR", number / VLOOKUP(source, rates, 2),
        number / VLOOKUP(source, rates, 2) * VLOOKUP(target, rates, 2)))
```

Where `rates` is a named range containing:
| Currency | Rate |
|----------|------|
| DEM | 1.95583 |
| FRF | 6.55957 |
| ... | ... |

## Tips and Best Practices

1. **Use valid currency codes only:** EUROCONVERT only supports EUR and the 12 legacy EMU currencies. Codes like USD, GBP, or CHF will produce errors. For non-EMU currencies, use different conversion methods.

2. **Understand triangulation:** When converting between two legacy currencies (e.g., DEM to FRF), the function automatically converts through Euro as required by EU law. This ensures legal compliance and consistency.

3. **Choose appropriate precision:** For accounting and legal purposes, use full_precision = FALSE to get standard currency rounding. For analytical calculations, use TRUE (or omit) to preserve maximum precision.

4. **Document your conversions:** For audit trails and legal compliance, always document that conversions use EUROCONVERT with official fixed rates, not historical market exchange rates.

5. **Verify round-trip accuracy:** Test conversions by converting back to the original currency. The result should match (subject to rounding). Large discrepancies indicate an error.

6. **Remember this is Excel-only:** If you need cross-platform compatibility, implement a custom solution using the fixed conversion rates table rather than relying on EUROCONVERT.

7. **Consider the year of adoption:** Greece (GRD) joined EMU in 2001, not 1999 like the original 11 countries. Later adopters (Slovenia, Cyprus, etc.) are NOT included in EUROCONVERT.

8. **Handle large Italian Lira amounts carefully:** Italian Lira amounts are typically 3 orders of magnitude larger than equivalent Euro amounts. Ensure your formatting can handle both scales.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CONVERT]] | Converts between various measurement units | For non-currency unit conversions (length, weight, etc.) |
| Custom formula | Manual conversion using fixed rates | When working in Google Sheets or needing non-EMU currencies |

### Commonly Used Together

**[[ROUND]]** - Rounding

*Apply custom rounding to conversion results:*
```
=ROUND(EUROCONVERT(1000, "DEM", "EUR"), 2)
```
Apply specific decimal precision when full_precision = TRUE but you need consistent rounding.

---

**[[VLOOKUP]]** - Dynamic Currency Selection

*Look up currency code from a table:*
```
=EUROCONVERT(A1, VLOOKUP(B1, currency_table, 2), "EUR")
```
When source currency varies by row, use VLOOKUP to retrieve the appropriate currency code.

---

**[[IF]]** - Conditional Conversion

*Apply conversion only when needed:*
```
=IF(currency="EUR", amount, EUROCONVERT(amount, currency, "EUR"))
```
Skip conversion if amount is already in Euros.

---

**[[SUMPRODUCT]]** - Aggregate Converted Values

*Sum multiple converted amounts:*
```
=SUMPRODUCT(EUROCONVERT(amounts, currencies, "EUR"))
```
Convert and aggregate multiple legacy currency amounts to a single Euro total.

## Official Documentation

- **Microsoft Excel:** [EUROCONVERT function](https://support.microsoft.com/en-us/office/euroconvert-function-79c8fd67-c665-450e-bb6c-15fc92f8345c)
- **European Central Bank:** [Euro Exchange Rates](https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/index.en.html)
- **EU Legal Basis:** [Council Regulation (EC) No 1103/97](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A31997R1103)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 (2007) | Built-in function |
| Excel 2003 and earlier | Euro Currency Tools add-in | Required manual activation of add-in |
| Google Sheets | Not available | Must use custom implementation |

---

*Last updated: 2026-01-10*
