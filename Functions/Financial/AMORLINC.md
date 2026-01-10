---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- depreciation
- french-accounting
- asset-management
- straight-line-depreciation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- French Linear Depreciation
- Amortissement Lineaire
- Prorated Straight-Line Depreciation
tags:
- depreciation
- french-accounting
- asset-management
- fixed-assets
- straight-line
---

# AMORLINC

## Description

**AMORLINC (Amortissement Lineaire)** calculates the depreciation of an asset for a specific accounting period using the French linear (straight-line) depreciation method with prorated first and last periods. Unlike the simple SLN function that assumes full-period depreciation, AMORLINC properly handles partial periods when an asset is purchased mid-year or disposed of before the end of its useful life. This makes it essential for accurate French accounting compliance where fiscal year conventions require precise allocation of depreciation expense.

The function applies a consistent depreciation rate throughout the asset's life, but allocates the first period's depreciation based on how much of the period the asset was actually in service. If you purchase equipment on July 1 with a calendar year-end, AMORLINC will calculate depreciation for only the 6 months from July through December, not the full year. Subsequent full years receive complete annual depreciation, and the final period receives only what remains to fully depreciate the asset to its salvage value.

**Critical distinction from AMORDEGRC:** While AMORDEGRC uses accelerated degressive depreciation with coefficients (1.5x, 2.0x, 2.5x) that front-load depreciation expense, AMORLINC uses a constant rate throughout the asset's life. The choice between them depends on French tax elections, the type of asset, and whether accelerated depreciation is advantageous for your tax situation. AMORLINC is often preferred for assets that provide consistent economic benefit over their life, while AMORDEGRC suits assets that lose productivity or value rapidly in early years.

AMORLINC is available in Excel (built-in since Excel 2007; requires Analysis ToolPak in earlier versions) and Google Sheets. The function is designed specifically for French accounting standards (Plan Comptable General) but is useful for any scenario requiring straight-line depreciation with proper partial-period proration. The function handles day count basis options to accommodate different accounting conventions.

## Syntax

> [!f(x)] AMORLINC Syntax
>
> ```
> =AMORLINC(cost, date_purchased, first_period, salvage, period, rate, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `cost` | Yes | The original cost of the asset. Must be a positive number representing the total acquisition cost including all costs necessary to place the asset in service (purchase price, delivery, installation, etc.). |
| `date_purchased` | Yes | The date the asset was purchased or placed in service. This date determines the proration of the first period's depreciation. Accepts date serial numbers, text dates, or cell references containing dates. |
| `first_period` | Yes | The end date of the first accounting period. This establishes when your fiscal year ends and is used to calculate how much of the first period the asset was in service. |
| `salvage` | Yes | The salvage value (residual value) of the asset at the end of its useful life. Must be 0 or a positive number less than cost. Represents the expected value when the asset is disposed. |
| `period` | Yes | The period for which you want to calculate depreciation. Period 0 is the first (partial) period from purchase date to first_period end. Periods 1, 2, 3... are subsequent full periods. |
| `rate` | Yes | The annual depreciation rate expressed as a decimal. For a 5-year life, use 0.20 (1/5 = 20%). For a 10-year life, use 0.10. This rate is applied to the depreciable base (cost - salvage). |
| `basis` | No | The day count basis for calculating the fraction of the first period. Default is 0. Options: 0 = US (NASD) 30/360, 1 = Actual/Actual, 2 = Actual/360, 3 = Actual/365, 4 = European 30/360. |

### Return Value

Returns a single numeric value representing the depreciation amount for the specified period. Returns 0 if the asset is fully depreciated in prior periods or if the period number exceeds the asset's depreciable life.

## Examples

> [!f(x)] AMORLINC Examples

### Example 1: Basic First-Period Depreciation
```
=AMORLINC(2400, "2024-08-19", "2024-12-31", 300, 0, 0.15)
```
**Result:** `$114.00`

**Explanation:** Calculates depreciation for a $2,400 asset purchased August 19, 2024, with a December 31 fiscal year end. With a 15% annual rate and $300 salvage value, the depreciable base is $2,100. The first period (August 19 to December 31) is approximately 4.5 months, so period 0 receives prorated straight-line depreciation of $114.00.

---

### Example 2: Full-Year Depreciation (Period 1)
```
=AMORLINC(2400, "2024-08-19", "2024-12-31", 300, 1, 0.15)
```
**Result:** `$315.00`

**Explanation:** Period 1 represents the first full year after the partial first period. The annual depreciation on a depreciable base of $2,100 ($2,400 - $300) at 15% is $315.00. Full years always receive the complete annual depreciation amount in straight-line calculations.

---

### Example 3: Equipment with 5-Year Life
```
=AMORLINC(50000, "2024-01-15", "2024-12-31", 5000, 0, 0.20)
```
**Result:** `$8,750.00`

**Explanation:** A $50,000 asset purchased January 15 with 20% annual rate (5-year life) and $5,000 salvage. Depreciable base is $45,000. Since purchase was early in January, almost the full year of depreciation applies. Annual depreciation would be $9,000, so period 0 receives approximately 97% of that ($8,750).

---

### Example 4: Mid-Year Purchase Comparison
```
=AMORLINC(100000, "2024-07-01", "2024-12-31", 10000, 0, 0.20)
```
**Result:** `$9,000.00`

**Explanation:** A $100,000 asset purchased July 1 with 20% rate and $10,000 salvage. Depreciable base is $90,000, annual depreciation is $18,000. Since the asset was purchased exactly halfway through the year (assuming 30/360 convention), period 0 receives exactly half: $9,000.

---

### Example 5: Comparing Periods 0 Through 5
```
Period 0: =AMORLINC(60000, "2024-04-01", "2024-12-31", 0, 0, 0.20) = $9,000.00
Period 1: =AMORLINC(60000, "2024-04-01", "2024-12-31", 0, 1, 0.20) = $12,000.00
Period 2: =AMORLINC(60000, "2024-04-01", "2024-12-31", 0, 2, 0.20) = $12,000.00
Period 3: =AMORLINC(60000, "2024-04-01", "2024-12-31", 0, 3, 0.20) = $12,000.00
Period 4: =AMORLINC(60000, "2024-04-01", "2024-12-31", 0, 4, 0.20) = $12,000.00
Period 5: =AMORLINC(60000, "2024-04-01", "2024-12-31", 0, 5, 0.20) = $3,000.00
```
**Result:** Consistent amounts with prorated first and last periods

**Explanation:** For a $60,000 asset with no salvage and 20% rate (5-year life), annual depreciation is $12,000. Period 0 gets 9 months (April-December) = $9,000. Periods 1-4 get full $12,000. Period 5 gets the remaining 3 months = $3,000. Total: $9,000 + $48,000 + $3,000 = $60,000.

---

### Example 6: Zero Salvage Value
```
=AMORLINC(25000, "2024-03-15", "2024-12-31", 0, 0, 0.10)
```
**Result:** `$1,958.33`

**Explanation:** With zero salvage value, the entire $25,000 cost is depreciated over the asset's 10-year life ($25,000 x 10% = $2,500 per year). Period 0 is prorated from March 15 to December 31, approximately 9.5 months, yielding $1,958.33.

---

### Example 7: Short Asset Life (3 Years)
```
=AMORLINC(15000, "2024-06-01", "2024-12-31", 1500, 0, 0.333)
```
**Result:** `$2,497.50`

**Explanation:** A $15,000 asset with $1,500 salvage and 33.3% rate (3-year life). Depreciable base is $13,500, annual depreciation is $4,495. Period 0 (June to December = 7 months) receives approximately $2,497.50.

---

### Example 8: Using Actual/Actual Day Count
```
=AMORLINC(10000, "2024-02-15", "2024-12-31", 1000, 0, 0.20, 1)
```
**Result:** Slightly different from default

**Explanation:** Using basis=1 (Actual/Actual) instead of the default 30/360 will produce a slightly different first-period amount because it uses actual days in the period rather than the simplified 30-day month convention. The difference is typically small but matters for precise accounting.

---

### Example 9: Non-Calendar Fiscal Year
```
=AMORLINC(80000, "2024-09-01", "2025-03-31", 8000, 0, 0.20)
```
**Result:** `$8,400.00`

**Explanation:** For a company with a March 31 fiscal year end, an asset purchased September 1 is in service for 7 months in period 0. Depreciable base of $72,000 at 20% = $14,400 annually. Period 0 = 7/12 x $14,400 = $8,400.

---

### Example 10: European Day Count Basis
```
=AMORLINC(50000, "2024-05-31", "2024-12-31", 5000, 0, 0.10, 4)
```
**Result:** European 30/360 calculation

**Explanation:** Basis 4 uses European 30/360 convention, which handles end-of-month dates differently than US 30/360 (basis 0). For assets purchased on the last day of a month, this can affect the proration calculation.

---

### Example 11: Comparing Linear vs Degressive
```
Linear Year 0:    =AMORLINC(100000, "2024-01-15", "2024-12-31", 10000, 0, 0.20)
Degressive Year 0: =AMORDEGRC(100000, "2024-01-15", "2024-12-31", 10000, 0, 0.20)
```
**Result:** Linear ~$17,500 vs Degressive ~$36,000

**Explanation:** For the same asset, AMORLINC produces consistent annual amounts while AMORDEGRC front-loads depreciation with the 2.0 coefficient (for 5-year assets). This comparison helps decide which method provides better tax benefits based on the company's financial situation.

---

### Example 12: Complete Schedule Validation
```
=SUMPRODUCT(AMORLINC($B$1, $B$2, $B$3, $B$4, ROW(A1:A10)-1, $B$5))
```
**Result:** Should equal Cost - Salvage

**Explanation:** Summing all periods' depreciation should equal the depreciable base (cost minus salvage). This formula creates an array of periods 0-9 and sums their AMORLINC results. Use this to validate that your depreciation schedule is complete and accurate.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format, text in numeric parameters | Ensure dates are valid Excel/Sheets date values. Verify cost, salvage, period, and rate are numeric. |
| `#NUM!` | Negative cost, salvage, or rate | All monetary values and the rate must be non-negative. Salvage must be less than cost. |
| `#NUM!` | Rate equals 0 | A 0% depreciation rate is mathematically invalid. Rate must be greater than 0. |
| `#NUM!` | Invalid basis value | Basis must be 0, 1, 2, 3, or 4. Any other value produces an error. |
| `0 (unexpected)` | Period exceeds asset life | Once total depreciation equals cost minus salvage, additional periods return 0. Check your period numbers. |
| `#NAME?` | Function not found in older Excel | In Excel 2003 and earlier, enable the Analysis ToolPak add-in via Tools > Add-Ins. |
| `Incorrect proration` | Date_purchased after first_period | Purchase date must be before the first period end date. Check date order. |

## Use Cases

### [[French GAAP Statutory Reporting]]

**Scenario:** A French corporation needs to calculate depreciation for its fixed assets according to Plan Comptable General (French GAAP) for statutory financial statements submitted to French authorities.

**Implementation:**
```
=AMORLINC(cost, in_service_date, fiscal_year_end, salvage, year_number, 1/useful_life)
```
Where:
- cost = Asset purchase price plus installation
- in_service_date = Date asset began operation
- fiscal_year_end = Company's accounting period end
- salvage = Estimated disposal value
- year_number = Current depreciation period (0 for first year)
- 1/useful_life = Depreciation rate (e.g., 1/5 = 0.20 for 5-year life)

**Business Application:** French companies are required to maintain statutory books following French GAAP. AMORLINC ensures depreciation calculations comply with linear depreciation requirements, including proper proration for partial first and last years. This is essential for filing corporate tax returns and financial statements with the Direction Generale des Finances Publiques.

**Technical Details:** French fiscal year often aligns with calendar year, but some companies use different fiscal periods. The first_period parameter must match the actual fiscal year end. Keep documentation of asset purchase dates, costs, and useful life determinations for audit purposes. Compare AMORLINC results with AMORDEGRC to determine which method optimizes tax position.

---

### [[Multinational Fixed Asset Register Reconciliation]]

**Scenario:** A global company maintains fixed assets in multiple countries and needs to track depreciation under both local GAAP (French for French subsidiary) and group reporting standards (IFRS or US GAAP).

**Implementation:**
```
French Statutory: =AMORLINC(cost, purchase_date, year_end, salvage, period, french_rate)
IFRS Reporting:   =SLN(cost, salvage, useful_life_years)
Adjustment:       =French_Depreciation - IFRS_Depreciation
```

**Business Application:** Multinational companies must reconcile depreciation differences between local and group reporting standards. While both may use straight-line, differences in useful life assumptions, salvage values, or first-period conventions create temporary differences that must be tracked for consolidated reporting and deferred tax calculations.

**Technical Details:** Create a fixed asset register that calculates depreciation under each required standard. Track differences in a separate column for journal entries during consolidation. These differences may create deferred tax assets or liabilities depending on whether local depreciation is higher or lower than group depreciation.

---

### [[Capital Budgeting with Tax Impact Analysis]]

**Scenario:** A company is evaluating a capital investment and needs to project after-tax cash flows, considering the French linear depreciation tax shield.

**Implementation:**
```
Depreciation Year N: =AMORLINC(capital_cost, purchase_date, year_end, salvage, N-1, rate)
Tax Shield Year N:   =Depreciation_Year_N * tax_rate
After-Tax Cash Flow: =Operating_Cash_Flow + Tax_Shield
NPV:                 =NPV(discount_rate, after_tax_cash_flows) - initial_investment
```

**Business Application:** Depreciation reduces taxable income, creating a "tax shield" that increases after-tax cash flows. Capital budgeting decisions should include the present value of depreciation tax shields. Using AMORLINC ensures accurate projections matching actual French tax depreciation.

**Technical Details:** Model each year's depreciation using AMORLINC to get precise tax shield amounts. First-year partial depreciation will produce a smaller tax shield if the asset is purchased mid-year. Include this timing effect in NPV calculations. Compare with AMORDEGRC to determine if accelerated depreciation produces higher NPV through earlier tax savings.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2007 and later (built-in); Excel 2003 and earlier requires Analysis ToolPak add-in
- **Specific Behavior:** Accepts all 7 parameters. Returns numeric value with full precision (not rounded like AMORDEGRC).
- **Add-in Installation:** In older Excel versions: Tools > Add-Ins > check "Analysis ToolPak"

### Google Sheets

- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Built-in, no add-in required.
- **Precision:** Same calculation precision as Excel

### Both Platforms

- Period 0 is the first partial period (purchase date to first_period end)
- Full years are periods 1, 2, 3, etc.
- Total depreciation over all periods equals cost minus salvage
- Day count basis affects only the first-period proration
- Returns 0 for periods after the asset is fully depreciated

## Tips and Best Practices

1. **Understand period numbering:** Period 0 is the first partial period from purchase to fiscal year end. If you purchase January 1 and your year ends December 31, period 0 covers the full first year. Periods 1+ are subsequent full fiscal years.

2. **Calculate the rate correctly:** Rate = 1 / useful life in years. For a 7-year asset, rate = 1/7 = 0.142857. Do not round this value prematurely as it affects all depreciation calculations.

3. **Verify total depreciation:** Sum depreciation for all periods (0 through end of asset life). The total should exactly equal cost minus salvage. Any difference indicates an error in parameters.

4. **Choose the right basis for your jurisdiction:** While basis 0 (US 30/360) is the default, French accounting may prefer basis 4 (European 30/360) or basis 1 (Actual/Actual) depending on company policy and auditor requirements.

5. **Compare with AMORDEGRC for tax optimization:** Run both functions to see which produces more favorable tax results. Generally, AMORDEGRC provides larger early deductions, improving cash flow if the company is profitable.

6. **Document asset details thoroughly:** Maintain records of purchase date, cost including capitalized installation, useful life determination, and salvage value estimate. Auditors will want supporting documentation.

7. **Handle disposals correctly:** When disposing of an asset mid-year, calculate depreciation only through the disposal date. Multiply the full-period AMORLINC result by the fraction of the year before disposal.

8. **Use absolute references in schedules:** When building depreciation schedules, use absolute references ($) for cost, dates, salvage, and rate so you can copy the formula down for different periods.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AMORDEGRC]] | French degressive (accelerated) depreciation | When French tax law permits degressive method and you want accelerated deductions |
| [[SLN]] | Simple straight-line depreciation | When you do not need partial-period proration (asset acquired at period start) |
| [[DB]] | Fixed declining balance depreciation | For declining balance without French coefficients |
| [[DDB]] | Double declining balance depreciation | For 200% declining balance method |
| [[VDB]] | Variable declining balance depreciation | For flexible depreciation with optional switch to straight-line |

### Commonly Used Together

**[[AMORDEGRC]]** - French Degressive Depreciation

*Compare methods for optimal tax planning:*
```
Linear Total Years 1-3:    =SUMPRODUCT(AMORLINC(cost, date, yearend, salvage, {0,1,2}, rate))
Degressive Total Years 1-3: =SUMPRODUCT(AMORDEGRC(cost, date, yearend, salvage, {0,1,2}, rate))
```
Compare cumulative depreciation to determine which method produces larger early deductions for tax planning.

---

**[[SLN]]** - Simple Straight-Line

*Use for quick estimates, AMORLINC for actual books:*
```
Quick estimate: =SLN(cost, salvage, life)
Actual Period 0: =AMORLINC(cost, purchase_date, year_end, salvage, 0, 1/life)
```
SLN provides the annual depreciation amount; AMORLINC prorates it appropriately for partial years.

---

**[[NPV]]** - Net Present Value

*Value the depreciation tax shield:*
```
=NPV(discount_rate, tax_rate * AMORLINC(..., 0, ...), tax_rate * AMORLINC(..., 1, ...), ...)
```
Calculate the present value of tax savings from depreciation deductions for capital budgeting.

---

**[[IF]]** - Conditional Logic

*Handle fully depreciated assets:*
```
=IF(accumulated_depreciation >= cost - salvage, 0, AMORLINC(cost, date, yearend, salvage, period, rate))
```
Prevent depreciation beyond the depreciable base in complex models.

## Official Documentation

- **Microsoft Excel:** [AMORLINC function](https://support.microsoft.com/en-us/office/amorlinc-function-7d417b45-f7f5-4dba-a0a5-3451a81079a8)
- **Google Sheets:** [AMORLINC function](https://support.google.com/docs/answer/3093155)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 (2007) | Built-in function; previously required Analysis ToolPak |
| Excel 2003 and earlier | Analysis ToolPak add-in | Required manual activation of add-in |
| Google Sheets | Original launch | Built-in support from the beginning |

---

*Last updated: 2026-01-10*
