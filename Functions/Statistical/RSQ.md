---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- coefficient-of-determination
- regression-analysis
- model-fit
- r-squared
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- R-Squared
- Coefficient of Determination
- R2
- Explained Variance
tags:
- statistical
- regression
- r-squared
- model-fit
- explained-variance
---

# RSQ

## Description

**RSQ** returns the square of the Pearson correlation coefficient (R-squared or R^2), known as the coefficient of determination. This critical statistic measures the proportion of variance in the dependent variable (Y) that is explained by the linear relationship with the independent variable (X). R-squared ranges from 0 to 1: a value of 0.80 means 80% of Y's variance is explained by X, while 20% remains unexplained by the linear model. This makes R-squared the primary metric for assessing how well your regression line fits your data.

The mathematical relationship between RSQ and CORREL is straightforward: RSQ(Y, X) = CORREL(X, Y)^2. However, the interpretation differs fundamentally. Correlation tells you direction and strength of linear association (-1 to +1), while R-squared tells you explanatory power (0 to 1). A correlation of 0.7 sounds strong, but squaring it reveals that only 49% of variance is explained, meaning more than half of Y's variation has nothing to do with X. This reality check is why R-squared is essential for understanding what regression can and cannot explain.

**Interpreting R-squared requires context.** In physics experiments with controlled conditions, R-squared of 0.99 is expected. In social science research, R-squared of 0.30 might be excellent because human behavior has many influences. In financial markets, even R-squared of 0.10 can be valuable if it represents consistent edge. Never judge R-squared in absolute terms; always compare to what is typical and meaningful in your specific domain.

**Critical caveats about R-squared:** High R-squared does not mean your model is correct. A model can have high R-squared but be fundamentally wrong (omitted variables, wrong functional form). R-squared always increases when you add more predictors, even if they are irrelevant (use adjusted R-squared from LINEST for multiple regression). R-squared does not indicate causation. Two trending time series can show R-squared near 1.0 while having no true relationship. And R-squared only measures linear fit; a perfect quadratic relationship might show low R-squared with simple linear regression. Always visualize your data rather than relying solely on R-squared.

## Syntax

> [!f(x)] RSQ Syntax
>
> ```
> =RSQ(known_y's, known_x's)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `known_y's` | Yes | The dependent variable data (the values you want to predict). Can be a cell range, named range, or array of numeric values. |
| `known_x's` | Yes | The independent variable data (the predictor). Must have the same number of data points as known_y's. Each position corresponds to the same observation. |

### Return Value

Returns a numeric value between 0 and 1 representing the coefficient of determination. Returns #N/A if arrays have different numbers of numeric data points. Returns #DIV/0! if either array has zero variance.

## Examples

> [!f(x)] RSQ Examples

### Example 1: Strong Fit
```
=RSQ(B2:B11, A2:A11)
```
Where A2:A11 (X - Study Hours): 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
And B2:B11 (Y - Test Scores): 52, 58, 65, 68, 75, 80, 84, 90, 94, 99

**Result:** 0.98 (approximately)

**Explanation:** 98% of test score variance is explained by study hours. This is an exceptionally strong linear relationship where study time almost perfectly predicts test performance.

---

### Example 2: Moderate Fit
```
=RSQ(Sales, Advertising)
```
Where Sales and Advertising are monthly figures

**Result:** 0.45 (example value)

**Explanation:** 45% of sales variance is explained by advertising. More than half the variation is due to other factors (seasonality, competition, product quality). The relationship exists but is far from deterministic.

---

### Example 3: Weak Fit
```
=RSQ(StockReturns_A, StockReturns_B)
```
**Result:** 0.15 (example value)

**Explanation:** Only 15% of Stock A's returns are explained by Stock B's returns. The stocks have some relationship but mostly move independently. This might be desirable for diversification.

---

### Example 4: No Relationship
```
=RSQ(RandomY, RandomX)
```
Where both ranges contain random, unrelated values

**Result:** Approximately 0.00 to 0.05 with moderate sample size

**Explanation:** Random data shows R-squared near zero. Small samples might show spurious higher values by chance, which is why sample size matters.

---

### Example 5: Relationship to Correlation
```
R-squared: =RSQ(Y, X)
From Correlation: =CORREL(X, Y)^2
```
**Result:** Both formulas return identical values

**Explanation:** RSQ equals correlation squared. This is why a "strong" correlation of 0.70 means only 49% explained variance. Squaring reveals the true explanatory power.

---

### Example 6: Perfect Linear Relationship
```
=RSQ(B2:B6, A2:A6)
```
Where X = 1, 2, 3, 4, 5 and Y = 3, 6, 9, 12, 15 (perfect Y = 3X)

**Result:** 1.00

**Explanation:** R-squared of 1 means 100% of Y variance is explained by X. All points fall exactly on the regression line. This is rare in real data but common in calculated values.

---

### Example 7: Model Comparison
```
Model 1: =RSQ(Sales, Price)
Model 2: =RSQ(Sales, Temperature)
```
**Result:** Compare which predictor explains more variance

**Explanation:** Higher R-squared indicates the predictor explains more of the sales variation. However, the best model might include both predictors (use LINEST for multiple regression).

---

### Example 8: Time Trend Analysis
```
=RSQ(Revenue, YearNumber)
```
**Result:** 0.92 (example value)

**Explanation:** 92% of revenue variance is explained by time trend. High R-squared in time series can be misleading; both variables trending upward creates spurious correlation.

---

### Example 9: Unexplained Variance
```
Explained: =RSQ(Y, X)
Unexplained: =1 - RSQ(Y, X)
```
**Result:** Partition of total variance

**Explanation:** If R-squared is 0.65, then 65% is explained by the linear model and 35% is unexplained (residual variance from other factors, randomness, or non-linear relationships).

---

### Example 10: Assessing Prediction Quality
```
R-squared: =RSQ(Y, X)
Interpretation: =IF(RSQ(Y, X) > 0.7, "Good for prediction", IF(RSQ(Y, X) > 0.4, "Moderate prediction", "Poor prediction"))
```
**Result:** Categorical assessment of model quality

**Explanation:** This provides rough guidance. Actual thresholds depend on your specific domain and decision context.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Arrays have different numbers of numeric data points | Ensure both ranges have equal length and matching observations |
| `#DIV/0!` | One or both arrays have zero variance (all identical values) | R-squared is undefined when there is no variation to explain |
| `#VALUE!` | Non-numeric text provided directly as argument | Use cell references; direct text arguments cause errors |
| `High R-squared but wrong model` | Overfitting or spurious correlation | Visualize residuals; test on holdout data; use domain knowledge |
| `R-squared = 0 with obvious relationship` | Non-linear relationship present | Plot data; consider transformations or non-linear models |

## Understanding and Interpreting R-Squared

### What R-Squared Tells You

R-squared measures the proportion of variance in Y that is linearly associated with X:

| R-squared | Interpretation | Typical Context |
|-----------|----------------|-----------------|
| 0.90-1.00 | Excellent fit | Physics, engineering with controlled conditions |
| 0.70-0.90 | Good fit | Quality industrial data, simple relationships |
| 0.50-0.70 | Moderate fit | Business, economics, some marketing |
| 0.30-0.50 | Weak but meaningful | Social sciences, psychology |
| 0.00-0.30 | Poor fit | Complex human behavior, financial markets |

**Key insight:** These ranges are guidelines, not rules. What matters is whether R-squared is high enough for your specific decision context.

### What R-Squared Does NOT Tell You

1. **Not about causation:** High R-squared does not mean X causes Y. Both might be caused by an unmeasured third variable.

2. **Not about correct model:** Your model can have high R-squared but be fundamentally misspecified (wrong functional form, omitted variables).

3. **Not about prediction accuracy:** R-squared measures fit to existing data. Future predictions depend on model stability and similarity to training data.

4. **Not about practical significance:** Even high R-squared might represent a relationship too weak to be useful, or low R-squared might capture a valuable signal in a noisy domain.

5. **Not absolute:** R-squared cannot be compared across different Y variables. Predicting stock returns (low R-squared expected) versus predicting exam scores from study time (high R-squared expected) are different problems.

## Use Cases

### [[Regression Model Validation]]

**Scenario:** A data analyst evaluates whether a linear model adequately captures the relationship between variables before using it for prediction.

**Implementation:**
```
=RSQ(ActualSales, PredictedSales_from_Model)
```
Or for a simple model:
```
=RSQ(Sales, Advertising)
```

**Business Application:** R-squared serves as the primary measure of model fit. Before relying on predictions, verify that R-squared is high enough for your decision context. Compare R-squared across different model specifications to identify the best approach. Report R-squared alongside predictions to communicate uncertainty.

**Technical Details:** For multiple regression, use adjusted R-squared from LINEST, which penalizes for additional predictors. Test R-squared on holdout data to assess generalization. Consider that R-squared can be high due to overfitting, especially with many predictors relative to observations.

---

### [[Forecast Reliability Assessment]]

**Scenario:** A financial analyst determines whether historical patterns provide reliable guidance for future projections.

**Implementation:**
```
Historical Fit: =RSQ(HistoricalRevenue, TimeIndex)
```
Also validate on recent out-of-sample periods.

**Business Application:** Higher R-squared in historical analysis suggests more consistent patterns, potentially supporting more confident forecasts. However, structural breaks, regime changes, and black swan events mean past fit does not guarantee future accuracy. Use R-squared as one input to confidence assessment, not the sole determinant.

**Technical Details:** Calculate R-squared on rolling windows to check stability over time. If R-squared varies dramatically, the relationship may be unstable. Consider that trending time series often show spuriously high R-squared even when the relationship is meaningless.

---

### [[Variable Selection]]

**Scenario:** A researcher compares potential predictors to determine which variables best explain the outcome of interest.

**Implementation:**
```
Predictor A: =RSQ(Outcome, VariableA)
Predictor B: =RSQ(Outcome, VariableB)
Predictor C: =RSQ(Outcome, VariableC)
```
Compare values to identify strongest predictors.

**Business Application:** When choosing among potential predictors, R-squared provides an objective comparison of explanatory power. The predictor with highest R-squared explains the most variance individually. For combined models, use LINEST for multiple regression, but start with individual R-squared for preliminary screening.

**Technical Details:** High R-squared between predictors (multicollinearity) means they provide redundant information. Choose based on theoretical relevance and data quality, not just R-squared. Be aware that complex predictors (many components) tend to show higher R-squared without necessarily being more useful.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 97 onward
- **Maximum data points:** Limited by worksheet size
- **Array handling:** Standard range inputs; dynamic arrays in 365
- **Precision:** 15 significant decimal digits
- **Empty cell handling:** Pairs with non-numeric values excluded

### Google Sheets

- **Availability:** All versions since launch
- **Maximum data points:** Limited by sheet size
- **Array handling:** Native array formula support
- **Precision:** 15 significant decimal digits
- **Empty cell handling:** Same as Excel

### Key Difference Alert

RSQ behaves identically between Excel and Google Sheets. Both calculate the same coefficient of determination using the same formula. No practical differences exist for standard use cases.

## Tips and Best Practices

1. **Do not judge R-squared in isolation:** A "good" R-squared depends entirely on context. R-squared of 0.3 might be excellent for stock returns but terrible for physics experiments. Know what is typical in your domain.

2. **Visualize before calculating:** Always plot Y versus X before computing R-squared. Non-linear relationships, outliers, and heteroscedasticity are invisible in R-squared but obvious in plots.

3. **R-squared is CORREL squared:** If you have correlation, you have R-squared. This relationship helps build intuition: r = 0.7 sounds strong, but R-squared = 0.49 reveals that less than half of variance is explained.

4. **Remember unexplained variance:** Whatever R-squared is, subtract from 1 to see unexplained variance. R-squared = 0.8 means 20% of Y variance comes from factors not in your model.

5. **Beware of time series:** Two trending series can show R-squared near 1.0 spuriously. GDP correlates with ice cream sales because both trend upward, not because of any true relationship.

6. **Use adjusted R-squared for multiple regression:** Standard R-squared always increases with more predictors, even irrelevant ones. LINEST provides adjusted R-squared that penalizes for model complexity.

7. **Check residuals:** High R-squared does not guarantee a good model. Examine residuals (actual - predicted) for patterns. Residuals should be random; patterns indicate model problems.

8. **R-squared does not prove causation:** This cannot be emphasized enough. High R-squared means X and Y vary together, not that X causes Y. Causation requires experimental design and domain knowledge.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CORREL]] | Returns Pearson correlation coefficient | When you need direction of relationship, not just explanatory power |
| [[PEARSON]] | Same as CORREL | Alternative name for correlation |
| [[LINEST]] | Returns complete regression statistics | When you need adjusted R-squared or significance tests |
| [[STEYX]] | Standard error of predicted Y | When you need prediction error magnitude, not proportion explained |

### Commonly Used Together

**[[SLOPE]] and [[INTERCEPT]]** - Regression parameters

*Complete regression analysis:*
```
R-squared: =RSQ(Y, X)
Slope: =SLOPE(Y, X)
Intercept: =INTERCEPT(Y, X)
```
R-squared tells you how well the model fits; slope and intercept define the model itself.

---

**[[STEYX]]** - Standard error of estimate

*Complementary fit measures:*
```
Proportion explained: =RSQ(Y, X)
Typical error: =STEYX(Y, X)
```
RSQ gives proportion; STEYX gives absolute error magnitude in Y's units.

---

**[[CORREL]]** - Correlation coefficient

*Mathematical relationship:*
```
R-squared: =RSQ(Y, X)
Correlation: =CORREL(X, Y)
Verification: =CORREL(X, Y)^2  // Same as RSQ
```
Understanding both helps interpret relationship strength and direction.

---

**[[LINEST]]** - Comprehensive regression statistics

*Statistical inference:*
```
=LINEST(Y, X, TRUE, TRUE)
```
Returns R-squared plus standard errors, F-statistic, and degrees of freedom for hypothesis testing.

## Official Documentation

- **Microsoft Excel:** [RSQ function](https://support.microsoft.com/en-us/office/rsq-function-d7161715-250d-4a01-b80d-a8f4fdf7faa7)
- **Google Sheets:** [RSQ function](https://support.google.com/docs/answer/3094099)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 | Original function |
| Excel 2007 | Continued support | Increased data capacity |
| Excel 2010+ | All subsequent versions | No changes to core functionality |
| Excel 365 | Continuous updates | Dynamic array support |
| Google Sheets | Original launch (2006) | Identical to Excel implementation |

---

*Last updated: 2026-01-10*
