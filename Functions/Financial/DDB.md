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
- double-declining-balance
- accelerated-depreciation
- asset-accounting
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Double Declining Balance
- 200% Declining Balance
- DDB Depreciation
tags:
- depreciation
- asset-management
- tax-planning
- accelerated-depreciation
---

# DDB

## Description

**DDB (Double Declining Balance)** calculates depreciation using the double declining balance method, the most aggressive standard accelerated depreciation approach. It applies twice the straight-line rate to the declining book value, creating very large deductions in early years that decrease rapidly over time. This method is popular for assets that lose value quickly when new, such as vehicles, computers, and technology equipment.

The double declining balance rate equals 2/life (or 200% of the straight-line rate). For a 5-year asset, DDB uses a 40% rate (2/5) instead of the 20% straight-line rate. This rate is applied to the beginning book value each period. Unlike DB (which calculates the exact rate to reach salvage), DDB may not fully depreciate an asset to its salvage value--it will stop depreciating once book value reaches salvage, leaving remaining depreciation unrecorded unless you switch methods.

**The factor parameter provides flexibility:** While "double" declining balance uses a factor of 2, you can specify any factor. A factor of 1.5 gives 150% declining balance, factor of 3 gives 300% (triple declining balance). Tax codes in different jurisdictions may specify different factors. However, DDB never depreciates below salvage value--it automatically stops when book value equals salvage, even if there's remaining useful life.

DDB is available in all versions of Excel and Google Sheets with identical behavior. This method is commonly used in US tax depreciation (MACRS often uses 200% declining balance with a switch to straight-line). For financial reporting, companies may use DDB when it best matches the asset's pattern of economic benefits. The VDB function offers more flexibility, including automatic switching to straight-line when that produces larger deductions.

## Syntax

> [!f(x)] DDB Syntax
>
> ```
> =DDB(cost, salvage, life, period, [factor])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `cost` | Yes | The initial cost of the asset. Must be a positive number. |
| `salvage` | Yes | The value at the end of the depreciation period (residual value). Can be 0. |
| `life` | Yes | The number of periods over which the asset is depreciated (useful life). Must be positive. |
| `period` | Yes | The period for which you want to calculate depreciation. Must be between 1 and life. |
| `factor` | No | The rate at which the balance declines. Defaults to 2 (double declining balance). |

### Return Value

Returns the depreciation amount for the specified period as a positive number. If the calculated depreciation would reduce book value below salvage, returns only the amount needed to reach salvage value (or 0 if already at salvage).

## Examples

> [!f(x)] DDB Examples

### Example 1: Basic Double Declining Balance
```
Cost: $10,000
Salvage: $1,000
Life: 5 years
Period: 1

=DDB(10000, 1000, 5, 1)
```
**Result:** `$4,000.00`

**Explanation:** DDB rate = 2/5 = 40%. First year: $10,000 * 40% = $4,000. This is double the straight-line amount of $1,800 ($9,000/5 years).

---

### Example 2: Complete 5-Year Schedule
```
=DDB(10000, 1000, 5, 1) = $4,000.00
=DDB(10000, 1000, 5, 2) = $2,400.00
=DDB(10000, 1000, 5, 3) = $1,440.00
=DDB(10000, 1000, 5, 4) = $864.00
=DDB(10000, 1000, 5, 5) = $296.00
Total: $9,000.00
```
**Result:** Full depreciation schedule

**Explanation:** Book value drops from $10,000 to $6,000 to $3,600 to $2,160 to $1,296 to $1,000. Year 5 is limited to $296 (not $518.40) because depreciation cannot reduce book value below salvage.

---

### Example 3: Custom Factor (150% Declining Balance)
```
Cost: $20,000
Salvage: $2,000
Life: 6 years
Factor: 1.5

=DDB(20000, 2000, 6, 1, 1.5)
```
**Result:** `$5,000.00`

**Explanation:** 150% declining rate = 1.5/6 = 25%. Year 1: $20,000 * 25% = $5,000. This is less aggressive than the default 200% rate.

---

### Example 4: Zero Salvage Value
```
Cost: $15,000
Salvage: $0
Life: 5 years

=DDB(15000, 0, 5, 1) = $6,000.00
=DDB(15000, 0, 5, 2) = $3,600.00
=DDB(15000, 0, 5, 3) = $2,160.00
=DDB(15000, 0, 5, 4) = $1,296.00
=DDB(15000, 0, 5, 5) = $777.60
Total: $13,833.60
```
**Result:** $1,166.40 remains undepreciated

**Explanation:** Unlike DB which requires positive salvage, DDB allows zero salvage. However, DDB may not fully depreciate the asset--$1,166.40 of book value remains. Switch to straight-line in later years to capture full depreciation.

---

### Example 5: Computer Equipment (Rapid Obsolescence)
```
Laptop cost: $2,000
Salvage: $100
Life: 3 years

Year 1: =DDB(2000, 100, 3, 1) = $1,333.33
Year 2: =DDB(2000, 100, 3, 2) = $444.44
Year 3: =DDB(2000, 100, 3, 3) = $22.22
Total: $1,799.99
```
**Result:** Near-complete depreciation in 3 years

**Explanation:** Rate = 2/3 = 66.67%. The aggressive depreciation matches laptop reality--after 3 years, most laptops are nearly worthless. Year 3 is limited to bring book value to $100 salvage.

---

### Example 6: Vehicle Depreciation
```
Company car: $45,000
Salvage: $12,000
Life: 5 years

Year 1: =DDB(45000, 12000, 5, 1) = $18,000.00
Year 2: =DDB(45000, 12000, 5, 2) = $10,800.00
Year 3: =DDB(45000, 12000, 5, 3) = $4,200.00
Year 4: =DDB(45000, 12000, 5, 4) = $0.00
Year 5: =DDB(45000, 12000, 5, 5) = $0.00
```
**Result:** Fully depreciated to salvage by Year 3

**Explanation:** By end of Year 3, book value reaches $12,000 (salvage). Years 4 and 5 show $0 depreciation because further depreciation would go below salvage.

---

### Example 7: Heavy Machinery with Triple Declining
```
Equipment: $200,000
Salvage: $20,000
Life: 10 years
Factor: 3

=DDB(200000, 20000, 10, 1, 3) = $60,000.00
```
**Result:** Triple declining balance depreciation

**Explanation:** 300% rate = 3/10 = 30%. Year 1: $200,000 * 30% = $60,000. Triple declining is very aggressive, appropriate when equipment loses value extremely rapidly.

---

### Example 8: Comparing DDB to Straight-Line
```
Asset: $50,000, Salvage: $5,000, Life: 5 years

Straight-Line (SLN): $9,000/year constant

DDB:
Year 1: $20,000 (vs $9,000)
Year 2: $12,000 (vs $9,000)
Year 3: $7,200 (vs $9,000)
Year 4: $4,320 (vs $9,000)
Year 5: $1,480 (limited by salvage)
```
**Result:** DDB front-loads 71% of depreciation in first 2 years

**Explanation:** Years 1-2 DDB: $32,000 vs SLN: $18,000. DDB provides nearly double the tax deduction in early years, but less in later years.

---

### Example 9: Monthly Depreciation
```
Annual DDB: =DDB(30000, 3000, 5, 1) = $12,000
Monthly: =$12,000 / 12 = $1,000/month in Year 1

Or use periods = months:
=DDB(30000, 3000, 60, 1) = $1,000
```
**Result:** Monthly depreciation calculation

**Explanation:** You can either divide annual DDB by 12, or set life and period in months (60 months = 5 years). The monthly method gives slightly different results due to the more frequent compounding of the declining balance.

---

### Example 10: When DDB Equals Zero Early
```
Cost: $10,000
Salvage: $6,000
Life: 5 years

=DDB(10000, 6000, 5, 1) = $4,000.00
=DDB(10000, 6000, 5, 2) = $0.00 (book value = salvage)
=DDB(10000, 6000, 5, 3) = $0.00
```
**Result:** Depreciation stops at Year 2

**Explanation:** High salvage value ($6,000) means only $4,000 can be depreciated. DDB Year 1 reaches this limit, leaving Years 2-5 with zero depreciation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Period is 0 or negative | Period must be >= 1 |
| `#NUM!` | Period exceeds life | Period must be <= life |
| `#NUM!` | Cost, life, or factor is <= 0 | All must be positive numbers |
| `#NUM!` | Salvage is negative | Salvage must be >= 0 |
| `#VALUE!` | Non-numeric arguments | Ensure all inputs are numbers |
| `Returns 0 unexpectedly` | Book value has reached salvage | Asset is fully depreciated to salvage; this is correct behavior |
| `Total depreciation < expected` | DDB doesn't guarantee full depreciation | Consider switching to SLN in later years using VDB |

## Use Cases

### [[Tax Depreciation Strategy]]

**Scenario:** A business is evaluating depreciation methods to maximize early tax deductions for newly purchased manufacturing equipment.

**Implementation:**
```
Equipment cost: $500,000
Salvage value: $50,000
Useful life: 7 years
Tax rate: 25%

DDB Depreciation and Tax Savings:
Year 1: =DDB(500000,50000,7,1) = $142,857 -> Tax savings: $35,714
Year 2: =DDB(500000,50000,7,2) = $102,041 -> Tax savings: $25,510
Year 3: =DDB(500000,50000,7,3) = $72,886  -> Tax savings: $18,222
Year 4: =DDB(500000,50000,7,4) = $52,062  -> Tax savings: $13,016
Year 5: =DDB(500000,50000,7,5) = $37,187  -> Tax savings: $9,297
Year 6: =DDB(500000,50000,7,6) = $26,562  -> Tax savings: $6,641
Year 7: =DDB(500000,50000,7,7) = $16,405  -> Tax savings: $4,101

Total: $450,000 depreciated, $112,500 tax savings
```
**Result:** $61,224 tax savings in first 2 years alone

**Business Application:** DDB front-loads tax deductions, providing larger tax savings in early years when the business may need cash flow most. Present value analysis shows DDB provides superior after-tax returns compared to straight-line.

**Technical Details:** Note that DDB may not fully depreciate to salvage in 7 years. Year 7 shows $16,405 which brings book value to approximately $50,000. Companies often switch to straight-line (using VDB) when it provides larger deductions.

---

### [[Asset Tracking and Book Value Projection]]

**Scenario:** A fleet management company needs to track book values of vehicles and project when assets should be sold or replaced based on depreciation schedules.

**Implementation:**
```
Vehicle: $38,000
Salvage: $8,000
Life: 5 years

Book Value Projection:
Year 0: $38,000 (purchase)
Year 1: =38000-DDB(38000,8000,5,1) = $22,800
Year 2: =22800-DDB(38000,8000,5,2) = $13,680
Year 3: =13680-DDB(38000,8000,5,3) = $8,208
Year 4: =8208-DDB(38000,8000,5,4) = $8,000 (reaches salvage)
Year 5: $8,000 (no further depreciation)

Optimal sale window: When market value > book value
If Year 3 resale value = $15,000 and book value = $8,208
Gain on sale = $6,792 (taxable)
```
**Result:** Clear visibility into asset value trajectory

**Business Application:** Fleet managers use book value projections to identify optimal sale timing. Selling when market value exceeds book value generates gains, while selling below book value creates losses. DDB's aggressive early depreciation often results in market value exceeding book value, creating flexibility for profitable sales.

**Technical Details:** The DDB schedule hits salvage value in Year 4, earlier than the 5-year life. This reflects the method's aggressive nature and the salvage floor that prevents over-depreciation.

---

### [[Financial Reporting and GAAP Compliance]]

**Scenario:** A company must choose a depreciation method for new equipment that best reflects the pattern of economic benefits consumed.

**Implementation:**
```
Production equipment that generates most revenue when new:
Cost: $120,000
Salvage: $10,000
Useful life: 8 years

Revenue pattern matching analysis:
Year 1 productivity: 100% -> DDB depreciation: $30,000 (27%)
Year 2 productivity: 85%  -> DDB depreciation: $22,500 (20%)
Year 3 productivity: 70%  -> DDB depreciation: $16,875 (15%)
Year 4 productivity: 60%  -> DDB depreciation: $12,656 (12%)
...

Compare to straight-line: $13,750 constant (12.5% per year)

DDB better matches declining productivity pattern
```
**Result:** DDB selected for equipment with declining productivity curve

**Business Application:** GAAP requires depreciation methods that reflect the pattern in which economic benefits are consumed. For equipment that's most productive when new, DDB provides better matching than straight-line, resulting in more accurate financial statements.

**Technical Details:** The matching principle suggests that depreciation expense should correlate with revenue generation. DDB's declining expense pattern mirrors declining productivity, improving the income statement's accuracy.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Factor defaults to 2 if omitted; accepts any positive number
- **Precision:** IEEE 754 double-precision floating point

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Identical to Excel in all respects
- **Precision:** Same precision as Excel

### Both Platforms
- DDB never depreciates below salvage value
- Factor parameter allows any positive rate multiplier
- Returns 0 when book value has already reached salvage
- Accepts 0 as a valid salvage value (unlike DB)

## Tips and Best Practices

1. **Plan for incomplete depreciation:** DDB may leave residual book value above salvage, especially with long-lived assets. Use VDB with switching, or manually switch to straight-line when it produces larger deductions.

2. **Understand the salvage floor:** DDB automatically stops at salvage value. This is correct behavior, not an error. Check your book value tracking to understand when this occurs.

3. **Use factor for regional tax codes:** Different jurisdictions may require 150%, 175%, or other declining balance rates. The factor parameter accommodates these requirements (1.5, 1.75, etc.).

4. **Compare to MACRS:** US tax depreciation often uses MACRS, which is DDB with a switch to straight-line. Use VDB to replicate MACRS behavior, or reference IRS depreciation tables directly.

5. **Monitor for zero depreciation:** Once DDB returns 0, the asset has reached salvage. This may happen before the end of useful life with high salvage values or long asset lives.

6. **Consider monthly calculations carefully:** When using monthly periods, remember that DDB compounds monthly. Annual DDB divided by 12 gives different results than monthly DDB. Choose the method your accounting standards require.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DB]] | Fixed declining balance depreciation | When you need exact depreciation to salvage |
| [[SLN]] | Straight-line depreciation | For even annual depreciation |
| [[SYD]] | Sum-of-years-digits depreciation | For alternative accelerated method |
| [[VDB]] | Variable declining balance | For switching methods or partial periods |

### Commonly Used Together

**[[VDB]]** - Switching to Straight-Line

*Switch to SLN when it gives larger deductions:*
```
=VDB(cost, salvage, life, start_period, end_period, factor, TRUE)
Setting no_switch to FALSE (or omitting) switches automatically
```
VDB provides MACRS-like behavior with automatic switching.

---

**[[SLN]]** - Compare Methods

*Compare DDB to straight-line depreciation:*
```
DDB Year 1: =DDB(10000, 1000, 5, 1) = $4,000
SLN Annual: =SLN(10000, 1000, 5) = $1,800
```
DDB Year 1 is 2.22x the straight-line amount.

---

**[[MIN]]** - Cap Depreciation at Remaining Value

*Ensure depreciation doesn't exceed remaining book value:*
```
=MIN(DDB(cost,salvage,life,period), book_value - salvage)
```
This manually implements the salvage floor (though DDB does this automatically).

---

**[[IF]]** - Method Selection Logic

*Choose method based on asset characteristics:*
```
=IF(rapid_obsolescence, DDB(cost,salvage,life,period), SLN(cost,salvage,life))
```
Apply DDB to technology assets, SLN to buildings.

## Official Documentation

- **Microsoft Excel:** [DDB function](https://support.microsoft.com/en-us/office/ddb-function-519a7a37-8772-4c96-85c0-ed2c209717a5)
- **Google Sheets:** [DDB function](https://support.google.com/docs/answer/3093168)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original implementation |
| Excel 2007+ | Unchanged | Consistent behavior maintained |
| Google Sheets | Original launch | Built-in function from start |

---

*Last updated: 2026-01-10*
