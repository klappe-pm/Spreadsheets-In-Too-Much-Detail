---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- number-conversion
- base-conversion
- number-systems
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Convert to Base
- Number Base Conversion
- Radix Conversion
tags:
- conversion
- binary
- hexadecimal
- octal
- number-systems
---

# BASE

## Description

**BASE** converts a decimal (base 10) number into a text representation of that number in another base (radix). Want to convert 255 to hexadecimal? BASE(255, 16) gives you "FF". Need binary? BASE(42, 2) returns "101010". It's the universal converter for any base from 2 to 36.

The function fills a gap that specific functions like DEC2BIN, DEC2HEX, and DEC2OCT can't—it handles ANY base from 2 to 36. While those specialized functions only work for binary (2), octal (8), and hexadecimal (16), BASE lets you convert to base 5, base 12, base 36, or any other radix you need. For bases above 10, letters A-Z represent digits 10-35.

**The padding parameter:** BASE includes an optional minimum length parameter that pads the result with leading zeros. BASE(15, 16) returns "F", but BASE(15, 16, 4) returns "000F". This is incredibly useful for consistent formatting in reports, for generating fixed-width codes, or for compatibility with systems expecting specific number lengths.

**Text output, not number:** BASE returns text, not a number. This is necessary because results like "FF" or "101010" can't be represented as numeric values in a spreadsheet. If you need to perform math on the result, you'll need to convert it back using DECIMAL or the specific conversion functions.

## Syntax

> [!f(x)] BASE Syntax
>
> ```
> =BASE(number, radix, [min_length])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `number` | Yes | The decimal integer to convert. Must be >= 0. Decimal portions are truncated. |
| `radix` | Yes | The target base (radix) for conversion. Must be an integer between 2 and 36 inclusive. |
| `min_length` | No | The minimum length of the returned string. If result is shorter, it's padded with leading zeros. Must be >= 0. |

### Return Value

Returns a text string representing the number in the specified base. Returns #NUM! if number is negative, radix is outside 2-36, or min_length is negative. Returns #VALUE! if parameters are non-numeric.

## Examples

> [!f(x)] BASE Examples

### Example 1: Binary Conversion
```
=BASE(10, 2)
```
**Result:** "1010"

**Explanation:** Converts decimal 10 to binary (base 2). 10 = 1*8 + 0*4 + 1*2 + 0*1 = 1010.

---

### Example 2: Hexadecimal Conversion
```
=BASE(255, 16)
```
**Result:** "FF"

**Explanation:** Converts decimal 255 to hexadecimal (base 16). 255 = 15*16 + 15 = F*16 + F = FF.

---

### Example 3: Octal Conversion
```
=BASE(64, 8)
```
**Result:** "100"

**Explanation:** Converts decimal 64 to octal (base 8). 64 = 1*64 + 0*8 + 0*1 = 100 in base 8.

---

### Example 4: With Minimum Length Padding
```
=BASE(15, 16, 4)
```
**Result:** "000F"

**Explanation:** Converts 15 to hex ("F") then pads to 4 characters with leading zeros. Useful for consistent formatting.

---

### Example 5: Binary with Padding
```
=BASE(5, 2, 8)
```
**Result:** "00000101"

**Explanation:** 5 in binary is "101", padded to 8 bits gives "00000101". Perfect for byte representation.

---

### Example 6: Base 36 (Maximum)
```
=BASE(35, 36)
```
**Result:** "Z"

**Explanation:** In base 36, digits go 0-9 then A-Z. 35 (the highest single digit) is represented as "Z".

---

### Example 7: Large Number in Base 36
```
=BASE(1000000, 36)
```
**Result:** "LFLS"

**Explanation:** Base 36 is very compact—a million in just 4 characters. Used in URL shorteners and compact codes.

---

### Example 8: Cell Reference
```
=BASE(A1, B1, C1)
```
Where A1=100, B1=2, C1=12

**Result:** "000001100100"

**Explanation:** Converts 100 to binary with 12-character minimum length. All parameters can be cell references.

---

### Example 9: Zero Value
```
=BASE(0, 16)
```
**Result:** "0"

**Explanation:** Zero in any base is zero. Returns "0" as text.

---

### Example 10: Zero with Padding
```
=BASE(0, 16, 4)
```
**Result:** "0000"

**Explanation:** Zero padded to 4 characters gives "0000". Useful for maintaining fixed-width formatting.

---

### Example 11: Unusual Base (Base 5)
```
=BASE(100, 5)
```
**Result:** "400"

**Explanation:** 100 in base 5 is 4*25 + 0*5 + 0*1 = 400. BASE handles any radix from 2-36.

---

### Example 12: Base 12 (Duodecimal)
```
=BASE(144, 12)
```
**Result:** "100"

**Explanation:** 144 = 12^2, so in base 12 it's written as "100". Duodecimal has interesting mathematical properties.

---

### Example 13: Verification with DECIMAL
```
=DECIMAL(BASE(255, 16), 16)
```
**Result:** 255

**Explanation:** Converting to base 16 and back to decimal returns the original number. Useful for verification.

---

### Example 14: Error Case - Negative Number
```
=BASE(-10, 2)
```
**Result:** #NUM!

**Explanation:** BASE only works with non-negative integers. Use workarounds for negative number representation.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Number is negative | BASE only handles non-negative integers. For negative numbers, convert absolute value and handle sign separately. |
| `#NUM!` | Radix is less than 2 or greater than 36 | Verify radix is an integer between 2 and 36 inclusive. |
| `#NUM!` | min_length is negative | If using padding, ensure minimum length is zero or positive. |
| `#VALUE!` | Non-numeric input | Ensure all parameters are numbers or references to numeric cells. |
| `Truncated result` | Decimal input truncated | BASE truncates decimal portions. 10.9 is treated as 10. Use ROUND/INT if needed. |

## Use Cases

### [[Color Code Conversion]]

**Scenario:** Convert RGB color values (0-255) to hexadecimal for web development or design.

**Implementation:**
```
Red Hex: =BASE(R, 16, 2)
Green Hex: =BASE(G, 16, 2)
Blue Hex: =BASE(B, 16, 2)
Full Color: ="#" & BASE(R, 16, 2) & BASE(G, 16, 2) & BASE(B, 16, 2)
```

**Business Application:** Web design, graphic design, UI/UX development. Converting RGB values to hex color codes for CSS, HTML, or design software.

**Technical Details:** The 2-character minimum ensures proper formatting (e.g., "0F" instead of "F" for value 15). Critical for valid color codes.

---

### [[Binary Data Representation]]

**Scenario:** Display binary representations of data for debugging, education, or documentation.

**Implementation:**
```
8-bit: =BASE(value, 2, 8)
16-bit: =BASE(value, 2, 16)
32-bit: =BASE(value, 2, 32)
```

**Business Application:** Network engineering, embedded systems documentation, computer science education, debugging bitwise operations.

**Technical Details:** Fixed-width binary display makes bit patterns visible and comparable. Essential for understanding flag fields and binary protocols.

---

### [[Short Code Generation]]

**Scenario:** Generate compact, unique codes from sequential IDs using base 36.

**Implementation:**
```
=BASE(sequential_id + 100000, 36)
```

**Business Application:** URL shorteners, order numbers, tracking codes, coupon codes. Base 36 provides very compact representation.

**Technical Details:** Adding an offset ensures minimum length without leading zeros. Base 36 uses all alphanumeric characters (0-9, A-Z) for maximum density.

---

### [[Permission and Flag Encoding]]

**Scenario:** Display and document permission bits or flags in a readable format.

**Implementation:**
```
Permission Binary: =BASE(permission_value, 2, 8)
Permission Octal: =BASE(permission_value, 8, 3)
```

**Business Application:** Unix file permissions, access control systems, feature flags, hardware register documentation.

**Technical Details:** Octal is traditional for Unix permissions (chmod 755). Binary shows individual permission bits clearly.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Maximum number:** Limited by Excel's numeric precision (about 10^15)
- **Case:** Returns uppercase letters (A-Z) for digits above 9
- **Truncation:** Decimal values are truncated to integers

### Google Sheets
- **Availability:** All versions
- **Maximum number:** Same practical limits as Excel
- **Case:** Returns uppercase letters (same as Excel)
- **Truncation:** Decimal values are truncated to integers

### Both Platforms
- Identical function name and syntax
- Same radix range (2-36)
- Same error conditions
- Same uppercase letter output
- Text string return type

### Comparison with Specific Functions
```
DEC2BIN(10) = "1010"       BASE(10, 2) = "1010"
DEC2HEX(255) = "FF"        BASE(255, 16) = "FF"
DEC2OCT(64) = "100"        BASE(64, 8) = "100"
```
BASE is more flexible; specific functions may have different limits and padding options.

## Tips and Best Practices

1. **Use padding for consistency:** When generating codes or displaying binary, always specify min_length for consistent formatting.

2. **Remember it returns text:** BASE returns a text string, not a number. You can't do arithmetic directly on the result. Use DECIMAL to convert back.

3. **Base 36 for compact codes:** For the shortest possible alphanumeric representation, use base 36. Great for URLs and user-facing codes.

4. **Combine with DECIMAL for round-trip:** Verify conversions with `=DECIMAL(BASE(x, r), r)` which should return x.

5. **Handle negatives manually:** BASE doesn't support negative numbers. Convert absolute value and prepend "-" if needed, or use two's complement for binary.

6. **Know common bases:** 2=binary, 8=octal, 10=decimal, 16=hexadecimal, 36=alphanumeric. These cover most practical needs.

7. **Truncation behavior:** Decimal inputs are truncated, not rounded. BASE(10.9, 2) = BASE(10, 2) = "1010".

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[DECIMAL]] | Converts text in any base to decimal | For the reverse conversion |
| [[DEC2BIN]] | Converts decimal to binary | When you specifically need binary with its limits |
| [[DEC2HEX]] | Converts decimal to hexadecimal | When you specifically need hex with its limits |
| [[DEC2OCT]] | Converts decimal to octal | When you specifically need octal with its limits |

### Commonly Used Together

**[[DECIMAL]]** - Reverse conversion

*Round-trip verification:*
```
=DECIMAL(BASE(100, 16), 16)  // Returns 100
```
Convert from any base back to decimal.

---

**[[CONCATENATE]] / [[&]]** - String building

*Build color codes:*
```
="#" & BASE(R, 16, 2) & BASE(G, 16, 2) & BASE(B, 16, 2)
```
Combine multiple BASE outputs into formatted strings.

---

**[[LEN]]** - Check result length

*Validate output length:*
```
=LEN(BASE(A1, 2))  // How many bits needed?
```
Determine the natural length before padding decisions.

---

**[[UPPER]] / [[LOWER]]** - Case conversion

*Change letter case:*
```
=LOWER(BASE(255, 16))  // Returns "ff"
```
BASE returns uppercase; use LOWER if lowercase is needed.

---

**[[INT]]** - Ensure integer input

*Pre-process decimals:*
```
=BASE(INT(A1), 16, 4)
```
Explicitly truncate to make behavior clear.

## Official Documentation

- **Microsoft Excel:** [BASE function](https://support.microsoft.com/en-us/office/base-function-2ef61411-aee9-4f29-a811-1c42456c6342)
- **Google Sheets:** [BASE function](https://support.google.com/docs/answer/9061467)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Initial implementation |
| Excel 2016+ | Enhanced | Performance improvements |
| Google Sheets | 2019 | Added to match Excel functionality |

---

*Last updated: 2026-01-10*
