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
- complementary-error-function
- erfc-integral
- tail-probability
tags:
- engineering-function
- error-function
- complementary
- probability
- statistics
---

# ERFC

## Description

ERFC calculates the complementary error function, defined as erfc(x) = 1 - erf(x), where erf(x) is the error function. The complementary error function represents the probability that a standard normal random variable falls outside a specified range, making it essential for calculating tail probabilities in statistics, bit error rates in communications, and rare event probabilities in reliability engineering. ERFC provides better numerical precision than 1-ERF(x) for large positive values of x where erf(x) approaches 1.

Mathematically, erfc(x) = (2/sqrt(pi)) * integral from x to infinity of exp(-t^2) dt. This integral represents the "complementary" area under the Gaussian curve, measuring the probability in the tails of the distribution. While erf(x) approaches 1 as x increases, erfc(x) approaches 0, and using ERFC directly avoids the numerical precision loss that would occur when subtracting a number very close to 1 from 1 (catastrophic cancellation).

The complementary error function is particularly important in communications engineering for calculating bit error rates (BER), where the probability of errors is typically very small (10^-6 or smaller). In reliability engineering, ERFC helps calculate the probability of extreme events. In physics, it appears in solutions to diffusion equations, heat transfer problems, and wave propagation. The Q-function commonly used in communications is related by Q(x) = 0.5 * erfc(x/sqrt(2)).

Both Excel and Google Sheets implement ERFC, accepting a single numeric argument. The function returns values between 0 and 2: erfc(0) = 1, erfc approaches 0 as x approaches positive infinity, and erfc approaches 2 as x approaches negative infinity. For negative arguments, erfc(-x) = 2 - erfc(x), which follows from the odd symmetry of erf. ERFC provides full floating-point precision (~15 significant digits) even for large arguments where 1-ERF would lose precision.

## Syntax

> [!NOTE] Syntax
> ```
> ERFC(x)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **x** | The lower limit of integration for the complementary error function. Represents the value from which to integrate to infinity. Can be any real number. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=ERFC(0)` | 1 | erfc(0) = 1 - erf(0) = 1 - 0 = 1 |
| `=ERFC(1)` | 0.15730 | erfc(1) = 1 - 0.8427 = 0.1573 |
| `=ERFC(2)` | 0.00468 | erfc(2) is small; erf(2) nearly 1 |
| `=ERFC(3)` | 0.0000221 | Very small tail probability |
| `=ERFC(-1)` | 1.84270 | erfc(-x) = 2 - erfc(x) |
| `=ERFC(0.5)` | 0.47950 | Intermediate value |
| `=ERFC(4)` | 1.54E-08 | Extremely small for large x |
| `=ERFC(5)` | 1.54E-12 | ERFC maintains precision here |
| `=0.5*ERFC(A1/SQRT(2))` | Q(A1) | Q-function for communications |
| `=1-0.5*ERFC(-A1/SQRT(2))` | CDF(A1) | Normal CDF via ERFC |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric argument | Ensure argument is a numeric value |
| `#VALUE!` | Text input | Use VALUE() to convert text to number |
| `#NAME?` | Function not available | Enable Analysis ToolPak in older Excel versions |
| Precision loss | Using 1-ERF instead of ERFC | Use ERFC directly for better precision with large x |
| Unexpected large value | Negative argument confusion | Remember erfc(-x) = 2 - erfc(x), ranges from 0 to 2 |

## Use Cases

### 1. Digital Communications - Bit Error Rate Analysis

**Scenario**: A telecommunications engineer needs to calculate the bit error rate (BER) for various modulation schemes operating at different signal-to-noise ratios (SNR), essential for system design and performance verification.

**Implementation**: Use ERFC to calculate BER for BPSK and QPSK modulation, where BER = 0.5 * erfc(sqrt(Eb/N0)) for BPSK.

```
| Eb/N0 (dB) | Eb/N0 (linear) | Formula | BER |
|------------|----------------|---------|-----|
| 0          | 1.00           | =0.5*ERFC(SQRT(B2)) | 7.865E-02 |
| 2          | 1.58           | =0.5*ERFC(SQRT(B3)) | 5.628E-02 |
| 4          | 2.51           | =0.5*ERFC(SQRT(B4)) | 3.751E-02 |
| 6          | 3.98           | =0.5*ERFC(SQRT(B5)) | 2.388E-02 |
| 8          | 6.31           | =0.5*ERFC(SQRT(B6)) | 1.909E-03 |
| 10         | 10.00          | =0.5*ERFC(SQRT(B7)) | 3.872E-06 |
| 12         | 15.85          | =0.5*ERFC(SQRT(B8)) | 9.006E-10 |

Note: BER = 0.5*ERFC(SQRT(Eb/N0)) for BPSK
Linear conversion: Eb/N0_linear = 10^(Eb/N0_dB/10)
Target BER typically 1E-6 to 1E-9 for reliable communications
```

### 2. Statistical Outlier Detection

**Scenario**: A data scientist needs to calculate the probability of observing values beyond certain thresholds in normally distributed data, identifying statistically significant outliers for quality control or anomaly detection.

**Implementation**: Use ERFC to calculate two-tailed probability of values beyond a given z-score threshold, representing outlier probability.

```
P(|Z| > z) = erfc(z/sqrt(2)) for two-tailed test

| z-threshold | Formula | P(|Z| > z) | Interpretation |
|-------------|---------|------------|----------------|
| 1.0         | =ERFC(1/SQRT(2)) | 31.73% | ~1 in 3 |
| 1.5         | =ERFC(1.5/SQRT(2)) | 13.36% | ~1 in 7 |
| 2.0         | =ERFC(2/SQRT(2)) | 4.55% | ~1 in 22 |
| 2.5         | =ERFC(2.5/SQRT(2)) | 1.24% | ~1 in 80 |
| 3.0         | =ERFC(3/SQRT(2)) | 0.27% | ~1 in 370 |
| 4.0         | =ERFC(4/SQRT(2)) | 0.0063% | ~1 in 16,000 |
| 5.0         | =ERFC(5/SQRT(2)) | 5.7E-05% | ~1 in 1.7 million |

Significance levels:
- z > 2: marginally significant (p < 0.05)
- z > 3: significant (p < 0.003)
- z > 4: highly significant (p < 0.0001)
```

### 3. Reliability Engineering - Survival Probability

**Scenario**: A reliability engineer needs to model time-to-failure for components where the logarithm of failure time follows a normal distribution (lognormal failure model), calculating the probability of surviving beyond a specified time.

**Implementation**: Use ERFC to calculate survival probability (reliability) at various time points for a lognormal distribution model.

```
For lognormal distribution: R(t) = 0.5 * erfc((ln(t) - mu) / (sigma * sqrt(2)))

Given: mu = 5 (median life e^5 = 148.4 hours), sigma = 0.5

| Time (hrs) | ln(t) | z = (ln(t)-mu)/sigma | Formula | R(t) |
|------------|-------|----------------------|---------|------|
| 50         | 3.91  | -2.18                | =0.5*ERFC(D2/SQRT(2)) | 98.54% |
| 100        | 4.61  | -0.78                | =0.5*ERFC(D3/SQRT(2)) | 78.23% |
| 148        | 5.00  | 0.00                 | =0.5*ERFC(D4/SQRT(2)) | 50.00% |
| 200        | 5.30  | 0.60                 | =0.5*ERFC(D5/SQRT(2)) | 27.43% |
| 300        | 5.70  | 1.41                 | =0.5*ERFC(D6/SQRT(2)) | 7.93% |
| 500        | 6.21  | 2.43                 | =0.5*ERFC(D7/SQRT(2)) | 0.76% |

B10 life (10% failure): solve R(t) = 0.90
z = -1.28, t = exp(5 + (-1.28)*0.5) = 79.4 hours
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Yes | Yes |
| Argument count | 1 | 1 |
| Return range | 0 to 2 | 0 to 2 |
| Negative arguments | Supported | Supported |
| Precision for large x | ~15 significant digits | ~15 significant digits |
| Array support | Yes | Yes |

Both platforms implement ERFC identically with no functional differences.

## Tips and Best Practices

1. **Use ERFC for small probabilities**: When calculating P(X > threshold) for large thresholds, use ERFC directly instead of 1-ERF to maintain precision.

2. **Q-function conversion**: The communications Q-function is Q(x) = 0.5 * ERFC(x/SQRT(2)). This is widely used for BER calculations.

3. **Symmetric property**: Remember erfc(-x) = 2 - erfc(x). This means ERFC can return values up to 2 for negative arguments.

4. **Relationship to ERF**: While ERFC(x) = 1 - ERF(x) mathematically, using ERFC directly gives better precision for positive x where ERF approaches 1.

5. **Normal distribution tails**: For standard normal tail probability P(Z > z), use 0.5 * ERFC(z/SQRT(2)).

6. **Verify with known values**: erfc(1) ≈ 0.1573, erfc(2) ≈ 0.00468, erfc(3) ≈ 0.0000221.

7. **Diffusion tail concentration**: In diffusion problems, concentration in the "far field" involves erfc(x/(2*sqrt(D*t))).

8. **Combine with LOG for orders of magnitude**: Use LOG10(ERFC(x)) to find the order of magnitude of very small tail probabilities.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[ERF]] | Error function: erf(x) = 1 - erfc(x) |
| [[ERF.PRECISE]] | Precise error function |
| [[ERFC.PRECISE]] | Precise complementary error function |
| [[NORM.S.DIST]] | Standard normal CDF |

### Commonly Used With

- **[[SQRT]]** - Convert between erfc and normal distribution scales
- **[[ERF]]** - Related calculations; erfc(x) = 1 - erf(x)
- **[[LOG10]]** - Calculate orders of magnitude for small probabilities
- **[[EXP]]** - Related Gaussian calculations
- **[[NORM.S.DIST]]** - Alternative normal distribution functions
- **[[POWER]]** - BER calculations often involve powers of 10

## Official Documentation

- [Microsoft Excel ERFC Documentation](https://support.microsoft.com/en-us/office/erfc-function-736e0318-70ba-4e8b-8d08-461fe68b71b3)
- [Google Sheets ERFC Documentation](https://support.google.com/docs/answer/9116267)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Available via Analysis ToolPak add-in |
| Excel 2007 | 2007 | Integrated into standard function library |
| Excel 2010 | 2010 | Added ERFC.PRECISE variant |
| Excel 2013 | 2013 | No changes |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2019 | Added ERFC function |
