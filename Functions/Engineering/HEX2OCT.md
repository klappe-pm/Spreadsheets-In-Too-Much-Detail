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
- hexadecimal-conversion
- octal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- hexadecimal-to-octal
- hex-to-oct
tags:
- engineering-function
- base-conversion
- hexadecimal
- octal
- number-systems
---

# HEX2OCT

## Description

HEX2OCT converts a hexadecimal (base-16) number to its octal (base-8) equivalent. This function provides a direct conversion path between two number systems commonly used in computing, bypassing decimal as an intermediate step. While hexadecimal is dominant in modern computing for its compact binary representation, octal remains relevant in Unix/Linux file permissions and certain legacy systems.

The conversion from hexadecimal to octal internally translates through binary: each hex digit becomes 4 binary digits, then binary digits are regrouped into sets of 3 for octal representation. For example, hex "1F" = binary "00011111" = octal "37" (grouping as 000 011 111). HEX2OCT handles a limited range suitable for the 10-digit octal output, covering values from -536,870,912 to 536,870,911 decimal.

Use HEX2OCT when converting hex values to Unix permission format, working with legacy systems that use octal addressing, or when you need octal representation of data originally provided in hex. The optional places parameter lets you specify output width with leading zeros for consistent formatting.

Both Excel and Google Sheets implement HEX2OCT identically. The function accepts hex strings representing values within the 30-bit signed range (hex FFFFFFE000 to 1FFFFFFF for the full range) and returns octal strings up to 10 characters. Two's complement representation is used for negative numbers, producing octal results starting with digits 4-7.

## Syntax

> [!NOTE] Syntax
> ```
> HEX2OCT(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The hexadecimal number to convert. Must represent a value within the 30-bit signed range. Case-insensitive. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Maximum is 10. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=HEX2OCT("F")` | 17 | F hex = 15 decimal = 17 octal |
| `=HEX2OCT("FF")` | 377 | FF hex = 255 decimal = 377 octal |
| `=HEX2OCT("1FF")` | 777 | 1FF hex = 511 decimal = 777 octal |
| `=HEX2OCT("8")` | 10 | 8 hex = 8 decimal = 10 octal |
| `=HEX2OCT("40")` | 100 | 40 hex = 64 decimal = 100 octal |
| `=HEX2OCT("1ED", 4)` | 0755 | Common Unix permission padded |
| `=HEX2OCT("FFFFFFFFFF")` | 7777777777 | Two's complement -1 |
| `=HEX2OCT("FFFFFFE000")` | 4000000000 | Minimum negative value |
| `=HEX2OCT("1FFFFFFF")` | 3777777777 | Maximum positive value |
| `=HEX2OCT("0", 3)` | 000 | Zero padded to 3 digits |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Hex value outside valid range | Use value within 30-bit signed range |
| `#NUM!` | Invalid hex characters | Use only 0-9 and A-F |
| `#NUM!` | Places parameter negative | Use positive integer for places |
| `#NUM!` | Places too small for result | Increase places or omit parameter |
| `#VALUE!` | Empty or invalid input | Provide valid hex string |
| `#NUM!` | Input exceeds 10 characters | Limit hex input appropriately |

## Use Cases

### 1. Converting Hex Permission Masks to Octal

**Scenario**: A system administrator has file permission masks stored in hexadecimal format and needs to convert them to standard octal notation for chmod commands.

**Implementation**: Hex permission values are converted to their octal equivalents.

```
| Permission Description | Hex | Formula | Octal | chmod |
|------------------------|-----|---------|-------|-------|
| Full access owner only | 1C0 | =HEX2OCT("1C0",3) | 700 | chmod 700 |
| Standard executable | 1ED | =HEX2OCT("1ED",3) | 755 | chmod 755 |
| Read-only file | 1A4 | =HEX2OCT("1A4",3) | 644 | chmod 644 |
| Restricted access | 180 | =HEX2OCT("180",3) | 600 | chmod 600 |

Conversion: 0x1ED = 0755 (rwxr-xr-x)
```

### 2. Legacy System Data Migration

**Scenario**: An engineer is migrating data from a modern system using hex notation to a legacy mainframe system that requires octal addresses.

**Implementation**: Hex addresses are systematically converted to octal format.

```
Modern system (hex) → Legacy system (octal):
Device address 0x3F → =HEX2OCT("3F",3) → 077
Port number 0xFF → =HEX2OCT("FF",3) → 377
Buffer offset 0x200 → =HEX2OCT("200",4) → 1000
Channel code 0x7 → =HEX2OCT("7",2) → 07

Documentation format: "Set device to octal " & HEX2OCT(hex_value)
```

### 3. Educational Cross-Base Comparison

**Scenario**: A computer science instructor creates materials showing the relationships between hexadecimal and octal, demonstrating how both relate to binary.

**Implementation**: A comparison table shows values across number systems.

```
| Hex | Binary | Octal | Decimal | Formula |
|-----|--------|-------|---------|---------|
| 7 | 111 | 7 | 7 | =HEX2OCT("7") |
| 8 | 1000 | 10 | 8 | =HEX2OCT("8") |
| F | 1111 | 17 | 15 | =HEX2OCT("F") |
| 10 | 10000 | 20 | 16 | =HEX2OCT("10") |
| 3F | 111111 | 77 | 63 | =HEX2OCT("3F") |
| 40 | 1000000 | 100 | 64 | =HEX2OCT("40") |
| FF | 11111111 | 377 | 255 | =HEX2OCT("FF") |
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Minimum hex input | FFFFFFE000 (-536,870,912) | FFFFFFE000 (-536,870,912) |
| Maximum hex input | 1FFFFFFF (536,870,911) | 1FFFFFFF (536,870,911) |
| Maximum places | 10 | 10 |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Negative number handling | Two's complement | Two's complement |
| Return type | Text | Text |

Both platforms implement HEX2OCT identically. The function returns text to preserve leading zeros when the places parameter is used. The 30-bit range is smaller than HEX2DEC's 40-bit range because octal output is limited to 10 digits.

## Tips and Best Practices

1. **Use places=3 for Unix permissions**: Standard chmod codes use 3 octal digits: `=HEX2OCT(A1, 3)` ensures proper formatting like 755, 644.

2. **Understand the range limitation**: HEX2OCT handles a 30-bit range (up to about 536 million), smaller than HEX2DEC's 40-bit range. For larger values, convert to decimal first.

3. **Verify with intermediate decimal**: For important conversions, verify: `=OCT2DEC(HEX2OCT(A1))` should equal `=HEX2DEC(A1)`.

4. **Input is case-insensitive**: Both "FF" and "ff" produce "377". Use whichever matches your source data.

5. **Map common values**: Memorize key conversions: F=17, FF=377, 100=400 (hex to octal). These help verify results.

6. **Handle negative numbers carefully**: Negative values require 10-character hex input with high bits set. Shorter inputs are always positive.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[OCT2HEX]] | Converts octal to hexadecimal (inverse function) |
| [[HEX2DEC]] | Converts hexadecimal to decimal |
| [[HEX2BIN]] | Converts hexadecimal to binary |
| [[DEC2OCT]] | Converts decimal to octal |
| [[BIN2OCT]] | Converts binary to octal |

### Commonly Used With

- **[[OCT2DEC]]** - Verify conversion via decimal intermediate
- **[[HEX2DEC]]** - Alternative path through decimal
- **[[IF]]** - Validate input before conversion
- **[[UPPER]]** / **[[LOWER]]** - Normalize hex input case
- **[[TEXT]]** - Additional formatting options

## Official Documentation

- [Microsoft Excel HEX2OCT Documentation](https://support.microsoft.com/en-us/office/hex2oct-function-54d52808-5d19-4bd0-8a63-1096a5d11912)
- [Google Sheets HEX2OCT Documentation](https://support.google.com/docs/answer/3093230)

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
