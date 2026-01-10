---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- probability-distributions
- normal-distribution
- gaussian-distribution
- continuous-distributions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- NORMDIST
- Normal Distribution
- Gaussian Distribution
tags:
- statistics
- probability
- bell-curve
- distribution-functions
---

# NORM.DIST

## Description

NORM.DIST returns the normal distribution for a specified value, mean, and standard deviation. The normal distribution, also called the Gaussian distribution or "bell curve," is the most important probability distribution in statistics. It describes how values are spread around an average, with most values clustering near the center and fewer values appearing as you move further away. Think of it like heights in a population: most people are close to average height, with progressively fewer people being very short or very tall.

The function can return two different types of results depending on the cumulative parameter. When cumulative is TRUE, it returns the cumulative distribution function (CDF), which tells you the probability that a randomly selected value will be less than or equal to your specified value. When cumulative is FALSE, it returns the probability density function (PDF), which shows the relative likelihood of observing a particular value. The PDF is useful for visualizing the shape of the distribution, while the CDF is essential for calculating probabilities.

Use NORM.DIST when you need to calculate probabilities for normally distributed data, such as test scores, measurement errors, heights, weights, or manufacturing tolerances. It is fundamental for quality control (calculating defect rates), finance (modeling returns), scientific research (analyzing experimental data), and any situation where you expect data to follow the classic bell-shaped pattern. The function is particularly powerful for answering questions like "What percentage of products will fall within specification?" or "How likely is a test score above 90?"

Both Excel and Google Sheets implement NORM.DIST identically, following the mathematical definition of the normal distribution. The function replaced the older NORMDIST function (without the period) starting in Excel 2010, though the legacy version remains available for backward compatibility. Google Sheets supports both naming conventions. The underlying calculation uses the mathematical formula involving Euler's number (e) and pi, but the function handles all complexity for you.

## Syntax

> [!info] NORM.DIST Syntax
>
> ```excel
> =NORM.DIST(x, mean, standard_dev, cumulative)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| `x` | The value for which you want the distribution | Yes |
| `mean` | The arithmetic mean (average) of the distribution | Yes |
| `standard_dev` | The standard deviation of the distribution (must be positive) | Yes |
| `cumulative` | TRUE returns the cumulative distribution function; FALSE returns the probability density function | Yes |

## Examples

```excel
=NORM.DIST(42, 40, 5, TRUE)
```
**Result:** `0.6554` (approximately 0.655421742)
**Explanation:** Returns the probability that a value from a normal distribution with mean 40 and standard deviation 5 is less than or equal to 42. About 65.5% of values fall at or below 42.

```excel
=NORM.DIST(42, 40, 5, FALSE)
```
**Result:** `0.0737` (approximately 0.073654028)
**Explanation:** Returns the probability density at x=42 for a normal distribution with mean 40 and standard deviation 5. This is the height of the bell curve at that point, useful for plotting.

```excel
=NORM.DIST(100, 100, 15, TRUE)
```
**Result:** `0.5`
**Explanation:** When x equals the mean, the cumulative probability is always exactly 0.5 (50%), because half the values in a normal distribution fall below the mean.

```excel
=NORM.DIST(85, 100, 15, TRUE)
```
**Result:** `0.1587` (approximately 0.158655254)
**Explanation:** For an IQ distribution (mean 100, standard deviation 15), about 15.87% of the population has an IQ of 85 or below. This is one standard deviation below the mean.

```excel
=NORM.DIST(130, 100, 15, TRUE)
```
**Result:** `0.9772` (approximately 0.977249868)
**Explanation:** About 97.72% of the population has an IQ of 130 or below, meaning only about 2.28% have an IQ above 130 (often called "gifted" threshold).

```excel
=1 - NORM.DIST(120, 100, 15, TRUE)
```
**Result:** `0.0912` (approximately 0.091211219)
**Explanation:** To find the probability of a value being GREATER than x, subtract the cumulative probability from 1. About 9.12% of people have an IQ above 120.

```excel
=NORM.DIST(75, 70, 10, TRUE) - NORM.DIST(60, 70, 10, TRUE)
```
**Result:** `0.5328` (approximately 0.532807207)
**Explanation:** To find the probability of a value falling BETWEEN two numbers, subtract the lower CDF from the upper CDF. About 53.28% of values fall between 60 and 75 in this distribution.

```excel
=NORM.DIST(A2, AVERAGE(A:A), STDEV.S(A:A), TRUE)
```
**Result:** (Varies based on data)
**Explanation:** Calculates the percentile rank of cell A2 within a dataset, using the sample's actual mean and standard deviation. This is useful for determining where a specific value falls relative to the whole dataset.

```excel
=NORM.DIST(1000, 1000, 50, FALSE) * 2 * 50
```
**Result:** `0.7979` (approximately)
**Explanation:** Multiplying the PDF by a bin width (here 100, represented as 2*50) gives an approximation of the probability of falling within that bin, useful for creating histograms.

```excel
=IF(NORM.DIST(B2, 500, 100, TRUE) < 0.05, "Below Spec", "Pass")
```
**Result:** "Below Spec" or "Pass"
**Explanation:** Uses the cumulative distribution to flag items that fall in the bottom 5% of expected values. This is common in quality control for identifying potential defects.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric arguments provided | Ensure x, mean, and standard_dev are numbers or cell references containing numbers |
| `#NUM!` | Standard deviation is zero or negative | Standard deviation must be a positive number greater than 0 |
| `#NUM!` | Result is too large or small for Excel to handle | Check for extreme x values far from the mean; consider using logarithmic transformations |
| Unexpected 0 | x is many standard deviations below the mean | Normal for extreme values; the probability becomes vanishingly small |
| Unexpected 1 | x is many standard deviations above the mean | Normal for extreme values; cumulative probability approaches 1 |

## Use Cases

### Quality Control in Manufacturing

**Scenario:** A pharmaceutical company produces tablets that should weigh 500mg. The production process has a standard deviation of 10mg, and tablets outside 480-520mg must be rejected.

**Implementation:** The quality team uses NORM.DIST to calculate expected rejection rates:
```excel
=1 - (NORM.DIST(520, 500, 10, TRUE) - NORM.DIST(480, 500, 10, TRUE))
```
This returns approximately 0.0455, meaning about 4.55% of tablets will be rejected. The team can then evaluate whether process improvements are cost-effective.

**Business Impact:** By understanding the theoretical rejection rate, the company can set realistic production targets, calculate raw material needs accounting for waste, and identify when actual rejection rates indicate a process problem requiring investigation.

### Academic Grading Curves

**Scenario:** A professor wants to assign grades on a curve where 10% receive As, 20% receive Bs, 40% receive Cs, 20% receive Ds, and 10% receive Fs. The class mean score is 72 with a standard deviation of 12.

**Implementation:** Using NORM.DIST with various thresholds:
```excel
=NORM.INV(0.90, 72, 12)  // A threshold: ~87.4
=NORM.INV(0.70, 72, 12)  // B threshold: ~78.3
=NORM.INV(0.30, 72, 12)  // C threshold: ~65.7
=NORM.INV(0.10, 72, 12)  // D threshold: ~56.6
```
Then verify with NORM.DIST:
```excel
=1 - NORM.DIST(87.4, 72, 12, TRUE)  // Confirms ~10% As
```

**Business Impact:** This approach ensures consistent, defensible grading that accounts for class difficulty variations while maintaining predetermined grade distributions across semesters.

### Financial Risk Assessment

**Scenario:** An investment portfolio has historical daily returns averaging 0.05% with a standard deviation of 1.2%. The risk manager needs to calculate the probability of a daily loss exceeding 3%.

**Implementation:** Calculate the probability of returns below -3%:
```excel
=NORM.DIST(-3, 0.05, 1.2, TRUE)
```
This returns approximately 0.0055, indicating about a 0.55% chance of losing more than 3% in a single day.

**Business Impact:** This probability helps set Value at Risk (VaR) limits, determine capital reserves, and communicate risk to stakeholders. The calculation supports regulatory compliance and informed decision-making about position sizes and hedging strategies.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function name | NORM.DIST | NORM.DIST (also NORMDIST) |
| Legacy support | NORMDIST still works | Both versions fully supported |
| Precision | 15 significant digits | 15 significant digits |
| Array formulas | Supported with Ctrl+Shift+Enter or dynamic arrays | Natively supports arrays |
| Cumulative parameter | Required (TRUE/FALSE or 1/0) | Required (TRUE/FALSE or 1/0) |
| Localization | Function name may vary by language | English only |

Both platforms produce identical results within floating-point precision limits. The primary difference is that Google Sheets handles array operations more naturally without special entry methods.

## Tips and Best Practices

1. **Choose the right cumulative setting:** Use TRUE (cumulative) for probability calculations like "what percent falls below this value?" Use FALSE (density) only when you need to visualize the distribution shape or calculate specific mathematical properties.

2. **Remember the symmetry:** The normal distribution is symmetric around the mean. The probability of being 2 standard deviations above the mean equals the probability of being 2 standard deviations below. Use this to simplify calculations and verify results.

3. **Know the 68-95-99.7 rule:** Approximately 68% of values fall within 1 standard deviation of the mean, 95% within 2, and 99.7% within 3. This quick mental check helps validate your NORM.DIST results.

4. **Use NORM.DIST with NORM.INV as inverses:** These functions are mathematical inverses. NORM.DIST converts values to probabilities; NORM.INV converts probabilities back to values. Verify your work by applying one after the other.

5. **Validate the normality assumption:** NORM.DIST assumes your data follows a normal distribution. For highly skewed or bounded data (like incomes or wait times), consider alternative distributions like lognormal or exponential.

6. **Calculate between-value probabilities correctly:** To find P(a < x < b), always use NORM.DIST(b, mean, sd, TRUE) - NORM.DIST(a, mean, sd, TRUE). Never subtract density (FALSE) values, as they represent different concepts.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[NORM.S.DIST]] | Standard normal distribution (mean=0, standard deviation=1) |
| [[NORM.INV]] | Returns the inverse of NORM.DIST (value for a given probability) |
| [[LOGNORM.DIST]] | Log-normal distribution for positively skewed data |
| [[T.DIST]] | Student's t-distribution for small samples |
| [[NORMDIST]] | Legacy version of NORM.DIST (compatibility) |

### Commonly Used Together

**[[NORM.INV]]** - The mathematical inverse of NORM.DIST, converting probabilities to values

*Finding the value at a specific percentile:*
```excel
=NORM.INV(0.95, 100, 15)
```
Returns 124.67, the 95th percentile of IQ scores. Use with NORM.DIST to verify: NORM.DIST(124.67, 100, 15, TRUE) returns 0.95.

**[[AVERAGE]]** and **[[STDEV.S]]** - Calculate distribution parameters from sample data

*Using sample statistics in NORM.DIST:*
```excel
=NORM.DIST(B2, AVERAGE(B:B), STDEV.S(B:B), TRUE)
```
This calculates where a specific value falls in the distribution of your actual data.

**[[STANDARDIZE]]** - Converts values to z-scores for use with NORM.S.DIST

*Converting to standard normal:*
```excel
=NORM.S.DIST(STANDARDIZE(85, 100, 15), TRUE)
```
This is equivalent to NORM.DIST(85, 100, 15, TRUE) but shows the standardization step explicitly.

## Official Documentation

- [Microsoft Excel NORM.DIST](https://support.microsoft.com/en-us/office/norm-dist-function-edb1cc14-a21c-4e53-839d-8082074c9f8d)
- [Google Sheets NORM.DIST](https://support.google.com/docs/answer/3094021)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2010 | 2010 | Introduced NORM.DIST as replacement for NORMDIST with improved accuracy |
| Excel 2013 | 2013 | Enhanced precision for extreme values |
| Excel 365 | Ongoing | Supports dynamic array spilling |
| Google Sheets | 2006 | Available since launch as NORMDIST |
| Google Sheets | 2010 | Added NORM.DIST alias for Excel compatibility |
