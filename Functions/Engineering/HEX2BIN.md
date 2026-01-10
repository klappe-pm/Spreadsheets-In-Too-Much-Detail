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
- binary-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- hexadecimal-to-binary
- hex-to-bin
tags:
- engineering-function
- base-conversion
- hexadecimal
- binary
- number-systems
---

# HEX2BIN

## Description

HEX2BIN converts a hexadecimal (base-16) number to its binary (base-2) equivalent. This function performs the inverse of BIN2HEX, expanding compact hexadecimal notation into full binary representation. Each hexadecimal digit maps to exactly four binary digits, making hex a convenient shorthand for binary. This function is essential for programmers, hardware engineers, and network administrators who need to expand hex values into their bit-level representation for analysis or debugging.

The conversion from hexadecimal to binary replaces each hex digit with its 4-bit binary equivalent: 0=0000, 1=0001, ..., 9=1001, A=1010, B=1011, C=1100, D=1101, E=1110, F=1111. For example, hex "A5" becomes binary "10100101": A=1010 and 5=0101. HEX2BIN handles two's complement negative numbers, where hex values with high bits set (starting with 8-F in certain positions) represent negative values.

Use HEX2BIN when you need to analyze individual bits in color codes, examine flag registers, debug memory contents displayed in hex, or understand bit patterns in network protocols. The optional places parameter lets you specify output width with leading zeros, essential for maintaining consistent bit-width in documentation or when working with fixed-size registers.

Both Excel and Google Sheets implement HEX2BIN identically. The function accepts hex strings up to 10 characters but can only convert values that fit in 10 binary digits (range: -512 to 511 decimal, or hex 200 to 1FF for positive values). Input is case-insensitive (both "FF" and "ff" work). Two's complement is used for negative numbers.

## Syntax

> [!NOTE] Syntax
> ```
> HEX2BIN(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The hexadecimal number to convert. Must represent a value between -512 and 511 decimal. Can use uppercase or lowercase letters. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Maximum is 10. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=HEX2BIN("F")` | 1111 | F = 15 decimal = 1111 binary |
| `=HEX2BIN("A")` | 1010 | A = 10 decimal = 1010 binary |
| `=HEX2BIN("FF")` | 11111111 | FF = 255 decimal = 8 bits all 1s |
| `=HEX2BIN("F", 8)` | 00001111 | F padded to 8 bits |
| `=HEX2BIN("1FF")` | 111111111 | Maximum positive value (511 decimal) |
| `=HEX2BIN("FFFFFFFE00")` | 1000000000 | Two's complement -512 |
| `=HEX2BIN("FFFFFFFFFF")` | 1111111111 | Two's complement -1 |
| `=HEX2BIN("0", 4)` | 0000 | Zero padded to 4 bits |
| `=HEX2BIN("10")` | 10000 | 10 hex = 16 decimal = 10000 binary |
| `=HEX2BIN("ff")` | 11111111 | Lowercase input accepted |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Hex value too large (> 1FF for positive) | Use smaller hex value within range |
| `#NUM!` | Invalid hex characters (G-Z, special chars) | Use only 0-9 and A-F |
| `#NUM!` | Places parameter is negative | Use positive integer for places |
| `#NUM!` | Places too small for result | Increase places or omit parameter |
| `#VALUE!` | Empty or non-text input | Provide valid hex string |
| `#NUM!` | Result would exceed 10 binary digits | Limit input to valid range |

## Use Cases

### 1. Color Code Bit Analysis

**Scenario**: A web developer needs to analyze the binary composition of hex color codes to understand color channel values at the bit level for image processing algorithms.

**Implementation**: Each hex pair in a color code is converted to 8-bit binary to reveal individual bit patterns.

```
Color: #FF8040 (coral/orange)
Red channel:   =HEX2BIN("FF", 8) → 11111111 (full red)
Green channel: =HEX2BIN("80", 8) → 10000000 (half green)
Blue channel:  =HEX2BIN("40", 8) → 01000000 (quarter blue)

Analysis: High red, medium green, low blue = warm orange tone
```

### 2. Hardware Register Debugging

**Scenario**: An embedded systems engineer receives hex status codes from a debugger and needs to examine individual flag bits to diagnose hardware behavior.

**Implementation**: Hex register values are expanded to binary for bit-by-bit analysis.

```
Status register: 0xA5 (hex dump output)
=HEX2BIN("A5", 8) → 10100101

Bit interpretation:
Bit 7 (128): 1 = Error flag SET
Bit 6 (64):  0 = Interrupt disabled
Bit 5 (32):  1 = Ready flag SET
Bit 4 (16):  0 = Mode: Normal
Bit 3 (8):   0 = Reserved
Bit 2 (4):   1 = Data available SET
Bit 1 (2):   0 = No overflow
Bit 0 (1):   1 = Power on SET
```

### 3. Network Packet Analysis

**Scenario**: A network engineer is analyzing packet headers displayed in hex and needs to examine specific bit flags within protocol fields.

**Implementation**: Hex fields from packet captures are converted to binary for flag analysis.

```
TCP Header Flags (1 byte from Wireshark): 0x12
=HEX2BIN("12", 8) → 00010010

Flag interpretation (TCP):
Bit 7-6: Reserved
Bit 5: URG (0) - No urgent data
Bit 4: ACK (1) - Acknowledgment SET
Bit 3: PSH (0) - No push
Bit 2: RST (0) - No reset
Bit 1: SYN (1) - Synchronize SET
Bit 0: FIN (0) - No finish

Result: SYN-ACK packet (connection handshake)
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum positive input | 1FF (511 decimal) | 1FF (511 decimal) |
| Negative input format | 10-char two's complement hex | 10-char two's complement hex |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Maximum output length | 10 characters | 10 characters |
| Leading zeros with places | Supported | Supported |
| Return type | Text | Text |

Both platforms implement HEX2BIN identically. The limited range (10-bit binary output) means this function is suitable for byte-level analysis but not for large hex values like full 32-bit addresses. For larger values, convert to decimal first with HEX2DEC.

## Tips and Best Practices

1. **Use places=8 for byte analysis**: When examining byte values (00-FF), always use `=HEX2BIN(A1, 8)` to see all 8 bits with proper alignment.

2. **Remember the limited range**: HEX2BIN only handles hex values up to 1FF (511 decimal) for positive numbers. For larger hex values, use HEX2DEC first, then work with the decimal value.

3. **Map hex digits to binary nibbles**: Memorize common mappings: 0=0000, 5=0101, A=1010, F=1111. This helps verify conversions mentally.

4. **Input is case-insensitive**: Both "FF", "ff", and "Ff" produce the same result. Use whichever matches your source data.

5. **Understand two's complement input**: Negative values require 10-character hex input (e.g., FFFFFFFFFF for -1). Shorter hex strings are always treated as positive.

6. **Combine with other conversions**: For hex values larger than 1FF, chain functions: first use HEX2DEC, then work with decimal, or convert nibble by nibble.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BIN2HEX]] | Converts binary to hexadecimal (inverse function) |
| [[HEX2DEC]] | Converts hexadecimal to decimal |
| [[HEX2OCT]] | Converts hexadecimal to octal |
| [[DEC2BIN]] | Converts decimal to binary |
| [[OCT2BIN]] | Converts octal to binary |

### Commonly Used With

- **[[MID]]** - Extract individual hex digits for separate conversion
- **[[BIN2DEC]]** - Convert binary result to decimal for calculations
- **[[CONCAT]]** - Join multiple binary conversions
- **[[LEN]]** - Check binary output length
- **[[RIGHT]]** - Extract specific bits from result

## Official Documentation

- [Microsoft Excel HEX2BIN Documentation](https://support.microsoft.com/en-us/office/hex2bin-function-a13aafaa-5f4d-48b5-89e5-4d82aef5e9c0)
- [Google Sheets HEX2BIN Documentation](https://support.google.com/docs/answer/3093228)

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
