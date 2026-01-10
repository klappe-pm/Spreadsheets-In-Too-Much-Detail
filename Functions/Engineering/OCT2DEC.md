---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
subTopics:
- number-base-conversion
- octal-conversion
- decimal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- octal-to-decimal
- oct-to-dec
tags:
- engineering-function
- base-conversion
- octal
- decimal
- number-systems
---

# OCT2DEC

## Description

OCT2DEC converts an octal (base-8) number to its decimal (base-10) equivalent. Octal uses eight digits (0-7), with each position representing a power of 8. This function is essential for interpreting Unix file permissions in human-readable form, working with legacy computing systems, and educational contexts where understanding number base relationships is important.

The conversion from octal to decimal multiplies each digit by its corresponding power of 8 and sums the results. For example, octal 755 = (7 x 8^2) + (5 x 8^1) + (5 x 8^0) = (7 x 64) + (5 x 8) + (5 x 1) = 448 + 40 + 5 = 493 decimal. OCT2DEC handles a 30-bit range, accepting octal values from 4000000000 (-536,870,912 decimal) to 3777777777 (536,870,911 decimal) using two's complement for negative numbers.

Use OCT2DEC when you need to perform arithmetic on Unix permission values, convert legacy system addresses to decimal for documentation, calculate memory offsets from octal addresses, or verify octal conversions. The function returns a numeric value (not text), enabling direct use in calculations.

Both Excel and Google Sheets implement OCT2DEC identically. The function accepts octal strings (digits 0-7 only) up to 10 characters and returns a decimal number. Unlike OCT2BIN which has a limited range, OCT2DEC supports the full 30-bit signed integer range. Two's complement is used for negative numbers, where octal values starting with 4, 5, 6, or 7 in a 10-digit string represent negative values.

## Syntax

> [!NOTE] Syntax
> ```
> OCT2DEC(number)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The octal number to convert. Must contain only digits 0-7. Can be up to 10 characters long. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=OCT2DEC("7")` | 7 | Single digit: same in octal and decimal |
| `=OCT2DEC("10")` | 8 | Octal 10 = 1*8 + 0 = 8 decimal |
| `=OCT2DEC("77")` | 63 | 7*8 + 7 = 63 (largest 2-digit octal) |
| `=OCT2DEC("100")` | 64 | 1*64 = 64 (8^2) |
| `=OCT2DEC("755")` | 493 | Unix permission: 7*64 + 5*8 + 5 = 493 |
| `=OCT2DEC("644")` | 420 | File permission: 6*64 + 4*8 + 4 = 420 |
| `=OCT2DEC("777")` | 511 | Maximum 3-digit octal = 7*64 + 7*8 + 7 |
| `=OCT2DEC("7777777777")` | -1 | Two's complement -1 |
| `=OCT2DEC("4000000000")` | -536870912 | Minimum value (two's complement) |
| `=OCT2DEC("3777777777")` | 536870911 | Maximum positive value |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input contains invalid digits (8, 9) | Use only digits 0-7 for octal |
| `#NUM!` | Input exceeds 10 characters | Limit octal string to 10 characters |
| `#VALUE!` | Non-numeric or empty input | Provide valid octal string |
| `#VALUE!` | Input contains spaces | Remove spaces from octal string |
| `#VALUE!` | Input contains letters | Octal uses only digits 0-7 |

## Use Cases

### 1. Unix Permission Decimal Calculation

**Scenario**: A developer needs to perform calculations on Unix file permissions, requiring conversion from octal chmod values to decimal for mathematical operations.

**Implementation**: Permission octal codes are converted to decimal for comparison and computation.

```
Permission analysis spreadsheet:
| File | Octal | Formula | Decimal | Analysis |
|------|-------|---------|---------|----------|
| script.sh | 755 | =OCT2DEC("755") | 493 | Executable |
| config.txt | 644 | =OCT2DEC("644") | 420 | Read-only |
| secret.key | 600 | =OCT2DEC("600") | 384 | Owner only |
| public.html | 777 | =OCT2DEC("777") | 511 | Full access |

Permission difference: =OCT2DEC("755") - OCT2DEC("644") = 73
This shows 755 has 73 more permission "units" than 644
```

### 2. Legacy Mainframe Address Conversion

**Scenario**: An engineer is documenting memory addresses from a legacy PDP-11 or similar system that used octal addressing, converting to decimal for modern documentation.

**Implementation**: Octal addresses are converted to decimal for cross-referencing with modern systems.

```
Legacy system memory map:
| Component | Octal Address | Formula | Decimal | Hex |
|-----------|---------------|---------|---------|-----|
| ROM Start | 160000 | =OCT2DEC("160000") | 57344 | E000 |
| RAM Start | 0 | =OCT2DEC("0") | 0 | 0000 |
| I/O Base | 177400 | =OCT2DEC("177400") | 65280 | FF00 |
| Vector Table | 100 | =OCT2DEC("100") | 64 | 0040 |

Address calculation: =OCT2DEC("160000") + OCT2DEC("400") = 57600
```

### 3. Octal Arithmetic Verification

**Scenario**: A student or engineer needs to verify octal arithmetic by converting to decimal, performing the operation, and converting back.

**Implementation**: Complex octal operations are verified through decimal intermediates.

```
Verify: 755 (octal) + 22 (octal) = 777 (octal)

Step 1: Convert to decimal
=OCT2DEC("755") → 493
=OCT2DEC("22")  → 18

Step 2: Perform decimal arithmetic
493 + 18 = 511

Step 3: Convert back
=DEC2OCT(511) → 777 ✓

Verified: 755 + 22 = 777 in octal
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum input length | 10 characters | 10 characters |
| Minimum value | -536,870,912 | -536,870,912 |
| Maximum value | 536,870,911 | 536,870,911 |
| Valid input digits | 0-7 only | 0-7 only |
| Return type | Number | Number |
| Negative number handling | Two's complement | Two's complement |

Both platforms implement OCT2DEC identically. The function returns an actual number (not text), allowing the result to be used directly in arithmetic operations. This differs from OCT2BIN and OCT2HEX which return text strings.

## Tips and Best Practices

1. **Remember octal uses 0-7 only**: The digits 8 and 9 do not exist in octal. If you see them in supposed octal input, it's an error.

2. **Use for permission calculations**: Convert permissions to decimal to compare them: `=OCT2DEC("755")>OCT2DEC("644")` returns TRUE (755 is more permissive).

3. **Result is numeric**: Unlike OCT2BIN and OCT2HEX, OCT2DEC returns a number that can be used directly in calculations without conversion.

4. **Understand octal powers**: 8^0=1, 8^1=8, 8^2=64, 8^3=512, 8^4=4096. Quick mental math: multiply each digit by its power position.

5. **Watch for negative values**: 10-digit octal starting with 4, 5, 6, or 7 represents negative numbers in two's complement. "4000000000" is the minimum (-536,870,912).

6. **Validate input**: Use `=IFERROR(OCT2DEC(A1),"Invalid octal")` to handle invalid input gracefully.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[DEC2OCT]] | Converts decimal to octal (inverse function) |
| [[OCT2BIN]] | Converts octal to binary |
| [[OCT2HEX]] | Converts octal to hexadecimal |
| [[BIN2DEC]] | Converts binary to decimal |
| [[HEX2DEC]] | Converts hexadecimal to decimal |

### Commonly Used With

- **[[DEC2OCT]]** - Round-trip verification of conversions
- **[[DEC2HEX]]** - Convert decimal result to hex
- **[[SUM]]** / **[[AVERAGE]]** - Aggregate converted permission values
- **[[IF]]** - Compare permission levels
- **[[MOD]]** - Extract individual octal digits from decimal

## Official Documentation

- [Microsoft Excel OCT2DEC Documentation](https://support.microsoft.com/en-us/office/oct2dec-function-87606014-cb98-44b2-8dbb-e48f8ced1554)
- [Google Sheets OCT2DEC Documentation](https://support.google.com/docs/answer/3093232)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Available via Analysis ToolPak add-in |
| Excel 2007 | 2007 | Integrated into standard function library |
| Excel 2010 | 2010 | No changes |
| Excel 2013 | 2013 | No changes |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2006 | Available since launch |
