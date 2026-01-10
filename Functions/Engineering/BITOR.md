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
- bitwise-or
- binary-or
- bit-or
tags:
- engineering-function
- bitwise-operation
- binary-logic
- digital-systems
- bit-manipulation
---

# BITOR

## Description

BITOR performs a bitwise OR operation on two non-negative integers, comparing each bit position and returning 1 when either or both corresponding bits are 1. The bitwise OR is a fundamental operation in digital logic and computer science, used for setting flags, combining permissions, merging bit fields, and creating composite values from individual components. BITOR enables spreadsheet users to perform low-level binary operations essential for working with digital systems and binary protocols.

In binary representation, each number is expressed as a sequence of bits (0s and 1s). The OR operation compares bits at each position: the result bit is 1 if either input bit (or both) at that position is 1; it's 0 only when both input bits are 0. For example, 12 OR 10 in binary is 1100 OR 1010 = 1110, which equals 14 in decimal. This behavior makes BITOR perfect for combining values, adding flags, and creating union sets at the bit level.

The bitwise OR operation is essential in many computing scenarios: setting specific flags in status registers without affecting others, combining multiple permission bits into a single value, merging separate bit fields into packed data structures, and implementing set union operations on bit vectors. In network programming, BITOR can construct broadcast addresses. In embedded systems, it's used to set control bits while preserving other register values.

Both Excel and Google Sheets implement BITOR for non-negative integers up to 2^48 - 1 (281,474,976,710,655). The function accepts decimal integers and internally converts them to binary for the operation. Negative numbers and non-integers cause errors. The result is always greater than or equal to the larger of the two inputs, since OR can only set bits, never clear them.

## Syntax

> [!NOTE] Syntax
> ```
> BITOR(number1, number2)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number1** | The first non-negative integer. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |
| **number2** | The second non-negative integer. Must be >= 0 and < 2^48. Decimals are truncated to integers. | Yes |

## Binary OR Truth Table

| Bit A | Bit B | A OR B |
|-------|-------|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

## Examples

| Formula | Result | Binary Explanation |
|---------|--------|-------------------|
| `=BITOR(12, 10)` | 14 | 1100 OR 1010 = 1110 |
| `=BITOR(8, 7)` | 15 | 1000 OR 0111 = 1111 |
| `=BITOR(240, 15)` | 255 | 11110000 OR 00001111 = 11111111 |
| `=BITOR(0, 255)` | 255 | 0 OR anything = that value |
| `=BITOR(128, 64)` | 192 | 10000000 OR 01000000 = 11000000 |
| `=BITOR(1, 2)` | 3 | 01 OR 10 = 11 |
| `=BITOR(255, 255)` | 255 | Identical values return themselves |
| `=BITOR(5, 4)` | 5 | 101 OR 100 = 101 (set bit 2, already set) |
| `=BITOR(3, 4)` | 7 | 011 OR 100 = 111 (add bit 2) |
| `=BITOR(0, 0)` | 0 | Only case where OR returns 0 |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Negative number input | Ensure both arguments are >= 0 |
| `#NUM!` | Number >= 2^48 | Keep values below 281,474,976,710,655 |
| `#VALUE!` | Non-numeric argument | Ensure both arguments are numeric values |
| `#VALUE!` | Text input | Convert text to number using VALUE() function |
| Unexpected result | Decimal truncation | Be aware that decimals are truncated; use INT() or TRUNC() explicitly |

## Use Cases

### 1. Permission System - Granting Access Rights

**Scenario**: A system administrator needs to grant additional permissions to users by combining new access rights with existing ones, without accidentally removing any current permissions.

**Implementation**: Use BITOR to add new permission bits to existing permission values, ensuring that current rights are preserved while new ones are added.

```
Permission bits:
Bit 0 (1): Read
Bit 1 (2): Write
Bit 2 (4): Execute
Bit 3 (8): Delete
Bit 4 (16): Admin

| User | Current Permissions | Add Permission | Formula | New Permissions |
|------|---------------------|----------------|---------|-----------------|
| Alice | 1 (Read) | 2 (Write) | =BITOR(B2, C2) | 3 (Read+Write) |
| Bob | 5 (Read+Exec) | 2 (Write) | =BITOR(B3, C3) | 7 (Read+Write+Exec) |
| Carol | 3 (Read+Write) | 4 (Execute) | =BITOR(B4, C4) | 7 (Read+Write+Exec) |
| Dave | 7 | 8 (Delete) | =BITOR(B5, C5) | 15 (All except Admin) |

Grant multiple permissions at once:
=BITOR(BITOR(current, 2), 4) adds Write AND Execute
Or: =BITOR(current, BITOR(2, 4)) = BITOR(current, 6)
```

### 2. Status Flag Aggregation

**Scenario**: A monitoring system tracks multiple error conditions using individual flag bits. An operator needs to combine status flags from different subsystems into a single aggregate status value for dashboard display.

**Implementation**: Use BITOR to merge status flags from multiple sources into a comprehensive status word.

```
Status bits:
Bit 0 (1): System A error
Bit 1 (2): System B error
Bit 2 (4): Network error
Bit 3 (8): Disk error
Bit 4 (16): Memory warning
Bit 5 (32): CPU warning

| Subsystem | Status Value | Combined Status |
|-----------|--------------|-----------------|
| Controller | 1 | =BITOR(B2, BITOR(B3, B4)) |
| Network | 4 | ↓ |
| Storage | 8 | ↓ |
| Combined | - | 13 (System A + Network + Disk) |

Hierarchical combination:
Level 1: =BITOR(controller_status, network_status) → local_status
Level 2: =BITOR(local_status, storage_status) → total_status

For many sources:
=BITOR(BITOR(BITOR(A, B), C), D) for 4 sources
```

### 3. Data Packing - Creating Composite Values

**Scenario**: An embedded systems engineer needs to pack multiple small values into a single integer for efficient storage or transmission, combining several data fields into one packed value.

**Implementation**: Use BITOR with shifted values to combine multiple fields into a single packed integer.

```
Data format to pack:
Bits 0-3: Sensor ID (0-15)
Bits 4-7: Reading type (0-15)
Bits 8-11: Quality flag (0-15)
Bits 12-15: Sequence number (0-15)

| Field | Value | Shifted Value | Formula |
|-------|-------|---------------|---------|
| Sensor ID | 5 | 5 | =5 |
| Reading type | 3 | 48 | =BITLSHIFT(3, 4) |
| Quality | 12 | 3072 | =BITLSHIFT(12, 8) |
| Sequence | 7 | 28672 | =BITLSHIFT(7, 12) |

Packed value:
=BITOR(BITOR(BITOR(5, 48), 3072), 28672) = 31797

Or build incrementally:
Step 1: =BITOR(5, BITLSHIFT(3, 4)) = 53
Step 2: =BITOR(53, BITLSHIFT(12, 8)) = 3125
Step 3: =BITOR(3125, BITLSHIFT(7, 12)) = 31797

Verify: DEC2BIN(31797) shows the packed bits
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Function availability | Excel 2013+ | Yes |
| Maximum value | 2^48 - 1 | 2^48 - 1 |
| Negative numbers | Error | Error |
| Decimal handling | Truncated | Truncated |
| Return type | Integer | Integer |

Both platforms implement BITOR identically. The function was added to Excel in version 2013.

## Tips and Best Practices

1. **Set specific bits**: To set bit n in a value, use `=BITOR(value, 2^n)`. This adds the bit if not present, or leaves it if already set.

2. **Combine multiple flags**: Chain BITOR calls or combine masks first: `=BITOR(value, BITOR(flag1, flag2))` is equivalent to setting both flags at once.

3. **Use for defaults**: OR with a default value to ensure certain bits are always set: `=BITOR(user_input, minimum_required_flags)`.

4. **Idempotent operation**: BITOR(x, x) = x. Setting a bit that's already set has no effect, making OR safe for repeated applications.

5. **Create composite masks**: Build up complex masks incrementally: mask = BITOR(BITOR(field1_mask, field2_mask), field3_mask).

6. **Document bit assignments**: Always document which bits represent what; use named ranges for bit values (READ_BIT = 1, WRITE_BIT = 2, etc.).

7. **Combine with BITLSHIFT**: Pack fields by shifting then OR-ing: `=BITOR(BITOR(lowfield, BITLSHIFT(midfield, 8)), BITLSHIFT(highfield, 16))`.

8. **Verify with DEC2BIN**: Use `=DEC2BIN(result)` to visually verify that the expected bits are set.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BITAND]] | Bitwise AND: result bit is 1 only if both input bits are 1 |
| [[BITXOR]] | Bitwise XOR: result bit is 1 if input bits differ |
| [[BITNOT]] | Bitwise NOT: inverts all bits (Excel has no direct BITNOT) |

### Commonly Used With

- **[[BITAND]]** - Test specific bits after combining with OR
- **[[BITXOR]]** - Toggle bits selectively
- **[[BITLSHIFT]]** - Shift values left before combining with OR
- **[[BITRSHIFT]]** - Shift values right for field extraction
- **[[DEC2BIN]]** - Convert results to binary for verification
- **[[HEX2DEC]]** - Convert hexadecimal values for OR operations
- **[[POWER]]** - Calculate bit position values (2^n)

## Official Documentation

- [Microsoft Excel BITOR Documentation](https://support.microsoft.com/en-us/office/bitor-function-f6ead5c8-5b98-4c9e-9053-8ad5234919b2)
- [Google Sheets BITOR Documentation](https://support.google.com/docs/answer/9061524)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2013 | 2013 | Function introduced |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | Ongoing | Current version |
| Google Sheets | 2013 | Available since introduction |
