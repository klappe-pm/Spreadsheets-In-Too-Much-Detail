---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- financial
subTopics:
- interest-rate-conversion
- nominal-rate
- compounding
- apy-to-apr
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Nominal Rate
- Nominal Annual Rate
- APR Calculator
tags:
- interest-rates
- compounding
- loan-analysis
- investment-comparison
---

# NOMINAL

## Description

**NOMINAL** calculates the nominal annual interest rate from an effective annual rate and the number of compounding periods per year. This is the inverse of the EFFECT function. When you know the true annual yield (APY) and want to find the stated rate (APR) that would produce it, NOMINAL does this conversion. This is useful for understanding what quoted rate corresponds to an observed or target effective rate.

The formula is: Nominal Rate = npery × ((1 + effective_rate)^(1/npery) - 1), where npery is the number of compounding periods per year. More frequent compounding means a lower nominal rate is needed to achieve the same effective rate. A 12.68% effective rate requires a 12% nominal rate with monthly compounding, but would require a higher nominal rate with quarterly compounding.

**Understanding nominal vs. effective rates is critical:** Effective rates (APY) represent true returns or costs including compounding. Nominal rates (APR) are the base rates before compounding effects. If a savings account promises 5% APY, NOMINAL tells you the underlying APR they're using. If you want 10% effective return from monthly compounding, NOMINAL tells you to seek a 9.57% APR product.

NOMINAL is available in all versions of Excel and Google Sheets with identical behavior. The function is commonly used when you have a target effective rate and need to determine the nominal rate to quote or seek, when reverse-engineering quoted rates from observed yields, and when standardizing rates across different compounding frequencies for comparison.

## Syntax

> [!f(x)] NOMINAL Syntax
>
> ```
> =NOMINAL(effect_rate, npery)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `effect_rate` | Yes | The effective annual interest rate (APY). Express as a decimal (0.10 for 10%). |
| `npery` | Yes | The number of compounding periods per year. Must be >= 1. Common values: 1 (annual), 2 (semi-annual), 4 (quarterly), 12 (monthly), 52 (weekly), 365 (daily). |

### Return Value

Returns the nominal annual interest rate as a decimal. Multiply by 100 for percentage. The result represents the stated rate before compounding effects.

## Examples

> [!f(x)] NOMINAL Examples

### Example 1: Monthly Compounding
```
Effective rate: 12.68%
Compounding: Monthly (12 times per year)

=NOMINAL(0.1268, 12)
```
**Result:** `0.12` (12.00%)

**Explanation:** A 12.68% effective rate corresponds to exactly 12% nominal with monthly compounding. This is the inverse of EFFECT(0.12, 12) = 0.1268.

---

### Example 2: Quarterly Compounding
```
Effective rate: 8.24%
Compounding: Quarterly (4 times per year)

=NOMINAL(0.0824, 4)
```
**Result:** `0.08` (8.00%)

**Explanation:** 8.24% effective with quarterly compounding means 8% nominal. Less frequent compounding requires a higher nominal rate to achieve the same effective rate.

---

### Example 3: Annual Compounding (No Conversion Needed)
```
=NOMINAL(0.10, 1)
```
**Result:** `0.10` (10.00%)

**Explanation:** With annual compounding, nominal and effective rates are identical. NOMINAL simply returns the input rate.

---

### Example 4: Daily Compounding
```
Effective rate: 5.13%
Compounding: Daily (365 times per year)

=NOMINAL(0.0513, 365)
```
**Result:** `0.05` (5.00%)

**Explanation:** A savings account advertising 5.13% APY uses a 5.00% APR with daily compounding. This is common bank practice.

---

### Example 5: Credit Card Rate Conversion
```
Effective annual rate: 19.56%
Compounding: Monthly (12)

=NOMINAL(0.1956, 12)
```
**Result:** `0.18` (18.00%)

**Explanation:** A credit card with 19.56% effective cost quotes 18% APR (1.5% monthly). NOMINAL helps reverse-engineer the stated rate from observed costs.

---

### Example 6: Target Return Calculation
```
Goal: 10% effective annual return
Investment compounds: Monthly

=NOMINAL(0.10, 12)
```
**Result:** `0.0957` (9.57%)

**Explanation:** To achieve 10% effective return with monthly compounding, seek products offering 9.57% APR or higher. The compounding provides the extra 0.43%.

---

### Example 7: Comparing Different Compounding Frequencies
```
Target effective rate: 8%

=NOMINAL(0.08, 1) = 8.00% (annual compounding)
=NOMINAL(0.08, 2) = 7.85% (semi-annual)
=NOMINAL(0.08, 4) = 7.77% (quarterly)
=NOMINAL(0.08, 12) = 7.72% (monthly)
=NOMINAL(0.08, 365) = 7.70% (daily)
```
**Result:** Lower nominal rates achieve same effective rate with more frequent compounding

**Explanation:** More compounding periods means interest earns interest more often, so a lower starting rate achieves the same annual result.

---

### Example 8: Bond Yield Conversion
```
Bond with 6% effective annual yield
Standard bond compounding: Semi-annual (2)

=NOMINAL(0.06, 2)
```
**Result:** `0.0591` (5.91%)

**Explanation:** A bond with 6% effective yield would be quoted as 5.91% nominal yield in bond market convention (semi-annual compounding).

---

### Example 9: Mortgage Rate Conversion
```
Lender advertises: 7.25% APY (effective rate)
Standard mortgage compounding: Monthly (12)

=NOMINAL(0.0725, 12)
```
**Result:** `0.0702` (7.02%)

**Explanation:** The lender's 7.25% APY corresponds to a 7.02% APR. Borrowers should look for the APR for standardized comparison.

---

### Example 10: High-Frequency Compounding
```
Target: 10% effective
Hourly compounding: 8,760 periods per year

=NOMINAL(0.10, 8760)
```
**Result:** `0.0953` (9.53%)

**Explanation:** With hourly compounding, a 9.53% nominal rate produces 10% effective. This approaches continuous compounding: ln(1.10) = 9.53%.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | npery < 1 | Compounding periods must be at least 1 |
| `#NUM!` | Effective rate <= -1 | Effective rate cannot be -100% or worse |
| `#VALUE!` | Non-numeric arguments | Ensure both inputs are numbers |
| `Result seems too low` | Misunderstanding output | Nominal is always <= effective when npery > 1 |
| `Result equals input` | npery = 1 | With annual compounding, nominal equals effective |

## Use Cases

### [[Loan Product Design]]

**Scenario:** A bank needs to determine what APR to quote on a new savings product that will offer a competitive 4.25% APY.

**Implementation:**
```
Target APY (effective rate): 4.25%
Compounding options being considered:

Daily compounding (365):
=NOMINAL(0.0425, 365) = 4.16% APR

Monthly compounding (12):
=NOMINAL(0.0425, 12) = 4.17% APR

Quarterly compounding (4):
=NOMINAL(0.0425, 4) = 4.18% APR

Marketing decision: Quote "4.16% APR / 4.25% APY" with daily compounding
```
**Result:** Bank can accurately advertise both APR and APY

**Business Application:** Banks must quote both APR and APY for deposits (Truth in Savings Act). NOMINAL ensures the quoted APR is mathematically consistent with the advertised APY.

**Technical Details:** The difference between daily and monthly nominal rates for the same effective rate is minimal (0.01%), so the bank's choice between daily and monthly compounding is primarily operational, not material to customers.

---

### [[Investment Target Setting]]

**Scenario:** A portfolio manager needs to set return targets for investment products that compound at different frequencies.

**Implementation:**
```
Target annual return: 8% effective
Different fund types with different compounding:

Equity fund (annual rebalancing):
=NOMINAL(0.08, 1) = 8.00% target APR

Bond fund (semi-annual income):
=NOMINAL(0.08, 2) = 7.85% target APR

Money market (daily compounding):
=NOMINAL(0.08, 365) = 7.70% target APR

Performance benchmarks set accordingly
```
**Result:** Consistent 8% effective return target across all funds

**Business Application:** Setting nominal rate targets that produce equivalent effective returns ensures fair performance comparison across funds with different distribution frequencies.

**Technical Details:** The equity fund must earn 8.00% to match the money market's 7.70% because it only compounds annually. This adjustment accounts for the timing difference in returns.

---

### [[Regulatory Compliance]]

**Scenario:** A lender must convert effective rates to APR for Truth in Lending disclosures.

**Implementation:**
```
Loan terms produce 7.50% effective annual rate
Required disclosure: APR with monthly compounding

=NOMINAL(0.075, 12) = 7.25% APR

Disclosure statement:
"Annual Percentage Rate (APR): 7.25%
Effective Annual Rate: 7.50%"
```
**Result:** Compliant APR disclosure derived from effective rate

**Business Application:** Lenders often calculate effective rates first (from all fees and terms), then must convert to APR for regulatory disclosures. NOMINAL ensures accurate, compliant conversion.

**Technical Details:** APR calculation for mortgages includes points and fees amortized over the loan. Once the effective rate is determined, NOMINAL converts it to the required APR format.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions (Excel 2000 through Excel 365); originally required Analysis ToolPak
- **Specific Behavior:** npery is truncated to an integer
- **Precision:** IEEE 754 double-precision floating point

### Google Sheets
- **Availability:** All versions since launch
- **Specific Behavior:** Identical to Excel
- **Precision:** Same as Excel

### Both Platforms
- Effective rate can be zero (returns zero)
- npery must be >= 1
- Result is always <= effective rate when npery > 1
- For continuous compounding, use =LN(1+effect_rate)

## Tips and Best Practices

1. **NOMINAL is the inverse of EFFECT:** NOMINAL(EFFECT(r, n), n) = r. Use this relationship to verify calculations.

2. **For target-setting, work backwards:** Start with desired effective return, use NOMINAL to find the quoted rate needed from the market.

3. **Remember APR < APY:** For any compounding more frequent than annual, the nominal rate (APR) is less than effective rate (APY). This is mathematically necessary.

4. **Use for regulatory conversion:** Many regulations require APR disclosure. Convert your calculated effective rates to APR using NOMINAL.

5. **Consider continuous compounding:** For theoretical work, continuous compounding nominal rate = ln(1 + effective_rate). This is the limit as npery approaches infinity.

6. **Pair with EFFECT for validation:** After using NOMINAL, verify with EFFECT(nominal_result, npery) = original_effective_rate.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[EFFECT]] | Convert nominal rate to effective rate | When you have APR and need APY |
| [[RATE]] | Calculate interest rate from loan terms | For rate implicit in a payment schedule |
| [[YIELD]] | Calculate bond yield | For bond-specific yield calculations |

### Commonly Used Together

**[[EFFECT]]** - Verification

*Verify NOMINAL calculation:*
```
Effective: 12.68%
=NOMINAL(0.1268, 12) = 12.00%
=EFFECT(0.12, 12) = 12.68% ✓ (verifies)
```
EFFECT(NOMINAL(rate, n), n) should return the original rate.

---

**[[LN]]** - Continuous Compounding

*Calculate continuous nominal rate:*
```
Continuous nominal rate = LN(1 + effective_rate)
=LN(1.10) = 9.53%

This is the limit of NOMINAL as npery → infinity
```
For theoretical or mathematical applications.

---

**[[PMT]]** - Loan Payment Calculation

*Use nominal rate in payment functions:*
```
Effective target: 6%
=NOMINAL(0.06, 12) = 5.84% = nominal monthly rate
=PMT(0.0584/12, 360, 200000) = monthly mortgage payment
```
Financial functions like PMT use nominal rates divided by periods.

---

**[[FV]]** - Future Value Calculation

*Convert rates for consistency:*
```
If given effective rate, convert to nominal for FV:
=NOMINAL(0.08, 12) = 7.72%
=FV(0.0772/12, 60, -100, 0) = future value of monthly investments
```
Ensures rate basis matches payment frequency.

## Official Documentation

- **Microsoft Excel:** [NOMINAL function](https://support.microsoft.com/en-us/office/nominal-function-7f1ae29b-6b92-435e-b950-ad8b190ddd2b)
- **Google Sheets:** [NOMINAL function](https://support.google.com/docs/answer/3093232)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 (Analysis ToolPak) | Originally required add-in |
| Excel 2007+ | Built-in function | No longer requires add-in |
| Google Sheets | Original launch | Built-in function from start |

---

*Last updated: 2026-01-10*
