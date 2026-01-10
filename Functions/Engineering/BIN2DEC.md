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
- decimal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- binary-to-decimal
- bin-to-dec
tags:
- engineering-function
- base-conversion
- binary
- decimal
- number-systems
---

# BIN2DEC

## Description

BIN2DEC converts a binary (base-2) number to its decimal (base-10) equivalent. Binary is the foundational numbering system used by all digital computers, representing values using only two digits: 0 and 1. Each position in a binary number represents a power of 2, starting from 2^0 on the right. This function is essential for engineers, programmers, and data analysts who work with low-level computing data, hardware specifications, or digital signal processing.

The conversion from binary to decimal works by multiplying each binary digit by its corresponding power of 2 and summing the results. For example, the binary number 1101 equals (1 x 2^3) + (1 x 2^2) + (0 x 2^1) + (1 x 2^0) = 8 + 4 + 0 + 1 = 13 in decimal. BIN2DEC handles both positive numbers and negative numbers using two's complement representation, where the leftmost bit (bit 9 for a 10-digit binary) indicates the sign.

Use BIN2DEC when you need to interpret binary data in human-readable decimal format, convert memory addresses or register values, analyze network packets, or work with embedded systems data. The function is particularly valuable when debugging hardware interfaces, reviewing binary file contents, or converting between number systems for documentation purposes. It accepts binary numbers as text strings or numeric values up to 10 characters long.

Both Excel and Google Sheets implement BIN2DEC identically, accepting the same input format and returning the same decimal results. The function handles two's complement negative numbers (where the first bit is 1 in a 10-digit binary) automatically, returning negative decimal values. The maximum positive value is 0111111111 (511 in decimal), and the minimum negative value is 1000000000 (-512 in decimal).

## Syntax

> [!NOTE] Syntax
> ```
> BIN2DEC(number)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The binary number to convert. Can be up to 10 characters (bits) long. Can be entered as text in quotes or as a number. Characters must be 0 or 1 only. | Yes |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=BIN2DEC("1010")` | 10 | Converts binary 1010 to decimal. (1x8)+(0x4)+(1x2)+(0x1)=10 |
| `=BIN2DEC(1111)` | 15 | Numeric input works. (1x8)+(1x4)+(1x2)+(1x1)=15 |
| `=BIN2DEC("0")` | 0 | Zero in any base is zero |
| `=BIN2DEC("1")` | 1 | Single bit representing 2^0 = 1 |
| `=BIN2DEC("11111111")` | 255 | Maximum 8-bit unsigned value (2^8 - 1) |
| `=BIN2DEC("0111111111")` | 511 | Maximum positive value with 10 bits |
| `=BIN2DEC("1000000000")` | -512 | Two's complement negative (minimum value) |
| `=BIN2DEC("1111111111")` | -1 | Two's complement representation of -1 |
| `=BIN2DEC("1111111110")` | -2 | Two's complement representation of -2 |
| `=BIN2DEC("100000000")` | 256 | 9-bit number: 2^8 = 256 (positive, not two's complement) |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input contains characters other than 0 or 1 | Ensure input contains only binary digits (0 and 1) |
| `#NUM!` | Input exceeds 10 characters | Limit binary input to 10 bits maximum |
| `#VALUE!` | Non-numeric text input | Verify input is a valid binary number string |
| `#NUM!` | Negative number input | Binary input cannot be negative; use two's complement for negative values |
| `#VALUE!` | Empty cell reference | Ensure referenced cell contains a value |

## Use Cases

### 1. Hardware Register Analysis

**Scenario**: An embedded systems engineer needs to interpret status register values from a microcontroller that reports data in binary format.

**Implementation**: The engineer receives binary status codes from diagnostic output and needs to convert them to decimal for documentation and comparison with specification sheets.

```
| Register Name | Binary Value | Formula | Decimal Value |
|---------------|--------------|---------|---------------|
| STATUS_A      | 10110100     | =BIN2DEC(A2) | 180 |
| STATUS_B      | 01001011     | =BIN2DEC(A3) | 75 |
| ERROR_CODE    | 00001111     | =BIN2DEC(A4) | 15 |
```

### 2. Network Subnet Mask Conversion

**Scenario**: A network administrator needs to convert binary subnet mask octets to decimal notation for configuring network devices.

**Implementation**: Each octet of an IP subnet mask is converted from binary to decimal to create the familiar dotted-decimal notation.

```
Binary subnet mask: 11111111.11111111.11111100.00000000
=BIN2DEC("11111111") → 255
=BIN2DEC("11111111") → 255
=BIN2DEC("11111100") → 252
=BIN2DEC("00000000") → 0
Result: 255.255.252.0 (a /22 subnet)
```

### 3. Digital Signal Processing Data Analysis

**Scenario**: A data analyst is reviewing ADC (Analog-to-Digital Converter) output from a sensor system that logs data in binary format.

**Implementation**: Raw binary sensor readings need conversion to decimal values for statistical analysis and visualization.

```
=BIN2DEC(A1) where A1 contains ADC binary output
=BIN2DEC("01111111") → 127 (mid-scale for 8-bit ADC)
=BIN2DEC("11111111") → 255 (full-scale for 8-bit ADC)
=BIN2DEC("00000000") → 0 (zero-scale for 8-bit ADC)
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum input length | 10 characters | 10 characters |
| Negative number handling | Two's complement | Two's complement |
| Leading zeros | Accepted | Accepted |
| Numeric input format | Supported | Supported |
| Text input format | Supported | Supported |
| Return type | Number | Number |

Both platforms implement BIN2DEC identically with no functional differences. The function was originally part of the Analysis ToolPak in older Excel versions but is now built into the standard function library.

## Tips and Best Practices

1. **Use text format for binary input**: Wrap binary numbers in quotes ("1010") to prevent Excel from interpreting leading zeros incorrectly or treating the number as decimal.

2. **Understand two's complement**: When working with 10-bit binary numbers starting with 1, the result will be negative. This follows two's complement representation where 1000000000 = -512.

3. **Validate input before conversion**: Use a helper formula like `=AND(ISNUMBER(SEARCH("0",A1)+SEARCH("1",A1)),LEN(A1)<=10)` to verify valid binary input.

4. **Chain with other conversions**: Combine with DEC2HEX or DEC2OCT when you need to convert binary to other bases via decimal as an intermediate step.

5. **Handle variable-length binary**: The function works with binary numbers of any length up to 10 digits. Leading zeros don't affect the result: BIN2DEC("00001010") = BIN2DEC("1010") = 10.

6. **Consider bit significance**: For hardware applications, remember that the rightmost bit is the LSB (Least Significant Bit) with value 2^0, and each position left increases the power by 1.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BIN2HEX]] | Converts binary to hexadecimal |
| [[BIN2OCT]] | Converts binary to octal |
| [[DEC2BIN]] | Converts decimal to binary (inverse function) |
| [[HEX2DEC]] | Converts hexadecimal to decimal |
| [[OCT2DEC]] | Converts octal to decimal |

### Commonly Used With

- **[[DEC2HEX]]** - Chain BIN2DEC with DEC2HEX for binary-to-hex conversion with custom formatting
- **[[IF]]** - Validate binary input before conversion
- **[[TEXT]]** - Format decimal results with specific number formatting
- **[[IFERROR]]** - Handle invalid binary input gracefully

## Official Documentation

- [Microsoft Excel BIN2DEC Documentation](https://support.microsoft.com/en-us/office/bin2dec-function-63905b57-b3a0-453d-99f4-647bb519cd6c)
- [Google Sheets BIN2DEC Documentation](https://support.google.com/docs/answer/3093222)

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
