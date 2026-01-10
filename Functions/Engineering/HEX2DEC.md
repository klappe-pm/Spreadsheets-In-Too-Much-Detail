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
- decimal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- hexadecimal-to-decimal
- hex-to-dec
tags:
- engineering-function
- base-conversion
- hexadecimal
- decimal
- number-systems
---

# HEX2DEC

## Description

HEX2DEC converts a hexadecimal (base-16) number to its decimal (base-10) equivalent. Hexadecimal is the standard notation for representing binary data in computing, using digits 0-9 and letters A-F to represent values 0-15. This function is essential for programmers, web developers, network administrators, and anyone who needs to interpret hex values in human-readable decimal format for calculations, comparisons, or documentation.

The conversion from hexadecimal to decimal multiplies each hex digit by powers of 16. For example, hex "2A" = (2 * 16^1) + (10 * 16^0) = 32 + 10 = 42 decimal. HEX2DEC handles a 40-bit range, accepting values from FFFFFFFE00 (-549,755,813,888) to 7FFFFFFFFF (549,755,813,887). Negative values use two's complement representation where hex strings starting with 8-F (in the first position of a 10-character string) represent negative numbers.

Use HEX2DEC when you need to convert memory addresses for arithmetic, translate color codes to RGB decimal values, interpret network protocol fields, analyze Unicode code points, or perform calculations on hex data from system logs. The function enables mathematical operations that aren't possible with hex strings directly.

Both Excel and Google Sheets implement HEX2DEC identically. The function accepts hex strings up to 10 characters (40 bits), case-insensitive input (both "FF" and "ff" work), and returns a numeric value (not text). This allows the result to be used directly in calculations, unlike the text output of functions like DEC2HEX.

## Syntax

> [!NOTE] Syntax
> ```
> HEX2DEC(number)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The hexadecimal number to convert. Can be up to 10 characters (0-9, A-F). Case-insensitive. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=HEX2DEC("FF")` | 255 | FF = 15*16 + 15 = 255 (max 8-bit) |
| `=HEX2DEC("A")` | 10 | Single hex digit A = 10 decimal |
| `=HEX2DEC("10")` | 16 | 10 hex = 1*16 + 0 = 16 decimal |
| `=HEX2DEC("FFFF")` | 65535 | Maximum 16-bit unsigned value |
| `=HEX2DEC("FFFFFF")` | 16777215 | Maximum 24-bit value (white color) |
| `=HEX2DEC("FFFFFFFF")` | 4294967295 | Maximum 32-bit unsigned value |
| `=HEX2DEC("FFFFFFFFFF")` | -1 | Two's complement -1 |
| `=HEX2DEC("FFFFFFFE00")` | -512 | Two's complement -512 |
| `=HEX2DEC("7FFFFFFFFF")` | 549755813887 | Maximum positive value |
| `=HEX2DEC("abc")` | 2748 | Lowercase input works |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Invalid hex characters (G-Z, symbols) | Use only 0-9 and A-F |
| `#NUM!` | Input exceeds 10 characters | Limit hex string to 10 characters |
| `#VALUE!` | Empty cell or non-text input | Provide valid hex string |
| `#VALUE!` | Spaces in input | Remove spaces from hex string |
| `#NUM!` | Input starts with 0x prefix | Remove "0x" prefix before conversion |

## Use Cases

### 1. RGB Color Code to Decimal Conversion

**Scenario**: A web developer needs to convert hex color codes to decimal RGB values for use in image processing functions or APIs that require decimal input.

**Implementation**: Each 2-character hex pair is converted to its decimal equivalent (0-255).

```
Color: #3498DB (a pleasant blue)
Red:   =HEX2DEC("34") → 52
Green: =HEX2DEC("98") → 152
Blue:  =HEX2DEC("DB") → 219

Result: RGB(52, 152, 219)
CSS: rgb(52, 152, 219)
```

### 2. Memory Address Calculations

**Scenario**: A systems programmer needs to calculate offsets and perform arithmetic on memory addresses displayed in hexadecimal format from a debugger.

**Implementation**: Hex addresses are converted to decimal for arithmetic operations.

```
Base address: 0x00400000
=HEX2DEC("00400000") → 4194304

Function offset: 0x1A2B
=HEX2DEC("1A2B") → 6699

Absolute address calculation:
=HEX2DEC("00400000") + HEX2DEC("1A2B") → 4201003
Converting back: =DEC2HEX(4201003) → 401A2B
```

### 3. Network Protocol Analysis

**Scenario**: A network engineer needs to analyze packet fields captured in hex format, calculating values like packet lengths, checksums, and port numbers.

**Implementation**: Hex fields from network captures are converted to decimal for analysis.

```
From packet capture (Wireshark hex):
Source Port: 0x0050 → =HEX2DEC("0050") → 80 (HTTP)
Dest Port: 0xC000 → =HEX2DEC("C000") → 49152 (ephemeral)
Packet Length: 0x0540 → =HEX2DEC("0540") → 1344 bytes
Checksum: 0xA7B3 → =HEX2DEC("A7B3") → 42931

Total data transferred: =SUM(HEX2DEC(len1), HEX2DEC(len2), ...)
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum input length | 10 characters | 10 characters |
| Minimum value | -549,755,813,888 | -549,755,813,888 |
| Maximum value | 549,755,813,887 | 549,755,813,887 |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Return type | Number | Number |
| Negative number handling | Two's complement | Two's complement |

Both platforms implement HEX2DEC identically. The function returns an actual number (not text), so results can be used directly in arithmetic operations. This is different from hex output functions which return text strings.

## Tips and Best Practices

1. **Strip common prefixes**: Remove "0x" or "#" prefixes before using HEX2DEC: `=HEX2DEC(SUBSTITUTE(A1,"0x",""))` or `=HEX2DEC(SUBSTITUTE(A1,"#",""))`.

2. **Use for color math**: Calculate color averages, differences, or blends by converting to decimal: `=(HEX2DEC("FF")+HEX2DEC("00"))/2` = 127.5 for midpoint.

3. **Result is numeric**: Unlike DEC2HEX which returns text, HEX2DEC returns a number. You can use it directly in calculations without conversion.

4. **Handle large hex values carefully**: 10-character hex starting with 8-F represents negative numbers in two's complement. "8000000000" = -549,755,813,888.

5. **Extract and convert color channels**: For colors, use `=HEX2DEC(MID(color,2,2))` for red, `=HEX2DEC(MID(color,4,2))` for green, `=HEX2DEC(MID(color,6,2))` for blue.

6. **Validate hex input**: Use `=IF(ISERROR(HEX2DEC(A1)),"Invalid",HEX2DEC(A1))` to gracefully handle invalid input.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[DEC2HEX]] | Converts decimal to hexadecimal (inverse function) |
| [[HEX2BIN]] | Converts hexadecimal to binary |
| [[HEX2OCT]] | Converts hexadecimal to octal |
| [[BIN2DEC]] | Converts binary to decimal |
| [[OCT2DEC]] | Converts octal to decimal |

### Commonly Used With

- **[[MID]]** - Extract hex digit pairs from color codes
- **[[SUBSTITUTE]]** - Remove prefixes like "0x" or "#"
- **[[DEC2HEX]]** - Convert result back to hex after calculations
- **[[IF]]** / **[[IFERROR]]** - Handle invalid input gracefully
- **[[CONCATENATE]]** - Build hex strings from components

## Official Documentation

- [Microsoft Excel HEX2DEC Documentation](https://support.microsoft.com/en-us/office/hex2dec-function-8c8c3155-9f37-45a5-a3ee-ee5379ef106e)
- [Google Sheets HEX2DEC Documentation](https://support.google.com/docs/answer/3093229)

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
