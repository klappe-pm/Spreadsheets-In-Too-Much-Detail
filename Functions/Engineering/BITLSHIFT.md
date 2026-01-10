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
- binary-multiplication
- digital-systems
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- left-shift
- bit-shift-left
- binary-left-shift
tags:
- engineering-function
- bitwise-operation
- bit-shift
- binary-multiplication
- digital-systems
---

# BITLSHIFT

## Description

BITLSHIFT performs a bitwise left shift operation, moving all bits in a number to the left by a specified number of positions. Each left shift position effectively multiplies the value by 2, as bits move to higher-value positions and zeros fill in from the right. This operation is fundamental in digital systems for fast multiplication by powers of 2, positioning data within bit fields, creating masks, and implementing efficient arithmetic in binary-based systems.

In binary representation, shifting left moves each bit to a higher-order position. For example, shifting 5 (binary 101) left by 2 positions gives 20 (binary 10100). The original bit 0 (value 1) moves to bit 2 (value 4), bit 2 (value 4) moves to bit 4 (value 16), giving 4 + 16 = 20. Mathematically, BITLSHIFT(n, k) equals n * 2^k, but the bit shift operation is how computers implement fast multiplication by powers of 2.

Left shifting is essential in many computing scenarios: packing multiple values into a single integer (shift each field to its position before combining with OR), creating bit masks at specific positions, implementing fixed-point arithmetic, converting between different numeric representations, and optimizing calculations that involve powers of 2. In embedded systems and hardware interfaces, left shift prepares data for specific register positions.

Both Excel and Google Sheets implement BITLSHIFT for non-negative integers up to 2^48 - 1. The function accepts the number to shift and the number of bit positions. If shifted bits would exceed the 48-bit limit, they are lost (overflow). Negative shift amounts cause an error; use BITRSHIFT for right shifts. The shift amount must also be non-negative and reasonable (typically 0 to 48).

## Syntax

> [!NOTE] Syntax
> ```
> BITLSHIFT(number, shift_amount)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The non-negative integer to shift. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |
| **shift_amount** | The number of bit positions to shift left. Must be >= 0. Typically 0 to 48. | Yes |

## Examples

| Formula | Result | Binary Explanation |
|---------|--------|-------------------|
| `=BITLSHIFT(1, 0)` | 1 | No shift, value unchanged |
| `=BITLSHIFT(1, 1)` | 2 | 1 << 1 = 10 binary = 2 |
| `=BITLSHIFT(1, 4)` | 16 | 1 << 4 = 10000 binary = 16 |
| `=BITLSHIFT(5, 2)` | 20 | 101 << 2 = 10100 = 20 |
| `=BITLSHIFT(3, 3)` | 24 | 11 << 3 = 11000 = 24 |
| `=BITLSHIFT(255, 8)` | 65280 | Shift byte to high position |
| `=BITLSHIFT(1, 10)` | 1024 | 2^10 = 1024 |
| `=BITLSHIFT(7, 4)` | 112 | 0111 << 4 = 01110000 = 112 |
| `=BITLSHIFT(A1, 8)` | A1 * 256 | Equivalent to multiplying by 2^8 |
| `=BITLSHIFT(1, 47)` | 2^47 | Maximum single-bit position |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Negative number input | Ensure number is >= 0 |
| `#NUM!` | Negative shift amount | Use BITRSHIFT for right shifts |
| `#NUM!` | Number >= 2^48 | Keep values within 48-bit range |
| `#NUM!` | Result would exceed 2^48 | Reduce shift amount or use smaller input |
| `#VALUE!` | Non-numeric argument | Ensure both arguments are numeric values |
| `#VALUE!` | Text input | Convert text to number using VALUE() |

## Use Cases

### 1. Data Packing - Creating Multi-Field Integers

**Scenario**: A data engineer needs to pack multiple small values into a single 32-bit or 48-bit integer for efficient storage or network transmission, where each value occupies a specific bit field.

**Implementation**: Use BITLSHIFT to position each field value at its correct bit offset, then combine with BITOR.

```
Packed format (32 bits):
Bits 0-7: Field A (8 bits, 0-255)
Bits 8-15: Field B (8 bits, 0-255)
Bits 16-23: Field C (8 bits, 0-255)
Bits 24-31: Field D (8 bits, 0-255)

| Field | Value | Shift | Formula | Shifted Value |
|-------|-------|-------|---------|---------------|
| A | 65 | 0 | =BITLSHIFT(65, 0) | 65 |
| B | 66 | 8 | =BITLSHIFT(66, 8) | 16896 |
| C | 67 | 16 | =BITLSHIFT(67, 16) | 4390912 |
| D | 68 | 24 | =BITLSHIFT(68, 24) | 1140850688 |

Combined: =BITOR(BITOR(BITOR(65, 16896), 4390912), 1140850688)
Result: 1145324865

Verify: DEC2HEX(1145324865) = "44434241"
Reading hex right-to-left: 41='A', 42='B', 43='C', 44='D'
```

### 2. Creating Bit Masks at Specific Positions

**Scenario**: A hardware engineer needs to create bit masks for testing or setting specific bits in control registers, where the mask position varies based on configuration parameters.

**Implementation**: Use BITLSHIFT to create single-bit masks or multi-bit field masks at runtime-determined positions.

```
| Mask Type | Bit Position | Formula | Mask Value | Binary |
|-----------|--------------|---------|------------|--------|
| Single bit | 0 | =BITLSHIFT(1, 0) | 1 | 00000001 |
| Single bit | 3 | =BITLSHIFT(1, 3) | 8 | 00001000 |
| Single bit | 7 | =BITLSHIFT(1, 7) | 128 | 10000000 |
| 4-bit field | 0 | =BITLSHIFT(15, 0) | 15 | 00001111 |
| 4-bit field | 4 | =BITLSHIFT(15, 4) | 240 | 11110000 |
| 8-bit field | 8 | =BITLSHIFT(255, 8) | 65280 | 1111111100000000 |

Dynamic mask creation:
Single bit at position n: =BITLSHIFT(1, n)
n-bit field starting at position p: =BITLSHIFT((2^n)-1, p)
Example: 5-bit field at position 10: =BITLSHIFT(31, 10) = 31744
```

### 3. Fast Multiplication by Powers of 2

**Scenario**: A financial analyst needs to perform many multiplications by powers of 2 (such as doubling, quadrupling, or scaling by 1024) and wants to use the computationally efficient bit shift method.

**Implementation**: Replace multiplication by powers of 2 with equivalent left shift operations for clarity and to demonstrate binary arithmetic principles.

```
Multiplication equivalents:
n * 2 = BITLSHIFT(n, 1)
n * 4 = BITLSHIFT(n, 2)
n * 8 = BITLSHIFT(n, 3)
n * 16 = BITLSHIFT(n, 4)
n * 256 = BITLSHIFT(n, 8)
n * 1024 = BITLSHIFT(n, 10)

| Original | Multiply By | Formula | Result | Verification |
|----------|-------------|---------|--------|--------------|
| 100 | 2 | =BITLSHIFT(100, 1) | 200 | 100*2=200 |
| 100 | 4 | =BITLSHIFT(100, 2) | 400 | 100*4=400 |
| 100 | 8 | =BITLSHIFT(100, 3) | 800 | 100*8=800 |
| 25 | 16 | =BITLSHIFT(25, 4) | 400 | 25*16=400 |
| 5 | 1024 | =BITLSHIFT(5, 10) | 5120 | 5*1024=5120 |

For n * 2^k, use BITLSHIFT(n, k)
General formula: =BITLSHIFT(n, LOG(multiplier, 2)) when multiplier is power of 2
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2013+ | Yes |
| Maximum input value | 2^48 - 1 | 2^48 - 1 |
| Maximum shift amount | 48 | 48 |
| Overflow behavior | Bits lost | Bits lost |
| Negative shift | Error | Error |
| Return type | Integer | Integer |

Both platforms implement BITLSHIFT identically. The function was added to Excel in version 2013.

## Tips and Best Practices

1. **Equivalent to multiplication**: BITLSHIFT(n, k) equals n * 2^k, but shifting is how computers optimize this calculation internally.

2. **Create powers of 2**: BITLSHIFT(1, n) gives 2^n directly, useful for creating bit masks.

3. **Beware of overflow**: If bits shift beyond position 47 (the 48th bit), they are lost. Check that inputs won't overflow.

4. **Combine with BITOR for packing**: The pattern `BITOR(BITLSHIFT(value, offset), existing)` adds a field to a packed integer.

5. **Position calculation**: To place a value in bits n through m, shift by n: `BITLSHIFT(value, n)`.

6. **Test before production**: Verify shift operations with DEC2BIN to ensure bits land where expected.

7. **Document bit positions**: Always document what each bit position represents in packed data formats.

8. **Use named constants**: Define FIELD_A_SHIFT = 0, FIELD_B_SHIFT = 8, etc., to make formulas self-documenting.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BITRSHIFT]] | Shift bits right (divide by powers of 2) |
| [[BITAND]] | AND operation often used after shifting to mask bits |
| [[BITOR]] | OR operation used to combine shifted values |

### Commonly Used With

- **[[BITRSHIFT]]** - Reverse operation; shift right to extract or divide
- **[[BITOR]]** - Combine shifted fields into packed integers
- **[[BITAND]]** - Mask specific bits after shifting
- **[[DEC2BIN]]** - Visualize shift results in binary
- **[[POWER]]** - Calculate equivalent multiplication (2^n)
- **[[HEX2DEC]]** / **[[DEC2HEX]]** - Work with hexadecimal representations

## Official Documentation

- [Microsoft Excel BITLSHIFT Documentation](https://support.microsoft.com/en-us/office/bitlshift-function-c55f27cf-4478-414c-b1c4-8b21a5d06a25)
- [Google Sheets BITLSHIFT Documentation](https://support.google.com/docs/answer/9061506)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Function introduced |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2013 | Available since introduction |
