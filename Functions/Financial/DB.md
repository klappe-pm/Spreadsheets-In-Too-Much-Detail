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
- fixed-declining-balance
- asset-valuation
- tax-accounting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Declining Balance
- Fixed Declining Balance
- DB Depreciation
tags:
- depreciation
- asset-management
- tax-planning
- accounting
---

# DB

## Description

**DB (Declining Balance)** calculates depreciation using the fixed-declining balance method, where an asset loses a fixed percentage of its remaining book value each year. This creates larger depreciation expenses in early years that decline over time, reflecting how many assets lose value more rapidly when new. The DB function is specifically designed for tax and accounting purposes, using a formula that ensures the asset depreciates to its salvage value over the specified life.

The fixed-declining balance method calculates depreciation by applying a constant rate to the declining book value. Excel and Google Sheets calculate this rate as: rate = 1 - ((salvage / cost) ^ (1 / life)), rounded to three decimal places. This rate is then applied to the beginning-of-year book value to determine each period's depreciation. The first and last years receive special handling to account for partial-year acquisitions--the first year is prorated by month, and the final year captures any remaining value.

**Understanding the calculation is essential:** DB calculates a fixed rate that, when applied annually, reduces the asset from cost to salvage value over the life. The month parameter adjusts the first year's depreciation for mid-year acquisitions. If you buy equipment in July (month 7), the first year gets only 6/12 of a full year's depreciation, with the remainder spread across an additional partial year at the end. This matches how assets are actually depreciated for tax purposes.

DB is available in all versions of Excel and Google Sheets with identical behavior. Unlike DDB (double declining balance) which uses a factor of 2 or custom factor, DB calculates the mathematically precise rate needed to reach the salvage value. For assets that lose value rapidly early on--vehicles, computers, machinery--DB provides more realistic expense matching than straight-line depreciation while being simpler than MACRS schedules used in US tax law.

## Syntax

> [!f(x)] DB Syntax
>
> ```
> =DB(cost, salvage, life, period, [month])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `cost` | Yes | The initial cost of the asset. Must be a positive number. |
| `salvage` | Yes | The value at the end of the depreciation period (residual value). Must be >= 0. |
| `life` | Yes | The number of periods over which the asset is depreciated (useful life in years). Must be positive. |
| `period` | Yes | The period for which you want to calculate depreciation. Must be between 1 and life (or life+1 if month < 12). |
| `month` | No | The number of months in the first year. Defaults to 12. Use values 1-12 for partial first years. |

### Return Value

Returns the depreciation amount for the specified period as a positive number. The result represents the expense that can be deducted in that period.

## Examples

> [!f(x)] DB Examples

### Example 1: Basic Full-Year Depreciation
```
Cost: $10,000
Salvage: $1,000
Life: 5 years
Period: 1

=DB(10000, 1000, 5, 1)
```
**Result:** `$3,690.00`

**Explanation:** The function calculates the depreciation rate as 1 - (1000/10000)^(1/5) = 0.369. First year depreciation: $10,000 * 0.369 = $3,690. This high first-year expense reflects accelerated depreciation.

---

### Example 2: Second Year Depreciation
```
Cost: $10,000
Salvage: $1,000
Life: 5 years
Period: 2

=DB(10000, 1000, 5, 2)
```
**Result:** $2,328.39

**Explanation:** Year 2 book value: $10,000 - $3,690 = $6,310. Year 2 depreciation: $6,310 * 0.369 = $2,328.39. Each year's depreciation is smaller because it's calculated on the declining book value.

---

### Example 3: Complete Depreciation Schedule
```
=DB(10000, 1000, 5, 1) = $3,690.00
=DB(10000, 1000, 5, 2) = $2,328.39
=DB(10000, 1000, 5, 3) = $1,469.21
=DB(10000, 1000, 5, 4) = $926.88
=DB(10000, 1000, 5, 5) = $584.66
Total: $9,000.00 (equals cost minus salvage)
```
**Result:** Complete schedule totaling $9,000

**Explanation:** The sum of all depreciation periods equals the depreciable base (cost - salvage). The DB method automatically calculates the rate to achieve this exact result.

---

### Example 4: Mid-Year Acquisition (July Purchase)
```
Cost: $50,000
Salvage: $5,000
Life: 7 years
Month: 6 (purchased in July, 6 months remaining in year)

=DB(50000, 5000, 7, 1, 6)
```
**Result:** $10,714.29 (prorated for 6 months)

**Explanation:** With only 6 months in the first year, depreciation is $21,428.57 * (6/12) = $10,714.29. The remaining half-year of depreciation shifts to an eighth period.

---

### Example 5: Final Partial Year
```
=DB(50000, 5000, 7, 8, 6)
```
**Result:** Remaining depreciation in year 8

**Explanation:** When month < 12, there's a partial year at the end. Period 8 captures the remaining depreciation to bring the asset to salvage value exactly.

---

### Example 6: Vehicle Depreciation
```
Vehicle cost: $35,000
Trade-in value after 5 years: $8,000
Life: 5 years

Year 1: =DB(35000, 8000, 5, 1) = $11,515.00
Year 2: =DB(35000, 8000, 5, 2) = $7,727.86
Year 3: =DB(35000, 8000, 5, 3) = $5,186.23
Year 4: =DB(35000, 8000, 5, 4) = $3,479.96
Year 5: =DB(35000, 8000, 5, 5) = $2,090.95
```
**Result:** Vehicle depreciation schedule showing accelerated early-year deductions

**Explanation:** Vehicles depreciate rapidly in early years. The DB method matches this reality, with Year 1 depreciation ($11,515) nearly 6 times Year 5 ($2,091).

---

### Example 7: Computer Equipment
```
Cost: $2,500
Salvage: $100
Life: 3 years

=DB(2500, 100, 3, 1) = $1,375.00
=DB(2500, 100, 3, 2) = $618.75
=DB(2500, 100, 3, 3) = $406.25
```
**Result:** 3-year computer depreciation schedule

**Explanation:** Computers become obsolete quickly. The DB method front-loads depreciation, matching the actual economic decline in value as technology ages.

---

### Example 8: Zero Salvage Value
```
Cost: $15,000
Salvage: $0
Life: 5 years

=DB(15000, 0, 5, 1)
```
**Result:** #NUM! (Error)

**Explanation:** When salvage value is 0, the depreciation rate formula involves dividing by zero. Use DDB or SLN instead, or set a minimal salvage value like $1.

---

### Example 9: Manufacturing Equipment
```
CNC Machine cost: $150,000
Salvage value: $15,000
Useful life: 10 years
Purchased October 1 (3 months remaining in year 1)

=DB(150000, 15000, 10, 1, 3)
```
**Result:** $5,775.00 (3/12 of full year)

**Explanation:** October purchase means only 3 months of depreciation in year 1. Full annual depreciation would be $23,100; prorated amount is $5,775.

---

### Example 10: Comparing Book Values Over Time
```
Cost: $100,000, Salvage: $10,000, Life: 8 years

Period | Depreciation | Book Value
   1   |   $26,050    |   $73,950
   2   |   $19,264    |   $54,686
   3   |   $14,246    |   $40,440
   4   |   $10,535    |   $29,906
   5   |    $7,790    |   $22,116
   6   |    $5,759    |   $16,357
   7   |    $4,257    |   $12,100
   8   |    $2,100    |   $10,000 (salvage)
```
**Result:** Book value declining to salvage over 8 years

**Explanation:** The declining book value column shows how DB depreciation systematically reduces the asset from cost to salvage value.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Salvage value is 0 or negative | Use a small positive salvage value (e.g., $1) or choose DDB/SLN method |
| `#NUM!` | Period exceeds life (when month=12) | Ensure period is between 1 and life |
| `#NUM!` | Period exceeds life+1 (when month<12) | With partial years, maximum period is life+1 |
| `#NUM!` | Cost or life is 0 or negative | All values must be positive |
| `#VALUE!` | Non-numeric arguments | Ensure all inputs are numbers, not text |
| `Wrong result` | Month parameter misunderstood | Month is months IN first year, not acquisition month |

## Use Cases

### [[Fixed Asset Accounting]]

**Scenario:** A manufacturing company needs to calculate depreciation for their fleet of delivery trucks for financial reporting and tax purposes.

**Implementation:**
```
Fleet of 5 trucks:
- Purchase price: $45,000 each
- Residual value: $7,500 each
- Useful life: 6 years
- Purchased: March 1 (10 months in first year)

Annual depreciation per truck:
=DB(45000, 7500, 6, 1, 10) = Year 1
=DB(45000, 7500, 6, 2, 10) = Year 2
...through Year 7 (partial final year)

Fleet depreciation: =5 * DB(45000, 7500, 6, period, 10)
```
**Result:** Accurate depreciation schedule for financial statements and tax filings

**Business Application:** The accounting department uses this to record depreciation expense monthly, update accumulated depreciation accounts, and prepare fixed asset schedules for auditors. The accelerated method provides larger tax deductions in early years when the trucks generate the most revenue.

**Technical Details:** The 10-month first year reflects a March 1 purchase (10 months remaining). The depreciation schedule extends into a 7th year to capture the final 2 months of depreciation.

---

### [[Tax Planning and Optimization]]

**Scenario:** A small business owner is deciding between purchasing equipment in December or January, analyzing the tax impact using declining balance depreciation.

**Implementation:**
```
Equipment cost: $80,000
Salvage value: $8,000
Life: 7 years

Option A: December purchase (1 month in Year 1)
=DB(80000, 8000, 7, 1, 1) = $2,057.14 Year 1 deduction

Option B: January purchase (12 months in Year 1)
=DB(80000, 8000, 7, 1, 12) = $24,685.71 Year 1 deduction

5-year cumulative comparison:
December: Sum of periods 1-5 with month=1
January: Sum of periods 1-5 with month=12
```
**Result:** January purchase provides significantly higher early-year deductions

**Business Application:** By understanding the month parameter's impact, business owners can time purchases to optimize tax deductions. A December purchase provides minimal first-year benefit, while a January purchase maximizes the accelerated depreciation advantage.

**Technical Details:** The December scenario spreads depreciation over 8 years (7 + partial), while January completes in exactly 7 years. Present value analysis favors earlier deductions due to time value of money.

---

### [[Asset Replacement Planning]]

**Scenario:** A logistics company needs to plan equipment replacement by projecting book values and matching depreciation expense with actual asset productivity.

**Implementation:**
```
Forklift fleet analysis:
- Original cost: $25,000 per unit
- Estimated salvage: $3,500
- Depreciable life: 5 years

Book value at each year-end:
Year 0: $25,000 (purchase)
Year 1: =25000 - DB(25000, 3500, 5, 1) = $17,550
Year 2: =17550 - DB(25000, 3500, 5, 2) = $12,316
Year 3: =12316 - DB(25000, 3500, 5, 3) = $8,642
Year 4: =8642 - DB(25000, 3500, 5, 4) = $6,065
Year 5: =6065 - DB(25000, 3500, 5, 5) = $3,500 (salvage)

Compare to actual market value for replacement decisions
```
**Result:** Book value projection helps identify optimal replacement timing

**Business Application:** When book value exceeds market value, the asset is over-valued on the books. When market value exceeds book value, there's potential gain on sale. Operations managers use this analysis to decide when to sell/replace equipment.

**Technical Details:** DB depreciation often tracks actual market depreciation better than straight-line for equipment. Comparing book value to actual resale quotes helps validate the depreciation method choice.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Rate is rounded to three decimal places internally
- **Precision:** Standard IEEE 754 double-precision

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Identical calculation to Excel
- **Precision:** Same rounding behavior for rate calculation

### Both Platforms
- Default month value is 12 (full first year)
- Rate formula: 1 - ((salvage / cost) ^ (1 / life)), rounded to 3 decimals
- First year is prorated by month/12
- Final period captures any remaining depreciation to reach salvage exactly

## Tips and Best Practices

1. **Avoid zero salvage:** DB cannot calculate depreciation to zero salvage value because the rate formula involves division. Use $1 or another nominal value, or switch to DDB or SLN.

2. **Understand the month parameter:** Month is the number of months in the FIRST year, not the purchase month. A June purchase means 7 months remaining (June-December), so use month=7.

3. **Account for the extra period:** When month < 12, the depreciation extends to life+1 periods. Build your schedule accordingly and ensure you include the final partial year.

4. **Verify your totals:** Sum all depreciation periods to confirm they equal cost minus salvage. This validates your schedule is complete and correct.

5. **Compare methods before choosing:** DB provides a fixed rate decline. Compare to DDB (which may switch to SLN) and SLN to choose the method that best matches the asset's actual value decline pattern.

6. **Use for tax planning:** Because DB front-loads depreciation, it provides larger deductions in early years. Consider the time value of money when evaluating depreciation methods for tax purposes.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DDB]] | Double declining balance depreciation | When you want 200% rate or custom factor |
| [[SLN]] | Straight-line depreciation | When even annual depreciation is preferred |
| [[SYD]] | Sum-of-years-digits depreciation | For alternative accelerated method |
| [[VDB]] | Variable declining balance | For complex scenarios with switching |

### Commonly Used Together

**[[SUM]]** - Total Depreciation Verification

*Verify depreciation schedule sums to depreciable base:*
```
=SUM(DB(10000,1000,5,1),DB(10000,1000,5,2),...,DB(10000,1000,5,5))
Should equal: 10000 - 1000 = 9000
```
The sum of all periods must equal cost minus salvage.

---

**[[VDB]]** - Flexible Depreciation Calculations

*VDB can replicate DB with more flexibility:*
```
DB: =DB(cost, salvage, life, period)
VDB: =VDB(cost, salvage, life, period-1, period, factor)
```
VDB allows switching methods and calculating partial periods.

---

**[[IF]]** - Conditional Depreciation

*Choose depreciation method based on asset type:*
```
=IF(asset_type="vehicle", DB(cost,salvage,life,period), SLN(cost,salvage,life))
```
Use IF to apply appropriate depreciation methods to different asset categories.

---

**[[ROUND]]** - Financial Reporting

*Round depreciation for clean reporting:*
```
=ROUND(DB(cost, salvage, life, period), 2)
```
Round results to two decimal places for financial statements.

## Official Documentation

- **Microsoft Excel:** [DB function](https://support.microsoft.com/en-us/office/db-function-354e7d28-5f93-4ff1-8a52-eb4ee549d9d7)
- **Google Sheets:** [DB function](https://support.google.com/docs/answer/3093167)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original implementation |
| Excel 2007+ | Unchanged | Consistent behavior maintained |
| Google Sheets | Original launch | Built-in function from start |

---

*Last updated: 2026-01-10*
