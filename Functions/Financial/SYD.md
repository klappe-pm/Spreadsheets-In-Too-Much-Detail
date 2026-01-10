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
- sum-of-years-digits
- accelerated-depreciation
- asset-management
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Sum of Years Digits
- Sum of the Years Digits Depreciation
- SYD Depreciation
tags:
- financial-modeling
- accounting
- depreciation
- tax-planning
---

# SYD

## Description

**SYD (Sum-of-Years-Digits Depreciation)** calculates depreciation using the sum-of-years-digits method, which provides accelerated depreciation that declines in a smooth, predictable pattern. The method assigns a fraction to each year based on the remaining life of the asset divided by the sum of all the years' digits. For a 5-year asset, the sum is 1+2+3+4+5=15, so year 1 gets 5/15 of depreciation, year 2 gets 4/15, and so on.

This method provides moderate acceleration compared to double declining balance (DDB)--not as aggressive in early years but with a more predictable declining pattern throughout the asset's life. Unlike DDB, SYD always completely depreciates the asset to its salvage value, with no residual book value issues. The depreciation each year can be calculated directly without needing the previous year's book value.

**Sign conventions are straightforward:** SYD returns a positive number representing the depreciation expense for the specified period. All inputs should be positive values, with salvage being zero or positive. The result decreases linearly each year, providing a predictable pattern for budgeting and planning.

SYD is available in all versions of Excel and Google Sheets with identical behavior. The function is useful when you want accelerated depreciation that is less aggressive than DDB, when you need to fully depreciate assets (including zero salvage), or when you prefer a simple, predictable declining pattern for financial planning.

## Syntax

> [!f(x)] SYD Syntax
>
> ```
> =SYD(cost, salvage, life, period)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `cost` | Yes | The initial cost of the asset. Must be positive. |
| `salvage` | Yes | The salvage value at end of useful life. Can be 0 (fully depreciable). Must be >= 0 and < cost. |
| `life` | Yes | The number of periods over which the asset is depreciated. Must be positive integer. |
| `period` | Yes | The period for which you want the depreciation. Must be between 1 and life inclusive. |

### Return Value

Returns the depreciation expense for the specified period as a positive number. Depreciation decreases linearly each year in equal increments. Total depreciation over all periods equals cost minus salvage.

## Examples

> [!f(x)] SYD Examples

### Example 1: First Year SYD Depreciation
```
=SYD(100000, 10000, 5, 1)
```
**Result:** `$30,000.00`

**Explanation:** For a 5-year asset, sum of years = 1+2+3+4+5 = 15. Year 1 fraction = 5/15 = 1/3. Depreciation = (100000 - 10000) x (5/15) = $90,000 x 1/3 = $30,000. This is accelerated but less than DDB's $40,000.

---

### Example 2: Complete SYD Schedule
```
Year 1: =SYD(100000, 10000, 5, 1) = $30,000.00 (5/15)
Year 2: =SYD(100000, 10000, 5, 2) = $24,000.00 (4/15)
Year 3: =SYD(100000, 10000, 5, 3) = $18,000.00 (3/15)
Year 4: =SYD(100000, 10000, 5, 4) = $12,000.00 (2/15)
Year 5: =SYD(100000, 10000, 5, 5) = $6,000.00 (1/15)
Total: $90,000.00 (= cost - salvage)
```
**Result:** Linear decrease totaling exactly the depreciable base

**Explanation:** SYD decreases by exactly $6,000 each year (90,000/15 = 6,000 per "digit"). This predictable pattern makes budgeting straightforward while still providing acceleration.

---

### Example 3: Zero Salvage Value
```
=SYD(50000, 0, 4, 1)
```
**Result:** `$20,000.00`

**Explanation:** With zero salvage, the full cost is depreciated. Sum = 1+2+3+4 = 10. Year 1 = $50,000 x (4/10) = $20,000. Unlike DB, SYD handles zero salvage without issues.

---

### Example 4: Compare SYD to Other Methods
```
Year 1 on $100,000 asset, $10,000 salvage, 5-year life:
SYD:  =SYD(100000, 10000, 5, 1) = $30,000 (5/15 of $90,000)
DDB:  =DDB(100000, 10000, 5, 1) = $40,000 (40% of $100,000)
DB:   =DB(100000, 10000, 5, 1)  = $36,900 (36.9% of $100,000)
SLN:  =SLN(100000, 10000, 5)    = $18,000 ($90,000/5)
```
**Result:** SYD provides moderate acceleration

**Explanation:** SYD ($30,000) falls between aggressive DDB ($40,000) and even SLN ($18,000). It is a middle-ground accelerated method, providing tax benefits without the extreme front-loading of double declining.

---

### Example 5: Calculating the Sum
```
5-year asset: Sum = 1+2+3+4+5 = 15
Formula: Sum = n(n+1)/2 = 5(6)/2 = 15

10-year asset: Sum = 10(11)/2 = 55
Year 1 fraction: 10/55 = 0.1818
```
**Result:** Sum-of-years formula is n(n+1)/2

**Explanation:** You can calculate the sum directly using the formula n(n+1)/2 rather than adding all numbers. This helps verify SYD calculations for any asset life.

---

### Example 6: Book Value Tracking
```
Cost: $100,000, Salvage: $10,000, Life: 5 years
Year 1: BV = $100,000 - $30,000 = $70,000
Year 2: BV = $70,000 - $24,000 = $46,000
Year 3: BV = $46,000 - $18,000 = $28,000
Year 4: BV = $28,000 - $12,000 = $16,000
Year 5: BV = $16,000 - $6,000 = $10,000 (salvage)
```
**Result:** Book value reaches exactly salvage

**Explanation:** Unlike DDB, which may leave residual book value, SYD always reaches exactly the salvage value. This is because the fractions are designed to sum to 1.

---

### Example 7: Long-Life Asset
```
=SYD(500000, 50000, 10, 1)
```
**Result:** `$81,818.18`

**Explanation:** For 10-year life, sum = 55. Year 1 = ($500,000 - $50,000) x (10/55) = $450,000 x 0.1818 = $81,818.18. The longer the life, the smaller the annual fraction.

---

### Example 8: Tax Shield Comparison
```
SYD Year 1 Tax Shield (at 25%): $30,000 * 0.25 = $7,500
SLN Year 1 Tax Shield: $18,000 * 0.25 = $4,500
SYD advantage: $3,000 more tax savings in year 1
```
**Result:** SYD provides 67% more tax savings in year 1 than SLN

**Explanation:** Accelerated depreciation creates larger early tax deductions. While total tax savings is identical over asset life, getting savings sooner has time value of money benefits.

---

### Example 9: Vehicle Fleet Depreciation
```
Fleet cost: $500,000, Salvage: $75,000, Life: 5 years
Year 1: =SYD(500000, 75000, 5, 1) = $141,666.67
Year 3: =SYD(500000, 75000, 5, 3) = $85,000.00
Year 5: =SYD(500000, 75000, 5, 5) = $28,333.33
```
**Result:** Predictable declining pattern for budgeting

**Explanation:** The linear decrease ($28,333.33 per year) makes depreciation expense predictable for budgeting. Finance teams know exactly how depreciation will change each year.

---

### Example 10: Partial Period Consideration
```
Mid-year acquisition: Asset bought July 1
Year 1 (6 months): =SYD(100000, 10000, 5, 1) * 6/12 = $15,000
Year 2 (full year): weighted average of Year 1 and Year 2
```
**Result:** SYD does not have built-in partial period support

**Explanation:** Unlike DB function, SYD does not have a month parameter. For partial periods, manually prorate the first and last year depreciation.

---

### Example 11: Direct Calculation Formula
```
SYD depreciation = (Cost - Salvage) * (Remaining Life / Sum of Years)
Year 3 of 5: = $90,000 * (3/15) = $18,000
Verify: =SYD(100000, 10000, 5, 3) = $18,000
```
**Result:** Manual calculation matches function

**Explanation:** SYD can be calculated manually: (Cost - Salvage) x (Life - Period + 1) / (Life x (Life + 1) / 2). This transparency makes SYD easy to audit and verify.

---

### Example 12: Annual Depreciation Decrease
```
Depreciation decrease per year = (Cost - Salvage) / Sum of Years
= $90,000 / 15 = $6,000 decrease each year

Year 1: $30,000
Year 2: $30,000 - $6,000 = $24,000
Year 3: $24,000 - $6,000 = $18,000
```
**Result:** Depreciation decreases by a constant amount each year

**Explanation:** The linear decrease makes SYD unique among accelerated methods. Each year's depreciation is exactly $6,000 less than the previous year, creating a perfectly predictable pattern.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Period > Life | Period must be between 1 and life inclusive. |
| `#NUM!` | Salvage >= Cost | Salvage must be less than cost for depreciation to occur. |
| `#NUM!` | Life or period <= 0 | Life and period must be positive values. |
| `#VALUE!` | Non-numeric inputs | Ensure all parameters are numbers. Check for text in cells. |
| `Unexpected result` | Period/Life mismatch | Remember that period 1 gets the largest depreciation (remaining life = full life). |

## Use Cases

### [[Predictable Depreciation Budgeting]]

**Scenario:** A manufacturing company needs to forecast depreciation expense for the next 5 years for budgeting purposes, preferring accelerated depreciation with a predictable pattern.

**Implementation:**
```
Equipment: $2,000,000 cost, $200,000 salvage, 5-year life
Depreciable base: $1,800,000
Sum of years: 15
Annual decrease: $1,800,000 / 15 = $120,000 per year

Depreciation Forecast:
Year 1: =SYD(2000000, 200000, 5, 1) = $600,000
Year 2: =SYD(2000000, 200000, 5, 2) = $480,000
Year 3: =SYD(2000000, 200000, 5, 3) = $360,000
Year 4: =SYD(2000000, 200000, 5, 4) = $240,000
Year 5: =SYD(2000000, 200000, 5, 5) = $120,000
```

**Business Application:** CFOs and budget analysts prefer SYD when they want accelerated depreciation but also need predictability for multi-year budgets. The constant annual decrease ($120,000) makes forecasting straightforward.

**Technical Details:** Unlike DDB which may stop depreciating mid-life or leave residual value, SYD provides a complete, predictable schedule that sums exactly to the depreciable base. This simplifies financial planning and audit verification.

---

### [[Tax and Book Depreciation Comparison]]

**Scenario:** An accountant needs to maintain both book depreciation (SYD for GAAP) and tax depreciation (MACRS) for a major asset, tracking the deferred tax liability created by timing differences.

**Implementation:**
```
Equipment: $500,000, Salvage: $50,000, 5-year life

Book Depreciation (SYD):
Year 1: =SYD(500000, 50000, 5, 1) = $150,000

Tax Depreciation (MACRS 5-year, approximate):
Year 1: $500,000 * 20% = $100,000 (half-year convention)

Timing Difference: $150,000 - $100,000 = $50,000
Deferred Tax Asset: $50,000 * 25% = $12,500
```

**Business Application:** Companies often use different depreciation methods for book (financial reporting) versus tax purposes. SYD is acceptable under GAAP and provides accelerated depreciation for book purposes. Tracking differences creates deferred tax assets or liabilities.

**Technical Details:** Book depreciation affects reported earnings; tax depreciation affects cash taxes paid. Different methods create temporary differences that reverse over the asset's life. The deferred tax account tracks these differences.

---

### [[Equipment Replacement Planning]]

**Scenario:** A facilities manager is planning equipment replacements and needs to understand when assets will be fully depreciated under the SYD method to time purchases and budget for replacements.

**Implementation:**
```
HVAC System: $300,000, $30,000 salvage, 10-year life, installed 4 years ago

Depreciation schedule (Sum = 55):
Year 5: =SYD(300000, 30000, 10, 5) = $29,454.55
Year 6: =SYD(300000, 30000, 10, 6) = $24,545.45
...
Year 10: =SYD(300000, 30000, 10, 10) = $4,909.09

Accumulated through Year 4: $150,545.45
Current book value: $149,454.55
Remaining depreciation: $149,454.55 - $30,000 = $119,454.55

Replacement timeline: 6 more years to full depreciation
Annual depreciation remaining: Average $19,909 per year
```

**Business Application:** Facilities managers use depreciation schedules to plan capital replacements. Knowing book value helps with sale/disposal decisions, and knowing future depreciation expense helps with budget planning.

**Technical Details:** Book value and market value often differ. An asset with $149K book value might be worth more or less on the market. Replacement decisions should consider both financial (depreciation) and operational (performance, maintenance costs) factors.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Uses exact fraction calculation. No rounding issues.
- **Precision:** IEEE 754 double-precision floating point.

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel.
- **Precision:** Same as Excel.

### Both Platforms
- Handles zero salvage value without issues
- Returns positive depreciation expense
- Depreciation decreases by constant amount each period
- Total depreciation equals exactly (cost - salvage)
- No partial period parameter (manual proration required)

## Tips and Best Practices

1. **Understand the pattern:** SYD decreases by a constant amount each year, equal to (cost - salvage) / sum of years. This linear decrease is unique among accelerated methods.

2. **Calculate the sum easily:** Sum of years = n(n+1)/2, where n = life. For 5 years: 5(6)/2 = 15. For 10 years: 10(11)/2 = 55.

3. **Use for complete depreciation:** SYD always fully depreciates to salvage, unlike DDB which may leave residual. Choose SYD when you need guaranteed full depreciation.

4. **Handle partial years manually:** SYD does not have a month parameter. Prorate first and last year depreciation manually for mid-year acquisitions.

5. **Compare to DDB for method selection:** SYD provides moderate acceleration; DDB is more aggressive. Model both to see which better matches your asset's actual value decline and tax strategy.

6. **Verify with sum:** Sum all SYD results to verify they equal (cost - salvage). This is a simple validation check.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DDB]] | Double declining balance | For more aggressive early depreciation |
| [[DB]] | Fixed declining balance | When you need to hit salvage exactly (non-zero salvage) |
| [[SLN]] | Straight-line | For even depreciation each period |
| [[VDB]] | Variable declining balance | For partial periods or method switching |

### Commonly Used Together

**[[DDB]]** - Compare Acceleration Levels

*Method comparison:*
```
SYD Year 1: =SYD(100000, 10000, 5, 1) = $30,000
DDB Year 1: =DDB(100000, 10000, 5, 1) = $40,000
SYD is 75% of DDB's first-year depreciation
```
Compare accelerated methods to select appropriate aggressiveness.

---

**[[SLN]]** - Compare to Straight-Line

*Acceleration ratio:*
```
SYD Year 1: =SYD(100000, 10000, 5, 1) = $30,000
SLN: =SLN(100000, 10000, 5) = $18,000
SYD is 167% of straight-line in year 1
```
Quantify the acceleration benefit of SYD over straight-line.

---

**[[NPV]]** - Tax Shield Valuation

*Present value of tax savings:*
```
SYD tax shields: {$7500, $6000, $4500, $3000, $1500} at 25% tax rate
=NPV(0.1, 7500, 6000, 4500, 3000, 1500) = $17,907

SLN tax shields: {$4500, $4500, $4500, $4500, $4500}
=NPV(0.1, 4500, 4500, 4500, 4500, 4500) = $17,053

SYD NPV advantage: $854 (5% higher)
```
Calculate the present value advantage of accelerated depreciation.

---

**[[SUM]]** - Verify Total Depreciation

*Validation check:*
```
=SYD(100000,10000,5,1)+SYD(100000,10000,5,2)+SYD(100000,10000,5,3)+
 SYD(100000,10000,5,4)+SYD(100000,10000,5,5)
= $90,000 = $100,000 - $10,000
```
Verify that total SYD depreciation equals the depreciable base.

## Official Documentation

- **Microsoft Excel:** [SYD function](https://support.microsoft.com/en-us/office/syd-function-069f8106-b60b-4ca2-98e0-2a0f206bdb27)
- **Google Sheets:** [SYD function](https://support.google.com/docs/answer/3093231)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 (1997) | Original implementation |
| Excel 2007+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
