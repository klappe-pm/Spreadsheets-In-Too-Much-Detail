---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
subTopics:
- bitwise-operations
- bit-shift
- binary-division
- digital-systems
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- right-shift
- bit-shift-right
- binary-right-shift
tags:
- engineering-function
- bitwise-operation
- bit-shift
- binary-division
- digital-systems
---

# BITRSHIFT

## Description

BITRSHIFT performs a bitwise right shift operation, moving all bits in a number to the right by a specified number of positions. Each right shift position effectively divides the value by 2 (with integer truncation), as bits move to lower-value positions and the rightmost bits are discarded. This operation is fundamental in digital systems for fast division by powers of 2, extracting fields from packed data, implementing fixed-point arithmetic, and parsing binary protocols.

In binary representation, shifting right moves each bit to a lower-order position. For example, shifting 20 (binary 10100) right by 2 positions gives 5 (binary 101). The original bit 4 (value 16) moves to bit 2 (value 4), bit 2 (value 4) moves to bit 0 (value 1), and the original bits 0 and 1 are discarded. Mathematically, BITRSHIFT(n, k) equals FLOOR(n / 2^k), providing integer division by powers of 2.

Right shifting is essential in many computing scenarios: extracting high-order fields from packed integers (shift right then mask), implementing efficient integer division, parsing multi-byte data structures where fields are stored at various bit offsets, and decoding binary communication protocols. Combined with BITAND for masking, BITRSHIFT enables extraction of any arbitrary bit field from a packed value.

Both Excel and Google Sheets implement BITRSHIFT for non-negative integers up to 2^48 - 1. The function accepts the number to shift and the number of bit positions. Bits shifted off the right edge are lost; this is a logical right shift (not arithmetic), meaning zeros always fill from the left regardless of the original value. Negative shift amounts cause an error; use BITLSHIFT for left shifts.

## Syntax

> [!NOTE] Syntax
> ```
> BITRSHIFT(number, shift_amount)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The non-negative integer to shift. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |
| **shift_amount** | The number of bit positions to shift right. Must be >= 0. Typically 0 to 48. | Yes |

## Examples

| Formula | Result | Binary Explanation |
|---------|--------|-------------------|
| `=BITRSHIFT(16, 0)` | 16 | No shift, value unchanged |
| `=BITRSHIFT(16, 1)` | 8 | 10000 >> 1 = 1000 = 8 |
| `=BITRSHIFT(16, 4)` | 1 | 10000 >> 4 = 1 |
| `=BITRSHIFT(20, 2)` | 5 | 10100 >> 2 = 101 = 5 |
| `=BITRSHIFT(255, 4)` | 15 | 11111111 >> 4 = 00001111 = 15 |
| `=BITRSHIFT(1000, 3)` | 125 | Equivalent to FLOOR(1000/8) |
| `=BITRSHIFT(65535, 8)` | 255 | Extract high byte from 16-bit value |
| `=BITRSHIFT(7, 1)` | 3 | 111 >> 1 = 11 = 3 (bit lost) |
| `=BITRSHIFT(A1, 8)` | A1 / 256 | Equivalent to integer division by 2^8 |
| `=BITRSHIFT(1, 1)` | 0 | Single bit shifted out = 0 |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Negative number input | Ensure number is >= 0 |
| `#NUM!` | Negative shift amount | Use BITLSHIFT for left shifts |
| `#NUM!` | Number >= 2^48 | Keep values within 48-bit range |
| `#VALUE!` | Non-numeric argument | Ensure both arguments are numeric values |
| `#VALUE!` | Text input | Convert text to number using VALUE() |
| Unexpected 0 | Shift amount too large | Shifting by >= number of significant bits yields 0 |

## Use Cases

### 1. Data Unpacking - Extracting Fields from Packed Integers

**Scenario**: A data engineer receives packed sensor data where multiple values are encoded into single integers. Each field occupies specific bit positions and must be extracted for analysis.

**Implementation**: Use BITRSHIFT to move high-order fields to the low-order position, then BITAND to mask off unwanted bits.

```
Packed format (32 bits):
Bits 0-7: Temperature (8 bits, 0-255)
Bits 8-15: Humidity (8 bits, 0-255)
Bits 16-23: Pressure code (8 bits, 0-255)
Bits 24-31: Status flags (8 bits)

Packed value: 2164295680 (hex: 0x81004200)

| Field | Shift | Mask | Formula | Value |
|-------|-------|------|---------|-------|
| Temperature | 0 | 255 | =BITAND(2164295680, 255) | 0 |
| Humidity | 8 | 255 | =BITAND(BITRSHIFT(2164295680, 8), 255) | 66 |
| Pressure | 16 | 255 | =BITAND(BITRSHIFT(2164295680, 16), 255) | 0 |
| Status | 24 | 255 | =BITAND(BITRSHIFT(2164295680, 24), 255) | 129 |

General extraction formula:
=BITAND(BITRSHIFT(packed_value, field_start_bit), (2^field_width)-1)

For 8-bit field starting at bit 8:
=BITAND(BITRSHIFT(value, 8), 255)
```

### 2. Fast Integer Division by Powers of 2

**Scenario**: A programmer needs to perform many integer divisions by powers of 2 and wants to use the computationally efficient bit shift method, which also provides automatic floor behavior.

**Implementation**: Replace division by powers of 2 with equivalent right shift operations, demonstrating binary arithmetic principles.

```
Division equivalents:
n / 2 = BITRSHIFT(n, 1)
n / 4 = BITRSHIFT(n, 2)
n / 8 = BITRSHIFT(n, 3)
n / 16 = BITRSHIFT(n, 4)
n / 256 = BITRSHIFT(n, 8)
n / 1024 = BITRSHIFT(n, 10)

| Original | Divide By | Formula | Result | Verification |
|----------|-----------|---------|--------|--------------|
| 100 | 2 | =BITRSHIFT(100, 1) | 50 | FLOOR(100/2)=50 |
| 100 | 4 | =BITRSHIFT(100, 2) | 25 | FLOOR(100/4)=25 |
| 100 | 8 | =BITRSHIFT(100, 3) | 12 | FLOOR(100/8)=12 |
| 1000 | 16 | =BITRSHIFT(1000, 4) | 62 | FLOOR(1000/16)=62 |
| 1000000 | 1024 | =BITRSHIFT(1000000, 10) | 976 | FLOOR(1000000/1024)=976 |

Note: Result is always FLOOR of division (rounds toward zero)
For n / 2^k, use BITRSHIFT(n, k)
```

### 3. Color Component Extraction from RGB Values

**Scenario**: A web developer has RGB color values stored as single integers (common in graphics programming) and needs to extract individual red, green, and blue components for color analysis or manipulation.

**Implementation**: Use BITRSHIFT and BITAND to extract each 8-bit color channel from a 24-bit RGB value.

```
RGB format (24-bit):
Bits 0-7: Blue component (0-255)
Bits 8-15: Green component (0-255)
Bits 16-23: Red component (0-255)

Example colors:
| Color | RGB Value | Red | Green | Blue |
|-------|-----------|-----|-------|------|
| White | 16777215 | =BITAND(BITRSHIFT(B2,16),255)=255 | =BITAND(BITRSHIFT(B2,8),255)=255 | =BITAND(B2,255)=255 |
| Red | 16711680 | =BITAND(BITRSHIFT(B3,16),255)=255 | =BITAND(BITRSHIFT(B3,8),255)=0 | =BITAND(B3,255)=0 |
| Cyan | 65535 | =BITAND(BITRSHIFT(B4,16),255)=0 | =BITAND(BITRSHIFT(B4,8),255)=255 | =BITAND(B4,255)=255 |
| Purple | 8388736 | =BITAND(BITRSHIFT(B5,16),255)=128 | =BITAND(BITRSHIFT(B5,8),255)=0 | =BITAND(B5,255)=128 |

General formulas:
Red: =BITAND(BITRSHIFT(rgb, 16), 255)
Green: =BITAND(BITRSHIFT(rgb, 8), 255)
Blue: =BITAND(rgb, 255)

Luminance calculation:
=0.299*BITAND(BITRSHIFT(rgb,16),255) + 0.587*BITAND(BITRSHIFT(rgb,8),255) + 0.114*BITAND(rgb,255)
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2013+ | Yes |
| Maximum input value | 2^48 - 1 | 2^48 - 1 |
| Maximum shift amount | 48 | 48 |
| Shift type | Logical (zero-fill) | Logical (zero-fill) |
| Negative shift | Error | Error |
| Return type | Integer | Integer |

Both platforms implement BITRSHIFT identically as a logical right shift (zeros fill from left). The function was added to Excel in version 2013.

## Tips and Best Practices

1. **Equivalent to integer division**: BITRSHIFT(n, k) equals FLOOR(n / 2^k), providing fast integer division by powers of 2.

2. **Extract high bytes**: To get the high byte of a 16-bit value, use `=BITRSHIFT(value, 8)`. For 32-bit, use shifts of 24, 16, 8 for successive bytes.

3. **Combine with BITAND**: The pattern `=BITAND(BITRSHIFT(packed, offset), mask)` extracts any field from a packed integer.

4. **Lost bits are gone**: Unlike division, right shift discards bits completely. BITRSHIFT(7, 1) = 3 (the rightmost 1 is lost).

5. **Zero-fill behavior**: BITRSHIFT always fills with zeros from the left. For signed numbers requiring sign extension, manual handling is needed.

6. **Reverse of BITLSHIFT**: BITRSHIFT(BITLSHIFT(n, k), k) = n only if no bits were lost during the left shift.

7. **Field extraction formula**: For a field of width w bits starting at position p: `=BITAND(BITRSHIFT(value, p), 2^w - 1)`.

8. **Use for byte access**: BITRSHIFT(value, 8*n) moves byte n to the lowest position for extraction.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BITLSHIFT]] | Shift bits left (multiply by powers of 2) |
| [[BITAND]] | AND operation for masking after shifting |
| [[BITOR]] | OR operation for combining values |

### Commonly Used With

- **[[BITLSHIFT]]** - Reverse operation; shift left for packing or multiplication
- **[[BITAND]]** - Mask bits after shifting to extract specific fields
- **[[BITOR]]** - Combine extracted/modified fields back into packed values
- **[[DEC2BIN]]** - Visualize shift results in binary
- **[[HEX2DEC]]** / **[[DEC2HEX]]** - Work with hexadecimal representations
- **[[FLOOR]]** - Equivalent mathematical operation (n/2^k)
- **[[MOD]]** - Extract low bits (alternative to shift-and-mask)

## Official Documentation

- [Microsoft Excel BITRSHIFT Documentation](https://support.microsoft.com/en-us/office/bitrshift-function-274d6996-f42c-4743-abdb-4ff95351222c)
- [Google Sheets BITRSHIFT Documentation](https://support.google.com/docs/answer/9061544)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Function introduced |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2013 | Available since introduction |
