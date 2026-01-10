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
- octal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- binary-to-octal
- bin-to-oct
tags:
- engineering-function
- base-conversion
- binary
- octal
- number-systems
---

# BIN2OCT

## Description

BIN2OCT converts a binary (base-2) number to its octal (base-8) equivalent. Octal is a numeral system that uses eight digits (0-7) and historically was important in computing because early systems used word sizes divisible by 3 bits. Each octal digit represents exactly three binary digits, making conversion straightforward. While hexadecimal has largely replaced octal in modern computing, octal remains relevant in Unix/Linux file permissions, some programming languages (C, Python), and legacy systems.

The conversion from binary to octal groups binary digits into sets of three (from right to left), converting each group to its octal equivalent (0-7). For example, binary 101110 becomes 56 in octal: 101 = 5 and 110 = 6. BIN2OCT handles negative numbers using two's complement representation, where a 10-digit binary number starting with 1 represents a negative value and produces a 10-character octal result with leading 7s.

Use BIN2OCT when working with Unix file permissions (where rwx maps to 3 bits = 1 octal digit), legacy system documentation, educational contexts teaching number systems, or when interfacing with systems that expect octal input. The optional places parameter allows you to specify exact output length for consistent formatting in documentation or fixed-width data fields.

Both Excel and Google Sheets implement BIN2OCT identically. The function accepts binary numbers up to 10 characters long and outputs octal values with optional padding. Positive values can range from 0 to 777 octal (0 to 511 decimal), and negative values in two's complement produce 10-digit octal results. Both platforms handle the conversion consistently across all valid input ranges.

## Syntax

> [!NOTE] Syntax
> ```
> BIN2OCT(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The binary number to convert. Can be up to 10 characters (bits) long. Characters must be 0 or 1 only. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Pads with leading zeros if needed. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=BIN2OCT("111")` | 7 | Binary 111 = decimal 7 = octal 7 (maximum single octal digit) |
| `=BIN2OCT("1000")` | 10 | Binary 1000 = decimal 8 = octal 10 |
| `=BIN2OCT("111111111")` | 777 | Binary for 511 decimal = maximum 3-digit octal |
| `=BIN2OCT("111", 3)` | 007 | Padded to 3 characters with leading zeros |
| `=BIN2OCT("0111111111")` | 777 | Maximum positive 10-bit value (511 decimal) |
| `=BIN2OCT("1000000000")` | 7777777000 | Two's complement negative (-512 decimal) |
| `=BIN2OCT("1111111111")` | 7777777777 | Two's complement -1 in octal |
| `=BIN2OCT("110100101")` | 645 | 110=6, 100=4, 101=5 (421 decimal) |
| `=BIN2OCT("101101", 4)` | 0055 | Padded: 101=5, 101=5 (45 decimal) |
| `=BIN2OCT("0")` | 0 | Zero in any base is zero |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Input contains characters other than 0 or 1 | Ensure input contains only binary digits (0 and 1) |
| `#NUM!` | Input exceeds 10 characters | Limit binary input to 10 bits maximum |
| `#NUM!` | Places parameter is negative | Use a positive integer for places |
| `#NUM!` | Places is too small for the result | Increase places value or omit parameter |
| `#VALUE!` | Non-numeric or invalid text input | Verify input is a valid binary number string |

## Use Cases

### 1. Unix File Permission Conversion

**Scenario**: A system administrator needs to convert binary permission masks to octal notation for chmod commands. In Unix, permissions are represented as three bits (read=4, write=2, execute=1) for owner, group, and others.

**Implementation**: Each 3-bit permission set is converted to its octal equivalent for the chmod command.

```
Owner permissions (rwx):  111 → =BIN2OCT("111") → 7
Group permissions (r-x):  101 → =BIN2OCT("101") → 5
Other permissions (r--):  100 → =BIN2OCT("100") → 4
Full permission string: =BIN2OCT("111")&BIN2OCT("101")&BIN2OCT("100") → 754
chmod command: chmod 754 filename
```

### 2. Educational Number System Demonstration

**Scenario**: A computer science instructor is creating teaching materials showing the relationship between binary and octal number systems, demonstrating how each octal digit maps to exactly three binary digits.

**Implementation**: A conversion table shows the systematic grouping of binary digits into octal.

```
| Binary | Grouping | Formula | Octal | Decimal |
|--------|----------|---------|-------|---------|
| 1 | 001 | =BIN2OCT("1") | 1 | 1 |
| 10 | 010 | =BIN2OCT("10") | 2 | 2 |
| 111 | 111 | =BIN2OCT("111") | 7 | 7 |
| 1000 | 001 000 | =BIN2OCT("1000") | 10 | 8 |
| 111111 | 111 111 | =BIN2OCT("111111") | 77 | 63 |
| 1000000 | 001 000 000 | =BIN2OCT("1000000") | 100 | 64 |
```

### 3. Legacy Mainframe Data Conversion

**Scenario**: A data migration specialist is converting binary data from a legacy system that used octal representation for address and device codes.

**Implementation**: Binary device addresses are converted to octal for documentation and verification against original system manuals.

```
Device Channel: =BIN2OCT("110", 1) → 6
Controller ID:  =BIN2OCT("010101", 2) → 25
Unit Address:   =BIN2OCT("111000", 2) → 70
Full Address:   =BIN2OCT("110")&BIN2OCT("010101",2)&BIN2OCT("111000",2) → 62570
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Maximum input length | 10 characters | 10 characters |
| Maximum places value | 10 | 10 |
| Maximum positive output | 777 (511 decimal) | 777 (511 decimal) |
| Negative number handling | Two's complement | Two's complement |
| Leading zeros with places | Supported | Supported |
| Return type | Text | Text |

Both platforms implement BIN2OCT identically. The function returns a text string to preserve leading zeros when the places parameter is used. This is consistent behavior across both Excel and Google Sheets.

## Tips and Best Practices

1. **Group binary in threes for mental conversion**: When checking results, group binary digits from right to left in sets of 3: 10110111 → 010 110 111 → 2 6 7 = 267 octal.

2. **Use places for Unix permissions**: When generating chmod-style permission codes, use consistent formatting: `=BIN2OCT(A1,1)` ensures single-digit output for each permission set.

3. **Remember the 0-7 range**: Each octal digit can only be 0-7. If you see 8 or 9 in your result, something went wrong (this shouldn't happen with valid binary input).

4. **Watch for two's complement with 10-bit input**: Binary numbers with 10 digits starting with 1 produce 10-digit octal results representing negative values. Use 9 or fewer bits for positive-only conversion.

5. **Combine three BIN2OCT calls for full permission strings**: For Unix permissions, convert owner, group, and other separately: `=BIN2OCT(owner)&BIN2OCT(group)&BIN2OCT(other)`.

6. **Validate with reverse conversion**: Use OCT2BIN to verify your conversion: `=OCT2BIN(BIN2OCT(A1))` should return the original value.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[BIN2DEC]] | Converts binary to decimal |
| [[BIN2HEX]] | Converts binary to hexadecimal |
| [[OCT2BIN]] | Converts octal to binary (inverse function) |
| [[DEC2OCT]] | Converts decimal to octal |
| [[HEX2OCT]] | Converts hexadecimal to octal |

### Commonly Used With

- **[[CONCAT]]** / **[[CONCATENATE]]** - Combine multiple octal digits into permission strings
- **[[OCT2DEC]]** - Convert octal result to decimal for calculations
- **[[MID]]** - Extract individual octal digits from results
- **[[TEXT]]** - Format results with leading zeros

## Official Documentation

- [Microsoft Excel BIN2OCT Documentation](https://support.microsoft.com/en-us/office/bin2oct-function-0a4e01ba-ac8d-4158-9b29-16c25c4c23fd)
- [Google Sheets BIN2OCT Documentation](https://support.google.com/docs/answer/3093224)

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
