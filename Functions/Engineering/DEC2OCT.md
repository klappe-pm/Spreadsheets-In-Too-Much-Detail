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
- octal-conversion
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- decimal-to-octal
- dec-to-oct
tags:
- engineering-function
- base-conversion
- decimal
- octal
- number-systems
---

# DEC2OCT

## Description

DEC2OCT converts a decimal (base-10) number to its octal (base-8) equivalent. Octal uses eight digits (0-7), with each digit representing a power of 8. Historically, octal was popular in computing because early computers used 12-bit, 24-bit, or 36-bit words, all divisible by 3, making octal a natural fit. Today, octal remains important primarily in Unix/Linux systems for file permissions, where the familiar chmod 755 or chmod 644 commands use octal notation.

The conversion from decimal to octal involves repeatedly dividing by 8 and recording remainders. For example, 64 decimal: 64/8 = 8 remainder 0, 8/8 = 1 remainder 0, 1/8 = 0 remainder 1. Reading remainders backward gives 100 octal. DEC2OCT handles a range from -536,870,912 to 536,870,911 (30-bit signed integers), using two's complement representation for negative numbers.

Use DEC2OCT when working with Unix file permissions, interfacing with legacy systems that use octal addressing, educational demonstrations of number systems, or any application requiring octal representation. The optional places parameter specifies output width with leading zeros, useful for consistent formatting in permission strings or documentation.

Both Excel and Google Sheets implement DEC2OCT identically. The function supports a 30-bit signed integer range, returning octal strings up to 10 characters long. Positive numbers can be formatted with specified character counts, while negative numbers produce 10-character octal strings in two's complement format (starting with digits 4-7 to indicate the negative sign bit).

## Syntax

> [!NOTE] Syntax
> ```
> DEC2OCT(number, [places])
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The decimal integer to convert. Must be between -536,870,912 and 536,870,911. | Yes |
| **places** | The number of characters to use in the result. If omitted, uses minimum necessary characters. Maximum is 10. | No |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=DEC2OCT(8)` | 10 | 8 decimal = 1*8 + 0 = 10 octal |
| `=DEC2OCT(64)` | 100 | 64 decimal = 1*64 = 100 octal |
| `=DEC2OCT(7)` | 7 | Maximum single octal digit |
| `=DEC2OCT(511)` | 777 | 511 = 7*64 + 7*8 + 7 = 777 octal |
| `=DEC2OCT(493, 3)` | 755 | Common Unix permission (rwxr-xr-x) |
| `=DEC2OCT(420, 3)` | 644 | Common file permission (rw-r--r--) |
| `=DEC2OCT(-1)` | 7777777777 | Two's complement -1 (all 7s) |
| `=DEC2OCT(-8)` | 7777777770 | Two's complement -8 |
| `=DEC2OCT(0, 3)` | 000 | Zero padded to 3 characters |
| `=DEC2OCT(4096)` | 10000 | 4096 = 8^4 = 10000 octal |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number exceeds maximum (536,870,911) | Use smaller value within range |
| `#NUM!` | Number below minimum (-536,870,912) | Use larger value within range |
| `#NUM!` | Places parameter is negative | Use positive integer for places |
| `#NUM!` | Places is too small for result | Increase places or omit parameter |
| `#VALUE!` | Non-numeric input | Ensure input is a valid number |
| `#NUM!` | Places exceeds 10 | Maximum places value is 10 |

## Use Cases

### 1. Unix File Permission Generation

**Scenario**: A system administrator needs to generate chmod octal codes from a permission matrix showing read (4), write (2), and execute (1) permissions for owner, group, and others.

**Implementation**: Permission values for each category are summed and converted to octal.

```
Permission Calculator:
Owner: read(4) + write(2) + execute(1) = 7
Group: read(4) + execute(1) = 5
Other: read(4) + execute(1) = 5

Combined decimal: 7*64 + 5*8 + 5 = 448 + 40 + 5 = 493
=DEC2OCT(493, 3) → 755

Or calculate directly:
=DEC2OCT(owner*64 + group*8 + other, 3) → 755
chmod 755 = rwxr-xr-x (typical for executable scripts)
```

### 2. Legacy System Interface

**Scenario**: An engineer is documenting device addresses from a legacy PDP-11 system that used octal addressing conventions.

**Implementation**: Modern decimal addresses are converted to octal format matching historical documentation.

```
| Device | Decimal Address | Formula | Octal Address |
|--------|-----------------|---------|---------------|
| Console | 177560 | =DEC2OCT(A2,6) | 532370 |
| Line Printer | 177514 | =DEC2OCT(A3,6) | 532272 |
| Disk Controller | 177400 | =DEC2OCT(A4,6) | 532070 |
| Memory Start | 0 | =DEC2OCT(A5,6) | 000000 |
```

### 3. Educational Number System Conversion Tool

**Scenario**: A computer science professor creates an interactive worksheet showing decimal-to-octal conversion with step-by-step breakdown.

**Implementation**: Students can input decimal numbers and see octal equivalents alongside conversion steps.

```
Input: 125 decimal
Step 1: 125 / 8 = 15 remainder 5
Step 2: 15 / 8 = 1 remainder 7
Step 3: 1 / 8 = 0 remainder 1
Reading remainders backward: 175

Verification: =DEC2OCT(125) → 175
Check: 1*64 + 7*8 + 5*1 = 64 + 56 + 5 = 125 ✓
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Minimum value | -536,870,912 | -536,870,912 |
| Maximum value | 536,870,911 | 536,870,911 |
| Maximum places | 10 | 10 |
| Negative format | 10-char two's complement | 10-char two's complement |
| Decimal input handling | Truncates to integer | Truncates to integer |
| Return type | Text | Text |

Both platforms implement DEC2OCT identically. The function returns text to preserve leading zeros when the places parameter is used. The 30-bit range supports values up to about half a billion, sufficient for most practical applications.

## Tips and Best Practices

1. **Use places=3 for Unix permissions**: Standard chmod codes are 3 digits: `=DEC2OCT(permission_value, 3)` ensures consistent formatting like 755, 644, etc.

2. **Calculate permissions with formula**: Build permission values systematically: `=(read*4+write*2+exec*1)*64 + (read*4+write*2+exec*1)*8 + (read*4+write*2+exec*1)` where read/write/exec are 1 or 0.

3. **Remember octal digit range**: Each octal digit is 0-7 only. If you see 8 or 9 in expected octal output, you're looking at decimal.

4. **Understand the power progression**: Octal powers: 8^0=1, 8^1=8, 8^2=64, 8^3=512, 8^4=4096. Useful for manual verification.

5. **Watch for negative output format**: Negative numbers always produce 10-digit output starting with 4, 5, 6, or 7 (indicating the sign bits are set).

6. **Validate Unix permission input**: Valid chmod values are 0-511 (000-777 octal). Use `=IF(AND(A1>=0,A1<=511),DEC2OCT(A1,3),"Invalid")`.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[OCT2DEC]] | Converts octal to decimal (inverse function) |
| [[DEC2BIN]] | Converts decimal to binary |
| [[DEC2HEX]] | Converts decimal to hexadecimal |
| [[BIN2OCT]] | Converts binary to octal |
| [[HEX2OCT]] | Converts hexadecimal to octal |

### Commonly Used With

- **[[CONCAT]]** - Build formatted octal strings
- **[[IF]]** - Validate input ranges
- **[[OCT2DEC]]** - Reverse conversion for verification
- **[[MOD]]** - Calculate individual permission digits
- **[[INT]]** - Ensure integer input before conversion

## Official Documentation

- [Microsoft Excel DEC2OCT Documentation](https://support.microsoft.com/en-us/office/dec2oct-function-c9d835ca-20b7-40c4-8a9e-2a35571c68d2)
- [Google Sheets DEC2OCT Documentation](https://support.google.com/docs/answer/3093227)

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
