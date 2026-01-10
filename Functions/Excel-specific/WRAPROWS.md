---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- dynamic-arrays
- array-reshaping
subTopics:
- array-transformation
- row-wrapping
- reshape
- layout-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Wrap Rows
- Row Wrap
- Array to Rows
- Reshape to Rows
tags:
- wraprows
- dynamic-arrays
- reshaping
- transformation
- excel-365
---

# WRAPROWS

## Description

**WRAPROWS** transforms a one-dimensional array into a two-dimensional array by wrapping values into rows of a specified length. It reads values from a single row or column and outputs them row by row, filling across each row before moving to the next. A 12-element list with wrap_count of 4 becomes a 3-row by 4-column array.

This function is essential for reshaping data layouts. Convert a single-column list into a multi-row grid, transform sequential data into tabular format, or restructure data for specific report layouts. The wrap direction fills horizontally within each row, then moves down to the next row.

**WRAPROWS in Google Sheets:** Google Sheets added WRAPROWS in 2023, providing the same functionality as Excel. The syntax and behavior are identical. Formulas are portable between platforms.

**Complement to WRAPCOLS:** WRAPROWS fills rows first (left to right, then down); WRAPCOLS fills columns first (top to bottom, then right). Choose based on your desired reading order in the output.

## Syntax

> [!f(x)] WRAPROWS Syntax
>
> ```
> =WRAPROWS(vector, wrap_count, [pad_with])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `vector` | Yes | A single row or column array to wrap. Multi-dimensional arrays are not supported. |
| `wrap_count` | Yes | Number of values per row (row width). Must be positive integer. |
| `pad_with` | No | Value to fill incomplete rows. Default is #N/A. |

### Return Value

Returns a 2D array with wrap_count columns and enough rows to contain all values.

## Examples

> [!f(x)] WRAPROWS Examples

### Example 1: Basic Row Wrap
```
=WRAPROWS(A1:A12, 4)
```
**Result:** 3x4 array (3 rows, 4 columns)

**Explanation:** 12 values wrapped into rows of 4. Natural left-to-right reading order.

---

### Example 2: Wrap with Padding
```
=WRAPROWS(A1:A10, 4, "-")
```
**Result:** 3x4 array, last row padded with "-"

**Explanation:** 10 values don't complete third row. Last two cells get "-" padding.

---

### Example 3: Convert Column to Rows
```
=WRAPROWS(A1:A100, 10)
```
**Result:** 10x10 grid from column data

**Explanation:** Reshape column data into 10-item rows.

---

### Example 4: Matrix Creation
```
=WRAPROWS(SEQUENCE(25), 5)
```
**Result:** 5x5 matrix numbered 1-25

**Explanation:** Generate sequence, wrap into square matrix.

---

### Example 5: Data Cards Layout
```
=WRAPROWS(CustomerNames, 3, "")
```
**Result:** Names in 3-column rows

**Explanation:** Display names in card-like 3-column layout.

---

### Example 6: Weekly Schedule
```
=WRAPROWS(AllTimeSlots, 7, "")
```
**Result:** 7-day rows for weekly view

**Explanation:** Time slots organized by week.

---

### Example 7: Quiz Answer Grid
```
=WRAPROWS(Answers, 5, "N/A")
```
**Result:** 5 answers per row

**Explanation:** Test answers in consistent row format.

---

### Example 8: Batch Display
```
=WRAPROWS(OrderNumbers, 10)
```
**Result:** Orders in 10-item rows

**Explanation:** Display batches of 10 orders per row.

---

### Example 9: Combined with SORT
```
=WRAPROWS(SORT(UNIQUE(A:A)), 5, "")
```
**Result:** Sorted unique values in 5-column rows

**Explanation:** Sort and dedupe, then wrap for display.

---

### Example 10: Tile Layout
```
=WRAPROWS(ImageIDs, 4, "placeholder")
```
**Result:** 4 images per row grid

**Explanation:** Gallery layout with placeholder for incomplete rows.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-vector input (2D array) | Input must be single row or column |
| `#VALUE!` | wrap_count is 0 or negative | Must be positive integer |
| `#SPILL!` | No room for output | Clear target cells |

## Use Cases

### [[Grid Display Generation]]

**Scenario:** Display items in a multi-column grid format.

**Implementation:**
```
=WRAPROWS(ItemList, 4, "")
```

**Business Application:** Product grids, image galleries, card layouts.

---

### [[Form Data Restructuring]]

**Scenario:** Convert linear form responses into structured rows.

**Implementation:**
```
=WRAPROWS(FormResponses, FieldCount)
```

**Business Application:** Parse form data where each N values form one record.

---

### [[Print Layout]]

**Scenario:** Arrange labels or cards for printing.

**Implementation:**
```
=WRAPROWS(LabelData, 3)
```

**Business Application:** 3-across label sheets, business cards.

## Platform Differences

Both Excel and Google Sheets support WRAPROWS identically since 2022/2023.

## Tips and Best Practices

1. **Understand fill order:** Values fill across rows, then down to next row.

2. **Natural reading order:** WRAPROWS follows typical left-to-right, top-to-bottom reading.

3. **Calculate wrap_count:** Total elements / wrap_count = number of rows (rounded up).

4. **Use padding for consistency:** Ensures complete grid even with uneven data.

## Related Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WRAPCOLS]] | Wrap into columns first | When vertical reading order preferred |
| [[TOCOL]] | Flatten to single column | When you need linear input first |
| [[TOROW]] | Flatten to single row | For horizontal input |

## Official Documentation

- **Microsoft Excel:** [WRAPROWS function](https://support.microsoft.com/en-us/office/wraprows-function-796825f3-975a-4f3e-a96b-294707450ac3)
- **Google Sheets:** [WRAPROWS function](https://support.google.com/docs/answer/13190071)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | 2022 | Part of array reshaping functions |
| Google Sheets | 2023 | Added to match Excel |

---

*Last updated: 2026-01-10*
