---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
- excel-specific
subTopics:
- depreciation
- asset-management
- tax-accounting
- fixed-assets
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Variable Declining Balance
- Variable Depreciation
- Flexible Depreciation
tags:
- financial-modeling
- depreciation
- accounting
- fixed-assets
- tax-planning
- capital-assets
---

# VDB

## Description

**VDB (Variable Declining Balance)** calculates depreciation for any specified period using the double-declining balance method, with an optional switch to straight-line when that becomes more advantageous. It's the most flexible depreciation function in Excel and Sheets, allowing you to calculate depreciation for partial periods, custom date ranges, and non-standard depreciation factors. If you need depreciation for "month 15 through month 18" of an asset's life, or want to model 150% declining balance instead of 200%, VDB is the function to use.

The function's power lies in its flexibility with period specification. Unlike DDB (which calculates depreciation for a single whole period), VDB accepts fractional period boundaries. You can calculate depreciation for period 2.5 to 3.5, for the first quarter of year 3, or for any arbitrary slice of the asset's life. This makes VDB essential for mid-year asset additions, fiscal year calculations that don't align with purchase dates, or detailed month-by-month depreciation schedules.

**Key feature: the no_switch parameter.** By default, VDB automatically switches from declining balance to straight-line depreciation when straight-line would yield a higher deduction—this is the MACRS-style approach used in U.S. tax depreciation. Setting no_switch to TRUE forces pure declining balance throughout, which never fully depreciates the asset (always leaves residual value). Most real-world scenarios want the default behavior (FALSE or omitted), but understanding this switch is crucial for matching specific accounting policies.

VDB is available in Excel (built-in since Excel 2007, Analysis ToolPak in earlier versions) and Google Sheets (native support). Both platforms produce identical results. For simpler depreciation needs, consider SLN (straight-line), DDB (double-declining balance for whole periods), or SYD (sum-of-years-digits). VDB shines when you need flexibility in period specification or factor customization.

## Syntax

> [!f(x)] VDB Syntax
>
> ```
> =VDB(cost, salvage, life, start_period, end_period, [factor], [no_switch])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `cost` | Yes | Original cost of the asset. Enter as a positive number. |
| `salvage` | Yes | Salvage value at the end of the asset's useful life. Use 0 if asset will have no residual value. |
| `life` | Yes | Total useful life of the asset (number of periods). Must match the period units used in start/end. |
| `start_period` | Yes | Starting period for depreciation calculation. Use 0 for the beginning of the first period. Can be fractional. |
| `end_period` | Yes | Ending period for depreciation calculation. Must be > start_period. Can be fractional. |
| `factor` | No | Rate at which the balance declines. Default is 2 (double-declining = 200%). Use 1.5 for 150% declining balance. |
| `no_switch` | No | FALSE (default) = switch to straight-line when advantageous. TRUE = never switch, pure declining balance throughout. |

### Return Value

Returns the depreciation amount for the specified period range. Always a positive number (expense). Returns 0 if start_period equals end_period or if asset is fully depreciated before the period.

## Examples

> [!f(x)] VDB Examples

### Example 1: Basic Full-Year Depreciation (Year 1)
```
=VDB(50000, 5000, 5, 0, 1)
```
**Result:** `$20,000.00`

**Explanation:** A $50,000 asset with $5,000 salvage over 5 years. First full year (period 0 to 1) depreciation using double-declining balance: 2/5 = 40% x $50,000 = $20,000. This matches DDB for the first period.

---

### Example 2: Second Year Depreciation
```
=VDB(50000, 5000, 5, 1, 2)
```
**Result:** `$12,000.00`

**Explanation:** Year 2 depreciation. Book value after year 1 is $30,000. Double-declining rate (40%) x $30,000 = $12,000. The declining book value means each year's depreciation decreases.

---

### Example 3: Mid-Year Asset Addition (First Partial Year)
```
=VDB(24000, 2000, 5, 0, 0.5)
```
**Result:** `$4,800.00`

**Explanation:** Asset purchased mid-year, so only half a year of depreciation in year 1. VDB calculates depreciation from period 0 to 0.5 (half year). Full year would be $9,600, so half year is $4,800.

---

### Example 4: Monthly Depreciation Calculation
```
=VDB(120000, 10000, 60, 0, 1)
```
**Result:** `$4,000.00`

**Explanation:** $120,000 asset over 60 months (5 years expressed in months). First month's depreciation. With 60 periods, rate is 2/60 = 3.33% per month. First month: $120,000 x 0.0333 = $4,000.

---

### Example 5: Cumulative Depreciation (Year 1-3)
```
=VDB(50000, 5000, 5, 0, 3)
```
**Result:** `$38,560.00`

**Explanation:** Total depreciation from beginning through end of year 3. VDB calculates cumulative depreciation across multiple periods in one formula—useful for calculating accumulated depreciation.

---

### Example 6: 150% Declining Balance
```
=VDB(80000, 8000, 10, 0, 1, 1.5)
```
**Result:** `$12,000.00`

**Explanation:** Using 150% declining balance (factor = 1.5) instead of 200% (default). Rate = 1.5/10 = 15%. First year: $80,000 x 0.15 = $12,000. Common for longer-lived assets.

---

### Example 7: Pure Declining Balance (No Switch)
```
=VDB(50000, 5000, 5, 4, 5, 2, TRUE)
```
**Result:** `$3,240.00`

**Explanation:** With no_switch = TRUE, depreciation continues using declining balance even in the final year. Compare to no_switch = FALSE which would switch to straight-line to fully depreciate to salvage value.

---

### Example 8: Final Year with Automatic Switch to Straight-Line
```
=VDB(50000, 5000, 5, 4, 5, 2, FALSE)
```
**Result:** `$5,440.00`

**Explanation:** With no_switch = FALSE (default), VDB switches to straight-line in the final period to ensure the asset fully depreciates to salvage value. The switch happens when straight-line yields higher depreciation.

---

### Example 9: Quarterly Depreciation
```
=VDB(100000, 10000, 20, 0, 1)
```
Where 20 = 5 years x 4 quarters per year

**Result:** `$10,000.00`

**Explanation:** For quarterly reporting, express life in quarters. A 5-year asset has 20 quarters. First quarter depreciation: 2/20 = 10% x $100,000 = $10,000.

---

### Example 10: Specific Quarter in Year 3
```
=VDB(100000, 10000, 20, 8, 9)
```
**Result:** `$4,289.82`

**Explanation:** Quarter 9 (first quarter of year 3) in a 20-quarter schedule. VDB calculates just this specific quarter's depreciation. Book value has declined significantly by Q9.

---

### Example 11: Fiscal Year Crossing Calendar Year
```
=VDB(60000, 6000, 5, 0.5, 1.5)
```
**Result:** `$19,200.00`

**Explanation:** Fiscal year running from mid-year 1 to mid-year 2. VDB handles this fractional period: depreciation for the 12-month fiscal year starting 6 months after asset acquisition.

---

### Example 12: Complete Depreciation Schedule
```
Year 1: =VDB(50000, 5000, 5, 0, 1)  → $20,000.00
Year 2: =VDB(50000, 5000, 5, 1, 2)  → $12,000.00
Year 3: =VDB(50000, 5000, 5, 2, 3)  → $7,200.00
Year 4: =VDB(50000, 5000, 5, 3, 4)  → $3,360.00
Year 5: =VDB(50000, 5000, 5, 4, 5)  → $2,440.00
Total: $45,000.00 (= $50,000 - $5,000 salvage)
```
**Result:** Complete 5-year schedule

**Explanation:** Full depreciation schedule year by year. Total equals cost minus salvage ($45,000). Note how later years show straight-line switching to fully depreciate the asset.

---

### Example 13: Zero Salvage Value
```
=VDB(30000, 0, 6, 0, 1)
```
**Result:** `$10,000.00`

**Explanation:** When salvage value is zero, the entire cost is depreciated. Double-declining rate: 2/6 = 33.33%. First year: $30,000 x 0.333 = $10,000.

---

### Example 14: Comparing Depreciation Methods
```
VDB (200% DB):   =VDB(100000, 10000, 10, 0, 1, 2)    → $20,000
VDB (150% DB):   =VDB(100000, 10000, 10, 0, 1, 1.5)  → $15,000
Straight-line:   =SLN(100000, 10000, 10)             → $9,000
```
**Result:** Comparison of methods

**Explanation:** Same asset, different depreciation approaches. Double-declining front-loads depreciation. 150% DB is more moderate. Straight-line spreads evenly. Choose based on tax strategy and accounting policy.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | start_period < 0 or end_period < 0 | Periods must be >= 0. Start_period = 0 means beginning of first period. |
| `#NUM!` | end_period <= start_period | End must be greater than start. For period 1, use start=0, end=1. |
| `#NUM!` | life <= 0 | Life must be positive. Check that life matches your period units. |
| `#NUM!` | factor < 0 | Factor must be positive. Use 2 for double-declining, 1.5 for 150% declining. |
| `#VALUE!` | Non-numeric inputs | Ensure all parameters are numbers. Check for text formatting. |
| Wrong result | Life units don't match period units | If life is in years, periods are years. If life is 60 (months), periods are months. |
| Zero result | Asset fully depreciated before specified period | Check accumulated depreciation to ensure asset still has remaining value. |

## Use Cases

### [[MACRS Depreciation Simulation]]

**Scenario:** A tax accountant needs to calculate MACRS-style depreciation with mid-quarter convention for assets placed in service at different times during the year.

**Implementation:**
```
Asset placed in service in Q1 (1.5 months into year):
First year: =VDB(cost, 0, 5, 0, 1-(1.5/12), 2)
Second year: =VDB(cost, 0, 5, 1-(1.5/12), 2-(1.5/12), 2)
...and so on

Or with mid-year convention:
First year: =VDB(cost, 0, 5, 0, 0.5, 2)
Second year: =VDB(cost, 0, 5, 0.5, 1.5, 2)
```

**Business Application:** Tax preparers use VDB to model depreciation that matches IRS MACRS requirements, including half-year and mid-quarter conventions. This enables accurate tax projections and compliance.

**Technical Details:** MACRS typically uses 200% declining balance with switch to straight-line (VDB's default no_switch = FALSE). Different asset classes have different lives: 5-year, 7-year, 15-year, etc.

---

### [[Monthly Fixed Asset Reporting]]

**Scenario:** A financial controller needs monthly depreciation amounts for accurate monthly financial statements, even for assets purchased mid-month.

**Implementation:**
```
Asset: $120,000, 5-year life, no salvage
Express life in months: 60 months

Month 1 (purchased on 15th): =VDB(120000, 0, 60, 0, 0.5, 2)  → half-month
Month 2: =VDB(120000, 0, 60, 0.5, 1.5, 2)                    → full month
Month 3: =VDB(120000, 0, 60, 1.5, 2.5, 2)                    → full month
...continuing for 60 months
```

**Business Application:** Monthly financial close requires precise depreciation for accurate P&L and balance sheet. VDB enables true monthly depreciation that accounts for acquisition timing.

**Technical Details:** Build a depreciation table with cumulative start and end periods. Use absolute cell references for cost and life, incrementing periods for each row.

---

### [[Capital Budgeting with Accelerated Depreciation]]

**Scenario:** A CFO wants to compare investment scenarios using different depreciation methods to understand cash flow timing and tax implications.

**Implementation:**
```
Investment: $500,000 equipment, 7-year life, $50,000 salvage

200% Declining Balance (aggressive):
=VDB(500000, 50000, 7, 0, 1, 2)  → Year 1: $142,857

150% Declining Balance (moderate):
=VDB(500000, 50000, 7, 0, 1, 1.5)  → Year 1: $107,143

Straight-Line (conservative):
=SLN(500000, 50000, 7)  → Year 1: $64,286
```

**Business Application:** Capital investment decisions consider tax timing. Accelerated depreciation provides larger early deductions, improving project NPV. VDB enables scenario modeling with different depreciation approaches.

**Technical Details:** For NPV calculations, build out all years of depreciation, apply tax rate, and discount cash flows. VDB's year-by-year output feeds directly into DCF models.

## Platform Differences

### Microsoft Excel
- **Availability:** Built-in since Excel 2007. Earlier versions require Analysis ToolPak add-in.
- **Specific Behavior:** All five required parameters must be provided. Optional factor defaults to 2, no_switch defaults to FALSE.

### Google Sheets
- **Availability:** Native support since original launch. No add-in required.
- **Specific Behavior:** Identical to Excel. Same parameter requirements and defaults.

### Both Platforms
- Handle fractional periods correctly
- Automatically switch to straight-line when no_switch is FALSE or omitted
- Return 0 for periods after asset is fully depreciated
- Use the same calculation methodology

## Tips and Best Practices

1. **Use 0 for start_period, not 1:** Period 0 is the beginning of the first period. `VDB(..., 0, 1, ...)` gives first period depreciation. `VDB(..., 1, 2, ...)` gives second period.

2. **Match life units to period units:** If life is 5 (years) and you want monthly depreciation, use life = 60 (months) and period units in months. Don't mix annual life with monthly periods.

3. **Validate with cumulative check:** Sum of all period depreciation should equal (cost - salvage). Use `=VDB(cost, salvage, life, 0, life)` to verify total depreciation.

4. **Use no_switch = FALSE for tax depreciation:** U.S. MACRS switches to straight-line automatically. Only use TRUE for pure declining balance analysis.

5. **Build depreciation schedules with calculated periods:** In row 1, use start=0, end=1. In row 2, reference row 1's end as start. This creates an error-free cascading schedule.

6. **Consider factor carefully:** Default factor of 2 (200% DB) is aggressive. Use 1.5 for longer-lived assets or when 150% declining balance is preferred.

7. **Document your assumptions:** Note the depreciation method, convention (mid-year, mid-quarter), and any company-specific policies.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DDB]] | Double-declining balance for single whole periods | When you only need whole period depreciation and don't need period flexibility |
| [[SLN]] | Straight-line depreciation | When using straight-line method with equal annual amounts |
| [[SYD]] | Sum-of-years-digits depreciation | When using sum-of-years-digits accelerated depreciation |
| [[DB]] | Fixed-declining balance depreciation | When using fixed (not double) declining balance method |

### Commonly Used Together

**[[SLN]]** - Straight-Line Depreciation

*Combined for method comparison:*
```
Accelerated (VDB): =VDB(100000, 10000, 10, 0, 1)  → $20,000
Straight-line (SLN): =SLN(100000, 10000, 10)     → $9,000
First-year difference: $11,000
```
Compare depreciation methods to optimize tax strategy and financial reporting.

---

**[[DDB]]** - Double-Declining Balance

*Combined for validation:*
```
VDB first period: =VDB(50000, 5000, 5, 0, 1)  → $20,000
DDB first period: =DDB(50000, 5000, 5, 1)     → $20,000
```
VDB and DDB should match for first whole period. Use this to verify your VDB setup.

---

**[[NPV]]** - Net Present Value

*Combined for capital budgeting:*
```
Year 1 tax savings: =VDB(cost, salvage, life, 0, 1) * tax_rate
Year 2 tax savings: =VDB(cost, salvage, life, 1, 2) * tax_rate
...build out all years, then:
=NPV(discount_rate, year1_savings:yearN_savings)
```
Calculate present value of depreciation tax shields for investment analysis.

---

**[[IF]]** - Conditional Logic

*Combined for reporting switches:*
```
=IF(VDB(cost, salvage, life, period-1, period) > 0,
    VDB(cost, salvage, life, period-1, period),
    "Fully Depreciated")
```
Handle the end of asset life gracefully in reports.

---

**[[SUM]]** - Accumulated Depreciation

*Combined for balance sheet:*
```
Accumulated Depreciation through Period N:
=VDB(cost, salvage, life, 0, period_number)
Or: =SUM of all individual period VDB calculations
```
Both approaches give accumulated depreciation for asset book value calculation.

## Official Documentation

- **Microsoft Excel:** [VDB function](https://support.microsoft.com/en-us/office/vdb-function-dde4e207-f3fa-488d-91d2-66d55e861d73)
- **Google Sheets:** [VDB function](https://support.google.com/docs/answer/3093272)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2003 & earlier | Analysis ToolPak add-in | Required manual add-in installation |
| Excel 2007+ | Built-in function | No add-in required |
| Excel 2010+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Native support, identical to Excel |

---

*Last updated: 2026-01-10*
