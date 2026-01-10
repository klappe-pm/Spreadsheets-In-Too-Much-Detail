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
- tax-accounting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- French Degressive Depreciation
- Amortissement Degressif
- Declining Balance Depreciation French
tags:
- depreciation
- french-accounting
- asset-management
- fixed-assets
- tax-compliance
---

# AMORDEGRC

## Description

**AMORDEGRC (Amortissement Degressif)** calculates the depreciation of an asset for a specific accounting period using the French degressive (declining balance) depreciation method. This function is specifically designed for French accounting standards and applies a coefficient-based accelerated depreciation system that allows businesses to claim larger depreciation expenses in the early years of an asset's life. The "RC" in the name stands for "rounded to the cent," indicating that results are rounded to two decimal places for accounting precision.

The French degressive depreciation method applies different coefficients to the straight-line rate based on the asset's useful life: assets with 3-4 year life use a 1.5 coefficient, 5-6 year assets use 2.0, and assets with 7+ years use 2.5. This means a 5-year asset depreciates at 40% (20% straight-line rate times 2.0 coefficient) in year one, applied to the remaining book value each year. This accelerated approach reflects the reality that many assets lose value more quickly in their early years and provides tax benefits by deferring taxable income to later periods.

**Critical distinction from AMORLINC:** While AMORLINC uses straight-line depreciation prorated for partial periods, AMORDEGRC uses degressive (declining balance) depreciation with French-specific coefficients. The prorated first-period calculation in AMORDEGRC also differs--it uses a specific day-counting convention that allocates depreciation based on where the purchase date falls within the year. Additionally, AMORDEGRC may skip a depreciation period in certain cases due to its coefficient application rules.

AMORDEGRC is available in Excel (requires Analysis ToolPak add-in in older versions, built-in since Excel 2007) and Google Sheets. The function is particularly important for multinational companies with French subsidiaries or any business following French GAAP (Plan Comptable General). Note that this function is designed specifically for French tax and accounting compliance--other countries use different depreciation methods that may require different functions.

## Syntax

> [!f(x)] AMORDEGRC Syntax
>
> ```
> =AMORDEGRC(cost, date_purchased, first_period, salvage, period, rate, [basis])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `cost` | Yes | The original cost of the asset. Must be a positive number representing the total acquisition cost including installation, delivery, and any other costs necessary to put the asset into service. |
| `date_purchased` | Yes | The date the asset was purchased or acquired. This date determines how the first period's depreciation is prorated. Can be entered as a date serial number, a text string in date format, or a cell reference containing a date. |
| `first_period` | Yes | The end date of the first accounting period. This establishes when the fiscal year ends for depreciation calculation purposes. The function uses this to determine prorated depreciation for the first period. |
| `salvage` | Yes | The salvage value (residual value) of the asset at the end of its useful life. Must be 0 or a positive number less than cost. Many tax systems allow salvage value of 0 for fully depreciating assets. |
| `period` | Yes | The period for which you want to calculate depreciation. Period 0 represents the first (partial) period from purchase date to first_period end date. Periods 1, 2, 3... represent subsequent full periods. |
| `rate` | Yes | The annual depreciation rate expressed as a decimal. For example, 0.20 for 20% annual depreciation (5-year life). This is the base rate before the degressive coefficient is applied. |
| `basis` | No | The day count basis to use for date calculations. Default is 0. Options: 0 = US (NASD) 30/360, 1 = Actual/Actual, 2 = Actual/360, 3 = Actual/365, 4 = European 30/360. |

### Return Value

Returns a single numeric value representing the depreciation amount for the specified period, rounded to two decimal places (hence "RC" - rounded to cent). Returns 0 if the asset is fully depreciated in prior periods.

## Examples

> [!f(x)] AMORDEGRC Examples

### Example 1: Basic Degressive Depreciation (First Period)
```
=AMORDEGRC(2400, "2024-08-19", "2024-12-31", 300, 0, 0.15)
```
**Result:** `$776.00`

**Explanation:** Calculates depreciation for a $2,400 asset purchased August 19, 2024, with fiscal year ending December 31, 2024. With a 15% base rate and salvage value of $300, the degressive rate (with coefficient) and first-period proration yields $776.00 for period 0 (the partial first year from purchase to fiscal year end).

---

### Example 2: Full Year Depreciation (Period 1)
```
=AMORDEGRC(2400, "2024-08-19", "2024-12-31", 300, 1, 0.15)
```
**Result:** `$406.00`

**Explanation:** This calculates the depreciation for period 1 (the first full year after the partial first period). The book value at start of period 1 is $2,400 - $776 = $1,624. The degressive depreciation is applied to this remaining value, resulting in $406.00. Note how the amount is smaller than period 0 because the book value has decreased.

---

### Example 3: Equipment with 5-Year Life
```
=AMORDEGRC(50000, "2024-01-15", "2024-12-31", 5000, 0, 0.20)
```
**Result:** `$18,000.00`

**Explanation:** A $50,000 piece of equipment purchased January 15, 2024, with 20% annual depreciation rate (5-year life). The 2.0 coefficient applies (for 5-year assets), making the effective rate 40%. Since purchase was early in January, nearly a full year of depreciation applies in period 0.

---

### Example 4: Mid-Year Purchase
```
=AMORDEGRC(100000, "2024-07-01", "2024-12-31", 10000, 0, 0.20)
```
**Result:** `$20,000.00`

**Explanation:** A $100,000 asset purchased July 1 with 20% rate. The purchase date falls in the second half of the year, so depreciation is calculated for approximately 6 months of the first period. The coefficient adjustment and proration rules determine the exact amount.

---

### Example 5: Subsequent Year Depreciation
```
=AMORDEGRC(100000, "2024-01-01", "2024-12-31", 10000, 2, 0.20)
```
**Result:** Varies based on remaining book value

**Explanation:** Calculates period 2 depreciation for a $100,000 asset. By period 2, the degressive method has already applied accelerated depreciation in periods 0 and 1. The depreciation for period 2 is calculated on the reduced book value at the start of that period.

---

### Example 6: Short-Lived Asset (3-Year Life)
```
=AMORDEGRC(15000, "2024-03-01", "2024-12-31", 0, 0, 0.333)
```
**Result:** `$4,163.00`

**Explanation:** A $15,000 asset with approximately 3-year life (33.3% annual rate) and zero salvage value. For 3-4 year assets, the coefficient is 1.5, making the effective degressive rate approximately 50%. The first period is prorated from March to December.

---

### Example 7: Long-Lived Asset (10-Year Life)
```
=AMORDEGRC(200000, "2024-04-15", "2024-12-31", 20000, 0, 0.10)
```
**Result:** `$35,625.00`

**Explanation:** A $200,000 asset with 10% annual depreciation (10-year life). For assets with 7+ year life, the coefficient is 2.5, making the effective rate 25%. Depreciation is prorated from April 15 to December 31.

---

### Example 8: Using Different Day Count Basis
```
=AMORDEGRC(10000, "2024-06-15", "2024-12-31", 1000, 0, 0.20, 1)
```
**Result:** Varies with basis

**Explanation:** The same calculation using Actual/Actual day count (basis=1) instead of the default 30/360. This can result in slightly different first-period proration depending on the actual number of days in the period.

---

### Example 9: Zero Salvage Value
```
=AMORDEGRC(75000, "2024-02-01", "2024-12-31", 0, 0, 0.20)
```
**Result:** `$27,500.00`

**Explanation:** An asset with zero salvage value means 100% of the cost will be depreciated over its useful life. This is common for tax purposes where assets can be fully written off. The degressive method will accelerate this depreciation into early periods.

---

### Example 10: Comparing Multiple Periods
```
Period 0: =AMORDEGRC(30000, "2024-05-01", "2024-12-31", 3000, 0, 0.20) = $10,800.00
Period 1: =AMORDEGRC(30000, "2024-05-01", "2024-12-31", 3000, 1, 0.20) = $7,680.00
Period 2: =AMORDEGRC(30000, "2024-05-01", "2024-12-31", 3000, 2, 0.20) = $4,608.00
Period 3: =AMORDEGRC(30000, "2024-05-01", "2024-12-31", 3000, 3, 0.20) = $912.00
```
**Result:** Declining amounts over time

**Explanation:** This shows the degressive depreciation pattern: accelerated depreciation in early periods with declining amounts as the book value decreases. Total depreciation over all periods equals $30,000 - $3,000 = $27,000 (cost minus salvage).

---

### Example 11: Fiscal Year Not Calendar Year
```
=AMORDEGRC(50000, "2024-10-15", "2025-06-30", 5000, 0, 0.20)
```
**Result:** Prorated amount for fiscal period

**Explanation:** For companies with non-calendar fiscal years (ending June 30), the first_period parameter reflects this. The depreciation is calculated from purchase date (October 15) to fiscal year end (June 30), spanning approximately 8.5 months.

---

### Example 12: Building Complete Depreciation Schedule
```
=SUMPRODUCT(--(ROW(INDIRECT("1:"&10))-1<=A1), AMORDEGRC($B$1, $B$2, $B$3, $B$4, ROW(INDIRECT("1:"&10))-1, $B$5))
```
**Result:** Cumulative depreciation through specified period

**Explanation:** This formula structure can build a complete depreciation schedule by calculating each period's depreciation. In practice, you would typically create a column with periods 0 through n and apply AMORDEGRC to each row.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date format or text in numeric parameters | Ensure dates are valid date values, not text strings. Verify cost, salvage, period, and rate are numeric. |
| `#NUM!` | Negative values for cost, salvage, or rate; period beyond asset life | Cost, salvage, and rate must be non-negative. Check that period does not exceed the number of depreciation periods. |
| `#NUM!` | Salvage value greater than or equal to cost | Salvage must be less than cost. An asset cannot be worth more at end of life than when purchased. |
| `#NUM!` | Rate is 0.5 or greater (50%+) | AMORDEGRC has restrictions on rate values. The base rate (before coefficient) typically should not exceed certain thresholds. |
| `0 (unexpected)` | Period number exceeds asset's depreciation life | Once the asset is fully depreciated, subsequent periods return 0. Verify your depreciation schedule. |
| `Skipped period` | AMORDEGRC may skip a period due to coefficient rules | This is expected behavior in certain cases. The French degressive method has specific rules that may result in skipped periods. |
| `#NAME?` | Function not recognized (older Excel versions) | Enable the Analysis ToolPak add-in in Excel 2003 and earlier. Excel 2007+ includes this function by default. |

## Use Cases

### [[French Subsidiary Fixed Asset Management]]

**Scenario:** A US-based multinational corporation has a French subsidiary that must report depreciation according to French GAAP (Plan Comptable General) for local statutory reporting while also maintaining US GAAP books.

**Implementation:**
```
French books: =AMORDEGRC(cost, purchase_date, fiscal_year_end, salvage, period, rate)
US books: =SLN(cost, salvage, life) or =DDB(cost, salvage, life, period)
Difference: =French_depreciation - US_depreciation (Book-tax/GAAP difference)
```

**Business Application:** Multinational companies must maintain depreciation calculations under multiple accounting standards. The French subsidiary uses AMORDEGRC for local statutory filings and tax returns, while consolidation into US parent requires reconciliation to US GAAP depreciation. Finance teams track these differences for deferred tax calculations and management reporting.

**Technical Details:** Create parallel depreciation schedules using AMORDEGRC for French requirements and appropriate US functions (SLN, DDB, VDB) for US requirements. Maintain mapping tables for each asset showing depreciation under both methods and calculate temporary differences for deferred tax accounting under ASC 740 or IAS 12.

---

### [[French Tax Compliance for Capital Expenditures]]

**Scenario:** A French company purchases manufacturing equipment for EUR 250,000 and needs to calculate depreciation for tax deduction purposes over the asset's 7-year useful life.

**Implementation:**
```
=AMORDEGRC(250000, DATE(2024,3,15), DATE(2024,12,31), 25000, 0, 1/7)
```
Rate = 1/7 (approximately 0.1429 for 7-year life)
Coefficient = 2.5 (for assets with 7+ year life)
Effective rate = 0.1429 * 2.5 = 0.357 (35.7%)

**Business Application:** French tax law allows businesses to use degressive depreciation for tax purposes, providing greater deductions in early years and improving cash flow by deferring tax payments. AMORDEGRC ensures calculations comply with French tax authority (Direction Generale des Finances Publiques) requirements.

**Technical Details:** The degressive coefficient is determined by asset life: 1.5 for 3-4 years, 2.0 for 5-6 years, 2.5 for 7+ years. Track the purchase date carefully as it affects first-period proration. Maintain supporting documentation including purchase invoices, delivery dates, and asset-in-service dates for audit purposes.

---

### [[Asset Disposal and Partial-Year Depreciation]]

**Scenario:** A company needs to calculate depreciation for an asset that was purchased mid-year and will be disposed of before full depreciation is achieved.

**Implementation:**
```
Year 0 (partial): =AMORDEGRC(cost, purchase_date, year_end, salvage, 0, rate)
Year 1 (full): =AMORDEGRC(cost, purchase_date, year_end, salvage, 1, rate)
Year 2 (disposal): =AMORDEGRC(cost, purchase_date, year_end, salvage, 2, rate) * (disposal_month/12)
```

**Business Application:** When assets are disposed of before full depreciation, companies need accurate partial-year calculations for the disposal year. This affects gain/loss on disposal calculations, tax reporting, and fixed asset register maintenance.

**Technical Details:** AMORDEGRC handles first-period proration automatically. For disposal year, multiply the full-year AMORDEGRC result by the fraction of the year the asset was held. Track accumulated depreciation through disposal to calculate correct book value for gain/loss determination.

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 2007 and later (built-in); Excel 2003 and earlier requires Analysis ToolPak add-in
- **Specific Behavior:** Returns depreciation rounded to two decimal places. Uses French degressive coefficients (1.5, 2.0, 2.5) based on implied asset life from rate.
- **Add-in Requirement:** In Excel 2003 and earlier, go to Tools > Add-Ins > check "Analysis ToolPak"

### Google Sheets

- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. No add-in required.
- **Precision:** Same rounding behavior as Excel (to the cent)

### Both Platforms

- Results are rounded to two decimal places ("RC" = rounded to cent)
- The degressive coefficient is automatically determined based on the depreciation rate
- Period 0 represents the first partial period from purchase date to first_period
- Day count basis affects proration calculations
- Function may return 0 for periods beyond asset life

## Tips and Best Practices

1. **Understand the coefficient system:** AMORDEGRC automatically applies French degressive coefficients (1.5 for 3-4 years, 2.0 for 5-6 years, 2.5 for 7+ years). You provide the base rate; the function applies the multiplier. Do not pre-multiply your rate by the coefficient.

2. **Period 0 is the first partial period:** Unlike many depreciation functions that start with period 1, AMORDEGRC uses period 0 for the first partial period from purchase to fiscal year end. Full years are periods 1, 2, 3, etc.

3. **Match your fiscal year:** The first_period parameter should reflect your actual fiscal year end date, not necessarily December 31. French companies often have different fiscal year ends.

4. **Document the day count basis:** Different basis values can produce different results. Document which basis you use (default is 0 = US 30/360) for audit trail purposes.

5. **Be aware of skipped periods:** The French degressive method may skip a period in certain circumstances due to its calculation rules. This is correct behavior, not an error.

6. **Calculate cumulative depreciation for asset registers:** Use SUMPRODUCT or a helper column to sum depreciation across all periods for accurate accumulated depreciation tracking.

7. **Reconcile to straight-line endpoint:** Total depreciation over all periods should equal cost minus salvage. Verify your depreciation schedule sums correctly.

8. **Consider AMORLINC alternative:** If your French accounting requires straight-line (linear) method rather than degressive, use AMORLINC instead.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[AMORLINC]] | French linear (straight-line) depreciation with proration | When French accounting requires straight-line rather than degressive depreciation |
| [[DDB]] | Double declining balance depreciation | For US GAAP declining balance depreciation without French coefficients |
| [[VDB]] | Variable declining balance depreciation | For flexible declining balance with optional switch to straight-line |
| [[SLN]] | Straight-line depreciation | For simple straight-line depreciation without first-period proration |
| [[SYD]] | Sum-of-years-digits depreciation | For an alternative accelerated depreciation method |

### Commonly Used Together

**[[AMORLINC]]** - French Linear Depreciation

*Compare degressive vs. linear for tax planning:*
```
Degressive: =AMORDEGRC(100000, purchase_date, year_end, 10000, period, 0.20)
Linear: =AMORLINC(100000, purchase_date, year_end, 10000, period, 0.20)
Difference: Higher deductions early with AMORDEGRC, lower later
```
French tax law often allows choice between methods. Compare total present value of tax savings.

---

**[[SUM]]** - Accumulate Depreciation

*Calculate accumulated depreciation through any period:*
```
Accumulated through Period 3:
=AMORDEGRC(..., 0, ...) + AMORDEGRC(..., 1, ...) + AMORDEGRC(..., 2, ...) + AMORDEGRC(..., 3, ...)
```
Track accumulated depreciation for balance sheet reporting and net book value calculations.

---

**[[IF]]** - Conditional Depreciation Method

*Select method based on asset characteristics:*
```
=IF(asset_life >= 7,
    AMORDEGRC(cost, purchase, yearend, salvage, period, rate),
    AMORLINC(cost, purchase, yearend, salvage, period, rate))
```
Apply different depreciation methods based on asset type, life, or regulatory requirements.

---

**[[MAX]]** - Ensure Positive Depreciation

*Handle edge cases where calculation might go negative:*
```
=MAX(0, AMORDEGRC(cost, purchase, yearend, salvage, period, rate))
```
In some edge cases, ensure depreciation does not become negative.

## Official Documentation

- **Microsoft Excel:** [AMORDEGRC function](https://support.microsoft.com/en-us/office/amordegrc-function-a14d0ca1-64a4-42eb-9b3d-b0dce54de3bf)
- **Google Sheets:** [AMORDEGRC function](https://support.google.com/docs/answer/3093154)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2007 (2007) | Built-in function, previously required Analysis ToolPak |
| Excel 2003 and earlier | Analysis ToolPak add-in | Required manual add-in activation |
| Google Sheets | Original launch | Built-in support from the beginning |

---

*Last updated: 2026-01-10*
