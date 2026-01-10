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
- binary-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- decimal-to-binary
- dec-to-bin
tags:
- engineering-function
- base-conversion
- decimal
- binary
- number-systems
---

# DEC2BIN

## Description

DEC2BIN converts a decimal (base-10) number to its binary (base-2) equivalent. Binary is the fundamental language of digital computing, representing all data as sequences of 0s and 1s. Each binary digit (bit) represents a power of 2, with the rightmost bit being 2^0 (1), the next being 2^1 (2), then 2^2 (4), and so on. This function is essential for programmers, hardware engineers, and anyone who needs to understand or work with computer data at the bit level.

The conversion from decimal to binary involves dividing the decimal number by 2 repeatedly and recording the remainders. For example, 13 decimal becomes 1101 binary: 13/2=6 remainder 1, 6/2=3 remainder 0, 3/2=1 remainder 1, 1/2=0 remainder 1, reading remainders backward gives 1101. DEC2BIN handles negative numbers using two's complement representation, where the most significant bit indicates the sign.

Use DEC2BIN when you need to visualize bit patterns, understand bitwise operations, create binary masks for hardware programming, or convert human-readable decimal values to machine-level binary. The function is particularly useful for generating bit patterns for flags, control registers, or any application where individual bit positions have specific meanings. The optional places parameter lets you specify exact output length with leading zeros.

Both Excel and Google Sheets implement DEC2BIN identically. The function accepts decimal integers from -512 to 511 and produces binary strings up to 10 characters long. Positive numbers can be padded with leading zeros using the places parameter, while negative numbers always return 10 binary digits in two's complement format.

## Syntax

> [!NOTE] Syntax
> ```
> DEC2BIN(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The decimal integer to convert. Must be between -512 and 511 inclusive. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Only applies to non-negative numbers. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=DEC2BIN(10)` | 1010 | 10 = 8+2 = 2^3 + 2^1 |
| `=DEC2BIN(255)` | 11111111 | Maximum 8-bit unsigned value |
| `=DEC2BIN(255, 8)` | 11111111 | 8-bit format (no padding needed) |
| `=DEC2BIN(15, 8)` | 00001111 | 15 padded to 8 bits with leading zeros |
| `=DEC2BIN(511)` | 111111111 | Maximum positive value (9 bits) |
| `=DEC2BIN(-1)` | 1111111111 | Two's complement of -1 (all 1s) |
| `=DEC2BIN(-512)` | 1000000000 | Minimum negative value (two's complement) |
| `=DEC2BIN(0)` | 0 | Zero in any base |
| `=DEC2BIN(1)` | 1 | Simplest positive binary number |
| `=DEC2BIN(128, 8)` | 10000000 | Common byte boundary value |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number is less than -512 | Keep input within range -512 to 511 |
| `#NUM!` | Number is greater than 511 | Keep input within range -512 to 511 |
| `#NUM!` | Places parameter is negative | Use a positive integer for places |
| `#NUM!` | Places is too small for result | Increase places or omit parameter |
| `#VALUE!` | Non-numeric input | Ensure input is a valid number |
| `#NUM!` | Decimal (non-integer) input | Use INT() or TRUNC() to get integer portion |

## Use Cases

### 1. Bit Flag Generation for Hardware Control

**Scenario**: An embedded systems programmer needs to create binary control words for a hardware register where each bit enables a specific feature.

**Implementation**: Decimal values representing feature combinations are converted to binary to visualize active bits.

```
| Feature Combination | Decimal | Formula | Binary | Active Bits |
|---------------------|---------|---------|--------|-------------|
| No features | 0 | =DEC2BIN(0,8) | 00000000 | None |
| Feature A only | 1 | =DEC2BIN(1,8) | 00000001 | Bit 0 |
| Features A+B | 3 | =DEC2BIN(3,8) | 00000011 | Bits 0,1 |
| All features | 255 | =DEC2BIN(255,8) | 11111111 | All bits |
| Features A+C+D | 13 | =DEC2BIN(13,8) | 00001101 | Bits 0,2,3 |
```

### 2. Network Subnet Mask Visualization

**Scenario**: A network engineer needs to visualize subnet masks in binary format to understand which bits represent network vs. host portions.

**Implementation**: Each octet of the subnet mask is converted to binary to show the bit pattern.

```
Subnet mask: 255.255.252.0 (/22)
=DEC2BIN(255,8) → 11111111 (first octet)
=DEC2BIN(255,8) → 11111111 (second octet)
=DEC2BIN(252,8) → 11111100 (third octet - note last 2 bits are 0)
=DEC2BIN(0,8)   → 00000000 (fourth octet)

Visual: 11111111.11111111.11111100.00000000
        |----network bits----||host bits|
```

### 3. Binary Arithmetic Teaching Tool

**Scenario**: A computer science instructor creates a worksheet demonstrating how decimal addition translates to binary addition with carry operations.

**Implementation**: Decimal operands and results are shown alongside their binary equivalents.

```
| Operation | Decimal | Binary Formula | Binary |
|-----------|---------|----------------|--------|
| Operand A | 45 | =DEC2BIN(45,8) | 00101101 |
| Operand B | 27 | =DEC2BIN(27,8) | 00011011 |
| Sum | 72 | =DEC2BIN(72,8) | 01001000 |

Students can verify: 00101101 + 00011011 = 01001000 with carries
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Input range | -512 to 511 | -512 to 511 |
| Maximum output length | 10 characters | 10 characters |
| Places parameter range | 1 to 10 | 1 to 10 |
| Negative number format | 10-digit two's complement | 10-digit two's complement |
| Decimal input handling | Truncates to integer | Truncates to integer |
| Return type | Text | Text |

Both platforms implement DEC2BIN identically. Decimal inputs are truncated (not rounded) to integers before conversion. The function returns text to preserve leading zeros when the places parameter is used.

## Tips and Best Practices

1. **Always use places for byte alignment**: When working with 8-bit values, always specify `=DEC2BIN(n, 8)` to get consistent 8-character output with leading zeros.

2. **Understand the range limitation**: DEC2BIN only works with -512 to 511. For larger numbers, use a custom formula or convert to hex first, then hex to binary.

3. **Remember truncation behavior**: DEC2BIN truncates decimals: `=DEC2BIN(10.9)` returns "1010" (same as 10), not "1011". Use ROUND() if you want rounding.

4. **Use for bit manipulation visualization**: Before performing BITAND, BITOR, or BITXOR operations, convert operands to binary to visualize what will happen bit-by-bit.

5. **Negative numbers always return 10 digits**: When converting negative numbers, the places parameter is ignored. `=DEC2BIN(-1, 4)` still returns "1111111111", not "1111".

6. **Build conversion helper ranges**: Create a lookup table with DEC2BIN for values 0-15 to quickly reference when analyzing hex nibbles: each hex digit = 4 binary digits.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BIN2DEC]] | Converts binary to decimal (inverse function) |
| [[DEC2HEX]] | Converts decimal to hexadecimal |
| [[DEC2OCT]] | Converts decimal to octal |
| [[HEX2BIN]] | Converts hexadecimal to binary |
| [[OCT2BIN]] | Converts octal to binary |

### Commonly Used With

- **[[BITAND]]** - Perform AND operations, visualize with DEC2BIN
- **[[BITOR]]** - Perform OR operations, visualize with DEC2BIN
- **[[BITXOR]]** - Perform XOR operations, visualize with DEC2BIN
- **[[INT]]** / **[[TRUNC]]** - Ensure integer input before conversion
- **[[CONCAT]]** - Combine multiple binary bytes

## Official Documentation

- [Microsoft Excel DEC2BIN Documentation](https://support.microsoft.com/en-us/office/dec2bin-function-0f63dd0e-5d1a-42d8-b511-5bf5c6d43838)
- [Google Sheets DEC2BIN Documentation](https://support.google.com/docs/answer/3093225)

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
