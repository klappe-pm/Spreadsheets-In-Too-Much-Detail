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
- bitwise-xor
- binary-xor
- exclusive-or
tags:
- engineering-function
- bitwise-operation
- binary-logic
- digital-systems
- bit-manipulation
---

# BITXOR

## Description

BITXOR performs a bitwise exclusive OR (XOR) operation on two non-negative integers, comparing each bit position and returning 1 only when the corresponding bits differ. The XOR operation is a fundamental building block in digital logic, cryptography, error detection, and computer science, used for toggling bits, detecting changes, simple encryption, checksum calculations, and swap operations without temporary variables.

In binary representation, each number is expressed as a sequence of bits (0s and 1s). The XOR operation compares bits at each position: the result bit is 1 if the input bits are different (one is 0 and the other is 1); the result is 0 if both bits are the same (both 0 or both 1). For example, 12 XOR 10 in binary is 1100 XOR 1010 = 0110, which equals 6 in decimal. This "difference detector" behavior gives XOR unique properties not shared by AND or OR.

The XOR operation has remarkable mathematical properties: it's commutative (A XOR B = B XOR A), associative ((A XOR B) XOR C = A XOR (B XOR C)), self-inverse (A XOR A = 0), and has identity (A XOR 0 = A). These properties make XOR essential for parity calculations, toggle operations, encryption/decryption, comparing values for differences, and implementing linear feedback shift registers. In error detection, XOR is the basis for parity bits and CRC calculations.

Both Excel and Google Sheets implement BITXOR for non-negative integers up to 2^48 - 1 (281,474,976,710,655). The function accepts decimal integers and internally converts them to binary for the operation. Negative numbers and non-integers cause errors. Unlike AND and OR, XOR doesn't have a predictable relationship between input and output magnitudes; the result depends entirely on which bits differ.

## Syntax

> [!NOTE] Syntax
> ```
> BITXOR(number1, number2)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number1** | The first non-negative integer. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |
| **number2** | The second non-negative integer. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |

## Binary XOR Truth Table

| Bit A | Bit B | A XOR B |
|-------|-------|---------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

## Examples

| Formula | Result | Binary Explanation |
|---------|--------|-------------------|
| `=BITXOR(12, 10)` | 6 | 1100 XOR 1010 = 0110 |
| `=BITXOR(5, 3)` | 6 | 101 XOR 011 = 110 |
| `=BITXOR(255, 0)` | 255 | XOR with 0 returns original (identity) |
| `=BITXOR(170, 170)` | 0 | XOR with itself = 0 (self-inverse) |
| `=BITXOR(255, 255)` | 0 | Same value XOR = 0 |
| `=BITXOR(240, 15)` | 255 | 11110000 XOR 00001111 = 11111111 |
| `=BITXOR(7, 4)` | 3 | 111 XOR 100 = 011 (toggle bit 2) |
| `=BITXOR(5, 255)` | 250 | 00000101 XOR 11111111 = 11111010 (invert) |
| `=BITXOR(A, BITXOR(A, B))` | B | XOR is reversible: (A XOR A) XOR B = B |
| `=BITXOR(BITXOR(65, key), key)` | 65 | Encrypt then decrypt returns original |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Negative number input | Ensure both arguments are >= 0 |
| `#NUM!` | Number >= 2^48 | Keep values below 281,474,976,710,655 |
| `#VALUE!` | Non-numeric argument | Ensure both arguments are numeric values |
| `#VALUE!` | Text input | Convert text to number using VALUE() function |
| Unexpected result | Decimal truncation | Be aware that decimals are truncated; use INT() explicitly |

## Use Cases

### 1. Simple Data Encryption/Obfuscation

**Scenario**: A developer needs to implement simple, reversible obfuscation of numeric data using XOR encryption. While not cryptographically secure, XOR encryption is useful for basic data scrambling where the same key encrypts and decrypts.

**Implementation**: Use BITXOR with a secret key to encrypt data, then BITXOR with the same key to decrypt. The XOR operation is its own inverse.

```
Secret key: 12345

| Original | Encrypt Formula | Encrypted | Decrypt Formula | Recovered |
|----------|-----------------|-----------|-----------------|-----------|
| 100      | =BITXOR(100, 12345) | 12301 | =BITXOR(12301, 12345) | 100 |
| 255      | =BITXOR(255, 12345) | 12438 | =BITXOR(12438, 12345) | 255 |
| 1000     | =BITXOR(1000, 12345) | 13337 | =BITXOR(13337, 12345) | 1000 |
| 50000    | =BITXOR(50000, 12345) | 62313 | =BITXOR(62313, 12345) | 50000 |

Property: BITXOR(BITXOR(value, key), key) = value
This works because: (value XOR key) XOR key = value XOR (key XOR key) = value XOR 0 = value

Note: This is NOT secure encryption; use proper cryptography for sensitive data
```

### 2. Change Detection - Finding Differences

**Scenario**: A system administrator needs to compare two configuration values bit by bit to identify exactly which settings have changed between versions, useful for auditing and troubleshooting.

**Implementation**: Use BITXOR to highlight differences between old and new values; any bit that's 1 in the result represents a changed setting.

```
| Config | Old Value | New Value | XOR Result | Changed Bits |
|--------|-----------|-----------|------------|--------------|
| A | 135 | 143 | =BITXOR(135, 143) = 8 | Bit 3 changed |
| B | 255 | 255 | =BITXOR(255, 255) = 0 | No changes |
| C | 100 | 110 | =BITXOR(100, 110) = 10 | Bits 1, 3 changed |
| D | 0 | 15 | =BITXOR(0, 15) = 15 | Bits 0-3 changed |

Analysis of Config C (100 vs 110):
Binary: 01100100 XOR 01101110 = 00001010
Changed bits: bit 1 (value 2) and bit 3 (value 8)

Count changes: Count 1-bits in XOR result
=LEN(SUBSTITUTE(DEC2BIN(BITXOR(old, new)), "0", "")) gives number of changed bits
```

### 3. Parity Calculation for Error Detection

**Scenario**: A communications engineer needs to calculate parity bits for data transmission, where a parity bit is set such that the total number of 1-bits is even (even parity) or odd (odd parity).

**Implementation**: Use BITXOR to combine all data bytes; the result's least significant bit gives the parity of all the data.

```
XOR parity: XOR of all bytes gives a "summary" byte
If all bytes XOR to 0, the data has even parity across all bits

| Byte | Value | Running XOR | Formula |
|------|-------|-------------|---------|
| B1   | 85    | 85          | =85 |
| B2   | 170   | 255         | =BITXOR(85, 170) |
| B3   | 51    | 204         | =BITXOR(255, 51) |
| B4   | 204   | 0           | =BITXOR(204, 204) |

Final XOR = 0 means the 4 bytes have complementary bit patterns

For simple parity bit (even parity):
Data bytes: 7, 14, 21 (binary: 0111, 1110, 10101)
XOR: =BITXOR(BITXOR(7, 14), 21) = 16 (binary: 10000)
Parity of all bits: =MOD(16 + other bits, 2) for LSB parity

Checksum verification: If XOR of data + checksum = 0, no errors detected
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2013+ | Yes |
| Maximum value | 2^48 - 1 | 2^48 - 1 |
| Negative numbers | Error | Error |
| Decimal handling | Truncated | Truncated |
| Return type | Integer | Integer |

Both platforms implement BITXOR identically. The function was added to Excel in version 2013.

## Tips and Best Practices

1. **Toggle specific bits**: To flip bit n, use `=BITXOR(value, 2^n)`. This turns the bit on if off, or off if on.

2. **Create NOT operation**: XOR with all 1s inverts bits: `=BITXOR(value, 255)` inverts the low 8 bits (bitwise NOT for 8 bits).

3. **Swap without temp variable**: A = A XOR B, B = A XOR B, A = A XOR B swaps A and B (programming trick, less relevant in spreadsheets).

4. **Detect any difference**: If BITXOR(a, b) = 0, then a = b. Non-zero means the values differ somewhere.

5. **Reversible encryption**: XOR encryption/decryption uses the same key: `=BITXOR(BITXOR(data, key), key)` returns the original data.

6. **Parity checking**: XOR all bytes together; the result helps detect single-bit errors in the original data.

7. **Combine for checksums**: XOR is fast and simple for basic checksums, though CRC or cryptographic hashes are better for security.

8. **Visualize with DEC2BIN**: Use `=DEC2BIN(BITXOR(a, b))` to see exactly which bits differ between a and b.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BITAND]] | Bitwise AND: result bit is 1 only if both input bits are 1 |
| [[BITOR]] | Bitwise OR: result bit is 1 if either input bit is 1 |
| [[BITNOT]] | Bitwise NOT: inverts bits (simulate with XOR against all 1s) |

### Commonly Used With

- **[[BITAND]]** - Mask bits before or after XOR operations
- **[[BITOR]]** - Combine flags (different from XOR's toggle behavior)
- **[[BITLSHIFT]]** - Shift values for bit manipulation
- **[[BITRSHIFT]]** - Shift for field extraction
- **[[DEC2BIN]]** - Visualize XOR results in binary
- **[[HEX2DEC]]** - Convert hex masks for XOR operations
- **[[POWER]]** - Calculate bit masks (2^n) for toggling specific bits

## Official Documentation

- [Microsoft Excel BITXOR Documentation](https://support.microsoft.com/en-us/office/bitxor-function-c81306a1-03f9-4e89-85ac-b86c3cba10e4)
- [Google Sheets BITXOR Documentation](https://support.google.com/docs/answer/9061463)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Function introduced |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2013 | Available since introduction |
