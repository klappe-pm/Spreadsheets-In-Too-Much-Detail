---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- statistical
subTopics:
- central-tendency
- descriptive-statistics
- frequency-analysis
- multimodal
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- multiple-modes
- multimodal
tags:
- statistical
- central-tendency
- mode
- array-function
---

# MODE.MULT

## Description

The MODE.MULT function returns a vertical array of all values that share the highest frequency in a dataset. Unlike MODE.SNGL (or the legacy MODE), which returns only the first mode encountered when multiple modes exist, MODE.MULT captures all modal values, making it essential for analyzing multimodal distributions. This function was introduced in Excel 2010 to address the limitation of the original MODE function, which could obscure important patterns in bimodal or multimodal data.

When a dataset has a single mode (unimodal), MODE.MULT returns that single value in an array. For bimodal distributions with two equally frequent values, it returns both. For multimodal distributions, it returns all values sharing the maximum frequency. If no value appears more than once, the function returns #N/A since no mode exists. The returned array is vertical, with one mode per row, sorted in the order the modes first appear in the data.

MODE.MULT is particularly valuable for identifying patterns in survey responses, detecting multiple peak hours in activity data, finding popular options when several choices are equally common, and understanding distribution shapes. Bimodal distributions often indicate two distinct populations in the data (such as male and female heights), different operating conditions (weekday vs. weekend patterns), or the presence of subgroups that should be analyzed separately.

As an array function, MODE.MULT behavior depends on your Excel version. In Excel 365/2019 with dynamic arrays, the function spills results automatically. In earlier versions, you must select the output range, enter the formula, and press Ctrl+Shift+Enter to commit as a legacy array formula. The function ignores text, logical values, and empty cells in the reference range.

## Syntax

> [!info] Function Syntax
> ```
> =MODE.MULT(number1, [number2], ...)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| number1 | The first number, cell reference, or range for which to find modes | Yes |
| number2, ... | Additional numbers, cell references, or ranges (up to 255 total arguments) | No |

**Return Value:** A vertical array containing all values that appear with the highest frequency.

**Note:** Arguments can be numbers, named ranges, arrays, or cell references. Within references, only numeric values are counted; empty cells, logical values, text, and errors are ignored.

## Examples

### Example 1: Basic Multimodal Data
Find all modes in a dataset with two peaks.
```
=MODE.MULT(1, 1, 2, 2, 3)
```
**Result:** Returns {1; 2} as a vertical array (both appear twice).

### Example 2: Single Mode
When only one mode exists.
```
=MODE.MULT(1, 2, 2, 2, 3, 3)
```
**Result:** Returns {2} (appears three times, more than any other).

### Example 3: Comparing with MODE.SNGL
Demonstrate the difference in output.
```
Data: 5, 5, 10, 10, 15
=MODE.SNGL(A1:A5) → 5 (first mode only)
=MODE.MULT(A1:A5) → {5; 10} (both modes)
```
**Result:** MODE.MULT captures all equally frequent values.

### Example 4: No Mode Exists
When all values are unique.
```
=MODE.MULT(1, 2, 3, 4, 5)
```
**Result:** #N/A - no value appears more than once.

### Example 5: Survey Response Peaks
Find all equally popular ratings.
```
=MODE.MULT(SurveyResponses!Rating)
```
**Result:** Array of all rating values that are equally most common.

### Example 6: Using ROWS to Count Modes
Determine how many modes exist.
```
=ROWS(MODE.MULT(A1:A100))
```
**Result:** Returns the count of modes (e.g., 2 for bimodal data).

### Example 7: Peak Hours Identification
Find all hours with maximum activity.
```
=MODE.MULT(HOUR(ActivityLog!Timestamp))
```
**Result:** Array of hours that are equally busy (e.g., {12; 18} for lunch and dinner peaks).

### Example 8: Error Handling
Handle the #N/A error when no mode exists.
```
=IFERROR(MODE.MULT(A1:A10), "No repeated values")
```
**Result:** Returns modes if they exist, otherwise displays message.

### Example 9: Named Range with Multiple Peaks
Analyze data with known multimodal pattern.
```
=MODE.MULT(ProductSizes)
```
**Result:** All size codes that are equally most common.

### Example 10: Combining with INDEX
Extract specific mode from the array.
```
=INDEX(MODE.MULT(A1:A100), 1)  → First mode
=INDEX(MODE.MULT(A1:A100), 2)  → Second mode (if exists)
```
**Result:** Access individual modes from the array.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #N/A | No value appears more than once | Use IFERROR to handle datasets without repeated values |
| #VALUE! | Non-numeric text directly in arguments | Use numeric values or cell references |
| #NAME? | Function not recognized (Excel 2007 or earlier) | Upgrade to Excel 2010+ for MODE.MULT support |
| Single value returned | Data is unimodal | This is correct behavior for single-mode data |
| #SPILL! | Spill range blocked by existing data | Clear cells where array needs to spill |
| Results not showing | Legacy array formula needed | Use Ctrl+Shift+Enter in older Excel versions |

## Use Cases

### 1. Bimodal Distribution Analysis
**Scenario:** A clothing retailer notices that shirt sales don't follow a simple pattern and suspects two distinct customer groups with different size preferences.

**Implementation:** Use MODE.MULT to identify if multiple sizes share peak popularity.

**Formula:**
```
=MODE.MULT(SalesData!SizeCode)
```

**Analysis:** If MODE.MULT returns {2; 4} (Small and Large), this confirms a bimodal distribution suggesting two customer segments. The retailer should stock both sizes heavily rather than focusing on a medium size that appears less frequently. This insight would be missed using MODE.SNGL.

### 2. Customer Service Multi-Peak Staffing
**Scenario:** A call center manager suspects both lunch hours and end-of-workday create peak demand but needs data to confirm before adjusting schedules.

**Implementation:** Find all hours that share maximum call volume.

**Formula:**
```
=MODE.MULT(HOUR(CallLog!ArrivalTime))
```

**Analysis:** If the result is {12; 17}, both noon and 5 PM are equally busy peaks. The staffing model should account for two peaks rather than assuming a single busy period. MODE.SNGL would only identify the first peak, leading to understaffing during the second.

### 3. Product Feature Preference Analysis
**Scenario:** A product team analyzes feature request votes and wants to identify all features that are equally most requested, not just the first one.

**Implementation:** Calculate all modes from feature ID votes.

**Formula:**
```
=MODE.MULT(FeatureRequests!FeatureID)
```

**Analysis:** If three features tie for most votes, MODE.MULT returns all three, ensuring no popular feature is overlooked. This informs prioritization discussions where all top-requested features deserve consideration rather than arbitrarily selecting just one.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2010 and later | Fully supported |
| Dynamic array support | Excel 365/2019 (spills automatically) | Native (always spills) |
| Legacy array formula | Ctrl+Shift+Enter in Excel 2007-2016 | Not needed |
| Maximum arguments | 255 | 255 |
| Return type | Vertical array | Vertical array |
| #N/A for no mode | Yes | Yes |
| Spill error (#SPILL!) | Excel 365 only | N/A (different behavior) |

**Note:** In older Excel versions, you must select multiple cells and enter MODE.MULT as an array formula using Ctrl+Shift+Enter.

## Tips and Best Practices

1. **Allocate space for results:** In Excel 365, ensure cells below the formula are empty for spill. In older versions, select enough cells to capture all potential modes before entering the formula.

2. **Check for multimodality first:** Use ROWS(MODE.MULT(range)) to count modes. If it returns 1, data is unimodal and MODE.SNGL would suffice.

3. **Handle #N/A appropriately:** When all values are unique, MODE.MULT returns #N/A. Decide if this should display as an error, a message, or trigger alternative analysis.

4. **Combine with INDEX for specific modes:** Use INDEX(MODE.MULT(range), n) to extract the nth mode when you need a single value rather than the full array.

5. **Consider underlying causes:** Multiple modes often indicate distinct subgroups. Investigate whether data should be segmented (by category, time period, etc.) for more meaningful analysis.

6. **Round continuous data:** Like MODE.SNGL, MODE.MULT works best with discrete data. Round continuous measurements before finding modes to ensure meaningful results.

## Related Functions

| Function | Description |
|----------|-------------|
| [[MODE.SNGL]] | Returns single mode (first encountered if tied) |
| [[MODE]] | Legacy function identical to MODE.SNGL |
| [[MEDIAN]] | Returns middle value; single result regardless of distribution |
| [[AVERAGE]] | Returns arithmetic mean; single result |
| [[FREQUENCY]] | Creates complete frequency distribution |
| [[COUNTIF]] | Counts occurrences of specific values |

## Official Documentation

- [Microsoft Excel MODE.MULT function](https://support.microsoft.com/en-us/office/mode-mult-function-50fd9464-b2ba-4191-b57a-39446689ae8c)
- [Google Sheets MODE function](https://support.google.com/docs/answer/3094029)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2010 | MODE.MULT introduced as array function |
| Excel 2013 | Performance improvements |
| Excel 2016 | Improved array formula handling |
| Excel 2019/365 | Dynamic array support with spill behavior |
| Google Sheets | Supported with native array behavior |

---

## Understanding Multimodal Distributions

**Unimodal (one mode):**
- MODE.MULT returns single value
- Example: Normal distribution centered on one value

**Bimodal (two modes):**
- MODE.MULT returns two values
- Common causes:
  - Mixed populations (male/female heights)
  - Different operating modes (weekday/weekend)
  - Two distinct customer segments

**Multimodal (multiple modes):**
- MODE.MULT returns all modes
- May indicate:
  - Complex system with multiple states
  - Data from different sources mixed together
  - Natural clustering in the phenomenon

**No mode:**
- MODE.MULT returns #N/A
- All values unique or uniformly distributed
- Common with continuous data not rounded
