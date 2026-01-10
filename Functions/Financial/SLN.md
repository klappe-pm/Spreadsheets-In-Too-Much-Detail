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
- asset-management
- accounting
- tax-calculation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Straight-Line Depreciation
- Linear Depreciation
- Even Depreciation
tags:
- financial-modeling
- accounting
- depreciation
- asset-valuation
---

# SLN

## Description

**SLN (Straight-Line Depreciation)** calculates the depreciation expense of an asset for each period using the straight-line method, where the asset loses equal value every period over its useful life. This is the simplest and most commonly used depreciation method, dividing the depreciable amount (cost minus salvage value) evenly across all periods. If you buy a $50,000 machine with $5,000 salvage value and a 10-year life, SLN tells you it depreciates $4,500 per year, every year.

The straight-line method assumes an asset provides equal benefit every period throughout its useful life. While this is a simplification--most assets are more productive when new and less productive as they age--the method's simplicity makes it popular for financial reporting and tax purposes. The same depreciation expense each year also makes budgeting and financial forecasting straightforward, as there are no changing depreciation amounts to track.

**There are no confusing sign conventions with SLN:** Unlike time-value-of-money functions where cash flow direction matters, SLN simply returns a positive number representing the depreciation expense per period. Cost, salvage, and life are all entered as positive values. The result represents an expense that reduces taxable income and appears as a line item on the income statement.

SLN is available in all versions of Excel and Google Sheets with identical behavior. The function is useful for financial statements, tax planning, capital budgeting, and asset management. For accelerated depreciation methods (which front-load depreciation in early years), see DB (declining balance), DDB (double-declining balance), or SYD (sum-of-years-digits).

## Syntax

> [!f(x)] SLN Syntax
>
> ```
> =SLN(cost, salvage, life)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `cost` | Yes | The initial cost of the asset. This is the purchase price plus any costs to prepare the asset for use (installation, shipping, etc.). Enter as a positive number. |
| `salvage` | Yes | The residual value at the end of the asset's useful life. This is what you expect to sell or dispose of the asset for. Can be 0 if no residual value. |
| `life` | Yes | The number of periods over which the asset is depreciated. Typically years, but can be any consistent time unit. Must be positive. |

### Return Value

Returns the depreciation expense per period as a positive number. The value is constant for all periods (that is the nature of straight-line depreciation). Multiply by the number of periods to get total depreciation, which should equal cost minus salvage.

## Examples

> [!f(x)] SLN Examples

### Example 1: Basic Equipment Depreciation
```
=SLN(50000, 5000, 10)
```
**Result:** `$4,500.00`

**Explanation:** A $50,000 machine with $5,000 salvage value over 10 years depreciates $4,500 per year. Total depreciation: $4,500 x 10 = $45,000 = $50,000 - $5,000. The formula is simply (cost - salvage) / life.

---

### Example 2: Vehicle Depreciation
```
=SLN(35000, 8000, 5)
```
**Result:** `$5,400.00`

**Explanation:** A $35,000 company vehicle with $8,000 residual value after 5 years. Annual depreciation is $5,400. After 5 years, accumulated depreciation is $27,000, leaving a book value of $8,000 (the salvage value).

---

### Example 3: Zero Salvage Value
```
=SLN(25000, 0, 7)
```
**Result:** `$3,571.43`

**Explanation:** Some assets have no residual value--they are completely used up or worthless at end of life. A $25,000 specialized machine fully depreciated over 7 years equals $3,571.43 per year. Total depreciation equals full cost.

---

### Example 4: Monthly Depreciation
```
=SLN(120000, 20000, 60)
```
**Result:** `$1,666.67`

**Explanation:** Depreciation can be calculated monthly. A $120,000 asset with $20,000 salvage over 60 months (5 years) = $1,666.67/month. This is useful for monthly financial statements and interim reporting.

---

### Example 5: Computer Equipment
```
=SLN(5000, 500, 3)
```
**Result:** `$1,500.00`

**Explanation:** Computers typically have short useful lives. A $5,000 server with $500 salvage over 3 years depreciates $1,500 annually. After 3 years, book value is $500--though market value may differ significantly.

---

### Example 6: Office Furniture
```
=SLN(15000, 1500, 7)
```
**Result:** `$1,928.57`

**Explanation:** Office furniture with a 7-year life. The $15,000 cost less $1,500 salvage = $13,500 depreciable base, divided by 7 years = $1,928.57 per year.

---

### Example 7: Building Depreciation
```
=SLN(500000, 50000, 39)
```
**Result:** `$11,538.46`

**Explanation:** Commercial buildings are typically depreciated over 39 years (IRS requirement for non-residential property). A $500,000 building with $50,000 land value excluded depreciates $11,538.46 per year. Note: land is never depreciated.

---

### Example 8: Total Accumulated Depreciation
```
=SLN(80000, 8000, 10) * 6
```
**Result:** `$43,200.00`

**Explanation:** After 6 years of a 10-year depreciation schedule, accumulated depreciation is $7,200/year x 6 = $43,200. Book value at year 6: $80,000 - $43,200 = $36,800.

---

### Example 9: Book Value at Any Point
```
Cost - (SLN * periods elapsed) = Book Value
$80,000 - ($7,200 * 6) = $36,800
```
**Result:** Book value is cost minus accumulated depreciation

**Explanation:** To find book value at any point, subtract accumulated depreciation (SLN x periods elapsed) from original cost. Book value starts at cost and declines linearly to salvage value.

---

### Example 10: Quarterly Depreciation
```
=SLN(60000, 12000, 20)
```
**Result:** `$2,400.00`

**Explanation:** For quarterly financial statements, express life in quarters. A 5-year asset has 20 quarters. $60,000 cost less $12,000 salvage over 20 quarters = $2,400 per quarter.

---

### Example 11: Compare to Accelerated Depreciation
```
Year 1 Straight-Line: =SLN(100000, 10000, 5) = $18,000
Year 1 Double-Declining: =DDB(100000, 10000, 5, 1) = $40,000
Year 1 Sum-of-Years: =SYD(100000, 10000, 5, 1) = $30,000
```
**Result:** Straight-line has lowest early depreciation

**Explanation:** Compared to accelerated methods, straight-line spreads depreciation evenly. Accelerated methods (DDB, SYD) front-load depreciation, providing larger tax deductions in early years but smaller deductions later.

---

### Example 12: Depreciation Schedule Table
```
Year 1: Depreciation = $4,500, Book Value = $45,500
Year 2: Depreciation = $4,500, Book Value = $41,000
Year 3: Depreciation = $4,500, Book Value = $36,500
...
Year 10: Depreciation = $4,500, Book Value = $5,000 (salvage)
```
**Result:** Linear decline to salvage value

**Explanation:** Straight-line depreciation creates a simple, predictable schedule. Book value declines by exactly SLN each period until reaching salvage value. This predictability aids budgeting and forecasting.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Life is 0 or negative | Life must be a positive number. Even partial years should be positive. |
| `#VALUE!` | Non-numeric inputs | Ensure cost, salvage, and life are numbers. Check for text in cells. |
| `#DIV/0!` | Life is 0 | Cannot divide by zero periods. Life must be at least 1. |
| `Negative result` | Salvage > Cost | Salvage value should not exceed cost. If asset appreciates, depreciation does not apply. |
| `Book value below salvage` | Over-depreciation | Stop depreciation when book value reaches salvage. Track accumulated depreciation carefully. |
| `Mismatch with tax records` | Different methods/lives | Tax depreciation may use different rules (MACRS in US). SLN is for book/financial purposes. |

## Use Cases

### [[Annual Financial Statement Preparation]]

**Scenario:** An accountant needs to calculate and record depreciation expense for all company assets for the annual financial statements.

**Implementation:**
```
Asset Register:
Equipment: =SLN(250000, 25000, 10) = $22,500/year
Vehicles: =SLN(180000, 36000, 5) = $28,800/year
Computers: =SLN(50000, 5000, 3) = $15,000/year
Furniture: =SLN(40000, 4000, 7) = $5,142.86/year

Total Annual Depreciation: $71,442.86
```

**Business Application:** Companies must record depreciation expense to comply with GAAP (Generally Accepted Accounting Principles). SLN provides the annual expense for each asset class, which flows to the income statement as an expense and to the balance sheet as accumulated depreciation.

**Technical Details:** For financial statements, consistency is important. Once an asset's depreciation method is chosen, it should generally continue for that asset's life. SLN's simplicity makes it easy to audit and verify.

---

### [[Capital Budget Planning]]

**Scenario:** A CFO is evaluating a $500,000 equipment purchase and needs to understand the annual depreciation expense impact on profit and loss over the asset's useful life.

**Implementation:**
```
New Equipment: $500,000 cost, $50,000 salvage, 10-year life
Annual Depreciation: =SLN(500000, 50000, 10) = $45,000

Year 1-10 Impact:
Operating Income before depreciation: $200,000
Less: Depreciation expense: $45,000
Operating Income after depreciation: $155,000

Tax Shield (at 25% rate): $45,000 * 0.25 = $11,250 annual tax savings
```

**Business Application:** Capital budgeting requires understanding how asset purchases affect profitability over time. SLN provides the annual depreciation expense, which reduces taxable income and provides a "tax shield." The steady expense also aids multi-year forecasting.

**Technical Details:** While depreciation is a non-cash expense (no actual cash outflow after purchase), it reduces taxable income, providing real tax savings. The "tax shield" is depreciation times the tax rate.

---

### [[Asset Replacement Planning]]

**Scenario:** A facilities manager needs to project when assets will be fully depreciated and potentially need replacement, and budget for replacement costs.

**Implementation:**
```
Current HVAC System:
Original cost: $150,000, Installed 4 years ago, 15-year life, $15,000 salvage
Annual depreciation: =SLN(150000, 15000, 15) = $9,000
Accumulated depreciation: $9,000 * 4 = $36,000
Current book value: $150,000 - $36,000 = $114,000
Years remaining: 11 years
Fully depreciated: Year 2036

Replacement Planning:
Estimated replacement cost (with inflation): $220,000
Annual savings needed: $220,000 / 11 = $20,000/year
```

**Business Application:** Facilities and operations managers use depreciation schedules to plan capital replacements. Knowing when assets will be fully depreciated helps time replacements and budget for major capital expenditures.

**Technical Details:** Book value and market value often differ significantly. An asset may be fully depreciated (book value = salvage) but still functional. Conversely, an asset may be worth less than book value if it becomes obsolete. Physical and economic life may differ from depreciation life.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 97 through Excel 365)
- **Specific Behavior:** Simple division calculation. Returns exact decimal result.
- **Precision:** IEEE 754 double-precision floating point.

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Functionally identical to Excel. Same calculation method.
- **Precision:** Same as Excel.

### Both Platforms
- Returns positive depreciation expense (not negative like TVM functions)
- All parameters should be positive numbers
- Result is identical for all periods (no period-specific calculation)
- Formula: (cost - salvage) / life

## Tips and Best Practices

1. **Remember the formula:** SLN is simply (cost - salvage) / life. If you understand this, you can verify any SLN result or calculate manually when needed.

2. **Match life to your reporting period:** If you prepare monthly statements, express life in months. If annual, use years. Just be consistent with the period used.

3. **Track accumulated depreciation separately:** SLN gives per-period depreciation. For book value, track accumulated depreciation (SLN x periods elapsed) and subtract from cost.

4. **Stop at salvage value:** If an asset is not disposed of, stop recording depreciation when book value equals salvage value. Do not depreciate below salvage.

5. **Consider tax vs. book differences:** Tax authorities (IRS in US) may require different methods (like MACRS). SLN is typically for financial/book purposes; tax depreciation may differ.

6. **Exclude land from cost:** Land is never depreciated. When depreciating buildings, exclude the land portion of the purchase price from the cost used in SLN.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DDB]] | Double declining balance depreciation | When you want accelerated depreciation (front-loaded) |
| [[SYD]] | Sum-of-years-digits depreciation | Another accelerated method, more moderate than DDB |
| [[DB]] | Fixed declining balance depreciation | For specific declining balance rates |
| [[VDB]] | Variable declining balance | For flexible depreciation periods and rates |

### Commonly Used Together

**[[DDB]]** - Accelerated Comparison

*Compare methods:*
```
Straight-line: =SLN(100000, 10000, 5) = $18,000/year all years
DDB Year 1: =DDB(100000, 10000, 5, 1) = $40,000
DDB Year 2: =DDB(100000, 10000, 5, 2) = $24,000
```
DDB front-loads depreciation; SLN spreads it evenly.

---

**[[SYD]]** - Moderate Acceleration

*Compare first-year impact:*
```
SLN: =SLN(100000, 10000, 5) = $18,000
SYD Year 1: =SYD(100000, 10000, 5, 1) = $30,000
```
SYD provides moderate acceleration between SLN and DDB.

---

**[[SUM]]** - Total Depreciation Verification

*Verify total equals depreciable base:*
```
Annual: =SLN(50000, 5000, 10) = $4,500
Total: $4,500 * 10 = $45,000
Depreciable base: $50,000 - $5,000 = $45,000
```
Total depreciation over life should equal cost minus salvage.

## Official Documentation

- **Microsoft Excel:** [SLN function](https://support.microsoft.com/en-us/office/sln-function-cdb666e5-c1c6-40a7-806a-e695edc2f1c8)
- **Google Sheets:** [SLN function](https://support.google.com/docs/answer/3093228)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 (1997) | Original implementation |
| Excel 2007+ | All subsequent versions | No changes to function behavior |
| Google Sheets | Original launch | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
