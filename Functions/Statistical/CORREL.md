---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- correlation
- linear-relationship
- regression-analysis
- bivariate-statistics
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Correlation Coefficient
- Pearson Correlation
- Pearson r
- Linear Correlation
tags:
- statistical
- correlation
- regression
- relationship
- pearson
---

# CORREL

## Description

**CORREL** calculates the Pearson correlation coefficient between two datasets, measuring the strength and direction of the linear relationship between two variables. The result ranges from -1 to +1, where +1 indicates a perfect positive linear relationship (as X increases, Y increases proportionally), -1 indicates a perfect negative linear relationship (as X increases, Y decreases proportionally), and 0 indicates no linear relationship. This coefficient, often denoted as "r," is the foundation of correlation analysis and serves as the starting point for understanding relationships between variables in fields ranging from finance to medicine to social sciences.

The mathematical foundation of CORREL involves calculating how much two variables vary together (covariance) relative to how much they vary individually (the product of their standard deviations). The formula normalizes covariance into a dimensionless coefficient: r = Cov(X,Y) / (StdDev(X) * StdDev(Y)). This normalization is what makes correlation coefficients comparable across different scales and units. Whether you are correlating stock prices in dollars with trading volumes in millions, or heights in centimeters with weights in kilograms, the resulting r value is always between -1 and 1, making interpretation consistent regardless of the underlying data scales.

**Critical distinction: Correlation does NOT imply causation.** This is perhaps the most important caveat in all of statistics. A strong correlation between two variables does not mean one causes the other. Ice cream sales and drowning deaths are correlated because both increase in summer, not because ice cream causes drowning. A correlation between advertising spending and sales might reflect that advertising drives sales, or that companies with strong sales can afford more advertising, or that both are driven by seasonal factors. Establishing causation requires controlled experiments, temporal precedence, and elimination of confounding variables. CORREL tells you that variables move together; it cannot tell you why.

**Interpreting correlation strength:** While no universal rules exist, common guidelines suggest: |r| < 0.3 is weak, 0.3-0.7 is moderate, and > 0.7 is strong. However, context matters enormously. In physics experiments, r = 0.95 might indicate measurement problems, while in social sciences, r = 0.4 might represent a remarkably strong effect. Always consider what correlation values are typical in your field. CORREL is mathematically identical to the PEARSON function; both return the same result. Use whichever name feels more intuitive for your documentation context.

## Syntax

> [!f(x)] CORREL Syntax
>
> ```
> =CORREL(array1, array2)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array1` | Yes | The first range of values. Can be a cell range (A1:A100), a named range, or an array constant. Must contain numeric values. |
| `array2` | Yes | The second range of values. Must have the same number of data points as array1. Each position in array2 corresponds to the same position in array1. |

### Return Value

Returns a numeric value between -1 and 1 (inclusive) representing the Pearson correlation coefficient. Returns #N/A if the arrays have different numbers of numeric values or fewer than 2 data points. Returns #DIV/0! if either array has zero standard deviation (all identical values).

## Examples

> [!f(x)] CORREL Examples

### Example 1: Strong Positive Correlation
```
=CORREL(A2:A11, B2:B11)
```
Where A2:A11 contains hours studied: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
And B2:B11 contains test scores: 52, 58, 63, 71, 74, 79, 86, 91, 95, 99

**Result:** 0.9915

**Explanation:** An r value of 0.99 indicates a near-perfect positive linear relationship. As study hours increase, test scores increase proportionally. The relationship is strong enough to predict scores from hours with high accuracy.

---

### Example 2: Strong Negative Correlation
```
=CORREL(A2:A11, B2:B11)
```
Where A2:A11 contains car age (years): 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
And B2:B11 contains car value ($K): 28, 24, 21, 18, 15, 13, 11, 9, 8, 7

**Result:** -0.9832

**Explanation:** An r value of -0.98 indicates a near-perfect negative linear relationship. As car age increases, value decreases predictably. This is typical depreciation behavior for most vehicles.

---

### Example 3: No Correlation
```
=CORREL(A2:A21, B2:B21)
```
Where A2:A21 contains random X values
And B2:B21 contains unrelated random Y values

**Result:** Approximately 0.0 (may range from -0.2 to 0.2)

**Explanation:** Random, unrelated data produces correlation near zero. However, small samples of random data may show spurious correlations by chance. Always verify with adequate sample sizes (typically n > 30).

---

### Example 4: Moderate Positive Correlation in Business Data
```
=CORREL(AdvertisingSpend, SalesRevenue)
```
Where AdvertisingSpend is a named range of monthly ad budgets
And SalesRevenue is corresponding monthly sales

**Result:** 0.68

**Explanation:** A moderate positive correlation suggests advertising and sales move together, but the relationship is not deterministic. Other factors (seasonality, competition, product quality) also influence sales.

---

### Example 5: Financial Asset Correlation
```
=CORREL(StockA_Returns, StockB_Returns)
```
Where both ranges contain daily percentage returns

**Result:** 0.45

**Explanation:** Moderate correlation indicates the stocks move together to some extent but not lockstep. For portfolio diversification, investors seek assets with low or negative correlations to reduce overall risk.

---

### Example 6: Handling Missing Data
```
=CORREL(A2:A10, B2:B10)
```
Where A2:A10 contains: 5, 10, 15, 20, [empty], 30, 35, [empty], 45
And B2:B10 contains: 2, 4, 6, 8, 10, 12, 14, 16, 18

**Result:** #N/A (if empty cells cause mismatch) or correlation of paired values only

**Explanation:** CORREL requires paired data. If a cell is empty or contains text in one array, both that position and its pair are excluded. Ensure data alignment before calculating correlation.

---

### Example 7: Temperature and Ice Cream Sales
```
=CORREL(DailyTemperature, IceCreamSales)
```
**Result:** 0.92

**Explanation:** High correlation shows temperature and ice cream sales move together strongly. However, this does not mean high temperatures cause ice cream purchases. Both may be influenced by summer season and outdoor activity levels.

---

### Example 8: Using with Calculated Arrays
```
=CORREL(B2:B13-AVERAGE(B2:B13), C2:C13-AVERAGE(C2:C13))
```
**Result:** Same as =CORREL(B2:B13, C2:C13)

**Explanation:** Centering data (subtracting means) does not change correlation because the formula already accounts for means internally. This demonstrates that correlation measures relative movement, not absolute levels.

---

### Example 9: Correlation Matrix Entry
```
=CORREL(OFFSET($A$2:$A$100,0,COLUMN()-2), OFFSET($A$2:$A$100,0,ROW()-2))
```
**Result:** One cell in a correlation matrix

**Explanation:** Build a correlation matrix by using OFFSET to dynamically reference different variable columns. This technique creates symmetric matrices showing correlations between multiple variables.

---

### Example 10: Before/After Analysis
```
=CORREL(BeforeTraining, AfterTraining)
```
Where BeforeTraining contains employee skill scores before a program
And AfterTraining contains the same employees' scores after

**Result:** 0.78

**Explanation:** High correlation indicates employees who scored high before training also scored high after (and vice versa). This does not measure improvement; it measures consistency of relative ranking.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Arrays have different numbers of numeric data points | Ensure both ranges have the same size and corresponding cells both contain numbers. Remove or fill blank cells. |
| `#DIV/0!` | One or both arrays contain only identical values (zero variance) | Correlation is undefined when there is no variation. Verify data has actual variability. |
| `#VALUE!` | Non-numeric text entered directly as argument | Use cell references to numeric data only. Text cells in ranges are ignored but direct text arguments cause errors. |
| `Result = 0` | No linear relationship OR non-linear relationship exists | Plot data to check for non-linear patterns (quadratic, exponential). Use different analysis methods for non-linear relationships. |
| `Unexpected sign` | Data order mismatch between arrays | Verify array1[i] corresponds to array2[i] for all positions. Misaligned data produces meaningless results. |
| `Inflated correlation` | Outliers dominating the calculation | Identify and investigate outliers. Consider robust correlation methods or removing influential points after verification. |

## Use Cases

### [[Stock Portfolio Diversification]]

**Scenario:** An investment manager builds a portfolio seeking low correlation between assets to reduce overall portfolio volatility and manage risk.

**Implementation:**
```
=CORREL(INDEX(Returns,,1), INDEX(Returns,,2))
```
Build a correlation matrix for all assets, then select combinations with correlations below 0.3.

**Business Application:** Modern Portfolio Theory relies on correlation to optimize risk-adjusted returns. Two stocks with 0.8 correlation provide less diversification benefit than two with 0.2 correlation. During market stress, correlations often increase (called "correlation breakdown"), reducing diversification when it is needed most. Regular correlation monitoring helps identify changing market dynamics.

**Technical Details:** Use rolling windows (e.g., 60-day or 250-day returns) to calculate time-varying correlations. Correlations calculated from daily returns differ from those using monthly returns due to noise and mean-reversion effects. Consider using logarithmic returns for more normal distributions.

---

### [[Medical Research: Risk Factor Analysis]]

**Scenario:** Researchers analyze the relationship between patient biomarkers and disease outcomes to identify potential risk factors for further investigation.

**Implementation:**
```
=CORREL(CholesterolLevels, CardiacEvents)
```

**Business Application:** Screening for correlated variables helps prioritize which relationships warrant expensive clinical trials. A correlation of 0.4 between a biomarker and disease incidence justifies deeper investigation, while r = 0.05 suggests the biomarker is not promising. Correlation analysis is a first step, not a conclusion; establishing causation requires controlled studies.

**Technical Details:** Medical data often violates normality assumptions. Consider Spearman rank correlation for ordinal or non-normal data. Partial correlation controls for confounding variables (e.g., age, sex). Statistical significance testing uses Fisher z-transformation: z = 0.5 * ln((1+r)/(1-r)).

---

### [[Sales and Marketing Attribution]]

**Scenario:** A marketing team analyzes which channels and activities correlate most strongly with sales to guide budget allocation decisions.

**Implementation:**
```
Channel 1: =CORREL(EmailCampaigns, WeeklySales)
Channel 2: =CORREL(SocialMediaPosts, WeeklySales)
Channel 3: =CORREL(TVAdSpend, WeeklySales)
```
Compare correlation coefficients across all channels.

**Business Application:** Identifying highly correlated activities helps focus resources on potentially effective channels. However, correlation-based attribution has severe limitations: channels may be correlated with each other, effects may be lagged, and reverse causation (spending more on successful products) confounds interpretation. Use as directional guidance, not definitive allocation rules.

**Technical Details:** Apply lagged correlations to account for delayed effects: =CORREL(OFFSET(Marketing,0,0,n-lag,1), OFFSET(Sales,lag,0,n-lag,1)). Consider multiple regression (LINEST) to untangle overlapping channel effects. Time-series data requires stationarity testing; correlated trends can produce spuriously high correlations.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 2000 onward
- **Maximum data points:** Limited by worksheet size (1,048,576 rows in modern versions)
- **Array handling:** Pre-365 requires arrays to be ranges; 365 supports dynamic array results
- **Empty cell handling:** Pairs with empty or text cells in either position are excluded
- **Precision:** 15 significant decimal digits

### Google Sheets

- **Availability:** All versions since launch
- **Maximum data points:** Limited by sheet size (10 million cells total)
- **Array handling:** Native array formula support
- **Empty cell handling:** Same as Excel; pairs with non-numeric values excluded
- **Precision:** 15 significant decimal digits

### Key Difference Alert

CORREL behaves identically between Excel and Google Sheets for all standard use cases. The function uses the same mathematical formula and produces the same results. The only practical differences involve array formula syntax in legacy Excel versions, which require Ctrl+Shift+Enter for non-range array inputs.

## Tips and Best Practices

1. **Always visualize first:** Before calculating correlation, create a scatter plot. Non-linear relationships (quadratic, exponential) may show r near zero even with strong relationships. Anscombe's Quartet famously demonstrates how different datasets can have identical correlations but completely different patterns.

2. **Remember: correlation is not causation.** This bears repeating. A strong correlation between variables A and B could mean A causes B, B causes A, both are caused by C, or the correlation is coincidental. Never make causal claims based solely on correlation.

3. **Consider sample size:** Small samples produce unreliable correlations. With n=10, even random data can show |r| > 0.5 by chance. Use significance testing or confidence intervals. Rule of thumb: n > 30 for reasonable stability.

4. **Watch for outliers:** A single extreme point can dramatically inflate or deflate correlation. Plot your data and investigate outliers. One incorrect data entry can turn r=0.2 into r=0.8.

5. **Check for restriction of range:** If you only observe a narrow slice of possible values, correlation will be attenuated. Correlating height and weight only among basketball players (restricted height range) understates the true population correlation.

6. **Use appropriate correlation types:** CORREL calculates Pearson correlation, which assumes linear relationships and roughly normal distributions. For ordinal data or non-linear relationships, consider Spearman rank correlation (not directly available but calculable using RANK functions).

7. **Beware of confounders:** Two variables may correlate because both are influenced by a third variable. Time trends are notorious confounders. Almost everything that has grown over time (GDP, population, internet usage, avocado consumption) will correlate with everything else that has grown over time.

8. **Square r for explained variance:** The coefficient of determination (r-squared) tells you what proportion of variance in Y is "explained" by X. If r = 0.7, then r-squared = 0.49, meaning 49% of variance is explained, and 51% remains unexplained by the linear relationship.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[PEARSON]] | Returns Pearson correlation coefficient (identical to CORREL) | Personal preference; same result |
| [[RSQ]] | Returns r-squared (coefficient of determination) | When you need explained variance rather than correlation direction |
| [[COVARIANCE.P]] | Returns population covariance | When you need unstandardized co-movement measure |
| [[COVARIANCE.S]] | Returns sample covariance | For sample-based unstandardized co-movement |
| [[SLOPE]] | Returns slope of linear regression | When predicting Y from X, not just measuring relationship |

### Commonly Used Together

**[[RSQ]]** - Coefficient of determination

*Calculate explained variance from correlation:*
```
Correlation: =CORREL(X, Y)
R-squared: =RSQ(X, Y) or =CORREL(X, Y)^2
```
RSQ = r-squared tells you what proportion of Y's variance is explained by the linear relationship with X.

---

**[[SLOPE]] and [[INTERCEPT]]** - Regression parameters

*Move from correlation to prediction:*
```
Correlation: =CORREL(A2:A100, B2:B100)
Slope: =SLOPE(B2:B100, A2:A100)
Intercept: =INTERCEPT(B2:B100, A2:A100)
Prediction: =INTERCEPT(B:B, A:A) + SLOPE(B:B, A:A) * NewX
```
Correlation tells you the strength; slope and intercept let you predict.

---

**[[STEYX]]** - Standard error of predicted y

*Assess prediction uncertainty:*
```
=STEYX(Y_range, X_range)
```
After establishing correlation, STEYX indicates how far predictions typically deviate from actual values.

---

**[[LINEST]]** - Complete regression statistics

*Get comprehensive regression output:*
```
=LINEST(Y_range, X_range, TRUE, TRUE)
```
Returns slope, intercept, standard errors, R-squared, F-statistic, and more in an array.

## Understanding Correlation vs. Causation

### The Fundamental Problem

Correlation measures statistical association: when X is high, is Y typically high, low, or unpredictable? Causation means X directly influences Y. The critical insight is that **association can occur without causation** through several mechanisms:

**1. Reverse Causation:** You might find that fire truck presence correlates with fire damage. But fire trucks do not cause damage; damage causes fire trucks to be dispatched.

**2. Confounding Variables:** Ice cream sales correlate with drowning deaths. Neither causes the other; summer weather causes both. Any "third variable" that influences both X and Y creates correlation without direct causation.

**3. Coincidence:** With enough variables and data mining, random correlations appear. The number of films Nicolas Cage appeared in correlates with swimming pool drownings (spurious correlation from tylervigen.com). Given millions of potential comparisons, some will correlate by chance.

**4. Selection Bias:** If you only observe survivors, successful companies, or published studies, correlations in your sample may not reflect true relationships in the population.

### When Can We Infer Causation?

Establishing causation typically requires:
- **Temporal precedence:** Cause precedes effect
- **Controlled experiments:** Randomization eliminates confounders
- **Dose-response relationship:** More cause leads to more effect
- **Biological or theoretical plausibility:** Mechanism makes sense
- **Consistency:** Relationship replicates across studies

CORREL cannot establish any of these criteria. It is a starting point for investigation, not a conclusion.

## Interpreting R-Squared: The Coefficient of Determination

When you square the correlation coefficient (r-squared or R^2), you get the proportion of variance in Y that is "explained" by the linear relationship with X.

| Correlation (r) | R-squared | Interpretation |
|-----------------|-----------|----------------|
| 0.90 | 0.81 | 81% of Y's variance explained by X |
| 0.70 | 0.49 | 49% of Y's variance explained by X |
| 0.50 | 0.25 | 25% of Y's variance explained by X |
| 0.30 | 0.09 | 9% of Y's variance explained by X |

**Key insight:** A "moderate" correlation of 0.50 means only 25% of variance is explained, leaving 75% unexplained. This is why strong correlations (|r| > 0.7) are relatively rare in social sciences and business, while they are common in physics and engineering where relationships are more deterministic.

## Official Documentation

- **Microsoft Excel:** [CORREL function](https://support.microsoft.com/en-us/office/correl-function-995dcef7-0c0a-4bed-a3fb-239d7b68ca92)
- **Google Sheets:** [CORREL function](https://support.google.com/docs/answer/3093990)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2000 | Original function |
| Excel 2003 | Continued support | Enhanced accuracy for edge cases |
| Excel 2007+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Native dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
