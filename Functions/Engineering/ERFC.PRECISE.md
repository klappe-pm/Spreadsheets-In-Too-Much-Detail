---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
subTopics:
- complementary-error-function
- probability
- statistics
- tail-probability
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- precise-complementary-error
- erfc-precise
- high-precision-erfc
tags:
- engineering-function
- error-function
- complementary
- precision
- statistics
---

# ERFC.PRECISE

## Description

ERFC.PRECISE calculates the complementary error function, erfc(x) = 1 - erf(x), with guaranteed single-argument behavior and consistent naming conventions. The complementary error function represents the integral (2/sqrt(pi)) * integral from x to infinity of exp(-t^2) dt, which equals the probability mass in the tails of a Gaussian distribution beyond the specified point. ERFC.PRECISE was introduced in Excel 2010 as part of a standardization effort to clarify function behavior.

The "PRECISE" suffix indicates that this function accepts exactly one argument and provides unambiguous behavior. While ERFC and ERFC.PRECISE are mathematically identical and return the same results for the same inputs, the PRECISE naming convention makes code more readable and self-documenting. This is particularly valuable in shared workbooks and templates where clarity of intent is important for maintenance and auditing purposes.

The complementary error function is critical for precision in scientific computing. When calculating tail probabilities where erf(x) is very close to 1, computing 1 - erf(x) directly causes loss of significant digits due to catastrophic cancellation. ERFC.PRECISE (and ERFC) compute the complementary function directly, maintaining full floating-point precision even for large arguments. For example, ERFC.PRECISE(5) returns approximately 1.54 x 10^-12 with full precision, whereas 1 - ERF(5) would lose precision.

ERFC.PRECISE is available only in Excel 2010 and later versions. Google Sheets does not have ERFC.PRECISE but provides ERFC which is functionally identical. The function returns values between 0 and 2: it equals 1 at x = 0, approaches 0 as x increases toward infinity, and approaches 2 as x decreases toward negative infinity. For cross-platform compatibility, use ERFC instead of ERFC.PRECISE.

## Syntax

> [!NOTE] Syntax
> ```
> ERFC.PRECISE(x)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **x** | The value at which to evaluate the complementary error function. Represents the lower limit of integration to infinity. Can be any real number, positive or negative. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=ERFC.PRECISE(0)` | 1 | erfc(0) = 1 - erf(0) = 1 |
| `=ERFC.PRECISE(1)` | 0.15729921 | Complement of erf(1) = 0.8427 |
| `=ERFC.PRECISE(2)` | 0.00467773 | Small tail probability |
| `=ERFC.PRECISE(3)` | 2.2090E-05 | Very small tail |
| `=ERFC.PRECISE(4)` | 1.5418E-08 | Extremely small, precision maintained |
| `=ERFC.PRECISE(5)` | 1.5375E-12 | Full precision for tiny probability |
| `=ERFC.PRECISE(-1)` | 1.84270079 | erfc(-x) = 2 - erfc(x) |
| `=ERFC.PRECISE(0.5)` | 0.47950012 | Intermediate value |
| `=ERFC.PRECISE(6)` | 2.15E-17 | Near machine precision limit |
| `=0.5*ERFC.PRECISE(A1/SQRT(2))` | Q(A1) | Q-function via ERFC.PRECISE |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric argument | Ensure argument is a numeric value |
| `#VALUE!` | Text input | Use VALUE() to convert text representations to numbers |
| `#NAME?` | Function not recognized | Requires Excel 2010+; not available in earlier versions |
| `#NAME?` | Google Sheets usage | Use ERFC instead (ERFC.PRECISE not available) |
| `#VALUE!` | Multiple arguments | ERFC.PRECISE accepts exactly one argument |

## Use Cases

### 1. High-Precision BER Calculations for Optical Communications

**Scenario**: An optical communications engineer needs to calculate extremely low bit error rates (10^-9 to 10^-15) for fiber optic systems where precision is critical for system specification and margin analysis.

**Implementation**: Use ERFC.PRECISE to maintain numerical accuracy when calculating very small error probabilities that would suffer precision loss with 1-ERF calculations.

```
Optical BPSK BER = 0.5 * erfc(sqrt(SNR_linear))

| SNR (dB) | SNR (linear) | sqrt(SNR) | Formula | BER |
|----------|--------------|-----------|---------|-----|
| 10       | 10.0         | 3.162     | =0.5*ERFC.PRECISE(B2) | 3.87E-06 |
| 12       | 15.85        | 3.981     | =0.5*ERFC.PRECISE(B3) | 3.40E-08 |
| 14       | 25.12        | 5.012     | =0.5*ERFC.PRECISE(B4) | 1.31E-12 |
| 15       | 31.62        | 5.623     | =0.5*ERFC.PRECISE(B5) | 4.68E-15 |
| 16       | 39.81        | 6.310     | =0.5*ERFC.PRECISE(B6) | 6.22E-19 |

Note: For BER < 10^-15, ERFC.PRECISE maintains precision
Standard 1-ERF(x) would lose significant digits for these values
Industry target: BER < 10^-12 for error-free transmission
```

### 2. Rare Event Probability in Risk Analysis

**Scenario**: A financial risk analyst needs to calculate the probability of extreme market movements (black swan events) assuming normally distributed returns, where the probabilities are extremely small but financially significant.

**Implementation**: Use ERFC.PRECISE to accurately calculate the probability of returns exceeding multiple standard deviations from the mean.

```
P(|Return| > n*sigma) = erfc(n/sqrt(2)) for two-tailed

| Event | Standard Deviations | Formula | Probability | Frequency |
|-------|---------------------|---------|-------------|-----------|
| Typical | 2 sigma | =ERFC.PRECISE(2/SQRT(2)) | 4.55% | 1 in 22 days |
| Unusual | 3 sigma | =ERFC.PRECISE(3/SQRT(2)) | 0.27% | 1 in 370 days |
| Rare | 4 sigma | =ERFC.PRECISE(4/SQRT(2)) | 0.0063% | 1 in 16,000 days |
| Very Rare | 5 sigma | =ERFC.PRECISE(5/SQRT(2)) | 5.7E-05% | 1 in 1.7M days |
| Extreme | 6 sigma | =ERFC.PRECISE(6/SQRT(2)) | 2.0E-07% | 1 in 500M days |
| Black Swan | 7 sigma | =ERFC.PRECISE(7/SQRT(2)) | 2.6E-10% | 1 in 390B days |

Note: Real markets have "fat tails" - actual rare events more frequent
These calculations assume perfect normal distribution
```

### 3. Semiconductor Process Capability Analysis

**Scenario**: A semiconductor process engineer needs to calculate defect rates for processes where specifications are set at multiple sigma levels, requiring precise tail probability calculations for yield prediction.

**Implementation**: Use ERFC.PRECISE to calculate the proportion of product outside specification limits for Six Sigma and beyond analysis.

```
For symmetric specs at +/- n*sigma from target:
Defect rate = erfc(n/sqrt(2))

| Process Capability | Sigma Level | Formula | Defect Rate | DPMO |
|--------------------|-------------|---------|-------------|------|
| Poor               | 2 sigma     | =ERFC.PRECISE(2/SQRT(2)) | 4.55% | 45,500 |
| Marginal           | 3 sigma     | =ERFC.PRECISE(3/SQRT(2)) | 0.27% | 2,700 |
| Capable            | 4 sigma     | =ERFC.PRECISE(4/SQRT(2)) | 63.4 ppm | 63.4 |
| Good               | 5 sigma     | =ERFC.PRECISE(5/SQRT(2)) | 0.57 ppm | 0.57 |
| Six Sigma          | 6 sigma     | =ERFC.PRECISE(6/SQRT(2)) | 2.0 ppb | 0.002 |

DPMO = Defects Per Million Opportunities = Defect Rate * 1,000,000
Six Sigma with 1.5 sigma shift: 3.4 DPMO (industry standard)

Yield at n-sigma: Y = 1 - ERFC.PRECISE(n/SQRT(2))
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| ERFC.PRECISE availability | Excel 2010+ | Not available |
| Equivalent function | ERFC.PRECISE(x) | ERFC(x) |
| Precision | ~15 significant digits | ~15 significant digits |
| Negative argument support | Yes | Yes (via ERFC) |
| Array support | Yes | Yes (via ERFC) |
| Return range | 0 to 2 | 0 to 2 |

For cross-platform compatibility, use ERFC instead of ERFC.PRECISE.

## Tips and Best Practices

1. **Use for extreme tail calculations**: ERFC.PRECISE is essential when calculating probabilities smaller than 10^-10, where 1-ERF would lose precision.

2. **Q-function relationship**: Q(x) = 0.5 * ERFC.PRECISE(x/SQRT(2)) is the standard form used in communications engineering.

3. **Verify precision**: For x > 5, compare ERFC.PRECISE(x) with 1-ERF(x) to see the precision difference; ERFC.PRECISE maintains accuracy.

4. **Symmetric property**: erfc(-x) = 2 - erfc(x), so ERFC.PRECISE(-x) = 2 - ERFC.PRECISE(x).

5. **Cross-platform code**: Use ERFC for compatibility with Google Sheets; reserve ERFC.PRECISE for Excel-only workbooks where naming clarity is prioritized.

6. **Orders of magnitude**: Combine with LOG10 for readability: LOG10(ERFC.PRECISE(x)) shows the order of magnitude directly.

7. **Known values for verification**: erfc(1) ≈ 0.1573, erfc(2) ≈ 0.00468, erfc(3) ≈ 2.21E-05, erfc(4) ≈ 1.54E-08.

8. **Documentation benefit**: Using ERFC.PRECISE makes it explicit that the complementary function is intentionally chosen for precision, not just convenience.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[ERFC]] | Complementary error function (identical calculation) |
| [[ERF]] | Error function: erf(x) = 1 - erfc(x) |
| [[ERF.PRECISE]] | Precise error function |
| [[NORM.S.DIST]] | Standard normal distribution functions |

### Commonly Used With

- **[[SQRT]]** - Convert between erfc scale and z-scores
- **[[LOG10]]** - Display orders of magnitude for tiny probabilities
- **[[ERF.PRECISE]]** - Related function; erfc(x) = 1 - erf(x)
- **[[POWER]]** - SNR calculations in dB conversions
- **[[NORM.S.DIST]]** - Alternative normal distribution calculations
- **[[EXP]]** - Related Gaussian function calculations

## Official Documentation

- [Microsoft Excel ERFC.PRECISE Documentation](https://support.microsoft.com/en-us/office/erfc-precise-function-e90e6bab-f45e-45df-b2ac-cd2eb4d4a273)
- [Google Sheets ERFC Documentation](https://support.google.com/docs/answer/9116267) (Use ERFC instead)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Function introduced for consistency in function naming |
| Excel 2013 | 2013 | No changes |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | N/A | Not available; use ERFC instead |
