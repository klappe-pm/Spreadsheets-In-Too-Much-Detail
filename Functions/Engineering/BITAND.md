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
- binary-logic
- bit-manipulation
- digital-systems
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- bitwise-and
- binary-and
- bit-and
tags:
- engineering-function
- bitwise-operation
- binary-logic
- digital-systems
- bit-manipulation
---

# BITAND

## Description

BITAND performs a bitwise AND operation on two non-negative integers, comparing each bit position and returning 1 only when both corresponding bits are 1. The bitwise AND is a fundamental operation in digital logic and computer science, used for bit masking, flag testing, permission checking, and extracting specific bits from binary data. BITAND enables spreadsheet users to perform low-level binary operations typically found in programming languages.

In binary representation, each number is expressed as a sequence of bits (0s and 1s). The AND operation compares bits at each position: the result bit is 1 only if both input bits at that position are 1; otherwise, it's 0. For example, 12 AND 10 in binary is 1100 AND 1010 = 1000, which equals 8 in decimal. This behavior makes BITAND perfect for "masking" operations where you want to extract or test specific bits while zeroing out others.

The bitwise AND operation is essential in many computing scenarios: extracting fields from packed data structures, testing whether specific flags are set in status registers, implementing permission systems where each bit represents a different access right, and creating bit masks for hardware interface programming. In network programming, BITAND is used to apply subnet masks to IP addresses. In graphics, it's used for alpha channel operations and color masking.

Both Excel and Google Sheets implement BITAND for non-negative integers up to 2^48 - 1 (281,474,976,710,655). The function accepts decimal integers and internally converts them to binary for the operation. Negative numbers and non-integers cause errors. The result is always less than or equal to the smaller of the two inputs, since AND can only preserve or clear bits, never set new ones.

## Syntax

> [!NOTE] Syntax
> ```
> BITAND(number1, number2)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number1** | The first non-negative integer. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |
| **number2** | The second non-negative integer. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |

## Binary AND Truth Table

| Bit A | Bit B | A AND B |
|-------|-------|---------|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

## Examples

| Formula | Result | Binary Explanation |
|---------|--------|-------------------|
| `=BITAND(12, 10)` | 8 | 1100 AND 1010 = 1000 |
| `=BITAND(15, 7)` | 7 | 1111 AND 0111 = 0111 |
| `=BITAND(255, 170)` | 170 | 11111111 AND 10101010 = 10101010 |
| `=BITAND(255, 15)` | 15 | 11111111 AND 00001111 = 00001111 (low nibble mask) |
| `=BITAND(240, 255)` | 240 | 11110000 AND 11111111 = 11110000 (high nibble preserved) |
| `=BITAND(0, 255)` | 0 | 0 AND anything = 0 |
| `=BITAND(255, 255)` | 255 | Identical values return themselves |
| `=BITAND(7, 4)` | 4 | 111 AND 100 = 100 (test if bit 2 is set) |
| `=BITAND(5, 4)` | 4 | 101 AND 100 = 100 (bit 2 is set) |
| `=BITAND(3, 4)` | 0 | 011 AND 100 = 000 (bit 2 is not set) |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Negative number input | Ensure both arguments are >= 0 |
| `#NUM!` | Number >= 2^48 | Keep values below 281,474,976,710,655 |
| `#VALUE!` | Non-numeric argument | Ensure both arguments are numeric values |
| `#VALUE!` | Text input | Convert text to number using VALUE() function |
| Unexpected result | Decimal truncation | Be aware that decimals are truncated; use INT() or TRUNC() explicitly |

## Use Cases

### 1. Permission System - Access Control Checking

**Scenario**: A system administrator manages user permissions using a bit-field where each bit represents a different access right. They need to check if users have specific permissions before allowing operations.

**Implementation**: Use BITAND to test if specific permission bits are set in a user's permission value.

```
Permission bits:
Bit 0 (1): Read
Bit 1 (2): Write
Bit 2 (4): Execute
Bit 3 (8): Delete
Bit 4 (16): Admin

| User | Permission Value | Test | Formula | Has Permission? |
|------|------------------|------|---------|-----------------|
| Alice | 7 (Read+Write+Exec) | Write (2) | =BITAND(B2, 2)>0 | TRUE |
| Bob | 5 (Read+Exec) | Write (2) | =BITAND(B3, 2)>0 | FALSE |
| Carol | 31 (All) | Admin (16) | =BITAND(B4, 16)>0 | TRUE |
| Dave | 15 | Admin (16) | =BITAND(B5, 16)>0 | FALSE |

Check multiple permissions at once:
=BITAND(B2, 3) = 3 means has both Read AND Write
=BITAND(B2, 6) = 6 means has both Write AND Execute
```

### 2. Network Addressing - Subnet Mask Application

**Scenario**: A network engineer needs to apply subnet masks to IP addresses to determine network addresses, essential for routing and network segmentation analysis.

**Implementation**: Convert IP address octets to decimal, apply BITAND with mask, then reconstruct the network address.

```
IP Address: 192.168.45.100
Subnet Mask: 255.255.255.0 (/24)

| Octet | IP Value | Mask Value | Formula | Network Address |
|-------|----------|------------|---------|-----------------|
| 1st   | 192      | 255        | =BITAND(192, 255) | 192 |
| 2nd   | 168      | 255        | =BITAND(168, 255) | 168 |
| 3rd   | 45       | 255        | =BITAND(45, 255) | 45 |
| 4th   | 100      | 0          | =BITAND(100, 0) | 0 |

Network Address: 192.168.45.0

For /22 subnet (255.255.252.0):
| Octet | IP Value | Mask Value | Formula | Network Address |
|-------|----------|------------|---------|-----------------|
| 3rd   | 45       | 252        | =BITAND(45, 252) | 44 |

Network Address: 192.168.44.0
```

### 3. Data Extraction - Parsing Packed Binary Fields

**Scenario**: An embedded systems engineer receives sensor data where multiple values are packed into single integers (common in IoT and industrial protocols). They need to extract individual fields from the packed data.

**Implementation**: Use BITAND with appropriate masks to extract specific bit fields from packed data.

```
Packed data format (16-bit):
Bits 0-3: Sensor ID (0-15)
Bits 4-7: Status code (0-15)
Bits 8-11: Error flags (0-15)
Bits 12-15: Reserved

Raw value: 2748 (binary: 0000101010111100)

| Field | Mask | Shift | Formula | Value |
|-------|------|-------|---------|-------|
| Sensor ID | 15 (0x0F) | 0 | =BITAND(2748, 15) | 12 |
| Status | 240 (0xF0) | 4 | =BITAND(2748, 240)/16 | 11 |
| Errors | 3840 (0xF00) | 8 | =BITAND(2748, 3840)/256 | 10 |
| Reserved | 61440 (0xF000) | 12 | =BITAND(2748, 61440)/4096 | 0 |

Alternative using BITRSHIFT for shifting:
Status: =BITRSHIFT(BITAND(2748, 240), 4) → 11
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2013+ | Yes |
| Maximum value | 2^48 - 1 | 2^48 - 1 |
| Negative numbers | Error | Error |
| Decimal handling | Truncated | Truncated |
| Return type | Integer | Integer |

Both platforms implement BITAND identically. The function was added to Excel in version 2013; earlier versions require VBA or workarounds.

## Tips and Best Practices

1. **Create named masks**: Define named ranges for common bit masks (e.g., MASK_READ = 1, MASK_WRITE = 2) to make formulas self-documenting.

2. **Test for bit presence**: To check if bit n is set, use `=BITAND(value, 2^n) > 0` or `=BITAND(value, 2^n) = 2^n`.

3. **Extract low bits**: Use `=BITAND(value, 2^n - 1)` to extract the lowest n bits of a value.

4. **Clear specific bits**: To clear bit n, use `=BITAND(value, BITNOT(2^n))` or in Excel: `=BITAND(value, 2^48-1-2^n)`.

5. **Combine with BITRSHIFT**: Extract mid-range bits by masking then shifting: `=BITRSHIFT(BITAND(value, mask), shift_amount)`.

6. **Document bit positions**: Always document what each bit position represents; bit manipulation code is notoriously hard to read without documentation.

7. **Use hex for clarity**: While Excel doesn't directly accept hex, convert using HEX2DEC: `=BITAND(value, HEX2DEC("FF"))` for mask 255.

8. **Verify with DEC2BIN**: Use `=DEC2BIN(result)` to visually verify bitwise operation results during development.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BITOR]] | Bitwise OR: result bit is 1 if either input bit is 1 |
| [[BITXOR]] | Bitwise XOR: result bit is 1 if input bits differ |
| [[BITNOT]] | Bitwise NOT: inverts all bits (Excel has no BITNOT; use XOR with all 1s) |

### Commonly Used With

- **[[BITOR]]** - Combine flags using OR after testing with AND
- **[[BITXOR]]** - Toggle bits or detect differences
- **[[BITLSHIFT]]** - Shift bits left before or after AND operations
- **[[BITRSHIFT]]** - Shift bits right for extracting high-order fields
- **[[DEC2BIN]]** - Convert results to binary for visual verification
- **[[HEX2DEC]]** - Convert hexadecimal masks to decimal for BITAND
- **[[POWER]]** - Calculate bit position values (2^n)

## Official Documentation

- [Microsoft Excel BITAND Documentation](https://support.microsoft.com/en-us/office/bitand-function-8a2be3d7-91c3-4b48-9517-64571571b2a0)
- [Google Sheets BITAND Documentation](https://support.google.com/docs/answer/9061440)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Function introduced |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2013 | Available since introduction |
