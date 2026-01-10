---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
- rounding
subTopics:
- ceiling-functions
- iso-standard
- negative-numbers
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ISO-CEILING
- CEILING-ISO
tags:
- rounding
- ceiling
- iso-standard
- mathematical
---

# ISO.CEILING

## Description

ISO.CEILING rounds a number up to the nearest integer or to the nearest multiple of significance, following the ISO 8601 standard. Unlike standard ceiling functions that may have platform-dependent behavior with negative numbers, ISO.CEILING consistently rounds toward positive infinity regardless of the sign of the number being rounded. This means negative numbers are rounded toward zero, not away from it.

This function is particularly valuable when you need predictable, standardized rounding behavior across different platforms and applications. In financial modeling, time calculations, and resource allocation scenarios, ISO.CEILING ensures that negative values round consistently toward zero (up on the number line), which matches international standards and mathematical conventions.

A critical gotcha is understanding the difference between ISO.CEILING and other ceiling functions. Standard CEILING in some applications rounds negative numbers away from zero (more negative), while ISO.CEILING always rounds toward positive infinity. This distinction matters significantly when processing negative values like losses, debits, or temperature changes. Additionally, if significance is omitted, it defaults to 1 (rounds to nearest integer up).

Both Excel and Google Sheets support ISO.CEILING with identical behavior. However, Google Sheets also offers CEILING.PRECISE as a perfect synonym. In Excel, both ISO.CEILING and CEILING.PRECISE exist and behave identically. The function ignores the sign of the significance argument - only the absolute value matters.

## Syntax

> [!f(x)] ISO.CEILING Syntax
>
> ```excel
> ISO.CEILING(number, [significance])
> ```
>
> Rounds a number up toward positive infinity to the nearest multiple of significance.

### Parameters

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| number | The value to be rounded | Yes | - |
| significance | The multiple to which you want to round | No | 1 |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=ISO.CEILING(4.3)` | 5 | Rounds 4.3 up to the nearest integer |
| `=ISO.CEILING(4.3, 1)` | 5 | Explicit significance of 1 |
| `=ISO.CEILING(6.7, 2)` | 8 | Rounds 6.7 up to nearest multiple of 2 |
| `=ISO.CEILING(4.2, 0.5)` | 4.5 | Rounds 4.2 up to nearest 0.5 |
| `=ISO.CEILING(-4.3)` | -4 | Rounds -4.3 toward positive infinity (toward zero) |
| `=ISO.CEILING(-4.3, 2)` | -4 | Rounds -4.3 up to nearest multiple of 2 toward zero |
| `=ISO.CEILING(-4.3, -2)` | -4 | Sign of significance is ignored |
| `=ISO.CEILING(21.25, 0.1)` | 21.3 | Rounds to nearest 0.1 |
| `=ISO.CEILING(0, 5)` | 0 | Zero remains zero |
| `=ISO.CEILING(-0.5, 1)` | 0 | Rounds -0.5 up to 0 |
| `=ISO.CEILING(1.2, 0.25)` | 1.25 | Rounds to nearest quarter |
| `=ISO.CEILING(-7, 3)` | -6 | Rounds -7 up to -6 (nearest multiple of 3 toward zero) |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #VALUE! | Non-numeric arguments provided | Ensure both number and significance are numeric values |
| #NUM! | Significance is zero | Use a non-zero significance value |
| #DIV/0! | In some versions, zero significance | Provide a valid non-zero significance |

## Use Cases

### [[Financial Calculations - Credit Limits]]
- **Implementation**: Round credit calculations up to standardized amounts
- **Business Application**: Setting minimum payment amounts or credit line increments using consistent rounding
- **Technical Details**: `=ISO.CEILING(BalanceDue * 0.02, 5)` rounds minimum payments to $5 increments; negative balances (credits) round toward zero appropriately

### [[Time Billing - Duration Rounding]]
- **Implementation**: Round billable time up to standard billing increments
- **Business Application**: Law firms and consultants rounding time entries to 6-minute or 15-minute increments
- **Technical Details**: `=ISO.CEILING(TimeWorked * 24 * 60, 6) / 60 / 24` converts time to 6-minute increments; handles time adjustments (negative) consistently

### [[Inventory Management - Order Quantities]]
- **Implementation**: Round order quantities up to package or pallet sizes
- **Business Application**: Ensuring orders meet minimum packaging requirements and shipping efficiency
- **Technical Details**: `=ISO.CEILING(RequiredUnits, PackageSize)` rounds up to full packages; inventory adjustments (negative) round properly toward zero

### [[Resource Allocation - Staff Scheduling]]
- **Implementation**: Round required staff hours up to shift increments
- **Business Application**: Converting workload calculations to practical shift assignments
- **Technical Details**: `=ISO.CEILING(TotalWorkHours, 4)` rounds to 4-hour shift blocks; understaffing adjustments maintain consistent rounding behavior

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| ISO.CEILING function | Yes (2013+) | Yes |
| CEILING.PRECISE synonym | Yes | Yes |
| Significance default | 1 | 1 |
| Negative significance | Sign ignored | Sign ignored |
| Array formula support | Yes (365) | Yes |
| Behavior with negatives | Rounds toward +infinity | Rounds toward +infinity |

## Tips and Best Practices

1. **Use ISO.CEILING for consistent negative number handling** - Unlike CEILING or CEILING.MATH, ISO.CEILING always rounds toward positive infinity regardless of mode settings
2. **Omit significance for integer rounding** - The default of 1 means you don't need to specify it for simple integer ceiling operations
3. **Ignore significance sign** - ISO.CEILING uses absolute value of significance, so `-2` and `2` produce identical results
4. **Consider CEILING.MATH for alternative negative behavior** - If you need negative numbers to round away from zero (more negative), use CEILING.MATH with appropriate mode
5. **Combine with FLOOR functions for range allocation** - Use ISO.CEILING for upper bounds and FLOOR.MATH for lower bounds in bin allocation
6. **Test with edge cases** - Verify behavior with zero, small decimals, and numbers exactly at multiples
7. **Document rounding choice in financial models** - ISO.CEILING's standardized behavior makes it audit-friendly for international applications

## Related Functions

### Similar Functions

| Function | Description | Key Difference |
|----------|-------------|----------------|
| [[CEILING.PRECISE]] | Identical ISO-standard ceiling function | Synonym for ISO.CEILING |
| [[CEILING.MATH]] | Rounds up with mode for negative numbers | Has mode parameter for different negative handling |
| [[CEILING]] | Basic ceiling function | Platform-dependent negative number behavior |
| [[ROUNDUP]] | Rounds away from zero | Always increases magnitude; different from ceiling behavior |
| [[INT]] | Rounds down to integer | Opposite direction; always toward negative infinity |

### Commonly Used Together

- **[[FLOOR.MATH]]** - Creates complementary upper and lower bounds for data binning
- **[[MOD]]** - Determines remainder to check if value is already at a multiple
- **[[ABS]]** - Gets absolute value when working with signed quantities
- **[[SIGN]]** - Determines sign for conditional rounding logic
- **[[ROUND]]** - Provides standard rounding when ceiling behavior isn't needed

## Official Documentation

- [Microsoft: ISO.CEILING function](https://support.microsoft.com/en-us/office/iso-ceiling-function-e587bb73-6cc2-4113-b664-ff5b09859a83)
- [Google: ISO.CEILING function](https://support.google.com/docs/answer/9061514)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2012-10 | Function introduced |
| Excel 365 | Ongoing | Dynamic array support added |
| Google Sheets | 2019 | Full support added |
