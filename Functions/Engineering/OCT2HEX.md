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
- hexadecimal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- octal-to-hexadecimal
- oct-to-hex
tags:
- engineering-function
- base-conversion
- octal
- hexadecimal
- number-systems
---

# OCT2HEX

## Description

OCT2HEX converts an octal (base-8) number to its hexadecimal (base-16) equivalent. This function provides a direct conversion path between two number systems used in computing, internally translating through binary representation. While both systems are used to compactly represent binary data, hexadecimal has become the dominant notation in modern computing, making OCT2HEX useful for modernizing legacy octal data.

The conversion from octal to hexadecimal works through binary as an intermediate: each octal digit converts to 3 binary bits, then the binary is regrouped into 4-bit nibbles for hexadecimal. For example, octal 755 = binary 111101101 = hex 1ED (regrouping: 0001 1110 1101). OCT2HEX handles a 30-bit range, accepting octal values that fit within the signed integer range of approximately -536 million to +536 million.

Use OCT2HEX when converting Unix permission values to hexadecimal for programming APIs, modernizing legacy system documentation, or when you need hexadecimal representation of data originally stored in octal format. The optional places parameter lets you specify output width with leading zeros for consistent formatting.

Both Excel and Google Sheets implement OCT2HEX identically. The function accepts octal strings (digits 0-7 only) up to 10 characters and returns uppercase hexadecimal strings up to 10 characters. Two's complement representation is used for negative numbers, producing hex strings starting with 8-F for negative values.

## Syntax

> [!NOTE] Syntax
> ```
> OCT2HEX(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The octal number to convert. Must contain only digits 0-7. Can be up to 10 characters. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Maximum is 10. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=OCT2HEX("7")` | 7 | Octal 7 = hex 7 (same for 0-7) |
| `=OCT2HEX("10")` | 8 | Octal 10 = 8 decimal = hex 8 |
| `=OCT2HEX("17")` | F | Octal 17 = 15 decimal = hex F |
| `=OCT2HEX("20")` | 10 | Octal 20 = 16 decimal = hex 10 |
| `=OCT2HEX("755")` | 1ED | Unix permission to hex |
| `=OCT2HEX("644")` | 1A4 | File permission to hex |
| `=OCT2HEX("777")` | 1FF | Maximum 3-digit octal |
| `=OCT2HEX("755", 4)` | 01ED | Padded to 4 characters |
| `=OCT2HEX("7777777777")` | FFFFFFFFFF | Two's complement -1 |
| `=OCT2HEX("3777777777")` | 1FFFFFFF | Maximum positive value |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input contains invalid digits (8, 9) | Use only digits 0-7 for octal |
| `#NUM!` | Octal value outside valid range | Use values within 30-bit signed range |
| `#NUM!` | Places parameter is negative | Use positive integer for places |
| `#NUM!` | Places too small for result | Increase places or omit parameter |
| `#VALUE!` | Non-numeric or empty input | Provide valid octal string |
| `#NUM!` | Input exceeds 10 characters | Limit octal string to 10 characters |

## Use Cases

### 1. Unix Permission to Hex API Conversion

**Scenario**: A developer is building a file management API that internally uses hexadecimal permission masks but receives input in traditional Unix octal format.

**Implementation**: Octal chmod values are converted to hexadecimal for internal processing.

```
Permission conversion table:
| Description | Octal | Formula | Hex | Binary |
|-------------|-------|---------|-----|--------|
| rwxr-xr-x | 755 | =OCT2HEX("755") | 1ED | 111101101 |
| rw-r--r-- | 644 | =OCT2HEX("644") | 1A4 | 110100100 |
| rwx------ | 700 | =OCT2HEX("700") | 1C0 | 111000000 |
| rwxrwxrwx | 777 | =OCT2HEX("777") | 1FF | 111111111 |

API usage: setPermissions(fileId, 0x1ED) // 755 in hex
```

### 2. Legacy System Documentation Modernization

**Scenario**: An organization is updating technical documentation from a legacy system that used octal notation to modern hexadecimal format standard in contemporary documentation.

**Implementation**: All octal values in legacy documents are systematically converted to hex.

```
Legacy → Modern documentation:
| Component | Legacy (Octal) | Formula | Modern (Hex) |
|-----------|----------------|---------|--------------|
| Interrupt vector | 100 | =OCT2HEX("100",4) | 0040 |
| Device register | 177570 | =OCT2HEX("177570") | FFFF8 |
| Status word | 340 | =OCT2HEX("340",4) | 00E0 |
| Control mask | 377 | =OCT2HEX("377",4) | 00FF |

Documentation format: "Register at 0x" & OCT2HEX(octal_addr)
```

### 3. Cross-Platform Number System Reference

**Scenario**: A computer science instructor creates a comprehensive reference showing equivalent values across octal and hexadecimal systems.

**Implementation**: Key boundary values are shown in both number systems.

```
| Value | Octal | Hex | Decimal | Notes |
|-------|-------|-----|---------|-------|
| Nibble max | 17 | =OCT2HEX("17") → F | 15 | 4 bits all 1s |
| Byte max | 377 | =OCT2HEX("377") → FF | 255 | 8 bits all 1s |
| Unix full | 777 | =OCT2HEX("777") → 1FF | 511 | rwxrwxrwx |
| Power of 8 | 1000 | =OCT2HEX("1000") → 200 | 512 | 8^3 |
| Hex 100 | 400 | =OCT2HEX("400") → 100 | 256 | 2^8 |

Pattern: Octal and hex share only digits 0-7, but represent different values
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum input length | 10 characters | 10 characters |
| Minimum octal value | 4000000000 (-536,870,912) | 4000000000 (-536,870,912) |
| Maximum octal value | 3777777777 (536,870,911) | 3777777777 (536,870,911) |
| Maximum places | 10 | 10 |
| Letter case in output | Uppercase (A-F) | Uppercase (A-F) |
| Return type | Text | Text |

Both platforms implement OCT2HEX identically. The function returns text (not numbers) because hex values contain letters. The 30-bit range matches other octal conversion functions.

## Tips and Best Practices

1. **Remember octal 8 doesn't exist**: The digits 8 and 9 are not valid in octal. Common mistake: thinking "18" octal is valid (it's not).

2. **Use consistent padding for documentation**: For permission codes, use `=OCT2HEX(A1, 4)` to get consistent 4-character hex values like 01ED.

3. **Verify with decimal intermediate**: For critical conversions, verify: `=HEX2DEC(OCT2HEX(A1))` should equal `=OCT2DEC(A1)`.

4. **Memorize key boundaries**: Octal 7 = hex 7, octal 10 = hex 8, octal 17 = hex F, octal 20 = hex 10. These help verify conversions.

5. **Output is uppercase**: Results use A-F. Use LOWER() if you need lowercase: `=LOWER(OCT2HEX("755"))` → "1ed".

6. **Handle negative numbers carefully**: 10-digit octal starting with 4-7 represents negative values in two's complement.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[HEX2OCT]] | Converts hexadecimal to octal (inverse function) |
| [[OCT2DEC]] | Converts octal to decimal |
| [[OCT2BIN]] | Converts octal to binary |
| [[DEC2HEX]] | Converts decimal to hexadecimal |
| [[BIN2HEX]] | Converts binary to hexadecimal |

### Commonly Used With

- **[[LOWER]]** - Convert output to lowercase hex if needed
- **[[HEX2DEC]]** - Convert result to decimal for calculations
- **[[CONCAT]]** - Build formatted hex strings with prefixes
- **[[IF]]** - Validate octal input before conversion
- **[[OCT2DEC]]** - Alternative path through decimal for verification

## Official Documentation

- [Microsoft Excel OCT2HEX Documentation](https://support.microsoft.com/en-us/office/oct2hex-function-912175b4-d497-41b4-a029-221f051b858f)
- [Google Sheets OCT2HEX Documentation](https://support.google.com/docs/answer/3093233)

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
