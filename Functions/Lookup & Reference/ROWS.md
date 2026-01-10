---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- range-dimensions
- array-functions
- counting-functions
- dynamic-ranges
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Row Count
- Count Rows
- Number of Rows
- Range Height
tags:
- lookup
- reference
- rows
- count
- array
- dimensions
---

# ROWS

## Description

**ROWS** returns the number of rows in a reference or array. Give it a range like `=ROWS(A1:A10)` and it returns 10. Give it `=ROWS(B3:D15)` and it returns 13 (rows 3 through 15). This function answers a simple but essential question: "How tall is this range?" Whether you're validating data dimensions, building dynamic formulas, or working with array functions that need to know range sizes, ROWS provides that crucial dimension information.

Unlike ROW (which returns the row *number*), ROWS returns the *count* of rows. `=ROW(A5)` returns 5 (the row number), but `=ROWS(A1:A5)` returns 5 (the number of rows in the range). This distinction matters: ROW tells you position, ROWS tells you size. When building formulas that need to know how many items are in a vertical list—for SEQUENCE, for array sizing, for loop-like patterns—ROWS is your tool.

**ROWS works with arrays, not just ranges.** `=ROWS({1;2;3;4;5})` returns 5 because the array constant has 5 rows (semicolons create rows). This is powerful when working with dynamic arrays in Excel 365 or when you need to measure the output size of another formula. You can wrap any formula that returns an array with ROWS to know its vertical dimension.

**Practical applications abound:** Use ROWS(table) to find how many data rows exist; use ROWS(A:A) to get the total rows in a column (1,048,576 in Excel, varies in Sheets); combine ROWS with ROW for relative positioning calculations; pass ROWS to INDEX or SEQUENCE to create correctly-sized outputs. Any time your formula needs to know "how many rows are we dealing with?", ROWS provides the answer.

## Syntax

> [!f(x)] ROWS Syntax
>
> ```
> =ROWS(array)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `array` | Yes | A reference, range, or array for which you want the number of rows. Can be a cell range (A1:A10), named range, table reference, or array constant. |

### Return Value

Returns a positive integer representing the number of rows in the reference or array. For a single cell, returns 1. For a full column reference (A:A), returns the total number of rows in the worksheet.

## Examples

> [!f(x)] ROWS Examples

### Example 1: Basic Row Count of a Range
```
=ROWS(A1:A10)
```
**Result:** 10

**Explanation:** Counts the rows from A1 to A10 inclusive. The range spans rows 1 through 10, which is 10 rows total. Simple but fundamental—this is the most common usage.

---

### Example 2: Row Count of a Multi-Column Range
```
=ROWS(B3:F15)
```
**Result:** 13

**Explanation:** Returns 13 (rows 3 through 15). The number of columns (B through F = 5 columns) doesn't matter; ROWS only counts the row dimension. Use COLUMNS for the horizontal dimension.

---

### Example 3: Row Count of a Single Cell
```
=ROWS(D7)
```
**Result:** 1

**Explanation:** A single cell is a 1x1 range, so ROWS returns 1. This might seem trivial but is useful in formulas that need to handle both single cells and multi-cell ranges consistently.

---

### Example 4: Row Count of a Full Column
```
=ROWS(A:A)
```
**Result:** 1048576 (in Excel), varies in Sheets

**Explanation:** Returns the total number of rows in the worksheet for that column. In Excel 2007 and later, this is 1,048,576. In older Excel (pre-2007), it was 65,536. Google Sheets varies by subscription.

---

### Example 5: Row Count of an Array Constant
```
=ROWS({1;2;3;4;5})
```
**Result:** 5

**Explanation:** Array constants use semicolons to separate rows. This array has 5 rows (each containing one value). This works for measuring any array, including formula outputs in Excel 365.

---

### Example 6: Row Count of a Named Range
```
=ROWS(SalesData)
```
**Result:** Number of rows in the named range SalesData

**Explanation:** Named ranges work just like cell references. If SalesData refers to $A$2:$D$100, ROWS returns 99. Useful for documenting and validating that named ranges have expected dimensions.

---

### Example 7: Row Count of an Excel Table
```
=ROWS(Table1)
```
**Result:** Number of data rows in Table1 (excludes header and total rows)

**Explanation:** When referencing an Excel Table, ROWS returns only the data rows—not the header row or totals row (if present). This is the body/data portion of the table.

---

### Example 8: ROWS with SEQUENCE for Dynamic Sizing
```
=SEQUENCE(ROWS(A2:A100))
```
**Result:** Array {1;2;3;...;99}

**Explanation:** Creates a sequence with as many numbers as rows in the reference. If your data range grows or shrinks, this automatically adjusts. Essential for dynamic array formulas that need to match source data dimensions.

---

### Example 9: ROWS for Relative Row Positioning
```
=ROWS($A$1:A1)
```
**Result:** 1 in row 1, 2 in row 2, 3 in row 3... when dragged down

**Explanation:** A clever pattern for sequential numbering. The first reference is locked ($A$1), but the second (A1) moves as you drag down. In row 3, this becomes ROWS($A$1:A3) = 3. An alternative to ROW()-based numbering.

---

### Example 10: Validating Array Dimensions
```
=IF(ROWS(LookupTable)>=100, "Table OK", "Table too small")
```
**Result:** Validates that a reference contains at least 100 rows

**Explanation:** Use ROWS for data validation—ensuring reference tables have expected minimum sizes before performing lookups. Catches issues where named ranges accidentally point to truncated data.

---

### Example 11: ROWS with INDEX for Last Row Value
```
=INDEX(A:A, ROWS(A:A))
```
**Result:** Value in the last possible row of column A (usually empty)

**Explanation:** While this technically works, it returns the 1,048,576th row (likely empty). More practical: `=INDEX(A:A, COUNTA(A:A))` gets the last filled row. But ROWS demonstrates how to reference the dimensional limits of a range.

---

### Example 12: Dynamic Range Reference with ROWS
```
=SUM(OFFSET(A1, 0, 0, ROWS(DataRange), 1))
```
**Result:** Sums a range with the same row count as DataRange

**Explanation:** OFFSET creates a dynamic range, and ROWS(DataRange) sets its height. If DataRange is $B$2:$B$50, this sums 49 rows starting from A1. Useful for creating parallel ranges that match an existing range's dimensions.

---

### Example 13: ROWS in Array Formula for Position Calculation
```
=SUMPRODUCT((ROW(A2:A100)-ROW(A2)+1)/ROWS(A2:A100)*B2:B100)
```
**Result:** Weighted sum where position within range affects weight

**Explanation:** This calculates weights based on relative position. ROW()-ROW(start)+1 gives 1,2,3..., and dividing by ROWS gives 1/99, 2/99, 3/99... Useful for declining balance or position-weighted calculations.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Argument is not a valid reference or array | Ensure you're passing a range reference (A1:A10), named range, or array constant—not plain text or a single number. |
| `#REF!` | Reference points to a deleted range | Check that referenced ranges still exist. Update any broken references. |
| `#NAME?` | Named range doesn't exist or function misspelled | Verify named ranges exist in Name Manager. Check spelling—ROWS not ROW. |
| `Unexpected result` | Confused ROWS with ROW | ROWS counts rows in a range; ROW returns the row number of a cell. Verify you're using the correct function. |
| `Large number` | Used full column reference (A:A) unexpectedly | Full column references return the worksheet's total row count. Use a bounded range if you want just your data. |

## Use Cases

### [[Data Validation and Range Verification]]

**Scenario:** Before performing lookups or calculations, verify that reference ranges have expected dimensions—catching issues like truncated data imports or incorrectly defined named ranges.

**Implementation:**
```
Check minimum size:
=IF(ROWS(LookupTable)>=1000, VLOOKUP(...), "Error: Table too small")

Verify exact dimensions:
=IF(AND(ROWS(Matrix)=10, COLUMNS(Matrix)=10), "Valid 10x10", "Invalid dimensions")

Compare range sizes:
=IF(ROWS(SourceData)=ROWS(TargetData), "Aligned", "Row count mismatch: "&ROWS(SourceData)&" vs "&ROWS(TargetData))
```

**Business Application:** Financial models requiring specific data dimensions, data imports that should match expected sizes, and any automated process where incorrect range sizes would cause errors or incorrect results.

**Technical Details:** Place validation formulas prominently (perhaps with conditional formatting) to catch issues early. In complex workbooks, a "Data Validation" sheet can centralize all dimension checks using ROWS and COLUMNS.

---

### [[Dynamic Array Sizing]]

**Scenario:** Create formulas whose output dimensions automatically match the source data—ensuring that results arrays, helper columns, and derived calculations stay synchronized with changing data volumes.

**Implementation:**
```
Create index numbers matching data:
=SEQUENCE(ROWS(DataTable))

Generate matching random numbers:
=RANDARRAY(ROWS(DataTable), 1)

Create a formula that spans the same rows:
=IF(SEQUENCE(ROWS(SourceData))<=COUNTIF(SourceData, ">0"), "Include", "Exclude")
```

**Business Application:** Automated reporting where data volumes change monthly, dynamic dashboards that adapt to varying dataset sizes, and any scenario where manual range adjustments would be error-prone.

**Technical Details:** In Excel 365, dynamic arrays automatically spill. Use ROWS to size your SEQUENCE, RANDARRAY, or other array functions to match existing data. This eliminates the need to update formulas when data grows or shrinks.

---

### [[Building Relative Position Formulas]]

**Scenario:** Create sequential numbering or position-based calculations that work regardless of where the data starts and automatically extend as you copy the formula down.

**Implementation:**
```
Auto-incrementing row numbers (alternative to ROW()):
=ROWS($A$1:A1)

Percentage through list:
=ROWS($A$1:A1)/ROWS($A$1:$A$100)

Reverse numbering (countdown):
=ROWS(DataRange)-ROWS($A$1:A1)+1
```

**Business Application:** Invoice line numbering, queue position tracking, progress indicators, or any list requiring automatic sequential identifiers that don't break when rows are inserted or filtered.

**Technical Details:** The `ROWS($Fixed:$Expanding)` pattern is powerful. The first reference stays fixed ($A$1), while the second expands as you drag (A1 becomes A2, A3...). This creates 1, 2, 3... without using ROW().

---

### [[Array Formula Dimension Control]]

**Scenario:** Control the output size of array formulas, ensuring they produce exactly the right number of results—especially important when array results feed into other calculations.

**Implementation:**
```
Limit array output to match source:
=TAKE(LargeArrayFormula, ROWS(SourceData))

Expand or pad array to specific size:
=IF(SEQUENCE(ROWS(TargetRange))<=ROWS(SourceArray), INDEX(SourceArray, SEQUENCE(ROWS(TargetRange))), "")

Match output to input dimensions:
=MAKEARRAY(ROWS(InputRange), COLUMNS(InputRange), LAMBDA(r,c, CustomCalculation))
```

**Business Application:** Report generation requiring specific row counts, data transformation pipelines where intermediate arrays must match dimensions, and template sheets where output areas are pre-defined.

**Technical Details:** Modern Excel's TAKE, DROP, and MAKEARRAY functions work excellently with ROWS for dimension control. In older versions, use INDEX with ROWS to limit array sizes, or wrap in IF to pad shorter arrays.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions from Excel 5.0 (1993) onward
- **With Excel Tables:** Returns data row count (excludes headers and totals)
- **Full column reference:** Returns 1,048,576 for A:A (Excel 2007+)
- **With dynamic arrays:** Works with spilled array references (Excel 365)
- **Performance:** Extremely fast; no performance concerns

### Google Sheets

- **Availability:** All versions
- **Full column reference:** Returns sheet row limit (varies by account type)
- **With ARRAYFORMULA:** Works within ARRAYFORMULA context
- **Syntax:** Identical to Excel
- **Performance:** Highly optimized

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum row count (full column) | 1,048,576 | Varies (up to 10,000,000) |
| Structured table references | Full support | Limited (no Tables) |
| Dynamic array integration | ROWS(spilled_range#) | ROWS(array_formula_result) |
| Named range support | Full | Full |

## Tips and Best Practices

1. **Use ROWS for dimension validation:** Before complex calculations, verify `ROWS(DataRange)>=ExpectedMinimum` to catch truncated or missing data early.

2. **ROWS vs ROW—know the difference:** ROWS(A1:A10) = 10 (count), ROW(A10) = 10 (position). They can return the same number coincidentally, but they mean different things.

3. **Use ROWS($A$1:A1) for auto-incrementing:** This pattern creates 1, 2, 3... as you drag down, providing an alternative to ROW()-based numbering that works regardless of starting row.

4. **Combine with COLUMNS for complete dimensions:** ROWS gives height, COLUMNS gives width. Together they fully describe a range's shape: `ROWS(R) & "x" & COLUMNS(R)` returns "10x5" for a 10-row, 5-column range.

5. **Size dynamic arrays correctly:** Use `SEQUENCE(ROWS(DataRange))` to create arrays that automatically match your data's row count.

6. **Watch out for full column references:** ROWS(A:A) returns the worksheet maximum, not your data count. Use ROWS(A1:A1000) or ROWS(NamedRange) for bounded counts.

7. **In Excel Tables, ROWS excludes headers:** `ROWS(Table1)` counts only data rows. For total rows including headers, use `ROWS(Table1[#All])`.

8. **Validate array formula outputs:** Wrap array formulas in `ROWS(formula)` to confirm they're producing the expected number of results before using those results elsewhere.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ROW]] | Returns the row number of a cell | When you need position, not count |
| [[COLUMNS]] | Returns the number of columns in a range | When you need horizontal dimension instead of vertical |
| [[COUNTA]] | Counts non-empty cells | When you need count of filled cells, not total rows in range |
| [[COUNT]] | Counts numeric cells | When you need count of numbers, not total rows |

### Commonly Used Together

**[[COLUMNS]]** - Count columns in a range

*Combined with ROWS for complete dimensions:*
```
="Range is "&ROWS(A1:D10)&" rows by "&COLUMNS(A1:D10)&" columns"
```
Together they describe the full shape of any rectangular range.

---

**[[SEQUENCE]]** - Generate array of sequential numbers

*Combined with ROWS for dynamic array sizing:*
```
=SEQUENCE(ROWS(DataTable))
```
Creates a numbered sequence matching your data's row count.

---

**[[INDEX]]** - Return value at position

*Combined with ROWS for last-row access:*
```
=INDEX(DataRange, ROWS(DataRange), 1)
```
Gets the last row's value by using ROWS to find the row count.

---

**[[OFFSET]]** - Create dynamic range reference

*Combined with ROWS for dynamic range sizing:*
```
=SUM(OFFSET(A1, 0, 0, ROWS(DataRange), 1))
```
Creates a range with height matching another range.

---

**[[COUNTA]]** - Count non-empty cells

*Alternative to ROWS for actual data counting:*
```
=COUNTA(A:A) vs =ROWS(A1:A100)
```
COUNTA counts filled cells; ROWS counts total cells in range. Use COUNTA for data rows, ROWS for range dimensions.

## Official Documentation

- **Microsoft Excel:** [ROWS function](https://support.microsoft.com/en-us/office/rows-function-b592593e-3fc2-47f2-bec1-bda493811597)
- **Google Sheets:** [ROWS function](https://support.google.com/docs/answer/3093306)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 5.0 (1993) | Original implementation |
| Excel 2007 | Enhanced | Returns 1,048,576 for full column references |
| Excel 365 | Dynamic Arrays | Works with spilled range references |
| Google Sheets | Original launch (2006) | Full compatibility with Excel behavior |

---

*Last updated: 2026-01-10*
