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
- binary-conversion
- hexadecimal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- binary-to-hexadecimal
- bin-to-hex
tags:
- engineering-function
- base-conversion
- binary
- hexadecimal
- number-systems
---

# BIN2HEX

## Description

BIN2HEX converts a binary (base-2) number to its hexadecimal (base-16) equivalent. Hexadecimal is widely used in computing because it provides a compact representation of binary data - each hexadecimal digit represents exactly four binary digits (bits). This makes it ideal for representing memory addresses, color codes, MAC addresses, and other computing values that would be unwieldy in binary. The function is essential for programmers, hardware engineers, and anyone working with low-level computing data.

The conversion from binary to hexadecimal groups binary digits into sets of four (nibbles), converting each group to its hex equivalent (0-9, A-F). For example, binary 11110101 becomes F5 in hex: 1111 = F and 0101 = 5. BIN2HEX handles negative numbers using two's complement representation, where a 10-digit binary starting with 1 represents a negative value and produces a 10-character hexadecimal result with leading Fs.

Use BIN2HEX when working with color codes in web development (converting RGB binary values), analyzing network packets, documenting memory addresses, or creating compact representations of binary data. The optional places parameter allows you to specify the exact number of characters in the output, which is useful for maintaining consistent formatting in technical documentation or when padding is required for fixed-width fields.

Both Excel and Google Sheets implement BIN2HEX identically. The function accepts binary numbers up to 10 characters long and can output hexadecimal values with specified padding. The maximum positive result is 1FF (binary 0111111111 = 511 decimal), and negative values are represented in two's complement hex format. Both platforms return uppercase hexadecimal letters (A-F).

## Syntax

> [!NOTE] Syntax
> ```
> BIN2HEX(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The binary number to convert. Can be up to 10 characters (bits) long. Characters must be 0 or 1 only. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Pads with leading zeros if needed. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=BIN2HEX("1111")` | F | Binary 1111 = decimal 15 = hex F |
| `=BIN2HEX("10101010")` | AA | Binary 10101010 = 170 decimal = AA hex |
| `=BIN2HEX("1111", 4)` | 000F | Padded to 4 characters with leading zeros |
| `=BIN2HEX("11111111")` | FF | Maximum 8-bit value (255 decimal) |
| `=BIN2HEX("0111111111")` | 1FF | Maximum positive 10-bit value (511 decimal) |
| `=BIN2HEX("1000000000")` | FFFFFFFE00 | Two's complement negative (-512 decimal) |
| `=BIN2HEX("1111111111")` | FFFFFFFFFF | Two's complement -1 in hex |
| `=BIN2HEX("11110000")` | F0 | High nibble all 1s, low nibble all 0s |
| `=BIN2HEX("1100110011")` | FFFFFFFF33 | Negative number (-205) in two's complement |
| `=BIN2HEX("0", 2)` | 00 | Zero padded to 2 characters |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input contains characters other than 0 or 1 | Ensure input contains only binary digits (0 and 1) |
| `#NUM!` | Input exceeds 10 characters | Limit binary input to 10 bits maximum |
| `#NUM!` | Places parameter is negative | Use a positive integer for places |
| `#NUM!` | Places is too small for the result | Increase places value or omit parameter |
| `#VALUE!` | Non-numeric or invalid text input | Verify input is a valid binary number string |

## Use Cases

### 1. Web Color Code Generation

**Scenario**: A web developer needs to convert individual RGB color channel values from binary sensor data to hexadecimal color codes for CSS styling.

**Implementation**: Each 8-bit color channel is converted from binary to hex, then combined into a standard web color code.

```
Red channel:   =BIN2HEX("11111111", 2) → FF
Green channel: =BIN2HEX("10000000", 2) → 80
Blue channel:  =BIN2HEX("00000000", 2) → 00
Combined: ="#"&BIN2HEX(R,2)&BIN2HEX(G,2)&BIN2HEX(B,2) → #FF8000 (orange)
```

### 2. MAC Address Formatting

**Scenario**: A network administrator receives binary representations of MAC address octets from a device diagnostic and needs to format them as standard hexadecimal MAC addresses.

**Implementation**: Each 8-bit octet is converted to 2-digit hex for proper MAC address formatting.

```
| Octet | Binary | Formula | Hex |
|-------|--------|---------|-----|
| 1 | 00001010 | =BIN2HEX(A2,2) | 0A |
| 2 | 10110111 | =BIN2HEX(A3,2) | B7 |
| 3 | 11001101 | =BIN2HEX(A4,2) | CD |
| 4 | 11100101 | =BIN2HEX(A5,2) | E5 |
| 5 | 00011111 | =BIN2HEX(A6,2) | 1F |
| 6 | 10101010 | =BIN2HEX(A7,2) | AA |
Result: 0A:B7:CD:E5:1F:AA
```

### 3. Memory Address Documentation

**Scenario**: An embedded systems engineer is documenting memory maps and needs to convert binary address ranges to hexadecimal notation for technical specifications.

**Implementation**: Binary memory addresses are converted to hex for compact, readable documentation.

```
Flash Start:  =BIN2HEX("0000000000", 4) → 0000
Flash End:    =BIN2HEX("0111111111", 4) → 01FF
RAM Start:    =BIN2HEX("1000000000") → Two's complement treated as address
Note: For addresses, use DEC2HEX for values > 511
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum input length | 10 characters | 10 characters |
| Maximum places value | 10 | 10 |
| Letter case in output | Uppercase (A-F) | Uppercase (A-F) |
| Negative number handling | Two's complement | Two's complement |
| Leading zeros with places | Supported | Supported |
| Return type | Text | Text |

Both platforms implement BIN2HEX identically. The function returns a text string (not a number) because hexadecimal values contain letters. This means the result cannot be used directly in arithmetic without first converting back to decimal.

## Tips and Best Practices

1. **Use places for consistent formatting**: When creating fixed-width outputs like MAC addresses or color codes, always specify the places parameter (e.g., `=BIN2HEX(A1, 2)` for byte values).

2. **Understand nibble grouping**: Each hex digit represents 4 binary digits. When visually converting, split binary into groups of 4 from the right: 10110111 → 1011 0111 → B7.

3. **Watch for two's complement**: 10-bit binary numbers starting with 1 produce 10-character hex results representing negative values. If you want unsigned conversion for values 512-1023, convert to decimal first.

4. **Combine with CONCAT for formatting**: Build formatted hex strings like "0x" prefix: `="0x"&BIN2HEX(A1)` produces "0xFF" style output.

5. **Validate binary input**: Use `=IF(ISERROR(BIN2HEX(A1)),"Invalid",BIN2HEX(A1))` to gracefully handle invalid input.

6. **Remember hex is text**: The result is a text string. To use it numerically, convert back with HEX2DEC: `=HEX2DEC(BIN2HEX(A1))`.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BIN2DEC]] | Converts binary to decimal |
| [[BIN2OCT]] | Converts binary to octal |
| [[HEX2BIN]] | Converts hexadecimal to binary (inverse function) |
| [[DEC2HEX]] | Converts decimal to hexadecimal |
| [[OCT2HEX]] | Converts octal to hexadecimal |

### Commonly Used With

- **[[CONCAT]]** / **[[CONCATENATE]]** - Build formatted hex strings with prefixes or delimiters
- **[[HEX2DEC]]** - Convert hex result back to decimal for calculations
- **[[RIGHT]]** / **[[LEFT]]** - Extract specific hex digits from results
- **[[UPPER]]** - Not needed; output is already uppercase

## Official Documentation

- [Microsoft Excel BIN2HEX Documentation](https://support.microsoft.com/en-us/office/bin2hex-function-0375e507-f5e5-4077-9af8-28d84f9f41cc)
- [Google Sheets BIN2HEX Documentation](https://support.google.com/docs/answer/3093223)

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
