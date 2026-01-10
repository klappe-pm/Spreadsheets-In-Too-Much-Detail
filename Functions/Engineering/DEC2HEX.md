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
- decimal-conversion
- hexadecimal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- decimal-to-hexadecimal
- dec-to-hex
tags:
- engineering-function
- base-conversion
- decimal
- hexadecimal
- number-systems
---

# DEC2HEX

## Description

DEC2HEX converts a decimal (base-10) number to its hexadecimal (base-16) equivalent. Hexadecimal uses 16 symbols: digits 0-9 for values zero through nine, and letters A-F for values ten through fifteen. Each hexadecimal digit represents exactly 4 bits, making it the standard notation for representing binary data compactly in computing. This function is indispensable for programmers, web developers, hardware engineers, and anyone working with memory addresses, color codes, or low-level system data.

The conversion from decimal to hexadecimal involves repeatedly dividing by 16 and converting remainders. For example, 255 decimal: 255/16 = 15 remainder 15, then 15/16 = 0 remainder 15. Reading remainders as hex gives FF (15=F). DEC2HEX handles a much larger range than DEC2BIN, accepting values from -549,755,813,888 to 549,755,813,887, using two's complement representation for negative numbers.

Use DEC2HEX when creating web color codes, documenting memory addresses, converting IP addresses to hex, working with Unicode code points, or any situation requiring compact binary representation. The optional places parameter lets you specify exact output width with leading zeros, essential for fixed-width formatting like "0x00FF" style notation or consistent MAC address octets.

Both Excel and Google Sheets implement DEC2HEX identically, returning uppercase hexadecimal letters (A-F). The function supports a 40-bit signed integer range, far exceeding the 10-bit range of DEC2BIN. Negative numbers produce 10-character hex strings in two's complement format, while positive numbers can be formatted with specified character counts up to 10.

## Syntax

> [!NOTE] Syntax
> ```
> DEC2HEX(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The decimal integer to convert. Must be between -549,755,813,888 and 549,755,813,887. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Maximum is 10. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=DEC2HEX(255)` | FF | 255 decimal = 15*16 + 15 = FF |
| `=DEC2HEX(255, 4)` | 00FF | Padded to 4 characters |
| `=DEC2HEX(16)` | 10 | 16 decimal = 1*16 + 0 = 10 hex |
| `=DEC2HEX(4095)` | FFF | 4095 = 16^3 - 1 = FFF |
| `=DEC2HEX(65535)` | FFFF | Maximum 16-bit unsigned (2^16 - 1) |
| `=DEC2HEX(-1)` | FFFFFFFFFF | Two's complement -1 (all Fs) |
| `=DEC2HEX(-256)` | FFFFFFFF00 | Two's complement -256 |
| `=DEC2HEX(16777215)` | FFFFFF | Maximum 24-bit value (white color) |
| `=DEC2HEX(0, 2)` | 00 | Zero padded to 2 characters |
| `=DEC2HEX(2748)` | ABC | Memorable example: 2748 = ABC |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number exceeds maximum (549,755,813,887) | Use smaller value within range |
| `#NUM!` | Number below minimum (-549,755,813,888) | Use larger value within range |
| `#NUM!` | Places parameter is negative | Use positive integer for places |
| `#NUM!` | Places is too small for result | Increase places or omit parameter |
| `#VALUE!` | Non-numeric input | Ensure input is a valid number |
| `#NUM!` | Places exceeds 10 | Maximum places value is 10 |

## Use Cases

### 1. Web Color Code Generation

**Scenario**: A web designer needs to convert RGB color values (each 0-255) to hexadecimal color codes for CSS styling.

**Implementation**: Each color channel is converted to 2-digit hex and concatenated with a hash prefix.

```
Color: RGB(128, 64, 255)
Red:   =DEC2HEX(128, 2) → 80
Green: =DEC2HEX(64, 2)  → 40
Blue:  =DEC2HEX(255, 2) → FF

Formula: ="#"&DEC2HEX(R,2)&DEC2HEX(G,2)&DEC2HEX(B,2)
Result: #8040FF (purple shade)
```

### 2. Memory Address Formatting

**Scenario**: A systems programmer needs to convert decimal memory offsets to hexadecimal addresses for debugging and documentation.

**Implementation**: Decimal addresses are converted to standard hex notation with consistent width.

```
| Description | Decimal | Formula | Hex Address |
|-------------|---------|---------|-------------|
| Base address | 65536 | =DEC2HEX(A2,8) | 00010000 |
| Stack pointer | 131072 | =DEC2HEX(A3,8) | 00020000 |
| Heap start | 262144 | =DEC2HEX(A4,8) | 00040000 |
| Function entry | 65793 | =DEC2HEX(A5,8) | 00010101 |

Display format: ="0x"&DEC2HEX(A2,8) → 0x00010000
```

### 3. MAC Address Construction

**Scenario**: A network administrator needs to generate MAC addresses from decimal device identifiers stored in a database.

**Implementation**: Decimal values are converted to properly formatted MAC address octets.

```
Device identifier: 175921860444402
Breaking into octets and converting:
=DEC2HEX(160,2) → A0
=DEC2HEX(54,2)  → 36
=DEC2HEX(156,2) → 9C
=DEC2HEX(248,2) → F8
=DEC2HEX(175,2) → AF
=DEC2HEX(242,2) → F2

Result: A0:36:9C:F8:AF:F2
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Minimum value | -549,755,813,888 | -549,755,813,888 |
| Maximum value | 549,755,813,887 | 549,755,813,887 |
| Maximum places | 10 | 10 |
| Letter case | Uppercase (A-F) | Uppercase (A-F) |
| Negative format | 10-char two's complement | 10-char two's complement |
| Return type | Text | Text |

Both platforms implement DEC2HEX identically. The function returns text (not numbers) because hex values can contain letters. The 40-bit range (2^39 positive values plus 2^39 negative values) accommodates most practical computing needs.

## Tips and Best Practices

1. **Always use places for consistent formatting**: For color codes use 2 per channel, for 16-bit values use 4, for 32-bit use 8: `=DEC2HEX(value, 8)`.

2. **Add prefixes with concatenation**: Create standard hex notation: `="0x"&DEC2HEX(A1,4)` produces "0x00FF" style output common in programming.

3. **Validate RGB range before conversion**: For color codes, ensure values are 0-255: `=IF(AND(A1>=0,A1<=255),DEC2HEX(A1,2),"Invalid")`.

4. **Use LOWER() for lowercase hex if needed**: Some systems prefer lowercase: `=LOWER(DEC2HEX(255))` returns "ff" instead of "FF".

5. **Remember the range difference from DEC2BIN**: DEC2HEX handles numbers up to ~550 billion, while DEC2BIN only handles -512 to 511. Use DEC2HEX for larger values.

6. **Chain conversions for binary from large decimals**: For large numbers to binary, convert to hex first, then each hex digit to 4 binary digits manually or via lookup table.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[HEX2DEC]] | Converts hexadecimal to decimal (inverse function) |
| [[DEC2BIN]] | Converts decimal to binary |
| [[DEC2OCT]] | Converts decimal to octal |
| [[BIN2HEX]] | Converts binary to hexadecimal |
| [[OCT2HEX]] | Converts octal to hexadecimal |

### Commonly Used With

- **[[CONCAT]]** / **[[CONCATENATE]]** - Build color codes or formatted addresses
- **[[LOWER]]** - Convert to lowercase hex if required
- **[[IF]]** - Validate input ranges before conversion
- **[[HEX2DEC]]** - Reverse conversion for verification
- **[[TEXT]]** - Additional formatting options

## Official Documentation

- [Microsoft Excel DEC2HEX Documentation](https://support.microsoft.com/en-us/office/dec2hex-function-6344ee8b-b6b5-4c6a-a672-f64666704619)
- [Google Sheets DEC2HEX Documentation](https://support.google.com/docs/answer/3093226)

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
