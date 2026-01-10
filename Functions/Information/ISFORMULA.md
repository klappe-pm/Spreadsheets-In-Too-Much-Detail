---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - formula-detection
  - cell-analysis
  - auditing
  - data-validation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-formula
  - formula-check
  - detect-formula
tags:
  - information
  - is-functions
  - formula-detection
  - auditing
  - cell-analysis
  - excel
  - google-sheets
---

# ISFORMULA

## Description

ISFORMULA is an information function that tests whether a cell reference contains a formula, returning TRUE if the cell contains a formula and FALSE if it contains a constant value, is empty, or contains an error value that was entered directly. This function is essential for spreadsheet auditing, allowing you to quickly identify which cells contain calculated values versus static data, which is crucial for understanding data flow and dependencies within complex workbooks.

The function accepts a single reference argument pointing to the cell you want to test. Unlike most IS functions that can accept values directly, ISFORMULA specifically requires a cell reference because it's testing a property of the cell itself (whether it contains a formula) rather than a property of the value. Passing a literal value or expression directly will result in an error because there's no cell to examine for formula presence.

One important distinction to understand is that ISFORMULA returns TRUE based on whether the cell contains a formula, not whether the formula produces a meaningful result. A cell containing `=5` (a formula that simply returns the number 5) will return TRUE, even though the result is identical to entering the number 5 directly. Similarly, cells containing formulas that result in errors still return TRUE because they contain formulas. This behavior makes ISFORMULA particularly useful for auditing spreadsheet structure rather than just analyzing values.

Both Excel and Google Sheets support ISFORMULA with nearly identical behavior. The function was added to Excel in version 2013, making it unavailable in earlier versions. When auditing spreadsheets, ISFORMULA is often combined with conditional formatting to highlight formula cells, or used in validation routines to ensure that certain cells contain formulas rather than manually entered values that might not update when source data changes.

## Syntax

> [!f(x)]
> `ISFORMULA(reference)`

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| reference | Yes | A reference to the cell you want to test for the presence of a formula. Must be a valid cell reference, not a value or expression. |

## Examples

### Example 1: Basic Formula Detection

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=B1+C1` | `=ISFORMULA(A1)` | TRUE | Cell contains a formula |
| A2 | `100` | `=ISFORMULA(A2)` | FALSE | Cell contains a static value |

This example demonstrates the fundamental behavior of ISFORMULA. Cell A1 contains a formula that adds two cells, so ISFORMULA returns TRUE. Cell A2 contains a directly entered number, so ISFORMULA returns FALSE.

### Example 2: Empty Cell Detection

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | *(empty)* | `=ISFORMULA(A1)` | FALSE | Empty cells are not formulas |
| A2 | `=""` | `=ISFORMULA(A2)` | TRUE | Formula returning empty string |

Empty cells return FALSE because they contain no formula. However, a cell with a formula that returns an empty string (like `=""`) returns TRUE because it contains a formula, even though it appears empty.

### Example 3: Text Versus Text Formula

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `Hello` | `=ISFORMULA(A1)` | FALSE | Directly entered text |
| A2 | `="Hello"` | `=ISFORMULA(A2)` | TRUE | Formula returning text |
| A3 | `=CONCATENATE("Hel","lo")` | `=ISFORMULA(A3)` | TRUE | Formula building text |

These examples show that the function detects formulas regardless of whether the result looks like static text. Both A2 and A3 contain formulas, so both return TRUE, even though A1 and A2 display identical values.

### Example 4: Error Values

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=1/0` | `=ISFORMULA(A1)` | TRUE | Formula producing #DIV/0! |
| A2 | `=VLOOKUP(99,A:A,1,0)` | `=ISFORMULA(A2)` | TRUE | Formula producing #N/A |
| A3 | *(typed #N/A)* | `=ISFORMULA(A3)` | FALSE | Manually entered error text |

Cells containing formulas that produce errors still return TRUE because they contain formulas. However, if you somehow enter error text directly (which Excel typically prevents), it would return FALSE.

### Example 5: Simple Value Formulas

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=5` | `=ISFORMULA(A1)` | TRUE | Formula returning a number |
| A2 | `5` | `=ISFORMULA(A2)` | FALSE | Directly entered number |
| A3 | `=TRUE` | `=ISFORMULA(A3)` | TRUE | Formula returning boolean |
| A4 | `TRUE` | `=ISFORMULA(A4)` | FALSE | Directly entered boolean |

Even formulas that simply return a constant value (like `=5`) are detected as formulas. The function tests for formula presence, not formula complexity.

### Example 6: Cell Reference Only

| Formula | Result | Notes |
|---------|--------|-------|
| `=ISFORMULA(100)` | #VALUE! | Cannot test a literal value |
| `=ISFORMULA("Hello")` | #VALUE! | Cannot test a literal string |
| `=ISFORMULA(A1+B1)` | #VALUE! | Cannot test an expression |
| `=ISFORMULA(A1)` | TRUE/FALSE | Correct: tests a cell reference |

ISFORMULA requires a cell reference as its argument. Passing literal values or expressions results in a #VALUE! error because there's no cell to examine for formula presence.

### Example 7: Range Reference Behavior

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| A1 | `=SUM(B:B)` | `=ISFORMULA(A1:A3)` | TRUE | Returns result for first cell only |
| A2 | `100` | | | |
| A3 | `=A1*2` | | | |

When given a range reference, ISFORMULA returns the result for only the first cell in the range (top-left corner). To test multiple cells, you need to use array formulas or apply ISFORMULA to each cell individually.

### Example 8: Conditional Formatting Setup

| Cell | Purpose | Formula | Result | Notes |
|------|---------|---------|--------|-------|
| A1:D10 | Data range | `=ISFORMULA(A1)` | *(varies)* | Use in conditional formatting |

To highlight all formula cells in a range, apply conditional formatting with the formula `=ISFORMULA(A1)` (referencing the top-left cell of the selection). This will highlight every cell in the range that contains a formula, making it easy to visually audit your spreadsheet.

### Example 9: Counting Formula Cells

| Data Range | Formula | Result | Notes |
|------------|---------|--------|-------|
| A1:A10 (mixed) | `=SUMPRODUCT(--ISFORMULA(A1:A10))` | 6 | Counts formula cells |
| A1:A10 (mixed) | `=SUMPRODUCT(--(NOT(ISFORMULA(A1:A10))))` | 4 | Counts non-formula cells |

Using SUMPRODUCT with ISFORMULA allows you to count how many cells in a range contain formulas. The double negative (`--`) converts TRUE/FALSE to 1/0 for counting.

### Example 10: Validation for Formula Requirement

| Cell | Contents | Formula | Result | Notes |
|------|----------|---------|--------|-------|
| B2 | `=A1*1.1` | `=IF(ISFORMULA(B2),"OK","Enter formula!")` | OK | Cell has formula |
| B3 | `110` | `=IF(ISFORMULA(B3),"OK","Enter formula!")` | Enter formula! | Static value warning |

This validation pattern ensures that certain cells contain formulas rather than hard-coded values. This is useful for maintaining spreadsheet integrity where calculations should update automatically with source data.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Passing a literal value instead of cell reference | Always use a cell reference (e.g., `=ISFORMULA(A1)`) |
| #VALUE! | Passing an expression instead of reference | Use only cell references, not calculated expressions |
| #REF! | Reference to deleted cells | Update the reference to valid cells |
| Unexpected FALSE | Testing a cell that looks like it has a formula | Verify the cell actually contains a formula, not just a value that looks calculated |
| Only first cell tested | Passing a range reference | Use SUMPRODUCT or array formula to test multiple cells |

## Use Cases

### Spreadsheet Auditing and Documentation

**Scenario**: You're auditing a complex financial model and need to identify which cells contain formulas versus manually entered assumptions.

**Solution**: Use ISFORMULA with conditional formatting to create a visual map of formula cells.

```excel
// Apply conditional formatting to range A1:Z100
// Conditional format formula: =ISFORMULA(A1)
// Format: Light blue background for formula cells

// Create a summary of formula distribution
=SUMPRODUCT(--ISFORMULA(A1:Z100))  // Total formula cells
=ROWS(A1:Z100)*COLUMNS(A1:Z100)-SUMPRODUCT(--ISFORMULA(A1:Z100))  // Static cells
```

**Why this works**: ISFORMULA returns TRUE for every cell containing a formula, allowing conditional formatting to highlight them. The SUMPRODUCT formula counts all formula cells across the range, giving you metrics for documentation.

### Data Integrity Validation

**Scenario**: You have a financial model where certain cells must contain formulas (calculated values) and others must contain static inputs. You want to validate that users haven't accidentally overwritten formulas with hard-coded values.

**Solution**: Create a validation system that checks formula presence in critical cells.

```excel
// In a validation column, check each critical cell
=IF(ISFORMULA(D5),"OK","WARNING: Formula missing in D5")

// For a range of cells that should all be formulas
=IF(SUMPRODUCT(--ISFORMULA(D5:D20))=ROWS(D5:D20),"All formulas intact","Some formulas overwritten")

// Count specific issues
="Formula cells: "&SUMPRODUCT(--ISFORMULA(D5:D20))&" of "&ROWS(D5:D20)&" expected"
```

**Why this works**: By checking whether critical calculation cells still contain formulas, you can detect when users have accidentally replaced a formula with a static value, which would break the model's automatic updating.

### Dynamic Documentation Generator

**Scenario**: You want to create automatic documentation that describes whether each cell in a summary table is calculated or an input value.

**Solution**: Build a documentation helper that classifies each cell.

```excel
// Create a cell type classifier
=IF(ISBLANK(A1),"Empty",IF(ISFORMULA(A1),"Calculated","Input"))

// More detailed classification
=CHOOSE(
    1+ISFORMULA(A1)*1+ISBLANK(A1)*2,
    "Static Value",
    "Formula",
    "Empty",
    "Empty"
)

// Generate a mini data dictionary
=A1&": "&IF(ISFORMULA(B1),"Calculated from formula","Manual input value")
```

**Why this works**: By combining ISFORMULA with ISBLANK, you can categorize every cell as either empty, a static input value, or a calculated formula result. This classification is essential for creating data dictionaries and user documentation for complex spreadsheets.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic syntax | `=ISFORMULA(reference)` | `=ISFORMULA(reference)` |
| Array formula support | Yes (Excel 365 dynamic arrays) | Yes (ARRAYFORMULA) |
| Range behavior | Returns first cell result | Returns first cell result |
| Error on literal value | #VALUE! | #VALUE! |
| Version introduced | Excel 2013 | Always supported |
| Named range support | Yes | Yes |
| Cross-sheet reference | Yes | Yes |

Both platforms implement ISFORMULA identically for standard use cases. The main difference is availability: Excel 2010 and earlier versions do not support ISFORMULA, requiring workarounds using GET.CELL with named ranges or VBA for similar functionality.

## Tips and Best Practices

1. **Audit Before Modifying**: Use ISFORMULA with conditional formatting before making bulk changes to a spreadsheet. This helps you identify and protect formula cells from being accidentally overwritten with static values.

2. **Validate User Inputs**: In shared workbooks where users might accidentally overwrite formulas, create validation cells that use ISFORMULA to check that critical calculation cells still contain formulas.

3. **Document Data Flow**: Combine ISFORMULA with cell addressing functions to create automatic documentation showing which cells are inputs (static) versus outputs (calculated), making your spreadsheet easier to understand.

4. **Use With Array Formulas for Ranges**: When testing multiple cells, wrap ISFORMULA in SUMPRODUCT or use array formula syntax to get counts or check all cells, since ISFORMULA alone only returns the first cell's result for a range.

5. **Remember Reference Requirement**: Unlike most IS functions, ISFORMULA cannot accept values directly. It must receive a cell reference because it's testing a property of the cell, not the value.

6. **Consider Excel Version Compatibility**: If your spreadsheet needs to work in Excel 2010 or earlier, avoid ISFORMULA as it's not available. Use VBA or the GET.CELL macro function with named ranges as alternatives.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|---------------|
| ISBLANK | Tests for empty cells | Tests for emptiness, not formula presence |
| ISNUMBER | Tests for numeric values | Tests value type, not how value was created |
| ISTEXT | Tests for text values | Tests value type, not whether cell has formula |
| ISERROR | Tests for error values | Tests the result, ISFORMULA tests cell property |
| CELL | Returns cell information | Can get format info but not formula detection |
| TYPE | Returns data type number | Returns type of value, not cell property |
| FORMULATEXT | Returns formula as text | Returns the actual formula, not just TRUE/FALSE |

### Commonly Used Together

- **IF + ISFORMULA**: Conditional logic based on formula presence
- **SUMPRODUCT + ISFORMULA**: Count formula cells in a range
- **ISFORMULA + ISBLANK**: Classify cells as formula, value, or empty
- **ISFORMULA + FORMULATEXT**: Check for formula and display it if present
- **CONDITIONAL FORMATTING + ISFORMULA**: Visually highlight formula cells

## Official Documentation

- [Microsoft Excel ISFORMULA Function](https://support.microsoft.com/en-us/office/isformula-function-e4d1355f-7121-4ef2-801e-3839bfd6b1e5)
- [Google Sheets ISFORMULA Function](https://support.google.com/docs/answer/9061539)

## Version History

| Version | Changes |
|---------|---------|
| Excel 2013 | Function introduced |
| Excel 2016 | No changes |
| Excel 2019 | No changes |
| Excel 365 | Dynamic array support for range arguments |
| Google Sheets | Supported since launch |
