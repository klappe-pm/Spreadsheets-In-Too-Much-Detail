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
- octal-conversion
- binary-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- octal-to-binary
- oct-to-bin
tags:
- engineering-function
- base-conversion
- octal
- binary
- number-systems
---

# OCT2BIN

## Description

OCT2BIN converts an octal (base-8) number to its binary (base-2) equivalent. This function provides a direct path from octal to binary, which is particularly useful because each octal digit maps precisely to exactly three binary digits. This 1-to-3 relationship makes octal a natural compact notation for binary in certain contexts, especially Unix file permissions where each permission category (owner, group, other) is represented by one octal digit corresponding to three permission bits.

The conversion from octal to binary replaces each octal digit with its 3-bit binary equivalent: 0=000, 1=001, 2=010, 3=011, 4=100, 5=101, 6=110, 7=111. For example, octal 755 becomes binary 111101101: 7=111, 5=101, 5=101. OCT2BIN handles a limited range suitable for 10-bit binary output, accepting octal values from 7777777000 (-512 decimal) to 777 (511 decimal) for positive numbers.

Use OCT2BIN when you need to visualize the bit-level representation of Unix file permissions, analyze legacy system data stored in octal, or understand the binary foundation of octal values for educational purposes. The optional places parameter allows you to specify output width with leading zeros for consistent bit formatting.

Both Excel and Google Sheets implement OCT2BIN identically. The function accepts octal strings (digits 0-7 only) and produces binary strings up to 10 characters. The range is limited to values representable in 10 binary digits, using two's complement for negative numbers. Both platforms handle the conversion consistently.

## Syntax

> [!NOTE] Syntax
> ```
> OCT2BIN(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The octal number to convert. Must contain only digits 0-7. Must represent a value between -512 and 511 decimal. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Maximum is 10. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=OCT2BIN("7")` | 111 | Octal 7 = binary 111 (maximum single octal digit) |
| `=OCT2BIN("10")` | 1000 | Octal 10 = 8 decimal = binary 1000 |
| `=OCT2BIN("777")` | 111111111 | Maximum positive value (511 decimal) |
| `=OCT2BIN("7", 3)` | 111 | 7 already needs 3 bits, no padding needed |
| `=OCT2BIN("5", 3)` | 101 | 5 = 101 (exactly 3 bits) |
| `=OCT2BIN("755", 9)` | 111101101 | Unix 755 permission expanded to binary |
| `=OCT2BIN("7777777000")` | 1000000000 | Two's complement -512 |
| `=OCT2BIN("7777777777")` | 1111111111 | Two's complement -1 |
| `=OCT2BIN("0", 8)` | 00000000 | Zero padded to 8 bits |
| `=OCT2BIN("644", 9)` | 110100100 | Unix 644 permission expanded |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input contains invalid digits (8, 9) | Use only digits 0-7 for octal |
| `#NUM!` | Octal value exceeds valid range | Use values representing -512 to 511 decimal |
| `#NUM!` | Places parameter is negative | Use positive integer for places |
| `#NUM!` | Places too small for result | Increase places or omit parameter |
| `#VALUE!` | Non-numeric or empty input | Provide valid octal string |
| `#NUM!` | Result would exceed 10 binary digits | Limit octal input appropriately |

## Use Cases

### 1. Unix Permission Bit Visualization

**Scenario**: A system administrator wants to understand exactly which permission bits are set in a chmod octal code by expanding it to binary format.

**Implementation**: Each octal permission value is converted to binary to visualize the rwx (read/write/execute) bits.

```
chmod 755 breakdown:
Full binary: =OCT2BIN("755", 9) → 111101101

Breaking down by position:
Owner (7): =OCT2BIN("7", 3) → 111 (rwx - all permissions)
Group (5): =OCT2BIN("5", 3) → 101 (r-x - read and execute)
Other (5): =OCT2BIN("5", 3) → 101 (r-x - read and execute)

Bit mapping: r=4(100), w=2(010), x=1(001)
7 = 4+2+1 = rwx = 111
5 = 4+0+1 = r-x = 101
```

### 2. Legacy System Binary Analysis

**Scenario**: An engineer is analyzing data from a legacy system that displays addresses in octal format and needs to understand the underlying binary bit patterns.

**Implementation**: Octal addresses are expanded to binary for bit-level analysis.

```
Legacy system octal address: 377
=OCT2BIN("377", 9) → 011111111

Binary analysis:
Bit 8 (MSB): 0 - Address type: User space
Bits 7-4: 1111 - Page number: 15
Bits 3-0: 1111 - Offset: 15

Another example: 100 octal
=OCT2BIN("100", 9) → 001000000
Bit 6 set = 64 decimal = special flag enabled
```

### 3. Educational Binary-Octal Relationship Demonstration

**Scenario**: A computer science instructor creates materials demonstrating the systematic 3-bit grouping relationship between binary and octal.

**Implementation**: A reference table shows how each octal digit maps to exactly 3 binary digits.

```
| Octal | Formula | Binary | Explanation |
|-------|---------|--------|-------------|
| 0 | =OCT2BIN("0",3) | 000 | No bits set |
| 1 | =OCT2BIN("1",3) | 001 | LSB set |
| 2 | =OCT2BIN("2",3) | 010 | Middle bit |
| 3 | =OCT2BIN("3",3) | 011 | Low 2 bits |
| 4 | =OCT2BIN("4",3) | 100 | MSB of group |
| 5 | =OCT2BIN("5",3) | 101 | MSB + LSB |
| 6 | =OCT2BIN("6",3) | 110 | High 2 bits |
| 7 | =OCT2BIN("7",3) | 111 | All 3 bits |

Pattern: Each octal digit = one complete binary triplet
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum positive input | 777 (511 decimal) | 777 (511 decimal) |
| Negative input format | 10-char two's complement octal | 10-char two's complement octal |
| Maximum output length | 10 characters | 10 characters |
| Maximum places | 10 | 10 |
| Valid input digits | 0-7 only | 0-7 only |
| Return type | Text | Text |

Both platforms implement OCT2BIN identically. The function returns text to preserve leading zeros when the places parameter is used. The 10-bit output range limits positive octal values to 777 (511 decimal).

## Tips and Best Practices

1. **Use places=3 per octal digit**: For permission analysis, use multiples of 3 for places: `=OCT2BIN("755", 9)` ensures each octal digit maps to exactly 3 binary digits.

2. **Memorize the 8 mappings**: Learn 0=000 through 7=111. This makes visual verification instant and helps catch errors.

3. **Remember valid digits are 0-7 only**: Octal never contains 8 or 9. If your input has these digits, it's not valid octal.

4. **Understand the range limitation**: OCT2BIN only handles values up to 777 octal (511 decimal) for positive numbers. For larger values, convert to decimal first.

5. **Use for permission debugging**: When troubleshooting file access issues, expand permissions to binary to see exactly which bits are set: `=OCT2BIN(permission, 9)`.

6. **Verify with reverse conversion**: Use BIN2OCT to verify: `=BIN2OCT(OCT2BIN(A1))` should return the original octal value.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BIN2OCT]] | Converts binary to octal (inverse function) |
| [[OCT2DEC]] | Converts octal to decimal |
| [[OCT2HEX]] | Converts octal to hexadecimal |
| [[DEC2BIN]] | Converts decimal to binary |
| [[HEX2BIN]] | Converts hexadecimal to binary |

### Commonly Used With

- **[[MID]]** - Extract individual octal digits for separate conversion
- **[[BIN2DEC]]** - Convert binary result to decimal for calculations
- **[[CONCAT]]** - Join binary results from multiple octal digits
- **[[LEN]]** - Verify binary output length
- **[[REPT]]** - Pad binary strings manually if needed

## Official Documentation

- [Microsoft Excel OCT2BIN Documentation](https://support.microsoft.com/en-us/office/oct2bin-function-55383471-3c56-4d27-9522-1a8ec646c589)
- [Google Sheets OCT2BIN Documentation](https://support.google.com/docs/answer/3093231)

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
